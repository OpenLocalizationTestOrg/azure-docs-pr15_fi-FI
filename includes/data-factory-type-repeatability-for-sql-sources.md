## <a name="repeatability-during-copy"></a>Toistuvuuden kopioinnin aikana

Kun kopioitaessa Azure SQL-tai SQL Server-tietoja muista tallentaa yhden on kannattaa ottaa huomioon välttää tahattomia tulosten toistettavissa. 

Kun kopioit tietoja Azure SQL-tai SQL Server-tietokantaan, kopioi tehtävän on oletusarvoisesti tietojoukon käsittelytoiminto taulukkoon LIITTÄMISKYSELYN oletusarvoisesti. Kun kopioit tietoja tietolähteestä CSV (Luetteloerottimella erotetut arvot) tiedoston sisältävä Azure SQL-tai SQL Server-tietokantaan kaksi tietuetta, tämä on esimerkiksi taulukko näyttää tältä:
    
    ID  Product     Quantity    ModifiedDate
    ... ...         ...         ...
    6   Flat Washer 3           2015-05-01 00:00:00
    7   Down Tube   2           2015-05-01 00:00:00

Oletetaan, että löytyvät virheet lähdetiedosto ja päivitetty alaspäin putki määrä 2-4 lähdetiedostossa. Jos suoritat uudelleen tietojen sektoria kyseisen aikajakson, löydät kahden uuden tietueen, joka on lisätty Azure SQL-tai SQL Server-tietokantaan. Alla oletetaan, taulukon sarakkeiden ole ensisijaisen avaimen rajoitus.
    
    ID  Product     Quantity    ModifiedDate
    ... ...         ...         ...
    6   Flat Washer 3           2015-05-01 00:00:00
    7   Down Tube   2           2015-05-01 00:00:00
    6   Flat Washer 3           2015-05-01 00:00:00
    7   Down Tube   4           2015-05-01 00:00:00

Voit välttää tämän, sinun on määrittäminen UPSERT semantiikkaan liittyvien hyödyntäminen jokin alle 2 järjestelmiä esitetyn alapuolella.

> [AZURE.NOTE] Sektoria voidaan käyttää automaattisesti Azure Data Factory-yritä käytännön määritetyn mukaisesti.

### <a name="mechanism-1"></a>Järjestelmä 1

Suoritettava puhdistustoiminto ensin sektoria suoritettaessa **sqlWriterCleanupScript** -ominaisuuden avulla voidaan hyödyntää. 

    "sink":  
    { 
      "type": "SqlSink", 
      "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM table WHERE ModifiedDate >= \\'{0:yyyy-MM-dd HH:mm}\\' AND ModifiedDate < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
    }

Uudelleenjärjestäminen komentosarja suoritetaan ensimmäinen annetun sektoria, jonka poistaa tiedot SQL-taulukosta, sitä vastaava kopioinnin aikana. Tehtävän myöhemmin lisää tiedot SQL-taulukkoon. 

Jos sektori on nyt käyttää ja valitse voit etsii määrä päivittyvät haluttu.
    
    ID  Product     Quantity    ModifiedDate
    ... ...         ...         ...
    6   Flat Washer 3           2015-05-01 00:00:00
    7   Down Tube   4           2015-05-01 00:00:00

Oletetaan, että tasainen pesin tietue poistetaan alkuperäinen csv. Suorittamalla sektoria uudelleen tuottaa seuraavat tulokset: 
    
    ID  Product     Quantity    ModifiedDate
    ... ...         ...         ...
    7   Down Tube   4           2015-05-01 00:00:00

Mitään uusi oli tehtävä. Kopioi tehtävän suoritettiin uudelleenjärjestäminen-komentosarjan, joka poistaa sitä vastaavan tiedot. Valitse lukea csv (joka sitten sisältyvät vain 1 tietueen) syöte, ja se lisätään taulukkoon. 

### <a name="mechanism-2"></a>Järjestelmä 2
> [AZURE.IMPORTANT] sliceIdentifierColumnName ei tueta Azure SQL-tietovarasto tällä hetkellä. 

Toisen järjestelmä saavuttaa toistettavissa on erillinen sarake (**sliceIdentifierColumnName**) tallentamisessa kohde taulukossa. Tämän sarakkeen käytettävän Azure Data Factory varmistamiseksi lähde- ja pysyvät synkronoituina. Tämän menetelmän silloin, kun on joustavuutta kohde SQL Taulukkorakenteen määrittäminen tai muuttaminen. 

Tämän sarakkeen käytettävän Azure Data Factory toistettavissa tarkoituksiin ja prosessin Azure Data Factory ei tekee rakenteeseen tehdyt muutokset taulukkoon. Tapa käyttää tätä tapaa:

1.  Määritä kohde SQL-taulukon sarakkeen tyyppi binaariluvun (32). Pitäisi olla mitään rajoituksia sarake. Oletetaan, että nimeä sarake kuin "ColumnForADFuseOnly" tässä esimerkissä.
2.  Käytä Kopioi-toiminnossa seuraavasti:

        "sink":  
        { 
          "type": "SqlSink", 
          "sliceIdentifierColumnName": "ColumnForADFuseOnly"
        }

Azure Data Factory täyttää tämän sarakkeen mukaan varmistaa lähde- ja pysyvät synkronoituina tarvitse. Tämän sarakkeen arvojen ei voi käyttää tässä yhteydessä ulkopuolella käyttäjän. 

Samalla järjestelmä 1, kopioi tehtävä näkyy automaattisesti ensin Puhdista annetun sektoria kohteesta SQL-taulukon tiedot ja suorita sitten Kopioi tehtävän tavallisesti, jos haluat lisätä lähteen tiedot sitä kohteeseen. 

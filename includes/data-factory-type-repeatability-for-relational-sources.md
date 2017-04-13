## <a name="repeatability-during-copy"></a>Toistuvuuden kopioinnin aikana

Kun kopioit tietoja, mistä ja mihin relaatio stores, sinun täytyy ottaa huomioon, jotta odottamattomia tuloksia, toistettavissa. 

Sektoria voit olla uudelleen automaattisesti Azure Data Factory-määritetty uudelleen käytännön mukaan. On suositeltavaa määrittäminen uudelleen käytännön lyhytkestoisia virheiden ehkäisemiseksi. Näin ollen toistettavissa on olennainen osa projektinhallintaa huolehtia tietojen siirto aikana. 

**Lähteenä:**

> [AZURE.NOTE] Seuraavat esimerkit ovat Azure SQL, mutta eivät koske mitään tietosäilö, joka tukee suorakulmainen tietojoukkoja. Joudut ehkä muuttamaan lähde- ja **kysely** -ominaisuus **tyyppi** (esimerkiksi: kyselyn sqlReaderQuery sijaan) tietojen tallentamiseen.   

Yleensä, kun luettaessa relaatio stores haluat lukea vain sitä vastaavat tiedot. Voi tehdä olisi käytettävissä WindowStart ja WindowEnd muuttujat avulla Azure Data Factory. Lue lisää muuttujat ja funktioiden [ajoitus ja suorittamisen](../articles/data-factory/data-factory-scheduling-and-execution.md) on artikkelissa Azure Data Factory tähän. Esimerkki: 
    
      "source": {
        "type": "SqlSource",
        "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm\\'', WindowStart, WindowEnd)"
      },

Tämä kysely lukee tiedot 'MyTable', joka on sektoria kesto-alue. Suorita tämä muotoilu myös aina takaa tämän ongelman. 

Joskus saatat haluta lukea koko taulukko (oletetaan, kerran siirtää vain) ja voivat määritellä sqlReaderQuery seuraavasti:

    
    "source": {
                "type": "SqlSource",
                "sqlReaderQuery": "select * from MyTable"
              },
    

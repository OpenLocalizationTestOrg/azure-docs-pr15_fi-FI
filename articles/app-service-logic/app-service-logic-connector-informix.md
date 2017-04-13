<properties
   pageTitle="Informix Connectorin avulla Microsoft Azure-sovelluksen palvelun | Microsoft Azure"
   description="Informix-yhdistin käyttäminen logiikan app Käynnistimet ja toiminnot"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="gplarsen"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="05/31/2016"
   ms.author="plarsen"/>

# <a name="informix-connector"></a>Informix-yhdistin
>[AZURE.NOTE] Tässä versiossa on artikkelissa koskee logiikan sovellusten 2014 – 12-01 – esikatselu rakenteen versio.

Informix yhdistimen Microsoft on API-sovellus, jonka yhteyden sovellusten Azure-sovelluksen kautta resurssit, jotka on tallennettu IBM Informix-tietokantaan. Yhdistimen on Microsoft-Client muodostaa Informix-palvelimen etätietokoneiden TCP/IP verkkoyhteyden, mukaan lukien paikallisen Informix palvelinten käyttäminen Azure palvelun Bus välitys Azure hybrid yhteyksiä. Yhdistimen tukee tietokannan seuraavat toimenpiteet:

- Valitse luku rivien laskeminen
- Kyselyn rivien laskeminen ja valitse sen jälkeen valitse Laske lukeminen
- Lisää kaksi tai useita (joukko) rivejä käyttämällä Lisää
- Muuta yhden rivin tai useita (joukko) rivejä UPDATEN avulla
- Poistaa yhden rivin tai useita (joukko) rivejä käyttämällä poistaminen
- Voit muuttaa rivien laskeminen ja Päivitä kohtaa, johon on KOHDISTIN sen jälkeen Valitse KOHDISTIN luku
- Jos haluat poistaa rivien laskeminen ja Päivitä kohtaa, johon on KOHDISTIN sen jälkeen Valitse KOHDISTIN luku
- Suorittaa syöttö- ja parametreilla, palautusarvo, tulosjoukon,-puhelun
- Mukautettuja komentoja ja koosteen toimintoja käyttämällä valitsemalla Lisää, Päivitä, poista

## <a name="triggers-and-actions"></a>Käynnistimet ja toiminnot
Yhdistimen tukee seuraava logiikan app käynnistimien toiminnot:

Käynnistimien | Toiminnot
--- | ---
<ul><li>Kyselyn tiedot</li></ul> | <ul><li>Lisää samalla kertaa</li><li>Lisää</li><li>Joukkopäivitys</li><li>Päivitys</li><li>Kutsu</li><li>Poista samalla kertaa</li><li>Poista</li><li>Valitse</li><li>Ehdollinen päivitys</li><li>Kirjaa EntitySet</li><li>Ehdollinen poistaminen</li><li>Valitse yksi kohde</li><li>Poista</li><li>Upsert EntitySet</li><li>Mukautetut komennot</li><li>Koosteen toiminnot</li></ul>


## <a name="create-the-informix-connector"></a>Informix-yhdistimen luominen
Voit määrittää yhdistimen logiikan sovelluksen tai Azure Marketplace-sivustosta, kuten seuraavassa esimerkissä:  

1. Valitse Azure startboard **Marketplacesta**.
2. Valitse **kaikki** -sivu **informix** Kirjoita **Hae kaikki** -ruutuun ja valitse sitten enter-näppäintä.
3. Etsi kaikki tulokset ruudun, valitse **Informix yhdistin**.
4. Valitse **Luo**Informix yhdistimen kuvaus-sivu.
5. Kirjoita nimi (esimerkiksi "InformixConnectorNewOrders"), sovellus-palvelun suunnittelu ja muita ominaisuuksia Informix yhdistimen paketti-sivu.
6. Valitse **pakettiasetukset**ja kirjoita seuraava pakettiasetukset.

    Nimi | Pakollinen |  Kuvaus
--- | --- | ---
ConnectionString | Kyllä | Informix asiakkaan yhteysmerkkijonon (esimerkiksi "verkko-osoite = palvelimen nimi; Verkko-portti = 9089; Käyttäjätunnus = käyttäjänimi; Salasanan = salasanan; alkuperäisen luettelon = nwind; oletus rakenteen = informix ").
Taulukot | Kyllä | Pilkuilla erotettu luettelo taulukon, näkymän ja alias nimet OData-toimintoja varten ja luo swagger dokumentaatio esimerkkien (esimerkiksi "NEWORDERS").
Ohjeita | Kyllä | Pilkuilla erotettu nimiluettelo ohjeiden mukaisesti ja -funktio (esimerkiksi "SPORDERID").
Paikallisesti käytettävät versiot | Ei | Ota käyttöön paikallisen Azure palvelun Bus välitys avulla.
ServiceBusConnectionString | Ei | Azure palvelun Bus välitys yhteysmerkkijono.
PollToCheckData | Ei | Valitse Laske lauseen logiikan sovelluksen käynnistimen käyttäminen (esimerkiksi "Valitse Laske (\*) NEWORDERS WHERE SHIPDATE IS NULL-").
PollToReadData | Ei | SELECT-lauseen logiikan sovelluksen käynnistimen käyttäminen (esimerkiksi "Valitse \* NEWORDERS SHIPDATE missä NULL FOR UPDATE-").
PollToAlterData | Ei | Päivitä tai poistolauseen logiikan sovelluksen käynnistimen käyttäminen (esimerkiksi "päivitys NEWORDERS määrittäminen SHIPDATE = nykyisen PÄIVÄMÄÄRÄN kohtaa, johon nykyisen, &lt;KOHDISTIMEN&gt;").

7. Valitse **OK**ja valitse sitten **Luo**.
8. Kun olet valmis, Pakettiasetukset näyttää seuraavankaltaiselta:  
![][1]


## <a name="logic-app-with-informix-connector-action-to-add-data"></a>Logiikan app Informix connector-toiminnolla voit lisätä tietoja ##
Voit määrittää logiikan sovelluksen-toiminnon käyttäminen API Lisää tai Kirjaa kohteen OData-toiminnon Informix-taulukon tietojen lisäämiseen. Esimerkiksi voit lisätä uusia tilauksen asiakastietueen, lisää SQL-lause identity-sarake on määritetty taulukon käsittelemällä identity-arvon tai vaikuttaa logiikan sovellukseen rivit (Valitse ORDID-lopullinen taulukko (Lisää KYSELYJÄ NEWORDERS (ASIAKASTUNNUS, LÄHETYSNIMI, SHIPADDR, TOIMITUSKAUPUNKI, SHIPREG, SHIPZIP)-arvot (?,?,?,?,?,?))).

> [AZURE.TIP] Informix yhteyden "*Kirjaa EntitySet*" palauttaa identity-sarakkeen arvo ja "*API Lisää*" palauttaa rivit vaikuttaa

1. Valitse Azure startboard **+** (plusmerkki) **Web + Mobile**ja sitten **logiikan sovelluksen**.
2. Kirjoita nimi (esimerkiksi "NewOrdersInformix"), sovellus-palvelun suunnittelu ja muita ominaisuuksia ja valitse sitten **Luo**.
3. Valitse Azure startboard logiikan juuri luomasi sovelluksen, **asetukset**ja sitten **Käynnistimet ja toiminnot**.
4. Valitse Käynnistimet ja toiminnot-sivu logiikan sovelluksen mallien **luominen alusta alkaen** .
5. API sovellukset-ruudussa Valitse **Toistuminen**, määrittää korkojakso ja aikavälin, ja valitse **valintamerkki**.
6. API sovellukset-ruudussa Valitse **Informix yhdistin**, laajenna toimintojen luettelo ja valitse **NEWORDER lisääminen**.
7. Laajenna parametrit-luettelosta antamaan seuraavat arvot:  

    Nimi | Arvo
--- | --- 
ASIAKASTUNNUS | 10042
KIRJAIMELLA | 10000
LÄHETYSNIMI | Kuitata K Kountry säilö 
SHIPADDR | 12 Orkesteri Terrace
TOIMITUSKAUPUNKI | Walla Walla 
SHIPREG | WA
SHIPZIP | 99362 

8. Valitse Tallenna Toimintoasetukset ja **Tallenna** **valintamerkki** .
9. Asetusten pitäisi näyttää seuraavasti:  
![][3]  
10. Valitse **kaikki toimii** luettelosta **toiminnoissa**luettelon ensimmäisestä-kohteen (uusin asennus). 
11. Valitse **toiminto** -kohteen **informixconnectorneworders** **logiikan sovelluksen Suorita** -sivu.
12. Valitse **SYÖTTEIDEN LINKKI** **logiikan sovelluksen toiminto** -sivu. Informix yhdistimen käyttää syötteiden käsittelemiseen Parametroitu INSERT-lauseessa.
13. Valitse **Tulostus LINKKI** **logiikan sovelluksen toiminto** -sivu. Syötteiden pitäisi näyttää seuraavasti:  
![][4]

#### <a name="what-you-need-to-know"></a>Mitä on tiedettävä

- Yhdistimen katkaisee Informix taulukoiden nimet, kun muodostaen logiikan sovelluksen toimintojen nimet. Esimerkiksi **NEWORDERS lisääminen** toiminnon katkaistaan **NEWORDER**lisääminen.
- Kun olet tallentanut logiikan sovelluksen **Käynnistimet ja toiminnot**, logiikan app käsittelee toiminto. Mahdollinen viive määrä (esimerkiksi 3 – 5 sekuntia) (sekunteina) ennen logiikan app käsittelee toiminto. Vaihtoehtoisesti voit napsauttaa **Suorita** käsittelemään toiminto.
- Informix yhdistimen määrittää EntitySet määritteitä, onko jäsenen vastaa Informix-sarake, jossa oletusarvon mukaan lukien tai jäsenet luotu sarakkeiden (kuten käyttäjätiedot). Logiikan sovellus tuo näkyviin EntitySet jäsenen tunnus-nimen osoittamaan Informix-sarakkeet, jotka edellyttävät arvon vieressä näkyy punainen tähti. Älä kirjoita arvo ORDID jäsenen, joka vastaa Informix identity-sarake. Voit kirjoittaa arvoja muut valinnainen jäsenet (KOHTEITA, ORDDATE, REQDATE, KIRJAIMELLA, rahti, SHIPCTRY), jotka vastaavat Informix sarakkeiden oletusarvoilla. 
- Informix yhdistimen palauttaa logiikan sovellukseen vastauksen kirjaa EntitySet, joka tunnistetietojen sarakkeiden arvoja joka johdetaan DRDA SQLDARD (SQL tietojen alueen vastaustiedot) sisältää valmiita Lisää SQL-lause. Informix-palvelin ei palauta oletusarvoilla sarakkeisiin lisätyt arvot.  


## <a name="logic-app-with-informix-connector-action-to-add-bulk-data"></a>Logiikan app Informix connector-toiminnolla voit lisätä tiedot joukkona ##
Voit määrittää logiikan sovelluksen-toiminto, joka lisää API joukkona Lisää toiminnon Informix-taulukon tietoja. Esimerkiksi voit lisätä kaksi uutta asiakkaan tilauksen-tietuetta käsittelemällä SQL Lisää lauseke, joka käyttää matriisin rivin arvoista on tunnussarake on määritetty taulukon rivit vaikuttaa logiikan-sovellukseen, joka palauttaa (Valitse ORDID-lopullinen taulukko (Lisää KYSELYJÄ NEWORDERS (ASIAKASTUNNUS, LÄHETYSNIMI, SHIPADDR, TOIMITUSKAUPUNKI, SHIPREG, SHIPZIP)-arvot (?,?,?,?,?,?))).

1. Valitse Azure startboard **+** (plusmerkki) **Web + Mobile**ja sitten **logiikan sovelluksen**.
2. Kirjoita nimi (esimerkiksi "NewOrdersBulkInformix"), sovellus-palvelun suunnittelu ja muita ominaisuuksia ja valitse sitten **Luo**.
3. Valitse Azure startboard logiikan juuri luomasi sovelluksen, **asetukset**ja sitten **Käynnistimet ja toiminnot**.
4. Valitse Käynnistimet ja toiminnot-sivu logiikan sovelluksen mallien **luominen alusta alkaen** .
5. API sovellukset-ruudussa Valitse **Toistuminen**, määrittää korkojakso ja aikavälin, ja valitse **valintamerkki**.
6. API sovellukset-ruudussa Valitse **Informix yhdistin**, laajenna toimintojen luettelo ja valitse **Uusi lisääminen joukkona**.
7. Lisää **rivejä** -arvo matriisina. Esimerkiksi kopioiminen ja liittäminen seuraavasti:  

    ```
    [{"custid":10081,"shipid":10000,"shipname":"Trail's Head Gourmet Provisioners","shipaddr":"722 DaVinci Blvd.","shipcity":"Kirkland","shipreg":"WA","shipzip":"98034"},{"custid":10088,"shipid":10000,"shipname":"White Clover Markets","shipaddr":"305 14th Ave. S. Suite 3B","shipcity":"Seattle","shipreg":"WA","shipzip":"98128","shipctry":"USA"}]
    ```
        
8. Valitse Tallenna Toimintoasetukset ja **Tallenna** **valintamerkki** . Asetusten pitäisi näyttää seuraavasti:  
![][6]

9. Valitse **Toiminnot**-kohdassa **kaikki suoritetaan** -luettelosta luettelon ensimmäisestä-kohteen (uusin asennus).
10. Valitse **toimen** **logiikan sovelluksen Suorita** -sivu.
11. Napsauta **SYÖTTEIDEN LINKKI** **logiikan sovelluksen toiminto** -sivu. Hyväksyttäväksi pitäisi näyttää seuraavasti:  
[][7]
12. Valitse **Tulostus LINKKI** **logiikan sovelluksen toiminto** -sivu. Hyväksyttäväksi pitäisi näyttää seuraavasti:  
![][8]

#### <a name="what-you-need-to-know"></a>Mitä on tiedettävä

- Yhdistimen katkaisee Informix taulukoiden nimet, kun muodostaen logiikan sovelluksen toimintojen nimet. Esimerkiksi **NEWORDERS lisääminen joukkona** toiminnon katkaistaan **Uusi**lisääminen joukkona.
- Informix-tietokanta voi olla kirjainkoko taulukoiden ja sarakkeiden nimet. Esimerkiksi joukkona Lisää toiminto taulukon sarakkeiden nimet ehkä määritettävä pienillä kirjaimilla ("Asiakastunnus") sen sijaan, että isoilla kirjaimilla ("ASIAKASTUNNUS").
- Käyttäjätietojen sarakkeiden (kuten ORDID), nullable sarakkeiden (kuten SHIPDATE) ja sarakkeiden oletusarvoilla (kuten ORDDATE REQDATE KIRJAIMELLA, rahti, SHIPCTRY) poistamalla Informix tietokannan Luo arvot.
- Määrittämällä "tänään" ja "huomenna" Informix yhdistimen Luo "Nykyinen päivämäärä" ja "Nykyinen päivämäärä + 1 päivä-funktioita (esimerkiksi REQDATE). 


## <a name="logic-app-with-informix-connector-trigger-to-read-alter-or-delete-data"></a>Logiikan-sovelluksessa, jossa Informix yhdistimen käynnistimen lukea, muuta tai poista tiedot ##
Voit määrittää logiikan sovelluksen käynnistimen äänestyksen järjestäminen ja lukea tietoja koosteen API kyselyn tiedot-toiminnon Informix-taulukosta. Esimerkiksi jokin voivat lukea tai Lisää uusi tilaus asiakastietueet, palauttaa tietueet logiikan-sovellukseen. Paketti tai sovelluksesta Informix yhteysasetukset pitäisi näyttää seuraavasti:

    Asetus | Arvo
--- | --- | ---
PollToCheckData | Valitse Laske (\*) NEWORDERS missä SHIPDATE NULL-
PollToReadData | Valitse \* -NEWORDERS missä SHIPDATE NULL päivitys
PollToAlterData | <no value specified>


Voit myös määrittää logiikan sovelluksen käynnistimen äänestyksen järjestäminen, lue ja muuttaa koosteen API kyselyn tiedot-toiminnon Informix-taulukon tietojen. Esimerkiksi jokin voivat lukea tai Lisää uusia tilauksen asiakastietueet, Päivitä rivi-arvot (ennen päivitystä) valituissa tietueissa, joka palauttaa logiikan-sovellukseen. Paketti tai sovelluksesta Informix yhteysasetukset pitäisi näyttää seuraavasti:

    Asetus | Arvo
--- | --- | ---
PollToCheckData | Valitse Laske (\*) NEWORDERS missä SHIPDATE NULL-
PollToReadData | Valitse \* -NEWORDERS missä SHIPDATE NULL päivitys
PollToAlterData | Päivitä NEWORDERS määrittäminen SHIPDATE = nykyisen PÄIVÄMÄÄRÄN kohtaa, johon nykyisen OF &lt;KOHDISTIN&gt;


Lisäksi voit määrittää logiikan sovelluksen käynnistimen äänestyksen järjestäminen, lue ja poista tiedot koosteen API kyselyn tiedot-toiminnon Informix-taulukosta. Esimerkiksi jokin voivat lukea tai Lisää uusi tilaus asiakastietueet, Poista rivit (ennen poistaminen) valituissa tietueissa, joka palauttaa logiikan-sovellukseen. Paketti tai sovelluksesta Informix yhteysasetukset pitäisi näyttää seuraavasti:

    Asetus | Arvo
--- | --- | ---
PollToCheckData | Valitse Laske (\*) NEWORDERS missä SHIPDATE NULL-
PollToReadData | Valitse \* -NEWORDERS missä SHIPDATE NULL päivitys
PollToAlterData | Poista NEWORDERS kohtaa, johon nykyisen OF &lt;KOHDISTIN&gt;

Tässä esimerkissä logiikan app äänestyksen järjestäminen, lukea, päivittää, ja lukea Informix-taulukon tiedot uudelleen.

1. Valitse Azure startboard **+** (plusmerkki) **Web + Mobile**ja sitten **logiikan sovelluksen**.
2. Kirjoita nimi (esimerkiksi "ShipOrdersInformix"), sovellus-palvelun suunnittelu ja muita ominaisuuksia ja valitse sitten **Luo**.
3. Valitse Azure startboard logiikan juuri luomasi sovelluksen, **asetukset**ja sitten **Käynnistimet ja toiminnot**.
4. Valitse Käynnistimet ja toiminnot-sivu logiikan sovelluksen mallien **luominen alusta alkaen** .
5. API sovellukset-ruudussa Valitse **Informix yhdistin**, määrittää korkojakso ja aikavälin, ja valitse **valintamerkki**.
6. API sovellukset-ruudussa Valitse **Informix yhdistin**, laajenna toimintojen luettelo ja valitse **NEWORDERS valitseminen**.
7. Valitse Tallenna Toimintoasetukset ja **Tallenna** **valintamerkki** . Asetusten pitäisi näyttää seuraavasti:  
![][10]
8. Sulje **Käynnistimet ja toiminnot** -sivu valitsemalla ja valitse sitten Sulje **asetukset** -sivu.
9. Valitse **Toiminnot**-kohdassa **kaikki suoritetaan** -luettelosta luettelon ensimmäisestä-kohteen (uusin asennus).
10. Valitse **toimen** **logiikan sovelluksen Suorita** -sivu.
11. Valitse **Tulostus LINKKI** **logiikan sovelluksen toiminto** -sivu. Hyväksyttäväksi pitäisi näyttää seuraavasti:  
![][11]


## <a name="logic-app-with-informix-connector-action-to-remove-data"></a>Informix connector-toiminnolla voit poistaa tiedot sovelluksen logiikka ##
Voit määrittää logiikan sovellus-toiminnon käyttäminen API Poista tai Kirjaa kohteen OData-toiminnon Informix-taulukon tietojen poistaminen. Esimerkiksi voit lisätä uusia tilauksen asiakastietueen, lisää SQL-lause identity-sarake on määritetty taulukon käsittelemällä identity-arvon tai vaikuttaa logiikan sovellukseen rivit (Valitse ORDID-lopullinen taulukko (Lisää KYSELYJÄ NEWORDERS (ASIAKASTUNNUS, LÄHETYSNIMI, SHIPADDR, TOIMITUSKAUPUNKI, SHIPREG, SHIPZIP)-arvot (?,?,?,?,?,?))).

## <a name="create-logic-app-using-informix-connector-to-remove-data"></a>Informix Connectorin avulla voit poistaa tietoja logiikan-sovelluksen luominen ##
Voit luoda uuden logiikan sovelluksen from Azure Marketplacesta kuluessa ja käyttää sitten Informix-yhdistimen toiminnon Poista asiakkaiden tilaukset. Esimerkiksi Informix yhdistimen ehdollinen poistotoiminto avulla voi käsitellä Poista SQL-lause (Poista-NEWORDERS missä ORDID > = 10000).

1. Valitse **Käynnistä** Azure-taulun toiminto-valikosta **+** (plusmerkki), valitse **Web + Mobile**ja valitse sitten **logiikan sovelluksen**. 
2. Kirjoita **nimi**, esimerkiksi **RemoveOrdersInformix** **luominen logiikan sovellus** -sivu.
3. Valitse tai määritä muut asetukset (esimerkiksi palvelusopimus, resurssiryhmä)-arvot.
4. Asetusten pitäisi nyt. Valitse **Luo**:  
![][12]
5. Valitse **asetukset** -sivu **Käynnistimet ja toiminnot**.
6. Valitse **Käynnistimet ja toiminnot** -sivu **logiikan sovelluksen mallit** -luettelosta **luominen alusta alkaen**.
7. Valitse **Käynnistimet ja toiminnot** -sivu **API sovellukset** -paneelissa resurssi-ryhmästä **Toistuva tapahtuma**.
8. Logiikan app suunnitteluosaan, valitse **Toistuminen** -kohdetta, **korkojakso** ja **aikavälin**, esimerkiksi **päivän** ja **1**, ja valitse sitten Tallenna kohde toistoasetukset **valintamerkki** .
9. Valitse **Käynnistimet ja toiminnot** -sivu **API sovellukset** -paneelissa resurssiryhmä- **Informix yhdistin**.
10. Logiikan app suunnitteluosaan, valitse **Informix yhdistimen** toiminnon kohde, napsauta kolmea pistettä (****...) voit laajentaa luettelon toiminnot ja valitse **Ehdollinen poistaminen N**.
11. Informix yhdistimen toiminnon kohteessa, kirjoita **ordid sivu 10000** **lauseke, joka kertoo alijoukkoa tapahtumia**varten.
12. Valitse **valintamerkki** Tallenna Toimintoasetukset ja valitse sitten **Tallenna**. Asetusten pitäisi näyttää seuraavasti:  
![][13]
13. Sulje **Käynnistimet ja toiminnot** -sivu valitsemalla ja valitse sitten Sulje **asetukset** -sivu.
14. Valitse **Toiminnot**-kohdassa **kaikki suoritetaan** -luettelosta luettelon ensimmäisestä-kohteen (uusin asennus).
15. Valitse **toimen** **logiikan sovelluksen Suorita** -sivu.
16. Valitse **Tulostus LINKKI** **logiikan sovelluksen toiminto** -sivu. Hyväksyttäväksi pitäisi näyttää seuraavasti:  
![][14]

**Huomautus:** Logiikan app designer katkaisee taulukoiden nimet. Esimerkiksi **Ehdollinen poistaminen NEWORDERS** toiminnon katkaistaan **Ehdollinen poistaminen N**.


> [AZURE.TIP] SQL-lauseet avulla voit luoda esimerkkitaulukko ja tallennettujen toimintosarjojen. 

Voit luoda NEWORDERS esimerkkitaulukko Informix SQL DDL-lauseita käyttäen:
 
    create table neworders (  
        ordid serial(10000) unique ,  
        custid int not null ,  
        empid int not null default 10000 ,  
        orddate date not null default today ,  
        reqdate date default today ,  
        shipdate date ,  
        shipid int not null default 10000 ,  
        freight decimal (9,2) not null default 0.00 ,  
        shipname char (40) not null ,  
        shipaddr char (60) not null ,  
        shipcity char (20) not null ,  
        shipreg char (15) not null ,  
        shipzip char (10) not null ,  
        shipctry char (15) not null default ''USA'' 
        )


Voit luoda otoksen SPORDERID tallennettu toimintosarja-Informix DDL-lausetta:
 
    create procedure sporderid ( ord_id int)  
        returning int, int, int, date, date, date, int, decimal (9,2), char (40), char (60), char (20), char (15), char (10), char (15)  
        define xordid, xcustid, xempid, xshipid int;  
        define xorddate, xreqdate, xshipdate date;  
        define xfreight decimal (9,2);  
        define xshipname char (40);  
        define xshipaddr char (60);  
        define xshipcity char (20);  
        define xshipreg, xshipctry char (15);  
        define xshipzip char (10);  
        select ordid, custid, empid, orddate, reqdate, shipdate, shipid, freight, shipname, shipaddr, shipcity, shipreg, shipzip, shipctry  
            into xordid, xcustid, xempid, xorddate, xreqdate, xshipdate, xshipid, xfreight, xshipname, xshipaddr, xshipcity, xshipreg, xshipzip, xshipctry  
            from neworders where ordid = ord_id;  
        return xordid, xcustid, xempid, xorddate, xreqdate, xshipdate, xshipid, xfreight, xshipname, xshipaddr, xshipcity, xshipreg, xshipzip, xshipctry;  
    end procedure; 


## <a name="hybrid-configuration-optional"></a>Hybrid määrittäminen (valinnainen)

> [AZURE.NOTE] Tämä vaihe on pakollinen vain, jos käytössäsi on DB2 yhdistimen paikalliseen palomuurin takana.

Sovelluksen palvelu käyttää Hybrid hallintatoiminnon muodostaa suojatusti paikalliseen järjestelmään. Jos yhdistimen käyttää paikalliseen IBM DB2 Server for Windows-Hybrid Yhteyksienhallinnan ei tarvita.

Katso [Hybrid Yhteyksienhallinnan](app-service-logic-hybrid-connection-manager.md).


## <a name="do-more-with-your-connector"></a>Älä lisää ja sitten yhdistimen
Yhdistimen on luotu, voit lisätä business työnkulkuun logiikan sovellusta käytettäessä. Katso [mitä logiikan sovelluksia?](app-service-logic-what-are-logic-apps.md).

Luo käyttämällä REST API API-sovelluksia. Katso [yhdistimien ja API sovellusten viittaus](http://go.microsoft.com/fwlink/p/?LinkId=529766).

Voit myös tarkastella suorituskyvyn Tilasto- ja tietoturva yhdistin. Katso [hallinta ja valvonta valmiin API-sovellusten ja yhdistimet](app-service-logic-monitor-your-connectors.md).


<!--Image references-->
[1]: ./media/app-service-logic-connector-informix/ApiApp_InformixConnector_Create.png
[2]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersInformix_Create.png
[3]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersInformix_TriggersActions.png
[4]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersInformix_Outputs.png
[5]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersBulkInformix_Create.png
[6]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersBulkInformix_TriggersActions.png
[7]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersBulkInformix_Inputs.png
[8]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersBulkInformix_Outputs.png
[9]: ./media/app-service-logic-connector-informix/LogicApp_UpdateOrdersInformix_Create.png
[10]: ./media/app-service-logic-connector-informix/LogicApp_UpdateOrdersInformix_TriggersActions.png
[11]: ./media/app-service-logic-connector-informix/LogicApp_UpdateOrdersInformix_Outputs.png
[12]: ./media/app-service-logic-connector-informix/LogicApp_RemoveOrdersInformix_Create.png
[13]: ./media/app-service-logic-connector-informix/LogicApp_RemoveOrdersInformix_TriggersActions.png
[14]: ./media/app-service-logic-connector-informix/LogicApp_RemoveOrdersInformix_Outputs.png



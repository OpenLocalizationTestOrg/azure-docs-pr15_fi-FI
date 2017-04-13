<properties
   pageTitle="Microsoft Azure App palvelussa DB2 Connectorin avulla | Microsoft Azure"
   description="Voit käyttää DB2 yhdistimen logiikan app Käynnistimet ja toiminnot"
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

# <a name="db2-connector"></a>DB2-yhdistin
>[AZURE.NOTE] Tässä versiossa on artikkelissa koskee logiikan sovellusten 2014 – 12-01 – esikatselu rakenteen versio.

Microsoft yhdistimen DB2 on API-sovellus, jonka yhteyden sovellusten Azure-sovelluksen kautta resurssit, jotka on tallennettu IBM DB2-tietokantaan. Yhdistimen on Microsoft-Client muodostaa DB2-palvelin etätietokoneiden TCP/IP-verkkoyhteyden, mukaan lukien Azure hybrid yhteydet paikallisen DB2-palvelimiin käyttämällä Azure palvelun Bus välitys yhdistimen tukee tietokannan seuraavat toimenpiteet:

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


## <a name="create-the-db2-connector"></a>Luo DB2-yhdistin
Voit määrittää yhdistimen logiikan sovelluksen tai Azure Marketplace-sivustosta, kuten seuraavassa esimerkissä:  

1. Valitse Azure startboard **Marketplacesta**.
2. Valitse **kaikki** -sivu **db2** Kirjoita **Hae kaikki** -ruutuun ja valitse sitten enter-näppäintä.
3. Hae kaikki tulokset ruudun, valitse **DB2 yhdistin**.
4. Valitse **Luo**DB2 yhdistimen kuvaus-sivu.
5. Kirjoita nimi (esimerkiksi "Db2ConnectorNewOrders"), sovellus-palvelun suunnittelu ja muita ominaisuuksia DB2 yhdistimen paketti-sivu.
6. Valitse **pakettiasetukset**ja kirjoita seuraava pakettiasetukset:  

    Nimi | Pakollinen |  Kuvaus
--- | --- | ---
ConnectionString | Kyllä | DB2 asiakkaan yhteysmerkkijonon (esimerkiksi "verkko-osoite = palvelimen nimi; Verkko-portti = 50000; Käyttäjätunnus = käyttäjänimi; Salasanan = salasanan; alkuperäisen luettelon = NÄYTE; Pakkaa sivustokokoelman = NWIND; oletus rakenteen = NWIND ").
Taulukot | Kyllä | Pilkuilla erotettu luettelo taulukon, näkymän ja alias nimet OData-toimintoja varten ja luo swagger dokumentaatio esimerkkien (esimerkiksi "*NEWORDERS*").
Ohjeita | Kyllä | Pilkuilla erotettu nimiluettelo ohjeiden mukaisesti ja -funktio (esimerkiksi "SPORDERID").
Paikallisesti käytettävät versiot | Ei | Ota käyttöön paikallisen Azure palvelun Bus välitys avulla.
ServiceBusConnectionString | Ei | Azure palvelun Bus välitys yhteysmerkkijono.
PollToCheckData | Ei | Valitse Laske lauseen logiikan sovelluksen käynnistimen käyttäminen (esimerkiksi "Valitse Laske (\*) NEWORDERS WHERE SHIPDATE IS NULL-").
PollToReadData | Ei | SELECT-lauseen logiikan sovelluksen käynnistimen käyttäminen (esimerkiksi "Valitse \* NEWORDERS SHIPDATE missä NULL FOR UPDATE-").
PollToAlterData | Ei | Päivitä tai poistolauseen logiikan sovelluksen käynnistimen käyttäminen (esimerkiksi "päivitys NEWORDERS määrittäminen SHIPDATE = nykyisen PÄIVÄMÄÄRÄN kohtaa, johon nykyisen, &lt;KOHDISTIMEN&gt;").

7. Valitse **OK**ja valitse sitten **Luo**.
8. Kun olet valmis, Pakettiasetukset näyttää seuraavankaltaiselta:  
![][1]


## <a name="logic-app-with-db2-connector-action-to-add-data"></a>Logiikan app DB2 connector-toiminnolla voit lisätä tietoja ##
Voit määrittää logiikan app-toimintoa, voit lisätä tietoja DB2 taulukkoon ohjatulla API Insert- tai Kirjaa kohteen OData-toiminnon. Esimerkiksi voit lisätä uusia tilauksen asiakastietueen, lisää SQL-lause identity-sarake on määritetty taulukon käsittelemällä identity-arvon tai rivien vaikuttaa logiikan sovellukseen (Valitse ORDID-lopullinen taulukko (Lisää YHDEKSI NWIND. NEWORDERS (ASIAKASTUNNUS, LÄHETYSNIMI, SHIPADDR, TOIMITUSKAUPUNKI, SHIPREG, SHIPZIP)-ARVOT (?,?,?,?,?,?))).

> [AZURE.TIP] DB2 yhteyden "*Kirjaa EntitySet*" palauttaa identity-sarakkeen arvo ja "*API Lisää*" palauttaa rivit vaikuttaa

1. Valitse Azure startboard **+** (plusmerkki) **Web + Mobile**ja sitten **logiikan sovelluksen**.
2. Kirjoita nimi (esimerkiksi "NewOrdersDb2"), sovellus-palvelun suunnittelu ja muita ominaisuuksia ja valitse sitten **Luo**.
3. Valitse Azure startboard logiikan juuri luomasi sovelluksen, **asetukset**ja sitten **Käynnistimet ja toiminnot**.
4. Valitse Käynnistimet ja toiminnot-sivu logiikan sovelluksen mallien **luominen alusta alkaen** .
5. API sovellukset-ruudussa Valitse **Toistuminen**, määrittää korkojakso ja aikavälin, ja valitse **valintamerkki**.
6. API sovellukset-ruudussa Valitse **DB2 yhdistin**, laajenna toimintojen luettelo ja valitse **NEWORDER lisääminen**.
7. Laajenna parametrit-luettelosta antamaan seuraavat arvot:  

    Nimi | Arvo
--- | --- 
ASIAKASTUNNUS | 10042
LÄHETYSNIMI | Kuitata K Kountry säilö 
SHIPADDR | 12 Orkesteri Terrace
TOIMITUSKAUPUNKI | Walla Walla 
SHIPREG | WA
SHIPZIP | 99362 

8. Valitse Tallenna Toimintoasetukset ja **Tallenna** **valintamerkki** .
9. Asetusten pitäisi näyttää seuraavasti:  
![][3]

10. Valitse **kaikki toimii** luettelosta **toiminnoissa**luettelon ensimmäisestä-kohteen (uusin asennus). 
11. Valitse **toiminto** -kohteen **db2connectorneworders** **logiikan sovelluksen Suorita** -sivu.
12. Valitse **SYÖTTEIDEN LINKKI** **logiikan sovelluksen toiminto** -sivu. DB2 yhdistimen käyttää syötteiden käsittelemiseen Parametroitu INSERT-lauseessa.
13. Valitse **Tulostus LINKKI** **logiikan sovelluksen toiminto** -sivu. Syötteiden pitäisi näyttää seuraavasti:  
![][4]

#### <a name="what-you-need-to-know"></a>Mitä on tiedettävä

- Yhdistimen katkaisee DB2 taulukoiden nimet, kun muodostaen logiikan sovelluksen toimintojen nimet. Esimerkiksi **NEWORDERS lisääminen** toiminnon katkaistaan **NEWORDER**lisääminen.
- Kun olet tallentanut logiikan sovelluksen **Käynnistimet ja toiminnot**, logiikan app käsittelee toiminto. Mahdollinen viive määrä (esimerkiksi 3 – 5 sekuntia) (sekunteina) ennen logiikan app käsittelee toiminto. Vaihtoehtoisesti voit napsauttaa **Suorita** käsittelemään toiminto.
- DB2 yhdistimen määrittää EntitySet määritteitä, mukaan lukien jäsenen vastaa DB2 saraketta oletus- tai jäsenet luotu sarakkeiden (kuten käyttäjätiedot). Logiikan sovellus tuo näkyviin EntitySet jäsenen tunnus-nimen osoittamaan DB2-sarakkeet, jotka edellyttävät arvon vieressä näkyy punainen tähti. Älä kirjoita arvo ORDID jäsenen, joka vastaa DB2 identity-sarake. Voit kirjoittaa arvoja muut valinnainen jäsenet (KOHTEITA, ORDDATE, REQDATE, KIRJAIMELLA, rahti, SHIPCTRY), jotka vastaavat DB2 sarakkeiden oletusarvoilla. 
- DB2 yhdistimen palauttaa logiikan sovellukseen vastauksen kirjaa EntitySet, joka tunnistetietojen sarakkeiden arvoja joka johdetaan DRDA SQLDARD (SQL tietojen alueen vastaustiedot) sisältää valmiita Lisää SQL-lause. DB2-palvelin ei palauta oletusarvoilla sarakkeisiin lisätyt arvot.  


## <a name="logic-app-with-db2-connector-action-to-add-bulk-data"></a>Logiikan app DB2 connector-toiminnolla voit lisätä tiedot joukkona ##
Voit määrittää logiikan-sovelluksen toiminto, joka tietojen lisääminen taulukkoon DB2-Ohjelmointirajapinnan joukkona Lisää toiminnon. Esimerkiksi voit lisätä kaksi uutta asiakkaan tilauksen-tietuetta käsittelemällä SQL Lisää lauseke, joka käyttää matriisin rivin arvoista on tunnussarake on määritetty taulukon, joka palauttaa rivit vaikuttaa logiikan sovellukseen (Valitse ORDID-lopullinen taulukko (Lisää YHDEKSI NWIND. NEWORDERS (ASIAKASTUNNUS, LÄHETYSNIMI, SHIPADDR, TOIMITUSKAUPUNKI, SHIPREG, SHIPZIP)-ARVOT (?,?,?,?,?,?))).

1. Valitse Azure startboard **+** (plusmerkki) **Web + Mobile**ja sitten **logiikan sovelluksen**.
2. Kirjoita nimi (esimerkiksi "NewOrdersBulkDb2"), sovellus-palvelun suunnittelu ja muita ominaisuuksia ja valitse sitten **Luo**.
3. Valitse Azure startboard logiikan juuri luomasi sovelluksen, **asetukset**ja sitten **Käynnistimet ja toiminnot**.
4. Valitse Käynnistimet ja toiminnot-sivu logiikan sovelluksen mallien **luominen alusta alkaen** .
5. API sovellukset-ruudussa Valitse **Toistuminen**, määrittää korkojakso ja aikavälin, ja valitse **valintamerkki**.
6. API sovellukset-ruudussa Valitse **DB2 yhdistin**, laajenna toimintojen luettelo ja valitse **Uusi lisääminen joukkona**.
7. Lisää **rivejä** -arvo matriisina. Esimerkiksi kopioiminen ja liittäminen seuraavasti:

    ```
    [{"CUSTID":10081,"SHIPNAME":"Trail's Head Gourmet Provisioners","SHIPADDR":"722 DaVinci Blvd.","SHIPCITY":"Kirkland","SHIPREG":"WA","SHIPZIP":"98034"},{"CUSTID":10088,"SHIPNAME":"White Clover Markets","SHIPADDR":"305 14th Ave. S. Suite 3B","SHIPCITY":"Seattle","SHIPREG":"WA","SHIPZIP":"98128","SHIPCTRY":"USA"}]
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

- Yhdistimen katkaisee DB2 taulukoiden nimet, kun muodostaen logiikan sovelluksen toimintojen nimet. Esimerkiksi **NEWORDERS lisääminen joukkona** toiminnon katkaistaan **Uusi**lisääminen joukkona.
- Käyttäjätietojen sarakkeiden (kuten ORDID), nullable sarakkeiden (kuten SHIPDATE) ja sarakkeiden oletusarvoilla (kuten ORDDATE REQDATE KIRJAIMELLA, rahti, SHIPCTRY) poistamalla DB2-tietokantaan Luo arvot.
- Määrittämällä "tänään" ja "huomenna" DB2 yhdistimen Luo "Nykyinen päivämäärä" ja "Nykyinen päivämäärä + 1 päivä-funktioita (esimerkiksi REQDATE). 


## <a name="logic-app-with-db2-connector-trigger-to-read-alter-or-delete-data"></a>Logiikan-sovelluksessa, jossa DB2 yhdistimen käynnistimen lukea, muuta tai poista tiedot ##
Voit määrittää logiikan sovelluksen käynnistimen äänestyksen järjestäminen ja lukea tietoja koosteen API kyselyn tiedot-toiminnon DB2-taulukosta. Esimerkiksi jokin voivat lukea tai Lisää uusi tilaus asiakastietueet, palauttaa tietueet logiikan-sovellukseen. Paketti tai sovelluksesta DB2 yhteysasetukset pitäisi näyttää seuraavasti:

    Asetus | Arvo
--- | --- | ---
PollToCheckData | Valitse Laske (\*) NEWORDERS missä SHIPDATE NULL-
PollToReadData | Valitse \* -NEWORDERS missä SHIPDATE NULL päivitys
PollToAlterData | <no value specified>


Voit myös määrittää logiikan sovelluksen käynnistimen äänestyksen järjestäminen, lue ja muuttaa koosteen API kyselyn tiedot-toiminnon DB2-taulukon tietojen. Esimerkiksi jokin voivat lukea tai Lisää uusia tilauksen asiakastietueet, Päivitä rivi-arvot (ennen päivitystä) valituissa tietueissa, joka palauttaa logiikan-sovellukseen. Paketti tai sovelluksesta DB2 yhteysasetukset pitäisi näyttää seuraavasti:

    Asetus | Arvo
--- | --- | ---
PollToCheckData | Valitse Laske (\*) NEWORDERS missä SHIPDATE NULL-
PollToReadData | Valitse \* -NEWORDERS missä SHIPDATE NULL päivitys
PollToAlterData | Päivitä NEWORDERS määrittäminen SHIPDATE = nykyisen PÄIVÄMÄÄRÄN kohtaa, johon nykyisen OF &lt;KOHDISTIN&gt;


Lisäksi voit määrittää logiikan sovelluksen käynnistimen äänestyksen järjestäminen, lue ja tietojen poistaminen koosteen API kyselyn tiedot-toiminnon DB2-taulukkoon. Esimerkiksi jokin voivat lukea tai Lisää uusi tilaus asiakastietueet, Poista rivit (ennen poistaminen) valituissa tietueissa, joka palauttaa logiikan-sovellukseen. Paketti tai sovelluksesta DB2 yhteysasetukset pitäisi näyttää seuraavasti:

    Asetus | Arvo
--- | --- | ---
PollToCheckData | Valitse Laske (\*) NEWORDERS missä SHIPDATE NULL-
PollToReadData | Valitse \* -NEWORDERS missä SHIPDATE NULL päivitys
PollToAlterData | Poista NEWORDERS kohtaa, johon nykyisen OF &lt;KOHDISTIN&gt;

Tässä esimerkissä logiikan app äänestyksen järjestäminen, lukea, päivittää, ja lukea DB2-taulukon tiedot uudelleen.

1. Valitse Azure startboard **+** (plusmerkki) **Web + Mobile**ja sitten **logiikan sovelluksen**.
2. Kirjoita nimi (esimerkiksi "ShipOrdersDb2"), sovellus-palvelun suunnittelu ja muita ominaisuuksia ja valitse sitten **Luo**.
3. Valitse Azure startboard logiikan juuri luomasi sovelluksen, **asetukset**ja sitten **Käynnistimet ja toiminnot**.
4. Valitse Käynnistimet ja toiminnot-sivu logiikan sovelluksen mallien **luominen alusta alkaen** .
5. API sovellukset-ruudussa Valitse **DB2 yhdistin**, määrittää korkojakso ja aikavälin, ja valitse **valintamerkki**.
6. API sovellukset-ruudussa Valitse **DB2 yhdistin**, laajenna toimintojen luettelo ja valitse **NEWORDERS valitseminen**.
7. Valitse Tallenna Toimintoasetukset ja **Tallenna** **valintamerkki** . Asetusten pitäisi näyttää seuraavasti:  
![][10]  
8. Sulje **Käynnistimet ja toiminnot** -sivu valitsemalla ja valitse sitten Sulje **asetukset** -sivu.
9. Valitse **Toiminnot**-kohdassa **kaikki suoritetaan** -luettelosta luettelon ensimmäisestä-kohteen (uusin asennus).
10. Valitse **toimen** **logiikan sovelluksen Suorita** -sivu.
11. Valitse **Tulostus LINKKI** **logiikan sovelluksen toiminto** -sivu. Hyväksyttäväksi pitäisi näyttää seuraavasti:  
![][11]


## <a name="logic-app-with-db2-connector-action-to-remove-data"></a>Logiikan app DB2 yhdistimen toiminnon tietojen poistamiseksi ##
Voit määrittää logiikan-sovelluksen toiminto, joka tietojen poistaminen DB2 taulukon käyttäminen API Poista tai Kirjaa kohteen OData-toimintoa. Esimerkiksi voit lisätä uusia tilauksen asiakastietueen, lisää SQL-lause identity-sarake on määritetty taulukon käsittelemällä identity-arvon tai rivien vaikuttaa logiikan sovellukseen (Valitse ORDID-lopullinen taulukko (Lisää YHDEKSI NWIND. NEWORDERS (ASIAKASTUNNUS, LÄHETYSNIMI, SHIPADDR, TOIMITUSKAUPUNKI, SHIPREG, SHIPZIP)-ARVOT (?,?,?,?,?,?))).

## <a name="create-logic-app-using-db2-connector-to-remove-data"></a>Logiikan sovelluksen DB2 Connectorin avulla voit poistaa tietoja luominen ##
Voit luoda uuden logiikan sovelluksen from Azure Marketplacesta kuluessa ja käyttää sitten DB2-yhdistimen toiminnon Poista asiakkaiden tilaukset. Esimerkiksi DB2 yhdistimen ehdollinen poistotoiminto avulla voi käsitellä Poista SQL-lause (Poista-NEWORDERS missä ORDID > = 10000).

1. Valitse **Käynnistä** Azure-taulun toiminto-valikosta **+** (plusmerkki), valitse **Web + Mobile**ja valitse sitten **logiikan sovelluksen**. 
2. Kirjoita **nimi**, esimerkiksi **RemoveOrdersDb2** **luominen logiikan sovellus** -sivu.
3. Valitse tai määritä muut asetukset (esimerkiksi palvelusopimus, resurssiryhmä)-arvot.
4. Asetusten pitäisi nyt. Valitse **Luo**:  
![][12]  
5. Valitse **asetukset** -sivu **Käynnistimet ja toiminnot**.
6. Valitse **Käynnistimet ja toiminnot** -sivu **logiikan sovelluksen mallit** -luettelosta **luominen alusta alkaen**.
7. Valitse **Käynnistimet ja toiminnot** -sivu **API sovellukset** -paneelissa resurssi-ryhmästä **Toistuva tapahtuma**.
8. Logiikan app suunnitteluosaan, valitse **Toistuminen** -kohdetta, **korkojakso** ja **aikavälin**, esimerkiksi **päivän** ja **1**, ja valitse sitten Tallenna kohde toistoasetukset **valintamerkki** .
9. Valitse **Käynnistimet ja toiminnot** -sivu **API sovellukset** -paneelissa resurssiryhmä- **DB2 yhdistin**.
10. Logiikan app suunnitteluosaan, valitse **DB2 yhdistimen** toiminnon kohde, napsauta kolmea pistettä (****...) voit laajentaa luettelon toiminnot ja valitse **Ehdollinen poistaminen N**.
11. Kirjoita DB2 yhdistimen toimi **ORDID sivu 10000** **lauseke, joka kertoo alijoukkoa tapahtumia**varten.
12. Valitse **valintamerkki** Tallenna Toimintoasetukset ja valitse sitten **Tallenna**. Asetusten pitäisi näyttää seuraavasti:  
![][13]  
13. Sulje **Käynnistimet ja toiminnot** -sivu valitsemalla ja valitse sitten Sulje **asetukset** -sivu.
14. Valitse **Toiminnot**-kohdassa **kaikki suoritetaan** -luettelosta luettelon ensimmäisestä-kohteen (uusin asennus).
15. Valitse **toimen** **logiikan sovelluksen Suorita** -sivu.
16. Valitse **Tulostus LINKKI** **logiikan sovelluksen toiminto** -sivu. Hyväksyttäväksi pitäisi näyttää seuraavasti:  
![][14]

**Huomautus:** Logiikan app designer katkaisee taulukoiden nimet. Esimerkiksi **Ehdollinen poistaminen NEWORDERS** toiminnon katkaistaan **Ehdollinen poistaminen N**.


> [AZURE.TIP] SQL-lauseet avulla voit luoda esimerkkitaulukko ja tallennettujen toimintosarjojen. 

Voit luoda NEWORDERS esimerkkitaulukko DB2 SQL DDL-lauseita käyttäen:
 
    CREATE TABLE ORDERS (  
        ORDID INT NOT NULL GENERATED BY DEFAULT AS IDENTITY (START WITH 10000, INCREMENT BY 1) ,  
        CUSTID INT NOT NULL ,  
        EMPID INT NOT NULL DEFAULT 10000 ,  
        ORDDATE DATE NOT NULL DEFAULT CURRENT DATE ,  
        REQDATE DATE DEFAULT CURRENT DATE ,  
        SHIPDATE DATE ,  
        SHIPID INT NOT NULL DEFAULT 10000,  
        FREIGHT DECIMAL (9,2) NOT NULL DEFAULT 0.00 ,  
        SHIPNAME CHAR (40) NOT NULL ,  
        SHIPADDR CHAR (60) NOT NULL ,  
        SHIPCITY CHAR (20) NOT NULL ,  
        SHIPREG CHAR (15) NOT NULL ,  
        SHIPZIP CHAR (10) NOT NULL ,  
        SHIPCTRY CHAR (15) NOT NULL DEFAULT 'USA' ,  
        PRIMARY KEY(ORDID)  
        )  
 
    CREATE UNIQUE INDEX XORDID ON ORDERS (ORDID ASC)  



Voit luoda otoksen SPOERID tallennettu toimintosarja-DB2 DDL-lausetta:
 
    CREATE OR REPLACE PROCEDURE NWIND.SPORDERID (IN ORDERID VARCHAR(128))  
        DYNAMIC RESULT SETS 1  
    P1: BEGIN  
        DECLARE CURSOR1 CURSOR WITH RETURN FOR  
            SELECT * FROM NWIND.NEWORDERS  
                WHERE ORDID = ORDERID;  
        OPEN CURSOR1;  
    END P1  
    ') 


## <a name="hybrid-configuration-optional"></a>Hybrid määrittäminen (valinnainen)

> [AZURE.NOTE] Tämä vaihe on pakollinen vain, jos käytössäsi on DB2 yhdistimen paikalliseen palomuurin takana.

Sovelluksen palvelu käyttää Hybrid hallintatoiminnon muodostaa suojatusti paikalliseen järjestelmään. Jos yhdistimen käyttää paikalliseen IBM DB2 Server for Windows-Hybrid Yhteyksienhallinnan ei tarvita.

Katso [Hybrid Yhteyksienhallinnan](app-service-logic-hybrid-connection-manager.md).


## <a name="do-more-with-your-connector"></a>Älä lisää ja sitten yhdistimen
Yhdistimen on luotu, voit lisätä business työnkulkuun logiikan sovellusta käytettäessä. Katso [mitä logiikan sovelluksia?](app-service-logic-what-are-logic-apps.md).

Luo käyttämällä REST API API-sovelluksia. Katso [yhdistimien ja API sovellusten viittaus](http://go.microsoft.com/fwlink/p/?LinkId=529766).

Voit myös tarkastella suorituskyvyn Tilasto- ja tietoturva yhdistin. Katso [hallinta ja valvonta valmiin API-sovellusten ja yhdistimet](app-service-logic-monitor-your-connectors.md).


<!--Image references-->
[1]: ./media/app-service-logic-connector-db2/ApiApp_Db2Connector_Create.png
[2]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersDb2_Create.png
[3]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersDb2_TriggersActions.png
[4]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersDb2_Outputs.png
[5]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersBulkDb2_Create.png
[6]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersBulkDb2_TriggersActions.png
[7]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersBulkDb2_Inputs.png
[8]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersBulkDb2_Outputs.png
[9]: ./media/app-service-logic-connector-db2/LogicApp_UpdateOrdersDb2_Create.png
[10]: ./media/app-service-logic-connector-db2/LogicApp_UpdateOrdersDb2_TriggersActions.png
[11]: ./media/app-service-logic-connector-db2/LogicApp_UpdateOrdersDb2_Outputs.png
[12]: ./media/app-service-logic-connector-db2/LogicApp_RemoveOrdersDb2_Create.png
[13]: ./media/app-service-logic-connector-db2/LogicApp_RemoveOrdersDb2_TriggersActions.png
[14]: ./media/app-service-logic-connector-db2/LogicApp_RemoveOrdersDb2_Outputs.png


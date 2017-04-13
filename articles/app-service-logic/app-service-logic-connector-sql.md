<properties
   pageTitle="SQL-Connectorin avulla logiikan sovelluksissa | Sovelluksen Microsoft Azure-palvelu"
   description="Voit luoda ja määritä SQL yhdistimen tai API-sovellus ja käyttää sitä Azure-sovelluksen palvelun logiikan-sovelluksessa"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="anuragdalmia"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="09/01/2016"
   ms.author="sameerch"/>


# <a name="get-started-with-the-microsoft-sql-connector-and-add-it-to-your-logic-app"></a>Microsoft SQL-Connector käytön aloittaminen ja lisää se logiikan-sovellukseen
>[AZURE.NOTE] Tässä versiossa on artikkelissa koskee logiikan sovellusten 2014 – 12-01 – esikatselu rakenteen versio. Valitse [SQL Azure-Ohjelmointirajapinnan](../connectors/connectors-create-api-sqlazure.md)Azure SQL-2015-08-01 – preview rakenne-versiosta.

Muodosta yhteys paikallisen SQL Server tai Azure SQL-tietokantaan, voit luoda ja muuttaa tietoja tai tietoja. Yhdistimien voidaan noutaa, prosessin tai push tietojen osana "työnkulun" logiikan-sovelluksissa. Kun käytät SQL-yhdistin työnkulun, voit toteuttaa skenaarioita. Voit tehdä esimerkiksi seuraavia toimia:

- Näytä SQL-tietokantaan käyttämällä verkossa tai mobiilisovelluksen asuvat tieto osan.
- Tietojen lisääminen SQL tietokantataulukon tallennustilan. Voit esimerkiksi kirjoittaa työntekijätietojen, Päivitä myyntitilausten ja niin edelleen.
- Tietojen noutaminen SQL ja käyttää sitä liiketoimintaprosessien. Voit esimerkiksi Hae asiakkaan tietueet ja sijoittaa ne asiakkaiden tietueet SalesForce.

Voit lisätä tämän työnkulun logiikan-sovelluksen osana yrityksen työnkulku ja prosessin tietoja SQL-yhdistin. 

## <a name="triggers-and-actions"></a>Käynnistimet ja toiminnot
*Käynnistimien* ovat tapahtumia, jotka käynnistyvät. Esimerkiksi kun tilaus on päivitetty tai kun uusi asiakas on lisätty. *Toiminto* on käynnistin tulos. Esimerkiksi kun tilaus on päivitetty, Myyjä lähettää ilmoituksen. Tai kun lisätään uusi asiakas, lähettää Tervetuloa sähköpostia uusi asiakas.

SQL-yhdistin voidaan käynnistimen tai toiminnon logiikan sovelluksen ja tukee tietojen JSON ja XML-muodossa. Pakettiin sisältyy jokaisen taulukon asetukset (Lisätietoja tämän artikkelin) on JSON toimintojoukon ja joukon XML-toimintoja.

SQL-yhdistin on seuraava Käynnistimet ja toiminnot, jotka ovat käytettävissä:

Käynnistimien | Toiminnot
--- | ---
Kyselyn tiedot | <ul><li>Lisää taulukkoon</li><li>Päivitä taulukko</li><li>Valitse taulukosta</li><li>Taulukon poistaminen</li><li>Soita tallennettu toimintosarja</li></ul>

## <a name="create-the-sql-connector"></a>Luo SQL-yhdistin

Yhdistimen logiikan-sovelluksen sisällä voidaan luoda tai luodaan suoraan Azure Marketplacesta. Voit luoda yhdistimen Marketplace-sivulla seuraavasti:  

1. Valitse Azure startboard **Marketplacesta**.
2. Etsi "SQL-yhdistin", napsauttamalla sitä ja valitse **Luo**.
3. Kirjoita nimi, sovellus-palvelun suunnittelu ja muita ominaisuuksia.
4. Kirjoita seuraavat pakettiasetukset:

    Nimi | Pakollinen |  Kuvaus
--- | --- | ---
Palvelimen nimi | Kyllä | Kirjoita SQL Server-nimi. Kirjoita esimerkiksi *SQL Server tai sqlexpress* tai *SQLserver.mydomain.com*.
Port (portti) | Ei | Oletusarvo on 1433.
Käyttäjänimi | Kyllä | Kirjoita käyttäjänimi, jolla voit kirjautua SQL Serveriin. Jos yhteyden paikallisen SQL Serveriä, kirjoita SQL-todennuksen tunnistetiedot.
Salasana | Kyllä | Kirjoita käyttäjänimen ja salasanan.
Tietokannan nimi | Kyllä | Kirjoita yrität muodostaa yhteyden tietokantaan. Voit esimerkiksi kirjoittaa *asiakkaiden* tai *dbo/tilaukset*.
Paikallisen | Kyllä | Oletusarvo on EPÄTOSI. Kirjoita EPÄTOSI, jos yhteyden muodostaminen Azure SQL-tietokantaan. Kirjoita TOSI, jos yhteyden muodostaminen paikalliseen SQL Serveriä.
Palvelun Bus yhteysmerkkijonon | Ei | Jos olet muodostamassa yhteyttä paikalliseen, kirjoita palvelun Bus välitys yhteysmerkkijono.<br/><br/>[Hybrid yhteyden hallinnan avulla](app-service-logic-hybrid-connection-manager.md)<br/>[Palvelun Bus hinnat](https://azure.microsoft.com/pricing/details/service-bus/)
Kumppanin palvelimen nimi | Ei | Jos ensisijainen palvelin ei ole käytettävissä, voit kirjoittaa kumppanin palvelimen vaihtoehtoinen tai varmuuskopio-palvelimeksi.
Taulukot | Ei | Luettelo tietokannan taulukot, jotka voidaan päivittää yhdistin. Kirjoita esimerkiksi *OrdersTable* tai *EmployeeTable*. Jos ole taulukoita on määritetty, kaikki taulukot voidaan. Voit käyttää tätä yhdistimen toiminnon tarvitaan kelvollinen taulukot ja/tai tallennettujen toimintosarjojen.
Tallennettujen toimintosarjojen | Ei | Kirjoita aiemmin tallennettu toimintosarja, joka voidaan kutsua yhdistin. Kirjoita esimerkiksi *sp_IsEmployeeEligible* tai *sp_CalculateOrderDiscount*. Voit käyttää tätä yhdistimen toiminnon tarvitaan kelvollinen taulukot ja/tai tallennettujen toimintosarjojen.
Käytettävissä olevat tietokyselyn | Käynnistimen tuki | SQL-lause, onko saatavilla kyselyt SQL Server-tietokanta-taulukon tietoja. Tämä tulee palauttaa numeerinen arvo, joka edustaa käytettävissä olevien tietojen rivien määrä. Esimerkki: Valitse määrä(*) table_name.
Kyselyn tietojen kysely | Käynnistimen tuki | SQL-lausekkeen lisääminen SQL Server-tietokannan taulukkoon. Voit kirjoittaa jokin muu luku SQL-lauseita puolipisteellä eroteltuina. Tämä lause on suorittaa muulla tavoin ja vahvistetun vain, kun tiedot on tallennettu turvallisesti logiikan-sovelluksessa. Esimerkki: Valitse *table_name; Poista-table_name. <br/>**Note**<br/>Sinun on määritettävä kyselyn ilmaisee, että vältetään äärettömän silmukan poistamalla, siirtäminen tai päivittää valitut tiedot, varmista, että samat tiedot kohdistaa ei ole pyyntöjä uudelleen.

5. Kun olet valmis, Pakettiasetukset näyttää seuraavankaltaiselta:  
![][1]  

6. Valitse **Luo**. 


## <a name="use-the-connector-as-a-trigger"></a>Käyttää yhdistimen käynnistin
Katsotaan yksinkertainen logiikan-sovellusta, joka tekee kyselyn SQL-taulukon tiedot ja lisää tiedot toisen taulukon tiedot päivitetään.

Käyttää SQL-yhdistin käynnistin, kirjoita **Käytettävissä tietokyselyn** ja **Kysely tietokyselyn** arvot. **Tietoja käytettävissä kysely** suoritetaan aikataulussa, anna ja määrittää kaikki tiedot on käytettävissä. Koska tämä kysely palauttaa vain skalaarinen luvuksi, se voidaan virittää ja optimoitu usein käytetyt suorittamisen.

**Kyselyn tietojen kysely** suoritetaan vain, kun käytettävissä kyselyn ilmaisee, että tiedot ovat käytettävissä. Tämä lause suorittaa tapahtumasta ja on vain vahvistettu kun poimitun tiedot tallennetaan kuuluminen työnkulun. On tärkeää välttää puretaan infinitely samat tiedot uudelleen. Tapahtumien laatu suorittaminen voidaan poistaa tai päivittää sen varmistamiseksi, se ei ole kerättyjä tietoja kyselyitä seuraavan kerran.

> [AZURE.NOTE] Tietosuojatiedoissa palauttama rakenteen tunnistaa sitten yhdistimen käytettävissä olevat ominaisuudet. Kaikkien sarakkeiden nimen on oltava.

#### <a name="data-available-query-example"></a>Esimerkki tietojen käytettävissä kysely

    SELECT COUNT(*) FROM [Order] WHERE OrderStatus = 'ProcessedForCollection'

#### <a name="poll-data-query-example"></a>Kyselyn tietojen kysely esimerkin

    SELECT *, GetData() as 'PollTime' FROM [Order]
        WHERE OrderStatus = 'ProcessedForCollection'
        ORDER BY Id DESC;
    UPDATE [Order] SET OrderStatus = 'ProcessedForFrontDesk'
        WHERE Id =
        (SELECT Id FROM [Order] WHERE OrderStatus = 'ProcessedForCollection' ORDER BY Id DESC)

### <a name="add-the-trigger"></a>Lisää käynnistin
1. Luomisessa ja muokkaamisessa logiikan-sovellus, valitse luomasi SQL-yhdistin käynnistintä. Tämä näyttää käytettävissä käynnistimet: **Kyselyn tiedot (JSON)** ja **Kyselyn tiedot (XML)**:  
![][5]

2. Valitse **Kyselyn tiedot (JSON)** käynnistin, kirjoita korkojakso ja valitse ✓:  
![][6]

3. Käynnistin näkyy nyt määritetty logiikan-sovelluksessa. Lähtö tai lähdöt käynnistimen näkyvät ja voidaan käyttää seuraavien toimintojen syötteiden:  
![][7]

## <a name="use-the-connector-as-an-action"></a>Käyttää yhdistimen toiminnon
Microsoftin yksinkertainen logiikan app skenaario, joka tekee kyselyn SQL-taulukon tiedot Lisää tiedot toisen taulukon ja päivittää tiedot.

Jos haluat käyttää SQL-yhdistimen toiminnon, taulukoita ja/tai tallennetut toimintosarjat kirjoittamasi luotaessa SQL-yhdistin nimi:

1. Kun käynnistin (tai valitse 'Suorita tämä logiikka manuaalisesti'), lisää SQL-yhdistin luomasi valikoimasta. Valitse jokin Lisää Toiminnot, kuten *Lisätä yhdeksi TempEmployeeDetails (JSON)*:  
![][8]

2. Kirjoita syöttöarvojen lisätään ja valitse ✓ tietueen:  
![][9]

3. Valikoimasta Valitse luomasi saman SQL-yhdistin. Valitse toiminnon samassa taulukossa, kuten *Päivityksen EmployeeDetails*päivitys-toiminto:  
![][11]

4. Olevien syöttöarvojen syöttösolun päivitystoimintoa ja valitse ✓:  
![][12]

Voit testata logiikan sovelluksen lisäämällä taulukon, joka on kohdistaa pyyntöjä uusi tietue.

## <a name="what-you-can-and-cannot-do"></a>Mitä voit ja mitä et voi tehdä

SQL-kysely | Tuettu | Ei tueta
--- | --- | ---
Jos lause | <ul><li>Operaattorit: Ja tai -=, <>, <, < =, >, > = ja, kuten</li><li>Sub useita ehtoja voidaan yhdistää mukaan "(" ja")"</li><li>Merkkijono literaalien, Datetime (Kehystetty-heittomerkki), numerot (pitäisi sisältää vain numeerista merkkiä)</li><li>On ehdottoman oltava binaarinen lauseke-muodossa, kuten ((operand operator operand) ja/tai (operandi operaattori operandi)) *</li></ul> | <ul><li>Operaattorit: Välillä, IN</li><li>Kaikki sisäiset Funktiot, kuten kokoelmalla, MAX() NOW(), POWER() ja niin edelleen</li><li>Matemaattisia operaattoreita, kuten *, -, + ja niin edelleen</li><li>Merkkijonojen katkaisemiset käyttämällä +.</li><li>Kaikki liitosten</li><li>ON tyhjä ja ei ole Null</li><li>Numerot ja merkit ole lukuarvo, kuten heksadesimaaliluvun numerot</li></ul>
(Valitse kyselyn) kentät | <ul><li>Kelvollinen sarakenimet erotettuina. Taulukon nimi-etuliitteitä ei ole sallittu (yhdistimen toimii yhden taulukon kerrallaan).</li><li>Nimet on sisällettävä ' ["ja"] "</li></ul> | <ul><li>Avainsanoja, kuten ylä-, DISTINCT, ja niin edelleen</li><li>Tunnuksen määritys, kuten katu, kaupunki + Zip-AS-osoite</li><li>Kaikki sisäiset Funktiot, kuten kokoelmalla, MAX() NOW(), POWER() ja niin edelleen</li><li>Matemaattisia operaattoreita, kuten *, -, + ja niin edelleen</li><li>Merkkijonojen katkaisemiset käyttämällä +</li></ul>

#### <a name="tips"></a>Vihjeitä

- Tarkennetut kyselyt Ehdota tallennetun toimintosarjan ja Suorita tallennettu tavalla API suoritus luominen.
- Sisemmän kyselyjä käytettäessä käyttää niitä tallennettujen toimintosarjojen kuluessa.
- Jos luot useita ehtoja, voit käyttää 'Ja' ja 'Tai' operaattorit.

## <a name="hybrid-configuration-optional"></a>Hybrid määrittäminen (valinnainen)

> [AZURE.NOTE] Tämä vaihe on pakollinen vain, jos käytössäsi on SQL Server paikallisen palomuurin takana.

Sovelluksen palvelu käyttää Hybrid hallintatoiminnon muodostaa suojatusti paikalliseen järjestelmään. Jos olet yhdistimen käyttää paikallisen SQL Serveriä, Hybrid Yhteyksienhallinnan ei tarvita.

> [AZURE.NOTE] Jos haluat aloittaa Azure logiikan sovellukset ennen rekisteröimässä Azure-tili, siirry [Yritä logiikan sovellus](https://tryappservice.azure.com/?appservice=logic), jossa lyhytkestoinen starter logiikan-sovelluksen heti voit luoda sovelluksen-palvelussa. Ei ole pakollinen; luottokortit ei ole sitoumukset.

Katso [Hybrid Yhteyksienhallinnan](app-service-logic-hybrid-connection-manager.md).


## <a name="do-more-with-your-connector"></a>Älä lisää ja sitten yhdistimen
Yhdistimen on luotu, voit lisätä business työnkulkuun logiikan sovellusta käytettäessä. Katso [mitä logiikan sovelluksia?](app-service-logic-what-are-logic-apps.md).

Tarkastele Swagger REST API-viittaus [yhdistimien](http://go.microsoft.com/fwlink/p/?LinkId=529766)ja API sovellusten viittaus.

Voit myös tarkastella suorituskyvyn Tilasto- ja tietoturva yhdistin. Katso [hallinta ja valvonta valmiin API-sovellusten ja yhdistimet](app-service-logic-monitor-your-connectors.md).


<!--Image references-->
[1]: ./media/app-service-logic-connector-sql/Create.png
[5]: ./media/app-service-logic-connector-sql/LogicApp1.png
[6]: ./media/app-service-logic-connector-sql/LogicApp2.png
[7]: ./media/app-service-logic-connector-sql/LogicApp3.png
[8]: ./media/app-service-logic-connector-sql/LogicApp4.png
[9]: ./media/app-service-logic-connector-sql/LogicApp5.png
[10]: ./media/app-service-logic-connector-sql/LogicApp6.png
[11]: ./media/app-service-logic-connector-sql/LogicApp7.png
[12]: ./media/app-service-logic-connector-sql/LogicApp8.png

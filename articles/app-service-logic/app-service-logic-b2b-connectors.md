<properties 
    pageTitle="Business Business yhdistimien ja API sovellusten logiikan sovelluksissa | Microsoft Azure" 
    description="Lue, miten voit luoda ja määrittää Muokkaa, EDIFACT tai AS2 TPM yhdistimiä. microservices arkkitehtuuri" 
    services="logic-apps" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="anneta" 
    editor=""/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="mandia"/> 

# <a name="business-to-business-connectors-and-api-apps"></a>Business Business yhdistimien ja API-sovellukset

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]

Logiikan sovellukset on monta BizTalk API sovellukset, jotka ovat tärkeä integrointi ympäristöt. API nämä sovellukset perustuvat käsitteitä ja -työkalujen avulla voidaan BizTalkin, mutta ovat nyt käytettävissä osana logiikan sovellukset. 

Yritysten (B2B) API-sovellukset ovat yhtä luokkaa API nämä sovellukset. Käytä B2B API nämä sovellukset, voit helposti Lisää kumppanien, Luo sopimuksia ja tee kaikki tavalliseen tapaan kuin paikallisen käyttämällä Muokkaa AS2 ja EDIFACT.  

Nämä B2B API sovellukset tarjoavat "Käynnistimen" ja "Toiminto" ominaisuuksia. Käynnistimen käynnistää tietyn tapahtuman, kuten X12 saapumisen perusteella uuden esiintymän viestin kumppanilta. Toiminto on tulos, kuten saatuaan X12 viestin ja lähettää viestin käyttämällä AS2.


## <a name="what-is-a-business-to-business-connector-or-api-apps"></a>Mikä on Business Business yhdistimen tai API sovelluksia
Yritysten (B2B)-toiminto on olemassa olevia, valmiita, joka mahdollistaa eri yritykset, rajapintojen, sovellusten ja niin edelleen vaihtamaan AS2, Muokkaa ja EDIFACT API-sovellukset. 

B2B API-sovellukset ovat seuraavat: 

Yhdistimen tai API-sovellukset | Kuvaus
--- | ---
BizTalk Kaupankäyntipäivä kumppanin hallinta | API-sovellus, joka luo yhteyksiä yritysten (B2B) kumppanit ja sopimusten. Nämä suhteet käyttämiseen AS2, EDIFACT ja X12 protokolla.<br/><br/>TPM API-sovellus on kantaluku vaatimus AS2 yhdistimen, ja X12 tai EDIFACT API sovellukset. 
AS2 yhdistin | Yhdistin, joka vastaanottaa ja lähettää viestiä, joissa AS2 transport. Yhdistimen siirtotapahtumat tietoja suojatusti ja luotettavasti Internetin välityksellä.
BizTalk EDIFACT | API-sovellus, joka vastaanottaa ja lähettää viestiä, joissa EDIFACT. EDIFACT käytetään laajasti yli teollisuuden ja sitä kutsutaan myös yleisesti UN/EDIFACT (Yhdistyneiden Kansakuntien/sähköiset Data Interchange, hallinta ja Commerce Transport).
BizTalk X12 | API-sovellusta, joka vastaanottaa ja lähettää viestiä, joissa X12 protokolla. X12 käytetään yleisesti koko teollisuuden ja sitä kutsutaan myös yleisesti ASC X 12 (millä standardeja komitean X12). 


Nämä API-sovellukset voivat tehdä eri Muokkaa messaging tehtävät. Esimerkiksi AS2 Connectorin avulla voit turvallisesti vastaanottaa ja lähettää viestejä erityyppisiä (Muokkaa, XML, Normaali-tiedoston, ja niin edelleen) asiakkaalle jako yrityksessäsi, kuten henkilöstöresurssien tai kuka tahansa, joka käyttää AS2. 

Voit luoda niin monta API sovellukset kuin haluat ja luo ne helposti. Voit myös käyttää useita skenaarioita tai työnkulkujen yksittäinen API-sovellus.

Voit tehdä tämän kirjoittamatta lainkaan koodia. Aloitetaanpa. 


## <a name="requirements-to-get-started"></a>Aloita vaatimukset
Kun luot B2B API sovellukset, on joitakin tarvittavat resurssit. Nämä kohteet on luotava itse, ennen kuin niitä voidaan käyttää muita API-sovelluksista. Nämä vaatimukset ovat seuraavat: 

Vaatimus | Kuvaus
--- | ---
Azure SQL-tietokanta | Tallentaa B2B-kohteita, kuten kumppanit, mallit, varmenteet ja agreeements. Lisätietoja kunkin sovelluksen B2B API edellyttää Azure SQL-tietokanta. <br/><br/>**Huomautus** Kopioi yhteysmerkkijono tämä tietokanta.<br/><br/>[Azure SQL-tietokannan luominen](../sql-database/sql-database-get-started.md)
Azure-Blob-säiliö säilö | Tallentaa viestin ominaisuudet, kun AS2 arkistointi on käytössä. Jos et tarvitse AS2 viestin arkistointi-tallennustilan säilö ei tarvita. <br/><br/>**Huomautus** Jos otat arkistointi, kopioi yhteysmerkkijono tämän Blob-objektien tallennustilaan.<br/><br/>[Tietoja Azure-tallennustilan](../storage/storage-create-storage-account.md)
Palvelun Bus Namespace ja arvonsa avain | Tallentaa X12 ja EDIFACT jonottaminen tiedot. Jos et tarvitse jonottaminen-palvelun Bus nimitila ei tarvita.<br/><br/>**Huomautus** Jos otat jonottaminen, kopioi seuraavat arvot.<br/><br/>[Palvelun Bus-Namespace luominen](http://msdn.microsoft.com/library/azure/hh690931.aspx)
TPM esiintymä | AS2 yhdistimen ja X12 tai EDIFACT API sovelluksen luomiseen tarvitaan BizTalk myynti kumppanin Management (TPM)-esiintymässä. Kun luot TPM API-sovelluksen, luot TPM esiintymä. <br/><br/>**Huomautus** Tietää TPM API sovelluksen nimeä. 


## <a name="create-the-api-apps"></a>API-sovellusten luominen
B2B API-sovellukset voi luoda Azure-portaalissa tai käyttämällä REST API. 


### <a name="create-the-api-apps-using-rest-apis"></a>REST API käyttämällä API-sovellusten luominen
[REST API käyttämisestä ohjeissa.](http://go.microsoft.com/fwlink/p/?LinkId=529766)


### <a name="create-the-b2b-api-apps-in-the-azure-portal"></a>Luo B2B API sovellukset Azure-portaalissa
Azure-portaalissa voit luoda B2B API sovellusten logiikan sovellusten Web Apps-sovellusten ja Mobile-sovellusten luomiseen. Vaihtoehtoisesti voit luoda sen avulla oma sivu. Molempiin suuntiin ovat helposti, joten se on riippuvainen tarpeiden tai asetukset. Joillakin käyttäjillä haluavat luoda B2B API sovellukset kaikki tietyn ominaisuuksiensa ensin. Luo logiikan sovellukset/Web Apps/Mobile-sovellusten ja luomasi B2B API-sovellukset.  

Seuraavia ohjeita voit luoda B2B API-sovelluksista käsin API sovellukset-sivu.


#### <a name="create-the-biztalk-trading-partner-management-tpm-api-apps"></a>Luo BizTalk myynti kumppanin Management (TPM) API-sovellukset

> [AZURE.NOTE] AS2 yhdistimen ja X12 tai EDIFACT API sovelluksen luomiseen tarvitaan BizTalk myynti kumppanin Management (TPM)-esiintymässä. Kun luot TPM API-sovelluksen, luot TPM esiintymä.

Seuraavat vaiheet luominen TPM esiintymä:

1. Valitse Azure portaalissa Startboard (kotisivun) **Marketplacesta**. **API-sovellukset** on lueteltu kaikki olemassa olevat API-sovellukset ja yhdistimet. Voit myös tiettyjä B2B API-sovellusten **haku** .
2. Valitse **BizTalk myynti kumppanin hallinta**. Valitse **Luo**uusi sivu. 
3. Määritä ominaisuudet: 

    Ominaisuus | Kuvaus
--- | ---
Nimi | Kirjoita kaikki TPM-esiintymän nimi. Esimerkiksi voit nimetä sen *AccountsPayableTPM*.
Pakettiasetukset | Kirjoita ADO.NET- **Tietokannan yhteysmerkkijono** luomasi Azure SQL-tietokantaan. <br/><br/>Kun kopioit yhteysmerkkijonon, salasana ei lisätä yhteysmerkkijono. Muista salasana yhteysmerkkijonon.
Sovelluksen palvelusopimus | Näyttää maksusuunnitelma. Voit muuttaa sitä, jos tarvitset enemmän tai vähemmän resursseja.
Hinnoittelua taso | Vain luku-ominaisuus, joka näyttää Azure-tilauksen piiriin kuuluvien hinnoittelu luokka. 
Resurssiryhmä | Luo uusi tai aiemmin luodun ryhmän käyttäminen. Kaikki API-sovellukset ja yhdistimet logiikan sovellukset ja Web Apps-sovellusten ja Mobile-sovellusten on oltava sama resurssiryhmä. <br/><br/>[Resurssiryhmien käyttäminen](../azure-resource-manager/resource-group-overview.md) kerrotaan tätä ominaisuutta. 
Tilauksen | Vain luku-ominaisuus, joka näyttää nykyisen tilauksesi.
Sijainti | Maantieteellisen sijainnin, joka isännöi Azure palvelua. 
Startboard lisääminen | Valitse tämä B2B API-sovelluksen lisääminen oman paapuurin (kotisivun).

4. Valitse **Luo**. 

Kun TPM API APP (TPM esiintymän) on luotu, voit luoda AS2 yhdistimen ja/tai X12 tai EDIFACT API sovelluksia. 


#### <a name="create-the-as2-connector"></a>Luo AS2-yhdistin

1. Valitse Azure portaalissa Startboard (kotisivun) **Marketplacesta**. **API-sovellukset** on lueteltu kaikki olemassa olevat API-sovellukset ja yhdistimet. Voit myös tiettyjä B2B API-sovellusten **haku** .
2. Valitse **AS2 yhdistin**. Valitse **Luo**uusi sivu. 
3. Määritä ominaisuudet: 

    Ominaisuus | Kuvaus
--- | ---
Nimi | Kirjoita kaikki AS2 yhdistimen nimi. Esimerkiksi voit nimetä sen *AS2Connector*.
Pakettiasetukset | Kirjoita tiettyjä asetuksia, API-sovellukseen, kuten TPM esiintymän nimi. <br/><br/>Katso [Lisää AS2 Pakettiasetukset](#AddAS2Conn) tämän ohjeaiheen ominaisuudet. 
Sovelluksen palvelusopimus | Näyttää maksusuunnitelma. Voit muuttaa sitä, jos tarvitset enemmän tai vähemmän resursseja.
Hinnoittelua taso | Vain luku-ominaisuus, joka näyttää Azure-tilauksen piiriin kuuluvien hinnoittelu luokka. 
Resurssiryhmä | Luo uusi tai aiemmin luodun ryhmän käyttäminen. [Resurssiryhmien käyttäminen](../azure-resource-manager/resource-group-overview.md) kerrotaan tätä ominaisuutta. 
Tilauksen | Vain luku-ominaisuus, joka näyttää nykyisen tilauksesi.
Sijainti | Maantieteellisen sijainnin, joka isännöi Azure palvelua. 
Startboard lisääminen | Valitse tämä B2B API-sovelluksen lisääminen oman paapuurin (kotisivun).

    **<a name="AddAS2Conn"></a>Yhdistimen AS2 Pakettiasetukset**

    Ominaisuus | Kuvaus
--- | --- 
Yhteysmerkkijonon. | Kirjoita ADO.NET-yhteysmerkkijonoa Azure SQL-tietokanta, jonka loit. Kun kopioit yhteysmerkkijonon, salasana ei lisätä yhteysmerkkijono. Muista salasana yhteysmerkkijonon ennen liittämistä.
Ota käyttöön saapuvien viestien arkistointi | Valinnainen. Ottaa tämän ominaisuuden arvoksi tallentaa saapuvan AS2 sanoman kumppanin vastaanotti viestin-ominaisuuksia. 
Azure-Blob Storage yhteysmerkkijonon  | Kirjoita yhteysmerkkijonoa Azure-Blob-säiliö säilöön luomasi. Kun arkistointi on käytössä, koodattu ja purettu viestit tallennetaan tämän tallennustilan säilö.
TPM esiintymänimi | Kirjoita nimi **BizTalk myynti kumppanin hallinta** aiemmin luotu API-sovellus. Kun luot AS2 yhdistimen, tämä yhdistin suorittaa vain AS2 sopimusten tähän TPM kuluessa.

4. Valitse **Luo**. 


#### <a name="create-the-x12-or-edifact-api-apps"></a>X12 tai EDIFACT API-sovellusten luominen

1. Valitse Azure portaalissa Startboard (kotisivun) **Marketplacesta**. **API-sovellukset** on lueteltu kaikki olemassa olevat API-sovellukset ja yhdistimet. Voit myös tiettyjä B2B API-sovellusten **haku** .
2. Valitse **BizTalk X 12** tai **BizTalk EDIFACT**. Valitse **Luo**uusi sivu. 
3. Määritä ominaisuudet: 

    Ominaisuus | Kuvaus
--- | ---
Nimi | Kirjoita kaikki B2B API-sovellus. Esimerkiksi voit nimetä sen *EDI850APIApp*.
Pakettiasetukset | Kirjoita tiettyjä asetuksia, API-sovellukseen, kuten TPM esiintymän nimi. <br/><br/>Tiettyjen ominaisuuksien tämän ohjeaiheen kohdassa [X12 tai EDIFACT Pakettiasetukset](#AddX12) . 
Sovelluksen palvelusopimus | Näyttää maksusuunnitelma. Voit muuttaa sitä, jos tarvitset enemmän tai vähemmän resursseja.
Hinnoittelua taso | Vain luku-ominaisuus, joka näyttää Azure-tilauksen piiriin kuuluvien hinnoittelu luokka. 
Resurssiryhmä | Luo uusi tai aiemmin luodun ryhmän käyttäminen. [Resurssiryhmien käyttäminen](../azure-resource-manager/resource-group-overview.md) kerrotaan tätä ominaisuutta. 
Tilauksen | Vain luku-ominaisuus, joka näyttää nykyisen tilauksesi.
Sijainti | Maantieteellisen sijainnin, joka isännöi Azure palvelua. 
Startboard lisääminen | Valitse tämä B2B API-sovelluksen lisääminen oman paapuurin (kotisivun).

    **<a name="AddX12"></a>X12 ja EDIFACT API sovellukset paketti asetukset**  

    Ominaisuus | Kuvaus
--- | --- 
Yhteysmerkkijonon. | Kirjoita ADO.NET-yhteysmerkkijonoa Azure SQL-tietokanta, jonka loit. Kun kopioit yhteysmerkkijonon, salasana ei lisätä yhteysmerkkijono. Muista salasana yhteysmerkkijonon ennen liittämistä.
Palvelun Bus Namespace | Kirjoita luomasi palvelun Bus nimitilan. Pakollinen vain kun jonottaminen on käytössä. 
Jaettu palvelun Bus Namespace-pikanäppäin nimi | Kirjoita palvelun Bus nimitilan luomasi pikanäppäin. Pakollinen vain kun jonottaminen on käytössä. 
Jaettu palvelun Bus Namespace-pikanäppäin arvo | Kirjoita palvelun Bus nimitilan luomasi pikanäppäin-arvo. Pakollinen vain kun jonottaminen on käytössä. 
TPM esiintymänimi | Kirjoita nimi **BizTalk myynti kumppanin hallinta** aiemmin luotu API-sovellus. Kun luot X12 tai EDIFACT API sovelluksia, API sovelluksen suorittaa vain X12/EDFIACT sopimusten tähän TPM kuluessa.

4. Valitse **Luo**. 


## <a name="add-your-partners-agreements-certificates-and-schemas"></a>Lisää kumppanien, toimeenpano, varmenteet ja rakenteet 
Avaa TPM API sovelluksen Azure-portaalissa. Lisää kumppanien, toimeenpano, varmenteet ja rakenteet **osat** -kohdassa. 

Voit myös lisätä sopimusten AS2 yhdysviivat X12 API-sovellukset ja EDIFACT API sovellukset. 


## <a name="monitor-your-api-apps"></a>API-sovellusten valvonta
Avaa TPM API sovelluksen Azure-portaalissa. Valitse **Toiminnot** -osassa voit tarkastella eri hallintatoiminnot. Voit tehdä esimerkiksi seuraavia toimia:

- Näytä tiedottavat ja virheen tapahtumat
- Näytä työntekijä-prosessi (w3wp) muisti käyttö- ja viestiketjun määrä
- Näytä sovellus ja WWW-palvelimen lokit

Lisää osoitteessa [näytön logiikan sovelluksia](app-service-logic-monitor-your-logic-apps.md).


## <a name="add-the-api-apps-to-your-application"></a>Sovelluksen API-sovellusten lisääminen 
Microsoft Azure-sovelluksen palvelun paljastaa toiseen sovellukseen tyypit, jotka nämä B2B API-sovelluksen avulla. Voit luoda uuden tai lisätä aiemmin B2B API sovelluksia logiikan sovellukset, Mobile-sovellusten ja Web Apps-sovelluksista. 

-Sovelluksen sisällä valitsemalla B2B API sovellukset valikoimasta automaattisesti lisää sen sovelluksen.  

> [AZURE.IMPORTANT] Voit lisätä yhdistimiä ja API-sovellukset, jotka on aiemmin luotu, Luo saman resurssiryhmän logiikan sovellukset, Mobile-sovellusten tai verkkosovelluksissa. 

Seuraavat vaiheet lisääminen B2B API sovellukset logiikan sovellukset, Mobile-sovellusten tai Web Apps: 

1. Siirry Azure portal Startboard (kotisivun) **Marketplace**ja Etsi logiikan, matkapuhelin tai verkkosovelluksissa. 

    Jos olet luomassa uutta sovellusta, Etsi logiikan sovellukset, Mobile-sovellusten tai verkkosovelluksissa. Valitse sovellus ja valitse **Luo**uusi sivu. [Logiikan-sovelluksen luominen](app-service-logic-create-a-logic-app.md) on esitetty vaiheet. 

2. Avaa sovellus ja valitse **Käynnistimet ja toiminnot**. 

3. **Valikoima**Valitse B2B API-sovellus, joka lisää sen automaattisesti sovelluksen. Voit myös luoda uuden B2B API sovelluksen.

    > [AZURE.IMPORTANT] AS2 yhdysviivaa ja X12, EDIFACT API-sovellukset edellyttävät TPM esiintymä. Jos olet luomassa uudessa B2B API-sovelluksessa TPM API-sovelluksen luominen ensin ja luo sitten AS2 connector X12 API-sovelluksen tai EDIFACT API-sovellus. 

4. Valitse **OK** , jos haluat tallentaa tekemäsi muutokset. 

>[AZURE.NOTE] Aloita Azure logiikan sovellukset ennen Azure tilin, [Yritä logiikan sovellusten](https://tryappservice.azure.com/?appservice=logic)rekisteröimistä. Voit luoda heti lyhytkestoinen starter-logiikkaa-sovelluksen. Ei ole pakollinen; luottokortit ei ole sitoumukset.

## <a name="more-b2b-resources"></a>Lisää B2B resursseja

[B2B prosessin luominen](app-service-logic-create-a-b2b-process.md)<br/>
[Kaupan kumppanin sopimuksen luominen](app-service-logic-create-a-trading-partner-agreement.md)<br/>
[Mitä yhdistimien ja BizTalk API sovelluksia](app-service-logic-what-are-biztalk-api-apps.md)


## <a name="read-about-logic-apps-and-web-apps"></a>Lisätietoja logiikan sovellukset ja Web Apps-sovelluksista
[Mitkä ovat logiikan sovelluksia?](app-service-logic-what-are-logic-apps.md)<br/>
[Sivustojen ja verkkosovelluksissa Azure sovelluksen-palvelussa](../app-service-web/app-service-web-overview.md)


## <a name="more-connectors"></a>Lisää yhdistimet

[Yhdistimien ja API sovellusluettelosta](app-service-logic-connectors-list.md)<br/><br/>
[Mitä yhdistimien ja BizTalk API sovelluksia](app-service-logic-what-are-biztalk-api-apps.md) 

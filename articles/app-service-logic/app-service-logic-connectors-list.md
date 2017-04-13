<properties
    pageTitle="Käytettävissä olevat yhdysviivat ja API-sovellusten luettelo | Sovelluksen Microsoft Azure-palvelu"
    description="Lue lisätietoja Azure App Service API-sovellusten ja yhdistimet"
    services="logic-apps"
    documentationCenter=""
    authors="MandiOhlinger"
    manager="erikre"
    editor="cgronlun"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/01/2016"
    ms.author="mandia"/>


# <a name="list-of-connectors-and-api-apps-to-use-in-your-logic-apps"></a>Yhdistimien ja API sovellusten logiikan-sovellusten käyttäminen
>[AZURE.NOTE] Tässä versiossa on artikkelissa koskee logiikan sovellusten 2014 – 12-01 – esikatselu rakenteen versio. Katso logiikan sovellusten Yleiset käytettävyys (GA)-version [Uudet yhdysviivat-luettelosta](../connectors/apis-list.md).

Lue lisää kaikki käytettävissä olevat yhdysviivat ja API-sovellukset, jotka on luotu Microsoft käyttää logiikan-sovelluksista.

Hinnoittelutiedot ja mitä sisältyy palvelun kunkin tason luettelo-kohdassa [Azure App palvelun hinnat](https://azure.microsoft.com/pricing/details/app-service/).

> [AZURE.NOTE] Aloita logiikan sovellukset ennen rekisteröimässä Azure-tili, siirry [Yritä logiikan-sovellukseen](https://tryappservice.azure.com/?appservice=logic). Voit luoda lyhytkestoinen starter logiikan-sovelluksen heti sovelluksen-palvelussa. Ei ole pakollinen; luottokortit ei ole sitoumukset.

## <a name="core-connectors"></a>Core yhdistimet
Seuraavassa taulukossa on lueteltu kaikki käytettävissä olevat yhdysviivat ja Microsoft luonut API sovellukset, jotka ovat käytettävissä Core yhdistimet:

Nimi | Kuvaus
--- | ---
[Bing Translator](https://azure.microsoft.com/marketplace/partners/bing/microsofttranslator/) | Tekstin kääntäminen toiselle kielelle Bing avulla.
[HTTP](app-service-logic-connector-http.md) | HTTP-kuuntelutoiminnon avautuu päätepiste, joka toimii HTTP-palvelin ja seuraa HTTP tai HTTPS pyynnöt. HTTP-toiminto ei edellytä API-sovelluksen ja tuetaan grafiikkatiedostomuotoja logiikan-sovelluksista.
[Liukuma](app-service-logic-connector-slack.md) | Muodosta yhteys liukuma ja liukuma kanavien viestejä.


## <a name="enterprise-integration-connectors"></a>Yrityksen integrointi yhdistimet
Seuraavassa taulukossa on lueteltu kaikki käytettävissä olevat yhdysviivat ja API-sovellukset, jotka on luotu Microsoft yrityksen integrointi yhdistimet käytettävissä:

Nimi  | Kuvaus
------------- | -------------
[BizTalk säännöt](app-service-logic-use-biztalk-rules.md) | BizTalk sääntöjen avulla voit määrittää ja hallita organisaation liiketoimintalogiikan. Yrityksen käytäntöjä voi päivittää ilman sijaan tai ilman mallirakenteeseen siihen liittyvät sovellukset.
[BizTalk XPath Extractor](app-service-logic-xpath-extract.md) | Etsii ja poimii tietojen perusteella voit valita XPath XML-sisällön.
[DB2-yhdistin](app-service-logic-connector-db2.md) | Muodostaa yhteyden, IBM DB2-tietokantaan paikallisen ja Azure virtuaalikoneen käynnissä Windows-käyttöjärjestelmässä. Voit yhdistää verkko-Ohjelmointirajapinnan ja OData-Ohjelmointirajapinnan Informix Structured Query Language-komentoja. <br/><br/>Ei ole Käynnistimet. Toiminnot ovat Valitse taulukko, Lisää, päivitys, poistaminen ja mukautetun lausekkeen<br/><br/>Tämä yhdistin myös Microsoft Client for DRDA muodostaa yhteyden Informix-palvelimeen TCP/IP-verkon kautta.
[Tiedoston](app-service-logic-connector-file.md) | Tämä Connectorin avulla voit muodostaa yhteyden paikallisen tiedostojärjestelmän- tai verkko- ja eri tiedostosta tehtäviä, kuten lataamisesta, poistaminen, luettelo-tiedostot ja lisää.
[Informix](app-service-logic-connector-informix.md) | Yhdistää IBM Informix-tietokantaan, paikallisen ja Azure virtual-tietokoneessa, jossa on Windows-käyttöjärjestelmä. Voit yhdistää verkko-Ohjelmointirajapinnan ja OData-Ohjelmointirajapinnan Informix Structured Query Language-komentoja.<br/><br/>Ei ole Käynnistimet. Toiminnot ovat, valitse taulukko, Lisää, päivitys, poistaminen ja mukautetun lausekkeen.<br/><br/>Kun käytät paikallisen, VPN- tai Azure ExpressRoute voidaan. Tämä yhdistin myös Microsoft-Client for DRDA muodostaa yhteyden Informix-palvelimeen TCP/IP-verkon kautta.
[Microsoft SQL Server](app-service-logic-connector-sql.md) | Voit muodostaa yhteyden paikallisen SQL Server tai Azure SQL-tietokanta. Voit luoda, päivittää, Hae ja poistaa SQL-tietokannan taulukkoon.
MQ | Muodostaa IBM WebSphere MQ palvelimen versio 8: n paikallisen ja Azure virtual-tietokoneessa, jossa on Windows-käyttöjärjestelmä. Kun käytät paikallisen, VPN- tai Azure ExpressRoute voidaan. Yhdistimen myös Microsoft Client MQ varten.<br/><br/>Ei ole Käynnistimet. Mitään toimia.<br/><br/>**Huomautus** Tällä hetkellä ei voi käyttää logiikan sovellukset.

## <a name="connectors-as-triggers"></a>Yhdistimien kuin Käynnistimet
Useita yhdistimien avulla käynnistimien logiikan sovellukset. Nämä käynnistimet on kahdenlaisia:

1. Kyselyn käynnistimien: Nämä käynnistimien äänestyksen järjestäminen palveluun osoitteessa määritettyinä tarkistavan uudet tiedot. Kun uudet tiedot ovat käytettävissä, logiikan sovelluksen uuden esiintymän suoritetaan syötteeksi tiedoilla. Voit estää samat tiedot on käytetty useita kertoja käynnistin saattaa SIIVOA lukea ja logiikka sovelluksen välitetään tiedot. Esimerkkejä näiden yhdistimet ovat tiedoston ja SQL Azure-tallennustilan.
2. Push-käynnistimet: Nämä käynnistimien kuuntelevat päätepisteen tiedot tai tapahtuman suoritetaan. Käynnistää sitten logiikan sovelluksen uuden esiintymän. Esimerkkejä näiden yhdistimet ovat HTTP-kuuntelutoiminnon- ja Twitter.

## <a name="connectors-as-actions"></a>Yhdistimien toimenpiteitä
Yhdistimien voi käyttää myös logiikan sovelluksen toimintoja. Toiminnot on hyötyä logiikan sovelluksen, voit käyttää sitä sitten suorittamisen tietojen hakeminen. Voit joutua esimerkiksi tietojen hakemiseen SQL-tietokannasta, saat lisätietoja asiakkaan tilauksen käsiteltäessä. Tai joudut ehkä kirjoittaa, Päivitä tai poista kohde tiedot. Voit tehdä yhdistimet myöntämä toimintojen käyttäminen. Toiminnot Yhdistä toimintojen API-sovelluksissa (määritelty Swagger niiden metatietojen mukaan).

## <a name="create-your-own-connectors-and-api-apps"></a>Luo omia yhdistimien ja API-sovellukset
[Yhdistimien ja API-sovellusten viiteopas](http://aka.ms/appservicesconnectorreference)  
[Azure App Service API app Käynnistimet](../app-service-api/app-service-api-dotnet-triggers.md)  
[Logiikan App viittaus](https://msdn.microsoft.com/library/azure/dn948510.aspx)

## <a name="more-on-connectors-and-api-apps"></a>Lisätietoja yhdistimien ja API-sovellukset
[Mitä yhdistimien ja BizTalk API sovelluksia](app-service-logic-what-are-biztalk-api-apps.md)  
[Hybrid Yhteyksienhallinnan käyttäminen Azure sovelluksen-palvelu](app-service-logic-hybrid-connection-manager.md)  
[Hallinta ja valvonta valmiin API-sovellusten ja yhdistimet](app-service-logic-monitor-your-connectors.md)

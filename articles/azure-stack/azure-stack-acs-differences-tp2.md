
<properties
    pageTitle="Azure yhdenmukaisia tallennustilan: erot ja huomioon otettavia seikkoja | Microsoft Azure"
    description="Tietoja Azuren tallennustilaan ja muut Azure yhdenmukaisia tallennustilan käyttöönotto-näkökohdat erot."
    services="azure-stack"
    documentationCenter=""
    authors="MChadalapaka"
    manager="siroy"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/26/2016"
    ms.author="mchad"/>

# <a name="azure-consistent-storage-differences-and-considerations"></a>Azure yhdenmukaisia tallennustilan: erot ja huomioon otettavia seikkoja

Azure yhdenmukaisia tallennustila on tallennustilan pilvipalveluihin Microsoft Azure Pinotut joukko. Azure yhdenmukaisia tallennustilan on blob, taulukko tai jonon tilin hallinta-toiminnon kanssa Azure yhdenmukaisia semantiikkaan liittyvien. Tässä artikkelissa on yhteenveto Azuren tallennustilaan tunnetut Azure yhdenmukaisia tallennustilan erot. On yhteenveto myös muita huomioon otettavia seikkoja kannattaa pitää mielessä, kun asennat Microsoft Azure pinon tällä hetkellä yleisesti saatavilla preview-versiosta.

<span id="Concepts" class="anchor"><span id="_Toc386544169" class="anchor"><span id="_Toc389466742" class="anchor"><span id="_Ref428966996" class="anchor"><span id="_Toc433223853" class="anchor"></span></span></span></span></span>
## <a name="known-differences"></a>Erot

Azure yhdenmukaisia tallennustilaa Technical Preview-versiossa ei ole 100 %: n ominaisuus välistä eroa kanssa Azuren tallennustilaan API-versiot, jotka ovat tuettuja. Tunnetut ominaisuuksien erojen ovat seuraavat:

-   Tallennustilan tilin tietyntyyppisten eivät ole vielä käytettävissä, esimerkiksi Standard\_RAGRS ja standardin\_GRS.

-   Premium\_LRS tallennustilan tilejä voi valmistelun yhteydessä. Kuitenkin tällä hetkellä ole suorituskyvyn rajoitukset tai oikeudet.

-   Azure tiedostot-toiminto ei ole vielä käytettävissä.

-   Hae sivun alueet API ei tue sivujen työpöytäkäytössä eri sivun BLOB tilannevedosten hakemisen.

-   Hae sivun alueet API palauttaa sivut, jotka eivät rakeisuuden 4 Kilotavua.

-   Osion ja rivin avaimen Azure yhdenmukaisia tallennustilan taulukon toteutukseen ovat kunkin enintään 400 merkkiä (eli 800 tavua) kokoinen.

-   Azure yhdenmukaisia tallennustilan Blob palvelun käyttöönoton Blob-objektien nimet on 880 merkkiä (eli 1760 tavua) kokoinen.

-   Vuokraajan tallennustilan käyttötietojen raportointi on tallennustilan käyttö metriä, jotka ovat samat kuin Azure-tietokannassa, on sama semantiikkaan liittyvien ja yksiköt Azure yhdenmukaisia tallennustilan käyttöönoton. Tällä hetkellä kuitenkin tallennustilan tapahtumat käyttö mittari ei ole IaaS tapahtumia ja tiedonsiirron käyttö mittari ei eritellä kaistanleveyden käytön sisäisiä ja ulkoisia verkkoliikennettä Azure pino-alueen mukaan.

-   Tiettyjä eroja olevia toimintoja vaikutusalueen tallennustilan hallinta. Ei voi muuttaa tilityyppi tai on mukautetut toimialueet. Lisäksi vain API-tason yhdenmukaisuuden tarjotaan Premium\_LRS tallennustilan tilin tyyppi.

## <a name="deployment-considerations"></a>Käyttöönottoon liittyviä huomioita

-   **Testi.** Älä ota Azure yhdenmukaisia tallennustilan tuotantoympäristössä, jotka käyttävät Microsoft Azure pinon Technical Preview-versio. Tässä versiossa on tarkoitettu vain arviointiin tarkoituksiin kurssin testiympäristössä.

-   **Suorituskykyä**. Azure yhdenmukaisia tallennustilan Microsoft Azure pinon Technical Preview-versio ei ole täysin suorituskyvyn optimoitu.

-   **Skaalattavuus.** Skaalattavuus ei ole tarkoitettu Azure yhdenmukaisia säilön Microsoft Azure pinon Technical Preview-version.

## <a name="version-support-considerations"></a>Version tuki huomioon otettavia seikkoja

Kanssa Azure yhdenmukaisia tallennustilaa tähän preview-versio tukevat seuraavia versioita:

> Azure-tallennustilan asiakkaan kirjasto: [Microsoft Azure tallennustilan 6.x .NET SDK: ssa](http://www.nuget.org/packages/WindowsAzure.Storage/6.2.0)
>
> Azure-tallennustilan tietopalvelut: [2015-04-05 REST API-versio](https://msdn.microsoft.com/library/azure/mt705637.aspx)
>
> Azure-tallennustilan hallintapalvelut: [2015-05-01 – esikatselu](https://msdn.microsoft.com/library/azure/mt163683.aspx)
> [2015-06-15](https://msdn.microsoft.com/library/azure/mt163683.aspx)
## <a name="next-steps"></a>Seuraavat vaiheet

-   [Azure yhdenmukaisia tallennustilan esittely](azure-stack-storage-overview.md)

<properties
    pageTitle="Azure sovelluksen-palvelu: Skaalaus App palvelusovellukset"
    description="Lue-kentän käyttäminen skaalaus sovellus App-palvelussa."
    keywords="sovelluksen, azure app palvelun, skaalaus-skaalattava-sovelluksen palvelusopimus-sovelluksen palvelun kustannukset"
    services="app-service"
    documentationCenter=""
    authors="btardif"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/07/2016"
    ms.author="byvinyal"/>

# <a name="azure-app-service-scaling-app-service-applications"></a>Azure sovelluksen-palvelu: Skaalaus App palvelusovellukset

Sovellusten ylläpidettävä Azure App palvelun voit [saavuttamiseksi valtaviin asteikko](https://azure.microsoft.com/blog/canadian-broadcasting-corporation-radio-canada-leverage-azure-for-smooth-election-coverage/).
Sovelluksen skaalaus on kuitenkin monimutkaisia ongelman, joka ei ole "paperilokeroita sopii kaikille"-ratkaisun. Sovellus ovat oikein näytöstä 3 tärkeimmät alueet, joka liittyy sovellusten success:

1. Tietoja sovelluksesi rakenteen ja sen heikkouksien.
    * Onko sovelluksen tilallisen? Tilattomien?
    * Mitkä ovat kaikki sovelluksesi osat?
        * Missä ovat pullonkaulojen-sovelluksessa?
    * Kun lataaminen on lisännyt sovelluksen, mitä vaihtuvat ensimmäisen?
2. Tietoja odotettu kuormituksen ja suorituskykyä koskevat vaatimukset.
    * Täytyykö sovelluksen yhteyshenkilönä tuhat käyttäjille? tai miljoonan?
    * Toimitetaan liikenne yksittäisen maantieteellisen sijainnin tai yleisesti?
    * Onko kausiluonteisista variaatiot? liikenne päät?
    * Kuinka nopeasti sovellus tulee vastata? sekunnin? 1 millisekunnin?
3. Tietoja ja hyödyntää oikein isännöinnin sovelluksen ympäristössä.
    * Mitä ominaisuuksia asteikko-tavoitteet olisi hyödyntää?

Tässä osassa auttavat sinua ymmärtämään tekijät ja Ohje, joka hyödyntää tarvittavat sovelluksen Service-toimintoja skaalattavuus tavoitteet strategiakartta suunnittelemaan.

[AZURE.INCLUDE [app-service-blueprint-scaling-app-service-applications](../../includes/app-service-blueprint-scaling-app-service-applications.md)]

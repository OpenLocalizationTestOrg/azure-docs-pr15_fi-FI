<properties
    pageTitle="Testaa tuotannon Web-sovellusten käytön aloittaminen"
    description="Lisätietoja Azure palvelun Web sovellukset tuotannon (TiP)-ominaisuuden testi."
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="01/13/2016"
    ms.author="cephalin"/>

# <a name="get-started-with-test-in-production-for-web-apps"></a>Testaa tuotannon Web-sovellusten käytön aloittaminen

Tuotannon testaaminen tai live-testataan käyttämällä suoraa asiakkaan liikenne koodiin on testi strategia, sovellusten kehittäjille integroida yhä [Joustava kehittäminen](https://en.wikipedia.org/wiki/Agile_software_development) niiden kehitysmenetelmistä. Sen avulla voit testata sovelluksia kanssa live käyttäjien tietoliikenne laadun tuotantoympäristössä synteettiseksi tietojen testiympäristössä sijaan. Mukaan paljastaa uusi sovellus reaali käyttäjille, voit voit ilmoitettava reaali ongelmista, sovellus saattaa yleisölle tarkoitetun, kun se on otettu käyttöön. Voit tarkistaa toimintoja, suorituskykyä ja sovelluksen-päivitysten äänenvoimakkuutta, nopeuden ja erilaisia reaali liikenteestä, joka voi koskaan lähentää testiympäristössä.

## <a name="traffic-routing-in-app-service-web-apps"></a>Liikenteen reititys App palvelun Web Apps-sovelluksissa

[Azure App palvelun](http://go.microsoft.com/fwlink/?LinkId=529714)liikenne reititys-toiminto voit ohjata reaaliaikaisia käyttäjien tietoliikenne vähintään yksi [käyttöönoton paikkojen](web-sites-staged-publishing.md)osa ja analysoida sovelluksen [Azure hakemuksen tiedot](/services/application-insights/) tai [Azure Hdinsightiin](/services/hdinsight/)tai kolmannen osapuolen työkalu, kuten [Uuden Relic](/marketplace/partners/newrelic/newrelic/) Vahvista muutokset. Esimerkiksi sovellus-palvelussa voit toteuttaa seuraavissa tilanteissa:

- Tutustu toimintojen virheet tai paikantaa suorituskyvyn pullonkaulat Päivityksesi ennen sivuston laajuinen käyttöönotto
- Suorittaa "valvottu testi lentojen" muutosten mittaaminen usibility arvot beeta-sovellukseen
- Vähitellen ylöspäin uusi päivitys asteikko ja tilanteen takaisin uusimpaan versioon, ilmenee virhe 
- Optimoi sinua sovelluksen liiketoiminnan tulosten suorittamalla [A / B Testaa](https://en.wikipedia.org/wiki/A/B_testing) tai useita käyttöönoton paikkojen [multivariate testit](https://en.wikipedia.org/wiki/Multivariate_testing_in_marketing)

### <a name="requirements-for-using-traffic-routing-in-web-apps"></a>Käyttövaatimukset liikenne reititys Web Apps-sovelluksissa

- Web-sovelluksen on suoritettava **Vakio** - tai **Premium** taso edellyttää useita käyttöönoton paikkojen ennalleen.
- Toimi oikein, jotta liikenne reititys edellyttää evästeet käyttöön käyttäjien selaimessa. Liikenne reititys evästeet kiinnittäminen elinkaaressa käyttöönoton paikka asiakkaan selaimen asiakas-istunto.
- Liikenne reititys tukee Vihje tilanteita – Azure PowerShellin cmdlet-komennot.

## <a name="route-traffic-segment-to-a-deployment-slot"></a>Reitin liikenne segmentin käyttöönoton paikka

Vihje jokaisen tilanteessa basic tasolla live liikenne ennalta määritettyjä prosentteina reitittämiseen tuotannon käyttöönoton paikka. Toiminto, noudata seuraavia ohjeita:

>[AZURE.NOTE] Nämä ohjeet oletetaan, että sinulla on jo [tuotannon käyttöönoton paikka](web-sites-staged-publishing.md) ja että halutun web app-sisältöä on jo sen [käyttöön](web-sites-deploy.md) .

1. Lokitiedoston [Azure Portal](https://portal.azure.com/).
2. Valitse sivu web-sovelluksen **asetukset** > **Liikenne reititys**.
  ![](./media/app-service-web-test-in-production/01-traffic-routing.png)
3. Valitse paikka, johon haluat reitittää liikenteen ja prosentin yhteensä liikenne haluavat ja valitse sitten **Tallenna**.

    ![](./media/app-service-web-test-in-production/02-select-slot.png)

4. Siirry käyttöönoton paikka-sivu. Live liikenteen reitittämisen siihen pitäisi tulla näkyviin.

    ![](./media/app-service-web-test-in-production/03-traffic-routed.png)

Liikenne reititys on määritetty, kun tietty prosenttimäärä asiakkaiden reititetään tuotannon paikka satunnaisesti. Kuitenkin on tärkeää muistaa, että kun asiakkaan reititetään automaattisesti tietyn paikka, se "kiinnitetty" asiakkaan kyseisen istunnon aikana, paikka. Tämä valmis käyttämällä eväste kiinnittäminen käyttäjäistunto. Jos tarkistat HTTP-pyyntöjen, huomaat `TipMix` eväste jokaisen myöhemmin pyynnön.

![](./media/app-service-web-test-in-production/04-tip-cookie.png)

## <a name="force-client-requests-to-a-specific-slot"></a>Asiakkaan pyyntöihin pakottaminen tietyn paikka

Lisäksi automaattinen liikenne reititys-sovelluksen palvelun pystyy reitin pyynnöt tietyn paikka. Tästä on hyötyä, kun haluat voi osallistua kyselyjä tai osallistumiseen beeta-sovelluksesi-käyttäjille. Toiminto, voit käyttää `x-ms-routing-name` kyselyn parametri.

Viiva käyttäjien tietyn paikan käyttämällä `x-ms-routing-name`, varmista, että paikka on jo lisätty liikenne reititysluetteloon. Koska haluat reitittää eksplisiittisesti paikka, voit määrittää Todellinen reititys prosentti ei ole väliä. Jos haluat, voit luoda "beta linkki-, jota napsauttamalla käyttäjät voivat käyttää beeta-sovellusta.

![](./media/app-service-web-test-in-production/06-enable-x-ms-routing-name.png)

### <a name="opt-users-out-of-beta-app"></a>Valita, käyttäjät ulos beeta-sovelluksesta

Voit antaa käyttäjien valita ulos beeta-sovelluksesta, esimerkiksi voit lisätä tämän linkin web-sivulle:

    <a href="<webappname>.azurewebsites.net/?x-ms-routing-name=self">Go back to production app</a>

Merkkijonon `x-ms-routing-name=self` määrittää tuotannon paikka. Kun selaimeen käyttää linkki, paitsi ohjataan tuotannon paikka, mutta jokainen myöhemmin pyyntö sisältää `x-ms-routing-name=self` evästeiden, kiinnittää tuotannon paikka istunto.

![](./media/app-service-web-test-in-production/05-access-production-slot.png)

### <a name="opt-users-in-to-beta-app"></a>Osallistua käyttäjien beeta-sovellukseen

Jotta käyttäjät voivat halutessaan beeta-sovelluksen, saman Kyselyparametrin arvoksi ei, paikka nimi esimerkiksi:

        <webappname>.azurewebsites.net/?x-ms-routing-name=staging

## <a name="more-resources"></a>Lisää resursseja ##

-   [Väliaikaisen ympäristöissä Azure App palvelun web-sovellusten määrittäminen](web-sites-staged-publishing.md)
-   [Monimutkaisen laadukkaampi Azure-sovelluksen käyttöönotto](app-service-deploy-complex-application-predictably.md)
-   [Joustava software development Azure-sovelluksen ominaisuudet](app-service-agile-software-development.md)
-   [Käytä DevOps ympäristöissä tehokkaasti web Apps-sovellukset](app-service-web-staged-publishing-realworld-scenarios.md)
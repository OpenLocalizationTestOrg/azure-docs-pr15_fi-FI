<properties
    pageTitle="Voit korjata Node.js verkkosovellukseen Azure sovelluksen-palvelussa"
    description="Opettele virheenkorjaus Node.js verkkosovellukseen Azure sovelluksen-palvelussa."
    tags="azure-portal"
    services="app-service\web"
    documentationCenter="nodejs"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="how-to-debug-a-nodejs-web-app-in-azure-app-service"></a>Voit korjata Node.js verkkosovellukseen Azure sovelluksen-palvelussa

Azure on sisäinen vianmääritys helpottamiseksi virheenkorjaus Node.js sovellusten ylläpidettävä [Azure App palvelun](http://go.microsoft.com/fwlink/?LinkId=529714) verkkosovelluksissa. Tässä artikkelissa kerrotaan stdout ja stderr kirjaaminen on virhetietojen näyttäminen selaimessa ja voit ladata ja katsella lokitiedostot.

Diagnostiikan Node.js sovellusten isännöimät Azure tarjoaa [IISNode]. Tässä artikkelissa kuvataan yleisimmät asetukset diagnostiikka tietojen keräämistä varten, kun se ei tarjoa valmis viittauksen IISNode käsittelyyn. Lisätietoja käsittelystä IISNode näy GitHub [IISNode Lueminut-tiedosto] .

<a id="enablelogging"></a>
## <a name="enable-logging"></a>Kirjaamisen ottaminen käyttöön

Oletusarvon mukaan sovelluksen web App-palvelun sieppaa vain diagnostiikkatietojen käyttöönotoissa, esimerkiksi kun otat käyttöön Git-sovellusta. Nämä tiedot ovat hyödyllinen, jos sinulla on ongelmia käyttöönoton, kuten virhe asennettaessa **package.json**Viitattu moduuli aikana tai jos käytössäsi on mukautettu käyttöönoton komentosarja.

Stdout ja stderr virtaa kirjaaminen käyttöön **IISNode.yml** -tiedoston luominen Node.js sovelluksen ylimmällä tasolla ja lisää seuraava:

    loggingEnabled: true

Näin kirjaaminen stderr ja stdout Node.js-sovelluksesta.

**IISNode.yml** tiedoston voidaan ohjausobjektiin myös, onko helpossa muodossa virheet tai developer virheitä palautetaan selaimeen virheen yhteydessä. Jotta developer virheitä, Lisää seuraava rivi **IISNode.yml** -tiedosto:

    devErrorsEnabled: true

Kun tämä asetus on käytössä, IISNode palauttaa tiedot lähetetään stderr sijaan ystävällisen virhesanoman, kuten "ilmeni virhe sisäisen palvelimen" viimeisen 64 kilotavua.

> [AZURE.NOTE] Kun devErrorsEnabled on hyötyä, kun ongelmien kehityksen aikana, ottaminen käyttöön tuotantoympäristössä voi johtaa kehittäminen virheet on lähetetty käyttäjille.

Jos **IISNode.yml** -tiedoston ei ole jo sovelluksessa, sinun on käynnistettävä koodiin päivitetty sovellus julkaisemisen jälkeen. Jos haluat muuttaa vain **IISNode.yml** aiemmin luotu tiedosto, joka on aiemmin julkaistu asetuksia, ei ole uudelleenkäynnistys ei tarvita.

> [AZURE.NOTE] Jos web Appissa on luotu käyttämällä Azure komentorivivalitsimet työkalut- tai Azure PowerShellin cmdlet-komennot, oletusarvo **IISNode.yml** tiedosto luodaan automaattisesti.

Käynnistämään web app valitsemalla web-sovelluksen [Azure-portaaliin](https://portal.azure.com)ja valitse sitten **Käynnistä** -painiketta:

![Käynnistä-painike][restart-button]

Jos Azure komentorivivalitsimet-työkalut asennetaan kehittäminen-ympäristössä, voit web-sovelluksen käynnistää uudelleen seuraava komento:

    azure site restart [sitename]

> [AZURE.NOTE] Vaikka loggingEnabled ja devErrorsEnabled ovat yleisimmät IISNode.yml asetukset tallentamiskäytäntöjen vianmääritystiedot, IISNode.yml voidaan määrittää erilaisia isännöintipalvelu-ympäristön asetuksia. Katso täysi luettelo kokoonpanoasetusten määrittäminen [iisnode_schema.xml](https://github.com/tjanczuk/iisnode/blob/master/src/config/iisnode_schema.xml) -tiedosto.

<a id="viewlogs"></a>
## <a name="accessing-logs"></a>Lokit käyttäminen

Vianmäärityslokit niitä voi käyttää kolmella eri tavalla; Käyttämällä Transfer Protocol FTP (File), lataamalla Zip-arkiston tai kuin live päivitetty stream lokin (tunnetaan myös nimellä kanta). Lokitiedostojen Zip-arkiston lataaminen tai reaaliaikaista stream tarkasteleminen edellyttää Azure komentorivivalitsimet työkalut. Nämä voidaan asentaa käyttämällä seuraava komento:

    npm install azure-cli -g

Kun asennettu, työkalujen niitä voi käyttää 'azure'-komento. Komentorivin Työkalut ensin on määritetty käyttämään Azure tilauksen. Lisätietoja siitä, miten voit suorittaa tämän tehtävän on artikkelissa **lataamisesta ja tuo Julkaisuasetukset** [Azure komentorivivalitsimet työkalujen määrittämisestä](../xplat-cli-connect.md) on artikkelissa-osassa.

###<a name="ftp"></a>FTP

Voit käyttää diagnostiikkatiedot FTP, siirry [Azure-portaalissa](https://portal.azure.com), valitse web-sovellus ja valitse sitten **raporttinäkymät-IKKUNAN**. **Pikalinkit** -osassa **FTP VIANMÄÄRITYSLOKIT** ja **FTPS VIANMÄÄRITYSLOKIT** -linkkien kautta pääset lokit, FTP-protokollan avulla.

> [AZURE.NOTE] Jos et ole aiemmin määrittänyt käyttäjänimi ja salasana, FTP- tai, voit tehdä **pikaopas** hallinta-sivulta valitsemalla **Määritä käyttöönoton tunnistetiedot**.

Palauttaa koontinäytön FTP URL-osoite on **publish LogFiles** -kansio, joka sisältää seuraavat alikansioiden:

* [Käyttöönottomenetelmä](web-sites-deploy.md) - käyttöönotto-menetelmää, kuten Git samannimistä kansio luodaan, ja se sisältää ominaisuuksissa liittyviä tietoja.

* nodejs - Stdout ja stderr tiedot tallennetaan-sovelluksen kaikki esiintymät (kun loggingEnabled on TOSI.)

###<a name="zip-archive"></a>Zip-arkisto

Voit ladata Zip-arkiston vianmäärityslokeihin, käytä seuraavaa komentoa Azure komentorivivalitsimet työkaluista:

    azure site log download [sitename]

Tämä Lataa **diagnostics.zip** nykyiseen kansioon. Arkisto sisältää seuraava kansiorakenne:

* ominaisuuksissa - sovelluksen-versioiden tietoja loki

* Publish LogFiles

    * [Käyttöönottomenetelmä](web-sites-deploy.md) - käyttöönotto-menetelmää, kuten Git samannimistä kansio luodaan, ja se sisältää ominaisuuksissa liittyviä tietoja.

    * nodejs - Stdout ja stderr tiedot tallennetaan-sovelluksen kaikki esiintymät (kun loggingEnabled on TOSI.)

###<a name="live-stream-tail"></a>Live stream (kanta)

Live stream diagnostiikan lokitiedot, katselemista Käytä Azure komentorivivalitsimet Työkalut seuraava komento:

    azure site log tail [sitename]

Tämä palauttaa stream log tapahtumia, jotka päivittyvät ilmenee palvelimessa. Vuosta voi palauttaa tietoja käyttöönotosta sekä stdout ja stderr (kun loggingEnabled on TOSI.)

<a id="nextsteps"></a>
## <a name="next-steps"></a>Seuraavat vaiheet

Tämän artikkelin opit ottaminen käyttöön ja käyttää Azure diagnostiikka tiedot. Kun tiedot on hyödyllisiä tietoja, jotka esiintyvät sovelluksen kanssa ongelmia, se saattaa osoittamalla moduuli käytät tai sovelluksen palvelun verkkosovelluksissa käytettävä Node.js versio on sama kuin se käyttää käyttöönotto-ympäristössä, ongelma.

Lisätietoja moduulit käyttäminen Azure-kohdassa [Käyttämällä Node.js moduulit Azure-sovellusten kanssa](../nodejs-use-node-modules-azure-apps.md).

Lisätietoja Node.js versio sovelluksen määrittämisestä on artikkelissa [määrittäminen Node.js versio Azure-sovelluksessa].

Lisätietoja on Katso myös [Node.js Developer Center](/develop/nodejs/).

## <a name="whats-changed"></a>Mikä on muuttunut
* Katso muutoksen opas verkkosivuilta App palveluun: [Azure App palvelu ja sen vaikutus aiemmin Azure-palvelut](http://go.microsoft.com/fwlink/?LinkId=529714)

>[AZURE.NOTE] Jos haluat aloittaa Azure App palvelun ennen rekisteröimässä Azure-tili, siirry [Yritä App palvelu](http://go.microsoft.com/fwlink/?LinkId=523751), jossa lyhytkestoinen starter verkkosovellukseen heti voit luoda sovelluksen-palvelussa. Ei ole pakollinen; luottokortit ei ole sitoumukset.

[IISNode]: https://github.com/tjanczuk/iisnode
[IISNode Lueminut-tiedosto]: https://github.com/tjanczuk/iisnode#readme
[How to Use The Azure Command-Line Interface]: ../xplat-cli-install.md
[Using Node.js Modules with Azure Applications]: ../nodejs-use-node-modules-azure-apps.md
[Node.js-versio, joka määrittää Azure-sovelluksessa]: ../nodejs-specify-node-version-azure-apps.md

[restart-button]: ./media/web-sites-nodejs-debug/restartbutton.png
 

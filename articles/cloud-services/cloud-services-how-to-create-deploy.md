<properties
    pageTitle="Voit luoda ja ottaa käyttöön pilvipalveluun | Microsoft Azure"
    description="Opi luomaan ja käyttöönotto-menetelmällä nopea luominen Azure-tietokannassa pilvipalveluun."
    services="cloud-services"
    documentationCenter=""
    authors="Thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="adegeo"/>




# <a name="how-to-create-and-deploy-a-cloud-service"></a>Voit luoda ja ottaa käyttöön pilvipalveluun

> [AZURE.SELECTOR]
- [Azure portal](cloud-services-how-to-create-deploy-portal.md)
- [Azure perinteinen portal](cloud-services-how-to-create-deploy.md)

Azure perinteinen-portaalissa on kahdella tavalla voit luoda ja ottaa käyttöön pilvipalveluun: **Nopea luominen** ja **Luo mukautettu**.

Tässä ohjeaiheessa kerrotaan, miten voit nopeasti luoda menetelmän avulla Luo uusi pilvipalvelussa ja käytä sitten lataaminen ja otetaan käyttöön Azure cloud palvelun paketin **lataaminen** . Tätä tapaa käytettäessä Azure perinteinen portaalissa on käytettävissä helposti linkit olet suorittanut kaikki vaatimukset edetessäsi. Jos olet valmis ottamaan yhteyttä pilvipalvelussa luomisen yhteydessä, voit tehdä molemmat samanaikaisesti käyttämällä **Mukautettu luominen**.

> [AZURE.NOTE] Jos haluat julkaista pilvipalvelussa-Visual Studio ryhmän Services (VSTS), käytä nopea luominen ja valitsemalla Määritä VSTS julkaiseminen **Pika-aloitus** -tai koontinäytön. Lisätietoja on artikkelissa [Azure käyttämällä Visual Studio ryhmän palveluissa jatkuva toimitus][TFSTutorialForCloudService], tai Katso **Pika-aloitus** -sivun Ohje.

## <a name="concepts"></a>Käsitteitä
Jotta voit ottaa käyttöön sovelluksen pilvipalvelussa Azure-tietokannassa tarvitaan kolme osaa:

- **Palvelun määritys**  
  Cloud-palvelun määritystiedosto (.csdef) määrittää service-malli, kuten roolien määrä.

- **Palvelun määritykset**  
  Cloud-palvelun kokoonpanotiedosto (.cscfg) on pilveen käyttöasetukset palvelu ja yksittäisten rooleihin liittyvistä aiheista roolin esiintymien määrän.

- **Palvelupakettiin**  
  Palvelupakettiin (.cspkg) sisältää sovelluksen-koodin ja määrityksiä ja palvelun määritystiedostoa.
  
Voit lukea lisää nämä ja paketin luomisesta [tähän](cloud-services-model-and-package.md).

## <a name="prepare-your-app"></a>Sovelluksen valmisteleminen
Ennen kuin voit ottaa käyttöön pilvipalveluun, sinun on luotava cloud palvelupakettiin (.cspkg) sovelluksen-koodin ja cloud palvelun määritystiedoston (.cscfg). Azure-SDK työkalujen valmistelemiseksi tarvittavat käyttöönoton nämä tiedostot. Voit asentaa SDK [Azure-lataukset](https://azure.microsoft.com/downloads/) -sivulla kieli, johon haluat kehittää sovellusta koodi.

Kolme cloud palvelun ominaisuuksien käyttöön tarvitaan erityiset määrityksiä ennen service-pakkauksen vieminen:

- Jos haluat ottaa käyttöön pilvipalveluun, joka käyttää Secure Sockets Layer (SSL) salauksen, [määrittää sovelluksesi](cloud-services-configure-ssl-certificate.md#step-2-modify-the-service-definition-and-configuration-files) SSL.

- Jos haluat määrittää roolin esiintymät, [määrittää roolit](cloud-services-role-enable-remote-desktop.md) etätyöpöydän etätyöpöydän yhteydet.

- Jos haluat määrittää yksityiskohtainen pilvipalvelussa sijaitsevaan valvonta-käyttöön pilvipalvelussa Azure Diagnostiikka. *Mahdollisimman vähän seuranta* (oletusasetus seuranta taso) käyttää suorituskyvyn laskureita kerättyjen roolin esiintymien (näennäiskoneiden) Host (isäntä)-käyttöjärjestelmissä. "Yksityiskohtainen seuranta * kerää muut arvot roolin esiintymät, voit ottaa käyttöön sovelluksen käsiteltäessä ilmenevät ongelmat tarkempi analyysi suorituskyvyn tietojen perusteella. Ota käyttöön Azure diagnostiikka lisätietoja [Ottaminen käyttöön diagnostiikka Azure-tietokannassa](cloud-services-dotnet-diagnostics.md).

Jos haluat luoda pilvipalveluun web roolit tai työntekijä roolit-versioiden kanssa, sinun täytyy [luoda palvelupakettiin](cloud-services-model-and-package.md#servicepackagecspkg).

## <a name="before-you-begin"></a>Ennen aloittamista

- Jos et ole vielä asentanut Azure SDK-paketissa, napsauta Avaa [Azure-lataussivulta](https://azure.microsoft.com/downloads/) **Asentaa Azure SDK** ja lataa kieli, johon haluat kehittää koodisi SDK-paketissa. (Sinun on suoritettava toiminto myöhemmin mahdollisuuden.)

- Roolin kaikki esiintymät vaativatko sertifikaatin luominen varmenteet. Cloud Services-palvelut edellyttävät .pfx-tiedosto ja yksityinen avain. Voit [ladata Azure varmenteet](cloud-services-configure-ssl-certificate.md#step-3-upload-a-certificate) luominen ja käyttöönotto pilvipalvelussa.

- Jos aiot käyttöönotto pilvipalvelussa affiniteetti ryhmään, Luo affiniteetti-ryhmä. Voit käyttää affiniteetti ryhmän pilvipalvelussa sijaitsevaan ja Azure muiden käyttöön alueen samaan sijaintiin. Voit luoda affiniteetti ryhmän Azure perinteinen portal **affiniteetti ryhmät** -sivulla **verkoissa** -alueella.


## <a name="how-to-create-a-cloud-service-using-quick-create"></a>Toimintaohje: luominen käyttämällä nopea luominen pilvipalveluun

1. [Azure perinteinen portal](http://manage.windowsazure.com/), valitse **Uusi**>**Laske**>**Pilvipalvelussa**>**Nopea luominen**.

    ![CloudServices_QuickCreate](./media/cloud-services-how-to-create-deploy/CloudServices_QuickCreate.png)

2. Kirjoita **URL-osoite**, alitoimialueen nimi, jota käytetään julkisen URL-osoitteen avaaminen tuotannon käyttöönotoissa cloud-palvelussa. Tuotannon käyttöönotoissa URL-muoto on: http://*myURL*. cloudapp.net.

3. Valitse **alue tai affiniteetti ryhmän**, maantieteellisen alueen tai affiniteetti ryhmän pilvipalvelussa, jos haluat ottaa käyttöön. Valitse affiniteetti ryhmä, jos haluat ottaa käyttöön pilvipalvelussa sijaitsevaan Azure muiden alueella samaan sijaintiin.

4. Valitse **Luo pilvipalvelussa**.

    ![CloudServices_Region](./media/cloud-services-how-to-create-deploy/CloudServices_Regionlist.png)

    Voit valvoa viestialueelle ikkunan alareunassa prosessin tila.

    **Cloud Services** -alueen avautuu uusi pilvipalvelussa näkyviin. Tilan muuttuessa luotu cloud palvelun luonti onnistui.

    ![CloudServices_CloudServicesPage](./media/cloud-services-how-to-create-deploy/CloudServices_CloudServicesPage.png)


## <a name="how-to-upload-a-certificate-for-a-cloud-service"></a>Toimintaohje: lataa varmenteen pilvipalveluun

1. [Azure perinteinen portal](http://manage.windowsazure.com/) **Pilvipalveluihin**ja valitse cloud-palvelun nimi **Varmenteet**.

    ![CloudServices_QuickCreate](./media/cloud-services-how-to-create-deploy/CloudServices_EmptyDashboard.png)


2. Valitse **Lataa varmenteen** tai **Lataa**.

3. **Tiedoston**käyttää (.pfx-tiedosto)-sertifikaatti **Selaa** .

4. Kirjoita **salasana**-sertifikaatin yksityinen avain.

5. Valitse **OK** (valintamerkki).

    ![CloudServices_AddaCertificate](./media/cloud-services-how-to-create-deploy/CloudServices_AddaCertificate.png)

    Voit katsoa viestialueen Lataa, alla etenemisen. Kun lataus on valmis, varmenne on lisätty taulukkoon. Valitse OK ja Sulje viesti viestialueen.

    ![CloudServices_CertificateProgress](./media/cloud-services-how-to-create-deploy/CloudServices_CertificateProgress.png)

## <a name="how-to-deploy-a-cloud-service"></a>Toimintaohje: käyttöönotto pilvipalveluun

1. [Azure perinteinen portal](http://manage.windowsazure.com/) **Pilvipalveluihin**ja pilvipalvelussa nimeä **raporttinäkymät-ikkunan**.

2. Valitse **Lataa uusi tuotannon käyttöönoton** tai **Lataa**.

3. Kirjoita uuden käyttöönotto – esimerkiksi MyCloudServicev4 nimi **käyttöönoton otsikko**.

3. **Paketin**avulla voit valita palvelun pakettitiedoston (.cspkg) käyttää **Selaa** .

4. **Määritysten**käyttävät palvelun määrittäminen tiedostolle (.cscfg) Valitse **Selaa** .

5. Jos pilvipalvelussa sijaitsevaan sisällä mitään rooleja vain yhtä esiintymää, valitse **käyttöön, vaikka useita rooleja sisältävät yhden esiintymän** -valintaruutu, jos haluat jatkaa käyttöönoton ottaminen käyttöön.

    Azure takaa vain 99.95 prosenttia access pilvipalveluun ylläpito ja palvelun päivitysten aikana Jos jokaisen roolilla on vähintään kaksi esiintymät. Tarvittaessa voit lisätä muita roolin esiintymät **asteikko** -sivulla sen jälkeen, kun otat pilvipalvelussa. Lisätietoja on artikkelissa [Palvelun tason sopimuksia](https://azure.microsoft.com/support/legal/sla/).

6. Valitse **OK** (valintamerkki) Aloita cloud palvelun käyttöönoton.

    ![CloudServices_UploadaPackage](./media/cloud-services-how-to-create-deploy/CloudServices_UploadaPackage.png)

    Voit valvoa viestialueen käyttöönoton tilan. Voit piilottaa sanoma valitsemalla OK.

    ![CloudServices_UploadProgress](./media/cloud-services-how-to-create-deploy/CloudServices_UploadProgress.png)

## <a name="verify-your-deployment-completed-successfully"></a>Tarkista käyttöönoton onnistui

1. Valitse **koontinäyttö**.

    Tilan pitäisi näkyä, että palvelu on **käynnissä**.

2. Valitse **quick glance**sivuston URL-osoite cloud-palvelun avaaminen selaimessa.

    ![CloudServices_QuickGlance](./media/cloud-services-how-to-create-deploy/CloudServices_QuickGlance.png)


[TFSTutorialForCloudService]: cloud-services-continuous-delivery-use-vso.md
 
## <a name="next-steps"></a>Seuraavat vaiheet

* [Yleisten määritysten cloud-palvelun](cloud-services-how-to-configure.md).
* Määritä [mukautettua toimialuenimeä](cloud-services-custom-domain-name.md).
* [Hallitse pilvipalvelussa sijaitsevaan](cloud-services-how-to-manage.md).
* Määritä [ssl-varmenteita](cloud-services-configure-ssl-certificate.md).
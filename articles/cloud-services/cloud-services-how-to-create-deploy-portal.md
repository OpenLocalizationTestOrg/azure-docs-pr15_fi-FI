<properties
    pageTitle="Voit luoda ja ottaa käyttöön pilvipalveluun | Microsoft Azure"
    description="Lue, miten voit luoda ja ottaa käyttöön pilvipalveluun Azure-portaalissa."
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
    ms.date="10/11/2016"
    ms.author="adegeo"/>




# <a name="how-to-create-and-deploy-a-cloud-service"></a>Voit luoda ja ottaa käyttöön pilvipalveluun

> [AZURE.SELECTOR]
- [Azure portal](cloud-services-how-to-create-deploy-portal.md)
- [Azure perinteinen portal](cloud-services-how-to-create-deploy.md)

Azure-portaalissa on kahdella tavalla voit luoda ja ottaa käyttöön pilvipalveluun: *Nopea luominen* ja *Luo mukautettu*.

Tässä artikkelissa kerrotaan, miten voit nopeasti luoda menetelmän avulla Luo uusi pilvipalvelussa ja käytä sitten Lataa ja asentaa cloud-palvelun pakkauksen Azure-tietokannassa **Lataa** . Tätä tapaa käytettäessä Azure portaalissa on käytettävissä helposti linkit olet suorittanut kaikki vaatimukset edetessäsi. Jos olet valmis ottamaan yhteyttä pilvipalvelussa luomisen yhteydessä, voit tehdä molemmat samanaikaisesti käyttämällä mukautettu luominen.

> [AZURE.NOTE] Jos haluat julkaista pilvipalvelussa-Visual Studio ryhmän Services (VSTS), käytä nopea luominen ja valitsemalla Määritä VSTS julkaiseminen Azure-pikaopas tai koontinäytön. Lisätietoja on artikkelissa [Azure käyttämällä Visual Studio ryhmän palveluissa jatkuva toimitus][TFSTutorialForCloudService], tai Katso **Pika-aloitus** -sivun Ohje.

## <a name="concepts"></a>Käsitteitä
Ottaa käyttöön sovelluksen pilvipalvelussa Azure-tietokannassa on oltava kolme osaa:

- **Palvelun määritys**  
  Cloud-palvelun määritystiedosto (.csdef) määrittää service-malli, kuten roolien määrä.

- **Palvelun määritykset**  
  Cloud-palvelun kokoonpanotiedosto (.cscfg) on pilveen käyttöasetukset palvelu ja yksittäisten rooleihin liittyvistä aiheista rooli esiintymien määrän.

- **Palvelupakettiin**  
  Palvelupakettiin (.cspkg) sisältää sovelluksen-koodin ja määrityksiä ja palvelun määritystiedostoa.

Voit lukea lisää nämä ja paketin luomisesta [tähän](cloud-services-model-and-package.md).

## <a name="prepare-your-app"></a>Sovelluksen valmisteleminen
Ennen kuin voit ottaa käyttöön pilvipalveluun, sinun on luotava cloud palvelupakettiin (.cspkg) sovelluksen-koodin ja cloud palvelun määritystiedoston (.cscfg). Azure-SDK työkalujen valmistelemiseksi tarvittavat käyttöönoton nämä tiedostot. Voit asentaa SDK [Azure-lataukset](https://azure.microsoft.com/downloads/) -sivulla kieli, johon haluat kehittää sovellusta koodi.

Kolme cloud palvelun ominaisuuksien käyttöön tarvitaan erityiset määrityksiä ennen service-pakkauksen vieminen:

- Jos haluat ottaa käyttöön pilvipalveluun, joka käyttää Secure Sockets Layer (SSL) salauksen, [määrittää sovelluksesi](cloud-services-configure-ssl-certificate-portal.md#modify) SSL.

- Jos haluat määrittää roolin esiintymät, [määrittää roolit](cloud-services-role-enable-remote-desktop.md) etätyöpöydän etätyöpöydän yhteydet. Tämän voi tehdä vain classic-portaalissa.

- Jos haluat määrittää yksityiskohtainen pilvipalvelussa sijaitsevaan valvonta-käyttöön pilvipalvelussa Azure Diagnostiikka. *Mahdollisimman vähän seuranta* (oletusasetus seuranta taso) käyttää suorituskyvyn laskureita kerättyjen roolin esiintymien (näennäiskoneiden) Host (isäntä)-käyttöjärjestelmissä. *Yksityiskohtainen seuranta* kerää muut arvot roolin esiintymät, voit ottaa käyttöön sovelluksen käsiteltäessä ilmenevät ongelmat tarkempi analyysi suorituskyvyn tietojen perusteella. Ota käyttöön Azure diagnostiikka lisätietoja [ottaminen käyttöön diagnostiikka Azure-tietokannassa](cloud-services-dotnet-diagnostics.md).

Jos haluat luoda pilvipalveluun web roolit tai työntekijä roolit-versioiden kanssa, sinun täytyy [luoda palvelupakettiin](cloud-services-model-and-package.md#servicepackagecspkg).

## <a name="before-you-begin"></a>Ennen aloittamista

- Jos et ole vielä asentanut Azure SDK-paketissa, napsauta Avaa [Azure-lataussivulta](https://azure.microsoft.com/downloads/) **Asentaa Azure SDK** ja lataa kieli, johon haluat kehittää koodisi SDK-paketissa. (Sinun on suoritettava toiminto myöhemmin mahdollisuuden.)

- Roolin kaikki esiintymät vaativatko sertifikaatin luominen varmenteet. Cloud Services-palvelut edellyttävät .pfx-tiedosto ja yksityinen avain. [Voit ladata Azure varmenteet]() tavalliseen tapaan luominen ja käyttöönotto pilvipalvelussa.

- Jos aiot käyttöönotto pilvipalvelussa affiniteetti ryhmään, Luo affiniteetti-ryhmä. Voit käyttää affiniteetti ryhmän pilvipalvelussa sijaitsevaan ja Azure muiden käyttöön alueen samaan sijaintiin. Voit luoda affiniteetti ryhmän Azure perinteinen portal **affiniteetti ryhmät** -sivulla **verkot** -alueella.


## <a name="create-and-deploy"></a>Luoda ja ottaa käyttöön

1. Kirjaudu sisään [Azure portal](https://portal.azure.com/).
2. Valitse **Uusi > näennäiskoneiden**, ja vieritä sitten alaspäin ja valitse **Pilvipalvelussa**.

    ![Julkaise pilvipalvelussa sijaitsevaan](media/cloud-services-how-to-create-deploy-portal/create-cloud-service.png)

3. Tiedot-sivu, jossa näkyy alareunassa valitsemalla **Luo**. 
4. Kirjoita **Pilvipalvelussa** uusi sivu- **DNS-nimi**-arvo.
5. Luo uusi **Resurssiryhmä** tai valitse aiemmin luotu.
6. Valitse **sijainti**.
7. Valitse **paketti**. **Paketin lataaminen** -sivu avautuu. Täytä tarvittavat kentät.  

     Jos jokin oman roolit sisältää yksittäisen esiintymän, varmista **käyttöön, vaikka useita rooleja sisältävät yhden esiintymän** on valittuna.

8. Varmista, että **Käynnistä käyttöönoton** on valittuna.
9. Valitse **OK** , joka sulkee **paketin lataaminen** -sivu.
10. Jos sinulla ei ole Lisää sertifikaatteja, valitse **Luo**.

    ![Julkaise pilvipalvelussa sijaitsevaan](media/cloud-services-how-to-create-deploy-portal/select-package.png)

## <a name="upload-a-certificate"></a>Lataa varmenteen

Jos yhteyttä käyttöönottopaketti on [määritetty käyttämään varmenteet](cloud-services-configure-ssl-certificate-portal.md#modify), voit ladata varmenteen nyt.

1. Valitse **Varmenteet**ja valitse **Lisää varmenteet** -sivu SSL-varmenteen .pfx-tiedosto ja anna **salasana** varmenteelle
2. Valitse **Liitä varmenne**ja valitse sitten **OK** - **Lisää varmenteet** -sivu.
3. Valitse **Luo** **Pilvipalvelussa** -sivu. Kun käyttöönoton on päässyt **Valmis** , voit siirtyä seuraaviin vaiheisiin.

    ![Julkaise pilvipalvelussa sijaitsevaan](media/cloud-services-how-to-create-deploy-portal/attach-cert.png)


## <a name="verify-your-deployment-completed-successfully"></a>Tarkista käyttöönoton onnistui

1. Valitse cloud palveluesiintymä.

    Tilan pitäisi näkyä, että palvelu on **käynnissä**.

2. Valitse **Essentials**- **Sivuston URL-osoite** cloud-palvelun avaaminen selaimessa.

    ![CloudServices_QuickGlance](./media/cloud-services-how-to-create-deploy-portal/running.png)


[TFSTutorialForCloudService]: http://go.microsoft.com/fwlink/?LinkID=251796

## <a name="next-steps"></a>Seuraavat vaiheet

* [Yleisten määritysten cloud-palvelun](cloud-services-how-to-configure-portal.md).
* Määritä [mukautettua toimialuenimeä](cloud-services-custom-domain-name-portal.md).
* [Hallitse pilvipalvelussa sijaitsevaan](cloud-services-how-to-manage-portal.md).
* Määritä [ssl-varmenteita](cloud-services-configure-ssl-certificate-portal.md).
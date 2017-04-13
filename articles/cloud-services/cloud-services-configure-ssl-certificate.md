<properties 
    pageTitle="Määrittää SSL: ää pilvipalvelussa (perinteinen) | Microsoft Azure" 
    description="Katso HTTPS-päätepisteen web-roolin määrittäminen ja SSL-varmenteen suojaamiseen sovelluksen lataaminen." 
    services="cloud-services" 
    documentationCenter=".net" 
    authors="Thraka" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/04/2016"
    ms.author="adegeo"/>




# <a name="configuring-ssl-for-an-application-in-azure"></a>Sovelluksen Azure SSL määrittäminen

> [AZURE.SELECTOR]
- [Azure portal](cloud-services-configure-ssl-certificate-portal.md)
- [Azure perinteinen portal](cloud-services-configure-ssl-certificate.md)

Secure Socket Layer (SSL)-salaus on lähetetty Internetissä tietojen suojaaminen yleisimmin käytettyjä maksutavan. Yleisiä tehtävän käsitellään HTTPS-päätepisteen web-roolin määrittäminen ja SSL-varmenteen suojaamiseen sovelluksen lataaminen.

> [AZURE.NOTE] Tässä tehtävässä toimet koskevat Azure pilvipalveluihin; Sovelluksen Services Katso [Tässä](../app-service-web/web-sites-configure-ssl-certificate.md) artikkelissa.

Tämän tehtävän käyttää tuotannon käyttöönotto. Lisätietoja käyttämisestä väliaikaisen käyttöönoton annetaan tämän ohjeaiheen lopussa.

Lue [artikkeli](cloud-services-how-to-create-deploy.md) ensimmäisen kerran, jos sinulla ei ole vielä luonut pilvipalveluun.

[AZURE.INCLUDE [websites-cloud-services-css-guided-walkthrough](../../includes/websites-cloud-services-css-guided-walkthrough.md)]


## <a name="step-1-get-an-ssl-certificate"></a>Vaihe 1: Hanki SSL-varmenne

SSL määrittämiseksi sovelluksen, sinun on Hanki SSL-varmenne, joka on allekirjoitettu mukaan varmenteen myöntäjä (CA), luotettujen kolmannelta osapuolelta, joka tätä varten-varmenteet ongelmat. Jos sinulla ei ole yhtä, sinun on hankittava se yritys, joka myy SSL-varmenteita.

Varmenne on täytettävä seuraavat vaatimukset Azure SSL-varmenteita:

-   Varmenne on oltava yksityinen avain.
-   Varmenne on luotava avaimen exchange, voi viedä henkilökohtaisten tietojen vaihtaminen (.pfx)-tiedostoon.
-   Varmenteen aihe on vastattava käyttää pilvipalvelussa sijaitsevaan toimialueen. SSL-varmenteen et voi hankkia cloudapp.net toimialueen varmenteiden myöntäjältä (CA). Sinun on hankittava mukautettua toimialuenimeä, kun käyttää palvelussa. Kun pyydät varmenteen myöntäjältä, sertifikaatin aihe on vastattava sovellus käyttää mukautettua toimialuenimeä. Esimerkiksi jos mukautettu toimialuenimi on **contoso.com** sinun varmenteen pyytämisestä Myöntäjän varten * **. contoso.com** tai * *www.contoso.com**.
-   Varmenne on käytettävä vähintään 2048-bittinen salaus.

Testaus, voit [luoda](cloud-services-certs-create.md) ja käyttää itse allekirjoitettua varmennetta. Itse allekirjoitetun varmenteen ei ole todennettu käyttää CA ja käyttää cloudapp.net toimialueen sivuston URL-osoite. Esimerkiksi seuraava tehtävä käyttää itse allekirjoitettua varmennetta, joissa käytetään varmenteen yleinen nimi (CN) on **sslexample.cloudapp.net**.

Seuraavaksi on lisättävä tietoja varmenteen palvelun määritys ja palvelun määritys-tiedostot.

## <a name="step-2-modify-the-service-definition-and-configuration-files"></a>Vaihe 2: Muokkaa palvelun määritys- ja määritystietoja tiedostoja

Sovelluksen on määritetty käyttämään varmennetta ja HTTPS-päätepisteen on lisättävä. Tämän vuoksi palvelun määritys ja palvelun asetustiedostot, on päivitettävä.

1.  Kehitysympäristö Avaa palvelun määritystiedosto (CSDEF), **Varmenteet** osan **WebRole** -osan lisääminen ja varmenne (ja keskitason sertifikaatteja) seuraavat tiedot:

        <WebRole name="CertificateTesting" vmsize="Small">
        ...
            <Certificates>
                <Certificate name="SampleCertificate" 
                             storeLocation="LocalMachine" 
                             storeName="My"
                             permissionLevel="limitedOrElevated" />
                <!-- IMPORTANT! Unless your certificate is either
                self-signed or signed directly by the CA root, you
                must include all the intermediate certificates
                here. You must list them here, even if they are
                not bound to any endpoints. Failing to list any of
                the intermediate certificates may cause hard-to-reproduce
                interoperability problems on some clients.-->
                <Certificate name="CAForSampleCertificate"
                             storeLocation="LocalMachine"
                             storeName="CA"
                             permissionLevel="limitedOrElevated" />
            </Certificates>
        ...
        </WebRole>

    **Varmenteet** -osa määrittää Microsoftin varmenne, sen sijainti ja nimi, jossa se sijaitsee kaupan nimi.
    
    Käyttöoikeudet (`permisionLevel` määrite) voidaan määrittää jokin seuraavista arvoista:

  	| Oikeuksien arvo  | Kuvaus |
  	| ----------------  | ----------- |
  	| limitedOrElevated | **(Oletus)** Kaikki roolin prosessit voivat käyttää yksityinen avain. |
  	| laajennettuja          | Vain laajennettuja prosessit voivat käyttää yksityinen avain.|

2.  Lisää palvelu määritystiedosto **InputEndpoint** osan käyttöön HTTPS **päätepisteet** -osassa:

        <WebRole name="CertificateTesting" vmsize="Small">
        ...
            <Endpoints>
                <InputEndpoint name="HttpsIn" protocol="https" port="443" 
                    certificate="SampleCertificate" />
            </Endpoints>
        ...
        </WebRole>

3.  Lisää palvelu määritystiedosto **sidonta** elementissä **sivustojen** -osassa. Tässä osassa Lisää HTTPS sitominen päätepisteen yhdistäminen sivustoon:

        <WebRole name="CertificateTesting" vmsize="Small">
        ...
            <Sites>
                <Site name="Web">
                    <Bindings>
                        <Binding name="HttpsIn" endpointName="HttpsIn" />
                    </Bindings>
                </Site>
            </Sites>
        ...
        </WebRole>

    Kaikki tarvittavat muutokset palvelun määritystiedoston on suoritettu, mutta haluat varmenne-tietojen lisääminen service-kokoonpanotiedosto.

4.  Määritystiedostossa service (CSCFG) ServiceConfiguration.Cloud.cscfg, Lisää **Varmenteet** osa **roolin** jaksoon korvaaminen otoksen allekirjoitus-arvo, jonka varmennetta alla:

        <Role name="Deployment">
        ...
            <Certificates>
                <Certificate name="SampleCertificate" 
                    thumbprint="9427befa18ec6865a9ebdc79d4c38de50e6316ff" 
                    thumbprintAlgorithm="sha1" />
                <Certificate name="CAForSampleCertificate"
                    thumbprint="79d4c38de50e6316ff9427befa18ec6865a9ebdc" 
                    thumbprintAlgorithm="sha1" />
            </Certificates>
        ...
        </Role>

(Edellä olevasta esimerkistä käyttää **sha1** Allekirjoitusalgoritmi. Määritä haluamasi arvo varmenteen Allekirjoitusalgoritmi.)

Nyt kun palvelun määritys ja palvelun asetustiedostot on päivitetty, pakkaa käyttöönoton Azure ladataan. Jos käytössäsi on **cspack**, älä käytä **/generateConfigurationFile** lippu, kun, joka korvaa lisäsit varmenteen tiedot.

## <a name="step-3-upload-a-certificate"></a>Vaihe 3: Lataa varmenteen

Käyttöönottopaketti on päivitetty käyttämään varmennetta ja HTTPS-päätepisteen on lisätty. Nyt voit ladata paketin ja varmennetta Azure Azure perinteinen-portaalissa.

1. Kirjaudu sisään [Azure perinteinen portal][]. 
2. Valitse vasemman reunan siirtymisruudussa **Pilvipalveluihin** .
3. Valitse haluamasi pilvipalvelussa.
4. Valitse **Varmenteet** -välilehti.

    ![Valitse varmenteet-välilehti](./media/cloud-services-configure-ssl-certificate/click-cert.png)

5. Napsauttamalla **Lataa** -painiketta.

    ![Lataa](./media/cloud-services-configure-ssl-certificate/upload-button.png)
    
6. Anna **tiedoston** **salasana**ja valitse sitten **Valmis** (valintamerkki).

## <a name="step-4-connect-to-the-role-instance-by-using-https"></a>Vaihe 4: Muodosta yhteys roolin esiintymän käyttämällä HTTPS

Nyt kun käyttöönoton on Azure käytön, voit muodostaa yhteyden sen HTTPS.

1.  Azure perinteinen-portaalissa käyttöönoton ja valitse **Sivuston URL-osoite**-kohdassa.

    ![Määrittää sivuston URL-osoite][2]

2.  Selaimessa Muokkaa linkkiä **http:n**sijaan **HTTPS-protokollan** avulla ja sitten ‑sivustossa.

    >[AZURE.NOTE] Jos käytössäsi on itse allekirjoitettua varmennetta, johon on liitetty itse allekirjoitetun varmenteen HTTPS-päätepisteen selatessasi huomaat saattaa varmenteen selaimessa. Allekirjoittanut Luotetut varmenteiden myöntäjä sertifikaatilla ratkaisee tämän ongelman; Tässä vaiheessa voit ohittaa virheen. (Toinen vaihtoehto on itse allekirjoitetun varmenteen lisääminen käyttäjän luotettu varmenteen myöntäjä varmenteen säilössä.)

    ![SSL-Esimerkki web-sivusto][3]

Jos haluat käyttää sen sijaan, että tuotannon käyttöönoton väliaikaisen käyttöönottoa SSL, sinun on määrittää URL-Osoitteen, väliaikaisen käyttöönottoa varten. Käyttöönottaminen pilvipalvelussa sijaitsevaan väliaikaisen ympäristön ilman myös varmenteen tai varmenteen tiedot. Kun tiedot on käytössä, voit määrittää GUID-pohjaista URL-osoite, joka näkyy Azure perinteinen portaalin **Sivuston URL-osoite** -kenttään. Varmenteen luominen yleisiä nimellä (CN) yhtä GUID-pohjaista URL-osoite (esimerkiksi **32818777 6e77-4ced-a8fc-57609d404462.cloudapp.net**). Varmenteen lisääminen vaiheistettu pilvipalvelussa Azure perinteinen portaalin avulla. Lisää sitten sertifikaattitiedot CSDEF ja CSCFG tiedostojen, pakata sovelluksesi ja päivittää vaiheistettu käyttöönoton käyttämään uutta pakettia.

## <a name="next-steps"></a>Seuraavat vaiheet

* [Yleisten määritysten cloud-palvelun](cloud-services-how-to-configure.md).
* Lisätietoja käyttöönottamisesta [pilvipalveluun](cloud-services-how-to-create-deploy.md).
* Määritä [mukautettua toimialuenimeä](cloud-services-custom-domain-name.md).
* [Hallitse pilvipalvelussa sijaitsevaan](cloud-services-how-to-manage.md).


  [Azure perinteinen portal]: http://manage.windowsazure.com
  [0]: ./media/cloud-services-configure-ssl-certificate/CreateCloudService.png
  [1]: ./media/cloud-services-configure-ssl-certificate/AddCertificate.png
  [2]: ./media/cloud-services-configure-ssl-certificate/CopyURL.png
  [3]: ./media/cloud-services-configure-ssl-certificate/SSLCloudService.png
  [4]: ./media/cloud-services-configure-ssl-certificate/AddCertificateComplete.png  

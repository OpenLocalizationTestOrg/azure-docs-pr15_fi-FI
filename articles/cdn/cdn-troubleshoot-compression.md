<properties
    pageTitle="Tiedoston pakkauksen Azure CDN vianmääritys | Microsoft Azure"
    description="Azure CDN tiedostojen pakkaamisen ongelmien vianmääritystä."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="casoper"/>
    
# <a name="troubleshooting-cdn-file-compression"></a>CDN tiedostojen pakkaamisen vianmääritys

Tämän artikkelin avulla voit [CDN tiedostojen pakkaamisen](cdn-improve-performance.md)ongelmien vianmääritystä.

Jos tarvitset apua milloin tahansa tämän artikkelin, ota Azure asiantuntijoille [MSDN-Azure](https://azure.microsoft.com/support/forums/)ja Pinon ylivuoto-keskustelupalstoilla. Voit vaihtoehtoisesti myös jättää Azure tuki tapahtuma. Siirry [Azure tukisivustossa](https://azure.microsoft.com/support/options/) ja valitse **Pyydä tuki**.

## <a name="symptom"></a>Oire

Päätepiste pakkaus on käytössä, mutta tiedostoja palautetaan pakkaamattomat.

>[AZURE.TIP] Voit tarkistaa, onko tiedostojen palautetaan pakattu, sinun on esimerkiksi [Fiddler](http://www.telerik.com/fiddler) tai selaimen [Kehitystyökalut](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/)-työkalun avulla.  Tarkista HTTP vastausten otsikot palautettu välimuistissa CDN sisällön.  Onko otsikkoa `Content-Encoding` arvona **gzip**, **bzip2**tai **Kovera**sisältöä on pakattu.
>
>![Sisällön koodaus otsikko](./media/cdn-troubleshoot-compression/cdn-content-header.png)

## <a name="cause"></a>Syy

On useita mahdollisia syitä, mukaan lukien:

- Vaadittua sisältöä ei oikeuta pakkaus.
- Pakkaamisen ei ole otettu käyttöön pyydetty tiedostotyyppi.
- HTTP-pyyntö eivät kuulu ylätunnisteen pyytää kelvollinen pakkaamisen tyyppi.

## <a name="troubleshooting-steps"></a>Vianmääritysohjeita

> [AZURE.TIP] Samoin kuin otat uuden päätepisteet CDN määritysmuutoksia kestää jonkin aikaa leviäminen verkon kautta.  Yleensä tehdyt muutokset tulevat voimaan 90 minuutin kuluessa.  Jos olet määrittänyt pakkaamisen CDN päätepisteen ensimmäistä kertaa, ota huomioon odottaa 1-2 tuntia ja varmista pakkaamisen asetukset on välitetty avautuu näyttöön. 

### <a name="verify-the-request"></a>Tarkista pyyntö

Olemme ensin muuta nopeasti sanity tarkistaa pyynnön.  Selaimen [Kehitystyökalut](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/) avulla voit tarkastella pyynnöt.

- Varmista, että päätepisteen URL-osoite, lähetetään pyyntö `<endpointname>.azureedge.net`, ja alkuperän yhteyttä.
- Tarkista pyynnön sisältää **Hyväksy koodaus** otsikko ja kyseisen arvon **gzip**, **Kovera**tai **bzip2**.

> [AZURE.NOTE] **Azure CDN Akamai-** profiileista tukevat vain **gzip** koodaus.

![CDN pyynnön otsikot](./media/cdn-troubleshoot-compression/cdn-request-headers.png)

### <a name="verify-compression-settings-standard-cdn-profile"></a>Tarkista pakkausasetuksia (CDN vakio-profiilit)

> [AZURE.NOTE] Tämä vaihe koskee vain, jos CDN profiilin on **Azure CDN vakio-Verizon** tai **Azure CDN vakio-Akamai** profiili. 

Siirry [Azure portal](https://portal.azure.com) päätepiste ja valitse **Määritä** -painiketta.

- Tarkista pakkaus on käytössä.
- Tarkista sisällön MIME-tyyppiä pakataan sisältyy pakattu muotoilut-luettelosta.

![CDN pakkaamisen asetukset](./media/cdn-troubleshoot-compression/cdn-compression-settings.png)

### <a name="verify-compression-settings-premium-cdn-profile"></a>Tarkista pakkausasetuksia (Premium CDN-profiilit)

> [AZURE.NOTE] Tämä vaihe koskee vain, jos CDN profiilin on **Azure CDN Premium-Verizon** profiili.

Siirry [Azure portal](https://portal.azure.com) päätepiste ja valitse **hallinta** -painike.  Täydentävä portaaliin ladatut tiedostot avautuvat.  Vie hiiren osoitin **HTTP suuri** -välilehti ja valitse viemällä hiiren osoittimen **Asetukset** -valikon avauspainike.  Valitse **pakkaamisen**. 

- Tarkista pakkaus on käytössä.
- Tarkista **Tiedostotyyppien** luettelo sisältää CSV-luettelo (ilman välilyöntejä) MIME-tyypit.
- Tarkista sisällön MIME-tyyppiä pakataan sisältyy pakattu muotoilut-luettelosta.

![CDN premium pakkaamisen asetukset](./media/cdn-troubleshoot-compression/cdn-compression-settings-premium.png)

### <a name="verify-the-content-is-cached"></a>Tarkista sisältö on välimuistissa

> [AZURE.NOTE] Tämä vaihe koskee vain, jos CDN profiilin on **Azure CDN Verizon-** profiili (standardiksi tai Premium).

Selaimen developer työkaluilla Tarkista varmistaaksesi, että tiedosto on tallennettu välimuistiin alue, johon se sinulta pyydetään vastauksen otsikot.

- Tarkista **palvelimen** vastauksen otsikon.  Otsikon pitäisi olla **ympäristö (POP-palvelimen tunnus)**-muoto, tarkastelu seuraavan esimerkin mukaisesti.
- Tarkista **X-välimuistin** vastauksen otsikon.  Otsikon Lue **viesti**.  

![CDN vastausten otsikot](./media/cdn-troubleshoot-compression/cdn-response-headers.png)

### <a name="verify-the-file-meets-the-size-requirements"></a>Tarkista tiedosto vastaa kokoa

> [AZURE.NOTE] Tämä vaihe koskee vain, jos CDN profiilin on **Azure CDN Verizon-** profiili (Standard tai Premium).

Voi saada pakkaus, tiedoston on täytettävä seuraavat vaatimukset kokoa:

- Suurempi kuin 128 tavua.
- Pienempi kuin 1 Megatavu.

### <a name="check-the-request-at-the-origin-server-for-a-via-header"></a>Tarkista origin-palvelimen **kautta** ylätunnisteen pyyntö

**Kautta** HTTP-otsikon ilmaisee WWW-palvelimeen, että pyyntö on välitettävä välityspalvelinta.  Microsoft IIS-WWW-palvelimien oletusarvoisesti ei pakata vastaukset, kun kutsu on **kautta** otsikko.  Jos haluat ohittaa tämän ongelman, tee seuraavat:

- **IIS 6**: [määrittäminen päätetään = "FALSE" IIS ominaisuudet](https://msdn.microsoft.com/library/ms525390.aspx)
- **IIS 7 tai uudempi**: [ **noCompressionForHttp10** ja **noCompressionForProxies** EPÄTOSI-palvelimen määrittäminen](http://www.iis.net/configreference/system.webserver/httpcompression)


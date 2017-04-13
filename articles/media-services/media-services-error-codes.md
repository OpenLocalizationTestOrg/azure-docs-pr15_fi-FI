<properties
    pageTitle="Azure Media-palveluiden virhekoodit | Microsoft Azure"
    description="Aihe on yleiskuvaus Azure Media-palveluiden virhekoodit."
    authors="Juliako"
    manager="erikre"
    editor=""
    services="media-services"
    documentationCenter=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016" 
    ms.author="juliako"/>

# <a name="azure-media-services-error-codes"></a>Azure Media-palveluiden virhekoodit

Kun käytät Microsoft Azure Media Services, näyttöön voi tulla HTTP virhekoodit sen mukaan, ongelmat, kuten todennus tunnusten päättyvän toiminnot, joita ei tueta Media Services-palvelusta. Seuraavassa on luettelo **HTTP-virhekoodit** , jotka voivat palauttaa Media Services ja mahdollisia syitä niiden.  
  
## <a name="400-bad-request"></a>Pyyntö 400 ei kelpaa

Sisältää virheellisiä tietoja sekä on hylätty jokin seuraavista syistä:

- API-versiota ei tueta on määritetty. Katso uusin versio [Media Services REST API kehittäminen asetukset](media-services-rest-how-to-use.md).
- Media-palveluiden API-versio ei ole määritetty. Lisätietoja siitä, miten API-versio on artikkelissa [yhteyden muodostaminen Media Services Media Services REST-ohjelmointirajapinnalla](media-services-rest-connect-programmatically.md). 
   
    >[AZURE.NOTE] Jos käytät .NET tai Java SDK: T muodostaa Media Services, sinulle on määritetty API-versio aina, kun ryhdyt poistamaan ja suorittaa jonkin toiminnon vastaan Media Services.
- Määrittämätön ominaisuus, joka on määritetty. Ominaisuuden nimi on virheilmoituksen. Vain ominaisuudet, jotka kuuluvat tiettyä kohdetta voidaan määrittää. Katso [Azure Media Services REST API-viittaus](http://msdn.microsoft.com/library/azure/hh973617.aspx) luettelon kohteiden ja niiden ominaisuudet.
- Virheellinen arvo on määritetty. Ominaisuuden nimi on virheilmoituksen. Katso edellisen linkkiä kelvollinen ominaisuuden tyypit ja niiden arvot.
- Kiinteistön hinta puuttuu ja vaaditaan.
- Osana määritetty URL-osoite on virheellinen arvo.
- Yritettiin WriteOnce-ominaisuuden päivittäminen.
- Luo työ, jossa on syötteen resurssi, jossa ensisijaisen AssetFile, joka ei ole määritetty tai ei voitu määrittää yritettiin.
- Päivitä SAS Locator yritettiin. SAS locators voi vain luodaan tai poistetaan. Streaming locators voidaan päivittää. Lisätietoja on artikkelissa [Locators](http://msdn.microsoft.com/library/azure/hh974308.aspx).
- Ei tueta toiminnon tai kyselyn lähetettiin. 

## <a name="401-unauthorized"></a>401-ei valtuuksia

Pyyntö ei voitu todentaa (ennen sen on lupa) jokin seuraavista syistä:

- Puuttuva todennus otsikko.
- Virheelliset todennus otsikon arvo.
    - Tunnus on vanhentunut. Jos käytät REST API suoraan, artikkelissa [yhteyden muodostaminen Media Services Media Services REST-ohjelmointirajapinnalla](media-services-rest-connect_programmatically.md) opit luomaan uuden todennus-tunnuksen. Jos käytössäsi on .NET tai Java SDK: T, luoda CloudMediaContext tai MediaContract objektin tunnuksen luomiseen. Lisätietoja hallinnasta on artikkelissa [yhteyden muodostaminen Media-palvelun .NET Media Services SDK-paketissa](media-services-dotnet-connect-programmatically.md).
    - Tunnus sisältää epäkelpo allekirjoitus.</li></ul></li></ul>

## <a name="403-forbidden"></a>403-Käyttö estetty

Pyyntö ei sallita jokin seuraavista syistä:

- Media Services-tilin ei löydy, tai se on poistettu.
- Media Services-tili on poistettu käytöstä ja pyynnön tyyppiä ei ole HTTP GET. Palvelun toimintojen palauttaa sekä 403 vastausta.
- Todennus-sarakkeessa ei ole käyttäjän tunnistetietoja: AccountName ja/tai SubscriptionId. Löydät nämä tiedot Media Services Käyttöliittymä-tunniste Media Services-tilin Azure hallinta-portaalissa.
- Resurssin ei voi käyttää.
    - Yritettiin käyttämään MediaProcessor, joka ei ole käytettävissä omalla tililläsi Media-palveluita.
    - Yritettiin päivittää JobTemplate, joka on määritetty Media-palveluissa.
    - Korvaa joidenkin muihin Media Services asiakkaan Locator yritettiin.
    - Korvaa joidenkin muihin Media Services asiakkaan ContentKey yritettiin.

- Resurssin ei onnistunut, joka on saavutettu Media Services-tilin palvelun kiintiön vuoksi. Saat lisätietoja palvelu-kiintiön [kiintiöiden ja rajoitukset](media-services-quotas-and-limitations.md).

## <a name="404-not-found"></a>404 ei löydy

Pyyntö ei sallita resurssien vuoksi jompikumpi seuraavista syistä:

- Yritettiin päivittää kohteen, jota ei ole.
- Jos haluat poistaa kohteen, jota ei ole yritettiin.
- Voit luoda kohteen, joka linkittyy kohde, jota ei ole yritettiin.
- Yritettiin saat kohde, jota ei ole.
- Yritettiin tallennustilan-tili, jota ei ole liitetty Media Services-tilin määrittäminen.  

## <a name="409-conflict"></a>409 ristiriita

Pyyntö ei sallita jokin seuraavista syistä:

- Useamman kuin yhden AssetFile on määritetty nimi kohteen sisällä.
- Luo toinen ensisijainen AssetFile sisällä kohteen yritettiin.
- Yritettiin luomiseen ContentKey määritetty tunnus on jo käytössä.
- Yritettiin luomiseen Locator määritetyn tunnus on jo käytössä.
- Useamman kuin yhden IngestManifestFile on määritetty nimi IngestManifest kuluessa.
- Yritettiin linkittäminen toisen tallennustilan salauksen ContentKey tallennustilan salattu kohteen.
- Linkittäminen saman ContentKey kohteen yritettiin.
- Voit luoda locator, jonka säilytykseen puuttuu tai ei ole enää liity kohteen omaisuuden yritettiin.
- Yritettiin locator, resurssi, joka on jo käytössä 5 locators luomiseen. (Azure-tallennustilan pakottaa sallitut viisi jaetun access-käytäntöjä yksi tallennustilan säilö.)
- Tallennustilan tilin amerikkalaisen linkittäminen IngestManifestAsset ei ole sama kuin ylemmän tason IngestManifest käyttämä tallennustila-tili.  

## <a name="500-internal-server-error"></a>500-Sisäinen palvelinvirhe

Pyynnön käsiteltäessä Media Services kohtaa joitakin virhe, joka estää jatkuvasta käsittely. Tämä virhe voi johtua seuraavista syistä:

- Resurssi tai projektin luominen epäonnistuu, koska Media Services-tilin palvelun kiintiötiedot on tilapäisesti poissa käytöstä.
- Resurssi tai IngestManifest blob-säilytykseen luominen epäonnistuu, koska asiakkaan tallennustilan tilin tiedot on tilapäisesti poissa käytöstä.
- Muu odottamaton virhe. 

## <a name="503-service-unavailable"></a>HTTP 503 – palvelu ei ole käytettävissä

Palvelin ei voi vastaanottaa pyyntöjä. Tämä virhe voi johtua liiallinen pyynnöt-palveluun. Media-palveluiden rajoittimen järjestelmä rajoittaa resurssien käyttö-sovelluksista, joihin liiallinen pyydettävä-palveluun.

>[AZURE.NOTE]Tarkista virhesanoma ja virheen koodimerkkijonon saat tarkempia tietoja syy, olet saanut 503 virhe. Tämä virhe ei aina tarkoita rajoitusta.

Mahdollinen tila kuvaukset ovat:

- "Palvelin on varattu. Edelliseen pyyntö tällaista peräkkäisiä kulunut yli {0} sekuntia."
- "Palvelin on varattu. Yli {0} pyynnöt sekunnissa voi olla rajoitettu."
- "Palvelin on varattu. Enemmän kuin {0} pyynnöt {1} muutamassa on rajoitettu."

Käsittelee tämän virheen, on suositeltavaa käyttää eksponentiaalisen Edellinen käytöstä uudelleen logiikan. Tämä tarkoittaa käyttämällä asteittain pidempi odottaa uudelleenyritykset peräkkäisen virhearvon vastausten välillä.  Lisätietoja on artikkelissa [Lyhytkestoisia vika käsittely sovelluksen estä](https://msdn.microsoft.com/library/hh680905.aspx). 

>[AZURE.NOTE]Jos käytössäsi on [Azure Media Services SDK.Net](https://github.com/Azure/azure-sdk-for-media-services/tree/master), yritä logiikan 503 virhe on toteutettu SDK.  
  
## <a name="see-also"></a>Katso myös  

[Media Services-palveluiden virhekoodit hallinta](http://msdn.microsoft.com/library/windowsazure/dn167016.aspx)

## <a name="next-steps"></a>Seuraavat vaiheet

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

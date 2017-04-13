<properties 
    pageTitle="Sisällön avain luvan käytännön Azure-portaalissa | Microsoft Azure" 
    description="Opettele määrittämään todennus-käytäntö sisältö-näppäimen." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/12/2016" 
    ms.author="juliako"/>



#<a name="configure-content-key-authorization-policy"></a>Sisällön avaimen luvan käytännön määrittäminen
[AZURE.INCLUDE [media-services-selector-content-key-auth-policy](../../includes/media-services-selector-content-key-auth-policy.md)]


##<a name="overview"></a>Yleiskatsaus

Microsoft Azure Media-palveluiden avulla voit pitää MPEG-VIIVA, tasainen Streaming ja HTTP-Live-virtautetun median (HLS) virtaa salaus AES (Advanced Standard) (joko 128 bittistä salausavaimet) tai [Microsoft PlayReady DRM](https://www.microsoft.com/playready/overview/)suojattu. AMS avulla voit pitää VIIVA virtaa Widevine DRM salattu. Yleisiä salaus (ISO/IEC 23001 7 CENC) määrityksen kohden salattuja PlayReady ja Widevine.

Media-palveluiden sisältää myös **Avaimen/käyttöoikeuden toimituksen palvelun** josta asiakkaat voivat hankkia AES avaimet tai PlayReady/Widevine käyttöoikeuksia, jotta voit toistaa salattuja sisältöä.

Tässä ohjeaiheessa kerrotaan, miten Azure portaalin sisällön avaimen todennus-käytännön määrittäminen. Avaimen myöhemmin voidaan salata dynaamisesti sisältöä. Huomaa, että tällä hetkellä voit salata seuraavat streaming tiedostomuodot: HLS, MPEG VIIVA ja tasainen Streaming. Et voi salata streaming format HDS tai progressiivinen lataaminen.

Kun pelaaja pyytää virtaa, joka on määritetty dynaamisesti salata, Media Services käyttää määritetyn avain salaa dynaamisesti AES tai DRM salausta käyttämällä sisältöä. Purkaa virta player pyytää avain avaimen toimitus-palvelusta. Päättävän riippumatta siitä, onko käyttäjällä on oikeus hankkia avain palvelun arvioi luvan käytäntöjä, jonka olet määrittänyt näppäimen.


Jos aiot on useita sisällön näppäimiä tai määritä kuin avaimen toimituksen Media palvelut **Avain/käyttöoikeuden toimitus-palvelun** URL-osoite, käytä Media Services .NET SDK tai REST API.

[Sisällön avain luvan käytännön avulla Media Services .NET SDK määrittäminen](media-services-dotnet-configure-content-key-auth-policy.md)

[Sisällön avain luvan käytännön avulla Media Services REST API määrittäminen](media-services-rest-configure-content-key-auth-policy.md)

###<a name="some-considerations-apply"></a>Käytä seikkoja:

- Ainakin yksi streaming varattu yksikkö on muista voi käyttää Dynaaminen pakkaus ja dynaaminen salauksen. Lisätietoja on artikkelissa [miten Media-palvelun](media-services-portal-manage-streaming-endpoints.md).
- Oman resurssi on oltava mukautuvat bittitaajuus MP4s tai mukautuvat bittitaajuus tasainen Streaming-tiedostoja. Lisätietoja on artikkelissa [Käytä sijoituksen](media-services-encode-asset.md).
- Avaimen toimitus-palvelun välimuistiin 15 minuutin ContentKeyAuthorizationPolicy ja liittyvät objektit (käytäntöasetukset ja rajoituksia).  Jos luot ContentKeyAuthorizationPolicy ja määritä käyttämään "Tunnus" rajoitus, Testaa, ja Päivitä käytäntö "Avaa" rajoitus, kestää noin 15 minuuttia ennen kuin käytäntöä siirtyy käytännön "Avaa" versio.


##<a name="how-to-configure-the-key-authorization-policy"></a>Toimintaohje: avaimen todennus-käytännön määrittäminen

Avaimen todennus-käytännön määrittäminen, valitse **SISÄLLÖNSUOJAUS** -sivu.

Media-palveluiden tukee useita tapoja käyttäjät, jotka avaimen pyytää todennustapa. Sisällön avaimen todennus-käytäntö voi olla **avaaminen**, **tunnuksen**ja **IP** -käyttöoikeuksien rajoitukset (REST-tai .NET SDK on määritetty**IP** ).

###<a name="open-restriction"></a>Avaa rajoitus

**Avaa** rajoituksen tarkoittaa, että järjestelmä toimittaa avain kaikkien sellaisten käyttäjien, niiden tekijöistä avaimen pyynnön. Tämä rajoitus voi olla hyötyä testausta varten.

![OpenPolicy][open_policy]

###<a name="token-restriction"></a>Suojaustunnuksen rajoitus

Suojaustunnuksen rajoitettu käytännön täyttämistavan paina **tunnuksen** -painiketta.

**Suojaustunnuksen** rajoitettu käytäntö on liitettävä tunnuksen, joka on myöntänyt **Suojatun suojaustunnuksen Service** (STS). Media-palveluiden tukee tunnusten **Yksinkertaisen Web tunnusten** ([SWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2))-muodossa ja **JSON Web suojaustunnuksen** (JWT)-muodossa. Lisätietoja on artikkelissa [JWT suojaustunnuksen todennusta](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/).

Media-palveluiden ei tarjoa **Suojatun suojaustunnuksen palvelut**. Voit luoda mukautetun STS tai hyödyntää Microsoft Azure ACS ongelman tunnukset. Luoda määritetyn avaimen ja ongelman saatavat, jonka olet määrittänyt suojaustunnuksen rajoitus määritys on allekirjoitettu tunnuksen on oltava määritettynä STS. Avaimen toimituksen Media palvelut palauttaa salausavaimen asiakkaalle, jos tunnus on voimassa ja tunnuksen saatavat vastaavat määritetty sisällön avain. Lisätietoja on artikkelissa [Ongelman tunnusten käyttö Azure ACS](http://mingfeiy.com/acs-with-key-services).

Kun käytäntö määritetään **tunnuksen** rajoitettu, sinun on määritettävä **vahvistus-näppäintä**, **myöntäjä** ja **yleisön**arvot. Ensisijainen vahvistus-avain sisältää avain, joka on allekirjoitettu tunnuksen, myöntäjä on suojattu suojaustunnuksen palvelu, joka ongelmat tunnuksen. (Kutsutaan joskus laajuus) yleisön kuvataan tunnuksen palveluita tai resurssin tunnus sallii käyttöoikeus. Media-palveluiden avaimen toimitus-palvelu tarkistaa tunnuksen kyseiset arvot vastaavat arvot mallissa.

###<a name="playready"></a>PlayReady

Kun suojaat kanssa **PlayReady**sisältöä, sinun on määritettävä luvan käytännön asiaan on XML-merkkijono, joka määrittää PlayReady käyttöoikeus-mallin. Seuraavat käytäntö on määritetty oletusarvoisesti:

<PlayReadyLicenseResponseTemplate xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1">
      <LicenseTemplates>
        <PlayReadyLicenseTemplate><AllowTestDevices>true</AllowTestDevices>
          <ContentKey i:type="ContentEncryptionKeyFromHeader" />
          <LicenseType>Nonpersistent</LicenseType>
          <PlayRight>
            <AllowPassingVideoContentToUnknownOutput>Allowed</AllowPassingVideoContentToUnknownOutput>
          </PlayRight>
        </PlayReadyLicenseTemplate>
      </LicenseTemplates>
    </PlayReadyLicenseResponseTemplate>

Voit **tuoda käytännön xml** -painiketta ja antaa eri XML joka mukainen XML-rakenne määritellään [tähän](https://msdn.microsoft.com/library/azure/dn783459.aspx).


##<a name="next-step"></a>Seuraava vaihe

Tarkista oppimispolut Media-palveluita.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]





[open_policy]: ./media/media-services-portal-configure-content-key-auth-policy/media-services-protect-content-with-open-restriction.png
[token_policy]: ./media/media-services-key-authorization-policy/media-services-protect-content-with-token-restriction.png


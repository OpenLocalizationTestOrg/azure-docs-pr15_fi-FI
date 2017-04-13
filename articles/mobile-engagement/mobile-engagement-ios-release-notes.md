<properties
    pageTitle="Azure Mobile välitys iOS SDK julkaisutiedot | Microsoft Azure"
    description="Uusimmat päivitykset ja ohjeet iOS SDK Azure Mobile välitys"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="09/12/2016"
    ms.author="piyushjo" />

#<a name="azure-mobile-engagement-ios-sdk-release-notes"></a>Azure Mobile välitys iOS SDK julkaisutiedot

##<a name="400-09122016"></a>4.0.0 (09/12/2016)

-   Kiinteä ilmoitus ei actioned iOS 10-laitteisiin.
-   Käytöstä XCode 7.

##<a name="324-06302016"></a>3.2.4 (06/30/2016)

-   Kiinteä kooste tekniset lokit ja muut lokit välillä.

##<a name="323-06072016"></a>3.2.3 (06/07/2016)

-   Kiinteä ohjelmavirhe, jossa toimituksen palaute ilmoitetaan ei, kun sovellus on käynnissä taustalla.
-   Optimoitu teknisen lokit lähettäminen.

##<a name="322-04072016"></a>3.2.2 (04/07/2016)

-   Kiinteä ohjelmavirhe, joka johtaa joskus kaatumisen HTTP pyynnön peruuttamisesta.

##<a name="321-12112015"></a>3.2.1 (12/11/2015)

-   Viive kiinteää uuden sovelluksen esiintymän käynnistämä ilmoituksen laaja linkit

##<a name="320-10082015"></a>3.2.0 (10/08/2015)

-   Käytössä Bitcode SDK: ssa, jotta se toimii **Xcode 7: n**kanssa.
-   Kiinteä-sovellusten ilmoitukset liittyvät virheet.
-   Tehtävä-sovellusten ilmoitusten luotettava vähäinen ja muissa tilanteissa.
-   Poistaa ylimääräiset konsolin 3 osapuolen kirjaston luomia lokeja.

##<a name="310-08262015"></a>3.1.0 (08/26/2015)

-   Korjaa iOS 9 yhteensopivuuden ohjelmavirhe kolmannen osapuolen kirjaston kanssa. Se on aiheuttaa kaatuu, kun lähettäminen tekee kyselyn tuloksia, hakemuksen tiedot tai lisätiedot.

##<a name="300-06192015"></a>3.0.0 (06-19-2015)

-   Mobile välitys käyttää hiljainen Push-ilmoitukset.
-   Pudottaminen tuki iOS 4.X. Sovelluksen käyttöönotto kohde lähtien tämän version on oltava vähintään iOS 6.

##<a name="220-05212015"></a>2.2.0 (05-21-2015)

-   Mobile välitys laitteen tunnus laitteiden < iOS 6 perustuu nyt GUID-tunnus luodaan asennuksen aikana.

##<a name="210-04242015"></a>2.1.0 (04-24-2015)

-   Lisätty nopeasti yhteensopivuus.
-   Ilmoituksen napsauttaminen URL-osoite on nyt toiminto suorittaa oikealle, kun sovellus avataan.
-   Lisätty puuttuvan otsikon tiedoston SDK-paketissa.
-   Kiinteä ongelma, kun Mobile välitys kaatumisen reporter on poistettu käytöstä.

##<a name="200-02172015"></a>2.0.0 (02-17-2015)

-   Ensimmäinen versio Azure Mobile välitys
-   sovelluksen tunnus/sdkKey määritys on korvattu merkkijonon yhteysmäärityksen.
-   Poistaa Ohjelmointirajapinnan lähettää ja vastaanottaa haluamaansa XMPP viestien haluamaansa XMPP kohteita.
-   Poistaa API voit lähettää ja vastaanottaa viestejä laitteiden välillä.
-   Parannettu suojaus.
-   SmartAd seuranta poistetaan.

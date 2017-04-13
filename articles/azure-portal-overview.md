<properties
    pageTitle="Microsoft Azure portaalin yleiskatsaus"
    description="Opettele käyttämään Microsoft Azure-portaalissa."
    services=""
    documentationCenter=""
    authors="davidwrede"
    manager="dwrede"
    editor="jimbe"/>

<tags
    ms.service="na"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="12/16/2015"
    ms.author="dwrede"/>

# <a name="microsoft-azure-portal-overview"></a>Microsoft Azure portaalin yleiskatsaus

Microsoft Azure-portaalissa on keskitetysti, jossa voit valmistella ja hallita Azure resursseja.  Tässä opetusohjelmassa voit tutustua portaalin ja kuinka voit käyttää seuraavia keskeisiä ominaisuuksia:
- **Täydellinen marketplace** , jonka avulla voit selata tuhansia kohteita Microsoftin ja muiden valmistajien, jotka on ostettu tai valmistelun yhteydessä.
- **Yhdistetty ja skaalattava Selaa-toiminto** , joka on helppo etsiä resursseja tärkeisiin ja suorittaa eri hallinta.
- **Yhtenäinen hallinnan sivut** (tai lavat), joiden avulla voit hallita Azure on erilaisia palveluihin johdonmukaisesti, paljastaa asetukset, toiminnot laskutuksen tiedot, kunnon seuranta- ja käyttö tietojen tai paljon enemmän.
- **Kokemus** , jolla voit luoda mukautettuja aloitusnäyttö, jossa näkyy tietoja, jotka haluat nähdä aina, kun kirjaudut sisään.  Voit myös mukauttaa jokin hallinta näiden, jotka sisältävät ruudut.

 ![Azure portaalin Käyttöliittymän suunta][UIOrientation]

## <a name="before-you-get-started"></a>Ennen aloittamista

Sinun on kelvollinen Azure tilaus tässä opetusohjelmassa käsitellään.  Jos sinulla ei ole yksi, valitse [Rekisteröidy maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/) tänään.  Kun tilaus, voit käyttää portaalissa osoitteessa [https://portal.azure.com].

## <a name="how-to-create-a-resource"></a>Resurssin luominen

Azure on marketplace kanssa tuhansia yhdestä paikasta luomiasi kohteita.  Oletetaan, että haluat luoda uuden Windows Server 2012 AM.  + Uusi pääsivusto on esitelty luokat Marketplacesta curated joukko käyttötapoja.  Jokaisella luokalla on pieni joukko esittelyssä sekä linkin koko Marketplace, jossa näkyy kaikissa luokissa ja Etsi. Luo, että uuden Windows Server 2012 AM tehdä seuraavat toimet:  

1.  Windows Server 2012 tarjottu, jotta voit valita sen Laske-luokasta.  
2.  Täytä lomakkeen joitakin basic syötteiden.
3.  Valitse Luo- ja oman AM alkaa valmistelu heti.

Ilmoitukset-toiminto ilmoittaa, kun resurssi on luotu ja hallinta-sivu avautuu (Voit aina selata resurssien myöhemmin).

![Portaalin luokat][PortalCategories]


## <a name="how-to-find-your-resources"></a>Resurssien etsiminen

Oman startboard aina kiinnittämällä usein käytetyt resurssit, mutta voit joutua muuttamaan jotakin, mitä ei usein käyttää selaamalla.  Selaa-toiminnossa alla on tapa siirtyä kaikki resurssit.  Voit suodattaa tilauksen, valitse/koon sarakkeita, ja siirry hallinta näiden valitsemalla yksittäisille kohteille.

![Selaa toiminnossa][BrowseHub]

## <a name="how-to-manage-and-delegate-access-to-a-resource"></a>Hallinta ja edustajakäyttöoikeuden resurssille

Valvoa suorituskyvyn mittarit tämä sivu, voit muodostaa yhteyden etätyöpöydän virtuaalikoneen, ohjaa tämän AM roolista Accessilla (RBAC), Määritä AM ja suorittaa muita tärkeitä hallintatehtäviä.  Delegoiminen perusteella rooli on ehdottoman tärkeää hallinta tasolla.  Napsauttamalla [tätä](./active-directory/role-based-access-control-configure.md) Saat lisätietoja sen. Resurssin edustajakäyttöoikeudet, voit tehdä seuraavat toimet:

1.  Siirry resurssi.
2.  Valitse "kaikkia asetuksia Essentials-osassa.
3.  Napsauta Asetukset-luettelosta "Käyttäjät".
4.  Valitse "Lisää" painikkeita.
5.  Valitse käyttäjä ja rooli.

![Resurssin hallinta][ManageResource]

## <a name="how-to-customize-a-resource-blade"></a>Voit mukauttaa resurssi-sivu

Azure salasanapohjaisella näiden resurssien, mutta näitä lavat ruutujen on teille ohjausobjektiin.  Voit siirtyä helposti kyselyjä Mukauta tilaa, lisätä, poistaa tai kokoa tai järjestää uudelleen ruudut. Voit mukauttaa sivu suorittaa seuraavat toimenpiteet:

1.  Siirry resurssi.
2.  Valitse "..." yläreunaan sivu, jonka haluat mukauttaa.
3.  Valitse "Lisää osia".
4.  Aloita vetäminen ja pudottaminen osat.  

![Lavat mukauttaminen][CustomizeBlades]

## <a name="how-to-get-help"></a>Ohjeiden saaminen

Jos sinulla on joskus on ongelmia, voimme puolestasi.  Portaalissa on Ohje ja tuki-sivu, voit osoittaa oikealle haluamaasi suuntaan.  Sen mukaan, että [tukevat palvelupaketti](https://azure.microsoft.com/support/plans/)voit myös luoda tukipyyntöjä suoraan Portalissa.  Kun olet luonut tuki lippu, voit hallita lippu portaalin elinkaari. Voit siirtyä Ohje ja tuki-sivun Selaa siirtymällä -> Ohje + tuki.  

![Ohje ja tuki][HelpSupport]

## <a name="summary"></a>Yhteenveto

Seuraavaksi tarkastellaan Tässä opetusohjelmassa asiat:
- Oppinut tilaa ja saat tilauksen Selaa-portaaliin
- Käytössä aloittaminen portaalin Käyttöliittymä ja opitut asiat luomisesta ja Selaa resurssit
- Oppimiasi resurssin luomisesta ja Selaa resurssit
- Rakenne- tai hallintatehtävät näiden ja miten voit johdonmukaisesti hallita erityyppisiä resursseja asiat
- Oppimiasi mukauttamisesta tiedot portaalin eteen ja center tärkeisiin
- Oppimiasi roolista Accessilla (RBAC) resurssien käyttöoikeuksien hallinta
- Oppimiasi hankkiminen Ohje ja tuki

Microsoft Azure-portaalin yksinkertaistaa jolla luomisesta ja hallinnasta sovellustesi pilveen.  Tutustu pitäminen ajan tasalla, kun sinut jatkuvasti [kuunteleminen palaute](https://feedback.azure.com/forums/223579-azure-preview-portal/) ja tehdä parannuksia [hallinta blogiin](https://azure.microsoft.com/blog/topics/management/) .  [ScottGu's blogi](http://weblogs.asp.net/scottgu) on toinen kätevä työkalu, voit etsiä kaikki Azure päivitykset.

[UIOrientation]: ./media/azure-portal-how-to-use/azure_portal_1.png
[PortalCategories]: ./media/azure-portal-how-to-use/azure_portal_2.png
[BrowseHub]: ./media/azure-portal-how-to-use/azure_portal_3.png
[ManageResource]: ./media/azure-portal-how-to-use/azure_portal_4.png
[CustomizeBlades]: ./media/azure-portal-how-to-use/azure_portal_5.png
[HelpSupport]: ./media/azure-portal-how-to-use/azure_portal_6.png

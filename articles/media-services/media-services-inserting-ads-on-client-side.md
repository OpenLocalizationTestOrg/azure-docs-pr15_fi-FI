<properties 
    pageTitle="Active Directory asiakaspuolen lisääminen | Microsoft Azure" 
    description="Tässä ohjeaiheessa esitellään mainosten asiakaspuolen lisäämisestä." 
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
    ms.date="09/26/2016" 
    ms.author="juliako"/>


#<a name="inserting-ads-on-the-client-side"></a>Asiakaspuolen mainokset lisääminen

Tässä artikkelissa on lisätietoja siitä, miten voit lisätä erilaisia mainosten asiakaspuolen.

Tietoja suljettu Live streaming videoiden tekstitys ja ad tuki on artikkelissa [Tuetut suljettu tekstitys ja Ad kohdistimen standardeja](media-services-live-streaming-with-onprem-encoders.md#cc_and_ads).

>[AZURE.NOTE] Azure Media Player ei tue mainokset tällä hetkellä.

##<a id="insert_ads_into_media"></a>Active Directory lisäämisestä mediatiedostojen

Azure Media Services tukee ad kohdistimen kautta Windows Media-ympäristössä: Playerin puitteissa. Windows 8, Silverlight, Windows Phone 8 ja iOS laitteisiin saatavilla olevia Playerin kehysten ad-tukeen. Kunkin Playerin framework on otoksen koodi, joka näytetään, miten voit toteuttaa toisto-ohjelmaa. On kolme erilaista, voit lisätä media: luettelon mainosten.

- **Lineaarinen** – koko kehyksen mainos, joka pysäyttää tärkeimmät videon.
- **Nonlinear** – kerros Active Directory, jotka näkyvät, kun tärkeimmät video toistetaan, yleensä logon tai muun staattisen kuvan sijoittaa toisto-ohjelman.
- **Avustaja** – mainokset, jotka näkyvät toisto-ohjelman ulkopuolella.

Active Directory voi asettaa milloin tahansa tärkeimmät video aikajana. Toisto-ohjelman täytyy ilmoittaa, milloin toistetaan mainos ja mitkä mainosten toistamiseen. Tämä on valmis käyttämällä vakio XML-pohjaisia tiedostoja: Video Ad palvelun malli (VAST), digitaalisen videon useita Ad soittoluettelo (VMAP), Media abstraktit jaksotetaan malli (LAPPU) ja digitaalisen videon toisto-ohjelman Ad käyttöliittymän määritys (VPAID). SUURIA tiedostoja Määritä, mitä mainosten näyttämiseen. VMAP tiedostot Määritä toiminnon toistaminen eri mainosten ja suuria XML sisältää. LAPPU tiedostot on sarjan Active Directory, joka voi sisältää myös suuria XML tapa. VPAID tiedostot Määritä käyttöliittymään videon toisto-ohjelman ja ad tai ad-palvelin.

Kunkin Playerin framework toimii eri tavalla ja kunkin kattaa erillisessä ohjeaiheessa. Tässä aiheessa kerrotaan Lisää Active Directory basic menetelmät. Videosoittimen sovellusten pyytää mainosten ad-palvelin. Ad-palvelin voi vastata usealla eri tavalla:

- Palauta suuria tiedosto
- Palauttaa VMAP tiedosto (on upotettu VAST)
- Palauttaa LAPPU tiedosto (on upotettu VAST)
- Palauttaa suuria tiedosto, jonka VPAID mainokset

 
###<a name="using-a-video-ad-service-template-vast-file"></a>Videon Ad palvelun malli (VAST)-tiedoston avulla

SUURIA tiedosto määrittää, mitä ad tai mainosten näyttämiseen. Seuraava XML-koodi on esimerkki lineaarinen ad suuria tiedoston:
    
    <VAST version="2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="oxml.xsd">
      <Ad id="115571748">
        <InLine>
          <AdSystem version="2.0 alpha">Atlas</AdSystem>
          <AdTitle>Unknown</AdTitle>
          <Description>Unknown</Description>
          <Survey></Survey>
          <Error></Error>
          <Impression id="Atlas"><![CDATA[http://www.myserver.com/tracking-resource]]></Impression>
          <Creatives>
            <Creative id="video" sequence="0" AdID="">
              <Linear>
                <Duration>00:00:32</Duration>
                <TrackingEvents>
                  <Tracking event="start"><![CDATA[http://www.myserver.com/start-tracking-resource]]></Tracking>
                  <Tracking event="midpoint"><![CDATA[http://www.myserver.com/midpoint-tracking-resource]]></Tracking>
                  <Tracking event="complete"><![CDATA http://www.myserver.com/complete-tracking-resource]]></Tracking>
                  <Tracking event="expand"><![CDATA[http://www.myserver.com/expand-tracking-resource]]></Tracking>
                </TrackingEvents>
                <VideoClicks>
                  <ClickThrough id="Atlas Redirect"><![CDATA[http://www.myserver.com/click-resource]]></ClickThrough>
                  <ClickTracking id="Spare"></ClickTracking>
                </VideoClicks>
                <MediaFiles>
                  <MediaFile apiFramework="Windows Media" id="windows_progressive_200" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="200" width="400" height="300" type="video/x-ms-wmv">
                    <![CDATA[http://www.myserver.com/media/myad_200_4x3.wmv]]>
                  </MediaFile>
                  <MediaFile apiFramework="Windows Media" id="windows_progressive_300" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="300" width="400" height="300" type="video/x-ms-wmv">
                    <![CDATA[http://www.myserver.com/media/myad_300_4x3.wmv]]>
                  </MediaFile>
                </MediaFiles>
              </Linear>
            </Creative>
          </Creatives>
          <Extensions>
            <Extension type="Atlas">
            </Extension>
          </Extensions>
        </InLine>
      </Ad>
    </VAST>
    
Lineaarinen ad kuvaaman **<Linear>** elementti. Se määrittää mainos keston, tapahtumien seuranta, valitse kautta, valitse seuranta ja useita **<MediaFile>** osat. Määritettyjen tapahtumien seuranta **<TrackingEvents>** elementin ja Salli ad-palvelin eri tapahtumia, jotka ilmenevät tarkasteltaessa mainos. Tässä tapauksessa Käynnistä-painiketta, keskipiste-valmiina, ja laajenna tapahtumien seurataan. Käynnistä tapahtuma tapahtuu, kun näkyviin tulee mainos. Keskipiste tapahtuma tapahtuu, kun vähintään 50 prosenttia ad-aikajana on katsottu. Valmis tapahtuman tapahtuu, kun mainos on suoritettu loppuun. Laajenna tapahtuma tapahtuu, kun käyttäjä laajenee videon toisto-ohjelman koko näytön kokoiseksi. Clickthroughs on määritetty **<ClickThrough>** elementissä **<VideoClicks>** elementin ja määrittää resurssin URI-tunnus tulee näkyviin, kun käyttäjä napsauttaa mainos. ClickTracking on määritetty **<ClickTracking>** elementti, myös toimintoa **<VideoClicks>** elementin ja määrittää seuranta resurssin Playerin pyytää, kun käyttäjä napsauttaa mainos. **<MediaFile>** Elementtien määrittää tietyn koodaus kuvasarjan tietoja. Kun kyseessä on useampi kuin yksi **<MediaFile>** elementti, videon toisto-ohjelman voit valita parhaan koodaus-ympäristöön. 

Lineaarinen mainokset voidaan näyttää määritetyllä tavalla. Voit tehdä tämän Lisää muita <Ad> elementit VAST tiedoston ja määrittää järjestyksen, järjestys-määritteen avulla. Seuraavassa esimerkissä havainnollistetaan näin:
    
    <VAST version="2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="oxml.xsd">
      <Ad id="1" sequence="0">
        <InLine>
          <AdSystem version="2.0 alpha">Atlas</AdSystem>
          <AdTitle>Unknown</AdTitle>
          <Description>Unknown</Description>
          <Survey></Survey>
          <Error></Error>
          <Impression id="Atlas"><![CDATA[http://myserver.com/Impression/Ad1trackingResouce]]></Impression>
          <Creatives>
            <Creative id="video" sequence="0" AdID="">
              <Linear>
                <Duration>00:00:32</Duration>
                <MediaFiles>
                  <!-- ... -->
                </MediaFiles>
              </Linear>
            </Creative>
          </Creatives>
        </InLine>
      </Ad>
      <Ad id="2" sequence="1">
        <InLine>
          <AdSystem version="2.0 alpha">Atlas</AdSystem>
          <AdTitle>Unknown</AdTitle>
          <Description>Unknown</Description>
          <Survey></Survey>
          <Error></Error>
          <Impression id="Atlas"><![CDATA[http://myserver.com/Impression/Ad2trackingResouce]]></Impression>
          <Creatives>
            <Creative id="video" sequence="0" AdID="">
              <Linear>
                <Duration>00:00:30</Duration>
                <MediaFiles>
                  <!-- ... -->
                </MediaFiles>
              </Linear>
            </Creative>
          </Creatives>
        </InLine>
      </Ad>
    </VAST>
    
Nonlinear Active Directory on määritetty <Creative> sekä elementti. Seuraavassa esimerkissä <Creative> elementti, joka kuvaa nonlinear ad.

    <Creative id="video" sequence="1" AdID="">
      <NonLinearAds>
        <NonLinear width="216" height="121" minSuggestedDuration="00:00:15">
          <StaticResource creativeType="image/png"><![CDATA[http://myserver/images/image.png]]></StaticResource>
          <StaticResource creativeType="image/jpg"><![CDATA[http://myserver/images/image.jpg]]></StaticResource>
        </NonLinear>
        <TrackingEvents>
             <Tracking event="acceptInvitation"><![CDATA[http://myserver/tracking/trackingID]></Tracking>
             <Tracking event="collapse"><![CDATA[http://myserver/tracking/trackingID2]]></Tracking>
         </TrackingEvents>
       </NonLinearAds>
    </Creative>

 
**<NonLinearAds>** Elementti voi sisältää vähintään yhden **<NonLinear>** osat, joihin voidaan kuvata nonlinear ad. **<NonLinear>** Elementti määrittää resurssin nonlinear ad. Resurssi voidaan **<StaticResouce>**, **<IFrameResource>**, tai **<HTMLResouce>**.**<StaticResource>** HTML resurssin kuvaus ja määrittää creativeType-määrite, joka määrittää resurssin näkymän:

Kuva/gif, kuva tai jpeg, kuva tai png – resurssin näkyy HTML- **<img>** tunniste.

Sovelluksen/x-javascript – resurssin näkyy <**script**> HTML-tunniste.

Sovelluksen/x-shockwave-flash – resurssin näkyy Flash Player-ohjelmassa.

**<IFrameResource>**Tässä artikkelissa kuvataan, jotka voivat näkyä IFRAME HTML-resurssin. **<HTMLResource>**Tässä artikkelissa kuvataan osan HTML-koodi, joka voidaan lisätä verkkosivulle. **<TrackingEvents>**Määritä seuranta-tapahtumien ja pyytää tapahtuman toteutuessa URI. Tässä esimerkissä seurataan acceptInvitation ja Kutista-tapahtumat. Lisätietoja **<NonLinearAds>** elementin ja sen alisivustojen käyttöä rajoitetaan artikkelissa IAB.NET/VAST. Huomaa, että **<TrackingEvents>** elementti on sijaitsevat** <NonLinearAds> ** elementin sijaan **<NonLinear>** elementti.

Avustaja Active Directory on määritetty sisällä <CompanionAds> elementti. <CompanionAds> Elementti voi sisältää vähintään yhden <Companion> osat. Kunkin <Companion> kuvataan avustaja ad ja voivat sisältää <StaticResource>, <IFrameResource>, tai <HTMLResource> joka on määritetty samalla tavalla kuin nonlinear ad. SUURIA tiedosto voi sisältää useita avustaja mainosten ja player-sovelluksen voit valita sopivimman ad esittämiseksi. Saat lisätietoja VAST [suuria 3.0](http://www.iab.net/media/file/VASTv3.0.pdf).

###<a name="using-a-digital-video-multiple-ad-playlist-vmap-file"></a>Digitaalisen videon useita Ad soittoluettelo (VMAP)-tiedoston avulla

VMAP-tiedoston avulla voit määrittää, milloin ad vaihdot suoritettiin, kuinka kauan kunkin sivunvaihto on, kuinka monta mainokset voidaan näyttää sisällä tauon ja mainosten tyypit voidaan näyttää tauon aikana. Esimerkki VMAP tiedostosta, joka määrittää yksittäisen ad tauko seuraavasti:
    
    <vmap:VMAP xmlns:vmap="http://www.iab.net/vmap-1.0" version="1.0">
      <vmap:AdBreak breakType="linear" breakId="mypre" timeOffset="start">
        <vmap:AdSource allowMultipleAds="true" followRedirects="true" id="1">
          <vmap:VASTData>
            <VAST version="2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="oxml.xsd">
              <Ad id="115571748">
                <InLine>
                  <AdSystem version="2.0 alpha">Atlas</AdSystem>
                  <AdTitle>Unknown</AdTitle>
                  <Description>Unknown</Description>
                  <Survey></Survey>
                  <Error></Error>
                  <Impression id="Atlas"><![CDATA[http://view.atdmt.com/000/sview/115571748/direct;ai.201582527;vt.2/01/634364885739970673]]></Impression>
                  <Creatives>
                    <Creative id="video" sequence="0" AdID="">
                      <Linear>
                        <Duration>00:00:32</Duration>
                        <MediaFiles>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_200" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="200" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_1_000_200_4x3.wmv]]>
                          </MediaFile>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_300" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="300" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_2_000_300_4x3.wmv]]>
                          </MediaFile>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_500" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="500" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_1_000_500_4x3.wmv]]>
                          </MediaFile>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_700" maintainAspectRatio="true" scaleable="true" delivery="progressive" bitrate="700" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_2_000_700_4x3.wmv]]>
                          </MediaFile>
                        </MediaFiles>
                      </Linear>
                    </Creative>
                  </Creatives>
                </InLine>
              </Ad>
            </VAST>
          </vmap:VASTData>
        </vmap:AdSource>
        <vmap:TrackingEvents>
          <vmap:Tracking event="breakStart">
            http://MyServer.com/breakstart.gif
          </vmap:Tracking>
        </vmap:TrackingEvents>
      </vmap:AdBreak>
    </vmap:VMAP>
     
VMAP tiedoston alussa <VMAP> elementti, joka sisältää vähintään yhden <AdBreak> osat-määrittäminen ad-sivunvaihto. Jokaisen ad tauon määrittää osanvaihdon tyyppi, tauko-tunnus ja ajan siirtymän. BreakType-määrite määrittää ad voidaan toistaa aikana valitsemalla sivunvaihdon: lineaarinen, nonlinear, tai näyttää. Näytä Active Directory-kartan suuria avustaja mainosten. Useamman kuin yhden ad tyyppi voidaan määrittää CSV (ilman välilyöntejä)-luettelosta. BreakID on valinnainen tunniste mainos. TimeOffset määrittää, milloin mainos näytetään. Se voidaan määrittää jollakin seuraavista tavoista:

1. Aika – ss tai hh:mm:ss.mmm muodossa, missä .mmm millisekuntia. Tämän määritteen arvo määrittää ajan videon aikajanan alusta alkuun ad-sivunvaihto.
1. Prosentti – n % muodossa jossa n on videon aikajanan ennen toistaminen mainos toistaminen
1. Aloita/Lopeta – määrittää, että mainos näytetään ennen tai jälkeen videon näytetään
1. Sijoita – määrittää ad vaihdot järjestystä, kun ad-vaihdot ajoituksen on tuntematon, esimerkiksi live streaming. Jokaisen ad tauon järjestyksen on määritetty #n-muodossa, jossa n on kokonaisluku, 1 tai suurempi. 1 osoittaa, että kyseessä ad soitettavat ensimmäisen mahdollisuuden 2 ilmaisee mainos soitettavat toisen mahdollisuuden ja niin edelleen.

<**AdBreak**> elementissä voi olla jokin <**AdSource**> elementti. <**AdSource**> elementti sisältää seuraavat ominaisuudet:

1. Tunnus – määrittää ad lähteen tunnus
1. allowMultipleAds – totuusarvon, joka määrittää, näkyvätkö useita mainosten ad-vaihdon aikana
1. followRedirects – valinnainen totuusarvon, joka määrittää, jos videon toisto-ohjelman olisi tukee ohjaa ad-vastauksen sisällä

<**AdSource**> elementti on Playerin tekstiin-ad vastauksen tai viittaus ad-vastauksen. Se voi sisältää seuraavat osat:

- <VASTAdData>Ilmaisee suuria ad vastaus on upotettu VMAP-tiedosto
- <AdTagURI>URI, joka viittaa ad-vastauksen toisesta järjestelmästä
- <CustomAdData>– tapahtui haluamaansa merkkijono, respresents-suuria vastaus

Tässä esimerkissä sitoutuvat ad-vastauksen määritetään <VASTAdData> elementti, joka sisältää suuria ad vastausta. Saat lisätietoja muiden osien [VMAP](http://www.iab.net/guidelines/508676/digitalvideo/vsuite/vmap).

<**AdBreak**> elementti voi sisältää myös <**TrackingEvents**> elementti. <**TrackingEvents**>-osan avulla voit seurata alussa tai lopussa ad-sivunvaihto tai onko ad-sivunvaihto tapahtui virhe. <**TrackingEvents**>-elementti sisältää vähintään yhden <**Seuranta**> elementit, joista jokaisella määrittää seuranta-tapahtuma- ja seuranta URI. Mahdollisia seuranta tapahtumat ovat:

1. breakStart – seuraa ad-sivunvaihto alkuun
1. breakEnd – seurata, ad-sivunvaihto valmiiksi
1. Virhe – seuraa ad-vaihdon aikana tapahtuneen virheen

Seuraavassa esimerkissä VMAP-tiedosto, joka määrittää tapahtumien seuranta

    <vmap:VMAP xmlns:vmap="http://www.iab.net/vmap-1.0" version="1.0">
      <vmap:AdBreak breakType="linear" breakId="mypre" timeOffset="start">
        <vmap:AdSource allowMultipleAds="true" followRedirects="true" id="1">
          <vmap:VASTData>
            <!--Inline VAST -->
          </vmap:VASTData>
        </vmap:AdSource>
        <vmap:TrackingEvents>
          <vmap:Tracking event="breakStart">
            http://MyServer.com/breakstart.gif
          </vmap:Tracking>
          <vmap:Tracking event="breakend">
            http://MyServer.com/breakend.gif
          </vmap:Tracking>
          <vmap:Tracking event="error">
            http://MyServer.com/error.gif
          </vmap:Tracking>
        </vmap:TrackingEvents>
      </vmap:AdBreak>
    </vmap:VMAP>

Katso lisätietoja <**TrackingEvents**>-elementin ja sen alisivustojen käyttöä rajoitetaan http://iab.org/VMAP.pdf

###<a name="using-a-media-abstract-sequencing-template-mast-file"></a>Media-tiivistelmä jaksotetaan mallitiedosto (LAPPU) avulla

LAPPU-tiedoston avulla voit määrittää käynnistimien, jotka määrittävät, milloin mainos on näkyvissä. Seuraavassa on esimerkki LAPPU tiedostosta, joka sisältää vuotta kuvat-ad, keskellä kuvat ad ja jälkeinen kuvat ad Käynnistimet.

    <MAST xsi:schemaLocation="http://openvideoplayer.sf.net/mast http://openvideoplayer.sf.net/mast/mast.xsd" xmlns="http://openvideoplayer.sf.net/mast" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
      <triggers>
        <trigger id="preroll" description="preroll every item"  >
          <startConditions>
            <condition type="event" name="OnItemStart" />
          </startConditions>
          <sources>
            <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
              <sources />
            </source>
          </sources>
        </trigger>
    
        <trigger id="midroll" description="midroll at 15 sec."  >
          <startConditions>
            <condition type="property" name="Position" value="00:00:15.0" operator="GEQ" />
          </startConditions>
          <endConditions>
            <condition type="event" name="OnItemEnd"/>
            <!--This 'resets' the trigger for the next clip-->
          </endConditions>
          <sources>
            <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
              <sources />
            </source>
          </sources>
        </trigger>
    
        <trigger id="postroll" description="postroll"  >
          <startConditions>
            <condition type="event" name="OnItemEnd"/>
          </startConditions>
          <sources>
            <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
              <sources />
            </source>
          </sources>
        </trigger>
      </triggers>
    </MAST>

 

LAPPU tiedoston alussa **<MAST>** elementti, joka sisältää yhden **<triggers>** elementti. <triggers> Elementti sisältää vähintään yhden **<trigger>** osat, jotka määrittävät, milloin mainos esitetään. 

**<trigger>** Elementti sisältää **<startConditions>** elementti, joka määrittää, milloin mainos toistaminen alkaa. **<startConditions>** Elementti sisältää vähintään yhden <condition> osat. Kun kunkin <condition> käynnistimen käynnistetään tai peruuttaa sen mukaan, onko tosi <condition> sisältyy **< startConditions**> tai **<endConditions>** elementin tarpeen mukaan. Kun useita <condition> osat ovat näkyvissä, niitä käsitellään implisiittinen tai, TOSI arvioimisen ehdon aiheuttaa käynnistin aloittaa. <condition>osat voivat olla ylemmän tason. Kun lapsen <condition> osat ovat valmiit, niitä käsitellään implisiittinen ja, kaikki ehdot on arvo on TOSI, jos käynnistin aloittaa. <condition> Elementti sisältää ehdon määrittävät määritteet: 

1. **Kirjoita** – määrittää ehtoa, tapahtuma tai ominaisuus
1. **nimi** – ominaisuus tai tapahtuman aikana arviointi
1. **arvo** , arvo, joka lasketaan vastaan ominaisuus
1. **operaattori** – arvioinnin yhdistämisessä käytettävä toiminto: EQ (yhtä suuri kuin), NEQ (eri suuri kuin), GTR (suurempi), GEQ (suurempi tai yhtä), LT (pienempi kuin), Ekvivalenttinen (pienempi tai yhtä), MOD (modulo)

**<endConditions>**sisältää myös <condition> osat. Kun ehto on tosi käynnistin palautetaan. <trigger> Elementti on myös <sources> elementti, joka sisältää vähintään yhden <source> osat. <source> Osat määrittävät URI ad-vastauksen ja ad vastauksen tyyppi. Tässä esimerkissä URI-tunnus annetaan suuria vastausta. 


    <trigger id="postroll" description="postroll"  >
      <startConditions>
        <condition/>
      </startConditions>
      <sources>
        <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
          <sources />
        </source>
      </sources>
    </trigger>
 

###<a name="using-video-player-ad-interface-definition-vpaid"></a>Videon toisto-Ad käyttöliittymän määrityksen (VPAID) avulla

VPAID on suoritettava ad yksiköiden yhteydessä videon toisto-ohjelman ottaminen käyttöön API. Näin vuorovaikutteinen ad kokemukset. Käyttäjä voi käsitellä mainos ja mainos vastata Viewerin toimet. Esimerkiksi mainos saattaa näkyä painikkeita, joiden avulla käyttäjä voi tarkastella lisätietoja tai pidempi ad-versiota. Videon toisto-ohjelman on tuettava VPAID Ohjelmointirajapinnan ja suoritettavan ad on pantava täytäntöön Ohjelmointirajapinnan. Kun pelaaja pyytää mainos ad-palvelimesta palvelimeen saattaa toimia ja suuria vastaus, joka sisältää VPAID ad.

Suoritettava ad luodaan tunnus, jolla on suoritettava runtime-ympäristössä, kuten Adobe Flash™ tai JavaScript voi suorittaa selaimessa. Kun ad-palvelin palauttaa suuria vastauksen, joka sisältää VPAID ad-arvo apiFramework määritteen <MediaFile> elementti on oltava "VPAID". Tämän määritteen määrittää, että sisältämät ad VPAID suoritettavan ad. Tyyppi-määrite on määritettävä suoritettavan tiedoston, kuten "sovelluksen/x-shockwave-flash" tai "sovelluksen/x-javascript-MIME-tyypin. XML-koodikatkelman seuraavassa <MediaFile> suuria vastauksen sisältävän VPAID suoritettavan ad osan. 

    
    <MediaFiles>
       <MediaFile id="1" delivery="progressive" type=”application/x-shockwaveflash”
                  width=”640” height=”480” apiFramework=”VPAID”>
           <!-- CDATA wrapped URI to executable ad -->
       </MediaFile>
    </MediaFiles>
 

Suoritettava ad voi alustaa käyttämällä <AdParameters> sisällä <Linear> tai <NonLinear> suuria vastauksen osat. Lisätietoja <AdParameters> elementti, katso [suuria 3.0](http://www.iab.net/media/file/VASTv3.0.pdf). Saat lisätietoja VPAID Ohjelmointirajapinnan [VPAID 2.0](http://www.iab.net/media/file/VPAID_2.0_Final_04-10-2012.pdf).

##<a name="implementing-a-windows-or-windows-phone-8-player-with-ad-support"></a>Käyttöönoton Windows- tai Windows Phone 8 pelaaja Ad-tuki

Microsoft Media-ympäristö: Playerin Framework Windows 8- tai Windows Phone 8 sisältää otoksen sovellukset, jotka näyttävät videosoittimen-sovellus, joka käyttää puitteissa ottamisesta käyttöön on joukko. Voit ladata Playerin Framework ja mallit [Player Framework Windows 8](https://playerframework.codeplex.com)-ja Windows Phone 8.

Kun avaat Microsoft.PlayerFramework.Xaml.Samples-ratkaisun näet määrä kansiota projektissa. Mainonta-kansio on esimerkki koodi verkkosivustojen luomiseen videoiden toisto ad-tukeen. Mainostaminen sisällä kansio on XAML/cs tiedostojen määrä, joka näyttää mainokset lisäämisestä eri tavalla. Seuraavassa luettelossa kuvataan kunkin:

- AdPodPage.xaml näytetään, miten ad-pod näytettävä.
- AdSchedulingPage.xaml näyttää ajoittamisesta mainosten.
- FreeWheelPage.xaml esitetään, kuinka voit järjestää Active Directory FreeWheel-lisäosan avulla.
- MastPage.xaml näyttää ajoittamisesta mainosten LAPPU-tiedoston.
- ProgrammaticAdPage.xaml näyttää ajoittamisesta ohjelmallisesti mainosten videoksi.
- ScheduleClipPage.xaml näyttää ajoittamisesta mainos ilman suuria tiedostoa.
- VastLinearCompanionPage.xaml näytetään, miten voit lisätä lineaarinen ja avustaja ad.
- VastNonLinearPage.xaml näytetään, miten voit lisätä-lineaarisen ad.
- VmapPage.xaml näytetään, miten voit määrittää Active Directory VMAP-tiedoston.

Jokainen näitä esimerkkejä käyttää Playerin framework määrittämiä MediaPlayer-luokka. Useimmat esimerkkejä, joiden avulla laajennukset, jotka lisäävät ad-vastauksen erimuotoisia tuki. ProgrammaticAdPage esimerkki toimii ohjelmallisesti MediaPlayer esiintymää.

###<a name="adpodpage-sample"></a>AdPodPage Esimerkki

Tässä esimerkissä AdSchedulerPlugin määrittää näyttämään mainos. Tässä esimerkissä esitetään 5 sekunnin kuluttua on varattu keskellä kuvat-ilmoitus. Ad-pod (mainosten näyttää järjestyksessä ryhmä) on määritetty suuria tiedoston palautettu ad-palvelimesta. SUURIA tiedoston URI on määritetty <RemoteAdSource> elementti.

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
    
        <mmppf:MediaPlayer.Plugins>
            <ads:AdSchedulerPlugin>
                <ads:AdSchedulerPlugin.Advertisements>
    
                    <ads:MidrollAdvertisement Time="00:00:05">
                        <ads:MidrollAdvertisement.Source>
                            <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_adpod.xml" Type="vast"/>
                        </ads:MidrollAdvertisement.Source>
                    </ads:MidrollAdvertisement>
    
                </ads:AdSchedulerPlugin.Advertisements>
            </ads:AdSchedulerPlugin>
            <ads:AdHandlerPlugin/>
        </mmppf:MediaPlayer.Plugins>
    </mmppf:MediaPlayer>

Katso lisätietoja siitä, AdSchedulerPlugin [mainonta Playerin puitteissa Windows 8: ssa ja Windows Phone 8](http://playerframework.codeplex.com/wikipage?title=Advertising&referringTitle=Windows%208%20Player%20Documentation)

###<a name="adschedulingpage"></a>AdSchedulingPage

Tässä esimerkissä käytetään myös AdSchedulerPlugin. Kokouksen kolme Active Directory, vanhat kuvat ad, keskellä kuvat ad ja jälkeinen kuvat ad. URI VAST AD määritetty <RemoteAdSource> elementti.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>
    
                            <ads:PrerollAdvertisement>
                                <ads:PrerollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" Type="vast"/>
                                </ads:PrerollAdvertisement.Source>
                            </ads:PrerollAdvertisement>
    
                            <ads:MidrollAdvertisement Time="00:00:15">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" Type="vast"/>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>
    
                            <ads:PostrollAdvertisement>
                                <ads:PostrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" Type="vast"/>
                                </ads:PostrollAdvertisement.Source>
                            </ads:PostrollAdvertisement>
    
                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>


###<a name="freewheelpage"></a>FreeWheelPage

Tässä esimerkissä käytetään FreeWheelPlugin, joka määrittää lähdemäärite, joka määrittää URI-tunnus, joka osoittaa SmartXML tiedostoon, joka määrittää ad sisältöä sekä ad ajoitustiedot.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:FreeWheelPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/freewheel.xml"/>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

###<a name="mastpage"></a>MastPage

Tässä esimerkissä käytetään MastSchedulerPlugin, jonka avulla voit käyttää LAPPU-tiedostoa. Lähde-määrite määrittää LAPPU-tiedoston sijainti.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:MastSchedulerPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/mast.xml" />
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

###<a name="programmaticadpage"></a>ProgrammaticAdPage

Tässä esimerkissä toimii ohjelmallisesti MediaPlayer kanssa. ProgrammaticAdPage.xaml tiedoston muodostaa MediaPlayer:

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4"/>

ProgrammaticAdPage.xaml.cs tiedoston Luo AdHandlerPlugin TimelineMarker, voit määrittää lisää kun mainos näytetään ja lisää käsittelytoiminto MarkerReached tapahtuman joka lataa RemoteAdSource, URI suuria tiedostoon, joka määrittää, ja toistaa sitten mainos.
    
    public sealed partial class ProgrammaticAdPage : Microsoft.PlayerFramework.Samples.Common.LayoutAwarePage
        {
            AdHandlerPlugin adHandler;
    
            public ProgrammaticAdPage()
            {
                this.InitializeComponent();
                adHandler = new AdHandlerPlugin();
                player.Plugins.Add(new AdHandlerPlugin());
                player.Markers.Add(new TimelineMarker() { Time = TimeSpan.FromSeconds(5), Type = "myAd" });
                player.MarkerReached += pf_MarkerReached;
            }
    
            async void pf_MarkerReached(object sender, TimelineMarkerRoutedEventArgs e)
            {
                if (e.Marker.Type == "myAd")
                {
                    var adSource = new RemoteAdSource() { Type = VastAdPayloadHandler.AdType, Uri = new Uri("http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml") };
                    //var adSource = new AdSource() { Type = DocumentAdPayloadHandler.AdType, Payload = SampleAdDocument };
                    var progress = new Progress<AdStatus>();
                    try
                    {
                        await player.PlayAd(adSource, progress, CancellationToken.None);
                    }
                    catch { /* ignore */ }
                }
            }

###<a name="scheduleclippage"></a>ScheduleClipPage

Tässä esimerkissä AdSchedulerPlugin ajoittaa keskellä kuvat ad määrittämällä .wmv-tiedosto, joka sisältää mainos.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.cloudapp.net/html5/media/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>
    
                            <ads:MidrollAdvertisement Time="00:00:05">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:AdSource Type="clip">
                                        <ads:AdSource.Payload>
                                            <ads:ClipAdPayload MediaSource="http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_2_000_700_4x3.wmv" MimeType="video/x-ms-wmv" />
                                        </ads:AdSource.Payload>
                                    </ads:AdSource>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>
    
                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

###<a name="vastlinearcompanionpage"></a>VastLinearCompanionPage

Tässä esimerkissä kuvataan, miten ajoittaminen keskellä kuvat lineaarinen ad kanssa avustaja-ad AdSchedulerPlugin avulla. <RemoteAdSource> Elementti määrittää suuria-tiedoston sijainti.
    
    <mmppf:MediaPlayer Grid.Row="1"  x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>
    
                            <ads:MidrollAdvertisement Time="00:00:05">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear_companions.xml" Type="vast"/>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>
    
                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

###<a name="vastlinearnonlinearpage"></a>VastLinearNonLinearPage

Tässä esimerkissä käytetään ajoittaa lineaarinen AdSchedulerPlugin ja ei-lineaarisen ad. SUURIA tiedostosijainti määritetään <RemoteAdSource> elementti.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>
    
                            <ads:MidrollAdvertisement Time="00:00:05">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear_nonlinear.xml" Type="vast"/>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>
                            
                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

###<a name="vmappage"></a>VMAPPage

Tämän esimerkin ajoittaminen VMAP tiedoston Active Directory VmapSchedulerPlugin avulla. Lähde-määrite on määritetty VMAP tiedoston URI <VmapSchedulerPlugin> elementti.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:VmapSchedulerPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/vmap.xml"/>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

##<a name="implementing-an-ios-video-player-with-ad-support"></a>Käyttöönoton iOS videon toisto-ohjelman Ad-tuki


Microsoft Media-ympäristöä: Playerin Framework iOS on kokoelma otoksen sovellukset, jotka näyttävät videosoittimen-sovellus, joka käyttää puitteissa ottamisesta käyttöön. Player-Framework ja malleja voi ladata [Azure Media Playerin Framework](https://github.com/Azure/azure-media-player-framework). Github-sivulla on linkki, joka sisältää lisätietoja Playerin framework Wikin ja Playerin otosten esittely: [Azure Media Playerin Wiki](https://github.com/Azure/azure-media-player-framework/wiki/How-to-use-Azure-media-player-framework).


###<a name="scheduling-ads-with-vmap"></a>Ajoituksen mainosten VMAP kanssa

Seuraavassa esimerkissä esitetään ajoittamisesta mainosten VMAP-tiedoston avulla.

    // How to schedule an Ad using VMAP.
    //First download the VMAP manifest
    
    if (![framework.adResolver downloadManifest:&manifest withURL:[NSURL URLWithString:@"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVMAP.xml"]])
            {
                [self logFrameworkError];
            }
            else
            {
                // Schedule a list of ads using the downloaded VMAP manifest
                if (![framework scheduleVMAPWithManifest:manifest])
                {
                    [self logFrameworkError];
                }          
            }

###<a name="scheduling-ads-with-vast"></a>Ajoituksen mainosten VAST kanssa

Seuraavassa esimerkissä esitetään, kuinka voit ajoittaa Myöhäinen sidonta suuria ad.
    
    //Example:3 How to schedule a late binding VAST ad.
    // set the start time for the ad
    adLinearTime.startTime = 13;
    adLinearTime.duration = 0;
    // Specify the URI of the VAST file
    NSString *vastAd1=@"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVAST.xml";
    // Create an AdInfo object
     AdInfo *vastAdInfo1 = [[[AdInfo alloc] init] autorelease];
    // set URL to VAST file
    vastAdInfo1.clipURL = [NSURL URLWithString:vastAd1];
    // set running time of ad
    vastAdInfo1.mediaTime = [[[MediaTime alloc] init] autorelease];
    vastAdInfo1.mediaTime.clipBeginMediaTime = 0;
    vastAdInfo1.mediaTime.clipEndMediaTime = 10;
    vastAdInfo1.policy = @"policy for late binding VAST";
    // specify ad type
    vastAdInfo1.type = AdType_Midroll;
    vastAdInfo1.appendTo=-1;
    adIndex = 0;
    // schedule ad
    if (![framework scheduleClip:vastAdInfo1 atTime:adLinearTime forType:PlaylistEntryType_VAST andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }
         
   Seuraavassa esimerkissä esitetään, kuinka voit ajoittaa Aikainen sidonta suuria ad.
Esimerkki: 4 aikataulu Aikainen sidonta suuria ad //Download VAST mahdollisesti tiedostoa (! [ framework.adResolver downloadManifest: ja tiedostojen withURL: [NSURL URLWithString:@"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVAST.xml"]]) {[itse logFrameworkError];} muita {adLinearTime.startTime = 7; adLinearTime.duration = 0;
        
        // Create AdInfo instance
        AdInfo *vastAdInfo2 = [[[AdInfo alloc] init] autorelease];
        vastAdInfo2.mediaTime = [[[MediaTime alloc] init] autorelease];
        vastAdInfo2.policy = @"policy for early binding VAST";
        // specify ad type
        vastAdInfo2.type = AdType_Midroll;
        vastAdInfo2.appendTo=-1;
        // schedule ad
        if (![framework scheduleVASTClip:vastAdInfo2 withManifest:manifest atTime:adLinearTime andGetClipId:&adIndex])
        {
            [self logFrameworkError];
        }
    }

Seuraavassa esimerkissä esitetään, kuinka voit lisätä mainos käyttämällä karkea Leikkaa muokkaaminen (RCE)

    //Example:1 How to use RCE.
    // specify manifest for ad content
    NSString *secondContent=@"http://wamsblureg001orig-hs.cloudapp.net/6651424c-a9d1-419b-895c-6993f0f48a26/The%20making%20of%20Microsoft%20Surface-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    
    // specify ad length
    mediaTime.currentPlaybackPosition = 0;
    mediaTime.clipBeginMediaTime = 0;
    mediaTime.clipEndMediaTime = 80;
    // append ad content
    if (![framework appendContentClip:[NSURL URLWithString:secondContent] withMediaTime:mediaTime andGetClipId:&clipId])
    {
        [self logFrameworkError];
    }

Seuraavassa esimerkissä esitetään ajoittamisesta ad-pod.

    //Example:5 Schedule an ad Pod.
    // Set start time for ad
    adLinearTime.startTime = 23;
    adLinearTime.duration = 0;
    
    // Specify URL to content
    NSString *adpodSt1=@"https://portalvhdsq3m25bf47d15c.blob.core.windows.net/asset-e47b43fd-05dc-4587-ac87-5916439ad07f/Windows%208_%20Cliffjumpers.mp4?st=2012-11-28T16%3A31%3A57Z&se=2014-11-28T16%3A31%3A57Z&sr=c&si=2a6dbb1e-f906-4187-a3d3-7e517192cbd0&sig=qrXYZBekqlbbYKqwovxzaVZNLv9cgyINgMazSCbdrfU%3D";
    // Create an AdInfo instance
    AdInfo *adpodInfo1 = [[[AdInfo alloc] init] autorelease];
    // set URI to ad content
    adpodInfo1.clipURL = [NSURL URLWithString:adpodSt1];
    // Set ad running time
    adpodInfo1.mediaTime = [[[MediaTime alloc] init] autorelease];
    adpodInfo1.mediaTime.clipBeginMediaTime = 0;
    adpodInfo1.mediaTime.clipEndMediaTime = 17;
    adpodInfo1.policy = @"policy for ad pod 1";
    // Set ad type
    adpodInfo1.type = AdType_Midroll;
    adpodInfo1.appendTo=-1;
    adIndex = 0;
    // Schedule ad
    if (![framework scheduleClip:adpodInfo1 atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

Seuraavassa esimerkissä esitetään ajoittamisesta-muistilappuja keskellä kuvat ad. Muistilappuja ad toistetaan vain, kun riippumatta siitä, mitä tahansa hakemista viewer suorittaa.
    
    //Example:6 Schedule a single non sticky mid roll Ad
    // specify URL to content
    NSString *oneTimeAd=@"http://wamsblureg001orig-hs.cloudapp.net/5389c0c5-340f-48d7-90bc-0aab664e5f02/Windows%208_%20You%20and%20Me%20Together-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    
    // create an AdInfo instance
    AdInfo *oneTimeInfo = [[[AdInfo alloc] init] autorelease];
    // set URL of ad
    oneTimeInfo.clipURL = [NSURL URLWithString:oneTimeAd];
    oneTimeInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    oneTimeInfo.mediaTime.clipBeginMediaTime = 0;
    oneTimeInfo.mediaTime.clipEndMediaTime = -1;
    oneTimeInfo.policy = @"policy for one-time ad";
    // set ad start time
    adLinearTime.startTime = 43;
    adLinearTime.duration = 0;
    // set ad type
    oneTimeInfo.type = AdType_Midroll;
    // non sticky ad
    oneTimeInfo.deleteAfterPlayed = YES;
    // schedule ad
    if (![framework scheduleClip:oneTimeInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

Seuraavassa esimerkissä esitetään ajoittamisesta muistilappuja keskellä kuvat ad. Muistilappuja ad tulee näkyviin aina, kun kohdan videon aikajanan on saavutettu.

    //Example:7 Schedule a single sticky mid roll Ad
    NSString *stickyAd=@"http://wamsblureg001orig-hs.cloudapp.net/2e4e7d1f-b72a-4994-a406-810c796fc4fc/The%20Surface%20Movement-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    // create AdInfo instance
    AdInfo *stickyAdInfo = [[[AdInfo alloc] init] autorelease];
    // set URI to ad
    stickyAdInfo.clipURL = [NSURL URLWithString:stickyAd];
    stickyAdInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    stickyAdInfo.mediaTime.clipBeginMediaTime = 0;
    stickyAdInfo.mediaTime.clipEndMediaTime = 15;
    stickyAdInfo.policy = @"policy for sticky mid-roll ad";
    // set ad start time
    adLinearTime.startTime = 64;
    adLinearTime.duration = 0;
    // set ad type
    stickyAdInfo.type = AdType_Midroll;
    stickyAdInfo.deleteAfterPlayed = NO;
    // schedule ad
    if (![framework scheduleClip:stickyAdInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }


Seuraavassa esimerkissä esitetään, kuinka voit ajoittaa jälkeinen kuvat ad.

    //Example:8 Schedule Post Roll Ad
    NSString *postAdURLString=@"http://wamsblureg001orig-hs.cloudapp.net/aa152d7f-3c54-487b-ba07-a58e0e33280b/wp-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    // create AdInfo instance
    AdInfo *postAdInfo = [[[AdInfo alloc] init] autorelease];
    postAdInfo.clipURL = [NSURL URLWithString:postAdURLString];
    postAdInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    postAdInfo.mediaTime.clipBeginMediaTime = 0;
    // set ad length
    postAdInfo.mediaTime.clipEndMediaTime = 45;
    postAdInfo.policy = @"policy for post-roll ad";
    // set ad type
    postAdInfo.type = AdType_Postroll;
    adLinearTime.duration = 0;
    if (![framework scheduleClip:postAdInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

Seuraavassa esimerkissä esitetään, kuinka voit ajoittaa vanhat kuvat ad.
    
    //Example:9 Schedule Pre Roll Ad
    NSString *adURLString = @"http://wamsblureg001orig-hs.cloudapp.net/2e4e7d1f-b72a-4994-a406-810c796fc4fc/The%20Surface%20Movement-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    AdInfo *adInfo = [[[AdInfo alloc] init] autorelease];
    adInfo.clipURL = [NSURL URLWithString:adURLString];
    adInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    adInfo.mediaTime.currentPlaybackPosition = 0;
    adInfo.mediaTime.clipBeginMediaTime = 40; //You could play a portion of an Ad. Yeh!
    adInfo.mediaTime.clipEndMediaTime = 59;
    adInfo.policy = @"policy for pre-roll ad";
    adInfo.appendTo = -1;
    adInfo.type = AdType_Preroll;
    adLinearTime.duration = 0;
    // schedule ad
    if (![framework scheduleClip:adInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

Seuraavassa esimerkissä esitetään, kuinka voit ajoittaa keskellä kuvat kerros ad.
    
    // Example10: Schedule a Mid Roll overlay Ad
    NSString *adURLString = @"https://portalvhdsq3m25bf47d15c.blob.core.windows.net/asset-e47b43fd-05dc-4587-ac87-5916439ad07f/Windows%208_%20Cliffjumpers.mp4?st=2012-11-28T16%3A31%3A57Z&se=2014-11-28T16%3A31%3A57Z&sr=c&si=2a6dbb1e-f906-4187-a3d3-7e517192cbd0&sig=qrXYZBekqlbbYKqwovxzaVZNLv9cgyINgMazSCbdrfU%3D";
    //Create AdInfo instance
    AdInfo *adInfo = [[[AdInfo alloc] init] autorelease];
    adInfo.clipURL = [NSURL URLWithString:adURLString];
    adInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    adInfo.mediaTime.currentPlaybackPosition = 0;
    adInfo.mediaTime.clipBeginMediaTime = 0;
    // specify ad length
    adInfo.mediaTime.clipEndMediaTime = 20;
    adInfo.policy = @"policy for mid-roll overlay ad";
    adInfo.appendTo = -1;
    // specify ad type
    adInfo.type = AdType_Midroll;
    // specify ad start time & duration
    adLinearTime.startTime = 300;
    adLinearTime.duration = 20;
    // schedule ad            if (![framework scheduleClip:adInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }



##<a name="media-services-learning-paths"></a>Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

 
##<a name="see-also"></a>Katso myös

[Videosoittimen sovellusten kehittämiseen](media-services-develop-video-players.md)

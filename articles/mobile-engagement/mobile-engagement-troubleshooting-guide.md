<properties 
   pageTitle="Azure Mobile välitys vianmääritys apuviivat" 
   description="Azure Mobile välitys oppaan vianmääritys" 
   services="mobile-engagement" 
   documentationCenter="" 
   authors="piyushjo" 
   manager="dwrede" 
   editor=""/>

<tags
   ms.service="mobile-engagement"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="mobile-multiple"
   ms.workload="mobile" 
   ms.date="08/19/2016"
   ms.author="piyushjo"/>

# <a name="azure-mobile-engagement---troubleshooting-guide"></a>Azure Mobile välitys - Vianmääritysoppaan käyttäminen

## <a name="introduction"></a>Johdanto
Seuraava vianmäärityksen opas auttaa ymmärtämään joitakin usein näkyy ja pääkansion syitä, joiden avulla voit vianmääritys Omat muistiinpanot. 

## <a name="general"></a>Yleiset

Yleensä aina Varmista seuraavasti:

1. Varmista, että ovat kadonneet läpi kaikki vaiheet, jotka tarvitaan integroinnin kuvatulla tavalla Microsoftin [aloittaminen opetusohjelmat](mobile-engagement-windows-store-dotnet-get-started.md)
2. Käytössäsi on uusin käyttöympäristö SDK: T. 
3. Testaa todellista laitteen ja emulaattorin, koska asioita, jotka ovat vain emulaattorin. 
4. Ovat ei pallolla kaikki rajoitukset tai ylikuormitustilan Mobile välitys-joka on kuvattu [seuraavassa](../azure-subscription-service-limits.md)
5. Jos et ole pystyttävä muodostamaan yhteys Mobile välitys palvelun taustatietokannan tai pelkkiä tietoja ei ladattavasta jatkuvasti sitten varmistaa, että valitsemalla ovat ei ole jatkuvaa palvelutapahtumia [tähän](https://azure.microsoft.com/status/)

## <a name="monitor-issues"></a>"Näyttöjä"

### <a name="i-am-not-seeing-my-device-showing-up-on-the-monitor-tab"></a>Ei näy laitteeseeni valvonta-välilehdessä näkyvät
Valvonta-välilehdessä näkyy reaaliajassa Mobile välitys omaa ympäristöäsi liitettyjen laitteiden. Jos korjattava emulaattorin ja laitteessa, pitäisi näkyä vähintään yksi istunto tähän. Jos sovellus on jaettu, sinun tulee näkyviin vastaavat laitteet, jotka ovat yhteydessä reaaliajassa ympäristö istuntojen mittari. 

Jos et näe laitteen näyttö-välilehdessä on todennäköisesti SDK integrointi ongelma. Joitakin yleisiä ohjeita vianmäärityksessä ovat seuraavat:

1. Varmista, että käytät oikeaa yhteysmerkkijono-mobiilisovelluksessa ja SDK näppäimet-osassa ja ei Ohjelmointirajapinta näppäimet-osio on. Yhteysmerkkijonon muodostaa mobile-sovellus Mobile välitys-sovellus, joka tulee näkyviin valvonta-välilehdessä laitteen esiintymään. 
2. Windows-ympäristön – Jos sivusi ohittaa `OnNavigatedTo` menetelmä, varmista, että Soita `base.OnNavigatedTo(e)`.
3. Jos Mobile välitys ovat integroiminen aiemmin mobiilisovelluksen sitten voit varmistaa, että sinun ei puuttuu toimenpiteet katsomalla kehittyneitä integraatio vaiheet [tähän](mobile-engagement-windows-store-integrate-engagement.md)
4. Varmista lähetät vähintään yhden näytön/tehtävän mukaan ohittaminen EngagementActivity sivun ympäristö työskentelyn [aloittaminen opetusohjelmat](mobile-engagement-windows-store-dotnet-get-started.md)ohjeiden mukaan.

### <a name="i-am-seeing-the-monitor-tab-showing-a-session-even-when-i-have-disconnected-or-closed-my-app-emulator"></a>Valvonta-välilehti, jossa näkyvät istunnon tulee näkyviin myös kun minulla on katkennut tai suljettu sovelluksen / emulaattorin. 
Yhdistetty platform tässä vaiheessa vain yksi ja käytössäsi on emulaattorin Avaa sovellus sitten tämä on todennäköisesti emulaattorin elementtimäärityksen vuoksi. Yleensä haluat varmistaa, että palaat aloitusnäytössä app istunnon yhteyden katkaiseminen onnistuneesti emulointiohjelma. Lisäksi Windows-käyttöjärjestelmässä korjattaessa Visual Studiossa, joudut ehkä varmistaa, että Visual Studiossa **Elinkaari tapahtumien** valikkoriviltä Siirry ja valitse Sulje istunto todella **keskeytys** . [Windows-opetusohjelma](mobile-engagement-windows-store-dotnet-get-started.md) lisätietoja. 

## <a name="analytics-issues"></a>"Analytics-ongelmien vianmääritys

### <a name="i-am-not-seeing-any-data-refreshed-data-on-analytics-tab"></a>Minulla ei tulee näkyviin mitään tietoja / Analytics-välilehdessä tietojen päivittäminen 
Analytics-tiedot lasketaan uudelleen säännöllisesti ja se voi kestää enintään 24 tunnin tämän päivitykseen. Nämä tiedot eivät ole reaaliaikainen ja näet sitä päivittää sisällä tämän 24 tunnin ajanjakso.
Varmista, että kuitenkin, että lähetät vähintään yksi näyttö tai tehtävän ympäristö Taustajärjestelmä mukaan joko ohittavaa vähintään yksi sivu, jossa `EngagementActivity` tai soittamista `SendActivity` explcitly. 

### <a name="i-am-seeing-incorrectly-captured-datetime-for-a-device-on-the-analytics-tab"></a>Käyttäjäprofiilissa virheellisesti siepatun päivämäärä ja kellonaika laitteen Analytics-välilehdessä
Tunnin ajanjakso perustuu käyttäjien laiteasetusten päivämäärä. Varmista niin, että laitteessa on määritetty oikein päivämäärä. 

## <a name="segment-issues"></a>'Segmentin' ongelmien vianmääritys

### <a name="i-created-a-segment-and-it-is-showing-up-as-greyed-out-or-not-showing-any-data"></a>Luotu segmentin ja se on tule näkyviin, kun harmaana tai se ei näy mitään tietoja
Osion luominen ei ole reaaliaikainen tällä hetkellä. Se lasketaan samanaikaisesti, kun analytics-tiedot koostetaan ja siten voi kestää enintään 24 tuntia. Sinun on otettava takaisin myöhemmin, mutta tänä Varmista myös mobile-sovellukset ovat varmasti tietojen perusteella osat ovat muodostaen lähettämisen. Esimerkiksi Jos tapahtuma kestää sano "tapahtuman nimenä" ei ole lähetetty mukaan tahansa mobiililaitteessa sitten osion, joka on luotu käyttämällä EventName segmentin mitään tietoja ei voi = tapahtuman nimenä ehto. Kannattaa myös tarkistaa SDK-integrointi varmistaa mobile-sovellus lähettää tiedot oikein. 

## <a name="reach-or-push-notifications-issues"></a>Saavuttaa' tai Push-ilmoitukset

### <a name="my-push-messages-are-not-being-delivered"></a>Push-viestit toimitetaan ei 

1. Yritä lähettää ilmoitukset testi-laitteeseen, ensimmäisen kerran, jotta voit varmistaa, että kaikki osat - mobiilisovelluksen, SDK ja -palvelu on yhdistetty oikein ja voi pitää push-ilmoitukset. 
2. Lähetä helpoin 'sovelluksen ilmoitus"aina ensin markkinointikampanjan, joka ei ole ajoitettu ja eikä se on määritetty yleisön minkä tahansa ehdon. Tämä on uudelleen todistaa ilmoituksen yhteyden toimii oikein. 
3. Jos sinulla on ongelmia-välittää-sovellusten ilmoitukset myös on hyvä ensimmäiseksi, yritä lähettää sovelluksen ilmoituksen ensin. 
4. Varmista, että 'alkuperäisen Push' on oikein määritetty for mobile-sovellus. Käyttöympäristön mukaan se joko sisältyä näppäimet (Android, Windows) tai varmenteet (iOS). Tarkasteluoikeudet [- asetukset](mobile-engagement-user-interface-settings.md)
5. Ulos sovelluksesta ilmoitukset saattavat myös estää käyttäjän kautta mobile OS siten varmistaa näin ei ole. 
6. Varmista, että haluat ei määrittää *Ohita yleisön push lähetetään käyttäjien Ohjelmointirajapinnan kautta* -vaihtoehto Reach markkinointikampanjan **Markkinointikampanja** -osassa koska Näin varmistat, että push-ilmoitukset voi vain lähetetään ohjelmointirajapinnan kautta. 
7. Varmista, että testaat push-markkinointikampanjan sekä WIFI ja puhelimen operaattori verkon poistamiseksi mahdollista lähteenä ongelmia verkkoyhteyden kautta liitetyn laitteen kanssa.
8. Varmista, että järjestelmän päivämäärä ja kellonaika laitteen/emulointiohjelma ovat oikein, koska synkronoitu laitteeseen myös häiritä Push-Ilmoituspalvelun 's mahdollisuus toimittaa ilmoitukset. 

Lisää ympäristö tietyn vianmääritys alla oleviin asennusohjeisiin:

1. **iOS** 

    - Varmista, että varmenteet on voimassa ja jäljellä iOS Push-ilmoitukset. 
    - Varmista, että oikein määritetään *tuotannon* varmenteen Mobile välitys-sovelluksessa. 
    - Varmista, että testaat käyttöön *reaali fyysinen laite.* IOS-simulator ei voi käsitellä push-viestit.
    - Varmista, että Pikaoppaista tunniste on määritetty oikein mobiilisovelluksessa. Katso ohjeet [tähän](https://developer.apple.com/library/prerelease/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6)
    - Kun testataan, "Ad Hoc-jakaumaa käytetään mobile valmistelu profiiliin. Ei voi vastaanottaa ilmoituksen, jos sovellus on käännetty käyttämällä "Virheenkorjaus"

2. **Android-laitteeseen**

    - Varmista, että olet määrittänyt oikein projektinumero oman mobiilisovelluksen AndroidManifest.xml tiedostossa sen \n-merkin perässä. 
    
            <meta-data android:name="engagement:gcm:sender" android:value="************\n" />
        
    - Varmista, että eivät näy tai määritetty väärin oikeuksia Android luettelon-tiedostossa 
    - Varmista, että olet lisäämässä asiakkaan sovelluksen projektinumero on saman tilin kohtaa, johon olet saanut GCM Server-näppäintä. Minkä tahansa kahden toisiaan estää oman työntää siirtymällä. 
    - Jos vastaanotat järjestelmäilmoituksia, mutta ei sovelluskohtaisesta sitten [Määritä ilmoitukset-osiossa kuvakkeen](mobile-engagement-android-get-started.md) sellaisena kuin se on todennäköisesti Tarkista oikea kuvake on määrität ei Android luettelon-tiedosto. 
    - Jos lähettää ilmoituksen BigPicture, varmista, että jos käytät ulkoisia kuva palvelimia sitten tarvitsemansa voivat tukea HTTP "Tulee" ja "Otsikko".

3. **Windows**
    
    - Varmista, että liittämäsi sovelluksen kelvollinen Windows-kaupan sovellus. Visual Studio - joudut projektin napsauttamalla hiiren kakkospainikkeella ja valitse "Liitä sovelluksen kanssa Store"-vaihtoehto ja valitse luomasi Windows-kaupan sovellus. Windows-kaupan sovelluksen on oltava sama kuin kohtaa, johon olet saanut alkuperäisen push-tunnistetietojen määrittäminen Mobile välitys-portaalissa.
    - Jos vastaanotat sovelluksen push-ilmoitukset, mutta ei-sovellusten ilmoitukset, joiden `EngagementOverlay` sitten integrointi varmistaa on ruudukon pääelementti-sivulle. EngagementOverlay käyttää ensimmäinen xaml-tiedostossa, voit lisätä kaksi web lukee "Ruudukon" elementti näkymät-sivulla. Jos haluat etsiä, jossa web-näkymien määrittäminen, voit määrittää ruudukon nimeltä "EngagementGrid", kuten Tämä kuitenkin sinun on varmistettava on riittävä korkeuden ja leveyden kaksi myöhemmin Web-näkymät, joka näyttää ilmoituksen ja -sovelluksen ilmoituksella kuin seuraava ilmoitus:
        
            <Grid x:Name="EngagementGrid"></Grid>

### <a name="i-created-a-push-notificationannouncement-campaign-and-even-after-it-sent-me-the-notification-it-is-showing-as-active-what-does-it-mean"></a>Push-ilmoituksen/ilmoitus luotu/kampanja ja senkin jälkeen, kun se lähettää minulle ilmoitus, se on näkyvissä "Aktiivinen". Mitä se tarkoittaa? 
**Markkinointikampanjan** , jonka loit Mobile välitys kutsutaan, jotta koska se on uusissa laitteissa yhdistäminen mobile välitys omaa ympäristöäsi pitkään käynnissä push-ilmoituksen merkitystä, ne automaattisesti lähetetään ilmoitus, voit määrittää, kunhan, kun he täyttävät ehdon, voit määrittää markkinointikampanja. Tämä on näyttökuva yksittäisen ilmoituksen yhden asetukset. Sinun on manuaalisesti valitsemalla Lopeta markkinointikampanja niin, että se ei lähetä edelleen ilmoitukset **Valmis** -painiketta. 

### <a name="i-created-a-push-campaign-and-i-am-receiving-notifications-successfully-however-whenever-i-open-up-the-app-i-get-the-same-notification-even-when-i-had-actioned-it-before"></a>Push-markkinointikampanjan luotu ja vastaanoteta ilmoitukset onnistuneesti kuitenkin aina, kun avaan sovelluksen ylös, saan saman ilmoituksen myös silloin, kun ollut actioned ennen? 
Tämä tapahtuu todennäköisesti testauksen aikana ja jos käytät emulointeja tai joitakin Testaa framework, kuten TestFlight. Mitä tapahtuu näin app jokaisen esiintymän Suorita-palvelussa on uusi DeviceID hankkiminen ja lähettäminen Microsoftin taustatietokannan, joka aiheuttaa Mobile välitys-ympäristössä, jotta Käsittele uusi laite ja lähettää ilmoituksen. 

## <a name="getting-support"></a>Tuen saaminen

Jos et voi ratkaista ongelman itse voit tehdä seuraavia toimia:

1. Etsi ongelmaa aiemmin viestiketjussa StackOverflow keskustelupalstalla ja [MSDN-keskustelupalsta](https://social.msdn.microsoft.com/Forums/windows/en-US/home?forum=azuremobileengagement) ja jos ei sitten esittää kysymyksen siellä. 
2. Jos löydät puuttuu sitten Lisää tai äänestä meidän [Palautesivuston keskustelupalstalla](https://feedback.azure.com/forums/285737-mobile-engagement/) pyynnön ominaisuus
3. Jos sinulla on Microsoft tue Avaa tuki tapahtumaa toimittamalla seuraavat asiat: 
    - Azure tilauksen tunnus
    - Ympäristössä (esimerkiksi iOS-Android jne.)
    - Sovellustunnus
    - Markkinointikampanjan tunnus (push ilmoituksen ongelmat)
    - Laitteen tunnus
    - Mobile välitys SDK-versio (kuten Android SDK v2.1.0)
    - Tarkka virhesanoma ja skenaarion virhetiedot

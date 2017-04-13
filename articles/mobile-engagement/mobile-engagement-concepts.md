<properties
    pageTitle="Mobile välitys käsitteitä | Microsoft Azure"
    description="Azure Mobile välitys käsitteitä"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="azure-mobile-engagement-concepts"></a>Azure Mobile välitys käsitteitä

Mobile välitys määrittää muutamia yleisiä käsitteitä kaikki tuetut käyttöympäristöt. Tässä artikkelissa kuvataan lyhyesti näiden käsitteiden.

Tässä artikkelissa on hyvä aloitus, jos ole aiemmin käyttänyt Mobile välitys. Varmista myös, että lue ohjeet koskevat ympäristö käytössäsi on, kuin se tarkentaa käsitteistä, joka on kuvattu tämän artikkelin ja lisää tiedot ja esimerkit sekä mahdolliset rajoitukset.

## <a name="devices-and-users"></a>Laitteet ja käyttäjät
Mobile välitys tunnistaa käyttäjät luodaan laitteilta yksilöllinen. Tämän tunnisteen kutsutaan laite-tunnus (tai `deviceid`). Luo siten, että kaikki sovellusten toiminnan samaan laitteeseen jakaa sama laitteen tunnus.

Implisiittisesti se tarkoittaa, että Mobile välitys katsoo yhteen laitteeseen kuuluu täsmälleen yhden käyttäjän ja näin ollen käyttäjille ja laitteille on vastaava käsitteitä.

## <a name="sessions-and-activities"></a>Istunnot ja toiminnot
Istunnon yhden käyttäjän suorittamat sovelluksen käyttö, siitä, kun käyttäjä aloittaa käyttää sitä, kunnes käyttäjä lopettaa.

Tehtävä on yksi käyttö tietyn aliraportti yhden käyttäjän suorittamat sovelluksen osa (se on yleensä näyttöön, mutta se voi olla mikä tahansa sopivan sovellukseen).

Käyttäjä voi suorittaa vain yksi tehtävä kerrallaan.

Tehtävän nimi (rajoitettu 64 merkkiä) tunnistetaan ja voit upottaa ylimääräisiä tietoja (enintään 1 024 tavua).

Istunnot automaattisesti lasketaan-toiminnot, jotka suoritetaan käyttäjien järjestystä. Istunnon käynnistyy, kun käyttäjä käynnistyy ensimmäisen toimintaansa ja pysähtyy, kun hän on valmis viimeisen toimintaansa. Tämä tarkoittaa, että istunnon ei tarvitse erikseen käynnistää tai pysäyttää. Toiminnot ovat sen sijaan erikseen aloittaa tai pysäyttää. Jos aktiviteettia ilmoitetaan, ei ole istunnon ilmoitetaan.

## <a name="events"></a>Tapahtumat
Tapahtumien käytetään raportin pikaviestien toiminnot (kuten-painike painettuna tai käyttäjien lukea artikkeleita).

Tapahtuman voi liittyä istunnon käytössä olevaan projektiin, tai se voi olla erillinen tapahtuma.

Tapahtuman tunnistetaan (rajoitettu 64 merkkiä) nimi ja halutessasi upottaa ylimääräisiä tietoja (enintään 1 024 tavua).

## <a name="error"></a>Virhe
Virheet käytetään raportoida ongelmista oikein havaitsemien sovelluksen (kuten virheellinen käyttäjän toimet, tai Ohjelmointirajapinnan kutsu epäonnistuu).

Virhe voi liittyä istunnon käytössä olevaan projektiin, tai se voi olla erillinen virhe.

Virhe tunnistetaan (rajoitettu 64 merkkiä) nimi ja halutessasi upottaa ylimääräisiä tietoja (enintään 1 024 tavua).

## <a name="job"></a>Työn
Töiden käytetään raportin ottaa kestoa toiminnot (kuten API-kutsujen kesto, Näytä Active Directory-aika, taustatehtävien kestoa tai tehtävien kestoa).

Työn ei liity istuntoon, koska tehtävän voi suorittaa taustalla ilman käyttäjän toimia.

Työn tunnistetaan (rajoitettu 64 merkkiä) nimi ja halutessasi upottaa ylimääräisiä tietoja (enintään 1 024 tavua).

## <a name="crash"></a>Kaatumisen
Kaatuu myöntävät automaattisesti Mobile välitys SDK ilmoittamaan sovellusvirheet, jossa sovellus ei tunnista ongelmat ansiosta kaatua.

## <a name="application-information"></a>Hakemuksen tiedot
Hakemuksen tiedot (tai sovelluksen tiedot) käytetään tunnisteen käyttäjille, eli yhdistää tietoja (tämä muistuttaa web-evästeet, paitsi että app tiedot tallentuvat palvelinpuolen Azure Mobile välitys-ympäristössä) sovelluksen käyttäjille.

Sovelluksen tiedot voidaan rekisteröidä Mobile välitys SDK-Ohjelmointirajapinnan käyttäminen tai käyttämällä laitteen API Mobile välitys-ympäristössä.

Sovelluksen tiedot on liitetty laitteeseen avain/arvo-pari. Avain on nimi (enintään 64 ASCII kirjaimia [a-zA-Z], [0-9] numeroita ja alaviivoja [_])-sovelluksen tiedot. Arvo (rajoitettu 1 024 merkkiä) voi olla mikä tahansa merkkijono, kokonaisluku, päivämäärä (yyyy-MM-dd) tai totuusarvon (tosi tai EPÄTOSI).

Sovelluksen tiedot määrää voi liittää laitteeseen, määritetty Mobile välitys hinnoittelutiedot ehtojen mukaan rajoissa. Yksi annetun avaimen Mobile välitys säilyttää vain niiden uusimman arvojoukko (ei historiatietoja). Pakottaa Mobile välitys lasketaan uudelleen yleisön ehdot määritetty tämän sovelluksen tiedot (jos saatavilla) eli sovelluksen näitä tietoja voidaan käyttää käynnistettävän reaaliaikainen työntää määrittäminen tai muuttaminen arvosta sovelluksen tiedot.

## <a name="extra-data"></a>Lisätiedot
Lisätiedot (tai lisäominaisuudet) on satunnaisia tietoja, jotka voidaan liittää tapahtumia, virheitä, tehtävät ja työt.

Lisäominaisuudet selkeä rakenne JSON objektien vastaavasti: ne tehdään puun avain/arvo-pareina. Näppäimet on oikeutettu enintään 64 ASCII kirjaimia [a-zA-Z], [0-9] numeroita ja alaviivoja [_]) ja lisäominaisuudet kokonaiskoko on 1 024 merkkiä (kerran koodattu JSON Mobile välitys SDK mukaan).

Koko puun avain/arvo paria tallennetaan JSON-objekteina. Kuitenkin vain ensimmäisen tason avaimien ja arvojen on hajonneen suoraan käytettävissä joidenkin edistyneiden funktioiden, kuten osia (esimerkiksi voit helposti määrittää nimeltä "SciFi tuulettimista", joka on tehty, kaikki käyttäjät, jotka on lähetetty vähintään 10 kertaa tapahtuman nimeltä "content_viewed" segmentin kanssa ylimääräisiä avaimen "content_type" arvo "scifi" viime kuussa). On siis suositeltavaa lähetetään vain lisäominaisuudet tehdä yksinkertaisia luetteloiden avain/arvo paria skalaarinen arvojen (esimerkiksi merkkijonoja, päivämäärät, kokonaisluvut tai totuusarvo).

## <a name="next-steps"></a>Seuraavat vaiheet

- [Windows SDK-paketissa yleinen yleistä Azure Mobile välitys](mobile-engagement-windows-store-sdk-overview.md)
- [Windows Phone Silverlight SDK yleistä Azure Mobile välitys](mobile-engagement-windows-phone-sdk-overview.md)
- [iOS SDK Azure Mobile välitys](mobile-engagement-ios-sdk-overview.md)
- [Android SDK Azure Mobile välitys](mobile-engagement-android-sdk-overview.md)

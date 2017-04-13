<properties 
   pageTitle="Vianmääritys käyttöön StorSimple-laite | Microsoft Azure"
   description="Vianmääritys ja korjaus tapahtuvien virheiden StorSimple laitteella, joka on otettu käyttöön ja toiminnallisia käsitellään."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="05/16/2016"
   ms.author="v-sharos" />

# <a name="troubleshoot-an-operational-storsimple-device"></a>Vianmääritys toiminnallisia StorSimple-laite

## <a name="overview"></a>Yleiskatsaus

Tässä artikkelissa on hyötyä vianmäärityksen ohjeita virheiden määritysongelmat saattaa ilmetä sen jälkeen, kun StorSimple laite on otettu käyttöön ja toiminnassa. Tässä artikkelissa kuvataan, yleisiä ongelmia, mahdollisia syitä ja suositeltua vaihetta, joiden avulla voit ratkaista ongelmat, jotka voi kärsiä, kun suoritat Microsoft Azure StorSimple. Nämä tiedot koskevat StorSimple paikallisen fyysinen laite ja StorSimple virtual laite.

Tämän artikkelin löydät, joita voi ilmetä, kun Microsoft Azure StorSimple aikana virhekoodit luettelon lopussa vaiheet sekä niihin virheiden korjaamiseksi. 

## <a name="setup-wizard-process-for-operational-devices"></a>Ohjatun toiminnon määritysprosessi laitteiden

Käytä ohjattua määritystoimintoa ([Käynnistä HcsSetupWizard][1]) voit tarkistaa laitteen määrityksiä ja toteuttaa korjaavat toimenpiteet tarvittaessa.

Kun suoritat ohjatun määritystoiminnon aiemmin määritetty ja toiminnallisia laitteessa, prosessinkulku on erilainen. Voit muuttaa seuraavia tapahtumia:

- IP-osoite ja aliverkkopeite yhdyskäytävä
- Ensisijainen DNS-palvelin
- Ensisijainen NTP-palvelin
- Valinnainen web välityspalvelimen määritykset

Ohjatun määritystoiminnon Suorita salasanan sivustokokoelman ja laitteen rekisteröinti liittyvät toiminnot.

## <a name="errors-that-occur-during-subsequent-runs-of-the-setup-wizard"></a>Ohjatun määritystoiminnon myöhemmin peräkkäisiä aikana virheet

Seuraavassa taulukossa on kuvattu virheitä saattaa ilmetä, kun käynnistät ohjatun määritystoiminnon toiminnallisia laitteessa, virheiden mahdollisia syitä ja suositeltuja toimia ratkaisemiseen. 

| Ei. | Virhesanoma tai ehto | Mahdollisia syitä | Suositellut toimet |
|:--- |:-------------------------- |:--------------- |:------------------ |
|  1  | Virhe 350032: Tämä laite on jo poistettu. | Näyttöön tulee tämä virhe, jos suoritat ohjatun määritystoiminnon laitteessa, joka on poistettu käytöstä. | [Ota yhteyttä Microsoftin tukeen](storsimple-contact-microsoft-support.md) vaiheisiin. Aktivointi laite ei voi asettaa-palvelussa. Factory Palauta voidaan tarvita, ennen kuin laitteen voidaan aktivoida uudelleen. |
|  2  | Käynnistää-HcsSetupWizard: ERROR_INVALID_FUNCTION (poikkeus kohteesta HRESULT: 0x80070001) | DNS-palvelimen päivitys epäonnistui tuntemattomasta. DNS-asetukset ovat yleisiä asetuksia, ja niitä käytetään kaikissa käytössä Verkkoliittymät. | Salli-käyttöliittymän ja käytä DNS-asetukset uudelleen. Tämä voi häiritä muiden käyttöön liittymät verkosta, koska nämä asetukset ovat yleisiä. |
|  3  | On online-palvelun StorSimple hallinta-portaalin laitteen, mutta kun yrität pienin asennuksen ja Tallenna kokoonpano, epäonnistuu. | Ensimmäistä kertaa määritettäessä WWW-välityspalvelimen on määritetty, vaikka oli todellinen välityspalvelimen sijainti. | [Testaa HcsmConnection cmdlet] -komennolla[ 2] Etsi virheen. Jos et voi korjata ongelman [Ota yhteyttä Microsoftin tukeen](storsimple-contact-microsoft-support.md) . |
|  4  | Käynnistää-HcsSetupWizard: Arvo ei kuulu odotetulla alueella. | Virheellinen aliverkkopeite tuottaa virheen. Mahdollisia syitä: <ul><li> Aliverkkopeite on puuttuu tai on tyhjä.</li><li>Ipv6-etuliite muoto on virheellinen.</li><li>Rajapinta on cloud käytössä, mutta yhdyskäytävä on puuttuvien ja virheellisten.</li></ul>Huomaa, että tietojen 0 on automaattisesti cloud-käyttöön, jos määritetty ohjatun määritystoiminnon avulla. | Voit määrittää ongelma, käytä aliverkon 0.0.0.0 tai 256.256.256.256 ja katso tulos. Kirjoita oikeat arvot aliverkkopeite, yhdyskäytävän ja Ipv6-etuliite tarpeen mukaan. |
 
## <a name="error-codes"></a>Virhekoodit

Virheet näkyvät numerojärjestyksessä.

|Virhenumero|Virheteksti- tai kuvaus|Suositeltu käyttäjältä-toiminto|
|:---|:---|:---|
|10502|Tallennustilan tiliä käytettäessä havaittiin virhe.|Odota muutama minuutti ja yritä sitten uudelleen. Jos virhe toistuu, ota seuraavat vaiheet, ota yhteyttä Microsoftin tukeen.|
|40017|Varmuuskopiointi epäonnistui, kun varmuuskopioinnin käytännön määritetyn aseman ei löytynyt laitteesta.|Yrittää varmuuskopiointia toiminto, jos virhe toistuu, ota yhteyttä Microsoft Support. Saat seuraavat vaiheet.|
|40018|Varmuuskopiointi epäonnistui, kun yhtään määritetyn varmuuskopioinnin käytännön tietomääristä ei löytynyt laitteesta. |Yrittää varmuuskopiointia toiminto, jos virhe toistuu, ota yhteyttä Microsoft Support. Saat seuraavat vaiheet.|
|390061|Järjestelmä on varattu tai poissa käytöstä.|Odota muutama minuutti ja yritä sitten uudelleen. Jos virhe toistuu, ota seuraavat vaiheet, ota yhteyttä Microsoftin tukeen.|
|390143|Virhekoodi 390143 tapahtui virhe. (Tuntematon virhe).|Jos virhe toistuu, ota Microsoft Support for seuraavat vaiheet.|

## <a name="next-steps"></a>Seuraavat vaiheet

Jos et voi ratkaise ongelmaa, [Ota yhteys Microsoftin tuotetukeen](storsimple-contact-microsoft-support.md) järjestelmänvalvojalta. 


[1]: https://technet.microsoft.com/en-us/%5Clibrary/Dn688135(v=WPS.630).aspx
[2]: https://technet.microsoft.com/en-us/%5Clibrary/Dn715782(v=WPS.630).aspx

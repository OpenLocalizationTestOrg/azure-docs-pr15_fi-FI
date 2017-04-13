<properties
 pageTitle="Kehittäjän guide - suora menetelmiä | Microsoft Azure"
 description="Azure IoT keskittimeen developer guide - käyttö suoraan menetelmiä käynnistää koodin laitteissa"
 services="iot-hub"
 documentationCenter=".net"
 authors="nberdy"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="nberdy"/>

# <a name="invoke-a-direct-method-on-a-device-preview"></a>Suora menetelmän laitteessa (ennakkoversio)

## <a name="overview"></a>Yleiskatsaus

IoT keskittimeen avulla voi käynnistää menetelmiä pilvestä laitteissa. Menetelmien edustavat samalla tavalla kuin HTTP-kutsun laitteen pyynnön vastaus käsittelemisen siinä, että ne onnistuu tai epäonnistuu heti (jälkeen käyttäjän määrittämä aikakatkaisu) antaa käyttäjän puhelun tilasta. Tästä on hyötyä heti toiminnon kurssin on erilaiset riippuen siitä, onko laitteen pystyi vastaavan, kuten lähettää SMS aktivointi-up laitteeseen, jos laite on offline-tilassa (SMS parhaillaan kalliimpi kuin menetelmän kutsussa).

Voit ajatella menetelmän etäproseduurikutsu suoraan laitteeseen. Vain menetelmiä, jotka on toteutettu laitteessa voidaan kutsua pilvestä. Jos laite, joka ei ole määritetty, että menetelmää menetelmän yrittää pilveen, menetelmäkutsu epäonnistuu.

Laitteen kummassakin menetelmässä on tarkoitettu yhteen laitteeseen. [Töiden] [ lnk-devguide-jobs] lisäämistapaa kutsua menetelmiä useilla eri laitteilla ja jonon menetelmän kutsu yhteys katkaistu laitteita.

Kaikki, joilla on IoT keskittimeen **palvelun yhteyden** käyttöoikeuksia voi käynnistää menetelmän laitteessa.

### <a name="when-to-use"></a>Käyttäminen

Laitteen menetelmät muistuttavat [cloud laitteen viestien] [ lnk-devguide-messages] siten, että molemmat käyttöön cloud taustatietokantaan välittämään tietoja käyttävän laitteen, mutta ne eroavat toisistaan käyttötavat. Käsitteellisesti menetelmiä ovat synkronoidusti ja ei kestävät cloud laitteen viestit ovat asynkroninen 48 tuntia viimeinen myyntipäivä kanssa.

Menetelmien vastaus mallia ja eivät ole kestävät. Viimeinen myyntipäivä puuttuminen on kaksi heti etuja, kun ovat commanding laitteet:

- **Menetelmän suorittaminen heti palautetta** tarkoittaa sitä, voit hallita korrelaatio pyynnön ja vastauksen välillä ei tarvita.
- **Suurempi nopeus** tarkoittaa toiminnot voi suorittaa nopeammin, koska IoT keskittimeen ei tarjoa minkä tahansa kestävyyttä. IoT keskittimeen sallii Lisää menetelmä-puhelut kohti kuin cloud laitteen viestejä.

Cloud laitteen viestit eivät välttämättä komennot laitteeseen, mutta edustaa mieluummin kulkeva joitakin bittinen tietojen sen sen vapaa-ajan, nosta laitteen ja jotka laitteen voi tai ei välttämättä vastaa pilvipalveluun. Cloud laitteen viestien aiherivi on pidempi aikakatkaisu aika (enintään 48 tuntia) aikana menetelmiä päättyy paljon nopeammin.

Käytä laitteen menetelmiä heti komento kutsu laite ja töiden ajoitetun kutsu komennon laitteessa.

## <a name="method-lifecycle"></a>Menetelmä elinkaari

Menetelmistä on otettu käyttöön laitteessa ja vaatia nolla tai syötteiden-menetelmä paketti vahvistamiseen oikein. Suoritat suora menetelmä – palvelu osoittava URI (`{iot hub}/twins/{device id}/methods/`). Laite vastaanottaa suoraan menetelmiä laitekohtaiset MQTT aiheen kautta (`$iothub/methods/POST/{method name}/`). Olemme voivat tukea menetelmiä muita laitteen Include verkko protokollat-tulevaisuudessa.

> [AZURE.NOTE] Suora menetelmä laitteessa suorittaessasi ominaisuuden nimet ja arvot voivat sisältää US-ASCII tulostettavan vain aakkosnumeerinen, ellei jokin seuraavista määrittäminen: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.

Menetelmistä on synkronoitu ja joko onnistuu tai epäonnistuu aikakatkaisuajan jälkeen (oletusarvon: 30 sekuntia, asetettavan määrittäminen 3 600 sekuntia). Menetelmien on hyötyä haluamaasi laite toimii vain, jos laite on online- ja komentoja, kuten otetaan käyttöön puhelimella valo vuorovaikutteinen tilanteissa. Näissä tilanteissa haluat nähdä välittömästi onnistumisesta tai epäonnistumisesta, niin pilvipalvelussa voi suorittaa tulos mahdollisimman pian. Laitteen voi palauttaa jotkin viestin tekstiosaan tuloksena menetelmä, mutta ei kiellä menetelmä edellyttää. Ei-järjestys ei takaa tai minkä tahansa samanaikainen semantiikkaan liittyvien menetelmä puheluja.

Laitteen menetelmäkutsujen ovat vain HTTP cloud reunasta ja vain MQTT laitteen reunasta.

## <a name="reference-topics"></a>Viittaus aiheet:

Viittaus seuraavissa ohjeaiheissa on lisätietoja suora menetelmillä.

## <a name="invoke-a-direct-method-from-a-back-end-app"></a>Suora menetelmän taustatietokantaan-sovelluksesta

### <a name="method-invocation"></a>Menetelmä käynnistäminen

Suora menetelmä ohjelmarakennekaaviossa laitteessa on HTTP puhelut, jotka kuuluvat:

- *URI* tietyn laitteeseen (`{iot hub}/twins/{device id}/methods/`)
- POST- *menetelmällä*
- *Otsikoiden* lupa, joissa on pyytää tunnus, sisällön tyypin ja sisällön koodaus
- Läpinäkyvä JSON *leipätekstin* seuraavanlainen:

```
{
    "methodName": "reboot",
    "timeoutInSeconds": 200,
    "payload": {
        "input1": "someInput",
        "input2": "anotherInput"
    }
}
```

  Aikakatkaisu on sekunteina. Jos aikakatkaisu ei määritetä, sen oletusarvo on 30 sekuntia.
  
### <a name="response"></a>Vastaus

Sitten taustatietokantatiedosto saa vastausta, joka sisältää:

- *HTTP-tilakoodin*, jota käytetään tulossa IoT-toiminnosta, mukaan lukien ei tällä hetkellä liitettyjen laitteiden virhesanoma 404 virheet
- *Otsikoiden* etag, joissa on pyytää tunnus, sisällön tyypin ja sisällön koodaus
- JSON *leipätekstin* seuraavanlainen:

```
{
    "status" : 201,
    "payload" : {...}
}
```
  
   Molemmat `status` ja `body` ovat myöntämä laite ja käytetään vastaa laitteen oman tilakoodin ja/tai kuvaus.

## <a name="handle-a-direct-method-on-a-devcie"></a>Suora menetelmä devcie-kahva

### <a name="method-invocation"></a>Menetelmä käynnistäminen

Laitteiden vastaanottaa suora menetelmä pyyntöjä MQTT aiheesta:`$iothub/methods/POST/{method name}/?$rid={request id}`

Jossa laite vastaanottaa teksti on seuraavanlainen:

```
{
    "input1": "someInput",
    "input2": "anotherInput"
}
```

Menetelmä pyynnöt ovat QoS 0.

### <a name="response"></a>Vastaus

Laite lähettää vastaukset `$iothub/methods/res/{status}/?$rid={request id}`, jossa:

 - `status` -Ominaisuus on menetelmän suorittaminen laitteen annettu tila.
 - `$rid` Ominaisuus on vastaanotettu IoT-toiminnosta menetelmän käynnistys-pyynnön tunnus.

Tekstiin on määritetty, laite ja voi olla mikä tahansa tila.

## <a name="additional-reference-material"></a>Lisää aineistoa

Kehittäjän opas muiden viittauksen aiheet ovat seuraavat:

- [IoT keskittimeen päätepisteet] [ lnk-endpoints] kuvataan eri päätepisteet, jotka kunkin IoT-toiminnossa paljastaa avaamiseen runtime ja hallintaa varten.
- [Rajoitusten ja kiintiöiden] [ lnk-quotas] kuvataan kiintiön, jotka koskevat IoT keskittimeen-palveluun ja rajoittava toiminnan toiminta palvelua käytettäessä.
- [IoT keskittimeen laite- ja palveluluettelon SDK: T] [ lnk-sdks] näyttää eri kielellä SDK: T voit Käytä tätä, kun laite ja palvelun sovelluksista, jotka toimivat IoT keskittimeen ja kehittää.
- [Kyselyn kielen twins, menetelmistä ja töiden] [ lnk-query] kuvataan kyselykieltä, voit hakea tietoja IoT-toiminnosta laitteen twins, menetelmät ja projektit.
- [IoT keskittimeen MQTT tuki] [ lnk-devguide-mqtt] on lisätietoja IoT keskittimeen tuki MQTT-protokollaa.

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet oppinut suoraan eri tavalla, voit tarkastella Sovelluskehittäjän opas seuraavassa artikkelissa:

- [Ajoita työt useilla eri laitteilla][lnk-devguide-jobs]

Jos haluat kokeilla osa käsitteistä, joka on kuvattu tämän artikkelin, voit tarkastella käyttämällä seuraavia IoT keskittimeen opetusohjelma:

- [Cloud laitteen menetelmillä][lnk-methods-tutorial]

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-methods-tutorial]: iot-hub-c2d-methods.md
[lnk-devguide-messages]: iot-hub-devguide-messaging.md

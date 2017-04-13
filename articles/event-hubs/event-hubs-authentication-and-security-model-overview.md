<properties 
    pageTitle="Yleistä tapahtuman keskittimet todennuksen ja mallin | Microsoft Azure"
    description="Tapahtuman keskittimet todennuksen ja mallin yleiskatsaus."
    services="event-hubs"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="event-hubs"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/16/2016"
    ms.author="sethm;clemensv" />

# <a name="event-hubs-authentication-and-security-model-overview"></a>Tapahtuman keskittimet todennuksen ja mallin yleiskatsaus

Tapahtuman keskittimet suojausmalli täyttää seuraavat vaatimukset:

- Vain laitteissa, jotka esittävät kelvolliset tunnistetiedot voivat lähettää tietoja tapahtumaa-toiminnossa.
- Laite ei voi käyttää jotakin muuta laitetta.
- Päivittämättömien laite on estetty tietojen lähettämistä tapahtumaa-toiminnossa.

## <a name="device-authentication"></a>Laitteen todentaminen

Tapahtuman keskittimet suojausmalli perustuu [Jaettu Access allekirjoitus (SAS)](../service-bus-messaging/service-bus-shared-access-signature-authentication.md) tunnusten ja *tapahtumien julkaisijoiden*yhdistelmän. Tapahtuman julkaisijan määrittää virtual päätepisteen tapahtumaa-toiminnossa. Julkaisija vain voidaan lähettää viestejä tapahtumaa-toiminnossa. Ei ole mahdollista vastaanottamaan viestejä julkaisijan.

Tapahtuma-toiminnossa käytetään yleensä yhden julkaisijan laite. Kaikki viestit, joka on tullut johonkin tapahtumaa-toiminnossa julkaisijat ovat enqueued sisällä, tapahtuma-toiminnossa. Julkaisijat Ota tarkasti rajattuja käyttöoikeuksia ja rajoitusta.

Kunkin laitteen määritetään yksilöivä tunnus, joka on ladattu palvelimeen laitteeseen. Tunnukset on valmistettu siten, että kunkin yksilöllisen tunnuksen myöntää käyttö eri yksilöllinen Publisheriin. Laite, jonka tunnus on lähettää vain yhden julkaisijan, mutta ei muiden publisher. Jos useilla eri laitteilla jakaa saman tunnuksen, kukin seuraaviin laitteisiin jakaa julkaisijan.

Vaikka ei suositella, on mahdollista oheislaitteet laitteet, joissa on tunnuksia, jotka suoraan käyttöoikeuden myöntäminen tapahtumaa-toiminnossa. Kaikissa laitteissa, joissa on suojaustunnuksen voi lähettää viestejä suoraan kyseisen tapahtuman-toiminnossa. Käyttävän laitteen eivät ole veloittaa rajoitusta. Lisäksi et voi mustalla lähettämästä kyseisen tapahtuman keskittimeen listalla laite.

Tunnusten on allekirjoitettu SAS-näppäintä. Useimmiten tunnusten on allekirjoitettu samaa avainta. Laitteet eivät tiedä näppäimen. Tämä estää laitteiden tuotannon tunnukset.

### <a name="create-the-sas-key"></a>Luo SAS avain

Tapahtuman keskittimet nimitilan luotaessa Azure tapahtuman keskittimet Luo 256-bittinen SAS avain nimeltä **RootManageSharedAccessKey**. Tämä avaimen myöntää Lähetä, kuuntele ja hallita nimitilan oikeudet. Voit luoda uusia avaimet. On suositeltavaa tuottaa myöntää Lähetä käyttöoikeudet tiettyyn tapahtuma-keskittimeen näppäintä. Tässä ohjeaiheessa loput oletetaan nimeltä avaimeen `EventHubSendKey`.

Seuraavassa esimerkissä luodaan vain Lähetä-näppäintä, kun luot tapahtumaa-toiminnossa:

```
// Create namespace manager.
string serviceNamespace = "YOUR_NAMESPACE";
string namespaceManageKeyName = "RootManageSharedAccessKey";
string namespaceManageKey = "YOUR_ROOT_MANAGE_SHARED_ACCESS_KEY";
Uri uri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, string.Empty);
TokenProvider td = TokenProvider.CreateSharedAccessSignatureTokenProvider(namespaceManageKeyName, namespaceManageKey);
NamespaceManager nm = new NamespaceManager(namespaceUri, namespaceManageTokenProvider);

// Create Event Hub with a SAS rule that enables sending to that Event Hub
EventHubDescription ed = new EventHubDescription("MY_EVENT_HUB") { PartitionCount = 32 };
string eventHubSendKeyName = "EventHubSendKey";
string eventHubSendKey = SharedAccessAuthorizationRule.GenerateRandomKey();
SharedAccessAuthorizationRule eventHubSendRule = new SharedAccessAuthorizationRule(eventHubSendKeyName, eventHubSendKey, new[] { AccessRights.Send });
ed.Authorization.Add(eventHubSendRule); 
nm.CreateEventHub(ed);
```

### <a name="generate-tokens"></a>Luo tunnusten

Voit luoda tunnusten Suojaussidosten avaimen avulla. Vain yksi tunnuksen laite on tuottaa. Tunnusten voidaan tuottaa sitten alla kuvatulla tavalla. Tunnusten luodaan **EventHubSendKey** avaimen avulla. Kunkin tunnuksen määritetään yksilöivä URI.

```
public static string SharedAccessSignatureTokenProvider.GetSharedAccessSignature(string keyName, string sharedAccessKey, string resource, TimeSpan tokenTimeToLive)
```

Tämä menetelmä soitettaessa URI määritetyssä muodossa `//<NAMESPACE>.servicebus.windows.net/<EVENT_HUB_NAME>/publishers/<PUBLISHER_NAME>`. Saat kaikki tunnusten URI on sama kuin lukuun ottamatta `PUBLISHER_NAME`, joiden pitäisi olla erilaisia kunkin tunnuksen. Ihannetapauksessa `PUBLISHER_NAME` edustaa laitteella, joka saa ilmoituksen, että tunnuksen.

Tämä menetelmä luo tunnuksen, jonka seuraavaa rakennetta:

```
SharedAccessSignature sr={URI}&sig={HMAC_SHA256_SIGNATURE}&se={EXPIRATION_TIME}&skn={KEY_NAME}
```

Suojaustunnuksen päättymisaika on määritetty tammi 1, 1970 sekunnin ajan. Seuraavassa on esimerkki tunnusta:

```
SharedAccessSignature sr=contoso&sig=nPzdNN%2Gli0ifrfJwaK4mkK0RqAB%2byJUlt%2bGFmBHG77A%3d&se=1403130337&skn=RootManageSharedAccessKey
```

Yleensä tunnukset on käyttöikä, joka muistuttaa tai ylittää laitteen käyttöikä. Jos laite on ominaisuutta, voit hankkia uuden tunnuksen, tunnusten lyhentää käyttöikä kanssa voidaan käyttää.

### <a name="devices-sending-data"></a>Tietojen lähettäminen laitteet

Kun tunnukset on luotu, kukin laite on valmisteltu omassa yksilöllisen tunnuksen.

Kun laite lähettää tietoja tapahtuma-keskittimeen, laite tunnisteet sen tunnuksen ja lähettämispyyntö. Jos haluat estää voi saada salakuuntelulta ja varastamasta tunnuksen, välisen laitteen ja tapahtumaa-toiminnossa on oltava salatun kanavan kautta.

### <a name="blacklisting-devices"></a>Blacklisting laitteet

Jos tunnus on varastamiselta voi, hän voi käyttää laite, jonka tunnus on ollut varastamiselta. Laitteen blacklisting hahmontaa laite ei käytettävissä, kunnes laite on annettu uuden tunnuksen, joka käyttää eri publisher.

## <a name="authentication-of-back-end-applications"></a>Taustatietokannan sovellusten todennus

Tapahtuman keskittimet käytetään tarkistamiseen sovelluksia, jotka käyttävät laitteet luomien tietojen suojausmalli, joka muistuttaa mallia, jota käytetään palvelun Bus ohjeita. Tapahtuman keskittimet-kuluttaja-ryhmän vastaa palvelun Bus aihe-tilausta. Asiakas voi luoda kuluttaja-ryhmä, jos pyynnön kuluttaja-ryhmän luominen on liitettävä tunnuksen myöntää hallinta oikeudet tapahtumaa-toiminnossa tai nimitilan, johon tapahtuma-toiminnossa kuuluu. Asiakas on oikeus tarjoaman kuluttaja-ryhmän tiedot, jos vastaanotto-pyyntö on liitettävä tunnuksen, vastaanotto-oikeudet kuluttaja ryhmään, tapahtuma-toiminnossa tai nimitilan, johon tapahtuma-toiminnossa kuuluu.

Palvelun Bus nykyinen versio ei tue SAS säännöt yksittäisiä tilaukset. Sama koskee tapahtuman keskittimet ryhmiä. SAS tuki lisätään sekä ominaisuuksien tulevaisuudessa.

Yksittäisten ryhmiä SAS todennus puuttuessa kaikkia ryhmiä suojaaminen yleisiä näppäintä SAS näppäimet avulla. Tätä tapaa kannattaa ottaa käyttöön sovelluksen tarjoaman tietojen tapahtumaa-toiminnossa kuluttaja-ryhmä.

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja tapahtuman keskittimet, seuraavassa on seuraavissa artikkeleissa:

- [Tapahtuman keskittimet yleiskatsaus]
- [Jonossa tekstiviesti ratkaisu] palvelun Bus olevien avulla.
- Valmis [tapahtuman keskittimet käyttävän sovelluksen malli].

[Tapahtuman keskittimet yleiskatsaus]: event-hubs-overview.md
[Tapahtuman keskittimet käyttävä malli-sovellus]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[jonossa tekstiviesti ratkaisu]: ../service-bus-messaging/service-bus-dotnet-multi-tier-app-using-service-bus-queues.md
 

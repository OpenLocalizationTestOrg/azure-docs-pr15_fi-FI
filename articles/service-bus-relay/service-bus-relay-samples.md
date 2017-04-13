<properties 
    pageTitle="Palvelun Bus välitys esimerkit yleiskatsaus | Microsoft Azure"
    description="Luokittelee ja kuvataan linkit jokaiseen palvelun Bus välitys näytteiden."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/07/2016"
    ms.author="sethm" />

# <a name="service-bus-relay-samples"></a>Palvelun Bus välitys objektit

Palvelun Bus välitys mallit näytetään [palvelun Bus välitys](https://azure.microsoft.com/services/service-bus/)tärkeimmistä ominaisuuksista. Tässä artikkelissa luokittelee ja vertaamalla linkit jokaiseen mallit.

>[AZURE.NOTE] Palvelun Bus mallit eivät asennu SDK: N kanssa. Voit hankkia mallit seuraavasta [Azure SDK näytteiden sivun](https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=5).
>
>Lisäksi on päivitetty määrittäminen palvelun Bus välitys näytteiden [tähän](https://github.com/Azure-Samples/azure-servicebus-relay-samples) (vuodesta tämän virkkeet, ne ei tässä artikkelissa kuvattuja).  

Katso tekstiviesti näytteiden [palvelun Bus messaging objektit](../service-bus-messaging/service-bus-samples.md).

## <a name="service-bus-relay"></a>Palvelun Bus välitys

Seuraavat esimerkit osoittavat viestin kirjoittaminen palvelun Bus välityspalvelu käyttävistä sovelluksista.

Huomaa, että välitys-mallit edellyttävät yhteysmerkkijonon käyttämään palvelua Bus nimitilan.

### <a name="to-obtain-a-connection-string-for-azure-service-bus"></a>Voit hankkia yhteysmerkkijonoa Azure palvelun Bus

1. Kirjaudu [Azure portal](http://portal.azure.com).

1. Valitse vasemmanpuoleisessa sarakkeessa **Palvelun Bus**.

1. Napsauta oman nimitilan luettelon nimeä.

3. Valitse **jaettu käytäntöjen**nimitilan-sivu.

4. Valitse **RootManageSharedAccessKey** **jaettu käytäntöjen** -sivu.

6. Kopioi yhteysmerkkijono Leikepöydälle.

### <a name="to-obtain-a-connection-string-for-service-bus-for-windows-server"></a>Voit hankkia yhteysmerkkijonon palvelun Bus Windows Server

1. Suorittaa seuraavan PowerShell cmdlet-komennon:

    ```
    get-sbClientConfiguration
    ```

2. Liitä yhteysmerkkijonon otosten App.config-tiedostoa.

## <a name="service-bus-relay"></a>Palvelun Bus välitys

Objektit, jotka kuvaavat palvelun Bus välitys.

### <a name="getting-started"></a>Käytön aloittaminen

|Esimerkki nimestä|Kuvaus|Pienin SDK-versio|Käytettävyys|
|---|---|---|---|
|[Välittäminen Messaging: Azure](http://code.msdn.microsoft.com/Relayed-Messaging-Windows-0d2cede3)|Käsittelee suorittaa palvelun Bus asiakkaan ja palvelun Azure. Tässä esimerkissä määrittää palvelun Bus ohjelmallisesti. Vain ympäristön ja suojaus-tiedot tallennetaan määritystiedostoja.|1.8|Microsoft Azure-palvelu Bus|
|[Välittäminen tekstiviesti todennus: Jaettu salaisuus](http://code.msdn.microsoft.com/Relayed-Messaging-92b04c02)|Esittelee myöntäjän nimi ja myöntäjä salaisuus käyttäminen palvelun Bus todentamismenetelmä.|1.8|Microsoft Azure-palvelu Bus|
|[Tekstiviesti todennus välittäminen: WebNoAuth](http://code.msdn.microsoft.com/Relayed-Messaging-a4f0b831)|Esitellään, miten voit näyttää HTTP-palvelu, joka ei edellytä asiakkaan käyttäjän todennusta.|1.8|Microsoft Azure-palvelu Bus|
|[Tekstiviesti sidontojen välittäminen: WebHttp](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-a6477ba0)|Käsittelee **WebHttpRelayBinding** sidonta avulla voit palauttaa Internet-ohjelmoinnin mallin binaaritietoja.|1.8|Microsoft Azure-palvelu Bus|
|[Välittäminen tekstiviesti sidontojen: NetTcp välittäminen](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-2dec7692)|Esittelee käyttämisestä **NetTcpRelayBinding** sidonta.|1.8|Microsoft Azure-palvelu Bus|

### <a name="exploring-features"></a>Tutustuminen ominaisuudet

Objektit, jotka kuvaavat eri palvelun Bus välitys ominaisuuksia.

|Esimerkki nimestä|Kuvaus|Pienin SDK-versio|Käytettävyys|
|---|---|---|---|
|[Välittäminen tekstiviesti todennus: Yksinkertainen WebToken](http://code.msdn.microsoft.com/Relayed-Messaging-32c74392)|Esitellään, miten palvelun Bus todentamismenetelmä yksinkertaisen web suojaustunnuksen tunnistetietojen avulla. Otosten muistuttaa Kaiku näytteen muutamia muutoksia. Tarkemmin sanottuna tässä esimerkissä Lisää toiminnan ServiceHost (palvelu)-ja ChannelFactory (asiakas)-sovelluksissa.|1.8|Microsoft Azure-palvelu Bus|
|[Välittäminen Messaging: Kuormituksen tasaus](http://code.msdn.microsoft.com/Relayed-Messaging-Load-bd76a9f8)|Esitellään, miten Microsoft Azure palvelun Bus käyttämään useita vastaanottajia viestien reitittämiseen. Se näyttää eri esiintymissä yksinkertainen palvelun kautta **NetTcpRelayBinding** sidonta asiakkaan kanssa|1.8|Microsoft Azure-palvelu Bus|
|[Välittäminen tekstiviesti sidontojen: Nettonykyarvon tapahtuma](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-c0176977)|Esittelee **NetEventRelayBinding** sidonta käyttäminen Microsoft Azure palvelun Bus.|1.8|Microsoft Azure-palvelu Bus|
|[Välittäminen tekstiviesti sidontojen: WS2007Http istuntoon](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-ef1f1fcb)|Esittelee **WS2007HttpRelayBinding** sidonta käyttäminen luotettavia istuntoja käytössä. Se näyttää myös määrittäminen palvelun Bus tunnistetietojen sijaan ohjelmallisesti määritystiedostossa.|1.8|Microsoft Azure-palvelu Bus|
|[Välittäminen tekstiviesti sidontojen: WS2007Http MsgSecCertificate](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-f29c9da5)|Esittelee **WS2007HttpRelayBinding** sidonta käyttäminen viestin suojauksen suojaamiseen lopusta loppuun-viestien vaativat edelleen asiakkaiden palvelun Bus todentamismenetelmä.|1.8|Microsoft Azure-palvelu Bus|
|[Välittäminen Messaging: Metatietojen vaihdon](http://code.msdn.microsoft.com/Relayed-Messaging-Metadata-f122312e)|Esitellään, miten voit näyttää metatiedot päätepisteen, joka käyttää välitys sidonta. **MetadataExchange** tukee seuraavia välitys sidontojen: **NetTcpRelayBinding**, **NetOnewayRelayBinding**, **BasicHttpRelayBinding**ja **WS2007HttpRelayBinding**.|1.8|Microsoft Azure-palvelu Bus|
|[Välittäminen tekstiviesti sidontojen: NetTcp suoraan](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-ca039161)|Esittelee **NetTcpRelayBinding** sidonta tukemaan **Hybrid** yhteyden tilan joka muodostaa ensin relayed yhteyden ja, jos se on mahdollista vaihtaa automaattisesti suoran muodostaminen asiakkaan ja palvelun määrittämisestä.|1.8|Microsoft Azure-palvelu Bus|
|[Välittäminen tekstiviesti sidontojen: NetTcp MsgSec käyttäjänimi](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-30542392)|Esittelee **NetTcpRelayBinding** sidonta käyttäminen viestin suojaus.|1.8|Microsoft Azure-palvelu Bus|
|[Välittäminen tekstiviesti sidontojen: Nettonykyarvon Oneway](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-bb5b813a)|Esitellään, miten tarjoaman palvelun-päätepisteen **NetOnewayRelayBinding** sidontaa käyttämällä ja näyttää.|1.8|Microsoft Azure-palvelu Bus|
|[Välittäminen tekstiviesti sidontojen: WS2007Http yksinkertainen](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-aa4b793a)|Esittelee **WS2007HttpRelayBinding** sidontaa käyttämällä. Esimerkissä yksinkertainen palvelu, joka käyttää ei suojausasetukset ja ei edellytä asiakkaiden tarkistamiseen.|1.8|Microsoft Azure-palvelu Bus|

## <a name="next-steps"></a>Seuraavat vaiheet

Seuraavissa artikkeleissa saat palvelun Bus käsitteellinen yleiskatsauksen.

- [Palvelun Bus välitys yleiskatsaus](service-bus-relay-overview.md)
- [Palvelun Bus-arkkitehtuuri](../service-bus-messaging/service-bus-architecture.md)
- [Palvelun Bus perusteet](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)
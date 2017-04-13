<properties
   pageTitle="Yleistä Azure palvelun kangasta luotettava toimijoiden KVSActorStateProvider määritysten | Microsoft Azure"
   description="Lisätietoja Azure palvelun kangasta tilallisten toimijoiden KVSActorStateProvider tyypin määrittäminen."
   services="Service-Fabric"
   documentationCenter=".net"
   authors="sumukhs"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/20/2016"
   ms.author="sumukhs"/>

# <a name="configuring-reliable-actors--kvsactorstateprovider"></a>Luotettavan toimijoiden--KVSActorStateProvider määrittäminen
Voit muokata KVSActorStateProvider oletusarvon määrittäminen muuttamalla settings.xml-tiedosto, joka on luotu määritetyn toimija Microsoft Visual Studio paketin päätasolla Config-kansiossa.

Azure-palvelu kangasta runtime etsitään ennalta määritetyn osan settings.xml tiedostossa olevat nimet ja siinä käytetään määritysten arvojen pohjana runtime-osien luomisen aikana.

>[AZURE.NOTE] Tee **ei** Poista tai Muokkaa seuraavat määritykset settings.xml-tiedostossa, joka on luotu Visual Studio-ratkaisun osan nimet.

## <a name="replicator-security-configuration"></a>Monistus suojausasetukset
Monistus suojausmäärityksiä käytetään suojaamiseen tietoliikenneyhteyden, jota käytetään replikoinnin aikana. Tämä tarkoittaa sitä, palvelujen näe toistensa replikoinnin tietoliikenteen varmistetaan, että tiedot, jotka tehdään erittäin käytettävissä on myös suojattu.
Tyhjä suojauksen määritykset-osasta estää oletusarvoisesti replikoinnin suojaus.

### <a name="section-name"></a>Osan nimi
&lt;ActorName&gt;ServiceReplicatorSecurityConfig

## <a name="replicator-configuration"></a>Monistus määritys
Monistus käyttömahdollisuudet määrittäminen Monistus, joka on vastuussa tekeminen toimija tila palvelun tilan erittäin luotettavia.
Oletusarvo-määritys on luonut Visual Studio-mallin ja pitäisi riittää. Tässä osassa puheen lisämäärityksiä paranna Replikointitoiminto käytettävissä olevia tietoja.

### <a name="section-name"></a>Osan nimi
&lt;ActorName&gt;ServiceReplicatorConfig

### <a name="configuration-names"></a>Määritysten nimet

|Nimi|Yksikkö|Oletusarvo|Huomautuksia|
|----|----|-------------|-------|
|BatchAcknowledgementInterval|Sekunnin ajan|0.015|Monistus osoitteessa toissijainen odottaa saatuaan ennen lähettämistä toiminnon takaisin kuittausta on ensisijainen, jonka ajan kuluessa. Muut kuittaussanomien voidaan lähettää toimille käsitellään aikaväliä lähetetään yksi vastaus.|
|ReplicatorEndpoint|PUUTTUU|Ei ole oletusarvoisesti--pakollinen parametri|Määritä IP-osoite ja portin ensisijainen ja toissijainen Monistus käytetään se muita ohjelmistomonistajilta yhteydessä. Tämä tulee viitata TCP-resurssin päätepistettä service-luettelossa. Lisätietoja [palvelun luettelon resurssien](service-fabric-service-manifest-resources.md) määrittämisestä päätepisteen resurssien service-luettelossa. |
|RetryInterval|Sekunnin ajan|5|Ajanjakso, jonka jälkeen Replikointitoiminto uudelleen välittää viestin jos se ei saa vastausta toiminnon.|
|MaxReplicationMessageSize|Tavua|50 MT|Replikoinnin tiedot, jotka voidaan lähettää yhden viestin enimmäiskoon.|
|MaxPrimaryReplicationQueueSize|Toimintojen määrä|vähintään 1 024|Ensisijainen jonossa toimintojen enimmäismäärä. Toiminto on vapauttanut, kun ensisijainen Monistus saa vastausta toissijaisen ohjelmistomonistajilta. Tämän arvon on oltava suurempi kuin 64 ja 2 potenssiin.|
|MaxSecondaryReplicationQueueSize|Toimintojen määrä|2 048|Toimien toissijaisen jonossa enimmäismäärä. Kun olet tehnyt tilaan erittäin saatavana pysyvyys on vapauttanut toiminnon. Tämän arvon on oltava suurempi kuin 64 ja 2 potenssiin.|

## <a name="store-configuration"></a>Säilön määritys
Säilön määrityksiä käytetään paikalliseen säilöön, jota käytetään jatkuvat tilaan, jossa on käytössä replikoida määrittämiseen.
Oletusarvo-määritys on luonut Visual Studio-mallin ja pitäisi riittää. Tässä osassa puheen lisämäärityksiä paranna paikallista käytettävissä olevia tietoja.

### <a name="section-name"></a>Osan nimi
&lt;ActorName&gt;ServiceLocalStoreConfig

### <a name="configuration-names"></a>Määritysten nimet

|Nimi|Yksikkö|Oletusarvo|Huomautuksia|
|----|----|-------------|-------|
|MaxAsyncCommitDelayInMilliseconds|Millisekunteina|200|Määrittää suurin jonottaminen kestävät Paikallinen säilö tapahtumasarjan vahvistukset aikaväli.|
|MaxVerPages|Sivujen määrä|16384|Paikallisen version sivujen enimmäismäärä tallentaa tietokannan. Määrittää avoimen tapahtumien enimmäismäärä.|

## <a name="sample-configuration-file"></a>Esimerkki kokoonpanotiedosto

```xml
<?xml version="1.0" encoding="utf-8"?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Section Name="MyActorServiceReplicatorConfig">
      <Parameter Name="ReplicatorEndpoint" Value="MyActorServiceReplicatorEndpoint" />
      <Parameter Name="BatchAcknowledgementInterval" Value="0.05"/>
   </Section>
   <Section Name="MyActorServiceLocalStoreConfig">
      <Parameter Name="MaxVerPages" Value="8192" />
   </Section>
   <Section Name="MyActorServiceReplicatorSecurityConfig">
      <Parameter Name="CredentialType" Value="X509" />
      <Parameter Name="FindType" Value="FindByThumbprint" />
      <Parameter Name="FindValue" Value="9d c9 06 b1 69 dc 4f af fd 16 97 ac 78 1e 80 67 90 74 9d 2f" />
      <Parameter Name="StoreLocation" Value="LocalMachine" />
      <Parameter Name="StoreName" Value="My" />
      <Parameter Name="ProtectionLevel" Value="EncryptAndSign" />
      <Parameter Name="AllowedCommonNames" Value="My-Test-SAN1-Alice,My-Test-SAN1-Bob" />
   </Section>
</Settings>
```
## <a name="remarks"></a>Huomautuksia

BatchAcknowledgementInterval-parametri ohjaa replikoinnin viive. Arvo on "0" tuloksena pienin mahdollinen viive kustannuksella siirtonopeuden (Katso Lisää kuittaus viestit on lähetetty ja käsitellä sisältävä vähemmän kuittaussanomien).
Mitä suurempi arvo BatchAcknowledgementInterval, suuremman yleinen replikoinnin siirtonopeuden, kustannuksella suurempi toiminnon viive. Tämä kääntää suoraan tapahtuma hyväksytään viive.

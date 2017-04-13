<properties
   pageTitle="Yleistä Azure palvelun kangasta luotettava toimijoiden ReliableDictionaryActorStateProvider määritysten | Microsoft Azure"
   description="Lisätietoja Azure palvelun kangasta tilallisten toimijoiden ReliableDictionaryActorStateProvider tyypin määrittäminen."
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
   ms.date="07/18/2016"
   ms.author="sumukhs"/>

# <a name="configuring-reliable-actors--reliabledictionaryactorstateprovider"></a>Luotettavan toimijoiden--ReliableDictionaryActorStateProvider määrittäminen
Voit muokata ReliableDictionaryActorStateProvider oletusarvon määrittäminen muuttamalla settings.xml tiedosto luodaan määritetyn toimija Visual Studio paketin päätasolla Config-kansiossa.

Azure-palvelu kangasta runtime etsitään ennalta määritetyn osan settings.xml tiedostossa olevat nimet ja siinä käytetään määritysten arvojen pohjana runtime-osien luomisen aikana.

>[AZURE.NOTE] Tee **ei** Poista tai Muokkaa seuraavat määritykset settings.xml-tiedostossa, joka on luotu Visual Studio-ratkaisun osan nimet.

Saatavilla on myös yleinen ReliableDictionaryActorStateProvider määritysten vaikuttavia asetuksia.

## <a name="global-configuration"></a>Yleiset määritykset

Yleiset määritykset on määritetty KtlLogger-osassa klusterin klusterin luettelo. Se mahdollistaa jaetun lokin sijainnin ja koon plus lokin käyttämä yleinen muistirajoitukset. Huomaa, että klusterin luettelo muutokset vaikuta kaikki palvelut, jotka käyttävät ReliableDictionaryActorStateProvider ja luotettavia tilallisten palveluja.

Klusterin luettelo on yksi XML-tiedosto, joka sisältää asetukset ja määritykset, jotka koskevat kaikkia solmujen ja palveluiden klusterin. Tiedoston nimi on yleensä ClusterManifest.xml. Voit tarkastella klusterin luettelon yhteyttä klusterin Get-ServiceFabricClusterManifest powershell-komennolla.

### <a name="configuration-names"></a>Määritysten nimet

|Nimi|Yksikkö|Oletusarvo|Huomautuksia|
|----|----|-------------|-------|
|WriteBufferMemoryPoolMinimumInKB|Kilotavua.|8388608|Vähimmäismäärä kt varata lokin kirjoittaminen puskurin muistin resurssivarantoon ydin-tilassa. Muistin kannan käytetään välimuistin tietoja ennen levylle kirjoittamista.|
|WriteBufferMemoryPoolMaximumInKB|Kilotavua.|Ei rajaa|Enimmäiskoko, johon lokin kirjoittaa puskurin muistin resurssivarantoon voi kasvaa.|
|SharedLogId|GUID-TUNNUS|""|Määrittää yksilöllinen GUID-tunnus, joka käyttää jaettua tiedostoa kaikissa klusterin solmuissa, joissa ei määritetä SharedLogId niiden palvelun tiettyä määrityksessä luotettava palveluiden käyttämän tunnistamista varten. Jos SharedLogId on määritetty, myös SharedLogPath määritettävä.|
|SharedLogPath|Täydellinen polku|""|Määrittää, jossa jaetun lokitiedosto kaikissa klusterin solmuissa, joissa ei määritetä SharedLogPath niiden palvelun tietyn määrityksessä luotettavasta palveluiden käyttämän täydellinen polku. Kuitenkin jos SharedLogPath on määritetty, valitse SharedLogId on myös määritettävä.|
|SharedLogSizeInMB|Megatavua|8192|Määrittää, kuinka Megatavua levytilaa varata nimipalvelimet jaetun sisäänkirjautumista varten. Arvon on oltava 2048 tai suurempi.|

### <a name="sample-cluster-manifest-section"></a>Esimerkki klusterin luettelo-osassa
```xml
   <Section Name="KtlLogger">
     <Parameter Name="WriteBufferMemoryPoolMinimumInKB" Value="8192" />
     <Parameter Name="WriteBufferMemoryPoolMaximumInKB" Value="8192" />
     <Parameter Name="SharedLogId" Value="{7668BB54-FE9C-48ed-81AC-FF89E60ED2EF}"/>
     <Parameter Name="SharedLogPath" Value="f:\SharedLog.Log"/>
     <Parameter Name="SharedLogSizeInMB" Value="16383"/>
   </Section>
```

### <a name="remarks"></a>Huomautuksia
Lokin on varattu, joka on käytettävissä kaikkien luotettavia palveluja, solmun välimuistiin tallentaminen tilatiedot, ennen kuin oma lokiin kirjoitetaan liittyvät luotettavaa palvelun se ei ole sivutettu ydin muistin yleinen resurssivarantoon. Ryhmän koko määräytyy WriteBufferMemoryPoolMinimumInKB ja WriteBufferMemoryPoolMaximumInKB asetusten mukaan. WriteBufferMemoryPoolMinimumInKB määrittää muistin kannan alkuperäisen koon ja alimman, johon muistin resurssivarantoon voi pienentää kokoa. WriteBufferMemoryPoolMaximumInKB on suurin koko, johon muistin resurssivarantoon saattaa kasvaa. Luotettavan palvelun replikoissa, joka on avattu voi muistin altaan suurentaa määritetään järjestelmän verran ylöspäin WriteBufferMemoryPoolMaximumInKB. Jos näkyvissä on enemmän kuin on käytettävissä oleva muisti käytetään varannon muistin tarve, muistin pyyntö voidaan lykätä, kunnes muisti. Tämän vuoksi Jos kirjoittaminen puskurin muistin varanto on liian pientä tietty kokoonpano sitten suorituskyky voi kärsiä.

SharedLogId ja SharedLogPath-asetuksia käytetään aina yhdessä klusterin GUID-tunnus ja sijainnin oletusarvon jaetun lokin kaikkien solmujen määrittämiseen. Oletusarvon jaetun lokiin käytetään kaikkien luotettavaa palvelua, joiden asetuksia ei määritetä tietyn palvelun settings.xml. Parhaan mahdollisen suorituskyvyn jaetun lokitiedostojen sijoitetaan levyille, joita käytetään tuloskorteissa jaetun lokitiedosto vähentää ristiriita.

SharedLogSizeInMB määrittää levytilaa kohdistettavien oletusarvon jaetun kirjautumisen kaikkien solmujen määrän.  SharedLogId ja SharedLogPath ei tarvitse erikseen, jotta SharedLogSizeInMB erikseen.

## <a name="replicator-security-configuration"></a>Monistus suojausasetukset
Monistus suojausmäärityksiä käytetään suojaamiseen tietoliikenneyhteyden, jota käytetään replikoinnin aikana. Tämä tarkoittaa sitä, palvelujen näe toistensa replikoinnin tietoliikenteen varmistaa, että tiedot, jotka tehdään erittäin käytettävissä on myös suojattu.
Tyhjä suojauksen määritykset-osasta estää oletusarvoisesti replikoinnin suojaus.

### <a name="section-name"></a>Osan nimi
&lt;ActorName&gt;ServiceReplicatorSecurityConfig

## <a name="replicator-configuration"></a>Monistus määritys
Monistus määrityksiä käytetään määrittäminen Monistus, joka on vastuussa tekeminen toimija tila palvelun tilan erittäin luotettavien replikoiminen ja toteaa tilan paikallisesti.
Oletusarvo-määritys on luonut Visual Studio-mallin ja pitäisi riittää. Tässä osassa puheen lisämäärityksiä paranna Replikointitoiminto käytettävissä olevia tietoja.

### <a name="section-name"></a>Osan nimi
&lt;ActorName&gt;ServiceReplicatorConfig

### <a name="configuration-names"></a>Määritysten nimet

|Nimi|Yksikkö|Oletusarvo|Huomautuksia|
|----|----|-------------|-------|
|BatchAcknowledgementInterval|Sekunnin ajan|0.015|Monistus osoitteessa toissijainen odottaa saatuaan ennen lähettämistä toiminnon takaisin kuittausta on ensisijainen, jonka ajan kuluessa. Muut kuittaussanomien voidaan lähettää toimille käsitellään aikaväliä lähetetään yksi vastaus.||
|ReplicatorEndpoint|PUUTTUU|Ei ole oletusarvoisesti--pakollinen parametri|Määritä IP-osoite ja portin ensisijainen ja toissijainen Monistus käytetään se muita ohjelmistomonistajilta yhteydessä. Tämä tulee viitata TCP-resurssin päätepistettä service-luettelossa. Lisätietoja [palvelun luettelon resurssien](service-fabric-service-manifest-resources.md) määrittämisestä päätepisteen resurssien palvelun luettelo. |
|MaxReplicationMessageSize|Tavua|50 MT|Replikoinnin tiedot, jotka voidaan lähettää yhden viestin enimmäiskoon.|
|MaxPrimaryReplicationQueueSize|Toimintojen määrä|8192|Ensisijainen jonossa toimintojen enimmäismäärä. Toiminto on vapauttanut, kun ensisijainen Monistus saa vastausta toissijainen ohjelmistomonistajilta. Tämän arvon on oltava suurempi kuin 64 ja 2 potenssiin.|
|MaxSecondaryReplicationQueueSize|Toimintojen määrä|16384|Toimien toissijaisen jonossa enimmäismäärä. Kun olet tehnyt tilaan erittäin saatavana pysyvyys on vapauttanut toiminnon. Tämän arvon on oltava suurempi kuin 64 ja 2 potenssiin.|
|CheckpointThresholdInMB|MT|200|Määrä, jonka jälkeen tila on checkpointed lokitiedoston tila.|
|MaxRecordSizeInKB|KT|vähintään 1 024|Suurin tietueen koko, joka replikointitoiminto voi kirjoittaa lokiin. Tämän arvon on oltava 4 ja yli 16 kerrannaiseen.|
|OptimizeLogForLowerDiskUsage|Totuusarvo|TOSI|Kun arvo on TOSI, kirjaudu on määritetty siten, että se on erillinen lokitiedosto luodaan lyhyet NTFS-tiedoston avulla. Laskee muuta tiedoston todellinen levytilan. Kun arvo on EPÄTOSI, tiedosto on luotu kiinteä kohdistukset, jotka tarjoavat parhaan suorituskyvyn kirjoittaminen.|
|SharedLogId|GUID-tunnus|""|Määrittää yksilöllinen GUID-tunnus, joka käyttää yksilöimiseen jaetun lokitiedosto tämän replikan käyttämisestä. Yleensä services Älä käytä asetusta. Kuitenkin jos SharedLogId on määritetty, valitse SharedLogPath on myös määritettävä.|
|SharedLogPath|Täydellinen polku|""|Määrittää, missä tämän replikan jaetun lokitiedoston luodaan täydellinen polku. Yleensä services Älä käytä asetusta. Kuitenkin jos SharedLogPath on määritetty, valitse SharedLogId on myös määritettävä.|


## <a name="sample-configuration-file"></a>Esimerkki kokoonpanotiedosto

```xml
<?xml version="1.0" encoding="utf-8"?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Section Name="MyActorServiceReplicatorConfig">
      <Parameter Name="ReplicatorEndpoint" Value="MyActorServiceReplicatorEndpoint" />
      <Parameter Name="BatchAcknowledgementInterval" Value="0.05"/>
      <Parameter Name="CheckpointThresholdInMB" Value="180" />
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

CheckpointThresholdInMB-parametrin ohjausobjektit, Replikointitoiminto avulla voit tallentaa sen erillinen lokitiedoston tilatietoihin levytilan määrän. Lisääntyvien tämä suuremmat arvot kuin oletusarvoista saattaa johtaa kokoonpanon uudelleenmääritys nopeammin, kun uusi replika lisätään joukkoa. Tämä on osittain tilan siirto joka suoritetaan, Lisää historia-toimintoa lokiin saatavuuden vuoksi. Tämä suurentaa replikan palautus aikana mahdollisesti kaatumisen jälkeen.

Jos määrität OptimizeForLowerDiskUsage TOSI, lokitiedoston tila on perusteettomasti valmistellun niin, että aktiivinen replikoiden voit tallentaa enemmän tietoja lokitiedostoja, kun passiivinen replikoiden käyttää vähemmän levytilaa. Näin isännöimiseen Lisää replikoiden solmun. Jos määrität OptimizeForLowerDiskUsage FALSE, tilatiedot kirjoitetaan kirjautuvat lokitiedostoihin nopeammin.

MaxRecordSizeInKB-asetus määrittää tietueen, joka voi kirjoittaa Replikointitoiminto lokitiedostoon enimmäiskoon. Useimmissa tapauksissa vähintään 1 024 kt tietueen oletuskoko on paras mahdollinen. Kuitenkin palvelun aiheutuuko suurempi tieto-osat tilatiedot osaksi sitten arvoksi joutua muuttamaan kasvaa. On hyötyä tekemistä MaxRecordSizeInKB pienempi kuin 1 024, pienempi tietueiden Käytä vain tarvittavaa pienempi tietueen tilaa. Odotamme, tämä arvo on voidaan muuttaa vain vain harvoin.

SharedLogId ja SharedLogPath-asetuksia käytetään aina yhdessä tehdä käyttää erillisen jaetun lokia oletusarvon jaetun lokista solmun palvelu. Paras tehokkuuden mahdollisimman monta services Määritä saman jaetun lokin. Jaetun lokitiedostojen sijoitetaan levyille, joita käytetään tuloskorteissa jaetun lokitiedostoon, voit pienentää pään siirto ristiriita. Odotamme, että nämä arvot on voidaan muuttaa vain vain harvoin.

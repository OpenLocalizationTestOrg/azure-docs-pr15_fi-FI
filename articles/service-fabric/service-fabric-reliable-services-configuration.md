<properties
   pageTitle="Yleistä Azure palvelun kangasta luotettavia palveluja määritys | Microsoft Azure"
   description="Lisätietoja tilallisten luotettava palveluiden määrittäminen Azure palvelun kangasta."
   services="Service-Fabric"
   documentationCenter=".net"
   authors="sumukhs"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/18/2016"
   ms.author="sumukhs"/>

# <a name="configure-stateful-reliable-services"></a>Tilallisten luotettava palvelujen määrittäminen

On kaksi joukkoa luotettavia palveluja asetukset. Yksi on yleinen luotettavaa palvelun klusterin, kun taas muut määrittäminen on luotettava tietyn palvelun.

## <a name="global-configuration"></a>Yleiset määritykset

Yleinen luotettavaa palvelun asetuksiin on määritetty KtlLogger-osassa klusterin klusterin luettelo. Se mahdollistaa jaetun lokin sijainnin ja koon plus lokin käyttämä yleinen muistirajoitukset. Klusterin luettelo on yksi XML-tiedosto, joka sisältää asetukset ja määritykset, jotka koskevat kaikkia solmujen ja palveluiden klusterin. Tiedoston nimi on yleensä ClusterManifest.xml. Voit tarkastella klusterin luettelon yhteyttä klusterin Get-ServiceFabricClusterManifest powershell-komennolla.

### <a name="configuration-names"></a>Määritysten nimet

|Nimi|Yksikkö|Oletusarvo|Huomautuksia|
|----|----|-------------|-------|
|WriteBufferMemoryPoolMinimumInKB|Kilotavua.|8388608|Vähimmäismäärä kt varata lokin kirjoittaminen puskurin muistin resurssivarantoon ydin-tilassa. Muistin kannan käytetään välimuistin tietoja ennen levylle kirjoittamista.|
|WriteBufferMemoryPoolMaximumInKB|Kilotavua.|Ei rajaa|Enimmäiskoko, johon lokin kirjoittaa puskurin muistin resurssivarantoon voi kasvaa.|
|SharedLogId|GUID-TUNNUS|""|Määrittää yksilöllinen GUID-tunnus, joka käyttää jaettua tiedostoa kaikissa klusterin solmuissa, joissa ei määritetä SharedLogId niiden palvelun tietyn määrityksessä luotettava palveluiden käyttämän tunnistamista varten. Jos SharedLogId on määritetty, myös SharedLogPath määritettävä.|
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

SharedLogId ja SharedLogPath-asetuksia käytetään aina yhdessä klusterin GUID-tunnus ja sijainnin oletusarvon jaetun lokin kaikkien solmujen määrittämiseen. Oletusarvon jaetun lokin käytetään kaikkien luotettavaa palvelua, joiden asetuksia ei määritetä, kyseisen palvelun settings.xml. Parhaan mahdollisen suorituskyvyn jaetun lokitiedostojen sijoitetaan levyille, joita käytetään tuloskorteissa jaetun lokitiedosto vähentää ristiriita.

SharedLogSizeInMB määrittää levytilaa kohdistettavien oletusarvon jaetun kirjautumisen kaikkien solmujen määrän.  SharedLogId ja SharedLogPath ei tarvitse erikseen, jotta SharedLogSizeInMB erikseen.


## <a name="service-specific-configuration"></a>Palvelun tietty määritys
Voit muokata tilallisten luotettava palvelujen oletusarvon käyttömahdollisuudet määritys-paketti (Config) tai palvelun käyttöönoton (koodi) avulla.

+ **Config** - määritysten config package tehdään muuttamalla Settings.xml-tiedosto, joka on luotu Microsoft Visual Studio package pääkansio jokaisen palvelun sovelluksen Config-kansiossa.
+ **Koodi** - koodin määritysten tehdään luomalla ReliableStateManager, käyttämällä ReliableStateManagerConfiguration-objekti ja määrittää haluamasi asetukset.

Oletusarvon mukaan Azure palvelun kangasta runtime etsitään ennalta määritetyn osan Settings.xml tiedostossa olevat nimet ja siinä käytetään määritysten arvojen pohjana runtime-osien luomisen aikana.

>[AZURE.NOTE] Älä **Poista osan nimet seuraavat määritykset Settings.xml-tiedosto, joka on** luotu Visual Studio-ratkaisun, paitsi jos aiot määrittää palvelun kautta koodi.
Uudelleennimeäminen config package tai osan nimet edellyttävät koodin muutos ReliableStateManager määritettäessä.


### <a name="replicator-security-configuration"></a>Monistus suojausasetukset
Monistus suojausmäärityksiä käytetään suojaamiseen tietoliikenneyhteyden, jota käytetään replikoinnin aikana. Tämä tarkoittaa, että palvelut eivät näe toistensa replikoinnin tietoliikenteen varmistetaan, että tiedot, jotka tehdään erittäin käytettävissä on myös suojattu. Tyhjä suojauksen määritykset-osasta estää oletusarvoisesti replikoinnin suojaus.

### <a name="default-section-name"></a>Osan oletusnimi
ReplicatorSecurityConfig

>[AZURE.NOTE] Jos haluat muuttaa tämän osan nimeä, ohittaa ReliableStateManagerConfiguration konstruktoria replicatorSecuritySectionName parametri tämän palvelun ReliableStateManager luotaessa.


### <a name="replicator-configuration"></a>Monistus määritys
Monistus käyttömahdollisuudet määrittäminen Monistus, joka on vastuussa tekeminen tilallisten luotettavaa palvelun tilan erittäin luotettavaa replikoiminen ja toteaa tilan paikallisesti.
Oletusarvo-määritys on luonut Visual Studio-mallin ja pitäisi riittää. Tässä osassa puheen lisämäärityksiä paranna Replikointitoiminto käytettävissä olevia tietoja.

### <a name="default-section-name"></a>Osan oletusnimi
ReplicatorConfig

>[AZURE.NOTE] Jos haluat muuttaa tämän osan nimeä, ohittaa ReliableStateManagerConfiguration konstruktoria replicatorSettingsSectionName parametri tämän palvelun ReliableStateManager luotaessa.


### <a name="configuration-names"></a>Määritysten nimet
|Nimi|Yksikkö|Oletusarvo|Huomautuksia|
|----|----|-------------|-------|
|BatchAcknowledgementInterval|Sekunnin ajan|0.015|Monistus osoitteessa toissijainen odottaa saatuaan ennen lähettämistä toiminnon takaisin kuittausta on ensisijainen, jonka ajan kuluessa. Muut kuittaussanomien voidaan lähettää toimille käsitellään aikaväliä lähetetään yksi vastaus.|
|ReplicatorEndpoint|PUUTTUU|Ei ole oletusarvoisesti--pakollinen parametri|Määritä IP-osoite ja portin ensisijainen ja toissijainen Monistus käytetään se muita ohjelmistomonistajilta yhteydessä. Tämä tulee viitata TCP-resurssin päätepistettä service-luettelossa. Lisätietoja [palvelun luettelon resurssien](service-fabric-service-manifest-resources.md) määrittämisestä päätepisteen resurssien palvelun luettelo. |
|MaxPrimaryReplicationQueueSize|Toimintojen määrä|8192|Ensisijainen jonossa toimintojen enimmäismäärä. Toiminto on vapauttanut, kun ensisijainen Monistus saa vastausta toissijainen ohjelmistomonistajilta. Tämän arvon on oltava suurempi kuin 64 ja 2 potenssiin.|
|MaxSecondaryReplicationQueueSize|Toimintojen määrä|16384|Toimien toissijaisen jonossa enimmäismäärä. Kun olet tehnyt tilaan erittäin saatavana pysyvyys on vapauttanut toiminnon. Tämän arvon on oltava suurempi kuin 64 ja 2 potenssiin.|
|CheckpointThresholdInMB|MT|50|Määrä, jonka jälkeen tila on checkpointed lokitiedoston tila.|
|MaxRecordSizeInKB|KT|vähintään 1 024|Suurin tietueen koko, joka replikointitoiminto voi kirjoittaa lokiin. Tämän arvon on oltava 4 ja yli 16 kerrannaiseen.|
|MinLogSizeInMB|MT|0 (järjestelmä määritetään)|Tapahtumien lokin vähimmäiskoko. Lokiin ei sallita katkaistava koko on jäljempänä tämän asetuksen. 0 osoittaa, että Replikointitoiminto määräytyy pienin lokin koko. Mahdollisuus osittaisen kopiot ja lisäävän varmuuskopioinnin jälkeen haluamasi katkaistaan alentaa tietueet mahdollisuuksia arvon kasvattaminen kasvaa.|
|TruncationThresholdFactor|Kerroin|2|Määrittää, mitä koossa lokin katkaisu käynnistyy. Katkaisu kynnysarvo määräytyy TruncationThresholdFactor kerrottuna MinLogSizeInMB. TruncationThresholdFactor on oltava suurempi kuin 1. MinLogSizeInMB * TruncationThresholdFactor on oltava pienempi kuin MaxStreamSizeInMB.|
|ThrottlingThresholdFactor|Kerroin|4|Määrittää, mitä koossa lokin se käynnistyy on rajoitettu. Rajoittava raja (megatavuina) määräytyy Maks ((MinLogSizeInMB *ThrottlingThresholdFactor),(CheckpointThresholdInMB* ThrottlingThresholdFactor)). Rajoittava raja (megatavuina) on oltava suurempi kuin katkaisu raja-arvon (megatavuina). Katkaisu raja-arvon (megatavuina) on oltava pienempi kuin MaxStreamSizeInMB.|
|MaxAccumulatedBackupLogSizeInMB|MT|800|Maks kumulatiivisen koon varmuuskopion lokit annetun varmuuskopion log ketjussa (megatavuina). Vaiheittainen varmuuskopion pyynnöt epäonnistuu, jos lisäävän varmuuskopioinnin luoda varmuuskopion lokitiedoston, joka voi aiheuttaa kertynyt varmuuskopion lokit haluamasi koko varmuuskopioinnin on suurempi kuin tämän jälkeen. Tässä tapauksessa käyttäjän tarvitaan koko Varmuuskopioi.|
|SharedLogId|GUID-TUNNUS|""|Määrittää yksilöllinen GUID-tunnus, käyttämään yksilöimiseen jaetun lokitiedosto tämän replikan käyttämisestä. Yleensä services Älä käytä asetusta. Kuitenkin jos SharedLogId on määritetty, valitse SharedLogPath on myös määritettävä.|
|SharedLogPath|Täydellinen polku|""|Määrittää, missä tämän replikan jaetun lokitiedoston luodaan täydellinen polku. Yleensä services Älä käytä asetusta. Kuitenkin jos SharedLogPath on määritetty, valitse SharedLogId on myös määritettävä.|
|SlowApiMonitoringDuration|Sekunnin ajan|300|Määrittää hallitun Ohjelmointirajapinnan puhelut seurantaa aikavälin. Esimerkki: käyttäjä annettu varmuuskopion takaisinkutsutoiminto. Aikavälin kuluttua, Varoitus kuntoraportti lähetetään kunto hallinta.|

### <a name="sample-configuration-via-code"></a>Esimerkki määritysten koodi
```csharp
class Program
{
    /// <summary>
    /// This is the entry point of the service host process.
    /// </summary>
    static void Main()
    {
        ServiceRuntime.RegisterServiceAsync("HelloWorldStatefulType",
            context => new HelloWorldStateful(context, 
                new ReliableStateManager(context, 
        new ReliableStateManagerConfiguration(
                        new ReliableStateManagerReplicatorSettings()
            {
                RetryInterval = TimeSpan.FromSeconds(3)
                        }
            )))).GetAwaiter().GetResult();
    }
}    
```
```csharp
class MyStatefulService : StatefulService
{
    public MyStatefulService(StatefulServiceContext context, IReliableStateManagerReplica stateManager)
        : base(context, stateManager)
    { }
    ...
}
```


### <a name="sample-configuration-file"></a>Esimerkki kokoonpanotiedosto
```xml
<?xml version="1.0" encoding="utf-8"?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Section Name="ReplicatorConfig">
      <Parameter Name="ReplicatorEndpoint" Value="ReplicatorEndpoint" />
      <Parameter Name="BatchAcknowledgementInterval" Value="0.05"/>
      <Parameter Name="CheckpointThresholdInMB" Value="512" />
   </Section>
   <Section Name="ReplicatorSecurityConfig">
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


### <a name="remarks"></a>Huomautuksia
BatchAcknowledgementInterval ohjausobjektit replikoinnin viive. Arvo on "0" tuloksena pienin mahdollinen viive kustannuksella siirtonopeuden (Katso Lisää kuittaus viestit on lähetetty ja käsitellä sisältävä vähemmän kuittaussanomien).
Mitä suurempi arvo BatchAcknowledgementInterval, suuremman yleinen replikoinnin siirtonopeuden, kustannuksella suurempi toiminnon viive. Tämä kääntää suoraan tapahtuma hyväksytään viive.

CheckpointThresholdInMB arvo ohjausobjektit, Replikointitoiminto avulla voit tallentaa sen erillinen lokitiedoston tilatietoihin levytilan määrän. Lisääntyvien tämä suuremmat arvot kuin oletusarvoista saattaa johtaa kokoonpanon uudelleenmääritys nopeammin, kun uusi replika lisätään joukkoa. Tämä on osittain tilan siirto joka suoritetaan, Lisää historia-toimintoa lokiin saatavuuden vuoksi. Tämä suurentaa replikan palautus aikana mahdollisesti kaatumisen jälkeen.

MaxRecordSizeInKB-asetus määrittää tietueen, joka voi kirjoittaa Replikointitoiminto lokitiedostoon enimmäiskoon. Useimmissa tapauksissa vähintään 1 024 kt tietueen oletuskoko on paras mahdollinen. Kuitenkin palvelun aiheutuuko suurempi tieto-osat tilatiedot osaksi sitten arvoksi joutua muuttamaan kasvaa. On hyötyä tekemistä MaxRecordSizeInKB pienempi kuin 1 024, pienempi tietueiden Käytä vain tarvittavaa pienempi tietueen tilaa. Odotamme, tämä arvo on voi muuttaa vain vain harvoin.

SharedLogId ja SharedLogPath-asetuksia käytetään aina yhdessä tehdä käyttää erillisen jaetun lokia oletusarvon jaetun lokista solmun palvelu. Paras tehokkuuden mahdollisimman monta services Määritä saman jaetun lokin. Jaetun lokitiedostojen sijoitetaan levyille, joita käytetään tuloskorteissa jaetun lokitiedosto vähentää pään siirto ristiriita. Odotamme, tämä arvo on voi muuttaa vain vain harvoin.

## <a name="next-steps"></a>Seuraavat vaiheet
 - [Visual Studio palvelun kangasta sovelluksen korjaaminen](service-fabric-debugging-your-application.md)
 - [Luotettavan palveluiden Sovelluskehittäjän opas](https://msdn.microsoft.com/library/azure/dn706529.aspx)

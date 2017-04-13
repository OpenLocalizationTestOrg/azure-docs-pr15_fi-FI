<properties
   pageTitle="Testattavuus: Palvelun viestintä | Microsoft Azure"
   description="Palvelun viestintä on kriittinen integrointi palvelun kangasta sovelluksen. Tässä artikkelissa käsitellään tyyliseikat ja testauksen avulla."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/06/2016"
   ms.author="vturecek"/>

# <a name="service-fabric-testability-scenarios-service-communication"></a>Palvelun kangasta testattavuus skenaariot: palvelun viestintä

Microservices ja luonnollisesti Azure palvelun kangasta-palvelun aloittaminen arkkitehtuuri tyylit pinta. Hajautettu arkkitehtuureihin tämäntyyppisten componentized microservice sovellusten koostuvat yleensä vaikuttavat toisiinsa jutella palveluihin. Myös helpoin tapauksissa yleensä sinulla vähintään tilattomien web-palvelu ja tilallisten tietojen tallennuspalvelu, viestintään.

Palvelun viestintä on kriittinen integrointi piste-sovelluksen, koska kunkin palvelun paljastaa remote API muihin palveluihin. API rajat joukkoa, joka liittyy yleensä i/o käsitteleminen edellyttää, että jotkin tarkkaan hyvä summa-ja kelpoisuuden.

On monia tapoja, joilla voit tehdä, kun palvelua rajoja ovat langallisen yhdessä hajautettu järjestelmä:

 - *Protokollan*. Voit käyttää parantavat yhteentoimivuus HTTP- tai mukautetun binaarinen protokollan suurin nopeus?
 - *Virheen käsittely*. Miten pysyvä ja lyhytkestoisia käsitellään? Mitä tapahtuu, kun palvelu siirtyy eri solmun?
 - *Aikakatkaisu ja viive*. Peruspalveluita sovelluksissa miten palvelun kerroksen käsittelee viive Selaa ja käyttäjälle?

Onko jollakin palvelun kangasta myöntämä sisäinen viestintä osia tai voit luoda omia testaaminen palvelujen vuorovaikutuksesta on ehdottoman tärkeää varmistaa, että sovelluksesi vikasietoisuudelle.

## <a name="prepare-for-services-to-move"></a>Siirrä services valmisteleminen

Palveluesiintymiä voi liikkua ajan kuluessa. Tämä on erityisen TOSI, kun ne on määritetty mukautettu räätälöityjä resurssien tasaamisen kuormituksen mittaukset. Palvelun kangasta siirtää palvelun-esiintymät Suurenna käytettävyyden myös päivityksiä, failovers, asteikko-kohtaa ja muissa tilanteissa, jotka ilmenevät päälle hajautettu järjestelmä elinkaaren aikana.

Kun klusterin siirtyminen palvelut, asiakkaiden ja muut palvelut laaditaan käsittelee kahden skenaarion, kun ne jutella palvelu:

- Service-esiintymän tai osion se on siirretty puhuin ihan siihen edellisen jälkeen. Tämä on Normaali palvelun elinkaari-osa ja olisi oikein sovelluksen elinkaaren aikana.
- Service-esiintymän tai osion se parhaillaan siirtäminen. Vaikka vikasietotila yhden solmun palvelun toiseen ilmenee nopeasti palvelun kangasta, voi olla viive käytettävyyden Jos palvelun viestintä-komponentti on hidas Käynnistä-painiketta.

Näissä tilanteissa käsittely tilanteen on tärkeää tasainen suoritettavien järjestelmän. Voit tehdä muistaa seuraavat asiat:

- Jokaisella palvelulla, jotka voidaan yhdistää on *osoite* , se seuraa (esimerkiksi HTTP tai WebSockets). Kun esiintymää tai osion siirtää sen osoite päätepisteen muuttuu. (Se siirtää eri solmu eri IP-osoite.) Jos käytät valmiin viestintä-osia, ne käsittelee uudelleen selvitettäessä palvelun osoitteet puolestasi.
- Palvelun viive kuin palvelun esiintymä käynnistyy sen listener tilapäinen suureneminen voi olla uudelleen. Tämä riippuu siitä, kuinka nopeasti palvelun avautuu kuuntelua jälkeen palvelun esiintymän siirretään.
- Kaikki aiemmin luodut yhteydet täytyy olla suljetaan ja avataan uudelleen, kun palvelua avautuu uusi solmu. Kaksivaiheista solmu sulkeminen tai uudelleenkäynnistyksen avulla aika sulkea tilanteen muodostetut yhteydet.

### <a name="test-it-move-service-instances"></a>Testaa se: Siirrä palveluesiintymiä

Palvelun kangasta testattavuus työkalujen avulla luot Testiskenaario voit esikatsella toteutuu eri tavalla:

1. Siirrä tilallisten palvelun ensisijainen replikan.

    Ensisijainen se tilallisten service-osion voidaan siirtää syistä määrälle. Tämän toiminnon avulla voit kohdistaa tiettyyn osioon, jos haluat nähdä, miten palvelujen reagoi, siirrä hyvin hallittu ensisijainen se.

    ```powershell

    PS > Move-ServiceFabricPrimaryReplica -PartitionId 6faa4ffa-521a-44e9-8351-dfca0f7e0466 -ServiceName fabric:/MyApplication/MyService

    ```

2. Lopeta solmu.

    Kun solmu on pysäytetty-palvelun kangasta siirtää kaikki palveluesiintymiä tai osioita, jotka olivat johonkin muiden käytettävissä solmujen klusterin solmun. Tällä komennolla voit testata tilanteeseen, jossa solmu menetetään yhteyttä klusterin ja kaikki service-esiintymät ja replikoiden-solmun on siirrettävä.

    Voit estää solmu PowerShell **Stop ServiceFabricNode** cmdlet-komennolla:

    ```powershell

    PS > Restart-ServiceFabricNode -NodeName Node_1

    ```

## <a name="maintain-service-availability"></a>Ylläpidä palvelun saatavuus

Alustan, kuin palvelun kangasta on suunniteltu suuren käytettävyyden palvelujen tarjoaminen. Mutta erittäin tapauksissa pohjana infrastruktuurin saattavat yhä aiheuttaa siitä. On tärkeää testata liian näissä tilanteissa.

Tilallisten services replikoida suuren käytettävyyden osavaltio quorum-järjestelmän avulla. Tämä tarkoittaa, että replikoiden äänistä on oltava käytettävissä suorittaa kirjoittaminen. Harvinaisissa tapauksissa juuri epäonnistui, kuten replikoiden äänistä eivät välttämättä ole käytettävissä. Tällaisissa tapauksissa ei voi suorittaa kirjoittaminen, mutta edelleen voi suorittaa luku.

### <a name="test-it-write-operation-unavailability"></a>Testaa se: Kirjoita toiminnon siitä

Voit lisätä virheen, joka aiheuttaa quorum tappio kokeeksi palvelun kangasta testattavuus työkalujen avulla. Tällainen tilanne on harvinaisissa, mutta on tärkeää, että asiakkaat ja palvelut, jotka riippuvat tilallisten palvelu on valmistettu käsittelemään tilanteissa, jossa niitä ei voi tehdä kirjoittaa pyynnöt. On myös tärkeää tilallisten palvelun itse tietää tätä mahdollisuutta ja voit toimittaa sen tilanteen soittajat.

Voi aiheuttaa quorum tappio PowerShell **Käynnistä ServiceFabricPartitionQuorumLoss** cmdlet-komennolla:

```powershell

PS > Invoke-ServiceFabricPartitionQuorumLoss -ServiceName fabric:/Myapplication/MyService -QuorumLossMode QuorumReplicas -QuorumLossDurationInSeconds 20

```

Tässä esimerkissä on määritetty `QuorumLossMode` , `QuorumReplicas` osoittamaan, että haluamme aiheuttaa quorum tappio tekemättä replikoita alaspäin. Tällä tavalla luku toiminnot ovat silti. Voit testata tilanne, jossa koko osion ei ole käytettävissä, voit määrittää tätä valitsinta `AllReplicas`.

## <a name="next-steps"></a>Seuraavat vaiheet

[Lisätietoja testattavuus toiminnot](service-fabric-testability-actions.md)

[Lisätietoja testattavuus skenaariot](service-fabric-testability-scenarios.md)

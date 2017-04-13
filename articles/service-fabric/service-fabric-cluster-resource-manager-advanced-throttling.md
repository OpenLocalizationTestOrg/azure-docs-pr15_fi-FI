<properties
   pageTitle="Palvelun kangasta klusterin Resurssienhallinta rajoitin | Microsoft Azure"
   description="Opettele määrittämään ylikuormitustilan annettu mukaan palvelun kangasta klusterin Resurssienhallinta."
   services="service-fabric"
   documentationCenter=".net"
   authors="masnider"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/19/2016"
   ms.author="masnider"/>


# <a name="throttling-the-behavior-of-the-service-fabric-cluster-resource-manager"></a>Rajoittimen toimintaa, palvelujen kangasta klusterin resurssien hallinta
Vaikka olet määrittänyt klusterin Resurssienhallinta oikein, klusterin Hae häirinneet. Esimerkiksi voi olla samanaikaisesti solmu tai vian toimialueen virheet - mitä haluat käydä, jos, joka on tapahtunut päivittämisen aikana? Resurssienhallinta yrittää parhaan korjata kaikkea, mutta kertaa tältä sinun kannattaa harkita backstop niin, että itse klusterin on kokeilemaan tasapainota (solmut, jotka olet luomassa tulee tehdä takaisin, verkon heal itse, korjattu bittien tule käyttöön). Ohjeen Tällaisia tilanteita, palvelun kangasta klusterin Resurssienhallinta sisältää useita ylikuormitustilan. Huomautus nämä ylikuormitustilan on melko häiritsevä ja yleensä ei kannata voidaan käyttää, ellei ole osa varovainen valmis ympärille rinnakkainen työmäärä, joka voidaan tehdä todella-klusterin sekä usein käytetyt matemaattiset on vastannut näihin lajittelee (ahem) suunnittelematon makroskooppiset kokoonpanon uudelleenmääritys tapahtumat (EU: "Hyvin virheelliset päivän").

Yleensä Suosittelemme välttämään hyvin virheelliset päivän muiden asetukseen (kuten säännöllisesti koodin päivitykset ja ilman overscheduling klusterin aluksi) sen sijaan, että rajoittimen yhteyttä klusterin estäminen resurssien käyttäminen, kun se yrittää korjata itse). Ylikuormitustilan on oletusarvo, jotka olet löydetyillä kokemus on ok oletusasetusten kautta, mutta olisi todennäköisesti voit tarkastella ja hienosäätää ne että odotettu todellinen latautuvat. Kun ole liian rajoittaminen tai klusterin on ehkä määrittää, että on paras käytäntö ladataan käyttötapauksiin joka (ennen kuin voit korjata ne) kohtaa, johon on oltava pari ylikuormitustilan paikassa, vaikka se tarkoittaa sitä, klusterin kestää kauemmin tasoittumista.

##<a name="configuring-the-throttles"></a>Ylikuormitustilan määrittäminen
Ylikuormitustilan, jotka kuuluvat oletusarvoisesti ovat seuraavat:

-   GlobalMovementThrottleThreshold – tämä ohjaa liikkeitä klusterin kokonaismäärän joitakin jaettuina (määritellään GlobalMovementThrottleCountingInterval arvo sekuntia)
-   MovementPerPartitionThrottleThreshold – tämä ohjaa tahansa service-osion liikkeitä kokonaismäärän joitakin jaettuina (MovementPerPartitionThrottleCountingInterval-arvon sekunnit)

``` xml
<Section Name="PlacementAndLoadBalancing">
     <Parameter Name="GlobalMovementThrottleThreshold" Value="1000" />
     <Parameter Name="GlobalMovementThrottleCountingInterval" Value="600" />
     <Parameter Name="MovementPerPartitionThrottleThreshold" Value="50" />
     <Parameter Name="MovementPerPartitionThrottleCountingInterval" Value="600" />
</Section>
```

Huomaa, että olemme katsomista asiakkaille, käytä näitä ylikuormitustilan on yleensä koska siirretyt jo ympäristössä, jossa on rajoitettu resurssi (kuten rajoitettu kaistanleveys yksittäisiä solmujen tai levyjä, joka ei ole ylöspäin rinnakkain replikan vaatimukset muodostaa on siirretty niitä) joka tarkoitetaan, että nämä toiminnot ei onnistu tai olisi hidas silti.  Näissä tilanteissa asiakkaiden on perehtynyt tietää, ne on mahdollisesti laajentaminen, kuinka paljon se noudatetaan klusterin vakaata vaiheeseen, mukaan lukien tietää, hän voi sinut käynnissä alemman yleinen luotettavuutta samalla, kun ne on rajoittanut saavuttamiseksi.

## <a name="next-steps"></a>Seuraavat vaiheet
- Saat lisätietoja siitä, miten klusterin Resurssienhallinta hallitsee ja lataa klusterin saldojen, katso artikkeli [kuormituksen tasaamisen](service-fabric-cluster-resource-manager-balancing.md)
- Klusterin Resurssienhallinta on paljon kuvaava klusterin asetukset. Selvitä lisätietoja niitä Katso tässä artikkelissa [kerrotaan palvelun kangasta klusterin](service-fabric-cluster-resource-manager-cluster-description.md)

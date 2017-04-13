<properties
   pageTitle="Palvelun kangasta klusterin resurssien hallinta - sovelluksen ryhmistä | Microsoft Azure"
   description="Yleistä sovelluksen ryhmän ominaisuudet-palvelujen kangasta klusterin resurssien hallinta"
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

# <a name="introduction-to-application-groups"></a>Johdanto sovelluksen ryhmiin
Palvelun kangasta klusterin Resurssienhallinta hallitsee yleensä levittämällä kuormituksen (vastaa kautta arvot) tasaisesti koko klusterin klusteriresursseja. Palvelun kangasta hallitsee myös kapasiteetti klusterin ja klusterin solmujen kokonaisuutena käsite kapasiteetin kautta. Tämä toimii useita erityyppisiä työmääriä, mutta kuviot, jotka helpottavat kuormitettu eri kangasta Palvelusovellusten esiintymät, joskus tuoda lisävaatimukset hienoa. Jotkin lisävaatimukset on yleensä:

- Mahdollisuus varata kapasiteettia joitakin solmujen määrän sovelluksen esiintymä-palveluille
- Mahdollisuus yhteensä, jotka määritettyjen palvelut-sovelluksesta on oikeus suorittaa solmujen määrän rajoittaminen
- Jotta voit rajoittaa luku- tai resurssin kulutus sisällä palveluja itse sovelluksen esiintymässä määrittäminen integroimiseen

Täytä näitä vaatimuksia, jotta voit kehittää tuki Soita, mikä on sovelluksen ryhmistä.

## <a name="managing-application-capacity"></a>Sovelluksen kapasiteetin hallinta
Sovelluksen kapasiteetti voidaan koottu sovelluksen sekä yhteensä metrisillä kuormituksen, sovellusten esiintymiä yksittäisiä solmuissa solmujen määrän rajoittaminen. Se voidaan myös Varaa resurssit-sovelluksen klusterin.

Sovelluksen kapasiteetti voidaan määrittää uusien sovellusten luomisen yhteydessä; sitä voi päivittää myös sovellukset, jotka on luotu määrittämättä sovelluksen kapasiteetti.

### <a name="limiting-the-maximum-number-of-nodes"></a>Suurin solmujen määrän rajoittaminen
Yksinkertaisin sovelluksen kapasiteetti käyttötapauksen on, kun sovellus-esiintymässä on oltava rajoitettu tiettyjen enimmäismäärä solmujen. Jos sovellusta ei ole kapasiteetti ei määritetä, palvelun kangasta klusterin Resurssienhallinta vahvistaa replikoiden tasaamisen eli eheytys, Normaali sääntöjen mukaisesti joka tarkoittaa yleensä, että kaikissa klusterin käytettävissä solmujen levitä palveluja, tai jos eheytys on käytössä joitakin haluamaansa mutta pienempänä solmujen määrän.

Seuraava kuva esittää sovelluksen esiintymä mahdolliset sijaintia ei ole määritetty solmujen enimmäismäärä ja sitten samaan sovellukseen solmujen enimmäismäärä määrittäminen. Huomaa, että ei ole oikeudet tekemistään mitä replikoiden tai esiintymät, joiden services Hae sijoittaa yhdessä.

![Sovelluksen esiintymää määrittäminen solmujen enimmäismäärä][Image1]

Vasen esimerkissä sovellus ei ole sovelluksen kapasiteettia ja siinä on kolme palvelua. CRM on tehnyt looginen päätös levittäminen replikoita yli kuusi käytettävissä solmujen klusterin paras tasapainon saavuttamiseksi. Oikea esimerkissä näemme sama sovellus, joka on rajoitettu kolme solmuissa ja jossa palvelun kangasta CRM on saavuttaa sovelluksen services replikoiden parhaat saldo.

Parametri, joka ohjaa ongelman kutsutaan MaximumNodes. Tämä parametri voidaan luonnin aikana tai päivittää sovelluksen esiintymää, jota on käynnissä, jossa kirjainkoko palvelun kangasta CRM rajoittaa sovelluspalveluja määritetyn enimmäismäärää solmujen replikoita.

PowerShellin

``` posh
New-ServiceFabricApplication -ApplicationName fabric:/AppName -ApplicationTypeName AppType1 -ApplicationTypeVersion 1.0.0.0 -MaximumNodes 3
Update-ServiceFabricApplication –Name fabric:/AppName –MaximumNodes 5
```

C#

``` csharp
ApplicationDescription ad = new ApplicationDescription();
ad.ApplicationName = new Uri("fabric:/AppName");
ad.ApplicationTypeName = "AppType1";
ad.ApplicationTypeVersion = "1.0.0.0";
ad.MaximumNodes = 3;
fc.ApplicationManager.CreateApplicationAsync(ad);

ApplicationUpdateDescription adUpdate = new ApplicationUpdateDescription(new Uri("fabric:/AppName"));
adUpdate.MaximumNodes = 5;
fc.ApplicationManager.UpdateApplicationAsync(adUpdate);

var appMetric = new ApplicationMetricDescription();
appMetric.Name = "Metric1";
appMetric.TotalApplicationCapacity = 1000;

adUpdate.Metrics.Add(appMetric);
```

## <a name="application-metrics-load-and-capacity"></a>Sovelluksen arvot ja lataa kapasiteetti
Sovelluksen ryhmien avulla voit määrittää sovelluksen esiintymää niin kuin ne arvot koskevat sovelluksen kapasiteetin liittyvät arvot. Esimerkiksi niin voit määrittää, kuten monia palveluita kuin haluat voi luoda

Kunkin metrijärjestelmän ovat 2 arvoja, jotka voidaan määrittää kuvaamaan sovelluksen kyseisen esiintymän kapasiteetin:

-   Yhteensä sovelluksen kapasiteetti – edustaa tietyn mittarin sovelluksen kokonaiskapasiteetti. Palvelun kangasta CRM yrittää rajoittaa summan metrisillä Lataa tämän sovelluksen palvelujen määritetty arvo;. Lisäksi sovelluksen services jo varaavat kuormituksen rajoitusta ylöspäin, jos palvelun kangasta klusterin Resurssienhallinta disallow luomisen uusia palveluita tai osioita, joka aiheuttaa yhteensä kuormituksen Siirry tämä on ylitetty.
-   Solmun kapasiteetin – määrittää sovellusten palvelujen replikoiden suurin yhteensä Lataa yksittäinen solmu. Jos yhteensä kuormituksen solmun esitellään tämä kapasiteetti-palvelun kangasta CRM yrittää siirtää replikat solmujen niin, että kapasiteetin rajoituksen noudatetaan.

## <a name="reserving-capacity"></a>Varaa kapasiteettia
Toisen sovelluksen ryhmien käytetään varmistaa, että klusterin resurssit varataan sovelluksen-esiintymän sovelluksen esiintymää ei ole vielä palveluilla tai vaikka ne eivät ole vielä muissa resurssit. Voit tarkastella, kuinka, jotka toimivat.  

### <a name="specifying-a-minimum-number-of-nodes-and-resource-reservation"></a>Solmut ja resurssien varaamiseen vähimmäismäärä määrittäminen
Resurssien varaamista vaatii erillisen sovelluksen on muutamia muita parametrien määrittäminen: *MinimumNodes* ja *NodeReservationCapacity*

- MinimumNodes - tavoin määrittämällä kohteen enimmäismäärä solmujen palvelut-sovelluksesta voi käynnissä olevat, voit myös määrittää solmujen sovellus on käynnissä olevat vähimmäismäärä. Tämä asetus määrittää tehokkaasti, resurssit varataan vähintään, solmujen määrän voimassaolevan valmiuksien klusterin luominen sovelluksen esiintymää osana.
- NodeReservationCapacity - NodeReservationCapacity voidaan määrittää kunkin metrijärjestelmän-sovelluksessa. Tämä määrittää metrisillä kuormituksen varattu sovelluksen minkä tahansa solmun jokin replikat tai sen sisältämät palveluiden esiintymiä sisältävään määrää.

Voit tarkastella osoitteessa kapasiteetin varauksen Esimerkki:

![Palvelusovellusten esiintymät varatun kapasiteetin määrittäminen][Image2]

Vasen esimerkissä sovellusten ei ole määritetty sovelluksen minkä tahansa kapasiteetti. Palvelun kangasta klusterin Resurssienhallinta saldo sovelluksen lapsen services replikoita ja esiintymät niitä muiden palveluiden (ulkopuolella sovellus) sekä taata tasapaino klusterin.

Valitse oikealla olevassa esimerkissä oletetaan, että sovellus on luotu MinimumNodes, 2, 3 ja sovelluksen arvo on määritetty varaus solmu 50 – 100, yhteensä kapasiteetista 20, max kapasiteetista MaximumNodes asettaminen palvelun kangasta varaa kapasiteettia kaksi solmuissa sininen-sovelluksen ja ei salli muiden replikoiden klusterin tarjoaman kyseisen kapasiteetti. Varattu sovelluksen tämä kapasiteetti katsotaan kulutettu ja lasketut vastaan jäljellä oleva klusterin kapasiteetti.

Kun sovellus on luotu ja varaus-klusterin Resurssienhallinta varaa kapasiteettia yhtä MinimumNodes * NodeReservationCapacity klusterin, mutta se ei ole varaa tietyn solmujen kapasiteettia, kunnes sovelluksen services replikoiden luodaan ja sijoittaa. Näin määrityksen joustavuutta, koska solmujen valitaan uusi replikoiden vain kun ne on luotu. Kapasiteetti on varattu tietyn solmun, kun se asetetaan vähintään yksi replikan.

## <a name="obtaining-the-application-load-information"></a>Sovelluksen lataaminen tietojen hankkiminen
Kunkin sovelluksen että on sovelluksen kapasiteetin määritetty voit voit hankkia ilmoittaa sen palveluiden replikoita kooste kuormituksen tietoja. Palvelun kangasta on PowerShell ja hallitun Ohjelmointirajapinnan kyselyt tähän tarkoitukseen.

Esimerkiksi kuormituksen voi hakea seuraavalla PowerShell cmdlet-komennolla:

``` posh
Get-ServiceFabricApplicationLoad –ApplicationName fabric:/MyApplication1

```

Kyselyn tulos sisältää sovelluksen kapasiteetin, joka on määritetty sovellus, esimerkiksi solmujen pienin ja suurin solmujen perustietoja. Myös ilmenee, sovellus on käytössä solmujen määrän tietoja. Näin ollen kunkin kuormituksen metrijärjestelmän hakutuloksissa näkyy tietoja tietoja:
- Metrijärjestelmän nimi: Nimi arvo.
-   Varaus kapasiteetin: Klusterin kapasiteetin, joka on varattu klusterin tämän sovelluksen.
-   Sovelluksen lataaminen: Yhteensä Lataa tämän sovelluksen lapsen replikoiden.
-   Sovelluksen kapasiteetin: Suurin sallittu arvo sovelluksen lataaminen.

## <a name="removing-application-capacity"></a>Sovelluksen kapasiteetin poistaminen
Kun sovellus kapasiteetin parametrit on määritetty sovelluksen, ne voidaan poistaa päivityksen sovelluksen API tai PowerShell cmdlet-komentojen käyttäminen. Esimerkki:

``` posh
Update-ServiceFabricApplication –Name fabric:/MyApplication1 –RemoveApplicationCapacity

```

Tämä komento poistaa kaikki sovelluksen kapasiteetin parametrit sovelluksesta ja palvelun kangasta klusterin Resurssienhallinta alkaa tämän sovelluksen kohteleminen jonkin muun sovelluksen klusteriin, joka ei ole näitä parametrit. Komento vaikuttaa välittömästi ja klusterin Resurssienhallinta poistaa kaikki tämän sovelluksen; sovelluksen kapasiteetin parametrit määrittäminen uudelleen edellyttäisi päivityksen sovelluksen API kutsuttava tarvittavat parametrit.

## <a name="restrictions-on-application-capacity"></a>Sovelluksen kapasiteetin rajoitukset
On useita rajoitukset, jotka on otettava huomioon sovelluksen kapasiteetin parametrit. Jos tarkistusvirheitä, luominen tai päivittäminen sovelluksen hylätään virheen.
Kaikki kokonaisluku parametrit on oltava positiivisia lukuja.
Lisäksi yksittäisiä parametrien rajoitukset ovat seuraavat:

-   MinimumNodes koskaan olla suurempi kuin MaximumNodes.
-   Jos valmiuksia kuormituksen arvo on määritetty, ne on noudata seuraavia sääntöjä:
  - Solmun varauksen kapasiteetti ei saa olla yli solmu kapasiteetin. Ei voi esimerkiksi rajoittaa metrijärjestelmä "Suorittimen" 2 yksikköä solmun kapasiteetti ja yrität varata 3 jokaisen solmun yksikköä.
  - Jos MaximumNodes on määritetty, valitse MaximumNodes ja solmu kapasiteetin ei saa olla suurempi kuin sovelluksen kokonaiskapasiteetti. Esimerkiksi jos määrität suurin solmu kapasiteetin kuormituksen metrijärjestelmä "Suorittimen" 8 ja voit määrittää suurin solmujen 10, valitse sovelluksen kokonaiskapasiteetti on oltava suurempi kuin 80, Lataa tämä metrijärjestelmän.

Rajoitukset on pakotettu sekä luonnin (Valitse asiakaspuolen) sekä aikana sovelluksen päivitys (Valitse palvelinpuolen). Luonnissa, tämä on esimerkki Tyhjennä rikkomuksesta vaatimusten jälkeen MaximumNodes < MinimumNodes ja komento epäonnistuu työasemaohjelmassa ennen pyynnön lähetetään myös palvelun kangasta klusterin:

``` posh
New-ServiceFabricApplication –Name fabric:/MyApplication1 –MinimumNodes 6 –MaximumNodes 2
```

Esimerkki virheellinen päivitys on seuraavasti. Jos on olemassa olevan sovelluksen ottaa ja Päivitä suurin solmujen arvoon, välittää päivitys:

``` posh
Update-ServiceFabricApplication –Name fabric:/MyApplication1 6 –MaximumNodes 2
```

Tämän jälkeen on voit yrittää päivittää pienin solmujen:

``` posh
Update-ServiceFabricApplication –Name fabric:/MyApplication1 6 –MinimumNodes 6
```

Asiakas ei ole riittävästi tietoja kontekstista-sovellusta, joten se sallii palvelun kangasta klusterin siirtää päivitys. Kuitenkin klusterin-palvelun vahvistaa uuden parametrin ja aiemmin luotuja ja epäonnistuu päivitystoimintoa, koska arvon foe pienin solmut on suurempi kuin arvo, suurin solmujen. Tässä tapauksessa sovelluksen kapasiteetin parametrit säilyvät ennallaan.

Näiden rajoitusten sijoitetaan tilauksen klusterin resurssien hallinnan voivat antaa replikoiden parhaat sijoittelua sovellusten palvelut-ikkuna.

## <a name="how-not-to-use-application-capacity"></a>Opi käyttämään ei sovelluksen kapasiteetti

-   Älä käytä sovelluksen kapasiteetti voit rajoittaa tiettyjä sovelluksen solmujen: vaikka palvelun kangasta varmistat, että suurin solmujen noudatetaan kunkin sovelluksen, joka sisältää määritetyn sovelluksen kapasiteetin käyttäjät et voi päättää sen vahvistaa-solmut. Tämä onnistuu käyttämällä sijaintia rajoitukset Services.
-   Älä käytä sovelluksen kapasiteetti varmistaa, että sama sovellus kaksi palvelut aina sijoitetaan toisiinsa rinnalla. Tämä onnistuu käyttämällä affiniteetti suhteen services ja affiniteetti voidaan rajoittaa vain palvelut, jotka todella sijoitetaan yhdessä.

## <a name="next-steps"></a>Seuraavat vaiheet
- Jos haluat lisätietoja muiden määrittämiseen käytettävissä olevat asetukset services uloskuittaus klusterin Resurssienhallinta-määrityksiä aiheen käytettävissä [tietoja palveluiden määrittäminen](service-fabric-cluster-resource-manager-configure-services.md)
- Saat lisätietoja siitä, miten klusterin Resurssienhallinta hallitsee ja jakaa kuormituksen klusterin, katso artikkeli [kuormituksen tasaamisen](service-fabric-cluster-resource-manager-balancing.md)
- Aloita alusta ja [Tutustu sovelluksen, palvelujen kangasta klusterin resurssien hallinta](service-fabric-cluster-resource-manager-introduction.md)
- Lisätietoja siitä, miten arvot toimivat yleensä Lue [Palvelun kangasta kuormituksen arvot](service-fabric-cluster-resource-manager-metrics.md)
- Klusterin Resurssienhallinta on paljon kuvaava klusterin asetukset. Selvitä lisätietoja niitä Katso tässä artikkelissa [kerrotaan palvelun kangasta klusterin](service-fabric-cluster-resource-manager-cluster-description.md)


[Image1]:./media/service-fabric-cluster-resource-manager-application-groups/application-groups-max-nodes.png
[Image2]:./media/service-fabric-cluster-resource-manager-application-groups/application-groups-reserved-capacity.png

<properties
   pageTitle="Skaalaa palvelun kangasta klusterin sisään tai ulos | Microsoft Azure"
   description="Skaalaa palvelun kangasta klusterin sisään tai ulos vastaamaan demand määrittämällä solmu tyyppi/AM asteikko kukin ehtojoukko Automaattinen skaalaus säännöt. Lisää tai poista solmujen palvelun kangasta klusteriin"
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/09/2016"
   ms.author="chackdan"/>


# <a name="scale-a-service-fabric-cluster-in-or-out-using-auto-scale-rules"></a>Skaalaa palvelun kangasta klusterin sisään tai ulos Automaattinen skaalaus sääntöjen avulla

Virtuaalikoneen asteikko ovat Azure Laske-resurssi, joiden avulla voit ottaa käyttöön ja hallita näennäiskoneiden kokoelma joukkona. Solmun verkkosivustoasi, joka on määritetty palvelun kangasta klusterissa on määritetty joukkona erillisessä AM-asteikko. Kunkin solmutyyppi skaalata sitten ulos itsenäisesti on erilaisia arvojoukkoja avoimet portit ja voi olla eri kapasiteetin arvot. Lue lisää siitä [palvelun kangasta nodetypes](service-fabric-cluster-nodetypes.md) asiakirjassa. Koska solmutyypit yhteyttä klusterin tehdään AM asteikolla palvelun kangasta asettaa osoitteessa taustaan, sinun täytyy määrittää niille sääntöjä Automaattinen skaalaus solmu tyyppi/AM asteikko kukin ehtojoukko.

>[AZURE.NOTE] Tilauksesi on oltava tarpeeksi sydämiä, jos haluat lisätä uuden VMs, jotka muodostavat tämän klusterin. Ei ole mallin tarkistaminen tällä hetkellä, niin saat käyttöönoton aika-virhe, jos jokin kiintiötä osumien.

## <a name="choose-the-node-typevm-scale-set-to-scale"></a>Valitse solmun tyyppi/AM asteikon skaalata määrittäminen

Ei ole tällä hetkellä voi määrittää AM asteikko joukkojen portaalissa Automaattinen skaalaus-säännöt, luettelon solmutyypit ja lisää ne Automaattinen skaalaus-sääntöjä niin uutiskirjeistä PowerShellin Azure (1.0 +) avulla.

Saat AM asteikko määrittäminen, jotka muodostavat yhteyttä klusterin luettelo, suorita seuraavat cmdlet-komennot:

```powershell
Get-AzureRmResource -ResourceGroupName <RGname> -ResourceType Microsoft.Compute/VirtualMachineScaleSets

Get-AzureRmVmss -ResourceGroupName <RGname> -VMScaleSetName <VM Scale Set name>
```

## <a name="set-auto-scale-rules-for-the-node-typevm-scale-set"></a>Määritä Automaattinen skaalaus säännöt solmu tyyppi/AM asteikko määrittäminen

Jos yhteyttä klusterin on useita solmutyypit, toista tämä jokaiselle solmu tyypit/AM akselille määrittää, että haluat skaalata (sisään tai ulos). Huomioon näkyvien solmujen määrän, että sinulla on oltava ennen kuin ryhdyt määrittämään Automaattinen skaalaus. Solmut, jotka sinulla on oltava ensisijainen solmu tyypin vähimmäismäärä on perustuva valitsemasi luotettavuutta tason mukaan. Lisätietoja [luotettavuutta tasot](service-fabric-cluster-capacity.md).

>[AZURE.NOTE]  Ensisijainen solmu alaspäin skaalaus kirjoittamalla pienempi kuin pienin luku Tee epävakaa klusterin tai Siirrä. Tämä voi aiheuttaa tietojen menettämisen sovellukset ja järjestelmän-palveluita.

Tällä hetkellä Automaattinen skaalaus-ominaisuus ei ole perustuva latautuu, sovellukset voivat raportoida palvelun kangasta mukaan. Tällä hetkellä automaattinen-akseli, saat laskettuna perustuva mukaan suorituskyvyn laskureita, jotka ovat lähettämän kunkin AM skaalata niin määrittäminen esiintymät.  

Noudata näitä ohjeita [mittakaavan AM asteikko kukin ehtojoukko automaattinen määrittäminen](../virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-overview.md).

>[AZURE.NOTE] Ellei solmutyyppi on kestävyyttä taso kultaa tai hopea alaspäin skenaarion mittakaava, sinun on Soita tarvittavat solmu-niminen [Poista ServiceFabricNodeState cmdlet-komento](https://msdn.microsoft.com/library/azure/mt125993.aspx) .

## <a name="manually-add-vms-to-a-node-typevm-scale-set"></a>Solmun tyyppi/AM asteikko joukon VMs lisääminen manuaalisesti

Noudata [pikaopas mallivalikoiman](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) kunkin Nodetype VMs määrän muuttaminen. 

>[AZURE.NOTE] VMs lisääminen kestää jonkin aikaa, joten ei odoteta olevan välittömästi lisäykset. Suunnitteleminen niin Lisää kapasiteetin sekä aika, jos haluat sallia yli 10 minuutin ennen kuin AM resursseja on käytettävissä replikat / palvelun esiintymät Hae kohta.

## <a name="manually-remove-vms-from-the-primary-node-typevm-scale-set"></a>Voit poistaa VMs manuaalisesti olevasta ensisijainen solmu tyyppi/AM asteikko

>[AZURE.NOTE] Palvelun kangasta järjestelmän-palvelut toimivat yhteyttä klusterin ensisijainen solmutyyppi. Niin, että olisi koskaan sammuta tai skaalata alaspäin kerrat, solmu tyypeissä pienempi kuin mitä luotettavuutta tason takaa. Lisätietoja [tähän luotettavuutta tasoa tiedot](service-fabric-cluster-capacity.md). 

Sinun täytyy suorittaa seuraavat vaiheet yksi AM esiintymä kerrallaan. Näin järjestelmän palveluita (ja tilallisten palvelujen) sulkea tilanteen AM esiintymä poistetaan ja uusia replikoita solmujen on luotu.

1. Suorita [Käytöstä ServiceFabricNode](https://msdn.microsoft.com/library/mt125852.aspx) palveluita 'RemoveNode' käytöstä solmu aiot poistaa (solmutyypin suurin esiintymän).

2. Suorittamalla [Get-ServiceFabricNode](https://msdn.microsoft.com/library/mt125856.aspx) varmistaaksesi, että solmu on varmasti transitioned käytöstä. Jos et, odota, kunnes solmu on poistettu käytöstä. Tämä vaihe ei hurry.

2. Noudata [pikaopas mallit-osa](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) kerrallaan, Nodetype VMs määrän muuttaminen. Poistaa esiintymä on suurin AM-esiintymä. 

3. Toista vaiheet 1 – 3 haluamallasi tavalla, mutta skaalata aina alaspäin ensisijainen solmutyypit pienempi kuin luotettavuutta tason takaa esiintymien määrän. Lisätietoja [tähän luotettavuutta tasoa tiedot](service-fabric-cluster-capacity.md). 

## <a name="manually-remove-vms-from-the-non-primary-node-typevm-scale-set"></a>Perus solmu tyyppi/AM asteikko määrittäminen VMs poistaminen manuaalisesti

>[AZURE.NOTE] Tilallisten palvelun sinun on tietty määrä solmujen käytettävyys aina enintään voidaan ylläpitää ja säilyttää palvelun kunto. On erittäin pienin sinun on näkyvien solmujen määrän yhtä suuri kuin kohde replikan määrittäminen osion/palvelun. 

Suorita seuraavat vaiheet yksi AM esiintymää on kerrallaan. Näin järjestelmän palveluita (ja tilallisten palvelujen) sulkea tilanteen AM-esiintymässä poistetaan ja uusia replikoita luodaan muita where.

1. Suorita [Käytöstä ServiceFabricNode](https://msdn.microsoft.com/library/mt125852.aspx) palveluita 'RemoveNode' käytöstä solmu aiot poistaa (solmutyypin suurin esiintymän).

2. Suorittamalla [Get-ServiceFabricNode](https://msdn.microsoft.com/library/mt125856.aspx) varmistaaksesi, että solmu on varmasti transitioned käytöstä. Jos et odota, kunnes solmu on poistettu käytöstä. Tämä vaihe ei hurry.

2. Noudata [pikaopas mallit-osa](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) kerrallaan, Nodetype VMs määrän muuttaminen. Tämä toiminto poistaa nyt suurin AM esiintymä. 

3. Toista vaiheet 1 – 3 haluamallasi tavalla, mutta skaalata aina alaspäin ensisijainen solmutyypit pienempi kuin luotettavuutta tason takaa esiintymien määrän. Lisätietoja [tähän luotettavuutta tasoa tiedot](service-fabric-cluster-capacity.md).

## <a name="behaviors-you-may-observe-in-service-fabric-explorer"></a>Näyttöön saattaa tulla kangasta Explorerissa toiminnat

Voit skaalata määrittäminen klusterin palvelun kangasta Explorer vaikuttavat näkyvien solmujen määrän (AM asteikko määritetty esiintymiä), jotka ovat osa klusterin.  Voit skaalata klusterin alaspäin sinun tulee näkyviin näkyvät virheellisessä tilassa, ellet [Poista ServiceFabricNodeState cmd](https://msdn.microsoft.com/library/mt125993.aspx) Soita tarvittavat solmu-niminen poistetaan solmu/AM esiintymä.   

Seuraavassa on ongelman kuvaus.

Luettelossa kangasta Explorerissa solmut ovat heijastus, mitä palvelun kangasta järjestelmän palveluja (Kankkulan erityisesti) tietää klusterin oli/on solmujen määrän. Kun joukko AM asteikon skaalata, AM on poistettu mutta Kankkulan järjestelmäpalvelulla edelleen saattavat mukaan solmu (joka on yhdistetty, jotka on poistettu AM) tulee uudelleen. Palvelun kangasta Explorer säilyy niin näyttää solmun (vaikka kuntotietojen voi olla virhe tai tuntematon).

Jotta voit varmistaa, että solmu poistetaan, kun AM poistetaan, sinulla on kaksi vaihtoehtoa:

1) Valitse kultaa tai hopea kestävyyttä käyttöoikeustaso (käytettävissä pian) yhteyttä klusterin solmu-tyypin, joiden avulla voit infrastruktuuri-integrointi. Jossa sitten poistaa automaattisesti solmut järjestelmän services (Kankkulan) meidän tilasta voit skaalata.
Viitata [tähän kestävyyttä tasojen tiedot](service-fabric-cluster-capacity.md)

2) AM esiintymää on ollut skaalata, kun haluat kutsua [Poista ServiceFabricNodeState cmdlet-komento](https://msdn.microsoft.com/library/mt125993.aspx).

>[AZURE.NOTE] Palvelun kangasta klustereiden edellyttävät tietyn määrän solmujen olla määrittäminen aina säilyttämiseksi käytettävyys ja säilyttää tila - kutsutaan "ylläpito quorum." Näin on on yleensä vaarallinen sulkeutumaan klusterin kaikissa tietokoneissa, paitsi jos olet jo suorittanut [Varmuuskopiointi osavaltio](service-fabric-reliable-services-backup-restore.md).

## <a name="next-steps"></a>Seuraavat vaiheet
Lue lisätietoja myös klusterin kapasiteetin suunnittelu, klusterin päivittäminen ja jakaminen services seuraavasti:

- [Klusterin kapasiteetin suunnitteleminen](service-fabric-cluster-capacity.md)
- [Klusterin päivitykset](service-fabric-cluster-upgrade.md)
- [Osion tilallisten palvelut suurin asteikko](service-fabric-concepts-partitioning.md)

<!--Image references-->
[BrowseServiceFabricClusterResource]: ./media/service-fabric-cluster-scale-up-down/BrowseServiceFabricClusterResource.png
[ClusterResources]: ./media/service-fabric-cluster-scale-up-down/ClusterResources.png

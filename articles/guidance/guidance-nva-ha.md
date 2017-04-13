<properties
   pageTitle="Käyttöönotto virtual laitteiden suuren käytettävyyden | Microsoft Azure"
   description="Voit ottaa verkon virtual laitteiden suuren käytettävyyden."
   services=""
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/21/2016"
   ms.author="telmos"/>

# <a name="deploying-virtual-appliances-in-high-availability"></a>Suuri käytettävyys-virtual laitteiden käyttöönotto

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Tässä artikkelissa käsitellään verkon virtual laitteiden (NVAs) käyttöönoton erittäin käytettävissä tavalla käytännöt joukko. Ennen kuin jatkat, varmista, että tiedät, miten [käyttäjän määrittämä tiet (UDR)] [ udr-overview] ja [kuormituksen] [ lb-overview] työpaikan Azure-tietokannassa.

Voit eri NVAs käytettävissä Azure Marketplacesta laajentamaan Azure laitteiden käyttäminen paikallisen palvelinkeskuksen samalla tavalla. Seuraavassa kuvassa on esimerkki käyttöönoton [yksittäisen NVA] [ nva-scenario] kuin palomuuri-laitteen. 

![[0]][0]

Vaikka edellisen käyttöönoton toimii, esitellään virheen yhden pisteen. Jos virtual laitteen epäonnistuu, ei ole liikenne jatkuu. Voit ratkaista tämän ongelman haluat käyttää useita NVAs. Kuitenkin, joka edellyttää myös muita asetuksia ja resursseja, jota käytetään tarpeen mukaan.

Ottaa käyttöön laajasti käytettävissä NVA-ympäristön jollakin seuraavista toimista.

|Ratkaisu|Edut|Huomioon otettavia seikkoja|
|---|---|---|
|Tunkeutumisen kerroksen 7 virtual laitteiden kanssa|Kaikki solmut ovat käytössä|Vaatii NVA, voit lopettaa yhteydet ja käyttää SNAT<br/>Edellyttää joukko NVAs Internetistä ja Azure-liikenne<br/>Voidaan käyttää vain tietoliikenteen Azure ulkopuolelta|
|Tunkeutumisen-juniin kerroksen 7 virtual laitteiden kanssa|Kaikki solmut ovat käytössä<br/>Voit käsitellä peräisin Azure liikenne |Vaatii NVA, voit lopettaa yhteydet ja käyttää SNAT<br/>Edellyttää joukko NVAs Internetistä ja Azure-liikenne|
|PIP UDR valitsin|Kaikki tietoliikenteen NVAs yhtenäisiä<br/>Voit käsitellä kaikki liikenne (ei rajoitusta portin sääntöjen)|Aktiivinen-passiivinen<br/>Edellyttää automaattisesti prosessi|

## <a name="ingress-with-layer-7-virtual-appliances"></a>Tunkeutumisen kerroksen 7 virtual laitteiden kanssa
NVAs joukko takana Azure kuormituksen avulla voit muodostaa yhteys Azure työmääriä takana on pieni joukko palvelinpuolen portit (kuten HTTP ja HTTPS). Seuraavassa kuvassa korostaa miten suuren käytettävyyden tässä skenaariossa NVA tasolla.

![[1]][1]

Tässä skenaariossa käyttää verkon virtual laitteen on lopettaa kaikki yhteydet ja välittää niitä työmäärää aliverkon. Kuormituksen näennäiskoneiden (VMs) vastaa NVA ne vastaanotettu pyynnön ja liikenne kulkee ongelmaton. 

## <a name="ingress-egress-with-layer-7-virtual-appliances"></a>Tunkeutumisen-juniin kerroksen 7 virtual laitteiden kanssa
Voit laajentaa edellisen arkkitehtuuri ja lisätä toisen NVAs käsitellään liikenteen peräisin Azure NVAs, joita seuraavassa kuvassa osoitetulla tavalla:

![[2]][2]

Tässä skenaariossa peräisin olevien Azure kaikki liikenne reititetään sisäinen kuormituksen, joka jakaa kuormituksen on erilaisia NVAs välillä. Nämä NVAs suora liikenne Internetissä niiden yksittäiset julkiseen IP-osoitteet. 

## <a name="pip-udr-switch"></a>PIP UDR valitsin
Voit välttää useiden NVA pinoa luominen kahden NVAs Aktiivinen-passiivinen-tilassa. Tässä skenaariossa voit siirtyä julkiseen IP-osoite (PIP) ja käyttäjän määrittämät tiet (UDRs), kun aktiivisen solmun lopettaa.  

![[3]][3]

Tässä skenaariossa on samanlainen kuin yhden NVA skenaariota. Ainoa ero on se, että PIP ja UDRs on vaihdettava liikenne NVAs välillä siirtyminen. Nämä muutokset voidaan tehdä manuaalisesti tai voit myös automatisoida niitä. Jos haluat automatisoida, voit ottaa sovelluksen, joka tarkistaa aktiivisen solmun terveyteen sekä NVAs. Aktiivinen solmu ei ole käytettävissä, kun sovellus muuttaa PIP ja UDRs passiivista solmu linkittäminen.

Tämä ratkaisu mahdollista soveltaminen on käyttää [ZooKeeper] [ zookeeper] daemon-NVAs käsittelemään Täytemerkki osavaltioiden (päättäminen solmun on aktiivinen solmu). Kun johtaja on valittuna, se kutsuu Azure REST API PIP poistaminen epäonnistui solmu ja liitä se johtajalla. Kannattaa myös muuttaa UDRs osoittamaan uuden johtajan yksityinen IP-osoite.

## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja ottamisesta käyttöön [DMZ Azure ja paikallisen palvelinkeskuksen] [ dmz-on-prem] kerroksen 7 NVAs avulla.
- Lisätietoja ottamisesta käyttöön [DMZ Azure ja Internetin välille] [ dmz-internet] kerroksen 7 NVAs avulla.

<!-- links -->
[udr-overview]: ../virtual-network/virtual-networks-udr-overview.md
[lb-overview]: ../load-balancer/load-balancer-overview.md
[zookeeper]: https://zookeeper.apache.org/
[nva-scenario]: ../virtual-network/virtual-network-scenario-udr-gw-nva.md
[dmz-on-prem]: guidance-iaas-ra-secure-vnet-hybrid.md
[dmz-internet]: guidance-iaas-ra-secure-vnet-dmz.md

<!-- images -->
[0]: ./media/guidance-nva-ha/single-nva.png "Yksittäisen NVA-arkkitehtuuri"
[1]: ./media/guidance-nva-ha/l7-ingress.png "Kerroksen 7 tunkeutumisen"
[2]: ./media/guidance-nva-ha/l7-ingress-egress.png "Kerroksen 7 tunkeutumisen ja ulospääsy"
[3]: ./media/guidance-nva-ha/active-passive.png "Aktiivinen-passiivinen klusterin"
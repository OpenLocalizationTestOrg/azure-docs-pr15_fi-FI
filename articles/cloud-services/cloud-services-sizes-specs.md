<properties
 pageTitle="Pilvipalveluihin koot | Microsoft Azure"
 description="Luettelo Azure cloud palvelun verkko- ja työntekijä roolien eri virtuaalikoneen koot (ja tunnukset)."
 services="cloud-services"
 documentationCenter=""
 authors="Thraka"
 manager="timlt"
 editor=""/>
<tags
 ms.service="cloud-services"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="tbd"
 ms.date="10/27/2016"
 ms.author="adegeo"/>

# <a name="sizes-for-cloud-services"></a>Koot pilvipalveluihin

Tässä ohjeaiheessa kerrotaan käytettävissä kokojen ja pilvipalvelussa roolin esiintymien (web roolit ja työntekijä roolit) asetukset. Se sisältää myös käyttöönottoon liittyviä huomioita pitää mielessä, kun suunnittelet näiden resurssien. Jokaisen koon on tunnuksen, joka sijoitetaan [palvelun määritystiedostoa](cloud-services-model-and-package.md#csdef).

Cloud Services-palvelut on erilaisia Laske resurssien Azure tarjoamia. Napsauttamalla [tätä](cloud-services-choose-me.md) Saat lisätietoja pilvipalveluihin.

> [AZURE.NOTE]Aiheeseen liittyvät Azure rajoitukset on ohjeartikkelissa [Azure tilaus ja palvelun rajoitukset, kiintiöiden ja rajoitukset](../azure-subscription-service-limits.md)

## <a name="sizes-for-web-and-worker-role-instances"></a>Web- ja Työntekijä roolin esiintymät koot

On useita vakiokoosta Azure-valittavana. Huomioitavaa joitakin näistä koot ovat seuraavat:

* D-sarjan VMs on suunniteltu sovelluksia, jotka vaatimus suurempi Laske power ja tilapäinen suorituskyvyn. D-sarjan VMs tilapäinen levyn avulla voit nopeampaa suorittimien ja muistin core suhde SSD asema (Suoritettaessa). Lisätietoja on artikkelissa Azure blogista [Uusi D-sarjan virtuaalikoneen koot](https://azure.microsoft.com/blog/2014/09/22/new-d-series-virtual-machine-sizes/)ilmoituksen.

* Dv2 sarjan, alkuperäinen D-laskutoimitusta, seuraaja-välilehdessä tehokkaampi Suoritin. Dv2 sarjan Suoritin on noin 35 % D-sarjan suorittimen nopeammin. Se perustuu uusinta 2,4 GHz Intel Xeon® E5-2673 v3 suoritin (Haswell) ja Intel Turbo tehon lisäys tekniikka-2.0, voit siirtyä enintään 3.1 GHz. Dv2-sarjan on sama muistia ja määrityksiä D-arvosarjaa.

*   G-sarjan VMs tarjota eniten muistia ja suorittamisesta isännät, joissa on Intel Xeon E5 V3 perhe-suorittimista.

*   A-sarjan VMs voidaan ottaa käyttöön erilaisilla laitteiston tiedostotyypit ja -suorittimista. Kokoa on rajoitettu perustuvan laitteiston tarjota yhdenmukaisia suorittimen suoritettavan riippumatta siitä, kun laite on otettu käyttöön. Voit selvittää, tämä koko on otettu käyttöön fyysinen laitteisto-kyselyn virtual laitteisiin, virtuaalikoneen kuluessa.

*   A0 koko on perusteettomasti merkityn fyysinen laitteessa. Tämä koon vain muut asiakkaan ominaisuuksissa vaikuttaa käynnissä työmäärää suorituskykyä. Suhteellinen suorituskyky on rajattu alla odotettu perusaikataulun veloittaa lähes vaihtelevuudesta on 15 %.


Virtuaalikoneen koko vaikuttaa hinnoittelua. Koko vaikuttaa myös virtuaalikoneen käsittelyn, muistin ja tallennustilaa kapasiteetti. Tallennustilan kustannukset lasketaan erikseen perusteella sivuilla avattujen tallennustilan tilin. Lisätietoja on artikkelissa [Näennäiskoneiden hinnat tiedot](https://azure.microsoft.com/pricing/details/virtual-machines/) ja [Azure-tallennustilan hinnoittelusta](https://azure.microsoft.com/pricing/details/storage/). 


Seuraavat asiat voi olla apua siihen, valitse koko on:


* A8 A11 ja H-sarjan koot kutsutaan myös *Laske paljon esiintymät*. Laitteet, jotka suoritetaan näitä kokoja on suunniteltu ja Laske paljon optimoitu ja verkon paljon sovellukset, myös tehokas tietojenkäsittely (HPC) klusterin sovellukset, mallinnus ja simulaatioita. A8 A11 sarjan käyttää Intel Xeon E5-2670 @ 2.6 GHZ ja H-sarjan käyttää Intel Xeon E5-2667 v3 @ 3.2 GHz. Yksityiskohtaiset tiedot ja huomioon otettavia seikkoja näitä kokoja käyttämisestä on artikkelissa [tietoja H-sarjan ja Laske paljon A sarjan VMs](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md). 

* Dv2 sarjan, D-sarjan, G-sarjan soveltuvat erinomaisesti sovelluksia, jotka vaatimus nopeampaa suorittimessa, parempi paikallisen levyn suorituskykyä tai jos käytössäsi on suurempi muistin vaatimukset.  Ne tarjoavat useita yrityksen arvosanojen sovellusten tehokas yhdistelmä.

*   Jotkin fyysinen isännät Azure tietojen keskikohdan mukaan-eivät tue suuremman virtuaalikoneen kokoja, kuten A5 – A11. Tämän vuoksi näkyviin voi tulla virhesanoma **ei voitu määrittää virtuaalikoneen {tietokoneen nimi}** tai **ei voi luoda virtuaalikoneen {tietokoneen nimi}** aiemmin virtual-koneen uuteen kokoon; koon muuttaminen uuden virtual koneen luominen virtual verkon luotu ennen huhtikuussa 16-2013; tai lisätä uuden virtual koneen aiemmin pilvipalveluun. Katso [Virhe: "Virtuaalikoneen määrittäminen epäonnistui"](https://social.msdn.microsoft.com/Forums/9693f56c-fcd3-4d42-850e-5e3b56c7d6be/error-failed-to-configure-virtual-machine-with-a5-a6-or-a7-vm-size?forum=WAVirtualMachinesforWindows) -tuki-keskustelupalsta kunkin käyttöönottotapa ratkaisuehdotuksia.  

* Tilauksen myös saattaa rajoittaa tiettyjä koon perheille käyttöönottoa sydämiä määrä. Kun haluat lisätä kiintiön, Azure tuelta.


## <a name="performance-considerations"></a>Suorituskykyyn liittyviä tietoja

Microsoft on luonut käsitteestä, Azure Laske yksikkö (ACU) voit verrata Laske (Suoritin) suorituskyvyn Azure tuotteissa eri lisäämistapaa. Tämän avulla voit helposti tunnistaa tuote on todennäköisesti suorituskyvyn tarpeiden tyydyttämiseksi.  Valitse pieni (Standard_A1) on tällä hetkellä standardoitu ACU on 100 ja kaikissa tuotteissa sitten AM edustavat noin kuinka paljon nopeammin tuote voidaan suorittaa vakio ensisijainen. 

>[AZURE.IMPORTANT] ACU on vain yleisohjeet.  Havainnollistamiseen tulokset saattavat vaihdella. 

<br>

|TUOTE-tuoteperhe |ACU/Core |
|---|---|
|[Standard_A0](#a-series)   |50 |
|[Standard_A1-4](#a-series) |100 |
|[Standard_A5 – 7](#a-series) |100 |
|[A8 A11](#a-series)    |225 *|
|[D1 14](#d-series) |160 |
|[D1 15v2](#dv2-series) |210 – 250 *|
|[G1 – 5](#g-series)  |180 - 240 *|
|[H](#h-series) |290 – 300 *|

Merkitty ACUs * Intel® Turbo tekniikka avulla voit parantaa suorittimen korkojakso ja anna suorituskyvyn tehon lisäys.  Tehon lisäys määrä voi muuttua AM kokoa, työmäärää ja muut työmääriä käynnissä saman isännän mukaan.

## <a name="size-tables"></a>Taulukoiden kokoa

Seuraavissa taulukoissa on koot ja ne sisältävät valmiuksia.

* Tallennustilan näkyy GiB tai vähintään 1 024 yksiköiden ^ 3 tavua. Kun vertailu levyjen mitataan Gigatavua (1000 ^ 3 tavua) mitattuna GiB levyille (vähintään 1 024 ^ 3) muista, että alan GiB kapasiteetti-numerot voivat näkyä pienempi. Esimerkiksi 1023 GiB = 1098.4 gt

* Levyn siirtonopeuden mitataan i/toimintoja sekunnissa (IOPS) ja MBps missä MBps = 10 ^ 6 tavua/sec.

* Tietoja levyjen voi toimia välimuistissa olevia tai sellaisten tilat. Välimuistiin tallennetut tiedot levyn toiminnolle host-välimuistitila määritetään **vain luku** - tai **ReadWrite**.  Sellaisten tietojen levyn toiminnolle host-välimuistitila on määritetty **ei mitään**.

* Suurin kaistanleveys on suurin koostetun kaistanleveyden varattu ja määritetyn AM tyyppiä kohden. Verkon kaistanleveyden enimmäismäärä on ohjeet valitsemalla oikean AM tyypin, jotta riittävä verkon kapasiteettia on käytettävissä. Liikkuva pieni, Normaali, suuri ja erittäin suuri välillä siirtonopeuden kasvattaa vastaavasti. Todellinen verkon suorituskykyyn riippuu verkon ja sovelluksen Lataa sovellus verkkoasetukset monet eri tekijät.


## <a name="a-series"></a>A-sarjan

| Kokoa        | Suorittimen sydämiä | Muisti: GiB | Paikallinen Kiintolevylle: GiB | Maks tietojen levyjen | Maks tietojen levyn siirtonopeuden: IOPS | Maks NIC / kaistanleveyttä |
|-------------|-----------|--------------|-----------------------|----------------|--------------------|-----------------------|
| Standard_A0 | 1         | 0.768        | 20                    | 1              | 1 x 500              | 1 / pienen                   |
| Standard_A1 | 1         | 1,75         | 70                    | 2              | 2 x 500              | 1 / Keskitaso              |
| Standard_A2 | 2         | 3,5 GIGATAVUA       | 135                   | 4              | 4-x 500              | 1 / Keskitaso              |
| Standard_A3 | 4         | 7            | 285                   | 8              | 8 x 500              | 2 / suuri                  |
| Standard_A4 | 8         | 14           | 605                   | 16             | 16 x 500             | 4 / suuri                  |
| Standard_A5 | 2         | 14           | 135                   | 4              | 4-X 500              | 1 / Keskitaso              |
| Standard_A6 | 4         | 28           | 285                   | 8              | 8 x 500              | 2 / suuri                  |
| Standard_A7 | 8         | 56           | 605                   | 16             | 16 x 500             | 4 / suuri                  |

## <a name="a-series---compute-intensive-instances"></a>A-sarjat-Laske paljon esiintymiä

Ja huomioon otettavia seikkoja näitä kokoja käyttämisestä on artikkelissa [tietoja H-sarjan ja Laske paljon A sarjan VMs](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md).


| Kokoa         | Suorittimen sydämiä | Muisti: GiB | Paikallinen Kiintolevylle: GiB | Maks tietojen levyjen | Maks tietojen levyn siirtonopeuden: IOPS | Maks NIC / kaistanleveyttä |
|--------------|-----------|--------------|-----------------------|----------------|--------------------|-----------------------|
| Standard_A8 * | 8         | 56           | 382                   | 16             | 16 x 500             | 2 / suuri                  |
| Standard_A9 * | 16        | 112          | 382                   | 16             | 16 x 500             | 4 / erittäin suuri             |
| Standard_A10 | 8         | 56           | 382                   | 16             | 16 x 500             | 2 / suuri                  |
| Standard_A11 | 16        | 112          | 382                   | 16             | 16 x 500             | 4 / erittäin suuri             |

* RDMA pystyvät

## <a name="d-series"></a>Sarjan D


| Kokoa         | Suorittimen sydämiä | Muisti: GiB | Paikallinen Suoritettaessa: GiB | Maks tietojen levyjen | Maks tietojen levyn siirtonopeuden: IOPS | Maks NIC / kaistanleveyttä |
|--------------|-----------|--------------|----------------------|----------------|--------------------|-----------------------|
| Standard_D1  | 1         | 3,5          | 50                   | 2              | 2 x 500              | 1 / Keskitaso              |
| Standard_D2  | 2         | 7            | 100                  | 4              | 4-x 500              | 2 / suuri                  |
| Standard_D3  | 4         | 14           | 200                  | 8              | 8 x 500              | 4 / suuri                  |
| Standard_D4  | 8         | 28           | 400                  | 16             | 16 x 500             | 8 / suuri                  |
| Standard_D11 | 2         | 14           | 100                  | 4              | 4-x 500              | 2 / suuri                  |
| Standard_D12 | 4         | 28           | 200                  | 8              | 8 x 500              | 4 / suuri                  |
| Standard_D13 | 8         | 56           | 400                  | 16             | 16 x 500             | 8 / suuri                  |
| Standard_D14 | 16        | 112          | 800                  | 32             | 32-x 500             | 8 / erittäin suuri             |

## <a name="dv2-series"></a>Dv2 sarja

| Kokoa            | Suorittimen sydämiä | Muisti: GiB | Paikallinen Suoritettaessa: GiB | Maks tietojen levyjen | Maks tietojen levyn siirtonopeuden: IOPS | Maks NIC / kaistanleveyttä |
|-----------------|-----------|--------------|----------------------|----------------|--------------------|-----------------------|
| Standard_D1_v2  | 1         | 3,5          | 50                   | 2              | 2 x 500              | 1 / Keskitaso              |
| Standard_D2_v2  | 2         | 7            | 100                  | 4              | 4-x 500              | 2 / suuri                  |
| Standard_D3_v2  | 4         | 14           | 200                  | 8              | 8 x 500              | 4 / suuri                  |
| Standard_D4_v2  | 8         | 28           | 400                  | 16             | 16 x 500             | 8 / suuri                  |
| Standard_D5_v2  | 16        | 56           | 800                  | 32             | 32-x 500             | 8 / erittäin suuri        |
| Standard_D11_v2 | 2         | 14           | 100                  | 4              | 4-x 500              | 2 / suuri                  |
| Standard_D12_v2 | 4         | 28           | 200                  | 8              | 8 x 500              | 4 / suuri                  |
| Standard_D13_v2 | 8         | 56           | 400                  | 16             | 16 x 500             | 8 / suuri                  |
| Standard_D14_v2 | 16        | 112          | 800                  | 32             | 32-x 500             | 8 / erittäin suuri        |
| Standard_D15_v2 | 20        | 140          | 1 000                | 40             | 40 x 500             | 8 / erittäin suuri        |

## <a name="g-series"></a>G sarja

| Kokoa        | Suorittimen sydämiä | Muisti: GiB  | Paikallinen Suoritettaessa: GiB  | Maks tietojen levyjen | Maks-levyn siirtonopeuden: IOPS | Maks NIC / kaistanleveyttä |
|-------------|-----------|--------------|----------------------|----------------|--------------------|-----------------------|
| Standard_G1 | 2         | 28           | 384                  | 4              | 4-x 500            | 1 / suuri                  |
| Standard_G2 | 4         | 56           | 768                  | 8              | 8 x 500            | 2 / suuri                  |
| Standard_G3 | 8         | 112          | 1,536                | 16             | 16 x 500           | 4 / erittäin suuri             |
| Standard_G4 | 16        | 224          | 3 072                | 32             | 32-x 500           | 8 / erittäin suuri        |
| Standard_G5 | 32        | 448          | 6,144                | 64             | 64-x 500           | 8 / erittäin suuri        |


## <a name="h-series"></a>H-sarjan

H-sarjan azuren näennäiskoneiden on seuraava luonti erinomainen suorituskyky tietojenkäsittely VMs pyritään hyvin Lopeta laskennallinen tarpeita, kuten molekyyli mallinnus ja laskennallinen neste dynamics. Nämä 8 ja 16 core VMs on suunniteltu DDR4 muistia ja paikallinen Suoritettaessa mukaan tallennus Intel Haswell E5 2667 V3 suoritin-tekniikkaa. 

Lisäksi merkittäviin suorittimen potenssiin H-sarjan on lyhyt odotusaika RDMA tuomasta käyttämällä FDR InfiniBand ja useita muistin määritykset tukemaan muistin tehostettu laskennallinen vaatimukset monipuolisen vaihtoehtoja.


| Kokoa           | Suorittimen sydämiä | Muisti: GiB | Paikallinen Suoritettaessa: GiB | Maks tietojen levyjen | Maks-levyn siirtonopeuden: IOPS | Maks NIC / kaistanleveyttä |
|----------------|-----------|-------------|--------------------------|----------------|---------------------------|------------------------------|
| Standard_H8    | 8         | 56          | 1 000                     | 16             | 16 x 500                    | 8 / suuri                      |
| Standard_H16   | 16        | 112         | 2000: ssa                     | 32             | 32-x 500                    | 8 / erittäin suuri                  |
| Standard_H8m   | 8         | 112         | 1 000                     | 16             | 16 x 500                    | 8 / suuri                      |
| Standard_H16m  | 16        | 224         | 2000: ssa                     | 32             | 32-x 500                    | 8 / erittäin suuri                 |
| Standard_H16r * | 16        | 112         | 2000: ssa                     | 32             | 32-x 500                    | 8 / erittäin suuri                  |
| Standard_H16mr * | 16        | 224         | 2000: ssa                     | 32             | 32-x 500                    | 8 / erittäin suuri                  |


* RDMA pystyvät

## <a name="notes-standard-a0---a4-using-cli-and-powershell"></a>Huomautus: Vakio A0 - A4 CLI ja PowerShellin avulla 

Perinteinen käyttöönotto-mallissa jotkin AM koon nimet ovat hieman erilaiset CLI ja PowerShell:

* Standard_A0 on ExtraSmall 
* Standard_A1 on pieni
* Standard_A2 on Normaali
* Standard_A3 on suuri
* Standard_A4 on ExtraLarge

## <a name="configure-sizes-for-cloud-services"></a>Koon määrittäminen pilvipalveluihin

Voit määrittää roolin esiintymän virtuaalikoneen koon seuraavan service- [palvelun määritystiedoston](cloud-services-model-and-package.md#csdef)mallin osana. Koko rooli määrää suorittimen sydämiä, muistin koko ja paikallisen tiedoston järjestelmän koko, joka on kohdistettu suoritettavan määrän. Valitse rooli koko sovelluksen resurssin tarpeen mukaan.

Tässä on esimerkki varten roolin koon [Standard_D2](#general-purpose-d) on tarkoitettu Web-rooli-esiintymän määrittäminen:

```xml
<WorkerRole name="Worker1" vmsize="<mark>Standard_D2</mark>">
...
</WorkerRole>
```

## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja [azure tilaus ja palvelun rajoitukset, kiintiöiden ja rajoitukset](../azure-subscription-service-limits.md).
- Lue lisää [H-sarjan ja Laske paljon A sarjan VMs tietoja](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md) , kuten tehokas tietojenkäsittely (HPC) toiminnoista.


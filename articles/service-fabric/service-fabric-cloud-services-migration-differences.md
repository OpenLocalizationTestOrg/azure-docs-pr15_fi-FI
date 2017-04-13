<properties
   pageTitle="Pilvipalveluihin ja palvelun kangasta erot | Microsoft Azure"
   description="Käsitteellinen yleiskatsaus siirtämistä varten sovellusten Cloud Services-palvelun kangasta."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/19/2016"
   ms.author="vturecek"/>

# <a name="learn-about-the-differences-between-cloud-services-and-service-fabric-before-migrating-applications"></a>Mitä eroa pilvipalveluihin ja palvelun kangasta tietoja ennen siirtoa sovellukset.
Microsoft Azure palvelun kangasta on erittäin skaalattava, erittäin luotettavia jaetut sovellukset seuraavan sukupolven cloud-sovelluksen ympäristö. Se sisältää monia uusia ominaisuuksia pakkaus, käyttöönotosta, päivittäminen ja hajautettu cloud sovellusten hallinta. 

Tämä on johdanto opas siirtyminen sovellusten Cloud Services-palvelun kangasta. Se ohjeessa ensisijaisesti arkkitehtuuri ja suunnittelun ja rakenteen erot Pilvipalveluiden ja palvelun kangasta.
 
## <a name="applications-and-infrastructure"></a>Sovellusten ja infrastruktuurin

Cloud Services ja palvelun kangasta ratkaisevan ero on VMs, toiminnoista ja sovellusten välisen suhteen. Työmäärä tähän on määritetty tietyn tehtävän suorittamiseen tai tarjota kirjoittamasi koodin.
 
 - **Cloud Services käsittelee kuin VMs sovellusten käyttöönotto.** Voit kirjoittaa koodin tiiviisti yhdistää AM esiintymään, kuten Web tai työntekijän rooli. Ottaa käyttöön työmäärää Cloud Services-palveluissa on ottamaan AM-esiintymät, jotka suoritetaan työmäärää. Ei ole sovellukset ja VMs erottaminen ja sitä ei ole sovelluksen määritelmä. Sovelluksen voidaan ajatella, verkossa tai työntekijän rooli esiintymien sisällä pilvipalveluihin käyttöönoton joukon tai koko Cloud Services-käyttöönotto. Tässä esimerkissä sovellus näkyy roolin esiintymiä.
 
![Cloud Services-sovellusten ja topologian][1]

 - **Palvelun kangasta on aiemmin VMs tai, joissa käytetään palvelun kangasta Windows-tai Linux-sovellusten käyttöönotto.** Voit kirjoittaa palvelut ovat täysin erilliset pohjana oleva infrastruktuuri, jossa on otetaan tieltä palvelun kangasta sovelluksen-ympäristössä, jotta sovellus voidaan ottaa käyttöön useita ympäristöt kohteesta. Työmäärä-palvelun kangasta kutsutaan "palvelu" ja palveluista vähintään on ryhmitelty varsinaisesti määritetty sovellus, joka suoritetaan palvelun kangasta sovelluksen-ympäristössä. Useita sovelluksia voidaan ottaa käyttöön yhden palvelun kangasta-klusterin.
 
![Palvelun kangasta sovellukset ja topologian][2]
 
Palvelun kangasta itse on sovelluksen ympäristö kerroksen, joka suoritetaan, kun Windows-tai Linux-pilvipalveluihin on järjestelmän käyttöönoton Azure hallitun VMs toiminnoista, jotka on liitetty kanssa.
Palvelun kangasta-sovelluksen malli on monia etuja:

 - Nopea käyttöönoton kertaa. AM esiintymät luominen voi kestää kauan. Palvelun kangasta VMs vain on otettu käyttöön kun muodostamiseksi klusterin, joka isännöi palvelun kangasta sovelluksen-ympäristössä. Pisteestä sovelluksen pakettien voidaan ottaa käyttöön klusterin nopeasti.
 - HD isännöidä. Cloud Services-Työntekijä-roolin AM isännöi yhden työmäärää. Palvelun kangasta sovellukset ovat erillään VMs, joka näyttää niitä, eli Valittavanasi on runsaasti VMs, johon voit pienentää kokonaiskustannukset suurempi käyttöönotoissa pieni useita sovelluksia.
 - Service-kangasta ympäristö voidaan suorittaa missä tahansa, joka on Windows Server- tai Linux koneet, olipa Azure tai paikallisen. Ympäristön on otetaan kerroksen pohjana oleva infrastruktuuri päällä, jotta sovelluksesi toimii eri ympäristöissä. 
 - Jaettu sovellusten hallinta. Palvelun kangasta on paitsi isännät distributed sovellukset, mutta myös auttaa hallitsemaan elinkaarensa ulkopuolisista isännöintipalvelu AM tai tietokoneen elinkaari.

## <a name="application-architecture"></a>Sovelluksen arkkitehtuuri

Cloud Services-sovelluksen arkkitehtuuri sisältää yleensä monia ulkoisen palvelun riippuvuudet, kuten palvelun Bus, Azure-taulukosta ja Blob-objektien tallennustilaan SQL, Redis.txt tai muiden tilan ja tietojen sovelluksen ja Internetin kautta tai työntekijä roolit välisessä pilvipalveluihin käyttöönoton hallinta. Esimerkki täydellisen Cloud Services-sovellus voi näyttää tältä:  

![Cloud Services-arkkitehtuuri][9]

Palvelun kangasta sovellusten valita myös käyttää samaa ulkoisia palveluja valmis-sovelluksessa. Tämän esimerkin pilvipalveluihin arkkitehtuuri helpoin siirtymispolku Cloud Services-palvelun kangasta, on vain pilvipalveluihin käyttöönoton korvaaminen palvelun kangasta sovelluksen pitäminen arkkitehtuurista sama. Web- ja työntekijä roolit voit siirtämisen palvelun kangasta tilattomien palvelun mahdollisimman vähän koodin muuttuu.

![Palvelun kangasta arkkitehtuuri yksinkertainen siirtämisen jälkeen][10]

Tässä vaiheessa järjestelmä on edelleen sama kuin ennen. Palvelun kangasta tilallisten ominaisuuksista, ulkoiset tilan stores ottaen etuna on internalized kuin tilallisen services tarvittaessa. Tämä on kuin yksinkertainen siirtoon palvelun kangasta tilattomien services Web ja työntekijä roolit enemmän aikaa, kun se täytyy kirjoittaa mukautetun palveluja, jotka tarjoavat vastaavia toimintoja sovelluksen, ennen kuin ulkoiset palvelut. Tällöin etuja ovat: 

 - Poistaminen riippuvuussuhteet 
 - Unifying käyttöönoton, hallinnasta ja Päivitä mallit. 
 
Esimerkki tuloksena arkkitehtuuri, internalizing palveluista näyttää tältä:

![Palvelun kangasta arkkitehtuuri täyden siirron jälkeen][11]

## <a name="communication-and-workflow"></a>Viestintä- ja työnkulku

Useimmat pilvipalvelussa sovellukset sisältää useamman kuin yhden tason. Vastaavasti palvelun kangasta sovelluksen koostuu useamman kuin yhden palvelun (monet palvelut). Kaksi yleisiä viestintä-malleja on suora viestintä ja ulkoiset kestävät tallennustilan kautta.

### <a name="direct-communication"></a>Suora viestintä

Suora viestintä ja tasojen viestiä suoraan kunkin tason näyttämiä päätepiste. Tilattomien ympäristöissä, kuten pilvipalveluihin, tämä tarkoittaa valitsemalla AM rooli-esiintymä joko satunnaisesti tai pyöreän pöydän saldo kuormituksen ja liittää sen päätepisteen suoraan.

![Pilvipalveluihin suora viestintä][5]

 Suora viestintä on yhteinen viestintä malli-palvelun kangasta. Palvelun kangasta ja pilvipalveluihin avaimen ero on kyseisen tekstimuodossa pilvipalveluihin AM, yhdistäminen olisi palvelun kangasta voit muodostaa yhteyden palvelu. Tämä on tärkeää ero on muutamia syitä:

 - Services-palvelun kangasta niitä ole sidottu VMs, jotka isännöivät palvelujen saattaa siirtyminen klusterin ja itse asiassa odotetaan siirtyä eri syistä: resurssien tasaamisen, automaattisesti, sovellusten ja infrastruktuurin päivitykset ja sijainti tai lataamiseen rajoitukset. Tämä tarkoittaa palvelun esiintymän osoite voi muuttaa milloin tahansa. 
 - AM-palvelun kangasta ylläpitää palveluihin, joissa yksilöllinen päätepisteet.

Palvelun kangasta sisältää palvelun etsiminen järjestelmä, kutsutaan nimeäminen-palvelun, jonka avulla voidaan ratkaista palvelujen päätepisteen osoitteita. 

![Palvelun kangasta suora viestintä][6]

### <a name="queues"></a>Olevien

Yleisiä viestintä-järjestelmä tasojen tilattomien ympäristöissä, kuten pilvipalveluihin välissä on ulkoisille jonon avulla voit tallentaa kuuluminen työ-toimintoja, kuten yhden tason toiseen. Käytetty vaihtoehto on web-taso, joka lähettää työt Azure jonossa tai palvelun Bus, jossa työntekijä roolin esiintymät jonosta poistamisen ja käsitellä työt.

![Cloud Services jonon viestintä][7]

Viestintä samaan tietomalliin voidaan palvelun kangasta. Tämä on hyvä aiemmin luodun pilvipalveluihin sovelluksen palvelun kangasta siirrettäessä. 

![Palvelun kangasta suora viestintä][8]
 
## <a name="next-steps"></a>Seuraavat vaiheet

Helpoin siirtymispolku Cloud Services-palvelun kangasta, on vain pilvipalveluihin käyttöönoton korvaaminen palvelun kangasta sovelluksen pitäminen sovelluksesi arkkitehtuurista suurin piirtein sama. Seuraavassa artikkelissa oppaan avulla verkossa tai työntekijän rooli muuntaminen palvelun kangasta tilattomien palvelu.

 - [Yksinkertainen siirto: sivuston tai työntekijän rooli muuntaminen palvelun kangasta tilattomien palvelu](./service-fabric-cloud-services-migration-worker-role-stateless-service.md)

<!--Image references-->
[1]: ./media/service-fabric-cloud-services-migration-differences/topology-cloud-services.png
[2]: ./media/service-fabric-cloud-services-migration-differences/topology-service-fabric.png
[5]: ./media/service-fabric-cloud-services-migration-differences/cloud-service-communication-direct.png
[6]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-communication-direct.png
[7]: ./media/service-fabric-cloud-services-migration-differences/cloud-service-communication-queues.png
[8]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-communication-queues.png
[9]: ./media/service-fabric-cloud-services-migration-differences/cloud-services-architecture.png
[10]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-architecture-simple.png
[11]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-architecture-full.png

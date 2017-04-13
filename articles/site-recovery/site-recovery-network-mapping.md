<properties
    pageTitle="Valmistele verkon määritys Hyper-V virtuaalikoneen suojautumista VMM Azure palauttaminen | Microsoft Azure"
    description="Määritä Hyper-V virtuaalikoneen replikoinnin suorittamisen paikallisen palvelinkeskuksen Azure tai toissijaisen sivuston verkko-yhdistäminen."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="10/05/2016"
    ms.author="raynew"/>


# <a name="prepare-network-mapping-for-hyper-v-virtual-machine-protection-with-vmm-in-azure-site-recovery"></a>Valmistele verkon määritys Hyper-V virtuaalikoneen suojautumista VMM Azure sivuston palauttaminen

Azure palauttaminen osaltaan mukaan orchestrating replikoinnin automaattisesti ja näennäiskoneiden ja fyysiset palvelimet yrityksen liiketoiminnan jatkuvuuden ja tietojen palauttaminen (BCDR) määrittäminen.

Tässä artikkelissa verkon yhdistämistä, joka auttaa määrität verkkoasetukset optimaalisesti, kun käytät palauttaminen replikointia Hyper-V näennäiskoneiden sijaitsevat VMM paveikslėlis kaksi paikallisen palvelinkeskusten tai paikallisen palvelinkeskuksen ja Azure välillä. Huomaa, että, jos olet replikoiminen Hyper-V VMs ilman VMM pilvestä tai replikoiminen VMware VMs tai fyysinen palvelimissa, tässä artikkelissa ei asiaa.

Kirjaa kommentteja tai kysymyksiä alaosassa on tämän artikkelin tai [Azure palautus Services keskustelupalstalle](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="overview"></a>Yleiskatsaus

Verkon määritys käytetään, kun Azure palauttaminen otetaan käyttöön replikoida Hyper-V näennäiskoneiden Azure tai toissijainen palvelinkeskukseen Hyper-V replikan tai SAN replikoinnin avulla.

- **Replikoiminen Hyper-V näennäiskoneiden-VMM paveikslėlis kahden paikallisen palvelinkeskusten**– verkon yhdistäminen kartat AM verkostojen source VMM palvelimessa ja AM verkkojen kohde VMM palvelimessa, toimi seuraavasti:

    - **Yhdistä näennäiskoneiden jälkeen automaattisesti**– varmistaa, että näennäiskoneiden yhdistetään tarvittavat verkkoihin jälkeen automaattisesti. Replikan virtuaalikoneen yhdistetään kohde verkossa, joka on määritetty lähde-verkkoon.
    - **Paikka replikan näennäiskoneiden host-palvelimissa**, sijoita optimaalisesti replikan näennäiskoneiden Hyper-V host-palvelimissa. Replikan näennäiskoneiden sijoitettu isännät, jotka voivat käyttää yhdistetyn AM verkkojen.
    - **Verkon muuntomallia**— Jos verkon määritys ei määritetä, AM verkkoihin ei yhdistettävä replikoitua näennäiskoneiden jälkeen automaattisesti.

- **Replikoiminen Hyper-V näennäiskoneiden paikallisen VMM-cloud, Azure**– verkon määritys kartat AM-verkkojen VMM lähdepalvelimeen välillä ja kohdistaa Azure verkkojen seuraavasti:
    - **Yhdistä näennäiskoneiden jälkeen automaattisesti**, mitkä automaattisesti samassa verkossa voit muodostaa yhteyden toisistaan riippumatta palautus palvelupaketin ne sijaitsevat kaikissa tietokoneissa.
    - **Yhdyskäytävien**— Jos yhdyskäytävien on määritetty aikataulussa Azure verkkoon, näennäiskoneiden ottaa yhteyden muiden paikallisen näennäiskoneiden.
    - **Verkon muuntomallia**— Jos verkon määritys ei määritetä, vain näennäiskoneiden kyseisen automaattisesti samaan palautus-suunnitelmassa saa oikeuden yhdistäminen toisiinsa jälkeen virheiden päälle Azure.


## <a name="network-mapping-example"></a>Esimerkki verkon määritys

Verkon määritys voi määrittää AM verkkojen kaksi VMM palvelimissa tai yksittäisen VMM-palvelimeen Jos kahden sivuston hallitaan samaan palvelimeen välillä. Kun yhdistäminen on määritetty oikein ja replikointi on käytössä, virtual koneen ensisijainen sijainnissa yhdistetään verkkoon ja sen kohdesijainnin replikaa yhdistetään sen yhdistetystä.

Jos verkot on määritetty oikein VMM, kun valitset kohde AM verkon verkon yhdistämisen aikana, VMM lähde-paveikslėlis, jotka käyttävät lähde AM verkko näkyvät, sekä kohteen paveikslėlis, joita käytetään suojaus käytettävissä kohde-AM-verkoissa.

Tässä on esimerkki esitä se. Voit liitetty New York ja Chicagon kahdessa paikassa.

**Sijainti** | **VMM server** | **AM verkot** | **Yhdistetty**
---|---|---|---
New Yorkin | VMM NewYork| VMNetwork1 NewYork | Yhdistetty VMNetwork1 Chicagon
 |  | VMNetwork2 NewYork | Yhdistämättömät
Chicagon | VMM Chicagon| VMNetwork1 Chicagon | Yhdistetty VMNetwork1 NewYork
 | | VMNetwork2 Chicagon | Yhdistämättömät

Ja tässä esimerkissä:

- Kun virtual laitetta, joka on yhteydessä VMNetwork1 NewYork replikan virtual koneen on luotu, se yhdistetään VMNetwork1 Chicagon.
- Kun replikan virtual koneen luodaan VMNetwork2 NewYork tai VMNetwork2 Chicagon, se ole yhteydessä minkä tahansa verkkoon.

Näin miten VMM paveikslėlis on määritetty Esimerkki organisaation ja looginen verkostot, jotka paveikslėlis kanssa.

### <a name="cloud-protection-settings"></a>Cloud suojausasetuksia

**Suojatun pilveen** | **Cloud suojaaminen** | **Looginen verkko (New Yorkissa)**  
---|---|---
GoldCloud1 | GoldCloud2 |
SilverCloud1| SilverCloud2 |
GoldCloud2 | <p>PUUTTUU</p><p></p> | <p>LogicalNetwork1 NewYork</p><p>LogicalNetwork1 Chicagon</p>
SilverCloud2 | <p>PUUTTUU</p><p></p> | <p>LogicalNetwork1 NewYork</p><p>LogicalNetwork1 Chicagon</p>

### <a name="logical-and-vm-network-settings"></a>Loogiset Funktiot ja AM verkkoasetukset

**Sijainti** | **Looginen verkko** | **AM verkossa**
---|---|---
New Yorkin | LogicalNetwork1 NewYork | VMNetwork1 NewYork
Chicagon | LogicalNetwork1 Chicagon | VMNetwork1 Chicagon
 | LogicalNetwork2Chicago | VMNetwork2 Chicagon

### <a name="target-networks"></a>Kohde-verkoissa

Perusteella nämä asetukset, kun valitset kohde AM verkon, seuraavassa taulukossa näkyvät vaihtoehdot, jotka ovat käytettävissä.

**Valitse** | **Suojatun pilveen** | **Cloud suojaaminen** | **Kohteen verkon käytettävissä**
---|---|---|---
VMNetwork1 Chicagon | SilverCloud1 | SilverCloud2 | Käytettävissä
 | GoldCloud1 | GoldCloud2 | Käytettävissä
VMNetwork2 Chicagon | SilverCloud1 | SilverCloud2 | Ei käytettävissä
 | GoldCloud1 | GoldCloud2 | Käytettävissä



## <a name="multiple-subnets"></a>Useita aliverkosta

Jos jokin näistä aliverkosta on sama nimi kuin aliverkon, jossa lähde virtuaalikoneen on kohde verkossa on useita aliverkosta, valitse replikan virtuaalikoneen yhdistetään kyseisen kohteen aliverkon jälkeen automaattisesti. Jos ei ole kohteen aliverkon vastaavaa nimeä, virtuaalikoneen yhdistetään ensimmäisen aliverkon verkossa.


### <a name="failback"></a>Tuntisesta

Jos haluat nähdä, mitä tapahtuu, jos tuntisesta (käänteisen sallittuja), oletetaan, että VMNetwork1 NewYork on yhdistetty VMNetwork1-Chicagon seuraavilla asetuksilla.


**Virtuaalikoneen** | **Yhdistetty AM verkkoon**
---|---
VM1 | VMNetwork1 verkossa
VM2 (replikan VM1) | VMNetwork1 Chicagon

Nämä asetukset Tarkista katsotaan, mitä tapahtuu, jos mahdollista skenaariota usealla.

**Skenaario** | **Tulos**
---|---
Verkon ominaisuuksien AM 2 jälkeen automaattisesti ei muutu. | AM 1 säilyy lähde-verkkoon.
Verkon ominaisuuksien AM 2 on muutettu jälkeen automaattisesti ja on katkennut. | AM 1 on katkennut.
Verkon ominaisuuksien AM 2 muutetaan jälkeen automaattisesti, ja se on yhdistetty VMNetwork2 Chicagon. | Jos VMNetwork2 Chicagon ei ole yhdistetty, AM 1 katkaistaan.
Verkon määritystä VMNetwork1 Chicagon muutetaan. | AM 1 yhdistetään nyt yhdistetty VMNetwork1 Chicagon verkkoon.


## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet luonut paremman käsityksen verkon yhdistäminen [sivustojen palauttamisen käyttöönotto käytön aloittaminen](site-recovery-best-practices.md).

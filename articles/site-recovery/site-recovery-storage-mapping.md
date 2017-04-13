<properties
    pageTitle="Määritä Hyper-V virtuaalikoneen replikoinnin välillä paikallisen palvelinkeskusten Azure palauttaminen tallennustilan | Microsoft Azure"
    description="Valmistele tallennustilan yhdistäminen Hyper-V virtuaalikoneen replikointi kahden paikallisen palvelinkeskusten kanssa Azure palauttaminen välillä."
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
    ms.date="07/06/2016"
    ms.author="raynew"/>


# <a name="prepare-storage-mapping-for-hyper-v-virtual-machine-replication-between-two-on-premises-datacenters-with-azure-site-recovery"></a>Valmistele Hyper-V virtuaalikoneen replikointi välillä kaksi paikallisen palvelinkeskusten Azure palauttaminen ja tallennustilaa yhdistäminen


Azure palauttaminen osaltaan mukaan orchestrating replikoinnin automaattisesti ja näennäiskoneiden ja fyysiset palvelimet yrityksen liiketoiminnan jatkuvuuden ja tietojen palauttaminen (BCDR) määrittäminen. Tässä artikkelissa kuvataan tallennustilan yhdistämistä, jonka avulla voit tehdä näin ollen optimaalisten Käytä tallennustilaa, kun käytät palauttaminen replikointia Hyper-V näennäiskoneiden kaksi paikallisen VMM palvelinkeskusten välillä.

Kirjaa kommentteja tai kysymyksiä alaosassa on tämän artikkelin tai [Azure palautus Services keskustelupalstalle](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="overview"></a>Yleiskatsaus

Tallennustilan yhdistäminen on merkitystä vain, kun olet replikoiminen Hyper-V näennäiskoneiden, jotka sijaitsevat VMM paveikslėlis-ensisijainen palvelinkeskuksen toissijainen palvelinkeskukseen, käyttämällä Hyper-V replikan tai SAN replikoinnin seuraavasti:


- **Paikalliseen paikallisen replikointi Hyper-V replikaan)** – Voit määrittäminen tallennustilan yhdistäminen yhdistäminen tallennustilan luokitukset lähteen ja kohteen VMM palvelinten seuraavasti:

    - **Anna kohde tallennustilan replikan näennäiskoneiden**– näennäiskoneiden replikoida tallennustilan kohde (SMB Jaa tai klusterin jaetun asemat (CSVs)), voit valita.
    - **Replikan virtuaalikoneen sijaintia**, tallennustilan yhdistämistä käytetään sijoittaa optimaalisesti replikan näennäiskoneiden Hyper-V host-palvelimissa. Replikan näennäiskoneiden sijoitettu isännät, jotka voivat käyttää yhdistetyn tallennustilan luokitus.
    - **Tallennustilan muuntomallia**— Jos tallennustilan yhdistämistä ei määritetä, näennäiskoneiden replikoida oletuskansio määritetty replikan virtuaalikoneen liittyvät Hyper-V Host (isäntä)-palvelimeen.

- **Paikalliseen paikallisen replikointi SAN kanssa**– määrittäminen tallennustilan yhdistäminen yhdistäminen tallennustilan matriisin jakavat lähteen ja kohdistaa VMM-palvelimiin.
    - **Määritä ryhmän**– määrittää, mitkä toissijainen tallennusväline-ryhmän vastaanottaa replikoinnin tietoja ensisijainen varannon.
    - **Määritä target tallennustilan jakavat**– varmistaa, että lähde replikoinnin ryhmän LUNs, replikoida yhdistetty kohde tallennustilan resurssivarantoon valittua.

## <a name="set-up-storage-classifications-for-hyper-v-replication"></a>Tallennustilan Hyper-V replikoinnin luokitusten määrittäminen

Kun käytät Hyper-V replikan replikointia palauttaminen kanssa, voit yhdistää välillä tallennustilan luokitukset lähde- ja VMM palvelimissa tai yhteen VMM palvelimeen, jos kahden sivuston hallitaan samaan VMM palvelimeen. Huomaa, että:

- Kun yhdistäminen on määritetty oikein ja replikointi on käytössä, virtual machine virtual kiintolevyn ensisijainen sijainnissa replikoida yhdistetyn kohdesijainnin tallennustilan.
- Tallennustilan luokitukset on oltava käytettävissä lähde- ja paveikslėlis sijaitsevat host-ryhmiin.
- Luokitusten ei tarvitse samantyyppisten lisätallennustilaa. Voit esimerkiksi yhdistää lähde-luokittelu, joka sisältää SMB osakkeet kohde luokitukseen, joka sisältää CSVs.
- Lue lisää [tallennustilaa luokitukset-VMM luomisesta](https://technet.microsoft.com/library/gg610685.aspx).

## <a name="example"></a>Esimerkki

Jos luokitukset määritetään oikein VMM, kun valitset lähde- ja kohdesivustojen VMM palvelimen tallennustilan yhdistämisen aikana, lähde- ja luokitukset näkyvät. Tässä on esimerkki tallennustilan tiedostot osakkeet ja liitetty kahdessa paikassa New York ja Chicagon luokitukset.

**Sijainti** | **VMM server** | **Tiedoston jakaminen (lähde)** | **Luokitus (lähde)** | **Yhdistetty** | **Tiedoston jakaminen (kohde)**
---|---|--- |---|---|---
New Yorkin | VMM_Source| SourceShare1 | KULTA | GOLD_TARGET | TargetShare1
 |  | SourceShare2 | HOPEA | SILVER_TARGET | TargetShare2
 | | SourceShare3 | PRONSSI | BRONZE_TARGET | TargetShare3
Chicagon | VMM_Target |  | GOLD_TARGET | Yhdistämättömät |
| | | SILVER_TARGET | Yhdistämättömät |
 | | | BRONZE_TARGET | Yhdistämättömät

Määrittää nämä **Palvelimen tallennustilassa** -välilehden **resurssit** -sivulla sivuston palautus-portaalissa.

![Määritä tallennustilan yhdistäminen](./media/site-recovery-storage-mapping/storage-mapping1.png)

Ja tässä esimerkissä:
- Kun replikan virtual koneen luodaan virtual koneen KULTA säilössä (SourceShare1), se replikoida GOLD_TARGET storage (TargetShare1).
- Kun replikan virtual koneen on luotu virtual koneen HOPEA säilössä (SourceShare2), se voi replikoida SILVER_TARGET (TargetShare2) tallennustilan ja niin edelleen.

Todellinen tiedostoresurssit ja niiden varattujen luokitukset-VMM näkyvät seuraavassa näytössä-koodin.

![Tallennustilan luokitukset-VMM](./media/site-recovery-storage-mapping/storage-mapping2.png)

## <a name="multiple-storage-locations"></a>Useita tallennuspaikkojen

Jos luokitus kohde on liitetty useita SMB osakkeet tai CSVs, tallenteen tallennussijainti tulevat valituiksi automaattisesti, kun virtuaalikoneen on suojattu. Jos ei ole sopivaa kohde-tallennustila on käytettävissä määritetyn luokitus-määritetyn isännän Hyper-V oletuskansio käytetään replikan näennäiskiintolevyjen paikoilleen.

Seuraavassa taulukossa näytetään, miten tallennustilan luokittelu ja klusterin jaetut asemat on määritetty tässä esimerkissä.

**Sijainti** | **Luokitus** | **Liitetty tallennustila**
---|---|---
New Yorkin | KULTA | <p>C:\ClusterStorage\SourceVolume1</p><p>\\FileServer\SourceShare1</p>
 | HOPEA | <p>C:\ClusterStorage\SourceVolume2</p><p>\\FileServer\SourceShare2</p>
Chicagon | GOLD_TARGET | <p>C:\ClusterStorage\TargetVolume1</p><p>\\FileServer\TargetShare1</p>
 | SILVER_TARGET| <p>C:\ClusterStorage\TargetVolume2</p><p>\\FileServer\TargetShare2</p>

Tässä taulukossa on yhteenveto toiminnan, kun otat suojauksen näennäiskoneiden (VM1 - VM5) tässä esimerkissä-ympäristössä.

**Virtuaalikoneen** | **Lähde-tallennustilan** | **Tietolähteen luokittelu** | **Yhdistetty kohde-tallennustilan**
---|---|---|---
VM1 | C:\ClusterStorage\SourceVolume1 | KULTA | <p>C:\ClusterStorage\SourceVolume1</p><p>\\\FileServer\SourceShare1</p><p>Molemmat GOLD_TARGET</p>
VM2 | \\FileServer\SourceShare1 | KULTA | <p>C:\ClusterStorage\SourceVolume1</p><p>\\FileServer\SourceShare1</p> <p>Molemmat GOLD_TARGET</p>
VM3 | C:\ClusterStorage\SourceVolume2 | HOPEA | <p>C:\ClusterStorage\SourceVolume2</p><p>\FileServer\SourceShare2</p>
VM4 | \FileServer\SourceShare2 | HOPEA |<p>C:\ClusterStorage\SourceVolume2</p><p>\\FileServer\SourceShare2</p><p>Molemmat SILVER_TARGET</p>
VM5 | C:\ClusterStorage\SourceVolume3 | PUUTTUU | Muuntomallia, joten oletuskansio Hyper-V isännän käytetään

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet luonut paremman käsityksen tallennustilan yhdistämistä, [käyttäjien Azure sivustojen palauttamisen käyttöönotto](site-recovery-best-practices.md).

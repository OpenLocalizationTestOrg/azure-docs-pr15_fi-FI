<properties
    pageTitle="Azure palauttaminen: Usein kysytyt kysymykset | Microsoft Azure"
    description="Tässä artikkelissa käsitellään Suositut kysymykset Azure palauttaminen tietoja."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="cfreeman"
    editor=""/>

<tags
    ms.service="get-started-article"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="10/10/2016"
    ms.author="raynew"/>


# <a name="azure-site-recovery-frequently-asked-questions-faq"></a>Azure palauttaminen: Usein kysytyt kysymykset

Tässä artikkelissa on usein kysyttyjä kysymyksiä Azure palauttaminen. Jos sinulla on kysymyksiä luettuasi tämän artikkelin, julkaise ne käyttämiseen [Azure palautus Services keskustelupalstalle](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr).


## <a name="general"></a>Yleiset

### <a name="what-does-site-recovery-do"></a>Mitä sivuston palauttaminen tarkoittaa?

Sivuston palauttaminen prosentuaalista osuutta yrityksen liiketoiminnan jatkuvuuden ja tietojen palauttaminen (BCDR) määrittäminen, orchestrating ja automatisointi replikoinnin paikallisen näennäiskoneiden ja fyysiset palvelimet Azure tai toissijaisen palvelinkeskukseen. [Lue lisää](site-recovery-overview.md).


### <a name="what-can-site-recovery-protect"></a>Mitä sivuston palauttaminen suojata?

- **Hyper-V näennäiskoneiden**: palauttaminen voit suojata kaikki käynnissä olevat Hyper-V AM työmäärää.
- **Fyysinen palvelimiin**: palauttaminen suojata fyysinen Windows- tai Linux-palvelimiin.
- **VMware näennäiskoneiden**: palauttaminen voit suojata kaikki käynnissä VMware AM työmäärää.

### <a name="does-site-recovery-support-the-azure-resource-manager-model"></a>Tukeeko palauttaminen Azure Resurssienhallinta-malli?

Sivuston palauttaminen Azure perinteinen-portaalissa löytyy palauttaminen Azure tukeen resurssien hallinta-portaalissa. Useimmissa käyttöönoton skenaarioissa sivuston palautus Azure-portaalissa on virtaviivaistettu käyttöönoton kokemus ja voit toistaa VMs ja fyysinen palvelinten perinteinen tallennustilan tai Resurssienhallinta-tallennustilan. Seuraavassa on tuettu ominaisuuksissa:

- [Toistaa VMware VMs tai fyysinen palvelimia Azure Azure-portaalissa](site-recovery-vmware-to-azure.md)
- [Toistaa Hyper-V VMs-VMM paveikslėlis Azure Azure-portaalissa](site-recovery-vmm-to-azure.md)
- [Toistaa Hyper-V VMs (ilman VMM) Azure Azure-portaalissa](site-recovery-hyper-v-site-to-azure.md)
- [Toistaa Hyper-V VMs-VMM paveikslėlis toissijaisen sivuston Azure-portaalissa](site-recovery-vmm-to-vmm.md)


### <a name="what-do-i-need-in-hyper-v-to-orchestrate-replication-with-site-recovery"></a>Mitä minun täytyy Hyper-v orchestrate palauttaminen replikointia?

Hyper-V Host (isäntä)-palvelimen tarvittavat riippuu siitä, käyttöönottotapa. Tutustu Hyper-V edellytykset:

- [Replikoiminen Hyper-V VMs (ilman VMM) Azure](site-recovery-hyper-v-site-to-azure.md#before-you-start)
- [Replikoiminen Hyper-V VMs (VMM) Azure](site-recovery-vmm-to-azure.md#before-you-start)
- [Replikoiminen Hyper-V VMs toissijainen palvelinkeskukseen](site-recovery-vmm-to-vmm.md#before-you-start)

- Jos olet replikoiminen toissijainen palvelinkeskukseen, lue lisätietoja [Hyper-V VMs tuetut Vieras-käyttöjärjestelmissä](https://technet.microsoft.com/library/mt126277.aspx).
- Jos olet replikoiminen, Azure-sivuston palauttaminen tukee kaikkia vieraskäyttöjärjestelmät, jotka ovat [Azure tukemat](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx).

### <a name="can-i-protect-vms-when-hyper-v-is-running-on-a-client-operating-system"></a>Voinko suojata VMs suoritettaessa Hyper-V asiakkaan käyttöjärjestelmä?

Ei, VMs on sijaittava Hyper-V host palvelimessa, jossa on käytössä tuetut Windows server-tietokoneessa. Jos haluat suojata asiakastietokone voi replikoida fyysinen tietokoneen [Azure](site-recovery-vmware-to-azure.md) tai [toissijaisen palvelinkeskuksen](site-recovery-vmware-to-vmware.md)nimellä.


### <a name="what-workloads-can-i-protect-with-site-recovery"></a>Mitä työmääriä voit suojata sivuston palauttaminen kanssa?

Voit suojata useimmat toiminnoista on tuettu AM tai fyysinen palvelimen palauttaminen. Sivuston palauttaminen tuki sovelluksen tukeva toistoa siten, että sovellukset voidaan palauttaa älykkäät tilaan. Microsoft-sovellusten, kuten SharePointin, Exchangen, Dynamics, SQL Server ja Active Directoryn integroituu ja työskentelee tiiviisti alussa toimittajia, esimerkiksi Oracle ja SAP, IBM punainen on. [Lue lisää](site-recovery-workload.md) siitä, kuormituksen suojaus.


### <a name="do-hyper-v-hosts-need-to-be-in-vmm-clouds"></a>Tarvitseeko Hyper-V isännät VMM paveikslėlis ovat?

Jos haluat toistaa toissijainen palvelinkeskukseen, ja valitse Hyper-V VMs on oltava Hyper-V isännöi palvelinten VMM pilvestä sijaitsee. Jos haluat toistaa Azure, voit toistaa VMs Hyper-V host-palvelimissa kanssa tai ilman VMM paveikslėlis. [Lue lisää](site-recovery-hyper-v-site-to-azure.md).

### <a name="can-i-deploy-site-recovery-with-vmm-if-i-only-have-one-vmm-server"></a>Voin ottaa palauttaminen VMM, jos käytössä on vain yksi VMM palvelin?

Kyllä. Voit toistaa VMs joko Hyper-V palvelimissa VMM pilveen Azure, tai voit toistaa VMM paveikslėlis samassa palvelimessa välillä. Paikallisen paikallisen replikointi Suosittelemme, että VMM palvelin on ensisijainen ja toissijainen-sivustoissa.  [Lisätietoja on artikkelissa](site-recovery-single-vmm.md)

### <a name="what-physical-servers-can-i-protect"></a>Mitä fyysiset palvelimet voit suojata?

Voit toistaa fyysinen palvelimessa on Windows- ja Linux, Azure tai toissijaisen sivustoon. [Lue lisää](site-recovery-vmware-to-azure.md#protected-machine-prerequisites) käyttöjärjestelmävaatimukset.  Onko fyysiset palvelimet olet replikoiminen Azure tai toissijaisen sivustoon sovelletaan saman vaatimuksia.

Huomaa, että fyysiset palvelimet suoritetaan VMs Azure Jos paikallisen palvelin lakkaa. Tuntisesta paikallisen fyysinen palvelimeen ei ole tällä hetkellä tueta, mutta voi epäonnistua takaisin Hyper-V tai VMware virtual koneen.


### <a name="what-vmware-vms-can-i-protect"></a>Mitä VMware VMs voit suojata?

Suojaa VMware VMs tarvitset vSphere hypervisor ja näennäiskoneiden VMware työkalut. Suosittelemme, että sinulla on VMware vCenter palvelimen hallinta hypervisors. [Lue lisää](site-recovery-vmware-to-azure.md#protected-machine-prerequisites) siitä, tarkka vaatimukset replikoiminen VMware palvelimia ja VMs Azure tai toissijaisen sivustoon.

### <a name="can-i-manage-disaster-recovery-for-my-branch-offices-with-site-recovery"></a>Tietojen palauttaminen hallinta, oma toimipisteiden ja palauttaminen

Kyllä. Kun käytät palauttaminen orchestrate replikointi ja automaattisesti oman toimipisteiden, saat yhdistetty tiedonsiirron ja kaikki oman haaran office työmäärät näkymän keskitetyssä paikassa. Voit suorittaa failovers ja hallita haarat pään toimistossa palauttaminen ilman ohjesisältöä haaran.

## <a name="security"></a>Tietoturva

### <a name="is-replication-data-sent-to-the-site-recovery-service"></a>Lähetetään replikoinnin tietojen palauttaminen-palveluun?

Ei, sivuston palauttaminen ei leikkauspiste replikoitua tiedot mutta ei ole tietoja mitä toimii näennäiskoneiden tai fyysiset palvelimet.
Replikoinnin tietoja vaihtavat paikallisen Hyper-V isännät, VMware hypervisors tai fyysiset palvelimet ja Azure tallennustilan tai toissijaisen sivustoon. Sivuston palauttaminen ei ole mahdollisuus leikkauspiste tiedot. Sivuston palautus-palveluun lähetetään vain orchestrate replikointi ja automaattisesti tarvittavat metatiedot.

Sivuston palauttaminen on ISO 27001:2013 27018, HIPAA, DPA sertifioitu ja SOC2 ja FedRAMP JAB arvioinnit parhaillaan.


### <a name="for-compliance-reasons-even-our-on-premises-metadata-must-remain-within-the-same-geographic-region-can-site-recovery-help-us"></a>Yhteensopivuuden vuoksi edes Microsoftin paikallisen metatietojen on oltava sama maantieteellisen alueen sisällä. Voiko sivuston palauttaminen Auta meitä?

Kyllä. Kun luot sivuston palauttaminen säilö alueen, varmistamme, että kaikki metatiedot, joilla on annettava ottaminen käyttöön ja orchestrate replikoinnin ja automaattisesti pysyy alueella olevat nollat maantieteelliset reunaa.

### <a name="does-site-recovery-encrypt-replication"></a>Sivuston palauttaminen salata replikointia?

Näennäiskoneiden ja fyysinen palvelinten välillä paikallisen sivustojen salaus--siirrossa replikoiminen tuetaan. Näennäiskoneiden ja fyysiset palvelimet replikoiminen Azure-salaus--siirrossa ja salaus-palvelussa-loput (Azure) tuetaan.


## <a name="replication"></a>Replikoinnin


### <a name="are-there-any-prerequisites-for-replicating-virtual-machines-to-azure"></a>Onko edellytyksistä replikoiminen näennäiskoneiden Azure?

Haluat toistaa Azuren näennäiskoneiden on [Azure vaatimusten](site-recovery-best-practices.md#virtual-machines)mukainen.

### <a name="can-i-replicate-hyper-v-generation-2-virtual-machines-to-azure"></a>Hyper-V luonti 2 näennäiskoneiden replikoida Azure avulla

Kyllä. Sivuston palauttaminen muuntaa luonti 2 luonti 1 aikana automaattisesti. Milloin tuntisesta koneen muunnetaan takaisin luonti 2. [Lue lisää](http://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/).

### <a name="if-i-replicate-to-azure-how-do-i-pay-for-azure-vms"></a>Jos voin toistaa Azure kuinka voin maksaa Azure VMs?

Tavallinen replikoinnin aikana tietojen replikoinnin geo tarpeettomat Azure tallennustilan ja sinun ei tarvitse maksaa kaikki Azure IaaS virtuaalikoneen kulujen tarjoaa merkittäviä advantage. Kun suoritat automaattisesti Azure, sivuston palauttaminen luo automaattisesti Azure IaaS näennäiskoneiden ja tämän jälkeen voit laskuteta Laske resurssien, jotka käyttävät Azure-tietokannassa.


### <a name="is-there-an-sdk-i-can-use-to-automate-the-asr-workflow"></a>Onko minulla avulla voit automatisoida ASR työnkulun SDK-paketissa?

Kyllä. Voit automatisoida sivuston palautus-työnkulut käyttämällä Rest-Ohjelmointirajapinta, PowerShell tai Azure SDK-paketissa. Käyttöönoton palauttaminen PowerShellin avulla tällä hetkellä tueta skenaariot:

- [Toistaa Hyper-V VMs VMMs paveikslėlis-perinteinen PowerShellin Azure](site-recovery-deploy-with-powershell.md)
- [Toistaa Hyper-V VMs-VMMs paveikslėlis Azure PowerShell resurssien hallinta](site-recovery-vmm-to-azure-powershell-resource-manager.md)
- [Replikoida Hyper-V VMs ilman VMM PowerShellin Azure perinteinen](site-recovery-hyper-v-site-to-azure-classic.md)
- [Toistaa Hyper-V VMs ilman VMM Azure PowerShell Resurssienhallinta](site-recovery-deploy-with-powershell-resource-manager.md)


### <a name="if-i-replicate-to-azure-what-kind-of-storage-account-do-i-need"></a>Jos voin replikoida Azure millaisia tallennustilan tiliä tarvitaan?

- **Azure perinteinen portaalissa**: Jos olet käyttöönotto palauttaminen Azure perinteinen-portaalissa, sinun on [Vakio geo tarpeettomat tallennustilan tilin](../storage/storage-redundancy.md#geo-redundant-storage). Premium tallennustila ei ole tällä hetkellä tueta. Tilin on oltava sama kuin sivuston palauttaminen säilö alue.

- **Azure portaalissa**: Jos olet käyttöönotto palauttaminen Azure-portaalissa, sinun on tallennustilan LRS tai GRS-tilin. Suosittelemme GRS niin, että tiedot ovat joustavat alueellisen käyttökatkosta sattuessa tai jos ensisijainen alue ei voi palauttaa. Tilin on oltava sama alue kuin palautus Services säilö. Premium tallennustilan tuetaan vain, jos olet replikoiminen VMware VMs tai fyysinen palvelimia.

### <a name="how-often-can-i-replicate-data"></a>Kuinka usein tiedot voit toistaa?

- **Hyper-v** Hyper-V VMs voi replikoida 30 sekunnin välein, 5 minuuttia tai 15 minuuttia. Jos olet määrittänyt SAN replikoinnin replikointi on synkronoitu.
- **VMware ja fyysinen palvelimissa:** Replikoinnin korkojakso ei ole liittyvien tähän. Replikointi on jatkuva.

### <a name="can-i-extend-replication-from-existing-recovery-site-to-another-tertiary-site"></a>Voit laajentaa replikoinnin aiemmin palautus-sivustosta toiseen ensisijaisen sivustoon?

Laajennettu tai ketjutetut replikointia ei tueta. Pyydä tämän ominaisuuden [palautetta](http://feedback.azure.com/forums/256299-site-recovery/suggestions/6097959-support-for-exisiting-extended-replication)keskustelupalstalle.


### <a name="can-i-do-an-offline-replication-the-first-time-i-replicate-to-azure"></a>Voin offline-tilassa replikoinnin voin replikoida Azure ensimmäistä kertaa?

Tämä ei tueta. Pyydä [palautetta keskustelupalstalla](http://feedback.azure.com/forums/256299-site-recovery/suggestions/6227386-support-for-offline-replication-data-transfer-from)tätä ominaisuutta.


### <a name="can-i-exclude-specific-disks-from-replication"></a>Tietyn levyjen pois replikoinnin kohteesta

Tuetaan, kun olet [replikoiminen VMware VMs ja fyysinen palvelinten](site-recovery-vmware-to-azure.md#exclude-disks-from-replication) Azure-Azure-portaalissa.


### <a name="can-i-replicate-virtual-machines-with-dynamic-disks"></a>Voit toistaa näennäiskoneiden dynaamisiksi kanssa?

Dynaamisiksi tuetaan, kun replikoiminen Hyper-V näennäiskoneiden. Ne tuetaan myös, kun replikoiminen VMware VMs ja fyysisten laitteiden Azure. Käyttöjärjestelmä on oltava tavallinen DVD-levyllä.

### <a name="can-i-add-a-new-machine-to-an-existing-replication-group"></a>Uuden tietokoneen lisääminen aiemmin luotuun replikoinnin-ryhmään

Uudet tietokoneet lisääminen aiemmin replikoinnin ryhmiin tuetaan. Voit tehdä replikoinnin ryhmän ('Replikoitu kohteiden' sivu) ja replikoinnin ryhmä, napsauta hiiren kakkospainikkeella ja valitse pikavalikko ja valitse haluamasi vaihtoehto.

![Replikoinnin ryhmään](./media/site-recovery-faq/add-server-replication-group.png)

### <a name="can-i-throttle-bandwidth-allotted-for-hyper-v-replication-traffic"></a>Varattu Hyper-V replikoinnin tietoliikenteen kaistanleveyden rajoita

Kyllä. Voit lukea lisää tietoja käyttöönoton artikkeleissa Kaistanleveyden rajoittaminen:

- [Kapasiteetin replikoiminen VMware VMs ja fyysinen palvelinten suunnitteleminen](site-recovery-vmware-to-azure.md#step-5-capacity-planning)
- [Kapasiteetista, suunnittelusta replikoiminen Hyper-V VMs-VMM paveikslėlis](site-recovery-vmm-to-azure.md#step-5-capacity-planning)
- [Kapasiteetin replikoiminen Hyper-V VMs ilman VMM suunnitteleminen](site-recovery-hyper-v-site-to-azure.md#step-5-capacity-planning)

## <a name="failover"></a>Automaattisesti


### <a name="if-im-failing-over-to-azure-how-do-i-access-the-azure-virtual-machines-after-failover"></a>Jos I 'M kirjautumatta päälle Azure, miten voin käyttää Azure-virtuaalikoneissa jälkeen automaattisesti?

Voit käyttää Azure VMs suojatun Internet-yhteyden kautta, sivuston sivuston VPN tai Azure ExpressRoute päälle. Sinun on useita kohtia, jotta voidaan yhdistää. Lisätietoja on kohdassa:

- [Yhteyden muodostaminen Azure VMs automaattisesti VMware VMs tai fyysiset palvelimet jälkeen](hsite-recovery-vmware-to-azure.md#step-7-test-the-deployment)
- [Yhteyden muodostaminen Azure VMs jälkeen Hyper-V VMs-VMM paveikslėlis automaattisesti](site-recovery-vmm-to-azure.md#step-7-test-your-deployment)
- [Yhteyden muodostaminen Azure VMs jälkeen Hyper-V VMs ilman VMM automaattisesti](site-recovery-hyper-v-site-to-azure.md#step-7-test-the-deployment)


### <a name="if-i-fail-over-to-azure-how-does-azure-make-sure-my-data-is-resilient"></a>Jos voin epäonnistuvat päälle, miten Azure varmistaa, että Azure tiedoissa on joustavat?

Azure on suunniteltu toimintakykyyn. Sivuston palauttaminen jo muutetaan toissijainen Azure palvelinkeskuksen Azure SLA mukaisesti automaattisesti tarvittaessa. Tässä tapauksessa emme Varmista metatietojen ja vaults jäävät saman maantieteellisen alueen, jolla haluat lisääminen säilöön.  

### <a name="if-im-replicating-between-two-datacenters-what-happens-if-my-primary-datacenter-experiences-an-unexpected-outage"></a>Jos I 'M replikoiminen välillä kaksi palvelinkeskusten entä jos Omat ensisijainen palvelinkeskuksen ilmenee odottamattomia käyttökatkosta?

Voit käynnistää suunnittelematon automaattisesti, toissijainen-sivustosta. Sivuston palauttaminen ei tarvitse connectivity suorittamiseen vikasietotilaa ensisijainen-sivustosta.

### <a name="is-failover-automatic"></a>Automaattisuus automaattisesti?

Automaattisen ei tue. Voit aloittaa failovers kanssa yhdellä napsautuksella portaalissa tai [Sivuston palauttaminen PowerShellin](https://msdn.microsoft.com/library/dn850420.aspx) avulla voit käynnistää automaattisesti. Kaatuvat takaisin on yksinkertainen toiminto sivuston palautus-portaalissa.

Voit automatisoida voi tunnistaa virtuaalikoneen epäonnistui paikallisen Orchestrator tai Operations Manager avulla ja käynnistää käyttämällä SDK vikasietotilaa.

- [Lue lisää](site-recovery-create-recovery-plans.md) palautus-palvelupakettia.
- [Lue lisää](site-recovery-failover.md) automaattisesti.
- [Lue lisää](site-recovery-failback-azure-to-vmware.md) epäonnistumista takaisin VMware VMs ja fyysinen palvelimissa


## <a name="service-providers"></a>Palveluntarjoajien


### <a name="im-a-service-provider-does-site-recovery-work-for-dedicated-and-shared-infrastructure-models"></a>Olen palveluntarjoajan. Toimiiko palauttaminen erillinen ja jaetun infrastruktuuri mallien?

Kyllä, sivuston palauttaminen tukee sekä erillinen ja jaetun infrastruktuuri-mallit.

### <a name="for-a-service-provider-is-the-identity-of-my-tenant-shared-with-the-site-recovery-service"></a>Palveluntarjoaja on jaettu palauttaminen-palvelun alihallintaan tunnus?

Ei. Vuokraajan tunnistetietojen pysyy anonyymi. Oman alihallinnat ei tarvitse käyttää sivuston palautus-portaaliin. Palveluntarjoajan järjestelmänvalvoja toimii portaalissa.


### <a name="will-tenant-application-data-ever-go-to-azure"></a>Vuokraajan sovelluksen tietoja koskaan siirtyvät Azure?

Kun replikoiminen palvelun tarjoajan omistaa sivustojen välillä, tiedot yhdistetään koskaan Azure. Tiedot salataan-siirrossa ja replikoitua suoraan palvelu tarjoaja sivustot välillä.

Jos olet replikoiminen, Azure-sovelluksen tiedot lähetetään Azure-tallennustilan, mutta et palauttaminen-palvelun. Tiedot on salattu-siirrossa ja jää salattuja Azure-tietokannassa.


### <a name="will-my-tenants-receive-a-bill-for-any-azure-services"></a>Omat alihallinnat, jotka saat laskun Azure-palveluiden?

Ei. Azure's laskutuksen suhde on suoraan palveluntarjoaja. Palveluntarjoajien ovat vastuussa luodaan tietyn laskut niiden alihallinnat.

### <a name="if-im-replicating-to-azure-do-we-need-to-run-virtual-machines-in-azure-at-all-times"></a>Jos haluat Azure 'M replikoiminen, tarvitsee toimimaan Azuren näennäiskoneiden aina?

Ei, tietojen replikoinnin Azure-tallennustilan tilin-tilaukseesi. Kun suoritat testin automaattisesti (DR Poraudu) tai todellinen automaattisesti, sivuston palauttaminen luo automaattisesti näennäiskoneiden tilauksen.

### <a name="do-you-ensure-tenant-level-isolation-when-i-replicate-to-azure"></a>Varmistaa vuokraajan tasolla eristystaso silloin, kun voin toistaa Azure?

Kyllä.

### <a name="what-platforms-do-you-currently-support"></a>Mitä ympäristöjen olet tällä hetkellä tukevat?

Sovellus tukee Azure kielipaketti, Cloud-ympäristössä järjestelmä- ja System Center perusteella (2012 tai uudempi versio) ominaisuuksissa. [Lue lisää](https://technet.microsoft.com/library/dn850370.aspx) siitä, Azure Pack ja palauttaminen integrointi.

### <a name="do-you-support-single-azure-pack-and-single-vmm-server-deployments"></a>Tukea yksittäisen Azure-pakettiin ja yksi VMM-palvelinta?

Kyllä, voit replikoida Hyper-V näennäiskoneiden Azure tai replikoida palveluntarjoajan sivustojen välillä.  Huomaa, että jos replikoida välillä palvelu tarjoaja sivustot, Azure runbookin integrointi ei ole käytettävissä.


## <a name="next-steps"></a>Seuraavat vaiheet

- Lue [palauttaminen yleiskatsaus](site-recovery-overview.md)
- Lisätietoja [sivuston palautus-arkkitehtuuri](site-recovery-components.md)  

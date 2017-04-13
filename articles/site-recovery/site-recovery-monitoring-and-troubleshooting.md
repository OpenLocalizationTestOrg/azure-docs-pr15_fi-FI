<properties
    pageTitle="Valvo ja suojauksen näennäiskoneiden ja fyysiset palvelimet vianmääritys | Microsoft Auzre" 
    description="Azure palauttaminen koordinoi replikoinnin, automaattisesti ja paikallisen palvelimessa Azure tai toissijainen palvelinkeskuksen näennäiskoneiden palauttaminen. Tämän artikkelin avulla voit valvoa ja vianmääritys VMM tai Hyper-V sivuston suojaus." 
    services="site-recovery" 
    documentationCenter="" 
    authors="anbacker" 
    manager="mkjain" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery" 
    ms.date="10/13/2016"    
    ms.author="rajanaki"/>
    
# <a name="monitor-and-troubleshoot-protection-for-virtual-machines-and-physical-servers"></a>Valvo ja suojauksen näennäiskoneiden ja fyysiset palvelimet vianmääritys

Seuranta- ja vianmääritys oppaan avulla voit replikoinnin kunnon seuranta ja vianmääritys tekniikat Azure sivuston palauttamista varten.

## <a name="understanding-the-components"></a>Tietoja osat

### <a name="vmwarephysical-site-deployment-for-replication-between-on-premises-and-azure"></a>VMware/fyysisiä sivuston käyttöönotto replikoinnin paikallisen ja Azure välillä.
Paikallisen VMware/fyysisiä koneen; välillä DR määritys Määrityspalvelimen, perustyyli kohde ja prosessin Server on määritetty. Kun lähdepalvelimeen Azure palauttaminen suojauksen ottaminen käyttöön Asenna Mobility-palvelu. Kirjaa paikalliset käyttökatkosta kun lähdepalvelimeen ei onnistu-over Azure-asiakkaiden on asennuksen Azure prosessin palvelimeen, ja perustyyli kohde palvelimen suojaaminen lähdepalvelimeen takaisin uudelleenkootut paikallisen paikallista. 

![Replikoinnin välillä paikallisen ja Azure VMware/fyysisiä käyttöönotto](media/site-recovery-monitoring-and-troubleshooting/image18.png)

### <a name="vmm-site-deployment-for-replication-between-on-premises-site"></a>VMM sivuston käyttöönotto replikoinnin paikallisen sivuston välillä.

Osana määrittäminen DR välillä kaksi paikallisen sijainnista. Azure sivuston palauttaminen tarjoajan on ladattu ja asennettu VMM-palvelimeen. Palvelu on internet-yhteys, varmista, että kaikki saatu Azure-portaalista toiminnot saa muuntaa paikallisen toimintojen käyttöön suojauksen, kuten Sammuta ensisijainen puoli näennäiskoneiden osana failovers jne.

![VMM sivuston käyttöönotto replikoinnin paikallisen sivuston välillä](media/site-recovery-monitoring-and-troubleshooting/image1.png)

### <a name="vmm-site-deployment-for-replication-between-on-premises--azure"></a>VMM sivuston käyttöönotto replikoinnin paikallisen ja Azure välillä.

Osana DR välillä paikallisen ja Azure; määrittäminen Azure sivuston palauttaminen tarjoajan on ladattu ja asennettu Azure palautus Services tekijää, joka on asennettava kunkin Hyper-V isännän VMM palvelimelle. Lisätietoja on [Tietoja sivuston Azure suojaus](./site-recovery-understanding-site-to-azure-protection.md) .

![Replikoinnin välillä paikallisen ja Azure VMM käyttöönotto](media/site-recovery-monitoring-and-troubleshooting/image2.png)

### <a name="hyper-v-site-deployment-for-replication-between-on-premises--azure"></a>Replikoinnin välillä paikallisen ja Azure Hyper-V käyttöönotto

Tämä on sama kuin, VMM käyttöönoton – vain erona tarjoaja ja edustaja saa asennettu Hyper-V isännän itse. Lisätietoja on [Tietoja sivuston Azure suojaus](./site-recovery-understanding-site-to-azure-protection.md) .

## <a name="monitor-configuration-protection-and-recovery-operations"></a>Valvoa määritykset, suojaus ja palauttaminen

Jokaisen ASR-toiminnon saa valvoa ja seurataan "TYÖT-välilehdessä. Määritysten, suojaus tai virhe Siirry TYÖT-välilehti ja ovatko kaikki virheet.

![Valvoa määritykset, suojaus ja palauttaminen](media/site-recovery-monitoring-and-troubleshooting/image3.png)

Kun löydät virheet TYÖT-näkymä, työ, ja valitse kyseisen työn virheen yksityiskohtaiset tiedot.

![Valvoa määritykset, suojaus ja palauttaminen](media/site-recovery-monitoring-and-troubleshooting/image4.png)

Virheen yksityiskohtaiset tiedot avulla voit tunnistaa mahdollinen syy ja ongelman toiminnon.

![Valvoa määritykset, suojaus ja palauttaminen](media/site-recovery-monitoring-and-troubleshooting/image5.png)

Yllä kirjoitusmuotoon on todennäköisesti toinen toiminto, jossa on käynnissä, joiden vuoksi suojauksen määritys kaatuvat. Varmista, että voit ratkaista ongelman mukaan suositus – sieltä jälkeen Valitse RESART aloittamaan toiminnon uudelleen.

![Valvoa määritykset, suojaus ja palauttaminen](media/site-recovery-monitoring-and-troubleshooting/image6.png)

Mahdollisuus uudelleenkäynnistys ei ole käytettävissä kaikki toiminnot – käyttäjille, joka ei ole uudelleen-vaihtoehto palata objekti- ja tee toiminto uudelleen. Jokaisen työn voi peruuttaa milloin tahansa ajan ja edistymisen Peruuta-painikkeen avulla.

![Valvoa määritykset, suojaus ja palauttaminen](media/site-recovery-monitoring-and-troubleshooting/image7.png)

## <a name="monitor-replication-health-for-virtual-machine"></a>Toistoa varten virtuaalikoneen kunnon valvonta

ASR tarjoaja keskitetyn & remote palvelun Azure-portaalissa kunkin suojatun kohteiden seuranta. Siirry jälkeen on suojattu-kohteet, valitse VMM PAVEIKSLĖLIS tai suojauksen ryhmät. Kaikissa muissa tilanteissa on suojattu kohteiden ryhmät Suojaus-välilehden VMM PAVEIKSLĖLIS-välilehti on vain VMM mukaan käyttöönottoa varten.

![Toistoa varten virtuaalikoneen kunnon valvonta](media/site-recovery-monitoring-and-troubleshooting/image8.png)

Sieltä jälkeen Valitse suojatun kohteen vastaaviin pilveen tai suojaus-ryhmä. Kun olet valinnut suojatun kohteen kaikki sallitut toiminnot näkyvät alaruudussa.

![Toistoa varten virtuaalikoneen kunnon valvonta](media/site-recovery-monitoring-and-troubleshooting/image9.png)

Yllä olevassa muodossa palvelupyyntö-virtuaalikoneen kunto on ehdottoman – voit valita virheen yksityiskohtaiset tiedot-painiketta saat näkyviin virheen pohjassa. Perusteella "mahdollisia syitä" ja "Suositus" mainituista ratkaista ongelman.

![Toistoa varten virtuaalikoneen kunnon valvonta](media/site-recovery-monitoring-and-troubleshooting/image10.png)

![Toistoa varten virtuaalikoneen kunnon valvonta](media/site-recovery-monitoring-and-troubleshooting/image11.png)

Huomautus: Jos aktiivinen toimintoja, jotka ovat käynnissä tai epäonnistui Siirry TYÖT näkymän kuten edellä mainittiin voit tarkastella PROJEKTIN virheen.

## <a name="troubleshoot-on-premises-hyper-v-issues"></a>Paikallisen Hyper-V ongelmien vianmääritys

Yhteyden muodostaminen paikalliseen Hyper-V hallinta-konsolin, valitse virtuaalikoneen ja katso replikoinnin kunto.

![Paikallisen Hyper-V ongelmien vianmääritys](media/site-recovery-monitoring-and-troubleshooting/image12.png)

Tässä tapauksessa *Replikoinnin kunto* on merkitty kriittinen – *Näytä replikoinnin kunto* tietojasi nimellä.

![Paikallisen Hyper-V ongelmien vianmääritys](media/site-recovery-monitoring-and-troubleshooting/image13.png)

Tapausten, jossa replikoinnin on keskeytetty virtuaalikoneen hiiren kakkospainikkeella valitse *replikoinnin*->*ansioluettelon replikoinnin*
![vianmääritys paikallisen Hyper-V ongelmat](media/site-recovery-monitoring-and-troubleshooting/image19.png)

Siltä varalta, että virtuaalikoneen siirtää uusi Hyper-V isäntä (sisällä klusterin tai erillisen machine), joka on määritetty ASR kautta, virtuaalikoneen replikoinnin Eikö heikentyä. Varmista, että uusi Hyper-V isäntä täyttää kaikki kohden-urheiluvälineet eivätkä on määritetty ASR.

### <a name="event-log"></a>Tapahtumaloki

| Tapahtuman lähteistä                | Tiedot                                                                                                                                                                                           |
|-------------------------  |:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------    |
| **Sovellusten ja palvelujen lokit/Microsoft/VirtualMachineManager/palvelin/järjestelmänvalvoja** (VMM Server)   |  Tämä on hyötyä kirjaaminen, jotta useita eri VMM-ongelmien vianmääritys. |
| **Sovellusten ja lokeja/MicrosoftAzureRecoveryServices/replikointi** (Hyper-V isäntä)   | Tämä on hyötyä kirjaaminen, jotta useita Microsoft Azure palautus Services Agent vianmääritys. <br/> ![Tapahtuman tietolähteeseen yhdistetyn Hyper-V Host (isäntä)](media/site-recovery-monitoring-and-troubleshooting/eventviewer03.png) |
| **Sovellusten ja palvelun lokit/Microsoft/Azure sivuston palauttaminen/tarjoajan/toiminnassa** (Hyper-V isäntä)   | Tämä on hyötyä kirjaaminen, jotta useita Microsoft Azure sivuston palautus-palvelun vianmääritys. <br/> ![Tapahtuman tietolähteeseen yhdistetyn Hyper-V Host (isäntä)](media/site-recovery-monitoring-and-troubleshooting/eventviewer02.png) |
| **Sovellusten ja palvelujen lokit/Microsoft/Windows/Hyper-V-VMMS/järjestelmänvalvoja** (Hyper-V isäntä) | Tämä on hyötyä kirjaaminen, jotta useita Hyper-V virtuaalikoneen ongelmien vianmääritys. <br/> ![Tapahtuman tietolähteeseen yhdistetyn Hyper-V Host (isäntä)](media/site-recovery-monitoring-and-troubleshooting/eventviewer01.png) |


### <a name="hyper-v-replication-logging-options"></a>Hyper-V replikoinnin kirjaamisasetusten

Kaikki tapahtumat, jotka liittyvät Hyper-V replikan kirjautunut Hyper-V-VMMS\\järjestelmänvalvojan log sijaitseva **sovellukset- ja palvelulokit\\Microsoft\\Windows**. Lisäksi analyyttisen lokin voi ottaa käyttöön Hyper-V-VMMS. Tämä loki käyttöön ensin julkaista analyyttiset ja virheenkorjaus-lokit-Tapahtumienvalvonta. Avaa Tapahtumienvalvonta, valitse **Näytä-valikko**, valitse **Näytä Analyyttisten ja virheenkorjaus lokitiedot**.

![Paikallisen Hyper-V ongelmien vianmääritys](media/site-recovery-monitoring-and-troubleshooting/image14.png)

Valitse Hyper-V-VMMS näkyy analyyttisen lokin

![Paikallisen Hyper-V ongelmien vianmääritys](media/site-recovery-monitoring-and-troubleshooting/image15.png)

Valitse **Toiminnot** -ruudusta **Ota loki käyttöön**. Kun otettu käyttöön, se näkyy **Suorituskyvyn valvonta** kuin tapahtuma-Trace Session sijaitseva **kerääminen tietojoukot.**

![Paikallisen Hyper-V ongelmien vianmääritys](media/site-recovery-monitoring-and-troubleshooting/image16.png)

Voit tarkastella kerättyjä tietoja ensin jäljitysistunnon lopettaminen poistamalla lokiin, ja tallentaa lokin ja avaa se uudelleen Tapahtumienvalvonta tai käyttää muita työkaluja, muunna haluamallasi tavalla.



## <a name="reaching-out-for-microsoft-support"></a>Microsoft Support tavoittelevat

### <a name="log-collection"></a>Log-kokoelmaan

VMM sivuston suojaus-kohdassa tarvittavat lokien kerääminen [ASR Log sivustokokoelman tuki diagnostiikka ympäristö (SDP)-työkalun avulla](http://social.technet.microsoft.com/wiki/contents/articles/28198.asr-data-collection-and-analysis-using-the-vmm-support-diagnostics-platform-sdp-tool.aspx) .

Hyper-V sivuston suojautumista Lataa [työkalu](https://dcupload.microsoft.com/tools/win7files/DIAG_ASRHyperV_global.DiagCab) ja suorita Hyper-V lokit keräämiseen isännän.

VMware/fyysisiä tilanteessa Katso tarvittavat lokien kerääminen [Azure palautus Log sivustokokoelman VMware ja fyysinen sivuston suojaus](http://social.technet.microsoft.com/wiki/contents/articles/30677.azure-site-recovery-log-collection-for-vmware-and-physical-site-protection.aspx) .

Työkalu kerää paikallisesti satunnaisesti nimetyn alikansion **%LocalAppData%\ElevatedDiagnostics** -kohdassa Valitse lokit

![Esimerkki vaiheet näkyvissä Hyper-V sivuston suojaus.](media/site-recovery-monitoring-and-troubleshooting/animate01.gif)

### <a name="opening-a-support-ticket"></a>Tuen lippu avaaminen

Ottaa tuki lipun ASR saavuttaminen tukemaan Azure <http://aka.ms/getazuresupport> URL-Osoitetta käyttämällä

## <a name="kb-articles"></a>Tietopankin artikkelit

-   [Voit säilyttää suojatun näennäiskoneiden, jotka ovat yli epäonnistui tai siirry Azure kirjain](http://support.microsoft.com/kb/3031135)
-   [Paikalliseen, Azure suojaus verkon kaistanleveyden käytön hallinnasta](https://support.microsoft.com/kb/3056159)
-   [ASR: "klusteriresurssin ei löydy" virhe, kun yrität virtual machine suojauksen ottaminen käyttöön](http://support.microsoft.com/kb/3010979)
-   [Tietoja & vianmääritys Hyper-V replikan opas](http://www.microsoft.com/en-in/download/details.aspx?id=29016) 

## <a name="common-asr-errors-and-their-resolutions"></a>Yleisiä ASR virheitä ja niiden ratkaisuja

Seuraavassa on yleisiä virheitä, jotka voivat valitset ja niiden ratkaisuja. Jokainen virhe on kuvattu eri wikisivun.

### <a name="general"></a>Yleiset
-   <span style="color:green;">Uusi</span> [Työt kaatuvat virhe "toiminto on käynnissä." Virhe 505, 514, 532](http://social.technet.microsoft.com/wiki/contents/articles/32190.azure-site-recovery-jobs-failing-with-error-an-operation-is-in-progress-error-505-514-532.aspx)
-   <span style="color:green;">Uusi</span> [Työt eivät läpäise virhe "Palvelin ei ole yhteydessä Internetiin". Virhe 25018](http://social.technet.microsoft.com/wiki/contents/articles/32192.azure-site-recovery-jobs-failing-with-error-server-isn-t-connected-to-the-internet-error-25018.aspx)

### <a name="setup"></a>Asetukset
-   [VMM palvelin ei voi rekisteröidä sisäisen virheen vuoksi. Tutustu työt näkymän sivuston palauttaminen portaalissa lisätietoja virheestä. Suorita asennus uudelleen, jos haluat rekisteröidä palvelimen.](http://social.technet.microsoft.com/wiki/contents/articles/25570.the-vmm-server-cannot-be-registered-due-to-an-internal-error-please-refer-to-the-jobs-view-in-the-site-recovery-portal-for-more-details-on-the-error-run-setup-again-to-register-the-server.aspx)
-   [Hyper-V palautus hallinta säilö ei voi muodostaa yhteyden. Välityspalvelimen asetusten tarkistaminen tai yritä uudelleen myöhemmin.](http://social.technet.microsoft.com/wiki/contents/articles/25571.a-connection-cant-be-established-to-the-hyper-v-recovery-manager-vault-verify-the-proxy-settings-or-try-again-later.aspx)

### <a name="configuration"></a>Määritys
-   [Ei voi luoda suojaus-ryhmä: Virhe noudettaessa palvelinten luettelon.](http://blogs.technet.com/b/somaning/archive/2015/08/12/unable-to-create-the-protection-group-in-azure-site-recovery-portal.aspx)
-   [Hyper-V host klusteri sisältää vähintään yhden staattinen verkkosovittimen tai ei ole yhdistetty sovittimet on määritetty käyttämään DHCP.](http://social.technet.microsoft.com/wiki/contents/articles/25498.hyper-v-host-cluster-contains-at-least-one-static-network-adapter-or-no-connected-adapters-are-configured-to-use-dhcp.aspx)
-   [Viimeistele toiminnon oikeudet eivät riitä VMM](http://social.technet.microsoft.com/wiki/contents/articles/31110.vmm-does-not-have-permissions-to-complete-an-action.aspx)
-   [Ei voi valita määritettäessä suojaus-tilauksen piiriin kuuluvien tallennustilan-tili](http://social.technet.microsoft.com/wiki/contents/articles/32027.can-t-select-the-storage-account-within-the-subscription-while-configuring-protection.aspx)

### <a name="protection"></a>Suojaus
- <span style="color:green;">Uusi</span> Ota suojaus [epäonnistumista kanssa virhe "suojaus ei ole määritetty virtuaalikoneen". Virhe 60007, 40003](http://social.technet.microsoft.com/wiki/contents/articles/32194.azure-site-recovery-enable-protection-failing-with-error-protection-couldn-t-be-configured-for-the-virtual-machine-error-60007-40003.aspx)
- <span style="color:green;">Uusi</span> Ota suojaus [epäonnistumista kanssa virhe "suojaus ei ole otettu käyttöön virtuaalikoneen." Virhe 70094](http://social.technet.microsoft.com/wiki/contents/articles/32195.azure-site-recovery-enable-protection-failing-with-error-protection-couldn-t-be-enabled-for-the-virtual-machine-error-70094.aspx)
- <span style="color:green;">Uusi</span> [Live siirron virhe 23848 - virtuaalikoneen suorittaminen voi siirtää Live tyyppi. Tämä saattaa katkaista palautus suojauksen tilan virtuaalikoneen.](http://social.technet.microsoft.com/wiki/contents/articles/32021.live-migration-error-23848-the-virtual-machine-is-going-to-be-moved-using-type-live-this-could-break-the-recovery-protection-status-of-the-virtual-machine.aspx) 
- [Ota suojaus epäonnistui, koska agentti ole asennettu tietokoneeseen Host (isäntä)](http://social.technet.microsoft.com/wiki/contents/articles/31105.enable-protection-failed-since-agent-not-installed-on-host-machine.aspx)
- [Sopivan isännän replikan virtuaalikoneen, ei löydy - pienen Laske resurssien vuoksi](http://social.technet.microsoft.com/wiki/contents/articles/25501.a-suitable-host-for-the-replica-virtual-machine-can-t-be-found-due-to-low-compute-resources.aspx)
- [Sopivan isännän replikan virtuaalikoneen, ei löydy - vuoksi ei ole liitetty looginen verkko](http://social.technet.microsoft.com/wiki/contents/articles/25502.a-suitable-host-for-the-replica-virtual-machine-can-t-be-found-due-to-no-logical-network-attached.aspx)
- [Ei voi muodostaa yhteyttä replikan host - koneen yhteyttä ei voitu muodostaa](http://social.technet.microsoft.com/wiki/contents/articles/31106.cannot-connect-to-the-replica-host-machine-connection-could-not-be-established.aspx)


### <a name="recovery"></a>Palautus
- Host (isäntä) - toimintoa ei voi suorittaa VMM
    -   [Ei onnistu virtuaalikoneen varten valitut palautus-kohdan päälle: Yleinen käyttö estetty-virhesanoman.](http://social.technet.microsoft.com/wiki/contents/articles/25504.fail-over-to-the-selected-recovery-point-for-virtual-machine-general-access-denied-error.aspx)
    -   [Hyper-V epäonnistui epäonnistuu päälle virtuaalikoneen valitun palautuspiste: toiminto keskeytyi yritä uudempaan palautus-kohtaa. (0x80004004)](http://social.technet.microsoft.com/wiki/contents/articles/25503.hyper-v-failed-to-fail-over-to-the-selected-recovery-point-for-virtual-machine-operation-aborted-try-a-more-recent-recovery-point-0x80004004.aspx)
    -   Yhteyttä palvelimeen ei onnistunut aiemmin luotuja (0x00002EFD)
        -   [Hyper-V ottaminen käyttöön Käänteiset toistoa varten virtuaalikoneen epäonnistui](http://social.technet.microsoft.com/wiki/contents/articles/25505.a-connection-with-the-server-could-not-be-established-0x00002efd-hyper-v-failed-to-enable-reverse-replication-for-virtual-machine.aspx)
        -   [Hyper-V virtuaalikoneen virtuaalikoneen replikoinnin ottaminen käyttöön epäonnistui](http://social.technet.microsoft.com/wiki/contents/articles/25506.a-connection-with-the-server-could-not-be-established-0x00002efd-hyper-v-failed-to-enable-replication-for-virtual-machine-virtual-machine.aspx)
    -   [Ei voi vahvistaa virtuaalikoneen automaattisesti](http://social.technet.microsoft.com/wiki/contents/articles/25508.could-not-commit-failover-for-virtual-machine.aspx)
-   [Palautus-suunnitelma sisältää näennäiskoneiden, jotka eivät ole valmis suunnitellun automaattisesti](http://social.technet.microsoft.com/wiki/contents/articles/25509.the-recovery-plan-contains-virtual-machines-which-are-not-ready-for-planned-failover.aspx)
-   [Virtuaalikoneen ei ole valmis suunnitellun automaattisesti](http://social.technet.microsoft.com/wiki/contents/articles/25507.the-virtual-machine-isn-t-ready-for-planned-failover.aspx)
-   [Virtuaalikoneen ei ole käytössä ja ei ole kytketty poistaminen käytöstä](http://social.technet.microsoft.com/wiki/contents/articles/25510.virtual-machine-is-not-running-and-is-not-powered-off.aspx)
-   [Poissa-nauha-toiminto on tapahtunut virtuaalikoneen ja vahvista automaattisesti epäonnistui](http://social.technet.microsoft.com/wiki/contents/articles/25507.the-virtual-machine-isn-t-ready-for-planned-failover.aspx)
-   Testaa automaattisesti
    -   [Automaattisesti ei saa aloittaa, koska testi automaattisesti on käynnissä](http://social.technet.microsoft.com/wiki/contents/articles/31111.failover-could-not-be-initiated-since-test-failover-is-in-progress.aspx)
-   <span style="color:green;">Uusi</span>  Automaattisesti aikakatkaistaan ja 'PreFailoverWorkflow tehtävän WaitForScriptExecutionTaskTimeout' vuoksi verkon suojaus-ryhmä, virtuaalikoneen tai johon koneen kuuluu aliverkon liittyvät asetukset. Katso tiedot ["PreFailoverWorkflow](https://aka.ms/troubleshoot-nsg-issue-azure-site-recovery) tehtävään WaitForScriptExecutionTaskTimeout".


### <a name="configuration-server-process-server-master-target"></a>Määritysten palvelin prosessin palvelin perustyyli kohde
Määrityspalvelimen (CS) prosessin Server (PS) perustyyli Targer (MT)
-   [ESXi isäntä, jossa PS/CS nykyisessä ja Violetti näytön kuolinaika epäonnistuu AM.](http://social.technet.microsoft.com/wiki/contents/articles/31107.vmware-esxi-host-experiences-a-purple-screen-of-death.aspx)

### <a name="remote-desktop-troubleshooting-after-failover"></a>Etätyöpöytä jälkeen automaattisesti vianmääritys
-   Monet asiakkaat on järjestelmässä ongelmat muodostaa epäonnistunutta Azure AM kautta. [Käyttää vianmäärityksen asiakirjan RDP AM](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx)

#### <a name="adding-a-public-ip-on-a-resource-manager-virtual-machine"></a>Lisääminen julkiseen IP resurssien hallinnan virtual tietokoneeseen
Jos et ole yhteydessä Azure Express reitin tai sivuston sivuston VPN-yhteyden kautta portaalissa **Yhdistä** -painike näkyy harmaana, joudut Luo ja määritä oman AM julkiseen IP-osoite, ennen kuin voit käyttää RDP/SSH. Noudata seuraavia ohjeita voit lisätä julkiseen IP virtuaalikoneen verkko-liittymän.  

![Lisääminen julkiseen IP-verkkoliittymän epäonnistui virtuaalikoneen päälle](media/site-recovery-monitoring-and-troubleshooting/createpublicip.gif)
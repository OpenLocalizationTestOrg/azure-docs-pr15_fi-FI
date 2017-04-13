<properties
    pageTitle="Sivustojen palauttamisen käyttöönotto valmisteleminen | Microsoft Azure"
    description="Tässä artikkelissa käsitellään käyttäjien replikointia Azure sivustojen palauttamisen käyttöönotto."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor="tysonn"/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="10/05/2016"
    ms.author="raynew"/>

# <a name="prepare-for-azure-site-recovery-deployment"></a>Valmistaudu Azure sivustojen palauttamisen käyttöönotto

Lue tästä artikkelista korkean tason yleisiä Azure palauttaminen-palvelun tukemat replikoinnin kutakin skenaariota käyttöönoton vaatimuksista. Kun olet lukenut jokaisen skenaarion yleiset vaatimukset, voit linkittää käyttöönotto tiedot kunkin käyttöönottoa varten.

Luettuasi tämän artikkelin kirjaa kommentteja tai kysymyksiä alaosassa on artikkelissa tai [Azure palautus Services keskustelupalstalla](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="overview"></a>Yleiskatsaus

Organisaatioiden on BCDR strategia, joka määrittää, miten sovellukset ja työmääriä pysy käynnissä ja käytettävissä aikana suunnitellut ja suunnittelemattomat käyttökatkot ja palauttaa normaalijakauman käytön edellytykset mahdollisimman pian. BCDR strategia kannattaa säilyttäminen yritystietojen turvassa ja palautettavissa ja varmistetaan, että työmääriä pysyvät jatkuvasti käytettävissä, kun tietojen tapahtuu. 

Sivuston palautus on Azure-palvelu prosentuaalista osuutta BCDR strategia mukaan orchestrating replikoinnin paikallisen fyysiset palvelimet ja näennäiskoneiden pilveen (Azure) tai toissijaisen palvelinkeskukseen. Kun katkokset ensisijainen sijainti-asiakas ei päälle toissijainen sijainnin vastaamaan itsellesi sovellukset ja toiminnoista, jotka ovat käytettävissä. Asiakas ei takaisin ensisijainen sijainti, kun se palauttaa normaalijakauman toimintoihin. Lisätietoja [Mikä on sivuston palauttaminen?](site-recovery-overview.md)

## <a name="select-your-deployment-model"></a>Valitse käyttöönottomalli

Azure on kaksi eri [käyttöönoton mallien](../resource-manager-deployment-model.md) luominen ja käyttäminen resurssit: Azure Resurssienhallinta mallia ja perinteinen palveluiden hallinta-malli. Azure on myös kaksi portaaleihin – [Azure perinteinen portal](https://manage.windowsazure.com/) , joka tukee perinteinen käyttöönoton mallia ja [Azure portal](https://ms.portal.azure.com/) tuki sekä käyttöönotto-mallit.

Sivuston palautus on käytettävissä sekä perinteinen portaalin ja Azure-portaalin. Azure perinteinen portaalissa voit tukea palauttaminen perinteinen palveluiden hallinta-malli. Azure-portaalissa voit tukea perinteinen malli tai resurssi hallinnan käyttöönotto. [Lue lisää](site-recovery-overview.md#site-recovery-in-the-azure-portal) käyttöönotto Azure-portaalissa.

Kun olet olet valitseminen käyttöönoton mallin huomautuksen:

- Suosittelemme, että käytät [Azure portal](https://portal.azure.com) ja resurssien hallinnan malli mahdollisuuksien mukaan.
- Sivuston palauttaminen tarjoaa helpompaa ja nopeampaa käytön aloittaminen kokemus Azure-portaalissa.
- Käytä Azure portaalin, voit toistaa koneet perinteinen ja Azure tallennustilan Resurssienhallinta. Lisäksi Azure-portaalissa voit käyttää LRS tai GRS tallennustilan tilit.
- Azure portaalin yhdistää palauttaminen ja varmuuskopiointi-palveluiden yksittäisen palautus palvelut-säilö niin, että voit määrittää ja hallita BCDR palveluita yhdestä paikasta.
- Käyttäjät, joilla on valmisteltu Cloud ratkaisu Provider (CSP)-ohjelmalla Azure tilaukset voivat nyt hallita sivuston palauttaminen toimintojen Azure-portaalissa.
- Replikoiminen VMware VMs tai fyysisten laitteiden Azure Azure-portaalissa on useita uusia ominaisuuksia, myös tiettyjen levyjen ulkopuolelle replikoinnin tuki premium tallennustilaa ja mahdollisuus.


## <a name="select-your-deployment-scenario"></a>Valitse käyttöönoton-skenaario

**Käyttöönotto** | **Tiedot** | **Azure portal** | **Perinteinen portal** | **PowerShellin**
---|---|---|---|---
**VMware VMs Azure avulla** | Toistaa VMware VMs Azure-tallennustilan | Azure-portaalin VMs voi epäonnistua päälle perinteinen tai Resurssienhallinta-tallennustilan<br/><br/> Käyttöönoton [Azure portaalissa](site-recovery-vmware-to-azure.md) on virtaviivaistettu käyttöönoton kokemus ja toimintojen eduista määrä. | Azure perinteinen portaalissa voit [parannettu mallin](site-recovery-vmware-to-azure-classic.md) ottaminen käyttöön ja ei onnistu perinteinen Azure tallennustilan päälle.<br/><br/> On myös aiempien versioiden mallin, joka ei kannata käyttää uuden käyttöönotoissa. |  PowerShellin ei tällä hetkellä tueta.
**Voit Azure VMs Hyper-V** | Hyper-V VMs Azure säilöön. VMs voidaan hallita System Center VMM paveikslėlis tai ilman VMM Hyper-V isännät. | Azure-portaalin VMs voi epäonnistua päälle perinteinen tai Resurssienhallinta-tallennustilan.<br/><br/> Azure-portaalissa on virtaviivaistettu käyttöönotto-toiminto. [Lue lisää](site-recovery-vmm-to-azure.md) siitä, että replikoiminen Hyper-V VMs-VMM paveikslėlis. [Lue lisää](site-recovery-hyper-v-site-to-azure.md) siitä, että replikoiminen Hyper-V VMs (ilman VMM).| Perinteinen Azure-portaalissa voit voi epäonnistua VMs päälle perinteinen Azure-tallennustilan | PowerShell-käyttöönoton tuetaan.
**Fyysinen Azure-palvelinten** | Toistaa fyysiset Windows-tai Linux-palvelimet Azure-tallennustilan | Azure-portaalin palvelinten voi epäonnistua päälle perinteinen tai Resurssienhallinta-tallennustilan.<br/><br/> Käyttöönoton [Azure portaalissa](site-recovery-vmware-to-azure.md) on virtaviivaistettu käyttöönotto-toiminto ja toimintojen eduista määrä. | Azure perinteinen portaalissa voit käyttöönotto [parannettu mallia](site-recovery-vmware-to-azure-classic.md) ja ei onnistu perinteinen Azure tallennustilan päälle.<br/><br/> On myös aiempien versioiden mallin, joka ei kannata käyttää uuden käyttöönotoissa. | PowerShell-käyttöönoton ei tällä hetkellä tueta.
**Toissijaisen VMware VMs/fyysisiä palvelinten* | Replikoida toissijaisen sivuston VMware VMs tai fyysinen Windows-tai Linux-palvelimiin. [Lisätietoja ja lataa](http://download.microsoft.com/download/E/0/8/E08B3BCE-3631-4CED-8E65-E3E7D252D06D/InMage_Scout_Standard_User_Guide_8.0.1.pdf) InMage Scout käyttöoppaassa. | Ei käytettävissä Azure-portaalissa | Perinteinen-portaalissa Lataa InMage Scout palauttaminen säilöstä. | PowerShell-käyttöönoton ei tällä hetkellä tueta
**Hyper-V VMs toissijainen sivustoon** | Toistaa Hyper-V VMs-VMM paveikslėlis toissijaisen pilveen | [Azure portaalissa](site-recovery-vmm-to-vmm.md) voit toistaa Hyper-V VMs-VMM paveikslėlis käyttämällä Hyper-V replikan vain toissijainen sivustoon. SAN ei tällä hetkellä tueta. | Azure perinteinen portaalissa voit toistaa Hyper-V VMs-VMM paveikslėlis [Hyper-V replikan](site-recovery-vmm-to-vmm-classic.md) tai [SAN replikoinnin](site-recovery-vmm-san.md) toissijainen sivustoon | PowerShellin käyttöönotto on tuettu



## <a name="check-what-you-need-for-deployment"></a>Tarkista, mitä tarvitset käyttöönottoa varten

### <a name="replicate-to-azure"></a>Azure replikoida

**Vaatimus** | **Tiedot** 
---|---
**Azure-tili** | Tarvitset [Microsoft Azure](http://azure.microsoft.com/) -tiliin.<br/><br/> Voit aloittaa [maksuttoman kokeiluversion käyttäjäksi](https://azure.microsoft.com/pricing/free-trial/). [Lue lisää](https://azure.microsoft.com/pricing/details/site-recovery/) siitä, että sivuston palauttaminen hinnoittelua.
**Azure-tallennustilan** | Replikoitua tiedot tallennetaan Azure tallennustilan ja Azure VMs luodaan, kun vikasietotila ilmenee. Replikoida Azure, tarvitset [Azure-tallennustilan tilin](../storage/storage-introduction.md).<br/><br/>Jos olet käyttöönotto palauttaminen perinteinen portaalissa tarvitset vähintään yksi [Vakio GRS tallennustilan tilit](..storage/storage-redundancy.md#geo-redundant-storage).<br/><br/> Jos otat käyttöön Azure-portaalissa voit GRS tai LRS tallennustilan.<br/><br/> Jos olet replikoiminen VMware VMs tai fyysinen palvelimia Azure portaalin premium muistiin tueta. Huomaa, että jos käytät premium-tallennustilan tilin myös tarvitset vakio tallennustilan-tilin tallentamiseen replikoinnin lokit, jotka siepata jatkuvaa muutokset paikalliset tiedot. [Premium tallennustilan](../storage/storage-premium-storage.md) käytetään yleensä näennäiskoneiden, jotka tarvitsevat johdonmukaisesti tehokas IO ja host IO tehostettu työmääriä, pieni viive.<br/><br/> Jos haluat tallentaa replikoitua premium-tilin avulla, sinun on myös vakio tallennustilan-tilin tallentamiseen replikoinnin lokit, jotka siepata jatkuvaa muutokset paikalliset tiedot.
**Azure verkossa** | Replikoida Azure, tarvitset Azure verkossa, jossa Azure VMs muodostaa yhteyden, kun ne on luotu automaattisesti jälkeen.<br/><br/> Jos otat käyttöön perinteinen portaalissa käytät perinteistä verkkoon. Jos olet käyttöönotto Azure-portaalissa, voit käyttää perinteinen tai Resurssienhallinta verkkoon.<br/><br/> Verkossa on oltava sama kuin säilö alue.
**Verkon määritys (VMM Azure)** | Jos olet replikoiminen from VMM Azure- [verkon määritys](site-recovery-network-mapping.md) varmistaa, että Azure VMs on yhdistetty oikein verkkoihin jälkeen automaattisesti.<br/><br/> Voit määrittää verkon määritys tarvitset määrittää AM verkot VMM-portaalissa.
**Paikallisen** | **VMware VMs**: tarvitset paikallisen tietokoneen palauttaminen osat, VMware vSphere isännät/vCenter palvelimen ja VMs, jonka haluat kopioida. [Lue lisää](site-recovery-vmware-to-azure.md#configuration-server-prerequisites).<br/><br/> **Fyysinen palvelimiin**: Jos olet replikoiminen fyysiset palvelimet on käynnissä palauttaminen osat ja fyysinen palvelimissa, jotka haluat kopioida paikallisen-tietokoneissa. [Lue lisää](site-recovery-vmware-to-azure.md#configuration-server-prerequisites). Jos haluat automaattisesti Azure jälkeen [takaisin](site-recovery-failback-azure-to-vmware.md) epäonnistuu tarvitset VMware infrastruktuuri, voit tehdä sen.<br/><br/> **Hyper-V VMs**: Jos haluat toistaa Hyper-V VMs-VMM paveikslėlis tarvitset VMM palvelimeen ja mitkä VMs haluat suojata Hyper-V isännät sijaitsevat. [Lue lisää](site-recovery-vmm-to-azure.md#on-premises-prerequisites).<br/><br/> Jos haluat toistaa Hyper-V VMs ilman VMM tarvitset Hyper-V isännät joina VMs sijaitsevat. [Lue lisää](site-recovery-hyper-v-site-to-azure.md#on-premises-prerequisites).
**Suojatun koneet** | Suojatun koneet, joka replikoida Azure on täytettävä [Azure edellytykset](#azure-virtual-machine-requirements) seuraavalla tavalla.


### <a name="replicate-to-a-secondary-site"></a>Toissijainen sivuston replikoida

**Vaatimus** | **Tiedot** 
---|---
**Azure-tili** | Tarvitset [Microsoft Azure](http://azure.microsoft.com/) -tiliin.<br/><br/> Voit aloittaa [maksuttoman kokeiluversion käyttäjäksi](https://azure.microsoft.com/pricing/free-trial/). [Lue lisää](https://azure.microsoft.com/pricing/details/site-recovery/) siitä, että sivuston palauttaminen hinnoittelua.
**Paikallisen** | **VMware VMs**: ensisijainen sivuston sinun on prosessin palvelimen välimuistiin, pakkaaminen ja salaaminen replikoinnin tietoja ennen sen lähettämistä toissijaisen sivuston. Toissijaisen sivuston asennat määrityspalvelimen voit valvoa käyttöönottoa ja perusmuodon kohde-palvelimeen. Suosittelemme myös vContinuum palvelimen helpompaa hallintaa varten. Lisäksi tarvitset vähintään yksi VMware vSphere isäntien ja halutessasi vCenter palvelimeen. [Lisätietoja on artikkelissa](site-recovery-vmware-to-vmware.md)<br/><br/> **Valitse VMM paveikslėlis VMs Hyper-V**: sinun on VMM palvelinten määrittäminen ja Hyper-V isännät, joka sisältää VMs, jonka haluat kopioida. [Lue lisää](site-recovery-vmm-to-vmm.md#on-premises-prerequisites).
**Verkon määritys (VMM VMM)** | Jos olet replikoiminen VMM, VMM, [verkon määritys](site-recovery-network-mapping.md) varmistaa, että replikan VMs on yhdistetty tieto verkkoihin jälkeen automaattisesti ja sijoitetaan optimaalisesti Hyper-V isännät. Voit määrittää verkon määritys tarvitset määrittää AM verkot VMM palvelimiin.
**Tallennustilan yhdistäminen** | Jos olet replikoiminen VMM-VMM, voit halutessasi määrittää [tallennustilan yhdistäminen](site-recovery-storage-mapping.md) tallennustilan kohteen määrittäminen replikoitua VMs. Tallennustilan määritys voidaan määrittää Hyper-V replikan ja SAN replikoinnin.<br/><br/> Tallennustilan määritystä ei ole tällä hetkellä tueta Azure-portaalissa.


## <a name="verify-url-access"></a>Tarkista URL-yhteys

Varmista, että nämä URL-osoitteet ovat käytettävissä olevat palvelinten.

**URL-OSOITE** | **VMM VMM** | **VMM Azure** | **Azure Hyper-V** | **VMware Azure**
---|---|---|---|---
*. accesscontrol.windows.net | Salli | Salli | Salli | Salli
*. backup.windowsazure.com | Ei tarvita | Salli | Salli | Salli
*. hypervrecoverymanager.windowsazure.com | Salli | Salli | Salli | Salli
*. store.core.windows.net | Salli | Salli | Salli | Salli
*. blob.core.windows.net | Ei tarvita | Salli | Salli | Salli
https://www.msftncsi.com/ncsi.txt | Salli | Salli | Salli | Salli
https://dev.MySQL.com/Get/Archives/MySQL-5.5/MySQL-5.5.37-Win32.msi | Ei tarvita | Ei tarvita | Ei tarvita | Salli

## <a name="azure-virtual-machine-requirements"></a>Azure virtuaalikoneen vaatimukset

Voit ottaa sivuston palauttaminen replikointia näennäiskoneiden ja fyysinen sellaisille käyttöjärjestelmille, tukemat Azure-palvelimiin. Tämä sisältää useimmat Windowsin ja Linux versiot. Tarvitset varmistaaksesi, että paikallinen näennäiskoneiden, jonka haluat suojata Azure vaatimusten mukainen.


**Toiminto** | **Vaatimukset** | **Tiedot**
---|---|---
Hyper-V Host (isäntä) | On oltava käynnissä Windows Server 2012 R2 | Edellytykset valinta ei onnistu, jos käyttöjärjestelmä, jota ei tueta
VMware hypervisor  | Tuetut käyttöjärjestelmä | [Tarkistaminen](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment)
Vieras-käyttöjärjestelmä | Hyper-V Azure replikointi: palauttaminen tukee kaikissa käyttöjärjestelmissä, jotka ovat [Azure tukemat](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx). <br/><br/> VMware ja fyysinen palvelimen replikoinnin: Tarkista Windowsin ja Linux [edellytykset](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment) | Edellytykset valintaruudun epäonnistuu, jos, joita ei tueta.
Vieras Käyttöjärjestelmäarkkitehtuuri | 64-bittinen | Edellytykset valinta ei onnistu, jos, joita ei tueta
Käyttöjärjestelmä levyn koko |  1023 gt | Edellytykset valinta ei onnistu, jos, joita ei tueta
Käyttöjärjestelmä levyn määrä | 1 | Edellytykset valintaruudun epäonnistuu, jos, joita ei tueta.
Tietoja levyn määrä | 16 tai pienempi (suurin arvo on funktio luomisen virtuaalikoneen kokoa. 16 = XL) | Edellytykset valinta ei onnistu, jos, joita ei tueta
Tietoja levyn Näennäiskiintolevyn koko | Enintään 1023 gt | Edellytykset valinta ei onnistu, jos, joita ei tueta
Verkkosovittimien | Useita sovittimia tuetaan |
Staattinen IP-osoite | Tuettu | Jos ensisijainen virtuaalikoneen käyttää staattinen IP-osoite, voit määrittää virtuaalikoneen Azure: ssä luodut staattinen IP-osoite. Huomaa, että käytössä Hyper-v linux virtual koneen staattinen IP-osoite ei tueta.
iSCSI-levyltä | Ei tueta | Edellytykset valinta ei onnistu, jos, joita ei tueta
Jaetun Näennäiskiintolevyn | Ei tueta | Edellytykset valinta ei onnistu, jos, joita ei tueta
FC levy | Ei tueta | Edellytykset valinta ei onnistu, jos, joita ei tueta
Kiintolevyn muoto| NÄENNÄISKIINTOLEVYN <br/><br/> VHDX | Vaikka VHDX ei ole tällä hetkellä tukemien Azure-sivuston palauttaminen muuntaa automaattisesti VHDX Näennäiskiintolevyn, kun asiakas ei Azure päälle. Kun takaisin paikalliseen näennäiskoneiden jatkaa VHDX-muodossa.
BitLocker | Ei tueta | BitLocker on poistettava käytöstä ennen virtual machine suojaamista.
Virtuaalikoneen nimi| 1 – 63 merkkiä. Vain kirjaimia, numeroita ja väliviivoja. Olisi alkamis- ja kirjain tai numero | Päivittää arvon palauttaminen virtuaalikoneen ominaisuudet
Virtuaalikoneen tyyppi | <p>Luonti 1</p> <p>2 - Windows luonti</p> |  Luonti 2 virtual tietokoneen perustiedot levy, joka sisältää 1 tai 2 tietomääriä levyn muodossa VHDX, joka on alle 300 gt tuetaan OS levyn tyyppi. Linux luonti 2 näennäiskoneiden ei tueta. [Lue lisätietoja](https://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/)



## <a name="optimizing-your-deployment"></a>Käyttöönoton optimointi

Seuraavat vihjeet auttavat optimointi ja Skaalaa käyttöönoton.

- **Käyttöjärjestelmän Asemakoko**: kun virtual machine replikoida Azure käyttöjärjestelmän äänenvoimakkuuden on oltava pienempi kuin 1 TT. Jos sinulla on enemmän kuin tämä tietomääristä voit manuaalisesti siirtää ne toiselle levylle ennen kuin aloitat, käyttöönotto.
- **Tietoja levyn kokoa**: Jos olet replikoiminen Azure, voi olla enintään 32 tietojen levylle virtual tietokoneeseen, joissa on enintään 1 TT. Voit replikoida tehokkaasti ja epäonnistua ~ 32 TT virtual koneen päälle.
- **Palautus suunnitelman rajoitukset**: palauttaminen voi skaalata näennäiskoneiden tuhansia. Palautus suunnitelmien on suunniteltu mallina sovelluksissa, jotka pitäisi epäonnistua päälle yhteen niin, että olemme koneet määrän rajoittaminen 50 palautus suunnitelma.
- **Azure palvelun rajoitukset**: aina Azure-tilaukseesi sisältyy oletusarvoisesti rajoitukset joukko sydämiä, valitse cloud services jne. On suositeltavaa, että suoritat testin automaattisesti, voit tarkistaa tilauksen resurssien käytettävyyden. Voit muokata nämä raja-arvot Azure tuki kautta.
- **Kapasiteetin suunnittelu**: Tässä artikkelissa kerrotaan [kapasiteetin suunnittelua](site-recovery-capacity-planner.md) palauttaminen.
- **Replikoinnin kaistanleveyden**: Jos ole lyhyt replikoinnin kaistanleveyden Huomaa, että:
    - **ExpressRoute**: palauttaminen toimii Azure ExpressRoute ja WAN optimointien, kuten Riverbed. [Lue lisää](http://blogs.technet.com/b/virtualization/archive/2014/07/20/expressroute-and-azure-site-recovery.aspx) ExpressRoute.
    - **Replikoinnin liikenne**: palauttaminen käyttää suorittaa vain tietolohkot ja koko Näennäiskiintolevyn smart ensimmäisen replikoinnin. Vain muutokset, replikoida jatkuvan replikoinnin aikana.
    - **Verkkoliikenteen**: verkkoliikennettä [Windows QoS](https://technet.microsoft.com/library/hh967468.aspx) määrittäminen perustuu kohde-IP-osoite ja portin käytännön kanssa replikoinnin avulla voit hallita.  Lisäksi jos olet replikoiminen Azure palauttaminen käyttämällä Azure Backup agent, voit määrittää rajoitusten määrittäminen, agentti. [Lue lisää](https://support.microsoft.com/kb/3056159).
- **RTO**: mitataan tulostusasetuksilla palauttaminen palautus aika tavoitteen (RTO) Suosittelemme Suorita testi automaattisesti ja Näytä sivuston palauttaminen työt selvittää, paljonko aikaa suorittamiseen kuluvaa toiminnot. Jos olet kirjautumatta päälle Azure-paras RTO Suosittelemme automatisoida kaikki manuaaliset toiminnot integroimalla Azure automaatio ja palautus-palvelupakettien kanssa.
- **RPO**: palauttaminen tukee lähellä-painikkeen palautus pisteen tavoitteelle (RPO) Azure replikoinnin. Tämä olettaa riittävästi kaistanleveyden palvelinkeskuksen ja Azure välillä.


##<a name="service-urls"></a>Palvelun URL-osoitteet
Varmista, että nämä URL-osoitteet ovat käytettävissä palvelimesta


**URL-osoitteet** | **VMM VMM** | **VMM Azure** | **Azure Hyper-V-sivusto** | **VMware Azure**
---|---|---|---|---
 \*. accesscontrol.windows.net | Pakollinen käyttö  | Pakollinen käyttö  | Pakollinen käyttö  | Pakollinen käyttö
 \*. backup.windowsazure.com |  | Pakollinen käyttö  | Pakollinen käyttö  | Pakollinen käyttö
 \*. hypervrecoverymanager.windowsazure.com | Pakollinen käyttö  | Pakollinen käyttö  | Pakollinen käyttö  | Pakollinen käyttö
 \*. store.core.windows.net | Pakollinen käyttö  | Pakollinen käyttö  | Pakollinen käyttö  | Pakollinen käyttö
 \*. blob.core.windows.net |  | Pakollinen käyttö  | Pakollinen käyttö  | Pakollinen käyttö
 https://www.msftncsi.com/ncsi.txt | Pakollinen käyttö  | Pakollinen käyttö  | Pakollinen käyttö  | Pakollinen käyttö
 https://dev.MySQL.com/Get/Archives/MySQL-5.5/MySQL-5.5.37-Win32.msi | | | | Pakollinen käyttö


## <a name="next-steps"></a>Seuraavat vaiheet

Oppiminen ja vertailu Yleiset käyttöönottovaatimukset jälkeen voit lukea yksityiskohtaiset edellytykset ja Käynnistä kutakin skenaariota käyttöönotto.

- [Toistaa VMware näennäiskoneiden Azure](site-recovery-vmware-to-azure-classic.md)
- [Toistaa fyysiset palvelimet Azure](site-recovery-vmware-to-azure-classic.md)
- [Toistaa VMM paveikslėlis Hyper-V server Azure](site-recovery-vmm-to-azure.md)
- [Replikoida Azure Hyper-V näennäiskoneiden (ilman VMM)](site-recovery-hyper-v-site-to-azure.md)
- [Toistaa Hyper-V VMs toissijainen sivustoon](site-recovery-vmm-to-vmm.md)
- [Toistaa Hyper-V VMs toissijaisen sivuston SAN kanssa](site-recovery-vmm-san.md)
- [Toistaa Hyper-V VMs yksittäisen VMM-palvelimen kanssa](site-recovery-single-vmm.md)

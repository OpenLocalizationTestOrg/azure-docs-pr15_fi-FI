<properties
    pageTitle="Miten sivuston palauttaminen toimii? | Microsoft Azure"
    description="Tässä artikkelissa on yleiskatsaus sivuston palautus-arkkitehtuuri"
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/05/2016"
    ms.author="raynew"/>

# <a name="how-does-azure-site-recovery-work"></a>Miten Azure palauttaminen toimii?

Tässä artikkelissa ymmärtämään taustalla arkkitehtuuri Azure palauttaminen-palvelun ja osat, jotka helpottavat toimi. 

Kirjaa kommentteja tai kysymyksiä alaosassa on tämän artikkelin tai [Azure palautus Services keskustelupalstalle](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="overview"></a>Yleiskatsaus

Organisaatioiden on BCDR strategia, joka määrittää, miten sovellukset ja työmääriä pysy käynnissä ja käytettävissä aikana suunnitellut ja suunnittelemattomat käyttökatkot ja palauttaa normaalijakauman käytön edellytykset mahdollisimman pian. BCDR strategia kannattaa säilyttäminen yritystietojen turvassa ja palautettavissa ja varmistetaan, että työmääriä pysyvät jatkuvasti käytettävissä, kun tietojen tapahtuu. 

Sivuston palautus on Azure-palvelu prosentuaalista osuutta BCDR strategia mukaan orchestrating replikoinnin paikallisen fyysiset palvelimet ja näennäiskoneiden pilveen (Azure) tai toissijainen palvelinkeskukseen. Kun katkokset ensisijainen sijainti-asiakas ei päälle toissijainen sijainnin vastaamaan itsellesi sovellukset ja toiminnoista, jotka ovat käytettävissä. Asiakas ei takaisin ensisijainen sijainti, kun se palauttaa normaalijakauman toimintoihin. Lisätietoja [Mikä on sivuston palauttaminen?](site-recovery-overview.md)

## <a name="site-recovery-in-the-azure-portal"></a>Sivuston palauttaminen Azure-portaalissa

Azure on kaksi eri [käyttöönoton mallien](../resource-manager-deployment-model.md) luominen ja käyttäminen resurssit: Azure Resurssienhallinta-malli ja perinteinen palveluiden hallinta-malli. Azure on myös kaksi portaaleihin – [Azure perinteinen portal](https://manage.windowsazure.com/) , joka tukee perinteinen käyttöönottomalli ja [Azure portal](https://portal.azure.com) tuki sekä käyttöönotto-mallit.

Sivuston palautus on käytettävissä sekä perinteinen portaalin ja Azure-portaalin. Azure perinteinen portaalissa voit tukea palauttaminen perinteinen palveluiden hallinta-malli. Azure-portaalissa voit tukea perinteinen malli tai resurssi mallin ominaisuuksissa. [Lue lisää](site-recovery-overview.md#site-recovery-in-the-azure-portal) käyttöönotto Azure-portaalissa.

Tämän artikkelin tiedot koskevat perinteinen- ja Azure portaalin ominaisuuksissa. Erot on merkitty tarvittaessa.

## <a name="deployment-scenarios"></a>Käyttöönottoskenaariot

Sivuston palauttaminen voidaan ottaa käyttöön skenaarioiden määrä replikoinnin orchestrate:

- **Toistaa VMware näennäiskoneiden**: Voit toistaa paikallisen VMware näennäiskoneiden Azure tai toissijainen palvelinkeskukseen.
- - **Toistaa fyysisten laitteiden**: Voit toistaa fyysinen, joissa käytetään Windows- tai Linux Azure tai toissijainen palvelinkeskukseen. Prosessi, jossa replikoiminen fyysisten laitteiden on lähes sama kuin prosessi, jossa VMware VMs replikoiminen
- **Toistaa Hyper-V VMs (ilman VMM)**: Voit toistaa Hyper-V VMs, jotka eivät ole hallitsee VMM Azure.
- **System Center VMM paveikslėlis hallitaan replikoida Hyper-V VMs**: Voit toistaa paikallisen Hyper-V näennäiskoneiden Hyper-V host-palvelimissa VMM paveikslėlis Azure tai toissijaisen palvelinkeskukseen. Voit toistaa käyttämällä vakio Hyper-V replikan tai käyttämällä SAN replikoinnin.
- **Siirtää VMs**: sivuston palauttaminen alueiden välillä [siirtää Azure IaaS VMs](site-recovery-migrate-azure-to-azure.md) tai [siirtää esiintymät sisältyy AWS Windows](site-recovery-migrate-aws-to-azure.md) Azure IaaS VMs, voit käyttää. Tällä hetkellä vain siirron tuetaan toisin sanoen voi epäonnistua nämä VMs päälle, mutta niitä ei voi epäonnistua takaisin.

Sivuston palauttaminen toistaa useimpien sovellusten fyysiset palvelimet näitä VMs käytössä. Voit avata täydellinen yhteenveto tuettujen sovelluksia [mitä työmääriä Azure palauttaminen suojata?](site-recovery-workload.md)


## <a name="replicate-to-azure-vmware-virtual-machines-or-physical-windowslinux-servers"></a>Rinnakkaiset Azure: VMware näennäiskoneiden tai fyysinen Windows-tai Linux-palvelimia

Muutamalla eri tapoja replikoida VMware VMs palauttaminen kanssa.

- **Azure-portaalissa**– kun otat käyttöön palauttaminen Azure voi epäonnistua VMs päälle, perinteinen palvelun hallinta säilöön- tai resurssien hallinta-portaalissa. Monia etuja, mukaan lukien pätevyys replikoida perinteinen tai Resurssienhallinta tallennustilan Azure näyttää replikoiminen VMware VMs Azure-portaalissa. [Lue lisää](site-recovery-vmware-to-azure.md).
- **Perinteinen portaalissa**– voit ottaa sivuston palauttaminen käyttämällä tehostettu käyttökokemus perinteinen portaalissa. Tätä kannattaa käyttää kaikkien uusien versioiden perinteinen-portaalissa. Käyttöönottoon voit voit vain epäonnistua päälle VMs Azure perinteinen tallennustilan eikä Resurssienhallinta-tallennustilan. [Lue lisää](site-recovery-vmware-to-azure-classic.md). [Vanha versio](site-recovery-vmware-to-azure-classic-legacy.md) VMware replikoinnin perinteinen portaalissa määrittämisestä on myös. Tämä ei kannata käyttää uuden käyttöönotoissa.  Jos olet jo asentanut käyttämällä vanha versio [tietoja siirtyminen](site-recovery-vmware-to-azure-classic-legacy.md#migrate-to-the-enhanced-deployment) parannettu käyttöönotto.



Arkkitehtuuri edellytykset sivustojen palauttamisen käyttöönotto replikointia VMware VMs/fyysisiä palvelinten Azure portaalin tai Azure perinteinen portaalin (parannettu) ovat samanlaiset vaivattomasti erot:

- Jos otat Azure-portaalissa voit replikoida Resurssienhallinta-pohjainen tallennustilan ja käyttävät Resurssienhallinta verkkoja yhdistää Azure VMs jälkeen automaattisesti.
- Kun otat Azure-portaalissa sekä LRS ja GRS tallennustilan tuetaan. Perinteinen portaalissa GRS tarvitaan.
- Käyttöönottoprosessin on yksinkertaistettu ja käyttäjäystävällinen Azure-portaalissa.


Näin käytön edellytykset:

- **Azure-tili**: tarvitaan Microsoft Azure-tili.
- **Azure-tallennustilan**: sinun on Azure-tallennustilan tilin replikoitua tietojen tallentamiseen. Voit käyttää perinteinen asiakas- tai Resurssienhallinta-tallennustilan tilin. Tilin voi olla LRS tai GRS, kun otat käyttöön Azure-portaalissa. Replikoitua tiedot tallennetaan Azure tallennustilan ja Azure VMs kehrätty määrittäminen, kun vikasietotila ilmenee. 
- **Azure verkon**: sinun on Azure virtual verkko, jossa Azure VMs muodostaa yhteyden, kun ne on luotu automaattisesti. Azure-portaalissa ne voivat olla verkkojen luotu perinteinen palvelun hallinnan malli tai resurssi hallinnan malli.
- **Paikallisen määrityspalvelimen**: tarvitset paikallisen Windows Server 2012 R2-tietokoneen, joka suoritetaan määrityspalvelimen ja muut sivuston palautus-osat. Jos olet replikoiminen VMware VMs on oltava käytettävissä VMware AM. Jos haluat toistaa fyysiset palvelimet tietokoneessa voi olla fyysinen. Sivuston palauttaminen komponentit asennetaan tietokoneeseen:
    - **Määrityspalvelimen**: paikallisen ympäristön ja Azure välisen koordinoi ja hallitsee tietojen replikoinnin ja palauttaminen.
    - **Prosessin palvelimen**: replikoinnin yhdyskäytävän toimii. Se vastaanottaa replikoinnin tietoja suojatun lähde koneet, optimoi tallentamisesta välimuistiin, pakkaamisen ja salaus ja lähettää tiedot Azure-tallennustilan. Se myös käsittelee suojatun koneet Mobility palvelun push-asennus ja suorittaa VMware VMs Automaattinen etsiminen. Kun käyttöönoton kasvaa voit lisätä muita erillinen erillinen prosessi palvelinten käsittelemään replikoinnin liikenne kasvava tietomääristä.
    - **Perustyylisivujen kohdepalvelin**: käsittelee replikoinnin tietojen tuntisesta azuren aikana. 
- **VMware VMs tai fyysinen palvelinten replikoida**: Jokaisessa koneessa, jonka haluat toistaa Azure on asennettu Mobility service-osa. Tämä palvelu sieppaa tietojen kirjoituksia tietokoneeseen ja välittää niitä prosessi-palvelimeen. Tämän osan voit voidaan asentaa manuaalisesti, tai voit olla miten ja automaattisesti prosessi-palvelin kun otat replikoinnin tietokoneelle.
- **vSPhere isännät/vCenter server**: tarvitset vähintään yksi vSphere host palvelimessa on VMware VMs. On suositeltavaa vCenter palvelimeen ja hallita näitä isännät otetaan käyttöön.
- **Tuntisesta**: näin edellytykset:
    - **Fyysinen fyysisiä tuntisesta ei tueta**: Tämä tarkoittaa, että jos epäonnistua fyysinen Azure-palvelinten päälle ja haluat epäonnistua takaisin, sinun on epäonnistua takaisin VMware AM. Palaa fyysisiä palvelimeen ei onnistu. Sinun on Azure-AM takaisin ja jos et käyttöön kuin VMware AM määrityspalvelimen sinun on erillinen perusmuodon kohde palvelimeen VMware AM määrittäminen. Tämä tarvita, sillä perusmuodon palvelimesta toimii ja liittää VMware tallennustilan Palauta levyjen VMware AM.
    - - **Tilapäinen prosessin server Azure-tietokannassa**: haluat epäonnistua takaisin Azure-automaattisesti, sinun on määritetty prosessin palvelimeksi Azure-AM määrittäminen käsittelemään replikoinnin azuren jälkeen. Voit poistaa tämän AM tuntisesta päätyttyä.
    - **VPN-yhteys**: tarvitset tuntisesta VPN yhteys (tai Azure ExpressRoute) Määritä Azure verkosta paikallisen-sivustoon.
    - **Erillinen paikallisen perusmuodon kohde server**: paikallisen perusmuodon palvelimesta käsittelee tuntisesta. Perusmuodon palvelimesta asennetaan oletusarvoisesti hallinta palvelimessa, mutta jos olet kaatuvat liikenne takaisin suurempia tietomääriä kannattaa määrittää erillisen paikallisen perusmuodon kohde-palvelimen tähän tarkoitukseen.

**Yleiset arkkitehtuuri**

![Parannettu](./media/site-recovery-components/arch-enhanced.png)

**Käyttöönotto-osat**

![Parannettu](./media/site-recovery-components/arch-enhanced2.png)

**Tuntisesta**

![Parannettu tuntisesta](./media/site-recovery-components/enhanced-failback.png)


- [Lue lisää](site-recovery-vmware-to-azure.md#azure-prerequisites) siitä, vaatimukset Azure portaalin käyttöönottoa varten.
- [Lue lisää](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment) siitä, parannettu käyttöönottovaatimukset perinteinen-portaalissa.
- [Lue lisää](site-recovery-failback-azure-to-vmware.md) siitä, tuntisesta Auzre-portaalissa.
- [Lue lisää](site-recovery-failback-azure-to-vmware-clas- [Learn more](site-recovery-failback-azure-to-vmware-classic.md) about failback in the Auzre portal.sic.md) siitä, tuntisesta perinteinen-portaalissa.

## <a name="replicate-to-azure-hyper-v-vms-not-managed-by-vmm"></a>Rinnakkaiset Azure: Hyper-V VMs ei hallita VMM

Voit toistaa Hyper-V VMs, jotka eivät ole hallitsee System Center VMM, palauttaminen ja Azure seuraavasti:

- **Azure-portaalissa**– kun otat käyttöön palauttaminen Azure voi epäonnistua VMs päälle, perinteinen säilöön- tai resurssien hallinta-portaalissa. [Lue lisää](site-recovery-hyper-v-site-to-azure.md).
- **Perinteinen portaalissa**-sivuston palauttaminen voit ottaa käyttöön perinteinen-portaalissa. Käyttöönottoon voit voit vain epäonnistua päälle VMs Azure perinteinen tallennustilan eikä Resurssienhallinta-tallennustilan. [Lue lisää](site-recovery-hyper-v-site-to-azure-classic.md).

Molempien versioiden arkkitehtuuri muistuttaa, paitsi että:

- Jos otat Azure-portaalissa voit toistaa Resurssienhallinta tallennustilan ja käyttävät Resurssienhallinta verkkoja yhdistää Azure VMs jälkeen automaattisesti.
- Käyttöönottoprosessin on yksinkertaistettu ja käyttäjäystävällinen Azure-portaalissa.

Näin käytön edellytykset:

- **Azure-tili**: tarvitaan Microsoft Azure-tili.
- **Azure-tallennustilan**: sinun on Azure-tallennustilan tilin replikoitua tietojen tallentamiseen. Azure-portaalissa voit perinteinen asiakas- tai Resurssienhallinta-tallennustilan tilin. Perinteinen portaalissa voit käyttää vain perinteinen tiliä. Replikoitua tiedot tallennetaan Azure tallennustilan ja Azure VMs luodaan, kun vikasietotila ilmenee.
- **Azure verkon**: sinun on Azure verkossa, jossa Azure VMs muodostaa yhteyden, kun ne on luotu automaattisesti jälkeen. 
- **Hyper-v Host (isäntä)**: tarvitset vähintään Windows Server 2012 R2 Hyper-V host-palvelin. Sivuston palauttaminen käyttöönoton aikana asentaa isännän Azure sivuston palauttaminen tarjoaja ja Microsoft Azure palautus Services-agentti.
- **Hyper-V VMs**: tarvitset vähintään yksi VMs Hyper-V Host (isäntä)-palvelimeen. Azure sivuston palauttaminen tarjoaja ja Azure palautus palvelut-agentti Hyper-V isännän palauttaminen käyttöönoton aikana. Palvelun koordinoi ja orchestrates replikoinnin palauttaminen-palvelun kanssa Internetissä. Agentti käsittelee tiedot replikoinnin tiedot HTTPS 443 päälle. Viestinnän tarjoaja ja agentti on suojattu ja salattuja. Replikoitua tiedot Azuren tallennustilaan myös salataan.

**Yleiset arkkitehtuuri**

![Azure Hyper-V-sivusto](./media/site-recovery-components/arch-onprem-azure-hypervsite.png)


- [Lue lisää](site-recovery-hyper-v-site-to-azure.md#azure-prerequisites) siitä, vaatimukset Azure portaalin käyttöönottoa varten.
- [Lue lisää](site-recovery-hyper-v-site-to-azure-classic.md#azure-prerequisites) siitä, vaatimukset perinteinen portaalin käyttöönottoa varten.



## <a name="replicate-to-azure-hyper-v-vms-managed-by-vmm"></a>Rinnakkaiset Azure: Hyper-V VMs VMM hallitsee

Hyper-V VMs-VMM paveikslėlis voi replikoida Azure palauttaminen kanssa seuraavasti:

- **Azure-portaalissa**– kun otat käyttöön palauttaminen Azure voi epäonnistua VMs päälle, perinteinen säilöön- tai resurssien hallinta-portaalissa. [Lue lisää](site-recovery-vmm-to-azure.md).
- **Perinteinen portaalissa**-sivuston palauttaminen voit ottaa käyttöön perinteinen-portaalissa. Käyttöönottoon voit voit vain epäonnistua päälle VMs Azure perinteinen tallennustilan eikä Resurssienhallinta-tallennustilan. [Lue lisää](site-recovery-vmm-to-azure-classic.md).

Molempien versioiden arkkitehtuuri muistuttaa, paitsi että:

- Jos otat Azure-portaalissa voit toistaa Resurssienhallinta-pohjainen tallennustilan ja käyttävät Resurssienhallinta verkkoja yhdistää Azure VMs jälkeen automaattisesti.
- Käyttöönottoprosessin on yksinkertaistettu ja käyttäjäystävällinen Azure-portaalissa.


Näin käytön edellytykset:

- **Azure-tili**: tarvitaan Microsoft Azure-tili.
- **Azure-tallennustilan**: sinun on Azure-tallennustilan tilin replikoitua tietojen tallentamiseen. Azure-portaalissa voit perinteinen asiakas- tai Resurssienhallinta-tallennustilan tilin. Perinteinen portaalissa voit käyttää vain perinteinen tiliä. Replikoitua tiedot tallennetaan Azure tallennustilan ja Azure VMs luodaan, kun vikasietotila ilmenee.
- **Azure verkon**: sinun on määrittäminen verkon yhdistäminen niin, että Azure VMs on yhdistetty tarvittavat verkkoihin, kun ne on luotu automaattisesti jälkeen. 
- **VMM palvelin**: on vähintään yksi paikallisen VMM palvelimilla System Center 2012 R2: n ja vähintään yksi yksityinen paveikslėlis, kanssa. Jos otat käyttöön Azure-portaalin sinun on looginen ja AM verkkojen määrittäminen niin, että voit määrittää verkon määritys. Perinteinen portaalissa tämä on valinnainen.  Looginen verkko, joka liittyy pilveen on linkitettävä AM verkkoon.
- **Hyper-v Host (isäntä)**: tarvitset vähintään Windows Server 2012 R2 Hyper-V host palvelinten VMM pilveen.
- **Hyper-V VMs**: tarvitset vähintään yksi VMs Hyper-V Host (isäntä)-palvelimeen.

**Yleiset arkkitehtuuri**

![VMM Azure](./media/site-recovery-components/arch-onprem-onprem-azure-vmm.png)

- [Lue lisää](site-recovery-vmm-to-azure.md#azure-requirements) siitä, vaatimukset Azure portaalin käyttöönottoa varten.
- [Lue lisää](site-recovery-vmm-to-azure-classic.md#before-you-start) siitä, vaatimukset perinteinen portaalin käyttöönottoa varten.




## <a name="replicate-to-a-secondary-site-vmware-virtual-machines-or-physical-servers"></a>Rinnakkaiset toissijainen sivustoon: VMware näennäiskoneiden tai fyysinen palvelimia 

Replikointia VMware VMs tai fyysinen palvelimia toissijaisen sivuston latauksena InMage Scout, joka sisältyy Azure sivuston palautus-tilaus. Voit ladata Azure-portaalista tai perinteinen Azure-portaalista. 

Sivustosta (määritys, prosessi, master kohde)-osan palvelimien määrittäminen ja asenna yhdistetyn-agentti tietokoneissa, joihin haluat kopioida. Ensimmäisen replikoinnin jälkeen jokaiseen tietokoneeseen agentti lähettää delta replikoinnin muutokset prosessi-palvelimeen. Prosessin palvelimen optimoi tiedot ja siirtää sen perusmuodon palvelimesta toissijainen-sivustossa. Configuration-palvelin hallitsee replikoinnin prosessi.

Näin edellytykset:

**Azure-tili**: Tämä skenaario käyttämällä InMage Scout otetaan käyttöön. Voit hankkia sen tarvitset Azure tilauksen. Kun olet luonut palauttaminen säilö Lataa InMage Scout ja asenna uusimmat päivitykset käyttöönoton määrittämisen.
**Prosessin server (ensisijainen sivusto)**: määrittää prosessin server-osan ensisijainen sivuston käsittelemään tallentamisesta välimuistiin, pakkaus ja tietojen optimointi. Se myös käsittelee push asennettuun yhdistetyn-edustaja, joka koneet, jonka haluat suojata. 
**VMware ESX/ESXi ja vCenter server (ensisijainen sivusto)**: Jos olet suojaaminen VMware VMs tarvitset VMware EXS/ESXi hypervisor ja halutessasi VMware vCenter palvelimen hypervisors hallinta.
- **VMs/fyysisiä servers (ensisijainen sivusto)**: VMware VMs tai Windows-tai Linux suojattava fyysinen palvelin on asennettu yhdistetyn agentti. Yhdistetty-agentti asennetaan myös visuaalisessa muodossa perusmuodon palvelimesta tietokoneissa. Agentti toimii viestintä palveluntarjoaja kaikkien osien välillä. 
- - **Määrityspalvelimen (toissijainen sivusto)**: configuration-palvelin on ensimmäisen komponentin asentamisen ja se on asennettu hallintaa, määrittäminen ja seurata käyttöönoton, käyttämällä joko hallinta-sivustossa tai vContinuum konsolin toissijaisen-sivustossa. On vain yksi määrityspalvelimen käyttöönoton ja sen on oltava asennettuna tietokoneessa, joka on käynnissä Windows Server 2012 R2.
- **vContinuum server (toissijainen sivusto)**: se on asennettu määrityspalvelimen samassa sijainnissa (toissijainen sivusto). Se tarjoaa konsolin hallintaa sekä suojatussa ympäristössä. Oletus-asennuksen vContinuum palvelin on ensimmäinen perusmuodon kohdepalvelin ja on yhdistetty-agentti asennettuna.
- **Perustyylisivujen kohde server (toissijainen sivusto)**: perustyylin palvelimesta pitää replikoitua tiedot. Se vastaanottaa tietoja prosessin palvelimesta, luo replikan koneen toissijainen-sivustoon ja pitää säilytys arvopisteiden. Tarvitset perusmuodon kohde-palvelimien määrä määräytyy koneet olet suojaaminen määrä. Jos haluat takaisin ensisijainen sivuston sinun on perusmuodon kohdepalvelin on liian. 

**Yleiset arkkitehtuuri**

![VMware VMware](./media/site-recovery-components/vmware-to-vmware.png)


## <a name="replicate-to-a-secondary-site-hyper-v-vms-managed-by-vmm"></a>Rinnakkaiset toissijainen sivustoon: Hyper-V VMs VMM hallitsee


Voit toistaa Hyper-V VMs, joita hallitaan System Center VMM toissijainen palvelinkeskukseen, jossa sivuston palauttaminen seuraavasti:

- **Azure-portaalissa**– kun otat käyttöön palauttaminen Azure-portaalissa. [Lue lisää](site-recovery-hyper-v-site-to-azure.md).
- **Perinteinen portaalissa**-sivuston palauttaminen voit ottaa käyttöön perinteinen-portaalissa. [Lue lisää](site-recovery-hyper-v-site-to-azure-classic.md).

Molempien versioiden arkkitehtuuri eroaa, sillä erotuksella, että:

- Jos otat Azure-portaalissa verkon yhdistäminen on määritettävä. Tämä on valinnainen perinteinen-portaalissa.
- Käyttöönottoprosessin on yksinkertaistettu ja käyttäjäystävällinen Azure-portaalissa.
- - Jos otetaan käyttöön Azure perinteinen portaalin [tallennustilan yhdistämistä](site-recovery-storage-mapping.md) ei käytettävissä.

Näin käytön edellytykset:

- **Azure-tili**: tarvitaan Microsoft Azure-tili.
- **VMM palvelin**: suosittelemme VMM palvelimen ensisijainen sivuston ja yhden toissijaisen sivuston kunkin joka sisältää vähintään yhden VMM yksityinen pilveen. Palvelin on käytössä vähintään System Center 2012 SP1 ja uusimmat päivitykset ja yhteydessä Internetiin. Paveikslėlis pitäisi olla Määritä Hyper-V ominaisuuksien profiili. Azure sivuston palauttaminen tarjoajan asentaa VMM palvelimessa. Palvelun koordinoi ja orchestrates replikoinnin palauttaminen-palvelun kanssa Internetissä. Palvelun ja Azure välisten yhteyksien on suojattu ja salattuja.
- **Hyper-V server**: Hyper-V host-palvelimissa sijaitsee ensisijainen ja toissijainen VMM paveikslėlis. Host palvelimet on oltava käynnissä vähintään Windows Server 2012 ja viimeisimmät päivitykset asennettu ja yhteydessä Internetiin. Tietojen replikoinnin ensisijainen ja toissijainen Hyper-V host palvelinten välillä Lähiverkon tai VPN Kerberos- tai varmenteen todennusta.  
- **Suojattu koneet**: Hyper-V host lähdepalvelimeen on oltava vähintään yksi AM, jonka haluat suojata.

**Yleiset arkkitehtuuri**

![Paikalliseen paikalliseen](./media/site-recovery-components/arch-onprem-onprem.png)


- [Lue lisää](site-recovery-vmm-to-vmm.md#azure-prerequisites) siitä, käyttöönottovaatimukset Azure-portaalissa.
- - [Lue lisää](site-recovery-vmm-to-vmm-classic.md#before-you-start) siitä, käyttöönottovaatimukset Azure perinteinen-portaalissa.




## <a name="replicate-to-a-secondary-site-with-san-replication-hyper-v-vms-managed-by-vmm"></a>Rinnakkaiset toissijaisen sivustoon SAN toistot: Hyper-V VMs VMM hallitsee

Voit toistaa Hyper-V VMs hallitaan VMM paveikslėlis käyttämällä SAN replikoinnin Azure perinteinen portaalissa toissijainen sivustoon. Tässä skenaariossa ei ole tällä hetkellä tueta uusi Azure-portaalissa. 

Tässä skenaariossa palauttaminen käyttöönoton aikana Azure sivuston palauttaminen tarjoajan asentaminen VMM-palvelimiin. Palvelun koordinoi ja orchestrates replikoinnin palauttaminen-palvelun kanssa Internetissä. Tietojen replikoinnin välillä käyttämällä synkronisen SAN replikoinnin ensisijainen ja toissijainen tallennustilan matriiseja.

Näin käytön edellytykset:

**Azure-tili**: sinun on Azure tilauksen
- **SAN matriisi**: [tuettu SAN matriisin](http://social.technet.microsoft.com/wiki/contents/articles/28317.deploying-azure-site-recovery-with-vmm-and-san-supported-storage-arrays.aspx) hallitsee ensisijainen VMM-palvelin. SAN jakaja verkkoinfrastruktuuria toisen SAN taulukon toissijainen-sivustossa.
- **VMM palvelin**: suosittelemme VMM palvelimen ensisijainen sivuston ja yhden toissijaisen sivuston kunkin joka sisältää vähintään yhden VMM yksityinen pilveen. Palvelin on käytössä vähintään System Center 2012 SP1 ja uusimmat päivitykset ja yhteydessä Internetiin. Paveikslėlis pitäisi olla Määritä Hyper-V ominaisuuksien profiili.
- **Hyper-V server**: Hyper-V host palvelinten ensisijainen ja toissijainen VMM paveikslėlis sijaitsee. Host palvelimet on oltava käynnissä vähintään Windows Server 2012 ja viimeisimmät päivitykset asennettu ja yhteydessä Internetiin.
- **Suojattu koneet**: Hyper-V host lähdepalvelimeen on oltava vähintään yksi AM, jonka haluat suojata.

**SAN replikoinnin arkkitehtuuri**

![SAN sallittuja](./media/site-recovery-components/arch-onprem-onprem-san.png)

[Lue lisää](site-recovery-vmm-san.md#before-you-start) siitä, käyttöönoton vaatimuksia.
### <a name="on-premises"></a>Paikallisen



## <a name="hyper-v-protection-lifecycle"></a>Hyper-V suojaus elinkaari

Tämä työnkulku näyttää suojaaminen ja replikoiminen kaatuvat Hyper-V näennäiskoneiden päälle. 

1. **Ota suojaus käyttöön**: määrittäminen palauttaminen säilö, Määritä Replikointiasetukset VMM cloud tai Hyper-V sivuston ja VMs suojauksen ottaminen käyttöön. Suojauksen **Ottaminen käyttöön** työ on aloitettu ja voidaan valvoa **työt** -välilehti. Työn tarkistaa, että koneen edellytykset noudattaa ja käynnistää [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx) menetelmä, joka määrittää Azure replikoinnin olet määrittänyt asetukset. **Ota suojaus käyttöön** -työ käynnistää myös alustaa koko AM replikoinnin [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx) -menetelmää.
2. **Ensimmäinen replikointi**: virtuaalikoneen tilannevedoksen otetaan ja virtual kiintolevyillä ovat replikoitua yksi kerrallaan, kunnes ne on kaikki kopioitu Azure tai toissijainen palvelinkeskukseen. Tämä suorittamiseen tarvittavan ajan määräytyy AM kokoa, kaistanleveys ja ensimmäisen replikoinnin-menetelmää. Jos levyn muuttuvat, kun ensimmäinen replikointi on käynnissä Hyper-V replikan replikoinnin seuranta jäljittää muutokset kuin Hyper-V replikoinnin lokeja (.hrl), jotka sijaitsevat levyjen samaan kansioon. Kunkin levyn on liitetty .hrl-tiedosto, joka lähetetään toissijainen tallennusväline. Huomaa, että tilannevedoksen ja kirjaudu tiedostot postipalvelimen levyn resursseja ensimmäisen replikoinnin ollessa käynnissä. Ensimmäisen replikoinnin päätyttyä AM tilannevedoksen poistetaan ja lokin delta levyn muutokset synkronoidaan ja yhdistää.
3. **Viimeistele suojaus**: ensimmäisen replikoinnin päätyttyä **Viimeistele suojaus** -työ määrittää verkko- ja muut jälkeinen Replikointiasetukset niin, että virtuaalikoneen suojataan. Jos olet replikoiminen Azure, voit joutua säätää tehosteasetuksilla virtuaalikoneen asetuksia niin, että se on valmis automaattisesti. Tässä vaiheessa voit suorittaa testin automaattisesti, voit tarkistaa kaikki toimii odotetulla tavalla.
4. **Replikoinnin**: ensimmäisen replikoinnin jälkeen delta synkronoinnin alkaessa Replikointiasetukset mukaisesti. 
    - **Replikoinnin virhe**: Jos delta replikointi epäonnistuu, ja koko replikoinnin olisi kallista kaistanleveyden tai aika ja valitse uudelleensynkronointi tapahtuu. Jos esimerkiksi jos .hrl tiedostot saavuttaa 50 prosenttia levyn kokoa, valitse AM merkitään uudelleensynkronointi varten. Uudelleensynkronointi mahdollisimman vähän tietojenkäsittely lähde- ja näennäiskoneiden tarkistussummat ja lähettämällä vain delta lähetetyt tiedot. Uudelleensynkronointi päätyttyä delta Replikointi jatkuu. Oletusarvoisesti uudelleensynkronointi on suunniteltu toimimaan itsenäisesti työaikasi ulkopuolella, mutta voit synkronoida virtual machine manuaalisesti.
    - **Replikoinnin virhe**: replikoinnin virheen ilmetessä on valmis uudelleen. Jos se on ei - ratkaistavissa oleva virhe kuten todennuksen tai lupaa virheen tai replikan koneen on virheellisessä tilassa, ei ole uudelleen yritetään. Jos se on ratkaistavissa oleva virhe, kuten virheen tai levytila tilaa ja muistin ja valitse Yritä ilmenee kasvavilla välit uudelleenyritykset (1, 2, 4, 8, 10 ja sitten 30 minuutin välein).
4. **Suunniteltu suunnittelematon failovers**: Voit suorittaa suunniteltuja tai suunnittelemattomia failovers tarpeen mukaan. Jos olet suorittanut suunnitellun automaattisesti sitten lähde VMs ovat Sammuta varmistaa ei ole tietojen menettämisen. Replikan VMs luomisen jälkeen sijoittaa-Vahvista Odottaa-tilassa. Niihin vikasietotilaa suorittamiseen tarvitset, paitsi jos olet replikoiminen kanssa SAN tapauksessa Vahvista on automaattinen. Tuntisesta voi ilmetä, kun ensisijainen sivusto on hyvin alkuun. Jos olet replikoida Azure käänteisen replikoinnin hallitaan automaattisesti. Muussa tapauksessa voit aloituskokouksen käänteisen replikoinnin manuaalisesti.
 

![työnkulun](./media/site-recovery-components/arch-hyperv-azure-workflow.png)

## <a name="next-steps"></a>Seuraavat vaiheet

[Valmistele käyttöönottoa varten](site-recovery-best-practices.md)

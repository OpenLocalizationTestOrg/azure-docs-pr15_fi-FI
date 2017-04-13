<properties
    pageTitle="Replikoida Hyper-V näennäiskoneiden VMM paveikslėlis-sivuston palauttaminen käyttämällä Azure-portaalissa Azure | Microsoft Azure"
    description="Käsitellään Azure palauttaminen orchestrate replikoinnin, automaattisesti ja Hyper-V VMs-VMM paveikslėlis palauttamista Azure Azure-portaalissa, voit ottaa käyttöön"
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor="tysonn"/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/16/2016"
    ms.author="raynew"/>

# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-azure-using-azure-site-recovery-with-the-azure-portal--microsoft-azure"></a>Toistaa Hyper-V näennäiskoneiden-VMM paveikslėlis käyttämällä Azure palauttaminen Azure-portaalissa Azure | Microsoft Azure

> [AZURE.SELECTOR]
- [Azure portal](site-recovery-vmm-to-azure.md)
- [Azure perinteinen](site-recovery-vmm-to-azure-classic.md)
- [PowerShellin resurssien hallinta](site-recovery-vmm-to-azure-powershell-resource-manager.md)
- [PowerShell-perinteinen](site-recovery-deploy-with-powershell.md)

Tervetuloa käyttämään Azure palauttaminen! Käytä tämän artikkelin, jos haluat toistaa paikallisen Hyper-V näennäiskoneiden hallitaan System Center Virtual Machine Manager (VMM) paveikslėlis, Azure käyttämällä Azure sivuston Azure-portaalissa.

> [AZURE.NOTE]Azure on kaksi eri [käyttöönoton mallien](../resource-manager-deployment-model
> ) luominen ja käyttäminen resurssit: Azure Resurssienhallinta ja perinteinen. Azure on myös kaksi portaaleihin – Azure perinteinen portaalin, joka tukee perinteinen käyttöönottomalli ja Azure-portaalin tukeen käyttöönoton sekä malleille.


Azure palauttaminen Azure-portaalissa on useita uusia ominaisuuksia:

- Azure-portaalissa Azure varmuuskopiointi ja palauttaminen Azure-palveluiden yhdistetään yhteen palautus palvelut-säilö, niin, että voit määrittää ja hallita liiketoiminnan jatkuvuus ja palauttaminen (BCDR) yhdestä paikasta. Yhdistetty Raporttinäkymät-ikkunan avulla voit valvoa ja toimintojen hallitsemaan paikallisen-sivustojen ja Azure julkisen pilveen.
- Käyttäjät, joilla on valmisteltu Cloud ratkaisu Provider (CSP)-ohjelmalla Azure tilaukset voivat nyt hallita sivuston palauttaminen toimintojen Azure-portaalissa.
- Sivuston palauttaminen Azure-portaalissa toistaa koneet Azure Resurssienhallinta tallennustilan tileihin. Sivuston palauttaminen luo automaattisesti, milloin Resurssienhallinta-pohjainen VMs Azure.
- Sivuston palauttaminen säilyy tue replikointia perinteinen tallennustilan tileihin. Sivuston palauttaminen luo automaattisesti, milloin VMs perinteinen mallin.


Luettuasi tämän artikkelin kirjaa kommentteja Disqus kommenttien alaosassa. Esitä kysymyksiä tekniset [Azure palautus Services keskustelupalstalle](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="overview"></a>Yleiskatsaus

Organisaatioiden on BCDR strategia, joka määrittää, miten sovellukset ja työmääriä pysy käynnissä ja käytettävissä aikana suunnitellut ja suunnittelemattomat käyttökatkot ja palauttaa normaalijakauman käytön edellytykset mahdollisimman pian. BCDR strategia kannattaa säilyttäminen yritystietojen turvassa ja palautettavissa ja varmistetaan, että työmääriä pysyvät jatkuvasti käytettävissä, kun tietojen tapahtuu.

Sivuston palautus on Azure-palvelu prosentuaalista osuutta BCDR strategia orchestrating replikointi paikallisen fyysiset palvelimet ja näennäiskoneiden pilveen (Azure) tai toissijainen palvelinkeskukseen. Kun katkokset ensisijainen sijainti-asiakas ei päälle toissijainen sijainnin vastaamaan itsellesi sovellukset ja toiminnoista, jotka ovat käytettävissä. Asiakas ei takaisin ensisijainen sijainti, kun se palauttaa normaalijakauman toimintoihin. Lisätietoja [Azure palauttaminen ominaisuudet?](site-recovery-overview.md)

Tässä artikkelissa on kaikki tiedot, jotka haluat kopioida paikallisen Hyper-V VMs VMM paveikslėlis, Azure. Se sisältää arkkitehtuuri yleiskatsaus, tiedot ja käyttöönoton vaiheet määrittämistä Azure, paikalliset palvelimet, Replikointiasetukset ja kapasiteetin suunnittelua varten. Kun olet määrittänyt infrastruktuuri voit ottaa replikoinnin tietokoneissa, jotka haluat suojata ja tarkista, että automaattisesti toimii.


## <a name="business-advantages"></a>Liiketoiminnan edut

- Sivuston palauttaminen suojaa sähköntuotannosta business toiminnoista ja Hyper-V VMs sovellusten.
- Palautus palvelut-portaalissa on yhden paikan määrittäminen, hallita ja seurata replikoinnin automaattisesti ja palauttaminen.
- Voit helposti suorittaa paikallisen infrastruktuurin failovers Azure ja Hyper-V host-palvelimissa azuren (Palauta) tuntisesta paikallisen sivuston.
- Voit määrittää useita koneet palautus suunnitelmien niin, että Porrastettu sovelluksen työmääriä epäonnistua yhdessä päälle.


## <a name="scenario-architecture"></a>Skenaario-arkkitehtuuri


Skenaario osat ovat seuraavat:

- **VMM palvelin**: paikallisen VMM-palvelimen kanssa vähintään yksi paveikslėlis.
- **Hyper-V isäntä- tai klusterin**: Hyper-V host-palvelimissa tai klustereiden hallitaan VMM paveikslėlis.
- **Azure sivuston palauttaminen tarjoaja ja palautus services agentti**: käyttöönoton aikana Azure sivuston palauttaminen Providerin asentaminen VMM palvelimessa ja Hyper-V host-palvelimissa Microsoft Azure palautus Services-agentti. Palvelun VMM palvelimessa yhteydessä palauttaminen HTTPS 443 replikointia tiedonsiirron päälle. Hyper-V Host (isäntä)-palvelimeen agentti kopioi tiedot Azure tallennustilan HTTPS 443 kautta oletusarvoisesti.
- **Azure**: tarvitset Azure tilauksen, replikoida myymälätietojen Azure-tallennustilan tilin ja Azure virtual verkon niin, että Azure VMs ovat yhteydessä verkkoon jälkeen automaattisesti.

![E2A topologian](./media/site-recovery-vmm-to-azure/architecture.png)


## <a name="azure-prerequisites"></a>Azure edellytykset

Seuraavassa on hyödyllisiä Azure-tietokannassa ottamaan Tämä skenaario.

**Edellytyksenä** | **Tiedot**
--- | ---
**Azure-tili**| Tarvitset [Microsoft Azure](http://azure.microsoft.com/) -tiliin. Voit aloittaa [maksuttoman kokeiluversion käyttäjäksi](https://azure.microsoft.com/pricing/free-trial/). [Lue lisää](https://azure.microsoft.com/pricing/details/site-recovery/) siitä, että sivuston palauttaminen hinnat.
**Azure-tallennustilan** | Tarvitset vakio Azure-tallennustilan tilin replikoitua tietojen tallentamiseen. Voit käyttää tallennustilan LRS tai GRS-tilin. Suosittelemme GRS niin, että tiedot ovat joustavat alueellisen käyttökatkosta sattuessa tai jos ensisijainen alue ei voi palauttaa. [Lue lisää](../storage/storage-redundancy.md). Tilin on oltava sama alue kuin palautus Services säilö.<br/><br/>Premium tallennustilan ei tueta.<br/><br/> Replikoitua tiedot tallennetaan Azure tallennustilan ja Azure VMs luodaan, kun vikasietotila ilmenee. <br/><br/> [Lue lisää](../storage/storage-introduction.md) Azure-tallennustilan.
**Azure verkossa** | Tarvitset Azure virtual verkko, jossa Azure VMs muodostaa automaattisesti yhteydessä. Verkossa on oltava sama alue kuin palautus Services säilö.

## <a name="on-premises-prerequisites"></a>Paikallisen edellytykset

Seuraavassa on tarvittavat paikallisen

**Edellytyksenä** | **Tiedot**
--- | ---
**VMM**| Yhden tai useamman VMM palvelimilla System Center 2012 R2: n. Kunkin VMM palvelin on määritetty vähintään yksi paveikslėlis. Pilvestä tulisi sisältää:<br/><br/> Yksi tai useampi VMM host ryhmä.<br/><br/> Hyper-V host-palvelimissa tai klustereiden host kunkin ryhmän.<br/><br/>[Lue lisää](http://www.server-log.com/blog/2011/8/26/vmm-2012-and-the-clouds.html) VMM paveikslėlis määrittämisestä.
**Hyper-V** | Hyper-V host-palvelimissa on oltava vähintään **Windows Server 2012 R2** ja Hyper-V-roolin tai **Microsoft Hyper-V Server 2012 R2** , muttet asentanut uusimmat päivitykset.<br/><br/> Hyper-V server on sisällettävä vähintään yksi VMs.<br/><br/> Klusterin, joka sisältää VMs, jonka haluat kopioida tai Hyper-V host-palvelin on hallittua VMM pilvestä.<br/><br/>Hyper-V palvelinten tulee olla yhdistetty yhteyden Internetiin, joko suoraan tai välityspalvelimen kautta.<br/><br/>Hyper-V-palvelimet on artikkelissa [2961977](https://support.microsoft.com/kb/2961977) asennettu mainitusta korjaukset.<br/><br/>Hyper-V host-palvelimissa on internet-yhteyttä Azure replikointi tiedot.
**Tarjoaja ja agentti** | Azure palauttaminen käyttöönoton aikana asentaa Azure sivuston palauttaminen tarjoajan VMM palvelimessa ja Hyper-V isännät palautus palvelut-agentti. Toimittaja- ja agent pitää liittyä Azure suoraan Internetin kautta tai välityspalvelimen kautta. Huomaa, että HTTPS-pohjainen välityspalvelimen ei tueta. Välityspalvelimen VMM server ja Hyper-V isännät olisi Salli pääsy: <br/><br/> *. hypervrecoverymanager.windowsazure.com <br/> <br/> *. accesscontrol.windows.net <br/><br/> *. backup.windowsazure.com <br/> <br/> *. blob.core.windows.net <br/><br/> *. store.core.windows.net<br/><br/>Jos sinulla on IP-osoite-pohjainen palomuurisäännöt VMM palvelimessa, tarkista sääntöjä Salli Azure tietoliikenne. Sinun on sallittava [Azure-palvelinkeskuksen IP-alueita](https://www.microsoft.com/download/confirmation.aspx?id=41653) ja HTTPS (443)-porttiin.<br/><br/>Salli IP-osoitealueet tilauksen Azure alue ja Länsi US.<br/><br/>Lisäksi. välityspalvelimen VMM palvelimessa on pääsy https://www.msftncsi.com/ncsi.txt


## <a name="protected-machine-prerequisites"></a>Suojattujen kone edellytykset


**Edellytyksenä** | **Tiedot**
--- | ---
**Suojatun VMs** | Ennen kuin epäonnistuu AM päälle, varmista, että nimi, joka on liitetty Azure AM noudattaa [Azure edellytykset](site-recovery-best-practices.md#azure-virtual-machine-requirements). Voit muokata nimeä, kun olet AM replikoinnin. <br/><br/> Yksittäisten levyn kapasiteetti suojatun tietokoneissa ei kannata olla yli 1023 Gigatavua. AM voi olla enintään 16 levyjen (näin enintään 16 TT).<br/><br/> Jaettu levyn Vieras klustereiden ei tueta.<br/><br/> Yhdistetty Extensible laitteisto Interface (UEFI) / Extensible laitteisto Interface(EFI) uudelleenkäynnistys ei tueta.<br/><br/> Jos lähde AM on NIC teaming se muunnetaan yhden NIC jälkeen Azure automaattisesti.<br/><br/>Suojaaminen virtuaalilaitteiksi Linux staattinen IP-osoite ei tueta.

## <a name="prepare-for-deployment"></a>Valmistele käyttöönottoa varten

Valmistele käyttöönoton, sinun on:

1. [Azure verkon määrittäminen](#set-up-an-azure-network) , jossa Azure VMs sijoitetaan automaattisesti jälkeen.
2. [Azure-tallennustilan tilin](#set-up-an-azure-storage-account) replikoitua tiedot.
4. [Valmistele VMM palvelimen](#prepare-the-vmm-server) palauttaminen käyttöönottoa varten.
5. [Valmistele verkon määritystä varten](#prepare-for-network-mapping). Määritä verkot, jotta voit määrittää verkon määritys palauttaminen käyttöönoton aikana.

### <a name="set-up-an-azure-network"></a>Azure-verkoston määrittäminen

Sinun on Azure verkko, niin, että Azure VMs luodaan jälkeen muodostaa automaattisesti.

- Verkossa on oltava sama alue kuin yksi, jossa käyttöönottoa palautus Services säilö.
- Haluat käyttää epäonnistui päälle Azure VMs resurssin mallista riippuen Määritä Azure verkon [Resurssienhallinta-tilassa](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) tai [perinteinen tila](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).
- Suosittelemme, että olet määrittänyt verkon ennen aloittamista. Jos et, sinun on suoritettava palauttaminen käyttöönoton aikana.

> [AZURE.NOTE] [Siirron verkostojen](../resource-group-move-resources.md) saman tilauksen piiriin kuuluvien resurssien ryhmien tai tilauksissa ei tue verkkojen käytettäviä sivustojen palauttamisen käyttöönotto.


### <a name="set-up-an-azure-storage-account"></a>Azure-tallennustilan tilin määrittäminen

- Sinun on vakio Azure tallennustilan-tili sisältää replikoida Azure tietoja. Tilin on oltava sama alue kuin palautus Services säilö.
- Haluat käyttää epäonnistui päälle Azure VMs resurssin mallista riippuen voit määrittää tilin [Resurssienhallinta-tilassa](../storage/storage-create-storage-account.md) tai [perinteinen tila](../storage/storage-create-storage-account-classic-portal.md).
- On suositeltavaa, määritä ensin tili ennen aloittamista. Jos et, sinun on suoritettava palauttaminen käyttöönoton aikana.

> [AZURE.NOTE] [Siirron tallennustilan tilien](../resource-group-move-resources.md) saman tilauksen piiriin kuuluvien resurssien ryhmien tai tilauksissa ei tue tallennustilan tilejä sivustojen palauttamisen käyttöönotto.

### <a name="prepare-the-vmm-server"></a>Valmistele VMM-palvelin

- Varmista, että [edellytykset](#on-premises-prerequisites)noudattaa VMM palvelimeen.
- Sivuston palauttaminen käyttöönoton aikana voit määrittää, että kaikki paveikslėlis VMM palvelimessa pitäisi olla käytettävissä Azure-portaalissa. Jos haluat vain tiettyjen paveikslėlis näkyvän portaalissa, voit ottaa VMM järjestelmänvalvojan konsolissa cloud-asetusta.


### <a name="prepare-for-network-mapping"></a>Valmistele verkon yhdistämistä varten

Sinun täytyy määrittää verkon määrityksen palauttaminen käyttöönoton aikana. Verkko-yhdistämismääritys yhdistää toisistaan lähteessä VMM AM verkkojen ja kohdistaa Azure verkkojen käyttöön seuraavasti:

- Koneet, jotka eivät läpäise päälle samassa verkossa muodostaa toisiinsa, vaikka ne olet ei epäonnistui päälle samalla tavalla tai vastaavan palautus-palvelupaketin.
- Jos yhdyskäytävien on määritetty aikataulussa Azure verkkoon, Azuren näennäiskoneiden muodostaa yhteyden paikallisen näennäiskoneiden.
- Verkon määrittäminen yhdistäminen seuraavassa on hyödyllisiä valmisteleminen:

    - Varmista, että VMM AM verkon yhteydessä VMs lähde Hyper-V Host (isäntä)-palvelimeen. Verkon olisi voidaan linkittää looginen verkko, joka on liitetty pilveen.
    - Azure verkon, kuten kuvattu [yllä](#set-up-an-azure-network)

- [Lisätietoja](site-recovery-network-mapping.md) verkon määritys toiminnasta.


## <a name="create-a-recovery-services-vault"></a>Luo palautus palvelut-säilö

1. Kirjautuminen [Azure portal](https://portal.azure.com).
2. Valitse **Uusi** > **hallinnan** > **palautus-palvelut**. Vaihtoehtoisesti voit valitsemalla **Selaa** > **Palautus Services** vaults > **Lisää**.

    ![Uusi säilö](./media/site-recovery-vmm-to-azure/new-vault3.png)

3. Määritä **nimi**-kutsumanimi tunnistavan säilö. Jos sinulla on useita tilauksia, valitse jonkin niistä.
4. [Luo resurssiryhmä](../resource-group-template-deploy-portal.md), tai valitse aiemmin luotu. Määritä Azure alue. Tällä alueella replikoida koneet. Tarkista tuettujen alueiden artikkelissa [Azure sivuston palauttaminen hinnat tiedot](https://azure.microsoft.com/pricing/details/site-recovery/) maantieteelliset käytettävyys
4. Jos haluat käyttää nopeasti koontinäytöstä säilö, valitse **Kiinnitä Raporttinäkymät-ikkunan** > **Luo säilö**.

    ![Uusi säilö](./media/site-recovery-vmm-to-azure/new-vault-settings.png)

Uusi säilö näkyy **raporttinäkymät-ikkunan** > **kaikille resursseille**, ja sisällön **palauttaminen Services vaults** sivu.

## <a name="getting-started"></a>Käytön aloittaminen

Sivuston palautus on aloittaminen-toiminto, jonka avulla voit ottaa käyttöön mahdollisimman pian. Aloittaminen tarkistaa edellytykset, sekä esitellään sivustojen palauttamisen käyttöönotto ohjeet ovat oikeassa järjestyksessä.

Voit valita tyyppi aloittaminen laitteiden, jonka haluat kopioida, ja mihin replikoida. Voit määrittää paikallisen palvelimissa, Azure tallennustilan tilit ja käyttäminen. Voit luoda replikoinnin käytännöt ja kapasiteetin suunnitteleminen. Kun olet määrittänyt infrastruktuurin otetaan käyttöön VMs replikoinnin. Voit suorittaa tiettyjä koneet failovers tai luoda palautus suunnitelmien epäonnistuu useiden laitteiden päälle.

Aloita valitsemalla ensin, kuinka haluat ottaa käyttöön palauttaminen aloitusopas. Aloittaminen-työnkulku muuttuu hieman replikoinnin tarpeen mukaan.



## <a name="step-1-choose-your-protection-goals"></a>Vaihe 1: Valitse suojausta tavoitteita

Valitse haluamasi replikoida ja mihin replikoida.

1. Valitse **palautus-palveluiden vaults** , sivu lisääminen säilöön ja valitse **asetukset**.
2. Valitse **aloittaminen** **Sivuston palauttaminen** > **Vaihe 1: valmisteleminen infrastruktuurin** > **Suojaus tavoite**.

    ![Valitse tavoitteiden](./media/site-recovery-vmm-to-azure/choose-goals.png)

3. Valitse **Suojaus-tavoite** **Azure,**ja valitse **Kyllä, jossa on Hyper-V**. Valitse **Kyllä** Vahvista käytät VMM Hyper-V isännät- ja palautus-sivuston hallinta. Valitse **OK**.

    ![Valitse tavoitteiden](./media/site-recovery-vmm-to-azure/choose-goals2.png)



## <a name="step-2-set-up-the-source-environment"></a>Vaihe 2: Lähde-ympäristön määrittäminen

Asenna Azure sivuston palauttaminen tarjoajan VMM-palvelimeen ja rekisteröidä palvelimen säilö. Asenna Azure palautus palvelut-agentti Hyper-V isännät.

1. Valitse **Vaihe 2: valmisteleminen infrastruktuurin** > **lähde**.

    ![Tietolähteen määrittäminen](./media/site-recovery-vmm-to-azure/set-source1.png)

2. Valitse **Valmistele lähde** **+ VMM** Lisää VMM palvelimen.

    ![Tietolähteen määrittäminen](./media/site-recovery-vmm-to-azure/set-source2.png)

3. **Lisää palvelin** sivu-valintaruutu, joka näkyy **System Center VMM palvelimen** **Palvelintyyppi** ja että VMM palvelimen täyttää sen [edellytykset ja URL-Osoitteen vaatimukset](#on-premises-prerequisites).
4. Lataa Azure sivuston palauttaminen tarjoajan asennustiedostoa.
5. Lataa rekisteröinti-näppäintä. Tämä on asennusohjelman. Viisi päivää, kun se luo avain on voimassa.

    ![Tietolähteen määrittäminen](./media/site-recovery-vmm-to-azure/set-source3.png)

6. Asenna Azure sivuston palauttaminen tarjoajan VMM-palvelimeen.


### <a name="set-up-the-azure-site-recovery-provider"></a>Azure sivuston palauttaminen tarjoajan määrittäminen

1.  Suorita palveluntarjoajan asennustiedosto.
2. **Microsoft Update** -voit valita päivitykset niin, että palvelun päivitykset on asennettu Microsoft Update-käytännön mukaisesti.
3. **Asennus**Hyväksy tai muokata palvelun asennuksen oletussijainti ja valitse **Asenna**.

    ![Asennuksen sijainti](./media/site-recovery-vmm-to-azure/provider2.png)

4. Kun asennus on valmis valitsemalla **Rekisteröi** VMM palvelimen rekisteröidään säilö.
5. Valitse **Säilö asetukset** -sivun säilö avaimen tiedosto valitsemalla **Selaa** . Määritä Azure palauttaminen tilaus ja säilöön-nimi.

    ![Palvelimen rekisteröinti](./media/site-recovery-vmm-to-azure/provider10.PNG)

6. Määritä **Internet-yhteys**, miten VMM palvelimessa tarjoaja muodostaa palauttaminen Internetin välityksellä.

    - Jos haluat muodostaa yhteyden suoraan palveluntarjoajan Valitse **Yhdistä suoraan Azure palauttaminen ilman välityspalvelinta**.
    - Jos aiemmin välityspalvelimen edellyttää käyttöoikeuden tarkistusta tai haluat käyttää mukautettu välityspalvelimen Valitse **Muodosta yhteys Azure palauttaminen välityspalvelimen kautta**.
    - Jos käytät mukautettua välityspalvelinta, Määritä osoite, portin ja tunnistetiedot.
    - Jos käytät välityspalvelinta olisi on jo sallittu [edellytykset](#on-premises-prerequisites)kuvattu URL-osoitteet.
    - Jos käytössäsi on mukautettu välityspalvelimen VMM RunAs-tili (DRAProxyAccount) luodaan automaattisesti määritetyn käyttäjätietojen avulla. Määritä välityspalvelin, niin, että tilin voi todentaa onnistuneesti. VMM RunAs Tiliasetukset voi muokata VMM konsolissa. **Asetukset**-kohdassa Laajenna **Suojaus** > **Suorittaa kuin tilit**ja muuta salasana DRAProxyAccount varten. Sinun on VMM-palvelun käynnistäminen uudelleen niin, että tämä asetus tulee voimaan.

    ![Internet](./media/site-recovery-vmm-to-azure/provider13.PNG)

7. Hyväksy tai muokata SSL-varmenteen automaattisesti luotava salauksen sijainti. Tämä varmenne on käytössä, jos otat suojattu Azure Azure palauttaminen portaalissa pilvestä salauksen. Lukemaan sertifikaatti. Kun suoritat automaattisesti Azure tarvitset sen salauksen, jos salauksen on käytössä.


8. Määritä **palvelimen nimi**-tunnistavan säilö VMM-palvelimen nimi. Määritä klusterimääritys-VMM klusterin roolinimi.
9. Ota käyttöön **synkronointi pilven metatietoja** , jos haluat synkronoida kaikki paveikslėlis VMM palvelimessa metatietojen säilö. Tämä toiminto on vain tapahtua kerran jokaisessa palvelimessa. Jos et halua synkronoida kaikki paveikslėlis, voit jättää tämän asetuksen valinta ja synkronoida kunkin cloud yksitellen VMM konsolissa cloud-ominaisuudet. Valitse **Rekisteröi** loppuun.

    ![Palvelimen rekisteröinti](./media/site-recovery-vmm-to-azure/provider16.PNG)

10. Rekisteröinti alkaa. Rekisteröinnin päätyttyä palvelimen näkyy **asetukset** > säilö-**palvelimet** -sivu.


#### <a name="command-line-installation-for-the-azure-site-recovery-provider"></a>Komentorivin asennuksen tarjoajan Azure sivuston palauttaminen

Azure sivuston palauttaminen tarjoajan voidaan asentaa komentoriviltä. Tämän menetelmän avulla voidaan Providerin asentaminen Server Core for Windows Server 2012 R2.

1. Lataa Provider-asennuksen tiedosto- ja rekisteröintitietojen avaimen kansioon. Esimerkiksi C:\ASR.
2. Suorita järjestelmänvalvojan oikeuksin suoritettava komentokehote näiden komentojen poimia palveluntarjoajan asennusohjelma:

            C:\Windows\System32> CD C:\ASR
            C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
3. Suorita tällä komennolla voit asentaa osat:

            C:\ASR> setupdr.exe /i

4. Suorittamalla nämä komennot rekisteröidä palvelimen säilö:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>       

Jos:

- **/Credentials**: pakollinen parametri, joka määrittää, jossa rekisteröinti avaimen tiedosto sijaitsee.  
- **/FriendlyName**: pakollinen parametri, joka näkyy Azure palauttaminen portaalissa Hyper-V Host (isäntä)-palvelimen nimi.
- - **/EncryptionEnabled**: valinnaisten parametrien, kun olet replikoiminen Hyper-V VMs-VMM paveikslėlis Azure avulla. Määritä, jos haluat salata näennäiskoneiden Azure-tietokannassa (ja muiden salaus). Varmista, tiedoston nimi on **.pfx** -tunniste. Salaus on oletusarvoisesti pois käytöstä.
- **/proxyAddress**: valinnainen parametri, joka määrittää välityspalvelimen osoitteen.
- **/proxyport**: valinnainen parametri, joka määrittää välityspalvelimen porttiin.
- **/proxyUsername**: valinnainen parametri, joka määrittää välityspalvelimen käyttäjänimi (Jos välityspalvelimen edellyttää käyttöoikeuden tarkistusta).
- **/proxyPassword**: valinnainen parametri, joka määrittää salasanan todentamismenetelmä välityspalvelinta (Jos välityspalvelin edellyttää todennusta).


### <a name="install-the-azure-recovery-services-agent-on-hyper-v-hosts"></a>Azure palautus palvelut-agentti asentaminen Hyper-V isännät

1. Kun olet määrittänyt palveluntarjoajan, sinun täytyy ladata asennustiedosto Azure palautus palvelut-agentti. Asennusohjelman Hyper-V kussakin palvelimessa VMM pilveen.

    ![Hyper-V sivustot](./media/site-recovery-vmm-to-azure/hyperv-agent1.png)

2. Valitse **Edellytykset Tarkista** -sivulla **Seuraava**. Mikä tahansa puuttuu edellytykset asennetaan automaattisesti.

    ![Edellytykset palautus palvelut-agentti](./media/site-recovery-vmm-to-azure/hyperv-agent2.png)

3. Valitse **Asennuksen asetukset** -sivun Hyväksy tai muokata asennuksen sijainti ja välimuistin sijaintia. Voit määrittää välimuistin asemaan, jossa on vähintään 5 gt tallennustilaa, mutta suosittelemme välimuisti-asema vähintään 600 gt vapaata tilaa. Valitse **Asenna**.
4. Kun asennus on valmis, valitse **Sulje** loppuun.

    ![Rekisteröi MARS agentti](./media/site-recovery-vmm-to-azure/hyperv-agent3.png)

#### <a name="command-line-installation-for-azure-site-recovery-services-agent"></a>Komentorivin asennuksen Azure sivuston palauttaminen palvelut-agentti

Voit asentaa Microsoft Azure palautus Services Agent komentoriviltä käyttämällä seuraava komento:

     marsagentinstaller.exe /q /nu

#### <a name="set-up-internet-proxy-access-to-site-recovery-from-hyper-v-hosts"></a>Määrittää välityspalvelimen internetyhteys Hyper-V isäntien palauttaminen

Palautus palvelut-agentti Hyper-V isännät-käyttöjärjestelmässä on internet-yhteyttä Azure AM replikoinnin. Jos käytät internet välityspalvelimen kautta, määrittämällä seuraavasti:

1. Avaa Microsoft Azure varmuuskopiointi MMC-laajennuksen Hyper-V isännän. Microsoft Azure varmuuskopiointi pikakuvake on oletusarvoisesti käytettävissä työpöydällä tai C:\Program Files\Microsoft Azure palautus Services Agent\bin\wabadmin.
2. Valitse laajennuksen **Ominaisuuksien muuttaminen**.
3. Määritä välityspalvelin palvelimen tiedot **Välityspalvelimen määritys** -välilehti.

    ![Rekisteröi MARS agentti](./media/site-recovery-vmm-to-azure/mars-proxy.png)

4. Varmista, että agentti saavuttaa [edellytykset](#on-premises-prerequisites)kuvattu URL-osoitteet.


## <a name="step-3-set-up-the-target-environment"></a>Vaihe 3: kohdeympäristön määrittäminen

Määritä Azure tallennustilan-tili, jota käytetään replikoinnin ja Azure verkkoon, johon Azure VMs muodostaa yhteyden automaattisesti jälkeen.

1.  Valitse **Valmistele infrastruktuurin** > **kohde** ja valitse Azure tilauksen, jota haluat käyttää.
2.  Määritä käyttöönoton mallin haluat käyttää VMs jälkeen automaattisesti.
3.  Sivuston palauttaminen tarkistaa, että sinulla on vähintään yksi yhteensopiva Azure tallennustilan asiakkaat ja verkkoja.

    ![Tallennustilan](./media/site-recovery-vmm-to-azure/compatible-storage.png)

4.  Jos et ole vielä luonut tallennustilan tilin ja haluat luoda resurssien hallinnan avulla napsauttamalla **+ -tallennustilan tilin** tekemään, että tekstiin.  Määritä **Luo tallennustilan tili** -sivu tilin nimi, tyyppi, tilaus ja sijainti. Tilin on oltava samassa sijainnissa kuin palautus Services säilö.

    ![Tallennustilan](./media/site-recovery-vmm-to-azure/gs-createstorage.png)

    Huomaa, että:

    - Jos haluat luoda perinteinen mallin tallennustilan tilin, tee se Azure-portaalissa. [Opi lisää](../storage/storage-create-storage-account-classic-portal.md)
    - Jos käytät premium-tallennustilan tilin replikoitua tietoja, joudut lisätallennustilaa vakio-tilin määrittäminen replikoinnin lokit, jotka siepata jatkuvaa muutokset paikalliset tiedot.

4.  Jos et ole vielä luonut Azure verkon ja haluat luoda resurssien hallinnan avulla valitsemalla **+ verkon** tekemään, että tekstiin. Määritä **Luo virtuaalisia verkko** -sivu verkkonimi, osoitealueita, aliverkon tiedot, tilaus ja sijainti. Verkossa on oltava samassa sijainnissa kuin palautus Services säilö.

    ![Verkon](./media/site-recovery-vmm-to-azure/gs-createnetwork.png)

    Jos haluat luoda verkon käyttäen perinteinen malli, joka tehdä Azure-portaalissa. [Lue lisää](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).

### <a name="configure-network-mapping"></a>Määritä verkon määritys

- [Lue](#prepare-for-network-mapping) tietoja verkon määritys pikaisesti ei. [Lue tämä](site-recovery-network-mapping.md) tarkemmin kuvauksen.
- Varmista, että näennäiskoneiden VMM palvelimessa on liitetty AM verkkoon ja että olet luonut ainakin yksi Azure virtual verkon. Useiden AM verkostojen voidaan määrittää yhden Azure verkoston.

Määritä yhdistäminen seuraavasti:

1. **Asetuksissa** > **Sivuston palauttaminen infrastruktuurin** > **verkon yhdistämismääritykset** > **Verkon yhdistäminen** **+ verkon yhdistäminen** -kuvaketta.

    ![Verkon määritys](./media/site-recovery-vmm-to-azure/network-mapping1.png)

2. Valitse **Lisää verkko-yhdistämismääritys** VMM lähdepalvelin ja **Azure** kohteena.
3. Tarkista tilaus ja käyttöönoton mallin jälkeen automaattisesti.
4. **Lähde-verkkoon**Valitse haluat yhdistää liittyvät VMM palvelin luettelosta lähde paikallisen AM verkko.
5. Valitse **kohde verkon**Azure verkon Azure VMs olla mitä replikan sijainti, kun ne on luotu. Valitse **OK**.

    ![Verkon määritys](./media/site-recovery-vmm-to-azure/network-mapping2.png)

Näin, mitä tapahtuu, kun verkon määritys alkaa:

- Olemassa olevan VMs lähde AM verkossa on yhdistetty kohde-verkkoon, kun yhdistäminen alkaa. Uusi VMs lähde AM verkkoyhteys on yhdistetty Azure yhdistetystä, kun Replikointi tapahtuu.
- Jos muokkaat aiemmin verkko-määritystä, replikan näennäiskoneiden yhdistetään käyttämällä uusia asetuksia.
- Jos jokin näistä aliverkosta on sama nimi kuin aliverkon, jossa lähde virtuaalikoneen on kohde verkossa on useita aliverkosta, valitse replikan virtuaalikoneen yhdistetään kyseisen kohteen aliverkon jälkeen automaattisesti.
- Jos ei ole kohteen aliverkon vastaavaa nimeä, virtuaalikoneen yhdistetään ensimmäisen aliverkon verkossa.



## <a name="step-4-set-up-replication-settings"></a>Vaihe 4: Määrittäminen Replikointiasetukset


1. Voit luoda uuden käytännön replikoinnin valitsemalla **Valmistele infrastruktuurin** > **Replikointiasetukset** > **+ luominen ja liitä**.

    ![Verkon](./media/site-recovery-vmm-to-azure/gs-replication.png)

2. **Luo ja associate käytännön**Määritä käytännön nimen.
3. **Kopioi korkojakso**Määritä kuinka usein haluat toistaa delta tietojen jälkeen ensimmäisen replikoinnin (30 sekunnin välein, 5-15 minuuttia).
4. **Palautus valitsemalla säilytys**Määritä tunteina keston säilytys-ikkuna on jokaiselle palautus kohdalle. Suojatun koneet voidaan palauttaa mihin tahansa kohtaan ikkunassa.
6. **Sovelluksen yhdenmukaisia tilannevedoksen korkojakso**Määritä tiheyden (1 – 12 tuntia) palauttaminen pistettä sisältävä sovelluksen yhdenmukaisia tilannevedoksia on luotu. Hyper-V käyttää kahdenlaisia tilannevedoksia, vakio tilannevedoksen, joka sisältää koko virtuaalikoneen vaiheittainen tilannevedoksen ja sovelluksen yhdenmukaisia tilannevedoksen, joka luo ajankohta tilannevedoksen sovelluksen tietojen virtuaalikoneen sisällä. Sovelluksen yhdenmukaisia tilannevedoksia varmistaa, että sovellukset ovat yhdenmukaisia tilaan tilannevedoksen otettaessa aseman varjostus kopioi Service (VSS) avulla. Huomaa, että jos otat sovelluksen yhdenmukaisia tilannevedoksia, se vaikuttaa lähteen näennäiskoneiden sovellusten suorituskykyä. Varmista, että olet määrittänyt arvo on pienempi kuin määrität palautuksen pisteiden lukumäärä.
3. **Ensimmäinen replikoinnin alkamisaika**Määritä aloittaminen ensimmäisen replikoinnin. Replikointi tapahtuu päälle internet-kaistanleveyden, niin voit ajoittaa varattu työtunnit ulkopuolella.
5. Määritä vai muiden tietojen Azuren tallennustilaan **Salaa Azure tallennetut tiedot**. Valitse **OK**.

    ![Replikoinnin käytäntö](./media/site-recovery-vmm-to-azure/gs-replication2.png)

6. Kun luot uuden käytännön se liittyy automaattisesti VMM pilveen. Valitse **OK**. Voit yhdistää muita VMM Paveikslėlis (ja niiden VMs) replikoinnin käytäntö **asetuksissa** > **replikoinnin** > käytännön nimi > **Liitä VMM pilveen**.

    ![Replikoinnin käytäntö](./media/site-recovery-vmm-to-azure/policy-associate.png)

## <a name="step-5-capacity-planning"></a>Vaihe 5: Kapasiteetin suunnittelu

Nyt kun olet luonut oman basic infrastruktuuri, voit määrittää voit ottaa huomioon kapasiteetin suunnittelu ja selvittää, tarvitsetko lisäresursseja.

Sivuston palautus on kapasiteetin suunnittelu auttaa oikean resurssien varaaminen lähde-ympäristössä, sivuston palauttaminen osista, verkko ja tallennustilaa. Voit suorittaa suunnittelija arviot keskimääräinen montako VMs levyille ja tallennustilaa pikatilassa tai yksityiskohtaisessa tilassa, jossa Lisää luvut työmäärää tasolla. Ennen aloittamista sinun on:

- Kerää tietoja replikoinnin ympäristöstä, kuten VMs levyjen VMs kohden ja tallennustilaa levyä kohden.
- Arvio päivittäin muuttaminen (churn) korko on replikoitua tiedoille. Voit [Hyper-V replikaan kapasiteetin suunnittelu](https://www.microsoft.com/download/details.aspx?id=39057) auttaa seuraavasti.

1.  Valitse Lataa työkalu ja suorita sitten **Lataa** . [Lue artikkeli](site-recovery-capacity-planner.md) mukana toimitettava työkalu.
2.  Kun olet valmis **olet suorittanut Capacity Planner**Valitse **Kyllä** ?

    ![Kapasiteetin suunnittelu](./media/site-recovery-vmm-to-azure/gs-capacity-planning.png)

### <a name="network-bandwidth-considerations"></a>Verkon kaistanleveyden huomioon otettavia seikkoja

Voit laskea replikoinnin (ensimmäinen replikointi ja sitten delta) tarvittavan kaistanleveyden kapasiteetin suunnittelutyökalua. Voit hallita määrää kaistanleveyden käytön replikoinnin sinulla on useita vaihtoehtoja:

- **Kaistanleveyden**: Hyper-V liikenteestä, joka kopioi toissijaisen sivuston käy läpi tietyn Hyper-V-isännän. Voit kaistanleveyden isäntäpalvelimen.
- **Säätää tehosteasetuksilla kaistanleveyden**: voivat vaikuttaa replikoinnin pari rekisteriavaimet käyttämällä kaistanleveyden.

#### <a name="throttle-bandwidth"></a>Kaistanleveyden

1. Avaa Microsoft Azure varmuuskopiointi MMC-laajennuksen Hyper-V Host (isäntä)-palvelimeen. Microsoft Azure varmuuskopiointi pikakuvake on oletusarvoisesti käytettävissä työpöydällä tai C:\Program Files\Microsoft Azure palautus Services Agent\bin\wabadmin.
2. Valitse laajennuksen **Ominaisuuksien muuttaminen**.
3. **Throttling** -välilehden **käyttöön Internetin kaistanleveyden käytön rajoittaminen backup toimille**, valitse Määritä työn rajat ja vapaa-aika tuntia. Kelvollista alueista on 512 Kbps 102 Mbps sekunnissa.

    ![Kaistanleveyden](./media/site-recovery-vmm-to-azure/throttle2.png)

Voit myös määrittää rajoittimen [Määrittäminen OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) cmdlet-komento. Tässä on esimerkki:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Määrittäminen OBMachineSetting NoThrottle** osoittaa, että ei ole rajoitus on tehty.


#### <a name="influence-network-bandwidth"></a>Vaikutus kaistanleveys

**UploadThreadsPerVM** rekisteriarvon ohjausobjektit, joita käytetään tiedonsiirto (alku- tai delta sallittuja) levyn viestiketjuissa siirtyminen määrä. Suurempi arvo kasvaa replikoinnin käytettäviä kaistanleveys. **DownloadThreadsPerVM** rekisteriarvon määrittää viestiketjuissa siirtyminen tuntisesta tiedonsiirron käytettyä määrän.

1. Siirry rekisterin **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.

    - Muokkaa arvoa **UploadThreadsPerVM** (tai Luo avain, jos sitä ei ole) voit käyttää levyn replikoinnin ohjausobjektin viestiketjuissa siirtyminen.
    - Muokkaa arvoa **DownloadThreadsPerVM** (tai Luo avain, jos sitä ei ole), ohjausobjektin viestiketjuissa siirtyminen tuntisesta tietoliikenteen azuren käytetään.
2. Oletusarvo on 4. Valitse "overprovisioned" verkko-nämä rekisteriavaimet olisi muutetaan oletusarvot. Enimmäismäärä on 32. Valvonta-liikenne paikalliseen optimointi arvo.

## <a name="step-6-enable-replication"></a>Vaihe 6: Ota käyttöön sallittuja

Ota nyt replikoinnin seuraavasti:

1. Valitse **Vaihe 2: replikoida sovelluksen** > **lähde**. Kun olet replikoinnin ensimmäistä kertaa voit napsauttaa **+ replikoida** muihin tietokoneisiin replikoinnin käyttöön säilöön.

    ![Replikoinnin ottaminen käyttöön](./media/site-recovery-vmm-to-azure/enable-replication1.png)

2. **Lähde** -sivu-> Valitse VMM-palvelin ja jossa Hyper-V-isäntien sijaitsevat pilvessä. Valitse **OK**.

    ![Replikoinnin ottaminen käyttöön](./media/site-recovery-vmm-to-azure/enable-replication-source.png)

3. Valitse tilaus, kirjaa automaattisesti käyttöönoton mallin ja käytät replikoitua tietojen tallennustilan tilin **kohde** .

    ![Replikoinnin ottaminen käyttöön](./media/site-recovery-vmm-to-azure/enable-replication-target.png)

4. Valitse tallennustilan-tili, jota haluat käyttää. Jos haluat käyttää eri tallennustilan tiliä kuin ne on voit voit [luoda](#set-up-an-azure-storage-account). Voit luoda tallennustilan Resurssienhallinta mallin tili valitsemalla **Luo uusi**. Jos haluat luoda perinteinen mallin tallennustilan tilin, voit tehdä kyseisen [Azure-portaalissa](../storage/storage-create-storage-account-classic-portal.md). Valitse **OK**.
5. Valitse Azure verkko- ja aliverkon, johon Azure VMs muodostaa yhteyden, kun ne on kehrätty jälkeen automaattisesti. Valitse **Määritä nyt valitun koneet,** verkon koskee suojautumista kaikissa-tietokoneissa valitset. Valitse **Määritä myöhemmin** kohti koneen Azure-verkosto. Jos haluat käyttää eri verkosta ne on voit voit [luoda](#set-up-an-azure-network). Voit luoda Resurssienhallinta mallin verkon valitsemalla **Luo uusi**. Jos haluat luoda perinteinen mallin avulla voit tehdä kyseisen [Azure-portaalissa](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). Valitse aliverkon tarvittaessa. Valitse **OK**.
6. **Näennäiskoneiden** > **näennäiskoneiden** Valitse ja valitse Jokaisessa koneessa haluat kopioida. Voit valita vain koneet, jossa on käytössä replikoinnin. Valitse **OK**.

    ![Replikoinnin ottaminen käyttöön](./media/site-recovery-vmm-to-azure/enable-replication5.png)

5. **Ominaisuuksien** > **Määritä ominaisuudet**, valitse käyttöjärjestelmä valitun VMs ja OS levy. Valitse **OK**. Voit määrittää muita ominaisuuksia myöhemmin.

    ![Replikoinnin ottaminen käyttöön](./media/site-recovery-vmm-to-azure/enable-replication6.png)


12. Valitse **Replikointiasetukset** > **replikoinnin asetusten määrittäminen**-haluat hakea suojatun VMs replikoinnin käytäntö. Valitse **OK**. Voit muokata replikoinnin käytännön **asetuksissa** > **replikoinnin käytännöt** > käytännön nimi > **Muokkaa asetuksia**. Haluat käyttää muutosten käytetään koneet, jotka ovat jo replikoiminen ja uusiin koneisiin.

    ![Replikoinnin ottaminen käyttöön](./media/site-recovery-vmm-to-azure/enable-replication7.png)

**Asetusten** **Ottaminen käyttöön suojaus** työn edistymistä voi seurata > **työt** > **palauttaminen työt**. Kun **Viimeisteleminen suojaus** suoritetaan tietokoneessa on valmiina automaattisesti.

### <a name="view-and-manage-vm-properties"></a>Tarkastella ja hallita AM ominaisuudet

On suositeltavaa, että tarkistat lähdetietokoneen ominaisuudet. Muista, että Azure AM nimen tulisi mukaisia [Azure virtuaalikoneen vaatimukset](site-recovery-best-practices.md#azure-virtual-machine-requirements).

1. Valitse **asetukset** > **Suojatut osat** > **Replikoida kohteet** > ja valitse tietokoneen tiedot näkyviin.

    ![Replikoinnin ottaminen käyttöön](./media/site-recovery-vmm-to-azure/vm-essentials.png)

2. Voit tarkastella **Ominaisuudet** AM tietoja replikoinnin ja automaattisesti.

    ![Replikoinnin ottaminen käyttöön](./media/site-recovery-vmm-to-azure/test-failover2.png)

3. **Laske**-ja verkon > **Laske ominaisuudet** , voit määrittää Azure AM nimi- ja kohdesivustojen kokoa. Muokkaa tarvittaessa [Azure](site-recovery-best-practices.md#azure-virtual-machine-requirements) vaatimusten mukainen nimi. Voit myös tarkastella ja muokata tietoja kohde verkko-, aliverkon, ja IP-osoitetta, joka on määritetty Azure AM. Ota seuraavat seikat huomioon:

    - Voit määrittää kohde-IP-osoite. Jos et anna osoitetta epäonnistunutta päälle koneen käyttää DHCP. Jos määrität osoitetta, joka ei ole käytettävissä osoitteessa automaattisesti, vikasietotilaa epäonnistuu. Sama kohde-IP-osoite voidaan testin automaattisesti, jos osoite ei ole käytettävissä testi automaattisesti verkossa.
    - Verkkosovittimien määrä määräytyy koon kohde-virtuaalikoneen määrittäminen seuraavasti:

        - Jos verkkosovittimien lähde-koneessa määrä on pienempi tai yhtä sovittimet sallittu kohde koneen kokoa, kohde on yhtä monta sovittimet lähde.
        - Jos lähde virtuaalikoneen sovittimet ylittää sallitun kohteen koon kohde enimmäiskoko käytetään.
        - Jos lähdetietokoneen on kaksi verkkosovittimien ja kohde koneen koon tukee neljä, kohdetietokoneen on kahden sovittimen esimerkiksi. Jos lähde on kahden sovittimen mutta tuetut kohteen koon tukee vain yksi kohdetietokoneen on vain yksi sovittimen.     
        - Jos AM on useita verkkosovittimien ne kaikki muodostaa yhteyden samassa verkossa.

    ![Replikoinnin ottaminen käyttöön](./media/site-recovery-vmm-to-azure/test-failover4.png)

5.  **Kohtauksen** näet käyttöjärjestelmän ja tietojen levyjä, joka voi replikoida AM.



## <a name="step-7-test-your-deployment"></a>Vaihe 7: Testata käyttöönoton

Voit testata käyttöönoton voit suorittaa testi-automaattisesti virtual yhteen tietokoneeseen tai palautus-palvelupaketti, joka sisältää vähintään yhden näennäiskoneiden.


### <a name="prepare-for-failover"></a>Automaattisesti valmisteleminen

- Suorita testi automaattisesti Suosittelemme, että luot uuden Azure verkkoon, joka on eristetty Azure tuotannon verkosta (tämä on oletustoiminnan kun luot uuden verkon Azure). [Lue lisää](site-recovery-failover.md#run-a-test-failover) testi failovers suorittamisesta.
- Saat parhaan suorituskyvyn, jos olet ette Azure päälle, asenna Azure-agentti suojatun tietokoneeseen. Tekee käynnistäminen nopeammin ja auttaa vianmäärityksessä. Asenna [Linux](https://github.com/Azure/WALinuxAgent) - tai [Windows](http://go.microsoft.com/fwlink/?LinkID=394789) -agentti.
- Voit testata täysin käyttöönoton tarvitset infrastruktuuri replikoitua koneen toimii odotetulla tavalla. Jos haluat testata Active Directory- ja DNS-voit luoda virtual machine kuin toimialueen ohjauskoneen DNS ja replikoida tämä Azure käyttämällä Azure palauttaminen. Lue lisää tekstimuodossa [testi automaattisesti Huomioitavaa Active Directorysta](site-recovery-active-directory.md#considerations-for-test-failover).
- Jos haluat suorittaa suunnittelematon automaattisesti, sen sijaan, että testi automaattisesti huomautuksen seuraavasti:

    - Olisi mahdollisuuksien Sammuta ensisijainen koneet, ennen kuin suoritat suunnittelematon automaattisesti. Näin varmistat, että sinulla ei ole lähde- ja replikan, joissa käytetään yhtä aikaa.
    - Kun suoritat suunnittelematon automaattisesti lopettaa tietojen replikoinnin suorittamisen ensisijainen koneet niin mitään tietoja delta ei siirretä, kun suunnittelematon automaattisesti alkaa. Lisäksi jos suoritat suunnittelematon automaattisesti palautus-palvelupaketin se suoritetaan, kunnes valmiina, vaikka näyttöön tulee virhesanoma.

### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Yhteyden muodostaminen Azure VMs jälkeen automaattisesti valmistelu

Jos haluat muodostaa yhteyden Azure VMs käyttämällä RDP automaattisesti sen jälkeen, varmista, että seuraavat toimet:

**Ennen automaattisesti paikalliseen tietokoneeseen**:

- Käytön Internetin välityksellä käyttöön RDP, varmista, **julkisen**lisätään TCP- ja UDP-säännöt ja varmistaa, että **Windowsin palomuuri**on sallittua RDP -> **sallitut sovellukset ja toiminnot** profiilien.
- Sivuston sivusto-yhteyden käytön käyttöön RDP tietokoneeseen ja varmistaa, että **Windowsin palomuuri**on sallittua RDP -> **sallitut sovellukset ja toiminnot** **toimialueen** ja **yksityiset** verkkojen.
- Asenna [Azure AM agentti](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) paikalliseen tietokoneeseen.
- Varmista, että käyttöjärjestelmän SAN käytäntö on määritetty OnlineAll. [Opi lisää]( https://support.microsoft.com/kb/3031135)
- Poistaa IP-palvelun käytöstä, ennen kuin suoritat vikasietotilaa.

**Valitse Azure AM automaattisesti jälkeen**:

- Lisää julkisen päätepiste RDP-protokolla (portti 3389) ja määritä kirjautumisen tunnistetiedot.
- Varmista toimialuekäytäntöjä, jotka estävät yhteyden julkisen osoitteen virtual tietokoneeseen ei ole.
- Yritä muodostaa yhteys. Jos et voi muodostaa, varmista, että AM on käynnissä. Lisää vianmääritysohjeita Lue tämän [artikkelin](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).

Jos haluat käyttää käynnissä Linux automaattisesti suojattu runko Client-asiakkaalla (ssh) jälkeen Azure-AM, toimi seuraavasti:

**Ennen automaattisesti paikalliseen tietokoneeseen**:

- Varmista, että Azure AM suojattu runko-palvelu on määritetty käynnistymään automaattisesti järjestelmän käynnistyksen yhteydessä.
- Tarkista, että palomuurisäännöt salli SSH-yhteyden siihen.

**Valitse Azure AM automaattisesti jälkeen**:

- Verkon suojaussäännöt-epäonnistui AM ja Azure aliverkon, johon se on kytketty on sallittava saapuvia yhteyksiä SSH-porttiin.
- Julkinen päätepisteen luodaanko sallimaan yhteyksiä SSH porttiin (TCP portti 22 oletusarvoisesti).
- Jos VPN-yhteyden (Express reitin tai sivuston-sivusto VPN) kautta AM asiakas voidaan suoraan yhteyden AM SSH päälle.


### <a name="run-a-test-failover"></a>Suorita testi automaattisesti

Suorita testi automaattisesti Älä seuraavasti:

1. Yksittäisen AM **asetuksissa**päälle epäonnistuu > **Replikoida kohteita**, valitsemalla AM > **+ testi automaattisesti**.
2. Epäonnistuu päälle palautus-palvelupaketti, **asetuksissa** > **Palautus-Palvelupaketit**, suunnitelman hiiren kakkospainikkeella > **Testi automaattisesti**. Luo palautuksen suunnitteleminen [seuraavien ohjeiden mukaisesti](site-recovery-create-recovery-plans.md).

3. Valitse **Testaa automaattisesti** , johon Azure VMs muodostaa, kun vikasietotila ilmenee Azure verkkoon.
4. Valitse **OK** , jos haluat aloittaa vikasietotilaa. Voit seurata edistymistä napsauttamalla sen ominaisuuksia AM tai **Testi automaattisesti** projektin **asetukset** > **palauttaminen työt**.
5. Kun vikasietotilaa **valmiina testaaminen** vaihe, toimi seuraavasti:

    1. Tarkastele replikan virtuaalikoneen Azure-portaalissa. Varmista, että virtuaalikoneen käynnistäminen onnistuu.
    2. Jos ole access näennäiskoneiden määrittää paikallisen verkosta voit aloittaa virtuaalikoneen etätyöpöydän yhteyden.
    3. Valitse Viimeistele se **valmiina testi** .
    4. Valitse **muistiinpanot** ja tallentaminen testi vikasietotilaa liittyvät huomautukset.
    5. Valitse **Testaa vikasietotilaa on valmis**. Puhdista testiympäristössä virtaa ja poista virtuaalikoneen testi automaattisesti.
    6. Tässä vaiheessa tahansa osien ja luo automaattisesti palauttaminen aikana testi vikasietotilaa VMs poistetaan. Testaa automaattisesti luomasi muita elementtejä ei poisteta.

    > [AZURE.NOTE] Jos testi automaattisesti edelleen kestää yli kaksi viikkoa on valmis voimassa mukaan.

6. Vikasietotilaa päätyttyä myös pitäisi näe se Azure tietokoneen näkyvän Azure portaalissa > **näennäiskoneiden**. Varmista että AM on haluamasi kokoinen, johon se on kytketty tarvittaessa verkon ja että se on käynnissä.
7. Jos olet [olemalla yhteydet automaattisesti jälkeen](#prepare-to-connect-to-Azure-VMs-after-failover) sinun pitäisi muodostaa Azure AM.


## <a name="monitor-your-deployment"></a>Käyttöönoton valvonta

Näin, miten voit seurata asetukset, tila ja terveyteen palauttaminen käyttöönottoa varten:

1. Valitse säilö nimi, joka käyttää **Essentials** Raporttinäkymät-ikkunan. Tämä koontinäyttö voit palauttaminen työt, replikoinnin tila, palautus suunnitelmat, palvelimen kunnon ja tapahtumia.  Voit mukauttaa Essentials ruudut ja-asetteluja, jotka ovat hyödyllisimpiä sinua, kuten muiden palauttaminen ja varmuuskopiointi vaults tilaa.

    ![Olennaiset asiat](./media/site-recovery-vmm-to-azure/essentials.png)

2. **Kunto** -ruudussa voit valvoa sivustoon palvelimet (VMM tai määritysten), joka on ilmennyt ongelma ja tapahtumia aiheutuneet palauttaminen viimeisen 24 tunnin aikana.
3. Voit hallita ja seurata replikointi **Replikoida kohteiden** **Palauttaminen-Palvelupaketit**, ja **Sivuston palauttaminen työt** ruutujen. Voit siirtyä työt **asetuksissa** -> **työt** -> **Sivuston palauttaminen työt**.


## <a name="next-steps"></a>Seuraavat vaiheet

Käyttöönoton jälkeen määrittämisen jälkeen ja käytössä, [Katso](site-recovery-failover.md) lisätietoja automaattisesti.

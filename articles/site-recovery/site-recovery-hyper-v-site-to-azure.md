<properties
    pageTitle="Toistaa Hyper-V näennäiskoneiden (ilman VMM) käyttämällä Azure palauttaminen Azure-portaalissa Azure | Microsoft Azure"
    description="Tässä artikkelissa kuvataan ottamisesta käyttöön, orchestrate replikoinnin, automaattisesti ja paikallisen Hyper-V VMs, jotka eivät ole hallitsee VMM Azure Azure-portaalissa palautus Azure palauttaminen"
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
    ms.date="09/19/2016"
    ms.author="raynew"/>


# <a name="replicate-hyper-v-virtual-machines-without-vmm-to-azure-using-azure-site-recovery-with-the-azure-portal"></a>Toistaa Hyper-V näennäiskoneiden (ilman VMM) Azure käyttämällä Azure palauttaminen Azure-portaalissa

> [AZURE.SELECTOR]
- [Azure Portal](site-recovery-hyper-v-site-to-azure.md)
- [Azure perinteinen](site-recovery-hyper-v-site-to-azure-classic.md)
- [PowerShell - resurssien hallinta](site-recovery-deploy-with-powershell-resource-manager.md)



Tervetuloa käyttämään Azure palauttaminen! Tässä artikkelissa, jos haluat toistaa paikallisen Hyper-V virtual koneet, **eivät ole** System Center näennäiskoneiden Manager (VMM) paveikslėlis Azure voit hallita käyttöä. Tässä artikkelissa käsitellään määrittäminen replikoinnin käyttämällä Azure sivuston Azure-portaalissa.

> [AZURE.NOTE] Azure on kaksi eri [käyttöönoton mallien](../resource-manager-deployment-model.md) luominen ja käyttäminen resurssit: Azure Resurssienhallinta ja perinteinen. Azure on myös kaksi portaaleihin – Azure perinteinen portaalin, joka tukee perinteinen käyttöönottomalli ja Azure-portaalin tukeen käyttöönoton sekä malleille.

> Azure palauttaminen tukee Hyper-V näennäiskoneiden Azure, siirto ja palauttaminen. Tässä artikkelissa kerrotut vaiheet koskevat samannimisen määritettäessä replikoinnin Azure palauttaminen tai siirtäminen VMs Azure

Azure palauttaminen Azure-portaalissa on useita uusia ominaisuuksia:

- Azure-tietokannassa portal, Azure varmuuskopiointi ja Azure palauttaminen palvelut yhdistetään yhteen palautus palvelut-säilö niin, että voit määrittää ja hallita liiketoiminnan jatkuvuus ja palauttaminen (BCDR) yhdestä paikasta. Yhdistetty Raporttinäkymät-ikkunan avulla seurata ja toimintojen hallitsemaan paikallisen-sivustojen ja Azure julkisen pilveen.
- Käyttäjät, joilla on valmisteltu Cloud ratkaisu Provider (CSP)-ohjelmalla Azure tilaukset voivat nyt hallita sivuston palauttaminen toimintojen Azure-portaalissa.
- Sivuston palauttaminen Azure-portaalissa toistaa koneet Resurssienhallinta tallennustilan tileihin. Sivuston palauttaminen luo automaattisesti, milloin Resurssienhallinta-pohjainen VMs Azure.
- Sivuston palauttaminen tukee replikoinnin perinteinen tallennustilan asiakkaat ja perinteinen käyttöönoton mallin VMs automaattisesti edelleen.


Tässä artikkelissa kirjaa jälkeen lukemista palautetta alareunassa Disqus kommentit-kohtaan. Esitä kysymyksiä tekniset [Azure palautus Services keskustelupalstalle](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="overview"></a>Yleiskatsaus


Organisaatioiden on BCDR strategia, joka määrittää, miten sovellukset ja työmääriä pysy käynnissä ja käytettävissä aikana suunnitellut ja suunnittelemattomat käyttökatkot ja palauttaa normaalijakauman käytön edellytykset mahdollisimman pian. Yrityksen BCDR määrittäminen säilyttää yritystietojen turvassa ja palautettavissa ja varmistaa, että työmääriä ovat jatkuvasti käytettävissä, kun tietojen tapahtuu.

Sivuston palautus on Azure-palvelu prosentuaalista osuutta BCDR strategia mukaan orchestrating replikoinnin paikallisen fyysiset palvelimet ja näennäiskoneiden pilveen (Azure) tai toissijainen palvelinkeskukseen. Kun katkokset ensisijainen sijainti, voit voi epäonnistua päälle toissijaisen sijainnin vastaamaan itsellesi sovellukset ja toiminnoista, jotka ovat käytettävissä. Takaisin ensisijainen sijainti saattaa epäonnistua, kun se palauttaa normaalijakauman toimintoihin. Lisätietoja [Azure palauttaminen ominaisuudet?](site-recovery-overview.md)

Tässä artikkelissa on kaikki tiedot, jotka haluat kopioida paikallisen Hyper-V virtual koneet, **eivät ole** System Center näennäiskoneiden Manager (VMM) paveikslėlis Azure voit hallita. Se sisältää arkkitehtuuri yleiskatsaus, tiedot ja käyttöönoton ohjeet määrittäminen paikallista palvelimissa, Azure, replikoinnin käytännön ja kapasiteetin suunnittelu. Kun olet määrittänyt infrastruktuurin käyttöön replikoinnin tietokoneissa, jonka haluat suojata ja Suorita testi automaattisesti, voit tarkistaa määritysten. Voit siirtää myös oman VMs Azure mukaan suorittamiseen suunnitellun automaattisesti ja sitten siirtyminen on valmis.

## <a name="business-advantages"></a>Liiketoiminnan edut

- Tarjoaa business toiminnoista ja Hyper-V näennäiskoneiden sovellusten sähköntuotannosta (Azure) automaattisesti.
- On yksittäinen palautus palvelut-konsolissa yksinkertainen määritys ja replikoinnin automaattisesti ja palautus prosesseista hallinta.
- Voit helposti failovers pitäminen paikallisen infrastruktuurin Azure ja virheiden back (Palauta) azuren paikallisen sivuston.
- Voit määrittää useita koneet palautus suunnitelmien niin, että Porrastettu sovelluksen työmääriä epäonnistua yhdessä päälle.

## <a name="scenario-architecture"></a>Skenaario-arkkitehtuuri

Skenaario osat ovat seuraavat:

- **Hyper-V isäntä- tai klusterin**: paikallisen Hyper-V host-palvelimissa tai klustereiden. Käynnissä VMs, jonka haluat suojata Hyper-V isännät kerätään looginen Hyper-V sivustojen palauttaminen käyttöönoton aikana.
- **Azure sivuston palauttaminen tarjoaja ja palautus services agentti**: käyttöönoton aikana asennat Azure sivuston palauttaminen tarjoaja ja Microsoft Azure palautus Services-agentti Hyper-V host-palvelimissa. Palvelun yhteydessä Azure palauttaminen HTTPS 443 replikointia tiedonsiirron päälle. Hyper-V Host (isäntä)-palvelimeen agentti kopioi tiedot Azure tallennustilan HTTPS 443 kautta oletusarvoisesti.
- **Azure**: tarvitset Azure tilauksen, replikoida myymälätietojen Azure-tallennustilan tilin ja Azure virtual verkon niin, että Azure VMs ovat yhteydessä verkkoon jälkeen automaattisesti.

![Hyper-V sivustoarkkitehtuuri](./media/site-recovery-hyper-v-site-to-azure/architecture.png)



## <a name="azure-prerequisites"></a>Azure edellytykset

Näin käytön edellytykset Azure-tietokannassa ottamaan Tämä skenaario.

**Edellytyksenä** | **Tiedot**
--- | ---
**Azure-tili**| Tarvitset [Microsoft Azure](http://azure.microsoft.com/) -tiliin. Voit aloittaa [maksuttoman kokeiluversion käyttäjäksi](https://azure.microsoft.com/pricing/free-trial/). [Lue lisää](https://azure.microsoft.com/pricing/details/site-recovery/) siitä, että sivuston palauttaminen hinnat.
**Azure-tallennustilan** | Sinun on vakio tallennustilan tilin. Voit käyttää tallennustilan LRS tai GRS-tilin. Suosittelemme GRS niin, että tiedot ovat joustavat alueellisen käyttökatkosta sattuessa tai jos ensisijainen alue ei voi palauttaa. [Lue lisää](../storage/storage-redundancy.md). Tilin on oltava sama alue kuin palautus Services säilö.<br/><br/> Premium tallennustilan ei tueta.<br/><br/> Replikoitua tiedot tallennetaan Azure tallennustilan ja Azure VMs luodaan, kun vikasietotila ilmenee.<br/><br/> [Lue lisää](../storage/storage-introduction.md) Azure-tallennustilan.
**Azure verkossa** | Tarvitset Azure virtual verkko, jossa Azure VMs muodostaa yhteyden, kun vikasietotila ilmenee. Azure virtual verkko on oltava sama alue kuin palautus Services säilö.

## <a name="on-premises-prerequisites"></a>Paikallisen edellytykset

Näin käytön edellytykset paikallisen.

**Edellytyksenä** | **Tiedot**
--- | ---
**Hyper-V** | Yhden tai useamman paikalliset palvelimessa on **Windows Server 2012 R2: n** uusimmat päivitykset ja Hyper-V rooli käytössä tai **Microsoft Hyper-V Server 2012 R2: n**kanssa.<br/><br/>Hyper-V-palvelin on sisällettävä vähintään yksi näennäiskoneiden.<br/><br/>Hyper-V palvelinten tulee olla yhdistetty yhteyden Internetiin, joko suoraan tai välityspalvelimen kautta.<br/><br/>Hyper-V-palvelimet on mainittu [KB2961977](https://support.microsoft.com/en-us/kb/2961977 "KB2961977") asennettu korjaukset.
**Tarjoaja ja agentti** | Azure palauttaminen käyttöönoton aikana asentaa Azure sivuston palauttaminen tarjoaja. Palvelun asentamista myös asentaa Azure palautus palvelut-agentti kunkin Hyper-V-palvelimessa on näennäiskoneiden, jonka haluat suojata. Sivuston palauttaminen säilöön Hyper-V-palvelimilta pitäisi olla sama versio tarjoaja ja agentti.<br/><br/>Palvelu on yhdistäminen Azure palauttaminen Internetin välityksellä. Liikenne voidaan lähettää suoraan tai välityspalvelimen kautta. Huomaa, että mukaan HTTPS-välityspalvelimen ei tueta. Välityspalvelimen olisi Salli pääsy: <br/><br/> *. hypervrecoverymanager.windowsazure.com <br/> <br/> *. accesscontrol.windows.net <br/><br/> *. backup.windowsazure.com <br/> <br/> *. blog.core.windows.net <br/><br/> *Store.Core.Windows.NET <br/><br/> https://www.msftncsi.com/ncsi.txt<br/><br/>Jos sinulla on IP-osoite-pohjainen palomuurisäännöt palvelimessa, tarkista sääntöjä Salli Azure tietoliikenne. Tarvitset sallimaan [Azure-palvelinkeskuksen IP-alueita](https://www.microsoft.com/download/confirmation.aspx?id=41653) ja HTTPS (443)-porttiin.<br/><br/>Salli IP-osoitealueet tilauksen Azure alue ja Länsi US.

## <a name="protected-machine-prerequisites"></a>Suojattujen kone edellytykset


**Edellytyksenä** | **Tiedot**
--- | ---
**Suojatun VMs** | Ennen kuin päälle AM epäonnistua tarvitset varmistaaksesi, että nimi, joka määritetään Azure AM noudattaa [Azure edellytykset](site-recovery-best-practices.md#azure-virtual-machine-requirements). Voit muokata nimeä, kun olet AM replikoinnin.<br/><br/> Yksittäisten levyn kapasiteetti suojatun tietokoneissa ei kannata olla yli 1023 Gigatavua. AM voi olla enintään 16 levyjen (näin enintään 16 TT).<br/><br/> Jaettu levyn Vieras klustereiden ei tueta.<br/><br/> Jos lähde AM on NIC teaming se muunnetaan yhden NIC jälkeen Azure automaattisesti.<br/><br/>Suojaaminen virtuaalilaitteiksi Linux staattinen IP-osoite ei tueta.

## <a name="prepare-for-deployment"></a>Valmistele käyttöönottoa varten

Valmistele käyttöönoton, sinun on:

1. [Azure verkon määrittäminen](#set-up-an-azure-network) Azure VMs olla missä sijaitsevat, kun ne on luotu automaattisesti jälkeen.
2. [Azure-tallennustilan tilin](#set-up-an-azure-storage-account) replikoitua tiedot.
3. [Valmistele Hyper-V isännät](#prepare-the-hyper-v-hosts) jotta he pääsevät vaadittavat URL-osoitteet.

### <a name="set-up-an-azure-network"></a>Azure-verkoston määrittäminen

Azure-verkoston määrittäminen. Tarvitset tätä niin, että Azure VMs luodaan jälkeen automaattisesti ovat yhteydessä verkkoon.

- Verkossa on oltava sama alue kuin yksi, joka käyttöön palautus Services säilö.
- Haluat käyttää epäonnistui päälle Azure VMs resurssin mallista riippuen Määritä Azure verkon [Resurssienhallinta-tilassa](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) tai [perinteinen tila](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).
- Suosittelemme, että olet määrittänyt verkon ennen aloittamista. Jos sinun on käsiteltävä palauttaminen käyttöönoton aikana.

> [AZURE.NOTE] [Siirron verkostojen](../resource-group-move-resources.md) saman tilauksen piiriin kuuluvien resurssien ryhmien tai tilauksissa ei tue verkkojen käytettäviä sivustojen palauttamisen käyttöönotto.

### <a name="set-up-an-azure-storage-account"></a>Azure-tallennustilan tilin määrittäminen

- Sinun on vakio Azure tallennustilan-tili sisältää replikoida Azure tietoja.
- Haluat käyttää epäonnistui päälle Azure VMs resurssin mallista riippuen määrität tiliä [Resurssienhallinta-tilassa](../storage/storage-create-storage-account.md) tai [perinteinen tila](../storage/storage-create-storage-account-classic-portal.md).
- On suositeltavaa, että määrität tallennustilan tilin ennen aloittamista. Jos sinun on käsiteltävä palauttaminen käyttöönoton aikana. Tilit on oltava samalla alueella kuin palautus Services säilö.

> [AZURE.NOTE] [Siirron tallennustilan tilien](../resource-group-move-resources.md) saman tilauksen piiriin kuuluvien resurssien ryhmien tai tilauksissa ei tue tallennustilan tilejä sivustojen palauttamisen käyttöönotto.

### <a name="prepare-the-hyper-v-hosts"></a>Valmistele Hyper-V-isäntien

- Varmista, että Hyper-V-isäntien noudattavat [edellytykset](#on-premises-prerequisites).

### <a name="create-a-recovery-services-vault"></a>Luo palautus palvelut-säilö

1. Kirjautuminen [Azure portal](https://portal.azure.com).
2. Valitse **Uusi** > **hallinnan** > **Varmuuskopiointi ja palauttaminen (OMS)**. Vaihtoehtoisesti voit valitsemalla **Selaa** > **Palautus Services** vaults > **Lisää**.

    ![Uusi säilö](./media/site-recovery-hyper-v-site-to-azure/new-vault3.png)

3. Määritä kutsumanimi tunnistavan säilö **nimi** . Jos sinulla on useita tilauksia, valitse jonkin niistä.
4. [Luo uusi resurssiryhmä](../resource-group-template-deploy-portal.md) tai valitse aiemmin luotu ja Määritä Azure alue. Tällä alueella replikoida koneet. Tarkista tuettujen alueiden artikkelissa [Azure sivuston palauttaminen hinnat tiedot](https://azure.microsoft.com/pricing/details/site-recovery/) maantieteelliset käytettävyys
4. Jos haluat käyttää nopeasti koontinäytöstä säilö napsauttamalla **raporttinäkymät-ikkunan kiinnittäminen** ja valitse sitten **Luo säilö**.

    ![Uusi säilö](./media/site-recovery-hyper-v-site-to-azure/new-vault-settings.png)

Uusi säilö näkyvät **raporttinäkymät-ikkunan** > **kaikille resursseille**, ja sisällön **palauttaminen Services vaults** sivu.

## <a name="getting-started"></a>Käytön aloittaminen

Sivuston palautus on aloittaminen-toiminto, jonka avulla voit ottaa käyttöön mahdollisimman pian. Aloittaminen tarkistaa edellytykset, sekä esitellään sivustojen palauttamisen käyttöönotto ohjeet ovat oikeassa järjestyksessä.

Voit valita tyyppi aloittaminen laitteiden, jonka haluat kopioida, ja mihin replikoida. Voit määrittää paikallisen palvelimissa, Azure tallennustilan tilit ja käyttäminen. Voit luoda replikoinnin käytännöt ja kapasiteetin suunnitteleminen. Kun olet määrittänyt infrastruktuurin otetaan käyttöön VMs replikoinnin. Voit suorittaa tiettyjä koneet failovers tai luoda palautus suunnitelmien epäonnistuu useiden laitteiden päälle.

Aloita valitsemalla ensin, kuinka haluat ottaa käyttöön palauttaminen aloitusopas. Aloittaminen-työnkulku muuttuu hieman replikoinnin tarpeen mukaan.



## <a name="step-1-choose-your-protection-goals"></a>Vaihe 1: Valitse suojausta tavoitteita

Valitse haluamasi replikoida ja mihin replikoida.

1. Valitse **palautus-palveluiden vaults** sivu lisääminen säilöön ja valitse **asetukset**.
2. **Asetuksissa** > **Aloittaminen** Valitse **Sivuston palauttaminen** > **Vaihe 1: valmisteleminen infrastruktuurin** > **Suojaus tavoite**.

    ![Valitse tavoitteiden](./media/site-recovery-hyper-v-site-to-azure/choose-goals.png)

3. Valitse **Suojaus-tavoite** **Azure,**ja valitse **Kyllä, jossa on Hyper-V**. Valitse Vahvista käytössä ei ole VMM **ei** . Valitse **OK**.

    ![Valitse tavoitteiden](./media/site-recovery-hyper-v-site-to-azure/choose-goals2.png)


## <a name="step-2-set-up-the-source-environment"></a>Vaihe 2: Lähde-ympäristön määrittäminen

Määritä Hyper-V-sivusto, asentaa Azure sivuston palauttaminen tarjoaja ja Azure palautus palvelut-agentti Hyper-V isännät ja rekisteröi isäntien säilö.


1. Valitse **Vaihe 2: valmisteleminen infrastruktuurin** > **lähde**. Voit lisätä uuden Hyper-V sivuston säilön Hyper-V isännät tai klustereiden, valitsemalla **+ Hyper-V-sivusto**.

    ![Tietolähteen määrittäminen](./media/site-recovery-hyper-v-site-to-azure/set-source1.png)

2. Määritä **Luo Hyper-V sivuston** sivu sivuston nimi. Valitse **OK**. Valitse juuri luomasi sivusto.

    ![Tietolähteen määrittäminen](./media/site-recovery-hyper-v-site-to-azure/set-source2.png)

3. Valitse **+ Hyper-V Server** palvelimen lisääminen sivustoon.
4. **Lisää**Serverissä > **Palvelintyyppi** Tarkista, että **Hyper-V server** näytetään. Varmista, että Hyper-V-palvelin, johon haluat lisätä [edellytykset](#on-premises-prerequisites) noudattaa ja on käyttää määritetyt URL-osoitteet.
4. Lataa Azure sivuston palauttaminen tarjoajan asennustiedostoa. Suoritat tämän tiedoston asentaminen sekä palvelun palautus palvelut-agentti kunkin Hyper-V isännän.
5. Lataa rekisteröinti-näppäintä. Tarvitset tätä asennusohjelman. Avain on voimassa viisi päivää loisi sen jälkeen.

    ![Tietolähteen määrittäminen](./media/site-recovery-hyper-v-site-to-azure/set-source3.png)

6. Suorita palveluntarjoajan asennustiedosto kunkin isännän Hyper-V sivustoon lisätään. Jos asennat Hyper-V-klusterin, suorita asennus klusterin kunkin solmun. Asentaminen ja rekisteröiminen kunkin Hyper-V klusterin solmujen varmistaa, että näennäiskoneiden pysyvät suojatun, vaikka ne siirtää solmujen välillä.

### <a name="install-the-provider-and-agent"></a>Asenna tarjoaja ja agentti

1. Suorita palveluntarjoajan asennustiedosto.
2. **Microsoft Update** -voit valita päivitykset niin, että palvelun päivitykset on asennettu Microsoft Update-käytännön mukaisesti.
3. **Asennuksen** Hyväksy tai muokata palvelun asennuksen oletussijainti ja valitse **Asenna**.
4. Valitse **Säilö asetukset** -sivun **Selaa** ja valitse säilöön avaimen tiedosto, jonka latasit. Määritä Azure sivuston palautus-tilaus, säilö nimi ja Hyper-V-sivustossa, johon Hyper-V-palvelin kuuluu.

    ![Palvelimen rekisteröinti](./media/site-recovery-hyper-v-site-to-azure/provider3.png)

5- **Välityspalvelimen asetukset** määrittää kuinka palvelu, joka asennetaan palvelimeen muodostetaan Azure palauttaminen Internetin välityksellä.

- Jos haluat muodostaa yhteyden suoraan palveluntarjoajan Valitse **Yhdistä suoraan ilman välityspalvelinta**.
- Jos haluat muodostaa yhteyden välityspalvelin, joka on tällä hetkellä määritetty palvelimeen valitsemalla **Yhdistä aiemmin välityspalvelimen asetusten kanssa**.
- Jos aiemmin välityspalvelimen edellyttää käyttöoikeuden tarkistusta tai haluat käyttää mukautettu välityspalvelimen tarjoajan yhteyden Valitse **Yhdistä mukautetun välityspalvelimen asetusten kanssa**.
- Jos käytössäsi on mukautettu välityspalvelimen, sinun on määritettävä osoite, portti ja tunnistetiedot
- Jos käytät välityspalvelinta luovan Varmista, että URL-osoitteet [edellytykset](#on-premises-prerequisites) kuvattu sallitaan kulkee.

    ![Internet](./media/site-recovery-hyper-v-site-to-azure/provider7.PNG)

6 kun asennus on valmis valitsemalla **Rekisteröi** rekisteröidä palvelimen säilö.

![Asennuksen sijainti](./media/site-recovery-hyper-v-site-to-azure/provider2.png)

7. päätyttyä rekisteröinti metatietojen Hyper-v server noutamien Azure palauttaminen ja palvelimen tulee näkyviin **asetukset** > **Sivuston palauttaminen infrastruktuurin** > **Hyper-V isännät** -sivu.


### <a name="command-line-installation"></a>Komentorivin asennus

Azure sivuston palauttaminen tarjoaja ja agentti voidaan asentaa myös käyttämällä seuraavaa komentoa. Tämän menetelmän avulla voidaan Providerin asentaminen Server Core for Windows Server 2012 R2.

1. Lataa Provider-asennuksen tiedosto- ja rekisteröintitietojen avaimen kansioon. Esimerkki C:\ASR.
2. Suorita järjestelmänvalvojan oikeuksin suoritettava komentokehote näiden komentojen poimia palveluntarjoajan asennusohjelma:

            C:\Windows\System32> CD C:\ASR
            C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
3. Suorita tällä komennolla voit asentaa osat:

            C:\ASR> setupdr.exe /i

4. Suorittamalla nämä komennot rekisteröidä palvelimen säilö: CD-levylle C:\Program Files\Microsoft Azure sivuston palauttaminen Provider\
            C:\Program Files\Microsoft Azure sivuston palauttaminen tarjoajan\> DRConfigurator.exe /r /Friendlyname <friendly name of the server> /tunnistetiedot<path of the credentials file>

<br/>
Jos:

- **/Credentials** : pakollinen parametri, joka määrittää sijainti, johon rekisteröinti avaimen tiedosto sijaitsee  
- **/FriendlyName** : pakollinen parametri, joka näkyy Azure palauttaminen portaalissa Hyper-V Host (isäntä)-palvelimen nimi.
- **/proxyAddress** : valinnainen parametri, joka määrittää välityspalvelimen osoitteen.
- **/proxyport** : valinnainen parametri, joka määrittää välityspalvelimen porttiin.
- **/proxyUsername** : valinnainen parametri, joka määrittää välityspalvelimen käyttäjänimi (Jos välityspalvelimen edellyttää käyttöoikeuden tarkistusta).
- **/proxyPassword** : valinnainen parametri, joka määrittää salasanan todennustapa välityspalvelimeen (Jos välityspalvelimen edellyttää käyttöoikeuden tarkistusta).


## <a name="step-3-set-up-the-target-environment"></a>Vaihe 3: kohdeympäristön määrittäminen

Määritä Azure tallennustilan-tili, jota käytetään replikoinnin ja Azure verkkoon, johon Azure VMs muodostaa yhteyden automaattisesti jälkeen.

1.  Valitse **Valmistele infrastruktuurin** > **kohde** ja valitse Azure tilauksen, jota haluat käyttää.
2.  Määritä käyttöönoton mallin haluat käyttää VMs jälkeen automaattisesti.
3.  Sivuston palauttaminen tarkistaa, että sinulla on vähintään yksi yhteensopiva Azure tallennustilan asiakkaat ja verkkoja.

    ![Tallennustilan](./media/site-recovery-hyper-v-site-to-azure/select-target.png)

4.  Jos et ole vielä luonut tallennustilan tilin ja haluat luoda resurssien hallinnan avulla valitsemalla **+ tallennustilan** tili, että tekstiin. Määritä **Luo tallennustilan tili** -sivu tilin nimi, tyyppi, tilaus ja sijainti. Tilin on oltava samassa sijainnissa kuin palautus Services säilö.

    ![Tallennustilan](./media/site-recovery-hyper-v-site-to-azure/gs-createstorage.png)

    Jos haluat luoda perinteinen mallin tallennustilan tilin Tee kyseisen [Azure-portaalissa](../storage/storage-create-storage-account-classic-portal.md).

5.  Jos et ole vielä luonut Azure verkon ja haluat luoda resurssien hallinnan avulla valitsemalla **+ verkon** tekemään, että tekstiin. Määritä **Luo virtuaalisia verkko** -sivu verkkonimi, osoitealueita, aliverkon tiedot, tilaus ja sijainti. Verkossa on oltava samassa sijainnissa kuin palautus Services säilö.

    ![Verkon](./media/site-recovery-hyper-v-site-to-azure/gs-createnetwork.png)

    Jos haluat luoda perinteinen mallin avulla voit tehdä kyseisen [Azure-portaalissa](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).


## <a name="step-4-set-up-replication-settings"></a>Vaihe 4: Määrittäminen Replikointiasetukset

1. Voit luoda uuden replikoinnin käytännön valitsemalla **Valmistele infrastruktuurin** > **Replikointiasetukset** > **+ luominen ja liitä**.

    ![Verkon](./media/site-recovery-hyper-v-site-to-azure/gs-replication.png)

2. **Luo** ja associate käytäntö määrittää käytännön nimen.
3. **Kopioi korkojakso** Määritä kuinka usein haluat toistaa delta tietojen jälkeen ensimmäisen replikoinnin (30 sekunnin välein, 5-15 minuuttia).
4. **Palautus valitsemalla säilytys**Määritä tunteina keston säilytys-ikkuna on jokaiselle palautus kohdalle. Suojatun koneet voidaan palauttaa mihin tahansa kohtaan ikkunassa.
6. **Sovelluksen yhdenmukaisia tilannevedoksen korkojakso** Määritä tiheyden (1 – 12 tuntia) palauttaminen pistettä sisältävä sovelluksen yhdenmukaisia tilannevedoksia on luotu. Hyper-V käyttää kahdenlaisia tilannevedoksia, vakio tilannevedoksen, joka sisältää koko virtuaalikoneen vaiheittainen tilannevedoksen ja sovelluksen yhdenmukaisia tilannevedoksen, joka luo ajankohta tilannevedoksen sovelluksen tietojen virtuaalikoneen sisällä. Sovelluksen yhdenmukaisia tilannevedoksia varmistaa, että sovellukset ovat yhdenmukaisia tilaan tilannevedoksen otettaessa aseman varjostus kopioi Service (VSS) avulla. Huomaa, että jos otat sovelluksen yhdenmukaisia tilannevedoksia, se vaikuttaa lähteen näennäiskoneiden sovellusten suorituskykyä. Varmista, että olet määrittänyt arvo on pienempi kuin määrität palautuksen pisteiden lukumäärä.
3. Määritä ensimmäisen replikoinnin aloittaminen **ensimmäinen replikoinnin aloitusaika** . Replikointi tapahtuu päälle internet-kaistanleveyden, niin voit ajoittaa varattu työtunnit ulkopuolella. Valitse **OK**.

    ![Replikoinnin käytäntö](./media/site-recovery-hyper-v-site-to-azure/gs-replication2.png)

Kun luot uuden käytännön se liittyy automaattisesti Hyper-V-sivustossa. Valitse **OK**. Voit yhdistää Hyper-V-sivusto (ja sen VMs) useita replikoinnin käytäntöjä **asetuksissa** > **replikoinnin** > käytännön nimi > **Liitä Hyper-V-sivustossa**.

## <a name="step-5-capacity-planning"></a>Vaihe 5: Kapasiteetin suunnittelu

Nyt kun olet luonut oman basic infrastruktuuri, voit määrittää voit ottaa huomioon kapasiteetin suunnittelu ja selvittää, tarvitsetko lisäresursseja.

Sivuston palautus on kapasiteetin suunnittelu auttaa oikean resurssien varaaminen lähde-ympäristössä, sivuston palauttaminen osista, verkko ja tallennustilaa. Voit suorittaa suunnittelija arviot keskimääräinen montako VMs levyille ja tallennustilaa pikatilassa tai yksityiskohtaisessa tilassa, jossa Lisää luvut työmäärää tasolla. Ennen aloittamista sinun on:

- Kerää tietoja replikoinnin ympäristöstä, kuten VMs levyjen VMs kohden ja tallennustilaa levyä kohden.
- Arvio päivittäin muuttaminen (churn) korko on replikoitua tiedoille. Voit [Hyper-V replikaan kapasiteetin suunnittelu](https://www.microsoft.com/download/details.aspx?id=39057) auttaa seuraavasti.

1.  Valitse Lataa työkalu ja suorita sitten **Lataa** . [Lue artikkeli](site-recovery-capacity-planner.md) mukana toimitettava työkalu.
2.  Kun olet valmis **olet suorittanut Capacity Planner**Valitse **Kyllä** ?

    ![Kapasiteetin suunnittelu](./media/site-recovery-hyper-v-site-to-azure/gs-capacity-planning.png)

### <a name="network-bandwidth-considerations"></a>Verkon kaistanleveyden huomioon otettavia seikkoja

Voit laskea replikoinnin (ensimmäinen replikointi ja sitten delta) tarvittavan kaistanleveyden kapasiteetin suunnittelutyökalua. Voit hallita määrää kaistanleveyden käytön replikoinnin sinulla on useita vaihtoehtoja:

- **Kaistanleveyden**: Hyper-V liikenteestä, joka kopioi Azure käy läpi tietyn Hyper-V-isännän. Voit kaistanleveyden isäntäpalvelimen.
- **Säätää tehosteasetuksilla kaistanleveyden**: voivat vaikuttaa replikoinnin pari rekisteriavaimet käyttämällä kaistanleveyden.

#### <a name="throttle-bandwidth"></a>Kaistanleveyden

1. Avaa Microsoft Azure varmuuskopiointi MMC-laajennuksen Hyper-V Host (isäntä)-palvelimeen. Microsoft Azure varmuuskopiointi pikakuvake on oletusarvoisesti käytettävissä työpöydällä tai C:\Program Files\Microsoft Azure palautus Services Agent\bin\wabadmin.
2. Valitse laajennuksen **Ominaisuuksien muuttaminen**.
3. **Throttling** -välilehden **käyttöön Internetin kaistanleveyden käytön rajoittaminen backup toimille**, valitse Määritä työn rajat ja vapaa-aika tuntia. Kelvollista alueista on 512 Kbps 102 Mbps sekunnissa.

    ![Kaistanleveyden](./media/site-recovery-hyper-v-site-to-azure/throttle2.png)

Voit myös määrittää rajoittimen [Määrittäminen OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) cmdlet-komento. Tässä on esimerkki:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Määrittäminen OBMachineSetting NoThrottle** osoittaa, että ei ole rajoitus on tehty.


#### <a name="influence-network-bandwidth"></a>Vaikutus kaistanleveys

1. Siirry rekisterin **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.
    - Rajoittamaan kaistanleveyden liikenne replicating levyllä arvo **UploadThreadsPerVM**Muokkaa tai Luo avain, jos sitä ei ole.
    - Rajoittamaan tuntisesta tietoliikenteen azuren kaistanleveyden muokata arvoa **DownloadThreadsPerVM**.
2. Oletusarvo on 4. Valitse "overprovisioned" verkko-nämä rekisteriavaimet olisi muutetaan oletusarvot. Enimmäismäärä on 32. Valvonta-liikenne paikalliseen optimointi arvo.

## <a name="step-6-enable-replication"></a>Vaihe 6: Ota käyttöön sallittuja

Ota nyt replikoinnin seuraavasti:

1. Valitse **Vaihe 2: replikoida sovelluksen** > **lähde**. Kun olet replikoinnin ensimmäistä kertaa Valitse **+ replikoida** muihin tietokoneisiin replikoinnin käyttöön säilöön.

    ![Replikoinnin ottaminen käyttöön](./media/site-recovery-hyper-v-site-to-azure/enable-replication.png)

2. **Lähde** -sivu-> Valitse Hyper-V-sivusto. Valitse **OK**.
3. Valitse **kohde** säilöön-tilausta ja haluat käyttää Azure-tietokannassa (perinteinen tai resurssi management) jälkeen automaattisesti automaattisesti malli.
4. Valitse tallennustilan-tili, jota haluat käyttää. Jos haluat käyttää eri tallennustilan tiliä kuin ne on voit voit [luoda](#set-up-an-azure-storage-account). Voit luoda tallennustilan Resurssienhallinta mallin tili valitsemalla **Luo uusi**. Jos haluat luoda perinteinen mallin tallennustilan tilin Tee kyseisen [Azure-portaalissa](../storage/storage-create-storage-account-classic-portal.md). Valitse **OK**.
5.  Valitse Azure verkko- ja aliverkon, johon Azure VMs muodostaa yhteyden, kun ne on kehrätty jälkeen automaattisesti. Valitse **Määritä nyt valitun koneet,** verkon koskee suojautumista kaikissa-tietokoneissa valitset. Valitse **Määritä myöhemmin** kohti koneen Azure-verkosto. Jos haluat käyttää eri verkosta ne on voit voit [luoda](#set-up-an-azure-network). Voit luoda Resurssienhallinta mallin verkon valitsemalla **Luo uusi**. Jos haluat luoda perinteinen mallin avulla voit tehdä kyseisen [Azure-portaalissa](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). Valitse aliverkon tarvittaessa. Valitse **OK**.

    ![Replikoinnin ottaminen käyttöön](./media/site-recovery-hyper-v-site-to-azure/enable-replication11.png)

6. **Näennäiskoneiden** > **näennäiskoneiden** Valitse ja valitse Jokaisessa koneessa haluat kopioida. Voit valita vain koneet, jossa on käytössä replikoinnin. Valitse **OK**.

    ![Replikoinnin ottaminen käyttöön](./media/site-recovery-hyper-v-site-to-azure/enable-replication5.png)

11. **Ominaisuuksien** > **Määritä ominaisuudet**, valitse käyttöjärjestelmä valitun VMs ja OS levy. Varmista, että [Azure virtuaalikoneen](site-recovery-best-practices.md#azure-virtual-machine-requirements) vaatimusten mukainen Azure AM name (kohdenimi) ja muokkaa sitä, jos haluat. Valitse **OK**. Voit määrittää muita ominaisuuksia myöhemmin.

    ![Replikoinnin ottaminen käyttöön](./media/site-recovery-hyper-v-site-to-azure/enable-replication6.png)

12. Valitse **Replikointiasetukset** > **replikoinnin asetusten määrittäminen**-haluat hakea suojatun VMs replikoinnin käytäntö. Valitse **OK**. Voit muokata replikoinnin käytännön **asetuksissa** > **replikoinnin käytännöt** > käytännön nimi > **Muokkaa asetuksia**. Haluat käyttää muutosten käytetään koneet, jotka ovat jo replikoiminen ja uusiin koneisiin.

    ![Replikoinnin ottaminen käyttöön](./media/site-recovery-hyper-v-site-to-azure/enable-replication7.png)

**Asetusten** **Ottaminen käyttöön suojaus** työn edistymistä voi seurata > **työt** > **palauttaminen työt**. Kun **Viimeisteleminen suojaus** suoritetaan tietokoneessa on valmiina automaattisesti.

### <a name="view-and-manage-vm-properties"></a>Tarkastella ja hallita AM ominaisuudet

On suositeltavaa, että tarkistat lähdetietokoneen ominaisuudet.

1. Valitse **asetukset** > **Suojatut osat** > **Replikoida kohteet** > ja valitse tietokone.

    ![Replikoinnin ottaminen käyttöön](./media/site-recovery-hyper-v-site-to-azure/test-failover1.png)

2. Voit tarkastella **Ominaisuudet** AM tietoja replikoinnin ja automaattisesti.

    ![Replikoinnin ottaminen käyttöön](./media/site-recovery-hyper-v-site-to-azure/test-failover2.png)

3. **Laske**-ja verkon > **Laske ominaisuudet** , voit määrittää Azure AM nimi- ja kohdesivustojen kokoa. Muokkaa tarvittaessa Azure vaatimusten mukainen nimi. Voit myös tarkastella ja muokata tietoja kohde verkon, aliverkon ja määritetään Azure AM IP-osoitetta. Ota seuraavat seikat huomioon:

    - Voit määrittää kohde-IP-osoite. Jos et anna osoitetta epäonnistunutta päälle koneen käyttää DHCP. Jos määrität osoitetta, joka ei ole käytettävissä osoitteessa automaattisesti, vikasietotilaa epäonnistuu. Sama kohde-IP-osoite voidaan testin automaattisesti, jos osoite ei ole käytettävissä testi automaattisesti verkossa.
    - Verkkosovittimien määrä määräytyy koon kohde-virtuaalikoneen määrittäminen seuraavasti:

        - Jos verkkosovittimien lähde-koneessa määrä on pienempi tai yhtä sovittimet sallittu kohde koneen kokoa, kohde on yhtä monta sovittimet lähde.
        - Jos lähde virtuaalikoneen sovittimet ylittää sallittu, Kohdekoko sitten kohde enimmäiskoko käytetään.
        - Jos lähdetietokoneen on kaksi verkkosovittimien ja kohde koneen koon tukee neljä, kohdetietokoneen on kahden sovittimen esimerkiksi. Jos lähde on kahden sovittimen mutta tuetut kohteen koon tukee vain yksi kohdetietokoneen on vain yksi sovittimen.     
        - Jos AM on useita verkkosovittimien ne kaikki muodostaa yhteyden samassa verkossa.

    ![Replikoinnin ottaminen käyttöön](./media/site-recovery-hyper-v-site-to-azure/test-failover4.png)

5.  **Kohtauksen** näet käyttöjärjestelmän ja tietojen levyjä, joka voi replikoida AM.


## <a name="step-7-test-the-deployment"></a>Vaihe 7: Käyttöönoton testaaminen

Voit testata käyttöönoton voit suorittaa testi-automaattisesti virtual yhteen tietokoneeseen tai palautus-palvelupaketti, joka sisältää vähintään yhden näennäiskoneiden.


### <a name="prepare-for-test-failover"></a>Testaa automaattisesti valmisteleminen

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

- Lisää sallimaan RDP Azure-AM liittyvät NIC julkiseen IP-osoite.
- Varmista toimialuekäytäntöjä, jotka estävät yhteyden julkisen osoitteen virtual tietokoneeseen ei ole.
- Yritä muodostaa yhteys. Jos et voi muodostaa Varmista, että AM on käynnissä. Lisää vianmääritysohjeita Lue tämän [artikkelin](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).

Jos haluat käyttää käynnissä Linux automaattisesti suojattu runko Client-asiakkaalla (ssh) jälkeen Azure-AM, toimi seuraavasti:

**Ennen automaattisesti paikalliseen tietokoneeseen**:

- Varmista, että Azure AM suojattu runko-palvelu on määritetty käynnistymään automaattisesti järjestelmän käynnistyksen yhteydessä.
- Tarkista, että palomuurisäännöt salli SSH-yhteyden siihen.

**Valitse Azure AM automaattisesti jälkeen**:

- Verkon suojaussäännöt-epäonnistui AM ja Azure aliverkon, johon se on kytketty on sallittava saapuvia yhteyksiä SSH-porttiin.
- Julkinen päätepisteen luodaanko sallimaan yhteyksiä SSH porttiin (TCP portti 22 oletusarvoisesti).
- Jos VPN-yhteyden (Express reitin tai sivuston sivuston VPN) kautta AM asiakas voidaan suoraan yhteyden AM SSH päälle.

### <a name="run-a-test-failover"></a>Suorita testi automaattisesti

Suorita testi automaattisesti Älä seuraavasti:

1. Yksittäisen AM **asetuksissa**päälle epäonnistuu > **Replikoida kohteita**, valitsemalla AM > **+ testi automaattisesti**.

    ![Testaa automaattisesti](./media/site-recovery-hyper-v-site-to-azure/run-failover1.png)

2. Epäonnistuu päälle palautus-palvelupaketti, **asetuksissa** > **Palautus-Palvelupaketit**, suunnitelman hiiren kakkospainikkeella > **Testi automaattisesti**. Luo palautuksen suunnitteleminen [seuraavien ohjeiden mukaisesti](site-recovery-create-recovery-plans.md).

3. Valitse **Testaa automaattisesti** , johon Azure VMs yhdistetään, kun vikasietotila ilmenee Azure verkkoon.

    ![Testaa automaattisesti](./media/site-recovery-hyper-v-site-to-azure/run-failover2.png)

4. Valitse **OK** , jos haluat aloittaa vikasietotilaa. Voit seurata edistymistä valitsemalla Avaa sen properies AM tai **Testi automaattisesti** projektin **asetukset** > **palauttaminen työt**.
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


## <a name="failover"></a>Automaattisesti

Kun ensimmäisen replikoinnin on valmis tietokoneesi, voit käynnistää failovers tarpeen vaatiessa. Sivuston palauttaminen tukee erilaisia failovers - testi automaattisesti, suunniteltu automaattisesti ja suunnittelematon automaattisesti.
[Lue lisää](site-recovery-failover.md) siitä erilaisia failovers ja yksityiskohtainen kuvaus milloin ja miten suorittavan kullekin niistä.

> [AZURE.NOTE] Jos tarkoituksen näennäiskoneiden siirtäminen Azure, suosittelemme, että näennäiskoneiden siirtäminen Azure [suunniteltu automaattisesti-toiminnon](site-recovery-failover.md#run-a-planned-failover-primary-to-secondary) avulla. Siirretyt sovelluksen tarkistetaan Azure käyttämällä Testaa automaattisesti, kun yhteyttä näennäiskoneiden siirtyminen on valmis kohdassa [Valmis siirron](#Complete-migration-of-your-virtual-machines-to-Azure) vaiheiden avulla. Sinun ei tarvitse suorittaa Vahvista tai poista. Valmis siirron siirto on valmis, poistaa virtuaalikoneen suojauksesta ja lopettaa koneen Azure palauttaminen laskutus.


###<a name="run-an-unplanned-failover"></a>Suorita suunnittelematon automaattisesti

Tämä on valittava kun ensisijainen sivuston vaihtuu ei ole käytettävissä odottamattomia tapahtuma, kuten käyttökatkosta tai virus hyökkäyksen vuoksi. Tässä kuvataan suorittamisesta "suunnittelematon automaattisesti" palautus-palvelupaketteihin. Vaihtoehtoisesti voit suorittaa yhteen virtual tietokoneeseen vikasietotilaa näennäiskoneiden-välilehdessä. Ennen kuin aloitat, varmista, että kaikki haluamasi epäonnistuu päälle näennäiskoneiden suorittanut ensimmäisen replikoinnin.

1. Valitse **palautus suunnitelmien > recoveryplan_name**.
2. Valitse palautus suunnittelu-sivu **Suunnittelematon automaattisesti**.
3. Valitse **suunnittelematon automaattisesti** -sivulla lähde- ja sijainnit.
4. Valitse **Sammuta näennäiskoneiden ja synkronoida uusimmat tiedot** voit määrittää, että sivuston palauttaminen yrittää suojatun näennäiskoneiden Sammuta ja synkronoida tiedot niin, että tiedot ovat uusimmat epäonnistuu päälle.
5. Vikasietotilaa, jälkeen näennäiskoneiden on Vahvista Odottaa-tilassa.  Valitse **Vahvista** vahvistusta vikasietotilaa.

[Opi lisää](site-recovery-failover.md#run-an-unplanned-failover)

## <a name="complete-migration-of-your-virtual-machines-to-azure"></a>Viimeistele Azure, että näennäiskoneiden siirto


>[AZURE.NOTE] Seuraavat vaiheet koskevat vain, jos olet siirtymässä näennäiskoneiden Azure

1. Suorittaa suunnitellun automaattisesti mainitaan [Tässä](site-recovery-failover.md)
2. Valitse **Asetukset > replikoida kohteet**, virtuaalikoneen hiiren kakkospainikkeella ja valitse **Valmis siirto**

    ![completemigration](./media/site-recovery-hyper-v-site-to-azure/migrate.png)

2. Valitse **OK** , jos haluat suorittaa siirron. Voit seurata edistymistä valitsemalla sen ominaisuuksia AM tai käyttämällä valmis siirtotyön- **Asetukset > sivuston palauttaminen työt**.


## <a name="monitor-your-deployment"></a>Käyttöönoton valvonta

Näin, miten voit seurata asetukset, tila ja terveyteen palauttaminen käyttöönottoa varten:

1. Valitse säilö nimi, joka käyttää **Essentials** Raporttinäkymät-ikkunan. Tämä koontinäyttö voit palauttaminen työt, replikoinnin tila, palautus suunnitelmat, palvelimen kunnon ja tapahtumia.  Voit mukauttaa Essentials ruudut ja-asetteluja, jotka ovat hyödyllisimpiä sinua, kuten muiden palauttaminen ja varmuuskopiointi vaults tilaa.

    ![Olennaiset asiat](./media/site-recovery-hyper-v-site-to-azure/essentials.png)

2. **Kunto** -ruudussa voit valvoa sivustoon palvelimet, joilla on ilmennyt ongelma ja tapahtumia aiheutuneet palauttaminen viimeisen 24 tunnin aikana.
3. Voit hallita ja seurata replikointi **Replikoida kohteiden** **Palauttaminen-Palvelupaketit**, ja **Sivuston palauttaminen työt** ruutujen. Voit siirtyä työt **asetuksissa** -> **työt** -> **Sivuston palauttaminen työt**.

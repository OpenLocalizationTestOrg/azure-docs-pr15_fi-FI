<properties
    pageTitle="Replikoida Hyper-V näennäiskoneiden-VMM paveikslėlis Azure | Microsoft Azure"
    description="Tässä artikkelissa käsitellään replikoida Hyper-V näennäiskoneiden Hyper-V isännät sijaitsee System Center VMM paveikslėlis Azure, valitse."
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
    ms.topic="hero-article"
    ms.date="05/06/2016"
    ms.author="raynew"/>

#  <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-azure"></a>Replikoida Azuren näennäiskoneiden Hyper-V-VMM paveikslėlis

> [AZURE.SELECTOR]
- [Azure Portal](site-recovery-vmm-to-azure.md)
- [PowerShell - resurssien hallinta](site-recovery-vmm-to-azure-powershell-resource-manager.md)
- [Perinteinen Portal](site-recovery-vmm-to-azure-classic.md)
- [PowerShell – perinteinen](site-recovery-deploy-with-powershell.md)



Azure palauttaminen-palvelun osaltaan mukaan orchestrating replikoinnin, automaattisesti ja näennäiskoneiden ja fyysiset palvelimet yrityksen liiketoiminnan jatkuvuuden ja tietojen palauttaminen (BCDR) määrittäminen. Koneet voi replikoida Azure tai toissijaisen paikallisen tietokeskuksen. Lue pikaisesti [Azure palauttaminen ominaisuudet?](site-recovery-overview.md).

## <a name="overview"></a>Yleiskatsaus

Tässä artikkelissa käsitellään palauttaminen replikointia Hyper-V näennäiskoneiden Hyper-V host-palvelimissa, jotka sijaitsevat VMM yksityinen paveikslėlis Azure voit ottaa käyttöön.

Artikkelin sisältää skenaarion edellytyksistä ja näytetään, miten määrittäminen sivuston palauttaminen säilöön, saaminen VMM lähdepalvelimeen asennettuihin Azure-sivuston palauttaminen tarjoajan, rekisteröidä palvelimen säilö, Azure-tallennustilan tilin lisääminen, asenna Azure palautus palvelut-agentti Hyper-V host-palvelimissa, VMM paveikslėlis, jota käytetään kaikissa suojatun näennäiskoneiden suojauksen asetusten määrittäminen , ja valitse Ota käyttöön kyseiset näennäiskoneiden suojaus. Valmis testaamalla vikasietotilaa varmistaaksesi, että kaikki toimii odotetulla tavalla.

Kirjaa kommentteja tai kysymyksiä alaosassa on tämän artikkelin tai [Azure palautus Services keskustelupalstalle](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="architecture"></a>Arkkitehtuuri

![Arkkitehtuuri](./media/site-recovery-vmm-to-azure-classic/topology.png)

- Azure sivuston palauttaminen tarjoaja on asennettu VMM palauttaminen käyttöönoton aikana ja VMM-palvelin on rekisteröity palauttaminen säilöön. Palvelun yhteydessä käsittelemään replikoinnin tiedonsiirron palauttaminen.
- Azure palautus palvelut-agentti asennettu Hyper-V host-palvelimissa palauttaminen käyttöönoton aikana. Se käsittelee tietojen replikoinnin Azure-tallennustilan.


## <a name="azure-prerequisites"></a>Azure edellytykset

Seuraavassa on käytön edellytykset Azure-tietokannassa.

**Edellytyksenä** | **Tiedot**
--- | ---
**Azure-tili**| Tarvitset [Microsoft Azure](https://azure.microsoft.com/) -tiliin. Voit aloittaa [maksuttoman kokeiluversion käyttäjäksi](https://azure.microsoft.com/pricing/free-trial/). [Lue lisää](https://azure.microsoft.com/pricing/details/site-recovery/) siitä, että sivuston palauttaminen hinnat.
**Azure-tallennustilan** | Tarvitset Azure-tallennustilan tilin replikoitua tietojen tallentamiseen. Replikoitua tiedot tallennetaan Azure tallennustilan ja Azure VMs kehrätty määrittäminen, kun vikasietotila ilmenee. <br/><br/>Sinun on [Vakio geo tarpeettomat tallennustilan tilin](../storage/storage-redundancy.md#geo-redundant-storage). Tilin palauttaminen palveluna samalla alueella on ja saman tilauksen yhdistetä. Huomaa, että replikoinnin premium-tallennustilan tilin ei tällä hetkellä tueta ja ei kannata käyttää.<br/><br/>[Lue lisää](../storage/storage-introduction.md) Azure-tallennustilan.
**Azure verkossa** | Tarvitset Azure virtual verkko, jossa Azure VMs muodostaa yhteyden, kun vikasietotila ilmenee. Azure virtual verkko on oltava sama kuin sivuston palauttaminen säilö alue.

## <a name="on-premises-prerequisites"></a>Paikallisen edellytykset

Näin käytön edellytykset paikallisen.

**Edellytyksenä** | **Tiedot**
--- | ---
**VMM** | Tarvitset vähintään yksi VMM palvelimen fyysinen tai näennäinen yksittäiseen palvelimeen tai virtual klusterin käyttöön. <br/><br/>VMM palvelimen suoritetaan System Center 2012 R2: n kumulatiiviset päivitykset kanssa.<br/><br/>Tarvitset vähintään yksi cloud määritettynä VMM-palvelimessa.<br/><br/>Tietolähteen pilveen, jonka haluat suojata on sisällettävä vähintään yksi VMM host ryhmät.<br/><br/>Lisätietoja määrittäminen VMM paveikslėlis [vaiheittaisissa ohjeissa: luominen yksityinen paveikslėlis System Center 2012 SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx) Keijo Mayer blogista.
**Hyper-V** | Tarvitset Hyper-V host-palvelimissa tai klustereiden VMM pilveen. Isäntäpalvelimen pitäisi olla ja vähintään yhden VMs. <br/><br/>Hyper-V-palvelin on oltava vähintään **Windows Server 2012 R2** ja Hyper-V rooli tai **Microsoft Hyper-V Server 2012 R2** , muttet asentanut uusimmat päivitykset.<br/><br/>Mikä tahansa Hyper-V-palvelimeen, jossa haluat suojata VMs on sijaittava VMM pilvestä.<br/><br/>Jos käytössäsi on Hyper-V-klusterin muistiinpanon kyseisen klusterin broker ei ole luodaan automaattisesti, jos sinulla on staattinen IP osoite-pohjainen klusterin. Sinun on määrittää klusterin broker manuaalisesti. [Lue lisää](https://www.petri.com/use-hyper-v-replica-broker-prepare-host-clusters) Aidan Finn blogimerkinnän.
**Suojatun koneet** | Suojattava VMs olisi [Azure vaatimusten](site-recovery-best-practices.md#azure-virtual-machine-requirements)mukainen.


## <a name="network-mapping-prerequisites"></a>Verkon määritys edellytykset
Kun suojaat näennäiskoneiden Azure verkon määritys karttoja AM-verkkojen VMM lähdepalvelimeen välillä ja kohdeluettelo Azure verkkojen käyttöön seuraavasti:

- Kaikissa tietokoneissa, mitkä automaattisesti samassa verkossa muodostaa yhteyden toisistaan riippumatta palautus palvelupaketin ne sijaitsevat.
- Jos yhdyskäytävien on aikataulussa Azure verkon määritys, näennäiskoneiden muodostaa paikallisen muiden näennäiskoneiden.
- Jos et määritä verkon määritys vain näennäiskoneiden, jotka eivät läpäise päälle saman palautus-suunnitelmassa voivat muodostaa yhteys toisiinsa jälkeen Azure automaattisesti.

Jos haluat ottaa käyttöön verkon määritys sinun on seuraavasti:

- Tietolähteen VMM palvelimessa suojattava näennäiskoneiden tulee olla yhdistetty AM verkkoon. Verkon olisi voidaan linkittää looginen verkko, joka on liitetty pilveen.
- Azure verkko, johon replikoitua näennäiskoneiden muodostaa automaattisesti jälkeen. Voit valita verkoston automaattisesti aikaan. Verkossa on oltava sama alue kuin Azure palauttaminen tilauksen.

Valmistaudu verkon yhdistäminen seuraavasti:

1. [Lue lisää](site-recovery-network-mapping.md) verkon yhdistämismäärityksiä koskevat vaatimukset.
2. Valmistele AM verkot VMM:

    - [Loogisen verkkojen määrittäminen](https://technet.microsoft.com/library/jj721568.aspx).
    - [AM verkkojen määrittäminen](https://technet.microsoft.com/library/jj721575.aspx).


## <a name="step-1-create-a-site-recovery-vault"></a>Vaihe 1: Luo sivuston palautus-säilö

1. Kirjaudu [Hallinta-portaalin](https://portal.azure.com) haluat rekisteröidä VMM palvelimesta.
2. Valitse **tietopalvelut** > **palautus Services** > **sivuston palauttaminen säilö**.
3. Valitse **Luo uusi** > **nopea luominen**.
4. Kirjoita **nimi**-kenttään kutsumanimi tunnistavan säilö.
5. Valitse **alue**-säilö maantieteellinen alue. Tarkista tuettujen alueiden artikkelissa maantieteelliset käytettävyys [Azure sivuston palauttaminen hinnat tiedot](https://azure.microsoft.com/pricing/details/site-recovery/).
6. Valitse **Luo säilö**.

    ![Uusi säilö](./media/site-recovery-vmm-to-azure-classic/create-vault.png)

Valitse Vahvista, että säilö on luotu tilarivillä. Säilö merkitään **aktiiviseksi** pääsivulta palautus-palvelut.

## <a name="step-2-generate-a-vault-registration-key"></a>Vaihe 2: Luo säilöön rekisteröinti avain

Luo säilö rekisteröinti-näppäintä. Kun olet ladannut Azure sivuston palauttaminen tarjoaja ja asenna se VMM-palvelimeen, käytät tätä näppäintä voit rekisteröidä VMM palvelimen säilö.

1. Valitse Pika-aloitus-sivun avaamiseen säilö **Palautus palveluita** -sivulle. Pikaopas voi avata myös milloin tahansa napsauttamalla kuvaketta.

    ![Pika-aloitusopas-kuvake](./media/site-recovery-vmm-to-azure-classic/qs-icon.png)

2. Valitse avattavasta luettelosta **paikallisen-VMM sivuston ja Microsoft Azure välillä**.
3. **Valmistele VMM palvelimet**Valitse **Luo rekisteröinti avaimen** tiedosto. Avaimen tiedosto luodaan automaattisesti ja 5 päivää sen luonnin jälkeen on voimassa. Jos olet ei käytä Azure portaalin VMM-palvelimesta, sinun on tämän tiedoston kopioiminen palvelimeen.

    ![Rekisteröinti avain](./media/site-recovery-vmm-to-azure-classic/register-key.png)

## <a name="step-3-install-the-azure-site-recovery-provider"></a>Vaihe 3: Azure sivuston palauttaminen Providerin asentaminen

1. **Pikaoppaassa** > **valmisteleminen VMM palvelimissa**, valitse **Lataa Microsoft Azure sivuston palauttaminen tarjoajan asennuksen VMM palvelimissa** hankkiminen tarjoajan asennustiedosto uusimman version.
2. Suorita tiedosto jossakin VMM lähdepalvelimeen.

    >[AZURE.NOTE] Jos VMM on otettu käyttöön klusterin ja asennat palvelua ensimmäistä kertaa asentaminen aktiivisen solmun ja viimeistele asennus VMM palvelimen rekisteröidään säilö. Valitse Providerin asentaminen muissa solmuissa. Huomaa, että jos päivität palveluntarjoajan sinun on päivitettävä kaikissa solmuissa, koska ne kaikki suoritetaan samalla palveluntarjoajan versio.

3. Asennusohjelma ei prerequirements-valintaruutu ja pyytää oikeutta Aloita tarjoajan asennus VMM-palvelun pysäyttäminen. VMM-palvelu käynnistetään automaattisesti, kun asennus on valmis. Jos asennat VMM-klusterin, sinua pyydetään lopettavat klusterin rooli.

4. **Microsoft Update** -voit valita päivitykset. Kun tämä asetus on käytössä tarjoajan päivitykset asennetaan Microsoft Update-käytännön mukaan.

    ![Microsoft-päivitykset](./media/site-recovery-vmm-to-azure-classic/updates.png)


5.  Asennuksen sijainnin tarjoajan on määritetty ** <SystemDrive>Files\Microsoft System Center 2012 R2\Virtual tietokoneen Manager\bin**. Valitse **Asenna**.

    ![InstallLocation](./media/site-recovery-vmm-to-azure-classic/install-location.png)

6. Palvelun asentamisen jälkeen valitsemalla **Rekisteröi** rekisteröidä palvelimen säilö.

    ![InstallComplete](./media/site-recovery-vmm-to-azure-classic/install-complete.png)

9. Tarkista nimi, johon palvelimen rekisteröity säilö **säilö nimi**. Valitse *Seuraava*.

    ![Palvelimen rekisteröinti](./media/site-recovery-vmm-to-azure-classic/vaultcred.PNG)

7. Määritä **Internet-yhteys** miten VMM palvelimessa palvelu on yhteydessä Internetiin. Valitse **Yhdistä aiemmin välityspalvelimen asetusten kanssa** käytettävä oletusarvoinen Internet-yhteyden asetukset määritetty palvelimeen.

    ![Internet-asetukset](./media/site-recovery-vmm-to-azure-classic/proxydetails.PNG)

    - Jos haluat käyttää mukautettua välityspalvelimen olisi määritystä ennen Providerin asentaminen. Kun mukautettu välityspalvelimen asetusten määrittäminen testi toimii tarkistamaan yhteyden välityspalvelimen kautta.
    - Jos käytössäsi on mukautettu välityspalvelimen tai oletusarvo-välityspalvelimen edellyttää käyttöoikeuden tarkistusta, sinun on Anna välityspalvelimen tiedot, mukaan lukien välityspalvelimen osoite ja portin.
    - Seuraavien URL-osoitteiden on oltava käytettävissä olevat VMM palvelimeen ja Hyper-v-isäntien
        - *. hypervrecoverymanager.windowsazure.com
        - *. accesscontrol.windows.net
        - *. backup.windowsazure.com
        - *. blob.core.windows.net
        - *. store.core.windows.net
    - Salli IP-osoitteiden kuvattu [Azure-palvelinkeskuksen IP-alueita](https://www.microsoft.com/download/confirmation.aspx?id=41653) ja HTTPS (443)-protokollaa. Sinun on valkoinen luettelo IP-alueita Azure alueen, jota aiot käyttää ja että Länsi US.
    - Jos käytössäsi on mukautettu välityspalvelimen VMM RunAs-tili (DRAProxyAccount) luodaan automaattisesti määritetyn käyttäjätietojen avulla. Määritä välityspalvelin, niin, että tilin voi todentaa onnistuneesti. VMM RunAs Tiliasetukset voi muokata VMM konsolissa. Voit tehdä tämän Avaa **asetukset** -työtila, laajenna **Suojaus**, **Suorita kuin**tilit ja muuta salasana DRAProxyAccount varten. Sinun on VMM-palvelun käynnistäminen uudelleen niin, että tämä asetus tulee voimaan.


8. **Rekisteröinti-näppäintä**Valitse avain Azure palauttaminen ladataan ja kopioi VMM palvelimeen.


10.  Salausasetusta käytetään vain, kun olet replikoiminen Hyper-V VMs-VMM paveikslėlis, Azure. Jos olet replikoiminen toissijaisen sivustoon se ei ole käytössä.

11.  Määritä **palvelimen nimi**-tunnistavan säilö VMM-palvelimen nimi. Määritä klusterin kokoonpano VMM klusterin roolinimi.
12.  **Synkronoi cloud metatietojen** Valitse, haluatko VMM palvelimessa kaikki paveikslėlis metatietojen synkronointi säilö. Tämä toiminto on vain tapahtua kerran jokaisessa palvelimessa. Jos et halua synkronoida kaikki paveikslėlis, voit jättää tämän asetuksen valinta ja synkronoida kunkin cloud yksitellen VMM konsolissa cloud-ominaisuudet.

13.  Valitse **Seuraava** loppuun. Metatietojen VMM palvelimesta noutaa Azure palauttaminen rekisteröinnin jälkeen. Palvelimen näkyy säilö **palvelimia** -sivulla **VMM-palvelimet** -välilehden.

    ![LastPage](./media/site-recovery-vmm-to-azure-classic/provider13.PNG)

Metatietojen VMM palvelimesta noutaa Azure palauttaminen rekisteröinnin jälkeen. Palvelimen näkyy säilö **palvelimia** -sivulla **VMM-palvelimet** -välilehden.

### <a name="command-line-installation"></a>Komentorivin asennus

Azure sivuston palauttaminen tarjoajan voidaan asentaa myös käyttämällä seuraavaa komentoa. Tämän menetelmän avulla voidaan Providerin asentaminen Server Core for Windows Server 2012 R2.

1. Lataa Provider-asennuksen tiedosto- ja rekisteröintitietojen avaimen kansioon. Esimerkki: C:\ASR.
2. System Center virtuaalikoneen hallinta-palvelun pysäyttäminen
3. Järjestelmänvalvojan oikeuksin suoritettava komentokehote-poimia palveluntarjoajan asennusohjelma näitä komentoja:

        C:\Windows\System32> CD C:\ASR
        C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q

4. Providerin asentaminen seuraavasti:

        C:\ASR> setupdr.exe /i

5. Rekisteröi palvelu seuraavasti:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>       

Jos parametrit ovat seuraavat:

 - **/Credentials** : pakollinen parametri, joka määrittää sijainti, johon rekisteröinti avaimen tiedosto sijaitsee  
 - **/FriendlyName** : pakollinen parametri, joka näkyy Azure palauttaminen portaalissa Hyper-V Host (isäntä)-palvelimen nimi.
 - **/EncryptionEnabled** : valinnaisten parametrien, voit määrittää, haluatko salaus oman näennäiskoneiden Azure-tietokannassa (ja muiden salaus). Tiedostonimen on oltava **.pfx** -tunniste.
 - **/proxyAddress** : valinnainen parametri, joka määrittää välityspalvelimen osoitteen.
 - **/proxyport** : valinnainen parametri, joka määrittää välityspalvelimen porttiin.
 - **/proxyUsername** : valinnainen parametri, joka määrittää välityspalvelimen käyttäjänimen.
 - **/proxyPassword** : valinnainen parametri, joka määrittää välityspalvelimen salasanan.  


## <a name="step-4-create-an-azure-storage-account"></a>Vaihe 4: Azure-tallennustilan tilin luominen

1. Jos sinulla ei ole Azure-tallennustilan tilin valitsemalla **Lisää tili Azure-tallennustilan** tilin luominen.
2. Luo tili toistot sallittuja käytössä geo. Saman alueen Azure palauttaminen-palvelu on, ja saman tilauksen yhdistetä.

    ![Tallennustilan tilin](./media/site-recovery-vmm-to-azure-classic/storage.png)

> [AZURE.NOTE] [Siirron tallennustilan tilien](../resource-group-move-resources.md) saman tilauksen piiriin kuuluvien resurssien ryhmien tai tilauksissa ei tue tallennustilan tilejä sivustojen palauttamisen käyttöönotto.

## <a name="step-5-install-the-azure-recovery-services-agent"></a>Vaihe 5: Asenna Azure palautus palvelut-agentti

Asenna Azure palautus palvelut-agentti VMM pilvipalvelussa kunkin Hyper-V Host (isäntä)-palvelimeen.

1. Napsauta **Pika-aloitus** > **Lataa Azure sivuston palauttaminen Services agentti ja asenna isännät-** agentti asennustiedosto uusimman version hankkiminen.

    ![Asenna palautus palvelut-agentti](./media/site-recovery-vmm-to-azure-classic/install-agent.png)

2. Jokaisen Hyper-V Host (isäntä)-palvelimen asennustiedoston suorittaminen
3. Valitse **Edellytykset Tarkista** -sivulla **Seuraava**. Mikä tahansa puuttuu edellytykset asennetaan automaattisesti.

    ![Edellytykset palautus palvelut-agentti](./media/site-recovery-vmm-to-azure-classic/agent-prereqs.png)

4. Määritä **Asennuksen asetukset** -sivun kohtaa, johon haluat asentaa agentti ja valitse välimuistin sijainti, johon varmuuskopion metatietoihin asennetaan. Valitse **Asenna**.
5. Kun asennus on valmis, valitse **Sulje** ohjattu toiminto loppuun.

    ![Rekisteröi MARS agentti](./media/site-recovery-vmm-to-azure-classic/agent-register.png)

### <a name="command-line-installation"></a>Komentorivi-asennus

Voit myös asentaa Microsoft Azure palautus Services Agent komentoriviltä tällä komennolla:

    marsagentinstaller.exe /q /nu

## <a name="step-6-configure-cloud-protection-settings"></a>Vaihe 6: Määritä cloud suojausasetuksia

Kun VMM-palvelin on rekisteröity, voit määrittää cloud suojausasetuksia. Vaihtoehto **säilö Synkronoi cloud tietojen** käyttöön, kun asennettu palvelu, niin, että kaikki paveikslėlis VMM palvelimessa näkyvät säilö <b>Suojatut osat</b> -välilehden.

![Julkaistun pilveen](./media/site-recovery-vmm-to-azure-classic/clouds-list.png)

1. Napsauta Pika-aloitus-sivulla **VMM paveikslėlis suojauksen määrittäminen**.
2. Valitse **Suojatut osat** -välilehden **määritys** -välilehti ja määritä haluamasi pilveen.
3. Valitse **kohde** **Azure**.
4. Valitse **Tallennustilan tilin** replikoinnin Azure tallennustilan tiliä.
5. Määritä **Salaa tallennetut tiedot** **käytöstä**. Tämä asetus määrittää, että tiedot salataan replikoida paikallisen sivuston ja Azure välillä.
6. **Kopioi korkojakso** jätä oletusasetusta. Tämä arvo määrittää, kuinka usein tietoja synkronoitu lähde- ja sijaintien välillä.
7. **Säilytä palautus pisteet**jätä oletusasetusta. Oletusarvo on nolla vain uusimmat palautuspisteen ensisijainen virtual tietokoneelle on tallennettu replikan Host (isäntä)-palvelimeen.
8. **Sovelluksen yhdenmukaisia tilannevedoksia taajuus**jätä oletusasetusta. Tämä arvo määrittää, kuinka usein tilannevedosten luominen. Tilannevedoksia aseman varjostus kopioi Service (VSS) avulla voit varmistaa, että sovellukset ovat yhdenmukaisia tilaan tilannevedoksen otettaessa.  Jos määrität arvon, varmista, että se on pienempi kuin määrität palautuksen pisteiden lukumäärä.
9. **Replikoinnin aloitusaika**Määritä tietojen Azure ensimmäinen replikointi käynnistettäessä. Käytetään aikavyöhyke Hyper-V Host (isäntä)-palvelimeen. On suositeltavaa ajoittaa ensimmäisen replikoinnin myöhemmin.

    ![Pilven Replikointiasetukset](./media/site-recovery-vmm-to-azure-classic/cloud-settings.png)

Kun olet tallentanut asetukset työn luodaan ja voidaan valvoa **työt** -välilehden. Kaikki Hyper-V Host (isäntä)-palvelimet VMM lähde pilvipalvelussa määritetään replikoinnin.

Tallennuksen jälkeen cloud asetuksia voi muokata **määrittäminen** -välilehdessä. Voit muokata kohdesijainti tai kohde-tallennustilan tilin sinun on poistettava Tunnistepilven määritykset ja määritä pilveen. Huomaa, että jos muutat tallennustilan tilin muutos vain käytetään näennäiskoneiden, joissa on otettu käyttöön suojaus, kun tallennustilan-tili on muokattu. Olemassa olevan näennäiskoneiden ei siirretä tallennustilan uuteen tiliin.

## <a name="step-7-configure-network-mapping"></a>Vaihe 7: Määritä verkon määritys
Ennen kuin aloitat verkon määritys Varmista, että näennäiskoneiden lähteen VMM palvelimessa yhteydessä verkkoon AM. Lisäksi voit luoda yhden tai useamman Azure virtual verkot. Huomaa, että useiden AM verkostojen voidaan määrittää yhden Azure verkoston.

1. Napsauta Pika-aloitus-sivulla **Yhdistä verkot**.
2. Valitse VMM lähdepalvelimeen **verkot** -välilehden **lähdesijainnissa**. Valitse **kohdesijainti** Azure.
3. **Lähde** -verkoissa näkyvät AM verkkojen VMM palvelimen liittyvä luettelo. **Kohde** -verkoissa tilaukseen liittyvää Azure verkot tulevat näyttöön.
4. Valitse lähde AM verkko ja valitse **Yhdistä**.
5. Valitse **Valitse kohde-verkko** -sivun target Azure verkon, jota haluat käyttää.
6. Valitse Viimeistele yhdistäminen valintamerkkiä.

    ![Pilven Replikointiasetukset](./media/site-recovery-vmm-to-azure-classic/map-networks.png)

Kun olet tallentanut asetukset työn käynnistää yhdistämisen edistymisen seuranta ja se voidaan valvoa työt-välilehden. Kaikki aiemmin replikan näennäiskoneiden, jotka vastaavat tietolähteen AM verkon yhdistetään kohde Azure verkot. Uusien näennäiskoneiden, joka on yhteydessä Internetiin lähde AM yhdistetään Azure yhdistetystä replikoinnin jälkeen. Jos muokkaat aiemmin luodun uuden verkon yhdistäminen, replikan näennäiskoneiden yhdistetään käyttämällä uusia asetuksia.

Huomaa, että, jos kohde verkossa on useita aliverkosta ja jokin näistä aliverkosta on sama nimi kuin aliverkon joina lähde virtuaalikoneen sijaitsee ja valitse replikan virtuaalikoneen yhdistetään kyseisen kohteen aliverkon jälkeen automaattisesti. Jos ei ole kohteen aliverkon vastaavaa nimeä, virtuaalikoneen yhdistetään ensimmäisen aliverkon verkossa.

> [AZURE.NOTE] [Siirron verkostojen](../resource-group-move-resources.md) saman tilauksen piiriin kuuluvien resurssien ryhmien tai tilauksissa ei tue verkkojen käytettäviä sivustojen palauttamisen käyttöönotto.

## <a name="step-8-enable-protection-for-virtual-machines"></a>Vaihe 8: Ota suojaus näennäiskoneiden

Kun palvelimissa, paveikslėlis ja käyttäminen on määritetty oikein, voit ottaa suojauksen näennäiskoneiden pilveen. Ota seuraavat seikat huomioon:

- Näennäiskoneiden on täytettävä [Azure vaatimukset](site-recovery-best-practices.md#azure-virtual-machine-requirements).
- Ottaa käyttöön suojauksen käyttöjärjestelmä ja käyttöjärjestelmän levyn ominaisuudet on määritettävä virtuaalikoneen. Kun luot virtual machine VMM virtuaalikoneen mallin avulla voit määrittää ominaisuus. Voit myös määrittää nämä ominaisuudet aiemmin näennäiskoneiden virtuaalikoneen ominaisuudet **Yleiset** ja **Laitekokoonpanon** -välilehdissä. Jos et määritä nämä ominaisuudet-VMM voit voi määrittää ne Azure sivuston palautus-portaalissa.

    ![Luo virtuaalikoneen](./media/site-recovery-vmm-to-azure-classic/enable-new.png)

    ![Virtuaalikoneen ominaisuuksien muokkaaminen](./media/site-recovery-vmm-to-azure-classic/enable-existing.png)


1. Ota Suojaus-välilehden **näennäiskoneiden** pilveen, jossa virtuaalikoneen sijaitsee, valitse **Ota suojaus käyttöön** > **Lisää näennäiskoneiden**.
2. Valitse haluamasi suojaa näennäiskoneiden pilveen-luettelosta.

    ![Virtuaalikoneen suojauksen ottaminen käyttöön](./media/site-recovery-vmm-to-azure-classic/select-vm.png)

    **Työt** -välilehti, mukaan lukien ensimmäisen replikoinnin **Ota suojaus** -toiminnon edistymisen seuraaminen. Kun **Viimeisteleminen suojaus** suoritetaan virtuaalikoneen on valmiina automaattisesti. Kun suojaus on käytössä ja näennäiskoneiden on replikoida, voit voi tarkastella niitä Azure.


    ![Virtuaalikoneen suojaus työ](./media/site-recovery-vmm-to-azure-classic/vm-jobs.png)

3. Tarkista virtuaalikoneen ominaisuudet ja Muokkaa halutulla tavalla.

    ![Tarkista näennäiskoneiden](./media/site-recovery-vmm-to-azure-classic/vm-properties.png)


4. Virtuaalikoneen-ominaisuuksien **määrittäminen** -välilehdessä seuraavat verkko-ominaisuuksia voi muokata.





- **Kohteen virtuaalikoneen verkkosovittimien määrä** - verkkosovittimien määrä määräytyy kohde virtuaalikoneen määrittäminen koon mukaan. Tarkista [virtuaalikoneen koon Etkö](../virtual-machines/virtual-machines-linux-sizes.md#size-tables) sovittimet virtuaalikoneen koon tukemat lukumääräksi. Jos muokkaat virtual machine koon ja Tallenna asetukset, verkkosovittimen määrä muuttuvat, kun avaat **Määritä** sivun seuraavan kerran. Verkkosovittimien kohde näennäiskoneiden on määrä on verkkosovittimien lähde virtual koneessa vähimmäismäärä ja valinnut seuraavasti virtuaalikoneen koon tukemat verkkosovittimien enimmäismäärä:

    - Jos verkkosovittimien lähde-koneessa määrä on pienempi tai yhtä sovittimet sallittu kohde koneen kokoa, kohde on yhtä monta sovittimet lähde.
    - Jos lähde virtuaalikoneen sovittimet ylittää sallittu, Kohdekoko sitten kohde enimmäiskoko käytetään.
    - Jos lähdetietokoneen on kaksi verkkosovittimien ja kohde koneen koon tukee neljä, kohdetietokoneen on kahden sovittimen. Jos lähde on kahden sovittimen mutta tuetut kohteen koon tukee vain yksi kohdetietokoneen on vain yksi sovittimen.    

- **Kohteen virtuaalikoneen verkko** -, johon virtuaalikoneen muodostaa verkon määräytyy verkon määritystä lähde virtual machine verkkoon. Jos lähde virtuaalikoneen on useampi kuin yksi verkkosovittimen ja lähde-verkoissa on yhdistetty eri verkkoihin aikataulussa, valitse ne on valita jokin kohde-verkoissa.
- **Verkkosovittimen aliverkon** - kunkin verkko-sovittimen, voit valita, johon aliverkon epäonnistunutta päälle virtuaalikoneen muodostaa yhteyden.
- **Kohde-IP-osoite** - tietolähteen virtual machine verkkosovittimen on määritetty käyttämään staattinen IP-osoite ja valitse kohde virtuaalikoneen voit antaa IP-osoite. Käyttää tätä toimintoa Säilytä lähde virtual koneen IP-osoite automaattisesti jälkeen. Jos IP-osoitetta ei ole annettu käytettävissä minkä tahansa IP-osoite annetaan verkkosovittimen automaattisesti aikaan. Jos kohde-IP-osoite on määritetty, mutta se on jo käytössä Azure käyttävä virtual tietokone automaattisesti epäonnistuu.  

    ![Verkon ominaisuuksien muokkaaminen](./media/site-recovery-vmm-to-azure-classic/multi-nic.png)

>[AZURE.NOTE] Linux näennäiskoneiden staattinen IP-osoitetta ei tueta.

## <a name="test-the-deployment"></a>Käyttöönoton testaaminen

Voit testata käyttöönoton Suorita testi-automaattisesti virtual yhdelle tietokoneelle, tai useita näennäiskoneiden koostuva palautus suunnitelman luominen ja Suorita testi-automaattisesti palvelupaketin.  

Testaa automaattisesti varmistaminen automaattisesti ja palautus-järjestelmä eristetyssä verkossa. Huomaa, että:

- Jos haluat muodostaa yhteyden virtuaalikoneen Azure-tietokannassa etätyöpöydän vikasietotilaa jälkeen, ota käyttöön virtuaalikoneen Etätyöpöytäyhteys, ennen kuin suoritat testin vikasietotilaa.
- Automaattisesti jälkeen käytät julkiseen IP-osoite muodostaa virtuaalikoneen etätyöpöydän Azure-tietokannassa. Jos haluat tehdä tämän, varmista toimialuekäytäntöjä, jotka estävät yhteyden julkisen osoitteen virtual tietokoneeseen ei ole.

>[AZURE.NOTE] Saat parhaan suorituskyvyn, kun teet automaattisesti Azure, varmista, että olet asentanut Azure-agentti suojattujen kone. Tämä auttaa-käynnistäminen nopeammin ja myös auttaa Jos ongelmien vianmäärityksen. Linux edustaja voi olla löytyneen [tähän](https://github.com/Azure/WALinuxAgent) - ja Windows agentti löytyy [tähän](http://go.microsoft.com/fwlink/?LinkID=394789)

### <a name="create-a-recovery-plan"></a>Suunnittele palauttaminen

1. Lisää uusi suunnitelma **Palautus suunnitelmien** -välilehteen. Nimi, **VMM** **lähdetyyppi**ja **lähde**-kohteen VMM lähdepalvelimeen päivitetään Azure.

    ![Luo suunnitelma palauttaminen](./media/site-recovery-vmm-to-azure-classic/recovery-plan1.png)

2. Valitse **Valitse näennäiskoneiden** -sivulla näennäiskoneiden lisääminen palautus-suunnitelma. Nämä näennäiskoneiden on lisätty palautus suunnitelman oletusryhmään, ryhmä 1. Enintään 100 näennäiskoneiden yksittäisen palautus-suunnitelmassa on testattu.

- Jos haluat tarkistaa ennen niiden lisäämistä suunnitelman virtuaalikoneen ominaisuudet valitsemalla Ominaisuudet-sivulla, jossa se pilveen virtuaalikoneen on tallennettu. Voit myös määrittää virtuaalikoneen ominaisuudet VMM konsolissa.
- Kaikki, jotka näkyvät näennäiskoneiden on otettu käyttöön suojaus. Luettelo sisältää sekä näennäiskoneiden, jotka ovat käytössä, suojaus ja ensimmäinen replikointi on valmis, ja ne, jotka on otettu käyttöön suojaaminen alkuperäinen replikoinnin odottaa. Vain ensimmäinen toistot valmis näennäiskoneiden voi epäonnistua päälle osana palautus suunnitelma.

    ![Luo suunnitelma palauttaminen](./media/site-recovery-vmm-to-azure-classic/select-rp.png)

Kun palautus-palvelupaketti on luotu se näkyy **Palautus suunnitelmien** -välilehti. Voit lisätä myös [Azure automaatio runbooks](site-recovery-runbook-automation.md) palautus aiot automatisoida aikana automaattisesti.

### <a name="run-a-test-failover"></a>Suorita testi automaattisesti

Suorita testi automaattisesti Azure kahdella eri tavalla.

- **Testaa automaattisesti ilman Azure verkon**– tällaista testi automaattisesti tarkistaa, että virtuaalikoneen käynnistyy oikein Azure-tietokannassa. Ei ole yhdistetty Azure-verkkoon virtuaalikoneen jälkeen automaattisesti.
- **Testaa automaattisesti Azure verkoston kanssa**– tämäntyyppinen automaattisesti tarkistaa koko replikoinnin-ympäristö on enintään oikein ja päälle näennäiskoneiden epäonnistuneiden yhdistetään määritetty kohde Azure verkkoon. Aliverkon käsittelyä varten testin automaattisesti testi virtuaalikoneen aliverkon olla toimivuutta replikan virtuaalikoneen aliverkon mukaan. Tämä on eri säännöllisesti replikointi, kun aliverkon replikan virtual koneen perusteella lähde virtuaalikoneen aliverkon.

Jos haluat suorittaa testi-automaattisesti käytössä määrittämättä Azure kohde-verkkoon, sinun ei tarvitse tehdä mitään valmistelu suojauksen Azure virtual tietokoneelle. Voit suorittaa kohteelle, sinun on luoda uusi Azure verkkoon, joka on eristetty Azure tuotannon verkosta (oletusarvoista toimintamallia, kun luot uuden verkon Azure) Azure verkon testi automaattisesti. Katso lisätietoja [Suorita testi automaattisesti](site-recovery-failover.md#run-a-test-failover) .


Sinun on myös määrittämään infrastruktuuri replikoitua virtuaalikoneen toimii odotetulla tavalla. Esimerkiksi virtual tietokoneessa, jossa toimialueen ohjauskoneen ja DNS voi replikoida Azure käyttämällä Azure palauttaminen ja voidaan luoda testi verkkoon testata automaattisesti. Katso lisätietoja [testaaminen automaattisesti huomioon otettavia seikkoja active Directory](site-recovery-active-directory.md#considerations-for-test-failover) -osassa.

Suorita testi-automaattisesti Älä seuraavasti:

1. **Suunnitelmien palautus** -välilehdessä palvelupaketti, ja valitse **Testaa automaattisesti**.
2. Valitse **Vahvista testi automaattisesti** -sivulla **ei mitään** tai tiettyyn Azure verkkoon.  Huomaa, että jos valitset ei mitään testi vikasietotilaa tarkistaa, että virtuaalikoneen replikoida oikein mutta Azure ei tarkista replikoinnin verkon asetusten.

    ![Verkkoa](./media/site-recovery-vmm-to-azure-classic/test-no-network.png)

3. Jos salauksen on käytössä pilveen, valitse **Salausavaimen** varmenne, jota on annettu tarjoajan VMM palvelimessa, asennuksen aikana, kun halutessasi ottaa käyttöön pilvestä salauksen käytössä.
4. **Työt** -välilehden automaattisesti edistymisen seuraaminen. On myös oltava näe virtuaalikoneen testaa se Azure-portaalissa. Jos ole access näennäiskoneiden määrittää paikallisen verkosta voit aloittaa virtuaalikoneen etätyöpöydän yhteyden.
5. Kun vikasietotilaa saavuttaa **valmiina testaaminen** vaiheeseen, valitse Valmis testi vikasietotilaa **Valmis testata** . Voit siirtyä alaspäin **työ** -välilehdessä voit seurata automaattisesti edistymistä ja tilaa ja suorittaa toimintoja, joita tarvitaan.
6. Automaattisesti jälkeen on näe virtuaalikoneen testaa se Azure-portaalissa. Jos ole access näennäiskoneiden määrittää paikallisen verkosta voit aloittaa virtuaalikoneen etätyöpöydän yhteyden. Toimi seuraavasti:

    1. Varmista, näennäiskoneiden käynnistäminen onnistuu.
    2. Jos haluat muodostaa yhteyden virtuaalikoneen Azure-tietokannassa etätyöpöydän vikasietotilaa jälkeen, ota käyttöön virtuaalikoneen Etätyöpöytäyhteys, ennen kuin suoritat testin vikasietotilaa. Tarvitset myös lisätä RDP päätepisteen virtuaalikoneen. [Azure automaatio Runbooks](site-recovery-runbook-automation.md) voit tehdä sen avulla voidaan hyödyntää.
    3. Kun automaattisesti, jos julkisen IP-osoitteen avulla voit muodostaa virtuaalikoneen Azure-tietokannassa käyttämällä etätyöpöydän kautta, varmista toimialuekäytäntöjä, jotka estävät yhteyden julkisen osoitteen virtual tietokoneeseen ei ole.

7.  Kun testaus on suoritettu, toimi seuraavasti:
    - Valitse **Testaa vikasietotilaa on valmis**. Puhdista testiympäristössä virtaa ja poista testi näennäiskoneiden automaattisesti.
    - Valitse **muistiinpanot** ja tallentaminen testi vikasietotilaa liittyvät huomautukset.

>

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja [määrittäminen palautus suunnitelmien](site-recovery-create-recovery-plans.md) ja [automaattisesti](site-recovery-failover.md).

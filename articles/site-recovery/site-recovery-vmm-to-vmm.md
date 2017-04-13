<properties
    pageTitle="Replikoida Hyper-V näennäiskoneiden-VMM paveikslėlis toissijainen VMM sivuston Azure-portaalissa | Microsoft Azure"
    description="Tässä artikkelissa käsitellään Azure palauttaminen orchestrate replikoinnin, automaattisesti ja Hyper-V VMs-VMM paveikslėlis palauttamista toissijainen VMM sivustoon Azure-portaalissa voit ottaa käyttöön."
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
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="raynew"/>

# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-a-secondary-vmm-site-using-the-azure-portal"></a>Toistaa Hyper-V näennäiskoneiden-VMM paveikslėlis toissijainen VMM sivuston Azure-portaalissa

> [AZURE.SELECTOR]
- [Azure portal](site-recovery-vmm-to-vmm.md)
- [Perinteinen portal](site-recovery-vmm-to-vmm-classic.md)
- [PowerShell - resurssien hallinta](site-recovery-vmm-to-vmm-powershell-resource-manager.md)

Tervetuloa käyttämään Azure palauttaminen! Tässä artikkelissa, jos haluat toistaa paikallisen Hyper-V näennäiskoneiden hallitaan System Center Virtual Machine Manager (VMM) paveikslėlis toissijaisen sivuston käyttö Tässä artikkelissa käsitellään määrittäminen replikoinnin käyttämällä Azure sivuston Azure-portaalissa.

> [AZURE.NOTE] Azure on kaksi eri [käyttöönoton mallien](../resource-manager-deployment-model.md) luominen ja käyttäminen resurssit: Azure Resurssienhallinta ja perinteinen. Azure on myös kaksi portaaleihin – Azure perinteinen portaalin, joka tukee perinteinen käyttöönottomalli ja Azure-portaalin tukeen käyttöönoton sekä malleille.


Azure palauttaminen Azure-portaalissa on useita uusia ominaisuuksia:

- Azure-tietokannassa portal, Azure varmuuskopiointi- ja Azure palauttaminen palvelut yhdistetään yhteen palautus palvelut-säilö, niin, että voit määrittää ja hallita liiketoiminnan jatkuvuus ja palauttaminen (BCDR) yhdestä paikasta. Yhdistetty Raporttinäkymät-ikkunan avulla voit valvoa ja toimintojen hallitsemaan paikallisen-sivustojen ja Azure julkisen pilveen.
- Käyttäjät, joilla on valmisteltu Cloud ratkaisu Provider (CSP)-ohjelmalla Azure tilaukset voivat nyt hallita sivuston palauttaminen toimintojen Azure-portaalissa.


Luettuasi tämän artikkelin kirjaa kommentteja Disqus kommenttien alaosassa. Esitä kysymyksiä tekniset [Azure palautus Services keskustelupalstalle](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="overview"></a>Yleiskatsaus

Organisaatioiden on BCDR strategia, joka määrittää, miten sovellukset ja työmääriä pysy käynnissä ja käytettävissä aikana suunnitellut ja suunnittelemattomat käyttökatkot ja palauttaa normaalijakauman käytön edellytykset mahdollisimman pian. BCDR strategia kannattaa säilyttäminen yritystietojen turvassa ja palautettavissa ja varmistetaan, että työmääriä pysyvät jatkuvasti käytettävissä, kun tietojen tapahtuu.

Sivuston palautus on Azure-palvelu prosentuaalista osuutta BCDR strategia mukaan orchestrating replikoinnin paikallisen fyysiset palvelimet ja näennäiskoneiden pilveen (Azure) tai toissijainen palvelinkeskukseen. Kun katkokset ensisijainen sijainti-asiakas ei päälle toissijainen sijainnin vastaamaan itsellesi sovellukset ja toiminnoista, jotka ovat käytettävissä. Asiakas ei takaisin ensisijainen sijainti, kun se palauttaa normaalijakauman toimintoihin. Lisätietoja [Azure palauttaminen ominaisuudet?](site-recovery-overview.md)

Tässä artikkelissa on kaikki tiedot, jotka haluat kopioida paikallisen Hyper-V VMs-VMM paveikslėlis toissijainen VMM-sivustoon. Se sisältää arkkitehtuuri yleiskatsaus, tiedot ja käyttöönoton ohjeet määrittäminen paikallista palvelinten, Replikointiasetukset ja kapasiteetin suunnittelua. Kun olet määrittänyt infrastruktuuri voit ottaa replikoinnin tietokoneissa, jotka haluat suojata ja tarkista, että automaattisesti toimii.

## <a name="business-advantages"></a>Liiketoiminnan edut

- Sivuston palauttaminen suojaa business toiminnoista ja sovellusten replikoiminen ne toissijaisen Hyper-V palvelimen Hyper-V VMs suorittamalla.
- Palautus palvelut-portaalissa on yhden paikan määrittäminen, hallita ja seurata replikoinnin automaattisesti ja palauttaminen.
- Voit suorittaa helposti Suorita failovers ensisijainen paikallisen infrastruktuurin toissijaisen ja tuntisesta (Palauta) ensisijaisen toissijainen-sivustosta.
- Voit määrittää useita koneet palautus suunnitelmien niin, että Porrastettu sovelluksen työmääriä epäonnistua yhdessä päälle.

## <a name="scenario-architecture"></a>Skenaario-arkkitehtuuri

Skenaario osat ovat seuraavat:

- **Ensisijainen**: Valitse ensisijainen-sivustossa yhden tai useamman Hyper-V host palvelimessa on lähde VMs, jonka haluat toistaa. Seuraavissa ensisijainen host-palvelimissa sijaitsevat VMM yksityinen pilvestä.
- **Toissijaisen**: toissijainen-sivustossa on vähintään yksi Hyper-V host palvelimessa on kohde VMs, johon voit toistaa ensisijainen VMs. Seuraavissa host-palvelimissa sijaitsevat VMM yksityinen pilvestä. Pilveen voi olla ensisijainen palvelimella (Jos käytössä on vain yksi VMM-palvelin) tai toissijaisen VMM palvelimessa.
- **Palvelu**: palauttaminen käyttöönoton aikana Azure sivuston palauttaminen Providerin asentaminen VMM-palvelimiin ja rekisteröi kyseisiin palvelimiin palautus Services säilöön. VMM palvelimessa tarjoajan yhteydessä palauttaminen HTTPS 443 replikointia tiedonsiirron päälle. Tietoja Replikointi tapahtuu ensisijainen ja toissijainen Hyper-V host palvelinten välillä. Replikoitua tietojen sisäpuolella paikallisen sivustot ja verkkojen ja Azure ei lähetetä. Lisätietoja [Tietosuoja](#privacy-information-for-site-recovery).

![E2E topologian](./media/site-recovery-vmm-to-vmm/architecture.png)

### <a name="data-privacy-overview"></a>Tietojen tietosuoja yleiskatsaus

Tässä taulukossa esitetään, miten tiedot on tallennettu tässä skenaariossa:
****
Toiminto | **Tiedot** | **Kerättyjen tietojen** | **Käytä** | **Pakollinen**
--- | --- | --- | --- | ---
**Rekisteröinti** | Palautus Services säilöön rekisteröidä VMM palvelimeen. Jos haluat myöhemmin unregister palvelimeen, voit tehdä poistamalla Azure-portaalista palvelimen tiedot. | Kun VMM-palvelin on rekisteröity palauttaminen kerää, prosessit, ja siirtää metatietoa VMM palvelimen ja sivuston palauttaminen havaitsemien VMM paveikslėlis nimet. | Tietoja käytetään tunnistamaan yhteydenpito haluamasi VMM palvelin ja sopiva VMM paveikslėlis asetusten määrittäminen. | Tämä ominaisuus on pakollinen. Jos et halua lähettää tiedot palauttaminen ei kannata käyttää sivuston palautus-palvelun.
**Replikoinnin ottaminen käyttöön** | Azure sivuston palauttaminen tarjoaja on asennettu VMM-palvelimeen, ja se on siirtokanava viestintään sivuston palautus-palvelussa. Palvelu on ylläpidettävä VMM prosessin dll-kirjaston (DLL). Kun palvelu on asennettu, "palvelinkeskuksen" palautustoiminto saa käyttöön VMM järjestelmänvalvojan konsolissa. Uusille ja nykyisille VMs ottaa tämän ominaisuuden käyttöön AM suojaus. | Tämän ominaisuuden arvo, jossa toimittaja lähettää nimi ja tunnus AM palauttaminen.  Replikoinnin on käytössä Windows Server 2012: ssa tai Windows Server 2012 R2 Hyper-V replikan. Virtuaalikoneen tiedot saa replikoida isännästä Hyper-V toiseen (yleensä sijaitsevat eri "palautus" tietokeskuksen). | Sivuston palauttaminen käyttää metatiedot täytä AM tiedot Azure-portaalissa. | Tämä toiminto on olennainen osa palvelun ja voi poistaa käytöstä. Jos et halua lähettää tiedot, älä ota käyttöön VMs palauttaminen suojauksen. Huomaa, että kaikki tiedot lähetetään tarjoaja sivuston palauttaminen lähetetään HTTPS-protokollan välityksellä.
**Suunnitelman palautus** | Palautus suunnitelmien avulla voit täydentää palautus tietokeskuksen tiedonsiirron-suunnitteleminen. Voit määrittää järjestyksen, jossa VMs tai ryhmän näennäiskoneiden on käynnistettävä palautus-sivustossa. Voit myös määrittää automaattisia komentosarjat on otettava kunkin AM palauttamisen milloin suoritetaan tai mitä tahansa Manuaalinen toiminto. Yleensä automaattisesti käynnistyy, kun palautus suunnitelman tasolla koordinoidun palauttamista varten. | Sivuston palauttaminen kerää, käsittelee ja välittää palautus-palvelupakettia, mukaan lukien virtuaalikoneen metatietojen ja metatietojen Automaatiokomentosarjojen ja Manuaalinen toiminto muistiinpanot-metatiedot. | Metatietojen käytetään luominen Azure-portaalissa palautus-suunnitelma. | Tämä toiminto on olennainen osa palvelun ja voi poistaa käytöstä. Jos et halua lähettää tiedot palauttaminen ei tarvitse luoda palautus-palvelupakettia.
**Verkon määritys** | Yhdistää tiedot verkon ensisijainen tietokeskuksen palauttamisen tietojen käyttöoikeudet. Kun VMs palautetaan palautus-sivustossa, verkon määritys auttaa vahvistamiseksi verkkoyhteyden. | Sivuston palauttaminen kerää, käsittelee ja välittää metatiedot looginen verkostojen kunkin sivuston (ensisijainen ja palvelinkeskuksen). | Metatietojen käytetään täytä verkkoasetukset niin, että voit yhdistää verkkotiedot. | Tämä toiminto on olennainen osa palvelun ja voi poistaa käytöstä. Jos et halua lähettää tiedot sivustojen palauttamisen, älä käytä verkon määritys.
**Automaattisesti (suunniteltu/suunnittelematon/testi)** | Automaattisesti epäonnistuu VMs VMM hallitun tietojen Centeristä toiseen päälle. Automaattisesti-toiminto käynnistetään manuaalisesti Azure-portaalissa. | Palvelun VMM palvelimessa ilmoitetaan automaattisesti tapahtuman palauttaminen ja suorittaa automaattisesti-toiminto Hyper-V isännän VMM liittymät kautta. AM todellinen automaattisesti on yksi Hyper-V-isännästä toiseen ja Windows Server 2012: ssa tai Windows Server 2012 R2 Hyper-V replikan käsitellyt. Automaattisesti päätyttyä palvelun palautus tietokeskuksen VMM palvelimessa lähettää success tietoja sivuston palauttaminen. | Sivuston palauttaminen käyttää täyttäminen automaattisesti toimenpiteen tiedot Azure-portaalissa tilan lähetetyt tiedot. | Tämä toiminto on olennainen osa palvelun ja voi poistaa käytöstä. Jos et halua lähettää tiedot sivustojen palauttamisen, älä käytä automaattisesti.


## <a name="azure-prerequisites"></a>Azure edellytykset

Seuraavassa on hyödyllisiä Azure-tietokannassa käyttöön tässä skenaariossa:

**Edellytykset** | **Tiedot**
--- | ---
**Azure**| Tarvitset [Microsoft Azure](http://azure.microsoft.com/) -tiliin. Voit aloittaa [maksuttoman kokeiluversion käyttäjäksi](https://azure.microsoft.com/pricing/free-trial/). [Lue lisää](https://azure.microsoft.com/pricing/details/site-recovery/) siitä, että sivuston palauttaminen hinnat.


## <a name="on-premises-prerequisites"></a>Paikallisen edellytykset

Mitä sinun on ensisijainen ja toissijainen paikallinen sivustoissa käyttöön tässä skenaariossa:

**Edellytykset** | **Tiedot**
--- | ---
**VMM** | Suosittelemme, että otat VMM-palvelimen ensisijainen sivuston ja VMM toissijainen-sivustossa.<br/><br/> Voit myös [replikoida paveikslėlis yhteen VMM palvelimeen välillä](site-recovery-single-vmm.md). Toiminto on vähintään kaksi paveikslėlis määritettynä VMM-palvelimessa.<br/><br/> VMM palvelimet on oltava käynnissä vähintään System Center 2012 SP1 ja viimeisimmät päivitykset.<br/><br/> Kunkin VMM palvelimen on oltava yhdellä tai Lisää paveikslėlis määritetty ja kaikki paveikslėlis on oltava Määritä Hyper-V kapasiteetin profiili. <br/><br/>Paveikslėlis on sisällettävä vähintään yksi VMM host ryhmät.<br/><br/>Lisätietoja VMM paveikslėlis [määrittäminen VMM cloud kangasta](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric), määrittäminen ja [vaiheittaisissa ohjeissa: luominen yksityinen paveikslėlis System Center 2012 SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx).<br/><br/> VMM palvelimet on internet-yhteyttä.
**Hyper-V** | Hyper-V-palvelimet on oltava vähintään Windows Server 2012 Hyper-V-roolin kanssa, muttet asentanut uusimmat päivitykset.<br/><br/> Hyper-V server on sisällettävä vähintään yksi VMs.<br/><br/>  Host (isäntä)-ryhmien ensisijainen ja toissijainen VMM paveikslėlis pitäisi sijaita Hyper-V host-palvelimissa.<br/><br/> Jos käytössäsi on Hyper-V Windows Server 2012 R2-klusterin asentamista, [Päivitä 2961977](https://support.microsoft.com/kb/2961977)<br/><br/> Jos käytät Hyper-V-klusterin Windows Server 2012-Huomaa, että klusterin broker ei ole luodaan automaattisesti, jos sinulla on staattinen IP osoite-pohjainen klusterin. Sinun täytyy määrittää klusterin broker manuaalisesti. [Lue lisää](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx).
**Toimittaja** | Sivuston palauttaminen käyttöönoton aikana Azure sivuston palauttaminen tarjoajan asentaminen VMM-palvelimiin. Palvelun yhteydessä palauttaminen HTTPS 443, orchestrate replikoinnin päälle. Tietoja Replikointi tapahtuu Lähiverkon tai VPN-yhteyden kautta ensisijainen ja toissijainen Hyper-V-palvelinten välillä.<br/><br/> VMM palvelimessa tarjoajan tarvitseville näistä URL-osoitteista: *. hypervrecoverymanager.windowsazure.com; *. accesscontrol.Windows.NET; *. backup.windowsazure.com; *. BLOB.Core.Windows.NET; *. store.core.windows.net.<br/><br/> Lisäksi Salli palomuurin tietoliikenne palvelimiin VMM [Azure palvelinkeskuksen IP-alueita](https://www.microsoft.com/download/confirmation.aspx?id=41653) ja Salli HTTPS (443)-protokollaa.

## <a name="prepare-for-deployment"></a>Valmistele käyttöönottoa varten

Valmistele käyttöönoton, sinun on:

1. [Valmistele VMM palvelimen](#prepare-the-vmm-server) palauttaminen käyttöönottoa varten.
2. [Valmistele verkon määritystä varten](#prepare-for-network-mapping). Määritä verkot, jotta voit määrittää verkon määritys.

### <a name="prepare-the-vmm-server"></a>Valmistele VMM-palvelin

Tarkista, että VMM palvelimen [edellytykset](#on-premises-prerequisites) noudattaa ja käyttää luettelossa URL-osoitteet.


### <a name="prepare-for-network-mapping"></a>Valmistele verkon yhdistämistä varten

Verkon määritys kartat VMM AM verkkojen ensisijainen ja toissijainen VMM palvelimissa välillä:

- Aseta replikan VMs optimaalisesti toissijainen Hyper-V isännät jälkeen automaattisesti.
- Yhdistä replikan VMs tarvittavat AM verkkoihin.
- Jos et määritä verkon määritys replikan VMs ei liitettävä minkä tahansa verkkoon jälkeen automaattisesti.
- Jos haluat määrittää verkon yhdistäminen sivuston palauttamisen aikana käyttöönoton tähän seuraavasti:

    - Varmista, että VMM AM verkon yhteydessä VMs lähde Hyper-V Host (isäntä)-palvelimeen. Verkon olisi voidaan linkittää looginen verkko, joka on liitetty pilveen.
    - Varmista, että toissijainen pilveen, jota käytetään palauttamista varten on määritetty vastaavan AM verkkoon. Että AM verkon linkitetty looginen verkko, joka liittyy toissijainen pilveen.

- [Lisätietoja](site-recovery-network-mapping.md) verkon määritys toiminnasta.

## <a name="prepare-for-deployment-with-a-single-vmm-server"></a>Valmistele käyttöönoton yksittäisen VMM-palvelimen kanssa

Jos sinulla on vain yhden VMM palvelimen voit toistaa VMs-Hyper-V isännät VMM pilvipalvelussa [Azure](site-recovery-vmm-to-azure.md) tai toissijaisen VMM pilveen. Suosittelemme, että ensimmäinen vaihtoehto koska replikoiminen paveikslėlis välillä ei ole saumattomasti, mutta jos tarvitset täällä on sinun on suoritettava:

1. **Valitse Hyper-V AM VMM määrittäminen**. Kun olemme Suosittelemme VMM-saman AM käyttämän SQL Server-esiintymän colocate. Tämä säästää aikaa luoda on vain yksi AM. Jos haluat käyttää Laitteestaan ja SQL Server käyttökatkosta, sinun on palautettava kyseisen esiintymän, ennen kuin voit palauttaa VMM.
2. **Varmista, että VMM palvelin on määritetty vähintään kaksi paveikslėlis**. Yksi cloud sisältää VMs haluat toistaa ja muut pilveen palvelee toissijainen sijainniksi. Pilveen, joka sisältää VMs, jonka haluat suojata on oltava sähköpostistandardien [edellytykset](#on-premises-prerequisites).
3. Määritä palauttaminen tässä artikkelissa kuvatulla tavalla. Luominen ja rekisteröi VMM palvelimen säilö, replikoinnin käytännön määrittäminen ja replikoinnin käyttöön. Määritä, että ensimmäinen replikointi tapahtuu verkon kautta.
4. Kun määrität verkon määritys ensisijainen pilveen AM verkosta yhdistäminen toissijainen pilveen AM verkosta.
5. Hyper-V hallinta-konsolin käyttöön Hyper-V replikan, joka sisältää VMM AM Hyper-V isännän ja ota käyttöön AM replikoinnin. Varmista, että Älä lisää VMM virtuaalikoneen paveikslėlis, jotka on suojattu palauttaminen, varmista, että Hyper-V replikan asetukset eivät ole ohittaa sivuston palauttaminen.
6. Jos luot palautus suunnitelmat automaattisesti käytät samaan VMM palvelimeen lähde- ja kohdesivustojen.
7. Epäonnistua päälle ja palauttaa seuraavasti:

    - Manuaalisesti epäonnistua VMM AM Hyper-V hallinnan käyttäminen suunnitellun automaattisesti toissijaisen sivuston kautta.
    - Epäonnistuu VMs päälle.
    - Jälkeen on ensisijainen VMM AM on palautettu, kirjaudu sisään Azure-portaaliin -> palautus Services säilö ja suorita suunnittelematon automaattisesti, VMs toissijainen sivustosta ensisijainen-sivustoon.
    - Kun suunnittelematon vikasietotilaa on valmis, kaikkien resurssien niitä voi käyttää ensisijainen sivustosta uudelleen.


### <a name="create-a-recovery-services-vault"></a>Luo palautus palvelut-säilö

1. Kirjautuminen [Azure portal](https://portal.azure.com).
2. Valitse **Uusi** > **hallinnan** > **palautus-palvelut**. Vaihtoehtoisesti voit valitsemalla **Selaa** > **Palautus Services** vaults > **Lisää**.

    ![Uusi säilö](./media/site-recovery-vmm-to-vmm/new-vault3.png)

3. Määritä kutsumanimi tunnistavan säilö **nimi** . Jos sinulla on useita tilauksia, valitse jonkin niistä.
4. [Luo uusi resurssiryhmä](../resource-group-template-deploy-portal.md) tai valitse aiemmin luotu ja Määritä Azure alue. Tällä alueella replikoida koneet. Tarkista tuettujen alueiden artikkelissa [Azure sivuston palauttaminen hinnat tiedot](https://azure.microsoft.com/pricing/details/site-recovery/) maantieteelliset käytettävyys
4. Jos haluat käyttää nopeasti koontinäytöstä säilö napsauttamalla **raporttinäkymät-ikkunan kiinnittäminen** > **Luo säilö**.

    ![Uusi säilö](./media/site-recovery-vmm-to-vmm/new-vault-settings.png)

Uusi säilö näkyvät **raporttinäkymät-ikkunan** > **kaikille resursseille**, ja sisällön **palauttaminen Services vaults** sivu.




## <a name="getting-started"></a>Käytön aloittaminen

Sivuston palautus on aloittaminen-toiminto, jonka avulla voit ottaa käyttöön mahdollisimman pian. Aloittaminen tarkistaa edellytykset, sekä esitellään sivustojen palauttamisen käyttöönotto ohjeet ovat oikeassa järjestyksessä.

Voit valita tyyppi aloittaminen laitteiden, jonka haluat kopioida, ja mihin replikoida. Paikallisten palvelimien määrittäminen, Luo replikoinnin käytäntöjä ja kapasiteetin suunnitteleminen. Kun olet määrittänyt infrastruktuurin otetaan käyttöön VMs replikoinnin. Voit suorittaa tiettyjä koneet failovers tai luoda palautus suunnitelmien epäonnistuu useiden laitteiden päälle.

Aloita valitsemalla ensin, kuinka haluat ottaa käyttöön palauttaminen aloitusopas. Aloittaminen-työnkulku muuttuu hieman replikoinnin tarpeen mukaan.

## <a name="step-1-choose-your-protection-goal"></a>Vaihe 1: Valitse Suojaus-tavoite

Valitse haluamasi replikoida ja mihin replikoida.

1. Valitse **palautus-palveluiden vaults** sivu lisääminen säilöön ja valitse **asetukset**.
2. **Asetuksissa** > **Aloittaminen** Valitse **Sivuston palauttaminen** > **Vaihe 1: valmisteleminen infrastruktuurin** > **Suojaus tavoite**.
3. Valitse **Suojaus-tavoite** **palautus-sivustoon**ja valitse **Kyllä, jossa on Hyper-V**.
4. Valitse **Kyllä** , käytät VMM hallintaa Hyper-V-isäntien ja valitse **Kyllä** , jos sinulla on toissijainen VMM-palvelin. Jos olet käyttöönotto replikointi välillä paveikslėlis yhteen VMM palvelimeen Valitse **ei**. Valitse **OK**.

    ![Valitse tavoitteiden](./media/site-recovery-vmm-to-vmm/choose-goals.png)


## <a name="step-2-set-up-the-source-environment"></a>Vaihe 2: Lähde-ympäristön määrittäminen

Azure sivuston palauttaminen Providerin asentaminen VMM palvelimissa ja rekisteröi palvelinten säilö.

1. Valitse **Vaihe 2: valmisteleminen infrastruktuurin** > **lähde**.

    ![Tietolähteen määrittäminen](./media/site-recovery-vmm-to-vmm/goals-source.png)

2. Valitse **Valmistele lähde** **+ VMM** Lisää VMM palvelimen.

    ![Tietolähteen määrittäminen](./media/site-recovery-vmm-to-vmm/set-source1.png)

2. **Lisää palvelin** sivu-valintaruutu, joka näkyy **System Center VMM palvelimen** **Palvelintyyppi** ja että VMM palvelimen täyttää sen [edellytykset ja URL-Osoitteen vaatimukset](#on-premises-prerequisites).
4. Lataa Azure sivuston palauttaminen tarjoajan asennustiedostoa.
5. Lataa rekisteröinti-näppäintä. Tämä on asennusohjelman. Avain on voimassa viisi päivää loisi sen jälkeen.

    ![Tietolähteen määrittäminen](./media/site-recovery-vmm-to-vmm/set-source3.png)

6. Asenna Azure sivuston palauttaminen tarjoajan VMM-palvelimeen.

> [AZURE.NOTE] Ei tarvitse asentaa mitään erikseen Hyper-V host-palvelimissa.


### <a name="set-up-the-azure-site-recovery-provider"></a>Azure sivuston palauttaminen tarjoajan määrittäminen

1. Suorita palveluntarjoajan asennustiedosto VMM kussakin palvelimessa. Jos VMM on otettu käyttöön klusterin ja asennat palvelua ensimmäistä kertaa asentaminen aktiivisen solmun ja viimeistele asennus VMM palvelimen rekisteröidään säilö. Valitse Providerin asentaminen muissa solmuissa. Klusterin kaikki Suorita palveluntarjoajan samaa versiota.
2. Asennusohjelma suorittaa muutama prerequirement tarkistukset ja pyytää oikeutta VMM-palvelun pysäyttäminen. VMM-palvelu käynnistetään automaattisesti, kun asennus on valmis. Jos asennat VMM-klusterin, sinua pyydetään lopettavat klusterin rooli.

2.  **Microsoft Update** -voit valita päivitykset niin, että palvelun päivitykset on asennettu Microsoft Update-käytännön mukaisesti.
3. **Asennuksen** Hyväksy tai muokata palvelun asennuksen oletussijainti ja valitse **Asenna**.

    ![Asennuksen sijainti](./media/site-recovery-vmm-to-vmm/provider-location.png)

3. Kun asennus on valmis valitsemalla **Rekisteröi** rekisteröidä palvelimen säilö.

    ![Asennuksen sijainti](./media/site-recovery-vmm-to-vmm/provider-register.png)

9. Tarkista nimi, johon palvelimen rekisteröity säilö **säilö nimi**. Valitse *Seuraava*.

    ![Palvelimen rekisteröinti](./media/site-recovery-vmm-to-vmm-classic/vaultcred.PNG)

7. Määritä **Internet-yhteys** miten VMM palvelimessa palvelu on yhteydessä Internetiin. Valitse **Yhdistä aiemmin välityspalvelimen asetusten kanssa** käytettävä oletusarvoinen Internet-yhteyden asetukset määritetty palvelimeen.

    ![Internet-asetukset](./media/site-recovery-vmm-to-vmm-classic/proxydetails.PNG)

    - Jos haluat käyttää mukautettua välityspalvelimen olisi määritystä ennen Providerin asentaminen. Kun mukautettu välityspalvelimen asetusten määrittäminen testi toimii tarkistamaan yhteyden välityspalvelimen kautta.
    - Jos käytössäsi on mukautettu välityspalvelimen tai oletusarvo-välityspalvelimen edellyttää käyttöoikeuden tarkistusta, anna välityspalvelimen tiedot, mukaan lukien välityspalvelimen osoite ja portin.
    - Seuraavien URL-osoitteiden on oltava käytettävissä olevat VMM palvelimeen ja Hyper-v-isäntien
        - *. hypervrecoverymanager.windowsazure.com
        - *. accesscontrol.windows.net
        - *. backup.windowsazure.com
        - *. blob.core.windows.net
        - *. store.core.windows.net
    - Salli IP-osoitteiden kuvattu [Azure-palvelinkeskuksen IP-alueita](https://www.microsoft.com/download/confirmation.aspx?id=41653) ja HTTPS (443)-protokollaa. Sinun on valkoinen luettelo IP-alueita Azure alueen, jota aiot käyttää ja että Länsi US.
    - Jos käytössäsi on mukautettu välityspalvelimen VMM RunAs-tili (DRAProxyAccount) luodaan automaattisesti määritetyn käyttäjätietojen avulla. Määritä välityspalvelin, niin, että tilin voi todentaa onnistuneesti. VMM RunAs Tiliasetukset voi muokata VMM konsolissa. Voit tehdä tämän Avaa **asetukset** -työtila, laajenna **Suojaus**, **Suorita kuin**tilit ja muuta salasana DRAProxyAccount varten. Sinun on VMM-palvelun käynnistäminen uudelleen niin, että tämä asetus tulee voimaan.


8. **Rekisteröinti-näppäintä**Valitse avain Azure palauttaminen ladataan ja kopioi VMM palvelimeen.


10.  Salausasetusta käytetään vain, kun olet replikoiminen Hyper-V VMs-VMM paveikslėlis, Azure. Jos olet replikoiminen toissijainen sivustoon se ei ole käytössä.

11.  Määritä **palvelimen nimi**-tunnistavan säilö VMM-palvelimen nimi. Määritä klusterin kokoonpano VMM klusterin roolinimi.
12.  **Synkronoi cloud metatietojen** Valitse, haluatko VMM palvelimessa kaikki paveikslėlis metatietojen synkronointi säilö. Tämä toiminto on vain tapahtua kerran jokaisessa palvelimessa. Jos et halua synkronoida kaikki paveikslėlis, voit jättää tämän asetuksen valinta ja synkronoida kunkin cloud yksitellen VMM konsolissa cloud-ominaisuudet.

13.  Valitse **Seuraava** loppuun. Metatietojen VMM palvelimesta noutaa Azure palauttaminen rekisteröinnin jälkeen. Palvelimen näkyy säilö **palvelimia** -sivulla **VMM-palvelimet** -välilehden.

    ![Palvelin](./media/site-recovery-vmm-to-vmm-classic/provider13.PNG)

11. Sen jälkeen, kun palvelin on käytettävissä sivuston palautus-konsolissa **lähteen** > **valmisteleminen lähde** VMM-palvelin ja valitse pilveen, jossa Hyper-V host sijaitsee. Valitse **OK**.

#### <a name="command-line-installation"></a>Komentorivin asennus

Azure sivuston palauttaminen tarjoajan voidaan asentaa komentoriviltä. Tämän menetelmän avulla voidaan Providerin asentaminen Server Core for Windows Server 2012 R2.

1. Lataa Provider-asennuksen tiedosto- ja rekisteröintitietojen avaimen kansioon. Esimerkki C:\ASR.
2. System Center Virtual Machine Manager-palvelun pysäyttäminen.
3. Suorita järjestelmänvalvojan oikeuksin suoritettava komentokehote näiden komentojen poimia palveluntarjoajan asennusohjelma:

        C:\Windows\System32> CD C:\ASR
        C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q

4. Suorita tämä komento Providerin asentaminen:

        C:\ASR> setupdr.exe /i

5. Suorittamalla nämä komennot rekisteröidä palvelimen säilö:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>     

Jos parametrit ovat:

 - **/Credentials**: pakollinen parametri, joka määrittää sijainti, johon rekisteröinti avaimen tiedosto sijaitsee  
 - **/FriendlyName**: pakollinen parametri, joka näkyy Azure palauttaminen portaalissa Hyper-V Host (isäntä)-palvelimen nimi.
 - **/EncryptionEnabled**: valinnaisten parametrien, joita käytetään vain, kun replikoiminen from VMM Azure.
 - **/proxyAddress**: valinnainen parametri, joka määrittää välityspalvelimen osoitteen.
 - **/proxyport**: valinnainen parametri, joka määrittää välityspalvelimen porttiin.
 - **/proxyUsername**: valinnainen parametri, joka määrittää välityspalvelimen käyttäjänimi (Jos välityspalvelimen edellyttää käyttöoikeuden tarkistusta).
 - **/proxyPassword**: valinnainen parametri, joka määrittää salasanan todennustapa välityspalvelimeen (Jos välityspalvelimen edellyttää käyttöoikeuden tarkistusta).  

## <a name="step-3-set-up-the-target-environment"></a>Vaihe 3: kohdeympäristön määrittäminen

Valitse kohde VMM palvelin ja pilveen.

1. Valitse **Valmistele infrastruktuurin** > **kohde** ja valitse kohde VMM-palvelin, jota haluat käyttää.
2.  Paveikslėlis palvelimessa, jotka synkronoidaan sivuston palauttaminen ja näytetään. Valitse kohde pilveen.

    ![Kohde](./media/site-recovery-vmm-to-vmm/target-vmm.png)

## <a name="step-4-set-up-replication-settings"></a>Vaihe 4: Määrittäminen Replikointiasetukset

1. Voit luoda uuden replikoinnin käytännön valitsemalla **Valmistele infrastruktuurin** > **Replikointiasetukset** > **+ luominen ja liitä**.

    ![Verkon](./media/site-recovery-vmm-to-vmm/gs-replication.png)

2. **Luo** ja associate käytäntö määrittää käytännön nimen. Lähde- ja laji on oltava **Hyper-V**.
3. Valitse **Hyper-V host versio** käyttöjärjestelmän toimii isännän.

    > [AZURE.NOTE] VMM pilveen voi sisältää Hyper-V isännät eri (tuettu) Windows Server-versioiden, mutta replikoinnin-käytäntö on käytössä sama käyttöjärjestelmä isännät. Jos sinulla on käytössä useampi kuin yksi käyttöjärjestelmäversio isännät luoda erilliset replikoinnin käytännöt.

4. **Todennustyyppi** ja **todennus portti** Määritä kuinka liikenne todennetaan perus- ja hyödyntämistä Hyper-V Host (isäntä)-palvelinten välillä. Valitse **varmenne** , ellei sinulla ole toimiva Kerberos-ympäristössä. Azure palauttaminen määritetään automaattisesti varmenteet HTTPS-todennusta. Ei tarvitse tehdä mitään. Oletusarvoisesti Windowsin palomuurin Hyper-V host-palvelimissa avataan portti 8083 ja 8084 (varten-varmenteet). Jos valitset **Kerberos**-Kerberos-lippu käytettäviä molemminpuoleista Host (isäntä)-palvelimiin. Huomaa, että tämä asetus koskee vain Hyper-V Host (isäntä)-palvelinta Windows Server 2012 R2.
3. **Kopioi korkojakso** Määritä kuinka usein haluat replikoida delta tietojen jälkeen ensimmäisen replikoinnin (30 sekunnin välein, 5-15 minuuttia).
4. **Palautus valitsemalla säilytys**Määritä tunteina keston säilytys-ikkuna on jokaiselle palautus kohdalle. Suojatun koneet voidaan palauttaa mihin tahansa kohtaan ikkunassa.
6. **Sovelluksen yhdenmukaisia tilannevedoksen korkojakso** Määritä tiheyden (1 – 12 tuntia) palauttaminen pistettä sisältävä sovelluksen yhdenmukaisia tilannevedoksia on luotu. Hyper-V käyttää kahdenlaisia tilannevedoksia, vakio tilannevedoksen, joka sisältää koko virtuaalikoneen vaiheittainen tilannevedoksen ja sovelluksen yhdenmukaisia tilannevedoksen, joka luo ajankohta tilannevedoksen sovelluksen tietojen virtuaalikoneen sisällä. Sovelluksen yhdenmukaisia tilannevedoksia varmistaa, että sovellukset ovat yhdenmukaisia tilaan tilannevedoksen otettaessa aseman varjostus kopioi Service (VSS) avulla. Huomaa, että jos otat sovelluksen yhdenmukaisia tilannevedoksia, se vaikuttaa lähteen näennäiskoneiden sovellusten suorituskykyä. Varmista, että olet määrittänyt arvo on pienempi kuin määrität palautuksen pisteiden lukumäärä.
7. **Tietojen siirtäminen pakkaamisen** Määritä onko replikoitua tiedot, jotka siirretään pakata.
8. Valitse Määritä replikan virtuaalikoneen olisi voi poistaa, jos poistat suojauksen lähteen AM **poistaa replikan AM** . Jos tämä asetus käyttöön, kun lähteen AM, se on poistettu sivuston palautus-konsolin suojaus poistetaan käytöstä, sivuston palautusasetukset VMM poistetaan VMM-konsolin ja se poistetaan.
3. Jos olet replikoiminen verkossa Määritä, Käynnistä ensimmäisen replikoinnin vai Ajoita sen **ensimmäinen replikoinnin menetelmää** . Tallenna kaistanleveys voit haluta ajoittaa varattu työtunnit ulkopuolella. Valitse **OK**.

    ![Replikoinnin käytäntö](./media/site-recovery-vmm-to-vmm/gs-replication2.png)

6. Kun luot uuden käytännön se liittyy automaattisesti VMM pilveen. **Replikoinnin** käytännössä valitsemalla **OK**. Voit yhdistää muita VMM Paveikslėlis (ja niiden VMs) replikoinnin käytäntö **asetuksissa** > **replikoinnin** > käytännön nimi > **Liitä VMM pilveen**.

    ![Replikoinnin käytäntö](./media/site-recovery-vmm-to-vmm/policy-associate.png)

### <a name="prepare-for-offline-initial-replication"></a>Offline-tilassa ensimmäisen replikoinnin valmisteleminen

Tee offline-tilassa replikoinnin aloitustiedot kopion. Näin voit valmistella seuraavasti:

- Lähde-palvelimeen Määritä polku-sijaintiin, josta tietojen viennin tapahtuu. Määritä täydet oikeudet NTFS ja jakaminen käyttöoikeuksien Vie polulla VMM-palveluun. Kohde-palvelimeen Määritä polku-sijaintiin, josta tietojen tuominen tapahtuu. Määritä samat käyttöoikeudet tuonti-polun.
- Jos tuonti- tai polku on jaettu, määrittää järjestelmänvalvojan, tehokäyttäjän, Tulosta-operaattoria tai palvelimen ylläpitäjä Ryhmäjäsenyyden VMM palvelutilin etätietokoneessa, jossa jaetun sijaitsee.
- Jos käytössäsi on Suorita nimellä kaikki tilit, voit lisätä isännät-tuonti ja vienti polut, määrittää luku- ja kirjoitusoikeudet VMM Suorita nimellä-tilit.
- Tuo ja vie osakkeet olisi ei löydy millä tahansa tietokoneella käytetään Hyper-V palvelin, koska silmukan määritys ei tue Hyper-V.
- Active Directory-kunkin Hyper-V Host (isäntä)-palvelimeen, joka sisältää näennäiskoneiden, jonka haluat suojata, ottaminen käyttöön ja määrittäminen Rajoitettu delegointi luotetaanko etätietokoneiden, joina tuonti ja vienti polut sijaitsevat, seuraavasti:
    1. Toimialueen ohjauskoneen Avaa **Active Directoryn käyttäjät ja tietokoneet**.
    2. Valitse konsolipuussa **toimialuenimi** > **tietokoneissa**.
    3. Hyper-V host palvelimen nimeä hiiren kakkospainikkeella > **Ominaisuudet**.
    4. Valitse **delegointi** -välilehden **Luota tämän tietokoneen delegoinnin vain määritetyn palveluihin**.
    5. Napsauta **mitä tahansa todennus-protokollaa**.
    6. Valitse **Lisää** > **käyttäjät ja tietokoneet**.
    7. Tietokone, jossa vientipolku nimi > **OK**. Käytettävissä olevien palveluiden luettelosta, pidä CTRL-näppäintä painettuna ja valitse **cifs** > **OK**. Toista tietokone, jossa Tuontipolku nimi. Toista tarvittaessa muita Hyper-V host-palvelimissa.


### <a name="configure-network-mapping"></a>Määritä verkon määritys

Määrittää verkko-määrityksen lähde- ja verkkojen välillä.

- [Lue](#prepare-for-network-mapping) , mitä verkon määritys pikaisesti ei. Valitse lisäksi tarkemmin selitys [lukea](site-recovery-network-mapping.md) .
- Varmista, että näennäiskoneiden VMM palvelimissa yhteydessä verkkoon AM.

Määritä yhdistäminen seuraavasti:

1. **Asetukset** > **Sivuston palauttaminen infrastruktuurin** > **Verkon yhdistäminen** > **verkon yhdistämismääritykset** napsauttamalla **+ verkon yhdistäminen**.

    ![Verkon määritys](./media/site-recovery-vmm-to-azure/network-mapping1.png)

2. Valitse lähde **Lisää verkon määritys** -välilehti ja kohdistaa VMM-palvelimiin. AM verkkojen VMM palvelimet liittyvät tiedot noudetaan.
3. Valitse **lähde-verkko**-AM verkostojen liittyvä ensisijainen VMM palvelin-luettelosta haluamasi verkkoon.
6. Valitse **kohde verkon** haluat käyttää toissijainen VMM palvelimessa verkossa. Valitse **OK**.

    ![Verkon määritys](./media/site-recovery-vmm-to-vmm/network-mapping2.png)

Näin, mitä tapahtuu, kun verkon määritys alkaa:

- Kaikki aiemmin replikan näennäiskoneiden, jotka vastaavat tietolähteen AM verkon yhdistetään kohde AM verkkoon.
- Uusien näennäiskoneiden, joka on yhteydessä Internetiin lähde AM yhdistetään jälkeen replikoinnin määritetty kohde-verkkoon.
- Jos muokkaat aiemmin luodun uuden verkon yhdistäminen, replikan näennäiskoneiden yhdistetään käyttämällä uusia asetuksia.
- Jos jokin näistä aliverkosta on sama nimi kuin aliverkon, jossa lähde virtuaalikoneen on kohde verkossa on useita aliverkosta, valitse replikan virtuaalikoneen yhdistetään kyseisen kohteen aliverkon jälkeen automaattisesti. Jos ei ole kohteen aliverkon vastaavaa nimeä, virtuaalikoneen yhdistetään ensimmäisen aliverkon verkossa.

### <a name="configure-storage-mapping"></a>Määritä tallennustilan yhdistäminen
Oletusarvon mukaan lähde Hyper-V palvelin virtual tietokoneen replikoinnin kohde Hyper-V Host (isäntä)-palvelimeen, replikoitua tiedot on tallennettu oletussijainti, joka on merkitty kohde Hyper-V isännän Hyper-V hallinnassa. Jos haluat hallita replikoitua tietojen tallennussijainti, voit määrittää tallennustilan yhdistäminen<br/><br/> Määritä tallennustilan yhdistämistä, joudut lähteen tallennustilan luokitusten määrittäminen ja kohdistaa VMM palvelimet, ennen kuin aloitat, käyttöönotto. Tällä hetkellä tallennustilan yhdistäminen palvelun uusi Azure-portaalissa ei tueta. Se voi olla käytössä PowerShellin kautta. [Lue lisää](site-recovery-vmm-to-vmm-powershell-resource-manager.md#step-6-configure-storage-mapping).

## <a name="step-5-capacity-planning"></a>Vaihe 5: Kapasiteetin suunnittelu

Nyt kun olet luonut oman basic infrastruktuuri, voit määrittää voit ottaa huomioon kapasiteetin suunnittelu ja selvittää, tarvitsetko lisäresursseja.

Sivuston palautus on Excel-pohjaisen kapasiteetti-suunnittelu auttaa oikean resurssien varaaminen lähde-ympäristössä, sivuston palauttaminen osista, verkko ja tallennustilaa. Voit suorittaa suunnittelija arviot keskimääräinen montako VMs levyille ja tallennustilaa pikatilassa tai yksityiskohtaisessa tilassa, jossa Lisää luvut työmäärää tasolla. Ennen aloittamista sinun on:

- Kerää tietoja replikoinnin ympäristöstä, kuten VMs levyjen VMs kohden ja tallennustilaa levyä kohden.
- Arvio päivittäin muuttaminen (churn) korko on replikoitua delta tiedoille. Voit [Hyper-V replikaan kapasiteetin suunnittelu](https://www.microsoft.com/download/details.aspx?id=39057) auttaa seuraavasti.

1.  Valitse Lataa työkalu ja suorita sitten **Lataa** . [Lue artikkeli](site-recovery-capacity-planner.md) mukana toimitettava työkalu.
2.  Kun olet valmis **olet suorittanut Capacity Planner**Valitse **Kyllä** ?

    ![Kapasiteetin suunnittelu](./media/site-recovery-vmm-to-azure/gs-capacity-planning.png)

### <a name="control-bandwidth"></a>Ohjausobjektin kaistanleveyden

Kun olet kerännyt reaaliaikainen delta replikoinnin tietojen se Hyper-V kapasiteetin suunnittelu, Excel-pohjaisen kapasiteetin suunnittelutyökalua avulla voit laskea replikoinnin (alku ja delta) tarvittavan kaistanleveyden. Voivat hallita replikoinnin kaistanleveyden voit määrittää NetQos käytännön ryhmäkäytännön tai Windows PowerShellin avulla. Voit tehdä tämän muutaman tavoilla:

**PowerShellin** | **Tiedot**
--- | ---
**Uusi - NetQosPolicy "QoS kohde aliverkon"** | Rajoita liikenne käytössä Windows Server 2012 R2 toissijainen aliverkon Hyper-V isännältä. Käytä, jos ensisijainen ja toissijainen aliverkosta erot.
**Uusi - NetQosPolicy "QoS Kohdeportti"** | Rajoita liikenne käytössä Windows Server 2012 R2 Kohdeportti Hyper-V isännältä.
**Uusi - NetQosPolicy "Rajoituksen tietoliikenteen VMM"** | Rajoita vmms.exe tietoliikenteen. Tämä rajoita Hyper-V liikenteen ja Live siirto-liikenne. Voit verrata IP-protokollat ja tarkentaa portit.

Voit käyttää kaistanleveyden leveys-asetuksia tai rajoittaa liikenne bittiä toissijainen kohden. Jos käytät klusterin, sinun on asennettava kaikki klusterisolmu. Lisätietoja on artikkelissa:


- [Rajoittimen Hyper-V replikan](http://www.thomasmaurer.ch/2013/12/throttling-hyper-v-replica-traffic/) liikenteen Thomas Maurer-blogi
- Lisätietoja [Uusi NetQosPolicy cmdlet-komento](https://technet.microsoft.com/library/hh967468.aspx).


## <a name="step-6-enable-replication"></a>Vaihe 6: Ota käyttöön sallittuja

Ota nyt replikoinnin seuraavasti:

1. Valitse **Vaihe 2: replikoida sovelluksen** > **lähde**. Kun olet replikoinnin ensimmäistä kertaa, voit napsauttaa **+ replikoida** muihin tietokoneisiin replikoinnin käyttöön säilöön.

    ![Replikoinnin ottaminen käyttöön](./media/site-recovery-vmm-to-vmm/enable-replication1.png)

2. **Lähde** -sivu-> Valitse VMM-palvelin ja jossa haluat toistaa Hyper-V isännät sijaitsevat pilvessä. Valitse **OK**.

    ![Replikoinnin ottaminen käyttöön](./media/site-recovery-vmm-to-vmm/enable-replication2.png)

3. Varmista toissijaisen VMM palvelimen ja pilvi **kohde** -sivu.
4. Valitse suojattava luettelosta VMs **näennäiskoneiden** .

    ![Virtuaalikoneen suojauksen ottaminen käyttöön](./media/site-recovery-vmm-to-vmm/enable-replication5.png)

Voit seurata edistymistä **Salli suojaus** -toiminnon asetukset > **työt** > **palauttaminen työt**. Kun **Viimeisteleminen suojaus** suoritetaan virtuaalikoneen on valmiina automaattisesti.


>[AZURE.NOTE] Voit myös ottaa suojauksen näennäiskoneiden VMM konsolissa. Valitse **Ota suojaus** virtuaalikoneen ominaisuudet-työkalurivin > **Azure sivuston palautus** -välilehti.

Kun olet replikoinnin AM **asetuksissa**voit tarkastella ominaisuudet > **Replikoida kohteet** > AM nimi. **Essentials** -koontinäytössä näet tietoja AM ja sen tilan replikoinnin käytännön. Saat lisätietoja valitsemalla **Ominaisuudet** .

### <a name="onboard-existing-virtual-machines"></a>Määrän näennäiskoneiden

Jos sinulla on aiemmin näennäiskoneiden VMM-, Hyper-V replikaan replikoiminen voit siirtyvät ne Azure palauttaminen suojautumista seuraavasti:

1. Varmista Hyper-V isäntäpalvelimen aiemmin AM sijaitsee ensisijainen pilveen ja Hyper-V isäntäpalvelimen replikan virtuaalikoneen sijaitsee toissijainen pilveen.
2. Varmista, että replikoinnin käytäntö on määritetty ensisijainen VMM pilveen.
2. Ota käyttöön ensisijainen virtuaalikoneen replikoinnin. Azure palauttaminen ja VMM varmistavat saman replikan isännän ja virtuaalikoneen havaitaan ja Azure palauttaminen uudelleen ja palauttaa määritetyn asetusten replikoinnin.


## <a name="step-7-test-your-deployment"></a>Vaihe 7: Testata käyttöönoton

Voit testata käyttöönoton Suorita testi-automaattisesti yhden virtual tietokoneelle tai palautus-suunnitelman, joka sisältää vähintään yhden näennäiskoneiden.

### <a name="prepare-for-failover"></a>Automaattisesti valmisteleminen

- Voit testata täysin käyttöönoton tarvitset infrastruktuuri replikoitua koneen toimii odotetulla tavalla. Jos haluat testata Active Directory- ja DNS-voit luoda virtual machine kuin toimialueen ohjauskoneen DNS ja replikoida tämä Azure käyttämällä Azure palauttaminen. Lue lisää tekstimuodossa [testi automaattisesti Huomioitavaa Active Directorysta](site-recovery-active-directory.md#considerations-for-test-failover).
- Tässä artikkelissa annettuja ohjeita kerrotaan, kuinka voit suorittaa testin automaattisesti verkkoa. Tämä asetus, Testaa AM epäonnistuu päälle, mutta ei Testaa verkkoasetukset AM varten. [Lue lisää](site-recovery-failover.md#run-a-test-failover) siitä, että muut asetukset.
- Jos haluat suorittaa suunnittelematon automaattisesti, sen sijaan, että testi automaattisesti huomautuksen seuraavasti:

    - Olisi mahdollisuuksien Sammuta ensisijainen koneet, ennen kuin suoritat suunnittelematon automaattisesti. Näin varmistat, että sinulla ei ole lähde- ja replikan, joissa käytetään yhtä aikaa.
    - Kun suoritat suunnittelematon automaattisesti lopettaa tietojen replikoinnin suorittamisen ensisijainen koneet niin, että kaikki tiedot delta ei siirretä, kun suunnittelematon automaattisesti alkaa. Lisäksi jos suoritat suunnittelematon automaattisesti palautus-palvelupaketin se suoritetaan, kunnes valmiina, vaikka näyttöön tulee virhesanoma.




### <a name="run-a-test-failover"></a>Suorita testi automaattisesti

1. Yksittäisen AM **asetuksissa**päälle epäonnistuu > **Replikoida kohteita**, valitsemalla AM > **+ testi automaattisesti**.

    ![Testaa automaattisesti](./media/site-recovery-vmm-to-vmm/test-failover.png)

2. Epäonnistuu päälle palautus-palvelupaketti, **asetuksissa** > **Palautus-Palvelupaketit**, suunnitelman hiiren kakkospainikkeella > **Testi automaattisesti**. Luo palautuksen suunnitteleminen [seuraavien ohjeiden mukaisesti](site-recovery-create-recovery-plans.md).
2. **Testaa automaattisesti**Valitse **ei mitään**. Tällä asetuksella voit testata AM siirtyy oikein, mutta replikoitua AM ei ole yhdistetty minkä tahansa verkkoon.

    ![Testaa automaattisesti](./media/site-recovery-vmm-to-vmm/test-failover1.png)

3. Valitse **OK** , jos haluat aloittaa vikasietotilaa. Voit seurata edistymistä napsauttamalla sen ominaisuuksia AM tai **Testi automaattisesti** projektin **asetukset** > **työt** > **palauttaminen työt**.
4. Kun automaattisesti työn **valmiina testaaminen** vaihe, toimi seuraavasti:

    -  Tarkastele se AM toissijainen VMM pilveen.
    -  Valitse Viimeistele määrittäminen testi vikasietotilaa **valmiina testi** .
    -  Valitse **muistiinpanot** ja tallentaminen testi vikasietotilaa liittyvät huomautukset.

5. Testaa virtuaalikoneen luodaan samaan isännän kuin isäntä, jossa replikan virtuaalikoneen on olemassa. Se lisätään samaan pilveen, jossa replikan virtuaalikoneen sijaitsee.
6. Kun olet varmistanut, että VMs Käynnistä onnistuneesti valitsemalla **Testaa vikasietotilaa on valmis**. Tässä vaiheessa luodaan automaattisesti palauttaminen aikana testi vikasietotilaa elementtejä poistetaan.  

    > [AZURE.NOTE] Jos testi automaattisesti edelleen kestää yli kaksi viikkoa on valmis voimassa mukaan.



### <a name="update-dns-with-the-replica-vm-ip-address"></a>Päivitä DNS se AM IP-osoite

Kun automaattisesti se AM ei ehkä ole sama kuin ensisijaisen virtuaalikoneen IP-osoite.

- Näennäiskoneiden päivittää DNS-palvelimeen, joita hän käyttää sen jälkeen, kun he alkavat.
- Voit päivittää DNS myös manuaalisesti seuraavasti:

#### <a name="retrieve-the-ip-address"></a>Hae IP-osoite

Suorittamalla tämä esimerkki noutaa IP-osoite.

        $vm = Get-SCVirtualMachine -Name <VM_NAME>
        $na = $vm[0].VirtualNetworkAdapters>
        $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
        $ip.address  

#### <a name="update-dns"></a>Päivitä DNS

Suorittamalla tämän otoksen Päivitä DNS-IP-osoite, voit hakea käyttämällä Edellinen otoksen komentosarja, joka määrittää.

        string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord

## <a name="next-steps"></a>Seuraavat vaiheet

Käyttöönoton jälkeen määrittämisen jälkeen ja käytössä, [Katso](site-recovery-failover.md) lisätietoja erityyppisiä failovers.

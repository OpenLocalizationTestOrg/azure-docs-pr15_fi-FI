<properties
    pageTitle="Toistaa VMware näennäiskoneiden ja fyysiset palvelimet Azure kanssa Azure palauttaminen | Microsoft Azure"
    description="Tässä artikkelissa kuvataan, orchestrate replikoinnin, automaattisesti ja paikallisen VMware näennäiskoneiden ja Windows-tai Linux fyysinen palvelinten Azure Azure palauttaminen ottamisesta käyttöön."
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
    ms.date="09/29/2016"
    ms.author="raynew"/>

# <a name="replicate-vmware-virtual-machines-and-physical-servers-to-azure-with-azure-site-recovery"></a>Replikoida VMware näennäiskoneiden ja fyysiset palvelimet Azure kanssa Azure sivuston palauttaminen

> [AZURE.SELECTOR]
- [Azure Portal](site-recovery-vmware-to-azure.md)
- [Perinteinen Portal](site-recovery-vmware-to-azure-classic.md)
- [Perinteinen Portal (vanha)](site-recovery-vmware-to-azure-classic-legacy.md)


Azure palauttaminen-palvelun osaltaan mukaan orchestrating replikoinnin, automaattisesti ja näennäiskoneiden ja fyysiset palvelimet yrityksen liiketoiminnan jatkuvuuden ja tietojen palauttaminen (BCDR) määrittäminen. Koneet voi replikoida Azure tai toissijaisen paikallisen tietokeskuksen. Lue pikaisesti [Azure palauttaminen ominaisuudet?](site-recovery-overview.md).

## <a name="overview"></a>Yleiskatsaus

Tässä artikkelissa kuvataan, miten voit:

- **Azure voi replikoida VMware näennäiskoneiden**– ottaa käyttöön järjestämiseen replikoinnin automaattisesti ja paikallisen VMware näennäiskoneiden palauttamista Azure säilöön palauttaminen.
- **Toistaa fyysinen Azure-palvelinten**– käyttöönotto Azure sivuston palauttaminen replikoinnin automaattisesti ja paikallisen fyysinen Windowsin ja Linux-palvelinten Azure palautus järjestämiseen.

>[AZURE.NOTE] Tässä artikkelissa käsitellään replikoida Azure. Jos haluat toistaa VMware VMs tai Windows-tai Linux fyysinen palvelimia toissijainen palvelinkeskukseen, [tämän artikkelin](site-recovery-vmware-to-vmware.md)ohjeiden mukaisesti.

Kirjaa kommentteja tai kysymyksiä alaosassa on tämän artikkelin tai [Azure palautus Services keskustelupalstalle](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="enhanced-deployment"></a>Parannettu käyttöönotto

Tässä artikkelissa annetaan ohjeita parannettu käyttöönoton perinteinen Azure-portaalissa. Suosittelemme, että käytät tätä versiota kaikkien uusien versioiden. Jos olet jo asentanut versiota aiempien versioiden käyttäminen on suositeltavaa siirtäminen uuteen versioon. Lue [Lisää](site-recovery-vmware-to-azure-classic-legacy.md##migrate-to-the-enhanced-deployment) siirto.

Parannettu käyttöönoton on pää päivitys. Tässä on yhteenveto on tehty parannuksia:

- **Ei ole infrastruktuurin VMs Azure**: tietojen kopioi suoraan Azure-tallennustilan tilin. Lisäksi replikointi ja automaattisesti ei ole tarpeen voit määrittää minkä tahansa infrastruktuurin VMs (määrityspalvelimen ja perusmuodon kohde server) on tarvittaessa aiempien versioiden käyttöönotto.  
- **Yhdistetty asennus**: yksi asennus on yksinkertainen määritys ja skaalattavuus paikallisen osien.
- **Suojattu käyttöönoton**: kaikki liikenne salataan ja replikoinnin hallinta communications lähetetään HTTPS 443 kautta.
- **Palautus pisteet**: kaatumisen ja sovelluksen yhdenmukaisia palautus tuki animoiminen Windowsin ja Linux ympäristössä ja tukee sekä yksi AM ja usean AM yhdenmukaisia määrityksiä.
- **Testaa automaattisesti**: häiritsevä testi automaattisesti Azure ilman vaikuttavat tuotannon tai keskeyttäminen replikoinnin tuki.
- **Suunnittelematon automaattisesti**: Suunnittelematon automaattisesti Azure parannettu sähköpostit Sammuta VMs automaattisesti ennen automaattisesti tuki.
- **Tuntisesta**: integroitu tuntisesta, joka kopioi vain delta muutokset takaisin paikalliseen sivustoon.
- **vSphere 6.0**: rajoitettu tuki VMware Vsphere 6.0 asennuksia.


## <a name="how-does-site-recovery-help-protect-virtual-machines-and-physical-servers"></a>Miten sivuston palauttaminen auttaa suojaamaan näennäiskoneiden ja fyysiset palvelimet?


- VMware järjestelmänvalvojat voivat määrittää Azure business toiminnoista ja VMware näennäiskoneiden sovellusten sähköntuotannosta suojauksen. Palvelimen valvojat voi replikoida Azure fyysinen paikallisen Windows- ja Linux-palvelimiin.
- Azure palauttaminen konsoli tarjoaa yhden paikan yksinkertainen määritys ja replikoinnin automaattisesti ja palautus prosessit hallinta.
- Jos VMware näennäiskoneiden, joita hallitaan vCenter palvelimen replikoinnin palauttaminen voit tutustua näiden VMs automaattisesti. Jos koneet ESXi isännän palauttaminen löytää VMs isännän.
- Suorittaa helposti failovers paikallisen infrastruktuurin Azure ja tuntisesta (Palauta) azuren paikallisen sivuston VMware AM-palvelimiin.
- Määritä palautus-palvelupakettia, ryhmitellä yhteen sovelluksen toiminnoista, jotka ovat tasoisen useita tietokoneissa. Voi epäonnistua päälle näiden suunnitelmien ja sivuston palautus on usean AM yhdenmukaisuuden niin, että, joissa käytetään samaa työmääriä voidaan palauttaa yhdessä yhdenmukaisia arvopisteeseen.


## <a name="supported-operating-systems"></a>Tuetut käyttöjärjestelmät

### <a name="windows64-bit-only"></a>Windows (vain 64-bittinen)
- Windows Server 2008 R2 SP1 +
- Windows Server 2012
- Windows Server 2012 R2

### <a name="linux-64-bit-only"></a>Linux (vain 64-bittinen)
- Punainen on Enterprise Linux 6,7, 7.1, 7.2
- CentOS 6.5 6.6, 6,7, 7.0, 7.1, 7.2
- Oracle Enterprise Linux 6.4, 6.5, jossa punainen on yhteensopiva ydin tai Unbreakable yrityksen ydin versio 3 (UEK3)
- SUSE Linux Enterprise Server 11 SP3

## <a name="scenario-architecture"></a>Skenaario-arkkitehtuuri

Skenaario osat:

- **Paikallisen hallinnan palvelimen**: asiakirjanhallinnan palvelimeen suoritetaan palauttaminen osat:
    - **Määrityspalvelimen**: koordinoi viestintä ja hallitsee tietojen replikoinnin ja hyödyntämistä prosessit.
    - **Prosessin palvelimen**: replikoinnin yhdyskäytävän toimii. Se vastaanottaa tietoja suojatun lähde koneet, optimoi tallentamisesta välimuistiin, pakkaamisen ja salaus ja replikoinnin tiedot lähetetään Azure-tallennustilan. Se myös käsittelee push asennettuun suojatun koneet Mobility-palvelu ja suorittaa VMware VMs Automaattinen etsiminen.
    - **Perustyylisivujen kohdepalvelin**: käsittelee replikoinnin tietojen tuntisesta azuren aikana.
    Voit myös ottaa asiakirjanhallinnan palvelimeen, joka toimii prosessin palvelimen vain, jos haluat skaalata käyttöönoton.
- **Mobility-palvelu**: Tämä osa on otettu käyttöön Jokaisessa koneessa (VMware AM tai fyysinen palvelin), jonka haluat toistaa Azure. Se sieppaa tietojen kirjoituksia tietokoneeseen ja välittää niitä prosessi-palvelimeen.
- **Azure**: sinun ei tarvitse luoda minkä tahansa Azure VMs käsittelemään replikointi ja automaattisesti. Sivuston palauttaminen-palvelu käsittelee tietojen hallinta ja tietojen kopioi suoraan Azure-tallennustilan. Replikoitua Azure VMs ovat kehrätty määrittäminen automaattisesti vain, kun Azure automaattisesti tapahtuu. Kuitenkin halutessasi epäonnistuu takaisin Azure-paikallisen sivuston tarvitset Azure-AM määrittäminen toimimaan prosessin palvelimeksi.


Kuva esittää, kuinka nämä osat vuorovaikutuksessa.

![arkkitehtuuri](./media/site-recovery-vmware-to-azure/v2a-architecture-henry.png)

**Kuva 1: VMware/fyysisiä, Azure** (luoma Henry Robalino)


## <a name="capacity-planning"></a>Kapasiteetin suunnittelu

Kun suunnittelet kapasiteettia, tässä on tarvitset huomioon otettavia asioita:

- **Lähde-ympäristössä**, kapasiteetin suunnittelua tai VMware infrastruktuuri- ja tietolähteen koneen vaatimuksia.
- **Asiakirjanhallinnan palvelimeen**, ne paikallisen-palvelimet, suorita palauttaminen osien suunnitteleminen.
- **Kaistanleveys lähteestä kohteeseen**-kaistanleveys replikointi välillä lähde- ja Azure vaaditaan suunnitteleminen

### <a name="source-environment-considerations"></a>Tietolähteen ympäristön huomioon otettavia seikkoja

- **Muutos enintään päivittäin korko**, suojattu tietokone käyttää vain yksi prosessi-palvelin ja yksittäisen prosessin palvelimen voit käsitellä enintään 2 Teratavua tietojen muuttaminen päivässä. Näin 2 Teratavua on suurin päivittäinen tiedot muuttuvat, joka tukee suojattujen kone korko.
- **Suurin nopeus**– replikoitua koneen voivat kuulua yhden Azure-tallennustilan tilin. Vakio-tallennustilan tilin voi käsitellä enintään 20 000 pyynnöt sekunnissa ja suosittelemme, että pidät IOPS vierekkäin lähdetietokoneen 20 000. Esimerkiksi jos lähde-tietokoneessa, jossa 5 levyjen ja jokaisen levyn Luo 120 IOPS (8K koon) lähteen jälkeen se on sisällä Azure levyn IOPS raja 500 kohden. Numero pakollinen tallennustilan tilien = lähde yhteensä IOPs/20000.


### <a name="management-server-considerations"></a>Management server huomioon otettavia seikkoja

Hallintapalvelin suoritetaan palauttaminen osat, jotka koskevat tietojen optimointi toistoa ja hallinta. Pitäisi käsitellä muuta korko kapasiteettia koko kaikki työmäärät käynnissä suojatun tietokoneissa ja on riittävä kaistanleveyden replikointia jatkuvasti Azure säilöön tiedot. Tarkemmin:

- Prosessi-palvelin vastaanottaa replikoinnin tietoja suojatun koneet ja optimoi tallentamisesta välimuistiin, pakkaamisen ja salauksen, ennen kuin lähetät Azure. Asiakirjanhallinnan palvelimeen pitäisi olla näiden tehtävien suorittamiseen tarvittavat resurssit.
- Prosessi-palvelin käyttää levyn mukaan välimuistiin. On suositeltavaa erillisessä välimuistin DVD-levyllä 600 gt vähintään käsitellään verkon pullonkaula tai käyttökatkosta tallennetut muutokset. Käyttöönoton aikana voidaan määrittää välimuistin asema, jossa on vähintään 5 gt tallennustilaa, mutta 600 gt on pienin suositus.
- Paras käytäntö on suositeltavaa, että asiakirjanhallinnan palvelimeen sijaita samassa verkossa ja Lähiverkon-osiossa kuin koneet, jonka haluat suojata. Voivat sijaita eri verkossa, mutta haluat suojata tietokoneissa on oltava tason 3 verkon näkyvyyden siihen.

Seuraavassa taulukossa on koottu koon suosituksia asiakirjanhallinnan palvelimeen.

**Asiakirjanhallinnan palvelimeen Suoritin** | **Muisti** | **Levyn välimuisti** | **Tietojen muuttaminen korko** | **Suojatun koneet**
--- | --- | --- | --- | ---
8 vCPUs (2 sockets * 4 sydämiä @ 2,5 GHz) | 16 GT | 300 GT | 500 gt tai pienempi | Ota käyttöön asiakirjanhallinnan palvelimeen, replikointia alle 100 koneet asetuksilla.
12 vCPUs (2 sockets * 6 sydämiä @ 2,5 GHz) | 18 GT | 600 GT | 1 TT 500 Gigatavua | Ota käyttöön asiakirjanhallinnan palvelimeen, asetuksilla voit toistaa 100 150 laitteiden välillä.
16 vCPUs (2 sockets * 8 sydämiä @ 2,5 GHz) | 32 GIGATAVUA | 1 TT: | 1 TT – 2 TT | Ota käyttöön asiakirjanhallinnan palvelimeen, asetuksilla voit toistaa 150-200 laitteiden välillä.
Toinen prosessi Serverin käyttöönotto | | | > 2 Teratavua | Ottaa käyttöön prosessit palvelimissa, jos olet replikoiminen yli 200 koneet, tai jos päivittäiset tiedot muuttuvat korko on yli 2 Teratavua.

Jos:

- Jokaisessa lähde-koneessa on määritetty 3 levyä on 100 Gigatavua.
- On käytetty 8 SAS asemat 10 k JÄLLEENMYYNTIHINNAN esikuva tallennustilaan RAID 10 välimuistin levyn valintaikkunoihin.

### <a name="network-bandwidth-from-source-to-target"></a>Kaistanleveys lähteestä kohde
Varmista, että kaistanleveys, jotka tarvitaan ensimmäisen replikoinnin ja delta replikoinnin [kapasiteetin suunnittelutyökalua](site-recovery-capacity-planner.md) laskeminen

#### <a name="throttling-bandwidth-used-for-replication"></a>Replikoinnin käytettäviä Kaistanleveyden rajoittaminen

Azure replikoida VMware liikenne siirtyy prosessi-palvelimen kautta. Voit voit kaistanleveyden joka on käytettävissä sivuston palauttaminen replikoinnin palvelimen seuraavasti:

1. Avaa Microsoft Azure varmuuskopiointi MMC laajennuksen tärkeimmät hallinta palvelimessa tai hallinta palvelimessa on muita valmisteltu prosessi-palvelimiin. Oletusarvon mukaan pikakuvakkeen Microsoft Azure varmuuskopiointi luodaan työpöydälle tai sen löydät: C:\Program Files\Microsoft Azure palautus Services Agent\bin\wabadmin.
2. Valitse laajennuksen **Ominaisuuksien muuttaminen**.

    ![Kaistanleveyden](./media/site-recovery-vmware-to-azure-classic/throttle1.png)

3. Määritä kaistanleveyden, jota voi käyttää sivuston palauttaminen replikointi ja sovellettavan ajoituksen **Throttling** -välilehdessä.

    ![Kaistanleveyden](./media/site-recovery-vmware-to-azure-classic/throttle2.png)

Halutessasi voit myös määrittää rajoittimen PowerShellin avulla. Tässä on esimerkki:

    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth (512*1024) -NonWorkHourBandwidth (2048*1024)

#### <a name="maximizing-bandwidth-usage"></a>Kaistanleveyden käytön suurentaminen
Kun haluat lisätä Azure palauttaminen on muutettava rekisteriavaimen replikoinnin käyttämä kaistanleveys.

Seuraava rekisteriavain määrittää, kuinka kohti replikoiminen levyn viestiketjuissa siirtyminen, joita käytetään, kun replikoiminen

    HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication\UploadThreadsPerVM

 Tämän rekisteriavaimen on vaihdettava sen oletusarvoista "overprovisioned" verkossa. Sovellus tukee enintään 32.  


[Lue lisää](site-recovery-capacity-planner.md) siitä yksityiskohtainen kapasiteetin suunnittelua.

### <a name="additional-process-servers"></a>Prosessit-palvelimet

Jos haluat suojata yli 200 koneet tai päivittäinen muuta kurssi on suurempi 2 Teratavua, voit lisätä muita-palvelinten kuormituksen käsittelyssä. Jos haluat skaalata, voit tehdä seuraavia toimia:

- Lisää hallinta-palvelimiin. Voit esimerkiksi suojata enintään 400 koneet, jonka kaksi hallinta palvelimessa.
- Lisää prosessit-palvelimia ja käytä näitä käsitellään liikenteen sijaan (tai lisäksi) asiakirjanhallinnan palvelimeen.

Tässä taulukossa on kuvattu tilanne, jossa:

- Voit määrittää alkuperäisen asiakirjanhallinnan palvelimeen voi käyttää sitä vain määrityspalvelimen.
- Voit määrittää prosessit-palvelimeen.
- Voit määrittää suojatun näennäiskoneiden käyttämään prosessit-palvelimeen.
- Jokaisessa suojatun lähde-koneessa on määritetty kolme levyä on 100 Gigatavua.

**Alkuperäinen asiakirjanhallinnan palvelimeen**<br/><br/>(määrityspalvelimen) | **Prosessit-palvelin**| **Levyn välimuisti** | **Tietojen muuttaminen korko** | **Suojatun koneet**
--- | --- | --- | --- | ---
8 vCPUs (2 sockets * 4 sydämiä @ 2,5 GHz), 16 Gigatavun muistia | 4 vCPUs (2 sockets * 2 sydämiä @ 2,5 GHz), 8 Gigatavun muistia | 300 GT | 250 gt tai pienempi | Voit toistaa 85 tai vähemmän.
8 vCPUs (2 sockets * 4 sydämiä @ 2,5 GHz), 16 Gigatavun muistia | 8 vCPUs (2 sockets * 4 sydämiä @ 2,5 GHz)-12 Gigatavun muistia | 600 GT | 1 TT 250 Gigatavua | Voit toistaa 85 150 laitteiden välillä.
12 vCPUs (2 sockets * 6 sydämiä @ 2,5 GHz), 18 Gigatavun muistia | 12 vCPUs (2 sockets * 6 sydämiä @ 2,5 GHz) 24 Gigatavun muistia | 1 TT: | 1 TT – 2 TT | Voit toistaa 150 225 laitteiden välillä.


Tapa, jossa skaalata palvelinten määräytyvät yhteystietohakutulosten asteikolla ylös tai skaalata mallin ulos.  Skaalata muutaman tehokkaimpia hallinta ja prosessin palvelinten käyttöönotto tai skaalata ulos käyttöönotto palvelimien vähemmän tiedoilla. Esimerkki: Jos haluat suojata 220 koneet voi tehdä jommankumman seuraavista:

- Määritä alkuperäinen asiakirjanhallinnan palvelimeen 12vCPU, 18 gigatavun (gt) muistia, prosessit-palvelimen kanssa 12vCPU, 24 Gigatavua muistia, ja määrittää suojatun koneet käyttämään vain prosessit-palvelin.
- Tai vaihtoehtoisesti voi määrittää kahden hallinta servers (2 x 8vCPU, 16 gt RAM-Muistia) ja kaksi prosessit-servers (x 8vCPU 1) ja 4vCPU x 1 käsittelemään 135 + 85 (220) koneet ja määrittää suojatun koneet käyttämään vain prosessit-palvelimia.


Prosessit-palvelimen määrittäminen [seuraavien ohjeiden mukaisesti](#deploy-additional-process-servers) .

## <a name="before-you-start-deployment"></a>Ennen kuin aloitat, käyttöönotto

Taulukoissa on yhteenveto edellytyksistä käyttöönotto Tämä skenaario.


### <a name="azure-prerequisites"></a>Azure edellytykset

**Edellytyksenä** | **Tiedot**
--- | ---
**Azure-tili**| Tarvitset [Microsoft Azure](https://azure.microsoft.com/) -tiliin. Voit aloittaa [maksuttoman kokeiluversion käyttäjäksi](https://azure.microsoft.com/pricing/free-trial/). [Lue lisää](https://azure.microsoft.com/pricing/details/site-recovery/) siitä, että sivuston palauttaminen hinnat.
**Azure-tallennustilan** | Tarvitset Azure-tallennustilan tilin replikoitua tietojen tallentamiseen. Replikoitua tiedot tallennetaan Azure tallennustilan ja Azure VMs kehrätty määrittäminen, kun vikasietotila ilmenee. <br/><br/>Sinun on [Vakio geo tarpeettomat tallennustilan tilin](../storage/storage-redundancy.md#geo-redundant-storage). Tili on olla samalla alueella palauttaminen-palvelu ja olla sama tilaukseen. Huomaa, että replikoinnin premium-tallennustilan tilin ei tällä hetkellä tueta ja ei kannata käyttää.<br/><br/>Siirrä tallennustilan tilien luotu [uuden Azure portaalin](../storage/storage-create-storage-account.md) resurssin ryhmien ei tueta. [Lue lisää](../storage/storage-introduction.md) Azure-tallennustilan.<br/><br/>
**Azure verkossa** | Tarvitset Azure virtual verkko, jossa Azure VMs muodostaa yhteyden, kun vikasietotila ilmenee. Azure virtual verkko on oltava sama kuin sivuston palauttaminen säilö alue.<br/><br/>Huomaa, että tarvitset epäonnistuu jälkeen takaisin Azure automaattisesti VPN-yhteyden (tai Azure ExpressRoute) määrittää Azure verkosta paikallisen sivuston.


### <a name="on-premises-prerequisites"></a>Paikallisen edellytykset

**Edellytyksenä** | **Tiedot**
--- | ---
**Asiakirjanhallinnan palvelimeen** | Sinun on paikallisen Windows 2012 R2-palvelimen virtuaalikoneen tai fyysinen palvelimelle. Kaikkien osien paikallisen sivuston palautus on asennettu hallintapalvelin<br/><br/> Suosittelemme, että otat palvelimeen, on erittäin käytettävissä VMware AM. Paikallisen sivuston tuntisesta azuren on aina, riippumatta siitä, onko VMs VMware voit epäonnistua VMs tai fyysiset palvelimet. Jos et määritä Management server kuin VMware AM tarvitset määritetty erillisen perusmuodon kohdepalvelimen VMware AM vastaanottamaan tuntisesta tietoliikennettä.<br/><br/>Palvelin ei saa olla toimialueen ohjauskoneen.<br/><br/>Palvelin on staattinen IP-osoite.<br/><br/>Host (isäntä)-palvelimen nimen on oltava 15 merkkiä tai vähemmän.<br/><br/>Käyttöjärjestelmän kieli on oltava vain englannin kielen.<br/><br/>Hallintapalvelin edellyttää internet-yhteyttä.<br/><br/>Tarvitset lähtevän palvelimen käytön seuraavasti: HTTP 80 tilapäinen käyttöön määritettäessä palauttaminen osia (lataamaan MySQL). Jatkuva HTTPS 443 lähtevän käytön replikoinnin hallinnan; Jatkuva HTTPS 9443 lähtevä käytön replikoinnin tietoliikenteen (portin voi muokata)<br/><br/> Varmista, että nämä URL-osoitteet ovat käytettävissä olevat asiakirjanhallinnan palvelimeen: <br/>- \*. hypervrecoverymanager.windowsazure.com<br/>- \*. accesscontrol.windows.net<br/>- \*. backup.windowsazure.com<br/>- \*. blob.core.windows.net<br/>- \*. store.core.windows.net<br/>-https://www.msftncsi.com/ncsi.txt<br/>- [https://dev.MySQL.com/Get/Archives/MySQL-5.5/MySQL-5.5.37-Win32.msi]( https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi " https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi")<br/><br/>Jos sinulla on IP-osoite-pohjainen palomuurisäännöt palvelimessa, tarkista sääntöjä Salli Azure tietoliikenne. Tarvitset sallimaan [Azure-palvelinkeskuksen IP-alueita](https://www.microsoft.com/download/details.aspx?id=41653) ja HTTPS (443)-porttiin. Sinun on myös valkoinen luettelo IP-osoitealueet tilauksen Azure alue ja Länsi US. URL-osoite [https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi](https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi " https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi") on MySQL lataamista varten.
**VMware vCenter/ESXi Host (isäntä)**: | Tarvitset vähintään yksi vMware vSphere ESX/ESXi hypervisors hallinta VMware näennäiskoneiden ESX/ESXi 6.0, 5.5 tai 5.1 käytön uusimmat päivitykset.<br/><br/> Suosittelemme, että otat VMware vCenter palvelimen hallittavan ESXi isännät. Se on suoritettava vCenter versio 6.0 tai 5.5 ja viimeisimmät päivitykset.<br/><br/>Huomaa, että sivuston palauttaminen ei tue uuden vCenter vSphere 6.0 ominaisuuksia, kuten toimintojen vCenter vMotion virtual asemat ja tallennustilaa DRS. Sivuston palauttaminen tuki on rajoitettu ominaisuuksia, jotka olivat käytettävissä versiossa 5.5.
**Suojattu koneet**: | **AZURE**<br/><br/>Koneet, jonka haluat suojata olisi mukaisia [Azure edellytykset](site-recovery-best-practices.md#azure-virtual-machine-requirements) Azure VMs luomiseen.<br><br/>Jos haluat muodostaa yhteyden Azure VMs automaattisesti jälkeen täytyy etätyöpöydän yhteydet paikallinen palomuuri käyttöön.<br/><br/>Yksittäisten levyn kapasiteetti suojatun tietokoneissa ei kannata olla yli 1023 Gigatavua. AM voi olla enintään 64 levyjen (näin enintään 64 TT). Jos sinulla on suurempi kuin 1 TT kannattaa käyttää tietokannan replikoiminen, kuten SQL Server aina käyttöön tai Oracle tietojen suojaa.<br/><br/>Vähintään 2 gt käytettävissä olevan tilan komponentin asennus asennukseen.<br/><br/>Jaettu levyn Vieras klustereiden ei tueta. Jos sinulla on liitetty käyttöönoton kannattaa käyttää tietokannan replikoiminen, kuten SQL Server aina käyttöön tai Oracle tietojen suojaa.<br/><br/>Yhdistetty Extensible laitteisto Interface (UEFI) / Extensible laitteisto Interface(EFI) uudelleenkäynnistys ei tueta.<br/><br/>Nimiksi pitäisi olla 1 – 63 merkkiä (kirjaimia, numeroita ja väliviivoja). Nimi on alkaa kirjaimen tai numeron ja kirjaimen tai numeron perässä. Kun kone suojataan voit muuttaa Azure nimi.<br/><br/>**VMware VMs**<br/><br>Sinun on asennettava VMware vSphere PowerCLI 6.0. Valitse management server (määrityspalvelimen).<br/><br/>VMware VMs suojattava pitäisi olla VMware Työkalut asennettu ja käytössä.<br/><br/>Jos lähde AM on NIC teaming se muunnetaan yhden NIC jälkeen Azure automaattisesti.<br/><br/>Jos suojatun VMs iSCSI-levyltä sitten palauttaminen muuntaa suojatun AM iSCSI levyn .vhd-tiedoston kun AM epäonnistuu päälle Azure. Jos iSCSI-kohteen voi siirtyä sellaisen Azure AM se iSCSI-kohteen yhdistäminen ja katso itse asiassa kaksi levyä – Azure AM Näennäiskiintolevyn levy ja tietolähteen iSCSI-levyltä. Tässä tapauksessa sinun on Katkaise yhteys, joka näkyy päälle Azure AM epäonnistunutta iSCSI-kohteen.<br/><br/>[Lue lisää](#vmware-permissions-for-vcenter-access) siitä, VMware käyttöoikeuksien palauttaminen tarvitsemat.<br/><br/> **WINDOWS SERVER koneet (Valitse VMware AM tai fyysinen palvelin)**<br/><br/>Palvelimessa käytössä on tuettu 64-bittinen käyttöjärjestelmä: Windows Server 2012 R2, Windows Server 2012: ssa tai Windows Server 2008 R2 ja milloin vähintään SP1.<br/><br/>Käyttöjärjestelmä on asennettava C:\ asemassa ja OS levy on oltava Windows tavallinen DVD-levyllä (OS ei kannata voi asentaa Windows dynaamisen levyn.)<br/><br/>Windows Server 2008 R2-tietokoneissa on pystyttävä muodostamaan .NET Framework 3.5.1 asennettuna.<br/><br/>Sinun on annettava järjestelmänvalvojatilin (paikallisen järjestelmänvalvojan Windows-tietokoneessa on oltava) push-asennuksen Mobility-palvelu Windows-palvelimiin. Jos sinun on käyttäjän etäkäyttöpalvelimen ohjausobjektin paikallisessa tietokoneessa käytöstä toimialue tili on annettu tili. [Lue lisää](#install-the-mobility-service-with-push-installation).<br/><br/>Sivuston palauttaminen tukee VMs RDM levy.  Aikana tuntisesta palauttaminen käyttää RDM levyn Jos alkuperäinen lähde AM ja RDM levy on käytettävissä. Jos ne eivät ole käytettävissä, aikana tuntisesta palauttaminen luo uuden VMDK tiedoston kunkin levyn.<br/><br/>**LINUX KONEET**<br/><br/>Sinun on tuettu 64-bittinen käyttöjärjestelmä: punainen on Enterprise Linux 6,7; Centos 6.5, 6.6,6.7; Oracle Enterprise Linux 6.4, punainen on yhteensopiva ydin tai Unbreakable yrityksen ydin versio 3 (UEK3) SUSE Linux Enterprise Server 11 SP3 6.5.<br/><br/>suojatun tietokoneissa /ETC/Hosts tiedostojen pitäisi näkyä tapahtumat, jotka vastaavat paikallisen isäntänimi liittyvät kaikkien verkkosovittimien IP-osoitteet. <br/><br/>Jos haluat muodostaa yhteyden Azure virtual-tietokoneessa, jossa Linux automaattisesti suojattu runko Client-asiakkaalla (ssh) jälkeen, varmista, että suojatun tietokoneen suojattu runko-palvelu on määritetty alkamaan automaattisesti järjestelmän käynnistyksen ja palomuurisäännöt salli ssh yhteyden siihen.<br/><br/>Suojaus on käytössä vain Linux koneet seuraavat tallentamiseen: tiedostojärjestelmän (EXT3, ETX4, ReiserFS, XFS); Monipolku ohjelmisto-laite Mapper (monipolku)). Levyn hallinta: (LVM2). HP CCISS ohjauskoneen tallennustilan fyysinen palvelinten ei tueta. ReiserFS tiedostojärjestelmän tuetaan vain SUSE Linux Enterprise Server 11 SP3.<br/><br/>Sivuston palauttaminen tukee VMs RDM levy.  Sivuston palauttaminen ei aikana tuntisesta Linux varten, käyttää RDM levy. Sen sijaan se luo uuden VMDK tiedoston kunkin vastaavan RDM levylle.

Vain Linux AM - Varmista, että määrität disk.enableUUID=true asetus määritysten parametrit-VMware AM. Jos rivi ei ole, lisää se. Tämä edellyttää lisää yhdenmukaista UUID VMDK, jolloin liittää oikein. Huomaa, että myös ilman tämän asetuksen, tuntisesta aiheuttaa täydellisen latauksen, vaikka AM on käytettävissä prem.- Tämän asetuksen lisääminen varmistat, että vain delta muutokset siirretään takaisin tuntisesta aikana.

## <a name="step-1-create-a-vault"></a>Vaihe 1: Luo säilöön

1. Kirjautuminen [hallinta-portaalin](https://manage.windowsazure.com/).
2. Laajenna- **Tietopalvelut** > **Palautus Services** ja sitten **Sivuston palauttaminen säilö**.
3. Valitse **Luo uusi** > **nopea luominen**.
4. Kirjoita **nimi**-kenttään kutsumanimi tunnistavan säilö.
5. Valitse **alue**-säilö maantieteellinen alue. Tarkista tuettujen alueiden artikkelissa [Azure sivuston palauttaminen hinnat tiedot](https://azure.microsoft.com/pricing/details/site-recovery/) maantieteelliset käytettävyys
6. Valitse **Luo säilö**.
    ![Uusi säilö](./media/site-recovery-vmware-to-azure-classic/quick-start-create-vault.png)

Valitse Vahvista, että säilö on luotu tilarivillä. Säilö merkitään **aktiiviseksi** pääsivulta **Palautus-palvelut** .

## <a name="step-2-set-up-an-azure-network"></a>Vaihe 2: Määritä Azure verkko

Määritä Azure verkko, niin, että Azure VMs yhdistetään verkon jälkeen automaattisesti ja siten, että tuntisesta paikallisen sivuston toimii odotetulla tavalla.

1. Azure-portaalissa > **Luo virtuaalisia verkon** määrittäminen verkkonimi. IP address alue- ja aliverkon nimi.
2. On lisättävä VPN/ExpressRoute verkkoon, jos sinun on suoritettava tuntisesta. VPN/ExpressRoute voidaan lisätä verkon vaikka automaattisesti.

[Lue lisää](../virtual-network/virtual-networks-overview.md) Azure verkot.

> [AZURE.NOTE] [Siirron verkostojen](../resource-group-move-resources.md) saman tilauksen piiriin kuuluvien resurssien ryhmien tai tilauksissa ei tue verkkojen käytettäviä sivustojen palauttamisen käyttöönotto.

## <a name="step-3-install-the-vmware-components"></a>Vaihe 3: Asenna VMware osat

Jos haluat toistaa VMware virtual koneet Asenna asiakirjanhallinnan palvelimeen VMware seuraavat osat:

1. [Lataa](https://developercenter.vmware.com/tool/vsphere_powercli/6.0) ja asenna VMware vSphere PowerCLI 6.0.
2. Käynnistä-palvelin.


## <a name="step-4-download-a-vault-registration-key"></a>Vaihe 4: Lataa säilö rekisteröinti-avain

1. Johdon palvelimen Avaa sivuston palautus-konsolin Azure-tietokannassa. Valitse Pika-aloitus-sivun avaamiseen säilö **Palautus palveluita** -sivulle. Pikaopas voi avata myös milloin tahansa napsauttamalla kuvaketta.

    ![Pika-aloitusopas-kuvake](./media/site-recovery-vmware-to-azure-classic/quick-start-icon.png)

2. Napsauta **Pika-aloitus** -sivulla **Valmisteleminen kohde resurssien** > **Lataa rekisteröinti-näppäintä**. Rekisteröity-tiedosto luodaan automaattisesti. Se on voimassa viisi päivää sen luonnin jälkeen.


## <a name="step-5-install-the-management-server"></a>Vaihe 5: Asenna asiakirjanhallinnan palvelimeen
> [AZURE.TIP] Varmista, että nämä URL-osoitteet ovat käytettävissä olevat asiakirjanhallinnan palvelimeen:
>
- *. hypervrecoverymanager.windowsazure.com
- *. accesscontrol.windows.net
- *. backup.windowsazure.com
- *. blob.core.windows.net
- *. store.core.windows.net
- https://dev.MySQL.com/Get/Archives/MySQL-5.5/MySQL-5.5.37-Win32.msi
- https://www.msftncsi.com/ncsi.txt




[AZURE.VIDEO enhanced-vmware-to-azure-setup-registration]

1. **Pika-aloitus** -sivulla Lataa palvelimeen yhdistetty asennustiedostoa.

2. Aloita asennus sivuston palauttaminen yhdistetyn ohjatun asennustiedoston suorittaminen

3.  Valitse **ennen kuin aloitat** , **Asenna määrityspalvelimen ja prosessin palvelimeen**.

    ![Ennen aloittamista](./media/site-recovery-vmware-to-azure-classic/combined-wiz1.png)
4. Valitse **hyväksyn** ladattava ja asennettava MySQL **Kolmannen osapuolen ohjelmistokäyttöoikeus** . 

    ![= Kolmannen osapuolen ohjelmisto](./media/site-recovery-vmware-to-azure-classic/combined-wiz105.PNG)

5. **Rekisteröimiseen** Selaa ja valitse latasit säilö rekisteröinti-näppäintä.

    ![Rekisteröinti](./media/site-recovery-vmware-to-azure-classic/combined-wiz3.png)

6. Määritä **Internet-asetukset** miten määritys-palvelimella tarjoaja muodostaa Azure palauttaminen Internetin välityksellä.

    - Jos haluat muodostaa yhteyden välityspalvelin, joka on tällä hetkellä määritetty tietokoneeseen valitsemalla **Yhdistä aiemmin välityspalvelimen asetusten kanssa**.
    - Jos haluat muodostaa yhteyden suoraan palveluntarjoajan Valitse **Yhdistä suoraan ilman välityspalvelinta**.
    - Jos aiemmin välityspalvelimen edellyttää käyttöoikeuden tarkistusta tai haluat käyttää mukautettua välityspalvelimen toimittaja-yhteyden, valitse **Yhdistä mukautetun välityspalvelimen asetusten kanssa**.
        - Jos käytössäsi on mukautettu välityspalvelimen, sinun on määritettävä osoite, portti ja tunnistetiedot
        - Jos käytät välityspalvelinta olisi on jo sallittu seuraavat URL-osoitteet:
            - *. hypervrecoverymanager.windowsazure.com;    
            - *. accesscontrol.windows.net; 
            - *. backup.windowsazure.com; 
            - *. blob.core.windows.net; 
            - *. store.core.windows.net
            

    ![Palomuurin](./media/site-recovery-vmware-to-azure-classic/combined-wiz4.png)

7. **Edellytykset Tarkista** asennus suoritetaan tarkistusta ja varmista, että asennuksen suorittamisen. 

    
    ![Edellytykset](./media/site-recovery-vmware-to-azure-classic/combined-wiz5.png)

     Jos näyttöön tulee varoitus, tietoja **yleisen aika Synkronoi tarkistaminen** Tarkista, aika-asetus järjestelmän kelloa (**päivämäärä ja aika** -asetukset) on sama kuin aikavyöhyke.

    ![TimeSyncIssue](./media/site-recovery-vmware-to-azure-classic/time-sync-issue.png)

8. Luo tunnistetiedot, jotka asennetaan MySQL server-esiintymän kirjaudutaan **MySQL-määritys** .

    ![MySQL](./media/site-recovery-vmware-to-azure-classic/combined-wiz6.png)

9. Valitse **Ympäristön tiedot** onko aiot replikoida VMware VMs. Jos olet, asennusohjelma tarkistaa, että PowerCLI 6.0 on asennettu.

    ![MySQL](./media/site-recovery-vmware-to-azure-classic/combined-wiz7.png)

10. **Asenna sijainti** Valitse kohtaa, johon haluat asentaa binaaritiedostoja ja tallentaa välimuistiin. Voit valita aseman, jossa on vähintään 5 gt tallennustilaa, mutta suosittelemme välimuisti-asema, jossa on vähintään 600 Gigatavua vapaata tilaa.

    ![Asennuksen sijainti](./media/site-recovery-vmware-to-azure-classic/combined-wiz8.png)

11. Määritä **Verkonvalinta** kuuntelua (verkkosovittimen ja SSL-portti), configuration-palvelin lähettää ja vastaanottaa replikoinnin tiedot. Voit muuttaa oletusarvoista portti (9443). Lisäksi tämän portin verkkopalvelin joka orchestrates replikoinnin toimintoja käytetään porttiin 443. vastaanottamiseen replikoinnin liikenne ei kannata käyttää 443.


    ![Verkonvalinta](./media/site-recovery-vmware-to-azure-classic/combined-wiz9.png)



12.  **Yhteenvedossa** Tarkista tiedot ja valitse **Asenna**. Kun asennus on valmis salasana luodaan. Sinun on se kun otat toistoa siten kopioi se ja pitää sen turvalliseen paikkaan.

    ![Yhteenveto](./media/site-recovery-vmware-to-azure-classic/combined-wiz10.png)



13.  Tarkista tiedot **yhteenvedossa** .

    ![Yhteenveto](./media/site-recovery-vmware-to-azure-classic/combined-wiz10.png)

>[AZURE.WARNING]Microsoft Azure palautus Tukiedustajan 's välityspalvelimen on oltava asetukset.
>Kun asennus on valmis Käynnistä sovelluksen nimi "Microsoft Azure palautus Services Shell" Windowsin Käynnistä-valikosta. Komento-ikkunasta, joka avaa suorittavat seuraavat komennot välityspalvelimen asetusten määrittäminen.
>
    $pwd = ConvertTo-SecureString -String ProxyUserPassword
    Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumb – ProxyUserName domain\username -ProxyPassword $pwd
    net stop obengine
    net start obengine



### <a name="run-setup-from-the-command-line"></a>Asennusohjelman komentoriviltä

Voit suorittaa myös yhdistetty ohjattu komentoriviltä, seuraavasti:

    UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address to be used for data transfer] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>]

Jos:

- / ServerMode: pakollinen. Määrittää, onko asennusta Asenna määritys ja prosessi-palvelimia tai vain prosessin server (avulla Asenna prosessit-palvelimet). Kirjoita arvot: CS PS.
- InstallDrive: pakollinen. Määrittää kansio, johon on asennettu osat.
- / MySQLCredFilePath. Pakollinen. Määrittää, missä MySQL server-tunnistetiedot ovat Tarinan tiedoston polku. Hae tiedosto malli.
- / VaultCredFilePath. Pakollinen. Säilö tunnistetiedot-tiedoston sijainti
- / EnvType. Pakollinen. Asennuksen tyyppi. Arvot: VMware NonVMware
- / PSIP ja /CSIP. Pakollinen. Prosessi-palvelimen nimen ja määrityspalvelimen IP-osoite.
- / PassphraseFilePath. Pakollinen. Salasana-tiedoston sijainti.
- / ByPassProxy. Valinnainen. Määrittää asiakirjanhallinnan palvelimeen yhteyden Azure ilman välityspalvelinta.
- / ProxySettingsFilePath. Valinnainen. Määrittää mukautetun välityspalvelimen (oletusvälityspalvelimen todennusta vaativaa palvelimessa) tai mukautettu välityspalvelimen asetukset




## <a name="step-6-set-up-credentials-for-the-vcenter-server"></a>Vaihe 6: Määritä vCenter palvelimen tunnistetiedot

> [AZURE.VIDEO enhanced-vmware-to-azure-discovery]

Prosessin palvelimen löytävät VMware VMs, joita hallitaan vCenter-palvelimen automaattisesti. Automaattinen etsiminen sivuston palautus on tili ja tunnistetiedot, jotka voivat käyttää vCenter palvelimen. Tämä ei ole merkitystä, jos olet replikoiminen fyysinen palvelimiin.

Tehdä seuraavasti:

1. VCenter server Luo rooli (**Azure_Site_Recovery**) vCenter tasolla [tarvittavat käyttöoikeudet](#vmware-permissions-for-vcenter-access).
2. Määritä **Azure_Site_Recovery** -roolin vCenter-käyttäjälle.

    >[AZURE.NOTE] VCenter käyttäjätili, jolla on vain luku-rooli voi suorittaa automaattisesti ilman suojatun lähde koneet suljetaan. Jos haluat sulkea kyseiset koneet tarvitset Azure_Site_Recovery rooli. Huomaa, että jos käytössäsi on vain siirtyminen VMs VMware Azure ei tarvitse tuntisesta sitten vain luku-roolin on riittävä.

3. Jos haluat lisätä tilin Avaa **cspsconfigtool**. Se on käytettävissä pikakuvakkeen työpöydälle nimellä ja sijaitsevat [asentaa sijainti] \home\svsystems\bin-kansiossa.
2. **Tilien hallinta** -välilehti Valitse **Lisää tili**.

    ![Tilin lisääminen](./media/site-recovery-vmware-to-azure-classic/credentials1.png)

3. Lisää tunnistetiedot, jotka voidaan käyttää vCenter-palvelinta **Tilin tiedot** . Huomaa, että se voi kestää yli 15 minuuttia, tilin nimen näkyvän portaalissa. Päivitä heti, valitse Päivitä **Määritys-palvelimet** -välilehden.

    ![Tiedot](./media/site-recovery-vmware-to-azure-classic/credentials2.png)

## <a name="step-7-add-vcenter-servers-and-esxi-hosts"></a>Vaihe 7: Lisää vCenter palvelimia ja ESXi isännöi

Jos olet replikoiminen VMware VMs, sinun on lisättävä vCenter server (tai ESXi isäntä).

1. **Palvelinten** > **Configuration Servers** -välilehdestä määrityspalvelimen > **Lisää vCenter palvelin**.

    ![vCenter](./media/site-recovery-vmware-to-azure-classic/add-vcenter1.png)

2. Lisää vCenter palvelimeen tai ESXi host tiedot, määritetty käyttämään edellisessä vaiheessa vCenter-palvelin ja prosessin palvelimeen, jota käytetään saat tietää, joita hallitaan vCenter palvelimen VMware VMs tilin nimi. Huomaa, että vCenter palvelimeen tai ESXi host pitäisi sijaita samassa verkossa kuin palvelin, johon on asennettu prosessi-palvelimeen.

    >[AZURE.NOTE] Jos olet lisäämässä vCenter palvelimeen tai ESXi host tilille, jolla ei ole järjestelmänvalvojan oikeuksia vCenter tai Host (isäntä)-palvelimeen, varmista, että vCenter tai ESXi-tileillä on nämä oikeudet käytössä: Palvelinkeskukseen, Datastore, kansion, Jost, verkon, resurssi, Virtual koneen, vSphere hajautettu valitsinta. Lisäksi vCenter palvelin on tallennustilan näkymät-oikeus.

    ![vCenter](./media/site-recovery-vmware-to-azure-classic/add-vcenter2.png)

3. Etsinnän päätyttyä vCenter palvelimen listataan **Määritys-palvelimet** -välilehti.

    ![vCenter](./media/site-recovery-vmware-to-azure-classic/add-vcenter3.png)


## <a name="step-8-create-a-protection-group"></a>Vaihe 8: Luo suojaus-ryhmä

> [AZURE.VIDEO enhanced-vmware-to-azure-protection]


Suojaus-ryhmät ovat loogisesti ryhmiteltyjä näennäiskoneiden tai fyysinen palvelimissa, jotka haluat suojata käyttämällä samaa suojausasetuksia. Haluat käyttää suojausasetuksia suojaus-ryhmä ja nämä asetukset otetaan käyttöön kaikissa näennäiskoneiden/fyysisiä-tietokoneissa, voit lisätä ryhmään.

1. Avaa **Suojattu kohteet** > **Suojaus-ryhmä** ja valitse Lisää suojaus-ryhmä.

    ![Luo suojaus-ryhmä](./media/site-recovery-vmware-to-azure-classic/protection-groups1.png)

2. **Määritä ryhmäasetukset** -sivulla ryhmän nimi ja valitse **-** määrityspalvelimen, johon haluat luoda ryhmän. **Kohde** on Azure.

    ![Ryhmän suojausasetuksia](./media/site-recovery-vmware-to-azure-classic/protection-groups2.png)

3. **Määritä toistoa asetukset** -sivulla, jota käytetään kaikissa tietokoneissa ryhmässä replikoinnin-asetusten määrittäminen.

    ![Suojauksen ryhmän sallittuja](./media/site-recovery-vmware-to-azure-classic/protection-groups3.png)

    - **Usean AM yhdenmukaisuuden**: Jos otat tämän luomilleen jaetun sovelluksen yhdenmukaisia palautus pisteiden suojaus-ryhmä-tietokoneissa. Tämä asetus on tärkeimpiä, kun kaikki suojaus-ryhmä koneet on käytössä sama työmäärää. Kaikissa tietokoneissa palautetaan saman arvopisteeseen. Tämä on käytettävissä, olet replikoiminen VMware VMs tai Windows-tai Linux fyysinen palvelimia.
    - **RPO kynnysarvo**: määrittää RPO. Ilmoitusten luodaan, kun jatkuvan tietojen suojauksen replikoinnin ylittää määritetyn RPO raja-arvo.
    - **Palautus valitsemalla säilytys**: määrittää säilytys-ikkuna. Suojatun koneet voidaan palauttaa mihin tahansa kohtaan tässä ikkunassa.
    - **Sovelluksen yhdenmukaista tilannevedoksen korkojakso**: määrittää, kuinka usein palautus pistettä sisältävä sovelluksen yhdenmukaista tilannevedoksia luodaan.

Kun napsautat valintamerkki suojaus-ryhmä luodaan määrittämäsi nimi. In addition toisen suojaus-ryhmä on luotu nimellä < suojaus-ryhmä-nimi-tuntisesta). Tämä suojaus-ryhmä on käytössä, jos epäonnistua takaisin paikalliseen sivustoon jälkeen Azure automaattisesti. Voit valvoa suojaus-ryhmiä, kun luomiaan **Suojatut osat** -sivulla.

## <a name="step-9-install-the-mobility-service"></a>Vaihe 9: Mobility-palvelun asentaminen

Ensimmäinen vaihe näennäiskoneiden ja fyysiset palvelimet suojauksen ottaminen käyttöön on Mobility-palvelun asentaminen. Voit tehdä tämän kahdella tavalla:

- Push- ja -palvelun asentaminen tietokoneeseen kunkin prosessin palvelimesta automaattisesti. Huomaa, että kun lisäät koneet suojaus-ryhmä ja ne ovat jo Mobility service push-asennuksen haluamasi versio ei ilmetä.
- Asenna automaattisesti palvelun yrityksen push-menetelmällä, kuten WSUS tai System Center määritysten hallinta. Varmista, että olet määrittänyt asiakirjanhallinnan palvelimeen ennen tämän tekemistä.
- Asenna manuaalisesti jokaisen tietokoneeseen haluat suojata. ainkentästä Varmista, että olet määrittänyt asiakirjanhallinnan palvelimeen ennen tämän tekemistä.


### <a name="install-the-mobility-service-with-push-installation"></a>Push-asennuksen Mobility-palvelun asentaminen

Kun lisäät koneet suojaus-ryhmä Mobility-palvelun miten ja asennettuna tietokoneeseen kunkin prosessin-palvelimen automaattisesti.


#### <a name="prepare-for-automatic-push-on-windows-machines"></a>Windows-tietokoneissa automaattinen push valmisteleminen

Näin voit laatia Windows-tietokoneissa Mobility-palvelun voi asentaa automaattisesti prosessi-palvelin.

1.  Luo tili, jonka avulla voidaan prosessi-palvelin käyttää tietokoneessa. Tili on järjestelmänvalvojan oikeudet (paikallinen tai toimialue). Huomaa, että näitä tunnistetietoja käytetään vain push-asennuksen Mobility-palvelun.

    >[AZURE.NOTE] Jos et käytä toimialueen-tili, sinun on käyttäjän etäkäyttöpalvelimen ohjausobjektin paikallisessa tietokoneessa käytöstä. Voit tehdä tämän HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System rekisteriin Lisää DWORD-merkinnän LocalAccountTokenFilterPolicy arvo on 1-kohdassa. Voit lisätä rekisterin tapahtuma CLI Avaa-komento tai kirjoita PowerShellin avulla **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`**.

2.  Windowsin palomuurin koneen haluat suojata, valitse **Salli sovelluksen tai ominaisuuden läpäistä palomuurin** ja ota käyttöön **tiedostojen ja tulostimien jakaminen** ja **Ohjattu toiminto**. Voit määrittää palomuurin käytännön Ryhmäkäytäntöobjekti, jos tietokoneissa, joihin kuuluvat toimialueeseen.

    ![Palomuuriasetukset](./media/site-recovery-vmware-to-azure-classic/mobility1.png)

2. Lisää tili, jonka loit:

    - Avaa **cspsconfigtool**. Se on käytettävissä pikakuvakkeen työpöydälle nimellä ja sijaitsevat [asentaa sijainti] \home\svsystems\bin-kansiossa.
    - **Tilien hallinta** -välilehti Valitse **Lisää tili**.
    - Lisää tili, jonka loit. Kun olet lisännyt tilin tarvitset tunnistetietoja, kun lisäät koneen suojaus-ryhmä.


#### <a name="prepare-for-automatic-push-on-linux-servers"></a>Automaattinen push Linux palvelimissa valmisteleminen

1.  Varmista, että haluat suojata Linux koneen tuetaan kuvatulla tavalla [paikalliseen edellytykset](#on-premises-prerequisites). Varmista suojattava koneen välillä on verkkoyhteys ja asiakirjanhallinnan palvelimeen, joka suoritetaan prosessi-palvelimeen.

2.  Luo tili, jonka avulla voidaan prosessi-palvelin käyttää tietokoneessa. Tilin on oltava Linux lähdepalvelimeen pääkansion käyttäjän. Huomaa, että näitä tunnistetietoja käytetään vain push-asennuksen Mobility-palvelun.

    - Avaa **cspsconfigtool**. Se on käytettävissä pikakuvakkeen työpöydälle nimellä ja sijaitsevat [asentaa sijainti] \home\svsystems\bin-kansiossa.
    - **Tilien hallinta** -välilehti Valitse **Lisää tili**.
    - Lisää tili, jonka loit. Kun olet lisännyt tilin tarvitset tunnistetietoja, kun lisäät koneen suojaus-ryhmä.

3.  Tarkista, että Linux lähdepalvelimeen /etc/hosts-tiedosto on tapahtumat, jotka vastaavat paikallisen hostname liittyvät kaikkien verkkosovittimien IP-osoitteisiin.
4.  Asenna uusimmat openssh, openssh-palvelin, openssl pakettien suojattava tietokoneeseen.
5.  Varmista SSH on käytössä ja käytössä porttiin 22.
6.  Ottaa käyttöön SFTP Alirakenne ja salasanan todennuksen sshd_config tiedoston seuraavasti:

    - Kirjaudu sisään ylimmällä tasolla.
    - Etsi rivi, joka alkaa PasswordAuthentication /etc/ssh/sshd_config tiedosto.
    - Rivin kommentointi ja vaihda arvo **ei ole** **Kyllä**.
    - Etsi rivi, joka alkaa **Alirakenne** ja kommentointi rivi.

        ![Linux](./media/site-recovery-vmware-to-azure-classic/mobility2.png)


### <a name="install-the-mobility-service-manually"></a>Mobility-palvelun asentaminen manuaalisesti

Asennusohjelmia ovat käytettävissä C:\Program Files (x86) \Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository.

Lähde-käyttöjärjestelmä | Mobiilikäytön palvelun asennustiedosto
--- | ---
Windows Server (vain 64-bittinen) | Microsoft-ASR_UA_9. *.0.0_Windows_* release.exe
CentOS 6.4, 6.5 6.6 (vain 64-bittinen) | Microsoft-ASR_UA_9. *.0.0_RHEL6-64_*release.tar.gz
SUSE Linux Enterprise Server 11 SP3 (vain 64-bittinen) | Microsoft-ASR_UA_9. *.0.0_SLES11-SP3-64_*release.tar.gz
Oracle Enterprise Linux 6.4, 6.5 (vain 64-bittinen) | Microsoft-ASR_UA_9. *.0.0_OL6-64_*release.tar.gz


#### <a name="install-manually-on-a-windows-server"></a>Asenna Windows Server manuaalisesti


1. Lataa ja suorita asiaa asennusohjelma.
2. Valitse **ennen kuin aloitat ** **Mobility-palvelun**.

    ![Mobility-palvelu](./media/site-recovery-vmware-to-azure-classic/mobility3.png)

3. Määritä asiakirjanhallinnan palvelimeen ja salasana, joka on luotu, kun asensit management server-osat-IP-osoite **Palvelimen tiedot** . Voit noutaa salasana suorittamalla: ** <SiteRecoveryInstallationFolder>\home\sysystems\bin\genpassphrase.exe – n** hallinta palvelimessa.

    ![Mobility-palvelu](./media/site-recovery-vmware-to-azure-classic/mobility6.png)

4. **Asenna** sijainnissa jätä oletussijainti ja valitse **Seuraava** ja aloita asennus.
5. **Asennus** käynnissä valvoa asennus ja Käynnistä tietokone uudelleen, jos ohjelma pyytää sitä.

Voit myös asentaa komentoriviltä:

UnifiedAgent.exe [/ rooli < agentti/MasterTarget >] [/ InstallLocation <Installation Directory>] [/ CSIP <IP address of CS to be registered with>] [/ PassphraseFilePath <Passphrase file path>] [/ LogFilePath <Log File Path>]

Jos:

- / Rooli: pakollinen. Määrittää, onko Mobility-palvelun asennettuna.
- / InstallLocation: pakollinen. Määrittää minne palvelu.
- / PassphraseFilePath: pakollinen. Määrittää määritys palvelimen salasana.
- / LogFilePath: pakollinen. Määrittää lokin asetukset tiedostojen sijainti

#### <a name="uninstall-mobility-service-manually"></a>Mobility-palvelun asennuksen poistaminen manuaalisesti

Mobiilikäytön palvelun voidaan poistaa käyttämällä Lisää Poista ohjelma Ohjauspaneelin kautta tai komentorivin avulla.

Komennon komentoriviltä Mobility-palvelun asennuksen poistaminen

    MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1}

#### <a name="modify-the-ip-address-of-the-management-server"></a>Muokkaa hallinnan palvelimen IP-osoite

Ohjatun luomisen jälkeen voit muokata hallinnan palvelimen IP-osoitteen seuraavasti:

1. Avaa tiedosto-hostconfig.exe (työpöydällä).
2. Valitse **Yleiset** -välilehdessä voit muuttaa hallinnan palvelimen IP-osoite.

    >[AZURE.NOTE] Muuttaa vain hallinnan palvelimen IP-osoite. Porttinumero management server viestinnälle on oltava 443 ja käytä HTTPS tulisi olla käytössä vasemmalle. Salasana ei voi muokata.

    ![Hallinnan palvelimen IP-osoite](./media/site-recovery-vmware-to-azure-classic/host-config.png)

#### <a name="install-manually-on-a-linux-server"></a>Asenna manuaalisesti Linux-palvelimeen:

1. Kopioi edellä olevan taulukon suojattava Linux koneen perusteella tarvittavat tar arkisto.
2. Avaa shell-ohjelmasta ja purkaa zip tar arkisto paikallisen polun suorittamalla:`tar -xvzf Microsoft-ASR_UA_8.5.0.0*`
3. Luo passphrase.txt tiedoston paikalliseen kansioon, johon purit tar arkiston sisällön. Voit tehdä tämän kopioi salasana C:\ProgramData\Microsoft Azure sivuston Recovery\private\connection.passphrase hallinta palvelimessa ja tallenna se passphrase.txt suorittamalla *`echo <passphrase> >passphrase.txt`* -käyttöliittymän.
4. Mobility-palvelun asentaminen kirjoittamalla *`sudo ./install -t both -a host -R Agent -d /usr/local/ASR -i <IP address> -p <port> -s y -c https -P passphrase.txt`*.
5. Määritä sisäisen hallinnan palvelimen IP-osoite ja varmista, että porttiin 443 on valittuna.

**Voit myös asentaa komentoriviltä**:

1. Kopioi C:\Program Files (x86) \InMage Systems\private\connection hallinta palvelimessa salasana ja tallenna se "passphrase.txt" hallinta palvelimessa. Suorittamalla nämä komennot. Tässä esimerkissä hallinnan palvelimen IP-osoite on 104.40.75.37 ja HTTPS-porttiin 443 on oltava:

Asenna tuotannon-palvelimeen:

    ./install -t both -a host -R Agent -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt

Asenna perusmuodon kohde-palvelimeen:


    ./install -t both -a host -R MasterTarget -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt


## <a name="step-10-enable-protection-for-a-machine"></a>Vaihe 10: Käytä suojausta tietokoneelle

Suojauksen käyttöön lisätään näennäiskoneiden ja fyysiset palvelimet suojaus-ryhmä. Ennen kuin aloitat, ota huomioon seuraavat, jos olet suojaaminen VMware näennäiskoneiden:

- VMware VMs löytyy 15 minuutin välein, ja se voi kestää yli 15 minuuttia, ne näkyvät sivuston palautus-portaalissa etsiminen jälkeen.
- Ympäristön muutokset virtuaalikoneen (kuten VMware Työkalut asennus) myös voi kestää yli 15 minuuttia palauttaminen päivittäminen kestää.
- Voit tarkistaa havaitun viimeksi VMware VMs **Viimeisen yhteyshenkilön osoitteessa** -kenttään vCenter palvelin/ESXi isännän **Kokoonpano-palvelimet** -välilehden.
- Jos olet jo luonut suojaus-ryhmä ja lisää sen jälkeen vCenter Server tai ESXi host, voi kestää yli 15 minuuttia päivittämiseen Azure sivuston palautus-portaalin ja näennäiskoneiden **Lisää koneet suojaus-ryhmä** -valintaikkunassa näkyvän.
- Jos haluat jatkaa välittömästi koneet lisääminen suojaus-ryhmä ajoitetun etsiminen odottamatta, korosta määrityspalvelimen (Älä valitse) ja valitse **Päivitä** -painiketta.

Huomaa, että lisäksi:

- On suositeltavaa arkkitehti suojaus ryhmien niin, että ne vastaavat oman toiminnoista. Lisää, joissa käytetään tietyn sovelluksen samaan ryhmään.
- Kun lisäät koneet suojaus-ryhmä, prosessi-palvelin Vie automaattisesti ja asentaa Mobility-palvelu, jos se ei ole jo asennettu. Huomaa, että sinulla on oltava push-järjestelmä valmisteleminen edellisessä vaiheessa kuvatulla tavalla.


Lisää koneet suojaus-ryhmä:

1. Valitse **suojattu kohteiden** > **Suojaus-ryhmä** > **koneet** > Lisää koneet. \As parhaat käytännöt
2. **Valitse** näennäiskoneiden Jos olet suojaaminen VMware näennäiskoneiden, valitse vCenter-palvelin, joka hallitsee oman näennäiskoneiden (tai EXSi isännän, jossa ne on käynnissä) ja valitse sitten koneet.

    ![Ota suojaus käyttöön](./media/site-recovery-vmware-to-azure-classic/enable-protection2.png)

3.  **Valitse** näennäiskoneiden Jos olet suojaaminen fyysiset palvelimet, **Lisää fyysisten laitteiden** ohjatun antaa IP-osoite ja kutsumanimi. Valitse käyttöjärjestelmä perhe.

    ![Ota suojaus käyttöön](./media/site-recovery-vmware-to-azure-classic/enable-protection1.png)

4. Valitse **Määritä kohde resurssien** tallennustilan tilin käyttämäsi replikoinnin ja valita, käytetäänkö asetukset, kaikki työmäärät. Huomaa, että premium-tallennustilan tilin ei tällä hetkellä tueta.

    >[AZURE.NOTE] 1. Microsoft ei tue tallennustilan tilit luotu [uuden Azure portaalin](../storage/storage-create-storage-account.md) resurssin ryhmien Siirrä.                           2.[siirto tallennustilan tilien](../resource-group-move-resources.md) saman tilauksen piiriin kuuluvien resurssien ryhmien tai tilauksissa ei tue tallennustilan tilejä sivustojen palauttamisen käyttöönotto.

    ![Ota suojaus käyttöön](./media/site-recovery-vmware-to-azure-classic/enable-protection3.png)

5. **Määritä** tilien Valitse tili, jonka [määritetty](#install-the-mobility-service-with-push-installation) käyttämään automaattinen asennus Mobility-palvelun.

    ![Ota suojaus käyttöön](./media/site-recovery-vmware-to-azure-classic/enable-protection4.png)

6. Napsauta valintamerkkiä lisännyt koneet suojaus-ryhmä ja Käynnistä ensimmäisen replikoinnin kunkin tietokoneeseen.

    >[AZURE.NOTE] Jos push-asennus on valmisteltu Mobility-palvelu asennetaan automaattisesti tietokoneissa, joihin ei ole levyä, kun ne lisätään suojaus-ryhmä. Kun palvelu on asennuksen suojaus työn käynnistyy ja epäonnistuu. Virheen jälkeen sinun on käynnistettävä uudelleen manuaalisesti Jokaisessa koneessa, joka on ollut asennettuna Mobility-palvelun. Uudelleenkäynnistyksen jälkeen suojaus-työ alkaa uudelleen ja ensimmäinen replikointi tapahtuu.

Voit valvoa tilan **työt** -sivulta.

![Ota suojaus käyttöön](./media/site-recovery-vmware-to-azure-classic/enable-protection5.png)

Lisäksi tilaa voidaan valvoa **Suojattu**kohteiden > <protection group name> > **näennäiskoneiden**. Kun ensimmäisen replikoinnin on valmis ja tietojen synkronoinnin, tietokoneen tilaksi muuttuu** suojattu**.

![Ota suojaus käyttöön](./media/site-recovery-vmware-to-azure-classic/enable-protection6.png)


## <a name="step-11-set-protected-machine-properties"></a>Vaihe 11: Määritä suojattu tietokoneen ominaisuudet

1. Kun kone on **suojattu** tila voit määrittää automaattisesti sen ominaisuuksia. Valitse Suojaus-ryhmän tiedot koneen ja Avaa **määritys** -välilehti.
2. Sivuston palauttaminen ehdottaa ominaisuuksia Azure AM ja tunnistaa paikallista verkkoasetukset automaattisesti.

    ![Virtuaalikoneen ominaisuuksien määrittäminen](./media/site-recovery-vmware-to-azure-classic/vm-properties1.png)

3. Voit muuttaa näitä asetuksia:

    -  **Azure AM nimi**: Tämä on nimi, joka annetaan Azure koneen jälkeen automaattisesti. Nimen on oltava Azure vaatimusten mukainen.

    -  **Azure AM kokoa**: verkkosovittimien määrä määräytyy kohde virtuaalikoneen määrittäminen koon mukaan. [Lue lisää](../virtual-machines/virtual-machines-linux-sizes.md/#size-tables) kokojen ja sovittimet. Huomaa, että:

        - Kun muokkaat virtual machine koon ja Tallenna asetukset-verkkosovittimen määrä muuttuu, kun seuraavan kerran avataan **määritys** -välilehti. Verkkosovittimien kohde näennäiskoneiden on määrä on pienin verkkosovittimien lähde virtual koneessa määrän ja valinnut virtuaalikoneen koon tukemat verkkosovittimien enimmäismäärä.
            - Jos verkkosovittimien lähde-koneessa määrä on pienempi tai yhtä sovittimet sallittu kohde koneen kokoa, kohde on yhtä monta sovittimet lähde.
            - Jos lähde virtuaalikoneen sovittimet ylittää sallittu, Kohdekoko sitten kohde enimmäiskoko käytetään.
            - Jos lähdetietokoneen on kaksi verkkosovittimien ja kohde koneen koon tukee neljä, kohdetietokoneen on kahden sovittimen esimerkiksi. Jos lähde on kahden sovittimen mutta tuetut kohteen koon tukee vain yksi kohdetietokoneen on vain yksi sovittimen.
        - Jos virtuaalikoneen on useita verkkosovittimien sovittimet olisi yhteydessä saman Azure verkkoon.
    - **Azure verkon**: sinun on määritettävä Azure verkossa, jossa Azure VMs yhdistetään automaattisesti jälkeen. Jos vuotta ei määritetä Azure VMs-arvoksi ei ole yhdistetty verkon. Sinun on lisäksi voit määrittää Azure verkon, jos haluat tuntisesta azuren paikallisen-sivustoon. Tuntisesta edellyttää VPN-yhteyden Azure verkko- ja paikallisen verkon välillä.
    - **Azure-IP-osoite ja aliverkon**: kunkin verkkosovittimen valitset aliverkon, johon Azure AM pitää yhdistää. Huomaa, että:
        - Jos lähde-koneen verkkosovittimen on määritetty käyttämään staattinen IP-osoite voit määrittäminen Azure AM staattinen IP-osoite. Jos et anna staattinen IP-osoite jaetaan käytettävissä minkä tahansa IP-osoite. Jos kohde-IP-osoite on määritetty, mutta se on jo käytössä toisessa AM Azure-tietokannassa automaattisesti epäonnistuu. Jos lähde-koneen verkkosovittimen on määritetty käyttämään DHCP sitten on DHCP Azure-asetukseksi.

## <a name="step-12-create-a-recovery-plan-and-run-a-failover"></a>Vaihe 12: Palautus suunnitelman luominen ja suorittaminen automaattisesti

> [AZURE.VIDEO enhanced-vmware-to-azure-failover]

Voit suorittaa automaattisesti yhdelle tietokoneelle tai epäonnistua useita näennäiskoneiden, joka suorittaa saman tehtävän tai suorittaa saman työmäärää päälle. Epäonnistuu useiden laitteiden päälle niiden lisääminen palautus suunnitelma samanaikaisesti.

### <a name="create-a-recovery-plan"></a>Suunnittele palauttaminen

1. Valitsemalla **Lisää palautus suunnitelman** **Palautus suunnitelmien** -sivulla ja lisää palautus-suunnitelma. Määritä suunnitelman tiedot ja valitse **Azure** kohteena.

    ![Määritä palautus suunnitelma](./media/site-recovery-vmware-to-azure-classic/recovery-plan1.png)

2. **Valitse virtuaalikoneen** : Valitse Suojaus-ryhmä ja valitse sitten koneet ryhmä, johon haluat lisätä palautus-suunnitelma.

    ![Lisää näennäiskoneiden](./media/site-recovery-vmware-to-azure-classic/recovery-plan2.png)

Voit mukauttaa aiot luoda ryhmiä ja järjestyksen siinä järjestyksessä, jossa koneet palautus-suunnitelmassa epäonnistui päälle. Voit myös lisätä komentosarjojen ja pyytää manuaalinen toiminnot. Komentosarjojen luomista manuaalisesti tai ohjatulla [Azure automaatio Runbooks](site-recovery-runbook-automation.md)mukaan. [Lisätietoja](site-recovery-create-recovery-plans.md) mukauttamisesta palautus-palvelupakettia.

## <a name="run-a-failover"></a>Suorita automaattisesti

Ennen kuin suoritat automaattisesti huomautuksen:


- Varmista, että hallinta palvelimessa on saatavilla - muuten automaattisesti epäonnistuu.
- Jos suoritat suunnittelematon automaattisesti Huomautus, joka:

    - Olisi mahdollisuuksien Sammuta ensisijainen koneet, ennen kuin suoritat suunnittelematon automaattisesti. Näin varmistat, että sinulla ei ole lähde- ja replikan, joissa käytetään yhtä aikaa. Jos olet replikoiminen VMware VMs sitten kun suoritat suunnittelematon automaattisesti voit määrittää, että sivuston palauttaminen Varmista paras työmäärään sulkeutumaan lähde-tietokoneissa. Ensisijainen sivuston tilan mukaan tämä voi tai ei ehkä toimi. Jos olet replikoiminen fyysiset palvelimet palauttaminen ei voi käyttää asetus.
    - Kun suoritat suunnittelematon automaattisesti lopettaa tietojen replikoinnin suorittamisen ensisijainen koneet niin, että kaikki tiedot delta ei siirretä, kun suunnittelematon automaattisesti alkaa.

- Jos haluat muodostaa yhteyden replikan virtuaalikoneen Azure-tietokannassa jälkeen automaattisesti, käyttöön Etätyöpöytäyhteys lähde-laitteeseen, ennen kuin suoritat vikasietotilaa ja Salli RDP yhteyden palomuurin läpi. Sinun on myös Salli RDP Azure virtuaalikoneen julkisen päätepisteen automaattisesti jälkeen. Toimi näiden [parhaiden käytäntöjen](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx) voit varmistaa, että RDP toimii jälkeen automaattisesti.

>[AZURE.NOTE] Saat parhaan suorituskyvyn, kun teet automaattisesti Azure, varmista, että olet asentanut Azure-agentti suojattujen kone. Tämä auttaa-käynnistäminen nopeammin ja myös auttaa Jos ongelmien vianmäärityksen. Linux edustaja voi olla löytyneen [tähän](https://github.com/Azure/WALinuxAgent) - ja Windows agentti löytyy [tähän](http://go.microsoft.com/fwlink/?LinkID=394789)

### <a name="run-a-test-failover"></a>Suorita testi automaattisesti

Suorita testi automaattisesti, jotta saadaan aikaan automaattisesti ja palautus-prosesseja, joka ei vaikuta oman tuotantoympäristössä eristetyssä verkossa ja säännöllisesti Replikointi jatkuu normaalisti. Testaa automaattisesti aloittaa lähteen ja voit suorittaa se usealla tavalla:

- **Et määritä Azure verkon**: Jos suoritat testi-automaattisesti ilman verkkoa testi yksinkertaisesti tarkistaa näennäiskoneiden Käynnistä ja Azure näkyvät oikein. Ei ole yhdistetty Azure verkkoon näennäiskoneiden jälkeen automaattisesti.
- **Määritä Azure verkon**: tällaista automaattisesti tarkistaa koko replikoinnin-ympäristö on enintään oikein ja että Azuren näennäiskoneiden yhteydessä määritettyyn verkkoon.


1. **Palautus suunnitelmien** -sivulla palvelupaketti, ja valitse sitten **Testi automaattisesti**.

    ![Lisää näennäiskoneiden](./media/site-recovery-vmware-to-azure-classic/test-failover1.png)

2. Valitse **Vahvista testata automaattisesti** **mitään** ilmaisemaan, joita et halua käyttää Azure verkon testi vikasietotilaa tai valitse verkko, johon liitetty testi VMs jälkeen automaattisesti. Valitse Aloita vikasietotilaa valintamerkkiä.

    ![Lisää näennäiskoneiden](./media/site-recovery-vmware-to-azure-classic/test-failover2.png)

3. **Työt** -välilehden automaattisesti seurata.

    ![Lisää näennäiskoneiden](./media/site-recovery-vmware-to-azure-classic/test-failover3.png)

4. Vikasietotilaa päätyttyä myös pitäisi näe se Azure koneen näkyvät Azure portal > **näennäiskoneiden**. Jos haluat aloittaa Azure AM, sinun on Avaa portti 3389 AM päätepisteen RDP-yhteyttä.

5. Kun olet valmis, kun automaattisesti saavuttaa Viimeistele testaaminen vaiheen valitsemalla Valmis Testaa loppuun. Muistiinpanojen tallentaminen ja Tallenna testi vikasietotilaa liittyvät huomautukset.

6. Valitse automaattisesti ajoitettuina testiympäristössä **testi vikasietotilaa on valmis** . Tämän jälkeen testi vikasietotilaa näkyy **valmiina** tila. Kaikki osien ja luo automaattisesti aikana testi vikasietotilaa VMs poistetaan. Huomaa, että jos testi automaattisesti edelleen kestää yli kaksi viikkoa se on valmis voimassa mukaan.



### <a name="run-an-unplanned-failover"></a>Suorita suunnittelematon automaattisesti

Suunnittelematon automaattisesti on käynnistetty Azure ja voi suorittaa, vaikka ensisijainen kirjasto ei ole käytettävissä.


1. **Palautus suunnitelmien** -sivulla palvelupaketti, ja valitse sitten **automaattisesti** > **Suunnittelematon automaattisesti**.

    ![Lisää näennäiskoneiden](./media/site-recovery-vmware-to-azure-classic/unplanned-failover1.png)

2. Jos olet replikoiminen VMware näennäiskoneiden voit valita ja yritä paikallisen VMs Sammuta. Tämä on paras työmäärään ja automaattisesti säilyvät työmäärään, joka onnistuu vai ei. Jos se ei onnistunut virheen yksityiskohtaiset tiedot näkyvät **työt **-välilehden > **Suunnittelematon automaattisesti työt**.

    ![Lisää näennäiskoneiden](./media/site-recovery-vmware-to-azure-classic/unplanned-failover2.png)

    >[AZURE.NOTE] Tämä vaihtoehto ei ole käytettävissä, jos olet replikoiminen fyysinen palvelimiin. Sinun on kokeilla ja Sammuta ne manuaalisesti jos mahdollista.

3. Tarkista **Vahvista automaattisesti** automaattisesti suunnan (Azure) ja valitse palautus-kohtaan haluat käyttää vikasietotilaa. Jos usean AM otettu käyttöön, kun olet määrittänyt replikoinnin ominaisuudet voit palauttaa uusimman sovelluksen tai kaatumisen yhdenmukaisia palautus-kohtaan. Voit myös valita **Mukautettu palautus valitsemalla** Palauta aikaisempaan samanaikaisesti. Valitse Aloita vikasietotilaa valintamerkkiä.

    ![Lisää näennäiskoneiden](./media/site-recovery-vmware-to-azure-classic/unplanned-failover3.png)

3. Odota suunnittelematon automaattisesti työn suorittamiseen. Voit valvoa automaattisesti edistymisen **töihin** -välilehdessä. Huomaa, että vaikka suunnittelematon automaattisesti aikana ilmenee virheitä palautus-suunnitelma suoritetaan ennen kuin se on valmis. Katso Azure se pitäisi myös tietokoneen näkyvät näennäiskoneiden Azure-portaalissa.

### <a name="connect-to-replicated-azure-virtual-machines-after-failover"></a>Yhteyden muodostaminen replikoida Azuren näennäiskoneiden automaattisesti jälkeen

Jotta voit muodostaa yhteyden replikoida näennäiskoneiden Azure-tietokannassa automaattisesti tähän jälkeen käytön edellytykset:

1. Etätyöpöytäyhteys tulisi olla käytössä ensisijainen tietokoneeseen.
2. Windowsin palomuuri ensisijainen laitteeseen, jotta RDP.
3. Tarvitset jälkeen automaattisesti RDP lisääminen Azure virtuaalikoneen julkisen päätepisteen.

[Lue lisää](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx) määrityksestä.


## <a name="deploy-additional-process-servers"></a>Prosessit-palvelimien käyttöönottaminen

Jos sinulla on yli 200 lähde-konetta tai yhteensä päivittäinen churn-kurssi käyttöönoton ylittää ulos skaalata 2 Teratavua, sinun on käsitellään liikenteen äänenvoimakkuuden prosessit-palvelimiin. Voit määrittää prosessit-palvelimen [prosessit](#additional-process-servers) palvelimissa tarkistaminen ja noudata sitten ohjeita tähän prosessi-palvelimen määrittäminen. Kun olet määrittänyt palvelimen voit määrittää tietolähteen koneet käyttämään sitä.

### <a name="set-up-an-additional-process-server"></a>Prosessit-palvelimen määrittäminen

Voit määrittää prosessit-palvelimeen seuraavasti:

- Yhdistetty ohjatun hallinta-palvelimen määrittäminen vain prosessin palvelimeksi.
- Jos haluat hallita vain uusi prosessi-palvelimen käyttäminen tietojen replikoinnin tarvitset siirtää oman suojatun koneet toiminto.

### <a name="install-the-process-server"></a>Asenna prosessi-palvelin

1. Pika-aloitus-sivulla Lataa palauttaminen osien asennusta yhdistetty asennustiedostoa. Suorittamalla asennusohjelma.
2. Valitse **Lisää prosessit-palvelinten skaalata käyttöönoton,** **ennen kuin aloitat** .

    ![Lisää prosessi-palvelin](./media/site-recovery-vmware-to-azure-classic/add-ps1.png)

3. Suorita ohjattu teit milloin samalla tavalla voit [määrittää](#step-5-install-the-management-server) ensimmäisen asiakirjanhallinnan palvelimeen.

4. Määritä alkuperäinen asiakirjanhallinnan palvelimeen, johon on asennettu configuration-palvelin ja salasana IP-osoite **Palvelimen tiedot** . Alkuperäinen hallinta-palvelimeen, suorita ** <SiteRecoveryInstallationFolder>\home\sysystems\bin\genpassphrase.exe – n** voit hankkia salasana.

    ![Lisää prosessi-palvelin](./media/site-recovery-vmware-to-azure-classic/add-ps2.png)

### <a name="migrate-machines-to-use-the-new-process-server"></a>Siirtää koneet uuden prosessi-palvelimen käyttäminen

1. Avaa **Configuration Servers** > **Server** > alkuperäisen hallinta-palvelimen nimi > **Palvelimen tiedot**.

    ![Päivitetäänkö palvelin prosessi](./media/site-recovery-vmware-to-azure-classic/update-process-server1.png)

2. Valitse **Prosessi-palvelimet** -luettelosta **Muuta prosessi-palvelinta** haluat muokata palvelimen nimen vieressä.

    ![Päivitetäänkö palvelin prosessi](./media/site-recovery-vmware-to-azure-classic/update-process-server2.png)

3. **Muuta prosessin**Serverissä > **Kohde prosessin palvelimen** Valitse uusi asiakirjanhallinnan palvelimeen ja valitse sitten näennäiskoneiden, joka käsittelee uuteen prosessi-palvelimeen. Tietoja palvelimen tiedot-kuvaketta. Keskimääräinen tila on tarvittavat kunkin valitun virtuaalikoneen replikoida palvelimeen prosessin näkyy tehdä ladata päätöksiä. Valitse Aloita uusi prosessin palvelimeen replikoiminen valintamerkkiä.

    ![Päivitetäänkö palvelin prosessi](./media/site-recovery-vmware-to-azure-classic/update-process-server3.png)




## <a name="vmware-permissions-for-vcenter-access"></a>VMware käyttöoikeudet vCenter käytön

Prosessi-palvelimen automaattisesti löytävät VMs vCenter palvelimessa. Automaattinen etsiminen suorittamiseen tarvitset Määritä rooli (Azure_Site_Recovery), jotta sivuston palauttaminen vCenter palvelinta vCenter tasolla. Jos tarvitset vain VMware koneet siirtäminen Azure ja ei tarvitse tuntisesta azuren, voit määrittää vain luku-roolin, joka on riittävä. Voit määrittää käyttöoikeudet kuvatulla tavalla [Vaihe 6: Määritä tunnistetiedot vCenter palvelimen](#step-6-set-up-credentials-for-the-vcenter-server) seuraavaan taulukkoon on koottu roolin käyttöoikeuksia.

**Rooli** | **Tiedot** | **Käyttöoikeudet**
--- | --- | ---
Azure_Site_Recovery rooli | VMware AM etsiminen |Määritä nämä oikeudet v keskelle palvelimen:<br/><br/>Datastore -> Kohdista tilaa, Selaa datastore, pienen tason tiedoston toiminnot, poista tiedoston, Päivitä virtuaalikoneen tiedostot<br/><br/>Verkko -> verkon määrittäminen<br/><br/>Resurssi -> Määritä virtual machine resurssivarantoon artikkelista virtuaalikoneen virta, virtuaalikoneen virta siirtäminen<br/><br/>Tehtävät -> Luo tehtävän tehtävän päivittäminen<br/><br/>Virtuaalikoneen -> määritys<br/><br/>Virtuaalikoneen -> kielikoodeista Answer kysymys, laitteen yhteyden, määrittäminen CD-levylle media, Määritä levyketietovälineitä, Power käytöstä Power -> Valitse, VMware työkalut asennetaan<br/><br/>Virtuaalikoneen -> varaston luominen, rekisteriä, poista -><br/><br/>Virtuaalikoneen -> Provisioning -> Salli virtuaalikoneen Lataa, virtuaalikoneen tiedostojen lataaminen<br/><br/>Virtuaalikoneen -> tilannevedoksia -> Poista tilannevedokset
vCenter käyttäjärooli | VMware AM etsiminen/automaattisesti ilman lähteen AM sulkeminen | Määritä nämä oikeudet v keskelle palvelimen:<br/><br/>Tietokeskuksen objektin –> Propagate ali-objekti, roolin = vain luku-tilassa <br/><br/>Käyttäjä on määritetty palvelinkeskuksen tasolla ja siten käyttää kaikki objektit joten.  Jos haluat rajoittaa käyttöä, määrittää **Propagate lapselle** objektia **ei voi käyttää** -roolin aliobjektit (ESX isännät, datastores, VMs ja verkkoja).
vCenter käyttäjärooli | Automaattisesti ja tuntisesta | Määritä nämä oikeudet v keskelle palvelimen:<br/><br/>Palvelinkeskuksen objektin – Propagate ali-objekti, roolin = Azure_Site_Recovery<br/><br/>Käyttäjä on varattu palvelinkeskuksen tasolla ja siten käyttää kaikki objektit joten.  Jos haluat rajoittaa käyttöä, määrittää **Propagate lapsen objektia** **ei voi käyttää **rooleihin Aliobjektin (ESX isännät, datastores, VMs ja verkkoja).  



## <a name="third-party-software-notices-and-information"></a>Kolmannen osapuolen ilmoituksia ja tietoja

Älä kääntäminen tai lokalisoidaan

Ohjelmiston ja laiteohjelmiston käynnissä Microsoftin tuote tai palvelu perustuu tai kattaa materiaali alla projekteista (yhdessä "kolmannen osapuolen Code").  Microsoft ei ole häneltä kolmannen osapuolen-koodin.  Alkuperäinen tekijänoikeusmerkintä sekä käyttöoikeus-kohdassa Microsoft saanut kolmannen osapuolen esimerkiksi koodi on määritetty alla.

Kohdassa A liittyy kolmannen osapuolen koodin osat alla luetellut projektit. Nämä käyttöoikeudet ja tiedot on tarkoitettu vain tiedoksi.  Kolmannen osapuolen koodi on parhaillaan relicensed sinulle Microsoft Microsoftin ohjelmistojen käyttöoikeusehdot, Microsoftin tuote tai palvelu-kohdassa.  

Tietoja b osassa liittyy kolmannen osapuolen koodin osat, jotka tehdään käytettävissä sinulle Microsoft alkuperäisen Käyttöoikeussopimuksen ehtojen mukaisesti.

Koko tiedoston löytyy [Microsoft Download Centeristä](http://go.microsoft.com/fwlink/?LinkId=529428). Microsoft pidättää kaikki oikeudet ole nimenomaisesti myönnetty, onko toteutukseen, vastaväitteitä tai muulla tavoin.

## <a name="next-steps"></a>Seuraavat vaiheet

[Lisätietoja tuntisesta](site-recovery-failback-azure-to-vmware-classic.md) tuomaan päällä, joissa käytetään Azure oman epäonnistui takaisin paikalliseen ympäristöön.

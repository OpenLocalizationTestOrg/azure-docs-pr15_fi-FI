<properties
    pageTitle="Toistaa VMware näennäiskoneiden ja fyysiset palvelimet Azure kanssa Azure palauttaminen Azure-portaalissa | Microsoft Azure"
    description="Kerrotaan, miten voit ottaa Azure palauttaminen, orchestrate replikoinnin, automaattisesti ja paikallisen VMware näennäiskoneiden ja Windows-tai Linux fyysinen palvelinten Azure Azure-portaalissa"
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
    ms.date="08/12/2016"
    ms.author="raynew"/>

# <a name="replicate-vmware-virtual-machines-and-physical-machines-to-azure-with-azure-site-recovery-using-the-azure-portal"></a>Toistaa VMware näennäiskoneiden ja fyysisten laitteiden Azure kanssa Azure palauttaminen Azure-portaalissa

> [AZURE.SELECTOR]
- [Azure Portal](site-recovery-vmware-to-azure.md)
- [Azure perinteinen](site-recovery-vmware-to-azure-classic.md)
- [Azure perinteinen (vanha)](site-recovery-vmware-to-azure-classic-legacy.md)

Tervetuloa käyttämään Azure palauttaminen! Käytä tämän artikkelin, jos haluat toistaa paikallisen VMware näennäiskoneiden tai Windows-tai Linux fyysinen palvelimia Azure käyttämällä Azure sivuston Azure-portaalissa.

> [AZURE.NOTE] Azure on kaksi eri [käyttöönoton mallien](../resource-manager-deployment-model.md) luominen ja käyttäminen resurssit: Azure resurssien hallinta (ARM) ja perinteinen. Azure on myös kaksi portaaleihin – Azure perinteinen portaalin, joka tukee perinteinen käyttöönottomalli ja Azure-portaalin tukeen käyttöönoton sekä malleille.

Sivuston palauttaminen Azure-portaalissa on useita uusia ominaisuuksia:

- Azure varmuuskopiointi ja palauttaminen Azure-palveluiden yhdistetään yhteen palautus palvelut-säilö, niin, että voit määrittää ja hallita liiketoiminnan jatkuvuus ja palauttaminen (BCDR) yhdestä paikasta. Yhdistetty raporttinäkymät-ikkunassa voit valvoa ja toimintojen hallitsemaan paikallisen-sivustojen ja Azure julkisen pilveen.
- Käyttäjät, joilla on valmisteltu Cloud ratkaisu Provider (CSP)-ohjelmalla Azure tilaukset voivat nyt hallita sivuston palauttaminen toimintojen Azure-portaalissa.
- Sivuston palauttaminen Azure-portaalissa voi replikoida koneet KÄDESSÄ tallennustilan tilit. Sivuston palauttaminen luo automaattisesti, milloin ARM-pohjainen VMs Azure.
- Sivuston palauttaminen säilyy tue replikointia perinteinen tallennustilan tileihin. Sivuston palauttaminen luo automaattisesti, milloin VMs perinteinen mallin.

Luettuasi tämän artikkelin viestiin mahdolliset kommentit Disqus kommenttien alaosassa. Esitä kysymyksiä tekniset [Azure palautus Services keskustelupalstalle](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="overview"></a>Yleiskatsaus

Organisaatioiden on BCDR strategia, joka määrittää, miten sovellukset ja työmääriä pysy käynnissä ja käytettävissä aikana suunnitellut ja suunnittelemattomat käyttökatkot ja palauttaa normaalijakauman käytön edellytykset mahdollisimman pian. BCDR strategia kannattaa säilyttäminen yritystietojen turvassa ja palautettavissa ja varmistetaan, että työmääriä pysyvät jatkuvasti käytettävissä, kun tietojen tapahtuu.

Sivuston palautus on Azure-palvelu prosentuaalista osuutta BCDR strategia mukaan orchestrating replikoinnin paikallisen fyysiset palvelimet ja näennäiskoneiden pilveen (Azure) tai toissijainen palvelinkeskukseen. Kun katkokset ensisijainen sijainti-asiakas ei päälle toissijainen sijainnin vastaamaan itsellesi sovellukset ja toiminnoista, jotka ovat käytettävissä. Asiakas ei takaisin ensisijainen sijainti, kun se palauttaa normaalijakauman toimintoihin. Lisätietoja [Azure palauttaminen ominaisuudet?](site-recovery-overview.md)

Tässä artikkelissa on kaikki tiedot, jotka haluat kopioida paikallisen VMware VMs ja Windows-tai Linux fyysinen palvelinten Azure. Se sisältää arkkitehtuuri yleiskatsaus, tiedot ja käyttöönoton vaiheet määrittämistä Azure, paikalliset palvelimet, Replikointiasetukset ja kapasiteetin suunnittelua varten. Kun olet määrittänyt infrastruktuuri voit ottaa replikoinnin tietokoneissa, jotka haluat suojata ja tarkista, että automaattisesti toimii.

## <a name="business-advantages"></a>Liiketoiminnan edut

- Sivuston palauttaminen suojaa sähköntuotannosta business toiminnoista ja VMware VMs ja fyysinen palvelimissa sovellukset.
- Palautus palvelut-portaalissa on yhden paikan määrittäminen, hallita ja seurata replikoinnin automaattisesti ja palauttaminen.
- Sivuston palauttaminen löytävät VMware VMs vSphere isännät lisätään automaattisesti.
- Voit helposti suorittaa paikallisen infrastruktuurin failovers Azure ja VMware AM palvelinten azuren (Palauta) tuntisesta paikallisen sivuston.
- Voit ottaa usean AM ja ryhmitellä, niin, että sovelluksen työmääriä tasoisen yli useita keskusteluja koneet toisinto replikoinnin. Replikoinnin ryhmän kaikissa tietokoneissa on kaatumisen yhdenmukaisia ja sovelluksen yhdenmukaisia palautus pisteitä, kun ne epäonnistuvat päälle. Saat automaattisesti voit kerätä useiden laitteiden palautus-palvelupakettien niin, että Porrastettu sovelluksen työmääriä epäonnistua yhdessä päälle.

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

Skenaario osat ovat seuraavat:

- **Määrityspalvelimen**: paikallisen-koneessa, jossa koordinoi viestintä ja hallitsee tietojen replikoinnin ja hyödyntämistä prosessit. Tässä tietokoneessa suoritat asennustiedostoa configuration-palvelin ja näiden osien asentaminen:
    - **Prosessin palvelimen**: replikoinnin yhdyskäytävän toimii. Se vastaanottaa replikoinnin tietoja suojatun lähde koneet, optimoi tallentamisesta välimuistiin, pakkaamisen ja salaus ja lähettää sen Azure-tallennustilan. Se myös käsittelee suojatun koneet Mobility palvelun push-asennus ja suorittaa VMware VMs Automaattinen etsiminen. Prosessin oletuspalvelimeen on asennettu configuration-palvelin. Voit ottaa käyttöön Lisää erillinen prosessi palvelinten skaalata käyttöönoton.
    - **Perustyylisivujen kohdepalvelin**: käsittelee replikoinnin tietojen tuntisesta azuren aikana.

- **Mobility-palvelu**: Tämä osa on otettu käyttöön jokaiseen tietokoneeseen (VMware AM tai fyysinen palvelin), jonka haluat toistaa Azure. Se sieppaa tietojen kirjoituksia tietokoneeseen ja välittää niitä prosessi-palvelimeen.
- **Azure**: sinun ei tarvitse luoda minkä tahansa Azure VMs käsittelemään replikointi ja Azure automaattisesti.  Tarvitset Azure tilauksen Azure-tallennustilan tilin tallentamiseen replikoitua tiedot ja Azure virtual verkko, niin, että Azure VMs ovat yhteydessä verkkoon jälkeen automaattisesti. Tallennustilan tilin ja verkon on oltava sama alue kuin palautus Services säilö.
- **Tuntisesta**: Kun olet valmis epäonnistua takaisin Azure-paikallisen sivustoon jälkeen automaattisesti, sinun on luominen Azure-AM tilapäinen prosessin palvelimeksi. Voit poistaa sen tuntisesta päätyttyä. Tuntisesta, saat myös tarvitset VPN (tai Azure ExpressRoute) yhteyden paikallisen sivuston ja Azure verkossa, jossa Azure VMs sijaitsevat. Jos tuntisesta tietoliikenne on suurta haluat ehkä määrittää erillinen perustyyli palvelimesta tietokoneen paikalliseen. Voit vaalentaa tietoliikenteen voidaan määritys-palvelimella perusmuodon kohde oletuspalvelimeen.


Kuva esittää, kuinka nämä osat vuorovaikutuksessa.

![arkkitehtuuri](./media/site-recovery-vmware-to-azure/v2a-architecture-henry.png)

**Kuva 1: VMware/fyysisiä Azure avulla**

## <a name="azure-prerequisites"></a>Azure edellytykset

Näin käytön edellytykset Azure-tietokannassa ottamaan Tämä skenaario.

**Edellytyksenä** | **Tiedot**
--- | ---
**Azure-tili**| Tarvitset [Microsoft Azure](http://azure.microsoft.com/) -tiliin. Voit aloittaa [maksuttoman kokeiluversion käyttäjäksi](https://azure.microsoft.com/pricing/free-trial/). [Lue lisää](https://azure.microsoft.com/pricing/details/site-recovery/) siitä, että sivuston palauttaminen hinnat.
**Azure-tallennustilan** | Replikoitua tiedot tallennetaan Azure tallennustilan ja Azure VMs luodaan, kun vikasietotila ilmenee. <br/><br/>Voit tallentaa tietoja sinun on vakio- tai maksullisten tallennustilan-tilin palautus-palveluiden säilö kanssa samalla alueella.<br/><br/>Voit käyttää tallennustilan LRS tai GRS-tilin. Suosittelemme GRS niin, että tiedot ovat joustavat alueellisen käyttökatkosta sattuessa tai jos ensisijainen alue ei voi palauttaa. [Lue lisää](../storage/storage-redundancy.md).<br/><br/> [Premium tallennustilan](../storage/storage-premium-storage.md) käytetään yleensä näennäiskoneiden, jotka tarvitsevat johdonmukaisesti tehokas IO ja host IO tehostettu työmääriä, pieni viive.<br/><br/> Jos haluat tallentaa replikoitua premium-tilin avulla, sinun on myös vakio tallennustilan-tilin tallentamiseen replikoinnin lokit, jotka siepata jatkuvaa muutokset paikalliset tiedot.<br/><br/> Huomautus tallennustilan tilit luotu Azure-portaalissa ei voi siirtää resurssiryhmien välillä. Myös Intia Keski ja Etelä Intia premium-tallennustilan tilin suojausta ei tällä hetkellä tueta.<br/><br/> [Lue lisää](../storage/storage-introduction.md) Azure-tallennustilan.
**Azure verkossa** | Tarvitset Azure virtual verkko, jossa Azure VMs muodostaa yhteyden, kun vikasietotila ilmenee. Azure virtual verkko on oltava sama alue kuin palautus Services säilö.
**Azuren tuntisesta** | Sinun on väliaikainen prosessin server Azure-AM määritetty. Voit luoda tämän, kun olet valmis epäonnistua takaisin ja poista se sen jälkeen, kun virheiden takaisin on valmis.<br/><br/> Epäonnistuu takaisin tarvitset VPN-yhteyttä (tai Azure ExpressRoute) Azure verkosta paikallisen-sivustoon.

## <a name="configuration-server--scale-out-process-prerequisites"></a>Määrityspalvelimen / skaalaaminen Käsittele edellytykset ulos

Sinun määrittää paikallisen-tietokoneen määritykset palvelimeksi / asteikko-kohtaa prosessin palvelimeen.

**Edellytyksenä** | **Tiedot**
--- | ---
**Määrityspalvelimen**| Tarvitset paikallisen fyysisiä tai virtual tietokoneessa, jossa Windows Server 2012 R2. Tietokoneeseen asennettu kaikki paikallisen sivuston palauttaminen osat.<br/><br/>Replikoinnin VMware AM Suosittelemme käyttöönottoa palvelimeen, on erittäin käytettävissä VMware AM. Jos olet replikoiminen fyysisten laitteiden tietokoneessa voi olla fyysinen palvelimeen.<br/><br/> Paikallisen sivuston tuntisesta azuren on aina VMware VMs riippumatta siitä, onko epäonnistui VMs tai fyysinen palvelimiin. Jos otat ei määrityspalvelimen VMware AM kuin tarvitset määritetty erillisen perusmuodon kohdepalvelimen VMware AM vastaanottamaan tuntisesta tietoliikennettä.<br/><br/>Jos palvelin kuuluu VMware AM, verkkosovittimen tyyppi on oltava VMXNET3. Jos käytät verkkosovittimen erityyppisiä tarvitset Asenna [VMware Päivitä](https://kb.vmware.com/selfservice/microsites/search.do?cmd=displayKC&docType=kc&externalId=2110245&sliceId=1&docTypeID=DT_KB_1_1&dialogID=26228401&stateId=1) vSphere 5.5-palvelimeen.<br/><br/>Palvelin on staattinen IP-osoite.<br/><br/>Palvelin ei saa olla toimialueen ohjauskoneen.<br/><br/>Host (isäntä)-palvelimen nimen on oltava 15 merkkiä tai vähemmän.<br/><br/>Käyttöjärjestelmä on oltava vain englannin kielen.<br/><br/> Sinun on asennettava VMware vSphere PowerCLI 6.0. Valitse määrityspalvelimen.<br/><br/>Configuration-palvelin on internet-yhteyttä. Lähtevän tietoliikenteen käyttöoikeudet tarvitaan seuraavasti:<br/><br/>HTTP-80 tilapäinen käyttöön sivuston palauttaminen (lataamaan MySQL) osien asennuksen aikana<br/><br/>Jatkuva HTTPS 443 lähtevän käytön replikoinnin hallintaa varten<br/><br/>Jatkuva HTTPS 9443 lähtevä käytön replikoinnin tietoliikenteen (portin voi muokata)<br/><br/>Palvelin on seuraava URL-osoitteiden myös, niin, että se muodostaa yhteyden Azure: *. hypervrecoverymanager.windowsazure.com; *. accesscontrol.Windows.NET; *. backup.windowsazure.com; *. BLOB.Core.Windows.NET; *. store.core.windows.net<br/><br/>Jos sinulla on IP-osoite-pohjainen palomuurisäännöt palvelimessa, tarkista sääntöjä Salli Azure tietoliikenne. Tarvitset sallimaan [Azure-palvelinkeskuksen IP-alueita](https://www.microsoft.com/download/confirmation.aspx?id=41653) ja HTTPS (443)-protokollaa.<br/><br/>Salli IP-osoitealueet tilauksen Azure alue ja Länsi US.<br/><br/>Salli MySQL-ladata tätä URL-Osoitetta:.http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi


## <a name="vmware-vcentervsphere-host-prerequisites"></a>VMware vCenter/vSphere host edellytykset

**Edellytyksenä** | **Tiedot**
--- | ---
**vSphere**| Tarvitset vähintään yksi VMware vSphere hypervisors.<br/><br/>Hypervisors suoritetaan vSphere versio 6.0, 5.5 tai 5.1 ja viimeisimmät päivitykset.<br/><br/>On suositeltavaa, että vSphere isännät- ja vCenter palvelimet sijaitsevat samassa verkostossa prosessi-palvelimeksi (tämä on verkossa, jossa määrityspalvelimen sijaitsee paitsi, jos olet määrittänyt erillinen prosessi-palvelin).
**vCenter** | On suositeltavaa ottaa VMware vCenter palvelimen hallittavan vSphere isännät. Se on suoritettava vCenter versio 6.0 tai 5.5 ja viimeisimmät päivitykset.<br/><br/>Huomaa, että sivuston palauttaminen ei tue uuden vCenter vSphere 6.0 ominaisuuksia, kuten toimintojen vCenter vMotion virtual asemat ja tallennustilaa DRS. Sivuston palauttaminen tuki on rajoitettu ominaisuuksia, jotka olivat käytettävissä versiossa 5.5.


## <a name="protected-machine-prerequisites"></a>Suojattujen kone edellytykset

**Edellytyksenä** | **Tiedot**
--- | ---
**Paikallisen (VMware VMs)** | VMware VMs suojattava pitäisi olla VMware Työkalut asennettu ja käytössä.<br/><br/> Koneet, jonka haluat suojata olisi mukaisia [Azure edellytykset](site-recovery-best-practices.md#azure-virtual-machine-requirements) Azure VMs luomiseen.<br/><br/>Yksittäisten levyn kapasiteetti suojatun tietokoneissa ei kannata olla yli 1023 Gigatavua. AM voi olla enintään 64 levyjen (näin enintään 64 TT). <br/><br/>Vähintään 2 gt käytettävissä olevan tilan komponentin asennus asennukseen.<br/><br/>VMs suojaaminen salattuja levyjä ei tueta.<br/><br/>Jaettu levyn Vieras klustereiden ei tueta.<br/><br/>**Portti 20004** avata suojatun virtuaalikoneen paikallinen palomuuri, jos haluat ottaa käyttöön **sovelluksen yhdenmukaisuuden**.<br/><br/>Tietokoneissa, joissa on yhdistetty Extensible laitteisto Interface (UEFI) / Extensible laitteisto Interface(EFI) uudelleenkäynnistys ei tueta.<br/><br/>Nimiksi pitäisi olla 1 – 63 merkkiä (kirjaimia, numeroita ja väliviivoja). Nimi on alkaa kirjaimen tai numeron ja kirjaimen tai numeron perässä. Kun olet replikoinnin tietokoneelle lähettäjää Azure nimi.<br/><br/>Jos lähde AM on NIC teaming se muunnetaan yhden NIC jälkeen Azure automaattisesti.<br/><br/>Jos suojatun näennäiskoneiden on iSCSI-levyltä sitten palauttaminen muuntaa suojatun AM iSCSI levyn .vhd-tiedoston kun AM epäonnistuu päälle Azure. Jos iSCSI-kohteen voi siirtyä sellaisen Azure AM se muodostaa yhteyden ja katso itse asiassa kaksi levyä – Azure AM Näennäiskiintolevyn levy ja tietolähteen iSCSI-levyltä. Tässä tapauksessa sinun on Katkaise yhteys, joka näkyy Azure AM iSCSI-kohteen.
**Windows-tietokoneissa (fyysinen tai VMware)** | Tietokoneessa käynnissä on tuettu 64-bittinen käyttöjärjestelmä: Windows Server 2012 R2, Windows Server 2012: ssa tai Windows Server 2008 R2 ja milloin vähintään SP1.<br/><br/> Käyttöjärjestelmä on asennettava C:\ asemaan. OS levy on oltava Windows tavallinen DVD-levyllä ja ole dynaaminen. Tiedot-levyn voi olla dynaaminen.<br/><br/>Sivuston palauttaminen tukee VMs RDM-levy. Aikana tuntisesta sivuston palauttaminen käyttää RDM levyn Jos alkuperäinen lähde AM ja RDM levy on käytettävissä. Jos ne eivät ole käytettävissä, aikana tuntisesta palauttaminen luo uuden VMDK tiedoston kunkin levyn.
**Linux koneet** (phyical tai VMware)|  Sinun on tuettu 64-bittinen käyttöjärjestelmä: punainen on Enterprise Linux 6.7,7.1,7.2; Centos 6.5, 6.6,6.7,7.0,7.1,7.2; Oracle Enterprise Linux 6.4, punainen on yhteensopiva ydin tai Unbreakable yrityksen ydin versio 3 (UEK3) SUSE Linux Enterprise Server 11 SP3 6.5.<br/><br/>suojatun tietokoneissa /ETC/Hosts tiedostojen pitäisi näkyä tapahtumat, jotka vastaavat paikallisen isäntänimi liittyvät kaikkien verkkosovittimien IP-osoitteet.<br/><br/>Jos haluat muodostaa yhteyden Azure virtual-tietokoneessa, jossa Linux automaattisesti suojattu runko Client-asiakkaalla (ssh) jälkeen, varmista, että suojatun tietokoneen suojattu runko-palvelu on määritetty alkamaan automaattisesti järjestelmän käynnistyksen ja palomuurisäännöt salli ssh yhteyden siihen.<br/><br/>Isäntänimi, liityntäkohdat, laite nimet ja Linux järjestelmän polut ja tiedostonimet (esimerkiksi/jne /; /usr) on oltava englanniksi vain.<br/><br/>Suojaus on käytössä vain Linux koneet seuraavat tallentamiseen: tiedostojärjestelmän (EXT3, ETX4, ReiserFS, XFS); Monipolku ohjelmisto-laite Mapper (monipolku)). Levyn hallinta: (LVM2). HP CCISS ohjauskoneen tallennustilan fyysinen palvelinten ei tueta. ReiserFS tiedostojärjestelmän tuetaan vain SUSE Linux Enterprise Server 11 SP3.<br/><br/>Sivuston palauttaminen tukee VMs RDM-levy.  Sivuston palauttaminen ei aikana tuntisesta Linux varten, käyttää RDM levy. Sen sijaan se luo uuden VMDK tiedoston kunkin vastaavan RDM levylle.<br/><br/>Varmista, että määrität disk.enableUUID=true-asetus-VMware AM määritysparametrit. Luoda tapahtuman, jos sitä ei ole. Se on tarvittavat antamaan yhdenmukaisia UUID VMDK niin, että se liittää oikein. Tämän asetuksen lisääminen myös varmistaa, että vain delta muutokset siirretään takaisin paikalliseen tuntisesta ja koko replikoinnin aikana.
**Mobility-palvelu** |  **Windows**: Jos haluat siirtää Mobility-palvelun automaattisesti VMs käyttöjärjestelmässä, sinun on Lisää järjestelmänvalvojatilin (paikallisen järjestelmänvalvojan Windows-tietokoneessa), jolloin prosessin palvelimen mahdollisuuksista push-asennus on.<br/><br/>**Linux**: Jos haluat siirtää Mobility-palvelun automaattisesti virtuaalilaitteiksi Linux, sinun on Luo tili, jonka avulla voidaan prosessi-palvelin tekemään push-asennuksen.<br/><br/> Oletusarvon mukaan kaikki tietokoneeseen levyjä, replikoida. [Pois replikoinnin levyltä](#exclude-disks-from-replication)Mobility-palvelu on oltava asennettuna manuaalisesti tietokoneeseen ennen kuin otat replikoinnin.<br/>

## <a name="prepare-for-deployment"></a>Valmistele käyttöönottoa varten

Valmistele käyttöönoton, sinun on:

1. [Azure verkon määrittäminen](#set-up-an-azure-network) Azure VMs olla missä sijainti, kun ne on kehrätty jälkeen automaattisesti. Lisäksi tuntisesta varten tarvitset VPN-yhteyden (tai Azure ExpressRoute) Azure verkosta paikallisen sivustoon.
2. [Azure-tallennustilan tilin](#set-up-an-azure-storage-account) replikoitua tiedot.
3. [Valmistele tilin](#prepare-an-account-for-automatic-discovery) vCenter palvelimessa tai vSphere isännöi niin, että sivuston palauttaminen voi tunnistaa VMware VMs, joka lisätään automaattisesti.
4. [Valmistele määrityspalvelimen](#prepare-the-configuration-server) sen varmistamiseksi, että voit käyttää tarvittavat URL-osoitteet ja asentaa vSphere PowerCLI 6.0.


### <a name="set-up-an-azure-network"></a>Azure-verkoston määrittäminen

- Verkossa on oltava sama Azure alue, joka käyttöön palautus Services säilö.
- Haluat käyttää epäonnistui päälle Azure VMs resurssin mallista riippuen Määritä Azure verkon [ARM-tilassa](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) tai [perinteinen tila](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).
- Epäonnistua takaisin Azure-paikallisen VMware sivustoon edellyttää VPN-yhteyden (tai Azure ExpressRoute yhteys) Azure verkosta, jossa replikoitua Azure VMs sijaitsevat, paikalliseen verkkoon, jossa määrityspalvelimen sijaitsee.
- [Lue lisää](../vpn-gateway/vpn-gateway-site-to-site-create.md) tuettu käyttöönotto mallit VPN-sivusto sivusto yhteydet, ja voit [määrittää yhteyden](../vpn-gateway/vpn-gateway-site-to-site-create.md#create-your-virtual-network).
- Voit myös määrittää [Azure ExpressRoute](../expressroute/expressroute-introduction.md). [Lisätietoja](../expressroute/expressroute-howto-vnet-portal-classic.md) Azure verkko, jossa ExpressRoute määrittämisestä.

> [AZURE.NOTE] [Siirron verkostojen](../resource-group-move-resources.md) saman tilauksen piiriin kuuluvien resurssien ryhmien tai tilauksissa ei tue verkkojen käytettäviä sivustojen palauttamisen käyttöönotto.

### <a name="set-up-an-azure-storage-account"></a>Azure-tallennustilan tilin määrittäminen

- Sinun on standard- tai sisältää tietoja replikoida Azure premium tallennustilan Azure-tili. Tilin on oltava sama alue kuin palautus Services säilö. Haluat käyttää epäonnistui päälle Azure VMs resurssin mallista riippuen Määritä tili [ARM-tilassa](../storage/storage-create-storage-account.md) tai [perinteinen tila](../storage/storage-create-storage-account-classic-portal.md).
- Jos käytät premium-tilin replikoitua luomiseen tarvittavien tietojen tallentamiseen replikoinnin ylimääräisen vakio tilin kirjaa kyseisen sieppaus meneillään oleviin muutokset paikalliset tiedot.  

> [AZURE.NOTE] [Siirron tallennustilan tilien](../resource-group-move-resources.md) saman tilauksen piiriin kuuluvien resurssien ryhmien tai tilauksissa ei tue tallennustilan tilejä sivustojen palauttamisen käyttöönotto.

### <a name="prepare-an-account-for-automatic-discovery"></a>Tilin valmisteleminen Automaattinen etsiminen

Sivuston palauttaminen prosessin palvelimen löytävät automaattisesti VMware VMs vSphere isännät tai vCenter palvelimessa, joka hallitsee isännät. Suorittaa automaattisen etsinnän palauttaminen tunnistetiedot, jotka voivat käyttää VMware-palvelimen. Tämä ei ole merkitystä, jos olet replikoiminen fyysisten laitteiden.

1. Käyttämään Automaattinen etsiminen Oma tili-roolin luominen (esimerkiksi Azure_Site_Recovery) vCenter tasolla [tarvittavat käyttöoikeudet](#vmware-account-permissions).
2. Luo uusi käyttäjä vSphere isäntä- tai vCenter palvelimeen ja määrittää roolin käyttäjälle. Myöhemmin antaa sivuston palauttaminen tietää näitä tunnistetietoja niin, että se suorittaa Automaattinen etsiminen.

    >[AZURE.NOTE] Vain luku-roolissa vCenter käyttäjätilin suorittamisen automaattisesti, mutta ei sammuta suojatun lähde-tietokoneissa. Jos haluat sulkea kyseiset koneet tarvitset [Azure_Site_Recovery](#vmware-account-permissions) rooli. Jos käytössäsi on vain siirtyminen VMs VMware Azure ei tarvitse tuntisesta vain luku-roolin on riittävä.

### <a name="prepare-the-configuration-server"></a>Valmistele configuration-palvelin

1.  Varmista, että käytät määritysten palvelimen koneen noudattaa [edellytykset](#configuration-server-prerequisites). Varmista erityisesti, että tietokone on yhteydessä Internetiin nämä asetukset:

    - Käyttöoikeuden myöntäminen näistä URL-osoitteista: *. hypervrecoverymanager.windowsazure.com; *. accesscontrol.Windows.NET; *. backup.windowsazure.com; *. BLOB.Core.Windows.NET; *. store.core.windows.net
    - Salli pääsy [http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi](http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi) lataamaan MySQL.
    - Salli palomuuri tietoliikenne Azure [Azure palvelinkeskuksen IP-alueita](https://www.microsoft.com/download/confirmation.aspx?id=41653) ja HTTPS (443)-protokollaa.

2.  Lataa ja asenna [VMware vSphere PowerCLI 6.0](https://developercenter.vmware.com/tool/vsphere_powercli/6.0) määritys-palvelimeen. (Tällä hetkellä PowerCLI muut versiot eivät ole tuettuja, mukaan lukien R versioiden version 6.0.)


## <a name="create-a-recovery-services-vault"></a>Luo palautus palvelut-säilö

1. Kirjautuminen [Azure portal](https://portal.azure.com).
2. Valitse **Uusi** > **hallinnan** > **Varmuuskopiointi ja palauttaminen (OMS)**. Vaihtoehtoisesti voit valitsemalla **Selaa** > **Palautus Services säilö** > **Lisää**.

    ![Uusi säilö](./media/site-recovery-vmware-to-azure/new-vault3.png)

3. Määritä kutsumanimi tunnistavan säilö **nimi** . Jos sinulla on useita tilauksia, valitse jonkin niistä.
4. [Luo uusi resurssiryhmä](../resource-group-template-deploy-portal.md) tai valitse aiemmin luotu. Määritä Azure alue. Tällä alueella replikoida koneet. Huomaa, että Azure tallennustilan ja palauttaminen käytettäviin verkkoihin on oltava samassa alueella. Tarkista tuettujen alueiden artikkelissa [Azure sivuston palauttaminen hinnat tiedot](https://azure.microsoft.com/pricing/details/site-recovery/) maantieteelliset käytettävyys
4. Jos haluat käyttää nopeasti koontinäytöstä säilö napsauttamalla **raporttinäkymät-ikkunan kiinnittäminen** ja valitse sitten **Luo**.

    ![Uusi säilö](./media/site-recovery-vmware-to-azure/new-vault-settings.png)

Uusi säilö näkyvät **raporttinäkymät-ikkunan** > **kaikille resursseille**, ja sisällön **palauttaminen Services vaults** sivu.

## <a name="getting-started"></a>Käytön aloittaminen

Sivuston palautus on suunniteltu avulla voit helposti käytön aloittaminen-toiminto ja käynnissä mahdollisimman pian. Se tarkistaa edellytykset, sekä esitellään vaiheet, sinun on hankittava sivuston palautus on otettu käyttöön.

Voit valita haluamasi laitteiden, jonka haluat kopioida, ja mihin replikoida. Voit määrittää infrastruktuuri, mukaan lukien paikalliset palvelimet, Azure asetukset, replikoinnin käytännöt ja kapasiteetin suunnittelua. Kun infrastruktuurin on määritetty replikoinnin VMs ja fyysinen palvelinten Ota käyttöön. Voit suorittaa tiettyjä koneet failovers tai luoda palautus epäonnistuu useiden laitteiden päälle.

Aloita valitsemalla ensin, kuinka haluat ottaa käyttöön palauttaminen aloitusopas. Aloittaminen-työnkulku muuttuu hieman replikoinnin tarpeen mukaan.


## <a name="step-1-choose-your-protection-goals"></a>Vaihe 1: Valitse suojausta tavoitteita

Valitse haluamasi replikoida ja mihin replikoida.

1. Valitse **palautus-palveluiden vaults** sivu lisääminen säilöön ja valitse **asetukset**.
2. **Asetuksissa** > **Aloittaminen** Valitse **Sivuston palauttaminen** > **Vaihe 1: valmisteleminen infrastruktuurin** > **Suojaus tavoite**.

    ![Valitse tavoitteiden](./media/site-recovery-vmware-to-azure/choose-goals.png)

3. Valitse **Suojaus-tavoite** **Azure,**ja valitse **Kyllä, jossa on VMware vSphere Hypervisor**. Valitse **OK**.

    ![Valitse tavoitteiden](./media/site-recovery-vmware-to-azure/choose-goals2.png)


## <a name="step-2-set-up-the-source-environment"></a>Vaihe 2: Lähde-ympäristön määrittäminen

Määrityspalvelimen määrittäminen ja rekisteröi palautus palvelut-säilöön. Jos olet replikoiminen VMware VMs Määritä automaattinen etsiminen käytät VMware-tili.

1. Valitse **Vaihe 1: valmisteleminen infrastruktuurin** > **lähde**. **Valmistele** lähteessä Jos sinulla ei ole configuration-palvelin osoita **+ määrityspalvelimen** se lisätään.

    ![Tietolähteen määrittäminen](./media/site-recovery-vmware-to-azure/set-source1.png)

2. **Lisää palvelin** sivu-valintaruutu **Määrityspalvelimen** näkyy **Kirjoita palvelimen**.
3. Tarkista [edellytykset](#configuration-server-prerequisites), ennen kuin ryhdyt määrittämään configuration-palvelin. Valitse tietyn Tarkista, että tietokone käyttää vaadittavat URL-osoitteet.
4.  Lataa sivuston palauttaminen yhdistetyn asennus asennustiedosto.
5.  Lataa säilö rekisteröinti-näppäintä. Tarvitset tätä yhdistetyn asennusohjelman. Avain on voimassa viisi päivää loisi sen jälkeen.

    ![Tietolähteen määrittäminen](./media/site-recovery-vmware-to-azure/set-source2.png)

6.  Käytät tietokoneeseen kuin määrityspalvelimen yhdistetyn asennusohjelman configuration-palvelin, prosessi-palvelin ja perusmuodon palvelimesta asentamiseen.


### <a name="run-site-recovery-unified-setup"></a>Suorita sivuston palauttaminen yhdistetyn asetukset

1.  Yhdistetyn asennuksen asennustiedoston suorittaminen
2.  Valitse **ennen kuin aloitat** , **Asenna määrityspalvelimen ja prosessin palvelimeen**.

    ![Ennen aloittamista](./media/site-recovery-vmware-to-azure/combined-wiz1.png)

3. Valitse **hyväksyn** ladattava ja asennettava MySQL **Kolmannen osapuolen ohjelmistokäyttöoikeus** .

    ![= Kolmannen osapuolen ohjelmisto](./media/site-recovery-vmware-to-azure/combined-wiz105.PNG)

4. **Rekisteröimiseen** Selaa ja valitse latasit säilö rekisteröinti-näppäintä.

    ![Rekisteröinti](./media/site-recovery-vmware-to-azure/combined-wiz3.png)

5. Määritä **Internet-asetukset** miten määritys-palvelimella tarjoaja muodostaa Azure palauttaminen Internetin välityksellä.

    - Jos haluat muodostaa yhteyden välityspalvelin, joka on tällä hetkellä määritetty tietokoneeseen valitsemalla **Yhdistä aiemmin välityspalvelimen asetusten kanssa**.
    - Jos haluat muodostaa yhteyden suoraan palveluntarjoajan Valitse **Yhdistä suoraan ilman välityspalvelinta**.
    - Jos aiemmin välityspalvelimen edellyttää käyttöoikeuden tarkistusta tai haluat käyttää mukautettua välityspalvelimen toimittaja-yhteyden, valitse **Yhdistä mukautetun välityspalvelimen asetusten kanssa**.
        - Jos käytössäsi on mukautettu välityspalvelimen, sinun on määritettävä osoite, portti ja tunnistetiedot
        - Jos käytät välityspalvelinta olisi on jo sallittu [edellytykset](#configuration-server-prerequisites)kuvattu URL-osoitteet.

    ![Palomuurin](./media/site-recovery-vmware-to-azure/combined-wiz4.png)

6. **Edellytykset Tarkista** asennus suoritetaan tarkistusta ja varmista, että asennuksen suorittamisen. Jos näyttöön tulee varoitus, tietoja **yleisen aika Synkronoi tarkistaminen** Tarkista, aika-asetus järjestelmän kelloa (**päivämäärä ja aika** -asetukset) on sama kuin aikavyöhyke.

    ![Edellytykset](./media/site-recovery-vmware-to-azure/combined-wiz5.png)

7. Luo tunnistetiedot, jotka asennetaan MySQL server-esiintymän kirjaudutaan **MySQL-määritys** .

    ![MySQL](./media/site-recovery-vmware-to-azure/combined-wiz6.png)

8. Valitse **Ympäristön tiedot** onko aiot replikoida VMware VMs. Jos olet, asennusohjelma tarkistaa, että PowerCLI 6.0 on asennettu.

    ![MySQL](./media/site-recovery-vmware-to-azure/combined-wiz7.png)

9. **Asenna sijainti** Valitse kohtaa, johon haluat asentaa binaaritiedostoja ja tallentaa välimuistiin. Voit valita aseman, jossa on vähintään 5 gt tallennustilaa, mutta suosittelemme välimuisti-asema, jossa on vähintään 600 Gigatavua vapaata tilaa.

    ![Asennuksen sijainti](./media/site-recovery-vmware-to-azure/combined-wiz8.png)

10. Määritä **Verkonvalinta** kuuntelua (verkkosovittimen ja SSL-portti), configuration-palvelin lähettää ja vastaanottaa replikoinnin tiedot. Voit muuttaa oletusarvoista portti (9443). Lisäksi tämän portin verkkopalvelin joka orchestrates replikoinnin toimintoja käytetään porttiin 443. vastaanottamiseen replikoinnin liikenne ei kannata käyttää 443.


    ![Verkonvalinta](./media/site-recovery-vmware-to-azure/combined-wiz9.png)



11.  **Yhteenvedossa** Tarkista tiedot ja valitse **Asenna**. Kun asennus on valmis salasana luodaan. Sinun on se kun otat toistoa siten kopioi se ja pitää sen turvalliseen paikkaan.

    ![Yhteenveto](./media/site-recovery-vmware-to-azure/combined-wiz10.png)

12.  Rekisteröinnin päätyttyä palvelimen näkyy **asetukset** > säilö-**palvelimet** -sivu.



#### <a name="run-setup-from-the-command-line"></a>Asennusohjelman komentoriviltä

Voit määrittää määrityspalvelimen komentoriviltä:

    UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address to be used for data transfer] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>]

Parametrit:

- / ServerMode: pakollinen. Määrittää onko asennettu määritys ja prosessi-palvelimia tai vain prosessi-palvelin. Kirjoita arvot: CS PS.
- InstallLocation: pakollinen. Kansio, jonka osia on asennettu.
- / MySQLCredsFilePath. Pakollinen. Tiedostopolku, johon MySQL server-tunnistetiedot on tallennettu. Tiedoston on oltava tässä muodossa:
    - [MySQLCredentials]
    - MySQLRootPassword = "<Password>"
    - MySQLUserPassword = "<Password>"
- / VaultCredsFilePath. Pakollinen. Säilö tunnistetiedot-tiedoston sijainti
- / EnvType. Pakollinen. Asennuksen tyyppi. Arvot: VMware NonVMware
- / PSIP ja /CSIP. Pakollinen. Prosessi-palvelimen ja määrityspalvelimen IP-osoite.
- / PassphraseFilePath. Pakollinen. Salasana-tiedoston sijainti.
- / BypassProxy. Valinnainen. Määrittää, määrityspalvelimen yhdistetään Azure ilman välityspalvelinta.
- / ProxySettingsFilePath. Valinnainen. Välityspalvelimen (oletusvälityspalvelimen edellyttää todennusta tai mukautettu välityspalvelimen). Tiedoston on oltava tässä muodossa:
    - [ProxySettings]
    - ProxyAuthentication = "Kyllä/ei"
    - Välityspalvelimen IP = "IP-osoite >"
    - ProxyPort = "<Port>"
    - ProxyUserName = "<User Name>"
    - ProxyPassword = "<Password>"
- DataTransferSecurePort. Valinnainen. Porttinumero, jota käytetään replikoinnin tiedot.
- SkipSpaceCheck. Valinnainen. Ohita välimuistin tilan tarkistaminen.
- AcceptThirdpartyEULA. Pakollinen. Merkinnän merkitsee kolmansien osapuolten Käyttöoikeussopimuksen hyväksyminen.
- ShowThirdpartyEULA. Pakollinen. Näyttää kolmannen osapuolen käyttöoikeussopimus. Jos syötteeksi kaikki muut parametrit ohitetaan.

### <a name="add-the-vmware-account-used-for-automatic-discovery"></a>Automaattinen etsiminen käytettäviä VMware-tilin lisääminen

 Kun olet valmistellut käyttöönottoa varten on [luonut VMware tilin](#prepare-an-account-for-automatic-discovery) , voit käyttää Automaattinen etsiminen palauttaminen. Lisää tähän tiliin seuraavasti:

1. Avaa **CSPSConfigtool.exe**. Se on käytettävissä pikakuvakkeen työpöydälle nimellä ja sijaitsevat [asentaa sijainti] \home\svsystems\bin-kansiossa.
2. Valitse **tilien hallinta** > **Lisää tili**.

    ![Tilin lisääminen](./media/site-recovery-vmware-to-azure/credentials1.png)

3. Lisää tili, jota käytetään Automaattinen etsiminen **Tilin tiedot** . Huomaa, että se voi viedä vähintään 15 minuuttia tilin nimen näkyvän portaalissa. Päivitä heti valitsemalla **Configuration Servers** > palvelimen nimi > **Päivitä palvelimeen**.

    ![Tiedot](./media/site-recovery-vmware-to-azure/credentials2.png)

### <a name="connect-to-vsphere-hosts-and-vcenter-servers"></a>Yhteyden muodostaminen vSphere isännät- ja vCenter palvelimet

Jos olet replikoiminen VMware VMs yhdistäminen vSphere isännät- ja vCenter palvelimiin.

1. Varmista, määrityspalvelimen on verkkokäyttö vSphere isännät-ja vCenter palvelimissa.
2. Valitse **Valmistele infrastruktuurin** > **lähde**. **Valmistele** lähteessä Valitse määritys-palvelin ja sitten **+ vCenter** Lisää vSphere isäntä- tai vCenter palvelimen.
3. Määritä **Lisää vCenter** vSphere isäntä- tai vCenter palvelimen kutsumanimi ja Määritä IP-osoite tai Toimialuenimeen. Jätä porttiin 443 paitsi VMware palvelinten määritetään eri porttiin pyyntöjen kuunteleminen. Valitse tili, jota käytetään VMware-palvelimeen. Valitse **OK**.

    ![VMware](./media/site-recovery-vmware-to-azure/vmware-server.png)

    >[AZURE.NOTE] Jos olet lisäämässä vCenter palvelimeen tai vSphere host tilille, jolla ei ole järjestelmänvalvojan oikeuksia vCenter tai Host (isäntä)-palvelimeen, varmista, että tili on nämä oikeudet käytössä: Palvelinkeskukseen, Datastore, kansion, Host, verkon, resurssi, Virtual koneen, vSphere hajautettu valitsinta. Lisäksi vCenter palvelin on tallennustilan näkymät-oikeus.

Sivuston palauttaminen muodostaa asetuksilla voit määrittää ja löytää VMs VMware-palvelimiin.

## <a name="step-3-set-up-the-target-environment"></a>Vaihe 3: kohdeympäristön määrittäminen

Tarkista sinulla tallennustilan tilin replikointi ja jonkin Azure verkossa, johon Azure VMs muodostaa yhteyden automaattisesti jälkeen.

1.  Valitse **Valmistele infrastruktuurin** > **kohde** ja valitse Azure tilauksen, jota haluat käyttää.
2.  Määritä käyttöönoton mallin haluat käyttää VMs jälkeen automaattisesti.
3.  Sivuston palauttaminen tarkistaa, että sinulla on vähintään yksi yhteensopiva Azure tallennustilan asiakkaat ja verkkoja.

    ![Kohde](./media/site-recovery-vmware-to-azure/gs-target.png)

4.  Jos et ole vielä luonut tallennustilan tilin ja haluat luoda sen avulla voit muodostaa napsauttamalla **+ -tallennustilan tilin** tekemään, että tekstiin.  Määritä **Luo tallennustilan tili** -sivu tilin nimi, tyyppi, tilaus ja sijainti. Tilin on oltava sama alue kuin palautus Services säilö.

    ![Tallennustilan](./media/site-recovery-vmware-to-azure/gs-createstorage.png)

    Huomaa, että:

    - Jos haluat luoda perinteinen mallin tallennustilan tilin, joka tehdä Azure-portaalissa. [Opi lisää](../storage/storage-create-storage-account-classic-portal.md)
    - Jos käytät premium-tallennustilan tilin replikoitua tietoja, sinun on vakio lisätallennustilaa tilin määrittäminen kaupan replikoinnin kirjaa joka sieppaus meneillään oleviin muuttuu paikalliset tiedot.

    > [AZURE.NOTE] Intia Keski ja Etelä Intia premium-tallennustilan tilin suojausta ei tällä hetkellä tueta.

4.  Valitse Azure verkkoon. Jos et ole vielä luonut verkon etkä halua tehdä käyttämällä KÄDESSÄ napsauttamalla **+ verkon** tekemään, että tekstiin. Määritä **Luo virtuaalisia verkko** -sivu verkkonimi, osoitealueita, aliverkon tiedot, tilaus ja sijainti. Verkossa on oltava samassa sijainnissa kuin palautus Services säilö.

    ![Verkon](./media/site-recovery-vmware-to-azure/gs-createnetwork.png)

    Jos haluat luoda verkon käyttäen perinteinen malli, joka tehdä Azure-portaalissa. [Lue lisää](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).

## <a name="step-4-set-up-replication-settings"></a>Vaihe 4: Määrittäminen Replikointiasetukset

1. Voit luoda uuden replikoinnin käytännön valitsemalla **Valmistele infrastruktuurin** > **Replikointiasetukset** > **+ luominen ja liitä**.
2. **Luo** ja associate käytäntö määrittää käytännön nimen.
3. Valitse **RPO raja-arvo**: Määritä RPO rajoitus. Ilmoitusten luodaan, kun jatkuvan replikoinnin on suurempi.
5. **Palautus valitsemalla säilytys**Määritä tunteina keston säilytys-ikkuna on jokaiselle palautus kohdalle. Suojatun koneet voidaan palauttaa mihin tahansa kohtaan ikkunassa. Enintään 24 tunnin säilytys tuetaan koneet replikoida premium-tallennustilan.
6. Määritä **sovelluksen yhdenmukaisia tilannevedoksen korkojakso**kuinka usein (minuutteina) palauttaminen pistettä sisältävä sovelluksen yhdenmukaisia tilannevedoksia luodaan.
7. Kun luot replikoinnin käytännön, oletusarvoisesti vastaavaa käytännön luodaan automaattisesti tuntisesta. Esimerkki Jos replikoinnin käytäntö on **puhelukeskuksessa käytännön** tuntisesta käytäntö olla **puhelukeskuksessa-käytäntö-tuntisesta**. Käytäntö ei käytetä, ennen kuin käynnistät tuntisesta.  
8. Valitse **OK** , jos haluat luoda käytännön.

    ![Replikoinnin käytäntö](./media/site-recovery-vmware-to-azure/gs-replication2.png)

9. Kun luot uuden käytännön se liittyy configuration-palvelin automaattisesti. Valitse **OK**.

    ![Replikoinnin käytäntö](./media/site-recovery-vmware-to-azure/gs-replication3.png)


## <a name="step-5-capacity-planning"></a>Vaihe 5: Kapasiteetin suunnittelu

Nyt kun olet luonut oman basic infrastruktuuri, voit määrittää voit ottaa huomioon kapasiteetin suunnittelu ja selvittää, tarvitsetko lisäresursseja.

Sivuston palautus on kapasiteetin suunnittelu auttaa oikean resurssien varaaminen lähde-ympäristössä, sivuston palauttaminen osista, verkko ja tallennustilaa. Voit suorittaa suunnittelija arviot keskimääräinen montako VMs levyille ja tallennustilaa pikatilassa tai yksityiskohtaisessa tilassa, jossa Lisää luvut työmäärää tasolla. Ennen aloittamista sinun on:

- Kerää tietoja replikoinnin ympäristöstä, kuten VMs levyjen VMs kohden ja tallennustilaa levyä kohden.
- Arvio päivittäin muuttaminen (churn) korko on replikoitua tiedoille. Voit [vSphere kapasiteetin suunnittelu laitteen](https://labs.vmware.com/flings/vsphere-replication-capacity-planning-appliance) avulla voit tehdä tämän.

1.  Valitse Lataa työkalu ja suorita sitten **Lataa** . [Lue artikkeli](site-recovery-capacity-planner.md) mukana toimitettava työkalu.
2.  Kun olet valmis valitsemalla **Kyllä** **olet suorittanut kapasiteetin suunnittelua?**

    ![Kapasiteetin suunnittelu](./media/site-recovery-vmware-to-azure/gs-capacity-planning.png)

Alla olevassa taulukossa sieppaa apuna kapasiteetista, suunnittelusta tässä skenaariossa pisteiden lukumäärä.


**Osa** | **Tiedot**
--- | --- | ---
**Replikoinnin** | **Muutos enintään päivittäin korko**, suojattu tietokone käyttää vain yksi prosessi-palvelin ja yksittäisen prosessin palvelimen voit käsitellä päivän muuttaminen korko enintään 2 Teratavua. Näin 2 Teratavua on suurin päivittäinen tiedot muuttuvat, joka tukee suojattujen kone korko.<br/><br/> **Suurin nopeus**– replikoitua koneen voivat kuulua yhden Azure-tallennustilan tilin. Vakio-tallennustilan tilin voi käsitellä enintään 20 000 pyynnöt sekunnissa ja suosittelemme, että pidät IOPS vierekkäin lähdetietokoneen 20 000. Esimerkiksi jos lähde-tietokoneessa, jossa 5 levyjen ja jokaisen levyn Luo 120 IOPS (8K koon) lähteen jälkeen se on sisällä Azure levyn IOPS raja 500 kohden. Numero pakollinen tallennustilan tilien = lähde yhteensä IOPs/20000.
**Määrityspalvelimen** | Configuration-palvelin pitäisi käsitellä muuta korko kapasiteettia koko kaikki työmäärät käynnissä suojatun tietokoneissa, ja se on riittävän kaistanleveyden replikointia jatkuvasti Azure säilöön tiedot.<br/><br/> Paras käytäntö on suositeltavaa, että määrityspalvelimen sijaita samassa verkossa ja Lähiverkon-osiossa kuin koneet, jonka haluat suojata. Voivat sijaita eri verkossa, mutta haluat suojata tietokoneissa on oltava tason 3 verkon näkyvyyden siihen.<br/><br/> Seuraavassa taulukossa on koottu koon suosituksia configuration-palvelin.
**Prosessi-palvelin** | Ensimmäinen prosessi-palvelin asennetaan oletusarvoisesti määritys-palvelimeen. Voit ottaa käyttöön skaalata ympäristön prosessit-palvelimiin. Huomaa, että:<br/><br/> Prosessi-palvelin vastaanottaa replikoinnin tietoja suojatun koneet ja optimoi tallentamisesta välimuistiin, pakkaamisen ja salauksen, ennen kuin lähetät Azure. Prosessin palvelimen koneen pitäisi olla näiden tehtävien suorittamiseen tarvittavat resurssit.<br/><br/> Prosessi-palvelin käyttää levyn mukaan välimuistiin. On suositeltavaa erillisessä välimuistin DVD-levyllä 600 gt vähintään käsitellään verkon pullonkaula tai käyttökatkosta tallennetut muutokset.

### <a name="size-recommendations-for-the-configuration-server"></a>Koon määrityspalvelimen koskevia suosituksia

**SUORITIN** | **Muisti** | **Levyn välimuisti** | **Tietojen muuttaminen korko** | **Suojatun koneet**
--- | --- | --- | --- | ---
8 vCPUs (2 sockets * 4 sydämiä @ 2,5 GHz) | 16 GT | 300 GT | 500 gt tai pienempi | Replikoida alle 100 koneet.
12 vCPUs (2 sockets * 6 sydämiä @ 2,5 GHz) | 18 GT | 600 GT | 1 TT 500 Gigatavua | Replikoida 100 150 laitteiden välillä.
16 vCPUs (2 sockets * 8 sydämiä @ 2,5 GHz) | 32 GIGATAVUA | 1 TT: | 1 TT – 2 TT | Replikoida 150-200 laitteiden välillä.
Toinen prosessi Serverin käyttöönotto | | | > 2 Teratavua | Ottaa käyttöön prosessit palvelimissa, jos olet replikoiminen yli 200 koneet, tai jos päivittäiset tiedot muuttuvat korko on yli 2 Teratavua.

Jos:

- Jokaisessa lähde-koneessa on määritetty 3 levyä on 100 Gigatavua.
- On käytetty 8 SAS asemat 10 k JÄLLEENMYYNTIHINNAN esikuva tallennustilaan RAID 10 välimuistin levyn valintaikkunoihin.

### <a name="size-recommendations-for-the-process-server"></a>Prosessin palvelimen koon koskevia suosituksia

Jos haluat suojata yli 200 koneet tai päivittäinen muuta kurssi on suurempi kuin 2 Teratavua voit lisätä käsittelemään replikoinnin kuormituksen prosessit-palvelimiin. Jos haluat skaalata, voit tehdä seuraavia toimia:

- Lisää määrityspalvelimiin. Voit suojata esimerkiksi enintään 400 koneet kaksi määritys-palvelimiin.
- Lisää prosessit-palvelimia ja käytä näitä käsitellään liikenteen sijaan (tai lisäksi) configuration-palvelin.

Tässä taulukossa on kuvattu tilanne, jossa:

- Suunnittelet ei käytettävä prosessin server configuration-palvelin.
- Olet määrittänyt prosessit-palvelimeen.
- Määrität suojatun näennäiskoneiden käyttämään prosessit-palvelimeen.
- Jokaisessa suojatun lähde-koneessa on määritetty kolme levyä on 100 Gigatavua.

**Määrityspalvelimen** | **Prosessit-palvelin**| **Levyn välimuisti** | **Tietojen muuttaminen korko** | **Suojatun koneet**
--- | --- | --- | --- | ---
8 vCPUs (2 sockets * 4 sydämiä @ 2,5 GHz), 16 Gigatavun muistia | 4 vCPUs (2 sockets * 2 sydämiä @ 2,5 GHz), 8 Gigatavun muistia | 300 GT | 250 gt tai pienempi | Replikoida 85 tai vähemmän.
8 vCPUs (2 sockets * 4 sydämiä @ 2,5 GHz), 16 Gigatavun muistia | 8 vCPUs (2 sockets * 4 sydämiä @ 2,5 GHz)-12 Gigatavun muistia | 600 GT | 1 TT 250 Gigatavua | Replikoida 85 150 laitteiden välillä.
12 vCPUs (2 sockets * 6 sydämiä @ 2,5 GHz), 18 Gigatavun muistia | 12 vCPUs (2 sockets * 6 sydämiä @ 2,5 GHz) 24 Gigatavun muistia | 1 TT: | 1 TT – 2 TT | Replikoida 150 225 laitteiden välillä.


Tapa, jossa skaalata palvelinten määräytyvät yhteystietohakutulosten asteikolla ylös tai skaalata mallin ulos.  Skaalata käyttöönotto muutaman tehokkaimpia määritys ja prosessi-palvelimia tai skaalata ulos käyttöönotto palvelimien vähemmän tiedoilla. Esimerkki: Jos haluat suojata 220 koneet voi tehdä jommankumman seuraavista:

- 12vCPU, 18 gigatavun (gt) muistia, prosessit-palvelimen kanssa 12vCPU, 24 gigatavun (gt) muistia määrityspalvelimen ja suojatun koneet vain prosessit-palvelimen määrittäminen.
- Voit myös määrittää kahden määritysten servers (2 x 8vCPU, 16 gt RAM-Muistia) ja kaksi prosessit-servers (x 8vCPU 1) ja 4vCPU x 1 käsittelemään 135 + 85 (220) koneet ja määrittäminen käyttämään vain prosessit-palvelimia suojatun koneet.

Prosessit-palvelimen määrittäminen [seuraavien ohjeiden mukaisesti](#deploy-additional-process-servers) .

### <a name="network-bandwidth-considerations"></a>Verkon kaistanleveyden huomioon otettavia seikkoja

Voit laskea replikoinnin (ensimmäinen replikointi ja sitten delta) tarvittavan kaistanleveyden kapasiteetin suunnittelutyökalua. Voit hallita määrää kaistanleveyden käytön replikoinnin sinulla on useita vaihtoehtoja:

- **Kaistanleveyden**: VMware liikenteestä, joka kopioi Azure siirtyy prosessi-palvelimen kautta. Voit kaistanleveyden koneet, prosessi-palvelimet käynnissä.
- **Vaikuttaa kaistanleveyden**: voivat vaikuttaa replikoinnin pari rekisteriavaimet käyttämällä kaistanleveyden:
    - **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\UploadThreadsPerVM** rekisteriarvon määrittää viestiketjuissa siirtyminen, joita käytetään DVD-levyllä siirtää tietoja (alku- tai delta sallittuja). Suurempi arvo kasvaa replikoinnin käytettäviä kaistanleveys.
    - **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\DownloadThreadsPerVM** määrittää viestiketjuissa siirtyminen tuntisesta tiedonsiirron käytettyä määrän.

#### <a name="throttle-bandwidth"></a>Kaistanleveyden

1. Avaa Microsoft Azure varmuuskopiointi MMC-laajennuksen visuaalisessa muodossa prosessin palvelimen tietokoneeseen. Microsoft Azure varmuuskopiointi pikakuvake on oletusarvoisesti käytettävissä työpöydällä tai C:\Program Files\Microsoft Azure palautus Services Agent\bin\wabadmin.
2. Valitse laajennuksen **Ominaisuuksien muuttaminen**.

    ![Kaistanleveyden](./media/site-recovery-vmware-to-azure/throttle1.png)

3. **Throttling** -välilehden **käyttöön Internetin kaistanleveyden käytön rajoittaminen backup toimille**, valitse Määritä työn rajat ja vapaa-aika tuntia. Kelvollista alueista on 512 Kbps 102 Mbps sekunnissa.

    ![Kaistanleveyden](./media/site-recovery-vmware-to-azure/throttle2.png)

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

## <a name="step-6-replicate-applications"></a>Vaihe 6: Rinnakkaisten sovellukset

Varmista, että koneet, jonka haluat toistaa Mobility-palvelun asennuksen valmisteltu ja ottaa replikoinnin.

### <a name="install-the-mobility-service"></a>Asenna Mobility-palvelu

Ensimmäinen vaihe näennäiskoneiden ja fyysiset palvelimet suojauksen ottaminen käyttöön on Mobility-palvelun asentaminen. Voit tehdä tämän usealla tavalla:

- **Prosessin server push**: Kun otat replikoinnin tietokoneeseen, push ja asenna Mobility palvelun osa prosessi-palvelimesta. Huomaa, että push-asennus ei ilmetä, jos tietokoneissa on jo käytössä osan ylös todate-versiota.
- **Yrityksen push**: Asenna automaattisesti osa yrityksen push-prosessi, kuten WSUS tai System Center määritysten hallinta tai [Azure automaatio ja halutuksi tilan määritysten](./site-recovery-automate-mobility-service-install.md)avulla. Määritä määrityspalvelimen ennen tämän tekemistä.
- **Manuaalinen asennus**: Asenna osa manuaalisesti jokaiseen tietokoneeseen, johon haluat kopioida. Määritä määrityspalvelimen ennen tämän tekemistä.


#### <a name="prepare-for-automatic-push-on-windows-machines"></a>Windows-tietokoneissa automaattinen push valmisteleminen

Näin voit laatia Windows-tietokoneissa Mobility-palvelun voi asentaa automaattisesti prosessi-palvelin.

1.  Luo tili, jonka avulla voidaan prosessi-palvelin käyttää tietokoneessa. Tilin on oltava järjestelmänvalvojan oikeudet (paikallinen tai toimialue) ja niitä käytetään vain push-asennusta varten.

    >[AZURE.NOTE] Jos et käytä toimialueen-tili, sinun on käyttäjän etäkäyttöpalvelimen ohjausobjektin paikallisessa tietokoneessa käytöstä. Voit tehdä tämän HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System rekisteriin Lisää DWORD-merkinnän LocalAccountTokenFilterPolicy arvo on 1. Rekisterimerkinnän lisääminen CLI tyyppi **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`**.

2.  Windowsin palomuuri koneen haluat suojata, valitse **Salli sovelluksen tai ominaisuuden palomuurin läpi**. Ota **tiedostojen ja tulostimien jakaminen** ja **Ohjattu toiminto**. Voit määrittää Ryhmäkäytäntöobjekti palomuuriasetukset tietokoneissa, joihin kuuluvat toimialueeseen.

    ![Palomuuriasetukset](./media/site-recovery-vmware-to-azure/mobility1.png)

2. Lisää tili, jonka loit:

    - Avaa **cspsconfigtool**. Se on käytettävissä pikakuvakkeen työpöydälle nimellä ja sijaitsevat [asentaa sijainti] \home\svsystems\bin-kansiossa.
    - **Tilien hallinta** -välilehti Valitse **Lisää tili**.
    - Lisää tili, jonka loit. Kun olet lisännyt tilin tarvitset tunnistetietoja, kun otat replikoinnin tietokoneelle.


#### <a name="prepare-for-automatic-push-on-linux-servers"></a>Automaattinen push Linux palvelimissa valmisteleminen

1.  Varmista, että haluat suojata Linux koneen tuetaan kuvatulla tavalla [suojattujen kone edellytykset](#protected-machine-prerequisites). Varmista on verkon Linux-koneen ja prosessin palvelimen väliset yhteydet.

2.  Luo tili, jonka avulla voidaan prosessi-palvelin käyttää tietokoneessa. Tilin on oltava pääkansion käyttäjän tietolähteen Linux-palvelimeen ja niitä käytetään vain push-asennusta varten.

    - Avaa **cspsconfigtool**. Se on käytettävissä pikakuvakkeen työpöydälle nimellä ja sijaitsevat [asentaa sijainti] \home\svsystems\bin-kansiossa.
    - **Tilien hallinta** -välilehti Valitse **Lisää tili**.
    - Lisää tili, jonka loit. Kun olet lisännyt tilin tarvitset tunnistetietoja, kun otat replikoinnin tietokoneelle.

3.  Tarkista, että Linux lähdepalvelimeen /etc/hosts-tiedosto on tapahtumat, jotka vastaavat paikallisen hostname liittyvät kaikkien verkkosovittimien IP-osoitteisiin.
4.  Asenna uusimmat openssh, openssh-palvelin, openssl pakettien haluat toistaa tietokoneeseen.
5.  Varmista SSH on käytössä ja käytössä porttiin 22.
6.  Ottaa käyttöön SFTP Alirakenne ja salasanan todennuksen sshd_config tiedoston seuraavasti:

    - Kirjaudu sisään ylimmällä tasolla.
    - Etsi rivi, joka alkaa **PasswordAuthentication**/etc/ssh/sshd_config tiedosto.
    - Rivin kommentointi ja vaihda arvo **ei ole** **Kyllä**.
    - Etsi rivi, joka alkaa **Alirakenne** ja kommentointi rivi.

        ![Linux](./media/site-recovery-vmware-to-azure/mobility2.png)


### <a name="install-the-mobility-service-manually"></a>Mobility-palvelun asentaminen manuaalisesti

Asennusohjelmia ovat käytettävissä **C:\Program Files (x86) \Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository**määritys-palvelimeen.

Lähde-käyttöjärjestelmä | Mobiilikäytön palvelun asennustiedosto
--- | ---
Windows Server (vain 64-bittinen) | Microsoft-ASR_UA_9. *.0.0_Windows_* release.exe
CentOS 6.4, 6.5 6.6 (vain 64-bittinen) | Microsoft-ASR_UA_9. *.0.0_RHEL6-64_*release.tar.gz
SUSE Linux Enterprise Server 11 SP3 (vain 64-bittinen) | Microsoft-ASR_UA_9. *.0.0_SLES11-SP3-64_*release.tar.gz
Oracle Enterprise Linux 6.4, 6.5 (vain 64-bittinen) | Microsoft-ASR_UA_9. *.0.0_OL6-64_*release.tar.gz


#### <a name="install-mobility-service-on-a-windows-server"></a>Asenna Windows Server Mobility-palvelu


1. Lataa ja suorita asiaa asennusohjelma.
2. Valitse **ennen kuin aloitat** **Mobility-palvelun**.

    ![Mobility-palvelu](./media/site-recovery-vmware-to-azure/mobility3.png)

3. Määritä **Palvelimen tiedot** määrityspalvelimen ja salasana, joka on luotu, kun suoritit yhdistetyn asennuksen IP-osoite. Voit noutaa salasana suorittamalla: ** <SiteRecoveryInstallationFolder>\home\sysystems\bin\genpassphrase.exe – v** määritys-palvelimeen.

    ![Mobility-palvelu](./media/site-recovery-vmware-to-azure/mobility6.png)

4. **Asenna** sijainnissa jätä oletusasetus ja valitse **Seuraava** ja aloita asennus.
5. **Asennus** käynnissä valvoa asennus ja Käynnistä tietokone uudelleen, jos ohjelma pyytää sitä. Palvelun asentamisen jälkeen voi kestää noin 15 minuuttia, portaalissa Päivitä tila.

#### <a name="install-mobility-service-on-a-windows-server-using-the-command-prompt"></a>Mobility-palvelun asentaminen Windows Serverissä komentokehotteen käyttäminen

1. Kopioi asennusohjelma paikalliseen kansioon (vaikkapa C:\Temp)-palvelimeen, jonka haluat suojata. Asennusohjelma löytyy kohdasta **[asentaa sijainti] \home\svsystems\pushinstallsvc\repository**määrityspalvelimen. Package for Windows-käyttöjärjestelmä on Microsoft-ASR_UA_9.3.0.0_Windows_GA_17thAug2016_release.exe samalla nimi
2. MobilitySvcInstaller.exe tiedoston **nimeäminen uudelleen**
3. Suorita seuraava komento MSI-asennusohjelman purkaminen </br>

        C:\> cd C:\Temp
        C:\Temp> MobilitySvcInstaller.exe /q /xC:\Temp\Extracted
        C:\Temp> cd Extracted
        C:\Temp\Extracted> UnifiedAgent.exe /Role "Agent" /CSEndpoint "IP Address of Configuration Server" /PassphraseFilePath <Full path to the passphrase file>

#####<a name="full-command-line-syntax"></a>Täysi komentorivin syntaksi

    UnifiedAgent.exe [/Role <Agent/MasterTarget>] [/InstallLocation <Installation Directory>] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>] [/LogFilePath <Log File Path>]<br/>

**Parametrit**

- **/Role:** Pakollinen. Määrittää, onko Mobility-palvelun asennettuna. Kirjoita arvot agentti | MasterTarget
- **/InstallLocation:** Pakollinen. Määrittää minne palvelu.
- **/PassphraseFilePath:** Pakollinen. Määritys-palvelimen salasana.
- **/LogFilePath:** Pakollinen. Sijainti, johon asennuksen lokitiedostoja luodaan.



#### <a name="uninstall-mobility-service-manually"></a>Mobility-palvelun asennuksen poistaminen manuaalisesti

Mobiilikäytön palvelun voidaan poistaa käyttämällä Lisää Poista ohjelma Ohjauspaneelin kautta tai komentorivin avulla.

Komennon komentoriviltä Mobility-palvelun asennuksen poistaminen

    MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1}


#### <a name="install-mobility-service-on-a-linux-server-using-command-line"></a>Mobility-palvelun asentaminen käyttämällä komentorivin Linux palvelimessa

1. Kopioi edellä olevan taulukon haluat toistaa Linux koneen perusteella tarvittavat tar arkisto.
2. Avaa shell-ohjelmasta ja purkaa zip tar arkisto paikallisen polun suorittamalla:`tar -xvzf Microsoft-ASR_UA_8.5.0.0*`
3. Luo passphrase.txt tiedoston paikalliseen kansioon, johon purit tar arkiston sisällön. Voit tehdä tämän kopioi salasana C:\ProgramData\Microsoft Azure sivuston Recovery\private\connection.passphrase määritys-palvelimessa ja tallenna se passphrase.txt suorittamalla *`echo <passphrase> >passphrase.txt`* -käyttöliittymän.
4. Mobility-palvelun asentaminen suorittamalla *`sudo ./install -t both -a host -R Agent -d /usr/local/ASR -i <IP address> -p <port> -s y -c https -P passphrase.txt`*.
5. Määritä sisäisen IP-osoite määrityspalvelimen ja varmista, että porttiin 443 on valittuna. Palvelun asentamisen jälkeen voi kestää noin 15 minuuttia, portaalissa Päivitä tila.

**Voit myös asentaa komentoriviltä**:

1. Kopioi C:\Program Files (x86) \InMage Systems\private\connection määritys-palvelimeen salasana ja Tallenna "passphrase.txt" määritys-palvelimeen. Suorittamalla nämä komennot. Tässä esimerkissä määritysten palvelimen IP-osoite on 104.40.75.37 ja HTTPS-porttiin 443 on oltava:

Asenna tuotannon-palvelimeen:

    ./install -t both -a host -R Agent -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt

Asenna perusmuodon kohde-palvelimeen:


    ./install -t both -a host -R MasterTarget -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt


### <a name="enable-replication"></a>Replikoinnin ottaminen käyttöön

#### <a name="before-you-start"></a>Ennen aloittamista

Jos olet replikoiminen VMware virtual koneet Huomaa seuraavat:

- VMware VMs löytyy 15 minuutin välein ja saattaa kestää 15 minuuttia tai pidentää niiden portaalissa etsiminen jälkeen. Discovery voi myöskään kestää vähintään 15 minuuttia kun lisäät uuden vCenter palvelimeen tai vSphere host.
- Ympäristön muutokset (kuten VMware Työkalut asennus) virtuaalikoneen saattaa kestää myös vähintään 15 minuuttia portaalissa päivittäminen kestää.
- Voit tarkistaa havaitun viimeksi VMware VMs **Viimeisen yhteyshenkilön osoitteessa** -kenttään, vCenter palvelin/vSphere isännän **Kokoonpano-palvelimet** -sivu.
- Voit lisätä koneet replikoinnin ajoitetun etsiminen odottamatta, korosta määrityspalvelimen (Älä valitse) ja valitse **Päivitä** -painiketta.
- Kun otat replikoinnin, jos tietokoneessa on valmisteltu prosessi-palvelimen automaattisesti asentaa Mobility-palvelun sitä.

#### <a name="exclude-disks-from-replication"></a>Levyjen pois sallittuja

Kun otat replikoinnin, oletusarvoisesti kaikki levyjä tietokoneeseen, replikoida. Voit jättää levyjen replikoinnin pois. Esimerkiksi ehkä halua replikoida levyjen tilapäinen tietoja tai tietoja, jotka on päivitetty aina, kun kone tai sovellus käynnistetään uudelleen (esimerkiksi pagefile.sys tai SQL Server tempdb). Jos haluat jättää pois levyjen Huomaa, että:

- Voit jättää pois vain levyjä, jotka on jo asennettu Mobility-palvelu. Sinun on [Mobility-palvelun](#install-the-mobility-service-manually) asentaminen manuaalisesti koska Mobility-palvelu on asennettu vain käyttämällä push-järjestelmä, kun replikointi on otettu käyttöön.
- Vain basic levyjä jätetään pois replikoinnin. Ei voi poistaa OS tai dynaamisiksi.
- Kun replikointi on otettu käyttöön ei voi lisätä tai poistaa levyjen toistoa. Jos haluat lisätä tai poistaa DVD-levyllä, sinun on tietokoneen suojauksen käytöstä ja ottaa sen sitten uudelleen käyttöön.
- Jos DVD-levyllä, joka tarvitaan sovelluksen toimimaan pois jälkeen Azure automaattisesti tarvitset luominen sen manuaalisesti Azure niin, että replikoitua sovelluksen suorittamisen. Voit myös napsauttaa Azure automaatio voi integroida palautus suunnitelman luominen levy automaattisesti tietokoneen aikana.
- Levyjä, voit luoda manuaalisesti Azure epäonnistuu takaisin. Esimerkiksi jos epäonnistua kolmen levyille ja luo kaksi suoraan Azure, kaikki viisi epäonnistuu takaisin. Luotu manuaalisesti tuntisesta levyjä et voi jättää pois.

**Ota nyt replikoinnin seuraavasti**:

1. Valitse **Vaihe 2: replikoida sovelluksen** > **lähde**. Kun olet replikoinnin ensimmäistä kertaa Valitse **+ replikoida** muihin tietokoneisiin replikoinnin käyttöön säilöön.
2. **Lähde** -sivu-> **lähde** Valitse määritys-palvelin.
3. Valitse **tietokoneen tyyppi** **näennäiskoneiden** tai **Fyysisten laitteiden**.
4. Valitse **vCenter/vSphere Hypervisor** vCenter-palvelin, joka hallitsee vSphere-isäntä tai valitse isäntä. Tämä asetus ei ole merkitystä, jos olet replikoiminen fyysisten laitteiden.
5. Valitse prosessi-palvelin. Jos et ole luonut prosessit palvelimia tämä on määrityspalvelimen nimi. Valitse **OK**.

    ![Replikoinnin ottaminen käyttöön](./media/site-recovery-vmware-to-azure/enable-replication2.png)

6. **Kohde** säilö tilaus ja valitse **Kirjaa automaattisesti käyttöönoton mallin** mallin (perinteinen tai resurssi management), jota haluat käyttää Azure jälkeen automaattisesti.
7. Azure-tallennustilan tilin käytät varten replikoiminen tietojen valitseminen Huomaa, että:

    - Voit valita premium tai standard tallennustilan tilin. Jos valitset premium-tilin, sinun on määritettävä vakio lisätallennustilaa tilin jatkuvan replikoinnin lokitiedot. Tilit on oltava sama alue kuin palautus Services säilö.
    - Jos haluat käyttää eri tallennustilan tiliä kuin ne on voit voit [luoda](#set-up-an-azure-storage-account). Voit luoda tallennustilan ARM-mallin tili valitsemalla **Luo uusi**. Jos haluat luoda perinteinen mallin tallennustilan tilin Tee kyseisen [Azure-portaalissa](../storage/storage-create-storage-account-classic-portal.md).

8. Valitse Azure verkko- ja aliverkon, johon Azure VMs muodostaa yhteyden, kun ne on kehrätty jälkeen automaattisesti. Verkossa on oltava sama alue kuin palautus Services säilö. Valitse **Määritä nyt valitun koneet,** verkon koskee suojautumista kaikissa-tietokoneissa valitset. Valitse **Määritä myöhemmin** kohti koneen Azure-verkosto. Jos sinulla ei ole verkon tarvitset [luomaan](#set-up-an-azure-network). Voit luoda ARM-mallin avulla valitsemalla **Luo uusi**. Jos haluat luoda perinteinen mallin avulla voit tehdä kyseisen [Azure-portaalissa](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). Valitse aliverkon tarvittaessa. Valitse **OK**.

    ![Replikoinnin ottaminen käyttöön](./media/site-recovery-vmware-to-azure/enable-replication3.png)

9. **Näennäiskoneiden** > **näennäiskoneiden** Valitse ja valitse Jokaisessa koneessa haluat kopioida. Voit valita vain koneet, jossa on käytössä replikoinnin. Valitse **OK**.

    ![Replikoinnin ottaminen käyttöön](./media/site-recovery-vmware-to-azure/enable-replication5.png)

10. **Ominaisuuksien** > **Määritä ominaisuudet**, valitse tili, jota käytetään automaattisesti prosessin palvelimen asennuksen Mobility-palvelun tietokoneeseen. Oletusarvon mukaan kaikki levyjä, replikoida. Valitse **Kaikki levyjen** ja poista kaikki levyjä, joita et halua replikoida. Valitse **OK**. Voit määrittää muita ominaisuuksia myöhemmin.

    ![Replikoinnin ottaminen käyttöön](./media/site-recovery-vmware-to-azure/enable-replication6.png)

11. Valitse **Replikointiasetukset** > **replikoinnin asetusten määrittäminen** varmistamalla, että oikea replikoinnin käytäntö on valittuna. Voit muokata replikoinnin ryhmäkäytäntöasetukset **asetukset** > **replikoinnin käytännöt** > käytännön nimi > **Muokkaa asetuksia**. Käytäntöä sovelletaan muutokset otetaan käyttöön replikoiminen ja uusi koneet.

12. Ota käyttöön **usean AM yhdenmukaisuuden** , jos haluat kerätä koneet replikoinnin ryhmään, sekä ryhmän nimi. Valitse **OK**. Huomaa, että:

    - Replikoinnin koneet toisinto ryhmittelyyn ja olet jakanut kaatumisen yhdenmukaisia ja sovelluksen yhdenmukaisia palauttamisen pisteiden ne epäonnistuvat päälle.
    - On suositeltavaa, että sinulla on tarvitsemasi VMs ja fyysinen palvelinten yhteen niin, että ne vastaavat oman toiminnoista. Ottaminen käyttöön usean AM yhdenmukaisuuden voi vaikuttaa suorituskykyyn työmäärää ja käytetään vain, jos tietokoneissa on sama työmäärää ja tarvitset yhdenmukaisuuden.

    ![Replikoinnin ottaminen käyttöön](./media/site-recovery-vmware-to-azure/enable-replication7.png)

13. Valitse **Ota käyttöön toistoa**. **Asetusten** **Ottaminen käyttöön suojaus** työn edistymistä voi seurata > **työt** > **Sivuston palauttaminen työt**. Kun **Viimeisteleminen suojaus** suoritetaan tietokoneessa on valmiina automaattisesti.

> [AZURE.NOTE] Jos tietokoneessa on valmisteltu push-asennuksen Mobility service-komponentti asennetaan, kun suojaus on käytössä. Kun osa on asennettu suojaus-projekti alkaa machine ja epäonnistuu. Virheen jälkeen joudut uudelleen manuaalisesti Jokaisessa koneessa. Uudelleenkäynnistyksen jälkeen suojaus-työ alkaa uudelleen ja ensimmäinen replikointi tapahtuu.

### <a name="view-and-manage-vm-properties"></a>Tarkastella ja hallita AM ominaisuudet

On suositeltavaa, että tarkistat lähdetietokoneen ominaisuudet. Muista, että Azure AM nimen tulisi mukaisia [Azure virtuaalikoneen vaatimukset](site-recovery-best-practices.md#azure-virtual-machine-requirements).

1. Valitse **asetukset** > **replikoitu kohteet** > ja valitse tietokone. **Essentials** -sivu, jossa tietoja tilan ja koneet asetukset.

2. Voit tarkastella **Ominaisuudet** AM tietoja replikoinnin ja automaattisesti.

    ![Replikoinnin ottaminen käyttöön](./media/site-recovery-vmware-to-azure/test-failover2.png)

3. **Laske**-ja verkon > **Laske ominaisuudet** , voit määrittää Azure AM nimi- ja kohdesivustojen kokoa. Muokkaa tarvittaessa Azure vaatimusten mukainen nimi.
Voit myös tarkastella ja Lisää kohde verkko-, aliverkon, ja IP-osoitetta, joka määritetään Azure AM tietoja. Ota seuraavat seikat huomioon:

    - Voit määrittää kohde-IP-osoite. Jos et anna osoitetta epäonnistunutta päälle koneen käyttää DHCP. Jos määrität osoitetta, joka ei ole käytettävissä osoitteessa automaattisesti, vikasietotilaa ei toimi. Sama kohde-IP-osoite voidaan testin automaattisesti, jos osoite ei ole käytettävissä testi automaattisesti verkossa.
    - Verkkosovittimien määrä määräytyy koon kohde-virtuaalikoneen määrittäminen seuraavasti:

        - Jos verkkosovittimien lähde-koneessa määrä on pienempi tai yhtä sovittimet sallittu kohde koneen kokoa, kohde on yhtä monta sovittimet lähde.
        - Jos lähde virtuaalikoneen sovittimet ylittää sallittu, Kohdekoko sitten kohde enimmäiskoko käytetään.
        - Jos lähdetietokoneen on kaksi verkkosovittimien ja kohde koneen koon tukee neljä, kohdetietokoneen on kahden sovittimen esimerkiksi. Jos lähde on kahden sovittimen mutta tuetut kohteen koon tukee vain yksi kohdetietokoneen on vain yksi sovittimen.     
    - Jos AM on useita verkkosovittimien ne kaikki muodostaa yhteyden samassa verkossa.

    ![Replikoinnin ottaminen käyttöön](./media/site-recovery-vmware-to-azure/test-failover4.png)

4. **Kohtauksen** näet käyttöjärjestelmän ja tietojen levyjä, joka voi replikoida AM.


## <a name="step-7-test-the-deployment"></a>Vaihe 7: Käyttöönoton testaaminen

Jotta voit testata käyttöönoton voit suorittaa testi-automaattisesti virtual yhteen tietokoneeseen tai palautus-palvelupaketti, joka sisältää vähintään yhden näennäiskoneiden.


### <a name="prepare-for-failover"></a>Automaattisesti valmisteleminen

- Suorita testi automaattisesti Suosittelemme, että luot uuden Azure verkkoon, joka on eristetty Azure tuotannon verkosta (tämä on oletustoiminnan kun luot uuden verkon Azure). [Lue lisää](site-recovery-failover.md#run-a-test-failover) testi failovers suorittamisesta.
- Saat parhaan suorituskyvyn, jos olet ette Azure päälle, asenna Azure-agentti suojatun tietokoneeseen. Tekee käynnistäminen nopeammin ja auttaa vianmäärityksessä. Asenna [Linux](https://github.com/Azure/WALinuxAgent) - tai [Windows](http://go.microsoft.com/fwlink/?LinkID=394789) -agentti.
- Voit testata täysin käyttöönoton tarvitset infrastruktuuri replikoitua koneen toimii odotetulla tavalla. Jos haluat testata Active Directory- ja DNS-voit luoda virtual machine kuin toimialueen ohjauskoneen DNS ja replikoida tämä Azure käyttämällä Azure palauttaminen. Lue lisää tekstimuodossa [testi automaattisesti Huomioitavaa Active Directorysta](site-recovery-active-directory.md#considerations-for-test-failover).
- Varmista, että configuration-palvelin on käynnissä. Muussa tapauksessa automaattisesti epäonnistuu.
- Jos olet raporttikyselyä replikoinnin levyjen voit joutua luominen näiden levyjen manuaalisesti Azure jälkeen automaattisesti niin, että sovellus toimii odotetulla tavalla.
- Jos haluat suorittaa suunnittelematon automaattisesti, sen sijaan, että testi automaattisesti huomautuksen seuraavasti:

    - Olisi mahdollisuuksien Sammuta ensisijainen koneet, ennen kuin suoritat suunnittelematon automaattisesti. Näin varmistat, että sinulla ei ole lähde- ja replikan, joissa käytetään yhtä aikaa. Jos olet replikoiminen VMware VMs sitten voit määrittää, että sivuston palauttaminen Varmista paras työmäärään sulkeutumaan lähde-tietokoneissa. Ensisijainen sivuston tilan mukaan tämä voi tai ei ehkä toimi. Jos olet replikoiminen fyysiset palvelimet palauttaminen ei voi käyttää asetus.
    - Kun suoritat suunnittelematon automaattisesti lopettaa tietojen replikoinnin suorittamisen ensisijainen koneet niin, että kaikki tiedot delta ei siirretä, kun suunnittelematon automaattisesti alkaa. Lisäksi jos suoritat suunnittelematon automaattisesti palautus-palvelupaketin se suoritetaan, kunnes valmiina, vaikka näyttöön tulee virhesanoma.

### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Yhteyden muodostaminen Azure VMs jälkeen automaattisesti valmistelu

Jos haluat muodostaa yhteyden Azure VMs käyttämällä RDP automaattisesti sen jälkeen, varmista, että seuraavat toimet:

**Ennen automaattisesti paikalliseen tietokoneeseen**:

- Käytön Internetin välityksellä käyttöön RDP, varmista, **julkisen**lisätään TCP- ja UDP-säännöt ja varmistaa, että **Windowsin palomuuri**on sallittua RDP -> **sallitut sovellukset ja toiminnot** profiilien.
- Sivuston sivusto-yhteyden käytön käyttöön RDP tietokoneeseen ja varmistaa, että **Windowsin palomuuri**on sallittua RDP -> **sallitut sovellukset ja toiminnot** **toimialueen** ja **yksityiset** verkkojen.
- Asenna [Azure AM agentti](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) paikalliseen tietokoneeseen.
- [Manuaalinen asennus Mobility-palvelun](#install-the-mobility-service-manually) tietokoneissa sijaan push-palvelun automaattisesti prosessin palvelimen avulla. Tämä johtuu siitä push-asennus tapahtuu vain, kun laite on otettu käyttöön replikoinnin.
- Varmista, että käyttöjärjestelmän SAN käytäntö on määritetty OnlineAll. [Opi lisää]( https://support.microsoft.com/kb/3031135)
- Poistaa IP-palvelun käytöstä, ennen kuin suoritat vikasietotilaa.

**Valitse Azure AM automaattisesti jälkeen**:

- Lisää julkisen päätepiste RDP-protokolla (portti 3389) ja määritä kirjautumisen tunnistetiedot.
- Varmista toimialuekäytäntöjä, jotka estävät yhteyden julkisen osoitteen virtual tietokoneeseen ei ole.
- Yritä muodostaa yhteys. Jos et voi muodostaa Varmista, että AM on käynnissä. Lisää vianmääritysohjeita Lue tämän [artikkelin](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).


Jos haluat käyttää käynnissä Linux automaattisesti suojattu runko Client-asiakkaalla (ssh) jälkeen Azure-AM, toimi seuraavasti:

**Ennen automaattisesti paikalliseen tietokoneeseen**:

- Varmista, että Azure AM suojattu runko-palvelu on määritetty käynnistymään automaattisesti järjestelmän käynnistyksen yhteydessä.
- Tarkista, että palomuurisäännöt salli SSH-yhteyden siihen.

**Valitse Azure AM automaattisesti jälkeen**:

- Verkon suojaussäännöt-epäonnistui AM ja Azure aliverkon, johon se on kytketty on sallittava saapuvia yhteyksiä SSH-porttiin.
- Julkinen päätepisteen luodaanko sallimaan yhteyksiä SSH porttiin (TCP portti 22 oletusarvoisesti).
- Jos VPN-yhteyden (Express reitin tai sivuston-sivusto VPN) kautta AM asiakas voidaan suoraan yhteyden AM SSH päälle.

**Valitse Azure Windows/Linux AM automaattisesti jälkeen**:

Jos sinulla on virtuaalikoneen tai johon koneen kuuluu aliverkon liittyvä verkon käyttöoikeusryhmän, varmista, että verkko-käyttöoikeusryhmän on lähtevällä säännöllä sallimaan HTTP/HTTPS. Varmista myös, mitä virtuaalikoneen verkon DNS on käytön epäonnistui yli on määritetty oikein. Muita vikasietotilaa voitu aikakatkaistaan virhe-"PreFailoverWorkflow tehtävän WaitForScriptExecutionTask aikakatkaisu". Jotta ymmärrät yksityiskohtaisesti, viitata [Seuranta- ja vianmääritysoppaan](site-recovery-monitoring-and-troubleshooting.md#recovery)palautus-osiossa.

## <a name="run-a-test-failover"></a>Suorita testi automaattisesti

1. Epäonnistuu päälle yhteen tietokoneeseen, **asetuksissa** > **Replikoida kohteita**, valitsemalla AM > **+ testi automaattisesti** kuvake.

    ![Testaa automaattisesti](./media/site-recovery-vmware-to-azure/test-failover1.png)

2. Epäonnistuu päälle palautus-palvelupaketti, **asetuksissa** > **Palautus-Palvelupaketit**, suunnitelman hiiren kakkospainikkeella > **Testi automaattisesti**. Luo palautuksen suunnitteleminen [seuraavien ohjeiden mukaisesti](site-recovery-create-recovery-plans.md).

3. Valitse **Testaa automaattisesti** , johon Azure VMs yhdistetään, kun vikasietotila ilmenee Azure verkkoon.
4. Valitse **OK** , jos haluat aloittaa vikasietotilaa. Edistymistä voi seurata napsauttamalla sen ominaisuuksia AM tai **Testi automaattisesti** projektin säilö nimi > **asetukset** > **työt** > **palauttaminen työt**.
5. Kun vikasietotilaa **valmiina testaaminen** tila, toimi seuraavasti:

    1. Tarkastele replikan virtuaalikoneen Azure-portaalissa. Varmista, että virtuaalikoneen käynnistäminen onnistuu.
    2. Jos ole access näennäiskoneiden määrittää paikallisen verkosta voit aloittaa virtuaalikoneen etätyöpöydän yhteyden.
    3. Valitse Viimeistele se **valmiina testaaminen** .

        ![Testaa automaattisesti](./media/site-recovery-vmware-to-azure/test-failover6.png)


    4. Valitse **muistiinpanot** ja tallentaminen testi vikasietotilaa liittyvät huomautukset.
    5. Valitse automaattisesti ajoitettuina testiympäristössä **testi vikasietotilaa on valmis** . Tämän jälkeen testi vikasietotilaa näkyy **valmiina** tila.
    6.  Tässä vaiheessa tahansa osien ja luo automaattisesti palauttaminen aikana testi vikasietotilaa VMs poistetaan. Testaa automaattisesti luomasi muita elementtejä ei poisteta.

    > [AZURE.NOTE] Jos testi automaattisesti edelleen kestää yli kaksi viikkoa on valmis voimassa mukaan.


6. Vikasietotilaa päätyttyä myös pitäisi näe se Azure tietokoneen näkyvän Azure portaalissa > **näennäiskoneiden**. Varmista että AM on haluamasi kokoinen, johon se on kytketty tarvittaessa verkon ja että se on käynnissä.
7. Jos olet [olemalla yhteydet automaattisesti jälkeen](#prepare-to-connect-to-azure-vms-after-failover) sinun pitäisi muodostaa Azure AM.

## <a name="monitor-your-deployment"></a>Käyttöönoton valvonta

Näin, miten voit seurata asetukset, tila ja terveyteen palauttaminen käyttöönottoa varten:

1. Valitse säilö nimi, joka käyttää **Essentials** Raporttinäkymät-ikkunan. Tämä koontinäyttö voit palauttaminen työt, replikoinnin tila, palautus suunnitelmat, palvelimen kunnon ja tapahtumia.  Voit mukauttaa Essentials ruudut ja-asetteluja, jotka ovat hyödyllisimpiä sinua, kuten muiden palauttaminen ja varmuuskopiointi vaults tilaa.<br>
![Olennaiset asiat](./media/site-recovery-vmware-to-azure/essentials.png)

2. **Kunto** -ruudussa voit valvoa sivustoon palvelimet (VMM tai määritysten), joka on ilmennyt ongelma ja tapahtumia aiheutuneet palauttaminen viimeisen 24 tunnin aikana.
3. Voit hallita ja seurata replikointi **Replikoida kohteiden** **Palauttaminen-Palvelupaketit**, ja **Sivuston palauttaminen työt** ruutujen. Voit siirtyä alaspäin työt **asetuksissa** -> **työt** -> **Sivuston palauttaminen työt**.


## <a name="deploy-additional-process-servers"></a>Prosessit-palvelimien käyttöönottaminen

Jos sinulla on skaalata yli 200 lähde-konetta tai yhteensä päivittäinen churn-kurssi on yli 2 Teratavua käyttöönoton ulos, sinun on käsitellään liikenteen äänenvoimakkuuden prosessit-palvelimiin.

Tarkista [koon suosituksia prosessin palvelinten](#size-recommendations-for-the-process-server) ja noudata näitä ohjeita, prosessi-palvelimen määrittäminen. Kun olet määrittänyt palvelimen siirtää tietolähteen koneet käyttämään sitä.

### <a name="install-an-additional-process-server"></a>Asenna prosessit-palvelin

1. **Asetuksissa** > **palauttaminen palvelinten** napsauttamalla määrityspalvelimen > **prosessi-palvelimeen**.

    ![Lisää prosessi-palvelin](./media/site-recovery-vmware-to-azure/migrate-ps1.png)

2. Valitse **Palvelintyyppi** **prosessin server (paikallisena)**.

    ![Lisää prosessi-palvelin](./media/site-recovery-vmware-to-azure/migrate-ps2.png)

3. Sivuston palauttaminen yhdistetyn asennustiedosto Lataa ja suorita sen prosessin Serverin asentaminen ja rekisteröi-säilö.
4. Valitse **Lisää prosessit-palvelinten skaalata käyttöönoton,** **ennen kuin aloitat** .
5. Suorita ohjattu samalla tavalla kuin teit milloin [määrittäminen](#step-2-set-up-the-source-environment) configuration-palvelin.

    ![Lisää prosessi-palvelin](./media/site-recovery-vmware-to-azure/add-ps1.png)

6. Määritä **Palvelimen tiedot** IP-osoite määrityspalvelimen ja salasana. Voit hankkia Suorita salasana ** <SiteRecoveryInstallationFolder>\home\sysystems\bin\genpassphrase.exe – n** määritys-palvelimeen.

    ![Lisää prosessi-palvelin](./media/site-recovery-vmware-to-azure/add-ps2.png)

### <a name="migrate-machines-to-use-the-new-process-server"></a>Siirtää koneet uuden prosessi-palvelimen käyttäminen

1. **Asetuksissa** > **palauttaminen palvelinten** configuration-palvelin ja laajenna sitten **prosessi-palvelimiin**.

    ![Päivitetäänkö palvelin prosessi](./media/site-recovery-vmware-to-azure/migrate-ps2.png)

2. Napsauta käytössä oleva prosessi-palvelin ja valitse **Vaihda**.

    ![Päivitetäänkö palvelin prosessi](./media/site-recovery-vmware-to-azure/migrate-ps3.png)

3. **Valitse kohde prosessi-palvelin**Valitse haluat käyttää ja valitse sitten näennäiskoneiden, uusi prosessi-palvelin käsittelee uuden prosessi-palvelimen. Tietoja palvelimen tiedot-kuvaketta. Avulla voit tehdä ladata päätöksiä näkyy keskiarvo on tarvittavat kunkin valitun virtuaalikoneen replikoida palvelimeen prosessin tila. Valitse Aloita uusi prosessin palvelimeen replikoiminen valintamerkkiä.

## <a name="vmware-account-permissions"></a>VMware tilin käyttöoikeudet

Prosessi-palvelimen automaattisesti löytävät VMs vCenter palvelimessa. Automaattinen etsiminen suorittamiseen tarvitset roolille [(Azure_Site_Recovery)](#prepare-an-account-for-automatic-discovery) sallimaan palauttaminen VMware-palvelinta. Jos tarvitset vain VMware koneet siirtäminen Azure ja ei tarvitse tuntisesta azuren, voit määrittää vain luku-roolin, joka on riittävä. Seuraavaan taulukkoon on koottu tarvittavat roolin käyttöoikeuksia.

**Rooli** | **Tiedot** | **Käyttöoikeudet**
--- | --- | ---
Azure_Site_Recovery rooli | VMware AM etsiminen |Määritä nämä oikeudet v keskelle palvelimen:<br/><br/>Datastore -> Kohdista tilaa, Selaa datastore, pienen tason tiedoston toiminnot, poista tiedoston, Päivitä virtuaalikoneen tiedostot<br/><br/>Verkko -> verkon määrittäminen<br/><br/>Resurssi -> Määritä virtual machine resurssivarantoon artikkelista virtuaalikoneen virta, virtuaalikoneen virta siirtäminen<br/><br/>Tehtävät -> Luo tehtävän tehtävän päivittäminen<br/><br/>Virtuaalikoneen -> määritys<br/><br/>Virtuaalikoneen -> kielikoodeista Answer kysymys, laitteen yhteyden, määrittäminen CD-levylle media, Määritä levyketietovälineitä, Power käytöstä Power -> Valitse, VMware työkalut asennetaan<br/><br/>Virtuaalikoneen -> varaston luominen, rekisteriä, poista -><br/><br/>Virtuaalikoneen -> Provisioning -> Salli virtuaalikoneen Lataa, virtuaalikoneen tiedostojen lataaminen<br/><br/>Virtuaalikoneen -> tilannevedoksia -> Poista tilannevedokset
vCenter käyttäjärooli | VMware AM etsiminen/automaattisesti ilman lähteen AM sulkeminen | Määritä nämä oikeudet v keskelle palvelimen:<br/><br/>Tietokeskuksen objektin –> Propagate ali-objekti, roolin = vain luku-tilassa <br/><br/>Käyttäjä on määritetty palvelinkeskuksen tasolla ja siten käyttää kaikki objektit joten.  Jos haluat rajoittaa käyttöä, määrittää **Propagate lapselle** objektia **ei voi käyttää** -roolin alemman tason objektit (vSphere isännät, datastores, VMs ja verkkoja).
vCenter käyttäjärooli | Automaattisesti ja tuntisesta | Määritä nämä oikeudet v keskelle palvelimen:<br/><br/>Palvelinkeskuksen objektin – Propagate ali-objekti, roolin = Azure_Site_Recovery<br/><br/>Käyttäjä on määritetty palvelinkeskuksen tasolla ja siten käyttää kaikki objektit joten.  Jos haluat rajoittaa käyttöä, määrittää **Propagate lapsen objektia** **ei voi käyttää** rooleihin Aliobjektin (vSphere isännät, datastores, VMs ja verkkoja).  
## <a name="next-steps"></a>Seuraavat vaiheet

- [Lue lisää](site-recovery-failover.md) eri automaattisesti tyyppisiä tietoja.
- [Lisätietoja tuntisesta](site-recovery-failback-azure-to-vmware.md) tuomaan päällä, joissa käytetään Azure oman epäonnistui takaisin paikalliseen ympäristöön.

## <a name="third-party-software-notices-and-information"></a>Kolmannen osapuolen ilmoituksia ja tietoja

Älä kääntäminen tai lokalisoidaan

Ohjelmiston ja laiteohjelmiston käynnissä Microsoftin tuote tai palvelu perustuu tai kattaa materiaali alla projekteista (yhdessä "kolmannen osapuolen Code").  Microsoft ei ole häneltä kolmannen osapuolen-koodin.  Alkuperäinen tekijänoikeusmerkintä sekä käyttöoikeus-kohdassa Microsoft saanut kolmannen osapuolen esimerkiksi koodi on määritetty alla.

Kohdassa A liittyy kolmannen osapuolen koodin osat alla luetellut projektit. Nämä käyttöoikeudet ja tiedot on tarkoitettu vain tiedoksi.  Kolmannen osapuolen koodi on parhaillaan relicensed sinulle Microsoft Microsoftin ohjelmistojen käyttöoikeusehdot, Microsoftin tuote tai palvelu-kohdassa.  

Tietoja b osassa liittyy kolmannen osapuolen koodin osat, jotka tehdään käytettävissä sinulle Microsoft alkuperäisen Käyttöoikeussopimuksen ehtojen mukaisesti.

Koko tiedoston löytyy [Microsoft Download Centeristä](http://go.microsoft.com/fwlink/?LinkId=529428). Microsoft pidättää kaikki oikeudet ole nimenomaisesti myönnetty, onko toteutukseen, vastaväitteitä tai muulla tavoin.

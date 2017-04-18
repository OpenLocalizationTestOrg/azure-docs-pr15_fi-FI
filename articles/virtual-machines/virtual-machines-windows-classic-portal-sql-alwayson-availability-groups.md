<properties
    pageTitle="Määritä aina käytettävyys ryhmän Azure AM – perinteinen"
    description="Aina käytössä oleva käytettävyys-ryhmän luominen Azure-Virtuaalikoneissa kanssa. Tässä opetusohjelmassa käyttää ensisijaisesti käyttöliittymän ja työkalut sen sijaan, että komentosarjat."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/22/2016"
    ms.author="mikeray" />

# <a name="configure-always-on-availability-group-in-azure-vm---classic"></a>Määritä aina käytettävyys ryhmän Azure AM – perinteinen

> [AZURE.SELECTOR]
- [Resurssienhallinta: malli](virtual-machines-windows-portal-sql-alwayson-availability-groups.md)
- [Resurssienhallinta: Manuaalinen](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)
- [Perinteinen: Käyttöliittymä](virtual-machines-windows-classic-portal-sql-alwayson-availability-groups.md)
- [Perinteinen: PowerShell](virtual-machines-windows-classic-ps-sql-alwayson-availability-groups.md)

<br/>

> [AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Lopusta loppuun tässä opetusohjelmassa kerrotaan käytettävyys ryhmien käyttäminen SQL Server aina käynnissä Azuren näennäiskoneiden ottamisesta käyttöön.

Opetusohjelman lopussa Azure SQL Server aina-ratkaisu sisältää seuraavat osat:

- Virtuaalinen verkkoon, joka sisältää useita aliverkosta, mukaan lukien edusta- ja taustatietokantaan aliverkon

- Toimialueen ohjauskoneen Active Directory (AD)-toimialueella

- Kaksi SQL Server VMs käyttöön taustatietokantaan aliverkon ja AD-toimialueen

- 3-solmu WSFC klusterin solmu suurimmalla quorum-malli

- Käytettävyys ryhmä, josta on synkronoitu Vahvista replikat käytettävyys tietokanta

Seuraavassa kuvassa on graafinen esitys-ratkaisun.

![Testaa kurssin arkkitehtuuri AG Azure-tietokannassa](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC791912.png)

Huomaa, että kyseessä on yksi mahdollista määritys. Voit esimerkiksi pienentää VMs kaksi replikan käytettävyys ryhmän määrän tallentamiseksi Laske tunnit Azure käyttämällä toimialueen ohjauskoneen 2-solmu WSFC klusterin quorum tiedoston jakaminen-hallitun. Tätä tapaa vähentää AM Laske jollakin edellä asetuksista.

Tässä opetusohjelmassa oletetaan seuraavasti:

- Sinulla on jo Azure-tili.

- Tiedät voit valmistella perinteinen SQL Server AM avulla Graafisen virtuaalikoneen valikoimasta.

- Sinulla on jo aina käyttöön käytettävyys ryhmien tasainen tietoja. Lisätietoja on kohdassa [Aina käyttöön käytettävyys Groups (SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx).

>[AZURE.NOTE] Jos olet kiinnostunut aina käyttöön käytettävyys ryhmiä käytetään yhdessä SharePointin kanssa, katso myös [Määrittäminen SQL Server 2012 aina edelleen käytettävyys-ryhmiä SharePoint 2013: n](https://technet.microsoft.com/library/jj715261.aspx).

## <a name="create-the-virtual-network-and-domain-controller-server"></a>Luo virtuaalisia verkko- ja toimialueen ohjauskoneen Server

Voit aloittaa uuden Azure kokeilutili. Kun olet määrittänyt tilin määritys, sinulla on oltava aloitusnäytön Azure perinteinen portaalin.

1. Valitse sivun vasemmassa yläkulmassa **Uusi** -painikkeen alla kuvatulla tavalla.

    ![Valitse uusi-portaalissa](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665511.gif)

1. Valitse **Verkkopalveluita**ja valitse **Virtuaaliverkko** ja valitse sitten **Mukautettu luominen**, alla kuvatulla tavalla.

    ![Luo VPN](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665512.gif)

1. **Luo A VIRTUAALIVERKKO** -valintaikkunassa luomalla uuden virtual verkon käymättä läpi sivut asetuksilla. 

  	|Sivun|Asetukset|
|---|---|
|VPN-tiedot|**NIMI = ContosoNET**<br/>**ALUEEN = Länsi US**|
|DNS-palvelimet ja VPN-yhteys|Ei mitään|
|VPN osoite välilyöntejä|Alla olevassa näyttökuvassa esitetään asetukset: ![Luo VPN](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784620.png)|

1. Seuraavaksi voit luoda AM, voit käyttää toimialueen ohjauskoneen. Valitse **Uusi** uudelleen, valitse **Laske**-ja valitse **virtuaalikoneen**ja sitten **-Valikoiman**alla kuvatulla tavalla.

    ![Luo AM](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784621.png)

1. **Luo VIRTUAALIKONEEN** -valintaikkunassa määrittää uuden AM käymättä läpi sivut asetuksilla. 

  	|Sivun|Asetukset|
|---|---|
|Valitse virtuaalikoneen käyttöjärjestelmä|Windows Server 2012 R2 Palvelinkeskukseen|
|Virtuaalikoneen määritys|**VERSION JULKAISUPÄIVÄ** = (uusin)<br/>**VIRTUAALIKONEEN nimi** = ContosoDC<br/>**Taso** = STANDARD<br/>**Koon** = A2 (2 sydämiä)<br/>**Uuden käyttäjänimi** = AzureAdmin<br/>**Uusi salasana** = Contoso! 000<br/>**VAHVISTA** = Contoso! 000|
|Virtuaalikoneen määritys|**PILVIPALVELUSSA** = Luo uuden pilvipalvelussa<br/>**CLOUD palvelun DNS nimi** = yksilöllinen cloud palvelunimi<br/>**DNS-nimi** = yksilöllinen nimi (ex: ContosoDC123)<br/>**Alue/AFFINITEETTI ryhmän/VIRTUAL verkon** = ContosoNET<br/>**VIRTUAL verkon ALIVERKOSTA** = Back(10.10.2.0/24)<br/>**TALLENNUSTILAN tilin** = automaattisesti luotu tallennustilan tilin käyttäminen<br/>**Määritä KÄYTETTÄVYYS** = (ei mitään)|
|Virtuaalikoneen asetukset|Käytä oletusasetuksia|

Kun olet valmis, uusi AM määrittäminen, odota on provsioned AM. Tämä prosessi kestää jonkin aikaa suorittamiseen ja jos valitset Azure perinteinen portaalin **virtuaalikoneen** -välilehteen, näet ContosoDC Lämpötilavaihtelun tiloja **alussa (Provisioning)** **pysäytetty** **käynnistetään**, **suorittaminen (Provisioning)**ja lopuksi **käyttöön**.

Toimialueen Ohjauskoneen palvelin on nyt valmisteltu. Seuraavaksi voit määrittää Active Directory-toimialueen Ohjauskoneen tähän palvelimeen.

## <a name="configure-the-domain-controller"></a>Toimialueen ohjauskoneen määrittäminen

Seuraavissa vaiheissa määrittäminen ContosoDC koneen corp.contoso.com ohjauskoneen nimellä.

1. Valitse **ContosoDC** machine-portaalissa. **Raporttinäkymät-ikkunan** -välilehdessä **Yhdistä** RDP-tiedoston työpöydän etäkäyttöä varten.

    ![Yhteyden muodostaminen Vritual Machine](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784622.png)

1. Kirjaudu sisään oman määritetyn järjestelmänvalvojatilin (**\AzureAdmin**) ja salasana (**Contoso! 000**).

1. Oletusarvon mukaan pitäisi näkyä **Server Managerin** Raporttinäkymät-ikkunan.

1. Valitse **Lisää roolit ja ominaisuudet** -linkki koontinäytössä.

    ![Palvelimen Explorer Lisää roolit](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784623.png)

1. Siirry **Palvelinrooleista** -osan, valitse **Seuraava** .

1. Valitse **Active Directory-toimialueen palveluista** ja **DNS-palvelin** roolit. Kun sinulta kysytään, Lisää rooli vaatii lisäominaisuuksia.

    >[AZURE.NOTE] Näyttöön tulee varoitus, että ei ole staattinen IP-osoite kelpoisuustarkistuksen. Jos testaat määritystä, valitse Jatka. Tuotannon tilanteita, joissa [Voit määrittää toimialueen ohjauskoneen koneen staattinen IP-osoitteen PowerShellin käyttäminen](./virtual-network/virtual-networks-reserved-private-ip.md).

    ![Lisää roolit-valintaikkuna](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784624.png)

1. Valitse **Seuraava** , kunnes pääset **vahvistus** -osassa. Valitse **Käynnistä kohdepalvelin automaattisesti tarvittaessa** -valintaruutu.

1. Valitse **Asenna**.

1. Kun ominaisuuksia asennettaessa, palaa **Server Managerin** Raporttinäkymät-ikkunan.

1. Valitse vasemmanpuoleisessa ruudusta uusi **AD DS** -vaihtoehto.

1. Valitse **Lisää** -linkistä keltainen.

    ![AD DS-valintaikkunan DNS Server AM](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784625.png)

1. Valitse **Kaikki palvelimen tehtävän tiedot** -valintaikkuna **toiminto** -sarakkeessa **ylemmäksi toimialueen ohjauskoneen palvelimen**.

1. **Active Directory toimialueen palvelut ohjattu määritystoiminto**Käytä seuraavia arvoja:

  	|Sivun|Asetus|
|---|---|
|Käyttöönoton määritys|**Lisää uusi metsää** = valittu<br/>**Ensisijaisen toimialuenimen** = corp.contoso.com|
|Toimialueen ohjauskoneen Lisäasetukset|**Salasanan** = Contoso! 000<br/>**Vahvista salasana** = Contoso! 000|

1. Valitse **Seuraava** käsitellään ohjatun toiminnon muita sivuja. Varmista **Edellytykset Tarkista** -sivulla, näet seuraavan sanoman: **kaikki valmistelevat tarkistukset välitetty onnistuneesti**. Huomaa, että Tarkista sovellettavan varoitus viestejä, mutta se ei voi jatkaa asennusta.

1. Valitse **Asenna**. **ContosoDC** virtuaalikoneen käynnistyy automaattisesti uudelleen.

## <a name="configure-domain-accounts"></a>Toimialuetilien määrittäminen

Seuraavat vaiheet myöhempää käyttöä varten Active Directory (AD)-tilien määrittäminen.

1. Kirjaudu takaisin sisään **ContosoDC** koneen.

1. **Server** hallinnassa valitsemalla **Työkalut** ja valitse sitten **Active Directoryn järjestelmänvalvojan keskelle**.

    ![Active Directory-hallintakeskus](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784626.png)

1. Valitse **Active Directory-järjestelmänvalvojan Center** Valitse **corp (paikallinen)** vasemmanpuoleisesta ruudusta.

1. **Tehtävien** oikeanpuoleisen ruudun Valitse **Uusi** ja valitse sitten **käyttäjä**. Käytä seuraavia asetuksia:

  	|Asetus|Arvo|
|---|---|
|**Etunimi**|Asenna|
|**Käyttäjän SamAccountName**|Asenna|
|**Salasana**|Contoson! 000|
|**Vahvista salasana**|Contoson! 000|
|**Muista salasana-asetukset**|Valittu|
|**Salasana aina voimassa**|Valittuna|

1. Valitse **OK** **asentaa** käyttäjän luomiseen. Määritä automaattisesti-klusterin ja käytettävyys-ryhmän käytetään tähän tiliin.

1. Luo kaksi muita käyttäjiä kanssa samalla tavalla: **CORP\SQLSvc1** ja **CORP\SQLSvc2**. Näissä tileissä käytetään SQL Server-esiintymät. Seuraavaksi sinun täytyy antaa **CORP\Install** Windows palvelun automaattisesti klusterointi (WSFC) määrittämiseen tarvittavat käyttöoikeudet.

1. **Active Directory järjestelmänvalvojan keskelle**Valitse vasemmanpuoleisessa ruudussa **corp (paikallinen)** . **Tehtävien** oikeanpuoleisessa ruudussa, valitse **Ominaisuudet**.

    ![CORP käyttäjän ominaisuuksia](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784627.png)

1. Valitse **tunnisteet**ja valitse sitten **Suojaus** -välilehden **Lisäasetukset** -painiketta.

1. **Suojauksen lisäasetukset: corp** -valintaikkunassa. Valitse **Lisää**.

1. Valitse **lyhennyksen**. Hae **CORP\Install**. Valitse **OK**.

1. Valitse **Lue kaikki ominaisuudet** ja **Luo tietokoneen objektien** käyttöoikeudet.

    ![Corp käyttöoikeudet](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784628.png)

1. Valitse **OK**ja valitse sitten **OK** . Sulje corp ominaisuudet-ikkuna.

Nyt kun olet määrittäminen Active Directory- ja käyttäjä-objektit, luodaan kolme SQL Server VMs ja liittäminen toimialueessa.

## <a name="create-the-sql-server-vms"></a>SQL Server-VMs luominen

Seuraavaksi luodaan kolme VMs, mukaan lukien WSFC klusterisolmu ja kaksi SQL Server VMs. Voit luoda eri VMs palaa Azure perinteinen-portaaliin ja valitse **Uusi**, **Laske**, **virtuaalikoneen**ja sitten **-Valikoima**. Käytä malleja seuraavan taulukon avulla voit luoda VMs.

|Sivun|VM1|VM2|VM3|
|---|---|---|---|
|Valitse virtuaalikoneen käyttöjärjestelmä|**Windows Server 2012 R2 Palvelinkeskukseen**|**SQL Server 2014 RTM (Enterprise)**|**SQL Server 2014 RTM Enterprise**|
|Virtuaalikoneen määritys|**VERSION JULKAISUPÄIVÄ** = (uusin)<br/>**VIRTUAALIKONEEN nimi** = ContosoWSFCNode<br/>**Taso** = STANDARD<br/>**Koon** = A2 (2 sydämiä)<br/>**Uuden käyttäjänimi** = AzureAdmin<br/>**Uusi salasana** = Contoso! 000<br/>**VAHVISTA** = Contoso! 000|**VERSION JULKAISUPÄIVÄ** = (uusin)<br/>**VIRTUAALIKONEEN nimi** = ContosoSQL1<br/>**Taso** = STANDARD<br/>**Koon** = A3 (4 sydämiä)<br/>**Uuden käyttäjänimi** = AzureAdmin<br/>**Uusi salasana** = Contoso! 000<br/>**VAHVISTA** = Contoso! 000|**VERSION JULKAISUPÄIVÄ** = (uusin)<br/>**VIRTUAALIKONEEN nimi** = ContosoSQL2<br/>**Taso** = STANDARD<br/>**Koon** = A3 (4 sydämiä)<br/>**Uuden käyttäjänimi** = AzureAdmin<br/>**Uusi salasana** = Contoso! 000<br/>**VAHVISTA** = Contoso! 000|
|Virtuaalikoneen määritys|**PILVIPALVELUSSA** = aiemmin luodun yksilöllisen Cloud palvelun DNS nimi (ex: ContosoDC123)<br/>**Alue/AFFINITEETTI ryhmän/VIRTUAL verkon** = ContosoNET<br/>**VIRTUAL verkon ALIVERKOSTA** = Back(10.10.2.0/24)<br/>**TALLENNUSTILAN tilin** = automaattisesti luotu tallennustilan tilin käyttäminen<br/>**Määritä KÄYTETTÄVYYS** = luominen saatavuus määrittäminen<br/>**KÄYTETTÄVYYS MÄÄRITTÄÄ nimen** = SQLHADR|**PILVIPALVELUSSA** = aiemmin luodun yksilöllisen Cloud palvelun DNS nimi (ex: ContosoDC123)<br/>**Alue/AFFINITEETTI ryhmän/VIRTUAL verkon** = ContosoNET<br/>**VIRTUAL verkon ALIVERKOSTA** = Back(10.10.2.0/24)<br/>**TALLENNUSTILAN tilin** = automaattisesti luotu tallennustilan tilin käyttäminen<br/>**Määritä KÄYTETTÄVYYS** = SQLHADR (Voit myös määrittää käytettävyys määrittää tietokoneen luomisen jälkeen. Kaikkien kolmen koneet määritetään SQLHADR käytettävyys joukkoon.)|**PILVIPALVELUSSA** = aiemmin luodun yksilöllisen Cloud palvelun DNS nimi (ex: ContosoDC123)<br/>**Alue/AFFINITEETTI ryhmän/VIRTUAL verkon** = ContosoNET<br/>**VIRTUAL verkon ALIVERKOSTA** = Back(10.10.2.0/24)<br/>**TALLENNUSTILAN tilin** = automaattisesti luotu tallennustilan tilin käyttäminen<br/>**Määritä KÄYTETTÄVYYS** = SQLHADR (Voit myös määrittää käytettävyys määrittää tietokoneen luomisen jälkeen. Kaikkien kolmen koneet määritetään SQLHADR käytettävyys joukkoon.)|
|Virtuaalikoneen asetukset|Käytä oletusasetuksia|Käytä oletusasetuksia|Käytä oletusasetuksia|

<br/>

>[AZURE.NOTE] Edellinen kokoonpano ehdottaa VAKIO taso näennäiskoneiden, koska BASIC taso koneet eivät tue kuormituksen päätepisteet tarvitse luoda myöhemmin käytettävyys ryhmän kuuntelijoita. Myös tietokoneen koot ehdotetut tähän on tarkoitettu testikäyttöön käytettävyys ryhmät Azure VMs. Tuotannon työmääriä parhaan suorituskyvyn Katso SQL Server machine kokojen ja [suorituskyvyn parhaita käytäntöjä Azuren näennäiskoneiden SQL Server-palvelimen](virtual-machines-windows-sql-performance.md)määrittäminen suosituksia.

Kun kolme VMs täysin valmistelun yhteydessä, tarvitset liittäminen **corp.contoso.com** toimialueen ja myönnä CORP\Install koneet järjestelmänvalvojan oikeudet. Voit tehdä tämän Käytä kunkin kolme VMs seuraavien ohjeiden mukaisesti.

1. Muuta ensin ensisijainen DNS-palvelimen osoite. Aloita lataamalla kunkin AM Etätyöpöytä (RDP) tiedoston paikalliseen kansioon AM valitsemalla luettelosta ja napsauttamalla **Muodosta** -painiketta. Valitse AM napsauttamalla mitä tahansa rivin ensimmäiseen soluun mutta alla kuvatulla tavalla.

    ![Lataa RDP-tiedosto](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC664953.jpg)

1. Käynnistä RDP-tiedostoon, voit ladata ja kirjaudu sisään käyttämällä omaa määritetyn AM järjestelmänvalvojatilin (**BUILTIN\AzureAdmin**) ja salasana (**Contoso! 000**).

1. Kun olet kirjautunut sisään, pitäisi näkyä **Server Managerin** Raporttinäkymät-ikkunan. Valitse vasemmanpuoleisessa ruudussa **Paikalliseen palvelimeen** .

1. Valitse **IPv4-osoitteen määrittämä DHCP käytössä IPv6** -linkki.

1. Valitse **Verkkoyhteydet** -ikkunassa Verkko-kuvaketta.

    ![Muuta AM ensisijainen DNS-palvelin](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784629.png)

1. Valitse komentopalkista **yhteyden asetusten muuttaminen** (mukaan ikkunan suuri, voit joutua napsauttamaan näet tämän komennon Kaksoisnuoli).

1. Valitse **Internet Protocol Version 4 (TCP/IPv4)** ja valitse Ominaisuudet.

1. Valitse Käytä DNS-palvelin-osoitteet ja määritä **10.10.2.4** **ensisijainen DNS-palvelin**.

1. Osoite **10.10.2.4** on määritetty AM-10.10.2.0/24 aliverkon Azure virtual verkko-osoite ja kyseisen AM on **ContosoDC**. Voit tarkistaa **ContosoDC**IP-osoite, käyttää **nslookup contosodc** komentokehote, alla kuvatulla tavalla.

    ![Nslookup-työkalulla voit etsiä toimialueen Ohjauskoneen IP-osoite](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC664954.jpg)

1. Valitse O**K** ja valitse **Sulje** Vahvista muutokset. Voit nyt voi liittyä AM **corp.contoso.com**.

1. **Paikallinen palvelin** -ikkunasta napsauttamalla **Työryhmä** -linkkiä.

1. Valitse **Muuta** **Tietokonenimi** -kohtaan.

1. Valitse **toimialue** -valintaruutu ja kirjoita **corp.contoso.com** tekstiruutuun. Valitse **OK**.

1. **Windowsin suojaus** ponnahdusikkuna-valintaikkunassa Määritä tunnistetiedot oletustoimialueen järjestelmänvalvojatiliä (**CORP\AzureAdmin**) ja salasana (**Contoso! 000**).

1. Kun näyttöön tulee "Tervetuloa corp.contoso.com toimialueen"-sanoma, valitse **OK**.

1. Valitse **Sulje**ja valitse sitten **Käynnistä uudelleen** ponnahdusikkuna-valintaikkunassa.

### <a name="add-the-corpinstall-user-as-an-administrator-on-each-vm"></a>Lisää Corp\Install käyttäjän järjestelmänvalvojana kunkin AM:

1. Odota, kunnes AM käynnistetään ja Käynnistä uudelleen ja kirjaudu sisään käyttämällä tiliä **BUILTIN\AzureAdmin** AM RDP-tiedosto.

1. **Server** hallinnassa valitsemalla **Työkalut**ja valitse sitten **Tietokoneenhallinta**.

    ![Tietokoneenhallinta](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784630.png)

1. Valitse **Tietokoneenhallinta** -ikkunassa Laajenna **Paikalliset käyttäjät ja ryhmät**ja valitse sitten **ryhmät**.

1. Kaksoisnapsauta **Järjestelmänvalvojat** -ryhmän.

1. **Järjestelmänvalvojat ominaisuudet** -valintaikkunassa valitse **Lisää** -painiketta.

1. Kirjoita käyttäjän **CORP\Install**ja valitse sitten **OK**. Kun pyydetään tunnuksia, käytä **AzureAdmin** -tili, jolla **Contoso! 000** salasana.

1. Valitse **OK** ja sulje **Järjestelmänvalvojan ominaisuudet** -valintaikkuna.

### <a name="add-the-failover-clustering-feature-to-each-vm"></a>Lisää **Vikasietoklusterointia** kunkin AM.

1. **Palvelimen hallinnan** raporttinäkymät-ikkunassa Valitse **Lisää roolit ja toiminnot**.

1. **Lisää rooleista ja ominaisuudet ohjatun toiminnon**, valitse **Seuraava** Siirry **Ominaisuudet** -sivulla.

1. Valitse **vikasietoklustereihin**. Kun sinulta kysytään, Lisää riippuvaiset ominaisuudet.

    ![Lisää vikasietoklusteri ominaisuus, AM](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784631.png)

1. Valitse **Seuraava**ja valitse **Vahvista** -sivulla **Asenna** .

1. Kun **Vikasietoklusterointia** ominaisuus-asennus on valmis, valitse **Sulje**.

1. Kirjaudu ulos AM.

1. Toista kaikkien kolmen palvelinten-- **ContosoWSFCNode**, **ContosoSQL1**ja **ContosoSQL2**tämän osan vaiheet.

SQL Server-VMs nyt valmisteltu ja käytössä, mutta ne asennetaan SQL Serverin kanssa oletusasetukset.

## <a name="create-the-wsfc-cluster"></a>Luo WSFC klusteri

Tässä osassa voit luoda WSFC-klusteriin, joka isännöi myöhemmin luot käytettävyys-ryhmä. Nyt mukaan olisi tehnyt kunkin kolme VMs, jota käytät WSFC klusterin seuraavasti:

- Täysin valmisteltu Azure-tietokannassa

- Liitettyjen AM toimialueeseen

- Lisätty **CORP\Install** paikalliseen Järjestelmänvalvojat-ryhmään

- Lisätty Vikasietoklusterointia

Kaikki näihin sisältyvät edellytykset kunkin AM käyttöön, ennen kuin voit liittyä siihen WSFC-klusterin.

Huomaa, että Azure virtual verkon ei toimivat samalla tavalla kuin paikalliseen verkkoon. Sinun täytyy luoda klusterin seuraavassa järjestyksessä:

1. Luo yhden solmun-klusterin johonkin solmujen (**ContosoSQL1**).

1. Muokkaa käyttämättömät IP-osoite (**10.10.2.101**) klusterin IP-osoite.

1. Tuo klusterin-nimeä.

1. Lisää muita solmujen (**ContosoSQL2** ja **ContosoWSFCNode**).

Noudata seuraavia ohjeita voit tehdä nämä toimet, joka määrittää täysin klusterin.

1. Käynnistä RDP-tiedoston, **ContosoSQL1** ja kirjaudu sisään käyttämällä toimialuetiliä **CORP\Install**.

1. **Palvelimen hallinnan** raporttinäkymät-ikkunassa Valitse **Työkalut**ja sitten **Automaattisesti klusterin hallinta**.

1. Vasemmassa ruudussa hiiren kakkospainikkeella **Automaattisesti klusterin hallinta**ja valitse sitten **Luo klusterin**, alla kuvatulla tavalla.

    ![Klusterin luominen](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784632.png)

1. Luoda Ohjattu klusterin luominen yhden solmun-klusterin käymättä läpi sivut ja alla olevia asetuksia:

  	|Sivun|Asetukset|
|---|---|
|Ennen aloittamista|Käytä oletusasetuksia|
|Valitse palvelinten|Kirjoita **ContosoSQL1** **Kirjoita palvelimen nimi** ja valitse **Lisää**|
|Kelpoisuuden varoitus|Valitse **Nro ei tarvitse tukea Microsoftilta tämän klusterin ja sen vuoksi et halua suorittaa vahvistus testit. Kun olen valinnut Seuraava, jatka klusterin luominen**.|
|Tukiasemaa klusterin hallintaa varten|Kirjoita **Cluster1** - **klusterinimi**|
|Vahvistus|Käytä oletusasetuksia, jollet käytä tallennustilan välilyöntejä. Katso seuraavan taulukon huomautus.|

    >[AZURE.WARNING] Jos käytössäsi on [Tallennustilan välilyöntejä](https://technet.microsoft.com/library/hh831739), joka yhdistää useita levyjä tallennustilan jakavat kyselyjä, poista **vahvistus** -sivulla **Lisää kaikki olevalla tallennustilaa klusteriin** -valintaruudun valinta. Ei poista tämä asetus, jos virtual levyjen irrotettu klusteroinnin aikana. Tämän vuoksi ne myös ei tule näkyviin levyn hallinnan tai Resurssienhallinnassa, kunnes tallennustilan välilyönnit poistetaan klusterin ja liittäminen uudelleen PowerShellin avulla.

1. Vasemmassa ruudussa Laajenna **Automaattisesti klusterin hallinta**ja valitse sitten **Cluster1.corp.contoso.com**.

1. Siirry keskiruudun- **Klusterin Core resurssit** -osioon ja laajenna **nimi: Clutser1** tiedot. Raportissa pitäisi näkyä **nimi** ja **IP-osoite** resurssit **epäonnistui** -tilassa. IP-osoite-resurssi et voi online-tilaan koska klusterin on määritetty sama IP-osoite avattua machine itse, joka on kaksoiskappaleiden osoite.

1. Epäonnistuneiden **IP-osoite** resurssin hiiren kakkospainikkeella ja valitse sitten **Ominaisuudet**.

    ![Klusterin ominaisuudet](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784633.png)

1. Valitse **Staattinen IP-osoite** ja määritä **10.10.2.101** osoite-tekstiruutuun. Valitse **OK**.

1. **Klusterin Core resurssit** -kohdassa hiiren kakkospainikkeella **nimi: Cluster1** ja valitse **Ota käyttöön**. Valitse Odota, kunnes molemmat resurssit ovat online-tilassa. Klusterin nimi resurssille kirjautuu, toiminto päivittää Ohjauskoneen palvelimen AD uusi tili. AD-tilin käytetään suorittaa ryhmän liitetty käytettävyyspalvelu myöhemmin.

1. Lopuksi Lisää jäljellä olevat solmut klusterin. Selaimen puusta **Cluster1.corp.contoso.com** hiiren kakkospainikkeella ja valitse **Lisää solmu**alla kuvatulla tavalla.

    ![Lisää solmu klusterin](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784634.png)

1. Valitse **Solmu Ohjattu lisääminen**valitsemalla **Seuraava**. **Valitse palvelimia** -sivulla Lisää **ContosoSQL2** ja **ContosoWSFCNode** luetteloon kirjoittamalla palvelimen nimi **Kirjoita palvelimen nimi** ja valitsemalla sitten **Lisää**. Kun olet valmis, valitse **Seuraava**.

1. Valitse **Varoitus vahvistus** -sivulla **ei** (tuotannon tilanne Tee kelpoisuuden testejä). Valitse sitten **Seuraava**.

1. Valitse **Vahvista** -sivulla **Seuraava** Lisää solmut.

    >[AZURE.WARNING] Jos käytössäsi on [Tallennustilan välilyöntejä](https://technet.microsoft.com/library/hh831739), joka ryhmittelee useita levyjä tallennustilan jakavat kyselyjä, poista **kaikki klusterin olevalla tallennustilan lisääminen** -valintaruudun valinta. Ei poista tämä asetus, jos virtual levyjen irrotettu klusteroinnin aikana. Tämän vuoksi ne myös ei tule näkyviin levyn hallinnan tai Resurssienhallinnassa, kunnes tallennustilan välilyönnit poistetaan klusterin ja liittäminen uudelleen PowerShellin avulla.

1. Kun klusterin solmut on lisätty, valitse **Valmis**. Automaattisesti klusterin hallinnan pitäisi nyt näkyä yhteyttä klusterin on kolme solmujen ja lukumuotoilut **solmujen** säilössä.

1. Kirjaudu ulos työpöydän etäistunnon.

## <a name="prepare-the-sql-server-instances-for-availability-group"></a>SQL Server-esiintymät valmisteleminen käytettävyys-ryhmä

Tässä osassa teet **ContosoSQL1** ja **contosoSQL2**seuraavasti:

- Lisää kirjautumisen **NT AUTHORITY\System** tarvittavat käyttöoikeudet, joka on määritetty oletusarvoinen SQL Server-esiintymän kanssa

- Lisää **CORP\Install** järjestelmänvalvojaryhmään oletusarvon SQL Server-esiintymän

- Avaa palomuuri SQL Serverin etäkäyttöä varten

- Ota aina käyttöön käytettävyys ryhmät-ominaisuus

- Muuta SQL Server-tilin **CORP\SQLSvc1** ja **CORP\SQLSvc2**tarpeen mukaan

Näitä toimintoja voi käyttää missä tahansa järjestyksessä. Kuitenkin seuraavia ohjeita käy läpi järjestyksessä. **ContosoSQL1** ja **ContosoSQL2**noudattamalla ohjeita:

1. Jos olet ole kirjautunut ulos työpöydän etäistunnon varten AM, tee se nyt.

1. Käynnistä **ContosoSQL1** ja **ContosoSQL2** RDP-tiedostot ja kirjaudu sisään **BUILTIN\AzureAdmin**.

1. Lisää **NT AUTHORITY\System** SQL Server-kirjautumiset ja tarvittavat käyttöoikeudet. Käynnistä **SQL Server Management Studiossa**.

1. Valitse **Yhdistä** muodostaa yhteyden SQL Server oletusesiintymää.

1. **Objektin Explorer**Laajenna **Suojaus**ja laajenna sitten **Kirjautumiset**.

1. **NT AUTHORITY\System** kirjautuminen hiiren kakkospainikkeella ja valitse **Ominaisuudet**.

1. Paikallisen palvelimen **Securables** -sivulla Valitse **Myönnä** seuraavat oikeudet ja valitse **OK**.

    - Muuta käytettävyys ryhmälle

    - Yhteyden SQL

    - Näytä palvelimen tila

1. Lisää seuraavaksi **järjestelmänvalvojaryhmään** **CORP\Install** oletusarvon SQL Server-esiintymän. **Objektin Explorer** **Kirjautumiset** hiiren kakkospainikkeella ja valitse **New Login**.

1. Kirjoita **CORP\Install** **kirjautumisnimi**.

1. Valitse **Roolit** -sivulla **sysadmin**. Valitse **OK**. Kirjautuminen luodaan, kun näet sen laajentamalla **Kirjautumiset** **Objektin**Resurssienhallinnassa.

1. Seuraavaksi palomuurisäännön luominen SQL Serveriä varten. **Käynnistä** -näytöstä Käynnistä **Windowsin laajennettu palomuuri**.

1. Valitse vasemmanpuoleisessa ruudussa **Saapuvan liikenteen säännöt**. Valitse oikeanpuoleisessa ruudussa **Uusi sääntö**.

1. Valitse **Säännön tyyppi** -sivulla Valitse **ohjelma**ja valitse sitten **Seuraava**.

1. Valitse **ohjelma** -sivulla **ohjelman polku** ja kirjoita %ProgramFiles%\Microsoft **SQL Server\MSSQL12. MSSQLSERVER\MSSQL\Binn\sqlservr.exe** tekstiruutuun (Jos seuraat näitä ohjeita, mutta käytä SQL Server 2012, SQL Server-kansio on **MSSQL11. MSSQLSERVER**). Valitse sitten **Seuraava**.

1. Valitse **toiminto** -sivulla säilyttää **Salli yhteys** valittuna ja valitse **Seuraava**.

1. Valitse **profiili** -sivulla Hyväksy oletusasetukset ja valitse **Seuraava**.

1. **Nimi** -sivulla säännön nimi, esimerkiksi **nimi** -tekstiruutuun **SQL Server (ohjelman sääntöä)** valitse sitten **Valmis**.

1. Seuraavaksi voit ottaa käyttöön **Aina käyttöön käytettävyys ryhmät** -ominaisuus. Käynnistä **SQL Serverin määritysten hallinta** **aloitusnäytössä** .

1. Selaimen-kohtaa **SQL Server Services**, ja valitse **SQL Server (MSSQLSERVER)** -palvelun hiiren kakkospainikkeella ja valitse **Ominaisuudet**.

1. Valitse **Aina käyttöön suuri käytettävyys** -välilehti ja valitse **Ota aina käyttöön käytettävyys ryhmät**, valitse alla kuvatulla tavalla ja valitse sitten **Käytä**. Valitse avautuvassa valintaikkunassa **OK** ja sulje ominaisuudet-ikkunassa vielä. SQL Server-palvelu käynnistyy uudelleen, kun muutat palvelutilin.

    ![Ota aina käyttöön käytettävyys ryhmät](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665520.gif)

1. Seuraavaksi voit muuttaa SQL Server-tilin. Valitse **Kirjaudu sisään** -välilehti ja valitse **CORP\SQLSvc1** (for **ContosoSQL1**) tai (for **ContosoSQL2**) **CORP\SQLSvc2** Kirjoita **Tilin nimi**ja valitse täytä ja vahvista salasana ja valitse sitten **OK**.

1. Ponnahdusikkuna Valitse **Kyllä** käynnistämään SQL Server-palvelusta. Kun SQL Server-palvelu on käynnistettävä uudelleen, ominaisuudet-ikkunassa tekemäsi muutokset ovat voimassa.

1. Kirjaudu ulos VMs.

## <a name="create-the-availability-group"></a>Käytettävyys-ryhmän luominen

Olet nyt valmiina määrittäminen käytettävyys-ryhmä. Alla on jäsennyksen, mitä hyötyä:

- Luo uusi tietokanta (**MyDB1**) **ContosoSQL1**

- Siirtää varmuuskopiot ja tapahtuman log varmuuskopion tietokannasta

- Palauttaa täydellinen ja kirjaudu varmuuskopioiden **ContosoSQL2** **NORECOVERY** -vaihtoehto

- Luo käytettävyys-ryhmä (**AG1**) synkronisen Vahvista, automaattinen automaattisesti ja lukea toissijainen replikoita

### <a name="create-the-mydb1-database-on-contososql1"></a>Luo MyDB1 tietokannan ContosoSQL1:

1. Jos et ole ehkä jo kirjautunut ulos remote työpöydän istunnot **ContosoSQL1** ja **ContosoSQL2**, tee se nyt.

1. Käynnistä RDP-tiedoston, **ContosoSQL1** ja kirjaudu sisään **CORP\Install**.

1. **Resurssienhallinta**Valitse * *C:\**, Luo kansio nimeltä * *varmuuskopion**. Voit käyttää kansion käytön varmuuskopioida ja palauttaa tietokannan.

1. Napsauta uutta kansiota hiiren kakkospainikkeella, **ja**Jaa ja valitse sitten **tietyt henkilöt**, alla kuvatulla tavalla.

    ![Varmuuskopiokansion luominen](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665521.gif)

1. Lisää **CORP\SQLSvc1** ja antaa sille **** luku/kirjoitusoikeudet, ja valitse Lisää **CORP\SQLSvc2** ja antaa sille **lukuoikeudet,** alla kuvatulla tavalla ja valitse sitten **Jaa**. Kun tiedostojen jakamisen prosessi on valmis, valitse **Valmis**.

    ![Varmuuskopiokansion käyttöoikeuksien myöntäminen](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665522.gif)

1. Seuraavaksi voit luoda tietokantaan. **Käynnistä** -valikosta Käynnistä **SQL Server Management Studiossa**ja valitse sitten **Yhdistä** muodostaa yhteyden SQL Server oletusesiintymää.

1. **Object Explorer** **tietokantojen** hiiren kakkospainikkeella ja valitse **Uusi tietokanta**.

1. **Tietokannan nimeä**Kirjoita **MyDB1**ja valitse sitten **OK**.

### <a name="take-a-full-backup-of-mydb1-and-restore-it-on-contososql2"></a>Tutustu täydellinen varmuuskopio MyDB1 ja palauttaa sen ContosoSQL2:

1. Seuraavaksi voit ottaa täydellisen varmuuskopion tietokannasta. **Objektin Explorer**Laajenna **tietokantoja**, sitten **MyDB1**, napsauta hiiren kakkospainikkeella ja valitse **tehtävät**ja valitse sitten **Varmuuskopioi**.

1. **Lähde** -kohdassa Säilytä **Varmuuskopiotyyppi** **koko**asettaminen. Valitse **kohde** -osan **poistaminen** , jos haluat poistaa varmuuskopiotiedoston tiedostopolku.

1. Valitse **Lisää** **kohde** -osan.

1. Kirjoita **Tiedostonimi** -tekstiruutuun ** \\ContosoSQL1\backup\MyDB1.bak**. Valitse **OK**ja valitse sitten **OK** uudelleen, jotta voit varmuuskopioida tietokanta. Kun varmuuskopiointi on valmis, valitse **OK** uudelleen, voit sulkea valintaikkunan.

1. Seuraavaksi voit ottaa tapahtumalokia varmuuskopion tietokannasta. **Objektin Explorer**Laajenna **tietokantoja**, sitten **MyDB1**, napsauta hiiren kakkospainikkeella ja valitse **tehtävät**ja valitse sitten **Varmuuskopioi**.

1. Valitse **Varmuuskopiointi** tyyppi-kohdassa **Tapahtumaloki**. Pidä **kohde** tiedostopolku arvoksi yksi entisellään ja valitse **OK**. Kun varmuuskopiointi on valmis, valitse **OK** uudelleen.

1. Seuraavaksi voit palauttaa **ContosoSQL2**täydellinen ja tapahtuman Lokin varmuuskopioiden. Käynnistä RDP-tiedoston, **ContosoSQL2** ja kirjaudu sisään **CORP\Install**. Jätä **ContosoSQL1** työpöydän etäistunnon Avaa.

1. **Käynnistä** -valikosta Käynnistä **SQL Server Management Studiossa**ja valitse sitten **Yhdistä** muodostaa yhteyden SQL Server oletusesiintymää.

1. **Objektin Explorer** **tietokantojen** hiiren kakkospainikkeella ja valitse **Palauta tietokanta**.

1. **Lähde** -kohdassa Valitse **laite**ja valitse **** ... painike.

1. **Valitse Varmuuskopioi laitteet**Valitse **Lisää**.

1. Kirjoita varmuuskopion tiedostosijainti \\ContosoSQL1\backup, Päivitä, sitten MyDB1.bak, valitse ja valitse sitten OK ja valitse sitten OK. Pitäisi tulla näkyviin täydellisen varmuuskopion ja varmuuskopioinnin log varmuuskopion asettaa palauttaa ruutu.

1. Siirry asetukset-sivulla ja valitse Valitse RESTORE WITH NORECOVERY palautus-tilaan ja valitse sitten OK Palauta tietokanta. Kun palautus on valmis, valitse OK.

### <a name="create-the-availability-group"></a>Käytettävyys-ryhmän luominen:

1. Siirry takaisin työpöydän etäistunnon **ContosoSQL1**varten. **Objektin Explorer** -SSMS **Aina käyttöön suuri käytettävyys** hiiren kakkospainikkeella ja valitse **Käytettävyys-ryhmän ohjattu**alla kuvatulla tavalla.

    ![Käynnistä ohjattu käytettävyys-ryhmä](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665523.gif)

1. **Johdanto** -sivulle valitsemalla **Seuraava**. **Määritä käytettävyys ryhmänimi** -sivun Kirjoita **AG1** **käytettävyys ryhmänimi**ja valitse sitten **Seuraava** uudelleen.

    ![AG luontitoiminnossa Määritä AG nimi](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665524.gif)

1. **Valitse tietokantojen** -sivulla Valitse **MyDB1** ja valitse **Seuraava**. Tietokanta täyttää käytettävyys-ryhmän edellytyksistä, koska suorittamasi vähintään yksi varmuuskopiointi tarkoitettu ensisijainen se.

    ![Valitse AG luontitoiminnossa tietokannat](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665525.gif)

1. Valitse **Määritä replikoiden** -sivulla **Lisää replikan**.

    ![AG luontitoiminnossa Määritä replikat](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665526.gif)

1. **Muodosta yhteys palvelimeen** -valintaikkuna tulee. Kirjoita **ContosoSQL2** **palvelimen nimi**ja valitse sitten **Yhdistä**.

    ![AG luontitoiminnossa muodostaa yhteyttä palvelimeen](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665527.gif)

1. **Määritä replikoiden** -sivulla takaisin sisään pitäisi tulla näkyviin **ContosoSQL2** **Käytettävissä**replikassa luettelossa. Määritä replikat alla kuvatulla tavalla. Kun olet valmis, valitse **Seuraava**.

    ![AG luontitoiminnossa Määritä replikoiden (valmis)](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665528.gif)

1. **Valitse ensimmäinen tietojen synkronointi** -sivulle valitsemalla **liity ainoastaan** ja valitse **Seuraava**. Olet jo suorittanut tietojen synkronointi manuaalisesti kun noudatit täyden ja tapahtuman varmuuskopioinnin käyttöön **ContosoSQL1** ja palauttaa ne **ContosoSQL2**. Voit valita sen sijaan Varmuuskopiointi ja palauttaa tietokannan toiminnot ja valitse **koko** kertoaksesi käytettävyys ryhmän ohjattu suorittaa tietojen synkronointi. Tämä kuitenkin ei suositella hyvin suuria tietokantoja, jotka löytyvät Jotkut yritykset.

    ![Valitse AG luontitoiminnossa aloitustiedot synkronointi](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665529.gif)

1. Valitse **vahvistus** -sivulla **Seuraava**. Tämä sivu pitäisi nyt muistuttaa alapuolella. Ei varoituksen listener määrityksen, koska et ole määrittänyt käytettävyys-ryhmän listener. Voit ohittaa tämän varoituksen, koska tämä opetusohjelma ei määritetä kuuntelija. Määrittää kuuntelua, kun olet suorittanut tämän opetusohjelman, artikkelissa [määrittäminen ILB-listener aina käyttöön käytettävyys ryhmien Azure-tietokannassa](virtual-machines-windows-classic-ps-sql-int-listener.md).

    ![AG luontitoiminnossa vahvistus](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665530.gif)

1. **Yhteenveto** -sivulla **Valmis**, ja odota, kun ohjattu toiminto määrittää käytettävyys uuteen ryhmään. Valitse **edistyminen** -sivulla voit tarkastella yksityiskohtaisia edistymistä **enemmän tietoja** . Kun ohjattu toiminto on valmis, tarkista **tulokset** -sivulla voit tarkistaa, että käytettävyys-ryhmä on luotu, alla kuvatulla tavalla ja valitse sitten **Sulje** Sulje ohjattu toiminto.

    ![AG luontitoiminnossa tulokset](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665531.gif)

1. **Objektin Explorer**Laajenna **Aina käyttöön suuri käytettävyys**sitten Laajenna **Käytettävyys ryhmät**. Tämä säilö käytettävyys uuden ryhmän pitäisi tulla näkyviin. Napsauta hiiren kakkospainikkeella **AG1 (perus)** ja sitten **Näytä Raporttinäkymät-ikkunan**.

    ![Näytä AG Raporttinäkymät-ikkunan](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665532.gif)

1. **Aina-koontinäytön** pitäisi muistuttaa alla kuvatun. Voit tarkastella replikat, automaattisesti tila replikoissa ja synkronoinnin tila.

    ![AG Raporttinäkymät-ikkunan](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665533.gif)

1. Palauttaa **Palvelimen hallinta**, valitse **Työkalut**ja käynnistä sitten **Automaattisesti klusterin hallinta**.

1. Laajenna **Cluster1.corp.contoso.com**ja laajenna sitten **palvelut ja sovellukset**. Valitse **roolit** ja Huomaa, että **AG1** käytettävyys-ryhmä-roolin on luotu. Huomaa, että AG1 ei ole mitään IP-osoite mukaan tietokannan asiakkaat voivat muodostaa yhteyden käytettävyys-ryhmästä koska olet määrittänyt kuuntelija. Voit muodostaa yhteyden suoraan ensisijainen solmu luku-ja kirjoitusoikeudet toimintoja varten ja vain luku-muotoisten kyselyjen toissijainen solmu.

    ![AG automaattisesti klusterin hallinta](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665534.gif)

>[AZURE.WARNING] Älä yritä käytettävyys-ryhmästä automaattisesti klusterin Managerista epäonnistuu. Kaikki automaattisesti toiminnot tulee tehdä **Aina-koontinäytön** SSMS kuluessa. Lisätietoja on artikkelissa [rajoituksia käyttöön WSFC automaattisesti klusterin hallinnan avulla käytettävyys ryhmiin](https://msdn.microsoft.com/library/ff929171.aspx).

## <a name="next-steps"></a>Seuraavat vaiheet
Olet nyt on ottanut käyttöön SQL Server aina luomalla käytettävyys-ryhmän Azure-tietokannassa. Määritä käytettävyys ryhmän kuuntelija artikkelissa [määrittäminen ILB-listener aina käyttöön käytettävyys ryhmien Azure-tietokannassa](virtual-machines-windows-classic-ps-sql-int-listener.md).

Tietoja muista käyttämisestä SQL Server Azure-kohdassa [SQL Server-Azuren näennäiskoneiden](virtual-machines-windows-sql-server-iaas-overview.md).

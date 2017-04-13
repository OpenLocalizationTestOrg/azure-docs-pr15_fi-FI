<properties
    pageTitle="Aina käytettävyys ryhmän määrittäminen Azure AM automaattisesti - resurssien hallinta"
    description="Luo aina käytettävyys-ryhmä ja Azure-virtuaalikoneissa Azure Resurssienhallinta-tilassa. Tässä opetusohjelmassa käytetään ensisijaisesti käyttöliittymän luodaan automaattisesti koko ratkaisu."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-resource-manager" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/20/2016"
    ms.author="MikeRayMSFT" />

# <a name="configure-always-on-availability-group-in-azure-vm-automatically---resource-manager"></a>Aina käytettävyys ryhmän määrittäminen Azure AM automaattisesti - resurssien hallinta

> [AZURE.SELECTOR]
- [Resurssienhallinta: malli](virtual-machines-windows-portal-sql-alwayson-availability-groups.md)
- [Resurssienhallinta: Manuaalinen](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)
- [Perinteinen: Käyttöliittymä](virtual-machines-windows-classic-portal-sql-alwayson-availability-groups.md)
- [Perinteinen: PowerShell](virtual-machines-windows-classic-ps-sql-alwayson-availability-groups.md)

<br/>

Lopusta loppuun tässä opetusohjelmassa näytetään, miten voit luoda ja Azure Resurssienhallinta näennäiskoneiden SQL Server käytettävyys ryhmän. Opetusohjelman käyttää Azure lavat mallin määrittämiseen. Oletusasetusten tarkistamiseen ja kirjoita tarvittavat asetukset ja päivittää näiden portaalissa kuin tässä opetusohjelmassa kertovat.

Opetusohjelman lopussa Azure SQL Serverin käytettävyys ryhmän ratkaisu sisältää seuraavat osat:

- Virtuaalinen verkkoon, joka sisältää useita aliverkosta, mukaan lukien edusta- ja taustatietokantaan aliverkon

- Kahden toimialueen ohjauskoneen Active Directory (AD)-toimialueella

- Kaksi SQL Server VMs käyttöön taustatietokantaan aliverkon ja AD-toimialueen

- 3-solmu WSFC klusterin solmu suurimmalla quorum-malli

- Käytettävyys ryhmä, josta on synkronoitu Vahvista replikat käytettävyys tietokanta

Seuraavassa kuvassa on graafinen esitys-ratkaisun.

![Testaa kurssin arkkitehtuuri AG Azure-tietokannassa](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/0-EndstateSample.png)

Kaikki tämän ratkaisun resurssit kuuluvat yksittäinen resurssi-ryhmään.

Tässä opetusohjelmassa oletetaan seuraavasti:

- Sinulla on jo Azure-tili. Jos sinulla ei ole yhtä [kokeilutili rekisteröityminen](http://azure.microsoft.com/pricing/free-trial/).

- Tiedät voit valmistella SQL Server-AM avulla Graafisen virtuaalikoneen valikoimasta. Lisätietoja on artikkelissa [SQL Serverin virtual tietokoneen Azure valmistelu](virtual-machines-windows-portal-sql-server-provision.md)

- Sinulla on jo käytettävyys ryhmien tasainen tietoja. Lisätietoja on kohdassa [aina käytettävyys-groups (SQL Server)](http://msdn.microsoft.com/library/hh510230.aspx).

>[AZURE.NOTE] Jos olet kiinnostunut käytettävyys ryhmiä käytetään yhdessä SharePointin kanssa, katso myös [määrittäminen SQL Server 2012 aina käyttöön käytettävyys groups SharePoint 2013: n](http://technet.microsoft.com/library/jj715261.aspx).

Tässä opetusohjelmassa käytetään Azure-portaalissa:

- Valitse portaalista aina-malli

- Tarkista mallin ja muutama-ympäristön määritys-asetusten päivittäminen

- Valvoa Azure, kun se luo koko-ympäristössä

- Yhdistä johonkin toimialueen ohjaimet ja SQL-palvelimia

[AZURE.INCLUDE [availability-group-template](../../includes/virtual-machines-windows-portal-sql-alwayson-ag-template.md)]


## <a name="provision-the-cluster-from-the-gallery"></a>Valmistele klusterin valikoimasta

Azure sisältää valikoiman kuva koko ratkaisu. Jotta voit Etsi malli:

1.  Kirjaudu sisään käyttämällä tiliä Azure-portaaliin.
1.  Azure portaalin napsautettaessa **+ Uusi.** Portaalin avautuu uusi sivu.
1.  Valitse **AlwaysOn**uusi sivu Etsi.
![AlwaysOn hakeminen](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/16-findalwayson.png)
1.  Etsi **SQL Server-AlwaysOn klusterin**hakutulokset.
![AlwaysOn malli](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/17-alwaysontemplate.png)
1.  Valitse **käyttöönoton mallin valitseminen** **Resurssienhallinta**.

### <a name="basics"></a>Perusteet

Valitse **Perustiedot** ja määritä seuraavat asetukset:

- **Järjestelmänvalvojan käyttäjänimi** on käyttäjätilin toimialueen järjestelmänvalvojan oikeudet ja SQL Server järjestelmänvalvojaryhmään kiinteä palvelimen jäsen molemmat SQL Server-esiintymät. Käytä tässä opetusohjelmassa **DomainAdmin**.

- **Salasana** on toimialueen järjestelmänvalvojan tilin salasana. Käytä KOMPLEKSI salasanaa. Vahvista salasana.

- **Tilaus** on tilaus, Azure laskuttaa toimimaan kaikkien resurssien käytettävyyden ryhmän käyttöön. Voit määrittää eri tilauksen, jos tilisi on useita tilauksia.

- **Resurssiryhmä** on se, että kaikki Azure resurssit luonut Tässä opetusohjelmassa ryhmään kuuluvat. Käytä **SQL-HA-RG**Tässä opetusohjelmassa. Lisätietoja on artikkelissa (Azure Resurssienhallinta yleiskatsaus) [resurssi-ryhmä-overview.md / # resurssi-ryhmien].

- **Sijainti** on Azure alue, jossa Tässä opetusohjelmassa resurssien luodaan. Valitse Azure alue, infrastruktuuri isännöimiseen.

Alla on miltä **Perustiedot** -sivu näyttää:

![Perusteet](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/1-basics.png)

- Valitse **OK**.

### <a name="domain-and-network-settings"></a>Toimialueen ja verkon asetukset

Azure valikoima mallin Luo uusi toimialue, jossa uusi toimialue-ohjaimet. Se myös Luo uuden verkon ja kahden aliverkon. Mallia ei tule käyttöön palvelimet luominen aiemmin luotuun toimialueen tai VPN. Seuraava vaihe on toimialue ja verkko-asetusten määrittämiseen.

Tarkista **toimialue- ja asetukset** -sivu toimialueen ja verkon asetusten ennalta määritetyt arvot:

- Toimialuenimi, jota käytetään AD-toimialueen, joka isännöi klusterin on **metsää ensisijaisen toimialuenimi** . Käytä opetusohjelman **contoso.com**.

- **VPN nimi** on verkkonimi Azure virtual verkossa. Käytä tässä opetusohjelmassa **autohaVNET**.

- **Toimialueen ohjauskoneen aliverkon nimi** on osa, joka isännöi toimialueen ohjauskoneen virtual verkon nimi. Käytä tässä opetusohjelmassa **aliverkon 1**. Tämän aliverkon käyttää osoitteen etuliite **10.0.0.0/24**.

- **SQL Server aliverkon nimi** on osa isännöi SQL-palvelimien ja tiedoston jakaminen hallitun virtual verkon nimi. Käytä tässä opetusohjelmassa **aliverkon 2**. Tämän aliverkon käyttää osoitteen etuliite **10.0.1.0/26**.

Lisätietoja virtual verkot [Azure artikkelissa Virtual verkon yleiskatsaus](../virtual-network/virtual-networks-overview.md).  

**Toimialueen ja verkon** pitäisi näyttää tältä:

![Toimialueen ja verkon asetukset](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/2-domain.png)

Tarvittaessa voit muuttaa näitä arvoja. Tässä opetusohjelmassa Käytämme ennalta määritetyt arvot.

- Tarkista ja valitse **OK**.

###<a name="availability-group-settings"></a>käytettävyys ryhmäasetukset

Tarkista **käytettävyys ryhmäasetukset** käytettävyys-ryhmä ja kuuntelua ennalta määritetyt arvot.

- **Laiduntamismahdollisuuksien ryhmänimi** on liitetty resurssin käytettävyys-ryhmän nimi. Käytä tässä opetusohjelmassa **Contoso-ag**.

- klusterin ja sisäinen kuormituksen käytetään **käytettävyys ryhmän listener nimeä** . Yhteyden muodostaminen SQL Server asiakkaat tarvittavat se tietokannan yhdistäminen nimi avulla. Käytä tässä opetusohjelmassa **Contoso listener**.

-  **käytettävyys ryhmän kuuntelevan portin** määrittää porttinumeroa SQL Server-listener käyttää. Käytä tässä opetusohjelmassa oletusportti **1433**.

Tarvittaessa voit muuttaa näitä arvoja. Tässä opetusohjelmassa, käytä valmiiksi määritettyjä arvoja.  

![käytettävyys ryhmäasetukset](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/3-availabilitygroup.png)

- Valitse **OK**.

###<a name="vm-size-storage-settings"></a>AM koon tallennustilan asetukset

**AM koon** ja tallennustilaa asetukset SQL Server virtuaalikoneen koon valitseminen ja tarkastele muita asetuksia.

- **SQL Server virtuaalikoneen koko** on sekä SQL Server Azure virtuaalikoneen kokoa. Valitse virtuaalikoneen koko on sopiva havainnollistamiseen. Jos olet muodostamassa tämän opetusohjelman ympäristön käyttää **DS2**. Tuotannon toiminnoista, jotka tukevat työmäärää virtuaalikoneen koon valitseminen. Monta tuotannon työmääriä edellyttävät **DS4** tai suurempi. Malli muodosta kaksi näennäiskoneiden, joiden koko ja asentaa SQL Serverin yksitellen. Lisätietoja on artikkelissa [näennäiskoneiden koot](virtual-machines-linux-sizes.md).

>[AZURE.NOTE]Azure asentaa SQL Server Enterprise-version. Kustannus määräytyy versio ja virtuaalikoneen kokoa. Saat lisätietoja nykyiset kustannukset [näennäiskoneiden hinnat](http://azure.microsoft.com/pricing/details/virtual-machines/#Sql).

- **Toimialueen ohjauskoneen virtuaalikoneen virtuaalikoneen sivukoko on toimialue-ohjaimet.** Käytä tässä opetusohjelmassa **D2**.

- **Tiedoston jakaminen hallitun virtuaalikoneen koko** on tiedoston Jaa hallitun virtuaalikoneen koon. Käytä tässä opetusohjelmassa **A1**.

- **SQL-tallennustilan tilin** on SQL Serverin tietoihin ja käyttöjärjestelmän levyjen tallennustilan tilin nimi. Käytä tässä opetusohjelmassa **alwaysonsql01**.

- **Toimialueen Ohjauskoneen tallennustilan tili** on toimialueen ohjaimet-tallennustilan tilin nimi. Käytä tässä opetusohjelmassa **alwaysondc01**.

- **SQL Serverin tietoihin levyn koon** TT on haluamasi kokoinen ja SQL Server data levyn Teratavua. Määritä luku väliltä 1 – 4. Tämä on koko, joka on liitettynä kunkin SQL Server data levyn. Käytä tässä opetusohjelmassa **1**.

- **Tallennustilan optimointi** määrittää kuormituksen lajin SQL Server-näennäiskoneiden tietyn tallennustilan asetukset. Tässä skenaariossa SQL-palvelimilta käyttäminen premium tallennustilan Azure levyn host välimuistiin vain luku. Lisäksi voit optimoida kuormituksen SQL Serverin asetukset valitsemalla jokin seuraavista kolmesta asetuksia:

    - **Yleiset työmäärää** määrittää ei tiettyä asetukset

    - **Tapahtumallinen käsittelyn** asettaa Jäljitä merkintä 1117 ja 1118

    - **Tietovarastointi** asettaa Jäljitä merkintä 1117 ja 610

Käytä tässä opetusohjelmassa **Yleiset työmäärää**.

![Tallennustilan AM kokoasetukset](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/4-vm.png)

- Tarkista ja valitse **OK**.

####<a name="a-note-about-storage"></a>Muistiinpanon tallennustilan tietoja

Lisää optimointi määräytyvät SQL Server data levyjen koosta. Tietoja levyn kunkin teratavun Azure Lisää 1 TT: N premium lisätallennustilaa (Suoritettaessa). Palvelin edellyttää 2 Teratavua tai enemmän, malli luo tallennustilan resurssivarantoon kunkin SQL Server. Tallennustilan varanto on lomakkeeseen tallennustilan virtualization missä useita levyjä on määritetty antamaan suurempi kapasiteetti, vikasietoisuudelle ja suorituskykyä.  Mallin sitten Luo tallennustilan tarkistaminen tallennustilan resurssivarantoon, ja esittää tämä käyttöjärjestelmän yksittäisen tietoina. Mallin määrittää levyä tietojen levyn SQL Serveriä varten. Mallin säätää tallennustilan resurssivarantoon SQL Serverin seuraavat asetukset:

- Raitakoko on virtual levyn interleave-asetusta. Tapahtumien työmääriä tämä on määritetty 64 kilotavua. Tietovaraston työmääriä asetus on 256 kt.

- Vikasietoisuudelle on helppoa (ei vikasietoisuudelle).

>[AZURE.NOTE] Azure premium tallennustila on paikallisesti tarpeettomat ja säilyttää tiedot kolme kopiota yhden alueen, joten muita vikasietoisuudelle tallennustilan ryhmän etsiminen ei tarvita.

- Sarakkeiden määrä on sama kuin levyjen sarjassa tallennustilan määrä.

Saat lisätietoja tallennustilan välilyönti ja tallennustilaa jakavat on:

- [Yleistä tallennuksesta välilyöntejä](http://technet.microsoft.com/library/hh831739.aspx).

- [Windows Server Backup ja tallennustilaa jakavat](http://technet.microsoft.com/library/dn390929.aspx)

Katso lisätietoja SQL Serverin määritysten parhaat käytännöt [suorituskyvyn Azuren näennäiskoneiden SQL Server parhaat käytännöt](virtual-machines-windows-sql-performance.md)


###<a name="sql-server-settings"></a>SQL Server-asetukset

Tarkista **SQL Serverin asetukset** ja muokkaa SQL Server AM nimen etuliitteen, SQL Server-versiota, SQL Server-tilin ja salasanan ja SQL automaattinen korjaaminen ylläpito aikataulun.

- **SQL Server nimi etuliite** käytetään luomaan kunkin SQL-palvelimen nimi. Käytä tässä opetusohjelmassa **Contoso-ag**. SQL Server-nimet on *Contoso-ag-0* ja *Contoso-ag-1*.

- **SQL Serverin versio** on SQL Server-version. Käytä tässä opetusohjelmassa **SQL Server 2014**. Voit valita myös **SQL Server 2012** ja **SQL Server 2016**.

- **SQL Server-palvelun tilin käyttäjänimi** on SQL Server-palvelun tili toimialuenimi. Käytä tässä opetusohjelmassa **sqlservice**.

- **Salasana** on SQL Server-tilin salasana.  Käytä KOMPLEKSI salasanaa. Vahvista salasana.

- **SQL automaattista korjausta ylläpito aikataulun** ilmaisee, että Azure automaattisesti korjaustiedoston SQL-palvelimien viikonpäivä. Kirjoita tässä opetusohjelmassa **sunnuntai**.

- **SQL-automaattinen korjaaminen ylläpito Käynnistä tunti** on kellonajan Azure alue, kun automaattista korjausta alkaa.

>[AZURE.NOTE]Aikavyöhykkeiden ikkuna, jossa kukin AM on Porrastettu tunnin. Vain yksi virtual tietokoneeseen on asennettu kerrallaan häiriöt palvelujen estämiseksi.

![SQL Server-asetukset](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/5-sql.png)

Tarkista ja valitse **OK**.

###<a name="summary"></a>Yhteenveto

Yhteenveto-sivulla Azure tarkistaa asetuksia. Voit ladata mallia. Tarkista yhteenveto. Valitse **OK**.

###<a name="buy"></a>Osta

Tämä lopullinen sivu sisältää **käyttöehdot**ja **tietosuojakäytäntö**. Tarkista tiedot. Kun olet valmis, voit käynnistää näennäiskoneiden ja kaikki muut tarvittavat resurssit käytettävyys-ryhmän luominen Azure, valitse **Luo**.

Azure portaalin Luo resurssiryhmän ja kaikki resurssit.

##<a name="monitor-deployment"></a>Näytön käyttöönotto

Azure-portaalista seurata käyttöönotto. Azure portaalin Raporttinäkymät-ikkunan automaattisesti kiinnitettyinä käyttöönoton esittävä kuvake.

![Azure Raporttinäkymät-ikkunan](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/11-deploydashboard.png)

##<a name="connect-to-sql-server"></a>Yhteyden muodostaminen SQL Server

SQL Server uusia esiintymiä on käynnissä, joka ei ole internet-yhteyksien näennäiskoneiden. Toimialueen ohjaimet on kuitenkin vastakkaisten yhteyden internet. Jotta voit muodostaa yhteyden SQL-palvelimiin etätyöpöydän ensimmäisen RDP johonkin toimialueen ohjaimet avulla. Avaa toimialueen ohjauskoneen toisen RDP SQL Server.

Voit RDP ensisijaisen ohjauskoneen, toimimalla seuraavasti:

1.  Koontinäytöstä Azure portaalin hyvin käyttöönotto onnistui.

1.  Valitse **resurssit**.

1.  Valitse **Perus-toimialueen ohjauskoneen ad** ensisijaisen ohjauskoneen virtuaalikoneen tietokonenimi eli **resurssit** -sivu.

1.  Valitse sivu, **Perus-toimialueen ohjauskoneen ad** **Connect**. Selain kysyy, haluatko avata tai tallentaa etäyhteyden objekti. Valitse **Avaa**.
![Toimialueen Ohjauskoneen yhdistäminen](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/13-ad-primary-dc-connect.png)
1.  **Etätyöpöytäyhteys** voi varoittaa, tämä etäyhteyden publisher ei tunnisteta. Valitse **Muodosta yhteys**.

1.  Windowsin suojaus kehottaa sinua kirjoittamalla tunnistetietosi yhdistäminen ensisijaisen ohjauskoneen IP-osoite. Valitse **Käytä toiseen tiliin**. Kirjoita **käyttäjänimi** **contoso\DomainAdmin**. Tämä on järjestelmänvalvojan käyttäjänimi valitsemasi tili. Monimutkaisen salasanan, jonka valitsit, kun olet määrittänyt mallin avulla

1.  **Etätyöpöytä** voi varoittaa, että etätietokone ei voitu todentaa sen suojausvarmenteessa ongelmien vuoksi. Se näyttää suojauksen varmenteen nimi. Jos olet toiminut opetusohjelman nimi on **ad-perus-dc.contoso.com**. Valitse **Kyllä**.

Olet nyt muodostanut yhteyden ensisijaisen ohjauskoneen. SQL Server RDP-, toimi seuraavasti:

1.  Toimialueen ohjauskoneen Avaa **Etätyöpöytäyhteys**.

1.  **Tietokone**Kirjoita nimi SQL-palvelimia. Kirjoita tässä opetusohjelmassa **SQL Server-0**.

1.  Käytä samaa käyttäjänimi ja salasana, jota käytit RDP toimialueen ohjauskoneen.

On nyt yhdistetty RDP SQL Serverin kanssa. Voit avata SQL Server management Studiossa, muodosta yhteys SQL Server oletusesiintymää ja tarkista availabilty ryhmä on määritetty.



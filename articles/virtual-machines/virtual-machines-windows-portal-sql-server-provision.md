<properties
    pageTitle="Valmistele SQL Server Virtual kone | Microsoft Azure"
    description="Luo ja muodosta yhteys SQL Server-virtual tietokoneeseen portaalissa Azure-tietokannassa. Tässä opetusohjelmassa käyttää Resurssienhallinta-tilaa."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    editor=""
    manager="jhubbard"
    tags="azure-resource-manager" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/21/2016"
    ms.author="jroth" />

# <a name="provision-a-sql-server-virtual-machine-in-the-azure-portal"></a>Valmistele SQL Server-virtual tietokoneeseen Azure-portaalissa

> [AZURE.SELECTOR]
- [Portal](virtual-machines-windows-portal-sql-server-provision.md)
- [PowerShellin](virtual-machines-windows-ps-sql-create.md)

Lopusta loppuun tässä opetusohjelmassa näytetään, miten käyttää Azure-portaalia valmistelu virtual koneen SQL Serveriä.

Azure virtuaalikoneen (AM)-valikoima sisältää useita kuvia, jotka sisältävät Microsoft SQL Server. Muutamalla napsautuksella voit valita jonkin SQL AM kuvat valikoimasta ja valmistella Azure-ympäristössä.

Tässä opetusohjelmassa menettelet seuraavasti:

- [Valitse SQL AM kuva valikoimasta](#select-a-sql-vm-image-from-the-gallery)
- [Määritä ja luo AM](#configure-the-vm)
- [Avaa AM etätyöpöydän kanssa](#open-the-vm-with-remote-desktop)
- [SQL Server etäkäyttö](#connect-to-sql-server-remotely)

## <a name="select-a-sql-vm-image-from-the-gallery"></a>Valitse SQL AM kuva valikoimasta

1. Kirjaudu sisään käyttämällä tiliä [Azure portal](https://portal.azure.com) .

    >[AZURE.NOTE] Jos sinulla ei ole Azure-tili, käy [vapaa kokeiluversion Azure](https://azure.microsoft.com/pricing/free-trial/).

1. Azure-portaalissa Valitse **Uusi**. Portaalin avautuu **Uusi** sivu. SQL Server AM resurssit ovat on Marketplace **näennäiskoneiden** -ryhmässä.

1. Valitse **Uusi** sivu- **näennäiskoneiden**.

1. Jos haluat nähdä kaikki käytettävissä olevat kuvat, valitse **näet kaikki** **näennäiskoneiden** -sivu.

    ![Azure-Virtuaalikoneissa sivu](./media/virtual-machines-windows-portal-sql-server-provision/azure-compute-blade.png)

1. Valitse **SQL Server**- **tietokanta-palvelimet**-kohdassa. Voit joutua Etsi **tietokanta-palvelimien**alaspäin. Tarkista SQL Serverin käytettävissä olevat mallit.

    ![Virtual Machine-valikoiman SQL-kuvia](./media/virtual-machines-windows-portal-sql-server-provision/virtual-machine-gallery-sql-server.png)

1. Jokainen malli näkyy SQL Server-versiota ja käyttöjärjestelmän. Valitse jokin kuvien luettelosta. Miten tiedot-sivu, jossa on virtuaalikoneen kuvan kuvaus.

    >[AZURE.NOTE] SQL-AM kuvia sisällyttää SQL Serverin käyttöoikeuksien myöntämistä kustannukset luot AM minuutissa hinnat. On jokin muu vaihtoehto tuo-your-omistaja-käyttöoikeus (BYOL) ja maksamisen vain AM varten. Kuva nimet ovat etuliite {BYOL}. Saat lisätietoja tämän asetuksen, [SQL Server-Azuren näennäiskoneiden käytön aloittaminen](virtual-machines-windows-sql-server-iaas-overview.md).

1. **Valitse käyttöönottomalli**-kohdassa Tarkista, että **Resurssienhallinta** on valittuna. Resurssienhallinta on uusi näennäiskoneiden suositellut käyttöönottomalli. Valitse **Luo**.

    ![Luo SQL AM resurssien hallinta](./media/virtual-machines-windows-portal-sql-server-provision/azure-compute-sql-deployment-model.png)

## <a name="configure-the-vm"></a>Määritä AM
On viisi lavat SQL Server-virtual machine määrittämistä varten.

| Vaihe               | Kuvaus                          |
|---------------------|-------------------------------|
| **Perusteet**              | [Tavallinen asetusten määrittäminen](#1-configure-basic-settings)      |
| **Kokoa**                | [Virtuaalikoneen koon valitseminen](#2-choose-virtual-machine-size)   |
| **Asetukset**            | [Valinnainen ominaisuuksien määrittäminen](#3-configure-optional-features)   |
| **SQL Server-asetukset** | [SQL server-asetusten määrittäminen](#4-configure-sql-server-settings) |
| **Yhteenveto**             | [Tarkista yhteenveto](#5-review-the-summary)            |

## <a name="1-configure-basic-settings"></a>1. basic asetusten määrittäminen
**Perustiedot** -sivu, anna seuraavat tiedot:

* Kirjoita yksilöllinen virtual tietokoneen **nimi**.
* Määrittää paikallisen järjestelmänvalvojatilin **käyttäjänimi** AM. Tili lisätään myös **SQL Server järjestelmänvalvojaryhmään kiinteä palvelimeen** .
* Anna vahva **salasana**.
* Jos sinulla on useita tilauksia, varmista, että tilaus on oikeat uusi AM.
* Kirjoita **resurssiryhmä** -ruutuun nimi uusi resurssiryhmä. Voit käyttää aiemmin resurssien ryhmä valitsemalla vaihtoehtoisesti **Valitse aiemmin luotu**. Resurssiryhmä on kokoelma liittyvät resurssit Azure-tietokannassa (näennäiskoneiden, tallennustilan tilit, virtual verkkojen jne.).

    >[AZURE.NOTE] Uusi resurssiryhmä käyttämällä on hyödyllinen, jos olet juuri testaaminen tai SQL Server Azure-käyttöönotoissa tietoa. Kun olet lopettanut testauksessa, poista resurssiryhmä Poista automaattisesti AM ja kaikki kyseisen resurssiryhmä liittyvät resurssit. Saat lisätietoja resurssiryhmät [Azure resurssin hallinnassa: yleiskatsaus](../azure-resource-manager/resource-group-overview.md).

* Valitse **sijainti** tämä käyttöönottoa varten.
* Valitse **OK** , jos haluat tallentaa asetukset.

    ![SQL: n perusteet-sivu](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-basic.png)

## <a name="2-choose-virtual-machine-size"></a>2. Valitse virtuaalikoneen koko
Valitse **kokoa** -vaiheessa **Valitse koko** sivu virtuaalikoneen koon valitseminen. Sivu näkyy aluksi suositellut koneen koot valitun mallin pohjalta. Arvioi populaation myös Suorita AM kuukausihinta.

![SQL-AM koon asetukset](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-vm-choose-a-size.png)

Tuotannon toiminnoista Suosittelemme valitsemalla virtuaalikoneen koko, joka tukee [Premium tallentamista](../storage/storage-premium-storage.md). Jos asetat ei ole kyseisiä suorituskyvyn, käytä **Näytä kaikki** -painiketta, joka näyttää kaikki tietokoneen koon asetukset. Voit esimerkiksi käyttää tietokoneen pienemmäksi kehittäminen tai testiympäristössä.

>[AZURE.NOTE] Lisätietoja virtuaalikoneen koot näkyvissä, [näennäiskoneiden koot](virtual-machines-windows-sizes.md). Katso huomioon otettavia seikkoja SQL Server AM koot, [suorituskyvyn SQL Server Azuren näennäiskoneiden parhaat käytännöt](virtual-machines-windows-sql-performance.md).

Valitse tietokoneen koko ja valitse sitten **Valitse**.

## <a name="3-configure-optional-features"></a>3. valinnainen ominaisuuksien määrittäminen
Määritä Azure tallennus-verkko- ja virtuaalikoneen seurannan **asetukset** -sivu.

- Määritä **tallennustilan** **levyn Kirjoita** vakio- tai Premium (Suoritettaessa). Premium tallennustilan suositellaan tuotannon toiminnoista.

>[AZURE.NOTE] Jos valitset Premium Suoritettaessa tietokoneen koko, joka ei tue Premium, machine-koko muuttuu automaattisesti.  

- **Tallennustilan tilin**voit hyväksyä automaattisesti valmistellun tallennustilan tilin nimi. Voit myös napsauttaa **tallennustilan tilin** määrittäminen tallennustilan tilin tyyppi ja valitse aiemmin luotu tili. Azure luo oletusarvoisesti uuden tallennustilan tilin paikallisesti ylimääräinen. Saat lisätietoja tallennusasetukset [Azuren tallennustilaan replikoinnin](../storage/storage-redundancy.md).

- Valitse **Verkko**voit hyväksyä automaattisesti täyttää arvot. Voit myös napsauttaa kunkin toiminnon määrittäminen manuaalisesti **Virtual verkosta**, **aliverkon**, **julkiseen IP-osoite**sekä **Verkon käyttöoikeusryhmän**. Tässä opetusohjelmassa yhdistämiskyselyssä pidä oletusarvot.

- Azure ottaa **Seuranta** oletusarvon mukaan samaa tallennustilan-tilillä AM nimetyt. Voit muuttaa näitä asetuksia.

- Määritä **saatavuus**, käytettävyyden määrittäminen. Tässä opetusohjelmassa yhdistämiskyselyssä Valitse **ei mitään**. Jos aiot määrittää SQL AlwaysOn käytettävyys ryhmät, Määritä käytettävyys välttämiseksi luotaessa virtuaalikoneen.  Lisätietoja on kohdassa [Manage näennäiskoneiden käytettävyyttä](virtual-machines-windows-manage-availability.md).

Kun olet tehnyt nämä asetukset valitsemalla **OK**.

## <a name="4-configure-sql-server-settings"></a>4. SQL server-asetusten määrittäminen
Määritä tiettyjä asetuksia ja optimointi SQL Serverin **SQL Serverin asetukset** -sivu. Käyttäjä voi määrittää SQL Serverin asetuksia ovat.

| Asetus               |
|---------------------|
| [Yhteys](#connectivity)              |
| [Todennus](#authentication)                |
| [Tallennustilan kokoonpanon](#storage-configuration)            |
| [Automaattinen korjaaminen](#automated-patching) |
| [Automaattisen varmuuskopioinnin](#automated-backup)             |
| [Azure avaimen säilöön-integrointi](#azure-key-vault-integration)             |
| [R-palvelut](#r-services) |

### <a name="connectivity"></a>Yhteys
Määritä käyttöoikeudet haluat tämän AM SQL Server-esiintymän **SQL-yhteys**. Tässä opetusohjelmassa yhdistämiskyselyssä valitsemalla **julkinen (internet)** voivat muodostaa yhteyden SQL Server koneet tai internet-palvelut. Kun tämä asetus on valittuna, Azure määrittää asetukset automaattisesti palomuurin ja verkko-käyttöoikeusryhmän sallimaan liikenne porttia 1433.  

![SQL-yhteyden asetukset](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-connectivity-alt.png)

Voit muodostaa yhteyden SQL Serverin Internetin kautta, myös ottamalla käyttöön SQL Server-todennus, joka on kuvattu seuraavassa osassa.

>[AZURE.NOTE] Ei lisärajoituksia verkon viestinnälle lisääminen SQL Server-AM. Voit tehdä tämän muokkaamalla verkko-käyttöoikeusryhmän AM luomisen jälkeen. Lisätietoja on artikkelissa [verkon suojauksen ryhmän (NSG) ominaisuudet?](../virtual-network/virtual-networks-nsg.md)

Jos haluat mieluummin Salli yhteydet-tietokantamoduulin Internetin kautta, valitse jokin seuraavista vaihtoehdoista:

- **Paikallinen (sisällä AM)** voivat muodostaa yhteyden SQL Server vain sisällä AM.
- **Yksityinen (sisällä Virtual Network)** voivat muodostaa yhteyden SQL Server-tietokoneissa tai palveluista virtual samassa verkostossa.

>[AZURE.NOTE] SQL Server Express edition virtuaalikoneen kuva ei tule automaattisesti käyttöön TCP/IP-protokollan. Tämä koskee myös julkisen ja yksityisen-yhteyden asetukset. Express Editionin on käytettävä SQL Serverin määritysten hallinta käyttöön [manuaalisesti TCP/IP-protokollan](#configure-sql-server-to-listen-on-the-tcp-protocol) AM luomisen jälkeen.

Voit parantaa suojausta, valitsemalla eniten rajoittava yhteys, joka sallii käyttämässäsi skenaariossa. Mutta kaikki vaihtoehdot ovat suojattavan verkon käyttöoikeusryhmän säännöt ja SQL/Windows-todennuksen avulla.

**Portti** oletusarvoisesti 1433. Voit määrittää eri porttinumero.
Lisätietoja on artikkelissa [yhteyden, SQL Server Virtual Machine (Resurssienhallinta) | Microsoft Azure](virtual-machines-windows-sql-connect.md).

### <a name="authentication"></a>Todennus
Jos asetat SQL Server-todennusta, valitse **Ota käyttöön** **SQL**-todennuksessa.

![SQL Server-todennus](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-authentication.png)

>[AZURE.NOTE] Jos aiot käyttää SQL Serverin Internetin välityksellä (eli julkisen yhteystapa), sinun on otettava tähän SQL-todennusta. Julkinen access SQL Server edellyttää SQL-todennusta.

Jos otat SQL Server-todennus, Määritä **käyttäjätunnus** ja **salasana**. Käyttäjänimi on määritetty SQL Server-todennus kirjautuminen ja kiinteä rooli **sysadmin** jäsen. Artikkelissa [Todennustila](http://msdn.microsoft.com/library/ms144284.aspx) lisätietoja todennus-tilassa.

Jos otat ei SQL Server-todennusta, valitse voit paikallisen järjestelmänvalvojatilin AM-muodostaa yhteyden SQL Server-esiintymän.

### <a name="storage-configuration"></a>Tallennustilan kokoonpanon
Valitse **tallennustilan kokoonpanon** määrittäminen tallennuksen vaatimukset.

![SQL-tallennustilan määritys](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-storage.png)

>[AZURE.NOTE] Jos valitset vakio tallennustilaa, tämä asetus ei ole käytettävissä. Automaattinen tallennustilan optimointi on käytettävissä vain Premium-tallennustilan.

Voit määrittää vaatimukset i/toimintoina sekunnissa (IOPs) siirtonopeuden Mt/s ja tallennustilan kokoa. Määritä nämä arvot liukuovia Skaalaa avulla. Portaalin laskee automaattisesti näitä vaatimuksia perusteella levyjen määrä.

Oletusarvon mukaan Azure optimoi 5000 IOPs, 200 MBs ja tallennustilaa 1 TT tallennustilaa. Voit muuttaa työmäärän tallennustilan nämä asetukset. Valitse **tallennustilan optimoitu**-kohdassa jokin seuraavista vaihtoehdoista:

- **Yleiset** on oletusasetus ja tukee useimmat toiminnoista.
- **Tapahtumallinen** käsittely optimoi perinteinen tietokannan OLTP työmääriä tallennustila.
- Analyyttisten ja raportoinnin työmääriä tallennustila **Tietovarastointi** optimoi.

>[AZURE.NOTE] Liukusäätimiä ylemmässä rajoitusten vaihtelevat valittua virtuaalikoneen koon mukaan.

### <a name="automated-patching"></a>Automaattinen korjaaminen
**Automaattinen korjaus** on käytössä oletusarvoisesti. Azure korjaustiedoston automaattisesti SQL Server- ja käyttöjärjestelmän automaattista korjausta avulla. Määritä päivä, viikko, kellonajan ja keston ylläpito-ikkunan. Azure-analyysityökalu korjataan ylläpito-ikkunassa. Ylläpito-ikkunassa aikataulun käyttää AM aluekohtaisia asetuksia aika. Jos et halua Azure korjaustiedoston automaattisesti ja SQL Server-käyttöjärjestelmää, valitse **Poista käytöstä**.  

![SQL automaattinen korjaaminen](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-patching.png)

Lisätietoja on artikkelissa [Automaattinen korjaaminen SQL Serverin Azuren näennäiskoneiden](virtual-machines-windows-sql-automated-patching.md).

### <a name="automated-backup"></a>Automaattisen varmuuskopioinnin
Ota käyttöön automaattinen tietokannan varmuuskopiointi-kohdassa **automaattisen varmuuskopioinnin**kaikkien tietokantojen. Automaattisen varmuuskopioinnin on oletusarvoisesti pois käytöstä.

Kun otat automaattisen varmuuskopioinnin SQL, voit määrittää seuraavasti:

- Säilytysajan jälkeinen aika (päiviä) varmuuskopioiden hakeminen
- Tallennustilan tilin varmuuskopioiden hakeminen
- Salaus-asetusta ja salasanan varmuuskopioiden hakeminen

Salaa varmuuskopio, valitse **Ota käyttöön**. Valitse Määritä **salasana**. Azure Luo varmenteen salaamaan varmuuskopioista ja käyttää määritetty salasana suojaa sertifikaatin.

![SQL automaattinen varmuuskopiointi](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-autobackup.png)

 Lisätietoja on artikkelissa [Automaattisen varmuuskopioinnin SQL Serverin Azuren näennäiskoneiden](virtual-machines-windows-sql-automated-backup.md).

### <a name="azure-key-vault-integration"></a>Azure avain säilöön-integrointi
Tallentaa suojauksen tietoja Azure salauksen, **Azure avaimen säilö integrointi** ja valitse **Ota**.

![SQL Azure avaimen säilö integrointi](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-akv.png)

Seuraavassa taulukossa on määritettävä Azure avaimen säilö integrointi parametrit.

|PARAMETRI|KUVAUS|ESIMERKKI|
|----------|----------|-------|
|**Avaimen säilö URL-osoite** |Avaimen säilö sijainti.|https://contosokeyvault.vault.Azure.NET/ |
|**Käyttäjätunnuksen** |Azure Active Directory pääasiallista nimi. Tämä nimi kutsutaan myös kuin asiakas.  |fde2b411 - 33d 5-4e11-af04eb07b669ccf2|
| **Lainan salaisuus**|Azure Active Directory service principal salainen. Tämä salaisuus kutsutaan myös asiakkaan toiminta. | 9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm/azF1XDKM =|
|**Tunnistetietojen nimi**|**Tunnistetietojen nimi**: AKV integrointi Luo tunnistetiedon SQL Serverissä salliminen AM käyttää avaimen säilö. Valitse tämä tunnistetiedon nimi.| mycred1|

Lisätietoja on artikkelissa [Määrittäminen Azure avaimen säilö integrointi SQL Server Azure VMs käyttöön](virtual-machines-windows-ps-sql-keyvault.md).

Kun olet valmis, SQL Server-asetusten määrittämisessä, valitse **OK**.

### <a name="r-services"></a>R-palvelut
SQL Server 2016 Enterprise Editionin sinulla halutessasi ottaa käyttöön [SQL Server R Services](https://msdn.microsoft.com/library/mt604845.aspx). Voit käyttää kehittyneen analyysin ja SQL Server 2016. Valitse **Ota käyttöön** **SQL Server-asetukset** -sivu.

![SQL Server R palvelujen ottaminen käyttöön](./media/virtual-machines-windows-portal-sql-server-provision/azure-vm-sql-server-r-services.png)

>[AZURE.NOTE] SQL Server-kuvia, jotka eivät ole 2016 Enterprise edition-asetuksen käyttöön R-palvelut on poistettu käytöstä.

## <a name="5-review-the-summary"></a>5. Tarkista yhteenveto
Valitse **Yhteenveto** -sivu Tarkista yhteenveto ja valitse **OK** , jos haluat luoda SQL Server, resurssiryhmä ja tämän AM määritettyjen resurssien.

Voit valvoa käyttöönoton azure-portaalista. **Ilmoitukset** -painike näytön yläreunassa näkyy käyttöönoton basic tila.

>[AZURE.NOTE] Voit antaa voit verrata kanssa käyttöönoton ajat, voin käyttöön SQL-AM Yhdysvaltojen Itä-alueen oletusasetuksilla. Testi-käyttöönoton kulunut korkeintaan 26 minuuttia. Mutta voi kärsiä nopeammin tai hitaammin käytön aikana alueen perusteella ja asetukset.

## <a name="open-the-vm-with-remote-desktop"></a>Avaa AM etätyöpöydän kanssa

Seuraavien vaiheiden avulla voit muodostaa yhteyden virtuaalikoneen etätyöpöydän kanssa:

1. Kun Azure AM on muodostettu AM kuvake tulee näkyviin Azure koontinäytössä. Voit myös etsiä se selaamalla aiemmin näennäiskoneiden. Valitse uusi SQL-virtuaalikoneen. **Virtuaalikoneen** -sivu näyttää virtuaalikoneen tiedot.
1. Valitse **Yhdistä** **virtuaalikoneen** sivu yläreunassa.
1. Selain lataa RDP-tiedostoon AM varten. Avaa RDP-tiedosto.
    ![SQL-AM etätyöpöydän](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-vm-remote-desktop.png)
1. Etätyöpöytäyhteys ilmoittaa, että tämä etäyhteyden publisher ei tunnisteta. Valitse **Yhdistä** Jatka.
1. Valitse **Windowsin suojaus** -valintaikkunan **Käytä toiseen tiliin**.
1. **Käyttäjänimi** -tyypin ** \<käyttäjänimi >**, jossa <user name> on käyttäjänimi, kun olet määrittänyt AM määrittämääsi. Sinun on lisättävä ensimmäinen kenoviiva, nimen edessä.
1. Kirjoita **salasana** , jotka olet aiemmin määritetty tämä AM ja valitse sitten **OK** , jos haluat yhdistää.
1. Jos toinen **Etätyöpöytäyhteys** -valintaikkuna kysyy, haluatko muodostaa, valitse **Kyllä**.

Kun muodostat yhteyden SQL Server-virtuaalikoneen, voit käynnistää SQL Server Management Studiossa ja Yhdistä Windows-todennuksen paikallisen järjestelmänvalvojan tunnistetiedoilla. Jos SQL Server-todennus on käytössä, voit myös muodostaa SQL-todennuksen avulla SQL-käyttäjätunnuksella ja -salasanalla valmistelun aikana määritetty.

Koneen Accessin avulla voit muuttaa suoraan tietokoneeseen ja SQL Server tarpeen mukaan. Voit esimerkiksi palomuurin asetusten määrittäminen tai muuttaminen SQL Serverin määritysten asetukset.

## <a name="connect-to-sql-server-remotely"></a>SQL Server etäkäyttö

Tässä opetusohjelmassa on valittu **julkisen** access virtuaalikoneen ja **SQL Server-todennusta**. Nämä asetukset on määritetty automaattisesti virtuaalikoneen sallimaan SQL Server-yhteyksien minkä tahansa Internetin välityksellä (olettaen että heillä on oikea SQL-kirjautuminen).

>[AZURE.NOTE] Jos olet valinnut julkisen valmistelun aikana, lisävaiheita tarvitaan käyttämään SQL Server-esiintymän Internetin välityksellä. Lisätietoja on ohjeaiheessa [yhteyden SQL Server-Virtual-Machine](virtual-machines-windows-sql-connect.md).

Seuraavissa osissa näyttää, miten voit muodostaa yhteyttä AM, SQL Server-esiintymä toiseen tietokoneeseen Internetin välityksellä.

> [AZURE.INCLUDE [Connect to SQL Server in a VM Resource Manager](../../includes/virtual-machines-sql-server-connection-steps-resource-manager.md)]

## <a name="next-steps"></a>Seuraavat vaiheet
Tietoja muista käyttämisestä SQL Server Azure-kohdassa [SQL Server-Azuren näennäiskoneiden](virtual-machines-windows-sql-server-iaas-overview.md) ja [Usein kysyttyihin kysymyksiin](virtual-machines-windows-sql-server-iaas-faq.md).

Katso video yleiskatsaus SQL Server-Azuren näennäiskoneiden [Azure AM on paras ympäristö SQL Server 2016](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/Azure-VM-is-the-best-platform-for-SQL-Server-2016).

[Tutustu Oppimispolkuun](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) Azuren näennäiskoneiden SQL Server.

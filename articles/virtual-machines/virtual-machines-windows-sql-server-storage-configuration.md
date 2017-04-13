<properties
    pageTitle="SQL Server VMs tallennustilan määritys | Microsoft Azure"
    description="Tässä ohjeaiheessa kerrotaan, miten Azure määrittää tallennustilan SQL Server VMs aikana valmistelu (resurssien hallinnan käyttöönottomalli). Myös ohjeessa kerrotaan, miten voit määrittää oman aiemmin SQL Server-VMs tallennustilan."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="ninarn"
    manager="jhubbard"    
    tags="azure-resource-manager"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/04/2016"
    ms.author="ninarn" />

# <a name="storage-configuration-for-sql-server-vms"></a>SQL Server VMs tallennustilan määritys

Kun määrität SQL Server-virtuaalikoneen kuva Azure-portaalin voit automatisoida tallennustilan määrittämisen. Tämä sisältää tallennustilan liittäminen AM, varmistetaan, että tallennustilan käyttämään SQL Server ja määrität sen voi optimoida suorituskykyä koskevat.

Tässä ohjeaiheessa kerrotaan, miten Azure määrittää tallennustilan SQL Server-VMs sekä aiemmin VMs valmistelun aikana. Tässä määrityksessä perustuu SQL Server Azure virtuaalilaitteiksi [suorituskykyyn liittyviä parhaita käytäntöjä](virtual-machines-windows-sql-performance.md) .

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]perinteinen käyttöönoton mallia.

## <a name="prerequisites"></a>Edellytykset
Automaattisen tallennus asetukset käyttämään virtuaalikoneen edellyttää seuraavat ominaisuudet:

- [SQL Server-valikoiman kuva](virtual-machines-windows-sql-server-iaas-overview.md#option-1-deploy-a-sql-vm-per-minute-licensing)valmistelun yhteydessä.
- Käyttää [resurssien hallinnan käyttöönottomalli](../resource-manager-deployment-model.md).
- Käyttää [Premium tallennuspaikkaa](../storage/storage-premium-storage.md).

## <a name="new-vms"></a>Uusi VMs
Seuraavissa kohdissa kuvataan uusien SQL Server-näennäiskoneiden tallennustila määrittämisestä.

### <a name="azure-portal"></a>Azure Portal
SQL Server-valikoiman kuva käyttämällä Azure-AM valmisteltaessa voit säilytyksen määrittäminen automaattisesti uuden AM. Määrittämäsi tallennustilan koko, suorituskyvyn rajoitukset ja työmäärää tyyppi. Seuraavassa näyttökuvassa näkyy SQL AM käytettyä tallennustilan määritys-sivu valmistelu.

![SQL Serverin AM tallennustilan määritys valmistelun aikana.](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration-provisioning.png)

Valintojen perusteella Azure suorittaa tallennustilan määritystehtävät AM luomisen jälkeen:

- Luo ja liittää premium tallennustilan tietojen levyjen virtuaalikoneen.
- Määrittää tietojen-levyä, jos haluat käyttää SQL Server.
- Määrittää tietojen levyjen kyselyjä tallennustilan varanto määritetty koko ja suorituskyky (IOPS ja siirtonopeuden) tarpeiden perusteella.
- Liittää tallennustilan altaan virtuaalikoneen uuden levyaseman.
- Optimoi uuden aseman määritettyä työmäärää-lajin, (Tietovarastointi, tapahtumallinen käsittelyn tai yleinen).

Lisätietoja siitä, miten Azure määrittää tallennustilan asetukset-kohdassa [tallennustilan Tietolähdemääritykset-osasta](#storage-configuration). Saat koko vaiheittainen kuvaus siitä, miten voit luoda SQL Server-AM Azure-portaalissa [valmistelu opetusohjelman](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="resource-manage-templates"></a>Resurssien hallinta-mallit
Jos käytät seuraavat Resurssienhallinta-mallit, kaksi premium tietojen levyä liitetyt mukana ei ole tallennustilan kokoonpano oletusarvoisesti. Voit kuitenkin mukauttaa malleista, jotka on liitetty virtuaalikoneen premium tietojen levyjen määrän muuttaminen.

- [Luo AM automaattisen varmuuskopioinnin kanssa](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-autobackup)
- [Luo AM automaattinen korjaaminen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-autopatching)
- [Luo AM AKV-integrointi](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-keyvault)

## <a name="existing-vms"></a>Olemassa olevan VMs
Voit muokata aiemmin SQL Server VMs Azure-portaalissa tallennustilan joitakin asetuksia. Valitse oman AM, siirry Settings (asetukset) ja valitse sitten SQL Server-määritys. SQL Server-määritys-sivu näyttää oman AM nykyistä tallennustilan käyttö. Tässä kaaviossa näkyvät kaikki asemista siellä olevia oman AM. Kunkin aseman tallennustilaa näyttää neljä osaa:

- SQL-tiedot
- SQL-loki
- Muut (-SQL tallennus)
- Käytettävissä

![Määritä tallennustilan aiemmin SQL Serverin AM](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration-existing.png)

Voit määrittää lisää uuden aseman tai laajentaa aiemmin luodun aseman tallennustilan valitsemalla kaavion yläpuolella Muokkaa-linkki.

Määritysvaihtoehtoja, joita näet vaihtelee sen mukaan, onko ovat käyttäneet tätä ominaisuutta ennen. Kun käytät ensimmäistä kertaa, voit määrittää tallennustilan tarpeen uutta asemaa. Jos olet aiemmin käyttänyt tätä ominaisuutta aseman luomiseen, voit laajentaa levyn tallennustilan.

### <a name="use-for-the-first-time"></a>Käytä ensimmäistä kertaa
Jos käytät tätä ominaisuutta ensimmäistä kertaa, voit määrittää uuden aseman tallennustilan koko ja suorituskyky rajoituksia. Tämä ongelma on samanlainen kuin mitä nähdä osoitteessa valmistelu aika. Tärkein ero on se, että sinun ei sallita voit määrittää kuormituksen. Tämä rajoitus estää toiminnan-virtuaalikoneen kaikki aiemmin SQL Server-määrityksiä.

![Määritä SQL Server-tallennustilan liukusäädin](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-usage-sliders.png)

Azure Luo uuden aseman määritysten mukaisesti. Tässä skenaariossa Azure suorittaa tallennustilan määritystehtävät:

- Luo ja liittää premium tallennustilan tietojen levyjen virtuaalikoneen.
- Määrittää tietojen-levyä, jos haluat käyttää SQL Server.
- Määrittää tietojen levyjen kyselyjä tallennustilan varanto määritetty koko ja suorituskyky (IOPS ja siirtonopeuden) tarpeiden perusteella.
- Liittää tallennustilan resurssivarantoon virtuaalikoneen uuden levyaseman.

Lisätietoja siitä, miten Azure määrittää tallennustilan asetukset-kohdassa [tallennustilan Tietolähdemääritykset-osasta](#storage-configuration).

### <a name="add-a-new-drive"></a>Lisää uuden aseman
Jos olet jo määrittänyt tallennustilan SQL Server-AM, laajentaminen tallennustilan näyttää kaksi uudet asetukset. Ensimmäinen vaihtoehto on Lisää uusi asema, joka voi parantaa oman AM suorituskyvyn taso.

![Lisää uuden aseman SQL-AM](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration-add-new-drive.png)

Kun olet lisännyt asema, sinun on tehtävä joitakin ylimääräisiä Manuaalinen määritys saavuttamiseksi suorituskyky paranee.

### <a name="extend-the-drive"></a>Laajenna asema
Muut vaihtoehto laajentaminen tallennustilan on laajentaa aiemmin asemaa. Tämä vaihtoehto kasvattaa kiintolevyn tallennustilaa, mutta se ei paranna suorituskykyä. Tallennustilan jakavat kanssa et voi muuttaa sarakkeiden määrä, tallennustilan ryhmän luomisen jälkeen. Sarakkeiden määrä määrittää rinnakkain kirjoituksia, jotka voidaan Raidallinen tietojen levyille määrän. Tämän vuoksi kaikki lisätyt tiedot levyjä ei voi parantaa suorituskykyä. Vain saat käyttöösi lisää tallennustilaa tietojen kirjoittamisen. Tämä rajoitus myös tarkoittaa, että, kun laajentaminen asema sarakkeiden määrä määrää tietojen levyjä, voit lisätä vähimmäismäärä. Jos luot tallennustilan resurssivarantoon neljä tietojen levyjä, sarakkeiden määrä on myös neljä. Tahansa, kun laajennat tallennuspaikkaa, sinun on lisättävä vähintään neljä tietojen levyjä.

![Laajentaa asemaa SQL-AM varten](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-extend-a-drive.png)

## <a name="storage-configuration"></a>Tallennustilan kokoonpanon
Tässä osassa on viittaus tallennustilan määritysmuutoksia joka Azure suorittaa valmistelu SQL-AM tai määritysten Azure-portaalissa aikana.

- Jos olet valinnut vähemmän kuin kaksi TBs tallennustilan lisääminen AM Azure ei luoda tallennustilan resurssivarantoon.
- Jos olet valinnut vähintään kaksi TBs tallennustilan lisääminen AM Azure määrittää tallennustilan resurssivarantoon. Tämän ohjeaiheen seuraavassa osassa on tietoja tallennustilan resurssivarantoon kokoonpanon.
- Automaattinen tallennustilan kokoonpanon aina käyttää [premium tallennustilan](../storage/storage-premium-storage.md) P30 tietojen levyjä. Näin ollen on 1:1 yhdistämisen teratavua valitun määrä ja tietojen levyjen liitetty oman AM määrä.

Alla on tietoja, **Levytilasta** -välilehden artikkelissa [tallennustilan hinnoittelu](https://azure.microsoft.com/pricing/details/storage) -sivulla.

### <a name="creation-of-the-storage-pool"></a>Tallennustilan resurssivarannon luomista
Azure käyttää seuraavia asetuksia luo SQL Server VMs tallennustilan resurssivarantoon.

| Asetus | Arvo |
|-----|-----|
| Raita kokoa  | 256 kt (Tietovarastointi); 64 Kilotavua (tapahtumien) |
| Levyn koot | 1 TT / |
| Välimuistin | Luku |
| Varauksen koko | 64 Kilotavua NTFS kohdistus kokoa |
| Lähetä tiedosto valmistelu | Käytössä |
| Muistiin sivujen lukitseminen | Käytössä |
| Palautus | Yksinkertainen palauttaminen (ei vikasietoisuudelle) |
| Sarakkeiden määrä | Tietoja levyjen<sup>1</sup> määrä |
| TempDB sijainti | Tallennetut tiedot levyjä<sup>2</sup> |

<sup>1</sup> kun tallennustilan varanto on luotu, et voi muuttaa tallennustilan resurssivarantoon sarakkeiden määrä.

<sup>2</sup> tämä asetus koskee vain ensimmäinen asema tallennustilan kokoonpano-ominaisuuden avulla.

## <a name="workload-optimization-settings"></a>Kuormituksen hakukoneoptimoinnin asetukset
Seuraavassa taulukossa esitellään käytettävissä olevat kolme työmäärää tyyppi-asetukset ja niiden vastaavan optimointi:

| Kuormituksen tyyppi | Kuvaus | Optimointi |
|-----|-----|-----|
| **Yleiset** | Oletusasetus, joka tukee useimmat toiminnoista | Ei mitään |
| **Tapahtumien käsittely** | Optimoi perinteinen tietokannan OLTP työmääriä tallennustila | Jäljitä merkintä 1117<br/>Jäljitä merkintä 1118 |
| **Tietovaraston** | Optimoi Analyyttisten ja raportoinnin työmääriä tallennustila | Jäljitä merkintä 610<br/>Jäljitä merkintä 1117 |

>[AZURE.NOTE] Voit määrittää kuormituksen tyyppi vain, kun SQL-virtual machine valmistelu valitsemalla tallennustilan määritys vaiheessa.

## <a name="next-steps"></a>Seuraavat vaiheet
Katso muita aiheeseen liittyviä suorittamalla SQL Server Azure VMs:, [SQL Server-Azuren näennäiskoneiden](virtual-machines-windows-sql-server-iaas-overview.md).

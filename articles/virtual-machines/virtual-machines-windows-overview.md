<properties
    pageTitle="Windows näennäiskoneiden yleiskatsaus | Microsoft Azure"
    description="Lisätietoja luomisen ja ylläpidon näennäiskoneiden Windows Azure-tietokannassa."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/20/2016"
    ms.author="davidmu"/>

# <a name="overview-of-windows-virtual-machines-in-azure"></a>Yleistä näennäiskoneiden Windows Azure-tietokannassa

Azuren näennäiskoneiden (AM) on useita erilaisia [tarvittaessa, skaalattava tietojenkäsittely resurssit](../app-service-web/choose-web-site-cloud-service-vm.md) , joka tarjoaa Azure. Voit valita yleensä AM tietojenkäsittely-ympäristön haluat hallita kuin muut vaihtoehdot tarjoaa tarvittaessa. Tässä artikkelissa tutustutaan tietoja siitä, mitä Ota huomioon ennen kuin luot AM, miten voit luoda ja miten voit hallita.

Azure-AM lisää joustavuutta virtualization, eikä sinun tarvitse ostaa ja ylläpitää fyysinen laite, se suoritetaan. Kuitenkin yhä haluat säilyttää AM suorittaa tehtäviä, kuten määrittäminen, päivitetään ja ohjelmistojen, joka suoritetaan, sitä.

Azure-virtuaalikoneissa voidaan käyttää eri tavoin. On joitakin esimerkkejä:

- **Kehittäminen ja testaa** – Azure VMs tarjoavat on nopea ja helppo tapa luomiseen tietokoneen tiettyjä määrityksiä tarvitse koodi ja testaa sovelluksen.
- **Sovellusten pilveen** – sovelluksen tarvittaessa voit lisävaluutta, koska se voi tulkita taloudellisen suoritettaviksi AM Azure-tietokannassa. Maksat ylimääräisiä VMs, kun tarvitset niitä ja Sammuta niitä tarvittaessa.
- **Laajennettu palvelinkeskuksen** – näennäiskoneiden Azure virtual verkossa helposti voidaan yhdistää organisaation verkkoon.

Numero, joka käyttää sovelluksen VMs skaalata ylös- ja, mikä on pakollinen vastaamaan omia tarpeita.

## <a name="what-do-i-need-to-think-about-before-creating-a-vm"></a>Mitä tarvitsen ennen kuin luot AM mietittävä?

On aina [tyyliseikat](virtual-machines-windows-infrastructure-virtual-machine-guidelines.md) monipuolisia kun luot ulos sovelluksen infrastruktuuri Azure-tietokannassa. Nämä AM ovat tärkeä ottaa huomioon ennen aloittamista:

- Sovelluksen resurssien nimet
- Sijainti, johon on tallennettu resurssit
- Koon AM
- VMs, joita voidaan luoda enimmäismäärä
- Käyttöjärjestelmä, joka suoritetaan AM
- Sen jälkeen, kun se käynnistyy AM määrittäminen
- Aiheeseen liittyvät resurssit AM korjattava

### <a name="naming"></a>Nimeäminen

Virtual machine on sille [nimi](virtual-machines-windows-infrastructure-naming-guidelines.md) , ja se on määritetty osana käyttöjärjestelmän tietokonenimi. Nimi, AM voi olla enintään 15 merkkiä.

Jos käytät Azure luomaan käyttöjärjestelmän disk, tietokonenimi sekä nimi, virtuaalikoneen ovat samat. Jos voit [ladata ja käyttää oma kuva](virtual-machines-windows-upload-image.md) , joka sisältää aiemmin määritetyn käyttöjärjestelmän ja luoda virtual machine-nimet voivat olla eri. On suositeltavaa, että kun olet ladannut kuvan-tiedoston, tekemäsi tietokoneen käyttöjärjestelmä ja virtuaalikoneen nimi sama.

### <a name="locations"></a>Sijainnit

Kaikki resurssit, jotka on luotu Azure jakautuu useita [maantieteellisillä alueilla](https://azure.microsoft.com/regions/) eri puolilla maailmaa. Yleensä alueen kutsutaan **sijainti** , kun luot AM. AM sijainti määrittää levyjä tallennuspaikka.

Tässä taulukossa on joitakin tapoja, joilla saat luettelon käytettävissä olevat sijainnit.

| Menetelmä | Kuvaus |
| ------ | ----------- |
| Azure portal | Valitse sijainti-luettelosta, kun luot AM. |
| Azure PowerShell | [Hae AzureRmLocation](https://msdn.microsoft.com/library/mt619449.aspx) -komennolla. |
| REST-OHJELMOINTIRAJAPINNALLA | [Luettelon sijainnit](https://msdn.microsoft.com/library/dn790540.aspx) -toiminnolla. |

### <a name="vm-size"></a>AM kokoa

[Koon](virtual-machines-windows-sizes.md) , jota käytetään AM määräytyy työmäärää, jonka haluat suorittaa. Valitse sitten haluamasi kokoinen määrittää tekijöistä, kuten käsittelyn tehon, muistin ja tallennustilaa. Azure tarjoaa erilaisia koot tukemaan käyttää useita erityyppisiä.

Azure kulujen [tunnin välein hinta](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) AM koon ja käyttöjärjestelmän perusteella. Osittainen tuntia, Azure ovat vain käytetään minuuttiin. Tallennustilan hinnoiteltu ja veloitetaan erikseen.

### <a name="vm-limits"></a>AM rajoitukset

Tilauksesi on oletusarvoinen [kiintiötä](../azure-subscription-service-limits.md) paikkaa, jossa voi olla vaikutusta monta VMs projektin käyttöönotto. Kohti tilauksen välein nykyisen enimmäismäärä on 20 VMs alueittain. Rajoitukset on aiheutuneet siirtämiseen tuki lippu pyytää kasvaa.

### <a name="operating-system-disks-and-images"></a>Käyttöjärjestelmä levyille ja -kuvat

Näennäiskoneiden [virtual kiintolevyillä (näennäiskiintolevyjen)](virtual-machines-windows-about-disks-vhds.md) avulla voit tallentaa käyttöjärjestelmä (OS)- ja tiedot. Näennäiskiintolevyjen käytetään myös kuvia, voit valita Asenna Käyttöjärjestelmä. 

Azure on monta [marketplace kuvia](https://azure.microsoft.com/marketplace/virtual-machines/) eri versioita ja Windows Server-käyttöjärjestelmät tyypit käytettäväksi. Marketplace kuvia tunnistetaan kuva Publisherin, tarjouksen tai sku versio (yleensä versio määritetään uusin versio). 

Tässä taulukossa on joitakin tapoja, joilla voit etsiä kuvan tiedot.

| Menetelmä | Kuvaus |
| ------ | ----------- |
| Azure portal | Arvot määritetään automaattisesti, kun valitset kuva. |
| Azure PowerShell | [Hae AzureRMVMImagePublisher](https://msdn.microsoft.com/library/mt603484.aspx) -"sijainnin" sijainti<BR>[Hae AzureRMVMImageOffer](https://msdn.microsoft.com/library/mt603824.aspx) -"sijainnin" sijainti-Publisher "publisherName"<BR>[Hae AzureRMVMImageSku](https://msdn.microsoft.com/library/mt619458.aspx) -sijainti "sijainti"-"publisherName" Publisher-tarjota "offerName" |
| REST API | [Kuva julkaisijoiden luetteloon](https://msdn.microsoft.com/library/mt743702.aspx)<BR>[Kuva taulukkoluettelo tarjoaa](https://msdn.microsoft.com/library/mt743700.aspx)<BR>[Luettelon kuvan tuotteissa](https://msdn.microsoft.com/library/mt743701.aspx) |

Voit [ladata ja käyttää oma kuva](virtual-machines-windows-upload-image.md) ja, kun haluat tehdä julkaisijan nimi, tarjous ja tuote ei käytetä.

### <a name="extensions"></a>Tunnisteet

AM [tunnisteet](virtual-machines-windows-extensions-features.md) antaa AM lisäominaisuuksia kirjaa käyttöönoton määritys ja automaattisia tehtäviä.

Yleisiä tehtäviä onnistuu käyttämällä tunnisteet:

- **Mukautetun komentosarjojen suorittamisen** – [Mukautettu komentosarja-tunniste](virtual-machines-windows-extensions-customscript.md) auttaa työmääriä määrittämistä AM suorittamalla komentosarja AM on valmistelun yhteydessä.
- **Käyttöönotto ja hallita käyttömahdollisuudet** – [PowerShell toivottuja tilan määrittäminen (DSC) tunniste](virtual-machines-windows-extensions-dsc-overview.md) auttaa määrittämään DSC AM-määrityksiä ja ympäristöissä.
- **Diagnostiikan tietojen kerääminen** – [Azure diagnostiikka tunniste](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) avulla voit määrittää, jonka avulla voidaan valvoa sovellusta kunto diagnostiikka tietojen keräämiseen AM.

### <a name="related-resources"></a>Aiheeseen liittyvät resurssit

Tässä taulukossa resurssit AM käytössä, eikä sitä tarvitse olemassa tai voi luoda, jos AM luodaan.

| Resurssi | Pakollinen | Kuvaus |
| -------- | -------- | ----------- |
| [Resurssiryhmä](../azure-resource-manager/resource-group-overview.md) | Kyllä | Resurssiryhmä täytyy sisältyä AM. |
| [Tallennustilan tilin](../storage/storage-create-storage-account.md) | Kyllä | AM täytyy tallentaa sen virtual kiintolevyillä tallennustilan-tili. |
| [VPN](../virtual-network/virtual-networks-overview.md) | Kyllä | AM on kuuluttava virtual verkkoon. |
| [Julkinen IP-osoite](../virtual-network/virtual-network-ip-addresses-overview-arm.md) | Ei | AM voi olla määritetty käyttämään sitä etäyhteyden julkiseen IP-osoite. |
| [Verkkokäyttöliittymä](../virtual-network/virtual-network-network-interface-overview.md) | Kyllä | AM on verkkoliittymän voi olla yhteydessä verkkoon. |
| [Tietoja levyjen](virtual-machines-windows-attach-disk-portal.md) | Ei | AM voi olla tietoja levyjen Laajenna tallennustilan ominaisuuksia. |

## <a name="how-do-i-create-my-first-vm"></a>Miten voin luoda omat ensimmäisen AM?

Sinulla on useita vaihtoehtoja oman AM luomiseen. Valinta, jotka teet riippuu siitä, että olet ympäristössä. 

Tässä taulukossa on tietoja, joiden avulla pääset alkuun oman AM.

| Menetelmä | Artikkelissa |
| ------ | ------- |
| Azure portal | [Luo virtual koneeseen, jossa käytetään Windows-portaalissa](virtual-machines-windows-hero-tutorial.md) |
| Mallit | [Voit luoda Windows-virtual machine Resurssienhallinta-mallin avulla](virtual-machines-windows-ps-template.md) |
| Azure PowerShell | [Luo Windows-AM PowerShellin avulla](virtual-machines-windows-ps-create.md) |
| Asiakkaan SDK: T | [Ottaa käyttöön käyttämällä C# Azure-resurssit](virtual-machines-windows-csharp.md) |
| REST API | [Luo tai päivitä AM](https://msdn.microsoft.com/library/mt163591.aspx) |

Voit Toivottavasti koskaan tähän, mutta joskus jotain vikaan. Jos näin käy, tarkista tiedot [vianmääritys Resurssienhallinta käyttöönoton ongelmat luominen Windows-virtual machine Azure-tietokannassa](virtual-machines-windows-troubleshoot-deployment-new-vm.md).

## <a name="how-do-i-manage-the-vm-that-i-created"></a>Miten voin hallita AM, joka on luotu?

VMs voi hallita selainpohjaisia portal komentorivin työkalujen tukeen komentosarjojen tai suoraan ohjelmointirajapinnan käyttäminen. Jotkin tyypillinen hallintatehtäviä, joita voi suorittaa jää tietoja AM, AM käytettävyyden hallinta ja varmuuskopioiden kirjautuminen.

### <a name="get-information-about-a-vm"></a>Tietojen AM tarkasteleminen

Tässä taulukossa kerrotaan joitakin tapoja, joilla saat tietoja AM.

| Menetelmä | Kuvaus |
| ------ | ----------- |
| Azure portal | Valitse toiminto-valikosta **näennäiskoneiden** ja valitsemalla luettelosta AM. Sivu AM varten, valitse pystyt käyttämään yleisiä tietoja arvojen asettaminen ja valvonta arvot. |
| Azure PowerShell | Lisätietoja VMs hallinta PowerShellin avulla on artikkelissa [Hallitse Azuren näennäiskoneiden Resurssienhallinta ja PowerShellin avulla](virtual-machines-windows-ps-manage.md). |
| REST-OHJELMOINTIRAJAPINNALLA | Tietojen tarkasteleminen AM [Hae AM tiedot](https://msdn.microsoft.com/library/mt163682.aspx) -toiminnon avulla. |
| Asiakkaan SDK: T | Tietoja käyttämällä C# hallittavan VMs on artikkelissa [Azure Resurssienhallinta ja C# avulla hallita Azuren näennäiskoneiden](virtual-machines-windows-csharp-manage.md). |

### <a name="log-on-to-the-vm"></a>Kirjaudu AM

Yhdistä-painikkeella [Remote Desktop RDP-istunnon](virtual-machines-windows-connect-logon.md)aloittaminen Azure-portaalissa. Voit joskus sujuu virhe yritettäessä käyttää etäyhteyden. Jos näin käy, katso ohjeet [vianmääritys etätyöpöydän](virtual-machines-windows-troubleshoot-rdp-connection.md)yhteyksien Azure virtual-tietokoneeseen, jossa on käytössä Windows.

### <a name="manage-availability"></a>Käytettävyyden hallinta

On tärkeää ymmärtää sovelluksen [suuren käytettävyyden varmistamiseksi](virtual-machines-windows-manage-availability.md) . Tämä määritys tehdään luominen useita VMs, että vähintään yksi on käytössä.

Myös Microsoftin 99.95 AM tason palvelusopimus oikeutettu käyttöönoton pystyttävä käyttöönotto kahden tai useamman virtuaalilaitteiksi havainnollistamiseen sisällä [saatavuus](virtual-machines-windows-infrastructure-availability-sets-guidelines.md). Määritysten varmistaa, että VMs jaetaan vika useita toimialueita ja otetaan käyttöön isännät eri ylläpito Windowsin sivulle. Koko [Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_0/) kerrotaan kokonaisuutena Azure taattua käytettävyyttä.
 
### <a name="back-up-the-vm"></a>Varmuuskopiointi AM
 
Tietoja ja Azure varmuuskopiointi-ja Azure palauttaminen omaisuuden suojaaminen käytetään [palautus Services säilö](../backup/backup-introduction-to-azure-backup.md) . Voit käyttää palautus Services säilöön, ottaa [käyttöön ja hallita Resurssienhallinta käyttöön VMs PowerShellin varmuuskopiot](../backup/backup-azure-vms-automation.md). 

## <a name="next-steps"></a>Seuraavat vaiheet

- Jos tarkoituksen Linux VMs käsitteleminen, tarkista [Azure ja Linux](virtual-machines-linux-azure-overview.md).
- Lue lisää profiilikuva ympärille määrittäminen [Esimerkki Azure infrastruktuurin ongelmatilanteita](virtual-machines-windows-infrastructure-example.md)-infrastruktuuria.
- Varmista, että noudattamalla [Parhaat käytännöt Windows-AM Azure-käyttöjärjestelmä](virtual-machines-windows-guidance-compute-single-vm.md).



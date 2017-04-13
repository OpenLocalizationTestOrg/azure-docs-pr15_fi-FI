<properties
    pageTitle="Azure virtuaalikoneen varmuuskopiointi vianmääritys | Microsoft Azure"
    description="Vianmääritys Azuren näennäiskoneiden palauttaminen ja varmuuskopiointi"
    services="backup"
    documentationCenter=""
    authors="trinadhk"
    manager="shreeshd"
    editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/26/2016"
    ms.author="trinadhk;jimpark;"/>


# <a name="troubleshoot-azure-virtual-machine-backup"></a>Vianmääritys Azure virtuaalikoneen varmuuskopiointi

> [AZURE.SELECTOR]
- [Palautus services säilöön](backup-azure-vms-troubleshoot.md)
- [Varmuuskopion säilöön](backup-azure-vms-troubleshoot-classic.md)

Voit tehdä vianmäärityksen havaitsi samalla, kun Azure varmuuskopioinnista tiedoilla lueteltu alla olevassa taulukossa.

## <a name="discovery"></a>Etsiminen

| Varmuuskopiointi | Virheen yksityiskohtaiset tiedot | Vaihtoehtoinen menetelmä |
| -------- | -------- | -------|
| Etsiminen | Tutustu uudet kohteet - ja Microsoft Azure varmuuskopiointi tapahtui sisäinen virhe epäonnistui. Odota muutama minuutti ja yritä sitten uudelleen. | Yritä etsiminen prosessin 15 minuutin kuluttua.
| Etsiminen | Saat tietää, uusien kohteiden – epäonnistui toiseen etsintä-toiminto on jo käynnissä. Odota, kunnes nykyinen etsintä-toiminto on suoritettu. | Ei mitään |

## <a name="register"></a>Rekisteröidy
| Varmuuskopiointi | Virheen yksityiskohtaiset tiedot | Vaihtoehtoinen menetelmä |
| -------- | -------- | -------|
| Rekisteröidy | Tietoja levyjen liitetty virtuaalikoneen määrä ylittää tuetun raja - Ota irrottaa joidenkin tietojen levyjen virtual laitteessa ja yritä uudelleen. Azure varmuuskopiointi tukee jopa 16 tietojen levyjä, jotka on liitetty Azure virtual-konetta varmuuskopion | Ei mitään |
| Rekisteröidy | Microsoft Azure varmuuskopiointi sisäinen virhe - Odota muutama minuutti ja yritä sitten uudelleen. Jos ongelma jatkuu, ota yhteyttä Microsoft Support. | Saat virheen vuoksi jokin AM seuraavat määritystä ei tueta Premium LRS. <br> Tallennustilan Premium VMs voi varmuuskopioida käyttämällä palautus services säilö. [Opi lisää](backup-introduction-to-azure-backup.md/#back-up-and-restore-premium-storage-vms) |
| Rekisteröidy | Asenna agentti toiminnon aikakatkaisu rekisteröinti epäonnistui | Tarkista, jos virtuaalikoneen käyttöjärjestelmäversio tukee. |
| Rekisteröidy | Komennon suorittaminen epäonnistui - toinen toiminto on käynnissä kohteen. Odota, kunnes edellinen toiminto on valmis | Ei mitään |
| Rekisteröidy | Näennäiskoneiden on virtual kiintolevyillä tallennetun Premium tallennustilan ei tueta varmuuskopiointi | Ei mitään |
| Rekisteröidy | Virtuaalikoneen-agentti ei ole virtuaalikoneen – Asenna tarvittavat vanhat tarvittavat AM agentti ja Käynnistä toiminto uudelleen. | [Lue lisää](#vm-agent) tietoja AM agentti asennuksen ja asennuksen AM agentti. |

## <a name="backup"></a>Varmuuskopiointi

| Varmuuskopiointi | Virheen yksityiskohtaiset tiedot | Vaihtoehtoinen menetelmä |
| -------- | -------- | -------|
| Varmuuskopiointi | Tilannevedoksen tila AM-agentti ei voitu yhteyttä. Tilannevedoksen AM sub tehtävän aikakatkaisu. -, Katso vianmääritystietoja opas siitä, miten voit ratkaista ongelman. | Tämä virhe ilmenee, jos AM-agentti ongelma tai Azure infrastruktuurin verkkokäyttö on estetty jollakin tavalla. Lisätietoja [Virheenkorjaus AM tilannevedoksen määrittäminen ongelmien vianmääritys](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md). <br> Jos AM-agentti ei ole aiheuttaa ongelmia, Käynnistä AM. Joskus AM virheellisessä tilassa voi aiheuttaa ongelmia ja uudelleen AM palauttaa tämän "virheellisessä tilassa" |
| Varmuuskopiointi | Sisäinen virhe epäonnistui varmuuskopiointi - yritä toimintoa muutaman minuutin kuluttua. Jos ongelma jatkuu, ota yhteyttä Microsoft Support | Tarkista, onko käytettäessä AM tallennustilan lyhytkestoisia ongelma. Tarkista onko kaikki jatkuvaa ongelman liittyvät suorittaminen ja tallennustilaa/verkon alueen [Azure tila](https://azure.microsoft.com/en-us/status/) . Ota joiden lievity uudelleen varmuuskopion kirjaa ongelman. |
| Varmuuskopiointi | Toiminto ei voi suorittaa, kun AM ei enää ole. | Varmuuskopiointi ei voi suorittaa, koska määritetty varmuuskopiointi AM on poistettu. Lopeta edelleen varmuuskopioiden siirtymällä suojatun kohteet-näkymässä, valitse suojatun kohde ja valitse sitten Lopeta suojaus. Voit säilyttää tiedot valitsemalla Säilytä varmuuskopio tietojen vaihtoehto. Voit jatkaa suojaus myöhemmin napsauttamalla virtual tämän tietokoneen määrittäminen-suojauksen rekisteröity kohteet-näkymässä|
| Varmuuskopiointi | Ei pystynyt asentamaan Azure palautus-palveluiden valitulle kohteelle - AM agentti tunniste on vanhat tarvittavat Azure palautus palveluiden laajennuksen. Asenna Azure AM-agentti ja Käynnistä rekisteröinti-toiminto | <ol> <li>Tarkista, jos AM-agentti on asennettu oikein. <li>Varmista, että viestilipun, AM config on asetettu oikein.</ol> [Lue lisää](#validating-vm-agent-installation) tietoja AM agentti asennuksen ja asennuksen AM agentti. |
| Varmuuskopiointi | -Komennon suorittaminen epäonnistui - toinen toiminto on parhaillaan käynnissä kohteen. Odota, kunnes edellinen toiminto on valmis, ja yritä sitten uudelleen | On olemassa varmuuskopio tai AM palautustyön on käynnissä ja uusi projekti ei voi aloittaa, olemassa olevan projektin on käynnissä. |
| Varmuuskopiointi | Tunniste-asennus epäonnistui virheen "COM + ei voi ottaa yhteyttä Microsoftin Distributed tapahtuman koordinaattien | Tämä yleensä tarkoittaa, että COM +-palvelu ei ole käynnissä. Saat lisätietoja ongelman korjaamisesta, ota yhteyttä Microsoftin tuotetukeen. |
| Varmuuskopiointi | Tilannevedoksen epäonnistui VSS toiminnon virheen "BitLocker-salaus lukittu aseman. Lukitus täytyy poistaa aseman Ohjauspaneelin kautta. | Kaikki asemista AM BitLocker käytöstä ja noudata Jos VSS ongelma on ratkaistu |
| Varmuuskopiointi | Näennäiskoneiden on virtual kiintolevyillä tallennetun Premium tallennustilan ei tueta varmuuskopiointi | Ei mitään |
| Varmuuskopiointi | Azure Virtual Machine ei löydy. | Tämä tapahtuu, kun ensisijainen AM poistetaan, mutta varmuuskopion käytäntö jatkaa Etsi AM haluat varmuuskopioida. Voit korjata virheen: <ol><li>Luo virtuaalikoneen sama nimi ja saman resurssin ryhmänimi [cloud palvelunimi] <br>(OR) <li> Käytöstä tämän AM suojaus, niin, että seuraavien varmuuskopioiden ei käynnistetään. </ol> |
| Varmuuskopiointi | Virtuaalikoneen-agentti ei ole virtuaalikoneen – Asenna tarvittavat vanhat tarvittavat AM agentti ja Käynnistä toiminto uudelleen. | [Lue lisää](#vm-agent) tietoja AM agentti asennuksen ja asennuksen AM agentti. |

## <a name="jobs"></a>Projektit
| Toiminto | Virheen yksityiskohtaiset tiedot | Vaihtoehtoinen menetelmä |
| -------- | -------- | -------|
| Työn peruuttaminen | Peruutus ei tueta tässä työlaji - Odota, kunnes työ on valmis. | Ei mitään |
| Työn peruuttaminen | Työn ei ole peruutettava tilaan – Odota, kunnes työ on valmis. <br>TAI<br> Valitun työn ei ole peruutettava tilaan – Odota työn suorittamiseen.| Kaiken todennäköisyyden työ on melkein valmis; Odota, kunnes työ on valmis |
| Työn peruuttaminen | Työn voi peruuttaa, koska se ei ole käynnissä - peruutus tuetaan vain työt, jotka ovat käynnissä. Yritys Peruuta tekstimuodossa edistymisestä projektin. | Tämä tapahtuu näyttöominaisuusvaihtoehdon tilan vuoksi. Odota hetki ja yritä uudelleen Peruuta-toiminto |
| Työn peruuttaminen | Peruuta työ - Odota, kunnes työ on valmis epäonnistui. | Ei mitään |


## <a name="restore"></a>Palauttaminen
| Toiminto | Virheen yksityiskohtaiset tiedot | Vaihtoehtoinen menetelmä |
| -------- | -------- | -------|
| Palauttaminen | Palauttaminen epäonnistui Cloud sisäinen virhe | <ol><li>DNS-asetukset on määritetty, johon yrität palauttaa pilvipalvelussa. Voit tarkistaa <br>$deployment = get-AzureDeployment - palvelun nimi "palvelun nimi"-paikka "Tuotannon" Get-AzureDns - DnsSettings $deployment. DnsSettings<br>Jos näkyvissä on määritetty osoite, tämä tarkoittaa, että DNS-asetukset on määritetty.<br> <li>Pilvipalvelussa, johon haluat yrität palauttaa on määritetty ReservedIP ja aiemmin VMs pilvipalvelussa on pysäytetty-tilaan.<br>Voit tarkistaa pilvipalveluun on varattu IP käyttämällä seuraavia PowerShellin cmdlet-komennot:<br>$deployment = get-AzureDeployment - palvelun nimi "palvelun nimi"-paikka "Tuotannon" $estämistoiminnon ReservedIPName <br><li>Palauttaa saman pilvipalveluun virtual tietokoneessa, jossa seuraavat määräten verkon määritykset rooliin. <br>-Näennäiskoneiden kohdassa kuormituksen tasauspalvelun määritys (sisäinen ja ulkoinen)<br>-Näennäiskoneiden kanssa useita varattu IP-osoitteet<br>-Näennäiskoneiden useita NIC kanssa<br>Valitse uusi pilvipalvelussa Käyttöliittymän tai tutustu [palauttaa huomioon otettavia seikkoja](./backup-azure-restore-vms.md/#restoring-vms-with-special-network-configurations) , VMs määräten verkon määritysten kanssa</ol> |
| Palauttaminen | Valitun DNS-nimi on varattu – Määritä eri DNS-nimi ja yritä uudelleen. | DNS-nimi tähän viittaa cloud palvelunimi (yleensä päättyen. cloudapp.net). Tämä on oltava yksilöllinen. Tämä virhe ilmenee, jos haluat valita eri AM nimellä palauttamisen aikana. <br><br> Tämä virhe näkyy Azure-portaalin käyttäjille. PowerShellin kautta palautustoiminto onnistuu, koska se vain palauttaa levyjä ja ei luoda AM. Virhe järjestelmässä AM erikseen luomisen itse levyn palautuksen jälkeen. |
| Palauttaminen | Määritetyn virtual verkon määritys ei ole oikein – Määritä eri virtual verkon määritys ja yritä uudelleen. | Ei mitään |
| Palauttaminen | Määritetyn pilvipalvelussa käyttää varattu IP, joka ei vastaa palautetaan parhaillaan virtuaalikoneen määritys – Määritä eri pilvipalvelussa, joka ei käytä varattu IP tai valitse toinen palautus-kohta, johon palauttaminen. | Ei mitään |
| Palauttaminen | Pilvipalvelussa syötteen päätepisteestä, kuinka monta enimmäismäärä on saavutettu - yritä määrittämällä eri pilvipalvelussa tai käyttämällä aiemmin luotu päätepiste. | Ei mitään |
| Palauttaminen | Varmuuskopion säilö ja kohde-tallennustilan tilin ovat kaksi eri alueilla – varmistaa, että tallennustilan tilin palautustoiminto-parametrissa on sama Azure alue kuin varmuuskopion säilö. | Ei mitään |
| Palauttaminen | Tallennustilan tilin palautustoiminto ei ole tuettu - vain Basic/Standard asiakkaat, joilla paikallisesti tarpeettomat tai geo tarpeettomat Replikointiasetukset tuetaan. Valitse tuettu tallennustilan-tili | Ei mitään |
| Palauttaminen | Tallennustilan tilin palautustoiminto määritetty tyyppi ei ole online - Varmista, että tallennustilan tilin palautustoiminto-parametrissa on online-tilassa | Tämä voi johtua vuoksi lyhytkestoisia virhe Azuren tallennustilaan tai käyttökatkosta vuoksi. Valitse jokin toinen tallennustilan tili. |
| Palauttaminen | Resurssiryhmä-kiintiön on saavutettu - Poista joitakin resurssiryhmiä Azure-portaalista tai Azure tuelta niin, että rajoituksia. | Ei mitään |
| Palauttaminen | Valitun aliverkon ei ole - Valitse aliverkon, joka on olemassa | Ei mitään |


## <a name="policy"></a>Käytäntö
| Toiminto | Virheen yksityiskohtaiset tiedot | Vaihtoehtoinen menetelmä |
| -------- | -------- | -------|
| Käytännön luominen | -Käytännön luominen epäonnistui Pienennä käytännön määritys Jatka säilytys-valinnat. | Ei mitään |


## <a name="vm-agent"></a>AM agentti

### <a name="setting-up-the-vm-agent"></a>AM edustajan määrittäminen
Yleensä AM-agentti on jo VMs, jotka on luotu Azure-valikoimasta. Kuitenkin näennäiskoneiden, jotka siirretään-paikallisen palvelinkeskusten ei ole asennettu AM agentti. Näiden VMs AM-agentti on asennettu erikseen. Lisätietoja [asentaminen aiemmin AM AM-agentti](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx).

For Windowsin VMs:

- Lataa ja asenna [MSI-agentti](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Sinun on järjestelmänvalvojan oikeudet ja viimeistele asennus.
- [AM-ominaisuuden päivittäminen](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) osoittamaan, että agentti on asennettu.

Saat Linux VMs:

- Asenna uusimmat [Linux agentti](https://github.com/Azure/WALinuxAgent) github.
- [AM-ominaisuuden päivittäminen](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) osoittamaan, että agentti on asennettu.


### <a name="updating-the-vm-agent"></a>Päivittäminen AM-agentti
For Windowsin VMs:

- Päivittäminen AM-agentti sujuu yhtä helposti kuin uudelleenasentaminen [AM agentti binaaritiedostot](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Sinun täytyy kuitenkin varmistaa, että mitään varmuuskopioinnin ei käytössä, kun AM-agentti päivitetä.

Saat Linux VMs:

- [Päivittäminen Linux AM agentti](../virtual-machines/virtual-machines-linux-update-agent.md)ohjeiden mukaisesti.


### <a name="validating-vm-agent-installation"></a>AM agentti asennuksen tarkistaminen
Voit tarkistaa Windowsin VMs AM Agent-versio:

1. Kirjaudu sisään Azure virtuaalikoneen ja Selaa kansioon, *C:\WindowsAzure\Packages*. Löydät tulisi esitä WaAppAgent.exe tiedoston.
2. Tiedostoa hiiren kakkospainikkeella, valitse **Ominaisuudet**ja valitse **tiedot** -välilehti. Tuoteversio-kentässä on oltava 2.6.1198.718 tai uudempi versio






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

## <a name="backup"></a>Varmuuskopiointi

| Varmuuskopiointi | Virheen yksityiskohtaiset tiedot | Vaihtoehtoinen menetelmä |
| -------- | -------- | -------|
|Varmuuskopiointi|    Toiminto ei voi suorittaa, kun AM ei enää ole. – Pysäytä suojaaminen virtuaalikoneen poistamatta palautettavat tiedot. Lisätietoja http://go.microsoft.com/fwlink/?LinkId=808124 etsiminen   |Tämä tapahtuu, kun ensisijainen AM poistetaan, mutta varmuuskopion käytäntö jatkaa Etsi AM haluat varmuuskopioida. Voit korjata virheen: <ol><li> Luo virtuaalikoneen sama nimi ja saman resurssin ryhmänimi [cloud palvelunimi]<br>(OR)</li><li> Lopeta suojaaminen virtuaalikoneen kanssa tai ilman poistaminen palautettavat tiedot. [Lisätietoja](http://go.microsoft.com/fwlink/?LinkId=808124)</li></ol>|
|Varmuuskopiointi|Tilannevedoksen tila AM-agentti ei voitu yhteyttä. -Varmista, että AM on internet-yhteyttä. Päivitä myös AM-agentti osoitteessa http://go.microsoft.com/fwlink/?LinkId=800034 vianmäärityksen oppaassa mainitut |Tämä virhe ilmenee, jos AM-agentti ongelma tai Azure infrastruktuurin verkkokäyttö on estetty jollakin tavalla. Lue lisätietoja virheenkorjaus määrittäminen AM tilannevedoksen ongelmat.<br> Jos AM-agentti ei ole aiheuttaa ongelmia, Käynnistä AM. Joskus AM virheellisessä tilassa voi aiheuttaa ongelmia ja uudelleen AM palauttaa tämän "virheellisessä tilassa"|
|Varmuuskopiointi|    Services-tunniste palauttaminen epäonnistui. -Varmista, että uusin virtuaalikoneen-agentti ei virtuaalikoneen ja agent-palvelu on käynnissä. Yritä varmuuskopiointia ja jos se ei onnistu, ota yhteys Microsoftin tuotetukeen.|   Tämä virhe ilmenee, kun AM-agentti ei ole ajan tasalla. Lisätietoja on alla Päivitä AM-agentti "päivitys AM Agent-osio.|
|Varmuuskopiointi |Virtuaalikoneen ei ole. -Varmista, että kyseisen virtuaalikoneen olemassa tai valitse virtual toiseen tietokoneeseen. | Tämä tapahtuu, kun ensisijainen AM poistetaan, mutta varmuuskopion käytäntö jatkaa Etsi AM haluat varmuuskopioida. Voit korjata virheen: <ol><li> Luo virtuaalikoneen sama nimi ja saman resurssin ryhmänimi [cloud palvelunimi]<br>(OR)<br></li><li>Lopeta suojaaminen virtuaalikoneen poistamatta palautettavat tiedot. [Lisätietoja](http://go.microsoft.com/fwlink/?LinkId=808124)</li></ol>|
|Varmuuskopiointi |Komennon suorittaminen epäonnistui. -Toinen toiminto on parhaillaan käynnissä kohteen. Odota, kunnes edellinen toiminto on valmis, ja yritä sitten uudelleen |On olemassa varmuuskopio tai AM palautustyön on käynnissä ja uusi projekti ei voi aloittaa, olemassa olevan projektin on käynnissä.|
| Varmuuskopiointi | Kopiointi näennäiskiintolevyjen varmuuskopion säilöstä aikakatkaisu - Yritä toiminnon muutaman minuutin kuluttua. Jos ongelma jatkuu, ota yhteyttä Microsoft Support. | Näin tapahtuu, kun kyseessä on liian paljon tietoja kopioidaan. Tarkista, onko alle 16 tietojen levyjä. |
| Varmuuskopiointi | Sisäinen virhe epäonnistui varmuuskopiointi - yritä toimintoa muutaman minuutin kuluttua. Jos ongelma jatkuu, ota yhteyttä Microsoft Support | Saat tämän virheen 2 syistä: <ol><li> Käytettäessä AM tallennustila on lyhytkestoisia ongelma. Tarkista onko kaikki jatkuvaa ongelman liittyvät suorittaminen ja tallennustilaa/verkon alueen [Azure tila](https://azure.microsoft.com/en-us/status/) . Ota joiden lievity uudelleen varmuuskopion kirjaa ongelman. <li>Alkuperäinen AM on poistettu ja näin ollen varmuuskopiointi ei voi ottaa. Varmuuskopiotiedot säilyttää poistetut AM, mutta lopettaa varmuuskopion virheet, poista AM ja valitse vaihtoehto, jos haluat säilyttää tiedot. Tämä estää varmuuskopioinnin aikataulu ja toistuvien virhesanomia. |
| Varmuuskopiointi | Ei pystynyt asentamaan Azure palautus-palveluiden valitulle kohteelle - AM agentti tunniste on vanhat tarvittavat Azure palautus palveluiden laajennuksen. Asenna Azure AM-agentti ja Käynnistä rekisteröinti-toiminto | <ol> <li>Tarkista, jos AM-agentti on asennettu oikein. <li>Varmista, että viestilipun, AM config on asetettu oikein.</ol> [Lue lisää](#validating-vm-agent-installation) tietoja AM agentti asennuksen ja asennuksen AM agentti. |
| Varmuuskopiointi | Tunniste-asennus epäonnistui virheen "COM + ei voi ottaa yhteyttä Microsoftin Distributed tapahtuman koordinaattien | Tämä yleensä tarkoittaa, että COM +-palvelu ei ole käynnissä. Saat lisätietoja ongelman korjaamisesta, ota yhteyttä Microsoftin tuotetukeen. |
| Varmuuskopiointi | Tilannevedoksen epäonnistui VSS toiminnon virheen "BitLocker-salaus lukittu aseman. Lukitus täytyy poistaa aseman Ohjauspaneelin kautta. | Kaikki asemista AM BitLocker käytöstä ja noudata Jos VSS ongelma on ratkaistu |
| Varmuuskopiointi | Näennäiskoneiden on virtual kiintolevyillä tallennetun Premium tallennustilan ei tueta varmuuskopiointi | Ei mitään |
| Varmuuskopiointi | Azure Virtual Machine ei löydy. | Tämä tapahtuu, kun ensisijainen AM poistetaan, mutta varmuuskopion käytäntö jatkaa Etsi AM haluat varmuuskopioida. Voit korjata virheen: <ol><li>Luo virtuaalikoneen sama nimi ja saman resurssin ryhmänimi [cloud palvelunimi] <br>(OR) <li> Tämä AM suojauksen käytöstä niin, että varmuuskopiointi töitä ei luoda </ol> |
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
| Palauttaminen | Palauttaminen epäonnistui Cloud sisäinen virhe | <ol><li>DNS-asetukset on määritetty, johon yrität palauttaa pilvipalvelussa. Voit tarkistaa <br>$deployment = get-AzureDeployment - palvelun nimi "palvelun nimi"-paikka "Tuotannon" Get-AzureDns - DnsSettings $deployment. DnsSettings<br>Jos näkyvissä on määritetty osoite, tämä tarkoittaa, että DNS-asetukset on määritetty.<br> <li>Pilvipalvelussa, johon haluat yrität palauttaa on määritetty ReservedIP ja aiemmin VMs pilvipalvelussa on pysäytetty-tilaan.<br>Voit tarkistaa pilvipalveluun on varattu IP käyttämällä seuraavia PowerShellin cmdlet-komennot:<br>$deployment = get-AzureDeployment - palvelun nimi "palvelun nimi"-paikka "Tuotannon" $estämistoiminnon ReservedIPName <br><li>Palauttaa saman pilvipalveluun virtual tietokoneessa, jossa seuraavat määräten verkon määritykset rooliin. <br>-Näennäiskoneiden kohdassa kuormituksen tasauspalvelun määritys (sisäinen ja ulkoinen)<br>-Näennäiskoneiden kanssa useita varattu IP-osoitteet<br>-Näennäiskoneiden useita NIC kanssa<br>Valitse uusi pilvipalvelussa Käyttöliittymän tai tutustu [palauttaa huomioon otettavia seikkoja](./backup-azure-arm-restore-vms.md/#restoring-vms-with-special-network-configurations) , VMs määräten verkon määritysten kanssa</ol> |
| Palauttaminen | Valitun DNS-nimi on varattu – Määritä eri DNS-nimi ja yritä uudelleen. | DNS-nimi tähän viittaa cloud palvelunimi (yleensä päättyen. cloudapp.net). Tämä on oltava yksilöllinen. Tämä virhe ilmenee, jos haluat valita eri AM nimellä palauttamisen aikana. <br><br> Huomaa, että tämä virhe näkyy Azure-portaalin käyttäjille. PowerShellin kautta palautustoiminto onnistuu, koska se vain palauttaa levyjä ja ei luoda AM. Virhe järjestelmässä AM erikseen luomisen itse levyn palautuksen jälkeen. |
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

## <a name="troubleshoot-vm-snapshot-issues"></a>AM tilannevedoksen ongelmien vianmääritys
AM varmuuskopiointi on riippuvainen varmenteiden tilannevedoksen komento pohjana olevan tallennustilan. Tallennustilan tai viiveen tallentamisessa tilannevedoksen tehtävän suorittaminen ei voi epäonnistua varmuuskopion. Näin voi aiheuttaa tilannevedoksen Tehtävävirhe.

1. Tallennustilan verkkokäyttö on estetty NSG käyttäminen<br>
   Lue lisää siitä, miten voit [sallia verkkokäytön](backup-azure-vms-prepare.md#2-network-connectivity) tallennustilan käyttämällä joko WhiteListing, IP-osoitteet tai välityspalvelimen kautta.
2.  Sql Server varmuuskopioimalla määritetty VMs heikentää tilannevedoksen tehtävän viive <br>
    Oletusarvoisesti AM varmuuskopioida ongelmat Windows VMs VSS täyden varmuuskopion. VMs, joissa on Sql Server-ja Sql Server on määritetty varmuuskopiointi-Tämä saattaa aiheuttaa viiveen tilannevedoksen suorittamisen. Määritä seuraava rekisteriavain, jos kohtaat varmuuskopioinnin virheiden tilannevedoksen ongelmien vuoksi.

    ```
    [HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT]
    "USEVSSCOPYBACKUP"="TRUE"
    ```
3.  AM tila näkyvissä virheellisesti, koska RDP AM suljetaan.  <br>
    Jos suljet on RDP-virtuaalikoneen, tarkista takaisin portaalin AM tila näkyvät oikein. Muussa tapauksessa Sammuta AM "Sammuta"-asetuksen avulla AM dashboard-portaalissa.
4.  Jos enintään neljä AM Jaa sama cloud palvelu, Määritä useita varmuuskopion käytäntöjä vaiheittainen varmuuskopion ajat niin ei enintään neljä AM varmuuskopioiden käynnistetään samanaikaisesti. Kokeile levittämällä varmuuskopion Käynnistä aikoja tunnin toisistaan käytännöt välillä. 
5.  AM on käytössä, suuri suorittimen ja muistin.<br>
    Jos virtuaalikoneen suoritetaan suuri suorittimen usage(>90%) tai muistin tilannevedoksen tehtävä on jonossa ja lykätä pöytäkoneesi seuraavan kerran saa aikakatkaistu. Kokeile tarvittaessa varmuuskopiointi Tällaisissa tilanteissa.

<br>

## <a name="networking"></a>Verkko
Kaikki tunnisteet, kuten varmuuskopiointi-tunniste on toimimaan julkisen Internet-yhteys. Ilman julkinen Internet-yhteys voit luettelon itse useilla eri tavoilla:

- Tunniste-asennus voi epäonnistua
- Varmuuskopion toiminnot (kuten levyn tilannevedoksen) voi epäonnistua
- Varmuuskopiointi tilan näyttäminen voi epäonnistua

Edellyttää ratkaiseminen julkinen internet-osoitteet on ollut nivelöity [tähän](http://blogs.msdn.com/b/mast/archive/2014/06/18/azure-vm-provisioning-stuck-on-quot-installing-extensions-on-virtual-machine-quot.aspx). Tarvitset Tarkista DNS-määrityksiä varten VNET ja varmista, että Azure-URI voidaan ratkaista.

Kun nimenselvitys tapahtuu oikein, access Azure IP-osoitteet on myös annettava. Voit poistaa eston pääsy Azure infrastruktuuri, tekemällä jommankumman seuraavista seuraavasti:

1. WhiteList Azure palvelinkeskuksen IP-alueita.
    - Pyydä [Azure palvelinkeskuksen IP-osoitteet](https://www.microsoft.com/download/details.aspx?id=41653) on whitelisted luettelo.
    - Salli IP-osoitteet, [Uusi NetRoute](https://technet.microsoft.com/library/hh826148.aspx) cmdlet-komennolla. Tämän cmdlet sisällä Azure-AM laajennettuja PowerShell-ikkunassa (Suorita järjestelmänvalvojana).
    - Lisätä sääntöjä NSG (Jos sinulla on paikallaan) annettavien IP-osoitteet.
2. Luo HTTP-liikenne paikalliseen flow liikerata
    - Jos sinulla on joitakin verkon rajoitus on lisätty (verkon käyttöoikeusryhmän, esimerkiksi) käyttöön HTTP-välityspalvelin ja haluat reitittää liikenteen. HTTP-välityspalvelin ottamaan vaiheet löytyy [tähän](backup-azure-vms-prepare.md#2-network-connectivity).
    - Lisätä sääntöjä NSG (Jos sinulla on paikallaan) Salli käyttää Internet-HTTP-välityspalvelin.

>[AZURE.NOTE] DHCP on otettava käyttöön IaaS AM varmuuskopiointi toimimaan Vierailijan sisällä.  Jos tarvitset staattinen yksityinen IP-kannattaa määrittää platform kautta. Asetusta AM sisällä tulisi olla käytössä vasemmalle.
Lisätietoja [asetusten staattinen Sisäinen yksityinen IP](../virtual-network/virtual-networks-reserved-private-ip.md)-näkymässä

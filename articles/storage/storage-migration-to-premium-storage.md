<properties
    pageTitle="Siirtyminen Azure Premium tallennustilan | Microsoft Azure"
    description="Siirtää aiemmin näennäiskoneiden Azure Premium-tallennustilan. Premium tallennustila on tehokas, pieni viive levyn I/O-paljon työmääriä käynnissä Azuren näennäiskoneiden-tuki."
    services="storage"
    documentationCenter="na"
    authors="aungoo-msft"
    manager="tadb"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/21/2016"
    ms.author="yuemlu"/>


# <a name="migrating-to-azure-premium-storage"></a>Azure Premium tallennustilan siirtyminen

## <a name="overview"></a>Yleiskatsaus

Azure Premium tallennustilan toimittaa tehokas, pieni viive levyn tuki näennäiskoneiden voin/O-paljon toiminnoista. Voit ottaa hyödyntää nopeuden ja näiden levyjen suorituskykyä valinnanvapaus sovelluksen AM levyjen Azure Premium tallennustilaan.

Tämän oppaan tarkoituksena on auttaa uusia käyttäjiä Azure Premium tallennustilaa paremmin Valmistaudu tekemään sujuva niiden nykyinen järjestelmästä Premium-tallennustilan. Oppaan korjaa kolme prosessia tärkeimmät osat: 

  - [Premium tallennustilan siirrosta suunnitteleminen](#plan-the-migration-to-premium-storage)
  - [Valmisteleminen ja kopioida Virtual kiintolevyillä (näennäiskiintolevyjen) Premium säilöön](#prepare-and-copy-virtual-hard-disks-VHDs-to-premium-storage)
  - [Luo Azure Virtual Machine Premium tallennustilan käyttäminen](#create-azure-virtual-machine-using-premium-storage)

Voit siirtää VMs muiden ympäristöjen Azure Premium tallennustilan tai siirtää aiemmin Azure VMs vakio tallennustilan Premium-tallennustilan. Tässä oppaassa kerrotaan vaiheet sekä seuraavissa skenaarioissa. Noudattamalla kohdassa oman toimintamallin määritetty.

>[AZURE.NOTE] Voit etsiä ominaisuutta-yleiskatsaus ja hinnoittelu Premium tallennustilaa Premium tallennustilaan: [Tehokas tallennustila Azure virtuaalikoneen toiminnoista](storage-premium-storage.md). Suosittelemme, että siirtyminen minkä tahansa virtuaalikoneen levyn edellyttävän hyvin IOPS Azure Premium säilöön parhaan mahdollisen suorituskyvyn sovelluksen. Jos levytilaa ei edellytä hyvin IOPS, voit rajoittaa kustannukset yllä vakio tallennustilaan, joka tallentaa virtuaalikoneen levyn tietojen kiintolevyaseman levyasemien (HDDs) SSDs sijaan.

Viimeistellään siirtoprosessia kokonaisuudessaan saattaa edellyttää muita toimintoja ennen ja jälkeen tässä oppaassa ohjeita. Esimerkkejä tällaisista tiedostoista ovat määrittäminen virtual verkkoja tai päätepisteet tai sovelluksesta joka saattaa edellyttää sovelluksen joitakin käyttökatkot koodin muutosten tekemistä. Nämä toiminnot ovat kunkin sovelluksen yksilöllisiä ja on suoritettava tässä oppaassa tehdä Premium tallennustilan koko siirtymän kuin saumaton mahdollisimman ohjeita sekä.


## <a name="plan-the-migration-to-premium-storage"></a>Premium tallennustilan siirrosta suunnitteleminen

Tässä osassa varmistaa siirron noudattamalla tämän artikkelin haluat ja avulla voit tehdä parhaat päätös AM ja levy.

### <a name="prerequisites"></a>Edellytykset
- Sinun on Azure tilauksen. Jos sinulla ei ole, voit luoda yhden kuukauden [maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/) tilauksen tai siirry [Azure hinnat](https://azure.microsoft.com/pricing/) saat lisää vaihtoehtoja.
- PowerShell cmdlet-komentojen suorittamiseen tarvitset Microsoft Azure PowerShell-moduulin. Katso, [miten asennetaan ja määritetään PowerShellin Azure](../powershell-install-configure.md) , piste- ja asennusongelmien asennusohjeet.
- Jos aiot käyttää Azure virtuaalilaitteiksi Premium tallennustilaa, sinun on käytettävä Premium tallennustilan pystyvät VMs. Voit käyttää vakio- ja Premium tallennustilan levyjen Premium tallennustilan pystyvät VMs. Premium tallennustilan levyjen on käytettävissä Lisää käyttämällä AM tulevaisuudessa. Lisätietoja käytettävissä Azure AM levyn tyyppi ja koko-kohdassa [näennäiskoneiden koot](../virtual-machines/virtual-machines-windows-sizes.md) ja [koot pilvipalveluihin](../cloud-services/cloud-services-sizes-specs.md).

### <a name="considerations"></a>Huomioon otettavia seikkoja

Azure-AM tukee liittäminen useille Premium tallennustilan levyille niin, että sovelluksesi voi olla enintään 64 TT tallennustilaa AM kohden. Sovellusten toteuttaa Premium tallennus-ja toinen levy-siirtonopeuden kohti AM kanssa erittäin pienen viiveitä luku toimille kohden 80,000 IOPS (i/toimintoja sekunnissa) AM ja 2000 Mt kohden. Käytettävissä on erilaisia VMs ja levyjen vaihtoehtoista. Tässä osassa on avulla voit etsiä havainnollistamiseen parhaiten sopiva vaihtoehto.

#### <a name="vm-sizes"></a>AM koot
[Koot näennäiskoneiden](../virtual-machines/virtual-machines-windows-sizes.md)on lueteltu Azure AM koon määritykset. Voit tarkastella, käsitellä Premium tallennustilan ja valita sopivimman AM koon, joka vastaa parhaiten havainnollistamiseen näennäiskoneiden suorituskyvyn ominaisuudet. Varmista, että ole riittäviä kaistanleveyden käytettävissä oman AM asema levy-liikenne.


#### <a name="disk-sizes"></a>Levyn koot
On kolmenlaisia levyjä, jotka voidaan käyttää käyttäjän AM ja jokaisella tietyn IOPs ja siirtonopeuden rajoitukset. Ota huomioon nämä raja-arvot levy valitseminen oman AM perusteella sovelluksesi kannalta kapasiteettia, suorituskyky ja skaalattavuus tarpeisiin ja piikin Lataa.

|Levyn Premium tallennustyyppi|P10|Ñ20 =|P30|
|:---:|:---:|:---:|:---:|
|Levyn koko|128 GT|512 GT|1 024 GIGATAVUA (1 TT)|
|IOPS levyä kohden|500|2300|5000|
|Siirtonopeuden levyä kohden|100 Mt sekunnissa|150 Megatavua sekunnissa|200 Mt sekunnissa|

Määrittää havainnollistamiseen, mukaan, jos tiedot levyjen tarvitaan oman AM. Voit liittää useita pysyvien tietojen levyjen oman AM. Tarvittaessa voit stripe niin, että valmiuksien ja suorituskyvyn säätö levyille. (Mikä on levyn raitasarjoittamista [tätä](storage-premium-storage-performance.md#disk-striping).) Jos olet raita Premium tallennustilan tietojen levyjen [Tallennustilan välilyöntien]avulla[4], sinun kannattaa määrittää yhden sarakkeen kunkin levyn, jota käytetään. Muussa tapauksessa mustat äänenvoimakkuuden suorituskykyyn voi heiketä odotettua vuoksi liikenteen erimittainen jakautuminen levyille. Linux VMs, voit toteuttaa saman *mdadm* -apuohjelman avulla. Artikkelissa [Määrittäminen ohjelmiston RAID Linux](../virtual-machines/virtual-machines-linux-configure-raid.md) lisätietoja.

#### <a name="storage-account-scalability-targets"></a>Tallennustilan tilin skaalattavuus kohteet
Premium tallennustilan-tileillä on seuraavat skaalattavuus tavoitteet, [Azure-tallennustilan skaalattavuus ja suorituskyvyn kohteet](storage-scalability-targets.md). Jos sovelluksen tarpeen ylittävät skaalattavuus kohteet yhteen tallennustilan tilin, muodosta sovelluksesi voi käyttää tallennustilan useita tilejä ja osion tietojen tallennustilan näiden tilien välillä.

|Yhteensä kapasiteetin|Yhteensä kaistanleveyden paikallisesti tarpeettomat tallennustilan tilin|
|:--|:---|
|Levyn kapasiteetin: 35 TT<br />Tilannevedoksen kapasiteetin: 10 TT|Enintään 50 gigabits saapuvien + lähtevä sekunnissa|

Tutustu Premium tallennustilan määritykset, Lisätietoja on [skaalattavuus ja suorituskyvyn kohteet Premium tallennustilan käytettäessä](storage-premium-storage.md#scalability-and-performance-targets-when-using-premium-storage).

#### <a name="disk-caching-policy"></a>Käytännön välimuistiin tallentaminen levylle
Oletusarvon mukaan levyn välimuistiin käytäntö on *Vain luku -* Premium tietojen levyille ja *Luku-ja kirjoitusoikeudet* Premium käyttöjärjestelmän levyn liitetty AM. Sovelluksen iOS optimaalisen saavuttamiseksi suositellaan määritys-asetusta. Tietojen kirjoittaminen paksu tai vain kirjoitus-levyjen (esimerkiksi SQL Serverin lokitiedostot) Poista käytöstä levyn välimuistiin niin, että voit toteuttaa sovelluksen suorituskyvyn parantamiseksi. Aiemmin luotuja tietoja levyjen välimuistiasetukset voidaan päivittää [Azure kautta](https://portal.azure.com) tai *- HostCaching* parametrin *Määrittäminen AzureDataDisk* cmdlet-komennon avulla.

#### <a name="location"></a>Sijainti
Valitse sijainti, johon Azure Premium tallennustila on käytettävissä. Katso [Azure Services alueen mukaan](https://azure.microsoft.com/regions/#services) käytettävissä olevat sijainnit-tiedot. VMs saman alueella, tallentaa AM levyjen avulla paljon parempi suorituskyky kuin jos ne ovat eri alueilla tallennustilan tilinä.

#### <a name="other-azure-vm-configuration-settings"></a>Muut Azure AM asetukset
Luodessasi Azure-AM, sinua pyydetään tiettyjä AM asetusten määrittäminen. Muista, että joitakin asetuksia on vahvistettu, AM elinkaaren aikana voit muokata tai lisätä muiden myöhemmin. Tarkista nämä Azure AM asetukset ja varmista, että ne on määritetty oikein työmäärää tarpeiden.

### <a name="optimization"></a>Optimointi

[Azure Premium tallennustilan: rakenne suorituskyvyn](storage-premium-storage-performance.md) tehokas sovellusten Azure Premium tallennustilan avulla ohjeita. Voit seurata suorituskykyä koskevat sovelluksen käyttämät tekniikat parhaita käytäntöjä yhdistettynä ohjeita.


## <a name="prepare-and-copy-virtual-hard-disks-VHDs-to-premium-storage"></a>Valmisteleminen ja kopioida virtual kiintolevyillä (näennäiskiintolevyjen) Premium säilöön

Seuraavassa osassa on ohjeet valmistellaan näennäiskiintolevyjen oman AM- ja kopioimalla näennäiskiintolevyjen Azure-tallennustilan.

- [Tapaus 1: "I 'M siirtämisestä aiemmin Azure VMs Azure Premium-tallennustilan."](#scenario1)
- [Tapaus 2: "I 'M siirtyminen VMs muiden ympäristöjen Azure Premium-tallennustilan."](#scenario2)

### <a name="prerequisites"></a>Edellytykset

Valmistele näennäiskiintolevyt siirtoa varten, tarvitset:

- Azure tilauksen tallennustilan tilin ja säilön, johon voit kopioida oman Näennäiskiintolevyn tallennustilan kyseisessä tilissä. Huomaa, että kohde-tallennustilan tilin voi Standard tai Premium-tallennustilan tilin oman tarpeen mukaan.
- Työkalun generalize Näennäiskiintolevyn, jos aiot luoda AM useita kertoja. Esimerkiksi sysprep Windows-tai Sysprepin Ubuntu virt.
- Työkalu, voit ladata Näennäiskiintolevytiedosto tallennustilan tilin. [Siirrä tiedot AzCopy komentorivivalitsimet-apuohjelma ja](storage-use-azcopy.md) Katso tai käytä [Azure tallennustilan explorer](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx). Tässä oppaassa kerrotaan kopioiminen oman Näennäiskiintolevyn AzCopy-työkalun avulla.

> [AZURE.NOTE] Jos valitset synkronoitu kopio-asetus, jossa AzCopy, paras tulos saavutetaan, kopioi oman Näennäiskiintolevyn suorittamalla jompikumpi näistä työkaluista, joka on saman alueen kohde tallennustilan tiliksi Azure-AM. Jos haluat kopioida Näennäiskiintolevyn Azure-AM toisella alueella, suorituskyvyn voi hidastua.
>
> Kopioimiseen suuria määriä tietoa rajoitettu kaistanleveys päälle, voit [siirtää tietoja Blob-objektien tallennustilaan Azure Tuo/Vie-palvelun avulla](storage-import-export-service.md); Voit siirtää tietoja lähetyksen kiintolevyaseman levyasemien Azure palvelinkeskukseen. Azure Tuo/Vie-palvelun avulla voit kopioida vain vakio tallennustilan tilin tiedot. Kun tiedot on vakio tallennustilan-tilillä, voit [Kopioida Blob API](https://msdn.microsoft.com/library/azure/dd894037.aspx) tai AzCopy siirtämään premium-tallennustilan tilin tiedot.
>
> Huomaa, että Microsoft Azure tukee vain kiinteän kokoinen näennäiskiintolevytiedostoja. VHDX tiedostoja tai dynaaminen näennäiskiintolevyjen ei tueta. Jos sinulla on dynaamisen Näennäiskiintolevyn, voit muuntaa sen kiinteän kokoinen, [Muunna Näennäiskiintolevyn](http://technet.microsoft.com/library/hh848454.aspx) cmdlet-komennolla.

### <a name="scenario1"></a>Tapaus 1: "I 'M siirtämisestä aiemmin Azure VMs Azure Premium-tallennustilan."

Jos olet siirtymässä aiemmin Azure VMs, Lopeta AM, valmisteleminen näennäiskiintolevyjen kohti Näennäiskiintolevyn haluamasi tyypin ja kopioi Näennäiskiintolevyn AzCopy tai PowerShell.

AM on oltava kokonaan alaspäin siirtää SIIVOA tila. Ole käyttökatkot kunnes siirto on valmis.

#### <a name="step-1-prepare-vhds-for-migration"></a>Vaihe 1. Näennäiskiintolevyjen siirtoon valmistautuminen

Jos olet siirtymässä aiemmin Azure VMs Premium tallennustilaa, että Näennäiskiintolevyn voi olla:

- Generalized käyttöjärjestelmä-kuva
- Yksilöivä käyttöjärjestelmän DVD-levyllä
- Tietoja DVD-levyllä

Alla selkeät 3 tilanteista valmistelemiseksi oman Näennäiskiintolevyn.

##### <a name="use-a-generalized-operating-system-vhd-to-create-multiple-vm-instances"></a>Generalized käyttöjärjestelmän Näennäiskiintolevyn avulla voit luoda useita AM esiintymiä

Jos lataat Näennäiskiintolevyn, jota käytetään luomaan useita yleisiä Azure AM esiintymät, yleistetään on ensin Näennäiskiintolevyn sysprep-apuohjelmalla. Tämä koskee Näennäiskiintolevyn, joka on paikallisen tai pilveen. Sysprep poistaa minkä tahansa tietokoneen koskevien tietojen syöttölomake Näennäiskiintolevyn.

>[AZURE.IMPORTANT] Ota tilannevedos tai oman AM varmuuskopioida ennen joilla se. Käynnissä sysprep Pysäytä ja poistaa varauksen AM esiintymä. Noudata alla olevia ohjeita sysprep Windows-käyttöjärjestelmän Näennäiskiintolevyn. Huomaa, että Sysprep-komennon suorittaminen edellyttää virtuaalikoneen Sammuta. Katso lisätietoja Sysprep [Sysprep yleiskatsaus](http://technet.microsoft.com/library/hh825209.aspx) tai [Sysprepin tekniset tiedot](http://technet.microsoft.com/library/cc766049.aspx).

1. Avaa komentokehoteikkuna järjestelmänvalvojana.
2. Kirjoita seuraava komento Avaa Sysprep:

        %windir%\system32\sysprep\sysprep.exe

3. Järjestelmän valmistelu työkalun, valitse Kirjoita ruutuun esittelyyn (Tervetuloa-ohjelma), yleistä-valintaruutu, valitse **sulkeminen**ja valitse sitten **OK**, alla olevassa kuvassa esitetyllä tavalla. Sysprep generalize käyttöjärjestelmän ja sulje.

    ![][1]

Käytä Ubuntu-AM, virt sysprep saada samat. Katso lisätietoja [virt sysprep](http://manpages.ubuntu.com/manpages/precise/man1/virt-sysprep.1.html) . Tutustu myös Avaa lähde [Linux palvelimen valmistelu ohjelmiston](http://www.cyberciti.biz/tips/server-provisioning-software.html) Linux-käyttöjärjestelmissä.

##### <a name="use-a-unique-operating-system-vhd-to-create-a-single-vm-instance"></a>Yksilöivä käyttöjärjestelmän Näennäiskiintolevyn avulla voit luoda yksittäisen AM-esiintymä

Jos sinulla on sovelluksen, joka edellyttää tietokoneen tietyt tiedot AM käytössä, ei generalize Näennäiskiintolevyn. Generalized Näennäiskiintolevyn voidaan luoda yksilölliset Azure AM-esiintymä. Esimerkiksi jos olet määrittänyt toimialueen ohjauskoneen oman Näennäiskiintolevyn, suoritetaan sysprep tekee sen kelvoton toimialueen ohjauskoneen nimellä. Tarkista oman AM ja vaikutus suoritat Sysprepin ne ennen joilla Näennäiskiintolevyn sovellusten.

##### <a name="register-data-disk-vhd"></a>Rekisteröi tietojen levyn Näennäiskiintolevyn

Jos sinulla on tietoja levyjen Azure uuteen, on tehtävä, jotka käyttävät näitä tietoja levyjen VMs Sammuta.

Noudata alla kuvatun kopioi Näennäiskiintolevyn Azure Premium tallennustilan ja rekisteröi se valmistellun tietojen DVD-levyllä.

#### <a name="step-2-create-the-destination-for-your-vhd"></a>Vaihe 2. Luoda oman Näennäiskiintolevyn kohde

Tallennustilan tilin ylläpidon oman Näennäiskiintolevyjen luominen. Kun suunnittelet tallennuspaikan oman näennäiskiintolevyjen, ota huomioon seuraavat seikat:

- Kohde Premium-tallennustilan tilin.
- Tilin tallennuspaikka on oltava sama kuin Premium tallennustilan pystyvät Azure VMs luot viimeisessä vaiheessa. Voi kopioida uusi tallennustilan tili tai aiot käyttää samaa käyttötarkoitukseen tallennustilan tiliä.
- Kopioi ja Tallenna kohde-tallennustilan tilin tallennustilan tilin-avain seuraavaan vaiheeseen.

Tietojen levyjä voit säilyttää joidenkin tietojen levyjen vakio tallennustilan tilillä (esimerkiksi levyjä, joiden jäähdytyslaite tallennustilan), mutta suositeltavaa voit tuotannon kuormituksen premium tallennusvälineiden kaikkien tietojen siirtämistä.

#### <a name="copy-vhd-with-azcopy-or-powershell"></a>Vaihe 3. Kopioi Näennäiskiintolevyn AzCopy tai PowerShell

Sinun on säilön polku ja tallennustilaa tilin key-tuotetunnuksen etsiminen käsittelemään jompikumpi näistä vaihtoehdoista. Säilön polku ja tallennustilaa tilin avain löytyy **Azure**-portaalissa > **tallennustilan**. Säilön URL-osoite on esimerkiksi "https://myaccount.blob.core.windows.net/mycontainer/".

##### <a name="option-1-copy-a-vhd-with-azcopy-asynchronous-copy"></a>Vaihtoehto 1: Kopioi Näennäiskiintolevyn AzCopy (asynkroninen kopio) kanssa

Käytä AzCopy, voit helposti ladata Näennäiskiintolevyn Internetin välityksellä. Mukaan näennäiskiintolevyt suuri Tämä voi viedä jonkin aikaa. Muista tarkistaa tilin tunkeutumisen/juniin tallennustilarajojen, kun tämä vaihtoehto. Lisätietoja [Azure-tallennustilan skaalattavuus ja suorituskyvyn kohteet](storage-scalability-targets.md) .

1. Lataa ja asenna AzCopy täällä: [AzCopy uusin versio](http://aka.ms/downloadazcopy)
2. Avaa PowerShellin Azure ja siirry kansioon, johon on asennettu AzCopy.
3. Seuraavalla komennolla kopioi Näennäiskiintolevytiedosto "Lähteestä" "Kohde".

        AzCopy /Source: <source> /SourceKey: <source-account-key> /Dest: <destination> /DestKey: <dest-account-key> /BlobType:page /Pattern: <file-name>

    Esimerkki:

        AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /SourceKey:key1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /DestKey:key2 /Pattern:abc.vhd

    Seuraavassa on kuvaukset parametrit, joita käytetään AzCopy-komennolla:

 - * */Lähde: * &lt;lähteen&gt;:*** tallennustilan säilö URL-Osoitetta, joka sisältää Näennäiskiintolevyn tai kansion sijainti.
 - * */SourceKey: * &lt;lähde-tili-avain&gt;:*** tallennustilan tilin avain lähde-tallennustilan tilin.
 - * */Dest: * &lt;kohde&gt;:*** tallennustilan säilö URL-Osoitetta, kopioi Näennäiskiintolevyn avulla.
 - * */DestKey: * &lt;kohde-tili-avain&gt;:*** tallennustilan tilin avain kohde-tallennustilan tilin.
 - * */Mallia: * &lt;tiedostonimi&gt;:*** Näennäiskiintolevyn kopioi tiedoston nimi.

Lisätietoja AzCopy-työkalun käyttämisestä on artikkelissa [tiedonsiirron AzCopy komentorivivalitsimet-apuohjelman avulla](storage-use-azcopy.md).

##### <a name="option-2-copy-a-vhd-with-powershell-synchronized-copy"></a>Vaihtoehto 2: Kopioi Näennäiskiintolevyn PowerShellin (Synchronized kopio)

Voit myös kopioida PowerShell-cmdlet-komennolla Käynnistä AzureStorageBlobCopy Näennäiskiintolevytiedosto. PowerShellin Azure seuraavan komennon avulla voit kopioida Näennäiskiintolevyn. Korvaa arvot <> vastaavien arvojen lähde- ja tallennustilaa tililtä. Voit käyttää tätä komentoa, sinulla on oltava kutsutaan näennäiskiintolevyjen tilisi kohde tallennustilan säilö. Jos säilö ei ole, Luo eteen-komennon suorittaminen.

    $sourceBlobUri = <source-vhd-uri>

    $sourceContext = New-AzureStorageContext  –StorageAccountName <source-account> -StorageAccountKey <source-account-key>

    $destinationContext = New-AzureStorageContext  –StorageAccountName <dest-account> -StorageAccountKey <dest-account-key>

    Start-AzureStorageBlobCopy -srcUri $sourceBlobUri -SrcContext $sourceContext -DestContainer <dest-container> -DestBlob <dest-disk-name> -DestContext $destinationContext

Esimerkki:

    C:\PS> $sourceBlobUri = "https://sourceaccount.blob.core.windows.net/vhds/myvhd.vhd"

    C:\PS> $sourceContext = New-AzureStorageContext  –StorageAccountName "sourceaccount" -StorageAccountKey "J4zUI9T5b8gvHohkiRg"

    C:\PS> $destinationContext = New-AzureStorageContext  –StorageAccountName "destaccount" -StorageAccountKey "XZTmqSGKUYFSh7zB5"

    C:\PS> Start-AzureStorageBlobCopy -srcUri $sourceBlobUri -SrcContext $sourceContext -DestContainer "vhds" -DestBlob "myvhd.vhd" -DestContext $destinationContext

### <a name="scenario2"></a>Tapaus 2: "I 'M siirtyminen VMs muiden ympäristöjen Azure Premium-tallennustilan."

Jos olet siirtymässä Näennäiskiintolevyn-ei - Azure Pilvitallennustilaa Azure, kun olet vienyt Näennäiskiintolevyn paikallisessa kansiossa. On valmis lähteen polku paikallisen kansion, johon Näennäiskiintolevyn on tallennettu kätevää ja lataaminen Azuren tallennustilaan AzCopy avulla.

#### <a name="step-1-export-vhd-to-a-local-directory"></a>Vaihe 1. Vie Näennäiskiintolevyn paikallisessa kansiossa

##### <a name="copy-a-vhd-from-aws"></a>Kopioi Näennäiskiintolevyn sisältyy AWS

1. Jos käytössäsi on sisältyy AWS, viedä EC2 esiintymä-Amazon S3 aikajakson Näennäiskiintolevyn. Noudata vieminen Amazon EC2 esiintymät Amazon EC2 käyttöliittymä (CLI)-työkalun asentaminen ja suorittaminen Vie EC2 esiintymän .vhd-tiedoston luominen-esiintymä-vienti-tehtävä-komento Amazon ohjeissa kuvatut vaiheet. Varmista, että Käytä **Näennäiskiintolevyn** levyn & #95; KUVA & #95; Muotoile-muuttuja, kun **luominen-esiintymä-vienti-tehtävä** -komennon suorittaminen. Viedyt Näennäiskiintolevytiedosto tallennetaan Amazon S3 aikajakson, voit määrittää prosessin aikana.

        aws ec2 create-instance-export-task --instance-id ID --target-environment TARGET_ENVIRONMENT \
        --export-to-s3-task DiskImageFormat=DISK_IMAGE_FORMAT,ContainerFormat=ova,S3Bucket=BUCKET,S3Prefix=PREFIX

2. Lataa Näennäiskiintolevytiedosto S3 aikajakson. Valitse Näennäiskiintolevytiedosto, valitse **Toiminnot** > **Lataa**.

    ![][3]

##### <a name="copy-a-vhd-from-other-non-azure-cloud"></a>Kopioi Näennäiskiintolevyn-Azure muiden pilvestä

Jos olet siirtymässä Näennäiskiintolevyn-ei - Azure Pilvitallennustilaa Azure, kun olet vienyt Näennäiskiintolevyn paikallisessa kansiossa. Kopioi Näennäiskiintolevyn tallennuspaikan paikallisen kansion polun valmis lähde.

##### <a name="copy-a-vhd-from-on-premise"></a>Kopioi Näennäiskiintolevyn paikallinen

Jos olet siirtymässä Näennäiskiintolevyn paikallinen ympäristön, sinun on valmis lähdepolku Näennäiskiintolevyn tallennuspaikan. Lähdepolku voi olla palvelimen sijainnin tai tiedoston Jaa.

#### <a name="step-2-create-the-destination-for-your-vhd"></a>Vaihe 2. Luoda oman Näennäiskiintolevyn kohde

Tallennustilan tilin ylläpidon oman Näennäiskiintolevyjen luominen. Kun suunnittelet tallennuspaikan oman näennäiskiintolevyjen, ota huomioon seuraavat seikat:

- Kohde-tallennustilan tilin voi olla vakio tai maksullisten tallennustilan sovelluksen tarpeen mukaan.
- Tallennustilan tilin alue on oltava sama kuin Premium tallennustilan pystyvät Azure VMs luot viimeisessä vaiheessa. Voi kopioida uusi tallennustilan tili tai aiot käyttää samaa käyttötarkoitukseen tallennustilan tiliä.
- Kopioi ja Tallenna kohde-tallennustilan tilin tallennustilan tilin-avain seuraavaan vaiheeseen.

On erittäin suositeltavaa voit tuotannon kuormituksen premium tallennusvälineiden kaikkien tietojen siirtämistä.

#### <a name="step-3-upload-the-vhd-to-azure-storage"></a>Vaihe 3. Lataa Näennäiskiintolevyn Azure-tallennustilan

Nyt kun olet luonut oman Näennäiskiintolevyn paikallisessa kansiossa, voit käyttää AzCopy tai AzurePowerShell .vhd-tiedoston lataaminen Azure-tallennustilan. Molemmat vaihtoehdot on annettu tähän:

##### <a name="option-1-using-azure-powershell-add-azurevhd-to-upload-the-vhd-file"></a>Vaihtoehto 1: Lataa .vhd tiedosto Azure PowerShell lisää-AzureVhd avulla

    Add-AzureVhd [-Destination] <Uri> [-LocalFilePath] <FileInfo>

Esimerkki <Uri> voi olla **_"https://storagesample.blob.core.windows.net/mycontainer/blob1.vhd"_**. Esimerkki <FileInfo> voi olla **_"C:\path\to\upload.vhd"_**.

##### <a name="option-2-using-azcopy-to-upload-the-vhd-file"></a>Vaihtoehto 2: Käyttämällä AzCopy .vhd-tiedoston lataaminen

Käytä AzCopy, voit helposti ladata Näennäiskiintolevyn Internetin välityksellä. Mukaan näennäiskiintolevyt suuri Tämä voi viedä jonkin aikaa. Muista tarkistaa tilin tunkeutumisen/juniin tallennustilarajojen, kun tämä vaihtoehto. Lisätietoja [Azure-tallennustilan skaalattavuus ja suorituskyvyn kohteet](storage-scalability-targets.md) .

1. Lataa ja asenna AzCopy täällä: [AzCopy uusin versio](http://aka.ms/downloadazcopy)
2. Avaa PowerShellin Azure ja siirry kansioon, johon on asennettu AzCopy.
3. Seuraavalla komennolla kopioi Näennäiskiintolevytiedosto "Lähteestä" "Kohde".

        AzCopy /Source: <source> /SourceKey: <source-account-key> /Dest: <destination> /DestKey: <dest-account-key> /BlobType:page /Pattern: <file-name>

    Esimerkki:

        AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /SourceKey:key1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /DestKey:key2 /BlobType:page /Pattern:abc.vhd

    Seuraavassa on kuvaukset parametrit, joita käytetään AzCopy-komennolla:

 - * */Lähde: * &lt;lähteen&gt;:*** tallennustilan säilö URL-Osoitetta, joka sisältää Näennäiskiintolevyn tai kansion sijainti.
 - * */SourceKey: * &lt;lähde-tili-avain&gt;:*** tallennustilan tilin avain lähde-tallennustilan tilin.
 - * */Dest: * &lt;kohde&gt;:*** tallennustilan säilö URL-Osoitetta, kopioi Näennäiskiintolevyn avulla.
 - * */DestKey: * &lt;kohde-tili-avain&gt;:*** tallennustilan tilin avain kohde-tallennustilan tilin.
 - **/BlobType: sivun:** Määrittää, että kohde on sivun Blob-objektien.
 - * */Kuviota: * &lt;tiedostonimi&gt;:*** Näennäiskiintolevyn kopioi tiedoston nimi.

Lisätietoja AzCopy-työkalun käyttämisestä on artikkelissa [tiedonsiirron AzCopy komentorivivalitsimet-apuohjelman avulla](storage-use-azcopy.md).

##### <a name="other-options-for-uploading-a-vhd"></a>Muut asetukset ladataan Näennäiskiintolevyn

Voit ladata Näennäiskiintolevyn myös tallennustilan tilin jollakin seuraavista tavoista:

- [Azure-tallennustilan kopioi Blob Ohjelmointirajapinta](https://msdn.microsoft.com/library/azure/dd894037.aspx)
- [Azure-tallennustilan ladataan BLOB](https://azurestorageexplorer.codeplex.com/)
- [Tallennustilan Tuo/Vie palvelun REST API-viittaus](https://msdn.microsoft.com/library/dn529096.aspx)

>[AZURE.NOTE] On suositeltavaa käyttämällä Tuo/Vie-palvelua, jos arvioitu ladataan aika on yli seitsemän päivää. [DataTransferSpeedCalculator](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html) avulla voit arvioida tiedot koon ja siirron yksikkö ajasta.
>
> Tuo tai Vie avulla voidaan kopioida vakio tallennustilan tilin. Sinun on kopioimisesta vakio tallennustilan premium tallennustilan tilin, kuten AzCopy-työkalun avulla.


## <a name="create-azure-virtual-machine-using-premium-storage"></a>Luo Azure VMs Premium tallennustilan käyttäminen

Kun Näennäiskiintolevyn siirtää tai kopioida haluamasi tallennustilan-tilille, voit rekisteröidä Näennäiskiintolevyn OS kuvan tai oman toimintamallin OS levyn ja luo sitten siitä AM esiintymän ohjeiden mukaisesti. Tietoja levyn Näennäiskiintolevyn voidaan liittää AM sen luomisen jälkeen. Esimerkki siirto-komentosarja on annettu tämän osan loppuun. Tämän yksinkertaisen komentosarjan vastaa kaikissa tilanteissa. Saatat joutua päivittämään komentosarja vastaamaan ja tietyn skenaario. Jos haluat nähdä, koskeeko tämä komentosarja käyttämässäsi skenaariossa, noudata seuraavia ohjeita [A esimerkki siirron komentosarja](#a-sample-migration-script).

### <a name="checklist"></a>Tarkistusluettelo

1.  Odota, kunnes kaikki kopioiminen Näennäiskiintolevyn levyjen on valmis.
2.  Varmista, että Premium-tallennustila on käytettävissä, olet siirtymässä alueen.
3.  Päätä käytät uudet AM arvosarjat. Luultavasti Premium-tallennustilan pystyvät ja koko sen alueen käytettävyyden mukaan ja käyttötarkoitukseen.
4.  Päätä käytät tarkka AM koko. AM koon on oltava riittävän suuri tukemaan on tietoja levyjen määrä. Esimerkiksi Jos sinulla on 4 tietojen levyjä, AM on oltava vähintään 2 sydämiä. Voit myös suoritin, muistin ja kaistanleveys on.
5.  Premium-tallennustilan tilin luominen kohde-alueella. Tämä on uusi AM käytettävän tilin.
6.  On kätevä, mukaan lukien levyille ja vastaavan Näennäiskiintolevyn BLOB nykyisen AM tiedot.

Valmistele sovelluksesi käyttökatkot varten. SIIVOA siirto tapahtuu on kaikki käsittelyn lopettaminen nykyisen järjestelmän. Vasta sen jälkeen voit siirtyä siihen yhdenmukaisia vaiheeseen, joka voi siirtää uudessa ympäristössä. Käyttökatkot kesto riippuu levyä, jos haluat siirtää tietojen määrää.

>[AZURE.NOTE] Jos olet luomassa Azure-Resurssienhallinta AM erityinen Näennäiskiintolevyn levyltä, tutustu [tämän mallin](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd) käyttöönoton Resurssienhallinta AM käyttämällä nykyistä levyä.

### <a name="register-your-vhd"></a>Rekisteröi oman Näennäiskiintolevyn

AM luominen OS Näennäiskiintolevyn tai liittää tietoja DVD-levyllä uuden AM, sinun on rekisteröitävä ne ensin. Noudata alla olevia ohjeita oman Näennäiskiintolevyn toimintamallin.

#### <a name="generalized-operating-system-vhd-to-create-multiple-azure-vm-instances"></a>Generalized käyttöjärjestelmän Näennäiskiintolevyn luominen Azure AM useita kertoja

Kun generalized OS kuva Näennäiskiintolevyn on ladattu tallennustilan-tilille, Rekisteröi se **Azure AM kuvan** niin, että voit luoda yhden tai useamman AM esiintymät siitä. PowerShellin cmdlet-komennot avulla voit rekisteröidä oman Näennäiskiintolevyn Azure AM OS kuvana. Määrittää missä Näennäiskiintolevyn on kopioitu valmis säilö URL-Osoitteen.

    Add-AzureVMImage -ImageName "OSImageName" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/osimage.vhd" -OS Windows

Kopioi ja Tallenna tämä uusi Azure AM kuvan nimen. Yllä olevassa esimerkissä on *OSImageName*.

#### <a name="unique-operating-system-vhd-to-create-a-single-azure-vm-instance"></a>Yksilöivä käyttöjärjestelmän Näennäiskiintolevyn luominen Azure AM esiintymiä

Kun yksilöllinen OS Näennäiskiintolevyn on ladattu tallennustilan-tilille, Rekisteröi se **Azure OS levyn** niin, että voit luoda sen AM esiintymä. Rekisteröi oman Näennäiskiintolevyn Azure-OS levyn nämä PowerShell cmdlet-komentojen avulla. Määrittää missä Näennäiskiintolevyn on kopioitu valmis säilö URL-Osoitteen.

    Add-AzureDisk -DiskName "OSDisk" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd" -Label "My OS Disk" -OS "Windows"

Kopioida ja tallentaa uuden Azure OS levyn nimi. Yllä olevassa esimerkissä on *OSDisk*.

#### <a name="data-disk-vhd-to-be-attached-to-new-azure-vm-instances"></a>Tietoja levyn Näennäiskiintolevyn liitetään uusi Azure AM esiintymää

Kun tietojen levyn Näennäiskiintolevyn on ladattu tallennustilan tilin, Rekisteröi se Azure-tietoihin levyn niin, että työnkulku voidaan liittää uusi DS arvosarjan arvoja, DSv2 sarjan tai GS sarjan Azure AM esiintymä.

Rekisteröi oman Näennäiskiintolevyn Azure-tietoihin levyn nämä PowerShell cmdlet-komentojen avulla. Määrittää missä Näennäiskiintolevyn on kopioitu valmis säilö URL-Osoitteen.

    Add-AzureDisk -DiskName "DataDisk" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/datadisk.vhd" -Label "My Data Disk"

Kopioida ja tallentaa uuden Azure tietojen levyn nimi. Yllä olevassa esimerkissä on *DataDisk*.

### <a name="create-a-premium-storage-capable-vm"></a>Luo Premium tallennustilan pystyvät AM

Kerran OS kuva tai OS levy on rekisteröity, Luo uusi DS-sarjan, DSv2 sarjan tai GS sarjan AM. Haluatko käyttää, käyttöjärjestelmä kuvaa tai käyttöjärjestelmä levyn nimi, joka on rekisteröity. Valitse haluamasi AM Premium tallennustilan taso. Alla olevassa esimerkissä on käytössä *Standard_DS2* AM kokoa.

>[AZURE.NOTE] Varmista, että se vastaa omaa kapasiteetin ja suorituskyvyn vaatimukset ja Azure vapaata koot levyn kokoa päivittäminen

Vaihe vaiheelta PowerShell cmdlet-Luo uusi AM seuraavia ohjeita noudattamalla. Määritä ensin yleiset parametrit:

    $serviceName = "yourVM"
    $location = "location-name" (e.g., West US)
    $vmSize ="Standard_DS2"
    $adminUser = "youradmin"
    $adminPassword = "yourpassword"
    $vmName ="yourVM"
    $vmSize = "Standard_DS2"

Luo pilvipalvelussa, jossa voit oltava isännöidä uusi VMs.

    New-AzureService -ServiceName $serviceName -Location $location

Seuraavaksi oman toimintamallin Luo OS kuva tai OS levy, joka on rekisteröity Azure AM esiintymä.

#### <a name="generalized-operating-system-vhd-to-create-multiple-azure-vm-instances"></a>Generalized käyttöjärjestelmän Näennäiskiintolevyn luominen Azure AM useita kertoja

Luo vähintään yksi uusi DS sarjan Azure AM esiintymät käyttämällä **Azure OS kuva** , joka on rekisteröity. Määritä OS kuvan nimi AM määritykset, kun luot uuden AM alla kuvatulla tavalla.

    $OSImage = Get-AzureVMImage –ImageName "OSImageName"

    $vm = New-AzureVMConfig -Name $vmName –InstanceSize $vmSize -ImageName $OSImage.ImageName

    Add-AzureProvisioningConfig -Windows –AdminUserName $adminUser -Password $adminPassword –VM $vm

    New-AzureVM -ServiceName $serviceName -VM $vm

#### <a name="unique-operating-system-vhd-to-create-a-single-azure-vm-instance"></a>Yksilöivä käyttöjärjestelmän Näennäiskiintolevyn luominen Azure AM esiintymiä

Luo uuden DS sarjan Azure AM esiintymän käyttämällä **Azure OS levy** , joka on rekisteröity. Määritä OS levyn nimi AM määritykset, kun luot uuden AM alla kuvatulla tavalla.

    $OSDisk = Get-AzureDisk –DiskName "OSDisk"

    $vm = New-AzureVMConfig -Name $vmName -InstanceSize $vmSize -DiskName $OSDisk.DiskName

    New-AzureVM -ServiceName $serviceName –VM $vm

Voit määrittää muita Azure AM tietoja, kuten pilvipalvelussa, alueen, tallennustilan tilin, Määritä käytettävyys ja välimuistiin käytännön. Huomaa, että AM esiintymää on yhteyteen liittyvät käyttöjärjestelmän tai tietojen levyjä, jotta valitun cloud service-, alue- ja tallennustilaa tilin, on oltava samassa sijainnissa kuin pohjana näennäiskiintolevyjen näiden levyjen.

### <a name="attach-data-disk"></a>Liitä tiedot levy

Lopuksi, jos olet rekisteröinyt tietojen levyn näennäiskiintolevyjen, ne liitetään uusi Premium tallennustilan pystyvät Azure AM.

Käytä PowerShell cmdlet-komennon jälkeen tietojen levyn liittäminen uuteen AM ja määritä välimuistiin käytäntö. Alla olevassa esimerkissä välimuistiin käytäntö määritetään *vain luku-tilassa*.

    $vm = Get-AzureVM -ServiceName $serviceName -Name $vmName

    Add-AzureDataDisk -ImportFrom -DiskName "DataDisk" -LUN 0 –HostCaching ReadOnly –VM $vm

    Update-AzureVM  -VM $vm

>[AZURE.NOTE] Ehkä lisävaiheita tukemaan sovellus, joka on tarpeen ei koske tässä oppaassa.

### <a name="checking-and-plan-backup"></a>Tarkistaminen ja varmuuskopioinnin suunnittelu

Kun uusi AM on hyvin alkuun, avata sen saman kirjautumistunnus ja salasana on alkuperäinen AM ja tarkista, että kaikki toimii odotetulla. Kaikki asetukset, mukaan lukien mustat tietomääristä olisi paikalla olevat uusi AM.

Viimeinen vaihe on varmuuskopioinnin suunnittelu ja ylläpito aikataulua uusi AM sovelluksen tarpeiden perusteella.

### <a name="a-sample-migration-script"></a>Esimerkki siirto-komentosarja

Jos sinulla on useita tapoja siirtää VMs, automaatio PowerShell-komentosarjojen kautta on hyötyä. Seuraavassa on esimerkki komentosarjan, joka AM siirron automatisoi. Huomautus alla komentosarja, joka on vain yksi esimerkki ja on muutamia oletusten tehty nykyisen AM tietoja. Saatat joutua päivittämään komentosarja vastaamaan ja tietyn skenaario.

Perustietojen ovat seuraavat:

- Perinteinen Azure VMs luominen
- Tietolähteen OS levyille ja lähteen tiedot levyjä ovat samaa tallennustilan tiliä ja samaan säilö. Jos OS levyille ja tietojen levyjen eivät ole entisellä paikallaan, voit AzCopy tai PowerShellin Azure näennäiskiintolevyjen luodulla tallennustilan tilit ja säilöjä. Edellisessä vaiheessa viitata: [Kopioi Näennäiskiintolevyn AzCopy tai PowerShell](#copy-vhd-with-azcopy-or-powershell). Tämä komentosarja täyttävän käyttämässäsi skenaariossa muokkaaminen on joksikin muuksi vaihtoehdoksi, mutta suosittelemme, että käytät AzCopy tai PowerShell, koska se on helppoa ja nopeaa.

Automaatio-komentosarja on jäljempänä. Tekstin korvaaminen omilla tiedoillasi ja Päivitä komentosarja vastaamaan ja tietyn skenaario.

>[AZURE.NOTE] Olemassa olevan komentosarjan avulla ei säilytä lähde AM verkon määrittäminen. Tarvitset uudelleen config verkko asetukset valitse siirretyt VMs.

    <#
    .Synopsis
    This script is provided as an EXAMPLE to show how to migrate a VM from a standard storage account to a premium storage account. You can customize it according to your specific requirements.

    .Description
    The script will copy the vhds (page blobs) of the source VM to the new storage account.
    And then it will create a new VM from these copied vhds based on the inputs that you specified for the new VM.
    You can modify the script to satisfy your specific requirement, but please be aware of the items specified
    in the Terms of Use section.

    .Terms of Use
    Copyright © 2015 Microsoft Corporation.  All rights reserved.

    THIS CODE AND ANY ASSOCIATED INFORMATION ARE PROVIDED “AS IS” WITHOUT WARRANTY OF ANY KIND,
    EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE IMPLIED WARRANTIES OF MERCHANTABILITY
    AND/OR FITNESS FOR A PARTICULAR PURPOSE. THE ENTIRE RISK OF USE, INABILITY TO USE, OR
    RESULTS FROM THE USE OF THIS CODE REMAINS WITH THE USER.

    .Example (Save this script as Migrate-AzureVM.ps1)

    .\Migrate-AzureVM.ps1 -SourceServiceName CurrentServiceName -SourceVMName CurrentVMName –DestStorageAccount newpremiumstorageaccount -DestServiceName NewServiceName -DestVMName NewDSVMName -DestVMSize "Standard_DS2" –Location “Southeast Asia”

    .Link
    To find more information about how to set up Azure PowerShell, refer to the following links.
    http://azure.microsoft.com/documentation/articles/powershell-install-configure/
    http://azure.microsoft.com/documentation/articles/storage-powershell-guide-full/
    http://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/

    #>

    Param(
    # the cloud service name of the VM.
    [Parameter(Mandatory = $true)]
    [string] $SourceServiceName,

    # The VM name to copy.
    [Parameter(Mandatory = $true)]
    [String] $SourceVMName,

    # The destination storage account name.
    [Parameter(Mandatory = $true)]
    [String] $DestStorageAccount,

    # The destination cloud service name
    [Parameter(Mandatory = $true)]
    [String] $DestServiceName,

    # the destination vm name
    [Parameter(Mandatory = $true)]
    [String] $DestVMName,

    # the destination vm size
    [Parameter(Mandatory = $true)]
    [String] $DestVMSize,

    # the location of destination VM.
    [Parameter(Mandatory = $true)]
    [string] $Location,

    # whether or not to copy the os disk, the default is only copy data disks
    [Parameter(Mandatory = $false)]
    [Bool] $DataDiskOnly = $true,

    # how frequently to report the copy status in sceconds
    [Parameter(Mandatory = $false)]
    [Int32] $CopyStatusReportInterval = 15,

    # the name suffix to add to new created disks to avoid conflict with source disk names
    [Parameter(Mandatory = $false)]
    [String]$DiskNameSuffix = "-prem"

    ) #end param

    #######################################################################
    #  Verify Azure PowerShell module and version
    #######################################################################

    #import the Azure PowerShell module
    Write-Host "`n[WORKITEM] - Importing Azure PowerShell module" -ForegroundColor Yellow
    $azureModule = Import-Module Azure -PassThru

    if ($azureModule -ne $null)
    {
        Write-Host "`tSuccess" -ForegroundColor Green
    }
    else
    {
        #show module not found interaction and bail out
        Write-Host "[ERROR] - PowerShell module not found. Exiting." -ForegroundColor Red
        Exit
    }


    #Check the Azure PowerShell module version
    Write-Host "`n[WORKITEM] - Checking Azure PowerShell module verion" -ForegroundColor Yellow
    If ($azureModule.Version -ge (New-Object System.Version -ArgumentList "0.8.14"))
    {
        Write-Host "`tSuccess" -ForegroundColor Green
    }
    Else
    {
        Write-Host "[ERROR] - Azure PowerShell module must be version 0.8.14 or higher. Exiting." -ForegroundColor Red
        Exit
    }

    #Check if there is an azure subscription set up in PowerShell
    Write-Host "`n[WORKITEM] - Checking Azure Subscription" -ForegroundColor Yellow
    $currentSubs = Get-AzureSubscription -Current
    if ($currentSubs -ne $null)
    {
        Write-Host "`tSuccess" -ForegroundColor Green
        Write-Host "`tYour current azure subscription in PowerShell is $($currentSubs.SubscriptionName)." -ForegroundColor Green
    }
    else
    {
        Write-Host "[ERROR] - There is no valid Azure subscription found in PowerShell. Please refer to this article http://azure.microsoft.com/documentation/articles/powershell-install-configure/ to connect an Azure subscription. Exiting." -ForegroundColor Red
        Exit
    }


    #######################################################################
    #  Check if the VM is shut down
    #  Stopping the VM is a required step so that the file system is consistent when you do the copy operation.
    #  Azure does not support live migration at this time..
    #######################################################################

    if (($sourceVM = Get-AzureVM –ServiceName $SourceServiceName –Name $SourceVMName) -eq $null)
    {
        Write-Host "[ERROR] - The source VM doesn't exist in the current subscription. Exiting." -ForegroundColor Red
        Exit
    }

    # check if VM is shut down
    if ( $sourceVM.Status -notmatch "Stopped" )
    {
        Write-Host "[Warning] - Stopping the VM is a required step so that the file system is consistent when you do the copy operation. Azure does not support live migration at this time. If you’d like to create a VM from a generalized image, sys-prep the Virtual Machine before stopping it." -ForegroundColor Yellow
        $ContinueAnswer = Read-Host "`n`tDo you wish to stop $SourceVMName now? Input 'N' if you want to shut down the VM manually and come back later.(Y/N)"
        If ($ContinueAnswer -ne "Y") { Write-Host "`n Exiting." -ForegroundColor Red;Exit }
        $sourceVM | Stop-AzureVM

        # wait until the VM is shut down
        $VMStatus = (Get-AzureVM –ServiceName $SourceServiceName –Name $vmName).Status
        while ($VMStatus -notmatch "Stopped")
        {
            Write-Host "`n[Status] - Waiting VM $vmName to shut down" -ForegroundColor Green
            Sleep -Seconds 5
            $VMStatus = (Get-AzureVM –ServiceName $SourceServiceName –Name $vmName).Status
        }
    }

    # exporting the sourve vm to a configuration file, you can restore the original VM by importing this config file
    # see more information for Import-AzureVM
    $workingDir = (Get-Location).Path
    $vmConfigurationPath = $env:HOMEPATH + "\VM-" + $SourceVMName + ".xml"
    Write-Host "`n[WORKITEM] - Exporting VM configuration to $vmConfigurationPath" -ForegroundColor Yellow
    $exportRe = $sourceVM | Export-AzureVM -Path $vmConfigurationPath


    #######################################################################
    #  Copy the vhds of the source vm
    #  You can choose to copy all disks including os and data disks by specifying the
    #  parameter -DataDiskOnly to be $false. The default is to copy only data disk vhds
    #  and the new VM will boot from the original os disk.
    #######################################################################

    $sourceOSDisk = $sourceVM.VM.OSVirtualHardDisk
    $sourceDataDisks = $sourceVM.VM.DataVirtualHardDisks

    # Get source storage account information, not considering the data disks and os disks are in different accounts
    $sourceStorageAccountName = $sourceOSDisk.MediaLink.Host -split "\." | select -First 1
    $sourceStorageKey = (Get-AzureStorageKey -StorageAccountName $sourceStorageAccountName).Primary
    $sourceContext = New-AzureStorageContext –StorageAccountName $sourceStorageAccountName -StorageAccountKey $sourceStorageKey

    # Create destination context
    $destStorageKey = (Get-AzureStorageKey -StorageAccountName $DestStorageAccount).Primary
    $destContext = New-AzureStorageContext –StorageAccountName $DestStorageAccount -StorageAccountKey $destStorageKey

    # Create a container of vhds if it doesn't exist
    if ((Get-AzureStorageContainer -Context $destContext -Name vhds -ErrorAction SilentlyContinue) -eq $null)
    {
        Write-Host "`n[WORKITEM] - Creating a container vhds in the destination storage account." -ForegroundColor Yellow
        New-AzureStorageContainer -Context $destContext -Name vhds
    }


    $allDisksToCopy = $sourceDataDisks
    # check if need to copy os disk
    $sourceOSVHD = $sourceOSDisk.MediaLink.Segments[2]
    if ($DataDiskOnly)
    {
        # copy data disks only, this option requires deleting the source VM so that dest VM can boot
        # from the same vhd blob.
        $ContinueAnswer = Read-Host "`n`t[Warning] You chose to copy data disks only. Moving VM requires removing the original VM (the disks and backing vhd files will NOT be deleted) so that the new VM can boot from the same vhd. This is an irreversible action. Do you wish to proceed right now? (Y/N)"
        If ($ContinueAnswer -ne "Y") { Write-Host "`n Exiting." -ForegroundColor Red;Exit }
        $destOSVHD = Get-AzureStorageBlob -Blob $sourceOSVHD -Container vhds -Context $sourceContext
        Write-Host "`n[WORKITEM] - Removing the original VM (the vhd files are NOT deleted)." -ForegroundColor Yellow
        Remove-AzureVM -Name $SourceVMName -ServiceName $SourceServiceName

        Write-Host "`n[WORKITEM] - Waiting utill the OS disk is released by source VM. This may take up to several minutes."
        $diskAttachedTo = (Get-AzureDisk -DiskName $sourceOSDisk.DiskName).AttachedTo
        while ($diskAttachedTo -ne $null)
        {
            Start-Sleep -Seconds 10
            $diskAttachedTo = (Get-AzureDisk -DiskName $sourceOSDisk.DiskName).AttachedTo
        }

    }
    else
    {
        # copy the os disk vhd
        Write-Host "`n[WORKITEM] - Starting copying os disk $($disk.DiskName) at $(get-date)." -ForegroundColor Yellow
        $allDisksToCopy += @($sourceOSDisk)
        $targetBlob = Start-AzureStorageBlobCopy -SrcContainer vhds -SrcBlob $sourceOSVHD -DestContainer vhds -DestBlob $sourceOSVHD -Context $sourceContext -DestContext $destContext -Force
        $destOSVHD = $targetBlob
    }


    # Copy all data disk vhds
    # Start all async copy requests in parallel.
    foreach($disk in $sourceDataDisks)
    {
        $blobName = $disk.MediaLink.Segments[2]
        # copy all data disks
        Write-Host "`n[WORKITEM] - Starting copying data disk $($disk.DiskName) at $(get-date)." -ForegroundColor Yellow
        $targetBlob = Start-AzureStorageBlobCopy -SrcContainer vhds -SrcBlob $blobName -DestContainer vhds -DestBlob $blobName -Context $sourceContext -DestContext $destContext -Force
        # update the media link to point to the target blob link
        $disk.MediaLink = $targetBlob.ICloudBlob.Uri.AbsoluteUri
    }

    # Wait until all vhd files are copied.
    $diskComplete = @()
    do
    {
        Write-Host "`n[WORKITEM] - Waiting for all disk copy to complete. Checking status every $CopyStatusReportInterval seconds." -ForegroundColor Yellow
        # check status every 30 seconds
        Sleep -Seconds $CopyStatusReportInterval
        foreach ( $disk in $allDisksToCopy)
        {
            if ($diskComplete -contains $disk)
            {
                Continue
            }
            $blobName = $disk.MediaLink.Segments[2]
            $copyState = Get-AzureStorageBlobCopyState -Blob $blobName -Container vhds -Context $destContext
            if ($copyState.Status -eq "Success")
            {
                Write-Host "`n[Status] - Success for disk copy $($disk.DiskName) at $($copyState.CompletionTime)" -ForegroundColor Green
                $diskComplete += $disk
            }
            else
            {
                if ($copyState.TotalBytes -gt 0)
                {
                    $percent = ($copyState.BytesCopied / $copyState.TotalBytes) * 100
                    Write-Host "`n[Status] - $('{0:N2}' -f $percent)% Complete for disk copy $($disk.DiskName)" -ForegroundColor Green
                }
            }
        }
    }
    while($diskComplete.Count -lt $allDisksToCopy.Count)

    #######################################################################
    #  Create a new vm
    #  the new VM can be created from the copied disks or the original os disk.
    #  You can ddd your own logic here to satisfy your specific requirements of the vm.
    #######################################################################

    # Create a VM from the existing os disk
    if ($DataDiskOnly)
    {
        $vm = New-AzureVMConfig -Name $DestVMName -InstanceSize $DestVMSize -DiskName $sourceOSDisk.DiskName
    }
    else
    {
        $newOSDisk = Add-AzureDisk -OS $sourceOSDisk.OS -DiskName ($sourceOSDisk.DiskName + $DiskNameSuffix) -MediaLocation $destOSVHD.ICloudBlob.Uri.AbsoluteUri
        $vm = New-AzureVMConfig -Name $DestVMName -InstanceSize $DestVMSize -DiskName $newOSDisk.DiskName
    }
    # Attached the copied data disks to the new VM
    foreach ($dataDisk in $sourceDataDisks)
    {
        # add -DiskLabel $dataDisk.DiskLabel if there are labels for disks of the source vm
        $diskLabel = "drive" + $dataDisk.Lun
        $vm | Add-AzureDataDisk -ImportFrom -DiskLabel $diskLabel -LUN $dataDisk.Lun -MediaLocation $dataDisk.MediaLink
    }

    # Edit this if you want to add more custimization to the new VM
    # $vm | Add-AzureEndpoint -Protocol tcp -LocalPort 443 -PublicPort 443 -Name 'HTTPs'
    # $vm | Set-AzureSubnet "PubSubnet","PrivSubnet"

    New-AzureVM -ServiceName $DestServiceName -VMs $vm -Location $Location

#### <a name="optimization"></a>Optimointi

AM nykyiset voi mukauttaa erityisesti hyvin vakio levyjen-käyttöä varten. Esimerkiksi niin, että suorituskyvyn käyttämällä useita levyjä mustat äänenvoimakkuutta. Esimerkiksi sen sijaan, että käytät 4 levyjen erikseen Premium tallennustilaa, voit ehkä optimoi kustannukset että yhteen. Optimointi, kuten tässä tarvitse käsitellään tapaus mukaan välein ja edellyttävät mukautettuja siirron jälkeen. Huomaa, että tätä prosessia ei ehkä myös toimi tietokantojen ja sovelluksia, jotka riippuvat määritetty asetuksissa levyn asettelu.

##### <a name="preparation"></a>Valmistelu

1.  Yksinkertainen siirtyminen on valmis aiemmissa kohdassa kuvatulla tavalla. Optimointi suoritetaan uusi AM-siirron jälkeen.
2.  Määritä uuden levyn koot tarvitaan optimoitujen määritykset.
3.  Määrittää nykyisen levyille ja asemat uuden levyn määritysten määritystä.

##### <a name="execution-steps"></a>Suorittamisen vaiheet

1.  Voit luoda uusia levyjä oikean koot käyttöön Premium tallennustilan AM.
2.  Kirjautuminen uuden levyn, joka yhdistää kyseisen aseman nykyisen aseman AM ja kopioi tiedot. Toimi samoin kaikki nykyisen asemat, joka on yhdistettävä uuden levyn.
3.  Muuta sovellusasetuksia siirtyä käyttämään uutta levyä ja irrottaminen vanha asemat.

Säätämiseksi sovelluksen levyn suorituskyvyn parantamiseksi, tutustu [Optimointi sovelluksen suorituskykyä](storage-premium-storage-performance.md#optimizing-application-performance).

### <a name="application-migrations"></a>Sovelluksen siirrot

Tietokantojen ja monimutkaisia sovelluksissa saattaa edellyttää erityistä vaiheet määrityksen mukaisesti siirron sovelluksen-palvelu. Tutustu vastaaviin sovelluksen ohjeissa. Esimerkiksi yleensä tietokantojen uuteen kautta varmuuskopiointi ja palauttaminen.

## <a name="next-steps"></a>Seuraavat vaiheet

Katso, tietyissä skenaarioissa siirretään virtual tietokoneissa on seuraavissa resursseissa:

- [Siirtää Azuren näennäiskoneiden tallennustilan tilien välillä](https://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/)
- [Luo ja lataa Windows Server Näennäiskiintolevyn Azure.](../virtual-machines/virtual-machines-windows-classic-createupload-vhd.md)
- [Luominen ja lataamisen Virtual kiintolevyn, joka sisältää Linux-käyttöjärjestelmä](../virtual-machines/virtual-machines-linux-classic-create-upload-vhd.md)
- [Luotujen näennäiskoneiden Amazon sisältyy AWS-, Microsoft Azure](http://channel9.msdn.com/Series/Migrating-Virtual-Machines-from-Amazon-AWS-to-Microsoft-Azure)

Katso myös lisätietoja Azure-tallennustilan ja Azuren näennäiskoneiden on seuraavissa resursseissa:

- [Azure-tallennustilan](https://azure.microsoft.com/documentation/services/storage/)
- [Azure-Virtuaalikoneissa](https://azure.microsoft.com/documentation/services/virtual-machines/)
- [Premium tallennustila: Azure virtuaalikoneen työmääriä tehokas tallennustila](storage-premium-storage.md)

[1]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-1.png
[2]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-1.png
[3]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-3.png
[4]: http://technet.microsoft.com/library/hh831739.aspx

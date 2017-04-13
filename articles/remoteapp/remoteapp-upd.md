
<properties 
    pageTitle="Miten Azure RemoteApp tallentaa käyttäjätiedot ja asetuksia? | Microsoft Azure"
    description="Katso, miten Azure RemoteApp tallentaa käyttäjätiedot käyttäjän profiili-levyn avulla."
    services="remoteapp"
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />

# <a name="how-does-azure-remoteapp-save-user-data-and-settings"></a>Miten Azure RemoteApp tallentaa käyttäjätiedot ja asetuksia?

> [AZURE.IMPORTANT]
> Azure RemoteApp on on lopetettu. Lue lisätietoja [ilmoitus](https://go.microsoft.com/fwlink/?linkid=821148) .

Azure RemoteApp tallentaa käyttäjätietojen ja mukautukset laitteet ja istuntojen välillä. Käyttäjätiedot on tallennettu käyttäjäkohtainen sivustokokoelman DVD-levyllä, nimeltään profiilin levyn (UPD). Levyn jäljessä käyttäjälle ja varmistaa, että käyttäjällä on yhtenäinen kokemus, riippumatta siitä, missä ne kirjautuminen. tallentaa 

Käyttäjän profiiliin levyjä ovat täysin läpinäkyvä käyttäjälle – käyttäjät tallentaa asiakirjat niiden tiedostot-kansio (Valitse yhdensuuntaisia paikallisesta asemasta) ja muuta kuin tavanomainen sovelluksen asetuksia. Samanaikaisesti kaikki omat asetukset jatkuvat muodostettaessa yhteyttä Azure RemoteApp millä tahansa laitteella. Kaikki käyttäjä näkee on niiden tiedot samassa paikassa.

Kunkin UPD 50 gt: n pysyvä tallennustilan on, ja se sisältää sekä käyttäjän tietoja ja asetukset. 

Tutustu jakamistapoihin yksityiskohtia käyttäjäprofiilin tietoja.

>[AZURE.NOTE] Poista käytöstä UPD on? Tee, nyt - Pavithra on blogikirjoituksessa [Käytöstä käyttäjän profiiliin levyjen (UPDs) Azure RemoteApp](https://blogs.technet.microsoft.com/enterprisemobility/2015/11/11/disable-user-profile-disks-upds-in-azure-remoteapp/)lisätietoja uloskuittaus.


## <a name="how-can-an-admin-get-to-the-data"></a>Miten järjestelmänvalvoja voi hankkia tietoihin?

Jos haluat käyttää tietoja johonkin käyttäjien (, palauttaminen tai jos käyttäjä jättää yrityksen), Azure tuelta ja anna Tilaustiedot keräämiseen ja käyttäjätietoja. Azure RemoteApp-ryhmän antaa sinulle Näennäiskiintolevyn URL-Osoitetta. Lataa kyseisen Näennäiskiintolevyn ja noutaa kaikki asiakirjat tai tarvitsemasi tiedostot. Huomaa, että Näennäiskiintolevyn 50 Gigatavun käyttöösi bit lataamalla sen.


## <a name="is-the-data-backed-up"></a>Tietoja varmuuskopioidaan?

Kyllä, emme tallenna käyttäjätiedot kohti maantieteellisen sijainnin varmuuskopion. Tiedot on vain luku- ja niitä voi käyttää samalla tavalla kuin tavallinen tiedot (Ota Azure RemoteApp hankkiminen), jos ensisijainen tietokeskuksen alas. Tiedot kopioidaan reaaliaikainen varmuuskopion sijainti ja emme ole säilyttämään kopioita määritetyistä eri versioita. Tietovirheitä, emme voi palauttaa aikaisemmin hyvä versioon, mutta ensisijainen tietokeskuksen ei ole käytettävissä, jos osaat käyttäjätiedot käyttämistä toiseen sijaintiin.

## <a name="how-do-users-see-the-upd-on-the-server-side"></a>Miten käyttäjät tarkistaa UPD palvelimella?

Kullakin käyttäjällä on oman kansion palvelimessa, joka yhdistää niiden UPD: c:\Users\username.

## <a name="whats-the-best-way-to-use-outlook-and-upd"></a>Mikä on paras tapa käyttää Outlook ja UPD?

Azure RemoteApp tallentaa Outlook-tila (postilaatikot pst) istuntojen välillä. Voit ottaa tämän käyttäjäprofiilin tiedot tallennetaan PST annettava (c:\users\<käyttäjänimi >). Tämä on tietoja, sitä nopeammin, kun muutat ei sijainti oletussijainti, tiedot on käytössä, istuntojen välillä.

On suositeltavaa myös käyttää Outlookissa "synkronointitila" ja "palvelin/online-tilan käyttäminen etsimistä varten.

Tutustu [tämän artikkelin](remoteapp-outlook.md) lisätietoja Outlook-ja Azure RemoteApp.

## <a name="what-about-redirection"></a>Mitä uudelleenohjaus?
Voit määrittää Azure RemoteApp, jotta käyttäjät voivat käyttää Paikalliset laitteet määrittämällä [uudelleenohjaus](remoteapp-redirection.md). Paikalliset laitteet sitten voi käyttää UPD tiedot.

## <a name="can-i-use-my-upd-as-a-network-share"></a>Voinko käyttää Omat UPD nimellä jaettuun verkkoresurssiin?
Ei. UPDs ei voi käyttää jaettuun verkkoresurssiin. UPD on vain käyttäjien käytettävissä, kun käyttäjä on yhdistetty aktiivisesti Azure RemoteApp.

## <a name="if-i-delete-a-user-from-a-collection-is-their-upd-deleted"></a>Jos käyttäjä poistetaan kokoelma, niiden UPD poistetaan?

Ei, kun poistat käyttäjän, on automaattisesti Poista UPD -, tiedot tallennetaan, kunnes poistat sivustokokoelman. 90 päivää, kun olet poistanut keräämistä, Microsoft poistaa kaikki UPDs. 

Jos haluat poistaa UPD kokoelmasta, ota yhteyttä Azure RemoteApp - olemme poistaa UPD Microsoftin reunasta.

## <a name="can-i-access-my-users-upds-either-current-or-deleted-users"></a>Voit käyttää aiemmin käyttäjien UPDs (nykyisen tai poistetut käyttäjät)?

Kyllä, jos otat yhteyttä [Azure RemoteApp](mailto:remoteappforum@microsoft.com), emme määrittää voit tietojen URL-osoite. Voit ladata tietoja tai tiedostot UPD access vanhenee on noin 10 tuntia.

## <a name="are-upds-available-offline"></a>Ovat UPDs offline-tilassa?

Nyt ei annamme offline-tila UPDs, voit lisäksi kuvattuun 10 tunti access-ikkunan oikealla. Tämä tarkoittaa, että Microsoft ei ole voi antaa sinulle access pitkään tarpeeksi monimutkaisempia suorittaminen, kuten virustorjuntaohjelmaa käytössä UPDs tai tarkastus tietojen käyttäminen.

## <a name="do-registry-key-settings-persist"></a>Rekisteriavainten asetuksista jatkuvat?
Kyllä, mitään kirjoitettu HKEY_Current_User on osa UPD.

## <a name="can-i-disable-upds-for-a-collection"></a>Voinko poistaa käytöstä UPDs kokoelman?

Kyllä, voit pyytää Azure RemoteApp UPDs käytöstä tilauksen, mutta et voi tehdä kyseisen itse. Tämä tarkoittaa, että UPDs eivät ole käytettävissä kaikissa loppu-tilaus.

Haluat ehkä poistaa käytöstä UPDs kaikista seuraavissa tilanteissa: 

- Käytön ja käyttäjätietojen hallinta on valmis (, valvoa ja tarkista tarkoituksiin, kuten taloudellisen tilanteen laitokset).
- Sinun on 3rd osapuolen käyttäjän profiilin hallinta ratkaisuja paikalliseen ja haluat jatkaa käyttäminen toimialueen liittyneet Azure RemoteApp-käyttöönoton. Tämä edellyttää ladataan kyselyjä gold kuva profiili-agentti. 
- Et tarvitse mitään paikallisten tietojen tallentaminen tai kaikki tiedot ovat cloud tai tiedostojen jakaminen ja haluaisit ohjausobjektin paikallisesti käyttämällä Azure RemoteApp tietojen tallentamista.

Lisätietoja on kohdassa [Käytöstä käyttäjän profiiliin levyjen (UPDs) Azure RemoteApp](https://blogs.technet.microsoft.com/enterprisemobility/2015/11/11/disable-user-profile-disks-upds-in-azure-remoteapp/) .

## <a name="can-i-restrict-users-from-saving-data-to-the-system-drive"></a>Käyttäjien rajoittaa tallentamasta tietojen järjestelmäasema

Kyllä, mutta sinun on määritettävä, suunnittelumallin kuva ennen kuin luot kokoelma. Järjestelmäasema käytön estäminen noudattamalla seuraavia ohjeita:

1. Suorita **gpedit.msc** suunnittelumallin kuva.
2. Siirry **Käyttäjäasetukset > järjestelmänvalvojan mallit > Windows-osia > Explorer**.
3. Valitse seuraavista vaihtoehdoista:
    - **Piilota oman tietokoneen määritetyt asemat**
    - **Estä käyttö asemat paikallisena**

## <a name="can-i-seed-upds-i-want-to-put-some-data-in-the-upd-thats-available-the-first-time-the-user-signs-in"></a>Voit sisällyttää UPDs? Haluan tietoja sijoitetaan UPD, joka on käytettävissä käyttäjä kirjautuu ensimmäisen kerran.

Kyllä, kun luot mallin kuvaa, voit lisätä tietoja oletusprofiili. Nämä tiedot lisätään sitten UPD.

## <a name="can-i-change-the-size-of-the-upd-depending-on-how-much-data-i-want-to-store"></a>Sen mukaan, kuinka paljon tietoja haluan tallentaa UPD koon muuttaminen

Ei, kaikki UPDs on 50 Gigatavua lisätallennustilaa. Jos haluat tallentaa eri määriä tietoja, toimi seuraavasti:

1. Poista käytöstä UPDs kokoelman.
2. Määritä käyttäjät voivat käyttää jaettua tiedostoresurssia.
3. Lataa jaettua tiedostoresurssia käynnistyskomentosarjan avulla. Noudata seuraavia ohjeita lisätietoja Azure RemoteApp Käynnistys-komentosarjat.
4. Käyttäjät voivat tallentaa kaikki tiedot jaettua tiedostoresurssia.


## <a name="how-do-i-run-a-startup-script-in-azure-remoteapp"></a>Miten käynnistyskomentosarjan suorittaminen Azure RemoteApp?

Jos haluat suorittaa käynnistyskomentosarjan, aloita luomalla ajoitetun tehtävän aiot käyttää kokoelman mallin kuvassa. (Tee tämä *ennen kuin* suoritat Sysprepin.) 

![Järjestelmän tehtävän luominen](./media/remoteapp-upd/upd1.png)

![Järjestelmän tehtävän, joka suoritetaan, kun käyttäjä kirjautuu luominen](./media/remoteapp-upd/upd2.png)

Valitse **Yleiset** -välilehden muista vaihtaa **Käyttäjätilin** suojauksen "BUILTIN\Users."

![Muuta käyttäjätilin lisääminen ryhmään](./media/remoteapp-upd/upd4.png)

Ajoitettu tehtävä käynnistää käynnistys-komentosarja-käyttäjän tunnistetiedoilla. Tehtävän suorittamiseen jokaisen aika, jonka käyttäjä kirjautuu sisään.

![Määrittää tehtävän käynnistin kuin "kirjautuessa sisään"](./media/remoteapp-upd/upd3.png)

Voit käyttää myös [Ryhmäkäytäntö perustuva Käynnistys-komentosarjat](https://technet.microsoft.com/library/cc779329%28v=ws.10%29.aspx). 

## <a name="what-about-placing-a-startup-script-in-the-start-menu-would-that-work"></a>Entä siten käynnistys aloitusvalikkoon? , Joka toimii?

Toisin sanoen Voinko luoda .bat-tiedosto, joka suorittaa config ikkuna-komentosarja ja tallenna se c:\ProgramData\Microsoft\Windows\Start valikko\Ohjelmat\Käynnistys kansioon, ja on suoritetaan aina, kun käyttäjä aloittaa RemoteApp istunnon komentosarjan?

Joka ei, ei tueta Azure RemoteApp, joka käyttää RDSH, joka myös ei tue Käynnistys-komentosarjat Käynnistä-valikkoon.

## <a name="can-i-use-mstscexe-the-remote-desktop-program-to-configure-logon-scripts"></a>Määritä Sisäänkirjautuminen-komentosarjat mstsc.exe (ohjelma Remote Desktopia) avulla?

Ei Kiitos ei tue Azure RemoteApp.

## <a name="can-i-store-data-on-the-vm-locally"></a>Tiedot tallennetaan paikallisesti AM käyttöön

EI, missä tahansa muussa kuin AM UPD-tallennetut tiedot menetetään. On suuri mahdollisuus käyttäjä ei tule saman AM seuraavan kerran, ne kirjautua Azure RemoteApp. Microsoft ei enää käyttäjän AM pysyvyyttä niin, että käyttäjä ei Kirjaudu sisään samalla AM ja tiedot menetetään. Lisäksi, kun päivitämme kokoelma, aiemmin VMs korvataan uudella valikoimalla VMs -, joka tarkoittaa itse AM tallennetut tiedot menetetään. Suositus on UPD jaettua tallennustilan esimerkiksi Azure-tiedostot tiedostopalvelimessa VNET sisällä tai cloud tallennustilan-järjestelmää, kuten DropBox pilveen tietojen tallentamiseen.

## <a name="how-do-i-mount-an-azure-file-share-on-a-vm-using-powershell"></a>Miten Azure-tiedostoresurssin käyttöön-AM-PowerShellin avulla?

Verkon PSDrive cmdlet-komennon avulla voit liittää aseman, seuraavasti:

    New-PSDrive -Name <drive-name> -PSProvider FileSystem -Root \\<storage-account-name>.file.core.windows.net\<share-name> -Credential :<storage-account-name>


Voit myös tallentaa tunnistetiedot suorittamalla seuraavasti:

    cmdkey /add:<storage-account-name>.file.core.windows.net /user:<storage-account-name> /pass:<storage-account-key>


Jonka avulla voit ohittaa uusi PSDrive cmdlet-komento tunnistetiedon-parametrin.

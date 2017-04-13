<properties
    pageTitle="Uudelleenohjauksen käyttäminen Azure RemoteApp | Microsoft Azure"
    description="Opettele määrittäminen ja käyttäminen uudelleenohjaus RemoteApp"
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

# <a name="using-redirection-in-azure-remoteapp"></a>Uudelleenohjauksen käyttäminen Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp on on lopetettu. Lue lisätietoja [ilmoitus](https://go.microsoft.com/fwlink/?linkid=821148) .

Laitteen uudelleenohjaus avulla käsitellä remote-sovelluksista käsin paikalliseen tietokoneeseen, puhelinten ja taulutietokoneiden liitettyjen laitteiden käyttäjien. Jos olet Skype Azure RemoteApp kautta, käyttäjän on kamera asentanut niiden Tietokoneeseesi Skype-käyttöä varten. Tämä koskee myös tulostimet, kaiuttimet, näytöt ja USB alueen yhdistetty oheislaitteet.

RemoteApp hyödyntää Remote Desktop Protocol (RDP) ja RemoteFX antamaan uudelleenohjaus.

## <a name="what-redirection-is-enabled-by-default"></a>Mitä uudelleenohjaus on oletusarvoisesti käytössä?
Kun RemoteApp, seuraavat uudelleenohjausta ovat oletusarvoisesti käytössä. Tietoja sulkeissa Näytä RDP-asetus.

- Toista (**Toista tässä tietokoneessa**) paikallisen tietokoneen äänet. (audiomode:i:0)
- Sieppaa ääni paikallisesta tietokoneesta ja Lähetä etätietokoneeseen (**tämän tietokoneen tietue**). (audiocapturemode:i:1)
- Tulosta paikallisten tulostimien (redirectprinters:i:1)
- COM-portit (redirectcomports:i:1)
- Älykortin laitteen (redirectsmartcards:i:1)
- Leikepöydän (mahdollisuus kopioiminen ja liittäminen) (redirectclipboard:i:1)
- Poista tyyppi fonttien tasoitus (Salli fonttien tasoitus: i:1)
- Siirtää kaikki tuetut asentamisessa laitteet. (devicestoredirect:s: *)

## <a name="what-other-redirection-is-available"></a>Mitä uudelleenohjaus on saatavilla?
Uudelleenohjauksen vaihtoehdoista on oletusarvoisesti poistettu käytöstä:

- Asema uudelleenohjaus (asema yhdistäminen): paikallisen tietokoneen asemat muuttuvat Etäistunto yhdistetyt asemat. Näillä oikeuksilla voit tallentaa tai tiedostojen avaaminen paikallisen asemista Etäistunto työskentelyn aikana.
- USB-uudelleenohjaus: Voit käyttää paikallisen tietokoneen USB-laitteista Etäistunto kuluessa.

## <a name="change-your-redirection-settings-in-remoteapp"></a>RemoteApp uudelleenohjaus asetusten muuttaminen
Voit muuttaa kokoelma uudelleenohjaus laiteasetusten käyttämällä Microsoft Azure-PowerShell SDK. Kun olet asentanut uuden PowerShellistä ja SDK-paketissa, määritä ensin se Hallitse tilaustasi [asentaminen ja määrittäminen PowerShellin Azure](../powershell-install-configure.md)kuvatulla tavalla.

Mukautetun RDP-ominaisuuksien määrittäminen käyttämällä komentoa, joka on seuraavanlainen:

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*`nusbdevicestoredirect:s:*"

(Huomaa, että *'n* käytetään erottimena yksittäisiä ominaisuuksia välillä).

Saat luettelon käytettävissä minkä mukautetut RDP-ominaisuudet määritetään, suorittamalla seuraavat cmdlet-komento. Huomaa, että vain mukautetut ominaisuudet näkyvät ei oletusominaisuudet sekä tulos tulokset:  

    Get-AzureRemoteAppCollection -CollectionName <collection name>

Kun määrität mukautetut ominaisuudet on määritettävä kaikki mukautetut ominaisuudet aina, kun; Muussa tapauksessa asetus palautuu on poistettu käytöstä.   

### <a name="common-examples"></a>Yleisiä esimerkkejä
Ottaa käyttöön asema uudelleenohjaus seuraavan cmdlet-komennon avulla:  

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*”

Tämä cmdlet-komennon avulla voit ottaa käyttöön sekä USB-asemassa uudelleenohjaus:

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*`nusbdevicestoredirect:s:*"

Käytä tätä cmdlet-komento Leikepöydän jakamisen poistaminen käytöstä:  

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "redirectclipboard:i:0”

> [AZURE.IMPORTANT] Varmista, että kokonaan ulos sivustokokoelman kaikki käyttäjät (ja Katkaise eikä vain ne) ennen kuin kokeilet muutos. Voit varmistaa käyttäjät kirjautuvat kokonaan ulos, siirry Azure-portaalissa sivustokokoelman **istunnot** -välilehti ja käyttäjät, joilla on katkennut tai yrittänyt kirjautua ulos. Joskus kestää joitakin sekunteja paikalliset asemat näyttämään Resurssienhallinnassa istunnossa.

## <a name="change-usb-redirection-settings-on-your-windows-client"></a>Windows-asiakasohjelman USB uudelleenohjaus asetusten muuttaminen

Jos haluat käyttää USB-uudelleenohjauksen, joka yhdistää RemoteApp tietokoneessa, sinun on 2 toiminnot, jotka on tehtävä. 1 - järjestelmänvalvoja on otettava käyttöön USB-uudelleenohjauksen sivustokokoelman tasolla Azure PowerShell-toiminnolla. 2 - laitteissa, joissa haluat käyttää USB-uudelleenohjauksen sinun on otettava käyttöön Ryhmäkäytäntö, joka sallii. Tämä vaihe on valmis jokaiselle käyttäjälle, joka haluaa käyttää USB-uudelleenohjauksen.

> [AZURE.NOTE] Azure RemoteApp USB-uudelleenohjauksesta on tuettu vain Windows-tietokoneissa.

### <a name="enable-usb-redirection-for-the-remoteapp-collection"></a>Ota käyttöön USB-uudelleenohjauksen RemoteApp keräämistä varten
Ottaa käyttöön USB-uudelleenohjauksen sivustokokoelman tasolla seuraavan cmdlet-komennon avulla:

    Set-AzureRemoteAppCollection -CollectionName <collection_name> -CustomRdpProperty "nusbdevicestoredirect:s:*"

### <a name="enable-usb-redirection-for-the-client-computer"></a>Ota käyttöön asiakastietokone USB-uudelleenohjauksen

Voit määrittää tietokoneen USB-uudelleenohjauksen asetukset seuraavasti:

1. Avaa paikallinen ryhmän käytännön editori (GPEDIT. MSC). (Suoritetaan gpedit.msc komentoriviltä.)
2. Avaa **tietokoneen Configuration\Policies\Administrative käyttöön Components\Remote työpöydän Services\Remote työpöydän yhteyden Client\RemoteFX USB laitteen uudelleenohjaus**.
3. Kaksoisnapsauta **Salli RDP uudelleenohjaus muiden tuettujen RemoteFX USB-laitteiden tästä tietokoneesta**.
4. Valitse **käytössä**ja valitse sitten **Järjestelmänvalvojat ja RemoteFX USB uudelleenohjaus käyttöoikeudet käyttäjille**.
5. Avaa komentokehote, jolla on järjestelmänvalvojan oikeudet ja suorittamalla seuraavan komennon:

        gpupdate /force
6. Käynnistä tietokone uudelleen.

Voit myös luominen ja käyttäminen kaikissa tietokoneissa käytäntö USB-uudelleenohjauksen toimialueesi Ryhmäkäytäntöjen hallinta-työkalun avulla:

1. Kirjaudu sisään toimialueen ohjauskoneen toimialueen järjestelmänvalvojana.
2. Avaa ryhmäkäytäntöjen hallintakonsoli. (Valitse **Käynnistä > Valvontatyökalut > Ryhmäkäytäntöjen hallinta**.)
3. Siirry toimialueen tai organisaatioyksikkö, johon haluat luoda käytännön.
4. Napsauta **Toimialueen oletuskäytäntö**hiiren kakkospainikkeella ja valitse sitten **Muokkaa**.
5. Avaa **tietokoneen Configuration\Policies\Administrative käyttöön Components\Remote työpöydän Services\Remote työpöydän yhteyden Client\RemoteFX USB laitteen uudelleenohjaus**.
6. Kaksoisnapsauta **Salli RDP uudelleenohjaus muiden tuettujen RemoteFX USB-laitteiden tästä tietokoneesta**.
7. Valitse **käytössä**ja valitse sitten **Järjestelmänvalvojat ja RemoteFX USB uudelleenohjaus käyttöoikeudet käyttäjille**.
8. Valitse **OK**.  

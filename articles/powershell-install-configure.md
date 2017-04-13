<properties
    pageTitle="Asentaminen ja määrittäminen PowerShellin Azure"
    description="Lue, miten asennetaan ja määritetään PowerShellin Azure."
    editor="tysonn"
    manager="dongill"
    documentationCenter=""
    services=""
    authors="coreyp-at-msft"/>

<tags
    ms.service="multiple"
    ms.workload="multiple"
    ms.tgt_pltfrm="powershell"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="coreyp"/>

# <a name="how-to-install-and-configure-azure-powershell"></a>Asentaminen ja määrittäminen PowerShellin Azure

<div class="dev-center-tutorial-selector sublanding"><a href="/manage/install-and-configure-windows-powershell/" title="PowerShell" class="current">PowerShellin</a><a href="/manage/install-and-configure-cli/" title="Azure CLI">Azure CLI</a></div>

##<a name="what-is-azure-powershell"></a>PowerShellin Azure-ominaisuudet
Azure PowerShell on joukko moduulit, jotka tarjoavat cmdlet-komentojen Azure Windows PowerShellin avulla. Voit Cmdlet-komentoja avulla voit luoda, Testaa, ottaminen käyttöön ja hallita ratkaisuihin ja palveluihin Azure ympäristö kautta. Useimmissa tapauksissa Cmdlet-komentoja voi käyttää samoja toimintoja kuin Azure-portaaliin, kuten luominen ja määrittäminen pilvipalveluihin, näennäiskoneiden, virtual verkkojen ja verkkosovelluksissa.

## <a name="how-versioning-works"></a>Miten versiotietojen hallinta toimii

Azure PowerShell käyttää semanttinen versiotiedot, mikä tarkoittaa, että jos versio A > B versio ja sitten A on API uusin versio. Lisäksi se tarkoittaa sitä, että pääversion keskiarvo uusimmat muutokset muuttaa yhden tai useamman cmdlet-komennot.  Esimerkiksi versio 1.7.0, on korjaustiedoston osoite jakautumisen muutoksen PowerShellin Azure 1.x-versioissa.

Lisätietoja PowerShellin Azure semanttinen versiotietojen käytäntöjä, katso semanttinen versiotietojen määrityksen, milloin: http://semver.org
 
Saat uusimman API-versio kannattaa käyttää 2.x. Mutta jos käytössäsi on kirjoitettu komentosarjoja vastaan versio 1.x, etkä halua, Vaimenna uusimmat muutokset versiossa 2.x kuvattu 2.x [Julkaisutiedot](https://github.com/Azure/azure-powershell/blob/dev/documentation/release-notes/migration-guide.2.0.0.md), ja valitse Asenna 1.7.0.

Versioiden ristiriidan voi aiheuttaa, jos profiili-moduulin uusimman version on asennettu, mutta aiemmassa versiossa, josta se on riippuvainen moduuli ladataan myöhemmin. Asenna uusimmat .msi on helpointa ratkaista ongelman. .Msi tyhjentää automaattisesti moduulit vanhemmat versiot.
 
###<a name="installing-module-versions-side-by-side"></a>Moduulin versiot-rinnakkais asentaminen

Version 2.1.0 (ja AzureStack 1.2.6 versio) ovat ensimmäinen moduuli-versioita, tarkoituksena on oltava asennettuna ja käyttää rinnakkain. Koska PowerShellin Azure käyttää binaarinen moduulit, avaa uuden ikkunan PowerShell ja **Tuo-moduulin** avulla voit tuoda tietyn version AzureRM cmdlet-komennot:

    Import-Module AzureRM -RequiredVersion 2.1.0**

Ennen kuin 2.1.0 (muut kuin 1.2.6) eivät toimi versiot sekä rinnakkais PowerShellin Azure moduulin aiemman version kanssa. Aiemmassa versiossa komennolla edellä kuvattua PowerShellin Azure moduulit ladattaessa **AzureRM.Profile** -moduulia ei ole yhteensopiva versioiden ladataan, tuloksena on cmdlet-komennot, sinulta kysytään, haluatko kirjautua aina, kun suoritat cmdlet-komento, senkin jälkeen, kun olet kirjautunut.

Helpoin tapa korjata tämä on Asenna uusin PowerShellin Azure WebPI syötteen tai .msi – Tämä poistaa asennettu valikoimasta moduulit aiemmissa versioissa. 

Huomaa, että Azure ja AzureRM moduulit riippuvuudet yhteiset, joten jos käytät sekä moduulit päivitettäessä jokin, sinun on päivitettävä molemmat. Aiempien versioiden Azure-moduulin on sama ongelma rinnakkain moduuli ladataan ja AzureRM-moduulissa, aiempien versioiden kanssa on.

<a id="Install"></a>
## <a name="step-1-install"></a>Vaihe 1: asentaminen

Seuraavassa on kaksi tapaa, jolla voit asentaa PowerShellin Azure. Voit asentaa sen PowerShell-valikoiman tai WebPI.

###<a name="installing-azure-powershell-from-the-powershell-gallery"></a>Azure PowerShellin asentaminen PowerShell-valikoimasta

Ensisijainen tapa on PowerShell-valikoimaa. Tarvitset PowerShellGet moduulin PowerShell-valikoimaa. Tämä on saatavilla täällä: [PowerShellGallery.com](https://www.powershellgallery.com/)

Asenna PowerShellin Azure 1.3.0 tai suurempi PowerShell-valikoimasta käyttämällä tulee järjestelmänvalvojana Windows PowerShellin tai PowerShell integroitu komentosarjat ympäristössä (ise:)-kehote käyttämällä seuraavia komentoja:

    # Install the Azure Resource Manager modules from the PowerShell Gallery
    Install-Module AzureRM

    # Install the Azure Service Management module from the PowerShell Gallery
    Install-Module Azure

####<a name="more-about-these-commands"></a>Lue lisää komentoja

- **Asenna moduuli AzureRM** asentaa rollup Azure resurssien hallinnan cmdlet-komennot-moduuli. Määräytyy AzureRM-moduuli 
- tietyn version alueen kunkin Azure Resurssienhallinta-moduulin. Sisältyvää alueen varmistaa, että jakautumisen moduulin muutoksia voidaan sisällyttää asennettaessa AzureRM moduulit saman pääversion. Kun moduuli on AzureRM, minkä tahansa Azure Resurssienhallinta moduuli, jolla ei ole aiemmin asennettu ladattu ja asennettu PowerShell-valikoimasta. Lisätietoja PowerShellin Azure moduulit käyttämä semanttinen versiotiedot on artikkelissa [semver.org](http://semver.org). 
- **Asenna moduuli Azure** asentaa Azure moduuli. Tämä moduuli on PowerShellin Azure-palvelun hallinta-moduulin 0.9.x. Tämä on ei merkittäviä muutoksia ja on vaihdettavissa Azure moduulin aiempaa versiota.

###<a name="installing-azure-powershell-from-webpi"></a>Azure PowerShellin asentaminen WebPI

Asentaminen Azure PowerShell 1.0 ja suurempi WebPI on sama kuin se oli 0.9.x. Lataa [PowerShellin Azure](http://aka.ms/webpi-azps) ja aloita asennus. Jos sinulla on PowerShellin Azure 0.9.x asennettuna, versio 0.9.x poistetaan päivityksen osana. Jos olet asentanut PowerShellin Azure moduulit PowerShell-valikoimasta, asennusohjelma poistaa automaattisesti moduulit ennen asennusta varmistamiseksi yhdenmukaisia PowerShellin Azure-ympäristössä.

> [AZURE.NOTE] Jos olet asentanut Azure moduulit aiemmin PowerShell-valikoimasta, asennusohjelma poistaa niitä automaattisesti. Tällöin sekaannusta tietoja millaiseksi moduuli on asennettu ja missä ne sijaitsevat. PowerShell-valikoiman moduulit asennetaan tavallisesti **%ProgramFiles%\WindowsPowerShell\Modules**. Sen sijaan WebPI asennusohjelma asentaa *Azure moduulit *% ProgramFiles (x86) %\Microsoft SDKs\Azure\PowerShell\**. Jos asennuksen aikana tapahtuu virhe, voit poistaa manuaalisesti Azure* kansioiden **%ProgramFiles%\WindowsPowerShell\Modules** kansio ja yritä asennusta uudelleen.

Kun asennus on valmis, että ```$env:PSModulePath``` asetus on sisällettävä kansioihin, jotka sisältävät Azure PowerShell cmdlet-komentoja.

> [AZURE.NOTE] On tunnettu ongelma PowerShell **$env: PSModulePath** , voi ilmetä, kun sitä asennetaan WebPI. Jos tietokone vaatii uudelleenkäynnistyksen Järjestelmäpäivitykset tai muissa vuoksi, se voi aiheuttaa päivitykset **$env: PSModulePath** et halua käyttää polku, johon on asennettu PowerShellin Azure. Jos näin tapahtuu, voit nähdä "cmdlet-komento ei tunnisteta"-sanoma, kun yrität käyttää Azure PowerShellin cmdlet-komennot, kun asennus tai päivitys. Tässä tapauksessa tietokone olisi ratkaista ongelman.

Jos näyttöön tulee seuraava sanoma, kun yrität ladata tai suorita cmdlet-komennot:

```
    PS C:\> Get-AzureRmResource
    Get-AzureRmResource : The term 'Get-AzureRmResource' is not recognized as the name of a cmdlet, function,
    script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is
    correct and try again.
    At line:1 char:1
    + Get-AzureRmResource
    + ~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : ObjectNotFound: (get-azurermresourcefork:String) [], CommandNotFoundException
        + FullyQualifiedErrorId : CommandNotFoundException
```

Tämän voi korjata tietokone tai tuomalla Cmdlet-komentoja C:\Program Files\WindowsPowerShell\Modules\Azure\XXXX\ seuraavalla tavalla (jossa XXXX on asennettu PowerShell-versio:

```
Import-Module "C:\Program Files\WindowsPowerShell\Modules\Azure\XXXX\azure.psd1"
Import-Module "C:\Program Files\WindowsPowerShell\Modules\Azure\XXXX\expressroute\expressroute.psd1"
```

## <a name="step-2-start"></a>Vaihe 2: Aloita
Voit suorittaa Cmdlet-komentoja vakio Windows PowerShell-konsolin tai -PowerShell integroitu komentosarjat ympäristössä (ise:).
Avaat joko konsolin menetelmä riippuu siitä, käytössäsi on Windows-versio:

- Tietokoneessa, jossa on vähintään Windows 8: ssa tai Windows Server 2012: ssa, voit käyttää sisäinen haku. **Käynnistä** -näytöstä Aloita kirjoittaminen power. Palauttaa arvon kohdistetuissa sovellusluettelosta, joka sisältää Windows PowerShell. Voit avata konsolin, valitsemalla joko sovellus. ( **Aloitus** -näytössä sovelluksen kiinnittäminen kakkospainikkeella-kuvaketta.)

- Tietokoneessa, jossa versio kuin Windows 8: ssa tai Windows Server 2012 käyttämällä **Käynnistä-valikko**. **Käynnistä** -valikosta **Kaikki**ohjelmat, valitse **Apuohjelmat**, valitse **Windows PowerShell** -kansio ja valitse sitten **Windows PowerShell**.

Voit suorittaa **Windows PowerShell ise:** toteuttavat monia samoja tehtäviä, joita suorittaa Windows PowerShell console valikkovaihtoehtojen ja pikanäppäinten avulla. Voit käyttää ise:, Windows PowerShell-konsolin Cmd.exe, tai **Suorita** -ruutuun kirjoittamalla, **powershell_ise.exe**.

###<a name="commands-to-help-you-get-started"></a>Komentoja, jotka auttavat käytön aloittaminen

    # To make sure the Azure PowerShell module is available after you install
    Get-Module –ListAvailable 
    
    # To log in to Azure Resource Manager
    Login-AzureRmAccount

    # You can also use a specific Tenant if you would like a faster log in experience
    # Login-AzureRmAccount -TenantId xxxx

    # To view all subscriptions for your account
    Get-AzureRmSubscription

    # To select a default subscription for your current session
    Get-AzureRmSubscription –SubscriptionName “your sub” | Select-AzureRmSubscription

    # View your current Azure PowerShell session context
    # This session state is only applicable to the current session and will not affect other sessions
    Get-AzureRmContext

    # To select the default storage context for your current session
    Set-AzureRmCurrentStorageAccount –ResourceGroupName “your resource group” –StorageAccountName “your storage account name”

    # View your current Azure PowerShell session context
    # Note: the CurrentStorageAccount is now set in your session context
    Get-AzureRmContext

    # To list all of the blobs in all of your containers in all of your accounts
    Get-AzureRmStorageAccount | Get-AzureStorageContainer | Get-AzureStorageBlob


## <a name="step-3-connect"></a>Vaihe 3: Yhdistä
Cmdlet-komentoja on tilaus, jotta he voivat hallita palvelujen. Voit ostaa Azure tilauksen, jos sinulla ei ole vielä yksi. Ohjeita on artikkelissa [Azure ostaminen](http://go.microsoft.com/fwlink/p/?LinkId=320795).

1. Kirjoita **Login AzureRmAccount**

2. Kirjoita sähköpostiosoite ja salasana tilisi. Azure todentaa ja tallentaa tunnistetietoja ja sulkee ikkunan.

--TAI –

Kirjaudu sisään työpaikan tai oppilaitoksen tiliä:

    $cred = Get-Credential
    Login-AzureRmAccount -Credential $cred
> [AZURE.NOTE] Jos sinulla on useampi kuin yksi vuokraajan organisaation tilisi, Määritä TenantId-parametri:

    $loadersubscription = Get-AzureRmSubscription -SubscriptionName $YourSubscriptionName -TenantId $YourAssociatedSubscriptionTenantId


> [AZURE.NOTE] Tämä ei ole vuorovaikutteinen Kirjaudu sisään-menetelmä toimii vain työpaikan tai oppilaitoksen tiliä. Työpaikan tai oppilaitoksen tili on käyttäjä, jolla on työpaikan tai oppilaitoksen hallitsee ja määritetty töihin tai kouluun Azure Active Directory-esiintymä. Jos ei ole tällä hetkellä työpaikan tai oppilaitoksen tili ja käyttävät Azure tilauksen Kirjaudu Microsoft-tili, voit helposti luoda sen seuraavien ohjeiden mukaisesti.

> 1. Kirjaudu sisään [Azure perinteinen portal](https://manage.windowsazure.com)ja valitse **Active Directory**.

> 2. Jos hakemistoa löytyy, valitse **Luo hakemistossa** ja anna tarvittavat tiedot.

> 3. Valitse hakemisto ja Lisää uusi käyttäjä. Käyttöoikeuden uudelle käyttäjälle kirjautumalla sisään käyttämällä työpaikan tai oppilaitoksen tiliä. Käyttäjän luonnin aikana voit toimitetaan kummankin osoitteen käyttäjän ja tilapäinen salasana. Tallentaa nämä tiedot, kun sitä käytetään kuvatun vaiheen 5 ohjeita.

> 4. Azure perinteinen-portaalista Valitse **asetukset** ja valitse sitten **Järjestelmänvalvojat**. Valitse **Lisää**ja Lisää uusi käyttäjä työtovereiden järjestelmänvalvojana. Näin voit hallita Azure tilauksen työpaikan tai oppilaitoksen tiliä.

> 5. Lopuksi Kirjaudu ulos Azure perinteinen portaalin ja kirjaudu sitten takaisin sisään käyttämällä työ tai oppilaitoksen tilillä. Jos ensimmäisen kerran sisäänkirjautumisessa tämän tilillä, voit pyytää salasanan vaihtaminen.

> Lisätietoja Microsoft Azure rekisteröityminen työpaikan tai oppilaitoksen tilillä on artikkelissa [Microsoft Azure organisaationa rekisteröityminen](./active-directory/sign-up-organization.md).

> Saat lisätietoja Azure todennus- ja tilausasioiden hallinnan [tilien hallinta- ja tilaukset-järjestelmänvalvojan roolit](http://go.microsoft.com/fwlink/?LinkId=324796).

### <a name="view-account-and-subscription-details"></a>Tilin ja tilauksen tietojen tarkasteleminen

Voit määrittää useita tilejä ja tilaukset, jotka ovat käytettävissä PowerShellin Azure. Voit lisätä useita tilejä suorittamalla **Lisää AzureRmAccount** useita kertoja.

Jos haluat käytettävissä olevat Azure tilit, kirjoita **Hae AzureAccount**.

Jos haluat Azure tilauksistasi, kirjoita **Hae AzureRmSubscription**.

##<a id="Help"></a>Ohjeiden saaminen##

Nämä resurssit antaa tietyn cmdlet-komennot:


-   Konsolissa, voit käyttää valmiita ohjeessa. **Hanki ohjeita** cmdlet-komento on pääsy järjestelmän. 

- Ohjeita yhteisöstä Kokeile näitä Suositut keskustelupalstoilla:

 - [Azure MSDN-keskustelupalsta]( http://go.microsoft.com/fwlink/p/?LinkId=320212)
 - [Stackoverflow](http://go.microsoft.com/fwlink/?LinkId=320213)

##<a name="learn-more"></a>Opi lisää


Katso lisätietoja cmdlet-komentojen käyttämisestä on seuraavissa resursseissa:

Lisätietoja basic Windows PowerShellin avulla on artikkelissa [Windows PowerShellin](http://go.microsoft.com/fwlink/p/?LinkId=321939).

Cmdlet-komentoja Lisätietoja on artikkelissa [Azure Cmdlet-viittaus](https://msdn.microsoft.com/library/windowsazure/jj554330.aspx).

Komentosarjamallit ja ohjeiden avulla voit hallita Azure komentosarjan avulla on artikkelissa [Komentosarjat](http://go.microsoft.com/fwlink/p/?LinkId=321940).


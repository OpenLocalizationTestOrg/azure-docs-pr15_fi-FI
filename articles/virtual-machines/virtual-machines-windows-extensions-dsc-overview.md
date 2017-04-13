<properties
   pageTitle="Haluttu Azure yleisiä tilan määritys | Microsoft Azure"
   description="Yleistä Microsoft Azure-tunniste käsittelijä käytön PowerShell haluttu tila-määritys. Esimerkiksi edellytykset, arkkitehtuuri ja cmdlet-komennot..."
   services="virtual-machines-windows"
   documentationCenter=""
   authors="zjalexander"
   manager="timlt"
   editor=""
   tags="azure-service-management,azure-resource-manager"
   keywords=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="na"
   ms.date="09/15/2016"
   ms.author="zachal"/>

# <a name="introduction-to-the-azure-desired-state-configuration-extension-handler"></a>Johdanto Azure toivottuja tilan määritys tunniste käsittely #

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Azure AM agentti ja liittyvät laajennukset kuuluvat Microsoft Azure-infrastruktuuripalvelut. AM laajennukset ovat AM laajentamaan ja yksinkertaistaa eri AM hallintatoiminnot ohjelmisto-osat. Esimerkiksi VMAccess-tunniste voidaan käyttää järjestelmänvalvojan salasanan tai mukautettu komentosarja-tunniste voidaan suorittaa komentosarjan AM.

Tässä artikkelissa esitellään PowerShell toivottuja tilan määrittäminen (DSC)-tunniste, Azure VMs Azure PowerShell SDK osana. Voit ladata ja käyttää PowerShell DSC määritysten käytössä PowerShell DSC-tunniste Azure-AM uuden cmdlet-komennot. PowerShell DSC tunniste kutsuu PowerShell DSC antaa vastaanotetut DSC kokoonpano AM. Tämä toiminto on myös käytettävissä palvelun Azure-portaalissa.

## <a name="prerequisites"></a>Edellytykset ##
**Paikallisessa tietokoneessa** Haluat ottaa yhteyttä Azure AM-tunniste Azure portaalin tai Azure PowerShell SDK. 

**Vieras-agentti** Azure-AM, joka on määritetty DSC määrityksestä on oltava OS, joka tukee joko Windows Management Framework (WMF) 4.0 ja 5.0. Täydellinen luettelo tuetuista OS versioista tukikäytännöistä [DSC tunniste versiohistoria](https://blogs.msdn.microsoft.com/powershell/2014/11/20/release-history-for-the-azure-dsc-extension/).

## <a name="terms-and-concepts"></a>Termejä ja käsitteitä ##
Tämän oppaan presumes seuraavia käsitteitä tuntemusta:

Määritys - DSC määritysten asiakirjan. 

Solmu - DSC määrityksen kohde. Tässä asiakirjassa-solmu"viittaa aina Azure-AM.

Määritysten tiedot – .psd1 tiedoston sisältävä ympäristön tiedot määrityksen

## <a name="architectural-overview"></a>Arkkitehtuuri yleiskatsaus ##

Azure DSC-tunniste käyttää Azure AM agentti framework aikana, yksinoikeuksia ja raportoinnissa DSC käyttömahdollisuudet Azure VMs käytössä. DSC-tunniste odottaa .zip-tiedostoon, joka sisältää vähintään määritysten asiakirjan ja Azure PowerShell SDK kautta tai palvelun Azure-portaalissa parametrien määrittäminen.

Tunnisteen kutsutaan ensimmäistä kertaa, kun se suoritetaan asentunut. Tämä toimenpide asentaa version, Windows Management Framework (WMF) seuraava logiikan avulla:

1. Jos Azure AM Käyttöjärjestelmä on Windows Server 2016, mitään toimia ei oteta. Windows Server 2016 on jo asennettu PowerShell uusimman version.
2. Jos `wmfVersion` ominaisuus on määritetty, WMF versio on asennettu paitsi, jos se ei ole yhteensopiva AM OS.
3. Jos `wmfVersion` ominaisuus on määritetty, uusimmat sovellettavan version WMF on asennettu.

Asennus WMF on viimeisteltävä. Uudelleenkäynnistyksen jälkeen tunnisteen lataukset-parametrissa .zip-tiedosto `modulesUrl` ominaisuus. Jos tämä sijainti on Azure-blob-säiliö, SAS tunnuksen voi erikseen `sasToken` ominaisuus käyttää tiedostoa. Kun .zip ladataan ja avattaviksi, määritys-funktion määritelty `configurationFunction` suoritetaan MOF-tiedoston luomiseen. Tunnisteen suorittaa `Start-DscConfiguration -Force` luotu MOF-tiedoston avulla. Tunnisteen sieppaa tulostus ja se takaisin Azure tila kanavaan kirjoituksia. Tästä lähtien DSC pienin.yht.JAETTAVA käsittelee seuranta ja korjaus normaalisti. 

## <a name="powershell-cmdlets"></a>PowerShellin cmdlet-komennot ##

PowerShellin cmdlet-komennot voidaan ARM tai ASM pakata, julkaiseminen ja seurata DSC tunniste ominaisuuksissa. Luettelossa seuraavat cmdlet-komennot ovat ASM moduulit, mutta "Azure" voidaan korvata "AzureRm" käyttämään ARM-malli. Esimerkiksi `Publish-AzureVMDscConfiguration` käyttää ASM, jossa `Publish-AzureRmVMDscConfiguration` käyttää ARM. 

`Publish-AzureVMDscConfiguration`määritystiedostossa on etsii riippuvaiset DSC resurssit ja luo sisältävä määritys ja tarvittavien antaa määritykset DSC resurssien .zip-tiedosto. Voit myös luoda paikallisesti käyttämällä paketti `-ConfigurationArchivePath` parametria. Muussa tapauksessa julkaisee .zip-tiedosto Azure-blob-säiliö ja suojaaminen SAS-tunnuksen.

Cmdlet luoma .zip-tiedosto on .ps1 määritysten komentosarja sitten arkistokansio ylimmällä tasolla. Resursseja on moduuli-kansio sijoitetaan arkisto-kansioon. 

`Set-AzureVMDscExtension`injects asetuksia, jotka tarvitaan PowerShell DSC tunnisteen mukaan AM määritysten objektiin, joka voi suojata sitten Azure-AM kanssa `Update-AzureVM`.

`Get-AzureVMDscExtension`hakee tietyn AM DSC tunniste tila. 

`Get-AzureVMDscExtensionStatus`hakee tilan mukaan DSC tunniste käsittelijä säädetty DSC-määritys. Tämä toiminto voidaan suorittaa yhteen AM tai VMs ryhmän.

`Remove-AzureVMDscExtension`poistaa tunnisteen tapahtumakäsittelijän annetun virtual tietokoneesta. Tämä cmdlet-komento **Poista määritys-** asennuksen WMF tai muuta virtuaalikoneen käytettävät asetukset. Se poistaa vain tunniste käsittelijä. 

**Keskeisiä eroja ASM ja ARM cmdlet-komennot**

- ARM-cmdlet-komennot ovat synkronoitu. ASM cmdlet-komennot ovat asynkroninen.
- ResourceGroupName, VMName, ArchiveStorageAccountName, versio ja sijainti ovat kaikki uudet tarvittavat parametrit.
- ArchiveResourceGroupName on ARM uuden valinnainen parametrin. Voit määrittää tämän parametrin tallennustilan-tiliisi kuuluu eri resurssiryhmä kuin näkymän jossa virtuaalikoneen on luotu.
- ConfigurationArchive kutsutaan ArchiveBlobName KÄDESSÄ
- ContainerName kutsutaan ArchiveContainerName KÄDESSÄ
- StorageEndpointSuffix kutsutaan ArchiveStorageEndpointSuffix KÄDESSÄ
- AutoUpdate-valitsin on lisätty tunnisteen käsittelijä kuin uusimpaan versioon ja kun se on käytettävissä Automaattiset päivitykset käyttöön ARM. Tämä parametri on saattaa aiheuttaa päärakenteen käynnistyy AM, kun on julkaistu uusi versio WMF. 


## <a name="azure-portal-functionality"></a>Azure portaalin toiminnot ##
Siirry perinteiseen AM. Valitse Asetukset -> Yleiset napsauttamalla "Laajennukset." Uusi ruutu luodaan. "Lisää" ja valitse PowerShell DSC.

Portaalin tarvitsee syötteen.
**Määritysten moduulit tai komentosarjan**: Tämä kenttä on pakollinen. Vaatii .ps1 tiedosto, joka sisältää kokoonpanon komentosarjaa tai .zip-tiedosto, jonka .ps1 määritysten komentosarjan pääkansio ja kaikki riippuvaiset resurssit .zip moduulin kansiot. Se voidaan luoda `Publish-AzureVMDscConfiguration -ConfigurationArchivePath` sisältyvät Azure PowerShell SDK cmdlet-komento. .Zip-tiedosto ladataan suojattu SAS-tunnuksen lisääminen käyttäjän Blob-objektien tallennustilaan. 

**Tietojen PSD1 kokoonpanotiedosto**: Tämä kenttä on valinnainen. Jos kokoonpanosi edellyttää .psd1 määritysten tietojen tiedoston, käytä tätä kenttää napsauttamalla sitä ja lataa se käyttäjän-Blob-objektien tallennustilaan kohtaa, johon se on suojattu SAS tunnuksen mukaan. 
 
**Module-Qualified nimien määritykset**: .ps1 tiedostoja voi olla useita määritys-funktioita. Määritysten .ps1 komentosarja ja sen jälkeen nimi "\' ja määritys-funktion nimi. Esimerkiksi jos .ps1-komentosarja on nimi "configuration.ps1" ja "IisInstall" on, sinun on kirjoitettava:`configuration.ps1\IisInstall`

**Määritysten argumenttien**: Jos määritys-funktio ohittaa argumentit, kirjoita ne tässä muodossa `argumentName1=value1,argumentName2=value2`. Huomautus Tämä muoto on eri muodossa kuin miten määritysten argumentit hyväksytään PowerShellin cmdlet-komennot tai Resurssienhallinta malleja. 

## <a name="getting-started"></a>Käytön aloittaminen ##

Azure DSC-tunniste tulee DSC määritysten asiakirjoissa, ja enacts ne Azure VMs käyttöön. Esimerkki määrityksen seuraa. Tallenna paikallisesti "IisInstall.ps1":

```powershell
configuration IISInstall 
{ 
    node "localhost"
    { 
        WindowsFeature IIS 
        { 
            Ensure = "Present" 
            Name = "Web-Server"                       
        } 
    } 
}
```

Seuraavat vaiheet IisInstall.ps1 komentosarja sijoittaminen määritetyn AM, suorittaa määritykset ja takaisin tilasta raportointi.
 
```powershell
#Azure PowerShell cmdlets are required
Import-Module Azure

#Use an existing Azure Virtual Machine, 'DscDemo1'
$demoVM = Get-AzureVM DscDemo1

#Publish the configuration script into user storage.
Publish-AzureVMDscConfiguration -ConfigurationPath ".\IisInstall.ps1" -StorageContext $storageContext -Verbose -Force

#Set the VM to run the DSC configuration
Set-AzureVMDscExtension -VM $demoVM -ConfigurationArchive "demo.ps1.zip" -StorageContext $storageContext -ConfigurationName "runScript" -Verbose

#Update the configuration of an Azure Virtual Machine
$demoVM | Update-AzureVM -Verbose

#check on status
Get-AzureVMDscExtensionStatus -VM $demovm -Verbose
```

## <a name="logging"></a>Lokiin kirjaaminen ##

Lokit sijoitetaan:

C:\WindowsAzure\Logs\Plugins\Microsoft.PowerShell.DSC\[versionumero]

## <a name="next-steps"></a>Seuraavat vaiheet ##

Lisätietoja PowerShell-DSC [PowerShell dokumentaatio Centeristä](https://msdn.microsoft.com/powershell/dsc/overview). 

Tarkastele [Azure Resurssienhallinta mallia DSC-tunniste](virtual-machines-windows-extensions-dsc-template.md
). 

Voit etsiä lisätoimintoja voit hallita PowerShell DSC, [Selaa PowerShell-valikoiman](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) DSC lisämateriaalia kanssa.

Lisätietoja luottamuksellisia parametrien siirtäminen kyselyjä määritykset on artikkelissa [hallinta tunnistetiedot turvallisesti DSC tunniste käsittelijä sekä](virtual-machines-windows-extensions-dsc-credentials.md).

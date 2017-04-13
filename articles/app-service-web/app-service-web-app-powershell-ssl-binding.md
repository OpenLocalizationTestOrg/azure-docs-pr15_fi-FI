<properties
    pageTitle="SSL-varmenteita sidonta PowerShellin avulla"
    description="Lue, miten haluat sitoa SSL-varmenteita koodiin PowerShellin avulla."
    services="app-service\web"
    documentationCenter=""
    authors="ahmedelnably"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="01/13/2016"
    ms.author="ahmedelnably"/>

# <a name="azure-app-service-ssl-certificate-binding-using-powershell"></a>Azure App SSL varmenteen sidonta PowerShellin avulla #

Microsoft Azure PowerShell versio 1.1.0 versiossa uusi cmdlet-komento on lisätty, jotka antaa käyttäjälle mahdollisuus sitoa olemassa oleva vai uusi SSL-varmenteita aiemmin Web App-sovelluksessa.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

Lisätietoja käyttämisestä Azure Resurssienhallinta perusteella Azure PowerShell cmdlet-komentojen Web Apps-sovellusten Tarkista [Azure Resurssienhallinta perusteella PowerShell-komennot Azure Web App](app-service-web-app-azure-resource-manager-powershell.md)

## <a name="uploading-and-binding-a-new-ssl-certificate"></a>Lataaminen ja sidonta uusi SSL-varmenne ##

Skenaario: Käyttäjä haluat sitoa SSL-varmenteen johonkin hänen verkkosovelluksissa.

Tietää resurssin ryhmänimi, joka sisältää web-sovelluksen, web-sovelluksen nimi, käyttäjän tietokoneen, varmenne ja mukautetun isäntänimi salasanan sertifikaatin .pfx tiedostopolku Microsoft käyttää seuraavaa PowerShell-komentoa, SSL-sidonta luominen:

    New-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -CertificateFilePath PathToPfxFile -CertificatePassword PlainTextPwd -Name www.contoso.com

Huomaa, että ennen niiden lisäämistä verkkosovellukseen SSL-sidonta, on oltava isännän nimi (mukautettu toimialue), joka on jo määritetty. Jos isäntänimi ei ole määritetty, niin saat virheilmoituksen "hostname" ei ole uusi AzureRmWebAppSSLBinding käytön aikana. Voit lisätä isäntänimi suoraan portaalin tai Azure PowerShellin avulla. Seuraavat PowerShell-koodikatkelman voi määrittää isäntänimi ennen kuin suoritat uusi AzureRmWebAppSSLBinding.   
  
    $webApp = Get-AzureRmWebApp -Name mytestapp -ResourceGroupName myresourcegroup  
    $hostNames = $webApp.HostNames  
    $HostNames.Add("www.contoso.com")  
    Set-AzureRmWebApp -Name mytestapp -ResourceGroupName myresourcegroup -HostNames $HostNames   
  
On tärkeää ymmärtää, että määrittäminen AzureRmWebApp cmdlet-komento korvaa isäntänimet web-sovelluksen. Näin ollen yllä PowerShell-koodikatkelman liitettäväksi web-sovelluksen isäntänimet nykyistä luetteloa.  

## <a name="uploading-and-binding-an-existing-ssl-certificate"></a>Lataaminen ja sidonta SSL-varmenne ##

Skenaario: Käyttäjä haluat sitoa aiemmin ladatut SSL-varmenteen johonkin hänen verkkosovelluksissa.

Emme voi käyttää jo ladattu tietyn resurssiryhmän käyttämällä seuraavaa komentoa varmenteiden luettelo

    Get-AzureRmWebAppCertificate -ResourceGroupName myresourcegroup

Huomaa, että varmenteet ovat paikallisia tiettyyn paikkaan ja resurssiryhmä, käyttäjän on ladattava varmenteen uudelleen, jos määritetty web-sovellus on eri sijainnissa ja resurssien ryhmitteleminen muiden, joka tarvittava todistus 

Tietää resurssin ryhmänimi, joka sisältää web-sovelluksen, web-sovelluksen nimi, varmenteen allekirjoitus ja mukautetun hostname Microsoft käyttää seuraavaa PowerShell-komentoa, SSL-sidonta luominen:

    New-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Thumbprint <certificate thumbprint> -Name www.contoso.com

## <a name="deleting-an-existing-ssl-binding"></a>Olemassa olevan SSL-sidonta poistaminen  ##

Skenaario: Käyttäjä haluat poistaa aiemmin SSL-sidonta.

Tietää resurssin ryhmänimi, joka sisältää web-sovelluksen, web-sovelluksen nimi ja mukautetun hostname Microsoft käyttää PowerShell-komennolla voit poistaa kyseisen SSL-sidonta:

    Remove-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Name www.contoso.com

Huomaa, että jos poistettu SSL-sidonta on viimeksi sidonta käyttämällä sertifikaatin, sijainti, oletusarvoisesti varmenne poistetaan, jos käyttäjä haluat säilyttää sertifikaatin hän Säilytä varmenteen DeleteCertificate-vaihtoehdon avulla

    Remove-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Name www.contoso.com -DeleteCertificate $false

### <a name="references"></a>Viittaukset ###
- [Azure Resurssienhallinta perusteella Azure-verkkosovelluksessa PowerShell-komennot](app-service-web-app-azure-resource-manager-powershell.md)
- [Johdanto App Service-ympäristöön](app-service-app-service-environment-intro.md)
- [Azure PowerShell kanssa Azure resurssien hallinnan avulla](../powershell-azure-resource-manager.md)

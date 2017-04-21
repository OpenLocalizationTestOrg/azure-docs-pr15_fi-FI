<properties services="virtual-machines" title="Setting up PowerShell" authors="JoeDavies-MSFT" solutions="" manager="timlt" editor="tysonn" />

<tags
   ms.service="virtual-machines"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm=""
   ms.workload="infrastructure"
   ms.date="05/12/2015"
   ms.author="rasquill" />

## <a name="setting-up-powershell"></a>PowerShellin määrittäminen

Ennen kuin voit käyttää PowerShellin Azure, toimi seuraavasti.

### <a name="verify-powershell-versions"></a>Tarkista PowerShell-versiot

Ennen kuin voit käyttää Windows PowerShellin, sinulla on oltava Windows PowerShell-versiota 3.0 tai 4.0. Etsi Windows PowerShell-version, kirjoita komentokehotteeseen Windows PowerShell komento.

    $PSVersionTable

Raportissa pitäisi näkyä seuraavankaltaiselta.

    Name                           Value
    ----                           -----
    PSVersion                      3.0
    WSManStackVersion              3.0
    SerializationVersion           1.1.0.1
    CLRVersion                     4.0.30319.18444
    BuildVersion                   6.2.9200.16481
    PSCompatibleVersions           {1.0, 2.0, 3.0}
    PSRemotingProtocolVersion      2.2

Varmista, että **PSVersion** arvo on 3.0 tai 4.0. Asenna yhteensopiva versio-kohdassa [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) - tai [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855).

Sinulla on myös PowerShellin Azure 0.8.0 versio tai sitä uudemmalla versiolla. Voit tarkistaa, jotka ovat asentaneet tällä komennolla PowerShellin Azure komentokehotteen PowerShellin Azure-version.

    Get-Module azure | format-table version

Raportissa pitäisi näkyä seuraavankaltaiselta.

    Version
    -------
    0.8.16.1

Saat ohjeet ja linkin uusimman version [asentaminen ja määrittäminen PowerShellin Azure](powershell-install-configure.md).


### <a name="set-your-azure-account-and-subscription"></a>Azure-tili ja tilauksen määrittäminen

Jos sinulla ei vielä ole Azure tilaus, voit ottaa [MSDN tilaajan edut](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) tai sisäänkirjautumisen määrittäminen [maksuttoman kokeiluversion käyttäjäksi](https://azure.microsoft.com/pricing/free-trial/).

Avaa komentokehote PowerShellin Azure ja kirjaudu Azure tällä komennolla.

    Add-AzureAccount

Jos sinulla on useita tilauksia Azure, voit lisätä Azure tilauksistasi tällä komennolla.

    Get-AzureSubscription

Näyttöön tulee seuraava tietotyyppi:

    SubscriptionId            : fd22919d-eaca-4f2b-841a-e4ac6770g92e
    SubscriptionName          : Visual Studio Ultimate with MSDN
    Environment               : AzureCloud
    SupportedModes            : AzureServiceManagement,AzureResourceManager
    DefaultAccount            : johndoe@contoso.com
    Accounts                  : {johndoe@contoso.com}
    IsDefault                 : True
    IsCurrent                 : True
    CurrentStorageAccountName : 
    TenantId                  : 32fa88b4-86f1-419f-93ab-2d7ce016dba7

Voit määrittää nykyisen Azure tilauksesi suorittamalla PowerShellin Azure komentokehotteeseen seuraavat komennot. Korvaa kaiken tarjoukset, mukaan lukien < ja > merkkejä, oikea nimi.

    $subscr="<SubscriptionName from the display of Get-AzureSubscription>"
    Select-AzureSubscription -SubscriptionName $subscr -Current 

Saat lisätietoja Azure tilaukset ja -tilien [Toimintaohje: yhdistäminen tilauksen](powershell-install-configure.md#Connect).

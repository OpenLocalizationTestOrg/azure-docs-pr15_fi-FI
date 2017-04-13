## <a name="setting-up-powershell-for-resource-manager-templates"></a>Resurssienhallinta malleja PowerShell määrittäminen

Ennen kuin voit käyttää resurssien hallinta PowerShellin Azure-on pystyttävä muodostamaan oikea Windows PowerShell- ja PowerShellin Azure-versioissa.

### <a name="verify-powershell-versions"></a>Tarkista PowerShell-versiot

Tarkista, sinulla on Windows PowerShellin 3.0 tai 4.0-versio. Etsi Windows PowerShell-version, kirjoita komentokehotteeseen Windows PowerShell komento.

    $PSVersionTable

Näyttöön tulee seuraava tietotyyppi:

    Name                           Value
    ----                           -----
    PSVersion                      3.0
    WSManStackVersion              3.0
    SerializationVersion           1.1.0.1
    CLRVersion                     4.0.30319.18444
    BuildVersion                   6.2.9200.16481
    PSCompatibleVersions           {1.0, 2.0, 3.0}
    PSRemotingProtocolVersion      2.2


Varmista, että **PSVersion** arvo on 3.0 tai 4.0. Jos et, katso [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) - tai [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855).

### <a name="set-your-azure-account-and-subscription"></a>Azure-tili ja tilauksen määrittäminen

Jos sinulla ei vielä ole Azure tilaus, voit ottaa [MSDN tilaajan edut](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) tai sisäänkirjautumisen määrittäminen [maksuttoman kokeiluversion käyttäjäksi](https://azure.microsoft.com/pricing/free-trial/).

Avaa komentokehote PowerShellin Azure ja kirjaudu Azure tällä komennolla.

    Login-AzureRmAccount

Jos sinulla on useita tilauksia Azure, voit lisätä Azure tilauksistasi tällä komennolla.

    Get-AzureRmSubscription

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

    $subscr="<SubscriptionName from the display of Get-AzureRmSubscription>"
    Select-AzureRmSubscription -SubscriptionName $subscr -Current

Saat lisätietoja Azure tilaukset ja -tilien [Toimintaohje: yhdistäminen tilauksen](powershell-install-configure.md#Connect).

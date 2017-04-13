<properties
    pageTitle="Access-muuta historia-raportin luominen | Microsoft Azure"
    description="Luo raportti, jossa on luettelo kaikista muutoksista Accessissa Azure-tilauksia ja Roolipohjainen käyttöoikeuksien valvonta viimeisten 90 päivän aikana."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/03/2016"
    ms.author="kgremban"/>

# <a name="create-an-access-change-history-report"></a>Access-muuta historia-raportin luominen

Milloin tahansa joku myöntää tai poistaa oikeus tilauksistasi, muutokset Hae kirjautunut sisään Azure tapahtumat. Voit luoda access muuta näet kaikki viimeisten 90 päivän historia-raportit.

## <a name="create-a-report-with-azure-powershell"></a>Raportin luominen Azure PowerShellin avulla
Voit luoda access muuta historia-raportin PowerShell `Get-AzureRMAuthorizationChangeLog` komento. Lisätietoja cmdlet ovat käytettävissä [PowerShell-valikoimassa](https://www.powershellgallery.com/packages/AzureRM.Storage/1.0.6/Content/ResourceManagerStartup.ps1).

Kun soitat tätä komentoa, voit määrittää mitä ominaisuutta luettelossa, mukaan lukien seuraavat tehtävät:

| Ominaisuus | Kuvaus |
| -------- | ----------- |
| **Toiminto** | Onko access myöntää tai kumota |
| **Soittajan** | Access-muuta vastuussa omistaja |
| **Päivämäärä** | Päivämäärän ja kellonajan, access on muutettu |
| **Toiminta** | Azure Active Directory-hakemistosta |
| **PrincipalName** | Käyttäjän, ryhmän tai sovelluksen nimi |
| **PrincipalType** | Onko tehtävä on käyttäjän, ryhmän tai sovelluksen |
| **RoleId** | Rooli, joka on myöntänyt tai kumottu GUID-tunnus |
| **RoleName** | Rooli, joka on myöntänyt tai kumoaminen |
| **ScopeName** | Tilauksen, resurssiryhmä tai resurssin nimi |
| **ScopeType** | Onko tehtävä on tilaus, resurssiryhmä vai resurssin laajuus |
| **SubscriptionId** | Azure tilauksen GUID-tunnus |
| **SubscriptionName** | Azure tilauksen nimeä |

Tämän esimerkin-komento on lueteltu kaikki tilauksen viimeisten seitsemän päivän access muutokset:

```
Get-AzureRMAuthorizationChangeLog -StartTime ([DateTime]::Now - [TimeSpan]::FromDays(7)) | FT Caller,Action,RoleName,PrincipalType,PrincipalName,ScopeType,ScopeName
```

![PowerShellin Get-AzureRMAuthorizationChangeLog - näyttökuva](./media/role-based-access-control-configure/access-change-history.png)

## <a name="create-a-report-with-azure-cli"></a>Raportin luominen Azure CLI
Access muuta historia-raportin luominen Azure komentorivivalitsimet interface (CLI), käytä `azure role assignment changelog list` komento.

## <a name="export-to-a-spreadsheet"></a>Vie laskentataulukkoon
Tallenna raportti tai tietojen muokkaamiseen, Vie .csv-tiedoston tuominen access-muutokset. Voit tarkastella raportin sitten laskentataulukon tarkistettavaksi.

![Changelog tarkastella laskentataulukkona - näyttökuva](./media/role-based-access-control-configure/change-history-spreadsheet.png)

## <a name="see-also"></a>Katso myös
- Aloita [Azure Role-Based käyttöoikeuksien valvonta](role-based-access-control-configure.md)
- [Mukautettu roolien Azure RBAC](role-based-access-control-custom-roles.md) käsitteleminen

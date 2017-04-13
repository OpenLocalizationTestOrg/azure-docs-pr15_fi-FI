<properties
    pageTitle="Azure Active Directory-PowerShell esikatselu cmdlet-komennot Azure AD-ryhmän hallinta | Microsoft Azure"
    description="Tämä sivu sisältää PowerShell esimerkkejä, joiden avulla voit hallita Azure Active Directory-ryhmien"
    keywords="Azure AD-Azure Active Directory-PowerShell-ryhmä hallinta"
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/29/2016"
    ms.author="curtand"/>

# <a name="azure-active-directory-preview-cmdlets-for-group-management"></a>Azure Active Directory esikatselu cmdlet-komennot ryhmän hallinta

> [AZURE.SELECTOR]
- [Azure portal](active-directory-groups-create-azure-portal.md)
- [Azure perinteinen portal](active-directory-accessmanagement-manage-groups.md)
- [PowerShellin](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)

Seuraava asiakirja on esimerkkejä Azure Active Directory (Azure AD) ryhmien hallinta PowerShellin avulla on.  On myös tietoja siitä, miten voit määrittää esikatselu PowerShellin Azure AD-moduulin. Ensin sinun täytyy [ladata PowerShellin Azure AD-moduulin](http://go.microsoft.com/fwlink/p/?LinkId=828627).

## <a name="installing-the-azure-ad-powershell-module"></a>PowerShellin Azure AD-moduulin asentaminen

Asenna AzureAD PowerShell esikatselu-moduulissa, käytä seuraavia komentoja:

    PS C:\Windows\system32> install-module azureadpreview

Voit varmistaa, että esikatselu-moduuli on asennettu, käytä seuraavaa komentoa:

    PS C:\Windows\system32> get-module azureadpreview

    ModuleType Version    Name                                ExportedCommands
    ---------- -------    ----                                ----------------
    Binary     1.1.146.0  azureadpreview                      {Add-AzureADAdministrati...}

Voit nyt aloittaa käyttäminen moduulin Cmdlet-komentoja. Cmdlet-komentoja AzureAD esikatselu-moduulin koko kuvauksen Tutustu [online-oppaat](https://msdn.microsoft.com/library/azure/mt757216.aspx).

## <a name="connecting-to-the-directory"></a>Yhteyden muodostaminen kansioon
Ennen kuin voit aloittaa PowerShellin Azure AD esikatselu cmdlets-komennoista ryhmien hallinta, yhdistettävä PowerShell-istunnon haluat hallita hakemiston. Käytä seuraava komento:

    PS C:\Windows\system32> Connect-AzureAD -Force

Cmdlet pyytää haluat käyttää hakemiston tunnistetiedot. Tässä esimerkissä on käytössä karen@drumkit.onmicrosoft.com käyttöoikeuksia esittely-kansioon. Cmdlet palauttavat confirmation näyttämään istunnon on liitetty onnistuneesti hakemistossa:

    Account                       Environment Tenant
    -------                       ----------- ------
    Karen@drumkit.onmicrosoft.com AzureCloud  85b5ff1e-0402-400c-9e3c-0f…

Nyt voit aloittaa AzureAD esikatselu cmdlet-komentojen avulla voit hallita ryhmiä hakemistossa.

## <a name="retrieving-groups"></a>Ryhmien hakeminen
Olemassa olevia ryhmiä noutaa hakemistossa voit saada AzureADGroups cmdlet-komento. Noutaa kaikki ryhmät hakemistossa, käytä cmdlet-komento ilman parametreja:

    PS C:\Windows\system32> get-azureadgroup

Cmdlet palauttavat kaikki ryhmät yhdistetyn hakemistossa.

Voit hakea tietyn ryhmän, jonka voit määrittää ryhmän objektitunnus objektitunnus - parametrin:

    PS C:\Windows\system32> get-azureadgroup -ObjectId e29bae11-4ac0-450c-bc37-6dae8f3da61b

Cmdlet palauttaa ryhmän, jonka objektitunnus vastaa antamiasi parametrin arvo:

    DeletionTimeStamp            :
    ObjectId                     : e29bae11-4ac0-450c-bc37-6dae8f3da61b
    ObjectType                   : Group
    Description                  :
    DirSyncEnabled               :
    DisplayName                  : Pacific NW Support
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 9bb4139b-60a1-434a-8c0d-7c1f8eee2df9
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

Voit etsiä tietyn ryhmän käyttämällä suodatin-parametrin. Tämä parametri on ODATA-suodatinlausekkeen ja palauttaa kaikki ryhmät, jotka vastaavat suodatinta, kuten seuraavassa esimerkissä:

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

Huomaa, että AzureAD PowerShellin cmdlet-komennot Toteuta OData-kysely-standardia, löytyy lisätietoja [tästä](https://msdn.microsoft.com/library/gg309461.aspx#BKMK_filter).

## <a name="creating-groups"></a>Ryhmien luominen
Voit luoda uuden ryhmän hakemistossa käyttämällä uutta AzureADGroup cmdlet-komento. Tämä cmdlet-komento luo uusi käyttöoikeusryhmän "Markkinointi"-niminen:

    PS C:\Windows\system32> New-AzureADGroup -Description "Marketing" -DisplayName "Marketing" -MailEnabled $false -SecurityEnabled $true -MailNickName "Marketing"

## <a name="updating-groups"></a>Ryhmien päivittäminen
Jos haluat päivittää aiemmin luotuun ryhmään, käytä määrittäminen AzureADGroup cmdlet-komento. Tässä esimerkissä on haluat vaihtaa näyttönimi-ominaisuuden ryhmän "Intune järjestelmänvalvojia." Ensin on olet etsiminen ryhmän Hae AzureADGroup cmdlet-komennolla ja suodattaa näyttönimi-määritteen avulla:

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

Seuraavaksi on haluat vaihtaa kuvaus-ominaisuuden uuteen arvoon "Intune laitteen Järjestelmänvalvojat":

    PS C:\Windows\system32> Set-AzureADGroup -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -Description "Intune Device Administrators"

Nyt on löytänyt haluamasi ryhmän uudelleen-kohdassa näkyy kuvaus-ominaisuuden päivitetään uusi arvo:

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Device Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

## <a name="deleting-groups"></a>Ryhmien poistaminen
Ryhmien poistaminen hakemistossa, käytä Poista AzureADGroup cmdlet-komennolla seuraavasti:

    PS C:\Windows\system32> Remove-AzureADGroup -ObjectId b11ca53e-07cc-455d-9a89-1fe3ab24566b

## <a name="managing-members-of-groups"></a>Ryhmien jäsenten hallinta
Jos haluat lisätä ryhmään uusia jäseniä, käytä Lisää AzureADGroupMember cmdlet-komento. Tämä komento lisää on käytetty edellisen esimerkin Intune Järjestelmänvalvojat-ryhmän jäsen:

    PS C:\Windows\system32> Add-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

Objektitunnus - parametri on ryhmä, johon haluamme Lisää jäsen objektitunnus ja - RefObjectId on käytettävä jäsenen lisääminen ryhmään käyttäjän objektitunnus.

Jos haluat ryhmän jäsenet, käytä Get-AzureADGroupMember cmdlet-esimerkin osoittamalla tavalla:

    PS C:\Windows\system32> Get-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                        72cd4bbd-2594-40a2-935c-016f3cfeeeea User
                        8120cc36-64b4-4080-a9e8-23aa98e8b34f User

Jos haluat poistaa on aiemmin lisännyt ryhmän jäsen, käytä Poista AzureADGroupMember cmdlet, joka näkyy ohessa:

    PS C:\Windows\system32> Remove-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -MemberId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

Voit tarkistaa ryhmän membership(s) käyttäjän käyttämällä Valitse AzureADGroupIdsUserIsMemberOf cmdlet-komento. Cmdlet on sen parametrien, käyttäjä, jonka voit tarkistaa ryhmäjäsenyyksiä objektitunnus ja ryhmien jäsenyyksien tarkistamisen luettelo. Ryhmien luettelo on annettava kompleksiluvun muuttujan "Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck" lomakkeessa, niin on on luotava muuttuja, tyyppi:

    PS C:\Windows\system32> $g = new-object Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck

Seuraavaksi annamme groupIds sisään määrite "GroupIds" kompleksiluvun muuttujan arvot:

    PS C:\Windows\system32> $g.GroupIds = "b11ca53e-07cc-455d-9a89-1fe3ab24566b", "31f1ff6c-d48c-4f8a-b2e1-abca7fd399df"

Nyt, jos haluamme Tarkista objektitunnus 72cd4bbd-2594-40a2-935c-016f3cfeeeea vastaan $g ryhmiä ja käyttäjän kirjautuessa, emme kannattaa käyttää:

    PS C:\Windows\system32> Select-AzureADGroupIdsUserIsMemberOf -ObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea -GroupIdsForMembershipCheck $g

    OdataMetadata                                                                                               Value
    -------------                                                                                               -----
    https://graph.windows.net/85b5ff1e-0402-400c-9e3c-0f9e965325d1/$metadata#Collection(Edm.String)             {31f1ff6c-d48c-4f8a-b2e1-abca7fd399df}


Arvo on ryhmät, jonka jäsen käyttäjä on luettelo. Voit myös käyttää tätä menetelmää voit tarkistaa yhteystiedot, ryhmät tai palvelun ansaitun jäsenyyden ryhmät, valitse AzureADGroupIdsContactIsMemberOf, valitse AzureADGroupIdsGroupIsMemberOf tai valitse AzureADGroupIdsServicePrincipalIsMemberOf annetun luettelo

## <a name="managing-owners-of-groups"></a>Ryhmien omistajien hallinta
Jos haluat lisätä ryhmän omistajat, käytä Lisää AzureADGroupOwner cmdlet-komento:

    PS C:\Windows\system32> Add-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

Objektitunnus - parametri on ryhmä, johon on haluat lisätä omistaja objektitunnus ja - RefObjectId on käytettävä ryhmän omistaja Lisää käyttäjän objektitunnus.

Voit hakea ryhmän omistajien käyttämällä Get-AzureADGroupOwner:

    PS C:\Windows\system32> Get-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

Cmdlet palauttaa määritetyn ryhmän omistajien luettelon:

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                        e831b3fd-77c9-49c7-9fca-de43e109ef67 User

Jos haluat poistaa ryhmän omistaja, käytä Poista AzureADGroupOwner:

    PS C:\Windows\system32> remove-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -OwnerId e831b3fd-77c9-49c7-9fca-de43e109ef67

## <a name="next-steps"></a>Seuraavat vaiheet

Löydät lisää PowerShellin Azure Active Directory-ohjeista [Azure Active Directory cmdlet-komennot](http://go.microsoft.com/fwlink/p/?LinkId=808260).

* [Resurssien käytön hallinta ja Azure Active Directory-ryhmä](active-directory-manage-groups.md)

* [Azure Active Directory-integraation paikallisen käyttäjätietoja](active-directory-aadconnect.md)

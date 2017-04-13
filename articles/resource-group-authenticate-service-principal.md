<properties
   pageTitle="Luo Azure palvelu pääasiallista PowerShellin | Microsoft Azure"
   description="Tässä artikkelissa käsitellään PowerShellin Azure avulla voit luoda Active Directory-sovelluksen ja palvelun lyhennys ja myöntää resurssien Roolipohjainen käyttöoikeuksien valvonta kautta. Se näyttää, miten sovellus, jonka salasanan tai varmenteen todentamiseen."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="09/12/2016"
   ms.author="tomfitz"/>

# <a name="use-azure-powershell-to-create-a-service-principal-to-access-resources"></a>PowerShellin Azure avulla voit luoda service-lyhennys access-resurssit

> [AZURE.SELECTOR]
- [PowerShellin](resource-group-authenticate-service-principal.md)
- [Azure CLI](resource-group-authenticate-service-principal-cli.md)
- [Portal](resource-group-create-service-principal-portal.md)

Kun sovellus tai komentosarjan, joka on resursseja, sinulla ei todennäköisesti et halua suorittaa tämä prosessi tunnistetiedoillasi. Joudut ehkä, jota haluat käyttää sovelluksen käyttöoikeuksia ja haluat jatkaa käyttämällä tunnistetietoja, jos tehtäviisi muuttaa sovelluksen. Sen sijaan voit luoda sovelluksen, joka sisältää todennuksen käyttäjätiedot ja roolimäärityksiä jäsenyyden. Aina, kun sovellus käynnistyy, se todentaa itse nämä tunnuksilla. Tässä ohjeaiheessa kerrotaan, miten [PowerShellin Azure](powershell-install-configure.md) avulla voit määrittää kaikki sovelluksen suoritetaan omassa tunnistetiedot ja käyttäjätiedot.

PowerShellin käytettävissä on kaksi vaihtoehtoista todennustapa AD-sovelluksen:

 - salasana
 - varmenne

Tässä ohjeaiheessa esitellään käyttämisestä PowerShell kumpaankin vaihtoehtoon. Jos haluat kirjautua Azure ohjelmoinnin framework (esimerkiksi Python, Ruby eli Node.js), salasanan todennusta voi olla paras vaihtoehto. Ennen kuin päätät käyttää salasanan tai varmenteen on esimerkkejä eri kehysten todennustapa [otoksen sovellukset](#sample-applications) -osassa.

## <a name="active-directory-concepts"></a>Active Directory-käsitteestä

Tässä artikkelissa Luo kaksi objektia - Active Directory (AD)-sovelluksen ja lainan palvelu. AD-sovellus on yleinen esitys sovelluksesta. Se sisältää tunnistetiedot (tunnus ja joko salasanan tai varmenteen). Lainan palvelu on paikallisen Active Directory-sovelluksen-esitys. Siinä on roolimääritys. Tässä artikkelissa keskitytään yhden vuokraajan-sovellusta, jos sovellus on tarkoitettu toimimaan vain yhden organisaatiosi. Käytetään tavallisesti yhden vuokraajan sovellusten liiketoiminta-sovellukset, jotka suoritetaan organisaatiossa. Yhden vuokraajan-sovelluksessa on yhteen AD-sovellukseen ja yksi palvelu lyhennyksen.

Ihmettelet ehkä - mihin tarvitsen molemmat objektit? Tämän menetelmän tekee kannattaa, kun pidät usean vuokraajan sovellukset. Käytetään yleensä usean vuokraajan sovellusten ohjelmiston nimellä--palvelun (SaaS)-sovelluksia, jossa sovellus suoritetaan useita eri tilaukset. Usean vuokraajan sovellusten on yksi AD-sovellus ja useita palvelun ansaitun (yksi kullekin Active Directoryssa, joka antaa access-sovellukseen). Määrittämään monen vuokraajan sovelluksen [kehittäjän](resource-manager-api-authentication.md)oppaassa luvan Azure Resurssienhallinta-Ohjelmointirajapinnan kanssa.

## <a name="required-permissions"></a>Tarvittavat käyttöoikeudet

Tämän ohjeaiheen suorittamiseen tarvitset riittäviä käyttöoikeuksia Azure Active Directory-ja Azure tilauksen. Tarkemmin sanottuna sinun on voitava Active Directory-sovelluksen luominen ja määritä palvelun lyhennys rooli. 

Active Directory-tilin on oltava järjestelmänvalvoja (esimerkiksi **Yleisen järjestelmänvalvojan** tai **Käyttäjän järjestelmänvalvoja**). Jos tiliisi on määritetty **roolia** , joudut käyttöoikeuksien laajentamista järjestelmänvalvojaa.

Tilauksen, tilisi on oltava `Microsoft.Authorization/*/Write` access, jotka myönnetään [omistajan](./active-directory/role-based-access-built-in-roles.md#owner) rooli tai [käyttäjän Access](./active-directory/role-based-access-built-in-roles.md#user-access-administrator) -järjestelmänvalvojaksi. Jos tiliisi on määritetty **osallistujan** rooli, näyttöön tulee virhe yritettäessä palvelun lyhennys Määritä rooli. Uudelleen-tilauksen järjestelmänvalvoja on myönnettävä sinulle tarvittavat käyttöoikeudet.

Siirry nyt osan [salasanan](#create-service-principal-with-password) tai [varmenteen](#create-service-principal-with-certificate) todentamiseen.

## <a name="create-service-principal-with-password"></a>Luo palvelu pääasiallista salasanalla

Tässä osassa suorittaa seuraavien ohjeiden avulla:

- AD-sovelluksen luominen salasanalla
- Luo palvelun lyhennyksen.
- Lukija-roolin määrittäminen palvelun lyhennyksen.

Näiden vaiheiden suorittamista nopeasti, Katso seuraavat kolme cmdlet-komennot. 

     $app = New-AzureRmADApplication -DisplayName "{app-name}" -HomePage "https://{your-domain}/{app-name}" -IdentifierUris "https://{your-domain}/{app-name}" -Password "{your-password}"
     New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId
     New-AzureRmRoleAssignment -RoleDefinitionName Reader -ServicePrincipalName $app.ApplicationId.Guid

Seuraavassa kerrotaan seuraavasti tarkemmin, varmista, että ymmärrät prosessi.

1. Kirjaudu tiliisi.

        Add-AzureRmAccount

1. Luo uusi Active Directory-sovellus antamalla näyttönimi, joka kuvaa sovelluksen URI, joilla voidaan tunnistaa sovelluksen ja sovelluksen käyttäjätiedot salasanan URI.

        $app = New-AzureRmADApplication -DisplayName "exampleapp" -HomePage "https://www.contoso.org/exampleapp" -IdentifierUris "https://www.contoso.org/exampleapp" -Password "<Your_Password>"

     Yhden vuokraajan sovellusten URI ei tarkisteta.
     
     Jos tiliä ei ole [tarvittavia käyttöoikeuksia](#required-permissions) Active Directory, näyttöön tulee virhesanoma, jossa sanotaan "Authentication_Unauthorized" tai "tilausta ei löydy kontekstissa".

1. Tarkastele uuden sovellusobjektin. 

        $app
        
     Huomaa erityisesti **ApplicationId** -ominaisuutta, joka tarvitaan palvelun ansaitun roolimäärityksiä, luominen ja access-tunnuksen hankkiminen.

        DisplayName             : exampleapp
        ObjectId                : c95e67a3-403c-40ac-9377-115fa48f8f39
        IdentifierUris          : {https://www.contoso.org/example}
        HomePage                : https://www.contoso.org
        Type                    : Application
        ApplicationId           : 8bc80782-a916-47c8-a47e-4d76ed755275
        AvailableToOtherTenants : False
        AppPermissions          : 
        ReplyUrls               : {}

2. Luo service-lyhennys sovelluksen Active Directory-sovelluksen sovellustunnus kulkee.

        New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId

3. Tilauksen service principal-käyttöoikeuksien myöntäminen Tässä esimerkissä palvelun lyhennys lisääminen **lukija** -rooli, joka antaa lukuoikeudet tilauksen kaikki resurssit. Lisätietoja muiden roolien [RBAC: valmiin roolien](./active-directory/role-based-access-built-in-roles.md). **ServicePrincipalName** -parametrin on **ApplicationId** , jota käytit luodessasi sovellus. 

        New-AzureRmRoleAssignment -RoleDefinitionName Reader -ServicePrincipalName $app.ApplicationId.Guid

    Jos tiliä ei ole riittäviä käyttöoikeuksia, Määritä rooli, näyttöön tulee virhesanoma. Viesti, joka ilmaisee oman tilillä **ei ole lupaa suorittavan toiminnon Microsoft.Authorization/roleAssignments/write laajuus/tilaukset / {guid} päälle**. 

Joka on tämä! AD-sovelluksen ja palvelun lyhennys on määritetty. Seuraavan osan avulla voit kirjautua sisään tunnistetiedon PowerShellin kautta. Jos haluat käyttää tunnistetieto koodi-sovelluksessa, voit siirtyä [otoksen sovellukset](#sample-applications). 

### <a name="provide-credentials-through-powershell"></a>Anna tunnistetiedot PowerShellin kautta

Nyt sinun täytyy suorittaa sovelluksen Kirjaudu sisään.

1. Luo **PSCredential** -objekti, joka sisältää tunnistetiedot suorittamalla **Get tunnistetiedon** -komennon. Tarvitset **ApplicationId** ennen sen suorittamista komennon joten varmista, että sinulla on, että käytettävissä Liitä.

        $creds = Get-Credential

2. Ohjelma pyytää sinua antamaan tunnistetiedot. Käytä käyttäjänimi- **ApplicationId** , jota käytit luodessasi sovellus. Salasanan Käytä määritetty tiliä luodessasi.

     ![Anna tunnistetiedot](./media/resource-group-authenticate-service-principal/arm-get-credential.png)

2. Aina, kun kirjautuminen palvelun lyhennys, sinun täytyy antaa hakemiston vuokraajan tunnus AD-sovellus. Palvelutili on Active Directory-esiintymä. Jos käytössäsi on vain yksi tilaus, voit käyttää:

        $tenant = (Get-AzureRmSubscription).TenantId
    
     Jos sinulla on useita tilauksia, Määritä sijainti Active Directory tilaus. Lisätietoja on artikkelissa [miten Azure tilaukset liittyvät Azure Active Directory](./active-directory/active-directory-how-subscriptions-associated-directory.md).

        $tenant = (Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId

4. Kirjaudu palveluun lyhennys määrittämällä, että tili on palvelun lyhennys ja antamalla tunnistetiedot-objekti. 

        Add-AzureRmAccount -Credential $creds -ServicePrincipal -TenantId $tenant
        
     Voit nyt todennetaan Active Directory-sovellus, jonka loit pääasiallisena palveluna.

### <a name="save-access-token-to-simplify-log-in"></a>Tallenna käyttöoikeustietue yksinkertaistaa kirjautuminen

Estämiseksi palvelun pääasiallista käyttäjätiedot aina, kun se tarvitsee kirjautua sisään, voit tallentaa access-tunnuksen.

1. Käyttämään nykyisen käyttöoikeustietue myöhemmin istunnon tallentaminen profiili.

        Save-AzureRmProfile -Path c:\Users\exampleuser\profile\exampleSP.json
        
     Avaa profiili ja tarkastella sen sisältöä. Huomaa, että se sisältää access-tunnuksen. 
        
2. Sen sijaan, että manuaalisesti kirjautumisesta uudelleen, ladata profiilin.

        Select-AzureRmProfile -Path c:\Users\exampleuser\profile\exampleSP.json
        
> [AZURE.NOTE] Access-tunnuksen vanhenee, jotta käyttämällä tallennetun profiilin toimii vain, kun tunnus on voimassa.
        
## <a name="create-service-principal-with-certificate"></a>Luo palvelu pääasiallista sertifikaatilla

Tässä osassa suorittaa seuraavien ohjeiden avulla:

- itse allekirjoitetun varmenteen luominen
- sertifikaatin AD-sovelluksen luominen
- Luo palvelun lyhennyksen.
- Lukija-roolin määrittäminen palvelun lyhennyksen.

Näiden vaiheiden Azure PowerShell 2.0 nopeasti suorittamista Windows 10: ssä tai Windows Server 2016 Technical Preview artikkelissa seuraavat cmdlet-komennot. 

    $cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=exampleapp" -KeySpec KeyExchange
    $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())
    $app = New-AzureRmADApplication -DisplayName "exampleapp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.contoso.org/example" -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore
    New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId
    New-AzureRmRoleAssignment -RoleDefinitionName Reader -ServicePrincipalName $app.ApplicationId.Guid

Seuraavassa kerrotaan seuraavasti tarkemmin, varmista, että ymmärrät prosessi. Tässä artikkelissa kerrotaan myös tehtävien suorittamisesta PowerShellin Azure tai käyttöjärjestelmien aiemmissa versioissa.

### <a name="create-the-self-signed-certificate"></a>Itse allekirjoitetun varmenteen luominen

PowerShell sisältyy Windows 10: ssä ja Windows Server 2016 Technical Preview-version on päivitetty **Uusi SelfSignedCertificate** -cmdlet luonnissa itse allekirjoitettua varmennetta. Aiemmissa-käyttöjärjestelmissä on uusi SelfSignedCertificate cmdlet-komento, mutta se ei tarjoa tämän ohjeaiheen tarvittavat parametrit. Haluat tuoda moduulin Luo sertifikaatti. Tässä ohjeaiheessa esitellään luonnissa käyttöjärjestelmän perusteella varmenne on sekä tavoista. 

- Jos käytössäsi on **Windows 10: ssä tai Windows Server 2016 Technical Preview-version**, suorita seuraava komento itse allekirjoitetun varmenteen luominen: 

        $cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=exampleapp" -KeySpec KeyExchange
       
- Jos sinulla **ei ole Windows 10: ssä tai Windows Server 2016 Technical Preview**-haluat ladata [itse allekirjoitetun sertifikaatin luominen](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6/) Microsoft Script Center. Pura sen sisältö ja tuo cmdlet-komento, sinun on.
     
        # Only run if you could not use New-SelfSignedCertificate
        Import-Module -Name c:\ExtractedModule\New-SelfSignedCertificateEx.ps1
    
     Luo sertifikaatti.
    
        $cert = New-SelfSignedCertificateEx -Subject "CN=exampleapp" -KeySpec "Exchange" -FriendlyName "exampleapp"

Sinulla on varmennetta, ja voit jatkaa AD-sovelluksen.

### <a name="create-the-active-directory-app-and-service-principal"></a>Luo Active Directory-sovelluksen ja palvelun pääasiallista

1. Noutaa avainarvon varmenne.

        $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())

2. Kirjaudu Azure-tili.

        Add-AzureRmAccount

3. Luo uusi Active Directory-sovellus antamalla näyttönimi, joka kuvaa sovelluksen URI, joilla voidaan tunnistaa sovelluksen ja sovelluksen käyttäjätiedot salasanan URI.

     Jos sinulla on Azure PowerShell 2.0 (elokuu 2016 tai uudempi versio), käytä seuraavan cmdlet-komennon:

        $app = New-AzureRmADApplication -DisplayName "exampleapp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.contoso.org/example" -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore      
    
    Jos sinulla on Azure PowerShell 1.0, käytä seuraavan cmdlet-komennon:
    
        $app = New-AzureRmADApplication -DisplayName "exampleapp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.contoso.org/example" -KeyValue $keyValue -KeyType AsymmetricX509Cert  -EndDate $cert.NotAfter -StartDate $cert.NotBefore      
    
    Yhden vuokraajan sovellusten URI ei tarkisteta.
    
    Jos tiliä ei ole [tarvittavia käyttöoikeuksia](#required-permissions) Active Directory, näyttöön tulee virhesanoma, jossa sanotaan "Authentication_Unauthorized" tai "tilausta ei löydy kontekstissa".
        
    Tarkastele uuden sovellusobjektin. 

        $app

    Huomaa **ApplicationId** -ominaisuutta, joka tarvitaan palvelun ansaitun roolimäärityksiä, luominen ja hankkiminen access tunnukset.

        DisplayName             : exampleapp
        ObjectId                : c95e67a3-403c-40ac-9377-115fa48f8f39
        IdentifierUris          : {https://www.contoso.org/example}
        HomePage                : https://www.contoso.org
        Type                    : Application
        ApplicationId           : 8bc80782-a916-47c8-a47e-4d76ed755275
        AvailableToOtherTenants : False
        AppPermissions          : 
        ReplyUrls               : {}


5. Luo service-lyhennys sovelluksen Active Directory-sovelluksen sovellustunnus kulkee.

        New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId

6. Tilauksen service principal-käyttöoikeuksien myöntäminen Tässä esimerkissä palvelun lyhennys lisääminen **lukija** -rooli, joka antaa lukuoikeudet tilauksen kaikki resurssit. Lisätietoja muiden roolien [RBAC: valmiin roolien](./active-directory/role-based-access-built-in-roles.md). **ServicePrincipalName** -parametrin on **ApplicationId** , jota käytit luodessasi sovellus.

        New-AzureRmRoleAssignment -RoleDefinitionName Reader -ServicePrincipalName $app.ApplicationId.Guid

    Jos tiliä ei ole riittäviä käyttöoikeuksia, Määritä rooli, näyttöön tulee virhesanoma. Viesti, joka ilmaisee oman tilillä **ei ole lupaa toiminto Microsoft.Authorization/roleAssignments/write laajuus/tilaukset / {guid} päälle**.

Joka on tämä! AD-sovelluksen ja palvelun lyhennys on määritetty. Seuraavan osan avulla voit kirjautua sisään varmenteen PowerShellin kautta.

### <a name="provide-certificate-through-automated-powershell-script"></a>Antaa Varmenteen automaattinen PowerShell-komentosarjojen kautta

Aina, kun kirjautuminen palvelun lyhennys, sinun täytyy antaa hakemiston vuokraajan tunnus AD-sovellus. Palvelutili on Active Directory-esiintymä. Jos käytössäsi on vain yksi tilaus, voit käyttää:

    $tenant = (Get-AzureRmSubscription).TenantId
    
Jos sinulla on useita tilauksia, Määritä Active Directory sijainti tilaus. Lisätietoja on artikkelissa [Azure Active Directoryn hallinta](./active-directory/active-directory-administer.md).

    $tenant = (Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId

Tarkistamiseen-komentosarjan määritettävä tili on pääasiallista palvelu ja varmenteen allekirjoitus, sovellustunnus ja vuokraajan tunnus. Voit automatisoida komentosarja, voit tallentaa nämä arvot ympäristömuuttujat ja tarkastella niitä suorituksen aikana tai voit sisällyttää ne komentosarja.

    Add-AzureRmAccount -ServicePrincipal -CertificateThumbprint $cert.Thumbprint -ApplicationId $app.ApplicationId -TenantId $tenant

Voit nyt todennetaan Active Directory-sovellus, jonka loit pääasiallisena palveluna.

## <a name="sample-applications"></a>Esimerkki sovellukset

Esimerkki seuraavien sovellusten näyttää, miten Kirjaudu palvelun lyhennys.

**.NET**

- [Ottaa käyttöön SSH käytössä AM .NET-malli](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-template-deployment/)
- [Azure resurssit ja .NET resurssiryhmien hallinta](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-resources-and-groups/)

**Java**

- [Java-resurssit - käyttöönotto Azure Resurssienhallinta mallilla - aloittaminen](https://azure.microsoft.com/documentation/samples/resources-java-deploy-using-arm-template/)
- [Java-resurssit - hallita resurssiryhmä - aloittaminen](https://azure.microsoft.com/documentation/samples/resources-java-manage-resource-group//)

**Python**

- [Ottaa käyttöön SSH käytössä AM Python käyttämällä mallia](https://azure.microsoft.com/documentation/samples/resource-manager-python-template-deployment/)
- [Azure resurssi ja Python resurssiryhmien hallinta](https://azure.microsoft.com/documentation/samples/resource-manager-python-resources-and-groups/)

**Node.js**

- [Ottaa käyttöön SSH käytössä AM Node.js käyttämällä mallia](https://azure.microsoft.com/documentation/samples/resource-manager-node-template-deployment/)
- [Azure resursseja ja Node.js resurssiryhmien hallinta](https://azure.microsoft.com/documentation/samples/resource-manager-node-resources-and-groups/)

**Ruby**

- [Ottaa käyttöön SSH käytössä AM Ruby-malli](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-template-deployment/)
- [Azure resurssi ja Ruby resurssiryhmien hallinta](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)

## <a name="next-steps"></a>Seuraavat vaiheet
  
- Yksityiskohtaiset ohjeet sovelluksen integroiminen Azure resurssien hallintaa varten katso [luvan Azure Resurssienhallinta API Kehittäjän ohje](resource-manager-api-authentication.md).
- Katso tarkempia tietoja virhetilanteesta sovellukset ja palvelun ansaitun [sovelluksen ja palvelun lyhennys objekteja](./active-directory/active-directory-application-objects.md). 
- Saat lisätietoja Active Directory käyttöoikeuksien [Todennus tilanteita, joissa Azure AD](./active-directory/active-directory-authentication-scenarios.md).




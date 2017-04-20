## <a name="prepare-to-authenticate-azure-resource-manager-requests"></a>Valmistele tarkistamiseen Azure henkilöstöpäällikkö pyynnöt

Kaikki toiminnot, joita suorittaa resurssien [Azure resurssinhallinta] on todennettava[ lnk-authenticate-arm] on Azure Active Directory (AD). PowerShell tai Azure CLI on helpoin tapa määrittää tämän.

Asenna [Azure PowerShell 1.0] [ lnk-powershell-install] tai uudempi versio, ennen kuin jatkat.

Seuraavissa vaiheissa kuvataan salasananvahvistusta käyttäen PowerShell AD-sovelluksen määrittämisestä. Voit suorittaa nämä komennot standardin PowerShell-istunnossa.

1. Kirjaudu sisään ja Azure tilauksesi seuraavan komennon avulla:

    ```
    Login-AzureRmAccount
    ```

2. Muistiin, **TenantId** ja **SubscriptionId**. Tarvitset niitä myöhemmin.

3. Luo uusi Azure Active Directory-sovelluksen, korvata paikka haltijoille seuraavan komennon avulla:

    - **{Display name}:** **MySampleApp** kuten sovelluksen näyttönimi
    - **{Kotisivun URL}:** sovellusten kuten **http://mysampleapp/home**kotisivun URL-osoite. Tätä URL-Osoitetta ei tarvitse todellinen kohdistus.
    - **{Sovelluksen tunnus}:** Yksilöivä tunniste, kuten **http://mysampleapp**. Tätä URL-Osoitetta ei tarvitse todellinen kohdistus.
    - **{Salasana}:** Salasana, jota käytetään todentamiseen sovellusten kanssa.

    ```
    New-AzureRmADApplication -DisplayName {Display name} -HomePage {Home page URL} -IdentifierUris {Application identifier} -Password {Password}
    ```
    
4. **ApplicationId** on luotu sovelluksen muistiin. Tarvitset tätä myöhemmin.

5. Luo uuden palvelun pääasiallista, **{MyApplicationId}** korvaaminen edellisen vaiheen **ApplicationId** seuraavan komennon avulla:

    ```
    New-AzureRmADServicePrincipal -ApplicationId {MyApplicationId}
    ```
    
6. Roolien osoitus, että **ApplicationId**- **{MyApplicationId}** korvaaminen seuraavan komennon avulla asennus.

    ```
    New-AzureRmRoleAssignment -RoleDefinitionName Owner -ServicePrincipalName {MyApplicationId}
    ```
    
Nyt olet luonut Azure AD-sovellus, jonka avulla voit todentaa mukautetun C# sovelluksessa. -Myöhemmin tässä opetusohjelmassa tarvitaan seuraavat arvot:

- TenantId
- SubscriptionId
- ApplicationId
- Salasana

[lnk-authenticate-arm]: https://msdn.microsoft.com/library/azure/dn790557.aspx
[lnk-powershell-install]: ../articles/powershell-install-configure.md

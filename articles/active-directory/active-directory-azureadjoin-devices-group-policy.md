<properties
    pageTitle="Toimialueen liittyneet laitteiden yhdistäminen Azure AD for Windows 10: ssä ilmenee | Microsoft Azure"
    description="Tässä artikkelissa kerrotaan, miten järjestelmänvalvojat voivat määrittää ryhmäkäytännön laitteita käyttävät toimialueen yhdistetty yrityksen verkkoon."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""
    tags="azure-classic-portal"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="connect-domain-joined-devices-to-azure-ad-for-windows-10-experiences"></a>Toimialueen liittyneet laitteiden yhdistäminen Azure AD for Windows 10: ssä kokemukset

Toimialueen liitos on perinteinen asennustapa-organisaatiot yhdistettyjä laitteiden työn viimeisen 15 vuotta ja paljon muuta. Se on otettu käyttöön käyttäjät voivat kirjautua laitteensa käyttämällä Windows Server Active Directory (Active Directory) työpaikan tai oppilaitoksen tilit ja sallittu IT-hallinta täysin seuraaviin laitteisiin. Organisaatiot yleensä riippuvaisia imaging menetelmiä säännöstä laitteet käyttäjille ja käytetään yleensä System Center Configuration Manager (SCCM) tai Ryhmäkäytäntö, voit hallita niitä.

Toimialueen liity Windows 10: ssä on seuraavat edut, kun olet yhdistänyt laitteiden Azure Active Directory (Azure AD):

- Kertakirjautuminen (SSO) Azure AD-resurssien missä tahansa
- Accessin yrityksen Windows-kaupan käyttämällä työpaikan tai oppilaitoksen tilit (Microsoft account ei ole pakollinen)
- Enterprise-yhteensopiva roaming asetukset laitteiden välillä käyttämällä työpaikan tai oppilaitoksen tilit (Microsoft account ei ole pakollinen)
- Vahva todennus ja helposti-kirjautuminen Microsoft Passport ja Windows Hei työpaikan tai oppilaitoksen tilit
- Mahdollisuus käytön rajoittaminen vain laitteissa, joka noudattaa organisaation laitteen ryhmäkäytäntöasetukset

## <a name="prerequisites"></a>Edellytykset

Toimialueen liity edelleen hyödyllisiä. Voit kuitenkin saaminen Azure AD SSO edut, roaming työpaikan tai oppilaitoksen tilit ja access-asetukset, Windows-kaupan työpaikan tai oppilaitoksen tilin kanssa, sinun on seuraava:

- Azure AD-tilaus
- Azure AD Connect laajentaa Azure AD paikallisesta hakemistosta
- Käytännön, joka on määritetty muodostamaan yhteys Azure AD toimialueeseen liittyneet laitteet
- Laitteiden Muodosta Windows 10: ssä (koontiversio 10551 tai uudempi versio)

Jotta Microsoft Passport työn ja Windows Hei, tarvitset myös seuraavasti:

- Julkisen avaimen infrastruktuuri (PKI) käyttäjän varmenteet liittoutumispalvelujen varten.
- System Center määritysten hallinta teknisen ennakkoversion 1509 versio. Lisätietoja on artikkelissa [Microsoft System Center määritysten hallinnan Technical Preview](https://technet.microsoft.com/library/dn965439.aspx#BKMK_TP3Update) ja [System Center hallintatoiminto tiimin blogia](http://blogs.technet.com/b/configmgrteam/archive/2015/09/23/now-available-update-for-system-center-config-manager-tp3.aspx). Tämä edellyttää käyttäjävarmenteet perustuva Microsoft Passport käyttöönotto.

PKI käyttöönoton vaatimus vaihtoehtona toimi seuraavasti:

- On muutama toimialueen ohjaimet kanssa Windows Server 2016 Active Directory ‑toimialueen palveluihin.

Ehdollinen käyttämiseksi, voit luoda ryhmäkäytäntöasetukset, jotka mahdollistavat toimialueeseen liittyneet laitteet, joissa ei ole muita ominaisuuksissa käyttämisen. Hallittavan perustuu laitteen käytönvalvonta on seuraavasti:

- System Center määritysten hallinta Passport skenaariot teknisen ennakkoversion 1509 versio

## <a name="deployment-instructions"></a>Käyttöönotto-ohjeet



### <a name="step-1-deploy-azure-active-directory-connect"></a>Vaihe 1: Ota käyttöön Azure Active Directory yhdistäminen

Azure AD Connect avulla voit valmistella tietokoneiden paikallisen laitteen objekteina pilveen. Ottaa käyttöön Azure AD Connect viitata "Asenna Azure AD Connect" on artikkelissa [integrointi paikallisen-käyttäjätietojen Azure Active Directory-hakemistosta](active-directory-aadconnect.md#install-azure-ad-connect).

 - Jos olet toiminut [mukautetun asennuksen Azure AD Connect](./connect/active-directory-aadconnect-get-started-custom.md) (ei pika-asennus), noudata sitten ohjeita **Luo palvelun yhteyden valitsemalla paikallisen Active Directory**-jäljempänä tässä vaiheessa.
 - Jos sinulla on liitetyt kokoonpanon Azure AD ennen asennusta Azure AD Connect (esimerkiksi, jos olet ottanut Active Directory Federation Services (AD FS) ennen), noudata **Configure AD FS ryhmän säännöt** ohje on jäljempänä tässä vaiheessa.

#### <a name="create-a-service-connection-point-in-on-premises-active-directory"></a>Luo palvelun yhteyspistettä paikallisen Active Directory

Toimialueen liittyneet laitteet Azure AD-vuokraajan tietojen automaattinen rekisteröinti Azure laitteen rekisteröintipalvelu aikaa käyttämällä palvelun yhteyspistettä.

Azure AD Connect palvelimessa Suorita seuraavat PowerShell-komennot:

    Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1";

    $aadAdminCred = Get-Credential;

    Initialize-ADSyncDomainJoinedComputerSync –AdConnectorAccount [connector account name] -AzureADCredentials $aadAdminCred;


Kun käynnissä cmdlet $aadAdminCred = Get-tunnistetiedon muodossa *user@example.com* tunnistetiedot, jotka lisätään, kun tulee ponnahdusikkuna Get tunnistetiedon käyttäjänimen.

Kun käynnissä cmdlet alusta ADSyncDomainJoinedComputerSync tilalle toimialuetili, jota käytetään Active Directory connector-tilin [*connector-tilin nimi*].

#### <a name="configure-ad-fs-claim-rules"></a>Määritä AD FS ryhmän säännöt
Tietokone, jossa Azure laitteen rekisteröintipalvelu välittömästi rekisteröinti AD FS varaa sääntöjen määrittäminen mahdollistaa sallimalla tietokoneiden tarkistamiseen käyttämällä Kerberos/NTLM AD FS kautta. Tässä vaiheessa ilman tietokoneet saavat Azure AD, viivästyneet tavalla (veloittaa Azure AD Connect synkronointi kertaa).

>[AZURE.NOTE]
Jos sinulla ei ole AD FS federation server paikallisen nimellä, noudata ohjeita toimittajan varaa sääntöjen luominen.

ADFS-palvelimeen (tai istunnon yhteyttä palvelimeen AD FS) suorittamalla seuraavat PowerShell-komennot:

      <#----------------------------------------------------------------------
     |   Modify the Azure AD Relying Party to include the claims needed
     |   for DomainJoin++. The rules include:
     |   -ObjectGuid
     |   -AccountType
     |   -ObjectSid
     +---------------------------------------------------------------------#>

    $existingRules = (Get-ADFSRelyingPartyTrust -Identifier urn:federation:MicrosoftOnline).IssuanceTransformRules

    $rule1 = '@RuleName = "Issue object GUID"
          c1:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"] &&
          c2:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]
          => issue(store = "Active Directory", types = ("http://schemas.microsoft.com/identity/claims/onpremobjectguid"), query = ";objectguid;{0}", param = c2.Value);'

    $rule2 = '@RuleName = "Issue account type for domain joined computers"
          c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]
          => issue(Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", Value = "DJ");'

    $rule3 = '@RuleName = "Pass through primary SID"
          c1:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"] &&
          c2:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]
          => issue(claim = c2);'

    $updatedRules = $existingRules + $rule1 + $rule2 + $rule3

    $crSet = New-ADFSClaimRuleSet -ClaimRule $updatedRules

    Set-AdfsRelyingPartyTrust -TargetIdentifier urn:federation:MicrosoftOnline -IssuanceTransformRules $crSet.ClaimRulesString

>[AZURE.NOTE]
Windows 10-tietokoneet todentaa käyttämällä active WS luotettavaksi päätepisteen AD FS ylläpitämä Windowsin integroitua todennusta. Varmista, että tämä päätepiste on käytössä. Jos käytössäsi on Web-todennus välityspalvelinta, varmista myös, että tämä päätepiste julkaistaan välityspalvelimen kautta. Voit tehdä tämän tarkistamalla adfs/palvelut ja Salli/13/windowstransport. Se näyttää, kun käytössä AD FS-hallintakonsoli **palvelussa** > **päätepisteet**.


### <a name="step-2-configure-automatic-device-registration-via-group-policy-in-active-directory"></a>Vaihe 2: Määritä automaattinen laitteen rekisteröinti ryhmäkäytännön Active Directoryssa

Active Directoryn ryhmäkäytännön avulla voit määrittää Windows 10: ssä toimialueen liittyneet laitteet automaattisesti rekisteröitymään Azure AD.

> [AZURE.NOTE]
> Uusimman ohjeita siitä, miten voit määrittää Automaattiset laitteen rekisteröinti-kohdassa [automaattinen rekisteröinti Windows-toimialueen määrittämisestä liittyneet laitteiden Azure Active Directory-hakemistosta](active-directory-conditional-access-automatic-device-registration-setup.md).
>
> Ryhmäkäytäntö Tämä malli on nimetty uudelleen Windows 10: ssä. Jos käytät Ryhmäkäytäntö-työkalun poistaminen Windows 10-tietokoneessa, käytäntö näkyy muodossa: <br>
> **Rekisteröi toimialueelle liitetyissä tietokoneissa laitteet**<br>
> Käytäntö on seuraavassa kansiossa:<br>
> ***Tietokoneen määritykset ja käytännöt/järjestelmänvalvojan malleja/Windows osia tai laitteen rekisteröinti***


## <a name="additional-information"></a>Lisätietoja
* [Windows 10: n yrityksen: tapoja käyttää laitteet](active-directory-azureadjoin-windows10-devices-overview.md)
* [Cloud ominaisuuksia Windows 10-laitteisiin kautta Azure Active Directory-liittyä laajentaminen](active-directory-azureadjoin-user-upgrade.md)
* [Lisätietoja käyttötapoja Azure AD liittyminen](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Toimialueen liittyneet laitteiden yhdistäminen Azure AD for Windows 10: ssä kokemukset](active-directory-azureadjoin-devices-group-policy.md)
* [Määritä Azure AD liittyminen](active-directory-azureadjoin-setup.md)

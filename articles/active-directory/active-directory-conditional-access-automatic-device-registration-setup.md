<properties
    pageTitle="Määritä automaattinen rekisteröinti Windows toimialueeseen liittyneet laitteiden Azure Active Directory-hakemistosta | Microsoft Azure"
    description="Määrittää toimialueen liittyneet Windows-laitteiden rekisteröi automaattisesti ja äänettömästi Azure Active Directory-hakemistosta."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="markvi"/>



# <a name="set-up-automatic-registration-of-windows-domain-joined-devices-with-azure-active-directory"></a>Määrittäminen automaattinen rekisteröinti Windows Azure Active Directory-hakemistosta toimialueeseen liittyneet laitteet

[Azure Active Directory laitteen perustuva ehdollinen Accessin](active-directory-conditional-access.md)käyttäminen Windows toimialueeseen liittyneet tietokoneet rekisteröitävä Azure Active Directory (Azure AD). Tässä artikkelissa kerrotaan mitä sinun on suoritettava Windows toimialueeseen liittyneet laitteet, joissa on organisaation Azure AD-rekisteröinnin määrittäminen.

Ehdollinen Accessilla Azure AD-avulla voit seuraavia etuja:

-   Parannettu kertakirjautuminen (SSO) kohdata Azure AD-sovelluksen kautta työpaikan tai oppilaitoksen tilit
-   Yrityksen roaming asetuksia kaikissa laitteissa
-   Windows-kaupan pääsy yrityksille
-   Vahvempi todennus- ja käteviä, kirjaudu sisään Windows Hei

> [AZURE.NOTE] Windows 10 marraskuun päivitys on osa laajennettu käyttäjän kokemukset Azure AD-, mutta Windows 10 vuosipäivä päivitys tukee kokonaan laitteen perustuva ehdollisen käyttöoikeuden. Lisätietoja ehdollisen käyttöoikeuden on artikkelissa [Azure Active Directory laitteen perustuva ehdollinen](active-directory-conditional-access.md). Saat lisätietoja Windows 10-laitteissa työpaikan ja miten käyttäjä rekisteröi Windows 10-laitteeseen Azure AD- [yrityksen Windows 10: laitteiden käyttää](active-directory-azureadjoin-windows10-devices-overview.md).

Voit rekisteröidä joidenkin aiempien versioiden Windowsissa, kuten seuraavia versioita:

-   Windows 8.1
-   Windows 7: ssä

Jos käytät Windows Server-tietokoneen työpöydän, voit rekisteröidä näissä ympäristöissä:

- Windows Server 2016
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2


## <a name="prerequisites"></a>Edellytykset

Automaattinen rekisteröinti tärkeimmät vaatimus toimialueeseen liittyneet laitteiden käyttämällä Azure AD on ajan tasalla versio Azure Active Directory yhteyden (Azure AD Connect).

Sen mukaan, miten voit ottaa käyttöön Azure AD Connect ja oletko käyttänyt pika- tai mukautetun asennuksen tai paikallaan tapahtuva päivitys seuraavat edellytykset on ehkä määritetty automaattisesti:

-   **Palvelun yhteyspisteen paikallisen Active Directory**. Etsinnän Azure AD-vuokraajan tietojen tietokoneissa, Rekisteröidy Azure AD.

-   **Active Directory Federation Services (AD FS)-liittoutumispalvelujen transform säännöt**. Tietokoneen todennuksen rekisteröinnin (käytettävissä liitetyt määrityksiä).

Jos joitakin laitteita oman organisaatioissa eivät ole toimialueen liittyneet Windows 10-laitteissa, varmista, että voit tehdä seuraavia toimintoja:

- Määrittää käytännön Azure AD niin, että käyttäjät voivat rekisteröityä laitteet
- Määritä kelvollinen vaihtoehtona monimenetelmäisen todentamisen AD FS-integroitu Windows todennus (IWA)



## <a name="set-a-service-connection-point-for-discovery-of-the-azure-ad-tenant"></a>Määritä service yhteyspiste Azure AD-vuokraajan etsiminen

Palvelun connection piste-objekti on oltava nimeäminen konteksti-osio, jossa tietokoneet liitettyinä toimialueen metsää määritykset. Palvelun yhteyspistettä on Azure AD-vuokraajan etsiminen tietoja, jossa tietokoneet Rekisteröi. Usean metsää Active Directory-kokoonpanossa palvelun yhteyspistettä on oltava kaikki metsien toimialueeseen liittyneet tietokoneet sisältävät.

Määritä palvelun yhteyspistettä Active Directoryn seuraavista paikoista:

    CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,[Configuration Naming Context]

> [AZURE.NOTE] Metsää kanssa Active Directoryn toimialueen nimi *example.com*, nimeäminen kontekstin määritys on CN = Configuration, Ohjauskoneen = esimerkiksi Ohjauskoneen = com.

Voit tarkistaa Azure AD-vuokraajan objekti ja etsiminen arvot käyttämällä Windows PowerShell-komentosarjaa. (Korvaa määritysten nimeämällä kontekstin esimerkissä määritys-nimeämiskäytäntö kontekstin.)

    $scp = New-Object System.DirectoryServices.DirectoryEntry;

    $scp.Path = "LDAP://CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,CN=Configuration,DC=example,DC=com";

    $scp.Keywords;

`$scp.Keywords` Tulos näyttää Azure AD-vuokraajan tiedot:

    azureADName:microsoft.com

    azureADId:72f988bf-86f1-41af-91ab-2d7cd011db47

Jos palvelun yhteyspistettä ei ole, luo se suorittamalla seuraavaa PowerShell-komentosarjaa Azure AD Connect-palvelimeen:

    Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1";

    $aadAdminCred = Get-Credential;

    Initialize-ADSyncDomainJoinedComputerSync –AdConnectorAccount [connector account name] -AzureADCredentials $aadAdminCred;


> [AZURE.NOTE]Kun suoritat `$aadAdminCred = Get-Credential`, käytä muotoa *user@example.com* käyttäjänimen **Get-tunnistetiedot** -valintaikkunassa.  
> Kun suoritat alusta ADSyncDomainJoinedComputerSync cmdlet-komento, tilalle toimialuetili, jota käytetään Active Directory connector-tilin [*connector-tilin nimi*].  
> Cmdlet käyttää PowerShellin Active Directory-moduulin, joka perustuu Active Directory-verkkopalvelut-toimialueen ohjauskoneen. Active Directory-WWW-palveluista on tuettu toimialueen ohjaimet Windows Server 2008 R2 ja sitä uudemmissa versioissa. Toimialueen ohjaimet Windows Server 2008: n tai sitä aiemmissa versioissa System.DirectoryServices Ohjelmointirajapinnan kautta PowerShellin avulla voit luoda palvelun yhteyspistettä ja määritä sitten avainsanojen arvot.

## <a name="create-ad-fs-rules-for-instant-device-registration-in-federated-organizations"></a>Luo AD FS säännöt instant laitteen rekisteröinti liitetyt organisaatioissa

Liitetyt Azure-tietokannassa AD määritys-laitteita käyttävät AD FS-Liittoutumispalvelut (tai paikalliseen käytössä) Azure AD todentamiseen. Valitse rekisteröimään vastaan Azure Active Directory laitteen rekisteröintipalvelu (Azure AD-laitteen rekisteröintipalvelu).

> [AZURE.NOTE] Käyttäjän salasanan hajautusarvot on synkronoitu Azure AD ei ole liitetty määritys- ja Windows 10: ssä ja Windows Server 2016 toimialueeseen liittyneet tietokoneet todennetaan Azure AD-laitteen rekisteröintipalvelu. Käyttäjän todentaa käyttämällä tunnistetiedot, jotka käyttäjä kirjoittaa niiden paikallisen tietokonetilit kyselyjä ja jotka on välittäminen Azure AD Azure AD Connect kautta. Windows 10 ja Windows Server 2016 tietokoneisiin ei ole liitetty määrityksessä on eri laitteen perustuva varmenteen myöntäjä organisaatiossa. Lisätietoja on artikkelissa Windows 10-tietokoneissa tämän artikkelin osan lataaminen Windows Installer-pakettien.

Azure AD Connect liittää Windows 10: ssä ja Windows Server 2016-tietokoneiden laitteen objektin Azure AD-paikallisen tietokoneen tilin objekti. Seuraavat saatavat on oltava rekisteröintipalvelu Azure AD-laitteen rekisteröinti ja luo laitteen objekti todennuksen aikana:

- http://schemas.microsoft.com/ws/2012/01/AccountType sisältää DJ arvon, joka yksilöi pääasiallista käyttöoikeudet kuin toimialueeseen liitetyissä tietokoneissa.
- http://schemas.microsoft.com/Identity/claims/onpremobjectguid sisältää paikallisen tietokoneen tilin **objectGUID** -määritteen arvon.
- http://schemas.microsoft.com/ws/2008/06/Identity/claims/primarysid sisältää tietokoneen ensisijainen suojaustunnuksen (SUOJAUSTUNNUS), joka vastaa paikallisen tietokoneen tilin **objectSid** määrite-arvo.
- http://schemas.microsoft.com/ws/2008/06/Identity/claims/issuerid on arvo, jonka avulla Azure AD luota tunnuksen myöntää AD FS tai kohteesta-paikallisen suojauksen suojaustunnuksen Service (STS). Tämä on tärkeää usean metsää Active Directory-määritys. Tässä määrityksessä tietokoneita ehkä voi liittää metsää kuin se, joka muodostaa yhteyden Azure AD AD FS-palvelussa tai paikalliseen STS. AD FS-Liittoutumispalvelut arvoksi http://<*toimialuenimen*>/adfs/services/luota /, <*toimialuenimen*> on vahvistettu toimialuenimen Azure AD.

Sääntöjen luominen manuaalisesti, AD FS, käytä seuraavaa PowerShell-komentosarjaa istuntoon, joka on yhteydessä palvelimeen. Korvaa ensimmäisen rivin Azure AD-vahvistettu organisaation toimialuenimi.

> [AZURE.NOTE] Jos et käytä paikallista federation palvelimen AD FS-ohjeiden toimittajan Luo säännöt, jotka antaa nämä vaateet.

    $validatedDomain = "example.com"      # Replace example.com with your organization's validated domain name in Azure AD

    $rule1 = '@RuleName = "Issue object GUID"

        c1:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"] &&

        c2:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]

        => issue(store = "Active Directory", types = ("http://schemas.microsoft.com/identity/claims/onpremobjectguid"), query = ";objectguid;{0}", param = c2.Value);'

    $rule2 = '@RuleName = "Issue account type for domain-joined computers"

      c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]

      => issue(Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", Value = "DJ");'

> [AZURE.NOTE] Käytä vain kolme ensimmäistä sääntöä, jos oman ympäristön paikallisen on yksittäinen metsää. Jos tietokoneet ovat eri metsää kuin yksi synkronointiin Azure AD kanssa tai jos voit käyttää olevia synkronoinnin määritys vaihtoehtoinen nimet, sinun on lisättävä myös jäljellä olevia sääntöjä.

    $rule3 = '@RuleName = "Pass through primary SID"

      c1:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"] &&

      c2:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]

      => issue(claim = c2);'

    $rule4 = '@RuleName = "Issue AccountType with the value User when it’s not a computer account"

      NOT EXISTS([Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", Value == "DJ"])

      => add(Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", Value = "User");'

    $rule5 = '@RuleName = "Capture UPN when AccountType is User and issue the IssuerID"

      c1:[Type == "http://schemas.xmlsoap.org/claims/UPN"] &&

      c2:[Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", Value == "User"]

      => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c1.Value, ".+@(?<domain>.+)", "http://${domain}/adfs/services/trust/"));'

    $rule6 = '@RuleName = "Update issuer for DJ computer auth"

      c1:[Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", Value == "DJ"]

      => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = "http://'+$validatedDomain+'/adfs/services/trust/");'

    $existingRules = (Get-ADFSRelyingPartyTrust -Identifier urn:federation:MicrosoftOnline).IssuanceTransformRules

    $updatedRules = $existingRules + $rule1 + $rule2 + $rule3 + $rule4 + $rule5 + $rule6

    $crSet = New-ADFSClaimRuleSet -ClaimRule $updatedRules

    Set-AdfsRelyingPartyTrust -TargetIdentifier urn:federation:MicrosoftOnline -IssuanceTransformRules $crSet.ClaimRulesString

> [AZURE.NOTE] Windows 10: ssä ja Windows Server 2016 toimialueeseen liittyneet tietokoneet todennetaan käyttämällä IWA WS Salli aktiivisen päätepisteen, joka isännöi AD FS. Varmista, että päätepisteen on aktiivinen. Jos käytät Web-sovelluksen välityspalvelinta, muista, että tämä päätepiste julkaistaan välityspalvelimen kautta. Päätepisteen on adfs/palvelut ja Salli/13/windowstransport. Voit tarkistaa, onko käytössä-AD FS-hallintakonsoli Siirry **palvelun** > **päätepisteet**. Jos et käytä paikallista federation palvelimen AD FS-ohjeiden toimittajan Varmista, että vastaavan päätepisteen on aktiivinen.



## <a name="set-up-ad-fs-for-authentication-of-device-registration"></a>AD FS laitteen rekisteröinti todennuksen määrittäminen

Varmista, että IWA asetuksena on kelvollinen vaihtoehtona monimenetelmäisen todentamisen laitteen rekisteröinnin AD FS. Voit tehdä tämän haluat on liittoutumispalvelujen transform sääntö, joka kulkee todentamismenetelmä.

1. Siirry AD FS-hallintakonsoli **AD FS** > **Salli yhteydet** > **Käyttäisit osapuolen luottaa**.

2. Napsauta Microsoft Office 365: n tunnistetietojen Platform varmenteen käyttäjän osapuolen luota objektia hiiren kakkospainikkeella ja valitse sitten **Muokkaa ryhmän sääntöjä**.

3.  Valitse **Liittoutumispalvelujen Transform säännöt** -välilehdessä **Lisää sääntö**.

4.  Valitse **Varaa säännön** malli-luettelosta **Lähetä saatavat mukautetun säännön avulla**.

5.  Valitse **Seuraava**.

6.  Kirjoita **ryhmän säännön nimi** -ruutuun **Auth menetelmä varaa säännön**.

7.  Kirjoita **ryhmän säännön** -ruutuun tämän komennon:

    `c:[Type == "http://schemas.microsoft.com/claims/authnmethodsreferences"]
    => issue(claim = c);`.

8. Anna palvelimellesi federation PowerShell-komentoa:

    `Set-AdfsRelyingPartyTrust -TargetName <RPObjectName> -AllowedAuthenticationClassReferences wiaormultiauthn`

<*RPObjectName*> on Azure AD varmenteen käyttäjän osapuolen luota objektin varmenteen käyttäjän osapuolen objektinimi. Tämä objekti on yleensä nimetty *Microsoft Office 365: n Identity-ympäristössä*.



## <a name="deployment-and-rollout"></a>Käyttöönotto- ja käyttöönoton helpottaminen

Kun toimialueeseen liittyneet tietokoneet täyttävät tietyt edellytykset, ne ovat Azure AD rekisteröityä.

Windows 10 vuosipäivä Update- ja Windows Server 2016 toimialueeseen liittyneet tietokoneet rekisteröidä automaattisesti seuraavan kerran laitteen käynnistyy tai kun käyttäjä kirjautuu sisään Windows Azure AD. Uusi tietokoneissa, joihin on liitetty toimialueeseen rekisteröidä Azure AD, kun laite jälkeen toimialueen join-toiminto käynnistyy uudelleen.

> [AZURE.NOTE] Windows 10: ssä toimialueen liittyneet tietokoneet rekisteröidä automaattisesti Azure AD vain, jos rahoittaja ryhmäkäytäntöobjekti on määritetty.

Voit hallita Windows 10: ssä ja Windows Server 2016 toimialueeseen liittyneet tietokoneet automaattinen rekisteröinti rahoittaja ryhmäkäytäntöobjekti avulla. Windows 10 toimialueeseen liittyneet tietokoneet automaattinen rekisteröinti aloittamaan voit ottaa käyttöön Windows Installer-paketti tietokoneissa, joihin olet valinnut.

> [AZURE.NOTE] Ryhmäkäytäntö rahoittaja ohjausobjektin tuottaa myös Windows 8.1 toimialueeseen liittyneet tietokoneet rekisteröinti. Voit käyttää käytännön rekisteröitymisestä Windows 8.1 toimialueeseen liittyneistä tietokoneista. Tai jos käytössäsi on Windows-versioiden, kuten Windows 7 tai Windows Server-versiot sekä voit rekisteröidä Windows 10 ja Windows Server 2016 tietokoneiden käyttämällä Windows Installer-paketti.

### <a name="create-a-group-policy-object-to-control-the-rollout-of-automatic-registration"></a>Luo ryhmäkäytäntöobjekti, voit määrittää Automaattiset rekisteröinti rahoittaja

Voit määrittää toimialueen liittyneet tietokoneissa, joissa Azure AD automaattinen rekisteröinti rahoittaja, voit ottaa käyttöön **rekisteröidä toimialueeseen liittyneet tietokoneet kuin laitteet** -Ryhmäkäytäntö haluat rekisteröidä tietokoneissa. Voit esimerkiksi ottaa käytännön organisaatioyksikkö tai käyttöoikeusryhmän.

Jos haluat määrittää käytännön, toimi seuraavasti:

1. Avaa palvelimen hallinta ja valitse sitten **Työkalut** > **Ryhmäkäytäntöjen hallinta**.

2. Siirry toimialuesolmu, joka vastaa haluamaasi Aktivoi automaattinen rekisteröinti Windows 10: ssä tai Windows Server 2016 tietokoneiden toimialueen.

3. **Objektien ryhmittäminen käytännön**hiiren kakkospainikkeella ja valitse sitten **Uusi**.

4. Kirjoita Ryhmäkäytäntö objektin nimi. Esimerkiksi *Automaattinen rekisteröinti Azure AD*. Valitse **OK**.

4. Napsauta uusi ryhmäkäytäntöobjekti hiiren kakkospainikkeella ja valitse sitten **Muokkaa**.

5. Siirry **tietokoneen määritykset** > **käytännöt** > **järjestelmänvalvojan malleja** > **Windowsin osien** > **laitteen rekisteröinti**. Napsauta **Rekisteröi toimialueen liittyneet tietokoneita laitteet**ja valitse sitten **Muokkaa**.

    > [AZURE.NOTE] Ryhmäkäytäntö Tämä malli on nimetty uudelleen Ryhmäkäytäntöjen hallinta-konsolin aiemmissa versioissa. Jos käytössäsi on konsolin aiemmassa versiossa, siirry **Tietokoneen määritykset** > **käytännöt** > **Järjestelmänvalvojan malleja** > **Windowsin osien** > **Työpaikkasi liity** > **automaattisesti työpaikkasi liity asiakastietokoneiden**.

6. Valitse **käytössä**, ja valitse sitten **Käytä**.

7. Valitse **OK**.

8. Linkin tiettyyn kohtaan valittua ryhmäkäytäntöobjekti. Voit esimerkiksi linkittää sen tiettyyn organisaatioyksikkö. Voi linkittää sen tiettyyn käyttöoikeusryhmän tietokoneissa, joissa voit rekisteröidä automaattisesti Azure AD. Voit määrittää tämän käytännön kaikissa toimialueen liittyneet Windows 10: ssä ja Windows Server 2016 tietokoneissa organisaation-linkki ryhmäkäytäntöobjekti toimialueen.

### <a name="download-windows-installer-packages-for-non-windows-10-computers"></a>Lataa Windows Installer-pakettien Windows 10-tietokoneissa  

Voit rekisteröidä toimialueeseen liittyneet tietokoneet Windows 8.1, Windows 7: ssä, Windows Server 2012 R2, Windows Server 2012: ssa tai Windows Server 2008 R2, voit ladata ja asentaa Windows Installer-paketti (MSI) tiedostot:

- [x64](http://download.microsoft.com/download/C/A/7/CA79FAE2-8C18-4A8C-A4C0-5854E449ADB8/Workplace_x64.msi)
- [x86](http://download.microsoft.com/download/C/A/7/CA79FAE2-8C18-4A8C-A4C0-5854E449ADB8/Workplace_x86.msi)

Asentavat paketin ohjelmiston jakauman järjestelmän kuten System Center määritysten hallinta. Paketin tukee *hiljainen* käynnistysparametri hiljainen vakioasennuksen-asetukset. System Center hallintatoiminto 2016 on lisäksi muita etuja aiempien versioiden, kuten voi seurata valmiit merkintöjä. Lisätietoja on artikkelissa [System Center 2016](https://www.microsoft.com/en-us/cloud-platform/system-center).

Asennusohjelma luo ajoitetun tehtävän järjestelmään, joka suoritetaan käyttäjän kontekstissa. Tehtävän käynnistyy, kun käyttäjä kirjautuu Windowsiin. Tehtävän Rekisteröi laite äänettömästi Azure AD käyttäjän käyttöoikeuksilla kautta IWA tarkistamisen jälkeen. Jos haluat nähdä ajoitettua tehtävää, siirry **Microsoft** > **Työpaikkasi liity**ja valitse sitten Tehtävien ajoituksen kirjasto.



## <a name="next-steps"></a>Seuraavat vaiheet

- [Azure Active Directory ehdollisen käyttöoikeuden](active-directory-conditional-access.md)

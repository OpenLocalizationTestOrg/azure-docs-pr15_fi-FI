<properties
    pageTitle="Asennusohjeet äänettömästi Azure AD-sovelluksen välityspalvelimen yhdistimen | Microsoft Azure"
    description="Tässä artikkelissa käsitellään Azure AD sovelluksen välityspalvelimen yhdistimen paikallisen sovelluksia etäkäyttöä hiljainen asennus."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/22/2016"
    ms.author="kgremban"/>

# <a name="how-to-silently-install-the-azure-ad-application-proxy-connector"></a>Asennusohjeet äänettömästi Azure AD-sovelluksen välityspalvelimen-yhdistin

Haluat lähettää asennuksen komentosarjaa useita Windows-palvelimia tai Windows-palvelimet, joilla ei ole otettu käyttöön käyttöliittymä. Tässä ohjeaiheessa kerrotaan, miten voit luoda Windows PowerShell-komentosarja, joka mahdollistaa automaattisen asennuksen, asenna ja rekisteröi sitten Azure AD sovelluksen välityspalvelimen yhdistimen.

## <a name="enabling-access"></a>Accessin ottaminen käyttöön
Sovelluksen välityspalvelimen toimii asentamalla slim Windows Server-palvelu nimeltä verkoston sisällä yhdistin. Sovelluksen välityspalvelimen yhdistimen toimimaan se on rekisteröitävä Azure AD hakemistossa yleisenä järjestelmänvalvojana ja-salasanalla. Tavallisesti tämä on syötetty yhdistimen asennuksessa pop-valintaikkuna. Voit myös Windows PowerShellin avulla voit luoda tunnistetiedon objektin ja kirjoita rekisteröinnin tiedot tai voit luoda oman tunnuksen ja kirjoita rekisteröinnin tiedot sen avulla.

## <a name="step-1--install-the-connector-without-registration"></a>Vaihe 1: Asenna Connector ilman rekisteröinti


Asenna yhdistimen MSI-tiedostojen ilman rekisteröintiä yhdistimen seuraavasti:


1. Avaa komentorivi-ikkuna.
2. Suorita seuraava komento, jossa/q tarkoittaa hiljainen asennus - asennuksen kehottaa sinua Hyväksy käyttöoikeussopimus.

        AADApplicationProxyConnectorInstaller.exe REGISTERCONNECTOR="false" /q

## <a name="step-2-register-the-connector-with-azure-active-directory"></a>Vaihe 2: Rekisteröi Connector Azure Active Directory-hakemistosta
Tämä onnistuu jommallakummalla seuraavista tavoista:


- Rekisteröi Connector Windows PowerShell-tunnistetiedon objektin käyttäminen
- Rekisteröi offline-tilassa luotu tunnusta Connector

### <a name="register-the-connector-using-a-windows-powershell-credential-object"></a>Rekisteröi Connector Windows PowerShell-tunnistetiedon objektin käyttäminen


1. Windows PowerShell-tunnistetiedot-objektin luominen suorittamalla seuraavat tiedot, jossa "<username>"ja"<password>" korvataan käyttäjänimen ja salasanan hakemistossa:

        $User = "<username>"
        $PlainPassword = '<password>'
        $SecurePassword = $PlainPassword | ConvertTo-SecureString -AsPlainText -Force
        $cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $User, $SecurePassword

2. Siirry **C:\Program Files\Microsoft AAD sovelluksen välityspalvelimen yhdistimen** ja suorita PowerShell-tunnistetiedot objektin luomasi komentosarja, jossa $cred on PowerShell tunnistetiedot objekti, jonka loit:

        RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Credentials -Usercredentials $cred


### <a name="register-the-connector-using-a-token-created-offline"></a>Rekisteröi offline-tilassa luotu tunnusta Connector

1. Luo offline-tilassa tunnuksen, joka käyttää arvoja koodikatkelman AuthenticationContext-luokan avulla:


        using System;
        using System.Diagnostics;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;

        class Program
        {
        #region constants
        /// <summary>
        /// The AAD authentication endpoint uri
        /// </summary>
        static readonly Uri AadAuthenticationEndpoint = new Uri("https://login.windows.net/common/oauth2/token?api-version=1.0");

        /// <summary>
        /// The application ID of the connector in AAD
        /// </summary>
        static readonly string ConnectorAppId = "55747057-9b5d-4bd4-b387-abf52a8bd489";

        /// <summary>
        /// The reply address of the connector application in AAD
        /// </summary>
        static readonly Uri ConnectorRedirectAddress = new Uri("urn:ietf:wg:oauth:2.0:oob");

        /// <summary>
        /// The AppIdUri of the registration service in AAD
        /// </summary>
        static readonly Uri RegistrationServiceAppIdUri = new Uri("https://proxy.cloudwebappproxy.net/registerapp");

        #endregion

        #region private members
        private string token;
        private string tenantID;
        #endregion

        public void GetAuthenticationToken()
        {
            AuthenticationContext authContext = new AuthenticationContext(AadAuthenticationEndpoint.AbsoluteUri);

            AuthenticationResult authResult = authContext.AcquireToken(RegistrationServiceAppIdUri.AbsoluteUri,
                ConnectorAppId,
                ConnectorRedirectAddress,
                PromptBehavior.Always);

            if (authResult == null || string.IsNullOrEmpty(authResult.AccessToken) || string.IsNullOrEmpty(authResult.TenantId))
            {
                Trace.TraceError("Authentication result, token or tenant id returned are null");
                throw new InvalidOperationException("Authentication result, token or tenant id returned are null");
            }

            token = authResult.AccessToken;
            tenantID = authResult.TenantId;
        }





2. Kun olet tunnuksen luo SecureString, tunnuksen avulla: <br>
`$SecureToken = $Token | ConvertTo-SecureString -AsPlainText -Force`
3. Suorita seuraavat Windows PowerShell-komentoa, jossa SecureToken on edellä luomasi tunnuksen nimi ja tenantID on kohdassa vuokraajan GUID-tunnus: <br>
`RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Token -Token $SecureToken -TenantId <tenant GUID>`



## <a name="see-also"></a>Katso myös

- [Ottaa käyttöön sovelluksen välityspalvelinta Azure Active Directory](active-directory-application-proxy-enable.md)
- [Valinnaiseksi oman toimialuenimen käyttäminen](active-directory-application-proxy-custom-domains.md)
- [Yksi merkki ottaminen käyttöön](active-directory-application-proxy-sso-using-kcd.md)
- [Ilmenevien ongelmien sovelluksen välityspalvelimen ongelmien vianmääritys](active-directory-application-proxy-troubleshoot.md)

Uusimmat uutiset ja päivitykset Tutustu [sovelluksen välityspalvelin-blogi](http://blogs.technet.com/b/applicationproxyblog/)

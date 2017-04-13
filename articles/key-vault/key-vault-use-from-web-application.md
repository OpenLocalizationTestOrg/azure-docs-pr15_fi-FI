<properties
    pageTitle="Käytä Azure-Web-sovelluksen tärkeimmistä säilö | Microsoft Azure"
    description="Tässä opetusohjelmassa avulla voit opetella käyttämään Azure avaimen säilö WWW-sovelluksesta."
    services="key-vault"
    documentationCenter=""
    authors="adhurwit"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/05/2016"
    ms.author="adhurwit"/>

# <a name="use-azure-key-vault-from-a-web-application"></a>Käytä Azure avaimen säilö WWW-sovelluksesta #

## <a name="introduction"></a>Johdanto  
Tässä opetusohjelmassa avulla opit käyttämään Azure avaimen säilö Azure-web-sovelluksen. Se käydään läpi käyttäminen salaisuus Azure avain säilöstä niin, että sitä voi käyttää web-sovelluksen.

**Arvioitu kesto:** 15 minuuttia


Yleistietoja Azure avaimen säilö, katso [Azure avaimen säilö ominaisuudet?](key-vault-whatis.md)

## <a name="prerequisites"></a>Edellytykset

Jotta voit suorittaa tässä opetusohjelmassa, sinulla on oltava seuraavasti:

- URI, salaisuus Azure-avain säilöön
- Asiakas-tunnus ja asiakas-salaisuus web-sovelluksen rekisteröityjä Azure Active Directory, jolla on pääsy avain-säilö
- Web-sovelluksen. Microsoft näkyvissä ASP.NET MVC-sovelluksen käyttöön Azure kuin verkkosovellukseen vaiheet.

> [AZURE.NOTE]  On tärkeää, että olet suorittanut kerrotut vaiheet [Aloittaminen Azure avaimen säilö](key-vault-get-started.md) opetusohjelmassa niin, että sinulla salaisuus ja Asiakastunnus-ja asiakkaan salaisuus URI web-sovelluksen.

Web-sovelluksen pääsevät avaimen säilö on se, joka on rekisteröity Azure Active Directory ja on annettu access avaimen säilö. Jos tämä ei ole, siirry takaisin Rekisteröi sovelluksen käytön aloittaminen-opetusohjelma ja toista vaiheet-luettelossa.

Tässä opetusohjelmassa on tarkoitettu web-kehittäjille, web-sovellusten luominen Azure perusteet. Saat lisätietoja Azure verkkosovelluksissa [Web Apps -sovellusten yleiskatsaus](../app-service-web/app-service-web-overview.md).



## <a id="packages"></a>Lisää Nuget-paketit ##
On kaksi pakettien web-sovellus on asennettuna.

- Active Directory käyttöoikeuksien kirjasto - sisältää menetelmiä käyttäminen Azure Active Directory ja käyttäjätietojen hallinta
- Azure avaimen säilö kirjasto - sisältää menetelmiä Azure avaimen säilö käyttäminen


Toinen pakkauksissa voidaan asentaa asennuksen paketti-komennon avulla pakettien hallinta-konsolin avulla.

    // this is currently the latest stable version of ADAL
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.16.204221202

    Install-Package Microsoft.Azure.KeyVault


## <a id="webconfig"></a>Muokkaa Web.Config ##
On kolme sovellusasetuksia, jotka on lisättävä seuraavan koodin korostetut seuraavasti.

    <!-- ClientId and ClientSecret refer to the web application registration with Azure Active Directory -->
    <add key="ClientId" value="clientid" />
    <add key="ClientSecret" value="clientsecret" />

    <!-- SecretUri is the URI for the secret in Azure Key Vault -->
    <add key="SecretUri" value="secreturi" />


Jos aiot ei isännöi kuin Azure Web App-sovelluksen, ClientId, asiakkaan salaisuus ja salaisuus URI soluviittauksiaarvojen kannattaa lisätä web.config. Jätä muuten tyhjä arvot, koska olemme lisäämiseen todellisten arvojen muita suojaustasoa, Azure-portaalissa.


## <a id="gettoken"></a>Lisää menetelmä voit käyttää tunnus ##
Jotta voit käyttää Ohjelmointirajapinnan avain säilö on access-tunnuksen. Avaimen säilö asiakas käsittelee puhelut ohjelmointirajapinnan avain säilöön, mutta sinun on lisättävä funktio, joka hakee access-tunnuksen.  

Seuraavassa on koodin hankkiminen access suojaustunnuksen Azure Active Directorysta. Tämä koodi siirtyä mihin tahansa sovelluksen. Haluan lisätä Utils tai EncryptionHelper luokan.  

    //add these using statements
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System.Threading.Tasks;
    using System.Web.Configuration;

    //this is an optional property to hold the secret after it is retrieved
    public static string EncryptSecret { get; set; }

    //the method that will be provided to the KeyVaultClient
    public static async Task<string> GetToken(string authority, string resource, string scope)
    {
        var authContext = new AuthenticationContext(authority);
        ClientCredential clientCred = new ClientCredential(WebConfigurationManager.AppSettings["ClientId"],
                    WebConfigurationManager.AppSettings["ClientSecret"]);
        AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

        if (result == null)
            throw new InvalidOperationException("Failed to obtain the JWT token");

        return result.AccessToken;
    }

> [AZURE.NOTE] 
> Helpoin tapa todentaa Azure AD-sovelluksen käyttäminen Asiakastunnus ja asiakkaan salaisuus on. Ja web-sovelluksen käyttäminen mahdollistaa erottelua tehtävät ja määrittää tarkemmin avaimen hallinta. Mutta se riippuvaisia asiakkaan salaisuus sijoittaminen joka joillekin voi olla riskialtis niin kuin laajennettujen toiminta, jonka haluat suojata Hakumääritysten asetusten määritykset. Noudata seuraavia ohjeita keskustelun käyttämisestä Asiakastunnus ja varmenteen todentamiseen Azure AD-sovelluksen Asiakastunnus ja asiakkaan salaisuus sijaan.



## <a id="appstart"></a>Noutaa toiminta-sovelluksen käynnistäminen ##
Nyt annettava Soita säilö Ohjelmointirajapinnan avain ja hakea toiminta-koodi. Seuraava koodi voidaan sijoittaa mihin tahansa, kunhan, jonka nimi on ennen sitä käytetään. Olen siirtänyt koodi sovelluksen käynnistää tapahtuman Global.asax niin, että suoritetaan kerran, valitse Käynnistä ja toiminto toiminta sovelluksen käyttöön.

    //add these using statements
    using Microsoft.Azure.KeyVault;
    using System.Web.Configuration;

    // I put my GetToken method in a Utils class. Change for wherever you placed your method.
    var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetToken));

    var sec = kv.GetSecretAsync(WebConfigurationManager.AppSettings["SecretUri"]).Result.Value;

    //I put a variable in a Utils class to hold the secret for general  application use.
    Utils.EncryptSecret = sec;



## <a id="portalsettings"></a>Sovelluksen asetusten lisääminen (valinnainen) Azure-portaalissa ##
Jos sinulla on Azure Web App-sovelluksessa voit nyt lisätä todellisia arvoja, AppSettings Azure-portaalissa. Näin todellisten arvojen eivät ole web.config, mutta joissa erilliset käyttöoikeudet ohjausobjektin ominaisuuksia portaalin kautta suojattu. Nämä arvot korvataan arvot, joita käytit oman Web.config. Varmista, että nimet ovat samat.

![Sovelluksen asetukset näkyviin Azure-portaalissa][1]


## <a name="authenticate-with-a-certificate-instead-of-a-client-secret"></a>Sen sijaan, että asiakas salaisuus varmenteen todentamismenetelmä
Voi todentaa Azure AD-sovellus on käyttää asiakkaan-tunnus ja sertifikaatin Asiakastunnus ja asiakkaan salaisuus sijaan. Ohjeet, joiden avulla varmenteen Azure Web App-sovelluksessa ovat seuraavat:

1. Hae tai varmenteen luominen
2. Varmenteen liittäminen Azure AD-sovellus
3. Koodin lisääminen Web-sovelluksen käyttämään varmenne
4. Varmenteen lisääminen Web Appissa


**Hae tai varmenteen luominen** Saat parhaiten tarkoituksiimme on julkaista testi varmenne. Komennot, jotka voit käyttää varmenteen-Developer Komentorivi on muutamia. Vaihda kansio, johon haluat luoda varmenne-tiedostoja.

    makecert -sv mykey.pvk -n "cn=KVWebApp" KVWebApp.cer -b 07/31/2015 -e 07/31/2016 -r
    pvk2pfx -pvk mykey.pvk -spc KVWebApp.cer -pfx KVWebApp.pfx -po test123

Päivämäärä- ja salasana .pfx muistiin (Tässä esimerkissä: 07/31/2016 ja test123). Tarvitset niitä alla.

Lisätietoja Testivarmenteen luomisesta on artikkelissa [Toimintaohje: Luo oma oman testata sertifikaatti](https://msdn.microsoft.com/library/ff699202.aspx)


**Liitä varmenteen Azure AD-sovelluksen kanssa** Nyt kun olet luonut varmenteen, sinun on liitettävä Azure AD-sovelluksen. Mutta Azure hallinta-portaalin ei tue tämän tällä hetkellä. Sen sijaan sinun on käytettävä Powershell. Komennot, jotka sinun on suoritettava ovat seuraavat:

    $x509 = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2

    PS C:\> $x509.Import("C:\data\KVWebApp.cer")

    PS C:\> $credValue = [System.Convert]::ToBase64String($x509.GetRawCertData())

    PS C:\> $now = [System.DateTime]::Now

    # this is where the end date from the cert above is used
    PS C:\> $yearfromnow = [System.DateTime]::Parse("2016-07-31")

    PS C:\> $adapp = New-AzureRmADApplication -DisplayName "KVWebApp" -HomePage "http://kvwebapp" -IdentifierUris "http://kvwebapp" -KeyValue $credValue -KeyType "AsymmetricX509Cert" -KeyUsage "Verify" -StartDate $now -EndDate $yearfromnow

    PS C:\> $sp = New-AzureRmADServicePrincipal -ApplicationId $adapp.ApplicationId

    PS C:\> Set-AzureRmKeyVaultAccessPolicy -VaultName 'contosokv' -ServicePrincipalName $sp.ServicePrincipalName -PermissionsToSecrets all -ResourceGroupName 'contosorg'

    # get the thumbprint to use in your app settings
    PS C:\>$x509.Thumbprint

Kun olet suorittanut näitä komentoja, näet Azure AD-sovellus. Jos et näe "Sovellusten yritykseni omistaa" ensin hakeminen sovelluksen sijaan "Sovellusten yritykseni käyttää".

Lisätietoja Azure AD-sovelluksen objektit ja ServicePrincipal objektit on artikkelissa [sovelluksen ja palvelun lyhennys objektit](../active-directory/active-directory-application-objects.md)



**Lisää koodi Web App-sovellukseen, voit käyttää varmenteen** On nyt lisää koodi Web App-sovellukseen, access varmenteen ja todennusta varten.

On ensin koodin käyttämään varmenne.

    public static class CertificateHelper
    {
        public static X509Certificate2 FindCertificateByThumbprint(string findValue)
        {
            X509Store store = new X509Store(StoreName.My, StoreLocation.CurrentUser);
            try
            {
                store.Open(OpenFlags.ReadOnly);
                X509Certificate2Collection col = store.Certificates.Find(X509FindType.FindByThumbprint,
                    findValue, false); // Don't validate certs, since the test root isn't installed.
                if (col == null || col.Count == 0)
                    return null;
                return col[0];
            }
            finally
            {
                store.Close();
            }
        }
    }


Huomaa, että StoreLocation-arvon CurrentUser LocalMachine sijaan. Ja että olemme ovat "false" Etsi menetelmä koska testi-varmenne on käytössä.


On seuraava koodi, joka käyttää CertificateHelper ja luo ClientAssertionCertificate, joka tarvitaan todennusta varten.

    public static ClientAssertionCertificate AssertionCert { get; set; }

    public static void GetCert()
    {
        var clientAssertionCertPfx = CertificateHelper.FindCertificateByThumbprint(WebConfigurationManager.AppSettings["thumbprint"]);
        AssertionCert = new ClientAssertionCertificate(WebConfigurationManager.AppSettings["clientid"], clientAssertionCertPfx);
    }


Näin saat käyttöoikeustietue uusi koodi. Edellä kuvattuja GetToken korvataan. Voin valita sen eri nimellä helpottamiseksi.

    public static async Task<string> GetAccessToken(string authority, string resource, string scope)
    {
        var context = new AuthenticationContext(authority, TokenCache.DefaultShared);
        var result = await context.AcquireTokenAsync(resource, AssertionCert);
        return result.AccessToken;
    }

Minulla on sijoittaa kaikki tämän koodin helppous Web App-projektin Utils-luokka.

Koodin viimeisin muutos on Application_Start-menetelmää. Ensin annettava ladata ClientAssertionCertificate GetCert()-menetelmää. Ja sitten muutamme Takaisinkutsumenetelmän, joka on annettava, kun luot uuden KeyVaultClient. Huomaa, että tämä korvaa tunnus, jolla on ollut yli.

    Utils.GetCert();
    var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetAccessToken));


**Lisää sertifikaatti Web App-sovellukseen, palvelun Azure-portaalissa** Sertifikaatin lisääminen Web-sovelluksen on yksinkertainen kaksivaiheinen prosessi. Tarkista ensin Siirry Azure-portaaliin ja siirry Web App. Valitse merkintä "Mukautetut toimialueet ja SSL-Web-sovelluksen asetukset-sivu. Valitse sivu, joka avautuu, jonka saa oikeuden Lataa sertifikaatti, jonka loit edellisessä taulukossa, KVWebApp.pfx, varmista, että muistat salasanan pfx.

![Sertifikaatin lisääminen verkkosovellukseen Azure-portaalissa][2]


Viimeiseksi, sinun on suoritettava on Lisää sovellus-asetus, jossa on nimi-sivuston koodiin\_KUORMITUKSEN\_varmenteet ja arvoa *. Näin varmistat, että kaikki varmenteet ladataan. Jos haluat ladata vain sertifikaatteja, joita olet ladannut, voit kirjoittaa heidän thumbprints CSV-luettelo.

Lisätietoja sertifikaatin lisääminen verkkosovellukseen on artikkelissa [Azure sivustot-sovellusten käyttäminen varmenteet](https://azure.microsoft.com/blog/2014/10/27/using-certificates-in-azure-websites-applications/)


**Lisää varmenteen avaimen säilö salaisuus nimellä** Sen sijaan, että sertifikaatti lataaminen Online-palvelun suoraan, voit tallentaa avaimen säilöön salaisuus nimellä ja käyttöönotto sieltä. Tämä on kaksivaiheinen prosessi, jonka ympärillä on seuraavassa blogimerkinnässä [Käyttöönotto Azure Web App varmenteen avaimen säilö kautta](https://blogs.msdn.microsoft.com/appserviceteam/2016/05/24/deploying-azure-web-app-certificate-through-key-vault/)



## <a id="next"></a>Seuraavat vaiheet ##


Ohjelmointi viittaukset-kohdassa [Azure avaimen säilö C# asiakkaan API Ohje](https://msdn.microsoft.com/library/azure/dn903628.aspx).


<!--Image references-->
[1]: ./media/key-vault-use-from-web-application/PortalAppSettings.png
[2]: ./media/key-vault-use-from-web-application/PortalAddCertificate.png

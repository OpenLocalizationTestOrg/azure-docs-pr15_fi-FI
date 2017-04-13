<properties 
    pageTitle="Azure Active Directory-integraation paikallisen käyttäjätietoja."
    description="Tämä on Azure AD Connect, jossa kuvataan, mitä ja miksi sitä pitäisi käyttää."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="building-multi-factor-authentication-into-custom-apps-sdk"></a>Rakennuksen Monimenetelmäisen todentamisen mukautettujen sovellusten (SDK) tuominen

> [AZURE.IMPORTANT]  Jos haluat ladata SDK, sinun on Azure multi-factor Auth tarjoajan luomiseen, vaikka sinulla olisi Azure MFA, AAD Premium tai EMS käyttöoikeudet.  Jos olet luonut Azure multi-factor Auth tarjoajan tähän tarkoitukseen ja on jo käyttöoikeudet, sitä tarvitaan Luo palvelu **Käyttöön käyttäjään** mallin kanssa ja linkittää Azure MFA, Azure AD Premium tai EMS käyttöoikeudet sisältävän kansion toimittaja.  Näin varmistat, että sinun on ei ole laskutettu paitsi, jos sinulla on useita eri käyttäjää käyttämällä SDK kuin omistat käyttöoikeuksien määrä.

Azure multi-factor Authentication Software Development Kit (SDK) voit luoda puheluista ja teksti viestin tarkastaminen suoraan sisään tai tapahtuman prosessit Azure AD-vuokraajan sovellukset.

Multi-factor Authentication SDK on käytettävissä C#, Visual Basic (.NET), Java, Perl, PHP ja Ruby. SDK sisältää monimenetelmäisen todentamisen ympärille ohuen paketti. Se sisältää kaikki sinun on kirjoitettava koodi, mukaan lukien on kommentit lähdetiedostot koodi, esimerkiksi tiedostot ja yksityiskohtainen Lueminut-tiedosto. Kunkin SDK myös varmenne ja yksityinen avain salaamiseen tapahtumia, jotka on yksilöllinen multi-factor Authentication-palveluntarjoajaan. Kunhan sinulla palveluntarjoaja, voit ladata SDK niin monta kieliä ja muotoilut kuin tarvitset.

Multi-factor Authentication-SDK-ohjelmointirajapinnan rakenne on yksinkertainen. Voit tehdä yhden funktion kutsu Ohjelmointirajapinnan multi-factor vaihtoehto parametreilla, kuten vahvistus-tilassa ja käyttäjätietoja, kuten Soita puhelinnumero tai tunnusluku vahvistamiseen. API kääntää funktiokutsun kyselyjä web services pyynnöt pilvipohjainen Azure multi-factor Authentication Service. Kaikki puhelut täytyy sisältää yksityinen varmenne, joka sisältyy jokaisen SDK viittaus.

Koska API ei ole rekisteröity Azure Active Directoryn käyttäjien pääsyn, sinun on määritettävä käyttäjätiedot, kuten puhelinnumeroita ja PIN-koodit, tiedoston tai tietokannan. Lisäksi API ei ole rekisteröinti tai käyttäjän hallintaominaisuudet, joten sinun täytyy luoda nämä prosessit sovellukseen.






## <a name="download-the-azure-multi-factor-authentication-sdk"></a>Lataa Azure Monimenetelmäisen todentamisen SDK-paketissa

Lataaminen Azure multi-factor SDK edellyttää [Azure multi-factor Auth toimittaja](multi-factor-authentication-get-started-auth-provider.md).  Tämä edellyttää täydet Azure-tilauksen, vaikka Azure MFA, Azure AD Premium tai yrityksen Mobility Suite käyttöoikeuksien omistamasi.  Voit ladata SDK-edellyttää, siirryt multi-factor hallinta-portaalin hallinta multi-factor Auth palveluntarjoajan suoraan mukaan tai valitsemalla MFA-palvelun asetukset-sivun **"Siirry portaalin"** -linkki.


### <a name="to-download-the-azure-multi-factor-authentication-sdk-from-the-azure-portal"></a>Voit ladata Azure multi-factor Authentication SDK Azure-portaalista


1. Kirjaudu Azure-portaaliin järjestelmänvalvojana.
2. Valitse vasemmassa reunassa Active Directorysta.
3. Valitse Active Directory-sivun yläreunassa **Multi-Factor Auth tarjoajat**
4. Valitse **hallinta**
5. Tämä avaa uuden sivun.  Valitse vasemmassa reunassa ja SDK: ssa.
<center>![Lataa](./media/multi-factor-authentication-sdk/download.png)</center>
6. Valitse haluamasi kieli ja valitse yhdelle liittyvät linkkien.
7. Tallenna ladattava.



### <a name="to-download-the-azure-multi-factor-authentication-sdk-via-the-service-settings"></a>Voit ladata Azure multi-factor Authentication SDK kautta Palveluasetukset


1. Kirjaudu Azure-portaaliin järjestelmänvalvojana.
2. Valitse vasemmassa reunassa Active Directorysta.
3. Kaksoisnapsauta oman Azure AD-esiintymässä.
4. Valitse yläosasta kohta **määrittäminen**
5. Valitse monimenetelmäisen todentamisen **hallinta Palveluasetukset**
![lataaminen](./media/multi-factor-authentication-sdk/download2.png)
6. Valitse palvelut asetukset-sivun näytön alareunassa valitsemalla **-portaalista**.
![Lataa](./media/multi-factor-authentication-sdk/download3a.png)
7. Tämä avaa uuden sivun.  Valitse vasemmassa reunassa ja SDK: ssa.
8. Valitse haluamasi kieli ja valitse yhdelle liittyvät linkkien.
9. Tallenna ladattava.

## <a name="contents-of-the-azure-multi-factor-authentication-sdk"></a>Sisällön Azure Monimenetelmäisen todentamisen SDK-paketissa
Sisällä SDK etsii seuraavia kohteita:

- **Lueminut-tiedosto**. Kerrotaan, miten voit käyttää multi-factor Authentication-ohjelmointirajapinnan uuteen tai aiemmin luotuun-sovelluksessa.
- Monimenetelmäisen todentamisen **lähde-tiedostot**
- **Asiakasvarmenne** , jota käytetään multi-factor Authentication-palvelun yhteydessä
- Varmenteen **yksityinen avain**
- **Soita tulokset.** Puhelun tuloksen koodit luettelo. Avaa tiedosto-ohjelmiin muotoilua, kuten WordPad. Korjauksella puhelun tuloksen testaus ja vianmääritys sovelluksen Monimenetelmäisen todentamisen käyttöönoton. Ne eivät ole todennus tilakoodin.
- **Esimerkkejä.** Esimerkki Monimenetelmäisen todentamisen basic toimimasta-soveltaminen koodi.


>[AZURE.WARNING]Asiakkaan varmenne on yksilöllinen yksityinen varmenne, joka on luotu etenkin. Älä jaa tai menettää tätä tiedostoa. On avulla voit varmistaa, että viestintää multi-factor Authentication-palvelun suojausta.

## <a name="code-sample-standard-mode-phone-verification"></a>Koodi-malli: Vakiotilassa puhelimen todentaminen

Koodin tässä esimerkissä näytetään, miten vakiotilassa voice puhelun vahvistus lisääminen sovelluksen Azure multi-factor Authentication SDK API avulla. Vakiotilassa on puheluihin, jotka käyttäjä vastaa painamalla #-näppäintä.

Tässä esimerkissä käytetään C# .NET 2.0 multi-factor Authentication SDK ja C# palvelinpuolen logiikan basic ASP.NET-sovelluksessa, mutta prosessi on hyvin samantyyppinen yksinkertainen käyttöotot muilla kielillä. Koska SDK sisältää lähdetiedostot, ei suoritettavat tiedostot, voit luoda tiedostot ja värisinä tai sisällyttää ne suoraan sovelluksessa.

>[AZURE.NOTE]Kun Monimenetelmäisen todentamisen, Määritä muut tekijät toisen tai kolmannen tason vahvistus täydentää ensisijainen todentamismenetelmän. Näistä tavoista ei ole suunniteltu, jota käytetään ensisijainen todennustavat.

### <a name="code-sample-overview"></a>Koodin otoksen yleiskuvaus
Tämä esimerkki koodi erittäin yksinkertaisia verkkosovellusten esittely käyttävä # avaimen vastauksen puhelun suorittamiseen käyttäjän todennusta. Puhelun kertoimen kutsutaan Monimenetelmäisen todentamisen vakiotilassa.

Asiakkaan-koodia ei ole multi-factor Authentication-kohtaisia elementtejä. Koska todennus tekijät eivät määräydy ensisijainen käyttöoikeuden, voit lisätä ne muuttamatta aiemmin Sign-käyttöliittymän. Multi-factor SDK-ohjelmointirajapinnan avulla voit mukauttaa käyttäjäkokemuksen, mutta voit joutua ei voi muokata mitään ollenkaan.

Palvelinpuolen koodin Lisää standard todennusta vaiheessa 2. Se luo PfAuthParams objektin vakio-tilassa vahvistusta varten tarvittavat parametrit: käyttäjänimi, puhelin numero ja tila ja Asiakasvarmenne (CertFilePath), joka vaaditaan kunkin kutsussa polku. Katso esittelyyn, kaikki parametrien PfAuthParams, esimerkiksi tiedoston SDK: ssa.

Seuraavaksi koodin välittää PfAuthParams objektin pf_authenticate()-funktiota. Palautusarvo ilmaisee onnistumisesta tai epäonnistumisesta todennusta. Out parametrit callStatus ja taltioitu, sisältävät muita puhelutiedot tuloksen. Puhelun tuloksen koodit on kuvattu SDK: ssa kutsu tulokset-tiedosto.

Tämä mahdollisimman vähän toteutus voidaan kirjoittaa vain muutaman riveillä. Kuitenkin tuotannon koodissa sisällyttää kehittyneempiä Virheenkäsittely ja muita tietokannan koodi käyttökokemusta.

### <a name="web-client-code"></a>Web-asiakasohjelman koodi

Seuraavassa on web client koodit esittely-sivulta.


    <%@ Page Language="C#" AutoEventWireup="true" CodeFile="Default.aspx.cs" Inherits="_Default" %>

    <!DOCTYPE html>

    <html xmlns="http://www.w3.org/1999/xhtml">
    <head runat="server">
    <title>Multi-Factor Authentication Demo</title>
    </head>
    <body>
    <h1>Azure Multi-Factor Authentication Demo</h1>
    <form id="form1" runat="server">

    <div style="width:auto; float:left">
    Username:&nbsp;<br />
    Password:&nbsp;<br />
    </div>

    <div">
    <asp:TextBox id="username" runat="server" width="100px"/><br />
    <asp:Textbox id="password" runat="server" width="100px" TextMode="password" /><br />
    </div>

    <asp:Button id="btnSubmit" runat="server" Text="Log in" onClick="btnSubmit_Click"/>

    <p><asp:Label ID="lblResult" runat="server"></asp:Label></p>

    </form>
    </body>
    </html>


### <a name="server-side-code"></a>Palvelinpuolen koodi

Palvelinpuolen seuraava koodi Monimenetelmäisen todentamisen on määritetty ja Suorita vaiheessa 2. Vakiotilassa (MODE_STANDARD) on puheluista, johon käyttäjä vastaa painamalla #-näppäintä.

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.Web.UI;
    using System.Web.UI.WebControls;

    public partial class _Default : System.Web.UI.Page
    {
        protected void Page_Load(object sender, EventArgs e)
        {
        }

        protected void btnSubmit_Click(object sender, EventArgs e)
        {
            // Step 1: Validate the username and password
            if (username.Text != "Contoso" || password.Text != "password")
            {
                lblResult.ForeColor = System.Drawing.Color.Red;
                lblResult.Text = "Username or password incorrect.";
            }
            else
            {
                // Step 2: Perform multi-factor authentication

                // Add call details from the user database.
                PfAuthParams pfAuthParams = new PfAuthParams();
                pfAuthParams.Username = username.Text;
                pfAuthParams.Phone = "9134884271";
                pfAuthParams.Mode = pf_auth.MODE_STANDARD;

                // Specify a client certificate
                // NOTE: This file contains the private key for the client
                // certificate. It must be stored with appropriate file
                // permissions.
                pfAuthParams.CertFilePath = "c:\\cert_key.p12";

                // Perform phone-based authentication
                int callStatus;
                int errorId;

                if(pf_auth.pf_authenticate(pfAuthParams, out callStatus, out errorId))
                {
                    lblResult.ForeColor = System.Drawing.Color.Green;
                    lblResult.Text = "Multi-Factor Authentication succeeded.";
                }
                else
                {
                    lblResult.ForeColor = System.Drawing.Color.Red;
                    lblResult.Text = " Multi-Factor Authentication failed.";
                }
            }

        }
    }

<properties 
    pageTitle="Voit soittaa puhelun-Twilio (.NET) | Microsoft Azure" 
    description="Opettele puhelun soittaminen ja lähettää Azure tekstiviesti Twilio API-palvelussa. Kirjoitettu .NET MALLIKOODEJA." 
    services="" 
    documentationCenter=".net" 
    authors="devinrader" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="05/04/2016" 
    ms.author="microsofthelp@twilio.com"/>




# <a name="how-to-make-a-phone-call-using-twilio-in-a-web-role-on-azure"></a>Miten Twilio käyttäminen web-roolin Azure puhelun soittaminen

Tässä oppaassa kerrotaan, miten soittaminen verkkosivulta ylläpidettävä Azure Twilio avulla. Tuloksena sovellus kysyy puhelun arvoja, kuten seuraavassa näyttökuvassa.

![Azure puhelun lomakkeen Twilio ja ASP.NET käyttäminen][twilio_dotnet_basic_form]

## <a name="twilio-prereqs"></a>Edellytykset

Sinun on käytettävä koodi tämän ohjeaiheen seuraavasti:

1. Hankkia Twilio tili ja todennusta tunnuksen. Aloita Twilio, kirjaudut [https://www.twilio.com/try-twilio][try_twilio]. Voit laskea hinnat osoitteessa [http://www.twilio.com/pricing][twilio_pricing]. Lisätietoja Twilio myöntämä Ohjelmointirajapinnan on artikkelissa [http://www.twilio.com/voice/api][twilio_api].
2. Twilio .NET-kirjaston lisääminen web-rooli. Katso tämän artikkelin "lisäämään Twilio kirjastojen roolin projektin".

Pitäisi olla tuttuja basic web-roolin luominen Azure.

## <a name="howtocreateform"></a>Toimintaohje: puhelun soittaminen web lomakkeen luominen

<a id="use_nuget"></a>Voit lisätä Twilio kirjastojen web roolin projektin seuraavasti:

1.  Avaa-ratkaisun Visual Studiossa.
2.  Napsauta hiiren kakkospainikkeella **viittauksia**.
3.  Valitse **Hallitse NuGet paketit**.
4.  Valitse **online-tilaan**.
5.  Kirjoita Etsi online-ruutuun *twilio*.
6.  Valitse **Asenna** Twilio tuotenumeroa.

Seuraava koodi esitetään, kuinka voit noutaa puhelun soittaminen käyttäjätietoja web-lomakkeen luominen. Tässä esimerkissä ASP.NET web rooli, jonka **TwilioCloud** luodaan.

    <%@ Page Title="Home Page" Language="C#" MasterPageFile="~/Site.master"
        AutoEventWireup="true" CodeBehind="Default.aspx.cs"
        Inherits="WebRole1._Default" %>

    <asp:Content ID="HeaderContent" runat="server" ContentPlaceHolderID="HeadContent">
    </asp:Content>
    <asp:Content ID="BodyContent" runat="server" ContentPlaceHolderID="MainContent">
        <div>
            <asp:BulletedList ID="varDisplay" runat="server" BulletStyle="NotSet">
            </asp:BulletedList>
        </div>
        <div>
            <p>Fill in all fields and click <b>Make this call</b>.</p>
            <div>
                To:<br /><asp:TextBox ID="toNumber" runat="server" /><br /><br />
                Message:<br /><asp:TextBox ID="message" runat="server" /><br /><br />
                <asp:Button ID="callpage" runat="server" Text="Make this call"
                    onclick="callpage_Click" />
            </div>
        </div>
    </asp:Content>

## <a id="howtocreatecode"></a>Toimintaohje: soittaa koodin luominen
Seuraava koodi, jota kutsutaan, kun käyttäjä on valmis lomakkeen, Luo puhelun viestin ja luo puhelun. Tässä esimerkissä koodi suoritetaan napsautettaessa tapahtumakäsittelijä painikkeen-lomakkeessa. (Käytä Twilio tili ja todennusta tunnussanoma **accountSID** ja **authToken** alla olevassa koodissa paikkamerkin arvojen sijaan).

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.Web.UI;
    using System.Web.UI.WebControls;
    using Twilio;

    namespace WebRole1
    {
        public partial class _Default : System.Web.UI.Page
        {
            protected void Page_Load(object sender, EventArgs e)
            {

            }

            protected void callpage_Click(object sender, EventArgs e)
            {
                // Call porcessing happens here.

                // Use your account SID and authentication token instead of
                // the placeholders shown here.
                string accountSID = "ACNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN";
                string authToken =  "NNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN";

                // Instantiate an instance of the Twilio client.
                TwilioRestClient client;
                client = new TwilioRestClient(accountSID, authToken);

                // Retrieve the account, used later to retrieve the
                Twilio.Account account = client.GetAccount();
                string APIversuion = client.ApiVersion;
                string TwilioBaseURL = client.BaseUrl;

                this.varDisplay.Items.Clear();
                if (this.toNumber.Text == "" || this.message.Text == "")
                {
                    this.varDisplay.Items.Add(
                            "You must enter a phone number and a message.");
                }
                else
                {
                    // Retrieve the values entered by the user.
                    string to = this.toNumber.Text;
                    string myMessage = this.message.Text;

                    // Create a URL using the Twilio message and the user-entered
                    // text. You must replace spaces in the user's text with '%20'
                    // to make the text suitable for a URL.
                    String Url = "http://twimlets.com/message?Message%5B0%5D="
                            + myMessage.Replace(" ", "%20");

                    // Display the endpoint, API version, and the URL for the message.
                    this.varDisplay.Items.Add("Using Twilio endpoint "
                        + TwilioBaseURL);
                    this.varDisplay.Items.Add("Twilioclient API Version is "
                        + APIversuion);
                    this.varDisplay.Items.Add("The URL is " + Url);

                    // Instantiate the call options that are passed
                    // to the outbound call.
                    CallOptions options = new CallOptions();

                    // Set the call From, To, and URL values.                    
                    options.From = "+14155992671";
                    options.To = to;
                    options.Url = Url;

                    // Place the call.
                    var call = client.InitiateOutboundCall(options);
                    this.varDisplay.Items.Add("Call status: " + call.Status);
                }
            }
        }
    }

Puhelu soitetaan ja Twilio päätepisteen, API-versio ja puhelun tila näytetään. Seuraavassa näyttökuvassa näkyy esimerkki Suorita tulosteen.

![Azure puhelun vastauksen Twilio ja ASP.NET käyttäminen][twilio_dotnet_basic_form_output]

Lisätietoja TwiML tukikäytännöistä [http://www.twilio.com/docs/api/twiml][twiml]. Lisätietoja &lt;sanoa&gt; ja muut Twilio verbien tukikäytännöistä [http://www.twilio.com/docs/api/twiml/say][twilio_say].

## <a id="nextsteps"></a>Seuraavat vaiheet
Tämä koodi määritettyä näyttämään Twilio käyttäminen ASP.NET web-rooli Azure-perustoiminnot. Ennen kuin otat käyttöön Azure tuotannon, haluat ehkä lisätä Lisää Virheenkäsittely tai muita ominaisuuksia. Esimerkki:

* Onnistunut Käytä sijaan verkkolomakkeen tallentamiseen puhelinnumerot ja soita tekstin Azure-Blob-säiliö tai erillisen Azure SQL-tietokanta. Lisätietoja Azure BLOB käyttämisestä on artikkelissa [.NET Azure Blob storage-palvelun käyttäminen][howto_blob_storage_dotnet]. Lisätietoja SQL-tietokannan käyttämisestä on artikkelissa [Azure SQL-tietokannan .NET-sovellusten käyttämisestä][howto_sql_azure_dotnet].
* Voit käyttää RoleEnvironment.getConfigurationSettings noutaa Twilio Tilitunnus ja todennus tunnuksen koko käyttöönotto määritysten asetukset-sijaan Kova-koodaus lomakkeen arvot. Lisätietoja RoleEnvironment-luokka on artikkelissa [Microsoft.WindowsAzure.ServiceRuntime Namespace][azure_runtime_ref_dotnet].
* Lue osoitteessa [https://www.twilio.com/docs/security]Twilio suojaus-ohjeet[twilio_docs_security].
* Saat lisätietoja Twilio [https://www.twilio.com/docs][twilio_docs].

##<a name="seealso"></a>Katso myös
* [Opi käyttämään Twilio ääni-ja Azure SMS mahdollisuudet](twilio-dotnet-how-to-use-for-voice-sms.md)

[twilio_pricing]: http://www.twilio.com/pricing
[try_twilio]: http://www.twilio.com/try-twilio
[twilio_api]: http://www.twilio.com/voice/api
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#

[twilio_dotnet_basic_form]: ./media/partner-twilio-cloud-services-dotnet-phone-call-web-role/WA_twilio_dotnet_basic_form.png
[twilio_dotnet_basic_form_output]: ./media/partner-twilio-cloud-services-dotnet-phone-call-web-role/WA_twilio_dotnet_basic_form_output.png

[twiml]: http://www.twilio.com/docs/api/twiml



[howto_twilio_voice_sms_dotnet]: /develop/net/how-to-guides/twilio/

[howto_blob_storage_dotnet]: https://www.windowsazure.com/develop/net/how-to-guides/blob-storage/

[howto_sql_azure_dotnet]: https://www.windowsazure.com/develop/net/how-to-guides/sql-database/


[twilio_docs_security]: http://www.twilio.com/docs/security
[twilio_docs]: http://www.twilio.com/docs
[twilio_say]: http://www.twilio.com/docs/api/twiml/say


[azure_runtime_ref_dotnet]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.serviceruntime.aspx

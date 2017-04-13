<properties 
    pageTitle="Voit soittaa puhelun Twilio (Java) | Microsoft Azure" 
    description="Opettele puhelun soittaminen käyttämällä Twilio Azure Java-ohjelman web-sivulta." 
    services="" 
    documentationCenter="java" 
    authors="devinrader" 
    manager="twilio" 
    editor="mollybos"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="11/25/2014" 
    ms.author="microsofthelp@twilio.com"/>

# <a name="how-to-make-a-phone-call-using-twilio-in-a-java-application-on-azure"></a>Miten käyttämällä Twilio Java-sovelluksen Azure puhelun soittaminen 

Seuraavassa esimerkissä kerrotaan, miten voit käyttää Twilio soittaminen verkkosivulta ylläpidettävä Azure. Tuloksena oleva sovellus kysyy käyttäjältä puhelun arvolla, koodin seuraavassa kuvassa esitetyllä tavalla.

![Azure puhelun lomakkeen Twilio ja Java käyttäminen][twilio_java]

Sinun on käytettävä koodi tämän ohjeaiheen seuraavasti:

1. Hankkia Twilio tili ja todennusta tunnuksen. Aloita Twilio arvioi hinnat osoitteessa [http://www.twilio.com/pricing][twilio_pricing]. Voit rekisteröidä osoitteessa [https://www.twilio.com/try-twilio][try_twilio]. Lisätietoja Twilio myöntämä Ohjelmointirajapinnan on artikkelissa [http://www.twilio.com/api][twilio_api].
2. Hanki Twilio PURKKIIN. Milloin [https://github.com/twilio/twilio-java][twilio_java_github], voit ladata GitHub lähteiden ja luoda omia PURKKI tai ladata valmiita PURKKI (kanssa tai ilman riippuvuudet).
Tämän artikkelin koodi on kirjoitettu käyttämällä valmiita TwilioJava-3.3.8-ja-riippuvuudet PURKKIIN.
3. Lisää PURKKIIN Java-muodosta polku.
4. Jos käytät Pimennys Java-sovelluksen luominen, Sisällytä Twilio JAR Pimennys 's käyttöönoton kokoonpanon ominaisuudella sovelluksen käyttöönotto tiedoston (SODAN). Jos et käytä Pimennys Java-sovelluksen luominen, varmista Twilio JAR on sisällyttävä sama Azure rooli Java-sovellus ja sovelluksesi luokkaa polkuun.
5. Cacerts keystore sisältää Equifax suojatun varmenteen myöntäjä varmenteen MD5 tunnisteen 67:CB:9 D varmistamiseksi: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 (järjestysluvun 35:DE:F4:CF ja SHA1 tunnistetieto on D2:32:09:AD:23:D3:14:23:21:74:E4:0 D: 7F:9 D: 62:13:97:86:63:3A). Tämä on varmenteen myöntäjä (CA) varmennetta [https://api.twilio.com] [ twilio_api_service] palvelu, jota kutsutaan, kun käytät Twilio API. Lisätietoja CA-varmenteen lisääminen oman JDK cacert kaupan on [Java CA varmenteen tallentaa varmenteen lisääminen][add_ca_cert].

Lisäksi tietojen [luomista varten Pimennys Azure-työkalujen Hei maailma sovelluksen Using]tuntemusta[azure_java_eclipse_hello_world], tai muita tapoja isännöimiseen Java-sovellusten Azure-tietokannassa, jos et käytä Pimennys, jossa on erittäin suositeltavaa.

## <a name="create-a-web-form-for-making-a-call"></a>Puhelun soittaminen web lomakkeen luominen

Seuraava koodi esitetään, kuinka voit noutaa puhelun soittaminen käyttäjätietoja web-lomakkeen luominen. Tässä esimerkissä uuden dynaaminen WWW-projekti, nimeltä **TwilioCloud**on luotu ja **callform.jsp** on lisätty JSP-tiedostona.

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
        pageEncoding="ISO-8859-1" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Automated call form</title>
    </head>
    <body>
     <p>Fill in all fields and click <b>Make this call</b>.</p>
     <br/>
      <form action="makecall.jsp" method="post">
       <table>
         <tr>
           <td>To:</td>
           <td><input type="text" size=50 name="callTo" value="" />
           </td>
         </tr>
         <tr>
           <td>From:</td>
           <td><input type="text" size=50 name="callFrom" value="" />
           </td>
         </tr>
         <tr>
           <td>Call message:</td>
           <td><input type="text" size=400 name="callText" value="Hello. This is the call text. Good bye." />
           </td>
         </tr>
         <tr>
           <td colspan=2><input type="submit" value="Make this call" />
           </td>
         </tr>
       </table>
     </form>
     <br/>
    </body>
    </html>

## <a name="create-the-code-to-make-the-call"></a>Soittaa koodin luominen
Seuraava koodi, jota kutsutaan, kun käyttäjä on valmis callform.jsp näyttämät lomakkeen, Luo puhelun viestin ja luo puhelun. Tässä esimerkissä JSP-tiedosto nimeltä **makecall.jsp** ja on lisätty **TwilioCloud** projektin. (Käytä Twilio tili ja todennusta tunnussanoma **accountSID** ja **authToken** alla olevassa koodissa paikkamerkin arvojen sijaan).

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    import="java.util.*"
    import="com.twilio.*"
    import="com.twilio.sdk.*"
    import="com.twilio.sdk.resource.factory.*"
    import="com.twilio.sdk.resource.instance.*"
    pageEncoding="ISO-8859-1" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Call processing happens here</title>
    </head>
    <body>
        <b>This is my make call page.</b><p/>
     <%
    try 
    {
         // Use your account SID and authentication token instead
         // of the placeholders shown here.
         String accountSID = "your_twilio_account";
         String authToken = "your_twilio_authentication_token";
     
         // Instantiate an instance of the Twilio client.     
         TwilioRestClient client;
         client = new TwilioRestClient(accountSID, authToken);

         // Retrieve the account, used later to retrieve the CallFactory.
         Account account = client.getAccount();

         // Display the client endpoint. 
         out.println("<p>Using Twilio endpoint " + client.getEndpoint() + ".</p>");
     
         // Display the API version.
         String APIVERSION = TwilioRestClient.DEFAULT_VERSION;
         out.println("<p>Twilio client API version is " + APIVERSION + ".</p>");
    
         // Retrieve the values entered by the user.
         String callTo = request.getParameter("callTo");  
         // The Outgoing Caller ID, used for the From parameter,
         // must have previously been verified with Twilio.
         String callFrom = request.getParameter("callFrom");
         String userText = request.getParameter("callText");
     
         // Replace spaces in the user's text with '%20', 
         // to make the text suitable for a URL.
         userText = userText.replace(" ", "%20");
     
         // Create a URL using the Twilio message and the user-entered text.
         String Url="http://twimlets.com/message";
         Url = Url + "?Message%5B0%5D=" + userText;
     
         // Display the message URL.
         out.println("<p>");
         out.println("The URL is " + Url);
         out.println("</p>");
    
         // Place the call From, To and URL values into a hash map. 
         HashMap<String, String> params = new HashMap<String, String>();
         params.put("From", callFrom);
         params.put("To", callTo);
         params.put("Url", Url);
     
         CallFactory callFactory = account.getCallFactory();
         Call call = callFactory.create(params);
         out.println("<p>Call status: " + call.getStatus()  + "</p>"); 
    } 
    catch (TwilioRestException e) 
    {
        out.println("<p>TwilioRestException encountered: " + e.getMessage() + "</p>");
        out.println("<p>StackTrace: " + e.getStackTrace().toString() + "</p>");
    }
    catch (Exception e) 
    {
        out.println("<p>Exception encountered: " + e.getMessage() + "");
        out.println("<p>StackTrace: " + e.getStackTrace().toString() + "</p>");
    }
    %>
    </body>
    </html>

Lisäksi soittaa, makecall.jsp näyttää Twilio päätepisteen, API-versio ja puhelun tila. Koodin seuraavassa kuvassa on esimerkki:

![Azure puhelun vastauksen Twilio ja Java käyttäminen][twilio_java_response]

## <a name="run-the-application"></a>Suorita sovellus
Seuraavassa on korkean tason vaihe vaiheelta, miten suorittamaan sovelluksesi; tiedot-varten seuraavasti tukikäytännöistä [luomista varten Pimennys Azure-työkalujen Hei maailma sovelluksen Using][azure_java_eclipse_hello_world].

1. Vie TwilioCloud SODAN Azure **approot** -kansioon. 
2. Muokkaa **startup.cmd** purkaminen TwilioCloud SODAN.
3. Käännä sovelluksesi Laske emulaattori varten.
4. Käynnistä käyttöönoton suorittaminen emulaattori.
5. Avaa selain ja suorita **http://localhost:8080/TwilioCloud/callform.jsp**.
6. Kirjoita arvot muodossa, valitse **tämän puhelun**ja tarkastella makecall.jsp tuloksia.

Kun haluat ottaa käyttöön Azure-käyttöönoton pilvipalveluun + Käännä uudelleen käyttöön Azure ja suorita http://*your_hosted_name*.cloudapp.net/TwilioCloud/callform.jsp selaimessa (korvaa oman *your_hosted_name*arvo).

## <a name="next-steps"></a>Seuraavat vaiheet
Tämä koodi määritettyä näyttämään Twilio Java-käyttäminen Azure perustoiminnot. Ennen kuin otat käyttöön Azure tuotannon, haluat ehkä lisätä Lisää Virheenkäsittely tai muita ominaisuuksia. Esimerkki:

* Onnistunut Käytä sijaan verkkolomakkeen tallentamiseen puhelinnumerot ja soita tekstin tallennustilan Azure-BLOB- tai SQL-tietokanta. Saat lisätietoja Java-tallennustilan Azure-BLOB- [käyttämisestä Java-Blob-Tallennuspalvelu][howto_blob_storage_java]. Lisätietoja Java SQL-tietokannan käyttämisestä on artikkelissa [SQL-tietokannan käyttäminen Java][howto_sql_azure_java].
* Voit käyttää **RoleEnvironment.getConfigurationSettings** noutaa Twilio Tilitunnus ja todennus tunnuksen koko käyttöönotto määritysten asetukset-sijaan Kova-koodaus makecall.jsp arvot. Tietoja **RoleEnvironment** luokan artikkelissa [Azure palvelun suorituksenaikainen kirjastossa JSP käyttämällä] [ azure_runtime_jsp] ja Azure-palvelun suorituksenaikainen paketin documentation [http://dl.windowsazure.com/javadoc][azure_javadoc].
* Määrittää makecall.jsp koodi antamalla Twilio URL [http://twimlets.com/message][twimlet_message_url], muuttujan **URL-osoite** . Tätä URL-osoite sisältää Twilio Markup Language (TwiML)-vastauksen, joka ilmoittaa Twilio puhelun toimintatapaa. Esimerkiksi TwiML, joka palautetaan, voi olla ** &lt;sanoa&gt; ** toiminnon, joka johtaa tekstin lausumisen soitettaessa vastaanottajalle. Antamalla Twilio URL-Osoitteen sijaan voit luoda oman palvelun Twilio's kokouspyyntöön; Katso lisätietoja, [miten voit käyttää Twilio ääni-ja Java SMS-ominaisuuksien][howto_twilio_voice_sms_java]. Lisätietoja TwiML tukikäytännöistä [http://www.twilio.com/docs/api/twiml][twiml], ja haluat lisätietoja ** &lt;sanoa&gt; ** ja muut Twilio verbien tukikäytännöistä [http://www.twilio.com/docs/api/twiml/say][twilio_say].
* Lue osoitteessa [https://www.twilio.com/docs/security]Twilio suojaus-ohjeet[twilio_docs_security].

Saat lisätietoja Twilio [https://www.twilio.com/docs][twilio_docs].

## <a name="see-also"></a>Katso myös
* [Opi käyttämään Twilio ääni-ja SMS Java-ominaisuudet][howto_twilio_voice_sms_java]
* [Java CA Certificate Store sertifikaatin lisääminen][add_ca_cert]

[twilio_pricing]: http://www.twilio.com/pricing
[try_twilio]: http://www.twilio.com/try-twilio
[twilio_api]: http://www.twilio.com/api
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[twilio_java_github]: http://github.com/twilio/twilio-java
[twimlet_message_url]: http://twimlets.com/message
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api_service]: http://api.twilio.com
[add_ca_cert]: java-add-certificate-ca-store.md
[azure_java_eclipse_hello_world]: http://msdn.microsoft.com/library/windowsazure/hh690944.aspx
[howto_twilio_voice_sms_java]: partner-twilio-java-how-to-use-voice-sms.md
[howto_blob_storage_java]: http://www.windowsazure.com/develop/java/how-to-guides/blob-storage/
[howto_sql_azure_java]: http://msdn.microsoft.com/library/windowsazure/hh749029.aspx
[azure_runtime_jsp]: http://msdn.microsoft.com/library/windowsazure/hh690948.aspx
[azure_javadoc]: http://dl.windowsazure.com/javadoc
[twilio_docs_security]: http://www.twilio.com/docs/security
[twilio_docs]: http://www.twilio.com/docs
[twilio_say]: http://www.twilio.com/docs/api/twiml/say
[twilio_java]: ./media/partner-twilio-java-phone-call-example/WA_TwilioJavaCallForm.jpg
[twilio_java_response]: ./media/partner-twilio-java-phone-call-example/WA_TwilioJavaMakeCall.jpg

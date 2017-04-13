<properties 
    pageTitle="Opi käyttämään Twilio ääni-ja SMS (Java) | Microsoft Azure" 
    description="Opettele puhelun soittaminen ja lähettää Azure tekstiviesti Twilio API-palvelussa. Kirjoitettu Java MALLIKOODEJA." 
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

# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-java"></a>Opi käyttämään Twilio ääni-ja SMS Java-ominaisuudet

Tässä oppaassa kerrotaan, miten tekemässä Azure ohjelmoinnin perustehtäviä Twilio API-palvelussa. Tilanteita, joissa kattaa ovat soittaa puheluita ja lyhyen viestin Service (SMS) viestin lähettämistä. Lisätietoja Twilio ja ääni- ja SMS käyttäminen sovellukset-kohdassa [Seuraavat vaiheet](#NextSteps) .

## <a id="WhatIs"></a>Mikä on Twilio?
Twilio on Puhelin-WWW-palvelun API, voit käyttää aiemmin web kielet ja taitojen luonnissa ääni- ja SMS sovellukset. Twilio on kolmannen osapuolen palvelu (ei Azure ominaisuus ja ole Microsoft-tuotteen).

**Twilio äänen** avulla sovellukset voivat soittaa ja vastaanottaa puheluita. **Twilio SMS** avulla sovellukset voivat soittaa ja vastaanottaa tekstiviestejä. **Twilio asiakkaan** avulla sovellustesi voice tietoliikenne käyttämällä olemassa olevan Internet-yhteydet, mukaan lukien mobile yhteyksiä.

## <a id="Pricing"></a>Twilio hinnat ja Erikoistarjoukset
Tietoja Twilio hinnat on saatavilla osoitteessa [Twilio hinnat] [twilio_pricing]. Azure asiakkaiden käyttämien [erikoistarjous][special_offer]: ilmainen tilaus odottaa luottotietojen 1000 tekstejä tai 1000 saapuva minuuttia. Rekisteröityminen tarjoukseen tai saada lisätietoja, käy [http://ahoy.twilio.com/azure][special_offer].  

## <a id="Concepts"></a>Käsitteitä
Twilio-Ohjelmointirajapinnan on RESTful-Ohjelmointirajapinta, joka tarjoaa ääni- ja SMS toimintoja sovellukset. Asiakkaan kirjastot ovat käytettävissä useilla kielillä luettelo on artikkelissa [Twilio API kirjastojen] [twilio_libraries].

Twilio Ohjelmointirajapinnan keskeisiä ominaisuuksia ovat Twilio verbien ja Twilio Markup Language (TwiML).

### <a id="Verbs"></a>Twilio käyttö
Tekee Ohjelmointirajapinnan käyttäminen Twilio verbien; esimerkiksi ** &lt;sanoa&gt; ** verbin ohjaa Twilio aikana audibly puhelua viestiin. 

Seuraavassa on luettelo Twilio verbien.

* ** &lt;Soita&gt;**: soittajan muodostaa yhteyden toiseen puhelimeen.
* ** &lt;Kerätä&gt;**: kerää syötetty puhelimen näppäimistön numerona.
* ** &lt;Katkaisu&gt;**: lopettaa puhelun.
* ** &lt;Toistaa&gt;**: äänitiedoston toiston.
* ** &lt;Keskeytä&gt;**: odottaa äänettömästi määritetty sekuntimäärä.
* ** &lt;Tietueen&gt;**: soittajan voice tallentaa ja palauttaa tallenteen sisältävän tiedoston URL-osoite.
* ** &lt;Uudelleenohjata&gt;**: siirtää puhelun tai SMS hallinnan TwiML eri URL-osoitteessa.
* ** &lt;Hylkää&gt;**: Hylkää puhelu Twilio numeroon ilman Laskutus voit
* ** &lt;Sanoa&gt;**: muuntaa tekstin puheeksi, jotka tehdään puhelua.
* ** &lt;Sms&gt;**: lähettää tekstiviestin.

### <a id="TwiML"></a>TwiML
TwiML on joukko XML-pohjainen ohjeita, jotka ilmoittavat käsitteleminen puhelun Twilio tai SMS Twilio verbien perusteella.

Esimerkiksi seuraavat TwiML muuntaa tekstin **Hei maailma** puhe.

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World</Say>
    </Response>

Kun sovellus soittaa Twilio Ohjelmointirajapinnan, yksi API-parametrit on URL-Osoitetta, joka palauttaa TwiML vastausta. Kehittäminen tarkoituksiin voit antamalla Twilio URL-osoitteet antamaan sovelluksien käyttämien TwiML vastaukset. Voit myös isännöidä tuottamaan TwiML vastaukset oman URL-osoitteet ja toinen vaihtoehto on käyttää **TwiMLResponse** objekti.

Saat lisätietoja Twilio verbien niiden määritteitä ja TwiML [TwiML] [twiml]. Saat lisätietoja Twilio Ohjelmointirajapinnan [Twilio API] [twilio_api].

## <a id="CreateAccount"></a>Twilio-tilin luominen
Kun olet valmis, saat Twilio-tiliä, Rekisteröidy, [Yritä Twilio] [try_twilio]. Voit aloittaa maksuttoman tilillä, ja tilin päivityksen myöhemmin.

Kun kirjaudut Twilio tilin, saat tilin tunnus ja todennus-tunnuksen. Molemmat tarvitaan Twilio API soittamiseen. Voit estää näin luvattoman pääsyn tilisi, suojata todennus-tunnuksen. Tilitunnus ja todennusta tunnuksen ovat tarkasteltava [Twilio tilisivun] [twilio_account]-kentät nimetty **tilin SUOJAUSTUNNUS** ja **AUTH tunnuksen**, vastaavassa järjestyksessä.

## <a id="create_app"></a>Java-sovelluksen luominen
1. Hanki Twilio JAR ja lisää se Java-muodosta polku ja SODAN käyttöönotto-kokoonpanoa. Milloin [https://github.com/twilio/twilio-java][twilio_java], voit ladata GitHub lähteiden ja luoda omia PURKKI tai ladata valmiita PURKKI (kanssa tai ilman riippuvuudet).
2. Varmistaa, että JDK **cacerts** keystore sisältää MD5 tunnisteen 67:CB:9 D Equifax suojatun varmenteen myöntäjä sertifikaattiin: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 (järjestysluvun 35:DE:F4:CF ja SHA1 tunnistetieto on D2:32:09:AD:23:D3:14:23:21:74:E4:0 D: 7F:9 D: 62:13:97:86:63:3A). Tämä on varmenteen myöntäjä (CA) varmennetta [https://api.twilio.com] [ twilio_api_service] palvelu, jota kutsutaan, kun käytät Twilio API. Lisätietoja varmistaa, että oman JDK **cacerts** keystore sisältää oikeat CA-sertifikaatin on artikkelissa [lisääminen sertifikaattia Java CA varmenteen säilöön][add_ca_cert].

Yksityiskohtaiset ohjeet Twilio asiakas-kirjaston käytön Java on osoitteessa [poistamisesta puhelu käyttämällä Twilio Java-sovelluksen Azure][howto_phonecall_java].

## <a id="configure_app"></a>Määrittää sovelluksesi käyttämään Twilio kirjastot
Oman koodiin voit lisätä **Tuo** lauseet lähdetiedostot Twilio pakettien tai luokat, jota haluat käyttää sovelluksen yläreunassa. 

For Javan lähdetiedostot:

    import com.twilio.*;
    import com.twilio.sdk.*;
    import com.twilio.sdk.resource.factory.*;
    import com.twilio.sdk.resource.instance.*;

Java Server Page (JSP) lähdetiedostot:

    import="com.twilio.*"
    import="com.twilio.sdk.*"
    import="com.twilio.sdk.resource.factory.*"
    import="com.twilio.sdk.resource.instance.*"
**Tuo** -lauseet saattavat olla erilaiset sen mukaan, mitä Twilio pakettien tai luokat, jota haluat käyttää.

## <a id="howto_make_call"></a>Toimintaohje: lähtevän soittaminen
Seuraavassa esitetään, kuinka voit lähtevän soittaminen **CallFactory** -luokan avulla. Tämä koodi käytetään myös antamalla Twilio sivuston Twilio Markup Language (TwiML)-vastauksen palauttamiseen. Korvaa arvot ** **- ja** ** puhelinnumeroita ja poista Tarkista puhelinnumero **-** tilisi Twilio ennen koodin suorittamista.

    // Use your account SID and authentication token instead
    // of the placeholders shown here.
    String accountSID = "your_twilio_account";
    String authToken = "your_twilio_authentication_token";

    // Create an instance of the Twilio client.
    TwilioRestClient client;
    client = new TwilioRestClient(accountSID, authToken);

    // Retrieve the account, used later to create an instance of the CallFactory.
    Account account = client.getAccount();

    // Use the Twilio-provided site for the TwiML response.
    String Url="http://twimlets.com/message";
    Url = Url + "?Message%5B0%5D=Hello%20World";

    // Place the call From, To and URL values into a hash map. 
    HashMap<String, String> params = new HashMap<String, String>();
    params.put("From", "NNNNNNNNNN"); // Use your own value for the second parameter.
    params.put("To", "NNNNNNNNNN");   // Use your own value for the second parameter.
    params.put("Url", Url);

    // Create an instance of the CallFactory class.
    CallFactory callFactory = account.getCallFactory();

    // Make the call.
    Call call = callFactory.create(params);

Saat lisätietoja välitetään **CallFactory.create** menetelmä parametrit [http://www.twilio.com/docs/api/rest/making-calls][twilio_rest_making_calls].

Kuten edellä, koodi käytetään antamalla Twilio sivuston TwiML vastauksen palauttamiseen. Voit käyttää sen sijaan oman sivuston antamaan TwiML; vastaus Katso lisätietoja, [miten voit antaa TwiML vastaukset Java-sovelluksen Azure](#howto_provide_twiml_responses).

## <a id="howto_send_sms"></a>Toimintaohje: lähettää tekstiviestin
Seuraavassa esitetään, kuinka voit lähettää tekstiviestin **SmsFactory** -luokan avulla. **Valitse** numero, **4155992671**tarjoaa Twilio kokeiluversion tilien tekstiviestien lähettäminen. **Koskevat** on vahvistettava ennen koodin suorittamista Twilio tilin.

    // Use your account SID and authentication token instead
    // of the placeholders shown here.
    String accountSID = "your_twilio_account";
    String authToken = "your_twilio_authentication_token";

    // Create an instance of the Twilio client.
    TwilioRestClient client;
    client = new TwilioRestClient(accountSID, authToken);

    // Retrieve the account, used later to create an instance of the SmsFactory.
    Account account = client.getAccount();

    // Send an SMS message.
    MessageFactory messageFactory = account.getMessageFactory();
    
    List<NameValuePair> params = new ArrayList<NameValuePair>();
    params.add(new BasicNameValuePair("To", "+14159352345")); // Replace with a valid phone number for your account.
    params.add(new BasicNameValuePair("From", "+14158141829")); // Replace with a valid phone number for your account.
    params.add(new BasicNameValuePair("Body", "Where's Wallace?"));
    
    Message sms = messageFactory.create(params);
        
Saat lisätietoja välitetään **SmsFactory.create** menetelmä parametrit [http://www.twilio.com/docs/api/rest/sending-sms][twilio_rest_sending_sms].

## <a id="howto_provide_twiml_responses"></a>Toimintaohje: Anna TwiML vastaukset omasta sivustosta
Kun sovelluksen käynnistää Twilio API-puhelun, esimerkiksi **CallFactory.create** -menetelmällä Twilio lähetetään pyyntö URL-osoitteeseen, joka palauttaa TwiML vastausta odotetaan. Edellä olevassa esimerkissä käytetään antamalla Twilio URL-Osoitteen [http://twimlets.com/message][twimlet_message_url]. (Kun TwiML on suunniteltu verkkopalvelut, voit tarkastella TwiML selaimessa. Valitse esimerkiksi [http://twimlets.com/message] [ twimlet_message_url] Nähdäksesi tyhjän ** &lt;vastauksen&gt; ** elementin; Valitse toinen esimerkki [http://twimlets.com/message?Message%5B0%5D=Hello%20World] [ twimlet_message_url_hello_world] Nähdäksesi ** &lt;vastauksen&gt; ** elementti, joka sisältää ** &lt;sanoa&gt; ** elementti.)

Sen sijaan, että käyttäisit antamalla Twilio URL-osoite, voit luoda oman URL-osoite-sivusto, joka palauttaa HTTP vastaukset. Voit luoda sivuston kieli, joka palauttaa HTTP vastaukset; Tässä artikkelissa oletetaan, isännöinnin JSP sivun URL-osoite.

Seuraavan sivun JSP hakutulosten TwiML vastauksen, jossa lukee **Hei maailma** puheluun.

    <%@ page contentType="text/xml" %>
    <Response> 
        <Say>Hello World</Say>
    </Response>

Seuraavan sivun JSP hakutulosten TwiML vastauksen, joka lukee tekstiä, on useita keskeyttää ja tiedot näkökulma Twilio API-versio ja Azure roolinimi.


    <%@ page contentType="text/xml" %>
    <Response> 
        <Say>Hello from Azure</Say>
        <Pause></Pause>
        <Say>The Twilio API version is <%= request.getParameter("ApiVersion") %>.</Say>
        <Say>The Azure role name is <%= System.getenv("RoleName") %>.</Say>
        <Pause></Pause>
        <Say>Good bye.</Say>
    </Response>

**ApiVersion** -parametri on käytettävissä Twilio voice pyynnöt (ei SMS pyyntöjen). Lisätietoja Twilio ääni-ja SMS pyynnöt käytettävissä pyynnön-parametrit on <https://www.twilio.com/docs/api/twiml/twilio_request> ja <https://www.twilio.com/docs/api/twiml/sms/twilio_request>tarpeen mukaan. **RoleName** ympäristömuuttuja on saatavana osana Azure käyttöönotto. (Jos haluat lisätä mukautetun ympäristömuuttujat, jotta niitä voi poimia **System.getenv**, on artikkelissa ympäristön muuttujat [Muut roolin asetukset][misc_role_config_settings].)

Kun JSP sivusi määritetty tarjota TwiML vastaukset, Määritä JSP sivun URL-Osoitteen URL-Osoitteen välitetty **CallFactory.create** -menetelmää. Esimerkiksi jos nimeltä Web-sovelluksen käyttöön Azure MyTwiML isännöidään-palveluun, ja JSP-sivun nimi on mytwiml.jsp, URL-Osoitteen voi välittää **CallFactory.create** seuraavassa esitetyllä tavalla:

    // Place the call From, To and URL values into a hash map. 
    HashMap<String, String> params = new HashMap<String, String>();
    params.put("From", "NNNNNNNNNN");
    params.put("To", "NNNNNNNNNN");
    params.put("Url", "http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.jsp");

    CallFactory callFactory = account.getCallFactory();
    Call call = callFactory.create(params);

Toinen tapa antavat TwiML on **TwiMLResponse** -luokka, joka on käytettävissä **com.twilio.sdk.verbs** -paketti.

Lisätietoja Twilio käyttäminen Azure Java kanssa, katso, [miten voit tehdä puhelun soittaminen käyttämällä Twilio Java-sovelluksen Azure][howto_phonecall_java].

## <a id="AdditionalServices"></a>Toimintaohje: Käytä muita Twilio palvelut
Lisäksi tässä esitetty esimerkkejä Twilio on verkkopohjainen API, joiden avulla voit hyödyntää Twilio lisätoimintoja Azure-sovelluksesta. Katso tarkat tiedot [Twilio API-ohjeissa] [twilio_api_documentation].

## <a id="NextSteps"></a>Seuraavat vaiheet
Nyt kun olet oppinut perustiedot Twilio-palvelun, näistä linkeistä saat lisätietoja:

* [Twilio suojausta koskevia ohjeita] [twilio_security_guidelines]
* [Twilio ohjeet ja esimerkkikoodi] [twilio_howtos]
* [Twilio pikaopas opetusohjelmat][twilio_quickstarts] 
* [Valitse GitHub Twilio] [twilio_on_github]
* [Jutella Twilio tuki] [twilio_support]

[twilio_java]: https://github.com/twilio/twilio-java
[twilio_api_service]: https://api.twilio.com
[add_ca_cert]: java-add-certificate-ca-store.md
[howto_phonecall_java]: partner-twilio-java-phone-call-example.md
[misc_role_config_settings]: http://msdn.microsoft.com/library/windowsazure/hh690945.aspx
[twimlet_message_url]: http://twimlets.com/message
[twimlet_message_url_hello_world]: http://twimlets.com/message?Message%5B0%5D=Hello%20World
[twilio_rest_making_calls]: http://www.twilio.com/docs/api/rest/making-calls
[twilio_rest_sending_sms]: http://www.twilio.com/docs/api/rest/sending-sms
[twilio_pricing]: http://www.twilio.com/pricing
[special_offer]: http://ahoy.twilio.com/azure
[twilio_libraries]: https://www.twilio.com/docs/libraries
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api]: http://www.twilio.com/api
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_account]:  https://www.twilio.com/user/account
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[twilio_api_documentation]: http://www.twilio.com/api
[twilio_security_guidelines]: http://www.twilio.com/docs/security
[twilio_howtos]: http://www.twilio.com/docs/howto
[twilio_on_github]: https://github.com/twilio
[twilio_support]: http://www.twilio.com/help/contact
[twilio_quickstarts]: http://www.twilio.com/docs/quickstart

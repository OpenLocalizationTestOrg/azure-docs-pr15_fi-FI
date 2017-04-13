<properties
    pageTitle="Opi käyttämään Twilio ääni-ja SMS (PHP) | Microsoft Azure"
    description="Opettele puhelun soittaminen ja lähettää Azure tekstiviesti Twilio API-palvelussa. Kirjoitettu PHP MALLIKOODEJA."
    services=""
    documentationCenter="python"
    authors="devinrader"
    manager="twilio"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="python" 
    ms.topic="article"
    ms.date="02/19/2015"
    ms.author="MicrosoftHelp@twilio.com"/>





# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-php"></a>Opi käyttämään Twilio ääni-ja SMS-ominaisuuksien PHP
Tässä oppaassa kerrotaan, miten tekemässä Azure ohjelmoinnin perustehtäviä Twilio API-palvelussa. Tilanteita, joissa kattaa ovat soittaa puheluita ja lyhyen viestin Service (SMS) viestin lähettämistä. Lisätietoja Twilio ja ääni- ja SMS käyttäminen sovellukset-kohdassa [Seuraavat vaiheet](#NextSteps) .

## <a id="WhatIs"></a>Mikä on Twilio?
Twilio on ajan business viestintä, joten upottamisesta ääni-, VoIP- ja messaging sovelluksiin kehittäjät tulevaisuudessa. Ne virtualisoi kaikki tarvittava pilvipohjainen, yleinen-ympäristössä, paljastaa kautta Twilio communications API-ympäristö infrastruktuuri. Sovellukset on helppo luoda ja skaalattava. Nauti joustavasti maksu-muodossa-voit siirtyä hinnat ja hyötyä cloud luotettavuutta.

**Twilio äänen** avulla sovellukset voivat soittaa ja vastaanottaa puheluita. **Twilio SMS** avulla voit lähettää ja vastaanottaa tekstiviestejä sovelluksen. **Twilio asiakkaan** avulla voit tehdä VoIP-puheluihin puhelimella, tabletilla tai selaimessa, ja se tukee WebRTC.

## <a id="Pricing"></a>Twilio hinnat ja Erikoistarjoukset

Azure asiakkaiden käyttämien [erikoistarjous] 10 Twilio luottotietojen, kun päivität Twilio-tilillesi. Tämä Twilio luottotietojen voi suojata Twilio käyttö (10 euroa luottotietojen enintään 1 000 tekstiviestien lähettäminen tai vastaanottaminen enintään 1 000 saapuvia ääni minuutin puhelin numero ja viestin tai puhelun kohteen sijainnin mukaan). Tämän Twilio luottotietojen lunastaminen ja aloita osoitteessa: [ahoy.twilio.com/azure].

Twilio on tukiyhteyksiä palvelu. Ei ole määritetään maksut ja tilin sulkeminen milloin tahansa. Löydät lisätietoja [Twilio hinnat] [twilio_pricing].

## <a id="Concepts"></a>Käsitteitä
Twilio-Ohjelmointirajapinnan on RESTful-Ohjelmointirajapinta, joka tarjoaa ääni- ja SMS toimintoja sovellukset. Asiakkaan kirjastot ovat käytettävissä useilla kielillä luettelo on artikkelissa [Twilio API kirjastojen] [twilio_libraries].

Twilio Ohjelmointirajapinnan keskeisiä ominaisuuksia ovat Twilio verbien ja Twilio Markup Language (TwiML).

### <a id="Verbs"></a>Twilio käyttö
Tekee Ohjelmointirajapinnan käyttäminen Twilio verbien; esimerkiksi ** &lt;sanoa&gt; ** verbin ohjaa Twilio aikana audibly puhelua viestiin.

Seuraavassa on luettelo Twilio verbien. Tietoja verbien ja ominaisuuksia kautta [Twilio Markup Language dokumentaatio] [http://www.twilio.com/docs/api/twiml].

* ** &lt;Soita&gt;**: soittajan muodostaa yhteyden toiseen puhelimeen.
* ** &lt;Kerätä&gt;**: kerää syötetty puhelimen näppäimistön numerona.
* ** &lt;Katkaisu&gt;**: lopettaa puhelun.
* ** &lt;Toistaa&gt;**: äänitiedoston toiston.
* ** &lt;Keskeytä&gt;**: odottaa äänettömästi määritetty sekuntimäärä.
* ** &lt;Tietueen&gt;**: tietueiden soittajan ääni ja palauttaa tallenteen sisältävän tiedoston URL-osoite.
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
Kun saat Twilio-tiliä, Rekisteröidy, [Yritä Twilio] [try_twilio]. Voit aloittaa maksuttoman tilillä, ja tilin päivityksen myöhemmin.

Kun kirjaudut Twilio-tili, saat tilin tunnus ja todennus-tunnuksen. Molemmat tarvitaan Twilio API soittamiseen. Voit estää näin luvattoman pääsyn tilisi, suojata todennus-tunnuksen. Tilitunnus ja todennusta tunnuksen ovat tarkasteltava [Twilio tilisivun] [twilio_account]-kentät nimetty **tilin SUOJAUSTUNNUS** ja **AUTH tunnuksen**, vastaavassa järjestyksessä.

## <a id="create_app"></a>PHP-sovelluksen luominen
PHP-sovellus, joka käyttää Twilio-palvelua ja Azure käytössä on sama asia kuin minkä tahansa PHP sovelluksen, joka käyttää Twilio-palvelua. Twilio palvelut ovat REST-pohjaisia ja voidaan kutsua PHP usealla eri tavalla, tässä artikkelissa keskitytään Twilio palveluiden käyttäminen [PHP GitHub-kirjasto Twilio][twilio_php]. Katso lisätietoja käyttämisestä Twilio kirjaston PHP [http://readthedocs.org/docs/twilio-php/en/latest/index.html][twilio_lib_docs].

Yksityiskohtaisia ohjeita ja käyttöönottoon Twilio/PHP-sovelluksen Azure on osoitteessa [poistamisesta puhelun käyttämällä Twilio PHP-sovelluksen Azure][howto_phonecall_php].

## <a id="configure_app"></a>Määrittää sovelluksesi käyttämään Twilio kirjastot
Voit määrittää sovelluksen Twilio kirjaston käytettävät PHP kahdella tavalla:

1. Lataa Twilio kirjaston PHP-GitHub ([https://github.com/twilio/twilio-php][twilio_php]) ja lisää **Services** -hakemisto-sovellukseen.

    -TAI –

2. Asenna Twilio kirjaston PHP PÄÄRYNÄPUIDEN-pakettina. Se voidaan asentaa seuraavia komentoja:

        $ pear channel-discover twilio.github.com/pear
        $ pear install twilio/Services_Twilio

Kun olet asentanut Twilio kirjaston PHP PHP viittaamaan kirjaston tiedostojen yläreunassa lisätä **require_once** ilmoitus:

        require_once 'Services/Twilio.php';

Lisätietoja on artikkelissa [https://github.com/twilio/twilio-php/blob/master/README.md][twilio_github_readme].

## <a id="howto_make_call"></a>Toimintaohje: lähtevän soittaminen
Seuraavassa esitetään, kuinka voit lähtevän soittaminen **Services_Twilio** -luokan avulla. Tämä koodi käytetään myös antamalla Twilio sivuston Twilio Markup Language (TwiML)-vastauksen palauttamiseen. Korvaa arvot ** **- ja** ** puhelinnumeroita ja poista Tarkista puhelinnumero **-** tilisi Twilio ennen koodin suorittamista.

    // Include the Twilio PHP library.
    require_once 'Services/Twilio.php';

    // Library version.
    $version = "2010-04-01";

    // Set your account ID and authentication token.
    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";

    // The number of the phone initiating the the call.
    // (Must be previously validated with Twilio.)
    $from_number = "NNNNNNNNNNN";

    // The number of the phone receiving call.
    $to_number = "NNNNNNNNNNN";

    // Use the Twilio-provided site for the TwiML response.
    $url = "http://twimlets.com/message";

    // The phone message text.
    $message = "Hello world.";

    // Create the call client.
    $client = new Services_Twilio($sid, $token, $version);

    //Make the call.
    try
    {
        $call = $client->account->calls->create(
            $from_number,
            $to_number,
            $url.'?Message='.urlencode($message)
        );
    }
    catch (Exception $e)
    {
        echo 'Error: ' . $e->getMessage();
    }

Kuten edellä, koodi käytetään antamalla Twilio sivuston TwiML vastauksen palauttamiseen. Voit käyttää sen sijaan oman sivuston antamaan TwiML; vastaus Katso lisätietoja, [miten voit antaa TwiML vastaukset Your omasta sivustosta](#howto_provide_twiml_responses).


- **Huomautus**: SSL-varmenteen tarkistusvirheitä, kokeile [http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html][ssl_validation]


## <a id="howto_send_sms"></a>Toimintaohje: lähettää tekstiviestin
Seuraavassa esitetään, kuinka voit lähettää tekstiviestin **Services_Twilio** -luokan avulla. **Valitse** numero tarjoaa Twilio kokeiluversion tilien tekstiviestien lähettäminen. **Koskevat** on vahvistettava ennen koodin suorittamista Twilio tilin.

    // Include the Twilio PHP library.
    require_once 'Services/Twilio.php';

    // Library version.
    $version = "2010-04-01";

    // Set your account ID and authentication token.
    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";


    $from_number = "NNNNNNNNNNN"; // With trial account, texts can only be sent from your Twilio number.
    $to_number = "NNNNNNNNNNN";
    $message = "Hello world.";

    // Create the call client.
    $client = new Services_Twilio($sid, $token, $version);

    // Send the SMS message.
    try
    {
        $client->account->sms_messages->create($from_number, $to_number, $message);
    }
    catch (Exception $e)
    {
        echo 'Error: ' . $e->getMessage();
    }

## <a id="howto_provide_twiml_responses"></a>Toimintaohje: Anna TwiML vastaukset omasta sivustosta
Kun sovellus käynnistää Twilio API-puhelun, Twilio Lähetä pyyntö URL-osoitteeseen, joka palauttaa TwiML vastausta odotetaan. Edellä olevassa esimerkissä käytetään antamalla Twilio URL-Osoitteen [http://twimlets.com/message][twimlet_message_url]. (Kun TwiML on suunniteltu Twilio, voit tarkastella se selaimessa. Valitse esimerkiksi [http://twimlets.com/message] [ twimlet_message_url] Nähdäksesi tyhjän `<Response>` elementin; Valitse toinen esimerkki [http://twimlets.com/message?Message%5B0%5D=Hello%20World] [ twimlet_message_url_hello_world] Nähdäksesi `<Response>` elementti, joka sisältää `<Say>` elementti.)

Sen sijaan, että käyttäisit antamalla Twilio URL-osoite, voit luoda oman sivuston, joka palauttaa HTTP vastaukset. Voit luoda sivuston kieli, joka palauttaa XML-vastaukset; Tässä artikkelissa oletetaan, että käytät PHP luomiseen TwiML.

Seuraavan sivun PHP hakutulosten TwiML vastauksen, jossa lukee **Hei maailma** puheluun.

    <?php
        header("content-type: text/xml");
        echo "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n";
    ?>
    <Response>
        <Say>Hello world.</Say>
    </Response>

Kuten näet yllä olevassa esimerkissä, TwiML vastaus on yksinkertaisesti XML-tiedosto. PHP Twilio kirjasto sisältää luokat, jotka luovat TwiML puolestasi. Alla olevassa esimerkissä tuottaa vastaavat vastauksen yllä esitetyllä tavalla, mutta siinä käytetään **Services\_Twilio\_Twiml** PHP Twilio kirjasto luokan:

    require_once('Services/Twilio.php');

    $response = new Services_Twilio_Twiml();
    $response->say("Hello world.");
    print $response;

Saat lisätietoja TwiML [https://www.twilio.com/docs/api/twiml][twiml_reference].

Kun olet PHP sivusi määritetty tarjota TwiML vastaukset, käyttää PHP sivun URL-Osoitteen URL-Osoitteen välitetty `Services_Twilio->account->calls->create` menetelmää. Esimerkiksi jos sinulla on Web-sovelluksen nimeltä **MyTwiML** käyttöön Azure isännöidään-palvelua ja PHP-sivun nimi on **mytwiml.php**, URL-osoite välitetään **Services_Twilio -> tili -> puhelut -> Luo** seuraavan esimerkin mukaisesti:

    require_once 'Services/Twilio.php';

    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";
    $from_number = "NNNNNNNNNNN";
    $to_number = "NNNNNNNNNNN";
    $url = "http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.php";

    // The phone message text.
    $message = "Hello world.";

    $client = new Services_Twilio($sid, $token, "2010-04-01");

    try
    {
        $call = $client->account->calls->create(
            $from_number,
            $to_number,
            $url.'?Message='.urlencode($message)
        );
    }
    catch (Exception $e)
    {
        echo 'Error: ' . $e->getMessage();
    }

Lisätietoja Twilio käyttäminen Azure PHP kanssa, katso, [miten voit tehdä puhelun soittaminen käyttämällä Twilio PHP-sovelluksen Azure][howto_phonecall_php].

## <a id="AdditionalServices"></a>Toimintaohje: Käytä muita Twilio palvelut
Lisäksi tässä esitetty esimerkkejä Twilio on verkkopohjainen API, joiden avulla voit hyödyntää Twilio lisätoimintoja Azure-sovelluksesta. Katso tarkat tiedot [Twilio API-ohjeissa] [twilio_api_documentation].

## <a id="NextSteps"></a>Seuraavat vaiheet
Nyt olet oppinut perustiedot Twilio-palvelun, näistä linkeistä saat lisätietoja:

* [Twilio suojausta koskevia ohjeita] [twilio_security_guidelines]
* [Twilio ohjeet apuviivat ja esimerkkikoodi] [twilio_howtos]
* [Twilio pikaopas opetusohjelmat][twilio_quickstarts]
* [Valitse GitHub Twilio] [twilio_on_github]
* [Jutella Twilio tuki] [twilio_support]

[twilio_php]: https://github.com/twilio/twilio-php
[twilio_lib_docs]: http://readthedocs.org/docs/twilio-php/en/latest/index.html
[twilio_github_readme]: https://github.com/twilio/twilio-php/blob/master/README.md
[ssl_validation]: http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html

[howto_phonecall_php]: http://windowsazure.com/documentation/articles/partner-twilio-php-make-phone-call
[twimlet_message_url]: http://twimlets.com/message
[twimlet_message_url_hello_world]: http://twimlets.com/message?Message%5B0%5D=Hello%20World
[twiml_reference]: https://www.twilio.com/docs/api/twiml
[twilio_pricing]: http://www.twilio.com/pricing

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

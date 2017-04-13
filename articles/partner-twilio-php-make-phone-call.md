<properties
    pageTitle="Voit soittaa puhelun-Twilio (PHP) | Microsoft Azure"
    description="Opettele puhelun soittaminen ja lähettää Azure tekstiviesti Twilio API-palvelussa. Esimerkkejä, joiden on PHP sovelluksen."
    documentationCenter="php"
    services=""
    authors="devinrader"
    manager="twilio"
    editor="mollybos"/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="11/25/2014"
    ms.author="microsofthelp@twilio.com"/>

# <a name="how-to-make-a-phone-call-using-twilio-in-a-php-application-on-azure"></a>Miten Twilio käyttäminen PHP Azure-ohjelman puhelun soittaminen

Seuraavassa esimerkissä kerrotaan, miten voit käyttää Twilio soittaminen ylläpidettävä Azure PHP verkkosivulta. Tuloksena oleva sovellus kysyy käyttäjältä puhelun arvolla, koodin seuraavassa kuvassa esitetyllä tavalla.

![Azure puhelun lomakkeen Twilio ja PHP käyttäminen][twilio_php]

Sinun on käytettävä koodi tämän ohjeaiheen seuraavasti:

1. Hankkia Twilio tili ja todennusta tunnuksen. Aloita Twilio arvioi hinnat osoitteessa [http://www.twilio.com/pricing][twilio_pricing]. Voit rekisteröidä osoitteessa [https://www.twilio.com/try-twilio]kokeilutili[try_twilio]. Lisätietoja Twilio myöntämä Ohjelmointirajapinnan on artikkelissa [http://www.twilio.com/api][twilio_api].
2. Hanki [PHP Twilio kirjasto](https://github.com/twilio/twilio-php) tai asentaa sen PÄÄRYNÄPUIDEN pakettina. Lisätietoja on artikkelissa [Lueminut-tiedosto](https://github.com/twilio/twilio-php/blob/master/README.md).
3. Asenna Azure SDK PHP. Yleiskuvaus SDK ja sen asentamisesta on artikkelissa [Azure-SDK PHP määrittäminen][setup_php_sdk].

## <a name="create-a-web-form-for-making-a-call"></a>Puhelun soittaminen web lomakkeen luominen

HTML-koodi näkyy muodostaminen verkkosivuun (**callform.html**), joka noutaa puhelun soittaminen käyttäjätiedot:

    <html>
    <head>
        <title>Automated call form</title>
    </head>
    <body>
    <h1>Automated Call Form</h1>
    <p>Fill in all fields and click <b>Make this call</b>.</p>
    <form action="makecall.php" method="post">
    <table>
        <tr>
            <td>To:</td>
            <td><input type="text" size=50 name="callTo" value=""></td>
        </tr>
        <tr>
            <td>From:</td>
            <td><input type="text" size=50 name="callFrom" value=""></td>
        </tr>
        <tr>
            <td>Call message:</td>
            <td><input type="text" size=100 name="callText" value="Hello. This is the call text. Good bye." /></td>
        </tr>
        <tr>
            <td colspan=2><input type="submit" value="Make this call"></td>
        </tr>
    </table>
    </form>
    <br/>
    </body>
    </html>

## <a name="create-the-code-to-make-the-call"></a>Soittaa koodin luominen
Seuraava koodi näkyy muodostaminen jota kutsutaan, kun käyttäjä lähettää lomakkeen **callform.html**näkyvät web-sivun (**makecall.php**). Alla olevassa koodi luo puhelun viestin ja luo puhelun. (Käytä Twilio tili ja todennusta tunnussanoma **$sid** ja **$token** alla olevassa koodissa paikkamerkin arvojen sijaan).

    <html>
    <head><title>Making call...</title></head>
    <body>
    <p>Your call is being made.</p>

    <?php
    require_once 'Services/Twilio.php';

    $sid = "your_account_sid";
    $token = "your_authentication_token";

    $from_number = $_POST['callFrom']; // Calls must be made from a registered Twilio number.
    $to_number = $_POST['callTo'];
    $message = $_POST['callText'];

    $client = new Services_Twilio($sid, $token, "2010-04-01");

    $call = $client->account->calls->create(
        $from_number,
        $to_number,
        'http://twimlets.com/message?Message='.urlencode($message)
    );

    echo "Call status: ".$call->status."<br />";
    echo "URI resource: ".$call->uri."<br />";
    ?>
    </body>
    </html>

Lisäksi soittaa, **makecall.php** näyttää puhelun metatietoja (esimerkissä näyttökuvassa). Saat lisätietoja puhelun metatietojen [https://www.twilio.com/docs/api/rest/call#instance-properties][twilio_call_properties].

![Azure puhelun vastauksen Twilio ja PHP käyttäminen][twilio_php_response]

## <a name="run-the-application"></a>Suorita sovellus
Seuraava vaihe on ottaa käyttöön sovelluksen Azure-sivustoihin. Seuraavissa artikkeleissa on lisätietoja sivuston luomisesta ja käyttöönotto koodisi Git, FTP tai WebMatrix (mutta ei välttämättä kaikissa kunkin artikkelin tiedot on merkitystä):

* [PHP MySQL Azure-Web-sivuston luominen ja käyttäminen Git otetaan käyttöön][website-git]
* [PHP MySQL Azure Web-sivuston luominen ja ota käyttöön FTP: N avulla][website-ftp]

## <a name="next-steps"></a>Seuraavat vaiheet
Tämä koodi määritettyä näyttämään Twilio käyttäminen PHP Azure-perustoiminnot. Ennen kuin otat käyttöön Azure tuotannon, haluat ehkä lisätä Lisää Virheenkäsittely tai muita ominaisuuksia. Esimerkki:

* Onnistunut Käytä sijaan verkkolomakkeen tallentamiseen puhelinnumerot ja soita tekstin tallennustilan Azure-BLOB- tai SQL-tietokanta. Lisätietoja tallennustilan Azure-BLOB käyttämisestä PHP on artikkelissa [Azure-tallennustilan käyttämällä PHP-sovellusten kanssa][howto_blob_storage_php]. Lisätietoja SQL-tietokannan käyttäminen PHP on [SQL-tietokannan käyttäminen PHP sovellusten][howto_sql_azure_php].
* **Makecall.php** koodi käyttää antamalla Twilio URL-osoite ([http://twimlets.com/message][twimlet_message_url]) antamaan Twilio Markup Language (TwiML)-vastauksen, joka ilmoittaa Twilio puhelun toimintatapaa. Esimerkiksi TwiML, joka palautetaan, voi olla `<Say>` toiminnon, joka johtaa tekstin lausumisen soitettaessa vastaanottajalle. Antamalla Twilio URL-Osoitteen sijaan voit luoda oman palvelun Twilio's kokouspyyntöön; Katso lisätietoja, [miten voit käyttää Twilio ääni-ja PHP SMS-ominaisuuksien][howto_twilio_voice_sms_php]. Lisätietoja TwiML tukikäytännöistä [http://www.twilio.com/docs/api/twiml][twiml], ja haluat lisätietoja `<Say>` ja muut Twilio verbien tukikäytännöistä [http://www.twilio.com/docs/api/twiml/say][twilio_say].
* Lue osoitteessa [https://www.twilio.com/docs/security]Twilio suojaus-ohjeet[twilio_docs_security].

Saat lisätietoja Twilio [https://www.twilio.com/docs][twilio_docs].

## <a name="see-also"></a>Katso myös
* [Opi käyttämään Twilio ääni-ja SMS-ominaisuuksien PHP](partner-twilio-php-how-to-use-voice-sms.md)

[twilio_pricing]: http://www.twilio.com/pricing
[try_twilio]: http://www.twilio.com/try-twilio
[twilio_api]: http://www.twilio.com/api
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[setup_php_sdk]: http://azurephp.interoperabilitybridges.com/articles/setup-the-windows-azure-sdk-for-php
[twimlet_message_url]: http://twimlets.com/message
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api_service]: http://api.twilio.com
[build_php_azure_app]: http://azurephp.interoperabilitybridges.com/articles/build-and-deploy-a-windows-azure-php-application
[howto_twilio_voice_sms_php]: partner-twilio-php-how-to-use-voice-sms.md
[howto_blob_storage_php]: http://azure.microsoft.com/documentation/articles/storage-php-how-to-use-blobs/
[howto_sql_azure_php]: http://azure.microsoft.com/documentation/articles/sql-database-php-how-to-use/
[twilio_call_properties]: https://www.twilio.com/docs/api/rest/call#instance-properties
[twilio_docs_security]: http://www.twilio.com/docs/security
[twilio_docs]: http://www.twilio.com/docs
[twilio_say]: http://www.twilio.com/docs/api/twiml/say
[ssl_validation]: http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html
[twilio_php]: ./media/partner-twilio-php-make-phone-call/WA_TwilioPHPCallForm.jpg
[twilio_php_response]: ./media/partner-twilio-php-make-phone-call/WA_TwilioPHPMakeCall.jpg
[website-git]: ./web-sites/web-sites-php-mysql-deploy-use-git.md
[website-ftp]: ./web-sites/web-sites-php-mysql-deploy-use-ftp.md
[twilio_php_github]: https://github.com/twilio/twilio-php

<properties 
    pageTitle="Opi käyttämään SendGrid sähköpostipalvelu (PHP) | Microsoft Azure" 
    description="Lue, miten lähettää sähköpostia SendGrid sähköpostipalvelu Azure. Kirjoitettu PHP MALLIKOODEJA." 
    documentationCenter="php" 
    services="" 
    manager="sendgrid" 
    editor="mollybos" 
    authors="thinkingserious"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="PHP" 
    ms.topic="article" 
    ms.date="10/30/2014" 
    ms.author="elmer.thomas@sendgrid.com; erika.berkland@sendgrid.com; vibhork; matt.bernier@sendgrid.com"/>
# <a name="how-to-use-the-sendgrid-email-service-from-php"></a>Opi käyttämään SendGrid sähköpostipalvelu PHP kohteesta

Tässä oppaassa kerrotaan, miten Azure SendGrid sähköpostipalvelu yleisten ohjelmoinnin tehtävien suorittamiseen. Mallit on kirjoitettu PHP.
Tilanteita, joissa kattaa Sisällytä **sähköpostin luominen**ja **lähettäminen sähköpostin** **liitteiden lisääminen**. Lisätietoja SendGrid ja sähköpostin lähettämistä on kohdassa [Vaiheisiin](#next-steps) .

## <a name="what-is-the-sendgrid-email-service"></a>Mikä on SendGrid sähköpostipalvelu?

SendGrid on [pilvipohjainen sähköpostipalvelu] , joka sisältää luotettava [tapahtumien sähköpostin toimittamista], skaalattavuus ja reaaliaikainen analytics sekä joustava API, jotka helpottavat mukautetun integrointi. Yleisiä SendGrid käyttö tilanteita, joissa ovat seuraavat:

-   Lähettäminen automaattisesti kuittaukset asiakkaat
-   Jakauman hallinta näyttää lähettämiseen asiakkaiden kuukausittain e-fliers ja Erikoistarjoukset
-   Reaaliaikainen estetyn sähköposti- ja asiakkaan vastausajan esimerkiksi mittaukset kerääminen
-   Voit tunnistaa suuntaukset raporttien luominen
-   Välityksen asiakkaan kyselyt
- Sähköposti-ilmoitusten sovelluksestasi

Lisätietoja on artikkelissa [https://sendgrid.com][].

## <a name="create-a-sendgrid-account"></a>SendGrid-tilin luominen

[AZURE.INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## <a name="using-sendgrid-from-your-php-application"></a>Käyttämällä SendGrid PHP-sovelluksesta

SendGrid käyttäminen Azure PHP sovellus edellyttää ei määräten määritysten tai tuntemusta. Koska SendGrid on palvelu, sitä voi käyttää täsmälleen cloud-sovelluksesta samalla tavalla kuin mahdollista paikallisen-sovelluksesta.

## <a name="how-to-send-an-email"></a>Toimintaohje: Lähetä sähköpostiviesti

Voit lähettää sähköpostin SMTP- tai verkko-Ohjelmointirajapinnan SendGrid myöntämä.

### <a name="smtp-api"></a>SMTP-OHJELMOINTIRAJAPINTA

Lähetä sähköpostia käyttäen SendGrid SMTP-Ohjelmointirajapinta, käytä *Swift Postittaja*-osa-pohjainen kirjaston lähettämiseen sähköpostit PHP sovelluksista. Voit ladata *Swift Postittaja* kirjaston [http://swiftmailer.org/download][] v5.3.0 (Käytä asentaminen Swift Postittaja [tunnus] ). Kirjaston sähköpostin lähettämisestä liittyy luominen esiintymät <span class="auto-style2">Swift\_SmtpTransport</span>, <span class="auto-style2">Swift\_Postittaja</span>, ja <span class="auto-style2">Swift\_viestin</span> luokkien määrittäminen haluamasi ominaisuudet ja soitat <span class="auto-style2">Swift\_Mailer::send</span> menetelmää.

    <?php
     include_once "vendor/autoload.php";
     /*
      * Create the body of the message (a plain-text and an HTML version).
      * $text is your plain-text email
      * $html is your html version of the email
      * If the reciever is able to view html emails then only the html
      * email will be displayed
      */ 
     $text = "Hi!\nHow are you?\n";
     $html = "<html>
           <head></head>
           <body>
               <p>Hi!<br>
                   How are you?<br>
               </p>
           </body>
           </html>";
     // This is your From email address
     $from = array('someone@example.com' => 'Name To Appear');
     // Email recipients
     $to = array(
           'john@contoso.com'=>'Destination 1 Name',
           'anna@contoso.com'=>'Destination 2 Name'
     );
     // Email subject
     $subject = 'Example PHP Email';

     // Login credentials
     $username = 'yoursendgridusername';
     $password = 'yourpassword';
     
     // Setup Swift mailer parameters
     $transport = Swift_SmtpTransport::newInstance('smtp.sendgrid.net', 587);
     $transport->setUsername($username);
     $transport->setPassword($password);
     $swift = Swift_Mailer::newInstance($transport);

     // Create a message (subject)
     $message = new Swift_Message($subject);

     // attach the body of the email
     $message->setFrom($from);
     $message->setBody($html, 'text/html');
     $message->setTo($to);
     $message->addPart($text, 'text/plain');
     
     // send message 
     if ($recipients = $swift->send($message, $failures))
     {
         // This will let us know how many users received this message
         echo 'Message sent out to '.$recipients.' users';
     }
     // something went wrong =(
     else
     {
         echo "Something went wrong - ";
         print_r($failures);
     }

### <a name="web-api"></a>Verkko-Ohjelmointirajapinnan

Sähköpostin lähettäminen käyttämällä SendGrid verkko-Ohjelmointirajapinnan PHP's [curl-funktion][] avulla.

    <?php

     $url = 'https://api.sendgrid.com/';
     $user = 'USERNAME';
     $pass = 'PASSWORD'; 

     $params = array(
          'api_user' => $user,
          'api_key' => $pass,
          'to' => 'john@contoso.com',
          'subject' => 'testing from curl',
          'html' => 'testing body',
          'text' => 'testing body',
          'from' => 'anna@contoso.com',
       );
       
     $request = $url.'api/mail.send.json';
     
     // Generate curl request
     $session = curl_init($request);
     
     // Tell curl to use HTTP POST
     curl_setopt ($session, CURLOPT_POST, true);
     
     // Tell curl that this is the body of the POST
     curl_setopt ($session, CURLOPT_POSTFIELDS, $params);
     
     // Tell curl not to return headers, but do return the response
     curl_setopt($session, CURLOPT_HEADER, false);
     curl_setopt($session, CURLOPT_RETURNTRANSFER, true);
     
     // obtain response
     $response = curl_exec($session);
     curl_close($session);
     
     // print everything out
     print_r($response);

SendGrid's verkko-Ohjelmointirajapinnan on hyvin samantyyppinen REST API-, mutta se ei ole todella RESTful-Ohjelmointirajapinta, koska useimmat kutsuissa, sekä HAE ja kirjaa verbien voidaan tarkoittavat.

## <a name="how-to-add-an-attachment"></a>Toimintaohje: Lisää liitetiedosto

### <a name="smtp-api"></a>SMTP-OHJELMOINTIRAJAPINTA

Esimerkki komentosarjan lähettäminen sähköpostiviesti, jossa Swift Postittaja koodin yhden lisäosan SMTP-Ohjelmointirajapinnan käyttäminen liitteen lähettäminen kuuluu.

    <?php
     include_once "vendor/autoload.php";
     /*
      * Create the body of the message (a plain-text and an HTML version).
      * $text is your plain-text email
      * $html is your html version of the email
      * If the reciever is able to view html emails then only the html
      * email will be displayed
      */
     $text = "Hi!\nHow are you?\n";
      $html = "<html>
          <head></head>
          <body>
             <p>Hi!<br>
                How are you?<br>
             </p>
          </body>
          </html>";

     // This is your From email address
     $from = array('someone@example.com' => 'Name To Appear');
     
     // Email recipients
     $to = array(
          'john@contoso.com'=>'Destination 1 Name',
          'anna@contoso.com'=>'Destination 2 Name'
     );
     // Email subject
     $subject = 'Example PHP Email';
     
     // Login credentials
     $username = 'yoursendgridusername';
     $password = 'yourpassword';
     
     // Setup Swift mailer parameters
     $transport = Swift_SmtpTransport::newInstance('smtp.sendgrid.net', 587);
     $transport->setUsername($username);
     $transport->setPassword($password);
     $swift = Swift_Mailer::newInstance($transport);
     
     // Create a message (subject)
     $message = new Swift_Message($subject);
     
     // attach the body of the email
     $message->setFrom($from);
     $message->setBody($html, 'text/html');
     $message->setTo($to);
     $message->addPart($text, 'text/plain');
     $message->attach(Swift_Attachment::fromPath("path\to\file")->setFileName("file_name"));
     
     // send message 
     if ($recipients = $swift->send($message, $failures))
     {
          // This will let us know how many users received this message
          echo 'Message sent out to '.$recipients.' users';
     }
     // something went wrong =(
     else
     {
          echo "Something went wrong - ";
          print_r($failures);
     }

Lisää rivin koodi on seuraavanlainen:

     $message->attach(Swift_Attachment::fromPath("path\to\file")->setFileName('file_name'));

Tämä kutsuu Liitä-menetelmää <span class="auto-style2">Swift\_viestin</span> objekti ja käyttää staattinen menetelmä <span class="auto-style2">fromPath</span> <span class="auto-style2">Swift\_liitteen</span> luokan hakee ja tiedoston liittäminen viestiin.

### <a name="web-api"></a>Verkko-Ohjelmointirajapinnan

Verkko-Ohjelmointirajapinnan käyttäminen liitteen lähettäminen muistuttaa verkko-Ohjelmointirajapinnan käyttäminen sähköpostin lähettämiseen. Huomaa kuitenkin, että jälkeen tulevaa esimerkissä parametri-matriisissa on oltava tämä elementti:

    'files['.$fileName.']' => '@'.$filePath.'/'.$fileName

Esimerkki:

    <?php

     $url = 'https://api.sendgrid.com/';
     $user = 'USERNAME';
     $pass = 'PASSWORD';
     
     $fileName = 'myfile';
     $filePath = dirname(__FILE__);

     $params = array(
         'api_user' => $user,
         'api_key' => $pass,
         'to' =>'john@contoso.com',
         'subject' => 'test of file sends',
         'html' => '<p> the HTML </p>',
         'text' => 'the plain text',
         'from' => 'anna@contoso.com',
         'files['.$fileName.']' => '@'.$filePath.'/'.$fileName
     );
     
     print_r($params);
     
     $request = $url.'api/mail.send.json';
     
     // Generate curl request
     $session = curl_init($request);
     
     // Tell curl to use HTTP POST
     curl_setopt ($session, CURLOPT_POST, true);
     
     // Tell curl that this is the body of the POST
     curl_setopt ($session, CURLOPT_POSTFIELDS, $params);
     
     // Tell curl not to return headers, but do return the response
     curl_setopt($session, CURLOPT_HEADER, false);
     curl_setopt($session, CURLOPT_RETURNTRANSFER, true);
     
     // obtain response
     $response = curl_exec($session);
     curl_close($session);
     
     // print everything out
     print_r($response);

## <a name="how-to-use-filters-to-enable-footers-tracking-and-analytics"></a>Toimintaohje: suodattimien avulla voit ottaa käyttöön, alatunnisteita, seuranta ja Analytics

SendGrid on muita sähköpostin toimintoja käyttämällä "suodattimet". Nämä ovat asetuksia, jotka voidaan lisätä sähköpostiviestiin, jos haluat ottaa käyttöön tiettyjä toimintoja, kuten ottaminen käyttöön, valitse seuranta, Google analytics, tilauksen seuranta ja niin edelleen.

Suodattimia voi käyttää viestin suodattimet-ominaisuuden avulla. Kukin suodatin määrittämän hash, joka sisältää suodattimen kielikohtaiset asetukset. Seuraavassa esimerkissä alatunniste-suodattimen avulla ja määrittää tekstiviestin, joka lisätään viestin loppuun.
Tässä esimerkissä Käytämme [sendgrid php kirjastoon].
Asenna kirjaston [tunnus] avulla:
    
    php composer.phar require sendgrid/sendgrid 2.1.1

Esimerkki:    

    <?php
     /*
      * This example is used for sendgrid-php V2.1.1 (https://github.com/sendgrid/sendgrid-php/tree/v2.1.1)
      */
     include "vendor/autoload.php";

     $email = new SendGrid\Email();
     // The list of addresses this message will be sent to
     // [This list is used for sending multiple emails using just ONE request to SendGrid]
     $toList = array('john@contoso.com', 'anna@contoso.com');

     // Specify the names of the recipients
     $nameList = array('Name 1', 'Name 2');

     // Used as an example of variable substitution
     $timeList = array('4 PM', '5 PM');

     // Set all of the above variables
     $email->setTos($toList);
     $email->addSubstitution('-name-', $nameList);
     $email->addSubstitution('-time-', $timeList);

     // Specify that this is an initial contact message
     $email->addCategory("initial");

     // You can optionally setup individual filters here, in this example, we have 
     // enabled the footer filter
     $email->addFilter('footer', 'enable', 1);
     $email->addFilter('footer', "text/plain", "Thank you for your business");
     $email->addFilter('footer', "text/html", "Thank you for your business");

     // The subject of your email
     $subject = 'Example SendGrid Email';

     // Where is this message coming from. For example, this message can be from 
     // support@yourcompany.com, info@yourcompany.com
     $from = 'someone@example.com';

     // If you do not specify a sender list above, you can specifiy the user here. If 
     // a sender list IS specified above, this email address becomes irrelevant.
     $to = 'john@contoso.com';

     # Create the body of the message (a plain-text and an HTML version). 
     # text is your plain-text email 
     # html is your html version of the email
     # if the receiver is able to view html emails then only the html
     # email will be displayed

     /*
      * Note the variable substitution here =)
      */
     $text = "
     Hello -name-,
     Thank you for your interest in our products. We have set up an appointment to call you at -time- EST to discuss your needs in more detail.
     Regards,
     Fred";

     $html = "
     <html> 
     <head></head>
     <body>
     <p>Hello -name-,<br>
     Thank you for your interest in our products. We have set up an appointment
     to call you at -time- EST to discuss your needs in more detail.

     Regards,

     Fred<br>
     </p>
     </body>
     </html>";

     // set subject
     $email->setSubject($subject);

     // attach the body of the email
     $email->setFrom($from);
     $email->setHtml($html);
     $email->addTo($to);
     $email->setText($text);

     // Your SendGrid account credentials
     $username = 'sendgridusername@yourdomain.com';
     $password = 'example';

     // Create SendGrid object
     $sendgrid = new SendGrid($username, $password);

     // send message
     $response = $sendgrid->send($email);

     print_r($response);

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet tutustunut SendGrid sähköpostipalvelu perusteet, noudata näitä linkkejä lisätietoja.

-   SendGrid ohjeissa: <https://sendgrid.com/docs>
-   SendGrid PHP kirjaston: <https://github.com/sendgrid/sendgrid-php>
-   Azure asiakkaiden SendGrid erikoistarjous: <https://sendgrid.com/windowsazure.html>

Lisätietoja on Katso myös [PHP Developer Center](/develop/php/).


  [https://sendgrid.com]: https://sendgrid.com
  [https://sendgrid.com/transactional-email/pricing]: https://sendgrid.com/transactional-email/pricing
  [special offer]: https://www.sendgrid.com/windowsazure.html
  [Packaging and Deploying PHP Applications for Azure]: http://msdn.microsoft.com/library/windowsazure/hh674499(v=VS.103).aspx
  [http://swiftmailer.org/Download]: http://swiftmailer.org/download
  [CURL-funktio]: http://php.net/curl
  [cloud-pohjainen sähköpostipalvelu]: https://sendgrid.com/email-solutions
  [tapahtumien sähköpostin lähettämisestä]: https://sendgrid.com/transactional-email
  [sendgrid php kirjastoon]: https://github.com/sendgrid/sendgrid-php/tree/v2.1.1
  [Tunnus]: https://getcomposer.org/download/

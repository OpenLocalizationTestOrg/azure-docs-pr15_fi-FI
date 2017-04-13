<properties 
    pageTitle="Opi käyttämään Twilio ääni-ja SMS (Ruby) | Microsoft Azure" 
    description="Opettele puhelun soittaminen ja lähettää Azure tekstiviesti Twilio API-palvelussa. Ruby kirjoitetut MALLIKOODEJA." 
    services="" 
    documentationCenter="ruby" 
    authors="devinrader" 
    manager="twilio" 
    editor=""/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ruby" 
    ms.topic="article" 
    ms.date="11/25/2014" 
    ms.author="MicrosoftHelp@twilio.com"/>





# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-ruby"></a>Opi käyttämään Twilio ääni-ja SMS Ruby-ominaisuudet
Tässä oppaassa kerrotaan, miten tekemässä Azure ohjelmoinnin perustehtäviä Twilio API-palvelussa. Tilanteita, joissa kattaa ovat soittaa puheluita ja lyhyen viestin Service (SMS) viestin lähettämistä. Lisätietoja Twilio ja ääni- ja SMS käyttäminen sovellukset-kohdassa [Seuraavat vaiheet](#NextSteps) .

## <a id="WhatIs"></a>Mikä on Twilio?
Twilio on Puhelin-WWW-palvelun API, voit käyttää aiemmin web kielet ja taitojen luonnissa ääni- ja SMS sovellukset. Twilio on kolmannen osapuolen palvelu (ei Azure ominaisuus ja ole Microsoft-tuotteen).

**Twilio äänen** avulla sovellukset voivat soittaa ja vastaanottaa puheluita. **Twilio SMS** avulla sovellukset voivat soittaa ja vastaanottaa tekstiviestejä. **Twilio asiakkaan** avulla sovellustesi voice tietoliikenne käyttämällä olemassa olevan Internet-yhteydet, mukaan lukien mobile yhteyksiä.

## <a id="Pricing"></a>Twilio hinnat ja Erikoistarjoukset
Tietoja Twilio hinnat on saatavilla osoitteessa [Twilio hinnat] [twilio_pricing]. Azure asiakkaiden käyttämien [erikoistarjous][special_offer]: ilmainen tilaus odottaa luottotietojen 1000 tekstejä tai 1000 saapuva minuuttia. Rekisteröityminen tarjoukseen tai saada lisätietoja, käy [http://ahoy.twilio.com/azure][special_offer].  

## <a id="Concepts"></a>Käsitteitä
Twilio-Ohjelmointirajapinnan on RESTful-Ohjelmointirajapinta, joka tarjoaa ääni- ja SMS toimintoja sovellukset. Asiakkaan kirjastot ovat käytettävissä useilla kielillä luettelo on artikkelissa [Twilio API kirjastojen] [twilio_libraries].

### <a id="TwiML"></a>TwiML
TwiML on joukko XML-pohjainen ohjeita, jotka ilmoittavat käsitteleminen puhelun Twilio tai SMS.

Esimerkiksi seuraavat TwiML muuntaa tekstin **Hei maailma** puhe.

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World</Say>
    </Response>

Kaikki TwiML asiakirjojen `<Response>` kuin niiden pääelementti. Sieltä Käytä Twilio verbien voit määrittää sovelluksen toimintaa.

### <a id="Verbs"></a>TwiML käyttö
Twilio verbien ovat XML-tunnisteita, jotka kertovat Twilio mitä voit **tehdä**. Esimerkiksi ** &lt;sanoa&gt; ** verbin ohjaa Twilio aikana audibly puhelua viestiin. 

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

Saat lisätietoja Twilio verbien niiden määritteitä ja TwiML [TwiML] [twiml]. Saat lisätietoja Twilio Ohjelmointirajapinnan [Twilio API] [twilio_api].

## <a id="CreateAccount"></a>Twilio-tilin luominen
Kun olet valmis, saat Twilio-tiliä, Rekisteröidy, [Yritä Twilio] [try_twilio]. Voit aloittaa maksuttoman tilillä, ja tilin päivityksen myöhemmin.

Kun kirjaudut Twilio tilin, saat ilmaisia puhelinnumero sovelluksen. Saat myös tili SUOJAUSTUNNUS ja auth-tunnuksen. Molemmat tarvitaan Twilio API soittamiseen. Voit estää näin luvattoman pääsyn tilisi, suojata todennus-tunnuksen. Tilin SUOJAUSTUNNUS ja auth tunnuksen ovat tarkasteltava [Twilio tilisivun][twilio_account]-kentät nimetty **tilin SUOJAUSTUNNUS** ja **AUTH tunnuksen**, vastaavassa järjestyksessä.

### <a id="VerifyPhoneNumbers"></a>Tarkista puhelinnumeroita
Lisäksi numero on antamien Twilio, voit tarkistaa, lukuja, voit määrittää (eli Matkapuhelin tai koti puhelinnumerosi) sovellustesi käytettäviksi. 

Lisätietoja siitä, miten voit tarkistaa puhelinnumero [Hallinta numerot]näkyvät [verify_phone].

## <a id="create_app"></a>Ruby-sovelluksen luominen
Ruby-sovellus, joka käyttää Twilio-palvelua ja on käynnissä Azure-tietokannassa on sama asia kuin muiden Ruby sovellus, joka käyttää Twilio-palvelua. Kun Twilio palvelut RESTful ja voidaan kutsua Ruby usealla eri tavalla, tässä artikkelissa keskitytään Twilio palveluiden käyttäminen [Twilio helper kirjasto Ruby][twilio_ruby].

Ensin [sisältyvien uusien Azure-Linux AM] [ azure_vm_setup] uuden Ruby web-sovelluksen isäntä edustajana. Ohita henkilöihin liittyvät kiskot-sovelluksessa juuri määritetty AM luomisen vaiheet. Varmista, että luot päätepisteen ulkoisen portin 80 ja sisäinen portin 5000.

Alla olevassa esimerkissä on käyttää [Sinatra][sinatra], Ruby erittäin yksinkertaisia web raamit. Mutta voit käyttää Twilio helper kirjaston Ruby varmasti kaikki muut web Framework, mukaan lukien Ruby kiskoilla.

SSH tuominen uuteen AM ja luo uusi sovellus kansio. Kyseiseen kansioon sisällä Gemfile-tiedoston luominen ja kopioi se seuraava koodi:

    source 'https://rubygems.org'
    gem 'sinatra'
    gem 'thin'

Suorita komentorivillä `bundle install`. Asennetaan yllä riippuvuudet. Luo seuraava tiedosto nimeltä `web.rb`. Tämä on, jossa koodiin koodi sijaitsee. Liitä seuraava koodi siihen:

    require 'sinatra'

    get '/' do
        "Hello Monkey!"
    end

Tässä vaiheessa sinun pitäisi Suorita komennon `ruby web.rb -p 5000`. Tämä askelluspainikkeen ylöspäin portin 5000 pieni web-palvelimeen. Sinun pitäisi selaamalla ilmoituksen: sovellukseen selaimessa ohjesisältöä URL-Osoitteen voit aloittaa Azure AM. Kun pääset koodiin selaimessa, voit ryhtyä toimeen Twilio-sovelluksen luominen.

## <a id="configure_app"></a>Sovelluksen käyttämään Twilio määrittäminen
Voit määrittää Twilio-kirjaston käyttäminen päivittämällä koodiin oman `Gemfile` haluat sisällyttää tämän rivin:

    gem 'twilio-ruby'

Suorita komentorivillä `bundle install`. Avaa nyt `web.rb` yläreunassa rivin mukaan lukien:

    require 'twilio-ruby'

Olet nyt valmis käyttämään Twilio helper kirjaston Ruby-sovellukseen.

## <a id="howto_make_call"></a>Toimintaohje: lähtevän soittaminen
Seuraavassa kerrotaan, miten lähtevän soittaminen. Avaimen käsitteitä ovat käyttämällä Ruby Twilio helper kirjasto REST API-puheluissa ja värien TwiML. Korvaa arvot ** **- ja** ** puhelinnumeroita ja poista Tarkista puhelinnumero **-** tilisi Twilio ennen koodin suorittamista.

Lisää tämän funktion avulla `web.md`:

    # Set your account ID and authentication token.
    sid = "your_twilio_account_sid";
    token = "your_twilio_authentication_token";

    # The number of the phone initiating the the call.
    # This should either be a Twilio number or a number that you've verified
    from = "NNNNNNNNNNN";

    # The number of the phone receiving call.
    to = "NNNNNNNNNNN";

    # Use the Twilio-provided site for the TwiML response.
    url = "http://yourdomain.cloudapp.net/voice_url";
      
    get '/make_call' do
      # Create the call client.
      client = Twilio::REST::Client.new(sid, token);
      
      # Make the call
      client.account.calls.create(to: to, from: from, url: url)
    end

    post '/voice_url' do
      "<Response>
         <Say>Hello Monkey!</Say>
       </Response>"
    end
    
Jos voit avata ylöspäin `http://yourdomain.cloudapp.net/make_call` selaimessa, joka käynnistää puhelun Twilio ohjelmointirajapinnan Määritä puhelun. Kaksi ensimmäistä parametrit `client.account.calls.create` ovat varsin selkeitä: numero puhelu on `from` ja numero puhelu on `to`. 

Kolmas parametrin (`url`) Twilio pyytää saat ohjeita mitä tehdään, kun puhelu on yhdistetty URL-osoite. Tässä tapauksessa emme määritetään URL-osoitteen (`http://yourdomain.cloudapp.net`), joka palauttaa yksinkertaisen TwiML asiakirjan ja käyttää `<Say>` verbin joitakin teksti puheeksi-toiminnon ja sano "Hei-apina" hänelle soittaa.

## <a id="howto_recieve_sms"></a>Toimintaohje: vastaanota tekstiviestin
Edellisessä esimerkissä on omat **Lähtevä** puhelu. Tämä kerran, käytetään puhelinnumero, Twilio antama us rekisteröitymisen yhteydessä käsittelemään **saapuvia** tekstiviesti.

Ensimmäinen, kirjaudu sisään [Twilio Raporttinäkymät-ikkunan][twilio_account]. Valitse yläreunan siirtymispalkissa "Numerot" ja valitse sitten on annettu Twilio määrää. Näyttöön tulee kaksi URL-osoitteet, voit määrittää. Vastaajan pyyntö URL-osoite ja Tekstiviestin pyynnön URL-osoite. Nämä ovat Twilio soittaa aina, kun puhelu on tehty tai Tekstiviestin lähetetään sivunumeron URL-osoitteet. URL-osoitteet, kutsutaan myös "web koukkuja".

Haluamme käsittelemään saapuvia tekstiviestien, joten katsotaan seuraavaksi päivittäminen URL-Osoitteen `http://yourdomain.cloudapp.net/sms_url`. Siirry eteenpäin ja valitse Tallenna muutokset sivun alareunassa. Palattuasi takaisin `web.rb` ohjelman japanin Microsoftin käsittelemään tämän sovelluksen:

    post '/sms_url' do
      "<Response>
         <Message>Hey, thanks for the ping! Twilio and Azure rock!</Message>
       </Response>"
    end

Kun olet tehnyt muutoksen, varmista, että Käynnistä uudelleen web app. Tutustu puhelimesi ulos ja tekstiviestien Twilio numeroon. Näyttöön tulee välittömästi SMS-vastauksen, jossa lukee "Hey, ping Kiitos! Twilio ja Azure rock! ".

## <a id="additional_services"></a>Toimintaohje: Käytä muita Twilio palvelut
Lisäksi tässä esitetty esimerkkejä Twilio on verkkopohjainen API, joiden avulla voit hyödyntää Twilio lisätoimintoja Azure-sovelluksesta. Katso tarkat tiedot [Twilio API-ohjeissa] [twilio_api_documentation].

### <a id="NextSteps"></a>Seuraavat vaiheet
Nyt kun olet oppinut perustiedot Twilio-palvelun, näistä linkeistä saat lisätietoja:

* [Twilio suojausta koskevia ohjeita] [twilio_security_guidelines]
* [Twilio HowTos ja esimerkkikoodi] [twilio_howtos]
* [Twilio pikaopas opetusohjelmat][twilio_quickstarts] 
* [Valitse GitHub Twilio] [twilio_on_github]
* [Jutella Twilio tuki] [twilio_support]

[twilio_ruby]: https://www.twilio.com/docs/ruby/install





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
[sinatra]: http://www.sinatrarb.com/
[azure_vm_setup]: http://www.windowsazure.com/develop/ruby/tutorials/web-app-with-linux-vm/

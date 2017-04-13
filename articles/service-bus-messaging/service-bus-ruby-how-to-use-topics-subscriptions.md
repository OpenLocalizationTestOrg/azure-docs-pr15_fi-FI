<properties
    pageTitle="Käyttämisestä palvelun Bus aiheet (Ruby) | Microsoft Azure"
    description="Opettele käyttämään palvelua Bus aiheet ja tilaukset Azure-tietokannassa. Ruby-sovellusten kirjoitetaan MALLIKOODEJA."
    services="service-bus"
    documentationCenter="ruby"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="ruby"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-topicssubscriptions"></a>Opi käyttämään palvelua Bus aiheet/tilaukset

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Tässä artikkelissa käsitellään palvelun Bus aiheet ja tilaukset Ruby sovelluksista. Tilanteita, joissa kattaa Sisällytä **luominen aiheet ja tilaukset-luominen tilauksen suodattimet-viestien lähettäminen** aiheen, **vastaanottaa viestejä tilauksesta**ja **poistamalla aiheita ja tilaukset**. Lisätietoja aiheet ja tilaukset-kohdassa [Seuraavat vaiheet](#next-steps) .

## <a name="service-bus-topics-and-subscriptions"></a>Palvelun Bus aiheet ja tilaukset

Palvelun Bus aiheet ja tilaukset tukevat *Julkaise ja tilaa* messaging viestintä-malli. Kun käytät aiheet ja tilaukset, jaetun sovelluksen osia ei keskenään suoraan; ne vaihtaa sen sijaan viestejä ohjeaiheessa, johon välittämään kautta.

![TopicConcepts](./media/service-bus-ruby-how-to-use-topics-subscriptions/sb-topics-01.png)

Toisin kuin palvelun Bus olevien, jossa kukin viesti käsitellään yksittäisen kuluttaja, aiheet ja tilaukset on **yksi-moneen** -lomakkeen viestinnän, Julkaise ja tilaa kaavan. On voitu rekisteröidä useita tilauksia aiheeseen. Kun viesti on lähetetty aiheen, se tehdään sitten käytettävissä jokaisen tilauksen käsittelemään erikseen.

Aiheen tilauksen muistuttaa virtual jono, joka vastaanottaa viestit, jotka on lähetetty aiheen kopioita. Voit halutessasi rekisteröidä suodatusehdot aiheen tilausta kohti välein, jolloin voit suodattimen tai rajoittaa aiheen viestit on vastaanottanut aiheen päättyneet tilaukset.

Palvelun Bus aiheet ja tilaukset, joiden avulla voit skaalata käyttänyt paljon viestejä eri suuri määrä käyttäjiä ja sovellukset.

## <a name="create-a-namespace"></a>Luo nimitila

Kun haluat käyttää palvelun Bus olevien Azure, sinun on luotava nimitila. Nimitilan tarjoaa säilöön osoitteiden palvelun Bus resurssien sovelluksessa. Sinun on luotava nimitilan komentorivivalitsimet-liittymän kautta, koska [Azure portal][] ei luoda nimitilan ACS-yhteyden kautta.

Voit luoda nimitilan seuraavasti:

1. PowerShellin Azure konsoli-ikkunan avaaminen

2. Kirjoita seuraava komento nimitilan luomiseen. Nimitilan oman arvoa ja määritä saman alueen sovelluksen.

    ```
    New-AzureSBNamespace -Name 'yourexamplenamespace' -Location 'West US' -NamespaceType 'Messaging' -CreateACSNamespace $true
    ```

    ![Luo Namespace](./media/service-bus-ruby-how-to-use-topics-subscriptions/showcmdcreate.png)

## <a name="obtain-default-management-credentials-for-the-namespace"></a>Hanki oletusarvon nimitilan tunnistetietojen hallinta

Jotta voit suorittaa hallinta, kuten luomalla uuden nimitilan jonon sinun on hankittava nimitilan tunnistetietojen hallinta.

Luo palvelun Bus nimitilan suoritit PowerShell cmdlet-komento näkyy avulla voit hallita nimitilan avain. Kopioi **DefaultKey** -arvo. Käytät tätä arvoa koodisi jäljempänä tässä opetusohjelmassa olevassa.

![Kopioi avain](./media/service-bus-ruby-how-to-use-topics-subscriptions/defaultkey.png)

> [AZURE.NOTE]
> Voit etsiä avaimeen myös, jos [Azure portal][] kirjautumaan ja siirry Yhteystiedot oman nimitilan.

## <a name="create-a-ruby-application"></a>Ruby-sovelluksen luominen

Katso ohjeet [Luo Azure-Ruby-sovellus](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).

## <a name="configure-your-application-to-use-service-bus"></a>Käytä palvelun Bus sovellusta määrittäminen

Voit käyttää palvelun Bus, lataa ja Ruby Azure-pakettia, joka sisältää helppokäyttöisyys kirjastoja, joissa viestintä tallennustilan muiden palveluiden kanssa.

### <a name="use-rubygems-to-obtain-the-package"></a>Hanki paketin RubyGems avulla

1. Käytä käyttöliittymä esimerkiksi **PowerShell** (Windows), **terminaalissa** (Mac) tai **Bash** (Unix).

2. Kirjoita "gem Asenna azure" Asenna gem ja riippuvuuksien komentoikkunassa.

### <a name="import-the-package"></a>Tuo pakkaaminen

Ruby tiedosto, jonka aiot käyttää tallennustilan alkuun käyttämällä tuttuja tekstieditorissa, Lisää seuraava:

```
require "azure"
```

## <a name="set-up-a-service-bus-connection"></a>Palvelun Bus-yhteyden määrittäminen

Azure moduulin lukee ympäristömuuttujat **AZURE\_SERVICEBUS\_NIMITILAN** ja **AZURE\_SERVICEBUS\_ACCESS\_avain** varten muodostaa yhteyttä nimitilan tarvittavia tietoja. Jos ympäristössä muuttujia ei määritetä, sinun on määritettävä nimitilan tiedot ennen **Azure::ServiceBusService** käyttäminen seuraava koodi:

```
Azure.config.sb_namespace = "<your azure service bus namespace>"
Azure.config.sb_access_key = "<your azure service bus access key>"
```

Määritä nimitilan olet luonut sen sijaan, että koko URL-osoite-arvoa. Käytä esimerkiksi **"yourexamplenamespace"**, ei "yourexamplenamespace.servicebus.windows.net".

## <a name="create-a-topic"></a>Aiheen luominen

**Azure::ServiceBusService** objektin avulla voit käsitellä aiheet. Seuraava koodi luo **Azure::ServiceBusService** objektin. Voit luoda aihe **luominen\_topic()** menetelmää. Seuraava esimerkki luo aiheen tai tulostaa virheet, jos niitä on.

```
azure_service_bus_service = Azure::ServiceBusService.new
begin
  topic = azure_service_bus_service.create_queue("test-topic")
rescue
  puts $!
end
```

Voit myös siirtää **Azure::ServiceBus::Topic** -objekti, jossa on lisäasetuksia, joiden avulla voit korvata aiheen oletusasetukset viestin aika esimerkiksi Live- tai jonon enimmäiskoko. Seuraavassa esimerkissä jonon enimmäiskoko asettaminen 5 gt ja live minuutin ajan:

```
topic = Azure::ServiceBus::Topic.new("test-topic")
topic.max_size_in_megabytes = 5120
topic.default_message_time_to_live = "PT1M"

topic = azure_service_bus_service.create_topic(topic)
```

## <a name="create-subscriptions"></a>Luo tilaukset

Aiheen tilaukset luodaan myös **Azure::ServiceBusService** objekti. Tilaukset nimetään, ja se voi olla valinnainen suodatin, joka rajoittaa viestit toimitetaan tilauksen virtual jonossa olevat.

Tilaukset pysyvä ja edelleen olemassa ennen kuin he joko tai aiheen ne liitetään ja poistetaan. Jos sovellus on logiikan tilauksen luomiseen, se on ensin tarkistettava Jos tilaus on jo olemassa getSubscription-menetelmällä.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Luo tilauksen oletussuodatin (MatchAll)

**MatchAll** suodatin on oletussuodatin, jota käytetään, jos ei suodatusta määritetään, kun luodaan uusi tilaus. Kun **MatchAll** -suodatin on käytössä, kaikki viestit, jotka on julkaistu aiheeseen sijoitetaan tilauksen virtual jonon. Seuraava esimerkki luo tilausta nimeltä "kaikki viestit" ja käyttää **MatchAll** oletussuodatin.

```
subscription = azure_service_bus_service.create_subscription("test-topic", "all-messages")
```

### <a name="create-subscriptions-with-filters"></a>Luo tilaukset suodattimet

Voit myös määrittää suodattimia, joiden avulla voit määrittää, mitkä aiheen lähetetyt viestit näytetään tietyn tilauksen piiriin kuuluvien.

Tuettu tilauksissa suodattimen eniten joustavia tyyppi on **Azure::ServiceBus::SqlFilter**, joka toteuttaa SQL92 alijoukkoa. Viestit, jotka on julkaistu aiheen ominaisuudet toimivat SQL suodattimet. Lisätietoja lausekkeista, jotka voidaan käyttää SQL-suodatin Tarkista [SqlFilter.SqlExpression](http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx) syntaksi.

Voit lisätä suodattimia tilauksen avulla **luominen\_rule()** **Azure::ServiceBusService** objektin menetelmää. Tämän menetelmän avulla voit lisätä uusia suodattimia olemassa olevaan tilaukseen.

Koska oletussuodatin käytetään automaattisesti kaikissa uusissa tilauksissa, sinun on ensin poistettava oletussuodatin tai **MatchAll** ohittaa muut voivat määritellä suodattimet. Voit poistaa oletusarvon säännön avulla **poistaminen\_rule()** **Azure::ServiceBusService** objektin menetelmää.

Seuraavassa esimerkissä luodaan tilausta nimeltä "suuri-viestit" kanssa **Azure::ServiceBus::SqlFilter** , joka valitsee vain viestit, joissa on mukautettu **viestin\_numero** ominaisuus, joka on suurempi kuin 3:

```
subscription = azure_service_bus_service.create_subscription("test-topic", "high-messages")
azure_service_bus_service.delete_rule("test-topic", "high-messages", "$Default")

rule = Azure::ServiceBus::Rule.new("high-messages-rule")
rule.topic = "test-topic"
rule.subscription = "high-messages"
rule.filter = Azure::ServiceBus::SqlFilter.new({
  :sql_expression => "message_number > 3" })
rule = azure_service_bus_service.create_rule(rule)
```

Seuraavassa esimerkissä luodaan vastaavasti tilausta nimeltä "pieni-viestit" **Azure::ServiceBus::SqlFilter** , joka valitsee vain viestit, joiden **message_number** ominaisuus pienempi tai yhtä 3 kanssa:

```
subscription = azure_service_bus_service.create_subscription("test-topic", "low-messages")
azure_service_bus_service.delete_rule("test-topic", "low-messages", "$Default")

rule = Azure::ServiceBus::Rule.new("low-messages-rule")
rule.topic = "test-topic"
rule.subscription = "low-messages"
rule.filter = Azure::ServiceBus::SqlFilter.new({
  :sql_expression => "message_number <= 3" })
rule = azure_service_bus_service.create_rule(rule)
```

Kun viesti on nyt lähetetty "testin aihe", se on aina toimitetaan vastaanottajia "kaikki viestit" aihe-tilaus on tilattu ja valikoivasti toimitettu vastaanottajia tilannut "suuri-viestit" ja "pieni-viestit"-aiheen tilaukset, (sen mukaan, viestin sisältöä).

## <a name="send-messages-to-a-topic"></a>Viestien lähettäminen aiheen

Jos haluat lähettää viestin palvelun Bus aiheen, sovelluksen on käytettävä **Lähetä\_aiheen\_message()** menetelmä **Azure::ServiceBusService** objektin. Palvelun Bus aiheet lähetetyt viestit ovat **Azure::ServiceBus::BrokeredMessage** objektit esiintymät. **Azure::ServiceBus::BrokeredMessage** objekteissa on joukko vakio-ominaisuuksia ( **esimerkiksi** ja **aika\_,\_live**), joka sanasto, jota käytetään pidä mukautetun sovelluksen tiettyjen ominaisuuksien ja merkkijonon tiedot. Sovelluksen välittämällä merkkijonoarvon, voit määrittää viestin tekstiosaan **Lähetä\_aiheen\_message()** menetelmä ja siihen tarvitaan vakio-ominaisuudet täytetään oletusarvojen mukaan.

Seuraavassa esimerkissä viisi testata "testin aihe" viestien lähettäminen. Huomaa, että kustakin viestistä **message_number** mukautetun ominaisuuden arvo vaihtelee-silmukan iteraation (paina ALT + w määrittää, mikä tilaus vastaanottajalle):

```
5.times do |i|
  message = Azure::ServiceBus::BrokeredMessage.new("test message " + i,
    { :message_number => i })
  azure_service_bus_service.send_topic_message("test-topic", message)
end
```

Palvelun Bus ohjeita tukea enimmäiskoon koosta [Vakio taso](service-bus-premium-messaging.md) 256 Kilotavua ja 1 Megatavu [Premium taso](service-bus-premium-messaging.md). Otsikko, joka sisältää vakio- ja mukautettu-sovelluksen ominaisuudet, voi olla koko on enintään 64 Kilotavua. Ei ole rajoitettu viestit säilytetään aiheen määrän, mutta ole pää kokonaiskoko viestit aiheen mukaan. Tässä aiheessa kokoa on määritetty luominen aikaa, yläraja on 5 gt.

## <a name="receive-messages-from-a-subscription"></a>Vastaanottaa viestejä tilaukseen

Viestejä vastaanotetaan tilauksen käyttämisen **vastaanottaa\_tilauksen\_message()** **Azure::ServiceBusService** objektin menetelmää. Oletusarvon mukaan viestit ovat read(peak) ja poistamatta tilauksesta lukittu. Voit kuitenkin lukea ja poistaa viestin tilauksesta määrittämällä **pikanäkymä\_Lukitse** vaihtoehto **Epätosi**.

Oletusarvoisen toimintatavan tekee arvon ja kaksivaiheinen-toimintoa, joka myös, tekee sovellusten, joka ei hyväksy puuttuvat viestit poistetaan. Palvelun Bus saa pyynnön, kun se havaitsee seuraava viesti voi olla kulutettu, lukitsee voit estää muita kuluttajille vastaanottanut sitä ja palaa sovelluksen. Kun sovellus käsittelemisen viestin (tai tallentaa sen luotettavasti myöhempää käsittelyä varten), vastaanota-prosessin toisen vaiheen suorittamisen soittamalla **poistaminen\_tilauksen\_message()** menetelmä ja antamisen viesti poistetaan parametrina. **Poistaminen\_tilauksen\_message()** menetelmä viestin merkitseminen on käytetty ja poista se tilaus.

Jos **: pikanäkymä\_Lukitse** parametrin arvo on **Epätosi**, lukeminen ja viestin poistaminen on helpoin mallin ja sopii parhaiten skenaariot, jossa sovellus hyväksyt ei käsittele viesti, jos. Tutustu tämän harkitse tilanne, jossa kuluttaja vastaanota pyynnön ja sitten kaatuu, ennen kuin sitä käsitellään. Koska palvelun Bus merkittyihin kulutettu, viestin, ja sitten, kun sovellus käynnistyy ja alkaa muissa viestejä uudelleen, se jääneet viesti, joka on käytetty ennen kaatumisen.

Seuraavassa esimerkissä näytetään, miten voi vastaanottaa viestejä ja käsitellyt käyttämällä **vastaanottaa\_tilauksen\_message()**. Esimerkki ensin vastaanottaa ja poistaa viestin "pieni-viestit"-tilauksesi avulla **: pikanäkymä\_Lukitse** arvoksi **false**, ja valitse se saa toisen viestin "suuri viesteistä" ja poistaa viestin käyttämällä **poistaminen\_tilauksen\_message()**:

```
message = azure_service_bus_service.receive_subscription_message(
  "test-topic", "low-messages", { :peek_lock => false })
message = azure_service_bus_service.receive_subscription_message(
  "test-topic", "high-messages")
azure_service_bus_service.delete_subscription_message(message)
```

## <a name="handle-application-crashes-and-unreadable-messages"></a>Kahvan sovellus kaatuu ja voi lukea viestit

Palvelun Bus on toimintoja, joiden avulla virheet sovelluksessa tai viestin käsittelyn vaikeuksia tilanteen palauttamisessa. Jos vastaanottaja-sovellus ei voi käsitellä viestiä jostain syystä ja valitse se voi soittaa **lukituksen\_tilauksen\_message()** **Azure::ServiceBusService** objektin menetelmää. Tämä aiheuttaa palvelun Bus tilauksen piiriin kuuluvien viestin lukituksen ja vastaanottaa uudelleen samaan vievää sovelluksen tai jonkin muun vievää sovelluksen käytettäväksi.

On myös lukittu tilauksen piiriin kuuluvien viestin liittyvät aikakatkaisu ja jos sovellus ei käsitellä viestiä ennen lukituksen aikakatkaisu vanhenee (esimerkiksi, jos sovellus kaatuu), valitse palvelun Bus viestin lukituksen automaattisesti ja lisätään uudelleen Vastaanotettava.

Silloin, kun sovellus kaatuu, kun viestin käsittelyn mutta ennen kuin **poistaa\_tilauksen\_message()** menetelmää kutsutaan ja viesti on toimitettu edelleen sovelluksen uudelleenkäynnistyksen yhteydessä. Tätä kutsutaan usein **Vähintään kerran käsittely**; jokaisen viestin käsitellään vähintään kerran, mutta tietyissä tilanteissa sama viesti saattaa olla toimitettu edelleen. Jos skenaariota ei hyväksy kaksoiskappaleiden käsittely, sitten sovelluskehittäjät Lisää muita logiikan niiden sovelluksen käsittelee kaksoiskappaleita viestin lähettämisen. Tämä logiikka saavutetaan usein avulla **viestin\_tunnus** sanoman, joka pysyy vakiona toimitusyritysten koko-ominaisuuden.

## <a name="delete-topics-and-subscriptions"></a>Poista aiheet ja tilaukset

Ohjeita ja tilaukset on jatkuva ja täytyy erikseen poistaa [Azure portal][] kautta tai sen ohjelmallisesti. Alla olevassa esimerkissä kerrotaan, miten voit poistaa nimeltä "testin aihe" aihe.

```
azure_service_bus_service.delete_topic("test-topic")
```

Aiheen poistaminen poistaa myös tilaukset, jotka on rekisteröity aihetta. Tilauksia voi poistaa myös erikseen. Seuraava koodi kerrotaan, miten nimeltä "suuri-viestit" "testin aihe" aiheesta tilauksen poistaminen:

```
azure_service_bus_service.delete_subscription("test-topic", "high-messages")
```

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet tutustunut palvelun Bus aiheet perusteet, noudata näitä linkkejä lisätietoja.

- Katso [olevien, aiheet, ja tilaukset](service-bus-queues-topics-subscriptions.md).
- [SqlFilter](http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx)API-viittaus.
- Lisätietoja [Azure SDK Ruby](https://github.com/Azure/azure-sdk-for-ruby) -tietovarasto GitHub.
 
[Azure portal]: https://portal.azure.com

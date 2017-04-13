<properties
    pageTitle="Voit käyttää palvelun Bus olevien Ruby | Microsoft Azure"
    description="Opettele käyttämään palvelua Bus olevien Azure-tietokannassa. Ruby kirjoitetut MALLIKOODEJA."
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

# <a name="how-to-use-service-bus-queues"></a>Opi käyttämään palvelua Bus olevien

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Tässä oppaassa kerrotaan, miten käyttämään palvelua Bus olevien. Mallit on kirjoitettu Ruby ja käytä Azure gem. Tilanteita, joissa kattaa ovat **luominen olevien, lähettää ja vastaanottaa viestejä**ja **olevien poistaminen**. Lisätietoja palvelun Bus olevien-kohdassa [Seuraavat vaiheet](#next-steps) .

## <a name="what-are-service-bus-queues"></a>Mitkä ovat palvelun Bus olevien?

Palvelun Bus olevien tukevat *se messaging* viestintä-malli. Olevien, jossa jaetun sovelluksen osia ei keskenään suoraan; sen sijaan ne vaihtaa viestejä jono, jonka välittämään kautta. Viestin tuottaja (lähettäjä) kädet jonossa viestin käytöstä ja jatkaa sitten sen käsittely.
Asynkronisesti viestin kuluttaja (vastaanottaja) hakee jonossa lähettäjä ja käsittelee sen. Tuottaja ei tarvitse odottaa kuluttaja vastauksessa, jotta voit käsitellä ja viestien lähettäminen edelleen. Olevien tarjouksen **ensimmäisen-, ensimmäinen ulos FIFO** viestin lähettämisen vähintään yksi kilpailevien kuluttajille. Viestit ovat eli yleensä vastaanotettu ja käsittelemien siinä järjestyksessä, jossa ne on lisätty jonossa ja viesti on vastaanotettu ja käsitellä vain yhden viestin kuluttaja vastaanottajia.

![QueueConcepts](./media/service-bus-ruby-how-to-use-queues/sb-queues-08.png)

Palvelun Bus olevien on yleinen tekniikka, joka voi käyttää erilaisia skenaariot:

-   Viestintä Internetin kautta tai työntekijä roolien [monitasoisten Azure sovelluksen](service-bus-dotnet-multi-tier-app-using-service-bus-queues.md)välillä.
-   Paikallisen sovellukset ja Azure välisen isännöidään sovellusten [hybrid ratkaisu](service-bus-dotnet-hybrid-app-using-service-bus-relay.md).
-   Viestintä käynnissä paikallisen eri organisaatioissa tai organisaation osastot jaettu sovellus osien välillä.

Olevien avulla voit, joiden avulla voit skaalata sovellustesi paremmin, ja ota käyttöön lisää vikasietoisuudelle, että arkkitehtuuri.

## <a name="create-a-namespace"></a>Luo nimitila

Kun haluat käyttää palvelun Bus olevien Azure, sinun on luotava nimitila. Nimitilan tarjoaa säilöön osoitteiden palvelun Bus resurssien sovelluksessa. Sinun on luotava nimitilan komentorivivalitsimet-liittymän kautta, koska Azure portaalin ei luoda nimitilan ACS-yhteyden kautta.

Voit luoda nimitilan seuraavasti:

1. Avaa PowerShellin Azure-konsolin.

2. Kirjoita seuraava komento palvelun Bus nimitilan luomiseen. Nimitilan oman arvoa ja määritä saman alueen sovelluksen.

    ```
    New-AzureSBNamespace -Name 'yourexamplenamespace' -Location 'West US' -NamespaceType 'Messaging' -CreateACSNamespace $true

    ![Create Namespace](./media/service-bus-ruby-how-to-use-queues/showcmdcreate.png)
    ```

## <a name="obtain-management-credentials-for-the-namespace"></a>Hanki nimitilan tunnistetietojen hallinta

Jotta voit suorittaa hallinta, kuten luomalla uuden nimitilan jonon sinun on hankittava nimitilan tunnistetietojen hallinta.

Luo Azure palvelun bus nimitila suoritit PowerShell cmdlet-komento näkyy avulla voit hallita nimitilan avain. Kopioi **DefaultKey** -arvo. Käytät tätä arvoa koodisi jäljempänä tässä opetusohjelmassa olevassa.

![Kopioi avain](./media/service-bus-ruby-how-to-use-queues/defaultkey.png)

> [AZURE.NOTE] Voit etsiä avaimeen myös, jos [Azure portal](https://portal.azure.com/) kirjautumaan ja siirry Yhteystiedot palvelun Bus nimitilan.

## <a name="create-a-ruby-application"></a>Ruby-sovelluksen luominen

Luo Ruby-sovellus. Katso ohjeet [Luo Azure-Ruby-sovellus](/develop/ruby/tutorials/web-app-with-linux-vm/).

## <a name="configure-your-application-to-use-service-bus"></a>Määritä sovelluksesi voi käyttää palvelua Bus

Voit käyttää Azure palvelun Bus, lataa ja Ruby Azure-pakettia, joka sisältää helppokäyttöisyys kirjastoja, joissa viestintä tallennustilan muiden palveluiden kanssa.

### <a name="use-rubygems-to-obtain-the-package"></a>Hanki paketin RubyGems avulla

1. Käytä käyttöliittymä esimerkiksi **PowerShell** (Windows), **terminaalissa** (Mac) tai **Bash** (Unix).

2. Kirjoita "gem Asenna azure" Asenna gem ja riippuvuuksien komentoikkunassa.

### <a name="import-the-package"></a>Tuo pakkaaminen

Ruby tiedoston, jos aiot käyttää tallennustilan alkuun käyttämällä tuttuja tekstieditorissa, Lisää seuraava:

```
require "azure"
```

## <a name="set-up-an-azure-service-bus-connection"></a>Azure-palvelu Bus-yhteyden määrittäminen

Azure moduulin lukee ympäristömuuttujat **AZURE\_SERVICEBUS\_NIMITILAN** ja **AZURE\_SERVICEBUS\_ACCESS_KEY** muodostettava yhteys palvelun Bus nimitilan tietoja. Jos ympäristössä muuttujia ei määritetä, sinun on määritettävä nimitilan tiedot ennen **Azure::ServiceBusService** käyttäminen seuraava koodi:

```
Azure.config.sb_namespace = "<your azure service bus namespace>"
Azure.config.sb_access_key = "<your azure service bus access key>"
```

Määritä nimitilan olet luonut sen sijaan, että koko URL-osoite-arvoa. Käytä esimerkiksi **"yourexamplenamespace"**, ei "yourexamplenamespace.servicebus.windows.net".

## <a name="how-to-create-a-queue"></a>Voit luoda jonon

**Azure::ServiceBusService** objektin avulla voit käsitellä olevien. Voit myös luoda jonon **create_queue()** -menetelmällä. Seuraava esimerkki luo jonon tai virheitä, tulostuvat.

```
azure_service_bus_service = Azure::ServiceBusService.new
begin
  queue = azure_service_bus_service.create_queue("test-queue")
rescue
  puts $!
end
```

Voit myös siirtää **Azure::ServiceBus::Queue** objektin muita vaihtoehtoja, jonka avulla voit ohittaa jonon oletusasetukset, kuten viestin, kun haluat live tai jonon enimmäiskoko. Seuraavassa esimerkissä esitetään määrittämisestä jonon enimmäiskoko 5 gt ja live minuutin ajan:

```
queue = Azure::ServiceBus::Queue.new("test-queue")
queue.max_size_in_megabytes = 5120
queue.default_message_time_to_live = "PT1M"

queue = azure_service_bus_service.create_queue(queue)
```

## <a name="how-to-send-messages-to-a-queue"></a>Voit lähettää viestejä jonossa

Viestin lähettäminen palvelun Bus jono-sovelluksen puhelut **lähettää\_jonossa\_message()** **Azure::ServiceBusService** objektin menetelmää. Viestien lähetetty (ja vastaanotettujen kohteesta)-palvelun Bus olevien **Azure::ServiceBus::BrokeredMessage** objekteja ja on vakio-ominaisuuksia ( **esimerkiksi** ja **aika\_,\_live**), joka sanasto, jota käytetään pidä mukautetun sovelluksen tiettyjen ominaisuuksien ja haluamaansa sovelluksen tiedot. Sovelluksen, voit määrittää viestin tekstiosaan välittämällä merkkijonoarvo viestin ja tarvittavat vakio-ominaisuudet täytetään oletusarvot.

Seuraavassa esimerkissä kerrotaan, miten testi viestin lähettäminen nimeltä "testi-jonossa" käyttämällä jonossa **Lähetä\_jonossa\_message()**:

```
message = Azure::ServiceBus::BrokeredMessage.new("test queue message")
message.correlation_id = "test-correlation-id"
azure_service_bus_service.send_queue_message("test-queue", message)
```

Palvelun Bus olevien tue viestin koko on [Vakio taso](service-bus-premium-messaging.md) 256 Kilotavua ja 1 Megatavu [Premium taso](service-bus-premium-messaging.md). Otsikko, joka sisältää vakio- ja mukautettu-sovelluksen ominaisuudet, voi olla koko on enintään 64 Kilotavua. Ei ole rajoitettu pidetään jonossa viestien määrän, mutta ole pää kokonaiskoko saamisesta jonon viestit. Tämä jono koko on määritetty luominen aikaa, yläraja 5 gigatavua.

## <a name="how-to-receive-messages-from-a-queue"></a>Voit vastaanottaa viestejä jonossa

Viestejä vastaanotetaan jonon avulla **vastaanottaa\_jonon\_message()** **Azure::ServiceBusService** objektin menetelmää. Oletusarvon mukaan viestit lukea ja ilman poistetaan jonossa lukittu. Voit poistaa viestit jonossa, kun ne on luettu määrittämällä **: peek_lock** asetuksen arvoksi **false**.

Oletusarvoisen toimintatavan tekee arvon ja kaksivaiheinen-toimintoa, joka myös, tekee sovellusten, joka ei hyväksy puuttuvat viestit poistetaan. Palvelun Bus saa pyynnön, kun se havaitsee seuraava viesti voi olla kulutettu, lukitsee voit estää muita kuluttajille vastaanottanut sitä ja palaa sovelluksen. Kun sovellus käsittelemisen viestin (tai tallentaa sen luotettavasti myöhempää käsittelyä varten), vastaanota-prosessin toisen vaiheen suorittamisen soittamalla **poistaminen\_jonossa\_message()** menetelmä ja antamisen viestin parametrina poistetaan. **Poistaminen\_jonossa\_message()** menetelmä viestin merkitseminen on käytetty ja poista se jonossa.

Jos **: pikanäkymä\_Lukitse** parametrin arvo on **Epätosi**, lukeminen ja viestin poistaminen on helpoin mallin ja sopii parhaiten skenaariot, jossa sovellus hyväksyt ei käsittele viesti, jos. Tutustu tämän harkitse tilanne, jossa kuluttaja vastaanota pyynnön ja sitten kaatuu, ennen kuin sitä käsitellään. Palvelun Bus olet merkinnyt viestin parhaillaan kulutettu, kun sovellus käynnistyy ja aloittaa muissa viestejä uudelleen, koska se jääneet sanoma, joka on käytetty ennen kaatumisen.

Seuraavassa esimerkissä kerrotaan, miten voi vastaanottaa ja käsitellä viestiä, joissa **vastaanottaa\_jonossa\_message()**. Esimerkki ensin vastaanottaa ja poistaa viestin käyttämällä **: pikanäkymä\_Lukitse** arvoksi **false**, ja valitse se saa toisen viestin ja poistaa viestin käyttämällä **poistaminen\_jonossa\_message()**:

```
message = azure_service_bus_service.receive_queue_message("test-queue",
  { :peek_lock => false })
message = azure_service_bus_service.receive_queue_message("test-queue")
azure_service_bus_service.delete_queue_message(message)
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Sovellus kaatuu ja voi lukea viestien käsittelemisestä

Palvelun Bus on toimintoja, joiden avulla virheet sovelluksessa tai viestin käsittelyn vaikeuksia tilanteen palauttamisessa. Jos vastaanottaja-sovellus ei voi käsitellä viestiä jostain syystä ja valitse se voi soittaa **lukituksen\_jonon\_message()** **Azure::ServiceBusService** objektin menetelmää. Tämä aiheuttaa palvelun Bus jonossa viestin lukituksen ja vastaanottaa uudelleen samaan vievää sovelluksen tai jonkin muun vievää sovelluksen käytettäväksi.

On myös lukittu jonossa viestin liittyvät aikakatkaisu ja jos sovellus lakkaa käsitellä viestiä ennen lukituksen aikakatkaisu vanhenee (esimerkiksi, jos sovellus kaatuu)-palvelun Bus lukitus viestin automaattisesti ja mahdollistaa sen käytön vastaanotettu uudelleen.

Silloin, kun sovellus kaatuu, kun viestin käsittelyn mutta ennen kuin **poistaa\_jonossa\_message()** menetelmää kutsutaan ja sitten viesti on toimitettu edelleen sovelluksen uudelleenkäynnistyksen yhteydessä. Tätä kutsutaan usein **Vähintään kerran käsittely**; jokaisen viestin käsitellään vähintään kerran, mutta tietyissä tilanteissa sama viesti saattaa olla toimitettu edelleen. Jos skenaariota ei hyväksy kaksoiskappaleiden käsittely, sitten sovelluskehittäjät Lisää muita logiikan niiden sovelluksen käsittelee kaksoiskappaleita viestin lähettämisen. Tämä usein saavutetaan käyttämällä **viestin\_tunnus** sanoman, joka pysyy vakiona toimitusyritysten koko-ominaisuuden.

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet tutustunut palvelun Bus olevien perusteet, noudata näitä linkkejä lisätietoja.

-   Yleisiä tietoja [olevien, aiheet, ja tilaukset](service-bus-queues-topics-subscriptions.md).
-   Lisätietoja [Azure SDK Ruby](https://github.com/Azure/azure-sdk-for-ruby) -tietovarasto GitHub.

Katso tämän artikkelin ja Azure olevien [käyttämisestä jonon tallennustilaa Ruby](../storage/storage-ruby-how-to-use-queue-storage.md) -artikkelissa kuvatut Azure palvelun Bus olevien vertailu, [Azure olevien ja Azure palvelun Bus olevien - verrattuna ja Contrasted](service-bus-azure-and-service-bus-queues-compared-contrasted.md)
 

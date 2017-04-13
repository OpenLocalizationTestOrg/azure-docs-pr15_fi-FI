<properties 
    pageTitle="Käyttämisestä jonon tallennustilaa Ruby | Microsoft Azure" 
    description="Opettele Azure jonon-palvelun avulla voit luoda ja poistaa olevien, Lisää, Hae ja poistaa viestit. Esimerkkejä, joiden kirjoitettu Ruby." 
    services="storage" 
    documentationCenter="ruby" 
    authors="robinsh" 
    manager="carmonm" 
    editor=""/>

<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ruby" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="robinsh"/>


# <a name="how-to-use-queue-storage-from-ruby"></a>Opi käyttämään jonon tallennustilaa Ruby

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Yleiskatsaus

Tämän oppaan avulla voit suorittaa yleisiä tilanteita, joissa on Microsoft Azure jonon Storage-palvelun avulla. Mallit kirjoitetaan Ruby Azure-Ohjelmointirajapinnan käyttäminen.
Tilanteita, joissa kattaa Sisällytä **lisääminen**, **Vilkaiseminen**, **käytön**ja **poistamalla** niiden sanomien sekä **luominen ja poistaminen olevien**.

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a>Ruby-sovelluksen luominen

Luo Ruby-sovellus. Ohjeita on artikkelissa [Azure-AM kulkevia verkkosovelluksen Ruby](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).

## <a name="configure-your-application-to-access-storage"></a>Voit käyttää tallennustilaa sovelluksen määrittäminen

Käyttämään Azure-tallennustilan, sinun on ladattava ja Ruby azure pakettia, joka sisältää helppokäyttöisyys kirjastoja, joissa viestintä tallennustilan muiden palveluiden kanssa.

### <a name="use-rubygems-to-obtain-the-package"></a>Hanki paketin RubyGems avulla

1. Käytä käyttöliittymä esimerkiksi **PowerShell** (Windows), **terminaalissa** (Mac) tai **Bash** (Unix).

2. Kirjoita "gem Asenna azure" Asenna gem ja riippuvuuksien komentoikkunassa.

### <a name="import-the-package"></a>Tuo pakkaaminen

Käyttää tuttuja tekstieditorissa, Lisää seuraava teksti Ruby tiedoston, jos aiot käyttää tallennustilan alkuun:

    require "azure"

## <a name="setup-an-azure-storage-connection"></a>Azure-tallennustilan-yhteyden määrittäminen

Azure moduulin lukee ympäristömuuttujat **AZURE\_TALLENNUSTILAN\_tilin** ja **AZURE\_TALLENNUSTILAN\_ACCESS_KEY** , voit muodostaa yhteyden tiliisi Azure tallennustilan tarvittavat tiedot. Jos ympäristössä muuttujia ei määritetä, sinun on määritettävä tilitiedot ennen **Azure::QueueService** käyttäminen seuraava koodi:

    Azure.config.storage_account_name = "<your azure storage account>"
    Azure.config.storage_access_key = "<your Azure storage access key>"

 
Voit hankkia nämä arvot perinteinen tai Resurssienhallinta-tallennustilan tilin Azure-portaalissa:

1. Kirjaudu sisään [Azure portal](https://portal.azure.com).
2. Siirry tallennustilan-tili, jota haluat käyttää.
3. Valitse oikealla asetukset-sivu **Pikanäppäimet**.
4. Accessin näppäimet sivu, joka tulee näkyviin näet pikanäppäin 1 ja 2 pikanäppäin. Voit käyttää jommankumman nimenä. 
5. Kopioi Leikepöydälle avaimen kopio-kuvaketta. 

Voit hankkia nämä arvot perinteinen tallennustilan tililtä perinteinen Azure-portaalissa:

1. Kirjaudu sisään [perinteinen Azure portal](https://manage.windowsazure.com).
2. Siirry tallennustilan-tili, jota haluat käyttää.
3. Valitse siirtymisruudun alareunassa **Hallinta PIKANÄPPÄIMET** .
4. Pop-valintaikkunan näet tallennustilan tilin nimi, pikanäppäin ensisijainen ja toissijainen pikanäppäin. Pikanäppäin voit käyttää joko ensisijainen tai toissijainen tunnus. 
5. Kopioi Leikepöydälle avaimen kopio-kuvaketta.

## <a name="how-to-create-a-queue"></a>Toimintaohje: Jonon luominen

Seuraava koodi luo **Azure::QueueService** -objekti, jonka avulla voit käsitellä olevien.

    azure_queue_service = Azure::QueueService.new

Luo jono samanniminen **create_queue()** -menetelmällä.

    begin
      azure_queue_service.create_queue("test-queue")
    rescue
      puts $!
    end

## <a name="how-to-insert-a-message-into-a-queue"></a>Toimintaohje: Lisääminen viestiin jonossa

Jos haluat lisätä viestiin jonossa, käytä uuden viestin luominen ja lisääminen jonossa **create_message()** -menetelmää.

    azure_queue_service.create_message("test-queue", "test message")

## <a name="how-to-peek-at-the-next-message"></a>Toimintaohje: Vilkaista seuraava viesti

Voit vilkaista jonon edestä suojuksen viestin poistamatta jonossa, soittamalla **pikanäkymä\_messages()** menetelmää. Oletusarvon mukaan **pikanäkymä\_messages()** peeks osoitteessa käyttäminen yhdessä viestissä. Voit myös määrittää valvotun viestit.

    result = azure_queue_service.peek_messages("test-queue",
      {:number_of_messages => 10})

## <a name="how-to-dequeue-the-next-message"></a>Toimintaohje: Jonosta poistamisen seuraava viesti

Voit poistaa viestin jonon seuraavalla tavalla.

1. Kun soitat **luettelon\_messages()**, saat seuraavan viestin jonossa oletusarvoisesti. Voit myös määrittää, kuinka monta viestit haluat nähdä. Palauttamat sanomat **luettelon\_messages()** lukuoikeudet muut viestit luetaan Tämä jono koodi. Voit siirtää näkyvyyden aikakatkaisu sekunteina parametrina.

2. Viestin poistaminen jonossa lopuksi on myös soittaa **delete_message()**.

Poistaa viestin tavalla varmistaa, että kun koodisi ei käsitellä viestiä laitteiston ja ohjelmiston virheen vuoksi, koodisi esiintymässä voi saman sanoma ja yritä uudelleen. Koodi-puhelut **poistaminen\_message()** heti, kun viesti on käsitelty.

    messages = azure_queue_service.list_messages("test-queue", 30)
    azure_queue_service.delete_message("test-queue", 
      messages[0].id, messages[0].pop_receipt)

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Toimintaohje: Jonossa viestin sisällön muuttaminen

Voit muuttaa viestin käytönaikainen jonossa sisällön. Seuraava koodi päivittää viestin käyttämällä **update_message()** -menetelmää. Menetelmä palauttaa monikon, joka sisältää jonon ja UTC-ajan päivämäärä aika-arvon, joka saadaan, kun viesti on näkyvissä jonossa pop vastaanoton.

    message = azure_queue_service.list_messages("test-queue", 30)
    pop_receipt, time_next_visible = azure_queue_service.update_message(
      "test-queue", message.id, message.pop_receipt, "updated test message", 
      30)

## <a name="how-to-additional-options-for-dequeuing-messages"></a>Toimintaohje: Lisäasetusten määrittäminen Dequeuing viestit

Voit mukauttaa viestin hakeminen jonon kahdella tavalla.

1. Saat viestin erän.

2. Voit määrittää pidentää tai lyhentää invisibility aikakatkaisu, sallia koodissa enemmän tai vähemmän aikaa käsitellä täysin jokaisen viestin.

Seuraava koodi esimerkissä käytetään **luettelon\_messages()** menetelmä jotta saat 15 yhden käynnissä. Valitse tulostaa ja poistaa jokaisen viestin. Se myös asettaa invisibility aikakatkaisu kustakin viestistä viisi minuuttia.

    azure_queue_service.list_messages("test-queue", 300
      {:number_of_messages => 15}).each do |m|
      puts m.message_text
      azure_queue_service.delete_message("test-queue", m.id, m.pop_receipt)
    end

## <a name="how-to-get-the-queue-length"></a>Toimintaohje: Hae jonon pituuden

Saat jonossa arvio viestien määrä. **Hae\_jonossa\_metadata()** menetelmä pyytää jonon-palvelu palauttaa lähes viestin Laske- ja jonossa metatietoa.

    message_count, metadata = azure_queue_service.get_queue_metadata(
      "test-queue")

## <a name="how-to-delete-a-queue"></a>Toimintaohje: Poista jonossa

Poista jono, ja kaikki sen sisältävät viestit, soita **poistaminen\_queue()** jono-objekti-menetelmä.

    azure_queue_service.delete_queue("test-queue")

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet tutustunut jonon tallennustilan perusteet, näistä linkeistä saat lisätietoja tallennustilan monimutkaisia tehtäviä.

- [Azure-tallennustilan tiimin blogia](http://blogs.msdn.com/b/windowsazurestorage/)
- Vieraile GitHub [Azure SDK Ruby](https://github.com/WindowsAzure/azure-sdk-for-ruby) -säilöön

Katso tämän artikkelin ja Azure-palvelu Bus olevien [käyttämisestä palvelun Bus olevien](/develop/ruby/how-to-guides/service-bus-queues/) artikkelissa kuvatut Azure jonon palvelun vertailu, [Azure olevien ja Bus olevien - verrattuna ja Contrasted](../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md)
 

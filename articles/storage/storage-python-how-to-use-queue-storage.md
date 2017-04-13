<properties
    pageTitle="Käyttämisestä jonon tallennustilaa Python | Microsoft Azure"
    description="Opettele käyttämään Python Azure jonon huolto luominen ja poistaminen olevien, Lisää, Hae ja poistaa viestit."
    services="storage"
    documentationCenter="python"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="robinsh"/>

# <a name="how-to-use-queue-storage-from-python"></a>Opi käyttämään jono tallennustilaa Python

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Yleiskatsaus

Tämän oppaan avulla voit suorittaa yleisiä tilanteita, joissa Azure jonon tallennustilan-palvelun avulla. Mallit on kirjoitettu Python ja käytä [Microsoft Azure tallennustilan SDK Python]. Tilanteita, joissa kattaa Sisällytä **lisääminen**, **Vilkaiseminen**, **käytön**ja **poistamalla** niiden sanomien sekä **luominen ja poistaminen olevien**. Lisätietoja olevien viitata [vaiheisiin]-osassa.

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="how-to-create-a-queue"></a>Toimintaohje: Jonon luominen

**QueueService** objektin voit käsitellä olevien. Seuraava koodi luo **QueueService** objektin. Lisää minkä tahansa Python tiedoston, jossa haluat käyttää ohjelmallisesti Azuren tallennustilaan yläosassa seuraavasti:

    from azure.storage.queue import QueueService

Seuraava koodi luo **QueueService** objektin tallennustilan tilin nimi ja avaimen avulla. Korvaa 'myaccount' ja 'OmaAvain' tilin nimi ja avaimen avulla.

    queue_service = QueueService(account_name='myaccount', account_key='mykey')

    queue_service.create_queue('taskqueue')


## <a name="how-to-insert-a-message-into-a-queue"></a>Toimintaohje: Lisääminen viestiin jonossa

Jos haluat lisätä viestiin jonossa, käytä **valitseminen\_viestin** menetelmä, jolla uuden viestin luominen ja lisääminen jonossa.

    queue_service.put_message('taskqueue', u'Hello World')


## <a name="how-to-peek-at-the-next-message"></a>Toimintaohje: Vilkaista seuraava viesti

Voit vilkaista jonon edestä suojuksen viestin poistamatta jonossa, soittamalla **pikanäkymä\_viestien** menetelmää. Oletusarvon mukaan **pikanäkymä\_viestien** peeks osoitteessa käyttäminen yhdessä viestissä.

    messages = queue_service.peek_messages('taskqueue')
    for message in messages:
        print(message.content)


## <a name="how-to-dequeue-messages"></a>Toimintaohje: Jonosta poistamisen viestit

Koodin poistaa viestin jonon seuraavalla tavalla. Kun soitat **Hae\_viestien**, saat seuraavan viestin jonossa oletusarvoisesti. Viestin palauttamien **Hae\_viestien** lukuoikeudet muut viestit luetaan Tämä jono koodi. Oletusarvoisesti tämä viesti säilyy näkymätön 30 sekuntia. Viimeistele poistaminen jonossa viestin, sinun on myös kutsuttava **poistaminen\_viestin**. Poistaa viestin tavalla varmistaa, että kun koodisi ei käsitellä viestiä laitteiston ja ohjelmiston virheen vuoksi, koodisi esiintymässä voi saman sanoma ja yritä uudelleen. Koodi-puhelut **poistaminen\_viestin** heti, kun viesti on käsitelty.

    messages = queue_service.get_messages('taskqueue')
    for message in messages:
        print(message.content)
        queue_service.delete_message('taskqueue', message.id, message.pop_receipt)

Voit mukauttaa viestin hakeminen jonon kahdella tavalla.
Saat ensin viestien (enintään 32). Se voit määrittää pidentää tai lyhentää invisibility aikakatkaisu, sallia koodissa enemmän tai vähemmän aikaa käsitellä täysin jokaisen viestin. Seuraava koodi esimerkissä käytetään **Hae\_viestien** menetelmä jotta saat ilmoituksen 16 yhden käynnissä. Valitse käsittelee jokaisen viestin käyttämällä silmukan varten. Se myös asettaa invisibility aikakatkaisu kustakin viestistä viisi minuuttia.

    messages = queue_service.get_messages('taskqueue', num_messages=16, visibility_timeout=5*60)
    for message in messages:
        print(message.content)
        queue_service.delete_message('taskqueue', message.id, message.pop_receipt)      


## <a name="how-to-change-the-contents-of-a-queued-message"></a>Toimintaohje: Jonossa viestin sisällön muuttaminen

Voit muuttaa viestin käytönaikainen jonossa sisällön. Jos viesti on työtehtävä, työtehtävän tilan päivittäminen voi käyttää tätä toimintoa. Käyttää alla koodi **päivittää\_viestin** tapaa päivittää viestiin. Näkyvyyden aikakatkaisu asetetaan 0, mikä tarkoittaa, näyttöön tulee välittömästi ja sisältö päivitetään.

    messages = queue_service.get_messages('taskqueue')
    for message in messages:
        queue_service.update_message('taskqueue', message.id, message.pop_receipt, 0, u'Hello World Again')

## <a name="how-to-get-the-queue-length"></a>Toimintaohje: Hae jonon pituuden

Saat jonossa arvio viestien määrä. **Hae\_jono\_metatietojen** menetelmä pyytää jonopalvelu palauttaa metatietoa jono ja **approximate_message_count**. Tulos on vain lähes, koska viestien voi lisätä tai poistaa sen jälkeen, kun jonopalvelu vastaa lähettämäsi pyynnön.

    metadata = queue_service.get_queue_metadata('taskqueue')
    count = metadata.approximate_message_count

## <a name="how-to-delete-a-queue"></a>Toimintaohje: Poista jono

Poista jono, ja kaikki sen sisältävät viestit, soita **poistaminen\_jonon** menetelmä.

    queue_service.delete_queue('taskqueue')

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet tutustunut jonon tallennustilan perusteet, noudata näitä linkkejä lisätietoja.

- [Python Developer Center](/develop/python/)
- [Azure liittyviä palveluja REST-Ohjelmointirajapinnalla](http://msdn.microsoft.com/library/azure/dd179355)
- [Azure-tallennustilan tiimin blogia]
- [Microsoft Azuren tallennustilaan Python SDK]

[Azure-tallennustilan tiimin blogia]: http://blogs.msdn.com/b/windowsazurestorage/
[Microsoft Azuren tallennustilaan Python SDK]: https://github.com/Azure/azure-storage-python

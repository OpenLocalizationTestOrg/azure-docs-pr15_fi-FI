<properties 
    pageTitle="Kirjautuminen tapahtumien Azure tapahtuman porttiin Azure API hallinnan | Microsoft Azure" 
    description="Opettele kirjautumaan tapahtumien Azure tapahtuman porttiin Azure API hallinta." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-log-events-to-azure-event-hubs-in-azure-api-management"></a>Kuinka kirjaudun tapahtumien Azure tapahtuman porttiin Azure API hallinta

Azure tapahtuman keskittimet on erittäin skaalattava tietojen tunkeutumisen palvelu, jota voit ingest tapahtumat sekunnissa miljoonia niin, että voit käsitellä ja analysoida yhdistetyn laitteita ja sovelluksia tuottamat tiedot mahdutettavia. Tapahtuman keskittimet toimii "eteen oven" tapahtuman myyntijakso varten, ja kun tiedot on kerätty tapahtuma-keskittimeen, voidaan muuntaa ja tallentaa minkä tahansa reaaliaikainen analytics-palveluntarjoajan tai jonottaminen ja tallennustilaa sovittimet. Tapahtuman keskittimet decouples stream näitä tapahtumia kulutus tapahtumia tuotannon niin, että tapahtuman kuluttajille käyttää omia aikataulussa tapahtumat.

Tässä artikkelissa on avustaja [Integroida tapahtuman keskittimet Azure API hallinta](https://azure.microsoft.com/documentation/videos/integrate-azure-api-management-with-event-hubs/) videoon ja kerrotaan, miten voit kirjautua käyttämällä Azure tapahtuman keskittimet API hallinta tapahtumat.

## <a name="create-an-azure-event-hub"></a>Luo Azure tapahtumaa-toiminnossa

Luo uusi tapahtuma-toiminnossa kirjautuminen [Azure perinteinen portaaliin](https://manage.windowsazure.com) ja valitse **Uusi**->**Sovelluksen Services**->**Palvelun Bus**->**Tapahtumaa-toiminnossa**->**Nopea luominen**. Kirjoita tapahtuman keskittimeen, nimi alue, valitse tilaus ja valitse nimitila. Jos et ole aiemmin luonut nimitilan, voit luoda sen kirjoittamalla nimen **Namespace** -tekstiruutuun. Kun kaikki ominaisuudet on määritetty, valitse **Luo uusi tapahtuma-toiminnossa** voit luoda tapahtumaa-toiminnossa.

![Luo tapahtumaa-toiminnossa][create-event-hub]

Seuraavaksi Siirry **määritys** -välilehti, uusi tapahtuma-toiminnosta ja luo kaksi **jaettuun käyttöön käytännöt**. Nimeä ensin yhden **lähettäminen** ja antaa sille **Lähetä** käyttöoikeudet.

![Käytännön lähettäminen][sending-policy]

Nimeä toinen **vastaanottaminen**, antaa sille **kuunnella** käyttöoikeudet ja valitse **Tallenna**.

![Käytännön vastaanottaminen][receiving-policy]

Kunkin jaetun access-käytännön avulla voit lähettää ja vastaanottaa tapahtumia, ja valitse tapahtumaa-toiminnossa sovellukset. Käyttämään käytännöt yhteyden merkkijonojen Siirry tapahtumaa-toiminnossa **koontinäyttö** -välilehti ja valitse **yhteyden tietoja**.

![Yhteysmerkkijono][event-hub-dashboard]

**Lähettäminen** yhteysmerkkijonon käytetään, kun tapahtumien kirjaamisen ja **Vastaanota** -yhteysmerkkijonon käytetään, kun lataamalla tapahtumien tapahtumaa-toiminnossa.

![Yhteysmerkkijono][event-hub-connection-string]

## <a name="create-an-api-management-logger"></a>Luo API hallinta lokin

Nyt kun olet luonut tapahtumaa-toiminnossa, seuraava vaihe on [lokin](https://msdn.microsoft.com/library/azure/mt592020.aspx) paikallaan API hallintapalvelun, niin, että se voi kirjautua tapahtumien tapahtumaa-toiminnossa.

API hallinta tallentajia määritetään [API hallinta REST API](http://aka.ms/smapi). Ennen kuin käytät REST-Ohjelmointirajapinnalla ensimmäistä kertaa, tarkista [edellytykset](https://msdn.microsoft.com/library/azure/dn776326.aspx#Prerequisites) ja varmistaa, että sinulla on [pääsy REST-Ohjelmointirajapinnalla käytössä](https://msdn.microsoft.com/library/azure/dn776326.aspx#EnableRESTAPI).

Luomiseen lokin Varmista HTTP valitseminen pyynnön, käyttämällä seuraavaa URL-mallia.

    https://{your service}.management.azure-api.net/loggers/{new logger name}?api-version=2014-02-14-preview

-   Korvaa `{your service}` API Management-palvelun esiintymän nimi.
-   Korvaa `{new logger name}` uuden lokin haluamasi nimet. Viittaat nimi, kun [log eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) -käytännön määrittäminen

Lisää seuraavat otsikot pyynnön.

-   Sisältötyypin: sovelluksen/json
-   Todennus: SharedAccessSignature uid =...
    -   Lisätietoja luodaan `SharedAccessSignature` kohdassa [Azure API hallinta REST API tarkistus](https://msdn.microsoft.com/library/azure/dn798668.aspx).

Määritä pyyntö jotakin seuraavista mallin avulla.

    {
      "type" : "AzureEventHub",
      "description" : "Sample logger description",
      "credentials" : {
        "name" : "Name of the Event Hub from the Azure Classic Portal",
        "connectionString" : "Endpoint=Event Hub Sender connection string"
        }
    }

-   `type`määrityksen on oltava `AzureEventHub`.
-   `description`valinnainen kuvaus tapahtumalokin ja voi olla nolla merkkijono tarvittaessa.
-   `credentials`sisältää `name` ja `connectionString` , Azure tapahtumaa-toiminnossa.

Kun teet pyynnön, jos lokin luodaan tilakoodin `201 Created` palautetaan. 

>[AZURE.NOTE] Muut mahdolliset palautuksen koodit ja niiden syistä on artikkelissa [Create lokin](https://msdn.microsoft.com/library/azure/mt592020.aspx#PUT). Jos haluat nähdä kuinka suorittaa muita toimintoja, kuten luettelon, Päivitä ja poista on [lokin](https://msdn.microsoft.com/library/azure/mt592020.aspx) kohteen ohjeissa.

## <a name="configure-log-to-eventhubs-policies"></a>Lokitiedoston eventhubs käytäntöjen määrittäminen

Kun oman lokin on määritetty API hallinnassa, voit määrittää lokin eventhubs-käytännöt Kirjaudu haluamasi tapahtumat. Lokitiedoston eventhubs käytäntö voidaan saapuva käytäntö-osassa tai lähtevä käytäntö-osassa.

Voit määrittää käytäntöjä kirjautuminen [Azure perinteinen portal](https://manage.windowsazure.com)siirtyminen API hallintapalvelun ja **publisher-portaalin** tai publisher-portaalin **hallinta** .

![Publisher-portaalissa][publisher-portal]

Napsauta **käytäntöjen** vasemmalla API hallinta-valikossa, valitse haluamasi tuote- ja API ja valitse **Lisää käytännön**. Tässä esimerkissä on olet lisäämässä käytännön **Rajoittamaton** tuotteen **Kaiku API** .

![Lisää käytäntö][add-policy]

Aseta kohdistin `inbound` käytännön osassa ja valitse Lisää **lokin EventHub** -käytäntö `log-to-eventhub` käytännön tiliotemalliin.

![Käytännön-editori][event-hub-policy]

    <log-to-eventhub logger-id ='logger-id'>
      @( string.Join(",", DateTime.UtcNow, context.Deployment.ServiceName, context.RequestId, context.Request.IpAddress, context.Operation.Name))
    </log-to-eventhub>

Korvaa `logger-id` API hallinta-lokin edellisessä vaiheessa määritetty nimi.

Voit käyttää lauseke, joka palauttaa merkkijonon arvona `log-to-eventhub` elementti. Tässä esimerkissä on kirjautunut merkkijonon, joka sisältää päivämäärän ja kellonajan, palvelunimi, pyynnön tunnus, pyynnön ip-osoite ja toiminnon nimi.

Valitse **Tallenna** Tallenna päivitetty käytännön määrittämisessä. Heti, kun se on tallennettu käytäntö on käytössä ja tapahtumat kirjataan nimettyjen tapahtumaa-toiminnossa.

## <a name="next-steps"></a>Seuraavat vaiheet

-   Lisätietoja Azure tapahtuman keskittimet
    -   [Azure tapahtuman keskittimet käytön aloittaminen](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
    -   [Vastaanottamaan viestejä EventProcessorHost](../event-hubs/event-hubs-csharp-ephcs-getstarted.md#receive-messages-with-eventprocessorhost)
    -   [Tapahtuman keskittimet ohjelmointi opas](../event-hubs/event-hubs-programming-guide.md)
-   Lisätietoja API hallinta ja tapahtuman keskittimet integrointi
    -   [Lokin kohteesta](https://msdn.microsoft.com/library/azure/mt592020.aspx)
    -   [lokitiedoston eventhub käytännön viitata](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub)
    -   [Oman ohjelmointirajapinnan Azure API hallinta, tapahtuman keskittimet ja Runscope valvonta](api-management-log-to-eventhub-sample.md)    

## <a name="watch-a-video-walkthrough"></a>Katso video vaiheittainen

> [AZURE.VIDEO integrate-azure-api-management-with-event-hubs]


[publisher-portal]: ./media/api-management-howto-log-event-hubs/publisher-portal.png
[create-event-hub]: ./media/api-management-howto-log-event-hubs/create-event-hub.png
[event-hub-connection-string]: ./media/api-management-howto-log-event-hubs/event-hub-connection-string.png
[event-hub-dashboard]: ./media/api-management-howto-log-event-hubs/event-hub-dashboard.png
[receiving-policy]: ./media/api-management-howto-log-event-hubs/receiving-policy.png
[sending-policy]: ./media/api-management-howto-log-event-hubs/sending-policy.png
[event-hub-policy]: ./media/api-management-howto-log-event-hubs/event-hub-policy.png
[add-policy]: ./media/api-management-howto-log-event-hubs/add-policy.png







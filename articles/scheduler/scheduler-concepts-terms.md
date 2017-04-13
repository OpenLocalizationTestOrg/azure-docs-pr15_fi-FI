<properties
 pageTitle="Ajoitus-käsitteistä, ehdot ja kohteet | Microsoft Azure"
 description="Azure ajoitus-käsitteistä, terminologiaan ja kohteen hierarkian, kuten töiden ja työn sivustokokoelmat.  Näyttää ajoitetun työn täydellinen esimerkki."
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="get-started-article"
 ms.date="08/18/2016"
 ms.author="deli"/>

# <a name="scheduler-concepts-terminology--entity-hierarchy"></a>Ajoitus-käsitteistä, termejä + kohteen hierarkia

## <a name="scheduler-entity-hierarchy"></a>Ajoituksen kohteen hierarkia

Seuraavassa taulukossa on kuvattu tärkeimmät resursseja tarjoamia tai käyttää ajoitus-Ohjelmointirajapinnan:

|Resurssi | Kuvaus |
|---|---|
|**Työn sivustokokoelman**|Työn sivustokokoelman sisältää ryhmän töiden ja ylläpitää asetuksia, kiintiön ja ylikuormitustilan, jotka on jaettu töiden kokoelmassa. Työn sivustokokoelman luodaan tilauksen omistaja ja ryhmittelee töitä yhdessä käyttö- tai sovelluksen rajat perusteella. Se on rajoitettu yhden alueen. Sen avulla myös kiintiön rajoittaa kyseisen sivustokokoelman kaikki projektien käyttö käyttäminen. Kiintiöt ovat MaxJobs ja MaxRecurrence.|
|**Työn**|Työn määrittää yksittäisen toistuva toiminto, ja yksinkertaisen tai monimutkaisen strategioita suorittamisen. Toiminnot voi sisältää, HTTP, tallennustilan jonossa, palvelun bus jonossa ja bus aiheen palvelupyyntöjä.|
|**Työhistoria**|Työn historiatiedot edustaa työn suorituksen tiedot. Onnistunut virheen sekä vastauksen kaikki tiedot, ja se sisältää.|

## <a name="scheduler-entity-management"></a>Ajoituksen kohteen hallinta

Korkean tason päivitysyrityksen ja service management API näyttää resurssien seuraavat toimenpiteet:

|Ominaisuus|Kuvaus ja URI-osoite|
|---|---|
|**Työn sivustokokoelman hallinta**|HAE, SIJOITA ja poista tuki luomiseen ja muokkaamiseen työn sivustokokoelmat ja sisältämien työt. Työn sivustokokoelman on säilö töiden ja kartat kiintiöitä ja jaettuja asetuksia. Esimerkkejä kiintiöt, joka on kuvattu myöhemmin, työt ja pienin toistovälin enimmäismäärä. <p>SIJOITA ja poista:`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}`</p><p>HAKEMINEN:`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}`</p>
|**Töiden hallinta**|HAE, SIJOITA, viestin, KORJAUSTIEDOSTON ja poista tuki luomiseen ja muokkaamiseen työt. Kaikki työt on kuuluttava työn kokoelma, joka on jo niin ei ole implisiittinen luominen. <p>`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}/jobs/{jobName}`</p>|
|**Työn historia hallinta**|OTTAA yhteyttä tukeen haetaan 60 päivän suorittamisen Työhistoria, kuten projektin kulunut aika ja suorittamisen seurauksena. Lisää kyselymerkkijonon parametrin tuen suodattaminen tilan ja tilan perusteella. <P>`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}/jobs/{jobName}/history`</p>|

## <a name="job-types"></a>Töiden tyypit

Useita eri työt: HTTP-projektit (mukaan lukien HTTPS-työt, jotka tukevat SSL), tallennustilan jonossa olevat työt, palvelun bus jonossa olevat työt ja palvelun bus aiheen työt. HTTP-työt ovat ihanteellinen, jos sinulla on aiemmin luotuun työmäärää tai palvelun seinän päätepiste. Voit lähettää viestejä tallennustilan olevien, jotta näitä töitä soveltuvat erinomaisesti toiminnoista, jotka käyttävät tallennustilan olevien tallennustilan jonossa olevat työt. Vastaavasti palvelun bus työt soveltuvat erinomaisesti toiminnoista, jotka käyttävät palvelun bus olevien ja ohjeita.

## <a name="the-job-entity-in-detail"></a>"Työ"-kohteen yksityiskohtaiset tiedot

Tavallinen tasolla ajoitetun työn on useita osia:

- Toiminto, joka suoritetaan, kun työ ajastin käynnistyy  

- (Valinnainen) Työn aika  

- (Valinnainen) Milloin ja miten usein Toista työ  

- (Valinnainen) Toiminnon Kyselysäännön, jos ensisijainen toimenpide epäonnistuu  

Sisäisesti ajoitetun työn sisältää myös järjestelmän mukana tietoja, kuten seuraava ajoitettu suoritusaika.

Seuraava koodi on täydellinen esimerkki ajoitetun työn. Seuraavissa osissa annetaan tiedot.

    {
        "startTime": "2012-08-04T00:00Z",               // optional
        "action":
        {
            "type": "http",
            "retryPolicy": { "retryType":"none" },
            "request":
            {
                "uri": "http://contoso.com/foo",        // required
                "method": "PUT",                        // required
                "body": "Posting from a timer",         // optional
                "headers":                              // optional

                {
                    "Content-Type": "application/json"
                },
            },
           "errorAction":
           {
               "type": "http",
               "request":
               {
                   "uri": "http://contoso.com/notifyError",
                   "method": "POST",
               },
           },
        },
        "recurrence":                                   // optional
        {
            "frequency": "week",                        // can be "year" "month" "day" "week" "minute"
            "interval": 1,                              // optional, how often to fire (default to 1)
            "schedule":                                 // optional (advanced scheduling specifics)
            {
                "weekDays": ["monday", "wednesday", "friday"],
                "hours": [10, 22]
            },
            "count": 10,                                 // optional (default to recur infinitely)
            "endTime": "2012-11-04",                     // optional (default to recur infinitely)
        },
        "state": "disabled",                           // enabled or disabled
        "status":                                       // controlled by Scheduler service
        {
            "lastExecutionTime": "2007-03-01T13:00:00Z",
            "nextExecutionTime": "2007-03-01T14:00:00Z ",
            "executionCount": 3,
                                                "failureCount": 0,
                                                "faultedCount": 0
        },
    }

Esimerkki ajoitetun työn yllä tarkastelu työn määrityksen on useita osia:

- Aloitusaika ("aloitusajan")  

- Toiminnon ("toiminto"), joka sisältää toiminnon virhe ("errorAction")

- Toistuminen ("toistuminen")  

- Tila ("tila")  

- Tila ("tila")  

- Yritä käytännön ("retryPolicy")  

Tarkastellaan nyt kaikkien näiden yksityiskohtaiset tiedot:

## <a name="starttime"></a>Aloitusajan

"Aloitusajan" aloitusaika ja sallii soittajan määrittää aikavyöhykkeen poikkeama koneisiin [ISO 8601](http://en.wikipedia.org/wiki/ISO_8601)-muodossa.

## <a name="action-and-erroraction"></a>toiminto ja errorAction

"Toiminto" toiminnon kutsua jokaiselle esiintymälle on ja kerrotaan palvelun käynnistäminen tyyppi. Toiminto on mitä suoritetaan annettu aikataulun mukaisesti. Ajoitus tukee HTTP, tallennustilan jonon palvelun bus aiheen tai palvelun bus jonon toiminnot.

Toimi yllä olevassa esimerkissä on HTTP-toiminnon. Alla on esimerkki tallennustilan jonon-toiminto:

    {
            "type": "storageQueue",
            "queueMessage":
            {
                "storageAccount": "myStorageAccount",  // required
                "queueName": "myqueue",                // required
                "sasToken": "TOKEN",                   // required
                "message":                             // required
                    "My message body",
            },
    }

Alla on esimerkki palvelun bus aihe-toiminto.

  "toiminto": {"tyyppi": "serviceBusTopic", "serviceBusTopicMessage": {"topicPath": "t1"  
      "nimitilan": "mySBNamespace", "transportType": "netMessaging" / / voi olla netMessaging tai AMQP "todennus": {"sasKeyName": "QPolicy", "tyyppi": "sharedAccessKey"}, "sanoma": "Viesti", "brokeredMessageProperties": {}, "customMessageProperties": {"appname": "FromScheduler"}},}

Alla on esimerkki palvelun bus jonon-toiminto:


  "toiminto": {"serviceBusQueueMessage": {"queueName": "q1"  
      "nimitilan": "mySBNamespace", "transportType": "netMessaging" / / voi olla netMessaging tai AMQP "todennus": {}  
        "sasKeyName": "QPolicy", "tyyppi": "sharedAccessKey"}, "sanoma": "Viesti",  
      "brokeredMessageProperties": {}, "customMessageProperties": {"appname": "FromScheduler"}}, "tyyppi": "serviceBusQueue"}

"errorAction" on Virheenkäsittely-toiminto käynnistyy, kun ensisijainen toimenpide epäonnistuu. Voit käyttää tätä muuttujaa Virheenkäsittely-päätepisteen tai lähettää käyttäjälle ilmoituksen. Tämä voi käyttää kurottavat toissijainen päätepisteen siinä tapauksessa, että ensisijainen ei ole käytettävissä (esimerkiksi kyseessä päätepisteen sivustossa huono) tai voi käyttää ilmoituksen virheen käsittely päätepiste. Ensisijainen toimenpide, kuten virhe-toimintoa voi olla yksinkertainen tai koosteen logiikan muiden toimien mukaan. Opit luomaan SAS-tunnuksen, viitata [jaettu Access-allekirjoituksen luominen ja käyttäminen](https://msdn.microsoft.com/library/azure/jj721951.aspx).

## <a name="recurrence"></a>toistuvat tapahtumat

Toistuva tapahtuma on useita osia:

- Korkojakso: Minuutti, tunti, päivä, viikko, kuukausi, vuosi, yksi  

- Aikaväli: Aikaväli toistuminen annetun taajuus-palvelussa  

- Määrätyn aikataulun: Määritä minuutteina, tunnit, viikonpäivät, kuukauden ja monthdays toiston  

- Määrä: Esiintymien määrän  

- Päättymisaika: töitä suoritetaan määritetyn päättymisajan jälkeen  

Työn on toistuva, jos se on määritetty JSON sen määritelmän toistuvan objekti. Jos Laske- ja lopetusaika on määritetty, valmistuminen sääntö, joka toteutuu ensin on käyttää.

## <a name="state"></a>tila

Työn tila on jokin neljästä arvoista: käytössä, poissa käytöstä, valmis tai virhe. Voit SIJOITTAA tai KORJAUSTIEDOSTON työt siten, että ne on päivitettävä käytössä tai ei käytössä-tilassa. Jos työ on valmis tai virhe, joka on Lopputila, joita ei voi päivittää (vaikka työn edelleen voi poistaa). Esimerkki state-ominaisuus on seuraavanlainen:


        "state": "disabled", // enabled, disabled, completed, or faulted
Valmiit ja kohtasi virheen työt poistetaan 60 päivän jälkeen.

## <a name="status"></a>tila

Kun ajoitus-työ on alkanut, tiedot palautetaan työn nykyisestä tilasta. Tätä objektia ei voi määrittää käyttäjän, se on määritetty järjestelmä. Kuitenkin se sisältyy työn (erillinen linkitetyn resurssin sijaan) niin, että jokin pystyisivät projektin tila helposti.

Tilan sisältää ajan edellisen suorittamisen (jos saatavilla), seuraava ajoitettu (keskeneräiset työt) suorittamisen aikana suorittamisen työn määrää.

## <a name="retrypolicy"></a>retryPolicy

Jos ajoitus-työ epäonnistuu, on mahdollista määrittää uudelleen käytännön määrittämiseen ja miten toiminnon uudelleen. Tämä määritetään **retryType** -objekti, se on määritetty **ei mitään** käytäntöä ei ole uudelleen, jos yllä esitetyllä tavalla. Aseta sen arvoksi **Kiinteä** , onko uudelleen käytännön.

Voit määrittää uudelleen käytännön kaksi lisäasetuksia voidaan määrittää: (**retryInterval**) väli ja uudelleenyritykset (**retryCount**) määrä.

Yritä määritetyn ajan kuluttua, **retryInterval** , objekti on aikaväliä. Sen oletusarvo on 30 sekuntia, sen määritettäviä vähimmäisarvo on 15 sekunnin kuluttua ja sen suurin arvo on 18 kuukauden. Vapaa työn sivustokokoelmat työt on määritettävä pienin arvo on 1 tunti.  Se on määritetty ISO 8601-muodossa. Vastaavasti uudelleenyritykset numeron arvo on määritetty **retryCount** objektia. kuinka monta kertaa yritä yritetään on. Sen oletusarvo on 4 ja sen suurin arvo on 20\. sekä **retryInterval** ja **retryCount** ovat valinnaisia. Käyttäjät saavat niiden oletusarvoisen arvoja, jos **retryType** on määritetty **Kiinteä** ja ei-arvoja on määritetty erikseen.

## <a name="see-also"></a>Katso myös

 [Ajoituksen ominaisuudet](scheduler-intro.md)

 [Aloita käyttö ajoituksen Azure-portaalissa](scheduler-get-started-portal.md)

 [Suunnitelmien ja Azure ajoituksen Laskutus](scheduler-plans-billing.md)

 [Miten voit luoda monimutkaisia aikatauluja ja Lisäasetukset toistuminen Azure schedulerilla](scheduler-advanced-complexity.md)

 [Azure ajoituksen REST API-viittaus](https://msdn.microsoft.com/library/mt629143)

 [Azure ajoituksen PowerShell cmdlet-viittaus](scheduler-powershell-reference.md)

 [Azure ajoituksen suuren käytettävyyden ja luotettavuutta](scheduler-high-availability-reliability.md)

 [Azure ajoituksen rajoitukset, oletusarvot ja virhekoodit](scheduler-limits-defaults-errors.md)

 [Azure ajoituksen lähtevien yhteyksien todennusta](scheduler-outbound-authentication.md)

<properties
   pageTitle="Azure erä Diagnostiikan kirjaus | Microsoft Azure"
   description="Tallenna ja analysoida vianmäärityslokeihin tapahtumien Azure erä tilin resurssien kuten jakavat ja tehtäviä."
   services="batch"
   documentationCenter=""
   authors="mmacy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="batch"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="big-compute"
   ms.date="10/12/2016"
   ms.author="marsma"/>

# <a name="azure-batch-diagnostic-logging"></a>Azure erä Diagnostiikan kirjaus

Monta Azure Services-palveluiden avulla erä palvelun tietokoneesta kuuluu äänimerkki tapahtumien poistaminen tiettyjen resurssien resurssin elinkaaren aikana. Voit ottaa Azure erä vianmäärityslokit tietueen tapahtumia, esimerkiksi jakavat ja tehtävät ja käyttää lokit diagnostiikan arviointia ja seurantaa. Tapahtumat, kuten resurssivarannon luominen, resurssivarantoon Poista, tehtävän alku tai tehtävä valmis sisältyvät erä vianmäärityslokit.

>[AZURE.NOTE] Tässä artikkelissa käsitellään erä tilin resurssien itse tapahtumien kirjaamisen, ei projektin ja tehtävän siirtää tietoja. Lisätietoja tallentamista töiden ja tehtävien tiedot on artikkelissa [jatkuvat Azure erän työn ja tehtävän tulos](batch-task-output.md).

## <a name="prerequisites"></a>Edellytykset

* [Azure erä-tili](batch-account-create-portal.md)

* [Azure-tallennustilan tilin](../storage/storage-create-storage-account.md#create-a-storage-account)

  Poistu erä vianmäärityslokit, sinun on luotava mihin Azure tallentaa lokit Azure-tallennustilan tilin. Voit määrittää tallennustilan tilin kun otat erä tilin [Diagnostiikan kirjaus käyttöön](#enable-diagnostic-logging) . Voit määrittää, kun otat log-sivustokokoelman tallennustilan tilin ei ole sama kuin [sovelluksen pakettien](batch-application-packages.md) ja [tehtävän tulosteen pysyvyyttä](batch-task-output.md) artikkeleissa tarkoitetulle linkitetyn tallennustilan-tilille.

  >[AZURE.WARNING] Olet **veloitetaan** tilisi Azuren tallennustilaan tallennettuja tietoja. Tämä sisältää tässä artikkelissa kuvatut vianmäärityslokit. Muista tämä mielessä, kun suunnitteleminen [lokin säilytyskäytännön](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md).

## <a name="enable-diagnostic-logging"></a>Ota Diagnostiikan kirjaus

Diagnostiikan kirjaus ei ole käytössä oletusarvoisesti erä tilissäsi. Sinun on otettava käyttöön Diagnostiikan kirjaus haluat valvoa erä jokaiselle tilille erikseen:

[Miten voit ottaa käyttöön kokoelman vianmäärityslokit](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-diagnostic-logs)

On suositeltavaa lukea koko [Azure vianmäärityslokit yleiskatsaus](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) artikkeli, ja myönnä hallitsemista ei vain, miten eri Azure services tue käyttöön kirjaaminen, mutta lokin luokat. Esimerkiksi Azure erä tukee tällä hetkellä yhden lokin luokan: **Lokit**.

## <a name="service-logs"></a>Lokit

Azure erä lokit sisältää tapahtumien lähettämän Azure erä palvelun erä resurssin resurssivarantoon tai tehtävän elinkaaren aikana. Kuhunkin tapahtumaan erä lähettämän tallennetaan määritetyn tallennustilan tilin, JSON-muodossa. Tämä on esimerkiksi leipätekstiin malli- **ryhmän tapahtuman luominen**:

```json
{
    "poolId": "myPool1",
    "displayName": "Production Pool",
    "vmSize": "Small",
    "cloudServiceConfiguration": {
        "osFamily": "4",
        "targetOsVersion": "*"
    },
    "networkConfiguration": {
        "subnetId": " "
    },
    "resizeTimeout": "300000",
    "targetDedicated": 2,
    "maxTasksPerNode": 1,
    "vmFillType": "Spread",
    "enableAutoscale": false,
    "enableInterNodeCommunication": false,
    "isAutoPool": false
}
```

Jokaisen tapahtuman leipätekstin sijaitsee määritetyn Azure-tallennustilan tilin .json tiedoston. Jos haluat käyttää lokit suoraan, haluat ehkä tarkastella [vianmäärityslokit tallennustilan tilin rakenne](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md#schema-of-diagnostic-logs-in-the-storage-account).

## <a name="service-log-events"></a>Tapahtumien palvelun poistaminen

Erän palvelun tällä hetkellä tietokoneesta kuuluu äänimerkki loki tapahtumat. Tämä luettelo ei ehkä ole täydellisiä, koska muita tapahtumia mahdollisesti lisättyjä jälkeen tässä artikkelissa on päivitetty.

| **Tapahtumien palvelun poistaminen** |
| ------------------ |
| [Resurssivarannon luominen][pool_create] |
| [Ryhmän poistaminen alku][pool_delete_start] |
| [Ryhmän poistaminen valmis][pool_delete_complete] |
| [Resurssivarannon koon alku][pool_resize_start] |
| [Resurssivarannon koon valmis][pool_resize_complete] |
| [Tehtävän alkamis][task_start] |
| [Tehtävä valmis][task_complete] |
| [Tehtävän epäonnistuvat][task_fail] |

## <a name="next-steps"></a>Seuraavat vaiheet

Voit lisäksi tallentaa diagnostiikan tapahtumien poistaminen Azure-tallennustilan tilin, myös virtauttaa eräloki tapahtumien [Azure tapahtumaa-toiminnossa](../event-hubs/event-hubs-what-is-event-hubs.md)ja lähetä ne [Azure Log Analytics](../log-analytics/log-analytics-overview.md).

* [Virtauttaa Azure vianmäärityslokit tapahtuman porttiin](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md)

  Virtauttaa erä diagnostiikan tapahtumat erittäin skaalattava tietojen tunkeutumisen palveluun tapahtuman keskittimet. Tapahtuman keskittimet voit ingest miljoonia tapahtumat sekunnissa, jossa voit muuntaa ja tallentaa mitä tahansa reaaliaikainen analytics-palvelun avulla.

* [Analysoi Azure vianmäärityslokit Log-analyysin avulla](../log-analytics/log-analytics-azure-storage-json.md)

  Lähetä oman vianmäärityslokit Log Analytics kohtaa, johon voit analysoida niiden toimintojen hallinta Suite (OMS)-portaalissa tai viedä yhteystiedot analysointiin Power BI-tai Excelissä.

[pool_create]: https://msdn.microsoft.com/library/azure/mt743615.aspx
[pool_delete_start]: https://msdn.microsoft.com/library/azure/mt743610.aspx
[pool_delete_complete]: https://msdn.microsoft.com/library/azure/mt743618.aspx
[pool_resize_start]: https://msdn.microsoft.com/library/azure/mt743609.aspx
[pool_resize_complete]: https://msdn.microsoft.com/library/azure/mt743608.aspx
[task_start]: https://msdn.microsoft.com/library/azure/mt743616.aspx
[task_complete]: https://msdn.microsoft.com/library/azure/mt743612.aspx
[task_fail]: https://msdn.microsoft.com/library/azure/mt743607.aspx

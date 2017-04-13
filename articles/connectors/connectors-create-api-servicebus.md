<properties
pageTitle="Opi käyttämään Azure palvelun Bus yhdistimen logiikan sovelluksia | Microsoft Azure"
description="Luo logiikan sovelluksia Azure-sovelluksen-palvelun kanssa. Muodosta yhteys Azure palvelun Bus voit lähettää ja vastaanottaa viestejä. Voit suorittaa toimintoja, kuten Lähetä jonon, aiheen lähettäminen, vastaanottaa jonossa ja vastaanottaa tilauksen."
services="logic-apps"
documentationCenter=".net,nodejs,java"  
authors="msftman"
manager="erikre"
editor=""
tags="connectors" />

<tags
ms.service="logic-apps"
ms.devlang="multiple"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="08/02/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-azure-service-bus-connector"></a>Azure-palvelu Bus yhdistimen käytön aloittaminen

Muodosta yhteys Azure palvelun Bus voit lähettää ja vastaanottaa viestejä. Voit suorittaa toimintoja, kuten Lähetä jonon, aiheen lähettäminen, vastaanottaa jonossa ja vastaanottaa tilauksen.

Jos haluat käyttää [mitä tahansa yhdysviivaa](./apis-list.md), sinun on logiikan sovelluksen luominen. Voit aloittaa luomalla [logiikan sovellus nyt](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="connect-to-service-bus"></a>Palvelun Bus yhdistäminen

Ennen kuin logiikan sovelluksen voit käyttää mihinkään palveluun, sinun on yhteyden muodostaminen palvelu. [Yhteys](./connectors-overview.md) on logiikan-sovellus ja toisen palvelun välinen yhteys.  

>[AZURE.INCLUDE [Steps to create a connection to Azure Service Bus](../../includes/connectors-create-api-servicebus.md)]

## <a name="use-a-service-bus-trigger"></a>Käytä palvelun Bus käynnistin

Käynnistimen tapahtuma, jonka avulla voidaan aloittaa työnkulun määritetty logiikan-sovelluksessa. [Lisätietoja käynnistimien](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).  

>[AZURE.INCLUDE [Steps to create a Service Bus trigger](../../includes/connectors-create-api-servicebus-trigger.md)]  

## <a name="use-a-service-bus-action"></a>Palvelun Bus-toiminnolla

Toiminto on määritetty logiikan-sovelluksessa työnkulun suorittamiin toiminto. [Lisätietoja toiminnot](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

[AZURE.INCLUDE [Steps to create a Service Bus action](../../includes/connectors-create-api-servicebus-action.md)]  

## <a name="technical-details"></a>Teknisiä tietoja

Seuraavassa on tietoja käynnistimet, toiminnot ja vastaukset, joka tukee tätä yhteyttä.

### <a name="service-bus-triggers"></a>Palvelun Bus Käynnistimet

Palvelun Bus on seuraavassa käynnistimet:  

|Käynnistin | Kuvaus|
|--- | ---|
|[Kun viesti vastaanotetaan jonossa](connectors-create-api-servicebus.md#when-a-message-is-received-in-a-queue)|Tämä toiminto käynnistää vuo, kun viesti on vastaanotettu jonossa.|
|[Kun viesti vastaanotetaan aihe-tilaus](connectors-create-api-servicebus.md#when-a-message-is-received-in-a-topic-subscription)|Tämä toiminto tuottaa vuo, kun viesti vastaanotetaan aiheen tilauksen.|


### <a name="service-bus-actions"></a>Palvelun Bus toiminnot

Palvelun Bus on seuraavia toimia:


|Toiminto|Kuvaus|
|--- | ---|
|[Viestin lähettäminen](connectors-create-api-servicebus.md#send-message)|Tämä toiminto lähettää viestin jonossa tai aihe.|
### <a name="action-and-trigger-details"></a>Toiminto ja liipaisin tiedot

Seuraavien toimien tiedot ja käynnistimet yhdistimen, sekä niiden vastaukset.



#### <a name="send-message"></a>Viestin lähettäminen

|Ominaisuuden nimi| Näyttönimi|Kuvaus|
| ---|---|---|
|ContentData *|Sisältö|Viestin sisältö.|
|ContentType|Sisältötyyppi|Sisältötyypin viestin sisällön.|
|Ominaisuudet:|Ominaisuudet:|Avain-arvo-pareina kunkin se ominaisuus.|
|Yksikkö *|Jonon/aiheen nimi|Jonossa tai aiheen nimi.|

Myös nämä parametrit Lisäasetukset ovat käytettävissä:

|Ominaisuuden nimi| Näyttönimi|Kuvaus|
| ---|---|---|
|MessageId|Viestitunnus|Käyttäjän määrittämä arvo, palvelun Bus avulla voit tunnistaa viesteistä, jos käytössä.|
|Jos haluat|Jos haluat|Voit lähettää osoite.|
|ReplyTo|Vastaaminen|Voit vastata jonossa osoite.|
|ReplyToSessionId|Istunnon tunnus vastaaminen|Voit vastata istunnon tunnus.|
|Otsikko|Otsikko|Sovelluksen kielikohtaiset otsikko.|
|ScheduledEnqueueTimeUtc|ScheduledEnqueueTimeUtc|Päivämäärä ja kellonaika, UTC-muodossa, kun viestin lisätään jonossa.|
|Istunto|Istunnon tunnus|Istunnon tunnus.|
|CorrelationId|Korrelaatiotunnus|Korrelaatio tunnus.|
|TimeToLive|Elinaika|Kesto-jakoviivojen, että viesti on voimassa. Kun viesti on lähetetty palvelun Bus alkaa kesto.|



* Ilmaisee, että ominaisuus on tehty.


#### <a name="when-a-message-is-received-in-a-queue"></a>Kun viesti vastaanotetaan jonossa

|Ominaisuuden nimi| Näyttönimi|Kuvaus|
| ---|---|---|
|queueName *|Jonon nimi|Jonon nimi.|


* Ilmaisee, että ominaisuus on tehty.


##### <a name="output-details"></a>Tulostustiedot

ServiceBusMessage: Tämä objekti on sisältö ja palvelun Bus viestin ominaisuudet.


| Ominaisuuden nimi | Tietotyyppi | Kuvaus |
|---|---|---|
|ContentData|merkkijono|Viestin sisältö.|
|ContentType|merkkijono|Sisältötyypin viestin sisällön.|
|Ominaisuudet:|objektin|Avain-arvo-parit kunkin se ominaisuus.|
|MessageId|merkkijono|Käyttäjän määrittämä arvo, palvelun Bus avulla voit tunnistaa viesteistä, jos käytössä.|
|Jos haluat|merkkijono|Lähetä osoite.|
|ReplyTo|merkkijono|Voit vastata jonossa osoite.|
|ReplyToSessionId|merkkijono|Voit vastata istunnon tunnus.|
|Otsikko|merkkijono|Sovelluksen kielikohtaiset otsikko.|
|ScheduledEnqueueTimeUtc|merkkijono|Päivämäärä ja kellonaika, UTC-muodossa, kun viestin lisätään jonossa.|
|Istunto|merkkijono|Istunnon tunnus.|
|CorrelationId|merkkijono|Korrelaatio tunnus.|
|TimeToLive|merkkijono|Kesto-jakoviivojen, että viesti on voimassa. Kun viesti on lähetetty palvelun Bus alkaa kesto.|




#### <a name="when-a-message-is-received-in-a-topic-subscription"></a>Kun viesti vastaanotetaan aihe-tilaus

|Ominaisuuden nimi| Näyttönimi|Kuvaus|
| ---|---|---|
|topicName *|Aiheen nimi|Aiheen nimi.|
|subscriptionName *|Aiheen tilauksen nimi|Aiheen tilauksen nimeä.|


* Ilmaisee, että ominaisuus on tehty.


##### <a name="output-details"></a>Tulostustiedot

ServiceBusMessage: Tämä objekti on sisältö ja palvelun Bus viestin ominaisuudet.


| Ominaisuuden nimi | Tietotyyppi | Kuvaus |
|---|---|---|
|ContentData|merkkijono|Viestin sisältö.|
|ContentType|merkkijono|Sisältötyypin viestin sisällön.|
|Ominaisuudet:|objektin|Avain-arvo-pareina kunkin se ominaisuus.|
|MessageId|merkkijono|Käyttäjän määrittämä arvo, palvelun Bus avulla voit tunnistaa viesteistä, jos käytössä.|
|Jos haluat|merkkijono|Lähetä osoite.|
|ReplyTo|merkkijono|Voit vastata jonossa osoite.|
|ReplyToSessionId|merkkijono|Voit vastata istunnon tunnus.|
|Otsikko|merkkijono|Sovelluksen kielikohtaiset otsikko.|
|ScheduledEnqueueTimeUtc|merkkijono|Päivämäärä ja kellonaika, UTC-muodossa, kun viestin lisätään jonossa.|
|Istunto|merkkijono|Istunnon tunnus.|
|CorrelationId|merkkijono|Korrelaatio tunnus.|
|TimeToLive|merkkijono|Kesto-jakoviivojen, että viesti on voimassa. Kun viesti on lähetetty palvelun Bus alkaa kesto.|



### <a name="http-responses"></a>HTTP-vastaukset

Edellisen toiminnoista ja käynnistimien voi palauttaa yhden tai useamman HTTP tila-koodeista:

|Nimi|Kuvaus|
|---|---|
|200|Okei|
|202|Hyväksytty|
|400|Pyyntö ei kelpaa|
|401|Ei valtuuksia|
|403|Käyttö estetty|
|404|Ei löydy|
|500|Sisäinen palvelinvirhe. Tuntematon virhe.|
|Oletusarvo|Epäonnistui.|

## <a name="next-steps"></a>Seuraavat vaiheet
[Logiikan-sovelluksen luominen](../app-service-logic/app-service-logic-create-a-logic-app.md).

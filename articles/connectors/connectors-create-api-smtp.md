<properties
pageTitle="SMTP | Microsoft Azure"
description="Luo logiikan sovelluksia Azure-sovelluksen-palvelun kanssa. Muodosta yhteys sähköpostin SMTP."
services="logic-apps"   
documentationCenter=".net,nodejs,java"  
authors="msftman"   
manager="erikre"    
editor=""
tags="connectors" />

<tags
ms.service="app-service-logic"
ms.devlang="multiple"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="07/15/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-smtp-connector"></a>SMTP-yhdistin käytön aloittaminen

Muodosta yhteys sähköpostin SMTP.

Jos haluat käyttää [mitä tahansa yhdysviivaa](./apis-list.md), sinun on logiikan sovelluksen luominen. Voit aloittaa luomalla [logiikan sovellus nyt](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="connect-to-smtp"></a>Yhteyden muodostaminen SMTP

Ennen kuin logiikan sovelluksen voit käyttää mihinkään palveluun, sinun on palvelun *yhteyden* luominen. [Yhteys](./connectors-overview.md) on logiikan-sovellus ja toisen palvelun välinen yhteys. Esimerkiksi jotta voit muodostaa yhteyden SMTP, sinun on SMTP- *yhteyden*. Yhteyden luominen sinun on yleensä avulla voit käyttää haluat muodostaa yhteyden palvelun tunnistetiedoilla. SMTP-esimerkissä sinun on SMTP-yhteyden luominen yhteyden nimi ja SMTP-palvelimen osoite ja kirjautumistiedot käyttäjän tunnistetiedot. [Lisätietoja yhteydet]()  

### <a name="create-a-connection-to-smtp"></a>Yhteyden muodostaminen SMTP

>[AZURE.INCLUDE [Steps to create a connection to SMTP](../../includes/connectors-create-api-smtp.md)]

## <a name="use-an-smtp-trigger"></a>Käytä SMTP-käynnistin

Käynnistimen tapahtuma, jonka avulla voidaan aloittaa työnkulun määritetty logiikan-sovelluksessa. [Lisätietoja käynnistimien](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

Tässä esimerkissä koska SMTP ei ole omaa, käynnistintä käytetään **Salesforce - objektin luomisen** käynnistäminen. Tämä käynnistin Aktivoi kun Salesforce luodaan uusi objekti. Esimerkissä on määritetään sen siten, että aina, kun uusi liidi luodaan Salesforce- *Sähköpostin lähettäminen* toiminto suoritetaan SMTP-yhdistin, jolla ilmoitus luotavan uuden mahdollisuuden kautta.

1. *Salesforce* kirjoittaa hakukenttään logiikan sovellusten suunnittelu ja valitse sitten **Salesforce - objektin luomisen** käynnistäminen.  
 ![](../../includes/media/connectors-create-api-salesforce/trigger-1.png)  

2. **Kun objekti on luotu** ohjausobjektin tulee näkyviin.
 ![](../../includes/media/connectors-create-api-salesforce/trigger-2.png)  

3. Valitse **Objektilaji** ja valitse sitten luettelosta objektien *johtaa* . Tämä vaihe on osoittaa, että olet luomassa käynnistimen, joka ilmoittaa logiikan sovelluksen aina, kun uusi liidi luodaan Salesforce.  
 ![](../../includes/media/connectors-create-api-salesforce/trigger3.png)  

4. Käynnistin on luotu.  
 ![](../../includes/media/connectors-create-api-salesforce/trigger-4.png)  

## <a name="use-an-smtp-action"></a>SMTP-toiminnolla

Toiminto on määritetty logiikan-sovelluksessa työnkulun suorittamiin toiminto. [Lisätietoja toiminnot](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

Nyt kun käynnistin on lisätty, voit lisätä SMTP-toiminnon, joka suoritetaan, kun uusi liidi luodaan Salesforce seuraavasti.

1. Valitse **+ Uusi vaihe** haluat lisätä toiminnon, joka suoritetaan, kun uusi liidi luodaan haluat.  
 ![](../../includes/media/connectors-create-api-salesforce/trigger4.png)  

2. Valitse **Lisää toiminnon**. Avaa tämä hakuruutu, jossa voit hakea toiminnon voit haluat tehdä.  
 ![](../../includes/media/connectors-create-api-smtp/using-smtp-action-2.png)  

3. Kirjoita *smtp* Hae SMTP liittyviä toimintoja.  

4. Valitse toiminto, joka suoritetaan, kun luot uuden liidin **SMTP - Lähetä sähköpostia** . Avaa toiminnon hallinta estä. Sinun on määrittää suunnittelutyökalun estoasetukset smtp-yhteyden, jos et ole vielä tehnyt sitä aiemmin.  
 ![](../../includes/media/connectors-create-api-smtp/smtp-2.png)    

5. Kirjoita haluamasi sähköpostitietojen **SMTP - Lähetä sähköpostia** estoasetukset.  
 ![](../../includes/media/connectors-create-api-smtp/using-smtp-action-4.PNG)  

6. Tallenna työsi työnkulun aktivoimista.  

## <a name="technical-details"></a>Teknisiä tietoja

Seuraavassa on tietoja käynnistimet, toiminnot ja vastaukset, joka tukee tätä yhteyttä:

## <a name="smtp-triggers"></a>SMTP-Käynnistimet

SMTP ei ole Käynnistimet. 

## <a name="smtp-actions"></a>SMTP-toiminnot

SMTP on seuraavat toimet:


|Toiminto|Kuvaus|
|--- | ---|
|[Sähköpostin lähettäminen](connectors-create-api-smtp.md#send-email)|Tämä toiminto lähettää sähköpostiviestin yhdelle tai usealle vastaanottajalle.|

### <a name="action-details"></a>Toiminnon tiedot

Seuraavassa on tietoja yhdistimen, ja sen vastaukset toimenpiteitä varten:


### <a name="send-email"></a>Sähköpostin lähettäminen
Tämä toiminto lähettää sähköpostiviestin yhdelle tai usealle vastaanottajalle. 


|Ominaisuuden nimi| Näyttönimi|Kuvaus|
| ---|---|---|
|Jos haluat|Jos haluat|Määritä sähköpostiosoitteet puolipisteellä eroteltuina,recipient1@domain.com;recipient2@domain.com|
|KOPIO|kopio|Määritä sähköpostiosoitteet puolipisteellä eroteltuina,recipient1@domain.com;recipient2@domain.com|
|Aihe|Aihe|Sähköpostin aihe|
|Tekstissä|Tekstissä|Sähköpostiviestin teksti|
|Valitse|Valitse|Lähettäjän sähköpostiosoitesender@domain.com|
|IsHtml|Html on|Lähetä sähköposti (tosi tai EPÄTOSI) HTML-muodossa|
|Piilokopio|Piilokopio|Määritä sähköpostiosoitteet puolipisteellä eroteltuina,recipient1@domain.com;recipient2@domain.com|
|Merkitys|Merkitys|Merkitys sähköposti (suuri, Normaali tai pieni)|
|ContentData|Liitteet-sisällön tiedot|Tietoja sisällön (base64 koodattu virtaa ja nimellä-merkkijono on)|
|ContentType|Liitteiden sisältötyyppi|Sisältötyyppi|
|ContentTransferEncoding|Liitteiden sisällön siirron koodaus|Sisällön siirtäminen koodauksen (base64 tai ei mitään)|
|Tiedostonimi|Liitteiden tiedostonimi|Tiedostonimi|
|ContentId|Liitteiden sisällön tunnus|Sisällön tunnus|

* Ilmaisee, että ominaisuus on tehty


## <a name="http-responses"></a>HTTP-vastaukset

Toiminnoista ja käynnistimien yllä voi palauttaa yhden tai useamman HTTP tila-koodeista: 

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
[Logiikan sovelluksen luominen](../app-service-logic/app-service-logic-create-a-logic-app.md)
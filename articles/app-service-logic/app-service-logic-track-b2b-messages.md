<properties 
   pageTitle="Logiikan-sovelluksissa Azure-sovelluksen palvelun B2B viestien seuranta | Microsoft Azure" 
   description="Tässä artikkelissa käsitellään B2B käsittely seuranta" 
   services="logic-apps" 
   documentationCenter=".net,nodejs,java" 
   authors="rajram" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="04/20/2016"
   ms.author="rajram"/>


# <a name="track-b2b-messages"></a>B2B viestien seuranta

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]

## <a name="b2b-tracking-information"></a>B2B seurantatiedot
B2B viestintä liittyy viestin kaupan kumppanien välillä. Suhteet määritellään kaksi kaupan kumppanien välillä. Kun yhteys on muodostettu, käytettävissä on oltava seurannassa, jos tietoliikenne toimi odotetulla tavalla. 

Viestin seuranta B2B tilanteista indeksoitiin: AS2, EDIFACT, ja X12.

## <a name="as2"></a>AS2
Kun olet luonut AS2 API-sovelluksen esiintymää, Etsi kyseisen esiintymän ja **Seuranta**. Tässä voit tarkastella ja suodattaa kaikki AS2 seurantatiedot:  

![][1]  

## <a name="edifact"></a>EDIFACT
Kun olet luonut EDIFACT API-sovelluksen esiintymää, Etsi kyseisen esiintymän ja **Seuranta**. Tässä voit tarkastella ja suodattaa kaikki seurantatiedot EDIFACT. Lisäksi voit tarkastella interchange taso, ryhmittelytaso ja tapahtuman määrittäminen tason tietoja, kaikki samassa näkymässä. 

Jos erissä luodaan osana EDIFACT toimeenpano liittyvät myynti kumppanin hallinnan API-sovelluksessa, Batching-osa näyttää Näissä erissä. Voit valita erän Nähdäksesi aktiivinen viesti (jos saatavilla) ja valmis tiedot:  

![][2]      

## <a name="x12"></a>X12
Kun olet luonut X12 esiintymä API-sovelluksessa kyseisen esiintymän selaamalla ja valitse **Seuranta**. Tässä voit tarkastella ja suodattaa kaikki seurantatiedot X12. Lisäksi voit tarkastella interchange taso, ryhmittelytaso ja tapahtuman määrittäminen tason tietoja, kaikki samassa näkymässä.

Jos erissä luodaan osana X12 sopimuksia liittyvät myynti kumppanin hallinnan API-sovellus ja valitse Batching-osassa on luettelo Näissä erissä. Voit valita erän aktiivinen viesti (jos saatavilla) ja valmiit erissä tiedot.

<!--Image references-->
[1]: ./media/app-service-logic-track-b2b-messages/AS2Tracking.png
[2]: ./media/app-service-logic-track-b2b-messages/EDIFACTTracking.png

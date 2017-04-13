<properties
    pageTitle="Azure Funktiot ajastin käynnistimen | Microsoft Azure"
    description="Osaavat käyttää ajastimen käynnistimet Azure-funktiot."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure toimii, Funktiot, tapahtuman käsittelyn dynaaminen Laske serverless arkkitehtuuri"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="08/22/2016"
    ms.author="chrande; glenga"/>

# <a name="azure-functions-timer-trigger"></a>Azure Funktiot ajastin käynnistin

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Tässä artikkelissa kerrotaan, miten voit määrittää ajastimen käynnistimet Azure-funktiot. Ajastin käynnistää aikataulun, yhden kerran tai toistuva puhelu-funktiot.  

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a name="functionjson-for-timer-trigger"></a>Function.JSON ajastin käynnistimen

*Function.json* -tiedosto sisältää aikataulu-lauseke. Esimerkiksi seuraavat aikatauluun suorittaa funktion minuutin välein:

```json
{
  "bindings": [
    {
      "schedule": "0 * * * * *",
      "name": "myTimer",
      "type": "timerTrigger",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

Timer-käynnistin käsittelee monen esiintymän asteikko-kohtaa automaattisesti: vain yhden esiintymän tietyn timer-funktio toimii eri kaikkien esiintymien välillä.

## <a name="format-of-schedule-expression"></a>Aikataulun lausekkeen muoto

Aikataulu-lauseke on [CRON lauseke](http://en.wikipedia.org/wiki/Cron#CRON_expression) , joka sisältää 6 kenttiä: `{second} {minute} {hour} {day} {month} {day of the week}`. 

Huomautus monia online-etsiminen cron lausekkeiden pois {toisen}-kenttään, jotta Jos kopioit yhdellä on Säädä ylimääräisiä kentälle. 

Seuraavassa on joitakin aikataulun esimerkkilausekkeita:

Käynnistettävän 5 minuutin välein:

```json
"schedule": "0 */5 * * * *"
```

Käynnistettävän kerran tunnissa yläreunassa:

```json
"schedule": "0 0 * * * *",
```

Käynnistettävän kahden tunnin välein:

```json
"schedule": "0 0 */2 * * *",
```

Käynnistettävän kerran tunnissa 9 AM-5 PM avulla:

```json
"schedule": "0 0 9-17 * * *",
```

Käynnistettävän 9:30 Suomen päivittäin:

```json
"schedule": "0 30 9 * * *",
```

Käynnistettävän 9:30 Suomen jokaisen viikonpäivä:

```json
"schedule": "0 30 9 * * 1-5",
```

## <a name="timer-trigger-c-code-example"></a>Ajastin käynnistimen C#-koodiesimerkki

C# koodin esimerkin kirjoittaa yhden lokin aina, kun toiminto käynnistyy.

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");    
}
```

## <a name="next-steps"></a>Seuraavat vaiheet

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)] 

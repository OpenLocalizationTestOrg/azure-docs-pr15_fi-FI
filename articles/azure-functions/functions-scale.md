<properties
   pageTitle="Miten Azure Funktiot | Microsoft Azure"
   description="Tietoja siitä, miten Azure Funktiot skaalata tarpeiden tapahtumaohjattu-toiminnoista."
   services="functions"
   documentationCenter="na"
   authors="dariagrigoriu"
   manager="erikre"
   editor=""
   tags=""
   keywords="Azure Funktiot, funktioiden, käsittely, webhooks, dynaaminen suorittaminen, serverless arkkitehtuuri"/>

<tags
   ms.service="functions"
   ms.devlang="multiple"
   ms.topic="reference"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/03/2016"
   ms.author="dariagrigoriu"/>

# <a name="how-to-scale-azure-functions"></a>Miten Azure Funktiot

## <a name="introduction"></a>Johdanto

Azure-Funktiot etuna on se Laske resurssit ovat vain kulutettu tarvitaan. Tämä tarkoittaa, että et käyttämättömänä VMs maksaa tai on varata kapasiteettia, kun ehkä tarvitse sitä. Sen sijaan platform varaa Laske power, kun koodi on käynnissä, Skaalaa käsittelemään kuormituksen tarpeen mukaan ja valitse Skaalaa, kun koodi ei ole käynnissä.

Uusi ominaisuus voi koskevat on dynaaminen palvelusopimus.  

Tässä artikkelissa on yleiskatsaus dynaaminen palvelusopimus toiminta ja miten platform Skaalaa pyydettäessä Suorita koodisi.

Jos et ole tottunut Azure-Funktiot, varmista, että [yleisiä tietoja Azure-funktioista](functions-overview.md) on artikkelissa ymmärtämään sen ominaisuuksia.

## <a name="configure-azure-functions"></a>Määritä Azure-Funktiot

Kaksi tärkeimmät asetukset liittyvät skaalaus:

* [Azure-sovelluksen palvelusopimus](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) tai dynaaminen palvelupaketissasi
* Muistikoko suorittamisen-ympäristössä

Funktion kustannus muuttuu sen mukaan, kun valitset palvelusopimus. Dynaaminen palvelun luomalla suunnitelman kustannukset ovat suoritusaika, muistin koon ja suorituskerran määrä. Kulujen kertymä vain kun koodi on käynnissä.

Sovelluksen palvelusopimus isännöi oman toimii aiemmin VMs, joita voidaan käyttää myös muita suorittamisen. Kun maksat nämä VMs kuukausittain, ei ole ylimääräisiä maksutonta niiden toimintojen suorittamisen.

## <a name="choose-a-service-plan"></a>Valitse palvelun suunnitelma

Kun luot Funktiot, voit valita suorittamaan dynaaminen palvelusopimus tai [sovelluksen palvelusopimus](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
Sovelluksen palvelusopimus oman toiminnot suoritetaan erillinen AM, kuten web apps työtä tänään Basic, Standard tai Premium-tuotteissa.
Tämä erillinen AM on kohdistettu sovellukset ja Funktiot ja on aina käytettävissä onko koodi aktiivisesti suoritetaan vai ei. Tämä on hyvä vaihtoehto, jos sinulla on aiemmin, valitse käytettävä VMs, joka on jo käytössä muu koodi tai jos aiot käyttää funktioita, jatkuvasti tai lähes jatkuvasti. AM decouples kustannukset runtime ja muistin koosta. Voit rajoittaa seurauksena monien pitkään käynnissä olevien funktioiden vähintään yksi VMs ne käynnissä olevat kustannuksiksi kustannukset.

[AZURE.INCLUDE [Dynamic Service plan](../../includes/functions-dynamic-service-plan.md)]

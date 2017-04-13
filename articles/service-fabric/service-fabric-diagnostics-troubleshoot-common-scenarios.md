<properties
   pageTitle="Tapahtuman jäljityksen vianmääritys | Microsoft Azure"
   description="Tapahtui käyttöönotto palveluita Microsoft Azure palvelun kangasta yleisimmistä ongelmista."
   services="service-fabric"
   documentationCenter=".net"
   authors="mattrowmsft"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="03/31/2016"
   ms.author="mattrow"/>


# <a name="troubleshoot-common-issues-when-you-deploy-services-on-azure-service-fabric"></a>Yleisten ongelmien vianmääritys, kun otat käyttöön Azure palvelun kangasta palvelut

Kun käytössäsi on services developer-tietokoneeseen, on helppokäyttöinen [Visual Studio vianmääritystyökalut](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md). Remote klustereiden [kuntoraportit](service-fabric-view-entities-aggregated-health.md) ovat aina voit käynnistää. Helpoin tapa käyttää näitä raportteja ovat PowerShell tai [SFX](service-fabric-visualizing-your-cluster.md)kautta. Tässä artikkelissa oletetaan, että ovat virheenkorjaus remote klusterin ja perusteet käytöstä joko työkaluja.

##<a name="application-crash"></a>Sovelluksen kaatumisen
-"Osio on alle kohteen replikan tai esiintymän Laske" raportti on hyvä osoittaa, että palvelun kaatuu. Saat lisätietoja kaatuu, kun palvelua kestää hieman enemmän tutkimuksen. Kun palvelun ollessa käynnissä tasolla paras ystäväsi on joukko well-thought ulos jälkitiedot.  Suosittelemme, että yritä [Azure vianmäärityksen](service-fabric-diagnostics-how-to-setup-wad.md) näiden jäljittää ja ratkaisu, kuten [Joustavasti haun](service-fabric-diagnostic-how-to-use-elasticsearch.md) käyttäminen tarkasteleminen ja haku jälkitiedot.

![SFX osion kunto](./media/service-fabric-diagnostics-troubleshoot-common-scenarios/crashNewApp.png)

###<a name="during-service-or-actor-initialization"></a>Palvelun tai toimija alustaminen
Ennen kuin palvelutyypin alustaa poikkeukset aiheuttaa kaatumisen samalla tavalla. Kaatuu tämäntyyppisille sovelluksen tapahtumalokiin näkyy virhe-palvelussa.
Nämä ovat yleisimmät poikkeukset näkyviin ennen palvelun alustaa.

***System.IO.FileNotFoundException***

Tämä virhe on usein kokoonpanon riippuvuudet puuttuu. Tarkista Visual Studiossa tai kokoonpanovälimuistiin solmun CopyLocal-ominaisuutta.

***System.Runtime.InteropServices.COMException***
 *System.Fabric.Interop.NativeRuntime+IFabricRuntime.RegisterStatefulServiceFactory (IntPtr, IFabricStatefulServiceFactory)-palvelussa*
 
 Tämä ilmaisee, että palvelu rekisteröidyt tyyppinimi vastaa palvelun luetteloa.

[Azure diagnostiikka](service-fabric-diagnostics-how-to-setup-wad.md) voi määrittää kaikkien solmujen tapahtumaloki ladata automaattisesti.

###<a name="runasync-or-onactivateasync"></a>RunAsync() tai OnActivateAsync()
Jos kaatumisen tapahtuu alustus tai rekisteröity palvelutyypin tai toimija toiminnan aikana, poikkeuksen tunnisteta Azure palvelun kangasta mukaan. Voit tarkastella näitä EventSource tarjoajien, yksityiskohtaiset "Seuraava vaiheet"-osassa.

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja palvelun kangasta myöntämä aiemmin Diagnostiikka:

* [Luotettavan toimijoiden diagnostiikka](service-fabric-reliable-actors-diagnostics.md)
* [Luotettavan palvelujen diagnostiikka](service-fabric-reliable-services-diagnostics.md)

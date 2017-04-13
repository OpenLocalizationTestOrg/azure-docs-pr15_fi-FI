<properties 
   pageTitle="Virheenkorjaus Azure pilvipalveluihin | Microsoft Azure"
   description="Virheenkorjaus Azure pilvipalveluihin"
   services="visual-studio-online"
   documentationCenter="n/a"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="visual-studio-online"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="debugging-cloud-services"></a>Virheenkorjaus pilvipalveluihin

Voit korjata Azure sovelluksen Microsoft Visual Studio ja Azure SDK Azure-painikkeilla eri tavalla:

- Voit korjata Azure sovelluksen Visual Studio kun, kehittävät samalla tavalla kuin minkä tahansa Visual C#- tai Visual Basic-sovelluksen. Lisätietoja on artikkelissa [Virheenkorjaus oman pilvipalvelussa paikalliseen tietokoneeseen](vs-azure-tools-debug-cloud-services-virtual-machines.md#debug-your-cloud-service-on-your-local-computer).

- Voit kirjautua yksityiskohtaiset tiedot koodin suorittamisen sisällä roolit-, roolit on käytössä kehitysympäristö tai Azure Azure Diagnostiikka. Lisätietoja on artikkelissa [kerääminen kirjaaminen tietojen Azure Diagnostiikan avulla](http://go.microsoft.com/fwlink/p/?LinkId=400450).

- Jos käytät Visual Studio yrityksen kirjoittaa suunnattu .NET Framework 4 tai .NET Framework 4.5 roolit, voit ottaa IntelliTrace käyttöönotto Visual Studio Azure pilvipalvelussa aikaan. IntelliTrace on lokia, voit käyttää Visual Studiossa korjaamisessa sovelluksesi, ikään kuin se oli käynnissä Azure-tietokannassa. Lisätietoja on artikkelissa [Virheenkorjaus julkaistua pilvipalvelussa IntelliTrace ja Visual Studio]( http://go.microsoft.com/fwlink/p/?LinkId=623016).

- Voit ottaa remote virheenkorjaus cloud Services kun otat käyttöön Visual Studio pilvipalvelussa aikaan. Jos haluat ottaa käyttöön remote virheenkorjaus käyttöönottoa remote muistin services on asennettu, jotka suoritetaan roolin jokaiselle esiintymälle näennäiskoneiden. Nämä palvelut, kuten msvsmon.exe, älä vaikuttaa suorituskykyyn tai aiheutuu lisäkustannuksia. Lisätietoja on artikkelissa [Virheenkorjaus pilvipalvelussa Azure-tietokannassa](vs-azure-tools-debug-cloud-services-virtual-machines.md#debug-a-cloud-service-in-azure).




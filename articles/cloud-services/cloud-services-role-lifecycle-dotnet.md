<properties 
pageTitle="Käsittele pilvipalvelussa elinkaari tapahtumien | Microsoft Azure" 
description="Lue, miten pilvipalvelussa roolin elinkaari-menetelmiä voi käyttää .NET" 
services="cloud-services" 
documentationCenter=".net" 
authors="Thraka" 
manager="timlt" 
editor=""/>
<tags 
ms.service="cloud-services" 
ms.workload="tbd" 
ms.tgt_pltfrm="na" 
ms.devlang="na" 
ms.topic="article" 
ms.date="09/06/2016" 
ms.author="adegeo"/>

# <a name="customize-the-lifecycle-of-a-web-or-worker-role-in-net"></a>.NET-Web- tai työntekijän rooli Lifecycle mukauttaminen

Kun luot työntekijän rooli, voit laajentaa [RoleEntryPoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.aspx) luokan jonka avulla voit ohittaa menetelmiä, joiden avulla voit vastata elinkaari tapahtumiin. Web-roolien tähän luokkaan on valinnainen, joten sinun on käytettävä vastata elinkaari tapahtumiin.

## <a name="extend-the-roleentrypoint-class"></a>Laajenna RoleEntryPoint-luokkaa

[RoleEntryPoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.aspx) -luokkaan kuuluvat menetelmiä, joita kutsutaan mukaan Azure kun se **käynnistyy**, **suorittaa**tai **lopettaa** verkossa tai työntekijän rooli. Voit halutessasi ohittaa näistä tavoista voi hallita roolin alustus, roolin Sammuta sarjaa tai suorittamisen säikeen roolin. 

Kun laajentaminen **RoleEntryPoint**, sinun olisi otettava huomioon menetelmistä seuraavista ongelmista:

-   [OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) ja [OnStop](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx) menetelmiä palautettava totuusarvo, joten voi olla mahdollista palauttaa **Epätosi** seuraavista tavoista.

     Jos koodi palauttaa arvon **Epätosi**, roolin prosessi odottamatta lopetetaan, suorittamatta Sammuta tilde on ehkä paikassa. Yleensä Vältä palauttaminen **OnStart** menetelmästä **Epätosi** .
     
-   Minkä tahansa alueella liikaa **RoleEntryPoint** menetelmän poikkeus käsitellään käsittelemättömän poikkeuksen vuoksi.

     Jos poikkeuksen sisällä elinkaari tavoilla, Azure Nosta [UnhandledException](https://msdn.microsoft.com/library/system.appdomain.unhandledexception.aspx) tapahtuman ja sitten prosessi lopetetaan. Tehtäväsi on offline-tilassa, kun se käynnistetään Azure mukaan. Käsittelemättömän poikkeuksen vuoksi toteutuessa [pysäyttäminen](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.stopping.aspx) tapahtuma ole käynnistetään ja **OnStop** -menetelmää kutsutaan ei.

Jos tehtäväsi ei käynnisty tai valmistellaan varattu, ja pysäyttäminen Yhdysvaltojen välisen kierrättäminen, koodisi saattaa palautetaan käsittelemättömän poikkeusta johonkin aina, kun rooli käynnistyy Elinkaaritapahtumat. Käytä tässä tapauksessa [UnhandledException](https://msdn.microsoft.com/library/system.appdomain.unhandledexception.aspx) tapahtuman poikkeuksen syy ja käsitellä sen oikein. Tehtäväsi voi palauttaa myös [Suorita](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) -menetelmästä, joka aiheuttaa käynnistämään rooli. Lisätietoja käyttöönoton tilan Katso [Yleisten ongelmien joka syy roolit Roskakori](cloud-services-troubleshoot-common-issues-which-cause-roles-recycle.md).

> [AZURE.NOTE] Jos käytössäsi on **Microsoft Visual Studio työkalut Azure** kehittää sovellusta roolin project-mallit automaattisesti laajentaa **RoleEntryPoint** -luokka, *WebRole.cs* ja *WorkerRole.cs* -tiedostoissa.

## <a name="onstart-method"></a>OnStart menetelmä

**OnStart** menetelmää kutsutaan, kun rooli esiintymää on online-tilaan Azure. OnStart koodi suoritetaan, kun rooli esiintymää on merkitty **varattu** ja ei ulkoisen tietoliikenteen ohjataan siihen kuormituksen mukaan. Voit ohittaa tällä menetelmällä voit suorittaa alustus työlle, kuten tapahtuman käsittelytoimintoja toteuttaminen ja käynnistetään [Azure diagnostiikkaa](cloud-services-how-to-monitor.md).

Jos **OnStart** palauttaa arvon **Tosi**, esiintymän onnistuneesti alustaa ja Azure kutsuu **RoleEntryPoint.Run** -menetelmää. Jos **OnStart** palauttaa arvon **Epätosi**, roolin katkaisee heti ilman suorittamiseen suunnitellun Sammuta minkä tahansa sarjaa.

Seuraava koodi-esimerkissä esitetään, kuinka korvaavat **OnStart** -menetelmää. Tämä menetelmä määrittää ja käynnistää diagnostiikan näyttöä, roolin esiintymän käynnistyessä ja määrittää tallennustilan tilin lokitiedot siirretään:

```csharp
public override bool OnStart()
{
    var config = DiagnosticMonitor.GetDefaultInitialConfiguration();

    config.DiagnosticInfrastructureLogs.ScheduledTransferLogLevelFilter = LogLevel.Error;
    config.DiagnosticInfrastructureLogs.ScheduledTransferPeriod = TimeSpan.FromMinutes(5);

    DiagnosticMonitor.Start("DiagnosticsConnectionString", config);

    return true;
}
```

## <a name="onstop-method"></a>OnStop menetelmä

**OnStop** -menetelmää kutsutaan jälkeen roolin esiintymää on offline-tilassa Azure mukaan ja ennen prosessin poistuu. Voit ohittaa tällä menetelmällä voit soittaa koodi pakollinen rooli-esiintymän se suljetaan.

> [AZURE.IMPORTANT] Koodin **OnStop** -menetelmä on valmis, kun syistä kuin käyttäjän käynnistämä Sammuta rajoitetun ajan. Tällä hetkellä kuluu, kun tuonti on päättynyt, joten varmista koodia **OnStop** menetelmää voi käyttää nopeasti tai huolehtivan työn kesto päivinä ei suoriteta. **OnStop** -menetelmää kutsutaan, kun **pysäyttäminen** tapahtuma käynnistetään.


## <a name="run-method"></a>Suorita menetelmä

Voit ohittaa toteuttamisesta pitkään suoritettavien säikeen roolin esiintymän **Suorita** -menetelmää.

Ohittaminen **Suorita** -menetelmää ei ole pakollinen; Oletus-käyttöönotto alkaa säikeen, joka on lepotilassa jatkuvasti. Jos ohittaa **Suorita** -menetelmää, koodisi olisi estä jatkuvasti. **Suorita** -menetelmä palauttaa, onko rooli automaattisesti tilanteen kuluessa; Toisin sanoen Azure nostaa **pysäyttäminen** tapahtuma ja kutsuu **OnStop** -menetelmää niin, että sammuttamisen sarjaa voidaan suorittaa ennen rooli on offline-tilaan.


### <a name="implementing-the-aspnet-lifecycle-methods-for-a-web-role"></a>Web-roolin ASP.NET elinkaari-menetelmiä toteuttaminen

Voit lisäksi **RoleEntryPoint** -luokan tulostusvaihtoehtoja ASP.NET-lifecycle-menetelmiä alustus ja Sammuta järjestyksiä web-roolin hallinta. Tämä voi olla hyödyllinen yhteensopivuuden vuoksi, jos ovat sovittaminen aiemmin luotua ASP.NET-sovellusta, Azure. ASP.NET-lifecycle menetelmiä kutsutaan **RoleEntryPoint** menetelmiä kuluessa. **Sovelluksen\_Käynnistä** menetelmää kutsutaan **RoleEntryPoint.OnStart** menetelmä päätyttyä. **Sovelluksen\_Lopeta** menetelmää kutsutaan, ennen kuin **RoleEntryPoint.OnStop** -menetelmä kutsutaan.

## <a name="next-steps"></a>Seuraavat vaiheet
Lisätietoja luomisesta [cloud palvelupakettiin](cloud-services-model-and-package.md).
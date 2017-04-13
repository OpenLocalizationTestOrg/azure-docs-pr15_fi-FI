<properties
   pageTitle="TEMP-kansion oletuskoko on liian pientä roolin | Microsoft Azure"
   description="Cloud palvelun roolilla on rajoitettu määrä tilaa TEMP-kansiosta. Tässä artikkelissa on joitakin vinkkejä välttämisestä, jossa on loppumassa."
   services="cloud-services"
   documentationCenter=""
   authors="simonxjx"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd"
   ms.date="10/12/2016"
   ms.author="v-six" />

# <a name="default-temp-folder-size-is-too-small-on-a-cloud-service-webworker-role"></a>TEMP-kansion oletuskoko on liian pientä cloud palvelun web/Työntekijä rooliin

Oletusarvoinen tilapäiskansion cloud palvelun työntekijä tai web-roolin on koko on enintään 100 Mt, jotka voivat muuttua joskus koko. Tässä artikkelissa kerrotaan, miten voit välttää tilapäiskansion tallennustilan loppumisesta.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="why-do-i-run-out-of-space"></a>Miksi loppumassa käyttää?

Vakio Windows ympäristömuuttujat TEMP- ja Hallintasuunnitelmassa ovat käytettävissä tunnus, jolla sovellus on käynnissä. TEMP- ja Hallintasuunnitelmassa osoittamalla yksittäisen kansion, joka on koko on enintään 100 Megatavua. Tiedot, jotka on tallennettu tässä kansiossa ei säilytetä yli pilvipalvelussa; elinkaari rooli esiintymät pilvipalvelussa ovatko kuluessa hakemiston on puhdistettava.

## <a name="suggestion-to-fix-the-problem"></a>Voit korjata ongelman ehdotus

Toteuta jokin seuraavista vaihtoehdoista:

- Paikallinen tallennus resurssien ja käyttää sitä suoraan TEMP tai Hallintasuunnitelmassa sijaan. Käyttämään paikallisen säilön resurssin tunnus, jolla on käytössä sovelluksessa kutsua [RoleEnvironment.GetLocalResource](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.getlocalresource.aspx) -menetelmää. 

- Paikallinen tallennus resurssien ja valitse siten, että polku paikallisen säilön resurssin TEMP- ja Hallintasuunnitelmassa-kansioissa. Tämän muutoksen olisi voidaan suorittaa [RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) -menetelmää.

Koodin seuraavassa esimerkissä esitetään, muokkaamisesta kohde-kansioiden TEMP ja peräisin Teknologian sisällä OnStart menetelmää:


```csharp
using System;
using Microsoft.WindowsAzure.ServiceRuntime;

namespace WorkerRole1
{
    public class WorkerRole : RoleEntryPoint
    {
        public override bool OnStart()
        {
            // The local resource declaration must have been added to the
            // service definition file for the role named WorkerRole1:
            //
            // <LocalResources>
            //    <LocalStorage name="CustomTempLocalStore"
            //                  cleanOnRoleRecycle="false"
            //                  sizeInMB="1024" />
            // </LocalResources>

            string customTempLocalResourcePath =
            RoleEnvironment.GetLocalResource("CustomTempLocalStore").RootPath;
            Environment.SetEnvironmentVariable("TMP", customTempLocalResourcePath);
            Environment.SetEnvironmentVariable("TEMP", customTempLocalResourcePath);

            // The rest of your startup code goes here…

            return base.OnStart();
        }
    }
}
```

## <a name="next-steps"></a>Seuraavat vaiheet

Lue blogia, jossa kerrotaan, [miten voit suurentaa Azure Web roolin ASP.NET tilapäiskansion](http://blogs.msdn.com/b/kwill/archive/2011/07/18/how-to-increase-the-size-of-the-windows-azure-web-role-asp-net-temporary-folder.aspx).

Näytä lisää [artikkeleita vianmääritys](/?tag=top-support-issue&product=cloud-services) pilvipalveluihin.

Lisätietoja cloud palvelun roolin ongelmien vianmääritys käyttämällä Azure PaaS tietokoneen diagnostiikka tietoja, tarkastella [Kevin Williamson blogin sarjan](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).

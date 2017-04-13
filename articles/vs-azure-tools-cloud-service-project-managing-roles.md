<properties
   pageTitle="Azure pilveen roolien hallinta services projektien Visual Studiossa | Microsoft Azure"
   description="Opi lisää uusi roolit Azure cloud palvelun projektin tai poista se nykyisten roolit Visual Studion avulla."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="managing-roles-in-the-azure-cloud-services-projects-with-visual-studio"></a>Visual Studiossa Azure cloud services-projektit-roolien hallinta

Luotuasi Azure cloud palvelun projektin voit lisätä siihen uusia rooleja tai nykyisten roolit poistaminen. Voit myös tuoda aiemmin luodusta projektista ja muuntaminen rooli. Voit esimerkiksi tuoda ASP.NET web-sovelluksen ja nimetä sen verkko-roolissa.

## <a name="adding-or-removing-roles"></a>Lisäämällä tai poistamalla roolit

**Jos haluat lisätä rooli**

**Ratkaisunhallinnassa**cloud palvelun projektin **roolit** -solmu pikavalikon avaaminen ja sitten **Lisää**. Voit valita aiemmin luodun web-roolin tai työntekijän rooli nykyisestä ratkaisusta tai verkossa tai työntekijän rooli uuden projektin luominen. Tai voit valita haluamasi projekti, kuten ASP.NET web application projektin, ja liittää sen roolin projektin.

**Jos haluat poistaa roolin liitosta**

Napsauta ratkaisunhallinnassa cloud service-projektin **roolit** -solmu Avaa pikavalikon rooli ja valitse sitten **Poista**.

## <a name="removing-and-adding-roles-in-your-cloud-service"></a>Poistaminen ja lisääminen roolit oman pilvipalvelussa

Jos poistaminen roolista cloud projektista, mutta haluat lisätä rooliin projektiin myöhemmin, vain roolin ilmoituksen ja basic määritteiden, esimerkiksi päätepisteet ja diagnostiikka tiedot lisätään. Ei ole muita resursseja tai viittauksia lisätään ServiceDefinition.csdef tiedosto tai ServiceConfiguration.cscfg-tiedostoon. Jos haluat lisätä tiedot, sinun on lisätä sen manuaalisesti takaisin nämä tiedostot.

Esimerkiksi haluta poistaa WWW-palvelun rooli ja myöhemmin lisätä tämän roolin takaisin ratkaisu. Jos teet tämän, ilmenee virhe. Jos haluat estää tämän virheen, sinun on lisättävä `<LocalResources>` elementtien seuraavat XML takaisin ServiceDefinition.csdef-tiedosto. Käytä WWW-palvelun rooli, jonka lisäsit takaisin projektin nimimääritteen osana **<LocalStorage>** elementti. Tässä esimerkissä web roolin on **WCFServiceWebRole1**.

    <WebRole name="WCFServiceWebRole1">
        <Sites>
          <Site name="Web">
            <Bindings>
              <Binding name="Endpoint1" endpointName="Endpoint1" />
            </Bindings>
          </Site>
        </Sites>
        <Endpoints>
          <InputEndpoint name="Endpoint1" protocol="http" port="80" />
        </Endpoints>
        <Imports>
          <Import moduleName="Diagnostics" />
        </Imports>
       <LocalResources>
          <LocalStorage name="WCFServiceWebRole1.svclog" sizeInMB="1000" cleanOnRoleRecycle="false" />
       </LocalResources>
    </WebRole>

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja roolien määrittäminen Visual Studiossa lukemalla [Azure-pilvipalvelussa Visual Studiossa roolien määrittäminen](vs-azure-tools-configure-roles-for-cloud-service.md).

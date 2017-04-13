<properties
    pageTitle="CDN välimuistiin käytännön Media Services-tunniste"
    description="Tässä ohjeaiheessa on yleiskuvaus CDN välimuistiin käytännön Media-palveluiden laajennuksen."
    services="media-services,cdn"
    documentationCenter=".NET"
    authors="juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="juliako"/>
 
#<a name="cdn-caching-policy-in-media-services-extension"></a>CDN välimuistiin käytännön Media Services-tunniste

Azure Media-palvelut on HTTP säädön Streaming ja asteittain Lataa perusteella. HTTP mukaan streaming on erittäin skaalattava liittyvät edut välimuistiin tallentamisen välityspalvelimen ja CDN kerrokset sekä asiakkaan välimuistin. Streaming päätepisteet on yleinen streaming ominaisuuksia ja HTTP-välimuistin otsikot määritykset. Streaming päätepisteet asettaa HTTP välimuisti-komponenttien: max-ikä- ja Expires otsikot. Saat lisätietoja HTTP-välimuistin otsikot [W3.org](http://www.w3.org/Protocols/rfc2616/rfc2616-sec13.html).

##<a name="default-caching-headers"></a>Oletusarvoinen välimuisti otsikot

Oletusarvoisesti streaming päätepisteet käyttää 3 päivän välimuistin otsikot tarvittaessa streaming tietojen (todellinen media Vaillinaiset lauseet ja näkyvissä) ja manifest(playlist). Live streaming streaming päätepisteet Käytä 3 päivän välimuistin otsikot tietojen (todellinen media Vaillinaiset lauseet ja näkyvissä) ja 2 sekunnin ajan välimuistiin manifest(playlist) pyynnöt otsikko. Kun live ohjelma muuttuu tarvittaessa (live arkisto) Käytä tarvittaessa streaming välimuistin otsikot.

##<a name="azure-cdn-integration"></a>Azure CDN-integrointi

Azure Media-palvelut on [integroitu CDN](https://azure.microsoft.com/updates/azure-media-services-now-fully-integrated-with-azure-cdn/) streaming päätepisteet. Välimuistin-komponenttien otsikot koskee samalla tavalla CDN streaming päätepisteet käytössä streaming päätepisteet. Azure CDN käyttää streaming päätepisteen määritetty välimuistin arvot Määritä elinaika sisäisesti välimuistissa olevia objekteja ja Määritä toimituksen välimuistin otsikot myös käyttää tätä arvoa. Kun käyttämällä CDN käytössä streaming päätepisteet suositellaan ei pieni välimuistin arvojen määrittäminen. Pienet arvot määrittäminen pienentää suorituskykyä ja vähentää CDN etua. Määritä välimuistin otsikot pienempiä kuin 600 sekuntia CDN käytössä streaming päätepisteet ei voi suorittaa.

>[AZURE.IMPORTANT] Azure CDN Azure Media Services integrointi on toteutettu **Azure CDN-Verizon**.  Jos haluat käyttää **Azure CDN-Akamai** Azure Media Services, sinun täytyy [määrittää päätepisteen manuaalisesti](cdn-create-new-endpoint.md).  Lisätietoja Azure CDN ominaisuuksista on artikkelissa [CDN yleiskatsaus](cdn-overview.md).

##<a name="configuring-cache-headers-with-azure-media-services"></a>Välimuistin otsikoiden määrittäminen Azure Media-palvelujen kanssa

Voit käyttää Azure hallinta-portaalin tai Azure Media Services API määrittämiseen välimuistin otsikon arvoihin.

1. Ylätunnisteiden välimuistin hallinnan määrittäminen portal Katso [miten voit hallita Streaming päätepisteet](../media-services/media-services-portal-manage-streaming-endpoints.md) osan Streaming päätepisteen määrittäminen.
2. Azure Media Services-palveluiden REST API- [StreamingEndpoint](https://msdn.microsoft.com/library/azure/dn783468.aspx#StreamingEndpointCacheControl).
3. Azure Media Services-palveluiden .NET SDK-paketissa, [StreamingEndpointCacheControl ominaisuudet](http://go.microsoft.com/fwlink/?LinkId=615302).

##<a name="cache-configuration-precedence-order"></a>Välimuistin määritysten ensisijaisuusjärjestys

1. Azure Media-palvelut on määritetty välimuistin arvo ohittaa oletusarvo.
2. Jos ei ole manuaalista määritystä, oletusarvoja koskee.
3. Oletusarvoinen 2 sekunnin ajan välimuistin mukaan otsikot koskee live streaming manifest(playlist) riippumatta Azure Media-tai Azuren tallennustilaan ja ohitus tämä arvo ei ole käytettävissä.

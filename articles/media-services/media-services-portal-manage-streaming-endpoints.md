<properties 
    pageTitle="Hallitse streaming päätepisteet Azure-portaalissa | Microsoft Azure" 
    description="Tässä ohjeaiheessa esitellään hallinnasta streaming päätepisteet Azure-portaalissa." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    writer="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/24/2016"
    ms.author="juliako"/>


#<a name="manage-streaming-endpoints-with-the-azure-portal"></a>Hallitse streaming päätepisteet Azure-portaalissa

## <a name="overview"></a>Yleiskatsaus

> [AZURE.NOTE] Jotta voit suorittaa tässä opetusohjelmassa, sinun on Azure-tili. Lisätietoja on artikkelissa [Azure maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/). 

Microsoft Azure Media Services **Streaming päätepiste** edustaa streaming-palvelu, joka voi toimittaa sisältöä suoraan Playerin asiakassovellus, tai voit sisällön toimituksen verkon (CDN) edelleen. Media-palvelut on myös Azure CDN saumattomasti. Lähtevän stream StreamingEndpoint-palvelusta voi olla live stream tai videon Media Services-tilin kohteiden pyydettäessä.

Lisäksi voit hallita Streaming päätepisteen-palvelun kapasiteetti käsittelemään säätämällä streaming yksiköt kasvava kaistanleveyden tarpeisiin. On suositeltavaa varata vähintään yksi aikayksikön tuotantoympäristössä sovellukset. Streaming yksiköt antaa sinulle erillinen juniin kapasiteetin, joka voi ostaa 200 Mbps kokoisissa ja muita toimintoja, mukaan lukien: [Dynaaminen pakkaus](media-services-dynamic-packaging-overview.md), CDN integrointi ja lisämäärityksiä.

>[AZURE.NOTE]On vain laskuttaa Streaming päätepiste ollessa käynnissä tila.

Tässä ohjeaiheessa on yleiskuvaus tärkeimmät toimintoja, jotka on tarkoitettu Streaming päätepisteet. Aihe näkyy myös Azure portaalin avulla voit hallita streaming päätepisteet. Lisätietoja skaalata streaming päätepisteen, katso [ohjeaihe](media-services-portal-scale-streaming-endpoints.md) .

## <a name="start-managing-streaming-endpoints"></a>Voit hallinnoida streaming päätepisteet

Voit hallinnoida streaming päätepisteet tilisi toimimalla seuraavasti.

1. Valitse [Azure portal](https://portal.azure.com/)Azure Media Services-tilin.
2. Valitse **asetukset** -ikkunassa **Virtautetun median päätepisteet**.

    ![Päätepisteen Streaming](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints1.png)

##<a name="adddelete-a-streaming-endpoint"></a>Streaming päätepisteen lisääminen ja poistaminen

Lisää/Poista streaming päätepisteen Azure-portaalissa, toimi seuraavasti:

1. Voit lisätä streaming päätepiste valitsemalla **+ päätepisteen** sivun yläreunassa. 
2. Voit poistaa streaming päätepisteen painamalla **Poista** -painiketta. 

    Päätepisteen streaming oletusarvo ei voi poistaa.
2. Napsauta **Käynnistä** -painiketta voit käynnistää streaming päätepiste.

    ![Päätepisteen Streaming](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints2.png)

Oletusarvon mukaan käytössä voi olla enintään streaming päätepisteet. Jos haluat pyytää lisätietoja, katso [kiintiöiden ja rajoitukset](media-services-quotas-and-limitations.md).
    
##<a id="configure_streaming_endpoints"></a>Streaming päätepisteen määrittäminen

Streaming päätepisteen avulla voit määrittää seuraavat ominaisuudet, kun käytössä on oltava vähintään 1 asteikko: 

- Käyttöoikeuksien hallinta
- Välimuistin hallinta
- Toimintojen välinen sivuston käytön käytännöt

Saat lisätietoja näiden ominaisuuksien [StreamingEndpoint](https://msdn.microsoft.com/library/azure/dn783468.aspx).

Voit määrittää streaming päätepistettä seuraavasti:

1. Valitse streaming päätepiste, jonka haluat määrittää.
1. Valitse **asetukset**.
  
Lyhyt kuvaus-kenttien seuraa.

![Päätepisteen Streaming](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints4.png)
  
1. Välimuistin käytännön: määritetään välimuistin käyttöikä served – tämä streaming päätepiste resurssien osalta. Jos arvoa ei ole määritetty, käytetään oletusarvoa. Oletusarvot myös voidaan määrittää suoraan Azure-tallennustilan. Jos Azure CDN on käytössä streaming päätepisteen, välimuistin käytännön arvo olisi ei arvoksi enintään 600 sekuntia.  

2. Sallitut IP-osoitteet: Määritä IP-osoitteet, jotka voivat muodostaa julkaistun streaming päätepisteen avulla. Jos IP-osoitteita ei ole määritetty, mikä tahansa IP-osoite olisi pysty muodostamaan yhteyttä. IP-osoitteiden voidaan määrittää yksittäisen IP-osoite (esimerkiksi 10.0.0.1), käyttämällä IP-osoite ja CIDR aliverkkopeite (esimerkiksi 10.0.0.1/22) IP-alueen tai IP-osoite ja pisteviiva desimaaleja aliverkkopeite IP-alueen (esimerkiksi "10.0.0.1(255.255.255.0)').

3. Akamai allekirjoituksen otsikon käyttöoikeuksien määritykset: määrittää allekirjoituksen otsikon todennus pyytää Akamai palvelinten määrittämistavasta. Vanhenemisen on UTC-aika.



##<a id="enable_cdn"></a>Ota käyttöön Azure CDN-integrointi

Voit määrittää Azure CDN-integroinnin Streaming päätepisteen (se on poissa käytöstä oletusarvoisesti.)

Määritä Azure CDN integrointi tosi:

- Streaming päätepisteen on oltava vähintään yksi streaming yksikkö. Jos haluat myöhemmin aikayksikön arvoksi 0, sinun on poistettava käytöstä CDN integrointi. Oletusarvon mukaan kun luot uuden streaming päätepisteen yhden streaming yksikön määritetään automaattisesti.

- Pysäytetty tilan on oltava streaming päätepiste. Kun CDN saa käytössä, voit aloittaa streaming päätepiste. 

Voi kestää jopa 90 min Hae käytössä Azure CDN-integrointia varten.  Kestää jopa kaksi tuntia, jotta muutokset on yli kaikki CDN tulee aktiivinen.

CDN integrointi on otettu käyttöön Azure tietojen järjestelmänvalvojakeskuksiin: US Länsi, US Itä, Pohjois Europe, Länsi Europe, japani Länsi, japani Itä, Etelä Itä-Aasian ja Itä-Aasian.

Kun se on otettu käyttöön, **Käyttöoikeuksien valvonta** -asetukset saa käytöstä.

![Päätepisteen Streaming](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints5.png)

>[AZURE.IMPORTANT] Azure CDN Azure Media Services integrointi on toteutettu **Azure CDN-Verizon**.  Jos haluat käyttää **Azure CDN-Akamai** Azure Media Services, sinun täytyy [määrittää päätepisteen manuaalisesti](../cdn/cdn-create-new-endpoint.md).  Lisätietoja Azure CDN ominaisuuksista on artikkelissa [CDN yleiskatsaus](../cdn/cdn-overview.md).

###<a name="additional-considerations"></a>Muita huomioon otettavia seikkoja

- Kun CDN on käytössä streaming päätepisteen, asiakkaat voi pyytää sisältöä suoraan alkuperän. Jos tarvitset voi testata kanssa tai ilman CDN sisältöä, voit luoda toisen streaming päätepiste, joka ei ole käytössä CDN.
- Streaming päätepisteen-isäntänimi säilyy ennallaan CDN käyttöönoton jälkeen. Ei tarvitse tehdä muutoksia työnkulkuun media services kun CDN on otettu käyttöön. Esimerkiksi jos streaming päätepisteen-isäntänimi on strasbourg.streaming.mediaservices.windows.net, kun olet ottanut käyttöön CDN, täsmälleen saman hostname käytetään.
- Uusi streaming päätepisteiden voit ottaa CDN yksinkertaisesti luomalla uuden päätepisteen; olemassa olevan streaming päätepisteet haluat lopettaa ensin päätepiste ja valitse Ota käyttöön CDN.
 

Lisätietoja on artikkelissa, [julkaisu Azure Media Services integrointi Azure CDN (sisällön toimituksen Network)](http://azure.microsoft.com/blog/2015/03/17/announcing-azure-media-services-integration-with-azure-cdn-content-delivery-network/).


##<a name="next-steps"></a>Seuraavat vaiheet

Tarkista oppimispolut Media-palveluita.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
 

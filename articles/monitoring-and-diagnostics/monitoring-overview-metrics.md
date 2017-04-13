<properties
    pageTitle="Yleistä Microsoft Azure arvot | Microsoft Azure"
    description="Arvot ja niiden käyttötavat Microsoft Azure-yleiskatsaus"
    authors="kamathashwin"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="ashwink"/>

# <a name="overview-of-metrics-in-microsoft-azure"></a>Yleistä Microsoft Azure arvot 

Tässä artikkelissa kuvataan, mitä arvot ovat Microsoft Azure niiden edut ja kuinka voit alkaa käyttää niitä.  

## <a name="what-are-metrics"></a>Mitkä ovat arvot?

Azure näytön mahdollistaa tarjoaman telemetriatietojen ja myönnä näkyvyys suorituskykyä ja Azure-että työmääriä kunto. Tärkeimmät Azure telemetriatietoja ovat eniten Azure resurssien lähettämän arvot (kutsutaan myös suorituskyvyn laskureita). Azure näyttö on useita tapoja ja tarjoaman nämä mittaukset valvonta ja vianmääritys.


## <a name="what-can-you-do-with-metrics"></a>Mitä arvot voi tehdä?

Arvot ovat telemetriatietojen arvokkaita lähteen ja avulla voit tehdä seuraavasti:

- **Seuraa suorituskyvyn** resurssi (esimerkiksi AM, sivuston tai logiikan App) piirtäminen sen arvot-portaalin kaaviosta ja kiinnittäminen kaavion koontinäyttöön.
- **Saat ilmoituksen seurantakohteen** resurssin suorituskykyyn kun mittarin ylittää tietty määrä.
- **Määritä automaattisia toimintoja**, kuten automaattisen skaalauksen poistaminen resurssi tai käynnistettäessä runbookin, kun mittarin ylittää tietty määrä.
- **Suorita advanced analytics** tai raportoinut suorituskyvyn tai käyttö kehitystä oman resurssit.
- **Arkisto** oman resurssin **vaatimustenmukaisuus ja valvontaan** tarkoituksiin suorituskykyä tai kunto historia.

##  <a name="metric-characteristics"></a>Metrijärjestelmän ominaisuudet
Arvot ovat seuraavat ominaisuudet:

- Kaikki arvot on **1 minuutin korkojakso**. Saat metrisillä arvon minuutin välein resurssi, jossa lähellä reaaliaikainen näkyvyys tilaa ja resurssin kunto.
- Arvot ovat **käytettävissä ulos,-valmiilla tarvitsematta voit osallistua** tai muita Diagnostiikka on määritetty.
- Voit käyttää kunkin metrijärjestelmä **30 päivän historia** . Voit nopeasti tarkastella uusimpia ja kuukausittaiset trendejä suorituskykyä tai resurssi kunto.

Voit:

- Tutustu helposti, access ja valitse resurssi ja piirtää kaavion Azure portaalin kautta **Näytä kaikki arvot** . 
- Määritä metrisillä **ilmoitusten säännön, joka lähettää ilmoituksen tai on automaattinen toiminto** , kun lisätiedot ylittää raja-arvo on määritetty. Automaattinen skaalaus on erityisen automaattinen toiminto, jonka avulla voit skaalata, resurssi pyynnöt täyttävät tai ladata sivuston tai Laske resurssien. Voit määrittää Automaattinen skaalaus asetus sääntö, joka skaalaa ulos-/ mittarin leikkaavat raja-arvon perusteella.
- **Arkisto** mittaukset enää tai käyttää niitä offline-raportointia varten. Voit ohjata oman mittarit blob storage for resurssin diagnostiikan asetuksia määrittäessäsi.
- **Virta** arvot tapahtuma-keskittimeen, voit reitittää ne Azure Stream Analytics-tai mukautettujen sovellusten reaaliaikaiset lähellä analyysia varten. Tee käyttäen diagnostiikka-asetusten muuttaminen.
- **Reitin** kaikki mittarit OMS Log Analytics lukituksen pikaviestien analytics, haku ja resurssien tietoja arvot mukautetun Ilmoita.
- **Kulutettava** arvot uusi Azure näytön REST API kautta.
- **Kyselyn** arvot PowerShell-cmdlet-tai Office kaikissa ympäristöissä REST API.

 ![Reititys Azure näytön arvot](./media/monitoring-overview-metrics/MetricsOverview0.png)

## <a name="access-metrics-via-portal"></a>Access-arvot portaalin kautta
Seuraavassa on lyhyt vaiheista kautta Azure portal metrisillä kaavion luominen

### <a name="view-metrics-after-creating-a-resource"></a>Näytä arvot resurssin luomisen jälkeen
1. Avaa Azure portal
2. Luo uusi sovellus-palvelu - sivuston.
3. Kun luot sivuston, siirry web-sivuston yhteenveto-sivu.
4. Voit tarkastella uudet arvot 'seuranta-ruuduksi. Voit muokata ruutu ja valitse Lisää arvot

 ![Resurssin Azure valvonnasta arvot](./media/monitoring-overview-metrics/MetricsOverview1.png)    

### <a name="access-all-metrics-in-a-single-place"></a>Käyttää kaikki arvot yhteen paikkaan
1. Avaa Azure-portaali 
2. Siirry uuden näyttö-välilehti ja valitse se-kohdassa arvot-vaihtoehto 
3. Voit valita tilauksen resurssiryhmä ja resurssin nimi avattavasta luettelosta. 
4. Voit nyt tarkastella käytettävissä olevat arvot-luettelosta. 
5. Valitse lisätiedot ovat kiinnostuneita ja piirtää se. 
6. Voit kiinnittää sen koontinäyttö napsauttamalla oikeassa yläkulmassa PIN-tunnuksen.

 ![Käyttää kaikki arvot yhteen paikkaan Azure valvonta](./media/monitoring-overview-metrics/MetricsOverview2.png) 


>[AZURE.NOTE] Voit käyttää host tason arvot-VMs (Azure resurssien hallinta mukaan) ja AM asteikko joukot ilman mitään diagnostiikan Lisäasetukset. Uusi host tason nämä arvot ovat käytettävissä Windows ja Linux esiintymät. Nämä arvot eivät voi sekoittaa Vieras OS tason arvot, joihin sinulla on pääsy, kun otat käyttöön Azure diagnostiikka VMs tai VMSS. Lisätietoja Azure diagnostiikka määrittämisestä on artikkelissa [Mikä on Microsoft Azure diagnostiikka](../azure-diagnostics.md).

## <a name="access-metrics-via-rest-api"></a>Access-arvot REST API kautta
Azure arvot käyttää Azure näytön ohjelmointirajapinnan kautta. On kaksi API, joiden avulla voit tutkia ja käyttää arvot. Käytä: 

- [Azure näytön metrijärjestelmä määritelmät REST API](https://msdn.microsoft.com/library/mt743621.aspx) käyttää arvot, jotka ovat käytettävissä palvelun luetteloa.
- Todellinen arvot tietojen [Azure näytön arvot REST-Ohjelmointirajapinnalla](https://msdn.microsoft.com/library/mt743622.aspx)

>[AZURE.NOTE] Tässä artikkelissa käsitellään arvot [uudet arvot Ohjelmointirajapinta](https://msdn.microsoft.com/library/dn931930.aspx) Azure resurssien kautta. Uudet metrisillä määritykset API API-versio on 2016-03-01 ja arvot API numerotyyli 2016-09-01. Aiempien versioiden metrisillä määritelmät ja arvot niitä voi käyttää api-versiolla 2014-04-01.

Katso tarkempia ongelmatilanteita käyttämällä Azure näytön REST API- [Azure näytön REST API ongelmatilanteita](monitoring-rest-api-walkthrough.md).

## <a name="export-options-for-metrics"></a>Viedään mittarit tukivaihtoehdot
Voit siirtyä näyttö-välilehdessä diagnostiikka lokit-sivu ja tarkastella arvot vientiasetukset. Voit valita arvot (ja vianmäärityslokit) reititetään Blob-objektien tallennustilaan tapahtuman keskittimet tai OMS Log Analytics tapauksissa aiemmin tässä artikkelissa mainittujen käyttöä. 

 ![Viedään mittarit Azure valvonta-asetukset](./media/monitoring-overview-metrics/MetricsOverview3.png)   

Voit määrittää mallit- [PowerShell](insights-powershell-samples.md), [CLI](insights-cli-samples.md) tai [REST API](https://msdn.microsoft.com/library/dn931943.aspx)Resurssienhallinta kautta. 

## <a name="take-action-on-metrics"></a>Reagoi arvojen mukaisesti
Voit vastaanottaa ilmoituksia tai automaattisia toimintoja metrisillä tietoihin. Voit määrittää ilmoitusten sääntöjä tai Automaattinen skaalaus asetukset tekemään niin.

### <a name="alert-rules"></a>Ilmoitusten säännöt
Voit määrittää ilmoitusten säännöt arvojen mukaisesti. Ilmoitusten sääntöjen tarkistaa Jos mittarin on pystyyn tietty määrä ja ilmoittaa sähköpostitse tai Kyselysäännön webhook, valitse voidaan suorittaa minkä tahansa mukautettua komentosarjaa. Voit käyttää myös webhook määrittäminen 3 tuotteen integroinnit.

 ![Arvot ja ilmoitusten sääntöjä Azure-näyttö](./media/monitoring-overview-metrics/MetricsOverview4.png)

### <a name="autoscale"></a>Automaattinen skaalaus
Azure resurssien tukevat useita kertoja tai skaalata käsittelemään oman toiminnoista. Automaattinen skaalaus koskee App Services (online), virtuaalikoneen asteikko joukot (VMSS) ja perinteinen pilvipalveluihin. Voit määrittää Automaattinen skaalaus säännöt skaalata tai kun tiettyjä vaikuttavat havainnollistamiseen mittarin ylittää määrittämäsi raja-arvon. Lisätietoja on artikkelissa [Yleistä automaattisen skaalauksen poistaminen](monitoring-overview-autoscale.md).

 ![Arvot ja Automaattinen skaalaus Azure-näyttö](./media/monitoring-overview-metrics/MetricsOverview5.png)

## <a name="supported-services-and-metrics"></a>Tuetut palvelut ja arvot
Azure valvonta on uudet arvot-infrastruktuuria. Se tukee Azure portaalin ja Azure näytön Ohjelmointirajapinnan uuden version Azure seuraavat palvelut:

- VMs (Azure resurssien hallinta mukaan)
- AM asteikko joukot
- Erän
- Tapahtuman keskittimeen nimitila 
- Palvelun Bus nimitilan (vain premium SKU)
- SQL (12-versio)
- Joustavasti SQL resurssivarantoon
- Web-sivustot
- Web-palvelimen klustereihin
- Logiikan sovellukset
- IoT keskittimet
- Redis välimuisti
- Verkko: Sovelluksen yhdyskäytävät
- Haku

Voit tarkastella yksityiskohtainen luettelo Tuetut palvelut ja niiden arvot [Azure näytön arvot - tuetut arvot kohti resurssilaji](monitoring-supported-metrics.md)-palvelussa. 


## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja on artikkelissa linkeissä. Lisäksi tässä artikkelissa:  

- tietoja [yleisten mittaukset automaattisen skaalauksen poistaminen](insights-autoscale-common-metrics.md)
- [ilmoitusten sääntöjen](insights-alerts-portal.md) luominen





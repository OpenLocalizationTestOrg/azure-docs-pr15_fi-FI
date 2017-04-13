<properties
    pageTitle="Azure CDN käyttäminen CORS | Microsoft Azure"
    description="Opettele käyttämään Azure sisällön toimituksen verkon (CDN), ja rajat Origin resurssien jakaminen (CORS)."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="casoper"/>
    
# <a name="using-azure-cdn-with-cors"></a>Azure CDN käyttäminen CORS     

## <a name="what-is-cors"></a>Mikä on CORS?

CORS (toimintojen Origin resurssien jakaminen) on HTTP-ominaisuus, joka mahdollistaa resursseihin toisen toimialueen käynnissä yhdessä toimialueen verkkosovelluksen. Vähentämiseksi vahinkojen komentosarjoja sivustojen-Moderni selaimet toteuttaa [saman origin käytännön](http://www.w3.org/Security/wiki/Same_Origin_Policy)nimeltään suojauksen rajoitus.  Tämä estää verkkosivun puheluja API-toimialueesta.  CORS tarjoaa suojatun Soita ohjelmointirajapinnan toisen toimialueen yhtä toimialuetta (origin toimialue).
 
## <a name="how-it-works"></a>Toiminta
1.  Selain lähettää asetukset pyynnön **Origin** HTTP-otsikkoon. Tämä ylätunniste arvo served pääsivun muokattavan toimialueen. Kun https://www.contoso.com sivun yrittää käyttää käyttäjän tietoja fabrikam.com-toimialue, seuraavat pyynnön otsikko lähetetään fabrikam.com: 
    
    `Origin: https://www.contoso.com`
 
2.  Palvelin saattaa toimia kanssa seuraavasti:
    - **Access-ohjausobjekti-Salli-Origin** otsikossa osoittaa, mitkä origin sivustot saavat sen vastaus. Esimerkki:
        
        `Access-Control-Allow-Origin: https://www.contoso.com`
        
    - Jos palvelin ei salli rajat origin pyynnön virhesivun
    - **Access-ohjausobjekti-Salli-Origin** otsikkoon yleismerkkejä, jonka avulla kaikki toimialueet:
        
        `Access-Control-Allow-Origin: *`
 
Monimutkainen pyyntöjen on "kuvatiedostomuodot" pyyntö valmis ensimmäinen, voit selvittää, onko sen käyttöoikeus ennen lähettämistä koko pyynnön.
 
## <a name="wildcard-or-single-origin-scenarios"></a>Yleismerkkien tai yksittäisen origin skenaarioita

Valitse Azure CDN CORS toimii automaattisesti lisämäärityksiä kun **Access-ohjausobjekti-Salli-Origin** otsikko on määritetty yleismerkkien (*) tai yksittäinen alkuperän.  Tallentaa CDN välimuistiin ensimmäisen vastauksen ja myöhemmät käyttää samaa ylätunnistetta.
 
Jos pyynnöt on jo tehty CDN ennen CORS on määritetty että origin tarvitset Tyhjennä sisällön päätepisteen sisällön Lataa **Access-ohjausobjekti-Salli-Origin** otsikko ja sisältö.
 
## <a name="multiple-origin-scenarios"></a>Useita origin skenaariot

Jos haluat sallia vain tietyille alkuperistä hyväksytään CORS, asiat saat monimutkaisempia hieman. Ongelma ilmenee, kun CDN välimuistiin ensimmäisen CORS origin **Access-ohjausobjekti-Salli-Origin** otsikko.  Kun eri CORS origin tekee seuraavat pyynnön, CDN tulee served välimuistissa **Access-ohjausobjekti-Salli-Origin** otsikosta, jossa eivät täsmää.  Voit korjata ongelman seuraavilla tavoilla:
 
### <a name="azure-cdn-premium-from-verizon"></a>Azure CDN Premium-Verizon

Voit ottaa tämän helpoiten käyttämään **Azure CDN Premium-Verizon**, joka aiheuttaa joidenkin kehittyneitä toimintoja. 
 
Tarvitset [säännön](cdn-rules-engine.md) luominen tarkistamaan **Origin** ylätunnisteen pyynnön.  On kelvollinen origin säännön määrittäminen **Access-ohjausobjekti-Salli-Origin** otsikon pyynnössä esitetyt origin kanssa.  Jos **Origin** otsikossa määritetyn origin ei sallita, sääntö kannattaa jättää pois **Access-ohjausobjekti-Salli-Origin** otsikon mikä aiheuttaa selaimessa, voit hylätä pyynnön. 
 
Voit tehdä tämän säännöt vaihde kahdella eri tavalla.  Kummassakin tapauksessa **Access-ohjausobjekti-Salli-Origin** otsikon tiedoston alkuperän palvelimesta kokonaan ohitetaan, CDN säännöt ohjelma hallitsee kokonaan sallittu CORS alkuperistä.

#### <a name="one-regular-expression-with-all-valid-origins"></a>Kaikki kelvollinen alkuperän yhdessä säännöllisesti lausekkeen
 
Tässä tapauksessa luot säännöllinen lauseke, joka sisältää kaikki haluat sallia alkuperistä: 

    https?:\/\/(www\.contoso\.com|contoso\.com|www\.microsoft\.com|microsoft.com\.com)$
 
> [AZURE.TIP] **Azure-Verizon CDN** käyttää [Perl yhteensopiva säännöllisiä lausekkeita](http://pcre.org/) sen säännöllisiä lausekkeita-ohjelma.  Työkalu, kuten [säännöllisesti lausekkeiden 101](https://regex101.com/) avulla voit vahvistaa säännöllinen lauseke.  Huomaa, että "/"-merkki on voimassa säännönmukaiset lausekkeet ja ei tarvitse olla ohitettuja, kuitenkin vuotamaan merkin parhaiden käytäntöjen ja odotetaan joitakin regex tarkistajien.

Jos säännöllinen lauseke ei vastaa, sääntö korvaa **Access-ohjausobjekti-Salli-Origin** otsikko (jos saatavilla) kohteen alkuperäisestä origin pyynnön lähettäneen kanssa.  Voit lisätä myös muita CORS otsikot, kuten **Access-ohjausobjekti-Salli-menetelmiä**.

![Säännöt-esimerkki, jossa säännöllinen lauseke](./media/cdn-cors/cdn-cors-regex.png)
 
#### <a name="request-header-rule-for-each-origin"></a>Pyydä kunkin alkuperän säännön otsikko.

Sen sijaan, että säännöllisistä lausekkeista voit luoda sen sijaan erillisessä säännön kunkin alkuperän haluat sallia käyttämällä **Pyytää otsikon yleismerkkien** [vastaa ehtoa](https://msdn.microsoft.com/library/mt757336.aspx#Anchor_1). Säännöllinen lauseke-menetelmän kanssa yksin säännöt-ohjelma määrittää CORS otsikot. 
  
![Säännöllinen lauseke ilman säännöt-Esimerkki](./media/cdn-cors/cdn-cors-no-regex.png)

> [AZURE.TIP] Yleismerkki käyttö yllä olevassa esimerkissä * kertoo säännöt-ohjelma vastaamaan HTTP- ja HTTPS.
 
### <a name="azure-cdn-standard"></a>Azure CDN standardiksi

Azure CDN vakio-profiileista sallimaan useita alkuperän yleismerkkien origin käyttämättä vain järjestelmä on käyttää [kyselymerkkijonon välimuistiin](cdn-query-string.md).  Sinun on CDN päätepisteen kyselyn merkkijono-asetus käyttöön ja käyttää yksilöllinen kyselymerkkijonon sallittu toimialue-pyynnöt. Näin johtaa CDN välimuistiin kunkin yksilöllinen kyselymerkkijonon erillistä objektia. Tämä vaihtoehto ei ole kuitenkin ihanteellinen, kun se aiheuttaa välimuistissa olevien CDN saman tiedoston useita kopioita.  


<properties
    pageTitle="Azure-sovelluksen palvelun verkkosovelluksissa mukautetun toimialuenimen ostaminen"
    description="Opettele web App-ohjelmalla Azure-sovelluksen palvelun mukautetun toimialuenimen ostaminen."
    services="app-service\web"
    documentationCenter=""
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="buy-and-configure-a-custom-domain-name-in-azure-app-service"></a>Osta ja määrittäminen käyttämään mukautettua toimialuenimeä App Azure-palvelu

[AZURE.INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

Kun luot verkkosovellukseen, Azure määrittää sen alitoimialuetta azurewebsites.net. Jos web-sovelluksen nimi on **contoso**-URL-osoite on **contoso.azurewebsites.net**. Azure määrittää myös virtual IP-osoite.

Tuotannon web-sovelluksen haluat todennäköisesti käyttäjät näkevät mukautettua toimialuenimeä. Tässä artikkelissa kerrotaan, miten voit ostaa ja mukautetun toimialueen määrittäminen [Sovelluksen palvelun Web Apps -sovellusten](http://go.microsoft.com/fwlink/?LinkId=529714)kanssa. 

[AZURE.INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]


## <a name="overview"></a>Yleiskatsaus

Jos sinulla ei ole toimialuenimi web App-sovelluksen, voit helposti ostaa sen [Azure](https://portal.azure.com/)-portaalissa. Osta aikana voit olla WWW- ja ensisijaisen toimialueen DNS-tietueiden yhdistetään automaattisesti web App-sovellukseen. Voit hallita toimialueen oikealla sisällä Azure-portaalissa.


Seuraavien vaiheiden avulla voit ostaa toimialuenimiä ja Määritä web App-sovellukseen.

1. Avaa [Azure Portal](https://portal.azure.com/)selaimella.

2. **Web Apps** -välilehdessä web-sovelluksen nimi, valitse **asetukset**ja valitse sitten **Mukautetut toimialueet**

    ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-6.png)

3. Valitse **Mukautetut toimialueet** , sivu **Osta toimialueet**.

    ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-1.png)

4. Kirjoita toimialuenimi, jonka haluat ostaa ja Enter-näppäintä **Osta toimialueet** -sivu tekstiruudun avulla. Ehdotettu käytettävissä toimialueet näytetään tekstiruudun alapuolella. Valitse toimialue, mitä haluat ostaa. Voit ostaa useita toimialueita samalla kertaa. 

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-2.png)

5. Valitse **Yhteystiedot** ja anna toimialueen yhteystiedot-lomakkeen.

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-3.png)

    > [AZURE.NOTE] On tärkeää täyttää kaikki tarvittavat kentät mahdollisimman paljon tarkkuutta mahdollisimman erityisesti sähköpostiosoite. Jos olet ostanut toimialueen ilman "Tietosuojan", sinulta saatetaan pyytää vahvistamiseksi sähköpostiin, ennen kuin toimialue on aktiivinen. Joissakin tapauksissa virheellisten tietojen yhteystiedot aiheuttaa virheen, voit ostaa toimialueen. 

6. Nyt voit valita

    a) "Automaattinen uusiminen" toimialueen vuosittain
    
    b) asetukset-kohdassa "Yksityisyyden suojan", joka sisältyy ostohinta MAKSUTTA (lukuun ottamatta ylätason kuka rekisterin eivät tue tietosuoja. Http://servername:. co.in,. co.uk jne.)  
    
    (c) "oletus isäntänimet liittäminen" WWW ja pääkansio toimialueen nykyisen Web App-sovellukseen. 

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-2.5.png)
  
    > [AZURE.NOTE] Vaihtoehto C määrittää DNS-sidontojen ja Hostname sidontojen automaattisesti puolestasi.  Tällä tavalla Web Appissa voi käyttää mukautettua toimialuetta heti, kun ostamisesta on valmis (DNS välittäminen viipeitä joissakin tapauksissa baring). Siltä varalta, Web-sovellus on taustalla Azure liikenteen hallinta, et näe mahdollisuus määrittää päätoimialueen, kuten A-tietueet eivät toimi liikenteen hallinnan avulla. Voit aina määrittää-toimialueet/toimenpidekokonaisuuksia-domains ostettuja yhden Web App toiseen Web App ja päinvastoin. Katso lisätietoja vaihe 8: ssa. 
    
7. **Valitse** Valitse **Osta toimialueiden** sivu ja valitse näet Osta tiedot **vahvistus** -sivu. Jos hyväksyt ehdot ja valitse **Osta**, tilaus lähetetään ja voit valvoa ostamisen prosessin **ilmoituksen**. Toimialueen hankkiminen voi kestää useita minuutteja. 

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-4.png)

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-5.png)

8. Jos olet tilannut onnistuneesti toimialueen, voit hallita toimialueen ja määrittää web App-sovellukseen. Valitse **"..."** toimialueen oikealla puolella. Sitten voit **peruuttaa hankkiminen** tai **Hallitse toimialueen**. Valitse **Manage domain**, emme sitten sitoa **alitoimialueen** Microsoftin web-sovelluksen asentaminen **hallinta toimialueen** sivu. Jos haluat sitoa **alitoimialueen** eri verkkosovellukseen suorittaa tämän vaiheen vastaaviin Web-sovelluksen yhteydessä. Tässä kautta voit liikenteen hallinta päätepistettä toimialueen määrittäminen (Jos Web App on TM takana) yksinkertaisesti valitsemalla liikenteen hallinta voit nimetä avattavasta valikosta. Näin toimialueen/alitoimialueen määritetään automaattisesti, kaikki kyseisen liikenteen hallinta päätepisteen takana verkkosovelluksissa. 

    ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-6.png)

    > [AZURE.NOTE] Voit "peruuttaa hankkiminen" täyden tuen 5 päivän kuluessa. Jälkeen 5 päivää, se ei voi "Peruuta hankkiminen", sen sijaan näet "Poista" toimialueen sähköpostit. Toimialueen poistaminen aiheuttaa vapauttaminen tilauksesta ilman tuen ja tulee käytettävissä toimialueen. 

Kun määritys on valmis, web-sovelluksen **Hostname sidontojen** -osassa näkyvät mukautettua toimialuenimeä.

Tässä vaiheessa sinun pitäisi mukautettua toimialuenimeä selaimessa ja nähdä, että se onnistuu siirryt web Appissa.
 
## <a name="what-happens-to-the-custom-domain-you-bought"></a>Mitä tapahtuu hankkimasi mukautettu toimialue

**Mukautetut toimialueet ja SSL** -sivu hankkimasi mukautettu toimialue on yhdistetty Azure-tilaukseen. Azure resurssiksi tämän mukautetun toimialueen on erillinen ja riippumaton App Service-sovellusta, joka ostit toimialueen. Tämä tarkoittaa, että:

- Azure-portaalissa voit käyttää hankkimasi mukautettu toimialue useamman kuin yhden sovelluksen palvelun sovelluksen, sekä eikä vain sovellusta, joka hankkimasi mukautettu toimialue. 
- Voit hallita mukautettujen toimialueiden ostit Azure tilauksen siirtymällä **Mukautetut toimialueet ja SSL** -sivu, *kaikki* kyseisen tilauksen App Service-sovellusta.
- Voit määrittää minkä tahansa sovelluksen palvelun sovelluksen saman Azure tilauksesta alitoimialueen mukautetun toimialueen sisältämät.
- Jos päätät, voit poistaa sovelluksen palvelun sovelluksen, voit valita ei, jos haluat poistaa mukautetun toimialueen, se on sidottu voit edelleen käyttää sitä muissa sovellusten.

## <a name="if-you-cant-see-the-custom-domain-you-bought"></a>Jos et näe mukautetun toimialueen ostit

Jos olet ostanut-mukautetun toimialueessa **Mukautetut toimialueet ja SSL** -sivu, mutta eivät näy **hallitut toimialueet**-kohdassa mukautettu toimialue, tarkista seuraavat kohdat:

- Mukautetun toimialueen luominen ei ole valmis. Tarkista ilmoituksen Bellin Azure portaalin ylälaidassa edistymisen.
- Mukautetun toimialueen luominen on voinut jostain syystä. Tarkista ilmoituksen Bellin Azure portaalin ylälaidassa edistymisen.
- Mukautettua toimialuetta voi olla onnistui, mutta sivu ei voi päivittää vielä. Yritä avata uudelleen **Mukautetut toimialueet ja SSL** -sivu.
- Olet ehkä poistanut joskus mukautettua toimialuetta. Tarkista napsauttamalla **asetukset**valvontalokien > **Valvonta lokit** -sovelluksen käyttäjän ensisijaisen sivu. 
- Sovellus, joka on luotu eri Azure tilauksen voi kuulua etsit **Mukautetut toimialueet ja SSL** -sivu. Vaihda eri tilauksen toiseen sovellukseen ja tarkista sen **Mukautetut toimialueet ja SSL** -sivu.  
  Portaalin et voi nähdä tai eri Azure-tilauksen kuin sovellus luotujen mukautettujen toimialueiden hallinta. Jos valitset **Mukautettu hallinta** toimialueen **Manage domain** sivu, sinut ohjataan toimialueen kehittäjän sivustoon, jossa voit tarvittaessa määrittäminen   [manuaalisesti mukautetun toimialueen, kuten kaikki ulkoiset mukautetun toimialueen](web-sites-custom-domain-name.md)  
   sovellusten luotu eri Azure-tilauksen. 



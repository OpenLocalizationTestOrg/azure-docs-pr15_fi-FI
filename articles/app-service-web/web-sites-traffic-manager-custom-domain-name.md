<properties
    pageTitle="Määritä Azure-sovelluksen palvelun, joka käyttää liikenteen hallinta kuormituksen tasaamisen verkkosovellukseen mukautettua toimialuenimeä."
    description="Käytä mukautetun toimialuenimeä Azure-sovelluksen palvelun, joka sisältää liikenteen hallinta kuormituksen tasaamisen verkkosovellukseen."
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
    ms.date="09/20/2016"
    ms.author="robmcm"/>

# <a name="configuring-a-custom-domain-name-for-a-web-app-in-azure-app-service-using-traffic-manager"></a>Määrittäminen käyttämään mukautettua toimialuenimeä verkkosovellukseen Azure App palvelu liikenteen hallinta

[AZURE.INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

[AZURE.INCLUDE [intro](../../includes/custom-dns-web-site-intro-traffic-manager.md)]

Tässä artikkelissa on yleisiä käyttöohjeet mukautettua toimialuenimeä Azure-sovelluksen ominaisuudet, jotka käyttävät liikenteen hallinta kuormituksen tasaamisen.

[AZURE.INCLUDE [tmwebsitefooter](../../includes/custom-dns-web-site-traffic-manager-notes.md)]

[AZURE.INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]

<a name="understanding-records"></a>
## <a name="understanding-dns-records"></a>Perustietoja DNS-tietueet

[AZURE.INCLUDE [understandingdns](../../includes/custom-dns-web-site-understanding-dns-traffic-manager.md)]

<a name="bkmk_configsharedmode"></a>
## <a name="configure-your-web-apps-for-standard-mode"></a>Web Apps-sovellusten vakio-tilan määrittäminen

[AZURE.INCLUDE [modes](../../includes/custom-dns-web-site-modes-traffic-manager.md)]

<a name="bkmk_configurecname"></a>
## <a name="add-a-dns-record-for-your-custom-domain"></a>Mukautetun toimialueen DNS-tietueen lisääminen

> [AZURE.NOTE] Jos olet ostanut toimialueen Azure palvelun Web sovellukset – ohjeita noudattamalla voit ohittaa ja viittaa [Osta toimialue Web Apps](custom-dns-web-site-buydomains-web-app.md) -artikkelin viimeisessä vaiheessa.

Mukautetun toimialueen liitettävä verkkosovellukseen Azure-sovelluksen käytössä, sinun on lisättävä uuden tekstin DNS-taulukon mukautetun toimialueen toimialueen rekisteröintipalvelu, jota olet ostanut toimialuenimi-työkalujen avulla. Seuraavien vaiheiden avulla voit etsiä ja käyttää DNS-työkaluja.

1. Kirjaudu tiliisi toimialueen rekisteröintipalvelussa ja Etsi-sivun DNS-tietueiden hallintaan. Etsi linkkejä tai **Toimialuenimi**, **DNS**- tai **Name Server Management**merkintä sivuston alueille. Usein tämän sivun linkki löytyy tarkasteleminen tilitietojen ja esittää sitten linkin, kuten **omien toimialueiden**.

1. Kun olet löytänyt hallinta-sivulla toimialueen nimi, Etsi linkin, jonka avulla voit muokata DNS-tietueita. Tämä saattaa näkyä **Zone file**- **DNS-tietueet**, tai **Lisäasetukset** -määritysten linkkinä.

    * Sivun todennäköisesti on jo luotu, kuten tekstin liittäminen muutama tietue '**@**"tai"\*' "toimialueen pysäköinti"-sivu. Voi sisältää myös yleisiä alitoimialueisiin, kuten **www**-tietueet.
    * Sivun mainita **CNAME-tietueet**tai antaa avattavasta tietueen tyyppi. Se myös mainita muita tietueita, kuten **tietueisiin** ja **MX-tietueita**. Joissakin tapauksissa CNAME-tietueet kutsua muut nimet, kuten **Tietueen Alias**.
    * Sivulla on myös kenttiä, jotka mahdollistavat **karttaan** **isäntänimi** tai **toimialuenimen** toinen verkkotunnus.

1. Kun kunkin rekisteröintipalvelu tiedot vaihtelevat yleinen yhdistät *-* mukautettua toimialuenimeä (esimerkiksi **contoso.com**-) *,* liikenteen hallinta toimialuenimi (**contoso.trafficmanager.net**), jota käytetään web Appissa.

    > [AZURE.NOTE] Voit myös jos tietue on jo käytössä ja haluat sitoa synkronointiraja siihen sovelluksia, voit luoda lisää CNAME-tietue. Esimerkiksi **www.contoso.com** haluat sitoa synkronointiraja web App-sovellukseen, luoda CNAME-tietue **awverify.www** , **contoso.trafficmanager.net**. Voit lisätä "www.contoso.com" sitten koodiin muuttamatta "www" CNAME-tietue. Lisätietoja on kohdassa [DNS-tietueiden luominen mukautetun toimialueen web-sovelluksen][CREATEDNS].

1. Kun olet tehnyt lisääminen tai muokkaaminen rekisteröintipalvelussa DNS-tietueet, Tallenna muutokset.

<a name="enabledomain"></a>
## <a name="enable-traffic-manager"></a>Ota käyttöön liikenteen hallinta

[AZURE.INCLUDE [modes](../../includes/custom-dns-web-site-enable-on-traffic-manager.md)]

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja on artikkelissa [Node.js Developer Center](/develop/nodejs/).

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[CREATEDNS]: ../dns/dns-web-sites-custom-domain.md

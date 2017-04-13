<properties
   pageTitle="Logiikan sovelluksissa Azure resurssin Connectorin avulla | Sovelluksen Microsoft Azure-palvelu"
   description="Voit luoda ja Määritä Azure resurssin yhdistimen tai API-sovellus ja käytä Azure-sovelluksen palvelun logiikan-sovelluksessa"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="stepsic-microsoft-com"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="09/01/2016"
   ms.author="stepsic"/>

# <a name="get-started-with-the-azure-resource-connector-and-add-it-to-your-logic-app"></a>Azure resurssin yhdistimen käytön aloittaminen ja lisää se logiikan-sovellukseen
>[AZURE.NOTE] Tässä versiossa on artikkelissa koskee logiikan sovellusten 2014 – 12-01 – esikatselu rakenteen versio.

Azure resurssin yhdistimen avulla voit hallita helposti sisältyy logiikan sovelluksen Azure resurssit.

## <a name="create-the-azure-resource-connector"></a>Azure resurssin yhdistimen luominen
Voit käyttää Azure resurssin yhdistimen API-sovellusta, haluat luoda ensimmäisen esiintymän, se. Tämä voidaan toteuttaa joko sisäisen logiikan app luotaessa tai valitsemalla Azure resurssin Manager yhdistimen API-sovelluksen Azure Marketplacesta.

Voit määrittää sen, sinun on sinun on määritettävä ylös palvelun lyhennys oikeuksilla voit tehdä, joista on Azure ei. Kaikki kutsut tehdään--puolesta-, tämä palvelu lyhennys, voit määrittää. Näin voit käyttää vain täsmälleen mitä haluat tehdä yhdistimen laajuus eikä mitään Lisää.

David Ebbo on kirjoitettu [hyvien blogimerkintä](http://blog.davidebbo.com/2014/12/azure-service-principal.html) siitä, miten voit määrittää tämän. Seuraa kaikkien ohjeita ja **Vuokraajan tunnus**, **Ostajantunnus** ja **salaisuus**. Seuraavat kolme kenttää sekä **Tilaustunnus**ovat mitä tarvitaan määrittäminen yhdistin.

## <a name="using-the-azure-resource-connector-in-logic-apps-designer"></a>Logiikan sovellusten suunnittelussa Azure resurssin Connectorin avulla
### <a name="trigger"></a>Käynnistin
On kaksi käynnistimien, joita tuetaan yhdistin:

Nimi | Kuvaus
---- | -----------
Tapahtuman ilmetessä | Käynnistimen tapahtuman toteutuessa resurssille-tilaukseesi.
Metrijärjestelmän ylittää kynnysarvo |  Käynnistin, kun mittarin täyttää tietty määrä.

### <a name="action"></a>Toiminto

Vastaavasti suuri määrä toimintoja voidaan lisätä sisällä Azure tilauksen:

**Resurssin** ryhmien voit tehdä seuraavaa:

Nimi | Kuvaus
---- | -----------
Luettelon resurssiryhmät | Luettelo kaikista ylläpitosopimus resurssiryhmät.
Hae resurssiryhmä | Voit saada resurssiryhmä sen tunnus.
Luo resurssiryhmä | Luo tai päivitä resurssiryhmä.
Poista resurssiryhmä | Poista resurssiryhmä.

**Resurssien** voit tehdä seuraavaa:

Nimi | Kuvaus
---- | -----------
Resurssit | Resurssit-tilauksen mukaan erityyppisiä suodattimet.
Hae resurssi | Hae koottuja sen resurssin tunnus.
Luo tai päivitä resurssi | Luo resurssi tai Päivitä aiemmin luotu resurssi. Sinun on määritettävä kaikki ominaisuudet kyseiselle resurssille.
Resurssi-toiminto |  Suorittaa muita toimintoja resurssille. Sinun tarvitsee tietää toiminnon nimi ja tiedot, jotka tämä toiminto avaa (jos saatavilla).
Poista resurssi | Resurssin poistaminen.

**Resurssin** toimittajien voit tehdä seuraavaa:

Nimi | Kuvaus
---- | -----------
Luettelon resurssien tarjoajat | Luettelo kaikista käytettävissä resurssi-tarjoajan tilaus.

**Resurssien ryhmä** -versioiden voit tehdä seuraavaa:

Nimi | Kuvaus
---- | -----------
Luettelon käyttöönotoissa | Luettelo kaikista käyttöönotoissa resurssi-ryhmässä.
Hae käyttöönotto | Voit saada mallin käyttöönottoa sen tunnus.
Luo käyttöönotto | Luo uusi resurssi ryhmän käyttöönoton antamalla mallin.

**Tapahtumien** resursseista, voit tehdä seuraavia toimia:

Nimi | Kuvaus
---- | -----------
Hae tapahtumat | Hae tapahtumat, tilauksen tai resurssi.

**Arvot** resursseista, voit tehdä seuraavaa:

Nimi | Kuvaus
---- | -----------
Hae arvot | Hae mittarin resurssin tunnus.

## <a name="do-more-with-your-connector"></a>Älä lisää ja sitten yhdistimen
Yhdistimen on luotu, voit lisätä sen business vuo logiikan sovellusta käytettäessä. Katso [mitä logiikan sovelluksia?](app-service-logic-what-are-logic-apps.md).

>[AZURE.NOTE] Jos haluat aloittaa Azure logiikan sovellukset ennen rekisteröimässä Azure-tili, siirry [Yritä logiikan sovellus](https://tryappservice.azure.com/?appservice=logic), jossa lyhytkestoinen starter logiikan-sovelluksen heti voit luoda sovelluksen-palvelussa. Ei ole pakollinen; luottokortit ei ole sitoumukset.

Tarkastele osoitteessa Swagger REST API-viittaus [yhdistimien ja API sovellusten viittaus](http://go.microsoft.com/fwlink/p/?LinkId=529766).

<!--References -->

<!--Links -->
[Creating a Logic app]: app-service-logic-create-a-logic-app.md

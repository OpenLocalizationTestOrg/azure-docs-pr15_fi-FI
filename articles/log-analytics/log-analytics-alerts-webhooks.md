<properties
   pageTitle="Lokitiedoston Analytics ilmoitusten webhook Esimerkki"
   description="Voit suorittaa Log Analytics-ilmoitus vastauksen toiminnot on *webhook*, jonka avulla voit käynnistää ulkoinen toiminto läpi yksi HTTP-pyynnön. Tässä artikkelissa käydään läpi Esimerkki luominen webhook-toiminnon käyttäminen liukuma Log Analytics-ilmoituksen."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/27/2016"
   ms.author="bwren" />

# <a name="webhooks-in-log-analytics-alerts"></a>Lokitiedoston Analytics-ilmoitukset Webhooks

Voit suorittaa vastauksen [Log Analytics ilmoitus](log-analytics-alerts.md) toiminnot on *webhook*, jonka avulla voit käynnistää ulkoinen toiminto läpi yksi HTTP-pyynnön.  Voit lukea tietoja ilmoituksista ja webhooks [ilmoitusten Log Analytics](log-analytics-alerts.md) -tiedot

Tässä artikkelissa Käymme tässä läpi Esimerkki luominen webhook-toiminnon käyttäminen liukuma on tekstiviesti palvelun Log Analytics-ilmoituksen.

>[AZURE.NOTE] Sinulla on suoritettava tässä esimerkissä liukuma tilin.  Voit rekisteröidä ilmainen tili osoitteessa [slack.com](http://slack.com).

## <a name="step-1---enable-webhooks-in-slack"></a>Vaihe 1 – Ota käyttöön webhooks-liukuma
2.  Kirjaudu sisään osoitteessa [slack.com](http://slack.com)aloituksista.
3.  Valitse kanavan vasemmanpuoleisessa ruudussa **kanavat** -osassa.  Tämä on kanavan, viesti lähetetään.  Voit valita jonkin oletusarvon kanavan kuten **Yleiset** tai **satunnainen**.  Tuotannon tilanteessa loisi todennäköisesti määräten kanavaa, kuten **criticalservicealerts**. <br>

    ![Liukuman kanavat](media/log-analytics-alerts-webhooks/oms-webhooks01.png)

3. Avaa valitsemalla **Lisää sovellus- tai mukautetun integrointi** Sovelluskirjaston.
3.  Kirjoita hakuruutuun *webhooks* ja valitse sitten **Saapuvien WebHooks**. <br>

    ![Liukuman kanavat](media/log-analytics-alerts-webhooks/oms-webhooks02.png)

4.  Valitse **Asenna** ryhmän nimen vieressä.
5.  Valitse **Lisää määritykset**.
6.  Valitse joita aiot käyttää tätä esimerkiksi ja valitse sitten **Lisää saapuvan WebHooks integrointi**kanava.  
6. Kopioi **Webhook URL-osoite**.  Sinun olla liittäminen tämä loput määritykset. <br>

    ![Liukuman kanavat](media/log-analytics-alerts-webhooks/oms-webhooks05.png)

## <a name="step-2---create-alert-rule-in-log-analytics"></a>Vaihe 2: luo hälytyksen Log Analytics
1.  Seuraavilla asetuksilla [ilmoitusten säännön luominen](log-analytics-alerts.md) .
    - Kyselyn:```    Type=Event EventLevelName=error ```
    - Tarkista tämän ilmoituksen jokaisen: 5 minuuttia
    - Tulosten määrä on: suurempi kuin 10
    - Aikaikkunan päälle: 60 minuuttia
    - Valitse **Kyllä** **Webhook** ja **ei** muita toimintoja.
7. Liitä liukuma URL-osoite **Webhook URL-osoite** -kenttään.
8. Valitse vaihtoehto, jos haluat **lisätä mukautetun JSON-paketti**.
9. Odottaa liukuma paketti, joka on muotoiltu JSON-parametrin *teksti*.  Tämä on teksti, jossa se näkyy se luo viesti.  Voit käyttää vähintään yksi seuraavista ilmoitusten parametreja *#* symboli esimerkiksi seuraavan esimerkin mukaisesti.

    ```
    {
    "text":"#alertrulename fired with #searchresultcount records which exceeds the over threshold of #thresholdvalue ."
    }
    ```

    ![Esimerkki JSON-paketti](media/log-analytics-alerts-webhooks/oms-webhooks07.png)

9.  Valitse Tallenna hälytyksen **Tallenna** .

10. Odota ilmoituksen voi luoda, ja tarkista sitten viestistä, joka on seuraavankaltaiselta liukuma riittävästi aikaa.

    ![Esimerkki webhook liukuma-](media/log-analytics-alerts-webhooks/oms-webhooks08.png)


### <a name="advanced-webhook-payload-for-slack"></a>Laajennettu webhook-paketti, liukuma

Voit mukauttaa yleisesti liukuma saapuvia viestejä. Lisätietoja [Saapuviin Webhooks](https://api.slack.com/incoming-webhooks) liukuma-sivustossa. Seuraavassa on monimutkaisia paketti, voit luoda monipuolisia viestin muotoilua:

    {
        "attachments": [
            {
                "title":"OMS Alerts Custom Payload",
                "fields": [
                    {
                        "title": "Alert Rule Name",
                        "value": "#alertrulename"},
                    {
                        "title": "Link To SearchResults",
                        "value": "<#linktosearchresults|OMS Search Results>"},
                    {
                        "title": "Search Interval",
                        "value": "#searchinterval"},
                    {
                        "title": "Threshold Operator",
                        "value": "#thresholdoperator"},
                    {
                        "title": "Threshold Value",
                        "value": "#thresholdvalue"}
                ],
                "color": "#F35A00"
            }
        ]
    }


Tämä luo viestin liukuma seuraavankaltaiselta.

![Esimerkki viestin liukuma](media/log-analytics-alerts-webhooks/oms-webhooks09.png)

## <a name="summary"></a>Yhteenveto

Tämä ilmoitus säännöllä paikassa sinun on aina, kun ehto täyttyy liukuma lähetetty viesti.  

Tämä on vain yksi esimerkki toiminto, jonka voit luoda ilmoituksen vastauksen.  Voit luoda webhook-toiminnon, joka kutsuu toiseen ulkoiseen palveluun runbookin-toiminto, joka käynnistää runbookin Azure automaatio ja sähköposti-toiminnon sisältävän sähköpostiviestin lähettäminen itselle ja muille vastaanottajille.   

## <a name="next-steps"></a>Seuraavat vaiheet

- Tietoja [Log Analytics ilmoitukset](log-analytics-alerts.md) myös muita toimintoja.
- [Luo runbooks Azure automaatio-](../automation/automation-webhooks.md) , joka voidaan kutsua webhook.

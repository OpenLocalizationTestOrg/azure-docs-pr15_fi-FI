<properties 
   pageTitle="Seurata access ja suorituskyvyn lokit ja arvot sovelluksen Gatewayn | Microsoft Azure"
   description="Opettele ottaminen käyttöön ja hallita sovelluksen Gatewayn Access ja suorituskyvyn lokit"
   services="application-gateway"
   documentationCenter="na"
   authors="amitsriva"
   manager="rossort"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags 
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/26/2016"
   ms.author="amitsriva" />

# <a name="diagnostics-logging-and-metrics-for-application-gateway"></a>Diagnostiikan kirjaus ja sovelluksen yhdyskäytävän mittaukset

Azure tarjoaa mahdollisuuden seurata resurssien kirjaaminen ja arvot

[**Lokiin kirjaaminen**](#enable-logging-with-powershell) - kirjaaminen mahdollistaa suorituskyvyn, access ja muut lokien tallentaminen tai kulutettu resurssin seurantaa varten.

[**Arvot**](#metrics) - sovelluksen gateway tällä hetkellä on yksi arvo. Tämä arvo mittaa siirtonopeuden sovelluksen yhdyskäytävän tavuina sekunnissa.

Voit erityyppisiä lokit Azure hallintaa ja sovelluksen yhdyskäytävien vianmääritys. Jotkin lokitiedostot niitä voi käyttää portaalin kautta ja kaikki lokeja voidaan poimittujen Azure-blob-säiliö, ja tarkastella eri työkaluja, kuten [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md)-, Excel- ja PowerBI. Lisätietoja on tietoja eri tyyppiä lokit seuraavasta luettelosta:

- **Valvontalokit:** [Azure valvontalokien](../monitoring-and-diagnostics/insights-debugging-with-events.md) (aiemmalta nimeltään toiminnallisia lokit) avulla voit tarkastella lähetettävä Azure tilaus ja tilansa kaikki toiminnot. Valvontalokien ovat oletusarvoisesti käytössä, ja voit tarkastella Azure esikatselu-portaalissa.
- **Käyttää lokit:** Tämä loki avulla voit tarkastella sovelluksen yhdyskäytävän käyttötapa ja analysoida tärkeitä tietoja, mukaan lukien soittajan IP-URL-Osoitteen pyydettäessä vastauksen viive palauttaa koodi, tavua sisään ja ulos. Access-loki kerätään 300 sekunnin välein. Tämä loki sisältää yhden tietueen sovelluksen yhdyskäytävän esiintymässä. Sovelluksen yhdyskäytävän esiintymän voidaan tunnistaa esiintymän "tunnus"-ominaisuutta.
- **Suorituskyvyn lokit:** Voit tarkastella, kuinka sovelluksen yhdyskäytäväesiintymästä koostuva suorittamassa lokia. Lokia sieppaa Suorituskykytiedot kohti esiintymän välein yhteensä pyynnön served, mukaan lukien siirtonopeuden tavuina-pyyntöjen kokonaismäärä served epäonnistuneen pyynnön Laske kunnossa ja perusasemassa taustatietokantaan esiintymän määrä. Suorituskyvyn log kerätään 60 sekunnin välein.
- **Palomuuri lokit:** Tämä loki avulla voit tarkastella pyyntöjä, joka on kirjautunut tunnistus tai estäminen tilan yhdyskäytävän sovellus, joka on määritetty web-sovelluksen palomuurin läpi.

>[AZURE.WARNING] Lokit ovat käytettävissä vain resurssien Resurssienhallinta käyttöönoton mallin käyttöön. Et voi käyttää lokit resurssien perinteinen käyttöönotto-mallissa. Saat paremman käsityksen kaksi mallia, viittaus on artikkelissa [tietoja resurssien hallinnan käyttöönotto- ja perinteinen käyttöönotto](../resource-manager-deployment-model.md) .

## <a name="enable-logging-with-powershell"></a>Kirjaaminen PowerShellin avulla

Valvontalokit on automaattisesti käytössä Resurssienhallinta resurssi. Sinun on otettava käyttöön, access ja lokiin kirjaaminen käyttöön kyseiset lokit kautta tietojen kerääminen suorituskyvyn. Voit ottaa kirjaamisen käyttöön, katso seuraavasti: 

1. Huomautus tallennustilan-tilisi Resurssitunnus-lokitiedot tallennuspaikan. Tämä on lomakkeen: /subscriptions/\<subscriptionId\>/resourceGroups/\<resurssiryhmän nimi\>/providers/Microsoft.Storage/storageAccounts/\<tallennustilan tilin nimi\>. Tallennustilan tilien tilaukseesi voidaan. Esikatselu-portaalin avulla voit etsiä tiedot.

    ![Esikatsele portal - sovellus yhdyskäytävän diagnostiikka](./media/application-gateway-diagnostics/diagnostics1.png)

2. Huomautus sovelluksen yhdyskäytävän Resurssitunnus mitä kirjaaminen on käyttöön. Tämä on lomakkeen: /subscriptions/\<subscriptionId\>/resourceGroups/\<resurssiryhmän nimi\>/providers/Microsoft.Network/applicationGateways/\<sovelluksen yhdyskäytävän nimi\>. Esikatselu-portaalin avulla voit etsiä tiedot.

    ![Esikatsele portal - sovellus Gateway diagnostiikka](./media/application-gateway-diagnostics/diagnostics2.png)

3. Ota käyttöön vianmäärityksen kirjaus seuraavalla powershell cmdlet-komennolla:

        Set-AzureRmDiagnosticSetting  -ResourceId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name> -StorageAccountId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Storage/storageAccounts/<storage account name> -Enabled $true  

>[AZURE.INFORMATION] valvontalokien eivät edellytä erillisen tallennustilan tilin. Tallennustilan käyttö access ja suorituskyvyn kirjaaminen veloitetaan palvelun.

## <a name="enable-logging-with-azure-portal"></a>Kirjaaminen Azure-portaalissa

### <a name="step-1"></a>Vaihe 1

Siirry Azure-portaalissa resurssi. Valitse **vianmäärityslokit**. Jos tämä on ensimmäinen kerta määrittäminen diagnostiikka sivu näyttää seuraavassa kuvassa:

Sovelluksen Gateway 3 lokit ovat käytettävissä.

- Access-loki
- Suorituskyvyn loki
- Palomuuri loki

Valitse Käynnistä, tietojen kerääminen **diagnostiikka käyttöön** .

![Diagnostiikka-asetusten sivu][1]

### <a name="step-2"></a>Vaihe 2

**Diagnostiikan asetusten** -sivu, miten Epäsuotuisiin vianmäärityslokeihin on määritetty. Tässä esimerkissä Log analytics käytetään Tallenna lokit. **Määritä** kohdasta **Log Analytics** työtilan määrittämiseen. Tapahtuman keskittimet ja tallennustilaa tilin voidaan myös diagnostiikka lokien tallentaminen.

![Diagnostiikka-sivu][2]

### <a name="step-3"></a>Vaihe 3

Valitse aiemmin luotuun OMS-työtilaan tai Luo uusi tunnus. Tässä esimerkissä käytetään olemassa.

![OMS-työtilat][3]

### <a name="step-4"></a>Vaihe 4

Kun olet valmis, Vahvista asetukset ja valitse Tallenna asetukset **Tallenna** .

![Vahvista valinta][4]

## <a name="audit-log"></a>Valvontaloki

Azure luoda lokia (aiemmalta nimeltään "toiminnallisia lokiin") oletusarvoisesti.  Lokit säilytetään 90 päivää Azure's tapahtumalokien Storessa. Lue lisää lokitiedostot lukemalla artikkelin [tapahtumien tarkasteleminen ja valvontalokit](../monitoring-and-diagnostics/insights-debugging-with-events.md) .

## <a name="access-log"></a>Access-loki

Tämä loki luodaan vain, jos se on otettu käyttöön sovelluksen yhdyskäytävän perusteena edellisten vaiheiden mukaisesti. Tiedot tallennetaan tallennustilan tilin kirjautumiskerralla kirjaus käyttöön. Kunkin sovelluksen yhdyskäytävän access kirjataan JSON-muodossa, tarkastelu seuraavan esimerkin mukaisesti:

    {
        "resourceId": "/SUBSCRIPTIONS/<subscription id>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/<application gateway name>",
        "operationName": "ApplicationGatewayAccess",
        "time": "2016-04-11T04:24:37Z",
        "category": "ApplicationGatewayAccessLog",
        "properties": {
            "instanceId":"ApplicationGatewayRole_IN_0",
            "clientIP":"37.186.113.170",
            "clientPort":"12345",
            "httpMethod":"HEAD",
            "requestUri":"/xyz/portal",
            "requestQuery":"",
            "userAgent":"-",
            "httpStatus":"200",
            "httpVersion":"HTTP/1.0",
            "receivedBytes":"27",
            "sentBytes":"202",
            "timeTaken":"359",
            "sslEnabled":"off"
        }
    }

## <a name="performance-log"></a>Suorituskyvyn loki

Tämä loki luodaan vain, jos se on otettu käyttöön sovelluksen yhdyskäytävän perusteena edellisten vaiheiden mukaisesti. Tiedot tallennetaan tallennustilan tilin kirjautumiskerralla kirjaus käyttöön. Kirjataan seuraavat tiedot:

    {
        "resourceId": "/SUBSCRIPTIONS/<subscription id>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/<application gateway name>",
        "operationName": "ApplicationGatewayPerformance",
        "time": "2016-04-09T00:00:00Z",
        "category": "ApplicationGatewayPerformanceLog",
        "properties": 
        {
            "instanceId":"ApplicationGatewayRole_IN_1",
            "healthyHostCount":"4",
            "unHealthyHostCount":"0",
            "requestCount":"185",
            "latency":"0",
            "failedRequestCount":"0",
            "throughput":"119427"
        }
    }


## <a name="firewall-log"></a>Palomuuri loki

Tämä loki luodaan vain, jos se on otettu käyttöön sovelluksen yhdyskäytävän perusteena edellisten vaiheiden mukaisesti. Lokia edellyttää myös kyseisen web-sovelluksen palomuurin reititystiedot sovelluksen-yhdyskäytävä. Tiedot tallennetaan tallennustilan tilin kirjautumiskerralla kirjaus käyttöön. Kirjataan seuraavat tiedot:

    {
        "resourceId": "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/<applicationGatewayName>",
        "operationName": "ApplicationGatewayFirewall",
        "time": "2016-09-20T00:40:04.9138513Z",
        "category": "ApplicationGatewayFirewallLog",
        "properties":     {
            "instanceId":"ApplicationGatewayRole_IN_0",
            "clientIp":"108.41.16.164",
            "clientPort":1815,
            "requestUri":"/wavsep/active/RXSS-Detection-Evaluation-POST/",
            "ruleId":"OWASP_973336",
            "message":"XSS Filter - Category 1: Script Tag Vector",
            "action":"Logged",
            "site":"Global",
            "message":"XSS Filter - Category 1: Script Tag Vector",
            "details":{"message":" Warning. Pattern match "(?i)(<script","file":"/owasp_crs/base_rules/modsecurity_crs_41_xss_attacks.conf","line":"14"}}
    }

## <a name="view-and-analyze-the-audit-log"></a>Tarkastella ja analysoida valvontaloki

Voit tarkastella ja analysoida valvontalokin tietoja jollakin seuraavista tavoista:

- **Azure työkalut:** Tietojen noutaminen valvontalokien PowerShellin Azure, Azure komentorivillä (CLI), Azure REST-Ohjelmointirajapinnalla ja Azure esikatselu-portaalissa.  Kummassakin menetelmässä vaiheittaiset ohjeet on kuvattu artikkelin [valvonta-toimintojen kanssa Resurssienhallinta](../resource-group-audit.md) .
- **Power BI:** Jos sinulla ei vielä ole [Power BI](https://powerbi.microsoft.com/pricing) -tili, voit kokeilla ilmaiseksi. Käyttämällä [Azure valvontalokien sisällön Power BI-paketti,](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-pack-azure-audit-logs/) voit analysoida, jota voit käyttää esimääritettyjä raporttinäkymien tietojen-on tai mukauttaminen.

## <a name="view-and-analyze-the-access-performance-and-firewall-log"></a>Tarkastella ja analysoida access, suorituskykyä ja palomuuri loki

Azure [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md) voit kerätä laskuri ja tapahtumaloki Blob storage tililtä-tiedostot ja sisältää visualisoinnit ja tehokkaita hakuominaisuuksia lokeista analysointia varten.

Voit muodostaa yhteyden tiliisi tallennustilan ja hakea access ja suorituskyvyn lokit JSON log merkintöjä. Kun olet ladannut JSON-tiedostoja, voit muuntaa ne CSV- ja Näytä Excelissä, PowerBI tai jokin muu tietojen visualisoinnin työkalu.

>[AZURE.TIP] Jos olet tutustunut Visual Studio ja peruskäsitteet muuttaminen vakioita ja C# muuttujat arvot, voit käyttää [lokiin muuntimen Työkalut](https://github.com/Azure-Samples/networking-dotnet-log-converter) Github käytettävissä.

## <a name="metrics"></a>Arvot

Arvot on tiettyjä Azure resursseja, jolla voit tarkastella suorituskyvyn laskureita portaalissa ominaisuuden. Sovelluksen yhdyskäytävää yksi arvo on käytettävissä milloin kirjoittamisen tässä artikkelissa. Tämä arvo on siirtonopeuden ja näkevät portaalissa. Siirry yhdyskäytävän sovellus ja valitse **arvot**.  Valitse **käytettävissä olevat arvot** -osan, voit tarkastella arvot siirtonopeuden. Seuraavassa kuvassa näet Esimerkki suodattimia, joiden avulla voidaan näyttää tiedot eri alueiden kanssa.

Nykyinen luettelo tukevat arvot näkyviin käy [Tuetut arvot Azure näytön kanssa](../monitoring-and-diagnostics/monitoring-supported-metrics.md)

![metrijärjestelmän näkymä][5]

## <a name="alert-rules"></a>Ilmoitusten säännöt

Ilmoitusten säännöt voidaan aloittaa perusteella, valitse resurssi-arvojen mukaisesti. Tämä tarkoittaa sovelluksen Gateway ilmoituksen voit soittaa webhook tai sähköpostin järjestelmänvalvoja, jos sovelluksen yhdyskäytävän siirtonopeuden on yläpuolelle, alapuolelle tai osoitteessa kynnysarvo määritetyn ajanjakson aikana.

Seuraavassa esimerkissä edetään ohjatusti ilmoitusten säännön, joka lähettää sähköpostin järjestelmänvalvoja, kun siirtonopeuden raja-arvo on laiminlyödyksi luominen.

### <a name="step-1"></a>Vaihe 1

Valitse Aloita **Lisää metrisillä ilmoitus** . Tämä sivu voi siirtyä myös arvot-sivu.

![ilmoitusten säännöt-sivu][6]

### <a name="step-2"></a>Vaihe 2

**Lisää sääntö** -sivu-täytä nimi ehtoon, ja ilmoittaa osat ja valitse **OK** , kun olet valmis.

**Ehto** -valitsin sallii 4 arvoja, **suurempi kuin**, **suurempi tai yhtä kuin**, **pienempi kuin**tai **pienempi tai yhtä suuri kuin**.

**Piste** -valitsin sallii poiminnan ajan 5 minuuttia 6 tuntia.

**Sähköpostin omistajat, osallistujat, ja lukijat** valitsemalla sähköpostin voi olla dynaaminen käyttäjät, joilla on pääsy kyseiselle resurssille perusteella. Muussa pilkuilla erotettu luettelo käyttäjistä voidaan suorittaa **muita järjestelmänvalvojan email(s)** -tekstiruutuun.

![Lisää sääntö-sivu][7]

Jos kynnysarvo tulee vika, sähköpostia saapuu samanlainen kuin seuraavan kuvan mukaisesti:

![raja-arvon vakuudesta sähköposti][8]

Ilmoitusten luettelo on näkyvissä, kun metrisillä ilmoitus on luotu ja esitetään yleiskatsaus ilmoitusten säännöt.

![hälytyksen näkymä][9]

Lisätietoja ilmoitusten käy [Vastaanota ilmoitukset](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

Selvittääksesi lisätietoja webhooks ja miten voit käyttää niitä ilmoituksista, käy [webhook Azure metrisillä ilmoituksessa määrittäminen](../monitoring-and-diagnostics/insights-webhooks-alerts.md)

## <a name="next-steps"></a>Seuraavat vaiheet

- Laskuri- ja tapahtumalokien kanssa [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md) visualisointi 
- [Visualisoi Power BI Azure oman valvontalokien](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) blogimerkintä.
- [Näkymä ja analysoida Azure valvontalokien Power BI-ja lisää](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) blogimerkintä.

[1]: ./media/application-gateway-diagnostics/figure1.png
[2]: ./media/application-gateway-diagnostics/figure2.png
[3]: ./media/application-gateway-diagnostics/figure3.png
[4]: ./media/application-gateway-diagnostics/figure4.png
[5]: ./media/application-gateway-diagnostics/figure5.png
[6]: ./media/application-gateway-diagnostics/figure6.png
[7]: ./media/application-gateway-diagnostics/figure7.png
[8]: ./media/application-gateway-diagnostics/figure8.png
[9]: ./media/application-gateway-diagnostics/figure9.png
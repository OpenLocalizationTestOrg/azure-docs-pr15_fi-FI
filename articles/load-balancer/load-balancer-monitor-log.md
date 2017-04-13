<properties
   pageTitle="Valvoa toimintoja, laskureita ja tapahtumien kuormituksen | Microsoft Azure"
   description="Opettele käyttöön ilmoitusten tapahtumat ja tutkia Azure kuormituksen kunto tila kirjaaminen"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="log-analytics-for-azure-load-balancer-preview"></a>Lokitiedoston analysointitietoja Azure kuormituksen (ennakkoversio)

Voit erityyppisiä lokit Azure hallinta ja vianmääritys kuormituksen tasoitusmääritykset. Joitain näistä lokeista niitä voi käyttää portaalin kautta. Kaikki lokeja voidaan poimittujen Azure-blob-säiliö ja tarkastella eri työkaluja, kuten Excel- ja PowerBI. Voit lukea lisää eri tyyppiä lokit alla olevasta luettelosta.

- **Valvontalokit:** [Azure valvontalokien](../../articles/monitoring-and-diagnostics/insights-debugging-with-events.md) (aiemmalta nimeltään toiminnallisia lokit) avulla voit tarkastella lähetettävä Azure tilauksistasi ja niiden tiloista kaikki toiminnot. Valvontalokien ovat oletusarvoisesti käytössä, ja voit tarkastella Azure-portaalissa.
- **Ilmoita tapahtumalokien:** Tämä loki avulla voit tarkastella tietoja kuormituksen ilmoitukset ovat korotettuna. Kuormituksen tila kerätään viiden minuutin välein. Lokia kirjoitetaan vain, jos ilmoitusten kuormituksen tasauspalvelun-tapahtuma käynnistetään.
- **Kunto näytteenottimen lokit:** Voit tarkistaa näytteenottimen kunto Tarkista tila, kuinka monta esiintymät ovat online kuormituksen tasauspalvelun taustatietokantaan ja vastaanottaa verkkoliikennettä kuormituksen näennäiskoneiden prosenttiosuus lokia. Tämä loki kirjoitetaan näytteenottimen tila tapahtuman muutosten.

>[AZURE.IMPORTANT] Kirjaudu analytics toimii tällä hetkellä vain Internet vastakkaisten kuormituksen tasoitusmääritykset. Lokit ovat käytettävissä vain resurssien Resurssienhallinta käyttöönoton mallin käyttöön. Et voi käyttää lokit resurssien perinteinen käyttöönotto-mallissa. Saat lisätietoja käyttöönotto-mallien [tietoja resurssien hallinnan käyttöönotto- ja perinteinen käyttöönotto](../../articles/resource-manager-deployment-model.md).

## <a name="enable-logging"></a>Kirjaamisen ottaminen käyttöön

Valvontalokit on automaattisesti käytössä Resurssienhallinta resurssi. Sinun on otettava käyttöön tapahtuma- ja kuntotietojen näytteenottimen kirjaaminen käyttöön kyseiset lokit kautta tietojen kerääminen. Seuraavien vaiheiden avulla voit ottaa kirjaamisen käyttöön.

Kirjaudu sisään [Azure portal](http://portal.azure.com). Jos sinulla ei vielä ole kuormituksen [luominen kuormituksen tasauspalvelun](load-balancer-get-started-internet-arm-ps.md) , ennen kuin jatkat.

1. Valitse-portaalissa valitsemalla **Selaa**.
2. Valitse **kuormituksen tasoitusmääritykset**.

    ![Portal - kuormituksen](./media/load-balancer-monitor-log/load-balancer-browse.png)

3. Valitse aiemmin luotu kuormituksen >> **Kaikkia asetuksia**.
4. Kuormituksen nimellä valintaikkunan oikeaan reunaan Siirry **Seuranta**, valitse **Diagnostiikka**.

    ![Portal - kuormituksen tasauspalvelun-asetukset](./media/load-balancer-monitor-log/load-balancer-settings.png)

5. **Valitse **Diagnostiikka** -ruudun kohdassa **tila**.**
6. Valitse **tallennustilan tili**.
7. Valitse **LOKIT**-kohdassa käytössä olevan tallennustilan-tilin tai luoda uuden. Liukusäätimen avulla voit määrittää, kuinka monta päivää tapahtuman dat säilytetään tapahtumalokit. 8. Valitse **Tallenna**.

    ![Portal - diagnostiikka-lokit](./media/load-balancer-monitor-log/load-balancer-diagnostics.png)

>[AZURE.INFORMATION] valvontalokien eivät edellytä erillisen tallennustilan tilin. Käytä tallennustyyppi tapahtuma- ja kuntotietojen näytteenottimen kirjaaminen maksamaan palvelun kulut.

## <a name="audit-log"></a>Valvontaloki

Valvontaloki luodaan oletusarvon mukaan. Lokit säilytetään 90 päivää Azure's tapahtumalokien Storessa. Lue lisää lokitiedostot lukemalla artikkelin [tapahtumien tarkasteleminen ja valvontalokit](../../articles/monitoring-and-diagnostics/insights-debugging-with-events.md) .

## <a name="alert-event-log"></a>Ilmoitusten tapahtumaloki

Tämä loki luodaan vain, jos se on otettu käyttöön kuormituksen tasauspalvelun peruste kohden. Tapahtumat on kirjautunut JSON-muodossa ja tallennettu tallennustilan tilin kirjautumiskerralla kirjaus käyttöön. Seuraavassa on esimerkki tapahtuman.

    {
    "time": "2016-01-26T10:37:46.6024215Z",
    "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
    "category": "LoadBalancerAlertEvent",
    "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
    "operationName": "LoadBalancerProbeHealthStatus",
    "properties": {
        "eventName": "Resource Limits Hit",
        "eventDescription": "Ports exhausted",
        "eventProperties": {
            "public ip address": "40.117.227.32"
        }
    }

JSON tulos näyttää *eventname* -ominaisuutta, joka kuvataan luoda ilmoituksen kuormituksen syy. Tässä tapauksessa luoda ilmoituksen oli vuoksi TCP-portin riittävyysselvityksen aiheutuvat lähteen IP NAT rajoitukset (SNAT).

## <a name="health-probe-log"></a>Kuntotietojen näytteenottimen loki

Tämä loki luodaan vain, jos se on otettu käyttöön kuormituksen tasauspalvelun peruste esitetyllä tavalla yllä kohden. Tiedot tallennetaan tallennustilan tilin kirjautumiskerralla kirjaus käyttöön. 'Tiedot-lokit-loadbalancerprobehealthstatus' säilö luodaan ja kirjataan seuraavat tiedot:

    {
        "records":
        {
            "time": "2016-01-26T10:37:46.6024215Z",
            "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
            "category": "LoadBalancerProbeHealthStatus",
            "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
            "operationName": "LoadBalancerProbeHealthStatus",
            "properties": {
                "publicIpAddress": "40.83.190.158",
                "port": "81",
                "totalDipCount": 2,
                "dipDownCount": 1,
                "healthPercentage": 50.000000
            }
        },
        {
            "time": "2016-01-26T10:37:46.6024215Z",
            "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
            "category": "LoadBalancerProbeHealthStatus",
            "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
            "operationName": "LoadBalancerProbeHealthStatus",
            "properties": {
                "publicIpAddress": "40.83.190.158",
                "port": "81",
                "totalDipCount": 2,
                "dipDownCount": 0,
                "healthPercentage": 100.000000
            }
        }
    }

JSON tulos näyttää ominaisuudet-kentässä näytteenottimen kunnon tila perustiedot. *DipDownCount* -ominaisuus näkyy esiintymät kokonaismäärän taustatietokannan, joka ei saa verkkoliikennettä vuoksi epäonnistui näytteenottimen vastaukset.

## <a name="view-and-analyze-the-audit-log"></a>Tarkastella ja analysoida valvontaloki

Voit tarkastella ja analysoida valvontalokin tietoja jollakin seuraavista tavoista:

- **Azure työkalut:** Tietojen noutaminen valvontalokien PowerShellin Azure, Azure komentorivillä (CLI), Azure REST-Ohjelmointirajapinnalla ja Azure esikatselu-portaalissa. Kummassakin menetelmässä vaiheittaiset ohjeet on kuvattu artikkelin [valvonta-toimintojen resurssien hallinta](../../articles/resource-group-audit.md) .
- **Power BI:** Jos sinulla ei ole [Power BI](https://powerbi.microsoft.com/pricing) -tili, voit kokeilla ilmaiseksi. Käyttämällä [Azure valvontalokien sisällön pack Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs)-voit analysoida esimääritettyjä raporttinäkymien tietojen tai voit mukauttaa näkymää tarpeitasi.

## <a name="view-and-analyze-the-health-probe-and-event-log"></a>Tarkastella ja analysoida kunto näytteenotin ja tapahtumaloki

Haluat muodostaa yhteyden tiliisi tallennustilan ja hakea tapahtuma- ja kuntotietojen näytteenottimen lokit JSON log merkintöjä. Kun olet ladannut JSON-tiedostoja, voit muuntaa ne CSV- ja Näytä Excelissä, PowerBI tai jokin muu tietojen visualisoinnin työkalu.

>[AZURE.TIP] Jos olet tutustunut Visual Studio ja peruskäsitteet muuttaminen vakioita ja C# muuttujat arvot, voit käyttää [lokiin muuntimen Työkalut](https://github.com/Azure-Samples/networking-dotnet-log-converter) Github käytettävissä.

## <a name="additional-resources"></a>Lisäresursseja

- [Visualisoi Power BI Azure oman valvontalokien](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) blogimerkintä.
- [Näkymä ja analysoida Azure valvontalokien Power BI-ja lisää](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) blogimerkintä.

## <a name="next-steps"></a>Seuraavat vaiheet

[Kuormituksen tasauspalvelun keräysputkien ymmärtäminen](load-balancer-custom-probe-overview.md)

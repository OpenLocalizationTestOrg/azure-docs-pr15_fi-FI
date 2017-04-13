<properties
    pageTitle="Office kaikissa ympäristöissä komentorivillä (CLI) avulla voit luoda ilmoituksia Azure-palveluiden | Microsoft Azure"
    description="Rivin komentoliittymä avulla voit luoda Azure ilmoituksia, jotka voivat aiheuttaa ilmoituksia tai automaatio, kun määrittämäsi ehdot täyttyvät."
    authors="rboucher"
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
    ms.date="10/24/2016"
    ms.author="robb"/>

# <a name="use-the-cross-platform-command-line-interface-cli-to-create-alerts-for-azure-services"></a>Office kaikissa ympäristöissä komentorivillä (CLI) avulla voit luoda Azure-palveluiden ilmoitukset

> [AZURE.SELECTOR]
- [Portal](insights-alerts-portal.md)
- [PowerShellin](insights-alerts-powershell.md)
- [CLI](insights-alerts-command-line-interface.md)

## <a name="overview"></a>Yleiskatsaus

Tämän artikkelin avulla voit määrittää Azure hälytykset käyttäen komentorivillä (CLI).

>[AZURE.NOTE] Azure valvonta on uusi nimi "Azure-tiedot" kutsuttiin 2016 25 syyskuuhun asti. Kuitenkin nimitilan ja näin alla olevia komentoja edelleen sisältää "tiedot".

Voit vastaanottaa ilmoituksen seurantaa mittaukset tai Azure services-tapahtumat.

- **Metrijärjestelmän arvot** - ilmoitusten käynnistimet, kun tietyn mittarin arvo ylittää raja-arvon, voit määrittää jompaankumpaan suuntaan. Toisin sanoen se käynnistää sekä kun ehto täyttyy ja valitse sen jälkeen, kun ehto on enää täyty.    
- **Tehtävän tapahtumien poistaminen** - ilmoituksen esimerkkitilanne *jokaisen* tapahtuman tai, vain silloin, kun tietty määrä tapahtumien ilmetä.

Voit määrittää ilmoituksen, kun se seuraavasti:

- Lähetä sähköposti-ilmoitusten palvelun järjestelmänvalvoja ja apuyhteyshenkilöiden
- Sähköpostin lähettäminen uusia sähköpostiviestejä, jotka määrität.
- Kutsu webhook
- Aloita Azure runbookin, (vain-portaalista Azure tällä hetkellä) suorittaminen

Voit määrittää ja ilmoitusten sääntöjä käyttämällä koskevien tietojen hakeminen

- [Azure portal](insights-alerts-portal.md)
- [PowerShellin](insights-alerts-powershell.md)
- [käyttöliittymä (CLI)](insights-alerts-command-line-interface.md)
- [Azure näytön REST-Ohjelmointirajapinnalla](https://msdn.microsoft.com/library/azure/dn931945.aspx)


Näyttöön tulee komennot ohjetta aina kirjoittamalla komennon ja käyttöönotto - Ohjeen lopussa. Esimerkki:

    ```console
    azure insights alerts -help
    azure insights alerts actions email create -help
    ```

## <a name="create-alert-rules-using-the-cli"></a>Ilmoitusten avulla CLI sääntöjen luominen

1. Suorita edellytykset ja Azure kirjautuessasi. Katso [Azure näytön CLI objektit](insights-cli-samples.md). Lyhyesti sanottuna Asenna CLI ja Suorita nämä komennot. Ne pääset kirjautunut sisään, Näytä tilaus ja valmisteleminen suorittamisen Azure näytön komentoja.


    ```console
    azure login
    azure account show
    azure config mode arm

    ```

2.  Voit tehdä aiemmin luotuja sääntöjä, valitse resurssiryhmä-luettelosta seuraavat lomakkeen **azure havainnollistamisen ilmoitusten sääntö luettelossa** *[Valitsimet] &lt;resourceGroup&gt; *

    ```console
    azure insights alerts rule list myresourcegroupname

    ```
3. Jos haluat luoda säännön, on useita keskeistä tietojen ensin.
    - **Resurssitunnus** resurssin haluat ilmoituksen määrittäminen
    - **Metrijärjestelmän määritelmät** käytettävissä kyseiselle resurssille

    Yksi tapa löytää Resurssitunnus on käyttää Azure-portaalia. Jos resurssi on jo luotu, valitse se portaalissa. Seuraava sivu, valitse *Ominaisuudet* *asetukset* -kohdassa. *Resurssitunnus* on seuraava sivu-kentästä. Toinen tapa on käyttää [Azure resurssin Explorer](https://resources.azure.com/).

    Esimerkki resurssitunnus web-sovelluksen on

    ```console
    /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename
    ```

    Hanki luettelo käytettävissä olevat arvot ja yksiköt resurssin edellisessä esimerkissä näiden mittaukset, käytä CLI seuraava komento:  

    ```console
    azure insights metrics list /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename PT1M
    ```

    _PT1M_ on rakeisuuden käytettävissä mittayksiköt (1 minuutin välein). Käyttämällä eri tarkkuudet asetuksilla voit eri metrisillä.


4. Ilmoitusten metrijärjestelmä perustuvan säännön luominen, käytä seuraavan komennon:

    **Azure havainnollistamisen ilmoitusten säännön metrijärjestelmä määrittäminen** *[Valitsimet] &lt;SäännönNimi&gt; &lt;sijainti&gt; &lt;resourceGroup&gt; &lt;windowSize&gt; &lt;operaattori&gt; &lt;kynnysarvo&gt; &lt;targetResourceId&gt; &lt;metricName&gt; &lt;timeAggregationOperator&gt; *

    Seuraava esimerkki määrittää ilmoituksen sivuston resurssille. Ilmoitusten käynnistimet aina, kun se saa liikenteen johdonmukaisesti 5 minuuttia ja uudelleen milloin se saa eikä liikennettä 5 minuuttia.

    ```console
    azure insights alerts rule metric set myrule eastus myreasourcegroup PT5M GreaterThan 2 /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename BytesReceived Total

    ```

5. Voit luoda webhook tai lähettää sähköpostia, kun ilmoituksen käynnistyy, luo ensin sähköposti-ja/tai webhooks. Luo sääntö heti sen jälkeen. Ei voi liittää webhook tai sisältävien sähköpostien jo luonut sääntöjä CLI käyttämällä.

    ```console
    azure insights alerts actions email create --customEmails myemail@contoso.com

    azure insights alerts actions webhook create https://www.contoso.com

    azure insights alerts rule metric set myrulewithwebhookandemail eastus myreasourcegroup PT5M GreaterThan 2 /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename BytesReceived Total
    ```


6. Voit luoda tehtävän lokin tietyn ehdon ilmoituksen, joka käynnistyy, lomakkeen avulla:

    **tietoja ilmoitusten säännön Kirjaudu määrittäminen** *[Valitsimet] &lt;SäännönNimi&gt; &lt;sijainti&gt; &lt;resourceGroup&gt; &lt;operationName&gt; *

    Esimerkki

    ```console
    azure insights alerts rule log set myActivityLogRule eastus myresourceGroupName Microsoft.Storage/storageAccounts/listKeys/action
    ```

    OperationName vastaa tapahtumatyypin tehtävän lokin merkinnän. Esimerkkejä tällaisista tiedostoista ovat *Microsoft.Compute/virtualMachines/delete* ja *microsoft.insights/diagnosticSettings/write*.

    Voit hankkia luettelo mahdollista operationNames PowerShell-komentoa [Get-AzureRmProviderOperation](https://msdn.microsoft.com/library/mt603720.aspx) . Vaihtoehtoisesti voit kyselyyn tehtävän lokiin ja etsiä aiempia toiminnoista, joita haluat luoda ilmoituksen Azure portaalin. Toiminnot, Näytä kuvan loki helpossa muodossa nimet näkyvät. Etsi JSON-merkinnän ja tuoda esiin OperationName-arvo.   

7. Voit varmistaa, että ilmoitukset on luotu oikein katsomalla yksittäisen säännön.

    ```console
    azure insights alerts rule list myresourcegroup --ruleName myrule
    ```

8. Jos haluat poistaa sääntöjä, käytä komentoa lomakkeen:

    **tietoja ilmoitusten säännön poistaminen** [Valitsimet] &lt;resourceGroup&gt; &lt;SäännönNimi&gt;

    Nämä komennot poistaa tämän artikkelin aiemmin luotuja sääntöjä.

    ```console
    azure insights alerts rule delete myresourcegroup myrule
    azure insights alerts rule delete myresourcegroup myrulewithwebhookandemail
    azure insights alerts rule delete myresourcegroup myActivityLogRule
    ```



## <a name="next-steps"></a>Seuraavat vaiheet

* [Saat yleiskatsauksen Azure seuranta](monitoring-overview.md) , mukaan lukien tietotyypeistä voit kerätä ja valvonta.
* Lisätietoja [-ilmoituksia määritettäessä webhooks](insights-webhooks-alerts.md).
* Lisätietoja [Azure automaatio Runbooks](..\automation\automation-starting-a-runbook.md).
* Kerää yksityiskohtaisia hyvin usein arvot palvelun [Yleiskatsaus kerääminen vianmäärityslokit](monitoring-overview-of-diagnostic-logs.md) siirtyä.
* Varmista, että palvelu on käytettävissä ja vastaa [arvot sivustokokoelman yleiskatsaus](insights-how-to-customize-monitoring.md) hakeminen

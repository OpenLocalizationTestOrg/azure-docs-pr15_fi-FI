<properties 
    pageTitle="Voit valvoa pilvipalveluun | Microsoft Azure" 
    description="Lue, miten voit valvoa pilvipalveluihin Azure perinteinen-portaalissa." 
    services="cloud-services" 
    documentationCenter="" 
    authors="rboucher" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/04/2015" 
    ms.author="robb"/>


# <a name="how-to-monitor-cloud-services"></a>Voit valvoa pilvipalveluihin

[AZURE.INCLUDE [disclaimer](../../includes/disclaimer.md)]

Voit valvoa `key` suorituskyvyn mittarit cloud palvelujen Azure perinteinen-portaalissa. Voit määrittää valvonnan mahdollisimman vähän ja yksityiskohtainen kunkin palvelun roolin taso ja voit mukauttaa seurannan näyttää. Yksityiskohtaiset tiedot tallennetaan tallennustilan-tili, jota voi käyttää portaalin ulkopuolella. 

Seurannan näyttää Azure perinteinen portaalissa on erittäin määritettäviä. Voit valita haluat valvoa **valvonta** -sivulla arvot-luettelon arvot ja voit valita, mitkä arvot esitettävät arvot kaavioiden **valvonta** -sivulla ja raporttinäkymiä. 

## <a name="concepts"></a>Käsitteitä

Oletusarvon mukaan mahdollisimman vähän seurantaa varten on annettu uusi pilvipalveluun käyttämällä suorituskyvyn laskureita kerättyjen isäntäkäyttöjärjestelmän helppokäyttöasetuksia roolit esiintymien (näennäiskoneiden). Mahdollisimman vähän arvot ovat suorittimen prosentteina, tiedot-, tiedot ulos, levyn luku siirtonopeuden ja levyn kirjoittaa siirtonopeuden. Määrittämällä yksityiskohtainen seuranta, saat lisää arvot näennäiskoneiden (rooli esiintymät) suorituskyvyn tietojen perusteella. Yksityiskohtainen arvot ottaa käyttöön sovelluksen toimintojen aikana ilmenevät ongelmat tarkempi analyysi.

Oletusarvon mukaan laskuri suorituskykytietoja roolin esiintymistä näyte ja siirtää roolin esiintymästä 3 minuutin välein. Kun otat yksityiskohtainen seuranta, raaka suorituskykytietoja laskuri koostetaan roolin jokaiselle esiintymälle ja roolin esiintymät kunkin roolin yli 5 minuuttia-1 tunti ja 12 tunnin välein. Kootut tiedot on tyhjentää 10 päivän jälkeen.

Kun olet ottanut käyttöön yksityiskohtainen seuranta-kootut tiedot tallennetaan tallennustilan tilin taulukot. Jotta yksityiskohtainen seuranta-roolin, sinun on määritettävä diagnostiikka yhteysmerkkijonon, joka linkittyy tallennustilan tilin. Voit käyttää eri rooleja eri tallennustilan tilit.

Huomaa, että yksityiskohtainen seurannan ottaminen käyttöön kasvattaa tallennustilan tietosäilö, tiedonsiirto ja tallennustilaa tapahtumat liittyvät kustannukset. Mahdollisimman vähän seuranta ei edellytä tallennustilan tilin. Arvot, joka on määritetty mahdollisimman vähän seurantaa tasolla tiedot ei tallenneta tallennustilan-tilisi, vaikka määrität yksityiskohtainen seurannan taso.


## <a name="how-to-configure-monitoring-for-cloud-services"></a>Toimintaohje: määrittäminen seurantaa pilvipalveluihin

Määrittää yksityiskohtainen tai mahdollisimman vähän valvontaa Azure perinteinen portaalin ohjeiden avulla. 

### <a name="before-you-begin"></a>Ennen aloittamista

- Luoda tallennustilan tilin seurannan tietojen tallennusta varten. Voit käyttää eri rooleja eri tallennustilan tilit. Lisätietoja on artikkelissa [Luo tili tallennustilan](/manage/services/storage/how-to-create-a-storage-account/)sivusuuntaa tai Ohje **Tallennustilan tilit**.

- Azure diagnostiikka käyttöön cloud-palvelun roolit. Katso [määrittäminen pilvipalveluihin diagnostiikka](https://msdn.microsoft.com/library/azure/dn186185.aspx#BK_EnableBefore).

Varmista, että diagnostiikka-yhteysmerkkijonon esitä rooli-määritys. Et voi ottaa käyttöön yksityiskohtainen seuranta, kunnes käyttöön Azure diagnostiikka ja diagnostiikka yhteysmerkkijonon sisällyttäminen roolin määrittäminen.   

> [AZURE.NOTE] Projektien kohdistamisen Azure SDK 2,5 ei automaattisesti sisällytetty diagnostiikka-yhteysmerkkijonon project-malli. Näiden projektien sinun on lisättävä diagnostiikka-yhteysmerkkijonon roolin määrittäminen manuaalisesti.

**Voit lisätä diagnostiikka yhteysmerkkijonon manuaalisesti roolin määrittäminen**

1. Avaa Visual Studiossa pilvipalvelussa projekti
2. Kaksoisnapsauta **roolin** Avaa rooli suunnittelu ja valitse **asetukset** -välilehti
3. Etsi nimeltä **Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString**asetus. 
4. Jos tämä asetus ei ole valitse sitten Lisää työnkulkuun ja muuttaa uuden asetukselle **ConnectionString** **Lisää asetus** -painike
5. Yhteysmerkkijonon arvoksi asetetaan valitsemalla **...** -painiketta. Tämä avaa valintaikkunan, jonka avulla voi valita tallennustilan tilin.

    ![Visual Studio asetukset](./media/cloud-services-how-to-monitor/CloudServices_Monitor_VisualStudioDiagnosticsConnectionString.png)

### <a name="to-change-the-monitoring-level-to-verbose-or-minimal"></a>Jos haluat muuttaa seurantaa yksityiskohtainen tai mahdollisimman vähän

1. Avaa [Azure perinteinen portal](https://manage.windowsazure.com/)cloud palvelun käyttöönoton **määrittäminen** -sivulla.

2. Valitse **taso** **yksityiskohtainen** tai **mahdollisimman vähän**. 

3. Valitse **Tallenna**.

Kun olet ottanut käyttöön yksityiskohtainen seuranta, Aloita näe Azure perinteinen portaalissa seurantatiedot tunnin kuluessa.

Raaka suorituskykytietoja laskuri ja kootut tiedot on tallennettu tallennustilan taulukoiden qualified roolien käyttöönotto-tunnuksen mukaan tili. 

## <a name="how-to-receive-alerts-for-cloud-service-metrics"></a>Toimintaohje: saada ilmoituksia cloud palvelun arvot

Voit vastaanottaa ilmoituksia, että pilvipalvelussa seuranta arvot perusteella. Azure perinteinen portaalin **Management Services** -sivulla voit luoda säännön, joka aiheuttaa ilmoituksen, kun valitset metrijärjestelmä saavuttaa määrittämäsi arvo. Voit myös käyttää lähetetään, kun hälytyksen sähköpostiosoitetta. Lisätietoja on artikkelissa [Toimintaohje: ilmoitusten vastaanottaminen ja hallinta ilmoitusten Azure säännöt](http://go.microsoft.com/fwlink/?LinkId=309356).

## <a name="how-to-add-metrics-to-the-metrics-table"></a>Toimintaohje: arvot lisätään arvot-taulukkoon

1. Avaa [Azure perinteinen portal](http://manage.windowsazure.com/) **näytön** sivulle, jossa pilvipalvelussa.

    Arvot-taulukko näyttää oletusarvoisesti alijoukkoa käytettävissä olevat arvot. Kuvassa näkyy oletusarvoisesti yksityiskohtainen mittaukset pilvipalveluun, joka on rajoitettu Muisti\Käytettävissä olevat Mtavua suorituskyvyn laskuri-roolin tasolla koostetun tiedoilla. **Lisää arvot** avulla voit valita muita koostefunktioita ja roolin tason arvot seurannassa Azure perinteinen-portaalissa.

    ![Yksityiskohtainen näyttö](./media/cloud-services-how-to-monitor/CloudServices_DefaultVerboseDisplay.png)
 
2. Voit lisätä arvot arvot-taulukossa seuraavasti:

    1. Valitse **Lisää arvot** Avaa **Valitse arvot**, alla.

        Ensimmäinen käytettävissä arvo on laajennettu ja näyttää käytettävissä olevat vaihtoehdot. Kunkin metrijärjestelmän yläreunan asetus näyttää kaikki roolien koostetun seurantatiedot. Lisäksi voit valita yksittäisiä roolit tietojen näyttämiseen.

        ![Lisää arvot](./media/cloud-services-how-to-monitor/CloudServices_AddMetrics.png)

    2. Jos haluat valita näytettävät arvot

        - Napsauttamalla alanuolta metrijärjestelmä Laajenna valvonta-asetukset.
        - Valitse näytettävien seurantaa asetuksista-valintaruutu.

        Voit näyttää enintään 50 arvot taulukon arvot.

        > [AZURE.TIP] Yksityiskohtainen seurannassa arvot-luettelo voi sisältää kymmeniä arvot. Vierityspalkin näkyviin osoittamalla valintaikkunan oikealla puolella. Jos haluat suodattaa luettelon, haku-kuvake ja kirjoittaa hakukenttään, alla kuvatulla tavalla.
    
        ![Lisää arvot haku](./media/cloud-services-how-to-monitor/CloudServices_AddMetrics_Search.png)


3. Kun olet valinnut haluamasi arvot, valitse OK (valintamerkki).

    Valitun arvot lisätään arvot-taulukon alla kuvatulla tavalla.

    ![näytön arvot](./media/cloud-services-how-to-monitor/CloudServices_Monitor_UpdatedMetrics.png)

 
4. Poistetaan mittarin arvot-taulukosta, lisätiedot napsauttamalla sitä ja valitse sitten **Poista arvo**. (Vain näkyy **Poistaminen metrijärjestelmä** ollessasi arvo, joka on valittuna.)

### <a name="to-add-custom-metrics-to-the-metrics-table"></a>Kun haluat lisätä mukautettua arvot arvot-taulukkoon

**Yksityiskohtainen** seuranta taso sisältää oletusarvoista arvot, jotka voit valvoa luettelon portaalissa. Nämä lisäksi voit valvoa tahansa mukautettua arvot tai suorituskyvyn laskureita määrittämiä sovelluksesi portaalin.

Seuraavissa vaiheissa oletetaan, että ottanut käyttöön **yksityiskohtainen** seuranta taso ja on määrittäminen sovelluksesi voit kerätä ja siirtää mukautetun suorituskyvyn laskureita. 

Näytä mukautetun suorituskyvyn laskureita portaalissa haluat päivittää wad-ohjausobjekti-säilössä määritykset:
 
1. Avaa wad-ohjausobjekti-säilö Blob-objektien diagnostiikka-tallennustilan tilin. Voit käyttää Visual Studio tai tallennustilan-explorer toiminto.

    ![Visual Studio palvelimen Explorer](./media/cloud-services-how-to-monitor/CloudServices_Monitor_VisualStudioBlobExplorer.png)

2. Siirry blob-polku, rooli-esiintymän määrittäminen kuvion **DeploymentId/RoleName/RoleInstance** avulla. 

    ![Visual Studio tallennustilan Explorer](./media/cloud-services-how-to-monitor/CloudServices_Monitor_VisualStudioStorage.png)
3. Lataa roolin esiintymän määritystiedosto ja päivitä se, jos haluat mukautetun suorituskyvyn mitään laskureita. Esimerkiksi seurannassa *levyn tavua/sec* *C-asema* , Lisää seuraava **PerformanceCounters\Subscriptions** solmun

    ```xml
    <PerformanceCounterConfiguration>
    <CounterSpecifier>\LogicalDisk(C:)\Disk Write Bytes/sec</CounterSpecifier>
    <SampleRateInSeconds>180</SampleRateInSeconds>
    </PerformanceCounterConfiguration>
    ```
4. Tallenna muutokset ja lataa määritystiedosto korvaa aiemmin luotu tiedosto blob-samaan sijaintiin.
5. Vaihda yksityiskohtainen tilaan Azure perinteinen portaalin määrityksessä. Jos sinulla on jo yksityiskohtaisessa tilassa sinun on Vaihda mahdollisimman vähän ja takaisin yksityiskohtainen.
6. Mukautetun suorituskyvyn laskuri nyt olla käytettävissä **Lisää arvot** -valintaikkunassa. 

## <a name="how-to-customize-the-metrics-chart"></a>Toimintaohje: arvot-kaavion mukauttaminen

1. Valitse arvot-taulukosta enintään 6 arvot, jos haluat piirtää kaavion arvot. Voit valita mittarin, napsauttamalla sen vasemmalla puolella oleva valintaruutu. Jos haluat poistaa mittarin kaavion arvot, poistamalla sen valintaruudun valinnan arvot-taulukossa.

    Valitessasi arvot arvot taulukon arvot lisätään arvot-kaavio. Kapea näytössä **Lisää n** -avattavasta luettelosta sisältää metrisillä otsikot, joka ei mahdu näyttö.

 
2. Vaihtaa näyttäminen suhteellinen arvojen (viimeinen arvo vain kunkin metrijärjestelmä) ja absoluuttinen (Y-akselin näkyviin), valitse suhteellinen tai absoluuttinen kaavion yläreunassa.

    ![Suhteelliset ja suorat](./media/cloud-services-how-to-monitor/CloudServices_Monitor_RelativeAbsolute.png)

3. Voit olevan aikavälin muuttaminen arvot kaavio näyttää, valitse 1 tunti, 24 tunnin tai seitsemän päivää kaavion yläreunassa.

    ![Näytön näyttäminen piste](./media/cloud-services-how-to-monitor/CloudServices_Monitor_DisplayPeriod.png)

    Raporttinäkymät-ikkunan arvot-kaavion laskentatapaa piirtäminen arvot on erilainen. Valmiiksi määritetyn arvot on käytettävissä ja arvot lisätään tai poistetaan valitsemalla metrisillä otsikko.

### <a name="to-customize-the-metrics-chart-on-the-dashboard"></a>Voit mukauttaa koontinäytössä arvot-kaavio

1. Avaa pilvipalvelussa Raporttinäkymät-ikkunan.

2. Lisätä tai poistaa kaavion arvot:

    - Esitystapa uudet lisätiedot, valitse kaavion otsikot lisätiedot valintaruudun valinta. Kapea näytössä ** *n*??metrics** piirtää kaavion otsikko-osassa voi näyttää mittarin napsauttamalla alanuolta.

    - Poista metrijärjestelmä sille piirrettyjä tietoja kaaviossa, poista valintaruudun valinta, sen otsikon mukaan.

3. Siirtyminen **suhteellisten** ja **absoluuttisten** näyttää.

4. Valitse 1 tunti 24 tunnin tai näytettävät tiedot seitsemän päivää.

## <a name="how-to-access-verbose-monitoring-data-outside-the-azure-classic-portal"></a>Toimintaohje: Access yksityiskohtainen ulkopuolella Azure perinteinen portal-tietojen valvominen

Yksityiskohtaiset tiedot tallennetaan tallennustilan tilit, jotka voit määrittää kullekin roolille taulukot. Kuusi taulukot luodaan roolin kunkin cloud palvelun käyttöönottoa varten. Taulukot luodaan kullekin (5 minuuttia-1 tunti ja 12 tuntia). Yksi näistä taulukoista tallentaa roolin tason koosteet; toisen taulukon tallentaa roolin esiintymien koosteet. 

Taulukoiden nimet on seuraavanlainen:

```
WAD*deploymentID*PT*aggregation_interval*[R|RI]Table
```

Jos:

- *deploymentID* on määritetty pilvessä palvelun käyttöönoton GUID-tunnus

- *aggregation_interval* = 5 M, 1 H tai 12 H

- rooli tason koosteet = R

- koosteet roolin esiintymien = o

Esimerkiksi seuraavissa taulukoissa tallentaa yksityiskohtaiset tiedot yhdistetään 1 tunnin välein:

```
WAD8b7c4233802442b494d0cc9eb9d8dd9fPT1HRTable (hourly aggregations for the role)

WAD8b7c4233802442b494d0cc9eb9d8dd9fPT1HRITable (hourly aggregations for role instances)
```

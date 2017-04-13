<properties
    pageTitle="Voit valvoa tallennustilan tilin | Microsoft Azure"
    description="Lisätietoja tallennustilan-tilin Azure valvoa Azure-portaalissa."
    services="storage"
    documentationCenter=""
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="robinsh"/>

# <a name="monitor-a-storage-account-in-the-azure-portal"></a>Näytön tallennustilan-tilin Azure-portaalissa

## <a name="overview"></a>Yleiskatsaus

Voit valvoa [Azure Portal](https://portal.azure.com)-tallennustilan tilin. Kun määrität tallennustilan-tilisi kautta portaalin seurantaa varten, Azuren tallennustilaan käyttää [Tallennustilan Analytics](http://msdn.microsoft.com/library/azure/hh343270.aspx) seurata tilisi mittaukset ja kirjaudu pyynnön tiedot.

> [AZURE.NOTE]Lisäkustannuksia liittyy käsittelystä seurantatiedot [Azure-portaalissa](https://portal.azure.com). Lisätietoja <a href="http://msdn.microsoft.com/library/azure/hh360997.aspx">tallennustilan Analytics ja laskutukseen</a>. <br />

> Azure tiedostosäilön tällä hetkellä tukee tallennustilan Analytics arvot, mutta ei tue vielä kirjaaminen. Voit ottaa mittaukset Azure tiedostosäilön [Azure-portaalin](https://portal.azure.com)kautta.

> Tallennustilan tili ja replikoinnin tyyppi, vyöhykkeen tarpeettomat Storage (ZRS) ei ole arvot tai virheloki-ominaisuus on otettu käyttöön tällä hetkellä. 

> Perusteellisempaa oppaan avulla tallennustilan analyysin ja muita työkaluja tunnistaa, diagnosointi ja Azure-tallennustilan liittyvien ongelmien vianmääritys-artikkelissa [näytön vianmäärityksen, ja vianmääritys Microsoft Azuren tallennustilaan](storage-monitoring-diagnosing-troubleshooting.md).


## <a name="how-to-configure-monitoring-for-a-storage-account"></a>Toimintaohje: seuranta tallennustilan tilin määrittäminen

1. [Azure-portaalin](https://portal.azure.com) **tallennustilan**ja valitse sitten Avaa koontinäyttö tallennustilan tilin nimi.

2. Valitse **Määritä**ja vieritä blob, taulukon ja jonon services **seurannan** asetukset.

    ![MonitoringOptions](./media/storage-monitor-storage-account/Storage_MonitoringOptions.png)

3. **Valvonta**Määritä valvonnan taso ja tietojen arkistointikäytäntöjen kumpaankin palveluun on:

    -  Määritä valvonta tason valitsemalla jokin seuraavista:

      **Mahdollisimman vähän** - kerää arvot, kuten tunkeutumisen/juniin, käytettävyyttä, viiveen ja success prosenttiosuudet, jotka yhdistetään blob, taulukon ja jonon palvelut.

      **Yksityiskohtainen** - lisäksi mahdollisimman vähän arvot kerää Azure-tallennustilan Service API tallennustilan kunkin toiminnon mittaukset samat. Yksityiskohtainen arvot ottaa käyttöön sovelluksen toimintojen aikana ilmenevät ongelmat tarkempi analyysi.

      **Käytöstä** - poistaa käytöstä seuranta. Aiemmin luotu-tietojen valvominen on samanlainen säilytysaika loppuun.

- Jos haluat määrittää tietojen säilytyskäytäntö **säilytys (päivinä)**, kirjoita tietoja säilytetään 1 365 päivää päivien määrän. Jos et halua määrittää säilytyskäytännön, kirjoita nolla. Jos ei ole säilytyskäytännön, se on ylöspäin, voit poistaa tiedot. On suositeltavaa perusteella, kuinka kauan haluat säilyttää tallennustilan analytics-tiedot tilissäsi, niin, että vanha ja käyttämätöntä analytics-tiedot voi poistaa järjestelmä ilmaiseksi säilytyskäytännön määrittäminen.

4. Kun olet valmis seurantaa määritystä, valitse **Tallenna**.

Käynnistä näe seurantatiedot koontinäyttö ja noin tunnin jälkeen **valvonta** -sivulla.

Ennen kuin määrität tallennustilan tilin seuranta, ei ole seurantatiedot kerätään ja raporttinäkymät-ikkuna ja **näytön** sivulla arvot-kaaviot ovat tyhjiä.

Kun olet määrittänyt seurantaa tasot ja säilytyskäytännöt, voit valita mitkä seurannassa [Azure Portal](https://portal.azure.com)käytettävissä olevat arvot ja mitkä arvot esitystapa arvot kaavioissa. Oletusarvoiset arvot näytetään seurantaa jokaisella tasolla. Voit **Lisätä arvot** lisääminen tai poistaminen arvot arvot-luettelosta.

Arvot tallennetaan neljä taulukkoa, $MetricsTransactionsBlob, $MetricsTransactionsTable tai $MetricsTransactionsQueue $MetricsCapacityBlob tallennustilan tilin. Lisätietoja on artikkelissa [Tietoja tallennustilan Analytics arvot](http://msdn.microsoft.com/library/azure/hh343258.aspx).


## <a name="how-to-customize-the-dashboard-for-monitoring"></a>Toimintaohje: seurantaa varten Raporttinäkymät-ikkunan mukauttaminen

Raporttinäkymät-ikkunan voit määrittää enintään kuusi mittarit piirtää kaavion arvot yhdeksän käytettävissä olevat arvot. Jokaisen palvelun (blob, taulukon ja jonon) käytettävyyden, Success prosentti ja pyyntöjä yhteensä-arvot ovat käytettävissä. Käytettävissä olevat koontinäytössä arvot ovat samat mahdollisimman vähän tai yksityiskohtainen seurantaa varten.

1. [Azure-portaalin](https://portal.azure.com) **tallennustilan**ja valitse sitten Avaa koontinäyttö-tallennustilan tilin nimi.

2. Haluat muuttaa, jotka on piirretty kaavion arvot, tee jompikumpi seuraavista toimista:

    - Jos haluat lisätä uuden metrijärjestelmä kaavion, valitse värillinen kaavion alla olevassa taulukossa metrisillä otsikon vieressä oleva valintaruutu.

    - Jos haluat piilottaa metrijärjestelmä sille piirrettyjä tietoja kaaviossa, poista värillinen metrisillä otsikon vieressä oleva valintaruutu.

        ![Monitoring_nmore](./media/storage-monitor-storage-account/storage_Monitoring_nmore.png)

3. Kaavio näyttää oletusarvoisesti trendejä, näytetään vain nykyisen arvon kullekin metrijärjestelmä ( **suhteellinen** vaihtoehto kaavion yläreunassa). Näytä Y-akselin niin, että absoluuttinen arvot valitsemalla **suora**.

4. Voit muuttaa olevan aikavälin arvot kaavio näyttää, valitsemalla kaavion yläreunassa 6 tuntia, 24 tunnin tai seitsemän päivää.


## <a name="how-to-customize-the-monitor-page"></a>Toimintaohje: näyttö-sivun mukauttaminen

**Valvonta** -sivulla voit tarkastella kaikkia käytettävissä olevia arvot tallennustilan tilin.

- Jos tallennustilaa tilillä on määritetty mahdollisimman vähän seuranta-arvot, kuten tunkeutumisen/juniin, käytettävyyttä, viive ja success prosenttien koostetaan blob, taulukko tai jonon palveluista.

- Jos tallennustilaa tilillä on määritetty yksityiskohtainen seuranta-arvot ovat käytettävissä yksittäisiä tallennustilan toimintojen lisäksi palvelun tason koostefunktioista tarkempaan tarkkuudella.

Ohjeiden avulla voit valita, mitkä tallennustilan arvot voivat tarkastella arvot kaavioita ja taulukon, jossa näytetään **valvonta** -sivulla. Nämä asetukset eivät vaikuta sivustokokoelman, kooste ja tallennustilaan seuranta-tallennustilan tilin tiedot.

## <a name="how-to-add-metrics-to-the-metrics-table"></a>Toimintaohje: arvot lisätään arvot-taulukkoon


1. [Azure-portaalin](https://portal.azure.com) **tallennustilan**ja valitse sitten Avaa koontinäyttö-tallennustilan tilin nimi.

2. Valitse **valvonta**.

    **Valvonta** -sivu avautuu. Arvot-taulukko näyttää oletusarvoisesti alijoukkoa arvot, jotka ovat käytettävissä seurantaa varten. Kuvassa näkyy näytön oletusnäytön tallennustilan tilin ja kaikki kolme palvelua määritetty yksityiskohtainen valvonta. **Lisää arvot** avulla voit valita arvot, joita haluat seurata-kaikki käytettävissä olevat arvot.

    ![Monitoring_VerboseDisplay](./media/storage-monitor-storage-account/Storage_Monitoring_VerboseDisplay.png)

    > [AZURE.NOTE] Harkitse kustannukset, kun valitset arvot. On tapahtuman ja juniin päivittäminen seurantaa näyttää liittyvät kustannukset. Lisätietoja [tallennustilan Analytics ja laskutukseen](http://msdn.microsoft.com/library/azure/hh360997.aspx).

3. Valitse **Lisää arvot**.

    Kooste arvot, jotka ovat käytettävissä mahdollisimman vähän seuranta ovat luettelon yläosassa. Jos valintaruutu on valittuna, lisätiedot näkyy arvot-luettelossa.

    ![AddMetricsInitialDisplay](./media/storage-monitor-storage-account/Storage_AddMetrics_InitialDisplay.png)

4. Osoitin näyttää vierityspalkki, joita voit vetää vierittää muut arvot näkymänä valintaikkunan oikealla puolella.

    ![AddMetricsScrollbar](./media/storage-monitor-storage-account/Storage_AddMetrics_Scrollbar.png)


5. Napsauttamalla Laajenna arvo on määritetty sisältämään toimintojen luettelo mittarin alaspäin osoittavaa nuolta. Valitse kunkin toiminnon, jota haluat tarkastella [Azure Portal](https://portal.azure.com)taulukon arvot.

    Seuraavassa kuvassa on laajennettu luvan VIRHEPROSENTTI-arvo.

    ![ExpandCollapse](./media/storage-monitor-storage-account/Storage_AddMetrics_ExpandCollapse.png)


6. Kun olet valinnut kaikki palvelut mittaukset, valitsemalla OK Päivitä seurantaa konfigurointi (valintamerkki). Valitun arvot lisätään arvot-taulukossa.

7. Jos haluat poistaa mittarin taulukosta, metrijärjestelmä napsauttamalla sitä ja valitse sitten **Poista metrijärjestelmä**.

    ![DeleteMetric](./media/storage-monitor-storage-account/Storage_DeleteMetric.png)

## <a name="how-to-customize-the-metrics-chart-on-the-monitor-page"></a>Toimintaohje: valvonta-sivulla arvot-kaavion mukauttaminen

1. Valitse **näytön** sivulla tallennustilan tilin, arvot-taulukossa enintään 6 arvot, jos haluat piirtää kaavion arvot. Voit valita mittarin, napsauttamalla sen vasemmalla puolella oleva valintaruutu. Jos haluat poistaa kaavion mittarin, poista valintaruudun valinta.

2. Voit vaihtaa kaavion suhteelliset arvojen (viimeinen arvo näkyy vain) ja absoluuttinen (Y-akselin näkyviin) välillä, valitsemalla **suhteellinen** tai **suora** kaavion yläreunassa.

3.  Voit muuttaa aika alueen arvot kaavio näyttää, valitse **6 tuntia**, **24 tunnin**tai kaavion yläreunassa **7 päivää** .



## <a name="how-to-configure-logging"></a>Toimintaohje: kirjauksen määrittäminen

Kullekin tallennustilan palvelujen tallennustilan tilisi (blob, taulukon ja jonon) voivat tallentaa diagnostiikka lokit luku pyynnöt, kirjoita pyynnöt ja/tai poistaa pyynnöt ja voit määrittää tietojen arkistointikäytäntöjen palvelut.

1. [Azure-portaalin](https://portal.azure.com) **tallennustilan**ja valitse sitten Avaa koontinäyttö-tallennustilan tilin nimi.

2. Valitse **Määritä**ja vieritä kohtaan **lokiin kirjaaminen**näppäimistön alanuolipainiketta avulla.

    ![Storagelogging](./media/storage-monitor-storage-account/Storage_LoggingOptions.png)


3. Jokaisen palvelun (blob, taulukon ja jonon) Määritä seuraavat asetukset:

    - Kirjaudu pyynnön tyypit: luku pyynnöt, kirjoittaa pyynnöt ja poistaa pyynnöt.

    - Voit säilyttää kirjaustiedot päivien määrän. Kirjoita nolla on, jos et halua säilytyskäytännön määrittämiseen. Jos et määritä säilytyskäytännön, on enintään poistamisen lokit.

4. Valitse **Tallenna**.

Diagnostiikka-lokit tallennetaan $logs tallennustilan tilisi-blob-säilö. Lisätietoja $logs säilö käyttämisestä on artikkelissa [Tietoja tallennustilan Analytics lokiin kirjaaminen](http://msdn.microsoft.com/library/azure/hh343262.aspx).

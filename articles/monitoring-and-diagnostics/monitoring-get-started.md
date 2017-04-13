<properties
    pageTitle="Aloita Azure näytön | Microsoft Azure"
    description="Aloita käyttö Azure näytön Hanki tietoja resurssien toiminnan ja toimi luettelosta tietojen perusteella."
    authors="johnkemnetz"
    manager="rboucher"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="johnkem"/>

# <a name="get-started-with-azure-monitor"></a>Azure näytön käytön aloittaminen

Azure valvonta on platform-palvelun, joka tarjoaa yhden sisältölähteen Azure resurssien seurantaa varten. Azure-näyttö, jossa voit havainnollistaa, kyselyn, reitittää, arkistoida ja reagoi arvot ja Azure resurssien tulevat lokit. Voit käsitellä tiedot näytön portaalin sivu- [Näytön PowerShellin cmdlet-komennot](./insights-powershell-samples.md) [Office kaikissa ympäristöissä CLI](insights-cli-samples.md)ja [Azure näytön REST API](https://msdn.microsoft.com/library/dn931943.aspx). Tässä artikkelissa selkeät muutaman Azure näyttö-portaalin käytön esittely tärkeimmät osat.

1. Valitse-portaalissa **Lisää palveluja** ja Etsi **Näyttö** -vaihtoehto. Valitse tämä vaihtoehto lisääminen suosikkiluetteloon, niin, että se on aina helposti saatavilla, valitse vasemmanpuoleisesta siirtymispalkista tähtikuvake.

    ![Services-luettelosta valvonta](./media/monitoring-get-started/monitor-more-services.png)

2. Valitse **Näyttö** -vaihtoehto, jos haluat avata **Näyttö** -sivu. Tämä sivu yhdistää kaikki seurannan asetukset ja tiedot yhteen kootut näkymään. Se avaa ensin **tehtävä log** -osassa.

    ![Näytön sivu siirtyminen](./media/monitoring-get-started/monitor-blade-nav.png)

    > [AZURE.WARNING] Edellä näkyy **palvelun ilmoitukset** ja **ilmoituksen ryhmät** -vaihtoehdot näkyvät vain niille, jotka ovat liittyneet yksityinen esikatselu näistä toiminnoista.

    Azure näyttö on kolme basic luokkiin-tietojen valvominen: **tehtävän lokitiedoston**, **arvot**ja **vianmäärityslokit**.

3. Valitse varmistaa, että tehtävän log-osa näyttää **tehtävän Kirjaudu** .

    ![Tehtävän Log-sivu](./media/monitoring-get-started/monitor-act-log-blade.png)

    [**Tehtävän log**](./monitoring-overview-activity-logs.md) kuvataan kaikki toiminnot suoritetaan resurssit-tilaukseesi. Käytä tehtävän lokiin, voit määrittää "mitä, joka ja milloin ' luonti, päivittää tai poistaa tilauksesi resurssit-toimintoja. Esimerkiksi tehtävän lokin ilmoittaa, kun verkkosovellukseen keskeytettiin ja kuka sitä on lopetettu. Tehtävän tapahtumien poistaminen on tallennettu ympäristö ja käytettävissä kysely 90 päivää.
   
    Voit luoda ja tallentaa kyselyiden yleisimmät suodattimet ja valitse Kiinnitä portaalin Raporttinäkymät-ikkunan tärkeimmät kyselyjä, jotta tiedät aina, jos tapahtumia, jotka vastaavat on tapahtunut.

4. Suodattaminen tietyn resurssiryhmä näkymän edellisen viikon aikana ja valitse sitten **Tallenna** -painiketta.

    ![Tehtävän loki kyselyn tallentaminen](./media/monitoring-get-started/monitor-act-log-save.png)

5. Napsauta nyt **Kiinnitä** -painiketta.

    ![Valitse Kiinnitä tehtävän sisäänkirjautumista varten](./media/monitoring-get-started/monitor-act-log-pin.png)

    Useimmat tätä vaiheittaista näkymiä voi kiinnittää Raporttinäkymät-ikkunan. Näin voit luoda yksittäisen tietolähteenä toiminnallisia tietojen palvelujen. 

6. Palaa koontinäyttöön. Nyt näet, että kysely (ja tuloksia) näkyy koontinäyttöön. Tämä on kätevä, jos haluat nähdä nopeasti suuri profiilin toimintoja, jotka on tapahtunut viimeksi tilaukseesi, esimerkiksi. uusi rooli on määritetty tai AM on poistettu.

    ![Tehtävän lokin kiinnitettyinä Raporttinäkymät-ikkunan](./media/monitoring-get-started/monitor-act-log-db.png)

7. Palaa **Näyttö** -ruutu ja valitse **arvot** -osa. Sinun on ensin valitse resurssi ja suodattamalla valitseminen käyttämällä vieressä olevaa avattavan luettelon asetukset sivu yläreunassa.

    ![Suodata arvot resurssi](./media/monitoring-get-started/monitor-met-filter.png)

    Azure resursseille lähettää [**arvot**](./monitoring-overview-metrics.md). Tässä näkymässä monipuolisesti kaikki arvot yhteen ruutuun lasi niin avulla voit helposti ymmärtää, kuinka resurssien suorittamassa.

8. Kun olet valinnut resurssi, kaikki käytettävissä olevat arvot näkyvät vasemmalla puolella olevan sivu. Voit kaavion useita arvot kerralla valitsemalla arvot ja muokata kaavion tyyppi ja aluetta. Voit myös tarkastella kaikki tämän resurssin määrittäminen metrisillä ilmoitukset.

    ![Metrijärjestelmän sivu](./media/monitoring-get-started/monitor-metric-blade.png)

    > [AZURE.NOTE] Jotkin arvot ovat käytettävissä vain ottamalla yhteyttä resurssin [Sovelluksen tiedot](../application-insights/app-insights-overview.md) -ja/tai Windows- tai Linux Azure Diagnostiikka.

9. Kun olet tyytyväinen kaavion, voit kiinnittää koontinäyttöön **Kiinnitä** -painiketta.

10. Palaa **Näyttö** -sivu ja valitse **vianmäärityslokit**.

    ![Vianmäärityslokit sivu](./media/monitoring-get-started/monitor-diaglogs-blade.png)

    [**Vianmäärityslokit**](monitoring-overview-of-diagnostic-logs.md) ovat lokit lähettämän *mukaan* resurssi, jotka antavat tietoja kyseiseen resurssiin toiminnan. Verkon Security ryhmän säännön laskureita ja logiikka App työnkulun lokit ovat molemmat vianmäärityslokit. Lokitiedostot on tallennustilan tilin tallennetuista, virtauttaa tapahtumaa-toiminnossa tai [Lokin Analytics](../log-analytics/log-analytics-overview.md)lähetetään. Lokitiedoston Analytics on Microsoftin toiminnallisia liiketoimintatietojen tuote Tarkennettu haku- ja ilmoitat varten.
   
    Portaalissa voit tarkastella ja suodattaa tilauksen tunnistaminen, jos ne on otettu käyttöön vianmäärityslokit kaikki resurssit.

11. Valitse resurssi-vianmäärityslokit-sivu. Jos vianmäärityslokit tallennetaan tallennustilan-tilille, näet tunnin välein lokit, jonka voit ladata suoraan luettelo.

    ![Yksi resurssi vianmäärityslokit](./media/monitoring-get-started/monitor-diaglogs-detail.png)

    Voit myös napsauttaa **Diagnostiikan asetuksia**, jotka avulla voit määrittää tai muuttaa arkistointia tallennustilan tilin streaming tapahtuman porttiin tai lokin Analytics työtilan lähettämistä.

    ![Vianmäärityslokit ottaminen käyttöön](./media/monitoring-get-started/monitor-diaglogs-enable.png)

    Jos olet määrittänyt vianmäärityslokit Log-analyysin avulla voit hakea ne sitten portaalin **Log haku** -osassa.

12. Siirry näyttö-sivu **ilmoitukset** -osa.

    ![ilmoitusten julkisen sivu](./media/monitoring-get-started/monitor-alerts-nopp.png)

    Tässä voit hallita kaikki [**ilmoitukset**](./monitoring-overview-alerts.md) Azure resursseja. Tämä sisältää arvot, tehtävän tapahtumien (yksityinen esikatselunäkymässä), sovelluksen havainnollistamisen web testit (sijainti) ja sovelluksen havainnollistamisen ennakoiva diagnostiikan ilmoitukset. Ilmoitusten voit käynnistää voidaan lähettää sähköpostia tai HTTP-POST webhook URL-osoitteeseen.
   
13. Valitse **Lisää metrisillä ilmoitus** luoda ilmoituksen.

    ![Lisää metrisillä ilmoitus](./media/monitoring-get-started/monitor-alerts-add.png)

    Voit kiinnittää ilmoituksen sitten koontinäyttöön voit helposti tarkistaa sen, milloin tahansa.

14. Näyttö-kohta on myös linkit [Sovelluksen tiedot](../application-insights/app-insights-overview.md) -sovellukset ja [Kirjaudu Analytics](../log-analytics/log-analytics-overview.md) ratkaisujen. Näiden Microsoft-tuotteiden on laaja integrointi Azure näyttö.

15. Jos et käytä hakemuksen tiedot tai lokin Analytics-hyvinkin Azure näyttö on partnership nykyisen valvonta, kirjaaminen ja suunnattujen varoitusmenetelmien tuotteiden kanssa. Katso Microsoftin [kumppanisivustossa](./monitoring-partners.md) täydellisen luettelon sekä ohjeet, miten voit integroida.

Seuraavat vaiheet ja kaikki haluamasi ruudut Raporttinäkymät-ikkunan kiinnittäminen, voit luoda sovelluksen ja katsomalla infrastruktuurin täydellinen näkymiä:

![Azure näytön Raporttinäkymät-ikkunan](./media/monitoring-get-started/monitor-final-dash.png)

## <a name="next-steps"></a>Seuraavat vaiheet
- Lue [Azure näytön yleiskatsaus](./monitoring-overview.md)

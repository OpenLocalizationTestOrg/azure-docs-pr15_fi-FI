<properties
    pageTitle="Valvoa Log Analytics pinta lihavoituna | Microsoft Azure"
    description="Surface-toiminnossa-ratkaisun avulla voit seurata pinta keskittimet kunto ja ymmärtää, kuinka niitä käytetään."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="banders"/>

# <a name="monitor-surface-hubs-with-log-analytics"></a>Näytön Log Analytics pinta lihavoituna

Tässä artikkelissa kuvataan, miten voit käyttää toiminnossa pinta-ratkaisun Log Analytics seurannassa Microsoft Surface keskittimeen laitteiden kanssa Microsoft toimintojen hallinta Suite (OMS). Lokitiedoston Analytics auttaa seuraamaan pinta keskittimet kunto sekä ymmärtää, kuinka niitä käytetään.

Kunkin pinta-toiminnossa on asennettu Microsoft seuranta Agent. Sen avulla, että voit lähettää tiedot pinta-toiminnosta OMS agentti. Lokitiedostojen luetaan pinta keskittimet ja ne sitten lähetetään OMS-palvelun. Ongelmat, kuten palvelinten on offline-tilassa, kalenteri ei synkronoidu, tai jos laitteen tiliä ei voi kirjautua Skype näkyvät OMS pinta-toiminnossa raporttinäkymät-ikkunassa. Koontinäyttö-tietojen avulla voit tunnistaa laitteet, joka ei ole käynnissä tai, jotka ovat muita ongelmia, ja käyttää mahdollisesti havaitut ongelmat korjaukset.


## <a name="installing-and-configuring-the-solution"></a>Asentaminen ja määrittäminen ratkaisu

Seuraavat tiedot avulla voit asentaa ja määrittää ratkaisu. Jotta voit hallita pinta keskittimet-Microsoft toimintojen hallinta Suite (OMS), sinun on seuraavasti:

- [OMS](http://www.microsoft.com/oms)kelvollinen tilausta.
- [OMS tilaus](https://azure.microsoft.com/pricing/details/log-analytics/) -tason, joka tukee useita laitteita, joita haluat seurata. OMS hinnat vaihtelee sen mukaan, kuinka monta laitteet on rekisteröity ja kuinka paljon tietoja sen prosessit. Haluat Ota tämä huomioon, kun suunnittelet pinta-toiminnossa rahoittaja.

Seuraavaksi voit joko OMS tilauksen lisääminen olemassa olevan Microsoft Azure-tilauksen tai Luo uusi työtila suoraan OMS-portaalissa. Yksityiskohtaiset ohjeet joko menetelmällä on [lokin Analytics käytön aloittaminen](log-analytics-get-started.md). Kun OMS-tilaus on määritetty, liittyy voi rekisteröidä pinta-toiminnossa laitteet kahdella tavalla:

- Automaattisesti InTune kautta
- Manuaalisesti – pinta-toiminnossa laitteen **asetukset** .

## <a name="set-up-monitoring"></a>Määritä valvonta

Voit valvoa terveys- ja että pinta-toiminnossa Log Analytics käyttäminen OMS toiminnan. Voit rekisteröidä OMS pinta-toiminnosta käyttämällä InTune- tai paikallisesti käyttäminen pinta-toiminnossa **asetukset** .

## <a name="connect-surface-hubs-to-oms-through-intune"></a>Surface keskittimet yhdistäminen OMS InTune kautta

Sinun on työtilan tunnus ja työtilan avain OMS työtilan, joka hallitsee pinta keskittimet. Voit avata ne OMS-portaalista.

InTune on Microsoftin tuote, jonka avulla voit hallita keskitetysti OMS asetukset, joita käytetään yhden tai useamman laitteilla. Määrittää laitteet InTune kautta seuraavasti:

1. InTune kirjautuminen.
2. Siirry **asetuksiin** > **yhdistetty lähteistä**.
3. Luo tai Muokkaa käytäntöä pinta-toiminnossa mallin pohjalta.
4. Käytännön OMS (Azure toiminnallisia tiedot)-osassa Siirry ja lisää käytännön *Työtila-tunnus* ja *Työtila-näppäintä* .
5. Tallenna käytännön.
6. Liitä käytännön laitteiden haluamasi ryhmä.

  ![InTune-käytäntö](./media/log-analytics-surface-hubs/intune.png)

InTune Synkronoi OMS asetukset valitse rekisteröiminen ne OMS työtilassa kohde-ryhmässä laitteiden kanssa.

## <a name="connect-surface-hubs-to-oms-using-the-settings-app"></a>Surface keskittimet yhdistäminen OMS käyttämällä Settings-sovellukseen

Sinun on työtilan tunnus ja työtilan avain OMS työtilan, joka hallinnoi pinta keskittimet. Voit avata ne OMS-portaalista.

Jos et käytä InTune ympäristön hallintaan, voit rekisteröidä laitteita manuaalisesti jokaisen pinta-toiminnossa **asetukset** :

1. Avaa pinta-toiminnossa **asetukset**.
2. Kirjoita pyydettäessä laitteen järjestelmänvalvojan tunnistetietoja.
3. Valitse **laite**, ja valitse **Seuranta**-kohdassa **OMS asetusten määrittäminen**.
4. Valitse **Ota käyttöön seuranta**.
6. OMS asetukset-valintaikkunassa **Työtila-tunnus** ja kirjoita **Työtila-näppäintä**.  
  ![asetukset](./media/log-analytics-surface-hubs/settings.png)
7. Valitse **OK** , Viimeistele määritys.

Vahvistuksen tulee näkyviin, joka ilmoittaa, onko OMS määritys onnistui laitteeseen. Jos näin on, näyttöön tulee sanoma siitä, että agentti yhteydessä onnistuneesti OMS-palvelun. Laitteen aloittaa OMS, jossa voit tarkastella ja tehdä tietojen lähettämistä.

## <a name="monitor-surface-hubs"></a>Näytön pinta keskittimet

Seurannan pinta keskittimet käyttämällä OMS muistuttaa seuranta rekisteröityneet laitteet.

1. Kirjautuminen OMS-portaaliin.
2. Siirry pinta-toiminnossa ratkaisu pack Raporttinäkymät-ikkunan.
3. Laitteesi kunto näkyy.

  ![Surface keskittimeen Raporttinäkymät-ikkunan](./media/log-analytics-surface-hubs/surface-hub-dashboard.png)

Voit luoda olemassa oleva vai mukautettu log haut perustuvat [ilmoitukset](log-analytics-alerts.md) . OMS kerää pinta keskittimet tietojen avulla voit etsiä ongelmia ja ehdot, jotka voit määrittää laitteet koskeva ilmoitus.


## <a name="next-steps"></a>Seuraavat vaiheet

- [Log rajaamalla Log Analytics](log-analytics-log-searches.md) avulla voit tarkastella yksityiskohtaista tietoa sisältävä pinta-toiminnossa.
- Luo [ilmoituksia](log-analytics-alerts.md) ilmoittamaan, kun pinta keskittimet ilmetä ongelmia.

<properties 
    pageTitle="Tietoja Stream Analytics Työn seuranta | Microsoft Azure" 
    description="Tietoja Stream Analytics Työn seuranta" 
    keywords="kyselyn näyttö"
    services="stream-analytics" 
    documentationCenter="" 
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>

# <a name="understand-stream-analytics-job-monitoring-and-how-to-monitor-queries"></a>Lisätietoja Stream Analytics Työn seuranta sekä seurata kyselyt

## <a name="introduction-the-monitor-page"></a>Johdanto: Näyttö-sivu

Azure hallinta-portaalin ja Azure-portaalin tuoda suorituskyvyn mittarit, jonka avulla voidaan valvoa ja kysely- ja suorituskyvyn vianmääritysvaiheita. 

Valitse Azure hallinta-portaalin käynnissä Stream Analytics-työ, näet nämä arvot **Näyttö** -välilehdessä. Ei viiveen enintään 1 minuutti valvonta-sivulla näkyvät suorituskyvyn mittarit.  

  ![Raporttinäkymät-ikkunan Työn seuranta](./media/stream-analytics-monitoring/01-stream-analytics-monitoring.png)  

Siirry Azure-portaalissa Stream Analytics resurssit ovat kiinnostuneita pelkkiä mittaukset ja **Seuranta** -osassa.  

  ![Azure Portal seuranta työn Raporttinäkymät-ikkunan](./media/stream-analytics-monitoring/06-stream-analytics-monitoring.png)  

Ensimmäisen kerran Stream Analytics-työ luodaan alueen, sinun on diagnostiikka määrittämiseksi alueen. Voit tehdä tämän napsauttamalla mitä tahansa **Seuranta** -kohdassa ja **Diagnostiikka** -sivu tulee näkyviin. Tässä voit ottaa diagnostiikka- ja määritä tallennustilan-tili, tietoja seurantaa varten.  

  ![Azure portaalin määrittäminen kyselyn diagnostiikka](./media/stream-analytics-monitoring/07-stream-analytics-monitoring.png)  

## <a name="metrics-available-for-stream-analytics"></a>Arvot muodossa Analytics käytettävissä


| Arvo | Määritys |
|--------|-------------|
| SU % käyttö | Streaming-summat käyttö määritti työn työn mittakaava-välilehti. Tämä ilmaisin saavuttaa 80 %, tai edellä on suuri todennäköisyys, että käsittely saattaa viivästyä tai pysäyttää käynnissä tekeminen. |
| Syötteen tapahtumat | Tietomäärän vastaanottanut Stream Analytics-työ tapahtumien määrä. Tämä voidaan tarkistaa, että tapahtumat lähetetään lähde. |
| Tulosteen tapahtumat | Tietomäärän lähettämä Stream Analytics-työ tulostuksen, kohteen tapahtumien määrä. |
| Tilauksen tapahtumat | Vastaanotettujen ulos järjestyksessä, jossa on pudotettu tai annettu muutetut aikaleiman, tapahtumien järjestys käytännön perusteella tapahtumien määrä. Tämä voi heiketä Out of tilauksen poikkeama ikkunan-asetusta määrityksestä. |
| Muuntovirheitä | Muuntovirheitä aiheutuvat Stream Analytics työn määrä. |
| Runtimen virheet | Virheiden, jotka käynnistyvät Stream Analytics työn suorituksen aikana määrä. |
| Myöhässä olevat syötteen tapahtumat | Saapuvat myöhässä lähde, joka on joko poistettu tai niiden aikaleima tapahtumien määrä on muutettu tapahtumien järjestys käytännön määritys myöhässä saapumisen poikkeama ikkuna-asetuksen perusteella. |

## <a name="customizing-monitoring-in-the-azure-management-portal"></a>Mukauttaminen seuranta Azure hallinta-portaalissa ##

Enintään 6 arvot voidaan näyttää kaaviossa.

Vaihtaa näyttäminen suhteellinen arvojen (viimeinen arvo vain kunkin metrijärjestelmä) ja absoluuttinen (Y-akselin näkyviin), valitse suhteellinen tai absoluuttinen kaavion yläreunassa.

  ![Suhteellinen suora kyselyn näyttö](./media/stream-analytics-monitoring/02-stream-analytics-monitoring.png)  

Arvot, voit tarkastella näytön kaavion 1 tunti 12 tunnin tai 24 tunnin seitsemän päivää koosteet.

Voit olevan aikavälin muuttaminen arvot kaavio näyttää, valitse 1 tunti, 24 tunnin tai seitsemän päivää kaavion yläreunassa.

  ![Kyselyn näytön aika-asteikko](./media/stream-analytics-monitoring/03-stream-analytics-monitoring.png)  

Voit määrittää sääntöjä, jotka voit ilmoittaa sähköpostitse siltä varalta, että projektin ylittää määritetyn raja-arvon. 

## <a name="customizing-monitoring-in-the-azure-portal"></a>Mukauttaminen seuranta Azure-portaalissa ##

Voit muuttaa kaavion arvot näkyvät-tyypin ja kellonajan Muokkaa kaavion asetukset-alue. Lisätietoja on artikkelissa [miten voit mukauttaa seuranta](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

  ![Azure portaalin kyselyn näytön aika-asteikko](./media/stream-analytics-monitoring/08-stream-analytics-monitoring.png)  

## <a name="job-status"></a>Työn tila

Virta Analytics töiden tilan voi tarkastella Azure perinteinen-portaalissa on näkyvissä töiden luettelo. Näet töiden luettelo napsauttamalla Azure perinteinen portaalissa Stream Analytics-kuvaketta.

| Tila | Määritys |
|--------|------------|
| Luotu | Työn on luotu, mutta ei ole käynnistetty. |
| Aloittaminen | Käyttäjä napsauttaa käynnistettäessä työn ja työn käynnistäminen |
| Käynnissä | Työ jaetaan, käsittelyn syötteen tai käsitellä syötteen odottaa. Jos projektin näyttää käytössä-tilan ilman Tulosta, on todennäköistä, että tietojen käsittely aika-ikkuna on suuri tai kyselyn logiikan monimutkaisia. Voivat johtua voi olla, että tällä hetkellä ei ole lähetetty projektin tiedot. |
| Pysäyttäminen | Käyttäjä napsauttaa valitse Pysäytä työn ja työ pysäytetään. |
| Pysäytetty | Työn on lopetettu. |
| Heikentynyt | Tässä tilassa ilmaisee, että Stream Analytics työn olettaa lyhytkestoisia virheitä (for ex. Syöte/tulos virheitä, käsittelemisen virheitä, muuntovirheitä jne.). Työn on yhä käynnissä, mutta liittämisasetuksia on paljon luodaan virheitä. Työn tarvitsee asiakkaan huomiota ja asiakkaan näkee toimintojen lokit virheet. |
| Epäonnistui | Tämä ilmaisee, että työ on epäonnistunut virheiden vuoksi ja käsittely on pysähtynyt. Asiakas on Tarkista toimintojen lokitiedot kyselyjä, jotta voit korjata virheet. |
| Poistaminen | Tämä ilmaisee, että projektin poistetaan. |

## <a name="diagnosis"></a>Vianmääritys

Työn Raporttinäkymät-ikkunan on Azure hallinta-portaalissa tietoja tarvittaessa Etsi vianmäärityksen eli syötteiden, tulostaa ja/tai toimintojen Kirjaudu. Voit napsauttaa linkkiä, siirry oikeaan paikkaan vianmäärityksen tarkasteltavan.

  ![Kyselyn näytön virhe](./media/stream-analytics-monitoring/04-stream-analytics-monitoring.png)  

Syöttö- tai resurssien valitsemalla sisältää yksityiskohtaiset vianmääritystiedot. Tämä on päivitetty uusimpaan diagnostiikkatietojen työn ollessa käynnissä.

  ![Kyselyn diagnostiikka](./media/stream-analytics-monitoring/05-stream-analytics-monitoring.png)  

## <a name="get-help"></a>Ohjeiden saaminen
Saat lisäohjeita yritä Microsoftin [Azure Stream Analytics-keskustelupalsta](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Seuraavat vaiheet

- [Azure Stream Analytics esittely](stream-analytics-introduction.md)
- [Azure Stream Analytics käytön aloittaminen](stream-analytics-get-started.md)
- [Skaalaa Azure Stream Analytics töitä](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics kyselyn kieliohje](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics hallinta REST API-viittaus](https://msdn.microsoft.com/library/azure/dn835031.aspx)

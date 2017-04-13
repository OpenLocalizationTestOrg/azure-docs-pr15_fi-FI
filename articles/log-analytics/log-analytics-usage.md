<properties
    pageTitle="Analysoi tietoliikenteestä Log Analytics | Microsoft Azure"
    description="Voit Log Analytics käyttö-sivulla voit tarkastella, kuinka paljon tietoja lähetetään OMS-palvelun."
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
    ms.topic="get-started-article"
    ms.date="08/11/2016"
    ms.author="banders"/>

# <a name="analyze-data-usage-in-log-analytics"></a>Analysoi tietoliikenteestä Log Analytics

Log-analyysin toimintojen hallinta Suite (OMS) kerää tietoja ja lähettää sen OMS-palvelun säännöllisesti.  Voit tarkastella, kuinka paljon tietoja lähetetään OMS-palvelun **käytön** -sivulla. **Käyttö** -sivulla myös kerrotaan, kuinka paljon tietoja on lähetetty päivittäin ratkaisuja ja kuinka usein palvelinten lähettää tietoja.

>[AZURE.NOTE] Jos sinulla on luotu käyttämällä [OMS sivuston](http://www.microsoft.com/oms)vapaa-tili, on enintään 500 Megatavua tietojen lähettäminen OMS-palvelun päivittäin. Jos pääset päivittäisen, tietojen analysointi lopettaminen ja jatka seuraavaan päivään alkuun. Sinun on myös lähettää tiedot, joita ei ole hyväksyneet vai OMS käsittelemien uudelleen.

Voit tarkastella käyttö **käyttö** -ruudun avulla OMS **Yleiskatsaus** -koontinäytössä.

![käyttö-ruutu](./media/log-analytics-usage/usage-tile.png)

Jos olet ylittänyt päivittäinen käyttö-enimmäismäärän tai jos ole lähellä rajan, voit halutessasi poistaa ratkaisun vähentää tiedot, joita voi lähettää OMS-palvelun. Saat lisätietoja poistaminen ratkaisuja, [Lisää Log Analytics-ratkaisuja ratkaisuvalikoimasta](log-analytics-add-solutions.md).

![Raporttinäkymät-ikkunan käyttö](./media/log-analytics-usage/usage-dashboard.png)

**Käyttö** -sivulla näkyvät seuraavat tiedot:

- Keskimääräinen päivittäinen käyttö
- Tietojen käyttö kunkin ratkaisun viimeisen 30 päivän aikana
- Kuinka paljon tietoja ympäristössäsi palvelimet lähettämässä OMS-palvelun kautta viimeisten 30 päivän aikana
- Hinnat tason ja arvioidut tietojen suunnitelma
- Tietoja tason palvelusopimus (SLA), mukaan lukien, kuinka kauan rekisteröijän OMS tietojen käsittelemiseen

## <a name="to-work-with-usage-data"></a>Käyttötietojen käsittelemiseen

1. Valitse **sivulla** Napsauta **käyttö** -ruutua.
2. **Käyttö** -sivulla tarkastella käyttö luokat, jotka näyttävät alueet olet huolissasi siitä.
3. Jos sinulla on ratkaisun, joka on muissa liikaa päivittäinen Lataa-kiintiön, harkitse kyseisen ratkaisun poistaminen.

## <a name="to-view-your-estimated-cost-and-billing-information"></a>Jos haluat tarkastella arvioidut ja laskutustietoja
1. Valitse **sivulla** Napsauta **käyttö** -ruutua.
2. Valitse **käyttö**-kohdassa **käyttö** -sivulla nuolenkärki (**>**) vieressä **Arvioitu kustannukset**.
3. Laajennettu Valitse **tiedot-suunnitelman** tiedot, näet, että arvioitu maksaa kuukausittain.  
    ![Tiedot-suunnitelma](./media/log-analytics-usage/usage-data-plan.png)
4. Jos haluat tarkastella laskutustiedot, valitsemalla Näytä tilauksen tiedot **laskun tarkasteleminen** .
    - Valitse tilaukset-sivulla tilauksen tiedot ja rivi-kohdeluettelon käyttö.  
        ![tilauksen](./media/log-analytics-usage/usage-sub01.png)
    - Tilauksen yhteenvetosivulla voit suorittaa erilaisia tehtäviä voit hallita ja tarkastella tarkempia tietoja tilauksen.  
        ![Tilauksen tiedot](./media/log-analytics-usage/usage-sub02.png)

## <a name="to-view-data-batches-for-your-sla"></a>Voit tarkastella tietoja erissä, että SLA
1. Valitse **sivulla** Napsauta **käyttö** -ruutua.
2. **Taso-palvelusopimus**Valitse **Lataa SLA tiedot**.
3. Excelin XLSX-tiedosto on ladattu tarkistettavaksi.  
    ![SLA tiedot](./media/log-analytics-usage/usage-sla-details.png)

## <a name="next-steps"></a>Seuraavat vaiheet

- Katso [Log rajaamalla Log Analytics](log-analytics-log-searches.md) ratkaisuja keräämien yksityiskohtaiset tiedot.

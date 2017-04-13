<properties
    pageTitle="Tapahtumien tarkasteleminen ja valvontalokit"
    description="Lue, miten saat kaikki tapahtumat, jotka tapahtuvat Azure-tilaukseesi."
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
    ms.date="04/28/2015"
    ms.author="robb"/>

# <a name="view-events-and-audit-logs"></a>Tapahtumien tarkasteleminen ja valvontalokit

Kaikki toiminnot suoritetaan Azure resurssit ovat täysin tapahtumista mukaan Azure Resurssienhallinta, luominen ja poistaminen myöntämistä tai peruutetaan access. Voit selata lokitiedostot Azure-portaalissa, ja voit käyttää [REST API](https://msdn.microsoft.com/library/azure/dn931927.aspx) tai [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) myös käyttää kaikkia käytettävissä olevia tapahtumien ohjelmallisesti.

## <a name="browse-the-events-impacting-your-azure-subscription"></a>Siirry Azure tilauksen vaikuttavat tapahtumat

1. Kirjautuminen [Azure Portal](https://portal.azure.com/).
2. Valitse **Selaa** ja valitse **valvontalokien**.  
    ![Selaa toiminnossa](./media/insights-debugging-with-events/Insights_Browse.png)
3. Tämä avaa sivu, jossa on kaikki tapahtumat, jotka ovat vaikuttaneet kaikista tilauksistasi viimeisten seitsemän päivän, ylöspäin. Ylimpänä on kaaviosta, jossa tason mukaan ja alta, joihin tiedot on täysi luettelo lokit:  ![kaikki tapahtumat](./media/insights-debugging-with-events/Insights_AllEvents.png)

>[AZURE.NOTE] Voit tarkastella tietyn tilauksen 500 uusimmat tapahtumat vain Azure-portaalissa.

4. Voit valita minkä tahansa lokitapahtuman, jos haluat tarkastella tapahtumia, jotka koostuvat. Esimerkiksi kun otat käyttöön jotain resurssiryhmä, useista eri resursseista saattaa olla luodun tai muokatun. Jokaiselle merkinnälle näet:
    * Tapahtuma - **tason** esimerkiksi se voi olla vain jotain jäljittämiseksi (**tiedottavat**) tai jos järjestelmässä on suoriutunut väärä, että sinun tarvitsee tietää (**Virhe**).
    * **Tila** - lopullinen tila yleensä ovat **onnistui** tai **epäonnistui**, mutta se voi olla myös **hyväksytyt** pitkään käynnissä olevia toimintoja varten.
    * *Kun* tapahtuma on tapahtunut.
    * *Kuka* suorittaa toiminnon, jos kuka tahansa. Kaikki toiminnot suoritetaan käyttäjät, jotkin tehdään Taustajärjestelmä services, joten niitä ei ole **soittajan**.
    * **Korrelaatiotunnuksen** tapahtuman – tämä on yksilöivä tunnus tämän kohdan arvoksi-toimintoa.

5. Sieltä voit siirtyä näkevän tapahtuman tarkat tiedot-sivu.

    ![Resurssiryhmät](./media/insights-debugging-with-events/Insights_EventDetails.png)

    Tämä sivu sisältää yleensä **alitila** ja **Ominaisuudet** -osan, jotka sisältävät tärkeitä tietoja vianmääritystä varten tapahtumien **epäonnistui** .

## <a name="filter-to-specific-logs"></a>Suodattaminen tietyn lokit

Jotta voit nähdä tapahtumat, jotka koskevat tietyn kohteen tai tietyn tyyppisiä, voit suodattaa valvonta lokit-sivu napsauttamalla **Suodata** -komento. Suodata sivu käyttää myös muuttamiseen **aikaa välisenä ajankohtana** , Valvo lokit-sivu.

Kun napsautat tätä komentoa, uusi sivu avautuu:

![Suodatin](./media/insights-debugging-with-events/Insights_EventFilter.png)

Suodattimien neljä perustyyppiä:

1. Tilauksen
2. **Resurssiryhmä**
3. **Resurssilaji**
4. Tietyn **resurssi** - tämä on aiempia kiinnostavan resurssin koko *Resurssitunnus*

Lisäksi voit myös suodattaa tapahtumat mukaan suorittanut tapahtuma tai tapahtuman tason mukaan.

Kun olet valitseminen, mitä haluat nähdä, valitse sivu alareunassa **Päivitä** -painiketta.

## <a name="monitor-events-impacting-specific-resources"></a>Näytön tapahtumat vaikuttavat tiettyjä resursseja

1. Valitse **Selaa** ja Etsi kiinnostavan resurssi. Näet myös kaikki lokit kokonaisen **resurssiryhmä**.
2. Valitse resurssin sivu alaspäin, kunnes löydät **tapahtumat** -ruutu.  
    ![Tapahtumat-ruutu](./media/insights-debugging-with-events/Insights_EventsTile.png)
3. Napsauta ruudun haluat tarkastella vain käyttämäsi resurssi suodatettuna tapahtumia. **Suodatin** -komennolla olevan aikavälin muuttaminen tai tarkentaminen suodattimia.

## <a name="next-steps"></a>Seuraavat vaiheet

* [Ilmoitusten vastaanottaminen](insights-receive-alert-notifications.md) , kun tapahtuman tapahtuu.
* [Näytön palvelun mittarit](insights-how-to-customize-monitoring.md) , varmista, että palvelu on käytettävissä ja vastaa.
* Voit selvittää, milloin Azure on ilmennyt suorituskyvyn heikkeneminen tai palvelun keskeytyksiä [seuraa palvelun kuntoa](insights-service-health.md) .  

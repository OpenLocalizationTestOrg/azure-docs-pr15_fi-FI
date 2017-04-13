<properties
    pageTitle="Ota käyttöön seuranta- ja Microsoft Azure diagnostiikka | Microsoft Azure "
    description="Opi määrittämään diagnostiikka resurssien Azure-tietokannassa."
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
    ms.date="09/08/2015"
    ms.author="robb"/>

# <a name="enable-monitoring-and-diagnostics"></a>Ota käyttöön seuranta ja vianmääritys

Voit määrittää Monimuotoisen ja usein, seuranta- ja diagnostiikka tietojen [Azure Portal](https://portal.azure.com)resurssien tietoja. Voit käyttää myös [REST API](https://msdn.microsoft.com/library/azure/dn931932.aspx) tai [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) määrittäminen diagnostiikka ohjelmallisesti.

Azure-tietokannassa Diagnostiikka, seuranta ja metrijärjestelmän tiedot tallentuvat valittua tallennustilan-tilille. Voit käyttää jostakin sillä haluat lukea tietoja, tallennustilan Explorerista kolmannen osapuolen sillä Power BI.

## <a name="when-you-create-a-resource"></a>Kun luot resurssi

Useimmat palvelut mahdollistavat diagnostiikka käyttöön, kun luot ne [Azure-portaalissa](https://portal.azure.com).

1. **Uusi** ja valitse kiinnostavan resurssi.

2. Valitse **Vaihtoehtoinen määritys**.
    ![Diagnostiikka-sivu](./media/insights-how-to-use-diagnostics/Insights_CreateTime.png)

3. Valitse **kansio**ja **Valitse sitten**. Sinun on Valitse tallennustilan tili, jonka haluat diagnostiikka tallennetaan. Sinun veloitetaan normaali tietojen korvaukset ja tallennustilaa diagnostiikka lähettämistä tallennustilan tilin.

4. Valitse **OK** ja luo resurssi.

## <a name="change-settings-for-an-existing-resource"></a>Aiemman resurssin asetusten muuttaminen

Jos olet jo luonut resurssi ja haluat muuttaa diagnostiikka-asetukset (esimerkiksi muuttamaan tietojen kerääminen taso), voit tehdä Azure-portaalissa, oikealle.

1. Siirry resurssi ja valitse **asetukset** -komento.

2. Valitse **Diagnostiikka**.

3. **Diagnostiikka** -sivu on mahdollista diagnostiikka- ja resurssin sivustokokoelman seurantatiedot. Seuraavia resursseja, voit valita myös **säilytyskäytännön tietojen puhdistettava tiedosto tallennustilan-tililtä** .
    ![Tallennustilan diagnostiikka](./media/insights-how-to-use-diagnostics/Insights_StorageDiagnostics.png)

4. Kun olet valinnut asetukset, valitse **Tallenna** -komentoa. Voi kestää hieman samalla, kun seurantaa varten tiedot näkyvät, jos haluat ottaa käyttöön ensimmäisen kerran.

### <a name="categories-of-data-collection-for-virtual-machines"></a>Tietojen kerääminen näennäiskoneiden luokkiin
Näennäiskoneiden kaikki arvot ja lokit tallennetaan minuutin välein, joten voit aina uusimmat tiedot tietokoneen tietoja.

- **Tavallinen mittarit** : kunto arvot virtuaalikoneen, kuten suorittimen ja muistin tietoja
- **Verkko- ja web mittarit** : arvot verkkoyhteyksien ja WWW-palveluista
- **.NET-arvot** : tietoja virtuaalikoneen .NET ja ASP.NET sovellusten arvot
- **SQL-arvot** : Jos käytössäsi on Microsoft SQL-palvelu, sen suorituskyvyn mittarit
- **Windowsin tapahtumalokien sovelluksen** : Windows-tapahtumat, jotka lähetetään sovelluksen-kanava
- **Windowsin tapahtumalokien järjestelmän** : Windows-tapahtumat, jotka lähetetään järjestelmä. Tämä on myös [Microsoft Antimalware](http://go.microsoft.com/fwlink/?LinkID=404171&clcid=0x409)kaikki tapahtumat.
- **Windowsin tapahtumalokien suojaus** : Windows-tapahtumat, jotka lähetetään suojaus-kanava
- **Diagnostiikan infrastruktuurin lokit** : tietoja diagnostiikka sivustokokoelman infrastruktuuri kirjaaminen
- **IIS-lokeja** : lokeja IIS-palvelin

Huomaa, että tällä hetkellä tiettyjä Linux jaot ei tue ja Vieras-agentti on oltava asennettuna virtuaalikoneen.

## <a name="next-steps"></a>Seuraavat vaiheet

* [Vastaanota ilmoitukset](insights-receive-alert-notifications.md) aina, kun toiminnallisia tapahtuma tapahtuu tai arvot toimintojen raja-arvon.
* [Näytön palvelun mittarit](insights-how-to-customize-monitoring.md) , varmista, että palvelu on käytettävissä ja vastaa.
* Varmista, että palvelun asteikko demand perusteella [skaalata esiintymän Laske automaattisesti](insights-how-to-scale.md) .
* Voit halutessasi selvittääksesi, miten pilveen osassa koodisi toimii [näytön sovelluksen suorituskykyä](../application-insights/app-insights-azure-web-apps.md) .
* [Tapahtumien tarkasteleminen ja valvontalokit](insights-debugging-with-events.md) saat kaikki, joka on tapahtunut palvelussa.
* Voit selvittää, milloin Azure on ilmennyt suorituskyvyn heikkeneminen tai palvelun keskeytyksiä [seuraa palvelun kuntoa](insights-service-health.md) .

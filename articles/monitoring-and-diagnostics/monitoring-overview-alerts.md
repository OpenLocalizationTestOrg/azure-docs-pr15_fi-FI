<properties
    pageTitle="Yleistä Microsoft Azure ilmoitukset | Microsoft Azure"
    description="Ilmoitusten avulla voit seurata Azure resurssin mittarit, tapahtumia tai lokit ja saada ilmoituksen, kun määrittämäsi ehto täyttyy."
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
    ms.date="09/24/2016"
    ms.author="robb"/>

# <a name="overview-of-alerts-in-microsoft-azure"></a>Yleistä Microsoft Azure ilmoitukset


Tässä artikkelissa kuvataan, mitä ilmoitukset ovat niiden edut ja miten niiden avulla pääset alkuun.  

## <a name="what-are-alerts"></a>Mitkä ovat ilmoitukset?
Ilmoitukset ovat keinoja niiden seurantaa Azure resurssin mittarit, tapahtumia, tai lokit ja sitten ilmoituksen kun määritetty ehto täyttyy.

Voit vastaanottaa ilmoituksia perusteella:

- **Metrijärjestelmän arvot**: Tämä ilmoitus tuottaa, kun tietyn mittarin arvo ylittää raja-arvon, joka määrittää jompaankumpaan suuntaan. Toisin sanoen se käynnistää sekä kun ehto täyttyy ja sitten myöhemmin, kun ehto ei enää ole täyty.
- **Tehtävän tapahtumien poistaminen**: Tämä ilmoitus voi käynnistää jokaisen tapahtuman tai vain silloin, kun tietty määrä tapahtumien ilmetä.

## <a name="alerts-in-different-azure-services"></a>Eri Azure palveluiden ilmoitukset

Ilmoitukset ovat käytettävissä kaikissa eri palveluja, kuten:

- **Sovelluksen tietoja**: mahdollistaa WWW-testi ja metrijärjestelmän ilmoitukset. Katso [sovelluksen havainnollistamisen ilmoitusten määrittäminen](../application-insights/app-insights-alerts.md) ja [näytön käytettävyys- ja minkä tahansa verkkosivun vasteaikaa](../application-insights/app-insights-monitor-web-app-availability.md).
- **Lokitiedoston Analytics (toimintojen hallinta-ryhmä)**: ottaa vianmäärityslokit, loki Analytics reitityksen. Toimintojen hallinta Suite avulla metrijärjestelmä log ja ilmoitusten muuntyyppisten. Lisätietoja [lokin Analytics ilmoitukset](../log-analytics/log-analytics-alerts.md).  
- **Azure valvonta**: mahdollistaa ilmoituksia sekä metrisillä arvot tehtävän tapahtumien poistaminen. Azure näyttö sisältää [Azure näytön REST API](https://msdn.microsoft.com/library/dn931943.aspx).  Lisätietoja on artikkelissa [Azure portaalin, PowerShell- tai voit luoda ilmoituksia komentorivivalitsimet-liittymän avulla](insights-alerts-portal.md).

## <a name="alert-actions"></a>Ilmoitusten toiminnot
Voit määrittää ilmoituksen seuraavasti:

- Lähetä sähköposti-ilmoitusten, palvelun järjestelmänvalvoja, apuyhteyshenkilöiden tai ylimääräisten sähköpostiosoitteiden, jotka määrität.
- Soita webhook, jonka avulla voit käynnistää muita automaattiset toiminnot.

 ![Ilmoitusten kuvaus](./media/monitoring-overview-alerts/AlertsOverviewResource3.png)


## <a name="next-steps"></a>Seuraavat vaiheet

Saat ilmoituksen säännöt ja määrittämisestä niiden avulla:

- [Azure portal](insights-alerts-portal.md)
- [PowerShellin](insights-alerts-powershell.md)
- [Käyttöliittymä (CLI)](insights-alerts-command-line-interface.md)
- [Azure näytön REST-Ohjelmointirajapinnalla](https://msdn.microsoft.com/library/azure/dn931945.aspx)

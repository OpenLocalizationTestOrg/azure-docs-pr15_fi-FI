<properties 
   pageTitle="Lokitiedoston Analytics tietolähteiden | Microsoft Azure"
   description="Tietolähteiden määrittää lokiin Analytics kerää tekijöiden ja muu yhdistetty lähteistä tiedot.  Tässä artikkelissa kuvataan, miten Log analyysin tietolähteiden, tässä artikkelissa kerrotaan, miten ne määritetään tietoja ja yhteenvedon käytettävissä eri tietolähteistä käsite."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="bwren" />

# <a name="data-sources-in-log-analytics"></a>Lokitiedoston Analytics tietolähteet

Lokitiedoston Analytics kerää tietoja OMS työtilassa yhdistetty lähteistä ja tallentaa sen OMS säilöön.  Tiedot, jotka kerätään kaikkien määritetään tietolähteistä, joita voit määrittää.  OMS säilössä tiedot tallennetaan tietuejoukon.  Kunkin tietolähteen Luo tietueet tietyntyyppinen kullakin virhelajilla on oma tietyt ominaisuudet.

![Kirjaudu Analytics-tietojen kerääminen](./media/log-analytics-data-sources/overview.png)

Tietolähteet ovat erilaisia kuin OMS ratkaisuja, jotka myös kerää tietoja yhdistetty lähteistä ja luoda tietueita OMS säilössä.  Ratkaisujen voidaan lisätä työtilan ratkaisuvalikoimasta ja antaa yleensä muita Analyysityökalujen OMS-portaalissa.  

## <a name="summary-of-data-sources"></a>Yhteenveto tietolähteistä

Seuraavassa taulukossa on lueteltu tietolähteitä, jotka ovat tällä hetkellä käytettävissä Log Analytics.  Jokaisella on linkki erillisessä artikkeliin, joka tarjoaa tietolähteen tiedot.

| Tietolähde | Tapahtumalaji | Kuvaus |
|:--|:--|:--|
| [Mukautetun lokit](log-analytics-data-sources-custom-logs.md) | \<Loki\>_CL | Windows- tai Linux agenttien sisältävä lokitiedot tekstitiedostoja. |
| [Windowsin tapahtumalokien](log-analytics-data-sources-windows-events.md) | Tapahtuman | Tapahtumien kerätty tapahtumaloki Windows-tietokoneissa. |
| [Windowsin suorituskyky laskureita](log-analytics-data-sources-performance-counters.md) | Perf | Suorituskyvyn laskureita kerätty Windows-tietokoneissa. |
| [Linux suorituskyvyn laskureita](log-analytics-data-sources-performance-counters.md) | Perf | Suorituskyvyn laskureita kerätä Linux tietokoneista. |
| [IIS-lokit](log-analytics-data-sources-iis-logs.md) | W3CIISLog | Internet Information Services kirjaa W3C-muodossa. |
| [Syslog](log-analytics-data-sources-syslog.md) | Syslog | Syslog tapahtumat Windows- tai Linux-käyttöjärjestelmissä. |

## <a name="configuring-data-sources"></a>Tietolähteiden määrittäminen

Voit määrittää **Tietoasetukset** tietolähteitä Log Analytics- **asetukset**.  Kaikkien yhdistettyjen lähteiden OMS työtilassa toimitetaan kokoonpanoa.  Ei voi tällä hetkellä pois määritysten minkä tahansa agenttien vuoksi.

![Windowsin tapahtumien määrittäminen](./media/log-analytics-data-sources/configure-events.png)

2. Valitse OMS-konsolin **asetukset** -ruutu.
3. Valitse **tiedot**.
4. Valitse tietolähteen määrittäminen.
5. Lisätietoja edellä olevassa taulukossa kunkin tietolähteen ohjeista linkkiä niiden määritysten.

## <a name="data-collection"></a>Tietojen kerääminen

Tietojen lähteen käyttömahdollisuudet toimitetaan agenttien vuoksi, joka on suoraan yhteydessä OMS muutaman minuutin kuluessa.  Määritetyt tiedot on kerätty agentti ja toimitetaan suoraan Log Analytics tietyn kunkin tietolähteen väliajoin.  Lisätietoja näiden kunkin tietolähteen ohjeissa.

System Center Operations Manager (SCOM) yhdistetyn hallinta-ryhmässä agenttien vuoksi tietojen lähteen määrityksiä on käännetään management Pack-paketit ja toimitettu hallinta-ryhmään oletuksena 5 minuutin välein.  Agentti Lataa management Pack-paketti, kuten mihin tahansa muuhun ja kerää halutut tiedot. Riippuen tietolähteen tiedot päivitetään hallinta palvelimeen, joka välittää tiedot Log Analytics lähetetään joko tai agentti lähettää tiedot Analytics lokiin avaamatta asiakirjanhallinnan palvelimeen. Lisätietoja on lisätietoja [sivustokokoelman tietojen OMS ominaisuudet ja ratkaisut](log-analytics-add-solutions.md#data-collection-details-for-oms-features-and-solutions) .  On lisätietoja tiedot yhdistetään SCOM ja OMS ja muokkaamalla taajuus, kokoonpanon [Määrittäminen System Center Operations Manager integrointi](log-analytics-om-agents.md)on toimitettu.

## <a name="log-analytics-records"></a>Kirjaudu Analytics-tietueet

Kaikki lokin Analytics keräämät tiedot tallennetaan OMS säilö tietueiksi.  Eri tietolähteistä keräämiä tietueet saa oman tietyt ominaisuudet ja **niiden tyypiksi** tunnistetaan.  Katso ohjeet kunkin tietolähteen ja ratkaisu lisätietoja kunkin tietuetyypin.


## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja [ratkaisuja](log-analytics-add-solutions.md) , jotka Lisää toimintoja Log Analytics ja koota tietoja myös OMS säilö.
- Lisätietoja [lokin haut](log-analytics-log-searches.md) tietolähteet ja ratkaisujen kerättyjen tietojen analysointia varten.  
- Määritä [ilmoitukset](log-analytics-alerts.md) ilmoittaa sinulle tärkeitä tietoja kerätty tietolähteet ja ratkaisuja, kun muutoksista.

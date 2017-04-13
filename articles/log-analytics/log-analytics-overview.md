<properties
   pageTitle="Lokitiedoston Analytics-ominaisuudet | Microsoft Azure"
   description="Lokitiedoston analyysin on palvelu-toimintojen hallinta Suite (OMS), jonka avulla voit kerätä ja toiminnallisia oman cloud resurssien luomat tietojen analysointia ja paikallisen ympäristön.  Tässä artikkelissa esitellään lyhyesti eri osien Log analyysin ja linkkejä yksityiskohtaiset sisältöön."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/06/2016"
   ms.author="bwren" />

# <a name="what-is-log-analytics"></a>Lokitiedoston Analytics-ominaisuudet
Lokitiedoston Analytics on palvelu [Toimintojen hallinta Suite \(OMS\) ](../operations-management-suite/operations-management-suite-overview.md) , avulla voit kerätä ja analysoida tietoja oman cloud resurssien luomat ja paikallisten ympäristöjen. Tällä tavalla saat reaaliaikaiset tiedot integroitu haku- ja mukautettu raporttinäkymien käyttäminen haluat analysoida helposti tietueiden miljoonia kaikista toiminnoista ja palvelinten niiden fyysinen paikasta riippumatta.


## <a name="log-analytics-components"></a>Kirjaudu Analytics-osat
Osoitteessa center, loki analysointitietoja on joka Azure pilvipalvelussa nykyisessä OMS säilö.  Kerää tietoja kyselyjä säilö yhdistetyn lähteistä määrittäminen tietolähteiden ja lisäämällä ratkaisut-tilaukseen.  Tietolähteiden ja ratkaisut kunkin Luo eri tietuetyyppejä, joka sisältää omia ominaisuuksia, mutta edelleen voi analysoida yhdessä kyselyissä säilöön.  Voit käyttää samoja työkaluja ja menetelmiä eri lähteistä keräämät erilaisia käyttöä varten.


![OMS säilöön](media/log-analytics-overview/overview.png)


Yhdistettyjen lähteiden ovat tietokoneiden ja muita resursseja, joka luo lokitiedoston Analytics keräämät tiedot.  Tämä [yhdistetty System Center Operations Manager hallinta-ryhmässä](log-analytics-om-agents.md)tekijöiden asennettuina [Windows](log-analytics-windows-agents.md) ja [Linux](log-analytics-linux-agents.md) tietokoneissa, joissa muodostaa yhteyttä suoraan tai agenttien vuoksi sisällyttää.  Lokitiedoston Analytics voit koota tietoja myös [Azure-tallennustilan](log-analytics-azure-storage.md).

[Tietolähteet](log-analytics-data-sources.md) ovat erilaisia kerättyjä tietoja, kunkin yhdistetyn lähteestä.  Tämä sisältää tapahtuma- ja [Windows](log-analytics-data-sources-windows-events.md) - ja Linux tekijöiden lisäksi lähteistä, kuten [IIS-lokit](log-analytics-data-sources-iis-logs.md)ja [mukautetun tekstin lokit](log-analytics-data-sources-custom-logs.md) [suorituskykytietoja](log-analytics-data-sources-performance-counters.md) .  Voit määrittää kunkin tietolähteen, jotka haluat kerätä ja määritykset toimitetaan automaattisesti kunkin yhdistetyn lähde.


## <a name="analyzing-log-analytics-data"></a>Lokitiedoston Analytics-tietojen analysoiminen
Useimmat yhteytesi Log Analytics on palvelun OMS-portaalissa, joka toimii kaikissa selaimissa ja on voit käyttää asetuksia ja useita työkaluja, analysoida ja toimia kerättyjä tietoja.  Portaalista avulla voidaan hyödyntää [log haut](log-analytics-log-searches.md) kohtaa, johon voit luoda kyselyjä kerättyjen tietojen, jota mukauttamalla graafinen arvokkain hakuja ja [ratkaisut](log-analytics-add-solutions.md) -näkymien jotka tarjoavat lisätoimintoja ja -Analyysityökalujen käyttäminen [raporttinäkymät](log-analytics-dashboards.md) analysointia varten.

![OMS portal](media/log-analytics-overview/portal.png)


Lokitiedoston Analytics on kyselysyntaksia nopeasti hakeminen ja koota tietoja säilössä.  Voit luoda ja tallentaa [Lokin haut](log-analytics-log-searches.md) suoraan portaalin OMS tietojen analysointia varten tai on lokin hakujen suorittaminen automaattisesti luoda ilmoituksen, jos kyselyn tulokset osoittavat tärkeitä ehto.

![Log-haku](media/log-analytics-overview/log-search.png)

Antaa nopeasti graafinen yleinen ympäristön terveyden, voit lisätä [raporttinäkymät-ikkunan](log-analytics-dashboards.md)visualisoinnit tallennettu loki hakuja varten.   

![Raporttinäkymät-ikkunan](media/log-analytics-overview/dashboard.png)

Jotta voit analysoida tietoja Log Analytics ulkopuolella, voit viedä tiedot OMS säilöstä kyselyjä työkaluilla, kuten [Power BI](log-analytics-powerbi.md) - tai Excel.  Voit myös hyödyntää [Log haun Ohjelmointirajapinta](log-analytics-log-search-api.md) mukautettuja ratkaisuja, jotka hyödyntää Log Analytics-tiedot tai muista järjestelmistä integroida.

## <a name="solutions"></a>Ratkaisut
Ratkaisujen Lisää toimintoja Log Analytics.  Ne ensisijaisesti Suorita pilveen ja anna OMS säilössä kerättyjen tietojen analysointi. Ne voivat määritellä myös uusia tietuetyyppejä voi kerätä, joka voidaan analysoida Log hakujen tai muita käyttöliittymän myöntämä OMS koontinäytön ratkaisu.  

![Muuta seuranta-ratkaisu](media/log-analytics-overview/change-tracking.png)


Ratkaisujen voidaan käyttää tietyt toiminnot ja voit siirtyä helposti käytettävissä ratkaisuihin ja [lisätä ne OMS työtilan](log-analytics-add-solutions.md) ratkaisuvalikoimasta.  Monet otetaan automaattisesti käyttöön ja aloita työskentely välittömästi, kun muut saattaa edellyttää joitakin määritys.

![Ratkaisuvalikoima](media/log-analytics-overview/solution-gallery.png)

## <a name="log-analytics-architecture"></a>Kirjaudu Analytics-arkkitehtuuri
Lokitiedoston Analytics käyttöönottovaatimukset ovat mahdollisimman vähän, koska keskitetyn osien isännöidään Azure pilveen.  Tämä sisältää lisäksi palvelut, jotta voit yhdistää ja analysoida kerättyjen tietojen säilö.  Portaalin voi käyttää kaikissa selaimissa, joten ei tarvita Asiakasohjelmistoa.

Sinun on asennettava agenttien vuoksi [Windows](log-analytics-windows-agents.md) - ja [Linux](log-analytics-linux-agents.md) tietokoneissa, mutta ei ole muita agentti tarvitaan tietokoneissa, joissa on jo [yhdistetty SCOM hallinta ryhmän](log-analytics-om-agents.md)jäsenet.  SCOM tekijöiden säilyvät hallinta palvelinten, joka välittää tietonsa Log Analytics kanssa.  Jotkin ratkaisut vaikka edellyttävät viestintään Log Analytics agenttien vuoksi.  Kunkin ratkaisu ohjeissa määrittää sen viestintä vaatimuksia.

Kun olet [Log Analytics rekisteröityminen](log-analytics-get-started.md), Luo OMS-työtila.  Voit ajatella työtilan yksilöllinen OMS-ympäristössä, jossa on oma tietojen säilöön, tietolähteet ja ratkaisuja. Voit luoda useita työtilat tukemaan useita ympäristöissä, kuten tuotannon ja testaa-tilaukseesi.

![Kirjaudu Analytics-arkkitehtuuri](media/log-analytics-overview/architecture.png)


## <a name="next-steps"></a>Seuraavat vaiheet

- Voit esikatsella oman ympäristön [vapaa Log Analytics-tilin rekisteröiminen](log-analytics-get-started.md) .
- Näytä eri [Tietolähteisiin](log-analytics-data-sources.md) käytettävissä koota tietoja OMS säilö.
- Lisää toimintoja Log Analytics [Selaa käytettävissä olevia ratkaisuja ratkaisuvalikoimaan](log-analytics-add-solutions.md) .

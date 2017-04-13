<properties 
   pageTitle="Ilmoita seuranta tuotteiden Microsoft management | Microsoft Azure"
   description="Ilmoituksen osoittaa joitakin ongelman, joka edellyttää järjestelmänvalvojan toimia.  Tässä artikkelissa kuvataan, miten ilmoitukset luodaan ja hallitaan System Center Operations Manager (SCOM) ja kirjaudu Analytics eroihin ja parhaita käytäntöjä hyödyntäminen hybrid ilmoitusten hallintastrategia kaksi tuotteet." 
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/06/2016"
   ms.author="bwren" />

# <a name="managing-alerts-with-microsoft-monitoring"></a>Microsoft valvontaan ilmoitusten hallinta 

Ilmoituksen osoittaa joitakin ongelman, joka edellyttää järjestelmänvalvojan toimia.  Hallittavat System Center Operations Manager (SCOM) ja kirjaudu Analytics toimintojen hallinta Suite (OMS) kuinka ilmoitusten luodaan, miten ne hallita ja analysoida ja siitä, miten saavat ilmoituksen kriittinen ongelman havaita kannalta.

## <a name="alerts-in-operations-manager"></a>Operations Manager ilmoitukset
Ilmoitukset SCOM luomat yksittäisten sääntöjen tai näyttöjen osoittamaan kutakin ongelmaa.  Näyttö voi luoda ilmoituksen, kun se tulee virhetilan, kun sääntö voi luoda ilmoituksen osoittamaan joitakin tärkeitä ongelman, joka on eivät suoranaisesti liity objektia, jota hallitaan kunto.  Management Pack-paketit sisältävät erilaisia ajan työnkulut, Luo ilmoituksia sovelluksen tai palvelun ne hallinta.  Uusi hallintapaketin määrittämisestä yhteydessä on säätäminen se varmistaaksesi, että et saa liiallinen ilmoitusten ongelmat, jotka ole roskapostia kriittinen.

![SCOM ilmoitusten näkymä](media/operations-management-suite-monitoring-alerts/scom-alert-view.png)

SCOM avulla ilmoitukset ottaa tila, joka voi muuttaa Järjestelmänvalvojat, koska ne toimivat voit ratkaista ongelman valmis ilmoitusten hallinta.  Kun ongelma on ratkaistu, järjestelmänvalvoja määrittää suljettu ilmoitus, jolloin sitä ei enää näy aktiivinen ilmoitusten näyttäminen näkymissä.  Ilmoituksia, jotka on luotu näyttöjen voit ratkaistava automaattisesti, kun näyttö palauttaa kunnossa tilaan.

![Ilmoitusten tila](media/operations-management-suite-monitoring-alerts/scom-alert-status.png)

## <a name="alerts-in-log-analytics"></a>Lokitiedoston Analytics ilmoitukset
Ilmoituksen Log Analytics luodaan loki kysely, joka suoritetaan automaattisesti säännöllisin väliajoin.  Voit luoda ilmoituksen säännön loki kyselyn.  Jos kysely palauttaa tuloksia, jotka vastaavat määrittämiesi ehtojen, ilmoituksen luodaan.  Tämä voi johtua tietyn kyselyn, joka luo ilmoituksen, jos tietty tapahtuma havaitaan tai voi käyttää yleisiä kysely, joka etsii liittyvät erityisesti sovelluksen virhe tapahtuman.

Log Analytics-ilmoitukset kirjoitetaan OMS tapahtumana säilöön, ja voit hakea lokin kyselyn avulla.  Hän ei ole tila, kuten SCOM tapahtumat niin, että voit määrittää, kun ongelma on ratkaistu.

![OMS ilmoitus](media/operations-management-suite-monitoring-alerts/oms-alert.png)

Kun SCOM käytetään tietolähteenä Log Analytics-SCOM ilmoitukset kirjoitetaan OMS säilöön, kun ne on luotu ja muokata.  

![SCOM ilmoitus](media/operations-management-suite-monitoring-alerts/scom-alert.png)

[Ilmoitusten hallintaratkaisu](http://technet.microsoft.com/library/mt484092.aspx) yhteenvedon aktiivisen ilmoituksia ja useita yleisiä kyselyjä, jotka hakevat eri määrittää ilmoituksia.  Tämä voi tehokkaita analyysi ilmoituksia kuin SCOM raportin.  Voit siirtyä alaspäin yhteenvetoja-yksityiskohtaiset tiedot ja luoda itsenäisten kyselyjen hakemiseen erilaisia arvojoukkoja ilmoitukset.

![Ilmoitusten hallintaratkaisu](media/operations-management-suite-monitoring-alerts/alert-management.png)

## <a name="notifications"></a>Ilmoitukset
Ilmoitusten SCOM lähettää sinulle sähköposti- tai tekstin vastauksena ilmoituksia, jotka vastaavat tiettyä ehtoja.  Voit luoda eri ilmoitustilaukset, jotka ovat eri henkilöt ilmoituksen sen mukaan, kuten ehtoja seurataan objektina, ilmoituksen vakavuus, ongelman, joka tunnistaa, millaisia tai kellonaika.

Muutamia tilaukset voidaan toteuttaa management Pack-paketit on runsaasti täydellisen ilmoituksen strategia.

![SCOM ilmoitukset](media/operations-management-suite-monitoring-alerts/alerts-overview-scom.png)

Lokitiedoston Analytics voit ilmoittaa sähköpostin kautta ilmoituksen luotu määrittämällä sähköposti-ilmoituksen toiminnon niiden [hälytyksen](http://technet.microsoft.com/library/mt614775.aspx).  Se ei ole mahdollisuutta SCOM useita ilmoituksia yksittäisen säännön tilaa.  Haluat myös luoda ilmoituksen säännöistä, koska OMS ei tarjoa jokin valmiiksi.

![Kirjaudu Analytics-ilmoitukset](media/operations-management-suite-monitoring-alerts/alerts-overview-oms.png)

Et voi kokonaan hallita SCOM ilmoitukset Log Analytics vaikka jälkeen voit muokata niitä vain toiminnot-konsolissa.  Lokitiedoston Analytics on hyödyllinen ilmoitusten hallinta osa käsitellä vaikka varten antamisen Analyysityökalujen yksin kyseisen SCOM ei ole.

## <a name="alert-remediation"></a>Ilmoitusten korjaus
[Korjaus](http://technet.microsoft.com/library/mt614775.aspx) viittaa yrittää korjata ongelman merkittyä ilmoituksen automaattisesti.
  
SCOM avulla voit suorittaa diagnostiikka- ja saadut näyttöä kirjoittaminen virheellisessä tilassa.  Tähän näyttölaitteeseen luomista ilmoituksen samanaikaisesti.  Komentosarjan, joka suoritetaan agentti yleensä toteutettu diagnostiikka- ja saadut.  Kerää lisätietoja havaittu ongelma, kun palautuksen yrittää korjata ongelman yrittää vianmäärityksen.

Log-analyysin avulla voit [Azure automaatio runbookin](https://azure.microsoft.com/documentation/services/automation/) aloittaminen tai puhelun webhook saatuaan Log Analytics-ilmoitus.  Runbooks voi olla monimutkaisia logiikan PowerShellin käytössä.  Komentosarja suoritetaan Azure ja voivat käyttää mitä tahansa Azure resursseja tai Ulkoiset resurssit käytettävissä pilveen.  Azure automaatio on voi suorittaa runbooks paikallisen joten palvelimessa, mutta tämä toiminto ei ole tällä hetkellä käytettävissä käynnistettäessä: n runbookin vastauksena Log Analytics-ilmoitukset.

SCOM saadut sekä runbooks OMS voi sisältää PowerShell-komentosarjojen, mutta saadut on vaikea luominen ja hallinta, koska ne on sisällyttävä management pack.  Runbooks on tallennettu Azure automaatio, joka sisältää ominaisuuksia, jotka yhtä aikaa muiden kanssa, testaus ja runbooks hallinta.

Jos käytät SCOM lokin Analytics tietolähteenä, voit luoda loki kyselyn avulla voit hakea SCOM ilmoitusten OMS säilöön tallennettuja Log Analytics-ilmoitus.  Tämä mahdollistavat suorittaminen Azure automaatio-runbookin SCOM-ilmoitus.  Koska: n runbookin suoritetaan Azure, ei ole elinkykyisten strategia saadut paikallisen ongelmista.

## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja ilmoitusten [System Center Operations Manager (SCOM)](https://technet.microsoft.com/library/hh212913.aspx).
<properties
    pageTitle="Mitä on tietojen tiede Virtual kone? | Microsoft Azure"
    description="Katso keskeiset skenaariot ominaisuuksista, ja aloita kanssa tietojen tiede näennäiskoneiden-ympäristön ja työkalujen valmis analysoinnissa."
    keywords="Datatyökalut tiede, tietojen tiede virtuaalikoneen, tietojen tiede linux tietojen tiede Työkalut"
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="bradsev" />


# <a name="introduction-to-the-cloud-based-data-science-virtual-machine-for-linux-and-windows"></a>Johdanto, pilvipohjainen tietojen tiede virtuaalikoneen Linux-ja Windows

Tietoja tiede virtuaalikoneen on Microsoft Azure cloud laadittuihin varten suorittamalla tietojen tiede mukautetun AM-kuva. Siinä on monet Suositut tietojen tiede ja muita työkaluja esiasennettuna ja ennalta määritetty Nopeuta kehittyneen analyysin älykkäät-sovellusten luomiseen. Se on käytettävissä Windows Server 2012: ssa tai Linux OpenLogic 7.2 CentOS-pohjainen versio. 

Tässä ohjeaiheessa kerrotaan, mitä voit tehdä tietojen tiede AM, käsitellään joitakin käyttämiseen AM skenaariota, itemizes käytettävissä Windows ja Linux-versioiden keskeisiä ominaisuuksia ja ohjeet Aloita niiden.


## <a name="what-can-i-do-with-the-data-science-virtual-machine"></a>Mitä voin tehdä kanssa tietojen tiede virtuaalikoneen?

Tavoite, tietojen tiede virtuaalikoneen myös antaa tietoja asiantuntijoille kaikilla osaamisalueiden tasoilla ja roolien hankaukselle tiedot tiede-ympäristössä. Tämä AM säästät huomattavasti Jos olet ollut esiin vastaa ympäristön Omat kuluvaa aikaa. Sen sijaan Aloita tietojen tiede projektin heti juuri luomasi AM esiintymän. 

Tietoja tiede AM on suunniteltu ja määritetty käsittelyyn laaja käyttötapoja. Voit skaalata ympäristön ylös tai alas projektin tarpeiden muuttuessa. Pystyt käyttämään ohjelma tietojen tiede tehtävät haluamallasi kielellä. Voit asentaa muita työkaluja ja mukauta järjestelmä tarkka tarpeisiin.
 
## <a name="key-scenarios"></a>Keskeiset skenaariot
Tässä osiossa ehdotetaan joitakin skenaariota, jonka tiedot tiede AM otetaan käyttöön.

### <a name="preconfigured-analytics-desktop-in-the-cloud"></a>Valmiiksi määritetyt analytics työpöydän pilveen

Tietoja tiede AM sisältää tietoja tiede ryhmiä, haluatko korvata paikallisen joutumatta hallitun cloud työpöydän perusaikataulun konfiguroinnin. Perusaikatauluun varmistaa, että kaikki tiedot-ryhmän tutkijoiden, johon voit vahvistaa kokeissa ja sisältöihin yhdenmukaisia asetukset. Se myös vähentää kustannuksia vähentämällä sysadmin rasitteen ja arvioida, asenna ja ylläpitää eri ohjelmistopakettien Tee kehittyneen analyysin suorittamiseen tarvittava aika tallennetaan.  

### <a name="data-science-training-and-education"></a>Tietoja tiede-koulutus ja koulutus

Yrityksen kouluttajille ja opettajille, opeta tietojen tiede luokkia on yleensä virtuaalikoneen-kuva, kun haluat varmistaa, että oppilaita on yhtenäinen asetukset ja esimerkit toimivat laadukkaampi. Tietoja tiede AM Luo tarvittaessa ympäristön yhdenmukaisia määritykset, joka helpottaa tuki- ja eivät ole yhteensopivia haasteita. Tapaukset, joissa näissä ympäristöissä tarpeen laadittuihin usein, erityisesti niiden lyhentää kursseja hyötyä merkittävästi.

### <a name="on-demand-elastic-capacity-for-large-scale-projects"></a>Tarvittaessa joustavasti kapasiteetin suurissa projekteissa

Tietoja tiede hackathons/kilpailut tai suurissa tietomallien ja tarkasteluun vaativat skaalata ulos laitteiston kapasiteetin yleensä lyhyt ajaksi. Tietoja tiede AM auttaa replikoida nopeasti pyydettäessä, skaalatun ulos palvelimissa, joiden avulla kokeissa edellyttävän tehokkaat tietojenkäsittely resurssien avulla voidaan suorittaa tietojen tiede-ympäristössä.

### <a name="short-term-experimentation-and-evaluation"></a>Lyhytkestoinen vuorovaikutteisuudesta ja arviointi

Tietoja tiede AM voi olla laskeminen ja lue työkaluilla, kuten Microsoft R Server, SQL Serveristä, käytettävä Visual Studio työkalut, Jupyter, kattavaa Koulujen / ML ohjelmistosuunnittelijoille ja uusia työkaluja Suositut yhteisön kanssa mahdollisimman vähän asennuksen työmäärään. Koska nopeasti tietoja tiede AM voit määrittää, se voi käyttää muiden lyhytkestoinen käyttötapoja, kuten replikoiminen julkaistun tutkimuksiin suoritetaan esittelyt, seuraava vaihe vaiheelta online istunnot tai kokouksen opetusohjelmat.


## <a name="whats-included-in-the-data-science-vm"></a>Mitä sisältyy tietojen tiede AM?

Tietoja tiede virtuaalikoneen on monet Suositut tiede Datatyökalut jo asentanut ja määrittänyt. Se on myös työkaluista, joiden avulla on helppo käyttää eri Azure tiedot ja analysoinnin tuotteiden. Voit tutkia ja kehittää ennakoivan mallien suurissa tietojoukot käyttäen Microsoft-R Server tai käyttämällä SQL Server 2016. Muita työkaluja lukuisiin Avaa lähde-yhteisöltä ja Microsoftin on myös mukana sekä esimerkki koodi ja muistikirjat. Seuraavassa taulukossa itemizes ja vertaa Windowsin ja Linux-versioihin sekä tietojen tiede virtuaalikoneen pääosaa.


|**Windows-version** | **Linux Edition** |
|----------------|---------------|
|Microsoft R Server Developer Edition | Microsoft R Server Developer Edition |
|Anaconda Python 2.7, 3.5 | Anaconda Python 2.7, 3.5 |
|Jupyter muistikirjan Server (R, Python) | JupyterHub: Usean käyttäjän Jupyter muistikirjat (R Python, Julia) |
|SQL Server 2016 Developer Edition: Skaalattava tietokanta-analyysin R-palvelujen kanssa | Postgres, SQuirreL SQL (tietokantatyökalu) tai SQL Server-ohjaimet komentoriviltä (bcp, sqlcmd) |
|Visual Studio yhteisön Edition 2015 (IDE) </br> -Azure Hdinsightiin (Hadoop), tietojen järvi SQL Server Data tools </br> -Visual Studio Node.js, Python ja R-Työkalut |IDEs ja editors </br> -Pimennys Azure työkalujen laajennuksen kanssa </br> -Emacs (ja ESS auctex) gedit |
|Power BI Desktopin | -- |
|Koneen Learning Työkalut </br> -Azure koneen Learning integrointi </br> -CNTK (laaja learning/AI) </br> -Xgboost (Suositut ML työkalun tietojen tiede kilpailut) </br> -Vowpal Wabbit (nopea online paremmin katselemalla) </br> -Rattle (visuaalinen quick start tiedot ja analytics-työkalun) </br> -Mxnet (laaja learning/AI) | Koneen Learning Työkalut </br> -Integroinnit Learning Azure tietokoneen kanssa </br> -CNTK (laaja learning/AI) </br> -Xgboost (Suositut ML työkalun tietojen tiede kilpailut) </br> -Vowpal Wabbit (nopea online paremmin katselemalla) </br> -Rattle (visuaalinen quick start tiedot ja analytics-työkalun)  |
| Azure ja Cortana liiketoimintatietojen Suite palvelujen SDK: T | Azure ja Cortana liiketoimintatietojen Suite palvelujen SDK: T |
| Työkaluja tietojen siirto ja Azure ja Big datasta hoito: Azure-tallennustilan Explorer, CLI, PowerShell, AdlCopy (Azure tietojen järvi), AzCopy, dtui (for DocumentDB), Microsoft Data Management Gateway | Työkaluja tietojen siirto ja Azure ja Big datasta hoito: Azure-tallennustilan Explorer, CLI |
| Git, Visual Studio Team Services-laajennus | Git |
| Windows-portin suosituimpia Linux/Unix komentorivin apuohjelmia GitBash/komentokehote kautta | -- |



## <a name="how-to-get-started-with-the-windows-data-science-vm"></a>Miten Windows tietojen tiede AM käytön aloittaminen

- Luo erillisen AM Windows Siirtyminen [tälle](https://azure.microsoft.com/marketplace/partners/microsoft-ads/standard-data-science-vm/) sivulle ja valitsemalla vihreää **luominen virtuaalikoneen** -painiketta.
- Kirjaudu sisään AM etätyöpöydän määritit, kun olet luonut AM tunnistetiedoilla.
- Löydät ja Käynnistä käytettävät työkalut, valitse **Käynnistä** -valikko.


## <a name="get-started-with-the-linux-data-science-vm"></a>Linux tietojen tiede AM käytön aloittaminen

- Luo erillisen AM Linux (OpenLogic CentOS-pohjainen) Siirtyminen [tälle](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm/) sivulle ja valitsemalla **Luo virtuaalikoneen** -painike.
- Kirjaudu sisään AM SSH-asiakasohjelmassa, kuten painovärit, muste tai SSH-komento, määrittämäsi, kun loit AM tunnistetiedoilla.
- Kirjoita shell-kehote dsvm Lisää-tiedot.
- Graafisen työpöydän asiakas-ympäristön X2Go-asiakasohjelman lataaminen [tätä](http://wiki.x2go.org/doku.php/doc:installation:x2goclient) ja noudata ohjeita Linux tietojen tiede AM asiakirjan [säännöstä Linux tiede virtuaalikoneen](machine-learning-data-science-linux-dsvm-intro.md#installing-and-configuring-x2go-client).


## <a name="next-steps"></a>Seuraavat vaiheet

### <a name="for-the-windows-data-science-vm"></a>Windowsin tietojen tiede AM varten

- Katso lisätietoja siitä, miten voit suorittaa tiettyjä työkaluja Windows-version [säännöstä Microsoft tiede virtuaalikoneen](machine-learning-data-science-provision-vm.md) ja
-  Katso lisätietoja siitä, miten voit suorittaa erilaisia tehtäviä, jotka tarvitaan Windows-AM tietojen tiede projektille, [Voit käyttää tietojen tiede virtuaalikoneen-kymmenen toimintoja](machine-learning-data-science-vm-do-ten-things.md).

### <a name="for-the-linux-data-science-vm"></a>Linux tietojen tiede AM varten

- Katso lisätietoja siitä, miten voit suorittaa tiettyjä työkaluja Linux-versio, [säännöstä Linux tiede virtuaalikoneen](machine-learning-data-science-linux-dsvm-intro.md).
- Katso [tietoja tiede-Linux tietojen tiede virtuaalikoneen](machine-learning-data-science-linux-dsvm-walkthrough.md)vaiheittainen opas, joka näytetään, miten voit suorittaa useita yleisiä tietoja tiede tehtävien Linux AM.

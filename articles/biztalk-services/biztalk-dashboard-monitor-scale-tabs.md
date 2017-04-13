<properties 
    pageTitle="Dashboard-näyttö, mittakaava, määrittäminen ja Hybrid yhteydet BizTalk Services-palveluissa | Microsoft Azure" 
    description="Tietoja ohjausobjektit ja valvoa suorituskykyä perinteinen portaalin välilehdillä BizTalk palveluiden: Raporttinäkymät-ikkunan, näyttö, mittakaava, määrittäminen ja Hybrid yhteydet. MAB-WABS" 
    services="biztalk-services" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="biztalk-services" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/23/2016" 
    ms.author="mandia"/>




# <a name="review-the-dashboard-monitor-scale-configure-and-hybrid-connection-tabs"></a>Tarkista Raporttinäkymät-ikkunan, näytön, asteikko, määrittäminen ja yhteyden Hybrid-välilehdet

Kun olet luonut BizTalk-palvelun ja ottaa käyttöön sovelluksen, voit muuttaa joitakin BizTalk Palveluasetukset ja valvoa sovellusten suorituskykyä. 

Avattaessa Azure perinteinen portaalissa voit sijoitetaan automaattisesti **Kaikki kohteet** -välilehden. Voit tarkastella BizTalk-palvelussa, valitse BizTalk-palvelun **Kaikki kohteet** -välilehti tai valitse **BIZTALK palvelut** -välilehti; ja valitse sitten BizTalk palvelun nimi.

Tämä Avaa uudessa ikkunassa seuraavat välilehdet. Tässä ohjeaiheessa kerrotaan välilehdistä.

## <a name="quick-start-quick-startquickstart"></a>Pikaopas)![Pika-aloitusopas][QuickStart])
Sen mukaan, BizTalk Services Edition-luettelossa kaikki vaihtoehdot eivät välttämättä ole käytettävissä. 
<table border="1">
    <tr>
        <td><strong>Hanki työkalut</strong></td>
        <td>Lataa BizTalk Services SDK Asenna Visual Studio projektimallit paikallisen kehittäminen tietokoneeseen. Mallien luominen <strong>BizTalk Services</strong> (silta) ja <strong>BizTalk Palvelutietojen</strong> (muunnos) Visual Studio projektit, jotka on otettu käyttöön BizTalk-palveluun.
        <br/><br/>
        <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302335">Miten voin aloittaa käyttämällä Azure BizTalk Services SDK</a> ja <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=241589">Azure BizTalk Services SDK asentamisesta</a> on esitetty vaiheet, pääset alkuun.
        </td>
    </tr>
    <tr>
        <td><strong>Luo kumppanin toimeenpano</strong></td>
        <td>Avaa isännöimät Azure lisätään kumppanit ja luoda X12, AS2, Azure BizTalk palveluiden portaali ja EDIFACT Muokkaa sopimuksia.
        <br/><br/>
        <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Määritetään osia Muokkaa viestiliikenteen BizTalk palvelut-portaalissa</a> on esitetty vaiheet, pääset alkuun.
        </td>
    </tr>

<tr>
        <td><strong>Lisätietoja BizTalk palvelut</strong></td>
        <td>Siirry <a HREF="http://azure.microsoft.com/documentation/services/biztalk-services/">koulujen center</a> lisätietoja Azure BizTalk palvelut.</td>
</tr>
</table>


Alaosan tehtäväpalkissa voit tehdä seuraavaa:

<table border="1">

<tr>
<td><strong>Hallitse</strong> sovelluksen käyttöönotto</td>
<td>Avaa Azure BizTalk palveluiden portaali. BizTalk palveluiden portaali on sisäänkäynnin Muokkaa konfiguroinnin, mukaan lukien lisäämällä kumppanit ja X12, AS2, luominen ja EDIFACT sopimuksia.
<br/><br/>
Tämä on sama kuin <strong>Luo kumppanin toimeenpano</strong> <strong>Pika-aloitus</strong> -välilehdessä.
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Määritetään osia Muokkaa viestiliikenteen BizTalk palvelut-portaalissa</a> on lisätietoja BizTalk palvelut-portaalissa.</td>
</tr>

<tr>
<td><strong>Yhteyden tietoja</strong> Access-ohjausobjektin Namespace.</td>
<td>Kun valitset yhteyden tietoja, Access ohjausobjektin Namespace, oletus myöntäjä ja oletus avain näkyviin. Voit kopioida nämä arvot.
<br/><br/>
Voit myös avata Access hallinta-portaalissa. <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285670">Luo Access-ohjausobjekti Namespace</a> on lisätietoja käytön hallinta-portaalissa.</td>
</tr>

<tr>
<td><strong>Synkronoi näppäimet</strong> tallennustilan tilin</td>
<td>Kun luot tallennustilan tilin, perusavain ja toissijaisen avaimen luodaan automaattisesti. Nämä salausavaimet hallita tallennustilan-tiliisi. BizTalk-palvelun käyttää perusavaimen automaattisesti. <strong>Synkronoi näppäimet</strong> käyttäjien vaihtaa ensisijaisen ja toissijaisen avaimen ilman toiminnan BizTalk-palvelun käyttöön.
<br/><br/>
Jos esimerkiksi haluat BizTalk-palvelun käyttämään uusi perusavain tallennustilan tilin. Toiminto:
<br/><br/>
<ol>
<li>Valitse BizTalk-palvelu ja valita <strong>Synkronoi-näppäimiä</strong>. Valitse Toissijainen avain. Kun teet näin, BizTalk-palvelu käynnistyy toissijaisen avaimen avulla.</li>
<li>Azure perinteinen-portaalissa Valitse tallennustilan tilin ja luo perusavaimen. Muista, että BizTalk-palvelu on toissijainen avaimen avulla.</li>
<li>Valitse BizTalk-palvelu ja valita <strong>Synkronoi-näppäimiä</strong>. Valitse perusavain. Tämä on uusi perusavaimen, voit luoda uudelleen.</li>
<li>Azure perinteinen-portaalissa Valitse tallennustilan tilin ja luo toissijaisen avaimen.</li>
</ol>
<br/>
Tätä prosessia kutsutaan "palauttaminen näppäimet". Käyttäjät voivat vaihtaa ensisijaisen ja toissijaisen avaimen ilman toiminnan BizTalk-palvelu on.</td>
</tr>

<tr>
<td><strong>Poista</strong> sovelluksen</td>
<td>Kun valitset BizTalk-palvelun poistaminen ja kaikki kohteet, jotka on otettu käyttöön, se poistetaan.</td>
</tr>
</table>


## <a name="dashboard"></a>Raporttinäkymät-ikkunan
Sen mukaan, BizTalk Services Edition-luettelossa kaikki vaihtoehdot eivät välttämättä ole käytettävissä. 

Kun valitset BizTalk palvelun nimi, koontinäyttö-välilehti tulee näkyviin. Raporttinäkymät-ikkunassa voit tehdä seuraavaa:

##### <a name="usage-overview-shows-the-number-of-used-hybrid-connections"></a>Käyttö yleiskatsaus: Näyttää käytetyt Hybrid yhteyksien määrä
Näyttää myös tietoliikenteestä Gigatavua. 

##### <a name="metric-graph-shows-a-fixed-list-of-performance-metrics"></a>Metrijärjestelmän kaavio: Näyttää kiinteän luettelon suorituskyvyn mittarit
Nämä arvot Anna BizTalk-palvelun kunto koskevia reaaliaikaisia arvot. Voit myös valita **suhteellinen** tai **absoluuttinen** arvot ja aikavälin arvot, jotka näkyvät kaavio **aikaväli** . 

Näitä suorituskyvyn mittarit kuvauksen Siirry tämän artikkelin [Käytettävissä olevat arvot](#Metrics) .


##### <a name="quick-glance-lists-your-biztalk-service-properties"></a>Quick Glance: Luetteloiden BizTalk Service-ominaisuudet

<table border="1">

<tr>
<td><strong>Päivitä seuranta tietokannan tunnistetietoja</strong></td>
<td>Muuttaa käyttäjänimi ja salasana, jota käytetään kirjautua seuranta-tietokantaan.</td>
</tr>
<tr>
<td><strong>Päivitä SSL-varmenne</strong></td>
<td>Voit päivittää BizTalk-palvelun käyttämään eri SSL-varmenne. Itse allekirjoitetun SSL-varmenteen luodaan automaattisesti, kun <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">BizTalk-palvelun luominen</a>.</td>
</tr>
<tr>
<td><strong>Lataa</strong></td>
<td>Voit ladata BizTalk-palvelussa paikallisen tietokoneen käyttämän SSL-varmenne.</td>
</tr>
<tr>
<td><strong>Tila</strong></td>
<td>Näyttää nykyisen tilan BizTalk-palvelussa. Katso <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=329870">BizTalk Services: palvelun tilan kaavion</a>. </td>
</tr>
<tr>
<td><strong>Palvelun URL-osoite</strong></td>
<td>BizTalk-palvelun URL-osoite. Tämä on sama kuin <strong>Toimialueen URL-osoite</strong> BizTalk-palvelussa luotaessa.</td>
</tr>
<tr>
<td><strong>Julkinen Virtual IP (VIP)-osoite</strong></td>
<td>Määritetty BizTalk-palvelun IP-osoite. Sitä käytetään kaikissa syötteen päätepisteet ja on lähtevän liikenteen lähdeosoite. IP-osoitteen kuuluu BizTalk-palveluun, kunhan se luodaan. Jos poistat BizTalk-palvelu, toinen BizTalk palvelu määritetään IP-osoite.</td>
</tr>
<tr>
<td><strong>ACS Namespace</strong></td>
<td>Todentaa BizTalk-palvelussa.</td>
</tr>
<tr>
<td><strong>Edition</strong></td>
<td>Luettelot version kirjoittaa BizTalk-palvelun luomisen yhteydessä.</td>
</tr>
<tr>
<td><strong>Sijainti</strong></td>
<td>Näyttää maantieteellisen alueen, joka isännöi BizTalk-palvelua.</td>
</tr>
<tr>
<td><strong>Luotu</strong></td>
<td>Näyttää päivämäärän ja kellonajan BizTalk-palvelu on luotu.</td>
</tr>
<tr>
<td><strong>Seurantatietokanta</strong></td>
<td>Azure SQL-tietokannan nimi, joka tallentaa BizTalk-palvelun käyttämä seuranta-taulukoissa. 
<br/><br/>Seuranta-tietokannan 
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">Vaatimukset kuvaus</a> sisältää tietoja.</td>
</tr>
<tr>
<td><strong>Seuranta/arkistointi tallennustila</strong></td>
<td>Azure-tallennustilan tilin nimi, joka tallentaa seurantaa tulosteen BizTalk-palvelussa.
<br/><br/>Tallennustilan tilin 
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">Vaatimukset kuvaus</a> sisältää tietoja.</td>
</tr>
<tr>
<td><strong>Tilauksen nimi</strong></td>
<td>Näyttää tilauksen, joka isännöi BizTalk-palvelua. Tilauksen ohjaa Azure perinteinen portaalin.</td>
</tr>
<tr>
<td><strong>Tilauksen tunnus</strong></td>
<td>Kun tilaus on luotu, tilauksen tunnus luodaan automaattisesti. REST API käytettäessä voit joutua kirjoittamaan tilauksen tunnus.</td>
</tr>
</table>

[BizTalk Services: valmistelu käyttämällä Azure perinteinen portal](http://go.microsoft.com/fwlink/p/?LinkID=302280) on esitetty vaiheet, Luo BizTalk palvelu.


##### <a name="manage-connection-information-sync-keys-and-delete-in-the-task-bar"></a>Yhteyden tietojen synkronointi näppäimet hallinta ja poista tehtäväpalkissa:

<table border="1">

<tr>
<td><strong>Hallitse</strong> sovelluksen käyttöönotto</td>
<td>Avaa Azure BizTalk palveluiden portaali. BizTalk palveluiden portaali on sisäänkäynnin Muokkaa konfiguroinnin, mukaan lukien lisäämällä kumppanit ja X12, AS2, luominen ja EDIFACT sopimuksia.
<br/><br/>
Tämä on sama kuin <strong>Luo kumppanin toimeenpano</strong> <strong>Pika-aloitus</strong> -välilehdessä.
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Määritetään osia Muokkaa viestiliikenteen BizTalk palvelut-portaalissa</a> on lisätietoja BizTalk palvelut-portaalissa.</td>
</tr>
<tr>
<td><strong>Yhteyden tietoja</strong> Access-ohjausobjektin Namespace.</td>
<td>Näyttää Access-ohjausobjektin Namespace, oletus myöntäjän ja oletus avaimen arvot; jossa voidaan kopioida.
<br/><br/>
Voit myös avata Access hallinta-portaalissa. Tässä Access hallinta-portaalissa on sama kuin Active Directory-vaihtoehtoa käytettäessä vasemmanpuoleisessa siirtymisruudussa.
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285670">Hallinta Your ACS Namespace</a> on lisätietoja käytön hallinta-portaalissa.</td>
</tr>
<tr>
<td><strong>Synkronoi näppäimet</strong> tallennustilan tilin</td>
<td>Kun luot tallennustilan tilin, perusavain ja toissijaisen avaimen luodaan automaattisesti. Nämä salausavaimet hallita tallennustilan-tiliisi. BizTalk-palvelun käyttää perusavaimen automaattisesti. <strong>Synkronoi näppäimet</strong> käyttäjien vaihtaa ensisijaisen ja toissijaisen avaimen ilman toiminnan BizTalk-palvelun käyttöön.
<br/><br/>
Jos esimerkiksi haluat BizTalk-palvelun käyttämään uusi perusavain tallennustilan tilin. Toiminto:
<br/><br/>
<ol>
<li>Valitse BizTalk-palvelu ja valita <strong>Synkronoi-näppäimiä</strong>. Valitse Toissijainen avain. Kun teet näin, BizTalk-palvelu käynnistyy toissijaisen avaimen avulla.</li>
<li>Azure perinteinen-portaalissa Valitse tallennustilan tilin ja luo perusavaimen. Muista, että BizTalk-palvelu on toissijainen avaimen avulla.</li>
<li>Valitse BizTalk-palvelu ja valita <strong>Synkronoi-näppäimiä</strong>. Valitse perusavain. Tämä on uusi perusavaimen, voit luoda uudelleen.</li>
<li>Azure perinteinen-portaalissa Valitse tallennustilan tilin ja luo toissijaisen avaimen.</li>
</ol>
<br/>
Tätä prosessia kutsutaan "palauttaminen näppäimet". Käyttäjät voivat vaihtaa ensisijaisen ja toissijaisen avaimen ilman toiminnan BizTalk-palvelu on.</td>
</tr>

<tr>
<td><strong>Poista</strong> sovelluksen</td>
<td>BizTalk-palveluun ja kaikki kohteet, jotka on otettu käyttöön, se poistetaan.</td>
</tr>
</table>


## <a name="monitor"></a>Valvonta
Vapaa Edition ei koske.

Kun valitset BizTalk palvelun nimi, näyttö-välilehti on käytettävissä ja tuo näkyviin seuraavat tiedot:

##### <a name="metric-graph-displays-the-selected-performance-metrics"></a>Metrijärjestelmän kaavio: Näyttää valitun suorituskyvyn mittarit
Nämä arvot Anna BizTalk-palvelun kunto koskevia reaaliaikaisia arvot. Voit valita, mitkä suorituskyvyn mittarit ovat näkyvissä. Enintään kuusi suorituskyvyn mittarit voidaan näyttää samanaikaisesti. 

Voit myös valita **suhteellinen** tai **absoluuttinen** arvot ja aikavälin arvot, jotka näkyvät **aikaväli** . 

##### <a name="to-remove-or-display-metrics-in-the-graph"></a>Voit poistaa tai näyttää kaavion arvot:
1. Valitse **Näyttö** -välilehti.
2. Valitse **Lisää arvot** tehtäväpalkista.  
![Valitse Lisää arvot][AddMetrics]
3. Valitse näytettävien suorituskyvyn mittarit.
4. Valitse palaa **Näyttö** -välilehdessä valintamerkki.
5. Valitse ympyrä metrijärjestelmä kyseinen arvo-arvon näyttämiseen kaavion vieressä.  

    Esimerkiksi **Suorittimen käyttö** arvo näkyy harmaana, tulos ei ole näkyvissä kaaviossa:  
![Suorittimen käyttö arvo näkyy harmaana][GrayedMetric]  

    Valitse ulos ympyrä käyttöön **Suorittimen käyttö** metrijärjestelmä näyttämään tulos kaavio harmaana:  
![Suorittimen käyttö arvo on käytössä][EnabledMetric]

6. Jos haluat poistaa mittarin näyttö-kaavio ja luetteloon, valitse **Poista metrijärjestelmä** tehtäväpalkista. Metrijärjestelmän Edellinen lisääminen luetteloon, valitse **Lisää arvot** tehtäväpalkissa, tarkista lisätiedot ja valitse palaa **Näyttö** -välilehdessä valintamerkki. Valitse harmaa, ympyrän, jotta lisätiedot.

## <a name="Metrics"></a>Käytettävissä olevat arvot
Käytettävissä ovat seuraavat suorituskyvyn laskureita ja arvot:

<table border="1">

<tr>
<td><strong>RountdTrip viive</strong></td>
<td>Näyttää kaikki siltoja yli keskimääräinen aika (ms) millisekunteina keskimäärin viestin siitä, kun viesti on vastaanotettu, kunnes viesti käsitellään täysin BizTalk-palvelun. Funktio laskee käsitteli viesteihin.<br/><br/>
Seuraavista tilanteista toteutuessa aikaleima on luotu:
<ul>
<li>Viestin Lisää yhdyskäytävän</li>
<li>Viesti on reititetty kohteeseen</li>
<li>Kohde vastaus vastaanotetaan.</li>
<li>Yhdyskäytävän lähetettävä kohde kuittaus vastaus</li>
</ul>
<br/>
Tämä arvo näkyy seuraavassa laskutoimituksen tulos:
<br/><br/>
[Kohde kuittaus vastaus lähetetään yhdyskäytävän] - [viesti saapuu yhdyskäytävän]</td>
</tr>
<tr>
<td><strong>Tietolähteen virheitä</strong></td>
<td>Näyttää viestit, jotka epäonnistui kokonaismäärän BizTalk-palvelu, kun tietojen lähteen päätepisteet viestejä.</td>
</tr>
<tr>
<td><strong>Suorittimen käyttö</strong></td>
<td>Näyttää kaikki roolin esiintymät keskimääräinen % suorittimen ajasta.</td>
</tr>
<tr>
<td><strong>Viive käsittely</strong></td>
<td>Näyttää keskimäärin kuluva aika (ms) millisekunteina käyttänyt viestin BizTalk-palvelun kautta kaikki siltoja, lukuun ottamatta kohteet-aika. Funktio laskee käsitteli viesteihin.<br/><br/>
Molemmissa seuraavista tilanteista toteutuessa aikaleima on luotu:

<ul>
<li>Viestin Lisää yhdyskäytävän</li>
<li>Viesti on reititetty kohteeseen</li>
<li>Kohde vastaus vastaanotetaan.</li>
<li>Yhdyskäytävän lähetettävä kohde kuittaus vastaus</li>
</ul>
<br/>Tämä arvo näkyy seuraavassa laskutoimituksen tulos:<br/><br/>
[Kohde kuittaus vastaus lähetetään yhdyskäytävän] - [viesti saapuu yhdyskäytävän] - [kohde vastaus saadaan] + [viesti on reititetty kohteeseen]</td>
</tr>
<tr>
<td><strong>Virheiden prosessi</strong></td>
<td>Näyttää viestit, jotka epäonnistui käsiteltäessä BizTalk-palvelun kautta siltoja ajanjakson kokonaismäärän.</td>
</tr>
<tr>
<td><strong>Lähetetyt viestit</strong></td>
<td>Näyttää lähettämien viestien BizTalk-palvelun kautta kaikki siltoja ajanjakson kokonaismäärän. Tämä arvo suurenee, kun putkijohto viestin koko reitin kohde. Tämä arvo ei tarkoita, että viestin käsitteli.<br/><br/>
Pyyntö vastauksessa tilanteessa lisätiedot suurenee, kun reitin kohde lähettää vastaanotto-kuittaus takaisin putkijohto.</td>
</tr>
<tr>
<td><strong>Vastaanotettujen viestien</strong></td>
<td>Näyttää vastaanotettujen BizTalk-palvelun kautta kaikki siltoja ajanjakson viestien määrä. Tämä arvo suurenee, kun uusi viesti saapuu putkijohto.</td>
</tr>
<tr>
<td><strong>Viestien prosessi</strong></td>
<td>Näyttää viestit, jotka on tällä hetkellä käsittelemien BizTalk-palvelun ajanjakson kokonaismäärän.</td>
</tr>
<tr>
<td><strong>Viestien käsitteleminen</strong></td>
<td>Näyttää viestit käsitteli BizTalk-palvelun kautta kaikki siltoja ajanjakson kokonaismäärän. Tämä arvo suurenee, kun viesti on vastaanottanut onnistuneesti putkijohto ja reititetty kohteeseen.</td>
</tr>
</table>


## <a name="scale"></a>Asteikko
Mittakaava-välilehti voit lisätä tai poistaa BizTalk-palvelun käyttämä yksiköiden määrän. Oletusarvon mukaan sellainen on määritetty yksikkö. Lisää voidaan lisätä skaalata BizTalk-palvelussa. Kun lisäät mittakaava, ovat lisääntyvien siirtonopeuden. Varat myös kasvaa, mukaan lukien käyttöön siltoja, sopimusten LOB yhteydet ja suoritin. Esimerkiksi lisäät skaalauksen 1 yksiköstä 2 yksikköä. Tässä tilanteessa käyttöönotto kaksinkertainen siltoja määrä, kaksoisnapsauttamalla sopimusten, kaksoisnapsauttamalla LOB-yhteydet ja kaksoisnapsauttamalla käsittely power.

Jotkin BizTalk-versiot eivät tarjoa akseli-vaihtoehto. Tässä tilanteessa yhden yksikön on sallittua. Voit selvittää, kuinka monta yksikköä mukana voi skaalata, viitata [BizTalk Services: versiot kaavion](biztalk-editions-feature-chart.md).

Lisääntyvien yksiköiden määrän, voi olla vaikutusta, hinnoittelua. Jos kasvatat yksiköt, valitsemalla **Tallenna** näyttää sanoman, joka ilmoittaa, jos laskutus vaikuttaa. Valitse sitten Jatka. Kun lisäät BizTalk palvelun tilan muutokset aktiivisen päivittäminen yksiköiden määrän. Päivittäminen-tilaan BizTalk-palvelun säilyy Suorita.

[BizTalk Services: versiot kaavion](biztalk-editions-feature-chart.md) määrittää "Yksikön".


## <a name="configure"></a>Asetusten määrittäminen
Hybrid yhteydet ei koske.

Asettaa varmuuskopioinnin tilaksi ei mitään tai automaattinen. Jos määritetty ei mitään, varmuuskopioita luodaan automaattisesti. Jos asetettu automaattisesti, voit määrittää varmuuskopioiden sijaintia, korkojakso varmuuskopion, ja kuinka kauan pitämään varmuuskopiot. 

[BizTalk Services: varmuuskopioiminen ja palauttaminen](biztalk-backup-restore.md) tiedot. 


## <a name="HybridConnections"></a>Hybrid yhteydet
Hybrid yhteydet muodostaa Azure ohjelma, esimerkiksi verkkosovelluksissa tai Mobile-sovellusten Azure App palvelun paikallisen-resurssi, joka käyttää staattinen porttinumeroa, kuten SQL Server, MySQL, HTTP-verkko-ohjelmointirajapinnan ja useimmat mukautetun verkkopalvelut. Hybrid yhteydet hallitaan BizTalk Services Azure perinteinen-portaalissa.

Azure App palvelun Hybrid yhteyksien luomisesta on artikkelissa [Access paikallisen resurssien hybrid yhteyksien Azure sovelluksen-palvelun avulla](../app-service-web/web-sites-hybrid-connection-get-started.md).

Voit luoda tai hallita Hybrid yhteyksiä Azure BizTalk Services-palveluissa, katso [Hybrid yhteydet](integration-hybrid-connection-overview.md).



## <a name="next"></a>Seuraava
Nyt kun olet tutustunut välilehtien, voit lukea lisää Azure BizTalk Services ominaisuuksista:

- [BizTalk Services: rajoittaminen](biztalk-throttling-thresholds.md)  
- [BizTalk Services: Myöntäjän nimi ja myöntäjä avain](biztalk-issuer-name-issuer-key.md)  
- [BizTalk Services: Varmuuskopiointi ja palauttaminen](biztalk-backup-restore.md)

## <a name="see-also"></a>Katso myös
- [Hybrid yhteydet](integration-hybrid-connection-overview.md)  
- [BizTalk Services: Kehittäjä, Basic, Vakio ja Premium versiot kaavio](biztalk-editions-feature-chart.md)  
- [BizTalk Services: Valmistelu käyttämällä Azure perinteinen portal](biztalk-provision-services.md)  
- [BizTalk Services: BizTalk palvelun tilan kaavio](biztalk-service-state-chart.md)  
- [Miten voin aloittaa käyttämällä Azure BizTalk Services SDK-paketissa](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[QuickStart]: ./media/biztalk-dashboard-monitor-scale-tabs/QuickStartIcon.png
[AddMetrics]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_AddMetrics.png
[GrayedMetric]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_GrayedMetric.png
[EnabledMetric]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_EnabledMetric.png
 

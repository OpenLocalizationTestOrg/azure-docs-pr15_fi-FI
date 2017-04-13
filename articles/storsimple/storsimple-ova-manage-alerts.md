<properties 
   pageTitle="Tarkastella ja hallita StorSimple Virtual matriisin ilmoitusten | Microsoft Azure"
   description="Tässä artikkelissa kuvataan StorSimple Virtual matriisin ilmoitusten ehdot ja vakavuus ja käyttämään StorSimple hallintapalvelu ilmoitusten hallinta."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="06/07/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-view-and-manage-alerts-for-the-storsimple-virtual-array"></a>StorSimple hallinta-palvelun avulla voit tarkastella ja hallita ilmoituksia StorSimple Virtual taulukolle

## <a name="overview"></a>Yleiskatsaus

StorSimple hallintapalvelu **ilmoitukset** -välilehti avulla voit tarkistaa ja poista StorSimple Virtual matriisin – liittyvät ilmoitukset reaaliaikainen välein. Tässä välilehdessä voit valvoa keskitetysti että StorSimple Virtual matriisien (tunnetaan myös nimellä StorSimple paikallisen virtual laitteet) ja yleisen Microsoft Azure StorSimple ongelmia.

Tässä opetusohjelmassa kuvataan, miten voit määrittää ilmoitukset, Yleiset ilmoitusten ehtoja, ilmoitusten vakavuustasot ja tarkastelemisesta ja seurata ilmoitukset. Lisäksi se sisältää ilmoitusten pikaopas taulukoita, joiden avulla voit nopeasti etsiä tietyn ilmoituksen ja vastata.

![Ilmoitukset-sivu](./media/storsimple-ova-manage-alerts/alerts1.png)

## <a name="configure-alert-settings"></a>Ilmoitusasetusten määrittäminen

Voit valita, haluatko ilmoituksen sähköpostitse ilmoituksen ehdoista kunkin StorSimple virtual laitteilla. Lisäksi voit määrittää ilmoitusten ilmoituksen muille vastaanottajille kirjoittamalla heidän sähköpostiosoitteensa puolipisteillä erotettuina **sähköpostin muille vastaanottajille** -ruutuun.

>[AZURE.NOTE] Voit kirjoittaa enintään 20 sähköpostiosoitteet virtual laite.

Kun otat virtual laitteen sähköposti-ilmoituksen, ilmoitusluetteloon jäsenet saavat sähköpostiviestin aina, kun tärkeä ilmoitus tapahtuu. Viestit lähetetään *storsimple-alerts-noreply@mail.windowsazure.com* ja kuvataan ilmoitusten ehto. Vastaanottajat valitsemalla **Peruuta tilaus** , voit poistaa itse sähköposti-ilmoituksen luettelo.

#### <a name="to-enable-email-notification-of-alerts-for-a-virtual-device"></a>Jos haluat ottaa käyttöön sähköposti-ilmoituksen virtual laitteen ilmoitukset

1. Valitse **laitteet** > virtual laitteen**määritykset** . Siirry kohtaan **ilmoitusten oletusasetukset** .

    ![ilmoitusten oletusasetukset](./media/storsimple-ova-manage-alerts/alerts2.png)

2. **Ilmoitusasetusten**määrittäminen seuraavasti:

    1. **Lähetä sähköposti-ilmoituksen** -kentässä Valitse **Kyllä**.

    2. **Sähköpostin palvelun järjestelmänvalvojat** -kenttään Valitse **Kyllä** , jos haluat palvelun järjestelmänvalvoja ja kaikki apuyhteyshenkilöiden ilmoitusten ilmoituksia.

    3. Kirjoita **sähköpostin muille vastaanottajille** -kenttään sähköpostiosoite, kaikille muille vastaanottajille, jotka saavat ilmoituksen ilmoitukset. Kirjoita nimet muodossa *someone@somewhere.com*. Erota sähköpostiosoitteet puolipisteellä avulla. Voit määrittää enintään 20 sähköpostiosoitteet virtual laite. 

        ![ilmoitusten ilmoituksen määrittäminen](./media/storsimple-ova-manage-alerts/alerts3.png)

3. Sivun alareunassa valitsemalla Tallenna kokoonpanosi **Tallenna** .

4. Lähetä testi sähköposti-ilmoituksen, valitsemalla **Testaa sähköpostiviestin lähettäminen**vieressä olevaa nuolikuvaketta. StorSimple hallintapalvelu näkyy tilasanomat, kuin se välittää testi-ilmoitus. 

5. Kun näyttöön tulee seuraava sanoma, valitse **OK**. 

    ![Ilmoitusten testata lähetetty sähköposti-ilmoitus](./media/storsimple-ova-manage-alerts/alerts4.png)

    >[AZURE.NOTE] Jos testi ilmoitusviesti ei voi lähettää, StorSimple hallintapalvelu näyttää sanoman. Valitse **OK**, odota muutama minuutti ja yritä sitten Lähetä viesti testi ilmoituksen uudelleen. 

    Testaa Ilmoitusviesti on seuraavankaltaiselta.

    ![Ilmoitusten testata sähköpostiesimerkki](./media/storsimple-ova-manage-alerts/alerts5.png)

## <a name="common-alert-conditions"></a>Ilmoitusten yleiset ehdot

StorSimple Virtual matriisin Luo ilmoituksia vastauksena useita ehtoja. Seuraavassa on yleisimpien ilmoitusten ehdoista:

- **Yhteysongelmat** – äänimerkkien ilmetä, kun kyseessä on vaikeuksia tietojen siirtäminen. Viestintä ongelmat voivat ilmetä tietojen siirto aikana ja Azure tallennustilan-tililtä tai vuoksi puuttuminen virtual laitteet ja StorSimple hallintapalvelu väliset yhteydet. Tietoliikenne on kuvattu joitakin vaikein koska tilikaudessa on niin paljon kohtiin virheen korjaamiseksi. Aina ensin Tarkista, että verkkoyhteys ja Internet-yhteys on ovat käytettävissä ennen jatkamista voin monimutkaisemman vianmääritys. Lisätietoja portit ja palomuuriasetukset Siirry [StorSimple Virtual matriisin järjestelmävaatimukset](storsimple-ova-system-requirements.md). Siirry vianmääritysohjeita, [ja Testaa yhteys cmdlet-komennon vianmääritys](storsimple-troubleshoot-deployment.md).

- **Suorituskykyongelmia** – äänimerkkien virheet, kun järjestelmä ei ole suorittamiseen optimaalisesti, esimerkiksi kun kyseessä kuormitettu.

Näyttöön saattaa tulla suojaus, päivitykset tai projektin epäonnistuu liittyvät ilmoitukset.

## <a name="alert-severity-levels"></a>Ilmoitusten vakavuustasot

Ilmoitukset on eri vakavuustasoja, ilmoitus tilanne sisältävät vaikutus ja ilmoituksen vastauksen tarpeen mukaan. Vakavuustasot ovat:

- **Kriittinen** – tämä ilmoitus on vastaus tilanteeseen, jossa on vaikuttavia järjestelmän onnistuneen suorituskykyä. Toimia tarvita varmistaa, että palvelu ei ole keskeyttää StorSimple.

- **Varoitus** – tämä ehto voi tulla kriittisiä, jos ei ratkea. Kannattaako tutkia tilanne ja ongelman poistamalla tarvitse ryhtyä.

- **Tietoja** – tämä ilmoitus sisältää tietoja, joita voi olla hyödyllistä seurata ja hallita järjestelmässä.

## <a name="view-and-track-alerts"></a>Ilmoitusten projektin aikataulun hahmottaminen

StorSimple hallinta palvelun Raporttinäkymät-ikkunan avulla voit silmäyksellä ilmoitukset virtual laitteissa, vakavuus tason mukaan numeroon.

![Ilmoitusten Raporttinäkymät-ikkunan](./media/storsimple-ova-manage-alerts/alerts6.png)

Vakavuus-tason valitseminen avaa **ilmoitukset** -välilehti. Tulokset ovat vain ilmoituksia, jotka vastaavat tasolla oleviin vakavuus.

![Ilmoitusten raportin suodatetut laji](./media/storsimple-ova-manage-alerts/alerts7.png)

Valitsemalla ilmoituksen luettelon avulla voit lisätiedot ilmoituksen, mukaan lukien ilmoituksen ilmoitettiin, viimeksi laitteen ja suositellut toimet voit ratkaista ilmoituksen koskeva ilmoitus esiintymien lukumäärän.

Voit kopioida hälytyksen tiedot tekstitiedostoon, jos haluat lähettää tiedot Microsoft Support. Olet Seuratut suositus ja poistunut ilmoitusten ehto paikallisen, Poista-ilmoitus valitsemalla ilmoituksen **ilmoitukset** -välilehti ja valitsemalla **Poista**. Jos haluat poistaa useita ilmoituksia, valitse kunkin ilmoitus, paitsi **ilmoitus** -sarakkeen minkä tahansa sarakkeen napsauttamalla ja valitse sitten **Poista** , kun olet valinnut kaikki ilmoitukset, jos haluat tyhjentää. Huomaa, että jotkin ilmoitukset poistetaan automaattisesti, kun ongelma on ratkaistu tai kun järjestelmä päivittää ilmoituksen uusilla tiedoilla.

Kun valitset **Tyhjennä**, on mahdollisuus kommentteja ilmoituksen ja ohjeet, joita noudatit voit ratkaista ongelman. 

![ilmoitusten kommentit](./media/storsimple-ova-manage-alerts/clear-alert.png)

Valitse Tarkista-kuvake ![tarkistuksen kuvake](./media/storsimple-ova-manage-alerts/check-icon.png) Jos haluat tallentaa kommentit.

Jotkin tapahtumat poistetaan järjestelmä, jos toinen tapahtuma käynnistyy uusilla tiedoilla. Tällöin näet seuraavan sanoman.

![Poista ilmoitussanoma](./media/storsimple-ova-manage-alerts/alerts8.png)

## <a name="sort-and-review-alerts"></a>Lajittele ja tarkista ilmoitukset

**Ilmoitukset** -välilehti näkyy jopa 250 ilmoitukset. Jos olet ylittänyt ilmoitukset määrä, kaikki ilmoitukset näkyvät oletusnäkymässä. Voit yhdistää mukauttamiseen seuraavat viestit näkyvät seuraavat kentät:

- **Tila** – voit näyttää **aktiivinen** tai **valittu** ilmoitukset. Aktiivinen hälytyksiä edelleen käytössä annetaan järjestelmään samalla, kun valittuna ilmoitukset on joko poistettu manuaalisesti järjestelmänvalvojan oikeuksia tai ohjelmallisesti tyhjentää, koska järjestelmä päivitetty ilmoitusten ehto uusilla tiedoilla.

- **Vakavuus** – voit näyttää kaikki vakavuustasot (kriittisen, Varoitus-tiedot) tai vain tiettyjä vakavuus, kuten ainoastaan kriittisen ilmoitukset ilmoitukset.

- **Tietolähteen** – voit saada ilmoituksia kaikista lähteistä tai rajoittaa niille, jotka ovat peräisin palvelu tai yhden tai kaikkien virtual laitteiden ilmoituksia.

- **Aikavälin** – määrittämällä **mistä** ja **mihin** päivämäärät ja aikaleimat, voit tarkastella ilmoituksia, jotka ovat kiinnostuneita ajanjakson aikana.

## <a name="alerts-quick-reference"></a>Ilmoitukset-pikaopas

Seuraavissa taulukoissa luetellaan joitakin Microsoft Azure StorSimple ilmoituksia, joita voi esiintyä, sekä muut tiedot ja suositukset jos niitä on saatavilla. StorSimple virtual laitteen ilmoitusten kuuluvat johonkin seuraavista ryhmistä:

- [Cloud connectivity ilmoitukset](#cloud-connectivity-alerts)

- [Ilmoitusten määrittäminen](#configuration-alerts)

- [Työn virhe ilmoitukset](#job-failure-alerts)

- [Suorituskyvyn ilmoitukset](#performance-alerts)

- [Suojausvaroitusten](#security-alerts)

- [Päivitysilmoitukset](#update-alerts)

### <a name="cloud-connectivity-alerts"></a>Cloud connectivity ilmoitukset

|Ilmoitusten teksti|Tapahtuman|Lisätietoja / suositellut toiminnot|
|:---|:---|:---|
|Laitteen *<device name>* ei ole yhdistetty pilveen.|Nimetty laite ei voi muodostaa pilveen. |Pilveen ei voitu muodostaa yhteyttä. Tämä voi johtua seuraavista:<ul><li>Voi olla laitteen verkkoasetukset ongelma.</li><li>Voi olla ongelmia tallennustilan tilin tunnistetiedot.</li></ul>Lisätietoja yhteysongelmien vianmääritys Siirry laitteen [paikallisen sivuston Käyttöliittymä](storsimple-ova-web-ui-admin.md) .|


### <a name="configuration-alerts"></a>Ilmoitusten määrittäminen

|Ilmoitusten teksti|Tapahtuman|Lisätietoja / suositellut toiminnot|
|:---|:---|:---|
|Paikallisen virtual laitteen kokoonpanotietoja ei tueta.|Hidasta.|Nykyiset määritykset voi johtaa kansiohierarkiassa. Varmista, että palvelin vastaa pienin määritys. Saat lisätietoja Siirry [StorSimple Virtual matriisin vaatimuksia](storsimple-ova-system-requirements.md). 
|Käyttöjärjestelmäsi valmistellun levytila <*laitteen nimi*>.|Levyn tilaa varoitus.|Valmistellun levytila on vähän. Voit vapauttaa levytilaa tarvittaessa työmääriä siirtäminen toiseen äänenvoimakkuutta tai Jaa tai poistamasta tietoja.

### <a name="job-failure-alerts"></a>Työn virhe ilmoitukset

|Ilmoitusten teksti|Tapahtuman|Lisätietoja / suositellut toiminnot|
|:---|:---|:---|
|Varmuuskopion <*laitteen nimi*> ei voi suorittaa loppuun.|Varmuuskopiointi-virhe.|Ei voi luoda varmuuskopion. Harkitse jompikumpi seuraavista:<ul><li>Yhteysongelmat saattavat estää varmuuskopiointia suorittamasta onnistuneesti. Varmista, ettei tietoalueessa ei yhteysongelmat. Lisätietoja yhteysongelmien vianmääritys Siirry [paikallisen sivuston Käyttöliittymä](storsimple-ova-web-ui-admin.md) virtual laitteeseesi.</li><li>Käytettävissä olevan tallennustilan enimmäismäärä on saavutettu. Jos haluat vapauttaa tilaa, harkitse varmuuskopioita, joita et enää tarvitse poistamista.</li></ul> Ongelmat, poista ilmoituksen ja yritä uudelleen.|
|Palauta <*laitteen nimi*> ei voi suorittaa loppuun.|Palauttaa työn virhe.|Varmuuskopiosta ei voi palauttaa. Harkitse jompikumpi seuraavista:<ul><li>Varmuuskopion luettelon ei ehkä ole kelvollinen. Päivitä vahvistamiseksi on edelleen voimassa.</li><li>Yhteysongelmat saattavat estää palautustoiminto suorittamasta onnistuneesti. Varmista, ettei tietoalueessa ei yhteysongelmat.</li><li>Käytettävissä olevan tallennustilan enimmäismäärä on saavutettu. Jos haluat vapauttaa tilaa, harkitse varmuuskopioita, joita et enää tarvitse poistamista.</li></ul>Ongelmat, poista ilmoituksen ja yritä palautustoiminto.|
|<*Laitteen nimi*> Kloonaa ei voi suorittaa loppuun.|Kloonaa työn virhe.|Kloonaa ei voi luoda. Harkitse jompikumpi seuraavista:<ul><li>Varmuuskopion luettelon ei ehkä ole kelvollinen. Päivitä vahvistamiseksi on edelleen voimassa.</li><li>Yhteysongelmat saattavat estää Kloonaa toiminnon suorituksen onnistuneesti. Varmista, ettei tietoalueessa ei yhteysongelmat.</li><li>Käytettävissä olevan tallennustilan enimmäismäärä on saavutettu. Jos haluat vapauttaa tilaa, harkitse varmuuskopioita, joita et enää tarvitse poistamista.</li></ul>Ongelmat, poista ilmoituksen ja yritä uudelleen.|

### <a name="performance-alerts"></a>Suorituskyvyn ilmoitukset

|Ilmoitusten teksti|Tapahtuman|Lisätietoja / suositellut toiminnot|
|:---|:---|:---|
|On ilmennyt odottamaton viipeitä tiedonsiirto.|Tiedonsiirto on hidasta.|Rajoittava virheitä ilmetä, kun ylität tallennustilan palvelun skaalattavuus kohteet. Tallennustilan palvelun toimii Näin voit varmistaa, että ei ole yhteen asiakkaaseen tai vuokraajan voi käyttää palvelua muiden kustannuksella. Lisätietoja vianmäärityksestä Azure-tallennustilan tilin Siirry [näytön vianmäärityksen, ja vianmääritys Microsoft Azuren tallennustilaan](storage-monitoring-diagnosing-troubleshooting.md).
|Paikallinen varaus levytilaa <*laitteen nimi*> on vähän.|Hidas vastausajan.|valmistellun kokonaiskoko <*laitteen nimi*> 10 prosenttia on varattu paikallisen laitteen ja nyt käytössäsi on pieni varattu-tila. <*Laitteen nimi*> työmäärää aiheuttamaa churn määrää tai ehkä olet viimeksi siirtänyt suuria määriä tietoa. Tämä saattaa johtaa suorituskyvyn. Harkitse jompikumpi seuraavista toimista voit ratkaista ongelman:<ul><li>Tätä laitetta cloud-kaistanleveyden lisääminen.</li><li>Pienennä tai Siirrä työmääriä toiseen äänenvoimakkuutta tai Jaa.</li></ul>

### <a name="security-alerts"></a>Suojausvaroitusten

|Ilmoitusten teksti|Tapahtuman|Lisätietoja / suositellut toiminnot|
|:---|:---|:---|
|Salasana <*laitteen nimi*> vanhenee <*luku*> päivää.|Varoitus salasana.| Salasanasi vanhenee < numero < päivää. Harkitse salasanan vaihtaminen. Jos haluat lisätietoja, siirry [StorSimple Virtual matriisin laitteen järjestelmänvalvojan salasanan vaihtaminen](storsimple-ova-change-device-admin-password.md).

### <a name="update-alerts"></a>Päivitysilmoitukset

|Ilmoitusten teksti|Tapahtuman|Lisätietoja / suositellut toiminnot|
|:---|:---|:---|
|Uudet päivitykset ovat saatavana laitteeseesi.|Saatavilla olevat päivitykset StorSimple Virtual matriisia.|Voit asentaa uusia päivityksiä **ylläpito** -sivulta.|
|Tarkista uudet päivitykset <*laitteen nimi*> ei onnistunut.|Päivittäminen epäonnistui. |Virhe asennettaessa uusia päivityksiä. Voit asentaa päivitykset manuaalisesti. Jos ongelma jatkuu, ota yhteyttä [Microsoftin tuotetukeen](storsimple-contact-microsoft-support.md).|


## <a name="next-steps"></a>Seuraavat vaiheet

- [Lisätietoja StorSimple Virtual matriisi](storsimple-ova-overview.md).



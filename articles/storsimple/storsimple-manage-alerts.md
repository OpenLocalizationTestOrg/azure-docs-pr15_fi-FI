<properties 
   pageTitle="Tarkastella ja hallita StorSimple ilmoitusten | Microsoft Azure"
   description="Tässä artikkelissa kuvataan StorSimple ilmoitusten ehdot ja vakavuus, ilmoitusten määrittämisestä ja käyttämisestä StorSimple hallintapalvelu ilmoitusten hallintaoikeudet."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/18/2016"
   ms.author="anbacker" />

# <a name="use-the-storsimple-manager-service-to-view-and-manage-storsimple-alerts"></a>StorSimple hallinta-palvelun avulla voit tarkastella ja hallita StorSimple ilmoitukset

## <a name="overview"></a>Yleiskatsaus

StorSimple hallintapalvelu **ilmoitukset** -välilehti avulla voit tarkistaa ja poista StorSimple laitteen – liittyvät ilmoitukset reaaliaikainen välein. Tässä välilehdessä voit valvoa keskitetysti StorSimple laitteet ja yleisen Microsoft Azure StorSimple-ratkaisu ongelmia.

Tässä opetusohjelmassa kuvataan yleisiä ilmoitusten ehdot, ilmoitusten vakavuustasot ja ilmoitusten määrittämisestä. Lisäksi se sisältää ilmoitusten pikaopas taulukoita, joiden avulla voit nopeasti etsiä tietyn ilmoituksen ja vastata.

![Ilmoitukset-sivu](./media/storsimple-manage-alerts/HCS_AlertsPage.png)

## <a name="common-alert-conditions"></a>Ilmoitusten yleiset ehdot

StorSimple laitteen Luo ilmoituksia vastauksena useita ehtoja. Seuraavassa on yleisimpien ilmoitusten ehdoista:

- **Laitteisto-ongelmat** – äänimerkkien kertoa laitteistosta kuntoa. Ne ilmoittamaan, jos ohjelmiston päivitykset tarvitaan, jos verkko-liittymällä on ongelmia tai jokin tietojen asemat ongelma.

- **Yhteysongelmat** – äänimerkkien ilmetä, kun kyseessä on vaikeuksia tietojen siirtäminen. Viestintä ongelmat voivat ilmetä tietojen siirto aikana ja Azure tallennustilan-tililtä tai vuoksi puuttuminen laitteet ja StorSimple hallintapalvelu välinen yhteys. Tietoliikenne on kuvattu joitakin vaikein koska tilikaudessa on niin paljon kohtiin virheen korjaamiseksi. Aina ensin Tarkista, että verkkoyhteys ja Internet-yhteys on ovat käytettävissä ennen jatkamista voin monimutkaisemman vianmääritys. Siirry vianmääritysohjeita, [ja Testaa yhteys cmdlet-komennon vianmääritys](storsimple-troubleshoot-deployment.md).

- **Suorituskykyongelmia** – äänimerkkien virheet, kun järjestelmä ei ole suorittamiseen optimaalisesti, esimerkiksi kun kyseessä kuormitettu.

Näyttöön saattaa tulla suojaus, päivitykset tai projektin epäonnistuu liittyvät ilmoitukset.

## <a name="alert-severity-levels"></a>Ilmoitusten vakavuustasot

Ilmoitukset on eri vakavuustasoja, vaikutus ilmoitusten tilanne sisältävät ja ilmoituksen vastauksen tarpeen mukaan. Vakavuustasot ovat:

- **Kriittinen** – tämä ilmoitus on vastaus tilanteeseen, jossa on vaikuttavia järjestelmän onnistuneen suorituskykyä. Toimia tarvita varmistaa, että palvelu ei ole keskeyttää StorSimple.

- **Varoitus** – tämä ehto voi tulla kriittisiä, jos ei ratkea. Kannattaako tutkia tilanne ja ongelman poistamalla tarvitse ryhtyä.

- **Tietoja** – tämä ilmoitus sisältää tietoja, joita voi olla hyödyllistä seurata ja hallita järjestelmässä.

## <a name="configure-alert-settings"></a>Ilmoitusasetusten määrittäminen

Voit valita, haluatko ilmoituksen sähköpostitse ilmoituksen ehdoista kunkin StorSimple laitteilla. Lisäksi voit määrittää ilmoitusten ilmoituksen muille vastaanottajille kirjoittamalla heidän sähköpostiosoitteensa puolipisteillä erotettuina **sähköpostin muille vastaanottajille** -ruutuun.

>[AZURE.NOTE] Voit kirjoittaa enintään 20 sähköpostiosoitteet laite.

Kun otat laitteen sähköposti-ilmoituksen, ilmoitusluetteloon jäsenet saavat sähköpostiviestin aina, kun tärkeä ilmoitus tapahtuu. Viestit lähetetään *storsimple-alerts-noreply@mail.windowsazure.com* ja kuvataan ilmoitusten ehtoon. Vastaanottajat valitsemalla **Peruuta tilaus** , voit poistaa itse sähköposti-ilmoituksen luettelo.

#### <a name="to-enable-email-notification-of-alerts-for-a-device"></a>Jos haluat ottaa käyttöön sähköposti-ilmoituksen laitteen ilmoitukset

1. Valitse **laitteet** > laitteen**määrittäminen** .

2. **Ilmoitusasetusten**määrittäminen seuraavasti:

    1. **Lähetä sähköposti-ilmoituksen** -kentässä Valitse **Kyllä**.

    2. **Sähköpostin palvelun järjestelmänvalvojat** -kenttään Valitse **Kyllä** , jos haluat palvelun järjestelmänvalvoja ja kaikki apuyhteyshenkilöiden ilmoitusten ilmoituksia.

    3. Kirjoita **sähköpostin muille vastaanottajille** -kenttään sähköpostiosoite, kaikille muille vastaanottajille, jotka saavat ilmoituksen ilmoitukset. Kirjoita nimet muodossa *someone@somewhere.com*. Erota sähköpostiosoitteet puolipisteellä avulla. Voit määrittää enintään 20 sähköpostiosoitteet laite. 

        ![Ilmoitusten ilmoituksen määrittäminen](./media/storsimple-manage-alerts/AlertNotify.png)

3. Lähetä testi sähköposti-ilmoituksen, valitsemalla **Testaa sähköpostiviestin lähettäminen**vieressä olevaa nuolikuvaketta. StorSimple hallintapalvelu näkyy tilasanomat, kuin se välittää testi-ilmoitus. 

4. Kun näyttöön tulee seuraava sanoma, valitse **OK**. 

    ![Ilmoitusten testata lähetetty sähköposti-ilmoitus](./media/storsimple-manage-alerts/HCS_AlertNotificationConfig3.png)

    >[AZURE.NOTE] Jos testi ilmoitusviesti ei voi lähettää, StorSimple hallintapalvelu näyttää sanoman. Valitse **OK**, odota muutama minuutti ja yritä sitten Lähetä viesti testi ilmoituksen uudelleen. 

## <a name="view-and-track-alerts"></a>Tarkastele ja seurata ilmoitukset

StorSimple hallinta palvelun Raporttinäkymät-ikkunan avulla voit silmäyksellä ilmoitukset laitteisiisi vakavuus tason mukaan numeroon.

![Ilmoitusten Raporttinäkymät-ikkunan](./media/storsimple-manage-alerts/admin_alerts_dashboard.png)

Vakavuus-tason valitseminen avaa **ilmoitukset** -välilehti. Tulokset ovat vain ilmoituksia, jotka vastaavat tasolla oleviin vakavuus.

![Ilmoitusten raportin suodatetut laji](./media/storsimple-manage-alerts/admin_alerts_scoped.png)

Valitsemalla ilmoituksen luettelon avulla voit lisätiedot ilmoituksen, mukaan lukien ilmoituksen ilmoitettiin, viimeksi laitteen ja suositellut toimet voit ratkaista ilmoituksen koskeva ilmoitus esiintymien lukumäärän. Jos kyseessä on laitteisto-ilmoitus, se tunnistaa laitteisto-osan.

![Laitteiston Esimerkki työpöytäilmoituksesta](./media/storsimple-manage-alerts/admin_alerts_hardware.png)

Voit kopioida hälytyksen tiedot tekstitiedostoon, jos haluat lähettää tiedot Microsoft Support. Olet Seuratut suositus ja poistunut ilmoitusten ehto paikallisen, poista laitteesta ilmoitus valitsemalla ilmoituksen **ilmoitukset** -välilehti ja valitsemalla **Poista**. Jos haluat poistaa useita ilmoituksia, valitse kunkin ilmoitus, paitsi **ilmoitus** -sarakkeen minkä tahansa sarakkeen napsauttamalla ja valitse sitten **Poista** , kun olet valinnut kaikki ilmoitukset, jos haluat tyhjentää. Huomaa, että jotkin ilmoitukset poistetaan automaattisesti, kun ongelma on ratkaistu tai kun järjestelmä päivittää ilmoituksen uusilla tiedoilla.

Kun valitset **Tyhjennä**, on mahdollisuus kommentteja ilmoituksen ja ohjeet, joita noudatit voit ratkaista ongelman. Jotkin tapahtumat poistetaan järjestelmä, jos toinen tapahtuma käynnistyy uusilla tiedoilla. Tällöin näet seuraavan sanoman.

![Poista ilmoitussanoma](./media/storsimple-manage-alerts/admin_alerts_system_clear.png)

## <a name="sort-and-review-alerts"></a>Lajittele ja tarkista ilmoitukset

Voi olla tehokkaampaa suorittaa raportteja ilmoitukset niin, että voit tarkastella ja poista ne ryhmiin. Lisäksi **ilmoitukset** -välilehti näkyy jopa 250 ilmoitukset. Jos olet ylittänyt ilmoitusten määrä, kaikki ilmoitukset näkyvät oletusnäkymässä. Voit yhdistää mukauttamiseen seuraavat viestit näkyvät seuraavat kentät:

- **Tila** – voit näyttää **aktiivinen** tai **valittu** ilmoitukset. Aktiivinen hälytyksiä edelleen käytössä annetaan järjestelmään samalla, kun valittuna ilmoitukset on joko poistettu manuaalisesti järjestelmänvalvojan oikeuksia tai ohjelmallisesti tyhjentää, koska järjestelmä päivitetty ilmoitusten ehto uusilla tiedoilla.

- **Vakavuus** – voit näyttää kaikki vakavuustasot (kriittisen, Varoitus-tiedot) tai vain tiettyjä vakavuus, kuten ainoastaan kriittisen ilmoitukset ilmoitukset.

- **Tietolähteen** – voit saada ilmoituksia kaikista lähteistä tai rajoittaa ilmoituksia niille, jotka sisältyvät palvelun tai yhdestä tai kaikki laitteet.

- **Aikavälin** – määrittämällä **mistä** ja **mihin** päivämäärät ja aikaleimat, voit tarkastella ilmoituksia, jotka ovat kiinnostuneita ajanjakson aikana.

## <a name="alerts-quick-reference"></a>Ilmoitukset-pikaopas

Seuraavissa taulukoissa luetellaan joitakin Microsoft Azure StorSimple ilmoituksia, joita voi esiintyä, sekä muut tiedot ja suositukset jos niitä on saatavilla. StorSimple laitteen ilmoitusten kuuluvat johonkin seuraavista ryhmistä:

- [Cloud connectivity ilmoitukset](#cloud-connectivity-alerts)

- [Klusterin ilmoitukset](#cluster-alerts)

- [Tietojen palauttaminen ilmoitukset](#disaster-recovery-alerts)

- [Laitteiston ilmoitukset](#hardware-alerts)

- [Työn virhe ilmoitukset](#job-failure-alerts)

- [Paikallisesti kiinnitetty aseman ilmoitukset](#locally-pinned-volume-alerts)

- [Verkko-ilmoitukset](#networking-alerts)

- [Suorituskyvyn ilmoitukset](#performance-alerts)

- [Suojausvaroitusten](#security-alerts)

- [Paketin hälytysten tuki](#support-package-alerts)

- [Päivitysilmoitukset](#update-alerts)

### <a name="cloud-connectivity-alerts"></a>Cloud connectivity ilmoitukset

|Ilmoitusten teksti|Tapahtuman|Lisätietoja / suositellut toiminnot|
|:---|:---|:---|
|Yhteys <*cloud credential nimi*> ei onnistu.|Tallennustilan tiliä ei voi muodostaa yhteyttä.|Vaikuttaa siltä, ettei laitteen connectivity ongelma voi ilmetä. Suorita `Test-HcsmConnection` cmdlet StorSimple Windows PowerShell-käyttöliittymästä laitteen tunnistaa ja korjaa ongelman. Jos asetukset ovat oikein, ongelma saattaa olla tallennustilan tilin, jonka ilmoituksen korotettuna tunnuksilla. Tässä tapauksessa `Test-HcsStorageAccountCredential` cmdlet-komento, voit selvittää, ovatko ongelmat, jotka voit ratkaista.<ul><li>Tarkista verkkoasetukset.</li><li>Tarkista tallennustilan tilin tunnistetiedot.</li></ul>|
|Olemme saaneet ei uudelleenaktivoinnin yhteydessä laitteestasi viimeisen <*luku*> minuuttia.|Laite ei voi muodostaa yhteyttä.|Vaikuttaa siltä, ettei yhteys ongelma laitteen kanssa. Käytä `Test-HcsmConnection` cmdlet StorSimple Windows PowerShell-käyttöliittymästä laitteen tunnistamiseen ja korjata ongelman tai ota yhteys järjestelmänvalvojaan.|

### <a name="storsimple-behavior-when-cloud-connectivity-fails"></a>Cloud yhteys epäonnistuu StorSimple toiminta

Mitä tapahtuu, jos cloud yhteys epäonnistuu käynnissä tuotannon StorSimple laitteeseeni?

Jos cloud yhteys epäonnistuu StorSimple tuotannon laitteen, valitse laitteessasi tilan mukaan seuraavat voi ilmetä: 

- **Paikallisten tietojen laitteen**: jonkin aikaa ei ole mitään häiriöitä ja lukee edelleen oltava served. Kuitenkin avoin IOs määrä kasvaa, ja se ylittää rajoitukset lukee voi käynnistää epäonnistuu. 

    Laitteen tietojen summan mukaan kirjoituksia edelleen myös esiintyvät ensimmäisen muutaman tunnin jälkeen häiriöitä cloud yhteydet. Merkinnät sitten hidastaa ja aloita myöhemmin epäonnistuu, jos cloud yhteydet keskeytyy useita tunteja. (Ei väliaikaisten tiedot, jotka ovat, miten pilveen laitteessa. Tällä alueella on tyhjennetty, kun tiedot lähetetään. Jos yhteys epäonnistuu, tietojen tallennustilan tällä alueella ei sijaita pilveen, ja IO epäonnistuu.)   

 
- **Tietojen pilvipalvelussa**: Useimmat cloud connectivity virheet, palautetaan virhe. Kun yhteys on palautettu, IOs jatketaan ilman, että käyttäjällä on tuotava aseman online-tilaan. Harvoin käyttäjältä voidaan tarvita takaisin näkyviin äänenvoimakkuuden verkossa perinteinen Azure-portaalista. 
 
- **Cloud tilannevedoksia käynnissä varten**: toiminnon yritetään muutaman kerran 4 – 5 tunnin kuluessa ja jos yhteydet ei palautettu, cloud tilannevedoksia epäonnistuu.


### <a name="cluster-alerts"></a>Klusterin ilmoitukset

|Ilmoitusten teksti|Tapahtuman|Lisätietoja / suositellut toiminnot|
|:---|:---|:---|
|Laitteen epäonnistui <*laitteen nimi*>.|Laite on ylläpito-tilassa.|Laitteen epäonnistui päälle vuoksi kirjoittaminen tai sulkeminen ylläpidon tila. Tämä on normaalia ja mitään toimia ei tarvita. Kun olet kuitata tämän ilmoituksen, Poista ilmoitukset sivun.|
|Laitteen epäonnistui <*laitteen nimi*>.|Laitteen laitteisto tai ohjelmisto on juuri päivitetty.|Oli klusterin automaattisesti, päivitys vuoksi. Tämä on normaalia ja mitään toimia ei tarvita. Kun olet kuitata tämän ilmoituksen, Poista ilmoitukset sivun.|
|Laitteen epäonnistui <*laitteen nimi*>.|Ohjaimen Sammuta tai se uudelleen.|Laitteen epäonnistui päälle, koska aktiivinen ohjauskoneen on Sammuta tai käynnistetään uudelleen järjestelmänvalvojan oikeuksia. Mitään toimia ei tarvita. Kun olet kuitata tämän ilmoituksen, Poista ilmoitukset sivun.|
|Laitteen epäonnistui <*laitteen nimi*>.|Suunniteltu automaattisesti.|Varmista, että tämä on suunniteltu automaattisesti. Kun olet toiminnon, poista tämän ilmoituksen Ilmoitukset-sivun.|
|Laitteen epäonnistui <*laitteen nimi*>.|Suunnittelematon automaattisesti.|StorSimple valmista suunnittelematon failovers automaattisesti palauttaminen. Jos näet äänimerkkien suuri määrä, ota yhteyttä Microsoft Support.|
|Laitteen epäonnistui <*laitteen nimi*>.|Muut/Tuntematon syy.|Jos näet äänimerkkien suuri määrä, ota yhteyttä Microsoft Support. Kun ongelma on ratkennut, poista tämän ilmoituksen ilmoitukset-sivulta.|
|Kriittinen device Servicen raporttien tila, kun epäonnistui.|Tietopolkua palvelun virhe. |Pyydä apua Microsoft Support.|
|Virtuaalinen verkkoliittymän IP-osoite <*tietojen #*> Raportit tila, kuten epäonnistui. |Muut/Tuntematon syy. |Joskus tilapäinen ehdot voivat aiheuttaa äänimerkkien. Jos näin on, valitse tämä ilmoitus automaattisesti poistetaan hetken kuluttua. Jos ongelma jatkuu, ota yhteyttä Microsoft Support.|
|Virtuaalinen verkkoliittymän IP-osoite <*tietojen #*> Raportit tila, kuten epäonnistui.|Liittymän nimi: <*tietojen #*> IP-osoite <IP address> ei voi olla online-tilaan koska verkossa havaittiin kaksinkertainen IP-osoite. |Varmista, että kaksinkertainen IP-osoite on poistettu verkosta tai määritä eri IP-osoite-liittymällä.|


### <a name="disaster-recovery-alerts"></a>Tietojen palauttaminen ilmoitukset

|Ilmoitusten teksti|Tapahtuman|Lisätietoja / suositellut toiminnot|
|:---|:---|:---|
|Palautus toimintoja ei voi palauttaa kaikki tämän palvelun asetukset. Laitteen kokoonpanotietoja on joitakin laitteita epäyhtenäiseen tilaan.|Tietojen ristiriidan havaita palauttaminen jälkeen.|Salatut tiedot palvelu ei ole synkronoitu-laitteessa. Määritä laitteen <*laitteen nimi*> StorSimple Managerista Aloita synkronointi. StorSimple Windows PowerShell-käyttöliittymän avulla voit suorittaa `Restore-HcsmEncryptedServiceData` antamisen laitteen <*laitteen nimi*> cmdlet-komento, vanha salasana cmdlet suojausprofiilin palauttamaan syötteeksi. Suorita `Invoke-HcsmServiceDataEncryptionKeyChange` cmdlet-komento, jos haluat päivittää palvelun tiedot salausavaimen. Kun olet toiminnon, poista tämän ilmoituksen Ilmoitukset-sivun.|


### <a name="hardware-alerts"></a>Laitteiston ilmoitukset

|Ilmoitusten teksti|Tapahtuman|Lisätietoja / suositellut toiminnot|
|:---|:---|:---|
|Laitteisto-osan <*osan tunnus*> Raportit <*tila*> tila.||Joskus tilapäinen ehdot voivat aiheuttaa äänimerkkien. Jos näin on, tämä ilmoitus kohteet poistetaan automaattisesti jonkin ajan kuluttua. Jos ongelma jatkuu, ota yhteyttä Microsoft Support.|
|Passiivinen ohjauskoneen toimi oikein.|Passiivinen (toissijainen)-ohjain ei toimi.|Laite on toiminnassa, mutta jokin oman ohjaimet on virheellisesti. Yritä käynnistää kyseisen ohjauskoneen. Jos ongelma ei ratkennut, ota yhteyttä Microsoft Support.|

### <a name="job-failure-alerts"></a>Työn virhe ilmoitukset

|Ilmoitusten teksti|Tapahtuman|Lisätietoja / suositellut toiminnot|
|:---|:---|:---|
|Varmuuskopiointi <*lähde-asema Ryhmätunnus*> epäonnistui.|Varmuuskopiointityön epäonnistui.|Yhteysongelmat saattavat estää varmuuskopiointia suorittamasta onnistuneesti. Jos ei ole yhteysongelmat, olet ehkä saavuttanut varmuuskopioiden enimmäismäärä. Poista varmuuskopioita, joita et enää tarvitse ja yritä uudelleen. Kun olet toiminnon, poista tämän ilmoituksen Ilmoitukset-sivun.|
|<*Varmuuskopion lähdeobjekti tunnukset*> Kloonaa <*kohde aseman sarjanumerot*> epäonnistui.|Kloonaa työ epäonnistui.|Varmista, että varmuuskopiointi on edelleen voimassa Päivitä varmuuskopion. Jos varmuuskopioinnin ei kelpaa, on mahdollista cloud Yhteysongelmat estävät Kloonaa toiminnon suorituksen onnistuneesti. Jos ei ole yhteysongelmat, olet saattanut saavuttaa tallennustilarajan. Poista varmuuskopioita, joita et enää tarvitse ja yritä uudelleen. Kun olet ottanut toiminnon ratkaise ongelmaa, poista tämän ilmoituksen Ilmoitukset-sivun.|
|Palauta <*varmuuskopion lähdeobjekti tunnukset*> epäonnistui.|Palauttaa työn epäonnistui.|Varmista, että varmuuskopiointi on edelleen voimassa Päivitä varmuuskopion. Jos varmuuskopioinnin ei kelpaa, on mahdollista cloud Yhteysongelmat estävät palautustoiminto suorittamasta onnistuneesti. Jos ei ole yhteysongelmat, olet saattanut saavuttaa tallennustilarajan. Poista varmuuskopioita, joita et enää tarvitse ja yritä uudelleen. Kun olet ottanut toiminnon ratkaise ongelmaa, poista tämän ilmoituksen Ilmoitukset-sivun.|

### <a name="locally-pinned-volume-alerts"></a>Paikallisesti kiinnitetty aseman ilmoitukset

|Ilmoitusten teksti|Tapahtuman|Lisätietoja / suositellut toiminnot|
|:---|:---|:---|
|Paikallisen aseman <*levyn nimi*> luominen epäonnistui.| Työn aseman luominen epäonnistui. <*Virhe viestin vastaavat epäonnistui Virhekoodi*>.|Yhteysongelmat voivat estää toiminnon tilan luonti onnistui suorituksen. Paikallisesti kiinnitetty tietomääristä thickly valmisteltu ja luoda tilaa liittyy spilling Porrastettu tietomääristä pilveen. Jos ei ole yhteysongelmat, voit ehkä täyttynyt paikallisen tilaa laitteessa. Tarkistaa, jos tilaa ennen tätä toimintoa yritetään sisältyy laitteeseen.|
|Paikallisen aseman <*levyn nimi*> laajentaminen epäonnistui.|Äänenvoimakkuuden muokkaus-työ on epäonnistunut vuoksi <*Virhe viestin vastaavat epäonnistui Virhekoodi*>.|Yhteysongelmat saattavat estää aseman laajentamista toiminnon suorittamisen. Paikallisesti kiinnitetty tietomääristä thickly valmisteltu ja laajentaa aiemmin tilaa prosessi käsittää spilling Porrastettu tietomääristä pilveen. Jos ei ole yhteysongelmat, voit ehkä täyttynyt paikallisen tilaa laitteessa. Tarkistaa, jos tilaa ennen tätä toimintoa yritetään sisältyy laitteeseen.|
|Muunto aseman <*levyn nimi*> epäonnistui.|Muuntaa aseman tyypin paikallisesti aseman muuntaminen-työ kiinnitetty tasoisen epäonnistui.|Äänenvoimakkuuden muuntaminen paikallisesti kiinnitettyinä tyypistä tasoisen ei voi suorittaa loppuun. Varmista, ettei tietoalueessa ei yhteysongelmat estää toiminto onnistuu. Yhteysongelmien vianmääritys Siirry [kanssa testi HcsmConnection cmdlet-komennon vianmääritys](storsimple-troubleshoot-deployment.md#troubleshoot-with-the-test-hcsmconnection-cmdlet).<br>Alkuperäisen paikallisesti kiinnitetty äänenvoimakkuuden on nyt merkitty Porrastettu aseman jälkeen soluihin paikallisesti kiinnitetty asemasta on spilled pilveen muunnon aikana. Voimassa oleva Porrastettu äänenvoimakkuus on edelleen toimivien paikallisen laitteen, joka on palautettava tulevien asemat tilaa.<br>Minkä tahansa yhteysongelmien ratkaiseminen, poista ilmoituksen ja muuntaa aseman takaisin alkuperäiseen paikallisesti kiinnitetty asematyyppi, jotta kaikki tiedot on saatavana paikallisesti uudelleen.|
|Muunto aseman <*levyn nimi*> epäonnistui.|Muuntaa aseman tyypin aseman muuntaminen-työ tasoisen paikallisesti kiinnitetyt epäonnistui.|Äänenvoimakkuuden muuntaminen tyypistä tasoisen, paikallisesti kiinnitetyt ei onnistunut. Varmista, ettei tietoalueessa ei yhteysongelmat estää toiminto onnistuu. Yhteysongelmien vianmääritys Siirry [kanssa testi HcsmConnection cmdlet-komennon vianmääritys](storsimple-troubleshoot-deployment.md#troubleshoot-with-the-test-hcsmconnection-cmdlet).<br>Alkuperäisen Porrastettu äänenvoimakkuuden merkitty nyt paikallisesti kiinnitetty aseman kuin muuntaminen yhteydessä jatkuu asuvat pilvipalveluun, kun thickly valmistellun tilaa tämän aseman laitteessa voi enää palautettava tulevien paikallisen levyasemien tiedot.<br>Minkä tahansa yhteysongelmien ratkaiseminen, poista ilmoituksen ja muuntaa aseman takaisin alkuperäiseen Porrastettu asematyyppi, jotta paikallisen tilaa thickly valmisteltu laitteessa on palautettava.|
|Paikallinen tilaa kulutus paikallisen tilannevedosten lähestyvä <*aseman ryhmänimi*>|Paikallisen varmuuskopion käytännön tilannevedoksia saattaa toimia pian loppumassa ja mitätöidään host kirjoittaminen virheiden välttämiseksi.|Usein käytetyt paikallisen tilannevedoksia rinnalla ylimpien tietojen churn, varmuuskopioinnin käytännön tähän ryhmään liitettyä tietomääristä aiheuttavat paikallisen tilaa kulutettu nopeasti laitteessa. Poista paikallinen tilannevedoksia, joita et enää tarvitse. Päivitä myös paikallisen tilannevedoksen aikataulut varmuuskopion käytäntö vähemmän usein paikallisen tilannevedoksia ja varmistaa, että cloud tilannevedoksia otetaan säännöllisesti. Jos nämä toiminnot ei oteta, nämä tilannevedoksia paikallisen tilaa pian ehkä täyttynyt ja järjestelmä automaattisesti poistaa niitä voit varmistaa, että host kirjoituksia jatkaa käsitteleminen onnistunut.|
|Paikallinen tilannevedoksia <*aseman ryhmänimi*> on poistettu käytöstä.|Paikallinen tilannevedoksia <*aseman ryhmänimi*> on virheelliseksi ja sitten poistettu, koska ne on suurempi kuin paikallisen tilaa laitteessa.|Jotta tämä ei toistumaan tulevaisuudessa, tarkista tämän varmuuskopion käytännön paikallisen tilannevedoksen aikatauluja ja poista paikallinen tilannevedoksia, joita et enää tarvitse. Usein käytetyt paikallisen tilannevedoksia rinnalla ylimpien tietojen churn, varmuuskopioinnin käytännön tähän ryhmään liitettyä tietomääristä voi aiheuttaa paikallisen tilaa kulutettu nopeasti laitteessa.|
|Palauta <*varmuuskopion lähdeobjekti tunnukset*> epäonnistui.|Palautustyön epäonnistui.|Jos olet paikallisesti kiinnitetyt tai pitämällä järjestelmässä sekä paikallisesti kiinnitetty ja Porrastettu asemat tämän varmuuskopion käytännön Päivitä varmuuskopion luettelo ja varmista, että varmuuskopiointi on edelleen voimassa. Jos varmuuskopioinnin ei kelpaa, on mahdollista cloud Yhteysongelmat estävät palautustoiminto suorittamasta onnistuneesti. Paikallisesti kiinnitetty asemat, jotka palautettiin osana tilannevedos-ryhmä ei ole kaikkien niiden laitteen kopioitujen tietojen ja, jos sinulla on Porrastettu ja paikallisesti kiinnitetty erilaisten tilannevedos-ryhmässä, ne eivät ole synkronoituina keskenään. Suorittaminen palautustoiminto, ota asemat tässä ryhmässä offline-tilassa isännän ja yritä uudelleen palautustoiminto. Huomaa, että aseman tietoihin, jotka olivat suorittaa palautuksen yhteydessä muutokset menetetään.|

### <a name="networking-alerts"></a>Verkko-ilmoitukset

|Ilmoitusten teksti|Tapahtuman|Lisätietoja / suositellut toiminnot|
|:---|:---|:---|
|StorSimple palveluihin ei voitu käynnistää.|Tietopolkua virhe |Jos ongelma jatkuu, ota yhteyttä Microsoft Support.|
|Kaksoiskappaleiden havaittu "Data0" IP-osoite.| |Järjestelmä havaitsi ristiriidan IP-osoite "10.0.0.1". Laitteeseen verkkoresurssin 'Data0' *<device1>* on offline-tilassa. Varmistaa, että tämä IP-osoite käytetään ei muiden verkko-yritys. Siirry verkko-ongelmien vianmääritystä [ja sitten Get-NetAdapter cmdlet-komennon vianmääritys](storsimple-troubleshoot-deployment.md#troubleshoot-with-the-get-netadapter-cmdlet). Ongelman ratkaisemiseen apua verkonvalvojalta. Jos ongelma jatkuu, ota yhteyttä Microsoft Support. |
|'Data0' IPv4 (tai IPv6)-osoite on offline-tilassa.| |Verkkoresurssin 'Data0' IP-osoite "10.0.0.1." ja etuliitteen pituus '22' laitteeseen *<device1>* on offline-tilassa. Varmistaa, että valitsimen portit, johon on liitetty liittymän toiminnassa. Siirry verkko-ongelmien vianmääritystä [ja sitten Get-NetAdapter cmdlet-komennon vianmääritys](storsimple-troubleshoot-deployment.md#troubleshoot-with-the-get-netadapter-cmdlet). |
 

### <a name="performance-alerts"></a>Suorituskyvyn ilmoitukset

|Ilmoitusten teksti|Tapahtuman|Lisätietoja / suositellut toiminnot|
|:---|:---|:---|
|Laitteen kuormituksen on ylittänyt <*kynnysarvo*>.|Hitaammin Odotetaan vastausta kertaa.|Laitteen raportit i/kuormitettu käyttö. Tämä saattaa aiheuttaa laite ei aina toimi asianmukaisesti sen pitäisi. Tarkista työtaakkaa liittänyt laitteen ja määrittää, jos niitä on, joka voi siirtää toiseen laitteeseen tai joita ei enää tarvita.<br>Siirry selvittääksesi nykyisen tilan, [Käytä StorSimple hallintapalvelu seurannassa laitteesi](storsimple-monitor-device.md)|

### <a name="security-alerts"></a>Suojausvaroitusten

|Ilmoitusten teksti|Tapahtuman|Lisätietoja / suositellut toiminnot|
|:---|:---|:---|
|Microsoft Support istunnon on alkanut.|Kolmannen osapuolen käyttää tuki-istunnon.|Vahvista, että tämä käyttö on sallittua. Kun olet toiminnon, poista tämän ilmoituksen Ilmoitukset-sivun.|
|<*Aika*> päättyy <*elementti*> salasana.|Salasanan vanhentumisesta on lähellä.|Vaihda salasana, ennen kuin se päättyy.|
|Suojauksen kokoonpanotietoja ei löydy <*Elementtitunnus*>.||Tämä aseman säilö liittyvät asemat ei voi käyttää replikoiminen StorSimple-määritys. Voit varmistaa, että tiedot on tallennettu turvallisesti, on suositeltavaa aseman säilö ja kaikki asemat liittyvän aseman säilön poistaminen. Kun olet toiminnon, poista tämän ilmoituksen Ilmoitukset-sivun.|
|<*numero*> kirjautuminen yritykset epäonnistui <*Elementtitunnus*>.|Useita kirjautumisyrityksiksi.|Laitteen voi olla hyökkäyksen tai valtuutettu käyttäjä yrittää muodostaa väärällä salasanalla.<ul><li>Ota valtuutettujen käyttäjien ja varmista, että nämä yritykset on oikeutettu lähteestä. Jos näet edelleen paljon epäonnistui kirjautuminen yritykset, harkitse etähallinnan käytöstä ja otat verkonvalvoja. Kun olet toiminnon, poista tämän ilmoituksen Ilmoitukset-sivun.</li><li>Tarkista, että tilannevedoksen hallinta esiintymät on määritetty oikea salasana. Kun olet toiminnon, poista tämän ilmoituksen Ilmoitukset-sivun.</li></ul>Saat lisätietoja Siirry [vanhentuneet laitteen salasanan muuttaminen](storsimple-snapshot-manager-manage-devices.md#change-an-expired-device-password).|
|Yhden tai useamman virheet tapahtui palvelun tiedot salausavaimen muuttaminen.||Virheitä ilmeni palvelun tiedot salausavaimen muuttaminen. Kun on osoitettu virhe ehdot, suorita `Invoke-HcsmServiceDataEncryptionKeyChange` cmdlet StorSimple Windows PowerShell-käyttöliittymästä laitteessasi, jos haluat päivittää palvelun. Jos ongelma jatkuu, ota yhteyttä Microsoftin tuotetukeen. Kun olet ratkaissut ongelmaa, poista tämän ilmoituksen ilmoitukset-sivulta.|

### <a name="support-package-alerts"></a>Paketin hälytysten tuki

|Ilmoitusten teksti|Tapahtuman|Lisätietoja / suositellut toiminnot|
|:---|:---|:---|
|Tuki-paketin luominen epäonnistui.|StorSimple ei voitu muodostaa paketin.|Yritä uudelleen. Jos ongelma jatkuu, ota yhteyttä Microsoft Support. Kun olet ratkaissut ongelmaa, poista tämän ilmoituksen ilmoitukset-sivulta.|

### <a name="update-alerts"></a>Päivitysilmoitukset

|Ilmoitusten teksti|Tapahtuman|Lisätietoja / suositellut toiminnot|
|:---|:---|:---|
|Korjaustiedoston asennettuna.|Ohjelmisto päivitys on valmis.|Korjaus on asennettu laitteessasi.|
|Manuaalinen päivityksiä saatavilla.|Päivityksiä saatavilla-ilmoitus.|Käytetään Windows PowerShell-käyttöliittymän StorSimple, asenna nämä päivitykset laitteessasi. <br>Jos haluat lisätietoja, siirry [päivittää StorSimple 8000 sarjan laitteen](storsimple-update-device.md).|
|Uusia päivityksiä saatavilla.|Päivityksiä saatavilla-ilmoitus.|Voit asentaa päivitykset **ylläpito** -sivun tai käyttämällä Windows PowerShell-käyttöliittymän StorSimple laitteessasi. <br>Jos haluat lisätietoja, siirry [päivittää StorSimple 8000 sarjan laitteen](storsimple-update-device.md).|
|Asenna päivitykset epäonnistui.|Päivitysten asentaminen ei onnistunut.|Järjestelmä ei asenna päivitykset. Voit asentaa päivitykset **ylläpito** -sivun tai käyttämällä Windows PowerShell-käyttöliittymän StorSimple laitteessasi. Jos ongelma jatkuu, ota yhteyttä Microsoft Support. <br>Jos haluat lisätietoja, siirry [päivittää StorSimple 8000 sarjan laitteen](storsimple-update-device.md).|
|Uusien päivitysten tarkistaminen automaattisesti ei onnistu.|Automaattinen tarkistus epäonnistui.|Voit tarkistaa manuaalisesti uudet päivitykset **ylläpito** -sivulta.|
|Uusi WUA agentti käytettävissä.|Ilmoitus on saatavana päivitys.|Lataa uusimmat Windows Update-agentti ja asenna Windows PowerShell-käyttöliittymästä.|
|Laitteisto-osan <*osan tunnus*> vastaa laitteiston avulla.|Päivitystä laitteisto-ohjelmiston asennus ei onnistunut.|Ota yhteys Microsoftin tuotetukeen.|

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja [StorSimple virheet ja vianmääritys toiminnallisia laite](storsimple-troubleshoot-operational-device.md).


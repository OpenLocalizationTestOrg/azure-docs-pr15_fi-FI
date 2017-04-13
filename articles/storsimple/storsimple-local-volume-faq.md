<properties 
   pageTitle="Paikallisesti StorSimple kiinnitetyt tietomääristä usein kysytyt kysymykset | Microsoft Azure"
   description="On vastauksia usein kysyttyjä kysymyksiä StorSimple paikallisesti kiinnitetyt asemat."
   services="storsimple"
   documentationCenter="NA"
   authors="manuaery"
   manager="syadav"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/16/2016"
   ms.author="manuaery" />

# <a name="storsimple-locally-pinned-volumes-frequently-asked-questions-faq"></a>Paikallisesti StorSimple kiinnitetyt asemat: usein kysytyt kysymykset

## <a name="overview"></a>Yleiskatsaus

Seuraavassa on kysymyksiä ja vastauksia, joita saattaa olla Luo paikallisesti kiinnitetyt StorSimple asema, muuntaa Porrastettu aseman paikallisesti kiinnitetty merkkiin (ja päinvastoin) tai varmuuskopioiminen ja palauttaminen paikallisesti kiinnitetty äänenvoimakkuutta.

Kysymyksiä ja vastauksia on järjestetty seuraavaan luokkaan

- Paikallisesti kiinnitetty aseman luominen
- Paikallisesti kiinnitetty varmuuskopioiminen
- Porrastettu aseman muuntaminen paikallisesti kiinnitetty aseman
- Paikallisesti kiinnitetty aseman palauttaminen
- Epäonnistuneen paikallisesti kiinnitetty aseman päälle

## <a name="questions-about-creating-a-locally-pinned-volume"></a>Kysymyksiä paikallisesti kiinnitetty aseman luominen

**Q.** Mikä on paikallisesti kiinnitetty asema, joka voidaan luoda 8000 sarja-laitteissa enimmäiskokoa?

**A** voit valmistella paikallisesti kiinnitetty tietomääristä enintään 8,5 TT tai Porrastettu asemat enintään 200 TT 8100 laitteeseen. Suurempi 8600 laitteeseen, voit valmistella paikallisesti kiinnitetty tietomääristä enintään 22.5 TT tai Porrastettu asemat enintään 500 Teratavua.

**Q.** Voin viimeksi päivitetty 8100 laitteeseeni Update 2 ja luo paikallisesti kiinnitetty asema yritettäessä käytettävissä enimmäiskoko on vain 6 TT ja ei 8,5 Teratavua. Miksi 8,5 TT-asema ei voi luoda?

**A** voit valmistella paikallisesti kiinnitetty tietomääristä ylöspäin 8,5 TT tai tasoisen tietomääristä enintään 200 TT 8100 laitteeseen. Jos laitteen jo on tasoisen asemat ja valitse tilaa luomisen paikallisesti kiinnitetty äänenvoimakkuus on suhteessa pienempi kuin tässä enimmäismäärä. Esimerkiksi jos 100 TT: n Porrastettu asemat on jo valmisteltu 8100 laitteen (joka on puolet Porrastettu kapasiteetti), valitse paikallinen asema, jonka luot 8100 laitteeseen enimmäiskokoa vastaavasti vähennetään 4 TT (noin suurimmasta paikallisesti kiinnitetyt aseman kapasiteetti).

Koska laitteeseen paikallisen tilaa isännöimiseen Työsarja Porrastettu asemista, käytettävissä tilaa paikallisesti kiinnitetty aseman luominen heikkenee, jos laite on tasoisen asemat. Vastaavasti paikallisesti kiinnitetty aseman luominen suhteessa vähentää Porrastettu levyasemien käytettävissä olevaa tilaa. Seuraavassa taulukossa on yhteenveto käytettävissä Porrastettu kapasiteetti 8100 ja 8600: aa-laitteissa, kun paikallisesti kiinnitetty tietomääristä luodaan.

|Paikallisesti kiinnitetty tietomääristä valmistellun kapasiteetti|Käytettävissä oleva kapasiteetti Porrastettu levyasemien - 8100 Valmistellaan|Käytettävissä oleva kapasiteetti Porrastettu levyasemien - 8600 Valmistellaan|
|-----|------|------|
|0 | 200 TT | 500 TT |
|1 TT: | 176.5 TT | 477.8 TT|
|4 TT | 105.9 TT | 411.1 TT |
|8,5 TT | 0 TT | 311.1 TT|
|10 TT | PUUTTUU | 277.8 TT |
|15 TT | PUUTTUU | 166.7 TT |
|22.5 TT | PUUTTUU | 0 TT |


**Q.** Miksi paikallisesti kiinnitetty aseman luominen pitkään käynnissä toimintoa? 

**A.** Paikallisesti kiinnitetty tietomääristä thickly valmistelun yhteydessä. Paikallinen tasoa laitteen tilan luomiseksi aiemmin Porrastettu asemat-tietoja voi sijaita pilveen valmistelun yhteydessä. Ja koska tämä vaihtelee laitteen ja kaistanleveyttä pilvipalveluun olemassa olevia tietoja valmistelun yhteydessä, äänenvoimakkuuden koon aika, voit luoda paikallisen aseman voi olla useita tunteja.

**Q.** Kuinka kauan paikallisesti kiinnitetty aseman luominen kestää?

**A.** Paikallisesti kiinnitetty tietomääristä thickly valmisteltu, koska joitakin tietoja Porrastettu asemista voi sijaita pilveen valmistelun yhteydessä. Tämän vuoksi paikallisesti kiinnitetty aseman luominen aika riippuu useita tekijöistä, kuten äänenvoimakkuuden, laitteesi ja kaistanleveyttä tiedot koon. Juuri asennetun laitteissa, joissa on ei asemista, valitse paikallisesti kiinnitetty aseman luominen aika on noin 10 minuuttia teratavun tietojen kohden. Kuitenkin asemat luominen voi kestää useita tunteja selitetty edellä laitteessa, joka on käytössä tekijöiden perusteella.

**Q.** Haluan luoda paikallisesti kiinnitetty aseman. Mitä tahansa parhaita käytäntöjä minun täytyy huomioon?

**A.** Paikallisesti kiinnitetty tietomääristä soveltuvat toiminnoista, jotka edellyttävät paikallisten tietojen oikeudet aina ja merkitsevä cloud viiveet suurempia. Asiakkaiden käyttö asemat minkään oman toiminnoista, ota huomioon seuraavasti:

- Paikallisesti kiinnitetty tietomääristä thickly valmisteltu ja luominen asemat vaikuttaa Porrastettu levyasemien käytettävissä olevaa tilaa. Tästä syystä suosittelemme pienempi tietomääristä aloittaminen ja Skaalaa kuin tallennustilan vaatimus-kasvaa.

- Paikalliset asemat valmistelu on pitkä käynnissä toiminto, joka saattaa kuulua valitseminen olemassa olevia tietoja Porrastettu asemista pilveen. Tuloksena saattaa ilmetä rajoitetun suorituskyvyn asemat.

- Paikalliset asemat valmistelu on kauan. Useita tekijöitä määräytyy yhdistävää ajan: Valmistellaan äänenvoimakkuutta, tietoja laitteen ja kaistanleveyttä kokoa. Jos olet ole varmuuskopioinut aiemmin luodut asemat pilveen, äänenvoimakkuuden luominen on hidas. Suosittelemme tallentumaan cloud tilannevedosten aiemmin luodut asemat, ennen kuin voit valmistella paikalliseen asemaan.
 
- Voit muuntaa aiemmin luodut Porrastettu asemat paikallisesti kiinnitetty asemat ja tämä muuntaminen liittyy valmistelu tilaa laitteessa tuloksena paikallisesti kiinnitetty aseman (sen lisäksi, joka tuo alaspäin Porrastettu tietojen [mahdollisesti] pilvestä). Uudelleen Tämä on pitkä käynnissä toiminto, joka määräytyy on kerrottu yläpuolella. Suosittelemme varmuuskopioida ennen muuntamisen jälkeen aiemmin asemat, kun prosessi on heikompi myös, jos aiemmin luodut asemat varmuuskopioidaan ei. Laitteen myös kärsiä suorituskyvyn tämän prosessin aikana.
    
Lisätietoja siitä, miten voit [luoda paikallisesti kiinnitetty aseman](storsimple-manage-volumes-u2.md#add-a-volume)

**Q.** Useita paikallisesti kiinnitetty tietomääristä luominen samanaikaisesti

**A.** Kyllä, mutta paikallisesti kiinnitetty aseman luomista ja laajentamista töitä käsitellään peräkkäin.

Paikallisesti kiinnitetty tietomääristä thickly valmisteltu ja paikallisen tilaa laitteessa (mikä saattaa aiheuttaa olemassa olevia tietoja Porrastettu asemista, miten pilveen valmistelun aikana) luominen edellyttää. Vuoksi valmistelu työn ollessa käynnissä muut paikallisen aseman luominen työt olla jonossa kunnes, työ on valmis.

Vastaavasti jos aiemmin luotua paikallisen asemaa laajennetaan tai paikallisesti kiinnitetty aseman muunnetaan Porrastettu äänenvoimakkuutta, valitse uusi paikallisesti kiinnitetty asema luominen on jonossa ennen kuin edellisen työn on valmis. Aiemmin paikalliseen asemaan tilaa laajentamista paikallisesti kiinnitetty aseman koon laajentamista kuuluu. Porrastettu, paikallisesti kiinnitetyt muuntamista aseman siihen liittyy myös paikallisen tilaa luomista varten tuloksena paikallisesti kiinnitetyt äänenvoimakkuutta. Sekä nämä toiminnot, luominen tai laajentaminen paikallisen tilaa on pitkän käynnissä työn.

Voit tarkastella nämä työt Azure StorSimple hallintapalvelu **työt** -sivu. Työ, joka käsitellään aktiivisesti päivitetään jatkuvasti tilaa valmistelu etenemisen. Jäljellä oleva paikallisesti kiinnitetty aseman työt on merkitty on käytössä, mutta niiden etenemistä on pysähtynyt, toimi ja ne kerätään jonossa olleet järjestyksessä.

**Q.** Poistin paikallisesti kiinnitetty äänenvoimakkuutta. Miksi en näe näkyvät käytettävissä olevan tilan, kun yritän luoda uuden aseman regeneroitu tilaa? 

**A.** Jos poistat paikallisesti kiinnitetty äänenvoimakkuutta, uusia levytilaa eivät päivity välittömästi. StorSimple hallintapalvelu päivittää paikallisen tilaa noin kerran tunnissa. Suosittelemme, että odotat tuntia ennen kuin yrität luoda uuden äänenvoimakkuutta.

**Q.** Paikallisesti kiinnitetty tietomääristä cloud laitteen tukee?

**A.** Paikallisesti kiinnitetty tietomääristä ei tue cloud laitteen (8010 ja 8020 laitteet aiemmin nimitystä StorSimple virtual laite).

**Q.** Azure PowerShell cmdlet-komentojen avulla voit luoda ja hallita paikallisesti kiinnitetty tietomääristä 

**A.** Et voi luoda paikallisesti kiinnitetty tietomääristä Azure PowerShellin cmdlet-komennot (mitään asemaa, voit luoda PowerShellin Azure kautta on tasoisen) kautta. Myös Suosittelemme, että et käytä Azure PowerShellin cmdlet-komennot voit muokata mitä tahansa paikallisesti kiinnitetty aseman ominaisuuksia, kun se on valintojesi toivottavien vaikutus muokkaaminen asematyyppi, porrastettu.

## <a name="questions-about-backing-up-a-locally-pinned-volume"></a>Kysymyksiä paikallisesti kiinnitetty aseman varmuuskopiointi

**Q.** Paikallisesti kiinnitetty tietomääristä tueta paikallisessa tilannevedosten ovat?

**A.** Kyllä, voit ottaa paikallisen tilannevedoksen paikallisesti kiinnitetty asemat. Kuitenkin erittäin Suosittelemme, että voit varmuuskopioida säännöllisesti oman cloud tilannevedoksia varmistaa, että tiedot on suojattu huono tapauksessa paikallisesti kiinnitetty asemat.

**Q.** Onko ohjeet hallinta paikallisen tilannevedoksia paikallisesti kiinnitetty levyasemien?

**A.** Usein käytetyt paikallisen tilannevedoksia rinnalla tietojen churn paikallisesti kiinnitetty aseman suuri määrä voi aiheuttaa paikallisen tilaa kulutettu nopeasti ja aiheuttaa tietojen Porrastettu asemista niiden ollessa pilveen laitteeseen. Suosittelemme tämän vuoksi, voit pienentää paikallisen tilannevedosten määrää.

**Q.** Sain ilmoituksen siitä voi mitätöidään oma paikallinen tilannevedosten paikallisesti kiinnitetty asemat. Kun se johtuu?

**A.** Usein käytetyt paikallisen tilannevedoksia rinnalla tietojen churn paikallisesti kiinnitetty aseman suuri määrä voi aiheuttaa paikallisen tilaa kulutettu nopeasti laitteessa. Jos laitteen paikallisen tasoa raskaasti kuormitettu, muuttumisesta koko laite saattaa aiheuttaa laajennettu cloud käyttökatkosta ja saapuvien kirjoituksia asemaan saattaa aiheuttaa tilannevedosten rangaistussäännösten (kuten välilyöntiä olemassaolo ja Päivitä tiedot, jotka on korvattu vanhempia lohkot viittaaminen tilannevedoksia). Tilanne asemaan kirjoituksia edelleen oltava tiedoksi, mutta paikallisen tilannevedoksia voi olla virheellinen. Ei ole vaikuttavia tekijöitä aiemmin cloud-tilannevedoksia.

Ilmoitusten varoitus on ilmoittaa, että tilanne voi syntyä ja harkita sama ajoissa joko paikallisen tilannevedoksia aikataulut tulevat vähemmän usein paikallisen tilannevedoksia tarkistaminen tai poistamalla vanhat paikallisen tilannevedoksia, joita ei enää tarvita.

Jos paikallisen tilannevedoksia mitätöidään, saat ilmoituksen tiedot-ilmoitusta tietyn varmuuskopion käytännön paikallisen tilannevedoksia on poistettu käytöstä paikallisen tilannevedoksen, joka on mitätöidään aikaleimat luettelo rinnalla. Nämä tilannevedoksia ole automaattinen poistetaan ja ei enää voi tarkastella niitä **Varmuuskopiointi luettelot** -sivulla Azure perinteinen portaalissa.

## <a name="questions-about-converting-a-tiered-volume-to-a-locally-pinned-volume"></a>Porrastettu aseman muuntaminen paikallisesti kiinnitetty aseman kysymyksiä

**Q.** I 'M noudattaen joitakin hitaus laitteeseen aikana Porrastettu aseman muuntaminen paikallisesti kiinnitetty äänenvoimakkuutta. Mistä tämä johtuu? 

**A.** Muuntaminen on kaksi vaihetta: 

  1. Avauksen ja vaihdon valmistelu tilaa laitteessa pian –--muunnetaan paikallisesti kiinnitetty.
  2. Porrastettu tietojen lataaminen pilvestä, jotta paikallisen takaa.

Molemmat vaiheet ovat pitkiä käynnissä toiminnot, jotka ovat riippuvaisia muuntamisen, laite ja kaistanleveyttä äänenvoimakkuuden kokoa. Kun aiemmin luodut Porrastettu asemat-tietoja voi spill pilveen valmistelun yhteydessä, laitteesi kärsiä suorituskyvyn tänä aikana. Lisäksi muuntaminen voi olla hidas Jos:

- Aiemmin luodut asemat ei ole varmuuskopioinut pilveen; niin kannattaa varmuuskopioida ennen aloitetaan muunnoksen jälkeen asemat.

- Kaistanleveyden rajoittava käytännöt on käytetty, joka saattaa rajoittaa pilveen; kaistanleveyttä Suosittelemme sen vuoksi, sinulla on oma 40 Mbps tai Lisää yhteys pilveen.

- Muuntaminen voi kestää useita tunteja selitetty edellä, useita tekijöitä vuoksi Tästä syystä suosittelemme suorittaa tämän toiminnon aikana päät aikoja tai viikonlopun Lopeta kuluttajille vaikutus välttämiseksi.

Lisätietoja siitä, miten voit [muuntaa Porrastettu aseman paikallisesti kiinnitetty levylle](storsimple-manage-volumes-u2.md#change-the-volume-type)

**Q.** Äänenvoimakkuuden muunto-toiminnon peruuttaminen

**A.** Ei, et voi Peruuta kerran aloittanut muuntotoiminto. Kuten edellä edellisessä vastauksessa, ota huomioon mahdollisen suorituskyvyn ongelmista, että voit aikana ilmenee ja yllä olevassa luettelossa, kun suunnittelet oman muuntaminen parhaita käytäntöjä.

**Q.** Mitä tapahtuu Omat asemaan, jos muuntaminen epäonnistuu?

**A.** Äänenvoimakkuuden muuntaminen voi epäonnistua cloud yhteysongelmien vuoksi. Laitteen myöhemmin saattaa lopettaa muuntamisen jälkeen epäonnistuneita yrityksiä tuomaan alaspäin Porrastettu tietoja pilvestä sarjaa. Näiden tilanteessa asematyyppi on edelleen aseman lähdetyyppi ennen muuntaminen, ja:

- Kriittinen ilmoitus korotetaan ilmoittamaan aseman muuntovirhe. Saat lisätietoja [paikallisesti kiinnitetty tietomääristä liittyvät ilmoitukset](storsimple-manage-alerts.md#locally-pinned-volume-alerts)

- Jos haluat muuntaa Porrastettu kiinnitetty paikalliseen asemaan, äänenvoimakkuuden säilyvät toimimaan Porrastettu asemaa, kuten tietoja voi silti sijaitsevat pilveen. Suosittelemme, että yhteysongelmat Ratkaise ja yritä muuntotoiminto.
 
- Vastaavasti kun muunnos paikallisesti kiinnitetty Porrastettu asemaan epäonnistuu, vaikka merkitään äänenvoimakkuuden paikallisesti kiinnitetty äänenvoimakkuutta, se toimia Porrastettu aseman (koska tietoja voi olla spilled pilvipalveluun). Kuitenkin säilyy tilaa, valitse paikallinen tasoa laitteen. Tähän eivät ole käytettävissä muissa paikallisesti kiinnitetty asemat. Suosittelemme tämän toiminnon varmistaaksesi, että aseman muunnos on tehty ja paikallisen tilaa laitteessa on palautettava yritä uudelleen.

## <a name="questions-about-restoring-a-locally-pinned-volume"></a>Kysymyksiä paikallisesti kiinnitetty aseman palauttaminen

**Q.** Palauttaa välittömästi paikallisesti kiinnitetty asemat ovat?

**A.** Kyllä, paikallisesti kiinnitetty tietomääristä palautetaan välittömästi. Kun äänenvoimakkuuden metatietojen tiedot tulevat pilvestä palautustoiminto osana, äänenvoimakkuuden on online-tilaan ja niitä voi käyttää isäntä. Kuitenkin paikallisen oikeudet aseman tiedot eivät ole käytössä, kunnes kaikki tiedot on ladattu pilvestä, ja voit kokea vähentää asemat suorituskyvyn Palauta ajaksi.

**Q.** Kuinka kauan Palauta paikallisesti kiinnitetty asema kestää?

**A.** Paikallisesti kiinnitetty tietomääristä palauttaa välittömästi ja online-tilaan mahdollisimman pian volume metatietoja haetaan pilvipalveluun, kun aseman tiedot edelleen ladata taustalla. Palautustoiminto--käytön takaisin paikalliseen oikeudet aseman--tietojen jälkimmäisessä tässä osassa on pitkä käynnissä toiminto ja voi kestää useita tunteja voidaan tehdä uudelleen paikalliset tiedot. Viimeistele sama aika määräytyy useita tekijät, kuten palautetaan parhaillaan äänenvoimakkuuden koon ja kaistanleveyttä. Jos alkuperäisen asema, joka palautetaan on poistettu, ylimääräisen kerran otetaan on luotu paikallisen tilaa laitteeseen palautustoiminto osana.

**Q.** Minun täytyy palauttaa omat aiemmin paikallisesti kiinnitetty asemaa vanhempia tilannevedoksen, joka on (varovainen äänenvoimakkuuden on tasoisen). Palautetaan äänenvoimakkuuden, koska tällöin tasoisen?

**A.** Ei, äänenvoimakkuuden voi palauttaa paikallisesti kiinnitetty äänenvoimakkuutta. Vaikka tilannevedoksen päivämäärät ajaksi, kun äänenvoimakkuuden on tasoisen palauttaminen aiemmin luodut asemat, kun StorSimple käyttää aina aseman tyypin levyllä, kun se on tällä hetkellä.

**Q.** Voin laajennettu Omat paikallisesti kiinnitetty aseman äskettäin, mutta haluan nyt palauttaa aika, jona äänenvoimakkuuden on pienempi kokoisia tiedot. Palauta koko muuttuu nykyistä äänenvoimakkuutta ja laajentaa äänenvoimakkuuden kokoa, kun palautus on valmis on on?

**A.** Kyllä, Palauta kokoa äänenvoimakkuuden ja haluat laajentaa äänenvoimakkuuden kokoa, kun palautus on valmis.

**Q.** Aseman tyypin muuttaminen palauttamisen aikana

**A.** Ei, et voi muuttaa aseman tyypin palauttamisen aikana.

- Tallennettu tilannevedoksen tyypiksi palautetaan asemat, jotka on poistettu.

- Aiemmin luodut asemat palautetaan nykyisen lajin riippumatta tallennettu tilannevedoksen tyyppi (Lisätietoja edellisen kahta kysymystä).
 
**Q.** Haluat palauttaa omat paikallisesti kiinnitetty äänenvoimakkuutta, mutta voin poimia virheellinen pisteen aika tilannevedoksen. Voit peruuttaa nykyistä palauttamista?

**A.** Voit peruuttaa jatkuvaa-palautustoiminto. Äänenvoimakkuuden tilan peruutetaan tilaan palauttamisen alkuun. Kuitenkin kirjoituksia, joka on tehty äänenvoimakkuuden Palauta ollessa käynnissä, menetetään.

**Q.** Aloittamaani palautustoiminto johonkin Omat paikallisesti kiinnitetty asemat ja nyt näkyviin tilannevedoksen Omat keskeneräisen luetteloa, jota voin ei recollect luominen. Mitä tämä käytetään?

**A.** Tämä on väliaikainen tilannevedoksen, joka on luotu ennen palautustoiminto ja käytetään palauttaminen siltä varalta, palautus on peruutettu tai epäonnistuu. Älä poista tämä tilannevedos; se poistetaan automaattisesti, kun palautus on valmis. Tämä ongelma voi ilmetä, jos oman palautustyön vain paikallisesti on kiinnitetty asemat tai sekä paikallisesti kiinnitetty ja Porrastettu asemat. Jos palautustyön sisältää vain Porrastettu asemat, tämä ongelma ilmene.

**Q.** Paikallisesti kiinnitetty aseman Kloonaa

**A.** Kyllä, voit. Kuitenkin paikallisesti kiinnitetty äänenvoimakkuus voi kopioida Porrastettu aseman kuin oletusarvoisesti. Lisätietoja siitä, miten voit [Kloonaa paikallisesti kiinnitetty aseman](storsimple-clone-volume-u2.md)

## <a name="questions-about-failing-over-a-locally-pinned-volume"></a>Kysymyksiä epäonnistumista paikallisesti kiinnitetty aseman päälle

**Q.** Tarvitsen epäonnistuu laitteeseeni toisen laitteen fyysinen päälle. Oma paikallisesti kiinnitetty tietomääristä epäonnistuu yli paikallisesti kiinnitetyt tai tasoisen?

**A.** Kohde-laitteen ohjelmisto-version mukaan paikallisesti kiinnitetty tietomääristä epäonnistuu päälle nimellä:

- Jos kohde laitteesi käyttöjärjestelmänä on StorSimple 8000 paikallisesti kiinnitetyt sarjan Päivitä 2
- Jos kohde laitteesi käyttöjärjestelmänä on StorSimple tasoisen 8000 sarjan Päivitä 1.x
- Jos kohdelaitteen cloud laitteen tasoisen (versio ohjelmistopäivitys 2 tai Päivitä 1.x)

Saat lisätietoja [automaattisesti ja DR, paikallisesti kiinnitetyt tietomääristä eri versiot](storsimple-device-failover-disaster-recovery.md#device-failover-across-software-versions)

**Q.** Paikallisesti kiinnitetty tietomääristä välittömästi palautetaan palauttaminen (DR) aikana?

**A.** Kyllä, paikallisesti kiinnitetty tietomääristä palautetaan välittömästi aikana automaattisesti. Kun äänenvoimakkuuden metatietojen tiedot tulevat pilvestä osana automaattisesti-toimintoa, äänenvoimakkuuden on online-tilaan kohde laitteessa ja niitä voi käyttää isäntä. Tänä aikana aseman tiedot säilyvät Lataa taustalla ja asemat vikasietotilaa ajaksi rajoitettu suorituskyky saattaa olla.

**Q.** Näe automaattisesti työ valmis, miten voit voin etenemisen seuranta paikallisesti kiinnitetty asema, joka palautetaan kohde-laitteessa?

**A.** Automaattisesti aikana automaattisesti työ on merkitty valmiina kerran kaikki asemat automaattisesti joukossa on välittömästi palauttaa ja online-tilaan kohde-laitteessa. Tämä vaihtoehto sisältää kaikki paikallisesti kiinnitetty asemat, joka voi olla on epäonnistunut päälle, kuitenkin paikallisen oikeudet tiedot ovat vain valittavissa äänenvoimakkuuden kaikki tiedot on ladattu. Voit seurata tämän edistymistiedot kunkin paikallisesti kiinnitetty asema, joka korvaa seuranta vastaavan palauttaminen työt, jotka on luotu osana vikasietotilaa epäonnistui. Nämä yksittäiset palauttaminen työt luodaan vain paikallisesti kiinnitetty asemat.

**Q.** Aseman tyypin muuttaminen automaattisesti aikana

**A.** Ei, et voi muuttaa asematyyppi automaattisesti aikana. Jos olet epäonnistuvat päälle toiseen fyysinen laitteeseen, jossa on käytössä StorSimple 8000 sarjan päivityksen 2, asemat epäonnistuu päälle tallennettu tilannevedoksen aseman tyypin mukaan. Kun kaatuvat kautta laitteen-versioon, viitata kysymyksen aseman tyypin jälkeen automaattisesti.

**Q.** Voin epäonnistui päälle aseman säilön paikallisesti kiinnitetty asemat cloud-laitteen kanssa?

**A.** Kyllä, voit. Paikallisesti kiinnitetty tietomääristä epäonnistuu päälle kuin Porrastettu asemat. Saat lisätietoja [automaattisesti ja DR, paikallisesti kiinnitetyt tietomääristä eri versiot](storsimple-device-failover-disaster-recovery.md#considerations-for-device-failover)



<properties
    pageTitle="Parhaat käytännöt Azure näytön automaattisen skaalauksen poistaminen. | Microsoft Azure"
    description="Tietoja automaattisen skaalauksen poistaminen käyttäminen tehokkaasti Azure näytön periaatteet."
    authors="kamathashwin"
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
    ms.date="10/20/2016"
    ms.author="ashwink"/>

# <a name="best-practices-for-azure-monitor-autoscaling"></a>Parhaat käytännöt Azure näytön automaattisen skaalauksen poistaminen

Seuraavissa osissa tässä asiakirjassa auttavat ymmärtämään parhaita käytäntöjä, Automaattinen skaalaus Azure. Kun olet tarkistanut nämä tiedot, voit voi paremmin Automaattinen skaalaus käyttäminen tehokkaasti Azure infrastruktuuria.

## <a name="autoscale-concepts"></a>Automaattinen skaalaus käsitteitä

- Resurssi voi olla vain *yksi* Automaattinen skaalaus-asetus
- Automaattinen skaalaus-asetus voi olla yksi tai profiilien ja kunkin profiilin voi olla vähintään yksi Automaattinen skaalaus-sääntöjä.
- Automaattinen skaalaus-asetus Skaalaa esiintymät vaakasuunnassa, *joka on suurentamalla esiintymät ja *Valitse* pienentämällä esiintymien määrän* .
 Automaattinen skaalaus-asetus on suurin, pienin- ja esiintymien oletusarvo.
- Automaattinen skaalaus-työ lukee aina liittyvät metrijärjestelmä skaalata, tarkistaminen, jos se on pystyyn määritetty kynnysarvo asteikko-kohtaa tai asteikko. Voit tarkastella luetteloa, arvot voi skaalata, että Automaattinen skaalaus osoitteessa [Azure näytön automaattisen skaalauksen poistaminen yleiset arvot](insights-autoscale-common-metrics.md).
- Kaikki raja-arvot lasketaan esiintymän tasolla. Esimerkiksi "1 esiintymän lisäämällä asteikko keskimääräinen kun suorittimen > 80 %, kun esiintymän määrä on 2", tarkoittaa asteikko-kohtaa, kun keskimääräinen suorittimen kaikki esiintymät koko on yli 80 %.
- Saat aina virhesanomat sähköpostitse. Tarkemmin sanottuna omistaja, avustaja ja lukijat kohde resurssin vastaanottaa sähköpostia. Saat *palautus* -sähköpostia myös aina, kun Automaattinen skaalaus palauttaa virheen ja käynnistää toimivat normaalisti.
- Voit voit sisältyy onnistuneen asteikko-toiminnon ilmoituksen sähköpostitse ja webhooks.

## <a name="autoscale-best-practices"></a>Automaattinen skaalaus parhaat käytännöt

Käytä seuraavien parhaiden käytäntöjen, kun käytät Automaattinen skaalaus.

### <a name="ensure-the-maximum-and-minimum-values-are-different-and-have-an-adequate-margin-between-them"></a>Suurimmat ja pienimmät arvot ovat eri ja riittävä reunuksen, niiden välillä
Jos sinulla on asetus, joka on pienin = 2 suurin = 2 ja nykyisen esiintymän määrä on 2, asteikko mitään toimia ei voi ilmetä. Pidä riittävä reunuksen, suurin ja pienin esiintymän laskelmia, jotka ovat välillä. Automaattinen skaalaus Skaalaa aina nämä raja-arvot välillä.

### <a name="manual-scaling-is-reset-by-autoscale-min-and-max"></a>Manuaalinen skaalaus palauttaa Automaattinen skaalaus min- ja max
Jos päivität esiintymän Laske manuaalisesti ylä- tai alapuolelle suurin arvo, Automaattinen skaalaus-ohjelma Skaalaa automaattisesti takaisin pienin arvo (Jos alapuolella) tai enintään (Jos yläpuolella). Voit määrittää esimerkiksi 3 ja 6 välillä. Jos sinulla on yksi suoritettavan, Automaattinen skaalaus-ohjelma Skaalaa 3 esiintymissä seuraavan suorittamisen. Vastaavasti se asteikko-8 esiintymät takaisin 6 seuraavan suorittamisen.  Manuaalinen skaalaus on hyvin tilapäinen, ellei Palauta sekä Automaattinen skaalaus-sääntöjä.

### <a name="always-use-a-scale-out-and-scale-in-rule-combination-that-performs-an-increase-and-decrease"></a>Käytä aina asteikko-kohtaa ja skaalaus-sääntö, joka suorittaa lisäys- ja Pienennä yhdistelmä
Jos käytössäsi on vain yksi osa yhdistelmä, Automaattinen skaalaus mittakaava, jonka yksittäinen ulos tai sisään, ennen kuin suurin ja pienin, on saavutettu.

### <a name="do-not-switch-between-the-azure-portal-and-the-azure-classic-portal-when-managing-autoscale"></a>Siirry Azure portaalin ja Azure perinteinen-portaalin välillä, kun Automaattinen skaalaus hallinta
Pilvipalveluihin ja sovelluksen palveluja (online) Käytä Azure portaalin (portal.azure.com) voit luoda ja hallita Automaattinen skaalaus-asetukset. Virtuaalikoneen asteikko joukkojen avulla voit luoda ja hallita Automaattinen skaalaus-asetus PoSH, CLI tai REST API. Siirry Azure perinteinen portaalin (manage.windowsazure.com) ja Azure-portaalin (portal.azure.com) välillä hallinnassa Automaattinen skaalaus-määrityksiä. Azure perinteinen portaalin ja sen pohjana Taustajärjestelmä on rajoitukset. Siirtää Azure-portaaliin hallinta Automaattinen skaalaus käyttämällä graafisessa käyttöliittymässä. Vaihtoehdot ovat käyttämään Automaattinen skaalaus PowerShell, CLI tai REST API (joko Azure resurssin Explorer).

### <a name="choose-the-appropriate-statistic-for-your-diagnostics-metric"></a>Valitse haluamasi diagnostiikka-arvo-tilasto
Diagnostiikan arvot, voit valita *keskiarvo*, *vähintään*, *enintään* ja *yhteensä* kuin mittarin skaalata. Yleisimmät tilasto on *keskiarvon*.

### <a name="choose-the-thresholds-carefully-for-all-metric-types"></a>Valitse huolellisesti tyypeissä metrisillä raja-arvot
Kannattaa valita huolellisesti eri raja-asteikko-kohtaa ja skaalaus-käytännön tilanteissa perusteella.

Emme *suosittele* Automaattinen skaalaus-asetuksia, kuten alla esimerkkejä saman tai hyvin samankaltaisia kuin raja-arvot ulos ja ehtojen kanssa:

- Suurentaa esiintymät 1 laskeminen, kun säikeen Laske < = 600
- Pienentää esiintymät 1 laskeminen, kun säikeen Laske > = 600

Katsotaan esimerkki siitä, mikä voi aiheuttaa ongelma, joka saattaa näyttää sekava. Cosider seuraavassa järjestyksessä.

1. Oletetaan, että on 2 esiintymät aluksi ja sitten kohden esiintymän keskimääräinen määrä kasvaa 625.
2. Automaattinen skaalaus Skaalaa ulos lisääminen 3 esiintymä.
3. Seuraavaksi oletetaan, että esiintymän yli keskiarvon viestiketjun-Laske kuuluu 575 avulla.
4. Ennen skaalauksen, Automaattinen skaalaus yrittää arvioida mitä Lopputila on jos se on skaalattu. Esimerkiksi 575 x 3 (nykyisen esiintymän määrä) = 1,725 / 2 (lopullinen määrä esiintymät, kun skaalata) = 862.5 viestiketjuissa siirtyminen. Tämä tarkoittaa Automaattinen skaalaus on heti asteikko-kohtaa uudelleen senkin jälkeen, kun se skaalata, jos Laske keskiarvo viestiketjun säilyy ennallaan tai kuuluu myös vain vähän. Jos se skaalata uudelleen koko prosessia toista, johtavat äärettömän silmukan.
5. Voit välttää tämän tilanteen (eli "flapping"), Automaattinen skaalaus skaalaudu ollenkaan. Sen sijaan ohittaa ja reevaluates ehto uudelleen palvelun työn suorittaa seuraavan kerran. Tämä saattaa sekoittaa moni, koska Automaattinen skaalaus ei toimi, kun keskimääräinen viestiketjun määrä on 575.

Arvio aikana asteikko-tarkoituksena on estää "flappy". Tämä ongelma pitää mielessä, kun valitset saman raja-asteikko-kohtaa ja valitse.

Suosittelemme riittävä reunuksen välinen asteikko-kohtaa ja raja-arvot. Esimerkkinä harkitse paremmin sääntö-yhdistelmä.

- Suurentaa esiintymät 1 laskeminen, kun suorittimen % > = 80
- Pienentää esiintymät 1 laskeminen, kun Suoritin % < = 60

Tässä tapauksessa  

1. Oletetaan, että on 2 esiintymät aloittaa numeroinnin.
2. Jos keskiarvo Suoritin-% eri esiintymien välillä siirtyy 80, Automaattinen skaalaus Skaalaa ulos lisääminen kolmannen esiintymä.
3. Nyt oletetaan, että ajan kuluessa Suoritin-% kuuluu 60.
4. Automaattinen skaalaus 's asteikko-sääntö arvioi Lopputila, jos se on asteikko sisään. Esimerkiksi 60 x 3 (nykyisen esiintymän määrä) = 180 / 2 (lopullinen määrä esiintymät, kun skaalata) = 90. Jotta Automaattinen skaalaus eivät asteikko-koska se on asteikko-kohtaa uudelleen heti. Sen sijaan se siirtyy skaalaus.
5. Seuraavan kerran Automaattinen skaalaus-tarkistaa Suoritin edelleen oltava 50. Laskee uudelleen – 50 x 3-esiintymän = 150 / 2 esiintymät = 75, jossa on alle 80, asteikko-kohtaa raja-arvo, joten se Skaalaa onnistuneesti 2 esiintymissä.

### <a name="considerations-for-scaling-threshold-values-for-special-metrics"></a>Huomioitavaa skaalaus määräten arvot raja-arvot
 Erityiset arvot, kuten tallennustilan tai palvelun Bus jonon pituuden arvo raja-arvo on käytettävissä olevan numeron esiintymien viestien määrän keskiarvo. Valitse Valitse huolellisesti tämän metrijärjestelmän raja-arvo.

Oletetaan, että se osoittavat ja Esimerkki ymmärrät paremmin toiminnan varmistamiseksi.

- Suurentaa esiintymät 1 Laske kun jonossa tallennustilan määrä > = 50
- Pienentää esiintymät 1 Laske kun jonossa tallennustilan määrä < = 10

Harkitse seuraavassa järjestyksessä:

1. On 2 tallennustilan jonon esiintymät.
2. Viestien säilyttää tulossa ja kun tarkistat tallennustilan jonossa, kokonaismäärä lukee 50. Ehkä oletetaan, että Automaattinen skaalaus alkamisajan asteikko ulos-toiminto. Huomaa kuitenkin, että se on edelleen 50/2 = 25 viestien määrä esiintymä. Näin on asteikko-kohtaa ei tapahdu. Ensimmäisen asteikko-kohtaa ilmenee tallennustilan jonossa kaikkien viestien määrä on 100.
3. Seuraavaksi oletetaan, että kaikkien viestien määrä saavuttaa 100.
4. 3 tallennustilan-jonossa esiintymän lisätään vuoksi asteikko ulos-toiminto.  Seuraava asteikko ulos-toiminto ei suoriteta, kunnes kaikkien viestien määrä jonossa saavuttaa 150, koska 150/3 = 50.
5. Nyt jonossa olevien viestien määrä saa pienempiä. 3 esiintymät ensimmäinen asteikko-toiminto tapahtuu, kun kaikki olevien yhteensä viestit lisätä enintään 30, koska 30/3 = 10 viestien määrä esiintymän, joka on Skaalaus-kohdassa.

### <a name="considerations-for-scaling-when-multiple-profiles-are-configured-in-an-autoscale-setting"></a>Huomioitavaa skaalausta, kun Automaattinen skaalaus-asetus on määritetty profiileja

Automaattinen skaalaus-asetusta, voit valita oletusprofiilin, jota käytetään aina ilman riippuvuus aikataulun tai ajan, tai voit valita toistuvan profiilin tai profiilin määräajan päivämäärä ja aika-alue.

Kun Automaattinen skaalaus-palvelun käsittelee niitä, se aina tarkistaa seuraavassa järjestyksessä:

1. Kiinteän päivämäärän profiili
2. Toistuva profiili
3. Oletusprofiiliksi ("aina")

Jos profiilin ehto täyttyy, Automaattinen skaalaus ei tarkista seuraava profiilin ehto, sen alapuolella. Automaattinen skaalaus käsittelee kerrallaan vain yhtä profiilia. Tämä tarkoittaa sitä, voit halutessasi myös alemman tason profiilista käsittely ehdon on lisättävä kyseiset säännöt sekä nykyisen profiilin.

Seuraavaksi tarkastellaan tämän esimerkin avulla:

Alla olevassa kuvassa näkyy Automaattinen skaalaus-asetus kanssa oletusprofiilin pienin esiintymien = 2- ja enimmäisarvot esiintymät = 10. Tässä esimerkissä säännöt määritetään asteikko-kohtaa, kun viestin jonossa määrä on suurempi kuin 10 ja mittakaava, kun jonossa viestin määrä on alle 3. Nyt resurssin skaalata 2 ja 10 esiintymien välillä.

Lisäksi on toistuva profiilin maanantai määrittäminen. Se on määritetty vähintään esiintymien = 2- ja enimmäisarvot esiintymät = 12. Tämä tarkoittaa maanantaina, tämä ehto tarkistaa ensimmäisen kerran Automaattinen skaalaus Jos esiintymän määrä on 2, se sovittaa 3 uusi vähän. Kun Automaattinen skaalaus jatkaa Etsi profiilin ongelmaa vastaa (maanantai), vain käsittelee Suoritin asteikko ulos tai sisään sääntöjen määritetty profiilia. Tällä hetkellä se ei tarkista jonon pituuden. Kuitenkin, voit halutessasi myös jonon pituuden ehto tarkistettava Sisällytä nämä oletusprofiili säännöt sekä maanantai profiiliin.

Vastaavasti kun Automaattinen skaalaus vaihtaa takaisin oletusprofiili, se tarkistaa ensin vähimmäis- ja edellytykset täyttyvät. Jos kerrat aikaa on 12, se sovittaa 10, suurin sallittu oletusprofiilin.

![Automaattinen skaalaus-asetukset](./media/insights-autoscale-best-practices/insights-autoscale-best-practices.png)

### <a name="considerations-for-scaling-when-multiple-rules-are-configured-in-a-profile"></a>Huomioitavaa skaalausta, kun useita sääntöjä määritetään profiilissa
On tapauksia, joissa on ehkä määrittää useita sääntöjä profiili. Automaattinen skaalaus seuraavat sääntöryhmän käytetään palveluiden avulla, kun useita sääntöjä on määritetty.

Automaattinen skaalaus suorittaa *Mittakaava,*mitä tahansa sääntöä täyttyessä.
Valitse *asteikko-*Automaattinen skaalaus edellyttävät on täytyttävä, kaikki säännöt.

Esitä-oletetaan, että sinulla on 4 Automaattinen skaalaus-sääntöjä:

- Jos suorittimen < 30 %, asteikko-1
- Jos muistin < 50 %, asteikko-1
- Jos Suoritin > 75 %, asteikko-kohtaa 1
- Jos muistin > 75 %, asteikko-kohtaa 1

Noudata toteutuu:

- Jos Memory is 50 %: n Suoritin on 76 %, emme asteikko-kohtaa.
- Jos Suoritin on 50 % ja muistin 76 prosenttiin on asteikko-kohtaa.

Toisaalta, jos Suoritin on 25 % ja muistin 51 prosenttia Automaattinen skaalaus ei **ole** Skaalaus-kohdassa. Jotta asteikko-, Suoritin on oltava 29 % ja muistin 49 %.

### <a name="always-select-a-safe-default-instance-count"></a>Valitse aina turvallisten oletusarvo-esiintymän määrä
Oletusarvoinen esiintymän määrä on tärkeää Automaattinen skaalaus Skaalaa palvelussa kyseinen määrä, kun arvot eivät ole käytettävissä. Valitse vuoksi oletusarvon esiintymän määrä, joka on turvallista oman toiminnoista.

### <a name="configure-autoscale-notifications"></a>Automaattinen skaalaus-ilmoitusten määrittäminen
Automaattinen skaalaus ilmoittaa Järjestelmänvalvojat ja osallistujien resurssin sähköpostitse ilmetä jokin seuraavista ehdoista:

- Automaattinen skaalaus-palvelun epäonnistuu suorittavan toiminnon.
- Arvot eivät ole käytettävissä Automaattinen skaalaus-palvelun asteikko päätöksentekoa.
- Arvot ovat käytettävissä (palautus) uudelleen, jotta asteikko päätöksentekoa.
Edellä mainitut ehdot lisäksi voit määrittää sähköpostin tai webhook ilmoituksia, jotka saat ilmoituksen onnistuneen asteikko-toiminnot.

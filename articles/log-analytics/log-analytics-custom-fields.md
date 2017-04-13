<properties
   pageTitle="Mukautettujen kenttien Log Analytics | Microsoft Azure"
   description="Log Analytics mukautetut kentät-toiminnon avulla voit luoda omia hakukenttiä kerättyjen tietueen ominaisuuksien lisääminen OMS tiedoista.  Tässä artikkelissa kuvataan mukautettu kenttä luodaan ja otoksen tapahtumaan laskun."
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

# <a name="custom-fields-in-log-analytics"></a>Lokitiedoston Analytics mukautetut kentät

Lokitiedoston Analytics **Mukautetut kentät** -toiminnon avulla voit laajentaa olemassa olevien tietueiden OMS säilössä lisäämällä omia hakukenttiä.  Mukautetut kentät täytetään automaattisesti tiedoista poimittujen tietueessa muita ominaisuuksia.

![Mukautetut kentät-yleiskatsaus](media/log-analytics-custom-fields/overview.png)

Esimerkiksi alla otoksen tietueella on hyödyllisiä tietoja kadota muiden tapahtuman kuvaus.  Poimii tiedoista erilliseen ominaisuuksiin mahdollistaa sen käytön toimia kuin lajittelu ja suodatus.

![Kirjaudu hakupainike](media/log-analytics-custom-fields/sample-extract.png)

>[AZURE.NOTE] Esikatselussa sinun on oikeutettu enintään 100 mukautetut kentät työtilassa.  Tämä rajoitus laajennetaan, kun tämä toiminto saavuttaa yleiseen käyttöön.

## <a name="creating-a-custom-field"></a>Mukautetun kentän luominen

Kun luot mukautetun kentän, loki Analytics on hyvä tietää arvonsa täyttää tiedot.  Se käyttää Microsoft Researchin kutsutaan FlashExtract-tekniikkaa tunnistaa nopeasti tiedoista.  Sen sijaan, että edellyttävän voit lisätä eksplisiittinen ohjeita Log Analytics oppii haluat poimia esimerkkejä, jotka saadaan tiedoista.

Seuraavissa osioissa kuvatulla tavalla mukautetun kentän luomista varten.  Tässä artikkelissa alareunassa on esimerkki poiminnan vaiheista.

> [!NOTE] Mukautettu kenttä lisätään tiettyjä ehtoja vastaavia tietueita on lisätty OMS tietovaraston, niin se näkyy vain tietueiden kerääminen mukautetun kentän luomisen jälkeen.  Mukautettu kenttä lisätään ei tietueet, jotka ovat jo tietovaraston luonnin yhteydessä.

### <a name="step-1--identify-records-that-will-have-the-custom-field"></a>Vaihe 1 – löytää tietueet, jotka on mukautettu kenttä
Ensimmäiseksi on tunnistaa tietueet, jotka saavat mukautettu kenttä.  [Vakio log haku](log-analytics-log-searches.md) alkaa ja valitse sitten haluamasi malli Log Analytics oppiminen edustajana tietueen.  Jos määrität, että olet luomassa poimimaan mukautettu-kenttään, **Kentän poiminnan ohjattu** on avattu, jossa Vahvista ja tarkentaa ehtoja.

2. Siirry **Log haku** ja [kysely, joka noutaa tietueet](log-analytics-log-searches.md) , jotka on mukautettu kenttä.
2. Valitse tietue, joka Log Analytics käyttää mallina, joka poimii tietoja mukautetun kentän perusteella.  Tiedot, jotka haluat poimia tämä tietue tunnistetaan ja Log Analytics käyttää näitä tietoja logiikan mukautetun kentän kaikkien samalla tietueiden määrittämiseen.
3. Minkä tahansa tekstin ominaisuuden tietueen vasemmalle-painiketta ja valitse **Pura kenttiä**.
4. **Kentän poiminnan ohjattu avataan**ja valitsit tietue näkyy **Main esimerkissä** sarake.  Mukautettu kenttä määritettävä ne tietueet, jossa on samat arvot-ominaisuudet, jotka ovat valittuina.  
5. Jos valinta on ei täsmälleen oikein, valitse Lisää kenttiä, voit tarkentaa ehtoja.  Jotta voit muuttaa ehtoja kenttien arvoja, Peruuta ja valitse haluamasi ehdot täyttävää toiseen tietueeseen.

### <a name="step-2---perform-initial-extract"></a>Vaihe 2 – Suorita alkuperäinen Pura.
Kun olet määrittänyt tietueet, jotka on mukautettu kenttä, Määritä tiedot, jotka haluat purkaa.  Lokitiedoston Analytics käyttämällä näitä tietoja Määritä samalla vastaavat tietueet.  Tämän jälkeen vaiheessa osaat Tarkista tulokset ja antaa lisätietoja lokin Analytics käyttämään sen tiedot.

1. Korosta teksti otoksen tietueessa, jonka haluat täyttää mukautettu kenttä.  Valitse näyttöön tulee valintaikkuna kentän nimi ja alkuperäinen Pura tekemiseen.  Merkkien ** \_KV** lisätään automaattisesti.
2. Valitse **Pura** analysoinnissa kerättyjen tietueita.  
3. **Yhteenveto** - ja **Etsinnän tulokset** -osien näyttää tulokset Pura, joten tarkistat sen tarkkuutta.  **Yhteenveto** näkyy tietueiden ja laskeminen tunnistetaan kunkin tunnistaa tietoarvojen ehdot.  **Hakutuloksia** on yksityiskohtainen luettelo ehtoja vastaavia tietueita.


### <a name="step-3--verify-accuracy-of-the-extract-and-create-custom-field"></a>Vaihe 3 – Tarkista tarkkuudella Pura ja mukautetun kentän luominen

Kun olet suorittanut ensimmäisen Pura, loki Analytics näkyy sen tulokset, joka on jo kerättyjen tietojen perusteella.  Jos tulokset Näytä vastaavan todellisuutta voit luoda mukautetun kentän et lisätyötä kanssa.  Jos et valitse voit tarkentaa tuloksia, niin, että loki Analytics parantaa sen logiikan.

2.  Jos arvojen alkuperäinen Pura ne ovat virheellisiä, napsauta epätarkkoja tietueen vieressä olevaa **Muokkaa** -kuvaketta ja valitse **Muokkaa tätä Korosta** jotta voit muokata valintaa.
3.  **Muita esimerkkejä** osan alapuolella **Main Esimerkki**kopioidaan tapahtuma.  Voit muuttaa korostuksen tähän Log Analytics hahmottaminen olisi tekemäsi valinnan avulla.
4.  Valitse uusi tämän toiminnon avulla voit laskea kaikki olemassa olevien tietueiden **Pura** .  Tulokset voi muokata tietueiden kuin vain muokkaamasi yksi tämän uuden liiketoimintatietojen perusteella.
5.  Jatkaa ja lisätä korjaukset, kunnes kaikki tietueet Pura tunnistaa oikein tietojen täyttämiseen uutta mukautettua kenttää.
6. Kun olet tyytyväinen, valitse **Tallenna Pura** .  Mukautettu kenttä on nyt määritetty, mutta sitä ei voi lisätä tietueisiin vielä.
7.  Odota, että uusia tietueita, jotka vastaavat määritettyjä ehtoja voit kerätä ja suorita sitten log haku uudelleen. Uusien tietueiden on mukautettu kenttä.
8.  Käytä mukautettua kenttää kuten tietueen ominaisuus.  Voit käyttää tietojen koostaminen ja ryhmän ja käyttää sitä myös luomaan uusia johtopäätöksiä.


## <a name="viewing-custom-fields"></a>Mukautettujen kenttien tarkasteleminen
Voit tarkastella kaikkia mukautettujen kenttien luettelo OMS koontinäyttö- **asetukset** -ruutu ryhmän hallinta.  Valitse **tiedot** ja **mukautettujen kenttien** luettelo kaikki mukautetut kentät työtilassa.  

![Mukautetut kentät](media/log-analytics-custom-fields/list.png)

## <a name="removing-a-custom-field"></a>Mukautetun kentän poistaminen
Voit poistaa mukautetun kentän kahdella eri tavalla.  Ensimmäinen on kunkin kentän **Poista** -vaihtoehto, kun tarkastelet luettelo edellä kuvatulla.  Toisessa menetelmässä on hakea tietueen ja vasemmalla puolella olevaa-painiketta.  Valikko on vaihtoehto Poista mukautettu kenttä.

## <a name="sample-walkthrough"></a>Esimerkki vaiheittainen kuvaus

Seuraavassa osassa käydään läpi koko Esimerkki mukautetun kentän luominen.  Tässä esimerkissä poimii Windows tapahtumia, jotka osoittavat palvelun tilan muuttaminen palvelunimi.  Tämä on riippuvainen tapahtumien palvelun ohjausobjektin hallinnan avulla luotu järjestelmän tapahtumalokiin Windows-tietokoneissa.  Jos haluat seurata tässä esimerkissä, sinun on oltava [kerätä tietoja tapahtumien järjestelmän sisäänkirjautumista varten](log-analytics-data-sources-windows-events.md).

Olemme Syötä seuraava kysely palauttaa kaikki tapahtumat palvelujen ohjauksen hallinnasta, tapahtuman tunnus on 7036 tapahtuman, joka ilmaisee palvelun aloittaminen tai lopettaminen eli.

![Kyselyn](media/log-analytics-custom-fields/query.png)

Olemme Valitse tietue tapahtuman tunnus 7036 kanssa.

![Tietuetyyppi](media/log-analytics-custom-fields/source-record.png)

Haluamme palvelunimi, joka näkyy **RenderedDescription** -ominaisuus ja valitse tämän ominaisuuden vieressä olevaa-painiketta.

![Pura kentät](media/log-analytics-custom-fields/extract-fields.png)

**Kentän poiminnan ohjattu** avataan ja **Tapahtumaloki** ja **EventID** -kentät on valittu **Main esimerkissä** sarake.  Tämä ilmaisee, että tapahtumien kanssa Tapahtumatunnus on 7036 järjestelmän lokista määritetty mukautettu kenttä.  Tämä on riittävä, joten ei tarvitse valita muita kenttiä.

![Esimerkki pää](media/log-analytics-custom-fields/main-example.png)

Olemme Korosta palvelun **RenderedDescription** -ominaisuuden nimi ja **palvelun** avulla voit tunnistaa palvelunimi.  Mukautetun kentän kutsutaan **Service_CF**.

![Kentän otsikon](media/log-analytics-custom-fields/field-title.png)

Näemme, että palvelunimi on määritetty oikein tietueita mutta ei muiden.   **Etsinnän tulokset** osoittavat **WMI suorituskyvyn sovittimen** nimeen ei ole valittuna.  **Yhteenveto** näkyy, että neljä tietuetta **DPRMA** palvelussa sisältää väärin ylimääräisiä word ja kaksi tietuetta tunnistaa **Moduulit Installer** **Windows moduulit Installer**sijaan.  

![Etsinnän tulokset](media/log-analytics-custom-fields/search-results-01.png)

Aluksi **WMI suorituskyvyn sovittimen** -tietueen kanssa.  Napsautetaan sen Muokkaa-kuvaketta ja valitse sitten **Muokkaa tätä korostus**.  

![Korosta muokkaaminen](media/log-analytics-custom-fields/modify-highlight.png)

Olemme suurentaa palautteessa sana **WMI** ja suorita Pura korostuksen.  

![Lisää Esimerkki](media/log-analytics-custom-fields/additional-example-01.png)

Näemme, että **WMI suorituskyvyn sovittimen** merkintöjä on korjattu ja Log Analytics käyttää myös tietojen korjaaminen **Windows moduulin Installer**tietueet.  Olemme näkee **Yhteenveto** -osassa, vaikka kyseistä **DPMRA** on edelleen ei tunnistaa oikein.

![Etsinnän tulokset](media/log-analytics-custom-fields/search-results-02.png)

Olemme DPMRA-palvelussa tietue vierittämällä ja noudattamalla samoja ohjeita voit korjata tietueen.

![Lisää Esimerkki](media/log-analytics-custom-fields/additional-example-02.png)

 Purkaminen suoritettaessa on näkyvissä, että kaikki Microsoftin tulokset ovat nyt oikein.

![Etsinnän tulokset](media/log-analytics-custom-fields/search-results-03.png)

Näemme, että **Service_CF** on luotu, mutta ei ole vielä lisätty tietueisiin.

![Alkuperäinen määrä](media/log-analytics-custom-fields/initial-count.png)

Hetken kuluttua niin uudet tapahtumat kerätään, näemme, että **Service_CF** -kentässä on nyt lisätään voit tallentaa vastaavia Microsoftin ehdot.

![Lopputulokset](media/log-analytics-custom-fields/final-results.png)

Käytämme voi nyt mukautettu kenttä kuten record-ominaisuus.  Esitä tämä on Luo kysely, joka ryhmittelee uusi **Service_CF** kenttä on rajattu hiusristikon sisään palveluiden on eniten aktiivisia.

![Kyselyn Ryhmittelyperuste](media/log-analytics-custom-fields/query-group.png)

## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja [lokin haut](log-analytics-log-searches.md) luonnissa kyselyjen hakuehtona mukautettuja kenttiä.
- Seurata [mukautetun lokitiedostot](log-analytics-data-sources-custom-logs.md) , jotka voit jäsentää käyttämällä mukautettuja kenttiä.
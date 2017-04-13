<properties
    pageTitle="Usean Origin resurssien jakaminen (CORS) tuki | Microsoft Azure"
    description="Opettele CORS tuki käyttöön Microsoft Azure liittyviä palveluja."
    services="storage"
    documentationCenter=".net"
    authors="cbrooks"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/07/2016"
    ms.author="cbrooks"/>

# <a name="cross-origin-resource-sharing-cors-support-for-the-azure-storage-services"></a>Usean Origin resurssien jakaminen Azure liittyviä palveluja (CORS) tuki

Azure liittyviä palveluja tuki alkavat versio 2013-08-15 Cross Origin resurssien jakaminen (CORS) Blob, taulukko tai jonon tiedoston palvelut. CORS on HTTP-ominaisuus, joka mahdollistaa resursseihin toisen toimialueen käynnissä yhdessä toimialueen verkkosovelluksen. Selaimet toteuttaa [saman origin käytännön](http://www.w3.org/Security/wiki/Same_Origin_Policy) , joka estää verkkosivun puheluja ohjelmointirajapinnan eri toimialueen; nimeltään suojauksen rajoitus CORS tarjoaa suojatun Soita ohjelmointirajapinnan toisen toimialueen yhtä toimialuetta (origin toimialue). Katso lisätietoja [CORS määrityksen](http://www.w3.org/TR/cors/) CORS.

Voit määrittää CORS säännöt yksitellen kullekin tallennustilan palveluja, soittamalla [Blob palvelun ominaisuuksien määrittäminen](https://msdn.microsoft.com/library/hh452235.aspx) [Jonossa palvelun ominaisuuksien määrittäminen](https://msdn.microsoft.com/library/hh452232.aspx)ja [Taulukon palvelun ominaisuuksien määrittäminen](https://msdn.microsoft.com/library/hh452240.aspx). Kun määrität palvelun CORS säännöt, tehdyn vastaan toimialueesta oikein todennetut pyyntö lasketaan määrittääksesi, onko se sallittu on määritetty sääntöjen mukaisesti.

>[AZURE.NOTE] Huomaa, että CORS ei ole todennus-järjestelmä. Pyynnön vastaan tallennustilan resurssin CORS ollessa käytössä on oltava joko ERISNIMI todennus allekirjoitus tai on tehtävä julkisen resurssin vastaan.

## <a name="understanding-cors-requests"></a>Tietoja CORS pyynnöt

CORS pyyntö origin toimialueen voivat koostua kaksi erillistä pyynnöt:

- Kuvatiedostomuodot pyyntö, jonka kyselyt-palvelun CORS rajoitusten. Kuvatiedostomuodot pyynnön tarvitaan, paitsi pyyntömenetelmä on [Yksinkertainen tapa](http://www.w3.org/TR/cors/), eli GET, HEAD tai POST.

- Todellinen pyyntö, vastaan haluamasi resurssi.

### <a name="preflight-request"></a>Kuvatiedostomuodot pyyntö

Kuvatiedostomuodot pyynnön kyselyt CORS rajoitukset, jotka on osoitettu tallennustilan-palvelun tilin omistajan. Web-selaimessa (tai muita käyttäjäagentti) lähettää asetukset-pyynnön, joka sisältää pyynnön ylä- ja menetelmä ja alkuperän toimialueen. Tallennustilan palvelun arvioi haluamasi toiminto CORS säännöt, jotka määrittävät, mitä origin toimialueet, pyyntö menetelmistä ja pyynnön otsikot voidaan määrittää Todellinen pyydettäessä vastaan tallennustilan resurssin esimääritettyjä joukon perusteella.

Jos sinulla on CORS sääntö, joka täsmää kuvatiedostomuodot pyynnön CORS on käytössä-palvelun, palvelu vastaa tilakoodin 200 (OK) ja sisältää tarvittavat käyttöoikeuksien valvonta otsikot vastaus.

Jos CORS palvelu ei ole käytössä tai ei ole CORS sääntö vastaa kuvatiedostomuodot pyynnön, palvelun vastaamalla tilakoodin 403 (kielletty).

Jos asetukset-pyyntö ei sisällä CORS tarvittavat otsikot (alkuperän ja Access-ohjausobjekti-pyynnön-menetelmä otsikot)-palvelun vastataan tilakoodi 400 (pyyntö ei kelpaa).

Huomaa, että kuvatiedostomuodot pyyntö arvioidaan vastaan (Blob, jonossa ja taulukon) ja ei pyydetty resurssi. Tilin omistajan on otettava käyttöön CORS tilin palvelun ominaisuuksia, jotta pyyntö onnistuu osana.

### <a name="actual-request"></a>Todellinen pyyntö

Kun hyväksytään kuvatiedostomuodot pyynnön ja vastauksen palautetaan, selaimen lähetystä vastaan varatun tallennustilan todellinen pyynnön. Selaimen estää todellisen pyytää heti, jos kuvatiedostomuodot pyyntö on hylätty.

Todellinen pyyntö käsitellään Normaali pyynnön vastaan tallennustilan. Tavoitettavuustietojen Origin otsikon ilmaisee, että pyyntö on CORS pyyntö ja palvelun tarkistaa CORS sääntöjä. Jos vastine löytyy, Access-komponenttien otsikot lisätään vastauksen ja lähetetään asiakkaalle. Jos vastinetta ei löydy, ei CORS Access-komponenttien otsikot palautetaan.

## <a name="enabling-cors-for-the-azure-storage-services"></a>CORS ottaminen käyttöön Azuren tallennustilaan-palvelut

CORS säännöt määritetään palvelun tasolla, joten ottaminen käyttöön ja poistaminen CORS kunkin palvelun (Blob, jonossa ja taulukon) erikseen. Oletusarvon mukaan CORS ei ole käytettävissä kunkin palvelun. Jotta CORS on määritettävä versio 2013-08-15 haluamasi palveluominaisuudet tai uudempi versio, ja lisää CORS sääntöjä palvelun ominaisuuksia. Lisätietoja ottaminen käyttöön tai poistaa käytöstä CORS palvelun ja miten voit määrittää CORS sääntöjä, lue lisätietoja [Blob palvelun ominaisuuksien määrittäminen](https://msdn.microsoft.com/library/hh452235.aspx) [Jonossa palvelun ominaisuuksien määrittäminen](https://msdn.microsoft.com/library/hh452232.aspx)ja [Taulukon palvelun ominaisuuksien määrittäminen](https://msdn.microsoft.com/library/hh452240.aspx).

Tässä on esimerkki yhden CORS-säännön, määritetty palvelun ominaisuuksien määrittäminen-toiminnon kautta:

    <Cors>    
        <CorsRule>
            <AllowedOrigins>http://www.contoso.com, http://www.fabrikam.com</AllowedOrigins>
            <AllowedMethods>PUT,GET</AllowedMethods>
            <AllowedHeaders>x-ms-meta-data*,x-ms-meta-target*,x-ms-meta-abc</AllowedHeaders>
            <ExposedHeaders>x-ms-meta-*</ExposedHeaders>
            <MaxAgeInSeconds>200</MaxAgeInSeconds>
        </CorsRule>
    <Cors>

Muut elementit CORS sääntöön on seuraavalla tavalla:

- **AllowedOrigins**: origin toimialueet, jotka voivat pyydettävä vastaan tallennuspalvelu CORS kautta. Origin toimialue on toimialue, josta pyyntö on peräisin. Huomaa, että alkuperän on oltava tarkka kirjainkoko alkuperän, joka lähettää käyttäjän ikä palvelun kanssa. Voit myös käyttää yleismerkki "*", jotta kaikki origin toimialueiden pyytää CORS kautta. Yllä olevassa esimerkissä toimialueiden [http://www.contoso.com](http://www.contoso.com) ja [http://www.fabrikam.com](http://www.fabrikam.com) voit tehdä pyynnöt CORS-palveluun.

- **AllowedMethods**: menetelmiä (HTTP pyynnön verbien), origin toimialueen voi käyttää CORS pyynnön. Yllä olevassa esimerkissä valitseminen vain HYLLYTETTY ja GET-pyynnöt.

- **AllowedHeaders**: pyynnön otsikot, jotka origin toimialueen voi määrittää CORS pyynnön. Yllä olevassa esimerkissä kaikki metatietojen otsikot x-ms-metatiedon, x-ms-metatiedot-kohde ja x-ms-metatiedot-abc alkaen on sallittua. Huomaa, että yleismerkki "*" ilmaisee, että kaikki otsikon alkaa sanalla määritettyä etuliitettä on sallittua.

- **ExposedHeaders**: vastauksen otsikot, joka lähettää vastauksen CORS pyynnön ja näyttämiä selaimen pyynnön myöntäjä. Yllä olevassa esimerkissä selaimen kehotetaan näyttää kaikki otsikon alkaa sanalla x-ms-metatiedot.

- **MaxAgeInSeconds**: selaimessa olisi välimuistiin kuvatiedostomuodot asetukset mahdollisimman suureksi aika pyytää.

Azure-tallennustilan palvelut tukea **AllowedHeaders** ja **ExposedHeaders** elementtien etuliite otsikot. Anna luokan otsikon, voit määrittää yleisiä etuliite luokkaan. Määrittämällä esimerkiksi *x-ms-metatiedot** kuin prefixed ylätunnisteen Luo sääntö, joka vastaa kaikki otsikot, jotka alkavat x-ms-metatiedot.

Seuraavat rajoitukset koskevat CORS säännöt:

- Voit määrittää enintään viiteen CORS säännöt kohti tallennuspalvelu (Blob, taulukon ja jonon).
- Kaikkien CORS sääntöjen asetusten pyynnön XML-tunnisteita, lukuun ottamatta enimmäiskokoa enintään 2 Kilotavua.
- Aiemmin sallittu ylä-, näkyviä ylä- tai sallittujen origin pituuden enintään 256 merkkiä.
- Sallitun ylä- ja näkyviä otsikot voivat olla:
  - Literal otsikot, jossa tarkan otsikon nimeä anneta, kuten **x-ms-metatiedot-käsitteleminen**. Enintään 64 literal otsikot voidaan määrittää pyynnön.
  - Etuliite otsikot, jossa otsikon etuliite on annettu, kuten **x-ms-metatiedon***. Etuliitteen, joka määrittää tällä tavalla sallii tai paljastaa otsikko, joka alkaa tietyllä etuliitteellä. Kaksi etuliite otsikot enintään voidaan määrittää pyynnön.
- Menetelmiä (tai HTTP verbien), määritetty **AllowedMethods** -elementti on oltava Azure tallennuspalvelu API tukemat tavoista. Tuetut tavat ovat Poista, HAE, PÄÄTÄ, Yhdistä, viestin, asetukset ja HYLLYTETTY.

## <a name="understanding-cors-rule-evaluation-logic"></a>Tietoja CORS säännön arvioinnin logiikka

Tallennustilan palvelu saa kuvatiedostomuodot tai todellisiin pyynnön, kun olet muodostanut-palvelun kautta haluamasi palvelun ominaisuuksien määrittäminen toiminto CORS sääntöjen perusteella pyynnön sana. Arvioidaan CORS säännöt siinä järjestyksessä, jossa ne on asetettu pyynnön tekstiosaan palvelun ominaisuuksien määrittäminen-toiminto.

CORS säännöt käsitellään seuraavasti:

1. Ensin origin toimialueen pyynnön tarkistetaan luettelossa **AllowedOrigins** elementin toimialueet. Jos origin toimialueen sisältyy luettelossa tai kaikkien toimialueiden sallitaan yleismerkkiä "*"-säännöt arvioinnin tuotot. Jos origin toimialueen ei sisällytetä, kutsu epäonnistuu.

2. Seuraavaksi pyynnön menetelmä (tai HTTP verbin) tarkistetaan menetelmiä **AllowedMethods** -osaan. Jos menetelmä luettelossa, etenee säännöt arviointi; Jos et valitse kutsu epäonnistuu.

3. Jos pyynnön vastaa origin sen toimialueen ja sen menetelmä säännöt, pyynnön ja muita sääntöjä ei arvioidaan prosessin sääntöön on valittuna. Ennen pyynnön voi onnistua, kuitenkin minkä tahansa määritetyn pyynnön otsikot tarkistetaan luettelossa **AllowedHeaders** osaan otsikot. Jos lähetetään otsikot eivät vastaa sallittu otsikot, kutsu epäonnistuu.

Koska säännöt käsitellään siinä järjestyksessä, ne ovat näkyvissä pyynnön tekstissä, parhaat käytännöt suositeltavaa, että määrität alkuperistä eniten rajoittava säännöissä ensin luettelosta niin, että ne ovat ensimmäisinä. Määritä säännöt, jotka ovat joustavampi – esimerkiksi sääntö, joka sallii kaikkien alkuperistä – luettelon lopussa.

### <a name="example--cors-rules-evaluation"></a>Esimerkki – CORS säännöt arviointi

Seuraavassa esimerkissä esitetään osittaisen pyynnön tekstissä, toiminnon CORS sääntöjen liittyviä palveluja. Lue [Blob palvelun ominaisuuksien määrittäminen](https://msdn.microsoft.com/library/hh452235.aspx), [Jonossa palvelun ominaisuuksien määrittäminen](https://msdn.microsoft.com/library/hh452232.aspx)ja [Taulukon palvelun ominaisuuksien määrittäminen](https://msdn.microsoft.com/library/hh452240.aspx) lisätietoja luomisesta pyynnön.

    <Cors>
        <CorsRule>
            <AllowedOrigins>http://www.contoso.com</AllowedOrigins>
            <AllowedMethods>PUT,HEAD</AllowedMethods>
            <MaxAgeInSeconds>5</MaxAgeInSeconds>
            <ExposedHeaders>x-ms-*</ExposedHeaders>
            <AllowedHeaders>x-ms-blob-content-type, x-ms-blob-content-disposition</AllowedHeaders>
        </CorsRule>
        <CorsRule>
            <AllowedOrigins>*</AllowedOrigins>
            <AllowedMethods>PUT,GET</AllowedMethods>
            <MaxAgeInSeconds>5</MaxAgeInSeconds>
            <ExposedHeaders>x-ms-*</ExposedHeaders>
            <AllowedHeaders>x-ms-blob-content-type, x-ms-blob-content-disposition</AllowedHeaders>
        </CorsRule>
        <CorsRule>
            <AllowedOrigins>http://www.contoso.com</AllowedOrigins>
            <AllowedMethods>GET</AllowedMethods>
            <MaxAgeInSeconds>5</MaxAgeInSeconds>
            <ExposedHeaders>x-ms-*</ExposedHeaders>
            <AllowedHeaders>x-ms-client-request-id</AllowedHeaders>
        </CorsRule>
    </Cors>

Seuraavaksi Ota huomioon seuraavat CORS pyynnöt:

Pyyntö||| Vastaus||
---|---|---|---|---
**Menetelmä** |**Origin** |**Pyydä otsikot** |**Säännön vastaavuus** |**Tulos**
**VALITSEMINEN** | http://www.contoso.com |x-ms-blob-sisältö-tyyppi | Ensimmäistä sääntöä |Onnistui
**HAE** | http://www.contoso.com| x-ms-blob-sisältö-tyyppi | Toisen säännön |Onnistui
**HAE** | http://www.contoso.com| x-ms-blob-sisältö-tyyppi | Toisen säännön | Virhe

Ensimmäinen pyyntö vastaa ensimmäistä sääntöä – origin toimialueen vastaa sallitun alkuperistä, menetelmä vastaa sallitut menetelmät ja otsikon vastaa sallittu ylä – ja niin onnistuu.

Toisen pyynnön ensimmäistä sääntöä ei vastaa, koska menetelmä ei vastaa sallitut menetelmät. Se, mutta vastaa toisen säännön, joten se onnistuu.

Kolmas pyynnön vastaa toisen säännön origin toimialueen ja tapa, jotta arvioidaan edelleen sääntöjä ei. Kuitenkin *x-ms-asiakas-pyynnön-tunnuksen otsikkoa* ei sallita toisen säännön perusteella niin kutsu epäonnistuu, huolimatta, että semantiikkaan liittyvien kolmannen säännön on sallittu se onnistuu.

>[AZURE.NOTE] Tässä esimerkissä näytetään joustavampi säännön ennen rajoittavampi yksi, vaikka paras käytäntö on eniten rajoittava säännöt ensimmäiseksi yleinen.

## <a name="understanding-how-the-vary-header-is-set"></a>Muuta otsikon asetusten ymmärtäminen

*Muuta* otsikko on vakio HTTP/1.1-otsikon pyynnön otsikkokentät, jotka neuvoa ehdot, jotka on valittu palvelin pyynnön tietoja selaimessa tai käyttäjän agentti joukko koostuu. *Muuta* otsikon käytetään pääasiassa välityspalvelimet, selaimet ja Sisältöverkot, jonka avulla voit selvittää, kuinka vastaus välimuistiin tallentaminen välimuistiin. Lisätietoja on artikkelissa määrityksen [Muuta otsikko](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).

Kun selain tai toisen käyttäjäagentti välimuistiin CORS pyyntö vastausta, origin toimialue on välimuistin sallittu alkuperän. Kun toinen toimialue tallennustilan resurssin pyynnössä välimuistin ollessa aktiivinen, käyttäjäagentin hakee välimuistissa origin toimialueen. Toisen toimialueen vastaa välimuistissa toimialueen, joten pyyntö epäonnistui, kun se muuten toiminto onnistuu. Joissakin tapauksissa Azuren tallennustilaan asettaa muuta otsikon **Origin** siten, että käyttäjäagentin lähettämään myöhemmin CORS pyynnön-palveluun, kun pyytävä toimialueen eroaa välimuistissa alkuperän.

Azure-tallennustilan asettaa *Muuta* otsikon **Origin** todellinen GET/HEAD-pyyntöjen seuraavissa tapauksissa:

- Kun pyynnön alkuperän vastaa täsmälleen CORS säännön määrittämiä sallittu alkuperän. Jos tarkkaa vastinetta, CORS sääntö voi sisältää yleismerkkiä "*" merkki.

- Ei mitään sääntöä vastaavat pyynnön alkuperän, mutta CORS on otettu käyttöön tallennuspalvelu.

HAE/HEAD pyynnön, jossa vastaa CORS sääntö, joka sallii kaikkien alkuperistä tapauksessa vastaus näkyy, että kaikki alkuperistä sallitaan ja käyttäjän agentti välimuistin mahdollistaa seuraavat pyynnöt mistä tahansa origin välimuistin ollessa aktiivinen.

Huomaa, että pyynnöt usealla tavalla kuin GET/pää-tallennustilan palvelut ei aseta muuta otsikon, koska näistä tavoista vastauksia ei tallenneta välimuistiin käyttäjäagenttien mukaan.

Seuraavassa taulukossa esitetään, miten Azure tallennustilan reagoi GET/HEAD-pyyntöjen perusteella mainittiin tapauksissa:

Pyyntö|Tilin asetukset ja käsittelemisen tulos|||Vastaus|||
---|---|---|---|---|---|---|---|---
**Esitä pyyntö origin otsikko** | **CORS sääntöjen määritetty tämä palvelu** | **On Matching sääntö, jonka avulla kaikki alkuperistä (*)** | **Vastaavat sääntö on luotu origin tarkka vastaavuus** | **Vastauksen sisältää muuta otsikon Origin asettaminen** | **Vastaus sisältää Access-ohjausobjekti-sallittu-Origin: "*"** | **Vastauksen sisältää Access-ohjausobjekti-tarjoamia-otsikot**
Ei|Ei|Ei|Ei|Ei|Ei|Ei
Ei|Kyllä|Ei|Ei|Kyllä|Ei|Ei
Ei|Kyllä|Kyllä|Ei|Ei|Kyllä|Kyllä
Kyllä|Ei|Ei|Ei|Ei|Ei|Ei
Kyllä|Kyllä|Ei|Kyllä|Kyllä|Ei|Kyllä
Kyllä|Kyllä|Ei|Ei|Kyllä|Ei|Ei
Kyllä|Kyllä|Kyllä|Ei|Ei|Kyllä|Kyllä


## <a name="billing-for-cors-requests"></a>Laskutuksen CORS pyynnöt

Onnistuneiden kuvatiedostomuodot pyyntöjen ovat laskuttaa, jos olet ottanut CORS tilin tallennustilan-palveluja varten (soittamalla [Blob palvelun ominaisuuksien määrittäminen](https://msdn.microsoft.com/library/hh452235.aspx), [Jonossa palvelun ominaisuuksien määrittäminen](https://msdn.microsoft.com/library/hh452232.aspx)tai [Taulukon palvelun ominaisuuksien määrittäminen](https://msdn.microsoft.com/library/hh452240.aspx)). Pienennä kulujen, harkitse määrittäminen suureksi CORS sääntöjen **MaxAgeInSeconds** -elementin, niin, että käyttäjäagentin välimuistiin pyynnön.

Ei onnistu kuvatiedostomuodot pyynnöt laskuteta.

## <a name="next-steps"></a>Seuraavat vaiheet

[Blob-palvelun ominaisuuksien määrittäminen](https://msdn.microsoft.com/library/hh452235.aspx)

[Jonon palvelun ominaisuuksien määrittäminen](https://msdn.microsoft.com/library/hh452232.aspx)

[Taulukon palvelun ominaisuuksien määrittäminen](https://msdn.microsoft.com/library/hh452240.aspx)

[W3C rajat Origin resurssien jakaminen määritys](http://www.w3.org/TR/cors/)

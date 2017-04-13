<properties
   pageTitle="Esittely palvelun kangasta klusterin Resurssienhallinta | Microsoft Azure"
   description="Esittely, palvelun kangasta klusterin Resurssienhallinta."
   services="service-fabric"
   documentationCenter=".net"
   authors="masnider"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/19/2016"
   ms.author="masnider"/>

# <a name="introducing-the-service-fabric-cluster-resource-manager"></a>Palvelun kangasta klusterin Resurssienhallinta esittely
Hallinta perinteisesti IT-järjestelmistä tai joukko palveluita tarkoitetaan tuottaa muutaman fyysisiä tai omistautunut näiden palveluissa tai järjestelmien näennäiskoneiden. Monia pää palveluita on eritellä taso "WWW" ja "tiedot" tai "tallennustilan" tason ehkä muita muutaman erityinen osia kuten välimuistin. Muun tyyppisiä sovelluksia on tekstiviesti taso, jossa pyynnöt ohjattuja sisään ja ulos työn taso analyysi-ja muunnos messaging osana tarvittavat yhteydessä. Kullakin virhelajilla on työmäärää jonkin tietyn koneet, sille: tietokannan käytössä muutama koneet omistautunut, joka vähän verkko-palvelimiin. Jos työmäärää tietyn tyyppisiä koneet se oli suorittaa liian kuuma ja valitse Lisää koneet lisätty määritetty suorittamaan sitä työmäärää tyypin tai korvaa joidenkin koneet suurempi koneet. Helposti. Jos koneen epäonnistui, sovelluksen yleistä osa suoritettiin alemman kapasiteetin kunnes tietokoneen voi palauttaa. Edelleen melko helppoa (Jos ei välttämättä ole hauska).

Nyt kuitenkin japanin vaikkapa olet löytänyt tarvitse skaalata ulos ja ovat tulleet säilöjä ja/tai microservice plunge. Yhtäkkiä tiedä, kymmenlukuun, sataan tai tuhansia laitteiden kymmeniä erityyppisten palveluiden, esimerkiksi satoja eri esiintymien näistä palveluista, joissa esiintymiä tai replikoita, suuri käytettävyys (HA).

Hallinta yhtäkkiä ympäristön ei ole näin helppoa kuin muutaman koneet omistautunut työmääriä yksi tyypit hallinta. Palvelinten ovat virtual ja ei enää ole nimet (sinun *on* vaihdettu mindsets- [lemmikkieläimet naudat](http://www.slideshare.net/randybias/architectures-for-open-and-scalable-clouds/20) jälkeen). Määritys on pienempi koneet ja lisätietoja services itse. Erillinen laite on asian määrää aiemman ja palveluja on tullut pieni hajautettu järjestelmien kestävät tärkeämpää laitteiston useita pienempi laitteita.

Seurauksena purkaa aiemmin Monoliittiset, porrastettu sovelluksen erillisessä tärkeämpää laitteiston palveluita kyselyjä, käytössä on nyt käsitellä useita Lisää yhdistelmät. Kuka päättää, mitä työmääriä tyypit voidaan suorittaa mitkä laitteet, tai kuinka monta? Mitä toiminnoista toimii hyvin samassa laitteessa ja jotka ristiriidassa? Kun kone siirtyy... Mikä on käynnissä on? Kuka on vastuussa varmistetaan, että kyseinen työmäärää käynnistyy uudelleen? Voit palata (virtual?) koneen Odota tai oman työmääriä automaattisesti ei onnistu päälle muiden laitteiden ja Säilytä käynnissä? Onko ihmisten toimia tarvitaan? Mitä tietoja ympäristön tällaiset päivitykset?

Kehittäjille ja operaattorit maailman tällaiset luonnonvaraisena tutustutaan Tarvitsetko apua tämän monimutkaisuus hallinta ja saat siten, että työntekijän valintaa binge ja yrität arkki päälle monimutkaisuus henkilöiden kanssa ei ole oikea vastaus.

Mitä voi tehdä?

## <a name="introducing-orchestrators"></a>Orchestrators esittely
"Orchestrator" on Yleinen termi osan mukaan ohjelma, jonka avulla järjestelmänvalvojat ympäristöt seuraavanlaisiin hallinta. Orchestrators ovat osat, jotka vievät pyynnöt, kuten "Voin kuten Oma ympäristössä palvelua 5 kopioita", tee arvo on TOSI, ja valitse sitten (yrittää), jos haluat säilyttää sen mukaan.

Orchestrators (ei ihmisiin) ovat mitä iskettävä käyttöön koneen epäonnistuu tai työmäärä lopettaa odottamattomia jostakin syystä. Useimmat Orchestrators pelkästään käsitellä virhe, kuten tukea kanssa uusi käyttöönotoissa, käsittely päivitykset ja resurssin kulutus käsittely, mutta kaikki ovat olennaisesti ylläpitoon joitakin toivottuja tilan määritys-ympäristössä. Haluat, että voit kertoa Orchestrator, mitä haluat ja sen Tee näkyvä nosto. Chronos tai Marathon päälle Mesos, laivaston, Swarm, Kubernetes ja palvelun kangasta Orchestrators kaikki esimerkkejä (tai niiden hallinta). Lisää luodaan hallintaa käytännön ominaisuuksissa erilaisissa ympäristöissä monimutkaisia aikaan ja ehdot suurennus ja muuttaminen.

## <a name="orchestration-as-a-service"></a>Tiedonsiirron palveluna
Palvelun kangasta klusterin sisällä Orchestrator työn käsitellään pääasiassa mukaan klusterin Resurssienhallinta. Palvelun kangasta klusterin Resurssienhallinta on yksi palvelun kangasta järjestelmän palveluihin ja automaattisesti aloitetaan kunkin klusterin kuluessa.  Yleensä klusterin Resurssienhallinta työ on jaettu kolme osaa:

1. Pakotetut säännöt
2. Ympäristön optimointi
3. Muita prosesseja avustaminen

### <a name="what-it-isnt"></a>Mitä siihen ei kuulu
Perinteinen N taso web-sovellusten on aina joitakin käsite "kuormituksen", yleensä kutsutaan verkon kuormituksen tasauspalvelun (NLB) tai -sovelluksen kuormituksen tasauspalvelun (ALB) sen mukaan, missä se sat verkko pinossa. Jotkin kuormituksen tasoitusmääritykset ovat esimerkiksi F5's BigIP tarjoaa laitteista, muiden mukaan esimerkiksi Microsoftin ohjelmistojen NLB. Muiden ympäristöjen suunnilleen HAProxy saatat nähdä tämän roolin. Nämä arkkitehtuureihin, kuormituksen tasaamisen työ on varmistaa, että kaikki eri tilattomien edusta-laitteiden tai klusterin eri tietokoneissa saat (noin) työn sama määrä. Strategioita tämä lähettämästä jokainen eri kutsu toinen palvelin istunnon kiinnittäminen/tahmeutta, todellinen arvio ja puhelun kohdistus oletettu kustannus-ja nykyisen tietokoneen lataamisen perusteella haluat muuttaa.

Huomaa, että tämä on parhaalla järjestelmä varmistamisesta, jotka web-tason olleet suurin piirtein saapuva. Strategioita tasaamisen tietojen taso on täysin eri ja riippuvaisia tietojen tallennustilan, Keskitys yleensä tietojen sharding ympärille, tallentamisesta välimuistiin, tietokannan hallitut näkymät ja tallennettujen toimintosarjojen jne.

Joitain näistä strategioita on mielenkiintoista, palvelun kangasta klusterin Resurssienhallinta ei mitään kuten verkon kuormituksen tai välimuistiin. Verkon ladata tasaustoiminto varmistaa, että etu päät ovat saapuva siirtämällä liikenne missä palveluja on käytössä, kun palvelua kangasta klusterin Resurssienhallinta on täysin eri strategia – olennaisesti, palvelun kangasta siirtää *palveluita* , jossa hän katsoo sen perustelluksi eniten (ja odottaa liikenne tai lataamiseen noudattamalla). Tämä voi olla esimerkiksi solmut, jotka ovat tällä hetkellä kylmän, koska palveluja, joita ei ole edistymisestä on paljon tällä hetkellä ja jotka on poistettu tai siirtää toisaalla. Toinen esimerkki klusterin Resurssienhallinta voitu siirtää palvelu poissa tietokoneen, joka on päivitettävä tai jotka ylikuormitetaan vuoksi kulutus Piikkiin Services, joka on käytössä. Klusterin Resurssienhallinta on vastuussa siirtäminen services ympärille (ei välittää verkkoliikennettä missä palvelut ovat jo), koska se sisältää merkittävästi eri ominaisuusjoukko, mitä etsiä verkon kuormituksen verrattuna ja suojattu olennaisesti erilaisia strategioita varmistaa, että klusterin laitteisto-resurssit ovat käytössä hyvin.

## <a name="next-steps"></a>Seuraavat vaiheet
- Lisätietoja arkkitehtuuri ja tietojen kulun sisällä klusterin Resurssienhallinta Tutustu [tämän artikkelin](service-fabric-cluster-resource-manager-architecture.md)
- Klusterin Resurssienhallinta on paljon kuvaava klusterin asetukset. Selvitä lisätietoja niitä Katso tässä artikkelissa [kerrotaan palvelun kangasta klusterin](service-fabric-cluster-resource-manager-cluster-description.md)
- Jos haluat lisätietoja muiden määrittämiseen käytettävissä olevat asetukset services uloskuittaus klusterin Resurssienhallinta-määrityksiä aiheen käytettävissä [tietoja palveluiden määrittäminen](service-fabric-cluster-resource-manager-configure-services.md)
- Arvot ovat, miten palvelun kangasta klusterin resurssin Manager hallitsee kulutus ja kapasiteettiin klusterin. Saat lisätietoja niitä ja miten ne määritetään Tutustu [tämän artikkelin](service-fabric-cluster-resource-manager-metrics.md)
- Klusterin Resurssienhallinta toimii palvelun kangasta hallintatoiminnot. [Jos haluat lisätietoja, integrointi artikkeli](service-fabric-cluster-resource-manager-management-integration.md)
- Saat lisätietoja siitä, miten klusterin Resurssienhallinta hallitsee ja jakaa kuormituksen klusterin, katso artikkeli [kuormituksen tasaamisen](service-fabric-cluster-resource-manager-balancing.md)

<properties
    pageTitle="Analysoi Azure CDN käyttötavat | Microsoft Azure"
    description="Voit tarkastella käyttötavat oman CDN käyttämällä seuraavia raportteja: kaistanleveyden siirtää tiedot osumaa, välimuistin tilat, välimuistin osumien suhde, siirtää IPV4/IPV6-tiedot."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>

# <a name="analyze-azure-cdn-usage-patterns"></a>Analysoi Azure CDN käyttötavat

[AZURE.INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

Voit tarkastella käyttötavat oman CDN käyttämällä seuraavia raportteja:

- Kaistanleveyden
- Tiedonsiirron
- Osumia
- Välimuistin tilat
- Välimuistin osumien prosenttiosuus
- Tiedonsiirron IPv4 ja IPV6

## <a name="accessing-advanced-http-reports"></a>Lisäasetukset HTTP-raporttien käyttäminen

1. Valitse **hallinta** -painike CDN profiili-sivu.

    ![CDN profiilin sivu hallinta-painike](./media/cdn-reports/cdn-manage-btn.png)

    CDN hallinta-portaalin avautuu.

2. Vie hiiren osoitin **Analytics** -välilehti ja valitse Vie hiiren osoitin **Core raportit** -valikon avauspainike.  Valitse valikosta haluamasi raportti.

    ![CDN hallinta-portaalin - Core Raportit-valikko](./media/cdn-reports/cdn-core-reports.png)


## <a name="bandwidth"></a>Kaistanleveyden

Kaistanleveyden raportti koostuu kaavion ja tietojen taulukon osoittaa kaistanleveyden käytön HTTP-ja HTTPS­yhteydet tietyn ajan kuluessa. Voit tarkastella kaistanleveyden käytön kaikki CDN tulee tai tietyn POP yli. Voit tarkastella liikenne piikkarit ja jakelu yli CDN tulee Mbps.

- Valitse kaikki reunan solmut haluat nähdä kaikki solmut tietoliikenteen tai valita tietyn alueen/solmun avattavasta luettelosta.
- Valitse päivämääräalue, tarkastella tietoja tänään tällä viikolla/tässä kuussa yms tai mukautetut päivämäärät ja valitse sitten "Siirry" varmistaaksesi, että valintasi päivitetään.
- Voit viedä ja lataa tiedot sijaitsevat "Siirry" vieressä excel-taulukon kuvaketta napsauttamalla.

Raportti päivitetään 5 minuutin välein.

![Kaistanleveyden raportti](./media/cdn-reports/cdn-bandwidth.png)

## <a name="data-transferred"></a>Tiedonsiirron

Tämä raportti koostuu kaavion ja tietojen taulukon osoittaa liikenne käyttö HTTP-ja HTTPS­yhteydet tietyn ajan kuluessa. Voit tarkastella liikenne käyttö kaikki CDN tulee tai tietyn POP yli. Voit tarkastella liikenne piikkarit ja jakelu yli CDN tulee Gigatavua.

- Valitse kaikki reunan solmut haluat nähdä kaikki muistiinpanot tietoliikenteen tai valita tietyn alueen/solmun avattavasta luettelosta.
- Valitse päivämääräalue, tarkastella tietoja tänään tällä viikolla/tässä kuussa jne tai mukautetut päivämäärät ja valitse sitten "Siirry" varmistaaksesi, että valintasi päivitetään.
- Voit viedä ja lataa tiedot sijaitsevat "Siirry" vieressä excel-taulukon kuvaketta napsauttamalla.

Raportti päivitetään 5 minuutin välein.

![Siirtää raportin tiedot](./media/cdn-reports/cdn-data-transferred.png)

## <a name="hits-status-codes"></a>Osumia (tilakoodin)

Tämän raportin kuvaus pyynnön tilakoodin sisällölle jakautumisen. Jokainen pyyntö sisällön Luo HTTP-tilakoodin. Tilakoodin kuvataan käsittelyn reunan ponnahtaa pyynnön. Esimerkiksi 2xx tilakoodin osoittavat, että pyyntö on onnistuneesti served asiakkaalle, kun 4xx tilakoodin ilmaisee virhe. Saat lisätietoja HTTP-tilakoodin [tilakoodin](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes).

- Valitse päivämääräalue, tarkastella tietoja tänään tällä viikolla/tässä kuussa jne tai mukautetut päivämäärät ja valitse sitten "Siirry" varmistaaksesi, että valintasi päivitetään.
- Voit viedä ja lataa tiedot valitsemalla excel-laskentataulukko, joka sijaitsee "Siirry"-kohdan vieressä.

![Osumia raportti](./media/cdn-reports/cdn-hits.png)

## <a name="cache-statuses"></a>Välimuistin tilat

Tämä raportti kuvataan jakautumisen osumia ja välimuistin epäonnistumisia asiakkaan pyynnön. Koska nopein suorituskyky peräisin osumia, voit optimoida tietojen toimituksen nopeuksia pienentäminen välimuistin epäonnistumisten ja vanhentuneet osumia. Välimuistin epäonnistumisia voi pienentää määrittämällä origin palvelimen välttämiseksi määrittäminen "no-cache" vastausten otsikot, välttäminen kyselymerkkijonon välimuistin lukuun ottamatta ehdottoman tarvittaessa ja välttäminen ei välimuistiin tallennettava vastauksen koodit. Vanhentuneen välimuistin osumia voidaan välttää tekemällä sijoituksen on max-ikä niin kauan mahdollisimman vähän pyyntöjen origin palvelimeen.

![Välimuistin tilat-raportti](./media/cdn-reports/cdn-cache-statuses.png)

### <a name="main-cache-statuses-include"></a>Tärkeimmät välimuistin tilat ovat seuraavat:

- TCP_HIT: Served reunasta. Objekti on välimuistin ja ylitti ei sen max-ikä.
- TCP_MISS: Served lähettäjältä. Objekti ei ole välimuistin ja vastaus oli takaisin alkuperän.
- TCP_EXPIRED _MISS: Served lähettäjältä jälkeen uudelleenvahvistusta origin kanssa. Objekti on välimuistissa, mutta ylitti sen max-ikä. Uudelleenvahvistusta origin ja tuloksena on korvattu origin uusi vastausta välimuisti-objekti.
- TCP_EXPIRED _HIT: Served reunasta jälkeen uudelleenvahvistusta origin kanssa. Objekti on välimuistissa, mutta ylitti sen max-ikä. Uudelleenvahvistusta origin palvelimen kanssa tuloksena on muuttamattomia välimuisti-objekti.

- Valitse päivämääräalue, tarkastella tietoja tänään tällä viikolla/tässä kuussa jne tai mukautetut päivämäärät ja valitse sitten "Siirry" varmistaaksesi, että valintasi päivitetään.
- Voit viedä ja lataa tiedot sijaitsevat "Siirry" vieressä excel-taulukon kuvaketta napsauttamalla.

### <a name="full-list-of-cache-statuses"></a>Täydellinen luettelo välimuistin tilat

- TCP_HIT – tämä tila ilmoitetaan, kun pyyntö served suoraan POP asiakkaalle. Sijoituksen heti served välimuistiin lähinnä asiakkaan POP ja siinä on kelvollinen, aika-to-live POP-tai TTL. TTL määritetään vastausten-otsikot mukaan:

    - Välimuistin-komponenttien: s-maxage
    - Välimuistin-komponenttien: max-ikä
    - Vanhentuu

- TCP_MISS – tämä tila tarkoittaa, että pyydetty kohteen välimuistiin tallennettu versio ei löytynyt lähinnä asiakkaan POP. Kohteen pyydetään origin-palvelimeen tai alkuperäisestä-shield palvelimesta. Jos alkuperäisestä palvelimesta tai origin shield palvelimen Palauttaa sijoituksen, se asiakkaalle served ja välimuistiin asiakkaan ja palvelimen reuna. Muussa tapauksessa 200 – tilakoodi (esimerkiksi 403 estetty, 404 ei löydy, jne.) palautetaan.

- TCP_EXPIRED _HIT – tämä tila on näkyvissä, kun pyynnön, joka kohdistaa omaisuuden vanhentuneet TTL, esimerkiksi kun kohteen max-ikä on päättynyt, jossa on served suoraan POP asiakas.

    Vanhentuneen pyyntö johtaa uudelleenvahvistusta pyyntö yleensä origin palvelimeen. Jotta TCP_EXPIRED _HIT, ilmenee alkuperäisestä palvelimesta on ilmoitettava kohteen uudempaan versioon ei ole. Tällaista tilanne yleensä päivittää kyseinen resurssi välimuisti-komponenttien ja Expires otsikot.

- TCP_EXPIRED _MISS – tämä tila ilmoitetaan, kun uudempaan versioon vanhentuneet välimuistissa resurssi on served POP asiakkaalle. Tämä tapahtuu, kun TTL välimuistissa resurssi on päättynyt (esimerkiksi vanhentunut max-ikä) ja alkuperäisestä palvelimesta palauttaa kyseinen resurssi uudemmalla versiolla. Tämä kohteen uusi versio served asiakkaalle välimuistissa version sijaan. Lisäksi se välimuistiin reuna-palvelin ja asiakas.

- CONFIG_NOCACHE – tämä tila tarkoittaa, että asiakaskohtainen määrityksen Microsoftin reunan POP esti kohteen välimuistissa.

- Ei mitään - tämä tila tarkoittaa, että välimuistin sisällön tuoreus-valintaruutu ei suoritettu.

- TCP_ CLIENT_REFRESH _MISS – tämä tila on näkyvissä, kun HTTP-asiakasohjelma (esimerkiksi selain) pakottaa viiston reunan POP hakemiseen vanhentuneiden kohteiden uuden version alkuperäisestä palvelimesta.

    Microsoftin palvelimiin estää oletusarvoisesti HTTP-asiakas pakottaminen meidän reunapalvelimet hakemiseen kohteen uuden version alkuperäisestä palvelimesta.

- TCP_ PARTIAL_HIT – tämä tila on näkyvissä, kun tavu alueen pyyntö tuloksena on osuma, osittain välimuistissa annetaan. Pyydetty DBCS-alueen välittömästi served POP-asiakkaalle.

- UNCACHEABLE – tämä tila ilmoitetaan, kun resurssi välimuisti-komponenttien ja Expires ylätunnisteet osoittavat, että se ei ole välimuistiin POP- tai HTTP-asiakas. Seuraavanlaisiin pyynnöt saapuvat alkuperäisestä palvelimesta

## <a name="cache-hit-ratio"></a>Välimuistin osumien prosenttiosuus

Tämä raportti ilmaisee välimuistissa pyynnöt, jotka olivat served suoraan välimuistin prosentteina.

Raportissa on seuraavat asiat:

- Vaadittua sisältöä on välimuistiin lähinnä pyytäjä POP.
- Pyyntö on served suoraan sivustoissamme reunasta.
- Pyynnön, eivät edellytä uudelleenvahvistusta origin palvelimen kanssa.

Raportissa ei ole:

- Pyynnöt, joka on estetty, koska maan suodatusasetukset.
- Resurssit, joiden otsikot osoittaa, että ne eivät välimuistiin pyynnöt. Esimerkiksi välimuisti-komponenttien: yksityinen-välimuisti-komponenttien: ei välimuistin tai ohjelma: ei välimuistin otsikot estää sijoituksen välimuistissa.
- Tavu alueen pyynnöt osittain välimuistissa sisältöä.

Kaava on: (TCP_ OSUMIEN / (TCP_ OSUMIEN + TCP_MISS)) * 100

- Valitse päivämääräalue, tarkastella tietoja tänään tällä viikolla/tässä kuussa jne tai mukautetut päivämäärät ja valitse sitten "Siirry" varmistaaksesi, että valintasi päivitetään.
- Voit viedä ja lataa tiedot sijaitsevat "Siirry" vieressä excel-taulukon kuvaketta napsauttamalla.


![Välimuistin osumien raportti](./media/cdn-reports/cdn-cache-hit-ratio.png)

## <a name="ipv4ipv6-data-transferred"></a>Siirtää IPv4/IPV6-tiedot

Tämä raportti näyttää liikenne käyttöä jakelu IPV4 ja IPV6.

![Siirtää IPv4/IPV6-tiedot](./media/cdn-reports/cdn-ipv4-ipv6.png)

- Valitse päivämääräalue, voit tarkastella tietoja tänään tällä viikolla/tässä kuussa yms tai mukautetun päivämäärät.
- Napsauta "Siirry" varmistaaksesi, että valintasi päivitetään.


## <a name="considerations"></a>Huomioon otettavia seikkoja

Raporttien voidaan luoda vain viimeisen 18 kuukauden kuluessa.

<properties
    pageTitle="Azure SQL-tietokantaan ensisijainen yleiskatsaus"
    description="Tässä ohjeaiheessa kerrotaan Azure SQL-tietokannan ensisijainen käytetään määritettäessä Azure SQL-tietokannan suorituskykyä."
    services="sql-database"
    documentationCenter="na"
    authors="CarlRabeler"
    manager="jhubbard"
    editor="monicar" />


<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="06/21/2016"
    ms.author="carlrab" />

# <a name="azure-sql-database-benchmark-overview"></a>Azure SQL-tietokantaan ensisijainen yleiskatsaus

## <a name="overview"></a>Yleiskatsaus
Microsoft Azure SQL-tietokanta on kolme [palvelutasot](sql-database-service-tiers.md) kanssa suorituskyvyn tasoja. Suorituskyvyn tasoissa sisältää resurssien tai "power', pitää yhä suurempi siirtonopeuden suunniteltu kasvava joukko.

On tärkeää, että voit määrittää, miten suorituskyvyn tasoissa kasvava power kääntää parantavat tietokannan suorituskykyä. Voit tehdä tämän Microsoft on kehittänyt Azure SQL tietokanta ensisijaisen (ASDB). Ensisijainen käyttää pitämällä järjestelmässä sekä perustoimintojen löytyvät kaikki OLTP työmäärät. Olemme mittaa saavuttaa käynnissä suorituskyvyn jokaisella tasolla tietokantojen nopeus.

Resurssien ja service-tason ja suorituskyvyn tasoissa power ilmaistaan myyntialueella [tietokannan tapahtuman (DTUs)](sql-database-technical-overview.md#understand-dtus). DTUs lisäämistapaa kuvaavat suhteellisia kapasiteetista perustuvan suorittimen, muistin sekoitetun mitataan suorituskyvyn tason ja lukeminen ja kirjoittaminen korvaukset tarjoamia suorituskyvyn tasoissa. Laimennossarja tietokannan DTU luokitus vastaa tehtävässä laimennossarja tietokannan potenssiin. Ensisijainen pystyy arvioida tietokannan suorituskykyä tarjoamia suorituskyvyn tasoissa käyttämällä todellisen tietokannan toimintoja, kun skaalaus tietokannan koko useita käyttäjiä ja tapahtuman korvaukset suhteessa tietokantaan varat kasvava power vaikutus.

Ilmoittaminen siirtonopeuden Basic service-tason mukaan käyttämällä tapahtumat kohti tunnin käyttämällä tapahtumat minuutissa vakiorivejä taso ja käyttämällä tapahtumia sekunnissa se helpottaa suhteiden nopeasti suorituskyvyn mahdolliset sovelluksen vaatimukset palvelun kunkin tason Premium-palvelutaso.

## <a name="correlating-benchmark-results-to-real-world-database-performance"></a>Käytännön tietokannan suorituskykyä korreloivaa ensisijainen tulokset
On tärkeää ymmärtää, että ASDB, kuten kaikki viitearvoja on edustava ja indikatiivisten vain. Ensisijainen sovelluksessa saavuttaa tapahtuman korvaukset eivät ole samoja kuin voi saavuttaa sovellusten kanssa. Ensisijainen käsittää sivustokokoelman eri tapahtuman tyypit Suorita mallin pohjalta, sisältävän solualueen taulukot ja tietotyypit. Kun ensisijainen käyttää samaa peruslaskutoimituksia, jotka löytyvät kaikki OLTP työmäärät, se ei vastaa kaikki tietyn luokan tietokannan tai sovelluksen. Ensisijainen lähinnä antamaan tietokannan, joka saattaa odottaa, kun skaalaus ylös tai alas välillä suorituskykyyn tasot suhteelliset suorituskyvyn suositeltavaa kerralla opas. Todellisuudessa tietokantojen erikokoisia ja monimutkaisuus, ilmenee työmääriä eri yhdistelmiä ja vastaa eri tavoilla. Esimerkiksi IO paljon sovellus saattaa osumien IO raja-arvot nopeammin tai Suoritinta kuormittavaa sovellus saattaa osumien suorittimen rajoitukset nopeammin. Ei ole takaa, että kaikki tietyn tietokannan Skaalaa samalla tavalla kuin ensisijaisen kasvavia kuormituksen.

Ensisijainen ja sen kehitysmenetelmistä on kuvattu alla tarkemmin.

## <a name="benchmark-summary"></a>Ensisijainen yhteenveto
ASDB mittaa mix basic tietokannan toimia, jotka esiintyvät useimmin online tapahtuman käsittely (OLTP) työmääriä suorituskykyä. Vaikka ensisijainen on suunniteltu cloud tietojenkäsittely huomioon, tietokannan rakennetta, tietojen populaation ja tapahtumat on suunniteltu on laajasti edustavan useimmin käytetyt OLTP työmääriä perusosat.

## <a name="schema"></a>Rakenne
Rakenteen tarkoituksena on tarpeeksi eri ja monimutkaisuus tukemaan laajan-toimintoa. Vertailusuoritukset koostuu kuusi taulukot tietokantaa. Taulukoiden jakautuvat kolmeen luokkaan: kiinteä-kokoa skaalaus ja kasvava. On kaksi kiinteä taulukoihin. kolme skaalauksen taulukoihin. ja kasvava taulukossa. Kiinteä taulukoissa on kiinteä määrä rivejä. Skaalauksen taulukoissa on kardinaliteetti, joka on suhteellisen tietokannan suorituskykyä, mutta ei muuta ensisijainen aikana. Kasvava taulukon kokoa muutetaan alkuperäisen, mutta kardinaliteetti muutokset aikana käynnissä ensisijainen, kun rivejä lisätään ja poistetaan skaalauksen taulukon kaltaisen.

Malli sisältää useita tietotyyppejä, kuten kokonaisluku, numeerinen, merkki ja päivämäärä ja kellonaika. Malli sisältää perus-ja, mutta ei viiteavaimien – eli on viite-eheyden rajoitteita taulukoiden välille.

Tietojen luonti-ohjelma luo alkuperäisen tietokannan tiedot. Kokonaisluku ja numeeriset tiedot muodostetaan eri strategioita kanssa. Joissakin tapauksissa arvot jaetaan satunnaisesti alueen yli. Muussa tapauksessa arvojoukon on satunnaisesti laskettua varmistamiseksi tietyn jakauman säilytetään. Tekstikentät luodaan sanat tuottamaan Realistisen näköisen tietojen luettelo.

Tietokannan kokoa muutetaan perusteella "asteikko kerroin." (Lyhennettä SF) asteikon kerroin määrittää kasvava taulukoita ja skaalausta merkintöjä. Seuraavalla tavalla-kohdassa käyttäjät ja Pacing, tietokannan kokoa useita käyttäjiä ja parhaan mahdollisen suorituskyvyn kaikki skaalaudu suhteessa toisiinsa.

## <a name="transactions"></a>Tapahtumat
Työmäärää koostuu yhdeksän tapahtumatyypit, kuten alla olevassa taulukossa on esitetty. Myyntitapahtumien on suunniteltu Korosta tietyt järjestelmän ominaisuudet-tietokannan ohjelma ja järjestelmän laitteiston, suuri kontrasti-muiden tapahtumien kanssa. Tätä tapaa helpottaa arvioida vaikuttavat suorituskykyyn eri osat. Esimerkiksi "Luku paksu" tapahtuman tuottaa merkittäviä luvun levyltä Lue-toimintoa.

| Tapahtumatyyppi | Kuvaus |
|---|---|
| Lue Lite | VALITSE; ladatun; vain luku-tilassa |
| Lue Normaali | VALITSE; enimmäkseen ladatun; vain luku-tilassa |
| Lue näkyvä | VALITSE; enimmäkseen ei ladatun; vain luku-tilassa |
| Päivitä Lite | PÄIVITÄ; ladatun; luku-ja kirjoitusoikeudet |
| Päivitä näkyvä | PÄIVITÄ; pääosin ei ladatun; luku-ja kirjoitusoikeudet |
| Lisää Lite | LISÄÄ; ladatun; luku-ja kirjoitusoikeudet |
| Lisää näkyvä | LISÄÄ; enimmäkseen ei ladatun; luku-ja kirjoitusoikeudet |
| Poista | POISTA; ladatun sekä ei ladatun; luku-ja kirjoitusoikeudet |
| Suorittimen näkyvä | VALITSE; ladatun; suhteellisen suorittimen kuormitus; vain luku-tilassa |

## <a name="workload-mix"></a>Kuormituksen mix
Tapahtumat valitaan satunnaisesti seuraavat yleinen mix painotettu jaettavaksi. Yleistä mix on luku-/ kirjoitusoikeudet suhde on noin 2:1.

| Tapahtumatyyppi | Mix % |
|---|---|
| Lue Lite | 35 |
| Lue Normaali | 20 |
| Lue näkyvä | 5 |
| Päivitä Lite | 20 |
| Päivitä näkyvä | 3 |
| Lisää Lite | 3 |
| Lisää näkyvä | 2 |
| Poista | 2 |
| Suorittimen näkyvä | 10 |

## <a name="users-and-pacing"></a>Käyttäjien ja välistys
Ensisijainen työmäärää on perustuva työkalua, joka lähettää tapahtumat yhteyksien simuloida käyttäjien määrää toimintaa joukkoa. Vaikka kaikki yhteydet ja tapahtumia on luotu tietokoneeseen, yksinkertaisuuden viitataan asiakasyhteydet: "a". Vaikka kunkin käyttäjän toimii ulkopuolisista muille käyttäjille, kaikki käyttäjät suorittaa samaan jaksoon alla on esitetty vaiheet:

1. Muodosta tietokantayhteys.
2. Toista, kunnes tehdä, jos haluat poistua:
    - Valitse tapahtuma satunnaisesti (painotettu jakauman).
    - Suorittaa valitun tapahtuman ja mitataan vastausajan.
    - Odota pacing viive.
3. Sulje tietokantayhteys.
4. Lopeta.

Pacing viive (vaiheessa 2c) on valittuna satunnaisesti, mutta jakauman kanssa, joka sisältää 1.0 toisen keskiarvon. Näin kunkin käyttäjän keskimäärin voidaan luoda yksi tapahtuma sekunnissa.

## <a name="scaling-rules"></a>Skaalaus säännöt
Käyttäjien määrä määräytyy tietokannan koko (asteikko kerroin yksikköinä). Yksittäisen käyttäjän, joka viisi asteikko kerroin yksiköiden on. Pacing viive vuoksi yksi käyttäjä voi luoda yksi tapahtuma sekunnissa keskimäärin.

Esimerkiksi mittakaava-kerrointa 500 (SF = 500) tietokanta on 100 käyttäjää ja toteuttaa enimmäisnopeus, 100 TP merkinnät. Levyasemassa suurempi TP merkinnät korvaus tarvitaan useita käyttäjiä ja suuremman tietokannan.

Alla olevassa taulukossa näkyy kunkin palvelun taso ja suorituskyvyn tason todella sustained käyttäjämäärä.

| Palvelutaso (suorituskyvyn taso) | Käyttäjät | Tietokannan koko |
|---|---|---|
| Perustoiminnot | 5 | 720 MT |
| Vakio (S0) | 10 | VÄHINTÄÄN 1 GT |
| Vakio (S1) | 20 | 2.1 GT |
| Vakio (S2) | 50 | 7.1 GT |
| Premium (P1) | 100 | 14 GT |
| Premium (P2) | 200 | 28 GT |
| Premium (6/P3) | 800 | 114 GT |

## <a name="measurement-duration"></a>Mitta-kesto
Kelvollinen ensisijaisen, suorita edellyttää vähintään tunnin muuttumattoman mitta kestoa.

## <a name="metrics"></a>Arvot
Ensisijainen-avaimen arvot ovat siirtonopeuden ja vastaus kerran.

- On ensisijainen olennaiset suorituskyky-mitta. Tapahtumien-aikayksikkö, Laske kaikki siirtonopeuden on näkyvissä.
- Mittaa suorituskyvyn ennustettavuutta vastausajan. Vastauksen aikarajoituksia vaihtelee palvelun kanssa suurempi luokkien palvelun on tiukemmat vastaus kerran vaatimus alla kuvatulla tavalla.

| Palvelun  | Siirtonopeuden mitta | Vastauksen ajan vaatimus |
|---|---|---|
| Premium | Tapahtumia sekunnissa | 95. prosenttipiste 0,5-palvelussa |
| Vakio | Tapahtumien minuutissa | 90. prosenttipiste 1,0 sekuntia-palvelussa |
| Perustoiminnot | Tapahtumien tunnissa | 80th prosenttipisteen 2.0 sekuntia-palvelussa |

## <a name="conclusion"></a>Tekemistä
Azure SQL-tietokanta-ensisijainen mittaa suhteellinen suorituskykyä Azure SQL-tietokanta on käytettävissä palvelutasot ja suorituskyvyn taso alueen yli. Ensisijainen käyttää mix basic tietokannan toimia, jotka esiintyvät useimmin online tapahtuman käsittely (OLTP) toiminnoista. Mukaan mittaaminen todellisen suorituskyvyn ensisijainen tarjoaa merkityksellisempi arvioida vaikutukset siirtonopeuden vaihtamisesta suorituskyky kuin on mahdollista tekemällä luettelo vain myöntämä tasoissa, kuten suorittimen nopeutta, muistikoko ja IOPS resursseja. Olemme säilyvät myöhemmin, muodostaen ensisijainen, jos haluat laajentaa sen laajuus sekä laajentavat annetut tiedot.

## <a name="resources"></a>Resurssit
[Johdanto SQL-tietokantaan](sql-database-technical-overview.md)

[Palvelutasot ja suorituskyvyn tasoon](sql-database-service-tiers.md)

[Yksittäisen tietokantojen suorituskyvyn ohjeet](sql-database-performance-guidance.md)

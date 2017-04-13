<properties
    pageTitle="Mallinnus Multitenancy hakutoiminnolla Azure | Microsoft Azure | Isännöityjen pilvipalvelussa haku"
    description="Tietoja Yleiset rakenteen mallit multitenant SaaS sovellusten Azure haku-ohjelmalla."
    services="search"
    manager="jhubbard"
    authors="ashmaka"
    documentationCenter=""/>

<tags
    ms.service="search"
    ms.devlang="NA"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="10/26/2016"
    ms.author="ashmaka"/>

# <a name="design-patterns-for-multitenant-saas-applications-and-azure-search"></a>Suunnittele kuviot multitenant SaaS sovellusten ja Azure haku

Multitenant sovellus on sellainen, joka sisältää haluamasi määrän alihallinnat, kuka et voi nähdä tai jakaa tietoja muiden vuokraajan samoja palveluja ja ominaisuuksia. Tässä asiakirjassa kerrotaan vuokraajan eristystaso strategioita multitenant sovellusten Azure haulla.

## <a name="azure-search-concepts"></a>Azure haun käsitteitä
Etsi nimellä--palvelun, ratkaisuksi Azure haun tällä ohjelmalla ohjelmistokehittäjät voivat monipuolisia Etsi kokemukset lisääminen sovellusten ilman mitään infrastruktuurin hallinta tai tulossa haun asiantuntija. Tietojen lataaminen palveluun ja sitten pilveen tallennettujen. Käytä yksinkertaisen pyynnöt Azure haun Ohjelmointirajapinta, tiedot sitten voidaan muokata ja haku kohdistuu. Palvelun yleiskatsaus löytyy [sisältö](http://aka.ms/whatisazsearch). Ennen kuin keskustella rakenne kuviot, on tärkeää ymmärtää joidenkin käsitteiden Azure hakutoiminnolla.

### <a name="search-services-indexes-fields-and-documents"></a>Etsi palveluita, indeksit, kenttien ja asiakirjat
Azure-haun avulla, kun jokin tilaa _hakupalvelu_. Kun tietoja tuodaan Azure haun, se tallennetaan _indeksi_ search-palvelun. Voi olla yksittäisen palvelun indeksien määrä. Tietokantojen tuttuja käsitteitä käyttämään hakupalvelua voit voi tuotantovoimavara tietokannan samalla, kun indeksit sisällä palvelu on tuotantovoimavara tietokannan taulukoista.

Kunkin search-palvelun indeksi on oma rakennetta, joka on määritetty määrä voi mukauttaa _kentät_. Tiedot lisätään Azure Search-indeksin yksittäisiä _asiakirjoja_. Kunkin asiakirjan tietyn indeksi on ladattava ja on täytyttävä, indeksi-rakennetta. Kun etsit tietoja Azure-haun avulla, teksti-hakukyselyt myönnetään vastaan tietyn indeksi.  Voit verrata käsitteistä kaltaisia tietokannan-kentät voidaan tuotantovoimavara taulukon sarakkeiden ja asiakirjoja voi tuotantovoimavara rivit.

### <a name="scalability"></a>Skaalattavuus
Minkä tahansa Azure Search-palvelun [hinnat taso](https://azure.microsoft.com/pricing/details/search/) vakio-skaalata kaksi mitat: tallennustilan ja käytettävyyttä.
* _Osioiden_ voidaan lisätä niin, että hakupalvelu tallennustilaan.
* _Replikat_ voidaan lisätä niin, että pyynnöt, jotka voivat käsitellä search-palvelun nopeus-palveluun.

Lisääminen ja poistaminen osiot ja replikoiden osoitteessa sallii search-palvelun kanssa tietojen määrä kasvaa ja liikenteen sovelluksen vaatimukset kapasiteetti. Jotta saavuttamiseksi luku [SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/)hakupalvelu edellyttää replikat. Palvelun tavoitteet luku-ja kirjoitusoikeudet [SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/)järjestyksessä edellyttää kolmen replikoita.


### <a name="service-and-index-limits-in-azure-search"></a>Azure haun palvelun ja hakemisto-rajoitukset
On muutamia eri [tasojen hinnoittelua](https://azure.microsoft.com/pricing/details/search/) Azure haun, jokaisella tasoissa on eri [rajoitukset ja kiintiöt](search-limits-quotas-capacity.md). Jotkin nämä raja-arvot ovat service-tasolla, indeksi-tasolla ovat ja jotkin ovat osio-tasolla.


|                                  | Perustoiminnot     | Standard1   | Standard2   | Standard3   | Standard3 HD  |
|----------------------------------|-----------|-------------|-------------|-------------|---------------|
| Suurin replikoiden palvelua kohden     | 3         | 12          | 12          | 12          | 12            |
| Suurin osioiden palvelua kohden   | 1         | 12          | 12          | 12          | 1             |
| Suurin Etsi yksiköt (replikoiden * osiot) palvelua kohden | 3         | 36          | 36          | 36          | 36 (enintään 3 osiot)            |
| Suurin asiakirjojen palvelua kohden    | 1 miljoonaa | 180 miljoonaa | 720 miljoonaa | 1.4 miljardin | 600 miljoonan   |
| Tallennustilan enimmäiskoko palvelua kohden      | 2 GT      | 300 GT      | 1.2 TT      | 2.4 TT      | 600 GT        |
| Suurin asiakirjojen osion kohden  | 1 miljoonaa | 15 miljoonaa  | 60 miljoonaa  | 120 miljoonaa | 200 miljoonaa   |
| Tallennustilan enimmäiskoko osion kohden    | 2 GT      | 25 GT.       | 100 GT      | 200 GT      | 200 GT        |
| Suurin indeksit palvelua kohden      | 5         | 50          | 200         | 200         | 3000 (enintään 1 000 indeksit/osio)          |


#### <a name="s3-high-density"></a>S3 HD "
Azure haun S3 hinnoittelutiedot taso on vaihtoehto, joka on suunniteltu erityisesti multitenant skenaariot suuren arvon (HD)-tilassa. Monissa tapauksissa on suuri määrä pienempi alihallinnat yksittäistä-kohdassa päästä yksinkertaisuuden ja kustannukset tehokkuuden edut tukemaan.

S3 HD sallii useiden pienten indeksien pakataan mukaan myynti voi skaalata indeksit isännöidä indeksejä yksittäisen palveluun mahdollisuuden osioiden avulla, valitse yksittäinen search-palvelun hallinta.

Concretely S3-palvelua voi olla 1 ja 200 indeksit, joita yhdessä isännöidä 1.4 miljardin tiedostojen välillä. S3 HD toisaalta antaa yksittäisiä indeksit jatkaa vain enintään 1 miljoonaa asiakirjaa, mutta se voi käsitellä enintään 1 000 indeksit kohti osion 200 miljoonaa kohden osion asiakirjan määrä (enintään 3000 palvelua kohden) (enintään 600 miljoonaa palvelua kohden).



## <a name="considerations-for-multitenant-applications"></a>Huomioitavaa multitenant sovellusten
Multitenant sovelluksia tehokkaasti täytyy jakaa resurssien kesken alihallinnat säilyttämällä jonkin tason tietosuoja eri omistajien välillä. Liittyy muutama huomioon otettavia seikkoja, kun suunnittelet hakemuksen arkkitehtuuri:

* _Vuokraaja eristystaso:_ Sovelluksen kehittäjät tarvitse tehdä tarvittavat toimenpiteet sen varmistamiseksi, että ei ole alihallinnat-ei valtuuksia tai ei-toivottu ei voi käyttää tietoja muiden omistajien. Vuokraajan eristystaso strategioita edellyttävät lisäksi tietojen suojauksen kannalta häiriöistä muiden tekijöiden suojaus ja jaettujen resurssien tehokkaan hallinnan.
* _Cloud resurssin kustannusten:_ Ohjelmistoratkaisuja on oltava samalla tavalla kuin muiden sovellusten kustannukset kilpailukyvyn multitenant sovelluksen osana.
* _Helpottaminen-toimintoa:_ Kun kehittäminen multitenant arkkitehtuuri-sovelluksen toiminnot ja monimutkaisuus vaikutus on otettava huomioon. Azure haku on [99,9 % SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/).
* _Yleinen vaatiman tallennustilan:_ Multitenant sovelluksia on ehkä yhteyshenkilönä tehokkaasti alihallinnat, jotka jakautuu maapalloa.
* _Skaalattavuus:_ Sovellusten kehittäjille on otettava huomioon, miten ne Täsmäytä sovelluksen Palveltavien kohteiden määrä ja asiakasympäristöjen tietoja ja työmäärää koon skaalaaminen suunnitteleminen ja ylläpito sovelluksen monimutkaisuus riittävän vähäistä.

Azure haku on muutama rajat, jonka avulla voidaan erottaa asiakasympäristöjen tietoja ja työmäärää.

## <a name="modeling-multitenancy-with-azure-search"></a>Mallinnus multitenancy Azure haulla
Multitenant skenaarion sovelluksen kehittäjä siinä käytetään Etsi-palveluita ja jakaa niiden omistajien services ja/tai indeksien kesken. Azure haku on muutamia yleisiä kuviot, kun mallinnus multitenant tilanne:

1. _Indeksi kohden:_ Kunkin vuokraajan on oma indeksi search-palvelun, joka jaetaan muiden omistajien.
1. _Palvelua kohden:_ Kunkin vuokraajan on oma erillinen Azure Search-palvelun tarjoaa parhaan mahdollisen tiedot ja työmäärää etäisyyttä.
1. _Sekoitusta:_ Suureen, Lisää aktiivinen alihallinnat, jotka määritetään erillinen services kun pienempi alihallinnat, jotka on määritetty yksittäisiä indeksit jaettujen palveluiden sisällä.

## <a name="1-index-per-tenant"></a>1. indeksoida kohden
![Indeksi vuokraajan mallin portrayal](./media/search-modeling-multitenant-saas-applications/azure-search-index-per-tenant.png)

Indeksi vuokraajan-malli useita alihallinnat, jotka vievät yksittäisen Azure-hakupalvelun kunkin vuokraajan, joiden on yrityksen oma indeksi.

Alihallinnat saavuttamiseksi eristystaso tietoja, koska kaikki Etsi pyynnöt ja asiakirjan toiminnot on myönnetty indeksi Azure hakutoiminnossa tasolla. Sovelluskerroksen on tarpeen tilatieto liikenne voidaan ohjata eri alihallinnat ERISNIMI indeksiin samalla kun hallitset myös kaikki alihallinnat yli resurssien palvelun tasolla.

Indeksi vuokraajan mallin avaimen määrite on sovelluksen-kehittäjä voi oversubscribe hakupalvelun joukossa sovelluksen alihallinnat kapasiteetin mahdollisuuden. Jos alihallinnat, jotka on erimittainen jakautumisen työmäärää, optimaalisen yhdistelmä alihallinnat, jotka voivat jaetaan search-palvelun indeksit sopimaan aikana samanaikaisesti harvinaisissa vähemmän aktiivinen alihallinnat, erittäin aktiivinen, paljon resursseja Palveltavien kohteiden määrä. Trade-off on malli ei käsitellä tilanteita, joissa kunkin vuokraajan on samanaikaisesti erittäin aktiivisena.

Indeksi vuokraajan mallin pohjan muuttuvien kustannusten malli, jossa koko Azure hakupalvelun ostetaan huolellisesti ja sitten myöhemmin täynnä alihallinnat. Näin Käyttämätön kapasiteetti määritetään yritykset ja vapaa-tileille.

Yleinen vaatiman tallennustilan sovellusten indeksi vuokraajan-malli ei ehkä mahdollisimman tehokkaasti. Jos jokin sovellus alihallinnat jakautuu maapallon, erillinen palvelu saattaa olla tarvittavat kunkin alueen joka ehkä kaksoiskappaleiden kustannukset kullekin niistä.

Azure haun avulla asteikon sekä yksittäisiä indeksejä tai indeksien määrä kasvaa. Jos haluamasi hinnat taso valitaan, osiot ja replikat voidaan lisätä koko hakupalvelun kun yksittäisiä indeksin palvelun kasvaa liian suureksi tallennustilan tai liikenne.

Jos indeksit määrä kasvaa liian suureksi yksittäistä, toisen palvelun on valmisteltu sopimaan uusi alihallinnat. Jos indeksit voidaan siirtää hakupalveluiden välillä, kun uusia palveluita lisätään, indeksi tiedot on manuaalisesti kopioidaan indeksi toiseen kuin Azure haku ei salli indeksin siirretään.


## <a name="2-service-per-tenant"></a>2. palvelun kohden
![Palvelun palveltava mallin portrayal](./media/search-modeling-multitenant-saas-applications/azure-search-service-per-tenant.png)

Palvelun palveltava-arkkitehtuuri kunkin vuokraajan on oma search-palvelun.

Tämän mallin sovelluksen kertyy sen omistajien eristystaso enimmäismäärä. Jokaisen palvelun on varattu tallennustila ja siirtonopeuden käsittelyyn hakupyyntöä sekä erillisen API-näppäimiä.

Sovellusten, joissa kunkin vuokraajan on suuri vaatiman tallennustilan tai työmäärää on pieni vaihtelevuudesta vuokraajan vuokraajan-palvelun palveltava malli on tehokas valinta koska resurssit jaetaan ei eri alihallinnat toiminnoista.

Palvelun vuokraajan mallin kohden on myös etu ennakoitavissa, kiinteät kustannukset-mallin. Ei ole huolellisesti sijoituksen koko hakupalvelun kunnes vuokraajan kokoiseksi, mutta kustannukset-kohden-vuokraajan on suurempi kuin indeksi vuokraajan-malli.

Palvelun palveltava malli on yleinen vaatiman tallennustilan sovellusten tehokas vaihtoehto. Maantieteellisesti distributed alihallinnoista, jossa on helppo on kunkin vuokraajan-palvelu haluamasi alue.

Tätä mallia skaalaus-haasteita syntyy, kun yksittäisiä alihallinnat Selvitä niiden palvelun. Azure haku ei tällä hetkellä tue päivittäminen search-palvelun hinnat taso, jotta kaikki tiedot on kopioitava manuaalisesti uusi palvelu.

## <a name="3-mixing-both-models"></a>3. sekoittamalla sekä mallit
Toisen kuvio mallinnus multitenancy on sekoitus indeksi vuokraajan ja palvelun palveltava.

Sekoittamalla kaksi kuviot sovelluksen suurin alihallinnat, jotka voi viedä erillinen services kun aktiivinen pienempi kuin, pienempi alihallinnat harvinaisissa voi viedä indeksit jaettujen palveluiden. Tämä malli varmistaa, että suurin alihallinnat johdonmukaisesti erinomainen suorituskyky-palvelusta mikä auttaa suojaamaan pienempi alihallinnat minkä tahansa häiriöistä muiden tekijöiden aiheuttamia.

Kuitenkin käyttöönoton tämä strategia luottaa ennakointiin korrelaatio mitä alihallinnat, jotka edellyttävät erillinen palvelu ja jaettujen palveluiden hakemiston. Sovelluksen monimutkaisuus kasvaa hallinta molempia näistä multitenancy malleista ei tarvitse.

## <a name="achieving-even-finer-granularity"></a>Vaikka tarkempaan rakeisuuden saavuttamiseksi
Yllä rakenne kuviot, malliin multitenant skenaariot Azure hakutoiminnossa oletetaan, että kunkin vuokraajan missä sovelluksen koko esiintymän yhtenäisen alueen. Kuitenkin sovellusten joskus voit käsitellä useita pienempi alueet.

Jos palvelun palveltava ja indeksi vuokraajan mallit eivät ole riittävän pieni alueet, on mahdollista malli indeksin saavuttamiseksi rakeisuuden jopa tarkempaan aste.

On yksittäinen indeksin luominen toimivat eri tavalla eri asiakasohjelman päätepisteiden indeksin, joka määrittää tietyn arvon mahdollista kunkin asiakkaan voidaan lisätä kentän. Aina, kun asiakas soittaa tai muokata indeksin, Etsi Azure asiakassovellus koodin määrittää käyttämällä Azure haun [Suodatin](https://msdn.microsoft.com/library/azure/dn798921.aspx) -ominaisuus kyselyn aikana kentän arvo.

Tätä menetelmää voidaan käyttää saavuttamiseksi toimintoja eri käyttäjätilejä, erilliset käyttöoikeustasot, ja erota jopa kokonaan sovellukset.

## <a name="next-steps"></a>Seuraavat vaiheet
Azure haku on useita sovelluksia, [Lue lisää palvelun tehokkaat ominaisuuksia](http://aka.ms/whatisazsearch)näyttävien valinta. Eri rakenne kuviot, multitenant sovellusten laskettaessa tarvittaessa [eri hinnoittelu tasoa](https://azure.microsoft.com/pricing/details/search/) ja parhaiten mukauttaa Azure haku sovelluksen toiminnoista ja kaiken kokoisille arkkitehtuureihin mahtuvat vastaaviin [palvelun rajoitukset](search-limits-quotas-capacity.md) .

Azure-haku- ja multitenant skenaariot kysyttävää voidaan ohjata azuresearch_contact@microsoft.com.

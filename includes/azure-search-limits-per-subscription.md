Voit luoda palveluihin kuluessa tilauksen yksitellen valmisteltu osoitteessa tietyn taso, vain palvelujen sallittu kunkin tason määrän mukaan. Voit esimerkiksi luoda 12 osoitteessa Basic taso ja toinen 12 palvelut osoitteessa saman tilauksen piiriin kuuluvien S1 tason ylöspäin. Saat lisätietoja tasoa, [Valitse SKU: ta tai Azure hakuja taso](../articles/search/search-sku-tier.md).

Suurin rajoitukset voidaan nostaa pyydettäessä. Jos tarvitset lisää palveluja saman tilauksen piiriin kuuluvien, ota yhteyttä Azure-tukeen.

Resurssi|Vapaa|Perustoiminnot|S1|S2|S3 |S3 HD <sup>1</sup>
---|---|---|---|----|---|----
Suurin palvelut |1 |12 |12  |6 |6 |6 
Suurin asteikko SU <sup>2</sup>|Puuttuu- <sup>3</sup>|3 SU <sup>4</sup> |36 SU|36 SU|36 SU|12 SU 3 SU <sup>5</sup>

<sup>1</sup> S3 HD ei tue [Indeksoijilla](../articles/search/search-indexer-overview.md) tällä hetkellä. 

<sup>2</sup> haun yksiköt (SU) ovat laskutettavat yksiköt palvelua, kohdistettu *replikan* tai *osion*kohden. Tarvitset molempien resurssien tallennustilaa, indeksointi ja kyselyn toiminnot. Lisätietoja kelvollinen yhdistelmiä, pidä kohdassa enimmäispituus on artikkelissa [asteikko resurssitasolla kysely- ja indeksi työmääriä varten](../articles/search/search-capacity-planning.md). 

<sup>3</sup> vapaa perustuu jaettuja resursseja käyttää useita tilaajille. Tämä taso-palvelussa ei ole erillinen resursseja yksittäisiä tilaaja. Tästä syystä suurin asteikko on merkitty ei käytettävissä.

<sup>4</sup> basic on yksi kiinteä osio. Milloin tämä taso muita SUs käytetään varattaessa Lisää replikoiden parantavat kyselyn toiminnoista.

<sup>5</sup> S3 HD kannalta sallitun kombinaatioiden kohdentamisesta rakenne on. Replikoiden voi olla enintään 12. Osioiden oletusarvo on 3.





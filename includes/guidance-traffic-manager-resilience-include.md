##<a name="highly-available-solutions-with-azure-traffic-manager"></a>Erittäin käytettävissä ratkaisujen Azure liikenteen hallinta

Sinun täytyy määrittää oman työmäärää suuren käytettävyyden vaatimukset on täytettävä Azure liikenteen hallinta-valintaikkunassa yksin, tai jos haluat yhdistää liikenteen hallinta ja muut DNS-ratkaisuja tai prosessit. Tarpeidesi voit:

- **Liikenteen hallinta yksinään**. Jos 99,99 % kerta ei sovellu havainnollistamiseen, voit käyttää liikenteen hallinta yksinään. Jos liikenteen hallinta-palvelun, käyttäjät eivät voi käyttää havainnollistamiseen, kunnes liikenteen hallinta-palvelun muodostetaan.

- **Käytä liikenteen hallinta ratkaisu sekä Azure liikenteen hallinta**. Jos liikenteen hallinta-palvelussa, voit muuttaa CNAME-tietue osoittamaan muuhun liikenteen hallinta-palveluun. Havainnollistamiseen käyttöä on edelleen käytettävissä ja hajautettu kaikki sijainnit, isännöinnin havainnollistamiseen. Tämä on eniten kallista ratkaisu, mutta voidaan tarvita, joka on suurempi SLA toiminnoista.

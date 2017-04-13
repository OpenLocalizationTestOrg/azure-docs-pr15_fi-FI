## <a name="about-records"></a>Tietoja tietueiden

Kukin DNS-tietue on nimi ja tyyppi. Tietueet on järjestetty eri ne sisältävät tietojen mukaan. Yleisin on "" tietueeseen, joka yhdistää nimi IPv4-osoite. On "MX-tietue, joka yhdistää postipalvelimen nimi.

Azure DNS tukee kaikkia Yleiset DNS-tietuetyypit, mukaan lukien A, AAAA, CNAME, MX, NS, PTR, SOA, SRV ja TXT. Huomaa, että:
- SOA tietueen joukkoja luodaan automaattisesti vyöhykkeen, niitä ei voi luoda erikseen.
- SPF-tietueet luodaan käyttämällä TXT-tietuetyyppi. Lisätietoja on kerätty [tälle sivulle](http://tools.ietf.org/html/rfc7208#section-3.1).

Azure-DNS-tietueet määritetään käyttämällä suhteellinen nimiä. "Täydellinen" toimialuenimi (FQDN) sisältää alueen nimen "suhteellinen" nimi ei. Suhteellinen tietueen nimi "www" alue "contoso.com" antaa esimerkiksi täydellinen tietueen nimi www.contoso.com.

## <a name="about-record-sets"></a>Tietoja tietueiden joukot

Joskus sinun täytyy luoda useamman kuin yhden DNS-tietueen annettu nimi ja tyyppi. Oletetaan esimerkiksi, että "www.contoso.com" web-sivuston isännöidään kaksi eri IP-osoitteisiin. Sivuston edellyttää kaksi eri tietuetta, yksi kullekin IP-osoite. Seuraavassa on esimerkki tietuejoukon:

    www.contoso.com.        3600    IN  A   134.170.185.46
    www.contoso.com.        3600    IN  A   134.170.188.221

Azure DNS hallitsee DNS-tietueiden avulla tietueen joukot. Tietuejoukon on kokoelma DNS-tietueiden vyöhykkeessä, jotka on sama nimi ja tyyppi on sama. Useimmat tietueen joukkoja sisältää vastaavan tietueen, mutta esimerkkejä katsomalla, joiden tietuejoukon on useampi kuin yksi tietue eivät ole epätavallisten.

SOA ja CNAME-tietueen joukot ovat poikkeusten. DNS-standardit eivät salli useita tietueita saman niminen näiden.

Oletusaika eli TTL-määrittää, kuinka kauan kunkin tietueen on välimuistiin asiakkaat ennen uudelleen kyselyä. Tässä esimerkissä TTL on 3 600 sekuntia tai 1 tunti. TTL on määritetty ei kutakin tietuetta varten tietuejoukon käytettäväksi on sama arvo, tietuejoukon kaikki tietueet.

#### <a name="wildcard-record-sets"></a>Yleismerkkien tietueen joukot

Azure DNS tukee [yleismerkkien tietueita](https://en.wikipedia.org/wiki/Wildcard_DNS_record). Nämä ovat palautettiin kyselyä vastaavaa nimeä (paitsi-yleismerkki tietuejoukon tarkempi vastinetta ei). Yleismerkkien tietueen joukkoja tuetaan kaikkien tietuetyyppien NS ja SOA lukuun ottamatta.  

Voit luoda yleismerkkien tietuejoukon käyttämällä tietuejoukon nimi "\*". Tai käytä nimeä, jonka otsikko on "\*", esimerkiksi"\*.foo".

#### <a name="cname-record-sets"></a>CNAME-tietueen joukot

CNAME-tietueen joukkoja ei voi olla saman niminen muita tietueen joukkoja. Esimerkiksi et voi luoda CNAME-tietue, joka määrittää suhteellisen nimellä "www- ja A-tietue suhteellinen nimellä"www"samanaikaisesti. Koska alueen apex (nimen = ‘@’) sisältää aina NS ja SOA tietueeseen joukot, jotka on luotu vyöhykkeen luonnin yhteydessä, et voi luoda määrittää vyöhykkeen apex CNAME-tietue. Näiden rajoitusten aiheutuvat DNS-standardeja, eikä niitä voi Azure DNS rajoitukset.

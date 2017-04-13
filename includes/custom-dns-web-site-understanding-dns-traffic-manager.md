Toimialueen nimi järjestelmän (DNS) käytetään Etsi kohteita Internetissä. Esimerkiksi kun kirjoitat osoitteen selaimessa tai web-sivulla olevaa linkkiä, se käyttää DNS toimialueen muuntamiseksi IP-osoite. IP-osoite on sort, kuten katuosoitteen, mutta se ei ole hyvin ihmisille friendly. Esimerkiksi on helpompi muistaa kuin muista IP-osoite, kuten 192.168.1.88 tai 2001:0:4137:1f67:24a2:3888:9cce:fea3 DNS-nimi, esimerkiksi **contoso.com** .

DNS-järjestelmään perustuu *tietueita*. Tietueiden liittäminen määritetty *nimi*, esimerkiksi **contoso.com**IP-osoite tai toiseen DNS-nimi. Kun sovellus, kuten selain hakee nimi DNS-palvelimesta, se havaitsee tietueen ja käyttää riippumatta siitä, mitä se osoittaa osoitteeksi. Jos se osoittaa on IP-osoite, selain käyttää kyseistä arvoa. Jos se osoittaa toiseen DNS-nimi-sovellus on tarkkuus Tee uudelleen. Kaikki nimenselvitys päättyy kädessä IP-osoite.

Kun luot Azure-verkkosivustosta, DNS-nimi määritetään automaattisesti sivustoon. Tämä nimi Vie muodossa ** &lt;yoursitename&gt;. azurewebsites.net**. Kun lisäät sivuston päätepisteen Azure liikenteen hallinta, sivuston on sitten kautta ** &lt;yourtrafficmanagerprofile&gt;. trafficmanager.net** toimialueen.

> [AZURE.NOTE] Kun sivuston on määritetty liikenteen hallinta päätepisteen, jota käytät **. trafficmanager.net** osoite, kun luot DNS-tietueita.

> Voit käyttää vain CNAME-tietueet liikenteen hallinta

On myös useita erilaisia tietueet, joissa omia Funktiot ja rajoituksia, mutta liikenteen hallinta päätepisteet määrityksenä sivustot, että vain varoen yhden; *CNAME* -tietueet.

###<a name="cname-or-alias-record"></a>CNAME-tai Alias

CNAME-tietue yhdistää *tietyn* DNS-nimi, kuten **mail.contoso.com** tai **www.contoso.com**toiseen (canonical) toimialuenimi. Azure sivustojen liikenteen hallinnan canonical toimialuenimi on ** &lt;OmaSovellus >. trafficmanager.net** liikenteen hallinta profiilin toimialuenimi. Luomiasi CNAME-TIETUE Luo tunnusnimen ** &lt;OmaSovellus >. trafficmanager.net** toimialuenimi. CNAME-merkinnän ratkaisee IP-osoitteen lisääminen ** &lt;OmaSovellus >. trafficmanager.net** toimialuenimi automaattisesti, joten jos sivuston IP-osoite muuttuu, sinun ei tarvitse tehdä mitään.

Liikenne saapuu osoitteessa liikenteen hallinta, kun se reitittää sitten liikenne sivuston, se on määritetty menetelmä kuormituksen. Tämä on täysin läpinäkyvä sivuston kävijöille. He näkevät vain selaimessa mukautettua toimialuenimeä.

> [AZURE.NOTE] Jotkin toimialuerekisteröijät Salli vain voidaan yhdistää alitoimialueita, kun käytät CNAME-tietue, kuten **www.contoso.com**ja ei pääkansio nimiä, esimerkiksi **contoso.com**. Lisätietoja CNAME-tietueet on rekisteröintipalvelustasi, <a href="http://en.wikipedia.org/wiki/CNAME_record">CNAME-tietue Wikipedia-tapahtuma</a>tai <a href="http://tools.ietf.org/html/rfc1035">IETF toimialuenimet - käyttöönotto ja määritys</a> -asiakirjan ohjeissa.

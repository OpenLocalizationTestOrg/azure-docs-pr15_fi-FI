## <a name="load-balancer-differences"></a>Kuormituksen tasauspalvelun erot

Voit jakaa verkkoliikennettä käyttämällä Microsoft Azure eri vaihtoehtoja. Nämä vaihtoehdot toimivat eri tavalla, ottaa eri ominaisuuden määrittäminen ja tue eri vaihtoehtoja. He voivat kunkin voidaan käyttää yksinään tai yhdistää.

- **Azure kuormituksen** työskentelee transport layer (kerros 4 OSI verkon viittaus Pinotut). Se sisältää liikenteen jakautuminen verkon tason kaikissa samaa Azure tietokeskuksen sovelluksen esiintymät.

- **Sovelluksen yhdyskäytävän** työskentelee sovelluskerroksen (kerroksen 7 OSI verkon viittaus Pinotut). Se toimii vastakkainen välityspalvelimen palvelun, asiakkaan yhteyden ja edelleen taustatietokantaan päätepisteet pyynnöt.

- **Liikenteen hallinta** toimii DNS-tasolla.  Se käyttää DNS-vastauksia yleisesti eri aikajaksoille päätepisteiden loppukäyttäjien liikenne voidaan ohjata. Asiakkaiden liitä ne päätepisteet suoraan.

Seuraavassa taulukossa on yhteenveto kunkin palvelun tarjoamia lisäominaisuuksia:

| Palvelu | Azure kuormituksen | Sovelluksen yhdyskäytävä | Liikenteen hallinta |
|---|---|---|---|
|Tekniikka| Transport tason (kerros 4) | Sovellustason (kerroksen 7) | DNS-taso |
| Tuetut sovelluksen protokollat | Mikä tahansa | HTTP- ja HTTPS |  Kaikki (HTTP-päätepisteen on pakollinen päätepisteen seurantaa varten) |
| Päätepisteet | Azure VMs ja pilvipalveluihin roolin esiintymiä | Mikä tahansa Azure sisäinen IP-osoite tai julkinen internet IP-osoite | Azure VMs, pilvipalveluihin, Azure Web Apps-sovellusten ja ulkoisten päätepisteiden |
| Vnet tuki | Voi käyttää sekä Internet aukeaman ja sisäinen (Vnet)-sovellukset | Voi käyttää sekä Internet aukeaman ja sisäinen (Vnet)-sovellukset |    Tukee vain Internetiin yhteydessä oleva sovellukset |
Päätepisteen seuranta | Tuetut keräysputkien kautta | Tuetut keräysputkien kautta | Tuetut HTTP/HTTPS GET | 

Azure kuormituksen ja sovelluksen yhdyskäytävän reitin verkon liikenteen päätepisteet, mutta niissä on eri käyttötapoja käsittelemään liikenne. Seuraavassa taulukossa auttaa kaksi kuormituksen tasoitusmääritykset erot:

| Tyyppi | Azure kuormituksen | Sovelluksen yhdyskäytävän |
|---|---|---|
| Protokollat | UDP/TCP | HTTP / HTTPS |
| IP-varaus | Tuettu | Ei tueta | 
| Lataa Vastatilin tila | 5 tuple(source IP, source port, destination IP, destination port, protocol type) | PYÖRISTÄ-funktiota Mikko<br>URL-Osoitteen reititys perusteella | 
| Kuormituksen tila (lähde-IP / muistilappuja istunnot) |  2-monikon (lähde-IP- ja kohde-IP), 3-monikon (lähde-IP-kohde-IP ja portin). Skaalata ylös tai alas näennäiskoneiden määrän mukaan | Evästeiden perustuva affiniteetti<br>URL-Osoitteen reititys perusteella |
| Kuntotietojen keräysputkien | Oletusarvo: näytteenottimen väli - 15 ja osat. Ottaa pois kierto: 2 jatkuva virheet. Tukee käyttäjän määrittämä keräysputkien | Käyttämättömänä näytteenottimen aikaväli 30 sekuntia. Ottaa 5 peräkkäiset live liikenne virheet tai käyttämättömänä tilassa yksittäisestä virheen jälkeen. Tukee käyttäjän määrittämä keräysputkien | 
| SSL-purkaminen | Ei tueta | Tuettu | 
  
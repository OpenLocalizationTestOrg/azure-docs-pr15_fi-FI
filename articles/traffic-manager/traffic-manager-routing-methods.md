<properties
    pageTitle="Liikenteen hallinta - liikennettä reititys menetelmiä | Microsoft Azure"
    description="Tässä artikkelissa auttavat ymmärtämään eri liikenne reititys menetelmiä liikenteen hallinta"
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="traffic-manager-traffic-routing-methods"></a>Liikenteen hallinta liikenne reititys menetelmät

Azure liikenteen hallinta tukee liikenne reititys kolmella ja kysyttävä, kuinka verkkoliikennettä reitittämiseen eri Palvelupäätepisteet. Liikenteen hallinta koskee kukin DNS-kyselyn se saa liikenne reititys-menetelmää. Liikenne reititys-menetelmä määrittää, mitkä päätepisteen määrää DNS-vastauksen.

Azure Resurssienhallinta-tuki liikenteen hallinta käyttää erilaista terminologiaa kuin perinteinen käyttöönottomalli. Seuraavassa taulukossa on Resurssienhallinta ja perinteinen termejä erot:

| Resurssienhallinta termi | Perinteinen termi |
|-----------------------|--------------|
| Liikenne reititys menetelmä | Kuormituksen tasaamisen menetelmä |
| Prioriteetti-menetelmällä | Automaattisesti menetelmä |
| Painotetun menetelmän | Pyöreän pöydän menetelmä |
| Suorituskyvyn menetelmä | Suorituskyvyn menetelmä |

Asiakaspalautteen perusteella olemme muuttaa parantaa selkeyttä ja vähentää yleisiä väärinkäsityksiä termejä. Ei-toiminto ei ole ero.

Liikenne reititys kolmella ovat käytettävissä liikenteen hallinta:

- **Prioriteetti:** Valitse "Prioriteetti", kun haluat käyttää ensisijainen Palvelupäätepisteen kaikki liikenne ja anna varmuuskopiot siltä varalta, että ensisijainen tai varmuuskopion päätepisteet eivät ole käytettävissä.
- **Painotetun:** Valitse "painotettu", jos haluat jakaa liikenne päätepisteet joukko, joko tasaisesti tai mukaan paksuuksia ja jonka voi määrittää.
- **Suorituskyvyn:** Valitse "Suorituskyvyn", kun päätepisteet ovat eri Maantieteellisten sijaintien ja haluat, että käyttäjät voivat käyttää pienin verkon latenssin kannalta "lähinnä" päätepistettä.

Kaikki liikenteen hallinta-profiileista sisältyvät seurantaan päätepisteen terveys-ja automaattinen päätepisteen automaattisesti. Lisätietoja on artikkelissa [Liikenteen hallinta päätepisteen seuranta](traffic-manager-monitoring.md). Yhden liikenteen hallinta profiilin voit käyttää vain yksi liikenne reititys-menetelmää. Voit valita eri liikenne reititys menetelmän profiilisi milloin tahansa. Tehdyt muutokset tulevat voimaan minuutin kuluessa ja ei ole käyttökatkot on syntynyt. Liikenne reititys menetelmiä voidaan yhdistää käyttämällä sisäkkäisiä liikenteen hallinta-profiileista. Sisäkkäisiä mahdollistaa edistyksellisten ja joustavassa liikenne reititys määritykset, jotka täyttävät suurempia ja monimutkaisia sovellusten tarpeisiin. Lisätietoja on artikkelissa [sisäkkäisiä liikenteen hallinta-profiileista](traffic-manager-nested-profiles.md).

## <a name="priority-traffic-routing-method"></a>Prioriteetti liikenne reititys-menetelmällä

Usein organisaation haluaa antaa luotettavuutta palveluja ottamalla varmuuskopion palveluista vähintään siltä varalta, että hänen ensisijaiseen palveluun siirtyy. "Prioriteetti-liikenne reititys-menetelmän avulla Azure asiakkaat voivat helposti toteuttaa automaattisesti tätä mallia.

![Azure liikenteen hallinta 'prioriteetti-liikenne reititys menetelmä][1]

Liikenteen hallinta profiilin sisältää Palvelupäätepisteet prioriteettiluettelon. Oletusarvon mukaan liikenteen hallinta lähettää kaikki liikenne ensisijainen (suurimman prioriteetin) päätepisteelle. Jos ensisijainen päätepisteen ei ole käytettävissä, liikenteen hallinta reitittää liikenteen toisen päätepisteen. Jos ensisijainen ja toissijainen-päätepisteet eivät ole käytettävissä, liikenne siirtyy kolmannen ja niin edelleen. Käytettävyys päätepisteen perustuu määritetty tila on (käytössä tai poissa käytöstä) ja jatkuvaa päätepisteen seuranta.

### <a name="configuring-endpoints"></a>Päätepisteet määrittäminen

Azure resurssien hallinnan avulla voit määrittää päätepisteen prioriteetti päätepisteiden "prioriteetti"-ominaisuuden avulla erikseen. Tämä ominaisuus on arvo väliltä 1 – 1 000. Pienet arvot edustavat suuri prioriteetti. Päätepisteet voi jakaa Prioriteettiarvoja. Ominaisuuden on valinnainen. Kun jätetään pois, käytetään oletusarvon prioriteetti päätepisteen tilauksen perusteella.

Perinteinen-liittymällä päätepisteen-prioriteetti on määritetty implisiittisesti. Prioriteetti perustuu järjestyksessä, jossa päätepisteet on lueteltu profiili-määritys.

## <a name="weighted-traffic-routing-method"></a>Painotetun liikenne reititys menetelmän

'Painotetun' liikenne reititys-menetelmän avulla liikenne jakaminen tasaisesti tai käyttää ennalta määritettyjä painotus.

![Azure liikenteen hallinta painotettu' liikenne reititys menetelmä][2]

Painotetun liikenne reititys tapaa määrittää paino päätepisteiden liikenteen hallinta profiilin määrityksessä. Paino on kokonaisluku väliltä 1 – 1 000. Tämä parametri on valinnainen. Jos jätetään pois, liikenne valvojat käyttää '1' oletusarvon paksuus.

Kunkin vastaanotetut DNS-kyselyn liikenteen hallinta valitsee käytettävissä päätepisteen. Todennäköisyys, että valitset päätepisteen perustuu varattujen kaikkien käytettävissä päätepisteet leveydet. Samojen paino käyttö kaikki päätepisteet tulokset jopa liikenne-jakauman. Käyttämällä leveydet ylemmälle tai alemmalle tietyn päätepisteet aiheuttaa näitä päätepisteet enemmän tai vähemmän usein DNS-vastaukset palautetaan.

Painotetun menetelmässä on hyödyllinen joissakin tilanteissa:

- Asteittain sovelluksen päivitys: varaaminen prosentteina uusi päätepiste reitittää liikenteen ja suurenna vähitellen liikenne ajan kuluessa 100 %.
- Azure sovelluksen siirto: profiilin luominen Azure- ja ulkoisten päätepisteet kanssa. Muuta päätepisteet mieluummin uuden päätepisteet leveyden.
- Cloud bursting muita kapasiteetin: Laajenna nopeasti sijoittamalla liikenteen hallinta profiilin takana paikallisen käyttöönoton pilveen. Kun tarvitset ylimääräinen kapasiteetti pilvipalvelussa, voit lisätä tai Lisää päätepisteet ottaminen käyttöön ja määrittää, mikä osa liikenne siirtyy päätepisteiden.

Uuden Azure portaalin tukee painotettu liikenne reitityksestä määritykset. Leveydet ei voi määrittää Classic-portaalissa. Voit myös määrittää leveydet Resurssienhallinta ja perinteinen PowerShellin Azure CLI ja REST API-versioiden käyttäminen.

On tärkeää ymmärtää, että DNS-vastauksia välimuistissa asiakkaat ja rekursiivinen DNS-palvelimet, asiakkaat avulla voit selvittää DNS-nimiä. Tämä välimuisti voi olla vaikutusta painotettu liikenne jaot. Kun asiakkaat ja rekursiivinen DNS-palvelimet määrä on suuri, liikenne jakauman toimii odotetulla. Kun asiakkaat tai rekursiivinen DNS-palvelimet on pieni, välimuistiin voi merkittävästi siirtää liikenne jakauman.

Yleisiä Käytä tapauksissa ovat seuraavat:

- Kehittäminen ja testauksen ympäristöissä
- Sovellusten viestintä
- Sovellusten pyritään kapea käyttäjä-base, jotka jakavat yleisiä rekursiivinen DNS-infrastruktuurin (esimerkiksi yhteyden välityspalvelimen kautta yrityksen työntekijöiden)

Nämä DNS-välimuistin tehosteet ovat yleisiä kaikki DNS-pohjaisen liikenteen reititys järjestelmiin, ei pelkästään Azure liikenteen hallinta. Joissakin tapauksissa erikseen DNS-välimuistin tyhjentäminen voi antaa seuraavaa vaihtoehtoista menetelmää. Muussa tapauksessa liikenne reititys vaihtoehtoisesti voi sopia paremmin.

## <a name="performance-traffic-routing-method"></a>Suorituskyvyn liikenne reititys-menetelmällä

Kahden tai useamman sijainnin päätepisteet käyttöönotto yli maapallon voit parantaa useita sovelluksia vasteaikaa reitittää liikenteen haluamaasi kohtaan lähinnä '' sinulle. Tämä menetelmä 'Suorituskyvyn' liikenne reititys on tätä ominaisuutta.

![Azure liikenteen hallinta 'Suorituskyvyn' liikenne reititys menetelmä][3]

Lähin' päätepisteen ei ole välttämättä lähinnä maantieteelliset etäisyyden mukaan. Sen sijaan 'Suorituskyvyn' liikenne reititys-menetelmä mittaaminen verkon latenssin lähinnä päätepisteen määrittää. Liikenteen hallinta ylläpitää Internet viive taulukon, jolla seurataan IP-osoitealueet ja kunkin Azure palvelinkeskuksen välinen vastauksen aika.

Liikenteen hallinta etsii saapuva DNS-pyyntö Internet viive-taulukon lähde IP-osoite. Liikenteen hallinta valitsee käytettävissä päätepisteen Azure joten, joka on pienin viive kyseisen IP-osoitealueita varten, ja palauttaa kyseisen päätepisteen DNS-vastaus.

[Miten liikenteen hallinta toimii](traffic-manager-how-traffic-manager-works.md)esitetyllä liikenteen hallinta eivät saa DNS-kyselyt suoraan asiakkaille. Sen sijaan DNS-kyselyt peräisin rekursiivinen DNS-palvelun asiakkaat määritetään käyttämään. Tämän vuoksi IP-osoite käytetään lähinnä' päätepisteen ei ole asiakkaan IP-osoite, mutta se on rekursiivinen DNS-palvelun IP-osoite. Käytännössä tämä IP-osoite on hyvä välityspalvelimen asiakkaalle.

Liikenteen hallinta päivittää säännöllisesti asiakas muutokset Yleinen Internet- ja Azure kaksi uutta Internet viive-taulukossa. Kuitenkin sovelluksen suorituskyvyn vaihtelee reaaliaikainen variaatiot kuormituksen Internetissä. Suorituskyvyn liikenne reitityksestä Valvo tietyn Palvelupäätepisteen kuormitus. Kuitenkin päätepisteen ei ole käytettävissä, jos liikenteen hallinta ei ole sen DNS-kyselyn vastaukset.

Määritä Points to Huomautus

- Jos profiilin sisältää useita päätepisteet saman Azure alueen, valitse liikenteen hallinta jakaa liikenne tasaisesti alueen käytettävissä päätepisteet. Jos käytät mieluummin eri liikenne jakauman alueella, voit käyttää [sisäkkäisiä liikenteen hallinta-profiileista](traffic-manager-nested-profiles.md).

- Jos kaikki käytössä päätepisteet Azure alueella on heikentynyt, liikenteen hallinta jakaa liikenne muiden kaikki käytettävissä olevat päätepisteet seuraava lähinnä päätepisteen sijaan. Tämä logiikka estää CSS virheen ilmenemisen ylikuormituksen ei seuraava lähinnä päätepisteen mukaan. Jos haluat määrittää ensisijaiset automaattisesti, käytä [sisäkkäisiä liikenteen hallinta-profiileista](traffic-manager-nested-profiles.md).

- Käytettäessä suorituskyvyn liikenne reititys menetelmä ulkoisten päätepisteiden tai sisäkkäisiä päätepisteet haluat määrittää kyseiset päätepisteet sijainnin. Valitse käyttöönoton lähinnä Azure alue. Sijaintien on tuettu Internet viive-taulukon arvot.

- Algoritmin, valitsee päätepisteen on deterministic. Toistuvat saman asiakkaan DNS kyselyjen ohjataan saman päätepiste. Yleensä asiakkaat käyttää eri rekursiivinen DNS-palvelimet matkoilla. Asiakas voi reitittää eri Endpoint. Reititys voi vaikuttaa myös Internet viive taulukon päivitykset. Suorituskyvyn liikenne reititys-menetelmä vuoksi takaa, että asiakas reititetään aina sama päätepiste.

- Internet-viive taulukon muuttuessa saatat huomata, että jotkin asiakkaat ohjataan eri endpoint. Reititys muutos on tarkempaa nykyisen viive-tietojen perusteella. Nämä päivitykset, jotka ovat välttämättömiä säilyttää tarkkuudella suorituskyvyn liikenne reitityksestä Internet jatkuvasti siihen.

## <a name="next-steps"></a>Seuraavat vaiheet

Lue, miten voit kehittää suuren käytettävyyden sovellusten [liikenteen hallinta päätepisteen seuranta](traffic-manager-monitoring.md)

Lisätietoja [liikenteen hallinta-profiilin](traffic-manager-manage-profiles.md) luominen

<!--Image references-->
[1]: ./media/traffic-manager-routing-methods/priority.png
[2]: ./media/traffic-manager-routing-methods/weighted.png
[3]: ./media/traffic-manager-routing-methods/performance.png

<properties
    pageTitle="Huomioitavaa suorituskyvystä Azure liikenteen hallinta | Microsoft Azure"
    description="Tietoja suorituskyvyn liikenteen hallinta ja sivuston suorituskyvyn testaaminen käytettäessä liikenteen hallinta"
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

# <a name="performance-considerations-for-traffic-manager"></a>Huomioitavaa suorituskyvystä liikenteen hallinta

Tällä sivulla käsitellään liikenteen hallinnan suorituskykyyn liittyviä tietoja. Ota huomioon seuraavat skenaariota:

Sinulla on sivuston esiintymät WestUS ja EastAsia alueilla. Yksi esiintymien epäonnistui tuntemattomasta kuntotietojen tarkistuksessa keräysputken liikenteen hallinta. Sovelluksen liikenne ohjataan kunnossa alue. Tämä automaattisesti odotetaan mutta suorituskyky voi olla ongelma nyt matkustaminen kaukana alueen liikenne viive perusteella.

## <a name="how-traffic-manager-works"></a>Liikenteen hallinta toiminta

Vain vaikutus suorituskykyyn liikenteen hallinta voi olla sivuston on alkuperäinen DNS-haku. Microsoftin DNS-pääkansio palvelin, joka isännöi trafficmanager.net vyöhykkeen käsittelee DNS-pyynnön liikenteen hallinta profiilin nimi. Liikenteen hallinta täyttää ja päivittää säännöllisesti, on Microsoft DNS-pääkansio palvelimien liikenteen hallinta käytännön ja näytteenottimen tulokset perusteella. Siten myös alkuperäisen DNS-haun aikana ei ole DNS-kyselyt lähetetään liikenteen hallinta.

Liikenteen hallinta koostuu useita osia: DNS-palvelimet, API-palvelun tai tallennustilan kerros päätepisteen seuranta palvelun nimen. Jos liikenteen hallinta-palvelun osa epäonnistuu, ei ei vaikuta liikenteen hallinta profiilin liittyvän DNS-nimi. Microsoftin DNS-palvelimilta tietueet pysyy muuttumattomana. Kuitenkin päätepisteen seuranta ja DNS-päivitys ei suoriteta. Vuoksi liikenteen hallinta ei voi päivittää DNS osoittavan automaattisesti sivustoon ensisijainen sivuston siirtyy.

DNS-nimenselvitys on nopea ja tulokset säilyvät välimuistissa. Alkuperäinen DNS-haku nopeuden määräytyy asiakkaan käyttää nimenselvitys DNS-palvelimet. Yleensä asiakkaan voivat tehdä DNS-haku ~ 50 ms kuluessa. Haun tulokset säilyvät välimuistissa kesto DNS--elinaika (TTL). Oletus-TTL liikenteen hallinta on 300 sekuntia.

Liikenne ei rivity kautta liikenteen hallinta. Kun DNS-haku on valmis, asiakkaalla on esiintymän sivustosi IP-osoite. Asiakkaan osoitteen muodostaa ja Välitä kautta liikenteen hallinta. Voit valita liikenteen hallinta käytännön ei ole vaikutusta DNS-suorituskyky. Suorituskyvyn reititys-menetelmä voit kuitenkin haitallinen vaikutus sovelluksen kokemus. Esimerkiksi jos käytäntöjen ohjaa tietoliikenteen Pohjois-Amerikka ylläpidettävä Aasian esiintymä-verkon latenssin näistä istunnoista saattaa olla suorituskykyyn liittyvä ongelma.

## <a name="measuring-traffic-manager-performance"></a>Mittaaminen liikenteen hallinta suorituskyky

On useita sivustoja, voit tehdä suorituskyky ja toiminta liikenteen hallinta profiilin tietoja. Monia näistä sivustot ovat maksuttomia, mutta voi olla rajoituksia. Jotkin sivustot tarjoavat parannettu seuranta ja raportointi maksua vastaan.

Näiden sivustojen mittayksikön DNS viiveet suurempia ja Näytä Työkalut ratkaista IP-osoitteet asiakkaan sijainnit eri puolilla maailmaa. Useimmat näistä työkaluista Älä tallenna välimuistiin DNS-tulokset. Tämän vuoksi työkaluja Näytä koko DNS-haku aina, kun testaus on suoritettu. Kun testaat oman asiakaskoneelta, kohtaat vain koko DNS-haku suorituskyvyn kerran TTL keston aikana.

## <a name="sample-tools-to-measure-dns-performance"></a>Esimerkki Työkalut DNS suorituskykyä

- [SolveDNS](http://www.solvedns.com/dns-comparison/)

    SolveDNS tarjoaa useita suorituskyky-työkaluja. DNS-vertailu-työkalun voidaan näyttää, kuinka kauan rekisteröijän DNS-nimen ja verrata, DNS palveluntarjoajia.

- [WebSitePulse](http://www.websitepulse.com/help/tools.php)

    Yksinkertaisin työkalut on WebSitePulse. Kirjoita DNS tarkkuus kerran, ensimmäinen tavu, viimeisen tavun ja muut Tuottavuustilastot. Voit valita kolme eri koe sijainneista. Tässä esimerkissä näet, että ensimmäinen suoritus näkyy DNS-haku kestää 0.204 s.

    ![pulse1](./media/traffic-manager-performance-considerations/traffic-manager-web-site-pulse.png)

    Tulokset välimuistiin, koska toisen testin saman liikenteen hallinta päätepisteen DNS-haku kestää 0.002 s.

    ![pulse2](./media/traffic-manager-performance-considerations/traffic-manager-web-site-pulse2.png)

- [CA App synteettiset näyttö](https://asm.ca.com/en/checkit.php)

    Ennen nimeltään Watchmouse valintaruudun sivuston työkalu, tämä sivusto noudattamalla DNS tarkkuus aika useita maantieteellisiä alueelta samanaikaisesti. Kirjoita DNS tarkkuus aika, yhteyttä muodostettaessa ja nopeuden useita maantieteellisiä sijainneista. Tämän testin avulla voit nähdä, mitkä isännöityä palvelun palautetaan eri sijainneissa eri puolilla maailmaa.

    ![pulse1](./media/traffic-manager-performance-considerations/traffic-manager-web-site-watchmouse.png)

- [Pingdom](http://tools.pingdom.com/)

    Tämä työkalu sisältää Tuottavuustilastot jokaisen osan verkkosivulle. Sivun analysoi-välilehteen prosentteina ajan kirjaamisen DNS-haku.

- [DNS-ominaisuudet](http://www.whatsmydns.net/)

    Tämä sivusto ei DNS-haku 20 eri paikoissa ja näyttää tulokset kartalla.

- [Tarkastella Internet-käyttöliittymä](http://www.digwebinterface.com)

    Tämän sivuston näyttää yksityiskohtaisempia DNS-tiedot, kuten CNAMEs ja tietueisiin. Varmista, että Tarkista asetukset-kohdassa 'Colorize tulostus' ja 'Tilasto- ja all (kaikki)-kohdassa nimipalvelimia.

## <a name="next-steps"></a>Seuraavat vaiheet

[Tietoja liikenteen hallinta liikenne reititys menetelmät](traffic-manager-routing-methods.md)

[Testaa liikenteen hallinta](traffic-manager-testing-settings.md)

[Toiminnot-liikenteen hallinta (REST API hakemisto)](http://go.microsoft.com/fwlink/?LinkId=313584)

[Azure liikenteen hallinnan cmdlet-komennot](http://go.microsoft.com/fwlink/p/?LinkId=400769)

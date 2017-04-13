<properties
   pageTitle="Palvelun kangasta klusterin kapasiteetin suunnittelu | Microsoft Azure"
   description="Palvelun kangasta klusterin kapasiteetti suunnittelussa huomioon otettavia seikkoja. Nodetypes, kestävyyttä ja luotettavuutta tasoa"
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/09/2016"
   ms.author="chackdan"/>


# <a name="service-fabric-cluster-capacity-planning-considerations"></a>Palvelun kangasta klusterin kapasiteetin suunnittelussa huomioon otettavia seikkoja

Mikä tahansa tuotantoympäristössä kapasiteetin suunnittelu on tärkeä vaihe. Seuraavassa on kuvattu joitakin huomioitavia osana prosessin merkittyjä kohteita.

- Solmun määrän kirjoituskentän klusterin tarpeitasi aloittaa
- Ominaisuuksien kunkin solmutyyppi (koko, perus, internet vastakkaisten VMs jne.)
- Klusterin luotettavasti ja kestävyyttä ominaisuudet

Anna meidän lyhyesti Tarkista nämä kohteet.

## <a name="the-number-of-node-types-your-cluster-needs-to-start-out-with"></a>Solmun määrän kirjoituskentän klusterin tarpeitasi aloittaa

Ensin sinun täytyy selvittää, mitä olet luomassa klusterin suorittaminen käytetään ja millaisia sovellusten aiot asentaa tämän klusterin. Jos et Tyhjennä klusterin tarkoituksesta, ei todennäköisesti ole vielä valmis antamaan suunnitteluprosessin kapasiteetti.

Muodostaa yhteyttä klusterin on aluksi solmutyypit määrä.  Solmun kullakin virhelajilla on yhdistetty virtuaalikoneen asteikko joukkoon. Solmun kunkin tyyppiselle voi skaalata sitten tai alas itsenäisesti on erilaisia arvojoukkoja avoimet portit ja voi olla eri kapasiteetin arvot. Niin solmutyypit määrän päätös olennaisesti on seuraavat seikat:

- Onko sovelluksen palveluihin, ja halutut arvosarjat on julkinen tai Internetiin yhteydessä? Tyypillinen sovellukset sisältävät edusta gateway-palveluun, joka vastaanottaa syötteen asiakas, ja vähintään yksi taustatietokantaan palveluita viestiä edusta-palvelujen kanssa. Niin tässä tapauksessa lopputulos on vähintään kaksi solmun tyyppiä.

- Palvelujen (jotka muodostavat sovelluksen) on eri infrastruktuurin tarpeita, kuten suurempi RAM-Muistia tai uudempi versio suorittimen jaksot Oletetaan esimerkiksi, kerro meille, sovellus, jossa haluat ottaa käyttöön sisältää edusta palvelu ja taustatietokantaan palvelu. Edusta-palvelun voidaan suorittaa pienempi VMs (kuten D2 koot AM), joissa on Internet Avaa portit.  Taustatietokannan-palvelun kuitenkin on paljon laskenta ja täytyy suorittaa suurempi VMs (sekä AM koot D15 D4, D6, kuten), jotka eivät ole internet vastakkaisten.

 Tässä esimerkissä vaikka voit sijoittaa kaikki palvelut yhtä solmutyyppi, on suositeltavaa, että ne sijoitettu klusterin solmu kahdenlaisia kanssa.  Näin solmu mistäkin on eri ominaisuuksien, kuten internet-yhteys tai AM kokoa. VMs määrän skaalata itsenäisesti, sekä.  

- Koska et voi ennustaa myöhemmin, mukana tiedät tietoja ja päättää solmu Lisättävissä olevat sovellukset on aluksi määrän. Voit aina lisäät tai poistat solmutyyppejä myöhemmin. Palvelun kangasta klusteriin on oltava vähintään yksi solmutyyppi.

## <a name="the-properties-of-each-node-type"></a>Solmun kunkin tyypin ominaisuudet

**Solmutyyppi** näkevät Cloud Services-roolien vastaaviksi. Solmutyypit Määritä AM koot, VMs määrä ja niiden ominaisuudet. Solmun verkkosivustoasi, joka on määritetty palvelun kangasta klusterissa on määritetty joukkona erillisessä virtuaalikoneen asteikko. AM asteikko joukot ovat Azure Laske resurssien avulla voit ottaa käyttöön ja hallinnassa näennäiskoneiden joukkona. On määritetty eri AM mittakaava määrittää, kunkin solmutyypin skaalata sitten alaspäin itsenäisesti on erilaisia arvojoukkoja avoimet portit ja voi olla eri kapasiteetin arvot.

Yhteyttä klusterin voi olla useita solmutyyppi, mutta ensisijainen solmutyyppi (ensimmäinen tehtävä, joka määritetään portaalissa) on oltava vähintään viisi VMs varten käytettävät tuotannon työmääriä klustereiden (tai vähintään kolme VMs testi klustereiden varten). Jos olet luomassa klusterin Resurssienhallinta-mallin avulla, saatat huomata solmu tyyppi määritelmän **on ensisijainen** -määrite. Ensisijainen solmutyyppi on solmun tyyppiä palvelun kangasta järjestelmäpalveluita sisältävään.  

### <a name="primary-node-type"></a>Ensisijainen solmutyyppi
Klusterin käyttämällä useita solmun sinun on valitsemaan yhden niistä on ensisijainen. Seuraavassa on ensisijainen solmutyyppi tunnusmerkit:

- Ensisijainen solmun tyyppiä VMs vähimmäiskoko määräytyy valitset kestävyyttä taso. Kestävyyttä tason oletusarvo on pronssi. Vieritä kestävyyttä tason ominaisuudet ja se voi viedä arvot.  

- Ensisijainen solmun tyyppiä VMs vähimmäismäärä määräytyy valitset luotettavuuden taso. Luotettavuuden taso oletusarvo on hopea. Vieritä luotettavuutta tason ominaisuudet ja se voi viedä arvot.

- Palvelun kangasta järjestelmän palveluita (esimerkiksi klusterin hallintapalvelu tai kuvan säilöpalvelun) sijoitetaan ensisijainen solmutyyppi ja niin luotettavasti ja kestävyys klusterin määräytyy luotettavuutta taso ja kestävyyttä taso arvon ensisijainen solmu-tyypin valitseminen.

![Näyttökuva klusteriin, joka on kaksi solmu ][SystemServices]


### <a name="non-primary-node-type"></a>Muu kuin ensisijaisen solmutyyppi
Klusterin käyttämällä useita solmu on yksi ensisijainen solmutyyppi ja muut ovat muun ensisijaisen. Seuraavassa on ensisijainen solmutyyppi ominaisuudet:

- Solmun tällaista VMs vähimmäiskoko määräytyy valitset kestävyyttä taso. Kestävyyttä tason oletusarvo on pronssi. Vieritä kestävyyttä tason ominaisuudet ja se voi viedä arvot.  

- Solmun tällaista VMs vähimmäismäärä voi olla jokin. Valitse kuitenkin tämän numeron määrä on sovelluksen tai palveluja, jotka haluat suorittaa solmu tällaista perusteella. Solmutyyppi VMs määrä voidaan lisätä klusterin käyttöönoton jälkeen.


## <a name="the-durability-characteristics-of-the-cluster"></a>Klusterin kestävyyttä ominaisuudet

Kestävyyttä tason käytetään osoittamaan järjestelmälle yhteyttä VMs pohjana Azure infrastruktuuriin sisältävät oikeudet. Ensisijainen solmu, kirjoita tämä oikeus sallii palvelun kangasta pysähtyvän AM tason infrastruktuuri pyyntö (esimerkiksi AM uudelleenkäynnistyksen, AM reimage tai AM siirto), jotka vaikuttavat järjestelmäpalvelut ja tilallisten palvelujen quorum vaatimukset. Perus solmutyypit tämä oikeus sallii palvelun kangasta pysähtyvän AM tason infrastruktuuri pyyntö kuten AM uudelleenkäynnistyksen, AM reimage tai AM siirron jne, jotka vaikuttavat päätösvaltaisuutta koskevat vaatimukset tilallisten palvelujen ei käytössä.

Tämä oikeus on ilmaistu seuraavat arvot:

- Kulta - infrastruktuurin työt voi keskeyttää UD 2 tuntia, kesto

- Hopea - infrastruktuurin työt voi keskeyttää ajan 30 minuuttia kohden UD (tämä ei tällä hetkellä ole käytössä käyttöä varten. Kerran käytössä tämä on käytettävissä kaikkien vakio VMs yksittäisen core ja edellä).

- Pronssi - ei ole oikeuksia. Tämä on oletusarvo.

## <a name="the-reliability-characteristics-of-the-cluster"></a>Klusterin luotettavuutta ominaisuudet

Luotettavuuden taso käytetään määrä on järjestelmän palveluja, jonka haluat suorittaa tämän klusterin ensisijainen solmutyyppi. Lisää määrä replikoiden, Lisää luotettavia järjestelmän-palvelut ovat yhteyttä klusterin.  

Luotettavuuden taso voi tehdä seuraavat arvot.

- PLATINUM - järjestelmäpalveluiden suorittaminen kanssa kohde replikajoukon Laske / 9

- Kulta - järjestelmäpalveluiden suorittaminen kanssa kohde replikajoukon Laske / 7

- Hopea - järjestelmäpalveluiden suorittaminen kanssa kohde replikajoukon 5 määrä

- Pronssi - järjestelmäpalveluiden suorittaminen kanssa kohde replikajoukon Laske / 3

>[AZURE.NOTE] Valitse luotettavuutta taso määrittää solmujen ensisijainen solmutyyppi on oltava vähimmäismäärä. Luotettavuuden taso ei ole-klusterin enimmäiskoon. Jotta voi olla 20 solmun klusterin, joka suoritetaan pronssi luotettavuutta.

 Voit päivittää yhden tason toiseen yhteyttä klusterin luotettavuutta. Näin käynnistää klusteri päivityksistä tarvitaan muutettava järjestelmän services se määritetty määrä. Odota, että päivitys on meneillään suorittamiseen ennen kuin teet muutoksia klusterin, kuten lisätä solmujen jne.  Voit valvoa palvelun kangasta Explorer tai suorittamalla [Get-ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt126012.aspx) päivityksen etenemisen

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Seuraavat vaiheet

Kun lopetat kapasiteetin suunnitteleminen ja määrittäminen klusterin, lue seuraavat:
- [Palvelun kangasta klusterin suojaus](service-fabric-cluster-security.md)
- [Palvelun kangasta kunto mallin esittely](service-fabric-health-introduction.md)

<!--Image references-->
[SystemServices]: ./media/service-fabric-cluster-capacity/SystemServices.png

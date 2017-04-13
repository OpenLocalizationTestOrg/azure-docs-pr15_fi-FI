<properties
    pageTitle="Valitse SKU: ta tai hinnat taso Azure hakuja | Microsoft Azure"
    description="Azure haku on valmisteltu osoitteessa nämä tuotteissa: vapaa, Basic ja vakio, jossa vakio on käytettävissä eri resurssin käyttömahdollisuudet ja kapasiteettiin."
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="jhubbard"
    editor=""
    tags="azure-portal"/>

<tags
    ms.service="search"
    ms.devlang="NA"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="10/24/2016"
    ms.author="heidist"/>

# <a name="choose-a-sku-or-pricing-tier-for-azure-search"></a>Valitse SKU: ta tai hinnat taso Azure haussa

Kirjoita Azure haku [palvelu on valmisteltu](search-create-service-portal.md) tietyn hinnoittelu taso tai tuote. **Vapaa**, **Basic**tai **Vakio**, jos **Vakio** on käytettävissä useita määrityksiä ja valmiuksia on asetukset. 

Tässä artikkelissa on helpompi määrittää taso. Jos taso kapasiteetin muuttuu on liian alhainen, sinun on voit valmistella korkeampi taso, uusi palvelu ja lataa sitten indeksejä. Ei käytönaikainen päivittää samalla tavalla kuin yksi SKU toiseen. 

> [AZURE.NOTE] Kun valitset tason ja [säännöstä search-palvelun](search-create-service-portal.md), voit suurentaa replikan ja osion laskee palvelun. Ohjeita on artikkelissa [asteikko resurssitasolla kyselyn ja indeksoinnin toiminnoista](search-capacity-planning.md).

## <a name="how-to-approach-a-pricing-tier-decision"></a>Miten menetelmän hinnoittelu taso-päätös

Azure hakutoiminnossa tason määrittää kapasiteetti-ominaisuuksien saatavuudesta. Yleensä ominaisuudet ovat käytettävissä jokaisella taso, mukaan lukien esikatselutoiminnot-palvelussa. Ainoa poikkeus ei tueta Indeksoijia S3 HD.

> [AZURE.TIP] On suositeltavaa valmistella aina **vapaa** -palvelun (yksi tilaus ei vanhene, kohden) niin, että sen helposti himmennetty projekteissa käytettäväksi. Käytä testaus ja arvioinnissa, **vapaa** -palvelu Luo toinen laskutettavan palvelu tuotannon tai suurempi testi työmääriä **Basic** - tai **Vakio** -taso.

Kapasiteetin ja kustannukset palvelua Siirry käytettävissä käsin. Tämän artikkelin tietojen avulla voit päättää, mitkä SKU toimittaa oikea tasapaino, mutta jokin se on hyödyllinen, tarvitset vähintään karkea arvioiden seuraavasti:

- Määrän ja koon luotavaan indeksit
- Määrän ja koon tiedostojen lataaminen
- Jotkin verrata kyselyn äänenvoimakkuutta myös kyselyjen kohti toisen (QPS)

Luku- ja koko ovat tärkeitä, sillä suurin rajat saavutetaan kiinteä rajoitus indeksejä tai palvelu-tiedostojen määrä tai resurssit (tallennustilan tai replikoita)-palvelun käyttämä kautta. Palvelun todellinen rajoitus on kumpi saa käytetty ensimmäisen: resurssien tai objekteja.

Käsi arvioiden, jossa seuraavasti olisi yksinkertaistamaan prosessia:

- **Vaihe 1** Lue lisätietoja käytettävissä olevista vaihtoehdoista alla SKU kuvaukset.
- **Vaihe 2** Vastaa kysymyksiin alustava päätös vuoroon seuraavia ohjeita.
- **Vaihe 3** Viimeistele tämä päätös tarkastelemalla Kova tallennustilan rajoitusten ja hinnoittelusta.

## <a name="sku-descriptions"></a>TUOTE kuvaukset

Seuraavassa taulukossa on kuvattu kunkin tason. 

Taso|Ensisijainen skenaariot
----|-----------------
**Vapaa**|Jaettujen palveluiden, maksutta käyttää arviointi, tutkimuksen tai pieni toiminnoista. Koska se on jaettu verkkokansioon, kyselyn siirtonopeuden ja indeksoinnin vaihtelee kuka muu on-palvelun avulla. Kapasiteetti on pieni (50 Megatavua tai 3 indeksien määrittäminen 10 000 asiakirjat kunkin).
**Perustoiminnot**|Pieni tuotannon työmääriä erillinen laitteistoon. Erittäin käytettävissä. Kapasiteetti on enintään 3 replikoita ja 1 (2 gt)-osio.
**S1**|Standard 1 tukee osioiden (12) ja replikoiden (12), medium tuotannon työmääriä erillinen laitteistoon käytettäviä joustavia yhdistelmät. Voit kohdistaa osiot ja replikoiden enimmäismäärä 36 laskutettavan haun yksiköt tukemat yhdistelmät. Tällä tasolla on 25 gt. osioita ja QPS on noin 15 kyselyitä sekunnissa.
**S2**|Vakio 2 suorittaa suurempi tuotannon työmääriä käyttämällä samaa 36 haun yksiköt S1 mutta suurempi samankokoista osiot ja replikoita. Tällä tasolla on 100 Gigatavua osioita ja QPS on noin 60 kyselyitä sekunnissa.
**S3**|Vakio 3 suoritetaan suhteessa suurempi tuotannon työmääriä suurempi Lopeta järjestelmiin määritykset enintään 12 osioiden tai 12 replikoiden alle 36 haun yksiköt. Tällä tasolla osiot ovat 200 gt ja QPS on yli 60 kyselyitä sekunnissa. 
**S3 HD**|Vakio 3 HD on suunniteltu suuri määrä pienempi indeksit. Voit määrittää 3 osioiden 200 gt osoitteessa. QPS on yli 60 kyselyitä sekunnissa. 

> [AZURE.NOTE] Replikan ja osion enimmäisarvot laskuttaa Etsi yksiköt (36 yksikkö suurin palvelua kohden), joissa edellytetään tehokkaita alaraja kuin suurin merkitsee täydestä arvosta. Esimerkiksi käyttämään 12 replikoiden enimmäismäärä voi olla enintään 3 osioiden (12 * 3 = 36 yksiköt). Vastaavasti käyttää suurin osioita, Vähennä replikoiden 3. Saat sallitun yhdistelmien kaavion [Mittakaava resurssitasolla kysely ja Azure hakutoiminnossa indeksoinnin toiminnoista](search-capacity-planning.md) .

## <a name="review-limits-per-tier"></a>Tarkista rajoitukset taso kohden

Seuraavassa kaaviossa on alijoukkoa [palvelun](search-limits-quotas-capacity.md)rajat Azure hakutoiminnossa rajoitukset. Se näyttää todennäköisesti SKU päätökseen vaikuttavat tekijät. Tässä kaaviossa voidaan viitata, kun tarkastelet alla olevia kysymyksiä.

Resurssi|Vapaa|Perustoiminnot|S1|S2|S3 |S3 HD
---|---|---|---|----|---|----
Tason palvelusopimus (SLA)|Nro <sup>1</sup> |Kyllä |Kyllä  |Kyllä |Kyllä  |Kyllä 
Indeksi-rajoitukset|3|5|50|200|200|1000 <sup>2</sup>
Asiakirjan rajoitukset|10 000 yhteensä|1 miljoonaa palvelua kohden|15 miljoonaa osion kohden |60 miljoonaa osion kohden|120 miljoonaa osion kohden |1 miljoonaa indeksiä kohti
Suurin osioihin|PUUTTUU |1 |12  |12 |12|3 <sup>2</sup>
Osion koko|50 Mt yhteensä|2 gt palvelua kohden|25 Gigatavua kohden osio |100 Gigatavua kohden osion (enintään 1.2 TT palvelua kohden)|200 gt kohti osioon (enintään 2,4 TT palvelua kohden)|200 gt (enintään 600 gt palvelua kohden)
Suurin replikoita|PUUTTUU |3 |12 |12 |12|12
Kyselyitä sekunnissa|PUUTTUU|~ 3 replika|~ 15 replika|~ 60 replika|> 60 replika|> 60 replika

<sup>1</sup> vapaa- ja esikatselu-tuotteissa ei mukana palvelutasosopimuksia. Palvelutasosopimuksia ovat voimassa, kun tuote on yleensä käytettävissä.

<sup>2</sup> S3 ja S3 HD varmuuskopioidaan samanlaiset suuri kapasiteetti infrastruktuurin mukaan, mutta jokaisessa saavuttaa sen enimmäismäärä eri tavoilla. S3 jotakin erittäin suuri indeksien pienempi luku. Näin ollen sen enimmäismäärä on sidottu resurssin (2,4 TT kumpaankin palveluun on oma). S3 HD kohteena on runsaasti vähän indeksit. 1 000 indeksit osoitteessa S3 HD saavuttaa sen rajoitukset hakemiston rajoitteet muodossa. Jos olet S3 HD-asiakas, joka edellyttää yli 1 000 indeksit, pyydä lisätietoja toimintatapaa Microsoft Support.

## <a name="eliminate-skus-that-dont-meet-requirements"></a>Poista tuotteissa, joka ei täytä vaatimuksia 

Seuraavat kysymykset auttavat havainnollistamiseen oikean SKU päätökseen vuoroon.

1. Onko sinulla **Palvelussa palvelutasosopimusta (SLA)** vaatimuksia? Rajaa Basic tai esikatselu standardin SKU päätös.
2. **Kuinka monta indeksit** Haluatko tarvitaan? Yksi suurimmista muuttujat käyttötilejä kyselyjä SKU päätös on kunkin SKU tukemat indeksien määrä. Indeksi-tuki on pienempi hinnoittelu tasoa huomattavasti eri tasoilla. Indeksien määrä, jotka koskevat voi olla tuote päätös ensisijainen determinantti.
3. **Kuinka monta tiedostoja** ladataan kunkin indeksin? Määrä ja asiakirjojen koko määräytyy indeksin potentiaalisen koon. Jos arvioit indeksin arvioidut koon, voit verrata vastaan osion koko per tuote, laajennettu verran indeksin kokoiset tallentamiseen tarvittavien osioiden määrää. 
4. **Mitä on odotettavissa kyselyn kuormituksen**? Kun tallennustilaa on ymmärretty, harkitse kyselyn toiminnoista. S2 ja S3 sekä tuotteissa tarjoavat lähellä tuottona siirtonopeuden, mutta SLA vaatimukset sääntö minkä tahansa ennakkoversioon tuotteissa. 
5. Jos harkitset S2 tai S3 taso, selvitä, tarvitsetko [Indeksoijilla](search-indexer-overview.md). Indeksoijilla eivät ole vielä käytettävissä S3 HD-tason. Vaihtoehtoinen menetelmä on käyttää push-mallin indeksi-päivitykset-kohtaa, johon voit kirjoittaa sovelluksen koodia push tietojoukon indeksin.

Useimmat asiakkaat voivat säännön tietyn SKU sisään tai ulos niiden vastaukset edellä kysymyksiin perusteella. Jos et ole varma, mikä tuote mukana, lähetä kysymyksesi MSDN- tai StackOverflow keskustelupalstoilla tai Azure tuelta muita ohjeita.

## <a name="decision-validation-does-the-sku-offer-sufficient-storage-and-qps"></a>Päätös vahvistus: olevat versiot tarjota riittävästi säilytys- ja QPS?

Viimeisessä vaiheessa palata tähän määritykseen [sivun hinnat](https://azure.microsoft.com/pricing/details/search/) ja [kohdissa palvelun - ja hakemisto palvelun rajoitukset](search-limits-quotas-capacity.md) , tarkista tilaus- ja palveluluettelon rajoitukset oman arvioita. 

Jos hinnan ja tallennustilaa vaatimukset ovat sallittujen rajojen ulos, haluat ehkä refactor kesken pienempi palveluihin työmääriä (esimerkiksi). Eritellympiä tasolla voitu muuttamiseen indeksit pienemmän tai suodattimien avulla voit tehostaa kyselyt.

> [AZURE.NOTE] Tallennustilaa voi olla perusteettomasti paineistetun, jos tiedostoissa on ylimääräiset tietoja. Ihannetapauksessa tiedostoissa haettavat tiedot tai metatiedot. Binaaritietoja on ei-haettavaksi ja tallentaa erikseen (esimerkiksi Azure taulukon tai blob storage) kentän indeksin pitoon ulkoisen viittauksen URL-osoite. Yksittäisen tiedoston enimmäiskoko on 16 Megatavua (tai vähemmän, jos olet joukkona yhden pyynnössä useiden tiedostojen lataaminen). Lisätietoja on kohdassa [palvelun rajoitukset Azure hakutoiminnolla](search-limits-quotas-capacity.md) .

## <a name="next-step"></a>Seuraava vaihe

Kun tiedät, mitä tuote on oikea Sovita, jatka seuraavasti:

- [Luo search-palvelun-portaalissa](search-create-service-portal.md)
- [Muuta osiot ja replikoiden skaalata palvelusi jakamisesta](search-capacity-planning.md)


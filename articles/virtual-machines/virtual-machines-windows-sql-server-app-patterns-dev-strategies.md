<properties
    pageTitle="SQL Server sovelluksen kuvioita VMs | Microsoft Azure"
    description="Tässä artikkelissa käsitellään SQL Server Azure VMs-sovelluksen kuviot. Se on ratkaisun architects ja kehittäjät on hyvä application arkkitehtuuri ja rakenne."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="luisherring"
    manager="jhubbard"
    editor=""
    tags="azure-service-management,azure-resource-manager" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/19/2016"
    ms.author="lvargas" />

# <a name="application-patterns-and-development-strategies-for-sql-server-in-azure-virtual-machines"></a>Sovelluksen kuvioiden ja kehitys strategioita Azuren näennäiskoneiden SQL Server

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


## <a name="summary"></a>Yhteenveto:
Tarkistamalla, mitä sovelluksen kuvion tai kuviot käyttämään SQL Server perustuva sovellusten Azure-ympäristö on tärkeää rakenne-päätös ja siihen tarvitaan tiedot siitä, miten ja SQL Server Azure infrastruktuurin kunkin osan toimivat yhdessä. SQL Server Azure-infrastruktuuripalvelut, jossa voit helposti siirtää, ylläpitää ja seurata laadittuihin käyttöön Windows Server-Azuren näennäiskoneiden SQL Server sovellukset.

Tässä artikkelissa on antaa ratkaisu architects ja kehittäjille on hyvä application arkkitehtuuri ja rakenteen, jonka he voivat seurata aiemmin sovellusten Azure sekä uuden kehityssovellusten Azure siirrettäessä.

Kunkin sovelluksen viivakuvion etsii paikallisen-skenaario, sen vastaaviin cloud käyttävä ratkaisu ja Aiheeseen liittyvät teknisiä suosituksia. Lisäksi tässä artikkelissa käsitellään Azure-kohtaisia kehittämisen strategiat niin, että voit suunnitella sovellustesi oikein. Vuoksi mahdollisimman monta kuviot on suositeltavaa, että kehittäjät että architects kannattaa valita sopivimman kuvion niiden sovellukset ja käyttäjille.

**Tekninen osallistujien:** Matti Teemu Vargas sillin Madhan Arumugam Ramakrishnan

**Tekninen tarkistajat:** Corey hiomakoneet, laativan McDaniel, Narayan Annamalai, Nir Mashkowski, Sanjay Mishra, Silvano Coriani Teemun tilanne Schackow Tim Hickey, Tim Wieman, Xin Jin

## <a name="introduction"></a>Johdanto

Voit kehittää monenlaisia n tason sovelluksia erottamalla toiseen sovellukseen kerrokset eri tietokoneissa samoin kuin erillisten osat. Esimerkiksi voit sijoittaa asiakassovellus ja business säännöt osia yksi tietokone, edusta-WWW-taso ja tietojen käytön taso osia toiseen tietokoneeseen ja taustatietokannan taso-toiseen tietokoneeseen. Tällaisen rakenteen suunnitteluun avulla voidaan erottaa toisistaan kunkin tason. Jos muutat mistä tiedot tulevat, ei tarvitse muuttaa asiakas- tai web-sovelluksen, mutta vain taso tiedot access-komponentit.

Tyypillinen *n-taso* -sovelluksen sisältää esityksen taso, business taso ja tietojen taso:


| Taso              | Kuvaus                                                                                                                                                                     |
|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Esityksen** | *Esityksen taso* (web taso, edusta taso) on kerroksen, jossa käyttäjät käyttävät sovelluksen.                                                                      |
| **Business**     | *Business taso* (Keskitaso) on kerrosta, jonka esityksen taso ja tietoja-tason avulla keskenään ja sisältää järjestelmän perustoiminnot. |
| **Tietoja**         | *Tietojen taso* on lähinnä palvelin, joka tallentaa sovelluksen tiedot (esimerkiksi SQL Serveriä server).                                                             |

Sovelluksen kerrokset kuvaavat loogisesti ryhmiteltyjä toiminnot ja osien sovelluksessa. Tämän vuoksi tasoa kuvaavat fyysinen jakautumisen toiminnot ja -osien eri fyysinen palvelimiin, tietokoneiden, verkkojen tai remote sijainnit. Sovelluksen kerroksia voi asuvat samassa tietokoneessa fyysinen (samalla tasolla) tai olla suhteutettuna erillisen (n-taso) ja kerroksia osat yhteydenpito osia muut kerrokset hyvin määritetyn liittymät kautta. Voit ajatella termin tason viittauksiksi fyysinen jakauman kuviot, kuten kahden tai kolmen tason n-taso. **Tason 2 sovelluksen kuvio** on kaksi sovelluksen tasoa: sovelluksen ja tietokantapalvelimen. Suora viestintä tapahtuu sovelluspalvelin ja tietokantapalvelimen välillä. Sovelluspalvelin sisältää web-taso ja business tason osia. **3 tason sovelluksen kuvio**on kolme sovelluksen tasoa: WWW-palvelin, sovelluspalvelin, joka sisältää yrityksen logiikan taso ja/tai business taso tiedot access-komponentit, ja tietokantapalvelin. Verkkopalvelin ja tietokantapalvelimen välisen tapahtuu sovelluspalvelin päälle. Lisätietoja sovelluksen kerrokset ja tasojen [Microsoft Application](https://msdn.microsoft.com/library/ff650706.aspx)-arkkitehtuuri oppaassa.

Ennen kuin aloitat lukemalla tämän artikkelin, sinulla on oltava knowledge- ja SQL Server Azure peruskäsitteet. Lisätietoja on artikkelissa [SQL Server Books Online](https://msdn.microsoft.com/library/bb545450.aspx), [SQL Server Azuren näennäiskoneiden](virtual-machines-windows-sql-server-iaas-overview.md) ja [Azure.com](https://azure.microsoft.com/).

Tässä artikkelissa kuvataan useita sovelluksen kuviot, joka voi olla sopii yksinkertainen sovellustesi sekä erittäin monimutkaisia enterprise-sovellukset. Ennen näkyy kunkin kuvion yksityiskohtaisesti, on suositeltavaa, että sinulla olisi valintanauhaan tutustuminen Azure, kuten [Azuren tallennustilaan](../storage/storage-introduction.md) [Azure SQL-tietokanta](../sql-database/sql-database-technical-overview.md)tai [SQL Server Azure Virtual Machine](virtual-machines-windows-sql-server-iaas-overview.md)käytettävissä tietopalvelujen tallennustilan. Päättämään sovellusten parhaat rakenne ymmärtää käyttäminen tietojen mitä tallennuspalvelu selkeästi.

### <a name="choose-sql-server-in-an-azure-virtual-machine-when"></a>Valitse SQL Server Azure Virtual Machine, kun:

- Sinun on SQL Server- ja Windows-ohjausobjektin. Esimerkiksi tämä saattavat sisältää SQL Server-versiota, määräten korjausten, suorituskyvyn määritykset, jne.

- On koko paikalliseen SQL Server-yhteensopivuus ja haluat siirtää aiemmin sovellusten kuin Azure-on.

- Haluat hyödyntää Azure-ympäristön ominaisuuksia, mutta Azure SQL-tietokanta ei tue kaikkia sovellus edellyttää ominaisuuksia. Tämä saattaa sisältää seuraavilla alueilla:

    - **Tietokannan koko**: Tässä artikkelissa on päivitetty milloin SQL-tietokanta tukee enintään 1 TT: n tiedot tietokantaan. Jos et halua mukautetun sharding ratkaisuja sovellus edellyttää enintään 1 TT: n tietoja, on suositeltavaa, että käytät SQL Server Azure Virtual Machine. Katso uusimmat tiedot [Skaalaus ulos Azure SQL-tietokannat](https://msdn.microsoft.com/library/azure/dn495641.aspx) ja [Azure SQL-tietokannan palvelutasot ja suorituskykyä](../sql-database/sql-database-service-tiers.md).
    - **HIPAA yhteensopivuus**: voi valita terveydenhuollon asiakkaille sekä itsenäisten ohjelmistotoimittajat [SQL Server Azuren näennäiskoneiden](virtual-machines-windows-sql-server-iaas-overview.md) sijaan [Azure SQL-tietokanta](../sql-database/sql-database-technical-overview.md) sillä SQL Server Azure Virtual Machine käsitellään mukaan HIPAA Business liittää sopimuksen (BAA). Lisätietoja yhteensopivuuden on artikkelissa [Microsoft Azure Trust Centeriin: vaatimustenmukaisuus](https://azure.microsoft.com/support/trust-center/compliance/).
    - **Esiintymän tason ominaisuudet**: tällä hetkellä SQL-tietokanta ei tue live ulkopuolella tietokannan toimintoja (kuten linkitettyjä palvelimia agentti työ, FileStream, Service Brokeria jne.). Lisätietoja on artikkelissa [Azure SQL-tietokannan ohjeita ja rajoituksia](https://msdn.microsoft.com/library/azure/ff394102.aspx).

## <a name="1-tier-simple-single-virtual-machine"></a>1 ja tason (perus): yksi virtuaalikoneen

Tämän sovelluksen kuvion käyttöönottoa SQL Server-sovelluksen ja tietokannan erillinen virtual kone Azure-tietokannassa. Saman virtuaalikoneen sisältää asiakkaan/web-sovelluksen, business osia, tietojen käytön kerroksen ja tietokantapalvelimen. Esityksen, business ja tietojen valintanumero loogisesti erotettu mutta sijaitsevat fyysisesti yhden palvelimen tietokoneeseen. Useimmat asiakkaat alkavat sovelluksen tätä mallia ja valitse sitten ne Skaalaa lisäämällä useita web-roolien tai näennäiskoneiden niiden.

Tämän sovelluksen kuvion on kätevä seuraavissa tilanteissa:

- Haluat yksinkertainen siirto Azure ympäristössä, onko ympäristö vastauksia sovelluksen vaatimukset vai ei.

- Haluat säilyttää kaikki saman virtuaalikoneen saman Azure tietokeskuksen vähentää tasojen välissä viive ylläpidettävä sovelluksen tasoa.

- Haluat nopeasti valmistella kehittämisen ja testaa ympäristöissä ajan.

- Haluamasi toiminto kuormitus testaus eri työmäärää tasoja, mutta samanaikaisesti et halua omia ja ylläpitää useita fyysisten laitteiden aina.

Seuraavassa kaaviossa näytetään yksinkertainen paikallisen skenaario ja Azure-tietokannassa virtual yhteen tietokoneeseen sen käytössä cloud-ratkaisun käyttöönotto.

![tason 1-sovelluksen malli](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728008.png)

Käyttöönotto business kerroksen (liiketoimintalogiikan ja tietojen käyttöä osat) fyysinen samalla tasolla kuin esityksen kerroksen voit suurentaa sovelluksen suorituskykyä, ellei erillisessä taso vuoksi skaalattavuus tai suojaus on käytettävä.

Tämä on hyvin yleisiä määräytyy aluksi, hyödyllisiä seuraavassa artikkelissa-siirron, tietojen siirtäminen SQL Server-AM: [Siirtyminen tietokannalle SQL Server Azure-AM](virtual-machines-windows-migrate-sql.md).

## <a name="3-tier-simple-multiple-virtual-machines"></a>3 tason (perus): useita näennäiskoneiden

Tämän sovelluksen kuvion käyttöönottoa 3 taso-sovelluksen Azure lisäämällä sovelluksen kunkin tason virtual toiseen tietokoneeseen. Tämä on helppo asteikko ylöspäin ja skaalaus-kohtaa skenaarioissa joustavassa ympäristössä. Kun yksi virtual machine sisältää asiakkaan/web-sovelluksen, toinen isännöi business-osat ja sitten toiseen isännöi tietokantapalvelimen.

Tämän sovelluksen kuvion on kätevä seuraavissa tilanteissa:

- Haluat monimutkaisia tietokantasovelluksia, Azuren näennäiskoneiden siirto.

- Haluat toiseen sovellukseen tasoa, voit isännöidä eri alueilla. Esimerkiksi ehkä olet jakanut tietokannoissa, jotka on otettu käyttöön alueille raportointia varten.

- Haluat siirtää yrityksen paikallisen virtualisoidussa ympäristöjen Azuren näennäiskoneiden avulla. [Enterprise-sovellus on](https://msdn.microsoft.com/library/aa267045.aspx)artikkelissa yksityiskohtainen keskustelu on enterprise-sovellukset.

- Haluat nopeasti valmistella kehittämisen ja testaa ympäristöissä ajan.

- Haluamasi toiminto kuormitus testaus eri työmäärää tasoja, mutta samanaikaisesti et halua omia ja ylläpitää useita fyysisten laitteiden aina.

Seuraavassa kaaviossa näytetään, miten voit lisätä yksinkertaisen 3 taso-sovelluksen Azure lisäämällä sovelluksen kunkin tason virtual toiseen tietokoneeseen.

![3 taso-sovelluksen malli](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728009.png)

Tämän sovelluksen mallissa on vain yksi virtual machine (AM) kunkin tason. Jos sinulla on useita VMs Azure, on suositeltavaa määrittäminen virtual verkkoon. [Azure Virtual Network](../virtual-network/virtual-networks-overview.md) Luo luotettuna rajan ja mahdollistaa myös VMs tekstiviestintää keskenään yksityisen IP-osoitteen kautta. Lisäksi aina Varmista, että kaikki Internet-yhteydet Siirry vain esityksen taso. Kun seuraavan sovelluksen tätä mallia, hallita verkon suojaussäännöt hallintaa. Lisätietoja on artikkelissa [Salli ulkoinen käyttö oman AM Azure-portaalissa](virtual-machines-windows-nsg-quickstart-portal.md).

Kaavio Internet-protokollia voi olla TCP, UDP, HTTP tai HTTPS.

>[AZURE.NOTE] Virtuaalinen verkoston Azure-tietokannassa on maksutta. Voit kuitenkin perittävän VPN-yhdyskäytävän, joka yhdistää paikallisen. Tämä maksu perustuu ajan, yhteys on valmisteltu ja käytettävissä.

## <a name="2-tier-and-3-tier-with-presentation-tier-scale-out"></a>tason 2 ja 3 tason esityksen aikana taso asteikko-kohtaa

Tämän sovelluksen kuvion käyttöönottoa tason 2 tai 3 tason tietokantasovelluksen Azuren näennäiskoneiden, lisäämällä sovelluksen kunkin tason virtual toiseen tietokoneeseen. Lisäksi voit skaalata ulos esityksen tason vuoksi parantavat äänenvoimakkuuden asiakkaan pyynnöt.

Tämän sovelluksen kuvion on kätevä seuraavissa tilanteissa:

- Haluat siirtää yrityksen paikallisen virtualisoidussa ympäristöjen Azuren näennäiskoneiden avulla.

- Haluat skaalata ulos esityksen tason vuoksi parantavat äänenvoimakkuuden asiakkaan pyynnöt.

- Haluat nopeasti valmistella kehittämisen ja testaa ympäristöissä ajan.

- Haluamasi toiminto kuormitus testaus eri työmäärää tasoja, mutta samanaikaisesti et halua omia ja ylläpitää useita fyysisten laitteiden aina.

- Haluat omista infrastruktuuri-ympäristössä, joka voi skaalata ylös ja alas pyydettäessä.

Seuraavassa kaaviossa esitellään, miten voit soittaa sovelluksen tasoa Azure useita näennäiskoneiden laajentaminen esityksen tason vuoksi parantavat äänenvoimakkuuden asiakkaan pyynnöt. Kaavion tarkastelu Azure kuormituksen on vastuussa jakaminen liikenne yli useita näennäiskoneiden ja tarkistamalla myös, mitä verkkopalvelin muodostaa yhteyden. Esityksen tason suuren käytettävyyden varmistaa ottaa eri esiintymissä kuormituksen takana verkko-palvelimiin.

![Sovelluksen kuvio - esityksen taso asteikko ulos](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728010.png)

### <a name="best-practices-for-2-tier-3-tier-or-n-tier-patterns-that-have-multiple-vms-in-one-tier"></a>Tason 2, 3 tason tai n tason kuviot, joissa on useita VMs yhden tason parhaat käytännöt

On suositeltavaa sijoittaa näennäiskoneiden, jotka kuuluvat samalla tasolla saman pilvipalvelussa ja saman käytettävyys määrittäminen. Aseta esimerkiksi **CloudService1** ja **AvailabilitySet1** ja tietokannan palvelinten **CloudService2** ja **AvailabilitySet2**joukko joukon verkko-palvelimiin. Määritä Azure saatavuus avulla voit sijoittaa suuren käytettävyyden solmut erillisessä vika toimialueet ja Päivitä toimialueet.

Haluat hyödyntää useita AM esiintymiä taso, Määritä Azure kuormituksen sovelluksen tasojen välissä. Määrittämiseen ladata tasaustoiminto kunkin tason luoda kuormituksen päätepisteen kunkin tason VMs erikseen. Tietyn tason Luo VMs saman pilvipalvelussa. Näin varmistat, että heillä on sama julkisen Virtual IP-osoite. Seuraavaksi luodaan päätepisteen johonkin näennäiskoneiden kyseisen ylätasolla. Määrittää saman päätepisteen muiden näennäiskoneiden, kuormituksen tasaamisen, ylätasolla. Luomalla kuormituksen joukon liikenne jakaa useita näennäiskoneiden ja Salli ladata tasaustoiminto määrittämään solmun muodostetaan, kun Taustajärjestelmä AM solmu epäonnistuu. Esimerkiksi on useita kertoja kuormituksen takana WWW-palvelimien varmistaa esityksen tason suuren käytettävyyden.

Paras käytäntö Varmista aina, että kaikki internet-yhteydet Siirry ensin esityksen taso. Esityksen kerros käyttää business taso ja sitten business tason noutaa tiedot taso. Saat lisätietoja siitä, miten esityksen kerros annettavien [Salli ulkoinen käyttö oman AM Azure-portaalissa](virtual-machines-windows-nsg-quickstart-portal.md).

Huomaa, että Azure lataaminen-tasaustoiminto toimii samalla tavalla kuin kuormituksen paikallisen ympäristön tasaajiksi. Lisätietoja on artikkelissa [Azure infrastruktuuripalvelut kuormituksen](virtual-machines-windows-load-balance.md).

Lisäksi on suositeltavaa, että määrität yksityisverkon oman näennäiskoneiden Azure Virtual verkkoa käyttäen. Näin ne tekstiviestintää keskenään yksityisen IP-osoitteen kautta. Lisätietoja on artikkelissa [Azure Virtual Network](../virtual-network/virtual-networks-overview.md).

## <a name="2-tier-and-3-tier-with-business-tier-scale-out"></a>tason 2 ja 3 tason Businessin taso asteikko-kohtaa

Tämän sovelluksen kuvion käyttöönottoa tason 2 tai 3 tason tietokantasovelluksen Azuren näennäiskoneiden, lisäämällä sovelluksen kunkin tason virtual toiseen tietokoneeseen. Lisäksi voit haluta jakaa palvelimen osat useita näennäiskoneiden sovelluksen monimutkaisuuden vuoksi.

Tämän sovelluksen kuvion on kätevä seuraavissa tilanteissa:

- Haluat siirtää yrityksen paikallisen virtualisoidussa ympäristöjen Azuren näennäiskoneiden avulla.

- Haluat jakaa palvelimen osat useita näennäiskoneiden sovelluksen monimutkaisuuden vuoksi.

- Haluat siirtää business logiikan näkyvä paikallisen (-liiketoiminta) Toimialasovellusten Azuren näennäiskoneiden avulla. Toimialasovellusten ovat tärkeitä tietokoneen sovellukset, jotka ovat käytössä yrityksen, kuten laskenta, henkilöstöresurssien (HR), palkanlaskennan, toimistotarvikkeita ketju hallinta ja ERP-sovellukset.

- Haluat nopeasti valmistella kehittämisen ja testaa ympäristöissä ajan.

- Haluamasi toiminto kuormitus testaus eri työmäärää tasoja, mutta samanaikaisesti et halua omia ja ylläpitää useita fyysisten laitteiden aina.

- Haluat omista infrastruktuuri-ympäristössä, joka voi skaalata ylös ja alas pyydettäessä.

Seuraavassa kaaviossa näytetään paikallisen-skenaario ja sen käytössä cloud-ratkaisun. Tässä skenaariossa voit soittaa sovelluksen tasoa Azure useita näennäiskoneiden laajentaminen business taso, jossa on business logiikan taso ja tiedot access-komponentit. Kaavion tarkastelu Azure kuormituksen on vastuussa jakaminen liikenne yli useita näennäiskoneiden ja tarkistamalla myös, mitä verkkopalvelin muodostaa yhteyden. Suuren käytettävyyden business-tason varmistaa ottaa sovelluksen-palvelinten kuormituksen tasauspalvelun takana useita kertoja. Lisätietoja on artikkelissa [tason 2, 3 tason tai n tason sovelluksen kuviot, joka on yhden tason useita näennäiskoneiden parhaat käytännöt](#best-practices-for-2-tier-3-tier-or-n-tier-patterns-that-have-multiple-vms-in-one-tier).

![Sovelluksen kuvio ja business taso asteikko](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728011.png)

## <a name="2-tier-and-3-tier-with-presentation-and-business-tiers-scale-out-and-hadr"></a>tason 2 ja 3 tason esityksen ja business tasoa skaalaus-kohtaa ja HADR

Tämän sovelluksen kuvion käyttöönottoa tason 2 tai 3 tason tietokantasovelluksen, Azuren näennäiskoneiden jakamalla esityksen tason (verkkopalvelin) ja useita näennäiskoneiden business taso (sovelluspalvelin) osat. Lisäksi voit ottaa suuren käytettävyyden ja tietojen palauttaminen ratkaisuja Azuren näennäiskoneiden tietokannan.

Tämän sovelluksen kuvion on kätevä seuraavissa tilanteissa:

- Haluat siirtää yrityksen virtualisoidussa ympäristöjen paikallisen Azure SQL Serverin suuren käytettävyyden ja tietojen palauttaminen ominaisuuksia käyttämällä.

- Haluat skaalata esityksen taso ja parantavat äänenvoimakkuuden asiakkaan pyynnöt ja sovelluksen monimutkaisuuden vuoksi business-taso.

- Haluat nopeasti valmistella kehittämisen ja testaa ympäristöissä ajan.

- Haluamasi toiminto kuormitus testaus eri työmäärää tasoja, mutta samanaikaisesti et halua omia ja ylläpitää useita fyysisten laitteiden aina.

- Haluat omista infrastruktuuri-ympäristössä, joka voi skaalata ylös ja alas pyydettäessä.

Seuraavassa kaaviossa näytetään paikallisen-skenaario ja sen käytössä cloud-ratkaisun. Tässä skenaariossa voit skaalata ulos esityksen taso ja Azure useita näennäiskoneiden business osia. Lisäksi voit ottaa SQL Server-tietokannat suuri käytettävyys ja tietojen palauttaminen (HADR) tekniikat Azure-tietokannassa.

Käynnissä sovelluksen useita kopioita eri VMs Varmista, että olet kuormituksen pyynnöt kattavia. Jos sinulla on useita näennäiskoneiden, sinun täytyy Varmista, että kaikki VMs ovat käytettävissä ja sitä käytetään yhdessä vaiheessa ajassa. Jos määrität kuormituksen tasaamisen, Azure ladata tasaustoiminto seuraa VMs kunto ja ohjaa puhelut kunnossa toimii AM solmut oikein. Lisätietoja siitä, miten voit määrittää kuormituksen tasaamisen, näennäiskoneiden on artikkelissa [kuormituksen tasaaminen Azure infrastruktuuri-palveluiden](virtual-machines-windows-load-balance.md). On useita web- ja sovelluksen palvelinten kuormituksen tasauspalvelun takana esiintymiä varmistaa suuren käytettävyyden esityksen ja yrityksen tasoissa.

![Skaalaus-kohtaa ja suuri käytettävyys](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728012.png)

### <a name="best-practices-for-application-patterns-requiring-sql-hadr"></a>Sovelluksen kuviot vaatii SQL HADR parhaat käytännöt

Kun määrität SQL Server suuren käytettävyyden ja tietojen palauttaminen ratkaisut Azuren näennäiskoneiden, virtual verkoston oman [Azure Virtual verkon](../virtual-network/virtual-networks-overview.md) käyttäminen näennäiskoneiden on pakollinen.  Virtuaalinen verkossa oleville näennäiskoneiden on vakaata yksityinen IP-osoite senkin jälkeen, kun palvelua käyttökatkot, siten vältät DNS-nimenselvitys päivityksen aika. Lisäksi virtual verkon avulla voit laajentaa paikallisen verkon Azure ja luo luotettuna reunaa. Esimerkiksi jos sovellus on yrityksen toimialueen (kuten Windows-todennusta, Active Directory) rajoitukset, [Azure Virtual verkon](../virtual-network/virtual-networks-overview.md) määrittäminen on tarpeen.

Useimmat asiakkaat, joilla tuotannon koodi on käytössä Azure-ovat pitäminen ensisijainen ja toissijainen replikoiden Azure-tietokannassa.

Täydellinen tietojen ja opetusohjelmat suuren käytettävyyden ja tietojen palauttaminen avulla on artikkelissa [suuren käytettävyyden ja SQL Server Azuren näennäiskoneiden palauttaminen](virtual-machines-windows-sql-high-availability-dr.md).

## <a name="2-tier-and-3-tier-using-azure-vms-and-cloud-services"></a>tason 2 ja 3 tason Azure VMs ja pilvipalveluihin

Tämän sovelluksen kuvio-tason 2 tai 3 tason Azure sovelluksen käyttöönotto molemmat [Azure pilvipalveluihin](../cloud-services/cloud-services-choose-me.md#tellmecs) (Internetin kautta tai työntekijä roolit - ympäristö palveluna (PaaS)) avulla ja [Azuren näennäiskoneiden](virtual-machines-windows-about.md) (infrastruktuuri palveluna (IaaS)). Käyttäen [Azure pilvipalveluihin](https://azure.microsoft.com/documentation/services/cloud-services/) esityksen taso/business taso ja [Azuren](virtual-machines-windows-about.md) näennäiskoneiden SQL Server data tason on kätevä Azure useimmat sovellusten. Syynä on on käytössä pilvipalveluihin Laske-esiintymä sisältää asetusten hallinta käyttöönoton, seuranta ja skaalaus-kohtaa.

Pilvipalveluihin Azure ylläpitää infrastruktuuri, suorittaa säännöllisestä huollosta, korjaa käyttöjärjestelmät ja yrittää palauttaminen palvelu- ja virheet. Kun sovellus on asteikko-kohtaa, Automaattiset ja manuaaliset asteikko-kohtaa asetukset ovat käytettävissä cloud palvelun projektin suurenee tai pienenee esiintymiä tai näennäiskoneiden, jotka käyttävät sovelluksen määrän mukaan. Lisäksi voit ottaa käyttöön sovelluksen Azure cloud palvelun projektiin paikallisen Visual Studio.

Yhteenvedossa, jos et halua omista laaja hallintatehtäviä esityksen/yrityksille taso ja sovelluksen ei vaativat monimutkaisia kokoonpanoa, ohjelmiston tai käyttöjärjestelmä käyttää Azure pilvipalveluihin. Jos Azure SQL-tietokanta ei tue kaikkia ominaisuuksia hakua, käytä SQL Server Azure Virtual Machine tietojen tason. Sovelluksen suorittaminen Azure pilvipalveluihin ja tietojen tallentaminen Azuren näennäiskoneiden yhdistää molempia etuja. Saat yksityiskohtaiset vertailu tämän artikkelin perustuvat [Comparing Azure-tietokannassa](#comparing-web-development-strategies-in-azure).

Tämän sovelluksen kuvion esityksen taso sisältää web-rooli, joka on käynnissä Cloud Services-osan Azure suorittamisen-ympäristössä, ja se on mukautettu web application programming kuin tukemat IIS- ja ASP.NET. Yrityksen tai Taustajärjestelmä taso sisältää työntekijän rooli, joka on käynnissä Cloud Services-osan Azure suorittamisen-ympäristössä, ja se on hyödyllinen generalized kehittämisen ja voi suorittaa käsittelyn taustan web-roolin. Tietokannan tason sijaitsee SQL Server-virtual tietokoneeseen Azure-tietokannassa. Esityksen taso ja tietokannan tason välisen tapahtuu suoraan tai business taso-Työntekijä roolin osien päälle.

Tämän sovelluksen kuvion on kätevä seuraavissa tilanteissa:

- Haluat siirtää yrityksen virtualisoidussa ympäristöjen paikallisen Azure SQL Serverin suuren käytettävyyden ja tietojen palauttaminen ominaisuuksia käyttämällä.

- Haluat omista infrastruktuuri-ympäristössä, joka voi skaalata ylös ja alas pyydettäessä.

- Azure SQL-tietokanta ei tue kaikkia ominaisuuksia, jotka sovelluksen tai tietokanta on.

- Haluamasi toiminto kuormitus testaus eri työmäärää tasoja, mutta samanaikaisesti et halua omia ja ylläpitää useita fyysisten laitteiden aina.

Seuraavassa kaaviossa näytetään paikallisen-skenaario ja sen käytössä cloud-ratkaisun. Tässä skenaariossa Aseta esitys tason web-roolien työntekijä roolit business-taso, mutta tietojen tason näennäiskoneiden Azure-tietokannassa. Käynnissä esityksen tason useita kopioita eri sivustoon roolit varmistaa voivat ladata ne yli. Kun yhdistät Azure pilvipalveluihin Azuren näennäiskoneiden kanssa, on suositeltavaa, että määrität [Azure Virtual Network](../virtual-network/virtual-networks-overview.md) sekä. [Azure Virtual verkoston](../virtual-network/virtual-networks-overview.md)kanssa voi olla vakaata ja pysyvät yksityinen IP-osoitteet samaan pilvipalvelussa pilveen. Kun virtual verkon määrittäminen oman näennäiskoneiden ja cloud services, he voivat aloittaa viestintä keskenään yksityisen IP-osoitteen kautta. Lisäksi on näennäiskoneiden ja Azure web/työntekijä roolit samassa [Azure Virtual Network](../virtual-network/virtual-networks-overview.md) on pieni viive ja turvallisempi yhteys. Lisätietoja on artikkelissa [pilvipalveluun ominaisuudet](../cloud-services/cloud-services-choose-me.md).

Kaavion tarkastelu Azure kuormituksen jakaa useita näennäiskoneiden liikenteen ja määrittää myös WWW-palvelimessa tai sovelluksen, johon haluat muodostaa yhteyden. Ottaa eri esiintymissä Internetin kautta tai sovelluksen-palvelinten kuormituksen tasauspalvelun takana varmistaa suuren käytettävyyden esityksen taso ja business-taso. Lisätietoja on artikkelissa [sovelluksen kuviot vaatii SQL HADR parhaat käytännöt](#best-practices-for-application-patterns-requiring-sql-hadr).

![Sovelluksen kuviot pilvipalveluihin](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728013.png)

Toteuttamisesta tämän sovelluksen kuvion toinen vaihtoehto on käyttää kootut web-rooli, joka sisältää esityksen taso ja business osia seuraavassa kaaviossa esitetyllä tavalla. Tämän sovelluksen kuvion on hyötyä sovelluksissa, jotka edellyttävät tilallisten rakenne. Koska Azure tarjoaa tilattomien Laske solmujen Internetin kautta tai työntekijä roolit, on suositeltavaa toteuttaa logiikan tallentamiseen istunnon tila jollakin seuraavia tekniikoita: [Azure välimuistiin](https://azure.microsoft.com/documentation/services/redis-cache/), [Azure-taulukkotallennus](../storage/storage-dotnet-how-to-use-tables.md) tai [Azure SQL-tietokantaan](../sql-database/sql-database-technical-overview.md).

![Sovelluksen kuviot pilvipalveluihin](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728014.png)

## <a name="pattern-with-azure-vms-azure-sql-database-and-azure-app-service-web-apps"></a>Kuvion Azure VMs, Azure SQL-tietokanta ja Azure App Service (online)

Tämän sovelluksen kuvion on ensisijainen, kuinka voit yhdistää Azure infrastruktuurin service (IaaS)-osina Azure ympäristö nimellä--palvelun osia (PaaS)-ratkaisu. Tämä kaava on kohdistettu Azure SQL-tietokanta relaatio tietojen tallentamista varten. Se Sisällytä SQL Server Azure virtuaalikoneen, joka on osa nimellä palvelun tarjoaa Azure-infrastruktuuria.

Tämän sovelluksen kohdalla käyttöönottoa ja Azure tietokantasovelluksen siten esityksen ja business-tasoa saman virtuaalikoneen ja käyttävät tietokantaa Azure SQL-tietokanta (SQL-tietokanta) palvelimissa. Voit toteuttaa esityksen tason käyttämällä perinteinen IIS-pohjainen web-ratkaisuja. Tai [Azure Web Apps -sovellusten](https://azure.microsoft.com/documentation/services/app-service/web/)avulla voit toteuttaa yhdistetyn esityksen ja business taso.

Tämän sovelluksen kuvion on kätevä seuraavissa tilanteissa:

- Sinulla on jo olemassa olevan määritetty Azure SQL-tietokanta palvelimen ja testattava sovelluksen nopeasti.

- Haluat testata Azure-ympäristön ominaisuuksia.

- Haluat nopeasti valmistella kehittämisen ja testaa ympäristöissä ajan.

- Yrityksen tiedot access komponentit voi olla itsenäinen web-sovelluksen.

Seuraavassa kaaviossa näytetään paikallisen-skenaario ja sen käytössä cloud-ratkaisun. Tässä skenaariossa Aseta virtual yhteen tietokoneeseen Azure SQL-tietokannan tietojen Azure ja access-sovelluksen-tasoa.

![Erilaiset sovelluksen malli](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728015.png)

Jos valitset toteuttamisesta yhdistetyn web- ja sovelluksen taso Azure Web Apps-sovellusten avulla, on suositeltavaa säilyttää keskimmäisen tai sovelluksen tason kuin DLL-kirjastojen (dll) web-sovelluksen yhteydessä.

Lisäksi voit tarkastella lisätietoja ohjelmointi tekniikoita [Comparing web kehittämisen strategiat Azure-tietokannassa](#comparing-web-development-strategies-in-azure) on tämän artikkelin lopussa olevassa kohdassa suositusten.

## <a name="n-tier-hybrid-application-pattern"></a>N-taso hybrid sovelluksen malli

N-taso hybrid sovelluksen kohdalla, valitse Toteuta useita tasoa jaetaan kesken paikallisen ja Azure-sovelluksen. Tämän vuoksi joustava ja Uudelleenkäytettävän hybrid-järjestelmän luominen, jossa voit muokata tai lisätä tietyn taso muuttamatta muiden tasoa. Laajenna samassa yritysverkossa pilvipalveluun, voit käyttää [Azure Virtual](../virtual-network/virtual-networks-overview.md) verkkopalvelu.

Hybrid tämän sovelluksen kuvion on kätevä seuraavissa tilanteissa:

- Haluat luoda sovelluksia, jotka suoritetaan osittain pilvipalvelussa ja osittain paikallisen.

- Haluat siirtää sovellukseen paikallisen pilveen joistakin tai kaikista osista.

- Haluat yrityksen Siirry Azure paikallisen virtualisoidussa ympäristöissä.

- Haluat omista infrastruktuuri-ympäristössä, joka voi skaalata ylös ja alas pyydettäessä.

- Haluat nopeasti valmistella kehittämisen ja testaa ympäristöissä ajan.

- Haluat kannattava tapa ottaa yrityksen tietokantasovelluksia varmuuskopiot.

Seuraavassa kaaviossa näytetään n tason hybrid sovelluksen kuviota, paikallisen ja Azure laajuinen. Kaavion esitetyllä paikallisen infrastruktuuri sisältää [Active Directory-toimialueen palveluista](https://technet.microsoft.com/library/hh831484.aspx) toimialueen ohjauskoneen tukemaan käyttäjän todennus- ja. Huomaa, että kaavio osoittaa tilanne, jossa tietojen tason joitakin osia live keskuksessa paikallisen tietojen taas joissakin tietojen tason live Azure. Sovelluksen tarpeiden mukaan voit toteuttaa useita hybrid skenaarioita. Voi esimerkiksi säilyttää esityksen taso ja business-tason paikallisen ympäristön, mutta tietojen tason Azure-tietokannassa.

![N-taso-sovelluksen malli](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728016.png)

Azure-tietokannassa voit käyttää Active Directory erillinen cloud hakemistona organisaation tai voit integroida aiemmin paikallisen Active Directory [Azure Active](https://azure.microsoft.com/documentation/services/active-directory/)Directory-hakemistosta. Kaavion tarkastelu business osia voi käyttää useisiin tietolähteisiin, kuten [SQL Server Azure](virtual-machines-windows-sql-server-iaas-overview.md) kautta yksityisen sisäinen IP-osoitteen, paikallisen SQL Server [Azure Virtual verkon](../virtual-network/virtual-networks-overview.md)kautta tai [SQL-tietokannan](../sql-database/sql-database-technical-overview.md) käyttöä .NET Framework data provider toiminnot. Tässä kaaviossa Azure SQL-tietokanta on valinnainen tiedot-tallennuspalvelu.

N-taso hybrid sovelluksen kohdalla voit toteuttaa seuraavat työnkulun seuraavassa järjestyksessä:

1. Tunnista yrityksen tietokantasovelluksia, joiden täytyy voidaan siirtää pilveen käyttämällä [Microsoft Assessment and Planning (MAP)-työkalut](http://microsoft.com/map). KARTTA-työkalujen kerää varasto ja suorituskyvyn Virtualizationin harkitset tietokoneista ja neuvoo kapasiteetin ja arviointi suunnittelu.

1. Suunnittele resurssit ja Azure alustan, kuten tallennustilan asiakkaat ja näennäiskoneiden tarvittavat määritykset.

1. Määritä verkkoyhteys yritysverkon paikallisen ja [Azure Virtual verkon](../virtual-network/virtual-networks-overview.md)välillä. Määrittämään yritysverkon paikallisen ja Azure-tietokannassa virtual kone välinen yhteys jollakin seuraavista tavoista:

    1. Muodosta yhteys paikalliseen ja Azure kautta julkisen päätepisteestä virtual tietokoneeseen Azure-tietokannassa. Tämä menetelmä on helppo asennus ja avulla voit käyttää SQL Server-todennusta virtuaalikoneen. Lisäksi määrittää verkon suojauksen ryhmän säännöt AM julkisen liikenteen hallinta. Lisätietoja on artikkelissa [Salli ulkoinen käyttö oman AM Azure-portaalissa](virtual-machines-windows-nsg-quickstart-portal.md).

    1. Muodosta yhteys paikalliseen ja Azure Azure Virtual yksityisen verkon (VPN) tunnelin kautta. Tätä menetelmää voit laajentaa toimialueen käytäntöjen virtual koneeseen Azure-tietokannassa. Voit lisäksi määrittäminen palomuurisäännöt ja käytä Windows-todennusta virtuaalikoneen. Azure tukee tällä hetkellä suojatun sivuston sivuston VPN-yhteyden ja pisteen sivuston VPN-yhteydet:

        - Suojattu sivusto yhteys voit muodostaa Azure verkkoyhteyden paikallisen verkon ja virtual verkon välille. On suositeltavaa ympäristösi paikallisen center muodostamisesta Azure.

        - Suojatun pisteen sivuston yhteys voit muodostaa verkkoyhteyden virtual verkon Azure ja suoriteta missään yksittäisten tietokoneiden välillä. Suositellaan enimmäkseen kehittämisen ja testaa varten.

    Lisätietoja siitä, miten voit muodostaa yhteyden SQL Server Azure-tietokannassa on ohjeaiheessa [yhteyden, SQL Server Virtual tietokoneen Azure](virtual-machines-windows-classic-sql-connect.md).

1. Määrittää ajoitetuissa ja ilmoituksia, jotka varmuuskopioida paikallisen virtuaalikoneen DVD-levyllä Azure-tietokannassa. Lisätietoja [SQL Serverin varmuuskopiointi- ja Azure Blob Storage-palveluun](https://msdn.microsoft.com/library/jj919148.aspx) ja [Varmuuskopiointi- ja SQL Server Azuren näennäiskoneiden](virtual-machines-windows-sql-backup-recovery.md).

1. Sovelluksen tarpeiden mukaan voit toteuttaa jokin seuraavista kolmesta yleiset skenaariot:

    1. Voit pitää Azure-tietokantapalvelin verkkopalvelin, sovelluspalvelin ja joiden tietoja taas luottamuksellisia tietoja paikallisen säilyttää.

    1. Voit pitää verkkosivustoon ja sovelluspalvelin paikallisten olisi tietokantapalvelimen virtual kone Azure-tietokannassa.

    1. Voit pitää tietokantapalvelimeen, verkkopalvelin ja sovelluspalvelin paikallisen olisi pidät Tietokantareplikoita näennäiskoneiden Azure-tietokannassa. Tämän asetuksen avulla paikallisen WWW-palvelimien tai raportointisovelluksissa käyttämään Tietokantareplikoita Azure-tietokannassa. Sen vuoksi voit toteuttaa alentaa paikallisen tietokannan työmäärää. On suositeltavaa toteuttaa Tämä skenaario näkyvä lukea toiminnoista ja kehityshäiriöitä varten. Lisätietoja Azure Tietokantareplikoita luomisesta on artikkelissa AlwaysOn käytettävyys ryhmät [suuren käytettävyyden](virtual-machines-windows-sql-high-availability-dr.md)ja SQL Server Azuren näennäiskoneiden palauttaminen.

## <a name="comparing-web-development-strategies-in-azure"></a>Vertailu web kehittämisen strategiat Azure-tietokannassa

Toteuta ja otetaan käyttöön monitasoisten SQL Server-pohjaisesta sovelluksesta Azure-tietokannassa, voit käyttää kaksi ohjelmoinnin seuraavista tavoista:

- Määritä Azure ja access-tietokannan SQL Serverin Azuren näennäiskoneiden perinteinen verkkopalvelin (IIS - Internet Information Services).

- Toteuta ja ota käyttöön Azure pilvipalveluun. Varmista sitten, että tämä pilvipalvelussa käyttää tietokantojen SQL Server Azure Virtual koneet. Pilvipalveluun, voit lisätä useita verkko- ja työntekijä roolit.

Seuraavassa taulukossa on perinteinen web-kehitys Azure Pilvipalveluiden ja Azure Web Apps-sovelluksista, jotka koskevat SQL Server Azuren näennäiskoneiden vertailu. Taulukko sisältää Azure Web Apps-sovellusten, kuten on mahdollista, jotta voit käyttää SQL Server Azure AM tietolähteenä Azure Web Apps sen julkinen virtual IP-osoite tai DNS-nimi.

||Perinteinen web kehittäminen Azuren näennäiskoneiden|Azure-tietokannassa pilvipalveluihin|WWW-isännöinnin Azure Web Apps-sovellusten kanssa|
|---|---|---|---|
|**Paikallisen sovelluksen siirtyminen**|Aiemmin luotujen sovellusten nimellä-on.|Sovellukset on Internetin kautta tai työntekijä roolit.|Aiemmin luotujen sovellusten nimellä-sopii mutta hyödyntää itsenäinen web-sovellusten ja WWW-palvelut, jotka tarvitsevat nopeasti skaalattavuus.|
|**Kehittäminen ja käyttöönotto**|Visual Studio, WebMatrix, Visual Web Developer, WebDeploy, FTP, TFS, IIS Manager-PowerShell.|Visual Studio Azure SDK-paketissa, TFS, PowerShell. Kunkin pilvipalvelussa on ympäristöissä, johon voit ottaa palvelupakettiin ja määritykset: testaus- ja. Voit ottaa pilvipalveluun testaa se ennen edistää tuotannon väliaikaisen-ympäristöön.|Visual Studio, WebMatrix, Visual Web Developer, FTP, GIT, BitBucket, CodePlex, Dropboxin, GitHub, Mercurial, TFS, Web-käyttöön, PowerShell.|
|**Hallinta ja asetukset**|Olet vastuussa hallintatehtäviä sovelluksen, tiedot, palomuurisäännöt, VPN ja käyttöjärjestelmän.|Olet vastuussa hallintatehtäviä sovelluksen, tiedot, palomuurisäännöt ja VPN.|Olet vastuussa hallintatehtäviä sovelluksen ja vain tiedot.|
|**Suuri käytettävyys ja palauttaminen (HADR)**|On suositeltavaa sijoittaa näennäiskoneiden käytettävyys samat ja saman pilvipalvelussa. Oman VMs pitäminen saman käytettävyys määrittäminen avulla Azure suuren käytettävyyden solmut erillisessä vika toimialueille ja päivittää toimialueet. Vastaavasti pitäminen oman VMs saman pilvipalvelussa mahdollistaa kuormituksen tasaamisen ja VMs viestiä suoraan yhteydessä toisiinsa sisällä Azure tietokeskuksen paikallisen verkon kautta.<br/><br/>Olet vastuussa käyttöönoton suuren käytettävyyden ja tietojen palauttamisen ratkaisu SQL Server-Azuren näennäiskoneiden minkä tahansa käyttökatkot välttämiseksi. Katso tuetut HADR technologies [suuren käytettävyyden ja SQL Server Azuren näennäiskoneiden palauttaminen](virtual-machines-windows-sql-high-availability-dr.md).<br/><br/>Olet vastuussa Omat tiedot ja sovelluksen varmuuskopioiminen.<br/><br/>Azure voit siirtää oman näennäiskoneiden, jos tietokeskuksen host-koneen epäonnistuu laitteisto-ongelmien vuoksi. Lisäksi voi olla oman AM suunnitellun käyttökatkot host tietokoneen päivittämisestä suojaus tai ohjelmiston päivitykset. Tästä syystä suosittelemme, että sinulla on vähintään kaksi VMs sovelluksen kunkin tason jatkuva saatavuus. Azure tarjoa SLA virtual yhteen tietokoneeseen. Lisätietoja on artikkelissa [Azure vikasietoisuudelle teknisiä ohjeita](../resiliency/resiliency-technical-guidance.md).|Azure hallitsee virheet johtuvat pohjana olevan laitteen tai käyttöjärjestelmä ohjelmiston. Suosittelemme, että otat verkossa tai työntekijän rooli, jotta sovelluksesi suuren käytettävyyden useita kertoja. Lisätietoja on artikkelissa [pilvipalveluihin, näennäiskoneiden, ja Virtual verkon tason palvelusopimus](http://www.microsoft.com/download/details.aspx?id=38427) ja [palauttaminen ja Azure sovellusten suuri käytettävyys](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)<br/><br/>Olet vastuussa Omat tiedot ja sovelluksen varmuuskopioiminen.<br/><br/>Tietokantojen asuvat-Azure-AM SQL Server-tietokantaan olet vastuussa käyttöönoton suuri käytettävyys ja tietojen palauttaminen ratkaista minkä tahansa käyttökatkot välttämiseksi. Katso tuetut HDAR technologies SQL Serverin Azuren näennäiskoneiden suuren käytettävyyden ja palauttaminen.<br/><br/>**SQL Server-tietokantapeilausta**: Käytä Azure pilvipalveluihin (web/työntekijä roolit). SQL Server VMs ja cloud palvelun projektin voi olla samassa Azure Virtual verkossa. SQL Server AM ei ole samassa Virtual verkossa, jos haluat luoda SQL Server-tunnuksen viestintä reitittämiseen SQL Server-esiintymän. Lisäksi alias-nimen on oltava sama SQL Server-nimi.|Suuren käytettävyyden periytyy Azure työntekijä roolit, Azure-blob-säiliö ja Azure SQL-tietokantaan. Esimerkiksi Azuren tallennustilaan ylläpitää kolme replikoiden blob, taulukon ja jonon tiedot. Tiettynä hetkenä Azure SQL-tietokanta säilyttää tiedot, jossa on kolme replikoita – replikaan ensisijainen ja toissijainen replikat. Lisätietoja on artikkelissa [Azure-tallennustilan](https://azure.microsoft.com/documentation/services/storage/) ja [Azure SQL-tietokantaan](../sql-database/sql-database-technical-overview.md).<br/><br/>Kun käytät SQL Server Azure AM tietolähteenä Azure Web Apps-sovellukset, ota huomioon Azure online ei tue Azure Virtual Network. Toisin sanoen kaikki yhteydet Azure Web sovelluksista SQL Server-VMs Azure-tietokannassa on käymällä läpi julkisen päätepisteestä näennäiskoneiden on. Tämä voi aiheuttaa suuren käytettävyyden ja tietojen palauttaminen skenaarioiden joitakin rajoituksia. Esimerkiksi yhteyden muodostaminen SQL Server AM tietokantapeilauksen avulla Azure Web Apps-asiakassovellus ei olisi voi muodostaa uusi ensisijainen palvelimeen tietokantapeilausta vaatii määrittäminen Azure Virtual Network välillä SQL Server host VMs Azure-tietokannassa. Vuoksi käyttäminen Azure verkkosovelluksissa **SQL Server-tietokantapeilausta** ei voi tällä hetkellä.<br/><br/>**SQL Server AlwaysOn käytettävyys-ryhmät**: Voit määrittää AlwaysOn käytettävyys ryhmät käytettäessä Azure verkkosovelluksissa SQL Server-VMs Azure-tietokannassa. Mutta sinun on määritettävä AlwaysOn käytettävyys ryhmän Listener tietoliikenteen reitittämiseen ensisijainen se julkisen kuormituksen päätepisteet kautta.|
|**Usean tiloissa yhteys**|Voit muodostaa yhteyden paikallisen Azure Virtual Network.|Voit muodostaa yhteyden paikallisen Azure Virtual Network.|Azure Virtual Network tuetaan. Lisätietoja on artikkelissa [Web Apps Virtual verkon integrointi](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/).|
|**Skaalattavuus**|Skaalaa ylöspäin on virtuaalikoneen koon kasvattaminen tai lisäämällä enemmän levyjä. Saat lisätietoja virtuaalikoneen koosta [Virtuaalikoneen koot Azure](virtual-machines-windows-sizes.md).<br/><br/>**Tietokannan palvelin**: skaalaus-kohtaa on saatavana tietokannan osioinnin tekniikoita ja SQL Server AlwaysOn käytettävyys ryhmät.<br/><br/>Saat paljon luku-toiminnoista voit käyttää [AlwaysOn käytettävyys ryhmät](https://msdn.microsoft.com/library/hh510230.aspx) useita toissijainen solmujen sekä SQL Server-replikointi.<br/><br/>Paksu kirjoitus-toiminnoista voit toteuttaa vaakasuuntainen osioinnin tietojen useita fyysinen palvelinten antamaan sovelluksen asteikko-kohtaa.<br/><br/>Lisäksi voit toteuttaa käyttämällä [SQL Serverin tietoihin riippuvaiset reititys](https://technet.microsoft.com/library/cc966448.aspx)asteikko-kohtaa. Kanssa tietojen riippuvaiset reititys (DDR), sinun täytyy toteuttavien osioinnin järjestelmä asiakassovellus, yleensä business taso-kerros-tietokanta-pyyntöjen reitittämiseen useita SQL Server-solmut. Liiketoiminta-taso sisältää yhdistämismääritykset solmun sisältää tiedot, ja kuinka tiedot on osioitu.<br/><br/>Voit skaalata näennäiskoneiden käynnissä olevat sovellukset. Lisätietoja on artikkelissa [miten sovelluksen](../cloud-services/cloud-services-how-to-scale.md).<br/><br/>**Tärkeä huomautus**: Azure **Automaattinen skaalaus** -ominaisuuden avulla voit automaattisesti suurentaa tai pienentää näennäiskoneiden, jota käytetään sovelluksen mukaan. Tämä ominaisuus takaa, että käyttäjäkokemuksen ei vaikuta heikentää aikana ja VMs ovat suljetaan, kun demand on pieni. On suositeltavaa, että et määritä Automaattinen skaalaus-vaihtoehdon cloud-palvelun siinä on SQL Server VMs. Syy on, että Automaattinen skaalaus-toiminnon avulla voit ottaa virtual tietokoneeseen, kun suorittimen käyttö-, AM on suurempi kuin kynnysarvo joitakin ja virtual machine käytöstä, kun suorittimen käyttö on pienempi kuin se Azure. Automaattinen skaalaus-ominaisuudesta on hyötyä tilattomien sovellusten, kuten WWW-palvelimien, missä tahansa AM voit hallita työmäärää ilman mitään edelliseen tilaan viittauksia. Automaattinen skaalaus-toiminto ei ole kuitenkin hyötyä tilallisten sovellusten, kuten SQL Server, jossa vain yksi sallii tietokannan kirjoittaminen.|Asteikon ylöspäin on useita web- ja työntekijä rooleja käyttämällä. Saat lisätietoja virtuaalikoneen koot web roolit ja työntekijä roolit [Pilvipalveluihin määrittäminen koot](../cloud-services/cloud-services-sizes-specs.md).<br/><br/>Kun **Cloud Services-palvelujen**avulla voit määrittää useita rooleja jakaa käsittely ja saavuttamiseksi myös joustavia skaalaus-sovelluksen. Kunkin pilvipalvelussa sisältää vähintään yhden web roolit ja/tai työntekijä roolit, joissa oma sovelluksen tiedostoista ja määritykset. Voit skaalaus-up pilvipalveluun suurentamalla roolin esiintymät (näennäiskoneiden) määrän käyttöön roolin ja skaalaus-luettelo pilvipalveluun pienentämällä roolin esiintymien määrän. Lisätietoja on artikkelissa [Azure suorittamisen mallit](../cloud-services/cloud-services-choose-me.md).<br/><br/>Skaalaa-kohtaa on valmiita Azure suuren käytettävyyden tukea [pilvipalveluihin, näennäiskoneiden, ja Virtual verkon tason palvelusopimus](http://www.microsoft.com/download/details.aspx?id=38427) ja kuormituksen kautta.<br/><br/>Monitasoisten-sovelluksen on suositeltavaa liittyä web/työntekijä roolit sovelluksen tietokantapalvelin VMs Azure Virtual verkon kautta. Lisäksi Azure on kuormituksen VMs varten samaan pilvipalvelussa levittäminen pyynnöt niiden päälle. Tällä tavalla yhteydessä näennäiskoneiden viestiä suoraan yhteydessä toisiinsa sisällä Azure tietokeskuksen paikallisen verkon kautta.<br/><br/>Voit määrittää **Automaattinen skaalaus** -Azure perinteinen ‑portaaliin sekä aikataulun ajat. Lisätietoja on artikkelissa [miten sovelluksen](../cloud-services/cloud-services-how-to-scale.md).|**Skaalaa ylös ja alas**: voit voit suurentaa tai pienentää kokoa varattu sivuston esiintymän (AM).<br/><br/>Mittakaava,: Lisää varattu esiintymät (VMs) voit lisätä sivuston.<br/><br/>Voit määrittää **Automaattinen skaalaus** -portaalin sekä aikataulu-aikoja. Katso lisätietoja, [miten asteikko Web Apps -sovellusten](../app-service-web/web-sites-scale.md).|

Katso lisätietoja näiden selaimessa valita [Azure Web Apps-sovellusten ja pilvipalveluihin VMs: kumpaa käyttää](../app-service-web/choose-web-site-cloud-service-vm.md).

## <a name="next-steps"></a>Seuraavat vaiheet

Katso lisätietoja suorittamisesta SQL Server Azure Virtual koneet, [SQL Server Azure näennäiskoneiden yhteenvedossa](virtual-machines-windows-sql-server-iaas-overview.md).

<properties
   pageTitle="Mikä on Azure SQL-tietovarasto? | Microsoft Azure"
   description="Yritysluokan jaettu tietokanta voi käsittelyn relaatio- ja relaatio petabyte tietomääristä. Alan ensimmäisen cloud tietovarasto Suurenna kanssa on Pienennä ja keskeyttää sekunnin kuluttua."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/27/2016"
   ms.author="lodipalm;barbkess;mausher;jrj;sonyama;kevin"/>


# <a name="what-is-azure-sql-data-warehouse"></a>Mikä on Azure SQL-tietovarasto?

Azure SQL-tietovarasto on voi käsittelyn valtaviin tietomäärien, Relaatio- ja relaatio tietokanta pilvipohjainen, asteikko-kohtaa. Rakennettu Tutustu erittäin rinnakkain käsittely (MPP)-arkkitehtuuri SQL-tietovarasto voit käsitellä yrityksen työmäärää.

SQL-tietovarasto:

- Yhdistää relaatiotietokannasta SQL Server Azure cloud asteikko-kohtaa ominaisuuksia. Voit suurentaa, pienentää, Keskeytä tai Jatka Laske sekunnin kuluttua. Voit tallentaa kustannukset skaalaus suorittimen, kun tarvitset niitä hiiren kakkospainikkeella ja leikkaaminen takaisin käyttö huippu aikoina.
- Hyödyntää Azure ympäristössä. Se on helppo ottaa käyttöön, saumattomasti ylläpitää ja vika täysin automaattinen varmuuskopioita vuoksi varmatoimisempia.
- Täydentää SQL Server-ekosysteemiin. Voit kehittää tuttuja SQL Server Transact-SQL (T-SQL) ja työkalut.

Tässä artikkelissa kuvataan SQL-tietovarasto ominaisuuksia.

## <a name="massively-parallel-processing-architecture"></a>Erittäin rinnakkain käsittely-arkkitehtuuri

SQL-tietovarasto on erittäin rinnakkain käsittely (MPP) jakaa tietokannan. Tietojen jakaminen ja käsitteleminen ominaisuuksien useita solmujen, SQL-tietovarasto voit tarjota erittäin suuri skaalattavuus - paljon muutakin kuin mikä tahansa yksittäinen järjestelmä.  SQL-tietovarasto näkymät taustalla, tiedot näkyviin määrä jaettu mitään tallennustilan ja käsittelyn yksiköt. Tiedot on Premium paikallisesti tarpeettomat tallennustilaan tallennettuja ja linkittää Laske solmujen kyselyn suorittamista varten. SQL-tietovarasto siirtyä tämän rakenteen "Jaa ja nyt jäsentää" menetelmä Lataa ja monimutkaisia kyselyjä. Pyynnöt vastaanottanut ohjausobjektin-solmu, optimoitu ja sitten Laske solmut työssään rinnakkain siirretään.

MPP-arkkitehtuuri ja Azure tallennustilan ominaisuuksia yhdistämällä SQL-tietovarasto tehdä seuraavia toimia:

- Suurenna tai Pienennä tallennustilan Laske riippumatta.
- Suurenna tai Pienennä Laske siirtämättä tiedot.
- Keskeytä Laske kapasiteetin ja säilyttää tiedot ennalleen.
- Ansioluettelon laskemiseen kapasiteetin jonkin aikaa ilmoitus-palvelussa.

Seuraavassa kaaviossa on esitetty arkkitehtuuri tarkemmin.

![SQL Data Warehouse arkkitehtuuri][1]


**Ohjausobjektin solmu:** Ohjausobjektin-solmu hallitsee ja optimoi kyselyt. On edusta appsien kanssa kaikki sovellukset ja yhteydet. SQL-tietovarasto ohjausobjektin-solmu on tarjoaa SQL-tietokantaan, ja yhteyden siihen näyttää ja tuntuu sama. Ohjausobjektin solmu koordinoi pinta, valitse kaikki tietojen siirto ja pitää suorittaa rinnakkaisia kyselyjen hajautettu tietojen laskenta. T-SQL-kyselyn SQL-tietovarasto lähettäessäsi ohjausobjektin solmu muuntaa sen erilliset kyselyt, jotka toimivat kunkin Laske solmun rinnakkain.

**Laske solmujen:** Laske-solmut yhteyshenkilönä takana SQL-tietovarasto potenssiin. He ovat SQL-tietokantoja, joka tallentaa tiedot ja käsitellä kyselyn. Kun olet lisännyt tiedot, SQL-tietovarasto jakaa Laske solmujen rivit. Laske-solmut ovat työntekijöiden rinnakkain kyselyt käynnissä olevat tiedot. Käsitellyt välittävät takaisin ohjausobjektin solmu tulokset. Kun haluat lopettaa kyselyn, hallinta-solmu koostaa tulokset ja palauttaa lopputuloksen.

**Tallennustilan:** Tiedot tallennetaan Azure-Blob-säiliö. Kun Laske solmujen käsittelyä varten, kirjoittaminen ja lukea suoraan ja Blob-objektien tallennustilaan. Koska Azure tallennustilan laajenee läpinäkyvä ja suojausta, SQL-tietovarasto voit tehdä saman. Koska suorittaminen ja tallennustilaa riippumaton, SQL-tietovarasto automaattisesti skaalata tallennustilan erillään skaalauksen suorittaminen ja päinvastoin. Azure-Blob-säiliö on myös täysin vikasietoisia ja nopeuttaa varmuuskopiointi ja palauttaminen.

**Tietojen siirto-palvelu:** Tietojen siirtämistä Service (DMS) siirtää tietoja solmujen välillä. DMS kautta Laske solmujen käsiksi tietoihin, jotka he tarvitsevat liitokset ja koosteet. DMS ei ole Azure palvelu. Windows-palvelun, joka suoritetaan kaikissa solmuissa rinnalla SQL-tietokanta on. Koska DMS suoritetaan taustalla, voit ei käsitellä sitä suoraan. Kuitenkin kun tarkastelet kyselyn suunnitelmat, huomaat, että ne sisältävät DMS joitakin toimintoja, koska tietojen siirto on tarvittavat kunkin Queryä rinnakkain.


## <a name="optimized-for-data-warehouse-workloads"></a>Optimoitu data warehouse toiminnoista

MPP-vaihtoehto on tukea saavat hankkeet tietovaraston suorituskykyä optimointi, mukaan lukien määrä:

- Jaettu kysely optimointitoiminnon ja monimutkaisia tilasto yli kaikki tiedot. Tietojen avulla tietojen koko ja jakelu palvelu ei voi optimoida kyselyitä varten arvioidaan kustannukset tietyn jaettu kysely-toimintoa.

- Lisäasetukset algoritmit ja -tekniikoiden integroitu tietojen siirto prosessi, jos haluat siirtää tehokkaasti tietoja kesken tietojenkäsittely resurssien tarpeen, jos haluat suorittaa kyselyn. Näiden tietojen siirtämistä toimintojen sisäisissä ja kaikki optimointi tietojen siirto-palveluun tapahtuu automaattisesti.

- Liitetty **columnstore** indeksit oletusarvoisesti. SQL-tietovarasto saa keskimäärin noin 5 x pakkaamisen voitot perinteinen rivimuotoinen tallennustilan päälle ja korkeintaan 10 x tai Lisää kyselyn suorituskykyä voitot käyttämällä tallennustilan sarakkeen perusteella. Analytics-kyselyjä, jotka on paljon rivejä työn hyviltä columnstore indeksit tarkistamaan.


## <a name="predictable-and-scalable-performance"></a>Ennakoitavissa ja skaalattava suorituskyky

SQL-tietovarasto erottaa säilytys- ja Laske, joka sallii kunkin skaalata erikseen. SQL-tietovarasto nopeasti ja helposti skaalata Lisää muita Laske resurssien ilmoitus jonkin aikaa. Azure-Blob-säiliö käyttö täydentämisestä tämä on. BLOB avulla vakaata ja replikoitua tallennustilan lisäksi myös infrastruktuuri pienen hintaan vaivaton laajentamista. Käytä tätä yhdistelmää cloud-akseli tallennustilan ja Azure suorittaminen, SQL-tietovarasto avulla voit maksaa kyselyn suorituskykyä ja tallennustilaa, kun tarvitse sitä. Laske summa muuttaminen on pelkästään siirtämällä liukusäädintä vasemmalle tai oikealle Azure-portaalissa tai se myös voidaan ajoittaa T-SQL- ja PowerShellin avulla.

Mahdollisuus täysin hallita Laske ulkopuolisista tallennustilan määrää, sekä SQL-tietovarasto avulla voit keskeyttää täysin tietovarasto, mikä tarkoittaa sitä, Laske ei maksat, kun et tarvitse sitä. Kaikki Laske päästetään ja tallennustilaa säilyttää paikassa, Azure-tallentaminen rahaa tärkeimmät resurssivarantoon. Tarvittaessa, riittää, että ansioluettelo Laske tiedot ja Laske käytettävissä havainnollistamiseen.

## <a name="data-warehouse-units"></a>Varaston yksiköiden

Resurssien SQL-tietovarasto mitataan Data Warehouse mittayksikkö (DWUs). DWUs ovat pohjana resurssit, kuten suorittimen, muistin IOPS, joka on kohdistettu SQL-tietovarasto mitta. Lisääntyvien DWUs määrä kasvaa resurssit ja suorituskykyä. Tarkemmin sanottuna DWUs varmistaa, että:

- Olet voi skaalata data warehouse helposti tarvitsematta tietoa pohjana laitteiston ja ohjelmiston.

- Suorituskyvyn parantaminen DWU tason voi ennustaa, ennen kuin muutat data warehouse koon.

- Pohjana laitteiston ja ohjelmiston oman esiintymän voit muuttaa tai siirtää vaikuttamatta työmäärää suorituskykyä.

- Microsoft voit tehdä muutoksia pohjana arkkitehtuuri palvelun vaikuttamatta havainnollistamiseen suorituskykyä.

- Microsoft nopeasti voit parantaa suorituskykyä SQL-tietovarasto niin, että on skaalattava ja tehosteet tasaisesti järjestelmä.

Varaston yksiköiden on kolme tarkkoja tietoja, joista vastaavuussuhteet erittäin kanssa tietovaraston työmäärää suorituskyvyn mitta. Tavoitteena on, että seuraavat avaimen työmäärän arvot Skaalaa lineaarisesti kanssa, jotka olet valinnut tiedot varaston DWUs.

**Tarkistuksen/kooste:** Kuormituksen tämä arvo on vakio tietovaraston kysely, joka tarkistaa suuri määrä rivejä ja suorittaa sitten monimutkaisia kooste. Tämä on i/o ja suorittimen tehostettu toiminnon.

**Kuormituksen:** Tämä arvo mittaa mahdollisuus ingest tietojen palveluun. Lataa on valmis PolyBase ladataan edustavan tietojoukon Azure-Blob-säiliö. Tämä arvo on suunniteltu kuormitus verkon ja palvelun suorittimen ominaisuuksia.

**Luo taulukko valitsemalla (CTAS):** CTAS mittaa voi kopioida taulukon. Tämä koskee tietojen lukeminen tallennustilan, sen jakamista eri laitteen solmut ja muut säilöön uudelleen. Tehostettu suorittimen, IO ja verkko-toiminto on.

## <a name="pause-and-scale-on-demand"></a>Keskeytä ja skaalausta pyydettäessä

Tarvittaessa voit nopeuttaa tulosten Suurenna oman DWUs ja maksaa suurempi suorituskykyä. Kun on vähemmän Laske power, pienennä oman DWUs ja maksaa vain etsimääsi tietoa. Suunnittelet voi muuttaa oman DWUs näissä tilanteissa:

- Kun et tarvitse suorittaa kyselyjä, esimerkiksi ulos illalla tai viikonloput pysäytystilaan kyselyissä. Pysäytä Laske resurssien välttämiseksi maksaa DWUs varten, kun et tarvitse.

- Jos järjestelmässä on pieni demand, kannattaa ehkä vähentää DWU pieni koko. Voit silti käyttää tietoja, mutta kustannukset säästöjen.

- Suoritettaessa paksu tietojen lataaminen tai muunnos-toimintoa voit skaalata niin, että tiedot ovat nopeasti käytettävissä.

Saat lisätietoja ihanteellinen DWU-arvo on, yritä skaalaus ylös ja alas sekä käynnissä muutama kysely tietojen lataamisen jälkeen. Skaalaus on nopea, voit kokeilla suorituskyvyn tunnin tai vähemmän eri tasojen määrä.  Ota huomioon, että SQL-tietovarasto on suunniteltu käsittelemään suuria tietomääriä ja nähdäksesi sen tosi ominaisuuksia skaalauksen, erityisesti Tarjoamme suurempi Skaalaa, milloin kannattaa käyttää suuren tietojoukon joka lähestyy tai suurempi kuin 1 TT.


## <a name="built-on-sql-server"></a>SQL Server rakennettu

SQL-tietovarasto perustuu SQL Server-relaatiotietokannasta ohjelma ja sisältää monia enterprise-tietovarasto odotettua ominaisuuksia. Jos jo tiedät T-SQL-on helppo siirtää tietosi SQL-tietovarasto. Onko siirryt tai vasta, yli ohjeista esimerkkejä auttaa alkaa. Yleistä mieleesi siitä, miten, että olemme on rakennettava SQL-tietovarasto kielen osista seuraavasti:

- SQL-tietovarasto käyttää T-SQL-syntaksissa monet toiminnot. Se tukee myös laajan perinteinen SQL-rakenteita, kuten tallennettujen toimintosarjojen, käyttäjän määrittämät funktiot, taulukon jakaminen, indeksit ja lajittelut.

- SQL-tietovarasto on myös useita uudempaan SQL Server-ominaisuuksista, kuten: liitetty **columnstore** indeksit, PolyBase integrointi ja tietojen tarkistaminen (uhkien arvioinnissa täyttäminen).

- Tiettyjen T-SQL-kielen elementtejä, jotka on harvinaisempi tietovaraston työmääriä tai uusilla SQL Server ei ehkä ole käytettävissä. Katso lisätietoja [siirron ohjeissa][].

SQL Server, SQL-tietovarasto, SQL-tietokantaan ja Analytics Platform järjestelmän välillä Transact-SQL- ja -ominaisuus-commonality, jossa voit kehittää tiedot tarpeitasi vastaavaa ratkaisua. Voit päätä säilyttää tiedot, perusteella suorituskyvyn, suojaus ja mittakaava vaatimukset ja siirtää sitten välillä erilaisia tietoja tarpeen mukaan.

## <a name="data-protection"></a>Tietojen suojaaminen

SQL-tietovarasto kaikki tiedot tallennetaan paikallisesti tarpeettomat Azure Premium-tallennustilan. Synkronoitu kopioihin tiedot säilyvät paikallisen tietokeskuksen taata läpinäkyvä tietojen suojaaminen järjestelmän lokalisoidut virheiden varalta. Lisäksi SQL-tietovarasto automaattisesti Varmuuskopioi aktiivinen (ei keskeytetty) tietokantoja Azure-tallennustilan tilannevedosten käyttäminen säännöllisin väliajoin. Saat lisätietoja varmuuskopioiminen ja palauttaminen toimii artikkelissa [Yleistä varmuuskopiointi ja palauttaminen][].

## <a name="integrated-with-microsoft-tools"></a>Integroitu Microsoft-Työkalut

SQL-tietovarasto myös yhdistää useita työkaluja, SQL Server, käyttäjät voivat olla tuttuja. Näitä ovat:

**Perinteinen SQL Server-työkalut:** SQL-tietovarasto on täysin integroitu SQL Server Analysis Services, Integration Services sekä Reporting Services.

**Pilvipohjainen työkalut:** SQL-tietovarasto voidaan käyttää useita uusia työkaluja Azure-tietokannassa, mukaan lukien Data Factory, Stream Analytics, tietokoneen oppiminen ja Power BI: n rinnalla. Lisätietoja luettelo on artikkelissa [integroitujen yleiskatsaus][].

**Kolmannen osapuolen työkaluja:** Kolmannen osapuolen työkalun tarjoajat suuri määrä on hyväksytty integrointi SQL-tietovarasto niiden työkaluja. Täydellinen luettelo on artikkelissa [SQL-tietovarasto ratkaisu kumppaneille][].

## <a name="hybrid-data-sources-scenarios"></a>Hybrid tietolähteiden skenaariot

SQL-tietovarasto käyttäminen PolyBase antaa käyttäjille ennennäkemättömät mahdollisuus Siirrä tiedot niiden ekosysteemiin lukituksen mahdollisuus asennuksen Lisäasetukset hybrid skenaariot ja relaatio ja paikallisia tietolähteitä.

Polybase avulla voit hyödyntää tiedot eri lähteistä käyttämällä tavallisia T-SQL-komentoja. Polybase mahdollistaa kyselyn-suhteellinen tiedot säilytetään Azure-Blob-säiliö aivan kuin tavallinen taulukko on. Käytä Polybase kyselyn relaatio tietoja tai relaatio tietojen tuominen SQL-tietovarasto.

- PolyBase käyttää ulkoisen taulukoita, voit käyttää muita kuin relaatiotietoja. Taulukon määritykset tallennetaan SQL-tietovarasto, ja voit käyttää niitä käyttämällä SQL ja työkaluja, kuten, jonka käyttää Normaali relaatiotietoja.

- Polybase on agnostic sen integrointi. Se paljastaa saman ominaisuuksia ja toimintoja, joita se tukee lähteitä. Polybase lukea tietoja voi olla eri muodoissa, kuten erotinmerkeillä Erotellut tiedostot tai ORC tiedostoja.

- PolyBase voidaan käyttää Blob-objektien tallennustilaan, jota käytetään myös tallennustilan HDInsight-klusterin. Näin voit käyttää relaatio-ja relaatio samat tiedot.

## <a name="sla"></a>SLA

SQL-tietovarasto on tuotteen tason tason palvelusopimus (SLA) Microsoft Online Services-SLA osana. Katso lisätietoja seuraavasta [SQL-tietovarasto SLA][]. Muita tuotteita koskevissa asioissa SLA lisätietoja pääset käsiksi [Palvelun tason toimeenpano] Azure sivun tai ladata ne [Volume Licensing][] -sivulla. 

## <a name="next-steps"></a>Seuraavat vaiheet

Kun tiedät hieman SQL-tietovarasto, tutustu nopeasti [luoda SQL-tietovarasto][] ja [Lataa mallitiedot][]. Jos ole ennen käyttänyt Azure, saatat huomata [Azure sanasto][] hyötyä, kun selaat uusia termejä. Tai Katso osa nämä SQL Data Warehouse resursseihin.  

- [Asiakkaan menestystarinoita]
- [Blogeja]
- [Ehdottaa ominaisuuksia]
- [Videot]
- [Asiakkaan neuvoa ryhmän blogeihin]
- [Luo tuki lippu]
- [MSDN-keskustelupalsta]
- [Pinon ylivuoto-keskustelupalsta]
- [Twitter-]


<!--Image references-->
[1]: ./media/sql-data-warehouse-overview-what-is/dwarchitecture.png

<!--Article references-->
[Luo tuki lippu]: ./sql-data-warehouse-get-started-create-support-ticket.md
[Lataa mallitiedot]: ./sql-data-warehouse-load-sample-databases.md
[SQL-tietovarasto luominen]: ./sql-data-warehouse-get-started-provision.md
[Siirron dokumentaatio]: ./sql-data-warehouse-overview-migrate.md
[SQL-tietovarasto ratkaisu kumppaneille]: ./sql-data-warehouse-partner-business-intelligence.md
[Integroitujen yleiskatsaus]: ./sql-data-warehouse-overview-integrate.md
[Yleistä varmuuskopiointi ja palauttaminen]: ./sql-data-warehouse-restore-database-overview.md
[Azure-sanasto]: ../azure-glossary-cloud-terminology.md

<!--MSDN references-->

<!--Other Web references-->
[Asiakkaan menestystarinoita]: https://customers.microsoft.com/search?sq=&ff=story_products_services%26%3EAzure%2FAzure%2FAzure%20SQL%20Data%20Warehouse%26%26story_product_families%26%3EAzure%2FAzure%26%26story_product_categories%26%3EAzure&p=0
[Blogeja]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Asiakkaan neuvoa ryhmän blogeihin]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Ehdottaa ominaisuuksia]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[MSDN-keskustelupalsta]: https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=AzureSQLDataWarehouse
[Pinon ylivuoto-keskustelupalsta]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter-]: https://twitter.com/hashtag/SQLDW
[Videot]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[SQL-tietovarasto SLA]: https://azure.microsoft.com/en-us/support/legal/sla/sql-data-warehouse/v1_0/
[Volyymikäyttöoikeus]: http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=37
[Service Level Agreement]: https://azure.microsoft.com/en-us/support/legal/sla/

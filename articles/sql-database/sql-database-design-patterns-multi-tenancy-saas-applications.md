<properties
   pageTitle="Suunnittele kuviot multitenant SaaS sovellusten ja Azure SQL-tietokanta | Microsoft Azure"
   description="Tässä artikkelissa käsitellään vaatimukset ja yleisiä tietoja arkkitehtuuri kuviot sovellusten cloud-ympäristössä kannattaa ottaa huomioon multitenant tietokannan ja näiden kuvioiden liittyvät eri kompromissien. Se myös käsitellään miten Azure SQL-tietokanta, joustavasti jakavat ja joustavasti Työkalut, osoite ei haavoittuvan tavalla näitä vaatimuksia."
   keywords=""
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="sqldb-design"
   ms.date="08/24/2016"
   ms.author="carlrab"/>

# <a name="design-patterns-for-multitenant-saas-applications-and-azure-sql-database"></a>Suunnittele kuviot multitenant SaaS sovellusten ja Azure SQL-tietokanta

Tässä artikkelissa saat vaatimukset ja yhteisiä tietoja arkkitehtuuri kuviot multitenant ohjelmiston kuin service (SaaS) tietokantasovelluksia, joiden cloud-ympäristössä. Se kerrotaan myös seikat, jotka kannattaa ottaa huomioon ja valinnat, eri rakenne kuviot. Joustavasti jakavat ja joustavasti Työkalut Azure SQL-tietokantaan voit auttavat mukauttamaan erityistarpeita käyttämättä missään vaiheessa muihin tavoitteisiin.

Kehittäjät voivat tehdä joskus valintoja, jotka toimivat niiden pitkään etu vastaan, kun ne suunnitella tietojen tasoa multitenant sovellusten vuokraajan mallit. Aluksi vähintään kehittäjä voi puoliväliarvo helpottaa kehittämisen ja pienempi cloud palveluntarjoajan kustannukset kuin tulostusjälkeä tärkeämpi vuokraajan eristystaso tai sovelluksen skaalattavuus. Tämä valinta voi aiheuttaa asiakkaiden tyytyväisyyttä koskee ja kallista kurssin-korjaus myöhemmin.

Joka sisältää palvelujen satoja tai tuhansia alihallinnat, jotka Älä jaa tai muiden henkilöiden tiedot samat multitenant sovelluksen on sovelluksen pitäminen cloud-ympäristössä. Esimerkki on SaaS sovellus, joka tarjoaa vuokraajiin cloud isännöimä ympäristössä.

## <a name="multitenant-applications"></a>Multitenant sovellukset

Multitenant sovelluksissa tiedot ja työmäärää voi olla helposti osioitu. Voit osioihin tiedot ja työmäärää, esimerkiksi pitkin vuokraajan rajat, koska eniten pyyntöjä palvelutili allekirjoitusvarmenteella. Ominaisuus sisältyy tiedot ja työmäärää ja sen favors tässä artikkelissa kuvatut sovelluksen kuviot.

Kehittäjät voivat käyttää tämäntyyppistä sovelluksen yli koko eri pilvipohjainen sovellusten, kuten:

- Kumppanin tietokantasovelluksia, joiden on parhaillaan transitioned pilveen SaaS sovelluksina
- SaaS sovellusten laadittuihin ratkaisuihin alkava pilveen
- Suora, asiakkaan osoittava sovellukset
- Työntekijän osoittava yrityksen sovellukset

SaaS-sovelluksia, jotka on suunniteltu pilveen ja neliöjuuret sisältävän kuten kumppanin tietokantasovelluksia yleensä ovat multitenant sovellukset. SaaS nämä sovellukset pitää lisäohjelmistoa-sovelluksen palveluna niiden vuokraajiin. Alihallinnat, jotka voivat käyttää sovelluksen-palvelun ja omistajuus koko liittyvät tiedot, jotka on tallennettu sovelluksen osana. Mutta hyödyntää SaaS edut alihallinnat palauttaa jotkin hallita omia tietoja. Ne luota SaaS palveluntarjoaja niiden tiedot pysyvät turvallisten ja erillään muiden omistajien tiedoista. Esimerkkejä multitenant SaaS sovelluksen tällaista ovat MYOB, SnelStart ja Salesforce.com. Kaikki nämä sovellukset voi osioida vuokraajan rajat ja tukea sovelluksen suunnitella kuviot tässä artikkelissa käsiteltävät aiheet pitkin.

Sovellukset, jotka sisältävät suoraan palvelu, asiakkaat tai työntekijöille organisaation (kutsutaan usein käyttäjät alihallinnat sijaan) ovat multitenant sovelluksen eri toisen luokan. Asiakkaiden tilata palvelun ja omista palveluntarjoaja kerää ja tallentaa tiedot. Palveluntarjoajien on eristetty toisistaan lisäksi government määrätty yksityisyyttä koskevia määräyksiä asiakkaidensa tiedot pysyvät vaatimuksia. Asiakkaan osoittava multitenant sovelluksen tällaista ovat media sisältöpalveluista, kuten Netflix, Spotify ja Xbox LIVE. Muita esimerkkejä helposti ositettava sovellukset ovat asiakkaan osoittava, Internet-akseli sovellukset tai asioita Internet (IoT)-sovellukset, mitä kukin asiakas- tai laitteen voi yhteyshenkilönä osio. Osion rajat erottaa käyttäjille ja laitteille.

Kaikki sovellukset-osiossa helposti yhden ominaisuuden, kuten vuokraajan, asiakas, käyttäjän tai laitteen pitkin. Monimutkainen yritysresurssi suunnittelu (ERP)-sovelluksessa on esimerkiksi tuotteet ja asiakkaiden tilaukset. Se on yleensä monimutkaisia rakenne tuhaterotinta erittäin toisiinsa liittyviä taulukoita.

Osio ei ole strategiakartta voit käyttää kaikkien taulukoiden ja työskentelet yli sovelluksen työmäärää. Tässä artikkelissa keskitytään multitenant sovelluksia, jotka on helppo ositettava tiedot ja toiminnoista.

## <a name="multitenant-application-design-trade-offs"></a>Multitenant sovelluksen rakenne valinnat

Rakenne-kuvio, joka multitenant sovelluksen kehittäjä valitsee yleensä perustuu huomioon seuraavat seikat:

-   **Vuokraajan yksinään**. Kehittäjä on varmistettava, että ei ole vuokraajan on ei-toivottuja muiden omistajien tietojen käytön. Muita ominaisuuksia, kuten suojata häiriöistä muiden tekijöiden aiheuttamia, ei voi palauttaa vuokraajan tiedot ja vuokraajan kielikohtaiset mukautusten toteuttamisesta laajenee eristystaso taso.
-   **Cloud resurssin kustannukset**. SaaS sovelluksen on oltava kustannukset kilpailukyvyn. Multitenant sovelluskehittäjän halutessasi optimoida alemman kustannukset cloud resurssit, kuten tallennustilan käytössä ja Laske kustannukset.
-   **DevOps helpottamiseksi**. Multitenant sovelluksen kehittäjä on lisääminen eristystaso suojaus, säilyttää, ja niiden sovelluksen ja tietokannan rakenteen kunnon valvonta ja vuokraajan ongelmien vianmääritys. Sovellusten kehittämisen ja toiminnon monimutkaisuus vastaa suoraan parantavat kustannukset ja haastaa vuokraajan tyytyväisyyttä kanssa.
-   **Skaalattavuus**. Mahdollisuus lisätä vaiheittainen omistajien ja kapasiteetin alihallinnat, jotka tarvitsevat se on onnistunut SaaS-toiminnon.

Jokaisella seikoista on valinnat verrattuna toiseen. Ojentamassa ehkä tarjoa kätevin kehittäminen kokemus pienimmät kustannus pilveen. On tärkeää kehittäjä tuttua näistä asetuksista ja niiden valinnat sovelluksen suunnitteluprosessin aikana.

Suositut kehittäminen kuvion on useita alihallinnat pack yhtä tai useaa tietokantoihin. Tämän menetelmän etuja ovat alemman kustannukset, koska maksat muutamia tietokantojen ja suhteellinen helppokäyttöisyyden käyttäminen tietokantojen rajoitettu määrä. Mutta ajan myötä SaaS multitenant sovelluksen kehittäjä huomaat, että tämä vaihtoehto on huomattavan downsides vuokraajan eristystaso ja skaalattavuus. Jos vuokraajan eristystaso muuttuu tärkeä, Lisää työmäärään tarvitaan jaettua tallennustilan vuokraajan tietojen suojaaminen luvattomalta tai häiriöistä muiden tekijöiden. Tämä lisää työmäärään voi merkittävästi edistää kehitystyön ja eristystaso ylläpitokustannusten. Vastaavasti jos lisääminen alihallinnat, jotka on tehtävä, rakenne-kuvion vaatii yleensä osaamisalueet levittää vuokraajan tietojen yli tietokantojen skaalata oikein sovelluksen tiedot-taso.  

Vuokraajan eristystaso usein vaaditaan ratkaisevan SaaS multitenant sovelluksissa, jotka yrityksiä ja organisaatioita mukauttaminen. Kehittäjä voi houkutusta havaittu parempia yksinkertaisuuden ja kustannukset vuokraajan eristystaso ja skaalattavuus mukaan. Tämä trade-off voit todistaa monimutkaisia ja kallista vuokraajan eristystaso vaatimukset muuttuvat, kun palvelua kasvaa entistä tärkeitä ja hallittu sovellustason. Multitenant sovelluksissa, jotka sisältävät suoraan kuluttajille osoittava palvelu asiakkaille vuokraajan eristystaso voi kuitenkin pienemmäksi optimoiminen cloud resurssin kustannukset.

## <a name="multitenant-data-models"></a>Multitenant tietomallien

Yleiset rakenteen käytännöt liittyvät vuokraajan tietoja noudattamalla kolme eri tietokantamallien sekä kuvassa 1.


  ![Multitenant sovelluksen tietomallien](./media/sql-database-design-patterns-multi-tenancy-saas-applications/sql-database-multi-tenant-data-models.png)
 kuva 1: yleisiä multitenant tietomallien rakenne käytännöt

-   **Tietokannan vuokraajan**. Kunkin vuokraajan on oma tietokanta. Kaikki vuokraajan kielikohtaiset tiedot koskemaan vuokraajan tietokannan ja erillään muiden omistajien ja sen tiedot.
-   **Jaetun tietokannan sharded**. Useita alihallinnat jakaa jonkin useiden tietokantojen. Eri joukko alihallinnat, jotka on liitetty tietokantojen käyttämällä osioinnin strategia, kuten hash, alue tai luettelon jakaminen. Tämä tietojen jakauman strategia usein kutsutaan sharding.
-   **Jaetun tietokannan-yksittäinen**. Yksittäisen, joskus suuri-tietokannan tietoihin kaikki alihallinnat, jotka ovat disambiguated vuokraajan tunnussarake.

> [AZURE.NOTE] Sovelluksen kehittäjä voi valita eri alihallinnat, jotka sijoitetaan toisen tietokannan rakenteita ja disambiguate eri alihallinnat rakenteen nimen avulla. Microsoft ei suosittele tätä tapaa, koska se vaatii yleensä dynaamisen SQL käyttöä, eikä niitä voi tehokkaasti suunnitelman välimuistiin. Jäljempänä tässä artikkelissa on tietotasoja jaettuun tauluun tavasta multitenant sovelluksen luokan.

## <a name="popular-multitenant-data-models"></a>Suositut multitenant tietomallien

On tärkeää arvioida erityyppiset multitenant tietomallien-sovelluksen rakenne valinnat on olet jo määrittänyt kannalta. Seikoista auttaa vapausasteiden kolme yleisimmät multitenant tietomallien edempänä ja resurssien kuva 2 kuten tietokannan käyttöä.

-   **Yksinään**. Alihallinnat välinen eristystaso aste voi olla mitä vuokraajan eristystaso tietomallin kertyy mitta.
-   **Cloud resurssin kustannukset**. Resurssien jakaminen omistajien välillä määrä voi optimoida cloud resurssin kustannukset. Resurssi voidaan määrittää suorittaminen ja tallennustilaa kustannukset.
-   **DevOps kustannukset**. Sovelluksen kehitystä, käyttöönottoa ja hallittavuuden helpottaminen vähentää SaaS toiminnon kokonaiskustannukset.  

Kuva 2 Y-akselin näyttää vuokraajan eristystaso. X-akselilla näkyy resurssien jakaminen. Harmaa, keskellä vinoon nuoli osoittaa DevOps kustannukset, voit suurentaa tai pienentää maataloustuottajiin suuntaa.

![Suositut multitenant sovelluksen rakenne kuviot](./media/sql-database-design-patterns-multi-tenancy-saas-applications/sql-database-popular-application-patterns.png) kuva 2: Suositut multitenant tietomallien

Kuva 2 oikeassa-neljännes näkyy sovelluksen-kaava, joka käyttää mahdollisesti suuren, yhteen jaettuun tietokantaan, ja jaetun taulukon (tai erillinen rakenne) lähestymistapa. Kun se on hyvä resurssi, koska kaikki alihallinnat, jotka käyttävät saman tietokannan resurssit (suorittimen, muistin, i /) yhden tietokannan jakaminen. Mutta vuokraajan eristystaso on rajoitettu. Voit joutua muuttamaan suorittamalla lisätoimet suojaaminen alihallinnat päähän toisistaan sovellustason. Myös seuraavat vaiheet voivat huomattavasti DevOps hinnan kehittämiseen sekä sovelluksen hallinta. Skaalattavuus rajoittavat laite, joka isännöi tietokannan asteikon.

Vasemmassa neljännes kuvassa 2 on kuvattu useiden alihallinnat sharded yli useiden tietokantojen (yleensä eri laitteen skaalata yksiköt). Tietokantojen isännöi alijoukkoa alihallinnoista, mitä muut kuviot skaalattavuus-huolta osoitetta. Jos kapasiteetin vaaditaan Lisää alihallinnat, helposti uusien tietokantojen kohdistettu uusia laitteiston asteikon yksiköitä voit sijoittaa alihallinnat. Kuitenkin määrä resurssien jakaminen on pienennetty. Vain alihallinnat sijoitettu samaan aikayksikön Jaa resurssit. Tämän menetelmän on pieni monipuolisemmin kuin vuokraaja eristystaso, koska monet alihallinnat, jotka ovat edelleen collocated ilman automaattisesti suojata toistensa toiminnot. Sovelluksen monimutkaisuus pysyy suuri.

Kuva 2 vasemmassa-neljännes on kolmas vaihtoehto. Se sijoittaa kunkin vuokraajan tietojen oma tietokanta. Tämän menetelmän on hyvä vuokraajan-eristystaso ominaisuuksia, mutta rajoittaa resurssien jakaminen, kun kukin tietokanta on erillinen resursseja. Tämä vaihtoehto on hyvä, jos kaikki alihallinnat, jotka on varmasti ennakoitavissa toiminnoista. Jos vuokraajan työmääriä näkyvät hahmontaa, tarjoaja et voi optimoida resurssien jakaminen. Kommunikointiin yhteistä SaaS sovellusten. Palveluntarjoajan täytyy joko liiallinen säännöstä täyttämään tai alemman resurssit. Kustannuksia tai alemman vuokraajan tyytyväisyyttä joko tuloksena. Resurssien jakaminen välillä alihallinnat suurempi aste tulee kannattaa tehdä ratkaisuun edullisempaa. DevOps kustannukset ottaminen käyttöön ja ylläpitää sovelluksen myös lisääntyvien tietokantojen määrä kasvaa. Huolimatta nämä huolenaiheita Tämä menetelmä on paras ja helpoin eristystaso alihallinnat varten.

Seikoista vaikuttaa myös asiakas rakenne kaavaa:

-   **Vuokraajan tietojen omistajuus**. Sovelluksen, jossa alihallinnat säilyttää oman tietojen omistajuus favors yhden tietokantaa kohden mallia.
-   **Asteikko**. Sovellus, joka on tarkoitettu satoja tuhansia tai miljoonia alihallinnat favors tietokannan jakamisen tavoista, kuten sharding. Eristystaso vaatimukset voivat aiheuttaa edelleen haasteita.
-   **Arvo ja business mallin**. Jos jokin sovellus kohden-vuokraajan tuotto Jos pieni (alle dollari), eristystaso vaatimukset muuttuvat vähemmän tärkeitä ja jaettu tietokanta on järkevää. Jos vuokraajan kohti tuotto on vähintään muutaman dollareina, tietokannan vuokraajan malli on enemmän mahdollista. Voi olla apua pienentää kehittäminen kustannuksia.

Valita-rakenne valinnat kuvassa 2, ihanteellinen multitenant malli on hyvä vuokraajan eristystaso ominaisuuksien lisääminen optimaalisen resurssi voidaan jakaa muiden omistajien. Tämän mallin mahtuu kuva 2 oikeassa-neljännes kuvattu luokka.

## <a name="multitenancy-support-in-azure-sql-database"></a>Multitenancy tuki Azure SQL-tietokantaan

Azure SQL-tietokanta tukee kaikkia multitenant sovelluksen kuviot kuvatut kuva 2. Kun joustavasti jakavat se tukee myös sovelluksen-kaava, joka yhdistää hyvä resurssien jakaminen ja tietokanta-kohden-vuokraajan eristystaso edut esitellä (Katso oikeassa neljännes kuvassa 3). Joustavasti Tietokantatyökalut ja SQL-tietokanta-ominaisuuksien avulla pienentää kehittämään ja toimivat sovellus, jossa on useita tietokantoja (kuten kuvassa 3 erillinen tummennettu alue). Kyseisten työkalujen avulla voit luoda ja hallita usean tietokannan kuviot käyttävistä sovelluksista.

![Kuvioiden Azure SQL-tietokantaan](./media/sql-database-design-patterns-multi-tenancy-saas-applications/sql-database-patterns-sqldb.png) kuva 3: Multitenant sovelluksen kuviot Azure SQL-tietokantaan

## <a name="database-per-tenant-model-with-elastic-pools-and-tools"></a>Tietokannan vuokraajan mallin joustavasti jakavat ja työkalut

SQL-tietokantaan joustavasti jakavat yhdistää vuokraajan eristystaso ja resurssien jakaminen kesken vuokraajan tietokantojen parempi tuki tietokannan vuokraajan lähestymistapa. SQL-tietokanta on taso tietoratkaisun SaaS tarjoajien, jotka multitenant sovelluksia. Resurssi voidaan jakaa muiden omistajien yrityksille siirtää sovelluskerroksen tietokannan palvelun kerrokseen. Hallinta ja kysely tasolla tietokantojen koko monimutkaisuudesta on yksinkertaistettu joustavasti työt, joustavasti kyselyn, joustavasti tapahtumia ja joustavasti tietokannan asiakas-kirjasto.

| Sovelluksen vaatimukset | SQL-tietokanta-ominaisuudet |
| ------------------------ | ------------------------- |
| Vuokraajan eristystaso ja resurssien jakaminen | [Joustavasti jakavat](sql-database-elastic-pool.md): varaaminen resurssivarantoon SQL-tietokantaan ja resurssien jakaminen useiden tietokantojen. Yksittäiset tietokannat voit piirtää myös mahdollisimman paljon resursseja käytetään varannon tarvittaessa sopimaan kapasiteetin demand piikkarit vuokraajan työmääriä muutosten vuoksi. Joustavasti resurssivarantoon itse voi skaalata ylös tai alas, tarpeen mukaan. Joustavasti jakavat tarjoavat myös helpottaa hallittavuuden ja seurantaa ja ryhmän tasolla vianmääritys. |
| Tietokantojen yli DevOps helpottaminen | [Joustavasti jakavat](sql-database-elastic-pool.md): kuin edellä.|
||[Joustavasti kyselyn](sql-database-elastic-query-horizontal-partitioning.md): kyselyn yli tietokantojen raportointia varten tai rajat-vuokraajan analyysi.|
||[Joustavasti työt](sql-database-elastic-jobs-overview.md): paketin ja käyttöönotto luotettavasti useiden tietokantojen tietokannan ylläpito toimintojen tai tietokannan rakenteen muutokset.|
||[Joustavasti tapahtumat](sql-database-elastic-transactions-overview.md): prosessi muuttaa useiden tietokantojen atomisia ja eristetty tavalla. Joustavasti tapahtumat tarvitaan, kun sovellukset on "kaikki tai ei mitään-oikeudet tietokannan varallisuuden päälle. |
||[Joustavasti tietokannan asiakkaan kirjaston](sql-database-elastic-database-client-library.md): hallinta tietojen jakelu ja yhdistää alihallinnat tietokannat. |

## <a name="shared-models"></a>Jaetut mallit

Ohjeiden aiemmin useimmat SaaS toimittajien jaettu malli-menetelmän voivat aiheuttaa ongelmia vuokraajan eristystaso ongelmat ja monimutkaisia sovellusten kehittämisen ja ylläpitoa. Kuitenkin multitenant sovelluksissa, jotka tarjota kuluttajille suoraan-vuokraajan eristystaso vaatimukset eivät saa olla suuri prioriteetti kuin pienentäminen kustannukset. Hän voi voi pack alihallinnat vähintään yksi tietokantojen pienentämiseksi HD-palvelussa. Yhden tietokannan tai useiden sharded tietokantojen jaettu tietokantamallien saattaa aiheuttaa muita tehokkuus resurssien jakaminen ja yleistä kustannukset. Azure SQL-tietokanta on joitakin ominaisuuksia, jotta asiakkaat luominen eristystaso parannettu suojaus ja käytetään tason tietojen hallintaa varten.

| Sovelluksen vaatimukset | SQL-tietokanta-ominaisuudet |
| ------------------------ | ------------------------- |
| Eristystaso suojaustoiminnot | [Rivin käyttäjätason suojauksen](https://msdn.microsoft.com/library/dn765131.aspx) |
|| [Tietokannan rakenne](https://msdn.microsoft.com/library/dd207005.aspx) |
| Tietokantojen yli DevOps helpottaminen | [Joustavasti kysely](sql-database-elastic-query-horizontal-partitioning.md) |
|| [Joustavasti töitä](sql-database-elastic-jobs-overview.md) |
|| [Joustavasti tapahtumat](sql-database-elastic-transactions-overview.md) |
|| [Joustavasti tietokannan asiakkaan kirjastoon](sql-database-elastic-database-client-library.md) |
|| [Jaetun tietokannan joustavasti ja yhdistäminen](sql-database-elastic-scale-overview-split-and-merge.md) |

## <a name="summary"></a>Yhteenveto

Vuokraajan eristystaso vaatimukset ovat tärkeitä useimmat SaaS multitenant sovellukset. Paras tapa antaa eristystaso leans raskaasti tietokannan vuokraajan lähestymistapa kohti. Näitä kahta tapaa vaativat monimutkaisia sovelluksen tasojen, jotka edellyttävät taitoja kehittäminen henkilökunnan antamaan eristystaso, joka parantaa huomattavasti kustannus- ja risk sijoitukset. Jos eristystaso vaatimukset ovat ei ole laskettu aikaisin-palvelun kehitystä, retrofitting ne voidaan vieläkin kallista kaksi ensimmäistä malleissa. Tärkeimmät haitoista, liittyvän tietokannan vuokraajan mallin liittyvät parantavat cloud Resurssikustannukset rajoitetun jakaminen ja ylläpito ja hallita useita tietokantoja vuoksi. SaaS sovelluskehittäjät se usein, kun he nämä valinnat.

Vaikka valinnat voi olla pää esteistä kanssa cloud tietokannan useimmat palveluntarjoajat Azure SQL-tietokanta ei esteistä joustavasti resurssivarantoon ja joustavasti tietokannan ominaisuuksia. SaaS kehittäjät voit yhdistää tietokannan vuokraajan mallin eristystaso ominaispiirteet ja optimointi resurssien jakaminen ja useiden tietokantojen hallinta-parannukset joustavasti jakavat ja liittyvän työkalujen avulla.

Multitenant sovelluksen tarjoajien, joilla on ei ole vuokraajan eristystaso vaatimukset ja jotka pack alihallinnat tietokannan osoitteessa suuren arvon. saatat huomata, että jaetut tiedot mallit tuloksen muita tehokkuuden resurssien jakaminen ja vähentää kokonaiskustannukset. Azure SQL-tietokantaan joustavasti Tietokantatyökalut, sharding kirjastojen ja suojaustoiminnot auttaa SaaS tarjoajat muodosta ja multitenant sovellusten hallinta.

## <a name="next-steps"></a>Seuraavat vaiheet

Esimerkki-sovellusta, joka osoittaa asiakirjakirjaston [joustavasti Tietokantatyökalut käytön aloittaminen](sql-database-elastic-scale-get-started.md) .

Malli-sovelluksella, joka käyttää joustavasti jakavat edullinen skaalattava tietokanta-ratkaisun [joustavasti resurssivarantoon mukautetun raporttinäkymä SaaS](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools-custom-dashboard) luominen.

Voit [siirtää aiemmin luotujen tietokantojen skaalata ulos](sql-database-elastic-convert-to-use-elastic-tools.md)Azure SQL-tietokanta-työkalujen avulla.

Näytä tämän opetusohjelman luomisesta [joustavasti resurssivarantoon](sql-database-elastic-pool-create-portal.md).  

Lisätietoja siitä, miten voit [valvoa ja hallita joustavasti resurssivarantoon](sql-database-elastic-pool-manage-portal.md).

## <a name="additional-resources"></a>Lisäresursseja

- [Mikä on Azure joustavasti resurssivarantoon?](sql-database-elastic-pool.md)
- [Azure SQL-tietokanta ja laajentaminen](sql-database-elastic-scale-introduction.md)
- [Multitenant sovellusten joustavasti Tietokantatyökalut ja rivin käyttäjätason suojauksen](sql-database-elastic-tools-multi-tenant-row-level-security.md)
- [Käyttöoikeuksien multitenant käyttämällä Azure Active Directory- ja OpenID Yhdistä-sovelluksissa](../guidance/guidance-multitenant-identity-authenticate.md)
- [Tailspin kyselyt-sovellus](../guidance/guidance-multitenant-identity-tailspin.md)
- [Ratkaisu pika-aloituksen](sql-database-solution-quick-starts.md)

## <a name="questions-and-feature-requests"></a>Kysymyksiä ja ehdottaa ominaisuuksia

Kysymyksiä Etsi us [SQL-tietokanta-keskustelupalstalle](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted). Lisää ominaisuuspyyntö [SQL-tietokantaan palautetta keskustelupalstalle](https://feedback.azure.com/forums/217321-sql-database/).

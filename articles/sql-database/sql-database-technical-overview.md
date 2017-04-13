<properties
    pageTitle="Mikä on SQL-tietokantaan? Johdanto SQL-tietokantaan | Microsoft Azure"
    description="Tutustu sovelluksen SQL-tietokantaan: teknisistä tiedoista ja niiden ominaisuuksista Microsoftin relaatio tietokannan hallintajärjestelmän (RDBMS) pilveen."
    keywords="Johdanto sql-johdanto SQL, mikä on sql-tietokantaan"
    services="sql-database"
    documentationCenter=""
    authors="shontnew"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="08/16/2016"
   ms.author="shkurhek"/>

# <a name="what-is-sql-database-introduction-to-sql-database"></a>Mikä on SQL-tietokantaan? Johdanto SQL-tietokantaan

SQL-tietokanta on relaatiotietokannasta pilveen market johtavien Microsoft SQL Server-ohjelma, kriittisten ominaisuuksia perusteella. SQL-tietokantaan toimittaa ennakoitavissa suorituskyvyn skaalattavuus käyttökatkot, Liiketoiminnan jatkuvuus ja tietojen suojaaminen, kaikki nollaa hallinnan kanssa. Voit keskittyä nopea sovellusten kehittämiseen ja nopeuttamiseksi ajankäytön market sijaan näennäiskoneiden ja infrastruktuurin hallinta. Koska se perustuu [SQL Server](https://msdn.microsoft.com/library/bb545450.aspx) -ohjelma-SQL-tietokanta tukee aiemmin SQL Server työkaluja, kirjastojen ja ohjelmointirajapinnan, jolloin on helpompaa, voit siirtää ja laajentaa pilveen.

Tässä artikkelissa on esittely SQL-tietokantaan core käsitteitä ja suorituskyky ja skaalattavuus hallittavuuden, jossa on linkkejä tietojen tarkasteleminen liittyviä ominaisuuksia. Jos haluat siirtyä, voit [ensimmäisen SQL-tietokannan luominen](sql-database-get-started.md) tai [Luo joustavasti tietokannan resurssivarantoon](sql-database-elastic-pool-create-portal.md) minuutteina. Jos haluat tarkempaa perinpohjaisesti käsittelevään artikkeliin, tässä videossa 30 minuuttia.

> [AZURE.VIDEO azurecon-2015-get-started-with-azure-sql-database]

## <a name="adjust-performance-and-scale-without-downtime"></a>Säädä suorituskyky ja skaalattavuus ilman Käyttökatkos

SQL-tietokantoja on saatavana Basic, Vakio ja Premium *palvelutasot*. Palvelun kunkin tason on tukemaan lightweight painavat tietokannan työmääriä [eritasoista suorituskykyä ja ominaisuuksia](sql-database-service-tiers.md) . Voit luoda ensimmäisen sovelluksen pieni tietokannan, muutaman bucks kuussa, valitse [Muuta palvelun tason](sql-database-scale-up.md) manuaalisesti tai ohjelmallisesti milloin tahansa, kun sovellus siirtyy virusperäisen, ilman käyttökatkot sovelluksen tai asiakkaillesi.

Monissa yritykset ja sovelluksia ei voi luoda tietokantoja ja soita yhden tietokannan suorituskykyä ylös tai alas pyydettäessä on tarpeeksi, erityisesti jos käyttötavat ovat suhteellisen ennakoitavissa. Mutta jos käytössäsi on odottamattomia käyttötavat, voi olla vaikea hallita kustannuksia ja business-malli.

SQL-tietokantaan [joustavasti jakavat](sql-database-elastic-pool.md) ratkaista tämän ongelman. Joka on helppoa. Kohdista resurssivarantoon suorituskykyä ja maksaa lajittelemisesta suorituskyvyn altaan yhden tietokannan suorituskykyä sijaan. Ei tarvitse soittaa tietokannan suorituskykyä, ylös tai alas. Resurssivarannon *joustavasti tietokantoja*, jota kutsutaan tietokannoissa Skaalaa automaattisesti ylös ja alas tavata tarvittaessa. Joustavasti tietokantojen tarjoaman, mutta ei ylittää raja-kohdasta, jotta kustannukset pysyy ennakoitavissa, vaikka tietokannan käyttö ei. Lisäksi voit [lisätä ja poistaa tietokantojen resurssivarantoon](sql-database-elastic-pool-manage-portal.md), skaalausta, kun sovellus muutama tietokantojen tuhansissa, kaikki budjetissa, joka voidaan hallita. Lisätietoja SaaS-sovellusten käyttäminen joustavasti jakavat kuviot rakenne-kohdassa [Rakenne kuviot usean vuokraajan SaaS sovellusten Azure SQL-tietokantaan](sql-database-design-patterns-multi-tenancy-saas-applications.md).

Molemmilla tavoilla voit siirtyä – yksi- tai joustavasti, jollet ole lukittu. Voit mielessäsi joustavasti tietokannan jakavat yhden tietokantojen ja muuttaa yksittäisen tietokantojen ja jakavat luominen innovatiivista rakenteen palvelutasot. Lisäksi power ja Azure saatavilla, voit Azure mix-ja VASTINE-palvelun vastaa tarpeitasi yksilöllisen nykyaikaista sovelluksen rakenne ja aseman kustannus- ja tehokkuus lukituksen uusi liiketoimintamahdollisuuksia SQL-tietokantaan.

Mutta miten voit verrata tietokantojen ja tietokannan jakavat suhteellisia suorituskykyä? Mistä tiedän, napsauta hiiren kakkospainikkeella-Lopeta soittaessasi ylös ja alas? Tämä johtuu tietokannan tapahtuman yksikkö (DTU) yksittäisen tietokantoja ja joustavasti tietokantojen ja tietokannan jakavat joustavasti DTU (eDTU). Katso [SQL-tietokanta-asetukset ja suorituskyky: ymmärtää, mikä on käytettävissä palvelun kunkin tason](sql-database-service-tiers.md) lisätietoja.

## <a name="keep-your-app-and-business-running"></a>Sovelluksen ja yhdistyvät säilyttäminen

Azure's alan alussa 99,99 % käytettävyys tason palvelusopimus [(SLA)](http://azure.microsoft.com/support/legal/sla/)tarjoaa yleisen verkon ja Microsoftin hallitsemaan palvelinkeskusten auttaa pitämään käytössä 24/7 sovelluksen. Jokaisen SQL-tietokantaan voit hyödyntää valmiit tietosuoja, vikasietoa ja tietojen suojaaminen, jotka olet muuten on, osta, luominen ja hallinta. Voivat näin pyytää suojaus, jotta sovelluksesi ja yrityksesi palauttaa nopeasti huono, virheen tai jotakin muuta muita kerroksia yrityksesi tarpeiden mukaan. SQL-tietokanta, jossa palvelun kunkin tason on eri valikko ominaisuuksien avulla voit helposti ja käynnissä ja olla niihin sen mukaan. Voit palauttaa tietokannan aiempaan tilaan, organisaatiokaavioita 35 päivää ajankohta palauttaminen. Lisäksi jos isännöinnin tietokantoja palvelinkeskuksen ilmenee käyttökatkosta, voit automaattisesti Tietokantareplikoita toisella alueella. Tai voit käyttää replikoiden päivitykset tai siirtäminen eri alueille.

![SQL-tietokannan Geo-replikointi](./media/sql-database-technical-overview/azure_sqldb_map.png)


Lisätietoja [Liiketoiminnan jatkuvuus](sql-database-business-continuity.md) eri liiketoiminnan jatkuvuuden ominaisuuksista käytettävissä eri palvelutasot.

## <a name="secure-your-data"></a>Suojata tietoja
SQL Server on tasainen tietojen suojauksen, että SQL-tietokantaan vaalii ominaisuuksia, jotka rajoittaa ja tietojen avulla voit valvoa toisaalta. Saat nopean rundown suojausasetukset on SQL-tietokantaan, [suojaaminen SQL-tietokantaan](sql-database-security.md) . Katso suojaustoiminnot monipuolisemman näkymän [Tietoturvakeskus SQL Server-tietokantamoduuli ja SQL-tietokantaan](https://msdn.microsoft.com/library/bb510589) . Ja [Azure Valvontakeskus](https://azure.microsoft.com/support/trust-center/security/) lisätietoja Azure's platform suojaus.

## <a name="next-steps"></a>Seuraavat vaiheet
Nyt kun olet esittely SQL-tietokantaan lukea ja vastata kysymykseen "Mikä on SQL-tietokantaan?", voit ryhtyä:

- Katso [hinnat sivun](https://azure.microsoft.com/pricing/details/sql-database/) yhteen tietokantaan ja laskimet joustavasti tietokannan kustannukset vertailuja.
- Lisätietoja [joustavasti jakavat](sql-database-elastic-pool.md).
- Aloita luomalla [ensimmäisen tietokannan](sql-database-get-started.md).
- [Yhdistä ja kysely, jossa on SSMS](sql-database-connect-query-ssms.md)
- Luo ensimmäinen sovelluksen C# Java, Node.js, PHP, Python ja Ruby: [yhteyden kirjastojen SQL-tietokantaan ja SQL Server](sql-database-libraries.md)
- Saat otsikot ja kuvaukset [kaikista](sql-database-index-all-articles.md)aiheista Azure sql-tietokanta-palveluun.

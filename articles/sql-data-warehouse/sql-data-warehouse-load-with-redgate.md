<properties
   pageTitle="Redgate käyttäjän tiedot ympäristö Studio avulla voit ladata tietoja SQL-tietovarasto | Microsoft Azure"
   description="Opettele käyttämään tietoja laajemmissa skenaarioissa Redgate henkilön tiedot ympäristö Studio."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="twounder"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="10/13/2016"
   ms.author="mausher;barbkess"/>


# <a name="load-data-with-redgate-data-platform-studio"></a>Lataa tiedot Redgate tietojen ympäristö Studiossa

> [AZURE.SELECTOR]
- [Redgate](sql-data-warehouse-load-with-redgate.md)
- [Tietoja Factory](sql-data-warehouse-get-started-load-with-azure-data-factory.md)
- [PolyBase](sql-data-warehouse-get-started-load-with-polybase.md)
- [BCP](sql-data-warehouse-load-with-bcp.md)

Tässä opetusohjelmassa näytetään, miten tiedot Siirry Azure SQL-tietovarasto paikallinen SQL Serveriä [Redgate henkilön tiedot ympäristö Studio](http://www.red-gate.com/products/azure-development/data-platform-studio/) (DPS) avulla. Tietojen ympäristö Studio koskee sopivimman yhteensopivuuden korjaukset ja optimointi, joten se on nopein tapa SQL-tietovarasto käytön aloittaminen.

> [AZURE.NOTE] [Redgate](http://www.red-gate.com) on käyttänyt Microsoft-kumppani, joka tarjoaa erilaisia SQL Server-työkaluja. Tietoja ympäristö Studiossa tämä ominaisuus on tehty käytettävissä vapaasti sekä kaupallisen kaupallista käyttöä varten.

## <a name="before-you-begin"></a>Ennen aloittamista
### <a name="create-or-identify-resources"></a>Luo tai Määritä resurssit

Ennen kuin aloitat Tässä opetusohjelmassa, sinun on on:

- **Paikallinen SQL Server-tietokantaan**: SQL-tietovarasto tuotavat tiedot on peräisin paikallinen SQL Serveriä (versio 2008R2 tai uudempi). Tietoja ympäristö Studio voi tuoda tietoja suoraan Azure SQL-tietokanta tai tekstitiedostoista.
- **Azure-tallennustilan tilin**: tietojen ympäristö Studio vaiheiden Azure-Blob-säiliö tietoja ennen ladataan SQL-tietovarasto. Tallennustilan tilin on oltava käytössä sen sijaan, että "Perinteinen" käyttöönoton mallin "resurssien hallinnan" käyttöönottomalli (oletusarvo). Jos sinulla ei ole tallennustilan-tili, tietoja tallennustilan tilin luominen. 
- **SQL-tietovarasto**: Tässä opetusohjelmassa siirtää tiedot paikallinen SQL Serveristä SQL-tietovarasto niin on oltava data warehouse verkossa. Jos sinulla ei ole tietovarasto luominen Azure SQL-tietovarasto.

> [AZURE.NOTE] Suorituskykyä on parannettu Jos tallennustilan tilin ja tietovarasto luodaan samalla alueella.

## <a name="step-1-sign-in-to-data-platform-studio-with-your-azure-account"></a>Vaihe 1: Kirjaudu sisään tietojen ympäristö Studio Azure-tili
Avaa selain ja siirry [Tietojen ympäristö Studio](https://www.dataplatformstudio.com/) -sivusto. Kirjaudu sisään samalla Azure-tili, jota käytit Luo tallennustilan tilin ja tietojen varasto. Jos sähköpostiosoite on liitetty on työpaikan tai oppilaitoksen tilin ja Microsoft-tiliä, muista, valitse tili, jolla on pääsy resurssit.

> [AZURE.NOTE] Jos kyseessä on tietoja ympäristö Studiossa ensimmäistä kertaa, sinua pyydetään myöntää sovelluksen oikeuden hallita Azure resursseja.

## <a name="step-2-start-the-import-wizard"></a>Vaihe 2: Aloita Ohjattu tuominen
Valitse Tuo Azure SQL-tietovarasto linkin voit käynnistää ohjatun tuontitoiminnon DPS päänäytössä.

![][1]

## <a name="step-3-install-the-data-platform-studio-gateway"></a>Vaihe 3: Asenna Data ympäristö Studio Gateway
Muodosta yhteys paikallinen SQL Server-tietokantaan, sinun täytyy asentaa DPS yhdyskäytävän. Yhdyskäytävä on asiakas-agentti, joka sisältää access-paikalliseen ympäristöön, hakee tietoja ja lataa tallennustilan-tiliisi. Tietojen kulkee koskaan Redgate's-palvelimiin. Voit asentaa yhdyskäytävän seuraavasti:

1.  Valitse **Luo yhdyskäytävä** -linkki
2. Lataa ja asenna annettu installer yhdyskäytävän

![][2]

> [AZURE.NOTE] Yhdyskäytävän voidaan asentaa koneen verkkoyhteydessä lähteen SQL Server-tietokantaan. Se käyttää Windows-todennuksen avulla nykyisen käyttäjän tunnistetietojen SQL Server-tietokantaan.

Kun asennettu, yhdyskäytävän tila muuttuu yhteys on muodostettu, ja voit valita Seuraava.

## <a name="step-4-identify-the-source-database"></a>Vaihe 4: Määritä lähdetietokannan
Kirjoita *Kirjoita palvelimen nimi* -tekstiruutuun, joka isännöi tietokanta ja valitse **Seuraava**palvelimen nimi. Valitse sitten avattavasta valikosta haluamasi tiedot tuodaan tietokanta.

![][3]

DPS etsii valitun tietokannan tuotavat taulukot. Oletusarvon mukaan DPS tuo tietokannan kaikki taulukot. Voit valita tai poista taulukot laajentamalla kaikki taulukot-linkkiä. Valitse Siirrä eteenpäin Seuraava-painiketta.

## <a name="step-5-choose-a-storage-account-to-stage-the-data"></a>Vaihe 5: Valitse vaiheen tiedot tallennustilan-tili
DPS kehottaa sijainnille vaiheen tiedot. Käytössä olevan tallennustilan tilin tilauksesta ja valitse **Seuraava**.

> [AZURE.NOTE] DPS luodaan uusi blob-säilö valitsemasi tallennustilan tilin ja käyttää eri kansion tuonnin.

![][4]

## <a name="step-6-select-a-data-warehouse"></a>Vaihe 6: Valitse tietojen varasto
Valitse seuraavaksi online [Azure SQL-tietovarasto](http://aka.ms/sqldw) -tietokantaan voit tuoda tiedot. Kun olet valinnut tietokannan, sinun täytyy Anna Muodosta yhteys tietokantaan ja valitse **Seuraava**.

![][5]

> [AZURE.NOTE] DPS yhdistää tietoja lähdetaulukot tietovarasto. DPS varoittaa, jos taulukkonimi edellyttää, että korvaa olemassa olevat tietovarasto taulukot. Voit myös poistaa tietovarasto aiemmin objekteihin mukaan ticking Poista kaikki objektit ennen tuontia.

## <a name="step-7-import-the-data"></a>Vaihe 7: Tietojen tuominen
DPS vahvistaa, että haluat tuoda tiedot. Aloita tietojen tuonti Käynnistä Tuo-painiketta napsauttamalla.

![][6]

DPS näyttää visualisointi, joka poimii ja lataamisen paikallinen SQL Server- ja SQL-tietovarasto tuonnin etenemisen tiedot edistyminen näkyy.

![][7]

Kun tuonti on valmis, DPS näyttää yhteenvedon tietojen tuonnin ja muuta raportin, joka on suoritettu yhteensopivuuden korjaukset.

![][8]

## <a name="next-steps"></a>Seuraavat vaiheet
Tutustu tietojen sisällä SQL-tietovarasto ratkaisuun tarkasteleminen:

- [Kyselyn Azure SQL-tietovarasto (Visual Studiossa)][]
- [Power BI tietojen visualisointi][]

Lisätietoja Redgate henkilön tiedot ympäristö Studio:

- [Käy DPS kotisivu](http://www.dataplatformstudio.com/)
- [Katso esittely DPS-Channel9](https://channel9.msdn.com/Blogs/cloud-with-a-silver-lining/Loading-data-into-Azure-SQL-Datawarehouse-with-Redgate-Data-Platform-Studio)

Muita tapoja siirtää ja ladata SQL-tietovarasto tietojen yleiskatsauksen seuraavissa artikkeleissa:

- [Siirtää SQL-tietovarasto ratkaisu][]
- [Ladata tietoja SQL Azure-tietovarasto](./sql-data-warehouse-overview-load.md)

Artikkelissa kehitys Lisävihjeitä [SQL-tietovarasto kehittäminen yleiskatsaus](./sql-data-warehouse-overview-develop.md).

<!--Image references-->
[1]: media/sql-data-warehouse-redgate/2016-10-05_15-59-56.png
[2]: media/sql-data-warehouse-redgate/2016-10-05_11-16-07.png
[3]: media/sql-data-warehouse-redgate/2016-10-05_11-17-46.png
[4]: media/sql-data-warehouse-redgate/2016-10-05_11-20-41.png
[5]: media/sql-data-warehouse-redgate/2016-10-05_11-31-24.png
[6]: media/sql-data-warehouse-redgate/2016-10-05_11-32-20.png
[7]: media/sql-data-warehouse-redgate/2016-10-05_11-49-53.png
[8]: media/sql-data-warehouse-redgate/2016-10-05_12-57-10.png

<!--Article references-->
[Kyselyn Azure SQL-tietovarasto (Visual Studiossa)]: ./sql-data-warehouse-query-visual-studio.md
[Power BI tietojen visualisointi]: ./sql-data-warehouse-get-started-visualize-with-power-bi.md
[Siirtää SQL-tietovarasto ratkaisu]: ./sql-data-warehouse-overview-migrate.md
[Load data into Azure SQL Data Warehouse]: ./sql-data-warehouse-overview-load.md
[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md
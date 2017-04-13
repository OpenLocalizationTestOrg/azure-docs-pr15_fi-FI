<properties
   pageTitle="Azure SQL-tietokannan Yleiset rajoitukset ja ohjeita"
   description="Tällä sivulla kerrotaan Yleiset rajoituksia Azure SQL-tietokanta sekä alueet yhteensopivuus ja tuki."
   services="sql-database"
   documentationCenter="na"
   authors="CarlRabeler"
   manager="jhubbard"
   editor="monicar" />
<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/06/2016"
   ms.author="carlrab" />

# <a name="azure-sql-database-general-limitations-and-guidelines"></a>Azure SQL-tietokannan Yleiset rajoitukset ja ohjeita

Tämä artikkeli sisältää yleiset rajoitukset ja ohjeita Azure SQL-tietokantaan. Katso ymmärtämisen kiintiön, Resurssienhallinta ja tuki on [lisäresursseja](#additional-guidelines) on tämän artikkelin lopussa.

## <a name="connectivity-and-authentication"></a>Yhteys ja todentaminen

  - Windows-todennusta ei tueta. Katso [tietokantojen ja Azure SQL-tietokanta-kirjautumiset hallinta](sql-database-manage-logins.md). Azure Active Directory-todennus tukee kuitenkin tietyt rajoitukset. Artikkelissa [yhteyden muodostaminen SQL-tietokantaan ja Azure Active Directory-todennusta](sql-database-aad-authentication.md).

  - Microsoft Azure SQL-tietokanta tukee taulukkomuotoiset tiedot stream (TKJ) protokolla asiakkaan versio 7.3 tai uudempi.

  - Vain TCP/IP-yhteydet sallitaan.

  - SQL Server 2008: n SQL Server-selainta ei tueta, koska Microsoft Azure SQL-tietokanta ei ole dynaaminen portit, vain portin 1433.

## <a name="sql-server-agentjobs"></a>SQL Server Agent/työt

Microsoft Azure SQL-tietokanta ei tue SQL Server Agent kuitenkin joustavasti työt avulla voit suorittaa töitä yli yhtä monta tietokantoihin. Saat lisätietoja joustavasti työt [joustavasti työt](sql-database-elastic-jobs-overview.md).

## <a name="sql-server-collation-support"></a>SQL Server lajittelu-tuki

Tietokannan Oletuslajittelu käyttämän Microsoft Azure SQL-tietokanta on **SQL_LATIN1_GENERAL_CP1_CI_AS**, jossa **LATIN1_GENERAL** on englanti (Yhdysvallat), **CP1** on koodisivu 1252, **CI** on asennustavan ja **sellaisenaan korostus perustuva** . Ei voida muuttaa Vipuventtiili V12 tietokantojen lajittelua. Katso lisätietoja siitä, miten voit määrittää lajittelua, [LAJITTELE (Transact-SQL)](https://msdn.microsoft.com/library/ms184391.aspx).

## <a name="naming-requirements"></a>Nimeämisvaatimukset

Tiettyjen käyttäjien nimet eivät ole sallittuja tietoturvasyistä. Et voi käyttää seuraavia nimiä:

 - **Järjestelmänvalvoja**
 - **Järjestelmänvalvoja**
 - **Vieras**
 - **pääkansio**
 - **SA**

Kaikkien uusien objektien nimet on täytettävä tunnisteille SQL Server-säännöt. Lisätietoja on artikkelissa [tunnukset](https://msdn.microsoft.com/library/ms175874.aspx).

Lisäksi kirjautuminen ja käyttäjien nimet eivät saa sisältää \ merkin (Windows-todennusta ei voi käyttää).

## <a name="additional-guidelines"></a>Lisäohjeita

- Lisäksi tässä artikkelissa kuvattujen Yleiset rajoitukset SQL-tietokanta on tietyn palvelinresurssien kiintiön ja perustuvat käyttäjän **palvelutaso**rajoitukset. Katso yleiskuvaus palvelutasot on [SQL-tietokantaan palvelutasot](sql-database-service-tiers.md).

- Katso SQL-tietokannan muiden rajoitusten [Azure SQL tietokanta resurssien rajoitukset](sql-database-resource-limits.md).

- Aiheeseen liittyviä ohjeita on suojauksen [Azure SQL tietokanta suojauksen ohjeita ja rajoituksia](sql-database-security-guidelines.md).

- Toisen liittyvää aluetta ympäröi, jossa Azure SQL-tietokanta on paikallisen SQL Serveristä, kuten SQL Server 2014 ja SQL Server 2016-versioiden kanssa yhteensopivuuden. Azure SQL-tietokanta Vipuventtiili V12 uusin on tehty useita parannuksia tällä alueella. Lisätietoja on artikkelissa [SQL-tietokannan Vipuventtiili V12 uudet ominaisuudet](sql-database-v12-whats-new.md).

- Lisätietoja ohjaimen saatavuus ja SQL-tietokannan tuki on ohjeaiheessa [Yhteyden kirjastojen SQL-tietokantaan ja SQL Server](sql-database-libraries.md).

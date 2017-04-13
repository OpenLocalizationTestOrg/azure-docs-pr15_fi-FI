<properties
   pageTitle="SQL-tietokannan varmuuskopiointi - automaattisen ja geo tarpeettomat | Microsoft Azure" 
   description="SQL-tietokanta luo paikallisen tietokannan varmuuskopion viiden minuutin välein ja käyttää Azure lukuoikeudet geo tarpeettomat tallennuspaikkaa (RA GRS) tarjoaa geo luotettavuutta automaattisesti. "
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/20/2016"
   ms.author="carlrab;barbkess"/>

<!------------------
This topic is annotated with TEMPLATE guidelines for FEATURE TOPICS.


Metadata guidelines

pageTitle
    60 characters or less. Includes name of the feature - primary benefit. Not the same as H1. Its 60 characters or fewer including all characters between the quotes and the Microsoft Azure site identifier.

description
    115-145 characters. Duplicate of the first sentence in the introduction. This is the abstract of the article that displays under the title when searching in Bing or Google. 

    Example: "SQL Database automatically creates a local database backup every few minutes and uses Azure read-access geo-redundant storage for geo-redundancy."
------------------>

<!----------------

TEMPLATE GUIDELINES for feature topics

The Feature Topic is a one-pager (ok, sometimes longer) that explains a capability of the product or service. It explains what the capability is and characteristics of the capability.  

It is a "learning" topic, not an action topic.

DO explain this:
    • Definition of the feature terminology.  i.e., What is a database backup?
    • Characteristics and capabilities of the feature. (How the feature works)
    • Common uses with links to overview topics that recommend when to use the feature.
    • Reference specifications (Limitations and Restrictions, Permissions, General Remarks, etc.)
    • Next Steps with links to related overviews, features, and tasks.

DON'T explain this:
    • How to steps for using the feature (Tasks)
    • How to solve business problems that incorporate the feature (Overviews)
------------------->

<!------------------
GUIDELINES for the H1 
    
    The H1 should answer the question "What is in this topic?" Write the H1 heading in conversational language and use search key words as much as possible. Since this is a learning topic, make sure the title indicates that and doesn't mislead people to think this will tell them how to do tasks.  
    
    To help people understand this is a learning topic and not an action topic, start the title with "Learn about ... "

    Heading must use an industry standard term. If your feature is a proprietary name like "Elastic database pools", use a synonym. For example:    "Learn about elastic database pools for multi-tenant databases". In this case multi-tenant database is the industry-standard term that will be an anchor for finding the topic.

-------------------->

# <a name="learn-about-sql-database-backups"></a>Lisätietoja SQL-tietokannan varmuuskopiointi

<!------------------
    GUIDELINES for introduction
    
    The introduction is 1-2 sentences.  It is optimized for search and sets proper expectations about what to expect in the article. It should contain the top key words that you are using throughout the article.The introduction should be brief and to the point of what the feature is, what it is used for, and what's in the article. 

    If the introduction is short enough, your article can pop to the top in Google Instant Answers.

    In this example:
    
 

Sentence #1 Explains what the article will cover, which is what the feature is or does. This is also the metadata description. 
    SQL Database automatically creates a local database backup every five minutes and uses Azure read-access geo-redundant storage (RA-GRS) to provide geo-redundancy. 

Sentence #2 Explains why I should care about this.  
    Database backups are an essential part of any business continuity and disaster recovery strategy because they protect your data from accidental corruption or deletion.

-------------------->

SQL-tietokanta luo paikallisen tietokannan varmuuskopion muutaman minuutin välein ja käyttää Azure lukuoikeudet geo tarpeettomat tallennustilan geo redundancy automaattisesti. Tietokannan varmuuskopiointi ovat kaikki liiketoiminnan jatkuvuuden ja tietojen palauttamisen strategia olennainen osa, koska tietojen suojaaminen vahingossa vioittumisen tai poistaminen. 

<!-- This image needs work, so not putting it in right now.

This diagram shows SQL Database running in the US East region. It creates a database backup every five minutes, which it stores locally to Azure Read Access Geo-redundant Storage (RA-GRS). Azure uses geo-replication to copy the database backups to a paired data center in the US West region.

![geo-restore](./media/sql-database-geo-restore/geo-restore-1.png)

-->

<!---------------
GUIDELINES for the first ## H2.

    The first ## describes what the feature encompasses and how it is used. It points to related task articles.
    
    For consistency, being the heading with "What is ... "
----------------->

## <a name="what-is-a-sql-database-backup"></a>Mikä on SQL-tietokannan varmuuskopiointi?  

<!-- 
    Explains what a SQL Database backup is and answers an important question that people want to know.
-->

SQL-tietokannan varmuuskopiointi sisältää paikallisen tietokannan varmuuskopiointi- ja geo tarpeettomat varmuuskopiot. Nämä varmuuskopiot luodaan automaattisesti ja lisää maksutta. Ei tarvitse tehdä mitään, jotta ne tapahtua.

<!----------------- 
    Explains first component of the backup feature
------------------>

SQL-tietokanta käyttää paikallisen varmuuskopioiden hakeminen SQL Server-tekniikkaa [koko](https://msdn.microsoft.com/library/ms186289.aspx), [Eroavuus](https://msdn.microsoft.com/library/ms175526.aspx )ja [Tapahtumaloki](https://msdn.microsoft.com/library/ms191429.aspx) varmuuskopioita. Tapahtuman Lokin varmuuskopioiden käydä viiden minuutin välein, jonka avulla voit tehdä ajankohta palvelimeen, joka isännöi tietokannan palautuksen. Tietokannan palauttaminen, kun palvelua esiintyy, mitä kokonaan, eroa ja tapahtuman lokia varmuuskopioiden täytyy voi palauttaa.

<!--------------- 
    Explicit list of what to do with a local backup. "Use a ..." helps people to scan the topic and find the uses quickly.
---------------->

Käytä paikallisen tietokannan varmuuskopiointi:

- -Ajankohta, tietokannan palauttaminen säilytysajan kuluessa. Voit palauttaa tietokannan-ajankohta, poistetun tietokannan palauttaminen ajaksi, se on poistettu tai tietokannan palauttaminen jonkin muun maantieteellisen alueen tietokannan varmuuskopiointi. Katso palauttamisen suorittamiseen [tietokannan tietokannan varmuuskopiosta palauttaminen](sql-database-recovery-using-backups.md).

- Kopioi SQL server saman tai toisen alueen tietokannan. Kopioi vastaa muulla tavoin nykyisen SQL-tietokantaan. Suorittaa kopion, on artikkelissa [tietokannan kopiota](sql-database-copy.md).

- Voit arkistoida tietokannan varmuuskopiointi varmuuskopion säilytysaika lisäksi. Jos haluat suorittaa arkisto, [Vie on SQL-tietokanta BACPAC](sql-database-export.md) tiedosto. Voit arkistoida pitkään säilöön BACPAC ja tallentaa sen säilytys päättymisen jälkeen. Tai BACPAC avulla voit siirtää tietokannan kopion, SQL Serveristä, joko paikallisessa tai Azure virtuaalikoneen (AM).

<!----------------- 
    Explains first component of the backup feature
------------------>

SQL-tietokanta käyttää geo tarpeettomat varmuuskopioiden hakeminen [Azuren tallennustilaan replikoinnin](../storage/storage-redundancy.md). SQL-tietokanta tallentaa paikallisen tietokannan varmuuskopiointi tiedostojen [Lukuoikeus Geo tarpeettomat Storage (RA GRS)](../storage/storage-redundancy.md#read-access-geo-redundant-storage) -tili. Azure kopioi [pisteparin tietokeskuksen](../best-practices-availability-paired-regions.md)varmuuskopiot. 

<!--------------- 
    Explicit list of what to do with a geo-redundant backup. "Use a ..." helps people to scan the topic and find the uses quickly.
---------------->

Käytä geo tarpeettomat varmuuskopiointi:

- Palauttaa tietokannan eri maantieteellisen alueen siltä varalta, että et voi käyttää tietokannan varmuuskopiointi ensisijainen tietokanta-alueelta. 

>[AZURE.NOTE] Azuren tallennustilaan termin *replikoinnin* viittaa tiedostojen kopioiminen sijainnista toiseen. Käyttäjän SQL- *tietokannan replikoiminen* viittaa pitäminen useita toissijainen tietokantoja, jotka on synkronoitu ensisijaisen tietokannan kanssa. 

<!----------------
    The next ## H2's discuss key characteristics of how the feature works. The title is in conversational language and asks the question that will be answered.
------------------->
## <a name="how-much-backup-storage-is-included-at-no-cost"></a>Tallennustilan määrän varmuuskopioinnin sisältyy ilmaiseksi?

SQL-tietokanta on enintään 200 % tallennustilan enimmäismäärä valmistellun tietokannan varmuuskopiointi tallennustilan ilman lisäkustannuksia nimellä. Esimerkiksi jos sinulla on valmisteltu DB kokoisena vakio DB esiintymän 250 gigatavua, sinun on 500 gt: n varmuuskopion tallennustilan muita maksutta. Jos tietokannassa on suurempi kuin annettu varmuuskopion tallennustilan, voit pienentää säilytysaika ottamalla yhteyttä tukipalveluun Azure. Toinen vaihtoehto on erittäin varmuuskopion tallennustilaa, joka on lukuoikeudet maantieteellisesti tarpeettomat Storage (RA GRS) Normaalikorvaus laskutetaan maksaa. 

## <a name="how-often-do-backups-happen"></a>Kuinka usein varmuuskopiot johtuu?

Paikallisen tietokannan varmuuskopiointi koko tietokannan varmuuskopioita käydä viikoittain eroavuus tietokannan varmuuskopioita käynnistyvät hourly ja tapahtumaloki varmuuskopioiden tapahtua viiden minuutin välein. Ensimmäinen täydellinen varmuuskopio on varattu heti, kun tietokanta on luotu. Se on valmis yleensä 30 minuutin kuluessa, mutta se voi kestää kauemmin, kun tietokanta on merkittäviä koon. Esimerkiksi alkuperäisen varmuuskopioinnin saattaa toimia hitaasti palautettu tietokantaan tai tietokannan kopion. Ensimmäisen täyden varmuuskopioinnin jälkeen kaikki edelleen varmuuskopiot ajoitetaan automaattisesti ja hallita äänettömästi taustalla. Tietokannan varmuuskopioinnin täyden ja [eroavuuskopioinnin](https://msdn.microsoft.com/library/ms175526.aspx) tarkan ajoituksen määräytyy sen saldojen yleistä järjestelmän työmäärää. 

Geo tarpeettomat varmuuskopioiden hakeminen varmuuskopioinnin koko ja eroavuuskopioinnin kopioidaan Azuren tallennustilaan replikoinnin aikataulun mukaan.

## <a name="how-long-do-you-keep-my-backups"></a>Kuinka kauan Omat varmuuskopiot säilyttää?

Kunkin SQL-tietokannan varmuuskopiointi on säilytysajan jälkeinen aika, joka perustuu tietokannan [palvelun taso](sql-database-service-tiers.md) . Tietokannan säilytysaika:

<!------------------

    Using a list so the information is easy to find when scanning.
------------------->

- Tavallinen palvelutaso on seitsemän päivän aikana.
- Normaali palvelutaso on 35 päivää.
- Premium palvelutaso on 35 päivää.


Jos vakio- tai Premium-palvelutasot-tietokannan Basic aikaisempaan tiedostomuotoon, varmuuskopiot tallennetaan seitsemän päivän ajan. Kaikki olemassa olevat varmuuskopiot yli seitsemän päivää vanhoja eivät ole enää käytettävissä. 

Jos päivität tietokannan Basic palvelun tason standardiksi tai Premium-SQL-tietokantaan pitää varmuuskopioon, kunnes ne ovat 35 päivää. Uusi varmuuskopioiden pitää ilmenee 35 päivää.
 
Jos poistat tietokannan, SQL-tietokantaan pitää varmuuskopioista samalla tavalla kuin se kuin online-tietokannasta. Oletetaan esimerkiksi, että poistat Basic tietokantaa, jossa on seitsemän päivän säilytysaika. Varmuuskopiointi, joka on neljä päivää vanhoja tallentuu kolme päivää.

>[AZURE.IMPORTANT]
    Jos poistat Azure SQL-palvelimeen, joka isännöi SQL-tietokantoja, kaikki tietokannat, jotka kuuluvat palvelimeen poistetaan eikä sitä voi palauttaa. Et voi palauttaa poistettuja palvelimeen.

<!-------------------
OPTIONAL section
## Best practices 
--------------------->

<!-------------------
OPTIONAL section
## General remarks
--------------------->

<!-------------------
OPTIONAL section
## Limitations and restrictions
--------------------->

<!-------------------
OPTIONAL section
## Metadata
--------------------->

<!-------------------
OPTIONAL section
## Performance
--------------------->

<!-------------------
OPTIONAL section
## Permissions
--------------------->

<!-------------------
OPTIONAL section
## Security
--------------------->

<!-------------------
GUIDELINES for Next Steps

    The last section is Next Steps. Give a next step that would be relevant to the customer after they have learned about the feature and the tasks associated with it.  Perhaps point them to one or two key scenarios that use this feature.

    You don't need to repeat links you have already given them.
--------------------->

## <a name="next-steps"></a>Seuraavat vaiheet

Tietokannan varmuuskopiointi ovat kaikki liiketoiminnan jatkuvuuden ja tietojen palauttamisen strategia olennainen osa, koska tietojen suojaaminen vahingossa vioittumisen tai poistaminen. Näet miten tietokannan varmuuskopiointi kyselyjä laajempi strategia, katso [liiketoiminnan jatkuvuuden yhteenveto](sql-database-business-continuity.md).



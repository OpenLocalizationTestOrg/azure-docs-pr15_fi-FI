<properties
   pageTitle="Azure SQL-tietokanta muodostaa usean vuokraajan sovellusten eristystaso ja tehokkuutta"
   description="Lue, miten SQL-tietokanta muodostaa usean vuokraajan sovellukset"
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
   ms.workload="data-management"
   ms.date="10/13/2016"
   ms.author="carlrab"/>

# <a name="builds-multi-tenant-apps-with-azure-sql-database-with-isolation-and-efficiency"></a>Muodostaa usean vuokraajan sovellukset käyttämällä Azure SQL-tietokantaa, jossa eristystaso ja tehokkuutta

## <a name="leverage-elastic-pools-and-build-more-efficient-multi-tenant-apps"></a>Hyödyntää joustavasti jakavat ja tehokkaampaa usean vuokraajan sovellusten luominen

Jos ole SaaS keskihajonta kirjoittaminen usean vuokraajan-sovellus ja käsittelemisen useille asiakkaille, usein tekemäsi kompromissien asiakkaan suorituskyvyn, hallinnasta ja suojaus. Azure SQL tietokanta joustavasti tietokannan jakavat, jossa voit ei enää ole varmistaaksesi, että haavoittuvan. Nämä jakavat avulla voit hallita usean vuokraajan sovellusten valvonta ja saada eristystaso yhtä Käyttömukavuuden tietokannan edut. Katso [rakenne kuviot usean vuokraajan SaaS sovellusten Azure SQL-tietokanta](sql-database-design-patterns-multi-tenancy-saas-applications.md).

![Muodosta-usean-vuokraajan-sovellukset](./media/sql-database-build-multi-tenant-apps/sql-database-build-multi-tenant-apps.png)

## <a name="auto-scaling-you-control"></a>Automaattinen skaalaus käyttöoikeuksia voidaan hallita

Jakavat Skaalaa automaattisesti suorituskyky ja tallennustilaa joustavasti tietokantojen suoraan selaimessa. Voit hallita resurssivarantoon myönnetyt suorituskyvyn, Lisää tai poista joustavasti tietokantojen pyydettäessä ja määrittää joustavasti tietokantojen suorituskykyä vaikuttamatta altaan kokonaiskustannukset. Tällöin sinun ei tarvitse olla huolissaan siitä, että yksittäiset tietokannat käyttö hallinta.

[Lue ohjeet](sql-database-elastic-pool.md)

## <a name="intelligent-management-of-your-environment"></a>Älykäs ympäristön hallinta

Valmiin koonmuuttokahvan suosituksia tunnistaa itse tietokannoissa, jotka jakavat siitä. Näiden suositusten Salli "entä-jos-analyysin nopeasti optimointi, jotta suorituskyvyn. RTF suorituskyvyn seurantaa ja raporttinäkymien vianmääritys auttaa visualisointi historiallisten resurssivarantoon käyttö.

[Lue ohjeet](sql-database-elastic-pool-guidance.md)

## <a name="performance-and-price-to-meet-your-needs"></a>Suorituskyky ja hinta vastaamaan omia tarpeita

Perustiedot, Vakio, ja Premium jakavat antaa sinulle laaja suorituskyvyn, tallennustilan ja hinnoittelu asetukset. Jakavat voi olla enintään 400 joustavasti tietokannat. Joustavasti tietokantojen voit Automaattinen-skaalaus enintään 1 000 joustavasti tietokannan tapahtuman yksiköt (eDTU).

[Lue ohjeet](https://azure.microsoft.com/pricing/details/sql-database/?b=16.50)

## <a name="elastic-tools"></a>Joustavasti Työkalut

Lisäksi joustavasti jakavat on SQL-tietokantaan ominaisuuksia, jotka auttavat hallitsemaan useiden tietokantojen toiminnallisia toimintoja:

**Suorita rajat tietokantakyselyt ja raportointia varten.**  
[Joustavasti tietokantakyselyn](sql-database-elastic-query-overview.md) avulla voit suorittaa kyselyjä ja raportteja yli joustavasti resurssivarantoon tietokantojen ja käyttää remote kerralla lisääminen resurssivarantoon useita tietokantoja tallennettuja tietoja.

**Suorita cross tietokannan tapahtumia.**  
[Joustavasti tietokannan tapahtumia](sql-database-elastic-transactions-overview.md) avulla voit suorittaa tapahtumia, jotka ulottuvat useiden tietokantojen SQL-tietokannat ja suorittaa (eli, kun käsitteleminen taloudellisten tapahtumien tietokannat tai päivittää yhden tietokannan ja tilaukset varaston).

**Suorita useiden tietokantojen toiminnoista.**  
[Joustavasti tietokannan työt](sql-database-elastic-jobs-overview.md) Suorita järjestelmänvalvojan toimintoja, kuten joiden indeksit vai päivitätkö rakenteet eri tietokantojen joustavasti varannon.

Siirry muita SQL-tietokanta on oikeus tarjota kotisivulle.
[Kuittaa ulos](https://azure.microsoft.com/services/sql-database/) 

## <a name="next-steps"></a>Seuraavat vaiheet

Hanki [ilmainen Azure tilaus](https://azure.microsoft.com/get-started/) ja [ensimmäisen Azure SQL-tietokannan luominen](sql-database-get-started.md).

## <a name="additional-resources"></a>Lisäresursseja

Selaa kaikkia [ominaisuuksia SQL-tietokantaan](https://azure.microsoft.com/services/sql-database/).
 
Tarkista [SQL-tietokantaan teknisiä tietoja](sql-database-technical-overview.md).  

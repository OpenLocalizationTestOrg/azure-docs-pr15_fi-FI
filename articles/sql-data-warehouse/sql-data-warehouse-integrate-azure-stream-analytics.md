<properties
   pageTitle="Azure Stream Analytics käyttäminen SQL-tietovarasto | Microsoft Azure"
   description="Azure Stream Analytics käyttäminen Azure SQL-tietovarasto, ratkaisujen kehittämiseen liittyviä vinkkejä."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="kevinvngo"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/16/2016"
   ms.author="kevin;barbkess;sonyama"/>

# <a name="use-azure-stream-analytics-with-sql-data-warehouse"></a>SQL-tietovarasto Azure Stream Analytics käyttäminen

Azure Stream Analytics on täysin hallitun palvelu tarjoaa pieni viive, helposti saatavilla, skaalattava monimutkaisia tapahtuman käsittely päälle streaming tietojen pilveen. Voit lukea perustiedot lukemalla [Azure Stream Analytics esittely][]. Saat sitten luominen lopusta loppuun-ratkaisun Stream Analytics [käyttäminen Azure Stream Analytics][] -opetusohjelma.

Tässä artikkelissa kerrotaan Azure SQL-tietovarasto tietokannan käyttämisestä kuin tulosteen käsittelytoiminto höyryn Analytics-töiden.

## <a name="prerequisites"></a>Edellytykset

Tee seuraavien vaiheiden läpi ensin [käyttäminen Azure Stream Analytics][] -opetusohjelman.  

1. Luo tapahtumaa-toiminnossa-syöte
2. Määritettäessä ja käynnistettäessä tapahtuman luontitoiminnon sovelluksen
3. Valmistele Stream Analytics-työ
4. Määritä projektin lisätiedot ja kysely

Luo sitten Azure SQL-tietovarasto-tietokanta

## <a name="specify-job-output-azure-sql-data-warehouse-database"></a>Määritä projektin tulos: Azure SQL-tietovarasto tietokanta

### <a name="step-1"></a>Vaihe 1

Virta Analytics-työn **tulostus** sivun yläreunasta ja valitse sitten **Lisää tulos**.

### <a name="step-2"></a>Vaihe 2

Valitse SQL-tietokanta ja valitse sitten Seuraava.

![][add-output]

### <a name="step-3"></a>Vaihe 3
Kirjoita seuraavalla sivulla seuraavat arvot:

- *Tunnus*: Kirjoita kutsumanimi työ-tulostusta varten.
- *Tilauksen*:
    - Jos tietovarasto SQL-tietokanta on samassa muodossa Analytics-työ-tilauksen, valitse Käytä SQL-tietokantaan voimassa oleva tilaus.
    - Jos tietokannassa on eri tilauksen, valitse Käytä SQL-tietokannasta toiseen tilaukseen.
- *Tietokanta*: määrittää kohdetietokannan nimen.
- *Palvelimen nimi*: Määritä vain määritetty tietokanta palvelimen nimi. Voit käyttää Azure perinteinen portaalin haettava kohde.

![][server-name]

- *Käyttäjänimi*: tilille, jolla on kirjoitusoikeudet tietokannan käyttäjän nimi.
- *Salasana*: antaa määritetyn käyttäjän salasana.
- *Taulukko*: määrittää tietokannan nimen kohdetaulukkoon.

![][add-database]

### <a name="step-4"></a>Vaihe 4

Valitse Tarkista-painiketta, voit lisätä työ-tulostus ja varmista, että Stream Analytics onnistuneesti muodostaa yhteyden tietokantaan.

![][test-connection]

Tietokannan yhteys on muodostettu, näet ilmoituksen portaalin alareunassa. Testaa yhteys valitsemalla Testaa yhteys tietokantaan alaosassa.

## <a name="next-steps"></a>Seuraavat vaiheet

Katso yleiskuvaus integrointi on [SQL-tietovarasto integroinnin yleiskatsaus][].

Artikkelissa kehitys Lisävihjeitä [SQL-tietovarasto kehittäminen yleiskatsaus][].

<!--Image references-->

[add-output]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/add-output.png
[server-name]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/dw-server-name.png
[add-database]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/add-database.png
[test-connection]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/test-connection.png

<!--Article references-->

[Azure Stream Analytics esittely]: ../stream-analytics/stream-analytics-introduction.md
[Azure Stream Analytics käytön aloittaminen]: ../stream-analytics/stream-analytics-get-started.md
[SQL-tietovarasto kehittäminen yleiskatsaus]:  ./sql-data-warehouse-overview-develop.md
[SQL-tietovarasto integroinnin yleiskatsaus]:  ./sql-data-warehouse-overview-integrate.md

<!--MSDN references-->

<!--Other Web references-->
[Azure Stream Analytics documentation]: http://azure.microsoft.com/documentation/services/stream-analytics/

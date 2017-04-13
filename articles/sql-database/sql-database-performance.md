<properties 
   pageTitle="Azure SQL-tietokannan suorituskykyä tietoja | Microsoft Azure" 
   description="Azure SQL-tietokanta on suorituskyky-työkaluja, joiden avulla voit tunnistaa aluetta, joka voi parantaa nykyisen kyselyn suorituskykyä." 
   services="sql-database" 
   documentationCenter="" 
   authors="stevestein" 
   manager="jhubbard" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management" 
   ms.date="07/19/2016"
   ms.author="sstein"/>

# <a name="sql-database-performance-insight"></a>SQL-tietokannan suorituskykyä tietoja

Azure SQL-tietokanta on suorituskyky-työkaluja, joiden avulla voit tunnistaa ja suorituskyvyssä tietokantoja antamalla älykkäät säätäminen toiminnot ja suositukset. 

1. Siirry [Azure Portal](http://portal.azure.com) -tietokannan ja valitse **kaikki asetukset** > **suorituskyvyn **  >  **Yleiskatsaus** Avaa **Suorituskyky** -sivu. 


2. Avaa [SQL-tietokannan Advisor](#sql-database-advisor) **suosituksia** ja valitse Avaa [Kyselyn suorituskykyä tietoja](#query-performance-insight) **Kyselyt** .

    ![Näytä suorituskyky](./media/sql-database-performance/entries.png)



## <a name="performance-overview"></a>Suorituskyvyn yleiskatsaus

**Yleistä** tai **Suorituskyky** -ruutu valitsemalla siirtää sinut tietokannan suorituskykyä koontinäyttö. Tässä näkymässä yhteenvedon tietokannan suorituskykyä ja auttaa suorituskyvyn parantaminen ja vianmääritys. 

![Suorituskyky](./media/sql-database-performance/performance.png)

- **Suositukset** -ruutu sisältää säätäminen suosituksia tietokannan jakautuminen (ylhäältä 3 suosituksia näkyvät ole Lisää). Tämä ruutu valitsemalla Avaa **Advisor SQL-tietokantaan**. 
- **Säädön tehtävän** -ruutu on meneillään ja valmiit säätäminen tietokannan toiminnot, jossa Pikakatselu historiatiedot säätäminen tehtävän siirtäminen. Tämä ruutu valitsemalla Avaa koko säätäminen historia-näkymä, jos tietokanta.
- **Automaattinen säätö** -ruutu näyttää (säätäminen toimenpiteet on määritetty käytetään automaattisesti tietokannan) tietokannan automaattinen säätö-määritykset. Automaatio-asetusten valintaikkuna valitsemalla tämä ruutu avautuu.
- **Tietokantakyselyt** -ruutu näyttää yhteenvedon kyselyn suorituskykyä tietokannan (Yleinen DTU käyttö- ja top resurssi muissa kyselyt). Tämä ruutu valitsemalla Avaa **Kyselyn suorituskykyä tietoja**.



## <a name="sql-database-advisor"></a>SQL-tietokannan tukipalvelu


[SQL-tietokannan Advisor](sql-database-advisor.md) on älykäs säätäminen suositukset, joka voi parantaa tietokannan suorituskykyä. 

- Suosituksia, joka indeksoi luomiseen ja pudota (ja soveltaa automaattisesti ilman käyttäjän toimia ja automaattisesti peruutetaan suositukset, joka on negatiivinen vaikutus suorituskykyä indeksin suosituksia).
- Suosituksia rakenteen ongelmat on määritetty tietokannassa.
- Suosituksia kyselyt voi olla hyötyä Parametroitu kyselyjen.




## <a name="query-performance-insight"></a>Kyselyn suorituskykyä tietoja

[Kyselyn suorituskykyä tietoja](sql-database-query-performance.md) voit aikaa tietokannan suorituskyvyn vianmääritys tarjoamalla:

- Tietokantojen resurssi (DTU)-kulutus tarkempaa tietoja. 
- Ylimmät suorittimen käyttö kyselyjä, jolloin voit optimoitu mahdollisesti entistä parempi suorituskyky. 
- Voi siirtyä alaspäin kyselyn. 


## <a name="additional-resources"></a>Lisäresursseja

- [Azure SQL-tietokannan suorituskykyä ohjeet yksittäisen tietokantoja](sql-database-performance-guidance.md)
- [Kun joustavasti tietokannan resurssivarantoon voidaan käyttää?](sql-database-elastic-pool-guidance.md)
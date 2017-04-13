<properties
    pageTitle="Valvoa XTP ladatun tallennustilan | Microsoft Azure"
    description="Arviota ja näytön XTP ladatun tallennustilan käyttää, kapasiteetin; Ratkaise virhe kapasiteetin 41823"
    services="sql-database"
    documentationCenter=""
    authors="jodebrui"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="jodebrui"/>


# <a name="monitor-in-memory-oltp-storage"></a>Näytön OLTP ladatun tallennustilan

[Ladatun OLTP](sql-database-in-memory.md)käytettäessä muistin optimoitu taulukoiden ja taulukon muuttujat tiedot sijaitsevat ladatun OLTP tallennustilaan. Kunkin Premium-palvelutaso on OLTP ladatun tallennustilan enimmäiskoko, joka on kuvattu [artikkelissa SQL-tietokannan palvelutasot](sql-database-service-tiers.md#service-tiers-for-single-databases). Kun tämä raja ylittyy, lisääminen ja päivittäminen toiminnot voi aloittaa puuttuessa (virheeseen 41823). Senhetkinen sinulle on joko poistaa tietoja Vapauta muistia tai päivittää tietokannan suorituskyky-taso.

## <a name="determine-whether-data-will-fit-within-the-in-memory-storage-cap"></a>Määrittää, onko tiedot sopivat sisällä ladatun tallennustilan pää

Määrittää tallennustilan pää: Katso lisätietoja [SQL-tietokannan palvelutasot artikkelissa](sql-database-service-tiers.md#service-tiers-for-single-databases) eri Premium palvelutasot tallennustilan näppäinyhdistelmää CAPS LOCK.

Arvioidaan muistin vaatimukset muistin optimoitu taulukon tekee, SQL Server samalla tavalla kuin se, onko Azure SQL-tietokantaan. Kestää muutaman minuutin tarkistettava [MSDN](https://msdn.microsoft.com/library/dn282389.aspx)-aihetta.

Huomaa, että-taulukon ja muuttujien rivien sekä indeksien laskeminen max käyttäjän tietoja koko kohti. Lisäksi ALTER TABLE on tarpeeksi tilaa, voit luoda uuden version koko taulukko ja sen indeksit.

## <a name="monitoring-and-alerting"></a>Seuranta- ja ilmoitat

Voit valvoa ladatun tallennustilan käyttö prosentteina [tallennustilan cap suorituskyvyn tason](sql-database-service-tiers.md#service-tiers-for-single-databases) Azure- [portaalissa](https://portal.azure.com/): 

- Valitse tietokanta-sivu Etsi resurssien käyttö-ruutu ja valitse sitten Muokkaa.
- Valitse lisätiedot `In-Memory OLTP Storage percentage`.
- Jos haluat lisätä ilmoituksen, valitsemalla Avaa metrisillä sivu resurssien käyttö-valintaruutu ja valitse sitten Lisää ilmoitus.

Tai Näytä ladatun tallennustilan käyttö seuraavan kyselyn avulla:

    SELECT xtp_storage_percent FROM sys.dm_db_resource_stats


## <a name="correct-out-of-memory-situations---error-41823"></a>Korjaa muistia out tilanteissa - virhe 41823

Käynnissä muistia out tulokset Lisää, Päivitä ja luo toimintoja, ja näyttöön tulee virhesanoma 41823 kaatuvat.

Virhesanoma 41823 ilmaisee, että muistin optimoitu taulukot ja taulukon muuttujat ylittänyt enimmäiskoon.

Voit korjata tämän virheen joko:


- Tietojen poistaminen muistin optimoitu-taulukoita, mahdollisesti purkaminen tiedot perinteinen, levylle taulukoihin. Vaihtoehtoisesti
- Päivitä palvelun tason jokin tarpeeksi ladatun tallennustilan haluat säilyttää muistin optimoitu taulukoiden tietojen kanssa.

## <a name="next-steps"></a>Seuraavat vaiheet
Lisäresursseja tietoja [Seuranta Azure SQL-tietokannan](sql-database-monitoring-with-dmvs.md) käyttämisestä dynaaminen hallinta näkymät

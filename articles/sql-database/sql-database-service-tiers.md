<properties
    pageTitle="SQL-tietokannan suorituskykyä ja asetukset: palvelun tasoa | Microsoft Azure"
    description="Vertaa palvelutasot, saldo kustannus- ja ominaisuuksien, kuten kokoa SQL-tietokantaan suorituskyky ja liiketoiminnan jatkuvuuden piirteet."
    keywords="tietokanta-asetukset, tietokannan suorituskykyä"
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor="CarlRabeler"/>

<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="08/10/2016"
    ms.author="carlrab"/>

# <a name="sql-database-options-and-performance-understand-whats-available-in-each-service-tier"></a>SQL-tietokanta-asetukset ja suorituskyky: ymmärtää, mikä on käytettävissä palvelun kunkin tason

[Azure SQL-tietokanta](sql-database-technical-overview.md) on kolme palvelutasot useita suorituskyvyn määritetyt käsittelemään eri toiminnoista. Suorituskyvyn tasoissa sisältää kasvava resurssien suunniteltu yhä suurempi siirtonopeuden aikana. Voit hallita omaa [palvelutaso](sql-database-service-tiers.md#standalone-database-service-tiers-and-performance-levels) omassa suorituskyvyn viereiset kunkin tietokannan. Voit hallita useiden tietokantojen [joustavasti varannon](sql-database-service-tiers.md#elastic-pool-service-tiers-and-performance-in-edtus) resursseja jaettu joukko. Erillisestä tietokannoissa käytettävissä olevien resurssien ilmaistaan myyntialueella tietokannan tapahtuman (DTUs) ja joustavasti jakavat joustavasti DTUs tai eDTUs. Saat lisätietoja DTUs ja eDTUs-artikkelissa [DTU ominaisuudet](sql-database-what-is-a-dtu.md). 

Kummassakin tapauksessa palvelutasot sisältää **Perustiedot**, **Vakio**ja **Premium**. Nämä tasoa tietokanta-asetukset ovat erillinen tietokantojen ja joustavasti jakavat, mutta on huomioitava joustavasti jakavat varten. Tässä artikkelissa on erillinen tietokantojen ja joustavasti jakavat palvelutasot tiedot.

## <a name="service-tiers-and-database-options"></a>Palvelutasot ja tietokanta-asetukset
Perustiedot, Vakio, ja Premium palvelutasot kaikilla on toiminta-aika 99,99 % SLA ja tarjota ennakoitavissa suorituskyvyn, joustava liiketoiminnan jatkuvuuden asetukset tai suojaustoiminnot tunnin välein laskutus. Seuraavassa taulukossa on esimerkkejä parhaat tasoa toiseen sovellukseen työmääriä sopiviksi.

| Palvelutaso | Kohteen toiminnoista |
|---|---|
| **Perustoiminnot** | Parhaiten soveltuu pieni tietokantaan, tukevat yleensä yhdellä aktiivinen toimenpiteellä tiettynä hetkenä. Esimerkkejä tällaisista tiedostoista ovat tietokantojen käytettäviä kehittäminen tai testaaminen tai pieniä harvoin sovellukset. |
| **Vakio** | Useimpien cloud sovellusten tukeminen samanaikainen kyselyjen Siirry voit vaihtoehto. Esimerkkejä tällaisista tiedostoista ovat työryhmän tai web-sovelluksia. |
| **Premium** | Suunniteltu tapahtumien määrää, tukevat monia käyttäjiä ja lisää parhaan mahdollisen jatkuvuuden ominaisuuksia. Esimerkkejä kriittinen tehtävä-sovellukset tukevat tietokannat. |

>[AZURE.NOTE] Web- ja Business-versiot on poistettu käytöstä. Lue [Auringonlasku usein kysytyt kysymykset](https://azure.microsoft.com/pricing/details/sql-database/web-business/) , jos aiot käyttää Web- ja Business-versiot.

## <a name="standalone-database-service-tiers-and-performance-levels"></a>Erillisestä tietokannan palvelutasot ja suorituskyvyn tasoon
Erillisestä tietokantoja on useita suorituskyvyn tasojen palvelun kunkin tason. Voit halutessasi valita tasoa, joka vastaa parhaiten oman työmäärän. Jos haluat skaalata ylös tai alas, voit helposti muuttaa tietokannan tasoa. Lisätietoja [muuttaminen tietokannan palvelutasot ja suorituskykyä](sql-database-scale-up.md) .

Tässä luettelossa suorituskyvyn ominaisuudet koskevat tietokantoja, jotka on luotu käyttäen [SQL-tietokannan Vipuventtiili V12](sql-database-v12-whats-new.md). Riippumatta siitä, tietokantaa määrä tietokannan saa resurssien taattua joukon ja tietokannan suorituskykyominaisuudet näin ei käy.

[AZURE.INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

>[AZURE.NOTE] Katso kaikkien muiden tämän palvelun tasoa taulukon rivien tarkan [palvelun taso ominaisuudet ja rajoitukset](sql-database-performance-guidance.md#service-tier-capabilities-and-limits).

## <a name="elastic-pool-service-tiers-and-performance-in-edtus"></a>Joustavasti resurssivarantoon palvelutasot ja eDTUs suorituskykyä
Luominen ja skaalausta itsenäisen tietokannan lisäksi käytössä on myös mahdollisuus hallita useita tietokantoja [joustavasti ryhmän](sql-database-elastic-pool.md)sisällä. Kaikki tietokannat joustavasti sarjassa on yhteisiä resursseja. Suorituskyvyn ominaisuudet mitataan *Joustavasti tietokannan tapahtuman yksiköt* (eDTUs) mukaan. Kun erillinen tietokantoja jakavat olla kolme palvelutasot: **Perustiedot**, **Vakio**ja **Premium**. Nämä kolme palvelutasot määrittää edelleen suorituskykyyn rajoitukset ja useita ominaisuuksia jakavat.

Jakavat Salli tietokantoja, jakaa ja käyttää DTU resursseja ilman hänen nimeään tarvitse lisätä liittää kunkin ryhmän tietokannan suorituskykyä. Esimerkiksi vakio resurssivarantoon erillinen-tietokantaan voit siirtyä 0 eDTUs, voit määrittää, kun määrität altaan suurin tietokannan eDTU avulla. Jakavat sallii useiden tietokantojen erilaisten työmääriä eDTU resursseista avulla tehokkaasti koko ryhmän kanssa. Katso [hinnan ja suorituskyvyn Huomioitavaa joustavasti resurssivarannon](sql-database-elastic-pool-guidance.md) tiedot.

Seuraavassa taulukossa on kuvattu resurssivarantoon palvelutasot ominaisuudet.

[AZURE.INCLUDE [SQL DB service tiers table for elastic pools](../../includes/sql-database-service-tiers-table-elastic-db-pools.md)]

Tietokantojen varannon noudattaa myös kyseisen tason erillinen tietokanta-ominaisuuksia. Esimerkiksi Basic varanto on max istuntojen rajoitukset resurssivarantoon 4800-28800 kohti, mutta yksittäisten tietokannan, Basic varannon 300 istuntojen tietokannan enimmäismäärä on.

## <a name="choosing-a-service-tier"></a>Service-tason valitseminen

DNS-palvelun ylätasolla, käynnistää sen selvittäminen, onko tietokannan pitäisi olla erillinen tietokanta tai olla osa joustavasti resurssivarantoon. 

### <a name="choosing-a-service-tier-for-a-standalone-database"></a>Palvelutaso erillinen tietokannan valitseminen

DNS-palvelutaso erillinen tietokantaan, käynnistää sen tietokannan ominaisuudet, joita sinun pitää valita SQL-tietokanta-version selvittäminen:

- Tietokannan enimmäiskoko 2 Gigatavua Basic, standardin 250 gt. ja 500 gt 1 TT enintään Premium - suorituskyky mukaan
- Tietokannan varmuuskopiointi säilytysaika (7 päivää Basic-standardia 35 päivää ja 35 päivää Premium)

Kun olet määrittänyt SQL-tietokanta-version, olet valmis (DTUs määrä)-tietokannan suorituskyky-tason määrittäminen. Voit arvata ja valitse sitten [skaalata ylös tai alas dynaamisesti](sql-database-scale-up.md) todellinen kokemuksen perusteella. Voit myös lähentää tarvitaan DTUs määrän [DTU Laskimen](http://dtucalculator.azurewebsites.net/) . 

### <a name="choosing-a-service-tier-for-an-elastic-database-pool"></a>Valitse palvelutaso joustavasti tietokannan resurssivarantoon varten.

DNS-palvelun ylätasolla joustavasti tietokannan resurssivarantoon varten, Aloita selvittämällä tietokannan ominaisuudet, joita sinun pitää valita, että ryhmän service-taso.

- Tietokannan koko (2 gt, Basic, 250 gt vakio- ja 500 gt: N Premium)
- Tietokannan varmuuskopiointi säilytysaika (7 päivää Basic-standardia 35 päivää ja 35 päivää Premium)
- Määrä tietokannat kohden resurssivarantoon (400 Basic, 400 standardin ja 50 Premium)
- Tallennustilan enimmäiskoko resurssivarantoon kohden (Basic-standardia, 1200 117 Gigatavua ja 750 Premium)

Kun olet määrittänyt palvelun tason lisääminen resurssivarantoon, olet valmis määrittämään suorituskyky-tason sovellussarjan (eDTUs). Voit arvata ja sitten [dynaamisesti skaalata tai skaalata](sql-database-elastic-pool-manage-portal.md#change-performance-settings-of-a-pool) perustuu todellinen kokemus. Voit lähentää DTUs tarvittavat kunkin tietokannan resurssivarantoon voit selvittää altaan yläraja määrän [DTU Laskimen](http://dtucalculator.azurewebsites.net/) .

## <a name="next-steps"></a>Seuraavat vaiheet
- Lue lisää nämä tasoa- [SQL-tietokannan hinnat](https://azure.microsoft.com/pricing/details/sql-database/)hinnat.
- Lue tietoja [joustavasti jakavat](sql-database-elastic-pool-guidance.md) ja [hinnan ja suorituskyvyn huomioon otettavia seikkoja joustavasti jakavat](sql-database-elastic-pool-guidance.md).
- Lisätietoja [näytön, hallinta- ja koon joustavasti jakavat](sql-database-elastic-pool-manage-portal.md) ja [näytön erillinen tietokantojen suorituskykyä](sql-database-single-database-monitor.md).
- Nyt kun osaat tietoja SQL-tietokanta-tasoa, kokeilla [ilmaisen tilin](https://azure.microsoft.com/pricing/free-trial/) ja opi [luomaan ensimmäinen SQL-tietokantaan](sql-database-get-started.md).

## <a name="additional-resources"></a>Lisäresursseja

- [Rakenne-kuvioiden määrittäminen usean vuokraajan SaaS sovellusten Azure SQL-tietokanta](sql-database-design-patterns-multi-tenancy-saas-applications.md)
- [Microsoft Virtual Academy videokurssin-joustavasti tietokannan ominaisuuksien Azure SQL-tietokantaan](https://mva.microsoft.com/en-US/training-courses/elastic-database-capabilities-with-azure-sql-db-16554)

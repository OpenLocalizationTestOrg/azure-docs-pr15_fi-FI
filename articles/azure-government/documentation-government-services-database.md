<properties
    pageTitle="Azure Government-ohjeet | Microsoft Azure"
    description="Tämä sisältää ominaisuuksia ja ohjeita vertailua Azure Government-sovellusten kehittäminen"
    services="Azure-Government"
    cloud="gov"
    documentationCenter=""
    authors="ryansoc"
    manager="zakramer"
    editor=""/>

<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="10/18/2016"
    ms.author="ryansoc"/>


#  <a name="azure-government-databases"></a>Azure Government-tietokannat

##  <a name="sql-database"></a>SQL-tietokantaan

Lisätietoja<a href="https://msdn.microsoft.com/en-us/library/bb510589.aspx"> Microsoftin Tietoturvakeskuksessa SQL-tietokantamoduulin</a> ja [Azure SQL tietokanta julkisen ohjeissa](https://azure.microsoft.com/documentation/services/sql-database/) metatietojen näkyvyyden määrittäminen ja suojauksen parhaita käytäntöjä muita ohjeita.

### <a name="variations"></a>Variaatiot

Vipuventtiili V12 SQL-tietokantaan ei yleensä käytettävissä Azure Government.

SQL Azure-palvelimet Azure Government-osoite on eri:

Palvelutyyppi|Azure julkisen|Azure Government
---|---|---
SQL-tietokantaan|*. database.windows.net|*. database.usgovcloudapi.net

### <a name="considerations"></a>Huomioon otettavia seikkoja

Seuraavassa on esitetty Azure Government rajaa Azure SQL:

| Sallittu säännelty ja hallita tiedot | Säännelty ja hallita tietojen ei sallita |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Kaikki tiedot tallennetaan ja käsitellään Microsoft Azure SQL-sisältää Azure Government säännelty tietoja. Sinun on käytettävä Tietokantatyökalut Azure Government säännelty tietojen siirtää tietoja. | Azure SQL-metatiedot ei ole sallittua sisältävät ohjaa vientitiedot. Metatiedot sisältää kaikki kokoonpanotietoja, kun luominen ja ylläpito tallennustilan tuotteen.  Älä kirjoita säännelty ja hallita tiedot seuraaviin kenttiin: tietokannan nimi, tilauksen nimi, resurssiryhmät, palvelimen nimi, palvelimen järjestelmänvalvojan kirjautuminen, käyttöönoton nimet, resurssien nimet, resurssi-tunnisteet

## <a name="azure-redis-cache"></a>Azure Redis.txt välimuisti

Lisätietoja tämän palvelun ja miten sitä käytetään on artikkelissa [Azure Redis välimuistin julkisen ohjeissa](https://azure.microsoft.com/documentation/services/redis-cache/).

### <a name="variations"></a>Variaatiot

URL-osoitteet käyttämisen ja Azure Government Azure Redis välimuistin hallinnan erot:

Palvelutyyppi|Azure julkisen|Azure Government
---|---|---
Välimuistin päätepiste|*. redis.cache.windows.net|*. redis.cache.usgovcloudapi.net
Azure portal|https://Portal.Azure.com|https://Portal.Azure.us

>[AZURE.NOTE] Kaikki komentosarjojen ja koodi on tarvittavat päätepisteet ja ympäristöissä. Lisätietoja on artikkelissa [Azure Government Cloud yhdistämisestä](../redis-cache/cache-howto-manage-redis-cache-powershell.md#how-to-connect-to-azure-government-cloud-or-azure-china-cloud).


### <a name="considerations"></a>Huomioon otettavia seikkoja

Seuraavat tiedot on esitetty Azure Government rajan Azure Redis välimuistin:

| Sallittu säännelty ja hallita tiedot | Säännelty ja hallita tietojen ei sallita |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Kaikki tiedot tallennetaan ja käsitellään Azure Redis välimuistin sisältää Azure Government säännelty tietoja. | Azure Redis välimuistin metatietojen ei ole sallittua sisältävät ohjaa vientitiedot. Älä kirjoita säännelty ja hallita tiedot seuraaviin kenttiin: välimuistiin, VNET nimen, tilauksen nimi, resurssiryhmiä, resurssi tunnisteita, Redis.txt ominaisuudet.  

##  <a name="next-steps"></a>Seuraavat vaiheet

Varten lisätiedot ja päivitykset tilaaminen <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure Government-blogi.</a>

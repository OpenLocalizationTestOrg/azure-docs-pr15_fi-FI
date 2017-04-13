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
    ms.date="09/30/2016"
    ms.author="ryansoc"/>


#  <a name="azure-government-data-and-storage"></a>Azure Government tiedot ja tallennustilaa

##  <a name="azure-storage"></a>Azure-tallennustilan

Lisätietoja palvelua ja miten sitä käytetään on artikkelissa [Azure-tallennustilan julkisen ohjeissa](https://azure.microsoft.com/documentation/services/storage/).

### <a name="variations"></a>Variaatiot

URL-osoitteet Azure Government-tallennustilan tilin erot:

Palvelutyyppi|Azure julkisen|Azure Government
---|---|---
Blob-objektien tallennustilaan|*. blob.core.windows.net|*. blob.core.usgovcloudapi.net
Jonon tallennustila|*. queue.core.windows.net|*. queue.core.usgovcloudapi.net
Taulukkotallennus|*. table.core.windows.net| *. table.core.usgovcloudapi.net

>[AZURE.NOTE] Kaikki komentosarjojen ja koodi on tarvittavat päätepisteet laskemisesta.  Kohdassa [Määritä Azure tallennustilan yhteyden merkkijonoja](../storage-configure-connection-string.md#creating-a-connection-string-to-the-explicit-storage-endpoint). 

Katso lisätietoja-ohjelmointirajapinnan <a href="https://msdn.microsoft.com/en-us/library/azure/mt616540.aspx">Cloud tallennustilan tilin konstruktoria</a>.

Voit käyttää näitä Osastollasi päätepisteen-liite on core.usgovcloudapi.net 

### <a name="considerations"></a>Huomioon otettavia seikkoja

Seuraavassa on esitetty Azure Government rajan Azuren tallennustilaan:

| Sallittu säännelty ja hallita tiedot | Säännelty ja hallita tietojen ei sallita |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Kirjoitetut, tiedot tallennetaan ja käsitellään Azure-tallennustilan tuotteen sisältää ohjaa Vie tietoja. Staattinen todentajia, joiden, esimerkiksi salasanat ja älykortin PIN Azure käyttöympäristön osien käytön. Yksityiset avaimet varmenteiden Azure käyttöympäristön osien hallinta. Muut suojaus tiedot ja tietoja, kuten varmenteet, salausavaimet, perustyyli näppäimet ja tallennustilan näppäimet tallennettu Azure Services-palveluissa. | Azure tallennustilan metatietojen ei ole sallittua sisältävät ohjaa vientitiedot. Metatiedot sisältää kaikki kokoonpanotietoja, kun luominen ja ylläpito tallennustilan tuotteen.  Älä kirjoita Regulated ja hallita tiedot seuraaviin kenttiin: resurssiryhmiä, käyttöönoton nimiä, resurssien nimet, resurssin tunnisteet  

##  <a name="premium-storage"></a>Premium-tallennustilan

Lisätietoja tämän palvelun ja sen käyttämisestä on artikkelissa [Premium tallennustilan: tehokas tallennustila Azure virtuaalikoneen työmääriä](../storage/storage-premium-storage.md).

###  <a name="variations"></a>Variaatiot

Premium-tallennustila on yleisesti saatavilla USGov Virginia. Tämä sisältää DS-sarjan näennäiskoneiden. 

### <a name="considerations"></a>Huomioon otettavia seikkoja

Tallennustilan tietojen kuvaukset saman versio yllä sopia premium-tallennustilan tilin. 

##  <a name="sql-database"></a>SQL-tietokantaan

Lisätietoja<a href="https://msdn.microsoft.com/en-us/library/bb510589.aspx"> Microsoftin Tietoturvakeskuksessa SQL-tietokantamoduulin</a> ja [Azure SQL tietokanta julkisen ohjeissa](https://azure.microsoft.com/documentation/services/sql-database/) metatietojen näkyvyyden määrittäminen ja suojauksen parhaita käytäntöjä muita ohjeita.

### <a name="variations"></a>Variaatiot

Vipuventtiili V12 SQL-tietokantaan ei yleensä käytettävissä Azure Government.

SQL Azure-palvelimet Azure Government-osoite on eri:

Palvelutyyppi|Azure julkisen|Azure Government
---|---|---
SQL-tietokantaan|*. database.windows.net|*. database.usgovcloudapi.net

### <a name="considerations"></a>Huomioon otettavia seikkoja

Seuraavassa on esitetty Azure Government rajan Azuren tallennustilaan:

| Sallittu säännelty ja hallita tiedot | Säännelty ja hallita tietojen ei sallita |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Kaikki tiedot tallennetaan ja käsitellään Microsoft Azure SQL-sisältää Azure Government säännelty tietoja. Sinun on käytettävä Tietokantatyökalut Azure Government säännelty tietojen siirtää tietoja. | Azure SQL-metatiedot ei ole sallittua sisältävät ohjaa vientitiedot. Metatiedot sisältää kaikki kokoonpanotietoja, kun luominen ja ylläpito tallennustilan tuotteen.  Älä kirjoita säännelty ja hallita tiedot seuraaviin kenttiin: tietokannan nimi, tilauksen nimi, resurssiryhmät, palvelimen nimi, palvelimen järjestelmänvalvojan kirjautuminen, käyttöönoton nimet, resurssien nimet, resurssi-tunnisteet

##  <a name="next-steps"></a>Seuraavat vaiheet

Varten lisätiedot ja päivitykset tilaaminen <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure Government-blogi.</a>

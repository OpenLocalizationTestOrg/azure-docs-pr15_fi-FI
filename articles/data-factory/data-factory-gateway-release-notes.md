<properties 
    pageTitle="Data Management Gateway julkaisutiedot | Azure Data Factory" 
    description="Data Management Gateway tory julkaisutiedot" 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/26/2016" 
    ms.author="spelluru"/>

# <a name="release-notes-for-data-management-gateway"></a>Data Management Gateway julkaisutiedot
Yksi haasteita nykyaikaisen tietojen integroinnin on siirtää tietoja ja sieltä pois paikalliseen pilveen. Tietoja Factory tekee integrointi saumaton Data Management Gatewaylle, joka on, että olet asentanut paikalliseen käyttöön hybrid tietojen siirto agentti kanssa.

Katso lisätietoja Data Management Gateway ja sen käyttämisestä on seuraavissa artikkeleissa: 

- [Data Management Gateway](data-factory-data-management-gateway.md)
- [Tietojen siirtäminen paikalliseen ja cloud Azure Data Factory käyttäminen](data-factory-move-data-between-onprem-and-cloud.md) 

## <a name="current-version-2260721"></a>Nykyinen versio (2.2.6072.1)

- Tukee Gatewayn määritysten hallintaa käyttämällä yhdyskäytävän HTTP-välityspalvelin asettaminen. Jos määritetty, Azure-Blob, Azure-taulukosta, Azure tietojen järvi ja asiakirjan DB käytetään HTTP-välityspalvelin kautta.
- Tukee otsikon käsittelyn TekstinMuoto tiedot / Azure-Blob-Azure Lake Tietosäilölle, kopioitaessa paikallisen tiedostojärjestelmän ja paikallisen HDFS.
- Tukee tietojen kopioiminen liittäminen Blob-objektien ja sivun Blob jo tuetut estä Blob-objektien kanssa.
- Lisää uusi yhdyskäytävän tila **Online (rajoitettu)**, joka ilmaisee, että yhdyskäytävän tärkeimmät toiminnallisuus toimii Ohjattu kopiointi interaktiivinen toiminto tuki lukuun ottamatta.
- Parantaa vakauden yhdyskäytävän rekisteröinnin rekisteröinti avaimen avulla.

## <a name="earlier-versions"></a>Aiemmissa versioissa

## <a name="2160401"></a>2.1.6040.1

- DB2-ohjain sisältyy yhdyskäytävän asennuspaketti nyt. Sinun ei tarvitse asentaa erikseen. 
- DB2-ohjain tukee nyt z/OS ja DB2 varten voin (AS / 400) sekä ympäristöjen tukee jo (Linux, Unix ja Windows). 
- Tukee käyttämällä DocumentDB lähteen tai kohteen paikallisen tietojen säilöt
- Tietojen kopioiminen- / flunssaan/kuuman tukee blob storage jo tuetut yleinen tallennustilan-tilin kanssa. 
- Voit muodostaa paikallisen SQL Server remote kirjautuminen oikeuksilla yhdyskäytävän kautta.  

## <a name="2060131"></a>2.0.6013.1

- Voit valita kielen/culture käytettävän yhdyskäytävän manuaalisen asennuksen aikana.
- Kun yhdyskäytävä ei toimi odotetulla tavalla, voit lähettää Microsoftille helpottaa ongelman vianmäärityksen yhdyskäytävän lokit viimeisten seitsemän päivän aikana. Yhdyskäytävä ole yhteyttä pilvipalveluun, voit tallentaa ja arkistoida yhdyskäytävän lokitiedot.  
- Käyttöliittymän parannukset Gatewayn määritysten hallintaa varten:
    - Tee yhdyskäytävän tila Lisää näkyvä Aloitus-välilehden.
    - Järjestetyt ja yksinkertaistettu ohjausobjektit.
- Voit kopioida tietoja säilöstä, [vapauttaa koodin kopioi esikatselu-työkalun](data-factory-copy-data-wizard-tutorial.md)avulla. Lisätietoja [Vaiheistettu kopioi](data-factory-copy-activity-performance.md#staged-copy) tietoja tästä ominaisuudesta yleinen. 
- Voit Data Management Gatewayta tunkeutumisen tiedot suoraan paikallisen SQL Server-tietokantaan Azure koneen Learning.
- Suorituskykyä on parannettu
    - Paranna tarkastelemisesta koodin vapaa kopioi esikatselu-työkalun rakenteen/esikatselu vastaan SQL Server.



## <a name="11259531"></a>1.12.5953.1
- Virheenkorjauksia

## <a name="11159181"></a>1.11.5918.1

- Yhdyskäytävän tapahtumalokista enimmäiskoko on suurennettu 1 Mt 40 megatavua.
- Varoitusvalintaikkuna tulee näkyviin siltä varalta, että uudelleenkäynnistys tarvita yhdyskäytävän automaattisen päivityksen aikana. Voit aloittaa oikealle sitten tai myöhemmin. 
- Siltä varalta, että automaattinen päivitys epäonnistuu, yhdyskäytävän installer uudelleenyrityksiä automaattinen päivitys kolme kertaa enimmillään.
- Suorituskykyä on parannettu
    - Tehostaa suurten taulukoiden lataaminen paikalliseen Serveriin koodin vapaa kopioi skenaariossa.
- Virheenkorjauksia

## <a name="11058921"></a>1.10.5892.1

- Suorituskykyä on parannettu
- Virheenkorjauksia

## <a name="1958652"></a>1.9.5865.2

- Nolla touch automaattinen päivitys-ominaisuus
- Uusi yhdyskäytävä-Tilailmaisimet ja kuvake
- Mahdollisuus "Päivitys nyt" asiakasohjelmassa
- Mahdollisuus määrittää päivityksen ajoitus-aika
- PowerShell-komentosarjaa varten VAIHTO automaattinen päivitys käytössä/poissa käytöstä
- JSON-tiedostomuodon tuki  
- Suorituskykyä on parannettu
- Virheenkorjauksia

## <a name="1858221"></a>1.8.5822.1

- Vianmäärityksen parempana
- Suorituskykyä on parannettu
- Virheenkorjauksia

### <a name="1757951"></a>1.7.5795.1

- Suorituskykyä on parannettu
- Virheenkorjauksia

### <a name="1757641"></a>1.7.5764.1

- Suorituskykyä on parannettu
- Virheenkorjauksia

### <a name="1657351"></a>1.6.5735.1

- Tue paikallisen HDFS lähde/käsittelytoiminto
- Suorituskykyä on parannettu
- Virheenkorjauksia

### <a name="1656961"></a>1.6.5696.1

- Suorituskykyä on parannettu
- Virheenkorjauksia

### <a name="1656761"></a>1.6.5676.1

- Tuen diagnostiikkatyökalut-määritysten hallinta
- Taulukkomuotoinen tietolähteiden Azure Data Factory tuki taulukon sarakkeet
- Azure Data Factory SQL DW tuki
- Azure Data Factory Reclusive BlobSource ja FileSource tuki
- CopyBehavior – MergeFiles, PreserveHierarchy ja FlattenHierarchy BlobSink ja Azure Data Factory binaarinen kopiolla FileSink tuki
- Kopioi tehtävän edistymisestä, Azure Data Factory tuki
- Azure Data Factory tuki tietojen lähteen yhteyden varmistaminen
- Virheenkorjauksia


### <a name="1656721"></a>1.6.5672.1

- ODBC-tietolähde Azure Data Factory tuki taulukkonimi
- Suorituskykyä on parannettu
- Virheenkorjauksia

### <a name="1656581"></a>1.6.5658.1

- Tuen tiedoston allas Azure Data Factory varten
- Tue binaariluvun kopioiminen säilyttäminen hierarkian Azure Data Factory
- Kopioi tehtävän ainutkertaisuus tuki Azure Data Factory
- Virheenkorjauksia

### <a name="1656401"></a>1.6.5640.1

- 3 Lisää tietolähteitä tuki Azure Data Factory (ODBC, OData, HDFS)
- Tuen lainausmerkki csv-jäsentimen Azure Data Factory
- Tiivistys-tuki (BZip2)
- Virheenkorjauksia

### <a name="1556121"></a>1.5.5612.1

- Viisi relaatiotietokannat tuki Azure Data Factory (MySQL, PostgreSQL, DB2, Teradata ja Sybase)
- Tiivistys-tuki (Gzip ja Deflate)
- Suorituskykyä on parannettu
- Virheenkorjauksia


### <a name="1455491"></a>1.4.5549.1

- Lisää Oracle tietojen lähteen Azure Data Factory tuki
- Suorituskykyä on parannettu
- Virheenkorjauksia

### <a name="1454921"></a>1.4.5492.1

- Yhdistetty binaarinen, joka tukee Microsoft Azure Data Factory-ja Office 365 Power BI
- Hankittujen määritysten Käyttöliittymän ja rekisteröinnin jälkeen
- Azure Data Factory – Azure tunkeutumisen ja juniin tukevat SQL Server-tietolähteestä

### <a name="1253031"></a>1.2.5303.1

-   Korjaa aikakatkaisu ongelman tukemaan enemmän aikaa tietolähdeyhteydet. 
    
### <a name="1155268"></a>1.1.5526.8

- Edellyttää .NET Framework 4.5.1 edellytyksenä asennuksen aikana.

### <a name="1051442"></a>1.0.5144.2

- Ei muutoksia, jotka vaikuttavat Azure Data Factory skenaarioita. 

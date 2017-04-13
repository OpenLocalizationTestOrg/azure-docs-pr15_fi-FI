## <a name="fileshare-dataset-type-properties"></a>Tiedostossa tietojoukko ominaisuudet

Täydellinen luettelo osien ja ominaisuudet, jotka ovat käytettävissä määrittäminen tietojoukkoja [luominen tietojoukkoja](../articles/data-factory/data-factory-create-datasets.md) on artikkelissa. Tietojoukon tyypeissä muistuttavat osia, kuten rakenne, käytettävyys ja tietojoukko JSON käytännön. 

**TypeProperties** osa on eri mistäkin tietojoukko. Sen tietoja, jotka liittyvät tietojoukko tyyppi. Tyypin **tiedostossa** tietojoukko tietojoukko typeProperties osiossa on seuraavat ominaisuudet:

Ominaisuus | Kuvaus | Pakollinen
-------- | ----------- | --------
Kansiopolku | Sub-kansion polku. Käytä ohjausmerkkiä "\" erikoismerkkien merkkijonon. Katso esimerkkejä [otoksen linkitetty palvelu ja tietojoukko määritykset](#sample-linked-service-and-dataset-definitions) .<br/><br/>Voit yhdistää tämän ominaisuuden **partitionBy** on kansion polkujen sektoria perusteella ja Aloita/Lopeta päivämäärä kertaa. | Kyllä
Tiedostonimi | Määritä tiedoston nimi **kansiopolku** , jos haluat tietyn tiedoston kansioon viitata taulukon. Jos et määritä tätä ominaisuutta, jos arvo osoittaa taulukon kaikki tiedostot-kansiossa.<br/><br/>Kun tulostus-tietojoukko tiedostonimi ei ole määritetty, luodun tiedoston nimi on oltava seuraavat tässä muodossa: <br/><br/>Tietoja. <Guid>.txt (Esimerkki: Data.0a405f8a 93ff-4c6f-b3be-f69616f1df7a.txt | Ei
partitionedBy | partitionedBy avulla voidaan määrittää dynaamisen kansiopolku sarjan aikatietoja tiedostonimi. Esimerkiksi kansiopolku Parametroitu tunnissa tietoja varten. | Ei
Muotoile | Muotoile seuraavanlaisia tuetaan: **TekstinMuoto**, **AvroFormat**, **JsonFormat**ja **OrcFormat**. Muotoile **tyyppi** -ominaisuuden arvoksi jokin näistä arvoista. Lue lisätietoja [Määrittäminen TekstinMuoto](#specifying-textformat), [Määrittämällä AvroFormat](#specifying-avroformat), [Määrittämällä JsonFormat](#specifying-jsonformat)ja [Määrittämällä OrcFormat](#specifying-orcformat) osat. Jos haluat kopioida tiedostot muodossa-on välillä tiedostopohjaisia stores (binaarinen kopio) voit ohittaa sekä syöttö- ja tietojoukko määritelmät muoto-kohdassa. | Ei
fileFilter | Määritä suodatus, jota käytetään osajoukkoa kansiopolku tiedostojen kaikki tiedostot sijaan.<br/><br/>Sallitut arvot ovat: `*` (useita merkkejä) ja `?` (yksittäinen merkki).<br/><br/>Esimerkki 1:`"fileFilter": "*.log"`<br/>Esimerkki 2:`"fileFilter": 2014-1-?.txt"`<br/><br/> fileFilter ei käytettävissä syötteen tiedostossa-tietojoukko. Tätä ominaisuutta ei tueta HDFS.  | Ei
| pakkaus | Määritä tyyppi ja tietojen pakkaus taso. Tuetut tiedostotyypit ovat: **GZip**, **Deflate**ja **BZip2** ja tuettujen tasot ovat: **Optimal** ja **nopein**. Tällä hetkellä pakkausasetuksia ei tue tietojen **AvroFormat** tai **OrcFormat**. Jos haluat lisätietoja, katso [pakkaamisen tuki](#compression-support) -osa.  | Ei |
| useBinaryTransfer | Määrittää, käyttävätkö binaarinen siirtotila. TOSI, jos binaariluvun tila ja EPÄTOSI ASCII. Oletusarvo: TOSI. Tätä ominaisuutta voi käyttää vain, kun tyyppi on liitetty linkitetyn palvelutyyppi: FtpServer. | Ei | 
 

> [AZURE.NOTE] tiedoston nimen ja fileFilter ei voi käyttää samanaikaisesti.

### <a name="using-partionedby-property"></a>PartionedBy-ominaisuuden avulla

Mainittu edellisessä osassa voit määrittää dynaamisen kansiopolku sarjan aikatietoja partitionedBy ja tiedostonimi. Voit tehdä Data Factory-makrot ja järjestelmämuuttujan SliceStart, SliceEnd, jotka osoittavat looginen ajan, on annettu data-sektori. 

Lisätietoja aika sarjan tietojoukkoja, järjestäminen ja sektorit on artikkeleissa [Luominen tietojoukkoja](../articles/data-factory/data-factory-create-datasets.md), [ajoittaminen ja toteutus](../articles/data-factory/data-factory-scheduling-and-execution.md)ja [Luominen putkistot](../articles/data-factory/data-factory-create-pipelines.md) artikkelit. 

#### <a name="sample-1"></a>Esimerkki 1:

    "folderPath": "wikidatagateway/wikisampledataout/{Slice}",
    "partitionedBy": 
    [
        { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
    ],

Tässä esimerkissä {sektoria} tilalla on määritetty Data Factory järjestelmän muuttujaa SliceStart (YYYYMMDDHH)-muodossa arvo. SliceStart viittaa sektoria aloitusaika. Kansiopolku on erilainen jokaiselle sektoria. Esimerkki: wikidatagateway/wikisampledataout/2014100103 tai wikidatagateway/wikisampledataout/2014100104.

#### <a name="sample-2"></a>Esimerkki 2:

    "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
    "fileName": "{Hour}.csv",
    "partitionedBy": 
     [
        { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
    ],

Tässä esimerkissä vuoden, kuukauden, päivän ja ajan SliceStart puretaan tuominen eri muuttujat, joita käytetään kansiopolku ja tiedostonimi-ominaisuudet.

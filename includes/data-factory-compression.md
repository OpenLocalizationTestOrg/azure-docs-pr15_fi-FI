### <a name="compression-support"></a>Pakkaamisen tuki  
Suurten tietojoukkojen käsittelyn voi aiheuttaa i/o ja verkon pullonkaulojen. Tämän vuoksi stores pakattujen tietojen Voit paitsi nopeuttaa tietojen siirto verkossa ja säästää levytilaa, mutta tuo myös merkittäviä parannuksia processing big datasta. Pakkaus on tällä hetkellä tueta tiedostopohjaisia tietojen stores kuten Azure-Blob- tai paikallisen tiedostojärjestelmässä.  

> [AZURE.NOTE] Tietoja **AvroFormat**, **OrcFormat**tai **ParquetFormat**ei tue pakkaamisen asetukset. 

Voit määrittää tietojoukko pakkaus käyttämällä **pakkaus** -ominaisuuden tietojoukossa JSON seuraavan esimerkin mukaisesti:   

    {  
        "name": "AzureBlobDataSet",  
        "properties": {  
            "availability": {  
                "frequency": "Day",  
                "interval": 1  
            },  
            "type": "AzureBlob",  
            "linkedServiceName": "StorageLinkedService",  
            "typeProperties": {  
                "fileName": "pagecounts.csv.gz",  
                "folderPath": "compression/file/",  
                "compression": {  
                    "type": "GZip",  
                    "level": "Optimal"  
                }  
            }  
        }  
    }  
 
**Tiivistys** -osassa on kaksi ominaisuudet:  
  
- **Tyyppi:** pakkauksenhallintaa, joka voi olla **GZIP**, **Deflate** tai **BZIP2**.  
- **Taso:** pakkaamisen suhde, joka voi olla **Optimal** tai **nopein**. 
    - **Nopein:** Pakkaaminen toimintoa täyttävän mahdollisimman nopeasti, vaikka tiedosto ei pakata optimaalisesti. 
    - **Optimal**: pakkaaminen toimintoa kannattaa optimaalisesti pakata, vaikka toimintoa kestää kauemmin suorittamiseen. 
    
    Lisätietoja on [Pakkauksen](https://msdn.microsoft.com/library/system.io.compression.compressionlevel.aspx) aiheessa. 

Oletetaan, että edellä otoksen tietojoukko käytetään kopioi tehtävän tulosteen, kopioi tehtävän pakkaa tiedot GZIP-pakkauksenhallinnan avulla paras suhde kanssa ja kirjoita sitten pakattujen tietojen pagecounts.csv.gz Azure-Blob-objektien tallennustilaan-tiedostoon.   

Kun määrität pakkaus-ominaisuuden syötteen tietojoukko JSON, putkijohto lukea pakattujen tietojen lähteestä ja määrität tulostus-tietojoukko JSON-ominaisuus kopioi tehtävän kirjoittaa pakattujen tietojen kohteeseen. Tässä on muutama esimerkkiskenaarioita: 

- Azure-blob luku GZIP pakattujen tietojen purkaa sen ja kirjoittaa tiedot Azure SQL-tietokantaan. Voit määrittää syötteen Azure-Blob-objektien tietojoukkoa, jonka pakkaamisen JSON-ominaisuuden tällöin. 
- Lukea tietoja vain teksti-tiedostosta paikallisen tiedoston järjestelmästä pakata GZip-muodossa ja kirjoittaa pakattujen tietojen Azure-blob. Voit määrittää tulosteen Azuren Blob-objektien tietojoukkoa, jonka pakkaamisen JSON-ominaisuuden tällöin.  
- GZIP pakattujen tietojen lukeminen Azure-blob, sitä avata, käyttämällä BZIP2 pakkaa ja kirjoittaa tiedot Azure-blob. Voit määrittää syötteen Azure-Blob-objektien tietojoukkoa määritetty GZIP ja tulostus-tietojoukko pakkaamisen sisältötyyppi asettaminen BZIP2 tällöin pakkaamisen sisältötyyppi.   

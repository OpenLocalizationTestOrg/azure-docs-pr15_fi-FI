<properties 
    pageTitle="Siirrä tiedot palvelimen | Microsoft Azure" 
    description="Tietoja voit siirtää tietoja käyttämällä Azure Data Factory FTP-palvelimeen." 
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
    ms.date="10/20/2016" 
    ms.author="spelluru"/>

# <a name="move-data-from-an-ftp-server-using-azure-data-factory"></a>Siirrä tiedot käyttämällä Azure Data Factory FTP-palvelin
Tässä artikkelissa käsitellään kopio-toiminnon käyttämisestä Azure Data Factory Jos haluat siirtää tietoja tuetuista käsittelytoiminto tietosäilö FTP-palvelimesta. Tässä artikkelissa perustuu [tietojen siirtämistä toimintoja](data-factory-data-movement-activities.md) artikkeliin, jossa näkyy yleiskatsaus tietojen siirto kopioi tehtävän ja tietojen stores tuettuja lähteistä/poistumia luettelo. 

Tietoja factory tukee tällä hetkellä vain siirtäminen tiedot FTP-palvelimesta ja muiden tietojen stores, mutta ei muita tietoja stores tietojen siirtämisestä FTP-palvelimeen. Se tukee sekä paikallisen ja cloud FTP-palvelimiin. 

Jos tiedot ovat siirtäminen **paikalliseen** FTP-palvelimesta cloud tietosäilö (Esimerkki: Azure-Blob-säiliö), asentaa ja käyttää Data Management Gateway. Data Management Gateway on asiakas-agentti, johon on asennettu paikalliseen tietokoneeseen, joka sallii pilvipalveluihin Muodosta yhteys paikalliseen resurssi. [Data Management Gateway](data-factory-data-management-gateway.md) on yhdyskäytävän. Artikkelissa [paikallisen sijainnit ja cloud siirtäminen tietojen](data-factory-move-data-between-onprem-and-cloud.md) vaiheittaiset ohjeet yhdyskäytävän määrityksestä ja käyttää sitä. Yhdyskäytävän avulla voit muodostaa yhteyden FTP-palvelimeen, vaikka palvelin on Azure IaaS virtuaalikoneen (AM). 

Voit asentaa yhdyskäytävän samaan paikalliseen tietokoneeseen tai Azure IaaS AM kuin FTP-palvelimeen. Suosittelemme kuitenkin, että asennat yhdyskäytävän erillisessä koneen tai erilliset Azure IaaS AM välttää resurssiristiriita ja suorituskyvyn parantamiseksi. Kun asennat yhdyskäytävän eri tietokoneeseen, tietokoneen pitäisi FTP-palvelinta. 

## <a name="copy-data-wizard"></a>Ohjattu tietojen kopioiminen
Helpoin tapa luoda myyntijakso, kopioi tiedot FTP-palvelimesta on käyttämällä ohjattua Kopioi tiedot. Katso [Opetusohjelma: Luo kopio ohjatulla putkijohto](data-factory-copy-data-wizard-tutorial.md) nopeasti käyttämällä ohjattua tietojen kopioiminen putkijohto luomisesta saat. 

Seuraavissa esimerkeissä otoksen JSON määritykset, jonka avulla voit luoda putkijohto [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) tai [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) tai [PowerShellin Azure](data-factory-copy-activity-tutorial-using-powershell.md). 

## <a name="sample-copy-data-from-ftp-server-to-azure-blob"></a>Esimerkki: Kopioi tiedot Azure-blob FTP-palvelin

Tässä esimerkissä esitetään, kuinka haluat kopioida tiedot Azure-Blob-objektien tallennustilaan FTP-palvelimesta. Tietoja voi kuitenkin kopioidun **suoraan** johonkin seuraavista poistumia määritetyllä tavalla [tähän](data-factory-data-movement-activities.md#supported-data-stores) kopio-toimintojen käyttäminen Azure Data Factory.  
 
Malli on tietoja factory-kohteet:

- Linkitetyn palvelu [FtpServer](#ftp-linked-service-properties)tyyppi.
- Linkitetyn palvelu [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties)tyyppi.
- Syötteen [tietojoukko](data-factory-create-datasets.md) tyypin [tiedostossa](#fileshare-dataset-type-properties).
- Tulosteen [tietojoukko](data-factory-create-datasets.md) tyypin [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
- [Myyntijakso](data-factory-create-pipelines.md) kopioi aktiviteettiin, joka käyttää [FileSystemSource](#ftp-copy-activity-type-properties) ja [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Otosten kopioi tiedot FTP-palvelimesta Azure-blob tunnissa. Käyttää näitä esimerkkejä JSON-ominaisuuksia on kuvattu osia seuraavat mallit. 

**FTP linkitetty palvelu** Tässä esimerkissä käytetään käyttäjänimi ja salasana, vain teksti perustodentamista. Voit myös käyttää jollakin seuraavista tavoista: 

- Anonyymi todentaminen 
- Perustodentamista tunnistetiedot on salattu
- FTP-päälle SSL/TLS (FTPS)

Todennus, voit käyttää erilaista Katso [FTP linkitetty palvelun](#ftp-linked-service-properties) osa. 

    {
        "name": "FTPLinkedService",
        "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",          
            "authenticationType": "Basic",
            "username": "Admin",
            "password": "123456"
        }
      }
    }

**Azure linkitetty tallennuspalvelu**

    {
      "name": "AzureStorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**Syötteen FTP-tietojoukko** Tämä tietojoukko viittaa FTP-kansion `mysharedfolder` ja `test.csv`. Putkijohto kopioi tiedoston kohteeseen. 

Asetus "ulkoinen": "true" ilmoittaa Data Factory-palvelun dataset on ulkoiset tiedot-factory ja tietojen factory toimintaa ei tuottamat.
    
    {
      "name": "FTPFileInput",
      "properties": {
        "type": "FileShare",
        "linkedServiceName": "FTPLinkedService",
        "typeProperties": {
          "folderPath": "mysharedfolder",
          "fileName": "test.csv",
          "useBinaryTransfer": true
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }


**Azure-Blob tulosteen tietojoukko**

Tietoja kirjoitetaan uusia Blob-objektien tunnissa (taajuus: tunti, väli: 1). Kansiopolun blob arvioidaan dynaamisesti sektoria, käsitellään alkamispäivää perusteella. Kansiopolku käytetään vuoden, kuukauden, päivän ja alkamisajan tuntia osat.

    {
        "name": "AzureBlobOutput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/ftp/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
                "format": {
                    "type": "TextFormat",
                    "rowDelimiter": "\n",
                    "columnDelimiter": "\t"
                },
                "partitionedBy": [
                    {
                        "name": "Year",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "yyyy"
                        }
                    },
                    {
                        "name": "Month",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "MM"
                        }
                    },
                    {
                        "name": "Day",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "dd"
                        }
                    },
                    {
                        "name": "Hour",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "HH"
                        }
                    }
                ]
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }



**Kopioi aktiviteettiin putkijohto**

Putkisto sisältää kopio-toiminto, joka on määritetty käyttämään syöttö- ja tietojoukkoja ja on suunniteltu toimimaan tunnissa. Myyntijakso JSON määritelmä- **lähteen** tyyppi on määritetty **FileSystemSource** ja **käsittelytoiminto** tyyppi-asetukseksi on määritetty **BlobSink**. 
    
    {
        "name": "pipeline",
        "properties": {
            "activities": [{
                "name": "FTPToBlobCopy",
                "inputs": [{
                    "name": "FtpFileInput"
                }],
                "outputs": [{
                    "name": "AzureBlobOutput"
                }],
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "FileSystemSource"
                    },
                    "sink": {
                        "type": "BlobSink"
                    }
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "00:05:00"
                }
            }],
            "start": "2016-08-24T18:00:00Z",
            "end": "2016-08-24T19:00:00Z"
        }
    }

## <a name="ftp-linked-service-properties"></a>FTP linkitetyistä ominaisuudet

Seuraavassa taulukossa on kuvaus JSON elementtien tietyn FTP-linkitetyn palvelu.

| Ominaisuus | Kuvaus | Pakollinen | Oletusarvo |
| -------- | ----------- | -------- | ------- | 
| tyyppi | Type-ominaisuus on määritettävä FtpServer | Kyllä | &nbsp;
| Host (isäntä) | FTP-palvelimen nimi tai IP-osoite | Kyllä | &nbsp;
| authenticationType | Määritä todennustyyppi | Kyllä | Basic-anonyymi |
| käyttäjänimi | Käyttäjä, jolla on pääsy FTP-palvelin | Ei | &nbsp;
| salasana | Salasanan (käyttäjänimi) | Ei | &nbsp;
| encryptedCredential | Salatun tunnistetiedon FTP-palvelimen käyttäminen | Ei | &nbsp;
| gatewayName | Data Management Gateway-yhdyskäytävän Muodosta yhteys paikalliseen FTP-palvelimen nimeä | Ei    | &nbsp;
| Port (portti) | Seuraa, FTP-palvelimen portti | Ei | 21 |
| enableSsl | Määrittää, haluatko käyttää FTP SSL/TLS-kanavan kautta | Ei | TOSI | 
| enableServerCertificateValidation | Määritä, otetaanko palvelimen SSL-varmenteen vahvistamista käytettäessä FTP SSL/TLS-kanavan kautta | Ei | TOSI | 

### <a name="using-anonymous-authentication"></a>Käyttämällä Anonyymi todentaminen

    {
        "name": "FTPLinkedService",
        "properties": {
            "type": "FtpServer",
            "typeProperties": {     
                "authenticationType": "Anonymous",
                "host": "myftpserver.com"
            }
        }
    }

### <a name="using-username-and-password-in-plain-text-for-basic-authentication"></a>Käyttämällä käyttäjänimen ja salasanan perustodentamista tekstimuodossa
    
    {
        "name": "FTPLinkedService",
        "properties": {
        "type": "FtpServer",
            "typeProperties": {
                "host": "myftpserver.com",
                "authenticationType": "Basic",
                "username": "Admin",
                "password": "123456"
            }
        }
    }


### <a name="using-port-enablessl-enableservercertificatevalidation"></a>Portti, enableSsl, enableServerCertificateValidation käyttäminen

    {
        "name": "FTPLinkedService",
        "properties": {
            "type": "FtpServer",
            "typeProperties": {
                "host": "myftpserver.com",
                "authenticationType": "Basic",  
                "username": "Admin",
                "password": "123456",
                "port": "21",
                "enableSsl": true,
                "enableServerCertificateValidation": true
            }
        }
    }

### <a name="using-encryptedcredential-for-authentication-and-gateway"></a>Todennus- ja yhdyskäytävän encryptedCredential käyttäminen

    {
        "name": "FTPLinkedService",
        "properties": {
            "type": "FtpServer",
            "typeProperties": {
                "host": "myftpserver.com",
                "authenticationType": "Basic",
                "encryptedCredential": "xxxxxxxxxxxxxxxxx",
                "gatewayName": "mygateway"
            }
        }
    }

Saat lisätietoja määrittäminen paikallista FTP-tietolähteen tunnistetiedot [määrittää käyttöoikeudet ja suojaus](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) .

[AZURE.INCLUDE [data-factory-file-share-dataset](../../includes/data-factory-file-share-dataset.md)]   
[AZURE.INCLUDE [data-factory-file-format](../../includes/data-factory-file-format.md)]   
[AZURE.INCLUDE [data-factory-compression](../../includes/data-factory-compression.md)]

## <a name="ftp-copy-activity-type-properties"></a>Kopioi FTP tehtävän ominaisuudet

Täydellinen luettelo osat ja ominaisuudet, jotka ovat käytettävissä toiminnan määrittäminen [Putkistot luomisesta](data-factory-create-pipelines.md) on artikkelissa. Ominaisuudet, kuten nimi, kuvaus sekä syöttö- ja taulukot ja käytännöt ovat käytettävissä kaikki tehtävälajit. 

Tehtävän typeProperties-osassa käytettävissä olevat ominaisuudet vaihtelevat toisaalta kunkin tehtävätyyppi. Kopioi tehtävälle tyypin ominaisuudet vaihtelevat sen mukaan, lähteiden ja poistumia.

[AZURE.INCLUDE [data-factory-file-system-source](../../includes/data-factory-file-system-source.md)]   

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>Suorituskyky ja säätäminen  
Lisätietoja on artikkelissa [Kopioi tehtävän suorituskyvyn & säätäminen opas](data-factory-copy-activity-performance.md) lisätietoja tärkeimmät seikat, vaikutus suorituskykyä tietojen siirto (kopioi tehtävä) Azure Data Factory ja voidaan optimoida sen usealla tavalla.

## <a name="next-steps"></a>Seuraavat vaiheet
Seuraavissa artikkeleissa: 

- [Kopioi tehtävän opetusohjelma](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) vaiheittaiset ohjeet luomiseen putkijohto kopio-tehtävän. 

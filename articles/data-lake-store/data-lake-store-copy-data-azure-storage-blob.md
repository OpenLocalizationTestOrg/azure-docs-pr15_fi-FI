<properties
   pageTitle="Kopioi tiedot Azure-tallennustilan BLOB järvi tietovaraston | Microsoft Azure"
   description="Tietojen kopioiminen Azure-tallennustilan BLOB järvi tietovaraston AdlCopy-työkalun avulla"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/05/2016"
   ms.author="nitinme"/>

# <a name="copy-data-from-azure-storage-blobs-to-data-lake-store"></a>Tietojen kopioiminen Azure-tallennustilan BLOB järvi tietosäilö

> [AZURE.SELECTOR]
- [DistCp käyttäminen](data-lake-store-copy-data-wasb-distcp.md)
- [AdlCopy käyttäminen](data-lake-store-copy-data-azure-storage-blob.md)

Azure järvi tietosäilö on komentorivi-työkalun [AdlCopy](http://aka.ms/downloadadlcopy), jos haluat kopioida tiedot seuraavista lähteistä:

* FROM Azure tallennustilan BLOB järvi säilöön. Et voi käyttää AdlCopy tietojen kopioiminen järvi tietovaraston Azuren tallennustilaan BLOB-objektit.

* Kaksi Azure järvi tietosäilö-tilien välillä 

Voit myös käyttää AdlCopy-työkalun kaksi eri tilat:

* **Erillisestä**, johon työkalu käyttää järvi tietovaraston resurssien tehtävän suorittamiseen.

* **Tietoja järvi Analytics-tilillä**, käytettyjen tietojen järvi Analytics tiliin määritetyn yksiköt kopio-toiminnon. Haluat ehkä käyttää tätä asetusta, kun näyttöä kopioi tehtävien suorittamista helpommin ennakoitavia tavalla.

##<a name="prerequisites"></a>Edellytykset

Ennen kuin aloitat tämän artikkelin, sinulla on oltava seuraavasti:

- **Azure-tilaus**. Katso [Hae Azure maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/).

- **Tallennustilan Azure-BLOB** -säilö tietoja.

- **Azure tietojen järvi Analytics-tili (valinnainen)** – Katso [aloittaminen Azure tietojen järvi Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md) ohjeet järvi tietosäilö-tilin luominen.

- **AdlCopy-työkalu**. Asenna työkalu AdlCopy [http://aka.ms/downloadadlcopy](http://aka.ms/downloadadlcopy).

## <a name="syntax-of-the-adlcopy-tool"></a>AdlCopy-työkalun syntaksi

Käytä seuraavaa syntaksia toimimaan AdlCopy-työkalu

    AdlCopy /Source <Blob or Data Lake Store source> /Dest <Data Lake Store destination> /SourceKey <Key for Blob account> /Account <Data Lake Analytics account> /Unit <Number of Analytics units> /Pattern 

Parametreja, sen syntaksi on kuvattu seuraavassa:

| Vaihtoehto    | Kuvaus                                                                                                                                                                                                                                                                                                                                                                                                          |
|-----------|------------|
| Lähde    | Määrittää sijainnin lähdetietojen tallennustilan Azure-blob. Lähteen voi olla blob-säilö, blob tai järvi tietovaraston toiseen tiliin.                                                                                                                                                                                                                                                                                                 |
| Kohde      | Määrittää järvi tietovaraston kohteen kopioiminen.                                                                                                                                                                                                                                                                                                                                                                |
| SourceKey | Määrittää lähteen tallennustilan Azure blob storage pikanäppäin. Tämä on pakollinen vain, jos lähde on blob-säilö tai blob.                                                                                                                                                                                                                                                                                                                                                 |
| Tilin   | **Valinnainen**. Käytä tätä, jos haluat kopioida suoritetaan Azure tietojen järvi Analytics-tilin avulla. Jos syntaksi /Account-vaihtoehdon avulla, mutta tietojen järvi Analytics-tiliä ei määritetä, AdlCopy käyttää oletustili suoritetaan. Myös, jos käytät tätä vaihtoehtoa, on lisättävä lähde (Azure-tallennustilan Blob)- ja (Azure Lake Tietosäilölle) kuin tietolähteet tietojen järvi Analytics-tilillesi.  |
| Yksiköt     |  Määrittää, jota käytetään kopion projektin tietoja järvi Analytics yksiköiden määrän. Tämä asetus on pakollinen, jos **/Account** -vaihtoehdon avulla voit määrittää tietojen järvi Analytics-tili.
| Kuvion   | Määrittää regex kaavan, joka osoittaa, mitä BLOB tai kopioitavat tiedostot. AdlCopy käyttää kirjainkoon vastaavat. Käytetään, kun ei ole kuvio on määritetty oletusarvoinen kuvio on kopioi kaikki kohteet. Usean tiedoston kuvioiden määrittäminen ei tueta.                                                                                                                                                                                                                                                                                                                                               



## <a name="use-adlcopy-as-standalone-to-copy-data-from-an-azure-storage-blob"></a>Tietojen kopioiminen tallennustilan Azure-blob (yksittäisenä) AdlCopy avulla

1. Avaa komentokehote ja siirry kansioon, johon AdlCopy on asennettu, yleensä `%HOMEPATH%\Documents\adlcopy`.

2. Suorita seuraava komento tietyn Blob-objektien kopioiminen järvi tietovaraston tietolähteen säilö:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>

    Esimerkki:

        AdlCopy /source https://mystorage.blob.core.windows.net/mycluster/HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b.log /dest swebhdfs://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

    
    >[AZURE.NOTE] Yllä olevaa syntaksia määrittää tiedoston järvi tietosäilö-tilin kansion kopioidaan. AdlCopy työkalu luo kansion, jos määritetty kansionimi ei ole.

    Voit pyydetään kirjoittamaan tunnistetiedot vallitessa käytössä järvi tietovaraston tilisi Azure-tilaus. Seuraavankaltaiselta tulos tulee näkyviin:

        Initializing Copy.
        Copy Started.
        100% data copied.
        Finishing Copy.
        Copy Completed. 1 file copied.

3. Voit myös kopioida kaikki Blob-objektien yhden säilöstä järvi tietovaraston tilillä seuraava komento:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/ /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>        

    Esimerkki:

        AdlCopy /Source https://mystorage.blob.core.windows.net/mycluster/example/data/gutenberg/ /dest adl://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

## <a name="use-adlcopy-as-standalone-to-copy-data-from-another-data-lake-store-account"></a>Tietojen kopioiminen toisen järvi tietovaraston tilin (yksittäisenä) AdlCopy avulla

Voit käyttää myös AdlCopy, jos haluat kopioida tiedot kaksi järvi tietovaraston tilien välillä.

1. Avaa komentokehote ja siirry kansioon, johon AdlCopy on asennettu, yleensä `%HOMEPATH%\Documents\adlcopy`.

2. Suorita seuraava komento voit kopioida tietyn tiedoston järvi tietosäilö-tililtä toiselle.

        AdlCopy /Source adl://<source_adls_account>.azuredatalakestore.net/<path_to_file> /dest adl://<dest_adls_account>.azuredatalakestore.net/<path>/

    Esimerkki:

        AdlCopy /Source adl://mydatastore.azuredatalakestore.net/mynewfolder/909f2b.log /dest adl://mynewdatalakestore.azuredatalakestore.net/mynewfolder/

    >[AZURE.NOTE] Yllä olevaa syntaksia määrittää tiedoston kopioidaan kansioon kohde järvi tietovaraston tili. AdlCopy työkalu luo kansion, jos määritetty kansionimi ei ole.

    Voit pyydetään kirjoittamaan tunnistetiedot vallitessa käytössä järvi tietovaraston tilisi Azure-tilaus. Seuraavankaltaiselta tulos tulee näkyviin:

        Initializing Copy.
        Copy Started.|
        100% data copied.
        Finishing Copy.
        Copy Completed. 1 file copied.

3. Seuraava komento kopioi kaikki tiedostot tiettyyn kansioon lähteessä järvi tietovaraston tilin kohde järvi tietovaraston tilin kansioon.

        AdlCopy /Source adl://mydatastore.azuredatalakestore.net/mynewfolder/ /dest adl://mynewdatalakestore.azuredatalakestore.net/mynewfolder/

## <a name="use-adlcopy-with-data-lake-analytics-account-to-copy-data"></a>Tietojen kopioiminen (ja tietojen järvi Analytics-tili) AdlCopy avulla

Voit käyttää myös tietojen järvi Analytics-tilisi AdlCopy työn tietojen kopioiminen tallennustilan Azure-BLOB järvi tietosäilö. Yleensä käytetään tämä asetus, kun tiedot voidaan siirtää ovat gigatavua ja teratavua solualue ja haluat suorituskyvyn parantaminen ja ennakoitavissa siirtonopeuden.

Jos haluat käyttää tietojen järvi Analytics-tilisi AdlCopy Azure tallennustilan Blob-objektien kopioiminen, lähde (Azure-tallennustilan Blob) on lisättävä tietoja järvi Analytics-tilin lisääminen tietolähteeksi. Katso ohjeet Lisää tietolähteitä lisäämisestä tietojen järvi Analytics-tilin [hallinta tietojen järvi Analytics tilin tietolähteet](..//data-lake-analytics/data-lake-analytics-manage-use-portal.md#manage-account-data-sources).

>[AZURE.NOTE] Jos kopioit Azure järvi tietovaraston tilistä lähteenä tietojen järvi Analytics-tilillä, sinun ei tarvitse järvi tietovaraston tilin liittäminen tietojen järvi Analytics-tiliin. Lähde-kaupan liittäminen tietojen järvi Analytics-tiliin vaatimus on vain, kun lähde on Azure-tallennustilan tilin.

Suorita seuraava komento kopioi tallennustilan Azure-blob järvi tietovaraston tilin tiedot järvi Analytics-tilin avulla:

    AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container> /Account <data_lake_analytics_account> /Unit <number_of_data_lake_analytics_units_to_be_used>

Esimerkki:

    AdlCopy /Source https://mystorage.blob.core.windows.net/mycluster/example/data/gutenberg/ /dest swebhdfs://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ== /Account mydatalakeanalyticaccount /Units 2


Suorita vastaavasti seuraavalla komennolla kopioi tallennustilan Azure-blob järvi tietovaraston tilin tiedot järvi Analytics-tilin avulla:

    AdlCopy /Source adl://mysourcedatalakestore.azuredatalakestore.net/mynewfolder/ /dest adl://mydestdatastore.azuredatalakestore.net/mynewfolder/ /Account mydatalakeanalyticaccount /Units 2

## <a name="use-adlcopy-to-copy-data-using-pattern-matching"></a>Tietoja käyttämällä kuvion vastaavat AdlCopy avulla

Tässä osassa opit miten tietoja lähteestä AdlCopy avulla (Seuraavassa esimerkissä Microsoft käyttää Azure Blob-objektien tallennustila) käyttämällä kuvion vastaavat kohde järvi tietovaraston tiliin. Voit esimerkiksi kopioida kaikki tiedostot, joiden .csv-tunniste lähde-blob kohteeseen seuraavia ohjeita.

1. Avaa komentokehote ja siirry kansioon, johon AdlCopy on asennettu, yleensä `%HOMEPATH%\Documents\adlcopy`.

2. Suorita seuraava komento kopioi kaikki tiedostot, joiden *.csv tunniste tietyn Blob-objektien välityksellä lähde-säilö järvi tietosäilö:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container> /Pattern *.csv

    Esimerkki:

        AdlCopy /source https://mystorage.blob.core.windows.net/mycluster/HdiSamples/HdiSamples/FoodInspectionData/ /dest adl://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ== /Pattern *.csv

    
## <a name="billing"></a>Laskutus

* Jos käytät AdlCopy-työkalun yksittäisenä laskutetaan juniin kustannusten siirrettäessä tietoja, jos lähde Azure-tallennustilan tilin ei ole sama alue kuin järvi tietovaraston.

* Jos käytät AdlCopy-työkalun tietojen järvi Analytics-tilisi kanssa, normaalin [tietojen järvi Analytics laskutuksen korvaukset](https://azure.microsoft.com/pricing/details/data-lake-analytics/) koskevat.

## <a name="considerations-for-using-adlcopy"></a>AdlCopy Huomioitavaa

* AdlCopy (versiolle 1.0.5), tukee kopioiminen tietolähteiden, joiden muunnokset yli tuhansia tiedostoja ja kansioita. Kuitenkin Jos kohtaat ongelmia kopioiminen suuri tietojoukko, voit jakaa tiedostoja ja kansioita eri alikansiot ja avulla polku näiden alikansiot lähteenä sijaan.

## <a name="next-steps"></a>Seuraavat vaiheet

- [Tietojen suojaaminen järvi säilössä](data-lake-store-secure-data.md)
- [Azure tietojen järvi Analytics käyttäminen järvi tietosäilö](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure Hdinsightiin käyttäminen järvi tietosäilö](data-lake-store-hdinsight-hadoop-use-portal.md)

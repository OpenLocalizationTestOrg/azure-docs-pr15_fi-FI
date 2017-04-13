<properties
    pageTitle="Kopioida tai siirtää tietoja AzCopy ja tallennustilaa | Microsoft Azure"
    description="AzCopy-apuohjelmalla voit siirtää tai kopioida tietoja tai blob-taulukon ja tiedoston sisältöä. Azuren tallennustilaan tietojen kopioiminen paikallisina tiedostoina tai kopioida tietoja sisällä tai tallennustilan tilien välillä. Tietojen siirtäminen helposti Azure-tallennustilan."
    services="storage"
    documentationCenter=""
    authors="micurd"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/02/2016"
    ms.author="micurd"/>

# <a name="transfer-data-with-the-azcopy-command-line-utility"></a>Siirrä tiedot ja AzCopy komentorivivalitsimet-apuohjelma

## <a name="overview"></a>Yleiskatsaus

AzCopy on tarkoitettu tietojen kopioiminen ja sieltä pois Microsoft Azure-Blob, tiedoston ja taulukon tallennustilan yksinkertainen komentojen avulla saat parhaan mahdollisen suorituskyvyn Windows komentorivin apuohjelma. Voit kopioida tietoja objektista toiseen tallennustilan tilisi tai tallennustilan tilien välillä.

> [AZURE.NOTE] Tässä oppaassa oletetaan, että olet jo aiemmin [Azure-tallennustilan](https://azure.microsoft.com/services/storage/). Jos et, lukeminen [Azuren tallennustilaan johdanto](storage-introduction.md) ohjeista on hyötyä. Tärkeintä on sinun on [tallennustilan tilin](storage-create-storage-account.md#create-a-storage-account) luominen jotta AzCopy käytön aloittaminen.

## <a name="download-and-install-azcopy"></a>Lataa ja asenna AzCopy

### <a name="windows"></a>Windows

Lataa [uusin versio AzCopy](http://aka.ms/downloadazcopy).

### <a name="maclinux"></a>Mac tai Linux

AzCopy ei ole käytettävissä Mac tai Linux OSs. Azure CLI on kuitenkin tiedot kopioidaan ja sieltä pois Azuren tallennustilaan sopiva vaihtoehto. Lue lisätietoja [Azure CLI Azuren tallennustilaan kanssa käyttämällä](storage-azure-cli.md) .

## <a name="writing-your-first-azcopy-command"></a>Kirjoittaminen ensimmäinen AzCopy-komento

AzCopy komennot basic syntaksi on seuraava:

    AzCopy /Source:<source> /Dest:<destination> [Options]

Avaa komentorivi-ikkuna ja siirry AzCopy asennuksen hakemiston tietokoneessa - kohtaa, johon `AzCopy.exe` suoritettavan sijaitsee. Halutessasi voit lisätä AzCopy asennussijainnista järjestelmäpolussa. AzCopy asennetaan oletusarvoisesti `%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy` tai `%ProgramFiles%\Microsoft SDKs\Azure\AzCopy`.

Seuraavissa esimerkeissä kuvataan erilaisia tilanteita, joissa tietojen kopioiminen, ja Microsoft Azure-BLOB-tiedostot ja taulukot. Lisätietoja tarkan kussakin käytettävien parametrien [AzCopy parametrit](#azcopy-parameters) -osassa.

## <a name="blob-download"></a>BLOB: Lataa

### <a name="download-single-blob"></a>Lataa yksittäinen blob

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:"abc.txt"

Huomaa, että jos kansion `C:\myfolder` ei ole, AzCopy luo se ja lataa `abc.txt ` uuteen kansioon.

### <a name="download-single-blob-from-secondary-region"></a>Lataa yksittäinen blob toissijainen alue

    AzCopy /Source:https://myaccount-secondary.blob.core.windows.net/mynewcontainer /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt

Huomaa, että sinulla on oltava lukuoikeudet geo tarpeettomat tallennustilan käytössä.

### <a name="download-all-blobs"></a>Lataa kaikki Blob-objektien

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /S

Oletetaan, että seuraavat BLOB-objektit sijaitsevat määritetyssä säilössä:  

    abc.txt
    abc1.txt
    abc2.txt
    vd1\a.txt
    vd1\abcd.txt

Lataa-toiminnon hakemiston `C:\myfolder` sisältää seuraavat tiedostot:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\vd1\a.txt
    C:\myfolder\vd1\abcd.txt

Jos et määritä vaihtoehto `/S`, ei ole BLOB ladataan.

### <a name="download-blobs-with-specified-prefix"></a>Lataa BLOB määritetyn etuliitteellä

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:a /S

Oletetaan, että seuraavat BLOB-objektit sijaitsevat määritetyssä säilössä. Kaikki Blob-objektien etuliite alkavat `a` ladataan:

    abc.txt
    abc1.txt
    abc2.txt
    xyz.txt
    vd1\a.txt
    vd1\abcd.txt

Lataa-toiminnon kansion `C:\myfolder` sisältää seuraavat tiedostot:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt

Etuliitteen koskee näennäiskansio, joka muodostaa blob-nimen alkuosa. Edellä kuvatun esimerkin näennäiskansio ei vastaa määritettyä etuliitettä, joten se ei ole ladattu. Lisäksi jos vaihtoehto `\S` ei ole määritetty, AzCopy ei ladata kaikki BLOB-objektit.

### <a name="set-the-last-modified-time-of-exported-files-to-be-same-as-the-source-blobs"></a>Viimeksi muokannut-ajan viedyn tiedostojen olla sama kuin lähde-BLOB-objektit

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT

Voit jättää BLOB-lataamistoiminto on viimeksi muokattu ajan perusteella. Esimerkiksi jos haluat jättää pois BLOB-objektit, joiden on viimeksi muokattu-aika on sama tai uudempi kuin kohdetiedosto, lisätä `/XN` vaihtoehto:

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT /XN

Tai jos haluat jättää pois BLOB-objektit, joiden on viimeksi muokattu-aika on sama tai vanhempi kuin kohdetiedosto, lisätä `/XO` vaihtoehto:

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT /XO

## <a name="blob-upload"></a>BLOB: lataaminen

### <a name="upload-single-file"></a>Lataa tiedosto

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:"abc.txt"

Jos määritetty kohdesäilön ei ole, AzCopy luo se ja lataa tiedosto siihen.

### <a name="upload-single-file-to-virtual-directory"></a>Lataa Yksitiedostoinen näennäiskansio

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer/vd /DestKey:key /Pattern:abc.txt

Jos määritetty näennäiskansio ei ole, AzCopy ladata tiedoston sisältämään näennäiskansio nimessä (*kuten* `vd/abc.txt` yllä olevassa esimerkissä).

### <a name="upload-all-files"></a>Kaikkien tiedostojen lataaminen

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /S

Vaihtoehto, joka määrittää `/S` latauksia määritetyn kansion sisällön Blob storage rekursiivisesti, mikä tarkoittaa, että kaikki alikansiot ja niiden tiedostoja ladataan sekä. Oletetaan esimerkiksi, seuraavat tiedostot-kansiossa sijaitsevat `C:\myfolder`:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\subfolder\a.txt
    C:\myfolder\subfolder\abcd.txt

Lataa-toiminto, kun säilö sisältää seuraavat tiedostot:

    abc.txt
    abc1.txt
    abc2.txt
    subfolder\a.txt
    subfolder\abcd.txt

Jos et määritä vaihtoehto `/S`, AzCopy ei ladata rekursiivisesti. Lataa-toiminto, kun säilö sisältää seuraavat tiedostot:

    abc.txt
    abc1.txt
    abc2.txt

### <a name="upload-files-matching-specified-pattern"></a>Lataa tiedostot, jotka vastaavat määritettyä mallia

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:a* /S

Oletetaan, että seuraavat tiedostot-kansiossa sijaitsevat `C:\myfolder`:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\xyz.txt
    C:\myfolder\subfolder\a.txt
    C:\myfolder\subfolder\abcd.txt

Lataa-toiminto, kun säilö sisältää seuraavat tiedostot:

    abc.txt
    abc1.txt
    abc2.txt
    subfolder\a.txt
    subfolder\abcd.txt

Jos et määritä vaihtoehto `/S`, AzCopy ladata vain BLOB-objektit, jotka eivät sijaitse näennäiskansio:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt

### <a name="specify-the-mime-content-type-of-a-destination-blob"></a>Määritä destination Blob-objektien MIME-sisältötyyppi

Oletusarvon mukaan AzCopy määrittää kohde-blob sisältötyypin `application/octet-stream`. Alkavat 3.1.0 versio, voit määrittää eksplisiittisesti sisältötyypin kautta vaihtoehto `/SetContentType:[content-type]`. Seuraavalla syntaksilla määrittää kaikki Blob-objektien sisältötyypin Lataa-toiminto.

    AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.blob.core.windows.net/myContainer/ /DestKey:key /Pattern:ab /SetContentType:video/mp4

Jos määrität `/SetContentType` ilman arvoa, valitse AzCopy määrittää kunkin Blob-objektien tai tiedoston sisältötyyppi sen tiedostotunniste mukaan.

    AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.blob.core.windows.net/myContainer/ /DestKey:key /Pattern:ab /SetContentType

## <a name="blob-copy"></a>BLOB: kopioi

### <a name="copy-single-blob-within-storage-account"></a>Kopioi yhden blob Storage tilin

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1 /Dest:https://myaccount.blob.core.windows.net/mycontainer2 /SourceKey:key /DestKey:key /Pattern:abc.txt

Kun olet kopioinut blob Storage-tilin, [palvelinpuolen](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) kopiointi suoritetaan.

### <a name="copy-single-blob-across-storage-accounts"></a>Kopioi yhden blob Storage tilien välillä

    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt

Kun olet kopioinut blob Storage tilien välillä, [palvelinpuolen](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) kopiointi suoritetaan.

### <a name="copy-single-blob-from-secondary-region-to-primary-region"></a>Yksittäisen Blob-objektien kopioiminen ensisijainen alueen toissijainen alue

    AzCopy /Source:https://myaccount1-secondary.blob.core.windows.net/mynewcontainer1 /Dest:https://myaccount2.blob.core.windows.net/mynewcontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt

Huomaa, että sinulla on oltava lukuoikeudet geo tarpeettomat tallennustilan käytössä.

### <a name="copy-single-blob-and-its-snapshots-across-storage-accounts"></a>Kopioi yhden Blob-objektien ja sen tilannevedoksia tallennustilan tilien välillä

    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt /Snapshot

Kopiointi, kun kohde säilö sisällytetään blob ja sen tilannevedoksia. Jos Blob-objektien yllä olevassa esimerkissä on kaksi tilannevedoksen, säilö sisältää seuraavat Blob-objektien ja tilannevedoksia:

    abc.txt
    abc (2013-02-25 080757).txt
    abc (2014-02-21 150331).txt

### <a name="synchronously-copy-blobs-across-storage-accounts"></a>Kopioi synkronoidusti BLOB Storage tilien välillä

Oletusarvoisesti AzCopy kopioi tallennustilan päätepisteet tietojen asynkronisesti. Tämän vuoksi kopiointi suoritetaan käyttämällä vara kaistanleveyden kapasiteetin, joka on myös siitä, miten ei SLA nopeasti taustan blob kopioidaan ja AzCopy tarkistaa kopioi tila säännöllisesti, kunnes kopiointi on valmis tai epäonnistui.

`/SyncCopy` Vaihtoehto takaa kopiointi saavat yhdenmukaisen nopeus. AzCopy suorittaa synkronoitu kopio lataamalla BLOB määritetystä lähteestä kopioiminen paikalliseen muistiin ja lataamalla niitä Blob storage kohteeseen.

    AzCopy /Source:https://myaccount1.blob.core.windows.net/myContainer/ /Dest:https://myaccount2.blob.core.windows.net/myContainer/ /SourceKey:key1 /DestKey:key2 /Pattern:ab /SyncCopy

`/SyncCopy`voi luoda uusia juniin kustannukset verrattuna asynkroninen kopioi, voit käyttää tätä vaihtoehtoa, joka on saman alueen lähde tallennustilan-tiliksi välttämiseksi juniin kustannukset Azure-AM on suositellaan.

## <a name="file-download"></a>Tiedosto: Lataa

### <a name="download-single-file"></a>Lataa tiedosto

    AzCopy /Source:https://myaccount.file.core.windows.net/myfileshare/myfolder1/ /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt

Jos määritetty lähde on Azure tiedostoresurssin ja sitten tarkka tiedostonimi on määritettävä joko (*kuten* `abc.txt`) Lataa yksittäinen tiedosto tai määrittää `/S` Jaa rekursiivisesti olevien tiedostojen lataaminen. Määritä tiedoston kuvio- ja vaihtoehto yritetään `/S` yhdessä aiheuttaa virheen.

### <a name="download-all-files"></a>Kaikki tiedostot

    AzCopy /Source:https://myaccount.file.core.windows.net/myfileshare/ /Dest:C:\myfolder /SourceKey:key /S

Huomaa, että tyhjä kansioita ei ladata.

## <a name="file-upload"></a>Tiedosto: lataaminen

### <a name="upload-single-file"></a>Lataa tiedosto

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /Pattern:abc.txt

### <a name="upload-all-files"></a>Kaikkien tiedostojen lataaminen

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /S

Huomaa, että tyhjä kansioita ei siirretä.

### <a name="upload-files-matching-specified-pattern"></a>Lataa tiedostot, jotka vastaavat määritettyä mallia

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /Pattern:ab* /S

## <a name="file-copy"></a>Tiedosto: kopioi

### <a name="copy-across-file-shares"></a>Kopioi jaettujen tiedostojen välillä

    AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare1/ /Dest:https://myaccount2.file.core.windows.net/myfileshare2/ /SourceKey:key1 /DestKey:key2 /S

### <a name="copy-from-file-share-to-blob"></a>Kopioi tiedostoresurssin blob

    AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare/ /Dest:https://myaccount2.blob.core.windows.net/mycontainer/ /SourceKey:key1 /DestKey:key2 /S

Huomaa, että tiedostojen tallentamisesta asynkroninen kopioiminen sivun Blob, ei tueta.

### <a name="copy-from-blob-to-file-share"></a>Jaettua tiedostoresurssia Blob-objektien kopioiminen

    AzCopy /Source:https://myaccount1.blob.core.windows.net/mycontainer/ /Dest:https://myaccount2.file.core.windows.net/myfileshare/ /SourceKey:key1 /DestKey:key2 /S

### <a name="synchronously-copy-files"></a>Tiedostojen kopioiminen synkronoidusti

Voit määrittää `/SyncCopy` asetus, jos haluat kopioida tietoja tiedostojen tallentamisesta tiedosto säilöön, -Blob-objektien tallennustilaan tiedoston tallennustilan ja Blob-säiliö tiedostosäilön synkronoidusti, AzCopy tekee tämän lähdetietojen lataaminen paikalliseen muistiin ja lataa se uudelleen kohteeseen.

    AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare1/ /Dest:https://myaccount2.file.core.windows.net/myfileshare2/ /SourceKey:key1 /DestKey:key2 /S /SyncCopy

Kopioiminen tiedostojen tallentamisesta Blob-objektien tallennustilaan blob oletustietotyyppi on estä blob, käyttäjä voi määrittää vaihtoehto `/BlobType:page` kohde blob-tyypin muuttaminen.

Huomaa, että `/SyncCopy` voi luoda lisää juniin kustannusten vertailu asynkroninen kopioon, voit käyttää tätä vaihtoehtoa joka on saman alueen lähde tallennustilan-tiliksi välttämiseksi juniin kustannukset Azure-AM on suositellaan.

## <a name="table-export"></a>Taulukko: Vie

### <a name="export-table"></a>Vie taulukko

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key

AzCopy kirjoittaa luettelon tiedoston määritettyyn kohdekansioon. Tiedostojen tiedoston käytetään tuontiprosessin ja Etsi tarvittavat datatiedostot ja tehdä tietojen kelpoisuuden tarkistaminen. Tiedostojen tiedoston käyttää seuraavaa nimeämiskäytäntöä oletusarvoisesti:

    <account name>_<table name>_<timestamp>.manifest

Käyttäjä voi määrittää myös vaihtoehto `/Manifest:<manifest file name>` voit määrittää luettelon tiedostonimi.

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /Manifest:abc.manifest

### <a name="split-export-into-multiple-files"></a>Jakaa useisiin tiedostoihin vienti

    AzCopy /Source:https://myaccount.table.core.windows.net/mytable/ /Dest:C:\myfolder /SourceKey:key /S /SplitSize:100

AzCopy käytetään *äänenvoimakkuuden indeksi* Jaa tietojen tiedostonimissä erottaa useita tiedostoja. Äänenvoimakkuuden hakemiston koostuu kahdesta osasta, *osion avaimen alueen indeksin* luominen ja *jakaminen tiedostojen hakemisto*. Molemmat indeksit ovat nollasta.

Osion avaimen alueen indeksi on 0, jos käyttäjä ei määritä vaihtoehto `/PKRS`.

Oletetaan, että AzCopy Luo kaksi datatiedostot, kun käyttäjän määrittää vaihtoehto `/SplitSize`. Tietoja tiedostonimet voi olla:

    myaccount_mytable_20140903T051850.8128447Z_0_0_C3040FE8.json
    myaccount_mytable_20140903T051850.8128447Z_0_1_0AB9AC20.json

Huomaa, että pienin mahdollinen arvo vaihtoehto `/SplitSize` on 32 Mt. Jos määritetty kohde on Blob-objektien tallennustilaan, AzCopy jakaa tiedoston, kun sen kokoa saavuttaa blob enimmäiskoko (200 gt), riippumatta siitä, onko vaihtoehto `/SplitSize` käyttäjän määrittämän.

### <a name="export-table-to-json-or-csv-data-file-format"></a>Taulukon vieminen JSON- tai CSV tiedot-tiedostomuotoon

Oletusarvoisesti AzCopy Vie taulukot JSON datatiedostot. Voit määrittää haluamasi vaihtoehto `/PayloadFormat:JSON|CSV` vietävät JSON-tai CSV-taulut.

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /PayloadFormat:CSV

Kun paketti CSV-muodossa, joka määrittää, AzCopy myös luoda rakennetiedostoon tiedostotunniste `.schema.csv` jokaiselle datatiedostolle.

### <a name="export-table-entities-concurrently"></a>Vie taulukon kohteiden samanaikaisesti

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /PKRS:"aa#bb"

AzCopy alkavat rinnakkaiset toiminnot Vie kohteita, kun käyttäjä määrittää vaihtoehto `/PKRS`. Kunkin toiminnon Vie yksi osio alue.

Huomaa, että samanaikaisia toimintoja, myös hallitsemat vaihtoehto `/NC`. AzCopy käyttää oletusarvon core suorittimien määrän `/NC` kopioitaessa taulukon kohteita, vaikka `/NC` ei ole määritetty. Kun käyttäjä määrittää vaihtoehto `/PKRS`, AzCopy käyttää pienempi näistä kahdesta arvosta - osio ja tärkeimmät alueet tai epäsuorasti määritetty rinnakkaiset toiminnot -, rinnakkaiset toiminnot Aloita lukumäärän. Lisätietoja, kirjoita `AzCopy /?:NC` komentorivillä.

### <a name="export-table-to-blob"></a>Vie taulukko blob

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:https://myaccount.blob.core.windows.net/mycontainer/ /SourceKey:key1 /Destkey:key2

AzCopy Luo JSON datatiedoston kanssa seuraavan nimeämiskäytännön blob-säilö:

    <account name>_<table name>_<timestamp>_<volume index>_<CRC>.json

Luotu JSON-datatiedoston noudattaa mahdollisimman vähän metatietojen paketti-muotoa. Lisätietoja paketti-muodon on artikkelissa [Taulukon palvelun toimintojen muodon tiedot](http://msdn.microsoft.com/library/azure/dn535600.aspx).

Huomaa, että vietäessä taulukoiden BLOB AzCopy Lataa taulukon kohteiden paikallisen tilapäinen datatiedostoihin ja lataa näiden yksiköiden blob. Näiden tilapäinen datatiedostojen liikkeelle Päivyri tiedostokansio oletusarvon polulla "<code>%LocalAppData%\Microsoft\Azure\AzCopy</code>", voit määrittää asetuksen/Z: [Päivyri-tiedostokansio], jos haluat muuttaa Päivyri tiedoston kansiosijainti ja muuta näin tietojen tilapäinen tiedostojen sijainti. Tietojen tilapäinen tiedostojen kokoa on päättänyt taulukon kohteiden koon ja määritetyssä vaihtoehto /SplitSize, vaikka tietojen tilapäinen tiedoston paikalliseen levyasemaan poistetaan heti, kun se on ladattu blob, varmista, että sinulla on paikallinen levytila näiden tietojen tilapäinen tiedostojen tallentamiseen, ennen kuin ne poistetaan.

## <a name="table-import"></a>Taulukko: tuonti

### <a name="import-table"></a>Tuo taulukko

    AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.table.core.windows.net/mytable1/ /DestKey:key /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:InsertOrReplace

Vaihtoehto `/EntityOperation` osoittaa kohteiden lisääminen taulukkoon. Mahdolliset arvot ovat:

- `InsertOrSkip`: Ohittaa olemassa olevan kohteen tai lisää uuden kohteen, jos sitä ei ole taulukossa.
- `InsertOrMerge`: Yhdistää olemassa olevan kohteen tai lisää uuden kohteen, jos sitä ei ole taulukossa.
- `InsertOrReplace`: Korvaa olemassa olevan kohteen tai lisää uuden kohteen, jos sitä ei ole taulukossa.

Huomaa, että et voi määrittää vaihtoehto `/PKRS` tuo skenaariossa. Toisin kuin Vie-skenaario, jossa on määritettävä vaihtoehto `/PKRS` käynnistämiseen rinnakkaiset toiminnot AzCopy oletusarvoisesti alkavat rinnakkaiset toiminnot taulukon tuonnin aikana. Rinnakkaiset toiminnot, jotka on aloitettu oletusmäärän vastaa core; suorittimien määrä Voit määrittää eri määrän samanaikaisia vaihtoehdolla `/NC`. Lisätietoja, kirjoita `AzCopy /?:NC` komentorivillä.

Huomaa, että AzCopy tukee vain tietojen tuonti: JSON, ei CSV. AzCopy ei tue taulukon tuonnissa käyttäjän luoma JSON ja tiedostojen luettelon. Molempien tiedostojen on oltava peräisin AzCopy-taulukon vieminen. Voit välttää virheet, Älä muokkaa viedyn JSON tai luettelon tiedosto.

### <a name="import-entities-to-table-using-blobs"></a>Tuot kohteita taulukon käyttämällä BLOB-objektit

Oletetaan, että Blob-säilö sisältää seuraavat: A JSON tiedoston edustavat Azure-taulukosta ja sen mukana luettelon tiedosto.

    myaccount_mytable_20140103T112020.manifest
    myaccount_mytable_20140103T112020_0_0_0AF395F1DC42E952.json

Voit suorittaa kohteiden tuominen taulukkoa, jossa on luettelo tiedoston kyseisen blob-säilö on seuraava komento:

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:https://myaccount.table.core.windows.net/mytable /SourceKey:key1 /DestKey:key2 /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:"InsertOrReplace"

## <a name="other-azcopy-features"></a>AzCopy muut ominaisuudet

### <a name="only-copy-data-that-doesnt-exist-in-the-destination"></a>Kopioi vain tiedot, joita ei ole kohde

`/XO` Ja `/XN` parametrien avulla voit vanhat tai uudempaa versiota lähde resurssien jättäminen pois kopioitava tarpeen mukaan. Jos haluat vain kopioi lähde resurssit, joita ei ole kohde, voit määrittää molempien parametrien AzCopy-komennon:

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /XO /XN

    /Source:C:\myfolder /Dest:http://myaccount.file.core.windows.net/myfileshare /DestKey:<destkey> /S /XO /XN

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:http://myaccount.blob.core.windows.net/mycontainer1 /SourceKey:<sourcekey> /DestKey:<destkey> /S /XO /XN

Huomautus: Tämä ei tueta, kun lähde- tai kohde on taulukko.

### <a name="use-a-response-file-to-specify-command-line-parameters"></a>Määritä komentoriviparametreja vastaustiedoston avulla

    AzCopy /@:"C:\responsefiles\copyoperation.txt"

Voit lisätä minkä tahansa AzCopy komentoriviparametreja vastauksen tiedostossa. AzCopy käsittelee parametrit-tiedoston aivan kuin ne oli määritetty komentorivillä, suorittamiseen suoraan korvaaminen tiedoston sisällön.

Oletetaan, että vastauksen tiedoston nimeltä `copyoperation.txt`, joka sisältää seuraavat rivit. AzCopy kunkin parametrin voidaan määrittää yhdellä rivillä

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /Y

tai eri riveille seuraavasti:

    /Source:http://myaccount.blob.core.windows.net/mycontainer
    /Dest:C:\myfolder
    /SourceKey:<sourcekey>
    /S
    /Y

AzCopy epäonnistuu, jos parametrin jakaminen kaksi riviä, kuten kuvassa varten `/sourcekey` parametri:

    http://myaccount.blob.core.windows.net/mycontainer
    C:\myfolder
    /sourcekey:
    <sourcekey>
    /S
    /Y

### <a name="use-multiple-response-files-to-specify-command-line-parameters"></a>Usean vastauksen-tiedoston avulla voit määrittää komentoriviparametreja

Oletetaan, että vastauksen tiedoston nimeltä `source.txt` , joka määrittää tietolähteen säilön:

    /Source:http://myaccount.blob.core.windows.net/mycontainer

Ja vastaustiedosto nimeltä `dest.txt` , joka määrittää kohdekansio tiedostojärjestelmän:

    /Dest:C:\myfolder

Ja vastaustiedosto nimeltä `options.txt` , joka määrittää AzCopy asetukset:

    /S /Y

Soita AzCopy nämä vastaus-tiedostoja, mikä sijaitsevat hakemiston `C:\responsefiles`, tällä komennolla:

    AzCopy /@:"C:\responsefiles\source.txt" /@:"C:\responsefiles\dest.txt" /SourceKey:<sourcekey> /@:"C:\responsefiles\options.txt"   

AzCopy käsittelee komennon samalla tavalla kuin jos olet lisännyt kaikki yksittäisiä parametreja komentorivillä:

    AzCopy /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /Y

### <a name="specify-a-shared-access-signature-sas"></a>Määritä jaettu käyttö allekirjoituksen (SAS)

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1 /Dest:https://myaccount.blob.core.windows.net/mycontainer2 /SourceSAS:SAS1 /DestSAS:SAS2 /Pattern:abc.txt

Voit myös määrittää SAS-säilöä URI:

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1/?SourceSASToken /Dest:C:\myfolder /S

### <a name="journal-file-folder"></a>Päivyrin tiedostokansio

Aina, kun lähetät komennon AzCopy, se tarkistaa, onko Päivyri tiedoston olemassa oletuskansioon, tai onko se on kansiossa, jonka olet määrittänyt tämän asetuksen avulla. Jos Päivyri-tiedostoa ei ole joko paikassa, AzCopy toiminto tulkitsee uusi ja luo uusi Päivyri-tiedosto.

Jos Päivyri-tiedosto ole, AzCopy Tarkista, vastaako komentorivi, jonka olet kirjoittanut komentoriviltä Päivyri-tiedostossa. Jos komento kaksi riviä vastaavat, AzCopy jatkaa puutteellinen toiminto. Jos ne eivät vastaa, voit pyytää korvata joko Päivyri-tiedoston, voit aloittaa uuden toiminnon ja Peruuta tämä toiminto.

Jos haluat käyttää Päivyri tiedoston oletussijainti:

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Z

Jos jätät vaihtoehto `/Z`, tai määrittää `/Z` kansiopolku, ilman edellä kuvatulla AzCopy Luo Päivyrin tiedoston oletussijainti, joka on `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`. Jos Päivyri-tiedosto on jo olemassa, AzCopy jatkaa Päivyri-tiedoston toiminto.

Jos haluat määrittää mukautetun sijainnin Päivyri tiedoston:

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Z:C:\journalfolder\

Tässä esimerkissä Luo Päivyrin-tiedoston, jos se ei ole jo olemassa. Jos sitä ole AzCopy jatkaa Päivyri-tiedoston toiminto.

Jos haluat jatkaa AzCopy-toimintoa:

    AzCopy /Z:C:\journalfolder\

Tässä esimerkissä jatkaa viimeisen toiminnon, joka on voinut suorittaa loppuun.

### <a name="generate-a-log-file"></a>Luo lokitiedoston

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /V

Jos määrität valitsimen `/V` mutta estää tiedostopolusta yksityiskohtainen lokiin AzCopy luo lokitiedoston oletussijaintiin, joka on `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`.

Muussa tapauksessa voit luoda lokitiedosto on oma sijainti:

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /V:C:\myfolder\azcopy1.log

Jos määrität seuraavan vaihtoehdon suhteellinen polku `/V`, kuten `/V:test/azcopy1.log`, valitse yksityiskohtainen loki luodaan käytön hakemiston sisällä alikansion `test`.

### <a name="specify-the-number-of-concurrent-operations-to-start"></a>Rinnakkaiset toiminnot Aloita määrän määrittäminen

Vaihtoehto `/NC` määrittää samanaikainen kopiointi määrän. Oletusarvon mukaan AzCopy käynnistyy niin, että tiedonsiirto siirto rinnakkaiset toiminnot tietty määrä. Taulukon toimintojen samanaikaisia toimintoja, jotka on sama kuin sinulla suorittimien määrän. Blob-objektien ja tiedoston toimintoja, samanaikaisia toimintoja, jotka on yhtä suuri kuin 8 kertaa on suorittimien määrän. Jos käytät AzCopy hidas verkon kautta, voit määrittää /NC virheen aiheuttaa resurssin kilpailua välttäminen alemmaksi numeroksi.

### <a name="run-azcopy-against-azure-storage-emulator"></a>Suorita AzCopy Azure tallennustilan emulaattorin vasten

Voit suorittaa AzCopy vastaan [Azure-tallennustilan emulaattorin](storage-use-emulator.md) BLOB:

    AzCopy /Source:https://127.0.0.1:10000/myaccount/mycontainer/ /Dest:C:\myfolder /SourceKey:key /SourceType:Blob /S

ja taulukot:

    AzCopy /Source:https://127.0.0.1:10002/myaccount/mytable/ /Dest:C:\myfolder /SourceKey:key /SourceType:Table

## <a name="azcopy-parameters"></a>AzCopy parametrit

Alla on kuvattu AzCopy parametrit. Voit myös kirjoittaa jonkin seuraavista komennoista komentoriviltä ohjeita käyttämällä AzCopy:

- Lisätietoja komentorivin artikkelista AzCopy:`AzCopy /?`
- Mikä tahansa AzCopy-parametrin yksityiskohtaisia ohjeita:`AzCopy /?:SourceKey`
- Komentorivin Esimerkkejä:`AzCopy /?:Samples`

### <a name="sourcesource"></a>/ Lähde: "tietolähteen"

Määrittää lähdetietoihin, johon haluat kopioida. Lähteen voi olla tiedostojärjestelmän hakemiston, blob-säilö, blob näennäiskansio, tallennustilan jaetuista tiedostoresursseista, tallennustilan tiedostohakemisto tai Azuren taulukkojen.

**Koskevat:** BLOB-tiedostot-taulukot

### <a name="destdestination"></a>/ Kohde: "kohde"

Määrittää voit kopioida kohteen. Kohde voi olla tiedostojärjestelmän hakemiston, blob-säilö, blob näennäiskansio, tallennustilan jaetuista tiedostoresursseista, tallennustilan tiedostohakemisto tai Azuren taulukkojen.

**Koskevat:** BLOB-tiedostot-taulukot

### <a name="patternfile-pattern"></a>/ Kuviota: "tiedosto-kuvion"

Määrittää tiedoston kaavan, joka osoittaa, mitä tiedostoja haluat kopioida. /Pattern-parametrin toimintaa määräytyy lähdetietojen sijainnin ja siihen, onko rekursiivinen tila-vaihtoehto. Rekursiivisen tilan määritetään asetus valitsinta/s. kautta

Jos määritetty lähde on hakemiston tiedostojärjestelmän, vakio yleismerkit ovat voimassa ja annetun tiedoston rakenteen vastaa vastaan tiedostot kansiossa. Jos asetus /S on määritetty, valitse AzCopy vastaa myös alikansiot kansion alapuolella olevien tiedostojen vastaan määritettyä kaavaa.

Jos määritetty lähde on blob-säilö tai näennäiskansio, yleismerkkejä ovat ei käytetä. Jos asetus /S on määritetty, valitse AzCopy tulkitsee määritetyn tiedoston kuvion blob-etuliite. Jos asetus /S ei ole määritetty, valitse AzCopy etsii tiedoston tarkka Blob-objektien nimien perusteella.

Jos määritetty lähde on Azure tiedostoresurssin ja valitse joko Määritä tarkka tiedostonimi (esimerkiksi abc.txt) Kopioi yhden tiedoston, tai määrittää /S kopioi kaikki tiedostot Jaa rekursiivisesti. Määritä tiedoston kuvio- ja vaihtoehto yritetään /S yhdessä aiheuttaa virheen.

AzCopy käyttää kirjainkoon vastaavat, kun /Source on blob-säilö tai blob näennäiskansio ja käyttää kaikissa muissa tapauksissa asennustavan vastaavat.

Käytetään, kun tiedosto ei ole kuvio on määritetty oletusarvoinen tiedoston kuvio on *.* tiedostojärjestelmässä tai tyhjä etuliite Azure-tallennuspaikka. Usean tiedoston kuvioiden määrittäminen ei tueta.

**Koskevat:** BLOB-tiedostot

### <a name="destkeystorage-key"></a>/ DestKey: "avain tallennustilan"

Määrittää kohde resurssin tallennustilan tilin-näppäintä.

**Koskevat:** BLOB-tiedostot-taulukot

### <a name="destsassas-token"></a>/ DestSAS: "sas-tunnuksen"

Määrittää jaettu Access allekirjoitus (SAS) luku- ja KIRJOITUSOIKEUDET omaavista kohteen (jos saatavilla). Ympäröivät lainausmerkit Suojaussidokset, kun se saa sisältää komentorivin erikoismerkkejä.

Jos kohde resurssi on blob-säilö, tiedostoresurssin tai taulukon, voit joko määrittää asetus SAS tunnuksen perään tai voit määrittää Suojaussidokset osana blob kohdesäilön, tiedostoresurssin tai taulukon URI-vaihtoehto ei ole.

Jos lähde- ja ovat molemmat BLOB-objektit, kohde Blob-objektien on sijaittava saman tallennustilan tilin nimellä lähde-blob.

**Koskevat:** BLOB-tiedostot-taulukot

### <a name="sourcekeystorage-key"></a>/ SourceKey: "avain tallennustilan"

Määrittää lähteen resurssin tallennustilan tilin-näppäintä.

**Koskevat:** BLOB-tiedostot-taulukot

### <a name="sourcesassas-token"></a>/ SourceSAS: "sas-tunnuksen"

Määrittää jaettu Access-allekirjoituksen luku- ja luettelon käyttöoikeuksien lähteen avulla (jos saatavilla). Ympäröivät lainausmerkit Suojaussidokset, kun se saa sisältää komentorivin erikoismerkkejä.

Jos lähde-resurssi on blob-säilö, eikä näppäintä Suojaussidokset on annettu blob-säilö luetaan anonyymin käytön kautta.

Jos lähde on jaetun tiedostoresurssin tai taulukko tai näppäintä Suojaussidokset on annettava.

**Koskevat:** BLOB-tiedostot-taulukot

### <a name="s"></a>/S

Määrittää rekursiivisen tilan kopioida toimintoja. Rekursiiviset tilassa AzCopy kopioi kaikki BLOB tai tiedostot, jotka vastaavat määritettyä mallia, mukaan lukien alikansioon.

**Koskevat:** BLOB-tiedostot

### <a name="blobtypeblock--page--append"></a>/ BlobType: "Estä" | "sivu" | "Lisää"

Määrittää, onko kohde Blob-objektien estä Blob-objektien sivun Blob-objektien ja liitä-blob. Tämä asetus on käytettävissä vain, kun lataat blob. Muussa tapauksessa ilmenee virhe. Jos kohde on blob ja tämä vaihtoehto ei ole määritetty, oletusarvoisesti AzCopy Luo estä Blob-objektien.

**Koskevat:** BLOB-objektit

### <a name="checkmd5"></a>/ CheckMD5

Laskee MD5-hajautuksen kopioitujen tietojen varten ja varmistaa, että MD5-hajautuksen tallennetaan blob tai tiedoston sisällön MD5 vastaa lasketun hash. MD5-valintaruutu on poistettu käytöstä oletusarvoisesti, joten sinun on määritettävä tämän asetuksen voit suorittaa MD5-valintaruutu, kun tietojen lataus.

Huomaa, että Azuren tallennustilaan ei takaa, että tallennettu Blob-objektien tai tiedoston MD5-hajautuksen on ajan tasalla. Se on päivitettävä MD5 Blob-objektien tai tiedoston muutetaan asiakkaan vastuu.

AzCopy asettaa Azure-blob tai tiedoston sisällön MD5-ominaisuuden aina ladattuasi-palveluun.  

**Koskevat:** BLOB-tiedostot

### <a name="snapshot"></a>/ Tilannevedos

Ilmaisee, siirretäänkö tilannevedoksia. Tämä asetus on käytössä vain, kun lähde on blob.

Siirrettyjen blob-tilannevedoksia nimetään uudelleen tässä muodossa: .extension blob-nimi (tilannevedos-aika)

Oletusarvon mukaan tilannevedoksia ei kopioida.

**Koskevat:** BLOB-objektit

### <a name="vverbose-log-file"></a>/ V: [yksityiskohtainen-lokitiedoston]

Tulostaa yksityiskohtainen tilasanomat lokitiedostoon.

Oletusarvon mukaan yksityiskohtainen lokitiedoston nimi on AzCopyVerbose.log- `%LocalAppData%\Microsoft\Azure\AzCopy`. Jos määrität aiemmin luotu tiedostosijainti tämän vaihtoehdon, yksityiskohtainen loki lisätään tiedostoon.  

**Koskevat:** BLOB-tiedostot-taulukot

### <a name="zjournal-file-folder"></a>/ Z: [Päivyri-tiedostokansio]

Määrittää jatkaminen toiminnon Päivyri tiedostokansio.

Jos toiminto on keskeytetty jatkaminen AzCopy tukee.

Jos tämä vaihtoehto ei ole määritetty tai se on määritetty ilman kansiopolku, AzCopy luominen oletussijaintiin, joka on % LocalAppData%\Microsoft\Azure\AzCopy Päivyri-tiedosto.

Aina, kun lähetät komennon AzCopy, se tarkistaa, onko Päivyri tiedoston olemassa oletuskansioon, tai onko se on kansiossa, jonka olet määrittänyt tämän asetuksen avulla. Jos Päivyri-tiedostoa ei ole joko paikassa, AzCopy toiminto tulkitsee uusi ja luo uusi Päivyri-tiedosto.

Jos Päivyri-tiedosto ole, AzCopy Tarkista, vastaako komentorivi, jonka olet kirjoittanut komentoriviltä Päivyri-tiedostossa. Jos komento kaksi riviä vastaavat, AzCopy jatkaa puutteellinen toiminto. Jos ne eivät vastaa, voit pyytää korvata joko Päivyri-tiedoston, voit aloittaa uuden toiminnon ja Peruuta tämä toiminto.

Päivyri-tiedosto poistetaan onnistumiseen toiminnon yhteydessä.

Huomaa, että jatkaminen Päivyri-tiedostosta toiminnon luoma AzCopy aiemman version ei tueta.

**Koskevat:** BLOB-tiedostot-taulukot

### <a name="parameter-file"></a>/@:"parameter-file"

Määrittää parametrit sisältävän tiedoston. AzCopy käsittelee parametrit-tiedostossa, aivan kuin ne oli määritetty komentorivillä.

Vastauksen tiedostoon voit joko määrittää useita parametreja yhdellä rivillä tai määrittää kunkin parametrin omalle rivilleen. Huomautus yksittäisiä parametria ei kattavat useita rivejä.

Vastauksen tiedostoja voi sisältää kommentteja rivit, joiden alussa on #-merkki.

Voit määrittää useita vastauksen tiedostoja. Huomaa kuitenkin, että AzCopy ei tue sisäkkäisiä vastauksen tiedostoja.

**Koskevat:** BLOB-tiedostot-taulukot

### <a name="y"></a>/ Y

Estää kaikki AzCopy vahvistus kehotteet.

**Koskevat:** BLOB-tiedostot-taulukot

### <a name="l"></a>L

Määrittää luettelon toiminnon. tietoja kopioidaan.

AzCopy tulkitsee käyttäen tämän simuloinnissa suorittamiseen komentoriviltä ilman, että tämä vaihtoehto, l-asetus ja laskea, kuinka monta objektia kopioidaan, voit määrittää /V samanaikaisesti tarkistamaan, mitkä objektit kopioidaan yksityiskohtainen lokiin vaihtoehto.

Tämä vaihtoehto toimintaa määräytyy myös lähdetietojen sijainnin ja tavoitettavuus rekursiivinen tilassa vaihtoehto /S ja tiedoston kuvio-asetuksen /Pattern.

AzCopy edellyttää luettelon ja lue tämä Lähdesijainnin lupaa, kun tämä vaihtoehto.

**Koskevat:** BLOB-tiedostot

### <a name="mt"></a>/MT

Määrittää ladattua tiedostoa viimeksi muokanneen aika on sama kuin lähteen Blob-objektien tai tiedoston.

**Koskevat:** BLOB-tiedostot

### <a name="xn"></a>/XN

Jättää huomiotta uudempaan lähde-resurssi. Resurssin ei kopioida, jos edellisen muokkauksen ajankohdan lähde on sama tai uudempi kuin kohde.

**Koskevat:** BLOB-tiedostot

### <a name="xo"></a>/XO

Jättää huomiotta vanhempia lähde-resurssi. Resurssin ei kopioida, jos edellisen muokkauksen ajankohdan lähde on sama tai vanhempi kuin kohde.

**Koskevat:** BLOB-tiedostot

### <a name="a"></a>/A

Lataa vain tiedostot, joiden arkisto-määrite.

**Koskevat:** BLOB-tiedostot

### <a name="iarashcnetoi"></a>/ IA: [RASHCNETOI]

Lataa vain tiedostot, jotka on määritetty määritteet määrittäminen jokin.

Käytettävissä olevat määritteet ovat seuraavat:

- R = vain luku-tiedostot
- A = valmis arkistointia varten
- S = järjestelmätiedostoja
- T = piilotetut tiedostot
- C = pakatut tiedostot
- N = Normaali tiedostot
- E = salattu tiedostot
- T = väliaikaiset tiedostot
- O = Offline-tiedostot
- I = ei indeksoitu tiedostot

**Koskevat:** BLOB-tiedostot

### <a name="xarashcnetoi"></a>/ XA: [RASHCNETOI]

Jättää huomiotta tiedostoja, joissa on jonkin määritetyt määritteet määrittäminen.

Käytettävissä olevat määritteet ovat seuraavat:

- R = vain luku-tiedostot
- A = valmis arkistointia varten
- S = järjestelmätiedostoja
- T = piilotetut tiedostot
- C = pakatut tiedostot
- N = Normaali tiedostot
- E = salattu tiedostot
- T = väliaikaiset tiedostot
- O = Offline-tiedostot
- I = ei indeksoitu tiedostot

**Koskevat:** BLOB-tiedostot

### <a name="delimiterdelimiter"></a>/ Erottimen: "erotin"

Osoittaa erotinmerkki, jolla erotetaan Näennäiskansiot blob nimi.

Oletusarvoisesti AzCopy käyttää / erottimen merkkinä. Kuitenkin AzCopy tukee yleisiä merkin käyttämällä (kuten @, # tai %) erottimena. Jos haluat sisällyttää yhden komentorivillä erikoismerkkejä, kirjoita tiedostonimi lainausmerkeissä.

Tämä asetus on vain sovellettavan BLOB lataamista varten.

**Koskevat:** BLOB-objektit

### <a name="ncnumber-of-concurrent-operations"></a>/ NC: "luku-,-samanaikaisen-toimintojen"

Määrittää samanaikaisia toimintoja.

Oletusarvoisesti AzCopy käynnistyy niin, että tiedonsiirto siirto rinnakkaiset toiminnot tietty määrä. Huomaa, että rinnakkaiset toiminnot ympäristössä, jossa on hidas suuri määrä voi ärsyttävä liikaa täyttää verkkoyhteys ja estä tekemästä täysin toimintoja. Rajoita rinnakkaiset toiminnot todellinen käytettävissä kaistanleveys perusteella.

Rinnakkaiset toiminnot yläraja on 512.

**Koskevat:** BLOB-tiedostot-taulukot

### <a name="sourcetypeblob--table"></a>/ SourceType: "Blob" | "Taulukon"

Määrittää, että `source` resurssi on käytettävissä tallennustilan emulaattori käytössä paikallinen kehitysympäristö blob.

**Koskevat:** BLOB-taulukot

### <a name="desttypeblob--table"></a>/ DestType: "Blob" | "Taulukon"

Määrittää, että `destination` resurssi on käytettävissä tallennustilan emulaattori käytössä paikallinen kehitysympäristö blob.

**Koskevat:** BLOB-taulukot

### <a name="pkrskey1key2key3"></a>/ PKRS: "Avain1 Avain2 # key3 #..."

Jakaa osion avaimen alueen käyttöön rinnakkain, jossa vientitoiminnon nopeuttaa taulukon tietojen vieminen.

Jos tämä vaihtoehto ei ole määritetty, AzCopy käyttää yhden viestiketjun vietävän taulukon kohteiden. Jos käyttäjä määrittää /PKRS esimerkiksi: "aa #bb" ja valitse AzCopy alkaa kolmella rinnakkaisten toimintoja.

Kunkin toiminnon vie jonkin kolme osiota tärkeimmät alueet, alla kuvatulla tavalla:

  [ensimmäisen-osion-näppäintä, aa)

  [aa, bb)

  [bb viimeksi-osion-näppäintä]

**Koskevat:** Taulukot

### <a name="splitsizefile-size"></a>/ SplitSize: "tiedostokoko"

Määrittää vietävän tiedoston jakaminen koko megatavuina mahdollisimman vähän sallittu arvo on 32.

Jos tämä vaihtoehto ei ole määritetty, AzCopy Vie taulukkotiedot yhteen tiedostoon.

Jos taulukkotiedot viedään blob ja vietävän tiedostokoko saavuttaa Blob-objektien koon 200 gt rajoitus, valitse AzCopy jakaa vietävän tiedoston, vaikka tämä asetus ei ole määritetty.

**Koskevat:** Taulukot

### <a name="entityoperationinsertorskip--insertormerge--insertorreplace"></a>/ EntityOperation: "InsertOrSkip" | "InsertOrMerge" | "InsertOrReplace"

Määrittää taulukon tietojen tuonti toimintaa.

- InsertOrSkip – ohittaa olemassa olevan kohteen tai lisää uuden kohteen, jos sitä ei ole taulukossa.

- InsertOrMerge - yhdistää olemassa olevan kohteen tai lisää uuden kohteen, jos sitä ei ole taulukossa.

- InsertOrReplace - korvaa olemassa olevan kohteen tai lisää uuden kohteen, jos sitä ei ole taulukossa.

**Koskevat:** Taulukot

### <a name="manifestmanifest-file"></a>/ Tiedostojen: "tiedosto luettelo"

Määrittää luettelon tiedoston taulukon vieminen ja tuontitoiminnon.

Tämä asetus on valinnainen vientitoiminnon aikana, AzCopy Luo luettelo tiedoston, jonka valmiiksi määritetty nimi, jos tämä vaihtoehto ei ole määritetty.

Tämä asetus on tarvittavat datatiedostot etsimiseen tuontitoiminnon aikana.

**Koskevat:** Taulukot

### <a name="synccopy"></a>/ SyncCopy

Ilmaisee, kopioi synkronoidusti BLOB tai tiedostoja Azuren tallennustilaan päätepisteet välillä.

Oletusarvoisesti AzCopy käyttää palvelinpuolen asynkroninen kopiota. Määritä tämä asetus on synkronoitu kopio, joka lataa BLOB tai tiedostojen paikallinen muistiin ja siirtää ne sitten Azuren tallennustilaan suorittamiseen.

Käytät tätä vaihtoehtoa kopioitaessa tiedostot Blob-objektien tallennustilaan, tallentamista tai -Blob-objektien tallennustilaan tiedostojen tallentamisesta ja päinvastoin.

**Koskevat:** BLOB-tiedostot

### <a name="setcontenttypecontent-type"></a>/ SetContentType: "sisältötyypin"

Määrittää MIME-sisältötyyppi kohde BLOB tai tiedostot.

AzCopy määrittää Blob-objektien tai tiedoston sisältötyyppi sovelluksen/octet-stream oletusarvoisesti. Voit määrittää kaikki BLOB tai tiedostojen sisältötyypin määrittämällä tämän ominaisuuden arvon erikseen.

Jos määrität tämä asetus, jos arvo ei ole, määrittää AzCopy kunkin Blob-objektien tai tiedoston sisältötyyppi sen tiedostotunniste mukaan.

**Koskevat:** BLOB-tiedostot

### <a name="payloadformatjson--csv"></a>/ PayloadFormat: "JSON" | "CSV"

Määrittää taulukossa viedyn datatiedoston.

Jos tämä vaihtoehto ei ole määritetty, oletusarvoisesti AzCopy Vie taulukko datatiedoston JSON-muodossa.

**Koskevat:** Taulukot

## <a name="known-issues-and-best-practices"></a>Tunnetut ongelmat ja parhaat käytännöt

### <a name="limit-concurrent-writes-while-copying-data"></a>Samanaikainen kirjoituksia kopioitaessa tietojen rajoittaminen

Kun kopioit BLOB tai tiedostot, joiden AzCopy, ota huomioon, että toisessa sovelluksessa Muokkaa tietoja samalla, kun kopioit sen. Jos mahdollista Varmista, että haluat kopioida tiedot eivät muokataan kopioinnin aikana. Azure virtuaalikoneen liittyvät Näennäiskiintolevyn kopioitaessa Varmista, muut sovellukset ovat tällä hetkellä kirjoittaminen Näennäiskiintolevyn. Voit tehdä tämän hyvä tapa on ennenaikaisesti resurssin kopioidaan. Vaihtoehtoisesti voit luoda tilannevedoksen Näennäiskiintolevyn ensin ja kopioi sitten tilannevedoksen.

Jos et voi estää kirjoittamasta BLOB tai tiedostot, kun ne on kopioitava muissa sovelluksissa, valitse Ota huomioon, että ajan, työ on valmis, kopioidut resurssit ei enää ehkä koko välistä eroa ja lähde-resurssit.

### <a name="run-one-azcopy-instance-on-one-machine"></a>Suorita AzCopy esiintymä yhdessä koneessa.
AzCopy on suunniteltu Suurenna nopeuttamiseksi tiedonsiirron koneen resurssin käytön, suosittelemme, että suoritat vain yksi AzCopy esiintymä yhdessä koneessa ja määritä haluamasi vaihtoehto `/NC` Jos tarvitset lisää rinnakkaiset toiminnot. Lisätietoja, kirjoita `AzCopy /?:NC` komentorivillä.

### <a name="enable-fips-compliant-md5-algorithms-for-azcopy-when-you-use-fips-compliant-algorithms-for-encryption-hashing-and-signing"></a>FIPS-yhteensopivien MD5 algoritmien käyttöön AzCopy kun olet "Käytä FIPS yhteensopivien algoritmien salauksen hajautuksessa ja allekirjoituksessa".
Oletusarvoisesti AzCopy käyttää .NET MD5 käyttöönoton laskemiseen MD5 kopioitaessa objekteja, mutta on joitakin suojausvaatimukset, joka on AzCopy FIPS-yhteensopiva MD5-asetus käyttöön.

Voit luoda app.config-tiedostoa `AzCopy.exe.config` -ominaisuuden `AzureStorageUseV1MD5` ja sijoita se kesantoala AzCopy.exe kanssa.

    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
      <appSettings>
        <add key="AzureStorageUseV1MD5" value="false"/>
      </appSettings>
    </configuration>

• Ominaisuuden "AzureStorageUseV1MD5" True - oletusarvoa AzCopy käyttävät .NET MD5 käyttöönotto.
• False – AzCopy käyttää FIPS yhteensopiva MD5 algoritmin.

Huomaa, että FIPS-yhteensopivien algoritmien on poissa käytöstä oletusarvoisesti Windows-tietokoneeseen, voit kirjoittaa secpol.msc Suorita-ikkunassa ja tarkista osoitteessa suojausasetus valitsin -> paikallinen käytäntö -> Suojaus-asetukset > Järjestelmä salaus: Käytä FIPS-yhteensopivien algoritmien salauksen hajautuksessa ja allekirjoituksessa.

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja Azure säilytys- ja AzCopy Lisätietoja on seuraavissa resursseissa.

### <a name="azure-storage-documentation"></a>Azure tallennustilan dokumentaatio:

- [Azure-tallennustilan esittely](storage-introduction.md)
- [Opi käyttämään .NET-Blob-objektien tallennustilaan](storage-dotnet-how-to-use-blobs.md)
- [Opi käyttämään .NET-tiedostojen tallennus](storage-dotnet-how-to-use-files.md)
- [Opi käyttämään .NET-taulukkotallennus](storage-dotnet-how-to-use-tables.md)
- [Voit luoda, hallita tai poistaa tallennustilan tilin](storage-create-storage-account.md)

### <a name="azure-storage-blog-posts"></a>Azure tallennustilan blogimerkintöjen:
- [Azure-tallennustilan tietojen siirtämistä kirjaston Preview'n esittely](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/)
- [AzCopy: Synkronoitu kopio ja mukautetun sisältötyypin esittely](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/01/13/azcopy-introducing-synchronous-copy-and-customized-content-type.aspx)
- [AzCopy: Julkaisu Yleiset käytettävyys, AzCopy 3.0 plus AzCopy 4.0 preview-versio ja -tuki](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/10/29/azcopy-announcing-general-availability-of-azcopy-3-0-plus-preview-release-of-azcopy-4-0-with-table-and-file-support.aspx)
- [AzCopy: Optimoitu suurissa kopioi skenaariot](http://go.microsoft.com/fwlink/?LinkId=507682)
- [AzCopy: Lukuoikeudet geo tarpeettomat tallennustilan tuki](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/04/07/azcopy-support-for-read-access-geo-redundant-account.aspx)
- [AzCopy: Siirtää tietoja voi käyttää uudelleen käynnistämiseen tila ja Suojaussidosten tunnus](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/09/07/azcopy-transfer-data-with-re-startable-mode-and-sas-token.aspx)
- [AzCopy: Usean tilin kopioi Blob-objektien käyttäminen](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/04/01/azcopy-using-cross-account-copy-blob.aspx)
- [AzCopy: Azure-BLOB lataamisesta ja ladata tiedostoja](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/12/03/azcopy-uploading-downloading-files-for-windows-azure-blobs.aspx)

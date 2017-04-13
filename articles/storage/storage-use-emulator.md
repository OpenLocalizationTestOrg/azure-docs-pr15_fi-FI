<properties 
    pageTitle="Käytä Azure tallennustilan emulaattori kehittämiseen | Microsoft Azure" 
    description="Azure-tallennustilan emulaattori on ilmainen paikallisen kehitysympäristö kehittä ja testaus vastaan Azure-tallennustilan. Lisätietoja tallennustilan emulaattori, mukaan lukien miten pyynnöt todennetaan, yhdistämisestä emulaattori sovelluksestasi ja työkalun käyttämisestä." 
    services="storage" 
    documentationCenter="" 
    authors="tamram" 
    manager="carmonm" 
    editor="tysonn"/>
<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/07/2016" 
    ms.author="tamram"/>

# <a name="use-the-azure-storage-emulator-for-development-and-testing"></a>Käytä Azure tallennustilan emulaattori kehittäminen ja testaus

## <a name="overview"></a>Yleiskatsaus

Microsoft Azure-tallennustilan emulaattorin on paikallisessa ympäristössä, joka emuloi tarkoitettu Azure-Blob, jonossa ja taulukon palvelut. Käytä tallennustilan emulaattori, voit testata vastaan paikallisesti tallennustilan palveluiden sovelluksen ilman luominen Azure tilauksen tai valinnut kaikki kustannukset. Kun olet tyytyväinen miten sovelluksesi toimii emulaattori, voit siirtyä pilveen Azure-tallennustilan tilin avulla.

> [AZURE.NOTE] Tallennustilan emulointikone on käytettävissä [Microsoft Azure SDK](https://azure.microsoft.com/downloads/)osana. Voit myös asentaa tallennustilan emulaattori [erillisen asennuksen](https://go.microsoft.com/fwlink/?linkid=717179&clcid=0x409)avulla. Tallennustilan emulaattori määrittämiseen tietokoneessa on oltava järjestelmänvalvojan oikeudet.
> 
> Tallennustilan emulaattori toimii tällä hetkellä vain Windows.
>  
> Huomaa, että tallennustilan emulaattori yhden versiossa luodun tiedot eivät ole välttämättä voi käyttää, kun käytät eri versiota. Jos haluat tietojen jatkuvat pitkällä aikavälillä, on suositeltavaa tallentaa tiedot Azure-tallennustilan tilin sijaan tallennustilan emulaattori.

## <a name="how-the-storage-emulator-works"></a>Tallennustilan emulaattori toiminta
 
Tallennustilan emulaattori käyttää paikallisen Microsoft SQL Server-esiintymän ja paikallisen tiedostojärjestelmän voit emuloida Azure liittyviä palveluja. Tallennustilan emulaattori käyttää oletusarvon mukaan tietokannan Microsoft SQL Server 2012 Express LocalDB.  Voit määrittää tallennustilan emulaattori käyttää paikallisen SQL Server-esiintymän LocalDB esiintymän sijaan. Lisätietoja on kohdassa [alkamis- ja alusta tallennustilan emulaattori](#start-and-initialize-the-storage-emulator) alapuolella.

Voit asentaa SQL Server Management Studiossa Express hallittavan LocalDB asennuksen. Tallennustilan emulaattori muodostaa yhteyden SQL Server- tai Windows-todennuksen LocalDB. 

Tallennustilan emulointikoneen ja Azure tallennustilan palveluiden välillä on eroja toimintoja. Saat lisätietoja nämä erot [tallennustilan emulaattorin ja Azure-tallennustilan](#differences-between-the-storage-emulator-and-azure-storage).

## <a name="authenticating-requests-against-the-storage-emulator"></a>Todennustapa tallennustilan emulaattori pyynnöt

Vain samalla tavalla kuin Azuren tallennustilaan pilveen, välein, jotka teet vastaan tallennustilan emulaattori on voi todentaa, ellei se ole anonyymi pyynnön. Voi todentaa pyynnöt vastaan jaetun avaimen todennusta tallennustilan emulaattorin tai jaettuun käyttöön allekirjoituksella (SAS).

### <a name="authentication-with-shared-key-credentials"></a>Jaettu avain tunnistetietojen todentaminen

[AZURE.INCLUDE [storage-emulator-connection-string-include](../../includes/storage-emulator-connection-string-include.md)]

Katso lisätietoja yhteysmerkkijonon [Määrittäminen Azure tallennustilan yhteyden merkkijonoja](storage-configure-connection-string.md). 

### <a name="authentication-with-a-shared-access-signature"></a>Jaettu käyttö allekirjoituksen todentaminen 

Jotkin Azure tallennustilan asiakas-kirjastot, Xamarin kirjastoon, kuten tukevat vain todentaminen jaettu käyttöoikeustietue allekirjoitus (SAS). Sinun on luotava tunnuksessa SAS-työkalua tai sovelluksella, joka tukee jaetun avaimen todennusta. Luo SAS tunnuksen helposti on PowerShellin Azure kautta:

1. Asenna PowerShellin Azure, jos omistajuus on vielä vahvistamatta. On suositeltavaa käyttää Azure PowerShellin cmdlet-komennot uusimman version. Katso, [miten voit asentaa ja määrittää PowerShellin Azure](../powershell-install-configure.md#Install) asennusohjeet.

2. Avaa PowerShellin Azure ja suorittamalla seuraavat komennot. Muista korvata *tilin_nimi* ja *ACCOUNT_KEY ==* oman tunnuksilla. Korvaa *CONTAINER_NAME* sähköpostikansioon nimi.

        $context = New-AzureStorageContext -StorageAccountName "ACCOUNT_NAME" -StorageAccountKey "ACCOUNT_KEY=="
        
        New-AzureStorageContainer CONTAINER_NAME -Permission Off -Context $context
        
        $now = Get-Date 
        
        New-AzureStorageContainerSASToken -Name CONTAINER_NAME -Permission rwdl -ExpiryTime $now.AddDays(1.0) -Context $context -FullUri

Tuloksena oleva jaettu käyttö allekirjoituksen URI uuteen säilöön pitäisi näyttää seuraavankaltaiselta:

    https://storageaccount.blob.core.windows.net/sascontainer?sv=2012-02-12&se=2015-07-08T00%3A12%3A08Z&sr=c&sp=wl&sig=t%2BbzU9%2B7ry4okULN9S0wst%2F8MCUhTjrHyV9rDNLSe8g%3Dsss

Tässä esimerkissä luotu jaettu käyttö allekirjoitus on kelvollinen yhden päivän. Allekirjoituksen antaa täydet oikeudet (eli luku, kirjoita, poista, luettelo) BLOB säilöön.

Lisätietoja jaettujen käytön allekirjoitukset on artikkelissa [Käyttämällä jaetun Access allekirjoitukset (SAS)](storage-dotnet-shared-access-signature-part-1.md).


## <a name="start-and-initialize-the-storage-emulator"></a>Käynnistä ja tallennustilaa emulaattori valmistelu

Käynnistäminen Azure tallennustilan emulaattori, valitse Käynnistä-painiketta tai paina Windows-näppäintä. Aloita kirjoittaminen **Azure-tallennustilan emulaattorin**ja emulaattori sovellusten luettelosta. 

Kun emulaattori on käynnissä, näet kuvakkeen Windowsin tehtäväpalkin ilmaisinalueella.

Tallennustilan emulaattori käynnistyessä komentorivi-ikkuna tulee näkyviin. Voit tämän komentorivin ikkunan avulla voit aloittaa ja lopettaa tallennustilan emulaattori sekä Poista tiedot nykyisen tilan ja emulaattori alusta. Lisätietoja [Tallennustilan emulaattorin komentorivin työkalun Ohje](#storage-emulator-command-line-tool-reference).

Kun komentorivi-ikkuna on suljettu, tallennustilan emulaattori säilyy Suorita. Kun kosketat komentorivin uudelleen edellä olevien ohjeiden ikään tallennustilan emulaattori alkaen.

Tallennustilan-emulaattorin ensimmäisen kerran paikallinen tallennus-ympäristön on alustaminen puolestasi. Alustus luo tietokannan LocalDB ja varaa HTTP-portit kumpaankin palveluun on oma paikallinen tallennus. 

Tallennustilan emulaattori asennetaan oletusarvoisesti C:\Program Files (x86) \Microsoft SDKs\Azure\Storage emulaattorin hakemiston. 

### <a name="initialize-the-storage-emulator-to-use-a-different-sql-database"></a>Alusta tallennustilan emulaattori eri SQL-tietokantaan

Voit valmistella tallennustilan emulaattori osoittamaan SQL-tietokannan esiintymä kuin LocalDB oletusesiintymää tallennustilan emulaattorin komentorivin-työkalun avulla. Huomaa, että käytössä on oltava komentoriviltä alustaminen taustatietokannan tallennustilan emulaattori järjestelmänvalvojan oikeuksin:

1. Napsauta **Käynnistä** -painiketta tai paina **Windows** -näppäintä. Aloita kirjoittaminen `Azure Storage Emulator` ja valitse se ja tuo näkyviin tallennustilan emulaattorin komentorivin työkalun esiintyessään.
2. Komentokehote-ikkunassa, kirjoita seuraava komento, jossa `<SQLServerInstance>` on SQL Server-esiintymän nimi. Jos haluat käyttää LocalDb, Määritä `(localdb)\v11.0` kuin SQL Server-esiintymän.

        AzureStorageEmulator init /server <SQLServerInstance> 
    
    Voit käyttää myös seuraava komento, joka ohjaa käyttämään SQL Server oletusesiintymää emulaattorin:

        AzureStorageEmulator init /server .\\ 

    Tai voit käyttää seuraava komento, joka alustaa uudelleen LocalDB oletusesiintymää tietokannan:

        AzureStorageEmulator init /forceCreate 

Saat lisätietoja näitä komentoja [Tallennustilan emulaattorin komentorivin työkalun Ohje](#storage-emulator-command-line-tool-reference).

## <a name="addressing-resources-in-the-storage-emulator"></a>Tallennustilan emulaattori lähettämisestä resurssit

Tallennustilan emulaattori Palvelupäätepisteet ovat erilaisia kuin Azure-tallennustilan tilin. Ero on se, että paikallisen tietokoneen Suorita toimialuenimen selvitys, joten tallennustilan emulaattorin päätepisteet edellyttävät paikallisen osoitteen toimialuenimi sijaan.

Kun resurssin Azure-tallennustilan tilin osoite, voit käyttää seuraavia malli, jossa tilin nimi on osa URI-isäntänimi ja URI-polku on osoitettu resurssi kuuluu:

    <http|https>://<account-name>.<service-name>.core.windows.net/<resource-path>

Esimerkiksi seuraavat URI on kelvollinen Blob-objektien Azure-tallennustilan tilin osoite:

    https://myaccount.blob.core.windows.net/mycontainer/myblob.txt

Koska paikallisen tietokoneen Suorita toimialuenimen selvitys, tilin nimi on tallennustilan-emulaattorin osa sen sijaan, että isäntänimi URI-polku. Voit käyttää seuraavia värimallin resurssin tallennustilan emulaattori käynnissä:

    http://<local-machine-address>:<port>/<account-name>/<resource-path>

Esimerkiksi seuraava osoite voidaan käyttää blob-tallennustilan emulaattori käyttämiseen:

    http://127.0.0.1:10000/myaccount/mycontainer/myblob.txt

Tallennustilan emulaattori Palvelupäätepisteet ovat seuraavat:

    Blob Service: http://127.0.0.1:10000/<account-name>/<resource-path>
    Queue Service: http://127.0.0.1:10001/<account-name>/<resource-path>
    Table Service: http://127.0.0.1:10002/<account-name>/<resource-path>

### <a name="addressing-the-account-secondary-with-ra-grs"></a>Osoitteiden toissijainen kanssa RA GRS tili

Versio 3.1 alkaen tallennustilan emulaattorin tilin tukee lukuoikeudet geo tarpeettomat replikointia (RA GRS). Tallennustilan resurssien sekä paikallisen emulaattorin pilveen, voit käyttää toissijainen sijainnin mukaan liität - toissijaisen tilin nimi. Esimerkiksi seuraava osoite voidaan käyttää avaaminen vain luku-toissijaisen käyttäminen tallennustilan emulaattori Blob-objektien:

    http://127.0.0.1:10000/myaccount-secondary/mycontainer/myblob.txt 

> [AZURE.NOTE] Toissijaisen ja tallennustilaa emulaattori ohjelmallisesti Käytä tallennustilan asiakkaan kirjaston .NET 3.2 tai uudempi versio. Katso lisätietoja [Microsoft Azure tallennustilan asiakkaan kirjasto .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx) .

## <a name="storage-emulator-command-line-tool-reference"></a>Tallennustilan emulaattorin työkalun viittaus

Aloita 3.0-version, kun käynnistät tallennustilan emulaattori, näet komentorivin ikkunan ponnahdusikkuna. Komentorivin ikkunan avulla voit aloittaa ja lopettaa emulaattori sekä kyselyn tila ja tee muita toimintoja.

> [AZURE.NOTE] Jos sinulla on asennettu emulaattorin Laske Microsoft Azure, järjestelmä-kuvake tulee näkyviin, kun käynnistät tallennustilan emulaattori. Napsauta hiiren kakkospainikkeella valikko, joka sisältää graafisen tapa aloittaa ja lopettaa tallennustilan emulaattori-kuvaketta.

### <a name="command-line-syntax"></a>Komentorivisyntaksi

    AzureStorageEmulator [start] [stop] [status] [clear] [init] [help]

### <a name="options"></a>Asetukset

Voit tarkastella asetusten luettelo kirjoittamalla `/help` komentokehotteen.

| Vaihtoehto | Kuvaus                                                    | Komento                                                                                                 | Argumentit                                                                                                         |
|--------|----------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------|
| **Aloittaminen**  | Tallennustilan emulaattori käynnistyy.                                | `AzureStorageEmulator start [-inprocess]`                                                                    | *-inprocess*: Käynnistä emulaattori nykyisen prosessin sijaan uuden loppuun.                          |
| **Seis**   | Lopettaa tallennustilan emulaattori.                                    | `AzureStorageEmulator stop`                                                                                  |                                                                                                                   |
| **Tila** | Tulostaa tallennustilan emulaattori tila.                     | `AzureStorageEmulator status`                                                                                |                                                                                                                   |
| **Poista**  | Poistaa kaikki palvelut, jotka on määritetty komentorivillä tiedot. | `AzureStorageEmulator clear [blob] [table] [queue] [all]                                                    `| *BLOB*: tyhjentää blob-tietoja. <br/>*jonossa*: tyhjentää jonon tiedot. <br/>*taulukko*: poistaa taulukkotietoja. <br/>*kaikki*: poistaa kaikki tiedot kaikki palvelut. |
| **Alusta**   | Suorittaa erikseen alustus emulaattori määrittäminen.       | `AzureStorageEmulator.exe init [-server serverName] [-sqlinstance instanceName] [-forcecreate] [-inprocess]` | *-palvelimen Palvelimen_nimi\esiintymän_nimi*: määrittää SQL-esiintymää isännöivän palvelimen. <br/>*-sqlinstance EsiintymänNimi*: määrittää käytettäviksi palvelimen oletusesiintymää SQL-esiintymän nimi. <br/>*-forcecreate*: pakottaa SQL-tietokannan luominen, vaikka se on jo olemassa. <br/>*-inprocess*: suorittaa alustus nykyisen prosessin sijaan luodaan uusi prosessia. On käynnistää nykyisen prosessin ja järjestelmänvalvojan oikeudet, jotta voit suorittaa alustus.          |
                                                                                                                  
## <a name="differences-between-the-storage-emulator-and-azure-storage"></a>Tallennustilan emulaattorin ja Azure-tallennustilan väliset erot

Koska tallennustilan emulaattori on emuloitu ympäristössä, joka on käytössä paikallinen SQL-esiintymän, eroja toimintojen emulaattori ja tallennustilaa Azure-tilin pilvipalvelussa:

- Tallennustilan emulaattori tukee vain yhden kiinteä tilin ja tunnettu todennus-näppäintä.

- Tallennustilan emulaattori ei ole skaalattava tallennuspalvelu ja ei tue samanaikaisia asiakkaiden suuri määrä.

- Kuvatulla tavalla [osoitteet resurssien tallennustilan emulaattori](#addressing-resources-in-the-storage-emulator)resurssien käsitellään eri tavalla ja Azure-tallennustilan tilin tallennustilan emulaattori. Tämä on siitä, että toimialuenimen selvitys on käytettävissä pilveen, mutta ei paikalliseen tietokoneeseen.

- Versio 3.1 alkaen tallennustilan emulaattorin tilin tukee lukuoikeudet geo tarpeettomat replikointia (RA GRS). Kaikki tilit on RA GRS käytössä emulaattori, ja ensisijaisen ja toissijaisen replikoiden välillä ei ole koskaan minkä tahansa viive. Hae Blob palvelun Tilasto, Hae jonon palvelun Tilasto ja Hae taulukon palvelun tilasto-toimintoja tuetaan toissijaisen tilin, ja palauttaa aina arvon `LastSyncTime` vastauksen elementti nykyisen kellonajan mukaan pohjana SQL-tietokanta nimellä.

    Toissijaisen ja tallennustilaa emulaattori ohjelmallisesti Käytä tallennustilan asiakkaan kirjaston .NET 3.2 tai uudempi versio. Katso lisätietoja [Microsoft Azure tallennustilan asiakkaan kirjasto .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx) .

- Tiedostopalvelu ja SMB protokolla päätepisteiden ei tällä hetkellä tueta tallennustilan emulaattori.

- Tallennustilan emulaattori palauttaa VersionNotSupportedByEmulator-virheen (HTTP-tilakoodin 400 – pyyntö ei kelpaa), jos liittyviä palveluja versioissa, joka ei vielä tueta käytät emulaattorin versio.

### <a name="differences-for-blob-storage"></a>Erot Blob-objektien tallennustilaan 

Blob-säiliö emulaattori koskevat seuraavat erot:

- Tallennustilan emulaattorin vain tukee blob koko on enintään 2 gt.

- Sijoita Blob-toimintoa välttämättä onnistu vastaan Blob-objektien, joka sijaitsee tallennustilan emulaattori ja on aktiivinen varauksen, vaikka varauksen tunnus ei ole määritetty osana pyynnön. 

- Lisää toimintoja ei tue emulaattori Blob. Liitä Blob-objektien toiminnon yritetään palauttaa FeatureNotSupportedByEmulator-virheen (HTTP-tilakoodin 400 – pyyntö ei kelpaa).

### <a name="differences-for-table-storage"></a>Taulukkotallennus erot 

Taulukon tallennustilan emulaattori koskevat seuraavat erot:

- Päivämäärän ominaisuuksien tallennustilan emulaattori taulukko-palvelussa tukee vain SQL Server 2005: n tukemat alueen (*eli*, ne on oltava viimeistään 1753 1 tammikuussa). Tämä arvo muutetaan kaikki päivämäärät ennen 1753 1 tammikuussa. Päivämäärien tarkkuus on rajoitettu laskentatarkkuuden SQL Server 2005, mikä tarkoittaa, että päivämäärät ovat tarkat 1/300th sekunnin.

- Tallennustilan emulaattori tukee osion avain ja rivin avaimen ominaisuuden arvoja on pienempi kuin 512 tavua. Lisäksi kokonaiskoko tilin nimi, taulukkonimi ja yhdessä avaimen ominaisuuden nimet eivät saa ylittää 900 tavua.

- Tallennustilan emulaattori taulukon rivin kokonaiskoko on rajoitettu pienempi kuin 1 Megatavu.

- Tietojen ominaisuuksista Kirjoita tallennustilan-emulaattorin `Edm.Guid` tai `Edm.Binary` tukee vain `Equal (eq)` ja `NotEqual (ne)` vertailuoperaattorit kyselyn suodattaa merkkijonoja.

### <a name="differences-for-queue-storage"></a>Jonon tallennustilan erot

Tietyn jonon tallennustilan emulaattori ovat eroja.

## <a name="storage-emulator-release-notes"></a>Tallennustilan emulaattorin julkaisutiedot

### <a name="version-45"></a>Versio 4.5

- Kiinteä ohjelmavirhe, jotka aiheuttivat alustus ja tallennustilaa emulaattori epäonnistuu, kun tietokannan varmuuskopioiminen on nimetty uudelleen asentaminen.

### <a name="version-44"></a>Version 4.4

- Tallennustilan emulaattori tukee nyt tallennustilan-palveluiden 2015-12-11 version Blob, jonossa ja taulukon Palvelupäätepisteet.

- Tallennustilan emulaattorin muistista Blob-objektien tiedot on nyt tehokkaampaa tai kuinka paljon BLOB.

- Kiinteä ohjelmavirhe, jotka aiheuttivat säilö Käyttöoikeusluettelon XML tarkistetaan hieman eri tavalla kuin tallennustilan palvelun mistä se.

- Kiinteä, syynä joskus suurin ja pienin päivämääräaika-arvoja, voit raportoida virheellinen aikavyöhykkeen ohjelmavirhe.

### <a name="version-43"></a>Version 4.3

- Tallennustilan emulaattori tukee nyt tallennustilan-palveluiden 2015 07-08 version Blob, jonossa ja taulukon Palvelupäätepisteet.

### <a name="version-42"></a>4.2 versio

- Tallennustilan emulaattori tukee nyt tallennustilan-palveluiden 2015-04-05 version Blob, jonossa ja taulukon Palvelupäätepisteet.

### <a name="version-41"></a>Versio 4.1

- Tallennustilan emulaattori tukee nyt versio 2015 02-21 tallennustilan palvelut-Blob-objektien, jonossa ja taulukon päätepisteiden, lukuun ottamatta liittäminen Blob: n uusiin ominaisuuksiin. 

- Tallennustilan emulaattori palauttaa nyt kuvaava virhesanoman, jos käytät tallennustilan palvelujen versio, joka ei vielä tueta emulaattori versio. On suositeltavaa käyttää emulaattori uusimman version. Jos käytössä ilmenee VersionNotSupportedByEmulator-virheen (HTTP-tilakoodin 400 – pyyntö ei kelpaa), lataa tallennustilan emulaattori uusimman version.

- Kiinteä ohjelmavirhe jossa kilpa ehdon aiheuttaa taulukon kohteen tiedot on virheellinen samanaikainen yhdistämisen aikana.

### <a name="version-40"></a>4. 0:

- Suoritettava tallennustilan emulaattorin on nimetty *AzureStorageEmulator.exe*.

### <a name="version-32"></a>Version 3.2

- Tallennustilan emulaattori tukee nyt tallennustilan-palveluiden 2014-02 – 14 version Blob, jonossa ja taulukon Palvelupäätepisteet. Huomaa, että tiedoston päätepisteiden ei tällä hetkellä tueta tallennustilan emulaattori. Lisätietoja [Versiotietojen Azure-tallennustilan palveluiden](https://msdn.microsoft.com/library/azure/dd894041.aspx) versiota 2014-02 – 14.

### <a name="version-31"></a>Versio 3.1

- Tallennustilan emulaattori tukee yritystietopalveluja lukuoikeudet geo tarpeettomat storage (RA GRS). Hae Blob palvelun Tilasto, Hae jonon palvelun Tilasto ja Hae taulukon palvelun tilasto ohjelmointirajapinnan tuetaan toissijaisen tilin ja aina palauttaa arvon LastSyncTime vastauksen elementin nykyisen kellonajan mukaan pohjana SQL-tietokanta nimellä. Toissijaisen ja tallennustilaa emulaattori ohjelmallisesti Käytä tallennustilan asiakkaan kirjaston .NET 3.2 tai uudempi versio. Lisätietoja Microsoft Azure tallennustilan asiakkaan kirjaston .NET käyttöä.

### <a name="version-30"></a>3.0-versiota

- Azure-tallennustilan emulaattori toimitetaan enää samaan pakettiin kuin Laske emulaattori.

- Tallennustilan emulaattoriliitännän graafisen on merkitystä, koska scriptable rivin komentoliittymä. Lisätietoja komentoliittymä rivi on artikkelissa tallennustilan emulaattorin komentorivin työkalun Ohje. Graafisessa käyttöliittymässä on edelleen olemassa 3.0-versiota, mutta sitä voi käyttää vain Laske emulaattori on asennettu järjestelmän ilmaisinalueen kuvaketta hiiren kakkospainikkeella ja valitsemalla Näytä tallennustilan emulaattorin UI.

- Versiossa 2013-08-15 Azure tallennustilan palveluja nyt tuetaan täysin. (Aiemmin tässä versiossa on vain tukee tallennustilan emulaattorin versio 2.2.1 esikatselu.)

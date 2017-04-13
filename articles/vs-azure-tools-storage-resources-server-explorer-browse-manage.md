<properties
   pageTitle="Selaaminen ja palvelimen Resurssienhallinnassa resurssien tallennustilan hallinta | Microsoft Azure"
   description="Selaaminen ja palvelimen Resurssienhallinnassa resurssien tallennustilan hallinta"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="storage"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/18/2016"
   ms.author="tarcher" />

# <a name="browsing-and-managing-storage-resources-with-server-explorer"></a>Selaaminen ja palvelimen Resurssienhallinnassa resurssien tallennustilan hallinta

[AZURE.INCLUDE [storage-try-azure-tools](../includes/storage-try-azure-tools.md)]

## <a name="overview"></a>Yleiskatsaus
Jos olet asentanut Microsoft Visual Studio Azure-Työkalut, voit tarkastella blob jonossa ja taulukkotiedot tallennustilan tileistä Azure. Palvelimen Explorerissa Azuren tallennustilaan-solmu näyttää tiedot, jotka ovat paikallisen säilön emulaattorin tilisi ja muista tileistä Azure-tallennustilan.

Voit tarkastella palvelimen Explorer Visual Studiossa valikkorivillä valitsemalla **Näytä**- **Palvelimen Explorer**. Tallennustilan-solmu näyttää kaikki tallennustilan tilit, jotka ovat olemassa, valitse kunkin Azure tilauksen/varmenne on muodostettu. Jos tallennustilaa tilisi ei näy, voit lisätä sen seuraamalla ohjeita [tämän artikkelin](#add-storage-accounts-by-using-server-explorer).

Aloita Azure SDK 2.7, voit käyttää myös uusi Cloud Explorer voit tarkastella ja hallita Azure resursseja. Lisätietoja on kohdassa [Hallinta Azure resurssien Cloud Resurssienhallinnassa](./vs-azure-tools-resources-managing-with-cloud-explorer.md) .


## <a name="view-and-manage-storage-resources-in-visual-studio"></a>Tarkastella ja hallita tallennustilan resursseja Visual Studiossa

Palvelimen Explorer näyttää automaattisesti BLOB-objektit ja olevien taulukoiden luettelon tallennustilan emulaattorin tilissäsi. Tallennustilan emulaattorin tili näkyy palvelimen Explorer tallennustilan solmun **kehittäminen** solmu.

Voit tarkastella tallennustilan emulaattorin tilin resurssit, laajenna **Development** -solmu. Jos tallennustilaa emulaattori ei ole aloitettu, kun laajennat **Development** -solmu, se käynnistyy automaattisesti. Tämä voi kestää joitakin sekunteja. Voit jatkaa muualla Visual Studio työskentelyä tallennustilan emulaattori käynnistyy.

Tallennustilan tilin resurssien tarkastelemiseen Laajenna tallennustilan tilin solmu palvelimen Resurssienhallinnassa. Seuraavat vaihtoehdot solmut näkyvät:

- BLOB-objektit

- Olevien

- Taulukot

## <a name="work-with-blob-resources"></a>Blob-objektien resurssien käsitteleminen

BLOB-solmu näyttää valitun tallennustilan tilin säilöjen luettelo. Blob-objektien säilöt sisältävät blob-tiedostot ja voit järjestää nämä BLOB kansiot ja alikansiot. Lisätietoja on kohdassa [käyttämisestä .NET-Blob-objektien tallennustilaan](./storage/storage-dotnet-how-to-use-blobs.md) .

### <a name="to-create-a-blob-container"></a>Voit luoda blob-säilö

1. Avaa pikavalikon **BLOB** -solmu ja valitse sitten **Luo Blob-säilö**.

1. Kirjoita uusi säilö nimi **Luo Blob-säilö** -valintaikkunassa ja valitse sitten **Ok**

    >[AZURE.NOTE] Blob-säilö nimen alussa on oltava numero (0-9) tai pieni kirjain (-ö).

### <a name="to-delete-a-blob-container"></a>Jos haluat poistaa blob-säilö

- Avaa pikavalikon blob-säilö haluat poistaa, ja valitse sitten **Poista**.

### <a name="to-display-a-list-of-the-items-contained-in-a-blob-container"></a>Saat näkyviin luettelon kohteet blob-säilö

- Avaa pikavalikon blob-säilö nimi luettelosta ja valitse sitten **Näytä Blob-säilö**.

    Kun tarkastelet blob-säilö sisältöä, se näkyy nimeltään blob-säilö Näytä-välilehdessä.

    ![VST_SE_BlobDesigner](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC749016.png)

    Voit suorittaa seuraavat toimenpiteet BLOB blob-säilö näkymän oikeassa yläkulmassa painikkeiden avulla:

    - Kirjoita Suodatinarvo ja käyttäminen

    - Päivitä BLOB-säilö

    - Tiedoston lataaminen

    - Poista blob

      >[AZURE.NOTE] Tiedoston poistaminen blob-säilö ei poista taustalla tiedostoa. vain poistetaan sen blob-säilö.

    - Avaa blob

    - Tallenna blob paikalliseen tietokoneeseen

### <a name="to-create-a-folder-or-subfolder-in-a-blob-container"></a>Voit luoda kansion tai alikansion blob-säilö

1. Valitse palvelimen Explorer blob-säilö. Säilön-ikkunassa voit **Ladata Blob** -painiketta.

    ![Tiedoston lataaminen blob-kansioon](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766037.png)

1. Valitse **Lataa uusi tiedosto** -valintaikkunassa **Selaa ja määritä ladattava tiedosto** ja kirjoita **kansio (valinnainen)** -ruutuun kansionimi.

    Voit lisätä alikansioita säilö kansioissa noudattamalla seuraavia ohjeita. Jos et määritä kansionimen, tiedosto ladataan blob-säilö ylimmällä tasolla. Tiedosto näkyy säilössä määritettyyn kansioon.

    ![Kansion lisätään blob-säilö](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766038.png)

1. Kaksoisnapsauta kansiota, tai paina ENTER-kansion sisällön näkyviin. Kun olet säilö-kansiossa, voit siirtyä takaisin säilö ylimmällä valitsemalla **Avaa Yläkansiota** (Ylänuoli)-painiketta.

### <a name="to-delete-a-container-folder"></a>Poista säilö-kansio

 - Poistaa kaikki tiedostot-kansiossa

    >[AZURE.NOTE] Koska kansioiden blob säilöt ovat Näennäiskansiot, et voi luoda tyhjän kansion eikä voit poistaa kansion, voit poistaa sen tiedoston sisällön. Sinun tulee poistaa kansion poistaa kansion koko sisällön.

### <a name="to-filter-blobs-in-a-container"></a>Jos haluat suodattaa BLOB-säilö

Voit suodattaa BLOB-objektit, jotka näkyvät määrittämällä yleisiä etuliite.

Jos kirjoitat etuliite esimerkiksi `hello` suodattimen tekstiä ruutuun ja valitse sitten **Suorita** (****!) painike, vain BLOB, joiden alussa on "Hei" tule näkyviin.

![VST_SE_FilterBlobs](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC519076.png)


>[AZURE.NOTE] Suodatinkentän kirjainkoko on merkitsevä ja ei tue suodattaminen yleismerkkien kanssa. BLOB voi suodattaa vain etuliitteen mukaan. Etuliitteen voi sisältyä erottimen, jos käytössäsi on erottimen BLOB virtual hierarkian järjestämiseen. Esimerkiksi suodattaminen etuliite HelloFabric / palauttaa kaikki Blob-objektien merkkijono alkaa.

### <a name="to-download-blob-data"></a>Voit ladata blob-tietoja

- **Palvelimen Explorer**vähintään yksi BLOB pikakuvake-valikon avaaminen ja sitten **Avaa**, valitse Blob-objektien nimi ja sitten **Avaa** -painikkeen valitseminen tai kaksoisnapsauta blob-nimeä.

    **Azure tehtävän Log** -ikkunassa näkyy blob latauksen etenemistä.

    Blob Avaa tiedostotyypin oletusarvo-editorissa. Jos järjestelmä tunnistaa tiedostotyyppi, tiedosto avautuu paikallisesti asennettu sovelluksessa. Muussa tapauksessa sinua pyydetään valitsemaan sovellus, joka sopii blob tiedostotyyppi. Paikallinen tiedosto, joka on luotu, kun lataat blob on merkitty vain luku-muotoiseksi.

    Blob-objektien tiedot paikallisesti ja verrataan Blob-objektien edellisen muokkauksen ajankohdan Blob-palvelussa. Jos blob on päivitetty, koska se on viimeksi ladattu, se ladataan uudelleen. Muussa tapauksessa blob ladataan paikallisen levyltä. Oletusarvoisesti blob ladataan tilapäiskansion. Voit ladata BLOB tiettyyn kansioon, Avaa pikavalikon valitun Blob-objektien nimet ja valitse **Tallenna nimellä**. Kun tallennat blob tällä tavalla, blob-tiedostoa ei ole avattu ja paikallinen tiedosto luodaan luku-ja kirjoitusoikeudet ominaisuuksilla.

### <a name="to-upload-blobs"></a>Voit ladata BLOB-objektit

- Voit **Ladata Blob** -painiketta, kun säilö on avoinna blob-säilö Näytä tarkastelua varten.

    Voit valita yhden tai useamman tiedostoja ladataan ja voit ladata tiedostoja. Lataus edistyminen näkyy **Azure tehtävän loki** . Lisätietoja siitä, miten Blob-objektien käyttäminen Katso, [miten .NET Azure Blob Storage-palvelun käyttöä varten](http://go.microsoft.com/fwlink/p/?LinkId=267911).

### <a name="to-view-logs-transferred-to-blobs"></a>Voit tarkastella lokit siirretään BLOB-objektit

- Jos käytät Azure diagnostiikka Kirjaudu tietojen Azure-sovelluksesta ja olet siirtänyt lokit tallennustilan-tilisi, näet säilöt, jotka on luotu Azure lokitiedostot. On helppo tunnistaa sovelluksen ongelmien tarkasteleminen lokitiedostot palvelimen Explorerissa on, etenkin silloin, kun se on otettu käyttöön Azure. Saat lisätietoja Azure Diagnostiikka [Kirjaaminen tietojen kerääminen Azure Diagnostiikan avulla](https://msdn.microsoft.com/library/azure/gg433048.aspx).

### <a name="to-get-the-url-for-a-blob"></a>Jos haluat blob URL-Osoitteen hankkiminen

- Avaa Blob-objektien hiiren kakkospainikkeella ja valitse sitten **Kopioi URL-osoite**.

### <a name="to-edit-a-blob"></a>Jos haluat muokata blob

- Valitse blob ja valitse sitten **Avaa Blob** -painiketta.

    Tiedosto ladataan tilapäinen sijaintiin ja avata paikalliseen tietokoneeseen. Blob täytyy ladata uudelleen, kun olet tehnyt muutokset.

## <a name="work-with-queue-resources"></a>Jonon resurssien käsitteleminen

Tallennustilan services olevien isännöidään Azure-tallennustilan tilin ja niiden avulla voidaan Salli oman cloud palvelun roolien kommunikoida kulkeva järjestelmä viestin keskenään ja muiden palvelujen kanssa. Voit käyttää jonossa ohjelmallisesti pilvipalveluun ja ulkoisia asiakkaita WWW-palvelussa. Voit käyttää jonossa suoraan myös Visual Studiossa palvelimen Resurssienhallinnan avulla.

Kun kehität pilvipalvelussa, joka käyttää olevien, haluat ehkä Visual Studiolla voit luoda olevien ja käyttää niitä vuorovaikutteisesti samalla, kun kehittää ja testata koodisi.

Palvelimen Resurssienhallinnassa voit voit tarkastella, tallennustilan tilillä, luominen ja poistaminen olevien, avaa sen viestien lukemista varten jonon ja viestien lisääminen jonon. Avattaessa jonon tarkastelua voit tarkastella yksittäisten viestien ja voit suorittaa seuraavat toiminnot jonossa vasemmassa yläkulmassa painikkeiden avulla:

- Päivittää näkymän jonon

- Lisää viestiin jonossa

- Jonosta poistamisen ylin viestin.

- Poista koko jonossa

Seuraava kuva esittää jono, joka sisältää kaksi viestejä.

![Jonon tarkasteleminen](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC651470.png)

Lisätietoja tallennustilan palvelut olevien, lue [Toimintaohje: Käytä jonon Tallennuspalvelu](http://go.microsoft.com/fwlink/?LinkID=264702). Lisätietoja tallennustilan services olevien web-palvelu on ohjeaiheessa [Jonon palvelun käsitteitä](http://go.microsoft.com/fwlink/?LinkId=264788). Tietoja lähettää viestejä tallennustilan services jonon Visual Studion avulla on artikkelissa [Tallennustilan Services jonon lähettää viestejä](https://msdn.microsoft.com/library/azure/jj649344.aspx).

>[AZURE.NOTE] Tallennustilan services olevien poikkeavat palvelun bus olevien. Saat lisätietoja palvelun bus olevien palvelun Bus olevien, aiheet ja tilaukset.

## <a name="work-with-table-resources"></a>Taulukon resurssien käsitteleminen

Azure-taulukosta tallennuspalvelu tallentaa rakenteellisia suurista tietomääristä. Palvelu on NoSQL-datastore joka hyväksyy todennettu kutsujen ja Azure pilveen sivuihin. Azure-taulukoiden soveltuvat erinomaisesti rakenteellisia ja relaatio tietojen tallentaminen.

### <a name="to-create-a-table"></a>Taulukon luominen

1. Palvelimen Resurssienhallinnassa tallennustilan tilin **taulukot** -solmu ja valitse sitten **Luo taulukko**.

1. Kirjoita **Luo taulukko** -valintaikkunassa taulukon nimi.

### <a name="to-view-table-data"></a>Voit tarkastella taulukkotietoja

1. Palvelimen Resurssienhallinnassa Avaa **Azure** -solmu ja sitten **tallennustilan** -solmu.

1. Avaa tallennustilan tilin solmu, jotka ovat kiinnostuneita ja avaa sitten **taulukot** -solmun näet taulukkoluettelon tallennustilan tilin.

1. Avaa taulukon pikavalikon ja valitse sitten **Näytä taulukko**.

    ![Azuren taulukkojen, napsauta ratkaisunhallinnassa](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC744165.png)

Taulukko on järjestetty kohteiden (kuten rivejä) ja ominaisuudet (kuten sarakkeet). Seuraavassa kuvassa näkyy esimerkiksi kohteiden **Taulukon suunnitteluohjelman**luettelossa:

### <a name="to-edit-table-data"></a>Taulukkotietojen muokkaaminen

1. **Taulukon suunnitteluohjelman**Avaa pikavalikko kohteen (yksirivinen) tai ominaisuuden (solu) ja valitse sitten **Muokkaa**.

    ![Lisää tai Muokkaa taulukon kohde](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC656238.png)

    Yhden taulukon kohteet eivät ole pakollisia on samat ominaisuudet (sarakkeet). Ota huomioon seuraavat rajoitukset tarkasteleminen ja muokkaaminen taulukkotiedot.
    - Et voi tarkastella tai muokata binaaritietoja (tyyppi byte[]), mutta voi tallentaa taulukon.

    - Et voi muokata **PartitionKey** tai **RowKey** -arvoja, koska Azure-taulukkotallennus ei tue tätä toimintoa.

    - Et voi luoda aikaleima-ominaisuuden, Azuren tallennustilaan palveluiden käyttäminen ominaisuuden nimi.

    - Jos kirjoitat DateTime-arvo, sinun on noudatettava muoto on sopivan tietokoneen alue ja kieli-asetukset (esimerkiksi KK/PP/VVVV HH [AM | PM] Yhdysvaltain englannin kielen).

### <a name="to-add-entities"></a>Voit lisätä kohteita

1. **Taulukon suunnitteluohjelman**Valitse **Lisää kohde** -painiketta, joka on lähellä taulukkonäkymän oikeassa yläkulmassa.

    ![Lisää kohde](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC655336.png)

1. Kirjoita **Lisää kohde** -valintaikkunassa **PartitionKey** ja **RowKey** ominaisuuksien arvot.

    ![Lisää kohde-valintaikkuna](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC655335.png)

    Kirjoita haluamasi arvot huolellisesti, koska niitä ei voi muuttaa, kun suljet valintaikkunan, ellet poista kohde ja lisää se sitten uudelleen.

### <a name="to-filter-entities"></a>Kohteiden suodattaminen

Voit mukauttaa kohteet, jotka näkyvät taulukossa, jos käytät kyselyn muodostimessa.

1. Avaa kyselyn muodostin, Avaa taulukko tarkastelua varten.

1. Valitse taulukkonäkymän työkalurivin oikeanpuoleisimman-painiketta.

    **Kyselyn muodostin** -valintaikkuna. Seuraavassa kuvassa kysely, joka on käännetty kyselyn muodostimessa.

    ![Kyselyn muodostin](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC652231.png)

1. Kun olet valmis luominen kyselyn ja sulje sitten valintaikkuna. Kyselyn tuloksena teksti-muodossa näkyy tekstiruudun WCF-tietopalvelut suodattimena.

1. Jos haluat suorittaa kyselyn, valitse vihreä kolmio-kuvake.

    Voit myös suodattaa kohteen tiedot, joka tulee näkyviin **Taulukon suunnitteluohjelman** , jos kirjoitat WCF-tietopalvelut suodattimen merkkijonon Suodatin-kenttään. Merkkijono tällaista vastaa SQL WHERE-lauseen, mutta HTTP-pyynnön palvelimeen lähetetään. Lisätietoja suodattimen merkkijonojen muodostamisesta on kohdassa [Taulukon suunnitteluohjelman suodattimen merkkijonot luomisesta](https://msdn.microsoft.com/library/azure/ff683669.aspx).

    Seuraavassa kuvassa on esimerkki kelvollinen Suodatinmerkkijonon:

    ![VST_SE_TableFilter](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC655337.png)

### <a name="refresh-storage-data"></a>Tallennustilan tietojen päivittäminen

Kun palvelimen Explorer muodostaa tai hakee tallennustilan tilin tiedot, voi kestää toiminto on valmis minuuteiksi. Jos se ei voi muodostaa, toiminto saattaa kadota itsestään. Kun tiedot on noudettu, voit jatkaa toimimaan Visual Studio muihin osiin. Jos haluat peruuttaa toiminnon, jos kestää liian kauan, valitse palvelimen Explorerin työkalurivillä **Lopeta Päivitä** -painike.

#### <a name="to-refresh-blob-container-data"></a>Blob-säilö tietojen päivittäminen

- Valitse alapuolella **tallennustilan** **BLOB** -solmu ja valitse palvelin Explorerin työkalurivin **Päivitä** -painiketta.

- Päivittää BLOB luettelo, joka tulee näkyviin, valitse **Suorita** -painiketta.

#### <a name="to-refresh-table-data"></a>Taulukkotietojen päivittäminen

- Valitse **taulukot** -solmu **tallennustilan** alapuolella ja valitse **Päivitä** -painiketta.

- Päivitä kohteiden luettelo, joka tulee näkyviin **Taulukon suunnitteluohjelman**, valitse **Taulukon suunnitteluohjelman** **Suorita** -painiketta.

#### <a name="to-refresh-queue-data"></a>Jonon tietojen päivittäminen

- Valitse **olevien** -solmu ja valitse sitten **Päivitä** -painiketta.

#### <a name="to-refresh-all-items-in-a-storage-account"></a>Päivitä kaikki kohteet-tallennustilan tilin

- Valitse tilin nimi ja valitse palvelin Explorerin työkalurivin **Päivitä** -painiketta.

### <a name="add-storage-accounts-by-using-server-explorer"></a>Lisää tallennustilaa tilejä palvelimen Resurssienhallinnan avulla

Voit lisätä tallennustilaa tilit palvelimen Resurssienhallinnan avulla kahdella eri tavalla. Voit luoda uuden tallennustilan tilin Azure-tilaukseesi, tai voit lisätä käytössä olevan tallennustilan tilin.

#### <a name="to-create-a-new-storage-account-by-using-server-explorer"></a>Voit luoda uuden tallennustilan tilin palvelimen Resurssienhallinnassa

1. Palvelimen Resurssienhallinnassa Avaa pikavalikko tallennustilan solmun ja valitse sitten Luo tallennustilan tili.

    ![Luo uusi tallennustilan Azure-tili](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC744166.png)

1. Valitse tai anna seuraavat tiedot tallennustilan uuden tilin **Luominen tallennustilan tili** -valintaikkunassa.

    - Azure tilaus, johon haluat lisätä tallennustilaa-tili.

    - Haluat käyttää uuden tallennustilan tilin nimi.

    - Alueen tai affiniteetti ryhmän (esimerkiksi Länsi US tai Itä-Aasian).

    - Replikoinnin tyyppi, jota haluat käyttää tallennustilan tilin, kuten Geo tarpeettomat.

1. Valitse **Luo**.

    Uusi tallennustilan tili näkyy ratkaisunhallinnassa **tallennustilan** -luettelossa.

#### <a name="to-attach-an-existing-storage-account-by-using-server-explorer"></a>Voit liittää käytössä olevan tallennustilan-tilin palvelimen Resurssienhallinnan avulla

1. Palvelimen Resurssienhallinnassa Avaa pikavalikko Azure tallennustilan solmun ja valitse sitten **Liitä ulkoisen tallennustilan**.

    ![Käytössä olevan tallennustilan tilin lisääminen](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766039.png)

1. Valitse tai anna seuraavat tiedot tallennustilan uuden tilin **Luominen tallennustilan tili** -valintaikkunassa.

    - Olemassa olevan tallennustilan tilin nimi, jonka haluat liittää. Voit kirjoittaa nimeä tai valitse se luettelosta.

    - Valitun tallennustilan tilin avain. Tämä arvo annetaan yleensä puolestasi, kun valitset tallennustilan tilin. Jos haluat Visual Studio muistaa tallennustilan tilin-näppäintä, valitse muista tili valinta.

    - Protokolla, voit muodostaa yhteyden tallennustilan tilin, kuten HTTP, HTTPS tai mukautetun päätepiste. Katso, [miten voit määrittää yhteyden merkkijonot](https://msdn.microsoft.com/library/azure/ee758697.aspx) Lisätietoja mukautetun päätepisteet.

### <a name="to-view-the-secondary-endpoints"></a>Voit tarkastella toissijaisen päätepisteet

- Jos olet luonut tallennustilan tilillä **Lukuoikeudet Geo tarpeettomat** replikoinnin-vaihtoehdon, näet sen toissijainen päätepisteet. Avaa pikavalikon tilin nimi ja valitse sitten **Ominaisuudet**.

    ![Tallennustilan toissijainen päätepisteet](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766040.png)

### <a name="to-remove-a-storage-account-from-server-explorer"></a>Jos haluat poistaa tallennustilan tilin Server Explorerista

- Palvelimen Resurssienhallinnassa Avaa pikavalikon tilin nimi ja valitse sitten **Poista**. Jos poistat tallennustilan-tili, minkä tahansa tallennetun avaintiedoista tilin poistetaan.

    >[AZURE.NOTE] Jos poistat tallennustilan tilin Server Explorerista, sillä ole vaikutusta tallennustilan tilisi tai mitä tahansa sen sisältämiä; viittaus poistetaan vain palvelin Explorer. Jos haluat poistaa pysyvästi tallennustilan tilin, käytä [Azure perinteinen portal](http://go.microsoft.com/fwlink/?LinkID=213885).

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja lisätietoa siitä, miten käyttää Azure liittyviä palveluja on artikkelissa [Azure-tallennustilan palvelujen](https://msdn.microsoft.com/library/azure/ee405490.aspx).

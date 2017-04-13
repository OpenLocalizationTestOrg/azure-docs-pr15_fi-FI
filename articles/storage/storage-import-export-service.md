<properties
    pageTitle="Tuo tai Vie avulla voit siirtää tietoja Blob-objektien tallennustilaan | Microsoft Azure"
    description="Lue, miten voit luoda Tuo ja vie työt Azure perinteinen portaalin siirtää tiedot blob storage."
    authors="renashahmsft"
    manager="aungoo"
    editor="tysonn"
    services="storage"
    documentationCenter=""/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="renash"/>


# <a name="use-the-microsoft-azure-importexport-service-to-transfer-data-to-blob-storage"></a>Microsoft Azure Tuo/Vie-palvelun avulla voit siirtää tietoja Blob-objektien tallennustilaan

## <a name="overview"></a>Yleiskatsaus

Azure Tuo/Vie-palvelun avulla voit siirtää suojatusti suuria tietomääriä Azure-blob-säiliö toimitus kiintolevyaseman levyasemien Azure tietojen käyttöoikeudet. Voit käyttää myös tämän palvelun tietojen siirtämistä kiintolevyaseman levyasemien Azure-blob-säiliö ja lähettäminen paikallisen sivustoon. Tämä palvelu on sopiva tilanteissa, johon haluat siirtää tietoja tai azuren useita TBs, mutta lataus kokonaan verkossa ei ole mahdollista vuoksi rajoitettu kaistanleveys tai suuri verkon kustannukset.

Palvelu edellyttää, että kiintolevyaseman levyasemien pitäisi olla bit locker salattu tietojen suojauksen. Palvelun tukee perinteinen tallennustilan tilejä julkisen Azure kaikilla alueilla. Sinun on toimitettava kiintolevyaseman levyasemien johonkin tämän artikkelin määritetty tuetut sijainneista.

Tässä artikkelissa opit lisätietoja Azure Tuo/Vie-palvelusta ja lähettää asemista tietojen kopioiminen ja sieltä pois Azure-Blob-säiliö.

> [AZURE.IMPORTANT] Voit luoda ja hallita Tuo ja vie perinteinen tallennustilan Classic-portaalin tai [Tuo/Vie palvelun REST API](http://go.microsoft.com/fwlink/?LinkID=329099)työt. Resurssienhallinta tallennustilan tilit ei tueta tällä hetkellä.

## <a name="when-should-i-use-the-azure-importexport-service"></a>Kun Azure Tuo/Vie-palvelun kannattaa käyttää?

Harkitse Azure Tuo/Vie-palvelun avulla ladattaessa latauksen verkossa on liian hidasta tai muita verkon kaistanleveyden käytön on kieltoja kustannus.

Käytä tämän palvelun skenaariot seuraavasti:

- Tietojen siirtämisestä pilveen: suuria tietomääriä Siirry Azure nopeasti ja tehokkaasti kustannukset.
- Sisällön jakelu: tietojen lähettäminen asiakkaalle sivustojen nopeasti.
- Varmuuskopiointi: Kestää varmuuskopioiden paikallisen tietojen tallentamiseen Azure Blob-objektien tallennustilaan.
- Tietojen palauttaminen: palauttaa paljon Blob-objektien tallennustilaan tallennettuja tietoja ja sen toimitettu paikallisen sijaintiin.

## <a name="pre-requisites"></a>Vaatimukset

Tässä osassa on on lueteltu tämän palvelun käyttäminen edellyttää vaatimat. Tarkista ne huolellisesti ennen toimittamista asemat.

### <a name="storage-account"></a>Tallennustilan tilin

Sinulla on oltava olemassa olevaan tilaukseen Azure ja vähintään yhden **perinteinen** tallennustilan tilit Tuo/Vie-palvelun käyttöä varten. Kunkin projektin voidaan siirtää tietoja tai vain yhden perinteinen tallennustilan-tililtä. Toisin sanoen yhden Tuo/Vie työn et voi kattavat tallennustilan useiden eri tilien välillä. Saat lisätietoja tallennustilan uuden tilin luominen Katso, [miten tallennustilan tilin luominen](storage-create-storage-account.md#create-a-storage-account).

### <a name="blob-types"></a>Blob-objektien tyypit

Voit kopioida tietoja **Estä** BLOB tai **sivun** BLOB Azure Tuo/Vie-palvelun avulla. Vastaavasti voit viedä vain **Estä** BLOB-objektit, **sivun** BLOB tai **Liitä** BLOB Azure säilöstä tämän palvelun avulla.

### <a name="job"></a>Työn

Aloita tietojen tuominen tai vieminen Blob-objektien tallennustilaan, luot projektin. Työn voi olla tuontityö- tai vientityö:

- Luo tuontityö, kun haluat siirtää paikalliseen BLOB-objektit, ovat Azure-tallennustilan tilin tiedot.
- Luo vientityö, kun haluat siirtää tietoja tällä hetkellä tallennettujen BLOB storage-tilisi kiintolevyillä, jotka on lähetetty sinulle.

Kun luot projektin, voit ilmoittaa Tuo/Vie-palvelu, että voit olla toimitus vähintään yksi kiintolevyillä Azure tietojen käyttöoikeudet.

- Tuo-työn sinun toimitus tietoja sisältävä kiintolevyillä.
- Vie-työn sinun toimitus tyhjä kiintolevyillä.
- Voit lähettää enintään 10 kiintolevyaseman levyasemien projektia kohti.

Voit luoda tuonti tai viedä työn [perinteinen portal](https://manage.windowsazure.com/) tai [Azure tallennustilan Tuo/Vie REST API](http://go.microsoft.com/fwlink/?LinkID=329099).

### <a name="client-tool"></a>Asiakas-työkalu

**Tuo** projektin luominen ensimmäiseksi on levyasemasta, jotka toimitetaan valmistelu tuontia varten. Valmistele asema, muodosta yhteys paikalliseen palvelimeen ja Azure Tuo/Vie asiakas-työkalun suorittaminen paikallisessa palvelimessa. Asiakkaan tämä työkalu helpottaa tietojen kopioiminen asema, BitLockerilla asemassa tiedot salataan ja luodaan asema Päivyri-tiedostoja.

Päivyrin tiedostot tallentaa perustietoja työn ja asema, kuten asema sarjanumero ja tallennustilan tilin nimi. Päivyrin tämä tiedosto ei ole tallennettu asemaan. Tuo projektin luonnin aikana käyttää sitä. Vaihe vaiheelta työpaikkojen tietoja toimitetaan tämän artikkelin.

Asiakasohjelma on vain yhteensopiva 64-bittinen Windows-käyttöjärjestelmässä. [Käyttöjärjestelmä](#operating-system) on kohdassa tietyn OS versioiden tukemat.

Lataa [Azure Tuo/Vie-asiakasohjelma](http://go.microsoft.com/fwlink/?LinkID=301900&clcid=0x409)uusimman version. Lisätietoja Azure Tuo/Vie-työkalulla Hae [Azure Tuo/Vie työkalun viittaus](http://go.microsoft.com/fwlink/?LinkId=329032).

### <a name="hard-disk-drives"></a>Kiintolevyn levyasemien

3,5 tuuman SATA III tai II sisäinen kiintolevyillä tuetaan Tuo/Vie-palvelun kanssa käytettäväksi. Voit käyttää kiintolevyillä enintään 10 Teratavua.
Tuo töiden vain ensimmäinen tietojen äänenvoimakkuuden asemassa käsitellään. Tietoja äänenvoimakkuuden on muotoiltu NTFS.
Kun kopioit tietoja kiintolevylle, voit liittää sen suoraan SATA yhdistin tai voit liittää sen ulkoisesti ulkoisen SATA II/III USB-sovittimen. Suosittelemme, että Käytä jotakin seuraavista ulkoisen SATA II/III USB-sovittimia:

- Anker 68UPSATAA - 02BU
- Anker 68UPSHHDS-BU
- Startech SATADOCK22UE
- Sharkoon QuickPort XT HC

Jos sinulla on muuntimen, joka ei ole luettelossa, voit kokeilla Azure Tuo/Vie-työkalun avulla oman muuntimen valmisteleminen asema ja nähdä, jos se toimii ennen hankkimista tuettu tiedostomuunninta.

> [AZURE.IMPORTANT] Tämä palvelu ei tue ulkoisia kiintolevyaseman levyasemien valmiin USB-sovittimen mukana toimitettavista. Lisäksi levyn sisällä ulkoinen Kiintolevy kirjainkoko ei voi käyttää; Älä lähetä ulkoisen HDDs.

### <a name="encryption"></a>Salaus

Asemassa tiedot on salattu BitLocker-salaustoiminto avulla. Tämä suojaa tietosi siirron aikana.

Tuo töiden kahdella suorittamiseen salaus. Ensimmäinen tapa on käyttää / salata parametri, kun asiakas-työkalun suorittaminen asema valmistelun yhteydessä. Toinen tapa on BitLocker-salausta manuaalisesti asemassa ottaminen käyttöön ja määrittää salausavaimen asiakkaan työkalun komentorivillä asema valmistelun yhteydessä.

Vie töiden, kun tiedot on kopioitu asemat-palvelun salata BitLockerin ennen kuin lähetät sen takaisin asemaa. Salausavaimen toimitetaan Classic-portaalin kautta.  

### <a name="operating-system"></a>Käyttöjärjestelmä

Valmistele Azure Tuo/Vie-työkalun avulla ennen toimittamista Azure aseman kiintolevyä jollakin 64-bittisten käyttöjärjestelmien:

Windows 7: n Enterprise-, Windows 7 Ultimate Windows 8 Pro, yrityksen Windows 8 Windows 8.1 Pro, yrityksen Windows 8.1, Windows 10<sup>1</sup>, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2: n. Kaikki nämä käyttöjärjestelmät tue BitLocker-salausta.

> [AZURE.IMPORTANT] <sup>1</sup> Jos käytössäsi on Windows 10-koneen valmisteleminen kiintolevyä, lataa Azure Tuo/Vie-työkalun uusimman version.

### <a name="locations"></a>Sijainnit

Tietojen kopioiminen ja sieltä pois kaikki julkisen Azure-tallennustilan tilit Azure Tuo/Vie-palvelu tukee. Voit lähettää kiintolevyaseman levyasemien johonkin seuraavista sijainneista. Jos tallennustilan-tilisi on julkinen Azure sijaintiin, jossa ei ole määritetty, Vaihtoehtoinen toimitus-sijaintiin toimitetaan työ käyttämällä Classic-portaalin tai tuo/vie REST-Ohjelmointirajapinnalla luotaessa.

Tuetut lähetyksen sijainneista:

- Yhdysvaltojen Itä

- Länsi USA

- Yhdysvaltojen Itä 2

- Keskitetyn USA

- Pohjois-keskitetyn USA

- Etelä keskitetyn USA

- Pohjois-Eurooppa

- Länsi Europe

- Itä-Aasian

- Kaakkoisaasialaiset Aasian

- Australia Itä

- Australia varaaja

- Japani Länsi

- Japani Itä

- Keskitetyn Intia


### <a name="shipping"></a>Toimitus

**Toimitus ohjaa tietokeskuksen:**

Tuonti- tai projektin luotaessa voit antaa toimitusosoite jokin tuettu sijainnit toimitettava asemat. Annettu toimitusosoite riippuu tallennustilan tilin sijainti, mutta ne eivät välttämättä ole sama kuin tilin tallennuspaikka.

Lentoyhtiöiden, kuten FedEx, DHL, UPS tai US Postal Servicen avulla voit toimittaa asemat toimitus-osoitteeseen.

**Toimitus ohjaa Centeristä tiedot:**

Tuonti- tai projektin luotaessa sinun on määritettävä palautusosoitteen määrittäminen Microsoft käyttää toimitettaessa asemat takaisin, kun työ on valmis. Varmista, että annat kelvollinen palautusosoitteen käsittely viipeitä välttämiseksi.

Sinun on myös määritettävä kelvollinen FedEx tai DHL carrier tilin numero, jota käytetään Microsoft lähetyksen asemat takaisin. Lähetyksen asemat takaisin sijainneista Yhdysvalloissa ja Europe tarvitaan FedEx asiakasnumero. DHL asiakasnumero vaaditaan toimitus asemat takaisin Aasian ja Australian sijainneista. Voit luoda [FedEx](http://www.fedex.com/us/oadr/) (Yhdysvaltain sekä Europe) tai [DHL](http://www.dhl.com/) (Aasian ja Australia) carrier tili, jos sinulla ei ole yhtä. Jos sinulla on jo carrier asiakasnumero, varmista, että se on voimassa.

-Toimitus oman paketteja, sinun on noudatettava käyttöoikeussopimuksen ehdot [Microsoft Azure palveluehdot](https://azure.microsoft.com/support/legal/services-terms/)osoitteessa.

> [AZURE.IMPORTANT] Huomaa, että fyysinen media, jotka toimittavat joutua toimintojen kansainväliset reunat. Olet vastuussa fyysinen media ja tietojen tuoda ja viedä sovellettavan lainsäädännön mukaisesti. Kysy ennen toimittamista fyysinen media oman asiantuntijoita, tarkista, että media ja tiedot lainsäädännöllisistä syistä voidaan toimittaa tunnistettujen tietokeskuksen. Tämän avulla voit varmistaa, että se saavuttaa Microsoft ja julkaisemiseen tehokkaasti. Esimerkiksi paketin, jossa leikkaavat kansainväliset reunat on kaupallinen lasku (paitsi jos leikkaavat reunat Euroopan unionin alueella)-paketti liitetään. Kaupallinen lasku carrier sivustosta täytetty kopion voi tulostaa. Esimerkki kaupallinen lasku on [DHL kaupallinen lasku] (http://invoice-template.com/wp-content/uploads/dhl-commercial-invoice-template.pdf) tai [FedEx kaupallinen lasku](http://images.fedex.com/downloads/shared/shipdocuments/blankforms/commercialinvoice.pdf). Varmista, että Microsoft ei ole määritetty vientitoiminto nimellä.

## <a name="how-does-the-azure-importexport-service-work"></a>Miten Azure Tuo/Vie-palvelun toimii?

Voit siirtää tietoja paikallisen sivuston ja Azure-blob-säiliö Azure Tuo/Vie-palvelun avulla luomalla työt ja toimitus kiintolevyaseman levyasemien Azure data Centerissä välillä. Jokaisen kiintolevyaseman toimitetaan on liitetty yhden projektin. Jokainen työ on liitetty yhteen tallennustilan tilin. Lue [vaatimat osio](#pre-requisites) huolellisesti, jos haluat lisätietoja tämän palvelun, kuten tuettu Blob-objektien tyypit, levyn sekä sijainnit ja toimitus yksityiskohtia.

Tässä osassa on kuvaus ylätasolla vaiheita tuominen ja vieminen työt. Myöhemmin- [Pika-aloitus-osaan](#quick-start)annamme vaiheittaiset ohjeet luominen tuonti ja vienti työn.

### <a name="inside-an-import-job"></a>Tuo projektin sisällä

Korkean tason tuontityö etenee vaiheittain seuraavasti:

- Määrittää, tuotavat tiedot ja asemat tarvitset määrä.
- Määritä kohde-BLOB-Blob-säiliö tiedoillesi.
- Azure Tuo/Vie-työkalun avulla voit kopioida tiedot yhden tai useamman kiintolevyaseman levyasemien ja salata ne BitLockerilla.  
- Tuo projektin Classic-portaalin tai tuo/vie REST-Ohjelmointirajapinnalla kohde perinteinen tallennustilan tilin luominen Jos käytät perinteistä portaalin, lataa asema Päivyri tiedostot.
- Anna palautusosoitteen ja carrier numero, jota käytetään toimitus asemat puolestasi.
- Toimita kiintolevyaseman levyasemien lähetyksen osoitteeseen työpaikkojen aikana.
- Päivitä toimituksen seurantanumeron Tuo projektin tiedot ja Lähetä Tuontityön.
- Asemat vastaanotettu ja käsitellä Azure-palvelinkeskuksessa.
- Asemat toimitetaan käyttämällä carrier tiliä annettu Tuo projektin palautusosoite-kohtaan.

    ![Kuva 1:Import projektin työnkulku](./media/storage-import-export-service/importjob.png)


### <a name="inside-an-export-job"></a>Vientityö sisällä

Korkean tason vientityö etenee vaiheittain seuraavasti:

- Määrittää tiedot voidaan viedä ja asemat tarvitset määrä.
- Määritä lähde-BLOB- tai säilö polut tietojen Blob-objektien tallennustilaan.
- Luo lähde tallennustilan tilin käyttäminen Classic-portaalin tai tuo/vie REST-Ohjelmointirajapinnalla vientityö.
- Määritä lähde-BLOB tai säilö polut tietojen viennin työn.
- Anna palautusosoitteen ja carrier numero, jota käytetään toimitus asemat puolestasi.
- Toimita kiintolevyaseman levyasemien lähetyksen osoitteeseen työpaikkojen aikana.
- Päivitä toimituksen seurantanumeron Vie projektin tiedot ja Lähetä vientityön.
- Asemat vastaanotettu ja käsitellä Azure-palvelinkeskuksessa.
- Asemat salataan BitLockerilla; näppäimet ovat käytettävissä Classic-portaalin kautta.  
- Asemat toimitetaan käyttämällä carrier tiliä annettu Tuo projektin palautusosoite-kohtaan.

    ![Kuva 2:Export projektin työnkulku](./media/storage-import-export-service/exportjob.png)

### <a name="viewing-your-job-status"></a>Työn tilan tarkasteleminen

Voit seurata tilaa tuominen tai vieminen työt Classic-portaalista. Siirry Classic-portaalissa tallennustilan tilisi ja valitse **Tuo/Vie** -välilehti. Töiden luettelo tulee näkyviin sivulla. Voit suodattaa tilan, projektin nimi, työlaji tai seurantanumeron luettelossa.

Näyttöön tulee jokin seuraavista työn tiloista, sen mukaan, missä levyasemasta prosessi.

| Työn tila   | Kuvaus                                                                                                                                                                                                                                                                                                                                                                        |
|:-------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Luominen     | Työ on luotu, mutta et ole vielä antanut toimitustiedot.                                                                                                                                                                                                                                                                                                |
| Toimitus     | Työ on luotu ja olet määrittänyt toimitustiedot. **Huomautus**: kun asema toimitetaan Azure tietokeskuksen, tila saattaa näkyä edelleen "Toimitus" jonkin aikaa. Kun palvelu käynnistyy tietojen kopioiminen, tilaksi muuttuu "Transferring". Jos haluat nähdä aseman tiettyihin tilan, voit käyttää Tuo/Vie REST-Ohjelmointirajapinnalla. |
| Siirtäminen | Kiintolevyn (Tuo-projekti) siirretään tietojen tai kiintolevylle (, vientityö).                                                                                                                                                                                                                                                                 |
| Pakkaus    | Tietojen siirto on valmis ja kiintolevyä valmistellaan toimitusta takaisin varten.                                                                                                                                                                                                                                                                             |
| Viimeistele     | Kiintolevyä toimitettu puolestasi.                                                                                                                                                                                                                                                                                                                                      |

### <a name="time-to-process-job"></a>Prosessin työhön aikaa

Tuo/Vie-työ vaihtelee riippuen eri tekijöistä, kuten toimitusaikaa prosessi vie ajan projektin tyyppi, tyyppi ja koko kopioitava tiedot ja annettu levyjen koosta. Tuo tai Vie-palvelu ei ole SLA. REST-Ohjelmointirajapinnalla avulla voit seurata projektin tarkemmin. Luettelon työt-toimintoa, joka antaa kopioi edistymistä maininta on prosenttia valmiina-parametrin. Saavuttaminen us arvio suorittamiseen aika kriittinen Tuo/Vie työn tekemiseen.

### <a name="pricing"></a>Hinnat

**Asema maksun käsittely**

On asema käsittely maksu kansionsa käsitteleminen osana tuominen tai vieminen työn. Katso tietoja [Azure Tuo/Vie hinnat](https://azure.microsoft.com/pricing/details/storage-import-export/).

**Toimitus kustannukset**

Toimitettaessa asemat Azure maksat toimitus-kustannukset toimitus-carrier. Kun Microsoft palauttaa asemat sinulle, toimitus-kustannukset veloitetaan carrier-tilille, jonka antamasi työpaikkojen aikaan.

**Tapahtuman kustannukset**

Jakautuvat ei kustannukset, kun tietoja tuodaan Blob-objektien tallennustilaan. Vakio juniin maksut ovat käytettävissä, kun tiedot viedään Blob-objektien tallennustilaan. Lisätietoja kustannukset [tiedonsiirron hinnat.](https://azure.microsoft.com/pricing/details/data-transfers/)

## <a name="quick-start"></a>Pika-aloitusopas

Tässä osassa annamme vaiheittaiset ohjeet tuonti ja vientityön luomista varten. Varmista, että kaikki [vaatimukset](#pre-requisites) täyttyvät ennen työn edetessä.

## <a name="how-to-create-an-import-job"></a>Tuo projektin luomisesta?

Luo tuontityö tietojen kopioiminen Azure tallennustilan tiliisi kiintolevyillä toimitus vähintään yksi asemat, jotka sisältävät määritetyn tietokeskuksen tietojen mukaan. Tuo projektin välittää tietoja kiintolevyaseman levyasemien, tiedot kopioidaan, kohdeluettelo-tallennustilan tilin ja toimitustiedot Azure Tuo/Vie-palveluun. Tuo projektin luominen on kolme vaihetta. Valmistele ensin asemat Azure Tuo/Vie asiakas-työkalun avulla. Lähetä se, tuontityö, klassinen-portaalissa. Kolmas toimitus aikana työpaikkojen toimitusosoite asemat ja Päivitä projektin tiedot toimitus-tiedot.   

> [AZURE.IMPORTANT] Voit lähettää tallennustilan tilin on vain yksi työ. Kunkin aseman toimitetaan voidaan tuoda tallennustilan asiakkaalle. Esimerkiksi oletetaan, että haluat tuoda tiedot kaksi tallennustilan tilit. Käytä erillistä kiintolevyaseman levyasemien tallennustilan tileille ja luoda erilliset töitä tallennustilan tilin kohden.

### <a name="prepare-your-drives"></a>Valmistele asemat

Kun tuot tietoja Azure Tuo/Vie-palvelun avulla ensimmäiseksi on valmisteleminen asemat Azure Tuo/Vie asiakas-työkalun avulla. Noudata seuraavia ohjeita voit valmistella asemat.

1.  Määritä tuotavat tiedot. Tämä voi johtua kansioiden ja tiedostojen erillinen paikallisen palvelimen tai jaettuun verkkoasemaan.  

2.  Määrittää, kuinka asemista, sinun on kokonaiskoko tietojen mukaan. Hankitaan 3,5 tuuman SATA III tai II kiintolevyaseman levyasemien tarvittava määrä.

3.  Määritä target tallennustilan tilin, säilö, näennäiskansiot ja BLOB-objektit.

4.  Selvitä, kansioita ja/tai erillisen tiedostot, jotka kopioidaan jokaisen kiintolevyaseman.

5.  Tietojen kopioiminen vähintään yksi kiintolevyillä [Azure Tuo/Vie-työkalun](http://go.microsoft.com/fwlink/?LinkID=301900&clcid=0x409) avulla.

    - Azure Tuo/Vie-työkalu luo kopioi istuntoon ja tietojen kopioiminen kiintolevyaseman levyasemien lähde. Työkalu kopioida yksittäistä kopiota-istunnossa yksittäisen kansion ja sen alihakemistoista tai yksittäinen tiedosto.

    - Voit joutua useita kopioi istuntojen Jos lähdetiedot ulottuu monta kansiota.

    - Jokaisen kiintolevyaseman valmistelet edellyttävät vähintään yhden kappaleen istunto.

6.  Voit määrittää, / salaa parametrin käyttöön kiintolevy Bitlocker-salausta. Voit myös voinut käyttöön Bitlocker salauksen manuaalisesti kiintolevy ja anna avain, kun käytetään.

7.  Azure Tuo/Vie-työkalu luo kansionsa asema Päivyri-tiedoston, kun se on valmis. Asema Päivyri-tiedosto on tallennettu paikallisessa tietokoneessa ei ole asemassa itse. Lataa Päivyri-tiedosto Tuontityön luodessasi. Asema Päivyri-tiedosto sisältää asema-tunnus ja BitLocker-näppäintä sekä muita tietoja asemasta.
**Tärkeää**: jokaisen kiintolevyaseman valmistelet aiheuttaa Päivyri-tiedosto. Kun luot Tuontityön Classic-portaalissa, täytyy ladata kaikki Päivyri on, että tuo työhön levyasemien tiedostot. Ohjaa ilman Päivyri tiedostoja ei voi käsitellä.

8.  Älä muokkaa kiintolevyaseman levyasemien tai Päivyri-tiedoston tietojen suorittamisen levyn valmistelun jälkeen.

Alla on komentoja ja esimerkkejä valmistellaan kiintolevy Azure Tuo/Vie asiakas-työkalun avulla.

Azure Tuo/Vie asiakkaan työkalun PrepImport komento ensimmäisen kopioi istunnon, Kopioi kansioon:

    WAImportExport PrepImport /sk:<StorageAccountKey> /csas:<ContainerSas> /t: <TargetDriveLetter> [/format] [/silentmode] [/encrypt] [/bk:<BitLockerKey>] [/logdir:<LogDirectory>] /j:<JournalFile> /id:<SessionId> /srcdir:<SourceDirectory> /dstdir:<DestinationBlobVirtualDirectory> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]

**Esimerkki:**

Alla olevassa esimerkissä kopioidaan kaikki tiedostot ja sub hakemistojen poistaminen H:\Video asentaa x kiintolevylle. Tietoja tuodaan kohde-tallennustilan tilin tallennustilan tilin-näppäintä ja kutsua video tallennustilan säilöön. Jos tallennustilan säilö ei ole määritetty, se luodaan. Tämä komento myös muotoileminen ja salata kohde kiintolevy.

    WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Video1 /logdir:c:\logs /sk:storageaccountkey /t:x /format /encrypt /srcdir:H:\Video1 /dstdir:video/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt

Azure Tuo/Vie asiakkaan työkalun PrepImport komento kopioi myöhemmät istuntojen Kopioi kansioon:

    WAImportExport PrepImport /j:<JournalFile> /id:<SessionId> /srcdir:<SourceDirectory> /dstdir:<DestinationBlobVirtualDirectory> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]

Kopioi myöhemmät istuntojen saman kiintolevylle Määritä saman tiedostonimi ja anna uusi istunnon; tunnus ei ei tarvitse antaa tallennustilan tilin avain ja kohde-asema uudelleen, eikä muotoileminen ja salata asemaa. Tässä esimerkissä on kopioidaan H:\Photo-kansio ja sen sub-kansioiden saman kohdeasema valokuva tallennustilan säilöön.

    WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Photo /srcdir:H:\Photo /dstdir:photo/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt

Azure Tuo/Vie asiakasohjelma ensimmäisen kopioi istunnon, voit kopioida tiedoston PrepImport komento:

    WAImportExport PrepImport /sk:<StorageAccountKey> /csas:<ContainerSas> /t: <TargetDriveLetter> [/format] [/silentmode] [/encrypt] [/bk:<BitLockerKey>] [/logdir:<LogDirectory>] /j:<JournalFile> /id:<SessionId> /srcfile:<SourceFile> /dstblob:<DestinationBlobPath> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]

Azure Tuo/Vie asiakasohjelma PrepImport-komento, joka kopioi myöhemmät istuntoon ja tiedoston kopioiminen:

    WAImportExport PrepImport /j:<JournalFile> /id:<SessionId> /srcfile:<SourceFile> /dstblob:<DestinationBlobPath> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]

**Muista**: oletusarvoisesti tiedot tuodaan kuin estä BLOB-objektit. /BlobType-parametrin avulla voit tuoda tiedot sivun BLOB-objektit. Esimerkiksi jos olet tuomassa näennäiskiintolevytiedostoja, joka otetaan käyttöön kuin levyjen Azure-AM-ja tuotava ne kuin sivun BLOB-objektit. Jos et ole varma, mitä blob projektityypin valinnassa, voit määrittää /blobType:auto ja autamme oikean tyyppi. Tässä tapauksessa kaikki näennäiskiintolevyn ja vhdx tiedostot tuodaan kuin sivun BLOB-objektit ja muut tuodaan kuin estä BLOB-objektit.

Lisätietoja Azure Tuo/Vie asiakas-työkalun avulla [valmistellaan kiintolevyn](https://msdn.microsoft.com/library/dn529089.aspx)asemat tuomista varten.

Katso myös [Otoksen työnkulun valmisteleminen Kova ohjaa tuo työn](https://msdn.microsoft.com/library/dn529097.aspx) tarkempia vaiheittaisia ohjeita.  

### <a name="create-the-import-job"></a>Tuo projektin luominen

1.  Kun olet valmistellut levyasemasta, siirry [perinteiseen portal](https://manage.windowsazure.com) -tallennustilan tilin ja tarkastella raporttinäkymiä. Valitse **Quick Glance**-kohdassa **Luo tuonti-työ**. Tarkista vaiheet ja valitsemalla valintaruutu osoittaa, että olet valmistellut levyasemasta ja että sinulla on käytettävissä asema Päivyri-tiedosto.

2.  Ole-toiminnon vaihe 1 yhteystietojen Tuontityön ja kelvollinen palautusosoitteen vastuuhenkilö. Jos haluat tallentaa yksityiskohtainen lokitiedot tuo työn, tarkista voi **tallentaa yksityiskohtainen Kirjaudu sisään oma 'waimportexport' blob-säilö**.

3.  Valitse vaiheessa 2 lataa asema Päivyri tiedostot, jotka olet hankkinut asema valmistelu vaiheessa. Haluat ladata yhden tiedoston, jotka olet valmistellut kansionsa.

    ![Luo tuontityö - vaihe 3](./media/storage-import-export-service/import-job-03.png)

4.  Kirjoita vaiheessa 3 Tuontityön kuvaava nimi. Huomaa, että kirjoittamasi nimi voi olla vain pieniä kirjaimia, numeroita, väliviivoja ja alaviivoja, on alettava kirjaimella ja ei saa sisältää välilyöntejä. Voit käyttää haluat seurata työt, kun ne ovat käynnissä ja kun ne ovat valmiit nimeä.

    Valitse seuraavaksi center tietoalueen luettelosta. Keskitä tietoalueen kertovat tietokeskuksen ja osoite, johon on toimitettava paketti. Artikkelissa lisätietoja usein kysytyt alapuolella.

5.  Vaiheessa 4 Valitse Palauta carrier luettelosta ja kirjoita carrier asiakasnumero. Microsoft toimittaa asemat takaisin, kun työtäsi tuonti on valmis käyttämällä tähän tiliin.

    Jos sinulla on seurantanumeron, valitse toimituksen carrier luettelosta ja kirjoita seurantanumeron.

    Jos et ole seurantanumeron on vielä, valitse **I will provide määritän lähetyksen tietojani tuo työlle, kun minulla on toimitettu Omat paketti**, valitse Suorita tuonti.

6. Lisätäksesi seurantanumeron, kun olet toimitettiin paketti, palaa tallennustilan tilin Classic-portaalissa **Tuo/Vie** -sivun Valitse työtäsi luettelosta ja valitse **Toimitus tiedot**. Ohjatun toiminnon siirtyminen ja kirjoita seurantanumeron vaiheessa 2.

    Jos seurantanumeron ei päivitetä kahden viikon työn luomista, työ päättyy.

    Jos työ on luominen, toimitus tai Transferring-tilassa, voit myös päivittää carrier pankkitilin numeron vaiheessa 2 ohjatun toiminnon. Kun työ on pakkaus-tilassa, et voi päivittää carrier pankkitilin numeron, työn.

7. Projektin edistymisen seuraaminen portaalin koontinäytössä. Katso edellisessä osassa työn kukin tila tarkoittaa tarkastelemalla [työn tila](#viewing-your-job-status).

## <a name="how-to-create-an-export-job"></a>Vientityön luomisesta?

Luo vientityö Tuo/Vie-palvelun ilmoittaa, että voit olla toimitus vähintään yksi tyhjä asemat tietokeskuksen niin, että tiedot voidaan viedään tallennustilan tililtä asemat ja asemat sitten toimitettiin asiakkaalle.

### <a name="prepare-your-drives"></a>Valmistele asemat

Seuraavat vanhat tarkistukset suositellaan asemat valmisteleminen vientityö:

1. Tarkista pakollinen Azure Tuo/Vie-työkalun PreviewExport komennolla levyjen määrä. Lisätietoja on artikkelissa [Esikatselun vaikuttavat käyttö Vie työn](https://msdn.microsoft.com/library/azure/dn722414.aspx). Sen avulla voit esikatsella asema käyttö valitsit, BLOB perustuu aiot käyttää asemat kokoa.

2. Tarkista, että olet voivat lukea ja kirjoittaa, jotka toimitetaan Vie työn kiintolevylle.

### <a name="create-the-export-job"></a>Vientityön luominen

1.  Luo vientityö, siirry [perinteiseen portal](https://manage.windowsazure.com)-tallennustilan tilin ja tarkastella raporttinäkymiä. Valitse **Quick Glance**valitsemalla **Luo vientityön** ja jatka ohjatussa toiminnossa.

2.  Valitse vaiheessa 2 on vastuussa vientityön yhteystiedot. Jos haluat tallentaa yksityiskohtainen lokitiedot Vie työn, tarkista voi **tallentaa yksityiskohtainen Kirjaudu sisään oma 'waimportexport' blob-säilö**.

3.  Vaihe 3 Määritä, mitä haluat viedä tallennustilan tililtä tyhjä-asema tai asemat blob-tietoja. Voit viedä kaikki Blob-objektien tietoja tallennustilan tilin tai voit määrittää BLOB tai joukkoihin BLOB Vie.

    Jos haluat määrittää blob, käytä **Yhtä suuri kuin** -valitsin ja määritä blob-säilö nimi alkaa suhteellinen polku. *$Root* avulla voit määrittää pääkansion säilö.

    Voit määrittää kaikki Blob-objektien alkaen etuliitteen, käytä **Käynnistyy ja** valitseminen ja määritä etuliite vinoviivalla alkavat "/". Etuliite voi olla etuliite säilön nimi, koko säilössä tai etuliite blob nimen perässä valmis säilössä.

    Seuraavassa taulukossa on esimerkkejä kelvollinen Blob-objektien polut:

    Valitseminen|BLOB-polku|Kuvaus
    ---|---|---
    Alkaa seuraavasti:|/|Vie kaikki Blob-objektien tallennustila-tilille
    Alkaa seuraavasti:|/$root /|Vie kaikki Blob-objektien päämyöntäjien säilöön
    Alkaa seuraavasti:|/Book|Vie kaikki Blob-objektien säilössä, joka alkaa etuliite **Osoitteisto**
    Alkaa seuraavasti:|/Music/|Vie kaikki BLOB-säilö **musiikkia**
    Alkaa seuraavasti:|/ musiikkia/Rakkaus|Vie kaikki BLOB-säilö **musiikkia** , jotka alkavat etuliite **perinteisen Wordin kanssa**
    Yhtä suuri kuin|$root/logo.bmp|Vie blob **logo.bmp** päämyöntäjien säilöön
    Yhtä suuri kuin|videos/Story.mp4|Vie blob-säilö **videot** **story.mp4**

    Sinun on määritettävä kelvolliset muodot ja välttää virheet käsiteltäessä-blob-polut tässä näyttökuvassa esitetyllä tavalla.

    ![Luo vientityö - vaihe 3](./media/storage-import-export-service/export-job-03.png)


4.  Kirjoita vaiheessa 4 vientityön kuvaava nimi. Kirjoittamasi nimi voi olla vain pieniä kirjaimia, numeroita, väliviivoja ja alaviivoja, on alettava kirjaimella ja ei saa sisältää välilyöntejä.

    Keskitä tietoalueen ilmaisee, joihin sinun on toimitettava paketti tietokeskuksen. Artikkelissa lisätietoja usein kysytyt alapuolella.

5.  Valitse vaiheessa 5 palaa carrier luettelosta ja kirjoita carrier asiakasnumero. Microsoft toimittaa asemat takaisin, kun työtäsi vienti on valmis käyttämällä tähän tiliin.

    Jos sinulla on seurantanumeron, valitse toimituksen carrier luettelosta ja kirjoita seurantanumeron.

    Jos et ole seurantanumeron on vielä, valitse **I will provide määritän lähetyksen tietojani Vie työlle, kun minulla on toimitettu Omat paketti**, suorita vientiprosessissa.

6. Lisätäksesi seurantanumeron, kun olet toimitettiin paketti, palaa tallennustilan tilin Classic-portaalissa **Tuo/Vie** -sivun Valitse työtäsi luettelosta ja valitse **Toimitus tiedot**. Ohjatun toiminnon siirtyminen ja kirjoita seurantanumeron vaiheessa 2.

    Jos seurantanumeron ei päivitetä kahden viikon työn luomista, työ päättyy.

    Jos työ on luominen, toimitus tai Transferring-tilassa, voit myös päivittää carrier pankkitilin numeron vaiheessa 2 ohjatun toiminnon. Kun työ on pakkaus-tilassa, et voi päivittää carrier pankkitilin numeron, työn.

    > [AZURE.NOTE] Jos viedään blob ei kopioiminen kiintolevylle aikaa, Azure Tuo/Vie-palvelu ottaa talteen tilannevedoksia blob ja kopioi tilannevedoksen.

7.  Projektin edistymisen seuraaminen koontinäytössä Classic-portaalissa. Katso, mitä työn kukin tila tarkoittaa edellisessä osassa edelleen "tarkasteleminen projektin tila".

8.  Kun saat asemat viedyt tiedot, voit tarkastella ja kopioi järjestetyn palvelun aseman BitLocker-avaimet. Siirry Classic-portaalissa tallennustilan tilisi ja valitse Tuo/Vie-välilehti. Vie työtäsi luettelosta ja valitsemalla Näytä-näppäimiä. BitLocker näppäimet näkyvät alla seuraavasti:

    ![Tarkastele vientityön BitLocker näppäimet](.\media\storage-import-export-service\export-job-bitlocker-keys.png)

Siirry – usein kysytyt kysymykset-osan, kun se peittää eniten yleisiin kysymyksiin, jotka asiakkaat ilmetä, kun tämän palvelun avulla.

## <a name="frequently-asked-questions"></a>Usein kysytyt kysymykset ##


**Kuinka kauan kopioida tiedot, kun oma päivittämiseen saavuttaa tietokeskuksen kestää?**

Voit kopioida aika vaihtelee sen mukaan eri tekijöistä, kuten työlaji, tyyppi ja koko tietojen kopioitava-levyjen koosta on laatinut, ja nykyisen työmäärää. Se voi erota muutaman päivän, viikon seikoista riippuen tapahtuu. Tämän vuoksi on vaikea antamaan Yleiset arvio. Palvelun yrittää optimoida työtäsi kopioimalla useita asemat rinnakkain, kun se on mahdollista. Jos sinulla on aikaa kriittinen Tuo/Vie työn, saavuttaminen us arvion.

**Kun Azure Tuo/Vie palvelun kannattaa käyttää?**
Yksi huomioon Azure Tuo Vie avulla, jos lataus kokonaan verkossa kestää noin arvioida yli seitsemän päivää. Voit laskea, kuinka kauan kestää käyttämällä minkä tahansa online Laskimen tai [lataamalla](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/archive/master.zip) se sijaitsee Microsoftin Azure Tuo Vie REST API otoksen Azure näytteiden säilöön, [Tietojen siirtäminen nopeus laskuri](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html)-palvelussa. Tämä ei ole tarkka laskentaan, mutta karkea merkintä.

**Azure Tuo/Vie-palvelun avulla Resurssienhallinta-tallennustilan tilin kanssa**

Ei, et voi kopioida tietoja, tai Resurssienhallinta tallennustilan tililtä suoraan Azure Tuo/Vie-palvelun avulla. Palvelun tukee vain perinteinen tallennustilan tilit. Resurssienhallinta tallennustilan tilin tuki tulossa pian. Käyttää seuraavaa vaihtoehtona tietojen tuominen perinteinen tallennustilan tilin ja siirtäminen Resurssienhallinta tallennustilan tilin käyttäminen [AzCopy](storage-use-azcopy.md) tai CopyBlob.

**Voinko kopioida Azure tiedostot Azure Tuo/Vie-palveluun?**

Ei, Azure Tuo/Vie-palvelu tukee vain estä BLOB-objektit ja sivun BLOB-objektit. Kaikki muut sisältävät Azure-tiedostoja, taulukoita ja olevien tallennustilan tiedostotyypit ei tueta.

**On Azure Tuo/Vie-palvelun käytettävissä CSP tilauksissa?**

Azure Tuo/Vie-palvelu ei tue CSP tilaukset. Lisätään tuesta myöhemmin.

**Voin ohittaa asema valmistelu vaiheen tuo työn tai voin valmisteleminen aseman kopioimatta?**

Minkä tahansa asema, johon haluat lähettää tietojen tuomiselle on valmisteltava Azure Tuo/Vie asiakas-työkalun avulla. Asiakas-työkalun avulla on kopioida tietoja asemaan.

**Pitääkö suorittaa minkä tahansa levyn valmistelu vientityön luotaessa?**

Ei, mutta jotkin vanhat tarkistukset on suositeltavaa. Tarkista pakollinen Azure Tuo/Vie-työkalun PreviewExport komennolla levyjen määrä. Lisätietoja on artikkelissa [Esikatselun vaikuttavat käyttö Vie työn](https://msdn.microsoft.com/library/azure/dn722414.aspx). Sen avulla voit esikatsella asema käyttö valitsit, BLOB perustuu aiot käyttää asemat kokoa. Tarkista, että sähköpostiviestien lukeminen ja kirjoittaminen kiintolevylle, jotka toimitetaan Vie työn myös.

**Mitä tapahtuu, jos lähetän vahingossa Kiintolevy, joka ei ole tuettu vaatimuksia?**

Azure tietokeskuksen palauttaa asema, joka ei ole tuettu ohjelmistovaatimusten sinulle. Jos vain osa paketin asemat täyttävät tuki, kyseiset asemat käsitellään ja asemat, jotka eivät vastaa vaatimukset palautetaan sinulle.

**Voit peruuttaa Oma työ?**

Voit peruuttaa työn sen tila on luotaessa tai toimitus.

**Kuinka kauan valmiiden töiden tilan tarkasteleminen Classic-portaalissa?**

Voit tarkastella enintään 90 päivää valmiit työt tila. Valmiit työt poistetaan 90 päivän jälkeen.

**Jos halua tuominen ja vieminen yli 10 asemista, mitä pitäisi tehdä?**

Yksi tuonti tai vientityön voi viitata vain 10 asemat yhden projektin Tuo/Vie-palveluun. Jos haluat lähettää yli 10 asemista, voit luoda useita töitä. Asemat, jotka liittyvät saman työn on toimitettu yhdessä samassa paketissa.

**Voinko käyttää USB-SATA-sovittimen, joka ei ole suositellut luettelossa?**

Jos sinulla on muuntimen, joka ei ole luettelossa, voit kokeilla Azure Tuo/Vie-työkalun avulla oman muuntimen valmisteleminen asema ja nähdä, jos se toimii ennen hankkimista tuettu tiedostomuunninta.

**Palvelun muotoilla asemat ennen niiden palauttamisen?**

Ei. Kaikki asemat salataan BitLockerilla.

**Tuo tai Vie töiden asemat ostaa Microsoftilta**

Ei. Sinun on toimitus oman asemista sekä tuonti ja vienti työt.

**Tuo projektin päätyttyä, mikä Omat tiedot näyttävät tallennustilan tilin? Säilytetään hakemisto-hierarkia**

Kun valmistelet kiintolevylle tuontityö, kohde määritetään /dstdir: parametria. Tämä on kohdesäilön tallennustilan tilin, johon kiintolevyä tiedot kopioidaan. Tämä kohdesäilön Näennäiskansiot luodaan kiintolevyä kansiot ja BLOB-objektit luodaan tiedostot.

**Jos asema on tiedostoja, jotka ovat jo tallennustilan tilin, se palvelun korvaa aiemmin BLOB storage-tilini?**

Kun valmistelet asema, voivat määrittää kohde-tiedostoja voi korvata tai ohitetut käyttämällä parametria kutsutaan /Disposition: < nimeäminen uudelleen | ei korvaa | korvaa >. Oletusarvon mukaan palvelu nimeä uusia tiedostoja sijaan korvaa aiemmin luotu BLOB-objektit.

**Onko Azure Tuo/Vie-asiakasohjelma yhteensopiva 32-bittisten käyttöjärjestelmien?**
Ei. Asiakasohjelma on vain yhteensopiva 64-bittinen Windows-käyttöjärjestelmä. Tutustu käyttöjärjestelmät osan [vaatimat](#pre-requisites) kattavaan luetteloon, tuettujen OS versioiden.

**Pidä olla mitään kiintolevy kuin oma pakettiin?**

Tarkista toimitus vain oman kiintolevyillä. Älä sisällytä kohteita, kuten power toimituksen kaapeli tai USB-kaapeli.

**Onko minun toimitetaan Omat asemat FedEx tai DHL?**

Voit toimittaa asemat tietokeskuksen kaikki tunnetut carrier, kuten FedEx, DHL, UPS tai US Postal Servicen avulla. Kuitenkin lähetyksen asemat takaisin tietojen Centeristä, sinun on määritettävä FedEx asiakasnumero, Yhdysvalloissa ja EU: N tai DHL asiakasnumero, Aasian ja Australian alueilla.

**Mitä rajoituksia kanssa toimitus asema kansainvälisesti?**

Huomaa, että fyysinen media, jotka toimittavat joutua toimintojen kansainväliset reunat. Olet vastuussa fyysinen media ja tietojen tuoda ja viedä sovellettavan lainsäädännön mukaisesti. Kysy ennen toimittamista fyysinen media-että Advisor voit varmistaa, että media ja tiedot lainsäädännöllisistä syistä voidaan toimittaa tunnistettujen tietokeskuksen. Tämän avulla voit varmistaa, että se saavuttaa Microsoft ja julkaisemiseen tehokkaasti.

**Työn luotaessa toimitusosoite on sijaintiin, jossa ei ole sama kuin oma tili-tallennuspaikka. Mitä minun pitäisi tehdä?**

Jotkin tallennuspaikkojen tili on yhdistetty vaihtoehtoinen toimitus sijainteihin. Aiemmin lähetyksen paikat myös voidaan tilapäisesti määrittää vaihtoehtoiset sijainnit. Tarkista aina ennen toimittamista asemat työpaikkojen aikana toimitusosoite.

**Miksi oma projektin tila perinteinen portaalin sano "toimitus" Kun Carrier sivuston näkyy Omat paketti toimitetaan?**

Perinteinen-portaalissa tila muuttuu toimitus siirtäminen milloin käsittely alkaa asema. Jos levyasemasta tila on muutettu, mutta sitä ei ole aloitettu käsittely, työ-tila näkyy toimitus.

**Asema toimitettaessa carrier pyytää tietoja center yhteyshenkilön nimi ja puhelinnumero. Miten voin antaa?**

Puhelinnumeron annetaan sinulle työn luonnin aikana. Jos tarvitset yhteyshenkilön nimeä, ota yhteyttä osoitteessa waimportexport@microsoft.com ja toimittaa käyttäjälle tiedon.

**PST-postilaatikot ja SharePoint-tietojen kopioiminen O365 Azure Tuo/Vie-palvelun avulla?**

Tutustu [Tuo PST-tiedostoja tai SharePoint-tietoja Office 365: een](https://technet.microsoft.com/library/ms.o365.cc.ingestionhelp.aspx).

**Kopioi Azure varmuuskopiointi-palvelun offline-tilassa Omat varmuuskopiot Azure Tuo/Vie-palvelun avulla?**

Tutustu [Azure varmuuskopiointi offline-tilassa varmuuskopiointi-työnkulussa](../backup/backup-azure-backup-import-export.md).

## <a name="see-also"></a>Katso myös:

- [Määrittämisestä asiakasohjelma Azure Tuo/Vie](https://msdn.microsoft.com/library/dn529112.aspx)

- [Siirrä tiedot AzCopy komentorivin-apuohjelman avulla](storage-use-azcopy.md)

- [Azure Tuo Vie REST API malli](https://azure.microsoft.com/documentation/samples/storage-dotnet-import-export-job-management/)

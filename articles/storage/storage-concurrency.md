<properties
    pageTitle="Samanaikainen Microsoft Azuren tallennustilaan hallinta"
    description="Samanaikainen Blob, jonossa taulukon tai tiedosto-palveluiden hallinta"
    services="storage"
    documentationCenter=""
    authors="jasonnewyork"
    manager="tadb"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jahogg"/>

# <a name="managing-concurrency-in-microsoft-azure-storage"></a>Samanaikainen Microsoft Azuren tallennustilaan hallinta

## <a name="overview"></a>Yleiskatsaus

Nykyaikainen Internet-pohjaisten sovellusten on yleensä useita käyttäjiä tarkastelemista ja päivittämistä tietojen samanaikaisesti. Tämä edellyttää sovelluskehittäjät huolellisesti mietittävä, voit säätää ennakoitavissa kokemus loppukäyttäjille, erityisesti skenaariot, johon usea käyttäjä voi päivittää samat tiedot. On kolme tärkeimmät tiedot samanaikainen strategioita kehittäjät yleensä ottaa huomioon:  


1.  Optimistinen samanaikainen – sovelluksen suorittaminen-päivitys sen päivityksen osana tarkistaa, jos tiedot on muuttunut sovelluksen lukenut tiedot. Esimerkiksi jos tarkasteleminen wikisivun kaksi käyttäjää samalla sivulla päivitys sitten wiki-ympäristö on varmistettava, että toinen päivitys ei korvaa ensimmäinen päivitys – ja molemmat käyttäjät ymmärrä, onko niiden päivityksen onnistuneen vai ei. Tämä strategia käytetään useimmin verkkosovellusten.
2.  Pessimistinen samanaikainen –-sovelluksen Haluatko päivityksen suorittaminen kestää lukko objektiin, tiedot päivitetään, kunnes lukitus on muiden käyttäjien estäminen. Esimerkiksi missä vain perusmuodon kohteita päivitetään perustyyli/toissijaisen tietojen replikoinnin tilanne perusmuodon yleensä mahtuu yksityinen lukitus pitkäksi aikaa, jotta kukaan muu ei voi päivittää tiedot.
3.  Viimeinen kirjoittaja voittaa – toimintatavan, joka sallii päivityksen toiminnot Jatka tarkistamatta, jos jokin sovellus on päivitetty tiedot jälkeen sovellus ensin lukea tiedot. Tämä strategia (tai puutteen muodollinen strategia) käytetään yleensä kohtaa, johon tiedot osioidaan siten, että ei ole todennäköisyys, että usea käyttäjä voi käyttää samoja tietoja. Voi myös olla hyödyllistä missä lyhytkestoinen tietojen virtaa käsitellään.  

Tässä artikkelissa on yleiskatsaus siitä, miten Azuren tallennustilaan platform yksinkertaistaa kehittäminen antamalla ensimmäisen luokan tuki kaikille kolmelle nämä samanaikainen strategioita.  

## <a name="azure-storage--simplifies-cloud-development"></a>Azure-tallennustilan – yksinkertaistaa Cloud kehittäminen
Azure tallennuspalvelu tukee kaikki kolme strategioita, vaikka erottamiskykyä täyden tuen tarjoaminen Optimistinen ja Pessimistinen samanaikainen, koska se on suunniteltu laajuisiksi vahva yhdenmukaisuuden-malli, joka takaa, että tallennuspalvelu vahvistaa tietojen lisääminen tai kaikki päivitystoiminto edelleen käyttää haluat tietojen näkevät uusimmassa päivityksessä mahdollisuus. Tallennustilan ympäristöissä, jotka käyttävät potentiaalisen yhdenmukaisuuden-malli on välinen viive kun Kirjoita suoritetaan, yksi käyttäjä ja muiden käyttäjien sekoittavista näin asiakassovelluksissa estämiseksi epäyhtenäisyydet vaikutuksilta loppukäyttäjät kehittäminen näkevät päivitetyt tiedot.  

Lisäksi valitsemalla haluamasi samanaikainen strategia kehittäjien myös tulee tietää, miten tallennustilan ympäristö eristää muutokset – etenkin samaan objektiin yli tapahtumat. Azure tallennuspalvelu käyttää tilannevedoksen eristystaso sallimaan luku toimintojen tapahtuvan yksittäisen osion toiminnot kirjoittaminen samanaikaisesti. Toisin kuin muut eristystaso tasot tilannevedoksen eristystaso takaa, että kaikki lukee nähdä tiedot yhdenmukaisia tilannevedoksen myös silloin, kun päivitykset ovat määrä – olennaisesti palauttamalla päivityksen aikana vahvistettu viimeisen arvon tapahtumaa käsitellään.  

## <a name="managing-concurrency-in-blob-storage"></a>Samanaikainen hallinta Blob-objektien tallennustilaan
Voit valita joko Optimistinen tai Pessimistinen samanaikainen mallien avulla voit hallita niiden käyttöä BLOB-objektit ja säilöjä blob-palvelussa. Jos et erikseen Määritä strategia viimeisen kirjoituksia wins on oletusarvon mukaan.  

### <a name="optimistic-concurrency-for-blobs-and-containers"></a>Optimistinen samanaikainen BLOB-objektit ja säilöt  

Tallennustilan palvelun määrittää tunniste kaikki objektit, jotka on tallennettu. Tämän tunnisteen päivittyy aina, kun päivitystoiminto suoritetaan objektin. HTTP GET vastauksen, käyttämällä ETag (kohteen tunniste) otsikon, joka on määritetty sisällä HTTP-protokolla osana asiakkaan palauttaa tunnus. Päivityksen suorittaminen, objektin käyttäjä voi lähettää alkuperäisen ETag sekä ehdollisen ylä-varmistaa, että päivitys tapahtuu vain, jos tietty ehto täyttyy – tässä tapauksessa ehto on "Jos vastine"-otsikon joka edellyttää Tallennuspalvelu varmistamiseksi päivitys-pyynnössä ETag arvo on sama kuin tallennettu tallennustilan-palveluun.  

Tämä toimenpide Jäsennys on seuraavanlainen:  

1.  Noutaa blob storage-palvelusta, vastaus sisältää HTTP ETag-otsikko-arvo, joka määrittää nykyisen version objektin tallennustilan-palvelussa.
2.  Kun päivität blob, Sisällytä olet saanut vaiheessa 1, **Jos vastine** ehdollinen otsikossa palveluun lähettää pyynnön ETag-arvo.
3.  Palvelun vertaa ETag arvon pyynnössä blob ETag nykyarvon.
4.  Jos blob ETag nykyarvon on eri kuin **Jos vastine** ehdollinen otsikko-pyynnössä ETag versio, palvelu palauttaa 412 virheen asiakkaalle. Tämä ilmaisee asiakkaalle, että toinen prosessi on päivittänyt blob, koska asiakas noutaa sen.
5.  Jos blob ETag nykyarvon on ETag **Jos vastine** ehdollinen otsikko-pyynnössä versiota, palvelu suorittaa toimintoa ja päivittää Blob-objektien osoittamaan, että se on luonut uuden version ETag nykyarvon.  

Seuraavat C# koodikatkelman (joko asiakkaan tallennustilan kirjaston 4.2.0) näyttää yksinkertainen, miten voit käyttää **Jos vastine AccessCondition** perusteella ETag-arvo, jota käytetään Blob-objektien, jotka on aiemmin noudettu tai lisätä ominaisuuksista. Se käyttää **AccessCondition** objektin kun se päivitetään blob: **AccessCondition** objektin Lisää **Jos vastine** otsikon pyynnön. Jos toinen prosessi on päivitetty blob-blob-palvelu palauttaa HTTP 412 (ennen epäonnistui)-tila-sanoma. Voit ladata koko otoksen: [Käyttämällä Azure-tallennustilan hallinta samanaikainen](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114).  

    // Retrieve the ETag from the newly created blob
    // Etag is already populated as UploadText should cause a PUT Blob call
    // to storage blob service which returns the etag in response.
    string orignalETag = blockBlob.Properties.ETag;

    // This code simulates an update by a third party.
    string helloText = "Blob updated by a third party.";

    // No etag, provided so orignal blob is overwritten (thus generating a new etag)
    blockBlob.UploadText(helloText);
    Console.WriteLine("Blob updated. Updated ETag = {0}",
    blockBlob.Properties.ETag);

    // Now try to update the blob using the orignal ETag provided when the blob was created
    try
    {
        Console.WriteLine("Trying to update blob using orignal etag to generate if-match access condition");
        blockBlob.UploadText(helloText,accessCondition:
        AccessCondition.GenerateIfMatchCondition(orignalETag));
    }
    catch (StorageException ex)
    {
        if (ex.RequestInformation.HttpStatusCode == (int)HttpStatusCode.PreconditionFailed)
        {
            Console.WriteLine("Precondition failure as expected. Blob's orignal etag no longer matches");
            // TODO: client can decide on how it wants to handle the 3rd party updated content.
        }
        else
            throw;
    }  

Tallennustilan-palvelu sisältää myös muita ehdollinen otsikoita tuki kuten **Jos muokattu-kohdetta**, **Jos-muuttamattomia-jälkeen** ja **Jos-ei mitään-vastine** sekä niiden yhdistelmät. Katso lisätietoja MSDN-sivuston [Määrittäminen ehdollisen otsikot Blob-palvelutoimintoja varten](http://msdn.microsoft.com/library/azure/dd179371.aspx) .  

Seuraavassa taulukossa on yhteenveto säilö toiminnot, jotka Hyväksy ehdollinen otsikoiden esimerkiksi **Jos vastine** pyynnön ja, jotka palauttavat vastauksessa ETag-arvo.  

| Toiminto                | Palauttaa arvon säilö ETag | Hyväksyy ehdollinen otsikot |
|:-------------------------|:-----------------------------|:----------------------------|
| Luo säilö         | Kyllä                          | Ei                          |
| Hanki säilö ominaisuudet | Kyllä                          | Ei                          |
| Hanki säilö metatiedot   | Kyllä                          | Ei                          |
| Määritä säilö metatiedot   | Kyllä                          | Kyllä                         |
| Hanki säilö Käyttöoikeusluettelon        | Kyllä                          | Ei                          |
| Säilön Käyttöoikeusluettelo määrittäminen        | Kyllä                          | Kyllä (*)                     |
| Poista säilö         | Ei                           | Kyllä                         |
| Varaus-säilö          | Kyllä                          | Kyllä                         |
| Luettelon BLOB-objektit               | Ei                           | Ei                          |

(*) Välimuistiin SetContainerACL määrittämiä käyttöoikeuksia ja näiden oikeuksien päivitysten kestää 30 sekuntia leviäminen aikana mitä päivityksiä ei oteta yhtenäistämistä.  

Seuraavassa taulukossa on yhteenveto blob-toimintoja, Hyväksy ehdollinen otsikoiden esimerkiksi **Jos vastine** pyynnön ja, jotka palauttavat vastauksessa ETag-arvo.

| Toiminto           | Palauttaa arvon ETag | Hyväksyy ehdollinen otsikot           |
|:--------------------|:-------------------|:--------------------------------------|
| Blob-objektien valitseminen            | Kyllä                | Kyllä                                   |
| Hae Blob            | Kyllä                | Kyllä                                   |
| Hae Blob-ominaisuudet | Kyllä                | Kyllä                                   |
| Blob-ominaisuuksien määrittäminen | Kyllä                | Kyllä                                   |
| Hae Blob-metatiedot   | Kyllä                | Kyllä                                   |
| Määritä Blob-metatiedot   | Kyllä                | Kyllä                                   |
| Varaus-Blob (*)      | Kyllä                | Kyllä                                   |
| Tilannevedoksen Blob       | Kyllä                | Kyllä                                   |
| Blob-objektien kopioiminen           | Kyllä                | Kyllä (lähde- ja Blob-objektien) |
| Kopioi Blob-objektien Keskeytä     | Ei                 | Ei                                    |
| Blob-objektien poistaminen         | Ei                 | Kyllä                                   |
| Sijoita estäminen           | Ei                 | Ei                                    |
| Sijoita ruutuluettelo      | Kyllä                | Kyllä                                   |
| Hae ruutuluettelo      | Kyllä                | Ei                                    |
| Lisää sivu            | Kyllä                | Kyllä                                   |
| Hae alueet     | Kyllä                | Kyllä                                   |


(*) Varauksen Blob-objektien eivät muutu ETag-blob.  

### <a name="pessimistic-concurrency-for-blobs"></a>Pessimistinen samanaikainen BLOB varten
Lukitse Blob-objektien käyttöönsä, voit hankkia sen [varauksen](http://msdn.microsoft.com/library/azure/ee691972.aspx) . Kun hankit varaus, voit määrittää, kuinka kauan tarvitset varauksen: Tämä voidaan 15-60 sekuntia tai rajattoman joka summat yksityinen lukitus. Voit uusia rajallinen varauksen voit laajentaa ja minkä tahansa varauksen voi vapauttaa, kun olet valmis sitä. Blob-palvelun versiot rajallinen vuokrasopimukset automaattisesti, kun ne on voimassa.  

Vuokrasopimukset käyttöön eri synkronoinnin strategioita tuetaan, mukaan lukien poissulkeva kirjoittaminen / jaetut luku, poissulkeva kirjoittaminen / yksityisesti lukeminen ja kirjoittaminen jaetaan vain luku- oikeudet. Jos varaus tallennuspalvelu pakottaa yksityisesti kirjoituksia (valitseminen määrittäminen ja poistaminen toimintoja) kuitenkin varmistaa luettujen merkkikohtaisia edellyttää kehittäjä voi varmistaa, että kaikki asiakassovelluksissa käyttää varauksen-tunnus ja vain yksi asiakkaalle kerrallaan on on kelvollinen varauksen. Luku-toiminnoista, joita Älä sisällytä varauksen tunnus tuloksen jaetun lukuja.  

Seuraavat C#-koodikatkelman näkyy esimerkki hakee poissulkeva varausta 30 sekuntia blob-, päivitys Blob-objektien sisällön ja vapauttamalla sitten varauksen. Jos on jo kelvollinen varauksen blob yritettäessä hankkia uusi varaus-blob-palvelu palauttaa "HTTP (409) ristiriidan"-tilan tulos. Alla olevassa koodikatkelman käyttää **AccessCondition** objektin tärkeintä varauksen tiedot, kun se on päivitettävä blob storage palvelussa pyyntö.  Voit ladata koko otoksen: [Käyttämällä Azure-tallennustilan hallinta samanaikainen](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114).

    // Acquire lease for 15 seconds
    string lease = blockBlob.AcquireLease(TimeSpan.FromSeconds(15), null);
    Console.WriteLine("Blob lease acquired. Lease = {0}", lease);

    // Update blob using lease. This operation will succeed
    const string helloText = "Blob updated";
    var accessCondition = AccessCondition.GenerateLeaseCondition(lease);
    blockBlob.UploadText(helloText, accessCondition: accessCondition);
    Console.WriteLine("Blob updated using an exclusive lease");

    //Simulate third party update to blob without lease
    try
    {
        // Below operation will fail as no valid lease provided
        Console.WriteLine("Trying to update blob without valid lease");
        blockBlob.UploadText("Update without lease, will fail");
    }
    catch (StorageException ex)
    {
        if (ex.RequestInformation.HttpStatusCode == (int)HttpStatusCode.PreconditionFailed)
            Console.WriteLine("Precondition failure as expected. Blob's lease does not match");
        else
            throw;
    }  

Jos yrität kirjoitus-kiinteän Blob-objektien ilman kulkeva varauksen tunnus, pyynnön epäonnistuu 412 virhe. Huomaa, että jos varaus vanhenee ennen **UploadText** kutsumista, mutta edelleen välittää varauksen tunnus, pyynnön myös epäonnistuu **412** -virheen. Katso lisätietoja varauksen määräajan ajat-ja varauksen tunnukset [Varauksen Blob-objektien](http://msdn.microsoft.com/library/azure/ee691972.aspx) REST-ohjeista.  

Seuraavat toimenpiteet blob käyttää vuokrasopimukset pessimistisen hallintaan:  


-   Blob-objektien valitseminen
-   Hae Blob
-   Hae Blob-ominaisuudet
-   Blob-ominaisuuksien määrittäminen
-   Hae Blob-metatiedot
-   Määritä Blob-metatiedot
-   Blob-objektien poistaminen
-   Sijoita estäminen
-   Sijoita ruutuluettelo
-   Hae ruutuluettelo
-   Lisää sivu
-   Hae alueet
-   Tilannevedoksen Blob - varauksen ID valinnaiset, jos varaus
-   Kopioi Blob - tunnusta tarvitaan onko varaus kohde Blob-objektien varauksen
-   Keskeytä kopioi Blob - vuokrata ID-tunnusta tarvitaan, jos kohde Blob-objektien äärettömän varauksen
-   Varaus-Blob  

### <a name="pessimistic-concurrency-for-containers"></a>Pessimistinen samanaikainen säilöjen varten
Säilöt-vuokrasopimukset käyttöön saman synkronoinnin strategioita, aiotaan lisätä BLOB-(poissulkeva kirjoittaminen / jaetut luku, poissulkeva kirjoittaminen / yksityisesti lukeminen ja kirjoittaminen jaetaan / yksityinen lukea) kuitenkin toisin kuin BLOB storage-palvelua vain pakottaa merkkikohtaisia Poista toimintoja. Poista säilö, jossa on aktiivinen varauksen asiakkaan sisällytettävä aktiivinen varauksen tunnus Poista pyynnön kanssa. Kaikki säilön toiminnot onnistuvat kiinteän säilön ilman varauksen tunnuksen mukaan lukien jolloin niitä on jaettu toimintoja. Jos päivitys (hyllytetty tai Aseta) tai Lue toimintojen merkkikohtaisia tarvitaan sitten kehittäjät Varmista kaikkiin käyttää varauksen-tunnus ja että vain yksi kerrallaan asiakas on on kelvollinen varauksen.  

Säilön seuraavien toimintojen avulla vuokrasopimukset hallitaan pessimistisen:  

-   Poista säilö
-   Hanki säilö ominaisuudet
-   Hanki säilö metatiedot
-   Määritä säilö metatiedot
-   Hanki säilö Käyttöoikeusluettelon
-   Säilön Käyttöoikeusluettelo määrittäminen
-   Varaus-säilö  

Lisätietoja on artikkelissa:  

- [Ehdollinen otsikoiden määrittäminen Blob palvelutoiminnot](http://msdn.microsoft.com/library/azure/dd179371.aspx)
- [Varaus-säilö](http://msdn.microsoft.com/library/azure/jj159103.aspx)
- [Varaus-Blob](http://msdn.microsoft.com/library/azure/ee691972.aspx)

## <a name="managing-concurrency-in-the-table-service"></a>Samanaikainen taulukko-palvelun hallinta
Taulukko-palvelu käyttää Optimistinen samanaikainen tarkistaa oletustoiminnon, kun käsittelet kohteisiin, jossa että optimistisen tarkistuksia Määritä blob-palvelun toisin kuin. Muut taulukon ja blob-palveluiden välinen ero on, että vain voit hallita kohteiden samanaikainen toimintaa olisi blob-palvelun avulla voit hallita samanaikainen säilöjä ja BLOB-objektit.  

Voit käyttää optimistisen ja tarkista, jos toinen prosessi muokata kohteen, koska se hakee taulukon tallennuspalvelu voit näyttöön, kun taulukko-palvelu palauttaa kohteen ETag-arvo. Tämä toimenpide Jäsennys on seuraavanlainen:  

1.  Kohteen noutaminen taulukosta tallennuspalvelu, vastaus sisältää ETag-arvo, joka määrittää nykyisen tunnuksen liittyvät kyseisen kohteen tallennustilan-palvelussa.
2.  Kun päivität kohteen, Sisällytä olet saanut vaiheessa 1 pakollinen **Jos vastine** otsikossa, voit lähettää palveluun pyynnön ETag-arvo.
3.  Palvelun vertaa ETag ETag arvolla kohteen pyynnön.
4.  Jos yksikön nykyinen ETag-arvo on erilainen kuin pakollinen **Jos vastine** otsikossa pyynnössä ETag-palvelu palauttaa 412 virheen asiakas. Tämä ilmaisee asiakkaalle, että toinen prosessi on päivittänyt kohteen asiakkaan noutaa sen jälkeen.
5.  Jos yksikön nykyinen ETag-arvo on sama kuin ETag pakollinen **Jos vastine** otsikossa pyynnössä tai **Jos vastine** -otsikko sisältää yleismerkin (*), palvelun suorittaa toimintoa ja päivittää osoittamaan, että se on päivitetty kohteen ETag nykyarvon.  

Huomaa, että toisin kuin blob-palvelun taulukko-palvelu edellyttää asiakkaan sisällytettävien **Jos vastine** -otsikon päivityksen pyynnöt. On kuitenkin mahdollista voit pakottaa Ehdottomaan päivittää (viimeinen writer wins strategia) ja ohittaa samanaikainen tarkistukset, jos asiakkaan määrittää **Jos vastine** otsikon pyynnön yleismerkki (*).  

Seuraavat C#-koodikatkelman näkyy asiakas-yritys, joka on aiemmin luotu tai hakea ottaa henkilön sähköpostiosoite, joka on päivitetty. Alkuperäisen lisääminen tai hakea toiminto tallentaa asiakkaan objektin ETag-arvo ja koska otosten käyttää saman objektiesiintymää, kun se suoritetaan korvaustoimintoa, se lähettää automaattisesti ETag-arvo takaisin taulukon palvelun ottaminen käyttöön palvelun tarkistavan samanaikainen virheitä. Jos toinen prosessi on päivittänyt tallennustilaan kohde-palvelu palauttaa HTTP 412 (ennen epäonnistui)-tila-sanoma.  Voit ladata koko otoksen: [Käyttämällä Azure-tallennustilan hallinta samanaikainen](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114).

    try
    {
        customer.Email = "updatedEmail@contoso.org";
        TableOperation replaceCustomer = TableOperation.Replace(customer);
        customerTable.Execute(replaceCustomer);
        Console.WriteLine("Replace operation succeeded.");
    }
    catch (StorageException ex)
    {
        if (ex.RequestInformation.HttpStatusCode == 412)
            Console.WriteLine("Optimistic concurrency violation – entity has changed since it was retrieved.");
        else
            throw;
    }  

Erikseen käytöstä samanaikainen-valintaruudun, sinun kannattaa asettaa **työntekijän** objektin **ETag** -ominaisuuden "*" ennen kuin suoritat korvaustoimintoa.  

    customer.ETag = "*";  

Seuraavassa taulukossa kuvataan, miten taulukon kohteen toimintojen käyttää ETag arvot:

| Toiminto                | Palauttaa arvon ETag | Jos vastine pyynnön otsikossa edellyttää |
|:-------------------------|:-------------------|:---------------------------------|
| Kyselyn yksiköt           | Kyllä                | Ei                               |
| Lisää kohde            | Kyllä                | Ei                               |
| Päivitä kohteen            | Kyllä                | Kyllä                              |
| Kohteen yhdistäminen             | Kyllä                | Kyllä                              |
| Poista kohde            | Ei                 | Kyllä                              |
| Lisää tai korvaa kohteen | Kyllä                | Ei                               |
| Lisää tai kohteen yhdistäminen   | Kyllä                | Ei                               |

Huomaa, että Älä **Lisää tai korvaa kohteen** ja **Lisää tai yhdistää kohteen** toimintoja *ei* tehdä samanaikainen kaikki tarkistukset, koska ne Älä lähetä taulukon palveluun ETag-arvo.  

Yleinen kehittäjät taulukoiden kannata optimistisen skaalattava sovellusten kehittämisessä. Jos Pessimistinen lukitseminen tarvita, yksi tapa kehittäjät voi kestää kun taulukoiden käyttäminen on määrittää nimetyn blob kullekin taulukolle ja yrität käyttää varaus blob ennen taulukon toiminta. Tämän menetelmän edellyttävät sovellus varmistaaksesi, että kaikki tiedot access-polut saada käyttöönsä ennen taulukon toiminta. Huomaa myös, pienin varauksen aika on 15 sekunnin, jossa pyydetään skaalattavuus varovainen huomioon.  

Lisätietoja on artikkelissa:  

- [Kohteiden toimenpiteet](http://msdn.microsoft.com/library/azure/dd179375.aspx)  

## <a name="managing-concurrency-in-the-queue-service"></a>Samanaikainen jonon-palvelun hallinta
Yksi tilanne on mitä samanaikainen huolta asetetaan jonoon-palvelussa on kohtaa, johon useita asiakkaita haetko viestit-jonossa. Kun viestin haetaan jonossa, vastaus sisältää viesti ja pop vastaanotto-arvo, joka tarvitse poistaa viestin. Viestiä ei poisteta automaattisesti jonossa, mutta sen jälkeen, kun se on noudettu, se ei näy muiden asiakkaiden visibilitytimeout-parametrin määrittämän aikavälillä. Asiakas, joka hakee sanoman on tarkoitus poistaa viestin, kun se on suoritettu ja aika TimeNextVisible ennen vastauksen, jotka on laskettu osa perusteella visibilitytimeout-parametrin arvon. Visibilitytimeout arvo lisätään aika, jolloin viesti on noudettu määrittää TimeNextVisible arvon.  

Jonopalvelu ei ole Optimistinen tai Pessimistinen samanaikainen tuki ja tämän syy asiakkaiden käsitellään viestejä, jotka on haettu jonon varmistaa, viestit käsitellään idempotent tavalla. Viimeinen writer wins strategia käytetään päivityksen toiminnot, kuten SetQueueServiceProperties, SetQueueMetaData, SetQueueACL ja UpdateMessage.  

Lisätietoja on artikkelissa:  

- [Jonon palvelun REST-Ohjelmointirajapinnalla](http://msdn.microsoft.com/library/azure/dd179363.aspx)
- [Viestien noutaminen](http://msdn.microsoft.com/library/azure/dd179474.aspx)  

## <a name="managing-concurrency-in-the-file-service"></a>Samanaikainen tiedosto-palvelun hallinta
Tiedosto-palvelua voi käyttää kaksi eri protokollaa päätepisteet SMB- ja muut. REST-palvelu ei ole tuki Optimistinen lukitseminen tai lukituksen Pessimistinen ja kaikki päivitykset seuraa viimeisen writer wins strategia. SMB-ohjelmat, joita ottaa tiedostoresurssit voidaan hyödyntää tiedoston järjestelmän lukitsemalla järjestelmiä hallita jaettujen tiedostojen – mukaan lukien pätevyys suorittaa Pessimistinen lukitus. Kun SMB-asiakas Avaa tiedoston, se määrittää tiedoston käyttö ja jakaminen tilassa. "Kirjoita" tai "Kirjoitus" jaettua tiedostoresurssia tila sekä "ei mitään-asetus tiedoston käyttöoikeus johtaa tiedoston lukita SMB-asiakas, ennen kuin se on suljettu. Jos REST-toiminto on liikaa SMB-asiakas, joiden on lukita tiedoston tiedostoa REST-palvelu palauttaa tilakoodin 409 (ristiriita) virhekoodi SharingViolation.  

SMB-asiakas avatessa Poista tiedoston se merkitsee tiedoston, kuin poistamista, kunnes kaikki SMB-asiakas odottavat kahvat tiedostoon on suljettu. Samalla, kun tiedosto on merkitty poistamista odottavat, minkä tahansa tiedoston muiden laskutoimituksen palauttaa tilakoodin 409 (ristiriita) virhekoodi SMBDeletePending. Tilakoodin 404 (ei löydy) palautetaan ei ole, koska se on mahdollista SMB-asiakkaiden ennen tiedoston sulkeminen odottaa poistamista merkinnän poistaminen. Toisin sanoen tilakoodin 404 (ei löydy) odotetaan vain, kun tiedosto on poistettu. Huomaa, että kun tiedosto on SMB odottavat Poista tila, se ei sisällytetä tiedostojen luettelon tuloksista. Huomaa, että muut Poista tiedosto ja muut poistaa kansion toiminnot on vahvistettu atomically ja ei aiheuttaa odottavia Poista tila.  

Lisätietoja on artikkelissa:  

- [Lukitsee tiedoston hallinta](http://msdn.microsoft.com/library/azure/dn194265.aspx)  

## <a name="summary-and-next-steps"></a>Yhteenveto- ja seuraavat vaiheet
Microsoft Azure tallennuspalvelu on suunniteltu monimutkaisia online-sovellusten tarpeiden ilman pakottaminen kehittäjät havaittu tai tietoturvaan liittyvien ongelmien tärkeimmät perustiedot, kuten samanaikainen ja tietojen kelpoisuus, ne on tuotu, jos haluat tehdä myönnetty.  

Viitattu tässä blogissa valmis malli-sovelluksen:  

- [Samanaikainen käyttämällä Azuren tallennustilaan - sovelluksen malli hallinta](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114)  

Lisätietoja on artikkelissa Azure-tallennustilan:  

- [Microsoft Azure-tallennustilan kotisivu](https://azure.microsoft.com/services/storage/)
- [Azure-tallennustilan esittely](storage-introduction.md)
- Tallennustilan for [Blob](storage-dotnet-how-to-use-blobs.md), [taulukko](storage-dotnet-how-to-use-tables.md)tai [olevien](storage-dotnet-how-to-use-queues.md) [tiedostojen](storage-dotnet-how-to-use-files.md) käytön aloittaminen
- Tallennustilan arkkitehtuuri – [Azure tallennustilan: erittäin käytettävissä Pilvitallennuspalveluun kanssa vahva yhdenmukaisuuden](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)

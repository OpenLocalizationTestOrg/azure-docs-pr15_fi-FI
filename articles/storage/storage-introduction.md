<properties
    pageTitle="Johdanto tallennustilan | Microsoft Azure"
    description="Yleiskatsaus Azuren tallennustilaan Microsoftin online tietojen tallentaminen pilveen. Opettele käyttämään sovellusten paras käytettävissä cloud tallennustilan ratkaisu."
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
    ms.topic="get-started-article"
    ms.date="10/25/2016"
    ms.author="tamram"/>

# <a name="introduction-to-microsoft-azure-storage"></a>Johdanto Microsoft Azuren tallennustilaan

## <a name="overview"></a>Yleiskatsaus

Azure-tallennustila on cloud tallennustilan ratkaisu Moderni sovelluksia, jotka käyttävät kestävyyttä, käytettävyys ja skaalattavuus asiakkaidensa tarpeisiin. Tässä artikkelissa, kehittäjille, IT-ammattilaisille ja liiketoimintapäätös lukemalla valmistajat tietoja:

- Azure-tallennustila on ja miten niihin hyödyntää se pilvipalveluun, mobiili-palvelin ja työpöytäsovellukset
- Millaisia tietoja voit tallentaa Azuren tallennustilaan palveluihin: blob (objekti) tietojen, NoSQL Taulukkotiedot, sanomien ja tiedostoresursseihin.
- Voi käyttää tietojasi Azuren tallennustilaan hallintatavat
- Miten Azuren tallennustilaan tietojen tehdään kestävät redundancy ja replikoinnin kautta
- Miten jatkaa seuraava ensimmäisen Azuren tallennustilaan-sovelluksen luonnissa

Lisämääritykset kanssa Azuren tallennustilaan nopeasti, on artikkelissa [Azure-tallennustilan päättymässä käytön aloittaminen](storage-getting-started-guide.md).

Lisätietoja työkaluista kirjastot ja muut resurssit käsittelyyn Azure-tallennustilan, katso [Seuraavat vaiheet](#next-steps) alla.

## <a name="what-is-azure-storage"></a>Mikä on Azuren tallennustilaan?

Cloud tietojenkäsittely avulla uusia skenaarioita sovellusten edellyttävän skaalattava, kestävät ja erittäin käytettävissä tallennustilaa niiden tietojen – joka on juuri, miksi Microsoft on kehittänyt Azure-tallennustilan. Lisäksi tekeminen mahdollista kehittäjät voivat luoda uusia skenaarioita tukemaan suurissa sovelluksia Azuren tallennustilaan sekä tallennustilan foundation varten Azuren näennäiskoneiden, voit sen vakauden edelleen testament.

Azuren tallennustila on erittäin skaalattava, joten voit tallentaa ja prosessi satoja, teratavua tietojen tukemaan big datasta käyttötavoista vaatii tiede-, analyysi- ja media-sovellukset. Tai voit tallentaa small business-sivuston tarvittavat tiedot jonkin verran. Sitä tarpeisiisi kuuluvat maksat vain tallennettavien tietojen. Azure-tallennustilan tällä hetkellä tallentaa trillions yksilöllinen asiakkaan objektien kymmenlukuun, ja käsittelee pyynnöt sekunnissa miljoonia keskimäärin.

Azure-tallennustila on joustavasti, niin voit suunnitella yleinen suurelle käyttäjäryhmälle sovellukset ja skaalata näiden sovellusten tarvittaessa - sekä etäisyys tallennettuja tietoja, ja sitä vastaan pyyntöihin määrä. Voit maksaa vain, jos, voit käyttää ja vain silloin, kun sitä käytetään.

Azure tallennuspaikka automaattinen jakaminen järjestelmä, joka automaattisesti kuormituksen-saldojen tietojen liikenne perusteella. Tämä tarkoittaa, että-sovelluksen Suurenna vaatimukset kuin Azuren tallennustilaan automaattisesti varaa täyttävän ne tarvittavat resurssit.

Azure-tallennustila on käytettävissä olevat missä tahansa maantieteellisessä, minkä tahansa sovelluksen, onko se pilveen työpöydällä, paikallisen palvelimessa tai Mobile tai Tablet PC-laitetta. Voit käyttää Azuren tallennustilaan mobile tilanteissa, jossa sovellus tallentaa tietojen alijoukon laitteessa ja synkronointi pilveen koko tietojoukko.

Azure-tallennustilan tukee asiakkaita käyttämällä helposti kehittämiseen monipuolisen joukko käyttöjärjestelmät (myös Windows- tai Linux) ja eri ohjelmoinnin kielten (kuten .NET, Java, Node.js, Python, Ruby, PHP ja C++ ja mobile ohjelmoinnin kielet). Azure-tallennustilan paljastaa myös tietoresursseihin yksinkertainen REST API, jotka ovat käytettävissä minkä tahansa asiakas voi lähettää ja vastaanottaa tietoja HTTP/HTTPS kautta.

Azure Premium tallennustilan toimittaa tehokas, pieni viive levyn i/o-Azuren näennäiskoneiden on tehostettu työmääriä tuki. Azure Premium tallennusta voit liittää useita pysyvien tietojen levyjä virtual machine ja määrittää niitä suorituskyvyn vaatimukset täyttävän. Tietoja kunkin levyn varmuuskopioidaan Suoritettaessa levyn Azure Premium tallennustilaan i/o parhaan mahdollisen suorituskyvyn. Katso [Premium tallennustilan: tehokas tallennustila Azure virtuaalikoneen työmääriä](storage-premium-storage.md) lisätietoja.

## <a name="introducing-the-azure-storage-services"></a>Azure-tallennustilan palvelut esittely

Azure-tallennustilan sisältää seuraavat neljä palvelut: Blob storage, taulukkotallennus, jonossa tallennustilan ja tallentamista.

- Blob-objektien tallennustilaan tallentaa rakenteeton objektitiedot. Blob voi olla mikä tahansa tekstin tai binaarinen tietoja, kuten asiakirjan, mediatiedosto tai sovelluksen asennusohjelma. Blob-objektien tallennustilaan kutsutaan myös objektin säilön.
- Taulukkotallennus tallentaa rakenteellisia tietojoukkoja. Taulukkotallennus on NoSQL avain-määritteen tietosäilö, joka mahdollistaa nopeasti ja suuria määriä tietoja nopeasti käyttöönsä.
- Jonon tallennustila on luotettava messaging, työnkulkua käsitellään ja välisen cloud services-osia.
- Tiedostojen tallentamisesta on jaettu tallennustila vanhojen sovellusten vakio SMB-protokollan avulla. Azure-virtuaalikoneissa ja pilvipalveluihin jakaa tiedoston tiedot sovelluksen osia kautta liitetyn osakkeet ja paikallisen sovellukset voivat käyttää tiedoston tietojen Jaa tiedosto REST API-palvelun kautta.

Azure-tallennustilan tilin on suojattu tili, jonka kautta pääset käsiksi services Azuren tallennustilaan. Tallennustilan tilisi on yksilöllisen nimitilan tallennustilan resurssit. Alla olevassa kuvassa näkyy tallennustilan tilin Azure tallennustilan resursseista väliset suhteet:

![Azure-tallennustilan resurssit](./media/storage-introduction/storage-concepts.png)

[AZURE.INCLUDE [storage-account-types-include](../../includes/storage-account-types-include.md)]

[AZURE.INCLUDE [storage-versions-include](../../includes/storage-versions-include.md)]

## <a name="blob-storage"></a>Blob-objektien tallennustilaan

Käyttäjät, joilla rakenteeton objektin suuria tietomääriä tallentamiseen pilveen Blob-objektien tallennustilaan on edullinen ja skaalattava ratkaisu. Voit käyttää Blob-objektien tallennustilaan tallentaa sisältöä seuraavasti:

- Asiakirjojen
- Sosiaaliset tietoja, kuten kuvia, videoita, musiikkia ja blogeihin
- Varmuuskopioiden tiedostojen, tietokoneiden, tietokantojen ja laitteet
- Kuvien ja tekstin verkkosovellusten
- Cloud sovellusten kokoonpanotietoja
- Suuri tietoja, kuten lokit ja muiden suurten tietojoukkoja

Jokaisen blob on järjestetty säilön. Säilöt ovat myös käteviä suojauskäytäntöjen liittäminen ryhmiä. Tallennustilan tilin voi olla jokin muu luku säilöjä ja säilön voi olla enintään 500 TT kapasiteetin raja-tallennustilan tilin BLOB-objektit, minkä tahansa määrän.  

BLOB storage tarjouksia kolmenlaisia BLOB-objektit, estä BLOB-objektit, liittää BLOB-objektit ja sivun BLOB-objektit (levyjä).

- Estä BLOB-objektit on tarkoitettu streaming ja tallentamisesta pilveen objektit ja on hyvä projektiin asiakirjoja, mediatiedostoja, varmuuskopioiden jne.
- Liitä BLOB muistuttavat estä BLOB-objektit, mutta se on optimoitu Lisää toimintoja. Liitä-blob voidaan päivittää vain, jos lisäät uuden kohteen loppuun. Liitä BLOB on hyvä vaihtoehto, kuten kirjaaminen, jossa uudet tiedot tarvitsee kirjoittaa vain blob loppuun käyttötilanteita.
- Sivun BLOB-objektit on optimoitu edustava IaaS levyille ja tukeminen satunnaisia kirjoittaa ja voi olla enintään 1 TT: n kokoisia. Azure virtuaalikoneen verkon liittyvien levy on tallennettu sivun Blob-objektien Näennäiskiintolevyn IaaS.

Jossa verkon rajoitukset tehdä palvelimeen tai tietojen lataaminen Blob-objektien tallennustilaan epärealistisia ‑toiminto hyvin suuria tietojoukkoja voidaan lähettää Microsoftille Tuo ja vie tiedot suoraan tietokeskuksen kiintolevylle. Katso [käyttäminen tietojen Blob Storage siirtämiseen Microsoft Azure Tuo/Vie-palvelun](storage-import-export-service.md).

## <a name="table-storage"></a>Taulukkotallennus

Nykyaikainen sovellusten vaatimus Microsoft Data usein suurempi skaalattavuus ja joustavammin kuin edellinen sukupolvien pakollinen-ohjelmiston kanssa. Taulukkotallennus on käytettävissä, erittäin skaalattava tallennustilan niin, että sovelluksesi automaattisesti skaalata täyttävän käyttäjän tarvittaessa. Taulukkotallennus on Microsoftin NoSQL avain/määrite säilö – se on schemaless rakenne, jolloin eri perinteinen relaatiotietokannoista. Schemaless tietosäilö on helppo mukauttaa tietojen sovelluksen evolve tarpeisiin. Taulukkotallennus on helppo käyttää, jotta sovelluskehittäjät voivat luoda sovelluksia nopeasti. Tietojen käyttäminen on nopea ja edullinen kaikenlaisia sovelluksia varten.  Taulukkotallennus on yleensä kustannukset merkittävästi pienempi kuin perinteisen SQL-samalla tietomääristä.

Taulukkotallennus on avain määrite säilö, mikä tarkoittaa, että jokaisen arvon taulukosta tallennetaan kirjoitetun ominaisuuden nimi. Ominaisuuden nimi voidaan suodattaa ja määrittämällä valintaehtojen. Sivustokokoelman ominaisuuksien ja niiden arvojen sisältää kohteen. Taulukkotallennus on schemaless, kaksi saman taulukon kohteita voi olla eri hallintatyökalua ominaisuudet ja nämä ominaisuudet voivat olla erilaisia.

Taulukkotallennus avulla voit tallentaa joustavat tietojoukkoja, kuten verkkosovellusten, osoitteistoja tai laitteen tietojen muuntyyppisen metatiedot, joilla palvelu vaatii käyttäjätiedot.  Voit tallentaa minkä tahansa kohteiden määrän taulukon ja tallennustilan tilin voi olla jokin muu luku taulukoiden kapasiteetin raja-tallennustilan tilin ylöspäin.

BLOB-objektit ja olevien, kuten kehittäjät voivat hallita ja access-taulukko käyttämällä vakio muiden tallennustilan protokollia, mutta taulukkotallennus tukee myös alijoukkoa OData-protokolla yksinkertaistaminen Lisäasetukset kyselyt ominaisuuksia ja ottamalla käyttöön JSON- ja AtomPub (XML-pohjainen) muotoja.

Tämän päivän Internet-pohjaisten sovellusten NoSQL tietokantoja, kuten taulukkotallennus tarjoavat Suositut vaihtoehtoinen perinteinen relaatiotietokantoja.

## <a name="queue-storage"></a>Jonon tallennustila

-Sovellusten akselille suunnitteleminen sovelluksen osat ovat usein erillisen, niin, että ne voi skaalata erikseen. Jonon tallennustilan on luotettava tekstiviesti ratkaisu asynkronisen sovelluksen osia väliseen viestintään, onko ne ovat käytössä pilvipalvelussa, työpöydällä, paikallisen palvelimessa tai mobiililaitteessa. Jonon tallennustilan tukee myös prosessin työnkulkujen kehittämistä sekä asynkroninen tehtävien hallinnasta.

Tallennustilan tilin voi olla minkä tahansa olevien määrän. Jono voi olla jokin muu luku viestien kapasiteetin raja-tallennustilan tilin ylöspäin. Yksittäisten viestien voi olla enintään 64 Kilotavun kokoinen.

## <a name="file-storage"></a>Tiedostojen tallennus

Azure tiedostojen tallentamisesta on pilvipohjainen SMB tiedostoresurssit, niin, että voit siirtää vanhat sovellukset, jotka perustuvat tiedostoresurssit Azure voit nopeasti ja ilman kallista kirjoittaa uudelleen. Azure-tiedostojen tallentamisesta ja Azure-virtuaalikoneissa tai pilvipalveluihin sovellusten voi ottaa käyttöön jaetun tiedoston pilvipalvelussa, aivan kuin työpöydän sovelluksen liittää tyypillinen SMB-Jaa. Jokin muu luku sovelluksen osia, voit ottaa käyttöön ja käyttää jaettua tiedostoresurssia tallennustilan samanaikaisesti.

Jaetun tiedoston tallennustila on vakio SMB-tiedostoresurssin, Azure-sovellukset voi käyttää tietoja Jaa tiedostojärjestelmää i/o-ohjelmointirajapinnan kautta. Kehittäjät vuoksi voidaan hyödyntää niiden aiemmin luotua koodia ja taidot siirtää olemassa olevat sovellukset. IT-ammattilaisille käyttää PowerShellin cmdlet-komennot, voit luoda ja ottaa tiedostoresurssit tallennustilan hallinta osana Azure sovellusten hallinta.

Muita Azure tallennustilan-palveluja, kuten tiedostojen tallentamisesta paljastaa REST API jaettuun tietojen käyttämistä varten. Paikallinen-sovellukset voivat soittaa tiedostosäilön REST API jaetun tiedoston tietojen käyttämistä varten. Tällä tavalla yrityksen halutessasi siirtää vanhat Azure-sovelluksia ja edelleen oman organisaation sisällä suoritettavien muita käyttäjiä. Huomaa, että käyttöönottaminen jaetun tiedoston on vain sovellukset Azure; käynnissä paikallisen sovellus voi käyttää vain jaettua tiedostoresurssia REST-Ohjelmointirajapinnalla kautta.

Jaetut sovellukset käyttää myös tiedostosäilön tallentamiseen ja jakamiseen sovelluksen hyödyllisiä tietoja, kehittäminen ja testauksen työkalut. Esimerkiksi sovellus voi tallentaa tiedostojen ja diagnostiikkatiedot, kuten lokit, arvot ja kaatumisen kirjoittaa tallennustilan jakaminen niin, että ne ovat käytettävissä useita näennäiskoneiden tai roolit-tiedostossa. Kehittäjät ja järjestelmänvalvojat voivat tallentaa apuohjelmat, jotka tarvitsemiaan luoda tai hallita sovelluksen tallennustilan tiedostoresurssi, joka on käytettävissä kaikkien osien sen sijaan, että asennat niitä kaikkia virtuaalikoneen tai roolin esiintymä.

## <a name="access-to-blob-table-queue-and-file-resources"></a>Blob, taulukkoon, jonossa ja tiedostoresursseihin

Oletusarvon mukaan tallennustilan tilin omistajan voi käyttää tallennustilan tilin resursseja. Tietojen suojaukseen todennus vastaan resurssit-tilisi jokaisen pyynnön. Todennus on riippuvainen jaettu avain malli. BLOB myös on määritetty tukemaan Anonyymi todentaminen.

Tallennustilan tilin määritetään kahden yksityinen pikanäppäinten luominen, joita käytetään todennusta varten. Ottaa kahta näppäintä varmistaa sovelluksen säilyvät, kun säännöllisesti uudelleen näppäimet suojaus yleisiin hallintaa käytäntönä.

Jos tarvitset käyttäjille ohjaa tallennustilan resurssien käytön ja sitten voit luoda jaettu käyttö allekirjoituksen. Jaetun access-allekirjoitus (SAS) on tunnuksen, joka lisätään URL-osoitteeseen, joka mahdollistaa valtuutetun tallennustilan resurssin käyttöoikeutta. Kuka tahansa, jolla on tunnuksen käyttää se osoittaa käyttöoikeuksilla, se määrittää ajanjakson, että se on kelvollinen resurssi. Version 2015-04-05 alkaen Azuren tallennustilaan tukee kahdenlaisia jaettua käyttöä allekirjoitukset: service Suojaussidosten ja tilin Suojaussidokset.

Palvelun SAS Delegoi vain yhdessä liittyviä palveluja resurssin käyttöoikeutta: Blob, jonossa, taulukko tai tiedosto-palvelun.

Tilin SAS Delegoi resurssien yhdessä tai useammassa liittyviä palveluja. Palvelun tason toiminnot, jotka eivät ole käytettävissä palvelussa SAS voit edustajakäyttöoikeudet. Voit myös delegoida access lukeminen ja kirjoittaminen sekä poistaa blob säilöjen, taulukoita tai olevien, jotka eivät ole sallittuja SAS-palvelun kanssa jaettujen tiedostojen toimenpiteet.

Lopuksi voit määrittää, että säilön ja sen BLOB tai tietyn Blob-objektien ovat käytettävissä yleisön. Kun valitset säilö tai blob on julkinen, kuka tahansa voi lukea sen nimettömänä; tarkistusta ei vaadita.  Julkinen säilöjen ja BLOB-objektit on hyötyä paljastaa resurssit, kuten media ja asiakirjoja, joita isännöidään sivustoon.  Kun haluat vähentää verkon latenssin yleinen käyttäjäryhmälle, voit välimuistiin käytetyt sivustot ja Azure CDN blob-tiedot.

Katso jaetun access-allekirjoitukset Saat lisätietoja [Käyttämällä jaetun Access allekirjoitukset (SAS)](storage-dotnet-shared-access-signature-part-1.md) . Saat lisätietoja suojatun yhteyden tallennustilan tilin [hallinta anonyymi lukuoikeus säilöjä ja BLOB](storage-manage-access-to-resources.md) - ja [Azure-tallennustilan palveluiden todennus](https://msdn.microsoft.com/library/azure/dd179428.aspx) .

## <a name="replication-for-durability-and-high-availability"></a>Replikoinnin kestävyyttä ja suuri käytettävyys

Microsoft Azure-tallennustilan tilin tiedot aina replikoida kestävyyttä ja suuren käytettävyyden varmistamiseksi. Replikoinnin Kopioi tietosi joko samalla tietokeskuksen tai toisen tiedot-käyttöoikeudet, riippuen replikoinnin minkä vaihtoehdon valitset. Replikoinnin suojaa tietoja ja säilyttää sovelluksen ylös-ajankäytön lyhytkestoisia laitteisto yhteydessä. Jos tiedoissa on replikoida toisen tietokeskuksen, joka suojaa vastaan kriittinen virhe, valitse ensisijainen sijainti tiedoissa.

Replikoinnin varmistaa, tallennustilan tilin vastaa [Palvelun tason palvelutasosopimusta (SLA) tallennuksessa](https://azure.microsoft.com/support/legal/sla/storage/) jopa menettämisen tapahtuessa virheet. Katso lisätietoja Azure-tallennustilan SLA takaa kestävyyttä ja käytettävyys. 

Kun luot tallennustilan tilin, voit valita seuraavat Replikointiasetukset:  

- **Paikallisesti tarpeettomat storage (LRS).** Paikallisesti ylimääräinen ylläpitää tietojen kolme kopiota. LRS on replikoida yksittäisen tietokeskuksen yhden alueen sisällä kolme kertaa. LRS suojaa tietoja Normaali laitteisto, mutta ei yhtä center virhe.  

    LRS on tarjottujen alennus. Suurin varmistamiseksi on suositeltavaa, että käytät geo tarpeettomat tallennustilaa, jäljempänä kuvatulla tavalla.


- **Vyöhykkeen tarpeettomat storage (ZRS).** Vyöhykkeen ylimääräinen ylläpitää tietojen kolme kopiota. ZRS on replikoida kolme kertaa kahden tai kolmen tilat, vain yhden alueen sisällä tai eri kummallakin alueella yli antamisen suurempi kuin LRS kestävyyttä. ZRS varmistaa, että tiedot ovat kestävät yhden alueen.  

    ZRS sisältää viimeinen myyntipäivä ylemmälle tasolle kuin LRS; kuitenkin suurin varmistamiseksi, on suositeltavaa, että käytät geo tarpeettomat tallennustilaa, jäljempänä kuvatulla tavalla.  

    > [AZURE.NOTE] ZRS on tällä hetkellä käytettävissä vain estä BLOB-objektit ja on vain tukee versiot 2014-02 – 14 ja uudemmissa versioissa.
    >
    > Kun olet luonut tilin tallennustilan ja valittu ZRS, et voi muuntaa sitä muuntyyppisen replikointia avulla tai päinvastoin.

- **Geo tarpeettomat storage (GRS)**. GRS ylläpitää tietojen kuusi kopioita. GRS tiedot on replikoida kolme kertaa ensisijainen alueella ja on myös replikoida kolme kertaa toissijainen alueen satoja Mailia tarjoaa parhaan viimeinen myyntipäivä ensisijainen alueen ulkopuolella. Jos ensisijainen alue-palvelussa, Azuren tallennustilaan on toissijainen alueen automaattisesti. GRS varmistaa, että tiedot ovat kestävät kaksi eri alueilla.

    Ensisijaisen ja toissijaisen parit alueittain tietoja on artikkelissa [Azure alueet](https://azure.microsoft.com/regions/).

- **Lukuoikeudet geo tarpeettomat storage (RA GRS)**. Lukuoikeudet geo tarpeettomat tallennustilan kopioi tietojen toissijainen maantieteellisen sijainnin ja sisältää myös tietojen toissijainen sijainnissa lukuoikeudet. Lukuoikeudet geo tarpeettomat tallennustilan avulla voit käyttää ensisijaisen tai toissijaisen sijainti tiedoissa silloin, kun yhdessä sijainnissa ei ole käytettävissä. Lukuoikeudet geo tarpeettomat tallennustila on oletusarvoisesti tallennustilan tilin oletusasetus luomisen yhteydessä. 

    > [AZURE.IMPORTANT] Voit muuttaa sitä, kuinka tiedot on replikoida jälkeen tallennustilan tili on luotu, ellei määritetty ZRS, kun olet luonut tilin. Huomaa kuitenkin, että voi syntyä kustannukset, jos vaihdat LRS GRS tai RA GRS erikseen lisätiedot-siirto.

Saat lisätietoja tallennustilan Replikointiasetukset [Azuren tallennustilaan replikoinnin](storage-redundancy.md) .

Alla on tietoja tallennustilan tilin replikoinnin, katso [Azure-tallennustilan hinnat](https://azure.microsoft.com/pricing/details/storage/). Saat lisätietoja siitä, mitkä palvelut ovat käytettävissä kunkin alueen [Azure-alueita](https://azure.microsoft.com/regions/#services) .

Lisätietoja arkkitehtuuri kestävyyttä Azuren tallennustilaan kanssa-kohdassa [SOSP paperi - Azuren tallennustilaan: A erittäin käytettävissä Pilvitallennuspalveluun kanssa vahvan yhdenmukaisuuden](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx).


## <a name="transferring-data-to-and-from-azure-storage"></a>Tietojen siirtäminen ja sieltä pois Azure-tallennustilan

Komentorivin AzCopy-apuohjelman avulla voit kopioida blob, tiedosto ja taulukkotiedot tallennustilan tilisi tai tallennustilan tilien välillä. Lisätietoja on kohdassa [tiedonsiirron AzCopy komentorivivalitsimet-apuohjelman avulla](storage-use-azcopy.md) .

AzCopy rakentuu [Azure tietojen siirtämistä kirjastoon](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement/), joka on tällä hetkellä käytettävissä esikatselu.

Azure Tuo/Vie-palvelun avulla on helppo blob-tietojen tuominen tietojen tuominen / vieminen blob storage-tilisi kautta lähetettävä Azure tietokeskuksen kiintolevylle DVD-levyllä. Lisätietoja Tuo/Vie-palvelusta on artikkelissa [Microsoft Azure Tuo/Vie palvelun Blob-objektien tallennustilaan siirtää tietoja](storage-import-export-service.md).

## <a name="pricing"></a>Hinnat

[AZURE.INCLUDE [storage-account-billing-include](../../includes/storage-account-billing-include.md)]

## <a name="storage-apis-libraries-and-tools"></a>Tallennustilan API, kirjastojen ja työkalut

Azure tallennustilan resursseja voi käyttää minkä tahansa kielen, voit tehdä HTTP/HTTPS-pyyntöjen perusteella. Lisäksi Azuren tallennustilaan on Suositut kielet-ohjelmoinnin kirjastoissa. Nämä kirjastot yksinkertaistaa monia käsitteleminen Azuren tallennustilaan käsittely tiedot, kuten synkronoidusti ja asynkroninen kutsu, jonottaminen toimintoja, poikkeuksen hallinta, automaattinen uudelleenyritykset toiminnallisia toiminta ja niin edelleen. Kirjastojen ovat tällä hetkellä käytettävissä seuraavat kielet ja ympäristöjen muiden putkijohto kanssa:

### <a name="azure-storage-data-services"></a>Azure-tallennustilan tietopalvelut

- [Tallennustilan Services REST-Ohjelmointirajapinnalla](http://msdn.microsoft.com/library/azure/dd179355.aspx)
- [Tallennustilan asiakkaan kirjaston .NET, Windows Phone-ja Windows Runtime](https://www.nuget.org/packages/WindowsAzure.Storage/)
- [Tallennustilan asiakkaan kirjaston C++](https://github.com/Azure/azure-storage-cpp)
- [Tallennustilan asiakkaan kirjaston Java-/ androidille](/develop/java/)
- [Tallennustilan Node.js asiakkaan kirjasto](http://dl.windowsazure.com/nodestoragedocs/index.html)
- [Tallennustilan asiakkaan kirjaston PHP](/develop/php/)
- [Ruby tallennustilan asiakkaan kirjastossa](/develop/ruby/)
- [Tallennustilan Python asiakkaan kirjasto](/develop/python/)
- [Tallennustilan PowerShell 1.0 cmdlet-komennot](https://msdn.microsoft.com/library/azure/mt269418.aspx)

### <a name="azure-storage-management-services"></a>Azure-tallennustilan hallinta-palvelut

- [Tallennustilan resurssin tarjoajan REST API-viittaus](https://msdn.microsoft.com/library/azure/mt163683.aspx)
- [.NET tallennustilan resurssin tarjoajan asiakkaan kirjasto](https://msdn.microsoft.com/library/azure/mt131037.aspx)
- [PowerShell 1.0 tallennustilan resurssin tarjoajan cmdlet-komennot](https://msdn.microsoft.com/library/azure/mt607151.aspx)
- [Tallennustilan Service Management REST API (perinteinen)](https://msdn.microsoft.com/library/azure/ee460790.aspx)

### <a name="azure-storage-data-movement-services"></a>Azure-tallennustilan tietopalvelujen liikkuminen

- [Tallennustilan Tuo/Vie palvelun REST-Ohjelmointirajapinnalla](https://msdn.microsoft.com/library/azure/dn529096.aspx)
- [.NET tallennustilan tietojen siirtämistä asiakkaan kirjasto](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement/)

### <a name="tools-and-utilities"></a>Työkalut ja apuohjelmat.

- [Azure-tallennustilan Explorer](http://go.microsoft.com/fwlink/?LinkID=822673&clcid=0x409)
- [Azure-tallennustilan asiakastyökalut](storage-explorers.md)
- [Azure SDK: T ja työkalut](https://azure.microsoft.com/tools/)
- [Azure-tallennustilan emulaattorin](http://www.microsoft.com/download/details.aspx?id=43709)
- [Azure PowerShell](../powershell-install-configure.md)
- [Komentorivin AzCopy-apuohjelma](http://aka.ms/downloadazcopy)

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja Azure-tallennustilan, tutki nämä resurssit:

### <a name="documentation"></a>Ohjeet

- [Azure-tallennustilan dokumentaatio](https://azure.microsoft.com/documentation/services/storage/)

### <a name="for-administrators"></a>Järjestelmänvalvojille

- [Azure PowerShellin käyttäminen Azure-tallennustilan](storage-powershell-guide-full.md)
- [Azure CLI käyttäminen Azure-tallennustilan](storage-azure-cli.md)

### <a name="for-net-developers"></a>.NET-kehittäjille

- [Pääset alkuun käyttämällä .NET Azure-Blob-säiliö](storage-dotnet-how-to-use-blobs.md)
- [Pääset alkuun käyttämällä .NET Azure-taulukkotallennus](storage-dotnet-how-to-use-tables.md)
- [Käyttämällä .NET Azure jonon tallennustilan käytön aloittaminen](storage-dotnet-how-to-use-queues.md)
- [Windows Azure-tiedostosäilön käytön aloittaminen](storage-dotnet-how-to-use-files.md)

### <a name="for-javaandroid-developers"></a>Java/Android-kehittäjille

- [Opi käyttämään Java-Blob-objektien tallennustilaan](storage-java-how-to-use-blob-storage.md)
- [Opi käyttämään Java-taulukkotallennus](storage-java-how-to-use-table-storage.md)
- [Opi käyttämään Java jonon tallennustilaa](storage-java-how-to-use-queue-storage.md)
- [Opi käyttämään Java-tiedostojen tallennus](storage-java-how-to-use-file-storage.md)

### <a name="for-nodejs-developers"></a>Node.js kehittäjille

- [Opi käyttämään Node.js-Blob-objektien tallennustilaan](storage-nodejs-how-to-use-blob-storage.md)
- [Opi käyttämään Node.js-taulukkotallennus](storage-nodejs-how-to-use-table-storage.md)
- [Opi käyttämään jonon tallennustilaa Node.js](storage-nodejs-how-to-use-queues.md)

### <a name="for-php-developers"></a>PHP kehittäjille

- [Opi käyttämään PHP-Blob-objektien tallennustilaan](storage-php-how-to-use-blobs.md)
- [Opi käyttämään PHP-taulukkotallennus](storage-php-how-to-use-table-storage.md)
- [Opi käyttämään jonon tallennustilaa PHP](storage-php-how-to-use-queues.md)

### <a name="for-ruby-developers"></a>Ruby-kehittäjille

- [Opi käyttämään Ruby-Blob-objektien tallennustilaan](storage-ruby-how-to-use-blob-storage.md)
- [Opi käyttämään Ruby-taulukkotallennus](storage-ruby-how-to-use-table-storage.md)
- [Opi käyttämään jonon tallennustilaa Ruby](storage-ruby-how-to-use-queue-storage.md)

### <a name="for-python-developers"></a>Python kehittäjille

- [Opi käyttämään Python-Blob-objektien tallennustilaan](storage-python-how-to-use-blob-storage.md)
- [Opi käyttämään Python-taulukkotallennus](storage-python-how-to-use-table-storage.md)
- [Opi käyttämään jonon tallennustilaa Python](storage-python-how-to-use-queue-storage.md)
- [Tiedostojen tallennus-Python käyttäminen](storage-python-how-to-use-file-storage.md)

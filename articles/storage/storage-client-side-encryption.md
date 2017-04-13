<properties
    pageTitle="Asiakkaan salaus kanssa .NET Microsoft Azuren tallennustilaan | Microsoft Azure"
    description=".NET Azure tallennustilan asiakkaan kirjasto tukee asiakkaan salaus ja Azure avaimen säilö integrointi parhaan mahdollisen suojauksen Azuren tallennustilaan sovellusten."
    services="storage"
    documentationCenter=".net"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="robinsh"/>


# <a name="client-side-encryption-and-azure-key-vault-for-microsoft-azure-storage"></a>Asiakkaan salaus ja Azure avaimen säilö for Microsoft Azure-tallennustilan

[AZURE.INCLUDE [storage-selector-client-side-encryption-include](../../includes/storage-selector-client-side-encryption-include.md)]

## <a name="overview"></a>Yleiskatsaus

[Azure tallennustilan asiakkaan kirjaston .NET Nuget paketti](https://www.nuget.org/packages/WindowsAzure.Storage) tukee salaamaan tiedot ‑asiakassovelluksissa ennen Azuren tallennustilaan lataaminen ja salauksen tietojen ladattaessa asiakkaalle. Kirjaston tukee myös [Azure avaimen säilö](https://azure.microsoft.com/services/key-vault/) integrointi tallennustilan tilin hallintaa varten.

Katso vaiheittaiset opetusohjelma, joka opastaa salaaminen BLOB asiakkaan salaus ja Azure avaimen säilö, [salaaminen ja salauksen purkaminen BLOB Microsoft Azuren tallennustilaan käyttämällä Azure avaimen säilö](storage-encrypt-decrypt-blobs-key-vault.md).

Asiakkaan salauksen Java kanssa Katso [Asiakkaan salaus Microsoft Azuren tallennustilaan Java kanssa](storage-client-side-encryption-java.md).

## <a name="encryption-and-decryption-via-the-envelope-technique"></a>Salaaminen ja salauksen purkaminen kirjekuori-tekniikka kautta

Salaaminen ja salauksen purkaminen prosesseja noudattamalla kirjekuori-tekniikka.

### <a name="encryption-via-the-envelope-technique"></a>Kirjekuori-tekniikka kautta salaus

Salauksen kautta kirjekuori-tekniikka toimii seuraavasti:

1. Azure-tallennustilan asiakkaan kirjaston Luo sisällön salauksen näppäintä (CEK), joka on yhteen-aikainen käyttöön symmetrisen.
2. Käyttäjätiedot salataan käyttämällä tämän CEK.
3. CEK sitten rivittyy (salattu) avaimen salausavaimen (KEK). KEK avaimen tunnisteen tunnistetaan ja julkiseen avaimen pari tai symmetrisen avaimen ja voit voidaan hallitaan paikallisesti tai Azure avain Vaults tallennetaan.

    Itse tallennustilan asiakkaan kirjasto on aina KEK käyttöoikeus. Kirjaston käynnistää avaimen rivitys algoritmin, joka tarjoaa avaimen säilö. Käyttäjät voivat valita käytettävät mukautetut tarjoajat avaimen rivitys/purkaminen tarvittaessa.

4. Salatut tiedot tuodaan sitten Azure tallennuspalvelu. Rivitetty näppäintä sekä salauksen joitakin metatietoja on tallennettu metatiedot (-Blob-objektien) tai interpoloitu arvo salattuja tiedoilla (sanomien ja taulukon yksiköt).

### <a name="decryption-via-the-envelope-technique"></a>Salauksen kirjekuori-tekniikka kautta

Salauksen kautta kirjekuori-tekniikka toimii seuraavasti:

1. Asiakirjakirjaston oletetaan, että käyttäjä on avaimen Salausavaimen hallinta (KEK) paikallisesti tai Azure avaimen Vaults. Käyttäjän ei tarvitse tietää, jota käytettiin salauksen avain. Sen sijaan avaimen tulkintatoiminnon, joka korjaa avaimen tunnisteina näppäimet voidaan määrittää ja käyttää.
2. Asiakirjakirjaston lataaminen salattuja tiedot minkä tahansa salaus materiaali, joka on tallennettu palvelun kanssa.
3. Sitoa (purettu) käyttämällä avaimen salausta key (KEK) on rivitetty sisällön salausavaimen (CEK). Tähän uudelleen asiakas-kirjasto ei ole KEK käyttöoikeus. Se käynnistää yksinkertaisesti mukautettu tai avaimen säilö kehittäjän avaamista algoritmin.
4. Sisällön salausavaimen (CEK) käytetään salausta salattuja käyttäjätiedot.

## <a name="encryption-mechanism"></a>Salauksen järjestelmä

Tallennustilan asiakas-kirjasto käyttää [AES](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard) jotta salaa käyttäjätiedot. Tarkemmin sanottuna [Salaus estä ketjutus (CBC)](http://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Cipher-block_chaining_.28CBC.29) tilaa AES. Jokaisen palvelun toimii hieman eri tavalla, jotta käsiteltävät aiheet ne tähän.

### <a name="blobs"></a>BLOB-objektit

Asiakirjakirjaston tukee tällä hetkellä vain koko BLOB salausta. Tarkemmin sanottuna salausta tuetaan, kun käyttäjät käyttävät **UploadFrom** * menetelmistä tai * *OpenWrite** menetelmää. Jos lataamisen, molemmat valmiiksi ja alueen ladattavat tiedostot ovat tuettuja.

Salauksen, aikana asiakas-kirjasto satunnainen alustus Vector (IV) 16 tavua 32 tavua satunnaisia sisällön salauksen avaimen (CEK) ja luo ja suorita kirjekuori salauksen blob-tietojen avulla nämä tiedot. Rivitetty CEK ja salauksen joitakin metatietoja tallennetaan sen jälkeen, kun blob-metatiedot sekä salattu blob-palvelusta.

> [AZURE.WARNING] Jos muokkaat tai ladataan oman metatiedot blob-haluat varmistaa, että metatiedot säilytetään. Jos lataat uudet metatiedot ilman metatietoja, rivitetty CEK, IV ja muita metatietoja, menetetään ja blob-sisältöä voi koskaan noudatettavan uudelleen.

Salattu blob lataaminen kuuluu koko Blob-objektien käyttäminen **DownloadTo**sisällön noutaminen*/**BlobReadStream** helppokäyttöisyys tavoista. Rivitetty CEK kääreestä ja käyttää IV (tallennettu tällöin Blob-objektien metatietojen) ja palauttaa purettu tietoja käyttäjille.

Lataaminen haluamaansa alueen (**DownloadRange*** menetelmiä) liittyy salattu blob säätäminen asteikkoon, käyttäjät, jotta saat pienen osan tiedot, joita voi käyttää purkaa onnistuneesti pyydetty alue.

Kaikki tyypit blob (estää BLOB-objektit, sivun BLOB-objektit ja liittää BLOB) voit salata/salaus poistetaan tämän mallin avulla.

### <a name="queues"></a>Olevien

Sanomien voi olla minkä tahansa muodon, koska asiakirjakirjaston määrittää mukautetun muodon, joka sisältää viestin tekstin alustus Vector (IV) ja salatun sisällön salausavaimen (CEK).

Salauksen, aikana asiakirjakirjaston Luo satunnaisen IV 16 tavua sekä 32 tavun satunnaisia CEK ja suorittaa kirjekuori salaus jonon viestin tekstin näiden tietojen avulla. Rivitetty CEK ja salauksen joitakin metatietoja lisätään sitten jonon salatun viestin. Muokattu viestiä (kuten alla) on tallennettu palveluun.

    <MessageText>{"EncryptedMessageContents":"6kOu8Rq1C3+M1QO4alKLmWthWXSmHV3mEfxBAgP9QGTU++MKn2uPq3t2UjF1DO6w","EncryptionData":{…}}</MessageText>

Salauksen, aikana rivitetty avainta poimittujen jonon viestin ja kääreestä. IV on myös jonon viestin poimittujen ja käyttää sitoa näppäintä sekä purkaa jonon viestitietoja. Huomaa, että salaus metatiedot on pieni (kohdassa 500 tavua), niin kun laskea kohti jonon viestin 64 Kilotavun rajoitus, vaikutus voi hallita.

### <a name="tables"></a>Taulukot

Asiakirjakirjaston tukee salausta kohteen ominaisuuksien lisäämistä ja korvaa-toimintoja.

>[AZURE.NOTE] Yhdistämistä ei tueta. Koska alijoukkoa ominaisuudet voivat on salattu aiemmin eri avaimen avulla, yksinkertaisesti yhdistäminen uudet ominaisuudet ja metatietojen päivitys aiheuttaa tietojen menettämisen. Yhdistäminen joko edellyttää ylimääräisiä palvelun soittaminen lukemaan olemassa kohde-palvelusta tai uusi näppäimellä ominaisuutta kohti, jotka eivät sovellu suorituskyvyn parantamiseksi.

Taulukon tietojen salauksen toimii seuraavasti:  

1. Käyttäjien määrittää salattuja ominaisuudet.
2. Asiakirjakirjaston Luo satunnainen alustus Vector (IV) sekä satunnainen sisällön salausavaimen (CEK) 32 tavun jokaisen kohteen 16 tavua ja suorittaa kirjekuori salaus todistuksista uusi IV ominaisuutta kohti salataan yksittäisiä ominaisuudet. Salatun ominaisuus on tallennettu binaarimuodossa.
3. Rivitetty CEK ja salauksen joitakin metatietoja tallennetaan sitten kaksi varattu lisäominaisuuksia. Ensimmäisen varattu ominaisuuden (_ClientEncryptionMetadata1) on merkkijono-ominaisuus, joka sisältää tietoja IV, versio ja rivitetty avain. Toisen varatut (_ClientEncryptionMetadata2)-ominaisuus on binaarinen ominaisuus, joka sisältää ominaisuuksia, jotka ovat salattuja tietoja. Toisen ominaisuuden (_ClientEncryptionMetadata2) tiedot ovat itse salattu.
4. Vuoksi nämä varattu lisäominaisuuksia pakollinen salauksen käyttäjät voivat nyt ehkä vain 250 mukautettuja ominaisuuksia 252 sijaan. Kohteen kokonaiskoko on oltava pienempi kuin 1 Megatavu.

Huomaa, että vain merkkijonon ominaisuudet salattu. Jos muuntyyppisten ominaisuudet ovat salattuja, ne on muunnettava merkkijonoja. Salatun merkkijonot tallennetaan palvelun binaarinen ominaisuuksina ja ne muunnetaan takaisin merkkijonot salauksen jälkeen.

Taulukoiden lisäksi salaus-käytäntö käyttäjien on määritettävä salattuja ominaisuudet. Tämä voidaan tehdä määrittämällä joko [EncryptProperty]-määrite (for POCO TableEntity peräisin olevat kohteet) tai salaus-tulkintatoiminnon pyynnön asetukset. Salauksen tulkintatoiminnon on edustajan, joka hakee osio-näppäintä, rivi-näppäintä ja ominaisuuden nimi ja palauttaa totuusarvon, joka ilmaisee, onko kyseisestä ominaisuudesta salattu. Salauksen, aikana asiakas-kirjastossa käyttämällä näitä tietoja päättää, onko ominaisuus salattu kirjoitettaessa koneisiin. Edustajan nimen on myös ympärille miten ominaisuudet ovat salattuja logiikka vahinkojen. (Esimerkiksi jos X-, sitten salaa ominaisuus A; salaa muussa ominaisuudet A ja b) Huomaa, että se ei tarvitse lisätä luettaessa tai kyselyt kohteiden tiedot.

### <a name="batch-operations"></a>Erän toiminnot

Erän toiminnoissa samaa KEK käytetään kaikki erän toiminnon riveittäin koska asiakirjakirjaston sallii vain yhden asetukset objektin (ja näin ollen yhden käytännön/KEK) eräkäsittelytoimintoa kohden. Kuitenkin asiakirjakirjaston sisäisesti Luo uusi satunnaisia IV ja satunnaisia CEK riviä kohden osalta. Käyttäjät voivat valita myös salata eri ominaisuuksia jokaisen toiminnolle erän määrittämällä ongelman salaus tulkintatoiminnon.

### <a name="queries"></a>Kyselyt

Jos haluat suorittaa kyselyn, sinun on määritettävä avaimen tulkintatoiminnon, joka on yrittää ratkaista kaikkien tulosjoukkoa näppäimet. Jos kyselyn tulokseen sisältyvät kohde ei voi selvittää palveluntarjoaja, asiakirjakirjaston palauttaa virheen. Kyselyn, joka suorittaa palvelinpuolen ennusteiden asiakirjakirjaston lisätään erityinen salaus metatieto-ominaisuudet (_ClientEncryptionMetadata1 ja _ClientEncryptionMetadata2) oletusarvoisesti valitut sarakkeet.

## <a name="azure-key-vault"></a>Azure avaimen säilö

Azure avaimen säilö auttaa suojaavat salausavaimet ja cloud-sovellusten ja palveluiden käyttämän tietoja. Azure avaimen säilö avulla käyttäjät voivat salata näppäimet ja tietoja (kuten todennus näppäimet tallennustilan tilin näppäimet, tietojen salausavaimet. PFX-tiedostot ja salasanat), joka on suojattu laitteiston suojauksen moduulit (HSMs) näppäimiä käyttämällä. Lisätietoja on artikkelissa [Azure avaimen säilö ominaisuudet?](../key-vault/key-vault-whatis.md).

Tallennustilan asiakas-kirjasto käyttää avaimen säilö core kirjaston haluat säätää kaikissa Azure yhteisistä hallinta avaimet. Käyttäjät saavat myös muita etu avaimen säilö tunnisteet-kirjasto. Tunnisteet-kirjasto tarjoaa hyödyllisiä toimintoja ympärille perus- ja saumattoman Symmetric/RSA paikallisen ja cloud avaimen palveluntarjoajien sekä kooste ja tallentamisesta välimuistiin.

### <a name="interface-and-dependencies"></a>Käyttöliittymän ja riippuvuudet

Kolme avain säilöön-paketit on:

- Microsoft.Azure.KeyVault.Core sisältää IKey ja IKeyResolver. Se on pieni paketti ei ole riippuvuussuhteessa. Tallennustilan asiakkaan kirjasto .NET määritellään riippuvuus.
- Microsoft.Azure.KeyVault sisältää avaimen säilö REST-asiakas.
- Microsoft.Azure.KeyVault.Extensions on tunniste-koodi, joka sisältää salatun algoritmit ja RSAKey SymmetricKey käyttöotot. Se riippuu Core ja KeyVault nimitilan ja määrittää koostefunktioita tulkintatoiminnon (kun käyttäjien on käytettävä useita keskeisiä tarjoajia) ja välimuistiin avaimen tulkintatoiminnon toimintoja. Vaikka tallennustilan asiakkaan kirjasto ei ole suoraan riippuvainen tämän paketin Jos käyttäjät haluat käyttää Azure avaimen säilö tallentamiseen niiden avaimet tai tarjoaman paikallisen ja salauksen tarjoajat cloud avain säilöön-laajennukset avulla, tiedot on tämän paketin.

Avaimen säilö on suunniteltu suuria perustyyli näppäimet ja rajoittava rajoitukset kohti avaimen säilö on suunniteltu tämän mielessä. Asiakkaan salaus avaimen säilö, suoritettaessa ensisijainen malli on käyttää symmetrisen perustyyli-näppäimiä tallennettujen tietoja avain säilöön ja välimuistiin paikallisesti. Käyttäjien on seuraavasti:

1. Luo salaisuus offline-tilassa ja lataa se avaimen säilö.
2. Määritä salaisuus perus tunnus parametrin ratkaiseminen salauksen salaisuus nykyisen version ja tallentaa tiedot paikallisesti. Käytä CachingKeyResolver välimuistiin tallentaminen; käyttäjien ei odoteta toteuttamisesta omia välimuistiin logiikan.
3. Käytä välimuistiin tulkintatoiminnon syötteeksi salaus käytännön luomisen aikana.

Lisätietoja avaimen säilö käyttö löytyy [salauksen MALLIKOODEJA](https://github.com/Azure/azure-storage-net/tree/master/Samples/GettingStarted/EncryptionSamples).

## <a name="best-practices"></a>Parhaat käytännöt

Salauksen tuki on käytettävissä vain .NET tallennustilan asiakas-kirjasto. Windows Phone-ja Windows Runtimen tällä hetkellä tue salausta.

>[AZURE.IMPORTANT] Ota huomioon seuraavat tärkeät seikat asiakaspuolen salausta käytettäessä:
>
>- Luettaessa tai kirjoitettaessa salattu blob, käytä koko blob Lataa komennot ja alue/kokonaisuuteen blob Lataa komennot. Vältä käyttämällä protokolla toimintoja, kuten valitseminen estä, sijoita ruutuluettelo, kirjoittaa sivuja, Poista sivut tai liittäminen estäminen salattu blob kirjoittaminen saattaa muuten vioittunut salattu blob ja tekemällä siitä voi lukea.
>- Taulukoiden samalla rajoitus on olemassa. Älä päivitä salattuja ominaisuudet päivittämättä salaus metatiedot.
>- Jos olet määrittänyt metatietojen salattu blob, voi korvata vaaditaan salauksen, koska metatietojen määrittäminen ei ole toisiinsa salaus liittyviä metatietoja. Tämä koskee myös tilannevedoksia; Vältä määrittäminen metatietojen tilannevedoksen salattu blob luomisen aikana. Jos metatietojen on määritettävä ensin nykyinen salaus metatiedot pääset **FetchAttributes** -menetelmää, että ja välttää samanaikainen kirjoituksia, kun metatiedot määritetään.
>- Ota käyttöön-käyttäjät, jotka kannattaa käsitellä vain salattujen tietojen pyyntö oletusasetusten **RequireEncryption** -ominaisuutta. Noudata seuraavia ohjeita, katso lisätietoja.


## <a name="client-api--interface"></a>Asiakkaan API / liittymän

Objektin EncryptionPolicy luotaessa käyttäjät voivat antaa vain (käyttöönoton IKey) näppäintä, vain (käyttöönoton IKeyResolver) tulkintatoiminnon tai molemmat. IKey on basic avaimen tyyppi, joka on määritetty käyttämällä avaimen tunnisteen sekä, joka annetaan logiikan rivitys/purkaminen. IKeyResolver käytetään ratkaista avaimen salauksen aikana. Se määrittää ResolveKey menetelmän, joka palauttaa IKey annettu avaimen tunnisteen. Tämä on käyttäjien mahdollisuus valita useita näppäimiä, joita hallitaan useisiin sijainteihin.

- Salauksen avainta käytetään aina ja näppäintä puuttuminen aiheuttaa virheen.
- Saat salauksen:
    - Avaimen tulkintatoiminnon käynnistyy, jos määritetty hankkia avain. Jos tulkintatoiminnon on määritetty, mutta se ei ole yhdistämismäärityksen avaimen tunnus, ilmenee virhe.
    - Jos Ratkaisin ei ole määritetty mutta näppäintä on määritetty, avainta käytetään, jos tunnisteen vastaa tarvittavat avaimen tunniste. Jos tunnus ei täsmää, ilmenee virhe.

[Salauksen näytteiden](https://github.com/Azure/azure-storage-net/tree/master/Samples/GettingStarted/EncryptionSamples) kuvataan yksityiskohtaisempia lopusta loppuun-skenaario BLOB-objektit ja olevien taulukoiden, avaimen säilö integrointi.

### <a name="requireencryption-mode"></a>RequireEncryption tila

Käyttäjät voivat ottaa käyttöön myös moodilla-toiminto, jossa kaikki keskeytetyt lataukset on salattu. Tässä tilassa asiakkaan epäonnistuu yrittää ladata tiedot ilman salaus-käytäntö tai lataa tiedot, joita ei ole salattu-palvelusta. Pyynnön asetukset objektin **RequireEncryption** -ominaisuus ohjaa tämän ongelman. Jos sovelluksen salaa kaikki objektit Azuren tallennustilaan tallennettuja, voit määrittää **RequireEncryption** -ominaisuus-palvelun asiakkaan objektin pyynnön oletusasetusten. Määritä esimerkiksi **CloudBlobClient.DefaultRequestOptions.RequireEncryption** arvoksi **true** salata kaikki asiakkaan objektin luonnista blob-toimintoja varten.

### <a name="blob-service-encryption"></a>BLOB-palvelun salaus

Luo **BlobEncryptionPolicy** objekti ja aseta se pyynnön asetukset (API kohti tai asiakkaan tasolla käyttämällä **DefaultRequestOptions**). Monipuolisista käsitellään mukaan asiakirjakirjaston sisäisesti.

    // Create the IKey used for encryption.
    RsaKey key = new RsaKey("private:key1" /* key identifier */);

    // Create the encryption policy to be used for upload and download.
    BlobEncryptionPolicy policy = new BlobEncryptionPolicy(key, null);

    // Set the encryption policy on the request options.
    BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

    // Upload the encrypted contents to the blob.
    blob.UploadFromStream(stream, size, null, options, null);

    // Download and decrypt the encrypted contents from the blob.
    MemoryStream outputStream = new MemoryStream();
    blob.DownloadToStream(outputStream, null, options, null);

### <a name="queue-service-encryption"></a>Jonon palvelun salaus

Luo **QueueEncryptionPolicy** objekti ja aseta se pyynnön asetukset (API kohti tai asiakkaan tasolla käyttämällä **DefaultRequestOptions**). Monipuolisista käsitellään mukaan asiakirjakirjaston sisäisesti.


    // Create the IKey used for encryption.
    RsaKey key = new RsaKey("private:key1" /* key identifier */);

    // Create the encryption policy to be used for upload and download.
    QueueEncryptionPolicy policy = new QueueEncryptionPolicy(key, null);

    // Add message
    QueueRequestOptions options = new QueueRequestOptions() { EncryptionPolicy = policy };
    queue.AddMessage(message, null, null, options, null);

    // Retrieve message
    CloudQueueMessage retrMessage = queue.GetMessage(null, options, null);

### <a name="table-service-encryption"></a>Taulukon palvelun salaus

Salaus-käytännön luominen ja määrittäminen pyynnön asetukset-kohdan lisäksi voit täytyy määrittää **EncryptionResolver** **TableRequestOptions**tai määrittäminen [EncryptProperty]-määrite yksikön.

#### <a name="using-the-resolver"></a>Tulkintatoiminnon käyttäminen


    // Create the IKey used for encryption.
    RsaKey key = new RsaKey("private:key1" /* key identifier */);

    // Create the encryption policy to be used for upload and download.
    TableEncryptionPolicy policy = new TableEncryptionPolicy(key, null);

    TableRequestOptions options = new TableRequestOptions()
    {
        EncryptionResolver = (pk, rk, propName) =>
        {
            if (propName == "foo")
            {
                return true;
            }
            return false;
        },
        EncryptionPolicy = policy
    };

    // Insert Entity
    currentTable.Execute(TableOperation.Insert(ent), options, null);

    // Retrieve Entity
    // No need to specify an encryption resolver for retrieve
    TableRequestOptions retrieveOptions = new TableRequestOptions()
    {
        EncryptionPolicy = policy
    };

    TableOperation operation = TableOperation.Retrieve(ent.PartitionKey, ent.RowKey);
    TableResult result = currentTable.Execute(operation, retrieveOptions, null);

#### <a name="using-attributes"></a>Käyttämällä määritteet

Kuten edellä mainittiin, jos kohteen toteuttaa TableEntity, ja valitse ominaisuudet voivat päätteinen sijaan **EncryptionResolver**määrittäminen [EncryptProperty]-määrite.

    [EncryptProperty]
    public string EncryptedProperty1 { get; set; }

## <a name="encryption-and-performance"></a>Salaus ja suorituskyky

Huomaa, että salaaminen suorituskykyä katseltavan tallennustilan tietojen tulokset. Sisällön avain ja IV on luotava, sisällön itse täytyy olla salattu ja muita metatiedon on muotoiltu ja ladattu. Tämä katseltavan vaihtelevat sen mukaan, salauksen tietojen määrän mukaan. On suositeltavaa, että asiakkaiden testata verkkosovelluksistaan suorituskyvyn kehityksen aikana.

## <a name="next-steps"></a>Seuraavat vaiheet

- [Opetusohjelma: Salaaminen ja salauksen BLOB Microsoft Azuren tallennustilaan käyttämällä Azure avaimen säilö](storage-encrypt-decrypt-blobs-key-vault.md)
- [Azure tallennustilan asiakkaan kirjaston .NET NuGet paketin](https://www.nuget.org/packages/WindowsAzure.Storage) lataaminen
- Lataa Azure avaimen säilö NuGet [Core](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Core/) [asiakkaan](http://www.nuget.org/packages/Microsoft.Azure.KeyVault/)ja [laajennukset](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Extensions/) -paketit  
- Siirry [Azure avaimen säilö dokumentaatio](../key-vault/key-vault-whatis.md)


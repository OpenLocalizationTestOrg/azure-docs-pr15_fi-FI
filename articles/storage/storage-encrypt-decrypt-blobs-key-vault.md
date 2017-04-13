<properties
    pageTitle="Opetusohjelma: Salaaminen ja salauksen BLOB Microsoft Azuren tallennustilaan käyttämällä Azure avaimen säilö | Microsoft Azure"
    description="Tässä opetusohjelmassa esitellään salaaminen ja salauksen Blob-objektien käyttäen asiakaspuolen salausta Microsoft Azuren tallennustilaan Azure avaimen säilö kanssa."
    services="storage"
    documentationCenter=""
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="required"
    ms.date="10/18/2016"
    ms.author="lakasa;robinsh"/>

# <a name="tutorial-encrypt-and-decrypt-blobs-in-microsoft-azure-storage-using-azure-key-vault"></a>Opetusohjelma: Salaaminen ja salauksen BLOB Microsoft Azuren tallennustilaan käyttämällä Azure avaimen säilö

## <a name="introduction"></a>Johdanto

Tässä opetusohjelmassa kerrotaan, miten voit tehdä käyttää asiakkaan tallennustilan salauksen Azure avaimen säilö kanssa. Se käydään läpi salaaminen ja salauksen Blob-objektien console-sovelluksessa käyttämällä näitä vaihtoehtoja.

**Arvioitu kesto:** 20 minuutin ajan

Yleistietoja Azure avaimen säilö, katso [Azure avaimen säilö ominaisuudet?](../key-vault/key-vault-whatis.md).

Yleistietoja asiakkaan salaus Azure-tallennustilan Katso [asiakkaan salaus ja Azure avaimen säilö for Microsoft Azure-tallennustilan](storage-client-side-encryption.md).


## <a name="prerequisites"></a>Edellytykset

Jotta voit suorittaa tässä opetusohjelmassa, sinulla on oltava seuraavasti:

- Azure-tallennustilan tilin
- Visual Studio 2013 tai uudempi versio
- Azure PowerShell


## <a name="overview-of-client-side-encryption"></a>Asiakkaan salauksen yleiskatsaus

Yleiskatsaus asiakkaan salaus Azure-tallennustilan Katso [asiakkaan salaus ja Azure avaimen säilö, Microsoft Azuren tallennustilaan](storage-client-side-encryption.md)

Seuraavassa on lyhyt kuvaus asiakaspuolen salausta toiminnasta:

1. Azuren tallennustilaan asiakkaan SDK Luo sisällön salauksen näppäintä (CEK), joka on yhteen-aikainen käyttöön symmetrisen.
2. Asiakkaan tiedot salataan käyttämällä tämän CEK.
3. CEK sitten rivittyy (salattu) avaimen salausavaimen (KEK). KEK avaimen tunnisteen tunnistetaan ja julkiseen avaimen pari tai symmetrisen avaimen ja voit voidaan hallitaan paikallisesti tai Azure avain säilöön tallennettuja. Tallennustilan asiakkaan itse on aina KEK käyttöoikeus. Tuo näyttöön vain avaimen rivitys algoritmin, joka tarjoaa avaimen säilö. Asiakkaat voivat valita mukautetun tarjoajat käytettävät rivitys/purkaminen halutessaan avain.
4. Salatut tiedot tuodaan sitten Azure tallennuspalvelu.


## <a name="set-up-your-azure-key-vault"></a>Azure-avain säilö määrittäminen
Jotta voit jatkaa Tässä opetusohjelmassa, sinun on suoritettava seuraavat toimet, jotka kuvattuja opetusohjelman [aloittaminen Azure avaimen säilö](../key-vault/key-vault-get-started.md):

- Luo avaimen säilö.
- Lisää avaimen tai salaisuus avaimen säilö.
- Sovelluksen rekisteröiminen Azure Active Directory-hakemistosta.
- Määritä sovellus käyttää avainta tai toiminta.

Kirjoita muistiin ClientID ja ClientSecret, joka luotiin, kun sovellus rekisteröidään Azure Active Directory-hakemistosta.

Luo molemmat avaimet avaimen säilö. Oletetaan opetusohjelman muiden, että olet käyttänyt seuraavat nimet: ContosoKeyVault ja TestRSAKey1.


## <a name="create-a-console-application-with-packages-and-appsettings"></a>Pakettien ja AppSettings console-sovelluksen luominen

Visual Studiossa Luo uusi console-sovellus.

Lisää tarvittavat nuget pakettien Package hallinta-konsolin.

    Install-Package WindowsAzure.Storage

    // This is the latest stable release for ADAL.
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.16.204221202

    Install-Package Microsoft.Azure.KeyVault
    Install-Package Microsoft.Azure.KeyVault.Extensions


Lisää AppSettings App.Config.

    <appSettings>
        <add key="accountName" value="myaccount"/>
        <add key="accountKey" value="theaccountkey"/>
        <add key="clientId" value="theclientid"/>
        <add key="clientSecret" value="theclientsecret"/>
        <add key="container" value="stuff"/>
    </appSettings>

Lisää seuraava `using` lauseet ja varmista, että viittaus System.Configuration lisääminen projektiin.

    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System.Configuration;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    using Microsoft.Azure.KeyVault;
    using System.Threading;     
    using System.IO;


## <a name="add-a-method-to-get-a-token-to-your-console-application"></a>Lisää menetelmä konsolin sovelluksen tunnuksen hankkiminen

Avaimen säilö luokat, jotka on todennus käyttämään avaimen säilö käyttävät seuraavalla tavalla.

    private async static Task<string> GetToken(string authority, string resource, string scope)
    {
        var authContext = new AuthenticationContext(authority);
        ClientCredential clientCred = new ClientCredential(
            ConfigurationManager.AppSettings["clientId"],
            ConfigurationManager.AppSettings["clientSecret"]);
        AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

        if (result == null)
            throw new InvalidOperationException("Failed to obtain the JWT token");

        return result.AccessToken;
    }

## <a name="access-storage-and-key-vault-in-your-program"></a>Käyttää tallennustilaa ja avain säilöön-ohjelmassa

Pää-funktio lisää seuraava koodi.

    // This is standard code to interact with Blob storage.
    StorageCredentials creds = new StorageCredentials(
        ConfigurationManager.AppSettings["accountName"],
        ConfigurationManager.AppSettings["accountKey"]);
    CloudStorageAccount account = new CloudStorageAccount(creds, useHttps: true);
    CloudBlobClient client = account.CreateCloudBlobClient();
    CloudBlobContainer contain = client.GetContainerReference(ConfigurationManager.AppSettings["container"]);
    contain.CreateIfNotExists();

    // The Resolver object is used to interact with Key Vault for Azure Storage.
    // This is where the GetToken method from above is used.
    KeyVaultKeyResolver cloudResolver = new KeyVaultKeyResolver(GetToken);


> [AZURE.NOTE] Avaimen säilö objektin malleja
>
>On tärkeää ymmärtää, että ovat itse asiassa kaksi avaimen säilö objektimallit huomioon: yksi perustuu REST-Ohjelmointirajapinnalla (KeyVault nimitilan) ja toinen tiedostotunnistetta asiakaspuolen salausta.

> Avaimen säilö asiakkaan toimii REST-ohjelmointirajapinnalla ja ymmärtää JSON Web näppäimet ja asioita, jotka sisältyvät avaimen säilö kahdenlaisia tietoja.

> Avaimen säilö laajennukset ovat luokat, jotka näyttävät erityisesti luotu asiakkaan salauksen Azuren tallennustilaan. Ne sisältävät käyttöliittymään näppäimet (IKey) ja luokat avain tulkintatoiminnon käsite perusteella. On IKey, jotka on hyvä tietää kaksi käyttöotot: RSAKey ja SymmetricKey. Nyt näyttöön reaaliajassa asioista, jotka sisältyvät avaimen säilö kanssa, mutta tässä vaiheessa ne ovat itsenäisten luokkia (jotta avain ja salainen avain säilö asiakkaan noutamien Toteuta IKey).


## <a name="encrypt-blob-and-upload"></a>Salaa Blob-objektien ja lataa
Lisää seuraava koodi voit salata blob ja lataa se Azure-tallennustilan tilin. **ResolveKeyAsync** menetelmä, jota käytetään palauttaa IKey.


    // Retrieve the key that you created previously.
    // The IKey that is returned here is an RsaKey.
    // Remember that we used the names contosokeyvault and testrsakey1.
    var rsa = cloudResolver.ResolveKeyAsync("https://contosokeyvault.vault.azure.net/keys/TestRSAKey1", CancellationToken.None).GetAwaiter().GetResult();


    // Now you simply use the RSA key to encrypt by setting it in the BlobEncryptionPolicy.
    BlobEncryptionPolicy policy = new BlobEncryptionPolicy(rsa, null);
    BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

    // Reference a block blob.
    CloudBlockBlob blob = contain.GetBlockBlobReference("MyFile.txt");

    // Upload using the UploadFromStream method.
    using (var stream = System.IO.File.OpenRead(@"C:\data\MyFile.txt"))
        blob.UploadFromStream(stream, stream.Length, null, options, null);


Seuraavassa on [Azure perinteinen Portal](https://manage.windowsazure.com) -Blob-objektien, joka on salattu käyttämällä asiakkaan salauksen avaimen säilöön tallennettuja näppäintä näyttökuvan. **Avaimen tunnus** -ominaisuus on URI-näppäimen avain säilöön, joka toimii KEK. **EncryptedKey** -ominaisuus sisältää salatun CEK-version.

![Näyttökuva, joka sisältää salauksen metatietojen Blob-metatiedot][1]

> [AZURE.NOTE] Jos tarkastelet BlobEncryptionPolicy konstruktoria, näet, että se voi hyväksyä avaimen ja/tai tulkintatoiminnon. Ota huomioon, oikealle, kun et voi käyttää tulkintatoiminnon salauksen, koska se tekee ei tällä hetkellä tue oletusarvo-näppäintä.



## <a name="decrypt-blob-and-download"></a>Salauksen Blob-objektien ja lataaminen
Salauksen on todellakin, kun Ratkaisin-luokkien avulla katsoo sen perustelluksi. Salauksen käytettävän avaimen tunnus on liitetty Blob-objektien sen metatiedot niin ei ole syytä voit noutaa ja muista avain ja Blob-objektien välisen suhteen. Sinulla on varmistaaksesi, että avain pysyy avain säilöön.   

RSA-avain yksityinen avain säilyy avain säilöön, jotta varten salauksen yksittäiseksi salattu avainta, joka sisältää CEK metatiedoista Blob-objektien lähetetään avaimen säilö salauksen.

Lisää seuraavat salauksen Blob-objektien vain ladata mitä tahansa.

    // In this case, we will not pass a key and only pass the resolver because
    // this policy will only be used for downloading / decrypting.
    BlobEncryptionPolicy policy = new BlobEncryptionPolicy(null, cloudResolver);
    BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

    using (var np = File.Open(@"C:\data\MyFileDecrypted.txt", FileMode.Create))
        blob.DownloadToStream(np, null, options, null);


> [AZURE.NOTE] Muutamalla resolvers helpottaa hallintaa, myös muunlaisia: AggregateKeyResolver ja CachingKeyResolver.


## <a name="use-key-vault-secrets"></a>Käytä avaimen säilö tietoja
Tapa salaisuus käyttäminen asiakkaan salaus on kautta SymmetricKey-luokka toiminta perustuu olennaisesti symmetrisen avaimen. Mutta yllä mainittuja salainen avain säilöön ei yhdistetä täsmälleen SymmetricKey. Voit kartoittaa muutamia asioita:


- SymmetricKey avain on kiinteä pituus: 128, 192, 256, 384 tai 512 bittien.
- SymmetricKey avain on oltava Base64-koodattuja.
- Avaimen säilö salaisuus, jota käytetään SymmetricKey on oltava sisältötyyppi-sovelluksen/octet-stream"avain säilöön.

Tässä on esimerkki PowerShell luomisesta salainen avain säilöön, jonka avulla voidaan SymmetricKey.
Huomautus: Pysyvä arvo $key, on vain esittely tarkoitukseen. Oman koodin kannattaa luoda avain.

    // Here we are making a 128-bit key so we have 16 characters.
    //  The characters are in the ASCII range of UTF8 so they are
    //  each 1 byte. 16 x 8 = 128.
    $key = "qwertyuiopasdfgh"
    $b = [System.Text.Encoding]::UTF8.GetBytes($key)
    $enc = [System.Convert]::ToBase64String($b)
    $secretvalue = ConvertTo-SecureString $enc -AsPlainText -Force

    // Substitute the VaultName and Name in this command.
    $secret = Set-AzureKeyVaultSecret -VaultName 'ContoseKeyVault' -Name 'TestSecret2' -SecretValue $secretvalue -ContentType "application/octet-stream"

Console-sovelluksessa voit saman puhelu ennen tässä muodossa SymmetricKey salaisuus hakemiseen.

    SymmetricKey sec = (SymmetricKey) cloudResolver.ResolveKeyAsync(
        "https://contosokeyvault.vault.azure.net/secrets/TestSecret2/",
        CancellationToken.None).GetAwaiter().GetResult();

Se on. Nauti!

## <a name="next-steps"></a>Seuraavat vaiheet

Saat lisätietoja käyttäminen Microsoft Azuren tallennustilaan C# [.NET Microsoft Azure tallennustilan asiakkaan kirjasto](https://msdn.microsoft.com/library/azure/dn261237.aspx).

Saat lisätietoja Blob REST API- [Blob-objektien palvelun REST API](https://msdn.microsoft.com/library/azure/dd135733.aspx).

Microsoft Azuren tallennustilaan uusimmat tiedot Siirry [Microsoft Azure tallennustilan tiimin blogia](http://blogs.msdn.com/b/windowsazurestorage/).


<!--Image references-->
[1]: ./media/storage-encrypt-decrypt-blobs-key-vault/blobmetadata.png

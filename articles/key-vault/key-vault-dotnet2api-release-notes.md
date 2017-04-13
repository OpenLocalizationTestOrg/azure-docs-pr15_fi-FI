<properties
   pageTitle="Avaimen säilö .NET 2.x API julkaisutiedot | Microsoft Azure"
   description=".NET-kehittäjille käyttää tätä API-koodiin Azure avaimen säilö"
   services="key-vault"
   documentationCenter=""
   authors="BrucePerlerMS"
   manager="mbaldwin"
   editor="bruceper" />
<tags
   ms.service="key-vault"
   ms.devlang="CSharp"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/07/2016"
   ms.author="bruceper" />

# <a name="azure-key-vault-net-20---release-notes-and-migration-guide"></a>Azure avaimen säilö .NET 2.0 - julkaisutiedot ja siirtymisopas

Seuraavat muistiinpanoja ja ohjeet ovat käyttäminen Azure avaimen säilö .NET-kehittäjille ja C#-kirjasto. Päivitysten määrä on tehty muutos 1.0 versiosta versiota 2.0-edellyttävät siirron työn omassa koodissasi, jotta voit hyötyä toimintojen parannukset ja ominaisuus lisäykset, kuten avaimen säilö varmenteet tuki on.

Avaimen säilö varmenteet tuki on oman x509 hallinnan varmenteet ja seuraavista ongelmista:  

-   Sallii varmenne-omistajalle varmenteen avaimen säilö luontiprosessi tai varmenne, joka tuo kautta. Tämä sisältää sekä itse allekirjoitetun ja varmenteen myöntäjä luo varmenteet.

- Sallii avain säilöön-varmenteen omistajan toteuttamisesta suojattuun säilöön ja X509 hallinta varmenteet ilman toimia yksityisen avaimen aineistoa.  

-   Sallii varmenteen omistajan, joka ohjaa avaimen säilö hallittavan varmenteen elinkaari-käytännön luominen.  

-   Varmenteen omistajat voivat antaa yhteystiedot-ilmoituksen siitä, elinkaaren vanhentuminen ja uusiminen varmenteen avulla.  

-   Tukee automaattinen uusiminen valitun myöntäjistä - avain säilö kumppanin X509 varmenteen tarjoajat / varmenteen lähdeluettelon.
    - Huomautus - ei-organisaatio loi Researchin tarjoajat ja viranomaisten myös sallittu, mutta automaattisen uusimisen-ominaisuus ei tue.


## <a name="net-support"></a>.NET-tuki
- **.NET 4.0** ei tue Azure avaimen säilö .NET 2.0-version ja C#-kirjasto
- Azure avaimen säilö .NET 2.0 versio tue **.NET Core** ja C#-kirjasto

## <a name="namespaces"></a>Nimitilan
- **Mallien** nimitila muutetaan **Microsoft.Azure.KeyVault** **Microsoft.Azure.KeyVault.Models**.
- **Microsoft.Azure.KeyVault.Internal** nimitilan poistetaan.
- Azure SDK riippuvuudet nimitila muutetaan **Hyak.Common** ja **Hyak.Common.Internals** **Microsoft.Rest** ja **Microsoft.Rest.Serialization**


## <a name="type-changes"></a>Muuttuu
- *Salainen* *SecretBundle* on muutettu
- *Sanaston* *IDictionary* on muutettu
- *Luettelon<T>-merkkijono []* on muutettu *seuraavat: IList<T> *
- *NextPageLink* *NextList* on muutettu


## <a name="return-types"></a>Palauttaa tyypit
- Palauttaa **KeyList** ja **SecretList** *IPage<T> * *ListKeysResponseMessage* sijaan
- Luotu **BackupKeyAsync** palauttaa *BackupKeyResult* , joka sisältää *arvon* (varmuuskopioida blob). Ennen kuin tapa on rivitetty ja palauttaa vain arvon.

## <a name="exceptions"></a>Poikkeukset
- *KeyVaultClientException* muutetaan *KeyVaultErrorException*
- Palvelun virheen muutetaan *poikkeuksen. Virhe* *poikkeuksen. Body.Error.Message*.
- Poistaa lisätiedot virhesanoma, **[JsonExtensionData]**.

## <a name="constructors"></a>Constructors
- Sen sijaan, että hyväksyt *HttpClient* konstruktoriargumenttina, konstruktoria vain voidaan käyttää *HttpClientHandler* tai *DelegatingHandler []*.



## <a name="downloaded-packages"></a>Ladattujen pakettien  
Asiakkaan käsiteltäessä avaimen säilö riippuvuussuhteessa seuraavat on ladattu
#### <a name="previous-package-list"></a>Edellisen paketin luettelo
- Pakkaa id="Hyak.Common" version = "1.0.2" targetFramework = "net45"
- Pakkaa id="Microsoft.Azure.Common" version = "2.0.4" targetFramework = "net45"
- Pakkaa id="Microsoft.Azure.Common.Dependencies" version = "1.0.0" targetFramework = "net45"
- Pakkaa id="Microsoft.Azure.KeyVault" version = "1.0.0" targetFramework = "net45"
- Pakkaa id="Microsoft.Bcl" version = "1.1.9" targetFramework = "net45"
- Pakkaa id="Microsoft.Bcl.Async" version = "1.0.168" targetFramework = "net45"
- Pakkaa id="Microsoft.Bcl.Build" version = "1.0.14" targetFramework = "net45"
- Pakkaa id="Microsoft.Net.Http" version = "2.2.22" targetFramework = "net45"

#### <a name="current-package-list"></a>Nykyinen paketti-luettelo
- Pakkaa id="Microsoft.Azure.KeyVault" version = "2.0.0-preview" targetFramework = "net45"
- Pakkaa id="Microsoft.Rest.ClientRuntime" version = "2.2.0" targetFramework = "net45"
- Pakkaa id="Microsoft.Rest.ClientRuntime.Azure" version = "3.2.0" targetFramework = "net45"


## <a name="class-changes"></a>Luokan muutokset

- **UnixEpoch** luokka on poistettu
- **Base64UrlConverter** luokan nimetään uudelleen **Base64UrlJsonConverter**

## <a name="other-changes"></a>Muut muutokset

- KV-toiminnon uudelleen käytännössä lyhytkestoisia virheiden määritys tuki on lisätty Ohjelmointirajapinnan tässä versiossa.



## <a name="microsoftazuremanagementkeyvault-nuget"></a>Microsoft.Azure.Management.KeyVault NuGet
- Toiminnot, jotka palautti *säilöön*palautustyyppi on luokka, joka sisältää säilöön-ominaisuus. Palautustyyppi on nyt *säilö*.
- *PermissionsToKeys* ja *PermissionsToSecrets* ovat nyt *Permissions.Keys* ja *Permissions.Secrets*
- Joitakin muutoksia, palaa tyypit käyttää ohjausobjektin-suuntaisesti paikan päällä.

## <a name="microsoftazurekeyvaultextensions-nuget"></a>Microsoft.Azure.KeyVault.Extensions NuGet
- Paketin on katkennut **Microsoft.Azure.KeyVault.Extensions** ja **Microsoft.Azure.KeyVault.Cryptography** salaus toimille.

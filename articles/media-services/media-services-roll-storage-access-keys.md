<properties 
    pageTitle="Päivitä Media Services jälkeen juoksevan tallennustilan pikanäppäinten | Microsoft Azure" 
    description="Tässä artikkelissa antaa ohjeita siitä, miten voit päivittää Media Services jälkeen juoksevan tallennustilan pikanäppäimet." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako"
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="milangada;cenkdin;juliako"/>

#<a name="update-media-services-after-rolling-storage-access-keys"></a>Päivitä Media Services jälkeen juoksevan tallennustilan pikanäppäimet

Kun luot uuden Azure Media Services-tilin, voit myös pyytää Azuren tallennustilaan-tili, jota käytetään media-sisältö on tallennettu. Huomaa, että voit [lisätä useita tallennustilan tilit](meda-services-managing-multiple-storage-accounts.md) Media Services-tiliisi.

Kun uusi tallennustilan-tili on luotu, Azure Luo kaksi 512-bittinen tallennustilan pikanäppäimet, joita käytetään yhteyden tallennustilan tilin tarkistamiseen. Pitämään tallennustilan yhteydet suojattuina, on suositeltavaa säännöllisesti uudelleen ja Kierrä tallennustilan pikanäppäin. Jotta voit säilyttää yhteyden tallennustilan tilin käyttämistä yhden pikanäppäin, kun access-näppäintä uudelleen toimitetaan kaksi pikanäppäimet (ensisijainen ja toissijainen). Tämä toiminto on lyhenne sanoista "juokseva pikanäppäimet".

Media-palveluiden määräytyy annettu siihen tallennustilan-näppäintä. Tarkemmin sanottuna locators, joita käytetään virtauttaa tai ladata oman varat määräytyvät määritetyn tallennustilan pikanäppäin. Kun AMS-tili on luotu kestää riippuvuus Valitse ensisijainen tallennustilan pikanäppäin oletusarvoisesti mutta käyttäjänä voit päivittää AMS sisältävän tallennustilan-näppäintä. Varmistamalla, jos haluat sallia tietää, mitkä avain mainitut vaiheet on kuvattu tämän artikkelin avulla Media-palveluita. Lisäksi juoksevan tallennustilan pikanäppäimiä, kun haluat muista päivittää oman locators niin on olemassa palvelukatkoksia-streaming palvelussa (tämä vaihe on myös artikkelissa).

>[AZURE.NOTE]Jos käytössäsi on useita tallennustilan tilejä, voit suorittaa tämän toimenpiteen ja tallennustilaa jokaiselle tilille.
>
>Ennen kuin suoritat vaiheet on kuvattu tämän artikkelin tuotannon tilille, varmista, että testaamista edeltävien-tili.


## <a name="step-1-regenerate-secondary-storage-access-key"></a>Vaihe 1: Luo toissijainen tallennusväline-pikanäppäin

Aloita uudelleen toissijainen tallennusväline-näppäintä. Oletusarvon mukaan toissijaisen avaimen ei käytetä Media-palveluissa.  Lisätietoja siitä, miten voit koota tallennustilan näppäimet on artikkelissa [Toimintaohje: näkymän, kopioiminen ja valintanäppäimet Muodosta uudelleen sarjanumerot tallennustilan](../storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).
  
##<a id="step2"></a>Vaihe 2: Päivitä Media Services kun uusi toissijainen tallennusväline-näppäimellä

Päivitä Media Services käyttämään toissijainen tallennusväline-pikanäppäin. Regeneroitu tallennustilan avain synkronointiin Media Services jollakin seuraavista tavoista.

- Azure-portaalin käyttäminen: Voit etsiä nimen ja avaimen arvot, siirry Azure-portaaliin ja valitse tilisi. Asetukset-ikkuna oikealla. Valitse asetukset-ikkunassa avaimet. Sen mukaan, mitä tallennustilan avain haluat Media-palveluiden Synkronoi Valitse Synkronoi perusavain tai synkronoi avaimen kakkospainike. Tässä tapauksessa toissijaisen avaimen avulla

- Media-palveluiden hallinta REST API käyttäminen.

Seuraava koodi-esimerkissä muodostamisesta https://endpoint/***subscriptionId/services/mediaservices/tilit/accountName*/StorageAccounts/*storageAccountName*/Key pyynnön, jotta voit synkronoida määritetyn tallennustilan avain Media-palvelujen kanssa. Tässä tapauksessa toissijainen tallennusväline avainarvon käytetään. Lisätietoja on artikkelissa [Toimintaohje: Käytä Media palveluiden hallinta REST API](http://msdn.microsoft.com/library/azure/dn167656.aspx).
    
    public void UpdateMediaServicesWithStorageAccountKey(string mediaServicesAccount, string storageAccountName, string storageAccountKey)
    {
        var clientCert = GetCertificate(CertThumbprint);
        
        HttpWebRequest request = (HttpWebRequest)WebRequest.Create(string.Format("{0}/{1}/services/mediaservices/Accounts/{2}/StorageAccounts/{3}/Key",
        Endpoint, SubscriptionId, mediaServicesAccount, storageAccountName));
        request.Method = "PUT";
        request.ContentType = "application/json; charset=utf-8";
        request.Headers.Add("x-ms-version", "2011-10-01");
        request.Headers.Add("Accept-Encoding: gzip, deflate");
        request.ClientCertificates.Add(clientCert);
        
        
        using (var streamWriter = new StreamWriter(request.GetRequestStream()))
        {
            streamWriter.Write("\"");
            streamWriter.Write(storageAccountKey);
            streamWriter.Write("\"");
            streamWriter.Flush();
        }
        
        using (var response = (HttpWebResponse)request.GetResponse())
        {
            string jsonResponse;
            Stream receiveStream = response.GetResponseStream();
            Encoding encode = Encoding.GetEncoding("utf-8");
            if (receiveStream != null)
            {
                var readStream = new StreamReader(receiveStream, encode);
                jsonResponse = readStream.ReadToEnd();
            }
        }
    }

Tässä vaiheessa jälkeen Päivitä aiemmin locators (joita on riippuvuuden vanha tallennustilan avain) seuraavien ohjeiden mukaisesti.

>[AZURE.NOTE]Odottaa 30 minuuttia, ennen kuin suoritat estämiseksi mitään vaikutusta odottavat työt toiminnot Media Services (esimerkiksi luotaessa uusia locators).

##<a name="step-3-update-locators"></a>Vaihe 3: Päivitä locators

>[AZURE.NOTE]Kun juoksevan tallennustilan pikanäppäimet, sinun täytyy muista päivittää aiemmin locators, joten palvelukatkoksia streaming-palvelussa.

Odota vähintään 30 minuuttia jälkeen synkronointiin uusi tallennustilan avain AMS kanssa. Voit sitten Luo OnDemand locators, jotta ottaa riippuvuuden määritetyn tallennustilan-näppäintä ja säilyttää aiemmin URL-osoite.

Huomaa, että voit päivittää eli Luo SAS locator, URL-osoite aina muuttuessa.

>[AZURE.NOTE] Varmista, että Säilytä OnDemand locators aiemmin URL-osoitteet, sinun on aiemmin locator ja luoda uuden on sama.

.NET alla olevassa esimerkissä voit luoda locator, joilla on sama.

Yksityinen staattinen ILocator RecreateLocator(CloudMediaContext context, ILocator locator) {/ / Tallenna aiemmin locator ominaisuudet.
var-resurssi = locator. Resurssi; var accessPolicy = locator. AccessPolicy; var locatorId = locator. Tunnus; var-alkamispäivä = locator. Aloitusajan; var locatorType = locator. Kirjoita; var locatorName = locator. Nimeä.

Poista vanha locator.
Locator. DELETE();

Jos (locator. ExpirationDateTime < = DateTime.UtcNow) {palauttaa uuden poikkeus (String.Format ("ei voi luoda locator Id = {0}, koska sen locator päättymisaika on jo mennyt", locator. Tunnus)); }
    
        // Create new locator using saved properties.
        var newLocator = context.Locators.CreateLocator(
            locatorId,
            locatorType,
            asset,
            accessPolicy,
            startDate,
            locatorName);
    
    
    
        return newLocator;
    }


##<a name="step-5-regenerate--primary-storage-access-key"></a>Vaihe 5: Luo ensisijainen tallennustilan pikanäppäin

Luo ensisijainen tallennustilan pikanäppäin. Lisätietoja siitä, miten voit koota tallennustilan näppäimet on artikkelissa [Toimintaohje: näkymän, kopioiminen ja valintanäppäimet Muodosta uudelleen sarjanumerot tallennustilan](../storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).

##<a name="step-6-update-media-services-to-use-the-new-primary-storage-key"></a>Vaihe 6: Update Media Services kun uusi ensisijainen tallennustilan-näppäimellä
    
Käytä samalla tavalla ohjeiden vaiheessa [2](media-services-roll-storage-access-keys.md#step2) vain tällä hetkellä synkronoida uusi ensisijainen tallennustilan pikanäppäin Media Services-tilin kanssa.

>[AZURE.NOTE]Odottaa 30 minuuttia, ennen kuin suoritat estämiseksi mitään vaikutusta odottavat työt toiminnot Media Services (esimerkiksi luotaessa uusia locators).

##<a name="step-7-update-locators"></a>Vaihe 7: Päivitä locators  

30 minuutin kuluttua Luo OnDemand locators, jotta ne ottaa riippuvuuden ensisijainen tallennustilan uusi avain ja säilyttää aiemmin URL-osoite.

Käytä samalla tavalla kuin [vaiheessa 3](media-services-roll-storage-access-keys.md#step-3-update-locators)kuvattua.


##<a name="media-services-learning-paths"></a>Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



###<a name="acknowledgments"></a>Vahvistus 

Haluamme toki seuraavat henkilöt, jotka on lähettänyt kohti tämän asiakirjan luominen: Cenk Dingiloglu Milano Gada Seva Titov.

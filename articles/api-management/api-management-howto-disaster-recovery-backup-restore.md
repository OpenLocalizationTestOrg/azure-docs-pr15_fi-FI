<properties 
    pageTitle="Toteuta palvelun varmuuskopioinnista palauttaminen ja Azure API hallinnan palauttaminen | Microsoft Azure" 
    description="Opettele varmuuskopiointi ja palauttaminen suorittamiseen palauttaminen Azure API hallinta." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-implement-disaster-recovery-using-service-backup-and-restore-in-azure-api-management"></a>Toteuta palvelun varmuuskopioinnista palauttaminen ja Azure API hallinnan palauttaminen

Voit julkaista ja hallita oman ohjelmointirajapinnan kautta Azure API hallinta valitsemalla aloittamasi SharePointin useita vikasietoa ja infrastruktuuri-ominaisuudet, jotka muuten olisi suunnitella ja toteuttaa hallinta. Azure-ympäristö lieventää mahdolliset virheet, murtoluvun kustannusten suuri murtoluvuksi.

Käytettävyys palauttaminen ongelmista alue, jossa oman API hallinta-palvelu on isännöidään voit pitäisi olla valmis jakomenetelmä toisella alueella palvelun milloin tahansa. Eri käytettävyys tavoitteet ja palautus aika tavoitteen voit haluta varata yhteen tai useampaan varmuuskopion palvelu ja yritä ylläpitää niiden määritys ja sisällön aktiivisen-palvelun kanssa. Palvelun varmuuskopiointi ja palauttaminen-toiminto tarjoaa tarvittavat rakenneosan käyttöönoton yrityksen tietojen palauttaminen määrittäminen.

Tämän oppaan näytetään, miten tarkistamiseen Azure Resurssienhallinta pyynnöt ja varmuuskopioiminen ja palauttaminen API Management-palvelun esiintymät.

>[AZURE.NOTE] Prosessi, jossa varmuuskopiointia ja palauttamista API hallinnan palveluesiintymä, palauttaminen voi käyttää myös replikoiminen API hallinta palvelun esiintymät, kuten väliaikaisesta skenaarioita.
>
>Huomaa, että kunkin varmuuskopiointi vanhentuu seitsemän päivää. Jos yrität palauttaminen varmuuskopiosta jälkeen 7 päivän voimassaoloajan on päättynyt, Palauta epäonnistuu `Cannot restore: backup expired` viesti.

## <a name="authenticating-azure-resource-manager-requests"></a>Todentamalla Azure Resurssienhallinta pyytää

>[AZURE.IMPORTANT] Varmuuskopiointi ja palauttaminen REST-Ohjelmointirajapinta käyttää Azure Resurssienhallinta ja eri käyttöoikeuksien järjestelmä kuin REST API API hallinta kohteiden hallintaan. Tässä jaksossa kerrotaan, kuinka voit todentaa Azure Resurssienhallinta pyynnöt. Lisätietoja on artikkelissa [todennustapa Azure Resurssienhallinta pyynnöt](http://msdn.microsoft.com/library/azure/dn790557.aspx).

Kaikki tehtävät, jotka voit tehdä resursseja Azure resurssien hallintaa käyttämällä on todennetut Azure Active Directory seuraavien ohjeiden mukaisesti.

-   Lisää sovellus Azure Active Directory-vuokraajan.
-   Määritä sovellus, jonka lisäsit käyttöoikeudet.
-   Pyydä tunnuksen todennustapa pyynnöt Azure resurssien hallinta.

Ensimmäiseksi on luotava Azure Active Directory-sovelluksen. Kirjaudu sisään [Azure perinteinen Portal](http://manage.windowsazure.com/) tilauksen, joka sisältää API Management-palvelun esiintymän käyttäminen ja siirry Azure Active Directory-oletuskansioon **sovellukset** -välilehti.

>[AZURE.NOTE] Jos Azure Active Directory-oletuskansio ei ole näkyvissä tiliisi, ota yhteyttä järjestelmänvalvojaan Azure-tilauksen myöntää tilisi on tarvittavat käyttöoikeudet. Lisätietoja etsiminen oletusarvo-kansio on kohdassa [Etsi oletusarvo-kansio](../virtual-machines/virtual-machines-windows-create-aad-work-id.md#locate-your-default-directory-in-the-azure-portal).

![Azure Active Directory-sovelluksen luominen][api-management-add-aad-application]

**Lisää**, **Lisää organisaation kehittää sovellus**, ja valitse **alkuperäisen asiakassovellukseen**. Kirjoita kuvaava nimi ja valitse Seuraava-nuoli. Anna paikkamerkin URL-osoite, kuten `http://resources` **Uudelleenohjata URI**sellaisena kuin se on pakollinen kenttä, mutta arvo ei käytetä myöhemmin. Valitse Tallenna sovellus-valintaruutu.

Kun sovellus on tallennettu, **Määritä**, vieritä **muiden sovellusten käyttöoikeudet** -osaan ja valitse **Lisää sovellus**.

![Lisää käyttöoikeuksia][api-management-aad-permissions-add]

**Windows** **Azure Service Management API** ja valitse valintaruutu, jos haluat lisätä sovelluksen.

![Lisää käyttöoikeuksia][api-management-aad-permissions]

Valitse **Valtuutetut käyttöoikeudet** lisätyn **Windows** **Azure Service Management API** -sovelluksen vieressä, **Access Azure hallinta (ennakkoversio)**valintaruutu ja valitse **Tallenna**.

![Lisää käyttöoikeuksia][api-management-aad-delegated-permissions]

Ennen käynnistettäessä, luo varmuuskopion ja palauttaa laitteen ohjelmointirajapinnan on saat tunnusta. Seuraavassa esimerkissä [Microsoft.IdentityModel.Clients.ActiveDirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) nuget paketin hakemiseen tunnuksen.

    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System;

    namespace GetTokenResourceManagerRequests
    {
        class Program
        {
            static void Main(string[] args)
            {
                var authenticationContext = new AuthenticationContext("https://login.windows.net/{tenant id}");
                var result = authenticationContext.AcquireToken("https://management.azure.com/", {application id}, new Uri({redirect uri});

                if (result == null) {
                    throw new InvalidOperationException("Failed to obtain the JWT token");
                }

                Console.WriteLine(result.AccessToken);

                Console.ReadLine();
            }
        }
    }

Korvaa `{tentand id}`, `{application id}`, ja `{redirect uri}` seuraavien ohjeiden avulla.

Korvaa `{tenant id}` juuri luomasi Azure Active Directory-sovelluksen vuokraajan-tunnuksella. Voit avata tunnus valitsemalla **Näytä päätepisteet**.

![Päätepisteet][api-management-aad-default-directory]

![Päätepisteet][api-management-endpoint]

Korvaa `{application id}` ja `{redirect uri}` **Asiakastunnus** ja URL-Osoitteen avulla Azure Active Directory-sovelluksen **määrittäminen** -välilehdestä **Uudelleenohjata URI** -osiosta. 

![Resurssit][api-management-aad-resources]

Kun arvot on määritetty, esimerkki koodista tulee palauttaa tunnusta samalla tavalla kuin seuraavassa esimerkissä.

![Suojaustunnuksen][api-management-arm-token]

Ennen kuin puhelut varmuuskopiointi ja palauttaminen toimintoja, joka on kuvattu seuraavissa osissa, Määritä REST-puhelun authorization-pyynnön otsikko.

    request.Headers.Add(HttpRequestHeader.Authorization, "Bearer " + token);

## <a name="step1"> </a>Varmuuskopioi API Management-palvelu
Voit varmuuskopioida API hallinta palvelun myöntää HTTP-pyyntö:

`POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/backup?api-version={api-version}`

Jos:

* `subscriptionId`-tilauksen, joka sisältää API hallintapalvelun, jota yrität tunnus, varmuuskopiointi
* `resourceGroupName`-merkkijonon 'Api - oletusarvo-{service-alueen}' lomakkeessa kohtaa, johon `service-region` Azure alue, jossa API hallinta palvelun voit määrittää rooliin varmuuskopiointi nykyisessä, kuten`North-Central-US`
* `serviceName`-Ohjelmointirajapinnan Management-palvelun nimi teet varmuuskopion määritetty sen luomisen yhteydessä
* `api-version`-korvaaminen`2014-02-14`

Määritä pyynnön leipätekstissä kohde Azure-tallennustilan tilin nimi, pikanäppäin, blob-säilö nimi ja varmuuskopion nimi:

    '{  
        storageAccount : {storage account name for the backup},  
        accessKey : {access key for the account},  
        containerName : {backup container name},  
        backupName : {backup blob name}  
    }'

Määritä `Content-Type` pyynnön otsikko `application/json`.

Varmuuskopiointi on pitkä käynnissä toiminto, joka voi kestää useita minuutteja.  Jos pyyntö onnistui ja Varmuuskopiointi alkoi saat `202 Accepted` vastauksen tila-koodia, jossa `Location` otsikko.  Tee HAE pyynnöt URL-osoite `Location` otsikko Selvitä toiminnon tilan. Varmuuskopion ollessa käynnissä voit jatkossakin "202 hyväksytty" tilakoodi. Vastauksen koodi `200 OK` kertovat onnistumiseen varmuuskopiointia.

**Huomautus**:

- **Säilön** **on oltava**pyynnön-leipäteksti-parametrissa.
* Kun varmuuskopiointi on meneillään voit esimerkiksi SKU **Älä yritä service management toiminnot** päivittäminen tai aikaisempaan tiedostomuotoon toimialueen nimen muuttaminen jne. 
* Palauttaminen **Varmuuskopiointi on varmasti, vain 7 päivää** hetken, sen luomisen jälkeen. 
* Analytics luomiseen käytetyt **käyttötiedot** raportteja **ei** varmuuskopion. [Azure API hallinta REST API][] avulla voit hakea säännöllisesti säilytys analytics-raportit.
* Taajuus, jolla suoritat palvelun varmuuskopioiden vaikuttaa palautus-kohdan tavoitteen. Voit pienentää sen kannattaa varmuuskopioida säännöllisesti kohteet toteuttaminen sekä suorittamiseen tarvittaessa varmuuskopioiden API hallintapalvelun tärkeiden muutosten jälkeen.
* Palvelun määritykset (API, käytäntöjä, developer portaalin ulkoasu) varmuuskopioinnin aikana tehdyt **muutokset** on **ehkä sisällytetä varmuuskopion ja näin ollen menetetään**.

## <a name="step2"> </a>API hallintapalvelun palauttaminen
Palauttaa API hallinta palvelun aiemmin luodun varmuuskopiosta tehdä HTTP-pyyntö:

`POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/restore?api-version={api-version}`

Jos:

* `subscriptionId`-sisältävä palautat varmuuskopion kyselyjä API hallintapalvelun tilauksen tunnus
* `resourceGroupName`-merkkijonon 'Api - oletusarvo-{service-alueen}' lomakkeessa kohtaa, johon `service-region` tunnistaa Azure alue, johon palautat varmuuskopion kyselyjä API hallintapalvelun nykyisessä, kuten`North-Central-US`
* `serviceName`-palvelun kyselyjä palautetaan parhaillaan määritetty sen luomisen yhteydessä API hallinta nimi
* `api-version`-korvaaminen`2014-02-14`

Määritä pyynnön leipätekstissä varmuuskopiotiedoston sijainti, kuten Azure-tallennustilan tilin nimi, pikanäppäin, blob-säilö nimi ja varmuuskopion nimi:

    '{  
        storageAccount : {storage account name for the backup},  
        accessKey : {access key for the account},  
        containerName : {backup container name},  
        backupName : {backup blob name}  
    }'

Määritä `Content-Type` pyynnön otsikko `application/json`.

Palautus on pitkä käynnissä toiminto, joka saattaa kestää jopa 30 minuuttia suorittamiseen.  Jos pyyntö onnistui ja palautus alkoi saat `202 Accepted` vastauksen tila-koodia, jossa `Location` otsikko.  Tee HAE pyynnöt URL-osoite `Location` otsikko Selvitä toiminnon tilan. Palauta ollessa käynnissä voit jatkossakin "202 hyväksytty" tilakoodi. Vastauksen koodi `200 OK` kertovat palautustoiminto onnistumiseen.

>[AZURE.IMPORTANT] **Tuote** , palvelu on palautettu kyselyjä **on vastattava** varmuuskopioidut palvelu on palautettu tuote.
>
>Palvelun määritykset (API, käytäntöjä, developer portaalin ulkoasu) palautustoiminto aikana tehdyt **muutokset** on käynnissä, **voidaan korvata**.

## <a name="next-steps"></a>Seuraavat vaiheet
Katso seuraavat Microsoft blogit, kaksi eri vaiheittaisissa ohjeissa varmuuskopiointi ja palauttaminen prosessin varten.

-   [Toistaa Azure API hallinta-tilit](https://www.returngis.net/en/2015/06/replicate-azure-api-management-accounts/) 
    -   Kiitos, Gisela hänen osuus tässä artikkelissa.
-   [Azure API hallinta: Varmuuskopiointia ja palauttamista määritys](http://blogs.msdn.com/b/stuartleeks/archive/2015/04/29/azure-api-management-backing-up-and-restoring-configuration.aspx)
    -   Yksityiskohtaiset mukaan Stuart lähestymistapa vastaa virallinen ohjeet, mutta se on erittäin mielenkiintoista.

[Backup an API Management service]: #step1
[Restore an API Management service]: #step2


[Azure API hallinta REST-Ohjelmointirajapinnalla]: http://msdn.microsoft.com/library/azure/dn781421.aspx

[api-management-add-aad-application]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-add-aad-application.png

[api-management-aad-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions.png
[api-management-aad-permissions-add]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions-add.png
[api-management-aad-delegated-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-delegated-permissions.png
[api-management-aad-default-directory]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-default-directory.png
[api-management-aad-resources]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-resources.png
[api-management-arm-token]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-arm-token.png
[api-management-endpoint]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-endpoint.png
 

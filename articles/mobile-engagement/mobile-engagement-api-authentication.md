<properties 
    pageTitle="Mobiili välitys REST API todentamismenetelmä"
    description="Tässä artikkelissa käsitellään Azure Mobile välitys REST API todentamismenetelmä" 
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo"
    manager="erikre"
    editor=""/>

<tags
    ms.service="mobile-engagement"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="mobile-multiple"
    ms.workload="mobile" 
    ms.date="10/05/2016"
    ms.author="wesmc;ricksal"/>

# <a name="authenticate-with-mobile-engagement-rest-apis"></a>Mobiili välitys REST API todentamismenetelmä

## <a name="overview"></a>Yleiskatsaus

Tässä asiakirjassa kerrotaan, miten pääset kelvollinen AAD Oauth suojaustunnuksen Mobile välitys REST API todentamismenetelmä. 

Oletetaan, että sinulla on kelvollinen Azure tilaus ja olet luonut jollakin Microsoftin [Developer opetusohjelmat](mobile-engagement-windows-store-dotnet-get-started.md)välitys Mobile-sovelluksen.

## <a name="authentication"></a>Todennus

Microsoft Azure Active Directoryn perusteella OAuth tunnuksen käytetään todennusta varten. 

Jotta Ohjelmointirajapinnan todennuspyynnön authorization-otsikko on lisättävä jokaisen pyynnön, joka on seuraavassa muodossa:

    Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGmJlNmV2ZWJPamg2TTNXR1E...

>[AZURE.NOTE] Azure Active Directory-tunnusten päättyy 1 tunti.

Saat tunnusta useilla tavoilla. Koska pilvipalveluun yleensä kutsutut API-, jota haluat käyttää API-näppäintä. Ohjelmointirajapinnan avain Azure termejä kutsutaan palvelun pääasiallista salasana. Seuraavassa kuvataan, miten se asennetaan manuaalisesti yksi tapa.

### <a name="one-time-setup-using-script"></a>Erikseen asetukset (komentosarjan avulla)

Tee noudattamalla seuraavia ohjeita voit suorittaa käyttämällä PowerShell-komentosarjaa, jonka pienin aikaa asennuksen, mutta käyttää eniten sallittu oletusasetusten asetusten määrittäminen. Vaihtoehtoisesti voit noudata näin Azure-portaalista suoraan [Manuaalinen määritys](mobile-engagement-api-authentication-manual.md) ja tee tarkempaan määritys. 

1. Pyydä PowerShellin Azure uusin versio [tästä](http://aka.ms/webpi-azps). Katso lisätietoja latausohjeet tätä [linkkiä](../powershell-install-configure.md).  

2. Kun PowerShellin Azure on asennettu, voit varmistaa, että sinulla on asennettu **Azure moduuli** käyttää seuraavia komentoja:

    a. Varmista, että PowerShellin Azure-moduuli on käytettävissä moduulit-luettelossa. 
    
        Get-Module –ListAvailable 

    ![Käytettävissä olevat Azure moduulit][1]
        
    b. Jos et löydä Azure PowerShell-moduulin edellä olevassa luettelossa sinun on suorittamalla seuraava:
        
        Import-Module Azure 
        
3. Kirjautuminen Azure Resurssienhallinta PowerShell suorittamalla seuraava komento ja antamisen Azure-tilin käyttäjänimi ja salasana: 
        
        Login-AzureRmAccount

4. Jos sinulla on useita tilauksia, valitse pitäisi toimia seuraavasti:

    a. Luettelo kaikista tilauksistasi ja kopioi tilauksen, jota haluat käyttää SubscriptionId. Varmista, että tämä tilaus on sama jokin, joiden aiot käsitellä käyttämällä API Mobile välitys-sovelluksesta on. 

        Get-AzureRmSubscription

    b. Suorittamalla seuraavan komennon antamisen SubscriptionId, jota käytetään tilauksen määrittämiseen.

        Select-AzureRmSubscription –SubscriptionId <subscriptionId>

5. Kopioi teksti [Uusi AzureRmServicePrincipalOwner.ps1](https://raw.githubusercontent.com/matt-gibbs/azbits/master/src/New-AzureRmServicePrincipalOwner.ps1) komentosarjan paikallisessa tietokoneessa ja tallenna se PowerShell-cmdlet-komennon (kuten `APIAuth.ps1`) ja suorittaa sen `.\APIAuth.ps1`. 
    
6. Komentosarjan pyytää antamaan **principalName**syöte. Anna tähän sopiva nimi, jota haluat käyttää Active Directory-sovelluksen (kuten APIAuth) luominen. 

7. Kun komentosarja on valmis, se näyttää seuraavat neljä arvot, joita tarvitset todentamismenetelmä ohjelmallisesti AD joten varmista, että kopioi ne. 
        
    **TenantId**, **SubscriptionId**, **ApplicationId**ja **salaisuus**.

    Voit käyttää TenantId kuin `{TENANT_ID}`, ApplicationId kuin `{CLIENT_ID}` ja salaisen kuin `{CLIENT_SECRET}`.

    > [AZURE.NOTE] Oletus-suojauskäytäntö saattavat estää PowerShell-komentosarjojen suorittamisen. Jos näin on, voit määrittää tilapäisesti suorittamisen käytäntöjen sallimaan komentosarjan suorittaminen seuraava komento:

        > Set-ExecutionPolicy RemoteSigned

8. Näin miten PS cmdlet-komennot joukko näyttää. 

    ![][3]

9. Tarkista Azure hallinta-portaalissa, että uusi AD-sovellus on luotu antamasi nimi nimi-kohdassa **Näytä sovellusten yritykseni omistaa** **principalName** komentosarjan.

    ![][4]

#### <a name="steps-to-get-a-valid-token"></a>Toimia kelvollinen tunnus

1. Soita Ohjelmointirajapinnan seuraavilla parametreilla ja varmista, että korvaa VUOKRAAJAN\_tunnus ja asiakkaan\_tunnuksen ja asiakkaan\_SALAISUUS:

    - **Pyyntö URL-Osoitteen** *https://login.microsoftonline.com/ {VUOKRAAJAN\_tunnus} / oauth2/tunnuksen*
    - **HTTP-sisältötyypin otsikon** kuin *sovelluksen/x-www-form-urlencoded*
    - **HTTP pyytää leipätekstin** *myöntää\_tyyppi = asiakkaan\_tunnistetiedot & client_id = {asiakkaan\_tunnus} & client_secret = {asiakkaan\_SALAISUUS} & resource=https%3A%2F%2Fmanagement.core.windows.net%2F*

    Seuraavassa on esimerkki pyyntö:

        POST /{TENANT_ID}/oauth2/token HTTP/1.1
        Host: login.microsoftonline.com
        Content-Type: application/x-www-form-urlencoded
        grant_type=client_credentials&client_id={CLIENT_ID}&client_secret={CLIENT_SECRET}&reso
        urce=https%3A%2F%2Fmanagement.core.windows.net%2F

    Tässä on esimerkki vastaus:

        HTTP/1.1 200 OK
        Content-Type: application/json; charset=utf-8
        Content-Length: 1234
    
        {"token_type":"Bearer","expires_in":"3599","expires_on":"1445395811","not_before":"144
        5391911","resource":"https://management.core.windows.net/","access_token":{ACCESS_TOKEN}}

    Tässä esimerkissä sisällyttää VIESTIIN, parametrit URL-koodaus `resource` arvo on todella `https://management.core.windows.net/`. Varmista, ettet myös URL-osoitteen koodata `{CLIENT_SECRET}` koska siinä voi olla erikoismerkkejä.

    > [AZURE.NOTE] Testikäyttöön, voit käyttää HTTP-asiakasohjelma, kuten [Fiddler](http://www.telerik.com/fiddler) tai [Chrome Postman tunniste](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop) 

2. Sisältävät nyt API jokaisen kutsun luvan pyynnön otsikko:

        Authorization: Bearer {ACCESS_TOKEN}

    Jos palautettu 401 tilakoodi, tarkista vastauksen jotakin sen ehkä kertoa tunnus on vanhentunut. Tässä tapauksessa saat uuden tunnuksen.

##<a name="using-the-apis"></a>Käyttämällä API

Nyt kun sinulla on kelvollinen tunnus, olet valmis API-puheluissa.

1. API sivupyynnön sinun on kelvollinen, jäljellä tunnus, joka on saatu välittää edellisessä osassa.

2. Sinun on joitakin parametreja kutsuun URI, joka yksilöi sovelluksesi laajennus. Pyynnön URI näyttää seuraavalta

        https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/
        providers/Microsoft.MobileEngagement/appcollections/{app-collection}/apps/{app-resource-name}/

    Hakee parametrit, napsauta sovelluksen nimeä ja valitse sitten Raporttinäkymät-ikkunan ja tulee näkyviin 3 parametreilla seuraavalta sivulle.

    - **1**`{subscription-id}`
    - **2**`{app-collection}`
    - **3**`{app-resource-name}`
    - **4** your resurssiryhmä nimi on käyttäjästä tulee **MobileEngagement** , paitsi jos olet luonut uuden. 

    ![Mobile välitys API URI-parametrit][2]

>[AZURE.NOTE] <br/>
>1. Ohita API pääkansion osoite, tämä oli edellisen ohjelmointirajapinnan varten.<br/>
>2. Jos loit sovelluksen Azure perinteinen portaalissa sinun on käytettävä sovelluksen resurssin nimi on sama kuin itse sovelluksen nimeä. Jos loit sovelluksen Azure-portaalissa voit kannattaa käyttää sovelluksen nimen itse (ei ole eriyttäminen sovelluksen resurssinimi ja sovelluksen nimi uudessa portaalissa luodaan-sovellusten välillä).  

<!-- Images -->
[1]: ./media/mobile-engagement-api-authentication/azure-module.png
[2]: ./media/mobile-engagement-api-authentication/mobile-engagement-api-uri-params.png
[3]: ./media/mobile-engagement-api-authentication/ps-cmdlets.png
[4]: ./media/mobile-engagement-api-authentication/ad-app-creation.png




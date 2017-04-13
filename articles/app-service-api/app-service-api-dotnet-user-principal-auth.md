<properties
    pageTitle="Azure-sovelluksen Service API-sovellusten käyttöoikeuksien | Microsoft Azure"
    description="Opettele suojaa Azure App Service API-sovelluksen käyttöoikeudet vain todennetut käyttäjät sallimalla."
    services="app-service\api"
    documentationCenter=".net"
    authors="tdykstra"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-api"
    ms.workload="na"
    ms.tgt_pltfrm="dotnet"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/30/2016"
    ms.author="rachelap"/>

# <a name="user-authentication-for-api-apps-in-azure-app-service"></a>Azure-sovelluksen Service API-sovellusten käyttöoikeuksien

## <a name="overview"></a>Yleiskatsaus

Tässä artikkelissa kerrotaan, miten voit suojata Azure API-sovelluksen siten, että vain todennetut käyttäjät voivat soittaa. On artikkelissa oletetaan, että olet lukenut [Azure App palvelun todennus yleiskatsaus](../app-service/app-service-authentication-overview.md).

Opit:

* Miten määritetään todennus tarjoajan tiedot Azure Active Directory (Azure AD).
* Miten tarjoaman suojatun API-sovelluksen avulla [Active Directory käyttöoikeuksien kirjaston (ADAL) JavaScript varten](https://github.com/AzureAD/azure-activedirectory-library-for-js).

Artikkelissa on kaksi osaa:

* [Azure-sovelluksen palvelun käyttöoikeuksien määrittäminen](#authconfig) -osassa kerrotaan yleinen minkä tahansa API-sovelluksen käyttöoikeuksien määrittämisestä ja koskee myös kaikki kehysten tukemat App palvelun, kuten .NET, Node.js ja Java.

* [Jatkuvaa .NET API sovellukset-opetusohjelmat](#tutorialstart) osan artikkelissa apuviivat alkaen läpi määrittäminen sovelluksen malli .NET takaisin loppuun ja AngularJS edestä päättyy niin, että se vastaa Azure Active Directory-käyttäjän todennusta varten. 

## <a id="authconfig"></a>Azure-sovelluksen palvelun käyttöoikeuksien määrittämisestä

Tässä osassa on yleisiä ohjeita, jotka koskevat myös kaikkia API-sovelluksia. Ohjeet tiettyyn luettelon .NET Tee malli-sovellukseen Siirry [jatkuvaa .NET käytön aloittaminen-opetusohjelmat](#tutorialstart).

1. Siirry [Azure portal](https://portal.azure.com/)API-sovellus, jonka haluat suojata, **Ominaisuudet** -osan, ja valitse sitten **asetukset** -sivu **todennus / luvan**.

    ![Azure portal todennus ja todennus](./media/app-service-api-dotnet-user-principal-auth/features.png)

3. : **Todennus / luvan** sivu, **Valitse**.

4. Valitse jokin vaihtoehdoista **toiminto, joka suoritetaan, kun pyyntö ei ole todennettu** avattavasta luettelosta.

    * Jos haluat vain todennetut puheluiden saavuttamiseksi API-sovellus, valitse jokin vaihtoehdoista **kirjautua sisään...** . Tämän asetuksen avulla voit suojata API-sovelluksen kirjoittamatta koodia, joka suoritetaan ei.

    * Jos haluat kaikki kutsut saavuttamiseksi API-sovellus, valitse **Salli pyyntö (mitään toimia)**. Tämän asetuksen avulla voit ohjata Todentamattomille soittajien käyttöoikeustarkistuspalvelun valitsemaa. Tällä asetuksella sinulla on lupa käsittelemään koodin kirjoittaminen.

    Lisätietoja on artikkelissa [todennus- ja Azure App Service API varten](app-service-api-authentication.md#multiple-protection-options).

5. Valitse vähintään yksi seuraavista **Käyttöoikeustarkistuspalvelun**.

    Kuvassa näkyy valinnat, jotka edellyttävät kaikki soittajien Azure AD todennus.
 
    ![Azure-portaalin todennus/todennus-sivu](./media/app-service-api-dotnet-user-principal-auth/authblade.png)

    Valitessasi tarjoajan todennus-portaalin näyttää palvelun määritys-sivu. 

    Lisätietoja siitä, kuinka määrittäminen todennus tarjoajan määritys näiden on artikkelissa [Azure Active Directory-kirjautuminen käyttämään App palvelusovelluksen määrittämisestä](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md). (Linkki Azure AD artikkelista siirtyy, mutta itse artikkelin sisältää välilehtiä, joissa on linkki artikkeleihin muiden käyttöoikeustarkistuspalvelun.)

7. Kun enää tarvitse todennusta tarjoajan määritys-sivu, valitse **OK**.

7. - **Todennus / luvan** sivu, valitse **Tallenna**.

Kun tämä on valmis, sovelluksen palvelun todentaa kaikki API puhelut ennen niiden saapumista API-sovellus. Todennus-palvelut toimivat samalla kaikkien kielten, joka tukee sovelluksen palvelun, kuten .NET, Node.js ja Java. 

Soittajan sisältää soittamiseen todennetut API-todennus-palveluntarjoajan OAuth 2.0-haltijatunnukseen pyyntöjen Authorization-otsikko. Tunnuksen voi hankkia todennus-palveluntarjoajan SDK avulla.

## <a id="tutorialstart"></a>Jatkuvaa .NET API sovellukset-opetusohjelmat

Jos seuraat Node.js tai Java-opetusohjelmat API-sovellusten, siirry seuraavaan artikkelissa [service API-sovellusten pääasiallista tunnistus](app-service-api-dotnet-service-principal-auth.md). 

Jos seuraavat .NET opetusohjelmasarjan API-sovellusten ja olet jo asentanut sovelluksen malli [ensimmäisen](app-service-api-dotnet-get-started.md) ja [toisen](app-service-api-cors-consume-javascript.md) Opetusohjelmissa kuvatulla tavalla, siirry [sovelluksen ja Azure AD-todennuksen määrittäminen](#azureauth) -osassa.

Jos haluat seurata siirtymättä ensimmäisen ja toisen-opetusohjelmat – Tässä opetusohjelmassa, toimi seuraavasti, jossa kerrotaan, kuinka voit ottaa käyttöön sovelluksen malli automatisoidun prosessin avulla pääset alkuun.

>[AZURE.NOTE] Seuraavat vaiheet auttaa sinua saman aloituskohdan, ikään kuin teit kaksi ensimmäistä opetusohjelmat yhtä poikkeusta lukuun ottamatta: Visual Studio jo eivät tiedä, mitä verkkosovellukseen tai API-sovellusta, joka kunkin projektin saa käyttöön. Tämä tarkoittaa sitä ei ole tässä opetusohjelmassa tarkat ohjeet, joita kerrotaan, kuinka voit ottaa käyttöön oikealle kohteet. Jos et olet perehtynyt siihen, miten käyttöönoton toimet Omat käyttäminen, on parempi voit seurata opetusohjelmasarjan [Ensimmäinen opetusohjelma](app-service-api-dotnet-get-started.md) kuin voit aloittaa automaattisen käyttöönoton yhteydessä.

1. Varmista, että olet määrittänyt kaikki kuvatut [Ensimmäinen opetusohjelma](app-service-api-dotnet-get-started.md). Lisäksi edellytykset on lueteltu-todennus näissä Opetusohjelmissa oletetaan, että olet käsitellyt App palvelun web Apps-sovellusten ja Visual Studio ja Azure-portaalin API-sovellusten.

2. Napsauta [Tehtävät-luettelo otoksen säilöön Lueminut-tiedosto](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/readme.md) API-sovellukset ja web-sovelluksen käyttöönotto **käyttöönotto Azure** -painiketta. Pane merkille Azure resurssiryhmä, jotka luodaan, kun voit käyttää tätä myöhemmin hakeminen web app- ja API app nimet.
 
3. Lataa tai Kloonaa [tehtäväluetteloa otoksen säilöön](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list) saat tunnus, jolla käsittelet paikallisesti Visual Studiossa.

## <a id="azureauth"></a>Sovelluksen ja Azure AD-todennuksen määrittäminen

Olet nyt käynnissä Azure-sovelluksen palvelun tarvitsematta, käyttäjien todennus-sovelluksen. Tässä osassa Lisää todennus tekemällä seuraavat toimet:

* Määritä sovellus-palvelu edellyttää Azure Active Directory (Azure AD) todennusta soitettavien Keskitaso API-sovellus.
* Azure AD-sovelluksen luominen.
* Voit lähettää kirjautumisen jälkeen haltijatunnukseen AngularJS edusta Azure AD-sovelluksen kokoonpanon määrittäminen. 

Jos kohtaat ongelmia aikana opetusohjelman ohjeiden, on kohdassa [vianmääritys](#troubleshooting) opetusohjelman lopussa. 
 
### <a name="configure-authentication-for-the-middle-tier-api-app"></a>Keskitason API-sovelluksen käyttöoikeuksien määrittäminen

1. Siirry [Azure portal](https://portal.azure.com/)- **asetukset** -sivu, jonka loit API sovelluksen ToDoListAPI projektin, Etsi **Ominaisuudet** -osassa ja valitse sitten **todennus / luvan**.

    ![Azure portal todennus ja todennus](./media/app-service-api-dotnet-user-principal-auth/features.png)

3. : **Todennus / luvan** sivu, **Valitse**.

4. Valitse **toiminto, joka suoritetaan, kun pyyntö ei ole todennettu** avattavasta luettelosta, **Kirjaudu sisään Azure Active Directory**.

    Tämä asetus varmistaa, että unauathenticated ei ole pyynnöt saavuttaa API-sovellus. 

5. Valitse **Käyttöoikeustarkistuspalvelun** **Azure Active Directory**.

    ![Azure-portaalin todennus/todennus-sivu](./media/app-service-api-dotnet-user-principal-auth/authblade.png)

6. **Azure Active Directory-asetukset** -sivu valitsemalla **Pika-asennus**

    ![Azure portaalin todennus/luvan Express-vaihtoehto](./media/app-service-api-dotnet-user-principal-auth/aadsettings.png)

    **Express** -vaihtoehdon App palvelun voidaan luoda automaattisesti Azure AD-sovelluksen Azure AD- [vuokraajan](https://msdn.microsoft.com/en-us/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant). 

    Sinun ei tarvitse luoda vuokraajan, koska jokaisella Azure-tili on automaattisesti yksi.

7. Valitse **hallinta-tilassa**valitsemalla **Luo uusi AD-sovelluksesta** , jos se ei ole jo avattuna ja huomaat arvo, joka on **Luo sovelluksesta** teksti-ruutuun. etsiä AAD sovelluksen Azure perinteinen portaalissa myöhemmin.

    ![Azure portal Azure AD-asetukset](./media/app-service-api-dotnet-user-principal-auth/aadsettings2.png)

    Azure luo automaattisesti Azure AD-sovelluksen Azure AD-vuokraajan määritettyihin nimeen. Oletusarvon mukaan Azure AD-sovelluksen nimi on sama kuin API-sovellus. Jos käytät mieluummin, voit kirjoittaa eri nimellä.
 
7. Valitse **OK**.

7. - **Todennus / luvan** sivu, valitse **Tallenna**.

    ![Valitse Tallenna](./media/app-service-api-dotnet-user-principal-auth/authsave.png)

Nyt vain Azure AD-vuokraajan-käyttäjät voivat soittaa API-sovellus.

### <a name="optional-test-the-api-app"></a>Valinnainen: Testaa API-sovellus

1. Siirry selaimella API-sovelluksen URL-osoite: Napsauta **API app** sivu Azure-portaalissa, **URL-osoitteeseen**linkkiä.  

    Sinut ohjataan kirjautumisen näyttöön, koska Todentamattomille pyynnöt eivät ole sallittuja saavuttamiseksi API-sovellus.

    Jos selaimesi siirtyy "onnistuneesti luomaasi"-sivua, selaimessa voi jo kirjautunut edelleen – tässä tapauksessa Avaa InPrivate- tai Incognito-ikkuna ja siirry API-sovelluksen URL-osoite.

2. Kirjaudu sisään käyttämällä Azure AD-vuokraajan käyttäjän tunnistetietoja.

    Kun olet kirjautunut, "onnistuneesti luotu" sivu näkyy selaimessa.

9. Sulje selain.

### <a name="configure-the-azure-ad-application"></a>Azure AD-sovelluksen kokoonpanon määrittäminen

Kun olet määrittänyt Azure AD-todennus, sovelluksen palvelun Luo Azure AD-sovelluksen. Oletusarvoisesti uusi Azure AD haltijatunnukseen tarjota API-sovelluksen URL-osoite on määritetty sovelluksen. Tässä osassa Määritä Azure AD-sovelluksen antamaan haltijatunnukseen, AngularJS edusta sijaan suoraan Keskitaso API-sovelluksen avulla. AngularJS edusta lähettää tunnuksen API-sovellukseen, kun kutsuu API-sovellus.

>[AZURE.NOTE] Prosessin säilyttää mahdollisimman yksinkertainen, tämän opetusohjelman käyttää yhden Azure AD edusta-ja Keski taso API-sovellus. Toinen vaihtoehto on käyttää kaksi Azure AD-sovelluksia. Tässä tapauksessa sinun on antaa edusta Azure AD sovelluksen oikeudet Soita Keskitaso Azure AD-sovellus. Usean sovelluksen tämän menetelmän helmi eritellympiä hallintaoikeutta kunkin tason käyttöoikeudet.

11. Siirry [Azure perinteinen portal](https://manage.windowsazure.com/) **Azure Active Directory**.

    Sinun on käytettävä perinteinen portaalin, sillä tarvitset Azure Active Directory tietyt asetukset eivät ole vielä käytettävissä nykyisen Azure-portaalissa.

12. **Hakemisto** -välilehdessä AAD alihallintaan.

    ![Azure AD perinteinen-portaalissa](./media/app-service-api-dotnet-user-principal-auth/selecttenant.png)

14. Valitse **sovellukset > sovellukset yritykseni omistaa**, ja valitse sitten valintamerkkiä.

    Voit myös joutua päivittämään sivulle nähdäkseen uusi sovellus.

15. Valitse sovellukset-luettelosta haluamasi vaihtoehto Azure luodaan todennus on käytössä API-sovelluksen nimi.

    ![Azure AD-sovellukset-välilehti](./media/app-service-api-dotnet-user-principal-auth/appstab.png)

16. Valitse **Määritä**.

    ![Azure AD-määritys-välilehti](./media/app-service-api-dotnet-user-principal-auth/configure.png)

17. Määritä **Sign URL** AngularJS koodiin, ei ole vinoviiva URL-osoitteeseen.

    Esimerkki: https://todolistangular.azurewebsites.net

    ![Sign-on URL-osoite](./media/app-service-api-dotnet-user-principal-auth/signonurlazure.png)

17. Määritä **Vastaa URL-Osoitteen** web app ei ole vinoviiva URL-osoitteeseen.

    Esimerkki: https://todolistsangular.azurewebsites.net

16. Valitse **Tallenna**.

    ![Vastaa URL-osoite](./media/app-service-api-dotnet-user-principal-auth/replyurlazure.png)

15. Valitse sivun alareunassa **hallinta-luettelo > Lataa luettelo**.

    ![Lataa luettelo](./media/app-service-api-dotnet-user-principal-auth/downloadmanifest.png)

17. Lataa tiedosto uuteen paikkaan, jossa voit muokata sitä.

16. Etsi luettelon ladattu tiedosto `oauth2AllowImplicitFlow` ominaisuus. Muuta tämän ominaisuuden arvoa `false` , `true`, ja tallenna sitten tiedosto.

    Tämä asetus on pakollinen access JavaScript-sivu-sovelluksesta. Sen avulla voidaan palauttaa URL-Osoitteen osa Oauth 2.0-haltijatunnukseen.

16. Valitse **hallinta-luettelo > Lataa luettelo**, ja lataa tiedosto, jonka olet päivittänyt edellisessä vaiheessa.

    ![Lataa luettelo](./media/app-service-api-dotnet-user-principal-auth/uploadmanifest.png)

17. Kopioi **Asiakastunnus** -arvo ja tallenna se muualla voit avata sen myöhemmin.

## <a name="configure-the-todolistangular-project-to-use-authentication"></a>Määritä todennus käyttämään ToDoListAngular-projekti

Tässä osassa voit muuttaa AngularJS edusta niin, että se käyttää Active Directory käyttöoikeuksien kirjaston (ADAL), JS hankkimaan haltijatunnukseen Azure AD-on käyttäjiä varten. Koodin sisällyttää tunnuksen pyyntöjen lähetetään Keskitaso, kuten seuraavassa kaaviossa on esitetty. 

![Käyttäjän todentaminen-kaavio](./media/app-service-api-dotnet-user-principal-auth/appdiagram.png)

Tee seuraavat muutokset ToDoListAngular projektin tiedostoja.

1. Avaa *index.html* tiedosto.

2. Kommentointi rivit, jotka viittaavat Active Directory käyttöoikeuksien kirjaston (ADAL) JS komentosarjojen.

        <script src="app/scripts/adal.js"></script>
        <script src="app/scripts/adal-angular.js"></script>

1. Avaa *app/scripts/app.js* -tiedosto.

2. Merkitty "ilman todennusta"-koodin lohko, kommentoi ja kommentointi merkitty todennustavaksi"" koodin tekstialueen.

    Tämä muutos viittaa ADAL JS käyttöoikeustarkistuspalvelun ja tarjoaa siihen määritysten arvot. Seuraavia ohjeita voit määrittää API-sovellus ja Azure AD-sovelluksen kokoonpanon arvot.

8. Koodin, joka määrittää `endpoints` muuttuja-arvoksi API URL-osoite URL-Osoitteen API-sovelluksen ToDoListAPI projektin luotu ja määritetty Azure AD-Sovellustunnus, jonka kopioit Azure perinteinen portaalin Asiakastunnus.

    Koodi on nyt samalla tavalla kuin seuraavassa esimerkissä.

        var endpoints = {
            "https://todolistapi0121.azurewebsites.net/": "1cf55bc9-9ed8-4df31cf55bc9-9ed8-4df3"
        };

9. Kutsussa `adalProvider.init`, aseta `tenant` vuokraajan nimi ja `clientId` saman arvoa käytetään edellisessä vaiheessa.

    Koodi on nyt samalla tavalla kuin seuraavassa esimerkissä.

        adalProvider.init(
            {
                instance: 'https://login.microsoftonline.com/', 
                tenant: 'contoso.onmicrosoft.com',
                clientId: '1cf55bc9-9ed8-4df31cf55bc9-9ed8-4df3',
                extraQueryParameter: 'nux=1',
                endpoints: endpoints
            },
            $httpProvider
            );

    Nämä muutokset `app.js` määrittää, että puheluja koodi ja kutsuttu Ohjelmointirajapinnan samassa Azure AD-sovelluksessa.

1. Avaa *app/scripts/homeCtrl.js* -tiedosto.

2. Merkitty "ilman todennusta"-koodin lohko, kommentoi ja kommentointi merkitty todennustavaksi"" koodin tekstialueen.

1. Avaa *app/scripts/indexCtrl.js* -tiedosto.

2. Merkitty "ilman todennusta"-koodin lohko, kommentoi ja kommentointi merkitty todennustavaksi"" koodin tekstialueen.

### <a name="deploy-the-todolistangular-project-to-azure"></a>Ota käyttöön Azure ToDoListAngular-projekti

8. **Ratkaisunhallinnassa**ToDoListAngular projektin hiiren kakkospainikkeella ja valitse sitten **Julkaise**.

9. Valitse **Julkaise**.

    Visual Studio ottaa käyttöön projektin ja avaa web-sovelluksen Perusosoitteen selain. Tämä toiminto näyttää 403-virhesivu, jolla on tavallista, siirry verkko-Ohjelmointirajapinnan Perusosoitteen selaimesta yritettiin.

    On edelleen muutosten tekeminen Keskitaso API-sovellukseen, ennen kuin voit testata sovellus.

10. Sulje selain.

## <a name="configure-the-todolistapi-project-to-use-authentication"></a>Määritä todennus käyttämään ToDoListAPI-projekti

Tällä hetkellä ToDoListAPI projektin lähettää "*" nimellä `owner` ToDoListDataAPI arvon. Tässä osassa voit muuttaa koodin Käyttäjätunnus-on, Lähetä.

Tee seuraavat muutokset ToDoListAPI Projectissa.

1. Avaa *Controllers/ToDoListController.cs* -tiedosto ja kommentointi rivi, joka määrittää toiminnon kummassakin menetelmässä on `owner` , Azure AD `NameIdentifier` väittää arvo. Esimerkki:

        owner = ((ClaimsIdentity)User.Identity).FindFirst(ClaimTypes.NameIdentifier).Value;

    **Tärkeää**: et kommentointi-koodi `ToDoListDataAPI` menetelmä; Tee, myöhemmin palvelun pääasiallista todennus opetusohjelmaan.

### <a name="deploy-the-todolistapi-project-to-azure"></a>Ota käyttöön Azure ToDoListAPI-projekti

8. **Ratkaisunhallinnassa**ToDoListAPI projektin hiiren kakkospainikkeella ja valitse sitten **Julkaise**.

9. Valitse **Julkaise**.

    Visual Studio ottaa käyttöön projektin ja Avaa selaimessa API-sovelluksen Perusosoitteen.

10. Sulje selain.

### <a name="test-the-application"></a>Sovelluksen testaaminen

9. Siirry **käyttämällä HTTPS-ei HTTP**web Appin URL-osoite.

8. Valitse **Tehtävät-luettelo** -välilehti.

    Sinua kehotetaan Kirjaudu sisään.

9. Kirjaudu sisään vuokraajan AAD käyttäjän tunnistetiedot.

10. **Tehtävät-luettelo** -sivu tulee näkyviin.

    ![Sivun luettelo](./media/app-service-api-dotnet-user-principal-auth/webappindex.png)

    Tehtäväkohteita ei ole näkyvät, koska toistaiseksi ne kaikki on omistajan "*". Nyt Keskitaso pyytää kohteet-on käyttäjän ja ei ole vielä luotu.

11. Lisää uusi Tehtäväkohteita ja tarkista, että sovellus toimii.

12. Toisessa selainikkunassa, siirry Käyttöliittymän URL Swagger ToDoListDataAPI API-sovellus ja valitse **ToDoList > Hae**. Kirjoita tähti varten `owner` parametri, ja valitse sitten **Kokeile suoritetaan**.

    Vastaus näkyy, että uusi Tehtäväkohteita on todellinen Azure AD-Käyttäjätunnus omistaja-ominaisuudessa.

    ![JSON saatuaan omistajan tunnus](./media/app-service-api-dotnet-user-principal-auth/todolistapiauth.png)


## <a name="building-the-projects-from-scratch"></a>Luominen alusta alkaen projektit

Koontiprojekti verkko-Ohjelmointirajapinnan on luotu käyttämällä **Azure API App** project-malli ja oletusarvoisen arvot ohjauskoneen tilalle ToDoList ohjauskoneen. 

Lisätietoja verkko-Ohjelmointirajapinnan 2 taustatietokannaksi AngularJS yhden sovelluksen luomisesta on artikkelissa [kädet-testiympäristössä: Muodosta yhden sivun sovelluksen (SPA) ASP.NET-verkko-Ohjelmointirajapinnan ja Angular.js](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs). Lisätietoja Azure AD-todennus-koodin lisääminen on seuraavissa resursseissa:

* [Suojaaminen AngularJS yksi sivu-sovellusten käyttäminen Azure AD kanssa](../active-directory/active-directory-devquickstarts-angular.md).
* [ADAL JS v1 esittely](http://www.cloudidentity.com/blog/2015/02/19/introducing-adal-js-v1/)

## <a name="troubleshooting"></a>Vianmääritys

[AZURE.INCLUDE [troubleshooting](../../includes/app-service-api-auth-troubleshooting.md)]

* Varmista, että älä sekoita ToDoListAPI (Keskitaso) ja ToDoListDataAPI (tietojen taso). Tarkista esimerkiksi todennus lisätään Keskitaso API-sovelluksessa ei ole tietoja taso. 
* Varmista, että AngularJS lähdekoodin viittaa Keskitaso API sovelluksen URL-osoite (ToDoListAPI, ei ToDoListDataAPI) ja oikean Azure AD asiakkaan ID-tunnuksellasi. 

## <a name="next-steps"></a>Seuraavat vaiheet

Tässä opetusohjelmassa opit käyttämisestä sovelluksen todentaminen API-sovelluksen ja niiden puhelun API-sovellus käyttämällä ADAL JS-kirjasto. Opit seuraavaan opetusohjelmaan miten [API sovelluksen palvelun skenaarioissa suojattu käyttö](app-service-api-dotnet-service-principal-auth.md).


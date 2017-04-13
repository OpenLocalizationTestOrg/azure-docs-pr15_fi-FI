<properties
    pageTitle="Lainan todentaminen Azure App Service API-sovellusten | Microsoft Azure"
    description="Opettele suojaa palvelun skenaariot Azure App Service API apusovellus."
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

# <a name="service-principal-authentication-for-api-apps-in-azure-app-service"></a>Lainan todentaminen Azure App Service API varten

## <a name="overview"></a>Yleiskatsaus

Tässä artikkelissa kerrotaan, miten voit käyttää sovelluksen todentaminen *sisäinen* käyttämään API-sovelluksia. Sisäinen skenaario on kohtaa, johon sinulla on API-sovellus, jonka haluat voidaan kulutettavia vain sovelluksen oman koodilla. Toteuta Tämä skenaario App palvelun myös Azure AD avulla voit suojata kutsuttu API-sovelluksen avulla. Voit soittaa suojatun API-sovelluksessa, jossa haltijatunnukseen, jonka saat Azure AD antamalla sovelluksen käyttäjätiedot (service principal) tunnistetiedot. Katso vaihtoehtojen avulla Azure AD- [Azure App palvelun yleiskatsaus authentication](../app-service/app-service-authentication-overview.md#service-to-service-authentication) **palvelun todennus** -osa.

Tässä artikkelissa käydään:

* Voit suojata API-sovelluksen Todentamattomille Accessista Azure Active Directory (Azure AD) avulla.
* Miten tarjoaman suojatun API-sovelluksen API-sovelluksen, web App-sovelluksen tai mobiilisovelluksessa käyttämällä Azure AD-palvelun lyhennys (sovelluksen käyttäjätiedot) tunnistetietoja. Tietoja tarjoaman logiikan-sovelluksesta on artikkelissa [mukautetun API isännöimät logiikan sovellukset App-palvelun avulla](../app-service-logic/app-service-logic-custom-hosted-api.md).
* Voit varmistaa, että suojatun API-sovellus ei voi kutsua selaimesta kirjautunut käyttäjät.
* Miten varmistaaksesi, että suojatun API-sovellus voidaan kutsua vain mukaan tietyn Azure AD palvelun lyhennyksen.

Artikkelissa on kaksi osaa:

* [Lainan todentaminen Azure-sovelluksen palvelun määrittäminen](#authconfig) -kohdassa kerrotaan yleinen minkä tahansa API-sovelluksen käyttöoikeuksien määrittäminen ja tarjoaman suojatun API-sovellus. Tässä osassa koskee myös kaikki kehysten tukemat App palvelun, kuten .NET, Node.js ja Java.

* [Jatkuvaa .NET käytön aloittaminen-opetusohjelmat](#tutorialstart) -osassa alkaen opetusohjelman opastaa määrittäminen "sisäiseen käyttöön"-skenaario .NET otoksen sovelluksen käynnissä sovelluksen-palvelussa. 

## <a id="authconfig"></a>Lainan todentaminen määrittämisestä Azure sovelluksen-palvelussa

Tässä osassa on yleisiä ohjeita, jotka koskevat myös kaikkia API-sovelluksia. Ohjeet tiettyyn luettelon .NET Tee malli-sovellukseen Siirry [jatkuvaa .NET API sovellusten opetusohjelmasarjan](#tutorialstart).

1. Siirry [Azure portal](https://portal.azure.com/)API-sovellus, jonka haluat suojata, ja Etsi **Ominaisuudet** -osassa ja valitse **asetukset** -sivu **todennus / luvan**.

    ![Todennus ja luvan Azure-portaalissa](./media/app-service-api-dotnet-user-principal-auth/features.png)

3. : **Todennus / luvan** sivu, **Valitse**.

4. Valitse **toiminto, joka suoritetaan, kun pyyntö ei ole todennettu** avattavasta luettelosta, **Kirjaudu sisään Azure Active Directory** .

5. Valitse **Käyttöoikeustarkistuspalvelun** **Azure Active Directory**.

    ![Todennus ja luvan sivu Azure-portaalissa](./media/app-service-api-dotnet-user-principal-auth/authblade.png)

6. Määritä **Azure Active Directory-asetukset** -sivu, voit luoda uuden Azure AD-sovelluksen tai Käytä aiemmin luotua Azure AD-sovellusta, jos sinulla on jo jotakin, jota haluat käyttää.

    Sisäinen skenaariot liittyy yleensä API-sovelluksen kutsumista API-sovelluksen. Voit käyttää eri Azure AD-sovellusten kunkin API-sovelluksen tai vain yhden Azure AD-sovelluksen.

    Katso tarkat ohjeet tämän sivu- [sovelluksen palvelusovelluksen Azure Active Directory-kirjautuminen käyttämään määrittämisestä](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).

7. Kun enää tarvitse todennusta tarjoajan määritys-sivu, valitse **OK**.

7. - **Todennus / luvan** sivu, valitse **Tallenna**.

    ![Valitse Tallenna](./media/app-service-api-dotnet-service-principal-auth/authsave.png)

Kun tämä on valmis, sovellus-palvelun avulla vain pyynnöt-kohdassa määritetty soittajat Azure AD-vuokraajan. Todennuksen tai lupaa koodia tarvitaan suojatun API-sovelluksessa. Haltijatunnukseen välitetään API-sovelluksen sekä usein käytetyt saatavat HTTP-otsikon, ja voit lukea koodi tarkistaa, että pyynnöt ovat peräisin tietyn soittajan, kuten palvelun lyhennys tiedot.

Todennus-toiminto toimii samalla tavalla kaikissa kielissä, joka tukee sovelluksen palvelun, kuten .NET, Node.js ja Java. 

#### <a name="how-to-consume-the-protected-api-app"></a>Miten tarjoaman suojatun API-sovellus

Soittajan on annettava Azure AD-haltijatunnukseen API-kutsuja. Saat haltijatunnukseen, käyttämällä palvelun pääasiallista tunnistetietoja-soittajan käyttää Active Directory-todennus kirjaston (ADAL [.NET](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory), [Node.js](https://github.com/AzureAD/azure-activedirectory-library-for-nodejs)tai [Java](https://github.com/AzureAD/azure-activedirectory-library-for-java)). Saat tunnus-tunnus, jolla soittaa ADAL tarjoaa ADAL seuraavat tiedot:

* Azure AD-vuokraajan nimi.
* Ostajantunnus ja soittajan liittyvät Azure AD-sovelluksen toiminta asiakkaan (sovelluksen avain).
* Suojatun API-sovellukseen kytketty Azure AD-sovelluksen Asiakastunnus. (Jos käytössä on vain yksi Azure AD-sovellus, tämä on sama Asiakastunnus soittajan, jota.)

Nämä arvot ovat käytettävissä [Azure perinteinen portal](https://manage.windowsazure.com/)Azure AD-sivuilla.

Tunnuksen on hankittu, kun soittaja sisältää sen HTTP-pyyntöjen Authorization-otsikko.  Sovelluksen palvelun vahvistaa tunnuksen ja pyynnöt saavuttamiseksi suojatun API-sovelluksen avulla.

#### <a name="how-to-protect-the-api-app-from-access-by-users-in-the-same-tenant"></a>Suojaaminen API-sovelluksen käyttäjien samassa alihallinnassa Accessista

Haltijan-tunnukset käyttäjille samassa alihallinnassa pidetään kelvollinen suojatun API-sovellukseen.  Jos haluat varmistaa, että vain palvelun lyhennys soittaa suojatun API-sovellus, lisää koodi tarkistaa seuraavat saatavat tunnuksesta suojatun API-sovelluksessa:

* `appid`Azure AD-sovellus, joka on liitetty soittajan Asiakastunnus on oltava. 
* `oid`(`objectidentifier`) on oltava soittajan palvelun pääasiallista tunnus. 

Sovelluksen-palvelu sisältää myös `objectidentifier` väittää X-MS-asiakas-lyhennys-ID-otsikossa.

### <a name="how-to-protect-the-api-app-from-browser-access"></a>API-sovelluksen suojaaminen selaimen Accessista

Jos Älä tarkista saatavat koodissa suojatun API-sovelluksessa ja käytät erillistä Azure AD-sovelluksen suojatun API-sovellukseen, varmista, että Azure AD-sovelluksen vastaa URL-osoite ei ole sama kuin API-sovelluksen Perusosoitteen. Jos vastaa URL-osoite viittaa suoraan suojatun API-sovellus, käyttäjää saman Azure AD-vuokraajan voi selata API-sovellukseen, kirjaudu sisään ja soita onnistuneesti Ohjelmointirajapinnan.

## <a id="tutorialstart"></a>.NET API sovellusten opetusohjelmasarjan jatkaminen

Jos seuraat API-sovellusten Node.js tai Java opetusohjelmasarjan, siirry [seuraavat vaiheet](#next-steps) -osassa. 

Jäljempänä tässä artikkelissa jatkuu .NET API sovellusten opetusohjelmasarjan ja oletetaan, että suorittanut [Käyttäjän todentaminen-opetusohjelma](app-service-api-dotnet-user-principal-auth.md) ja on käyttäjän todennus on käytössä käytön Azure-sovelluksen malli.

## <a name="set-up-authentication-in-azure"></a>Azure todentamisen määrittäminen

Tässä osassa määrität sovelluksen palvelun niin, että se sallii saavuttamiseksi tietojen tason API-sovelluksen vain pyyntöjen ovat niistä, joilla on kelvollinen Azure AD haltijan-tunnukset. 

Seuraavassa osassa voit määrittää sovelluksen tunnistetiedot lähetetään Azure AD, pääset takaisin haltijatunnukseen ja Lähetä haltijatunnukseen tietojen tason API-sovellukseen Keskitaso API-sovellus. Tämä toimenpide on esitelty kaaviossa.

![Palvelun todennus-kaavio](./media/app-service-api-dotnet-service-principal-auth/appdiagram.png)

Jos kohtaat ongelmia aikana opetusohjelman ohjeiden, on kohdassa [vianmääritys](#troubleshooting) opetusohjelman lopussa. 

1. Valitse [Azure portal](https://portal.azure.com/)- **asetukset** -sivu, jonka loit API sovelluksen ToDoListDataAPI (tietojen taso) API-sovelluksen Siirry ja valitse sitten **asetukset**.

2. Etsi **Ominaisuudet** -osan **asetukset** -sivu ja valitse sitten **todennus / luvan**.

    ![Todennus ja luvan Azure-portaalissa](./media/app-service-api-dotnet-user-principal-auth/features.png)

3. : **Todennus / luvan** sivu, **Valitse**.

4. Valitse **toiminto, joka suoritetaan, kun pyyntö ei ole todennettu** avattavasta luettelosta, **Kirjaudu sisään Azure Active Directory**.

    Tämä asetus, joka aiheuttaa sovelluksen varmistamiseksi palvelu, joka todentaa vain pyynnöt reach API-sovellus on. Pyyntöjen, joilla on kelvollinen haltijan-tunnukset sovelluksen palvelu välittää pitkin tunnusten API-sovellukseen ja lisää tavallisimmat saatavat, voit jakaa tietoja helpommin koodisi HTTP-otsikot.

5. Valitse **Käyttöoikeustarkistuspalvelun** **Azure Active Directory**.

    ![Todennus ja luvan sivu Azure-portaalissa](./media/app-service-api-dotnet-user-principal-auth/authblade.png)

6. Valitse **Express** **Azure Active Directory-asetukset** -sivu.

    **Express** vaihtoehto Azure voidaan luoda automaattisesti AAD sovelluksen Azure AD- [vuokraajan](https://msdn.microsoft.com/en-us/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant). 

    Sinun ei tarvitse luoda vuokraajan, koska jokaisella Azure-tili on automaattisesti yksi.

7. Valitse **hallinta-tila**-kohdassa **Luo uusi AD-sovelluksesta** , jos se ei ole jo avattuna.

    Portaalin liitetään oletusarvoista **Luo sovelluksesta** tekstiruudussa. Oletusarvon mukaan Azure AD-sovelluksen nimi on sama kuin API-sovellus. Jos käytät mieluummin, voit kirjoittaa eri nimellä.
    
    ![Azure AD-asetukset](./media/app-service-api-dotnet-service-principal-auth/aadsettings.png)

    **Huomautus**: vaihtoehtona, voit käyttää yhden Azure AD sovelluksen puheluja API-sovellus ja suojatun API-sovellus. Jos olet valinnut, että vaihtoehtoinen, ei on tähän **Luo uusi AD-sovelluksesta** -vaihtoehto, koska olet jo luonut Azure AD-sovelluksen aiemmassa käyttäjän todennusta opetusohjelman. Tässä opetusohjelmassa, käytät erota Azure AD-sovellusten puheluja API-sovellus ja suojatun API-sovellus.

8. Pane merkille arvo, joka on **Luo sovelluksesta** tekstiruudussa; etsiä AAD sovelluksen Azure perinteinen portaalissa myöhemmin.

7. Valitse **OK**.

10. - **Todennus / luvan** sivu, valitse **Tallenna**.

    ![Valitse Tallenna](./media/app-service-api-dotnet-service-principal-auth/saveauth.png)

    Sovelluksen palvelu luo Azure Active Directory-sovelluksen kanssa **Sign URL** ja **Vastaa URL-osoite** Määritä automaattisesti API sovelluksen URL-osoite. Tämä arvo käyttäjät AAD vuokraajan käyttää API-sovellus ja kirjaudu sisään.

### <a name="verify-that-the-api-app-is-protected"></a>Varmista, että API-sovellus on suojattu

1. Siirry selaimella API-sovelluksen URL-osoite: Napsauta **API app** sivu Azure-portaalissa, **URL-osoitteeseen**linkkiä. 

    Sinut ohjataan kirjautumisen näyttöön, koska Todentamattomille pyynnöt eivät ole sallittuja saavuttamiseksi API-sovellus. 

    Jos selaimesi siirtyä Swagger-Käyttöliittymä, selaimessa voi jo kirjautunut edelleen – tässä tapauksessa Avaa InPrivate- tai Incognito-ikkuna ja siirry Swagger Käyttöliittymän URL-osoite.

18. Kirjaudu sisään vuokraajan AAD käyttäjän tunnistetiedot.

    Kun olet kirjautunut, "onnistuneesti luotu" sivu näkyy selaimessa.

## <a name="configure-the-todolistapi-project-to-acquire-and-send-the-azure-ad-token"></a>Määritä hankkia ja lähettää Azure AD-tunnuksen ToDoListAPI-projekti

Tässä osassa voit tehdä seuraavia toimintoja:

* Lisää koodi Keskitaso API-sovelluksessa, joka käyttää Azure AD-sovelluksen tunnistetiedot hankkia tunnuksen ja lähettää sen pyyntöjen tietojen tason API-sovellukseen.
* Pyydä Azure AD tarvitset tunnistetiedot.
* Azure-sovelluksen palvelun suorituksenaikainen-ympäristöasetuksia Keskitaso API-sovelluksen voit kirjoittaa tunnistetiedot. 

### <a name="configure-the-todolistapi-project-to-acquire-and-send-the-azure-ad-token"></a>Määritä hankkia ja lähettää Azure AD-tunnuksen ToDoListAPI-projekti

Tee seuraavat muutokset ToDoListAPI projektia Visual Studiossa.

1. Kommentointi koko koodi *ServicePrincipal.cs* -tiedostossa.

    Tämä on koodi, joka käyttää ADAL .NET hankkia Azure AD-haltijatunnukseen.  Se käyttää useita määritysarvoja, jotka määrität Azure runtime-ympäristössä myöhemmin. Näin koodi: 

        public static class ServicePrincipal
        {
            static string authority = ConfigurationManager.AppSettings["ida:Authority"];
            static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
            static string clientSecret = ConfigurationManager.AppSettings["ida:ClientSecret"];
            static string resource = ConfigurationManager.AppSettings["ida:Resource"];
        
            public static AuthenticationResult GetS2SAccessTokenForProdMSA()
            {
                return GetS2SAccessToken(authority, resource, clientId, clientSecret);
            }
        
            static AuthenticationResult GetS2SAccessToken(string authority, string resource, string clientId, string clientSecret)
            {
                var clientCredential = new ClientCredential(clientId, clientSecret);
                AuthenticationContext context = new AuthenticationContext(authority, false);
                AuthenticationResult authenticationResult = context.AcquireToken(
                    resource,
                    clientCredential);
                return authenticationResult;
            }
        }

    **Huomautus:** Koodi edellyttää .NET NuGet-paketti (Microsoft.IdentityModel.Clients.ActiveDirectory), joka on jo asennettu projektin ADAL. Jos luot projektin luominen alusta lähtien, sinun on asentanut. Tämän paketin ei ole asennettu automaattisesti API sovelluksen uusi projekti-malli.

2. Kommentointi *Ohjaimet/ToDoListController*-koodia `NewDataAPIClient` menetelmä, joka lisää tunnuksen HTTP pyytää authorization-otsikko.

        client.HttpClient.DefaultRequestHeaders.Authorization =
            new AuthenticationHeaderValue("Bearer", ServicePrincipal.GetS2SAccessTokenForProdMSA().AccessToken);

3. Ota käyttöön ToDoListAPI-projekti. (Projektin hiiren kakkospainikkeella ja valitse sitten **Julkaise > Julkaise**.)

    Visual Studio ottaa käyttöön projektin ja avaa web-sovelluksen Perusosoitteen selain. Tämä toiminto näyttää 403-virhesivu, jolla on tavallista, siirry verkko-Ohjelmointirajapinnan Perusosoitteen selaimesta yritettiin.

4. Sulje selain.

### <a name="get-azure-ad-configuration-values"></a>Azure AD-määritysten arvot

11. Siirry [Azure perinteinen portal](https://manage.windowsazure.com/) **Azure Active Directory**.

12. **Hakemisto** -välilehdessä AAD alihallintaan.

14. Valitse **sovellukset > sovellukset yritykseni omistaa**, ja valitse sitten valintamerkkiä.

15. Valitse sovellusten-luettelosta haluamasi vaihtoehto Azure luodaan todennus ToDoListDataAPI (tietojen taso) API-sovelluksen käytössä nimi.

16. Valitse **määritys** -välilehti.

5. Kopioi **Asiakastunnus** -arvo ja tallenna se suunnilleen voit avata sen myöhemmin. 

8. Azure perinteinen portaalissa palaa **yritykseni omistaa sovellusten**luettelosta ja valitse AAD-sovellus, jonka loit Keskitaso ToDoListAPI API-sovelluksen (yksi, jonka loit edellisessä opetusohjelmassa, ei siihen luomasi Tässä opetusohjelmassa).

16. Valitse **määritys** -välilehti.

5. Kopioi **Asiakastunnus** -arvo ja tallenna se suunnilleen voit avata sen myöhemmin.

6. Valitse **näppäimet**- **1 vuosi** **Valitse kesto** avattavasta luettelosta.

6. Valitse **Tallenna**.

    ![Luo sovelluksen avain](./media/app-service-api-dotnet-service-principal-auth/genkey.png)

7. Kopioi avainarvon ja tallenna se suunnilleen voit avata sen myöhemmin.

    ![Kopioi uusi sovellus-avain](./media/app-service-api-dotnet-service-principal-auth/genkeycopy.png)

### <a name="configure-azure-ad-settings-in-the-middle-tier-api-apps-runtime-environment"></a>Keskitason API-sovelluksen runtime ympäristön Azure AD-asetusten määrittäminen

1. Siirry [Azure portal](https://portal.azure.com/)ja siirry sitten API-sovellukseen, joka isännöi TodoListAPI (Keskitaso) projektin **API sovellus** -sivu.

2. Valitse **Asetukset > sovellusasetukset**.

3. Valitse **sovelluksen asetukset** -kohdassa Lisää seuraavat avaimet ja arvot:

  	| **Avain** | HVT: myöntäjä |
  	|---|---|
  	| **Arvo** | https://Login.microsoftonline.com/ {Azure AD vuokraajan nimesi} |
  	| **Esimerkki** | https://Login.microsoftonline.com/contoso.onmicrosoft.com |

  	| **Avain** | HVT: ClientId |
  	|---|---|
  	| **Arvo** | Ostajantunnus puheluja sovelluksen (Keskitaso - ToDoListAPI) |
  	| **Esimerkki** | 960adec2-b74a-484a-960adec2-b74a-484a |

  	| **Avain** | HVT: ClientSecret |
  	|---|---|
  	| **Arvo** | Sovelluksen avain puheluja sovelluksen (Keskitaso - ToDoListAPI) |
  	| **Esimerkki** | e65e8fc9-5f6b-48e8-e65e8fc9-5f6b-48e8 |

  	| **Avain** | HVT: resurssi |
  	|---|---|
  	| **Arvo** | Ostajantunnus kutsuttu sovelluksen (tietojen taso - ToDoListDataAPI) |
  	| **Esimerkki** | e65e8fc9-5f6b-48e8-e65e8fc9-5f6b-48e8 |

    **Huomautus**: N `ida:Resource`, varmista, että käytät kutsuttu sovelluksen **Asiakastunnus** ja ei sen **Sovelluksen tunnus URI**.

    `ida:ClientId`ja `ida:Resource` ovat eri arvoja Tässä opetusohjelmassa, koska käytössä erota Azure AD-applicaations Keskitaso ja tietojen taso. Jos käytössäsi on yhden Azure AD-sovelluksen puheluja API-sovellus ja suojatun API-sovellus on saman arvon käyttää sekä `ida:ClientId` ja `ida:Resource`.

    Koodi käyttää ConfigurationManager saat nämä arvot, jotta ne voidaan tallentaa Web.config projektitiedoston tai Azure runtime-ympäristössä. ASP.NET-sovelluksen ollessa käynnissä Azure-sovelluksen palvelun ympäristöasetuksia ohittaa Web.config asetukset. Ympäristöasetukset ovat yleensä [tehokkaampi tapa tallentaa luottamuksellisia tietoja verrattuna Web.config-tiedosto](http://www.asp.net/identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure).

6. Valitse **Tallenna**.

    ![Valitse Tallenna](./media/app-service-api-dotnet-service-principal-auth/appsettings.png)

### <a name="test-the-application"></a>Sovelluksen testaaminen

1. Siirry selaimella AngularJS edusta-web-sovelluksen HTTPS URL-osoite.

2. Valitse **Tehtävät-luettelo** -välilehti ja kirjaudu sisään Azure AD-vuokraajan käyttäjän tunnistetiedot. 

4. Lisää Seurattavien kohteiden ja tarkista, että sovellus toimii.

    ![Sivun luettelo](./media/app-service-api-dotnet-service-principal-auth/mvchome.png)

    Jos sovellus ei toimi odotetulla tavalla, Tarkista kaikki kirjoittamasi Azure-portaalissa asetukset. Jos kaikki asetukset eivät näy oikein, jäljempänä tässä opetusohjelmassa [vianmääritys](#troubleshooting) -osan kohdassa.

## <a name="protect-the-api-app-from-browser-access"></a>Suojaa API-sovelluksen selaimen Accessista

Tässä opetusohjelmassa, olet luonut erillinen Azure AD-sovelluksen ToDoListDataAPI (tietojen taso) API-sovellukseen. Kun nähnyt, kun sovellus palvelun Luo AAD-sovelluksen, se määrittää AAD-sovelluksen siten, että käyttäjä voi siirtyä API-sovelluksen URL-Osoitteeseen selaimessa ja kirjaudu sisään. Tämä tarkoittaa on mahdollista Azure AD vuokraajan, ei pelkästään palvelun pääasiallista, voit käyttää Ohjelmointirajapinnan käyttäjälle. 

Jos haluat estää selaimen kirjoittamatta koodia suojatun API-sovelluksessa, voit muuttaa **Vastaa URL-Osoitteen** AAD sovelluksessa niin, että se ei ole sama kuin API-sovelluksen Perusosoitteen. 

### <a name="disable-browser-access"></a>Poista selaimen-tila käytöstä

1. Perinteinen portal, joka on luotu TodoListService AAD-sovelluksen **määrittäminen** -välilehti muuta **Vastaa URL-osoite** -kentässä oleva arvo niin, että se on kelvollinen URL-osoite, mutta ei API-sovelluksen URL.
 
2. Valitse **Tallenna**.

### <a name="verify-browser-access-no-longer-works"></a>Tarkista selaimessa access ei enää toimi

Aiemmin vahvistaa, että voit siirtyä API sovelluksen URL-osoite selaimesta kirjautumalla yksittäisen käyttäjän tunnistetiedot. Tässä osassa olet varmistanut, että tämä ei enää ole mahdollista. 

1. Uudessa selainikkunassa Siirry URL-osoite, API-sovellus uudelleen.

2. Kirjaudu sisään pyydettäessä tekemään niin.

3. Kirjautuminen onnistuu, mutta virhesivun liidit.

    Olet määrittänyt AAD sovelluksen niin, että AAD-alihallinnan käyttäjien ei voi kirjautua sisään ja käyttämään Ohjelmointirajapinnan käyttäminen selaimella. Voit silti käyttää API-sovelluksen avulla palvelun pääasiallista tunnusta, jossa voit tarkistaa siirtymällä web Appin URL-osoite ja lisää Seurattavien kohteiden lisääminen.

## <a name="restrict-access-to-a-particular-service-principal"></a>Käytön rajoittaminen tietyn palvelun lyhennyksen.  

Tällä hetkellä soittajan, joka saa tunnusta käyttäjän tai palvelun lyhennys Azure AD-vuokraajan soittaa TodoListDataAPI (tietojen taso) API-sovellus. Haluat ehkä varmistaaksesi, että tietojen tason API-sovelluksen hyväksyy vain puhelut TodoListAPI (Keskitaso) API-sovelluksen ja vain tietyn palvelun lyhennyksen. 

Voit lisätä näiden rajoitusten lisäämällä koodi tarkistaa `appid` ja `objectidentifier` asemassa saapuvat puhelut.

Tässä opetusohjelmassa, sijoittaa koodin, joka vahvistaa Sovellustunnus ja palvelun pääasiallista tunnus suoraan-ohjain toiminnot.  Vaihtoehtoja on käytettävä mukautettu `Authorize` määrite tai voit tehdä tämän vahvistus käynnistyksen sequences (kuten OWIN middleware). Tämä on esimerkki on [tämän sovelluksen malli](https://github.com/mohitsriv/EasyAuthMultiTierSample/blob/master/MyDashDataAPI/Startup.cs). 

Tee seuraavat muutokset TodoListDataAPI projektin.

2. Avaa *Controllers/TodoListController.cs* -tiedosto.

3. Kommentointi rivit, jotka määrittäminen `trustedCallerClientId` ja `trustedCallerServicePrincipalId`.

        private static string trustedCallerClientId = ConfigurationManager.AppSettings["todo:TrustedCallerClientId"];
        private static string trustedCallerServicePrincipalId = ConfigurationManager.AppSettings["todo:TrustedCallerServicePrincipalId"];

4. Kommentointi koodi on CheckCallerId-menetelmää. Tätä menetelmää kutsutaan jokaisen toiminnon menetelmä ohjaimen alkuun. 

        private static void CheckCallerId()
        {
            string currentCallerClientId = ClaimsPrincipal.Current.FindFirst("appid").Value;
            string currentCallerServicePrincipalId = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
            if (currentCallerClientId != trustedCallerClientId || currentCallerServicePrincipalId != trustedCallerServicePrincipalId)
            {
                throw new HttpResponseException(new HttpResponseMessage { StatusCode = HttpStatusCode.Unauthorized, ReasonPhrase = "The appID or service principal ID is not the expected value." });
            }
        }

5. Ota ToDoListDataAPI projektin Azure-sovelluksen palveluun uudelleen.

6. Selaimessa Siirry AngularJS edusta web Appin HTTPS URL-osoite ja valitse aloitus-sivun **Tehtävät-luettelo** -välilehti.

    Sovellus ei toimi, koska taustatietokantaan puhelut epäonnistuvat. Uusi koodi tarkistaa todellinen sovelluksen tunnus ja objectidentifier, mutta siinä ei ole vielä niitä vastaan oikeat arvot. Selaimen Developer Työkalut konsolin ilmoittaa, että palvelin palauttaa HTTP 401-virheen.

    ![Virhe Developer Työkalut konsoli](./media/app-service-api-dotnet-service-principal-auth/webapperror.png)

    Seuraavat toimet voit määrittää odotetut arvot.

8. Palvelun lyhennys arvon hakeminen Azure AD-sovellus, jonka loit TodoListWebApp projektin Azure AD-PowerShellin avulla.

    a. Ohjeita siitä, miten voit asentaa PowerShellin Azure ja tilauksen yhdistäminen on artikkelissa [Azure PowerShellin Azure resurssien hallinta](../powershell-azure-resource-manager.md).

    b. Saat palvelun ansaitun luettelo-Suorita `Login-AzureRmAccount` komento ja valitse sitten `Get-AzureRmADServicePrincipal` komento.

    c-näppäinyhdistelmää. Etsi objektitunnus TodoListAPI sovelluksen palvelun lyhennys ja tallenna se sijainnissa, voit kopioida myöhemmin.

7. Siirry Azure-portaalissa API-sovellusta, joka ToDoListDataAPI projektin käyttöön API-sovelluksen sivu.

9. Valitse **Asetukset > sovellusasetukset**.

3. Valitse **sovelluksen asetukset** -kohdassa Lisää seuraavat avaimet ja arvot:

  	| **Avain** | TODO:TrustedCallerServicePrincipalId |
  	|---|---|
  	| **Arvo** | Palvelun kutsumista sovelluksen pääasiallista tunnus |
  	| **Esimerkki** | 4f4a94a4-6f0d-4072-4f4a94a4-6f0d-4072 |

  	| **Avain** | TODO:TrustedCallerClientId |
  	|---|---|
  	| **Arvo** | Ostajantunnus joka sovelluksen - kopioi TodoListAPI Azure AD-sovelluksesta |
  	| **Esimerkki** | 960adec2-b74a-484a-960adec2-b74a-484a |

6. Valitse **Tallenna**.

    ![Valitse Tallenna](./media/app-service-api-dotnet-service-principal-auth/trustedcaller.png)

6. Selaimessa palaa web Appin URL-osoite ja valitse aloitus-sivun **Tehtävät-luettelo** -välilehti.

    Tällä hetkellä sovellus toimii odotetulla tavalla, koska luotettujen soittajan sovelluksen tunnuksen ja palvelun pääasiallista tunnus odotetut arvot.

    ![Sivun luettelo](./media/app-service-api-dotnet-service-principal-auth/mvchome.png)

## <a name="building-the-projects-from-scratch"></a>Luominen alusta alkaen projektit

Koontiprojekti verkko-Ohjelmointirajapinnan on luotu käyttämällä **Azure API App** project-malli ja oletusarvoisen arvot ohjauskoneen tilalle ToDoList ohjauskoneen. Hankkiminen Azure AD palvelun pääasiallista tunnusten ToDoListAPI Projectissa, [Active Directory käyttöoikeuksien kirjaston (ADAL), .NET](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) NuGet paketti on asennettuna.
 
Lisätietoja taustatietokannaksi verkko-Ohjelmointirajapinnan kuten ToDoListAngular AngularJS yhden sovelluksen luomisesta on [kädet-testiympäristössä: Muodosta yhden sivun sovelluksen (SPA) ASP.NET-verkko-Ohjelmointirajapinnan ja Angular.js](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs). Lisätietoja Azure AD-todennus-koodin lisääminen kohdassa [suojaaminen AngularJS yksittäisen sivun Azure AD kanssa](../active-directory/active-directory-devquickstarts-angular.md).

## <a name="troubleshooting"></a>Vianmääritys

[AZURE.INCLUDE [troubleshooting](../../includes/app-service-api-auth-troubleshooting.md)]

* Varmista, että älä sekoita ToDoListAPI (Keskitaso) ja ToDoListDataAPI (tietojen taso). Esimerkiksi Tässä opetusohjelmassa lisäät todennus tietojen tason API-sovelluksen, **mutta app avain sijaittava Azure AD-sovellus, jonka loit Keskitaso API-sovellukseen**.

## <a name="next-steps"></a>Seuraavat vaiheet

Tämä on API sovellusten sarjan viimeinen opetusohjelman. 

Saat lisätietoja Azure Active Directory on seuraavissa resursseissa.

* [Azure AD kehittäjien opas](http://aka.ms/aaddev)
* [Azure AD-skenaariot](http://aka.ms/aadscenarios)
* [Azure AD-objektit](http://aka.ms/aadsamples)

    Mitä näytetään tässä opetusohjelmassa, mutta App todentaminen käyttämättä muistuttaa [Web App-sovelluksen WebAPI-OAuth2-AppIdentity-DotNet](http://github.com/AzureADSamples/WebApp-WebAPI-OAuth2-AppIdentity-DotNet) -malli.

Tutustu muihin tapoihin käyttöönotto Visual Studio projektien API sovellukset käyttämällä Visual Studio tai [automatisointi käyttöönoton](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery) [lähteen järjestelmä](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control) [Azure App Service-sovelluksen käyttöönotto](../app-service-web/web-sites-deploy.md).

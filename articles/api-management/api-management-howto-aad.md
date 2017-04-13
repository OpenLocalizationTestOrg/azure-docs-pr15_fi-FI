<properties 
    pageTitle="Voit sallia developer tilit Azure Active Directoryn avulla Azure API hallinta" 
    description="Opettele sallivat käyttäjien Azure Active Directoryn avulla API hallinta." 
    services="api-management" 
    documentationCenter="API Management" 
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

# <a name="how-to-authorize-developer-accounts-using-azure-active-directory-in-azure-api-management"></a>Voit sallia developer tilit Azure Active Directoryn avulla Azure API hallinta


## <a name="overview"></a>Yleiskatsaus
Tämän oppaan avulla voit developer-portaalissa käyttöönottaminen vähintään yksi Azure Active kansioissa kaikille käyttäjille. Tämän oppaan myös avulla voit hallita ulkoisten käyttäjien Azure Active Directory sisältävien ryhmien lisäämällä Azure Active Directory-käyttäjäryhmille.

>Jotta voit suorittaa tämän oppaan vaiheet on ensin Azure Active Directory, johon-sovelluksen luominen.

## <a name="how-to-authorize-developer-accounts-using-azure-active-directory"></a>Voit sallia developer tilit Azure Active Directoryn avulla

Aloita valitsemalla Azure perinteinen portaalissa API Management-palvelun **hallinta** . Tämä avaa API hallinta publisher-portaalin.

![Publisher-portaalissa][api-management-management-console]

>Jos et ole luonut erillisen service API hallinta-kohdassa [Azure API hallinnan käytön aloittaminen][] -opetusohjelma [Luo erillisen service API hallinta][] .

Vasemmalla **API hallinta** -valikosta **Suojaus** ja valitse sitten **Ulkoisten käyttäjätietojen**.

![Ulkoinen käyttäjätietoja][api-management-security-external-identities]

Valitse **Azure Active Directory**. Pane merkille **Uudelleenohjata URL-Osoitteen** ja vaihda ja Azure Active Directory perinteinen Azure-portaalissa.

![Ulkoinen käyttäjätietoja][api-management-security-aad-new]

Luo uusi Azure Active Directory-sovellus valitsemalla **Lisää** ja valitse **Lisää sovellus kehittää oman organisaation ulkopuolelta**.

![Lisää uusi Azure Active Directory-sovellus][api-management-new-aad-application-menu]

Sovelluksen nimi, valitse **Web-sovelluksen ja/tai verkko-Ohjelmointirajapinnan**ja valitse Seuraava.

![Uusi Azure Active Directory-sovellus][api-management-new-aad-application-1]

**Sign-on URL**Kirjoita Sign URL-osoite developer-portaalissa. Tässä esimerkissä **Sign URL** on `https://aad03.portal.current.int-azure-api.net/signin`. 

**Sovelluksen tunnus URL-osoite**Kirjoita oletustoimialueen tai mukautetun toimialueen Azure Active Directory ja liittää yksilöllinen merkkijono. Tässä esimerkissä **https://contoso5api.onmicrosoft.com** oletustoimialueen käyttämisestä **/api** määritetty liitteen.

![Azure Active Directory-sovelluksen uudet ominaisuudet][api-management-new-aad-application-2]

Tallenna ja luo uusi sovellus ja määritä uuden sovelluksen **määrittäminen** -välilehteen valitsemalla valintaruutu.

![Luo uusi Azure Active Directory-sovellus][api-management-new-aad-app-created]

Jos useita Azure Active kansioita on käytettävä tämän sovelluksen, valitse **Kyllä** , **sovellus on usean vuokraajan**. Oletusarvo on **ei**.

![Sovellus on usean vuokraajan][api-management-aad-app-multi-tenant]

Kopioi **URL-Osoitteen uudelleenohjaaminen** publisher-portaalissa **Ulkoisen jäsenyydet** -välilehti **Azure Active Directory** -osa ja liitä se **Vastaa URL** -tekstiruutuun. 

![Vastaa URL-osoite][api-management-aad-reply-url]

Vieritä näytön alareunaan määrittäminen-välilehden, valitse **Sovelluksen käyttöoikeudet** avattavasta ja **directory tietojen**.

![Sovelluksen käyttöoikeudet][api-management-aad-app-permissions]

Valitse **Edustajan käyttöoikeuksien** avattavasta ja tarkista **Sign ottaminen käyttöön ja lukea käyttäjien-profiileista**.

![Valtuutetun käyttöoikeudet][api-management-aad-delegated-permissions]

>Saat lisätietoja sovelluksen ja valtuutetun käyttöoikeudet-kohdassa [kaavion Ohjelmointirajapinnan käyttäminen][].

**OstajanTunnus** Kopioi Leikepöydälle.

![Asiakastunnus][api-management-aad-app-client-id]

Palaa publisher-portaalin ja Liitä kopioitu Azure Active Directory-sovelluksen määritys **Asiakastunnus** .

![Asiakastunnus][api-management-client-id]

Palaa Azure Active Directory-määritys ja valitse **Valitse kesto** avattavasta **näppäimet** -kohdan ja määritä aikaväli. Tässä esimerkissä käytetään **1 vuosi** .

![Avain][api-management-aad-key-before-save]

Valitse **Tallenna** Tallenna kokoonpano ja Näytä sitten-näppäintä. Kopioi Leikepöydälle avain.

>Pane merkille avain. Kun suljet ikkunan Azure Active Directory-määritykset, avainta ei voi näyttää uudelleen.

![Avain][api-management-aad-key-after-save]

Siirry takaisin publisher-portaalin ja liitä avain **Asiakkaan salaisuus** -tekstiruutuun.

![Asiakkaan salaisuus][api-management-client-secret]

**Sallittu Alihallinnat** määrittää, mitkä kansiot on käyttöoikeudet API Management-palvelun esiintymän API. Määritä sellaisten Azure Active Directory-esiintymät, jolle haluat myöntää käyttöoikeuden. Voit erotella useita toimialueita newlines, välilyöntejä tai pilkkuja.

![Sallitun alihallinnat][api-management-client-allowed-tenants]

Useita toimialueita voidaan määrittää **Sallittu Alihallinnat** -osassa. Ennen kuka tahansa käyttäjä, voit kirjautua toisen toimialueen kuin alkuperäinen toimialue, jossa sovellus on rekisteröity, eri toimialueen yleisellä järjestelmänvalvojalla on myönnettävä access-kansion tiedot sovelluksen käyttöoikeudet. Myönnä käyttöoikeudet yleisellä järjestelmänvalvojalla on sovelluksen kirjautuminen ja sitten **Hyväksy**. Seuraavassa esimerkissä `miaoaad.onmicrosoft.com` on lisätty **Sallittu asiakasympäristöjen** ja yleisenä, toimialueen järjestelmänvalvoja on kirjautuminen ensimmäistä kertaa.

![Käyttöoikeudet][api-management-permissions-form]

>Jos-yleinen järjestelmänvalvoja yrittää kirjautua sisään, ennen kuin Yleinen järjestelmänvalvoja on myönnetty tarvittavat käyttöoikeudet, Kirjautumisyritys epäonnistuu ja -virhe tulee näkyviin.

Kun uudet määritykset on määritetty, valitse **Tallenna**.

![Tallenna][api-management-client-allowed-tenants-save]

Kun muutokset tallentuvat, määritetyn Azure Active Directory-käyttäjät voivat kirjautua Kehitystyökalut-portaaliin, noudattamalla seuraavia ohjeita [Developer-portaalissa Azure Active Directory-tilillä kirjautuminen][].

## <a name="how-to-add-an-external-azure-active-directory-group"></a>Voit lisätä ulkoisen Azure Active Directory-ryhmän

Kun olet ottanut käyttöön käyttäjien Azure Active Directory-, voit lisätä Azure Active Directory-ryhmä Ohjelmointirajapinta hallinta voit hallita helpommin liitoksen kehittäjien-ryhmässä haluamasi tuotteet.

> Jotta voit määrittää ulkoisen Azure Active Directory-ryhmän Azure Active Directory on ensin määritettävä jäsenyydet-välilehti edellisen osan ohjeiden mukaan. 

Ulkoinen Azure Active Directory-ryhmä on lisätty, jonka haluat myöntää käyttöoikeuksia ryhmän tuotteen **näkyvyys** -välilehdestä. Valitse **tuotteet**ja valitse sitten haluamasi tuotteen nimi.

![Määritä tuote][api-management-configure-product]

**Näkyvyys** -välilehteen ja valitse **Lisää ryhmiä Azure Active Directorysta**.

![Lisää-ryhmä][api-management-add-groups]

Avattavasta luettelosta **Azure Active Directory-vuokraajan** ja kirjoita **ryhmät** lisätään tekstiruutuun haluamasi ryhmän nimi.

![Valitse ryhmä][api-management-select-group]

Tämän ryhmänimi löytyy **ryhmät** -luettelossa oman Azure Active Directory, kuten seuraavassa esimerkissä.

![Azure Active Directory-ryhmien luettelo][api-management-aad-groups-list]

Valitse **Lisää** Vahvista ryhmänimi ja Lisää ryhmä. Tässä esimerkissä **Contoso 5 kehittäjät** ulkoisen ryhmä lisätään. 

![Ryhmään lisätty][api-management-aad-group-added]

Valitse **Tallenna** Tallenna uusi ryhmä.

Kun Azure Azure Active Directory-ryhmän on määritetty yksi tuote, se on käytettävissä muita tuotteita API Management-palvelun esiintymän **näkyvyys** -välilehden tarkistettava.

Tarkistaa ja määrittää ulkoisen ryhmien ominaisuuksia, kun ne on lisätty, valitse **ryhmät** -välilehdestä ryhmän nimi.

![Ryhmien hallinta][api-management-groups]

Täällä voit muokata **nimeä** ja ryhmän **kuvaus** .

![Muokkaa ryhmää][api-management-edit-group]

Määritetty Azure Active Directory-käyttäjien voit kirjautua Developer-portaalissa ja Näytä ja ryhmistä, joihin heillä on näkyvyys noudattamalla seuraavassa osassa tilaa.

## <a name="how-to-log-in-to-the-developer-portal-using-an-azure-active-directory-account"></a>Kuinka kirjaudun Azure Active Directory-tilin avulla Developer-portaalissa

Kirjautumiseen määritetty edellisten kohtien Azure Active Directory-tilin avulla Developer-portaalissa, Avaa uudessa selainikkunassa, käyttämällä Active Directory-sovelluksen määritys- **Sign URL** ja sitten **Azure Active Directory**.

![Developer-portaalissa][api-management-dev-portal-signin]

Kirjoita toisen käyttäjän tunnistetiedot Azure Active Directoryssa, ja valitse **Kirjaudu sisään**.

![Kirjaudu sisään][api-management-aad-signin]

Sinua kehotetaan rekisteröintilomake kanssa, jos lisätiedot tarvitaan. Täytä rekisteröintilomake ja valitse **tilaa**.

![Rekisteröinti][api-management-complete-registration]

Käyttäjän on nyt kirjautunut API Management-palvelun esiintymän developer-portaalissa.

![Rekisteröityminen on valmis][api-management-registration-complete]



[api-management-management-console]: ./media/api-management-howto-aad/api-management-management-console.png
[api-management-security-external-identities]: ./media/api-management-howto-aad/api-management-security-external-identities.png
[api-management-security-aad-new]: ./media/api-management-howto-aad/api-management-security-aad-new.png
[api-management-new-aad-application-menu]: ./media/api-management-howto-aad/api-management-new-aad-application-menu.png
[api-management-new-aad-application-1]: ./media/api-management-howto-aad/api-management-new-aad-application-1.png
[api-management-new-aad-application-2]: ./media/api-management-howto-aad/api-management-new-aad-application-2.png
[api-management-new-aad-app-created]: ./media/api-management-howto-aad/api-management-new-aad-app-created.png
[api-management-aad-app-permissions]: ./media/api-management-howto-aad/api-management-aad-app-permissions.png
[api-management-aad-app-client-id]: ./media/api-management-howto-aad/api-management-aad-app-client-id.png
[api-management-client-id]: ./media/api-management-howto-aad/api-management-client-id.png
[api-management-aad-key-before-save]: ./media/api-management-howto-aad/api-management-aad-key-before-save.png
[api-management-aad-key-after-save]: ./media/api-management-howto-aad/api-management-aad-key-after-save.png
[api-management-client-secret]: ./media/api-management-howto-aad/api-management-client-secret.png
[api-management-client-allowed-tenants]: ./media/api-management-howto-aad/api-management-client-allowed-tenants.png
[api-management-client-allowed-tenants-save]: ./media/api-management-howto-aad/api-management-client-allowed-tenants-save.png
[api-management-aad-delegated-permissions]: ./media/api-management-howto-aad/api-management-aad-delegated-permissions.png
[api-management-dev-portal-signin]: ./media/api-management-howto-aad/api-management-dev-portal-signin.png
[api-management-aad-signin]: ./media/api-management-howto-aad/api-management-aad-signin.png
[api-management-complete-registration]: ./media/api-management-howto-aad/api-management-complete-registration.png
[api-management-registration-complete]: ./media/api-management-howto-aad/api-management-registration-complete.png
[api-management-aad-app-multi-tenant]: ./media/api-management-howto-aad/api-management-aad-app-multi-tenant.png
[api-management-aad-reply-url]: ./media/api-management-howto-aad/api-management-aad-reply-url.png
[api-management-permissions-form]: ./media/api-management-howto-aad/api-management-permissions-form.png
[api-management-configure-product]: ./media/api-management-howto-aad/api-management-configure-product.png
[api-management-add-groups]: ./media/api-management-howto-aad/api-management-add-groups.png
[api-management-select-group]: ./media/api-management-howto-aad/api-management-select-group.png
[api-management-aad-groups-list]: ./media/api-management-howto-aad/api-management-aad-groups-list.png
[api-management-aad-group-added]: ./media/api-management-howto-aad/api-management-aad-group-added.png
[api-management-groups]: ./media/api-management-howto-aad/api-management-groups.png
[api-management-edit-group]: ./media/api-management-howto-aad/api-management-edit-group.png

[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Azure API hallinnan käytön aloittaminen]: api-management-get-started.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Luo erillisen service API hallinta]: api-management-get-started.md#create-service-instance

[http://oauth.net/2/]: http://oauth.net/2/
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[Kaavio Ohjelmointirajapinnan käyttäminen]: http://msdn.microsoft.com/library/azure/dn132599.aspx#BKMK_Graph

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps

[Kirjaudu sisään ja Azure Active Directory-tilin avulla Developer-portaalissa]: #Log-in-to-the-Developer-portal-using-an-Azure-Active-Directory-account


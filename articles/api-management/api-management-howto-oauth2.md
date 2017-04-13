<properties 
    pageTitle="Voit sallia developer tilille OAuth 2.0 Azure API hallinta" 
    description="Opettele sallivat käyttäjien OAuth 2.0 käyttäminen API hallinta." 
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

# <a name="how-to-authorize-developer-accounts-using-oauth-20-in-azure-api-management"></a>Voit sallia developer tilille OAuth 2.0 Azure API hallinta

Monta ohjelmointirajapinnan tukevat [OAuth 2.0](http://oauth.net/2/) suojatun Ohjelmointirajapinnan ja varmista, että vain kelvollisia käyttäjät voivat käyttää, ja he voivat avata vain resursseja, joihin heillä on oikeus. Jotta voit käyttää Azure API hallinta vuorovaikutteinen Kehitystyökalut-konsolin tällaisten ohjelmointirajapinnan kanssa, palvelun avulla voit määrittää palvelun-esiintymä toimii oman OAuth 2.0 käytössä API.

## <a name="prerequisites"> </a>Edellytykset

Tämän oppaan avulla voit määrittää API Management service-esiintymän OAuth 2.0-todennus käyttämään Kehitystyökalut-tilien, mutta ei välttämättä näy sinua OAuth 2.0-palvelun määrittäminen. Kunkin OAuth 2.0-palvelun kokoonpano on erilainen, vaikka vaiheet ovat samanlaiset ja tietoja, joita käytetään määritettäessä OAuth 2.0 API Management-palvelun esiintymän tarvittavat laitteita ovat samat. Tässä ohjeaiheessa esitellään esimerkkejä käyttäen Azure Active Directory OAuth 2.0-palvelun.

>[AZURE.NOTE] Lisätietoja määrittäminen OAuth 2.0 Azure Active Directoryn avulla on artikkelissa [Web App-sovelluksen GraphAPI-DotNet][] -malli.

## <a name="step1"> </a>OAuth 2.0-todennus-palvelimen määrittäminen API hallinta

Aloita valitsemalla Azure perinteinen portaalissa API Management-palvelun **hallinta** . Tämä avaa API hallinta publisher-portaalin.

![Publisher-portaalissa][api-management-management-console]

>[AZURE.NOTE] Jos et ole luonut erillisen service API hallinta-kohdassa [Azure API hallinnan käytön aloittaminen][] -opetusohjelma [Luo erillisen service API hallinta][] .

Valitsemalla vasemmalla **API hallinta** -valikosta **Suojaus** ja valitse **OAuth 2.0** **Lisää luvan palvelin**.

![OAuth 2.0][api-management-oauth2]

Jälkeen valitsemalla **Lisää luvan palvelin**, uusi palvelimen valtuutuslomake tulee näkyviin.

![Uusi palvelin][api-management-oauth2-server-1]

Kirjoita **nimi** ja **kuvaus** -kenttiin nimi ja halutessasi kuvaus. 

>[AZURE.NOTE] Näitä kenttiä käytetään tunnistavan sisällä nykyisen API Management-palvelun esiintymän OAuth 2.0 todennus-palvelimessa ja niiden arvot eivät ole peräisin OAuth 2.0-palvelimessa.

Kirjoita **asiakkaan rekisteröinti kirjautumissivun URL-osoite**. Tämä sivu on kohtaa, johon käyttäjät voivat luoda ja hallita niiden tilit ja vaihtelee sen mukaan, käytetyn OAuth 2.0-palvelun. **Asiakkaan rekisteröinti kirjautumissivun URL-osoite** osoittaa sivu, jossa käyttäjät voivat käyttää voit luoda ja määrittää omia tilit OAuth 2.0-palvelut, jotka tukevat Käyttäjähallinta tilien osalta. Joissakin organisaatioissa Älä määrittäminen tai käyttää tätä toimintoa, vaikka OAuth 2.0-tarjoaja tukee sitä. Jos OAuth 2.0-palveluntarjoajan ei ole määritetty tilien Käyttäjähallinta, kirjoita paikkamerkin URL-osoite esimerkiksi yrityksen URL-Osoitteen tai URL-osoite esimerkiksi `https://placeholder.contoso.com`.

Seuraavassa osassa lomake sisältää **luvan koodin myöntää tyypit**, **luvan päätepisteen URL-osoite**ja **pyynnön todennusmenetelmän** asetukset.

![Uusi palvelin][api-management-oauth2-server-2]

Määritä **luvan koodin myöntää tyypit** valitsemalla haluamasi tyyppi. **Todennus-koodi** on määritetty oletusarvoisesti.

Kirjoita **luvan päätepisteen URL-osoite**. Azure Active Directory-tätä URL-osoite on samankaltainen kuin seuraava URL-osoite, johon `<client_id>` on korvattu Asiakastunnus, joka määrittää sovelluksen OAuth 2.0-palvelimeen.

    https://login.windows.net/<client_id>/oauth2/authorize

**Pyyntö todennusmenetelmän** määrittää, miten todennus-pyyntö lähetetään OAuth 2.0-palvelimessa. **HAE** on valittuna oletusarvoisesti.

Seuraavassa osassa on, jossa määritetty **tunnuksen päätepisteen URL-osoite**, **asiakkaan todennusmenetelmien** **käyttöoikeustietue lähettäminen menetelmä**tai **Oletus laajuus** .

![Uusi palvelin][api-management-oauth2-server-3]

Azure Active Directory OAuth 2.0-palvelin- **tunnussanoma päätepisteen URL-osoite** on muotoa, johon `<APPID>` muoto on `yourapp.onmicrosoft.com`.

    https://login.windows.net/<APPID>/oauth2/token

**Asiakkaan** todennusmenetelmien oletusarvo on **perustietoja**ja **lähettäminen menetelmä käyttöoikeustietue** on **Authorization-otsikko**. Nämä arvot määritetään lomakkeen sekä **Oletus alueen**sisältö.

**Asiakkaan käyttäjätiedot** -osio sisältää **Asiakastunnus** ja **asiakkaan salainen**, joka on saatu OAuth 2.0-palvelimessa luomista ja määritysten aikana. Kun **Asiakastunnus** ja **asiakkaan salaisen** on määritetty, **redirect_uri** **luvan koodi** luodaan. Tämä URI käytetään OAuth 2.0-palvelinkokoonpanon vastaa URL-Osoitteen määrittäminen.

![Uusi palvelin][api-management-oauth2-server-4]

Jos **luvan koodin myöntää tyypit** on määritetty **resurssien omistajasalasana**, **resurssien omistajan tunnistetietoja** -osa käytetään tunnistetiedot; Muussa tapauksessa voit jättää tyhjäksi.

![Uusi palvelin][api-management-oauth2-server-5]

Kun lomake on valmis, Tallenna valitsemalla **Tallenna** API Management OAuth 2.0 luvan server-määritys. Kun server-määritys on tallennettu, voit määrittää ohjelmointirajapinnan käyttäminen tässä määrityksessä seuraavan osion esitetyllä tavalla.

## <a name="step2"> </a>Ohjelmointirajapinnan käyttäminen OAuth 2.0 käyttäjän käyttöoikeuksien määrittäminen

Valitse **ohjelmointirajapinnan** vasemmalla **API hallinta** -valikosta, haluamasi Ohjelmointirajapinnan nimeä, valitse **Suojaus**ja **OAuth**2.0-valintaruutu.

![Käyttäjän käyttöoikeuksien][api-management-user-authorization]

Valitse avattavasta luettelosta haluamasi **luvan palvelimeen** , ja valitse **Tallenna**.

![Käyttäjän käyttöoikeuksien][api-management-user-authorization-save]

## <a name="step3"> </a>Testata OAuth 2.0 käyttäjän käyttöoikeuksien Developer-portaalissa

Kun olet määritetty OAuth 2.0 luvan palvelimen ja määrittänyt oman Ohjelmointirajapinnan käyttäminen tähän palvelimeen, voit kokeilla sitä Developer-portaalissa siirtymällä ja kutsumista Ohjelmointirajapinnan.  Valitse yläreunan oikealle valikossa **Developer-portaalissa** .

![Developer-portaalissa][api-management-developer-portal-menu]

**API** yläreunan-valikossa ja valitse **Päivitä API**.

![Päivitä Ohjelmointirajapinta][api-management-apis-echo-api]

>[AZURE.NOTE] Jos sinulla on vain yksi API määritetty tai näkyvissä tiliisi, valitsemalla ohjelmointirajapinnan siirryt suoraan kyseisen API toimintoja.

Valitse **HAE resurssi** -toiminnon, valitse **Avoimen konsolin**ja valitse sitten avattavasta luettelosta **luvan koodi** .

![Avaa konsoli][api-management-open-console]

Kun **luvan koodi** on valittuna, ponnahdusikkunan näkyy OAuth 2.0-palvelun kirjautumisen-lomakkeeseen. Tässä esimerkissä kirjautumisen lomakkeen tarjoaa Azure Active Directory.

>[AZURE.NOTE] Jos sinulla on poistettu käytöstä ponnahdusikkunat sinua pyydetään ne käyttöön selaimessa. Kun otat ne käyttöön, valitse **luvan koodi** uudelleen ja kirjaudu sisään-lomakkeen näytetään.

![Kirjaudu sisään][api-management-oauth2-signin]

Kun olet kirjautunut, **pyyntö otsikot** on täytetty `Authorization : Bearer` otsikon, joka sallii pyynnön.

![Pyynnön otsikon tunnus][api-management-request-header-token]

Tässä vaiheessa voit haluamasi arvot jäljellä olevien parametrien määrittäminen ja Lähetä pyyntö. 

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja OAuth 2.0 ja API hallinnan käyttämisestä on artikkelissa seuraavassa videossa ja [artikkelissa](api-management-howto-protect-backend-with-aad.md)mukana.

> [AZURE.VIDEO protecting-web-api-backend-with-azure-active-directory-and-api-management]

[api-management-management-console]: ./media/api-management-howto-oauth2/api-management-management-console.png
[api-management-oauth2]: ./media/api-management-howto-oauth2/api-management-oauth2.png
[api-management-user-authorization]: ./media/api-management-howto-oauth2/api-management-user-authorization.png
[api-management-user-authorization-save]: ./media/api-management-howto-oauth2/api-management-user-authorization-save.png
[api-management-oauth2-signin]: ./media/api-management-howto-oauth2/api-management-oauth2-signin.png
[api-management-request-header-token]: ./media/api-management-howto-oauth2/api-management-request-header-token.png
[api-management-developer-portal-menu]: ./media/api-management-howto-oauth2/api-management-developer-portal-menu.png
[api-management-open-console]: ./media/api-management-howto-oauth2/api-management-open-console.png
[api-management-oauth2-server-1]: ./media/api-management-howto-oauth2/api-management-oauth2-server-1.png
[api-management-oauth2-server-2]: ./media/api-management-howto-oauth2/api-management-oauth2-server-2.png
[api-management-oauth2-server-3]: ./media/api-management-howto-oauth2/api-management-oauth2-server-3.png
[api-management-oauth2-server-4]: ./media/api-management-howto-oauth2/api-management-oauth2-server-4.png
[api-management-oauth2-server-5]: ./media/api-management-howto-oauth2/api-management-oauth2-server-5.png
[api-management-apis-echo-api]: ./media/api-management-howto-oauth2/api-management-apis-echo-api.png


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
[Web App-sovelluksen GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps


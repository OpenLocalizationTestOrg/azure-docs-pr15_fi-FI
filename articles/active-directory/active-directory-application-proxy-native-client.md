<properties
    pageTitle="Alkuperäisen asiakassovellukset välityspalvelimen sovellusten julkaisemisen ottaminen käyttöön | Microsoft Azure"
    description="Kerrotaan, miten voit pitää yhteyttä Azure AD sovelluksen välityspalvelimen yhdistimen paikallisen sovelluksia etäkäyttöä alkuperäisen asiakassovellukset ottaminen käyttöön."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/22/2016"
    ms.author="kgremban"/>

# <a name="how-to-enable-native-client-apps-to-interact-with-proxy-applications"></a>Alkuperäisen asiakassovellukset vuorovaikutuksessa välityspalvelimen sovellusten ottaminen käyttöön

Azure Active Directory-sovelluksen välityspalvelimen käytetään yleisesti julkaista selaimen-sovelluksia, kuten SharePoint-, Outlook Web Access- ja mukautetun viivan yrityssovellusten. Myös se voidaan julkaista native client-sovelluksia, jotka eroavat web Apps-sovelluksista, koska niitä asennetaan laitteeseen. Tämä tapahtuu tukemaan Azure AD annettujen tunnusten, joka on tullut Määritä HTTP vakio-otsikot.

![Loppukäyttäjät, Azure Active Directory ja sovelluksiin välinen suhde](./media/active-directory-application-proxy-native-client/richclientflow.png)

Näiden sovellusten julkaisemiseen suositeltava tapa on käyttää Azure AD todennus kirjastoon, joka kestää hoitamaan todennus näissä ja tukee useita eri asiakas-ympäristössä. Sovelluksen välityspalvelimen sovittaa [Verkko-Ohjelmointirajapinnan skenaarion alkuperäisen sovelluksen](active-directory-authentication-scenarios.md#native-application-to-web-api). Prosessi, jossa avulla voi suorittaa tämä on seuraavanlainen:

## <a name="step-1-publish-your-application"></a>Vaihe 1: Julkaise sovelluksesi

Julkaise sovelluksesi välityspalvelimeen tapaan muut sovellukset, määrittää käyttäjiä ja antaa heille premium tai basic käyttöoikeuksia. Katso lisätietoja [sovelluksen välityspalvelimen Julkaise-sovelluksiin](active-directory-application-proxy-publish.md).

## <a name="step-2-configure-your-application"></a>Vaihe 2: Määritä sovellus

Määritä alkuperäisen sovelluksen seuraavasti:

1. Kirjaudu Azure perinteinen-portaaliin.
2. Valitse vasemmalla olevasta valikosta Active Directory-kuvaketta ja valitse sitten hakemistossa.
3. Valitse yläreunan **sovellukset**. Jos ei ole sovelluksia on lisätty kansioon, tällä sivulla näkyvät vain **Lisää sovellus** -linkki. Napsauttamalla linkkiä tai Vaihtoehtoisesti voit napsauttaa painiketta komentopalkista **Lisää** -painiketta.
4. **Mitä haluat tehdä** sivulla Valitse **Lisää sovellus organisaation kehittää**linkkiä.
5. **Kerro sovelluksesi tietoja** -sivulla sovelluksesi nimi ja valitse **alkuperäisen asiakassovellukseen**. Valitse Jatka napsauttamalla nuolta.
6. **Sovelluksen tiedot** -sivulla antaa **Uudelleenohjata URI** native client-sovellus ja valitse sitten Lopeta valintamerkki.

Sovellus on nyt lisätty ja siirryt Pika-aloitus-sivun sovelluksen.

## <a name="step-3-grant-access-to-other-applications"></a>Vaihe 3: Muiden sovellusten käyttöoikeuksien myöntäminen

Ota käyttöön voi luovuttaa muiden sovellusten hakemistossa alkuperäisen sovelluksen:

1. Ylimmän valikon **sovellukset**ja valitse uusi alkuperäisen sovellus **määrittäminen**.
2. Vieritä **muiden sovellusten käyttöoikeudet** -osaan. Valitse **Lisää sovellus** -painike ja valitse välityspalvelimen sovellus, jonka haluat myöntää oikeuden alkuperäisen sovelluksen ja valitse oikeassa alakulmassa valintamerkki. Valitse uuden käyttöoikeustason **Valtuutetun käyttöoikeudet** -pudotusvalikosta.

![Muut sovellukset - näyttökuva käyttöoikeudet sovelluksen lisääminen](./media/active-directory-application-proxy-native-client/delegate_native_app.png)

## <a name="step-4-edit-the-active-directory-authentication-library"></a>Vaihe 4: Muokkaa Active Directory käyttöoikeuksien kirjasto

Muokkaa alkuperäisen sovelluksen koodin todennus kontekstissa, Active Directory käyttöoikeuksien kirjaston (ADAL), ovat seuraavat:

        // Acquire Access Token from AAD for Proxy Application
        AuthenticationContext authContext = new AuthenticationContext("https://login.microsoftonline.com/<TenantId>");
        AuthenticationResult result = authContext.AcquireToken("< Frontend Url of Proxy App >",
                                                        "< Client Id of the Native app>",
                                                        new Uri("< Redirect Uri of the Native App>"),
                                                        PromptBehavior.Never);

        //Use the Access Token to access the Proxy Application
        HttpClient httpClient = new HttpClient();
        httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
        HttpResponseMessage response = await httpClient.GetAsync("< Proxy App API Url >");

Muuttujat korvataan seuraavasti:

- **TenantId** löytyvät oikealle jälkeen "/ Directory /" GUID-tunnus-sovelluksen **määritys** -sivulla URL-osoite.
- **Frontend URL-osoite** on kirjoitettu välityspalvelimen-sovellus ja muutettavista välityspalvelimen sovelluksen **määritys** -sivulla edusta-URL.
- **OstajanTunnus** alkuperäisen sovelluksen löytyy alkuperäisen-sovelluksen **määrittäminen** -sivulla.
- **Ohjaa URI alkuperäisen sovelluksen** löytyy alkuperäisen-sovelluksen **määrittäminen** -sivulla.

![Uuden alkuperäisen-sovelluksen määrittäminen sivun näyttökuva](./media/active-directory-application-proxy-native-client/new_native_app.png)

Saat lisätietoja alkuperäisen sovelluksen kulun [Verkko-Ohjelmointirajapinnan alkuperäisen sovelluksen](active-directory-authentication-scenarios.md#native-application-to-web-api).


## <a name="see-also"></a>Katso myös

- [Valinnaiseksi oman toimialuenimen käyttäminen](active-directory-application-proxy-custom-domains.md)
- [Ehdollinen käytön käyttöön ottaminen](active-directory-application-proxy-conditional-access.md)
- [Vaateet huomioon sovellusten käyttäminen](active-directory-application-proxy-claims-aware-apps.md)
- [Yksi merkki ottaminen käyttöön](active-directory-application-proxy-sso-using-kcd.md)

Uusimmat uutiset ja päivitykset Tutustu [sovelluksen välityspalvelin-blogi](http://blogs.technet.com/b/applicationproxyblog/)

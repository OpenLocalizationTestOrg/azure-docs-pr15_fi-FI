<properties
    pageTitle="Azure Active Directory tunnistetietojen suojaus- ja Microsoft Graph aloittaminen | Microsoft Azure"
    description="Sisältää esittely kyselyn Microsoft Graph riskin tapahtumien ja niihin liittyviä tietoja luettelo Azure Active Directorysta."
    services="active-directory"
    keywords="Azure active Directoryn tunnistetietojen suojaus, riskitilanteessa, heikkous suojauskäytäntö, Microsoft Graph"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="markvi"/>

# <a name="get-started-with-azure-active-directory-identity-protection-and-microsoft-graph"></a>Azure Active Directory tunnistetietojen suojaus- ja Microsoft Graph käytön aloittaminen

Microsoft Graph on Microsoftin yhdistetyn API päätepiste ja [Azure Active Directory tunnistetietojen suojaus on](active-directory-identityprotection.md) ohjelmointirajapinnan aloitus. Microsoftin ensimmäisen API **identityRiskEvents**mahdollistavat kyselyn Microsoft Graph luettelo [riskin tapahtumien](active-directory-identityprotection-risk-events-types.md) ja niihin liittyviä tietoja. Tämän artikkelin avulla pääset alkuun tämän API kyselyt. Tekstimuodossa syvyys johdanto täydelliset asiakirjat ja Graph-Explorer pääsyn on [Microsoft Graph-sivustossa](https://graph.microsoft.io/).

Sisältää kolme vaihetta – Microsoft Graph tunnistetiedot tietojen käyttäminen:

1. Lisää sovellus on asiakkaan salaisuus. 

2. Microsoft Graph, johon haluat vastaanottaa todennus-tunnuksen todentaa tämän salaisuus ja joitakin muita tietoja allekirjoitettavaksi avulla. 

3. Tämän tunnuksen avulla voit tehdä pyynnöt API päätepisteen ja palata tunnistetiedot tiedot.

Ennen kuin aloitat, sinun on:

- Järjestelmänvalvojan oikeudet Azure AD sovelluksen luominen
- Oman vuokraajan toimialue (esimerkiksi contoso.onmicrosoft.com)



## <a name="add-an-application-with-a-client-secret"></a>Lisää sovellus on asiakkaan salaisuus


1. [Kirjaudu sisään](https://manage.windowsazure.com) Azure perinteinen-portaaliin järjestelmänvalvojana. 

1. Valitse vasemmassa siirtymisruudussa Valitse **Active Directorysta**. 

    ![Sovelluksen luominen](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_01.png)

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Valitse ylä-valikossa **sovellukset**.

    ![Sovelluksen luominen](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_02.png)

4. Valitse sivun alareunassa **Lisää** .

    ![Sovelluksen luominen](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_03.png)

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus kehittää oman organisaation ulkopuolelta**.

    ![Sovelluksen luominen](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_04.png)


5. **Kerro sovelluksesi tietoja** -valintaikkunassa seuraavasti:

    ![Sovelluksen luominen](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_05.png)

    a. Kirjoita nimi sovelluksen **nimi** -tekstiruutuun (esimerkiksi: AADIP riski tapahtuman API sovellus).

    b. Valitse **tyyppi**- **Verkko-sovelluksen ja/tai verkko-Ohjelmointirajapinnan**.

    c-näppäinyhdistelmää. Valitse **Seuraava**.


5. Suorita **sovellus ominaisuudet** -valintaikkunassa seuraavasti:

    ![Sovelluksen luominen](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_06.png)

    a. Kirjoita **Sign URL** -tekstiruutuun `http://localhost`.

    b. Kirjoita **Sovelluksen tunnus URI** -tekstiruutuun `http://localhost`.

    c-näppäinyhdistelmää. Valitse **Valmis**.


Oman voi nyt määrittää sovelluksen.

![Sovelluksen luominen](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_07.png)



## <a name="grant-your-application-permission-to-use-the-api"></a>Myönnä käyttöoikeudet sovelluksen Ohjelmointirajapinnan käyttäminen


1. Sovelluksen-sivun yläreunassa valikko, valitse **Määritä**. 

    ![Sovelluksen luominen](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_08.png)

2. Valitse **muiden sovellusten käyttöoikeudet** -osassa **Lisää sovellus**.

    ![Sovelluksen luominen](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_09.png)


2. Valitse **muiden sovellusten käyttöoikeudet** -valintaikkunassa seuraavasti:

    ![Sovelluksen luominen](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_10.png)

    a. Valitse **Microsoft Graph**.

    b. Valitse **Valmis**.


1. Valitse **sovelluksen käyttöoikeudet: 0**, ja valitse sitten **lukea kaikki riski tapahtuman tunnistetietoja**.

    ![Sovelluksen luominen](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_11.png)

1. Valitse sivun alareunassa **Tallenna** .

    ![Sovelluksen luominen](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_12.png)


## <a name="get-an-access-key"></a>Hae pikanäppäin

1. Valitse vuoden sovelluksen-sivulla kohta **näppäimet** kestona.

    ![Sovelluksen luominen](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_13.png)

1. Valitse sivun alareunassa **Tallenna** .

    ![Sovelluksen luominen](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_12.png)

1. näppäimet-osassa juuri luomasi avaimen kopioi ja liitä se sitten turvalliseen paikkaan.

    ![Sovelluksen luominen](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_14.png)

    > [AZURE.NOTE] Jos kadotat avaimeen, sinun on tämän osan palaa ja luo uusi avain. Säilytä avaimeen salaisuus: kaikki, joilla on se käyttää tietoja.


1. **Ominaisuudet** -osan **Ostajantunnus**kopioi ja liitä se sitten turvalliseen paikkaan. 


## <a name="authenticate-to-microsoft-graph-and-query-the-identity-risk-events-api"></a>Microsoft Graph todennusta ja kyselyjen tunnistetietojen riskin tapahtumien Ohjelmointirajapinnan

Tässä vaiheessa pitäisi olla:

- Edellä kopioitu Ostajantunnus

- Edellä kopioitu avain

- Oman vuokraajan toimialueen nimi


Todentaa, Lähetä pyyntö kirjaa `https://login.microsoft.com` tekstissä seuraavilla parametreilla:

- grant_type: "**client_credentials**"

- resurssi: "**https://graph.microsoft.com**"

- client_id:<your client ID>

- client_secret:<your key>


> [AZURE.NOTE] Sinun on annettava **client_id** ja **client_secret** -parametrin arvot.

Jos onnistuu, tämä palauttaa todennus-tunnuksen.  
Soita Ohjelmointirajapinnan seuraavilla parametreilla ylätunnisteen luominen:

    `Authorization`=”<token_type> <access_token>"


Kun todennustapa, löytyvät tunnuksen tyyppi ja käyttöoikeustietue palautetut tunnuksen.

Lähetä tämä ylätunniste siten, että seuraava API URL-osoite:`https://graph.microsoft.com/beta/identityRiskEvents`

Vastauksen, jos onnistuu, on kokoelma tunnistetietojen riskin tapahtuma-ja liittyvät tiedot OData JSON-muotoon, joka voidaan jäsentää ja käsitellä tavalla sopivalta.

Tässä on esimerkki koodi todennustapa ja soittavan Ohjelmointirajapinnan PowerShellin avulla.  
Voit lisätä Asiakastunnus-näppäintä ja vuokraaja toimialueen.

    $ClientID       = "<your client ID here>"        # Should be a ~36 hex character string; insert your info here
    $ClientSecret   = "<your client secret here>"    # Should be a ~44 character string; insert your info here
    $tenantdomain   = "<your tenant domain here>"    # For example, contoso.onmicrosoft.com

    $loginURL       = "https://login.microsoft.com"
    $resource       = "https://graph.microsoft.com"

    $body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}
    $oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

    Write-Output $oauth

    if ($oauth.access_token -ne $null) {
        $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}

        $url = "https://graph.microsoft.com/beta/identityRiskEvents"
        Write-Output $url

        $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)

        foreach ($event in ($myReport.Content | ConvertFrom-Json).value) {
            Write-Output $event
        }

    } else {
        Write-Host "ERROR: No Access Token"
    } 


## <a name="next-steps"></a>Seuraavat vaiheet

Onnittelut, tekemäsi Microsoft Graph ensimmäisen puhelu!  
Voit nyt kyselyn tunnistetietojen riskin tapahtumia ja käyttää tietoja, mutta kohdassa Sovita.

Lisätietoja Microsoft Graph ja miten voit luoda sovelluksia Graph-Ohjelmointirajapinnan käyttäminen, tarkista [asiakirjat](https://graph.microsoft.io/docs) ulos ja paljon muuta [Microsoft Graph-sivustossa](https://graph.microsoft.io/). Varmista myös, että kirjanmerkki [Azure AD-tunnistetietojen suojauksen API](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root) -sivu, jossa on luettelo kaikista käytettävissä Graphiin tunnistetietojen suojaus-ohjelmointirajapinnan. Kun lisäämme uusia tapoja työskennellä tunnistetietojen käyttöoikeudella Ohjelmointirajapinnan kautta, näet ne sivulla.


## <a name="additional-resources"></a>Lisäresursseja

- [Azure Active Directory-tunnistetietojen suojaus](active-directory-identityprotection.md)

- [Riskin tapahtumien havaitsemien Azure Active Directory tunnistetietojen suojaus](active-directory-identityprotection-risk-events-types.md)

- [Microsoft Graph](https://graph.microsoft.io/)

- [Yleistä Microsoft Graph](https://graph.microsoft.io/docs)

- [Azure AD-tunnistetietojen suojauksen palvelun pääkansio](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root)

<properties
    pageTitle="Azure Active Directory-B2C: Yleiskatsaus | Microsoft Azure"
    description="Kuluttaja osoittava kehityssovellusten Azure Active Directory-B2C kanssa"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-sign-up-and-sign-in-consumers-in-your-applications"></a>Azure Active Directory-B2C: Rekisteröidy ja kirjaudu sisään sovelluksesi käyttäjät

Azure Active Directory-B2C on täydellinen cloud identity-hallintaratkaisu kuluttaja osoittava web-ja mobile-sovellukset. On erittäin käytettävissä yleinen palvelu, joka satoja kuluttaja käyttäjätietojen miljoonia Skaalaa. Rakennettu yrityksen arvosanojen suojatun ympäristössä Azure Active Directory-B2C pitää sovellukset ja yrityksesi lukijoille suojattu.

Aiemmin sovelluskehittäjille, jotka haluat rekisteröityä ja kirjaudu sisään kuluttajien niiden sovelluksiin kirjoittanut oman koodin. Ja ne olisi käyttänyt paikallisen tietokannan tai järjestelmien käyttäjänimet ja salasanat. Azure Active Directory-B2C on kehittäjät paremman tavan integroida verkkosovelluksistaan avulla suojattua ja standardit ympäristö ja extensible käytännöt monipuoliset kuluttaja tunnistetietojen hallinta. Azure Active Directory-B2C käytettäessä lukijoille voit rekisteröityä sovellustesi käyttämällä niiden aiemmin sosiaalisten tilit (Facebook, Google, Amazon, Linkedinin) tai luomalla uuden tunnistetiedot (sähköpostiosoite ja salasana, tai käyttäjänimi ja salasana); Olemme Soita jälkimmäisessä "paikalliset tilit."

## <a name="get-started"></a>Käytön aloittaminen

Sovellus, joka hyväksyy kuluttajien rekisteröityminen ja kirjaudu sisään, voit luoda on ensimmäinen tarvitse rekisteröidä sovelluksen Azure Active Directory-B2C palveltavan. Pyydä oman vuokraajan [luominen Azure AD B2C vuokraajan](active-directory-b2c-get-started.md)annettujen ohjeiden avulla.

Voit kirjoittaa sovelluksen vastaan Azure Active Directory-B2C protokolla viestien lähettämiseen suoraan [OAuth 2.0](active-directory-b2c-reference-protocols.md#oauth2-authorization-code-flow) tai [Avaa tunnus yhteyden](active-directory-b2c-reference-protocols.md#openid-connect-sign-in-flow), valitsemalla tai tutustu kirjastojen avulla voit tehdä tarvittavat työt puolestasi. Valitse suosikki omaa ympäristöäsi seuraavassa taulukossa ja käytön aloittaminen.

[AZURE.INCLUDE [active-directory-b2c-quickstart-table](../../includes/active-directory-b2c-quickstart-table.md)]

## <a name="whats-new"></a>Mitä uutta

Valitse tähän usein saat lisätietoja Azure Active Directory-B2C muutokset. Microsoft myös tweet päivityksiä tietoja käyttämällä @AzureAD.

- Lue tietoja Microsoftin [extensible toimintaperiaatteet](active-directory-b2c-reference-policies.md) ja käytäntöjä, joilla voi luoda ja käyttää sovellustesi tyypit.
- Lisää kirjanmerkki meidän [pilvipalvelublogin](https://blogs.msdn.microsoft.com/azureadb2c/) pieniä palveluongelmat, päivitykset, tila ja ongelman pienentämistavat ilmoituksia. Siirry [Azure tila Raporttinäkymät-ikkunan](https://azure.microsoft.com/status/) sekä seurata.
- Nykyinen [palvelun rajoituksista, rajoitukset ja rajoitukset](active-directory-b2c-limitations.md).
- Lopuksi [koodin otoksen](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect-aspnetcore-b2c) Azure AD B2C & ASP.NET Core avulla.

## <a name="how-to-articles"></a>Ohjeartikkelit

Opettele käyttämään Azure Active Directory-B2C lisäominaisuuksia:

- [Facebook](active-directory-b2c-setup-fb-app.md), [Google +](active-directory-b2c-setup-goog-app.md) [Microsoft-tiliä](active-directory-b2c-setup-msa-app.md), [Amazon](active-directory-b2c-setup-amzn-app.md)tai [LinkedIn](active-directory-b2c-setup-li-app.md) -tilien käytettäväksi määrittäminen kuluttaja osoittava-sovelluksissa.
- [Käytä mukautettuja määritteitä kerää tietoja lukijoille](active-directory-b2c-reference-custom-attr.md).
- [Ota käyttöön Azure Monimenetelmäisen todentamisen kuluttaja osoittava-sovelluksissa](active-directory-b2c-reference-mfa.md).
- [Käyttäjien Omatoiminen salasanan palauttaminen lukijoille määrittäminen](active-directory-b2c-reference-sspr.md).
- [Rekisteröidy, kirjaudu sisään, ulkoasu ja kuluttaja osoittava muilla sivuilla](active-directory-b2c-reference-ui-customization.md) , jotka avautuvat Azure Active Directory-B2C.
- [Käytä Azure Active Directory Graph Ohjelmointirajapinnan, ohjelmallisesti luoda, lukea, päivittää, ja poistaa kuluttajille](active-directory-b2c-devquickstarts-graph-dotnet.md) Azure Active Directory-B2C vuokraajan.

## <a name="next-steps"></a>Seuraavat vaiheet

Seuraavissa linkeissä on hyötyä tutustuminen perusteellisesti palvelu:

- [Azure Active Directory-B2C hintatiedot](https://azure.microsoft.com/pricing/details/active-directory-b2c/)näkyvissä.
- Ohjeita Pinon ylivuoto käyttämällä [azure active-kansio](http://stackoverflow.com/questions/tagged/azure-active-directory) tai [adal](http://stackoverflow.com/questions/tagged/adal) tunnisteet.
- Anna ajatuksesi käyttämällä [Käyttäjän Voice](https://feedback.azure.com/forums/169401-azure-active-directory/)--haluamme kuulla niitä! Käytä ilmaisu "AzureADB2C:" viestiin, jotta löydämme sen nimi.
- Tarkista [Azure AD-B2C protokolla viittaus](active-directory-b2c-reference-protocols.md).
- Tarkista [Azure AD-B2C suojaustunnuksen viittaus](active-directory-b2c-reference-tokens.md).
- Lue [Azure Active Directory B2C usein kysyttyjä kysymyksiä](active-directory-b2c-faqs.md).
- [Tiedoston tukevat Azure Active Directory-B2C pyynnöt](active-directory-b2c-support.md).

## <a name="get-security-updates-for-our-products"></a>Päivitysten hakeminen tuotteitamme

On suositeltavaa, kun suojauksen tapaukset esiintyvät käymällä [Tämän sivun](https://technet.microsoft.com/security/dd252948) ja tilaamalla neuvoa suojausvaroitusten ilmoitusten saaminen.

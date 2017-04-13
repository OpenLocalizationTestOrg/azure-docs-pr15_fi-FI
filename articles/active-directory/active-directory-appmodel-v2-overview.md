<properties
    pageTitle="v2.0 päätepisteen yleiskatsaus | Microsoft Azure"
    description="Johdanto ongelmia Microsoft-Account ja Azure Active Directory-sisäänkirjautumisessa sovellusten rakentamiseen."
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="dastrock"/>

# <a name="sign-in-microsoft-account--azure-ad-users-in-a-single-app"></a>Kirjaudu sisään Microsoft-Account & yhden sovelluksen Azure AD-käyttäjät

Aiemmin sovelluksen kehittäjä, joka määrittää tukemaan Microsoftin tili- ja Azure Active Directory on tarvittavan integroimalla ja kaksi erillistä järjestelmien.  Olemme olet nyt tuotu uusi todennusta API versio, jolla voit kirjautua sisään molemmat käyttäjätilit Azure AD-järjestelmän avulla käyttäjät.  Päätynyt todentaminen järjestelmä on nimeltään **v2.0 päätepiste**.  V2.0 päätepisteissä yksi Yksinkertainen integraatio antaa saavuttaa käyttäjäryhmän, joka ulottuu miljoonia sekä henkilökohtainen ja työpaikan tai oppilaitoksen tilin käyttäjille.

Sovellukset, jotka käyttävät v2.0 päätepisteen voit myös käyttää REST API [Microsoft Graph](https://graph.microsoft.io) -ja [Office 365: ssä](https://msdn.microsoft.com/office/office365/howto/authenticate-Office-365-APIs-using-v2) käyttämällä joko tilin tyyppi.

<!-- For a quick introduction to the v2.0 endpoint, please view the [Getting Started with Microsoft Identities: Enterprise Grade Sign In For Your Apps](https://azure.microsoft.com/documentation/videos/build-2016-getting-started-with-microsoft-identities-enterprise-grade-sign-in-for-your-apps/) video. -->

## <a name="getting-started"></a>Käytön aloittaminen
[AZURE.VIDEO build-2016-getting-started-with-microsoft-identities-enterprise-grade-sign-in-for-your-apps]

Valitse suosikki omaa ympäristöäsi luonnissa sovelluksen asentamisohjeisiin Avaa lähde-kirjastojen ja kehysten käyttäminen seuraavasta luettelosta.  Vaihtoehtoisesti voit, Lähetä ja vastaanota viestit protokolla suoraan käyttämättä auth kirjaston Microsoftin OAuth 2.0 & OpenID yhteyden protokolla-ohjeista.

<!-- TODO: Finalize this table  -->
[AZURE.INCLUDE [active-directory-v2-quickstart-table](../../includes/active-directory-v2-quickstart-table.md)]

## <a name="whats-new"></a>Mitä uutta
Käsitteellisiä tietoja on hyödyllisiä tietoja, mikä on ja mitä ei ole mahdollista v2.0 päätepiste.

- Lisätietoja [tyyppisiä sovelluksia, voit luoda v2.0 päätepisteen kanssa](active-directory-v2-flows.md).
- Ymmärtämään [rajoitukset, rajoitukset ja rajoitusten](active-directory-v2-limitations.md) v2.0 päätepisteen kanssa.
- Olemme lisänneet viimeksi [järjestelmänvalvojan rajoitettu käyttöalueen](active-directory-v2-scopes.md) ja [myöntää tunnistetiedot OAuth2-asiakasohjelman](active-directory-v2-protocols-oauth-client-creds.md)tuki.  Kokeile!

## <a name="reference"></a>Viittaus
Seuraavissa linkeissä on hyötyä tutustuminen perusteellisesti ympäristö:

- Muodosta 2016: [käytön aloittaminen Microsoft käyttäjätietojen: yrityksen arvosana kirjautunut sisään sovelluksia](https://azure.microsoft.com/documentation/videos/build-2016-getting-started-with-microsoft-identities-enterprise-grade-sign-in-for-your-apps/)
- Ohjeita Pinon ylivuoto [azure active-kansion](http://stackoverflow.com/questions/tagged/azure-active-directory) käyttäminen tai [adal](http://stackoverflow.com/questions/tagged/adal) tunnisteet.
- [v2.0 protokolla viittaus](active-directory-v2-protocols.md)
- [v2.0 tunnuksen viittaus](active-directory-v2-tokens.md)
- [v2.0 kirjaston ohje](active-directory-v2-libraries.md)
- [Käyttöalueen ja v2.0 päätepisteen lupaa](active-directory-v2-scopes.md)
- [Microsoft-sovelluksen rekisteröinti-portaalissa](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)
- [Office 365: n REST API-viittaus](https://msdn.microsoft.com/office/office365/howto/authenticate-Office-365-APIs-using-v2)
- [Microsoft Graph](https://graph.microsoft.io)
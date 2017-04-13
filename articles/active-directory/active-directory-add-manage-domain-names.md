<properties
    pageTitle="Hallinta mukautettuja toimialuenimiä Azure Active Directoryssa | Microsoft Azure"
    description="Hallinnan käsitteitä ja how-tos mukautetun Azure Active Directory-toimialueen hallintaan"
    services="active-directory"
    documentationCenter=""
    authors="jeffsta"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="curtand;jeffsta"/>

# <a name="managing-custom-domain-names-in-your-azure-active-directory"></a>Azure Active Directoryssa mukautettuja toimialuenimiä hallinta

Toimialuenimi on tärkeä osa monta directory-resurssien tunnus: se on osa käyttäjän nimi tai sähköpostiosoite käyttäjän, ryhmän, osoitteen ja voi olla osa sovelluksen tunnus URI-sovellusta. Resurssin Azure Active Directory (Azure AD) määrittää toimialuenimi, joka on jo vahvistettu kansio, joka sisältää resurssin omistaa. Yleinen järjestelmänvalvoja voi suorittaa toimialueen hallintatehtäviä Azure AD.

## <a name="set-the-primary-domain-name-for-your-azure-ad-directory"></a>Azure Active Directory ensisijaisen toimialuenimen määrittäminen

Hakemiston luomisen jälkeen alkuperäinen verkkotunnus, kuten "contoso.onmicrosoft.com" on myös hakemistossa toimialuenimi. Ensisijainen toimialue on oletustoimialuenimen uudelle käyttäjälle, kun luot uuden käyttäjän [Azure perinteinen portal](https://manage.windowsazure.com/)tai muita portaaleihin, kuten Office 365-hallintaportaalissa. Tämä tehostava järjestelmänvalvoja voi luoda uusia käyttäjiä-portaalissa.

Voit muuttaa hakemistossa ensisijaisen toimialuenimen seuraavasti:

1.  Kirjautuminen [Azure perinteinen portal](https://manage.windowsazure.com/) käyttäjätili, jolla on yleisen järjestelmänvalvojan Azure AD-hakemiston kanssa.

2.  Valitse vasemman reunan siirtymispalkissa **Active Directorysta** .

3.  Avaa hakemistossa.

4.  Valitse **Toimialueet** -välilehti.

5.  Valitse komentopalkista **ensisijainen muuta** -painiketta.

6.  Valitse toimialue, joihin haluat lisätä hakemiston uusi ensisijainen toimialue.

Voit muuttaa hakemistossa on vahvistettu mukautetun toimialueen, joka ei ole liitetty toimialuenimi. Hakemiston ensisijainen toimialue muuttaminen ei muuta käyttäjänimet kaikki aiemmin luodut käyttäjät.

## <a name="add-custom-domain-names-to-your-azure-ad"></a>Lisätä mukautettuja toimialuenimiä Azure AD

Voit lisätä enintään 900 mukautettuja toimialuenimiä kunkin Azure AD-kansioon. Prosessi, jos haluat [lisätä muita mukautettua toimialuenimeä](active-directory-add-domain.md) on sama ensimmäisen mukautettua toimialuenimeä.

## <a name="add-subdomains-of-a-custom-domain"></a>Lisätä mukautetun toimialueen alitoimialueita

Jos haluat lisätä kolmannen tason toimialuenimi, esimerkiksi "europe.contoso.com"-kansioon, kannattaa ensin Lisää ja tarkista toisen tason toimialue, esimerkiksi contoso.com. Alitoimialue tarkistetaan automaattisesti Azure AD mukaan. Näet juuri lisäämäsi alitoimialue on tarkistettu, Päivitä sivu selaimessa, jossa on luettelo hakemistossa toimialueet.

## <a name="what-to-do-if-you-change-the-dns-registrar-for-your-custom-domain-name"></a>Jos muutat DNS-palveluun mukautetun toimialueen nimeä

Jos muutat DNS-rekisteröintipalvelu mukautettua toimialuenimeä, voit jatkaa oman toimialuenimen käyttäminen Azure AD itse keskeytyksettä ja ilman muita määritystehtäviä. Jos käytät mukautettua toimialuenimeä Office 365: n, Intune ja muut palvelut, jotka perustuvat mukautettuja toimialuenimiä Azure AD-viitata näistä palveluista ohjeissa.

## <a name="delete-a-custom-domain-name"></a>Mukautetun toimialuenimen poistaminen

Voit poistaa Azure AD-mukautettua toimialuenimeä, jos organisaatiosi käyttää enää kyseisen toimialueen kohdalta tai jos haluat käyttää kyseisen toimialueen kohdalta toiseen Azure AD.

Jos haluat poistaa mukautettua toimialuenimeä, varmista ensin hakemistossa resursseja ei riippuvaisia toimialuenimi. Et voi poistaa toimialuenimen hakemistossa, jos:

-   Kuka tahansa käyttäjä on käyttäjänimi, sähköpostiosoite tai välityspalvelimen osoite, jotka sisältävät toimialuenimi.

-   Minkä tahansa ryhmän on sähköpostiosoite tai välityspalvelimen osoite, joka sisältää muutettavan toimialuenimen.

-   Mikä tahansa Azure AD-sovellus on sovelluksen tunnus URI, joka sisältää muutettavan toimialuenimen.

On muuta tai poista näiden resurssien Azure AD-kansiossa, ennen kuin voit poistaa mukautetun toimialuenimeä.

## <a name="use-powershell-or-graph-api-to-manage-domain-names"></a>Toimialuenimien hallinta PowerShellin tai Graph API avulla

Useimmat hallintatehtäviä verkkotunnusten Azure Active Directoryn voi tehdä myös Microsoft PowerShellin avulla tai Azure AD-kaavio-Ohjelmointirajapinnan käyttäminen ohjelmallisesti (julkisen esikatselunäkymässä).

-   [Toimialuenimien hallinta Azure AD-PowerShellin avulla](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)

-   [Toimialuenimien hallinta Azure AD-kaavion API avulla](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)

## <a name="next-steps"></a>Seuraavat vaiheet

-   [Tietoja toimialuenimistä Azure AD-](active-directory-add-domain-concepts.md)

-   [Mukautetun toimialuenimien hallinta](active-directory-add-manage-domain-names.md)

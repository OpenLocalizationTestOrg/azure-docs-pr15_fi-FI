<properties
    pageTitle="Azure Active Directory-esikatselu mukautettuja toimialuenimiä hallinta | Microsoft Azure"
    description="Hallinnan käsitteitä ja how-tos hallintaan Azure Active Directory-toimialuenimi"
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
    ms.date="09/12/2016"
    ms.author="curtand;jeffsta"/>

# <a name="managing-custom-domain-names-in-your-azure-active-directory-preview"></a>Azure Active Directory-esikatselu mukautettuja toimialuenimiä hallinta

Toimialuenimi on tärkeä osa monta directory-resurssien tunnus: se on osa käyttäjän nimi tai sähköpostiosoite käyttäjän, ryhmän, osoitteen ja voi olla osa sovelluksen tunnus URI-sovellusta. Azure Active Directory (Azure AD) esikatselussa resurssi voi olla toimialuenimi, joka on jo vahvistettu kansio, joka sisältää resurssin omistaa. [Mikä on esikatselussa?](active-directory-preview-explainer.md) Yleinen järjestelmänvalvoja voi suorittaa toimialueen hallintatehtäviä Azure AD.

## <a name="set-the-primary-domain-name-for-your-azure-ad-directory"></a>Azure Active Directory ensisijaisen toimialuenimen määrittäminen

Hakemiston luomisen jälkeen alkuperäinen verkkotunnus, kuten "contoso.onmicrosoft.com" on myös ensisijaisen toimialuenimen. Ensisijainen toimialue on oletustoimialuenimen uudelle käyttäjälle, kun luot uuden käyttäjän. Tämä tehostava järjestelmänvalvoja voi luoda uusia käyttäjiä-portaalissa. Voit muuttaa ensisijaisen toimialuenimen seuraavasti:

1.  Kirjautuminen [Azure portaaliin](https://portal.azure.com) tilille, jolla on yleisen järjestelmänvalvojan kansion.

2.  Valitse **Lisää palveluja**, kirjoita **Azure Active Directory** tekstiruutuun ja paina sitten **ENTER-näppäintä**.

    ![Avaava käyttäjien hallinta](./media/active-directory-domains-add-azure-portal/user-management.png)

3. Valitse **toimialuenimien** ***kansion nimi*** -sivu.

4. Valitse ** *kansion nimi* - toimialuenimet** -sivu, jotka haluat tehdä ensisijaisen toimialuenimen toimialuenimi.

5.  Valitse ***toimialue*** -sivu (eli sivu, joka avautuu uusi toimialuenimi on otsikko) **Määritä ensisijaiseksi** -komentoa. Vahvista valintasi pyydettäessä.

    ![Määritä toimialuenimi ensisijaiseksi](./media/active-directory-domains-manage-azure-portal/make-primary.png)

Voit muuttaa hakemistossa on vahvistettu mukautetun toimialueen, joka ei ole liitetty toimialuenimi. Hakemiston ensisijainen toimialue muuttaminen ei muuta käyttäjänimet kaikki aiemmin luodut käyttäjät.

## <a name="add-custom-domain-names-to-your-azure-ad"></a>Lisätä mukautettuja toimialuenimiä Azure AD

Voit lisätä enintään 900 mukautettuja toimialuenimiä kunkin Azure AD-kansioon. Prosessi, jos haluat [lisätä muita mukautettua toimialuenimeä](active-directory-domains-add-azure-portal.md) on sama ensimmäisen mukautettua toimialuenimeä.

## <a name="add-subdomains-of-a-custom-domain"></a>Lisätä mukautetun toimialueen alitoimialueita

Jos haluat lisätä kolmannen tason toimialuenimi, esimerkiksi "europe.contoso.com"-kansioon, kannattaa ensin Lisää ja tarkista toisen tason toimialue, esimerkiksi contoso.com. Alitoimialue tarkistetaan automaattisesti Azure AD mukaan. Näet juuri lisäämäsi alitoimialue on tarkistettu, Päivitä sivu selaimessa, jossa on luettelo toimialueet.

## <a name="what-to-do-if-you-change-the-dns-registrar-for-your-custom-domain-name"></a>Jos muutat DNS-palveluun mukautetun toimialueen nimeä

Jos muutat DNS-rekisteröintipalvelu mukautettua toimialuenimeä, voit jatkaa oman toimialuenimen käyttäminen Azure AD itse keskeytyksettä ja ilman muita määritystehtäviä. Jos käytät mukautettua toimialuenimeä Office 365: n, Intune ja muut palvelut, jotka perustuvat mukautettuja toimialuenimiä Azure AD-viitata näistä palveluista ohjeissa.

## <a name="delete-a-custom-domain-name"></a>Mukautetun toimialuenimen poistaminen

Voit poistaa Azure AD-mukautettua toimialuenimeä, jos organisaatiosi käyttää enää kyseisen toimialueen kohdalta tai jos haluat käyttää kyseisen toimialueen kohdalta toiseen Azure AD.

Jos haluat poistaa mukautettua toimialuenimeä, varmista ensin hakemistossa resursseja ei riippuvaisia toimialuenimi. Et voi poistaa toimialuenimen hakemistossa, jos:

-   Kuka tahansa käyttäjä on käyttäjänimi, sähköpostiosoite tai välityspalvelimen osoite, joka sisältää muutettavan toimialuenimen.

-   Minkä tahansa ryhmän on sähköpostiosoite tai välityspalvelimen osoite, joka sisältää muutettavan toimialuenimen.

-   Mikä tahansa Azure AD-sovellus on sovelluksen tunnus URI, joka sisältää muutettavan toimialuenimen.

On muuta tai poista näiden resurssien Azure AD-kansiossa, ennen kuin voit poistaa mukautetun toimialuenimeä.

## <a name="use-powershell-or-graph-api-to-manage-domain-names"></a>Toimialuenimien hallinta PowerShellin tai Graph API avulla

Useimmat hallintatehtäviä verkkotunnusten Azure Active Directoryn voi tehdä myös Microsoft PowerShellin avulla tai Azure AD-kaavio-Ohjelmointirajapinnan käyttäminen ohjelmallisesti (julkisen esikatselunäkymässä).

-   [Toimialuenimien hallinta Azure AD-PowerShellin avulla](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)

-   [Toimialuenimien hallinta Azure AD-kaavion API avulla](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)

## <a name="next-steps"></a>Seuraavat vaiheet

-   [Lisätä mukautettuja toimialuenimiä](active-directory-domains-add-azure-portal.md)

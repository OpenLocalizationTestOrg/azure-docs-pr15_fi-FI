<properties
    pageTitle="Mukautetun toimialueen ja määrittää liitetyt Sign Azure Active Directory | Microsoft Azure"
    description="Yrityksen toimialuenimien lisääminen Azure Active Directory ja määritysten liitetty Sign Azure Active Directory ja paikallisen federation-ratkaisun välillä."
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
    ms.topic="get-started-article"
    ms.date="10/04/2016"
    ms.author="curtand;jeffsta"/>

# <a name="add-your-custom-domain-name-to-azure-active-directory"></a>Mukautetun toimialuenimen lisääminen Azure Active Directory

Voit määrittää mukautettua toimialuenimeä, kuten "contoso.com" niin, että toimialueen contoso.com käyttäjillä voi olla liitetty yhden Sign kokemusta yrityksen verkossa. Jos sinulla on jo Active Directory Federation Services (AD FS) tai yrityksen verkossa eri liittoutumispalvelimen, voit määrittää Azure AD käyttämään mukautettua toimialuenimeä Azure AD Connect-työkalun avulla. Voit myös Azure AD Connect ottaminen käyttöön uuden AD FS-ympäristön avulla ja määritä, saat liitetyt kertakirjautumisen, Azure AD.

Jos ei ole ja et aio käyttöön AD FS tai toisen liittoutumispalvelimen, noudata seuraavia ohjeita: [Lisää Azure Active Directory mukautettua toimialuenimeä](active-directory-add-domain.md).

## <a name="add-a-custom-domain-name-to-your-directory"></a>Mukautetun toimialuenimen lisääminen kansioon

1. Kirjautuminen [Azure perinteinen portal](https://manage.windowsazure.com/) käyttäjätili, jolla on yleisen järjestelmänvalvojan Azure AD-hakemiston kanssa.

2. **Active Directory**-hakemiston Avaa ja valitse **Toimialueet** -välilehti.

3. Valitse komentopalkista **Lisää**ja kirjoita sitten mukautetun toimialueen, kuten "contoso.com" nimi. Muista lisätä .com, .net tai muiden ylimmän tason tunniste.

4. Valitse **voin aiot määrittää toimialueessa, kertakirjautuminen Omat paikallisen Active Directory** -valintaruutu.

5. Valitse **Lisää**.

Saat DNS-merkintä, joka Azure AD käyttää toimialueen vahvistamiseksi Azure AD Connect-työkalun suorittaminen. Näkyviin tulee DNS-merkintä **Azure AD-toimialueen** ohjatun toiminnon vaiheessa. Näet, mitä ohjatun toiminnon vaiheessa näyttää [näissä ohjeissa](active-directory-aadconnect-get-started-custom.md#verify-the-azure-ad-domain-selected-for-federation). Jos sinulla ei ole Azure AD Connect-työkalua, voit [ladata sen tähän](http://go.microsoft.com/fwlink/?LinkId=615771).

## <a name="add-the-dns-entry-at-the-domain-name-registrar-for-the-domain"></a>Lisää toimialuerekisteröijälle toimialueen DNS-merkintä

Jos haluat käyttää mukautettua toimialuenimeä Azure AD seuraava vaihe on päivitettävä toimialueen DNS-Vyöhyketiedosto. Näin voit varmistaa, että organisaation omistaa mukautettua toimialuenimeä Azure AD.

1. Kirjaudu oman toimialuenimen toimialuerekisteröijältä sivustoa. Jos access-toiminto ei ole, pyydä henkilön tai ryhmän organisaatiossa, joilla on tämän vaiheen 2 ja ilmoittaa, kun se on valmis.

2. Päivitä toimialueen DNS-Vyöhyketiedosto lisäämällä Azure AD myöntämä DNS-merkintä. Tämän DNS-vaihtoehdon avulla Azure AD tarkistamaan sivuston omistajuutesi toimialueen. DNS-merkintä ei muuta kaikki toiminnat, kuten sähköpostin reititys tai web-kansio.

Ohjeet tämän vaiheen Lue [ohjeet DNS-määritys, jonka Suositut DNS-rekisteröintipalveluissa](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/)

## <a name="verify-the-domain-name-with-azure-ad"></a>Tarkista Azure AD toimialuenimi

Kun olet lisännyt DNS-merkintä, olet valmis vahvistamiseksi Azure AD toimialuenimeä.

Tarkista toimialue, valitse **Azure AD-toimialue** Azure AD Connect ohjatun toiminnon vaiheessa **Seuraava** . Azure AD Näyttää DNS-Vyöhyketiedosto toimialueen DNS-merkinnän. Azure AD vahvistaa toimialuenimeä vain, kun DNS-tietueet on välitetty. Välitys usein kestää vain sekuntia, mutta se voi viedä joskus yli. Jos vahvistus ei onnistu ensimmäistä kertaa, yritä uudelleen myöhemmin.

Tämän jälkeen voit jatkaa Azure AD Connect ohjatun toiminnon vaiheet. Tämä Synkronoi Azure AD Windows-palvelimessa AD käyttäjät. Valitse toimialue, jonka määritit yhdistämiseen synkronoidut käyttäjät voi a liitetyt yksittäisen Sign-toiminto siirtyä Azure AD yrityksen verkkoon.

## <a name="troubleshooting"></a>Vianmääritys

Jos et pysty tarkistamaan mukautettua toimialuenimeä, toimi seuraavasti. Microsoft eniten yhteinen aloittaminen ja toimi alaspäin vähintään Yleiset.

1.  **Odota tunnissa**. DNS-tietueet on leviäminen ennen Azure AD voi tarkistaa toimialueen. Tämä voi kestää yli.

2.  **Varmista, että DNS-tietue on määritetty, ja se on oikein**. Suorita tämä vaihe sivustossa toimialueen toimialuerekisteröijälle. Azure AD ei voi vahvistaa toimialuenimeä, jos DNS-merkintä ei ole DNS-Vyöhyketiedosto tai jos se ei ole tarkkaa vastinetta DNS kohteesta kyseisen Azure AD edellyttäen. Jos access toimialuerekisteröijältä toimialueen DNS-tietueiden päivittäminen ei ole, DNS-merkintä jakaminen henkilön tai ryhmän organisaatiossa, joilla on tämä käyttöoikeus ja pyytää heitä Lisää DNS-merkintä.

3.  **Poista toisen hakemistosta Azure AD-toimialuenimi**. Vain yhden kansion voi tarkistaa toimialuenimi. Jos toimialuenimi on tarkistettu aiemmin toiseen hakemistoon, se poistettava siellä, ennen kuin sitä voidaan varmistaa uusi kansio. Saat lisätietoja poistaminen toimialuenimien [hallinta mukautettuja toimialuenimiä](active-directory-add-manage-domain-names.md)lukeminen

## <a name="add-more-custom-domain-names"></a>Lisää Lisää mukautettuja toimialuenimiä

Jos organisaatiosi käyttää useita mukautettuja toimialuenimiä, kuten 'contoso.com' ja 'contosobank.com', voit lisätä ne 900 toimialuenimien enintään. Tässä artikkelissa on samat vaiheet avulla voit lisätä kunkin toimialuenimiä.

## <a name="next-steps"></a>Seuraavat vaiheet

-   [Mukautetun toimialuenimien hallinta](active-directory-add-manage-domain-names.md)
-   [Lisätietoja domain management käsitteitä Azure AD-](active-directory-add-domain-concepts.md)
-   [Näytä yrityksesi tietoja kun käyttäjien kirjautuminen](active-directory-add-company-branding.md)
-   [Azure AD-toimialuenimien hallinta PowerShellin avulla](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)

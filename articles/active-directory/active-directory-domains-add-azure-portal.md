<properties
    pageTitle="Mukautetun toimialuenimen lisääminen Azure Active Directory-esikatselu | Microsoft Azure"
    description="Yrityksen toimialuenimien lisäämisestä Azure Active Directory ja kuinka voit vahvistaa toimialuenimeä."
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
    ms.date="10/17/2016"
    ms.author="curtand"/>

# <a name="add-a-custom-domain-name-to-azure-active-directory-preview"></a>Mukautetun toimialuenimen lisääminen Azure Active Directory-esikatselu

> [AZURE.SELECTOR]
- [Azure portal](active-directory-domains-add-azure-portal.md)
- [Azure perinteinen portal](active-directory-add-domain.md)

Koneessasi on vähintään yksi toimialuenimien, organisaatiossa käytetään käy kauppaa ja käyttäjien Kirjaudu sisään käyttämällä yrityksen toimialuenimen yrityksen verkkoon. Azure Active Directory (Azure AD) esikatselun käyttäminen, voit lisätä yrityksen toimialuenimen Azure AD. [Mikä on esikatselussa?](active-directory-preview-explainer.md) Näin voit määrittää hakemiston käyttäjien nimet, jotka ovat tuttuja muille käyttäjille, kuten ‘alice@contoso.com.’ prosessi on helppoa:

1. Mukautetun toimialuenimen lisääminen kansioon
2. Toimialueen Rekisteröintipalvelun toimialuenimen DNS sisäänkäynnin lisääminen
3. Tarkista mukautettua toimialuenimeä Azure AD-

## <a name="how-do-i-add-a-domain-name"></a>Kuinka lisään toimialuenimi?

1.  Kirjautuminen [Azure portaaliin](https://portal.azure.com) tilille, jolla on yleisen järjestelmänvalvojan kansion.

2.  Valitse **Lisää palveluja**, kirjoita **Azure Active Directory** tekstiruutuun ja paina sitten **ENTER-näppäintä**.

    ![Avaava käyttäjien hallinta](./media/active-directory-domains-add-azure-portal/user-management.png)

3. Valitse **toimialuenimien** ***kansion nimi*** -sivu.

4. Valitse **Lisää** -komennon ** *kansion nimi* - toimialuenimet** -sivu.

  ![Lisää-komennon valitseminen](./media/active-directory-domains-add-azure-portal/add-command.png)

5. Valitse **toimialuenimi** , sivu mukautetun toimialueen nimi-ruutuun "contoso.com", kuten ja valitse sitten **Lisää toimialue**. Muista lisätä .com, .net tai muiden ylimmän tason tunniste.

6. Valitse ***domainname*** (eli sivu, joka avautuu uusi toimialuenimi on otsikko)-sivu Hae DNS tapahtuman tiedot, jotka Azure AD käytetään Varmista, että organisaation omistaa mukautettua toimialuenimeä.

  ![Hae DNS-tapahtuman tiedot](./media/active-directory-domains-add-azure-portal/get-dns-info.png)

Nyt kun olet lisännyt toimialuenimi, Azure AD Tarkista, että organisaation omistaa toimialuenimi. Ennen kuin Azure AD voit suorittaa tämän tarkistuksen, sinun on lisättävä DNS-merkintä DNS-Vyöhyketiedosto toimialuenimeen. Tämä tehtävä suoritetaan toimialuenimen Rekisteröintipalvelun toimialuenimeen-sivustosta.

## <a name="add-the-dns-entry-at-the-domain-name-registrar-for-the-domain"></a>Lisää toimialuerekisteröijälle toimialueen DNS-merkintä

Jos haluat käyttää mukautettua toimialuenimeä Azure AD seuraava vaihe on päivitettävä toimialueen DNS-Vyöhyketiedosto. Näin voit varmistaa, että organisaation omistaa mukautettua toimialuenimeä Azure AD.

1.  Kirjaudu toimialueen toimialuerekisteröijälle. Jos access päivittää DNS-merkintä ei ole, pyydä käyttäjät tai ryhmät, joilla on tämän vaiheen 2 ja ilmoittaa, kun se on valmis.

2.  Päivitä toimialueen DNS-Vyöhyketiedosto lisäämällä Azure AD myöntämä DNS-merkintä. Tämän DNS-vaihtoehdon avulla Azure AD tarkistamaan sivuston omistajuutesi toimialueen. DNS-merkintä ei muuta kaikki toiminnat, kuten sähköpostin reititys tai web-kansio.

Ohjeita tämän lisääminen DNS-merkintä Lue [ohjeet DNS-määritys, jonka Suositut DNS-rekisteröintipalveluissa](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/)

## <a name="verify-the-domain-name-with-azure-ad"></a>Tarkista Azure AD toimialuenimi

Kun olet lisännyt DNS-merkintä, olet valmis vahvistamiseksi Azure AD toimialuenimeä.

Toimialuenimen voidaan varmistaa vasta, kun DNS-tietueet on välitetty. Välitys usein kestää vain sekuntia, mutta se voi viedä joskus yli. Jos vahvistus ei onnistu ensimmäistä kertaa, yritä uudelleen myöhemmin.

1.  Kirjautuminen [Azure portaaliin](https://portal.azure.com) tilille, jolla on yleisen järjestelmänvalvojan kansion.

2.  Valitse **Selaa**, kirjoita Käyttäjähallinta tekstiruutuun ja paina sitten **ENTER-näppäintä**.

    ![Avaava käyttäjien hallinta](./media/active-directory-domains-add-azure-portal/user-management.png)

3. Valitse **käyttäjien hallinta - toimialuenimet** -sivu, jonka haluat tarkistaa vahvistamaton toimialuenimi.

4. Valitse ***toimialue*** -sivu (eli sivu, joka avautuu uusi toimialuenimi on otsikko), **Tarkista** tarkistamisessa.

Nyt voit [määrittää käyttäjien nimet, jotka sisältävät mukautettua toimialuenimeä](active-directory-users-create-azure-portal.md).

## <a name="troubleshooting"></a>Vianmääritys

Jos et pysty tarkistamaan mukautettua toimialuenimeä, toimi seuraavasti. Microsoft eniten yhteinen aloittaminen ja toimi alaspäin vähintään Yleiset.

1.  **Odota tunnissa**. DNS-tietueet on leviäminen ennen Azure AD voi tarkistaa toimialueen. Tämä voi kestää yli.

2.  **Varmista, että DNS-tietue on määritetty, ja se on oikein**. Suorita tämä vaihe sivustossa toimialueen toimialuerekisteröijälle. Azure AD ei voi vahvistaa toimialuenimeä, jos DNS-merkintä ei ole DNS-Vyöhyketiedosto tai jos se ei ole tarkkaa vastinetta DNS kohteesta kyseisen Azure AD edellyttäen. Jos access toimialuerekisteröijältä toimialueen DNS-tietueiden päivittäminen ei ole, DNS-merkintä jakaminen henkilön tai ryhmän organisaatiossa, joilla on tämä käyttöoikeus ja pyytää heitä Lisää DNS-merkintä.

3.  **Poista toisen hakemistosta Azure AD-toimialuenimi**. Vain yhden kansion voi tarkistaa toimialuenimi. Jos toimialuenimi on tarkistettu aiemmin toiseen hakemistoon, se poistettava siellä, ennen kuin sitä voidaan varmistaa uusi kansio. Saat lisätietoja poistaminen toimialuenimien [hallinta mukautettuja toimialuenimiä](active-directory-domains-manage-azure-portal.md)lukeminen    

## <a name="add-more-custom-domain-names"></a>Lisää Lisää mukautettuja toimialuenimiä

Jos organisaatiosi käyttää useita mukautettuja toimialuenimiä, kuten 'contoso.com' ja 'contosobank.com', voit lisätä ne 900 toimialuenimien enintään. Tässä artikkelissa on samat vaiheet avulla voit lisätä kunkin toimialuenimiä.

## <a name="next-steps"></a>Seuraavat vaiheet

[Mukautetun toimialuenimien hallinta](active-directory-domains-manage-azure-portal.md)

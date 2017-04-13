<properties
    pageTitle="Yleistä mukautettuja toimialuenimiä Azure Active Directoryn | Microsoft Azure"
    description="Tässä artikkelissa kerrotaan käsitteellinen framework Azure Active directory federation kertakirjautumisen, mukaan lukien mukautettuja toimialuenimiä käyttämistä varten"
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

# <a name="conceptual-overview-of-custom-domain-names-in-azure-active-directory"></a>Azure Active Directoryn mukautettuja toimialuenimiä Käsitteellinen yleiskatsaus

Toimialuenimi on tärkeä osa monta directory-resurssien tunnus: se on osa käyttäjän nimi tai sähköpostiosoite käyttäjän, ryhmän, osoitteen ja voi olla osa sovelluksen tunnus URI-sovellusta. Resurssin Azure Active Directory (Azure AD) määrittää toimialuenimi, joka on jo vahvistettu kansio, joka sisältää resurssin omistaa. Yleinen järjestelmänvalvoja voi suorittaa toimialueen hallintatehtäviä Azure AD.

Azure AD-toimialuenimet ovat GUID. Yksittäisen Azure AD voidaan käyttää toimialuenimi. Jos yksi Azure AD-kansio on vahvistanut toimialuenimeä, ei ole Azure AD-kansion voit tarkistaa tai käytä samaa toimialueen nimeä.

## <a name="initial-and-custom-domain-names"></a>Alkuperäisen ja mukautettuja toimialuenimiä

Azure AD jokaisen verkkotunnus on alkuperäinen verkkotunnus tai mukautettua toimialuenimeä.

Jokaisen Azure AD sisältyy alkuperäinen verkkotunnus-lomakkeen contoso.onmicrosoft.com. Tämä kolmannen tason toimialuenimi, tässä esimerkissä "contoso.onmicrosoft.com," määritettiin, kun kansio on luotu, yleensä joka on luonut kansion järjestelmänvalvoja. Kansion alkuperäinen toimialuenimi ei voi muuttaa tai poistaa. Alkuperäinen verkkotunnus, täysin, kun taas on tarkoitettu käytetään automaattisesti käynnistyvää järjestelmä, kunnes mukautettu toimialuenimi on vahvistettu.

Useimmat tuotantoympäristössä kansio on vähintään yksi varmennettu mukautetun toimialueen, kuten "contoso.com", ja se on, että mukautettu toimialue, joka näkyy käyttäjille. Mukautetun toimialuenimen on toimialuenimi, omistaa ja käyttää organisaation, kuten "contoso.com," käyttötavat, kuten isännöinnin verkkosivusto. Tämä toimialuenimi on tuttu työntekijät, koska se on käyttäjänimi, jolla ne käyttävät kirjautua yrityksen verkkoon, voit lähettää ja hakea sähköpostin osa.

Ennen kuin sitä voidaan käyttää Azure AD-mukautettu toimialuenimi on lisännyt hakemistossa ja vahvistettu.

## <a name="verified-and-unverified-domain-names"></a>Varmennettu ja vahvistamaton toimialuenimet

Alkuperäinen verkkotunnus kansion arvioidaan implisiittisesti kuin Azure AD vahvistama. Kun järjestelmänvalvoja lisää Azure AD mukautettua toimialuenimeä, se on alun perin vahvistamaton tilaan. Azure AD ei salli vahvistamaton toimialuenimen directory resursseja. Näin varmistat, että vain yhden kansion käyttää tietyn toimialuenimeä ja organisaatio käyttää toimialuenimen todella omistaa kyseinen toimialue.

Azure AD tarkistaa toimialuenimen omistajuuden hakemalla toimialuenimen toimialueen nimi (DNS)-palvelun zone tiedoston tietyn tapahtuman. Järjestelmänvalvoja saa DNS-merkintä toimialuenimen omistajuuden vahvistamiseksi Azure AD, Azure AD etsii ja lisää siihen DNS-Vyöhyketiedosto toimialuenimeen. DNS-Vyöhyketiedosto ylläpitämä kyseisen toimialueen toimialuerekisteröijälle. Vaihe vaiheelta, miten toimialueen tarkistaminen näkyvät on artikkelissa lisätä [mukautetun toimialueen Azure AD-kansioon](active-directory-add-domain.md).

DNS-merkinnän lisääminen toimialuenimeen Vyöhyketiedosto ei vaikuta muiden toimialueen palvelujen, kuten sähköpostiviestiin tai web-kansio.

## <a name="federated-and-managed-domain-names"></a>Liitetyt ja hallitun toimialuenimet

Mukautetun toimialuenimen Azure AD voi määrittää käyttäjille liitetyt Kirjaudu uudelleen paikallisen Active Directory ja Azure AD-kokeiluversioon. Yhdistämisessä toimialueen määrittäminen edellyttää sellaisten resurssien Azure AD- ja Windows Server Active Directory-päivityksiä. Määrittäminen liitetyssä toimialueessa on täytettävä Azure AD Connect- tai PowerShellin avulla. Sisällytetyistä mukautettua toimialuetta ei voi aloittaa perinteinen Azure-portaalista. [Tässä videossa esitellään AD FS käyttäjän kirjauduttaessa sisään Azure AD Connect määrittämisestä](http://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Configuring-AD-FS-for-user-sign-in-with-Azure-AD-Connect).

Toimialueet, joita ei ole liitetty kutsutaan joskus hallitun toimialueet. Azure AD-kansion alustavaksi toimialueeksi arvioidaan implisiittisesti kuin hallitun toimialue.

## <a name="primary-domain-names"></a>Ensisijainen toimialuenimet

Kansion ensisijaisen toimialuenimen on toimialuenimi, joka on valmiiksi valittuna 'toimialueen' osiolle käyttäjänimi-oletusarvoksi, kun järjestelmänvalvoja luo uuden käyttäjän [Azure perinteinen portal](https://manage.windowsazure.com/) tai toisen portaalin, kuten Office 365-hallintaportaalissa. Kansio voi olla vain yksi ensisijainen toimialuenimi. Järjestelmänvalvoja voi muuttaa ensisijaisen toimialuenimen on jokin vahvistettu mukautetun toimialueen, joka ei ole liitetty, tai alustavaksi toimialueeksi.

## <a name="domain-names-in-azure-ad-and-other-microsoft-online-services"></a>Toimialuenimien käyttö Azure AD ja muihin Microsoft Online Services

Toimialuenimi on vahvistettava ennen kuin sitä voidaan käyttää toiseen Microsoft Online Services kuten Exchange Onlinen, SharePoint Onlinen ja Intune Azure AD. Muut palvelut edellyttävät yleensä järjestelmänvalvoja lisää DNS-tietueet, jotka liittyvät palveluun.

Azure-web-sovelluksen käyttää omaa järjestelmä toimialueen omistajuuden vahvistamiseksi. Toimialue on vahvistettava käytettäväksi Azure AD, vaikka se on aiemmin tarkistanut käytettäväksi tilauksessa, joka perustuu kyseisen Azure AD Azure web-sovelluksen. Azure web app-sovelluksessa voit käyttää toimialuenimi, joka on vahvistettu toiseen kansioon, joka suojaa web app-hakemistosta.

## <a name="managing-domain-names"></a>Toimialuenimien hallinta

Toimialueen hallintatehtäviä voidaan suorittaa perinteinen Azure-portaalista ja PowerShell. Useita tehtäviä voi viimeistellä Azure AD-kaavio-Ohjelmointirajapinnan käyttäminen (julkisen esikatselunäkymässä).

-   [Lisääminen ja tarkistaminen mukautettua toimialuenimeä](active-directory-add-domain.md)

-   [Azure perinteinen portaalissa toimialueiden hallinnassa](active-directory-add-manage-domain-names.md)

-   [Toimialuenimien hallinta Azure AD-PowerShellin avulla](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)

-   [Azure AD-kaavio-Ohjelmointirajapinnan käyttäminen käsittelemään toimialuenimiä Azure AD](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)

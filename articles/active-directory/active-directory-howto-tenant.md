<properties
    pageTitle="Azure AD-vuokraajan hankkiminen | Microsoft Azure"
    description="Miten saat Azure Active Directory-vuokraajan rekisteröiminen-sovellusten luominen."
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="terrylan"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/28/2015"
    ms.author="dastrock"/>

# <a name="how-to-get-an-azure-active-directory-tenant"></a>Azure Active Directory-vuokraajan hankkiminen

Azure Active Directory (Azure AD)- [vuokraajan](https://msdn.microsoft.com/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant) on organisaation edustajalle.  Se on erillinen esiintymä Azure AD-palvelu, joka organisaatio saa ilmoituksen ja omistaa, kun se rekisteröi Microsoftin pilvipalvelussa, kuten Azure, Microsoft Intune tai Office 365: ssä.  Kunkin Azure AD-vuokraajan on eri ja erillään muista Azure AD-alihallinnat.  

Palvelutili ovat suosikkiraporttisi yrityksen käyttäjien ja tiedon näyttämiseksi - salasanansa, käyttäjän profiilitiedot, käyttöoikeudet, ja niin edelleen.  Se sisältää myös ryhmät, sovellusten ja muita tietoja organisaatiossa ja sen suojaus.

Azure AD käyttäjät kirjautumaan sovellukseen, sinun on rekisteröitävä sovelluksen omia alihallinnassa.  Sovelluksen julkaiseminen Azure AD-vuokraajan on **täysin ilmaiseksi**.  Itse asiassa useimmat sovelluskehittäjille Luo useita alihallinnat ja vuorovaikutteisuudesta, kehitystä, sovellusten väliaikaisen ja testausta.  Organisaatioissa, joissa rekisteröitymään ja käyttää sovelluksen halutessasi voit myös ostaa käyttöoikeuksia, jos he haluavat hyödyntää lisäasetusten hakemiston ominaisuuksia.

Näkyy kuinka käymään hakeminen Azure AD-vuokraajan?  Prosessi voi olla vähän eri Jos olet:

- [On olemassa olevaan Office 365-tilaukseen](#use-an-existing-office-365-subscription)
- [On Microsoft Account liittyviä Azure olemassa olevaan tilaukseen](#use-an-msa-azure-subscription)
- [On liitetty organisaatiotilillä Azure olemassa olevaan tilaukseen](#use-an-organizational-azure-subscription)
- [On muu kuin yllä ja haluat aloittaa alusta alkaen](#start-from-scratch)

## <a name="use-an-existing-office-365-subscription"></a>Käytä olemassa olevaan Office 365-tilaukseen
Jos sinulla on Office 365-tilauksen, sinulla on jo Azure AD-vuokraajan! Voit kirjautua [Azure portal](https://portal.azure.com) O365-tilisi ja Azure AD käytön aloittaminen.

## <a name="use-an-msa-azure-subscription"></a>Käytä MSA Azure-tilaus
Jos olet aiemmin rekisteröityneet Azure-tilauksen yksittäisen Microsoft-Account, sinulla on jo liitetty!  Kun kirjaudut [Azure-portaalissa](https://portal.azure.com), voit automaattisesti kirjataan oletusarvon asiakasympäristöön. Olet maksuttomia tämän vuokraajan tarpeen mukaan -, mutta voit halutessasi luoda organisaation järjestelmänvalvojan tilillä.

Voit tehdä sen seuraavasti.  Voi myös haluat luoda uuden vuokraajan ja alihallintaan seuraavan vastaavalla menetelmällä järjestelmänvalvojan luominen.

1.  Kirjaudu sisään yksittäisiä tilisi [Azure-portaalissa](https://portal.azure.com)
2.  Siirry (löytyy vasemmasta siirtymispalkista **Lisää**palvelut)-portaalin "Azure Active Directory-kohta
3.  Sinun tulee automaattisesti kirjauduttava "Oletus hakemiston", jos et voit liikkua kansioiden napsauttamalla oikeassa yläkulmassa tilin nimeä.
4.  Valitse **Pikatehtävät** -osassa **Lisää käyttäjä**.
5.  Lisää-lomake ja anna seuraavat tiedot:

    - Nimi: (valitse sopiva arvo)
    - Käyttäjänimi: (Valitse tämä järjestelmänvalvojan käyttäjänimi)
    - Profiili: (Täytä tarvittavat arvot Etunimi, viimeisen nimi, tehtävänimike ja osasto)
    - Roolia: Yleisen järjestelmänvalvojan

6.  Lisää käyttäjä-lomakkeen suorittanut ja vastaanottoon uuden järjestelmänvalvojan tilapäisen salasanan, muista tallentaa salasana, sillä tarvitset kirjautua käyttämällä käyttöoikeuden uudelle käyttäjälle, jotta voit vaihtaa salasanan. Voit myös lähettää salasanan suoraan käyttäjän vaihtoehtoinen sähköpostitse.
7.  Valitse **Luo** , jos haluat luoda uuden käyttäjän.
8.  Tilapäinen salasana, kirjaudu sisään [https://login.microsoftonline.com](https://login.microsoftonline.com) kanssa tämän uuden käyttäjätilin ja muuta salasana pyydettäessä.


## <a name="use-an-organizational-azure-subscription"></a>Käytä organisaation Azure tilauksen
Jos olet aiemmin rekisteröityneet Azure-tilauksen organisaatiotilin, sinulla on jo liitetty!  [Azure-portaalin](https://portal.azure.com)palvelutili olisi löydät, kun siirryt "Lisää Services" ja "Azure Active Directory."  Olet maksuttomia tämän vuokraajan tarpeen mukaan. 


## <a name="start-from-scratch"></a>Aloita tyhjästä
Jos kaikki edellä mainitut tyhjänpäiväinen sinulle, ei hätää.  Käy yksinkertaisesti [https://account.windowsazure.com/organization](https://account.windowsazure.com/organization) rekisteröitävä Azure uusi organisaation kanssa.  Kun prosessi on valmis, on erittäin Omat Azure AD-vuokraajan toimialueen nimi, jonka valitsit rekisteröitymisen yhteydessä ylöspäin.  [Azure-portaalissa](https://portal.azure.com)voit etsiä alihallintaan siirtymällä "Azure Active Directory-vasemmalla olevasta NAV-ohjelmassa.

Osana prosessi, jossa Azure rekisteröityminen voit tarvitaan antamaan luottokorttitietoja.  Voit jatkaa LUOTTAMUSVÄLI - sinulla ei ole veloitetaanko sovellusten julkaiseminen Azure AD tai luoda uuden alihallinnat.

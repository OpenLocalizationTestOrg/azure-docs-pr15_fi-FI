<properties
   pageTitle="Office 365-tilauksen Azure Directoryn hallinta | Microsoft Azure"
   description="Office 365-tilauksen hakemistoon käyttämällä Azure Active Directory ja Azure perinteinen-portaalin hallinta"
   services="active-directory"
   documentationCenter=""
   authors="curtand"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/23/2016"
   ms.author="curtand"/>

# <a name="manage-the-directory-for-your-office-365-subscription-in-azure"></a>Office 365-tilauksen Azure Directoryn hallinta

Tässä artikkelissa käsitellään hallinta kansio, joka on luotu Office 365-tilauksen, Azure perinteinen-portaalissa. On oltava joko palvelun järjestelmänvalvoja tai muiden järjestelmänvalvojan Azure-tilauksen kirjautua Azure perinteinen-portaaliin. Jos et ole vielä tallentanut Azure tilaus, voit rekisteröityä [vapaa 30 päivän kokeiluversio](https://azure.microsoft.com/trial/get-started-active-directory/) tänään ja ottaa ensimmäisen cloud-ratkaisun kohdassa 5 minuuttia tämän linkin avulla. Muista käyttää Office 365: een kirjautuminen käyttämällesi työpaikan tai oppilaitoksen tilille.

Kun olet suorittanut Azure tilaus, voit Kirjaudu Azure perinteinen-portaaliin ja Azure-palvelujen. Valitse hallittavan samaan kansioon, joka todentaa Office 365-käyttäjille Active Directory-tunniste.

Jos sinulla jo on Azure-tilaus, hallitaan Lisää hakemisto on myös yksinkertaista. Esimerkiksi Esko Smith voi olla Office 365-tilauksen Contoso.com. Hän on myös Azure-tilaus, jotka hän rekisteröitynyt hänen Microsoft-tiliä käyttämällä msmith@hotmail.com. Tässä tapauksessa hän hallitsee kaksi kansioita.

  Tilauksen |  Office 365-palveluun  |  Azure
  -------------- | ------------- | -------------------------------
  Näyttönimi |  Contoso  |     Azure Active Directory (Azure AD) oletuskansio
  Toimialuenimi  |  contoso.com  | msmithhotmail.onmicrosoft.com

Hän haluaa hallinnoimaan käyttäjätietojen Contoso-kansiossa, kun hän on kirjautunut Azure hänen Microsoft-tilillä, jotta hän voi ottaa käyttöön Azure AD-ominaisuuksia, kuten monimenetelmäisen todentamisen. Seuraavassa kaaviossa voit havainnollistaa prosessin ohjeita:

![Kaavion kaksi riippumaton kansioiden hallinta](./media/active-directory-manage-o365-subscription/AAD_O365_03.png)

Tässä tapauksessa kaksi kansiot ovat riippuvaisia toisistaan.

## <a name="to-manage-two-independent-directories"></a>Voit hallita kaksi riippumaton hakemistojen selaaminen
Esko Smith hallittavan molemmat kansiot, kun hän on kirjautunut Azure kuin järjestyksessä msmith@hotmail.com, hän on tehtävä seuraavat toimet:

> [AZURE.NOTE]
> Nämä vaiheet voidaan suorittaa vain silloin, kun käyttäjä on kirjautunut sisään Microsoft-tilillä. Jos käyttäjä on kirjautunut sisään on työpaikan tai oppilaitoksen tilin, voit **käyttää kansiolla** ei ole käytettävissä. Työpaikan tai oppilaitoksen tiliä voi todentaa vain pääkansion (eli hakemiston johon työpaikan tai oppilaitoksen tili on tallennettu, ja omistavan yritys tai oppilaitos) mukaan.

1.  Kirjautuminen [Azure perinteinen portal](https://manage.windowsazure.com) kuin msmith@hotmail.com.
2.  **Uusi** > **sovelluksen services** > **Active Directory** > **Directory** > **Mukautettu luominen**.
3.  Valitse Käytä aiemmin kansiota ja valitse **olen valmis kirjauduttava nyt** -valintaruutu.
4.  Kirjaudu Azure perinteinen portaalin Contoso.onmicrosoft.com yleisen järjestelmänvalvojan (esimerkiksi msmith@contoso.com).
5.  Kun sinua pyydetään **Contoso-kansion käyttäminen Azure?**, valitse **Jatka**.
6.  Valitse **Kirjaudu**.
7.  Kirjaudu Azure perinteinen-portaaliin msmith@hotmail.com. Contoso-kansio ja oletuskansio näkyvät Active Directory-tunniste.

Näiden vaiheiden suorittamisen jälkeen msmith@hotmail.com on yleinen järjestelmänvalvoja Contoso-kansiossa.

## <a name="to-administer-resources-as-the-global-admin"></a>Voit hallita resurssien yleinen järjestelmänvalvoja
Nyt oletetaan, että Tiina Lassila tarvitsee hallita sivustot ja tietokannan resurssit, jotka liittyvät Azure tilaus msmith@hotmail.com. Ennen kuin hän voi tehdä, Esko Smith on tehtävä muita seuraavasti:

1.  Kirjautuminen [Azure perinteinen portal](https://manage.windowsazure.com) Azure-tilausta palvelun Järjestelmänvalvoja-tilillä (Tässä esimerkissä msmith@hotmail.com).
2.  Siirtää tilaukseen Contoso-kansio: **asetukset** > **tilaukset** > tilaus > **Muokkaa kansio** > Valitse **Contoso (Contoso.com)**. Osana siirron, minkä tahansa työpaikan tai oppilaitoksen tilit, jotka ovat apuyhteyshenkilöiden Tilauksen poistetaan.
3.  Lisää Tiina Lassila tilauksen kanssasi järjestelmänvalvojaksi: **asetukset** > **Järjestelmänvalvojat** > tilaus > **Lisää** > tyyppi **JohnDoe@Contoso.com**.

## <a name="next-steps"></a>Seuraavat vaiheet
Lisätietoja tilaukset ja hakemistojen suhteen Katso, [miten tilauksen on liitetty kansio](active-directory-how-subscriptions-associated-directory.md).

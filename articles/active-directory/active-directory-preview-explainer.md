<properties
    pageTitle="Azure Active Directory-esikatselu explainer | Microsoft Azure"
    description="Aihe, jossa kerrotaan, mitä eroa Azure Active Directory perinteinen portaalissa ja Azure Active Directory-esikatselu Azure-portaalissa."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/12/2016"
    ms.author="curtand"/>


# <a name="preview-of-the-azure-active-directory-management-experience-in-the-azure-portal"></a>Azure Active Directory-hallintatoimintojen Azure-portaalissa esikatselu

Azure Active Directory (Azure AD) hallintatoimintojen on esikatselu Azure-portaalissa. Voit kokeilla sitä pois kirjautumalla [Azure](https://portal.azure.com) -portaaliin Yleinen järjestelmänvalvoja, hakemistossa. Valitse Azure Active Directory services-luettelosta, jos se on näkyvissä tai valitse **Lisää palveluja** , voit tarkastella kaikkia palveluluetteloon. Et tarvitse Azure tilauksen käyttämään Azure AD-hallinnan kohdata Azure-portaalissa.


## <a name="capabilities-of-the-preview-experience"></a>Ominaisuuksien preview-versio

Preview-versio avulla voit hallita useita hakemisto-resurssit, kuten käyttäjien, ryhmät ja sovellusten sekä hakemistoasetusten Azure-portaalissa. Olemme ovat parantaminen Tämä ongelma koskemaan kaikkia ominaisuuksia, jotka ovat olemassa Azure AD-hallinnan kohdata [Azure perinteinen portal](https://manage.windowsazure.com). Vasta sitten on joitakin tehtäviä, jotka on edelleen loppuun perinteinen portaalissa Kansionhallinta.

## <a name="manage-the-same-azure-ad-tenants"></a>Saman Azure AD-omistajien hallinta

Preview-versio lukee ja kirjoittaa samaa Azure Active Directory-vuokraajan perinteinen portaalin ja Office 365-hallintakeskukseen. Jokin näistä portaaleihin tehdyt muutokset näkyvät kaikilla.

## <a name="use-the-same-authorization-logic"></a>Käytä samaa luvan logiikka

Preview-versio käyttää samaa luvan logiikan luodun Active Directory-asiakkaiden. Käyttäjien on oikeus tehdä muutoksia directory-resursseja directory rooli, kuten Yleinen järjestelmänvalvoja, käyttäjien järjestelmänvalvoja, salasanajärjestelmänvalvoja perusteella. Ottaa roolin Azure resursseja tai Azure tilaus ei määritä käyttäjä voi hallita directory resursseja. Lisätietoja Azure AD hallinta roolit artikkelissa [järjestelmänvalvojan roolien määrittäminen Azure Active Directoryn](active-directory-assign-admin-roles.md). 

Preview-versio on tarkoitettu Yleiset järjestelmänvalvojat. Jos käytät preview-versio samalla, kun kirjautunut sisään käyttäjänä, joka ei ole yleisenä järjestelmänvalvojana, joudut ehkä eivät toimi oikein kokemusta. Voit esimerkiksi ehkä, valitse painike, jolla voit aloittaa tehtävän, et saa suoritettua hakemistossa. Olemme ovat parantaminen Tämä ongelma pian.
 
## <a name="tell-us-what-you-think"></a>Kerro mielipiteesi

Voit antaa palautetta [Azure AD palautetta keskustelupalstalla](https://social.msdn.microsoft.com/Forums/home?forum=WindowsAzureAD&filter=alltypes&sort=lastpostdesc)portaalin järjestelmänvalvoja-osassa preview-versio.

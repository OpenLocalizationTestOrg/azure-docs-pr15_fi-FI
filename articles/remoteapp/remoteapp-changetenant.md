
<properties
    pageTitle="Muuta Azure RemoteApp Azure Active Directory-vuokraajan | Microsoft Azure"
    description="Opi muuttamaan Azure Active Directory-vuokraajan liittyvät Azure RemoteApp"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="change-the-azure-active-directory-tenant-in-azure-remoteapp"></a>Azure Active Directory-vuokraajan Azure RemoteApp muuttaminen

> [AZURE.IMPORTANT]
> Azure RemoteApp on on lopetettu. Lue lisätietoja [ilmoitus](https://go.microsoft.com/fwlink/?linkid=821148) .

Azure RemoteApp käyttää Azure Active Directory (Azure AD) Salli käyttöoikeus. Vain Azure AD-vuokraajan, jota voit käyttää Azure RemoteApp näkyy Azure-tilaukseen liittyvää. Voit tarkastella liittyvän tilauksen **portaalin asetussivulla** . Tarkista **Hakemisto** -sarakkeeseen **tilaukset** -välilehdessä.

> [AZURE.NOTE] Tämän muutoksen onnistu, poista ensin kaikki käyttäjät aiemmin Azure Active Directory-vuokraajan kaikkien Azure RemoteApp peräisin. Voit tehdä tämän Siirry Azure-portaaliin, siirry **Azure RemoteApp** -välilehteen ja avaa jokaisen Azure RemoteApp-sivustokokoelman. Siirry **käyttäjät** -välilehti ja Poista käyttäjät, jotka kuuluvat nykyisen Azure Active Directory-vuokraajan. Toista kaikki olemassa olevat Azure RemoteApp sivustokokoelmat. Ilman näin ei ole osaat luoda tai korjaustiedoston sivustokokoelmat.

Jos haluat käyttää eri palvelutili, muuta tilauksen liitosta näiden ohjeiden avulla:

1. Valitse-portaalissa poistaa Azure AD käyttäjiä, johon olet antanut Azure RemoteApp sivustokokoelmat access. (Katso yllä Huomautus ohjeita siitä, miten voit tehdä tämän.)


2. Määritä Microsoft-tilillä (aiemmin Live ID) palvelun järjestelmänvalvoja. (Tiedä, jos olet jo palvelujen järjestelmänvalvoja? Voit tarkistaa, valitsemalla **Asetukset -> järjestelmänvalvojat**.) Nyt näin siitä, miten voit muuttaa:
    1. Valitse oikeassa yläkulmassa käyttäjä ja valitse sitten **laskun tarkasteleminen**.
    2. Valitse tilaus. Valitse uusi-sivun alaspäin ja valitse **Muokkaa tilauksen tiedot** oikeassa. (Tyyppinen Keskimmäinen alas, oikealle, jos löydät sen avulla.)
    3. Kirjoita Microsoft-tiliä käyttäjälle, joka on oltava palvelun järjestelmänvalvoja.

3. Nyt Kirjaudu ulos portaalin ja kirjaudu sitten takaisin sisään Microsoft-tili, joka on määritetty edellisessä vaiheessa.


4. Valitse **Uusi sovellus -> palvelut -> Active Directory -> kansion -> mukautettu luominen**.
5. Valitse **kansio**, **Käytä kansiolla**. Tutustutaan on kirjaudut ulos portaalin nyt, joten valitse **olen valmis kirjauduttava nyt**.
6. Kirjaudu takaisin portaalin Yleinen järjestelmänvalvoja, johon haluat lisätä hakemiston. (Jos sinulla ei ole jo Yleinen järjestelmänvalvoja, sinun on jälkeen PYÖRISTÄ-funktiota Kirjaudu sisään ja kirjaudu sitten ulos.)
7. Sinua pyydetään kirjautuessasi sisään haluat käyttää aiemmin luotuja AD-vuokraajan tilaus. Valitse **Jatka**ja valitse sitten **Kirjaudu ulos**.
5. Kirjaudu takaisin sisään uudelleen ja palaa **Asetukset -> tilaukset**. Valitse tilaus ja valitse sitten **Muokkaa hakemisto**. Valitse Azure AD-vuokraajan, jota haluat käyttää.



Voit nyt käyttää uuden Azure AD vuokraajan Azure tilauksen käyttöoikeuksien hallitseminen ja määritä käyttöoikeudet Azure RemoteApp.

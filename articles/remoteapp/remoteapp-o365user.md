
<properties 
    pageTitle="Voit käyttää Office 365-käyttäjätilit Azure RemoteApp | Microsoft Azure"
    description="Opettele käyttämään Office 365-käyttäjätilien Azure RemoteApp"
    services="remoteapp"
    documentationCenter="" 
    authors="piotrci" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="how-to-use-azure-remoteapp-with-office-365-user-accounts"></a>Azure RemoteApp käyttäminen Office 365-käyttäjätilit

> [AZURE.IMPORTANT]
> Azure RemoteApp on on lopetettu. Lue lisätietoja [ilmoitus](https://go.microsoft.com/fwlink/?linkid=821148) .

Jos sinulla on Office 365-tilaus on Azure Active Directory, joka tallentaa käyttäjänimet ja salasanat käyttää Office 365-palveluissa. Esimerkiksi kun käyttäjien käyttöön Office 365 ProPlus ne todennetaan Azure AD Tarkista käyttöoikeudet. Useimmat asiakkaat haluat käyttää Azure RemoteApp samassa kansiossa.

Jos otat Azure RemoteApp käytössäsi on todennäköisesti Azure tilauksen, joka on liitetty eri Azure AD. Jotta voit käyttää Office 365-kansio on Siirry Azure tilauksen kyseiseen kansioon.

Lisätietoja Office 365-asiakassovellusten ottamisesta käyttöön on artikkelissa [Azure RemoteApp Office 365-tilauksen käyttäminen](remoteapp-officesubscription.md).
 
## <a name="phase-1-register-your-free-office-365-azure-active-directory-subscription"></a>Vaihe 1: Rekisteröityä maksuttoman Office 365: n Azure Active Directory-tilauksen
Jos käytössäsi on Azure perinteinen portal-ohjeiden mukaisesti [rekisteröityä maksuttoman Azure Active Directory-tilauksen](https://technet.microsoft.com/library/dn832618.aspx) voit käyttää järjestelmänvalvojan oman Azure AD Azure hallinta-portaalin kautta. Tämä toimenpide tuloksena on oltava Kirjaudu Azure portaalin ja nähdä hakemistossa siellä – tässä vaiheessa ei ole näkyvissä, paljon käytät Azure RemoteApp koko Azure tilaus on toiseen kansioon.

Muista nimi ja järjestelmänvalvojan tilin tässä vaiheessa luotu salasana – ne tarvitaan vaiheessa 2.

Jos käytät Azure portaalin, tarkista, [kuinka voit rekisteröityä ja aktivoida vapaa Azure Active Directory Office 365-portaalissa](http://azureblogger.com/2016/01/how-to-register-and-activate-a-free-azure-active-directory-using-office-365-portal/).

## <a name="phase-2-change-the-azure-ad-associated-with-your-azure-subscription"></a>Vaihe 2: Muuta Azure AD Azure-tilaukseen liittyvää.
Tarkastellaan Azure tilauksen muuttamiseen sen nykyisen hakemiston Office 365-kansioon on tutun vaihe 1.

Noudata [Muuta Azure RemoteApp Azure Active Directory-vuokraajan](remoteapp-changetenant.md)kuvattu. Kiinnitä erityistä huomiota seuraavasti:

- Vaihe #1: Jos olet ottanut Azure RemoteApp (ARA) tätä tilausta, muista poistaa kaikki Azure AD-käyttäjätilit ARA minkä tahansa peräisin ensin, ennen kuin yrität mihinkään muuhun. Voit vaihtoehtoisesti kannattaa poistaa kaikki olemassa olevat sivustokokoelmat.
- Vaihe #2: Tämä on tärkeä vaihe. Sinun on käytettävä Microsoft-tilillä (kuten @outlook.com) palvelun järjestelmänvalvojana tilauksen; Tämä johtuu siitä, emme voi olla kaikki käyttäjätilit, kun tilaus – liitetty aiemmin Azure AD-Tällaisissa tilanteissa emme voi siirtää sen eri Azure AD.
- Vaihe #4: Kun lisäät aiemmin luodun kansion, järjestelmä kysyy kirjautumaan sisään järjestelmänvalvojatilin kansion. Varmista, että järjestelmänvalvojatilin vaiheen 1.
- Vaihe #5: Muuttaminen tilauksen Yläkansiota Office 365-kansioon. Lopputulos on oltava, valitse Asetukset -> tilaukset-tilauksesi on lueteltu Office 365-kansio. 
![Muuta tilauksen pääkansio](./media/remoteapp-o365user/settings.png)
 

Tässä vaiheessa Azure RemoteApp-tilauksesi on liitetty Office 365: n Azure AD; Voit käyttää aiemmin Office 365-käyttäjätilit Azure RemoteApp!





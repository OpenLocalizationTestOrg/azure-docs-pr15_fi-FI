<properties
   pageTitle="Vianmääritys: "Active Directory-kohde on puuttuu tai ei ole käytettävissä | Microsoft Azure "
   description="Mitä tehdään, jos Active Directory-valikon vaihtoehto ei ole Azure hallinta-portaalin."
   services="active-directory"
   documentationCenter="na"
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin"/>

# <a name="troubleshooting-active-directory-item-is-missing-or-not-available"></a>Vianmääritys: "Active Directory-kohde on puuttuu tai ei ole käytettävissä

Monia Azure Active Directory-ominaisuuksia ja palveluita käyttöohjeet alussa on "Siirry Azure hallinta-portaalin ja valitse **Active Directory**." Mutta Mitä teen, jos Active Directory-tunniste tai valikon kohde ei näy, tai jos se on merkitty **Ei ole käytettävissä**? Tässä aiheessa on suunniteltu helpottamaan. Se kuvaa kohdassa **Active Directory** ei näy tai ei ole käytettävissä ja kerrotaan, miten se toimii.

## <a name="active-directory-is-missing"></a>Active Directory puuttuu

Yleensä **Active Directory** -kohteen näkyvät vasemmanpuoleisessa siirtymisruudussa. Azure Active Directory-menettelyt ohjeita oletetaan, että tämä kohde on näkymässä.

![Näyttökuva: Active Directory Azure](./media/active-directory-troubleshooting/typical-view.png)

Active Directory-kohde näkyy vasemmanpuoleisessa siirtymisruudussa, kun jokin seuraavista ehdoista täyttyy. Muussa tapauksessa kohde ei ole näkyvissä.

* Nykyinen käyttäjä on kirjautunut Microsoft-tiliä (aikaisemmin Windows Live ID).

    TAI

* Azure vuokraajan on kansio, ja nykyisen tilin directory-järjestelmänvalvoja on.

    TAI

* Azure vuokraajan on oltava vähintään yksi Azure AD-käytönvalvonta (ACS) nimitila. Lisätietoja on artikkelissa [Access-ohjausobjektin Namespace](https://msdn.microsoft.com/library/azure/gg185908.aspx).

    TAI

* Azure vuokraajan on vähintään yksi Azure Monimenetelmäisen todentamisen toimittaja. Lisätietoja on artikkelissa [Azure multi-factor käyttöoikeustarkistuspalvelun hallinta](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).

Voit luoda käyttöoikeuksien valvonta-nimitila tai Monimenetelmäisen todentamisen palveluntarjoaja valitsemalla **+ Uusi** > **Sovelluksen Services** > **Active Directorysta**.

Saat järjestelmänvalvojan oikeudet kansioon-on järjestelmänvalvoja määrittää järjestelmänvalvojan rooleja tiliisi. Lisätietoja on artikkelissa [järjestelmänvalvojan roolien määrittäminen](active-directory-assign-admin-roles.md).

## <a name="active-directory-is-not-available"></a>Active Directory ei ole käytettävissä

Kun napsautat **+ Uusi** > **Sovelluksen Services**, **Active Directory** -kohde näkyy. Tarkemmin sanottuna Active Directory-kohteen tulee näkyviin, kun jokin Active Directory-ominaisuudet, kuten hakemiston, käyttöoikeuksien valvonta tai multi-factor Auth palvelu ei ole nykyisen käyttäjän käytössä.

Sivun ladattaessa kohde näkyy himmennettynä ja on merkitty **Ei ole käytettävissä**. Tämä on väliaikainen tila. Odota hetki, jos kohde on käytettävissä. Jos viive pidentää, verkkosivun usein päivittäminen ratkaisee ongelman.

![Näyttökuva: Active Directory ei ole käytettävissä](./media/active-directory-troubleshooting/not-available.png)

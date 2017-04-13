<properties 
    pageTitle="Paikallisen Active Directory Azure-sovelluksen todentamismenetelmä | Microsoft Azure" 
    description="Lisätietoja eri asetuksista liiketoiminta-sovellusten Azure-sovelluksen palvelun tarkistamiseen paikallisen Active Directory-hakemistosta" 
    services="app-service" 
    documentationCenter="" 
    authors="cephalin" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="web" 
    ms.date="08/31/2016" 
    ms.author="cephalin"/>

# <a name="authenticate-with-on-premises-active-directory-in-your-azure-app"></a>Todentamismenetelmä paikallisen Active Directory Azure-sovelluksessa #

Tässä artikkelissa kerrotaan, miten todentamismenetelmä paikallisen Active Directory (AD) [Azure sovelluksen](../app-service/app-service-value-prop-what-is.md)-palvelussa. Azure sovelluksen nykyisessä pilveen, mutta sillä on tapoja todennetaan paikallisen AD-käyttäjiä suojatusti. 

## <a name="authenticate-through-azure-active-directory"></a>Azure Active Directory-todennus
Azure Active Directory-vuokraajan voi olla directory synkronoitu paikallisen kanssa AD. Tämän menetelmän avulla AD käyttäjät voivat käyttää sovelluksen Internetistä ja todennuksen paikallisen tunnistetietoja. Lisäksi Azure App palvelun tarjoaa [tätä menetelmää ratkaisu Poista-näppäintä](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md). Muutamalla hiiren napsautuksella voit ottaa directory synkronoitu vuokraajan todentaminen Azure-sovelluksen. Tämän menetelmän on seuraavia etuja:

-   Ei edellytä mitään sovelluksen todennuskoodi. Anna sovelluksen palvelun todennus voit tehdä ja aikaa antamalla sovelluksen toimintoja.
-   [Azure AD Graph API](http://msdn.microsoft.com/library/azure/hh974476.aspx) mahdollistaa käytön hakemiston tietojen Azure-sovellukset.
-   Sisältää [kaikki tukemat Azure Active Directory-sovellukset](/marketplace/active-directory/), myös Office 365: ssä, Dynamics CRM Online, Microsoft Intune ja tuhansia Microsoftin-sovellusten SSO. 
-   Azure Active Directory tukee Roolipohjainen käyttöoikeuksien valvonta. Voit [Authorize(Roles="X")]-kuvion koodisi mahdollisimman vähän muutokset.

Katso, miten voit kirjoittaa Azure liiketoiminta-sovellus, joka todentaa Azure Active Directory-kohdassa [luominen liiketoiminta - Azure app Azure Active Directory-todennuksen kanssa](web-sites-dotnet-lob-application-azure-ad.md).

## <a name="authenticate-through-an-on-premises-sts"></a>Todentaa paikallisen STS kautta
Jos sinulla on paikallisen suojaaminen tunnuksen palvelun (STS) kuten Active Directory Federation Services (AD FS), voit tehdä, järjestäjäorganisaatiota todennus Azure-sovelluksen. Tämä vaihtoehto on paras, kun yrityksen käytäntö estää AD-tiedot tallennetaan Azure. Huomaa kuitenkin seuraavasti:

-   STS topologian on oltava käyttöön paikallisen, kustannus- ja katseltavan kanssa.
-   Vain AD FS-järjestelmänvalvojat voivat määrittää [varmenteen käyttäjän osapuolen luottamussuhteet ja varaa säännöt](http://technet.microsoft.com/library/dd807108.aspx), jotka voivat rajoittaa kehittäjän asetukset. Toisaalta on mahdollista hallita ja mukauttaa [saatavat](http://technet.microsoft.com/library/ee913571.aspx) sovelluksen kohti välein.
-   Accessin paikallisen AD tietojen vaatii erillisen ratkaisun yrityksen palomuurin läpi.

Viestin kirjoittaminen Azure liiketoiminta-sovellus, joka todentaa paikallisen STS kanssa on ohjeartikkelissa [Liiketoiminta - Azure sovelluksen AD FS-todennuksen luominen](web-sites-dotnet-lob-application-adfs.md).
 

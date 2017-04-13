<properties
    pageTitle="Azure Active Directory-itse sovelluksen access hallinnan määrityksen | Microsoft Azure"
    description="Omatoiminen ryhmän hallinta avulla käyttäjät voivat luoda ja hallita käyttöoikeusryhmät tai Office 365-ryhmien Azure Active Directoryn ja tarjoaa käyttäjille mahdollisuuden pyynnön käyttöoikeusryhmän tai Office 365-ryhmän jäsenyys"
    services="active-directory"
    documentationCenter=""
  authors="curtand"
    manager="femila"
    editor=""
    />

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/10/2016"
    ms.author="curtand"/>

# <a name="setting-up-azure-active-directory-for-self-service-group-management"></a>Azure Active Directory määrittäminen Omatoiminen ryhmän hallinta

Omatoiminen ryhmän hallinnan avulla käyttäjät voivat luoda ja hallita käyttöoikeusryhmät tai Office 365-ryhmien Azure Active Directory (Azure AD). Käyttäjät voivat myös pyytää käyttöoikeusryhmän tai Office 365-ryhmäjäsenyyksiä ja sitten ryhmän omistaja voi hyväksyä tai hylätä jäsenyyttä. Tällä tavalla päivittäinen Ryhmäjäsenyyden hallinta voi delegoida henkilöille, jotka ymmärtää, jäsenyyden business kontekstissa. Omatoiminen ryhmän hallintatoiminnot ovat käytettävissä vain käyttöoikeusryhmiä ja Office 365-ryhmiä, muttei sähköpostia käyttävät käyttöoikeusryhmät ja jakeluluettelot.

Omatoiminen ryhmän hallinta tällä hetkellä koostuu kahdesta yleisintä tapausta: valtuutetun ryhmän hallinnointi ja Omatoiminen ryhmän hallinta.

- **Valtuutetun hallinnan ryhmän** 
   on esimerkiksi SaaS sovellus, jossa on käytössä yrityksen käyttöoikeuksien hallinta on ottava järjestelmänvalvoja. Näiden käyttöoikeuksien hallinta on jatkossa hankalaa, jotta tämä järjestelmänvalvojan pyytää business omistajaa uuden ryhmän luominen. Järjestelmänvalvoja määrittää access-sovelluksen uuteen ryhmään, ja lisää ryhmään kaikille henkilöille, jotka on jo sovelluksen käyttämiseen. Yrityksen omistajan jälkeen voit lisätä käyttäjiä ja nämä käyttäjät automaattisesti valmistelun yhteydessä sovellukseen. Yrityksen omistajan ei tarvitse odottaa, järjestelmänvalvoja voi hallita käyttäjien. Jos järjestelmänvalvoja antaa oikeuksien eri business-ryhmän esimiehen sitten kyseisen henkilön hallita omia käyttäjien. Ole business omistaja tai projektipäällikkö voi tarkastella tai toistensa käyttäjien hallinta. Järjestelmänvalvoja voivat edelleen nähdä kaikki käyttäjät, jotka voivat käyttää sovelluksen ja estä käyttöoikeuksia tarvittaessa.

- **Omatoiminen ryhmän hallinta** 
   Esimerkki tässä skenaariossa on kaksi käyttäjillä molemmat on SharePoint Online-sivustoissa, ne määrittäminen erikseen. Käyttäjät haluavat toistensa ryhmiä käyttöoikeuden antaminen sivustoihinsa. Tämän vuoksi he voivat luoda ryhmä Azure AD- ja SharePoint Onlinen kullekin niistä valitsee antamaan niiden sivustojen käyttöoikeuden myöntäminen ryhmälle. Kun joku haluaa access-ne pyytää sen Access-ruutu ja hyväksymisen jälkeen he saavat sekä SharePoint Online-sivustojen käyttäminen automaattisesti. Myöhemmin jonkin niistä päättää, että kaikki sivuston käyttäjät saavat myös tietyn SaaS-sovelluksen pääsyn. SaaS sovelluksen järjestelmänvalvoja voi lisätä sovelluksen SharePoint Online-sivuston käyttöoikeudet. Tämän jälkeen pyyntöihin, jotka haluat hyväksyä antaa käyttöoikeuden kaksi SharePoint Online-sivustojen ja SaaS-sovellukselle.

## <a name="making-a-group-available-for-end-user-self-service"></a>Saataville ryhmän Lopeta käyttäjän Omatoiminen

1. [Azure perinteinen portal](https://manage.windowsazure.com)Avaa Azure AD-kansio.

2. Määritä **Määritä** -välilehdessä **valtuutettu ryhmän hallinta** käytössä.

3. **Käyttäjät voivat luoda käyttöoikeusryhmät** tai **käyttäjät voivat luoda Office-ryhmien** asetukseksi käytössä.

Kun **käyttäjät voivat luoda käyttöoikeusryhmät** on käytössä, hakemistossa kaikki käyttäjät voivat luoda uusi SharePoint-ryhmät ja lisää jäseniä ryhmiin. Nämä uusia ryhmiä myös näkyvät muille käyttäjille Access-paneeli. Jos ryhmä käytäntöasetus sallii sen, muut käyttäjät voivat luoda liittymisen ryhmiin pyynnöt. Jos **käyttäjät voivat luoda käyttöoikeusryhmät** on poistettu käytöstä, käyttäjät voi luoda ryhmiä ja olemassa olevia ryhmiä, jossa ne ovat omistaja voi muuttaa. Kuitenkin he voivat edelleen ryhmien jäsenyyksien hallinta ja hyväksy liittymään niiden ryhmien pyynnöt muilta käyttäjiltä.

Voit käyttää myös **käyttäjille, kuka voi käyttää käyttöoikeusryhmät itsepalvelu** käyttäjien Omatoiminen ryhmän hallinta Lisää tarkasti rajattuja access-päättää saavuttamiseksi. Kun **käyttäjät voivat luoda ryhmiä** on käytössä, hakemistossa kaikki käyttäjät voivat luoda uusia ryhmiä ja lisää jäseniä ryhmiin. Voit määrittää **käyttäjät, kuka voi käyttää käyttöoikeusryhmät itsepalvelu** myös joitakin, olet rajoittavat vain rajoitettu ryhmään käyttäjien hallinta. Kun tätä valitsinta määritetään joissakin, sinun on lisättävä ryhmään SSGMSecurityGroupsUsers käyttäjät ennen kuin ne voi luoda uusia ryhmiä ja lisätä jäseniä. Määrittämällä kaikille **käyttäjille, kuka voi käyttää käyttöoikeusryhmät itsepalvelu** käyttöön kaikille käyttäjille, kun luot uusia ryhmiä hakemistossa.

Voit myös määrittää mukautetun ryhmän, jonka jäsenet voivat käyttää itsepalvelu nimen **, jotka käyttävät käyttöoikeusryhmät itsepalvelu ryhmän** kehyksen.

## <a name="additional-information"></a>Lisätietoja

Seuraavissa artikkeleissa on lisätietoja Azure Active Directory.

* [Resurssien käytön hallinta ja Azure Active Directory-ryhmä](active-directory-manage-groups.md)

* [Ryhmän asetusten määrittämiseen Azure Active Directory cmdlet-komennot](active-directory-accessmanagement-groups-settings-cmdlets.md)

* [Artikkelissa indeksin Azure Active Directory-sovellusten hallinta](active-directory-apps-index.md)

* [Azure Active Directory-ominaisuudet](active-directory-whatis.md)

* [Azure Active Directory-integraation paikallisen käyttäjätietoja](active-directory-aadconnect.md)

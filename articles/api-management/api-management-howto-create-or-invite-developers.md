<properties 
    pageTitle="Miten hallita käyttäjätilejä Azure API hallinta | Microsoft Azure" 
    description="Opettele luominen tai kutsua käyttäjiä Azure API hallinta" 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-manage-user-accounts-in-azure-api-management"></a>Voit hallita käyttäjätilejä Azure API hallinta

Kehittäjät ovat API hallinta-ohjelmointirajapinnan, voit näyttää API hallinnan avulla käyttäjät. Tämän oppaan näkyy luomisesta ja kutsu kehittäjät ohjelmointirajapinnan ja tuotteiden että Soita käytettävissä API hallinta-esiintymää. Lisätietoja Käyttäjätilien hallinta ohjelmallisesti [API hallinta muiden](https://msdn.microsoft.com/library/azure/dn776326.aspx) viittauksen ohjeessa [käyttäjän kohteen](https://msdn.microsoft.com/library/azure/dn776330.aspx) .

## <a name="create-developer"> </a>Luo uusi developer

Voit luoda uuden developer valitsemalla Azure perinteinen portaalissa API Management-palvelun **hallinta** . Tämä avaa API hallinta publisher-portaalin. Jos et ole luonut erillisen service API hallinta-kohdassa [Azure API hallinnan käytön aloittaminen][] -opetusohjelma [Luo erillisen service API hallinta][] .

![Publisher-portaalissa][api-management-management-console]

Valitse vasemmalla **API hallinta** -valikosta **käyttäjät** ja valitse sitten **Lisää käyttäjä**.

![Luo developer][api-management-create-developer]

Anna **sähköpostiosoite**, **salasana**ja **nimi** uusi kehittäjille ja valitse **Tallenna**.

![Luo developer][api-management-add-new-user]

Oletusarvon mukaan juuri luomasi Kehitystyökalut-tilit ovat **käytössä**ja **kehittäjät** ryhmään liittyvien.

![Uusi developer][api-management-new-developer]

Kehitystyökalut-tilit, jotka ovat **active** tilaan voidaan käyttää kaikki, joihin heillä on tilaukset API. Voit liittää juuri luomasi Kehitystyökalut ja muut ryhmät, katso, [miten voit liittää ryhmissä, joissa on kehittäjät][].

## <a name="invite-developer"> </a>Kutsu kehittäjä

Kutsu kehittäjä, valitse **käyttäjien** vasemmalla **API hallinta** -valikosta ja valitse sitten **Kutsu käyttäjä**.

![Kutsu developer][api-management-invite-developer]

Kirjoita nimesi ja sähköpostiosoitteesi kehittäjä osoite ja valitse **Kutsu**.

![Kutsu developer][api-management-invite-developer-window]

Näkyviin tulee vahvistussanoma, mutta äskettäin kutsuttu developer ei näy luettelossa, Hyväksyttyään kutsun. 

![Kutsu vahvistus][api-management-invite-developer-confirmation]

Kun kehittäjä on kutsuttu, kehittäjän lähettää sähköpostiviestin. Tämä sähköpostin luodaan mallin avulla ja mukautettavissa. Lisätietoja on artikkelissa [Configure sähköpostimalleja][].

Kun kutsu on hyväksytty, tili tulee aktiivinen.

## <a name="block-developer"></a> Aktivointi tai reactivate developer-tili

Oletusarvon mukaan äskettäin kutsuttu tai luotuja Kehitystyökalut-tilit ovat **aktiivisia**. Aktivoinnin Kehitystyökalut-tilin, valitse **Estä**. Aktivoi estettyjen Kehitystyökalut-tilin, valitse **Aktivoi**. Estettyjen Kehitystyökalut-tiliä ei voit käyttää developer-portaalissa tai minkä tahansa ohjelmointirajapinnan kutsu. Poista käyttäjätili valitsemalla **Poista**.

![Torju developer][api-management-new-developer]

## <a name="reset-a-user-password"></a>Käyttäjän salasanan palauttaminen

Jos haluat palauttaa käyttäjätilin salasanan, napsauta tilin nimeä.

![Salasanan vaihtaminen][api-management-view-developer]

Valitse Lähetä linkki hänen salasanansa on vaihdettu käyttäjän **salasanan** .

![Salasanan vaihtaminen][api-management-reset-password]

Voit käsitellä ohjelmallisesti käyttäjätilit, [API hallinta muiden](https://msdn.microsoft.com/library/azure/dn776326.aspx) viittauksen ohjeissa [käyttäjän kohteen](https://msdn.microsoft.com/library/azure/dn776330.aspx) . Jos haluat palauttaa käyttäjätilin salasanan tietyn arvon, voit [päivittää käyttäjän](https://msdn.microsoft.com/library/azure/dn776330.aspx#UpdateUser) -toimintoa ja määritä haluamasi salasana.

## <a name="pending-verification"></a>Odottaa vahvistusta varten

![Odottaa vahvistusta varten][api-management-pending-verification]

## <a name="next-steps"> </a>Seuraavat vaiheet

Kun Kehitystyökalut-tili on luotu, voit liittää rooleja ja tilaa tuotteita ja ohjelmointirajapinnan. Lisätietoja on artikkelissa [miten voit luoda ja käyttää ryhmiä][].


[api-management-management-console]: ./media/api-management-howto-create-or-invite-developers/api-management-management-console.png
[api-management-add-new-user]: ./media/api-management-howto-create-or-invite-developers/api-management-add-new-user.png
[api-management-create-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-create-developer.png
[api-management-invite-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer.png
[api-management-new-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-new-developer.png
[api-management-invite-developer-window]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer-window.png
[api-management-invite-developer-confirmation]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer-confirmation.png
[api-management-pending-verification]: ./media/api-management-howto-create-or-invite-developers/api-management-pending-verification.png
[api-management-view-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-view-developer.png
[api-management-reset-password]: ./media/api-management-howto-create-or-invite-developers/api-management-reset-password.png
[]: ./media/api-management-howto-create-or-invite-developers/.png



[Create a new developer]: #create-developer
[Invite a developer]: #invite-developer
[Deactivate or reactivate a developer account]: #block-developer
[Next steps]: #next-steps
[Voit luoda ja käyttää ryhmiä]: api-management-howto-create-groups.md
[Miten ryhmät liitetään kehittäjät]: api-management-howto-create-groups.md#associate-group-developer

[Azure API hallinnan käytön aloittaminen]: api-management-get-started.md
[Luo erillisen service API hallinta]: api-management-get-started.md#create-service-instance
[Määritä sähköpostimalleja]: api-management-howto-configure-notifications.md#email-templates
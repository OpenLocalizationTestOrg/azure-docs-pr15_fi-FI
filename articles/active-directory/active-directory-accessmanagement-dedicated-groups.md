<properties
    pageTitle="Varattu Azure Active Directory-ryhmien | Microsoft Azure"
    description="Miten oma ryhmien yleiskuva toimi Azure Active Directory- ja miten niitä luodaan."
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
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="curtand"/>

# <a name="dedicated-groups-in-azure-active-directory"></a>Erillinen Azure Active Directory-ryhmien

Azure Active Directoryn (Azure AD) erillinen ryhmissä-ominaisuuden automaattisesti Luo ja täyttää ennalta Azure AD-ryhmien jäsenyyden. Erillinen ryhmissä ei voi lisätä tai poistaa käyttämällä Azure perinteinen portal, Windows PowerShellin cmdlet-komennot tai ohjelmallisesti.

>[AZURE.NOTE] Erillinen ryhmät edellyttää, että Azure AD Premium-käyttöoikeus on määritetty
>- järjestelmänvalvoja, joka hallitsee sääntö-ryhmä
>- kaikki käyttäjät, jotka ovat käytössä sääntö on ryhmän jäsen

**Jos haluat ottaa käyttöön erillinen ryhmät**

1. [Azure perinteinen portal](https://manage.windowsazure.com)Valitse **Active Directory**ja avaa sitten organisaatiosi hakemistoon.

2. Valitse **ryhmät** -välilehti ja avaa sitten ryhmän, jota haluat muokata.

3. Valitse **määritys** -välilehti ja aseta arvoksi **Kyllä** **Käyttöön varattu ryhmät** .

Kun käyttöön erillinen ryhmien valitsin on määritetty **Kyllä**, voit edelleen ottaa kansio luodaan automaattisesti määrittämällä kaikille käyttäjille Oma ryhmä **"kaikki käyttäjät-ryhmän** Siirry **Kyllä**. Voit sitten myös muokata erillinen ryhmän nimen kirjoittamisen **Näyttönimi-kaikille käyttäjille-ryhmän** kentän.

Kaikki käyttäjät-ryhmän voidaan määrittää samat käyttöoikeudet kaikille käyttäjille hakemistossa. Esimerkiksi voit myöntää kaikille käyttäjille directory Access SaaS sovellukseen erillinen kaikki käyttäjät-ryhmän käyttöoikeuksien määrittämisen tämän sovelluksen.

Erillinen kaikki käyttäjät-ryhmä sisältää kaikki käyttäjät kansioon, mukaan lukien vieraiden ja Ulkoiset käyttäjät. Jos tarvitset ryhmän, joka sulkee pois Ulkoiset käyttäjät ja sitten voit tehdä tämän luomalla ryhmän määrite perustuvan dynaamisen säännöllä esimerkiksi seuraavat:

                (user.userPrincipalName -notContains "#EXT#@")

Ryhmän kaikki vieraiden pois jättävän Käytä sääntöä, kuten jokin seuraavista:

                (user.userType -ne "Guest")

Lisätietoja dynaaminen Ryhmäjäsenyyden *lisäsääntöjä (säännöt, jotka voivat sisältää useita vertailuja)* luomisesta on artikkelissa [käyttäminen määritteitä, jos haluat luoda Monitasoisempia sääntöjä](active-directory-accessmanagement-groups-with-advanced-rules.md).


Seuraavissa artikkeleissa on lisätietoja Azure Active Directory.

* [Resurssien käytön hallinta ja Azure Active Directory-ryhmä](active-directory-manage-groups.md)
* [Artikkelissa indeksin Azure Active Directory-sovellusten hallinta](active-directory-apps-index.md)
* [Azure Active Directory-ominaisuudet](active-directory-whatis.md)
* [Azure Active Directory-integraation paikallisen käyttäjätietoja](active-directory-aadconnect.md)

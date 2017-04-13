
<properties
    pageTitle="Dynaaminen ryhmien jäsenyyden vianmääritys | Microsoft Azure"
    description="Vianmääritysvihjeitä dynaaminen Azure AD-ryhmien jäsenyyden."
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


# <a name="troubleshooting-dynamic-memberships-for-groups"></a>Dynaaminen ryhmien jäsenyyksien vianmääritys

**Ryhmä-määritetty säännön, mutta jäsenyyksiä ei päivitä-ryhmässä**<br/>Tarkista, että **Ota käyttöön valtuutetun hallinnan ryhmän** -asetus on määritetty **Kyllä** **määrittäminen** -välilehdessä. Näet tämän asetuksen, vain, jos olet kirjautunut sisään käyttäjänä, jolle on määritetty Azure Active Directory-Premium-käyttöoikeus. Tarkista käyttäjän määritteet säännön arvot: onko käyttäjät, jotka täyttävät säännön?

**Voin määrittää säännön, mutta nyt säännön olemassa olevia jäseniä poistetaan**<br/>Tämä on normaalia. Olemassa olevan ryhmän jäsenet poistetaan, kun sääntö on käytössä tai muuttaa. Arvioinnin säännön palauttama käyttäjät lisätään jäsenet-ryhmään.     

**En näe jäsenyys muuttuu oleviin kun voin lisätä tai muuttaa säännön, miksi ei?**<br/>Oma jäsenyys arviointi tapahtuu säännöllisesti asynkroninen tausta-prosessin. Kuinka kauan menee määräytyy käyttäjämäärä hakemistossa ja luotu sääntö tuloksena ryhmän kokoa. Kansioita on pieni käyttäjät näkevät yleensä ryhmän jäsenyyden muutokset alle muutaman minuutin kuluttua. Kansioiden suuri määrä käyttäjiä voi kestää 30 minuuttia tai pidentää täyttämään.

Seuraavissa artikkeleissa on lisätietoja Azure Active Directory.

* [Resurssien käytön hallinta ja Azure Active Directory-ryhmä](active-directory-manage-groups.md)
* [Artikkelissa indeksin Azure Active Directory-sovellusten hallinta](active-directory-apps-index.md)
* [Azure Active Directory-ominaisuudet](active-directory-whatis.md)
* [Azure Active Directory-integraation paikallisen käyttäjätietoja](active-directory-aadconnect.md)

<properties
    pageTitle="Azure Active Directory tunnistetietojen suojaus - eston poistaminen käyttäjät | Microsoft Azure"
    description="Lue, miten salliminen käyttäjille, jotka on estänyt Azure Active Directory tunnistetietojen suojaus-käytäntö."
    services="active-directory"
    keywords="Azure active Directoryn tunnistetietojen suojaus-käyttäjän salliminen"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="markvi"/>

#<a name="azure-active-directory-identity-protection---how-to-unblock-users"></a>Azure Active Directory tunnistetietojen suojaus - käyttäjien eston poistaminen

Azure Active Directory tunnistetietojen suojauksen voit määrittää käytännöt estä käyttäjiä, jos määritetyt ehdot täyttyvät. Yleensä estetty käyttäjän yhteystiedot tukipalveluun luomisella esto. Tässä ohjeaiheessa kerrotaan, voit suorittaa estettyjen käyttäjän eston poistaminen ohjeita.


## <a name="determine-the-reason-for-blocking"></a>Määritä syy estäminen

Ensimmäisessä vaiheessa voit poistaa eston sinun täytyy määrittää käytännön, joka on estänyt käyttäjän, koska seuraavat vaiheet riippuu siitä. Azure Active Directory tunnistetietojen suojaus-ja käyttäjä voidaan estää joko kirjautumisen riskin käytännön tai käyttäjän riskin käytäntöä. 

Voit hankkia käytännön, joka on estänyt otsikkoa, joka on esitetty käyttäjälle aikana kirjautumisen yritys-valintaikkunassa käyttäjältä tyyppi:

|Käytäntö | Käyttäjä-valintaikkuna|
|--- | --- |
|Kirjaudu sisään riski | ![Estetyt kirjautuminen](./media/active-directory-identityprotection-unblock-howto/02.png) |
|Käyttäjän riski | ![Estetyn tilin](./media/active-directory-identityprotection-unblock-howto/104.png) |


Käyttäjä, jonka on estänyt:

- Kirjaudu sisään riskin käytännön käytetään myös nimitystä epäilyttävistä kirjautuminen
- Käyttäjän riskin käytäntöä käytetään myös nimitystä tilin vastuullasi

 
## <a name="unblocking-suspicious-sign-ins"></a>Eston epäilyttävistä merkki-laajennukset

Voit poistaa eston epäilyttävistä kirjautumisnimi, sinulla on seuraavat vaihtoehdot:

1. **Kirjaudu sisään tuttuja sijainnista tai laitteen** - yleisiä syy estettyjen epäilyttävistä merkki-laajennukset ovat kirjautumisen yritykset tuntemattomia sijainteja tai laitteet. Käyttäjien voit nopeasti tarkistaa, onko kyseessä eston syy yrittämällä sisään tuttuja sijainti tai laitteesta.


3. **Käytännön pois** - Jos-kirjautuminen käytäntöjen nykyiset määritykset aiheuttaa ongelmia tietyille käyttäjille, voit jättää käyttäjät siitä. Katso lisätietoja [kirjautumisen riskin käytännön](active-directory-identityprotection.md#sign-in-risk-policy) .
 
4. **Käytännön käytöstä** – Jos käytännön määritys aiheuttaa ongelmia kaikille käyttäjille, voit poistaa käytännön. Katso lisätietoja [kirjautumisen riskin käytännön](active-directory-identityprotection.md#sign-in-risk-policy) .


## <a name="unblocking-accounts-at-risk"></a>Vastuullasi eston tilit

Voit poistaa eston vastuullasi tili, sinulla on seuraavat vaihtoehdot:

1. **Salasanan** – voit palauttaa käyttäjän salasanan. Katso lisätietoja [Manuaalinen suojattua salasanan palauttaminen](active-directory-identityprotection.md#manual-secure-password-reset) .

2. **Hylkää kaikki riski tapahtumat** - käyttäjän riskin käytäntöä estää käyttäjän, jos määritetty käyttäjän riskin tasolla, access estää on saavutettu. Voit pienentää käyttäjä on riskin taso sulkemalla manuaalisesti raportoitua riskin tapahtumaa. Lisätietoja on artikkelissa [sulkeminen riskin tapahtumat manuaalisesti](active-directory-identityprotection.md#closing-risk-events-manually).

3. **Käytännön pois** - Jos-kirjautuminen käytäntöjen nykyiset määritykset aiheuttaa ongelmia tietyille käyttäjille, voit jättää käyttäjät siitä. Katso lisätietoja [käyttäjän riskin käytäntöä](active-directory-identityprotection.md#user-risk-policy) .
 
4. **Käytännön käytöstä** – Jos käytännön määritys aiheuttaa ongelmia kaikille käyttäjille, voit poistaa käytännön. Katso lisätietoja [käyttäjän riskin käytäntöä](active-directory-identityprotection.md#user-risk-policy) .




## <a name="next-steps"></a>Seuraavat vaiheet

 Haluatko lisätietoja Azure AD-tunnistetiedot? Tutustu [Azure Active Directory tunnistetietojen suojaus](active-directory-identityprotection.md).
 


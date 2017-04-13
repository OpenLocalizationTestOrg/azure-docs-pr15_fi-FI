<properties
    pageTitle="Azure MFA kirjautumisikkuna kokemusta Azure Monimenetelmäisen todentamisen"
    description="Tällä sivulla esitellään voit nähdä menetelmiä kirjautumisikkuna Azure MFA käytettävissä mistä."
    keywords="käyttäjien todentamiseen, kirjautuminen, kirjaudu sisään matkapuhelimeen, kirjaudu sisään toimiston puhelinnumero"
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="kgremban"/>

# <a name="the-sign-in-experience-with-azure-multi-factor-authentication"></a>Kirjaudu sisään Azure Monimenetelmäisen todentamisen kokemusta
> [AZURE.NOTE]  Seuraavat toimitetussa tällä sivulla näkyy tyypillinen kirjautuminen.  Saat apua kirjautumisessa [on ongelmia Azure Monimenetelmäisen todentamisen](multi-factor-authentication-end-user-manage-settings.md)



## <a name="what-will-your-sign-in-experience-be"></a>Mitä rekisteröityminen-toiminto on?
Käyttäjäkokemuksen vaihtelevat sen mukaan, kuinka voit kirjautua sisään ja käyttää monimenetelmäisen todentamisen.  Tässä osassa on päivitystavasta tietoja toiminta, kun kirjaudut sisään.  Valitse haluamasi vaihtoehto, joka kuvaa parhaiten seuraavat toimet:


Mitä teet?|Kuvaus
:------------- | :------------- |
[Kirjautuminen sisään matkapuhelimella tai office](#signing-in-with-mobile-or-office-phone) | Tämä on toimenpiteet-kirjautumisessa käyttäminen matkapuhelimella tai office.
[Kirjautuminen sisään Microsoft-Authenticator sovellusta ilmoitus](#signing-in-with-the-microsoft-authenticator-app-using-notification) | Tämä on toimenpiteet Authenticator Microsoft-sovelluksen käyttäminen ilmoitukset.
[Kirjautuminen sisään Microsoft-Authenticator sovellusta tarkistuskoodi](#signing-in-with-the-microsoft-authenticator-app-using-verification-code)|Tämä on toimenpiteet käyttämällä Microsoft Authenticator thapp vahvistus-koodilla.
[Vaihtoehtoinen menetelmä kirjautuminen](#signing-in-with-an-alternate-method)|Tämä näyttää toiminta, jota haluat käyttää vaihtoehtoista menetelmää.

## <a name="signing-in-with-mobile-or-office-phone"></a>Kirjautuminen sisään matkapuhelimella tai office

Seuraavassa kuvataan kokemukset monimenetelmäisen todentamisen käyttäminen matkapuhelimella tai office.

### <a name="to-sign-in-with-a-call-to-your-office-or-mobile-phone"></a>Kirjautumaan sisään puhelun office tai matkapuhelimeen

- Kirjaudu sisään sovelluksen tai palvelun, kuten Office 365: ssä käyttämällä käyttäjänimeä ja salasanaa.
- Microsoft soittaa sinulle.

![Microsoft-puhelut](./media/multi-factor-authentication-end-user-signin-phone/call.png)

- Vastaa puhelimeen ja painamalla #-näppäintä.

![Answer (vastaus)](./media/multi-factor-authentication-end-user-signin-phone/phone.png)

- Sinun pitäisi nyt kirjautunut sisään.</li>

## <a name="signing-in-with-the-microsoft-authenticator-app-using-notification"></a>Kirjautuminen sisään Microsoft-Authenticator sovellusta ilmoitus

Seuraavassa kuvataan kokemus monimenetelmäisen todentamisen käytön Microsoft Authenticator sovellus, kun lähetetään ilmoitus.

### <a name="to-sign-in-with-a-notification-sent-the-microsoft-authenticator-app"></a>Kirjaudu sisään ilmoituksen lähetetty Microsoft Authenticator-sovellus

- Kirjaudu sisään sovelluksen tai palvelun, kuten Office 365: ssä käyttämällä käyttäjänimeä ja salasanaa.
- Microsoft lähettää ilmoituksen.

![Microsoft lähettää ilmoituksen](./media/multi-factor-authentication-end-user-signin-app-notify/notify.png)


- Vastaa puhelimeen ja valitse sitten Tarkista-näppäintä.  Jos yrityksesi edellyttää PIN-tunnuksen sinua pyydetään sen tähän.

![Tarkista](./media/multi-factor-authentication-end-user-signin-app-notify/phone.png)

![Asetukset](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan3.png)

- Sinun pitäisi nyt kirjautunut sisään.


## <a name="signing-in-with-the-microsoft-authenticator-app-using-verification-code"></a>Kirjautuminen sisään Microsoft-Authenticator sovellusta tarkistuskoodi

Seuraavassa kuvataan kokemus monimenetelmäisen todentamisen käytön Microsoft Authenticator sovellus, kun käytät sitä vahvistus-koodilla.

### <a name="to-sign-in-using-a-verification-code-with-the-microsoft-authenticator-app"></a>Kirjautuminen vahvistus-koodin avulla Microsoft Authenticator-sovelluksen kanssa

- Kirjaudu sisään sovelluksen tai palvelun, kuten Office 365: ssä käyttämällä käyttäjänimeä ja salasanaa.
- Microsoft pyytää vahvistusta koodi.

![Kirjoita tarkistuskoodi](./media/multi-factor-authentication-end-user-signin-app-verify/verify.png)

- Avaa puhelimessa Microsoft Authenticator-sovellus ja kirjoita ruutuun, johon voit kirjautua sisään koodi.

![Koodin hankkiminen](./media/multi-factor-authentication-end-user-signin-app-verify/phone.png)



- Sinun pitäisi nyt kirjautunut sisään.


## <a name="signing-in-with-an-alternate-method"></a>Vaihtoehtoinen menetelmä kirjautuminen


Seuraavassa osassa kerrotaan, miten kirjaudutaan sisään Vaihtoehtoinen menetelmä ensisijainen menetelmä ei ehkä ole käytettävissä.

### <a name="to-sign-in-with-an-alternate-method"></a>Vaihtoehtoinen menetelmä kirjautumiseen

- Kirjaudu sisään sovelluksen tai palvelun, kuten Office 365: ssä käyttämällä käyttäjänimeä ja salasanaa.
- Valitse Käytä vaihtoehtoisen tarkistusmenetelmän.  Pystyt esitä valita eri vaihtoehdoista. Näkyy, kuinka monta on vielä määrittäminen perustuu näkyy numero.

![Vaihtoehtoinen menetelmä Käytä](./media/multi-factor-authentication-end-user-signin-alt/alt.png)

- Valitse Vaihtoehtoinen menetelmä ja kirjaudu sisään.

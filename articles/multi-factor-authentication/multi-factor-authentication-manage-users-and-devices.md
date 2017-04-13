<properties 
    pageTitle="Azure multi-factor Authentication-raportit"
    description="Tämä kerrotaan, miten voit muuttaa Käyttäjäasetukset, kuten pakottaminen käyttäjät voivat tehdä kuitti ylöspäin prosessin uudelleen."
    documentationCenter=""
    services="multi-factor-authentication"
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="managing-user-settings-with-azure-multi-factor-authentication-in-the-cloud"></a>Käyttäjäasetusten kanssa Azure Monimenetelmäisen todentamisen pilveen hallinta

Järjestelmänvalvojana voit hallita seuraavia käyttäjän ja laitteen asetuksia.  

- [Edellyttää valitun yhteyshenkilön menetelmiä uudelleen](#require-selected-users-to-provide-contact-methods-again)
- [Sovelluksen salasanat nykyisen käyttäjän poistaminen](#delete-users-existing-app-passwords)
- [Palauttaa kaikki keskeytetty laitteet käyttäjän MFA](#restore-mfa-on-all-suspended-devices-for-a-user)






Tämä on hyödyllinen, jos tietokoneen tai laitteen katoaa tai varastamiselta tai jos haluat poistaa käyttäjät voivat käyttää.


## <a name="require-selected-users-to-provide-contact-methods-again"></a>Edellyttää valitun yhteyshenkilön menetelmiä uudelleen

Tämä asetus on pakottaa käyttäjä voi suorittaa rekisteröinnin loppuun uudelleen, kun hän kirjautuu sisään. Huomaa, että selaimen sovellukset toimivat edelleen Jos käyttäjällä on sovelluksen salasanat niiden.  Voit poistaa käyttäjät-sovelluksen salasanat valitsemalla myös **poistaa kaikki aiemmin ohjelmasalasanat, luomien kyseisille käyttäjille**.

### <a name="how-to-require-users-to-provide-contact-methods-again"></a>Miten kehottavan käyttäjiä antamaan yhteyshenkilön menetelmiä uudelleen




1. Kirjaudu sisään Azure perinteinen-portaaliin.
2. Valitse vasemmassa reunassa Active Directorysta.
3. Valitse-kansion kohdasta haluat antaa heidän yhteydenottotapa uudelleen edellyttää käyttäjän kansio.
4. Valitse käyttäjät yläreunassa.
5. Valitse sivun alareunassa multi-factor todennussertifikaatin hallinta Multi-factor authentication-sivu avautuu.
6. Etsi käyttäjä, jota haluat hallita ja laita nimen vieressä olevaa ruutua. Joudut ehkä muuttamaan näkymän yläreunassa.
7. Tämä tuo mukanaan oikealla **Käyttäjäasetusten hallinta** -linkkiä. Napsauta sitä.
8. Laita **edellyttää valitun yhteyshenkilön menetelmiä uudelleen**.
![Anna yhteyshenkilön menetelmät](./media/multi-factor-authentication-manage-users-and-devices/reproofup.png)
10. Valitse Tallenna.
11. Sulje

## <a name="delete-users-existing-app-passwords"></a>Sovelluksen salasanat nykyisen käyttäjän poistaminen

Tämä poistaa kaikki sovelluksen salasanoja, joita käyttäjä on luotu. Muu selain sovellukset, jotka on liitetty nämä ohjelmasalasanat lakkaavat toimi, ennen kuin uusi sovelluksen salasana on luotu.

### <a name="how-to-delete-users-existing-app-passwords"></a>Voit poistaa sovelluksen salasanat nykyiset käyttäjät

1. Kirjaudu sisään Azure perinteinen-portaaliin.
2. Valitse vasemmassa reunassa Active Directorysta.
3. -Kohdassa kansio Valitse sovelluksen salasanat poistettavan käyttäjän kansio.
4. Valitse käyttäjät yläreunassa.
5. Valitse sivun alareunassa multi-factor todennussertifikaatin hallinta Multi-factor authentication-sivu avautuu.
6. Etsi käyttäjä, jota haluat hallita ja laita nimen vieressä olevaa ruutua. Joudut ehkä muuttamaan näkymän yläreunassa.
7. Tämä tuo mukanaan oikealla **Käyttäjäasetusten hallinta** -linkkiä. Napsauta sitä.
8. Valitse **Poista kaikki olemassa olevat ohjelmasalasanat, kyseisille käyttäjille luomaa**valintamerkki.
![Poista sovelluksen salasanat](./media/multi-factor-authentication-manage-users-and-devices/deleteapppasswords.png)
10. Valitse Tallenna.
10. Valitse Sulje.

## <a name="restore-mfa-on-all-remembered-devices-for-a-user"></a>Palauttaa kaikki tallennettujen laitteet käyttäjän MFA

Järjestelmänvalvojat voivat palauttaa Monimenetelmäisen todentamisen käyttäjien laitteet ja selaimet. Kun teet tämän, tämä poistaa muista MFA kaikki käyttäjän laitteet ja selaimet ja käyttäjän tarvitaan käyttämään MFA, kun kirjaudun sisään seuraavan kerran.

### <a name="how-to-restore-mfa-on-all-suspended-devices-for-a-user"></a>Kaikki keskeytetty laitteet käyttäjän MFA palauttaminen

1. Kirjaudu sisään Azure perinteinen-portaaliin.
2. Valitse vasemmassa reunassa Active Directorysta.
3. Valitse-kansion kohdasta haluat palauttaa mfa käyttäjän kansio.
4. Valitse käyttäjät yläreunassa.
5. Valitse sivun alareunassa multi-factor todennussertifikaatin hallinta Multi-factor authentication-sivu avautuu.
6. Etsi käyttäjä, jota haluat hallita ja laita nimen vieressä olevaa ruutua. Joudut ehkä muuttamaan näkymän yläreunassa.
7. Tämä tuo mukanaan oikealla **Käyttäjäasetusten hallinta** -linkkiä. Napsauta sitä.
8. Laita **palauttaa monimenetelmäisen todentamisen tallennettujen laitteissa**
![ohjelmasalasanat poistaminen](./media/multi-factor-authentication-manage-users-and-devices/rememberdevices.png)
9. Valitse Tallenna.
10. Valitse Sulje.

<properties
    pageTitle="Matkapuhelimet Microsoft Authenticator-sovellus | Microsoft Azure"
    description="Opi päivittämään Azure todentaja uusimman version."
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

# <a name="microsoft-authenticator"></a>Microsoft todentaja

Microsoft Authenticator sovelluksen tarjoaa suojauksen Azure-tilisi (esimerkiksi bsimon@contoso.onmicrosoft.com), paikallisen yhteyttä työpaikan tili (esimerkiksi bsimon@contoso.com), tai Microsoft-tilillä (esimerkiksi bsimon@outlook.com).

Sovellus toimii jollakin seuraavista tavoista:

- **Ilmoitus**. Sovellus voi auttaa estämään luvatonta pääsyä tilit ja Lopeta vilpilliseen tapahtumien valitseminen ilmoituksen älypuhelimeen tai tablettiin. Vain tarkastella ilmoituksen ja jos se on oikeutettu, valitse **Vahvista**. Muussa tapauksessa voit valita **Estä**. Tutustu tietoja ilmoitukset estäminen estä ja raportin petoksen ominaisuuksien käyttäminen Monimenetelmäisen todentamisen.

- **Salasanassa tarkistuskoodi**. Sovellus voidaan kuin ohjelmiston tunnuksen OAuth-tarkistuskoodi luomiseen. Voit kirjoittaa koodin myöntämä sovellus kirjautumisen näytössä, käyttäjänimi ja salasana, sekä pyydettäessä. Tarkistuskoodi on toisen lomakkeen todennusta.

Microsoft Authenticator sovelluksen versiossa korvataan Azure todentaja vanha sovellus.  Azure todentaja-sovellus toimii edelleen, mutta jos Siirrä uusi Microsoft Authenticator-sovellukseen, tämän artikkelin voi auttaa sinua.  

## <a name="install-the-app"></a>Asenna sovellus

Microsoft Authenticator-sovellus on käytettävissä [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071)-, [Android](http://go.microsoft.com/fwlink/?Linkid=825072)- ja [IOS](http://go.microsoft.com/fwlink/?Linkid=825073).

## <a name="add-accounts-to-the-app"></a>Tilien lisääminen sovellukseen

Jokaiselle tilille, jonka haluat lisätä Microsoft Authenticator-sovellukseen Käytä jotakin seuraavista menettelytavoista.

### <a name="add-an-account-to-the-app-by-using-the-qr-code-scanner"></a>Tilin lisääminen sovellukseen QR-koodi skannerin avulla

1. Siirry suojauksen vahvistus-näytössä.  Saat lisätietoja siitä, miten tämä näyttö, [tietoturva-asetusten muuttaminen](multi-factor-authentication-end-user-manage-settings.md).

2. Valitse **Määritä**.

    ![Suojauksen todentaminen näytössä Määritä-painike](./media/multi-factor-authentication-azure-authenticator/azureauthe.png)

    Tämä näyttää näyttö, jossa QR-koodi, valitse se.

    ![Näyttö, joka sisältää QR-koodi](./media/multi-factor-authentication-azure-authenticator/barcode2.png)

3. Avaa Microsoft todentaja-sovellus. Valitse **tilit** -ruudun **+**, ja määritä, johon haluat lisätä työpaikan tai oppilaitoksen tiliä.

    ![Tilit-näyttö, jossa plusmerkki](./media/multi-factor-authentication-azure-authenticator/addaccount3.png)

    ![Työpaikan tai oppilaitoksen tiliä, joka määrittää-näytössä](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan.png)

4. Kameran avulla skannaamalla QR-koodi ja valitse sitten Sulje QR-koodi näytön **Valmis** .

    ![Näytön lukemiseen QR-koodi](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan2.png)

    Jos kameraa ei toimi oikein, voit kirjoittaa QR-koodi ja URL-Osoitteen manuaalisesti. Lisätietoja on artikkelissa [Lisää sovellus-tili manuaalisesti](#add-an-account-to-the-app-manually).

5. Odota, kun tili on aktivoitu. Kun aktivointi on valmis, valitse **yhteyshenkilö minä**.  Tämä lähettää ilmoituksen tai vahvistusta koodi puhelimeen.  Valitse **Vahvista**.

    ![Näyttö, jossa valitaan Tarkista kirjautuminen](./media/multi-factor-authentication-end-user-first-time-mobile-app/verify.png)

6. Jos yrityksesi edellyttää PIN-kirjautuminen vahvistus hyväksymisestä, kirjoita se.

    ![Ruutuun PIN-tunnuksen kirjoittamista varten](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan3.png)

7. Kun PIN-määrite on valmis, valitse **Sulje**. Tässä vaiheessa tarkistuksen olisi onnistunut.
8. On suositeltavaa, anna matkapuhelinnumerosi siltä varalta, menetät access sovelluksen. Määritä avattavasta luettelosta maa ja anna matkapuhelinnumerosi maa-nimen vieressä olevaan ruutuun. Valitse **Seuraava**.
9. Tässä vaiheessa olet määrittänyt oman yhteydenottotapa. Nyt on aika määrittää sovelluksen salasanat selaimen sovellusten, kuten Outlook 2010: ssä tai vanhemmissa versioissa. Jos et käytä nämä sovellukset, valitse **Valmis**. Muussa tapauksessa siirry seuraavaan vaiheeseen.

    ![Näytön sovelluksen salasanan luomiseen](./media/multi-factor-authentication-end-user-first-time-mobile-app/step4.png)

10. Jos käytät selaimessa sovellukset, kopioi annettu ohjelman salasana ja salasanan Liitä sovelluksia. Yksittäisten sovellusten, kuten Outlook ja Lync-ohjeita on kohdassa muuttamisesta ohjelman salasana salasana sähköpostissa ja salasanan sovelluksesi sovelluksen salasana.
11. Valitse **Valmis**.

Valitse **tilit** -näytössä pitäisi tulla näkyviin uusi tili.

![Tilit-näytössä](./media/multi-factor-authentication-azure-authenticator/accounts.png)

### <a name="add-an-account-to-the-app-manually"></a>Tilin lisääminen sovellukseen manuaalisesti

1. Siirry suojauksen vahvistus-näytössä.  Saat lisätietoja siitä, miten tämä näyttö, [tietoturva-asetusten muuttaminen](multi-factor-authentication-end-user-manage-settings.md).

2. Valitse **Määritä**.

    ![Suojauksen todentaminen näytössä Määritä-painike](./media/multi-factor-authentication-azure-authenticator/azureauthe.png)

    Tämä näyttää näyttö, jossa QR-koodi, valitse se.  Huomautus koodi ja URL-osoite.

    ![Näyttö, joka tarjoaa URL-osoite ja QR-koodi](./media/multi-factor-authentication-azure-authenticator/barcode2.png)

3. Avaa Microsoft todentaja-sovellus. Valitse **tilit** -ruudun **+**, ja määritä, johon haluat lisätä työpaikan tai oppilaitoksen tiliä.

    ![Tilit-näyttö, jossa plusmerkki](./media/multi-factor-authentication-azure-authenticator/addaccount3.png)

    ![Työpaikan tai oppilaitoksen tiliä, joka määrittää-näytössä](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan.png)

4. Valitse skannerin **kirjoittaa koodin manuaalisesti**.

    ![Näytön lukemiseen QR-koodi](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan2.png)

5. Kirjoita sovelluksen vastaaviin ruutuihin koodi ja URL-osoite.

    ![Näytön koodi ja URL-osoite](./media/multi-factor-authentication-azure-authenticator/manual.png)

    ![Näytön koodi ja URL-osoite](./media/multi-factor-authentication-end-user-first-time-mobile-app/addaccount2.png)

6. Odota, kun tili on aktivoitu. Kun aktivointi on valmis, valitse **yhteyshenkilö minä**. Tämä lähettää ilmoituksen tai vahvistusta koodi puhelimeen. Valitse **Vahvista**.

Valitse **tilit** -näytössä pitäisi tulla näkyviin uusi tili.

![Tilit-näytössä](./media/multi-factor-authentication-azure-authenticator/accounts.png)

### <a name="add-an-account-to-the-app-by-using-touch-id"></a>Tilin lisääminen sovellukseen käyttämällä kosketa tunnus

IOS Microsoft Authenticator-sovellus tukee Touch ID-tunnuksellasi.  Azure Monimenetelmäisen todentamisen avulla organisaatiot voivat Vaadi PIN-tunnuksen laitteet. Kosketa tunnuksella iOS käyttäjien ei tarvitse tehdä PIN-tunnus. Ne, tarkistaa heidän tunnisteen ja valitse **Hyväksy**.

Kosketa tunnus, jonka Microsoft Authenticator määrittämisestä on helppoa. Olet tehnyt Normaali vahvistus hankala PIN-koodilla. Jos laitteesi tukee kosketa tunnus, Microsoft Authenticator määrittämällä automaattisesti tilin.

![Kosketa tunnusta asennuksen tarkistaminen](./media/multi-factor-authentication-azure-authenticator/touchid1.png)

Osoitteesta eteenpäin, kun sinua pyydetään tarkistamaan käyttäjän kirjautumisnimi, valitse vastaanotettu push-ilmoitus ja tarkista oman tunnisteen PIN-koodin kirjoittamisen sijaan.

![Push-ilmoitus](./media/multi-factor-authentication-azure-authenticator/touchid2.png)

## <a name="uninstall-the-old-azure-authentication-app"></a>Vanha Azure todennus-sovelluksen asennuksen poistaminen

Kun olet lisännyt kaikki tilit uutta sovellusta, voit poistaa vanhan puhelimesta.

## <a name="delete-an-account"></a>Tilin poistaminen

Voit poistaa tilin Authenticator Microsoft-sovelluksen, valitse tili ja valitse sitten **Poista**.

![Poista-painike](./media/multi-factor-authentication-azure-authenticator/remove.png)

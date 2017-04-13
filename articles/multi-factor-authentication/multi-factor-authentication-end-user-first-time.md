<properties
    pageTitle="Kaksivaiheinen vahvistus on oma työpaikan tai oppilaitoksen tilin määrittäminen"
    description="Kun yrityksesi määrittää Azure Monimenetelmäisen todentamisen, voit pyydetään kaksivaiheinen vahvistus on rekisteröitävä. Opettele määrittämällä sivuston. "
    services="multi-factor-authentication"
    keywords="Opi käyttämään azure hakemistosta, active Directoryn pilvipalvelussa, active directory-opetusohjelma"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="pblachar"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="kgremban"/>

# <a name="set-up-my-account-for-two-step-verification"></a>Määritä oma tili kaksivaiheista vahvistusta varten

Kaksivaiheinen vahvistus on lisäsuojauksen vaihe, joka auttaa suojaamaan tilisi määrittämällä käyttöön näyttävyyttä muut käyttäjät voivat katkaista. Jos olet lukenut tämän artikkelin, todennäköisesti saatu sähköpostiviestin tietoja Monimenetelmäisen todentamisen yhteyttä työpaikan tai oppilaitoksen järjestelmänvalvojaan. Tai ehkä yrittänyt kirjautua sisään ja sait sanoman, jossa pyydetään määrittämään lisäsuojauksen vahvistus. Jos tämä ei **pysty kirjautumaan sisään, kunnes olet automaattisen rekisteröinnin prosessin**tapauksessa.

Tämän artikkelin avulla voit määrittäminen **työpaikan tai oppilaitoksen tiliä**. Jos haluat ottaa käyttöön kaksivaiheinen vahvistus on omat, henkilökohtainen Microsoft-tili, katso [Lisätietoja kaksivaiheista vahvistusta varten](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification).

## <a name="determine-how-you-will-use-multi-factor-authentication"></a>Määrittää, miten voit käyttää monimenetelmäisen todentamisen

Kaksivaiheinen vahvistus on toimii siten, jossa kehotetaan lataamaan kaksi kappaletta tunnus, kun kirjaudut sisään. Tarkista ensin pyydämme käyttäjänimi ja salasana tavalliseen tapaan. Valitse sitten Microsoft yhteyshenkilön matkapuhelimen, joka on tiedettävä kuuluu, ja käyttäjä vahvistaa kirjautumisen yritys on oikeutettu.  

Aloita asennusprosessi, yritä kirjautumaan tilillesi, ettei sinun on yleensä tehtävä. Jos järjestelmänvalvoja on määrittänyt tilin kaksivaiheista vahvistusta varten, voit pyydetään automaattisen rekisteröinnin Aloita. Aloita tämä napsauttamalla **nyt määrittämällä.**

![Asetukset](./media/multi-factor-authentication-end-user-first-time/first.png)

Laitteen käyttöönottoprosessin yhteydessä ensimmäistä kysymystä on, miten haluat ottaa sinuun yhteyttä. Tutustu asetukset-taulukon ja linkkien avulla voit siirtyä määritysvaiheiden palauttamista varten.

| Yhteydenottotapa | Kuvaus |
| --- | --- |
[Mobiilisovelluksessa](#use-a-mobile-app-as-the-contact-method) | - **Voit saada ilmoituksen vahvistusta varten.** Tämä vaihtoehto Vie ilmoituksen todentaja sovellukseen älypuhelimeen tai taulutietokoneella. Näytä ilmoitus, ja jos se on oikeutettu, valitse **Tarkista** -sovelluksessa. Työpaikan tai oppilaitoksen saattaa edellyttää, että kirjoitat PIN-tunnus, ennen kuin voit todennusta.<br>- **Käytä tarkistuskoodi.** Tässä tilassa todentaja-app Luo tarkistuskoodi, joka päivittää 30 sekunnin välein. Kirjoita Kirjaudu sisään-liittymän uusimmat tarkistuskoodi.<br>Microsoft Authenticator-sovellus on käytettävissä [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071)-, [Android](http://go.microsoft.com/fwlink/?Linkid=825072)- ja [IOS](http://go.microsoft.com/fwlink/?Linkid=825073). |
[Mobiili-puhelun tai teksti](#use-your-mobile-phone-as-the-contact-method) | - **Puhelun** sijoittaa automaattinen äänipuhelu antaisit puhelinnumeroon. Vastaa puheluun ja #, paina puhelimen näppäimistön tarkistamiseen.<br>- **Tekstiviestin** päättyy vahvistus-koodia sisältävän tekstiviestillä. Tekstin komentokehotteeseen seuraavat vastaa tekstiä tai anna Kirjaudu sisään-liittymän kyselyjä annettu tarkistuskoodi. |  
[Office-puhelu](#use-your-office-phone-as-the-contact-method) | Sijoittaa automaattinen äänipuhelu antaisit puhelinnumeroon. Vastaa puheluun ja painaa # puhelimen näppäimistön tarkistamiseen. |

## <a name="use-a-mobile-app-as-the-contact-method"></a>Käytä mobiilisovelluksessa yhteydenottotapa

Tällä menetelmällä edellyttää todentaja-sovelluksen asentaminen puhelimeen tai taulutietokoneeseen. Tässä artikkelissa kuvattuja perustuvat Microsoft Authenticator-sovellus, joka on saatavana [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071)-, [Android](http://go.microsoft.com/fwlink/?Linkid=825072)- ja [IOS](http://go.microsoft.com/fwlink/?Linkid=825073).

1. Valitse **mobiilisovelluksessa** avattavasta luettelosta.
2. Valitse **ilmoituksia vahvistusta varten** tai **Käytä tarkistuskoodi**ja valitse sitten **Määritä**.

    ![Lisäsuojauksen vahvistus-näyttö](./media/multi-factor-authentication-end-user-first-time-mobile-app/mobileapp.png)

3. Puhelimella tai taulutietokoneella, Avaa sovellus ja valitse **+** tilin lisääminen. (Android-laitteisiin, valitse kolme pistettä, valitse **Lisää tili**.)
4. Määritä, johon haluat lisätä työpaikan tai oppilaitoksen tiliä. Avaa puhelimessa QR-koodi skanneri. Jos kameraa ei toimi oikein, voit valita yrityksen tietojen lisääminen manuaalisesti. Lisätietoja on artikkelissa [tilin lisääminen manuaalisesti](#add-an-account-manually).  
5. Skannaa QR-koodi kuva, jotka olivat ja näytön määrittämiseen mobile-sovellus.  Valitse Sulje QR-koodi näytön **Valmis** .  

    ![QR-koodi-näyttö](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan2.png)

6. Kun aktivointi puhelimessa on valmis, valitse **yhteyshenkilö minä**.  Tässä vaiheessa lähettää ilmoituksen tai vahvistusta koodi puhelimeen. Valitse **Vahvista**.  
7. Jos yrityksesi edellyttää PIN-kirjautuminen vahvistus hyväksymisestä, kirjoita se.

    ![Ruutuun PIN-tunnuksen kirjoittamista varten](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan3.png)

8. Kun PIN-määrite on valmis, valitse **Sulje**. Tässä vaiheessa tarkistuksen olisi onnistunut.
9. On suositeltavaa, että kirjoitat matkapuhelinnumerosi siltä varalta, menetät access mobile-sovellus. Määritä avattavasta luettelosta maa ja anna matkapuhelinnumerosi maa-nimen vieressä olevaan ruutuun. Valitse **Seuraava**.
10. Tässä vaiheessa sinua kehotetaan selaimen sovellukset, kuten Outlook 2010: ssä tai vanhemmissa versioissa ohjelmasalasanat tai Apple-laitteissa alkuperäisen sähköpostisovellusta. Tämä johtuu siitä, että jotkin sovellukset eivät tue kaksivaiheista vahvistusta varten. Jos et käytä nämä sovellukset, valitse **Valmis** ja ohittaa loput vaiheet.
11. Jos käytössäsi on nämä sovellukset, kopioi ohjelman salasana annettu ja liitä se sovelluksen sijaan säännöllisesti salasanasi. Voit käyttää useita sovelluksia saman ohjelman salasana. Lisätietojen saamiseksi [apua sovelluksen salasanat].
12. Valitse **Valmis**.


### <a name="add-an-account-manually"></a>Tilin lisääminen manuaalisesti
Jos haluat lisätä tilin mobiilisovelluksessa manuaalisesti, sijaan QR Reader-ohjelmaa, toimi seuraavasti.

1. Valitse **Lisää tili manuaalisesti** -painike.  
2. Anna koodi ja jotka ovat samalla sivulla, jossa näkyy viivakoodin URL-osoite. Tämä tieto tulee **koodi** ja **URL** -ruutuihin mobile-sovellus.

    ![Asetukset](./media/multi-factor-authentication-end-user-first-time-mobile-app/barcode2.png)

3. Kun aktivointi on valmis, valitse **yhteyshenkilö minä**. Tässä vaiheessa lähettää ilmoituksen tai vahvistusta koodi puhelimeen. Valitse **Vahvista**.

## <a name="use-your-mobile-phone-as-the-contact-method"></a>Käytä Matkapuhelin yhteydenottotapa

1. Valitse **Todennus puhelin** avattavasta luettelosta.  

    ![Asetukset](./media/multi-factor-authentication-end-user-first-time-mobile-phone/phone.png)  

2. Valitse avattavasta luettelosta maa ja anna matkapuhelinnumerosi.
3. Valitse menetelmä haluat käyttää Matkapuhelin - tekstin tai puhelun aikana.
4. Valitse **yhteyshenkilö me** vahvistamiseksi puhelinnumerosi. Sen mukaan, kun olet valinnut tila Microsoft lähettää sinulle tekstiviestin tai soittaa sinulle. Noudata näytössä näkyviä ohjeita ja valitse **Vahvista**.
5. Tässä vaiheessa sinua kehotetaan selaimen sovellukset, kuten Outlook 2010: ssä tai vanhemmissa versioissa ohjelmasalasanat tai Apple-laitteissa alkuperäisen sähköpostisovellusta. Tämä johtuu siitä, että jotkin sovellukset eivät tue kaksivaiheista vahvistusta varten. Jos et käytä nämä sovellukset, valitse **Valmis** ja ohittaa loput vaiheet.
6. Jos käytössäsi on nämä sovellukset, kopioi ohjelman salasana annettu ja liitä se sovelluksen sijaan säännöllisesti salasanasi. Voit käyttää useita sovelluksia saman ohjelman salasana. Lisätietojen saamiseksi [apua sovelluksen salasanat].
7. Valitse **Valmis**.

## <a name="use-your-office-phone-as-the-contact-method"></a>Käytä toimiston puhelinnumero yhteydenottotapa

1. Valitse avattavasta luettelosta **Toimiston puhelinnumero**  

    ![Asetukset](./media/multi-factor-authentication-end-user-first-time-office-phone/office.png)  

2. Puhelimen numeroruutu täytetään automaattisesti yrityksen yhteystiedot. Jos luku on väärä, tai se puuttuu, pyydä järjestelmänvalvojaa, voit tehdä muutoksia.
4. Valitse **yhteyshenkilö me** vahvistamiseksi puhelinnumerosi, ja olemme soittavat sivunumeron. Noudata näytössä näkyviä ohjeita ja valitse **Vahvista**.
5. Tässä vaiheessa sinua kehotetaan selaimen sovellukset, kuten Outlook 2010: ssä tai vanhemmissa versioissa ohjelmasalasanat tai Apple-laitteissa alkuperäisen sähköpostisovellusta. Tämä johtuu siitä, että jotkin sovellukset eivät tue kaksivaiheista vahvistusta varten. Jos et käytä nämä sovellukset, valitse **Valmis** ja ohittaa loput vaiheet.
6. Jos käytössäsi on nämä sovellukset, kopioi ohjelman salasana annettu ja liitä se sovelluksen sijaan säännöllisesti salasanasi. Voit käyttää useita sovelluksia saman ohjelman salasana. Saat lisätietoja, [mitä sovelluksen salasanat](multi-factor-authentication-end-user-app-passwords.md).
7. Valitse **Valmis**.

## <a name="next-steps"></a>Seuraavat vaiheet

- Vaihtoehtoja ja [hallita kaksivaiheinen vahvistus on asetusten](multi-factor-authentication-end-user-manage-settings.md) muuttaminen
- Määritä [sovelluksen salasanat](multi-factor-authentication-end-user-app-passwords.md) alkuperäisen sovelluksille, jotka eivät tue kaksivaiheista vahvistusta varten.
- Tutustu [Microsoft todentaja app](multi-factor-authentication-microsoft-authenticator.md) nopea ja suojatun todennustavaksi myös silloin, kun solun palvelu ei ole.

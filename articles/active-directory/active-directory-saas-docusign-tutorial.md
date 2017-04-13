<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi DocuSign | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directory- ja DocuSign välillä."
    services="active-directory"
    documentationCenter=""
    authors="jeevansd"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-docusign"></a>Opetusohjelma: DocuSign Azure Active Directory-integrointi

Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja DocuSign integrointi.
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

- Kelvollinen Azure tilaus
- Valitse DocuSign palvelutili



Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1. [DocuSign sovelluksen integrointi ottaminen käyttöön](#enabling-the-application-integration-for-docusign) 


2. [Kertakirjautumisen määrittäminen](#configuring-single-sign-on) 


3. [Määritä tili valmistelu](#configuring-account-provisioning) 


4. [Käyttäjien määrittäminen](#assigning-users) 

    ![Kertakirjautumisen määrittäminen][0]
 

## <a name="enabling-the-application-integration-for-docusign"></a>DocuSign sovelluksen integrointi ottaminen käyttöön

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön DocuSign sovelluksen integrointi.

### <a name="to-enable-the-application-integration-for-docusign-perform-the-following-steps"></a>DocuSign sovelluksen integrointi käyttöön toimimalla seuraavasti:

1. Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Kertakirjautumisen määrittäminen][1]

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi hakemisto-luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Kertakirjautumisen määrittäminen][2]

4. Valitse sivun alareunassa **Lisää** .

    ![Sovellukset][3]

5. Valitse mitä haluat tehdä valintaikkuna valitsemalla **Lisää sovellus-valikoimasta**.

    ![Kertakirjautumisen määrittäminen][4]


6. Kirjoita hakuruutuun **DocuSign**.

    ![Kertakirjautumisen määrittäminen][5]

7. Valitse tulosruudussa **DocuSign**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Kertakirjautumisen määrittäminen][6]


## <a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen DocuSign käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.


### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1. Azure perinteinen portaalissa **DocuSign sovelluksen integrointi** sivulla Valitse **Määritä kertakirjautumisen** määrittäminen kertakirjautuminen-valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen][7]

2. **Miten käyttäjät voivat kirjautua DocuSign haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten Seuraava.

    ![Kertakirjautumisen määrittäminen][8]

3. **Määritä sovelluksen asetukset** -sivulla toimimalla seuraavasti:

    ![Kertakirjautumisen määrittäminen][61]

    a. Kirjoita **Kirjaudu URL** -tekstiruutuun `https://account.docusign.com/*`.  

    b. Kirjoita **tunnus** -tekstiruutuun `https://account.docusign.com/*`.  
   
    c-näppäinyhdistelmää. Valitse **Seuraava**. 


    > [AZURE.TIP] Merkki-URL-osoite ja tunnus-arvot ovat vain paikkamerkkejä. Tämän artikkelin kuuluvat ohjeet noutamisesta todellisten arvojen-ympäristössä.
 

4. Valitse **Määritä kertakirjautumisen osoitteessa DocuSign** -sivulla valitsemalla **Lataa**ja Tallenna sertifikaattitiedosto paikallisesti tietokoneeseen.

    ![Kertakirjautumisen määrittäminen][10]


5. Eri selainikkunassa kirjautua sisään järjestelmänvalvojana **DocuSign hallintaportaalissa** .


6. Valitse vasemman reunan siirtymispalkista **Toimialueet**.

    ![Kertakirjautumisen määrittäminen][51]

7. Valitse oikeanpuoleisen ruudun **Varaa toimialueen**.

    ![Kertakirjautumisen määrittäminen][52]

8. Valitse **Varaa toimialue** -valintaikkunassa **Toimialuenimi** -tekstiruutuun yrityksen toimialueen nimi ja valitse sitten **Varaa**. Varmista, että olet varmistanut toimialueen tila on aktiivinen.

    ![Kertakirjautumisen määrittäminen][53]

9. Valitse vasemmalla puolella-valikossa **Tunnistetietojen toimittajat**  

    ![Kertakirjautumisen määrittäminen][54]

10. Napsauta oikeanpuoleisessa ruudussa **Lisää tunnistetietojen toimittaja**. 
    
    ![Kertakirjautumisen määrittäminen][55]

11. Valitse **Tunnistetietojen toimittaja asetukset** -sivun toimimalla seuraavasti:

    ![Kertakirjautumisen määrittäminen][56]


    a. Kirjoita **nimi** -tekstiruutuun yksilöivä nimi kokoonpanon. Älä käytä välilyöntejä.

    b. Azure perinteinen portaalissa myöntäjä URL-Osoitteen kopioiminen ja liittäminen **Tunnistetietojen toimittaja myöntäjä** -tekstiruutuun.

    c-näppäinyhdistelmää. Azure perinteinen-portaalissa kopioi **Remote sisäänkirjautumisen URL-osoite**ja liitä se sitten **Tunnistetietojen toimittaja sisäänkirjautumisen URL-osoite** -tekstiruutuun.

    d. Azure perinteinen-portaalissa kopioi **Remote Kirjaudu URL-osoite**ja liitä se sitten **Tunnistetietojen toimittaja Kirjaudu URL-osoite** -tekstiruutuun.

    e. Valitse **Kirjaudu AuthN pyynnön**.

    f. **Lähetä AuthN pyyntö**valitsemalla **viesti**.

    g. **Kirjaudu ulos Lähetyspyyntö mukaan**Valitse **JULKAISE**. 


12. Valitse **Mukautettu-määrite yhdistäminen** -osassa kenttä haluat kartta ja Azure AD-ryhmän. Tässä esimerkissä **emailaddress** vaatimus on nyt yhdistetty **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**arvolla. Tämä on sähköpostin varaa Azure Active Directory-ryhmän oletusnimeä. 

    > [AZURE.NOTE] Sopiva **käyttäjätunnus** avulla voit yhdistää Azure AD käyttäjältä Docusign käyttäjän yhdistämismääritys. Valitse oikea kenttä ja anna organisaation asetusten arvo.

    ![Kertakirjautumisen määrittäminen][57]

13. **Tunnistetietojen toimittaja varmenne** -osassa **Lisää varmenteen**ja lataa varmenteen olet ladannut perinteinen Azure AD-portaalista.   

    ![Kertakirjautumisen määrittäminen][58]

14. Valitse **Tallenna**.

15. **Tunnistetietojen toimittajat** -kohdassa **Toiminnot**ja valitse sitten **päätepisteet**.   

    ![Kertakirjautumisen määrittäminen][59]



10. Azure perinteinen-portaalissa Siirry takaisin **Määrittää sovelluksen asetukset** -sivulla. 

16. **DocuSign hallintaportaalissa** **Näkymän SAML 2.0 päätepisteet** -osassa, seuraavien toimien:

    ![Kertakirjautumisen määrittäminen][60]

    a. Kopioi **Palveluntarjoajan myöntäjä URL-osoite**ja liitä se sitten Azure perinteinen portaalissa **tunnus** -tekstiruutuun.

    b. Kopioi **Palveluntarjoajan sisäänkirjautumisen URL-osoite**ja liitä Azure perinteinen-portaalissa **Merkki-URL** -tekstiruutuun.

    c-näppäinyhdistelmää.  Valitse **Sulje**  


10. Azure perinteinen-portaalissa Valitse **Seuraava**. 


15. Azure perinteinen-portaalissa Valitse **Sign määritysten vahvistus**ja valitse sitten **Seuraava**.

    ![Sovellukset][14]

10. Valitse **Sign Vahvista** -sivulla **Valmis**.

    ![Sovellukset][15]
 

## <a name="configuring-account-provisioning"></a>Määritä tili valmistelu

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjän valmistelu Active Directory-käyttäjätilien DocuSign.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Voit määrittää käyttäjän valmistelu toimimalla seuraavasti:

1. **Azure perinteinen portal**, **DocuSign sovelluksen integrointi** sivulla, valitse Määritä käyttäjän valmistelu-valintaikkunan avaaminen **Määritä tili valmistelu** .

    ![Määritä tili valmistelu][30]

2. Valitse **asetukset ja järjestelmänvalvojan tunnistetiedot** -sivulla automaattinen käyttäjän valmistelu käyttöön antaa oikeudet DocuSign tilin tunnistetiedot, ja valitse sitten **Seuraava**. 

    ![Määritä tili valmistelu][31]

3. Valitse **Testaa yhteys** -valintaikkunan **Aloita testi**ja yhteydessä onnistuneen testi, valitse **Seuraava**.

    ![Määritä tili valmistelu][32]

3. Valitse **Vahvista** -sivulla **Valmis**.

    ![Määritä tili valmistelu][33]
 

## <a name="assigning-users"></a>Käyttäjien määrittäminen

Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

### <a name="to-assign-users-to-docusign-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä DocuSign, toimi seuraavasti:

1. Testaa tilin luominen **Azure perinteinen portal**.

2. Valitse **DocuSign sovelluksen integrointi** -sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen][40]
 

3. Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Käyttäjien määrittäminen][41]


Pitäisi nyt Odota 10 minuuttia ja varmista, että tili on synkronoitu DocuSign.

Vahvistus ensimmäisessä vaiheessa voit tarkistaa valmistelun tilan napsauttamalla Raporttinäkymät-ikkunan d Azure perinteinen portaalin DocuSign sovelluksen integrointi-sivulla.

![Käyttäjien määrittäminen][42]

Jakson valmistelu onnistuneesti suoritettujen käyttäjän ilmaistaan liittyvät tila:

![Käyttäjien määrittäminen][43]


Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli.

Katso lisätietoja Access-paneelin johdanto Access-paneeli.


## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[0]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_00.png
[1]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_01.png
[6]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_02.png
[7]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_03.png
[8]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_04.png
[9]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_05.png
[10]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_06.png

[14]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_10.png
[15]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_11.png

[30]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_400.png
[31]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_12.png
[32]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_13.png
[33]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_14.png



[40]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_15.png
[41]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_16.png
[42]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_17.png
[43]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_18.png

[51]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_21.png
[52]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_22.png
[53]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_23.png
[54]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_19.png
[55]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_20.png
[56]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_24.png
[57]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_25.png
[58]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_26.png
[59]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_27.png
[60]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_28.png
[61]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_29.png
<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Dropbox for Business | Microsoft Azure" 
    description="Opettele käyttämään Dropbox for Business ja Azure Active Directory käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
    services="active-directory" 
    authors="jeevansd"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="08/16/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-dropbox-for-business"></a>Opetusohjelma: Dropbox for Business Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure- ja Dropbox for Business integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Testi-vuokraajan dropbox for Business
  
Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt Dropbox for Business, voivat kertakirjauksen, että Dropbox for Business-sovellukseen Azure AD-käyttäjien yrityksen (palveluntarjoajan aloittanut allekirjoitus-)-sivustoon tai käyttämällä [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Dropbox for Business-sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769508.png "Skenaario")



##<a name="enabling-the-application-integration-for-dropbox-for-business"></a>Dropbox for Business-sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön Dropbox for Business-sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-dropbox-for-business-perform-the-following-steps"></a>Dropbox for Business-sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **Dropbox for Business**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC701010.png "Sovelluksen valikoima")

7.  Valitse tulokset-ruudussa **Dropbox for Business**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Dropbox for Business] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC701011.png "Dropbox for Business")

##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen Dropbox for Business käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.

Kun nämä toimet, sinun on base-64-koodattu varmenteen lataaminen että Dropbox for Business-vuokraajan. Jos et ole tottunut näin, katso, [miten voit muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa **Dropbox for Business** -sovellus integrointi-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC749323.png "Määritä kertakirjautuminen")

2.  Valitse **kuinka haluat käyttäjät kirjautumaan Dropbox for Business** -sivulla **Microsoft Azure AD kertakirjautumisen**Valitse ja valitse sitten **Seuraava**.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC749327.png "Määritä kertakirjautuminen")

3.  Tee **Määrittäminen sovelluksen URL-osoite** -sivulla seuraavat toimet:

    a. Sign, että Dropbox for business-vuokraajan. 

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769509.png "Määritä kertakirjautuminen")

    b. Valitse vasemmalla puolella olevassa siirtymisruudussa, **Järjestelmänvalvojan konsolissa**. 

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769510.png "Määritä kertakirjautuminen")

    c-näppäinyhdistelmää. Valitse **todennus** **Hallintakonsoli**vasemmanpuoleisessa siirtymisruudussa. 

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769511.png "Määritä kertakirjautuminen")

    d. **Kertakirjautumisen** -osassa Ota käyttöön **kertakirjautumisen**ja valitse sitten **Lisää** Laajenna sisältö.  

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769512.png "Määritä kertakirjautuminen")

    e. Kopioi URL-osoite, **käyttäjät voivat kirjautua sisään kirjoittamalla henkilön sähköpostiosoite tai he voivat siirtyä suoraan**vieressä. 

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769513.png "Määritä kertakirjautuminen")

    f. Liitä URL-osoite Azure perinteinen portaalin URL-Osoitteen **Kirjautuminen DropBox for business** -tekstiruutuun. 

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769514.png "Määritä kertakirjautuminen")  



4. Valitse **Määritä kertakirjautumisen Dropbox for Business-palvelussa** -sivulla valitsemalla **Lataa**ja Tallenna sertifikaattitiedosto tietokoneeseen.  

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769515.png "Määritä kertakirjautuminen")


5. Suorita oman Dropbox for Business-vuokraajan **todennus** -sivulla **Single Sign** -osassa seuraavasti: 

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769516.png "Määritä kertakirjautuminen")

    a. Valitse **pakollinen**.

    b. Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen Dropbox for Business-palvelussa** -valintasivun **kirjautumisen kirjautumissivun URL-osoite** -arvo kopioi ja liitä se sitten **Kirjaudu sisään URL** -tekstiruutuun.


    c-näppäinyhdistelmää. Luo **Base-64-koodattu** tiedosto ladattu varmennetta. 

    > [AZURE.TIP] Lisätietoja on artikkelissa [muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o).


    d. **"Valitse varmenne"** -painiketta ja selaamalla oman **base-64-koodattu sertifikaattitiedosto**.


    e. Valitse Viimeistele määritys-että DropBox for Business-vuokraajan **"tallentaa muutokset-** painiketta.


6. Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje. 

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC749329.png "Määritä kertakirjautuminen")



##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjän valmistelu Active Directory-käyttäjätilien Dropbox for Business.


### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Voit määrittää käyttäjän valmistelu toimimalla seuraavasti:

1. Valitse Azure perinteinen Portal **Dropbox for Business** -sovellus integrointi-sivulla **Määritä käyttäjän valmistelu** -valintaikkunan avaaminen **Määritä käyttäjän valmistelu** .

2. Valitse Ota käyttöön käyttäjän valmisteleminen DropBox for Business-sivu, ota käyttöön käyttäjien valmistelu Avaa kirjautuminen dropboxiin linkitettävän Azure AD-valintaikkunaa käyttäen.  

    ![Käyttäjän valmistelu] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769517.png "Käyttäjän valmistelu")

3. Valitse **Kirjaudu sisään dropboxiin linkittää Azure AD** -valintaikkunassa Kirjaudu oman Dropbox for Business-vuokraajan. 

    ![Käyttäjän valmistelu] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769518.png "Käyttäjän valmistelu")



4. Valitse **Salli** myöntää Azure AD käyttämään dropboxiin. 

    ![Käyttäjän valmistelu] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769519.png "Käyttäjän valmistelu")



5. Lopeta määritystä, napsauta **Valmis** -painiketta.  

    ![Käyttäjän valmistelu] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769520.png "Käyttäjän valmistelu")




##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-dropbox-for-business-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä Dropbox for Business, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse **Dropbox for Business **-sovelluksen integrointi-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769521.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC767830.png "Kyllä")
  


Pitäisi nyt Odota 10 minuuttia ja varmista, että tili on synkronoitu Dropbox for Business.

Vahvistus ensimmäisessä vaiheessa voit tarkistaa valmistelun tilan napsauttamalla **raporttinäkymät-ikkunan** Azure perinteinen portaalissa **Dropbox for Business** sovelluksen integrointi-sivu.

![Käyttäjien määrittäminen] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769522.png "Käyttäjien määrittäminen")


Jakson valmistelu onnistuneesti suoritettujen käyttäjän ilmaistaan liittyvät tila.

![Käyttäjien määrittäminen] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769523.png "Käyttäjien määrittäminen")


Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli.
Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).




## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)
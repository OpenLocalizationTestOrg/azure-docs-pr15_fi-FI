<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Sprinklr | Microsoft Azure" 
    description="Opettele käyttämään Sprinklr Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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
    ms.date="09/19/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-sprinklr"></a>Opetusohjelma: Sprinklr Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja Sprinklr integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Sprinklr palvelutili
  
Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt Sprinklr Azure AD-käyttäjien saa oikeuden kertakirjauksen Sprinklr yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Sprinklr sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-sprinklr-tutorial/IC782900.png "Skenaario")

##<a name="enabling-the-application-integration-for-sprinklr"></a>Sprinklr sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön Sprinklr sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-sprinklr-perform-the-following-steps"></a>Sprinklr sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-sprinklr-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-sprinklr-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-sprinklr-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-sprinklr-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **Sprinklr**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-sprinklr-tutorial/IC782901.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **Sprinklr**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Sprinklr] (./media/active-directory-saas-sprinklr-tutorial/IC782902.png "Sprinklr")

##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen siitä, miten käyttäjät voivat todentaa Sprinklr käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.  
Tämän toimintosarjan osana on base-64-koodattu sertifikaattitiedosto luomiseen.  
Jos et ole tottunut näin, katso, [miten voit muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **Sprinklr** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-sprinklr-tutorial/IC782903.png "Määritä kertakirjautuminen")

2.  **Miten käyttäjät voivat kirjautua Sprinklr haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-sprinklr-tutorial/IC782904.png "Määritä kertakirjautuminen")

3.  Kirjoita **Määritä sovelluksen URL-osoite** -sivulla **Sprinklr merkki-URL** -tekstiruutuun URL-osoite käyttämällä seuraavaa kaavaa "*https://\<vuokraajan nimi\>. sprinklr.com*", ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-sprinklr-tutorial/IC782905.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  Valitse **Määritä kertakirjautumisen osoitteessa Sprinklr** -sivulla Lataa sertifikaatti, valitsemalla **Lataa**, ja Tallenna sertifikaattitiedosto tietokoneeseen.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-sprinklr-tutorial/IC782906.png "Määritä kertakirjautuminen")

5.  Eri selainikkunassa Kirjaudu Sprinklr yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

6.  Siirry **hallinnan \> asetukset**.

    ![Hallinta] (./media/active-directory-saas-sprinklr-tutorial/IC782907.png "Hallinta")

7.  Siirry **hallinta kumppanin \> kertakirjauksen** Valitse vasemmanpuoleisesta ruudusta.

    ![Kumppanin hallinta] (./media/active-directory-saas-sprinklr-tutorial/IC782908.png "Kumppanin hallinta")

8.  Valitse **+ Kertakirjauksen lisäosat**.

    ![Yksi merkki-kuvakkeet] (./media/active-directory-saas-sprinklr-tutorial/IC782909.png "Yksi merkki-kuvakkeet")

9.  Tee **Kertakirjautuminen** -sivulla seuraavat toimet:

    ![Yksi merkki-kuvakkeet] (./media/active-directory-saas-sprinklr-tutorial/IC782910.png "Yksi merkki-kuvakkeet")

    1.  Kirjoita **nimi** -tekstiruutuun kokoonpanosi nimi (esimerkiksi: *WAADSSOTest*).
    2.  Valitse **käytössä**.
    3.  Valitse **Käytä uutta SSO varmennetta**.
    4.  Luo **base-64-koodattu** tiedosto ladattu varmennetta.  

        >[AZURE.TIP] Lisätietoja on artikkelissa [muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o)

    5.  Avaa base-64-koodattu varmennetta Muistiossa, kopioi se sisällön Leikepöydän ja liitä se **Tunnistetietojen toimittaja varmenne** -tekstiruutuun
    6.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Sprinklr** -valintasivun **Tunnistetietojen Toimittajan tunnus** -arvon kopioi ja liitä se sitten **Kohteen tunnus** -tekstiruutuun.
    7.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Sprinklr** -valintasivun **Remote sisäänkirjautumisen URL-osoite** -arvo kopioi ja liitä se sitten **Tunnistetietojen toimittaja sisäänkirjautumisen URL-osoite** -tekstiruutuun.
    8.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Sprinklr** -valintasivun **Remote Kirjaudu URL-osoite** -arvo kopioi ja liitä se sitten **Jäsenyyden tarjoajan Kirjaudu URL-Osoitteen** tekstiruutu.
    9.  **SAML käyttäjätyyppi**, valitse **vahvistus on käyttäjän "s sprinklr.com käyttäjänimi**.
    10. **SAML tunnus sijaintiin**Valitse **Käyttäjätunnus on aihe-lauseen nimen tunniste-osaan**.
    11. Sulje **Tallenna**.

        ![SAML] (./media/active-directory-saas-sprinklr-tutorial/IC782911.png "SAML")

10. Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-sprinklr-tutorial/IC782912.png "Määritä kertakirjautuminen")

##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
AAD käyttäjät voivat kirjautua ne on valmisteltava käytön Sprinklr-sovelluksessa.  
Tässä osassa käsitellään sisällä Sprinklr AAD Käyttäjätilien luominen.

###<a name="to-provision-a-user-account-in-sprinklr-perform-the-following-steps"></a>Valmistelu Sprinklr käyttäjän, toimi seuraavasti:

1.  Kirjaudu sisään Sprinklr yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

2.  Siirry **hallinnan \> asetukset**.

    ![Hallinta] (./media/active-directory-saas-sprinklr-tutorial/IC782907.png "Hallinta")

3.  Siirry **Hallitse asiakkaan \> käyttäjien** vasemmanpuoleisesta ruudusta.

    ![Asetukset] (./media/active-directory-saas-sprinklr-tutorial/IC782914.png "Asetukset")

4.  Valitse **Lisää käyttäjä**.

    ![Asetukset] (./media/active-directory-saas-sprinklr-tutorial/IC782915.png "Asetukset")

5.  **Käyttäjän muokkaaminen** -valintaikkunassa seuraavasti:

    ![Käyttäjän muokkaaminen] (./media/active-directory-saas-sprinklr-tutorial/IC782916.png "Käyttäjän muokkaaminen")

    1.  **Sähköposti**-, **Etunimi** ja **Sukunimi** tekstiruutujen, kirjoita Azure AD-käyttäjätilin tiedot haluat säätää.
    2.  Valitse **salasana käytöstä**.
    3.  Valitse **kieli**.
    4.  Valitse **käyttäjätyyppi**.
    5.  Valitse **Päivitä**.

    >[AZURE.IMPORTANT] **Salasanan käytöstä** valitsemista antaa käyttäjän kirjautua sisään Liittoutuminen kautta.

6.  Siirry **rooli**ja toimi sitten seuraavasti:

    ![Kumppanin roolit] (./media/active-directory-saas-sprinklr-tutorial/IC782917.png "Kumppanin roolit")

    1.  Valitse **Yleiset** -luettelosta **kaikki\_käyttöoikeuksien**.
    2.  Valitse **Päivitä**.

>[AZURE.NOTE] Voit käyttää mitä tahansa muita Sprinklr käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä Sprinklr valmistelu Azure AD-käyttäjätilejä.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-sprinklr-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä Sprinklr, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **Sprinklr **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-sprinklr-tutorial/IC782918.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-sprinklr-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).
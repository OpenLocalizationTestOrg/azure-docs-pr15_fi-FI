<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Spring CM | Microsoft Azure" 
    description="Opettele käyttämään Spring CM Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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

#<a name="tutorial-azure-active-directory-integration-with-spring-cm"></a>Opetusohjelma: Spring CM Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttää, miten voit määrittää kertakirjautumisen Azure Active Directory- ja SpringCM välillä.
  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Käytössä SpringCM kertakirjautumisen tilaus
  
Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt SpringCM Azure Active Directory-käyttäjät saa oikeuden kertakirjautumisen käyttämällä AAD Access-paneeli.

1.  SpringCM sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-spring-cm-tutorial/IC797044.png "Skenaario")

##<a name="enabling-the-application-integration-for-springcm"></a>SpringCM sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön SpringCM sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-springcm-perform-the-following-steps"></a>SpringCM sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-spring-cm-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-spring-cm-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-spring-cm-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-spring-cm-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **SpringCM**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-spring-cm-tutorial/IC797045.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **SpringCM**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![SpringCM] (./media/active-directory-saas-spring-cm-tutorial/IC797046.png "SpringCM")

##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa kuvataan antaa käyttäjien todentamiseen SpringCM niiden tilillä Azure Active Directory federation perusteella SAML-protokollan avulla.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **SpringCM** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-spring-cm-tutorial/IC797047.png "Määritä kertakirjautuminen")

2.  **Miten käyttäjät voivat kirjautua SpringCM haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-spring-cm-tutorial/IC797048.png "Määritä kertakirjautuminen")

3.  **Määritä sovelluksen URL-osoite** -sivulla **SpringCM merkki-URL** -tekstiruutuun käyttää käyttäjien kirjautua SpringCM sovelluksen URL-osoite ja valitse sitten **Seuraava**. 

    Sovelluksen URL-osoite on SpringCM vuokraajan URL-osoite (esimerkiksi: *https://na11.springcm.com/atlas/SSO/SSOEndpoint.ashx?aid=16826*):

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-spring-cm-tutorial/IC797049.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  Valitse **Määritä kertakirjautumisen osoitteessa SpringCM** -sivulla Lataa sertifikaatti, **Lataa**ja Tallenna sertifikaattitiedosto paikallisesti tietokoneeseen.

    ![Määrittää yksittäisen rekisteröityminen] (./media/active-directory-saas-spring-cm-tutorial/IC797050.png "Määrittää yksittäisen rekisteröityminen")

5.  Eri selainikkunassa Kirjaudu **SpringCM** yrityksen sivustoon järjestelmänvalvojana.

6.  Ylä-valikosta **Siirry**, **asetukset**ja sitten **Tiliasetukset** -osassa **SAML SSO**.

    ![SAML SSO] (./media/active-directory-saas-spring-cm-tutorial/IC797051.png "SAML SSO")

7.  Tunnistetietojen toimittaja määritykset-osasta toimimalla seuraavasti:

    ![Tunnistetietojen toimittaja määritys] (./media/active-directory-saas-spring-cm-tutorial/IC797052.png "Tunnistetietojen toimittaja määritys")

    1.  Voit ladata ladatut Azure Active Directory-varmenteen, valitsemalla **Valitse myöntäjän sertifikaattia** tai **Muuta myöntäjä**.
    2.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa SpringCM** -sivulla kopioi **Myöntäjä URL-osoite** -arvo ja liitä se sitten **myöntäjä** -tekstiruutuun.
    3.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa SpringCM** -sivulla **Singel Sign-palvelun URL-osoite** -arvo kopioi ja liitä se sitten **Service-palvelun (SP) aloittanut päätepisteen** tekstiruutu.
    4.  **SAML käytössä**Valitse **Ota käyttöön**.
    5.  Valitse **Tallenna**.

8.  Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Määrittää yksittäisen rekisteröityminen] (./media/active-directory-saas-spring-cm-tutorial/IC797053.png "Määrittää yksittäisen rekisteröityminen")

##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Jos haluat ottaa käyttöön Azure Active Directory-käyttäjät voivat kirjautua sisään SpringCM, ne on valmisteltava SpringCM kyselyjä.  
SpringCM valmistelu on manuaalinen tehtävä.

>[AZURE.NOTE] Lisätietoja on artikkelissa [luominen ja SpringCM käyttäjän muokkaaminen](http://knowledge.springcm.com/create-and-edit-a-springcm-user)

###<a name="to-provision-a-user-account-to-springcm-perform-the-following-steps"></a>Valmistelu SpringCM käyttäjätili, toimi seuraavasti:

1.  Kirjaudu sisään **SpringCM** yrityksesi sivuston järjestelmänvalvojana.

2.  Valitse **Siirry**ja valitse sitten **Osoitteisto**.

    ![Käyttäjän luominen] (./media/active-directory-saas-spring-cm-tutorial/IC797054.png "Käyttäjän luominen")

3.  Valitse **Luo käyttäjä**.

4.  Valitse **käyttäjärooli**.

5.  Valitse **aktivoinnin sähköpostin lähettäminen**.

6.  Kirjoita etunimi, Sukunimi ja sähköpostiosoite kelvollinen Azure Active Directory-käyttäjätilin haluat säännöstä liittyvät tekstiruutujen kyselyjä.

7.  Lisää käyttäjä **käyttöoikeusryhmän**.

8.  Valitse **Tallenna**.

>[AZURE.NOTE] Voit käyttää mitä tahansa muita SpringCM käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä SpringCM säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-springcm-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä SpringCM, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **SpringCM** sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-spring-cm-tutorial/IC797055.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-spring-cm-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).





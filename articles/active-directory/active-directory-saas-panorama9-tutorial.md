<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Panorama9 | Microsoft Azure" 
    description="Opettele käyttämään Panorama9 Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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
    ms.date="09/29/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-panorama9"></a>Opetusohjelma: Panorama9 Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja Panorama9 integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Käytössä Panorama9 kertakirjautumisen tilaus
  
Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt Panorama9 Azure AD-käyttäjien saa oikeuden kertakirjauksen Panorama9 yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Panorama9 sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-panorama9-tutorial/IC790016.png "Skenaario")
##<a name="enabling-the-application-integration-for-panorama9"></a>Panorama9 sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön Panorama9 sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-panorama9-perform-the-following-steps"></a>Panorama9 sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-panorama9-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-panorama9-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-panorama9-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-panorama9-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **Panorama9**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-panorama9-tutorial/IC790017.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **Panorama9**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Panorama9] (./media/active-directory-saas-panorama9-tutorial/IC790018.png "Panorama9")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen Panorama9 käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.  
Kertakirjautumisen varten Panorama9 määrittäminen edellyttää, että allekirjoitus arvon noutaa varmenne.  
Jos et ole tottunut näin, katso, [miten voit hakea varmenteen allekirjoitus-arvon](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **Panorama9** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-panorama9-tutorial/IC790019.png "Kertakirjautumisen määrittäminen")

2.  **Miten käyttäjät voivat kirjautua Panorama9 haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-panorama9-tutorial/IC790020.png "Kertakirjautumisen määrittäminen")

3.  Kirjoita **Määrittäminen sovelluksen URL-osoite** -sivulla **Panorama9 merkki-URL** -tekstiruutuun käyttää käyttäjien kirjautua Panorama9 URL-osoite (esimerkiksi: "*https://dashboard.panorama9.com/saml/access/3262*"), ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-panorama9-tutorial/IC790021.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  Valitse **Määritä kertakirjautumisen osoitteessa Panorama9** -sivulla Lataa sertifikaatti, **Lataa**ja tallennettava se paikallisesti tietokoneesta.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-panorama9-tutorial/IC790022.png "Kertakirjautumisen määrittäminen")

5.  Eri selainikkunassa Kirjaudu Panorama9 yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

6.  -Sivun työkaluriviltä **hallinta**ja valitse sitten **tunnisteet**.

    ![Tunnisteet] (./media/active-directory-saas-panorama9-tutorial/IC790023.png "Tunnisteet")

7.  Valitse **tunnisteet** -valintaikkunasta **Kertakirjautumisen**.

    ![Kertakirjautuminen] (./media/active-directory-saas-panorama9-tutorial/IC790024.png "Kertakirjautuminen")

8.  Valitse **asetukset** -kohdassa toimimalla seuraavasti:

    ![Asetukset] (./media/active-directory-saas-panorama9-tutorial/IC790025.png "Asetukset")

    1.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Panorama9** -valintasivun **Sign-palvelun URL** -arvon kopioi ja liitä se sitten **tunnistetietojen toimittaja URL** -tekstiruutuun.
    2.  Viedyn varmenteen **allekirjoitusta** kopioi ja liitä se **varmenteen tunnisteen** tekstiruutu.  

        >[AZURE.TIP]Lisätietoja on artikkelissa [noutamisesta varmenteen allekirjoitusarvo](http://youtu.be/YKQF266SAxI)

    3.  Valitse **Tallenna**.

9.  Azure AD perinteinen-portaalissa Valitse Sign määritysten vahvistus ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-panorama9-tutorial/IC790026.png "Kertakirjautumisen määrittäminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Jos haluat ottaa käyttöön Azure AD-käyttäjät voivat kirjautua sisään Panorama9, ne on valmisteltava Panorama9 kyselyjä.  
Panorama9 valmistelu on manuaalinen tehtävä.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Voit määrittää käyttäjän valmistelu toimimalla seuraavasti:

1.  Kirjaudu sisään **Panorama9** yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

2.  Valitse ylä-valikossa **hallinta**ja valitse sitten **käyttäjät**.

    ![Käyttäjät] (./media/active-directory-saas-panorama9-tutorial/IC790027.png "Käyttäjät")

3.  Valitse **+**.

4.  Käyttäjän tiedot-osassa seuraavasti:

    ![Käyttäjät] (./media/active-directory-saas-panorama9-tutorial/IC790028.png "Käyttäjät")

    1.  Kirjoita **sähköpostiosoite** -tekstiruutuun haluat säätää kelvollinen Azure Active Directory-käyttäjän sähköpostiosoite.
    2.  Valitse **Tallenna**.

>[AZURE.NOTE]Voit käyttää mitä tahansa muita Panorama9 käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä Panorama9 säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-panorama9-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä Panorama9, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **Panorama9** sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-panorama9-tutorial/IC790029.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-panorama9-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).
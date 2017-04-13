<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Bonus.ly | Microsoft Azure" 
    description="Opettele käyttämään Bonus.ly Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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

#<a name="tutorial-azure-active-directory-integration-with-bonusly"></a>Opetusohjelma: Bonus.ly Azure Active Directory-integrointi

Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja Bonus.ly integrointi. Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Testi-Bonus.ly palvelutili

Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Bonus.ly sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-bonus-tutorial/IC773679.png "Skenaario")
##<a name="enabling-the-application-integration-for-bonusly"></a>Bonus.ly sovelluksen integrointi ottaminen käyttöön

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön Bonus.ly sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-bonusly-perform-the-following-steps"></a>Bonus.ly sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Ottaa käyttöön kertakirjautumisen] (./media/active-directory-saas-bonus-tutorial/IC773680.png "Ottaa käyttöön kertakirjautumisen")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-bonus-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-bonus-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-bonus-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **Bonus.ly**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-bonus-tutorial/IC773681.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **Bonus.ly**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Bonusly] (./media/active-directory-saas-bonus-tutorial/IC773682.png "Bonusly")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen Bonus.ly käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.  
Kertakirjautumisen varten Bonus.ly määrittäminen edellyttää, että allekirjoitus arvon noutaa varmenne.  
Jos et ole tottunut näin, katso, [miten voit hakea varmenteen allekirjoitus-arvon](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **Bonus.ly** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-bonus-tutorial/IC749323.png "Määritä kertakirjautuminen")

2.  **Miten käyttäjät voivat kirjautua Bonus.ly haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-bonus-tutorial/IC773683.png "Määritä kertakirjautuminen")

3.  Kirjoita **Määritä sovelluksen URL-osoite** -sivulla **Bonus.ly vuokraajan URL** -tekstiruutuun URL-osoite käyttämällä seuraavaa kaavaa "*https://\<vuokraajan nimi\>. Bonus.LY*", ja valitse sitten **Seuraava**: 

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-bonus-tutorial/IC773684.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  Valitse **Määritä kertakirjautumisen osoitteessa Bonus.ly** -sivulla valitsemalla **Lataa**ja Tallenna sertifikaattitiedosto paikallisesti kuin **c:\\Bonusly.cer**.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-bonus-tutorial/IC773685.png "Määritä kertakirjautuminen")

5.  **Bonus.ly** vuokraajan kirjautuminen eri selainikkunassa.

6.  Yläkulmassa työkalurivin **asetukset**ja valitse sitten **integroinnit ja sovellukset**.

    ![Bonusly] (./media/active-directory-saas-bonus-tutorial/IC773686.png "Bonusly")

7.  Valitse **Kertakirjautumisen** **SAML**.

8.  Suorita **SAML** -sivulla seuraavat toimet:

    ![Bonusly] (./media/active-directory-saas-bonus-tutorial/IC773687.png "Bonusly")

    1.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Bonus.ly** -valintasivun **Remote sisäänkirjautumisen URL-osoite** -arvo kopioi ja liitä se sitten **IdP SSO kohde-URL** -tekstiruutuun.
    2.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Bonus.ly** -valintasivun **Myöntäjän tunnus** -arvon kopioi ja liitä se sitten **IdP myöntäjä** tekstiruutu.
    3.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Bonus.ly** -valintasivun **Remote sisäänkirjautumisen URL-osoite** -arvo kopioi ja liitä se sitten **IdP kirjautuminen URL** -tekstiruutuun.
    4.  Viedyn varmenteen **allekirjoitusta** kopioi ja liitä se **Varmenteen tunnisteen** tekstiruutu.

        >[AZURE.TIP] Lisätietoja on artikkelissa [noutamisesta varmenteen allekirjoitusarvo](http://youtu.be/YKQF266SAxI)

9.  Valitse **Tallenna**.

10. Microsoft Azure perinteinen-portaalissa Valitse määritysten vahvistus ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-bonus-tutorial/IC773689.png "Määritä kertakirjautuminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu

Jos haluat ottaa käyttöön Azure AD-käyttäjät voivat kirjautua sisään Bonus.ly, ne on valmisteltava Bonus.ly kyselyjä.  
Bonus.ly valmistelu on manuaalinen tehtävä.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Voit määrittää käyttäjän valmistelu toimimalla seuraavasti:

1.  Web-selainikkunassa Kirjaudu sisään Bonus.ly alihallintaan.

2.  Valitse **asetukset**

    ![Asetukset] (./media/active-directory-saas-bonus-tutorial/IC781041.png "Asetukset")

3.  Valitse **käyttäjät ja bonukset** -välilehti.

    ![Käyttäjien ja bonukset] (./media/active-directory-saas-bonus-tutorial/IC781042.png "Käyttäjien ja bonukset")

4.  Valitse **käyttäjien hallinta**.

    ![Käyttäjien hallinta] (./media/active-directory-saas-bonus-tutorial/IC781043.png "Käyttäjien hallinta")

5.  Valitse **Lisää käyttäjä**.

    ![Käyttäjän lisääminen] (./media/active-directory-saas-bonus-tutorial/IC781044.png "Käyttäjän lisääminen")

6.  Valitse **Lisää käyttäjä** -valintaikkunassa seuraavasti:

    ![Käyttäjän lisääminen] (./media/active-directory-saas-bonus-tutorial/IC781045.png "Käyttäjän lisääminen")

    1.  Kirjoita "**sähköpostin**, **Etunimi**, **Sukunimi**" kelvollinen AAD tilin haluat säännöstä liittyvät tekstiruutujen kyselyjä.
    2.  Valitse **Tallenna**.

    >[AZURE.NOTE] AAD tilin omistaja saavat sähköpostiviestin, joka sisältää linkin Vahvista tili, ennen kuin se on aktiivinen.

>[AZURE.NOTE] Voit käyttää mitä tahansa muita Bonus.ly käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä Bonus.ly säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen

Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-bonusly-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä Bonus.ly, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration Bonus.ly sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-bonus-tutorial/IC773690.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-bonus-tutorial/IC767830.png "Kyllä")

Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).

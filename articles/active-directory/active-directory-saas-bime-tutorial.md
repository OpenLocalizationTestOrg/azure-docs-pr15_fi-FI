<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Bime | Microsoft Azure" 
    description="Opettele käyttämään Bime Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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

#<a name="tutorial-azure-active-directory-integration-with-bime"></a>Opetusohjelma: Bime Azure Active Directory-integrointi

Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja Bime integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Bime palvelutili

Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt Bime Azure AD-käyttäjien saa oikeuden kertakirjauksen Bime yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.

Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Bime sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-bime-tutorial/IC775552.png "Skenaario")
##<a name="enabling-the-application-integration-for-bime"></a>Bime sovelluksen integrointi ottaminen käyttöön

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön Bime sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-bime-perform-the-following-steps"></a>Bime sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-bime-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-bime-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-bime-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-bime-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **Bime**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-bime-tutorial/IC775553.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **Bime**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Bime] (./media/active-directory-saas-bime-tutorial/IC775554.png "Bime")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen Bime käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.  
Kertakirjausta varten Bime määrittäminen edellyttää, että allekirjoitus arvon noutaa varmenne.  
Jos et ole tottunut näin, katso, [miten voit hakea varmenteen allekirjoitus-arvon](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **Bime** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-bime-tutorial/IC771709.png "Määritä kertakirjautuminen")

2.  **Miten käyttäjät voivat kirjautua Bime haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-bime-tutorial/IC775555.png "Kertakirjautumisen määrittäminen")

3.  Kirjoita **Määritä sovelluksen URL-osoite** -sivulla **Bime merkki-URL** -tekstiruutuun URL-osoite käyttämällä seuraavaa kaavaa "*https://\<vuokraajan nimi\>. Bimeapp.com*", ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-bime-tutorial/IC775556.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  Valitse **Määritä kertakirjautumisen osoitteessa Bime** -sivulla Lataa sertifikaatti, **Lataa**ja Tallenna sertifikaattitiedosto paikallisesti kuin **c:\\Bime.cer**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-bime-tutorial/IC775557.png "Kertakirjautumisen määrittäminen")

5.  Eri selainikkunassa Kirjaudu Bime yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

6.  Valitse työkaluriviltä **järjestelmänvalvoja**ja sitten **tili**.

    ![Järjestelmänvalvoja] (./media/active-directory-saas-bime-tutorial/IC775558.png "Järjestelmänvalvoja")

7.  Tilin määritys-sivulla toimimalla seuraavasti:

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-bime-tutorial/IC775559.png "Kertakirjautumisen määrittäminen")

    1.  Valitse **Ota käyttöön SAML todennusta**.
    2.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Bime** -valintasivun **Remote sisäänkirjautumisen URL-osoite** -arvo kopioi ja liitä se sitten **Remote kirjautuminen URL** -tekstiruutuun.
    3.  Viedyn varmenteen **allekirjoitusta** kopioi ja liitä se **Varmenteen tunnisteen** tekstiruutu.  

        >[AZURE.TIP] Lisätietoja on artikkelissa [noutamisesta varmenteen allekirjoitusarvo](http://youtu.be/YKQF266SAxI)

    4.  Valitse **Tallenna**.

8.  Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-bime-tutorial/IC775560.png "Kertakirjautumisen määrittäminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu

Jos haluat ottaa käyttöön Azure AD-käyttäjät voivat kirjautua sisään Bime, ne on valmisteltava Bime kyselyjä.  
Bime valmistelu on manuaalinen tehtävä.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Voit määrittää käyttäjän valmistelu toimimalla seuraavasti:

1.  Kirjaudu sisään **Bime** alihallintaan.

2.  Valitse työkaluriviltä **järjestelmänvalvoja**, ja **käyttäjät**.

    ![Järjestelmänvalvoja] (./media/active-directory-saas-bime-tutorial/IC775561.png "Järjestelmänvalvoja")

3.  **Käyttäjien luettelosta**Valitse **Lisää uusi käyttäjä** ("+").

    ![Käyttäjät] (./media/active-directory-saas-bime-tutorial/IC775562.png "Käyttäjät")

4.  Valitse **Käyttäjätiedot** -sivulla toimimalla seuraavasti:

    ![Käyttäjätiedot] (./media/active-directory-saas-bime-tutorial/IC775563.png "Käyttäjätiedot")

    1.  Kirjoita etunimi, Sukunimi, kirjaudu sisään, sähköpostin kelvollinen AAD tilin haluat säätää.
    2.  Valitse Tallenna.

>[AZURE.NOTE] Voit käyttää mitä tahansa muita Bime käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä Bime säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen

Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-bime-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä Bime, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **Bime **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-bime-tutorial/IC775564.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-bime-tutorial/IC767830.png "Kyllä")

Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).

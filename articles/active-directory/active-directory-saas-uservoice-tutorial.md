<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Palautesivuston | Microsoft Azure" 
    description="Opettele käyttämään Palautesivuston Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!." 
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
    ms.date="09/11/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-uservoice"></a>Opetusohjelma: Palautesivuston Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja Palautesivuston integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Palautesivuston palvelutili
  
Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt Palautesivuston Azure AD-käyttäjien saa oikeuden kertakirjauksen Palautesivuston yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Palautesivuston sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-uservoice-tutorial/IC777514.png "Skenaario")

##<a name="enabling-the-application-integration-for-uservoice"></a>Palautesivuston sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön Palautesivuston sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-uservoice-perform-the-following-steps"></a>Palautesivuston sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-uservoice-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-uservoice-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-uservoice-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-uservoice-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **Palautesivuston**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-uservoice-tutorial/IC777513.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **Palautesivuston**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Palautesivuston] (./media/active-directory-saas-uservoice-tutorial/IC777810.png "Palautesivuston")

##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen Palautesivuston käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.  
Kertakirjautumisen varten Palautesivuston määrittäminen edellyttää, että allekirjoitus arvon noutaa varmenne.  
Jos et ole tottunut näin, katso, [miten voit hakea varmenteen allekirjoitus-arvon](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **Palautesivuston** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-uservoice-tutorial/IC777515.png "Määritä kertakirjautuminen")

2.  **Miten käyttäjät voivat kirjautua Palautesivuston haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-uservoice-tutorial/IC777516.png "Määritä kertakirjautuminen")

3.  Kirjoita **Määritä sovelluksen URL-osoite** -sivulla **Palautesivuston merkki-URL** -tekstiruutuun URL-osoite käyttämällä seuraavaa kaavaa "*https://\<vuokraajan nimi\>. UserVoice.com*", ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-uservoice-tutorial/IC777517.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  Valitse **Määritä kertakirjautumisen osoitteessa UserVoice** -sivulla Lataa sertifikaatti, **Lataa**ja Tallenna sertifikaattitiedosto paikallisesti kuin **c:\\UserVoice.cer**.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-uservoice-tutorial/IC777518.png "Määritä kertakirjautuminen")

5.  Eri selainikkunassa Kirjaudu Palautesivuston yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

6.  Yläkulmassa työkalurivin asetukset ja valitse sitten valikosta Web-portaaliin.

    ![Asetukset] (./media/active-directory-saas-uservoice-tutorial/IC777519.png "Asetukset")

7.  Valitse **Web-portaalin** -välilehdessä **‑valintaruudunkäyttäjän todentaminen** ‑osassa, voit avata **Muokkaa käyttöoikeuksien** valintasivun **muokkaaminen**

    ![Web-portaalissa] (./media/active-directory-saas-uservoice-tutorial/IC777520.png "Web-portaalissa")

8.  Valitse **Muokkaa käyttöoikeuksien** -sivulla toimimalla seuraavasti:

    ![Käyttöoikeuksien muokkaaminen] (./media/active-directory-saas-uservoice-tutorial/IC777521.png "Käyttöoikeuksien muokkaaminen")

    1.  Valitse **Kertakirjautuminen (SSO)**.
    2.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa UserVoice** -valintasivun **Remote sisäänkirjautumisen URL-osoite** -arvo kopioi ja liitä se sitten **SSO Remote-kirjautuminen** -tekstiruutu.
    3.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa UserVoice** -valintasivun **Remote Kirjaudu URL-osoite** -arvo kopioi ja liitä se sitten **SSO Remote Sign-Out tekstiruutu**.
    4.  Kopioi **allekirjoitusta** viedyn varmenteen ja liitä se sitten **nykyisen Varmenteen SHA1 tunnisteen** tekstiruutu.  

        >[AZURE.TIP] Lisätietoja on artikkelissa [noutamisesta varmenteen allekirjoitusarvo](http://youtu.be/YKQF266SAxI)

    5.  Valitse **Tallenna todennusasetukset**.

9.  Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-uservoice-tutorial/IC777522.png "Määritä kertakirjautuminen")

##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Jotta voit kirjautua Palautesivuston Azure AD-käyttäjien käyttöön, ne on valmisteltava Palautesivuston kyselyjä.  
UserVoice-valmistelu on manuaalinen tehtävä.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Valmistelu käyttäjätilit, toimi seuraavasti:

1.  Kirjaudu sisään **Palautesivuston** alihallintaan.

2.  Valitse **asetukset**.

    ![Asetukset] (./media/active-directory-saas-uservoice-tutorial/IC777811.png "Asetukset")

3.  Valitse **Yleiset**.

4.  Valitse **agenttien ja käyttöoikeudet**.

    ![Tekijöiden ja käyttöoikeudet] (./media/active-directory-saas-uservoice-tutorial/IC777812.png "Tekijöiden ja käyttöoikeudet")

5.  Valitse **Lisää järjestelmänvalvojat**.

    ![Lisää järjestelmänvalvojat] (./media/active-directory-saas-uservoice-tutorial/IC777813.png "Lisää järjestelmänvalvojat")

6.  Valitse **Kutsu valvojat** -valintaikkunassa seuraavasti:

    ![Kutsu järjestelmänvalvojat] (./media/active-directory-saas-uservoice-tutorial/IC777814.png "Kutsu järjestelmänvalvojat")

    1.  Kirjoita sähköpostiosoite, jos haluat säätää, ja valitse sitten **Lisää**tilin sähköpostit, texbox.
    2.  Valitse **Kutsu**.

>[AZURE.NOTE] Voit käyttää mitä tahansa muita Palautesivuston käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä Palautesivuston säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-uservoice-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä Palautesivuston, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **UserVoice **-sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-uservoice-tutorial/IC777523.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-uservoice-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).
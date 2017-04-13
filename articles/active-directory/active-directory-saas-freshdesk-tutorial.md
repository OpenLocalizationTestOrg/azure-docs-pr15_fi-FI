<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Freshdesk | Microsoft Azure" 
    description="Opettele käyttämään Freshdesk Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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

#<a name="tutorial-azure-active-directory-integration-with-freshdesk"></a>Opetusohjelma: Freshdesk Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja Freshdesk integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Freshdesk palvelutili
  
Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt Freshdesk Azure AD-käyttäjien saa oikeuden kertakirjauksen Freshdesk yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Freshdesk sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-freshdesk-tutorial/IC776761.png "Skenaario")
##<a name="enabling-the-application-integration-for-freshdesk"></a>Freshdesk sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön Freshdesk sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-freshdesk-perform-the-following-steps"></a>Freshdesk sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-freshdesk-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-freshdesk-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-freshdesk-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-freshdesk-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **Freshdesk**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-freshdesk-tutorial/IC776762.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **Freshdesk**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Freshdesk] (./media/active-directory-saas-freshdesk-tutorial/IC776763.png "Freshdesk")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen Freshdesk käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.  
Kertakirjautumisen varten Freshdesk määrittäminen edellyttää, että allekirjoitus arvon noutaa varmenne.  
Jos et ole tottunut näin, katso, [miten voit hakea varmenteen allekirjoitus-arvon](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **Freshdesk** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-freshdesk-tutorial/IC776764.png "Kertakirjautumisen määrittäminen")

2.  **Miten käyttäjät voivat kirjautua Freshdesk haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-freshdesk-tutorial/IC776765.png "Kertakirjautumisen määrittäminen")

3.  Kirjoita **Määritä sovelluksen URL-osoite** -sivulla **Freshdesk merkki-URL** -tekstiruutuun URL-osoite käyttämällä seuraavaa kaavaa "*https://\<vuokraajan nimi\>. Freshdesk.com*", ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-freshdesk-tutorial/IC776766.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  Valitse **Määritä kertakirjautumisen osoitteessa Freshdesk** -sivulla Lataa sertifikaatti, **Lataa**ja Tallenna sertifikaattitiedosto paikallisesti kuin **c:\\Freshdesk.cer**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-freshdesk-tutorial/IC776767.png "Kertakirjautumisen määrittäminen")

5.  Eri selainikkunassa Kirjaudu Freshdesk yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

6.  Valitse ylä-valikossa Valitse **järjestelmänvalvoja**.

    ![Järjestelmänvalvoja] (./media/active-directory-saas-freshdesk-tutorial/IC776768.png "Järjestelmänvalvoja")

7.  Valitse **Yleisasetukset** -välilehdessä **Suojaus**.

    ![Tietoturva] (./media/active-directory-saas-freshdesk-tutorial/IC776769.png "Tietoturva")

8.  Valitse **Suojaus** -kohdassa toimimalla seuraavasti:

    ![Kertakirjautuminen] (./media/active-directory-saas-freshdesk-tutorial/IC776770.png "Kertakirjautuminen")

    1.  Valitse **Single Sign-(SSO)**, **Valitse**.
    2.  Valitse **SAML SSO**.
    3.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Freshdesk** -valintasivun **Remote sisäänkirjautumisen URL-osoite** -arvo kopioi ja liitä se sitten **SAML kirjautuminen URL** -tekstiruutuun.
    4.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Freshdesk** -valintasivun **Remote Kirjaudu URL-osoite** -arvo kopioi ja liitä se sitten **Kirjaudu ulos URL** -tekstiruutuun.
    5.  Kopioi **allekirjoitusta** viedyn varmenteen ja liitä se sitten **Suojaus varmenteen tunnisteen** tekstiruutu.  

        >[AZURE.TIP]Lisätietoja on artikkelissa [noutamisesta varmenteen allekirjoitusarvo](http://youtu.be/YKQF266SAxI)

    6.  Valitse **Tallenna**.

9.  Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-freshdesk-tutorial/IC776771.png "Kertakirjautumisen määrittäminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Jos haluat ottaa käyttöön Azure AD-käyttäjät voivat kirjautua sisään Freshdesk, ne on valmisteltava Freshdesk kyselyjä.  
Freshdesk valmistelu on manuaalinen tehtävä.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Valmistelu käyttäjätilit, toimi seuraavasti:

1.  Kirjaudu sisään **Freshdesk** alihallintaan.

2.  Valitse ylä-valikossa Valitse **järjestelmänvalvoja**.

    ![Järjestelmänvalvoja] (./media/active-directory-saas-freshdesk-tutorial/IC776772.png "Järjestelmänvalvoja")

3.  Valitse **Yleisasetukset** -välilehdessä **agenttien vuoksi**.

    ![Agenttien vuoksi] (./media/active-directory-saas-freshdesk-tutorial/IC776773.png "Agenttien vuoksi")

4.  Valitse **Uusi agentti**.

    ![Uusi agentti] (./media/active-directory-saas-freshdesk-tutorial/IC776774.png "Uusi agentti")

5.  Valitse edustajan tiedot-valintaikkunassa seuraavasti:

    ![Edustajan tiedot] (./media/active-directory-saas-freshdesk-tutorial/IC776775.png "Edustajan tiedot")

    1.  Kirjoita **KokoNimi** -tekstiruutuun haluat säätää Azure AD-tilin nimi.
    2.  Kirjoita **sähköpostiosoite** -tekstiruutuun Azure AD-sähköpostiosoite, jos haluat säätää Azure AD-tilin.
    3.  Kirjoita **otsikko** -tekstiruutuun sen Azure AD-tilin haluat säätää.
    4.  Valitse **agenttien vuoksi rooli**ja valitse sitten **Liitä**.
    5.  Valitse **Tallenna**.
    
        >[AZURE.NOTE] Azure AD-tilin omistaja saavat sähköpostiviestin, joka sisältää linkin Vahvista tili, ennen kuin se on aktivoitu.

>[AZURE.NOTE] Voit käyttää mitä tahansa muita Freshdesk käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä Freshdesk säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-freshdesk-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä Freshdesk, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **Freshdesk **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-freshdesk-tutorial/IC776776.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-freshdesk-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).
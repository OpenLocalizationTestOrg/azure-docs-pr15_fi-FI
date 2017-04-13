<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Huddle | Microsoft Azure" 
    description="Opettele käyttämään Huddle Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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

#<a name="tutorial-azure-active-directory-integration-with-huddle"></a>Opetusohjelma: Huddle Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja Huddle integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Käytössä Huddle kertakirjautumisen tilaus
  
Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt Huddle Azure AD-käyttäjien saa oikeuden kertakirjauksen Huddle yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Huddle sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-huddle-tutorial/IC787830.png "Kertakirjautumisen määrittäminen")
##<a name="enabling-the-application-integration-for-huddle"></a>Huddle sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön Huddle sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-huddle-perform-the-following-steps"></a>Huddle sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-huddle-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-huddle-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-huddle-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-huddle-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **Huddle**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-huddle-tutorial/IC787831.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **Huddle**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Huddle] (./media/active-directory-saas-huddle-tutorial/IC787832.png "Huddle")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen Huddle käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portal-sivulla **Huddle** sovelluksen integrointi, **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-huddle-tutorial/IC787833.png "Kertakirjautumisen määrittäminen")

2.  **Miten Haluaisitko kirjautumaan Huddle käyttäjät** -sivulla **Microsoft Azure AD kertakirjautumisen**Valitse ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-huddle-tutorial/IC787834.png "Kertakirjautumisen määrittäminen")

3.  **Määritä sovelluksen URL-osoite** -sivulla **Huddle merkki-URL** -tekstiruutuun käyttämällä seuraavaa kaavaa "*http://company.huddle.com*" Huddle alihallintaan URL-osoite ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-huddle-tutorial/IC787835.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  Valitse **Määritä kertakirjautumisen osoitteessa Huddle** -sivulla toimimalla seuraavasti:

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-huddle-tutorial/IC787836.png "Kertakirjautumisen määrittäminen")

    1.  Valitse **Lataa**ja Tallenna sertifikaattitiedosto tietokoneeseen.
    2.  Kopioi **Myöntäjä URL-osoite** -arvo, **SAML SSO URL-osoite** -arvo ja ladatut varmenne ja lähettää niitä Huddle tukihenkilöstön.

    >[AZURE.NOTE] Kertakirjautumisen täytyy ottaa käyttöön Huddle-tukeen.
Näyttöön tulee ilmoitus, kun määritykset on suoritettu.

5.  Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-huddle-tutorial/IC787837.png "Kertakirjautumisen määrittäminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Jos haluat ottaa käyttöön Azure AD-käyttäjät voivat kirjautua sisään Huddle, ne on valmisteltava Huddle kyselyjä.  
Huddle valmistelu on manuaalinen tehtävä.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Voit määrittää käyttäjän valmistelu toimimalla seuraavasti:

1.  Kirjaudu **Huddle** yritys-sivustoon järjestelmänvalvojana.

2.  Valitse **Työtila**.

3.  Valitse **henkilöiden \> kutsuminen**.

    ![Henkilöt] (./media/active-directory-saas-huddle-tutorial/IC787838.png "Henkilöt")

4.  **Luo uusi kutsu** -kohdassa toimimalla seuraavasti:

    ![Uusi kutsu] (./media/active-directory-saas-huddle-tutorial/IC787839.png "Uusi kutsu")

    1.  Valitse **Valitse ryhmä, jos haluat kutsua muita henkilöitä** -luettelosta **ryhmä**.
    2.  Kirjoita **Sähköpostiosoite** kelvollinen AAD tilin haluat säätää olevaan liittyvät tekstiruutuun.
    3.  Valitse **Kutsu**.

    >[AZURE.NOTE] Azure AD-tilin omistaja saavat sähköpostiviestin, mukaan lukien linkin Vahvista tili, ennen kuin se on aktiivinen.

>[AZURE.NOTE] Voit käyttää mitä tahansa muita Huddle käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä Huddle valmistelu AAD käyttäjätilit.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-huddle-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä Huddle, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  **Huddle **sovelluksen integrointi, napsauta sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-huddle-tutorial/IC787840.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-huddle-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).
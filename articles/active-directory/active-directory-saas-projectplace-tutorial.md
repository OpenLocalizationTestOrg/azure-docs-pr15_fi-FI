<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Projectplace | Microsoft Azure" 
    description="Opettele käyttämään Projectplace Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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
    ms.date="09/26/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-projectplace"></a>Opetusohjelma: Projectplace Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja Projectplace integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Käytössä Projectplace kertakirjautumisen tilaus
  
Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt Projectplace Azure AD-käyttäjien saa oikeuden kertakirjauksen Projectplace yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Projectplace sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-projectplace-tutorial/IC790217.png "Skenaario")
##<a name="enabling-the-application-integration-for-projectplace"></a>Projectplace sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön Projectplace sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-projectplace-perform-the-following-steps"></a>Projectplace sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-projectplace-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-projectplace-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-projectplace-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-projectplace-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **Projectplace**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-projectplace-tutorial/IC790218.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **Projectplace**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![ProjectPlace] (./media/active-directory-saas-projectplace-tutorial/IC790219.png "ProjectPlace")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen Projectplace käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **Projectplace** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Määrittää yksittäisen rekisteröityminen] (./media/active-directory-saas-projectplace-tutorial/IC790220.png "Määrittää yksittäisen rekisteröityminen")

2.  **Miten käyttäjät voivat kirjautua Projectplace haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Määrittää yksittäisen rekisteröityminen] (./media/active-directory-saas-projectplace-tutorial/IC790221.png "Määrittää yksittäisen rekisteröityminen")

3.  Kirjoita **Määritä sovelluksen URL-osoite** -sivulla **Projectplace merkki-URL** -tekstiruutuun ProjectPlace vuokraajan URL-osoite (esimerkiksi: "*http://company.projectplace.com*"), ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-projectplace-tutorial/IC790222.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  Valitse **Määritä kertakirjautumisen osoitteessa Projectplace** -sivulla valitsemalla **Lataa metatiedot**ja Tallenna tietokoneessa metatiedot-tiedostoon.

    ![Määrittää yksittäisen rekisteröityminen] (./media/active-directory-saas-projectplace-tutorial/IC790223.png "Määrittää yksittäisen rekisteröityminen")

5.  Lähetä MetadataFile määrittelee Projectplace-tukeen.

    >[AZURE.NOTE] Sign-määritys on Projectplace tukihenkilöstön tekemän. Näyttöön tulee ilmoitus, kun määritykset on suoritettu.

6.  Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Määrittää yksittäisen rekisteröityminen] (./media/active-directory-saas-projectplace-tutorial/IC790227.png "Määrittää yksittäisen rekisteröityminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Jos haluat ottaa käyttöön Azure AD-käyttäjät voivat kirjautua sisään Projectplace, ne on valmisteltava Projectplace kyselyjä.  
Projectplace valmistelu on manuaalinen tehtävä.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Valmistelu käyttäjätilit, toimi seuraavasti:

1.  Kirjaudu sisään **Projectplace** yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

2.  Siirry kohtaan **henkilöt**, ja valitse sitten **jäsenet**.

    ![Henkilöt] (./media/active-directory-saas-projectplace-tutorial/IC790228.png "Henkilöt")

3.  Valitse **Lisää jäsen**.

    ![Jäsenten lisääminen] (./media/active-directory-saas-projectplace-tutorial/IC790232.png "Jäsenten lisääminen")

4.  Valitse **Lisää jäsen** -osassa seuraavasti:

    ![Uusien jäsenten] (./media/active-directory-saas-projectplace-tutorial/IC790233.png "Uusien jäsenten")

    1.  Kirjoita **Uudet jäsenet** -tekstiruutuun haluat säännöstä kyselyjä liittyvät tekstiruutujen kelvollinen AAD tilin sähköpostiosoite.
    2.  Valitse **Lähetä**

        >[AZURE.NOTE] Vahvista tili, ennen kuin se on aktiivinen linkki sähköpostiviesti lähetetään Azure Active Directory-tilin omistaja.
    
>[AZURE.NOTE]Voit käyttää mitä tahansa muita Projectplace käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä Projectplace säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-projectplace-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä Projectplace, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **Projectplace **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-projectplace-tutorial/IC790234.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-projectplace-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).
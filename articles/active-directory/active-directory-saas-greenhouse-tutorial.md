<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi kasvihuonekaasujen | Microsoft Azure" 
    description="Opettele käyttämään kasvihuonekaasujen Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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

#<a name="tutorial-azure-active-directory-integration-with-greenhouse"></a>Opetusohjelma: Kasvihuonekaasujen Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja kasvihuonekaasujen integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Kasvihuonekaasujen yhden Sign-tilauksen
  
Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt kasvihuonekaasujen Azure AD-käyttäjien saa oikeuden kertakirjauksen kasvihuonekaasujen yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Kasvihuonekaasujen sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-greenhouse-tutorial/IC790783.png "Skenaario")
##<a name="enabling-the-application-integration-for-greenhouse"></a>Kasvihuonekaasujen sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön kasvihuonekaasujen sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-greenhouse-perform-the-following-steps"></a>Kasvihuonekaasujen sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-greenhouse-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-greenhouse-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-greenhouse-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-greenhouse-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **kasvihuonekaasujen**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-greenhouse-tutorial/IC790784.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **kasvihuonekaasujen**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Kasvihuonekaasujen] (./media/active-directory-saas-greenhouse-tutorial/IC790785.png "Kasvihuonekaasujen")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen kasvihuonekaasujen käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **kasvihuonekaasujen** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-greenhouse-tutorial/IC790786.png "Määritä kertakirjautuminen")

2.  **Miten käyttäjät voivat kirjautua kasvihuonekaasujen haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-greenhouse-tutorial/IC790787.png "Määritä kertakirjautuminen")

3.  **Määritä sovelluksen URL-osoite** -sivulla **Merkki-URL** -tekstiruutuun Kirjoita käyttämällä seuraavaa kaavaa "*https://company.greenhouse.io*" URL-osoite ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-greenhouse-tutorial/IC790788.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  Valitse **Määritä kertakirjautumisen osoitteessa kasvihuonekaasujen** -sivulla valitsemalla **Lataa metatiedot**ja Tallenna paikallisesti tietokoneen tiedoston metatiedot.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-greenhouse-tutorial/IC790789.png "Määritä kertakirjautuminen")

5.  Lähetä edelleen kasvihuonekaasujen tukihenkilöstön metatietojen kyseinen tiedosto.

    >[AZURE.NOTE] Kertakirjautumisen pitää ottaa käyttöön kasvihuonekaasujen-tukeen.

6.  Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-greenhouse-tutorial/IC790790.png "Määritä kertakirjautuminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Jotta voit kirjautua kasvihuonekaasujen Azure AD-käyttäjien käyttöön, ne on valmisteltava kasvihuonekaasujen kyselyjä.  
Kasvihuonekaasujen valmistelu on manuaalinen tehtävä.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Valmistelu käyttäjätilit, toimi seuraavasti:

1.  Kirjaudu sisään **kasvihuonekaasujen** yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

2.  Valitse ylä-valikossa **määrittäminen**ja valitse sitten **käyttäjät**.

    ![Käyttäjät] (./media/active-directory-saas-greenhouse-tutorial/IC790791.png "Käyttäjät")

3.  Valitse **uusille käyttäjille**.

    ![Uusi käyttäjä] (./media/active-directory-saas-greenhouse-tutorial/IC790792.png "Uusi käyttäjä")

4.  **Lisää uusi käyttäjä** -osassa seuraavasti:

    ![Uuden käyttäjän lisääminen] (./media/active-directory-saas-greenhouse-tutorial/IC790793.png "Uuden käyttäjän lisääminen")

    1.  Kirjoita sähköpostiosoite, jos haluat säätää kelvollinen Azure Active Directory-tilin **Enter käyttäjien sähköpostien** -tekstiruutuun.
    2.  Valitse **Tallenna**.
        
        >[AZURE.NOTE] Azure Active Directory-tilin haltijan saavat sähköpostiviestin, mukaan lukien linkin Vahvista tili, ennen kuin se on aktiivinen.

>[AZURE.NOTE] Voit käyttää mitä tahansa muita kasvihuonekaasujen käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä kasvihuonekaasujen säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-greenhouse-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä kasvihuonekaasujen, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **kasvihuonekaasujen **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-greenhouse-tutorial/IC790794.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-greenhouse-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).
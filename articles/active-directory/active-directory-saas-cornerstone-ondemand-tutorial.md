<properties 
    pageTitle="Opetusohjelma: Perusta OnDemand Azure Active Directory-integrointi | Microsoft Azure" 
    description="Opettele käyttämään perusta OnDemand Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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

#<a name="tutorial-azure-active-directory-integration-with-cornerstone-ondemand"></a>Opetusohjelma: Perusta OnDemand Azure Active Directory-integrointi

Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja perusta OnDemand integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Perusta OnDemand palvelutili

Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt perusta OnDemand Azure AD-käyttäjien saa oikeuden kertakirjauksen perusta OnDemand yrityksen sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.

Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Perusta OnDemand sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781593.png "Skenaario")
##<a name="enabling-the-application-integration-for-cornerstone-ondemand"></a>Perusta OnDemand sovelluksen integrointi ottaminen käyttöön

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön perusta OnDemand sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-cornerstone-ondemand-perform-the-following-steps"></a>Perusta OnDemand sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **perusta ondemand**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781594.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **Perusta OnDemand**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Perusta OnDemand] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781595.png "Perusta OnDemand")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen perusta OnDemand käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **Perusta OnDemand** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Ottaa käyttöön kertakirjautumisen] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781596.png "Ottaa käyttöön kertakirjautumisen")

2.  **Miten käyttäjät voivat kirjautua perusta OnDemand haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Microsoft Azure AD-Single Sign-On] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781597.png "Microsoft Azure AD-Single Sign-On")

3.  **Määritä sovelluksen URL-osoite** -sivulla **Perusta OnDemand merkki-URL** -tekstiruutuun Kirjoita käyttämällä seuraavaa kaavaa "*http://company.csod.com*" URL-osoite ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781598.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  **Määritä kertakirjautumisen osoitteessa perusta OnDemand** -sivulla Lataa sertifikaatti, valitse **Lataa**, ja Tallenna sertifikaattitiedosto paikallisesti tietokoneeseen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781599.png "Kertakirjautumisen määrittäminen")

5.  Lähetä perusta-OnDemand seuraavat kohteet tekniseen tukeen:

    1.  Ladatun varmenne
    2.  **Remote sisäänkirjautumisen URL-osoite** -arvo
    3.  **Remote Kirjaudu URL-osoite** -arvo.

    >[AZURE.NOTE] Kertakirjautumisen varten on määritettävä perusta OnDemand tukihenkilöstön mukaan.
Kun määritykset on suoritettu, tukitiimiltä saavat ilmoituksen.

6.  Valitse Sign määritysten vahvistus ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781600.png "Kertakirjautumisen määrittäminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu

Jotta voit kirjautua perusta OnDemand Azure AD-käyttäjien käyttöön, ne on valmisteltava perusta OnDemand kyselyjä.  
Perusta OnDemand valmistelu on manuaalinen tehtävä.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Voit määrittää käyttäjän valmistelu toimimalla seuraavasti:

1.  Lähetä tiedot (esimerkiksi: nimi, Emial) haluat säätää perusta-OnDemand Azure AD käyttäjästä tekniseen tukeen.

>[AZURE.NOTE] Voit käyttää mitä tahansa muita perusta OnDemand käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä perusta OnDemand säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen

Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-cornerstone-ondemand-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä perusta OnDemand, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **Perusta OnDemand **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC775564.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781601.png "Käyttäjien määrittäminen")

Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).

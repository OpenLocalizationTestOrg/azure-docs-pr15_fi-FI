<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Overdriveen kirjat | Microsoft Azure" 
    description="Opettele käyttämään Overdriveen kirjojen Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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

#<a name="tutorial-azure-active-directory-integration-with-overdrive-books"></a>Opetusohjelma: Azure Active Directory-integrointi Overdriveen kirjat
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja Overdriveen integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Käytössä Overdriveen kertakirjautumisen tilaus
  
Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt Overdriveen Azure AD-käyttäjien saa oikeuden kertakirjauksen Overdriveen yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Overdriveen sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-overdrive-books-tutorial/IC784462.png "Skenaario")
##<a name="enabling-the-application-integration-for-overdrive"></a>Overdriveen sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön Overdriveen sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-overdrive-perform-the-following-steps"></a>Overdriveen sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-overdrive-books-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-overdrive-books-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-overdrive-books-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-overdrive-books-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **Overdriveen**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-overdrive-books-tutorial/IC784463.png "Sovelluksen valikoima")

7.  Valitse tulokset-ruudun Valitse **Overdriveen**ja valitse sitten **Valmis** , voit lisätä sovelluksen.

    ![Overdriveen] (./media/active-directory-saas-overdrive-books-tutorial/IC799950.png "Overdriveen")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen voit antaa käyttäjien todentamiseen Overdriveen käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **Overdriveen** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Ottaa käyttöön kertakirjautumisen] (./media/active-directory-saas-overdrive-books-tutorial/IC784465.png "Ottaa käyttöön kertakirjautumisen")

2.  **Miten Haluaisitko kirjautumaan Overdriveen käyttäjät** -sivulla **Microsoft Azure AD kertakirjautumisen**Valitse ja valitse sitten **Seuraava**.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-overdrive-books-tutorial/IC784466.png "Määritä kertakirjautuminen")

3.  **Määritä sovelluksen URL-osoite** -sivulla **Overdriveen merkki-URL** -tekstiruutuun Kirjoita käyttämällä seuraavaa kaavaa "*http://mslibrarytest.libraryreserve.com*" URL-osoite ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-overdrive-books-tutorial/IC784467.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  **Määritä kertakirjautumisen osoitteessa Overdriveen** sivun, lataa metatieto-tiedosto ja lähetä se Overdriveen tukihenkilöstön.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-overdrive-books-tutorial/IC784468.png "Määritä kertakirjautuminen")

    >[AZURE.NOTE]Overdriveen tukihenkilöstön kertakirjautumisen voit määrittää ja lähettää sinulle ilmoituksen, kun määritykset on suoritettu.

5.  Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-overdrive-books-tutorial/IC784469.png "Määritä kertakirjautuminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Ei ei toimi, voit määrittää käyttäjän valmisteleminen Overdriveen.  
Kun määritetty käyttäjä yrittää kirjautua Overdriveen, Overdriveen tilin luodaan automaattisesti tarvittaessa.

>[AZURE.NOTE]Voit käyttää mitä tahansa muita Overdriveen käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä Overdriveen säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-overdrive-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä Overdriveen, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse **Overdriveen **sovelluksen integrointi-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-overdrive-books-tutorial/IC784470.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-overdrive-books-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).
<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Kontiki | Microsoft Azure" 
    description="Opettele käyttämään Kontiki Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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

#<a name="tutorial-azure-active-directory-integration-with-kontiki"></a>Opetusohjelma: Kontiki Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja Kontiki integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Käytössä Kontiki kertakirjautumisen tilaus
  
Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt Kontiki Azure AD-käyttäjien saa oikeuden kertakirjauksen Kontiki yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Kontiki sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-kontiki-tutorial/IC790235.png "Skenaario")
##<a name="enabling-the-application-integration-for-kontiki"></a>Kontiki sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön Kontiki sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-kontiki-perform-the-following-steps"></a>Kontiki sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-kontiki-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-kontiki-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-kontiki-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-kontiki-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **Kontiki**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-kontiki-tutorial/IC790236.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **Kontiki**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Kontiki] (./media/active-directory-saas-kontiki-tutorial/IC790237.png "Kontiki")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen Kontiki käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **Kontiki** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Määrittää yksittäisen rekisteröityminen] (./media/active-directory-saas-kontiki-tutorial/IC790238.png "Määrittää yksittäisen rekisteröityminen")

2.  **Miten käyttäjät voivat kirjautua Kontiki haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Määrittää yksittäisen rekisteröityminen] (./media/active-directory-saas-kontiki-tutorial/IC790239.png "Määrittää yksittäisen rekisteröityminen")

3.  Kirjoita **Määritä sovelluksen URL-osoite** -sivulla **Kontiki merkki-URL** -tekstiruutuun käyttämä käyttäjät kirjautumaan Kontiki URL-osoite (esimerkiksi: "*https://company.mc.eval.kontiki.com/*"), ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-kontiki-tutorial/IC790240.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  Valitse **Määritä kertakirjautumisen osoitteessa Kontiki** -sivulla valitsemalla **Lataa metatiedot**ja Tallenna tietokoneessa metatiedot-tiedostoon.

    ![Määrittää yksittäisen rekisteröityminen] (./media/active-directory-saas-kontiki-tutorial/IC790241.png "Määrittää yksittäisen rekisteröityminen")

5.  Lähetä MetadataFile määrittelee Kontiki-tukeen.

    >[AZURE.NOTE] Sign-määritys on Kontiki tukihenkilöstön tekemän. Näyttöön tulee ilmoitus, kun määritykset on suoritettu.

6.  Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Määrittää yksittäisen rekisteröityminen] (./media/active-directory-saas-kontiki-tutorial/IC790242.png "Määrittää yksittäisen rekisteröityminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Ei ei toimi, voit määrittää käyttäjän Kontiki valmistelu.  
Kun määritetty käyttäjä yrittää kirjautua käyttämällä access-paneelin Kontiki, Kontiki tarkistaa, onko käyttäjän olemassa.  
Jos ei ole käyttäjätiliä käytettävissä vielä, se luodaan automaattisesti Kontiki mukaan.
##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-kontiki-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä Kontiki, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **Kontiki **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-kontiki-tutorial/IC790243.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-kontiki-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).
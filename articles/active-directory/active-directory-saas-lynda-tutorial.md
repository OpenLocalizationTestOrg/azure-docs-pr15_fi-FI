<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Lynda.com | Microsoft Azure" 
    description="Opettele käyttämään Lynda.com Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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

#<a name="tutorial-azure-active-directory-integration-with-lyndacom"></a>Opetusohjelma: Lynda.com Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja Lynda.com integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Lynda.com palvelutili
  
Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt Lynda.com Azure AD-käyttäjien saa oikeuden kertakirjauksen Lynda.com yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Lynda.com sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-lynda-tutorial/IC781046.png "Skenaario")
##<a name="enabling-the-application-integration-for-lyndacom"></a>Lynda.com sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön Lynda.com sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-lyndacom-perform-the-following-steps"></a>Lynda.com sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-lynda-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-lynda-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-lynda-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-lynda-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **Lynda.com**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-lynda-tutorial/IC777524.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **Lynda.com**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Lynda.com] (./media/active-directory-saas-lynda-tutorial/IC777525.png "Lynda.com")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen Lynda.com käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.

>[AZURE.IMPORTANT]Jos haluat voi määrittää kertakirjautumisen Lynda.com vuokraajan, ota ensin Lynda.com tekniseen tukeen ja Hae tämä toiminto on käytössä.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **Lynda.com** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-lynda-tutorial/IC777526.png "Määritä kertakirjautuminen")

2.  **Miten käyttäjät voivat kirjautua Lynda.com haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-lynda-tutorial/IC777527.png "Määritä kertakirjautuminen")

3.  Kirjoita **Määritä sovelluksen URL-osoite** -sivulla **Lynda.com merkki-URL** -tekstiruutuun Lynda.com vuokraajan URL-osoite (esimerkiksi: *https://shib.lynda.com/Shibboleth.sso/InCommon?providerId=https://sts.windows-ppe.net/6247032d-9415-403c-b72b-277e3fb6f2c8/&target= https://shib.lynda.com/InCommon*), ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-lynda-tutorial/IC781047.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  Valitse **Määritä kertakirjautumisen osoitteessa Lynda.com** -sivulla lataamaan metatietojen valitsemalla **Lataa metatietojen**, ja Tallenna sertifikaattitiedosto paikallisesti tietokoneeseen.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-lynda-tutorial/IC777529.png "Määritä kertakirjautuminen")

5.  Lähetä Lynda.com tukihenkilöstön ladatut metatieto-tiedosto. Lynda.com tukihenkilöstön tekee kertakirjautuminen määritykset puolestasi.

6.  Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-lynda-tutorial/IC777530.png "Määritä kertakirjautuminen")
##<a name="configuring-user-provisioning"></a>Käyttäjän valmistelu määrittäminen
  
Ei ei toimi, voit määrittää käyttäjän Lynda.com valmistelu.  
Kun määritetty käyttäjä yrittää kirjautua käyttämällä access-paneelin Lynda.com, Lynda.com tarkistaa, onko käyttäjän olemassa.  
Jos ei ole käyttäjätiliä käytettävissä vielä, se luodaan automaattisesti Lynda.com mukaan.

>[AZURE.NOTE]Voit käyttää mitä tahansa muita Lynda.com käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä Lynda.com säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-lyndacom-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä Lynda.com, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **Lynda.com **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-lynda-tutorial/IC777531.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-lynda-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).
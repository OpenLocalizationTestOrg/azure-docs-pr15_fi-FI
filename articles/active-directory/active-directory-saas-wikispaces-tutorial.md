<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Wikispaces | Microsoft Azure" 
    description="Opettele käyttämään Wikispaces Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!." 
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

#<a name="tutorial-azure-active-directory-integration-with-wikispaces"></a>Opetusohjelma: Wikispaces Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja Wikispaces integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Käytössä Wikispaces kertakirjautumisen tilaus
  
Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt Wikispaces Azure AD-käyttäjien saa oikeuden kertakirjauksen Wikispaces yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Wikispaces sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Sceanrio] (./media/active-directory-saas-wikispaces-tutorial/IC787182.png "Sceanrio")

##<a name="enabling-the-application-integration-for-wikispaces"></a>Wikispaces sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön Wikispaces sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-wikispaces-perform-the-following-steps"></a>Wikispaces sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-wikispaces-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-wikispaces-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-wikispaces-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-wikispaces-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **Wikispaces**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-wikispaces-tutorial/IC787186.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **Wikispaces**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Wikispaces] (./media/active-directory-saas-wikispaces-tutorial/IC787187.png "Wikispaces")

##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen Wikispaces käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **Wikispaces** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-wikispaces-tutorial/IC787188.png "Kertakirjautumisen määrittäminen")

2.  **Miten käyttäjät voivat kirjautua Wikispaces haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-wikispaces-tutorial/IC787189.png "Kertakirjautumisen määrittäminen")

3.  **Määritä sovelluksen URL-osoite** -sivulla **Wikispaces merkki-URL** -tekstiruutuun Kirjoita käyttämällä seuraavaa kaavaa "*http://company.wikispaces.net*" URL-osoite ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-wikispaces-tutorial/IC787190.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  Valitse **Määritä kertakirjautumisen osoitteessa Wikispaces** -sivulla valitsemalla **Lataa metatiedot**ja Tallenna tietokoneessa metatiedot-tiedostoon.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-wikispaces-tutorial/IC787191.png "Kertakirjautumisen määrittäminen")

5.  Lähetä MetadataFile määrittelee Wikispaces-tukeen.

    >[AZURE.NOTE] Sign-määritys on Wikispaces tukihenkilöstön tekemän. Näyttöön tulee ilmoitus, kun määritykset on suoritettu.

6.  Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-wikispaces-tutorial/IC787192.png "Kertakirjautumisen määrittäminen")

##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Jos haluat ottaa käyttöön Azure AD-käyttäjät voivat kirjautua sisään Wikispaces, ne on valmisteltava Wikispaces kyselyjä.  
Wikispaces valmistelu on manuaalinen tehtävä.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Valmistelu käyttäjätilit, toimi seuraavasti:

1.  Kirjaudu sisään **Wikispaces** yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

2.  Siirry **jäsenet**.

    ![Jäsenet] (./media/active-directory-saas-wikispaces-tutorial/IC787193.png "Jäsenet")

3.  Valitse **Kutsu henkilöitä**.

    ![Kutsu käyttäjiä] (./media/active-directory-saas-wikispaces-tutorial/IC787194.png "Kutsu käyttäjiä")

4.  **Kutsu henkilöitä** -osassa seuraavasti:

    ![Kutsu käyttäjiä] (./media/active-directory-saas-wikispaces-tutorial/IC787208.png "Kutsu käyttäjiä")

    1.  Kirjoita **käyttäjänimet tai sähköpostiosoitetta kirjain** kelvollinen AAD tilin haluat säännöstä liittyvät tekstiruutujen kyselyjä.
    2.  Valitse **Lähetä**.  

        >[AZURE.NOTE] Azure Active Directory-tilin omistaja saa ilmoituksen sähköpostitse, mukaan lukien linkin Vahvista tili, ennen kuin se on aktiivinen.

>[AZURE.NOTE] Voit käyttää mitä tahansa muita Wikispaces käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä Wikispaces säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-wikispaces-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä Wikispaces, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **Wikispaces **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-wikispaces-tutorial/IC787195.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-wikispaces-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).
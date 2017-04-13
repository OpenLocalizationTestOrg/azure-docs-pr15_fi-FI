<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Mindflash | Microsoft Azure" 
    description="Opettele käyttämään Mindflash Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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

#<a name="tutorial-azure-active-directory-integration-with-mindflash"></a>Opetusohjelma: Mindflash Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja Mindflash integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Käytössä Mindflash kertakirjautumisen tilaus
  
Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt Mindflash Azure AD-käyttäjien saa oikeuden kertakirjauksen Mindflash yrityksen sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Mindflash sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-mindflash-tutorial/IC787132.png "Skenaario")
##<a name="enabling-the-application-integration-for-mindflash"></a>Mindflash sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön Mindflash sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-mindflash-perform-the-following-steps"></a>Mindflash sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-mindflash-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-mindflash-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-mindflash-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-mindflash-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **Mindflash**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-mindflash-tutorial/IC787133.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **Mindflash**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Mindflash] (./media/active-directory-saas-mindflash-tutorial/IC787134.png "Mindflash")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen Mindflash käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **Mindflash** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-mindflash-tutorial/IC787135.png "Kertakirjautumisen määrittäminen")

2.  **Miten käyttäjät voivat kirjautua Mindflash haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-mindflash-tutorial/IC787136.png "Kertakirjautumisen määrittäminen")

3.  **Määritä sovelluksen URL-osoite** -sivulla **Merkki-URL** -tekstiruutuun Kirjoita käyttämällä seuraavaa kaavaa "*http://company.mindflash.com*" URL-osoite ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-mindflash-tutorial/IC787137.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  Valitse **Määritä kertakirjautumisen osoitteessa Mindflash** -sivulla valitsemalla **Lataa metatiedot**ja Tallenna tietokoneessa metatiedot-tiedostoon.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-mindflash-tutorial/IC787138.png "Kertakirjautumisen määrittäminen")

5.  Lähetä MetadataFile määrittelee Mindflash-tukeen.

    >[AZURE.NOTE] Sign-määritys on Mindflash tukihenkilöstön tekemän. Näyttöön tulee ilmoitus, kun määritykset on suoritettu.

6.  Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-mindflash-tutorial/IC787139.png "Kertakirjautumisen määrittäminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Jos haluat ottaa käyttöön Azure AD-käyttäjät voivat kirjautua sisään Mindflash, ne on valmisteltava Mindflash kyselyjä.  
Mindflash valmistelu on manuaalinen tehtävä.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Valmistelu käyttäjätilit, toimi seuraavasti:

1.  Kirjaudu sisään **Mindflash** yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

2.  Siirry **käyttäjien hallinta**.

    ![Käyttäjien hallinta] (./media/active-directory-saas-mindflash-tutorial/IC787140.png "Käyttäjien hallinta")

3.  Valitse **Lisää käyttäjiä**, ja valitse sitten **Uusi**.

4.  **Lisää uusia käyttäjiä** -osassa seuraavasti:

    ![Lisää uusia käyttäjiä] (./media/active-directory-saas-mindflash-tutorial/IC787141.png "Lisää uusia käyttäjiä")

    1.  Kirjoita **Etunimi**, **Sukunimi** ja **sähköpostin** kelvollinen AAD tilin haluat säännöstä liittyvät tekstiruutujen kyselyjä.
    2.  Valitse **Lisää**.

>[AZURE.NOTE]Voit käyttää mitä tahansa muita Mindflash käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä Mindflash säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-mindflash-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä Mindflash, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **Mindflash **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-mindflash-tutorial/IC787142.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-mindflash-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).
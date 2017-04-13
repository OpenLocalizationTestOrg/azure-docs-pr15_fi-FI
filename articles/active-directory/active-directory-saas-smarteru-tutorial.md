<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi SmarterU | Microsoft Azure" 
    description="Opettele käyttämään SmarterU Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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
    ms.date="09/19/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-smarteru"></a>Opetusohjelma: SmarterU Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja SmarterU integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   SmarterU palvelutili
  
Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt SmarterU Azure AD-käyttäjien saa oikeuden kertakirjauksen SmarterU yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  SmarterU sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-smarteru-tutorial/IC777320.png "Skenaario")

##<a name="enabling-the-application-integration-for-smarteru"></a>SmarterU sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön SmarterU sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-smarteru-perform-the-following-steps"></a>SmarterU sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-smarteru-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-smarteru-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-smarteru-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-smarteru-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **SmarterU**.

    ![Sovelluksen fallery] (./media/active-directory-saas-smarteru-tutorial/IC777321.png "Sovelluksen fallery")

7.  Valitse tulosruudussa **SmarterU**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![SmarterU] (./media/active-directory-saas-smarteru-tutorial/IC777322.png "SmarterU")

##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen SmarterU käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **SmarterU** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-smarteru-tutorial/IC777323.png "Kertakirjautumisen määrittäminen")

2.  **Miten käyttäjät voivat kirjautua SmarterU haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-smarteru-tutorial/IC777324.png "Kertakirjautumisen määrittäminen")

3.  Valitse **Määritä kertakirjautumisen osoitteessa SmarterU** -sivulla Lataa metatietojen, valitse **Lataa metatiedot**ja sitten tiedot tiedosto paikallisesti nimellä **c:\\SmarterUMetaData.cer**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-smarteru-tutorial/IC777325.png "Kertakirjautumisen määrittäminen")

4.  Eri selainikkunassa Kirjaudu SmarterU yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

5.  Valitse yläreunassa olevalla työkalurivillä, **Tilin asetukset**.

    ![Tilin asetukset] (./media/active-directory-saas-smarteru-tutorial/IC777326.png "Tilin asetukset")

6.  Tilin määritys-sivulla toimimalla seuraavasti:

    ![Ulkoinen todennus] (./media/active-directory-saas-smarteru-tutorial/IC777327.png "Ulkoinen todennus")

    1.  Valitse **Ota käyttöön ulkoisen luvan**.
    2.  Valitse **Perustyyli kirjautuminen ohjausobjekti** -osassa **SmarterU** -välilehti.
    3.  Valitse **Oletus sisäänkirjautuminen** -osassa **SmarterU** -välilehti.
    4.  Valitse **Ota käyttöön Okta**.
    5.  Ladattu metatiedot-tiedoston sisällön kopioiminen ja liittäminen **Okta metatietojen** tekstiruutu.
    6.  Valitse **Tallenna**.

7.  Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-smarteru-tutorial/IC777328.png "Kertakirjautumisen määrittäminen")

##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Jos haluat ottaa käyttöön Azure AD-käyttäjät voivat kirjautua sisään SmarterU, ne on valmisteltava SmarterU kyselyjä.  
SmarterU valmistelu on manuaalinen tehtävä.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Valmistelu käyttäjätilit, toimi seuraavasti:

1.  Kirjaudu sisään **SmarterU** alihallintaan.

2.  Siirry **käyttäjät**.

3.  Valitse käyttäjä-osassa seuraavasti:

    ![Uusi käyttäjä] (./media/active-directory-saas-smarteru-tutorial/IC777329.png "Uusi käyttäjä")

    1.  Valitse **+ käyttäjä**.
    2.  Kirjoita Azure AD-käyttäjätilin liittyvän määritteen arvot seuraavat tekstiruutujen: **ensisijainen sähköposti** **Työntekijätunnus**, **salasana**, **Vahvista salasana**, **annettu nimi**, **Sukunimi**.
    3.  Valitse **aktiivinen**.
    4.  Valitse **Tallenna**.

>[AZURE.NOTE] Voit käyttää mitä tahansa muita SmarterU käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä SmarterU säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-smarteru-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä SmarterU, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **SmarterU **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-smarteru-tutorial/IC777330.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-smarteru-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).
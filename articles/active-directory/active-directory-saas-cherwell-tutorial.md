<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Cherwell | Microsoft Azure" 
    description="Opettele käyttämään Cherwell Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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
    ms.date="10/14/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-cherwell"></a>Opetusohjelma: Cherwell Azure Active Directory-integrointi

Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja Cherwell integrointi. Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Käytössä Cherwell kertakirjautumisen tilaus

Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt Cherwell Azure AD-käyttäjien saa oikeuden kertakirjauksen Cherwell yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.

Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Cherwell sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-cherwell-tutorial/IC798988.png "Skenaario")
##<a name="enabling-the-application-integration-for-cherwell"></a>Cherwell sovelluksen integrointi ottaminen käyttöön

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön Cherwell sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-cherwell-perform-the-following-steps"></a>Cherwell sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-cherwell-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-cherwell-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-cherwell-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-cherwell-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **Cherwell**.

    ![Cherwell] (./media/active-directory-saas-cherwell-tutorial/IC798989.png "Cherwell")

7.  Valitse tulosruudussa **Cherwell**ja valitse **Valmis** , voit lisätä sovelluksen.
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen

    ![Cherwell](./media/active-directory-saas-cherwell-tutorial/IC798996.png "Cherwell")

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen Cherwell käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **Cherwell** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-cherwell-tutorial/IC798990.png "Kertakirjautumisen määrittäminen")

2.  **Miten käyttäjät voivat kirjautua Cherwell haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-cherwell-tutorial/IC798991.png "Kertakirjautumisen määrittäminen")

3.  Tee **Määrittäminen sovelluksen URL-osoite** -sivulla seuraavat toimet:

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-cherwell-tutorial/IC798992.png "Sovelluksen URL-Osoitteen määrittäminen")

    a.  Kirjoita **Merkki-URL** -tekstiruutuun käyttää käyttäjien oman **Cherwell** kirjautumalla URL-osoite (esimerkiksi: *https://\<yrityksen nimi\>.cherwellondemand.com/cherwellclient*).

    b.  Valitse **Seuraava**

4.  Valitse **Määritä kertakirjautumisen osoitteessa Cherwell** -sivulla toimimalla seuraavasti:

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-cherwell-tutorial/IC798993.png "Kertakirjautumisen määrittäminen")

    a.  Valitse **Lataa**ja Tallenna sertifikaatin paikallisesti tietokoneeseen.

    b.  Kopioi **tunnistetietojen toimittaja URL-osoite**.

    c-näppäinyhdistelmää.  Kopioi **Sign-palvelun URL-osoite**.

    d.  Valitse **Seuraava**.

5.  Lähetä ladatut sertifikaatin **Identity palveluntarjoajan URL-osoite** ja **Sign-palvelun URL** Cherwell tukihenkilöstölle.

    >[AZURE.NOTE] Cherwell tukihenkilöstö on suoritettava todellinen SSO-määritys.
Näyttöön tulee ilmoitus, kun SSO on otettu käyttöön tilauksen.

6.  Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-cherwell-tutorial/IC798994.png "Kertakirjautumisen määrittäminen")

##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu

Jos haluat ottaa käyttöön Azure AD-käyttäjät voivat kirjautua sisään Cherwell, ne on valmisteltava Cherwell kyselyjä.  
Kyseessä Cherwell käyttäjätilit on luotava Cherwell tukihenkilöstölle.

>[AZURE.NOTE] Voit käyttää mitä tahansa muita Cherwell käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä Cherwell valmistelu Azure Active Directory käyttäjätilit.

##<a name="assigning-users"></a>Käyttäjien määrittäminen

Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-cherwell-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä Cherwell, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **Cherwell** sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-cherwell-tutorial/IC798995.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-cherwell-tutorial/IC767830.png "Kyllä")

Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).

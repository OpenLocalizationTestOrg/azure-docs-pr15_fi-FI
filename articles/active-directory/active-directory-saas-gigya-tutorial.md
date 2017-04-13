<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Gigya | Microsoft Azure" 
    description="Opettele käyttämään Gigya Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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
    ms.date="09/01/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-gigya"></a>Opetusohjelma: Gigya Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja Gigya integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Valitse Gigya kertakirjauksen käytössä tilaus
  
Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt Gigya Azure AD-käyttäjien saa oikeuden kertakirjauksen Gigya yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Gigya sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-gigya-tutorial/IC789512.png "Kertakirjautumisen määrittäminen")
##<a name="enabling-the-application-integration-for-gigya"></a>Gigya sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön Gigya sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-gigya-perform-the-following-steps"></a>Gigya sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-gigya-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-gigya-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-gigya-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-gigya-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **Gigya**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-gigya-tutorial/IC789513.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **Gigya**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Gigya] (./media/active-directory-saas-gigya-tutorial/IC789527.png "Gigya")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen Gigya käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.  
Tämän toimintosarjan osana on base-64-koodattu sertifikaattitiedosto luomiseen.  
Jos et ole tottunut näin, katso, [miten voit muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **Gigya** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-gigya-tutorial/IC789528.png "Kertakirjautumisen määrittäminen")

2.  **Miten käyttäjät voivat kirjautua Gigya haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-gigya-tutorial/IC789529.png "Kertakirjautumisen määrittäminen")

3.  **Määritä sovelluksen URL-osoite** -sivulla **Gigya merkki-URL** -tekstiruutuun Kirjoita käyttämällä seuraavaa kaavaa "*http://company.gigya.com*" URL-osoite ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-gigya-tutorial/IC789530.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  Valitse **Määritä kertakirjautumisen osoitteessa Gigya** -sivulla valitsemalla **Lataa**ja Tallenna sertifikaattitiedosto tietokoneeseen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-gigya-tutorial/IC789531.png "Kertakirjautumisen määrittäminen")

5.  Eri selainikkunassa Kirjaudu Gigya yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

6.  Siirry **asetukset \> SAML kirjautuminen**, ja valitse sitten **Lisää** -painiketta.

    ![SAML-kirjautuminen] (./media/active-directory-saas-gigya-tutorial/IC789532.png "SAML-kirjautuminen")

7.  **SAML-kirjautuminen** -osassa seuraavasti:

    ![SAML-määritys] (./media/active-directory-saas-gigya-tutorial/IC789533.png "SAML-määritys")

    1.  Kirjoita **nimi** -tekstiruutuun kokoonpanosi nimi.
    2.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Gigya** -valintasivun kopioi **Myöntäjä URL-osoite** -arvo ja liitä se sitten **myöntäjä** -tekstiruutuun.
    3.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Gigya** -valintasivun **Sign-palvelun URL** -arvon kopioi ja liitä se sitten **Sign-palvelun URL** -tekstiruutuun.
    4.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Gigya** -valintasivun **Nimimuotoa tunnus** -arvo kopioi ja liitä se sitten **Nimimuotoa tunnus** -tekstiruutuun.
    5.  Luo **base-64-koodattu** tiedosto ladattu varmennetta.
        
        >[AZURE.TIP]Lisätietoja on artikkelissa [muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o)

    6.  Avaa base-64-koodattu varmennetta Muistiossa, kopioi Leikepöydän sisällön se ja liitä se **X.509-varmenne** -tekstiruutuun.
    7.  Valitse **Tallenna asetukset**.

8.  Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-gigya-tutorial/IC789534.png "Kertakirjautumisen määrittäminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Jos haluat ottaa käyttöön Azure AD-käyttäjät voivat kirjautua sisään Gigya, ne on valmisteltava Gigya kyselyjä.  
Gigya valmistelu on manuaalinen tehtävä.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Valmistelu käyttäjätilit, toimi seuraavasti:

1.  Kirjaudu sisään **Gigya** yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

2.  Siirry **järjestelmänvalvojan \> käyttäjien hallinta**, ja valitse sitten **Kutsu käyttäjiä**.

    ![Käyttäjien hallinta] (./media/active-directory-saas-gigya-tutorial/IC789535.png "Käyttäjien hallinta")

3.  Valitse kutsu käyttäjiä-valintaikkunassa seuraavasti:

    ![Käyttäjien kutsuminen] (./media/active-directory-saas-gigya-tutorial/IC789536.png "Käyttäjien kutsuminen")

    1.  Kirjoita lisättävän sähköpostitunnuksen, jos haluat säätää kelvollinen Azure Active Directory-tilin **sähköpostiosoite** -tekstiruutuun.
    2.  Valitse **Kutsu käyttäjä**.
    
        >[AZURE.NOTE] Azure Active Directory-tilin omistaja saavat sähköpostiviestin, joka sisältää linkin Vahvista tili, ennen kuin se on aktiivinen.

>[AZURE.NOTE] Voit käyttää mitä tahansa muita Gigya käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä Gigya säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-gigya-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä Gigya, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **Gigya **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-gigya-tutorial/IC789537.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-gigya-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).
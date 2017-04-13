<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Egnyte | Microsoft Azure" 
    description="Opettele käyttämään Egnyte Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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

#<a name="tutorial-azure-active-directory-integration-with-egnyte"></a>Opetusohjelma: Egnyte Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja Egnyte integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Käytössä Egnyte kertakirjautumisen tilaus
  
Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt Egnyte Azure AD-käyttäjien saa oikeuden kertakirjauksen sovellukseen Egnyte yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Egnyte sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-egnyte-tutorial/IC787812.png "Skenaario")
##<a name="enabling-the-application-integration-for-egnyte"></a>Egnyte sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön Egnyte sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-egnyte-perform-the-following-steps"></a>Egnyte sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-egnyte-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-egnyte-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-egnyte-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-egnyte-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **egnyte**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-egnyte-tutorial/IC787813.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **Egnyte**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Egnyte] (./media/active-directory-saas-egnyte-tutorial/IC787814.png "Egnyte")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen Egnyte käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.  
Tämän toimintosarjan osana on base-64-koodattu sertifikaattitiedosto luomiseen.  
Jos et ole tottunut näin, katso, [miten voit muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **Egnyte** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-egnyte-tutorial/IC787815.png "Kertakirjautumisen määrittäminen")

2.  **Miten käyttäjät voivat kirjautua Egnyte haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-egnyte-tutorial/IC787816.png "Kertakirjautumisen määrittäminen")

3.  **Määrittää sovelluksen URL-osoite** -sivulla **Egnyte merkki-URL** -tekstiruutuun Kirjoita käyttämällä seuraavaa kaavaa "*https://company.egnyte.com*" URL-osoite ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-egnyte-tutorial/IC787817.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  Valitse **Määritä kertakirjautumisen osoitteessa Egnyte** -sivulla valitsemalla **Lataa**ja Tallenna sertifikaattitiedosto tietokoneeseen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-egnyte-tutorial/IC787818.png "Kertakirjautumisen määrittäminen")

5.  Eri selainikkunassa Kirjaudu Egnyte yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

6.  Valitse **asetukset**.

    ![Asetukset] (./media/active-directory-saas-egnyte-tutorial/IC787819.png "Asetukset")

7.  Valitse-valikosta **asetukset**.

    ![Asetukset] (./media/active-directory-saas-egnyte-tutorial/IC787820.png "Asetukset")

8.  Valitse **määritys** -välilehti ja valitse sitten **Suojaus**.

    ![Tietoturva] (./media/active-directory-saas-egnyte-tutorial/IC787821.png "Tietoturva")

9.  **Sign-On todennus** -osassa seuraavasti:

    ![Kertakirjautuminen todennus] (./media/active-directory-saas-egnyte-tutorial/IC787822.png "Kertakirjautuminen todennus")

    1.  Valitse **Sign-todennus**- **SAML 2.0**.
    2.  Valitse **AzureAD** **tunnistetietojen toimittaja**.
    3.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Egnyte** -valintasivun **Remote sisäänkirjautumisen URL-osoite** -arvo kopioi ja liitä se sitten **tunnistetietojen toimittaja sisäänkirjautumisen URL-osoite** -tekstiruutuun.
    4.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Egnyte** -valintasivun **Kohteen tunnus** -arvon kopioi ja liitä se sitten **tunnistetietojen toimittaja kohteen tunnus** -tekstiruutuun.
    5.  Luo **base-64-koodattu** tiedosto ladattu varmennetta.  

        >[AZURE.TIP]Lisätietoja on artikkelissa [muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o)

    6.  Avaa base-64-koodattu varmennetta Muistiossa, kopioi Leikepöydän sisällön se ja liitä se **tunnistetietojen toimittaja varmenne** -tekstiruutuun.
    7.  **Oletusarvoinen käyttäjän määritystä**Valitse **sähköpostiosoite**.
    8.  **Käytä toimialueen kielikohtaiset myöntäjä arvo**Valitse **poistettu käytöstä**.
    9.  Valitse **Tallenna**.

10. Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-egnyte-tutorial/IC787823.png "Kertakirjautumisen määrittäminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Jos haluat ottaa käyttöön Azure AD-käyttäjät voivat kirjautua sisään Egnyte, ne on valmisteltava Egnyte kyselyjä.  
Egnyte valmistelu on manuaalinen tehtävä.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Valmistelu käyttäjätilit, toimi seuraavasti:

1.  Kirjaudu sisään **Egnyte** Egnyte yrityksesi sivuston järjestelmänvalvojana.

2.  Siirry **asetukset \> käyttäjät ja ryhmät**.

3.  Valitse **Lisää uusi käyttäjä**ja valitse sitten käyttäjätyyppi, johon haluat lisätä.

    ![Käyttäjät] (./media/active-directory-saas-egnyte-tutorial/IC787824.png "Käyttäjät")

4.  **Uusi peruskäyttäjän** -osassa seuraavasti:

    ![Uusi peruskäyttäjän] (./media/active-directory-saas-egnyte-tutorial/IC787825.png "Uusi peruskäyttäjän")

    1.  Kirjoita **sähköpostiosoite**, **käyttäjänimi** ja muut tiedot, jos haluat säätää kelvollinen Azure Active Directory-tilin.
    2.  Valitse **Tallenna**.

    >[AZURE.NOTE] Azure Active Directory-tilin omistaja saavat sähköposti-ilmoitus.

>[AZURE.NOTE] Voit käyttää mitä tahansa muita Egnyte käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä Egnyte säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-egnyte-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä Egnyte, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **Egnyte **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-egnyte-tutorial/IC787826.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-egnyte-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).
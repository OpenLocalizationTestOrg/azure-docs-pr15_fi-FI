<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Coupa | Microsoft Azure" 
    description="Opettele käyttämään Coupa Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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

#<a name="tutorial-azure-active-directory-integration-with-coupa"></a>Opetusohjelma: Coupa Azure Active Directory-integrointi

Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja Coupa integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Käytössä Coupa kertakirjautumisen tilaus

Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt Coupa Azure AD-käyttäjien saa oikeuden kertakirjauksen käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.

Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Coupa sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-coupa-tutorial/IC791897.png "Skenaario")
##<a name="enabling-the-application-integration-for-coupa"></a>Coupa sovelluksen integrointi ottaminen käyttöön

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön Coupa sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-coupa-perform-the-following-steps"></a>Coupa sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-coupa-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-coupa-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-coupa-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-coupa-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **Coupa**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-coupa-tutorial/IC791898.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **Coupa**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Coupa] (./media/active-directory-saas-coupa-tutorial/IC791899.png "Coupa")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen Coupa käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.  
Kertakirjautumisen varten Coupa määrittäminen edellyttää, että allekirjoitus arvon noutaa varmenne.  
Jos et ole tottunut näin, katso, [miten voit hakea varmenteen allekirjoitus-arvon](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Kirjaudu Coupa yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

2.  Siirry **asennuksen \> suojauksen hallinta**.

    ![Turvaominaisuudet] (./media/active-directory-saas-coupa-tutorial/IC791900.png "Turvaominaisuudet")

3.  Voit ladata Coupa metatieto-tiedosto tietokoneeseen napsauttamalla **Lataa ja tuo SP metatiedot**.

    ![Coupa SP metatiedot] (./media/active-directory-saas-coupa-tutorial/IC791901.png "Coupa SP metatiedot")

4.  Azure perinteinen portaalin kirjautua eri selainikkunassa.

5.  Valitse integration **Coupa** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-coupa-tutorial/IC791902.png "Kertakirjautumisen määrittäminen")

6.  **Miten käyttäjät voivat kirjautua Coupa haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-coupa-tutorial/IC791903.png "Kertakirjautumisen määrittäminen")

7.  Tee **Määrittäminen sovelluksen URL-osoite** -sivulla seuraavat toimet:

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-coupa-tutorial/IC791904.png "Sovelluksen URL-Osoitteen määrittäminen")

    1.  Kirjoita **Merkki-URL** -tekstiruutuun käyttämä käyttäjien kirjautua Coupa sovelluksen URL-osoite (esimerkiksi: "*http://company.Coupa.com*").
    2.  Avaa ladattu tiedosto Coupa metatietojen ja kopioi sitten **AssertionConsumerService indeksin tai URL-osoite**.
    3.  Liitä **Coupa vastaa URL** -tekstiruutuun **AssertionConsumerService indeksin tai URL-osoite** -arvo.
    4.  Valitse **Seuraava**.

8.  **Määritä kertakirjautumisen osoitteessa Coupa** -sivulla lataamaan metatieto-tiedosto valitsemalla **Lataa metatietojen**, ja tallenna sitten tiedosto paikallisesti tietokoneeseen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-coupa-tutorial/IC791905.png "Kertakirjautumisen määrittäminen")

9.  Siirry Coupa yrityksen sivuston **asennuksen \> suojauksen hallinta**.

    ![Turvaominaisuudet] (./media/active-directory-saas-coupa-tutorial/IC791900.png "Turvaominaisuudet")

10. **Kirjaudu sisään Coupa tunnistetiedoilla** -osassa seuraavasti:

    ![Kirjaudu sisään Coupa tunnistetiedoilla] (./media/active-directory-saas-coupa-tutorial/IC791906.png "Kirjaudu sisään Coupa tunnistetiedoilla")

    1.  Valitse **Kirjaudu sisään käyttämällä SAML**.
    2.  Valitse **Selaa** ladatut Azure Active metatiedot-tiedoston lataaminen.
    3.  Valitse **Tallenna**.

11. Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-coupa-tutorial/IC791907.png "Kertakirjautumisen määrittäminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu

Jos haluat ottaa käyttöön Azure AD-käyttäjät voivat kirjautua sisään Coupa, ne on valmisteltava Coupa kyselyjä.  
Coupa valmistelu on manuaalinen tehtävä.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Voit määrittää käyttäjän valmistelu toimimalla seuraavasti:

1.  Kirjaudu sisään **Coupa** yrityksen sivuston järjestelmänvalvojana.

2.  Valitse ylä-valikossa **asetukset**ja valitse sitten **käyttäjät**.

    ![Käyttäjät] (./media/active-directory-saas-coupa-tutorial/IC791908.png "Käyttäjät")

3.  Valitse **Luo**.

    ![Käyttäjien luominen] (./media/active-directory-saas-coupa-tutorial/IC791909.png "Käyttäjien luominen")

4.  **Käyttäjän luominen** -osassa seuraavasti:

    ![Käyttäjätiedot] (./media/active-directory-saas-coupa-tutorial/IC791910.png "Käyttäjätiedot")

    1.  Kirjoita **Kirjaudu sisään**, **Etunimi**, **Sukunimi**, **Sign-tunnus**, **sähköpostin** määritteet haluat säännöstä kyselyjä liittyvät tekstiruutujen kelvollinen Azure Active Directory-tilin.
    2.  Valitse **Luo**.

    >[AZURE.NOTE] Azure Active Directory-tilin omistaja saavat sähköpostiviestin linkin Vahvista tili, ennen kuin se on aktiivinen.

>[AZURE.NOTE] Voit käyttää mitä tahansa muita Coupa käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä Coupa säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen

Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-coupa-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä Coupa, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **Coupa **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-coupa-tutorial/IC791911.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-coupa-tutorial/IC767830.png "Kyllä")

Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).

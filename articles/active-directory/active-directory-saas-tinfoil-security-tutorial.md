<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Tinapaperi suojauksen | Microsoft Azure"
    description="Opettele käyttämään Tinapaperi suojaus käyttöön kertakirjautumisen, automaattinen valmistelu ja muut Azure Active Directory-hakemistosta!." 
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

#<a name="tutorial-azure-active-directory-integration-with-tinfoil-security"></a>Opetusohjelma: Tinapaperi suojauksen Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja Tinapaperi suojaus.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Käytössä Tinapaperi suojauksen kertakirjautumisen tilaus
  
Kun olet suorittanut Tässä opetusohjelmassa, olet määrittänyt Tinapaperi suojaus Azure AD-käyttäjien saa oikeuden kertakirjauksen Tinapaperi suojauksen yrityksesi sivuston (tunnistetietojen toimittaja Omat allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Sovelluksen integrointi Tinapaperi suojauksen ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-tinfoil-security-tutorial/IC798965.png "Kertakirjautumisen määrittäminen")

##<a name="enabling-the-application-integration-for-tinfoil-security"></a>Sovelluksen integrointi Tinapaperi suojauksen ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen sovelluksen integrointi Tinapaperi suojauksen ottaminen käyttöön.

###<a name="to-enable-the-application-integration-for-tinfoil-security-perform-the-following-steps"></a>Ottaa käyttöön sovelluksen integroinnin Tinapaperi suojauksen, toimi seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-tinfoil-security-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-tinfoil-security-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-tinfoil-security-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-tinfoil-security-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **Tinapaperi suojaus**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-tinfoil-security-tutorial/IC798966.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **Tinapaperi suojaus**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Tinapaperi suojaus] (./media/active-directory-saas-tinfoil-security-tutorial/IC802771.png "Tinapaperi suojaus")

##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen Tinapaperi suojausta käyttämällä SAML-protokollan perusteella federation Azure AD-niiden tilillä.  
Kertakirjautumisen Tinapaperi suojauksen määrittäminen edellyttää, että allekirjoitus arvon noutaa varmenne.  
Jos et ole tottunut näin, katso, [miten voit hakea varmenteen allekirjoitus-arvon](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **Tinapaperi suojauksen** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-tinfoil-security-tutorial/IC798967.png "Kertakirjautumisen määrittäminen")

2.  **Miten käyttäjät voivat kirjautua Tinapaperi suojauksen haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-tinfoil-security-tutorial/IC798968.png "Kertakirjautumisen määrittäminen")

3.  Kirjoita **Määritä sovelluksen URL-osoite** -sivulla **Tinapaperi suojauksen vastaa URL** -tekstiruutuun Tinapaperi suojauksen vahvistus kuluttaja Service (ACS) URL-osoite (esimerkiksi: "*https://www.tinfoilsecurity.com/saml/consume*", ja valitse sitten **Seuraava**.

    >[AZURE.NOTE] Sinun pitäisi ACS URL-Osoitteen hankkiminen Tinapaperi suojauksen metatietosäilöstä (https://www.tinfoilsecurity.com/saml/metadata).

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-tinfoil-security-tutorial/IC798969.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  Valitse **Määritä kertakirjautumisen osoitteessa Tinapaperi suojaus** -sivulla Lataa sertifikaatti, **Lataa**ja Tallenna sertifikaattitiedosto paikallisesti kuin **c:\\Tinapaperi Security.cer**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-tinfoil-security-tutorial/IC798970.png "Kertakirjautumisen määrittäminen")

5.  Eri selainikkunassa Kirjaudu Tinapaperi suojauksen yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

6.  Valitse **Oma tili**-sivun työkalurivillä.

    ![Raporttinäkymät-ikkunan] (./media/active-directory-saas-tinfoil-security-tutorial/IC798971.png "Raporttinäkymät-ikkunan")

7.  Valitse **Suojaus**.

    ![Tietoturva] (./media/active-directory-saas-tinfoil-security-tutorial/IC798972.png "Tietoturva")

8.  Valitse **Kertakirjautumisen** määritys-sivulla toimimalla seuraavasti:

    ![Kertakirjautuminen] (./media/active-directory-saas-tinfoil-security-tutorial/IC798973.png "Kertakirjautuminen")

    1.  Valitse **Ota käyttöön SAML**.
    2.  Valitse **Manuaalinen määritys**.
    3.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Tinapaperi suojaus** -valintasivun **SAML SSO URL-osoite** -arvo kopioi ja liitä se sitten **SAML kirjaa URL** -tekstiruutuun.
    4.  Viedyn varmenteen **allekirjoitusta** kopioi ja liitä se **SAML varmenteen tunnisteen** tekstiruutu.  

        >[AZURE.TIP] Lisätietoja on artikkelissa [noutamisesta varmenteen allekirjoitusarvo](http://youtu.be/YKQF266SAxI)

    5.  Kopioi **tilin tunnus**.
    6.  Valitse **Tallenna**.

9.  Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-tinfoil-security-tutorial/IC798974.png "Kertakirjautumisen määrittäminen")

10. Valitse ylä-valikossa **määritteet** **SAML suojaustunnuksen määritteet** -valintaikkunan avaaminen.

    ![Määritteet] (./media/active-directory-saas-tinfoil-security-tutorial/IC795920.png "Määritteet")

11. Lisää pakollinen määrite yhdistämismääritykset, toimi seuraavasti:

    ![Määritteet] (./media/active-directory-saas-tinfoil-security-tutorial/IC798975.png "Määritteet")

    1.  Valitse **Lisää käyttäjä-määrite**.
    2.  Kirjoita **accountid** **Määritteen nimi** -tekstiruutuun.
    3.  Liitä kopioimasi edellisen kohdan tilin tunnus arvon **Määritteen arvo** -tekstiruutuun.
    4.  Valitse **Valmis**.

12. Valitse **Ota muutokset käyttöön**.

##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Jotta voit kirjautua Tinapaperi suojauksen Azure AD-käyttäjien käyttöön, ne on valmisteltava kyselyjä Tinapaperi suojaus.  
Tinapaperi suojauksen valmistelu on manuaalinen tehtävä.

###<a name="to-get-a-user-provisioned-perform-the-following-steps"></a>Saat valmisteltu käyttäjä-toimimalla seuraavasti:

1.  Jos käyttäjä on osa yrityksen tiliä, sinun on luotu käyttäjätili saat Tinapaperi suojaus-tukeen.

2.  Jos käyttäjä on tavallinen Tinapaperi suojauksen SaaS käyttäjä, sitten käyttäjä voi lisätä collaborator käyttäjän sivustoista. Tämä käynnistää prosessi, jos haluat lähettää kutsun määritetyn sähköpostin voit luoda uuden Tinapaperi suojaus-käyttäjätilin.

>[AZURE.NOTE] Voit käyttää mitä tahansa muita Tinapaperi suojauksen käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä Tinapaperi suojauksen säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-tinfoil-security-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä Tinapaperi suojauksen, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **Tinapaperi suojaus **-sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-tinfoil-security-tutorial/IC798976.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-tinfoil-security-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).
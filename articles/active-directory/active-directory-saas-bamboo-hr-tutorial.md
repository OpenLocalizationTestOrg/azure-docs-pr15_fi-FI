<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi bambu HR | Microsoft Azure" 
    description="Opettele käyttämään bambu HR Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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

#<a name="tutorial-azure-active-directory-integration-with-bamboo-hr"></a>Opetusohjelma: Bambu HR Azure Active Directory-integrointi

Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja BambooHR integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Käytössä BambooHR kertakirjautumisen tilaus

Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt BambooHR Azure AD-käyttäjien saa oikeuden kertakirjauksen BambooHR yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.

Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  BambooHR sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-bamboo-hr-tutorial/IC796685.png "Skenaario")
##<a name="enabling-the-application-integration-for-bamboohr"></a>BambooHR sovelluksen integrointi ottaminen käyttöön

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön BambooHR sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-bamboohr-perform-the-following-steps"></a>BambooHR sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-bamboo-hr-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-bamboo-hr-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-bamboo-hr-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-bamboo-hr-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **BambooHR**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-bamboo-hr-tutorial/IC796686.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **BambooHR**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![BambooHR] (./media/active-directory-saas-bamboo-hr-tutorial/IC796687.png "BambooHR")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen BambooHR käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.  
Tämän toimintosarjan osana on base-64-koodattu sertifikaattitiedosto luomiseen.  
Jos et ole tottunut näin, katso, [miten voit muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **BambooHR** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Skenaario] (./media/active-directory-saas-bamboo-hr-tutorial/IC796685.png "Skenaario")

2.  **Miten käyttäjät voivat kirjautua BambooHR haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-bamboo-hr-tutorial/IC796688.png "Määritä kertakirjautuminen")

3.  Kirjoita **Määrittäminen sovelluksen URL-osoite** -sivulla **BambooHR merkki-URL** -tekstiruutuun käyttää käyttäjien kirjautua BambooHR sovelluksen URL-osoite (esimerkiksi: https://company.bamboohr.com), ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-bamboo-hr-tutorial/IC796689.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  Valitse **Määritä kertakirjautumisen osoitteessa BambooHR** -sivulla valitsemalla **Lataa**ja Tallenna sertifikaattitiedosto tietokoneeseen.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-bamboo-hr-tutorial/IC796690.png "Määritä kertakirjautuminen")

5.  Eri selainikkunassa Kirjaudu BambooHR yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

6.  Aloitussivulla toimi seuraavasti:

    ![Kertakirjautuminen] (./media/active-directory-saas-bamboo-hr-tutorial/IC796691.png "Kertakirjautuminen")

    1.  Valitse **sovellukset**.
    2.  Valitse vasemmalla puolella sovellukset-valikossa **Kertakirjautumisen**.
    3.  Valitse **SAML kertakirjautumisen**.

7.  **SAML Single Sign** -osassa seuraavasti:

    ![SAML kertakirjautuminen] (./media/active-directory-saas-bamboo-hr-tutorial/IC796692.png "SAML kertakirjautuminen")

    1.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa BambooHR** -valintasivun **Sign-palvelun URL** -arvon kopioi ja liitä se sitten **SSO-kirjautumisen URL** -tekstiruutuun.
    2.  Luo **base-64-koodattu** tiedosto ladattu varmennetta.  

        >[AZURE.TIP] Lisätietoja on artikkelissa [muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o)

    3.  Avaa base-64-koodattu varmennetta Muistiossa, kopioi se sisällön Leikepöydän ja liitä se **X.509-varmenne** -tekstiruutu
    4.  Valitse **Tallenna**.

8.  Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-bamboo-hr-tutorial/IC796693.png "Määritä kertakirjautuminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu

Jos haluat ottaa käyttöön Azure AD-käyttäjät voivat kirjautua sisään BambooHR, ne on valmisteltava BambooHR kyselyjä.  
BambooHR valmistelu on manuaalinen tehtävä.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Valmistelu käyttäjätilit, toimi seuraavasti:

1.  Kirjaudu **BambooHR** -sivustoon järjestelmänvalvojana.

2.  Valitse **asetukset**-sivun työkalurivillä.

    ![Määrittäminen] (./media/active-directory-saas-bamboo-hr-tutorial/IC796694.png "Määrittäminen")

3.  Valitse **yleiskuvaus**.

4.  Valitse vasemmassa siirtymisruudussa **suojauksen \> käyttäjien**.

5.  Kirjoita käyttäjän nimi, salasanan ja sähköpostiosoitteen kelvollinen AAD tilin haluat säännöstä liittyvät tekstiruutujen kyselyjä.

6.  Valitse **Tallenna**.

>[AZURE.NOTE] Voit käyttää mitä tahansa muita BambooHR käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä BambooHR säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen

Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-bamboohr-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä BambooHR, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **BambooHR **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-bamboo-hr-tutorial/IC796695.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-bamboo-hr-tutorial/IC767830.png "Kyllä")

Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).

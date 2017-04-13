<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Panopto | Microsoft Azure" 
    description="Opettele käyttämään Panopto Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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

#<a name="tutorial-azure-active-directory-integration-with-panopto"></a>Opetusohjelma: Panopto Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja Panopto integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Panopto palvelutili
  
Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt Panopto Azure AD-käyttäjien saa oikeuden kertakirjauksen Panopto yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Panopto sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-panopto-tutorial/IC777665.png "Skenaario")
##<a name="enabling-the-application-integration-for-panopto"></a>Panopto sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön Panopto sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-panopto-perform-the-following-steps"></a>Panopto sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-panopto-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-panopto-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-panopto-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-panopto-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **Panopto**.

    ![Appkication valikoima] (./media/active-directory-saas-panopto-tutorial/IC777666.png "Appkication valikoima")

7.  Valitse tulosruudussa **Panopto**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Panopto] (./media/active-directory-saas-panopto-tutorial/IC782936.png "Panopto")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen Panopto käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.  
Tämän toimintosarjan osana on base-64-koodattu sertifikaattitiedosto luomiseen.  
Jos et ole tottunut näin, katso, [miten voit muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **Panopto** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-panopto-tutorial/IC777667.png "Määritä kertakirjautuminen")

2.  **Miten käyttäjät voivat kirjautua Panopto haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-panopto-tutorial/IC777668.png "Määritä kertakirjautuminen")

3.  Kirjoita **Määritä sovelluksen URL-osoite** -sivulla **Panopto merkki-URL** -tekstiruutuun URL-osoite käyttämällä seuraavaa kaavaa "*https://\<vuokraajan nimi\>. Panopto.com*", ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-panopto-tutorial/IC777528.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  Valitse **Määritä kertakirjautumisen osoitteessa Panopto** -sivulla Lataa sertifikaatti, valitsemalla **Lataa**, ja Tallenna sertifikaattitiedosto tietokoneeseen.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-panopto-tutorial/IC777669.png "Määritä kertakirjautuminen")

5.  Eri selainikkunassa Kirjaudu Panopto yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

6.  Työkalurivin vasemmalla puolella Valitse **Järjestelmä**ja valitse sitten **Tunnistetietojen toimittajat**.

    ![Järjestelmän] (./media/active-directory-saas-panopto-tutorial/IC777670.png "Järjestelmän")

7.  Valitse **Lisää palvelu**.

    ![Tunnistetietojen toimittajat] (./media/active-directory-saas-panopto-tutorial/IC777671.png "Tunnistetietojen toimittajat")

8.  SAML-palvelu-osassa seuraavasti:

    ![SaaS määritys] (./media/active-directory-saas-panopto-tutorial/IC777672.png "SaaS määritys")

    1.  **Palvelun laji** -luettelosta **SAML20**
    2.  Kirjoita nimi-esiintymän **Esiintymänimi** -tekstiruutuun.
    3.  Kirjoita kuvaus **Kuvaus** -tekstiruutuun.
    4.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Panopto** -valintasivun kopioi **Myöntäjä URL-osoite** -arvo ja liitä se sitten **myöntäjä** -tekstiruutuun.
    5.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Panopto** -valintasivun **SAML SSO URL-osoite** -arvo kopioi ja liitä se sitten **Pomppiva sivun Url** -tekstiruutuun.
    6.  Luo **base-64-koodattu** tiedosto ladattu varmennetta.  

        >[AZURE.TIP] Lisätietoja on artikkelissa [muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o)

    7.  Avaa base-64-koodattu varmennetta Muistiossa, kopioi Leikepöydän sisällön se ja liitä se **Julkinen avain** -tekstiruutu
    8.  Valitse **Tallenna**.
        ![Tallenna] (./media/active-directory-saas-panopto-tutorial/IC777673.png "Tallenna")

9.  Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-panopto-tutorial/IC777674.png "Määritä kertakirjautuminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Ei ei toimi, voit määrittää käyttäjän Panopto valmistelu.  
Kun määritetty käyttäjä yrittää kirjautua käyttämällä access-paneelin Panopto, Panopto tarkistaa, onko käyttäjän olemassa.  
Jos ei ole käyttäjätiliä käytettävissä vielä, se luodaan automaattisesti Panopto mukaan.

>[AZURE.NOTE]Voit käyttää mitä tahansa muita Panopto käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä Panopto valmistelu Azure AD-käyttäjätilejä.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-panopto-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä Panopto, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **Panopto **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-panopto-tutorial/IC777675.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-panopto-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).
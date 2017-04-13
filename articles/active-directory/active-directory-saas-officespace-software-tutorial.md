<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi OfficeSpace ohjelmiston | Microsoft Azure" 
    description="Opettele käyttämään OfficeSpace ohjelmiston Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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

#<a name="tutorial-azure-active-directory-integration-with-officespace-software"></a>Opetusohjelma: OfficeSpace ohjelmiston Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja OfficeSpace ohjelmisto.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Käytössä OfficeSpace ohjelmiston kertakirjautumisen tilaus
  
Kun olet suorittanut Tässä opetusohjelmassa, olet määrittänyt OfficeSpace-ohjelmistoa Azure AD-käyttäjien saa oikeuden kertakirjauksen OfficeSpace ohjelmiston yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Sovelluksen integrointi OfficeSpace ohjelmiston ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-officespace-software-tutorial/IC777764.png "Skenaario")
##<a name="enabling-the-application-integration-for-officespace-software"></a>Sovelluksen integrointi OfficeSpace ohjelmiston ottaminen käyttöön
  
Tässä osassa tavoitteena on Jäsennys, miten voit ottaa käyttöön sovelluksen integrointi OfficeSpace ohjelmisto.

###<a name="to-enable-the-application-integration-for-officespace-software-perform-the-following-steps"></a>Ottaa käyttöön sovelluksen integroinnin OfficeSpace ohjelmisto, toimi seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-officespace-software-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-officespace-software-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-officespace-software-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-officespace-software-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **OfficeSpace ohjelmisto**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-officespace-software-tutorial/IC777765.png "Sovelluksen valikoima")

7.  Valitse tulokset-ruudun Valitse **OfficeSpace, josta**ja valitse sitten **Valmis** , voit lisätä sovelluksen.

    ![OfficeSpace ohjelmisto] (./media/active-directory-saas-officespace-software-tutorial/IC781007.png "OfficeSpace ohjelmisto")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen OfficeSpace ohjelmiston Azure AD käyttämällä SAML-protokollan perusteella federation niiden tilillä.  
Kertakirjautumisen OfficeSpace-ohjelmiston määrittäminen edellyttää, että allekirjoitus arvon noutaa varmenne.  
Jos et ole tottunut näin, katso, [miten voit hakea varmenteen allekirjoitus-arvon](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa **OfficeSpace ohjelmisto** sovelluksen integrointi-sivun **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Määrittäminen kertakirjauksen = on] (./media/active-directory-saas-officespace-software-tutorial/IC777766.png "Määrittäminen kertakirjauksen = on")

2.  **Miten Haluatko, että käyttäjät voivat kirjautua OfficeSpace ohjelmisto** -sivulla **Microsoft Azure AD kertakirjautumisen**Valitse ja valitse sitten **Seuraava**.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-officespace-software-tutorial/IC777767.png "Määritä kertakirjautuminen")

3.  Kirjoita **Määritä sovelluksen URL-osoite** -sivulla **OfficeSpace ohjelmiston merkki-URL** -tekstiruutuun käyttämä käyttäjät kirjautumaan OfficeSpace ohjelmistosovellus URL-osoite (esimerkiksi: "*https://company.officespacesoftware.com*"), ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-officespace-software-tutorial/IC775556.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  **Määritä kertakirjautumisen OfficeSpace-ohjelma** -sivulla Lataa sertifikaatti, valitse **Lataa**, ja Tallenna sertifikaattitiedosto paikallisesti tietokoneeseen.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-officespace-software-tutorial/IC793769.png "Määritä kertakirjautuminen")

5.  Eri selainikkunassa Kirjaudu OfficeSpace ohjelmiston yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

6.  Siirry **järjestelmänvalvojan \> yhdistimien**.

    ![Järjestelmänvalvoja] (./media/active-directory-saas-officespace-software-tutorial/IC777769.png "Järjestelmänvalvoja")

7.  Valitse **SAML luvan**.

    ![Yhdistimet] (./media/active-directory-saas-officespace-software-tutorial/IC777770.png "Yhdistimet")

8.  **SAML-todennus** -osassa seuraavasti:

    ![SAML-määritys] (./media/active-directory-saas-officespace-software-tutorial/IC777771.png "SAML-määritys")

    1.  Azure perinteinen portaalissa-sivulla **Määritä kertakirjautumisen OfficeSpace ohjelma** valintaikkunan **Remote sisäänkirjautumisen URL-osoite** -arvo kopioi ja liitä se sitten **Kirjaudu ulos palvelun url** -tekstiruutuun.
    2.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen OfficeSpace ohjelma** valintaikkunan-sivulla kopioi **Remote Kirjaudu URL-osoite** -arvo ja liitä se **asiakkaan idp kohde-url** -tekstiruutuun.
    3.  Kopioi **allekirjoitusta** viedyn varmenteen ja liitä se **asiakkaan idp varmenteen tunnisteen** tekstiruutu.  

        >[AZURE.TIP]
        Lisätietoja on artikkelissa [noutamisesta varmenteen allekirjoitusarvo](http://youtu.be/YKQF266SAxI)

    4.  Valitse **Tallenna asetukset**.

9.  Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-officespace-software-tutorial/IC777772.png "Määritä kertakirjautuminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Jotta voit kirjautua OfficeSpace ohjelmiston Azure AD-käyttäjien käyttöön, ne on valmisteltava OfficeSpace ohjelmiston kyselyjä. OfficeSpace ohjelmisto valmistelu on Automaattinen tehtävä.  
Ei, ei toimi.  
Käyttäjät luodaan automaattisesti tarvittaessa ensimmäinen yksittäisen Sign yritys aikana.

>[AZURE.NOTE]Voit käyttää mitä tahansa muita OfficeSpace ohjelmiston käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä OfficeSpace ohjelmiston säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-officespace-software-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä OfficeSpace ohjelmiston, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse **OfficeSpace ohjelmiston **sovelluksen integrointi-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-officespace-software-tutorial/IC777773.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-officespace-software-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).
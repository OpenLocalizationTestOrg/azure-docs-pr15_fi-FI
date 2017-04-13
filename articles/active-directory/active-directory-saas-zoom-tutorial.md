<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Zoomaus | Microsoft Azure" 
    description="Opettele käyttämään Zoomaus käyttöön kertakirjautumisen, automaattinen valmistelu ja muut Azure Active Directory-hakemistosta!." 
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
    ms.date="08/16/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-zoom"></a>Opetusohjelma: Zoomaus Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure- ja zoomaus integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Zoomaus-vuokraajan
  
Kun olet suorittanut Tässä opetusohjelmassa, olet määrittänyt Zoomaa Azure AD-käyttäjien saa oikeuden kertakirjauksen sovellukseen Zoomaus yrityksen sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Zoomaus sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-zoom-tutorial/IC784693.png "Skenaario")

##<a name="enabling-the-application-integration-for-zoom"></a>Zoomaus sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön Zoomaus sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-zoom-perform-the-following-steps"></a>Zoomaus sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-zoom-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-zoom-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-zoom-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-zoom-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun**, **Zoomaus**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-zoom-tutorial/IC784694.png "Sovelluksen valikoima")

7.  Valitse tulokset-ruudun Valitse **Zoomaa**ja valitse sitten **Valmis** , voit lisätä sovelluksen.

    ![Zoomaus] (./media/active-directory-saas-zoom-tutorial/IC784695.png "Zoomaus")

##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen Zoomaus käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.  
Tämän toimintosarjan osana on base-64-koodattu sertifikaattitiedosto luomiseen.  
Jos et ole tottunut näin, katso, [miten voit muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa, valitse **Zoomaa** sovelluksen integrointi-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-zoom-tutorial/IC784696.png "Määritä kertakirjautuminen")

2.  **Miten haluat kirjautua Zoomaa käyttäjät** -sivulla **Microsoft Azure AD kertakirjautumisen**Valitse ja valitse sitten **Seuraava**.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-zoom-tutorial/IC784697.png "Määritä kertakirjautuminen")

3.  **Määrittää sovelluksen URL-osoite** -sivulla **Zoomaa merkki-URL** -tekstiruutuun Kirjoita käyttämällä seuraavaa kaavaa "*http://company.zoom.us*" URL-osoite ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-zoom-tutorial/IC784698.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  Valitse **Määritä kertakirjautumisen osoitteessa Zoomaus** -sivulla valitsemalla **Lataa**ja Tallenna sertifikaattitiedosto tietokoneeseen.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-zoom-tutorial/IC784699.png "Määritä kertakirjautuminen")

5.  Eri selainikkunassa Kirjaudu Zoomaus yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

6.  Valitse **Single Sign** -välilehti.

    ![Kertakirjautuminen] (./media/active-directory-saas-zoom-tutorial/IC784700.png "Kertakirjautuminen")

7.  Valitse **Suojaus** -välilehti ja siirry sitten **Single Sign** -asetukset.

8.  Kertakirjautumisen-osassa seuraavasti:

    ![Kertakirjautuminen] (./media/active-directory-saas-zoom-tutorial/IC784701.png "Kertakirjautuminen")

    1.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Zoomaus** -valintasivun **Sign-palvelun URL** -arvon kopioi ja liitä se sitten **Kirjaudu sisään sivun URL** -tekstiruutuun.
    2.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Zoomaus** -valintasivun **Yksittäisen Sign-Out palvelun URL-osoite** -arvo kopioi ja liitä se sitten **Kirjaudu ulos sivun URL** -tekstiruutuun.
    3.  Luo **base-64-koodattu** tiedosto ladattu varmennetta.  

        >[AZURE.TIP] Lisätietoja on artikkelissa [muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o)

    4.  Avaa base-64-koodattu varmennetta Muistiossa, kopioi se sisällön Leikepöydän ja liitä se **tunnistetietojen toimittaja varmenne** -tekstiruutu
    5.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Zoomaus** -valintasivun kopioi **Myöntäjä URL-osoite** -arvo ja liitä se sitten **myöntäjä** -tekstiruutuun.
    6.  Valitse **Tallenna**.

9.  Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-zoom-tutorial/IC784702.png "Määritä kertakirjautuminen")

##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Jotta voit kirjautua Zoomaus Azure AD-käyttäjien käyttöön, ne on valmisteltava kyselyjä Zoomaus.  
Zoomaus-valmistelu on manuaalinen tehtävä.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Valmistelu käyttäjätilit, toimi seuraavasti:

1.  Kirjaudu sisään **Zoomaa** yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

2.  **Tilinhallinta** -välilehti ja valitse sitten **Käyttäjien hallinta**.

3.  Valitse käyttäjien hallinta-osassa **Lisää käyttäjiä**.

    ![Käyttäjien hallinta] (./media/active-directory-saas-zoom-tutorial/IC784703.png "Käyttäjien hallinta")

4.  **Lisää käyttäjät** -sivulla toimimalla seuraavasti:

    ![Lisää käyttäjiä] (./media/active-directory-saas-zoom-tutorial/IC784704.png "Lisää käyttäjiä")

    1.  **Käyttäjätyyppi**Valitse **Perustiedot**.
    2.  Kirjoita sähköpostiosoite, jos haluat säätää kelvollinen AAD tilin **sähköpostit** -tekstiruutuun.
    3.  Valitse **Lisää**.

>[AZURE.NOTE] Voit käyttää mitä tahansa muita Zoomaus käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä Zoomaus valmistelu AAD käyttäjätilit.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-zoom-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä zoomaus, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  **Zoomaa **sovelluksen integrointi, napsauta sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-zoom-tutorial/IC784705.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-zoom-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).
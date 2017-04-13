<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi lähettämä | Microsoft Azure" 
    description="Opettele käyttämään lähettämä Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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

#<a name="tutorial-azure-active-directory-integration-with-envoy"></a>Opetusohjelma: Lähettämä Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja lähettämä integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Lähettämä palvelutili
  
Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt lähettämä Azure AD-käyttäjien saa oikeuden kertakirjauksen lähettämä yrityksen sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Sovelluksen-integrointia varten lähettämä ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-envoy-tutorial/IC776759.png "Skenaario")
##<a name="enabling-the-application-integration-for-envoy"></a>Sovelluksen-integrointia varten lähettämä ottaminen käyttöön
  
Tässä osassa tavoitteena on Jäsennys, miten voit ottaa käyttöön sovelluksen-integrointia varten lähettämä.

###<a name="to-enable-the-application-integration-for-envoy-perform-the-following-steps"></a>Voit ottaa käyttöön sovelluksen-integrointia varten lähettämä toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-envoy-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-envoy-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-envoy-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-envoy-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **lähettämä**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-envoy-tutorial/IC776760.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **lähettämä**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Lähettämä] (./media/active-directory-saas-envoy-tutorial/IC776777.png "Lähettämä")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen lähettämä käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.  
Kertakirjautumisen varten lähettämä määrittäminen edellyttää, että allekirjoitus arvon noutaa varmenne.  
Jos et ole tottunut näin, katso, [miten voit hakea varmenteen allekirjoitus arvon](http://youtu.be/YKQF266SAxI)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **lähettämä** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Ottaa käyttöön kertakirjautumisen] (./media/active-directory-saas-envoy-tutorial/IC776778.png "Ottaa käyttöön kertakirjautumisen")

2.  **Miten käyttäjät voivat kirjautua lähettämä haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-envoy-tutorial/IC776779.png "Määritä kertakirjautuminen")

3.  Kirjoita **Määritä sovelluksen URL-osoite** -sivulla **Lähettämä merkki-URL** -tekstiruutuun URL-osoite käyttämällä seuraavaa kaavaa "*https://\<vuokraajan nimi\>. Envoy.com*", ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-envoy-tutorial/IC776780.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  Valitse **Määritä kertakirjautumisen osoitteessa lähettämä** -sivulla Lataa sertifikaatti, **Lataa**ja Tallenna sertifikaattitiedosto paikallisesti kuin **c:\\Envoy.cer**.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-envoy-tutorial/IC776781.png "Määritä kertakirjautuminen")

5.  Eri selainikkunassa Kirjaudu lähettämä yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

6.  Valitse **asetukset**-sivun työkalurivillä.

    ![Lähettämä] (./media/active-directory-saas-envoy-tutorial/IC776782.png "Lähettämä")

7.  Valitse **yrityksen**.

    ![Yrityksen] (./media/active-directory-saas-envoy-tutorial/IC776783.png "Yrityksen")

8.  Valitse **SAML**.

    ![SAML] (./media/active-directory-saas-envoy-tutorial/IC776784.png "SAML")

9.  **SAML käyttöoikeuksien** määrittäminen-osassa seuraavasti:

    ![SAML-todennus] (./media/active-directory-saas-envoy-tutorial/IC776785.png "SAML-todennus")

    >[AZURE.NOTE] Sovelluksen luoman automaattinen menot sijainnin tunnus arvo.

    1.  Kopioi **allekirjoitusta** viedyn varmenteen ja liitä se sitten **tunnisteen** tekstiruutu.  

        >[AZURE.TIP] Lisätietoja on artikkelissa [noutamisesta varmenteen allekirjoitusarvo](http://youtu.be/YKQF266SAxI)

    2.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa lähettämä** -valintasivun **SAML SSO URL-osoite** -arvo kopioi ja liitä se sitten **Tunnistetietojen toimittaja HTTP SAML URL** -tekstiruutuun.
    3.  Valitse **Tallenna muutokset**.

10. Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-envoy-tutorial/IC776786.png "Määritä kertakirjautuminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Ei ei toimi, voit määrittää käyttäjän lähettämä valmistelu.  
Kun määritetty käyttäjä yrittää kirjautua käyttämällä access-paneelin lähettämä, lähettämä tarkistaa, onko käyttäjän olemassa.  
Jos ei ole käyttäjätiliä käytettävissä vielä, se luodaan automaattisesti lähettämä mukaan.
##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-envoy-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä lähettämä, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **lähettämä **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-envoy-tutorial/IC776787.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-envoy-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).
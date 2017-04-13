<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi AnswerHub | Microsoft Azure" 
    description="Opettele käyttämään AnswerHub Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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
    ms.date="10/10/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-answerhub"></a>Opetusohjelma: AnswerHub Azure Active Directory-integrointi

Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja [AnswerHub](http://www.dzonesoftware.com/products/answerhub-question-answer-software)integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Käytössä [AnswerHub](http://www.dzonesoftware.com/products/answerhub-question-answer-software) kertakirjautumisen tilaus

Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt AnswerHub Azure AD-käyttäjien saa oikeuden kertakirjauksen AnswerHub yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.

Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  AnswerHub sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-answerhub-tutorial/IC785165.png "Skenaario")

##<a name="enabling-the-application-integration-for-answerhub"></a>AnswerHub sovelluksen integrointi ottaminen käyttöön

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön AnswerHub sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-answerhub-perform-the-following-steps"></a>AnswerHub sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-answerhub-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-answerhub-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-answerhub-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-answerhub-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **AnswerHub**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-answerhub-tutorial/IC785166.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **AnswerHub**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![AnswerHub] (./media/active-directory-saas-answerhub-tutorial/IC785167.png "AnswerHub")

##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen AnswerHub käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.  
Tämän toimintosarjan osana on base-64-koodattu sertifikaattitiedosto luomiseen.  
Jos et ole tottunut näin, katso, [miten voit muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **AnswerHub** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-answerhub-tutorial/IC785168.png "Määritä kertakirjautuminen")

2.  **Miten käyttäjät voivat kirjautua AnswerHub haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-answerhub-tutorial/IC785169.png "Määritä kertakirjautuminen")

3.  **Määrittää sovelluksen URL-osoite** -sivulla **AnswerHub merkki-URL** -tekstiruutuun Kirjoita käyttämällä seuraavaa kaavaa "*https://company.answerhub.com*" URL-osoite ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-answerhub-tutorial/IC785170.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  **Määritä kertakirjautumisen osoitteessa AnswerHub** -sivulla Lataa sertifikaatti, valitse **Lataa**, ja Tallenna sertifikaattitiedosto paikallisesti tietokoneeseen.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-answerhub-tutorial/IC785171.png "Määritä kertakirjautuminen")

5.  Eri selainikkunassa Kirjaudu AnswerHub yrityksen sivuston järjestelmänvalvojan oikeuksilla.
    >[AZURE.NOTE] Jos tarvitset apua AnswerHub, ota [AnswerHub's tukihenkilöstöön](mailto:success@answerhub.com. ).








6.  Siirry **hallinta**.

7.  Valitse **käyttäjien ja ryhmien** -välilehti.

8.  Valitse siirtymisruudun vasemmassa reunassa, **Social asetukset** -osassa **SAML asetukset**.

9.  Valitse **IDP Config** -välilehti.

10. **IDP Config** -välilehden toimimalla seuraavasti:

    ![SAML-asetukset] (./media/active-directory-saas-answerhub-tutorial/IC785172.png "SAML-asetukset")

    1.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa AnswerHub** -valintasivun **Remote sisäänkirjautumisen URL-osoite** -arvo kopioi ja liitä se sitten **IDP kirjautuminen URL** -tekstiruutuun.
    2.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa AnswerHub** -valintasivun **Remote Kirjaudu URL-osoite** -arvo kopioi ja liitä se sitten **IDP Kirjaudu URL** -tekstiruutuun.
    3.  Azure perinteinen portaalissa-sivulla **Määritä kertakirjautumisen osoitteessa AnswerHub** valintaikkunan Kopioi **Nimimuotoa tunnus** -arvon ja liitä se sitten **IDP nimimuotoa tunnus** -tekstiruutuun.
    4.  Valitse **avaimet ja varmenteet**.

11. Avaimet ja varmenteet-välilehden toimimalla seuraavasti:

    ![Avaimet ja varmenteet] (./media/active-directory-saas-answerhub-tutorial/IC785173.png "Avaimet ja varmenteet")

    1.  Luo **base-64-koodattu** tiedosto ladattu varmennetta.  

        >[AZURE.TIP] Lisätietoja on artikkelissa [muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o)

    2.  Avaa base-64-koodattu varmennetta Muistiossa, kopioi Leikepöydän sisällön se ja liitä se **IDP julkinen avain (x 509 Format)** -tekstiruutuun.
    3.  Valitse **Tallenna**.

12. Valitse **IDP Config** -välilehdessä **Tallenna**.

13. Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-answerhub-tutorial/IC785174.png "Määritä kertakirjautuminen")

##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu

Jos haluat ottaa käyttöön Azure AD-käyttäjät voivat kirjautua sisään AnswerHub, ne on valmisteltava AnswerHub kyselyjä.  
AnswerHub valmistelu on manuaalinen tehtävä.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Voit määrittää käyttäjän valmistelu toimimalla seuraavasti:

1.  Kirjaudu sisään **AnswerHub** yrityksesi sivuston järjestelmänvalvojana.

2.  Siirry **hallinta**.

3.  Valitse **käyttäjät ja ryhmät** -välilehti.

4.  Valitse siirtymisruudun vasemmassa reunassa, **Käyttäjien hallinta** -kohdassa **Luo tai tuo käyttäjät**.

    ![Käyttäjät ja ryhmät] (./media/active-directory-saas-answerhub-tutorial/IC785175.png "Käyttäjät ja ryhmät")

5.  Kirjoita **sähköpostiosoite**, **käyttäjänimi** ja **salasana** kelvollinen Azure Active Directory-tilin haluat säännöstä liittyvät tekstiruutujen kyselyjä ja valitse sitten **Tallenna**.

>[AZURE.NOTE] Voit käyttää mitä tahansa muita AnswerHub käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä AnswerHub säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen

Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-answerhub-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä AnswerHub, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **AnswerHub **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-answerhub-tutorial/IC785176.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-answerhub-tutorial/IC767830.png "Kyllä")

Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).

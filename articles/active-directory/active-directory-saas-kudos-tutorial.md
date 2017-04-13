<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi pisteitä | Microsoft Azure" 
    description="Opettele käyttämään pisteitä Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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

#<a name="tutorial-azure-active-directory-integration-with-kudos"></a>Opetusohjelma: Pisteitä Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja pisteitä.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Pisteitä palvelutili
  
Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt pisteitä Azure AD-käyttäjien saa oikeuden kertakirjauksen pisteitä yrityksen sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Pisteitä sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-kudos-tutorial/IC787799.png "Skenaario")
##<a name="enabling-the-application-integration-for-kudos"></a>Pisteitä sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön pisteitä sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-kudos-perform-the-following-steps"></a>Pisteitä sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-kudos-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-kudos-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-kudos-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-kudos-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **pisteitä**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-kudos-tutorial/IC787800.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **pisteitä**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Pisteitä] (./media/active-directory-saas-kudos-tutorial/IC787801.png "Pisteitä")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen pisteitä käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.  
Tämän toimintosarjan osana on base-64-koodattu sertifikaattitiedosto luomiseen.  
Jos et ole tottunut näin, katso, [miten voit muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **pisteitä** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-kudos-tutorial/IC787802.png "Määritä kertakirjautuminen")

2.  **Miten käyttäjät voivat kirjautua pisteitä haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-kudos-tutorial/IC787803.png "Määritä kertakirjautuminen")

3.  **Määritä sovelluksen URL-osoite** -sivulla **Pisteitä merkki-URL** -tekstiruutuun Kirjoita käyttämällä seuraavaa kaavaa "*https://company.kudosnow.com*" URL-osoite ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-kudos-tutorial/IC787804.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  Valitse **Määritä kertakirjautumisen osoitteessa pisteitä** -sivulla valitsemalla **Lataa**ja Tallenna sertifikaattitiedosto tietokoneeseen.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-kudos-tutorial/IC787805.png "Määritä kertakirjautuminen")

5.  Eri selainikkunassa Kirjaudu pisteitä yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

6.  Valitse ylä-valikossa **asetukset**.

    ![Asetukset] (./media/active-directory-saas-kudos-tutorial/IC787806.png "Asetukset")

7.  Valitse **integroinnit \> SSO**.

8.  **SSO** -osassa seuraavasti:

    ![Kertakirjautuminen] (./media/active-directory-saas-kudos-tutorial/IC787807.png "Kertakirjautuminen")

    1.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa pisteitä** -valintasivun **Sign-palvelun URL** -arvo kopioi ja liitä se sitten **Kirjaudu URL** -tekstiruutuun.
    2.  Luo **base-64-koodattu** tiedosto ladattu varmennetta.  

        >[AZURE.TIP]
        Lisätietoja on artikkelissa [muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o)

    3.  Avaa base-64-koodattu varmennetta Muistiossa, kopioi se sisällön Leikepöydän ja liitä se **X.509-varmenne** -tekstiruutu
    4.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa pisteitä** -valintasivun **Yksittäisen Sign-Out palvelun URL-osoite** -arvo kopioi ja liitä se sitten **Kirjaudu ulos-URL** -tekstiruutuun.
    5.  Kirjoita yrityksen nimi **Your pisteitä URL** -tekstiruutuun.
    6.  Valitse **Tallenna**.

9.  Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-kudos-tutorial/IC787808.png "Määritä kertakirjautuminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Jos haluat ottaa käyttöön Azure AD-käyttäjät voivat kirjautua sisään pisteitä, ne on valmisteltava kyselyjä pisteitä.  
Pisteitä valmistelu on manuaalinen tehtävä.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Valmistelu käyttäjätilit, toimi seuraavasti:

1.  Kirjaudu sisään **pisteitä** yrityksesi sivuston järjestelmänvalvojana.

2.  Valitse ylä-valikossa **asetukset**.

    ![Asetukset] (./media/active-directory-saas-kudos-tutorial/IC787806.png "Asetukset")

3.  Valitse **käyttäjän järjestelmänvalvoja**.

4.  Valitse **käyttäjät** -välilehti ja valitse sitten **Lisää käyttäjä**.

    ![Käyttäjien hallinta] (./media/active-directory-saas-kudos-tutorial/IC787809.png "Käyttäjien hallinta")

5.  Valitse **Lisää käyttäjä** -osassa seuraavasti:

    ![Käyttäjän lisääminen] (./media/active-directory-saas-kudos-tutorial/IC787810.png "Käyttäjän lisääminen")

    1.  Kirjoita **Etunimi**, **Sukunimi**, **sähköpostin** ja muut tiedot, jos haluat säännöstä kyselyjä liittyvät tekstiruutujen kelvollinen Azure Active Directory-tilin.
    2.  Valitse **Luo käyttäjä**.

>[AZURE.NOTE]Voit käyttää mitä tahansa muita pisteitä käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä pisteitä säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-kudos-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä pisteitä, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **pisteitä **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-kudos-tutorial/IC787811.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-kudos-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).
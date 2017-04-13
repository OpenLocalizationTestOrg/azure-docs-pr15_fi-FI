<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Pagerduty | Microsoft Azure" 
    description="Opettele käyttämään Pagerduty Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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

#<a name="tutorial-azure-active-directory-integration-with-pagerduty"></a>Opetusohjelma: Pagerduty Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja Pagerduty integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Pagerduty palvelutili
  
Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt Pagerduty Azure AD-käyttäjien saa oikeuden kertakirjauksen Pagerduty yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Pagerduty sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-pagerduty-tutorial/IC778528.png "Skenaario")
##<a name="enabling-the-application-integration-for-pagerduty"></a>Pagerduty sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön Pagerduty sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-pagerduty-perform-the-following-steps"></a>Pagerduty sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure-hallinta-portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-pagerduty-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-pagerduty-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-pagerduty-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-pagerduty-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **Pagerduty**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-pagerduty-tutorial/IC778529.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **Pagerduty**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![PagerDuty] (./media/active-directory-saas-pagerduty-tutorial/IC778530.png "PagerDuty")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen Pagerduty käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.  
Tämän toimintosarjan osana on base-64-koodattu sertifikaattitiedosto luomiseen.  
Jos et ole tottunut näin, katso, [miten voit muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **Pagerduty** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-pagerduty-tutorial/IC778531.png "Määritä kertakirjautuminen")

2.  **Miten käyttäjät voivat kirjautua Pagerduty haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-pagerduty-tutorial/IC778532.png "Määritä kertakirjautuminen")

3.  Kirjoita **Määritä sovelluksen URL-osoite** -sivulla **Pagerduty merkki-URL** -tekstiruutuun URL-osoite käyttämällä seuraavaa kaavaa "*https://\<vuokraajan nimi\>. Pagerduty.com*", ja valitse sitten **Seuraava**.

    ![Määritä sovelluksen URL-osoite] (./media/active-directory-saas-pagerduty-tutorial/IC778533.png "Määritä sovelluksen URL-osoite")

4.  Valitse **Määritä kertakirjautumisen osoitteessa Pagerduty** -sivulla valitsemalla **Lataa**ja Tallenna sertifikaattitiedosto tietokoneeseen.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-pagerduty-tutorial/IC778534.png "Määritä kertakirjautuminen")

5.  Eri selainikkunassa Kirjaudu Pagerduty yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

6.  Valitse ylä-valikossa **Tiliasetukset**.

    ![Tilin asetukset] (./media/active-directory-saas-pagerduty-tutorial/IC778535.png "Tilin asetukset")

7.  Valitse **kertakirjautumisen**.

    ![Kertakirjautuminen] (./media/active-directory-saas-pagerduty-tutorial/IC778536.png "Kertakirjautuminen")

8.  Valitse Ota käyttöön kertakirjautumisen (SSO)-sivulla toimimalla seuraavasti:

    ![Ottaa käyttöön kertakirjautumisen] (./media/active-directory-saas-pagerduty-tutorial/IC778537.png "Ottaa käyttöön kertakirjautumisen")

    1.  Luo **base-64-koodattu** tiedosto ladattu varmennetta.  

        >[AZURE.TIP] Lisätietoja on artikkelissa [muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o)

    2.  Avaa base-64-koodattu varmennetta Muistiossa, kopioi se sisällön Leikepöydän ja liitä se **X.509-varmenne** -tekstiruutu
    3.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Pagerduty** valintaikkunan-sivulla kopioi **Remote sisäänkirjautumisen URL-osoite** -arvo ja liitä se sitten **Kirjautuminen URL** -tekstiruutuun.
    4.  Azure perinteinen portaalissa-sivulla **Määritä kertakirjautumisen osoitteessa Pagerduty** valintaikkunan **Remote Kirjaudu URL-osoite** -arvo kopioi ja liitä se sitten **Kirjaudu ulos URL** -tekstiruutuun.
    5.  Valitse **Ota käyttöön kertakirjautumisen**.
    6.  Valitse **Tallenna muutokset**.

9.  Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-pagerduty-tutorial/IC778538.png "Määritä kertakirjautuminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Jos haluat ottaa käyttöön Azure AD-käyttäjät voivat kirjautua sisään Pagerduty, ne on valmisteltava Pagerduty kyselyjä.  
Pagerduty valmistelu on manuaalinen tehtävä.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Valmistelu käyttäjätilit, toimi seuraavasti:

1.  Kirjaudu sisään **Pagerduty** alihallintaan.

2.  Valitse ylä-valikossa Valitse **käyttäjiä**.

3.  Valitse **Lisää käyttäjiä**.

    ![Käyttäjien lisääminen] (./media/active-directory-saas-pagerduty-tutorial/IC778539.png "Käyttäjien lisääminen")

4.  Kirjoita **ryhmän kutsu** -valintaikkuna **Etunimi ja Sukunimi** ja **haluat valmistella, valitse **Lisää**ja valitse sitten **Lähetä kutsuu**Azure AD-käyttäjän sähköpostiosoite** .

    ![Kutsu ryhmän] (./media/active-directory-saas-pagerduty-tutorial/IC778540.png "Kutsu ryhmän")

    >[AZURE.NOTE] Kaikki lisätyt käyttäjät saavat kutsu PagerDuty-tilin luominen.

>[AZURE.NOTE] Voit käyttää mitä tahansa muita Pagerduty käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä Pagerduty säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-pagerduty-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä Pagerduty, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **Pagerduty **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-pagerduty-tutorial/IC778541.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-pagerduty-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).
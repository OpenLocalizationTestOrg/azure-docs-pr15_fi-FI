<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Onit | Microsoft Azure" 
    description="Opettele käyttämään Onit Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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

#<a name="tutorial-azure-active-directory-integration-with-onit"></a>Opetusohjelma: Onit Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja Onit integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Käytössä Onit kertakirjautumisen tilaus
  
Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt Onit Azure AD-käyttäjien saa oikeuden kertakirjauksen Onit yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Onit sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-onit-tutorial/IC791166.png "Skenaario")
##<a name="enabling-the-application-integration-for-onit"></a>Onit sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön Onit sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-onit-perform-the-following-steps"></a>Onit sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-onit-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-onit-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-onit-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-onit-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **Onit**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-onit-tutorial/IC791167.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **Onit**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Onit] (./media/active-directory-saas-onit-tutorial/IC795325.png "Onit")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen Onit käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.  
Kertakirjautumisen varten Onit määrittäminen edellyttää, että allekirjoitus arvon noutaa varmenne.  
Jos et ole tottunut näin, katso, [miten voit hakea varmenteen allekirjoitus-arvon](http://youtu.be/YKQF266SAxI).
  
Onit sovelluksen odottaa SAML vahvistukset tietyssä muodossa, jossa pyydetään Lisää mukautettujen määritteiden yhdistämismäärityksiä **saml suojaustunnuksen määritteet** -määrityksiin.  
Seuraavassa näyttökuvassa näkyy esimerkki tämän.

![Kertakirjautuminen] (./media/active-directory-saas-onit-tutorial/IC791168.png "Kertakirjautuminen")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa **Onit** sovelluksen integrointi-sivun yläreunassa valikosta **SAML suojaustunnuksen määritteet** -valintaikkunan avaaminen **määritteet** .

    ![Määritteet] (./media/active-directory-saas-onit-tutorial/IC791169.png "Määritteet")

2.  Lisää pakollinen määrite yhdistämismääritykset, toimi seuraavasti:

    
  	|Määritteen nimi|Määritteen arvo|
  	|---|---|
  	|Nimi|User.UserPrincipalName|
  	|sähköpostin|User.Mail|

    1.  Tietoja kullekin riville edellä olevan taulukon Valitse **Lisää käyttäjä-määrite**.
    2.  Kirjoita määritteen nimi näkyy kyseisen rivin **Määritteen nimi** -tekstiruutuun.
    3.  **Määritteen arvo** -luettelosta kyseisen rivin määritteen arvo.
    4.  Valitse **Valmis**.

3.  Valitse **Ota muutokset käyttöön**.

4.  Selaimessa Valitse **takaisin** **Pika-aloitus** -valintaikkunan avaaminen uudelleen.

5.  Valitse **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-onit-tutorial/IC791170.png "Kertakirjautumisen määrittäminen")

6.  **Miten käyttäjät voivat kirjautua Onit haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-onit-tutorial/IC791171.png "Kertakirjautumisen määrittäminen")

7.  Kirjoita **Määritä sovelluksen URL-osoite** -sivulla **Onit merkki-URL** -tekstiruutuun käyttämä käyttäjien kirjautua Onit sovelluksen URL-osoite (esimerkiksi: "*https://ms-sso-test.onit.com*"), ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-onit-tutorial/IC791172.png "Sovelluksen URL-Osoitteen määrittäminen")

8.  **Määritä kertakirjautumisen osoitteessa Onit** -sivulla Lataa sertifikaatti, valitse **Lataa**, ja Tallenna sertifikaattitiedosto paikallisesti tietokoneeseen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-onit-tutorial/IC791173.png "Kertakirjautumisen määrittäminen")

9.  Eri selainikkunassa Kirjaudu Onit yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

10. Valitse ylä-valikossa **hallinta**.

    ![Hallinta] (./media/active-directory-saas-onit-tutorial/IC791174.png "Hallinta")

11. Valitse **Muokkaa Corporation**.

    ![Muokkaa Corporation] (./media/active-directory-saas-onit-tutorial/IC791175.png "Muokkaa Corporation")

12. Valitse **Suojaus** -välilehti.

    ![Muokkaa yrityksen tiedot] (./media/active-directory-saas-onit-tutorial/IC791176.png "Muokkaa yrityksen tiedot")

13. Valitse **Suojaus** -välilehden toimimalla seuraavasti:

    ![Kertakirjautuminen] (./media/active-directory-saas-onit-tutorial/IC791177.png "Kertakirjautuminen")

    1.  Valitse **Käyttöoikeuksien määrittäminen**, **kertakirjautuminen ja salasana**.
    2.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Onit** -valintasivun **Remote sisäänkirjautumisen URL-osoite** -arvo kopioi ja liitä se sitten **Idp kohde-URL** -tekstiruutuun.
    3.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Onit** -valintasivun **Remote Kirjaudu URL-osoite** -arvo kopioi ja liitä se sitten **Idp Kirjaudu URL** -tekstiruutuun.
    4.  Viedyn varmenteen **allekirjoitusta** kopioi ja liitä se **Idp varmenteen tunnisteen (SHA1)** -tekstiruutuun.  

        >[AZURE.TIP] Lisätietoja on artikkelissa [noutamisesta varmenteen allekirjoitusarvo](http://youtu.be/YKQF266SAxI)

    5.  Valitse **SAML** **SSO tyyppi**.
    6.  Kirjoita **SSO-kirjautumisen painikkeen teksti** -tekstiruutuun haluamasi Painiketeksti.
    7.  Valitse **Kirjautuminen ja Kertakirjautuminen: N seuraavat toimialueet/käyttäjät**, kirjoita testikäyttäjän sähköpostiosoite liittyvät tekstiruutu ja valitse sitten **Päivitä**.
        ![Muokkaa Corporation] (./media/active-directory-saas-onit-tutorial/IC791178.png "Muokkaa Corporation")

14. Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-onit-tutorial/IC791179.png "Kertakirjautumisen määrittäminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Jos haluat ottaa käyttöön Azure AD-käyttäjät voivat kirjautua sisään Onit, ne on valmisteltava Onit kyselyjä.  
Onit valmistelu on manuaalinen tehtävä.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Voit määrittää käyttäjän valmistelu toimimalla seuraavasti:

1.  Kirjaudu **Onit** yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

2.  Valitse **Lisää käyttäjä**.

    ![Hallinta] (./media/active-directory-saas-onit-tutorial/IC791180.png "Hallinta")

3.  Valitse **Lisää käyttäjä** -sivulla toimimalla seuraavasti:

    ![Käyttäjän lisääminen] (./media/active-directory-saas-onit-tutorial/IC791181.png "Käyttäjän lisääminen")

    1.  **Nimi** ja kelvollinen AAD tilin **Sähköpostiosoite** , jonka haluat valmistella liittyvät tekstiruutujen kyselyjä.
    2.  Valitse **Luo**.  

        >[AZURE.NOTE] Tilin omistajan saavat sähköpostiviestin, mukaan lukien linkin Vahvista tili, ennen kuin se on aktiivinen.

>[AZURE.NOTE] Voit käyttää mitä tahansa muita Onit käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä Onit säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-onit-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä Onit, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **Onit **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-onit-tutorial/IC791182.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-onit-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).
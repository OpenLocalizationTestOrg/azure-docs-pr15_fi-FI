<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi FreshService | Microsoft Azure" 
    description="Opettele käyttämään FreshService Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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

#<a name="tutorial-azure-active-directory-integration-with-freshservice"></a>Opetusohjelma: FreshService Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja FreshService integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Käytössä FreshService kertakirjautumisen tilaus
  
Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt FreshService Azure AD-käyttäjien saa oikeuden kertakirjauksen käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  FreshService sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-freshservice-tutorial/IC790807.png "Skenaario")
##<a name="enabling-the-application-integration-for-freshservice"></a>FreshService sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön FreshService sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-freshservice-perform-the-following-steps"></a>FreshService sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-freshservice-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-freshservice-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-freshservice-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-freshservice-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **FreshService**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-freshservice-tutorial/IC790808.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **FreshService**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Freshservice] (./media/active-directory-saas-freshservice-tutorial/IC790809.png "Freshservice")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen FreshService käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.  
Kertakirjautumisen varten FreshService määrittäminen edellyttää, että allekirjoitus arvon noutaa varmenne.  
Jos et ole tottunut näin, katso, [miten voit hakea varmenteen allekirjoitus-arvon](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **FreshService** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-freshservice-tutorial/IC790810.png "Kertakirjautumisen määrittäminen")

2.  **Miten käyttäjät voivat kirjautua FreshService haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-freshservice-tutorial/IC790811.png "Kertakirjautumisen määrittäminen")

3.  Kirjoita **Määrittäminen sovelluksen URL-osoite** -sivulla **FreshService merkki-URL** -tekstiruutuun käyttää käyttäjien kirjautua Freshdesk sovelluksen URL-osoite (esimerkiksi: "*http://democompany.freshservice.com/*"), ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-freshservice-tutorial/IC790812.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  **Määritä kertakirjautumisen osoitteessa FreshService** -sivulla Lataa sertifikaatti, valitse **Lataa**, ja Tallenna sertifikaattitiedosto paikallisesti tietokoneeseen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-freshservice-tutorial/IC790813.png "Kertakirjautumisen määrittäminen")

5.  Eri selainikkunassa Kirjaudu FreshService yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

6.  Valitse ylä-valikossa Valitse **järjestelmänvalvoja**.

    ![Järjestelmänvalvoja] (./media/active-directory-saas-freshservice-tutorial/IC790814.png "Järjestelmänvalvoja")

7.  **Asiakkaan Portal**-kohdassa **turvallisuus**.

    ![Tietoturva] (./media/active-directory-saas-freshservice-tutorial/IC790815.png "Tietoturva")

8.  Valitse **Suojaus** -kohdassa toimimalla seuraavasti:

    ![Kertakirjautuminen] (./media/active-directory-saas-freshservice-tutorial/IC790816.png "Kertakirjautuminen")

    1.  Siirry **yksi merkki OnON**.
    2.  Valitse **SAML SSO**.
    3.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa FreshService** -valintasivun **Remote sisäänkirjautumisen URL-osoite** -arvo kopioi ja liitä se sitten **SAML kirjautuminen URL** -tekstiruutuun.
    4.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa FreshService** -valintasivun **Remote Kirjaudu URL-osoite** -arvo kopioi ja liitä se sitten **Kirjaudu ulos URL** -tekstiruutuun.
    5.  Kopioi **allekirjoitusta** viedyn varmenteen ja liitä se sitten **Suojaus varmenteen tunnisteen** tekstiruutu.
    
        >[AZURE.TIP]Lisätietoja on artikkelissa [noutamisesta varmenteen allekirjoitusarvo](http://youtu.be/YKQF266SAxI)

9.  Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-freshservice-tutorial/IC790817.png "Kertakirjautumisen määrittäminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Jos haluat ottaa käyttöön Azure AD-käyttäjät voivat kirjautua sisään FreshService, ne on valmisteltava FreshService kyselyjä.  
FreshService valmistelu on manuaalinen tehtävä.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Valmistelu käyttäjätilit, toimi seuraavasti:

1.  Kirjaudu sisään **FreshService** yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

2.  Valitse ylä-valikossa Valitse **järjestelmänvalvoja**.

    ![Järjestelmänvalvoja] (./media/active-directory-saas-freshservice-tutorial/IC790814.png "Järjestelmänvalvoja")

3.  Valitse **Käyttäjien hallinta** -osassa **pyytäjät**.

    ![Pyytäjät] (./media/active-directory-saas-freshservice-tutorial/IC790818.png "Pyytäjät")

4.  Valitse **Uusi pyytäjä**.

    ![Uusi pyytäjät] (./media/active-directory-saas-freshservice-tutorial/IC790819.png "Uusi pyytäjät")

5.  **Uusi pyytäjä** -osassa seuraavasti:

    ![Uusi pyytäjä] (./media/active-directory-saas-freshservice-tutorial/IC790820.png "Uusi pyytäjä")

    1.  Kirjoita kelvollinen Azure Active Directory-tilin haluat **Etunimi** ja **sähköpostin** määritteiden valmistelu liittyvät tekstiruutujen.
    2.  Valitse **Tallenna**.

    >[AZURE.NOTE] Azure Active Directory-tilin omistaja saavat sähköpostiviestin, mukaan lukien Vahvista tili, ennen kuin se on aktiivinen linkki

>[AZURE.NOTE] Voit käyttää mitä tahansa muita FreshService käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä FreshService säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-freshservice-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä FreshService, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **FreshService **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-freshservice-tutorial/IC790821.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-freshservice-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).
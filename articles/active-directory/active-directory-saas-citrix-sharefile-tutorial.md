<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Citrix ShareFile | Microsoft Azure" 
    description="Opettele käyttämään Citrix ShareFile Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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

#<a name="tutorial-azure-active-directory-integration-with-citrix-sharefile"></a>Opetusohjelma: Citrix ShareFile Azure Active Directory-integrointi

Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja Citrix ShareFile integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Citrix ShareFile palvelutili

Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt Citrix ShareFile Azure AD-käyttäjien saa oikeuden kertakirjauksen Citrix ShareFile yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.

Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Citrix ShareFile sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773620.png "Skenaario")
##<a name="enabling-the-application-integration-for-citrix-sharefile"></a>Citrix ShareFile sovelluksen integrointi ottaminen käyttöön

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön Citrix ShareFile sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-citrix-sharefile-perform-the-following-steps"></a>Citrix ShareFile sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-citrix-sharefile-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-citrix-sharefile-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-citrix-sharefile-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-citrix-sharefile-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **Citrix ShareFile**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773621.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **Citrix ShareFile**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Citrix ShareFile] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773622.png "Citrix ShareFile")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen Citrix ShareFile käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **Citrix ShareFile** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Ottaa käyttöön kertakirjautumisen] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773623.png "Ottaa käyttöön kertakirjautumisen")

2.  **Miten käyttäjät voivat kirjautua Citrix ShareFile haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773624.png "Kertakirjautumisen määrittäminen")

3.  Kirjoita **Määritä sovelluksen URL-osoite** -sivulla **Citrix ShareFile merkki-URL** -tekstiruutuun URL-osoite käyttämällä seuraavaa kaavaa `https://<tenant-name>.shareFile.com`, ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773625.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  Valitse **Määritä kertakirjautumisen osoitteessa Citrix ShareFile** -sivulla Lataa sertifikaatti, valitsemalla **Lataa**, ja Tallenna sertifikaattitiedosto tietokoneeseen.

    ![ConfigureSingle Sign-On] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773626.png "ConfigureSingle Sign-On")

5.  Eri selainikkunassa Kirjaudu **Citrix ShareFile** yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

6.  Valitse **järjestelmänvalvoja**-sivun työkalurivillä.

7.  Valitse vasemmassa siirtymisruudussa **Määrittäminen kertakirjautumisen**.

    ![Tilinhallinta] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773627.png "Tilinhallinta")

8.  Valitse **kertakirjautumisen / SAML 2.0-määritys** valintasivun **Perusasetukset**-kohdassa toimimalla seuraavasti:

    ![Kertakirjautuminen] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773628.png "Kertakirjautuminen")

    1.  Valitse **Ota käyttöön SAML**.
    2.  Kopioi Azure perinteinen portaaliin, valitse **Määritä kertakirjautumisen osoitteessa Citrix ShareFile** -valintasivun **Kohteen tunnus** arvo ja liitä se kyselyjä **Your IDP myöntäjä / kohteen tunnus** tekstiruutu.
    3.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Citrix ShareFile** -valintasivun **Remote sisäänkirjautumisen URL-osoite** -arvo kopioi ja liitä se sitten **Kirjautuminen URL** -tekstiruutuun.
    4.  Azure perinteinen portaalissa **Määritä kertakirjautumisen osoitteessa Citrix ShareFile** valintaikkunan sivulla **Remote Kirjaudu URL-osoite** -arvo kopioi ja liitä se sitten **Kirjaudu ulos URL** -tekstiruutuun.
    5.  Valitse **Muuta** **X.509-varmenne** -kentän vieressä ja lataa perinteinen Azure AD-portaalista ladattuun sertifikaatin.
        ![Perusasetukset] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773629.png "Perusasetukset")

9.  Valitse **Tallenna** Citrix ShareFile hallinta-portaalissa.

10. Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773630.png "Määritä kertakirjautuminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu

Jotta voit kirjautua Citrix ShareFile Azure AD-käyttäjien käyttöön, ne on valmisteltava Citrix ShareFile kyselyjä.  
Citrix ShareFile valmistelu on manuaalinen tehtävä.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Valmistelu käyttäjätilit, toimi seuraavasti:

1.  Kirjaudu sisään **Citrix ShareFile** alihallintaan.

2.  Valitse **käyttäjien hallinta \> Hallitse käyttäjien aloitus \> + Luo työntekijä**.

    ![Luo työntekijä] (./media/active-directory-saas-citrix-sharefile-tutorial/IC781050.png "Luo työntekijä")

3.  Määritä **Sähköposti**, **Etunimi** ja **Sukunimi** , kelvollinen Azure AD pankkitilin haluat säätää.

    ![Perustiedot] (./media/active-directory-saas-citrix-sharefile-tutorial/IC799951.png "Perustiedot")

4.  Valitse **Lisää käyttäjä**.

    >[AZURE.NOTE] AAD tilin omistaja vastaanottaa sähköpostia ja johtavan linkin Vahvista tilin, ennen kuin se on aktiivinen.

>[AZURE.NOTE] Voit käyttää mitä tahansa muita Citrix ShareFile käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä Citrix ShareFile säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen

Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-citrix-sharefile-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä Citrix ShareFile, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **Citrix ShareFile **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773631.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-citrix-sharefile-tutorial/IC767830.png "Kyllä")

Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).

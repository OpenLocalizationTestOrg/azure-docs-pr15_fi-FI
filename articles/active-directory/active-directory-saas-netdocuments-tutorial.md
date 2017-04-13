<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi NetDocuments | Microsoft Azure" 
    description="Opettele käyttämään NetDocuments Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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

#<a name="tutorial-azure-active-directory-integration-with-netdocuments"></a>Opetusohjelma: NetDocuments Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja NetDocuments integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   NetDocuments palvelutili
  
Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt NetDocuments Azure AD-käyttäjien saa oikeuden kertakirjauksen NetDocuments yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  NetDocuments sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-netdocuments-tutorial/IC795040.png "Skenaario")
##<a name="enabling-the-application-integration-for-netdocuments"></a>NetDocuments sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön NetDocuments sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-netdocuments-perform-the-following-steps"></a>NetDocuments sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-netdocuments-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-netdocuments-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-netdocuments-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-netdocuments-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **NetDocuments**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-netdocuments-tutorial/IC795041.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **NetDocuments**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![NetDocuments] (./media/active-directory-saas-netdocuments-tutorial/IC795042.png "NetDocuments")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen NetDocuments käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.  
Kertakirjautumisen varten NetDocuments määrittäminen edellyttää, että allekirjoitus arvon noutaa varmenne.  
Jos et ole tottunut näin, katso, [miten voit hakea varmenteen allekirjoitus-arvon](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **NetDocuments** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-netdocuments-tutorial/IC795043.png "Kertakirjautumisen määrittäminen")

2.  **Miten käyttäjät voivat kirjautua NetDocuments haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-netdocuments-tutorial/IC795044.png "Kertakirjautumisen määrittäminen")

3.  Tee **Määrittäminen sovelluksen URL-osoite** -sivulla seuraavat toimet:

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-netdocuments-tutorial/IC795045.png "Sovelluksen URL-Osoitteen määrittäminen")

    1.  Kirjoita **Merkki-URL** -tekstiruutuun käyttää käyttäjien kirjautua NetDocuments sovelluksen URL-osoite (esimerkiksi: "*https://vault.netvoyage.com/neWeb2/docCent.aspx?whr=CA-JI1BG3H1*").
    2.  Kirjoita **NetDocuments vastaa URL** -tekstiruutuun on sama arvo, joka on kirjoitettu- **Merkki-URL** -tekstiruutuun.  

        >[AZURE.NOTE]Löydät oikean arvon **Liitetty tunnistetiedot** -valintaikkunan lopussa (katso näyttökuva vaihe 9).

    3.  Valitse **Seuraava**

4.  **Määritä kertakirjautumisen osoitteessa NetDocuments** -sivulla Lataa sertifikaatti, valitse **Lataa**, ja Tallenna sertifikaattitiedosto paikallisesti tietokoneeseen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-netdocuments-tutorial/IC795046.png "Kertakirjautumisen määrittäminen")

5.  Eri selainikkunassa Kirjaudu NetDocuments yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

6.  Valitse **järjestelmänvalvoja**.

7.  Valitse **Lisää ja Poista käyttäjät ja ryhmät**.

    ![Säilöön] (./media/active-directory-saas-netdocuments-tutorial/IC795047.png "Säilöön")

8.  Valitse **Määritä todennus Lisäasetukset**.

    ![Määritä todennus Lisäasetukset] (./media/active-directory-saas-netdocuments-tutorial/IC795048.png "Määritä todennus Lisäasetukset")

9.  Valitse **Liitetty tunnistetiedot** -valintaikkunassa seuraavasti:

    ![Liitetty Identitty] (./media/active-directory-saas-netdocuments-tutorial/IC795049.png "Liitetty Identitty")

    1.  **Organisaation ulkopuolisten tunnistetietojen palvelintyyppi**Valitse **Active Directory-liittoutumispalvelut**.
    2.  Valitse **Valitse tiedosto**, ladatut metatiedot-tiedoston lataaminen.
    3.  Valitse **OK**.

10. Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-netdocuments-tutorial/IC795050.png "Kertakirjautumisen määrittäminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Jos haluat ottaa käyttöön Azure AD-käyttäjät voivat kirjautua sisään NetDocuments, ne on valmisteltava NetDocuments kyselyjä.  
NetDocuments valmistelu on manuaalinen tehtävä.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Voit määrittää käyttäjän valmistelu toimimalla seuraavasti:

1.  Hyvinkin muuttaa mielesi voin **NetDocuments** yrityksesi sivuston järjestelmänvalvojana.

2.  Valitse ylä-valikossa Valitse **järjestelmänvalvoja**.

    ![Järjestelmänvalvoja] (./media/active-directory-saas-netdocuments-tutorial/IC795051.png "Järjestelmänvalvoja")

3.  Valitse **Lisää ja Poista käyttäjät ja ryhmät**.

    ![Säilöön] (./media/active-directory-saas-netdocuments-tutorial/IC795047.png "Säilöön")

4.  Kirjoita **Sähköpostiosoite** -tekstiruutuun sen sähköpostitilin osoite, kelvollinen Azure Active Directory-tilin haluat säätää, ja valitse sitten **Lisää käyttäjä**.

    ![Sähköpostiosoite] (./media/active-directory-saas-netdocuments-tutorial/IC795053.png "Sähköpostiosoite")

    >[AZURE.NOTE]Azure Active Directory-tilin omistaja saavat sähköpostiviestin, joka sisältää linkin Vahvista tili, ennen kuin se on aktiivinen.

>[AZURE.NOTE]Voit käyttää mitä tahansa muita NetDocuments käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä NetDocuments säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-netdocuments-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä NetDocuments, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **NetDocuments **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-netdocuments-tutorial/IC795054.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-netdocuments-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).
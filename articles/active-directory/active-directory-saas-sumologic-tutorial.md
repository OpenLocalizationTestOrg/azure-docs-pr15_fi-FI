<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi SumoLogic | Microsoft Azure" 
    description="Opettele käyttämään SumoLogic Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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
    ms.date="09/11/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-sumologic"></a>Opetusohjelma: SumoLogic Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja SumoLogic integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   SumoLogic palvelutili
  
Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt SumoLogicwill voidaan voi yksittäinen merkki, että SumoLogic sovellukseen Azure AD-käyttäjien yrityksen (palveluntarjoajan aloittanut allekirjoitus-)-sivustoon tai käyttämällä [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  SumoLogic sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-sumologic-tutorial/IC778549.png "Skenaario")

##<a name="enabling-the-application-integration-for-sumologic"></a>SumoLogic sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön SumoLogic sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-sumologic-perform-the-following-steps"></a>SumoLogic sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-sumologic-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-sumologic-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-sumologic-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-sumologic-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **sumologic**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-sumologic-tutorial/IC778550.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **SumoLogic**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![SumoLogic] (./media/active-directory-saas-sumologic-tutorial/IC778551.png "SumoLogic")

##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen SumoLogic käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.  
Tämän toimintosarjan osana on base-64-koodattu varmenteen lataaminen oman SumoLogictenant.  
Jos et ole tottunut näin, katso, [miten voit muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **SumoLogic** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-sumologic-tutorial/IC778552.png "Määritä kertakirjautuminen")

2.  **Miten käyttäjät voivat kirjautua SumoLogic haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-sumologic-tutorial/IC778553.png "Määritä kertakirjautuminen")

3.  Kirjoita **Määritä sovelluksen URL-osoite** -sivulla **SumoLogic merkki-URL** -tekstiruutuun URL-osoite käyttämällä seuraavaa kaavaa "*https://\<vuokraajan nimi\>. SumoLogic.com*", ja valitse sitten **Seuraava**.

    ![Määritä aoo URL-osoite] (./media/active-directory-saas-sumologic-tutorial/IC778554.png "Määritä aoo URL-osoite")

4.  Valitse **Määritä kertakirjautumisen osoitteessa SumoLogic** -sivulla Lataa sertifikaatti, valitsemalla **Lataa**, ja Tallenna sertifikaattitiedosto tietokoneeseen.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-sumologic-tutorial/IC778555.png "Määritä kertakirjautuminen")

5.  Eri selainikkunassa Kirjaudu SumoLogic yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

6.  Siirry **hallinta \> suojauksen**.

    ![Hallinta] (./media/active-directory-saas-sumologic-tutorial/IC778556.png "Hallinta")

7.  Valitse **SAML**.

    ![Yleinen tietoturva-asetukset] (./media/active-directory-saas-sumologic-tutorial/IC778557.png "Yleinen tietoturva-asetukset")

8.  **Valitse määrityksen tai luoda uuden** luettelosta Valitse **Azure AD**ja valitse sitten **Määritä**.

    ![Määritä SAML 2.0] (./media/active-directory-saas-sumologic-tutorial/IC778558.png "Määritä SAML 2.0")

9.  Valitse **Määritä SAML 2.0** -valintaikkunassa seuraavasti:

    ![Määritä SAML 2.0] (./media/active-directory-saas-sumologic-tutorial/IC778559.png "Määritä SAML 2.0")

    1.  Kirjoita **Azure AD** **Määritysten nimi** -tekstiruutuun.
    2.  Valitse **Virheenkorjaus tilassa**.
    3.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa SumoLogic** valintaikkunan-sivulla kopioi **Myöntäjä URL-osoite** -arvo ja liitä se **myöntäjä** -tekstiruutuun.
    4.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa SumoLogic** valintaikkunan-sivulla kopioi **Käyttöoikeuksien pyytäminen URL-osoite** -arvo ja liitä se **Authn pyyntö URL** -tekstiruutuun.
    5.  Luo **Base-64-koodattu** tiedosto ladattu varmennetta.  

        >[AZURE.TIP] Lisätietoja on artikkelissa [muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o)

    6.  Avaa base-64-koodattu varmennetta Muistiossa, kopioi se sisällön Leikepöydän ja liitä koko sertifikaatin **X.509-varmenne** tekstiruutu.
    7.  Valitse **Sähköposti-määrite**, **Käytä SAML aihe**.
    8.  Valitse **SP aloittanut kirjautuminen määritys**.
    9.  Kirjoita **Login polku** -tekstiruutuun **Azure**.
    10. Valitse **Tallenna**.

10. Azure perinteinen portaalissa-sivulla **Määritä kertakirjautumisen osoitteessa SumoLogic** valintaikkunan Valitse Sign määritysten vahvistus ja valitse sitten **Valmis**.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-sumologic-tutorial/IC778560.png "Määritä kertakirjautuminen")

##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Jotta voit ottaa käyttöön Azure AD-käyttäjät voivat kirjautua sisään SumoLogic, ne on valmisteltava SumoLogic avulla.  
SumoLogic valmistelu on manuaalinen tehtävä.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Valmistelu käyttäjätilit, toimi seuraavasti:

1.  Kirjaudu sisään **SumoLogic** alihallintaan.

2.  Siirry **hallinta \> käyttäjien**.

    ![Käyttäjät] (./media/active-directory-saas-sumologic-tutorial/IC778561.png "Käyttäjät")

3.  Valitse **Lisää**.

    ![Käyttäjät] (./media/active-directory-saas-sumologic-tutorial/IC778562.png "Käyttäjät")

4.  Valitse **Uusi käyttäjä** -valintaikkunassa seuraavasti:

    ![Uusi käyttäjä] (./media/active-directory-saas-sumologic-tutorial/IC778563.png "Uusi käyttäjä")

    1.  Kirjoita haluat säätää kyselyjä **Etunimi**, **Sukunimi** ja **sähköpostin** tekstiruutujen Azure AD-tilin Aiheeseen liittyvät tiedot.
    2.  Valitse rooli.
    3.  Valitse **aktiivinen** **tila**.
    4.  Valitse **Tallenna**.

>[AZURE.NOTE] Voit käyttää mitä tahansa muita SumoLogic käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä SumoLogic säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-sumologic-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä SumoLogic, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **SumoLogic** sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-sumologic-tutorial/IC778564.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-sumologic-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).
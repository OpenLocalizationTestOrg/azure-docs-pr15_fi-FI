<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi InsideView | Microsoft Azure" 
    description="Opettele käyttämään InsideView Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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

#<a name="tutorial-azure-active-directory-integration-with-insideview"></a>Opetusohjelma: InsideView Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja InsideView integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   InsideView palvelutili
  
Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt InsideView Azure AD-käyttäjien saa oikeuden kertakirjauksen InsideView yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  InsideView sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-insideview-tutorial/IC794128.png "Skenaario")
##<a name="enabling-the-application-integration-for-insideview"></a>InsideView sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön InsideView sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-insideview-perform-the-following-steps"></a>InsideView sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-insideview-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-insideview-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-insideview-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-insideview-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **InsideView**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-insideview-tutorial/IC794129.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **InsideView**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![InsideView] (./media/active-directory-saas-insideview-tutorial/IC794130.png "InsideView")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen InsideView käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.  
Tämän toimintosarjan osana on base-64-koodattu sertifikaattitiedosto luomiseen.  
Jos et ole tottunut näin, katso, [miten voit muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **InsideView** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Määrittää yksittäisen rekisteröityminen] (./media/active-directory-saas-insideview-tutorial/IC794131.png "Määrittää yksittäisen rekisteröityminen")

2.  **Miten käyttäjät voivat kirjautua InsideView haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Määrittää yksittäisen rekisteröityminen] (./media/active-directory-saas-insideview-tutorial/IC794132.png "Määrittää yksittäisen rekisteröityminen")

3.  Kirjoita **Määritä sovelluksen URL-osoite** -sivulla **InsideView vastaa URL** -tekstiruutuun InsideView SSO URL-osoite (esimerkiksi: `https://my.insideview.com/iv/<STS Name>/login.iv`), ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-insideview-tutorial/IC794133.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  Valitse **Määritä kertakirjautumisen osoitteessa InsideView** -sivulla Lataa sertifikaatti, valitsemalla **Lataa**, ja Tallenna sertifikaattitiedosto tietokoneeseen.

    ![Määrittää yksittäisen rekisteröityminen] (./media/active-directory-saas-insideview-tutorial/IC794134.png "Määrittää yksittäisen rekisteröityminen")

5.  Eri selainikkunassa Kirjaudu InsideView yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

6.  Yläkulmassa työkalurivin **järjestelmänvalvoja**, **SingleSignOn asetukset**, ja valitse sitten **Lisää SAML**.

    ![SAML kertakirjautuminen asetukset] (./media/active-directory-saas-insideview-tutorial/IC794135.png "SAML kertakirjautuminen asetukset")

7.  **Lisää uusi SAML** -osassa seuraavasti:

    ![Lisää uusi SAML] (./media/active-directory-saas-insideview-tutorial/IC794136.png "Lisää uusi SAML")

    1.  Kirjoita nimi kokoonpanon **STS nimi** -tekstiruutuun.
    2.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa InsideView** -valintasivun kopioi **Service-palvelun (SP) aloittanut päätepisteen** arvo ja liitä se **SamlP/WS-Fed Unsolicated päätepisteen** tekstiruutu.
    3.  Luo **base-64-koodattu** tiedosto ladattu varmennetta.
        
        >[AZURE.TIP]Lisätietoja on artikkelissa [muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o)

    4.  Avaa base-64-koodattu varmennetta Muistiossa, kopioi se sisällön Leikepöydän ja liitä se **STS-varmenne** -tekstiruutu
    5.  Kirjoita **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress** **Crm käyttäjän tunnus yhdistäminen** -tekstiruutuun.
    6.  Kirjoita **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress** **Crm sähköpostin yhdistäminen** -tekstiruutuun.
    7.  Kirjoita **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname** **Crm Etunimi yhdistäminen** -tekstiruutuun.
    8.  Kirjoita **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname** **Crm Sukunimi yhdistäminen** -tekstiruutuun.
    9.  Valitse **Tallenna**.

8.  Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Määrittää yksittäisen rekisteröityminen] (./media/active-directory-saas-insideview-tutorial/IC794137.png "Määrittää yksittäisen rekisteröityminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Jos haluat ottaa käyttöön Azure AD-käyttäjät voivat kirjautua sisään InsideView, ne on valmisteltava InsideView kyselyjä.  
InsideView valmistelu on manuaalinen tehtävä.
  
Pääset käyttäjät tai InsideView luotujen yhteyshenkilöiden Ota customer success esimiehesi tai Lähetä sähköpostiviesti**support@insideview.com**

>[AZURE.NOTE] Voit käyttää mitä tahansa muita InsideView käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä InsideView valmistelu Azure AD-käyttäjätilejä.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-insideview-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä InsideView, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **InsideView **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-insideview-tutorial/IC794138.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-insideview-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).
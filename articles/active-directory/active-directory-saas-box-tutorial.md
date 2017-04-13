<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi ruutuun | Microsoft Azure" 
    description="Lue, miten voit ottaa käyttöön kertakirjautumisen, automaattinen valmistelu ja muut Azure Active Directory-ruudun avulla!" 
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




#<a name="tutorial-azure-active-directory-integration-with-box"></a>Opetusohjelma: Ruutuun Azure Active Directory-integrointi


  
Tässä opetusohjelmassa tavoitteena on näyttämään integrointi ja Azure-ruutuun.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Testaa omistaja-ruutuun
  
Kun olet suorittanut Tässä opetusohjelmassa, olet määrittänyt ruutuun Azure AD-käyttäjien saa oikeuden kertakirjauksen ruutuun yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Sovelluksen integrointi ruudun ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Käyttäjän ja ryhmän valmistelu määrittäminen
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-box-tutorial/IC769537.png "Skenaario")



##<a name="enabling-the-application-integration-for-box"></a>Sovelluksen integrointi ruudun ottaminen käyttöön
  
Tässä osassa tavoitteena on Jäsennys, miten voit ottaa käyttöön sovelluksen integrointi-ruutu.

###<a name="to-enable-the-application-integration-for-box-perform-the-following-steps"></a>Ottaa käyttöön sovelluksen integroinnin valinta, toimi seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-box-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-box-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-box-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-box-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun**- **ruutuun**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-box-tutorial/IC701023.png "Sovelluksen valikoima")

7.  Tulosruudussa- **ruudusta**ja valitse sitten **Valmis** , voit lisätä sovelluksen.

    ![Valinta] (./media/active-directory-saas-box-tutorial/IC701024.png "Valinta")



##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen-ruutuun niiden tilillä, käyttämällä SAML-protokollan perusteella federation Azure AD. Tämän toimintosarjan osana tarvitaan metatietojen lataaminen Box.com.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portal- **ruutuun** sovellus integrointi-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-box-tutorial/IC769538.png "Määritä kertakirjautuminen")

2.  **Miten käyttäjät voivat kirjautua ruutuun haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-box-tutorial/IC769539.png "Määritä kertakirjautuminen")

3.  Kirjoita **Määritä sovelluksen URL-osoite** -sivulla **Ruutuun vuokraajan URL** -tekstiruutuun ruutuun vuokraajan URL-osoite (esimerkiksi: https://<mydomainname>. box.com), ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-box-tutorial/IC669826.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  Valitse **Määritä single Sign-ruudussa** -sivulla Lataa metatietojen **Lataa metatiedot**ja sitten datatiedoston paikallisesti tietokoneeseen.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-box-tutorial/IC669824.png "Määritä kertakirjautuminen")

5.  Siirrä metatiedot-ruutuun tiedoston tekniseen tukeen. Tuen työryhmän tarpeiden määrittää kertakirjautumisen puolestasi.

6.  Valitse Sign määritysten vahvistus ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-box-tutorial/IC769540.png "Määritä kertakirjautuminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön valmistelu käyttäjätileille Active Directory-ruutuun.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1. Valitse Azure perinteinen portal- **ruutuun** sovellus integrointi-sivulla **Määritä käyttäjän valmistelu** -valintaikkunan avaaminen **Määritä käyttäjän valmistelu** . 

    ![Ota käyttöön automaattinen käyttäjän valmistelu] (./media/active-directory-saas-box-tutorial/IC769541.png "Ota käyttöön automaattinen käyttäjän valmistelu")

2. Valitse valintaikkunan **valmistelu-ruutuun käyttäjälle** -sivulla **käyttäjän valmistelu käyttöön**. 

    ![Ota käyttöön automaattinen käyttäjän valmistelu] (./media/active-directory-saas-box-tutorial/IC769544.png "Ota käyttöön automaattinen käyttäjän valmistelu")

3. **Kirjautuminen käyttöoikeus-ruutuun** sivulla Anna tarvittavat tunnistetiedot ja valitse sitten **Hyväksy**. 

    ![Ota käyttöön automaattinen käyttäjän valmistelu] (./media/active-directory-saas-box-tutorial/IC769546.png "Ota käyttöön automaattinen käyttäjän valmistelu")


4. Valitse Määritä tämä toiminto ja palaa perinteiseen Azure-portaaliin **ruutuun käyttöoikeuksien myöntäminen** . 

    ![Ota käyttöön automaattinen käyttäjän valmistelu] (./media/active-directory-saas-box-tutorial/IC769549.png "Ota käyttöön automaattinen käyttäjän valmistelu")


5. **Objektityypit valmistelu** valintaruudut avulla voit valita riippumatta siitä, onko Ryhmitä objektit on valmisteltu ruutuun käyttäjän objektit **Valmistelu asetukset** -sivulla.  Lisätietoja on alla "Määrittäminen käyttäjät ja ryhmät-osio".


6. Lopeta määritystä, napsauta Valmis-painiketta. 

    ![Ota käyttöön automaattinen käyttäjän valmistelu] (./media/active-directory-saas-box-tutorial/IC769551.png "Ota käyttöön automaattinen käyttäjän valmistelu")



##<a name="assigning-a-test-user"></a>Testikäyttäjän määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-box-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä ruutuun, toimi seuraavasti:

1. Testaa tilin luominen Azure perinteinen portaalissa.

2. Napsauta **ruudun **sovelluksen integrointi-sivulla **määritettävä käyttäjille**. 

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-box-tutorial/IC769552.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi. 

    ![Kyllä] (./media/active-directory-saas-box-tutorial/IC767830.png "Kyllä")
  
Pitäisi nyt Odota 10 minuuttia ja varmista, että tili on synkronoitu-ruutuun.

Vahvistus ensimmäisessä vaiheessa voit tarkistaa valmistelun tilan napsauttamalla Raporttinäkymät-ikkunan d Azure perinteinen portaalin ruutuun sovelluksen integrointi-sivulla.

![Raporttinäkymät-ikkunan] (./media/active-directory-saas-box-tutorial/IC769553.png "Raporttinäkymät-ikkunan")

Jakson valmistelu onnistuneesti suoritettujen käyttäjän ilmaistaan liittyvät tila:

![Integrointitila] (./media/active-directory-saas-box-tutorial/IC769555.png "Integrointitila")


Ruutu-vuokraajan synkronoidut käyttäjät on lueteltu kohdassa **Hallintakonsoli** **Hallitun käyttäjät** .

![Integrointitila] (./media/active-directory-saas-box-tutorial/IC769556.png "Integrointitila")


##<a name="assigning-users-and-groups"></a>Käyttäjien ja ryhmien määrittäminen

**Ruutuun > käyttäjät ja ryhmät** Azure perinteinen portal-välilehdessä voit määrittää käyttäjät ja ryhmät on myönnettävä käyttöoikeus-ruutuun. Käyttäjän tai ryhmän varauksen aiheuttaa ilmetä seuraavilla tavoilla:

* Azure AD sallii määritetty käyttäjä (joko suoraan tehtävän tai Ryhmäjäsenyyden)-ruutuun tarkistamiseen. Jos käyttäjä ei ole määritetty, Azure AD ei salli ne ruutuun kirjautumaan ja palauttavat virheen Azure AD-kirjautumissivulla.

* Käyttäjän [sovellusten käynnistys](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users)lisätään ruudun sovellus-ruutua.

* Jos Automaattinen valmisteleminen on käytössä, valitse/määritetyt käyttäjät tai ryhmät lisätään automaattisesti valmistellaan valmistelu jonossa.

    * Vain käyttäjän objektit on määritetty valmisteltava, valitse kaikki suoraan määrittämän käyttäjät ovat käytettävissä valmistelu jonossa ja kaikki käyttäjät, jotka ovat minkä tahansa määritetyn ryhmissä sijoitetaan valmistelu jonossa. 
    
    * Jos valmisteltava Ryhmitä objektit on määritetty, kaikki määritetyn ryhmän objektit valmisteltu ruutuun sekä kaikki käyttäjät, jotka kuuluvat ryhmille. Ryhmä- tai jäsenyydet säilytetään yhteydessä ruutuun kirjoitetaan.
    
Voit käyttää **määritteet > kertakirjautumisen** -välilehti, Määritä, mitä käyttäjä määritteet (tai saatavat) esitetään ruutuun SAML-pohjaisen todennuksen aikana ja **määritteet > Provisioning** määrittää, miten käyttäjien ja ryhmien määritteitä flow Azure AD-ruutuun aikana valmistelu Toiminnot-välilehti. Katso lisätietoja resurssit.


## <a name="additional-resources"></a>Lisäresursseja

* [SAML-tunnuksen myöntää saatavat mukauttaminen](active-directory-saml-claims-customization.md)
* [Valmistelu: Määritteiden yhdistämismääritykset mukauttaminen](active-directory-saas-customizing-attribute-mappings.md)
* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)

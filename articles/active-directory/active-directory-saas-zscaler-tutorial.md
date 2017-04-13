<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Zscaler | Microsoft Azure" 
    description="Opettele käyttämään Zscaler Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!." 
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
    ms.date="08/16/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-zscaler"></a>Opetusohjelma: Zscaler Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja Zscaler integrointi. Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Valitse Zscaler palvelutili
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Zscaler sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Välityspalvelimen asetusten määrittäminen
4.  Määrittäminen käyttäjän valmistelu
5.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-zscaler-tutorial/IC769226.png "Skenaario")

##<a name="enabling-the-application-integration-for-zscaler"></a>Zscaler sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön Zscaler sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-zscaler-perform-the-following-steps"></a>Zscaler sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-zscaler-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-zscaler-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-zscaler-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-zscaler-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **Zscaler**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-zscaler-tutorial/IC769227.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **Zscaler**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Zscaler] (./media/active-directory-saas-zscaler-tutorial/IC769228.png "Zscaler")

##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen Zscaler käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.  
Tämän toimintosarjan osana tarvitaan varmenteen lataaminen Zscaler.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **Zscaler** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Ottaa käyttöön kertakirjautumisen] (./media/active-directory-saas-zscaler-tutorial/IC769229.png "Ottaa käyttöön kertakirjautumisen")

2.  **Miten käyttäjät voivat kirjautua Zscaler haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Määritä yhden sisäänkirjautuminen] (./media/active-directory-saas-zscaler-tutorial/IC769230.png "Määritä yhden sisäänkirjautuminen")

3.  **Määrittää sovelluksen URL-osoite** -sivulla **Zscaler merkki-URL** -tekstiruutuun Kirjoita rekisteröityminen Zscaler saatua URL-osoite ja valitse sitten **Seuraava**: 

    >[AZURE.NOTE] Jos et tiedä, mikä URL-osoitteen rekisteröityminen on, ota yhteyttä Zscaler-tukeen.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-zscaler-tutorial/IC769231.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  Valitse **Määritä kertakirjautumisen osoitteessa Zscaler** -sivulla toimimalla seuraavasti:

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-zscaler-tutorial/IC769232.png "Määritä kertakirjautuminen")

    1.  **Lataa**ja tallenna sitten sertifikaattitiedosto paikallisesti kuin **c:\\Zscaler.cer**.
    2.  Kopioi Leikepöydän **todennus pyyntö URL-osoite** .

5.  Kirjaudu Zscaler asiakasympäristöön.

6.  Valitse ylä-valikossa **hallinta**.

    ![Hallinta] (./media/active-directory-saas-zscaler-tutorial/IC769486.png "Hallinta")

7.  Valitse **Hallitse järjestelmänvalvojia ja roolit**-kohdassa **tyylien hallinta-käyttäjien ja käyttöoikeuksien**.

    ![Järjestelmänvalvojat ja roolien hallinta] (./media/active-directory-saas-zscaler-tutorial/IC769487.png "Järjestelmänvalvojat ja roolien hallinta")

8.  **Valitse todennusta-asetus organisaation** -osassa seuraavasti:

    ![Valitse todennusasetukset] (./media/active-directory-saas-zscaler-tutorial/IC769488.png "Valitse todennusasetukset")

    1.  Valitse **Tarkista käyttämällä SAML Single Sign-on**.
    2.  Valitse **Määritä SAML yksittäisen Sign parametrit**.

9.  Valitse **Määritä SAML yksittäisen Sign-On parametrit** -sivulla suorittamalla seuraavat vaiheet ja valitse sitten **Valmis**:

    ![Lataa sertifikaatti] (./media/active-directory-saas-zscaler-tutorial/IC769489.png "Lataa sertifikaatti")

    1.  Liitä **URL-osoite, johon käyttäjät lähettävät todennustavaksi SAML Portal** -tekstiruutuun perinteinen Azure-portaalista **todennus pyyntö URL-osoite** -kenttään.
    2.  Kirjoita **NameID** **määritteen sisältävä käyttäjätunnus** -tekstiruutuun.
    3.  **Lataa SSL julkisen sertifikaatti** -kenttään Lataa sertifikaatti olet ladannut perinteinen Azure-portaalista.
    4.  Valitse **Ota käyttöön automaattinen-valmistelu SAML**.

10. Tee **Käyttöoikeuksien määrittäminen** -sivulla seuraavat toimet:

    ![Käyttöoikeuksien määrittäminen] (./media/active-directory-saas-zscaler-tutorial/IC769490.png "Käyttöoikeuksien määrittäminen")

    1.  Valitse **Tallenna**.
    2.  Valitse **Aktivoi nyt**.

11. Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-zscaler-tutorial/IC769491.png "Määritä kertakirjautuminen")

##<a name="configuring-proxy-settings"></a>Välityspalvelimen asetusten määrittäminen

###<a name="to-configure-the-proxy-settings-in-internet-explorer"></a>Välityspalvelimen asetusten määrittäminen Internet Explorerissa

1.  Käynnistä **Internet Explorer**.

2.  Valitse **Työkalut** -valikosta **Internet-asetukset** -valintaikkunan avaaminen **Internet-asetukset** .

    ![Internet-asetukset] (./media/active-directory-saas-zscaler-tutorial/IC769492.png "Internet-asetukset")

3.  Valitse **yhteydet** -välilehti.

    ![Yhteydet] (./media/active-directory-saas-zscaler-tutorial/IC769493.png "Yhteydet")

4.  Valitse **Lähiverkon asetukset** -valintaikkunan avaaminen **Lähiverkon asetukset** .

5.  Välityspalvelimen server-osassa seuraavasti:

    ![Välityspalvelin] (./media/active-directory-saas-zscaler-tutorial/IC769494.png "Välityspalvelin")

    1.  Valitse Käytä välityspalvelinta lähiverkossa.
    2.  Kirjoita osoite-tekstiruutuun **gateway.zscalertwo.net**.
    3.  Kirjoita **80**Port (portti)-tekstiruutuun.
    4.  Valitse **Älä käytä välityspalvelinta paikallisissa osoitteissa**.
    5.  Valitse **OK** ja sulje **lähiverkossa (LAN) asetukset** -valintaikkuna.

6.  Valitse **OK** ja sulje **Internet-asetukset** -valintaikkunassa.

##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Jos haluat ottaa käyttöön Azure AD-käyttäjät voivat kirjautua sisään Zscaler, ne on valmisteltava Zscaler kyselyjä.  
Zscaler valmistelu on manuaalinen tehtävä.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Voit määrittää käyttäjän valmistelu toimimalla seuraavasti:

1.  Kirjaudu sisään **Zscaler** alihallintaan.

2.  Valitse **hallinta**.

    ![Hallinta] (./media/active-directory-saas-zscaler-tutorial/IC781035.png "Hallinta")

3.  Valitse **käyttäjien hallinta**.

    ![Käyttäjien hallinta] (./media/active-directory-saas-zscaler-tutorial/IC781036.png "Käyttäjien hallinta")

4.  Valitse **käyttäjät** -välilehdessä **Lisää**.

    ![Lisää] (./media/active-directory-saas-zscaler-tutorial/IC781037.png "Lisää")

5.  Valitse Lisää käyttäjä-osassa seuraavasti:

    ![Käyttäjän lisääminen] (./media/active-directory-saas-zscaler-tutorial/IC781038.png "Käyttäjän lisääminen")

    1.  Kirjoita **käyttäjätunnus**, **Näytä käyttäjänimi**, **salasana**, **Vahvista salasana**ja valitse sitten **ryhmät** ja **osaston** kelvollinen AAD tilin haluat säätää.
    2.  Valitse **Tallenna**.

>[AZURE.NOTE] Voit käyttää mitä tahansa muita Zscaler käyttäjän tilin luontityökalu tai ohjelmointirajapinnan myöntämä Zscaler säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-zscaler-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä Zscaler, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **Zscaler** sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-zscaler-tutorial/IC769495.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-zscaler-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).

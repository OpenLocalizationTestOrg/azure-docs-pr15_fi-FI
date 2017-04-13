<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Zscaler kahden | Microsoft Azure" 
    description="Opettele käyttämään Zscaler kaksi Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!." 
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

#<a name="tutorial-azure-active-directory-integration-with-zscaler-two"></a>Opetusohjelma: Zscaler kahden Azure Active Directory-integrointi

Tässä opetusohjelmassa tavoitteena on näyttämään Azure- ja ZScaler kaksi integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Käytössä ZScaler kaksi kertakirjautumisen tilaus
  
Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt ZScaler kaksi Azure AD-käyttäjien saa oikeuden kertakirjauksen sovellukseen ZScaler kahden yrityksen sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  -Sovelluksen integrointi ZScaler kaksi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Välityspalvelimen asetusten määrittäminen
4.  Määrittäminen käyttäjän valmistelu
5.  Käyttäjien määrittäminen

![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-zscaler-two-tutorial/IC800199.png "Kertakirjautumisen määrittäminen")

##<a name="enabling-the-application-integration-for-zscaler-two"></a>-Sovelluksen integrointi ZScaler kaksi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön sovelluksen integrointi ZScaler kaksi.

###<a name="to-enable-the-application-integration-for-zscaler-two-perform-the-following-steps"></a>-Sovelluksen integrointi ZScaler kaksi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-zscaler-two-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-zscaler-two-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-zscaler-two-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-zscaler-two-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **ZScaler kaksi**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-zscaler-two-tutorial/IC800200.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **ZScaler kaksi**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Kaksi ZScaler] (./media/active-directory-saas-zscaler-two-tutorial/IC800201.png "Kaksi ZScaler")

##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen ZScaler kaksi niiden tilillä Azure AD federation perusteella SAML-protokollan avulla.  
Tämän toimintosarjan osana on base-64-koodattu varmenteen lataaminen ZScaler kaksi alihallintaan.  
Jos et ole tottunut näin, katso, [miten voit muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa **ZScaler kaksi** integrointi sivulla **määrittäminen kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-zscaler-two-tutorial/IC800202.png "Kertakirjautumisen määrittäminen")

2.  **Miten Haluatko, että käyttäjät voivat kirjautua ZScaler kaksi** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-zscaler-two-tutorial/IC800203.png "Kertakirjautumisen määrittäminen")

3.  **Määritä sovelluksen URL-osoite** -sivulla **ZScaler kaksi merkki-URL** -tekstiruutuun Sign ZScaler kahden sovelluksen käyttäjille käyttämät URL-osoite ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-zscaler-two-tutorial/IC800204.png "Sovelluksen URL-Osoitteen määrittäminen")

    >[AZURE.NOTE] Saat varsinainen arvo-ympäristössä ZScaler kaksi tukihenkilöstölle Jos tarvitse sitä.

4.  **Määritä kertakirjautumisen osoitteessa ZScaler kaksi** -sivulla Lataa sertifikaatti, valitsemalla **Lataa**, ja Tallenna sertifikaattitiedosto tietokoneeseen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-zscaler-two-tutorial/IC800205.png "Kertakirjautumisen määrittäminen")

5.  Eri selainikkunassa Kirjaudu ZScaler kahden yrityksen sivuston järjestelmänvalvojan oikeuksilla.

6.  Valitse ylä-valikossa **hallinta**.

    ![Hallinta] (./media/active-directory-saas-zscaler-two-tutorial/IC800206.png "Hallinta")

7.  Valitse **Hallitse järjestelmänvalvojia ja roolit**-kohdassa **käyttäjien hallinta ja todennusta**.

    ![Käyttäjien ja käyttöoikeuksien hallinta] (./media/active-directory-saas-zscaler-two-tutorial/IC800207.png "Käyttäjien ja käyttöoikeuksien hallinta")

8.  **Valitse todennusasetukset organisaation** -osassa seuraavasti:

    ![Todennus] (./media/active-directory-saas-zscaler-two-tutorial/IC800208.png "Todennus")

    1.  Valitse **Tarkista käyttämällä SAML kertakirjautumisen**.
    2.  Valitse **Määritä SAML yksittäisen Sign parametrit**.

9.  Valitse **Määritä SAML yksittäisen Sign-On parametrit** -sivulla suorittamalla seuraavat vaiheet ja valitse sitten **Valmis**:

    ![Kertakirjautuminen] (./media/active-directory-saas-zscaler-two-tutorial/IC800209.png "Kertakirjautuminen")

    1.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa ZScaler kaksi** -valintasivun **Käyttöoikeuksien pyytäminen URL-osoite** -arvo kopioi ja liitä se sitten tekstiruutu **, johon käyttäjät lähettävät todennustavaksi SAML-portaalin URL-Osoitteen** .
    2.  Kirjoita **NameID** **määritteen sisältävä käyttäjätunnus** -tekstiruutuun.
    3.  Voit ladata ladatut varmenteen, valitsemalla **Zscaler pem**.
    4.  Valitse **Ota käyttöön automaattinen-valmistelu SAML**.

10. Tee **Käyttöoikeuksien määrittäminen** -sivulla seuraavat toimet:

    ![Hallinta] (./media/active-directory-saas-zscaler-two-tutorial/IC800210.png "Hallinta")

    1.  Valitse **Tallenna**.
    2.  Valitse **Aktivoi nyt**.

11. Azure perinteinen portaalissa-sivulla **Määritä kertakirjautumisen osoitteessa ZScaler kaksi** valintaikkuna Valitse Sign määritysten vahvistus ja valitse sitten **Valmis**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-zscaler-two-tutorial/IC800211.png "Kertakirjautumisen määrittäminen")

##<a name="configuring-proxy-settings"></a>Välityspalvelimen asetusten määrittäminen

###<a name="to-configure-the-proxy-settings-in-internet-explorer"></a>Välityspalvelimen asetusten määrittäminen Internet Explorerissa

1.  Käynnistä **Internet Explorer**.

2.  Valitse **Työkalut** -valikosta **Internet-asetukset** -valintaikkunan avaaminen **Internet-asetukset** .

    ![Internet-asetukset] (./media/active-directory-saas-zscaler-two-tutorial/IC769492.png "Internet-asetukset")

3.  Valitse **yhteydet** -välilehti.

    ![Yhteydet] (./media/active-directory-saas-zscaler-two-tutorial/IC769493.png "Yhteydet")

4.  Valitse **Lähiverkon asetukset** -valintaikkunan avaaminen **Lähiverkon asetukset** .

5.  Välityspalvelimen server-osassa seuraavasti:

    ![Välityspalvelin] (./media/active-directory-saas-zscaler-two-tutorial/IC769494.png "Välityspalvelin")

    1.  Valitse Käytä välityspalvelinta lähiverkossa.
    2.  Kirjoita osoite-tekstiruutuun **gateway.zscalerone.net**.
    3.  Kirjoita **80**Port (portti)-tekstiruutuun.
    4.  Valitse **Älä käytä välityspalvelinta paikallisissa osoitteissa**.
    5.  Valitse **OK** ja sulje **lähiverkossa (LAN) asetukset** -valintaikkuna.

6.  Valitse **OK** ja sulje **Internet-asetukset** -valintaikkunassa.

##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Jotta voit kirjautua ZScaler kaksi Azure AD-käyttäjien käyttöön, ne on valmisteltava, ZScaler kaksi.  
Kyseessä ZScaler kaksi valmistelu on manuaalinen tehtävä.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Voit määrittää käyttäjän valmistelu toimimalla seuraavasti:

1.  Kirjaudu sisään **Zscaler** alihallintaan.

2.  Valitse **hallinta**.

    ![Hallinta] (./media/active-directory-saas-zscaler-two-tutorial/IC781035.png "Hallinta")

3.  Valitse **käyttäjien hallinta**.

    ![Lisää] (./media/active-directory-saas-zscaler-two-tutorial/IC781037.png "Lisää")

4.  Valitse **käyttäjät** -välilehdessä **Lisää**.

    ![Lisää] (./media/active-directory-saas-zscaler-two-tutorial/IC781037.png "Lisää")

5.  Valitse Lisää käyttäjä-osassa seuraavasti:

    ![Käyttäjän lisääminen] (./media/active-directory-saas-zscaler-two-tutorial/IC781038.png "Käyttäjän lisääminen")

    1.  Kirjoita **käyttäjätunnus**, **Näytä käyttäjänimi**, **salasana**, **Vahvista salasana**ja valitse sitten **ryhmät** ja **osaston** kelvollinen AAD tilin haluat säätää.
    2.  Valitse **Tallenna**.

>[AZURE.NOTE] Voit käyttää mitä tahansa muita ZScaler kaksi käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä ZScaler kaksi säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-zscaler-two-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä ZScaler kaksi, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse **ZScaler kaksi** integrointi sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-zscaler-two-tutorial/IC800212.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-zscaler-two-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).
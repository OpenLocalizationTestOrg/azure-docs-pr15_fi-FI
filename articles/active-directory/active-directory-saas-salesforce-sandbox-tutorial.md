<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Salesforce eristetyn | Microsoft Azure"
    description="Opettele käyttämään Salesforce eristetyn Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!." 
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
    ms.date="10/28/2016" 
    ms.author="jeedes" />


#<a name="tutorial-azure-active-directory-integration-with-salesforce-sandbox"></a>Opetusohjelma: Salesforce eristetyn Azure Active Directory-integrointi
>[AZURE.TIP]Voit antaa palautetta napsauttamalla [tätä](http://go.microsoft.com/fwlink/?LinkId=521878).
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja Salesforce eristetyn integrointi.  
Hiekoittimien antaa luominen eri tarkoituksiin, kuten kehitystä, testaus ja koulutusta käyttämättä missään vaiheessa tietoja ja sovelluksia Salesforce tuotannon organisaation erillisestä organisaation useita kopioita.  
Lisätietoja on artikkelissa [Eristetyn yleiskatsaus](https://help.salesforce.com/HTViewHelpDoc?id=create_test_instance.htm&language=en_US)
  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Eristetyn Salesforce.comissa
  
Jos sinulla ei ole kelvollinen eristetyn Salesforce.comissa, mutta sinun on Salesforce.
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Salesforce Eristetyn sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Toimialueen ottaminen käyttöön
4.  Määrittäminen käyttäjän valmistelu
5.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC769571.png "Skenaario")
##<a name="enabling-the-application-integration-for-salesforce-sandbox"></a>Salesforce Eristetyn sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön Salesforce Eristetyn sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-salesforce-sandbox-perform-the-following-steps"></a>Salesforce Eristetyn sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC700994.png "Sovellukset")

4.  Avaa **Sovellus valikoima**, **Lisää App**ja valitse sitten **Lisää sovellus tehtävään organisaatiossani, jos haluat käyttää**.

    ![Mitä haluat tehdä?] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC700995.png "Mitä haluat tehdä?")

5.  Kirjoita **hakuruutuun** **Salesforce eristetyn**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC710978.png "Sovelluksen valikoima")

6.  Valitse tulosruudussa **Salesforce eristetyn**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Salesforce eristetyn] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC746474.png "Salesforce eristetyn")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen Salesforce käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **Salesforce eristetyn** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC749323.png "Määritä kertakirjautuminen")

2.  **Miten käyttäjät voivat kirjautua Salesforce eristetyn haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Salesforce eristetyn] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC746479.png "Salesforce eristetyn")

3.  Kirjoita **Määritä sovelluksen URL-osoite** -sivulla **Merkki-URL** -tekstiruutuun URL-osoite käyttämällä seuraavaa kaavaa `http://company.my.salesforce.com`, ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781022.png "Sovelluksen URL-Osoitteen määrittäminen")

4. Jos olet jo määrittänyt kertakirjautumisen toiseen Salesforce eristetyn esiintymän hakemistossa, valitse sinun on määritettävä **tunnus** , joka on saman arvon kuin **Kirjaudu URL-osoite**. **Tunnus** -kenttä löytyy valitsemalla valintaikkunassa **Määrittäminen sovelluksen URL-osoite** -sivulla **Näytä lisäasetukset** -valintaruutu.

4.  Valitse **Määritä kertakirjautumisen osoitteessa Salesforce eristetyn** -sivulla valitsemalla **Lataa**ja Tallenna sertifikaattitiedosto tietokoneeseen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781023.png "Kertakirjautumisen määrittäminen")

5.  Eri selainikkunassa Kirjaudu Salesforce-eristetyn järjestelmänvalvojana.

6.  Valitse ylä-valikossa **asetukset**.

    ![Asetukset] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781024.png "Asetukset")

7.  Vasemman reunan siirtymisruudussa **Turvaominaisuudet**ja valitse sitten **Sign-asetukset**.

    ![Kertakirjauksen asetukset] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781025.png "Kertakirjauksen asetukset")

8.  Suorita Sign-On asetukset-osassa seuraavasti:

    ![Kertakirjauksen asetukset] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781026.png "Kertakirjauksen asetukset")

    a.  Valitse **SAML käytössä**.
    
    b.  Valitse **Uusi**.

9.  Suorita SAML yksittäisen Sign-On asetukset-osassa seuraavasti:

    ![SAML yksittäisen kertakirjauksen asetukset] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781027.png "SAML yksittäisen kertakirjauksen asetukset")

    a.  Kirjoita nimi-tekstiruutuun määrityksen nimi (esimerkiksi: *SPSSOWAAD\_testi*).
    
    b.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Salesforce eristetyn** valintaikkunan-sivulla kopioi **Myöntäjä URL-osoite** -arvo ja liitä se **myöntäjä** -tekstiruutuun.
    
    c-näppäinyhdistelmää.  Kirjoita **Kohteen tunnus** -tekstiruutuun **https://test.salesforce.com** , jos kyseessä on ensimmäisen Salesforce eristetyn esiintymään, jota olet lisäämässä hakemistossa. Jos olet jo lisännyt erillisen Salesforce eristetyn ja sitten **Kohteen tunnus** -tyypin **Merkki-URL-osoite**, joiden pitäisi olla tässä muodossa:`http://company.my.salesforce.com`
    
    d.  Valitse Lataa ladatut sertifikaatti **Selaa** .
    
    e.  **SAML tunnuksen tyyppiä**Valitse **vahvistus sisältää käyttäjän objektista Federation-tunnuksen**.
    
    f.  **SAML tunnistetietojen sijaintia**Valitse **tunnistetiedot on aihe-lauseen NameIdentifier-osaan**.
    
    g.  Azure perinteinen portaalissa-sivulla **Määritä kertakirjautumisen osoitteessa Salesforce eristetyn** valintaikkunan **Remote sisäänkirjautumisen URL-osoite** -arvo kopioi ja liitä se sitten **Tunnistetietojen toimittaja sisäänkirjautumisen URL-osoite** -tekstiruutu.
    
    h.  SFDC ei tue SAML Kirjaudu ulos.  Voit kiertää tämän ongelman Liitä 'https://login.windows.net/common/wsfederation?wa=wsignout1.0' sen **Jäsenyyden tarjoajan Kirjaudu URL-Osoitteen** tekstiruutu.
    
    voin.  **Palveluntarjoajan aloittanut pyytää sidonta**valitsemalla **HTTP-viesti**.
    
    j. Valitse **Tallenna**.

10. Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781028.png "Kertakirjautumisen määrittäminen")

##<a name="enabling-your-domain"></a>Toimialueen ottaminen käyttöön
  
Tässä osassa oletetaan, että olet jo luonut toimialueeseen.  
Lisätietoja on artikkelissa [Määrittäminen oma toimialuenimi](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US).

###<a name="to-enable-your-domain-perform-the-following-steps"></a>Jotta toimialueen, toimi seuraavasti:

1.  Valitse vasemmassa siirtymisruudussa **Toimialuehallinta**ja valitse sitten **oman toimialueen.**

    ![Toimialueen omistajuutta] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781029.png "Toimialueen omistajuutta")

    >[AZURE.NOTE]Varmista, että toimialueesi on määritetty oikein.

2.  **Kirjautuminen sivun asetukset** -kohdasta **Muokkaa**, valitse **Todentamispalvelu**SAML yksittäisen Sign-asetuksen nimi linkitys edellisestä osasta, ja valitse lopuksi **Tallenna**.

    ![Toimialueen omistajuutta] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781030.png "Toimialueen omistajuutta")
  
Heti, kun sinulla on toimialue, joka on määritetty, käyttäjien on käytettävä Salesforce-eristetyn kirjautuessasi toimialueen URL-osoite.  
Saat URL-osoitteen arvo valitsemalla edellisessä osassa luomasi SSO-profiiliin.
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjän valmistelu Active Directory-käyttäjätilien Salesforce eristetyn.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Voit määrittää käyttäjän valmistelu toimimalla seuraavasti:

1.  Salesforce-portaalissa siirtymispalkissa Valitse nimesi Laajenna Käyttäjävalikko:

    ![Omat asetukset] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC698773.png "Omat asetukset")

2.  Valitse käyttäjä-valikosta Avaa **Omat asetukset** -sivulla **Oman asetukset** .

3.  Vasemmassa ruudussa Valitse **Henkilökohtainen** laajenna Omat osa ja valitse sitten **Palauta omat suojauksen salasana**:

    ![Omat asetukset] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC698774.png "Omat asetukset")

4.  Valitse **Palauta omat suojauksen salasana** -sivulla **Palauta suojauksen salasana** on pyydettävä sähköpostiviestin, jossa Salesforce.com suojaustunnus.

    ![Uuden tunnuksen] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC698776.png "Uuden tunnuksen")

5.  Valitse sähköpostin Saapuneet-kansiosta sähköpostiviestin Salesforce.comista kanssa "**palvelupyyntötietueen suojauksen vahvistus**" aiheeksi.

6.  Tarkista tässä sähköpostin ja kopioi vahvistustunnuksen arvo suojaus.

7.  Valitse Azure perinteinen portal-integroinnin **salesforce eristetyn** sovellus-sivulla **Määritä käyttäjän valmistelu** -valintaikkunan avaaminen **Määritä käyttäjän valmistelu** .

    ![Määritä käyttäjän valmistelu] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC769573.png "Määritä käyttäjän valmistelu")

8.  **Salesforce eristetyn tunnistetietosi Ota käyttöön automaattinen käyttäjän valmistelu** sivulla Anna seuraavat asetukset:

    ![Salesforce eristetyn] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC746476.png "Salesforce eristetyn")

    a.  Kirjoita Salesforce-eristetyn tilinimen, jolla on **Järjestelmänvalvojan** profiilin määritetty Salesforce.comissa **Salesforce eristetyn järjestelmänvalvojan käyttäjänimi** -tekstiruutuun.

    b.  Kirjoita **Salesforce eristetyn järjestelmänvalvojan salasana** -tekstiruutuun tilin salasana.

    c-näppäinyhdistelmää.  Liitä suojauksen vahvistustunnuksen arvo **Käyttäjän suojauksen salasana** -tekstiruutuun.

    d.  Valitse **Vahvista** vahvistamiseksi kokoonpanosi.

    e.  Napsauta Avaa **Vahvista** -sivulla **Seuraava** -painiketta.

9.  Valitse **vahvistussivulla** **Valmis** tallentamaan kokoonpanosi.
##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-salesforce-sandbox-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä Salesforce eristetyn, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse **Salesforce eristetyn **sovelluksen integrointi-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC769574.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC767830.png "Kyllä")
  
Pitäisi nyt Odota 10 minuuttia ja varmista, että tili on synkronoitu Salesforce eristetyn.
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](https://msdn.microsoft.com/library/dn308586).

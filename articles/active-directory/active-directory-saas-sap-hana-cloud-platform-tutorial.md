<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi SAP HANA Cloud-ympäristössä | Microsoft Azure" 
    description="Opettele käyttämään SAP HANA Cloud-ympäristössä Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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
    ms.date="09/26/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-sap-hana-cloud-platform"></a>Opetusohjelma: SAP HANA Cloud-ympäristössä Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja SAP HANA Cloud-ympäristössä.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   SAP: in HANA Cloud-ympäristössä-tili
  
Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt SAP HANA Cloud-ympäristössä Azure AD-käyttäjien saa oikeuden kertakirjauksen käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.

>[AZURE.IMPORTANT]Haluat ottaa itse sovelluksen tai SAP HANA Cloud käyttöympäristön-tililläsi sovelluksen kertakirjauksen testaamista tilaaminen. Tässä opetusohjelmassa sovellus on otettu käyttöön tilissä.
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Sovelluksen integrointi SAP HANA Cloud-ympäristössä ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Roolin käyttäjälle
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790795.png "Skenaario")
##<a name="enabling-the-application-integration-for-sap-hana-cloud-platform"></a>Sovelluksen integrointi SAP HANA Cloud-ympäristössä ottaminen käyttöön
  
Tässä osassa tavoitteena on Jäsennys, miten voit ottaa käyttöön sovelluksen integrointi SAP HANA Cloud-ympäristössä.

###<a name="to-enable-the-application-integration-for-sap-hana-cloud-platform-perform-the-following-steps"></a>Ottaa käyttöön sovelluksen integroinnin SAP HANA Cloud-ympäristössä, toimi seuraavasti:

1.  Valitse **Active Directory**Azure-hallinta-portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **SAP HANA Cloud-ympäristössä**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790796.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **SAP HANA Cloud-ympäristössä**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![SAP-Hana] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793929.png "SAP-Hana")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen SAP HANA Cloud-ympäristössä käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.  
Tämän toimintosarjan osana on base-64-koodattu varmenteen lataaminen SAP HANA Cloud-ympäristössä alihallintaan.  
Jos et ole tottunut näin, katso, [miten voit muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **SAP HANA Cloud-ympäristössä** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC778552.png "Määritä kertakirjautuminen")

2.  **Miten käyttäjät voivat kirjautua SAP HANA Cloud-ympäristössä haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790797.png "Kertakirjautumisen määrittäminen")

3.  SAP: in HANA Cloud ympäristö Cockpit https://account kirjautua eri selainikkunassa. \<vaaka host\>.ondemand.com/cockpit (esimerkiksi: *https://account.hanatrial.ondemand.com/cockpit*).

4.  Valitse **Salli** -välilehti.

    ![Luota] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790800.png "Luota")

5.  Salli hallinta-osassa seuraavasti:

    ![Metatietojen noutaminen] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793930.png "Metatietojen noutaminen")

    1.  Valitse **Paikallinen palveluntarjoajan** -välilehti.
    2.  Voit ladata SAP HANA Cloud-ympäristössä metatieto-tiedosto valitsemalla **Hae metatiedot**.

6.  Azure Active perinteinen portaalissa, **Määritä sovellus URL-osoite** -sivulla suorittamalla seuraavat vaiheet ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790798.png "Sovelluksen URL-Osoitteen määrittäminen")

    1.  Kirjoita **Merkki-URL** -tekstiruutuun URL-osoite käyttää käyttäjien kirjautua **SAP HANA Cloud ympäristön** sovellukseen. Tämä on suojattu resurssin SAP HANA Cloud-ympäristössä sovelluksen tilin kielikohtaiset URL-osoite. URL-Osoitteen perustuu seuraavaa kaavaa: *https://\<applicationName\>\<accountName\>.\< vaaka-isännän\>.ondemand.com/\<polku\_,\_suojattu\_resurssin\> * (esimerkiksi: *https://xleavep1941203872trial.hanatrial.ondemand.com/xleave*)

        >[AZURE.NOTE]Tämä on käyttäjän todennusta edellyttävän SAP HANA Cloud-ympäristössä-sovelluksen URL-osoite.

    2.  Avaa ladattu SAP HANA Cloud-ympäristössä metatiedot-tiedosto ja Etsi **ns3:AssertionConsumerService** -tunniste.
    3.  **Sijainti** -määritteen arvon kopioi ja liitä se sitten **SAP HANA Cloud ympäristö vastaa URL** -tekstiruutuun.

7.  **Määritä kertakirjautumisen osoitteessa SAP HANA Cloud-ympäristössä** , sivulla lataamaan metatietojen valitsemalla **Lataa metatietojen**, ja tallenna sitten tiedosto tietokoneesta.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790799.png "Kertakirjautumisen määrittäminen")

8.  Suorita seuraavat vaiheet SAP HANA Cloud Platform-Cockpit **Paikallisen palveluntarjoajan** -osassa:

    ![Merkitseminen luotetuksi] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793931.png "Merkitseminen luotetuksi")

    1.  Valitse **Muokkaa**.
    2.  **Määritysten tyyppi**Valitse **Mukautettu**.
    3.  **Paikallinen tarjoajan nimi**Jätä oletusarvo.
    4.  Voit luoda **Allekirjoituksen avain** ja **Allekirjoitusvarmenne** avaimen pari valitsemalla **Luo avainpari**.
    5.  **Lainan välittäminen**Valitse **ei käytössä**.
    6.  **Voimassa todennusta**Valitse **ei käytössä**.
    7.  Valitse **Tallenna**.

9.  Valitse **Luotetut tunnistetietojen toimittaja** -välilehti ja valitse sitten **Lisää luotettu tunnistetietojen toimittaja**.

    ![Merkitseminen luotetuksi] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790802.png "Merkitseminen luotetuksi")

    >[AZURE.NOTE]Hallita käytettävissäsi olevia luotettu tunnistetietojen toimittajat, tarvitset olet valinnut haluamasi Mukautettu määritys paikallisen palveluntarjoajan-osassa. Määritysten oletustietotyyppi, sinulla ei voi muokata ja implisiittinen luota SAP ID-palveluun. Ei sinulla ei ole Salli asetuksia.

10. Valitse **Yleiset** -välilehti ja valitse sitten **Selaa** ladatut metatiedot-tiedoston lataaminen.

    ![Merkitseminen luotetuksi] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793932.png "Merkitseminen luotetuksi")

    >[AZURE.NOTE] **Sign-on URL**, **Yksi Kirjaudu URL-osoite** ja **Allekirjoitusvarmenne** arvot täytetään automaattisesti ladattuasi metatieto-tiedosto.

11. Valitse **määritteet** -välilehti.

12. Suorita **määritteet** -välilehdessä seuraavasti:

    ![Määritteet] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790804.png "Määritteet")

    1.  **Add Assertion-Based määrite**valitsemalla Lisää vahvistus-määritteet:

        |Vahvistus-määrite| Lyhennys määrite|
        |-------------------|--------------------|
        |http://schemas.xmlsoap.org/ws/2005/05/Identity/claims/givenName|   Etunimi|--------------------|--------------------|
        |http://schemas.xmlsoap.org/ws/2005/05/Identity/claims/surname|        Sukunimi|-----------|
        |http://schemas.xmlsoap.org/ws/2005/05/Identity/claims/emailaddress|sähköpostin|

    >[AZURE.NOTE]Määritteet määritysten määräytyy sen mukaan, miten HCP-sovellukset on kehitetty, eli mitkä määritteet odotuksia SAML vastauksessa ja mitkä nimellä (lyhennys määrite) hän käyttää tämän määritteen koodi.
    >  
    >a.  Näyttökuvan **Määritteen oletusarvo** on vain kuvassa tarkoituksiin. Se ei tarvitse tehdä toimi skenaario.  
    >
    >b.  Nimet ja arvot näkyvät näyttökuvan **Lyhennys määrite** määräytyvät sen mukaan, miten sovellus on kehitetty. On mahdollista, että sovelluksesi vaatii eri yhdistämismääritykset.

13. Azure perinteinen portaalissa-sivulla **Määritä kertakirjautumisen SAP HANA Cloud-ympäristössä,** valintaikkunan Valitse Sign määritysten vahvistus ja valitse sitten **Valmis**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC796933.png "Kertakirjautumisen määrittäminen")
  
Valinnainen vaihe voi määrittää vahvistus-pohjainen ryhmät Azure Active Directory tunnistetietojen palveluntarjoajan

>[AZURE.NOTE]Ryhmien käyttäminen SAP HANA Cloud-ympäristössä voit määrittää yhden tai usean käyttäjän dynaamisesti vähintään yksi rooleille SAP HANA Cloud-ympäristössä-sovellusten, SAML 2.0-vahvistus määritteiden arvojen määrittämä. Jos vahvistus määrite on esimerkiksi "*sopimuksen = tilapäinen*", haluat ehkä kaikkien tarvittavien käyttäjien lisääminen "*TILAPÄINEN*" ryhmän. "*Väliaikaiset*" ryhmän voi sisältää vähintään yhden tai usean sovelluksista, jotka on otettu käyttöön tililläsi SAP HANA Cloud-ympäristössä roolit.
>  
>Käytä vahvistus-pohjainen ryhmät, jos haluat massa Määritä vähintään yksi rooleille sovellusten SAP HANA Cloud-ympäristössä tilisi monta käyttäjää. Jos haluat määrittää yksi- tai käyttäjämäärä (a) tietyt roolit Suosittelemme varataan suoraan käyttöön SAP HANA Cloud-ympäristössä cockpit "**lupa**"-välilehdessä.

##<a name="assigning-a-role-to-a-user"></a>Roolin käyttäjälle
  
Jotta Azure AD-käyttäjät voivat kirjautua sisään SAP HANA Cloud-ympäristössä, niitä on määritettävä roolien SAP HANA Cloud-ympäristössä.

###<a name="to-assign-a-role-to-a-user-perform-the-following-steps"></a>Jos haluat määrittää käyttäjän rooli, toimimalla seuraavasti:

1.  Kirjaudu sisään **SAP HANA Cloud-ympäristössä** cockpit.

2.  Suorita seuraavat vaiheet:

    ![Lupa] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790805.png "Lupa")

    1.  Valitse **luvan**.
    2.  Valitse **käyttäjät** -välilehti.
    3.  Kirjoita käyttäjän sähköpostiosoite **käyttäjän** -tekstiruutuun.
    4.  Valitse Määritä käyttäjän rooli **määrittää** .
    5.  Valitse **Tallenna**.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-sap-hana-cloud-platform-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä SAP HANA Cloud-ympäristössä, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse **SAP HANA Cloud-ympäristössä** sovelluksen integrointi-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790806.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).
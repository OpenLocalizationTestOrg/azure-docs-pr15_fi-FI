<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi liukuma | Microsoft Azure" 
    description="Opettele käyttämään liukuma Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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
    ms.date="09/19/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-slack"></a>Opetusohjelma: Liukuma Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja liukuma integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Käytössä liukuma kertakirjautumisen tilaus
  
Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt liukuma Azure AD-käyttäjien saa oikeuden kertakirjauksen liukuma yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Liukuman sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-slack-tutorial/IC794980.png "Skenaario")

##<a name="enabling-the-application-integration-for-slack"></a>Liukuman sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön liukuma sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-slack-perform-the-following-steps"></a>Liukuman sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-slack-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-slack-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-slack-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-slack-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **liukuma**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-slack-tutorial/IC794981.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **liukuma**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Skenaario] (./media/active-directory-saas-slack-tutorial/IC796925.png "Skenaario")

##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen liukuma käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.  
Tämän toimintosarjan osana on base-64-koodattu sertifikaattitiedosto luomiseen.  
Jos et ole tottunut näin, katso, [miten voit muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **liukuma** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-slack-tutorial/IC794982.png "Kertakirjautumisen määrittäminen")

2.  **Miten Haluaisitko kirjautumaan liukuma käyttäjät** -sivulla **Microsoft Azure AD kertakirjautumisen**Valitse ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-slack-tutorial/IC794983.png "Kertakirjautumisen määrittäminen")

3.  Kirjoita **Määritä sovelluksen URL-osoite** -sivulla **Liukuma merkki-URL** -tekstiruutuun liukuma vuokraajan URL-osoite (esimerkiksi: "*https://azuread.slack.com*"), ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-slack-tutorial/IC794984.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  **Määritä kertakirjautumisen osoitteessa liukuma** -sivulla Lataa sertifikaatti, valitse **Lataa**, ja Tallenna sertifikaattitiedosto paikallisesti tietokoneeseen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-slack-tutorial/IC794985.png "Kertakirjautumisen määrittäminen")

5.  Eri selainikkunassa Kirjaudu liukuma yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

6.  Siirry **Microsoft Azure AD \> ryhmän asetuksia**.

    ![Ryhmän asetukset] (./media/active-directory-saas-slack-tutorial/IC794986.png "Ryhmän asetukset")

7.  Valitse **Ryhmän asetukset** -kohdassa Valitse **todennus** -välilehti ja valitse sitten **Muuta asetuksia**.

    ![Ryhmän asetukset] (./media/active-directory-saas-slack-tutorial/IC794987.png "Ryhmän asetukset")

8.  Valitse **SAML todennusasetukset** -valintaikkunassa seuraavasti:

    ![SAML-asetukset] (./media/active-directory-saas-slack-tutorial/IC794988.png "SAML-asetukset")

    1.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa liukuma** -valintasivun **SAML SSO URL-osoite** -arvo kopioi ja liitä se sitten **SAML 2.0 päätepisteen (HTTP)** -tekstiruutuun.
    2.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa liukuma** -valintasivun **Myöntäjä URL-osoite** -arvo kopioi ja liitä se sitten **Tunnistetietojen toimittaja myöntäjä** -tekstiruutuun.
    3.  Luo **base-64-koodattu** tiedosto ladattu varmennetta.
    
        >[AZURE.TIP] Lisätietoja on artikkelissa [muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o)

    4.  Avaa base-64-koodattu varmennetta Muistiossa, kopioi Leikepöydän sisällön se ja liitä se **Julkisen varmenne** -tekstiruutuun.
    5.  Poista **Salli käyttäjien muokata henkilön sähköpostiosoite**.
    6.  Valitse **Salli käyttäjien valita oman käyttäjänimi**.
    7.  **Ryhmän todennus on käytettävä mukaan**Valitse **on valinnainen**.
    8.  Valitse **Tallenna määritys**.

9.  Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-slack-tutorial/IC794989.png "Kertakirjautumisen määrittäminen")

##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Jotta voit ottaa käyttöön Azure AD-käyttäjät voivat kirjautua sisään liukuma, ne on valmisteltava kyselyjä liukuma.
  
Ei ei toimi, voit määrittää käyttäjän valmisteleminen liukuma.  
Kun määritetty käyttäjä yrittää kirjautua liukuma, liukuma tilin luodaan automaattisesti tarvittaessa.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-slack-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä liukuma, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **liukuma **-sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-slack-tutorial/IC794990.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-slack-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).
<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi TeamSeer | Microsoft Azure" 
    description="Opettele käyttämään TeamSeer Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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

#<a name="tutorial-azure-active-directory-integration-with-teamseer"></a>Opetusohjelma: TeamSeer Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja TeamSeer integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   TeamSeer palvelutili
  
Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt TeamSeer Azure AD-käyttäjien saa oikeuden kertakirjauksen TeamSeer yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  TeamSeer sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-teamseer-tutorial/IC789618.png "Skenaario")

##<a name="enabling-the-application-integration-for-teamseer"></a>TeamSeer sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön TeamSeer sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-teamseer-perform-the-following-steps"></a>TeamSeer sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-teamseer-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-teamseer-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-teamseer-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-teamseer-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **TeamSeer**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-teamseer-tutorial/IC789619.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **TeamSeer**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![TeamSeer] (./media/active-directory-saas-teamseer-tutorial/IC789620.png "TeamSeer")

##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen TeamSeer käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.  
Tämän toimintosarjan osana on base-64-koodattu sertifikaattitiedosto luomiseen.  
Jos et ole tottunut näin, katso, [miten voit muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **TeamSeer** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-teamseer-tutorial/IC789621.png "Kertakirjautumisen määrittäminen")

2.  **Miten käyttäjät voivat kirjautua TeamSeer haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-teamseer-tutorial/IC789628.png "Kertakirjautumisen määrittäminen")

3.  **Määrittää sovelluksen URL-osoite** -sivulla **TeamSeer merkki-URL** -tekstiruutuun Kirjoita käyttämällä seuraavaa kaavaa "*http://www.teamseer.com/companyid*" URL-osoite ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-teamseer-tutorial/IC789629.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  Valitse **Määritä kertakirjautumisen osoitteessa TeamSeer** -sivulla Lataa sertifikaatti, valitsemalla **Lataa**, ja Tallenna sertifikaattitiedosto tietokoneeseen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-teamseer-tutorial/IC789630.png "Kertakirjautumisen määrittäminen")

5.  Eri selainikkunassa Kirjaudu TeamSeer yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

6.  Valitse **HR järjestelmänvalvoja**.

    ![HR järjestelmänvalvoja] (./media/active-directory-saas-teamseer-tutorial/IC789634.png "HR järjestelmänvalvoja")

7.  Valitse **asetukset**.

    ![Asetukset] (./media/active-directory-saas-teamseer-tutorial/IC789635.png "Asetukset")

8.  Valitse **Määritä SAML tarjoajan tiedot**.

    ![SAML-asetukset] (./media/active-directory-saas-teamseer-tutorial/IC789636.png "SAML-asetukset")

9.  SAML tarjoajan tiedot-osassa seuraavasti:

    ![SAML-asetukset] (./media/active-directory-saas-teamseer-tutorial/IC789637.png "SAML-asetukset")

    1.  Azure perinteinen portaalissa-sivulla **Määritä kertakirjautumisen osoitteessa TeamSeer** valintaikkunan **Sign-palvelun URL** -arvon kopioi ja liitä **URL** -tekstiruutuun.
    2.  Luo **base-64-koodattu** tiedosto ladattu varmennetta.  

        >[AZURE.TIP] Lisätietoja on artikkelissa [muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o)

    3.  Avaa base-64-koodattu varmennetta Muistiossa, kopioi Leikepöydän sisällön se ja liitä se **IdP julkisen varmenne** -tekstiruutuun.

10. Viimeistele määritys SAML tarjoajan, toimimalla seuraavasti:

    ![SAML-asetukset] (./media/active-directory-saas-teamseer-tutorial/IC789638.png "SAML-asetukset")

    1.  Kirjoita **Testata sähköpostiosoitteet**testikäyttäjän sähköpostiosoite.
    2.  Kirjoita **myöntäjä** -tekstiruutuun palveluntarjoaja myöntäjä URL-osoite.
    3.  Valitse **Tallenna**.

11. Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-teamseer-tutorial/IC789639.png "Kertakirjautumisen määrittäminen")

##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Jos haluat ottaa käyttöön Azure AD-käyttäjät voivat kirjautua sisään TeamSeer, ne on valmisteltava ShiftPlanning kyselyjä.  
TeamSeer valmistelu on manuaalinen tehtävä.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Valmistelu käyttäjätilit, toimi seuraavasti:

1.  Kirjaudu sisään **TeamSeer** yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

2.  Suorita seuraavat vaiheet:

    ![HR järjestelmänvalvoja] (./media/active-directory-saas-teamseer-tutorial/IC789640.png "HR järjestelmänvalvoja")

    1.  Siirry **HR järjestelmänvalvojan \> käyttäjien**.
    2.  Valitse **ohjatun toiminnon uusi käyttäjä**.

3.  **Käyttäjätiedot** -osassa seuraavasti:

    ![Käyttäjätiedot] (./media/active-directory-saas-teamseer-tutorial/IC789641.png "Käyttäjätiedot")

    1.  Kirjoita **Etunimi**, **Sukunimi**, **käyttäjänimi (sähköpostiosoite)** haluat säännöstä kyselyjä liittyvät tekstiruutujen kelvollinen AAD tili.
    2.  Valitse **Seuraava**.

4.  Noudata käytössä näytön ohjeita Lisää uusi käyttäjä ja valitse **Valmis**.

>[AZURE.NOTE] Voit käyttää mitä tahansa muita TeamSeer käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä TeamSeer valmistelu Azure AD-käyttäjätilejä.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-teamseer-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä TeamSeer, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **TeamSeer **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-teamseer-tutorial/IC789642.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-teamseer-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).
<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi LogicMonitor | Microsoft Azure" 
    description="Opettele käyttämään LogicMonitor Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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

#<a name="tutorial-azure-active-directory-integration-with-logicmonitor"></a>Opetusohjelma: LogicMonitor Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja LogicMonitor integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   LogicMonitor palvelutili
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  LogicMonitor sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-logicmonitor-tutorial/IC790045.png "Skenaario")
##<a name="enabling-the-application-integration-for-logicmonitor"></a>LogicMonitor sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön LogicMonitor sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-logicmonitor-perform-the-following-steps"></a>LogicMonitor sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-logicmonitor-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-logicmonitor-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-logicmonitor-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-logicmonitor-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **logicmonitor**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-logicmonitor-tutorial/IC790046.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **LogicMonitor**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![LogicMonitor] (./media/active-directory-saas-logicmonitor-tutorial/IC790047.png "LogicMonitor")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen LogicMonitor käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **LogicMonitor **sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-logicmonitor-tutorial/IC790048.png "Kertakirjautumisen määrittäminen")

2.  **Miten käyttäjät voivat kirjautua LogicMonitor haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-logicmonitor-tutorial/IC790049.png "Kertakirjautumisen määrittäminen")

3.  Kirjoita **Määrittäminen sovelluksen URL-osoite** -sivulla **Merkki-URL** -tekstiruutuun URL-osoite käyttää käyttäjien kirjautumaan LogicMonitor \(e, g: "*http://company.logicmonitor.com*"\), ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-logicmonitor-tutorial/IC790050.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  Valitse **Määritä kertakirjautumisen osoitteessa LogicMonitor** -sivulla **Lataa metatiedot**ja tallenna se tietokoneeseen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-logicmonitor-tutorial/IC790051.png "Kertakirjautumisen määrittäminen")

5.  Kirjaudu sisään **LogicMonitor** yrityksen sivuston järjestelmänvalvojan oikeuksilla.

6.  Valitse ylä-valikossa **asetukset**.

    ![Asetukset] (./media/active-directory-saas-logicmonitor-tutorial/IC790052.png "Asetukset")

7.  Valitse vasemman reunan siirtymispalkin, kuuma **Kertakirjautuminen**

    ![Kertakirjautuminen] (./media/active-directory-saas-logicmonitor-tutorial/IC790053.png "Kertakirjautuminen")

8.  **Kertakirjautuminen (SSO)-asetukset** -osassa seuraavasti:

    ![Kertakirjauksen asetukset] (./media/active-directory-saas-logicmonitor-tutorial/IC790054.png "Kertakirjauksen asetukset")

    1.  Valitse **Ota käyttöön kertakirjautumisen**.
    2.  Valitse **Oletus roolimääritys**, **vain luku-tilassa**.
    3.  Avaa ladatut metatieto-tiedosto Muistiossa ja liitä tiedoston sisällön **Tunnistetietojen toimittaja metatiedot** -tekstiruutuun.
    4.  Valitse **Tallenna muutokset**.

9.  Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-logicmonitor-tutorial/IC790055.png "Kertakirjautumisen määrittäminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
AAD käyttäjät voivat kirjautua ne on valmisteltava käyttämällä Azure Active Directory-käyttäjätunnuksen LogicMonitor-sovellukseen.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Voit määrittää käyttäjän valmistelu toimimalla seuraavasti:

1.  Kirjaudu sisään LogicMonitor yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

2.  Valitse ylä-valikossa **asetukset**ja valitse sitten **roolit ja käyttäjät**.

    ![Roolit ja käyttäjät] (./media/active-directory-saas-logicmonitor-tutorial/IC790056.png "Roolit ja käyttäjät")

3.  Valitse **Lisää**.

4.  Valitse **Lisää tili** -osassa seuraavasti:

    ![Lisää tili] (./media/active-directory-saas-logicmonitor-tutorial/IC790057.png "Lisää tili")

    1.  Kirjoita haluat säännöstä kyselyjä liittyvät tekstiruutujen Azure Active Directory-käyttäjän **käyttäjänimi**, **sähköpostin**, **salasana** ja **Kirjoita salasana uudelleen** -arvot.
    2.  Valitse **roolit**, **oikeuksien tarkasteleminen** ja **tila**.
    3.  Valitse **Lähetä**.

>[AZURE.NOTE]Voit käyttää mitä tahansa muita LogicMonitor käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä LogicMonitor valmistelu Azure Active Directory käyttäjätilit.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-logicmonitor-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä LogicMonitor, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **LogicMonitor** sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-logicmonitor-tutorial/IC790058.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-logicmonitor-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).





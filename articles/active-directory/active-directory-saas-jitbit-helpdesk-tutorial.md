<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Jitbit tukipalvelu | Microsoft Azure" 
    description="Opettele käyttämään Jitbit tukipalvelu Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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

#<a name="tutorial-azure-active-directory-integration-with-jitbit-helpdesk"></a>Opetusohjelma: Jitbit tukipalvelu Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja Jitbit tukipalvelu integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Jitbit tukipalvelu palvelutili
  
Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt Jitbit tukipalvelu Azure AD-käyttäjien saa oikeuden kertakirjauksen Jitbit tukipalvelu yrityksen sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Jitbit tukipalvelu sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777676.png "Skenaario")
##<a name="enabling-the-application-integration-for-jitbit-helpdesk"></a>Jitbit tukipalvelu sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön Jitbit tukipalvelu sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-jitbit-helpdesk-perform-the-following-steps"></a>Jitbit tukipalvelu sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **Jitbit tukeen**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777677.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **Jitbit tukipalvelu**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![JitBit] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC781008.png "JitBit")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen Jitbit tukipalvelu käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa. .  
Tämän toimintosarjan osana on base-64-koodattu sertifikaattitiedosto luomiseen.  
Jos et ole tottunut näin, katso, [miten voit muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o).

>[AZURE.IMPORTANT] Jos haluat voi määrittää kertakirjautumisen Jitbit tukipalvelu vuokraajan, ota ensin Jitbit tukipalvelu tekniseen tukeen ja Hae tämä toiminto on käytössä.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **Jitbit tukipalvelu** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777678.png "Määritä kertakirjautuminen")

2.  **Miten käyttäjät voivat kirjautua Jitbit tukipalvelu haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777679.png "Määritä kertakirjautuminen")

3.  Kirjoita **Määritä sovelluksen URL-osoite** -sivulla **Jitbit tukipalvelu merkki-URL** -tekstiruutuun URL-osoite käyttämällä seuraavaa kaavaa "*https://\<vuokraajan nimi\>. Jitbit.com*", ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777528.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  Valitse **Määritä kertakirjautumisen osoitteessa Jitbit tukipalvelu** -sivulla Lataa sertifikaatti, **Lataa**ja Tallenna sertifikaattitiedosto paikallisesti kuin **c:\\Jitbit Helpdesk.cer**.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777680.png "Määritä kertakirjautuminen")

5.  Eri selainikkunassa Kirjaudu Jitbit tukipalvelu yrityksen sivuston järjestelmänvalvojan oikeuksilla.

6.  Valitse **hallinta**-sivun työkalurivillä.

    ![Hallinta] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777681.png "Hallinta")

7.  Valitse **Yleiset asetukset**.

    ![Käyttäjät, yritykset ja käyttöoikeudet] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777682.png "Käyttäjät, yritykset ja käyttöoikeudet")

8.  **Todennusasetukset** määritykset-osasta toimimalla seuraavasti:

    ![Todennusasetukset] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777683.png "Todennusasetukset")

    1.  Valitse **käyttöön SAML 2.0 yksittäisen Kirjaudu** sisään käyttämällä **OneLogin**Single Sign-On (SSO).
    2.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Jitbit tukipalvelu** valintaikkunan-sivulla kopioi **Service-palvelun (SP) aloittanut päätepisteen** arvo ja liitä se sitten **Päätepisteen URL** -tekstiruutuun.
    3.  Luo **base-64-koodattu** tiedosto ladattu varmennetta.
        
        >[AZURE.TIP]Lisätietoja on artikkelissa [muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o)

    4.  Avaa base-64-koodattu varmennetta, kopioi se sisällön Leikepöydän ja liitä se **X.509-varmenne** -tekstiruutu
    5.  Valitse **Tallenna muutokset**.

9.  Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777684.png "Määritä kertakirjautuminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Jotta voit ottaa käyttöön Azure AD-käyttäjät voivat kirjautua sisään Jitbit tukipalvelu, ne on valmisteltava Jitbit tukipalvelu kyselyjä.  
Tukipalvelu Jitbit valmistelu on manuaalinen tehtävä.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Valmistelu käyttäjätilit, toimi seuraavasti:

1.  Kirjaudu sisään **Jitbit tukipalvelu** alihallintaan.

2.  Valitse ylä-valikossa **hallinta**.

    ![Hallinta] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777681.png "Hallinta")

3.  Valitse **käyttäjät, yritykset ja käyttöoikeudet**.

    ![Käyttäjät, yritykset ja käyttöoikeudet] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777682.png "Käyttäjät, yritykset ja käyttöoikeudet")

4.  Valitse **Lisää käyttäjä**.

    ![Lisää käyttäjä] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777685.png "Lisää käyttäjä")

5.  Kirjoita Luo-osiossa haluat säätää huomioon seuraavat tekstiruutujen Azure AD-tilin tiedot: **käyttäjänimi**, **sähköpostin**, **Etunimi**, **Sukunimi**

    ![Luo] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777686.png "Luo")

6.  Valitse **Luo**.

>[AZURE.NOTE] Voit käyttää mitä tahansa muita Jitbit tukipalvelu käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä Jitbit tukipalvelu säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-jitbit-helpdesk-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä Jitbit tukipalvelu, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **Jitbit tukipalvelu **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777687.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).
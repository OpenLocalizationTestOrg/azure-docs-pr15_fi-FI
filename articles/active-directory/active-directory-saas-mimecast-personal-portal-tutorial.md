<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Mimecast henkilökohtainen Portal | Microsoft Azure" 
    description="Opettele käyttämään Mimecast henkilökohtaisen portaalin Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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

#<a name="tutorial-azure-active-directory-integration-with-mimecast-personal-portal"></a>Opetusohjelma: Mimecast henkilökohtainen Portal Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja Mimecast henkilökohtaisen portaalin integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Käytössä Mimecast henkilökohtaisen portaalin kertakirjautumisen tilaus
  
Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt Mimecast henkilökohtaisen portaalin Azure AD-käyttäjien saa oikeuden kertakirjauksen Mimecast henkilökohtaisen portaalin yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Sovelluksen integrointi Mimecast Omat portaalin ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794991.png "Skenaario")
##<a name="enabling-the-application-integration-for-mimecast-personal-portal"></a>Sovelluksen integrointi Mimecast Omat portaalin ottaminen käyttöön
  
Tässä osassa tavoitteena on Jäsennys, miten voit ottaa käyttöön sovelluksen integrointi Mimecast Omat portaalin.

###<a name="to-enable-the-application-integration-for-mimecast-personal-portal-perform-the-following-steps"></a>Ottaa käyttöön sovelluksen integroinnin Mimecast Omat portaalin, toimi seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **Mimecast henkilökohtaisen portaalin**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794992.png "Sovelluksen valikoima")

7.  Tulokset-ruudussa Valitse **Mimecast henkilökohtaisen portaalin**ja valitse sitten **Valmis** , voit lisätä sovelluksen.

    ![Mimecast henkilökohtainen Portal] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794993.png "Mimecast henkilökohtainen Portal")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen Mimecast Omat portaaliin niiden tilillä, käyttämällä SAML-protokollan perusteella federation Azure AD.  
Tämän toimintosarjan osana on base-64-koodattu sertifikaattitiedosto luomiseen.  
Jos et ole tottunut näin, katso, [miten voit muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **Mimecast henkilökohtaisen portaalin** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794994.png "Kertakirjautumisen määrittäminen")

2.  **Miten käyttäjät voivat kirjautua Mimecast henkilökohtaisen portaalin haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794995.png "Kertakirjautumisen määrittäminen")

3.  Kirjoita **Määritä sovelluksen URL-osoite** -sivulla **Mimecast Omat Portal merkki-URL** -tekstiruutuun käyttämä käyttäjien kirjautua Mimecast henkilökohtaisen portaalin sovelluksen URL-osoite (esimerkiksi: "https://webmail-uk.mimecast.com" tai "https://webmail-us.mimecast.com"), ja valitse sitten **Seuraava**.

    >[AZURE.NOTE] Kirjaudu URL-osoitteessa on tietyn alueen.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794996.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  **Määritä kertakirjautumisen osoitteessa Mimecast henkilökohtaisen portaalin** -sivulla Lataa sertifikaatti, valitse **Lataa**, ja Tallenna sertifikaattitiedosto paikallisesti tietokoneeseen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794997.png "Kertakirjautumisen määrittäminen")

5.  Eri selainikkunassa Kirjaudu Mimecast Omat portaaliin järjestelmänvalvojana.

6.  Siirry **Services \> sovelluksen**.

    ![Sovellukset] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794998.png "Sovellukset")

7.  Valitse **todennus-profiileista**.

    ![Todennus-profiileista] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794999.png "Todennus-profiileista")

8.  Valitse **Uusi todennusta profiilia**.

    ![Uusi käyttöoikeuksien prfil] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795000.png "Uusi käyttöoikeuksien prfil")

9.  **Todennus-profiili** -kohdassa toimimalla seuraavasti:

    ![Todennus prfil] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795001.png "Todennus prfil")

    1.  Kirjoita **kuvaus** -tekstiruutuun kokoonpanosi nimi.
    2.  Valitse **Pakota SAML todennus Mimecast henkilökohtainen portaalin**.
    3.  Valitse **palvelu**, **Azure Active Directory**.
    4.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Mimecast henkilökohtaisen portaalin** -valintasivun **Myöntäjä URL-osoite** -arvo kopioi ja liitä se sitten **Myöntäjä URL** -tekstiruutuun.
    5.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Mimecast henkilökohtaisen portaalin** -valintasivun **Remote sisäänkirjautumisen URL-osoite** -arvo kopioi ja liitä se sitten **Kirjautuminen URL** -tekstiruutuun.
    6.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Mimecast henkilökohtaisen portaalin** -valintasivun **Remote sisäänkirjautumisen URL-osoite** -arvo kopioi ja liitä se sitten **Kirjaudu ulos URL** -tekstiruutuun.  

        >[AZURE.NOTE] Sisäänkirjautumisen URL-osoite-arvo-ja uloskirjautuminen URL-osoite-arvo--Mimecast henkilökohtaisen portaalin on sama.

    7.  Luo **base-64-koodattu** tiedosto ladattu varmennetta.  

        >[AZURE.TIP]Lisätietoja on artikkelissa [muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o).

    8.  Avaa base 64-koodattu varmenteen Muistiossa, poista ensimmäisen rivin ("*--*") ja viimeisen rivin ("*--*"), kopioi se sisältöä Leikepöydän ja liitä se **Tunnistetietojen toimittaja varmenne (metatiedot)** -tekstiruutuun.
    9.  Valitse **Salli kertakirjauksen**.
    10. Valitse **Tallenna**.

10. Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795002.png "Kertakirjautumisen määrittäminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Jotta voit kirjautua Mimecast henkilökohtaisen portaalin Azure AD-käyttäjien käyttöön, ne on valmisteltava Mimecast Omat portaaliin.  
Mimecast henkilökohtaisen portaalin valmistelu on manuaalinen tehtävä.
  
Haluat rekisteröidä toimialue, ennen kuin voit luoda käyttäjille.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Voit määrittää käyttäjän valmistelu toimimalla seuraavasti:

1.  Kirjaudu **Mimecast henkilökohtaisen portaalin** järjestelmänvalvojana.

2.  Siirry **kansioiden \> sisäinen**.

    ![Hakemistojen selaaminen] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795003.png "Hakemistojen selaaminen")

3.  Valitse **Rekisteröi uusi toimialue**.

    ![Rekisteröi uusi toimialue] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795004.png "Rekisteröi uusi toimialue")

4.  Kun uudelle toimialueelle on luotu, valitse **Uusi osoite**.

    ![Uusi osoite] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795005.png "Uusi osoite")

5.  Valitse uusi osoite-valintaikkunassa seuraavasti:

    ![Tallenna] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795006.png "Tallenna")

    1.  Kirjoita **Sähköpostiosoite**, **Yleinen nimi**, **salasana** ja **Vahvista salasana** määritteet haluat säännöstä kyselyjä liittyvät tekstiruutujen kelvollinen AAD tili.
    2.  Valitse **Tallenna**.

>[AZURE.NOTE]Voit käyttää Mimecast henkilökohtaisen portaalin käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä Mimecast henkilökohtaisen portaalin säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen

Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-mimecast-personal-portal-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä Mimecast henkilökohtaisen portaalin, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **Mimecast Omat Portal **-sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795007.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).
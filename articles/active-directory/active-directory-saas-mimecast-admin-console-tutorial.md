<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Mimecast hallintakonsoli | Microsoft Azure" 
    description="Opettele käyttämään Mimecast hallintakonsoli Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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

#<a name="tutorial-azure-active-directory-integration-with-mimecast-admin-console"></a>Opetusohjelma: Mimecast hallintakonsoli Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja Mimecast hallintakonsoli integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Käytössä Mimecast hallintakonsoli kertakirjautumisen tilaus
  
Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt Mimecast hallintakonsoli Azure AD-käyttäjien saa oikeuden kertakirjauksen Mimecast hallintakonsoli yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Mimecast hallintakonsoli sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795008.png "Skenaario")
##<a name="enabling-the-application-integration-for-mimecast-admin-console"></a>Mimecast hallintakonsoli sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön Mimecast hallintakonsoli sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-mimecast-admin-console-perform-the-following-steps"></a>Mimecast hallintakonsoli sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **Mimecast järjestelmänvalvojan konsolissa**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795009.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **Mimecast hallintakonsoli**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Mimecast hallintakonsoli] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795010.png "Mimecast hallintakonsoli")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen Mimecast hallintakonsoli käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.  
Tämän toimintosarjan osana on base-64-koodattu sertifikaattitiedosto luomiseen.  
Jos et ole tottunut näin, katso, [miten voit muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **Mimecast hallintakonsoli** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795011.png "Kertakirjautumisen määrittäminen")

2.  **Miten käyttäjät voivat kirjautua Mimecast hallintakonsoli haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795012.png "Kertakirjautumisen määrittäminen")

3.  Kirjoita **Määritä sovelluksen URL-osoite** -sivulla **Mimecast järjestelmänvalvojan konsolissa merkki-URL** -tekstiruutuun käyttämä käyttäjien kirjautua Mimecast hallintakonsoli sovelluksen URL-osoite (esimerkiksi: "https://webmail-uk.mimecast.com" tai "https://webmail-us.mimecast.com"), ja valitse sitten **Seuraava**.

    >[AZURE.NOTE] Kirjaudu URL-osoitteessa on tietyn alueen.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795013.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  **Määritä kertakirjautumisen osoitteessa Mimecast hallintakonsoli** -sivulla Lataa sertifikaatti, valitse **Lataa**, ja Tallenna sertifikaattitiedosto paikallisesti tietokoneeseen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795014.png "Kertakirjautumisen määrittäminen")

5.  Eri selainikkunassa Kirjaudu Mimecast hallintakonsoli järjestelmänvalvojana.

6.  Siirry **Services \> sovelluksen**.

    ![Palvelut] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC794998.png "Palvelut")

7.  Valitse **todennus-profiileista**.

    ![Todennus-profiileista] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC794999.png "Todennus-profiileista")

8.  Valitse **Uusi todennusta profiilia**.

    ![Uusi todennus-profiileista] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795000.png "Uusi todennus-profiileista")

9.  **Todennus-profiili** -kohdassa toimimalla seuraavasti:

    ![Todennus-profiili] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795015.png "Todennus-profiili")

    1.  Kirjoita **kuvaus** -tekstiruutuun kokoonpanosi nimi.
    2.  Valitse **Pakota SAML todennuksen Mimecast järjestelmänvalvojan konsolissa**.
    3.  Valitse **palvelu**, **Azure Active Directory**.
    4.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Mimecast hallintakonsoli** -valintasivun **Myöntäjä URL-osoite** -arvo kopioi ja liitä se sitten **Myöntäjä URL** -tekstiruutuun.
    5.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Mimecast hallintakonsoli** -valintasivun **Remote sisäänkirjautumisen URL-osoite** -arvo kopioi ja liitä se sitten **Kirjautuminen URL** -tekstiruutuun.
    6.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Mimecast hallintakonsoli** -valintasivun **Remote sisäänkirjautumisen URL-osoite** -arvo kopioi ja liitä se sitten **Kirjaudu ulos URL** -tekstiruutuun.  

        >[AZURE.NOTE]Sisäänkirjautumisen URL-osoite-arvo- ja uloskirjautuminen URL-osoite-arvo koskevat Mimecast hallintakonsoli sama.

    7.  Luo **base-64-koodattu** tiedosto ladattu varmennetta.  

        >[AZURE.TIP]Lisätietoja on artikkelissa [muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o).

    8.  Avaa base 64-koodattu varmenteen Muistiossa, poista ensimmäisen rivin ("*--*") ja viimeisen rivin ("*--*"), kopioi se sisältöä Leikepöydän ja liitä se **Tunnistetietojen toimittaja varmenne (metatiedot)** -tekstiruutuun.
    9.  Valitse **Salli kertakirjauksen**.
    10. Valitse **Tallenna**.

10. Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795016.png "Kertakirjautumisen määrittäminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Jotta voit kirjautua Mimecast hallintakonsoli Azure AD-käyttäjien käyttöön, ne on valmisteltava Mimecast hallintakonsoli kyselyjä.  
Mimecast hallintakonsoli valmistelu on manuaalinen tehtävä.
  
Haluat rekisteröidä toimialue, ennen kuin voit luoda käyttäjille.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Voit määrittää käyttäjän valmistelu toimimalla seuraavasti:

1.  Kirjaudu **Mimecast hallintakonsoli** järjestelmänvalvojana.

2.  Siirry **kansioiden \> sisäinen**.

    ![Hakemistojen selaaminen] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795003.png "Hakemistojen selaaminen")

3.  Valitse **Rekisteröi uusi toimialue**.

    ![Rekisteröi uusi toimialue] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795004.png "Rekisteröi uusi toimialue")

4.  Kun uudelle toimialueelle on luotu, valitse **Uusi osoite**.

    ![Uusi osoite] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795005.png "Uusi osoite")

5.  Valitse uusi osoite-valintaikkunassa seuraavasti:

    ![Tallenna] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795006.png "Tallenna")

    1.  Kirjoita **Sähköpostiosoite**, **Yleinen nimi**, **salasana** ja **Vahvista salasana** määritteet haluat säännöstä kyselyjä liittyvät tekstiruutujen kelvollinen AAD tili.
    2.  Valitse **Tallenna**.

>[AZURE.NOTE]Voit käyttää Mimecast hallintakonsoli käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä Mimecast hallintakonsoli säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-mimecast-admin-console-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä Mimecast hallintakonsoli, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **Mimecast hallintakonsoli **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795017.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).
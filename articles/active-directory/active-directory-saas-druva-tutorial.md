<properties 
    pageTitle="Opetusohjelma: Azure Active Directory integrointi integrointi Druva | Microsoft Azure" 
    description="Opettele käyttämään Druva Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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

#<a name="tutorial-azure-active-directory-integration-integration-with-druva"></a>Opetusohjelma: Azure Active Directory integrointi Druva integrointi

Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja Druva integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Käytössä Druva kertakirjautumisen tilaus

Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt Druva Azure AD-käyttäjien saa oikeuden kertakirjauksen Druva yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.

Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Druva sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-druva-tutorial/IC795084.png "Skenaario")
##<a name="enabling-the-application-integration-for-druva"></a>Druva sovelluksen integrointi ottaminen käyttöön

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön Druva sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-druva-perform-the-following-steps"></a>Druva sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-druva-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-druva-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-druva-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-druva-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **Druva**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-druva-tutorial/IC795085.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **Druva**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Druva] (./media/active-directory-saas-druva-tutorial/IC795086.png "Druva")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen

Tässä osassa tavoitteena on jäsentäminen siitä, miten käyttäjät voivat todentaa Druva käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.  
Tämän toimintosarjan osana on base-64-koodattu sertifikaattitiedosto luomiseen.  
Jos et ole tottunut näin, katso, [miten voit muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o).

Druva sovelluksen odottaa SAML vahvistukset tietyssä muodossa, jossa pyydetään Lisää mukautettujen määritteiden yhdistämismäärityksiä **saml suojaustunnuksen määritteet** -määrityksiin.  
Seuraavassa näyttökuvassa näkyy esimerkki tämän.

![SAML-tunnuksen määritteet] (./media/active-directory-saas-druva-tutorial/IC795087.png "SAML-tunnuksen määritteet")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **Druva** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-druva-tutorial/IC795027.png "Kertakirjautumisen määrittäminen")

2.  **Miten käyttäjät voivat kirjautua Druva haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-druva-tutorial/IC795088.png "Kertakirjautumisen määrittäminen")

3.  Kirjoita **Määritä sovelluksen URL-osoite** -sivulla **Druva merkki-URL** -tekstiruutuun käyttämä käyttäjien kirjautua Druva sovelluksen URL-osoite (esimerkiksi: "*https://cloud.druva.com/home/*"), ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-druva-tutorial/IC795089.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  **Määritä kertakirjautumisen osoitteessa Druva** -sivulla Lataa sertifikaatti, valitse **Lataa**, ja Tallenna sertifikaattitiedosto paikallisesti tietokoneeseen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-druva-tutorial/IC795090.png "Kertakirjautumisen määrittäminen")

5.  Eri selainikkunassa Kirjaudu Druva yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

6.  Siirry **hallinta \> asetukset**.

    ![Asetukset] (./media/active-directory-saas-druva-tutorial/IC795091.png "Asetukset")

7.  Valitse Sign-On asetukset-valintaikkunassa seuraavasti:

    ![Yksi osa kertakirjauksen asetukset] (./media/active-directory-saas-druva-tutorial/IC795092.png "Yksi osa kertakirjauksen asetukset")

    1.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Druva** -valintasivun **Remote sisäänkirjautumisen URL-osoite** -arvo kopioi ja liitä se sitten **Tunnus palveluntarjoajan kirjautuminen URL-osoite** -tekstiruutu.
    2.  Azure perinteinen portaalissa-sivulla **Määritä kertakirjautumisen osoitteessa Druva** valintaikkunan Kopioi **Remote Kirjaudu URL-osoite** -arvo ja liitä se **Tunnus tarjoajan Kirjaudu URL** -tekstiruutuun.
    3.  Luo **base-64-koodattu** tiedosto ladattu varmennetta.  

        >[AZURE.TIP] Lisätietoja on artikkelissa [muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o)

    4.  Avaa base-64-koodattu varmennetta Muistiossa, kopioi Leikepöydän sisällön se ja liitä se **Tunnus tarjoajan varmenne** -tekstiruutu
    5.  Avaa **asetukset** -sivun, valitse **Tallenna**.

8.  Valitse **asetukset** -sivun **Luo SSO-tunnuksen**.

    ![Asetukset] (./media/active-directory-saas-druva-tutorial/IC795093.png "Asetukset")

9.  Valitse **Yksittäinen Sign-todennus suojaustunnuksen** -valintaikkunassa seuraavasti:

    ![SSO-tunnus] (./media/active-directory-saas-druva-tutorial/IC795094.png "SSO-tunnus")

    1.  Valitse **Kopioi**.
    2.  Valitse **Sulje**.

10. Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-druva-tutorial/IC795095.png "Kertakirjautumisen määrittäminen")

11. Valitse ylä-valikossa **määritteet** **SAML suojaustunnuksen määritteet** -valintaikkunan avaaminen.

    ![Määritteet] (./media/active-directory-saas-druva-tutorial/IC795096.png "Määritteet")

12. Lisää pakollinen määrite yhdistämismääritykset, toimi seuraavasti:

  	|Määritteen nimi|Määritteen arvo|
  	|---|---|
  	|insync\_auth\_tunnus|<*Leikepöytä-arvo*>|

    1.  Tietoja kullekin riville edellä olevan taulukon Valitse **Lisää käyttäjä-määrite**.
    2.  Kirjoita määritteen nimi näkyy kyseisen rivin **Määritteen nimi** -tekstiruutuun.
    3.  Kirjoita **Määritteen arvo** -tekstiruutuun kyseisen rivin määritteen arvo.
    4.  Valitse **Valmis**.

13. Valitse **Ota muutokset käyttöön**.
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu

Jos haluat ottaa käyttöön Azure AD-käyttäjät voivat kirjautua sisään Druva, ne on valmisteltava Druva kyselyjä.  
Druva valmistelu on manuaalinen tehtävä.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Voit määrittää käyttäjän valmistelu toimimalla seuraavasti:

1.  Kirjaudu sisään **Druva** yrityksesi sivuston järjestelmänvalvojana.

2.  Siirry **hallinta \> käyttäjien**.

    ![Käyttäjien hallinta] (./media/active-directory-saas-druva-tutorial/IC795097.png "Käyttäjien hallinta")

3.  Valitse **Luo uusi**.

    ![Käyttäjien hallinta] (./media/active-directory-saas-druva-tutorial/IC795098.png "Käyttäjien hallinta")

4.  Valitse Luo uusi käyttäjä-valintaikkunassa seuraavasti:

    ![Luo uusi käyttäjä] (./media/active-directory-saas-druva-tutorial/IC795099.png "Luo uusi käyttäjä")

    1.  Kirjoita sähköpostiosoite ja kelvollinen Azure Active Directory-käyttäjätilin nimi, jonka haluat valmistella liittyvät tekstiruutujen kyselyjä.
    2.  Valitse **Luo käyttäjä**.

>[AZURE.NOTE] Voit käyttää mitä tahansa muita Druva käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä Druva säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen

Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-druva-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä Druva, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **Druva **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-druva-tutorial/IC795100.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-druva-tutorial/IC767830.png "Kyllä")

Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).

<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Facebookiin töissä | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directory- ja Facebookin välinen töissä."
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="04/26/2016"
    ms.author="asmalser"/>


# <a name="tutorial-azure-active-directory-integration-with-facebook-at-work"></a>Opetusohjelma: Facebook töissä Azure Active Directory-integrointi

Tässä opetusohjelmassa tavoitteena on, kuinka voit integroida Facebook töissä Azure Active Directory (Azure AD).

Azure AD-integraation Facebook töissä, jonka avulla seuraavat edut: 

- Voit hallita Azure AD, kenellä on oikeus Facebook käytössä 
- Voit valmistella automaattisesti käyttäjille, joilla on myönnetty käyttöoikeus Facebookiin työpaikan tili
- Voit ottaa käyttöön automaattisesti Hae kirjautunut-on käytössä (kertakirjautumisen) Facebookiin niiden Azure AD-tilien käyttäjille
- Voit hallita asiakkaiden central-sijaintiin 

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).


## <a name="prerequisites"></a>Edellytykset 

Määrittää Azure AD-integroinnin CS tähteä, tarvitset seuraavat:

- Azure AD-tilaus
- Facebook, kertakirjautuminen käsitteleminen käytössä

Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/). 


## <a name="adding-facebook-at-work-from-the-gallery"></a>Lisääminen Facebookista töissä valikoimasta
Voit määrittää Facebook-integrointia töissä suoraan Azure AD-haluat lisääminen Facebookista töissä valikoimasta hallitun SaaS sovellusluettelo.

**Jos haluat lisätä Facebook töissä valikoimasta, toimi seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa. 

    ![Active Directory][1]

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** .
    
    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Sovellukset][4]

6. Kirjoita hakuruutuun **Facebook töissä**.

7. Tulokset-ruudussa Valitse **Facebook käytössä**ja valitse sitten **Valmis** , voit lisätä sovelluksen.


### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD-Single Sign-On määrittäminen

Tässä osassa kuvataan antaa käyttäjien todentamiseen Facebookiin töissä niiden tilillä Azure Active Directory federation perusteella SAML-protokollan avulla.

**Jos haluat määrittää Azure AD kertakirjautumisen Facebookiin töissä, toimimalla seuraavasti:**

1.  Kun olet lisännyt Facebook töissä Azure perinteinen-portaalissa, valitse **Määritä kertakirjautumisen**.

2.  **Määritä sovelluksen URL-osoite** -näytön Kirjoita URL-osoite, johon käyttäjien kirjautua Facebook-, työ-sovelluksen. Tämä on Facebook-työ vuokraajan URL-osoitteessa (esimerkiksi: https://example.facebook.com/). Kun olet valmis, valitse **Seuraava**.

3.  Eri selainikkunassa Kirjaudu sisään yrityksen sivustossa Facebook järjestelmänvalvojana.

4. Noudattamalla ohjeita seuraavassa URL-osoitteessa Määritä Facebook töissä, voit käyttää Azure AD tunnistetietojen toimittaja: [https://developers.facebook.com/docs/facebook-at-work/authentication/cloud-providers](https://developers.facebook.com/docs/facebook-at-work/authentication/cloud-providers)

5.  Kun valmis, palaa selainikkunat, jossa Azure perinteinen portaalin, valitse Vahvista menettelyn suorittamisen-valintaruutu ja valitse sitten **Seuraava** ja **Valmis**.


## <a name="automatically-provisioning-users-to-facebook-at-work"></a>Valmistelu automaattisesti käyttäjien käytössä Facebookiin

Azure AD tukee automaattisesti synchonize mahdollisuus määritetyt käyttäjät Facebookiin työpaikan tilin tiedot. Tämä automaattinen sychronization mahdollistaa Facebook käytössä, saat sallivat käyttäjien käytön ennen niiden-kirjautuminen ensimmäisen kerran yrität tarvitsemansa tiedot. Se myös poistaa valmistelee käyttäjien Facebookista töissä kun access on kumottu Azure AD.

Automaattinen valmisteleminen voidaan määrittää valitsemalla **Määritä tili valmistelu** Azure perinteinen portal-ikkunassa.

Saat lisätietoja siitä, miten voit määrittää Automaattinen valmisteleminen [https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers](https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers)


## <a name="assigning-users-to-facebook-at-work"></a>Käyttäjien määrittäminen Facebookiin käytössä

Valmistellun AAD-käyttäjät voivat voi tarkastella niiden Access-ruudun käytössä Facebook ne on määritettävä access Azure perinteinen portaalin sisällä.

**Voit määrittää käyttäjät Facebook käytössä:**

1.  Valitse Azure perinteinen portaalissa Facebook töissä aloitussivun, **määrittää tilit**.

2.  Valitse **Näytä** -valikossa, haluatko määrittää käyttäjän tai ryhmän Facebookiin työssä ja valintamerkki-painiketta.

3.  Valitse tulosluettelon ne käyttäjät tai ryhmän, jolle haluat määrittää Facebook töissä.

4.  Napsauta sivun alatunnisteeseen, **Liitä** -painiketta.


## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_04.png





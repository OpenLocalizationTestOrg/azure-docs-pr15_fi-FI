<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Evidence.com | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directory- ja Evidence.com välillä."
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
    ms.date="02/23/2016"
    ms.author="asmalser"/>


# <a name="tutorial-azure-active-directory-integration-with-evidencecom"></a>Opetusohjelma: Evidence.com Azure Active Directory-integrointi

Tässä opetusohjelmassa tavoitteena on näyttää, miten voit määrittää kertakirjautumisen Azure Active Directory (AAD) ja Evidence.com välillä. Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:
    
* Kelvollinen Microsoft Azure-tilaus
* Kertakirjautuminen kanssa Evidence.com tilauksen käytössä (sähköpostin earlyaccess@evidence.com Jos SAML-pohjaisen kertakirjautuminen ei ole otettu käyttöön)

Kun olet suorittanut tämän opetusohjelman, AAD käyttäjät, joille olet määrittänyt Evidence.com access saa oikeuden kertakirjauksen sovellukseen käyttämällä AAD Access-paneeli.

## <a name="add-evidencecom-to-your-directory"></a>Lisää Evidence.com-kansioon

Tässä osassa kuvataan Evidence.com lisäämisestä integroitu sovelluksena Azure Active Directoryn.

**Ottaa talteen sovelluksen-integroinnin valitseminen käyttöön:**

1.  Valitse **Active Directory** [Azure perinteinen portal](https://manage.windowsazure.com), valitse vasemmassa siirtymisruudussa.

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

4.  Avaa sovellus-valikoiman **Lisää**ja valitse sitten **Lisää sovellus-valikoimasta**.

5.  Kirjoita hakuruutuun **Evidence.com**.

6.  Valitse tulosruudussa **Evidence.com**ja valitse **Valmis** , voit lisätä sovelluksen.


## <a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen

Tässä osassa kuvataan antaa käyttäjien todentamiseen Evidence.com niiden tilillä Azure Active Directory federation perusteella SAML-protokollan avulla.

**Voit määrittää kertakirjautumisen toimimalla seuraavasti:**

1.  Kun olet lisännyt Evidence.com Azure perinteinen-portaalissa, valitse **Määritä kertakirjautumisen**. 
 
2.  Valitse seuraavassa näytössä Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

3.  Kirjoita Määritä sovelluksen URL-osoite-näytön URL-osoite kohtaa, johon käyttäjien kirjautua Evidence.com vuokraajan URL-osoite (Esimerkki: https://yourtenant.evidence.com ja valitse sitten **Seuraava**. 

4.  Valitse **Lataa varmenteen** linkkiä ja tallenna se paikalliseen asemaan. Tämän todistuksen ja metatietojen URL-osoitteet (kohteen tunnus, kirjaudu SSO URL- ja kirjaudu ulos URL-osoite) käytetään määrittämään SSO Evidence.com-sivustossa. 

5.  Erillinen verkko selainikkunassa, kirjaudu oman Evidence.com vuokraaja järjestelmänvalvojana ja siirry kohtaan **järjestelmänvalvoja** -välilehteä
      
6.  Valitse **viraston kertakirjautuminen**
 
7.  Valitse **SAML perusteella kertakirjauksen**
 
8.  Kopioi **Myöntäjä URL-osoite**, **Kertakirjautuminen** ja **Yksittäisen Kirjaudu ulos** Azure perinteinen portaalissa ja Evidence.com vastaavien kenttien arvot.

9.  Avaa varmenteen ladata vaiheessa 4 tekstieditorissa, kuten Notepad.exe, ja kopioi ja liitä sisältö **Suojausvarmenne** -ruutuun. 

10. Tallenna Evidence.com määritykset.
 
11. Azure perinteinen-portaalissa Valitse **Vahvista, että olet määrittänyt kertakirjautumisen edellä kuvatulla**. Tämä tarkistus käyttöön nykyinen sertifikaatti voit aloittaa tiedostojesi käytön tämän sovelluksen-valintaruutu.
 
12. Valitse Sign Vahvista-sivulla **Valmis**.  


## <a name="creating-an-evidencecom-test-user"></a>Evidence.com-testikäyttäjän luominen

Azure AD käyttäjät voivat kirjautua ne on valmisteltava käytön Evidence.com-sovelluksessa. Tässä osassa kuvataan, miten Evidence.com sisällä Azure AD-Käyttäjätilien luominen

**Valmistelu Evidence.com käyttäjän:**

1.  Web-selainikkunassa Kirjaudu sisään Evidence.com yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

2.  Siirry **järjestelmänvalvoja** -välilehteä.

3.  Valitse **Lisää käyttäjä**.

4.  Napsauta **Lisää** -painiketta.

5.  Lisätty käyttäjän **Sähköpostiosoite** on vastattava käyttäjien käyttäjänimi Azure AD, jotka haluat antaa käyttöoikeuden. Jos käyttäjänimi ja sähköpostiosoite eivät ole organisaatiossa sama arvo, voit käyttää **Evidence.com > määritteet > kertakirjautumisen** Azure perinteinen portaalin nameidenitifer vastattaessa Evidence.com sähköpostiosoite on muutettava osa.


## <a name="assigning-users-to-evidencecom"></a>Käyttäjien määrittäminen Evidence.com

Valmistellun AAD-käyttäjät voivat mahdollisesti nähdä Evidence.com niiden Access-ruudun ne on määritettävä access Azure perinteinen portaalin sisällä.

**Jos haluat määrittää käyttäjiä Evidence.com:**

1.  Napsauta pika-aloitusopas sivun Evidence.com Azure perinteinen-portaalissa, **Evidence.com käyttäjien määrittäminen**.
 
2.  Valitse **Näytä** -valikossa, haluatko määrittää käyttäjän tai ryhmän Evidence.com ja valintamerkki-painiketta.
 
3.  Valitse **käyttäjät** -luettelosta, jolle haluat määrittää Evidence.com-ryhmän käyttäjät.
 
4.  Napsauta sivun alatunnisteeseen, **Liitä** -painiketta.


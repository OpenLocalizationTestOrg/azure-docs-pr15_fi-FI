<properties
   pageTitle="Azure Active Directory-sovelluksen valikoiman sovelluksen luettelo"
   description="Miten luettelon sovellus, joka tukee kertakirjautumisen Azure Active Directory-valikoiman | Microsoft Azure"
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin"/>


# <a name="listing-your-application-in-the-azure-active-directory-application-gallery"></a>Azure Active Directory-sovelluksen valikoiman sovelluksen luettelo

Luettelon sovellus, joka tukee kertakirjautumisen Azure Active Directory-hakemistosta [Azure AD-valikoima](https://azure.microsoft.com/marketplace/active-directory/all/), sovellus täytyy ensin toteuttamisesta jokin seuraavista integrointi tilasta:

* **OpenID yhteyden** - suoran OpenID yhdistämistä käyttämällä todennus ja määritysten Azure AD-suostumusta API Azure AD-integroinnin. Jos olet juuri mahdollisesti integrointi ja sovelluksen ei tue SAML, tämä on suositellun-tilassa.

* **SAML** – sovelluksen jo on mahdollisuus määrittää kolmannen osapuolen-tunnistetietojen toimittajat SAML-protokollan avulla.

Luettelon vaatimukset moodin ovat jäljempänä.

##<a name="openid-connect-integration"></a>OpenID yhteyden integrointi

Jos haluat liittää sovelluksen Azure AD- [developer ohjeita](active-directory-authentication-scenarios.md). Viimeistele alla olevia kysymyksiä ja lähetä sitten waadpartners@microsoft.com.

* Antaa testi vuokraajan tai tilin tunnistetiedot, jotka voidaan käyttää Azure AD-ryhmän Testaa integrointi sovelluksen.  

* Anna ohjeita siitä, miten Azure AD-ryhmän voit kirjautua sisään ja Azure AD-esiintymän yhdistäminen käyttämällä [Azure AD suostumus framework](active-directory-integrating-applications.md#overview-of-the-consent-framework)sovelluksen. 

* Anna Testaa kertakirjautumisen sovelluksen Azure AD-ryhmän vaaditaan muita ohjeita. 

* Anna tiedot, jotka ovat jäljempänä:

> Yrityksen nimi:
> 
> Yrityksen sivusto:
> 
> Sovelluksen nimi:
> 
> Sovelluksen kuvaus (enintään 256 merkkiä):
> 
> Sovelluksen sivusto (tiedoksi):
> 
> Sovelluksen tekninen tuki-sivustosta tai yhteystiedot:
> 
> Asiakastunnus sovelluksen-sovellusta koskevat tiedot https://manage.windowsazure.com mukaisesti:
> 
> Jos asiakkaiden Siirry rekisteröitymään sovelluksen ISP-kirjautumistarjoukset URL-osoite ja solujen ostaa sovelluksen:
> 
> Valitse kolmeen eri sovelluksesi (varten käytettävissä olevat luokat on artikkelissa Azure Active Directory-Marketplacesta) luettelossa:
> 
> Liitä sovelluksen pientä kuvaketta (PNG-tiedoston, 45px 45px, tasaisen taustavärin mukaan):
> 
> Liitä sovelluksen suuret kuvakkeet (PNG-tiedoston, 215px 215px, tasaisen taustavärin mukaan):
> 
> Liitä sovelluksen logoa (PNG-tiedoston, 150px 122px, läpinäkyvä taustaväri mukaan):

##<a name="saml-integration"></a>SAML-integrointi

Sovellusta, joka tukee SAML 2.0 voi integroida suoraan [seuraavia](active-directory-saas-custom-apps.md)ohjeita voit lisätä mukautetun sovelluksen Azure AD-vuokraajan kanssa. Kun olet testannut sovelluksen integrointi toimii Azure AD, Lähetä tiedot <waadpartners@microsoft.com>.

* Antaa testi vuokraajan tai tilin tunnistetiedot, jotka voidaan käyttää Azure AD-ryhmän Testaa integrointi sovelluksen.  

* Anna sovelluksesi kuin kuvattu [seuraavassa](active-directory-saas-custom-apps.md)SAML Sign-On URL-osoite, myöntäjä URL-osoite (kohteen tunnus) ja vastaa URL-osoite (vahvistus kuluttaja-palvelu)-arvot. Jos nämä arvot saadaan yleensä osana SAML metatieto-tiedosto, valitse Lähetä, sekä.

* Ongelman lyhyt kuvaus siitä, miten voit määrittää Azure AD tunnistetietojen toimittaja käyttämällä SAML 2.0-sovelluksessa. Jos sovellus tukee määrittäminen Azure AD tunnistetietojen toimittaja palvelun Omatoiminen järjestelmänvalvojan portaalissa nimellä, valitse Varmista, että yllä tunnistetiedot ovat mahdollisuus määrittää tämän.

* Anna tiedot, jotka ovat jäljempänä:

> Yrityksen nimi:
> 
> Yrityksen sivusto:
> 
> Sovelluksen nimi:
> 
> Sovelluksen kuvaus (enintään 256 merkkiä):
> 
> Sovelluksen sivusto (tiedoksi):
> 
> Sovelluksen tekninen tuki-sivustosta tai yhteystiedot:
> 
> Jos asiakkaiden Siirry rekisteröitymään sovelluksen ISP-kirjautumistarjoukset URL-osoite ja solujen ostaa sovelluksen:
> 
> Kolmeen eri sovelluksesi luettelossa (varten käytettävissä olevat luokat on artikkelissa [Azure Active Directory-Marketplacesta](https://azure.microsoft.com/marketplace/active-directory/)) Valitse):
> 
> Liitä sovelluksen pientä kuvaketta (PNG-tiedoston, 45px 45px, tasaisen taustavärin mukaan):
> 
> Liitä sovelluksen suuret kuvakkeet (PNG-tiedoston, 215px 215px, tasaisen taustavärin mukaan):
> 
> Liitä sovelluksen logoa (PNG-tiedoston, 150px 122px, läpinäkyvä taustaväri mukaan):

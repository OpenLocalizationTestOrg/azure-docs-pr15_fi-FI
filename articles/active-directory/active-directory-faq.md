<properties
    pageTitle="Azure Active Directory usein kysytyt kysymykset | Microsoft Azure"
    description="Azure Active Directory usein kysytyt kysymykset, jotka on vastauksia kysymyksiin yhdessä käyttämisessä Azure ja Azure Active Directory-salasanan hallinta ja sovelluksen käyttöä."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/16/2016"
    ms.author="markusvi"/>

# <a name="azure-active-directory-faq"></a>Azure Active Directory usein kysytyt kysymykset

Azure Active Directory on täydellinen tunnistetietojen Service (IDaaS)-ratkaisun, joka ulottuu ‑sivun tunnistetietojen, käyttöoikeuksien hallinnan ja suojaus.


Lisätietoja on artikkelissa [Azure Active Directory ominaisuudet?](active-directory-whatis.md).



## <a name="accessing-azure-and-azure-active-directory"></a>Azure ja Azure Active Directory


**K: Miksi "tilauksia ei löydy", kun yritän avata Azure AD Azure perinteinen portaalissa (https://manage.windowsazure.com)?**

**A:** Azure perinteinen-portaalin käyttöä edellyttää, että kullekin käyttäjälle oikeudet Azure-tilauksessa. Jos sinulla on maksettu Office 365: ssä tai [http://aka.ms/accessAAD](http://aka.ms/accessAAD) kertaluonteista aktivointi vaiheen Siirry Azure AD-muuten tarvitset täydet [Azure kokeiluversio](https://azure.microsoft.com/pricing/free-trial/) tai maksetun tilauksen aktivoiminen. 

Lisätietoja on artikkelissa:

- [Kuinka Azure tilaukset on liitetty Azure Active Directory](active-directory-how-subscriptions-associated-directory.md)

- [Office 365-tilauksen Azure Directoryn hallinta](active-directory-manage-o365-subscription.md)

---

**K: mikä on Azure AD-suhteen Office 365: ssä ja Azure?**

**A:** Azure Active Directory sisältää kaikki Microsoft online Services yleisiä tunnistetietojen ja käytön ominaisuuksia. Onko käytössäsi on Office 365: ssä, Microsoft Azure, Intune tai muiden, on jo käytössä Azure AD Sign ottaminen käyttöön ja käsitellä kaikkien näiden palvelujen hallinta. 

Itse asiassa kaikki käyttäjät, jotka on otettu käyttöön Microsoft Online Services on määritetty käyttäjätilit Azure AD-tapauksissa. Voit ottaa nämä tilit maksutta Azure AD-ominaisuudet, kuten sovelluksen pilvikäytön.
 
Lisäksi Azure AD maksettu palveluita (esimerkiksi: Azure AD basic-Premium, EMS, jne.) täydentää muut verkossa palvelut, kuten Office 365: ssä ja Microsoft Azure kattava asteikko hallinta- ja ratkaisujen.


---



## <a name="getting-started-with-hybrid-azure-ad"></a>Hybrid Azure AD käytön aloittaminen


**K: miten oma paikallisesta hakemistosta voit muodostaa yhteyden Azure AD?**

**A:** Voit muodostaa yhteyden käyttämällä **Azure AD Connect**Azure AD paikallisesta hakemistosta. 

Lisätietoja on artikkelissa [integrointi paikallisen-käyttäjätietojen Azure Active Directory-hakemistosta](active-directory-aadconnect.md).


---

**K: Miten määritän SSO Omat paikallista hakemistoa ja cloud-sovellusten välillä?**

**A:** Tarvitset vain SSO määrittäminen paikallista hakemistoa ja Azure AD välillä. Kun cloud-sovellusten käyttäminen Azure AD-palvelun ohjaa automaattisesti oikein todentamismenetelmä paikallisen käyttöoikeudet käyttäjille.

Käyttöönoton SSO-paikallisen helposti onnistuu federation ratkaisujen ADFS kuten tai määrittämällä salasanan hash synkronointi. Voit helposti käyttää molemmat vaihtoehdot käytettäessä ohjattua Azure AD Connect.
  

Lisätietoja on artikkelissa [integrointi paikallisen-käyttäjätietojen Azure Active Directory-hakemistosta](active-directory-aadconnect.md).
  

---

**K: Azure Active Directory antaa Omatoiminen portal organisaation käyttäjien?**

**A:** Kyllä, Azure Active Directoryn avulla voit Omatoiminen käyttäjä-ja sovellusten käyttö [Azure AD Access-paneeli](http://myapps.microsoft.com) . Jos olet Office 365-tilaus, voit etsiä monia samoja ominaisuuksia Office 365-portaalissa. 

Lisätietoja on artikkelissa [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md). 



---

**K: Azure AD auttaa paikallisen-infrastruktuurin hallintaan?**

**A:** Kyllä, se tekee. Azure AD Premium-version avulla voit **Muodostaa kunto**. Azure AD yhteyden kunto avulla voit valvoa ja Hanki tietoja paikallisen identity-infrastruktuuria ja synkronointipalvelut.  

Lisätietoja on artikkeleissa [näytön paikallisen että käyttäjätietojen infrastruktuuri ja synkronointi Servicesin pilveen](active-directory-aadconnect-health.md).  

---

## <a name="password-management"></a>Salasanojen hallinta

**K: Voinko käyttää Azure AD salasanan takaisinkirjoituksen ilman salasanan synkronointi? (Eli, haluan käyttää Azure AD Vaihtaminen salasanan takaisinkirjoituksen mutta en halua Omat salasanat cloud? tallennettu)**

**A:** Sinun ei tarvitse synkronoida Azure AD AD salasanat takaisinkirjoituksen ottaminen käyttöön. Liitetyt ympäristössä Azure AD SSO riippuvainen paikallisesta hakemistosta todennetaan. Tässä skenaariossa ei tarvita paikallisen salasana on seurattava Azure AD.

---

**K: miten paljon kuluu aikaa kirjoitetaan takaisin paikalliseen AD salasanan?**

**A:** Salasana: takaisinkirjoituksen toimii reaaliajassa. 

Lisätietoja on ohjeaiheessa [käytön aloittaminen salasanojen hallinta](active-directory-passwords-getting-started.md) 


---

**K: Voinko käyttää salasanan takaisinkirjoituksen salasanalla, joita hallitaan järjestelmänvalvoja?**

**A:** Kyllä, jos sinulla on käytössä takaisinkirjoituksen salasanan, järjestelmänvalvojan salasanan toimintoja kirjoitetaan takaisin paikalliseen ympäristöön.  

Lisätietoja on artikkelissa salasanan liittyvissä kysymyksissä artikkelissa [Salasanan hallinta usein kysyttyihin kysymyksiin](active-directory-passwords-faq.md).

---

## <a name="application-access"></a>Sovellusten käyttö


**K: Mistä löydän sovelluksia, jotka ovat valmiiksi integrointi Azure AD luettelo ja niiden ominaisuudet?**

**A:** Azure AD on yli 2600 valmiiksi integroitujen sovellusten Microsoftilta, sovelluksen palveluntarjoajien tai kumppanit. Kaikki valmiiksi integroidut sovellukset tukevat SSO. SSO mahdollistaa organisaation tunnistetietojen avulla voit käyttää sovelluksia. Jotkin sovellukset tukevat myös Automaattinen valmistelu ja sen valmistelu

Katso luettelo kaikista valmiiksi integroitujen sovellusten, [Active Directory-Marketplace](https://azure.microsoft.com/marketplace/active-directory/).


---

**K: entä jos tarvitsen sovellus ei ole Azure AD-marketplace?**

**A:** Azure AD-Premium voit lisätä ja jokin sovellus, jonka haluat määrittää. Voit määrittää SSO ja automaattinen valmistelu, sovelluksen ominaisuuksien ja asetusten mukaan.  

Lisätietoja on artikkelissa:

- [Kertakirjautumisen sovelluksia, jotka eivät ole Azure Active Directory-sovelluksen valikoiman määrittäminen](active-directory-saas-custom-apps.md)
- [Ota käyttöön automaattinen valmisteleminen käyttäjien ja ryhmien Azure Active Directorysta sovellusten SCIM avulla](active-directory-scim-provisioning.md) 


---

**K: kuinka käyttäjät rekisteröinti sovelluksiin Azure Active Directoryn avulla?**
 
**A:** Azure Active directory on useita tapoja, käyttäjät voivat tarkastella ja käsitellä verkkosovelluksistaan seuraavasti:

- Azure AD access-ruutu

- Office 365: n sovellusten käynnistys

- Suora Sign liitetyt sovelluksiin

- Laaja linkkejä useista lähteistä, salasana tai aiemmin sovellukset

Lisätietoja on artikkelissa [Azure AD käyttöönotto integroitu sovelluksia käyttäjille](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).


---

**K: mitkä ovat eri tavoista, joilla Azure Active Directory mahdollistaa tarkistamisen ja kertakirjautumisen sovelluksia?**
 
**A:** Azure Active Directory tukee monia standardoidun protokollat todennus- ja esimerkiksi SAML 2.0, OpenID yhteyden, OAuth 2.0 ja WS Federation. Azure AD tukee myös salasanan vaulting ja automaattisen kirjautumisen ominaisuuksia sovellukset, jotka tukevat vain Lomakepohjainen todentaminen.  

Lisätietoja on artikkelissa:

- [Azure AD todennus tilanteita, joissa](active-directory-authentication-scenarios.md)

- [Active Directory käyttöoikeuksien protokollat](https://msdn.microsoft.com/library/azure/dn151124.aspx)

- [Miten yksittäinen Kirjaudu sisään Azure Active Directory-työ?](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work)


---

**K: Voinko lisätä käytössäni paikallisen sovelluksia?**

**A:** Välityspalvelimen Azure AD-sovelluksen avulla voit helposti ja turvallisesti käytettävässä paikallisen web-sovellusten, joka on valittava. Voit käyttää näiden sovellusten samalla tavalla, kun käytät SaaS sovelluksia Azure Active Directoryn. Ei ole tarpeen VPN ja verkkoinfrastruktuuria muokkaamisen.  

Tutustu lisätietoja [etäkäyttöä paikallisen sovelluksia](active-directory-application-proxy-get-started.md).


--- 

**K: miten MFA edellyttävät ulkoisille käyttäjille tietyn-sovelluksen?**

**A:** Azure AD ehdollinen Accessissa voit määrittää yksilölliset käyttöoikeuskäytäntö kunkin sovelluksen. Käytännön voit määrittää MFA aina tai kun käyttäjät eivät ole yhteydessä paikalliseen verkkoon.  

Lisätietoja on artikkelissa [Office 365: ssä ja muiden sovellusten yhteydessä Azure Active Directory käyttöoikeuksien Securing](active-directory-conditional-access.md).


---

**K: mikä on automaattinen käyttäjän valmistelu SaaS sovelluksia?**

**A:** Azure Active Directoryn avulla voit automatisoida luominen, ylläpito ja monet Suositut cloud (SaaS)-sovelluksissa käyttäjätietojen poistaminen. 

Lisätietoja on artikkelissa [automatisoida käyttäjän valmistelu ja Deprovisioning SaaS sovellusten Azure Active Directory-hakemistosta](active-directory-saas-app-provisioning.md)

---




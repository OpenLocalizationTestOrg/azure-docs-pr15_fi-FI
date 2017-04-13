<properties
    pageTitle="Azure Active Directory-tunnistetietojen suojaus playbook | Microsoft Azure"
    description="Katso, miten Azure AD-tunnistetiedot avulla voit rajoittaa luvaton hyödyntää vaarantuneen tunnistetiedot tai laite ja suojaamiseen jäsenyyden tai laitteella, joka oli aiemmin epäilty tai tiedossa on käsiin."
    services="active-directory"
    keywords="Azure active Directoryn tunnistetietojen suojaus-cloud app etsiminen-sovellukset, suojaus, riski, riskin taso, heikkous, suojauskäytäntö hallinta"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="markvi"/>

#<a name="azure-active-directory-identity-protection-playbook"></a>Azure Active Directory-tunnistetietojen suojaus playbook 

Tämä playbook avulla voit:

- Täytä tiedot tunnistetiedot-ympäristössä, jotka jäljittelevät riskin tapahtumien ja heikkouksien
- Riskiin perustuva ehdollisen käyttöoikeuden käytäntöjen määrittäminen ja testaus käytännöt vaikutus


## <a name="simulating-risk-events"></a>Jotka jäljittelevät riskin tapahtumat

Tässä osassa on vaiheisiin seurantatyökalu riskin tapahtuman seuraavanlaisia:

- Kirjaudu apuohjelmien anonyymien IP-osoitteista (helposti)
- Kirjaudu apuohjelmien tuntemattomia sijainneista (normaali)
- Mahdotonta matka, jotka sijainteihin (vaikeaa)

Riskin muita tapahtumia ei voi olla Simuloitu turvallisesti.


### <a name="sign-ins-from-anonymous-ip-addresses"></a>Kirjaudu apuohjelmien anonyymien IP-osoitteista

Riskin tapahtuman tällaista määrittää käyttäjät, jotka olet kirjautunut sisään-IP-osoite, joka on määritetty anonyymi välityspalvelimen IP-osoite. Nämä välityspalvelimet käyttävät henkilöt, jotka haluat piilottaa niiden laitteen IP-osoite ja niitä voi käyttää väärinkäyttömahdollisuuksien takia.

**Jotta saadaan aikaan Kirjaudu sisään anonyymi IP-toimimalla seuraavasti**:

1.  Lataa [Tor selaimessa](https://www.torproject.org/projects/torbrowser.html.en).
2.  Siirry selaimella Tor [https://myapps.microsoft.com](https://myapps.microsoft.com).   
3.  Kirjoita **Kirjaudu apuohjelmien anonyymi IP-osoitteista** raportissa näytettävät tilin tunnistetiedot.

Kirjautuminen-näkyvät tunnistetiedot koontinäytössä 5 minuutin kuluessa. 


###<a name="sign-ins-from-unfamiliar-locations"></a>Kirjaudu apuohjelmien tuntemattomia sijainneista

Tuntemattomia sijainnit riski on reaaliaikainen kirjautumisen arviointi-toimintoa, joka katsoo aiempia sijainnit-kirjautuminen (IP, leveyttä / pituusasteet ja ASN) määrittää uuden / tuntemattomia sijainnit. Järjestelmä tallentaa edellisen IP-osoitteet, leveyttä / pituusasteet ja lähetysilmoitukset käyttäjän ja pitää nämä on tuttuja sijainnit. Kirjaudu sisään sijainti pidetään tuntemattomia, jos Kirjaudu sisään-sijainti ei vastaa mitään olemassa olevat tuttuja sijainnit.

Azure Active Directory-tunnistetietojen suojaus:  

 - on alkuperäinen learning-ajan 14 päivän ajan, jolloin se ei merkitse uusia sijainteja tuntemattomia sijaintiin.
 - ohittaa Kirjaudu apuohjelmien tuttuja laitteet ja sijainneista, jotka ovat maantieteellisesti lähellä tuttuja olemassa olevaan sijaintiin.

Voit simuloida tuntemattomia sijainnit, sinun on kirjauduttava sijainti ja laite, jonka tili on ole kirjautunut sisään-ennen. 


**Jotta saadaan aikaan Kirjaudu sisään tuntemattomia sijainnista, toimi seuraavasti**:

1.  Valitse tili, jossa on vähintään 14 päivän kirjautumisen historia. 

2.  Tee jompikumpi:
    
    a. VPN-yhteyttä käytettäessä Siirry [https://myapps.microsoft.com](https://myapps.microsoft.com) ja kirjoita simuloida riskin tapahtuman tilin tunnistetiedot.

    b. Pyydä Liitä eri sijainnissa Kirjaudu sisään käyttämällä tilin tunnistetiedot (ei suositella).

Kirjautuminen-näkyvät tunnistetiedot koontinäytössä 5 minuutin kuluessa.
 
### <a name="impossible-travel-to-atypical-location"></a>Mahdotonta matka, jotka sijaintiin
Simuloimalla mahdotonta matka-ehto on vaikeaa, koska algoritmin käyttää konepohjaisten oppimistekniikoiden rikkaruohojen, EPÄTOSI-positiivisten, kuten tutut laitteilla mahdotonta matka tai merkki-laajennukset-VPN-yhteydet, jotka käyttävät muita käyttäjää hakemistossa. Lisäksi algoritmin edellyttää kirjautumisen historiatiedot 3-14 päivän käyttäjälle, ennen kuin se alkaa luodaan riskin tapahtumat.

**Voit simuloida mahdotonta matka, jotka sijaintiin, toimi seuraavasti**:

1.  Siirry [https://myapps.microsoft.com](https://myapps.microsoft.com)vakio-selainta käyttäen.  

2.  Kirjoita Luo mahdotonta matka riskin tapahtuman tilin tunnistetiedot.

3.  Voit muuttaa käyttäjäagentti. Voit muuttaa käyttäjäagentti Internet Explorerissa Kehitystyökalut tai muuttaa käyttäjän-agentti Firefoxissa tai Chrome käyttäjäagentti Vaihtajan-laajennuksen avulla.

4.  IP-osoitteen muuttaminen. Voit muuttaa IP-osoitteelle käyttämällä VPN-Tor-lisäosa tai eri tietokeskuksen Azure-uuden tietokoneen määrittäminen.

5.  Kirjaudu sisään käyttämällä samoja tunnistetietoja kuin aiemmin [https://myapps.microsoft.com](https://myapps.microsoft.com) ja muutaman minuutin kuluessa edellisen kirjautumisnimi.

Kirjautuminen-näkyy tunnistetiedot Raporttinäkymät-ikkunan 2 – 4 tunnin kuluessa.<br>
Monimutkainen konepohjaisten oppimistekniikoiden yhdistävää mallit, koska ei mahdollisuutta, se ei Hae noudettu.<br> Voit toistaa nämä vaiheet on useita Azure AD-tilejä.


## <a name="simulating-vulnerabilities"></a>Jotka jäljittelevät heikkouksien 
Heikkouksien ovat heikkouksien Azure AD-ympäristössä, joka voi hyödyntää bad toimija. 3 heikkouksien tyypit ovat tällä hetkellä esiin Azure AD-tunnistetietojen suojaus, joka hyödyntää Azure AD muut toiminnot. Näiden heikkouksien näkyvät tunnistetiedot koontinäytössä automaattisesti, kun nämä ominaisuudet on määritetty.

-   Azure AD [Monimenetelmäisen todentamisen?](../multi-factor-authentication/multi-factor-authentication.md)
-   Azure AD [Cloud App etsiminen](active-directory-cloudappdiscovery-whatis.md).
-   Azure AD [sellaisten tunnistetietojen hallinta](active-directory-privileged-identity-management-configure.md). 



##<a name="user-compromise-risk"></a>Käyttäjän haavoittuvan riski

**Voit testata käyttäjän haavoittuvan riski, toimi seuraavasti**:

1.  Kirjaudu sisään, jolla on yleisen järjestelmänvalvojan oikeudet [https://portal.azure.com](https://portal.azure.com) vuokraajan.

2.  Siirry **tunnistetiedot**. 

3.  Valitse **asetukset**tärkeimmät **Azure AD-tunnistetiedot** -sivu. 

4.  Valitse **Videoportaalin asetukset** -sivu **suojaussäännöt**- **käyttäjän havaittu riski**. 

5.  Valitse **Kirjaudu sisään riski** , sivu käytöstä **Ota sääntö käyttöön** ja valitse sitten **Tallenna** asetukset.

6.  Tietyn käyttäjätilin simuloida tuntemattomia kohtiin tai anonyymi IP riskin tapahtuma. Tämä nostaa kyseisen käyttäjän riskin käyttäjätason **Normaali**.

7.  Odota muutama minuutti ja tarkista sitten, että käyttäjän käyttäjätason on **Normaali**.

8.  Siirry **Videoportaalin asetukset** -sivu.

9.  Valitse **Käyttäjän haavoittuvan riski** -sivu, valitse **Ota sääntö käyttöön**, **Valitse** . 

10. Valitse jokin seuraavista vaihtoehdoista:

    a. Jos haluat estää, valitse **Normaali** -kohdassa **Estä Kirjaudu sisään**.

    b. Jos haluat säilyttää suojattua salasanan vaihtaminen, valitse **Normaali** -kohdassa **monimenetelmäisen todentamisen**.

13. Valitse **Tallenna**.

14. Nyt voit testata riskiin perustuva ehdollisen käyttöoikeuden kirjautumalla sisään käyttämällä käyttäjän järjestelmänvalvojana riskin tasolla. Jos käyttäjä on Normaali, käytäntöjen määritysten mukaan oman kirjautumisnimi on joko estetty tai on pakotettu vaihtamaan salasanasi. 
<br><br>
![Playbook](./media/active-directory-identityprotection-playbook/201.png "Playbook")
<br>

 
##<a name="sign-in-risk"></a>Kirjaudu sisään riski

 
**Voit esikatsella merkki riski, toimi seuraavasti:**

1.  Kirjaudu sisään, jolla on yleisen järjestelmänvalvojan oikeudet [https://portal.azure.com](https://portal.azure.com) vuokraajan.

2.  Siirry **tunnistetiedot**.

3.  Valitse **asetukset**tärkeimmät **Azure AD-tunnistetiedot** -sivu. 

4.  Valitse **Videoportaalin asetukset** -sivu **suojaussäännöt**- **riski kirjautuminen**.

5.  Valitse **Kirjaudu sisään riski **, sivu **-** kohdassa **Ota sääntö käyttöön**. 

7.  Valitse jokin seuraavista vaihtoehdoista:

    a. Jos haluat estää, valitse **Normaali** -kohdassa **Estä kirjautuminen**

    b. Jos haluat säilyttää suojattua salasanan vaihtaminen, valitse **Normaali** -kohdassa **monimenetelmäisen todentamisen**.

8.  Jos haluat estää, valitse Normaali, valitse estä Kirjaudu sisään.

9.  Voit pakottaa monimenetelmäisen todentamisen valitsemalla **Normaali** -kohdassa **monimenetelmäisen todentamisen**.

10. Valitse **Tallenna**.

11. Nyt voit testata riskiin perustuva ehdollisen käyttöoikeuden simuloimalla tuntemattomia sijainnit- tai anonyymi IP riskin tapahtumat, koska ne ovat molemmat **Normaali** riskin tapahtumat.

<br>
![Playbook](./media/active-directory-identityprotection-playbook/200.png "Playbook")
<br>


## <a name="see-also"></a>Katso myös

 - [Azure Active Directory-tunnistetietojen suojaus](active-directory-identityprotection.md)

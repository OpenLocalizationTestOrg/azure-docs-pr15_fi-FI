<properties
    pageTitle="Ehdollisen käyttöoikeuden sovellusten julkaistu Azure AD-sovelluksen välityspalvelimen kanssa"
    description="Tässä artikkelissa käsitellään ehdollinen käyttöoikeuksien määrittäminen julkaiset voi käyttää etäyhteyden Azure AD-sovelluksen välityspalvelimen sovellukset."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/22/2016"
    ms.author="kgremban"/>

# <a name="working-with-conditional-access"></a>Ehdollisen käyttöoikeuden käsitteleminen

Voit määrittää käyttösäännöt sovellukset, jotka on julkaistu sovelluksen välityspalvelimen ehdollinen käyttöoikeus. Tämän avulla voit:

- Vaadi monimenetelmäisen todentamisen sovellusta kohden
- Monimenetelmäisen todentamisen edellyttävät vain silloin, kun käyttäjät eivät ole käytössä
- Estä käyttäjiä käyttämästä sovellus, kun he eivät ole käytössä

Sääntöjen voi suojata kaikille käyttäjille ja ryhmille tai vain tietyt käyttäjät ja ryhmät. Oletusarvon mukaan kaikki käyttäjät, joilla on sovelluksen käyttöä koskevat sääntö. Sääntö voi kuitenkin rajoitettu myös käyttäjille, jotka ovat suojausryhmien jäseniä.  

Käyttösäännöt arvioidaan, kun käyttäjä käyttää liitetyt sovellus, joka käyttää OAuth 2.0, OpenID yhteyden, SAML tai WS Federation. Lisäksi käyttösäännöt arvioidaan OAuth 2.0 ja OpenID yhteyden, kun päivityksen tunnuksen käytetään hankkia access-tunnuksen.

## <a name="conditional-access-prerequisites"></a>Ehdollinen käytön edellytykset

- Azure Active Directory-Premium-tilaus
- Liitetty tai hallittu Azure Active Directory-vuokraajan
- Liitetyt alihallinnat edellyttää, että monimenetelmäisen todentamisen (MFA) on käytössä  
    ![Määritä käyttösäännöt - edellyttävät monimenetelmäisen todentamisen](./media/active-directory-application-proxy-conditional-access/application-proxy-conditional-access.png)

## <a name="configure-per-application-multi-factor-authentication"></a>Sovelluksen kohti monimenetelmäisen todentamisen määrittäminen
1. Kirjaudu sisään järjestelmänvalvojana Azure perinteinen-portaalissa.
2. Siirry Active Directory ja valitse kansio, johon haluat ottaa käyttöön sovelluksen välityspalvelin.
3. Valitse **sovellukset** ja Vieritä alaspäin **Käyttösäännöt** -osaan. Accessin säännöt-osassa näkyy vain julkaistu sovelluksen välityspalvelimen sovellukset, jotka käyttävät liitetyt todennusta.
4. Ota sääntö käyttöön valitsemalla **Ota käyttöön käyttösäännöt** **käyttöön**.
5. Määritä käyttäjät ja ryhmät, jolle koskevat säännöt. Valitse yksi tai useampi ryhmä, johon access-sääntö koskee **Lisää ryhmä** -painikkeella. Tämän valintaikkunan avulla voidaan myös poistaa valitut ryhmät.  Kun säännöt valituista käyttää ryhmiä, access-säännöt vain suoritettaviksi käyttäjät, jotka kuuluvat johonkin määritetyn käyttöoikeusryhmät.  

  - Käyttöoikeusryhmät pois säännön erikseen, tarkista **lukuun ottamatta** ja määritä yksi tai useita ryhmiä. Käyttäjät, jotka ovat Except luettelossa ryhmän jäsenien ei tarvitse suorittaa monimenetelmäisen todentamisen.  

  - Jos käyttäjä on määritetty käyttäjäkohtainen multi-factor authentication-toiminnon avulla, tämä asetus ohittavat sovelluksen monimenetelmäisen todentamisen säännöt. Tämä tarkoittaa, että käyttäjä, joka on määritetty käyttäjäkohtainen monimenetelmäisen todentamisen voidaan suorittaa monimenetelmäisen todentamisen, vaikka ne on tehty poikkeus sovelluksen monimenetelmäisen todentamisen säännöt. Lisätietoja [monimenetelmäisen todentamisen ja käyttäjäkohtaisten asetusten](../multi-factor-authentication/multi-factor-authentication.md).

6. Valitse käyttösäännöt, jonka haluat määrittää:
    - **Vaadi monimenetelmäisen todentamisen**: käyttäjät, joille access säännöt otetaan käyttöön vaaditaan valmis monimenetelmäisen todentamisen ennen, johon tämä sääntö koskee sovelluksen käyttämistä.
    - **Vaadi monimenetelmäisen todentamisen kun ei töissä**: käyttäjä yrittää käyttää sovelluksen luotettu IP-osoite ei edellytetä monimenetelmäisen todentamisen suorittamiseen. Monimenetelmäisen todentamisen asetukset-sivulla voidaan määrittää luotettujen IP-osoitealueet.
    - **Kun ei ole toiminnassa käytön estäminen**: yrittää käyttää yritysverkon ulkopuolelta sovelluksen käyttäjät eivät voi käyttää sovellusta.


## <a name="configuring-mfa-for-federation-services"></a>MFA määrittäminen liittoutumispalvelut.
Liitetyt alihallinnoista, saat monimenetelmäisen todentamisen (MFA) voidaan suorittaa Azure Active Directory tai paikallisen AD FS ‑palvelin. Oletusarvon mukaan MFA ilmenee millä tahansa sivulla Azure Active Directory ylläpitämä. Jotta voit määrittää MFA paikallisen, suorita Windows PowerShellin ja – SupportsMFA-ominaisuuden avulla voit määrittää Azure AD-moduulin.

Seuraavassa esimerkissä esitetään ottamisesta käyttöön paikallisen MFA käyttämällä contoso.com-vuokraajassa [määrittäminen MsolDomainFederationSettings cmdlet-komennon](https://msdn.microsoft.com/library/azure/dn194088.aspx) :`Set-MsolDomainFederationSettings -DomainName contoso.com -SupportsMFA $true `

Määrittämisen tämän merkinnän lisäksi suorittamiseen monimenetelmäisen todentamisen on oltava määritettynä liitetyt vuokraajan AD FS-esiintymä. [Microsoft Azure monimenetelmäisen todentamisen paikalliseen käyttöönottoon](../multi-factor-authentication/multi-factor-authentication-get-started-server.md)ohjeiden mukaisesti.


## <a name="see-also"></a>Katso myös

- [Vaateet huomioon sovellusten käyttäminen](active-directory-application-proxy-claims-aware-apps.md)
- [Sovelluksen välityspalvelimen sovellusten julkaiseminen](active-directory-application-proxy-publish.md)
- [Yksi merkki ottaminen käyttöön](active-directory-application-proxy-sso-using-kcd.md)
- [Valinnaiseksi oman toimialuenimen käyttäminen](active-directory-application-proxy-custom-domains.md)

Uusimmat uutiset ja päivitykset Tutustu [sovelluksen välityspalvelin-blogi](http://blogs.technet.com/b/applicationproxyblog/)

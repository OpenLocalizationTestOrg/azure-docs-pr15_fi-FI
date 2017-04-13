<properties 
    pageTitle="Kertakirjautumisen sovelluksia, jotka eivät ole Azure Active Directory-sovelluksen valikoiman määrittäminen | Microsoft Azure" 
    description="Lisätietoja muodostaa miten itsepalvelu Azure Active Directory, käyttämällä SAML ja salasana-pohjainen SSO sovelluksia" 
    services="active-directory" 
    authors="asmalser-msft"  
    documentationCenter="na" manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="02/09/2016" 
    ms.author="asmalser" />

#<a name="configuring-single-sign-on-to-applications-that-are-not-in-the-azure-active-directory-application-gallery"></a>Kertakirjautumisen sovelluksia, jotka eivät ole Azure Active Directory-sovelluksen valikoiman määrittäminen

Tässä artikkelissa on tietoja ominaisuus, jonka avulla järjestelmänvalvojat voivat määrittää kertakirjautumisen sovelluksia ei ole Azure Active Directory app valikoima *kirjoittamatta koodia*. Tämä toiminto on vapauttaa teknisen ennakkoversion 2015 marraskuun 18th- ja [Azure Active Directory-Premium](active-directory-editions.md)sisältyy. Jos etsit sijaan developer ohjeita siitä, miten voit integroida mukautettujen sovellusten Azure AD-koodin avulla, katso [Todennus tilanteita, joissa Azure AD](active-directory-authentication-scenarios.md).

Azure Active Directory-sovelluksen-valikoimasta löydät sovelluksia, jotka tukevat kertakirjautumisen Azure Active Directory-lomakkeen [Tässä artikkelissa](active-directory-appssoaccess-whatis.md)kuvatulla tavalla tunnetusti luettelo. Kun olet (kuten IT specialist tai järjestelmän integraattori organisaation) löytänyt yhdistettävää sovelluksen, voit aloittaa mukaan noudattamalla vaiheittaisia ohjeita noudattaen esitetään Azure hallinta-portaalissa voit ottaa käyttöön kertakirjautumisen.

Asiakkaat, joilla [Azure Active Directory-Premium](active-directory-editions.md) käyttöoikeuksien saat myös nämä lisäominaisuuksia:

* Omatoiminen integrointi jokin sovellus, joka tukee SAML 2.0-tunnistetietojen toimittajat (SP käynnistämä tai IdP käynnistämä)
* On HTML-pohjaisia kirjautumissivu käyttämällä [salasana-pohjainen SSO](active-directory-appssoaccess-whatis.md#password-based-single-sign-on) web-sovelluksen Omatoiminen integrointi
* Omatoiminen yhteys sovelluksia, jotka käyttävät SCIM protokolla käyttäjän valmistelu ([kuvatut](active-directory-scim-provisioning.md))
* Mahdollisuus lisätä linkkejä [Office 365-sovellusten käynnistimeen](https://blogs.office.com/2014/10/16/organize-office-365-new-app-launcher-2/) tai [Azure AD access-ruudussa](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users) jokin sovellus

Tämä voi sisältää SaaS-sovelluksia, jotka käyttävät mutta on ei vielä ole--nousta Azure AD-sovelluksen valikoimaan lisäksi kolmannen osapuolen web-sovelluksia, jotka organisaatiosi on ottanut käyttöön voidaan hallita cloud tai paikallisen-palvelimiin.

Nämä ominaisuudet, eli *integrointi sovellusmallit*avulla standardien-pohjaisten yhteyspisteet sovellukset, jotka tukevat SAML, SCIM tai Lomakepohjainen todentaminen ja Sisällytä joustava vaihtoehdot ja laaja useiden sovellusten yhteensopivuuden asetukset. 

##<a name="adding-an-unlisted-application"></a>Luettelon ulkopuoliset-sovelluksen lisääminen

Yhteyden integrointi-sovellusmallin-sovellus, kirjaudu sisään Azure hallinta-portaalin Azure Active Directory-järjestelmänvalvojatilin käyttö ja Selaa **Active Directory > [hakemisto] > sovellusten** -osassa **Lisää**ja valitse sitten **Lisää sovellus-valikoimasta**. 

![][1]

Lisää sovellus-valikoimassa luettelon ulkopuoliset sovelluksen **Mukautettu** luokkaa käyttämällä vasemmalla tai valitsemalla **Lisää muu sovellus** linkkiä, joka näkyy hakutuloksissa, jos haluamasi sovellus ei löydy. Kun olet lisännyt sovelluksen nimi, voit määrittää Sign-asetukset ja toimintaa. 

**Vihje**: Paras käytäntö on käyttää hakutoimintoa, tarkista, onko sovellus jo sovelluksen-valikoimassa. Jos sovellus löytyy ja sen kuvaus maininnat "kertakirjautuminen" ja valitse sovelluksen jo tuetaan liitetyt kertakirjautumisen. 

![][2]

Tällä tavalla sovelluksen lisääminen tukee hyvin samankaltaisia kuin valmiiksi integroidut sovellukset käyttöön yhteystietoon. Aloita valitsemalla **Määritä kertakirjautumisen**. Seuraavassa esitellään seuraavista kolmesta vaihtoehdosta määrittämiseen yksittäinen merkki, joka on kuvattu seuraavissa osissa.

![][3]


##<a name="azure-ad-single-sign-on"></a>Azure AD-Single Sign-On

Valitse tämä asetus, jos sovellus SAML-pohjaisen todennuksen määrittäminen. Tämä edellyttää, että ohjelmien tuki SAML 2.0 ja sinulla olisi kerätä tietoja käyttämisestä sovelluksessa ennen jatkamista SAML ominaisuuksia. Kun olet valinnut **Seuraava**, voit pyydetään antamaan vastaavat SAML päätepisteet sovelluksen kolme eri URL-osoitteet. 

![][4]
 
Nämä ovat:

* **Merkki-URL-osoite (SP käynnistämä vain)** – Jos käyttäjä siirtyy sisäänkirjautumisongelmien tämän sovelluksen. Jos sovellus on määritetty suorittamaan palvelun tarjoajan käynnistämä yksittäinen merkki, sitten kun, jos käyttäjä siirtyy tätä URL-Osoitetta, palveluntarjoaja tehdä tarvittavat uudelleenohjauksen haluat Azure AD todennusta ja käyttäjän kirjautuminen. Jos tämä kenttä täytetään, Azure AD käyttäminen Office 365: ssä ja Azure AD Access-paneelin sovellusta käynnistettäessä tätä URL-Osoitetta. Jos tämä kenttä on ommited ja valitse tunnistetietojen toimittaja sen sijaan suorittaa Azure AD-omaan merkki, kun sovellus käynnistetään Office 365-Azure AD Access-paneelin tai Azure AD yksittäinen Sign URL-osoite (copiable koontinäyttö-välilehti).

* **Myöntäjän URL** - URL-Osoitteen tulisi yksilöivät mitä kertakirjauksen hakeminen-myöntäjä määritetään. Tämä on arvo, joka Azure AD lähettää takaisin käyttämisen **yleisön** parametrina SAML-tunnuksen ja sovelluksen odotetaan tarkistamista. Tämä arvo näkyy myös **Kohteen tunnus** -sovelluksen SAML minkä tahansa metatiedot. Tarkista sovelluksen SAML käyttöoppaasta lisätietoja mikä se on kohteen tunnus tai yleisön arvo on. Alla on esimerkki miten yleisön URL-osoite näkyy SAML-tunnuksen, palaa sovelluksen:

```
    <Subject>
    <NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:unspecificed">chad.smith@example.com</NameID>
        <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer" />
      </Subject>
      <Conditions NotBefore="2014-12-19T01:03:14.278Z" NotOnOrAfter="2014-12-19T02:03:14.278Z">
        <AudienceRestriction>
          <Audience>https://tenant.example.com</Audience>
        </AudienceRestriction>
      </Conditions>
```

* **Vastaa URL-osoite** - vastaa URL-osoite on, jossa sovellus odottaa vastaanottamaan SAML-tunnuksen. Tätä kutsutaan myös **vahvistus kuluttaja Service (ACS) URL-osoite**. Valitse sovelluksen SAML käyttöoppaasta SAML suojaustunnuksen vastauksensa tai ACS URL-osoite on lisätietoja.
 Sen jälkeen, kun ne on syötetty, valitse **Seuraava** , siirry seuraavaan ikkunaan. Tämä näyttö tietoja siitä, mitä varten on määritettävä, jotta se voi hyväksyä SAML-tunnuksen Azure AD-sovelluksen reunassa. 

![][5]

Mitkä arvot ovat pakollisia vaihtelevat sen mukaan, sovellus, joten Tarkista sovelluksen SAML ohjeissa. **Kirjaudu sisään** ja **Kirjaudu ulos** palvelun URL-osoite sekä ratkaista saman päätepisteelle, joka on SAML pyynnön käsittelyä päätepisteen oman Azure AD-esiintymän. Myöntäjän URL-osoite on arvo, joka näkyy "Myöntäjä" sisällä SAML-tunnuksen myöntää sovellukselle. 

Kun sovellus on määritetty, valitse **Seuraava** -painiketta ja valitse **Valmis** , sulje valintaikkuna. 

##<a name="assigning-users-and-groups-to-your-saml-application"></a>Käyttäjien ja ryhmien määrittäminen SAML-sovellukseen 

Kun sovellus on määritetty käyttämään Azure AD SAML-pohjaisen tunnistetietojen toimittaja, on melkein valmis kokeilemaan. Azure AD kuin suojaus-ohjausobjekti ei myöntäjän tunnuksen, he voivat kirjautua sovellusta paitsi, jos ne on myönnetty Azure AD-käytön. Käyttäjät voivat myöntää access suoraan tai ryhmä, johon ne on kuuluttava kautta. 

Jos haluat määrittää käyttäjän tai ryhmän sovelluksesi, valitsemalla **Määritä käyttäjät** . Valitse käyttäjän tai ryhmän, jonka haluat määrittää ja valitse sitten **Liitä** -painike. 

![][6]

Käyttäjän määrittäminen Salli Azure AD antaa tunnus käyttäjän sekä aiheuttaa tämän sovelluksen näkyvän käyttäjän Access-ruudussa. Sovelluksen kuvaketta näkyy myös Office 365: n sovellusten käynnistimessä, jos käyttäjä käyttää Office 365: ssä. 

Voit ladata vierekkäin-logo-sovelluksen **määrittäminen** -välilehden **Lataa Logo** -painikkeen avulla sovelluksen. 

###<a name="customizing-the-claims-issued-in-the-saml-token"></a>SAML-tunnuksen myöntää saatavat mukauttaminen 

Kun käyttäjä todentaa sovellukseen, Azure AD antaa SAML-tunnuksen käyttäjästä, joka yksilöi ne sovellus, joka sisältää tiedot (tai saatavat). Oletusarvoisesti tämä sisältää käyttäjän käyttäjänimi, sähköpostiosoitteen, Etunimi ja sukunimi. 

Voit tarkastella tai muokata vaateita, jotka lähetetään SAML-tunnuksen sovelluksen **määritteet** -välilehdessä. 

![][7]

Tähän on kaksi mahdollisia syitä, miksi voit joutua muuttamaan Muokkaa SAML-tunnuksen myöntää saatavat: •alue sovellus on kirjoitettu edellyttävät erilaisia varaa URI tai vaatimus arvot •oma-sovellus on otettu käyttöön niin, että edellyttää, NameIdentifier väittää jokin muu kuin tallennettu Azure Active Directoryn käyttäjänimi (EU täydellinen käyttäjätunnus). 

Lisätietoja siitä, miten voit lisätä ja muokata vaateita, jotka koskevat skenaariot tutustu tämän [artikkelin saatavat mukauttaminen](active-directory-saml-claims-customization.md). 

###<a name="testing-the-saml-application"></a>SAML-sovelluksen testaaminen 

Kun SAML URL-osoitteet ja varmenne on määritetty Azure AD-sovelluksessa, käyttäjät tai ryhmät on myönnetty Azure-sovellukseen ja saatavat olet käynyt läpi ja tarvittaessa muokata ja valitse käyttäjän on valmiina kirjautumaan sovellukseen. 

Testaa, riittää, että merkki-kyselyjä Azure AD käyttää Ohjauspaneelin https://myapps.microsoft.com sovelluksen määritit käyttäjätunnuksella, ja valitse sitten-sovelluksen käyttö Sign-prosessi-ruutu. Vaihtoehtoisesti voit siirtyä suoraan Sign-On URL-osoite sovellus ja kirjaudu sisään sieltä. 

Virheenkorjaus vihjeitä, on tämän [artikkelin virheenkorjaus SAML-pohjaisen single Sign-sovellukset](active-directory-saml-debugging.md) 

##<a name="password-single-sign-on"></a>Salasanan kertakirjautuminen

Valitse tämä vaihtoehto määrittäminen [salasana-pohjainen kertakirjautumisen](active-directory-appssoaccess-whatis.md) web-sovelluksen, jossa on HTML-kirjautumissivulla. Salasana-pohjainen SSO kutsutaan myös kuin vaulting, salasanan avulla voit hallita käyttöoikeuksia ja salasanat web-sovelluksia, jotka eivät tue tunnistetietojen yhdistämisessä. Kannattaa myös käyttää tilanteissa, joissa useiden käyttäjien on jaettava tilistä, kuten organisaation Yhteisöpalvelut app-tilit. 

Kun olet valinnut **Seuraava**, voit pyydetään antamaan sovelluksen verkkopohjaisia kirjautumissivu URL-osoite. Huomaa, että tämä on oltava käyttäjänimi ja salasana syötteen kentät sisältävä sivu. Kun Azure AD aloittaa jäsentää Lisää käyttäjänimi ja salasana, kirjoita kirjautumissivulla prosessi. Jos prosessin ei onnistu, valitse se opastaa Vaihtoehtoinen prosessi, jossa asennuksen selaimen tunniste on (edellyttää Internet Explorerin Chrome ja Firefox), jotta voit siepata manuaalisesti kentät.

Kirjaudu sisään-sivulla, kirjataan, kun käyttäjät ja ryhmät voidaan määrittää ja tunnistetiedon käytännöt voidaan määrittää tavoin säännöllisesti [salasanan SSO-sovellukset](active-directory-appssoaccess-whatis.md).

Huomautus: Voit ladata vierekkäin-logo-sovelluksen **määrittäminen** -välilehden **Lataa Logo** -painikkeen avulla sovelluksen. 

##<a name="existing-single-sign-on"></a>Olemassa olevan kertakirjautuminen

Valitse tämä asetus, jos haluat lisätä linkin lisääminen sovellukselle organisaation Azure AD Access-paneelin tai Office 365-portaaliin. Voit tehdä tämän linkkien lisääminen mukautetut verkko-sovellukset, jotka käyttävät Azure Active Directory Federation Services (tai muu federation-palvelu) sen sijaan, että Azure AD todennusta varten. Vaihtoehtoisesti voit lisätä syvyys linkkejä tiettyjä SharePoint-sivuja tai muiden sivustojen, jotka haluat näkyvän käyttäjän Access-ruutua. 

Kun olet valinnut **Seuraava**, voit pyydetään antamaan URL-osoite linkki-sovelluksen. Kun suoritettu, voi määritetty käyttäjät ja ryhmät-sovellukseen, joka aiheuttaa sovellus näkyy [Office 365-sovellusten käynnistimeen](https://blogs.office.com/2014/10/16/organize-office-365-new-app-launcher-2/) tai [Azure AD access Ohjauspaneelin](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users) kyseisille käyttäjille.

Huomautus: Voit ladata vierekkäin-logo-sovelluksen **määrittäminen** -välilehden **Lataa Logo** -painikkeen avulla sovelluksen.

## <a name="related-articles"></a>Aiheeseen liittyviä artikkeleita

- [Artikkelissa indeksin Azure Active Directory-sovellusten hallinta](active-directory-apps-index.md)
- [Vaateita, jotka on annettu etukäteen integroidut sovellukset SAML tunnus mukauttamisesta](active-directory-saml-claims-customization.md)
- [SAML-pohjaisen kertakirjautumisen vianmääritys](active-directory-saml-debugging.md)

<!--Image references-->
[1]: ./media/active-directory-saas-custom-apps/customapp1.png
[2]: ./media/active-directory-saas-custom-apps/customapp2.png
[3]: ./media/active-directory-saas-custom-apps/customapp3.png
[4]: ./media/active-directory-saas-custom-apps/customapp4.png
[5]: ./media/active-directory-saas-custom-apps/customapp5.png
[6]: ./media/active-directory-saas-custom-apps/customapp6.png
[7]: ./media/active-directory-saas-custom-apps/customapp7.png

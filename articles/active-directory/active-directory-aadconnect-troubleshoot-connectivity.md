<properties
    pageTitle="Azure AD Connect: Tehdä yhteysongelmien vianmääritys | Microsoft Azure"
    description="Tässä artikkelissa kerrotaan, miten Azure AD Connect yhteysongelmien vianmääritys."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/27/2016"
    ms.author="billmath"/>

# <a name="troubleshoot-connectivity-issues-with-azure-ad-connect"></a>Azure AD Connect yhteysongelmien vianmääritys
Tässä artikkelissa kerrotaan, miten Azure AD Connect ja Azure AD välinen yhteys käytetään ja yhteysongelmien vianmääritys. Ongelmaan on todennäköisesti näkyä ympäristössä, jossa välityspalvelinta.

## <a name="troubleshoot-connectivity-issues-in-the-installation-wizard"></a>Ohjatun asennuksen yhteysongelmien vianmääritys
Azure AD Connect käyttää Nykyaikainen todentaminen (ADAL-kirjaston käytön) todennusta varten. Ohjattu asennus ja ERISNIMI sync engine vaativat machine.config on määritetty oikein, koska nämä ovat .NET-sovellukset.

Tässä artikkelissa kerromme, miten Fabrikam muodostaa yhteyden Azure AD sen välityspalvelimen kautta. Välityspalvelimen nimi on fabrikamproxy ja portin 8080 on käytössä.

Ensin annettava Varmista, että [**machine.config**](active-directory-aadconnect-prerequisites.md#connectivity) on määritetty oikein.  
![machineconfig](./media/active-directory-aadconnect-troubleshoot-connectivity/machineconfig.png)

>[AZURE.NOTE]
Joissakin-Microsoft blogit se on kuvattu, että pitäisi tehdä muutoksia miiserver.exe.config sijaan. Tiedosto on korvata jopa ja jos se toimii asennuksen aikana, järjestelmä lakkaa toimimasta ensimmäisen päivitettäessä jokaisen päivityksen. Tästä syystä Suosituksena on päivitettävä machine.config.

Välityspalvelimen on oltava myös avata tarvittavat URL-osoitteet. Virallinen luettelossa on kuvattu [Office 365: n URL-osoitteet ja IP-osoitealueet ](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

Seuraavassa taulukossa on, suora ajoneuvon pienin voivat muodostaa yhteyden Azure AD lainkaan. Tämä luettelo ei ole valinnaisia ominaisuuksia, kuten salasanan takaisinkirjoituksen tai Azure AD yhteyden kunto. Se on kuvattu seuraavassa ohjeessa vianmääritysohjeita alkuperäiset määritykset.

URL-OSOITE | Port (portti) | Kuvaus
---- | ---- | ----
mscrl.microsoft.com | HTTP/80 | Lataa luettelon luetteloiden avulla.
\*. verisign.com | HTTP/80 | Lataa luettelon luetteloiden avulla.
\*. entrust.com | HTTP/80 | Käytettävä luettelon luetteloiden lataaminen MFA-Todentamista varten.
\*. windows.net | HTTPS JA 443 | Kirjaudu sisään Azure AD käytetään.
Secure.aadcdn.microsoftonline p.com | HTTPS JA 443 | Käyttää MFA.
\*. microsoftonline.com | HTTPS JA 443 | Avulla voidaan määrittää Azure AD-kansio ja tuo/vie tiedot.

## <a name="errors-in-the-wizard"></a>Ohjatun toiminnon virheet
Ohjattu asennus on käytössä kaksi eri suojauksen kontekstit. **Yhteyden muodostaminen Azure AD** -sivulla se käyttää tällä hetkellä kirjautuneena käyttäjä. **Määrittäminen** -sivulla se muuttuu [tilin käynnissä palvelu Synkronoi-ohjelma](active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-accounts). Microsoft välityspalvelimen määritykset ovat Yleinen tietokoneeseen, joten jos näkyvissä on ongelma, ongelma on useimmat todennäköisesti näkyvät jo ohjatun **yhteyden muodostaminen Azure AD** -sivulla.

Nämä ovat yleisimmistä virheistä, näyttöön tulee ohjatun asennuksen.

### <a name="the-installation-wizard-has-not-been-correctly-configured"></a>Ohjattu asennus ei ole määritetty oikein
Tämä virhesanoma tulee näyttöön, kun ohjattu toiminto ei löydä välityspalvelin.
![nomachineconfig](./media/active-directory-aadconnect-troubleshoot-connectivity/nomachineconfig.png)

- Jos tämä näkyvissä, varmista [machine.config](active-directory-aadconnect-prerequisites.md#connectivity) on määritetty oikein.
- Jos, joka näkyy oikein, onko ongelman esitä ulkopuolella ohjattu sekä [Tarkista välityspalvelimen yhteys](#verify-proxy-connectivity) ohjeiden mukaisesti.

### <a name="the-mfa-endpoint-cannot-be-reached"></a>MFA-päätepisteen ei voi siirtyä
Tämä virhesanoma tulee näyttöön, jos päätepisteen **https://secure.aadcdn.microsoftonline-p.com** ei voi siirtyä ja Yleinen järjestelmänvalvoja on ottanut käyttöön MFA.  
![nomachineconfig](./media/active-directory-aadconnect-troubleshoot-connectivity/nomicrosoftonlinep.png)

- Jos tämä näkyvissä, varmista, että päätepisteen secure.aadcdn.microsoftonline-p.com on lisätty välityspalvelimeen.

### <a name="the-password-cannot-be-verified"></a>Salasana ei voi vahvistaa
Jos ohjattu asennus on tarkistettu Azure AD-yhteyden muodostamisessa, mutta itse salasanan ei voi vahvistaa tulee näkyviin: ![badpassword](./media/active-directory-aadconnect-troubleshoot-connectivity/badpassword.png)

- On salasana tilapäinen salasana ja on vaihdettava? On todella oikea salasana? Yritä kirjautua https://login.microsoftonline.com (toisessa tietokoneessa kuin Azure AD Connect-palvelin) ja vahvista tili on käytettävissä.

### <a name="verify-proxy-connectivity"></a>Välityspalvelimen yhteyden tarkistaminen
Voit tarkistaa, onko Azure AD Connect palvelimen todellinen välityspalvelimen ja Internet-yhteyksien Käytämme joitakin PowerShell nähdäksesi, jos välityspalvelin on salliminen web pyynnöt vai ei. Suorita PowerShell-kehotteessa `Invoke-WebRequest -Uri https://adminwebservice.microsoftonline.com/ProvisioningService.svc`. (Tarkalleen ottaen ensimmäinen kutsu on https://login.microsoftonline.com ja se toimii myös mutta muiden URI on nopeampaa vastaavan.)

PowerShellin käyttäminen määritykset machine.config välityspalvelimen yhteystiedot. Winhttp/netsh asetukset olisi vaikuta näiden cmdlet-komennot.

Jos välityspalvelimen on määritetty oikein, hanki success tila: ![proxy200](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequest200.png)

Jos näyttöön tulee **ei voi muodostaa yhteyden etäpalvelimeen** sitten PowerShell yrittää soittaa suoraan käyttämättä välityspalvelimen tai DNS ei ole määritetty oikein. Varmista, että **machine.config** -tiedosto on määritetty oikein.
![unabletoconnect](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequestunable.png)

Jos välityspalvelimen ei ole määritetty oikein, emme tulee seuraava virhesanoma: ![proxy200](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequest403.png)
![proxy407](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequest407.png)

Virhe | Virheteksti | Kommentti
---- | ---- | ---- |
403 | Käyttö estetty | Välityspalvelimen ei ole avattu pyydetty URL-osoite. Palata tähän määritykseen välityspalvelimen määritykset ja varmista, että [URL-osoitteet](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2) on avattu.
407 | Välityspalvelimen todennus vaaditaan | Välityspalvelimen pakollinen Kirjaudu sisään ja ei mitään määritettyä. Jos välityspalvelimeen edellyttää todennusta, varmista, että tämä on määritetty machine.config. Varmista myös, että käytät toimialuetilit käyttäjän ohjatun sekä service-tilille.

## <a name="the-communication-pattern-between-azure-ad-connect-and-azure-ad"></a>Azure AD Connect – Azure AD viestintä-kaava
Jos olet tehnyt kaikki nämä vaiheet yllä ja edelleen muodostamasta voit aloittaa katsoo verkossa lokit tässä vaiheessa. Tässä osassa on dokumentoida normaalitilassa ja onnistuneen yhteyden kuvion. Se on myös luettelo yleisiä punainen herrings, jotka voi ohittaa, jos luet verkko-lokit.

- Ole https://dc.services.visualstudio.com kutsuja. Se ei tarvitse avata ne välityspalvelimen asennuksen onnistuu ja sen voi ohittaa ne.
- Näkyviin tulee dns-ratkaisun sisältyviä todellinen isännät DNS nimi tilaa nsatc.net ja muut nimitilan microsoftonline.com-kohdassa ei. Kuitenkin todellinen palvelinten nimet, ei ole ei pyyntöihin web-palvelu ja sinulla ei ole Lisää nämä välityspalvelimeen.
- Päätepisteet adminwebservice ja provisioningapi (Katso alla: lokeista) ovat etsiminen päätepisteet ja käyttää etsimään todellinen päätepiste käyttää, ja vaihtelevan alueen mukaan.

### <a name="reference-proxy-logs"></a>Viittaus välityspalvelimen lokit
Seuraavassa on todellinen välityspalvelimen sisäänkirjautumisen dump ja -asennuksen ohjatun toiminnon sivulla kohtaa, johon se on otettu (sama päätepisteelle kohteiden kaksoiskappaleet on poistettu). Tämä voidaan Viitteenä oman välityspalvelimen ja verkko-lokit. Todellinen päätepisteet voivat olla erilaisia ympäristössäsi (erityisesti kaltaisia *Kursivoitu*).

**Azure AD yhdistäminen**

Aika | URL-OSOITE
--- | ---
1/11/2016 8:31 | Connect://Login.microsoftonline.com:443
1/11/2016 8:31 | Connect://adminwebservice.microsoftonline.com:443
1/11/2016 8:32 | Yhdistäminen: / /*bba800 ankkurin*. microsoftonline.com:443
1/11/2016 8:32 | Connect://Login.microsoftonline.com:443
1/11/2016 8:33 | Connect://provisioningapi.microsoftonline.com:443
1/11/2016 8:33 | Yhdistäminen: / /*bwsc02 välitys*. microsoftonline.com:443

**Asetusten määrittäminen**

Aika | URL-OSOITE
--- | ---
1/11/2016 8:43 | Connect://Login.microsoftonline.com:443
1/11/2016 8:43 | Yhdistäminen: / /*bba800 ankkurin*. microsoftonline.com:443
1/11/2016 8:43 | Connect://Login.microsoftonline.com:443
1/11/2016 8:44 | Connect://adminwebservice.microsoftonline.com:443
1/11/2016 8:44 | Yhdistäminen: / /*bba900 ankkurin*. microsoftonline.com:443
1/11/2016 8:44 | Connect://Login.microsoftonline.com:443
1/11/2016 8:44 | Connect://adminwebservice.microsoftonline.com:443
1/11/2016 8:44 | Yhdistäminen: / /*bba800 ankkurin*. microsoftonline.com:443
1/11/2016 8:44 | Connect://Login.microsoftonline.com:443
1/11/2016 8:46 | Connect://provisioningapi.microsoftonline.com:443
1/11/2016 8:46 | Yhdistäminen: / /*bwsc02 välitys*. microsoftonline.com:443

**Alkusynkronoinnin**

Aika | URL-OSOITE
--- | ---
1/11/2016 8:48 | Connect://Login.Windows.NET:443
1/11/2016 8:49 | Connect://adminwebservice.microsoftonline.com:443
1/11/2016 8:49 | Yhdistäminen: / /*bba900 ankkurin*. microsoftonline.com:443
1/11/2016 8:49 | Yhdistäminen: / /*bba800 ankkurin*. microsoftonline.com:443

## <a name="authentication-errors"></a>Todennus-virheet
Tässä osassa kerrotaan virheistä, jonka funktio voi palauttaa TODENTAMISKIRJASTON (Azure AD Connect käyttämä) ja PowerShell. Kuvaus virheestä avulla voit-ymmärtää seuraavat vaiheet.

### <a name="invalid-grant"></a>Virheellinen myöntäminen
Virheellinen käyttäjänimi tai salasana. Lisätietoja on kohdassa [salasanaa ei voi vahvistaa](#the-password-cannot-be-verified) .

### <a name="unknown-user-type"></a>Tuntematon käyttäjätyyppi
Azure AD-kansio ei löydy tai ratkennut. Yrität ehkä kirjautumisen käyttäjänimi vahvistamaton toimialue kanssa?

### <a name="user-realm-discovery-failed"></a>Käyttäjän alueen etsiminen epäonnistui
Verkon tai välityspalvelimen määritysongelmat. Verkon voi päästä, katso [yhteysongelmien vianmääritys ohjatun asennuksen](#troubleshoot-connectivity-issues-in-the-installation-wizard).

### <a name="user-password-expired"></a>Käyttäjän salasana on vanhentunut
Tunnistetiedot ovat vanhentuneet. Vaihda salasana.

### <a name="authorizationfailure"></a>AuthorizationFailure
Tuntematon.

### <a name="authentication-cancelled"></a>Peruuttaa todentaminen
Monimenetelmäisen todentamisen (MFA)-todennus on peruutettu.

### <a name="connecttomsonline"></a>ConnectToMSOnline
Todennus onnistui, mutta PowerShellin Azure AD on todennus-ongelma.

### <a name="azurerolemissing"></a>AzureRoleMissing
Todennus on onnistunut. Et ole yleinen järjestelmänvalvoja.

### <a name="privilegedidentitymanagement"></a>PrivilegedIdentityManagement
Todennus on onnistunut. Sellaisten jäsenyyksien hallinta on otettu käyttöön ja et tällä hetkellä ole, Yleinen järjestelmänvalvoja. Lisätietoja on kohdassa [Sellaisten tunnistetietojen hallinta](active-directory-privileged-identity-management-getting-started.md) .

### <a name="companyinfounavailable"></a>CompanyInfoUnavailable
Todennus on onnistunut. Yritystiedot ei voitu hakea Azure AD.

### <a name="retrievedomains"></a>RetrieveDomains
Todennus on onnistunut. Toimialuetietoja ei voitu hakea Azure AD.

## <a name="troubleshooting-steps-for-previous-releases"></a>Aiempien versioiden vianmääritysvaiheita.
Kuuluvan alkaen koontiversio 1.1.105.0 (julkaistu helmikuussa 2016) kirjautumisavustaja on poistettu käytöstä. Tässä osassa ja asetusten on oltava ei enää tarvita, mutta on käytettävissä viittauksena.

Single-Kirjaudu sisään avustaja-winhttp on oltava määritettynä. Tämä voi tehdä [**netsh**](active-directory-aadconnect-prerequisites.md#connectivity).  
![netsh](./media/active-directory-aadconnect-troubleshoot-connectivity/netsh.png)

### <a name="the-sign-in-assistant-has-not-been-correctly-configured"></a>Office-avustaja-kirjautuminen ei ole määritetty oikein
Tämä virhe tulevat näkyviin, kun kirjautumisavustaja saavuta välityspalvelimen tai välityspalvelimen ei salli pyynnön.
![nonetsh](./media/active-directory-aadconnect-troubleshoot-connectivity/nonetsh.png)

- Jos tämä näkyvissä, katso [netsh](active-directory-aadconnect-prerequisites.md#connectivity) välityspalvelimen-määritykset ja tarkista ovat oikein.
![netshshow](./media/active-directory-aadconnect-troubleshoot-connectivity/netshshow.png)
- Jos, joka näkyy oikein, onko ongelman esitä ulkopuolella ohjattu sekä [Tarkista välityspalvelimen yhteys](#verify-proxy-connectivity) ohjeiden mukaisesti.

## <a name="next-steps"></a>Seuraavat vaiheet
Lisätietoja [ympäristön integroinnissa paikallisen-käyttäjätietojen Azure Active Directory-hakemistosta](active-directory-aadconnect.md).

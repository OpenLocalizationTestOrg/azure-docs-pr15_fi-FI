<properties 
    pageTitle="Päivittäminen PhoneFactor-agentti Azure multi-factor Authentication-palvelimeen"
    description="Tässä asiakirjassa kuvataan Azure MFA Serverin käytön aloittaminen ja päivittää vanhoja phonefactor-agentti."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="upgrading-the-phonefactor-agent-to-azure-multi-factor-authentication-server"></a>Päivittäminen PhoneFactor-agentti Azure multi-factor Authentication-palvelimeen

Päivittäminen PhoneFactor Agent-v5.x tai vanhempi Azure multi-factor Authentication-palvelimeen edellyttää, että PhoneFactor Agent ja keskuslaitoksen osat on poistettava ennen kuin multi-factor Authentication-palvelin ja sen keskuslaitoksen osat voidaan asentaa.

## <a name="to-upgrade-the-phonefactor-agent-to-azure-multi-factor-authentication-server"></a>Päivitä PhoneFactor-agentti Azure multi-factor Authentication-palvelimeen
<ol>
<li>Varmuuskopioi ensin PhoneFactor-datatiedosto. Asennuksen oletussijainti on C:\Program Files\PhoneFactor\Data\Phonefactor.pfdata.


<li>Jos käyttäjä-portaalissa on asennettu:</li>
<ol>
<li>Siirry kansioon, asenna ja seuraavan koodin korostetut takaisin. Asennuksen oletussijainti on C:\inetpub\wwwroot\PhoneFactor.</li>


<li>Jos olet lisännyt Mukautetut teemat-portaaliin, varmuuskopioi mukautettua kansiotasi C:\inetpub\wwwroot\PhoneFactor\App_Themes kansion alapuolella.</li>


<li>Poista käyttäjä-portaalin joko PhoneFactor agentti (käytettävissä vain, jos tietokoneeseen on asennettu samaan palvelimeen PhoneFactor edustajaksi) tai kautta Windows-ohjelmat ja toiminnot.</li></ol>




<li>Jos Mobile-sovelluksen Web-palvelu on asennettu:
<ol>
<li>Siirry kansioon, asenna ja seuraavan koodin korostetut takaisin. Asennuksen oletussijainti on C:\inetpub\wwwroot\PhoneFactorPhoneAppWebService.</li>
<li>Asennuksen poistaminen Windows Mobile App WWW-palvelun ohjelmat ja toiminnot.</li></ol>

<li>Jos Web-palvelu SDK on asennettu, poista se joko PhoneFactor-agentti tai kautta Windows-ohjelmat ja toiminnot.

<li>Poista ikkunoita PhoneFactor-agentti ohjelmat ja toiminnot.

<li>Asenna multi-factor Authentication-palvelimeen. Huomaa, että asennuspolku on noudettu rekisteristä aiemmasta PhoneFactor agentti asennuksesta, jotta se tulee asentaa samaan sijaintiin (esimerkiksi C:\Program Files\PhoneFactor). Uudet asennukset on oletusarvoisesti asentaa polku (esimerkiksi C:\Program Files\Multi kerroin todennus Server). Vasen mukaan edelliseen PhoneFactor agentti datatiedoston olisi päivitetään asennuksen aikana, joten käyttäjiä ja asetuksia on edelleen oltava asennettuasi uuteen multi-factor Authentication-palvelimeen.

<li>Pyydettäessä Aktivoi multi-factor Authentication-palvelin ja varmista, se on määritetty oikein replikoinnin-ryhmään.

<li>Jos Web-palvelu SDK aiemmin asennettu, Asenna uusi Web-palvelu SDK multi-factor Authentication Server-käyttöliittymän. Huomaa, että näennäiskansio oletusnimi on nyt "MultiFactorAuthWebServiceSdk", "PhoneFactorWebServiceSdk" sijaan. Jos haluat käyttää edellisen nimen, sinun on muutettava nimi näennäiskansio asennuksen aikana. Muussa tapauksessa Jos sallit asennus voi käyttää uuden oletusnimen, sinun on muutettava kaikissa sovelluksissa, jotka viittaavat Web palvelun SDK kuten oikea kohtaan osoittavan käyttäjän portaalin ja Mobile-sovelluksen verkkopalvelun URL-osoite.

<li>Jos käyttäjä-portaalin asennettiin aiemmin PhoneFactor Agent-palvelimeen, Asenna uusi multi-factor Authentication käyttäjän portaali multi-factor Authentication Server-käyttöliittymän. Huomaa, että näennäiskansio oletusnimi on nyt "MultiFactorAuth", "PhoneFactor" sijaan. Jos haluat käyttää edellisen nimen, sinun on muutettava nimi näennäiskansio asennuksen aikana. Jos sallit Asenna uusi oletusarvoista nimeä, voit olisi multi-factor Authentication Server käyttäjän Portal-kuvaketta ja käyttäjän portaalin URL-osoitteen asetukset-välilehdessä.

<li>Jos käyttäjä Portal-ja/tai Mobile-sovelluksen Web-palvelu on aiemmin asennettu eri palvelimessa kuin PhoneFactor-agentti:
<ol>
<li>Siirry Asenna kohteeseen (kuten C:\Program Files\PhoneFactor) ja kopioi haluamasi installer(s) toiseen palvelimeen. 32-bittinen ja 64-bittinen asennusohjelmista käyttäjän Portal- ja Mobile-sovelluksen Web-palvelu on. Niitä kutsutaan MultiFactorAuthenticationUserPortalSetupXX.msi ja MultiFactorAuthenticationMobileAppWebServiceSetupXX.msi tarpeen mukaan.</li>
<li>Käyttäjä-portaalin asennetaan verkkopalvelimeen, Avaa komentorivi järjestelmänvalvojana ja suorita MultiFactorAuthenticationUserPortalSetupXX.msi. Huomaa, että näennäiskansio oletusnimi on nyt "MultiFactorAuth", "PhoneFactor" sijaan. Jos haluat käyttää edellisen nimen, sinun on muutettava nimi näennäiskansio asennuksen aikana. Jos sallit Asenna uusi oletusarvoista nimeä, voit olisi multi-factor Authentication Server käyttäjän Portal-kuvaketta ja käyttäjän portaalin URL-osoitteen asetukset-välilehdessä. Aiemmin luoduille käyttäjille on ilmoitettava uusi URL-osoite.</li>
<li>Siirry käyttäjän portaaliin asennuksen sijainti (esimerkiksi C:\inetpub\wwwroot\MultiFactorAuth) ja Muokkaa seuraavan koodin korostetut. Kopioi arvot appSettings ja applicationSettings osissa, jotka ennen päivitystä on varmuuskopioinut tiedostoon web.config alkuperäisen web.config-tiedostosta. Jos uusi näennäiskansio oletusnimi on WWW-palvelun SDK asennettaessa, URL-Osoitteen muuttaminen applicationSettings-osassa osoittamaan oikeaan paikkaan. Jos muut oletukset on muutettu seuraavan edellisen koodin korostetut, koske seuraavan uuden koodin korostetut samat muutokset.</li>
<li>Asentamiseksi palvelimella Mobile-sovelluksen verkkopalvelun Avaa komentokehote järjestelmänvalvojana ja suorita MultiFactorAuthenticationMobileAppWebServiceSetupXX.msi. Huomaa, että näennäiskansio oletusnimi on nyt "MultiFactorAuthMobileAppWebService", "PhoneFactorPhoneAppWebService" sijaan. Jos haluat käyttää edellisen nimen, sinun on muutettava nimi näennäiskansio asennuksen aikana. Voit halutessasi valita helposti loppukäyttäjät kannettavissa laitteissa Kirjoita lyhyempi nimi. Muussa tapauksessa Jos sallit Asenna uusi oletusarvoista nimeä, voit olisi multi-factor Authentication Server Mobile-sovelluksen kuvakkeen ja päivittää Mobile App WWW-palvelun URL.</li>
<li>Siirry Mobile-sovelluksen Web-palveluun asennuksen sijainti (esimerkiksi C:\inetpub\wwwroot\MultiFactorAuthMobileAppWebService) ja Muokkaa seuraavan koodin korostetut. Kopioi arvot appSettings ja applicationSettings osissa, jotka ennen päivitystä on varmuuskopioinut tiedostoon web.config alkuperäisen web.config-tiedostosta. Jos uusi näennäiskansio oletusnimi on WWW-palvelun SDK asennettaessa, URL-Osoitteen muuttaminen applicationSettings-osassa osoittamaan oikeaan paikkaan. Jos muut oletukset on muutettu seuraavan edellisen koodin korostetut, koske seuraavan uuden koodin korostetut samat muutokset.</li></ol>

<properties
    pageTitle="Azure AD-v2.0 päätepisteen | Microsoft Azure"
    description="Alkuperäinen Azure AD vertailu ja v2.0 päätepisteet."
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>

# <a name="whats-different-about-the-v20-endpoint"></a>Mikä on eri v2.0 päätepisteen tietoja?

Jos osaat Azure Active Directory tai sovellukset on integroitu Azure AD aiemman, voi olla eroja v2.0 päätepisteen, et todennäköisesti odottanut.  Tämän asiakirjan soittaa ulos ymmärrät näiden erot.

> [AZURE.NOTE]
    V2.0 päätepisteen tukee kaikkia Azure Active Directory-skenaariot ja toiminnot.  Voit selvittää, jos sinun on käytettävä v2.0 päätepisteen, lue lisätietoja [v2.0 rajoitukset](active-directory-v2-limitations.md).


## <a name="microsoft-accounts-and-azure-ad-accounts"></a>Microsoft ja Azure AD-tilit
v2.0 päätepisteen Salli kehittäjät voivat kirjoittaa sovellukset, jotka hyväksyvät-kirjautuminen Microsoft Accounts ja Azure AD-tileistä, yhden auth päätepisteen avulla.  Kyseinen rivimäärä on voi kirjoittaa sovelluksen kokonaan tilin ympäristöstä riippumattomalla tavalla; voi olla ignorant tili, jolla käyttäjä kirjautuu sisään tyyppi.  *Voit* tehdä sovelluksen tietoinen käytetään tietyn istunnon tilin tyyppi, mutta sinun ei tarvitse.

Esimerkiksi jos sovelluksen soittaa [Microsoft Graph](https://graph.microsoft.io), jotkin lisätoimintoja ja tiedot ovat käytettävissä enterprise-käyttäjille, kuten niiden SharePoint-sivustoissa tai kansion tiedot.  Mutta monta toimintoja, kuten [käyttäjän viestien lukeminen](https://graph.microsoft.io/docs/api-reference/v1.0/resources/message), koodi voidaan kirjoittaa täsmälleen samalla Microsoft-Accounts ja Azure AD-tileissä.  

Sovelluksen integraation Microsoft Accounts ja Azure AD-tilit on nyt yksi yksinkertaista.  Voit päätepisteet yhteen kirjastoon ja yhden sovelluksen rekisteröinnin yhtenäisiä saat käyttöösi sekä kuluttajille maailman.  Lisätietoja v2.0 päätepisteen, tutustu [yleiskatsaukseen](active-directory-appmodel-v2-overview.md).


## <a name="new-app-registration-portal"></a>Uuden sovelluksen rekisteröinnin portal
v2.0 päätepisteen voi rekisteröidä vain uuteen paikkaan: [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).  Tämä on mistä voit hankkia sovellustunnus-portaalin sinua sovelluksen kirjautumissivulla ja paljon muuta ulkoasua.  Tarvitset portaalin on kytketty Microsoft-tili - henkilökohtainen tai työpaikan tai oppilaitoksen tiliä.  

Jatketaan enemmän toimintoja lisääminen sovelluksen rekisteröinnin portaalin ajan kuluessa.  Tarkoituksena on olevan portaalin uuteen sijaintiin, missä voit avata mitään ja kaikki tarvitse tehdä Microsoft-sovellusten hallinta.


## <a name="one-app-id-for-all-platforms"></a>Yhden sovelluksen tunnus kaikissa ympäristöissä
Alkuperäinen Azure Active Directory-palvelussa saattaa olet rekisteröinyt useita eri sovellusten yhden projektin.  On pakko käyttäminen erillisessä app merkintöjen alkuperäisiä ja verkkosovelluksissa:

![Vanha sovellus rekisteröinnin Käyttöliittymä](../media/active-directory-v2-flows/old_app_registration.PNG)

Esimerkiksi jos olet luonut sivusto- ja iOS-sovellus, oli rekisteröidä erikseen, käyttämällä kahta eri sovellusten tunnukset.  Jos haluat käyttää sivustoa ja verkko-ohjelmointirajapinnan taustassa, voit ehkä rekisteröity kunkin kuin erillisessä app Azure AD.  Jos haluat käyttää iOS-sovelluksen ja Android-sovelluksen, myös ehkä olet rekisteröinyt kaksi eri sovellusten.  

<!-- You may have even registered different apps for each of your build environments - one for dev, one for test, and one for production. -->

Nyt tarvitset on yhden sovelluksen rekisteröinti ja yksittäisen sovellustunnus kunkin projektit.  Voit lisätä useita "ympäristöjen" kunkin projektin ja anna tarvittavat tiedot eri ympäristöissä, voit lisätä.  Voit luoda, niin monta sovellukset, kuten tarpeen mukaan, mutta useimmissa tapauksissa vain yksi sovellustunnus pitäisi olla tarpeen.

<!-- You can also label a particular platform as "production-ready" when it is ready to be published to the outside world, and use that same Application Id safely in your development environments. -->

Microsoftin tavoitteena on, että tämä Lisää yksinkertaistettu sovellusten hallinta ja kehitys kokemus, ja lisää koottuja näyttää yhden projektin, jonka voi olla parissa.


## <a name="scopes-not-resources"></a>Vaikutusalueet, eivät resurssit
Alkuperäinen Azure AD-palvelussa sovellus voi toimia **resurssi**tai tunnusten vastaanottajan.  Resurssi voidaan määrittää **alueiden** tai **oAuth2Permissions** , se tukee useita salliminen asiakkaan tunnusten määrittäminen käyttöalueen resurssille pyytää sovelluksia.  Ota huomioon resurssin esimerkkinä Azure AD-kaavio-Ohjelmointirajapinnan:

- Resurssitunnus- tai `AppID URI`:`https://graph.windows.net/`
- Alueet- tai `OAuth2Permissions`: `Directory.Read`, `Directory.Write`jne.  

Tämä pitää paikkansa v2.0 päätepiste.  Sovelluksen voi silti toimia resurssiksi, käyttöalueen ja tunnistetaan URI-osoite.  Asiakassovellukset edelleen pyytää näiden käyttöalueen käyttöoikeutta.  Jossa asiakastietokone pyytää käyttöoikeudet ovat muuttuneet.  Aiemmin OAuth-2.0 Määritä Azure AD-pyyntö saattaa olla näytti:

```
GET https://login.microsoftonline.com/common/oauth2/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&resource=https%3A%2F%2Fgraph.windows.net%2F
...
```

Jos **resurssin** parametrin mitä resurssin asiakkaan sovellus pyytää lupa.  Azure AD laskettu sovelluksessa staattinen määritysten Azure-portaalissa ja annettujen tunnusten vastaavasti tarvittavista käyttöoikeuksista.  Nyt sama OAuth-2.0 Hyväksy pyyntö alkoholijuomat näyttävät:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read%20https%3A%2F%2Fgraph.windows.net%2Fdirectory.write
...
```

jossa **laajuus** -parametri ilmaisee, mikä resurssi ja käyttöoikeudet sovellus pyytää lupa. Haluttu resurssi on edelleen hyvin esitä pyyntö – se on vain sisältyvistä eristä kunkin alueen parametrien arvot.  Laajuus-parametrin avulla tällä tavalla on enemmän standardien kanssa OAuth 2.0-määrityksen v2.0-päätepisteen avulla ja Tasaa tarkemmin yleisiä alan käytäntöihin kanssa.  Ottaa käyttöön myös sovellusten suorittamiseen [vaiheittainen suostumuksen](#incremental-and-dynamic-consent), joka on kuvattu seuraavassa osassa.

## <a name="incremental-and-dynamic-consent"></a>Vaiheittainen ja dynaaminen lupaa
Sovellusten rekisteröity yleisesti saatavilla Azure AD-palvelun tarvitsee OAuth 2.0 tarvittavat käyttöoikeudet määrittää Azure-portaalissa sovelluksen luonnin aikana:

![Käyttöoikeuksien rekisteröinnin Käyttöliittymä](../media/active-directory-v2-flows/app_reg_permissions.PNG)

Sovelluksen pakollinen on käyttöoikeudet määritetty **nimipalvelimet**.  Tämä salliman olemassa Azure-portaalissa sovelluksen määrityksen ja säilyttää koodin nice ja yksinkertainen, sen tuottamat muutama kehittäjille:

- Sovelluksen oli tietää, kaikki sen koskaan on sovelluksen luonnin aikana käyttöoikeudet.  Käyttöoikeuksien lisääminen ajan kuluessa on vaikeaa.
- Sovelluksen oli tietää kaikki joskus käyttää etuajassa resurssit.  On vaikea luoda sovellukset, jotka voivat käyttää satunnaisia määrää resursseja.
- Sovelluksen oli pyydettävä kaikki sen koskaan on yhteydessä käyttäjän ensimmäinen kirjautuminen käyttöoikeudet.  Joissakin tapauksissa tämä johtanut käyttöoikeudet, joten suositella loppukäyttäjät hyväksymisestä sovelluksen käytön ensimmäinen kirjautuminen pitkissä luettelo.

V2.0 päätepisteen voit määrittää sovelluksen tarvitsee **dynaamisesti**suorituksen aikana sovelluksen Normaali käyttö käyttöoikeudet.  Tällöin voit määrittää sovelluksen tarpeita vaiheissa ajan mukaan lukien niiden käyttöalueiden `scope` todennus-pyynnön parametri:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read%20https%3A%2F%2Fgraph.windows.net%2Fdirectory.write
...
```

Yllä lukemaan Azure AD-käyttäjän kansion tiedot sekä niiden directory tietojen kirjoittaminen sovelluksen pyynnöt-oikeudet.  Jos käyttäjä on hyväksynyt käyttöoikeudet tiettyyn sovelluksen aiemman, riittää, että syöttää tunnistetietoja ja kirjauduttava sovellukseen.  Jos käyttäjä on ei sitoutunut näiden oikeuksien v2.0 päätepisteen Pyydä käyttäjää lupaa kyseiset käyttöoikeudet.  Lisätietoja, lue [käyttöoikeudet, suostumusta,](active-directory-v2-scopes.md)ja alueet.

Sallimisen sovelluksen Pyydä käyttöoikeudet dynaamisesti kautta `scope` parametrin antaa käyttäjän kokemus täydet oikeudet.  Jos haluat, voit valita frontload suostumuksesi kohdata ja pyydä kaikki alkuperäisen luvan-pyynnössä käyttöoikeudet.  Tai jos sovellus edellyttää on runsaasti käyttöoikeudet, voit kerätä käyttäjältä käyttöoikeudet soluissa, kuin ne yrittää käyttää tiettyjä ominaisuuksia sovelluksesi ajan kuluessa.

## <a name="well-known-scopes"></a>Tunnettuja käyttöalueet

#### <a name="offline-access"></a>Offline-tila
v2.0 päätepisteen saattaa tarvita uuden tunnetun käyttöoikeustason käyttö sovellusten - `offline_access` laajuus.  Kaikki sovellukset on pyytää nämä oikeudet, jos he haluavat resursseihin käyttäjän puolesta pitkäaikaisesta ajan kuluessa, vaikka käyttäjä voi olla Käytä aktiivisesti sovelluksen.  `offline_access` Laajuus näkyvät suostumusta valintaikkunat kuin käyttäjälle "Käyttää tietoja offline-tilassa", jossa käyttäjä on hyväksyttävä.  Pyytää `offline_access` käyttöoikeudet kokoustyötilaa vastaanottamaan OAuth 2.0 refresh_tokens v2.0 päätepisteestä koodiin.  Refresh_tokens ovat pitkäikäiset ja uusi OAuth 2.0 access_tokens, voidaan vaihtaa pitkän access.  

Jos sovellus ei pyydä `offline_access` laajuus, se saa refresh_tokens.  Tämä tarkoittaa, että kun lunastaa [OAuth 2.0 luvan koodin työnkulku](active-directory-v2-protocols.md#oauth2-authorization-code-flow)authorization_code, vain saat takaisin access_token kohteesta `/token` päätepiste.  Kyseisen access_token pysyy voimassa lyhyt tietyn ajan (yleensä tunnin), mutta myöhemmin päättyy.  AT, joka ohjaa käyttäjän on ajankohta, sovelluksen takaisin `/authorize` päätepisteen noutaa uusia authorization_code.  Tämä uudelleenohjaus aikana käyttäjä voi tai ei tarvitse syöttää tunnistetietoja uudelleen tai uudelleen suostuu käyttöoikeudet sen mukaan, sovelluksen tyyppi.

Lisätietoja OAuth 2.0, refresh_tokens ja access_tokens, tutustu [v2.0 protokolla viittaus](active-directory-v2-protocols.md).

#### <a name="openid-profile--email"></a>OpenID, profiilin ja sähköposti

Alkuperäinen Azure Active Directory-palvelun yleisin OpenID yhteyden kirjautumisen kulun ole runsaasti tuloksena id_token käyttäjän tietoja.  Id_token saatavat voivat sisältää käyttäjän nimi, ensisijainen käyttäjänimi, sähköpostiosoite, Objektitunnus, ja lisää.

On nyt rajoittaminen tiedot, jotka `openid` laajuus niille sovelluksen käyttöoikeus.  "Openid" alue vain sallia käyttäjien kirjautua sovelluksen ja saada käyttäjän app kielikohtaiset tunniste.  Jos haluat saada henkilökohtaisia tietoja (PII)-sovelluksen käyttäjästä, sovelluksen on Lisäkäyttöoikeuksien pyynnön käyttäjältä.  Olemme ovat esittely kahden uuden käyttöalueen – `email` ja `profile` käyttöalueita –, joita voit tehdä.

`email` Alue on hyvin helppoa – se sallii käyttäjän ensisijaisen sähköpostiosoitteen kautta sovelluksen käyttöoikeus `email` id_token väittää.  `profile` Laajuus niille kaikki muut perustiedot käyttäjästä – henkilön nimi, ensisijainen käyttäjänimi, sovelluksen käyttöoikeus objektin tunnus ja niin edelleen.

Näin voit koodi sovelluksen mahdollisimman vähän paljastaminen tavalla – käyttäjä voi vain Kysy sovellus edellyttää, että voit tehdä sen projektin tiedot.  Lisätietoja näiden käyttöalueen viitata [v2.0 alueen viittaus](active-directory-v2-scopes.md). 

## <a name="token-claims"></a>Suojaustunnuksen saatavat

V2.0 päätepisteen myöntämä tunnusten saatavat eivät ole sama kuin yleisesti saatavilla myöntämä tunnusten päätepisteet Azure AD - sovellukset, uusi palvelu siirtämisestä ei tule olettaa tietyn vaatimus on olemassa id_tokens tai access_tokens.   V2.0 päätepisteen myöntämä tunnusten ovat yhteensopivia OAuth 2.0 ja OpenID yhteyden määritykset, mutta voi seurata eri semantiikkaan liittyvien kuin yleisesti saatavilla Azure AD-palvelun.

Saat lisätietoja v2.0 tunnusten lähettämän yksittäisiä väitteitä Hae [v2.0 suojaustunnuksen viittaus](active-directory-v2-tokens.md).

## <a name="limitations"></a>Rajoitukset
On joitakin rajoituksia pitää mielessä, kun v2.0-kohdan.  Tutustu nähdäksesi, jos jotakin näistä rajoituksista koskevat tietyn skenaarion [v2.0 rajoitukset asiakirjaan](active-directory-v2-limitations.md) .

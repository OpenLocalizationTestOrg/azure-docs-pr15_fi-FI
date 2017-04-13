<properties
   pageTitle="Sovellus, jonka kaikki Azure Active Directory-käyttäjän kirjautumaan muodostaminen | Microsoft Azure"
   description="Step by step ohjeet sovelluksen luominen, jotka voivat kirjautua sisään minkä tahansa Azure Active Directory-vuokraajan, eli usean vuokraajan sovelluksen käyttäjältä."
   services="active-directory"
   documentationCenter=""
   authors="skwan"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/11/2016"
   ms.author="skwan;bryanla"/>

# <a name="how-to-sign-in-any-azure-active-directory-ad-user-using-the-multi-tenant-application-pattern"></a>Usean vuokraajan sovelluksen kaavan Azure Active Directory (AD) kuka tahansa käyttäjä kirjautuminen
Jos palvelun sovelluksena useissa organisaatioissa tarjoa ohjelmisto, voit määrittää Hyväksy Kirjaudu apuohjelmien minkä tahansa Azure AD-vuokraajan-sovelluksen.  Azure AD-eli tekeminen sovelluksen usean alihallintaan.  Minkä tahansa Azure AD-vuokraajan käyttäjät pystyvät kirjautumaan sovellukseen jälkeen suostut niiden tilin käyttäminen sovelluksen.  

Jos sinulla on aiemmin luotua sovellusta, joka on oma tili tai muita cloud muiden toimittajien sisäänkirjautuminen tukee, lisääminen Azure AD-Kirjaudu sisään minkä tahansa vuokraajasta on pelkästään sovellus rekisteröidään, lisäämällä koodissa OAuth2, OpenID yhteyden tai SAML kautta ja sijoittaminen sovellusta Kirjaudu sisään Microsoft-painiketta. Saat lisätietoja sovelluksen mukauttaminen alla olevaa painiketta.

[! [Kirjautuminen painike] [AAD-merkki]][AAD-App-Branding]


Tässä artikkelissa oletetaan, että olet jo aiemmin luominen Azure AD yhtä vuokraajan hakeminen.  Jos et ole, pää takaisin ylös [Sovelluskehittäjän opas kotisivun] [ AAD-Dev-Guide] ja Kokeile jotain Microsoftin nopeasti alkaa!

Neljä vaihetta yksinkertaisen sovelluksen muuntaa Azure AD-vuokraajan usean sovelluksen:

1.  Päivitä sovellus on usean vuokraajan rekisteröinti
2.  Päivitä koodia pyynnöt lähettäminen / Common päätepiste 
3.  Päivitä arvot useita myöntäjä käsitellään koodia
4.  Tietoja käyttäjän ja järjestelmänvalvojan suostumus ja tee tarvittavat muutokset

Katsotaan jokaisen vaiheen yksityiskohtaiset tiedot. Voit myös siirtyä suoraan luetteloon [usean vuokraajan näytteiden][AAD-Samples-MT].

## <a name="update-registration-to-be-multi-tenant"></a>Päivitä on usean vuokraajan rekisteröinti
Oletusarvon mukaan web app/API merkintöjen Azure AD-ovat yhtä vuokraajan.  Voit tehdä rekisteröinti usean vuokraajan etsimällä-sovellus on usean vuokraajan"Vaihda [Azure perinteinen portal] -sovellus rekisteröinti määritys-sivulla[ AZURE-classic-portal] ja miten se"Kyllä".

Huomautus: Ennen sovelluksen voidaan usean vuokraajan, Azure AD edellyttää sovelluksen yksilöiviä sovelluksen tunnus-URI. Sovelluksen tunnus URI on tapa sovelluksen tunnistetaan Protocol (protokolla)-viesteissä.  Yhtä vuokraajan-sovelluksen on riittävät sovelluksen tunnus URI on yksilöivä alihallintaan.  Usean vuokraajan-sovelluksen on oltava yksilöivä, Azure AD avulla voit etsiä sovelluksen kaikki alihallinnat yli.  Yleinen yksilöllisyyden pakotetaan edellyttäen sovelluksen tunnus URI on isäntänimi, joka vastaa Azure AD-vuokraajan vahvistama.  Esimerkiksi jos alihallintaan nimi on contoso.onmicrosoft.com sitten kelvollinen sovelluksen tunnus URI olisi `https://contoso.onmicrosoft.com/myapp`.  Jos alihallintaan oli vahvistama, `contoso.com`, sitten kelvollinen sovelluksen tunnus URI olisi myös `https://contoso.com/myapp`.  Usean vuokraajan sovelluksen määrittäminen ei onnistu, jos sovellus tunnus URI ei toimi tätä mallia.

Native client merkintöjen ovat oletusarvoisesti usean vuokraajan.  Sinun ei tarvitse tehdä mitään, jotta alkuperäisessä asiakkaan sovelluksen rekisteröinnin usean vuokraajan.

## <a name="update-your-code-to-send-requests-to-common"></a>Päivitä pyynnöt lähettäminen/Common koodia
Yhtä vuokraajan sovelluksessa Kirjaudu sisään pyynnöt lähetetään vuokraajan sisäänkirjautuminen päätepiste.   Esimerkiksi contoso.onmicrosoft.com varten päätepisteen on seuraava:

    https://login.microsoftonline.com/contoso.onmicrosoft.com

Lähetettyjen vuokraajan päätepisteen kirjautumaan käyttäjät (tai vieraiden) kyseisen alihallinnan kyseisen alihallinnan sovelluksia.  Usean vuokraajan-sovelluksen kanssa sovellus ei tunnista etukäteen mitä vuokraajan käyttäjä on, joten et voi lähettää pyyntöjä vuokraajan päätepiste.  Sen sijaan pyynnöt lähetetään päätepiste, joka multiplexes kaikki Azure AD-alihallinnat kautta:

    https://login.microsoftonline.com/common

Kun Azure AD saa pyynnön-/ Common päätepisteen, se käyttäjä kirjautuu ja löytää vuoksi mitä vuokraajan käyttäjä on peräisin.  / Yleisiä päätepisteen toimii kaikki Azure AD tukemat todennusprotokollat: OpenID yhteyden, OAuth 2.0, SAML 2.0 ja WS Federation.

Kirjaudu sisään vastauksen jälkeen sovellus sisältää tunnuksen, joka vastaa käyttäjän.  Tunnuksen myöntäjä arvo kertoo sovelluksen mitä vuokraajan käyttäjä on peräisin.  Kun vastauksen palauttaa / Common päätepisteen tunnuksen myöntäjä arvo vastaavat käyttäjän vuokraajan.  On tärkeää muistaa / Common päätepisteen ei ole liitetty ja ei ole myöntäjä, ja se on vain multiplekseri.  Käytettäessä/Common vahvistamiseen tunnusten sovelluksen logiikkaa on päivitettävä, jotta tämä ottaa huomioon. 

Kuten edellä mainittiin, usean vuokraajan sovellusten kannattaa antaa yhtenäinen kirjautuminen myös seuraavat ohjeet mukauttaminen Azure AD-sovelluksen käyttäjät. Saat lisätietoja sovelluksen mukauttaminen alla olevaa painiketta.

[! [Kirjautuminen painike] [AAD-merkki]][AAD-App-Branding]

Voit tarkastella / Common käyttäminen päätepisteen ja koodi-toteutuksen tarkemmin.

## <a name="update-your-code-to-handle-multiple-issuer-values"></a>Päivitä arvot useita myöntäjä käsitellään koodia
Web-sovellusten ja verkko-ohjelmointirajapinnan vastaanottaminen ja vahvista Azure AD tunnusta.  

> [AZURE.NOTE] Kun alkuperäisen asiakassovelluksissa pyytää ja vastaanottaa tunnusten Azure AD-ne tee niin lähettäminen API, johon ne on vahvistettu.  Alkuperäisen sovellukset eivät tarkista tunnusten ja on käsittele niitä peittävä.

Seuraavassa kerrotaan, miten sovelluksen tarkistaa tunnusten se saa Azure AD.  Yhtä vuokraajan sovelluksen tavallisesti vievät päätepisteen-arvo, kuten:

    https://login.microsoftonline.com/contoso.onmicrosoft.com

ja käytä muodostaa metatietojen URL-Osoitetta (Tässä tapauksessa OpenID yhteyden) kuten:

    https://login.microsoftonline.com/contoso.onmicrosoft.com/.well-known/openid-configuration

Voit ladata kaksi kriittinen kappaletta tietoja, joita käytetään vahvistamiseen tunnusten: vuokraajan käyttäjän kirjautuminen näppäimet ja myöntäjä arvo.  Kunkin Azure AD-vuokraajan on yksilöllinen myöntäjä arvon lomakkeen:

    https://sts.windows.net/31537af4-6d77-4bb9-a681-d2394888ea26/

missä GUID-arvon nimeä sopivaa version vuokraajan vuokraajan tunnus.  Jos valitset metatietojen linkin `contoso.onmicrosoft.com`, näet myöntäjä arvoksi asiakirjan.

Kun yhtä vuokraajan sovelluksen tarkistaa tunnistetta, se tarkistaa vastaan allekirjoitetun näppäimet metatietojen asiakirjasta tunnuksen allekirjoituksen ja varmistaa, että tunnuksen myöntäjä arvo vastaa, joka löytynyt metatietojen asiakirja.

Koska / Common päätepisteen ei vastaa vuokraajalle ja myöntäjä, kun tarkastelet metatiedot myöntäjä arvo/yleisiä se on malliin perustuvan URL-osoite, sen sijaan, että todellinen arvo ei ole:

    https://sts.windows.net/{tenantid}/

Tämän vuoksi usean vuokraajan sovelluksen ei voi vahvistaa tunnusten vertaamalla metatietojen kanssa myöntäjä arvo `issuer` tunnuksen arvo.  Usean vuokraajan sovelluksen tarpeisiin logiikan päättää myöntäjä arvot ovat kelvollisia ja jotka eivät ole, perusteella vuokraajan myöntäjä arvo ID-osa.  

Esimerkiksi jos usean vuokraajan-sovelluksen avulla Kirjaudu sisään vain tiettyjä alihallinnat, joka on kirjautunut palvelun, valitse se on kuitattava joko myöntäjä arvo tai `tid` ottaa kyseisen alihallinnan ovat niiden luettelo tilaajille tunnuksen arvo.  Jos usean vuokraajan-sovelluksen henkilöt käsitellään vain ja ei päätösten minkä tahansa access perusteella alihallinnat, valitse se voi ohittaa myöntäjä arvoa kokonaan.

Usean vuokraajan mallit näkyvät [Aiheeseen liittyvät sisältö](#related-content) -osassa on tämän artikkelin lopussa myöntäjä vahvistus on poissa käytöstä kaikki Azure AD-vuokraajan kirjautuminen käyttöön.

Nyt katsotaan käyttäjäkokemuksen käyttäjille, joilla on kirjautunut usean vuokraajan sovellukset.

## <a name="understanding-user-and-admin-consent"></a>Tietoja käyttäjän ja järjestelmänvalvojan hyväksyntä
Käyttäjän kirjautumaan sovellukseen Azure AD-sovellus on edustaa käyttäjän vuokraajan.  Näin voit esimerkiksi käyttää yksilöllinen käytäntöjä, kun sovellus kirjautuminen käyttäjän vuokraajan käyttäjiä organisaation.  Yhtä vuokraajan sovelluksen rekisteröinnin on helppoa. se on käytettävä tapahtuu, kun sovellus rekisteröidään [Azure perinteinen portal][AZURE-classic-portal].

Usean vuokraajan-sovelluksen alkuperäinen rekisteröinti sovelluksen sijaitsevat Azure AD-vuokraajan kehittäjä käytetään.  Kun eri vuokraajasta käyttäjä kirjautuu sisään sovelluksen ensimmäisen kerran, Azure AD pyytää niiden suostumus sovelluksen käyttöoikeuksia.  Jos suostumus, sitten *pääasiallista service* -sovelluksen esitys on luotu käyttäjän vuokraajan ja kirjaudu sisään edelleen. Delegointi luodaan myös kansio, joka tallentaa käyttäjän lupaa sovellukseen. Katso [sovelluksen ja palvelun lyhennys objektit] [ AAD-App-SP-Objects] Lisätietoja sovelluksen sovelluksen ja ServicePrincipal objektit ja kuinka ne liittyvät toisiinsa.

![Suostumus yhden tason-sovellukseen][Consent-Single-Tier] 

Suostumus Tämä ongelma koskee sovelluksen pyytämä käyttöoikeudet.  Azure AD tukee käyttöoikeudet, vain sovellus- ja valtuutetun kahdenlaisia:

- Käyttöoikeustason luominen valtuutetun antaa sovelluksen toimia kirjautuneena käyttäjänä alijoukon mitä käyttäjä voi tehdä.  Esimerkiksi myöntää sovellukselle valtuutetun lukemaan kirjautuneena käyttäjän kalenterin.
- Vain sovelluksen käyttöoikeus myönnetään suoraan sovelluksen tunnistetiedot.  Esimerkiksi myöntää sovellukselle vain sovelluksen lukemaan alihallinnan käyttäjien luettelo, ja se voi tehdä tämän riippumatta siitä, joka on kirjautunut sovelluksen.

Joitakin käyttöoikeuksia voi olla suostunut tavallisen käyttäjän samalla, kun taas joissakin on käytettävä vuokraajan järjestelmänvalvojan lupaa. 

### <a name="admin-consent"></a>Järjestelmänvalvojan suostumus
Vain sovelluksen oikeudet edellyttävät aina vuokraajan järjestelmänvalvojan suostumusta.  Jos sovellus pyytää vain sovelluksen käyttöoikeuksia ja Normaali-käyttäjä yrittää kirjautua sovelluksen, sovelluksen saavat tuleva virhesanoma ilmoittaa käyttäjä ei pysty suostumus.

Tietyt valtuutetun käyttöoikeudet edellyttävät lisäksi vuokraajan järjestelmänvalvojan suostumusta.  Esimerkiksi voi kirjoittaa takaisin Azure AD kirjautunut sisään käyttäjänä edellyttää vuokraajan järjestelmänvalvojan suostumusta.  Vain sovelluksen käyttöoikeuksia, kuten Jos tavallisen käyttäjä yrittää kirjautua sovellus, joka pyytää valtuutetun käyttöoikeuden, joka edellyttää järjestelmänvalvojan lupaa sovelluksen tulla virhesanoma.  Onko käyttöoikeustason luominen edellyttää järjestelmänvalvojan suostumus määritetään kehittäjä, joka resurssille julkaistu, ja se voi olla löydy resurssin ohjeissa.  Linkkejä ohjeaiheisiin kuvaava Azure AD Graph API ja Microsoft Graph-Ohjelmointirajapinnan käytettävissä olevat käyttöoikeudet ovat tämän artikkelin [Liittyvät sisältö](#related-content) -osassa.

Jos sovellus käyttää käyttöoikeudet, jotka edellyttävät järjestelmänvalvojan suostumus, joudut liikkeen ovat esimerkiksi painikkeen tai linkkiä, jossa järjestelmänvalvoja voivat aloittaa toiminnon sovelluksen.  Sovelluksen lähettää tämä toiminto on tavallista OAuth2/OpenID yhteyden luvan pyynnön, mutta, joka sisältää myös pyynnön `prompt=admin_consent` kyselyn kyselymerkkijonon parametrin.  Kun järjestelmänvalvoja on hyväksynyt ja palvelun lyhennys luodaan asiakkaan vuokraaja, pyynnöt myöhemmin sisäänkirjautuminen ei tarvita `prompt=admin_consent` parametria.   Järjestelmänvalvoja on päättänyt tarvittavat käyttöoikeudet ovat oikein, koska vuokraajan muut käyttäjät eivät pyydetään lupaa pisteestä eteenpäin.

`prompt=admin_consent` Parametrin voidaan käyttää myös sovellukset, joissa pyydetään käyttöoikeudet, jotka eivät edellytä järjestelmänvalvojan suostumus, mutta haluat antaa sisäänrakennetun, jossa palveltavan järjestelmänvalvojan "rekisteröi" sovelluksen yhden kerran ja muilta käyttäjiltä pyydetään lupaa lähtien.

Jos sovellus edellyttää järjestelmänvalvojan lupaa, sovelluksen kirjautuu järjestelmänvalvojan mutta `prompt=admin_consent` parametria ei lähetetä, järjestelmänvalvoja voi onnistuneesti suostuu sovelluksen, mutta ne vain niiden käyttäjätilin suostumus.  Tavallinen käyttäjät edelleen eivät voi kirjautua sisään ja sovelluksen suostuu.  Tämä on kätevä, jos haluat antaa palveltava järjestelmänvalvoja voi tutkia sovelluksen ennen kuin sallit muiden käyttäjien access.

Tavallinen käyttäjä suostuu sovellukset voi poistaa käytöstä vuokraajan järjestelmänvalvoja.  Jos tätä ominaisuutta ei ole käytössä, järjestelmänvalvojan suostumus on aina vuokraajan määritettävä sovellus edellyttää.  Jos haluat sovelluksen Testaa tavallisen käyttäjän lupaa käytöstä, löytyvät määritysten Vaihda Azure AD-vuokraajan Tietolähdemääritykset-osasta [Azure perinteinen portal][AZURE-classic-portal].

> [AZURE.NOTE] Jotkin sovellukset haluat sisäänrakennetun, jossa säännöllisesti käyttäjät voivat suostumus aluksi ja myöhemmin sovellus voi liittyä järjestelmänvalvojaan ja Pyydä käyttöoikeudet, jotka edellyttävät järjestelmänvalvojan suostumusta.  Tällä ei voi tehdä tämä yhden sovelluksen rekisteröinnin Azure AD tänään.  Tulevista Azure AD v2 päätepisteen ansiosta sovellukset Pyydä käyttöoikeudet suorituksen sijaan rekisteröinti aikaa, jotka mahdollistavat Tämä skenaario.  Lisätietoja on artikkelissa [Azure AD-sovellusmallia v2 Sovelluskehittäjän opas][AAD-V2-Dev-Guide].

### <a name="consent-and-multi-tier-applications"></a>Suostumus ja monitasoisten sovellukset
Sovellus voi olla useita tasoa, kukin vastaavan omassa rekisteröinti Azure AD.  Alkuperäisen sovellus, jossa verkko-Ohjelmointirajapinnan soittaa tai web-sovelluksen, esimerkiksi soittaa verkko-Ohjelmointirajapinnan.  Kummassakin tapauksessa (alkuperäisen sovelluksen tai web Appia) asiakas pyytää käyttöoikeuksia, Soita resurssi (verkko-Ohjelmointirajapinnan).  Jotta asiakas voi onnistuneesti sitoutunut asiakkaan vuokraajan kyselyjä kaikki resurssit, johon se pyytää käyttöoikeudet on jo olevia asiakkaan vuokraaja.  Jos tämä ehto ei täyty, Azure AD palauttaa virheen, resurssi on lisättävä ensin.

Tämä voi olla ongelma, jos looginen sovelluksen koostuu kahden tai useamman sovelluksen merkintöjä, kuten erillinen asiakas- ja resurssin.  Miten saat resurssin kyselyjä asiakkaan vuokraajan ensimmäisen?  Azure AD kerrotaan tässä tapauksessa, koska asiakas- ja resurssin sitoutunut yhdellä kertaa, jossa käyttäjä näkee pyytäjä asiakkaan ja hyväksymissivulla resurssien käyttöoikeuksien kokonaismäärä.  Jotta tämä ongelma resurssin sovelluksen rekisteröinti on sisällettävä asiakkaan Sovellustunnus kuin `knownClientApplications` -sen sovellusluettelo.  Esimerkki:

    knownClientApplications": ["94da0930-763f-45c7-8d26-04d5938baab2"]

Tämä ominaisuus voi päivittää resurssin ['s sovellusluettelo]kautta[AAD-App-Manifest], ja esitellään monitasoisten alkuperäinen asiakkaan kutsumista verkko-Ohjelmointirajapinnan otoksen [Liittyvät sisältö](#related-content) -osassa on tämän artikkelin lopussa. Seuraavassa kuvassa on esitetty yleiskatsaus suostumusta monitasoisten sovelluksen:

![Suostumus monitasoisten tunnetut asiakas-sovellukseen][Consent-Multi-Tier-Known-Client] 

Vastaavat tapauksen tapahtuu, jos sovellus eri tasojen rekisteröidyt eri alihallinnat.  Esimerkkinä muodostamisen alkuperäisen asiakassovellus, joka kutsuu Office 365: n Exchange Online-Ohjelmointirajapinnan kirjainkokoa.  Kehittää sovellusta ja uudempi alkuperäisen sovelluksen toimimaan asiakkaan vuokraajan native Exchange Online service lyhennys on sijaittava.  Tässä tapauksessa asiakkaalla ostamaan pääasiallista käyttäjän vuokraajan luoda-palvelun Exchange Online.  Kyseessä API muodostama organisaation kuin Microsoft-Ohjelmointirajapinnan kehittäjä on lisäämistapaa asiakkaidensa suostumus niiden sovelluksen asiakkaan vuokraajan, kuten verkkosivulle, joka ohjaa suostumusta käyttäen tässä artikkelissa kuvatulla tavalla.  Kun palvelun lyhennys on luotu alihallintaan, alkuperäisen sovelluksen voi tulla tunnusten Ohjelmointirajapinnan.

Seuraavassa kuvassa on esitetty yleiskatsaus suostumuksen rekisteröity eri alihallinnat monitasoisten sovelluksen:

![Suostumus monitasoisten usean osapuolen-sovellukseen][Consent-Multi-Tier-Multi-Party] 

### <a name="revoking-consent"></a>Suostumus peruuttaminen
Käyttäjien ja järjestelmänvalvojien kumota suostumusta milloin tahansa sovelluksen:

- Käyttäjien kumota pääsyn yksittäisiä sovelluksia poistamalla niiden [Access Ohjauspaneelin sovellukset] [ AAD-Access-Panel] luettelon.
- Järjestelmänvalvojat poistaminen access-sovellukset poistetaan Azure AD [Azure perinteinen portal]Azure AD hallinta-osassa[AZURE-classic-portal].

Jos järjestelmänvalvoja suostuu vuokraajan kaikkien käyttäjien-sovellukseen, käyttäjät ei voi kumota pääsyn yksitellen.  Vain järjestelmänvalvoja voi kumota pääsyn ja vain koko sovellukselle.

### <a name="consent-and-protocol-support"></a>Suostumus ja protokollatuki
Suostumus tuetaan Azure AD kautta OAuth OpenID yhteyden, WS Federation ja SAML protokollat.  SAML ja WS Federation protokollat eivät tue `prompt=admin_consent` parametri, jotta järjestelmänvalvojan suostumuksen on mahdollista OAuth- ja OpenID yhteyden kautta vain.

## <a name="multi-tenant-applications-and-caching-access-tokens"></a>Usean vuokraajan sovellukset ja tallentamisesta välimuistiin Access tunnusten
Usean vuokraajan sovellusten myös käyttää Accessin tunnusten Soita API, jotka on suojattu Azure AD.  Yleinen virhe, kun käyttämällä Active Directory käyttöoikeuksien kirjaston (ADAL) usean vuokraajan-sovelluksen kanssa on pyytää alun perin tunnuksen käyttämällä/Common, käyttäjän saa vastausta ja pyydä, että sama käyttäjä käyttää myös/Common myöhemmin tunnus.  Koska Azure AD vastausta peräisin vuokraajan, ei/yleisiä ADAL välimuistiin olevan peräisin vuokraajan tunnuksen. Seuraavien puhelun/Common access-tunnuksen hakeminen käyttäjän löytymättömien välimuistin merkinnän ja käyttäjää pyydetään kirjautumaan uudelleen.  Puuttuvat välimuistin välttämiseksi Varmista, että seuraavat suosikkiryhmän puhelut jo kirjautunut sisään käyttäjän tekemistä vuokraajan päätepiste.

## <a name="related-content"></a>Aiheeseen liittyvää sisältöä

- [Usean vuokraajan sovelluksen objektit][AAD-Samples-MT]
- [Sovellusten liittyviä ohjeita][AAD-App-Branding]
- [Azure AD-Sovelluskehittäjän opas][AAD-Dev-Guide]
- [Sovelluksen ja palvelun lyhennys objektit][AAD-App-SP-Objects]
- [Azure Active Directory-integraation sovellukset][AAD-Integrating-Apps]
- [Suostumus Framework yleiskatsaus][AAD-Consent-Overview]
- [Microsoft Graph API käyttöoikeuksien käyttöalueet][MSFT-Graph-AAD]
- [Azure AD-kaavion API käyttöoikeuksien käyttöalueet][AAD-Graph-Perm-Scopes]

Auta meitä tarkentaminen ja muodon Microsoftin sisältöä ja antaa palautetta Käytä alla Disqus kommentit-kohtaan.

<!--Reference style links IN USE -->
[AAD-Access-Panel]:  https://myapps.microsoft.com
[AAD-App-Branding]: ./active-directory-branding-guidelines.md
[AAD-App-Manifest]: ./active-directory-application-manifest.md
[AAD-App-SP-Objects]: ./active-directory-application-objects.md
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Consent-Overview]: ./active-directory-integrating-applications.md#overview-of-the-consent-framework
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Graph-Overview]: https://azure.microsoft.com/en-us/documentation/articles/active-directory-graph-api/
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Integrating-Apps]: ./active-directory-integrating-applications.md
[AAD-Samples-MT]: https://azure.microsoft.com/documentation/samples/?service=active-directory&term=multitenant
[AAD-Why-To-Integrate]: ./active-directory-how-to-integrate.md
[AZURE-classic-portal]: https://manage.windowsazure.com
[MSFT-Graph-AAD]: https://graph.microsoft.io/en-us/docs/authorization/permission_scopes

<!--Image references-->
[AAD-Sign-In]: ./media/active-directory-devhowto-multi-tenant-overview/sign-in-with-microsoft-light.png
[Consent-Single-Tier]: ./media/active-directory-devhowto-multi-tenant-overview/consent-flow-single-tier.png
[Consent-Multi-Tier-Known-Client]: ./media/active-directory-devhowto-multi-tenant-overview/consent-flow-multi-tier-known-clients.png
[Consent-Multi-Tier-Multi-Party]: ./media/active-directory-devhowto-multi-tenant-overview/consent-flow-multi-tier-multi-party.png

<!--Reference style links -->
[AAD-App-Manifest]: ./active-directory-application-manifest.md
[AAD-App-SP-Objects]: ./active-directory-application-objects.md
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Integrating-Apps]: ./active-directory-integrating-applications.md
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AAD-Graph-User-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity
[AAD-How-To-Integrate]: ./active-directory-how-to-integrate.md
[AAD-Security-Token-Claims]: ./active-directory-authentication-scenarios/#claims-in-azure-ad-security-tokens
[AAD-Tokens-Claims]: ./active-directory-token-and-claims.md
[AAD-V2-Dev-Guide]: ./active-directory-appmodel-v2-overview.md
[AZURE-classic-portal]: https://manage.windowsazure.com
[Duyshant-Role-Blog]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/
[JWT]: https://tools.ietf.org/html/draft-ietf-oauth-json-web-token-32
[O365-Perm-Ref]: https://msdn.microsoft.com/en-us/office/office365/howto/application-manifest
[OAuth2-Access-Token-Scopes]: https://tools.ietf.org/html/rfc6749#section-3.3
[OAuth2-AuthZ-Code-Grant-Flow]: https://msdn.microsoft.com/library/azure/dn645542.aspx
[OAuth2-AuthZ-Grant-Types]: https://tools.ietf.org/html/rfc6749#section-1.3 
[OAuth2-Client-Types]: https://tools.ietf.org/html/rfc6749#section-2.1
[OAuth2-Role-Def]: https://tools.ietf.org/html/rfc6749#page-6
[OpenIDConnect]: http://openid.net/specs/openid-connect-core-1_0.html
[OpenIDConnect-ID-Token]: http://openid.net/specs/openid-connect-core-1_0.html#IDToken















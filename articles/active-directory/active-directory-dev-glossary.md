<properties
   pageTitle="Azure Active Directory Developer sanasto | Microsoft Azure"
   description="Ehdot tavallisimmat Azure Active Directory developer käsitteitä ja toimintojen luettelo."
   services="active-directory"
   documentationCenter=""
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/31/2016"
   ms.author="bryanla"/>

# <a name="azure-active-directory-developer-glossary"></a>Azure Active Directory-Kehitystyökalut-sanasto
Tässä artikkelissa on joitakin core Azure Active Directory (AD) Kehitystyökalut-käsitteistä, jossa on hyötyä, kun Azure AD-sovellusten kehittämistä tietoa määritykset.

## <a name="access-token"></a>käyttöoikeustietue
[Suojaustunnus](#security-token) tyyppi on [lupa Serverin](#authorization-server)myöntämä ja [asiakassovellus](#client-application) käyttää [suojattu resurssin palvelimen](#resource-server)avulla. Yleensä [JSON Web suojaustunnuksen (JWT)]muodossa[JWT]-tunnuksen embodies luvan asiakkaalle myöntämien [resurssien omistajan](#resource-owner), pyydetty tason käyttöoikeudet. Tunnus sisältää kaikki käytettävissä [saatavat](#claim) jostakin aiheesta, sitä käytetään lomakkeena tunnistetieto määritetyn resurssin käytettäessä asiakassovellus ottaminen käyttöön. Näin voit näyttää asiakkaan tunnukset resurssien omistajan sinun myös.

Access-tunnusten viitataan joskus "Käyttäjän + App" tai "App vain"-mukaan esittää tunnistetiedot. Kun asiakassovellus esimerkiksi käyttää:

- ["Luvan koodi," käyttöoikeuksien myöntäminen](#authorization-grant), peruskäyttäjän todentaa ensin resurssin omistajana delegoiminen todennus käyttämään resurssia asiakkaalle. Asiakkaan todentaa myöhemmin, kun access-tunnuksen hankkiminen. Tunnuksen voi joskus voi nimitystä tarkemmin "Käyttäjän + App"-tunnuksen, sillä se edustaa sekä käyttäjälle, joka on sallittu asiakassovellus ja sovelluksen.
- ["Asiakkaan käyttäjätiedot" käyttöoikeuksien myöntäminen](#authorization-grant), asiakas on yksin todennus toimi ilman resurssi-omistaja-todennusta ja lupaa, jotta tunnuksen voi joskus kutsutaan "Vain App"-tunnuksen.

Katso [Azure AD-tunnuksen viittaus] [ AAD-Tokens-Claims] lisätietoja.

## <a name="application-manifest"></a>sovellusluettelo  
[Azure perinteinen portal]myöntämä ominaisuus[AZURE-classic-portal], joka tuottaa sovelluksen tunnistetietojen määritys päivityksessä liittyvää [sovelluksen] järjestelmä käyttää JSON esittäminen[ AAD-Graph-App-Entity] ja [ServicePrincipal] [ AAD-Graph-Sp-Entity] kohteita. Katso [tietoja Azure Active Directory-sovellusluettelo] [ AAD-App-Manifest] lisätietoja.

## <a name="application-object"></a>Sovellusobjektin  
Kun Rekisteröi ja päivität [Azure perinteinen portal]-sovellus[AZURE-classic-portal]-portaalin Luo/päivitykset sovellusobjektin ja vastaavaan [service Pääobjektin](#service-principal-object) alihallintaan. Sovelluksen objekti, sovellus *määrittää* käyttäjän tunnistetietojen määritys yleisesti (yli kaikki kohtaa, johon se on käyttöoikeus)-vuokraajiin antamisen mallina, josta sen vastaavan palvelun pääasiallista objekteja on *johdetun* käytettäväksi paikallisesti suorituksenaikainen (Valitse tietyn Alihallinta).

Katso [sovelluksen ja palvelun lyhennys objektien] [ AAD-App-SP-Objects] lisätietoja.

## <a name="application-registration"></a>Sovelluksen rekisteröiminen  
Jos haluat sallia sovelluksen integrointi ja delegoida tunnistetietojen ja käyttöoikeushallinta Azure AD-funktioita, se rekisteröitävä Azure AD- [vuokraajan](#tenant). Silloin, kun sovellus rekisteröidään kanssa Azure AD-kirjoitit identity-määritysten sovelluksen, jolloin Azure AD integrointi ja käytä ominaisuuksia, kuten:

- Tehokkaat hallinta, kertakirjautuminen Azure AD-jäsenyyksien hallinta ja [OpenID yhteyden] [ OpenIDConnect] protokolla käyttöönotto
- Se käyttää [suojattu resurssien](#resource-server) [asiakassovellukset](#client-application), kautta Azure AD OAuth 2.0 [luvan Serverin](#authorization-server) käyttöönotto
- [Suostumus framework](#consent) hallintaan asiakkaan suojattuja resursseja, resurssien omistajan luvan perusteella.

Katso [integrointi sovellusten Azure Active Directory-hakemistosta] [ AAD-Integrating-Apps] lisätietoja.

## <a name="authentication"></a>todennus
Act haastava osapuolen oikeutetut tunnistetiedot-tarjota suojaus-lyhennys käyttäjätietojen ja käyttöoikeuksien hallinta käytettävä luomista varten. Aikana [OAuth2 käyttöoikeuksien myöntäminen](#authorization-grant) todennustapa osapuolen on esimerkiksi täyttämällä rooli [resurssin omistaja](#resource-owner) tai [asiakassovellus](#client-application), käyttää myöntäminen mukaan.

## <a name="authorization"></a>todennus
Act todennetut suojauksen pääasiallista oikeuksia tekemään tiettyjä toimenpiteitä. Azure AD-ohjelmoinnin mallissa on kaksi ensisijainen tapauksissa:

- [OAuth2 käyttöoikeuksien myöntäminen](#authorization-grant) työnkulku aikana: kun [resurssien omistajan](#resource-owner) myöntää luvan [asiakassovellus](#client-application), sallimisen asiakkaan resurssien omistajan resursseihin.
- Asiakkaan resurssien käytön aikana: kuin täytäntöön [resurssin palvelimen](#resource-server)avulla [väittää](#claim) arvot esittäminen [käyttöoikeustietue](#access-token) access ohjausobjektin päätösten perustuvan ne.

## <a name="authorization-code"></a>todennus-koodi
Lyhyt olleet "tunnus" osana [asiakassovellus](#client-application) [luvan päätepisteen](#authorization-endpoint)"luvan koodi,"-työnkulku jokin neljä OAuth2 [luvan myöntää](#authorization-grant). Koodin palautetaan vastauksena todennus [resurssien omistajan](#resource-owner), ilmaisee, että resurssien omistajan valtuuttanut todennus käyttämään pyydetyt resurssit-asiakassovellukseen. Osana kulun koodi myöhemmin tuoteavaimella [käyttöoikeustietue](#access-token).

## <a name="authorization-endpoint"></a>luvan päätepiste
Yksi toteutettu [luvan palvelimen](#authorization-server)avulla voit käsitellä [resurssien omistajan](#resource-owner) annat asennuksen OAuth2 luvan [käyttöoikeuksien myöntäminen](#authorization-grant) päätepisteistä myöntää työnkulku. Todennus-myöntäminen työnkulku käyttää, mukaan annettu todellinen myöntäminen voi vaihdella, mukaan lukien [luvan koodi](#authorization-code) tai [suojaustunnus](#security-token).

Katso OAuth2 määrityksen [käyttöoikeuksien myöntäminen tyypit] [ OAuth2-AuthZ-Grant-Types] ja [luvan päätepisteen] [ OAuth2-AuthZ-Endpoint] osien ja [OpenIDConnect määrityksen] [ OpenIDConnect-AuthZ-Endpoint] lisätietoja.

## <a name="authorization-grant"></a>käyttöoikeuksien myöntäminen
Tunnistetietojen, joka vastaa [resurssien omistajan](#resource-owner) [todennus](#authorization) käyttämään suojattuja resursseja, myöntää [asiakassovellus](#client-application). Asiakassovellus käyttää useita [OAuth2 luvan Framework määrittämiä myöntäminen tyyppejä] [ OAuth2-AuthZ-Grant-Types] hankkiminen myöntäminen, sen mukaan, asiakkaan/vaatimukset: "luvan koodin myöntäminen", "asiakkaan käyttäjätiedot myöntää", "implisiittinen myöntäminen" ja "resurssien omistajan tunnistetietoja myöntää". Asiakkaalle tunnistetieto on [käyttöoikeustietue](#access-token)tai on [lupa koodin](#authorization-code) (vaihdetaan myöhemmin access-tunnuksen), käyttöoikeuksien myöntäminen käytetään tyypin mukaan.

## <a name="authorization-server"></a>todennus-palvelin
[OAuth2 luvan Framework]määrittämän[OAuth2-Role-Def], antavien access server tunnukset [asiakkaan](#client-application) onnistuneesti todennustapa [resurssin omistaja](#resource-owner) ja sen luvan jälkeen. [Asiakassovellus](#client-application) toimii suorituksen kautta [luvan](#authorization-endpoint) todennus-palvelimen kanssa ja [Suojaustunnuksen](#token-endpoint) päätepisteet mukaisesti OAuth2 määritetty [luvan myöntää](#authorization-grant).

Azure AD-sovelluksen integrointi, kyseessä lupa rooli Azure AD-sovellusten ja Microsoft service API, kuten [Microsoft Graph-ohjelmointirajapinnan]Azure AD toteuttaa[Microsoft-Graph].

## <a name="claim"></a>Varaa
[Suojaustunnus](#security-token) sisältää saatavat, jotka tarjoavat vahvistukset tietoja yhdestä kohde (esimerkiksi [asiakassovellus](#client-application) tai [resurssien omistajan](#resource-owner)) toiseen kohteeseen (kuten [resurssin server](#resource-server)). Vaateita, jotka ovat nimi-arvoa paria, joka välittää tietoja suojaustunnuksen aiheesta (esimerkiksi suojauksen lyhennys, joka on todennettu [luvan server](#authorization-server)). Esitä annetun tunnuksessa saatavat riippuvat useita muuttujia, kuten tunnuksen, aihe, sovelluksen määritys ja niin edelleen todennetaan tunnistetiedon tyyppi tyyppi.

Katso [Azure AD-tunnuksen viittaus] [ AAD-Tokens-Claims] lisätietoja.

## <a name="client-application"></a>asiakassovellukseen.  
[OAuth2 luvan Framework]määrittämän[OAuth2-Role-Def]-sovellus, joka mahdollistaa suojatun resurssin pyynnöt [resurssien omistajan](#resource-owner)puolesta. Termin "asiakas" ei tarkoita tietyn laitteisto käyttöönoton ominaisuuksia (esimerkiksi onko sovellus suoritetaan palvelimessa, työpöydän tai muissa laitteissa).  

Asiakassovellus pyytää [luvan](#authorization) resurssien omistajan, osallistumaan [OAuth2 käyttöoikeuksien myöntäminen](#authorization-grant) työnkulku ja voivat käyttää ohjelmointirajapinnan/tietoja resurssien omistajan puolesta. OAuth2 luvan Framework [määrittää kahdentyyppisiä asiakkaiden][OAuth2-Client-Types], "luottamuksellista" ja "Julkinen", asiakkaan mahdollisuus säilyttää sen luottamuksellisuus perusteella. Sovellusten voit toteuttaa [web-asiakasohjelma (luottamuksellisia)](#web-client) , joka suoritetaan verkkopalvelimeen, [native client (julkisten)](#native-client) asennetaan laitteeseen tai [Käyttäjän-agentti - pohjaiseen (julkisten)](#user-agent-based-client) joka suoritetaan laitteen selaimessa.

## <a name="consent"></a>suostumus
Prosessi, [resurssien omistajan](#resource-owner) myönnetään lupa [asiakassovellus](#client-application), tietyt [käyttöoikeudet](#permissions) suojatun resurssien käytön resurssien omistajan puolesta. -Asiakkaan käyttöoikeuksien järjestelmänvalvoja tai käyttäjä, sinua pyydetään sallimaan organisaation/yksittäiset niiden tietojen käytön sallia. Huomaa, että [usean vuokraajan](#multi-tenant-application) tilanteessa sovelluksen [palvelun pääasiallista](#service-principal-object) tallennetaan myös consenting käyttäjän vuokraajan.

## <a name="id-token"></a>Tunnus
[OpenID yhteyden] [ OpenIDConnect-ID-Token] [suojaustunnus](#security-token) [luvan palvelimen](#authorization-server) [luvan päätepisteen](#authorization-endpoint), joka sisältää [vaateita, jotka](#claim) liittyvät loppukäyttäjän [resurssien omistajan](#resource-owner)todennusta varten. Access-tunnuksen, kuten tunnuksen tunnusten myös esitetään digitaalisesti allekirjoitettu [JSON Web suojaustunnuksen (JWT)][JWT]. Toisin kuin access-tunnuksen,-ID-tunnuksen vaateita, jotka liittyvät resurssien tarkoituksiin ei käytetä sekä käyttää erityisesti ohjausobjektin.

Katso [Azure AD-tunnuksen viittaus] [ AAD-Tokens-Claims] lisätietoja.

## <a name="multi-tenant-application"></a>usean vuokraajan sovelluksen
Kirjaudu luokan [asiakassovellus](#client-application) , jonka avulla ja käyttäjien [suostumus](#consent) valmisteltu minkä tahansa Azure AD [vuokraajan](#tenant), mukaan lukien alihallinnat muuhun kuin osaan, jossa asiakkaan on rekisteröity. Sen sijaan sovelluksen rekisteröity single-vuokraajan, sallia vain Kirjaudu apuohjelmien käyttäjätilit valmisteltu samassa alihallinnassa yhtä missä sovellus on rekisteröity. [Native client](#native-client) sovellukset ovat oletusarvoisesti usean vuokraajan [web](#web-client) -asiakassovellusten olisi mahdollisuus valita yhden ja usean vuokraajan.

Katso [minkä tahansa usean vuokraajan sovelluksen kaavan Azure AD-käyttäjän kirjautuminen] [ AAD-Multi-Tenant-Overview] lisätietoja.

## <a name="native-client"></a>Native client
Kirjoita [asiakassovellus](#client-application) , johon on asennettu suoraan laitteessa. Koska kaikki koodi suoritetaan laitteessa, pidetään "Julkinen" asiakkaan vuoksi sen ei voi tallentaa tunnistetiedot yksityisesti/luottamuksellisena. Katso [OAuth2 asiakkaan tiedostotyypit ja -profiileista] [ OAuth2-Client-Types] lisätietoja.

## <a name="permissions"></a>käyttöoikeudet
[Asiakassovellus](#client-application) tarkoituksenaan [resurssin palvelimen](#resource-server) ilmoittamalla käyttöoikeuspyynnöt. Kaksi tiedostomalleja ovat käytettävissä:

- "Valtuutetun" käyttöoikeudet, joten valitse [laajuus-pohjainen](#scopes) käyttöoikeuksien pyytäminen valtuutetun kirjautunut sisään- [resurssien omistajan](#resource-owner)lupaa, resurssin suorituksen aikana esitetään muodossa ["scp" saatavat](#claim) asiakkaan [käyttöoikeustietue](#access-token).
- Resurssin suorituksen aikana esitetään "Sovelluksen" käyttöoikeudet, joka pyytää [Roolipohjainen](#roles) käytön asiakassovelluksen tunnistetiedot tai käyttäjätiedot-kohdassa nimellä ["roolit" väittää](#claim) -asiakkaan käyttöoikeustietue.

Ne myös surface aikana [suostumus](#consent) , jossa järjestelmänvalvojan tai omistajan resurssin myöntäminen ja estää niiden vuokraajan resurssien asiakkaan pääsyn mahdollisuuden.

Käyttöoikeuspyynnöt on määritetty "Sovelluksia" / "Määrittäminen" välilehti [Azure perinteinen portal][AZURE-classic-portal], valitse "muiden sovellusten", valitsemalla haluamasi "valtuutetun oikeudet" ja "Sovelluksen käyttöoikeudet" (tämä edellyttää jäsenyyttä yleisen järjestelmänvalvojan roolilla). [Julkinen asiakas](#client-application) ei voi säilyttää tunnistetiedot, koska se vain pyytää valtuutetun käyttöoikeudet [luottamuksellisia asiakas](#client-application) ei pyynnön sekä delegoida ja sovelluksen käyttöoikeudet. Asiakkaan [sovellusobjektin](#application-object) sisältää ilmoitettu käyttöoikeudet [requiredResourceAccess ominaisuuden][AAD-Graph-App-Entity].

## <a name="resource-owner"></a>resurssin omistaja
[OAuth2 luvan Framework]määrittämän[OAuth2-Role-Def], kohteen voi suojatun resurssin käyttöoikeuksien myöntäminen. Resurssin omistaja on henkilö, kun sitä käytetään nimitystä käyttäjälle. Esimerkiksi kun [asiakassovellus](#client-application) haluaa käyttää käyttäjän postilaatikon [Microsoft Graph-Ohjelmointirajapinnan][Microsoft-Graph], se edellyttää postilaatikon resurssien omistajan käyttöoikeuksia.

## <a name="resource-server"></a>resurssin palvelimeen
[OAuth2 luvan Framework]määrittämän[OAuth2-Role-Def], isännät suojattu resurssit, voi hyväksyä ja niihin vastaaminen palvelimen suojattu resurssin pyynnöt [asiakassovellukset](#client-application) , joka esittää, [käyttää tunnuksen](#access-token). Tunnetaan myös nimellä suojatun resurssin palvelimeen- tai resurssien sovelluksen.

Resurssin palvelimen paljastaa ohjelmointirajapinnan ja [alueet](#scopes) ja [roolien](#roles), käyttämällä OAuth 2.0 luvan Framework sen suojatun resurssien käytön. Esimerkkejä, josta pääsee Azure AD-vuokraajan tietojen Azure AD-kaavio-Ohjelmointirajapinnan ja, jotka mahdollistavat tietojen, kuten sähköpostin ja kalenterin käyttäminen Office 365-ohjelmointirajapinnan. Molemmat ovat myös ovat käytettävissä [Microsoft Graph-Ohjelmointirajapinnan]kautta[Microsoft-Graph].  

Asiakassovellus, kuten resurssin sovelluksen tunnistetietojen määritys on muodostettu [rekisteröinnin](#application-registration) kautta Azure AD-vuokraajan, sovellusten ja service Pääobjektin. Jotkin Microsoftin API, kuten Azure AD-kaavio-Ohjelmointirajapinnan on valmiiksi rekisteröity palvelun ansaitun saatavilla kaikissa alihallinnat valmistelun aikana.

## <a name="roles"></a>roolit
Roolien lisäämistapaa [alueet](#scopes), kuten [resurssin palvelimen](#resource-server) sen suojatun resurssien käytön hallintaan. Kahdenlaisia: "käyttäjärooliin" toteuttaa Roolipohjainen käyttöoikeuksien valvonta käyttäjille ja ryhmille, jotka edellyttävät resurssin, kun "sovelluksen"-roolin toteuttaa saman [asiakassovellukset](#client-application) , jotka tarvitsevat käyttöoikeudet.

Roolit ovat resurssille määrittämän merkkijonoja (esimerkiksi "kuluraportin hyväksyjän," "Vain luku-muotoisten", "Directory.ReadWrite.All"), hallitun [Azure perinteinen portal] [ AZURE-classic-portal] kautta resurssin [sovelluksen luettelon](#application-manifest), ja tallennetaan resurssin [appRoles ominaisuuden][AAD-Graph-Sp-Entity]. Azure perinteinen portaalin käytetään myös käyttäjien määrittäminen "käyttäjäroolit" ja "sovelluksen"-roolin käyttää asiakkaan [sovelluksen käyttöoikeudet](#permissions) .

Saat yksityiskohtaiset keskustelu Azure AD Graph API näyttämiä sovelluksen rooleista, [Graph API käyttöoikeuksien käyttöalueet][AAD-Graph-Perm-Scopes]. Katso vaiheittaiset käyttöönoton esimerkiksi [Roolipohjainen käyttöoikeuksien valvonta cloud sovellusten Azure AD-][Duyshant-Role-Blog].

## <a name="scopes"></a>käyttöalueet
Käyttöalueen lisäämistapaa [roolit](#roles), kuten [resurssin palvelimen](#resource-server) sen suojatun resurssien käytön hallintaan. Käyttöalueen käytetään toteuttamisesta [laajuus-pohjainen] [ OAuth2-Access-Token-Scopes] käyttää ohjausobjektin [asiakassovellus](#client-application) , joka on käyttöoikeudet valtuutetun resurssin omistaja.

Käyttöalueen ovat resurssille määrittämän merkkijonoja (esimerkiksi "Mail.Read", "Directory.ReadWrite.All"), hallitun [Azure perinteinen portal] [ AZURE-classic-portal] kautta resurssin [sovelluksen luettelon](#application-manifest), ja tallennetaan resurssin [oauth2Permissions ominaisuuden][AAD-Graph-Sp-Entity]. Azure perinteinen portaalin käytetään myös määrittäminen asiakkaan sovelluksen [valtuutetun käyttöoikeudet](#permissions) käyttämään alueen.

Parhaiden käytäntöjen nimeämiskäytännön mukaisesti, on "resource.operation.constraint"-muodossa. Azure AD Graph API näyttämiä käyttöalueiden yksityiskohtainen keskustelu-kohdassa [Kaavion API käyttöoikeuksien käyttöalueet][AAD-Graph-Perm-Scopes]. Käyttöalueen näyttämiä Office 365-palveluja, on artikkelissa [Office 365: n API käyttöoikeudet viittaus][O365-Perm-Ref].

## <a name="security-token"></a>suojaustunnus
Allekirjoitettu asiakirja, joka sisältää saatavat, kuten OAuth2 tunnuksen tai SAML 2.0 vahvistus. OAuth2- [käyttöoikeuksien myöntäminen](#authorization-grant), [käyttöoikeustietue](#access-token) (OAuth2) ja [Tunnus](OpenID Connect) ovat suojauksen tunnusten tyypit, jotka on toteutettu [JSON Web suojaustunnuksen (JWT)][JWT].

## <a name="service-principal-object"></a>Service Pääobjektin
Kun Rekisteröi ja päivität [Azure perinteinen portal]-sovellus[AZURE-classic-portal]-portaalin Luo/päivitykset [sovellusobjektin](#application-object) ja vastaavan service Pääobjektin alihallintaan. Sovelluksen objekti *määrittää* sovelluksen tunnistetietojen määritys yleisesti (yli kaikki alihallinnoista kohtaa, johon liittyvä sovellus on myönnetty käyttöoikeus), ja josta sen vastaavan palvelun pääasiallista objekteja on käytettäväksi paikallisesti suorituksenaikainen (Valitse tietyn Alihallinta) *johdetun* malleista.

Katso [sovelluksen ja palvelun lyhennys objektien] [ AAD-App-SP-Objects] lisätietoja.

## <a name="sign-in"></a>Kirjaudu sisään
Prosessi, [asiakassovellus](#client-application) aloitetaan käyttäjän todennusta ja sieppaamiseen liittyvät tilan [suojaustunnus](#security-token) ja rajaus sovelluksen istunnon samaan tilaan. Maininta voit sisällyttää käyttäjäprofiilin tietoja, kuten sekä tiedot johdettu suojaustunnuksen saatavat.

Sovelluksen sisään-funktiota käytetään yleensä toteuttamisesta single-kertakirjautumisportaaliin (SSO). Se myös edeltää "rekisteröitymistoiminto"-funktio, käyttäjä voi käyttää (yhteydessä ensimmäisen Kirjaudu sisään)-sovellukseen aloituskohdan nimellä. Rekisteröitymisen funktio käytetään kerää ja poistu Lisää valtion tietyn käyttäjän ja saattaa edellyttää [käyttäjän suostumus](#consent).

## <a name="sign-out"></a>Kirjaudu ulos
Prosessi, jossa yhdistelmävisualisoinnin todentamalla irrottaminen käyttäjän tilan peruskäyttäjän liittyvät [asiakassovellus](#client-application) istunnon aikana [- Kirjautuminen](#sign-in)

## <a name="tenant"></a>vuokraajan
Azure Active directory-esiintymä kutsutaan Azure AD-vuokraajan. Se on monenlaisia ominaisuuksia, kuten:

- integroitujen sovellusten rekisterin palvelun
- Käyttäjätilien ja rekisteröidyt sovellukset todennus
- MUUT päätepisteet tarvitaan, jotta eri protokollia, mukaan lukien OAuth2 ja SAML, mukaan lukien [luvan päätepisteen](#authorization-endpoint), [Suojaustunnuksen päätepiste](#token-endpoint) ja [usean vuokraajan sovellusten](#multi-tenant-application)käyttämä "yhteisen" päätepiste.

Palvelutili liittyy myös Azure AD- tai Office 365-tilauksen tilauksen antamisen käyttäjätietojen ja käyttöoikeuksien hallinnan toimintojen tilauksen valmistelun aikana. Miten [Azure Active Directory-vuokraajan] [ AAD-How-To-Tenant] lisätietoja eri tavoista voit avata vuokraajalle. Katso, [miten Azure tilaukset liittyvät Azure Active Directory] [ AAD-How-Subscriptions-Assoc] lisätietoja tilaukset ja Azure AD-vuokraajan välisen suhteen.

## <a name="token-endpoint"></a>Suojaustunnuksen päätepiste
Yksi toteutettu [luvan palvelimen](#authorization-server) tukemaan OAuth2 [luvan myöntää](#authorization-grant)päätepisteet. Myönnä, mukaan sen avulla voidaan hankkia [käyttöoikeustietue](#access-token) (ja Aiheeseen liittyvät "päivitys"-tunnuksen) [asiakas](#client-application)tai [tunnus](#ID-token) käytettäessä [OpenID yhteyden] [ OpenIDConnect] protokolla.

## <a name="user-agent-based-client"></a>Käyttäjä-agentti-pohjaiseen
Kirjoita [asiakassovellus](#client-application) , lataa koodin verkkopalvelimen ja suorittaa sisällä käyttäjäagentti (esimerkiksi selain) kuten yhden sivun sovelluksen (SPA). Koska kaikki koodi suoritetaan laitteessa, pidetään "Julkinen" asiakkaan vuoksi sen ei voi tallentaa tunnistetiedot yksityisesti/luottamuksellisena. Katso [OAuth2 asiakkaan tiedostotyypit ja -profiileista] [ OAuth2-Client-Types] lisätietoja.

## <a name="user-principal"></a>käyttäjän täydellinen
Samalla tavalla kuin service Pääobjektin käytetään esittämään sovelluksen esiintymä-käyttäjä Pääobjektin on eri suojauksen pääasiallista, joka vastaa käyttäjän. Azure AD Graph [käyttäjän kohteen] [ AAD-Graph-User-Entity] määrittää rakenteen käyttäjäobjektin, kuten esimerkiksi Etunimi ja Sukunimi, käyttäjätunnus, kansion roolien jäsenyyden käyttäjiin liittyvät ominaisuudet. Tämä on Azure AD muodostaa käyttäjän-täydellinen suorituksen aikana tunnistetietojen Käyttäjäasetukset. Käyttäjän täydellinen käytetään esittämään todennetun käyttäjän, kertakirjautuminen, nauhoittaminen [suostumus](#consent) delegointi, puheluiden access ohjausobjektin päätöksiä jne.

## <a name="web-client"></a>Web-asiakasohjelma
Kirjoita [asiakassovellus](#client-application) , joka suorittaa kaikki koodin verkkopalvelimeen, ja pysty toimimaan "luottamuksellista" asiakkaaksi sen tunnistetietojen tallentaminen turvallisesti palvelimessa. Katso [OAuth2 asiakkaan tiedostotyypit ja -profiileista] [ OAuth2-Client-Types] lisätietoja.

## <a name="next-steps"></a>Seuraavat vaiheet
[Azure AD Sovelluskehittäjän opas] [ AAD-Dev-Guide] on käytettävä kaikki Azure AD-kehitys portaali Aiheeseen liittyviä ohjeita, mukaan lukien yleiskatsaus [sovelluksen integrointi] [ AAD-How-To-Integrate] ja [Azure AD-todennusta]ja tuettujen todennus skenaariot[AAD-Auth-Scenarios].

Auta meitä tarkentaminen ja muodon Microsoftin sisältöä ja antaa palautetta Käytä Disqus kommentit-kohtaan.

<!--Image references-->

<!--Reference style links -->
[AAD-App-Manifest]: ./active-directory-application-manifest.md
[AAD-App-SP-Objects]: ./active-directory-application-objects.md
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AAD-Graph-User-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity
[AAD-How-Subscriptions-Assoc]: ./active-directory-how-subscriptions-associated-directory.md
[AAD-How-To-Integrate]: ./active-directory-how-to-integrate.md
[AAD-How-To-Tenant]: active-directory-howto-tenant.md
[AAD-Integrating-Apps]: ./active-directory-integrating-applications.md
[AAD-Multi-Tenant-Overview]: active-directory-devhowto-multi-tenant-overview.md
[AAD-Security-Token-Claims]: ./active-directory-authentication-scenarios/#claims-in-azure-ad-security-tokens
[AAD-Tokens-Claims]: ./active-directory-token-and-claims.md
[AZURE-classic-portal]: https://manage.windowsazure.com
[Duyshant-Role-Blog]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/
[JWT]: https://tools.ietf.org/html/draft-ietf-oauth-json-web-token-32
[Microsoft-Graph]: https://graph.microsoft.io
[O365-Perm-Ref]: https://msdn.microsoft.com/en-us/office/office365/howto/application-manifest
[OAuth2-Access-Token-Scopes]: https://tools.ietf.org/html/rfc6749#section-3.3
[OAuth2-AuthZ-Endpoint]: https://tools.ietf.org/html/rfc6749#section-3.1
[OAuth2-AuthZ-Grant-Types]: https://tools.ietf.org/html/rfc6749#section-1.3
[OAuth2-Client-Types]: https://tools.ietf.org/html/rfc6749#section-2.1
[OAuth2-Role-Def]: https://tools.ietf.org/html/rfc6749#page-6
[OpenIDConnect]: http://openid.net/specs/openid-connect-core-1_0.html
[OpenIDConnect-AuthZ-Endpoint]: http://openid.net/specs/openid-connect-core-1_0.html#AuthorizationEndpoint
[OpenIDConnect-ID-Token]: http://openid.net/specs/openid-connect-core-1_0.html#IDToken

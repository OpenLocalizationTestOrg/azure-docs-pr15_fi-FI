<properties
    pageTitle="v2.0 päätepisteen rajoitukset ja rajoituksia | Microsoft Azure"
    description="Luettelo rajoitukset ja Azure AD v2.0 päätepisteissä rajoitukset."
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
    ms.date="09/30/2016"
    ms.author="dastrock"/>

# <a name="should-i-use-the-v20-endpoint"></a>Milloin kannattaa käyttää v2.0 päätepisteen?

Integroi Azure Active Directory sovellusten kehittämiseen tarvitset päättää, onko v2.0 päätepisteen ja todennus protokollat vastaa tarpeitasi.  Alkuperäinen Azure AD-päätepiste on edelleen täysin tuettu ja jossakin on enemmän kuin v2.0 ominaisuus RTF.  Kuitenkin v2.0 päätepisteen [esittelee merkittäviä etuja](active-directory-v2-compare.md) kehittäjille, houkuttelemaan voit käyttää ohjelmoinnin uusi malli.

Tässä vaiheessa kohdassa kerralla Tutustu suositus käyttökerralla v2.0 päätepisteen on seuraavanlainen:

- Voit halutessasi henkilökohtainen Microsoft-tilien tukea sovelluksen käytettävä v2.0 päätepiste.  Mutta muista rajoituksista, jotka on lueteltu tämän artikkelin varsinkin liittyvät erityisesti toimi & oppilaitoksen tilit.
- Jos sovellus edellyttää vain tukevan työ- ja oppilaitostileiltä, kannattaa käyttää [alkuperäisen Azure AD-päätepisteet](active-directory-developers-guide.md).

V2.0 päätepisteen kasvaa ajan myötä poistamiseksi mainittu tässä, rajoitukset, niin, että sinun on vain joskus käyttää v2.0 päätepistettä.  Tämä artikkeli on tarkoitettu-avulla voit määrittää, jos v2.0 päätepiste on puolestasi.  Olemme säilyvät päivittää tämän artikkelin ajan kuluessa v2.0 päätepisteen tilaa, joten käy tarkistamassa takaisin uudelleen tarpeen mukaan v2.0-ominaisuuksia.

Jos sinulla on aiemmin luotu-sovelluksessa, jossa Azure AD, joka ei käytä v2.0 päätepisteen, ei tarvitse alusta.  Myöhemmin emme tarjota tavalla voit aiemmin Azure AD-sovellusten v2.0 päätepisteen kanssa käytettäväksi.

## <a name="restrictions-on-apps"></a>Sovellusten rajoitukset
Sovellusten seuraavanlaisia ei tällä hetkellä tue v2.0 päätepiste.  Tuetut tyyppisiä sovelluksia kuvauksen lisätietoja [Tässä artikkelissa](active-directory-v2-flows.md).

##### <a name="standalone-web-apis"></a>Erillinen verkko-ohjelmointirajapinnan
V2.0 päätepisteissä sinulla voi [muodostaa verkko-Ohjelmointirajapinnan, joka on suojattu käyttäen OAuth 2.0](active-directory-v2-flows.md#web-apis).  Kuitenkin, että verkko-Ohjelmointirajapinnan vain voi vastaanottaa tunnusten sovellus, joka jakaa sama sovelluksen.  Verkko-Ohjelmointirajapinnan, jota käytetään asiakkaan kanssa eri sovellustunnus rakentaminen ei tueta.  Asiakkaalle ei voi pyytää tai hankkimaan käyttöoikeuksia verkko-Ohjelmointirajapinnan.

Verkko-Ohjelmointirajapinnan, joka hyväksyy tunnusten-asiakasohjelmaa ja sama sovellustunnus muodostaminen on ohjeartikkelissa v2.0 päätepisteen verkko-Ohjelmointirajapinnan mallit [aloittaminen](active-directory-appmodel-v2-overview.md#getting-started).

##### <a name="web-api-on-behalf-of-flow"></a>Web API--puolesta-ja työnkulku
Monta arkkitehtuureihin Lisää verkko-Ohjelmointirajapinnan on Soita toiseen edeltävät Web API, sekä suojattu v2.0 päätepiste.  Tässä skenaariossa yhteistä alkuperäisen asiakkaat, joilla verkko-Ohjelmointirajapinnan taustassa, joka puolestaan kutsuu Microsoft online-palvelun tai toinen mukautettu sisäisten verkko-Ohjelmointirajapinnan, joka tukee Azure AD.

Tässä skenaariossa voit tukea OAuth 2.0 Jwt haltijan tunnistetiedon myönnä, eli On-Behalf-Of Flow.  Kuitenkin käytössä-puolesta-ja työnkulku ei tue tällä hetkellä v2.0 päätepisteelle.  Jos haluat nähdä, miten tämä työnkulku toimii yleisesti saatavilla Azure AD-palvelua, tutustu [koodi--puolesta-, malli-GitHub](https://github.com/AzureADSamples/WebAPI-OnBehalfOf-DotNet).

## <a name="restrictions-on-app-registrations"></a>Sovelluksen merkintöjen rajoitukset
Tässä vaiheessa kohdassa kerralla kaikki sovellukset, jotka haluat integroida v2.0 päätepisteen on luoda uusi sovellus rekisteröinti [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).  Kaikki olemassa olevat Azure AD tai Microsoft-Account sovellukset eivät ole yhteensopivia v2.0 päätepisteen eikä on rekisteröity minkä tahansa portaalissa lisäksi uuden sovelluksen rekisteröinnin portaalin sovellukset.  Olemme aiot lisäämistapaa aiemmin sovellusten v2.0-sovelluksen käyttöä varten. Tällä hetkellä, ei ole siirtymispolku sovelluksen v2.0 päätepisteelle.

Vastaavasti sovellusten rekisteröity uuden sovelluksen rekisteröinnin portaalissa ei toimi vastaan alkuperäisen Azure AD-todennus päätepiste.  Voit integroida onnistuneesti Microsoft tilin todennus päätepisteen kuitenkin sovellukset on luotu sovelluksen rekisteröinti-portaalissa avulla `https://login.live.com`.

Lisäksi app merkintöjen [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) luotu on seuraavalla tavalla:

- **Aloitussivu** -ominaisuutta kutsutaan myös **Sign URL** ei tueta.  Ilman kotisivulle nämä sovellukset ei näytetä Office Omat sovellukset-ruudussa.
- Vain kaksi sovelluksen tietoja sallitaan sovelluksen tunnus tällä hetkellä kohden.
- Sovelluksen rekisteröinti vain voi tarkastella ja hallitsee yksittäisen Kehitystyökalut-tili.  Se ei voi jakaa useita kehittäjät välillä.
- Ei sallittu redirect_uri muoto useita rajoituksia.  Katso lisätietoja seuraavassa osassa.

## <a name="restrictions-on-redirect-uris"></a>Redirect URI rajoitukset
Sovellukset, jotka on rekisteröity uuden sovelluksen rekisteröinnin portaalissa on rajoitettu redirect_uri rajoitettu arvojoukon.  Web Apps-sovellusten ja palveluiden redirect_uri on alettava mallin `https`, ja kaikki redirect_uri arvot täytyy jakaa yhden DNS-toimialue.  Esimerkiksi ei ole mahdollista Rekisteröi web-sovellusta, joka on redirect_uris:

`https://login-east.contoso.com`  
`https://login-west.contoso.com`

Rekisteröintijärjestelmän kehittäminen vertaa koko DNS-nimeä aiemmin redirect_uri lisättävästä redirect_uri DNS-niminen. Lisää DNS-nimi pyynnön epäonnistuu, jos jompikumpi seuraavista ehdoista täyttyy:  

- Jos uusi redirect_uri koko DNS-nimi ei täsmää aiemmin redirect_uri DNS-nimi
- Jos uusi redirect_uri koko DNS-nimeä ei ole alitoimialuetta aiemmin redirect_uri

Jos sovellus on tällä hetkellä redirect_uri esimerkiksi:

`https://login.contoso.com`

Valitse on mahdollista lisätä:

`https://login.contoso.com/new`

joka vastaa DNS-nimi tai:

`https://new.login.contoso.com`

Mikä on login.contoso.com alitoimialuetta DNS.  Jos haluat lisätä sovelluksen, joka sisältää kirjautuminen east.contoso.com ja kirjautuminen-west.contoso.com kuin redirect_uris, sinun on lisättävä seuraavat redirect_uris järjestyksessä:

`https://contoso.com`  
`https://login-east.contoso.com`  
`https://login-west.contoso.com`  

Kahteen voi lisätä, koska ne ovat ensimmäisen redirect_uri alitoimialueita contoso.com. Tämä rajoitus poistetaan tulevista versiossa.

Lisätietoja sovelluksen Rekisteröi uusi sovellus rekisteröinti-portaalissa on viittaa [tämän artikkelin](active-directory-v2-app-registration.md).

## <a name="restrictions-on-services--apis"></a>Palvelut ja ohjelmointirajapinnan rajoitukset
V2.0 päätepisteen tällä hetkellä tukee-kirjautuminen minkä tahansa sovelluksen rekisteröity uuden sovelluksen rekisteröinti-portaalissa, jos se kuuluu [tuettu todennus kulkee](active-directory-v2-flows.md)luetteloksi.  Kuitenkin nämä sovellukset vain saa oikeuden hankkia OAuth 2.0 access tunnusten resursseja on hyvin vähän joukko.  V2.0 päätepisteen vain antaa access_tokens varten:

- Sovellus, joka pyytää tunnuksen.  Sovelluksen voi hankkia access_token itselleen, jos looginen sovellus koostuu useista eri osia tai tasoa.  Tässä skenaariossa käytössä näkyviin Tutustu Microsoftin [Käytön aloittaminen](active-directory-appmodel-v2-overview.md#getting-started) -opetusohjelmat.
- Outlookin sähköpostin, kalenterin ja yhteystietojen REST API, mikä on lisätietoja https://outlook.office.com.  Saat viestin kirjoittaminen sovellus, joka käyttää näitä API viitata [Office aloittaminen](https://www.msdn.com/office/office365/howto/authenticate-Office-365-APIs-using-v2) näissä Opetusohjelmissa.
- Microsoft Graph-ohjelmointirajapinnan.  Lisätietoja Microsoft Graph ja kaikki tiedot, jotka ovat käytettävissä, käy [https://graph.microsoft.io](https://graph.microsoft.io).

Muiden palvelujen tuetaan tällä hetkellä.  Lisää Microsoft Online services lisätään myöhemmin, sekä omia mukautettuja valmiita verkko-ohjelmointirajapinnan ja palveluiden tuki.

## <a name="restrictions-on-libraries--sdks"></a>Kirjastot ja SDK: T rajoitukset
Kirjaston v2.0 päätepisteen tuki on melko tällä hetkellä.  Jos haluat käyttää v2.0 päätepisteen tuotannon-sovelluksessa, sinulla on seuraavat vaihtoehdot:

- Jos olet muodostamassa web-sovelluksen, turvallisesti voit Tutustu yleisesti saatavilla palvelinpuolen middleware suorittamiseen Kirjaudu sisään ja tunnussanoma vahvistus.  Näitä ovat OWIN Avaa tunnus-yhteyden middleware ASP.NET ja tutustu NodeJS Passport laajennuksen.  Käyttämällä Microsoftin middleware MALLIKOODEJA ovat käytettävissä Microsoftin [Aloittaminen](active-directory-appmodel-v2-overview.md#getting-started) osassa paikan päällä.
- Muiden ympäristöjen ja alkuperäisen ja mobile-sovellukset voit myös integroida v2.0 päätepisteissä suoraan lähettäminen ja vastaanottaminen protokolla viestien sovelluksen koodissa mukaan.  V2.0, OpenID yhteyden ja OAuth protokollat [erikseen kuvattu](active-directory-v2-protocols.md) avulla voit tehdä esimerkiksi integrointi.
- Voit lopuksi v2.0 päätepisteen integroida Avaa lähde Avaa tunnus yhteyden ja OAuth-kirjastoista.  V2.0-protokollan on oltava yhteensopiva monta Avaa lähde protokolla kirjastojen ilman suurimpiin muutoksiin.  Näiden kirjastojen käytettävyys vaihtelee kohti kieli- ja ympäristö ja [Avaa tunnus yhteyden](http://openid.net/connect/) ja [OAuth 2.0](http://oauth.net/2/) -sivustojen ylläpitää Suositut käyttöotot luettelo. Katso lisätietoja sekä luettelo Avaa lähde asiakkaan kirjastoihin ja esimerkkejä, joiden on testattu v2.0 päätepisteen [Azure Active Directory (AD) v2.0 ja todennusta kirjastojen](active-directory-v2-libraries.md) .

Microsoft on myös julkaissut [Microsoft todennus kirjaston (MSAL)](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) ensimmäisen esikatselukuvan .NET vain.  Olet Tervetuloa Kokeile tämän kirjaston .NET asiakas- ja sovellusten, mutta se ei ole liitettävä GA laadukkaita esikatselu kirjaston kuin tue.

## <a name="restrictions-on-protocols"></a>Protokollat rajoitukset
Avaa tunnus Yhdistä & OAuth 2.0 tukee vain v2.0 päätepiste.  Mutta ei kaikkia ominaisuuksia ja kunkin protokollan ominaisuudet on lisätty v2.0 päätepiste, mukaan lukien:

- Yhdistä OpenID `end_session_endpoint`, joka mahdollistaa sovelluksen käyttäjän istunnon v2.0 päätepisteen kanssa.
- v2.0 päätepisteen myöntämä id_tokens sisältää vain käyttäjän pairwise tunnus.  Tämä tarkoittaa, että kaksi eri sovellusta saavat sama käyttäjä eri tunnukset.  Huomaa, että tekemällä kyselyn Microsoft Graph `/me` päätepisteen osaat correlatable tunnuksen hakemiseen käyttäjälle, joka voi käyttää kaikissa sovelluksissa.
- v2.0 päätepisteen myöntämä id_tokens eivät sisällä `email` väittää käyttäjän tällä hetkellä, vaikka voit tarkastella niiden sähköpostin käyttäjän lupaa muokkaaminen.
- OpenID yhteyden käyttäjätiedot päätepiste. Käyttäjätiedot-päätepisteen ei ole käytettävissä v2.0 päätepisteen tällä hetkellä.  Kaikki mahdollisesti näyttöön tulee tämä päätepiste osoitteessa käyttäjän profiilitiedot ovat kuitenkin käytettävissä Microsoft Graph- `/me` päätepiste.
- Rooli ja ryhmän saatavat.  Tällä hetkellä v2.0 päätepiste ei tue antavan rooli tai ryhmän saatavat id_tokens.

Ymmärtämään protocol-toiminnon tukemat v2.0 päätepisteen laajuus-lukea Microsoftin [OpenID Yhdistä & OAuth 2.0 protokolla viittaus](active-directory-v2-protocols.md).

## <a name="restrictions-for-work--school-accounts"></a>Työpaikan ja koulun tilien määrittäminen
Tietyn Microsoft organisaation/business-käyttäjät, joita ei vielä tueta v2.0 päätepisteen on joitakin ominaisuuksia.

##### <a name="device-based-conditional-access-native-and-mobile-apps-and-the-microsoft-graph"></a>Laitteen perustuva ehdollisen käyttöoikeuden alkuperäisen ja mobile-sovellukset ja Microsoft Graph
V2.0 päätepiste ei tue for mobile ja alkuperäisen sovellusten, kuten alkuperäisen sovellusten iOS- tai Android-käyttöjärjestelmässä laitteen todennus vielä.  Se voi estää alkuperäisen sovelluksen-kutsumista Microsoft Graph tietyissä organisaatioissa.  Laitteen todennus vaaditaan, kun järjestelmänvalvoja määrittää laitteen perustuva ehdollinen käyttöoikeuskäytäntö sovelluksen.  V2.0 päätepisteen laitteen perustuva ehdollisen käyttöoikeuden todennäköisesti skenaario on käytännön määrittäminen Microsoft Graph, kuten Outlook-Ohjelmointirajapinnan resurssin järjestelmänvalvoja.  Jos järjestelmänvalvoja määrittää tämän käytännön ja alkuperäisen sovellus pyytää Microsoft Graph tunnistetta, pyynnön epäonnistuu kädessä, koska laitteeseen todennus ei vielä tueta.  Verkkosovellukset, joissa pyydetään tunnuksia Microsoft Graph-tuetaan kuitenkin, kun laite-pohjaisia käytäntöjä on määritetty.  Web-sovelluksen skenaarion laitteen todennus suoritetaan käyttäjän selaimen kautta.

Kehittäjän sinulla ei todennäköisesti nimen kohdalla ohjausobjektia kun käytännöt on määritetty Microsoft Graph resursseja tai jopa huomioon, kun se tapahtuu.  Jos olet muodostamassa sovelluksen työpaikan ja koulun käyttäjille, käytä [alkuperäisen Azure AD-päätepisteen](active-directory-developers-guide.md) kunnes v2.0 päätepisteen tukee laitteen todennusta.  Lisätietoja laitteen perustuva ehdollisen käyttöoikeuden Tutustu [tämän artikkelin](active-directory-conditional-access.md#device-based-conditional-access).

##### <a name="windows-integrated-authentication-for-federated-tenants"></a>Windows-todennusta varten liitetyt alihallinnat
Jos olet käyttänyt ADAL (ja näin ollen alkuperäisen Azure AD-päätepisteen) Windows-sovellusten, olet ehkä käyttänyt hyödyntää kutsutaan SAML vahvistus myöntäminen.  Tämän käyttäjät liitetyt Azure AD alihallinnat äänettömästi todentamismenetelmä niiden paikallinen Active Directory-esiintymä eikä sinun tarvitse syöttää tunnistetietoja.  SAML vahvistus myöntäminen on tällä hetkellä ei tueta v2.0 päätepiste.

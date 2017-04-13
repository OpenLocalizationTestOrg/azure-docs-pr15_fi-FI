<properties 
    pageTitle="Käyttöjärjestelmän toimintoja Azure sovelluksen-palvelusta" 
    description="Lisätietoja käytettävissä web Apps-sovellusten ja Azure App Service API-sovellusten mobiilisovelluksen backends OS toimintoja" 
    services="app-service" 
    documentationCenter="" 
    authors="cephalin" 
    manager="wpickett" 
    editor="mollybos"/>

<tags 
    ms.service="app-service" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/01/2016" 
    ms.author="cephalin"/>

# <a name="operating-system-functionality-on-azure-app-service"></a>Käyttöjärjestelmän toimintoja Azure sovelluksen-palvelusta #

Tässä artikkelissa on yleisiä perusaikataulun käyttöjärjestelmän toimintoja, jotka voivat käyttää kaikki sovellukset [App Azure-palvelu](http://go.microsoft.com/fwlink/?LinkId=529714)käytössä. Tämä toiminto on tiedoston, verkko- ja käyttää, ja diagnostiikka-lokit ja tapahtumia. 

<a id="tiers"></a>
## <a name="app-service-plan-tiers"></a>Sovelluksen-suunnitelma palvelutasot

Sovelluksen palvelu suoritetaan asiakkaan sovellusten usean vuokraajan isännöintipalvelu ympäristössä. Sovellukset, jotka on otettu käyttöön **vapaa** ja **jaettujen** tasoa suorittaa työntekijä prosesseihin jaetun näennäiskoneiden samalla, kun sovellukset, jotka on otettu käyttöön **Vakio** - ja **Premium** tasoa suorittaa virtual machine(s) erillinen yhteen asiakkaaseen liittyviä sovelluksia varten.

Koska App palvelun tukee sujuvan skaalauksen kokemuksen eri tasojen välissä, pakotettu App Service-sovellusten suojausasetukset säilyy ennallaan. Näin varmistat, että sovellukset ei yhtäkkiä toimivat eri tavalla, kaatuvat odottamattomia tavalla, kun sovellus palvelusopimus vaihtaa yhden tason toiseen.

<a id="developmentframeworks"></a>
## <a name="development-frameworks"></a>Kehittäminen kehysten

Laske (Suoritin, levytilasta, muistin ja verkon juniin) käytettävissä olevien resurssien sovelluksiin määrän hallinta hinnoittelu App palvelutasot. Kuitenkin käytettävissä oleviin sovelluksiin framework toimintoja kaikki säilyy ennallaan skaalauksen tasoa riippumatta.

Sovelluksen palvelu tukee erilaisia kehittäminen kehysten, mukaan lukien ASP.NET, perinteinen ASP node.js, PHP ja Python - mikä suoritetaan IIS: n tunnisteet. Yksinkertaistaa ja normalisointi suojausasetukset-sovelluksen palvelun sovellukset suorittaa yleensä eri kehittäminen kehysten niiden oletusasetuksia. Voit mukauttaa API pinta ja yksittäisiä kehittäminen kunkin framework toiminnot voi johtua yksi tapa sovellusten määrittäminen. Sovelluksen palvelun kestää yleisen lähestymistavan sen sijaan ottamalla yleisiä perusaikataulun käyttöjärjestelmän toimintoja riippumatta sovelluksen kehittämisen framework.

Seuraavissa osissa on yhteenveto käytettävissä sovelluksen palvelun sovelluksiin käyttöjärjestelmän toimintoja Yleiset tiedostotyyppejä.

<a id="FileAccess"></a>
##<a name="file-access"></a>Tiedoston käyttö

Eri asemat olemassa App palvelun, kuten paikalliset asemat ja verkkoasemat.

<a id="LocalDrives"></a>
### <a name="local-drives"></a>Paikalliset asemat

Sen core App palvelu on palvelua Azure PaaS (ympäristö palvelu)-infrastruktuuria päälle. Tuloksena on "liitetty" virtual machine paikalliset asemat on käytettävissä minkä tahansa Työntekijä-roolin Azure käynnissä saman asema tyyppiä. Tämä sisältää käyttöjärjestelmän-asema (D:\-asema), sovellus-asema, joka sisältää Azure-paketin cspkg tiedostot käyttämä yksinomaan App Service (ja käytettävissä asiakkaille) ja "käyttäjä"-asema (C:\-asema), joiden koko vaihtelee sen mukaan AM kokoa.

<a id="NetworkDrives"></a>
### <a name="network-drives-aka-unc-shares"></a>Verkkoasemassa (eli UNC jakaa)

Jokin sovellus-palvelu, joka mahdollistaa sovellusten käyttöönoton ja ylläpito yksinkertaista yksilöllinen ominaisuuksia on, että kaikki käyttäjän sisältö on tallennettu UNC osakkeet joukko. Tämä malli yhdistää erittäin hyvin sisällön paikallisen WWW-isännöinnin ympäristöissä, joissa on useita kuormituksen palvelimia tallennustilasta yleistä mallia. 

Palvelun sovellus on luotu kunkin tietokeskuksen UNC osakkeet lukumäärä. Prosentteina kunkin tietokeskuksen kaikki asiakkaat käyttäjän sisältö on kohdistettu kunkin UNC-Jaa. Lisäksi kaikki sisällön osalta yhteen asiakas-tilauksen sijoitetaan aina sama UNC-tiedoston jakaminen. 

Huomaa, että vuoksi miten cloud services-työ, UNC-Jaa isännöinnistä tietyn virtuaalikoneen muuttuvat ajan kuluessa. Varmistaa, että UNC osakkeet otetaan käyttöön eri näennäiskoneiden mukaan, kun ne tuodaan ylös ja alas aikana tavanomaiseen cloud-toimintoa. Tästä syystä sovellusten koskaan Varmista, että koneen tiedot UNC-tiedostopolku säilyy + ajan kuluessa koodattu perustiedot. Sen sijaan ne kannattaa käyttää helposti *faux* absoluuttinen polku **D:\home\site** , jonka sovellus-palvelun avulla. Tämä faux absoluuttinen polku mahdollistaa kannettavat, sovellusten ja-käyttäjä-ympäristöstä riippumattomalla tavalla omalla app viittaavat. Käyttämällä **D:\home\site**jokin voivat siirtää Jaetut tiedostot-sovelluksen ilman, että uusi suoran liikeradan määrittämiseksi kunkin siirto.

<a id="TypesOfFileAccess"></a>
### <a name="types-of-file-access-granted-to-an-app"></a>Tiedoston käyttöoikeuden sovellukselle tyypit

Kunkin asiakkaan tilauksella on varattu kansiorakenne tietyn UNC-resurssiin data Centerissä kuluessa. Asiakas voi olla useita sovelluksia luodaan sisällä tiettyä palvelinkeskuksessa, joten kaikki yhteen asiakas-tilaukseen kuuluvien kansioita luodaan samaan UNC jakaminen. Jaa voi sisältyä kansioita, kuten sisältöä, virhe ja vianmäärityslokit ja tietolähteen ohjausobjektin luoma sovelluksen aiemmissa versioissa. Odotetusti asiakkaan sovelluksen kansiot ovat käytettävissä luku-ja kirjoitusoikeudet suorituksen sovelluksen sovelluksen koodilla.

Liitetty virtuaalikoneen, joka suoritetaan sovelluksen paikallisen asemista sovelluksen palvelun varaa lohkon app kielikohtaiset paikallisen väliaikaisten C:\ asemasta tilaa. Sovellus on oma paikallinen väliaikaisten kirjoitus käyttöoikeus, vaikka kyseistä tallennustilan todella ei ole tarkoitettu suoraan sovelluksen koodilla. Sen sijaan tarkoituksena on säätää väliaikaisten tiedostojen tallentamisesta IIS- ja web application puitteissa. Sovelluksen palvelun rajoittaa myös väliaikaisen paikallisen tallennustilan käytettävissä muissa paikallisen tiedostosäilön liiallinen määriä yksittäisten sovellusten estäminen yksittäisistä sovelluksista.

Kaksi esimerkkiä siitä, miten sovelluksen palvelun käyttää paikallisen väliaikaisten ovat ASP.NET väliaikaisten tiedostojen kansio ja IIS kansio pakatut tiedostot. ASP.NET kääntäminen järjestelmä käyttää "Temporary ASP.NET Files-kansion tilapäinen kääntäminen välimuistin sijainniksi. IIS käyttää pakatun vastauksen tulosteen "IIS väliaikaiset pakatut tiedostot-kansio. Molemmat seuraavanlaisiin tiedoston käyttö (sekä muiden) ovat määrittää uudelleen-sovelluksen palvelun sovelluksen kohti, jossa on paikallinen väliaikaisten. Tämä liittämisen varmistaa toimintoja toistuu oikein.

Kunkin app-sovelluksen palvelun suoritetaan satunnaisia yksilöllinen pieni toimittajien työntekijä prosessin identity-nimeltä "sovellussarjan tunnistustietojen", tässä artikkelissa kuvattuja: [http://www.iis.net/learn/manage/configuring-security/application-pool-identities](http://www.iis.net/learn/manage/configuring-security/application-pool-identities). Sovelluksen koodin käyttää tätä jäsenyyttä basic käyttöjärjestelmä-asema (D:\-asema) vain luku-tilassa. Tämä tarkoittaa luettelon yleisiä directory-rakenteita ja lukea yhteiset tiedostot käyttöjärjestelmä asemassa sovelluksen koodia. Vaikka tämä saattaa näkyä voi olla hieman laaja taso access, sama kansiot ja tiedostot ovat käytettävissä, kun voit valmistella työntekijän rooli Azure-tietokannassa isännöidään palvelun ja lue aseman sisältöä. 

<a name="multipleinstances"></a>
### <a name="file-access-across-multiple-instances"></a>Tiedoston käyttöoikeuden yli useita kertoja

Pääkansion sisältää sovelluksen ja sovelluksen koodin voi kirjoittaa. Jos sovellus toimii eri esiintymissä, pääkansion on jaettu kaikkien esiintymien välillä, niin, että kaikki esiintymät näkyvät samassa kansiossa. Näin on esimerkiksi jos sovellus tallentaa ladatut tiedostot pääkansion, kyseiset tiedostot ovat heti käytettävissä kaikki esiintymät. 

<a id="NetworkAccess"></a>
## <a name="network-access"></a>Verkkokäyttö
Sovelluksen koodin käyttää TCP/IP ja UDP-pohjainen protokollat tekemään lähtevän verkkoyhteyksien Internet käytettävissä päätepisteet, joka näyttää ulkoisia palveluja. Sovellusten avulla saman protokollat muodostaa services Azure & #151 sisällä, kuten luomalla HTTPS yhteydet SQL-tietokantaan.

On myös rajoitettu vain yhden paikallisen silmukka-yhteyden, ja sovelluksen kuuntelevat paikallisen silmukka, socket sovellusten. Tämä ominaisuus on ensisijaisesti, jos haluat ottaa käyttöön sovellukset, jotka kuuntelevat paikallisen silmukan sockets niiden toimintoja osana. Huomaa, että kunkin sovelluksen näkee "Yksityinen" silmukan; yhteyden sovelluksen "A" ei voi kuunnella paikallisen silmukka-socket perustettu app "B".

Nimettyjen putkien tuetaan myös prosessien välisen tietoliikenteen (IPC)-järjestelmä eri prosessien, jotka suoritetaan muunnokset sovelluksen välillä. IIS FastCGI-moduulin riippuvainen nimettyjen putkien yksittäisiä sivuja PHP suoritettavat prosessit järjestämiseen.


<a id="Code"></a>
## <a name="code-execution-processes-and-memory"></a>Koodin suorittamisen, prosessit ja muistin
Aiemmin mainittuja sovellusten Suorita sisältyy pieni toimittajien työntekijä prosessit satunnaisia sovellussarjan avulla. Sovelluksen koodi on muistitilan liittyvä työntekijä prosessin sekä kaikki aliprosessit, jotka voivat olla näyttöriippuvainen CGI prosesseja tai muut sovellukset. Yhden sovelluksen ei voi kuitenkin käyttää muistia tai toisessa sovelluksessa tietoja, vaikka se on käytössä sama virtuaalikoneen.

Sovellusten voit suorittaa komentosarjoja tai kirjoitettu tuetut web kehittäminen kehysten sivuja. Sovelluksen palvelu ei Määritä web framework asetukset, tiukemmat tilat. Esimerkiksi sovelluksen-palvelu käytössä ASP.NET-sovellukset toimivat "täyden" luottamuksen tiukemmat luotetussa-tilassa eikä. Web-kehysten, mukaan lukien sekä perinteinen ASP ja ASP.NET, voit kutsua prosessin COM-komponentteja (mutta ei poissa-prosessin COM-komponentteja), kuten ADO (ActiveX Data Objects), joka on rekisteröity Windows-käyttöjärjestelmän oletusarvon mukaan.

Sovellukset voivat spawn ja suorittaa koodia. On sallittu sovelluksen, voit tehdä esimerkiksi spawn komentoliittymän tai suorittaa PowerShell-komentosarjaa. Vaikka koodin ja prosessit voivat näyttöriippuvainen sovelluksen, ohjelmia ja komentosarjoja on kuitenkin yhä rajoitettu myönnetyt sovellussarjan ylemmän tason oikeudet. Esimerkiksi sovelluksen voit spawn suoritettavan tiedoston, joka tekee lähtevä HTTP-puhelu, mutta, että sama suoritettavan et voi yrittää yhteyden IP-osoitteen virtual machine, valitse sen NIC-sivustosta. Puhelujen soittaminen lähtevän verkon on oikeus toimittajien matala koodi, mutta yritetään määrittämään verkkoasetukset virtual tietokoneeseen edellyttää järjestelmänvalvojan oikeuksia.


<a id="Diagnostics"></a>
## <a name="diagnostics-logs-and-events"></a>Diagnostiikan lokit ja tapahtumat
Lokitiedoston tiedot ovat toisessa tietojoukolle, jotkin sovellukset yrittävät tarkastella. Log tietotyypeistä on käytettävissä sovelluksen palvelun koodin sisältää diagnostiikan ja kirjaudu sovellusta, joka on myös helposti saatavilla sovelluksen luomat tiedot. 

Esimerkiksi aktiivisen sovelluksen luomia W3C HTTP lokeja ovat käytettävissä joko Jaa verkkosijainti luoda sovelluksen, tai se on käytettävissä Blob-objektien tallennustilaan, jos asiakas on määrittänyt tallennustilan W3C Kirjaus lokiin hakemistossa. Jälkimmäisessä valinnalla virhelokit kerännyt ilman jaetusta verkkoresurssista liittyvä tiedosto-tallennustilan rajoitusten ylittämisen riskin suuria määriä.

Samanlaiset alueilla-reaaliaikainen diagnostiikka tiedot .NET sovelluksista voidaan myös kirjata .NET jäljityksen ja diagnostiikka-infrastruktuurin käyttäminen asetukset kirjoittaa jäljityksen tiedot joko sovelluksen verkkoresurssiin tai vaihtoehtoisesti blob storage sijaintiin.

Diagnostiikan kirjaus ja jäljitys, jotka eivät ole käytettävissä oleviin sovelluksiin ovat Windowsin tapahtumien seuranta-tapahtumien ja yleisten Windowsin tapahtumalokien (kuten järjestelmä ja suojaus sovelluksen tapahtumalokien). Koska tapahtumien seuranta jäljitystiedot mahdollisesti voidaan tarkastella tietokoneen (omistavat käyttöoikeusluettelot), luku- ja kirjoitusoikeudet tapahtumien seuranta tapahtumat on estetty. Kehittäjät saatat huomata, että API soittaa lukemiseen ja kirjoittamiseen tapahtumien seuranta, tapahtuma- ja yleisten Windowsin tapahtumalokien näyttävät toimivat, mutta tämä johtuu App palvelu on "faking" kutsut niin, että ne näkyvät onnistuu. Todellisuudessa sovelluksen-koodia ei ole käyttöoikeutta tapahtuman tiedot.

<a id="RegistryAccess"></a>
## <a name="registry-access"></a>Rekisterin käyttö
Sovellukset on vain luku-suuri (mutta ei kaikkia) niitä suoritetaan virtuaalikoneen rekisterin. Käytännössä tämä tarkoittaa rekisteriavaimia, jotka sallivat vain luku-oikeudet paikallisten käyttäjäryhmälle ovat käytettävissä sovellukset. Alueesta, joka ei tällä hetkellä tueta luku-tai kirjoitusoikeudet rekisterin on HKEY\_nykyisen\_käyttäjän rakenne.

Kirjoitusoikeutta rekisteriin on estetty, mukaan lukien rekisteriavainten käyttäjäkohtainen käyttöoikeus. Sovelluksen näkökulmasta rekisterin kirjoitusoikeudet olisi koskaan olla huolehdi Azure-ympäristössä jälkeen apps (tai tehdä) siirretä eri näennäiskoneiden yli. Vain pysyvä kirjoitettavat tallennustilan, jota voit riippuvaisia sovelluksen mukaan on-app sisällön kansiorakenne, sovelluksen palvelun UNC jaetuille tallennettu. 

>[AZURE.NOTE] Jos haluat aloittaa Azure App palvelun ennen rekisteröimässä Azure-tili, siirry [Yritä App palvelu](http://go.microsoft.com/fwlink/?LinkId=523751), jossa lyhytkestoinen starter verkkosovellukseen heti voit luoda sovelluksen-palvelussa. Ei ole pakollinen; luottokortit ei ole sitoumukset.

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]
 
 

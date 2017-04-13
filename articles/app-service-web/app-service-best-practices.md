<properties
    pageTitle="Azure App palvelun parhaat käytännöt"
    description="Opi parhaita käytäntöjä ja Azure App palvelun vianmääritys."
    services="app-service"
    documentationCenter=""
    authors="dariagrigoriu"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/30/2016"
    ms.author="dariagrigoriu"/>
    
# <a name="best-practices-for-azure-app-service"></a>Azure App palvelun parhaat käytännöt

Tässä artikkelissa on yhteenveto [Azure App palvelun](http://go.microsoft.com/fwlink/?LinkId=529714)parhaat käytännöt. 

## <a name="colocation"></a>Ulkoistaminen
Kun Azure resurssien sähköpostiviestiä ratkaisu, kuten verkkosovellukseen ja tietokannan sijaitsevat eri alueilla tehosteita voi ovat seuraavat:

*  Parannettu resurssien välisen viive
*  Lähtevien tietojen raha kulujen siirtää rajat-alueen mainittuja [Azure hinnoittelu sivun](https://azure.microsoft.com/pricing/details/data-transfers).

Ulkoistaminen samalla alueella on parasta Azure resurssien ratkaisu, kuten verkkosovellukseen ja käyttää sisältöä tai tietojen tietokannasta tai tallennustilan tilin sähköpostiviestiä. Luotaessa resursseja, varmista, että ne ovat samassa Azure alueella, ellei on tietyn business tai syy niitä ei voi suunnitella. Voit siirtää sovelluksen palvelun sovelluksen sama alue kuin tietokannan hyödyntäminen [sovelluksen palvelun kloonaamalla ominaisuus](app-service-web-app-cloning-portal.md) tällä hetkellä käytettävissä Premium App palvelun suunnittelu-sovelluksissa.   

## <a name="memoryresources"></a>Kun sovellusten tarjoaman muistia odotettua
Kun huomaat sovelluksen siinä käytetään muistia odotettua kautta valvontaa esitetyllä tai palvelun suosituksia harkitse [sovelluksen palvelun automaattinen kenties ominaisuus](https://azure.microsoft.com/blog/auto-healing-windows-azure-web-sites). Jokin vaihtoehdoista automaattinen kenties toiminnon kestää mukautettujen toimintojen muistin raja-arvon perusteella. Toiminnot kattavat eri sähköposti-ilmoitusten kautta muistivedos paikan-rajoitus, tutkimuksen mukaan kierrättäminen työntekijä-prosessin. Automaattinen kenties voi määrittää web.config kautta ja ohjeiden etsiminen tämän blogimerkinnässä [App palvelun tuki sivuston laajennusta](https://azure.microsoft.com/blog/additional-updates-to-support-site-extension-for-azure-app-service-web-apps)helpossa muodossa käyttöliittymän välityksellä.   

## <a name="CPUresources"></a>Kun sovellusten tarjoaman Lisää suorittimen odotettua
Kun huomaat sovelluksen siinä käytetään enemmän suorittimen kuin oikein tai ilmenee toistuva suorittimen piikkarit kautta valvontaa esitetyllä tai palvelun suosituksia harkitse skaalauksen tai laajentaminen App palvelusopimus. Jos sovellus on statefull, skaalauksen on ainoa tapa, kun Jos sovellus on tilattomien, skaalauksen ulos avulla voit joustavammin ja suurempi mittakaava mahdollisuuksia. 

Katso myös tämä video lisätietoja "statefull" ja "tilattomien" sovellusten: [suunnittelu monitasoisten skaalattava lopusta loppuun-sovelluksen Microsoft Azure Web Appissa](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/DEV-B414#fbid=?hashlink=fbid). Saat lisätietoja sovelluksen skaalaus ja automaattisen skaalauksen poistaminen Palveluasetukset lukea: [asteikko Azure-sovelluksen palvelun verkkosovellukseen](web-sites-scale.md).  

## <a name="socketresources"></a>Kun socket resursseja on täyttynyt
Yleinen syy, joka on uuvuttavaa lähtevän TCP-yhteyksien on asiakkaan kirjastojen, joka ei ole otettu käyttöön uudelleen TCP-yhteydet tai uudempi versio tason protokolla, kuten HTTP - säilyttämisen ei hyödyntää. Tarkista viittaamat sovellukset-että App palvelun suunnitteleminen varmistaa, ne on määritetty tai käyttää koodissa Lähtevät yhteydet tehokasta uudelleenkäyttöä varten kirjastojen ohjeissa. Noudata myös ERISNIMI luominen ja vapauta tai uudelleenjärjestäminen välttämiseksi vuoda yhteydet kirjaston asiakirjat ohjeita. Asiakkaan kirjastojen tutkimuksia aikana edistymisen vaikutus joiden saattaa lievity mukaan skaalausta useita kertoja.  

## <a name="appbackup"></a>Kun sovellus Varmuuskopioi aloittaa puuttuessa
Miksi app varmuuskopion epäonnistuu on kaksi yleisimmät syyt: Virheellinen säilön asetukset ja Virheellinen tietokannan määritykset. Nämä virheet käydä yleensä, kun on muutokset tallennustilan tai tietokannan resurssien tai muutokset, voit käyttää seuraavia resursseja (tunnistetiedot esimerkiksi tietokannan valittuna salasanojen hallinta-asetusten päivittäminen). Varmuuskopioiden yleensä suorittaa aikataulun ja Vaadi tallennustila (määritetty varmuuskopioidut tiedostot) ja tietokantojen (kopioimalla ja varmuuskopioinnin sisällytettävät sisällön lukeminen). Voit käyttää joko nämä resurssit mahdollista tulos on yhtenäinen varmuuskopioinnin. 

Kun varmuuskopioinnin virheiden aktivoitu, tarkista viimeisin tulokset selvittääksesi, minkä tyyppistä virhe tapahtuu. Tallennustilan käyttö epäonnistuu, jos tarkistaa ja päivittää määritysten varmuuskopion tallennustilan asetuksia. Kyseessä tietokanta access-virheet Ota tarkistaa ja päivittää yhteydet merkkijonot sovelluksen asetuksissa; Siirry sitten Päivitä varmuuskopioinnin määritykset sisältämään tarvittavat tietokannat oikein. Katso lisätietoja sovelluksen varmuuskopioinnin [verkkosovellukseen Azure-sovelluksen palvelun varmuuskopiointi](web-sites-backup.md) -ohjeista.

## <a name="nodejs"></a>Kun uusi Node.js sovellukset käyttöön Azure sovelluksen-palveluun
Azure App palvelun oletusarvon mukainen määritys Node.js sovellukset on tarkoitettu parhaiten sopivat yleisimmät sovellusten tarpeisiin. Jos Node.js sovelluksen kokoonpanon hyötyä mukautettuja säätäminen suorituskyvyssä tai optimointi resurssien käyttö suorittimen ja muistin tai verkon resurssien, voi tarkastella Tutustu parhaisiin käytäntöihin ja vianmääritysohjeita. Ohjeet tässä artikkelissa kuvataan iisnode asetukset, saatat joutua määrittämään Node.js sovelluksen, kuvaus eri tilanteista tai sovellus voi olla vastakkaisten ja näyttää, miten voit ratkaista ongelmat: [parhaiden käytäntöjen ja solmu sovellukset App Azure-palvelusta vianmääritystietoja oppaan](app-service-web-nodejs-best-practices-and-troubleshoot-guide.md).   



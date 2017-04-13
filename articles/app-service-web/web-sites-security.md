<properties
    pageTitle="Suojatun sovelluksen Azure sovelluksen-palvelussa"
    description="Tietoja suojatun web Appissa, mobiilisovelluksen taustatietokannan tai Azure App Service API-sovellusta."
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="01/12/2016"
    ms.author="cephalin"/>


#<a name="secure-an-app-in-azure-app-service"></a>Suojatun sovelluksen Azure sovelluksen-palvelussa

Tämän artikkelin avulla pääset alkuun suojaamisesta verkkosovellukseen, mobiilisovelluksen taustatietokannan tai Azure App Service API-sovellusta. 

Azure-sovelluksen palvelun suojaus on kaksi tasoa: 

- **Infrastruktuuri ja ympäristö suojaus** - luotat Azure palveluja, sinun on suoritettava todella kohteita turvallisesti pilvipalveluun.
- **Sovelluksen suojaus** - sinun täytyy suunnitella sovelluksessa suojatusti. Tämä sisältää siitä, miten voit integroida Azure Active Directory, miten voit hallita varmenteet ja miten voit varmistaa, että, voit ottaa suojatusti eri palveluihin. 

#### <a name="infrastructure-and-platform-security"></a>Infrastruktuuri ja ympäristö suojaus
Koska App palvelun ylläpitää Azure VMs tallennustilaa, verkkoyhteyksien, web-kehysten, hallinnasta ja asiakasintegraatio-ominaisuuksia ja paljon muuta se aktiivisesti suojattu ja karkaistu ja käy läpi voimakkaan vaatimustenmukaisuus ja tarkistaa jatkuvasti ja varmistaa seuraavat asiat:

- Sovelluksen palvelun sovelluksia eristetään sekä Internetistä ja muiden asiakkaiden Azure resursseista.
- Tietoja (kuten yhteyden merkkijonot) – sovelluksen palvelun sovelluksen resurssi-ryhmässä muut Azure resurssit (esimerkiksi SQL-tietokanta) pysymisen rajoitusten sisällä Azure ja toimintojen ei mitään verkon rajat. Tietoja aina salataan.
- Kaikki viestintä palvelun App sovelluksen ja ulkoiset resurssit, kuten PowerShell hallinta, käyttöliittymä, Azure SDK: T, REST API ja hybrid yhteydet välillä salataan oikein.
- 24 tunnin uhkien hallinta suojaa App palvelun resursseja, haittaohjelmia vastaan distributed eston palvelun WWW.microsoft.com-sivustoa (kohtaan), mies---keskiosan (MITM) ja muut uhkien. 

Lisätietoja Azure infrastruktuuri ja ympäristö tietoturvasta on artikkelissa [Azure Valvontakeskus](/support/trust-center/security/).

#### <a name="application-security"></a>Sovelluksen suojaus

Azure ollessa vastuussa infrastruktuuri ja alustaksi, joka suoritetaan sovelluksen suojaaminen se on suojattu sovelluksen itse. Toisin sanoen haluat kehittää, ottaminen käyttöön ja suojatun sovelluksen koodin ja sisällön hallinta. Muuten sovelluksen koodin tai sisältöä voi olla yhä koske uhkien seuraavasti:

- SQL-lisäämisen
- Istunnon kaappaaminen
- Sivuston rajat komentosarjan
- Sovellustason MITM
- Sovellustason WWW.microsoft.com-sivustoa kohtaan

Web-sovelluksissa liittyviä suojaustietoja, koko keskustelu on tämän artikkelin piiriin. Muita ohjeita suojaaminen sovelluksen-kohdan artikkelissa määrittämän OWASP jäsenet [Avaa Web Application Security projektin (OWASP)](https://www.owasp.org/index.php/Main_Page), erityisesti [top 10 project.](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project), joka sisältää nykyisen Ylin 10 kriittinen web sovelluksen suojauksen virheet.

## <a name="perform-penetration-testing-on-your-app"></a>Suorittaa läpivienti-sovelluksen testaaminen

Helpoin tapa aloittamisesta vielä testaus heikkouksien App Service-sovellukseen on käytetään [integrointi Tinapaperi suojauksen](/blog/web-vulnerability-scanning-for-azure-app-service-powered-by-tinfoil-security/) yhdellä napsautuksella haavoittuvuus sovelluksen tarkistus. Voit tarkastella testitulokset helppoa ymmärtää raportti ja katso, miten voit korjata haavoittuvista vaiheittaiset ohjeet.

Jos mieluummin itse läpivienti testit tai haluat käyttää toista skannerin ohjelmiston tai palvelun, [Azure läpivienti testauksen hyväksymisprosessi](https://security-forms.azure.com/penetration-testing/terms) ja saada haluamasi läpivienti testit edellisen hyväksyntä.

##<a name="https"></a>Suojattu tietoliikenne asiakkaiden kanssa

Jos käytät ** \*. azurewebsites.net** toimialuenimen luotu sovelluksen Service-sovelluksen voit käyttää HTTPS-heti kuin SSL-varmenteen kaikkien ** \*. azurewebsites.net** toimialuenimien. Jos sivusto käyttää [mukautettua toimialuenimeä](web-sites-custom-domain-name.md), voit ladata SSL-varmenteen mukautetun toimialueen [käyttöön HTTPS](web-sites-configure-ssl-certificate.md) .

Ottaminen käyttöön [HTTPS](https://en.wikipedia.org/wiki/HTTPS) voit suojautua MITM Enterprisessa välisen sovelluksen ja sen käyttäjät.

## <a name="secure-data-tier"></a>Tietojen suojaaminen taso

Sovelluksen palvelun integroitu SQL-tietokantaan erittäin siten, että kaikki yhteyden merkkijonot on salattu yli tauluun ja ovat vain purkaa AM, sovellus toimii *ja* vain silloin, kun sovellus käynnistyy. Lisäksi Azure SQL-tietokanta sisältää useita suojausominaisuuksia, jotka auttavat suojaa cyber uhkien, myös, [milloin muiden salausta](https://msdn.microsoft.com/library/dn948096.aspx), [Aina salattu](https://msdn.microsoft.com/library/mt163865.aspx), [Dynamic Data Masking](../sql-database/sql-database-dynamic-data-masking-get-started.md)ja [Uhkien tunnistus](../sql-database/sql-database-threat-detection-get-started.md)hakemuksen tiedot. Jos sinulla on luottamuksellisia tietoja tai yhteensopivuusvaatimukset, katso lisätietoja tietojen suojaamisesta [suojaaminen SQL-tietokantaan](../sql-database/sql-database-security.md) .

Jos käytössäsi on kolmannen osapuolen tietokannan palveluntarjoaja, kuten ClearDB, ota yhteyttä suoraan palveluntarjoajan dokumentaatio suojauksen parhaiden käytäntöjen.  

##<a name="develop"></a>Suojatun kehittäminen ja käyttöönotto

### <a name="publishing-profiles-and-publish-settings"></a>Julkaisemisen profiilit ja Julkaisuasetukset

Sovellusten hallintatehtäviä suorittavien tai apuohjelmia, kuten **Visual Studiossa**, **Web-matriisi**, **PowerShellin Azure** tai **Azure käyttöliittymä (Azure CLI)**tehtävien automatisointi kehittämisessä voit *Julkaisuasetukset* tiedoston tai *julkaisun profiilin*. Molemmat tiedostotyypit todentamismenetelmä Azure ja estää näin luvattoman pääsyn suojata.

* **Julkaisuasetukset** -tiedosto sisältää

    * Azure Tilaustunnus

    * Hallinta-sertifikaatti, jonka avulla voit suorittaa hallintatehtävät oman tilauksen *kirjoittamatta tilin nimen tai salasanan*.

* **Julkaisun profiili** -tiedosto sisältää

    * Lisätietoja sovelluksen julkaiseminen

Jos käytät apuohjelma, joka käyttää Julkaise asetustiedosto tai Julkaise profiilitiedoston, sisältävän Julkaise asetukset tai profiilin apuohjelma ja valitse **Poista** tiedosto tiedoston tuominen. Jos sinulla on tiedoston, jakaminen muille projektissa työskentelevien esimerkiksi tallentaa turvalliseen paikkaan, kuten *salatun* kansio, jonka käyttöoikeuksia on rajoitettu.

Lisäksi Varmista että tuodut tunnistetiedot ovat paikallaan. Esimerkiksi **PowerShellin Azure** ja **Azure käyttöliittymä (Azure CLI)** tallentaa tuotavat tiedot **pääkansion** (*~* Linux tai OS X-ja Windows-järjestelmiin */users/yourusername* .) Ylimääräinen arvopaperille haluat ehkä **salata** paikoista käyttöjärjestelmäkohtaisia salaus työkalujen avulla.

### <a name="configuration-settings-and-connection-strings"></a>Asetukset ja yhteyden merkkijonot
Se on yleisin tapa tallentaa yhteyden merkkijonoja, todennuksen käyttäjätiedot ja muita luottamuksellisia tietoja tiedostojen. Nämä tiedostot voivat valitettavasti voi sivuston tarjoamia tai kuitataan julkisen säilöön, paljastaa nämä tiedot. Tarkennettu haku- [GitHub](https://github.com), voit avata esimerkiksi rajattomasti määritystiedostoja, joissa näkyviä tietoja julkisen säilöjen tietoihin.

Paras käytäntö on nämä tiedot pitää sinua sovelluksen kokoonpanotiedostot ulos. Sovelluksen Servicen avulla voit tallentaa kokoonpanotietoja **sovelluksen asetusten** ja **yhteyden merkkijonojen**osana runtime-ympäristössä. Arvot niitä julkaista sovelluksesi suorituksen *Ympäristömuuttujat* useimpien ohjelmoinnin kielten kautta. .NET-sovelluksia nämä arvot ovat syötetään .NET-kokoonpanosi suorituksen aikana. Näissä tilanteissa, lukuun ottamatta nämä asetukset, säilyvät salattuja ellet tarkastella tai määrittää niitä [Azure kautta](https://portal.azure.com) tai apuohjelmia, kuten PowerShell tai Azure-CLI avulla. 

Tallentaminen kokoonpanotietoja App palvelun ansiosta sovelluksen järjestelmänvalvojan Lukitse tuotannon-sovellusten luottamuksellisia tietoja. Kehittäjät voivat käyttää joukko määritysasetukset sovelluksen kehittämiseen ja asetuksia voidaan automaattisesti kumota määritetty sovelluksen palvelun asetusten mukaan. Edes kehittäjät on tunnettava määritetty tuotannon sovelluksen tietoja. Lisätietoja määrittämällä sovelluksen asetusten ja yhteysmerkkijonon sovelluksen palvelu on artikkelissa [määrittäminen verkkosovelluksissa](web-sites-configure.md).

### <a name="ftps"></a>FTPS

Azure App palvelu mahdollistaa suojatun FTP tiedostojärjestelmän, kun sovellus **FTPS**kautta. Näin voit käyttää turvallisesti web Appin sekä vianmääritys sovelluksen koodia lokitiedot. On suositeltavaa, että käytät FTPS aina FTP sijaan. 

Sovelluksen FTPS linkkiä löytyy seuraavasti:

1. Avaa [Azure Portal](https://portal.azure.com).
2. Valitse **Etsi kaikki**.
3. Valitse **Selaa** , sivu **Sovelluksen palvelut**.
4. Valitse haluamasi sovellus **App palvelut** -sivu.
5. Valitse sovelluksen sivu **kaikkia asetuksia**.
6. Valitse **asetukset** -sivu **Ominaisuudet**.
7. FTP- ja FTPS linkit ovat käytettävissä **asetukset** -sivu. 

Saat lisätietoja FTPS [File Transfer Protocol](http://en.wikipedia.org/wiki/File_Transfer_Protocol).

## <a name="next-steps"></a>Seuraavat vaiheet

Katso lisätietoja suojausta Azure ympäristön tiedot raportointi on **Suojaus tai väärinkäyttö**tai Microsoft ilmoittaa, että suoritat sivustosi **läpivienti testaaminen** [Microsoft Azure-Valvontakeskus](https://azure.microsoft.com/support/trust-center/security/)-suojaus-osa.

Lisätietoja **web.config** tai **applicationhost.config** tiedostoja App Service-sovelluksissa on artikkelissa [asetukset lukitus Azure App palvelun verkkosovelluksissa](https://azure.microsoft.com/blog/2014/01/28/more-to-explore-configuration-options-unlocked-in-windows-azure-web-sites/).

Lisätietoja lokiin kirjaaminen App Service-sovelluksia, joka voi olla hyötyä kalastelu-artikkelissa [Diagnostiikan kirjaus käyttöön](web-sites-enable-diagnostic-log.md).

>[AZURE.NOTE] Jos haluat aloittaa Azure App palvelun ennen rekisteröimässä Azure-tili, siirry [Yritä App palvelu](http://go.microsoft.com/fwlink/?LinkId=523751), jossa lyhytkestoinen starter-sovelluksen heti voit luoda sovelluksen-palvelussa. Ei ole pakollinen; luottokortit ei ole sitoumukset.

## <a name="whats-changed"></a>Mikä on muuttunut

* Katso muutoksen opas verkkosivuilta App palveluun: [Azure App palvelu ja sen vaikutus aiemmin Azure-palvelut](http://go.microsoft.com/fwlink/?LinkId=529714)

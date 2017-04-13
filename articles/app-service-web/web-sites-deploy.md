<properties
    pageTitle="Ottaa käyttöön sovelluksen Azure sovelluksen-palveluun"
    description="Opi ottamaan sovelluksen Azure-sovelluksen palveluun."
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="cephalin;dariac"/>
    
# <a name="deploy-your-app-to-azure-app-service"></a>Ottaa käyttöön sovelluksen Azure sovelluksen-palveluun

Tämän artikkelin avulla voit selvittää, paras vaihtoehto web Appissa, mobiilisovelluksen taustatietokannan tai API-sovelluksen tiedostot käyttöön [Azure App palvelun](http://go.microsoft.com/fwlink/?LinkId=529714)ja auttaa sinua tarvittavat resurssit ohjeet tiettyjen haluamallasi tavalla.

## <a name="overview"></a>Azure App palvelun käyttöönoton yleiskatsaus

Azure App palvelun ylläpitää application framework puolestasi (ASP.NET, PHP, Node.js jne.). Jotkin kehysten ovat käytössä oletusarvoisesti samalla, kun muut, kuten Java ja Python, Yksinkertainen valintamerkki määrityksiin, jotta se on ehkä. Lisäksi voit mukauttaa sovelluksen-framework, kuten PHP-versio tai oman runtime bittisyys. Lisätietoja on artikkelissa [Azure App palvelun sovelluksen määrittäminen](web-sites-configure.md).

Koska sinun ei tarvitse huolehtia WWW-palvelimeen tai sovelluksen framework-sovelluksen käyttöönotto sovelluksen-palveluun on ohjeisiin ja käyttöönotto koodi, binaaritiedostot, sisällön tiedostoja ja niiden vastaaviin kansiorakenne Azure [ **/site/wwwroot** directory](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure) (tai **/sivuston/wwwroot/App_Data/työt/** WebJobs kansio). Sovelluksen palvelu tukee seuraavia asennusvaihtoehdot: 

- [FTP- tai FTPS](https://en.wikipedia.org/wiki/File_Transfer_Protocol): Käytä tuttuja FTP- tai FTPS käytössä Siirry Azure-tiedostojen [FileZilla](https://filezilla-project.org) , kuten [NetBeans](https://netbeans.org)täydet toiminnot sisältävien ohjelmien IDEs-työkalu. Tämä on ehdottoman tiedoston lataus. Ei ole lisäpalveluja annettu sovelluksen palvelun, kuten Versionhallinta, rakenteen hallinta. 

- [Kudu (Git/Mercurial tai OneDrive tai iPad)](https://github.com/projectkudu/kudu/wiki/Deployment): sovelluksen palvelun [käyttöönoton ohjelma](https://github.com/projectkudu/kudu/wiki) käyttäminen. Siirtää koodisi Kudu suoraan minkä tahansa säilöstä. Kudu on lisätty services myös aina, kun koodi on miten, esimerkiksi Versionhallinta, paketti Palauta, MSBuild ja [web koukut](https://github.com/projectkudu/kudu/wiki/Web-hooks) jatkuva käyttöönoton ja automaatio muihin tehtäviin. Kudu käyttöönotto-ohjelma tukee 3 erityyppisiä käyttöönoton lähteet:   
    * Sisällön synkronointi OneDrive tai Dropbox   
    * Automaattinen-synkronoinnin GitHub, Bitbucket ja Visual Studio Team Services säilöön perustuva jatkuva käyttöönotto  
    * Tietovaraston perustuva-ympäristö, jonka paikallisen Git manuaalisen synkronoinnin  

- [Web-käyttöön](http://www.iis.net/learn/publish/using-web-deploy/introduction-to-web-deploy): Ota käyttöön koodi sovelluksen palveluun suoraan tuttuja Microsoft työkaluja, kuten Visual Studio samalla tooling, joka automatisoi käyttöönoton IIS-palvelimiin. Tämä työkalu tukee vain eroavuus käyttöönoton, tietokannan luonti, muunnokset yhteysmerkkijonon jne. Web-käyttöönotto eroaa Kudu siten, että sovelluksen binaaritiedostot on suunniteltu ennen niiden ottamista käyttöön Azure. Kaltaisilta FTP ei lisäpalveluja toimitetaan App palvelu.

Suositut web Kehitystyökalut tukevat vähintään yksi seuraavista prosesseista käyttöönotto. Samalla, kun valitset työkalu määrittää käyttöönottoprosessit voidaan hyödyntää, todellinen DevOps toimintoja, käytettävissä olevalla määräytyy käyttöönottoprosessin ja valitset tietyn Työkalut yhdistelmä. Esimerkiksi jos teet Web käyttöönotto [Visual Studiossa Azure SDK-paketissa](#vspros), vaikka et saa automaatio-Kudu, saat paketin palauttaminen ja MSBuild automaatio Visual Studiossa. 

>[AZURE.NOTE] Nämä käyttöönottoprosessit ei varsinaisesti [Varaa Azure resurssit](../resource-group-template-deploy-portal.md) , jotka on ehkä sovellus. Kuitenkin suurin osa linkitetyn Ohjeartikkelit, kuinka voit valmistella sovellus ja ota se käyttöön koodisi lopusta loppuun. Löydät myös muita ominaisuuksia valmistelu Azure resurssien [automatisoiminen käyttöönotto käyttämällä komentorivin Työkalut](#automate) -osasta.
     
## <a name="ftp"></a>Tiedostojen kopioiminen Azure manuaalisesti FTP käyttöönotto
Jos käytit kopioiminen web-sisältöä manuaalisesti verkkopalvelimeen, voit kopioida tiedostot, kuten Resurssienhallinnan tai [FileZilla](https://filezilla-project.org/) [FTP](http://en.wikipedia.org/wiki/File_Transfer_Protocol) -apuohjelma.

Tiedostojen kopiointia manuaalisesti ammattilaisille ovat seuraavat:

- Kannattaa ja rajoitustarve monimutkaisuutta, FTP sillä. 
- Tiedät tarkalleen siinä muodossa, jossa tiedostojen sujuvat.
- Lisätty arvopaperille, jonka FTPS.

Tiedostojen kopiointia manuaalisesti kuvakkeet ovat seuraavat:

- Tarvitsee tietää, miten tiedostoja käyttöön oikea kansioiden sovelluksen-palvelussa. 
- Peruuttaminen epäonnistuu toteutuessa ei Versionhallinta.
- Ei ole valmiita käyttöönoton historia käyttöönoton ongelmien vianmääritykseen.
- Mahdollinen pitkä käyttöönotto-aikoja, koska monet FTP-Työkalut ei anna vain eroavuus kopioiminen ja kopioi riittää, että kaikki tiedostot.  

### <a name="howtoftp"></a>Käyttöönottamisesta kopioimalla tiedostot Azure manuaalisesti
Tiedostojen kopioiminen Azure on seuraavien ohjeiden:

1. Oletetaan, että käyttöönoton tunnistetiedot on jo muodostettu hankkia FTP-yhteystietoja valitsemalla **asetukset** > **Ominaisuudet**ja kopioimalla **FTP/käyttöönoton käyttäjän**, **FTP-isäntänimi**ja **FTPS isäntänimi**arvot. Kopioi **FTP/käyttöönoton käyttäjän** käyttäjän arvon sellaisina kuin ne näkyvät Azure-portaali, mukaan lukien sovelluksen nimen, jos haluat säätää tietää FTP-palvelimeen.
2. Käytä FTP-asiakas muodostaa sovelluksen saadut yhteystiedot.
3. Tiedostojen ja niiden vastaaviin kansiorakenne kopioiminen Azure [ **/site/wwwroot** directory](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure) (tai **/sivuston/wwwroot/App_Data/työt/** WebJobs kansio).
4. Siirry sinua sovelluksen URL-Osoitteen vahvistamiseksi sovellus toimii oikein. 

Lisätietoja on artikkelissa seuraavaan resurssiin:

* [PHP MySQL-web-sovelluksen luominen ja ota käyttöön FTP: N avulla](web-sites-php-mysql-deploy-use-ftp.md).

## <a name="dropbox"></a>Cloud-kansion synkronoinnin käyttöönotto
[Tiedostojen kopioiminen manuaalisesti](#ftp) hyvä vaihtoehto on synkronoinnin tiedostojen ja kansioiden App palveluun pilvitallennuspalveluun, kuten OneDrive ja Dropbox. Synkronointi pilveen kansioon käyttämällä Kudu prosessin käyttöönottoa varten (katso [käyttöönoton prosesseista yleiskatsaus](#overview)).

On synkronoitu pilveen kansion ammattilaisille ovat seuraavat:

- Helppokäyttöisyyden käyttöönotto. Palvelujen, kuten OneDrive ja Dropbox antaa työpöydän Synkronoi asiakkaat, jolla paikallisen toimimasta-kansio on myös käyttöönotto-kansio.
- Yhdellä napsautuksella käyttöönotto.
- Kaikki Kudu käyttöönotto-ohjelma-toiminto on käytettävissä (esimerkiksi paketin Palauta, automaatio).

On synkronoitu pilveen kansion kuvakkeet ovat seuraavat:

- Peruuttaminen epäonnistuu toteutuessa ei Versionhallinta.
- Ei automaattista käyttöönoton manuaalinen synkronointi ei tarvita.

### <a name="howtodropbox"></a>Miten työkalun synkronointi pilven kansioon
[Azure-portaalissa](https://portal.azure.com)voit määrittää sisällön synkronointi kansion OneDrive tai Dropbox-pilvitallennustilaa, sovelluksen koodi ja-kansion sisältö ja synkronoida sovelluksen-palveluun napsauttamalla hiiren napsautuksella.

* [Synkronoi sisällön Azure-sovelluksen palveluun cloud-kansiosta](app-service-deploy-content-sync.md). 

## <a name="continuousdeployment"></a>Ottaa käyttöön jatkuvasti pilvipohjainen tietolähteen hallinta-palvelusta
Jos kehittäminen ryhmä käyttää pilvipohjainen lähde koodin Ohjelmistomääritysten hallintapalvelun kuten [Visual Studio Team Services](http://www.visualstudio.com/), [GitHub](https://www.github.com)tai [BitBucket](https://bitbucket.org/), voit määrittää sovelluksen palvelun integroida lisääminen säilöön ja otetaan käyttöön jatkuvasti. 

Ammattilaisille otat käyttöön pilvipohjainen tietolähteen hallinta-palvelusta on:

- Versionhallinta käyttöön palauttaminen.
- Mahdollisuus määrittää jatkuva käyttöönottoa varten Git (ja Mercurial tarvittaessa) säilöjen tietoihin. 
- Haara-kohtaisia käyttöönoton, voit ottaa käyttöön eri haaroja eri [paikkaa](web-sites-staged-publishing.md).
- Kaikki Kudu käyttöönotto-ohjelma-toiminto on käytettävissä (esimerkiksi käyttöönoton versiotietojen, peruutus, paketti palauttaminen, automaatio).

Con otat käyttöön pilvipohjainen tietolähteen hallinta-palvelusta on:

- Jotkin tuntemus vastaaviin palvelujen ohjauksen hallinta-palvelu edellyttää.

###<a name="vsts"></a>Käyttöönottamisesta jatkuvasti pilvipohjainen tietolähteen hallinta-palvelusta
[Azure-portaalissa](https://portal.azure.com)voit määrittää jatkuva käyttöönoton GitHub, Bitbucket ja Visual Studio Team Services.

* [Jatkuva käyttöönoton Azure sovelluksen-palveluun](app-service-continuous-deployment.md). 

## <a name="localgitdeployment"></a>Ottaa paikallisen Git
Jos kehittäminen ryhmä käyttää paikalliseen paikallisen lähteen koodin Ohjelmistomääritysten hallintapalvelu Git perusteella, voit määrittää tämän sovelluksen palvelun käyttöönoton lähteenä. 

Ammattilaisille, paikallinen Git käyttöönotto ovat seuraavat:

- Versionhallinta käyttöön palauttaminen.
- Haara-kohtaisia käyttöönoton, voit ottaa käyttöön eri haaroja eri [paikkaa](web-sites-staged-publishing.md).
- Kaikki Kudu käyttöönotto-ohjelma-toiminto on käytettävissä (esimerkiksi käyttöönoton versiotiedot, peruuttaminen, paketti palauttaminen, automaatio).

CON, paikallinen Git käyttöönotto on:

- Jotkin tuntemus vastaaviin palvelujen ohjauksen hallinta-järjestelmän pakollinen.
- Poista-näppäintä ratkaisua jatkuva käyttöönottoa varten. 

###<a name="vsts"></a>Voit ottaa paikallisen Git
[Azure-portaalissa](https://portal.azure.com)voit määrittää paikallisen Git käyttöönotto.

* [Paikallinen Git käyttöönoton Azure sovelluksen-palveluun](app-service-deploy-local-git.md). 
* [Julkaiseminen minkä tahansa git/hg repo Web-sovelluksia](http://blog.davidebbo.com/2013/04/publishing-to-azure-web-sites-from-any.html).  

## <a name="deploy-using-an-ide"></a>Ottaa käyttöön IDE käyttäminen
Jos käytät jo [Visual Studio](https://www.visualstudio.com/products/visual-studio-community-vs.aspx) [Azure SDK](https://azure.microsoft.com/downloads/)tai muita IDE-ohjelmistopaketit kuten [Xcode](https://developer.apple.com/xcode/) [Pimennys](https://www.eclipse.org)ja [IntelliJ VERRATA](https://www.jetbrains.com/idea/), voit ottaa käyttöön Azure suoraan yhteyttä IDE kuluessa. Tämä asetus sopii yksittäisiä developer.

Visual Studio tukee kaikki kolme käyttöönoton prosesseja (FTP Git ja Web käyttöön), sen mukaan, että asetus samalla, kun muut IDEs voit ottaa käyttöön sovelluksen palvelu, jos heillä on FTP- tai Git integrointi (katso [käyttöönoton prosesseista yleiskatsaus](#overview)).

Otat käyttöön käyttämällä IDE ammattilaisille ovat seuraavat:

- Pienentää mahdollisesti lopusta loppuun-sovelluksen elinkaarensa aikana, sillä. Kehittää, virheenkorjaus, seurata ja sovelluksen käyttöön Azure kaikki siirtymättä oman IDE ulkopuolella. 

Otat käyttöön käyttämällä IDE kuvakkeet ovat seuraavat:

- Lisätty monimutkaisuus sillä.
- Edellyttää edelleen ohjausobjektin järjestelmän työryhmän projektissa.

<a name="vspros"></a>Lisää ammattilaisille otat käyttöön Visual Studiossa käyttäminen Azure SDK ovat seuraavat:

- Azure SDK tekee Azure resurssien pääosin kansalaisten Visual Studiossa. Luo, poista, muokata, Käynnistä-painiketta, ja Lopeta sovellukset, kysely Taustajärjestelmä SQL-tietokantaan, live-virheenkorjaus Azure sovellus ja paljon muuta. 
- Reaaliaikainen Azure koodin tiedostojen muokkaamista.
- Reaaliaikainen Azure-muistin sovelluksia.
- Integroitu Azure Resurssienhallinnassa.
- Vain eroavuus käyttöönotto. 

###<a name="vs"></a>Voit ottaa käyttöön Visual Studio suoraan

* [Azure- ja ASP.NET käytön aloittaminen](web-sites-dotnet-get-started.md). Voit luoda ja asentavat yksinkertaisen ASP.NET MVC web projektin Visual Studio ja Web otetaan käyttöön.
* [Voit ottaa käyttöön Azure WebJobs Visual Studiossa](websites-dotnet-deploy-webjobs.md). Miten määritetään konsolisovelluksen projektien niin, että ne on otetaan käyttöön WebJobs.  
* [Suojatun ASP.NET MVC 5 sovelluksen jäsenyyden, OAuth-ja verkkosovelluksissa SQL-tietokannan käyttöönotto](web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md). Voit luoda ja asentavat ASP.NET MVC web-projekti on SQL-tietokanta, jossa Visual Studiossa, Web käyttöönottoa ja kohteen Framework koodin ensimmäisen siirrot.
* [ASP.NET Web käyttöönotto Visual Studiossa](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/introduction). 12-osan opetusohjelman sarjan, joka kattaa Lisätietoja solualueen käyttöönoton tehtävät kuin muiden tässä luettelossa. Azure käyttöönoton joitakin ominaisuuksia on lisätty, koska opetusohjelman on kirjoitettu, mutta muistiinpanoja lisättynä myöhemmin kerrotaan, mitä puuttuu.
* [ASP.NET-verkkosivustosta Visual Studio 2012 säilöstä Git suoraan Azure käyttöönotto](http://www.dotnetcurry.com/ShowArticle.aspx?ID=881). Kerrotaan, miten voit ottaa käyttöön Visual Studiossa, Vahvista koodi Git ja yhdistämisestä Azure Git säilöön laajennuksen Git avulla ASP.NET web-projekti. Visual Studio 2013: n alkaen Git tuki on valmiita ja laajennuksen asentaminen ei edellytä.

###<a name="aztk"></a>Azure-työkalupakettien käyttäminen Pimennys ja IntelliJ VERRATA ottamisesta

Microsoft tekee Web Apps-sovellusten käyttöön Azure suoraan Pimennys ja IntelliJ [Azure työkalujen Pimennys varten](../azure-toolkit-for-eclipse.md) ja [Azure Työkalut, IntelliJ](../azure-toolkit-for-intellij.md)kautta. Opetusohjelmassa kuvataan vaiheet, jotka osallistuvat käyttöönotossa yksinkertainen "" Hei Web App Azure käyttämällä joko IDE:

*  [Luo Hei maailma verkkosovellukseen Pimennys Azure varten](./app-service-web-eclipse-create-hello-world-web-app.md). Tässä opetusohjelmassa näytetään, miten varten Pimennys Azure-työkalujen avulla luodaan ja otetaan käyttöön Hei maailma verkkosovellukseen Azure varten.
*  [Luo Hei maailma verkkosovellukseen Azure IntelliJ varten](./app-service-web-intellij-create-hello-world-web-app.md). Tässä opetusohjelmassa näytetään, miten varten IntelliJ Azure-työkalujen avulla luodaan ja otetaan käyttöön Hei maailma verkkosovellukseen Azure varten.


## <a name="automate"></a>Komentorivin työkaluilla automatisoida käyttöönotto

* [Käyttöönoton MSBuild automatisointi](#msbuild)
* [Kopioi tiedostot, joiden FTP-työkalut ja komentosarjat](#ftp)
* [Automatisoida käyttöönoton Windows PowerShellin avulla](#powershell)
* [Automatisoida .NET hallinnan API käyttöönotto](#api)
* [Ottaa käyttöön azuren käyttöliittymä (Azure CLI)](#cli)
* [Web-käyttöön komentoriviltä käyttöönotto](#webdeploy)
* [FTP-komentosarjojen käyttäminen](http://support.microsoft.com/kb/96269).
 
Toinen käyttöönotto-vaihtoehto on käyttää pilvipohjaisia palveluja, kuten [Meritursas käyttöönotto](http://en.wikipedia.org/wiki/Octopus_Deploy). Lisätietoja on artikkelissa [käyttöönotto ASP.NET-sovelluksia, Azure sivustoja](https://octopusdeploy.com/blog/deploy-aspnet-applications-to-azure-websites).

###<a name="msbuild"></a>Käyttöönoton MSBuild automatisointi

Jos käytät [Visual Studio IDE](#vs) kehitystä, voit automatisoida mitään yhteyttä IDE mahdollisuuksista [MSBuild](http://msbuildbook.com/) . Voit määrittää MSBuild tiedostojen kopioiminen [Web ottaminen käyttöön](#webdeploy) tai [FTP/FTPS](#ftp) avulla. Web-käyttöönotto voit automatisoida myös monia muita käyttöönoton liittyviä tehtäviä, kuten tietokantoja käyttöönotto.

Saat lisätietoja komentorivin käyttöönottoa MSBuild on seuraavissa resursseissa:

* [ASP.NET Web käyttöönotto Visual Studiossa: komentoriviltä käyttöönoton](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/command-line-deployment). Tietoja käyttöönotto Visual Studiossa Azure opetusohjelmat numerosarjojen kymmenesosaan. Voit ottaa käyttöön, kun määrittäminen julkaissut profiilit Visual Studiossa komentorivin avulla.
* [Sisällä Microsoft muodosta-ohjelma: MSBuild ja ryhmän Foundation muodosta](http://msbuildbook.com/). Kova kopioi kirjan, joka sisältää luvun käyttämisestä MSBuild käyttöönottoa varten.

###<a name="powershell"></a>Automatisoida käyttöönoton Windows PowerShellin avulla

Voit suorittaa [Windows PowerShellin](http://msdn.microsoft.com/library/dd835506.aspx)MSBuild tai FTP käyttöönotto-funktiota. Jos teet näin, voit käyttää myös Windows PowerShellin cmdlet-komennot, jotka helpottavat Azure REST-hallinnan API Soita-apuohjelmista.

Lisätietoja on seuraavissa resursseissa:

* [Linkitetty GitHub säilöön web-sovelluksen käyttöönotto](app-service-web-arm-from-github-provision.md)
* [Valmistele web-sovelluksessa, jossa on SQL-tietokanta](app-service-web-arm-with-sql-database-provision.md)
* [Valmistele ja microservices laadukkaampi Azure-käyttöönotto](app-service-deploy-complex-application-predictably.md)
* [Rakennuksen tosielämän Cloud-sovellusten kanssa Azure - automatisoida kaikki](http://asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything). E-kirjan luvun, jossa kerrotaan, miten e-kirjan näkyvät sovelluksen malli käyttää Windows PowerShell-komentosarjojen luominen Azure testiympäristössä ja ota se käyttöön. Lisätietoja PowerShellin Azure lisäohjeita linkkejä [resurssit](http://asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything#resources) -osassa.
* [Windows PowerShell-komentosarjojen julkaiseminen keskihajonta ja testaa ympäristöissä käyttämällä](../vs-azure-tools-publishing-using-powershell-scripts.md). Miten Windows PowerShellin käyttäminen käyttöönoton komentosarjoja, joka luo Visual Studio.

###<a name="api"></a>Automatisoida .NET hallinnan API käyttöönotto

Voit kirjoittaa C#-koodin suorittamiseen MSBuild tai FTP-funktiota käyttöönottoa varten. Jos teet näin, voit käyttää Azure hallinta REST API sivuston hallinta toimintoihin.

Lisätietoja on artikkelissa seuraavaan resurssiin:

* [Automaattinen kaikki Azure hallinta kirjastojen ja .NET](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx). Johdanto .NET management API ja linkkejä lisätietoja.

###<a name="cli"></a>Ottaa käyttöön azuren käyttöliittymä (Azure CLI)

Voit komentorivin Windows, Mac tai Linux koneet FTP avulla. Jos teet näin, voit käyttää myös Azure REST-hallinnan Ohjelmointirajapinnan käyttäminen Azure-CLI.

Lisätietoja on artikkelissa seuraavaan resurssiin:

* [Azure-komennon viivatyökalun](/downloads/#cmd-line-tools). Komentorivin työkalun tiedot Azure.com portaalin sivu.

###<a name="webdeploy"></a>Web-käyttöön komentoriviltä käyttöönotto

[Web-käyttöön](http://www.iis.net/downloads/microsoft/web-deploy) on Microsoft-ohjelmiston IIS, paitsi tarjoaa älykkäät tiedoston synkronointiominaisuudet, mutta myös voit suorittaa tai järjestämiseen monia muita käyttöönoton liittyviä tehtäviä ei voi automatisoida, kun käytät FTP käyttöönottoa varten. Esimerkiksi Web käyttöönotto voi ottaa käyttöön uuden tietokannan tai tietokannan päivitykset-web-sovelluksen kanssa. Web-käyttöönotto myös pienentää aika, joka päivittää aiemmin luotuun sivustoon, koska se älykkäästi kopioida vain muuttuneet tiedostot. Microsoft Visual Studio ja Team Foundation Server on tuesta Web käyttöönotto valmiin, mutta voit käyttää myös Web käyttöön suoraan komentoriviltä automatisoida käyttöönotto. Web-käyttöönotto-komennot ovat hyvin tehokkaita mutta learning käyrän voi olla jyrkissä.

Lisätietoja on artikkelissa seuraavaan resurssiin:

* [Yksinkertainen verkkosovelluksissa: käyttöönoton](https://azure.microsoft.com/blog/2014/07/28/simple-azure-websites-deployment/). Blogin David Ebbo tietoja hän kirjoittamasi tiedostojasi on helpompi käyttää Web käyttöönotto työkalun avulla.
* [Web-käyttöönottotyökalun](http://technet.microsoft.com/library/dd568996). Virallinen asiakirjat Microsoft TechNet-sivustossa. Päivätty mutta edelleen hyvä alkavan.
* [Internet-käyttöön](http://www.iis.net/learn/publish/using-web-deploy). Virallinen asiakirjat Microsoft IIS.NET-sivustossa. Voit aloittaa mutta myös päivätty.
* [ASP.NET Web käyttöönotto Visual Studiossa: komentoriviltä käyttöönoton](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/command-line-deployment). MSBuild on Muodosta-ohjelma käyttää Visual Studio ja se voidaan myös komentoriviltä verkkosovellusten Web Apps-sovellusten käyttöön. Tässä opetusohjelmassa on osa sarjan, jossa on pääasiassa Visual Studio käyttöönotto.

##<a name="nextsteps"></a>Seuraavat vaiheet

Joissakin tilanteissa haluat ehkä voi vaihtaa helposti edestakaisin välillä väliaikainen ja sovelluksen tuotannon-versiota. Lisätietoja on kohdassa [Vaiheistettu Web Apps -sovellusten käyttöönoton](web-sites-staged-publishing.md).

Varmuuskopiointi ja palauttaminen suunnitelman paikassa on tärkeä osa käyttöönoton työnkulussa. Saat lisätietoja sovelluksen palvelun varmuuskopiointi ja palauttaminen ominaisuus on artikkelissa [Web Apps-varmuuskopiot](web-sites-backup.md).  

Saat lisätietoja Azure's Roolipohjainen käyttöoikeuksien valvonta avulla voit hallita niiden käyttöä App palvelun käyttöönoton [RBAC ja Web App-julkaisemisen](https://azure.microsoft.com/blog/2015/01/05/rbac-and-azure-websites-publishing/).


 

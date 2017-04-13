<properties
    pageTitle="Azure App palvelu, näennäiskoneiden, palvelun kangasta ja pilvipalveluihin vertailu | Microsoft Azure"
    description="Lue, miten Azure App palvelu, näennäiskoneiden, palvelun kangasta ja pilvipalveluihin isännöimiseen web-sovellusten välillä."
    services="app-service\web, virtual-machines, cloud-services"
    documentationCenter=""
    authors="tdykstra"
    manager="wpickett"
    editor="jimbe"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/07/2016"
    ms.author="tdykstra"/>

# <a name="azure-app-service-virtual-machines-service-fabric-and-cloud-services-comparison"></a>Azure App palvelu, näennäiskoneiden, palvelun kangasta ja pilvipalveluihin vertailu

## <a name="overview"></a>Yleiskatsaus

Azure tarjoaa useita tapoja host sivustoihin: [Azure App palvelu][], [näennäiskoneiden][], [Palvelun kangasta][]ja [Pilvipalveluihin][]. Tässä artikkelissa auttaa ymmärtämään asetukset ja tee web-sovelluksen sopiva vaihtoehto.

Azure sovelluksen-palvelu on paras vaihtoehto useimmat verkkosovelluksissa. Verkkosivuston käyttöönotto- ja on integroitu platform sivustojen skaalata nopeasti käsittelemään suuri tietoliikenne latautuu ja valmiin kuormituksen tasaamisen ja liikenteen hallinta antaa suuren käytettävyyden. Voit siirtää aiemmin luoduissa sivustoissa Azure-sovelluksen palveluun helposti [online siirtotyökalun](https://www.migratetoazure.net/), Web-sovelluksen valikoiman Avaa lähde-sovelluksen käyttäminen tai framework ja työkalut valittua uuden sivuston luominen. [WebJobs][] -ominaisuus on helppo lisätä taustan työn käsittely App palvelun web App-sovellukseen.

Palvelun kangasta on hyvä vaihtoehto, jos luot uuden sovelluksen tai uudelleen kirjoittaminen olemassa olevan sovelluksen käyttämään microservice arkkitehtuuri. Sovellukset, jotka suoritetaan jaetun resurssivarantoon tietokoneissa, voit aloittaa pieni ja kasvaa valtaviin asteikkoa, jossa satoja tai tuhansia koneet tarpeen mukaan. Tilallisten palveluiden avulla on helppo johdonmukaisesti ja luotettavasti tallentaa sovelluksen tila ja palvelun kangasta hallitsee automaattisesti palvelun jakaminen, skaalaus ja käytettävyyden puolestasi.  Palvelun kangasta tukee myös WebAPI Avaa Web-liittymällä .NET (OWIN) ja ASP.NET Core.  Verrattuna App Service-palvelun kangasta sisältää myös enemmän hallintaoikeutta tai suora pääsy, pohjana oleva infrastruktuuri. Voit remote kyselyjä palvelinten tai määrittää palvelimen käynnistys tehtävät. Pilvipalveluihin muistuttaa palvelun kangasta aste ohjausobjektin helppous, ja valitse, mutta se on nyt vanha palvelu ja palvelun kangasta suositellaan uusi kehittäminen.

Jos sovellukseen, jotka vaativat suorittaminen palvelun sovelluksen tai palvelun kangasta merkittäviä muutoksia, jotta voit yksinkertaistaa siirtyminen pilveen voi valita näennäiskoneiden. Kuitenkin oikein määrittäminen, suojaaminen ja ylläpitoon VMs edellyttää paljon aikaa ja IT-osaamisalueet Azure App palveluun ja palvelun kangasta verrattuna. Jos harkitset Azuren näennäiskoneiden, varmista, että voit ottaa huomioon jatkuvaa ylläpitoa työmäärään tarvitse korjaustiedoston, päivittäminen ja hallinta AM-ympäristössä. Azuren näennäiskoneiden on infrastruktuurin nimellä--palvelun (IaaS)-sovelluksen palvelun ja palvelun kangasta on Platform nimellä--palvelun (Paas). 

## <a name="features"></a>Ominaisuuksien vertailu

Seuraavassa taulukossa on Vertailtu App palvelu, pilvipalveluihin, näennäiskoneiden ja palvelun kangasta ominaisuuksia tehdä paras valinta. Kunkin asetuksen SLA tietoja on artikkelissa [Azure palvelun tason sopimuksia](/support/legal/sla/).

Toiminto|Sovelluksen Service (online)|Cloud Services (web roolit)|Näennäiskoneiden|Palvelun kangasta|Huomautuksia
---|---|---|---|---|---
Pikaviestikeskustelun lähellä käyttöönotto|X|||X|Sovelluksen tai sovelluksen päivityksen käyttöönotto pilvipalveluun tai luomalla AM, kestää useita minuutteja vähintään; sovelluksen verkkosovellukseen käyttöönotto kestää sekuntia.
Skaalaa suurempi koneet ilman laukaisun|X|||X|
Web server esiintymät Jaa sisältöä ja määritykset, mikä tarkoittaa, että sinun ei tarvitse ottaa uudelleen tai määrittää uudelleen, kun kokoa.|X|||X|
Useita käyttöönotto-ympäristössä (tuotannon ja väliaikaisen)|X|X||X|Palvelun kangasta mahdollistaa on useita sovelluksia ympäristöissä tai ottaessaan yhteyttä sovelluksen-rinnakkais eri versioita.
Automaattinen OS päivitysten hallinta|X|X|||OS Automaattiset päivitykset on suunniteltu varten tulevien palvelun kangasta vapauttamista.
Saumaton ympäristö vaihtaminen (helposti siirtymään 32-bittinen ja 64-bittinen)|X|X|||
Ottaa käyttöön koodia GIT, FTP kanssa|X||X||
Ota käyttöön-koodia, jossa on Web käyttöönotto|X||X||Pilvipalveluihin tukee Web käyttöönotto päivitykset käyttöön yksittäisiä roolin esiintymät. Voi käyttää sitä ensimmäisen käyttöönoton roolin, ja jos käytössäsi Web käyttöön päivitys on roolin jokaisen esiintymän erikseen käyttöön. Useita kertoja vaatii ovat oikeutettuja Cloud palvelun SLA tuotannon ympäristössä.
WebMatrix tuki|X||X||
Käyttää palveluita, kuten palvelun Bus, tallennustilan tai SQL-tietokantaan|X|X|X|X|
Host (isäntä)-web- tai web services taso monitasoisten-arkkitehtuuri|X|X|X|X|
Host Keskitaso, monitasoisten-arkkitehtuuri|X|X|X|X|Sovelluksen palvelun verkkosovelluksissa helposti ylläpitää REST API Keskitaso ja [WebJobs](http://go.microsoft.com/fwlink/?linkid=390226) -ominaisuus ylläpitää käsittelyn työt tausta. Voit suorittaa WebJobs on oma sivusto riippumaton skaalattavuus tason saavuttamiseksi. Esikatselu [API sovellukset](../app-service-api/app-service-api-apps-why-best-platform.md) -toimintoa on entistä enemmän toimintoja REST-isännöintipalveluja varten.
Integroitu MySQL-muodossa--palvelun tuki|X|X|X||Pilvipalveluihin integroida MySQL-muodossa--palvelun kautta ClearDB's tarjouksia, mutta Azure Portal-työnkulun osana.
ASP Node.js, PHP, Python ASP.NET-perinteinen tuki|X|X|X|X|Palvelun kangasta tukee edusta sivuston luomisen [ASP.NET-5](../service-fabric/service-fabric-add-a-web-frontend.md) tai voit ottaa käyttöön kaikenlaisia sovelluksen (Node.js, Java jne.) [Vieras suoritettavan](../service-fabric/service-fabric-deploy-existing-app.md).
Skaalaa ulos ilman laukaisun useita kertoja|X|X|X|X|Näennäiskoneiden voi skaalata useita kertoja, mutta ne palvelut on kirjoitettava käsittelemään tämän asteikko-kohtaa. Sinun on määritettävä kuormituksen reitittää pyynnöt tietokoneissa ja estää kaikki esiintymät ylläpito tai laitteiston virheiden vuoksi samanaikaisesti käynnistyy affiniteetti-ryhmän luominen.
SSL-tuki|X|X|X|X|Sovelluksen palvelun verkkosovelluksissa SSL-salausta mukautettuja toimialuenimiä tuetaan vain Basic ja vakio-tilassa. Lisätietoja SSL käyttäminen web Apps-sovelluksista on artikkelissa [Azure-sivuston SSL-varmenteen määrittäminen](../app-service-web/web-sites-configure-ssl-certificate.md).
Visual Studio-integrointi|X|X|X|X|
Remote virheenkorjaus|X|X|X||
Ottaa käyttöön koodia TFS kanssa|X|X|X|X|
Verkon eristystaso [Azure Virtual verkoston](/services/virtual-network/) kanssa|X|X|X|X|Katso myös [Azure sivustojen VPN-integrointi](/blog/2014/09/15/azure-websites-virtual-network-integration/)
Tuki [Azure liikenteen hallinta](/services/traffic-manager/)|X|X|X|X|
Integroitu päätepisteen seuranta|X|X|X||
Työpöydän etäkäyttöpalvelimen palvelimiin||X|X|X|
Asenna kaikki mukautetut MSI||X|X|X|Palvelun kangasta voit isännöidä minkä tahansa suoritettavan tiedoston [Vieras suoritettavan](../service-fabric/service-fabric-deploy-existing-app.md) tai voit asentaa VMs minkä tahansa sovelluksen.
Mahdollisuus määrittää ja suorita pysäyttämisestä tehtävät||X|X|X|
Voit kuunnella tapahtumien seuranta tapahtumat||X|X|X|

## <a name="scenarios"></a>Skenaariot ja suositukset

Tässä on muutamia yleisiä sovelluksen skenaarioita suositusten siitä, mitä Azure WWW-isännöinnin vaihtoehto voi olla sopivimman kunkin.

- [Web-edusta tausta käsittelyn ja tietokannan Taustajärjestelmä paikalliseen varat on integroitu business-sovelluksia ja määritettävä.](#onprem)
- [On luotettava tapa, joka skaalaa hyvin yrityksen sivuston isännöintiin ja tarjouksia yleinen saavuttaa.](#corp)
- [Minulla on käynnissä Windows Server 2003 IIS6 sovellus.](#iis6)
- [Olen small business-omistaja, ja Oma sivusto edullinen tapa on mutta tulevien kasvu mielessä.](#smallbusiness)
- [Olen web tai kuvan suunnittelussa ja haluan voit suunnitella ja luoda sivustoja Omat asiakkaille.](#designer)
- [Monitasoisten edusta Web sovelluksessa siirtyminen pilveen käsittelyä.](#multitier)
- [Oma riippuu erittäin mukautetun Windows tai Linux ympäristöissä ja haluat siirtää pilveen.](#custom)
- [Oma sivusto käyttää Avaa Lähdeohjelmisto ja haluan isännöintiin Azure-tietokannassa.](#oss)
- [Minulla on liiketoiminta-sovellus, jossa on yrityksen verkkoon.](#lob)
- [Haluan isännöintiin REST API tai web-palveluun mobiiliasiakkaille.](#mobile)


### <a id="onprem"></a>Web-edusta tausta käsittelyn ja tietokannan Taustajärjestelmä paikalliseen varat on integroitu business-sovelluksia ja määritettävä.

Azure sovelluksen-palvelu on erinomainen ratkaisu monimutkaisia yrityssovellusten. Sen avulla voit kehittää sovellukset, jotka skaalata automaattisesti kuormituksen tasapainoinen ympäristössä, on suojattu Active Directory-hakemistosta ja muodosta yhteys paikalliseen resurssit. Tekee helposti – tehokas portal ja ohjelmointirajapinnan näiden sovellusten hallinta- ja mahdollistaa Hanki tietoja siitä, miten asiakkaat ovat käyttänyt niitä sovelluksen tietoja työkaluilla. [Webjobs][] -toiminnon avulla voit suorittaa taustaprosessit ja tehtävien osana web-tason aikana hybrid yhteys- ja VNET ominaisuudet helpottavat muodostaa yhteys paikalliseen resurssit. Azure sovelluksen-palvelu sisältää kolme 9 SLA web-sovellusten ja avulla voit:

* Suorita sovellustesi luotettavasti itsekorjautumisominaisuudet, automaattinen korjaaminen cloud-ympäristössä.
* Skaalaa automaattisesti palvelinkeskusten yleinen verkossa.
* Varmuuskopiointi ja palauttaminen palauttaminen.
* Ole ISO, SOC2 ja PCI yhteensopiva.
* Integroi Active Directory-hakemistosta

### <a id="corp"></a>On luotettava tapa, joka skaalaa hyvin yrityksen sivuston isännöintiin ja tarjouksia yleinen saavuttaa.

Azure sovelluksen-palvelu on erinomainen ratkaisu isännöinnin yrityksen sivustot. Ottaa käyttöön web Apps-sovellusten skaalata nopeasti ja helposti täyttävän demand palvelinkeskusten yleinen verkossa. Se on paikallinen reach vikasietoa ja älykkäät liikenteen hallinta. Kaikki-ympäristössä, jossa on tehokas hallintatyökaluja joilla voit Hanki kuntotietojen ja sivuston liikenteen tietoja nopeasti ja helposti. Azure sovelluksen-palvelu sisältää kolme 9 SLA web-sovellusten ja avulla voit:

* Suorita oman sivuston luotettavasti itsekorjautumisominaisuudet, automaattinen korjaaminen cloud-ympäristössä.
* Skaalaa automaattisesti palvelinkeskusten yleinen verkossa.
* Varmuuskopiointi ja palauttaminen palauttaminen.
* Voit hallita lokit ja liikenteen integroitu työkaluilla.
* Ole ISO, SOC2 ja PCI yhteensopiva.
* Integroi Active Directory-hakemistosta

### <a id="iis6"></a>Minulla on käynnissä Windows Server 2003 IIS6 sovellus.

Azure sovelluksen-palvelu on helppo välttämiseksi infrastruktuurin siirretään vanhojen IIS6 sovellusten liittyvät kustannukset. Microsoft on luonut [helppokäyttöisiä siirron työkalut ja siirron yksityiskohtaiset ohjeet](https://www.movemetowebsites.net/) , joiden avulla yhteensopivuuden tarkistaminen ja löytää kaikki muutokset, jotka on tehtävä. Visual Studio, TFS ja yleisiä CMS työkaluja integrointi on helppo IIS6 sovellusten suoraan pilveen. Kun tiedot on käytössä, Azur-portaalissa on tehokkaat hallintatyökalut, joiden avulla skaalata kustannusten hallinta ja ylöspäin täyttävät vaatimus tarpeen mukaan. Siirtotyökalun avulla voit:

* Siirtää vanhat Windows Server 2003-verkkosovelluksen pilveen nopeasti ja helposti.
* Halutessaan jättää oman liitetyn SQL tietokanta paikallinen hybrid-sovelluksen luominen.
* Siirtää SQL-tietokantaan sekä vanhojen sovellusten automaattisesti.

### <a id="smallbusiness"></a>Olen pienyrityksen omistaja, ja Oma sivusto edullinen tapa on mutta tulevien kasvu mielessä.

Azure sovelluksen-palvelu on erinomainen ratkaisu tässä tilanteessa, sillä voit kokeilla sitä käytännössä maksutta ja lisää sitten muiden ominaisuuksien, kun tarvitset niitä. Kunkin vapaa web-sovelluksen sisältyy Azure myöntämä toimialue (*your_company*. azurewebsites.net), ja ympäristö on integroitu verkkosivuston käyttöönotto- ja työkalut sekä sovelluksen-valikoima, joiden avulla pääset alkuun. On useita services- ja Skaalausasetusten, jotka sallivat käyttäjien demand myötä sivuston. Azure-sovelluksen ominaisuudet voit:

- Alkavat vapaa taso ja sitten skaalata haluamallasi tavalla.
- Sovelluksen valikoiman avulla voit nopeasti määrittää suosittujen Internet-sovelluksia, kuten WordPress.
- Lisää Azure lisäpalveluja ja toiminnot sovelluksen tarpeen mukaan.
- Suojatun web-sovelluksen kanssa HTTPS.

### <a id="designer"></a>Olen web tai kuvan suunnittelussa ja haluan nyt suunnitteleminen ja rakentaminen sivustojen asiakkaan

Web-kehittäjille ja suunnittelijoiden Azure App palvelun integroituu helposti saatavilla kehysten ja työkalut, sisältää käyttöönoton tuen Git ja FTP ja tarjoaa integrointi työkalujen ja palvelujen, kuten Visual Studio ja SQL-tietokantaan. Sovelluksen-palvelun avulla voit:

- Komentorivin työkaluilla [Automaattinen]tehtävien[scripting].
- Suositut kieliä, kuten [.net]käyttäminen[dotnet], [PHP][], [Node.js][nodejs], ja [Python][].
- Valitse kolme eri skaalauksen tasoa skaalauksen erittäin suuri valmiuksia.
- Integroi muita Azure palveluja, kuten [SQL-tietokantaan][sqldatabase]- [Palvelun Bus] [ servicebus] ja [tallennustilaa][]tai kumppanin tarjouksia [Azure-kaupan][azurestore], kuten MySQL ja MongoDB.
- Integroi työkaluilla, kuten Visual Studiossa, Git, WebMatrix, WebDeploy, TFS ja FTP.

### <a id="multitier"></a>Monitasoisten edusta Web sovelluksessa siirtyminen pilveen 'M

Jos käytössäsi on monitasoisten-sovellus, esimerkiksi verkkopalvelimeen, joka muodostaa yhteyden tietokantaan, sovelluksen Azure-palvelu on hyvä vaihtoehto, joka tarjoaa integrointi Azure SQL-tietokantaan. Ja WebJobs-toiminnolla Taustajärjestelmä prosessien suorittamista varten.

Valitse palvelun kangasta yhden tai useamman oman tasoa, jos tarvitset lisää server-ympäristössä, kuten mahdollisuus remote kyselyjä palvelimellesi hallintaa tai määrittää palvelimen käynnistys tehtävät.

Valitse näennäiskoneiden yhden tai useamman oman tasoa, jos haluat käyttää tietokoneen-kuvaa tai suorittaa Palvelinohjelmisto tai palvelut, joita ei voi määrittää palvelun kangasta.

### <a id="custom"></a>Oma riippuu erittäin mukautetun Windows tai Linux ympäristöissä ja haluat siirtää pilveen.

Jos sovellus edellyttää monimutkaisia asennus tai ohjelmiston ja käyttöjärjestelmän, näennäiskoneiden on todennäköisesti paras mahdollinen ratkaisu. Näennäiskoneiden voit tehdä seuraavia toimia:

- Aloita käyttöjärjestelmä, kuten Windows- tai Linux-virtuaalikoneen valikoiman avulla ja mukautetaan sovelluksen vaatimukset.
- Luo ja lataa aiemmin luotu paikallisen palvelimen suorittamaan virtual Azure-koneeseen mukautetun kuvan.

### <a id="oss"></a>Oma sivusto käyttää Avaa lähde-ohjelmiston ja haluan nyt isännöidä Azure-tietokannassa

Jos Avaa lähde-framework tuetaan sovelluksen-palveluun, kielet, kehysten sovelluksen tarvitsemia määritetään automaattisesti puolestasi. Sovelluksen-palvelun avulla voit:

- Käytä monet Suositut Avaa lähde kieliä, kuten [.NET][dotnet], [PHP][], [Node.js][nodejs], ja [Python][].
- Määritä WordPress, Drupal, Umbraco, DNN ja monet muut kolmannen osapuolen web-sovellukset.
- Siirtää aiemmin luodun sovelluksen tai luoda uuden sovelluksen valikoimasta.

Jos Avaa lähde-framework ei tue sovelluksen-palvelussa, voit suorittaa sen johonkin muiden Azure verkossa isännöinnin asetukset. Näennäiskoneiden, jossa voit asentaa ja määrittää ohjelmiston machine-kuva, joka voi olla Windows- tai Linux-pohjaiset.

### <a id="lob"></a>Minulla on liiketoiminta-sovellus, jossa on yrityksen verkkoon

Jos haluat luoda liiketoiminta-sovelluksen, sivuston saattaa edellyttää suora pääsy Services-palveluiden vai tietojen yrityksen verkossa. Tämä on mahdollista App palvelu, palvelun kangasta ja [Azure Virtual verkkopalvelu](/services/virtual-network/)näennäiskoneiden. Sovelluksen-palvelussa voit [VNET integrointi ominaisuus](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/), jonka avulla Azure sovelluksia aivan kuin yhtä yritysverkossa.

### <a id="mobile"></a>Haluan isännöintiin REST API tai web-palveluun mobiiliasiakkaille

HTTP-pohjainen web-palvelujen avulla voit tukea asiakasohjelmilla, kuten mobiilisovellukset erilaisia. ASP.NET-verkko-Ohjelmointirajapinnan kuten kehysten integroi Visual Studio helpompi luoda ja käyttää REST-palveluita.  Näiden palvelujen niitä julkaista web-päätepisteen, joten niitä ei voi käyttää minkä tahansa WWW-isännöinnin Azure-tekniikka tukemaan Tämä skenaario. Kuitenkin App palvelu on isännöimiseen REST API. Sovelluksen-palvelun avulla voit:

- [Mobiilisovelluksen](../app-service-mobile/app-service-mobile-value-prop.md) luominen nopeasti tai [API app](../app-service-api/app-service-api-apps-why-best-platform.md) isännöimiseen HTTP-verkkopalvelun jossakin Azure on yleisesti jaetaan palvelinkeskusten.
- Siirtää olemassa olevia palveluja tai luoda uusia.
- Saavuttamiseksi SLA käytettävyyden yksittäisen esiintymän kanssa tai skaalata useita erillinen koneet.
- Julkaistun sivuston avulla voit antaa REST API minkä tahansa HTTP-asiakasohjelmilla, kuten mobiilisovellukset.

> [AZURE.NOTE]
> Jos haluat aloittaa Azure App palvelun ennen tilin käyttäjäksi, siirry <a href="https://trywebsites.azurewebsites.net/">https://trywebsites.azurewebsites.net</a>-kohtaa, johon voit heti luoda lyhytkestoinen starter-sovellus App Azure-palvelu maksutta. Ei ole luottokortti, ei ole sitoumukset.

## <a id="nextsteps"></a>Seuraavat vaiheet

Jos haluat lisätietoja kolme WWW-isännöintipalvelu asetukset on artikkelissa [Azure esittely](../fundamentals-introduction-to-azure.md).

Aloita sovelluksen valitset kokousasetukset-kohdassa on seuraavissa resursseissa:

* [Azure sovelluksen-palvelu](/documentation/services/app-service/)
* [Azure pilvipalveluihin](/documentation/services/cloud-services/)
* [Azure-Virtuaalikoneissa](/documentation/services/virtual-machines/)
* [Palvelun kangasta](/documentation/services/service-fabric)

<!-- URL List -->

[Azure sovelluksen-palvelu]: /services/app-service/
[Pilvipalveluihin]: http://go.microsoft.com/fwlink/?LinkId=306052
[Näennäiskoneiden]: http://go.microsoft.com/fwlink/?LinkID=306053
[Palvelun kangasta]: /services/service-fabric
[ClearDB]: http://www.cleardb.com/
[WebJobs]: http://go.microsoft.com/fwlink/?linkid=390226&clcid=0x409
[Configuring an SSL certificate for an Azure Website]: http://www.windowsazure.com/develop/net/common-tasks/enable-ssl-web-site/
[azurestore]: http://www.windowsazure.com/gallery/store/
[scripting]: http://www.windowsazure.com/documentation/scripts/?services=web-sites
[dotnet]: http://www.windowsazure.com/develop/net/
[nodejs]: http://www.windowsazure.com/develop/nodejs/
[PHP]: http://www.windowsazure.com/develop/php/
[Python]: http://www.windowsazure.com/develop/python/
[servicebus]: http://www.windowsazure.com/documentation/services/service-bus/
[sqldatabase]: http://www.windowsazure.com/documentation/services/sql-database/
[Tallennustilan]: http://www.windowsazure.com/documentation/services/storage/

<!-- IMG List -->

[ChoicesDiagram]: ./media/choose-web-site-cloud-service-vm/Websites_CloudServices_VMs_3.png

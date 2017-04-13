<properties
    pageTitle="Mikä on Azure .NET SDK"
    description="Katso, mitkä tuotteet sisältyvät Azure .NET SDK-paketissa."
    documentationCenter=".net"
    authors="tdykstra"
    manager="wpickett"
    editor="mollybos"
    services=""/>

<tags
    ms.service="multiple"
    ms.workload="multiple"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/30/2016"
    ms.author="rachelap"/>

# <a name="what-is-the-azure-sdk-for-net"></a>Mikä on Azure SDK .NET?

## <a name="overview"></a>Yleiskatsaus

Azure SDK .NET on Visual Studio työkaluja, komentorivin työkaluja, runtime binaaritiedostot ja asiakas-kirjastoja, jotka auttavat kehittämään, testaus ja asentaa sovellukset, jotka suoritetaan Azure-tietokannassa. Tässä artikkelissa kuvataan, näkyviin tulee asentaa SDK. Voit ladata SDK [Azure-lataussivulta](https://azure.microsoft.com/downloads/).

Azure SDK .NET käsittää myös [vievää Azure services client kirjastoissa](http://go.microsoft.com/fwlink/?LinkId=510472). Nämä kirjastot on asennettu erikseen NuGet avulla.

##<a id="included"></a>Mitä sisältyy .NET Azure SDK: ssa

Azure SDK .NET asentaa seuraavia tuotteita:

- [Visual Studio yhteisön Edition 2015](#vwd)
- [Microsoft Azure-tallennustilan emulaattorin](#stgemulator)
- [Microsoft Azure-tallennustilan Työkalut](#stgtools)
- [Microsoft Azure yhtä aikaa muiden kanssa-Työkalut](#auth)
- [Microsoft Azure emulaattorin](#emulator)
- [HDInsight Tools for Visual Studio ja Microsoft rakenne ODBC-ohjain](#hdinsight)
- [.NET Microsoft Azure-kirjastoissa](#libraries)
- [Microsoft Azure Mobile sovelluksen SDK-paketissa](#mobile)
- [Microsoft Azure PowerShell](#ps)
- [Microsoft Visual Studio Microsoft Azure-Työkalut](#tools)
- [Microsoft ASP.NET- ja Visual Studio Web-työkalut](#wte)
- [Microsoft Azure tietojen järvi Tools for Visual Studio](#datalake)

###<a id="vwd"></a>Visual Studio yhteisön Edition 2015

Jos tietokoneessa ei ole Visual Studiossa, SDK Asenna [Visual Studio yhteisön Edition 2015](https://www.visualstudio.com/products/visual-studio-community-vs).

###<a id="stgemulator"></a>Microsoft Azure-tallennustilan emulaattorin

[Azure-tallennustilan emulaattori](http://msdn.microsoft.com/library/hh403989.aspx) käyttää SQL Server-esiintymän ja paikallisen tiedostojärjestelmän voit simuloida Azuren tallennustilaan (olevien, taulukot, BLOB), jotta voit testata paikallisesti.

###<a id="stgtools"></a>Microsoft Azure-tallennustilan Työkalut

Tämä toiminto asentaa [AzCopy](http://aka.ms/AzCopy), voit siirtää tietoja sisään ja ulos Azure-tallennustilan tilin komentorivin työkalu.

###<a id="auth"></a>Microsoft Azure yhtä aikaa muiden kanssa-Työkalut

Tämä sisältää seuraavat:

* [CSPack komentorivin työkalun](http://msdn.microsoft.com/library/gg432988.aspx) käyttöönoton pakettien luomiseen.
* [CSEncrypt työkalun](http://msdn.microsoft.com/library/hh404001.aspx) salaamiseen salasanoja, joita käytetään käyttämään cloud palvelun roolin esiintymät Etätyöpöytäyhteys kautta.
* Suorituksenaikainen binaaritiedostot, cloud palvelun projektien edellyttävät yhteydenpito niiden suorituksenaikainen-ympäristön ja vianmääritys. Nämä binaaritiedostot eivät ole käytettävissä NuGet paketit.

###<a id="emulator"></a>Microsoft Azure emulaattorin

[Azure emulaattorin](http://msdn.microsoft.com/library/dn339018.aspx) varmistaminen palvelun cloud-ympäristössä niin, että voit testata cloud palvelun projektien paikallisesti tietokoneeseen ennen kuin otat ne käyttöön Azure.

###<a id="hdinsight"></a>HDInsight Tools for Visual Studio ja Microsoft rakenne ODBC-ohjain

Palvelimen Explorerissa Työkalut HDInsight avulla voit rakenne-tietokantojen ja linkitetyt tallennustilan tilit HDInsight klustereiden siirtyminen, luoda taulukoita, ja luoda ja lähettää rakenteen kyselyt. Lisätietoja on artikkelissa [käyttäminen HDInsight Hadoop Tools for Visual Studio](hdinsight/hdinsight-hadoop-visual-studio-tools-get-started.md).

###<a id="libraries"></a>.NET Microsoft Azure-kirjastoissa

Tämä sisältää seuraavat:

* Azure-tallennustilan, palvelun Bus ja välimuisti NuGet paketteja, jotka on tallennettu tietokoneeseen, niin, että Visual Studio voit luoda uuden cloud palvelun projektien samalla, kun offline-tilassa.
* Visual Studio laajennus, jonka avulla ja käyttää sitä paikallisesti Visual Studiossa projektit [- Roolin välimuisti](http://msdn.microsoft.com/library/dn386103.aspx) .

###<a id="mobile"></a>Microsoft Azure Mobile sovelluksen SDK-paketissa

[Azure App palvelun Mobile-sovellusten](app-service-mobile/app-service-mobile-value-prop.md)käyttäminen työkaluja.

###<a id="ps"></a>Microsoft Azure PowerShell

Azure PowerShellin avulla voidaan [automatisoida Azure ympäristön luominen ja käyttöönotto](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything).

###<a id="tools"></a>Microsoft Visual Studio Microsoft Azure-Työkalut

Näin voit käsitellä Azure resurssit, ensisijaisesti Pilvipalveluiden ja näennäiskoneiden:

* [Luo, Avaa, ja julkaise cloud palvelun projektit](cloud-services/cloud-services-dotnet-get-started.md).
* [Luo käyttöönoton pakettien cloud palvelun projekteissa](http://msdn.microsoft.com/library/ff683672.aspx).
* [Luo Azuren näennäiskoneiden-valintaruudun luotaessa uusia web-projektit](virtual-machines/virtual-machines-windows-classic-web-app-visual-studio.md).
* [Luo PowerShell-komentosarjojen luotaessa uusia näennäiskoneiden](http://msdn.microsoft.com/library/dn642480.aspx).
* [Näkymä ja hallita cloud projektin asetukset Visual Studio projektin ominaisuuksien Windowsin](http://msdn.microsoft.com/library/ee405486.aspx).
* Tarkastella ja hallita [pilvipalveluihin](http://msdn.microsoft.com/library/ff683675.aspx) [näennäiskoneiden](http://msdn.microsoft.com/library/jj131259.aspx)ja [Palvelun Bus](http://msdn.microsoft.com/library/jj149828.aspx) palvelimen Resurssienhallinnassa.
* [Suorita virheenkorjaus tilassa etäyhteyden pilvipalveluihin ja näennäiskoneiden varten](http://msdn.microsoft.com/library/ff683670.aspx).
* [Automatisoida resurssin valmistelu käyttämällä Azure resurssin Ryhmätöiden käyttöönotto](https://msdn.microsoft.com/library/dn872471.aspx)

###<a id="wte"></a>Microsoft-sovelluksen palvelun Tools for Visual Studio

Näin voit Azure-sivuston käyttäminen:

* [Julkaise web-projektit Azure-sivustoihin](app-service-web/web-sites-dotnet-get-started.md).
* [Julkaise konsolin sovelluksen projekteja Azure WebJobs](app-service-web/websites-dotnet-deploy-webjobs.md).
* [Luo Azure verkkosivusto- ja SQL-tietokantaan resurssien aikana web uuden projektin luominen tai verkko-projektin julkaiseminen](app-service-web/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md).
* [Luo PowerShell käyttöönotto-komentosarjojen luotaessa uusia sivustoja](http://msdn.microsoft.com/library/dn642480.aspx).
* [Hallinta ja vianmääritys Azure sivustot palvelimen Explorer](app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#sitemanagement).
* [Suorita etäyhteyden välityksellä, sivustoja ja WebJobs virheenkorjaus-tilassa](app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug).

>[AZURE.NOTE] Sinun ei tarvitse asentaa .NET, voit käyttää näitä toimintoja; Azure SDK myös ne sisältyisivät Visual Studio-päivitykset.

##<a id="datalake"></a>Microsoft Azure tietojen järvi Tools for Visual Studio

Lisätietoja on artikkelissa [Opetusohjelma: kehittää U-SQL-komentosarjoja järvi Datatyökalut Visual Studio](data-lake-analytics/data-lake-analytics-data-lake-tools-get-started.md).

##<a id="notincluded"></a>Mikä on sisälly, kun asennat Azure SDK .NET

On muutamia asioita, joita haluat ehkä Azure kehittämiseen, jotka eivät sisälly SDK: N asennuksen yhteydessä. Tärkeimmät nämä ovat seuraavat:

* [Asiakkaan kirjastoihin](http://go.microsoft.com/fwlink/?LinkId=510472).

    Azure-SDK sisältää käyttäjäprofiilipalvelun Azure services client kirjastot, mutta ei kaikkia ei asennettu SDK: N asennuksen yhteydessä. Jos sovellus täytyy asiakkaan kirjastossa, jossa SDK: N asentaminen ei onnistu, saat sen [NuGet.org](http://go.microsoft.com/fwlink/?LinkId=510472). Jos sovellus käyttää asiakkaan kirjastossa, jossa SDK: N asentaminen, se on hyvä päivittää sen uusimpaan versioon osoitteessa NuGet.org.

    **Asiakkaan kirjastojen paikalliset kopiot.** Azure SDK .NET kopioi tietokoneeseen joidenkin Azure asiakkaan kirjastojen, kuten tallennuskiintiön, palvelun Bus ja välimuisti NuGet paketit. Asiakkaan nämä kirjastot sisällytetään automaattisesti uuden cloud-palvelun projektit-niin paikallisen NuGet pakettien käyttöön Visual Studio projektien luominen, vaikka et ole yhteydessä Internetiin. Asiakkaan kirjastojen yleensä päivitetään useammin julkaistaan uusia SDK-versioita, niin osoitteessa NuGet.org asiakas-kirjastot ovat usein nykyisen kuin Hae SDK: N kanssa.

    **Project-mallit, jotka sisältävät asiakkaan kirjastoihin.** Vain [Azure pilvipalvelussa](cloud-services/cloud-services-dotnet-get-started.md) ja Azure App palvelun [Mobiilisovellukset](./app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) projektimallit Sisällytä asiakkaan Joidenkin kirjastojen automaattisesti. Asenna [asiakkaan kirjaston NuGet paketteja](http://go.microsoft.com/fwlink/?LinkId=510472) , jotka sinun on muita kirjastoja tai muista malleista.

* [Mobile-sovellusten projektimallien](./app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

    Mobile-sovellusten mallit ovat käytettävissä vain Visual Studio 2013: n päivitys 2 ja uudempi versio. Ne eivät ole käytettävissä Visual Studio 2012 tai sitä aiemmissa versioissa, ei Visual Studio 2013: n päivitys 1 tai vanhempi versio, vaikka asennat .NET Azure SDK-paketissa.

##<a id="faq"></a>Usein kysytyt kysymykset

- [Monta Azure ominaisuudet ovat Visual Studiossa. Pitääkö asentaminen Azure SDK .NET?](#azinvs)
- [Haluan, että asiakas-kirjasto. Tarvitsen .NET hankkiminen Azure SDK asentamiseksi?](#clientlib)
- [Mistä löydän Azure SDK vanhempien versioiden .NET?](#olderversions)
- [Mikä on elinkaarikäytännöstä Azure SDK .NET versiot?](#lifecycle)
- [Mitä Vieras-käyttöjärjestelmä on Azure SDK .NET-yhteensopiva?](#guestos)
- [Miten voin poistaa Azure-SDK .NET varten?](#uninstall)

###<a id="azinvs"></a>Monta Azure ominaisuudet ovat Visual Studiossa. Pitääkö asentaminen Azure SDK .NET?

Se on hyvä Asenna SDK, jos haluat Kehitä Azure uusimman työkaluilla. Jos mieluummin ei asenna SDK, voit tehdä, jos seuraavat ehdot täyttyvät:

* Olet asentanut uusimman [Visual Studio päivitys](http://www.visualstudio.com/downloads/download-visual-studio-vs#DownloadFamilies_5).
* Kehität vain Azure sivustot tai Mobile-palvelut, ei ole vähintään pilvipalveluihin näennäiskoneiden varten.
* Sovellus ei käytä tallennustilaa, tai se käyttää tallennuspaikkaa mutta et tarvitse tallennustilan emulaattori tai AzCopy-työkalua.

###<a id="clientlib"></a>Haluan, että asiakas-kirjasto. Tarvitsen .NET hankkiminen Azure SDK asentamiseksi?

SDK asentaa asiakkaan kirjastojen vain, jotta voit luoda cloud palvelun projekteja vaikka et ole yhteydessä Internetiin. Nykyisen asiakkaan kirjastot ovat käytettävissä NuGet pakettien [NuGet.org](http://go.microsoft.com/fwlink/?LinkId=510472). Lisätietoja on artikkelissa aiemmin tässä asiakirjassa [ominaisuuksiin ei on Azure SDK .NET asennuksen yhteydessä](#notincluded) .

###<a id="olderversions"></a>Mistä löydän Azure SDK vanhempien versioiden .NET?

Vanhemmat versiot on artikkelissa [Azure SDK.NET](https://azure.microsoft.com/downloads/archive-net-downloads/) Lataa sivu.

###<a id="lifecycle"></a>Mikä on elinkaarikäytännöstä Azure SDK .NET versiot?

Katso [Microsoft Azure pilvipalveluihin tuen elinkaarikäytännöstä](http://support.microsoft.com/gp/azure-cloud-lifecycle-faq).

###<a id="guestos"></a>Mitä Vieras-käyttöjärjestelmä on Azure SDK .NET-yhteensopiva?

Katso [Azure Vieras OS versioista ja SDK yhteensopivuuden matriisissa](http://msdn.microsoft.com/library/ee924680.aspx).

###<a id="uninstall"></a>Miten voin poistaa Azure-SDK .NET varten?

Kunkin kohteen suorittaa tässä artikkelissa kohdassa [mitä sisältyy Azure SDK .NET](#included) on erillinen ohjelma Windowsin Ohjauspaneelissa **Ohjelmat ja toiminnot**asennettujen ohjelmien luettelosta.  Tällä ei voi poistaa niitä ryhmänä; Sinun on kunkin ohjelman asennuksen yksitellen.

Kun sinulla on jo asennettu .NET Azure SDK ja uuden version asentamista, käytössäsi on yleensä ei tarvitse poistaa vanhan. Useimmissa tapauksissa SDK-asennus päivittää tietokoneessa olevan ohjelman sijaan uuden lisäämistä ja jätä vanhan.

Jos haluat poistaa ei-pidempi-tarvitaan osia aiemman version, vain ohjelmia, jotka määrittävät vanhempaa versionumeroa, ja poista ne vain, jos saman ohjelman uudemmalla versiolla. Esimerkiksi kun olet päivittänyt 2.5-2.6 näkyviin voi tulla 2.5- ja 2.6 "Microsoft Azure Työkalut Microsoft Visual Studio 2013-versioissa ja 2,5 version asennuksen. Näet ehkä vain "Microsoft Azure yhtä aikaa muiden kanssa Työkalut" 2,5 version mutta ei ole turvallinen asennuksen.

Huomaa, että versio luvuilla ohjelma otsikot näkyvät **Ohjelmat ja toiminnot** voi harhaanjohtavia.  Esimerkiksi SDK versiota 2.6 on "Microsoft Azure Mobile sovelluksen SDK V1.0", jos asennat SDK Visual Studio 2013 ja "Microsoft Azure Mobile sovelluksen SDK V2.0" Visual Studion 2015, versio ei ole tässä tapauksessa SDK-versio, mutta joiden Visual Studio versio ohjelma koskee ilmaisin.

Tässä artikkelissa ei Näytä kaikissa ohjelmissa, jotka jokaisen aiemmassa versiossa Azure SDK sisällyttää; on muiden ohjelmien asennuksen SDK-versioista, koska SDK aiempien joskus ohjelmat, jotka on jätetty pois uudemmissa versioissa. Esimerkiksi 2.5-version asentaa "Azure resurssien hallinnan Tools for Visual Studio" mutta, joka ei ole tässä artikkelissa luettelossa koska sitä ei enää näkyy erillisenä ohjelmana **Ohjelmat**ja toiminnot.  Tämä artikkeli sisältää vain ohjelmat, jotka sisältyvät .NET versiota 2.6 Azure SDK-paketissa.

> **Huomautus:** Ohjelmien, joka sisältää SDK voidaan asentaa erikseen myös muissa tilanteissa ja voivat olla tarpeen, vaikka et tarvitse koko SDK-paketissa. Sama voi olla tosi ohjelmat, jotka asennettiin SDK vanhemmissa mutta on jätetty pois SDK uudemmissa versioissa. Kun olet poistanut asennuksen ohjelmat, varmista, ettet välttämiseksi poistaa jotakin, mitä tarvitaan yhä tietokoneessa.



##<a id="versions"></a>Versiot

Saat näkyviin versiotiedot on ajan tasalla tai Lataa vanhemmat versiot, tutustu [Azure SDK .NET versiohistoria](https://azure.microsoft.com/downloads/archive-net-downloads/) -sivu.

##<a id="resources"></a>Resurssit

Jos haluat ladata nykyisen Azure SDK .NET tai asiakirjakirjaston, katso [Azure-lataussivulta](https://azure.microsoft.com/downloads/).

Katso .NET-lähdekoodi, mukaan lukien asiakkaan kirjastojen Azure SDK [GitHub.com/Azure](https://github.com/azure/).

Katso Azure asiakkaan kirjaston oppaat [Azure .NET viittaus](https://azure.microsoft.com/documentation/api/).

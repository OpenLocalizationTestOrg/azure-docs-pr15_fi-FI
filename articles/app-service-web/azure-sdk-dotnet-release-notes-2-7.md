
<properties 
   pageTitle="Azure SDK .NET 2.7 ja .NET 2.7.1 julkaisutiedot" 
   description="Azure SDK .NET 2.7 ja .NET 2.7.1 julkaisutiedot" 
   services="app-service\web" 
   documentationCenter=".net" 
   authors="Juliako" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="app-service"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="10/17/2016"
   ms.author="juliako"/>

# <a name="azure-sdk-for-net-27-and-net-271-release-notes"></a>Azure SDK .NET 2.7 ja .NET 2.7.1 julkaisutiedot

##<a name="overview"></a>Yleiskatsaus

Tämä asiakirja sisältää julkaisutiedot Azure SDK .NET 2.7 versioon. 

Asiakirja sisältää myös julkaisutiedot Azure SDK-paketissa, .NET 2.7.1 vapauttamista.

Azure SDK 2.7 tuetaan vain Visual Studio 2015 ja Visual Studio 2013: ssa. [Azure SDK 2.6](https://azure.microsoft.com/downloads/) on viimeisin tuetut SDK Visual Studio 2012.

Yksityiskohtaisia tietoja tässä versiossa on artikkelissa [Azure SDK 2.7 ilmoitus kirjaa](https://azure.microsoft.com/blog/2015/07/20/announcing-the-azure-sdk-2-7-for-net/) ja [Azure SDK 2.7.1 ilmoitus viestejä](http://go.microsoft.com/fwlink/?LinkId=623850).

##<a name="azure-sdk-for-net-27"></a>.NET 2.7 Azure SDK

###<a name="sign-in-improvements-for-visual-studio-2015"></a>Kirjaudu sisään Visual Studio 2015 parannukset

Azure SDK 2.7 Visual Studio 2015 julkaistut tukee uusien käyttäjätietojen hallintaominaisuudet Visual Studio 2015.  Tämä sisältää tuki tilien käyttäminen Azure roolin perusteella käyttöoikeuksien valvonta, Cloud palveluntarjoajat, DreamSpark ja tilin ja tilauksen muuntyyppisten kautta.

Azure SDK 2.7 mukana kirjautumisen parannuksia ovat käytettävissä vain Visual Studio 2015. Visual Studio 2013 tuki sisältyy Azure SDK 2.7.1.


###<a name="mobile-sdk"></a>Mobiili SDK-paketissa

Päivitetty **Mobiilisovellukset** mallit vastaamaan uusin [NuGet paketin](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) ja asennuksen jälkeen.

###<a name="service-bus"></a>Palvelun Bus 

Yleiset virheenkorjauksia ja parannukset. Yksityiskohtaisen päivitykset ja ominaisuuksia Katso uusimmat [Service Bus NuGet](http://www.nuget.org/packages/WindowsAzure.ServiceBus/)julkaisutiedot.

###<a name="hdinsight-tools"></a>HDInsight-Työkalut 

Tässä versiossa on tehty seuraavat päivitykset. Nämä päivitykset, jotka ovat esikatselu. Lisätietoja on artikkelissa [tämä blogiin](http://go.microsoft.com/fwlink/?LinkId=619108).

- Rakenteen kaaviot rakenteen Tez töihin
- Täydellinen rakenne Enimmäiskuolleisuusrajaa IntelliSense-tuki
- Possu mallin tuki
- Azure-palveluiden myrsky mallit

####<a name="breaking-changes"></a>Tärkeimmät muutokset

- Vanha **myrsky** projekti on päivitettävä, kun käytät tätä versiota työkaluja. Lisätietoja on artikkelissa [tämä blogiin](http://go.microsoft.com/fwlink/?LinkId=619108).
- Visual Studio Web Express ei enää tueta. Lisätietoja on artikkelissa [tämä blogiin](http://go.microsoft.com/fwlink/?LinkId=619108).

###<a name="azure-app-service-tools"></a>Azure App palvelun Työkalut

Tässä versiossa seuraavat päivitykset on tehty Web-työkalut-laajennuksia. Katso lisätietoja [tämän](https://azure.microsoft.com/blog/2015/07/20/announcing-the-azure-sdk-2-7-for-net/) blogin. 

- DreamSpark tilien yhteen tuki
- Uusi Azure resurssien hallinnan API tukemaan tehty koko muutos Azure-Työkalut
- Azure-sovelluksen palvelu lisätään [Cloud Explorer](#cloud_explorer) tuki

####<a name="known-issues"></a>Tunnetut ongelmat

Web App käyttöönoton paikka solmut eivät näy palvelimen Explorerissa paikkojen-solmu ja Web Appin käyttöönotto paikka alisolmut eivät lataudu Cloud Explorer-kohdassa. Tämä ongelma on ratkaistu ja valmisteltu seuraavaa SDK-versiota varten. 


###<a name="cloud_explorer"></a>Cloud Explorer for Visual Studio 2015

Azure SDK 2.7 sisältää Cloud Explorer Visual Studio 2015, jonka avulla voit tarkastella Azure resursseja, tarkista niiden ominaisuudet ja suorita avaimen developer toiminnot Visual Studion. 

Cloud explorer tukee seuraavasti:

- Azure resursseja resurssiryhmän ja resurssilaji näkymiä 
- Hae resurssien nimellä (käytettävissä resurssilaji-näkymässä)
- Tilaukset- ja resursseja, joiden roolin perusteella Access ohjausobjektin (RBAC) käytetään tuki 
- Jossa näkyy developer keskittyvässä toiminnot tietyn valitun resurssien integroitu toiminto-paneeli. Esimerkki: liittää etätyöpöydän virheenkorjaus varten luotu näennäiskoneiden avulla Resurssienhallinta-pino tarkastella diagnostiikka tietojen näennäiskoneiden jne.
- Integroitu ominaisuudet-ruutu, joka näyttää tavallisesti tarvittavat keskihajonta-testin aikana developer keskittyvässä ominaisuudet 
- Tili, jota käytetään, kun resursseja (käytä työkalurivin asetukset-komento) nopea vaihtaminen 
- Suodattaminen tilauksista, kun luettelointi resurssit (työkalurivin asetukset-komennon avulla) 
- Laaja linkkejä, resursseja ja resurssiryhmien hallinta Azure-portaaliin 
 
 
###<a name="azure-resource-manager-tools"></a>Azure Resurssienhallinta-Työkalut 

Roolin perusteella Access ohjausobjektin (RBAC) ja uusi tilaustyypit on päivitetty Azure Resurssienhallinta-työkalut.  Nämä muutokset sisältää voi käyttää tallennustilan käyttäjätilejä, perinteinen tallennustilan lisäksi tallentaa palvelutiedot käyttöönoton aikana.  

Jos käytät Azure resurssiryhmä-projektin aiemmasta versiosta SDK: n SDK-2.7, uusi käyttöönoton komentosarja tarvitaan käyttöön sen sijaan, että perinteinen tallennustilan uusi tallennustilan-tilin avulla.  Sinua pyydetään, ennen kuin projektin voit lisätä uuden komentosarjan tehdään muutoksia.  Vanha komentosarja nimetään uudelleen ja täytyy tehdä muutoksia uuden komentosarjan manuaalisesti.
 
 
###<a name="storage-explorer-tools"></a>Tallennustilan Explorerin Työkalut 

- Liitä BLOB tuki. Lisätietoja [rajaamisesta on blogikirjoituksessa](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/04/13/introducing-azure-storage-append-blob.aspx). 
- Palvelimen Explorer Premium tallennustilan-tilit tuki. Palvelimen Explorer näyttää vain sivun BLOB premium-tallennustilan tilin sellaisina kuin ne ovat ainoa tuettu premium-tallennustilan tilin tyyppi.

###<a name="azure-data-factory-tools-for-visual-studio"></a>Azure Data Factory Tools for Visual Studio 

Esittely **Azure Datatyökalut Factory** Visual Studio. Seuraavassa on käytössä ominaisuuksia. Katso lisätietoja [tämän blogin](http://go.microsoft.com/fwlink/?LinkId=617530) .

- **Malliin authoring**: Valitse Käytä toimivaltaisena perusteella, siirto Tietomallit tietojen käsittely malleja tai lopusta loppuun-tiedot-integrointi-ratkaisun käyttöönotto ja aloita käytännön nopeasti Data Factory kanssa. 
- **Integrointi ratkaisunhallinnassa yhtä aikaa muiden kanssa ja käyttöönotto Data Factory kohteiden**: Luo ja käyttöönotossa putkistot ja Visual Studio projekteja Aiheeseen liittyvät kohteet. 
- **Kaavion integrointi tarkastella visual vuorovaikutuksen aikana yhtä aikaa muiden kanssa**: author visuaalisesti putkistot ja tietojoukkoja kaavionäkymään tuen kanssa. 
- **Selaaminen ja käsittelemisen jo käyttöön kohteiden palvelimen explorer integrointi**: hyödyntää palvelimen Explorer Selaa Oletko jo ottanut käyttöön tietojen tehtaan ja vastaavat kohteet. Tuoda projektin käyttöön tietojen factory tai minkä tahansa kohteen (putkijohto, linkitetyn palvelun tietojoukkoja). 
- **JSON muokkaaminen rakenteen tarkistaminen ja rich IntelliSensen avulla**: tehokkaasti määrittäminen ja rich intellisense- ja rakennetiedostot kelpoisuuden tarkistamisen käyttöön liittyviä tietoja Factory kohteiden JSON-tiedostojen muokkaaminen 
- **Usean ympäristön julkaiseminen**: Julkaise kussakin ympäristössä erillisessä config-tiedostojen luominen putkistot keskihajonta, Testaa tai tuot ympäristön tekijä.
- **Possu, rakenne ja .net-pohjaisten tietojenkäsittely-tuki**: Possu ja rakenne-komentosarjoja Data Factory Projectissa tuki. Tuki viittaava C# projektin hallintaan .net tehtävän.

##<a name="azure-sdk-for-net-271"></a>.NET 2.7.1 Azure SDK

Seuraavassa osassa on päivitykset, jotka on tuotu Azure SDK-paketissa, jos .NET 2.7.1 vapauttamista.
###<a name="hdinsight-tools"></a>HDInsight-Työkalut 

Katso tarkempia tietoja virhetilanteesta HDInsight Työkalut päivityksistä, [tämä blogiin](http://go.microsoft.com/fwlink/?LinkId=623831).

- Rakenteen työn operaattori näkymä (uusi ominaisuus)

    Voi auttaa sinua ymmärtämään kyselyn rakenne paremmin, rakenne operaattori näkymä-ominaisuus on lisätty. Kärkipisteen sisällä operaattorit näkyviin kaksoisnapsauttamalla projektin kaavio kärkipisteitä. Jos haluat nähdä tietyn toimijan, Vie hiiren osoitin kyseiseen operaattori enemmän tietoja.
- Rakenne-virhe-merkki (uusi ominaisuus)

    Jotta voit tarkastella kielioppivirheet välittömästi, rakenne virhe merkki-ominaisuus on lisätty. Myös virhesanomia on parannettu ja näet yksityiskohtaisia kielioppivirheet heti (ennen kuin tässä versiossa on odottaa jonkin aikaa, ennen kuin virhe viestin tietojen käytön ja lähettämiseen klusterin rakenteen komentosarjan).  
- Myrsky topologian Graph (uusi ominaisuus)

    Kesäolympialaisten visualisointi on erittäin tärkeää, kun haluat nähdä, jos oman topologian toimii odotetulla tavalla. Tässä versiossa on lisätty myrsky viivakaavion visualisointi. Voit havainnollistaa tärkeitä mittarit, että topologian (esimerkiksi väri osoittaa sää tiettyjä lukko on "varattu- tai ei). Voit myös kaksoisnapsauttamalla napsauttaa lukko/nokkaan Näytä enemmän tietoja.

- HDInsight-klustereiden, jotka on luotu Azure-portaalissa (ohjelmavirhe korjaa) tuki

    Voit nyt Visual Studio voit tarkastella ja lähettää työt kaikki oman HDInsight klustereiden riippumatta siitä, missä klusterin on luotu.

- Lisätietoja IntelliSense-tuen & nopeammin rakenne metatietojen lataaminen (parantaminen)

    Microsoft on parannettu IntelliSense lisäämällä siihen lisää käyttäjän helpossa muodossa ehdotuksia. Esimerkiksi taulukon tunnus voi nyt myös ehdotettu IntelliSense-niin voit kirjoittaa kyselyn helpommin. Lisäksi on on entistä parempi lataaminen kestää vain muutaman sekunnin luettelon kaikki tietokannat, taulukoita ja sarakkeita rakenteen metastore niin rakenne-metatiedot.

Katso tarkempia tietoja virhetilanteesta HDInsight Työkalut päivityksistä, [tämä blogiin](http://go.microsoft.com/fwlink/?LinkId=623831).

###<a name="improvements-in-visual-studio-2013"></a>Visual Studio 2013 parannukset

- Azure SDK 2.7.1 mahdollistaa Visual Studio 2013 käyttää Azure asiakkaat ja tilaukset roolin perusteella käyttöoikeuksien valvonta, Cloud palveluntarjoajat ja Dreamspark kautta.
- Azure SDK 2.7.1, jossa uusi Cloud työkalun ikkunan sisältyy nyt myös Visual Studio 2013.

###<a name="known-issues"></a>Tunnetut ongelmat

Azure SDK 2.6 tai asennuksen 2.7.1 Visual Studio yhteisön 2013-ei - englannin kielen OS näkyy ilmoitus, että Visual Studio englannin kielen ja kuin englanninkielisessä resursseja voi olla ristiriidassa. Varoitus ehkä hylännyt turvallisesti. Se ilmenee vain, jos tietokoneessa ei sisällä edellisen asennettuun Visual Studio yhteisön 2013 ja oletko asentamassa SDK-ei - englannin kielen OS. Varoitus näkyy, kun kielipaketti koskee RTM resurssit Visual Studiossa, mutta ennen kuin se liittyy Päivitä 4. Varoitus sulkemisessa Salli kielipaketti Jatka ja täytä käyttämällä language pack sisällön Päivitä 4-versiota.

LightSwitch projektien eivät ole compatibile tämän version kanssa. Tämä ongelma on ratkaistu seuraava SDK-versiossa.

##<a name="also-see"></a>Katso myös
[Azure SDK 2.7.1 ilmoitus viestiin](http://go.microsoft.com/fwlink/?LinkId=623850)

[Azure SDK 2.7 ilmoitus viestiin](https://azure.microsoft.com/blog/2015/07/20/announcing-the-azure-sdk-2-7-for-net/)

[Tuki- ja käytöstä poistaminen lisätietoja Azure SDK .NET ja ohjelmointirajapinnan](https://msdn.microsoft.com/library/azure/dn479282.aspx/)

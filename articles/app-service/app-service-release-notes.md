<properties 
   pageTitle=".NET 2.5.1 Azure SDK julkaisutiedot" 
   description=".NET 2.5.1 Azure SDK julkaisutiedot" 
   services="app-service" 
   documentationCenter=".net,nodejs,java" 
   authors="Juliako" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="app-service"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="10/10/2016"
   ms.author="juliako"/>


# <a name="azure-sdk-for-net-251-release-notes"></a>.NET 2.5.1 Azure SDK julkaisutiedot

Tämä asiakirja sisältää julkaisutiedot Azure SDK-paketissa, .NET 2.5.1 Vapauta. 

##<a name="azure-sdk-for-net-251-release-notes"></a>.NET 2.5.1 Azure SDK julkaisutiedot

Seuraavassa on uusia ominaisuuksia ja .NET 2.5.1 Azure SDK päivityksiä.

- Uusi features\scenarios liittyvät **Web-työkalut-laajennuksia**. 

    - Azure-sovelluksen palveluun Azure sivustojen on nimetty uudelleen. Lisätietoja on artikkelissa [Azure App palvelun ja aiemmin Azure-palveluja](app-service-changes-existing-services.md).
    - Azure API Apps (ennakkoversio) tuki on lisätty, jotta asiakkaat voit julkaista ASP.NET projektit API-sovellukset ja käytä sitten Lisää > Azure API App asiakkaan liikkeen C# projektit Luo koodi käyttöön API sovelluksen rakenteen perusteella. 
    - Palvelimen Explorerissa sivustot-solmu on vähennetty Azure App Service-solmu, joka sisältää resurssin ryhmän perusteella ryhmittely Azure API sovellukset, Mobile-sovellusten ja verkkosovelluksissa tuki sijasta.
    - Azure Mobile-sovellusten (esikatselu)-tuki on lisätty, jotta asiakkaat voivat luoda uusia mobiilisovellukset projekteja lisääminen Mobile-sovellusten ohjaimet, julkaista projektien ja virheenkorjaus etäyhteyden sovellukset.
    - Lisää > Azure API App asiakkaan liikkeen tukee nyt paikallisia Swagger JSON-tiedostoja, jotta verkko-Ohjelmointirajapinnan kehittäjät voivat käyttää kolmannen osapuolen NuGets, kuten Swashbuckle Luo Swagger tai tekijän manuaalisesti. Tällä tavalla asiakkaan kehittäjät voivat käyttää koodin sukupolven ominaisuuksia tarjoaman minkä tahansa Swagger päätepisteen C# projekteissa. 
    - Web App-sovelluksen ja sovelluksen API julkaisun valintaikkunat on parannettu tukemaan Azure Portal-käsite resurssien ryhmittely ja valinnan/luominen Azure resurssiryhmien ja sovelluksen palvelun suunnitelmien esitetään uutta Web App- ja API sovelluksen valmistelu valintaikkunaa. 
    - Azure-portaalin sekä muita ominaisuuksia, kuten Log Streaming ja Remote virheenkorjaus-Ohjelmointirajapinnan sovelluksia laaja-linkki Azure API App palvelimen Explorer solmujen linkeissä.

    Tunnetut ongelmat ja nykyisen rajoitukset Azure SDK .NET 2.5.1 [tämän](app-service-release-notes.md#known_issues_2_5_1) osan alla.


- Uusi features\scenarios liittyvät **HDInsight Työkalut** Visual Studiossa otetaan käyttöön tässä versiossa. 
    - Rakenne-komentosarjoja paikallisen vahvistus. Napsauta työkalurivin ovatko komentosarja-virheitä Vahvista komentosarja-painiketta. 
    - Parannettu virheenkorjaus rakennetta työpaikkojen. Rakenteen työt korjaat nyt käyttäminen kuitenkaan lokit Visual Studiossa. Jos sovellus on suorituskykyongelmia, tutkiminen kuitenkaan lokit tarjoaa hyödyllisiä tietoja...
    - (Public Preview) Avainsanojen automaattinen-täydentäminen ja IntelliSense tuki rakenne. Avulla luot rakenteen komentosarjoja, HDInsight Tools for Visual Studio lisätty avainsana automaattinen-täydentäminen ja IntelliSense tukevat rakenne.
    - Myrsky tuki. Voit nyt HDInsight Tools for Visual Studio kehittää myrsky topologioissa/Spouts/Pultit C#. Voit lähettää kehittämä topologian myrsky klusteriin ja topologian/lukko/nokkaan tila. Voit käyttää järjestelmälokit ja asiakkaan lokitiedot oman myrsky topologioissa/Pultit/Spouts vianmääritys. Voit käyttää myös aiemmin JAVA varat myrsky HDInsight.
    
    Lisätietoja on artikkelissa [käyttäminen HDInsight Hadoop Tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).



##<a id="known_issues_2_5_1"></a>.NET 2.5.1 Azure SDK tunnettuja ongelmia ja rajoituksia

- Käyttöönoton kohteeksi mobiilisovellukset näkyy Azure API-sovellukset. Web Apps olisi mobiilisovellukset vain kohde vasta myöhemmin versiossa. 
- Azure API sovelluksen valmistelu voi aiheuttaa success mutta ajoittain Azure App aktiviteetin-ikkunassa edistymisen päivittäminen epäonnistui. Vaihtoehtoinen menetelmä on tilan tarkistaminen uuden Azure API-sovelluksen Azure-portaalissa. 
- Tiedosto > uusi projekti > API sovellus > F5 kokemus johtaa HTTP-virhe, koska ei ole default/index.html. Vaihtoehtoinen menetelmä ei siirry /api/values URL-Osoitteen manuaalisesti. 
- Ajoittain palvelimen Explorer kuvakkeet näkyvät litistetty. Uudelleenkäynnistys ja korjaa tämän ongelman. 
- Jos ilmenee poikkeus aikana verkkosovellukseen tai API sovelluksen valmistelu (esimerkiksi ylitti kiintiövirheet tai kaksoiskappaleiden Azure API App yhdyskäytävän nimi), jotkin raaka JSON-tekstin näyttäminen virheet. 
- Katkonainen projektin luomisen ongelmat, kun sovellus tiedot on valittuna projektin luomisen aikana.
- Joskus luotu Azure API App asiakas-koodi puuttuu nimitilan, he tarvitsevat koodin kääntäminen voi sisällyttää manuaalisesti (tai Visual Studio vihjeillä automaattisesti tuodut). 
- Mobile-sovelluksen projektien pitäisi julkistaa web Apps-sovelluksissa, mutta sinun on valittava jälkeen Mobile-sovelluksen projektit edellyttävät tietokannan Mobile-sovelluksen Azure-portaalissa kuin luotu sivusto. 
- Mobile-sovellusten aloitussivun käytetään termiä "mobiilipalvelu" "mobile-sovellukset" sijaan 
- Mobile-sovelluksen projektin luontia voi kestää hetken luomiseen. 
- Aikana API sovelluksen valmistelu (joissakin tapauksissa) virhe tulee takaisin Azure API Mieti, että käyttöoikeudet ei voitu määrittää oikein, kun API-sovellus on valmisteltu oikein, ja se on valmis käytettäväksi. Voit määrittää manuaalisesti Azure-portaalissa käyttöoikeudet.
- Hakemuksen tiedot ei tue API sovellusmallit ja Mobile sovellusmallit.
- API App projekteja ei voi käyttää yhdessä pilvipalvelussa projektit.
- API-projektin sovellusmallit ovat käytettävissä vain C#.
- API-sovelluksen kautta "Lisää Azure API App asiakas" pikavalikon kulutus tuetaan vain C#.

 

<properties
    pageTitle="Jatkuva lähettämisen ja Visual Studio ryhmän Services Azure | Microsoft Azure"
    description="Opettele määrittämään Visual Studio Team Services ryhmän projektien automaattisesti muodosta ja ota käyttöön Azure App palvelun tai cloud services Web App-toiminnon avulla."
    services="cloud-services"
    documentationCenter=".net"
    authors="mlearned"
    manager="douge"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="07/06/2016"
    ms.author="mlearned"/>

# <a name="continuous-delivery-to-azure-using-visual-studio-team-services"></a>Jatkuva toimittamista Azure Visual Studio Team Services-palvelujen avulla

Voit määrittää Visual Studio Team Services ryhmän projektien luominen ja Azure web Apps-sovellusten käyttöön tai cloud services automaattisesti.  (Tietoja siitä, miten voit määrittää jatkuva muodosta ja ota käyttöön järjestelmän *paikalliseen* Team Foundation Server on artikkelissa [Azure-tietokannassa pilvipalveluihin jatkuva toimitukseen](cloud-services-dotnet-continuous-delivery.md).)

Tässä opetusohjelmassa oletetaan, että Visual Studio 2013 ja Azure SDK asennettuna. Jos sinulla ei vielä ole Visual Studio 2013, lataa se valitsemalla [www.visualstudio.com](http://www.visualstudio.com) **Aloita maksutta** olevaa linkkiä. Asenna Azure SDK [täältä](http://go.microsoft.com/fwlink/?LinkId=239540).

> [AZURE.NOTE]Sinun on suoritettava tässä opetusohjelmassa Visual Studio Team Services-tilin: Voit [avata Visual Studio Team Services-tilin maksutta](http://go.microsoft.com/fwlink/p/?LinkId=512979).

Voit määrittää pilvipalvelussa, voit automaattisesti muodosta ja ota käyttöön Azure Visual Studio ryhmän Services-palveluiden avulla, toimimalla seuraavasti.

## <a name="1-create-a-team-project"></a>1: ryhmäprojektin luominen

Noudata [Tässä](http://go.microsoft.com/fwlink/?LinkId=512980) voit luoda projektin työryhmän ja linkittää sen Visual Studio. Tässä vaiheittaisessa ohjeessa oletetaan, että käytät ryhmän Foundation versio ohjausobjektin (TFVC) lähde-ohjausobjektin ratkaisuksi. Jos haluat käyttää Git Versionhallinta, voit tarkistaa [tätä vaiheittaista Git-version](http://go.microsoft.com/fwlink/p/?LinkId=397358).

## <a name="2-check-in-a-project-to-source-control"></a>2: projektin hallintaan kuittaaminen sisään

1. Avaa ratkaisu, jonka haluat ottaa käyttöön Visual Studiossa, tai luoda uuden.
Voit ottaa verkkosovellukseen tai pilvipalveluun (Azure-sovellus) käyttöön noudattamalla tämän vaiheittaisen kuvauksen suorittamiseksi.
Jos haluat luoda uuden ratkaisun, luo uuden Azure pilvipalvelussa projektin tai uuden ASP.NET MVC projektin. Varmista, että projektin kohteena .NET Framework 4 tai 4.5, ja jos luot cloud palvelun projektin, Lisää ASP.NET MVC web-rooli ja työntekijän rooli ja valitse Internet-sovelluksen web-roolin. Valitse **Internet-sovellus**.
Jos haluat luoda verkkosovellukseen, valitse ASP.NET Web-sovelluksen project-malli ja valitse sitten MVC. Lisätietoja on kohdassa [Create ASP.NET web-sovelluksen Azure App palvelu](../app-service-web/web-sites-dotnet-get-started.md).

    > [AZURE.NOTE] Visual Studio Team Services tukevat vain CI-versioiden Visual Studio verkkosovellusten tällä hetkellä. Web-sivuston projektit ovat alueen ulkopuolella.

1. Avaa pikavalikon ratkaisun ja valitse **Lisää ratkaisu hallintaan**.

    ![][5]

1. Hyväksy tai muuttaa oletusarvot ja valitse **OK** -painiketta. Kun prosessi on valmis, ja tietolähteen ohjausobjektin kuvakkeet näkyvät **Ratkaisunhallinnassa**.

    ![][6]

1. Avaa pikavalikon ratkaisu ja valitse **Kuittaa sisään**.

    ![][7]

1. **Ryhmän Explorer** **Odottavia muutoksia** -alueella Kirjoita sisäänkuittauksen kommenttia ja napsauta **Kuittaa sisään** -painiketta.

    ![][8]

    Huomautus Voit lisätä tai poistaa tietyn muutokset, kun tarkistat asetukset. Jos haluamasi muutokset jätetään pois, valitse **Sisällytä kaikki** -linkkiä.

    ![][9]

## <a name="3-connect-the-project-to-azure"></a>3: projektin yhdistäminen Azure

1. Nyt kun olet luonut ja Team Services ryhmäprojektin joitakin lähdekoodi ei, olet valmis ryhmäprojektin yhdistäminen Azure.  Valitse [Azure perinteinen portal](http://go.microsoft.com/fwlink/?LinkID=213885)cloud-palvelun tai web Appissa tai luoda uuden valitsemalla **+** vasemmassa alakulmassa ja valitse **Pilvipalvelussa** tai **Web App** ja valitse **Nopea luominen**-kuvaketta. Valitse **määrittäminen julkaisemista Visual Studio Team Services** -linkki.

    ![][10]

1. Kirjoita ohjatun toiminnon Visual Studio Team Services-tilin nimi tekstiruutuun ja valitse **Määritä nyt** -linkki. Sinulta saatetaan pyytää kirjautumaan.

    ![][11]

1. **Yhteyden pyytää** avautuvassa valintaikkunassa valitse **Hyväksy** -painike sallivat Azure määrittäminen ryhmäprojektin ja ryhmän Services-palveluissa.

    ![][12]

1. Kun luvan onnistuu, näet avattava luettelo Visual Studio Team Services-ryhmän projekteja. Valitse, jonka loit edellisessä vaiheessa ryhmäprojektin nimeä ja valitse sitten ohjatun toiminnon valintamerkki-painiketta.

    ![][13]

1. Kun projekti on linkitetty, näet hieman ohjeita Visual Studio Team Services ryhmäprojektin muutosten tarkistaminen.  Oman seuraava sisäänkuittauksen, valitse Visual Studio Team Services muodosta ja ota käyttöön projektin Azure.  Kokeile nyt mukaan valitsemalla **Kuittaa sisään Visual Studio** -linkki ja sitten **Käynnistä Visual Studio** -linkki (tai vastaava **Visual Studio** painike portaalin näytön alareunassa).

    ![][14]

## <a name="4-trigger-a-rebuild-and-redeploy-your-project"></a>4: käynnistäminen muodosta ja ota uudelleen projektin

1. Visual Studio **Ryhmän Explorer**Napsauta **Tietolähteen ohjausobjektin Explorer** -linkkiä.

    ![][15]

1. Siirry ratkaisu-tiedoston, ja avaa se.

    ![][16]

1. Napsauta **Ratkaisunhallinnassa**tiedoston avaaminen ja vaihda. Esimerkiksi muuttaa tiedoston `_Layout.cshtml` näkymät-kohdassa\\jaetun kansion MVC web-rooli.

    ![][17]

1. Muokkaa sivuston logo ja valitse Tallenna tiedosto **Ctrl + S** .

    ![][18]

1. Valitse **Ryhmän Explorer** **Odottavia muutoksia** -linkkiä.

    ![][19]

1. Kirjoita kommentti ja valitse sitten **Kuittaa sisään** -painike.

    ![][20]

1. Palauttaa **Ryhmän Explorerissa** aloitussivulle valitsemalla **Aloitus** .

    ![][21]

1. Valitse **muodostaa** linkin avulla versiot käynnissä.

    ![][22]

    **Ryhmän Explorer** näyttää muodosta saatu oman sisäänkuittauksen.

    ![][23]

1. Kaksoisnapsauta nimeä, Luo käynnissä lokin yksityiskohtaiset muodosta edetessä.

    ![][24]

1. Luo ollessa käynnissä Tutustu koontiversion määrityksen, joka on luotu, kun TFS linkitetty Azure ohjatulla toiminnolla.  Avaa pikavalikko koontiversion määrityksen ja valitse **Muokkaa muodosta määritys**.

    ![][25]

    **Käynnistimen** -välilehdessä näkyy, että koontiversion määrityksen määritetään voit luoda jokaisen sisäänkuittauksen oletusarvoisesti.

    ![][26]

    **Prosessi** -välilehden näet käyttöönottoympäristö on määritetty cloud palvelun tai web-sovelluksen nimi. Jos käsittelet web Apps-sovelluksista, näet ominaisuudet on poikkeavat kuvassa.

    ![][27]

1. Määritä ominaisuuksien arvot, jos eri arvoja kuin oletusarvoiset. Azure julkaisun ominaisuudet ovat **käyttöönotto** -osasta.

    Seuraavassa taulukossa näkyvät käytettävissä olevat ominaisuudet **käyttöönotto** -osasta:

  	|Ominaisuus|Oletusarvo|
  	|---|---|
  	|Salli luotettu varmenteet|Jos arvo on EPÄTOSI, SSL-varmenteita on kirjauduttava päämyöntäjä.|
  	|Salli päivitys|Päivitä olemassa olevaan käyttöympäristöön sijaan uuden käyttöönoton avulla. Säilyttää IP-osoite.|
  	|Älä poista|Jos arvo on TOSI, ei korvaa olemassa olevaan toisiinsa liittymättömiä käyttöympäristöön (päivitys on sallittu).|
  	|Polun käyttöönottoasetukset|Web-sovelluksen suhteessa repo pääkansio .pubxml tiedoston polku. Ohitettavaksi pilvipalveluihin.|
  	|SharePoint-käyttöönotto-ympäristössä|Sama kuin palvelunimi.|
  	|Azure käyttöönotto-ympäristössä|Web app ja pilvi palvelunimi.|

1. Jos käytössäsi on useita palvelun määrityksiä (.cscfg-tiedostoja), voit määrittää haluamasi palvelun **Muodosta, Lisäasetukset-MSBuild argumentit** -asetus. Esimerkiksi käyttämään ServiceConfiguration.Test.cscfg asetus MSBuild argumentit rivi `/p:TargetProfile=Test`.

    ![][38]

    Tässä vaiheessa oman muodosta on suoritettu onnistuneesti.

    ![][28]

1. Jos kaksoisnapsautat muodosta nimi, Visual Studio näkyy **Luominen yhteenveto**, mukaan lukien kaikki liittyvät yksikön testi projekteista tulokset.

    ![][29]

1. [Azure perinteinen portaalissa](http://go.microsoft.com/fwlink/?LinkID=213885)voit tarkastella liittyvät käyttöönoton **ominaisuuksissa** -välilehden väliaikaisen ympäristön valittaessa.

    ![][30]

1.  Siirry oman sivuston URL-osoite. Web-sovelluksen Valitse **Selaa, Valitse komentopalkista** . Valitse pilvipalveluun, URL-Osoitteen **Dashboard** -sivu, jolla näkyy pilvipalveluun käyttöympäristön väliaikaisen **Quick Glance** -osassa. Ominaisuuksissa pilvipalveluihin jatkuva integroinnissa julkaistaan väliaikaisen ympäristön oletusarvoisesti. Voit muuttaa määrittämällä **Vaihtoehtoisen Cloud palvelun ympäristön** -ominaisuuden arvoksi **tuotannon**. Tässä näyttökuva näkyy, missä sivuston URL-osoite pilvipalvelussa raporttinäkymäsivu.

    ![][31]

    Uusi selaimen välilehti avautuu käynnissä sivuston tulee näkyviin.

    ![][32]

    Jos projektin tehdä muita muutoksia, voit käynnistimen Lisää muodostaa ja saavutustasopisteet useita ominaisuuksissa cloud Services. Uusimman kohdalla lukee aktiivinen.

    ![][33]

## <a name="5-redeploy-an-earlier-build"></a>5: Ota aiemmissa Muodosta uudelleen

Tämä vaihe koskee pilvipalveluihin, ja se on valinnainen. Azure perinteinen-portaalissa Valitse aiemmissa käyttöönotto ja valitse sitten kelata sivustoon aiempi sisäänkuittauksen **Ota uudelleen** -painiketta.  Huomaa, että tämä käynnistää TFS-uusi versio ja luo uusi merkintä käyttöönoton historia.

![][34]

## <a name="6-change-the-production-deployment"></a>6: Muuta tuotannon käyttöönotto

Tämä vaihe koskee vain pilvipalveluihin, ei verkkosovelluksissa. Kun olet valmis, voit muuntaa tuotantoympäristössä väliaikaisen ympäristön valitsemalla **Vaihda** -painike Azure perinteinen-portaalissa. Äskettäin käyttöön väliaikaisen-ympäristö on ylennyksen tuotannon ja edellisen tuotantoympäristössä mahdollisesti muuttuu väliaikaisen-ympäristössä. Aktiivinen käyttöönoton saattavat olla erilaiset tuotannon ja väliaikainen ympäristössä, mutta uusimmat versiot käyttöönoton historia on sama ympäristön riippumatta.

![][35]

## <a name="7-run-unit-tests"></a>7: yksikön testien suorittaminen

Tämä vaihe koskee vain web apps, ei cloud services. Jos haluat sijoittaa laatu porttina ympäristöön, voit suorittaa yksikön testien ja jos ne eivät toimi, voit lopettaa käyttöönotto.

1.  Lisää Visual Studion yksikön testiprojektin.

    ![][39]

1.  Lisää testattava projektin projektiviittauksia.

    ![][40]

1.  Lisää mittayksikkö jotkin testit. Aloita yritä tyhjä testi, jolla aina välittää.

        ```
        using System;
        using Microsoft.VisualStudio.TestTools.UnitTesting;

        namespace UnitTestProject1
        {
            [TestClass]
            public class UnitTest1
            {
                [TestMethod]
                [ExpectedException(typeof(NotImplementedException))]
                public void TestMethod1()
                {
                    throw new NotImplementedException();
                }
            }
        }
        ```

1.  Muodosta kuvauksen muokkaaminen, **prosessi** -välilehti ja laajenna **testi** -solmu.

1.  **Virheiden muodosta testi virheen** arvoksi True. Tämä tarkoittaa, että käyttöönoton ei ilmene, ellet testejä Välitä.

    ![][41]

1.  Jono uusi versio.

    ![][42]

    ![][43]

1. Kun Luo jatkaa, valitse sen edistymisestä.

    ![][44]

    ![][45]

1. Kun koonti on valmis, tarkista tulokset.

    ![][46]

    ![][47]

1.  Yritä luoda testi, jolla epäonnistuu. Lisää uusi testi kopioimalla ensimmäisen vaiheen, nimetä sen ja kommentoi pois rivin tunnus, jolla mukaan NotImplementedException on odotettavissa poikkeuksen.

        ```
        [TestMethod]
        //[ExpectedException(typeof(NotImplementedException))]
        public void TestMethod2()
        {
            throw new NotImplementedException();
        }
        ```

1. Valitse Muuta muotoon jonon uusi versio.

    ![][48]

1. Jos haluat lisätietoja virheestä testitulokset tarkastelu

    ![][49]

    ![][50]

## <a name="next-steps"></a>Seuraavat vaiheet
Lisätietoja yksikön Visual Studio Team Servicesissä testaus on [yksikön testit oman muodosta](http://go.microsoft.com/fwlink/p/?LinkId=510474). Jos käytät Git, katso [Jaa Git lähdekoodin](http://www.visualstudio.com/get-started/share-your-code-in-git-vs.aspx) ja [Jatkuva käyttöönoton Azure-sovelluksen palveluun](../app-service-web/app-service-continuous-deployment.md).  Saat lisätietoja Visual Studio Team Services [Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkId=253861).

[0]: ./media/cloud-services-continuous-delivery-use-vso/tfs0.PNG
[1]: ./media/cloud-services-continuous-delivery-use-vso/tfs1.png
[2]: ./media/cloud-services-continuous-delivery-use-vso/tfs2.png

[5]: ./media/cloud-services-continuous-delivery-use-vso/tfs5.png
[6]: ./media/cloud-services-continuous-delivery-use-vso/tfs6.png
[7]: ./media/cloud-services-continuous-delivery-use-vso/tfs7.png
[8]: ./media/cloud-services-continuous-delivery-use-vso/tfs8.png
[9]: ./media/cloud-services-continuous-delivery-use-vso/tfs9.png
[10]: ./media/cloud-services-continuous-delivery-use-vso/tfs10.png
[11]: ./media/cloud-services-continuous-delivery-use-vso/tfs11.png
[12]: ./media/cloud-services-continuous-delivery-use-vso/tfs12.png
[13]: ./media/cloud-services-continuous-delivery-use-vso/tfs13.png
[14]: ./media/cloud-services-continuous-delivery-use-vso/tfs14.png
[15]: ./media/cloud-services-continuous-delivery-use-vso/tfs15.png
[16]: ./media/cloud-services-continuous-delivery-use-vso/tfs16.png
[17]: ./media/cloud-services-continuous-delivery-use-vso/tfs17.png
[18]: ./media/cloud-services-continuous-delivery-use-vso/tfs18.png
[19]: ./media/cloud-services-continuous-delivery-use-vso/tfs19.png
[20]: ./media/cloud-services-continuous-delivery-use-vso/tfs20.png
[21]: ./media/cloud-services-continuous-delivery-use-vso/tfs21.png
[22]: ./media/cloud-services-continuous-delivery-use-vso/tfs22.png
[23]: ./media/cloud-services-continuous-delivery-use-vso/tfs23.png
[24]: ./media/cloud-services-continuous-delivery-use-vso/tfs24.png
[25]: ./media/cloud-services-continuous-delivery-use-vso/tfs25.png
[26]: ./media/cloud-services-continuous-delivery-use-vso/tfs26.png
[27]: ./media/cloud-services-continuous-delivery-use-vso/tfs27.png
[28]: ./media/cloud-services-continuous-delivery-use-vso/tfs28.png
[29]: ./media/cloud-services-continuous-delivery-use-vso/tfs29.png
[30]: ./media/cloud-services-continuous-delivery-use-vso/tfs30.png
[31]: ./media/cloud-services-continuous-delivery-use-vso/tfs31.png
[32]: ./media/cloud-services-continuous-delivery-use-vso/tfs32.png
[33]: ./media/cloud-services-continuous-delivery-use-vso/tfs33.png
[34]: ./media/cloud-services-continuous-delivery-use-vso/tfs34.png
[35]: ./media/cloud-services-continuous-delivery-use-vso/tfs35.png
[36]: ./media/cloud-services-continuous-delivery-use-vso/tfs36.PNG
[37]: ./media/cloud-services-continuous-delivery-use-vso/tfs37.PNG
[38]: ./media/cloud-services-continuous-delivery-use-vso/AdvancedMSBuildSettings.PNG
[39]: ./media/cloud-services-continuous-delivery-use-vso/AddUnitTestProject.PNG
[40]: ./media/cloud-services-continuous-delivery-use-vso/AddProjectReferences.PNG
[41]: ./media/cloud-services-continuous-delivery-use-vso/EditBuildDefinitionForTest.PNG
[42]: ./media/cloud-services-continuous-delivery-use-vso/QueueNewBuild.PNG
[43]: ./media/cloud-services-continuous-delivery-use-vso/QueueBuildDialog.PNG
[44]: ./media/cloud-services-continuous-delivery-use-vso/BuildInTeamExplorer.PNG
[45]: ./media/cloud-services-continuous-delivery-use-vso/BuildRequestInfo.PNG
[46]: ./media/cloud-services-continuous-delivery-use-vso/BuildSucceeded.PNG
[47]: ./media/cloud-services-continuous-delivery-use-vso/TestResultsPassed.PNG
[48]: ./media/cloud-services-continuous-delivery-use-vso/CheckInChangeToMakeTestsFail.PNG
[49]: ./media/cloud-services-continuous-delivery-use-vso/TestsFailed.PNG
[50]: ./media/cloud-services-continuous-delivery-use-vso/TestsResultsFailed.PNG

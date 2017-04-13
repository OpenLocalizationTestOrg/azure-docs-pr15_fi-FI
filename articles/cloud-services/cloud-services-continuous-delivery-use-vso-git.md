<properties
    pageTitle="Jatkuva toimituksen Git ja Visual Studio Team Services Azure | Microsoft Azure"
    description="Opettele määrittämään Visual Studio Team Services ryhmän projektien Git avulla voit automaattisesti muodosta ja ota käyttöön Azure App palvelun tai cloud Services-palveluissa Web App-ominaisuus."
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

# <a name="continuous-delivery-to-azure-using-visual-studio-team-services-and-git"></a>Jatkuva toimittamista Azure Visual Studio Team Services ja Git käyttäminen

Voit käyttää Visual Studio Team Services ryhmän projekteja isännöidä Git säilön lähde-koodin ja automaattisesti luominen ja Azure web Apps-sovellusten käyttöön tai cloud services aina, kun painat vahvistamista säilöön.

Sinun on Visual Studio 2013 ja Azure SDK asennettuna. Jos sinulla ei vielä ole Visual Studio 2013, lataa se valitsemalla [www.visualstudio.com](http://www.visualstudio.com) **Aloita maksutta** olevaa linkkiä. Asenna Azure SDK [täältä](http://go.microsoft.com/fwlink/?LinkId=239540).


> [AZURE.NOTE]Sinun on suoritettava tässä opetusohjelmassa Visual Studio Team Services-tilin: Voit [avata Visual Studio Team Services-tilin maksutta](http://go.microsoft.com/fwlink/p/?LinkId=512979).

Voit määrittää pilvipalvelussa, voit automaattisesti muodosta ja ota käyttöön Azure Visual Studio ryhmän Services-palveluiden avulla, toimimalla seuraavasti.

## <a name="1-create-a-git-repository"></a>1: Luo Git säilö

1. Jos sinulla ei vielä ole Visual Studio Team Services-tilin, voit hankkia sen [tästä](http://go.microsoft.com/fwlink/?LinkId=397665). Kun luot ryhmäprojektin, valitse Git järjestelmässä Ohjausobjektin lähde. Visual Studio yhdistäminen ryhmäprojektin ohjeiden mukaan.

2. Valitse **Ryhmän Explorer** **Kloonaa tämän säilöön** -linkki.

    ![][3]

3. Määrittää paikallisen tietokannan sijainnin ja valitse sitten **Kloonaa** -painiketta.

## <a name="2-create-a-project-and-commit-it-to-the-repository"></a>2: projektin luominen ja vahvista se säilöön

1. **Ryhmän Explorer** **ratkaisut** -osassa valitsemalla **Uusi** linkki uuden projektin luominen paikalliseen säilöön.

    ![][4]

2. Voit ottaa verkkosovellukseen tai pilvipalveluun (Azure-sovellus) käyttöön noudattamalla tämän vaiheittaisen kuvauksen suorittamiseksi. Uuden projektin Azure pilvipalvelussa tai ASP.NET MVC uuden projektin luominen Varmista, että projektin kohteena .NET Framework 4 tai uudempi. Jos olet luomassa cloud palvelun projektin, Lisää ASP.NET MVC web-rooli ja työntekijän rooli.
Jos haluat luoda verkkosovellukseen, valitse **ASP.NET Web-sovelluksen** project-malli ja valitse sitten **MVC**. Lisätietoja on kohdassa [Luo ASP.NET web-sovelluksen Azure App palvelu](../app-service-web/web-sites-dotnet-get-started.md) .

3. Avaa pikavalikon ratkaisu ja valitse **Vahvista**.

    ![][7]

4. Jos olet käyttänyt Git Visual Studio Team Services ensimmäistä kertaa, tarvitset antamaan joitakin tietoja Ilmoita olevasi Git. Kirjoita **Ryhmän Explorer** **Odottavia muutoksia** -alueeseen että käyttäjänimi ja sähköpostiosoite. Kirjoita kommentti vahvistamista ja valitse sitten **Vahvista** -painiketta.

    ![][8]

5. Huomautus Voit lisätä tai poistaa tietyn muutokset, kun tarkistat asetukset. Jos haluamasi muutokset jätetään pois, valitse **Sisällytä kaikki**.

6. Olet nyt sitoutunut muutokset tietovaraston paikallisen tietokannan. Seuraavaksi synkronoida muutokset palvelimeen valitsemalla **Synkronoi** -linkkiä.

## <a name="3-connect-the-project-to-azure"></a>3: projektin yhdistäminen Azure

1. Nyt kun olet luonut Git säilön Visual Studio Team Servicesissä ei lähde lisäkoodin avulla, olet valmis git säilöön yhdistäminen Azure.  Valitse [Azure perinteinen portal](http://go.microsoft.com/fwlink/?LinkID=213885)cloud-palvelun tai web Appissa tai luoda uuden valitsemalla + vasemmassa alakulmassa ja valitse **Pilvipalvelussa** tai **Web App** ja valitse **Nopea luominen**-kuvaketta.

    ![][9]

3. Cloud Services-palveluissa Valitse **määrittäminen julkaisemista Visual Studio Team Services** -linkki. Web Apps-sovellukset Napsauta **tietolähteen ohjausobjektin käyttöönoton määrittämisen** -linkkiä.

    ![][10]

2. Kirjoita ohjatun toiminnon tekstiruutuun Visual Studio Team Services-tilin nimi ja valitsemalla **Määritä nyt** -linkki. Sinulta saatetaan pyytää kirjautumaan.

    ![][11]

3. **Yhteyden pyytää** avautuvassa valintaikkunassa valitsemalla **Hyväksy** ja Määritä Azure ryhmäprojektin paikallaan Visual Studio Team Services.

    ![][12]

4. Kun luvan onnistuu, näet avattavan luettelon, joka sisältää Visual Studio Team Services-ryhmän projekteja.  Valitse, jonka loit edellisessä vaiheessa ryhmäprojektin nimeä ja valitse ohjatun toiminnon valintamerkki-painiketta.

    ![][13]

    Kun seuraavan kerran vahvistamista push lisääminen säilöön Visual Studio Team Services muodosta ja ota käyttöön projektin Azure.

## <a name="4-trigger-a-rebuild-and-redeploy-your-project"></a>4: käynnistettävän muodosta ja ota uudelleen projektin

1. Visual Studiossa Avaa tiedosto ja muuta sen. Esimerkiksi muuttaa tiedoston `_Layout.cshtml` näkymät-kohdassa\\jaetun kansion MVC web-rooli.

    ![][17]

2. Muokkaa sivuston alatunnisteteksti ja Tallenna tiedosto.

    ![][18]

3. **Ratkaisunhallinnassa**Avaa pikavalikko ratkaisu-solmu, projektin solmun tai olet muuttanut tiedosto ja valitse **Vahvista**.

4. Kirjoita kommentti ja valitse **Vahvista**.

    ![][20]

5. Valitse **synkronointi** -linkki.

    ![][38]

6. Valitse oman Vahvista painaa Visual Studio Team Servicesissä säilöön **Push** -linkki. (Voit käyttää myös **Synkronoi** -painiketta voit kopioida oman tapahtumasarjan vahvistukset säilö. Ero on se, että **Sync** hakee myös uusimmat muutokset säilöstä.)

    ![][39]

7. Palauttaa **Ryhmän Explorerissa** aloitussivulle valitsemalla **Aloitus** .

    ![][21]

8. Valitse Näytä versiot käynnissä **muodostaa** .

    ![][22]

    **Ryhmän Explorer** näyttää muodosta saatu oman sisäänkuittauksen.

    ![][23]

9. Voit tarkastella yksityiskohtainen loki muodosta edetessä kaksoisnapsauttamalla käynnissä muodosta nimi.

10. Luo ollessa käynnissä Tutustu koontiversion määrityksen, joka on luotu, kun ohjatun toiminnon avulla voit luoda linkin Azure.  Avaa pikavalikko koontiversion määrityksen ja valitse **Muokkaa muodosta määritys**.

    ![][25]

11. **Käynnistimen** -välilehdessä näkyy, että koontiversion määrityksen määritetään voit luoda jokaisen sisäänkuittauksen, oletusarvoisesti. (Pilvipalveluun, for Visual Studio Team Services muodostaa ja ottaa käyttöön perusmuodon haaran väliaikaisen-ympäristöön automaattisesti. Sinun on tehtävä manuaalinen vaihe sivustoihin käyttöön edelleen. Web-sovelluksen, joka ei ole väliaikaisen ympäristössä se ottaa käyttöön perusmuodon haaran suoraan sivustoihin.

    ![][26]

1. **Prosessi** -välilehden näet käyttöönottoympäristö on määritetty cloud palvelun tai web-sovelluksen nimi.

    ![][27]

1. Määrittää ominaisuuksien arvot, jos haluat kuin oletusarvoiset eri arvoja. Azure julkaisun ominaisuudet ovat **käyttöönotto** -osasta ja joudut ehkä myös MSBuild parametrien määrittäminen. Esimerkiksi pilvestä palvelun projektin service-määrityksen kuin "Pilven", Määritä MSbuild parametrit `/p:TargetProfile=[YourProfile]` *[YourProfile]* , jossa vastaa palvelun kokoonpanotiedosto, kuten ServiceConfiguration nimellä. *YourProfile*.cscfg.

    Seuraavassa taulukossa näkyvät käytettävissä olevat ominaisuudet **käyttöönotto** -osasta:

  	|Ominaisuus|Oletusarvo|
  	|---|---|
  	|Salli luotettu varmenteet|Jos arvo on EPÄTOSI, SSL-varmenteita on kirjauduttava päämyöntäjä.|
  	|Salli päivitys|Päivitä olemassa olevaan käyttöympäristöön sijaan uuden käyttöönoton avulla. Säilyttää IP-osoite.|
  	|Älä poista|Jos arvo on TOSI, ei korvaa olemassa olevaan toisiinsa liittymättömiä käyttöympäristöön (päivitys on sallittu).|
  	|Polun käyttöönottoasetukset|Web-sovelluksen suhteessa repo pääkansio .pubxml tiedoston polku. Ohitettavaksi pilvipalveluihin.|
  	|SharePoint-käyttöönotto-ympäristössä|Sama kuin palvelunimi.|
  	|Azure käyttöönotto-ympäristössä|Web app ja pilvi palvelunimi.|

1. Tässä vaiheessa oman muodosta on suoritettu onnistuneesti.

    ![][28]

1. Jos kaksoisnapsautat muodosta nimi, Visual Studio näkyy **Luominen yhteenveto**, mukaan lukien kaikki testitulokset liittyvän yksikön testi projekteista.

    ![][29]

1. [Azure perinteinen portaalissa](http://go.microsoft.com/fwlink/?LinkID=213885)voit tarkastella liittyvät käyttöönoton **ominaisuuksissa** -välilehden väliaikaisen ympäristön valittaessa.

    ![][30]

1.  Siirry oman sivuston URL-osoite. Web-sovelluksen Valitse **Selaa** -portaalissa. Valitse pilvipalveluun, URL-Osoitteen **Dashboard** -sivu, jolla näkyy väliaikaisen ympäristön **Quick Glance** -osassa.

    Ominaisuuksissa pilvipalveluihin jatkuva integroinnissa julkaistaan väliaikaisen ympäristön oletusarvoisesti. Voit muuttaa määrittämällä **Vaihtoehtoisen Cloud palvelun ympäristön** -ominaisuuden arvoksi **tuotannon**. Seuraavassa on missä sivuston URL-osoite pilvipalvelussa raporttinäkymäsivu.

    ![][31]

    Uusi selaimen välilehti avautuu käynnissä sivuston tulee näkyviin.

    ![][32]

1.  Muut muutokset projektiin, voit käynnistimen Lisää muodostaa ja saavutustasopisteet useita ominaisuuksissa. Uusimman on merkitty aktiiviseksi.

    ![][33]

## <a name="5-redeploy-an-earlier-build"></a>5: Ota aiemmissa Muodosta uudelleen

Tämä vaihe on valinnainen. Azure perinteinen-portaalissa Valitse aiemmissa käyttöönotto ja valitse voit kelata sivustoon aiempi sisäänkuittauksen **Ota uudelleen** . Huomaa, että tämä käynnistää TFS-uusi versio ja luo uusi merkintä käyttöönoton historia.

![][34]

## <a name="6-change-the-production-deployment"></a>6: Muuta tuotannon käyttöönotto

Kun olet valmis, voit muuntaa tuotantoympäristössä väliaikaisen ympäristön valitsemalla **Vaihda** Azure perinteinen portaaliin. Äskettäin käyttöön väliaikaisen-ympäristö on ylennyksen tuotannon ja edellisen tuotantoympäristössä mahdollisesti muuttuu väliaikaisen-ympäristössä. Aktiivinen käyttöönoton saattavat olla erilaiset tuotannon ja väliaikainen ympäristössä, mutta uusimmat versiot käyttöönoton historia on sama ympäristön riippumatta.

![][35]

## <a name="7-deploy-from-a-working-branch"></a>7: työn päätasolta käyttöönotto.

Kun käytät Git, haluat muuttaa toimimasta haaran yleensä ja integroida perusmuodon haaran, kun yhteyttä kehittäminen saavuttaa valmiiseen tilaan. Suunnitteluvaiheessa projektin kannattaa muodosta ja ota käyttöön toimimasta haaran Azure.

1. **Ryhmän Explorer** **Koti** -painiketta ja valitse sitten **haaroja** -painiketta.

    ![][40]

2. Valitse **Uusi haara** -linkki.

    ![][41]

3. Haaran "käytät", kuten nimi ja valitse **Luo haaran**. Tämä luo uusi paikallinen haaran.

    ![][42]

4. Julkaise haaran. Valitse haaran nimi **julkaisemattomien haaroja**ja sitten **Julkaise**.

    ![][44]

6. Oletusarvon mukaan vain perusmuodon haaran muutokset käynnistäminen jatkuva muodosta. Määrittämään jatkuva muodosta toimimasta sivuliike, valitse **Ryhmän**Explorerissa **muodostaa** -sivu ja valitse **Muokata luoda määritys**.

7. Avaa **Tietolähteen asetukset** -välilehti. Valitse **Tarkkailtavat haarautuu jatkuva integrointi ja muodosta**-kohdassa **Lisää uusi rivi napsauttamalla tätä**.

    ![][47]

8. Määritä luomasi, kuten refs/perheen/toimimasta haaran.

    ![][48]

9. Muutosten tekeminen koodi, Avaa pikavalikon muutettu tiedosto ja valitse **Vahvista**.

    ![][43]

10. Valitse **Synkronoimattomia tapahtumasarjan vahvistukset** -linkki ja valitse **Synkronoi** -painiketta tai kopioi muutokset Visual Studio Team Services toimimasta haara kopio **Push** -linkkiä.

    ![][45]

11. **Muodostaa** -näkymään ja Etsi muodosta, joka on heti käytössä käynnistänyt toimimasta haaran varten.

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja käyttövinkit Visual Studio Team Services Git käyttämisestä on artikkelissa [kehittäminen ja jakaa Git Visual Studiossa lähdekoodin](http://www.visualstudio.com/get-started/share-your-code-in-git-vs.aspx) ja tietoja Git säilöön, jonka Visual Studio Team Services Azure julkaiseminen ei hallinnasta on kohdassa [Jatkuva käyttöönoton Azure-sovelluksen palveluun](../app-service-web/app-service-continuous-deployment.md). Saat lisätietoja Visual Studio Team Services [Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkId=253861).

[0]: ./media/cloud-services-continuous-delivery-use-vso/tfs0.PNG
[1]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateTeamProjectInGit.PNG
[2]: ./media/cloud-services-continuous-delivery-use-vso/tfs2.png
[3]: ./media/cloud-services-continuous-delivery-use-vso-git/CloneThisRepository.PNG
[4]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateNewSolutionInClonedRepo.PNG
[7]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitMenuItem.PNG
[8]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitAChange2.PNG
[9]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateCloudService.PNG
[10]: ./media/cloud-services-continuous-delivery-use-vso-git/SetUpPublishingWithVSO.PNG
[11]: ./media/cloud-services-continuous-delivery-use-vso-git/AuthorizeConnection.PNG
[12]: ./media/cloud-services-continuous-delivery-use-vso-git/ConnectionRequest.PNG
[13]: ./media/cloud-services-continuous-delivery-use-vso-git/ChooseARepo3.PNG
[14]: ./media/cloud-services-continuous-delivery-use-vso/tfs14.png
[15]: ./media/cloud-services-continuous-delivery-use-vso/tfs15.png
[16]: ./media/cloud-services-continuous-delivery-use-vso/tfs16.png
[17]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs17.png
[18]: ./media/cloud-services-continuous-delivery-use-vso-git/MakeACodeChange.PNG
[20]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitAChange2.PNG
[21]: ./media/cloud-services-continuous-delivery-use-vso-git/TeamExplorerHome.png
[22]: ./media/cloud-services-continuous-delivery-use-vso-git/TeamExplorerBuilds.PNG
[23]: ./media/cloud-services-continuous-delivery-use-vso-git/BuildInQueue.png
[24]: ./media/cloud-services-continuous-delivery-use-vso/tfs24.png
[25]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs25.png
[26]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs26.png
[27]: ./media/cloud-services-continuous-delivery-use-vso-git/ProcessTab.PNG
[28]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs28.png
[29]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs29.png
[30]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs30.png
[31]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs31.png
[32]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs32.png
[33]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs33.png
[34]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs34.png
[35]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs35.png
[36]: ./media/cloud-services-continuous-delivery-use-vso/tfs36.PNG
[37]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateANewAccount.PNG
[38]: ./media/cloud-services-continuous-delivery-use-vso-git/SyncChanges2.PNG
[39]: ./media/cloud-services-continuous-delivery-use-vso-git/PushCurrentBranch.PNG
[40]: ./media/cloud-services-continuous-delivery-use-vso-git/BranchesInTeamExplorer.PNG
[41]: ./media/cloud-services-continuous-delivery-use-vso-git/NewBranch.PNG
[42]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateBranch.PNG
[43]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitAChange2.PNG
[44]: ./media/cloud-services-continuous-delivery-use-vso-git/PublishBranch.PNG
[45]: ./media/cloud-services-continuous-delivery-use-vso-git/SyncChanges2.PNG
[47]: ./media/cloud-services-continuous-delivery-use-vso-git/SourceSettingsPage.PNG
[48]: ./media/cloud-services-continuous-delivery-use-vso-git/IncludeWorkingBranch.PNG

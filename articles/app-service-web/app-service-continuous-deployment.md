<properties
    pageTitle="Jatkuva käyttöönoton Azure App palveluun | Microsoft Azure"
    description="Opettele käyttöön jatkuva käyttöönoton Azure-sovelluksen palveluun."
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
    ms.date="10/28/2016"
    ms.author="dariagrigoriu"/>
    
# <a name="continuous-deployment-to-azure-app-service"></a>Jatkuva käyttöönoton Azure sovelluksen-palveluun

Tässä opetusohjelmassa näytetään, miten voit määrittää jatkuva käyttöönoton työnkulun [Azure App Service] -sovelluksen. Sovelluksen palvelun integrointi BitBucket, GitHub ja Visual Studio ryhmän Services (VSTS) mahdollistaa jatkuvan käyttöönoton työnkulun missä Azure hakee johonkin näistä palveluista julkaista projektin viimeisimmät päivitykset. Jatkuva käyttöönotto on hyvä vaihtoehto projekteille, jossa useita ja toistuvat maksut integroidaan.

## <a name="overview"></a>Ota jatkuva käyttöönotto

Jos haluat ottaa käyttöön jatkuva käyttöönotto 

1. App-sisällön julkaiseminen säilöön, jota käytetään jatkuva käyttöönottoa varten.  
    Saat lisätietoja näiden palvelujen projektin julkaiseminen [Luo repo (GitHub)], [Luo repo (BitBucket)]ja [VSTS käytön aloittaminen].

2. Napsauta sinua sovelluksen valikon sivu [Azure portal]- **Sovellusten KÄYTTÖÖNOTON > asennusvaihtoehdot**. **Valitse**lähde ja valitse sitten käyttöönoton lähteen.  

    ![](./media/app-service-continuous-deployment/cd_options.png)
    
    > [AZURE.NOTE] Voit määrittää VSTS tilin App palvelun käyttöönottoa varten, katso tässä [opetusohjelmassa](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).
    
3. Todennus-työnkulun suorittaminen 

4. Valitse projekti- ja ottaa-haara **käyttöönoton lähde** -sivu. Kun olet valmis, valitse **OK**.
  
    ![](./media/app-service-continuous-deployment/github_option.png)

    > [AZURE.NOTE] Kun otat käyttöön jatkuva-ympäristö, jonka GitHub tai BitBucket, sekä julkisista ja yksityisistä projektien näkyvät.

    Sovelluksen palvelun Luo liittyy valitun säilöön, hakee määritettyyn päätasolta tiedostot ja ylläpitää Kloonaa oman tietovaraston, kun sovellus Service-sovellus. Kun määrität VSTS jatkuva käyttöönoton Azure-portaalista, integrointi käyttää sovelluksen palvelun [Kudu käyttöönotto-ohjelma](https://github.com/projectkudu/kudu/wiki), joka automatisoi jo muodostukseen ja käyttöönottoon tehtävien jokaisen `git push`. Sinun ei tarvitse erikseen määrittäminen VSTS jatkuva käyttöönottoa. Kun tämä prosessi on valmis, **asennusvaihtoehdot** app sivu näkyy aktiivisen käyttöönoton, joka ilmaisee käyttöönoton onnistui.

5. Voit tarkistaa sovellus on otettu, valitsemalla sovelluksen sivu Azure-portaalissa yläreunassa **URL-osoite** . 

6. Jatkuva käyttöönotto tapahtuu valittua säilöstä vahvistamiseksi push muutoksen säilöön. Sovelluksen pitäisi päivittää muutoksia vastaavaksi pian push säilöön päätyttyä. Voit varmistaa, että viestit on Päivitä-sovelluksesi **Käyttöönottoasetukset** -sivu.

## <a name="VSsolution"></a>Jatkuva Visual Studio-ratkaisun käyttöönotto 

Visual Studio-ratkaisun valitseminen Azure-sovelluksen palveluun on yhtä helppoa kuin yksinkertainen index.html tiedoston valitseminen. Sovelluksen palvelun käyttöönottoprosessin tehostaa kaikki tiedot, kuten sovelluksen binaaritiedostot kehittämistä sekä palauttaminen NuGet riippuvuudet. Voit tietolähteen ohjausobjektin parhaita käytäntöjä säilyttää vain Git säilöön-koodi ja antaa muiden huolehtia App palvelun käyttöönoton.

Visual Studio-ratkaisun valitseminen sovelluksen palveluun vaiheet ovat samat kuin [edellisessä osassa](#overview), jos määrität ratkaisu ja säilöön seuraavasti:

-   Visual Studio tietolähteen hallinta-vaihtoehdon avulla voit luoda `.gitignore` tiedosto, kuten kuvan alla tai manuaalisesti lisätä `.gitignore` tiedosto säilöön-pääsivustoon samalla tavalla kuin tässä [.gitignore otoksen](https://github.com/github/gitignore/blob/master/VisualStudio.gitignore)sisältöä. 

    ![](./media/app-service-continuous-deployment/VS_source_control.png)
 
-   Lisää koko ratkaisun kansiopuu lisääminen säilöön säilöön päätasolla .sln-tiedoston.

Kun olet lisääminen säilöön kuvatulla määrittäminen ja sovelluksen määritetyissä Azure jatkuva julkaisun olevista online Git säilöjen tietoihin, voit kehittää ASP.NET-sovelluksen paikallisesti Visual Studiossa ja jatkuvasti asentavat koodisi yksinkertaisesti valitseminen muutokset online Git-säilöön.

## <a name="disableCD"></a>Poista jatkuva käyttöönotto

Jos haluat poistaa käytöstä jatkuva käyttöönotto 

1. Napsauta sinua sovelluksen valikon sivu [Azure portal]- **Sovellusten KÄYTTÖÖNOTON > asennusvaihtoehdot**. Valitse **Katkaise yhteys** **Käyttöönottoasetukset** -sivu.

    ![](./media/app-service-continuous-deployment/cd_disconnect.png)    

2. Kun vastaamalla **Kyllä** , näyttöön tulee vahvistussanoma, voit palata sinua sovelluksen sivu ja valitse **Sovellusten KÄYTTÖÖNOTON > asennusvaihtoehdot** Jos haluat määrittää julkaisun toisesta lähteestä.

## <a name="additional-resources"></a>Lisäresursseja

* [Voit tutkia jatkuva käyttöönoton yleisiä ongelmia](https://github.com/projectkudu/kudu/wiki/Investigating-continuous-deployment)
* [PowerShellin käyttäminen Azure varten]
* [Azure komentorivivalitsimet työkalujen käyttämisestä Mac-ja Linux]
* [Git dokumentaatio]
* [Projektin Kudu](https://github.com/projectkudu/kudu/wiki)

>[AZURE.NOTE] Jos haluat aloittaa Azure App palvelun ennen rekisteröimässä Azure-tili, siirry [Yritä App palvelu](http://go.microsoft.com/fwlink/?LinkId=523751), jossa lyhytkestoinen starter verkkosovellukseen heti voit luoda sovelluksen-palvelussa. Ei ole pakollinen; luottokortit ei ole sitoumukset.

[Azure sovelluksen-palvelu]: https://azure.microsoft.com/en-us/documentation/articles/app-service-changes-existing-services/ 
[Azure portal]: https://portal.azure.com
[VSTS Portal]: https://www.visualstudio.com/en-us/products/visual-studio-team-services-vs.aspx
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[PowerShellin käyttäminen Azure varten]: ../articles/powershell-install-configure.md
[Azure komentorivivalitsimet työkalujen käyttämisestä Mac-ja Linux]: ../articles/xplat-cli-install.md
[Git dokumentaatio]: http://git-scm.com/documentation

[Luo repo (GitHub)]: https://help.github.com/articles/create-a-repo
[Luo repo (BitBucket)]: https://confluence.atlassian.com/display/BITBUCKET/Create+an+Account+and+a+Git+Repo
[VSTS käytön aloittaminen]: https://www.visualstudio.com/get-started/overview-of-get-started-tasks-vs
[Continuous delivery to Azure using Visual Studio Team Services]: ../articles/cloud-services/cloud-services-continuous-delivery-use-vso.md

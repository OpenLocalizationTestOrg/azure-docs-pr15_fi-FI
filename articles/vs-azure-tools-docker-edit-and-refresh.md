<properties
   pageTitle="Paikallisen Docker säilön sovellusten virheenkorjaus | Microsoft Azure"
   description="Sovellus, jossa on käytössä paikallinen Docker säilön muokkaaminen, Päivitä säilö kautta muokkaaminen ja päivittäminen ja määrittäminen virheenkorjaus keskeytyskohdat"
   services="azure-container-service"
   documentationCenter="na"
   authors="mlearned"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="07/22/2016"
   ms.author="mlearned" />

# <a name="debugging-apps-in-a-local-docker-container"></a>Paikallinen Docker säilön sovellusten virheenkorjaus

## <a name="overview"></a>Yleiskatsaus
Visual Studio-työkalut Docker avulla voit tehdä ja vahvista sovelluksen paikallisesti Linux Docker säilö.
Sinun ei tarvitse säilö uudelleen aina, kun teet muuta koodi.
Tämän artikkelin esimerkeissä kuvataan "Muokkaaminen ja päivitys"-toiminnon käyttämisestä alkavat ASP.NET Core-Web-sovelluksen paikallisen Docker säilön, tee tarvittavat muutokset ja päivitä sitten selain näet muutokset.
Se myös kerrotaan, miten voit määrittää keskeytyskohdat virheenkorjaus.

> [AZURE.NOTE] Windows-säilö tuki tulossa tulevissa versioissa

## <a name="prerequisites"></a>Edellytykset
Seuraavia työkaluja on oltava asennettuna.

- [Visual Studio 2015 päivityksen 2](https://go.microsoft.com/fwlink/?LinkId=691978)
- Asenna [Visual Studio 2015 päivitys 3](https://go.microsoft.com/fwlink/?LinkId=691129)
- [Microsoft ASP.NET Core 1.0 SDK-paketissa](https://go.microsoft.com/fwlink/?LinkID=809122)

Suorita Docker säilöjen paikallisesti on paikallinen docker asiakas.
Voit käyttää julkaistua [Docker työkaluryhmän](https://www.docker.com/products/overview#/docker_toolbox) joka edellyttää Hyper-V poistetaan käytöstä, tai voit käyttää [Windows Docker beetaversio,](https://beta.docker.com) joka käyttää Hyper-V ja vaatii Windows 10.

Jos käytät Docker työkaluryhmä, sinun on [määrittää Docker asiakkaan](./vs-azure-tools-docker-setup.md)

## <a name="1-create-a-web-app"></a>1. web-sovelluksen luominen

[AZURE.INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-add-docker-support"></a>2. Lisää Docker tuki

[AZURE.INCLUDE [Add docker support](../includes/vs-azure-tools-docker-add-docker-support.md)]


## <a name="3-edit-your-code-and-refresh"></a>3. Muokkaa koodia ja päivittäminen

Käydä nopeasti muutoksia, voit aloittaa sovelluksesi säilöön ja jatka tehdä muutoksia, tarkastelemalla IIS Express samalla tavalla kuin.

1. Ratkaisun määrityksen asettaminen `Debug` ja paina ** &lt;CTRL + F5 >** voivat laatia docker kuva ja suorita se paikallisesti.

    Kun säilö-kuva on muodostettu, ja se toimii Docker säilön, Visual Studio Käynnistä Web app-oletusselaimessa.
    Jos käytössäsi on Microsoft Edge-selaimella tai muussa on virheitä, katso [vianmääritys](vs-azure-tools-docker-troubleshooting-docker-errors.md) -osassa.

1. Siirry tietoja-sivulla, joka kertoo, jossa on on Microsoftin muokkaamiseen.

1. Palaa Visual Studio ja Avaa `Views\Home\About.cshtml`.

1. Lisää HTML-sisällön loppuun tiedosto ja Tallenna muutokset.

    ```
    <h1>Hello from a Docker Container!</h1>
    ```

1.  Tarkastelet tulostusikkunassa, kun .NET koonti on valmis, ja näet seuraavista riveistä, palaa selaimen ja tietoja-sivu päivitetään.

    ```
    Now listening on: http://*:80
    Application started. Press Ctrl+C to shut down
    ```

1.  Tekemäsi muutokset on käytetty!

## <a name="4-debug-with-breakpoints"></a>4. virheenkorjaus keskeytyskohdat kanssa

Usein muutokset on edelleen tarkastuksen hyödyntäminen Visual Studio muistin ominaisuuksia.

1.  Palaa Visual Studio ja avaaminen`Controllers\HomeController.cs`

1.  Korvaa About() menetelmä sisällön seuraavasti:

    ```
    string message = "Your application description page from wthin a Container";
    ViewData["Message"] = message;
    ````

1.  Määrittää keskeytyskohdasta vasemmalla puolella `string message`... rivi.

1.  Osumien ** &lt;F5 >** Käynnistä virheenkorjaus.

1.  Siirry osumien oman keskeytyskohdasta tietoja-sivu.

1.  Siirry Visual Studiossa, voit tarkastella keskeytyskohdasta ja tarkasta viestin arvo.

    ![][2]

##<a name="summary"></a>Yhteenveto

[Visual Studio 2015 Työkalut Docker](https://aka.ms/DockerToolsForVS)saat tuottavuutta käsitteleminen kehittäminen Docker säilöön tuotannon-realism paikallisesti.

## <a name="troubleshooting"></a>Vianmääritys

[Visual Studio Docker kehittäminen vianmääritys](vs-azure-tools-docker-troubleshooting-docker-errors.md)

## <a name="more-about-docker-with-visual-studio-windows-and-azure"></a>Lisätietoja Visual Studio ja Windows Azure Docker

- [Docker Tools for Visual Studio](http://aka.ms/dockertoolsforvs) - säilön .NET Core lähdekoodin kehittäminen
- [Docker Tools for Visual Studio Team Services](http://aka.ms/dockertoolsforvsts) – muodosta ja ota käyttöön docker säilöt
- [Docker Tools for Visual Studio koodi](http://aka.ms/dockertoolsforvscode) - kieliä enemmän e2e skenaarioita tulossa docker tiedostojen muokkaamista varten
- [Windows-säilö tiedot](http://aka.ms/containers)– Windows Server ja Nano palvelimen tiedot
- [Azure säilö palvelun](https://azure.microsoft.com/services/container-service/) - [Azure säilö palvelun sisältö](http://aka.ms/AzureContainerService)
-    Katso Lisää esimerkkejä käyttäminen Docker [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 Yhdistä [esittely](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/)- [Docker käsitteleminen](https://github.com/Microsoft/HealthClinic.biz/wiki/Working-with-Docker) . Katso Lisää quickstarts-HealthClinic.biz esittely, [Azure Developer Työkalut Quickstarts](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).

## <a name="various-docker-tools"></a>Eri Docker Työkalut

[Jotkin hyvien docker työkalut (Steve Lasker blogiin)](https://blogs.msdn.microsoft.com/stevelasker/2016/03/25/some-great-docker-tools/)

## <a name="good-articles"></a>Hyvä artikkelit

[Johdanto Microservices-NGINX](https://www.nginx.com/blog/introduction-to-microservices/)

## <a name="presentations"></a>Esitykset

- [Steve Lasker: Ja Live Las Vegas 2016 - Docker e2e](https://github.com/SteveLasker/Presentations/blob/master/VSLive2016/Vegas/)
- [Johdanto ASP.NET Core @ luominen 2016 - kohtaa, johon olet osoitteessa esittely](https://channel9.msdn.com/Events/Build/2016/B810)
- [Säilöjen, kanavan 9 .NET-sovellusten kehittämisestä](https://blogs.msdn.microsoft.com/stevelasker/2016/02/19/developing-asp-net-apps-in-docker-containers/)

[2]: ./media/vs-azure-tools-docker-edit-and-refresh/breakpoint.png

<properties
   pageTitle="Vianmääritys Docker asiakkaan Windows Visual Studiossa | Microsoft Azure"
   description="Vianmääritys käytössä ilmenee, kun käytät Visual Studio Luo ja ota käyttöön Visual Studiossa Docker Windows verkkosovelluksissa."
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
   ms.date="06/08/2016"
   ms.author="allclark" />

# <a name="troubleshooting-visual-studio-docker-development"></a>Visual Studio Docker kehittäminen vianmääritys

Kun käsittelet Visual Studio Tools Docker esikatselua varten, voit kohdata ongelmia esikatselu laatu vuoksi.
Seuraavassa on muutamia yleisiä ongelmat ja ratkaisut.


## <a name="unable-to-validate-volume-mapping"></a>Ei voi vahvistaa aseman yhdistäminen
Äänenvoimakkuuden yhdistämismääritys tarvitaan lähdekoodia ja binaaritiedostot sovelluksen jakaminen säilössä sovelluksen-kansio.  Määritetyn aseman yhdistämismääritykset sisältyvät docker compose.dev.debug.yml ja docker compose.dev.release.yml tiedostot. Kun tiedostot on muutettu host käyttämääsi laitteeseen, säilöt vastaavat samalla kansiorakenne nämä muutokset.

Äänenvoimakkuuden yhdistämisen käyttöön **asetuksia** avaaminen Docker Windows "moby" ilmaisinalueen kuvaketta ja valitse sitten **Jaettu asemat** -välilehti.  Varmista, että joka isännöi projektin kirjain sekä % USERPROFILE % sijainti kirjain on jaettu tarkistamalla ne ja valitsemalla sitten **Käytä**.

Testaa Jos aseman yhdistäminen toimii, kun kohteessa on on jaettu, muodosta ja Visual Studio tai yritä F5-komennon seuraavasti kehote:

*Windows-komentokehotteen*

*[Huomautus: Tämä olettaa käyttäjät-kansio sijaitsee "C"-asema, ja se on jaettu.  Päivitä tarpeen mukaan, jos olet jakanut eri asemaan]*
```
docker run -it -v /c/Users/Public:/wormhole busybox
```

*Linux-säilöön*

```
/ # ls
```

Raportissa pitäisi näkyä kansioluettelon käyttäjät/julkinen-kansiosta.
Jos tiedostoja ei näkyvät ja /c/Users/Public-kansio ei ole tyhjä, äänenvoimakkuuden määritystä ei ole määritetty oikein. 

```
bin       etc       proc      sys       usr       wormhole
dev       home      root      tmp       var
```

Muuttaa madonreiän kansioon näet sisällön `/c/Users/Public` hakemisto:

```
/ # cd wormhole/
/wormhole # ls
AccountPictures  Downloads        Music            Videos
Desktop          Host             NuGet.Config     desktop.ini
Documents        Libraries        Pictures
/wormhole #
```

**Huomautus:** *Käsittelyyn liittyvistä Linux VMs säilö tiedostojärjestelmän kirjainkoko on merkitsevä.*

##<a name="build--prepareforbuild-task-failed-unexpectedly"></a>Muodosta: "PrepareForBuild" tehtävän epäonnistui odottamatta.

Microsoft.DotNet.Docker.CommandLine.ClientException: Virhe yhdistettäessä:

Tarkista oletusarvon docker host on käynnissä. Avaa komentorivi-ikkuna ja suorittaa:

```
docker info
```

Jos tämä funktio palauttaa virheen yrittää sitten aloittaa työpöydän **Docker For Windows** -sovelluksessa.  Jos Työpöytäsovelluksen toimii sitten **moby** -kuvaketta ilmaisinalueen pitäisi näkyä. Napsauta ilmaisinalueen kuvaketta hiiren kakkospainikkeella ja Avaa **asetukset**.  Valitse **Palauta** -välilehti ja sitten **uudelleen Docker..**.

##<a name="manually-upgrading-from-version-031-to-040"></a>Manuaalinen päivittäminen versiosta 0.31 0,40


1. Projektin varmuuskopiointi
1. Poista projektin seuraavat tiedostot:

    ```
      Dockerfile
      Dockerfile.debug
      DockerTask.ps1
      docker-compose-yml
      docker-compose.debug.yml
      .dockerignore
      Properties\Docker.props
      Properties\Docker.targets
    ```

1. Sulje ratkaisu ja poista seuraavat rivit .xproj tiedoston:

    ```
      <DockerToolsMinVersion>0.xx</DockerToolsMinVersion>
      <Import Project="Properties\Docker.props" />
      <Import Project="Properties\Docker.targets" />
    ```

1. Avaa ratkaisu
1. Poista seuraavat rivit Properties\launchSettings.json tiedoston:

    ```
      "Docker": {
        "executablePath": "%WINDIR%\\System32\\WindowsPowerShell\\v1.0\\powershell.exe",
        "commandLineArgs": "-ExecutionPolicy RemoteSigned .\\DockerTask.ps1 -Run -Environment $(Configuration) -Machine '$(DockerMachineName)'"
      }
    ```

1. Poista liittyviä Docker publishOptions project.json seuraavat tiedostot:

    ```
    "publishOptions": {
      "include": [
        ...
        "docker-compose.yml",
        "docker-compose.debug.yml",
        "Dockerfile.debug",
        "Dockerfile",
        ".dockerignore"
      ]
    },
    ```

1. Aiemman version ja asenna työkalut Docker 0,40 ja **Docker tuki-> Lisää** uudelleen pikavalikosta ASP.Net Core Web tai Console-sovelluksen. Tämä Lisää uusi tarvittavat Docker palvelutiedot takaisin projektiin. 

## <a name="an-error-dialog-occurs-when-attempting-to-add-docker-support-or-debug-f5-an-aspnet-core-application-in-a-container"></a>Virhe-valintaikkunassa ilmenee, kun yrität **Docker tuki-> Lisää** tai virheenkorjaus (F5) ASP.NET Core-sovelluksen säilöön

On joskus tuliko asennuksen ja asennuksen tunnisteet Visual Studio MEF (hallitun laajennettavuus Framework) välimuistin voit vioittunut. Kun tämä tapahtuu se voi aiheuttaa virheen eri keskustelut, kun lisäät Docker tuki ja/tai yritetään suorittaa tai virheenkorjaus (F5) ASP.NET-Core-sovelluksessa. Tilapäinen kiertää tämän ongelman Suorita seuraavat vaiheet ja MEF välimuistin uudelleen.

1. Sulje kaikki esiintymät Visual Studio
1. Avaa %USERPROFILE%\AppData\Local\Microsoft\VisualStudio\14.0\
1. Poista seuraavat kansiot
     ```
       ComponentModelCache
       Extensions
       MEFCacheBackup
    ```
1. Avaa Visual Studio
1. Yritä skenaarion uudelleen 

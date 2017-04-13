<properties
   pageTitle="Määritä Docker Host VirtualBox | Microsoft Azure"
   description="Vaiheittaiset ohjeet Docker Machine ja VirtualBox oletusesiintymää Docker määrittäminen"
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
   ms.author="mlearned" />

# <a name="configure-a-docker-host-with-virtualbox"></a>Määritä Docker Host VirtualBox

## <a name="overview"></a>Yleiskatsaus
Tässä artikkelissa opastaa määrittäminen Docker Machine ja VirtualBox oletusesiintymää Docker. Jos käytät [Docker Windows beeta](http://beta.docker.com/)-määritysten ei tarvitse.

## <a name="prerequisites"></a>Edellytykset
Seuraavia työkaluja on oltava asennettuna.

- [Docker työkalurivi](https://www.docker.com/products/overview#/docker_toolbox)

## <a name="configuring-the-docker-client-with-windows-powershell"></a>Docker asiakkaan määrittäminen Windows PowerShellin avulla

Voit määrittää Docker asiakas, yksinkertaisesti Avaa Windows PowerShell- ja toimimalla seuraavasti:

1. Luo docker host oletusesiintymää.

    ```PowerShell
    docker-machine create --driver virtualbox default
    ```
 
1. Tarkista oletusesiintymää on määritetyn ja käytössä. (Pitäisi näkyä erillisen nimeltä "oletus" käynnissä.

    ```PowerShell
    docker-machine ls 
    ```
        
    ![docker tietokoneen ls tulostus][0]
 
1. Oletusarvon määrittäminen nykyisen isännän ja määritä oman käyttöliittymän.

    ```PowerShell
    docker-machine env default | Invoke-Expression
    ```

1. Näyttää aktiivisen Docker säilöjen. Luettelossa pitäisi olla tyhjä.

    ```PowerShell
    docker ps
    ```

    ![docker ps tulostus][1]
 
> [AZURE.NOTE]Aina, kun käynnistät tietokoneen kehittäminen sinun on paikallinen docker host uudelleen.
> Voit tehdä tämän seurantakohteen komentokehotteeseen seuraava komento: `docker-machine start default`.

[0]: ./media/vs-azure-tools-docker-setup/docker-machine-ls.png
[1]: ./media/vs-azure-tools-docker-setup/docker-ps.png

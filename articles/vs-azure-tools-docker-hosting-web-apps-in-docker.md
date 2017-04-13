<properties
   pageTitle="ASP.NET Core Linux Docker säilö käyttöön Docker etäpalvelin | Microsoft Azure"
   description="Opettele käyttämään Visual Studio Tools for Docker ASP.NET Core web-sovelluksen käyttöön Docker säilön Azure Docker Host Linux AM käytössä"   
   services="azure-container-service"
   documentationCenter=".net"
   authors="mlearned"
   manager="douge"
   editor=""/>

<tags
   ms.service="azure-container-service"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="06/08/2016"
   ms.author="mlearned"/>

# <a name="deploy-an-aspnet-container-to-a-remote-docker-host"></a>Ottaa käyttöön Docker etäpalvelin ASP.NET-säilö

## <a name="overview"></a>Yleiskatsaus
Docker on kevyt säilö hakukone, vastaavan operaattoria virtual tietokoneeseen, jonka avulla voit isännöidä sovelluksia ja palveluja.
Tässä opetusohjelmassa esitellään ASP.NET Core-sovelluksen käyttöön Docker-isännästä PowerShellin Azure [Visual Studio 2015 Tools Docker](http://aka.ms/DockerToolsForVS) -tunniste avulla.

## <a name="prerequisites"></a>Edellytykset
Tarvitaan suorittamiseen tässä opetusohjelmassa seuraavasti:

- Luo Azure Docker Host (isäntä)-AM kuvatulla tavalla [docker-koneen kanssa Azure käyttäminen](./virtual-machines/virtual-machines-linux-docker-machine.md)
- Asenna [Visual Studio 2015 päivitys 3](https://go.microsoft.com/fwlink/?LinkId=691129)
- [Microsoft ASP.NET Core 1.0 SDK-paketissa](https://go.microsoft.com/fwlink/?LinkID=809122)
- Asenna [Visual Studio 2015 Työkalut Docker - esikatselu](http://aka.ms/DockerToolsForVS)

## <a name="1-create-an-aspnet-core-web-app"></a>1. ASP.NET Core web-sovelluksen luominen
Seuraavat vaiheet opastaa basic ASP.NET Core-sovelluksen, jota käytetään tässä opetusohjelmassa.

[AZURE.INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-add-docker-support"></a>2. Lisää Docker tuki

[AZURE.INCLUDE [create-aspnet5-app](../includes/vs-azure-tools-docker-add-docker-support.md)]

## <a name="3-use-the-dockertaskps1-powershell-script"></a>3. Käytä DockerTask.ps1 PowerShell-komentosarjaa 

1.  Avaa PowerShell-kehotteessa projektin pääkansioon. 

    ```
    PS C:\Src\WebApplication1>
    ```

1.  Vahvista kaukosäätimen host on käynnissä. Raportissa pitäisi näkyä tilan = suorittaminen 

    ```
    docker-machine ls
    NAME         ACTIVE   DRIVER   STATE     URL                        SWARM   DOCKER    ERRORS
    MyDockerHost -        azure    Running   tcp://xxx.xxx.xxx.xxx:2376         v1.10.3
    ```

    > [AZURE.NOTE] Jos käytät Docker beeta-isäntä ei ole mainittu tässä.

1.  Luo sovelluksen käyttäminen parametri - muodosta

    ```
    PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Build -Environment Release -Machine mydockerhost
    ```  

    > [AZURE.NOTE] Jos käytät Docker beetaversio, Ohita Machine-argumentti
    > 
    > ```
    > PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Build -Environment Release 
    > ```  


1.  Suorita sovellus, käyttämällä Suorita-parametri

    ```
    PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Run -Environment Release -Machine mydockerhost
    ```

    > [AZURE.NOTE] Jos käytät Docker beetaversio, Ohita Machine-argumentti
    > 
    > ```
    > PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Run -Environment Release 
    > ```

    Kun docker päättyy, pitäisi näkyä tulokset seuraavankaltaiselta:

    ![Sovelluksen tarkasteleminen][3]

[0]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/docker-props-in-solution-explorer.png
[1]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/change-docker-machine-name.png
[2]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/launch-application.png
[3]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/view-application.png

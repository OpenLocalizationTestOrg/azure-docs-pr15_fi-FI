<properties 
    pageTitle="Luomisesta verkkosovellukseen Linux palvelussa sovellus | Microsoft Azure" 
    description="Web app luominen työnkulun App palvelun Linux." 
    keywords="sovelluksen Azure-palvelu, online, linux oss"
    services="app-service" 
    documentationCenter="" 
    authors="naziml" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016" 
    ms.author="naziml"/>

# <a name="create-a-web-app-with-app-service-on-linux"></a>Web-sovelluksen luominen sovelluksen Linux-palvelun kanssa

## <a name="using-the-management-portal-to-create-your-web-app"></a>Hallinta-portaalin käyttäminen web-sovelluksen luominen
Voit aloittaa luominen Linux Web-sovelluksen [hallinta-portaalin](https://portal.azure.com) alla olevassa kuvassa esitetyllä tavalla.

![][1]

Kun olet valinnut seuraavista vaihtoehdoista, voit näkyy Luo-sivu alla olevassa kuvassa esitetyllä tavalla. 

![][2]

-   Anna web-sovelluksen nimi.
-   Valitse aiemmin resurssiryhmä tai Luo uusi tunnus. (Katso alueiden [rajoitukset osassa](./app-service-linux-intro.md)).
-   Valitse olemassa olevan sovelluksen-palvelusopimus tai Luo uusi jokin (Katso sovelluksen palvelun suunnitelman huomautukset [rajoitukset osassa](./app-service-linux-intro.md)). 
-   Valitse sovelluksen pino aiot käyttää. Näkyviin tulee Valitse Node.js useita versioita ja PHP välillä. 

Kun olet luonut sovelluksen, voit muuttaa sovelluksen asetukset-sovelluksen pino alla olevassa kuvassa esitetyllä tavalla.

![][3]

## <a name="deploying-your-web-app"></a>Web-sovelluksen ottaminen käyttöön

Valintaa "asennusvaihtoehdot" hallinta-portaalin antaa mahdollisuus käyttää paikallisen Git säilön tai GitHub säilön sovelluksen käyttöönotto. Ohjeet sen jälkeen ovat vastaavasti-Linux web-sovellukseen ja noudatat GitHub Microsoftin [paikallisen Git käyttöönoton](./app-service-deploy-local-git.md) tai [Jatkuva käyttöönoton](./app-service-continuous-deployment.md) tämän artikkelin ohjeita.

Voit käyttää myös FTP ladata sovelluksen sivustoon. Saat FTP-päätepisteen web App-ohjelman diagnostiikka lokit-osassa alla olevassa kuvassa esitetyllä tavalla.

![][4]


## <a name="next-steps"></a>Seuraavat vaiheet ##

* [Mikä on sovelluksen palvelun Linux?](./app-service-linux-intro.md)
* [Node.js verkkosovelluksissa Linux PM2 määritysten käyttäminen](./app-service-linux-using-nodejs-pm2.md)

<!--Image references-->
[1]: ./media/app-service-linux-how-to-create-a-web-app/top-level-create.png
[2]: ./media/app-service-linux-how-to-create-a-web-app/create-blade.png
[3]: ./media/app-service-linux-how-to-create-a-web-app/application-settings-change-stack.png
[4]: ./media/app-service-linux-how-to-create-a-web-app/diagnostic-logs-ftp.png

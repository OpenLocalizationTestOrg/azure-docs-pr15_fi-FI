<properties 
    pageTitle="PM2 määritysten käyttäminen NodeJS verkkosovelluksissa Linux | Microsoft Azure" 
    description="NodeJS verkkosovelluksissa Linux PM2 määritysten käyttäminen" 
    keywords="sovelluksen Azure-palvelu, online, nodejs, pm2, linux, oss"
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

# <a name="using-pm2-configuration-for-nodejs-in-web-apps-on-linux"></a>Node.js verkkosovelluksissa Linux PM2 määritysten käyttäminen

Jos määrität sovelluksen pino Node.js Web Apps-sovellusten Linux, näkyviin tulee määrittää Node.js käynnistys tiedoston alla olevassa kuvassa esitetyllä tavalla.

![][1]

Voit käyttää tätä joko

-   Määritä Node.js-sovelluksen käynnistyskomentosarjan (esimerkiksi: /bin/server.js)
-   Määritä käytettävä Node.js sovelluksen PM2 määritys-tiedosto (esimerkiksi: /foo/process.json)

 >[AZURE.NOTE] Jos käynnistymään automaattisesti uudelleen, kun tiettyjä tiedostoja muokataan solmu prosesseja, sinun on PM2 määritys. Muussa tapauksessa sovelluksen käynnistää ei, kun se saa muuta ilmoitukset kohteita, kuten jatkuva käyttöönoton, kun Sovelluskoodin muuttuu.

Voit tarkistaa kaikki vaihtoehdot Node.js [prosessin tiedoston dokumentaatio](http://pm2.keymetrics.io/docs/usage/application-declaration/) , mutta alapuolella on esimerkki siitä, mitä käyttää process.json-tiedostona

        {
          "name"        : "worker",
          "script"      : "/bin/server.js",
          "instances"   : 1,
          "merge_logs"  : true,
          "log_date_format" : "YYYY-MM-DD HH:mm Z",
          "watch": ["/bin/server.js", "foo.txt"],
          "watch_options": {
            "followSymlinks": true,
            "usePolling"   : true,
            "interval"    : 5
          }
        }

Huomautus Tässä määrityksessä huomioon ovat 

-   "Komentosarjan"-ominaisuus määrittää sovelluksen Käynnistä komentosarjan.
-   "Esiintymät"-ominaisuus määrittää kuinka monta esiintymää käynnistää solmu-prosessin. Jos käytät sovelluksen suurempi AM koot, joissa on useita sydämiä, haluat suurentaa resurssien määrittämällä suurempi arvo tähän.
-   "Katso" matriisin määrittää kaikki tiedostot, joiden muutos haluat käynnistää uudelleen solmu prosessit.
-   Saat "watch_options" tällä hetkellä haluat määrittää "usePolling" true, jolla sovellussisältöä on otettu käyttöön.


## <a name="next-steps"></a>Seuraavat vaiheet ##

* [Mikä on sovelluksen palvelun Linux?](./app-service-linux-intro.md)

<!--Image references-->
[1]: ./media/app-service-linux-using-nodejs-pm2/nodejs-startup-file.png
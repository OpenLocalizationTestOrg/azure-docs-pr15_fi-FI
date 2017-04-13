<properties
    pageTitle="Azure-sovelluksen palvelun Node.js web App-sovellusten käytön aloittaminen"
    description="Opettele Node.js-sovellus App Azure-palvelun web App-sovellukseen."
    services="app-service\web"
    documentationCenter="nodejs"
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="get-started-article"
    ms.date="07/01/2016"
    ms.author="cephalin"/>

# <a name="get-started-with-nodejs-web-apps-in-azure-app-service"></a>Azure-sovelluksen palvelun Node.js web App-sovellusten käytön aloittaminen

[AZURE.INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Tässä opetusohjelmassa näytetään, miten voit luoda yksinkertaisen [Node.js] -sovelluksen ja ota se käyttöön [Azure App palvelun] komentorivin ympäristöstä, kuten cmd.exe tai bash. Tässä opetusohjelmassa ohjeita on noudatettava sellaisille käyttöjärjestelmille, joita voit suorittaa Node.js.

>[AZURE.INCLUDE [app-service-linux](../../includes/app-service-linux.md)] 

<a name="prereq"></a>
## <a name="prerequisites"></a>Edellytykset

- [Node.js]
- [Bower]
- [Yeoman]
- [Git]
- [Azure CLI]
- Microsoft Azure-tili. Jos sinulla ei ole tiliä, voit [Rekisteröidy maksuttoman kokeiluversion] tai [aktivoida Visual Studio tilaajan etuja].

## <a name="create-and-deploy-a-simple-nodejs-web-app"></a>Luo ja ota käyttöön yksinkertainen Node.js web App-sovelluksessa

1. Avaa valittua komentorivin terminaali ja asenna [Express Yeoman luontitoiminto].

        npm install -g generator-express

2. `CD`työn kansioon ja luo express sovelluksen käyttämällä seuraavaa syntaksia:

        yo express
        
    Valitse pyydettäessä seuraavista vaihtoehdoista:  

    `? Would you like to create a new directory for your project?`**Kyllä**  
    `? Enter directory name`**{appname}**  
    `? Select a version to install:`**MVC**  
    `? Select a view engine to use:`**Jade**  
    `? Select a css preprocessor to use (Sass Requires Ruby):`**Ei mitään**  
    `? Select a database to use:`**Ei mitään**  
    `? Select a build tool to use:`**Grunt**

3. `CD`uusi sovellus pääkansioon ja käynnistä se varmistaaksesi, että se toimii oman kehitysympäristö:

        npm start

    Siirry selaimella <http://localhost:3000> varmistaaksesi, että näet Express-aloitussivu. Kun olet tarkistanut sovellus toimii oikein, `Ctrl-C` niin, ettei se.
    
1. Vaihda ASM tilaan ja kirjaudu sisään ja Azure (tarvitset [Azure CLI](#prereq)):

        azure config mode asm
        azure login

    Noudata kehotteen Jatka kirjautuminen Microsoft-tilillä, jossa on Azure tilauksen selaimessa.

2. Varmista, että olet edelleen sovelluksen pääkansiossa ja valitse sovelluksen palvelun sovelluksen resurssin luominen Azure yksilöllisen sovelluksen nimellä seuraava komento. Esimerkki: http://{appname}.azurewebsites.net

        azure site create --git {appname}

    Noudata kehote Azure alue, jos haluat ottaa käyttöön. Jos olet ole määrittänyt Git/FTP käyttöönoton tunnistetiedot Azure tilauksen, sinua pyydetään myös luoda niitä.

3. Avaa./config/config.js tiedoston sovelluksen ylimmällä ja muuta tuotannon portin `process.env.port`; oman `production` -ominaisuutta `config` objektin pitäisi näyttää seuraavalta:

        production: {
            root: rootPath,
            app: {
                name: 'express1'
            },
            port: process.env.port,
        }

    Näillä oikeuksilla Node.js sovelluksen vastaa WWW-pyyntöjä oletusportti kyseisen iisnode seuraa.
    
4. Avaa./package.json ja lisää `engines` ominaisuuden avulla voit [määrittää haluamasi Node.js-versio](#version).

        "engines": {
            "node": "6.6.0"
        }, 

4. Tallenna muutokset ja valitse sovelluksen käyttöön Azure git avulla:

        git add .
        git commit -m "{your commit message}"
        git push azure master

    Express-luontitoiminnon on jo .gitignore tiedoston, joten oman `git push` ei tarjoaman kaistanleveyden yritetään ladata node_modules / hakemistoon.

5. Käynnistä lopuksi live Azure sovelluksen selaimessa:

        azure site browse

    Pitäisi tulla näkyviin käynnissä live Azure-sovelluksen palvelun Node.js koodiin.
    
    ![Esimerkki sovelluksen selaamalla.][deployed-express-app]

## <a name="update-your-nodejs-web-app"></a>Node.js web-sovelluksen päivittäminen

Tehtävä päivitykset sovelluksen Node.js web App-palvelu käytössä, suorita `git add`, `git commit`, ja `git push` kuin teit kun käyttöön web-sovelluksen ensimmäisen kerran.
     
## <a name="how-app-service-deploys-your-nodejs-app"></a>Miten sovelluksen palvelun ottaa käyttöön sovelluksen Node.js

Azure App palvelu käyttää [iisnode] suorittaa Node.js sovellukset. Azure-CLI ja Kudu moduuli (Git käyttöönotosta) avulla voit virtaviivaistettu ongelmia voi kuitenkin ilmetä, kun kehittää ja ottaa käyttöön Node.js sovellukset komentoriviltä toimivat yhdessä. 

- `azure site create --git`tunnistaa yleisiä Node.js kuviota, server.js tai app.js ja luo iisnode.yml pääkansio. Voit käyttää tätä tiedostoa iisnode mukauttamiseen.
- Milloin `git push azure master`, Kudu automatisoi käyttöönoton seuraavat toimet:

    - Jos package.json on säilöön pääkansio, suorita `npm install --production`.
    - Luo Web.config iisnode, joka osoittaa Käynnistä komentosarja-package.json (kuten server.js tai app.js) varten.
    - Voit mukauttaa Web.config valmis jaettu virheenkorjaus solmu tarkastaminen kanssa.
    
## <a name="use-a-nodejs-framework"></a>Käytä Node.js kehys

Jos käytät usein käytetyt Node.js kehys, kuten [Sails.js] [ SAILSJS] tai [MEAN.js] [ MEANJS] kehittää sovellukset, voit asentaa ne App-palveluun. Suositut Node.js puitteissa on niiden tietyn elementtimäärityksen ja niiden paketin riippuvuuksia säilyttää käytön päivittää. Kuitenkin sovelluksen-palvelun avulla stdout ja stderr lokitiedostot on käytettävissä, jotta voit täsmälleen mitä teet sovelluksen kanssa ja tehdä muutoksia vastaavasti. Lisätietoja on kohdassa [Hae stdout ja stderr lokit-iisnode](#iisnodelog).

Opetusohjelmassa kerrotaan, kuinka tietyn framework sovelluksen-palvelun käyttöä varten:

- [Sails.js web-sovelluksen käyttöönotto Azure sovelluksen-palveluun]
- [Node.js keskustelu-sovelluksen luominen Socket.IO Azure sovelluksen-palvelun kanssa]
- [Voit käyttää io.js Azure palvelun Web sovellukset]

<a name="version"></a>
## <a name="use-a-specific-nodejs-engine"></a>Käytä tiettyä Node.js-ohjelma

Tyypillinen työnkulun näet sovelluksen palvelun käyttämään tietyn Node.js moduulin package.json tavalliseen tapaan.
Esimerkki:

    "engines": {
        "node": "6.6.0"
    }, 

Kudu käyttöönotto-ohjelma määrittää, mitkä Node.js ohjelma käyttää seuraavassa järjestyksessä:

- Tarkista ensin, ovatko iisnode.yml `nodeProcessCommandLine` on määritetty. Kyllä, valitse käyttää.
- Tarkista, ovatko package.json `"node": "..."` on määritetty `engines` objekti. Kyllä, valitse käyttää.
- Valitse Node.js oletusversio oletusarvoisesti.

>[AZURE.NOTE] On suositeltavaa, että määrität erikseen Node.js-ohjelma, jonka haluat. Node.js oletusversio voit muuttaa, ja näkyviin voi tulla virheitä Azure-sovellukseen, koska Node.js oletusversio ei sovellu sovelluksen.

<a name="iisnodelog"></a>
## <a name="get-stdout-and-stderr-logs-from-iisnode"></a>Pyydä iisnode stdout ja stderr lokit

Voit lukea iisnode lokit, toimimalla seuraavasti.

> [AZURE.NOTE] Näiden vaiheiden suorittamisen jälkeen lokitiedostojen ei ehkä ole, kunnes näyttöön tulee virhesanoma.

1. Avaa iisnode.yml-tiedosto, jonka Azure-CLI avulla.

2. Määritä seuraavat kaksi parametria: 

        loggingEnabled: true
        logDirectory: iisnode
    
    Yhdessä ne kertovat iisnode sovelluksen-palvelussa, voit sijoittaa stdout ja stderror tulos D:\home\site\wwwroot\**iisnode** hakemisto.

3. Tallenna muutokset ja valitse push muutokset Azure Git-komentojen kanssa:

        git add .
        git commit -m "{your commit message}"
        git push azure master
   
    iisnode on nyt määritetty. Seuraavat vaiheet näyttää, miten voit käyttää lokitiedostot.
     
4. Käyttää, kun sovellus, joka on osoitteessa Kudu virheenkorjaus-konsolin selaimessa:

        https://{appname}.scm.azurewebsites.net/DebugConsole 

    Tätä URL-Osoitetta eroaa web Appin URL-osoite lisäämällä "*.scm.*" DNS-nimeä. Jos URL-osoite, lisäksi, näkyviin tulee 404-virhe.

5. Siirry D:\home\site\wwwroot\iisnode

    ![Siirtyminen iisnode lokitiedostojen sijainti.][iislog-kudu-console-find]

6. Valitse **Muokkaa** -kuvaketta, jos haluat lukea sisäänkirjautumista varten. Voit myös halutessasi valita **lataaminen** tai **poistaminen** .

    ![Iisnode log-tiedoston avaaminen.][iislog-kudu-console-open]

    Nyt näet avulla sovelluksen palvelun käyttöönoton loki.
    
    ![Tutkittaessa iisnode-lokitiedoston.][iislog-kudu-console-read]

## <a name="debug-your-app-with-node-inspector"></a>Sovelluksen solmu tarkistaminen ja korjaaminen

Jos käytät solmu tarkastaminen virheenkorjaus Node.js sovelluksia, voit käyttää, kun live App Service-sovellus. Solmun tarkastaminen on asennettu valmiiksi App palvelun iisnode-asennuksen. Ja jos otat käyttöön Git kautta, luo automaattisesti Web.config-Kudu sisältää jo kaikki määritykset, sinun on otettava käyttöön solmu tarkastaminen.

Solmun tarkastaminen käyttöön seuraavasti:

1. Avaa iisnode.yml osoitteessa säilöön pääkansio ja määritä seuraavat parametrit: 

        debuggingEnabled: true
        debuggerExtensionDll: iisnode-inspector.dll

3. Tallenna muutokset ja valitse push muutokset Azure Git-komentojen kanssa:

        git add .
        git commit -m "{your commit message}"
        git push azure master
   
4. Nyt vain Siirry sinua sovelluksen Käynnistä tiedoston määritetyn mukaisesti oman package.json alku-komentosarjan URL-osoite lisätään Debug kanssa. Esimerkiksi

        http://{appname}.azurewebsites.net/server.js/debug
    
    Vaihtoehtoisesti
    
        http://{appname}.azurewebsites.net/app.js/debug

## <a name="more-resources"></a>Lisää resursseja

- [Node.js-versio, joka määrittää Azure-sovelluksessa](../nodejs-specify-node-version-azure-apps.md)
- [Parhaiden käytäntöjen ja Azure Node.js sovellusten vianmäärityksen opas](app-service-web-nodejs-best-practices-and-troubleshoot-guide.md)
- [Voit korjata Node.js verkkosovellukseen Azure sovelluksen-palvelussa](web-sites-nodejs-debug.md)
- [Käyttämällä Node.js moduulit Azure-sovellusten kanssa](../nodejs-use-node-modules-azure-apps.md)
- [Azure App palvelun verkkosovelluksissa: Node.js](http://blogs.msdn.com/b/silverlining/archive/2012/06/14/windows-azure-websites-node-js.aspx)
- [Node.js Developer Center](/develop/nodejs/)
- [Azure App palvelun web App-sovellusten käytön aloittaminen](app-service-web-get-started.md)
- [Huipputehokas salainen Kudu virheenkorjaus-konsolin tutustuminen]

<!-- URL List -->

[Azure CLI]: ../xplat-cli-install.md
[Azure sovelluksen-palvelu]: ../app-service/app-service-value-prop-what-is.md
[Aktivoi Visual Studio tilaajan etuja]: http://go.microsoft.com/fwlink/?LinkId=623901
[Bower]: http://bower.io/
[Node.js keskustelu-sovelluksen luominen Socket.IO Azure sovelluksen-palvelun kanssa]: ./web-sites-nodejs-chat-app-socketio.md
[Sails.js web-sovelluksen käyttöönotto Azure sovelluksen-palveluun]: ./app-service-web-nodejs-sails.md
[Huipputehokas salainen Kudu virheenkorjaus-konsolin tutustuminen]: /documentation/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[Express Yeoman luontitoiminto]: https://github.com/petecoop/generator-express
[Git]: http://www.git-scm.com/downloads
[Voit käyttää io.js Azure palvelun Web sovellukset]: ./web-sites-nodejs-iojs.md
[iisnode]: https://github.com/tjanczuk/iisnode/wiki
[MEANJS]: http://meanjs.org/
[Node.js]: http://nodejs.org
[SAILSJS]: http://sailsjs.org/
[maksuttoman kokeiluversion käyttäjäksi rekisteröityminen]: http://go.microsoft.com/fwlink/?LinkId=623901
[web app]: ./app-service-web-overview.md
[Yeoman]: http://yeoman.io/

<!-- IMG List -->

[deployed-express-app]: ./media/app-service-web-nodejs-get-started/deployed-express-app.png
[iislog-kudu-console-find]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-navigate.png
[iislog-kudu-console-open]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-open.png
[iislog-kudu-console-read]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-read.png

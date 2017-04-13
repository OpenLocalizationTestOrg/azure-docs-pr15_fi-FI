<properties
    pageTitle="Sails.js web-sovelluksen käyttöönotto Azure sovelluksen-palveluun"
    description="Opettele Node.js-sovellus App Azure-palvelu. Tässä opetusohjelmassa kerrotaan, miten Sails.js web-sovelluksen käyttöönotto."
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
    ms.topic="article"
    ms.date="09/23/2016"
    ms.author="cephalin"/>

# <a name="deploy-a-sailsjs-web-app-to-azure-app-service"></a>Sails.js web-sovelluksen käyttöönotto Azure sovelluksen-palveluun

Tässä opetusohjelmassa kerrotaan, miten Sails.js sovelluksen käyttöönotto Azure-sovelluksen palveluun. Prosessi voit glean joitakin yleisiä knowledge määrittäminen toimimaan App palvelun Node.js sovelluksen. 

Sinulla on oltava kokemusta Sails.js. Tässä opetusohjelmassa ei ole tarkoitus auttaa liittyviä käynnissä Sail.js yleinen.


## <a name="prerequisites"></a>Edellytykset

- [Node.js](https://nodejs.org/)
- [Sails.js](http://sailsjs.org/get-started)
- [Git](http://www.git-scm.com/downloads)
- [Azure CLI](../xplat-cli-install.md)
- Microsoft Azure-tili. Jos sinulla ei ole tiliä, voit [Rekisteröidy maksuttoman kokeiluversion](/pricing/free-trial/?WT.mc_id=A261C142F) tai [aktivoida Visual Studio tilaajan etuja](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

>[AZURE.NOTE] Siirry Nähdäksesi Azure App palvelun toiminto ennen Azure-tili rekisteröityminen [Yritä App kääntäjä](http://go.microsoft.com/fwlink/?LinkId=523751). Voit luoda lyhytkestoinen starter-sovelluksen heti sovelluksen käytössä, ei ole luottokortti, ei ole sitoumukset.

## <a name="step-1-create-a-sailsjs-app-locally"></a>Vaihe 1: Paikallisesti Sails.js sovelluksen luominen

Ensin nopeasti luoda oletusarvon Sails.js sovelluksen kehittämistä ympäristön seuraavasti:

1. Avaa valittua komentorivin terminaali ja `CD` toimimasta hakemistoon.

2. Sails.js sovelluksen luominen ja suorittaa sen:

        sails new <appname>
        cd <appname>
        sails lift

    Varmista, että voit siirtyä oletuskotisivu osoitteessa http://localhost:1377.

## <a name="step-2-create-the-azure-app-resource"></a>Vaihe 2: Luo Azure sovelluksen resurssi

Seuraavaksi luodaan sovelluksen palvelun resurssin Azure-tietokannassa. Aiot Sails.js-sovelluksen käyttöönotto siihen myöhemmin.

1. Kirjaudu sisään Azure kaltaisten näin:
1. Muuta ASM tilaan ja Azure kirjautuminen saman pääte:

        azure config mode asm
        azure login

    Noudata kehotteen Jatka kirjautuminen Microsoft-tilillä, jossa on Azure tilauksen selaimessa.

2. Varmista, että olet Sails.js projektin pääkansiossa. Luo sovelluksen palvelun sovelluksen resurssin Azure yksilöllisen sovelluksen nimellä seuraava komento. Web-sovelluksen URL-osoite on http://&lt;appname >. azurewebsites.net.

        azure site create --git <appname>

    Noudata kehote Azure alue, jos haluat ottaa käyttöön. Jos olet ole määrittänyt Git/FTP käyttöönoton tunnistetiedot Azure tilauksen, sinua pyydetään myös luoda niitä.

    Kun sovellus palvelun sovelluksen resurssi on luotu:

    - Sails.js-sovellus on Git alustaminen
    - Paikallisen Git alustaa säilöön on yhteydessä uuden sovelluksen Service-sovelluksen Git, joka on remote-niminen palveluprioriteetti "azure", ja
    - Ja iisnode.yml tiedosto luodaan pääkansion. Voit käyttää tätä tiedostoa [iisnode](https://github.com/tjanczuk/iisnode), joka suoritetaan Node.js sovellukset App palvelun käyttämällä määrittämiseen.

## <a name="step-3-configure-and-deploy-your-sailsjs-app"></a>Vaihe 3: Määritä ja Sails.js-sovelluksen käyttöönotto

 Sails.js-sovellus App-palvelun käyttäminen kuuluu kolme päävaihetta:

 - Määritä sen sovelluksen Suorita sovellus-palvelussa
 - Ottaa käyttöön sovelluksen-palveluun
 - Lue stderr ja stdout lokit käyttöönoton mahdollisten ongelmien vianmääritys

Noudattamalla seuraavia ohjeita:

1. Avaa uuden iisnode.yml tiedoston pääkansion ja lisää seuraavat kaksi riviä:

        loggingEnabled: true
        logDirectory: iisnode

    Lokiin kirjaaminen on otettu käyttöön iisnode. Saat lisätietoja siitä, miten tämä toimii  [Hae stdout ja stderr lokit-iisnode](app-service-web-nodejs-get-started.md#iisnodelog).

2. Avaa config/env/production.js tuotannon ympäristön ja määritä `port` ja `hookTimeout`:

        module.exports = {

            // Use process.env.port to handle web requests to the default HTTP port
            port: process.env.port,
            // Increase hooks timout to 30 seconds
            // This avoids the Sails.js error documented at https://github.com/balderdashy/sails/issues/2691
            hookTimeout: 30000,

            ...
        };

    Löydät nämä asetukset ohjeissa  [Sails.js ohjeissa](http://sailsjs.org/documentation/reference/configuration/sails-config).

    Seuraavaksi sinun täytyy Varmista, että [Grunt](https://www.npmjs.com/package/grunt) on yhteensopiva Azure's verkkoasemat. Grunt versiot, joka on pienempi kuin 1.0.0 käyttää vanhentuneiden [glob](https://www.npmjs.com/package/glob) -paketti (pienempi kuin 5.0.14), joka ei tue verkkoasemat. 

3. Avaa package.json ja muuta `grunt` version `1.0.0` ja poistaa kaikki `grunt-*` paketit. Oman `dependencies` ominaisuuden pitäisi näyttää tältä:

        "dependencies": {
            "ejs": "<leave-as-is>",
            "grunt": "1.0.0",
            "include-all": "<leave-as-is>",
            "rc": "<leave-as-is>",
            "sails": "<leave-as-is>",
            "sails-disk": "<leave-as-is>",
            "sails-sqlserver": "<leave-as-is>"
        },

3. Valitse package.json, Lisää seuraava `engines` ominaisuuden arvoksi yksi, jotka haluamme Node.js-versio.

        "engines": {
            "node": "6.6.0"
        },

6. Tallenna muutokset ja esikatsella muutoksiasi varmistaaksesi, että sovelluksesi toimii edelleen paikallisesti. Voit tehdä tämän poistaminen `node_modules` kansioon ja suorita:

        npm install
        sails lift

4. Nyt-sovelluksen käyttöön Azure git avulla:

        git add .
        git commit -m "<your commit message>"
        git push azure master

5. Lopuksi vain käynnistää live Azure sovelluksen selaimessa:

        azure site browse

    Saman Sails.js kotisivun pitäisi tulla näkyviin.
    
    ![](./media/app-service-web-nodejs-sails/sails-in-azure.png)

## <a name="troubleshoot-your-deployment"></a>Käyttöönoton vianmääritys

Jos Sails.js sovelluksen epäonnistuu jostain syystä sovelluksen-palvelussa, Etsi stderr lokit ongelmien sitä.
Lisätietoja on kohdassa [Hae stdout ja stderr lokit-iisnode](app-service-web-nodejs-sails.md#iisnodelog).
Jos se on aloitettu onnistuneesti, stdout lokiin olisi Näytä tuttuja viestin:

                .-..-.

    Sails              <|    .-..-.
    v0.12.4             |\
                        /|.\
                        / || \
                    ,'  |'  \
                    .-'.-==|/_--'
                    `--'-------' 
    __---___--___---___--___---___--___
    ____---___--___---___--___---___--___-__

    Server lifted in `D:\home\site\wwwroot`
    To see your app, visit http://localhost:\\.\pipe\c775303c-0ebc-4854-8ddd-2e280aabccac
    To shut down Sails, press <CTRL> + C at any time.

Voit hallita [config/log.js](http://sailsjs.org/#!/documentation/concepts/Logging) tiedoston stdout lokit rakeisuuden. 

## <a name="connect-to-a-database-in-azure"></a>Yhteyden muodostaminen Azure-tietokantaan

Muodostaa Azure-tietokantaan valittua tietokannan luominen Azure, kuten Azure SQL-tietokanta, MySQL, MongoDB, Azure (Redis.txt) välimuistin, jne., ja vastaavan [datastore sovittimen](https://github.com/balderdashy/sails#compatibility) avulla voit muodostaa yhteyden. Tämän osion ohjeita noudattamalla voit yhteyden muodostaminen MySQL-tietokantaan Azure-tietokannassa.

1. Noudata ohjelman [tähän](../store-php-create-mysql-database.md) luominen Azure MySQL-tietokantaan.

2. Komentorivin päätteen-MySQL-sovittimen asentaminen:

        npm install sails-mysql --save

3. Avaa config/connections.js ja connection-objektin lisääminen luetteloon: 

        mySql: {
            adapter: 'sails-mysql',
            user: process.env.dbuser,
            password: process.env.dbpassword,
            host: process.env.dbhost, 
            database: process.env.dbname,
            options: {
                encrypt: true
            }
        },

4. Ympäristön kunkin muuttujan (`process.env.*`), sinun on määritettävä sovelluksen-palvelussa. Toiminto, suorita seuraavat komennot päätteen. Sinun on kaikki yhteystiedot on Azure-portaalissa (katso [yhteyden muodostaminen MySQL-tietokannasta](../store-php-create-mysql-database.md#connect)).

        azure site appsetting add dbuser="<database user>"
        azure site appsetting add dbpassword="<database password>"
        azure site appsetting add dbhost="<database hostname>"
        azure site appsetting add dbname="<database name>"
        
    Laajennettujen asetusten Azure sovelluksen asetusten pitää luottamuksellisia tietoja ulos lähde-ohjausobjektin (Git). Seuraavaksi määrität oman kehitysympäristö, jos haluat käyttää samaa yhteystietoja.

4. Avaa config/local.js ja lisää seuraavan objektin yhteydet:

        connections: {
            mySql: {
                user: "<database user>",
                password: "<database password>",
                host: "<database hostname>", 
                database: "<database name>",
            },
        },
    
    Tässä määrityksessä ohittaa config/connections.js-tiedoston paikallisen ympäristön asetuksia. Tätä tiedostoa ei sisälly projektin oletusarvo-.gitignore mukaan niin, että se ei tallenneta Git. Nyt olet pysty muodostamaan yhteyttä MySQL-tietokantaan, sekä Azure-verkkosovelluksen ja paikallisen kehitysympäristö.

4. Avaa config/env/production.js tuotannon ympäristön määrittäminen ja lisää seuraavat `models` objekti:

        models: {
            connection: 'mySql',
            migrate: 'safe'
        },

4. Avaa config/env/development.js määrittäminen oman kehitysympäristö ja lisää seuraavat `models` objekti:

        models: {
            connection: 'mySql',
            migrate: 'alter'
        },

    `migrate: 'alter'`Voit luoda ja päivittää oman MySQL-Tietokantataulukoissa helposti tietokannan siirron ominaisuuksien avulla. Kuitenkin `migrate: 'safe'` käytetään Azure (tuotannon)-ympäristössä, Sails.js ei ole sallittua käyttää `migrate: 'alter'` tuotantoympäristössä (  [Sails.js](http://sailsjs.org/documentation/concepts/models-and-orm/model-settings)ohjeissa).

4. -Päätteen, [Luo](http://sailsjs.org/documentation/reference/command-line-interface/sails-generate) Sails.js [Sinikopio API](http://sailsjs.org/documentation/concepts/blueprints) tapaan tavallisesti, suoritetaan `sails lift` Sails.js tietokannan siirtoon tietokannan luomiseen. Esimerkki:

         sails generate api mywidget
         sails lift

    `mywidget` Mallin luoma Tämä komento on tyhjä, mutta Microsoft voi käyttää osoittamaan, että on tietokannan yhteys.
    Kun suoritat `sails lift`, luo puuttuvat taulukot mallien sovellus käyttää.

6. Käyttää Sinikopio API luomaasi selaimessa. Esimerkki:

        http://localhost:1337/mywidget/create
    
    Ohjelmointirajapinnan tulee palauttaa luotu tapahtuma selainikkunassa, mikä tarkoittaa, että tietokanta on luotu takaisin.

        {"id":1,"createdAt":"2016-09-23T13:32:00.000Z","updatedAt":"2016-09-23T13:32:00.000Z"}

5. Nyt siirtää muutokset Azure ja Etsi sovellus varmistaaksesi, että se toimii edelleen.

        git add .
        git commit -m "<your commit message>"
        git push azure master
        azure site browse

6. Käyttää Sinikopio API Azure koodiin. Esimerkki:

        http://<appname>.azurewebsites.net/mywidget/create

    Jos Ohjelmointirajapinnan palauttaa toisen uuden tapahtuman, Azure koodiin puhuu MySQL-tietokantaan.

## <a name="more-resources"></a>Lisää resursseja

- [Azure-sovelluksen palvelun Node.js web App-sovellusten käytön aloittaminen](app-service-web-nodejs-get-started.md)
- [Käyttämällä Node.js moduulit Azure-sovellusten kanssa](../nodejs-use-node-modules-azure-apps.md)

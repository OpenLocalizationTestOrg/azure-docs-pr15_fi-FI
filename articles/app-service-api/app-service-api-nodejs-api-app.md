<properties
    pageTitle="Azure App palvelun node.js API-sovellus | Microsoft Azure"
    description="Lue, miten voit luoda Node.js RESTful-Ohjelmointirajapinta ja Azure App Service API-sovelluksen käyttöön."
    services="app-service\api"
    documentationCenter="node"
    authors="bradygaster"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-api"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="node"
    ms.topic="get-started-article"
    ms.date="05/26/2016"
    ms.author="rachelap"/>

# <a name="build-a-nodejs-restful-api-and-deploy-it-to-an-api-app-in-azure"></a>Node.js RESTful-Ohjelmointirajapinta muodosta ja ota se käyttöön Azure API-sovelluksen

[AZURE.INCLUDE [app-service-api-get-started-selector](../../includes/app-service-api-get-started-selector.md)]

Tässä opetusohjelmassa näytetään luomisesta Yksinkertainen [Node.js](http://nodejs.org) API ja ota se käyttöön [Azure App](../app-service/app-service-value-prop-what-is.md) Service [API-sovelluksen](app-service-api-apps-why-best-platform.md) avulla [Git](http://git-scm.com). Voit käyttää mitä tahansa käyttöjärjestelmille, joita voit suorittaa Node.js ja yhteys työn kaikki käyttämällä komentorivityökaluja, kuten cmd.exe tai bash.

## <a name="prerequisites"></a>Edellytykset

1. ([Avaa tähän ilmaisen tilin](https://azure.microsoft.com/pricing/free-trial/)) Microsoft Azure-tiliin
1. [Node.js](http://nodejs.org) asennettu (Tässä esimerkissä oletetaan, että sinulla on Node.js 4.2.2 versio)
2. [Git](https://git-scm.com/) asennettuna
1. [GitHub](https://github.com/) tili

Kun sovellus palvelun tukee useita eri tapoja koodisi käyttöön API-sovelluksen, tässä opetusohjelmassa näkyy Git-menetelmä ja oletetaan, että sinulla on perustiedot tuntemus Git käsittelemisestä. Tietoja muiden käyttöönoton kautta on artikkelissa [Azure-sovelluksen palveluun sovelluksen käyttöönotto](../app-service-web/web-sites-deploy.md).

## <a name="get-the-sample-code"></a>Esimerkki koodin hankkiminen

1. Avaa rivin komentoliittymä, voit suorittaa Node.js ja Git komennot.

1. Siirry kansioon, joiden avulla paikallinen Git säilöön ja Kloonaa [GitHub säilöön sisältävä sample code](https://github.com/Azure-Samples/app-service-api-node-contact-list).

        git clone https://github.com/Azure-Samples/app-service-api-node-contact-list.git

    Esimerkki Ohjelmointirajapinnan on päätepisteet: Get-pyyntö `/contacts` palauttaa luettelon nimet ja sähköpostiosoitteet JSON-muodossa, kun `/contacts/{id}` palauttaa vain valitun yhteyshenkilön.

## <a name="scaffold-auto-generate-nodejs-code-based-on-swagger-metadata"></a>Scaffold (luo automaattisesti) Node.js koodin perusteella Swagger metatiedot

[Swagger](http://swagger.io/) on tiedostomuoto, joka kuvaa RESTful-Ohjelmointirajapinta metatietojen. Azure sovelluksen-palvelulla [tukee Swagger metatiedot](app-service-api-metadata.md). Tässä osassa opetusohjelman mallit API-kehitys työnkulun, jossa voit luoda Swagger metatietojen ensin ja voit scaffold (luo automaattisesti) Ohjelmointirajapinnan palvelimen koodi. 

>[AZURE.NOTE] Voit ohittaa tämän osan, jos et halua lisätietoja scaffold Node.js koodin Swagger metatietojen tiedostosta. Jos haluat ottaa käyttöön vain otoksen koodin uusi API-sovellukseen, siirry suoraan kohtaan [API-sovelluksen Azure](#createapiapp) .

### <a name="install-and-execute-swaggerize"></a>Asentaminen ja suorittaminen Swaggerize

1. Suorita seuraavat komennot asentaminen **yo** ja **luontitoiminto swaggerize** NPM moduulit yleisesti.

        npm install -g yo
        npm install -g generator-swaggerize

    Swaggerize on työkalua, joka luo server-koodin API kuvaaman Swagger metatieto-tiedosto. Aiot käyttää Swagger-tiedosto nimeltä *api.json* ja voit monistaa tietovaraston *Käynnistä* -kansiossa.

2. Siirry kansioon, *Käynnistä* ja suorita `yo swaggerize` komento. Swaggerize Kysy kysymyksiä.  Siitä, **mitä Soita projektin**Kirjoita "ContactList" **swagger tiedoston polku**, kirjoita "api.json" ja **Express-hyvää, tai Restify**, kirjoita "express".

        yo swaggerize

    ![Komentorivin swaggerize](media/app-service-api-nodejs-api-app/swaggerize-command-line.png)
    
    **Huomautus**: Jos saat virheilmoituksen tässä vaiheessa, seuraavaksi kerrotaan, miten voit korjata sitä.

    Swaggerize Luo sovelluksen-kansiossa, scaffolds tiedostokäsittelijöitä ja tiedostojen ja luo **package.json** tiedoston. Näytä pika-ohjelma sillä muodostetaan Swagger Ohje-sivu.  

3. Jos `swaggerize` komento epäonnistuu "Odottamaton tunnus" tai "Virheellinen ohjaussekvenssi"-virhe, korjata virheen syyn luotu *package.json* tiedostoa muokkaavat käyttäjät. Valitse `regenerate` viiva-kohdassa `scripts`, takaisin vinoviiva, joka edeltää *api.json* vinoviivalla, voit muuttaa niin, että rivi näyttää seuraavalta:

        "regenerate": "yo swaggerize --only=handlers,models,tests --framework express --apiPath config/api.json"

1. Siirry kansioon, joka sisältää (Tässä tapauksessa */start/ContactList* alikansion) scaffolded koodia.

1. Suorita `npm install`.
    
        npm install
        
2. **Jsonpath** NPM-moduulin asentaminen. 

        npm install --save jsonpath
        
    ![Jsonpath asentaminen](media/app-service-api-nodejs-api-app/jsonpath-install.png)

1. **Swaggerize käyttöliittymän** NPM-moduulin asentaminen. 

        npm install --save swaggerize-ui
        
    ![Swaggerize käyttöliittymän asentaminen](media/app-service-api-nodejs-api-app/swaggerize-ui-install.png)

### <a name="customize-the-scaffolded-code"></a>Mukauta scaffolded koodi

1. Kopioi **lib** kansio **Käynnistä** -kansiosta scaffolder luoma **ContactList** kansioon. 

1. Korvaa seuraava koodi koodin **handlers/contacts.js** -tiedostossa. 

    Tämä koodi käyttää JSON-tiedot tallennetaan **lib/contacts.json** tiedosto, jonka mukaan **lib/contactRepository.js**served. Uusi contacts.js koodi vastaa pyyntöjen hakee kaikki yhteystiedot ja palauttaa ne JSON-paketti. 

        'use strict';
        
        var repository = require('../lib/contactRepository');
        
        module.exports = {
            get: function contacts_get(req, res) {
                res.json(repository.all())
            }
        };

1. Korvaa koodin **handlers/contacts/{id}.js** tiedoston fofllowing-koodi. 

        'use strict';

        var repository = require('../../lib/contactRepository');
        
        module.exports = {
            get: function contacts_get(req, res) {
                res.json(repository.get(req.params['id']));
            }    
        };

1. Korvaa seuraava koodi **server.js** koodia. 

    Server.js-tiedostoon tekemäsi muutokset ovat korostettuina käyttämällä kommentteja, jotta näet tehdään muutoksia. 

        'use strict';

        var port = process.env.PORT || 8000; // first change

        var http = require('http');
        var express = require('express');
        var bodyParser = require('body-parser');
        var swaggerize = require('swaggerize-express');
        var swaggerUi = require('swaggerize-ui'); // second change
        var path = require('path');

        var app = express();

        var server = http.createServer(app);

        app.use(bodyParser.json());

        app.use(swaggerize({
            api: path.resolve('./config/api.json'), // third change
            handlers: path.resolve('./handlers'),
            docspath: '/swagger' // fourth change
        }));

        // change four
        app.use('/docs', swaggerUi({
          docs: '/swagger'  
        }));

        server.listen(port, function () { // fifth and final change
        });

### <a name="test-with-the-api-running-locally"></a>Testaa käytössä paikallisesti Ohjelmointirajapinta

1. Aktivoi Node.js komentorivin suoritettavan tiedoston palvelimeen. 

        node server.js

1. Selattaessa ****http://localhost:8000/yhteyshenkilöille näet yhteystietoluettelo JSON tulos (tai sinua pyydetään lataamaan, selaimen mukaan). 

    ![Kaikki yhteystiedot Api-kutsu](media/app-service-api-nodejs-api-app/all-contacts-api-call.png)

1. Kun selaat **http://localhost:8000/yhteystiedot/2**, näet kyseistä tunnus-arvoa vastaavan yhteystiedon.

    ![Tietyn yhteyshenkilön Api-kutsu](media/app-service-api-nodejs-api-app/specific-contact-api-call.png)

1. Swagger JSON tiedot on **served/swagger** päätepisteen kautta:

    ![Yhteystietojen Swagger Json](media/app-service-api-nodejs-api-app/contacts-swagger-json.png)

1. Swagger-Käyttöliittymän on served **/docs** päätepisteen kautta. Swagger-käyttöliittymän oman API testata rich client HTML-ominaisuuksien avulla.

    ![Swagger käyttöliittymä](media/app-service-api-nodejs-api-app/swagger-ui.png)

## <a id="createapiapp"></a>Uuden API-sovelluksen luominen

Tässä osassa Käytä Azure portaalin API uuden sovelluksen luominen Azure-tietokannassa. API sovelluksen kuvaa, jonka Azure voivat suorittaa koodin suorittaminen resurssit. Myöhemmin osien koodisi käyttöön uusi API-sovellus.

1. Siirry [Azure Portal](https://portal.azure.com/). 

1. Valitse **Uusi > Web + Mobile > API sovelluksen**. 

    ![Uusi API-sovellus-portaalissa](media/app-service-api-nodejs-api-app/new-api-app-portal.png)

4. Kirjoita **sovelluksen nimi** , joka on yksilöllinen *azurewebsites.net* -toimialue, kuten NodejsAPIApp plus luvun yksilölliseksi. 

    Jos nimi on esimerkiksi `NodejsAPIApp`, URL-osoite on `nodejsapiapp.azurewebsites.net`.

    Jos kirjoitat nimi, jonka joku muu on jo käytössä, näkyviin tulee punainen huutomerkki oikealle.

6. **Resurssiryhmä** avattavasta, valitse **Uusi**ja sitten **Uusi resurssi ryhmänimi** Kirjoita "NodejsAPIAppGroup" tai muu nimi halutessasi. 

    [Resurssiryhmä](../azure-resource-manager/resource-group-overview.md) on kokoelma Azure resursseja, kuten API sovellukset, ja VMs. Tässä opetusohjelmassa on helpointa luoda uusi resurssiryhmä, koska, joka on helppo poistaa yhdessä vaiheessa Azure resurssien avulla voit luoda opetusohjelman.

4. Valitse **Sovellus-palvelun suunnittelu ja sijainti**ja valitse sitten **Luo uusi**.

    ![Luo sovellus-palvelusopimus](./media/app-service-api-nodejs-api-app/newappserviceplan.png)

    Seuraavissa vaiheissa luodaan uusi resurssiryhmä App Service-suunnitteleminen. Sovelluksen palvelusopimus määrittää Laske resurssit, joiden API-sovellus toimii. Esimerkiksi jos vapaa taso, API-sovellus toimii jaetun VMs joitakin maksettu tasoa, se suorittamisen erillinen VMs aikana. Sovelluksen palvelusopimusten vaihtoehdot tietoja on artikkelissa [sovelluksen palvelun suunnitelmien yleiskatsaus](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).

5. Kirjoita **Sovelluksen-palvelun suunnittelu** -sivu "NodejsAPIAppPlan" tai muu nimi, jos käytät mieluummin.

5. Valitse **sijainti** avattavasta luettelosta, joka on lähimpänä sinulle sijaintiin.

    Tämä asetus määrittää, mitkä Azure palvelinkeskuksen sovelluksen suoritetaan. Tässä opetusohjelmassa, voit valita minkä tahansa alueen ja se ei tee huomattavia ero. Mutta tuotannon-sovelluksen halutaan olevan mahdollisimman lähellä ohjelmat, joita käytät sen minimoi [viive](http://www.bing.com/search?q=web%20latency%20introduction&qs=n&form=QBRE&pq=web%20latency%20introduction&sc=1-24&sp=-1&sk=&cvid=eefff99dfc864d25a75a83740f1e0090)palvelimellesi.

5. Valitse **hinnoittelu taso > Näytä kaikki > F1 vapaa**.

    Tässä opetusohjelmassa vapaa hinnoittelu taso antaa suorituskyky riittää.

    ![Valitse Vapauta hinnat taso](./media/app-service-api-nodejs-api-app/selectfreetier.png)

6. Valitse **Sovellus-palvelun suunnittelu** -sivu valitsemalla **OK**.

7. Valitse **Luo** **API sovellus** -sivu.

## <a name="set-up-your-new-api-app-for-git-deployment"></a>Määritä uusi API-sovellus Git käyttöönottoa varten

Asentaa koodisi API-sovelluksen valitseminen tapahtumasarjan vahvistukset Git säilöön Azure sovelluksen-palvelussa. Tämän opetusohjelman-osassa voit luoda tunnistetiedot ja Git säilöön Azure-tietokannassa, joita käytät käyttöönottoa varten.  

1. Kun API-sovellus on luotu, valitse **sovelluksen Services > {API sovelluksen}** portaalin kotisivulla. 

    Portaalin näyttää näiden **API-sovellus** ja **asetukset** .

    ![Portaalin API-sovellus ja asetukset-sivu](media/app-service-api-nodejs-api-app/portalapiappblade.png)

1. **Asetukset** -sivu-Siirry **julkaisun** -osioon ja valitse sitten **käyttöönoton tunnistetiedot**.
 
3. **Määritä käyttöönoton tunnistetiedot** , sivu-käyttäjänimi ja salasana ja valitse sitten **Tallenna**.

    Tarvitset näitä tunnistetietoja julkaisemisen Node.js koodin API-sovellukseen. 

    ![Käyttöönoton tunnistetiedot](media/app-service-api-nodejs-api-app/deployment-credentials.png)

1. Valitse **asetukset** -sivu **käyttöönoton lähteen > Valitse lähteen > paikallisen Git säilöön**, valitse **OK**.

    ![Luo Git Repo](media/app-service-api-nodejs-api-app/create-git-repo.png)

1. Kun Git säilö on luotu Näytä aktiivinen käyttöönottoa sivu muutokset. Säilön on uusi, sinulla ei ole aktiivinen ominaisuuksissa luettelossa. 

    ![Asennuksia ei ole aktiivinen](media/app-service-api-nodejs-api-app/no-active-deployments.png)

1. Kopioi Git säilöön URL-osoite. Voit tehdä tämän Siirry sivu, kun uusi API-sovellus ja tarkista **Essentials** -osa sivu. Huomaa **Git Kloonaa URL-Osoitteen** **Essentials** -osassa. Kun pidät osoitinta tätä URL-Osoitteen päällä, näet kuvakkeen oikealla, kopioi URL-osoite Leikepöydän. Napsauttamalla tätä kuvaketta kopioi URL-osoite.

    ![Git URL-osoitteen hankkiminen-portaalista](media/app-service-api-nodejs-api-app/get-the-git-url-from-the-portal.png)

    **Huomautus**: sinun on seuraavassa osassa URL-osoite joten varmista, että voit tallentaa sen toiseen sijaintiin tällä hetkellä Git Kloonaa.

Nyt kun olet luonut API-sovelluksen kanssa se varmuuskopioiminen Git säilö, voit push koodin kyselyjä säilön käyttöönotto koodin API-sovellukseen. 

## <a name="deploy-your-api-code-to-azure"></a>Ottaa käyttöön Azure API-koodi

Tässä osassa voit luoda paikallisen Git säilöön, jonka on server-koodi Ohjelmointirajapinnan ja sitten painat koodisi kyseisen säilöstä säilöön aiemmin luomasi Azure-tietokannassa.

1. Kopioi `ContactList` kansion sijaintiin, jossa voit käyttää uuden paikallisen Git säilö. Jos opetusohjelman alkuosa, kopioi `ContactList` kohteesta `start` -kansioon. Muussa tapauksessa kopioi `ContactList` - `end` kansio.

1. Komentorivi-työkalun uusi kansio-kansio ja valitse Suorita seuraava komento luo uusi paikallinen Git säilö. 

        git init

     ![Uuden paikallisen Git Repo](media/app-service-api-nodejs-api-app/new-local-git-repo.png)

1. Suorita seuraava komento lisää Git, joka on remote API-sovelluksen säilöön. 

        git remote add azure YOUR_GIT_CLONE_URL_HERE

    **Huomautus**: korvaa merkkijonon "YOUR_GIT_CLONE_URL_HERE" Oma Git Kloonaa URL, jonka kopioit aiemmassa versiossa. 

1. Suorita seuraavat komennot luomiseen, joka sisältää kaikki koodin vahvistaminen. 

        git add .
        git commit -m "initial revision"

    ![Vahvista Git tulostus](media/app-service-api-nodejs-api-app/git-commit-output.png)

1. Jos haluat siirtää koodisi Azure komennon suorittaminen. Kun sinua pyydetään antamaan salasana, kirjoita se, jonka loit aiemmin Azure portaalin.

        git push azure master

    Tämä käynnistää käyttöönoton API-sovellukseen.  

1. Selaimella palata **käyttöönottoa** sivu API-sovelluksen ja näet, että käyttöönotto on käynnissä. 

    ![Näistä käyttöönotto](media/app-service-api-nodejs-api-app/deployment-happening.png)

    Samanaikaisesti viivan komentoliittymä kuvastaa käyttöönoton tilan samalla, kun se tapahtuu. 

    ![Solmun Js käyttöönoton näppäimistöllä](media/app-service-api-nodejs-api-app/node-js-deployment-happening.png)

    Kun asennus on valmis, **käyttöönottoa** sivu kuvastaa koodin muutokset API-sovelluksen käyttöönotto onnistuu. 

## <a name="test-with-the-api-running-in-azure"></a>Testaa Ohjelmointirajapinnan Azure käynnissä
 
3. Kopioi **URL-Osoitteen** API App sivu **Essentials** -osassa. 

    ![Käyttöönotto on valmis](media/app-service-api-nodejs-api-app/deployment-completed.png)

1. REST API-asiakas, kuten Postman tai Fiddler (tai selaimen) määrittää URL-Osoitteen yhteyshenkilöiden API-kutsu, joka on `/contacts` API-sovelluksesi päätepiste. URL-osoite on`https://{your API app name}.azurewebsites.net/contacts`

    Kun lähetät GET-pyynnössä tämän päätepisteen, saat API sovelluksen JSON tulos.

    ![Postman pallolla ohjelmointirajapinta](media/app-service-api-nodejs-api-app/postman-hitting-api.png)

2. Siirry selaimella, `/docs` päätepisteen kokeilemaan Swagger-Käyttöliittymä, kun se suoritetaan Azure.

Nyt kun olet luonut jatkuva toimituksen langallisen ylös, voit tehdä muutokset ja ne käyttöön Azure muokkaamalla valitseminen tapahtumasarjan vahvistukset Azure Git säilöön.

## <a name="next-steps"></a>Seuraavat vaiheet

Tässä vaiheessa olet onnistuneesti luotu API-sovelluksen ja käyttöön Node.js API koodi. Seuraava opetusohjelman näyttää kuinka tarjoaman [JavaScript-asiakkaita, käyttämällä CORS API-sovelluksia](app-service-api-cors-consume-javascript.md).

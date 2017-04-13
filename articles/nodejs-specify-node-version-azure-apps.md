<properties
    pageTitle="Node.js-Version määrittäminen"
    description="Lue, miten voit määrittää Node.js Azure-sivustoista ja pilvipalveluihin versio"
    services=""
    documentationCenter="nodejs"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="specifying-a-nodejs-version-in-an-azure-application"></a>Node.js-versio, joka määrittää Azure-sovelluksessa

Kun isännöinnin Node.js-sovelluksen, haluat ehkä varmistaa, että sovelluksesi käyttää Node.js tietyn version. Voit tehdä tämän sovellusten isännöimät Azure useilla tavoilla.

##<a name="default-versions"></a>Oletus-versiot

Azure myöntämä Node.js versiot päivitetään jatkuvasti. Ellei toisin oletusversio, joka on määritetty `WEBSITE_NODE_DEFAULT_VERSION` ympäristömuuttuja käytetään. Voit ohittaa tämän oletusarvon noudattamalla seuraavissa osissa on tämän artikkelin

> [AZURE.NOTE] Jos isännöit sovelluksesi Azure pilvipalvelussa (verkossa tai työntekijän rooli) ja olet ottanut sovelluksen ensimmäisen kerran, Azure yrittää käyttää Node.js samaa versiota, kun olet asentanut oman kehitysympäristö, jos se vastaa jokin käytettävissä Azure oletusarvo-versioista.

##<a name="versioning-with-packagejson"></a>Versiotietojen package.json kanssa

Voit määrittää Node.js käytettävän lisäämällä seuraavat **package.json** tiedoston version:

    "engines":{"node":version}

Jos *versio* on käyttää tiettyä versionumeroa. Voit voit määrittää monimutkaisempia ehtoja version, kuten:

    "engines":{"node": "0.6.22 || 0.8.x"}

Koska 0.6.22 ei ole käytettävissä isännöintipalvelu ympäristössä yksittäinen versio, sarjan, jossa on käytettävissä on 0,8 uusin versio käyttää sen sijaan - 0.8.4.

##<a name="versioning-websites-with-app-settings"></a>Versiotietojen sivustojen sovelluksen asetukset
Jos isännöit sovellus sivuston, voit määrittää ympäristömuuttuja **WEBSITE_NODE_DEFAULT_VERSION** haluamasi versioon. 

##<a name="versioning-cloud-services-with-powershell"></a>Versiotietojen pilvipalveluihin PowerShellin avulla

Jos isännöit pilvipalvelussa sovelluksen ja sovelluksen PowerShellin Azure käyttöönoton, voit ohittaa Node.js oletusversion **Määrittäminen AzureServiceProjectRole** PowerShell-cmdlet-komennolla. Esimerkki:

    Set-AzureServiceProjectRole WebRole1 Node 0.8.4

Huomautus edellä lauseessa parametrit kirjainkoko on merkitsevä.  Voit varmistaa oikean version Node.js on valittu valitsemalla Oma rooli **package.json** **ohjelmien** -ominaisuutta.

Voit myös hakea Node.js versiot käytettävissä nykyisessä kuin pilvipalveluun sovellusten luettelo **Get-AzureServiceProjectRoleRuntime** .  Tarkista aina riippuvainen Node.js versio on tässä luettelossa.

##<a name="using-a-custom-version-with-azure-websites"></a>Käyttämällä mukautetun version Azure-sivuston käyttö

Azure tarjoaa useita oletusarvon versioita Node.js, haluat ehkä käyttää versio, joka ei ole annettu oletusarvoisesti. Jos sovelluksesi sijaitsee kuin Azure-sivusto, voit tehdä tämän **iisnode.yml** -tiedoston avulla. Seuraavat vaiheet käydä läpi Node.Js mukautetun version käyttäminen Azure-sivuston kanssa:

1. Luo uusi kansio ja luo sitten **server.js** tiedoston sisältä kansio. **Server.js** -tiedostossa on oltava seuraavasti:

        var http = require('http');
        http.createServer(function(req,res) {
          res.writeHead(200, {'Content-Type': 'text/html'});
          res.end('Hello from Azure running node version: ' + process.version + '</br>');
        }).listen(process.env.PORT || 3000);

    Tässä näkyvät niitä käytetään, kun siirryt sivuston Node.js versio.

2. Luoda uuden sivuston ja sivuston taulukon nimi. Esimerkiksi seuraavat käyttää [Azure komentorivin Työkalut] voit luoda uuden Azure-sivuston nimeltä **mywebsite**ja ottaa sitten verkkosivuston Git säilö.

        azure site create mywebsite --git

3. Luo uusi kansio nimeltä **bin** alitaso **server.js** tiedoston sisältävä kansio.

4. Lataa **node.exe** (Windows-versio), jota haluat käyttää sovelluksessa tiettyä versio. Esimerkiksi seuraavat käyttää **curl** Lataa 0.8.1:

        curl -O http://nodejs.org/dist/v0.8.1/node.exe

    Tallenna tiedosto **node.exe** aiemmin luotu **bin** -kansioon.

5. **Iisnode.yml** -tiedoston luominen samassa kansiossa **server.js** -tiedostona ja lisää seuraavan sisällön **iisnode.yml** -tiedosto:

        nodeProcessCommandLine: "D:\home\site\wwwroot\bin\node.exe"

    Tämä polku on, johon projektin **node.exe** -tiedosto luodaan, kun olet julkaissut sovelluksesi Azure-sivustoon.

6. Julkaise sovelluksesi. Esimerkiksi koska voin aiemmin luotu uuteen sivustoon--git-parametri, seuraavat komennot Lisää sovelluksen tiedostojen oma paikallinen Git säilöön ja push ne sivuston säilöön:

        git add .
        git commit -m "testing node v0.8.1"
        git push azure master

    Kun sovellus on julkaistu, avaa sivuston selaimessa. Näkyviin tulee viesti, jossa ilmoitetaan "Hei Azure käynnissä solmu-versiosta: v0.8.1".

##<a name="next-steps"></a>Seuraavat vaiheet

Nyt kun osaat Määritä sovelluksen käyttämä Node.js versio, lue [Moduulit käsitteleminen], [Muodosta ja ota käyttöön Node.js Web-sivuston]ja [käyttämisestä Mac- ja Linux Azure komentorivivalitsimet työkaluja].

Lisätietoja on artikkelissa [Node.js Developer Center](/develop/nodejs/).

[Azure komentorivivalitsimet työkalujen käyttämisestä Mac-ja Linux]: xplat-cli-install.md
[Azure komentorivin Työkalut]: xplat-cli-install.md
[Moduulit käsitteleminen]: nodejs-use-node-modules-azure-apps.md
[Muodosta ja ota käyttöön Node.js Web-sivusto]: web-sites-nodejs-develop-deploy-mac.md

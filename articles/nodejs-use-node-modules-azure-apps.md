<properties
    pageTitle="Node.js moduulit käsitteleminen"
    description="Lue, miten voit käyttää Node.js moduulit Azure App palvelun tai pilvipalveluihin käytettäessä."
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


# <a name="using-nodejs-modules-with-azure-applications"></a>Käyttämällä Node.js moduulit Azure-sovellusten kanssa

Tässä asiakirjassa on ohjeita Node.js moduulit käyttämisestä isännöimät Azure-sovellusten kanssa. Se sisältää ohjeita varmistetaan, että sovelluksesi käyttää moduulin tietyn version sekä alkuperäisen moduulit käyttäminen Azure.

Jos olet jo aiemmin käyttäminen Node.js moduulit, **package.json** ja **npm shrinkwrap.json** tiedostoja, seuraavassa on lyhyt yhteenveto tässä artikkelissa käsiteltävät:

* Azure App palvelun ymmärtää **package.json** ja **npm shrinkwrap.json** tiedostot ja asentaa moduulit tapahtumien nämä tiedostot mukaan.
* Azure pilvipalveluihin toiminta on asennettava kehitysympäristö, kaikki moduulit ja **solmu\_moduulit** hakemisto on asennuspaketin mukana. Se on mahdollista asentaminen moduulit **package.json** tai **npm shrinkwrap.json** tiedostoja käytetään pilvipalveluihin, mutta tämä edellyttää mukauttaminen pilvipalvelussa projektien käyttämä oletusarvo-komentosarjojen tuen ottaminen käyttöön. Esimerkki tämän tekemisestä on artikkelissa [Azure käynnistys tehtävän suorittamiseen npm Asenna välttämiseksi solmu moduulit käyttöönotto](https://github.com/woloski/nodeonazure-blog/blob/master/articles/startup-task-to-run-npm-in-azure.markdown)

> [AZURE.NOTE] Azuren näennäiskoneiden ei käsitellään tämän artikkelin, koska AM käyttöönotto-ratkaisun määräytyy käyttöjärjestelmän virtuaalikoneen ylläpitämä.

##<a name="nodejs-modules"></a>Node.js moduulit

Moduulit ovat ladattavissa JavaScript-paketteja, jotka tiettyjen toimintojen tarjoamista sovelluksen. Moduulin yleensä on asennettu **npm** komentorivin-työkalun avulla, mutta osa (kuten http-moduuli) toimitetaan core Node.js paketin osana.

Moduulit on asennettu, kun ne ovat tallennettuina **solmu\_moduulit** hakemiston sovelluksen kansiorakenne ylimmällä tasolla. Kunkin moduulin sisällä **solmu\_moduulit** directory ylläpitää omassa **solmu\_moduulit** kansio, joka sisältää kaikki moduulit, se on riippuvainen ja tämä toistaa uudelleen jokaisen moduulin aivan riippuvuuden ketjussa. Tällöin kunkin moduulin omassa versiovaatimusten moduulit odotusaika riippuu, mutta sen voi johtaa varsin suuren kansiorakenne on asennettu.

Kun otat **solmu\_moduulit** directory sovelluksen osana, se kasvattaa käyttöönoton verrattuna **package.json** tai **npm shrinkwrap.json** ; tiedoston kokoa kuitenkin varmistaa käytetään tuotannon moduulit version ovat samat kuin kehittäminen.

###<a name="native-modules"></a>Alkuperäisen moduulit

Vaikka useimmat moduulit ovat yksinkertaisesti tekstimuotoon JavaScript-tiedostot, jotkin moduulit ovat käyttöjärjestelmäkohtaiset binaarikuvia. Nämä moduulit käännetään asennuksen aikana, yleensä Python ja solmu gyp avulla. Koska Azure pilvipalveluihin riippuvaisia **solmu\_moduulit** otetaan käyttöön osana asennettu moduulit osana ohjelmistopakettia sovelluksen, kaikki alkuperäisen moduulin kansion pitäisi toimia pilvipalvelussa, kunhan se on asennettu ja käännetty Windows kehittäminen järjestelmässä.

Azure sovelluksen-palvelu ei tue kaikki alkuperäisen moduulit ja saattaa epäonnistua, käännös erittäin tietyt edellytykset sisältävän. Kun taas joissakin Suositut moduuleissa, kuten MongoDB on valinnainen alkuperäisen riippuvuudet ja työ vain hieno ilman niitä, ratkaista kahdella tavalla osoittanut onnistui lähes kaikki alkuperäisen moduulit käytettävissä tänään:

* Suorita **npm asentaminen** Windows-tietokoneessa, jossa on kaikki alkuperäisen moduulin edellytykset on asennettu. Ottaa sitten luotua **solmu\_moduulit** kansion Azure App palvelun sovelluksen osana.
* Azure App palvelu voidaan määrittää mukautetun bash tai shell-komentosarjojen suorittaminen käyttöönoton aikana jossa mahdollisuus suorittaa mukautettuja komentoja ja määrittää tarkasti tapa **npm asentaa** suoritetaan. Katso video, joka esittää, kuinka voit tehdä tämän, [Mukautettu verkkosivuston käyttöönotto-komentosarjojen Kudu].

###<a name="using-a-packagejson-file"></a>Package.json-tiedoston avulla

**Package.json** tiedosto ei voi määrittää sovellus edellyttää niin, että Isäntäympäristö asentaa riippuvuudet kuin edellyttävän sisällyttää ylimmän tason riippuvuudet **solmu\_pakettien** kansion käyttöönoton osana. Kun sovellus on otettu käyttöön, **npm asentaa** -komentoa käytetään jäsentää **package.json** -tiedosto ja asenna kaikki luettelossa riippuvuudet.

Aikana, voit käyttää **--Tallenna**, **--Tallenna keskihajonta**, tai **--Tallenna valinnainen** parametrit-moduulin tekstin lisääminen **package.json** tiedoston automaattisesti moduulit asennettaessa. Lisätietoja on artikkelissa [npm Asenna](https://docs.npmjs.com/cli/install).

Yksi mahdollisen ongelman **package.json** tiedostossa on, että se määrittää vain ylimmän tason riippuvuuksien versio. Kunkin moduuli on asennettu saattaa tai se on riippuvainen moduulit versiota ei voi määrittää ja siten on mahdollista, että saattaa lopputulos kuin yksi kehitteillä käytetään eri riippuvuuden yhdistettyjen.

> [AZURE.NOTE]
> Otettaessa Azure sovelluksen-palveluun, jos <b>package.json</b> tiedoston viittaa alkuperäisen moduuli seuraava virhe tulee näkyviin, kun sovellus, joka käyttää Git julkaiseminen:

>       npm ERR! module-name@0.6.0 install: 'node-gyp configure build'

>       npm ERR! 'cmd "/c" "node-gyp configure build"' failed with 1


###<a name="using-a-npm-shrinkwrapjson-file"></a>Npm shrinkwrap.json-tiedoston avulla

**Npm shrinkwrap.json** -tiedosto on yritys korjaamaan moduuli versiotietojen rajoituksista **package.json** -tiedosto. Kun **package.json** -tiedosto sisältää vain ylimmän tason moduulit-versiot, **npm shrinkwrap.json** -tiedosto sisältää koko moduulin riippuvuus komentoketju versiovaatimusten.

Kun sovellus on valmis tuotannon, voit lukita-version järjestelmävaatimukset ja **npm shrinkwrap.json** -tiedoston luominen **npm shrinkwrap** -komennolla. Tämä käyttää asennettuina olevat versiot **solmu\_moduulit** kansio, ja tallentaa nämä **npm shrinkwrap.json** -tiedostoon. Kun sovellus on otettu käyttöön isännöintipalvelu ympäristössä, **npm asentaa** -komentoa käytetään jäsentää **npm shrinkwrap.json** -tiedosto ja asenna kaikki luettelossa riippuvuudet. Lisätietoja on artikkelissa [npm shrinkwrap](https://docs.npmjs.com/cli/shrinkwrap).

> [AZURE.NOTE]
>Otettaessa Azure sovelluksen-palveluun, jos <b>npm shrinkwrap.json</b> tiedoston viittaa alkuperäisen moduuli seuraava virhe tulee näkyviin, kun sovellus, joka käyttää Git julkaiseminen:

>       npm ERR! module-name@0.6.0 install: 'node-gyp configure build'

>       npm ERR! 'cmd "/c" "node-gyp configure build"' failed with 1


##<a name="next-steps"></a>Seuraavat vaiheet

Nyt kun osaat käyttää Node.js moduulit Azure, lue [määrittää Node.js-version], [Muodosta ja ota käyttöön Node.js verkkosovellukseen]ja [käyttämisestä Mac-ja Linux Azure käyttöliittymä].

Lisätietoja on artikkelissa [Node.js Developer Center](/develop/nodejs/).

[Määritä Node.js-versio]: nodejs-specify-node-version-azure-apps.md
[Käyttämisestä Mac-ja Linux Azure käyttöliittymä]: xplat-cli-install.md
[Muodosta ja ota käyttöön Node.js verkkosovellukseen]: web-sites-nodejs-develop-deploy-mac.md
[Node.js Web Application with Storage on MongoDB (MongoLab)]: store-mongolab-web-sites-nodejs-store-data-mongodb.md
[Build and deploy a Node.js application to an Azure Cloud Service]: cloud-services-nodejs-develop-deploy-app.md
[Mukautettu verkkosivuston käyttöönotto-komentosarjojen Kudu]: /documentation/videos/custom-web-site-deployment-scripts-with-kudu/

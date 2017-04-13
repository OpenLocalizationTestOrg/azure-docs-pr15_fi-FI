<properties
   pageTitle="Aloita Azure erä CLI | Microsoft Azure"
   description="Azure erä palvelun resurssien hallinta Azure CLI erä komentoja lyhyt esittely hankkiminen"
   services="batch"
   documentationCenter=""
   authors="mmacy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="batch"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="multiple"
   ms.workload="big-compute"
   ms.date="09/30/2016"
   ms.author="marsma"/>

# <a name="get-started-with-azure-batch-cli"></a>Azure erä CLI käytön aloittaminen

Office kaikissa ympäristöissä Azure komentorivivalitsimet Interface (Azure-CLI) avulla voit hallita erä tilien ja resurssit, kuten jakavat, projektien ja tehtävien Linux-, Mac- ja Windows-komennon liittymät. Azure-CLI voit suorittaa ja komentosarjan monia samoja tehtäviä voit suorittaa Siirtoerän API, Azure portaalin ja erä PowerShellin cmdlet-komennot.

Tässä artikkelissa perustuu Azure CLI versio 0.10.5.

## <a name="prerequisites"></a>Edellytykset

* [Asenna Azure CLI](../xplat-cli-install.md)

* [Azure-CLI yhdistäminen Azure tilauksen](../xplat-cli-connect.md)

* Vaihda **Resurssienhallinta tilaan**:`azure config mode arm`

>[AZURE.TIP] On suositeltavaa, että olet päivittänyt Azure CLI asennuksen usein, jotta voit hyödyntää palvelupäivityksistä ja parannukset.

## <a name="command-help"></a>Komento-Ohje

Voit näyttää jokaisen komennon ohjeteksti Azure-CLI liittämällä `-h` kuin ainoa tapa komennon jälkeen. Esimerkki:

* Saat lisätietoja `azure` komento, kirjoita:`azure -h`
* Saat kaikki erän komentojen luettelo CLI avulla:`azure batch -h`
* Jos tarvitset apua luomisesta erä-tili, anna:`azure batch account create -h`

Epävarmoissa tapauksissa käyttää `-h` komentorivin vaihtoehto ohjeita Azure CLI komentoja.

## <a name="create-a-batch-account"></a>Erän tilin luominen

Käyttö:

    azure batch account create [options] <name>

Esimerkki:

    azure batch account create --location "West US"  --resource-group "resgroup001" "batchaccount001"

Luo uuden Siirtoerän tilin määritetyt parametrit. Määritä vähintään sijainti, resurssiryhmä ja tilin nimi. Jos sinulla ei vielä ole resurssiryhmä, luo sellainen suorittamalla `azure group create`, ja määritä jokin Azure alueet (esimerkiksi "Länsi US") `--location` vaihtoehto. Esimerkki:

    azure group create --name "resgroup001" --location "West US"

> [AZURE.NOTE] Erän tilin nimen on oltava yksilöllinen Azure tili on luotu alueella. Se voi olla vain pieniä aakkosnumeerisia merkkejä, ja se täytyy 3 – 24 merkkiä. Et voi käyttää erikoismerkkejä, kuten `-` tai `_` erä tilin nimet.

### <a name="linked-storage-account-autostorage"></a>Linkitetyn tallennustilan tilin (autostorage)

(Valinnainen) voit linkittää **yleisiä tarkoitus** tallennustilan tilin erä tilisi luomisen yhteydessä. Erän [sovelluksen pakettien](batch-application-packages.md) -ominaisuus käyttää Blob-objektien tallennustilaan linkitetyn yleisiä tarkoitus tallennustilan tilin, kuten [Erä tiedoston nimeämiskäytännön .NET](batch-task-output.md) -kirjasto. Seuraavia valinnaisia ominaisuuksia auttaa erän tehtävien suorittaminen sovellusten käyttöönotto ja toteaa fonttinsa tiedot.

Jos haluat linkittää aiemmin luodun Azure-tallennustilan tilin uuden Siirtoerän tilin luomisen yhteydessä, Määritä `--autostorage-account-id` vaihtoehto. Tämä vaihtoehto edellyttää täydellinen Resurssitunnus-tallennustilan tilin.

Näytä-tallennustilan tilin tiedot:

    azure storage account show --resource-group "resgroup001" "storageaccount001"

Käytä **URL-osoite** -arvo `--autostorage-account-id` vaihtoehto. Url-arvon alussa on "/ Tilaukset /" ja tilauksen tunnus ja resurssin tallennustilan tilille, polku on:

    azure batch account create --location "West US"  --resource-group "resgroup001" --autostorage-account-id "/subscriptions/8ffffff8-4444-4444-bfbf-8ffffff84444/resourceGroups/resgroup001/providers/Microsoft.Storage/storageAccounts/storageaccount001" "batchaccount001"

## <a name="delete-a-batch-account"></a>Poista erä-tili

Käyttö:

    azure batch account delete [options] <name>

Esimerkki:

    azure batch account delete --resource-group "resgroup001" "batchaccount001"

Poistaa on määritetty erä-tili. Kun sinulta kysytään, Vahvista haluat poistaa tilin (tilin poistaminen voi kestää kauan).

## <a name="manage-account-access-keys"></a>Hallitse tilin pikanäppäimet

Pikanäppäin voit [luoda ja muokata resursseja](#create-and-modify-batch-resources) on erän tilissäsi.

### <a name="list-access-keys"></a>Luettelon pikanäppäimet

Käyttö:

    azure batch account keys list [options] <name>

Esimerkki:

    azure batch account keys list --resource-group "resgroup001" "batchaccount001"

Näyttää tietyn erän tilin tili-näppäimiä.

### <a name="generate-a-new-access-key"></a>Luo uusi pikanäppäin

Käyttö:

    azure batch account keys renew [options] --<primary|secondary> <name>

Esimerkki:

    azure batch account keys renew --resource-group "resgroup001" --primary "batchaccount001"

Luo uudelleen annetun erä tilin määrättyjen-näppäintä.

## <a name="create-and-modify-batch-resources"></a>Luoda ja muokata erä resurssit

Voit luoda, lukea, päivittää, ja poistaa (CRUD) erä resurssit, kuten jakavat, Laske solmujen, projektien ja tehtävien Azure-CLI. CRUD nämä toiminnot vaativat erän tilin nimi ja pikanäppäin ja päätepiste. Voit määrittää ne `-a`, `-k`, ja `-u` asetukset tai määrittäminen- [Ympäristömuuttujat](#credential-environment-variables) joka CLI käyttää automaattisesti (Jos täydennetty).

### <a name="credential-environment-variables"></a>Tunnistetietojen ympäristömuuttujat

Voit määrittää `AZURE_BATCH_ACCOUNT`, `AZURE_BATCH_ACCESS_KEY`, ja `AZURE_BATCH_ENDPOINT` ympäristömuuttujat sijaan määrittäminen `-a`, `-k`, ja `-u` asetukset komentorivillä jokaisen komennon suorittamista. Erän CLI käyttää muuttujia (Jos määritetty) niin, että voit ohittaa `-a`, `-k`, ja `-u` asetukset. Jäljempänä tässä artikkelissa oletetaan, että ympäristömuuttujia käyttö.

>[AZURE.TIP] Luettelo avaimien kanssa `azure batch account keys list`, ja näyttää asiakkaan päätepisteen `azure batch account show`.

### <a name="json-files"></a>JSON-tiedostot

Kun luot erän resursseja, kuten jakavat ja työt, voit määrittää JSON-tiedoston, jossa on uusi resurssi määritysten sijaan kulkeva sen parametrit sellaisena komentorivivalitsimet. Esimerkki:

`azure batch pool create my_batch_pool.json`

Voit suorittaa useita resurssin luominen toimintoja käyttämällä vain komentorivivalitsimet, jotkin ominaisuudet vaativat JSON-muotoisen tiedoston, joka sisältää resurssitiedot. Esimerkiksi sinun on käytettävä JSON-tiedostona, jos haluat määrittää resurssitiedostojen Käynnistä tehtävälle.

Resurssin luominen edellyttää JSON etsimällä viitata [erä REST API viittaus] [ rest_api] MSDN: N ohjeet. "Lisää *resurssilaji"*aihe sisältää Esimerkki JSON resurssi, jonka avulla voit malleiksi JSON tiedostojen luomiseen. Esimerkiksi resurssivarannon luomista JSON löytyvät [lisääminen resurssivarantoon tiliin][rest_add_pool].

>[AZURE.NOTE] Jos määrität JSON-tiedostoa luotaessa resurssin, kaikki muut parametrit, voit määrittää kyseiselle resurssille komentorivillä ohitetaan.

## <a name="create-a-pool"></a>Luo resurssivarantoon

Käyttö:

    azure batch pool create [options] [json-file]

Esimerkki (virtuaalikoneen määritys):

    azure batch pool create --id "pool001" --target-dedicated 1 --vm-size "STANDARD_A1" --image-publisher "Canonical" --image-offer "UbuntuServer" --image-sku "14.04.2-LTS" --node-agent-id "batch.node.ubuntu 14.04"

Esimerkki (Cloud Services-määritys):

    azure batch pool create --id "pool002" --target-dedicated 1 --vm-size "small" --os-family "4"

Luo Laske solmujen resurssivarantoon erä-palvelussa.

Mainittu [erän ominaisuuden yhteenveto](batch-api-basics.md#pool)sinulla on kaksi vaihtoehtoa, kun valitset käyttöjärjestelmän solmujen lisääminen resurssivarantoon: **Virtuaalikoneen määritys** ja **Cloud Services-kokoonpanon määritys**. Käytä `--image-*` voi luoda virtuaalikoneen määritysten jakavat ja `--os-family` voit luoda Cloud Services-kokoonpanon määritys. Et voi määrittää sekä `--os-family` ja `--image-*` asetukset.

Voit määrittää ryhmän [sovelluksen pakettien](batch-application-packages.md) ja [Aloita tehtävä](batch-api-basics.md#start-task)komentoriviltä. Määritä resurssitiedostojen Käynnistä tehtävän kuitenkin käytössä on oltava sen sijaan [JSON-tiedosto](#json-files).

Poista ryhmän kanssa:

    azure batch pool delete [pool-id]

>[AZURE.TIP] Tarkista [luettelon virtuaalikoneen kuvia](batch-linux-nodes.md#list-of-virtual-machine-images) arvot sopiva vaihtoehto `--image-*` asetukset.

## <a name="create-a-job"></a>Työn luominen

Käyttö:

    azure batch job create [options] [json-file]

Esimerkki:

    azure batch job create --id "job001" --pool-id "pool001"

Lisää työn erä-tilille ja määrittää sovellussarjan, joina sen tehtävien suorittamista.

Poista työ kanssa:

    azure batch job delete [job-id]

## <a name="list-pools-jobs-tasks-and-other-resources"></a>Luettelon jakavat, työt, tehtävät ja muut resurssit

Kunkin erän resurssityyppi tukee `list` kyselyt erä-tilisi ja luetteloista resurssien kyseistä tiedostotyyppiä komentoa. Voit esimerkiksi näyttää jakavat tilisi ja projektin tehtävät:

    azure batch pool list
    azure batch task list --job-id "job001"

### <a name="listing-resources-efficiently"></a>Luettelon resurssien tehokkaasti

Nopeampi kyselyt, voit määrittää **Valitse**, **suodattaa**ja **laajentaa** lauseen asetukset `list` toimintoja. Näiden asetusten avulla voit rajoittaa erä palvelun palauttamia tietoja. Palvelinpuolen tapahtuu kaikki suodattimet, koska vain tiedot kiinnostavan ylittää koneisiin. Näiden lausekkeiden avulla voit tallentaa kaistanleveyden (ja sen vuoksi, kun) Kun teet luettelon toimia.

Tämä esimerkiksi palauttaa vain jakavat, joiden tunnukset alkavat "renderTask":

    azure batch task list --job-id "job001" --filter-clause "startswith(id, 'renderTask')"

Erän CLI tukee kaikki erän-palvelun tukemat kolme lausetta:

* `--select-clause [select-clause]`Palauttaa alijoukkoa kunkin kohteen ominaisuudet
* `--filter-clause [filter-clause]`Palauttaa vain yritykset, jotka vastaavat määritettyä OData-lauseke
* `--expand-clause [expand-clause]`Hankkia toimija tiedot yhteen pohjana REST-puhelun. Laajenna-lauseen tukee vain `stats` tällä hetkellä ominaisuus.

Lisätietoja kolme lausetta ja heidän kanssaan luettelon kyselyjä on artikkelissa [Azure erä-palvelun kyselyn tehokkaasti](batch-efficient-list-queries.md).

## <a name="application-package-management"></a>Sovelluksen paketin hallinta

Sovelluksen pakettien lisäämistapaa vaivaton sovellusten oman jakavat Laske solmujen. Voit ladata sovelluksen paketteja, paketti Versionhallinta ja pakettien poistaminen Azure-CLI.

Uuden sovelluksen luominen ja lisääminen paketin-versio:

**Luo** sovelluksen:

    azure batch application create "resgroup001" "batchaccount001" "MyTaskApplication"

**Lisää** sovelluspaketin:

    azure batch application package create "resgroup001" "batchaccount001" "MyTaskApplication" "1.10-beta3" package001.zip

**Aktivoi** paketin:

    azure batch application package activate "resgroup001" "batchaccount001" "MyTaskApplication" "1.10-beta3" zip

Määritä **oletusversio** sovelluksen:

    azure batch application set "resgroup001" "batchaccount001" "MyTaskApplication" --default-version "1.10-beta3"

### <a name="deploy-an-application-package"></a>Sovelluksen paketin käyttöönotto

Voit määrittää vähintään yksi sovellus pakettien käyttöönottoa varten, kun luot uusi ryhmä. Määrittäessäsi paketin varannon luontia milloin se otetaan käyttöön kunkin solmun kuin solmu liitokset resurssivarantoon. Pakettien myös otettu käyttöön, kun solmu on käynnistettävä tai reimaged.

Määritä `--app-package-ref` -asetus käyttöön sovelluspaketin resurssivarantoon solmujen, kun he liittyä ryhmään resurssivarantoon luotaessa. `--app-package-ref` Vaihtoehto hyväksyy sovellusten tunnukset käyttöön Laske solmut puolipisteillä erotettu luettelo.

    azure batch pool create --pool-id "pool001" --target-dedicated 1 --vm-size "small" --os-family "4" --app-package-ref "MyTaskApplication"

Kun luot resurssivarantoon käyttämällä komentoriviparametreja, et voi määrittää tällä hetkellä *version sovelluksen paketin suorittaminen solmujen, esimerkiksi "1.10-beta3" käyttöön* . Tämän vuoksi, sinun on ensin määritettävä oletusversio sovelluksen kanssa `azure batch application set [options] --default-version <version-id>` ennen kuin luot altaan (Katso edellisessä osassa). Jos käytät [JSON tiedoston](#json-files) sijaan komentorivivalitsimet ryhmän luomisen voit kuitenkin määrittää ryhmän paketin versio.

Voit Lisätietoja sovelluksen pakettien [Azure erä sovelluksen pakettien sovellusten käyttöön](batch-application-packages.md).

>[AZURE.IMPORTANT] [Linkki Azure-tallennustilan tilin](#linked-storage-account-autostorage) on käyttää sovelluksen pakettien erä-tiliisi.

### <a name="update-a-pools-application-packages"></a>Päivitä resurssivarantoon sovelluksen pakettien

Jos haluat päivittää aiemmin sovellussarjan liitettyihin sovellukset, Lähetä `azure batch pool set` komennon kanssa `--app-package-ref` vaihtoehto:

    azure batch pool set --pool-id "pool001" --app-package-ref "MyTaskApplication2"

Ottaa käyttöön uuden sovelluspaketin laskemiseen solmujen jo olemassa olevan ryhmän uudelleen tai reimage näiden solmujen:

    azure batch node reboot --pool-id "pool001" --node-id "tvm-3105992504_1-20160930t164509z"

>[AZURE.TIP] Voit hankkia resurssivarantoon, sekä niiden solmu tunnukset solmujen luettelon kanssa `azure batch node list`.

Ota huomioon, että olet on jo määrittänyt sovelluksen ennen käyttöönottoa oletusarvo-versiolla (`azure batch application set [options] --default-version <version-id>`).

## <a name="troubleshooting-tips"></a>Vianmääritysvihjeitä

Tässä osassa on tarkoitus antaa sinulle resursseja, joiden avulla Azure CLI vianmääritys. Ei välttämättä ole ratkaista ongelmat, mutta sen avulla voit tarkentaa syy ja valitse auttaa resurssit.

* Käytä `-h` **ohjeteksti** hakeminen CLI komentoja

* Käytä `-v` ja `-vv` näytettävä **yksityiskohtainen** komennon tulosteesta. `-vv` on "ylimääräisiä" yksityiskohtainen ja näyttää todellisia REST-pyynnöt ja vastaukset. Nämä valitsimet ovat käteviä virheen koko tulosteen näyttämistä varten.

* Voit tarkastella **komennon tuloksen kuin JSON** `--json` vaihtoehto. Esimerkiksi `azure batch pool show "pool001" --json` pool001 käyttäjän ominaisuudet näkyvät JSON-muodossa. Voit kopioida ja muokata tämän tulosteen käyttämisestä `--json-file` (katso tämän artikkelin [JSON-tiedostot](#json-files) ).

* [MSDN-keskustelupalsta erä] [ batch_forum] on auttaa resurssi ja seurattava tarkasti erä työryhmän jäsenet. Muista kirjaa kysymyksiisi, jos tulla ongelmia tai haluat tietyn toiminnon ohjeita.

* Azure-CLI tällä hetkellä tue kaikki erän resurssi-toimintoa. Et voi esimerkiksi tällä hetkellä määrittää-sovelluksen paketin *versio* resurssivarantoon, paketti-tunnus Tässä tapauksessa voit joutua toimittamaan `--json-file` komennon komentorivivalitsimet sijaan. Muista pysyä ajan tasalla CLI uusin päiväkodista tulevien parannukset.

## <a name="next-steps"></a>Seuraavat vaiheet

*  Katso katso ohjeet tämän toiminnon avulla voit hallita ja asentaa sovellukset, voit suorittaa Siirtoerän Laske solmujen [Azure erä sovelluksen pakettien sovellusten käyttöön](batch-application-packages.md) .

* Katso [kyselyn erä palvelun tehokkaasti](batch-efficient-list-queries.md) lisätietoa kohteiden määrän ja tietotyyppi, joka palautetaan kyselyjen erä.

[batch_forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[github_readme]: https://github.com/Azure/azure-xplat-cli/blob/dev/README.md
[rest_api]: https://msdn.microsoft.com/library/azure/dn820158.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
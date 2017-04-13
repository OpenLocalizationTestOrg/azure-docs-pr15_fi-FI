<properties
    pageTitle="Opi hallitsemaan AzureML verkkopalvelut API hallinnan avulla | Microsoft Azure"
    description="Opas, joka esittää, kuinka voit hallita AzureML verkkopalvelut API hallinnan avulla."
    keywords="konepohjaisten oppimistekniikoiden api hallinta"
    services="machine-learning"
    documentationCenter=""
    authors="roalexan"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="roalexan" />


# <a name="learn-how-to-manage-azureml-web-services-using-api-management"></a>Opi hallitsemaan AzureML verkkopalvelut API hallinnan avulla

##<a name="overview"></a>Yleiskatsaus

Tämän oppaan avulla voit nopeasti alkuun API hallinnan avulla voit hallita AzureML verkkopalvelut.

##<a name="what-is-azure-api-management"></a>Mikä on Azure API hallinta?

Azure API hallinta on Azure-palvelu, jolla voit hallita REST API-päätepisteet määrittämällä käytön, käytön rajoittaminen ja Raporttinäkymät-ikkunan seuranta. Valitse [tähän](https://azure.microsoft.com/services/api-management/) lisätietoja Azure API hallinta. Valitse [tähän](../api-management/api-management-get-started.md) lisätietoja ja siitä, miten voit Azure API hallinnan käytön aloittaminen. Muiden oppaan, joka perustuu tämän oppaan, kattaa aiheita, mukaan lukien ilmoituksen määritykset, taso hinnat, vastaus käsittely, käyttäjän todentaminen-tuotteita, developer tilaukset ja käyttö dashboarding luomista.

##<a name="what-is-azureml"></a>Mikä on AzureML?

AzureML on Azure-palvelu, konepohjaisten oppimistekniikoiden avulla voit helposti luoda, ottaminen käyttöön ja jakaa kehittyneen analyysin ratkaisuja. Valitse [tähän](https://azure.microsoft.com/services/machine-learning/) AzureML lisätietoja.

##<a name="prerequisites"></a>Edellytykset

Tämän oppaan suorittamiseen tarvitset:

* Azure tili. Jos sinulla ei ole Azure-tili, napsauttamalla [tätä](https://azure.microsoft.com/pricing/free-trial/) Saat lisätietoja siitä, miten voit luoda ilmainen kokeiluversio tili.
* AzureML tili. Jos sinulla ei ole tiliä AzureML, napsauttamalla [tätä](https://studio.azureml.net/) Saat lisätietoja siitä, miten voit luoda ilmainen kokeiluversio tili.
* Työtila, palvelun ja web-palvelu käyttöön AzureML kokeen api_key. Napsauttamalla [tätä](machine-learning-create-experiment.md) Saat lisätietoja siitä, miten voit luoda AzureML kokeen. Napsauttamalla [tätä](machine-learning-publish-a-machine-learning-web-service.md) Saat lisätietoja siitä, miten voit ottaa käyttöön AzureML kokeen web-palvelu. Voit vaihtoehtoisesti liite A on ohjeet, miten voit luoda ja testaa yksinkertainen AzureML kokeessa ja käyttöönotto web-palvelu.

##<a name="create-an-api-management-instance"></a>Luo erillisen API hallinta

Alla on ohjeet API hallinnan avulla voit hallita AzureML web-palveluun. Luo ensin palvelun esiintymän. Kirjaudu sisään [Perinteinen Portal](https://manage.windowsazure.com/) ja valitse **Uusi** > **Sovelluksen Services** > **API hallinta** > **luominen**.

![Luo esiintymä](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-instance.png)

Määritä yksilöllinen **URL-osoite**. Tässä oppaassa käytetään **demoazureml** – Valitse jokin toinen tarvitset. Valitse haluamasi **tilaus** - ja **alueasetusten** palveluesiintymä. Kun olet tehnyt haluamasi valinnat, valitse Seuraava.

![Luo-palvelu-1](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-service-1.png)

Määritä arvo **Organisaationimi**. Tässä oppaassa käytetään **demoazureml** – Valitse jokin toinen tarvitset. Kirjoita sähköpostiosoitteesi **järjestelmänvalvojan sähköposti** -kenttään. Tätä sähköpostiosoitetta käytetään API hallintajärjestelmän ilmoituksia.

![Luo-palvelu-2](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-service-2.png)

Valitse Luo palveluesiintymä-valintaruutu. *Kestää jopa 30 minuuttia luodaan uusi palvelu*.

##<a name="create-the-api"></a>Luo Ohjelmointirajapinnan

Kun palveluesiintymä on luotu, seuraava vaihe on Ohjelmointirajapinnan luomiseen. Ohjelmointirajapinnan koostuu joukko toiminnoista, joita voidaan kutsua asiakas-sovelluksesta. API-toiminnot ovat välityspalvelimen aiemmin web-palveluihin. Tämän oppaan Luo API välityspalvelimen aiemmin AzureML RRS ja BES web-palveluihin.

Ohjelmointirajapinnan on luotu ja määritetty API publisher-portaalista, jossa Azure perinteinen portaalin kautta. Publisher-portaalin saavuttamiseksi palvelun esiintymän valitseminen

![Valitse esiintymää](./media/machine-learning-manage-web-service-endpoints-using-api-management/select-service-instance.png)

Valitse **Hallitse** API Management-palvelun Azure perinteinen-portaalissa.

![hallinta-palvelu](./media/machine-learning-manage-web-service-endpoints-using-api-management/manage-service.png)

Valitse **ohjelmointirajapinnan** vasemmalla **API hallinta** -valikosta ja valitse sitten **Lisää API**.

![API-hallinta-valikko](./media/machine-learning-manage-web-service-endpoints-using-api-management/api-management-menu.png)

Kirjoita **AzureML esittely API** **Verkko-Ohjelmointirajapinnan nimi**. Kirjoita **https://ussouthcentral.services.azureml.net** **WWW-palvelun URL**. Kirjoita **azureml esittely** **API WWW-URL-osoite liite**. Tarkista **HTTPS** **API WWW-URL-osoite** mallina. Valitse **tuotteet** **Starter** . Kun olet valmis, valitse **Tallenna** Ohjelmointirajapinnan luomiseen.

![Lisää-uusi-ohjelmointirajapinta](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-new-api.png)

##<a name="add-the-operations"></a>Lisää toimintoja

Valitse **Lisää toiminto** toimintojen lisääminen tämän API.

![Lisää toiminto](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-operation.png)

**Uusi toiminto** -ikkunassa näkyvät ja **allekirjoitus** -välilehti valittuna oletusarvoisesti.

##<a name="add-rrs-operation"></a>Lisää RRS-toiminto

Luo AzureML RRS service-toiminto. Valitse **viestin** **HTTP-toiminnon**. Tyypin **/services/ /workspaces/ {työtilan} {palvelun} / execute?api version = {apiversion} & tiedot = {tiedot}** **mallin URL-osoite**. Kirjoita **Näyttönimi** **RRS suorittaminen** .

![Lisää rrs-toiminto-allekirjoitus](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-rrs-operation-signature.png)

Valitse **vastaukset** > vasemman ja valitse**Lisää** **200 OK**. Valitse Tallenna toiminto **tallentaa** .

![Lisää rrs-toiminto-vastaus](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-rrs-operation-response.png)

##<a name="add-bes-operations"></a>Lisää BES toiminnot

Näyttökuvat eivät sisälly BES toimille, kun ne ovat hyvin samankaltaisia kuin ne lisätään RRS-toimintoa.

###<a name="submit-but-not-start-a-batch-execution-job"></a>Lähetä (mutta ei välttämättä käynnisty) eräkäsittely työ

Valitse **Lisää toiminto** AzureML BES-toiminnon lisääminen Ohjelmointirajapinnan. Valitse **viestin** **HTTP-toiminnon**. Tyypin **/services/ /workspaces/ {työtilan} {palvelu} / jobs?api version = {apiversion}** **mallin URL-osoite**. Kirjoita **Näyttönimi** **BES Lähetä** . Valitse **vastaukset** > vasemman ja valitse**Lisää** **200 OK**. Valitse Tallenna toiminto **tallentaa** .

###<a name="start-a-batch-execution-job"></a>Eräkäsittely työn aloittaminen

Valitse **Lisää toiminto** AzureML BES-toiminnon lisääminen Ohjelmointirajapinnan. Valitse **viestin** **HTTP-toiminnon**. Tyypin **/workspaces/ {työtilan} {palvelun} /services/ /jobs/ {työn tunnus} / start?api version = {apiversion}** **mallin URL-osoite**. Kirjoita **Näyttönimi** **BES Käynnistä-painiketta** . Valitse **vastaukset** > vasemman ja valitse**Lisää** **200 OK**. Valitse Tallenna toiminto **tallentaa** .

###<a name="get-the-status-or-result-of-a-batch-execution-job"></a>Hae tila tai eräkäsittely työn tulos

Valitse **Lisää toiminto** AzureML BES-toiminnon lisääminen Ohjelmointirajapinnan. Valitse **HAE** **HTTP-toiminnon**. Kirjoita **/workspaces/ {työtilan} {palvelu} /services/ /jobs/ {työn tunnus} ?api version = {apiversion}** **mallin URL-osoite**. Kirjoita **Näyttönimi** **BES tila** . Valitse **vastaukset** > vasemman ja valitse**Lisää** **200 OK**. Valitse Tallenna toiminto **tallentaa** .

###<a name="delete-a-batch-execution-job"></a>Eräkäsittely työn poistaminen

Valitse **Lisää toiminto** AzureML BES-toiminnon lisääminen Ohjelmointirajapinnan. Valitse **Poista** **HTTP-toiminnon**. Kirjoita **/workspaces/ {työtilan} {palvelu} /services/ /jobs/ {työn tunnus} ?api version = {apiversion}** **mallin URL-osoite**. Kirjoita **Näyttönimi** **BES poistaminen** . Valitse **vastaukset** > vasemman ja valitse**Lisää** **200 OK**. Valitse Tallenna toiminto **tallentaa** .

##<a name="call-an-operation-from-the-developer-portal"></a>Toiminnon kutsua Developer-portaalissa

Toimintoja voidaan kutsua suoraan Developer-portaalissa, joiden avulla voit tarkastella ja testaa Ohjelmointirajapinnan toimintojen kätevästi. Opas tässä vaiheessa voit soittaa **RRS suorittaa** menetelmää, joka on lisätty **AzureML esittely API**. Valitse valikosta yläreunassa **Developer-portaalissa** perinteinen portaalin oikealla puolella.

![Developer-portaalissa](./media/machine-learning-manage-web-service-endpoints-using-api-management/developer-portal.png)

Valitse **ohjelmointirajapinnan** yläreunan valikosta ja valitse sitten **AzureML esittely API** voit tarkastella käytettävissä toimintoja.

![demoazureml-ohjelmointirajapinta](./media/machine-learning-manage-web-service-endpoints-using-api-management/demoazureml-api.png)

Valitse **RRS suorittaa** toiminnon. Valitse **Kokeile**.

![Kokeile it](./media/machine-learning-manage-web-service-endpoints-using-api-management/try-it.png)

Kirjoita oman **työtilan**, **palvelu**, **apiversion** **2.0** - ja **Tosi** **tiedot**pyynnön parametrit. Voit etsiä **työtilan** ja **palvelun** AzureML web-palvelu-koontinäytön (katso **testi WWW-palvelun** liite A).

Pyyntö otsikot, valitse **Lisää ylätunniste** ja kirjoita **Sisältötyyppi** ja **sovelluksen/json**, ja valitse **Lisää ylätunniste** napsauttamalla ja kirjoita **luvan** ja **haltijan <YOUR AZUREML SERVICE API-KEY> **. Voit tarkistaa AzureML WWW-palvelun Raporttinäkymät-ikkunan **ohjelmointirajapinnan avain** (katso **testi WWW-palvelun** liite A).

Kirjoita **{"Syötteiden": {"input1": {"ColumnNames": ["Sarake2"], "arvot": [["Tämä on hyvä päivän"]]}}, "GlobalParameters": {}}** pyynnön body.

![azureml esittely-ohjelmointirajapinta](./media/machine-learning-manage-web-service-endpoints-using-api-management/azureml-demo-api.png)

Valitse **Lähetä**.

![Lähetä](./media/machine-learning-manage-web-service-endpoints-using-api-management/send.png)

Kun toiminto käynnistetään, developer-portaalissa näyttää taustatietokantaan-palvelu, **vastauksen tila**, **vastausten otsikot**ja **vastaus sisällön** **Pyydetty URL-osoite** .

![vastauksen tila](./media/machine-learning-manage-web-service-endpoints-using-api-management/response-status.png)

##<a name="appendix-a---creating-and-testing-a-simple-azureml-web-service"></a>Liite A - luomisesta ja testaus yksinkertainen AzureML web-palvelu

###<a name="creating-the-experiment"></a>Kokeen luominen

Alla on yksinkertainen AzureML kokeen luominen ja käyttöönotto WWW-palvelulle. Web palvelu on kuin syötteen haluamaansa tekstisarake ja palauttaa vastaavan kokonaislukuina ominaisuusjoukko. Esimerkki:

Teksti | Hajautettujen teksti
--- | ---
Tämä on hyvä päivä | 1 1 2 2 0 2 0 1

Ensin selaimella valittua, siirry: [https://studio.azureml.net/](https://studio.azureml.net/) ja anna tunnistetiedot kirjautua sisään. Luo uusi tyhjä kokeen seuraavaksi.

![Etsi-koe-mallit](./media/machine-learning-manage-web-service-endpoints-using-api-management/search-experiment-templates.png)

Nimeä se **SimpleFeatureHashingExperiment**. Laajenna **Tallennettu tietojoukkoja** ja vedä oman kokeen **Kirjan tarkistukset-Amazon** .

![Perus-ominaisuus-hajautuksessa-kokeen](./media/machine-learning-manage-web-service-endpoints-using-api-management/simple-feature-hashing-experiment.png)

Laajenna **Muuntamiseen** ja **käytöstä** ja **Valitse sarakkeet-tietojoukko** vetäminen oman kokeen. Yhdistä **kirjan tarkistukset-Amazon** **tietojoukon**sarakkeet.

![Valitse sarakkeet](./media/machine-learning-manage-web-service-endpoints-using-api-management/project-columns.png)

Valitse **Valitse sarakkeet-tietojoukko** valitsemalla **Käynnistä sarakevalitsin** ja valitse **Sarake2**. Valitse Ota muutokset käyttöön valintamerkki.

![Valitse sarakkeet](./media/machine-learning-manage-web-service-endpoints-using-api-management/select-columns.png)

Laajenna **Tekstin Analytics** ja vedä kokeen **Ominaisuus hajautuksessa** . Yhteyden muodostaminen **ominaisuus hajautuksessa** **tietojoukon sarakkeet** .

![Yhdistä-project-sarakkeet](./media/machine-learning-manage-web-service-endpoints-using-api-management/connect-project-columns.png)

Kirjoita **3** **Hashing bitsize**. Tämä vaihtoehto Luo 8 (23) sarakkeita.

![hajautuksessa bitsize](./media/machine-learning-manage-web-service-endpoints-using-api-management/hashing-bitsize.png)

Tässä vaiheessa voit halutessasi valitsemalla Testaa kokeen **Suorita** .

![Suorita](./media/machine-learning-manage-web-service-endpoints-using-api-management/run.png)

###<a name="create-a-web-service"></a>Luo web-palvelu

Nyt voit luoda verkkopalvelun. Laajenna **Web-palvelu** ja vedä oman kokeen **syöte** . Yhdistä **syötteen** **ominaisuus hajautuksessa**. Myös vetää **tulosteen** oman kokeen. Yhdistä **tulosteen** **ominaisuus hajautuksessa**.

![tulostus-ja-ominaisuus-hajautuksessa](./media/machine-learning-manage-web-service-endpoints-using-api-management/output-to-feature-hashing.png)

Valitse **Julkaise web-palvelu**.

![Julkaise-web-palvelu](./media/machine-learning-manage-web-service-endpoints-using-api-management/publish-web-service.png)

Valitse **Kyllä** kokeen julkaisemaan.

![Kyllä-,-julkaiseminen](./media/machine-learning-manage-web-service-endpoints-using-api-management/yes-to-publish.png)

###<a name="test-the-web-service"></a>WWW-palvelun testaaminen

AzureML web-palvelu sisältää RSS (pyynnön ja vastauksen service) ja BES (erä suorittamisen service) päätepisteet. RSS on synkronoitu suorittamista varten. BES on asynkroninen työn suorittamisen. Voit testata WWW-palvelun otoksen Python tietolähteeseen alla, voit joutua, lataa ja asenna Azure SDK Python (katso: [asentaminen Python](../python-how-to-install.md)).

Sinun on myös **työtilan**, **palvelun**ja että kokeen **api_key** otoksen lähteen alla. Voit etsiä työtilan ja palvelun valitsemalla **Pyynnön ja vastauksen** tai **Eräkäsittely** lisääminen web-palvelu koontinäytön kokeen osalta.

![Etsi-työtila-ja-palvelu](./media/machine-learning-manage-web-service-endpoints-using-api-management/find-workspace-and-service.png)

Löydät **api_key** oman kokeen WWW-palvelun raporttinäkymät-ikkunassa valitsemalla.

![Etsi-api-näppäin](./media/machine-learning-manage-web-service-endpoints-using-api-management/find-api-key.png)

####<a name="test-rrs-endpoint"></a>Testaa RRS päätepiste

#####<a name="test-button"></a>Testaa-painike

Voit testata RRS päätepisteen helposti on valitsemalla **Testaa** WWW-palvelun koontinäytössä.

![testaaminen](./media/machine-learning-manage-web-service-endpoints-using-api-management/test.png)

Kirjoita **Tämä on hyvä päivän** **Sarake2**. Napsauta valintamerkkiä.

![Kirjoita tiedot](./media/machine-learning-manage-web-service-endpoints-using-api-management/enter-data.png)

Näet suunnilleen

![Esimerkki tulostus](./media/machine-learning-manage-web-service-endpoints-using-api-management/sample-output.png)

#####<a name="sample-code"></a>Esimerkki koodi

Toinen tapa Testaa yhteyttä RRS on asiakas-koodista. Raporttinäkymät-ikkunan ja vieritä sitten **pyynnön ja vastauksen** , näet esimerkkejä koodin C#, Python ja R. Näet myös RRS pyynnön, mukaan lukien pyynnön URI-syntaksin ylä- ja leipätekstin.

Tämän oppaan näyttää Python esimerkki toimii. Sinun on muokattava **työtilan**, **palvelun**ja että kokeen **api_key** .

    import urllib2
    import json
    workspace = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE WORKSPACE ID>"
    service = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE SERVICE ID>"
    api_key = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE API KEY>"
    data = {
    "Inputs": {
        "input1": {
            "ColumnNames": ["Col2"],
            "Values": [ [ "This is a good day" ] ]
        },
    },
    "GlobalParameters": { }
    }
    url = "https://ussouthcentral.services.azureml.net/workspaces/" + workspace + "/services/" + service + "/execute?api-version=2.0&details=true"
    headers = {'Content-Type':'application/json', 'Authorization':('Bearer '+ api_key)}
    body = str.encode(json.dumps(data))
    try:
        req = urllib2.Request(url, body, headers)
        response = urllib2.urlopen(req)
        result = response.read()
        print "result:" + result
            except urllib2.HTTPError, error:
        print("The request failed with status code: " + str(error.code))
        print(error.info())
        print(json.loads(error.read()))

####<a name="test-bes-endpoint"></a>Testaa BES päätepiste
Valitse **Eräkäsittely** Raporttinäkymät-ikkunan ja vieritä. Näet esimerkkejä koodin C#, Python ja R. Näet myös BES pyynnöt lähetät työn, työn aloittaminen, tila ja tulokset työn ja työn poistaminen syntaksia.

Tämän oppaan näyttää Python esimerkki toimii. Voit joutua muokkaamaan **työtilan**, **palvelun**ja että kokeen **api_key** . Lisäksi voit joutua muokkaamaan **tallennustilan tilin nimi**, **tallennustilan tilin avain**ja **tallennustilaa säilön nimi**. Lisäksi tarvitset muokata **tiedoston** sijainti ja **kohdetiedosto**sijainti.

    import urllib2
    import json
    import time
    from azure.storage import *
    workspace = "<REPLACE WITH YOUR WORKSPACE ID>"
    service = "<REPLACE WITH YOUR SERVICE ID>"
    api_key = "<REPLACE WITH THE API KEY FOR YOUR WEB SERVICE>"
    storage_account_name = "<REPLACE WITH YOUR AZURE STORAGE ACCOUNT NAME>"
    storage_account_key = "<REPLACE WITH YOUR AZURE STORAGE KEY>"
    storage_container_name = "<REPLACE WITH YOUR AZURE STORAGE CONTAINER NAME>"
    input_file = "<REPLACE WITH THE LOCATION OF YOUR INPUT FILE>" # Example: C:\\mydata.csv
    output_file = "<REPLACE WITH THE LOCATION OF YOUR OUTPUT FILE>" # Example: C:\\myresults.csv
    input_blob_name = "mydatablob.csv"
    output_blob_name = "myresultsblob.csv"
    def printHttpError(httpError):
    print("The request failed with status code: " + str(httpError.code))
    print(httpError.info())
    print(json.loads(httpError.read()))
    return
    def saveBlobToFile(blobUrl, resultsLabel):
    print("Reading the result from " + blobUrl)
    try:
        response = urllib2.urlopen(blobUrl)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    with open(output_file, "w+") as f:
        f.write(response.read())
    print(resultsLabel + " have been written to the file " + output_file)
    return
    def processResults(result):
    first = True
    results = result["Results"]
    for outputName in results:
        result_blob_location = results[outputName]
        sas_token = result_blob_location["SasBlobToken"]
        base_url = result_blob_location["BaseLocation"]
        relative_url = result_blob_location["RelativeLocation"]
        print("The results for " + outputName + " are available at the following Azure Storage location:")
        print("BaseLocation: " + base_url)
        print("RelativeLocation: " + relative_url)
        print("SasBlobToken: " + sas_token)
        if (first):
            first = False
            url3 = base_url + relative_url + sas_token
            saveBlobToFile(url3, "The results for " + outputName)
    return

    def invokeBatchExecutionService():
    url = "https://ussouthcentral.services.azureml.net/workspaces/" + workspace +"/services/" + service +"/jobs"
    blob_service = BlobService(account_name=storage_account_name, account_key=storage_account_key)
    print("Uploading the input to blob storage...")
    data_to_upload = open(input_file, "r").read()
    blob_service.put_blob(storage_container_name, input_blob_name, data_to_upload, x_ms_blob_type="BlockBlob")
    print "Uploaded the input to blob storage"
    input_blob_path = "/" + storage_container_name + "/" + input_blob_name
    connection_string = "DefaultEndpointsProtocol=https;AccountName=" + storage_account_name + ";AccountKey=" + storage_account_key
    payload =  {
        "Input": {
            "ConnectionString": connection_string,
            "RelativeLocation": input_blob_path
        },
        "Outputs": {
            "output1": { "ConnectionString": connection_string, "RelativeLocation": "/" + storage_container_name + "/" + output_blob_name },
        },
        "GlobalParameters": {
        }
    }
        body = str.encode(json.dumps(payload))
    headers = { "Content-Type":"application/json", "Authorization":("Bearer " + api_key)}
    print("Submitting the job...")
    # submit the job
    req = urllib2.Request(url + "?api-version=2.0", body, headers)
    try:
        response = urllib2.urlopen(req)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    result = response.read()
    job_id = result[1:-1] # remove the enclosing double-quotes
    print("Job ID: " + job_id)
    # start the job
    print("Starting the job...")
    req = urllib2.Request(url + "/" + job_id + "/start?api-version=2.0", "", headers)
    try:
        response = urllib2.urlopen(req)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    url2 = url + "/" + job_id + "?api-version=2.0"

    while True:
        print("Checking the job status...")
        # If you are using Python 3+, replace urllib2 with urllib.request in the follwing code
        req = urllib2.Request(url2, headers = { "Authorization":("Bearer " + api_key) })
        try:
            response = urllib2.urlopen(req)
        except urllib2.HTTPError, error:
            printHttpError(error)
            return
        result = json.loads(response.read())
        status = result["StatusCode"]
        print "status:" + status
        if (status == 0 or status == "NotStarted"):
            print("Job " + job_id + " not yet started...")
        elif (status == 1 or status == "Running"):
            print("Job " + job_id + " running...")
        elif (status == 2 or status == "Failed"):
            print("Job " + job_id + " failed!")
            print("Error details: " + result["Details"])
            break
        elif (status == 3 or status == "Cancelled"):
            print("Job " + job_id + " cancelled!")
            break
        elif (status == 4 or status == "Finished"):
            print("Job " + job_id + " finished!")
            processResults(result)
            break
        time.sleep(1) # wait one second
    return
    invokeBatchExecutionService()

<properties
    pageTitle="Luo Hadoop, HBase tai myrsky klustereiden kääntö ja Azure REST-Ohjelmointirajapinnalla HDInsight Linux | Microsoft Azure"
    description="Opettele luomaan Linux-pohjaiset HDInsight klustereiden kääntö, Azure Resurssienhallinta mallit ja Azure REST-Ohjelmointirajapinnalla. Voit määrittää klusterin tyyppi (Hadoop, HBase tai myrsky) tai komentosarjojen avulla voit asentaa mukautettuja osia."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="10/11/2016"
    ms.author="larryfr"/>

#<a name="create-linux-based-clusters-in-hdinsight-using-curl-and-the-azure-rest-api"></a>Luo Linux-pohjaiset klustereiden HDInsight kääntö ja Azure REST-Ohjelmointirajapinnalla

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Azure REST-Ohjelmointirajapinnalla avulla voit suorittaa Azure alustan, mukaan lukien uudet resurssit, kuten Linux-pohjaiset HDInsight klustereiden luomalla maksuttomien palveluiden hallinta-toimintoja. Tässä asiakirjassa kerrotaan luominen Azure Resurssienhallinta mallien avulla voit määrittää HDInsight-klusterin ja liittyvän tallennustilan ja sitten mallin käyttöön, voit luoda uuden HDInsight-klusterin Azure REST-Ohjelmointirajapinnalla kääntö avulla.

> [AZURE.IMPORTANT] Ohjeita tämän asiakirjan käyttää HDInsight-klusterin oletusmäärän työntekijä solmujen (4). Jos aio yli 32 työntekijä solmut-klusterin luominen tai mukaan skaalaus klusterin luonnin jälkeen, on valittava pään solmu kokoa, ja vähintään 8 sydämiä 14 gt RAM-muistia.
>
> Saat lisätietoja solmu kokojen ja niihin liittyvät kustannukset, [HDInsight hinnat](https://azure.microsoft.com/pricing/details/hdinsight/).

##<a name="prerequisites"></a>Edellytykset

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


- **Azure-tilaus**. Katso [Hae Azure maksuttoman kokeiluversion](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- __Azure CLI__. Azure-CLI käytetään luomaan palvelun nykyarvo; koon muuttamiseen tarkoitettu sitten Luo todennus tunnusten Azure REST API-pyyntöihin.

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

- __cURL__. Tämän apuohjelman paketti hallintajärjestelmän kautta tai voi ladata [http://curl.haxx.se/](http://curl.haxx.se/).

    > [AZURE.NOTE] Jos käytössäsi on PowerShell, voit suorittaa tässä asiakirjassa komennot, sinun on ensin poistettava `curl` alias, se luo oletusarvoisesti. Tämä alias käyttää sen sijaan, että kääntö, kun käytät Käynnistä-WebRequest, PowerShell-cmdlet `curl` PowerShell-kehotteessa-komento ja palauttaa virheitä kattaa monet tässä asiakirjassa käytetyt komennot.
    > 
    > Jos haluat poistaa tämän tunnuksen, käytä PowerShell-kehotteessa seuraava:
    >
    > `Remove-item alias:curl`
    >
    > Kun tunnus on poistettu, sinun pitäisi käyttää kääntö on asennettuna järjestelmään.

### <a name="access-control-requirements"></a>Access-ohjausobjektin vaatimukset

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

##<a name="create-a-template"></a>Mallin luominen

Azure Resurssienhallinta mallit ovat JSON-tiedostoja, jotka kuvaavat __resurssiryhmä__ ja kaikki sen resurssit (esimerkiksi Hdinsightista.) Malliin tämän menetelmän avulla voit määrittää kaikki resurssit, jotka tarvitset HDInsight yhteen malliin ja hallita ryhmän kokonaisuutena __käyttöönottoa__ , ota muutokset käyttöön ryhmän kautta.

Mallit toimitetaan yleensä kaksi osaa. mallin ja parametrit-tiedosto, joka täyttää tietyt arvoilla määrityksiin. Exmaple, klusterinimi, järjestelmänvalvojan nimi ja salasana. Kun käytät suoraan REST-Ohjelmointirajapinta, sinun täytyy yhdistää ne yhteen tiedostoon. JSON-tiedoston muoto on:

    {
        "properties": {
            "template": {
                contents of template file
            },
            "mode": "deploymentmode",
            "Parameters": {
                contents of the parameters element from parameters file
            }
        }
    }

Esimerkiksi jokin seuraavista ehdoista fuusion mallin ja parametrit-tiedostot- [https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password](https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password), joka luo Linux-pohjaiset klusterin suojatun SSH käyttäjätilin salasanan avulla.

    {
        "properties": {
            "template": {
                "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                "contentVersion": "1.0.0.0",
                "parameters": {
                    "location": {
                        "type": "string",
                        "allowedValues": ["Central US",
                        "East Asia",
                        "East US",
                        "Japan East",
                        "Japan West",
                        "North Europe",
                        "South Central US",
                        "Southeast Asia",
                        "West Europe",
                        "West US"],
                        "metadata": {
                            "description": "The location where all azure resources will be deployed."
                        }
                    },
                    "clusterType": {
                        "type": "string",
                        "allowedValues": ["hadoop",
                        "hbase",
                        "storm",
                        "spark"],
                        "metadata": {
                            "description": "The type of the HDInsight cluster to create."
                        }
                    },
                    "clusterName": {
                        "type": "string",
                        "metadata": {
                            "description": "The name of the HDInsight cluster to create."
                        }
                    },
                    "clusterLoginUserName": {
                        "type": "string",
                        "metadata": {
                            "description": "These credentials can be used to submit jobs to the cluster and to log into cluster dashboards."
                        }
                    },
                    "clusterLoginPassword": {
                        "type": "securestring",
                        "metadata": {
                            "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
                        }
                    },
                    "sshUserName": {
                        "type": "string",
                        "metadata": {
                            "description": "These credentials can be used to remotely access the cluster."
                        }
                    },
                    "sshPassword": {
                        "type": "securestring",
                        "metadata": {
                            "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
                        }
                    },
                    "clusterStorageAccountName": {
                        "type": "string",
                        "metadata": {
                            "description": "The name of the storage account to be created and be used as the cluster's storage."
                        }
                    },
                    "clusterWorkerNodeCount": {
                        "type": "int",
                        "defaultValue": 4,
                        "metadata": {
                            "description": "The number of nodes in the HDInsight cluster."
                        }
                    }
                },
                "variables": {
                    "defaultApiVersion": "2015-05-01-preview",
                    "clusterApiVersion": "2015-03-01-preview"
                },
                "resources": [{
                    "name": "[parameters('clusterStorageAccountName')]",
                    "type": "Microsoft.Storage/storageAccounts",
                    "location": "[parameters('location')]",
                    "apiVersion": "[variables('defaultApiVersion')]",
                    "dependsOn": [],
                    "tags": {
                        
                    },
                    "properties": {
                        "accountType": "Standard_LRS"
                    }
                },
                {
                    "name": "[parameters('clusterName')]",
                    "type": "Microsoft.HDInsight/clusters",
                    "location": "[parameters('location')]",
                    "apiVersion": "[variables('clusterApiVersion')]",
                    "dependsOn": ["[concat('Microsoft.Storage/storageAccounts/',parameters('clusterStorageAccountName'))]"],
                    "tags": {
                        
                    },
                    "properties": {
                        "clusterVersion": "3.2",
                        "osType": "Linux",
                        "clusterDefinition": {
                            "kind": "[parameters('clusterType')]",
                            "configurations": {
                                "gateway": {
                                    "restAuthCredential.isEnabled": true,
                                    "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
                                    "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
                                }
                            }
                        },
                        "storageProfile": {
                            "storageaccounts": [{
                                "name": "[concat(parameters('clusterStorageAccountName'),'.blob.core.windows.net')]",
                                "isDefault": true,
                                "container": "[parameters('clusterName')]",
                                "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('clusterStorageAccountName')), variables('defaultApiVersion')).key1]"
                            }]
                        },
                        "computeProfile": {
                            "roles": [{
                                "name": "headnode",
                                "targetInstanceCount": "2",
                                "hardwareProfile": {
                                    "vmSize": "Standard_D3"
                                },
                                "osProfile": {
                                    "linuxOperatingSystemProfile": {
                                        "username": "[parameters('sshUserName')]",
                                        "password": "[parameters('sshPassword')]"
                                    }
                                }
                            },
                            {
                                "name": "workernode",
                                "targetInstanceCount": "[parameters('clusterWorkerNodeCount')]",
                                "hardwareProfile": {
                                    "vmSize": "Standard_D3"
                                },
                                "osProfile": {
                                    "linuxOperatingSystemProfile": {
                                        "username": "[parameters('sshUserName')]",
                                        "password": "[parameters('sshPassword')]"
                                    }
                                }
                            }]
                        }
                    }
                }],
                "outputs": {
                    "cluster": {
                        "type": "object",
                        "value": "[reference(resourceId('Microsoft.HDInsight/clusters',parameters('clusterName')))]"
                    }
                }
            },
            "mode": "incremental",
            "Parameters": {
                "location": {
                    "value": "North Europe"
                },
                "clusterName": {
                    "value": "newclustername"
                },
                "clusterType": {
                    "value": "hadoop"
                },
                "clusterStorageAccountName": {
                    "value": "newstoragename"
                },
                "clusterLoginUserName": {
                    "value": "admin"
                },
                "clusterLoginPassword": {
                    "value": "changeme"
                },
                "sshUserName": {
                    "value": "sshuser"
                },
                "sshPassword": {
                    "value": "changeme"
                }
            }
        }
    }

Tässä esimerkissä käytetään tämän asiakirjan ohjeita. Tilalle arvoja, jota haluat käyttää yhteyttä klusterin paikkamerkin _arvot_ asiakirjan lopussa __Parametrit__ -osassa.

##<a name="login-to-your-azure-subscription"></a>Azure-tilaukseen kirjautuminen

[Yhdistä Azure tilaukseen Azure komentorivivalitsimet Interface (Azure CLI)-](../xplat-cli-connect.md) kuvattu ohjeiden mukaisesti ja muodostaa tilauksen käyttämällä `azure login` komento.

##<a name="create-a-service-principal"></a>Luo palvelun lyhennyksen.

> [AZURE.NOTE] Nämä vaiheet ovat lyhennetyt versio [todennustapa palvelun lyhennys Azure Resource Manager](../resource-group-authenticate-service-principal.md#authenticate-service-principal-with-password---azure-cli) -tiedoston _Tarkista palvelun pääasiallista salasanalla - Azure CLI_ -osassa tiedoista. Nämä vaiheet Luo uusi palvelu pääasiallista, joka voidaan tarkistamiseen Azure resurssit, kuten HDInsight-klusterin luomiseen käytetyt REST API-pyynnöt.

1. Komentokehote, pääte istunnon tai shell Käytä seuraavaa komentoa Azure tilaukset-luettelo.

        azure account list
        
    Valitse tilaus, jota haluat käyttää, ja tarkista __tunnussarake__ -luettelosta. Tämä on __Tilaustunnus__ ja käytetään yleensä tämän asiakirjan ohjeita.

2. Luo uusi sovellus Azure Active Directoryn.

        azure ad app create --name "exampleapp" --home-page "https://www.contoso.org" --identifier-uris "https://www.contoso.org/example" --password <Your_Password>
        
    Korvaa arvot `--name`, `--home-page`, ja `--identifier-uris` oman arvoilla. Uusi Active Directory-merkinnän salasanaa.
    
    > [AZURE.NOTE] Koska olet luomassa todennusta ja service-lyhennys tästä sovelluksesta `--home-page` ja `--identifier-uris` arvoja ei tarvitse viitata todellinen web-sivun isännöidään Internetissä. niiden on oltava yksilöllinen URI vain.
    
    Palauttaa tiedoista tallentaa __sovelluksen tunnus__ -arvo.
    
        data:    AppId:          4fd39843-c338-417d-b549-a545f584a745
        data:    ObjectId:       4f8ee977-216a-45c1-9fa3-d023089b2962
        data:    DisplayName:    exampleapp
        ...
        info:    ad app create command OK
    
3. Luo pääasiallista käyttämällä __sovelluksen tunnus__ -arvo palauttaa aiemmin palvelu.

        azure ad sp create 4fd39843-c338-417d-b549-a545f584a745
        
     Palauttaa tiedoista Tallenna __Objektitunnus__ arvo.
     
        info:    Executing command ad sp create
        - Creating service principal for application 4fd39843-c338-417d-b549-a545f584a74+
        data:    Object Id:        7dbc8265-51ed-4038-8e13-31948c7f4ce7
        data:    Display Name:     exampleapp
        data:    Service Principal Names:
        data:                      4fd39843-c338-417d-b549-a545f584a745
        data:                      https://www.contoso.org/example
        info:    ad sp create command OK
        
4. Määritä tärkeimmät käyttämällä __Objektitunnus__ -arvo palauttaa aiemmin palvelun __omistajan__ rooli. Sinun on käytettävä myös __Tilaustunnus__ hankit aiemmassa versiossa.
    
        azure role assignment create --objectId 7dbc8265-51ed-4038-8e13-31948c7f4ce7 -o Owner -c /subscriptions/{SubscriptionID}/
        
    Kun toiminto päättyy, pääasiallista palvelu on nyt omistajan käyttöoikeus on määritetty tilauksen.

##<a name="get-an-authentication-token"></a>Todennus-tunnuksen hankkiminen

1. Etsi __Vuokraajan tunnus__ tilauksen seuraavat avulla.

        azure account show -s <subscription ID>
        
    Palauttaa tiedoista Etsi __Vuokraajan tunnus__.
    
        info:    Executing command account show
        data:    Name                        : MyAzureAccount
        data:    ID                          : 45a1014d-0f27-25d2-b838-b8f373d6d52e
        data:    State                       : Enabled
        data:    Tenant ID                   : 22f988bf-56f1-41af-91ab-3d7cd011db47
        data:    Is Default                  : true
        data:    Environment                 : AzureCloud
        data:    Has Certificate             : No
        data:    Has Access Token            : Yes
        data:    User name                   : myname@contoso.org
        data:    
        info:    account show command OK

2. Luo uusi tunnus Azure REST-Ohjelmointirajapinnan käyttäminen.

        curl -X "POST" "https://login.microsoftonline.com/TenantID/oauth2/token" \
        -H "Cookie: flight-uxoptin=true; stsservicecookie=ests; x-ms-gateway-slice=productionb; stsservicecookie=ests" \
        -H "Content-Type: application/x-www-form-urlencoded" \
        --data-urlencode "client_id=AppID" \
        --data-urlencode "grant_type=client_credentials" \
        --data-urlencode "client_secret=password" \
        --data-urlencode "resource=https://management.azure.com/"
    
    Korvaa arvot saadaan tai aikaisemmin __TenantID__, __sovelluksen tunnus__ja __salasana__ .

    Jos tämä pyyntö on tarkistettu, saat 200 sarjan vastauksen ja vastauksen teksti sisältää JSON asiakirjan.

    Pyyntö palauttama JSON-asiakirja sisältää osan nimeltä __access_token__; Tämä elementti arvo käyttöoikeustietue, sinun on käytettävä todennusta käytetään seuraavien osien tämän asiakirjan pyynnöt.
    
        {
            "token_type":"Bearer",
            "expires_in":"3599",
            "expires_on":"1463409994",
            "not_before":"1463406094",
            "resource":"https://management.azure.com/","access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWoNBVGZNNXBPWWlKSE1iYTlnb0VLWSIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiJodHRwczovL21hbmFnZW1lbnQuYXp1cmUuY29tLyIsImlzcyI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI2Ny8iLCJpYXQiOjE0NjM0MDYwOTQsIm5iZiI6MTQ2MzQwNjA5NCwiZXhwIjoxNDYzNDA5OTk5LCJhcHBpZCI6IjBlYzcyMzM0LTZkMDMtNDhmYi04OWU1LTU2NTJiODBiZDliYiIsImFwcGlkYWNyIjoiMSIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0Ny8iLCJvaWQiOiJlNjgxZTZiMi1mZThkLTRkZGUtYjZiMS0xNjAyZDQyNWQzOWYiLCJzdWIiOiJlNjgxZTZiMi1mZThkLTRkZGUtYjZiMS0xNjAyZDQyNWQzOWYiLCJ0aWQiOiI3MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMWRiNDciLCJ2ZXIiOiIxLjAifQ.nJVERbeDHLGHn7ZsbVGBJyHOu2PYhG5dji6F63gu8XN2Cvol3J1HO1uB4H3nCSt9DTu_jMHqAur_NNyobgNM21GojbEZAvd0I9NY0UDumBEvDZfMKneqp7a_cgAU7IYRcTPneSxbD6wo-8gIgfN9KDql98b0uEzixIVIWra2Q1bUUYETYqyaJNdS4RUmlJKNNpENllAyHQLv7hXnap1IuzP-f5CNIbbj9UgXxLiOtW5JhUAwWLZ3-WMhNRpUO2SIB7W7tQ0AbjXw3aUYr7el066J51z5tC1AK9UC-mD_fO_HUP6ZmPzu5gLA6DxkIIYP3grPnRVoUDltHQvwgONDOw"
        }

##<a name="create-a-resource-group"></a>Resurssiryhmän luominen

Luo uusi resurssiryhmä seuraavat avulla. Sinun on luotava ryhmän ensimmäisen kerran, ennen kuin voit luoda resurssit, kuten HDInsight-klusterin. 

* Korvaa __SubscriptionID__ vastaanotettu luotaessa palvelun lyhennys Tilaustunnus.
* Korvaa __AccessToken__ vastaanotettu edellisessä vaiheessa käyttöoikeustietue.
* Korvaa __DataCenterLocation__ haluat luoda resurssiryhmä ja resursseja, tietokeskuksen. Esimerkiksi "Etelä keskitetyn US'. 
* Korvaa __ResourceGroupName__ nimi, jota haluat käyttää ryhmän:

```
curl -X "PUT" "https://management.azure.com/subscriptions/SubscriptionID/resourcegroups/ResourceGroupName?api-version=2015-01-01" \
    -H "Authorization: Bearer AccessToken" \
    -H "Content-Type: application/json" \
    -d $'{
"location": "DataCenterLocation"
}'
```

Jos tämä pyyntö on tarkistettu, saat 200 sarjan vastauksen ja vastauksen teksti sisältää JSON asiakirja, joka sisältää ryhmän tiedot. `"provisioningState"` Elementti sisältää arvon `"Succeeded"`.

##<a name="create-a-deployment"></a>Luo käyttöönoton

Käyttää seuraavaa ottamaan klusterin määrityksissä (malli ja parametrin arvot,) resurssiryhmä.

* Korvaa __SubscriptionID__ ja __AccessToken__ aiemmin käytettävät arvot. 
* Korvaa __ResourceGroupName__ edellisessä osassa luomasi resurssin ryhmän nimeä.
* Korvaa __DeploymentName__ nimi, jota haluat käyttää tätä käyttöönottoa varten.

```
curl -X "PUT" "https://management.azure.com/subscriptions/SubscriptionID/resourcegroups/ResourceGroupName/providers/microsoft.resources/deployments/DeploymentName?api-version=2015-01-01" \
-H "Authorization: Bearer AccessToken" \
-H "Content-Type: application/json" \
-d "{set your body string to the template and parameters}"
```

> [AZURE.NOTE] Jos olet tallentanut JSON-asiakirja, joka sisältää mallin ja parametrit-tiedostoon, voit käyttää sen sijaan, että seuraavat `-d "{ template and parameters}"`:
>
> `--data-binary "@/path/to/file.json"`

Jos tämä pyyntö on tarkistettu, saat 200 sarjan vastauksen ja vastauksen teksti sisältää JSON asiakirja, joka sisältää tietoja käyttöönotto-toimintoa.

> [AZURE.IMPORTANT] Huomaa, että käyttöönoton on lähetetty, mutta ei tällä hetkellä valmistumista. Voi kestää useita minuutteja, yleensä noin 15, viimeistele käyttöönottoa varten.

##<a name="check-the-status-of-a-deployment"></a>Käyttöönoton tilan tarkistaminen

Käyttöönoton tilan tarkistaminen toimimalla seuraavasti:

* Korvaa __SubscriptionID__ ja __AccessToken__ aiemmin käytettävät arvot. 
* Korvaa __ResourceGroupName__ edellisessä osassa luomasi resurssin ryhmän nimeä.

```
curl -X "GET" "https://management.azure.com/subscriptions/SubscriptionID/resourcegroups/ResourceGroupName/providers/microsoft.resources/deployments/DeploymentName?api-version=2015-01-01" \
-H "Authorization: Bearer AccessToken" \
-H "Content-Type: application/json"
```

Tämä voi palauttaa tietoja JSON asiakirja, joka sisältää tietoja käyttöönotto-toimintoa. `"provisioningState"` Elementti sisältää tilan käyttöönoton; Jos tämä arvo on `"Succeeded"`, valitse käyttöönotto onnistui. Tässä vaiheessa yhteyttä klusterin pitäisi olla käytettävissä.

##<a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet luonut HDInsight-klusterin, katso, miten voit käyttää yhteyttä klusterin seuraavat avulla. 

###<a name="hadoop-clusters"></a>Hadoop klustereiden

* [Rakenteen käyttäminen Hdinsightiin](hdinsight-use-hive.md)
* [Possu käyttäminen Hdinsightiin](hdinsight-use-pig.md)
* [MapReduce käyttäminen Hdinsightiin](hdinsight-use-mapreduce.md)

###<a name="hbase-clusters"></a>HBase klustereiden

* [HBase HDInsight-käytön aloittaminen](hdinsight-hbase-tutorial-get-started-linux.md)
* [Kehittää Java HBase HDInsight-sovelluksia](hdinsight-hbase-build-java-maven-linux.md)

###<a name="storm-clusters"></a>Myrsky klustereiden

* [Kehitä Java topologioissa myrsky HDInsight-](hdinsight-storm-develop-java-topology.md)
* [Myrsky HDInsight-Python osien käyttäminen](hdinsight-storm-develop-python-topology.md)
* [Ottaa käyttöön ja topologioissa myrsky HDInsight-ja seuranta](hdinsight-storm-deploy-monitor-topology-linux.md)

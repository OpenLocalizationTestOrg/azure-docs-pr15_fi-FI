<properties
    pageTitle="Käsittely tilan Resurssienhallinta mallien | Microsoft Azure"
    description="Näyttää suositellut tavoista monimutkaisia objekteja avulla voit jakaa tilatiedot Azure Resurssienhallinta mallit ja linkitetyt käyttäminen"
    services="azure-resource-manager"
    documentationCenter=""
    authors="tfitzmac"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="azure-resource-manager"
    ms.workload="multiple"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="tomfitz"/>

# <a name="sharing-state-in-azure-resource-manager-templates"></a>Tilan Azure Resurssienhallinta mallien jakaminen

Tässä ohjeaiheessa esitellään hallinta ja jakaminen malleja tilaan parhaat käytännöt. Parametrien ja muuttujien näkyvät tässä ohjeaiheessa on esimerkkejä objektien, voit määrittää tyypin, järjestele käyttöönoton tarpeen. Näissä esimerkeissä-voit toteuttaa oman objektit, joissa on ominaisuusarvoihin, joka katsoo sen perustelluksi ympäristössä.

Tässä aiheessa on suurempi julkaisu osa. Jos haluat lukea koko paperin, Lataa [maailman luokan resurssien hallinnan mallit huomioon otettavia seikkoja ja osoitettu käytännöt](http://download.microsoft.com/download/8/E/1/8E1DBEFA-CECE-4DC9-A813-93520A5D7CFE/World Class ARM Templates - Considerations and Proven Practices.pdf).


## <a name="provide-standard-configuration-settings"></a>Anna vakio asetukset

Sijaan tarjoaminen mallin, joka on yhteensä joustavampi ja lukemattomien variaatiot tavallisen kuvion on tunnetut käyttömahdollisuudet valintaa. Käyttäjät voivat valita, t-paidan vakiokoosta, kuten eristetyn, pieni, Normaali tai suuri. Muita esimerkkejä t-paidan koot ovat tuotteen palveluja, kuten yhteisön edition- tai enterprise-versio. Muissa tapauksissa kohdalla voi olla työmäärää kielikohtaiset käyttömahdollisuudet tekniikan – kuten kartan pienentäminen tai ei ole sql.

Monimutkaisia objekteja, jossa voit luoda muuttujat, jotka sisältävät tietoja, jota kutsutaan "ominaisuuden pakkaaminen" hallintatyökalua ja aseman resurssin ilmoituksen mallin tietojen avulla. Tämän menetelmän on hyvä, tunnetut käyttömahdollisuudet useita erikokoisia, joka on esimääritetty asiakkaille. Tunnetut käyttömahdollisuudet ilman mallin käyttäjät on selvittää klusterin yhteiskäyttöä koon, ympäristö rajoitteet ottaa huomioon ja laskutoimitusten tunnistavan tuloksena jakaminen tallennustilan tilit ja muut resurssit (vuoksi klusterin koko ja resurssien rajoitukset). Lisäksi tehdä tehostaa käyttökokemusta asiakkaan, muutaman tunnetut määrityksiä on helpompi tukevat ja avulla voit pitää ylemmän tason avulla.

Seuraavassa esimerkissä, voit määrittää, joissa on monimutkaisia objekteja sivustokokoelmat tietojen esittämiseen. Kokoelmien määrittää arvot, joita käytetään virtuaalikoneen kokoa, verkkoasetukset, käyttöjärjestelmän asetukset ja käytettävyyden asetukset.

    "variables": {
      "tshirtSize": "[variables(concat('tshirtSize', parameters('tshirtSize')))]",
      "tshirtSizeSmall": {
        "vmSize": "Standard_A1",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
        "vmCount": 2,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 1,
          "pool": "db",
          "map": [0,0],
          "jumpbox": 0
        }
      },
      "tshirtSizeMedium": {
        "vmSize": "Standard_A3",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-8disk-resources.json')]",
        "vmCount": 2,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 2,
          "pool": "db",
          "map": [0,1],
          "jumpbox": 0
        }
      },
      "tshirtSizeLarge": {
        "vmSize": "Standard_A4",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-16disk-resources.json')]",
        "vmCount": 3,
        "slaveCount": 2,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 2,
          "pool": "db",
          "map": [0,1,1],
          "jumpbox": 0
        }
      },
      "osSettings": {
        "scripts": [
          "[concat(variables('templateBaseUrl'), 'install_postgresql.sh')]",
          "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/shared_scripts/ubuntu/vm-disk-utils-0.1.sh"
        ],
        "imageReference": {
          "publisher": "Canonical",
          "offer": "UbuntuServer",
          "sku": "14.04.2-LTS",
          "version": "latest"
        }
      },
      "networkSettings": {
        "vnetName": "[parameters('virtualNetworkName')]",
        "addressPrefix": "10.0.0.0/16",
        "subnets": {
          "dmz": {
            "name": "dmz",
            "prefix": "10.0.0.0/24",
            "vnet": "[parameters('virtualNetworkName')]"
          },
          "data": {
            "name": "data",
            "prefix": "10.0.1.0/24",
            "vnet": "[parameters('virtualNetworkName')]"
          }
        }
      },
      "availabilitySetSettings": {
        "name": "pgsqlAvailabilitySet",
        "fdCount": 3,
        "udCount": 5
      }
    }

Huomaa, että **tshirtSize** muuttujan yhdistää tekstin **tshirtSize**t-paidan koon antamasi parametrin (**Pieni**, **Normaali**, **suuri**) kautta. Noutaa t-paidan kokoiset liittyvät monimutkaisia objektimuuttuja muuttujan avulla.

Tämän jälkeen voit viitata muuttujia myöhemmin-malli. Voit viitata nimeltä muuttujat ja niiden ominaisuudet yksinkertaistaa mallin syntaksi ja helppoa ymmärtää kontekstissa. Seuraava esimerkki määrittää resurssin ottamaan objektien osoitetulla-arvojen avulla. Esimerkiksi AM kokoa on määritetty arvo hakemalla `variables('tshirtSize').vmSize` aikana arvo, levyn kokoa haetaan `variables('tshirtSize').diskSize`. Lisäksi linkitettyä mallia URI on määritetty arvo `variables('tshirtSize').vmTemplate`.

    "name": "master-node",
    "type": "Microsoft.Resources/deployments",
    "apiVersion": "2015-01-01",
    "dependsOn": [
        "[concat('Microsoft.Resources/deployments/', 'shared')]"
    ],
    "properties": {
        "mode": "Incremental",
        "templateLink": {
          "uri": "[variables('tshirtSize').vmTemplate]",
          "contentVersion": "1.0.0.0"
        },
        "parameters": {
          "adminPassword": {
            "value": "[parameters('adminPassword')]"
          },
          "replicatorPassword": {
            "value": "[parameters('replicatorPassword')]"
          },
          "osSettings": {
            "value": "[variables('osSettings')]"
          },
          "subnet": {
            "value": "[variables('networkSettings').subnets.data]"
          },
          "commonSettings": {
            "value": {
              "region": "[parameters('region')]",
              "adminUsername": "[parameters('adminUsername')]",
              "namespace": "ms"
            }
          },
          "storageSettings": {
            "value":"[variables('tshirtSize').storage]"
          },
          "machineSettings": {
            "value": {
              "vmSize": "[variables('tshirtSize').vmSize]",
              "diskSize": "[variables('tshirtSize').diskSize]",
              "vmCount": 1,
              "availabilitySet": "[variables('availabilitySetSettings').name]"
            }
          },
          "masterIpAddress": {
            "value": "0"
          },
          "dbType": {
            "value": "MASTER"
          }
        }
      }
    }

## <a name="pass-state-to-a-template"></a>Välittää tilan lisääminen malliin

Voit jakaa tilan malliin parametrit, jotka sisältävät suoraan käyttöönoton aikana.

Seuraavassa taulukossa on lueteltu tavallisimmat parametrien malleja.

Nimi | Arvo | Kuvaus
---- | ----- | -----------
sijainti    | Rajoitettu luettelosta Azure alueiden merkkijono   | Sijainti, johon resurssit on otettu käyttöön.
storageAccountNamePrefix    | Merkkijono    | Yksilöivä DNS-nimi AM levyjen sisältävään tallennustilan tilin
Toimialuenimi  | Merkkijono    | Toimialuenimi on kaikkien käytettävissä jumpbox AM muodossa: **{toimialuenimi}. {} sijainti}.cloudapp.com** esimerkiksi: **mydomainname.westus.cloudapp.azure.com**
adminUsername   | Merkkijono    | Käyttäjänimi VMs
adminPassword   | Merkkijono    | Salasanan VMs
tshirtSize  | Merkkijono t-paidan tarjota kokoisille rajoitettu luettelosta   | Nimetty asteikon yksikkö koon valmistelu. Esimerkiksi "Pieni", "Medium", "Suuri"
virtualNetworkName  | Merkkijono    | Virtuaalinen verkko, jossa kuluttaja haluaa käyttää nimi.
enableJumpbox   | Merkkijono rajoitettu luettelosta (käytössä/poissa käytöstä) | Parametri, joka osoittaa, otetaanko jumpbox ympäristössä. Arvot: "käytössä", "käytöstä"

Edellisessä osassa käytetyn **tshirtSize** parametri määritetään seuraavasti:

    "parameters": {
      "tshirtSize": {
        "type": "string",
        "defaultValue": "Small",
        "allowedValues": [
          "Small",
          "Medium",
          "Large"
        ],
        "metadata": {
          "Description": "T-shirt size of the MongoDB deployment"
        }
      }
    }


## <a name="pass-state-to-linked-templates"></a>Välittää tilan linkitetyt mallit

Linkitetyt mallit yhdistettäessä voit Käytä staattinen usein ja muuttujien luodaan.

### <a name="static-variables"></a>Staattinen muuttujat

Staattinen muuttujaa käytetään usein antamaan perusarvot, kuten URL-osoitteet, koko mallin käytetyt.

Seuraavat malli-ote- `templateBaseUrl` määrittää pääkansion sijainti mallin GitHub. Seuraavalle riville muodostaa uusi muuttuja `sharedTemplateUrl` , joka yhdistää Perusosoitteen tunnetut jaettuja resursseja mallin nimi. Kyseisen rivin alapuolella monimutkaisia objektimuuttuja käytetään tallentamiseen t-paidan koko, jossa Perusosoitteen yhdistetään kokoonpanon mallin sijainti ja tallennetaan `vmTemplate` ominaisuus.

Tämän menetelmän etuna on se, että jos mallin sijainti muuttuu vain haluat muuttaa yhdessä paikassa, joka välittää linkitetyn malleja koko staattinen muuttuja.

    "variables": {
      "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/postgresql-on-ubuntu/",
      "sharedTemplateUrl": "[concat(variables('templateBaseUrl'), 'shared-resources.json')]",
      "tshirtSizeSmall": {
        "vmSize": "Standard_A1",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
        "vmCount": 2,
        "slaveCount": 1,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 1,
          "pool": "db",
          "map": [0,0],
          "jumpbox": 0
        }
      }
    }

### <a name="generated-variables"></a>Luotu muuttujat

Staattisten muuttujien lisäksi useita muuttujia luodaan dynaamisesti. Tässä osassa tunnistaa joitakin yleisiä liitetiedostotyyppejä luotu muuttujien.

#### <a name="tshirtsize"></a>tshirtSize

Olet aiemmin luotu muuttuja yllä käytettyä.

#### <a name="networksettings"></a>networkSettings

Kapasiteettia, ominaisuuksien tai kohdistetuissa lopusta loppuun-ratkaisun mallin linkitetty mallit Luo yleensä siellä olevia resursseja verkossa. Yksi selkeä tapa on monimutkaisia objektin avulla voit tallentaa verkkoasetukset ja välittää niitä linkitetyt mallit.

Esimerkki viestintä verkkoasetukset näkyvät jäljempänä.

    "networkSettings": {
      "vnetName": "[parameters('virtualNetworkName')]",
      "addressPrefix": "10.0.0.0/16",
      "subnets": {
        "dmz": {
          "name": "dmz",
          "prefix": "10.0.0.0/24",
          "vnet": "[parameters('virtualNetworkName')]"
        },
        "data": {
          "name": "data",
          "prefix": "10.0.1.0/24",
          "vnet": "[parameters('virtualNetworkName')]"
        }
      }
    }

#### <a name="availabilitysettings"></a>availabilitySettings

Resurssit linkitetyt mallit luodaan sijoitetaan usein käytettävyys määrittäminen. Seuraavassa esimerkissä käytettävyys joukon nimi on määritetty ja myös vian toimialueen ja Päivitä toimialueen laskeminen käyttämällä.

    "availabilitySetSettings": {
      "name": "pgsqlAvailabilitySet",
      "fdCount": 3,
      "udCount": 5
    }

Jos tarvitset tietoja solmujen käytettävyys useita ehtojoukkoja (esimerkiksi toinen pää solmujen) ja toinen, voit käyttää nimi etuliitteenä, Määritä käytettävyys useita ehtojoukkoja tai noudattamalla luomisen t-paidan koon muuttujan aiemmin mallin.

#### <a name="storagesettings"></a>storageSettings

Tallennustilan tiedot jaetaan usein linkitetyn malleja. Alla olevassa esimerkissä *storageSettings* -objekti on lisätietoja tallennustilan tilin ja säilö nimet.

    "storageSettings": {
        "vhdStorageAccountName": "[parameters('storageAccountName')]",
        "vhdContainerName": "[variables('vmStorageAccountContainerName')]",
        "destinationVhdsContainer": "[concat('https://', parameters('storageAccountName'), variables('vmStorageAccountDomain'), '/', variables('vmStorageAccountContainerName'), '/')]"
    }

#### <a name="ossettings"></a>osSettings

Linkitetyn malleja voit joutua käyttöjärjestelmän asetukset siirtää solmujen erilaiset eri kokoonpanon välillä. Monimutkainen objekti on helppo tapa tallentaa ja jakaa käyttöjärjestelmätietoja, ja myös on helppo tukemaan useita käyttöjärjestelmän vaihtoehtoja käyttöönottoa varten.

Seuraavassa esimerkissä esitetään *osSettings*objektia:

    "osSettings": {
      "imageReference": {
        "publisher": "Canonical",
        "offer": "UbuntuServer",
        "sku": "14.04.2-LTS",
        "version": "latest"
      }
    }

#### <a name="machinesettings"></a>machineSettings

Luotu muuttujan *machineSettings* on monimutkaisia objektit, joiden mix core muuttujien luomiseen AM. Muuttujat ovat järjestelmänvalvojan käyttäjänimi ja salasana, etuliitteen AM nimet ja käyttöjärjestelmän kuva on viittaus.

    "machineSettings": {
        "adminUsername": "[parameters('adminUsername')]",
        "adminPassword": "[parameters('adminPassword')]",
        "machineNamePrefix": "mongodb-",
        "osImageReference": {
            "publisher": "[variables('osFamilySpec').imagePublisher]",
            "offer": "[variables('osFamilySpec').imageOffer]",
            "sku": "[variables('osFamilySpec').imageSKU]",
            "version": "latest"
        }
    },

Huomaa, että *osImageReference* hakee arvoja tärkeimmät mallissa määritettyjä *osSettings* muuttuja. Tämä tarkoittaa sitä, voit helposti muuttaa käyttöjärjestelmän AM – kokonaan tai mallin kuluttaja preference perusteella.

#### <a name="vmscripts"></a>vmScripts

*VmScripts* objekti sisältää komentosarjoja, lataa ja suorita AM-esiintymässä, mukaan lukien ulkopuolella ja viittaukset sisällä tietoja. Ulkopuolella sisältyy infrastruktuuri. Sisä sisältyy asennettujen ohjelmien asennettu ja määritykset.

Luettelo komentosarjat AM lataamaan *scriptsToDownload* -ominaisuuden avulla. Tämä objekti sisältää myös erityyppisiä toimia komentorivin argumentit viittauksina. Nämä toiminnot ovat suoritetaan kunkin yksittäisiä solmun perusasennus, asennus, joka suoritetaan, kun kaikki solmut on otettu käyttöön ja muita komentosarjat, jotka voivat olla tiettyyn malliin.

Tässä esimerkissä on käytettävä käyttöön MongoDB, jossa pyydetään arbiter aikana suuren käytettävyyden mallista. Voit asentaa arbiter *vmScripts* on lisätty *arbiterNodeInstallCommand* .

Muuttujat-osa on missä muuttujat, jotka määrittävät tietyn tekstin suorittamaan komentosarja ERISNIMI-arvoilla.

    "vmScripts": {
        "scriptsToDownload": [
            "[concat(variables('scriptUrl'), 'mongodb-', variables('osFamilySpec').osName, '-install.sh')]",
            "[concat(variables('sharedScriptUrl'), 'vm-disk-utils-0.1.sh')]"
        ],
        "regularNodeInstallCommand": "[variables('installCommand')]",
        "lastNodeInstallCommand": "[concat(variables('installCommand'), ' -l')]",
        "arbiterNodeInstallCommand": "[concat(variables('installCommand'), ' -a')]"
    },


## <a name="return-state-from-a-template"></a>Palauttaa valtion mallista

Ei vain voit siirtää tietoja malliin, voit myös jakaa tiedot takaisin puheluja mallia. **Tulostaa** osan ja linkitettyä mallia voit antaa avain/arvo-pareina, joka on käytetty lähde-malli.

Seuraavassa esimerkissä esitetään, miten voit välittää yksityinen IP-osoite luodaan linkitettyä mallia.

    "outputs": {
        "masterip": {
            "value": "[reference(concat(variables('nicName'),0)).ipConfigurations[0].properties.privateIPAddress]",
            "type": "string"
         }
    }

Tärkeimmät mallin sisällä tietoja voi käyttää seuraavaa syntaksia:

    "[reference('master-node').outputs.masterip.value]"

Voit käyttää tätä lauseketta tulostaa osan tai tärkeimmät mallin resurssit-osa. Et voi käyttää lauseke muuttujat-osassa, koska se on riippuvainen suorituksenaikainen tila. Palautettava arvo tärkeimmät mallin avulla:

    "outputs": { 
      "masterIpAddress": {
        "value": "[reference('master-node').outputs.masterip.value]",
        "type": "string"
      }
     
Esimerkki linkitettyä mallia tulostus-osa, jossa käytetään palauttamaan tietoja levyjen virtual machine on artikkelissa [luominen useita tietojen levyjen Virtual Machine](resource-group-create-multiple.md#creating-multiple-data-disks-for-a-virtual-machine).

## <a name="define-authentication-settings-for-virtual-machine"></a>Määritä virtuaalikoneen todennusasetukset

Voit määrittää virtual machine todennusasetukset osoitetulla määritysasetukset samoissa. Voit luoda kulkee parametri todennuksen lajin.

    "parameters": {
      "authenticationType": {
        "allowedValues": [
          "password",
          "sshPublicKey"
        ],
        "defaultValue": "password",
        "metadata": {
          "description": "Authentication type"
        },
        "type": "string"
      }
    }

Voit lisätä eri todennustyypit muuttujat ja muuttujan tallentamiseen tyyppi käytetään käyttöönottoon parametrin arvon perusteella.

    "variables": {
      "osProfile": "[variables(concat('osProfile', parameters('authenticationType')))]",
      "osProfilepassword": {
        "adminPassword": "[parameters('adminPassword')]",
        "adminUsername": "notused",
        "computerName": "[parameters('vmName')]",
        "customData": "[base64(variables('customData'))]"
      },
      "osProfilesshPublicKey": {
        "adminUsername": "notused",
        "computerName": "[parameters('vmName')]",
        "customData": "[base64(variables('customData'))]",
        "linuxConfiguration": {
          "disablePasswordAuthentication": "true",
          "ssh": {
            "publicKeys": [
              {
                "keyData": "[parameters('sshPublicKey')]",
                "path": "/home/notused/.ssh/authorized_keys"
              }
            ]
          }
        }
      }
    }

Kun määrität virtuaalikoneen, **osProfile** arvoksi luomasi muuttujan.

    {
      "type": "Microsoft.Compute/virtualMachines",
      ...
      "osProfile": "[variables('osProfile')]"
    }


## <a name="next-steps"></a>Seuraavat vaiheet
- Lisätietoja mallin osat artikkelissa [Yhtä aikaa muiden kanssa Azure Resurssienhallinta-mallit](resource-group-authoring-templates.md)
- Toiminnot, jotka ovat käytettävissä mallin on ohjeartikkelissa [Azure Resurssienhallinta malli-Funktiot](resource-group-template-functions.md)


<properties
   pageTitle="Mukautetun komentosarjojen Linux VMs | Microsoft Azure"
   description="Mukautettu komentosarja-tunniste käyttämällä Linux AM määritysten tehtävien automatisointi"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="neilpeterson"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="09/22/2016"
   ms.author="nepeters"/>

# <a name="using-the-azure-custom-script-extension-with-linux-virtual-machines"></a>Azure mukautettu komentosarja-tunniste käyttäminen Linux näennäiskoneiden

Mukautettu komentosarja-tunniste-lataukset ja suorittaa komentosarjoja Azuren näennäiskoneiden. Tämä tunniste on hyötyä kirjaa käyttöönoton määritykset, Ohjelmistoasennus tai muiden asetusten ja tehtävien hallinta. Komentosarjojen voit ladata Azure tallennustilan tai muita käytettävissä internet-sijainti tai annettu suoritusaika-laajennus. Mukautettu komentosarja-tunniste integroituu Azure Resurssienhallinta mallit ja voi suorittaa Azure-CLI, PowerShell, Azure portal tai Azure virtuaalikoneen REST-Ohjelmointirajapinnalla avulla.

Tämä asiakirja on esitetty, kuinka Azure-CLI ja Azure Resurssienhallinta-mallin mukautettu komentosarja-tunniste ja tiedot myös vianmääritysohjeita Linux järjestelmiin.

## <a name="extension-configuration"></a>Tunniste-määritys

Mukautettu komentosarja-tunniste-määritys määrittää kohteita, kuten komentosarjasijainti ja voi suorittaa komennon. Tässä määrityksessä voidaan tallentaa määritysten tiedostot komentorivillä tai Azure Resurssienhallinta-mallissa. Luottamuksellisia tietoja voidaan tallentaa suojatun määritys, joka salataan ja vain purkaa virtuaalikoneen sisällä. Suojatun määritys on hyödyllinen, kun suorittamisen-komento sisältää tietoja, kuten salasanan.

### <a name="public-configuration"></a>Julkinen määritys

Rakenne:

- **commandToExecute**: (pakollinen-merkkijono) tapahtuma pisteen komentosarjan suorittaminen
- **fileUris**: (valinnainen; merkkijonon matriisi) tiedostojen lataamisen URL-osoitteen.
- **aikaleima** (valinnainen; kokonaisluku) tätä kenttää käytetään vain, jos haluat käynnistää Suorita komentosarjan muuttamalla kentän arvo.

```none
{
  "fileUris": ["<url>"],
  "commandToExecute": "<command-to-execute>"
}
```

### <a name="protected-configuration"></a>Suojattu määritys

Rakenne:

- **commandToExecute**: (valinnainen; merkkijonon) tapahtuma pisteen komentosarjan suorittaminen. Käytä tätä kenttää sen sijaan, jos komento sisältää tietoja, kuten salasanat.
- **storageAccountName**: (valinnainen; merkkijonon) tallennustilan tilin nimi. Jos määrität tallennustilan tunnistetiedot, kaikki fileUris on oltava Azure-BLOB URL-osoitteet.
- **storageAccountKey**: (valinnainen; merkkijonon) tallennustilan tilin pikanäppäin.


```json
{
  "commandToExecute": "<command-to-execute>",
  "storageAccountName": "<storage-account-name>",
  "storageAccountKey": "<storage-account-key>"
}
```

## <a name="azure-cli"></a>Azure CLI

Kun Azure-CLI avulla voit suorittaa mukautettu komentosarja-tunniste, luoda määritystiedoston tai tiedostot, jotka sisältävät vähintään tiedoston uri ja komentosarjojen suorittamisen-komento.

```none
azure vm extension set <resource-group> <vm-name> CustomScript Microsoft.Azure.Extensions 2.0 --auto-upgrade-minor-version --public-config-path /scirpt-config.json
```

Voit myös komento voidaan suorittaa avulla `--public-config` ja `--private-config` vaihtoehto, joka mahdollistaa suorituksen aikana ja ilman erillistä kokoonpanotiedosto määrittämisen.

```none
azure vm extension set <resource-group> <vm-name> CustomScript Microsoft.Azure.Extensions 2.0 --auto-upgrade-minor-version --public-config '{"fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"],"commandToExecute": "./hello.sh"}'
```

### <a name="azure-cli-examples"></a>Azure CLI esimerkkejä

**Esimerkki 1** - komentosarjatiedosto julkisen kokoonpanon.

```json
{
  "fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"],
  "commandToExecute": "./hello.sh"
}
```

Azure CLI-komento:

```none
azure vm extension set <resource-group> <vm-name> CustomScript Microsoft.Azure.Extensions 2.0 --auto-upgrade-minor-version --public-config-path /public.json
```

**Esimerkki 2** - komentosarjatiedostoa ei ole julkisten kokoonpanon.

```json
{
  "commandToExecute": "apt-get -y update && apt-get install -y apache2"
}
```

Azure CLI-komento:

```none
azure vm extension set <resource-group> <vm-name> CustomScript Microsoft.Azure.Extensions 2.0 --auto-upgrade-minor-version --public-config-path /public.json
```

**Esimerkki 3** – julkisen kokoonpanotiedosto käytetään komentosarjatiedosto URI ja suojatun kokoonpanotiedosto käytetään komennon, joka suoritetaan.

Julkinen kokoonpanotiedosto:

```json
{
  "fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"],
}
```

Suojatun kokoonpanotiedosto:  

```json
{
  "commandToExecute": "./hello.sh <password>"
}
```

Azure CLI-komento:

```none
azure vm extension set <resource-group> <vm-name> CustomScript Microsoft.Azure.Extensions 2.0 --auto-upgrade-minor-version --public-config-path ./public.json --private-config-path ./protected.json
```

## <a name="resource-manager-template"></a>Resurssienhallinta-malli

Azure mukautettu komentosarja-tunniste voidaan suorittaa milloin virtuaalikoneen käyttöönoton Resurssienhallinta mallin avulla. Tee Lisää oikein muotoiltu JSON käyttöönotto-malliin.

### <a name="resource-manager-examples"></a>Resurssienhallinta-esimerkkejä

**Esimerkki 1** – julkisen määritys.

```json
{
    "name": "scriptextensiondemo",
    "type": "extensions",
    "location": "[resourceGroup().location]",
    "apiVersion": "2015-06-15",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', parameters('scriptextensiondemoName'))]"
    ],
    "tags": {
        "displayName": "scriptextensiondemo"
    },
    "properties": {
        "publisher": "Microsoft.Azure.Extensions",
        "type": "CustomScript",
        "typeHandlerVersion": "2.0",
        "autoUpgradeMinorVersion": true,
      "settings": {
        "fileUris": [
          "https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"
        ],
        "commandToExecute": "sh hello.sh"
      }
    }
}
```

**Esimerkki 2** - suojatun määritysten suorittamisen-komento.

```json
{
  "name": "config-app",
  "type": "extensions",
  "location": "[resourceGroup().location]",
  "apiVersion": "2015-06-15",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh
      ]              
    },
    "protectedSettings": {
      "commandToExecute": "sh hello.sh <password>"
    }
  }
}
```

Katso .net Core musiikkia kaupan esittely täydellinen esimerkki – [Musiikkia kaupan esittely](https://github.com/neilpeterson/nepeters-azure-templates/tree/master/dotnet-core-music-linux-vm-sql-db).

## <a name="troubleshooting"></a>Vianmääritys

Mukautettu komentosarja-tunniste suoritetaan, kun komentosarja luodaan tai ladataan samalla tavalla kuin seuraavassa esimerkissä hakemistoon. Komento-tuloste tallennetaan myös tähän kansioon `stdout` ja `stderr` tiedosto.

```none
/var/lib/azure/custom-script/download/0/
```

Azure-komentosarjan tunniste tuottaa loki, johon täällä.

```none
/var/log/azure/customscript/handler.log
```

Mukautettu komentosarja-tunniste suorittamisen tilan voi hakea myös Azure-CLI.

```none
azure vm extension get <resource-group> <vm-name>
```

Tulos näyttää seuraavan tekstin:

```none
info:    Executing command vm extension get
+ Looking up the VM "scripttst001"
data:    Publisher                   Name                                      Version  State
data:    --------------------------  ----------------------------------------  -------  ---------
data:    Microsoft.Azure.Extensions  CustomScript                              2.0      Succeeded
data:    Microsoft.OSTCExtensions    Microsoft.Insights.VMDiagnosticsSettings  2.3      Succeeded
info:    vm extension get command OK
```

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja muiden AM komentosarja-laajennukset on ohjeaiheessa [Linux yleistä Azure komentosarjan tunniste](./virtual-machines-linux-extensions-features.md).
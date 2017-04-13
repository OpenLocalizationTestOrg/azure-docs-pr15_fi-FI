<properties
   pageTitle="Haluttu tilan resurssien hallinnan malli | Microsoft Azure"
   description="Resurssienhallinta Tietomallin määritys toivottuja tilan kokoonpanon Azure esimerkkien ja vianmääritys"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="zjalexander"
   manager="timlt"
   editor=""
   tags="azure-service-management,azure-resource-manager"
   keywords=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="na"
   ms.date="09/15/2016"
   ms.author="zachal"/>

# <a name="windows-vmss-and-desired-state-configuration-with-azure-resource-manager-templates"></a>Windows-VMSS ja haluttu tilan määritys Azure Resurssienhallinta malleja
Tässä artikkelissa kuvataan [toivottuja tilan määritys tunniste käsittelijä](virtual-machines-windows-extensions-dsc-overview.md)Resurssienhallinta mallia. 

## <a name="template-example-for-a-windows-vm"></a>Windows-AM malli-esimerkki

Seuraavat koodikatkelman siirtyy mallin resurssi-osa.

```json
            "name": "Microsoft.Powershell.DSC",
            "type": "extensions",
             "location": "[resourceGroup().location]",
             "apiVersion": "2015-06-15",
             "dependsOn": [
                  "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
              ],
              "properties": {
                  "publisher": "Microsoft.Powershell",
                  "type": "DSC",
                  "typeHandlerVersion": "2.20",
                  "autoUpgradeMinorVersion": true,
                  "forceUpdateTag": "[parameters('dscExtensionUpdateTagVersion')]",
                  "settings": {
                      "configuration": {
                          "url": "[concat(parameters('_artifactsLocation'), '/', variables('dscExtensionArchiveFolder'), '/', variables('dscExtensionArchiveFileName'))]",
                          "script": "dscExtension.ps1",
                          "function": "Main"
                      },
                      "configurationArguments": {
                          "nodeName": "[variables('vmName')]"
                      }
                  },
                  "protectedSettings": {
                      "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                  }

```

## <a name="template-example-for-windows-vmss"></a>Windowsin VMSS malli-esimerkki

VMSS solmu on "ominaisuudet-osan"VirtualMachineProfile","extensionProfile"-määrite. DSC lisätään kohtaan "laajennukset". 

```json
"extensionProfile": {
            "extensions": [
                {
                    "name": "Microsoft.Powershell.DSC",
                    "properties": {
                        "publisher": "Microsoft.Powershell",
                        "type": "DSC",
                        "typeHandlerVersion": "2.20",
                        "autoUpgradeMinorVersion": true,
                        "forceUpdateTag": "[parameters('DscExtensionUpdateTagVersion')]",
                        "settings": {
                            "configuration": {
                                "url": "[concat(parameters('_artifactsLocation'), '/', variables('DscExtensionArchiveFolder'), '/', variables('DscExtensionArchiveFileName'))]",
                                "script": "DscExtension.ps1",
                                "function": "Main"
                            },
                            "configurationArguments": {
                                "nodeName": "localhost"
                            }
                        },
                        "protectedSettings": {
                            "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                        }
                    }
                }
            ]
```

## <a name="detailed-settings-information"></a>Asetusten yksityiskohtaiset tiedot

Seuraavat rakenne on Azure Resurssienhallinta-mallin Azure DSC laajennuksen asetukset osiolle.

```json

"settings": {
  "wmfVersion": "latest",
  "configuration": {
    "url": "http://validURLToConfigLocation",
    "script": "ConfigurationScript.ps1",
    "function": "ConfigurationFunction"
  },
  "configurationArguments": {
    "argument1": "Value1",
    "argument2": "Value2"
  },
  "configurationData": {
    "url": "https://foo.psd1"
  },
  "privacy": {
    "dataCollection": "enable"
  },
  "advancedOptions": {
    "downloadMappings": {
      "customWmfLocation": "http://myWMFlocation"
    }
  } 
},
"protectedSettings": {
  "configurationArguments": {
    "parameterOfTypePSCredential1": {
      "userName": "UsernameValue1",
      "password": "PasswordValue1"
    },
    "parameterOfTypePSCredential2": {
      "userName": "UsernameValue2",
      "password": "PasswordValue2"
    }
  },
  "configurationUrlSasToken": "?g!bber1sht0k3n",
  "configurationDataUrlSasToken": "?dataAcC355T0k3N"
}

```

## <a name="details"></a>Tiedot
| Ominaisuuden nimi | Tyyppi | Kuvaus |
| --- | --- | --- |
| settings.wmfVersion | merkkijono | Määrittää, jotka asennetaan oman AM Windows Management Framework version. Asetukset tämän ominaisuuden uusimman' asennuksia WMF viimeksi päivitetty versio. Tämän ominaisuuden vain nykyisen mahdolliset arvot ovat **'4.0', '5.0', ' 5.0PP' ja 'uusimmat'**. Mahdolliset arvot ovat veloittaa päivitykset. Oletusarvo on uusin".|
| Settings.Configuration.URL | merkkijono | Määrittää, josta voit ladata DSC määritysten zip-tiedoston URL-osoitteen. Jos annettu URL-osoite edellyttää SAS tunnuksen käytön, sinun on protectedSettings.configurationUrlSasToken ominaisuuden arvon SAS-tunnuksen. Tämä ominaisuus on pakollinen, jos settings.configuration.script ja/tai settings.configuration.function ole määritetty. |
| Settings.Configuration.Script | merkkijono | Määrittää komentosarjan, joka sisältää DSC kokoonpanon määrityksessä nimi. Tämä komentosarja on oltava zip-tiedoston URL-osoite configuration.url-ominaisuudessa määritetyn ladattujen pääkansioon. Tämä ominaisuus on pakollinen, jos settings.configuration.url ja/tai settings.configuration.script ole määritetty. |
| Settings.Configuration.Function | merkkijono | Määrittää nimen DSC-määritys. Nimetty määritysten täytyy sisältyä configuration.script määrittämiä komentosarja. Tämä ominaisuus on pakollinen, jos settings.configuration.url ja/tai settings.configuration.function ole määritetty. |
| settings.configurationArguments | Sivustokokoelman | Määrittää parametreja haluat siirtää DSC-määrityksiin. Tämä ominaisuus ei ole salattu. |
| settings.configurationData.url | merkkijono | Määrittää URL-osoite, joista voit ladata tiedot (.pds1) kokoonpanotiedosto syötteeksi DSC kokoonpanon. Jos annettu URL-osoite edellyttää SAS tunnuksen käytön, sinun on protectedSettings.configurationDataUrlSasToken ominaisuuden arvon SAS-tunnuksen.|
| settings.privacy.dataEnabled | merkkijono | Ottaa käyttöön tai poistaa telemetriatietojen sivustokokoelman. Tämän ominaisuuden vain mahdolliset arvot ovat **"Ota käyttöön", "Poista käytöstä", ", tai $null**. Telemetriatietojen mahdollistaa jätä tämän ominaisuuden tyhjä tai null. Oletusarvo on ". [Lisätietoja](https://blogs.msdn.microsoft.com/powershell/2016/02/02/azure-dsc-extension-data-collection-2/) |
| settings.advancedOptions.downloadMappings | Sivustokokoelman | Määrittää vaihtoehtoiset sijainnit, joista voit ladata WMF. [Lisätietoja](http://blogs.msdn.com/b/powershell/archive/2015/10/21/azure-dsc-extension-2-2-amp-how-to-map-downloads-of-the-extension-dependencies-to-your-own-location.aspx) |
| protectedSettings.configurationArguments | Sivustokokoelman | Määrittää parametreja haluat siirtää DSC-määrityksiin. Tämä ominaisuus on salattu. |
| protectedSettings.configurationUrlSasToken | merkkijono | Määrittää SAS tunnuksen configuration.url määrittämän URL-osoitteen. Tämä ominaisuus on salattu. |
| protectedSettings.configurationDataUrlSasToken | merkkijono | Määrittää SAS tunnuksen configurationData.url määrittämän URL-osoitteen. Tämä ominaisuus on salattu. |

## <a name="settings-vs-protectedsettings"></a>Asetukset ja ProtectedSettings
Kaikki asetukset tallennetaan AM asetukset tekstitiedosto.
Valitse "asetukset" ominaisuudet eivät yleisten ominaisuuksien, koska niitä ei ole salattu asetukset tekstitiedostoon.
Ominaisuudet: "protectedSettings"-kohdassa sertifikaatilla salataan ja eivät näy vain teksti AM tässä tiedostossa.

Jos kokoonpano on tunnistetiedot, ne voidaan lisätä protectedSettings:

```json
"protectedSettings": {
    "configurationArguments": {
        "parameterOfTypePSCredential1": {
            "userName": "UsernameValue1",
            "password": "PasswordValue1"
        }
    }
}
```

## <a name="example"></a>Esimerkki

Seuraavassa esimerkissä johdetaan "Aloittaminen" osa [DSC tunniste käsittelijä yhteenveto-sivu](virtual-machines-windows-extensions-dsc-overview.md).
Tässä esimerkissä Resurssienhallinta malleja cmdlet-komentojen sijaan ottamaan tunniste. Tallenna "IisInstall.ps1"-määritys, sijoita se:. ZIP-tiedosto ja lataa tiedosto voi käyttää URL-osoitteeseen. Tässä esimerkissä käytetään Azure-blob-säiliö, mutta ei ladata. ZIP-tiedostot haluamaansa milloin.

Seuraava koodi ohjaa oikean tiedoston lataamisen ja suorittamisen sopivan PowerShell-funktion AM Azure Resurssienhallinta-mallissa:

```json
"settings": {
    "configuration": {
        "url": "https://demo.blob.core.windows.net/",
        "script": "IisInstall.ps1",
        "function": "IISInstall"
    }
    } 
},
"protectedSettings": {
    "configurationUrlSasToken": "odLPL/U1p9lvcnp..."
}
```

## <a name="updating-from-the-previous-format"></a>Päivittää vanhemmassa tiedostomuodossa
Edellinen-muodossa (jossa julkiset ominaisuudet ModulesUrl, ConfigurationFunction, SasToken tai ominaisuudet) automaattisesti asetuksia nykyisessä tiedostomuodossa sopeuduttava ja suorittaa vain ennen samalla tavalla.

Seuraavassa rakenteessa on mitä edellisen asetukset rakenteen näytti:

```json
"settings": {
    "WMFVersion": "latest",
    "ModulesUrl": "https://UrlToZipContainingConfigurationScript.ps1.zip",
    "SasToken": "SAS Token if ModulesUrl points to private Azure Blob Storage",
    "ConfigurationFunction": "ConfigurationScript.ps1\\ConfigurationFunction",
    "Properties":  {
        "ParameterToConfigurationFunction1":  "Value1",
        "ParameterToConfigurationFunction2":  "Value2",
        "ParameterOfTypePSCredential1": {
            "UserName": "UsernameValue1",
            "Password": "PrivateSettingsRef:Key1" 
        },
        "ParameterOfTypePSCredential2": {
            "UserName": "UsernameValue2",
            "Password": "PrivateSettingsRef:Key2"
        }
    }
},
"protectedSettings": { 
    "Items": {
        "Key1": "PasswordValue1",
        "Key2": "PasswordValue2"
    },
    "DataBlobUri": "https://UrlToConfigurationDataWithOptionalSasToken.psd1"
}
```

Näin miten vanhemmassa tiedostomuodossa sopeutuu nykyiseen muotoon:

| Ominaisuuden nimi | Edellisen mallin vastine |
| --- | --- |
| settings.wmfVersion | asetukset. WMFVersion |
| Settings.Configuration.URL | asetukset. ModulesUrl |
| Settings.Configuration.Script | Ensimmäinen osa asetukset. ConfigurationFunction (ennen '\\\\") |
| Settings.Configuration.Function | Toisen osan asetuksia. ConfigurationFunction (kun '\\\\") |
| settings.configurationArguments | asetukset. Ominaisuudet: |
| settings.configurationData.url | protectedSettings.DataBlobUri (ilman SAS tunnus) |
| settings.privacy.dataEnabled | asetukset. Privacy.DataEnabled |
| settings.advancedOptions.downloadMappings | asetukset. AdvancedOptions.DownloadMappings |
| protectedSettings.configurationArguments | protectedSettings.Properties |
| protectedSettings.configurationUrlSasToken | asetukset. SasToken |
| protectedSettings.configurationDataUrlSasToken | SAS tunnuksen protectedSettings.DataBlobUri kohteesta |


## <a name="troubleshooting---error-code-1100"></a>Vianmääritys – virhekoodi 1100
Virhekoodi 1100 ilmaisee, että ongelma syöttötapa DSC-laajennus.
Nämä virheet teksti on muuttuja ja voivat muuttua.
Seuraavassa on joitakin virheitä, joita saattaa tulla ja miten voit korjata ne.

### <a name="invalid-values"></a>Virheellisiä arvoja
"Privacy.dataCollection on {0}. Vain mahdolliset arvot ovat ","Ota käyttöön"ja"Käytöstä"" "WmfVersion on {0}. Mahdolliset arvot ovat... ja 'uusimmat' "

Ongelma: Annettua arvoa ei voi suorittaa.

Ratkaisu: Muuta virheellisen arvon kelvollinen arvo. Katso tiedot-kohdassa taulukko.

### <a name="invalid-url"></a>Virheellinen URL-osoite
"ConfigurationData.url on {0}. Tämä ei ole kelvollinen URL-osoite""DataBlobUri on {0}. Tämä ei ole kelvollinen URL-osoite""Configuration.url on {0}. Tämä ei ole kelvollinen URL-osoite"

Ongelma: A annettu URL-osoite ei ole kelvollinen.

Ratkaisu: Tarkista kaikki annettu URL. Varmista, että kaikki URL-osoitteet ratkaista kelvollinen sijainteihin tunnisteen käyttöoikeus etätietokoneessa.

### <a name="invalid-configurationargument-type"></a>Virheellinen ConfigurationArgument tyyppi
"Virheellinen configurationArguments Kirjoita {0}"

Ongelma: ConfigurationArguments-ominaisuutta ei voi ratkaista hajautustaulukko-objekti. 

Ratkaisu: Varmista ConfigurationArguments-ominaisuuden Hajautustaulukkoa. Noudata annettu kuin edellä olevissa esimerkissä muotoa. Tarkkaile tarjoukset, välimerkit ja aaltosulkeet.

### <a name="duplicate-configurationarguments"></a>Kaksoiskappaleiden ConfigurationArguments
"Löytyvät sekä julkinen ja suojatun configurationArguments kaksoiskappaleiden argumentit {0}"

Ongelma: ConfigurationArguments julkisen asetukset- ja suojatun asetuksissa ConfigurationArguments sisältää ominaisuuksia, jolla on sama nimi.

Ratkaisu: Poista yksi kaksoiskappaleiden ominaisuuksista.

### <a name="missing-properties"></a>Puuttuvat ominaisuudet
"Configuration.function edellyttää, että configuration.url tai configuration.module on määritetty"

"Configuration.url edellyttää, että configuration.script on määritetty"

"Configuration.script edellyttää, että configuration.url on määritetty"

"Configuration.url edellyttää, että configuration.function on määritetty"

"ConfigurationUrlSasToken edellyttää, että configuration.url on määritetty"

"ConfigurationDataUrlSasToken edellyttää, että configurationData.url on määritetty"

Ongelma: Määritetty ominaisuus on toinen ominaisuus, joka ei ole.

Ratkaisuja: 
- Anna ominaisuus puuttuu.
- Poista ominaisuus, joka on ominaisuus puuttuu.


## <a name="next-steps"></a>Seuraavat vaiheet
DSC tietoja ja virtuaalikoneen asteikko määrittää [Virtuaalikoneen-asteikon asettaa käyttämällä Azure DSC-tunniste](../virtual-machine-scale-sets/virtual-machine-scale-sets-dsc.md)

Lisätietoja etsiminen [DSC on suojattu tunnistetietojen hallinta](virtual-machines-windows-extensions-dsc-credentials.md). 

Lisätietoja Azure DSC tunniste käsittelijä on artikkelissa [Johdanto Azure toivottuja tilan määritys tunniste käsittelyn](virtual-machines-windows-extensions-dsc-overview.md). 

Lisätietoja PowerShell-DSC [PowerShell dokumentaatio Centeristä](https://msdn.microsoft.com/powershell/dsc/overview). 

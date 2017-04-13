<properties
   pageTitle="Automaattinen sovelluksen käyttöönottoa virtuaalikoneen tunniste on | Microsoft Azure"
   description="Azure virtuaalikoneen DotNet Core-opetusohjelma"
   services="virtual-machines-windows"
   documentationCenter="virtual-machines"
   authors="neilpeterson"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/21/2016"
   ms.author="nepeters"/>

# <a name="application-deployment-with-azure-resource-manager-templates"></a>Sovelluksen käyttöönotto Azure Resurssienhallinta malleja

Kun kaikki Azure kuivatusohjelmat vaatimukset on tunnistaa ja käyttöönoton mallin käännetään, todellinen sovelluksen käyttöönottoa on pystyttävä vastaamaan. Sovelluksen käyttöönotto tähän viittaaminen asentaminen sivulle Azure resurssien toteutunut sovelluksen-binaaritiedostoja. Musiikin Store-otoksen .net Core ja IIS on asennettu ja määritetty kunkin virtuaalikoneen. Musiikin kaupan binaaritiedostot on asennettava virtuaalikoneen sivulle ja musiikkia myymälän tietokantaan valmiiksi luotu.

Tämän asiakirjan tiedot siitä, miten virtuaalikoneen tunnisteet voit automatisoida sovelluksen käyttöönottoa ja Azure-virtuaalikoneissa määritykset. Kaikki riippuvuudet ja yksilöivä määritykset näkyvät korostettuina. Saat parhaan kokemuksen käyttöönotto valmiiksi Azure tilauksen ja työ sekä Azure Resurssienhallinta-mallin ratkaisun esiintymän. [Musiikin säilön käyttöönotto Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-Windows)löytyy tähän – valmis malli.

## <a name="configuration-script"></a>Kokoonpanon komentosarjaa

Virtuaalikoneen laajennukset ovat erityisiä ohjelmat, jotka suorittavat näennäiskoneiden antamaan määritysten automaatio vastaan. Tunnisteet ovat käytettävissä, kuten anti-virus, kirjauksen määrittäminen ja Docker määritysten monta erityistarkoituksiin. Mukautettu komentosarja-tunniste voidaan suorittaa minkä tahansa komentosarjan virtual machine vastaan. Musiikin Store-malli on enintään musiikkia Store-sovelluksen asentaminen ja määrittäminen Windows-näennäiskoneiden mukautettu komentosarja-tunniste.

Ennen näkyy yksityiskohtaisesti, kuinka Virtuaalikoneen tunnisteet tietueeksi Azure Resurssienhallinta-mallin, tutustu komentosarja, joka suoritetaan. Tämä komentosarja määrittää Windows virtuaalikoneen isännöimiseen musiikkia Store-sovellus. Suorittaa, kun komentosarja asentaa kaikki tarvittavat ohjelmistot, asenna musiikkia store-sovellus ja tietolähteen ohjausobjektin ja tietokannan valmisteleminen. 

> Tässä esimerkissä on esittelyä varten.

```none
Param (
    [string]$user,
    [string]$password,
    [string]$sqlserver
)

# firewall
netsh advfirewall firewall add rule name="http" dir=in action=allow protocol=TCP localport=80

# folders
New-Item -ItemType Directory c:\temp
New-Item -ItemType Directory c:\music

# install iis
Install-WindowsFeature web-server -IncludeManagementTools

# install dot.net core sdk
Invoke-WebRequest http://go.microsoft.com/fwlink/?LinkID=615460 -outfile c:\temp\vc_redistx64.exe
Start-Process c:\temp\vc_redistx64.exe -ArgumentList '/quiet' -Wait
Invoke-WebRequest https://go.microsoft.com/fwlink/?LinkID=809122 -outfile c:\temp\DotNetCore.1.0.0-SDK.Preview2-x64.exe
Start-Process c:\temp\DotNetCore.1.0.0-SDK.Preview2-x64.exe -ArgumentList '/quiet' -Wait
Invoke-WebRequest https://go.microsoft.com/fwlink/?LinkId=817246 -outfile c:\temp\DotNetCore.WindowsHosting.exe
Start-Process c:\temp\DotNetCore.WindowsHosting.exe -ArgumentList '/quiet' -Wait

# download / config music app
Invoke-WebRequest  https://github.com/Microsoft/dotnet-core-sample-templates/raw/master/dotnet-core-music-windows/music-app/music-store-azure-demo-pub.zip -OutFile c:\temp\musicstore.zip
Expand-Archive C:\temp\musicstore.zip c:\music
(Get-Content C:\music\config.json) | ForEach-Object { $_ -replace "<replaceserver>", $sqlserver } | Set-Content C:\music\config.json
(Get-Content C:\music\config.json) | ForEach-Object { $_ -replace "<replaceuser>", $user } | Set-Content C:\music\config.json
(Get-Content C:\music\config.json) | ForEach-Object { $_ -replace "<replacepass>", $password } | Set-Content C:\music\config.json

# workaround for db creation bug
Start-Process 'C:\Program Files\dotnet\dotnet.exe' -ArgumentList 'c:\music\MusicStore.dll'

# configure iis
Remove-WebSite -Name "Default Web Site"
Set-ItemProperty IIS:\AppPools\DefaultAppPool\ managedRuntimeVersion ""
New-Website -Name "MusicStore" -Port 80 -PhysicalPath C:\music\ -ApplicationPool DefaultAppPool
& iisreset
```

## <a name="vm-script-extension"></a>AM komentosarja-tunniste

AM tunnisteet voidaan suorittaa vastaan virtual machine muodosta milloin sisällyttämällä tunniste resurssin Azure Resurssienhallinta-malli. Tunniste voidaan lisätä Visual Studio Lisää resurssin ohjatun haun avulla tai lisäämällä kelvollinen JSON malliin. Komentosarjan tunniste resurssi on ylemmän tason virtuaalikoneen; resurssin sisällä Tämä näkevät seuraavan esimerkin mukaisesti.

Tätä linkkiä nähdäksesi Resurssienhallinta mallin – [AM komentosarjan tunniste](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L339)sisällä JSON-malli. 

Huomaa, että komentosarja on tallennettu GitHub JSON alla. Tämä komentosarja myös voidaan tallentaa Azure-Blob-objektien tallennustilaan. Lisäksi Azure Resurssienhallinta mallien avulla komentosarjojen suorittamisen merkkijonon rakennettava siten, että mallin parametrien arvot voidaan parametreiksi komentosarjojen suorittamisen. Tässä tapauksessa tiedot annetaan, kun otat mallit ja nämä arvot voidaan sitten komentosarja suoritettaessa.

```none
{
  "apiVersion": "2015-06-15",
  "type": "extensions",
  "name": "config-app",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'),copyindex())]",
    "[variables('musicstoresqlName')]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Compute",
    "type": "CustomScriptExtension",
    "typeHandlerVersion": "1.4",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1"
      ]
    },
    "protectedSettings": {
      "commandToExecute": "[concat('powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1 -user ',parameters('adminUsername'),' -password ',parameters('adminPassword'),' -sqlserver ',variables('musicstoresqlName'),'.database.windows.net')]"
    }
  }
}
```

Lisätietoja mukautettu komentosarja-tunniste on artikkelissa [mukautettu komentosarja-laajennukset Resurssienhallinta malleja](./virtual-machines-windows-extensions-customscript.md).

## <a name="next-step"></a>Seuraava vaihe

<hr>

[Tutustu Lisää Azure Resurssienhallinta-mallit](https://github.com/Azure/azure-quickstart-templates)

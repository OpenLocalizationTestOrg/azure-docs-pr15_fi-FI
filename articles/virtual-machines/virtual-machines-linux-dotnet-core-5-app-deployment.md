<properties
   pageTitle="Automaattinen sovelluksen käyttöönottoa virtuaalikoneen tunniste on | Microsoft Azure"
   description="Azure virtuaalikoneen DotNet Core-opetusohjelma"
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="neilpeterson"
   manager="timlt"
   editor="tysonn"
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="09/21/2016"
   ms.author="nepeters"/>

# <a name="application-deployment-with-azure-resource-manager-templates"></a>Sovelluksen käyttöönotto Azure Resurssienhallinta malleja

Kun kaikki Azure kuivatusohjelmat vaatimukset on tunnistaa ja muuntamaan käyttöönoton mallia, todellinen sovelluksen käyttöönottoa on pystyttävä vastaamaan. Sovelluksen käyttöönotto tähän viittaaminen asentaminen sivulle Azure resurssien toteutunut sovelluksen-binaaritiedostoja. Musiikin Store-otoksen .net Core NGINX ja valvojan on asennettu ja määritetty kunkin virtuaalikoneen. Musiikin kaupan binaaritiedostot on asennettava virtuaalikoneen sivulle ja musiikkia myymälän tietokantaan valmiiksi luotu.

Tämän asiakirjan tiedot siitä, miten virtuaalikoneen tunnisteet voit automatisoida sovelluksen käyttöönottoa ja Azure-virtuaalikoneissa määritykset. Kaikki riippuvuudet ja yksilöivä määritykset näkyvät korostettuina. Saat parhaan kokemuksen käyttöönotto valmiiksi esiintymän Azure tilaus ja työ sekä Azure Resurssienhallinta mallia. [Musiikin kaupan käyttöönoton Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux)löytyy tähän – valmis malli.

## <a name="configuration-script"></a>Kokoonpanon komentosarjaa

Virtuaalikoneen laajennukset ovat erityisiä ohjelmat, jotka suorittavat näennäiskoneiden antamaan määritysten automaatio vastaan. Tunnisteet ovat käytettävissä, kuten anti-virus, kirjauksen määrittäminen ja Docker määritysten monta erityistarkoituksiin. Mukautettu komentosarja-tunniste voidaan suorittaa minkä tahansa komentosarjan virtual machine vastaan. Musiikin Store-malli on enintään musiikkia Store-sovelluksen asentaminen ja määrittäminen Ubuntu näennäiskoneiden mukautettu komentosarja-tunniste.

Ennen näkyy yksityiskohtaisesti, kuinka Virtuaalikoneen tunnisteet tietueeksi Azure Resurssienhallinta-mallin, tutustu komentosarja, joka suoritetaan. Tämä komentosarja määrittää Ubuntu virtuaalikoneen isännöimiseen musiikkia Store-sovellus. Suorittaa, kun komentosarja asentaa kaikki tarvittavat ohjelmistot, asenna musiikkia store-sovellus ja tietolähteen ohjausobjektin ja tietokannan valmisteleminen. 

Lisätietoja isännöinnin .net Core Linux-sovellusta, Julkaise [Linux tuotantoympäristössä](https://docs.asp.net/en/latest/publishing/linuxproduction.html). 

> Tässä esimerkissä on esittelyä varten.

```none
#!/bin/bash

# install dotnet core
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ trusty main" > /etc/apt/sources.list.d/dotnetdev.list'
sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
sudo apt-get update
sudo apt-get install -y dotnet-dev-1.0.0-preview2-003121

# download application
sudo wget https://raw.github.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/music-app/music-store-azure-demo-pub.tar /
sudo mkdir /opt/music
sudo tar -xf music-store-azure-demo-pub.tar -C /opt/music

# install nginx, update config file
sudo apt-get install -y nginx
sudo service nginx start
sudo touch /etc/nginx/sites-available/default
sudo wget https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/music-app/nginx-config/default -O /etc/nginx/sites-available/default
sudo cp /opt/music/nginx-config/default /etc/nginx/sites-available/
sudo nginx -s reload

# update and secure music config file
sed -i "s/<replaceserver>/$1/g" /opt/music/config.json
sed -i "s/<replaceuser>/$2/g" /opt/music/config.json
sed -i "s/<replacepass>/$3/g" /opt/music/config.json
sudo chown $2 /opt/music/config.json
sudo chmod 0400 /opt/music/config.json

# config supervisor
sudo apt-get install -y supervisor
sudo touch /etc/supervisor/conf.d/music.conf
sudo wget https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/music-app/supervisor/music.conf -O /etc/supervisor/conf.d/music.conf
sudo service supervisor stop
sudo service supervisor start

# pre-create music store database
/usr/bin/dotnet /opt/music/MusicStore.dll &
```

## <a name="vm-script-extension"></a>AM komentosarja-tunniste

AM tunnisteet voidaan suorittaa vastaan virtual machine muodosta milloin sisällyttämällä tunniste resurssin Azure Resurssienhallinta-malli. Tunniste voidaan lisätä Visual Studio Lisää resurssin ohjatun haun avulla tai lisäämällä kelvollinen JSON malliin. Komentosarjan tunniste resurssi on ylemmän tason virtuaalikoneen; resurssin sisällä Tämä näkevät seuraavan esimerkin mukaisesti.

Tätä linkkiä nähdäksesi Resurssienhallinta mallin – [AM komentosarjan tunniste](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L359)sisällä JSON-malli. 

Huomaa, että komentosarja on tallennettu GitHub JSON alla. Tämä komentosarja myös voidaan tallentaa Azure-Blob-objektien tallennustilaan. Lisäksi Azure Resurssienhallinta mallien avulla komentosarjojen suorittamisen merkkijonon rakennettava siten, että mallin parametrien arvot voidaan parametreiksi komentosarjojen suorittamisen. Tässä tapauksessa tiedot annetaan, kun otat mallit ja nämä arvot voidaan sitten komentosarja suoritettaessa.

```none
{
  "apiVersion": "2015-06-15",
  "type": "extensions",
  "name": "config-app",
  "location": "[resourceGroup().location]",
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
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"
      ]
    },
    "protectedSettings": {
      "commandToExecute": "[concat('sudo sh config-music.sh ',variables('musicStoreSqlName'), ' ', parameters('adminUsername'), ' ', parameters('sqlAdminPassword'))]"
    }
  }
}
```

Lisätietoja mukautettu komentosarja-tunniste on artikkelissa [mukautettu komentosarja-laajennukset Resurssienhallinta malleja](./virtual-machines-linux-extensions-customscript.md).

## <a name="next-step"></a>Seuraava vaihe

<hr>

[Tutustu Lisää Azure Resurssienhallinta-mallit](https://github.com/Azure/azure-quickstart-templates)

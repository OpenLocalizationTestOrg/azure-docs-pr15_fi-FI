<properties
    pageTitle="CustomScript tunnisteen käyttäminen Linux AM | Microsoft Azure"
    description="Opettele käyttämään CustomScript-tunniste sovellusten-Linux näennäiskoneiden Azure-tietokannassa luotu perinteinen käyttöönoton mallia."
    editor="tysonn"
    manager="timlt"
    documentationCenter=""
    services="virtual-machines-linux"
    authors="gbowerman"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="multiple"
    ms.tgt_pltfrm="linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="guybo"/>

#<a name="deploy-a-lamp-app-using-the-azure-customscript-extension-for-linux"></a>Azure CustomScript laajennuksen käytön Linux hehkulampun sovelluksen käyttöönotto#

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Microsoft Azure CustomScript-laajennus Linux avulla on helppo mukauttaa näennäiskoneiden (VMs) suorittamalla koodin kirjoitettu mitä tahansa tukemat AM (esimerkiksi Python ja Bash) kieltä. Tämä tarjoaa joustavia voi automatisoida useita koneet sovelluksen käyttöönottoa.

Voit ottaa käyttöön CustomScript laajentaminen käyttämällä Azure perinteinen portaalin ja Windows PowerShellin Azure komentorivivalitsimet Interface (Azure CLI).

Tässä artikkelissa käytetään käyttöön yksinkertainen hehkulampun-sovelluksen Ubuntu AM Azure-CLI luotu perinteinen käyttöönotto-mallin.

## <a name="prerequisites"></a>Edellytykset

Tässä esimerkissä Luo kaksi Azure virtuaalilaitteiksi Ubuntu 14.04 tai uudempi versio. VMs kutsutaan *komentosarjan AM* ja *hehkulampun AM*. Käytä yksilöllisiä nimiä, kun haluat luoda VMs. Voit suorittaa CLI komennot käytetään ja yksi käytetään hehkulampun-sovelluksen käyttöönotto.

Sinun on myös Azure-tallennustilan tilin ja käyttää sitä (Saat tämä perinteinen Azure-portaalista) avain.

Jos tarvitset apua Linux VMs luomisesta Azure viitata [Luo virtuaalikoneen käynnissä Linux](virtual-machines-linux-classic-createportal.md).

Asenna komentoja oletetaan, että Ubuntu, mutta voit mukauttaa minkä tahansa tuetut Linux-distro asennuksen.

Komentosarjan AM AM täytyy olla Azure CLI asennetulla toimimasta Azure-yhteyttä. Yhteyttä viitata [asennetaan](../xplat-cli-install.md)ja määritetään Azure käyttöliittymä.

## <a name="upload-a-script"></a>Lataa komentosarja

Olemme komentosarjan suorittaminen remote AM PHP sivun luominen ja asenna hehkulampun pino CustomScript tunniste käytetään. Jotta voit käyttää missä tahansa komentosarja on ladata sen Azure-blob nimellä.

### <a name="script-overview"></a>Komentosarjan yleiskatsaus

Komentosarjan Esimerkki asentaa hehkulampun pinon Ubuntu (mukaan lukien määrittäminen MySQL hiljainen asennus), kirjoittaa yksinkertaisen PHP-tiedoston ja Apache käynnistyy.

    #!/bin/bash
    # set up a silent install of MySQL
    dbpass="mySQLPassw0rd"

    export DEBIAN_FRONTEND=noninteractive
    echo mysql-server-5.6 mysql-server/root_password password $dbpass | debconf-set-selections
    echo mysql-server-5.6 mysql-server/root_password_again password $dbpass | debconf-set-selections

    # install the LAMP stack
    apt-get -y install apache2 mysql-server php5 php5-mysql  

    # write some PHP
    echo \<center\>\<h1\>My Demo App\</h1\>\<br/\>\</center\> > /var/www/html/phpinfo.php
    echo \<\?php phpinfo\(\)\; \?\> >> /var/www/html/phpinfo.php

    # restart Apache
    apachectl restart

### <a name="upload-script"></a>Lataa komentosarja

Tallenna komentosarja tekstitiedostona, kuten *install_lamp.sh*, ja lataa se sitten Azure-tallennustilan. Voit tehdä tämän helposti Azure CLI kanssa. Seuraavassa esimerkissä Lataa tiedosto tallennustilan säilö "komentosarjojen". Jos säilö ei ole tarvitset luo se ensin.

    azure storage blob upload -a <yourStorageAccountName> -k <yourStorageKey> --container scripts ./install_lamp.sh

Luo myös JSON-tiedosto, jossa kuvataan Lataa komentosarja Azure-tallennustilan. Tallentaa sen *public_config.json* ("mystorage" tilalle tallennustilan tilin nimi):

    {"fileUris":["https://mystorage.blob.core.windows.net/scripts/install_lamp.sh"], "commandToExecute":"sh install_lamp.sh" }


## <a name="deploy-the-extension"></a>Käyttöönotto tunniste

Voit nyt Linux CustomScript tunniste käyttöön käyttämällä Azure-CLI remote AM seuraava komento.

    azure vm extension set -c "./public_config.json" lamp-vm CustomScript Microsoft.Azure.Extensions 2.0

Edellisen komennon Lataa, ja se toimii *install_lamp.sh* komentosarja kutsutaan *hehkulampun AM*AM.

Koska sovelluksen sisältyy verkkopalvelimeen, muista Avaa HTTP-kumpaan, valitse remote AM seuraava komento.

    azure vm endpoint create -n Apache -o tcp lamp-vm 80 80

## <a name="monitoring-and-troubleshooting"></a>Valvonta ja vianmääritys

Voit tarkistaa, kuinka hyvin mukautettu komentosarja suorittaa remote AM lokitiedoston ulkoasu. *Hehkulampun AM* ja siten, että seuraava komento lokitiedosto SSH.

    cd /var/log/azure/customscript
    tail -f handler.log

Kun olet suorittanut CustomScript-tunniste, voit selata luomasi lisätietoja PHP sivulle. Tämän artikkelin Esimerkki PHP-sivu on *http://lamp-vm.cloudapp.net/phpinfo.php*.

## <a name="additional-resources"></a>Lisäresursseja

Voit käyttää samaa perusvaiheet monimutkaisia Apps-sovellusten käyttöön. Tässä esimerkissä Asenna komentosarja on tallennettu nimellä julkisen blob Azuren tallennustilaan. Turvallisempi vaihtoehto olisi tallentaa Asenna komentosarja suojatun blob [Suojatun Access-allekirjoitus](https://msdn.microsoft.com/library/azure/ee395415.aspx) (SAS).

Lisäresursseja Azure CLI, Linux ja CustomScript-tunniste näkyvät Seuraava.

[Käyttämällä CustomScript tunniste Linux AM mukauttaminen tehtävien automatisointi](https://azure.microsoft.com/blog/2014/08/20/automate-linux-vm-customization-tasks-using-customscript-extension/)

[Azure Linux-laajennukset (GitHub)](https://github.com/Azure/azure-linux-extensions)

[Linux ja Avaa lähde-Azure tietojenkäsittely](virtual-machines-linux-opensource-links.md)

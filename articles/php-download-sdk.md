<properties
    pageTitle="Lataa Azure SDK PHP"
    description="Opettele Lataa ja asenna Azure SDK PHP."
    documentationCenter="php"
    services="app-service\web"
    authors="allclark"
    manager="douge"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="06/01/2016"
    ms.author="allclark;yaqiyang"/>

#<a name="download-the-azure-sdk-for-php"></a>Lataa Azure SDK PHP

## <a name="overview"></a>Yleiskatsaus

Azure SDK PHP sisältää osat, joiden avulla voit kehittää, käyttöönottoa ja Azure PHP-sovellusten hallinta. Tarkemmin sanottuna Azure SDK PHP sisältää seuraavat:

* **Azure PHP asiakkaan kirjastoissa**. Luokan nämä kirjastot sisältävät käyttöliittymään Azure-ominaisuuksia, kuten data management Services-palvelun käyttöä varten, ja cloud services.  
* **Azure komentorivivalitsimet liittymän Mac, Linux- ja Windows (Azure CLI)**. Tämä on joukko käyttöönottoon ja hallintaan Azure palvelut, kuten Azure sivustot ja Azuren näennäiskoneiden komentoja. Alustan, mukaan lukien Mac, Linux ja Windows Azure CLI työt.
* **Azure PowerShell (vain Windowsissa)**. Tämä on joukko PowerShell cmdlet-komennot käyttöönotto ja Azure-palvelut, kuten pilvipalveluihin ja näennäiskoneiden hallinta.
* **Azure emulaattorit (vain Windowsissa)**. Laske- ja tallennustilaa emulointeja on paikallinen emulointeja pilvipalveluihin ja tietojen hallinta-palveluista, joita voidaan testata sovelluksen paikallisesti. Azure-emulointeja suorittaa vain Windows.

Seuraavissa osissa kuvataan Lataa ja asenna edellä kuvatun osat.

Tämän artikkelin ohjeita oletetaan, että [PHP] [ install-php] asennettuna.

> [AZURE.NOTE] Sinulla on oltava PHP asiakkaan kirjastoissa käytettävät Azure PHP 5,5 tai sitä uudemmassa versiossa.

##<a name="php-client-libraries-for-azure"></a>Azure PHP asiakkaan kirjastoissa

Azure PHP-asiakasohjelman kirjastoissa antaa käyttöliittymään Azure-ominaisuuksia, kuten data management Services-palvelun käyttäminen ja cloud services-sellaisille käyttöjärjestelmille. Nämä kirjastot voidaan asentaa tunnus kautta.

Saat tietoja siitä, miten PHP asiakkaan kirjastoissa käytettävät Azure- [Blob-palvelun käyttäminen][blob-service]- [taulukko-palvelun käyttäminen] [ table-service] ja [Opi käyttämään jono-palvelun][queue-service].

###<a name="install-via-composer"></a>Asenna kautta tunnus

1. [Asenna Git][install-git].


    > [AZURE.NOTE] Windows voit myös on suoritettava Git lisääminen PATH-ympäristömuuttuja.

2. Luo tiedosto nimeltä **composer.json** päätasolla projektiin ja lisää seuraava koodi:

        {
            "require": {
                "microsoft/windowsazure": "^0.4"
            }
        }

3. Lataa ** [composer.phar] [ composer-phar] ** projektin pääsivustoon.

4. Avaa komentorivi-ikkuna ja suorittaa tämän project-pääkansio

        php composer.phar install

##<a name="azure-powershell-and-azure-emulators"></a>Azure PowerShellistä ja Azure emulaattorit

Azure PowerShell on joukko PowerShell cmdlet-komennot käyttöönotto ja Azure-palvelujen (kuten pilvipalveluihin ja näennäiskoneiden) hallinta. Azure-emulointeja ovat emulointeja pilvipalveluihin ja tietojen hallinta-palveluista, joita voidaan testata sovelluksen paikallisesti. Nämä osat tuetaan vain Windows.

Suositeltu tapaa asentaa PowerShellin Azure ja Azure-emulointeja on [Microsoftin WWW-ympäristö asennusohjelma][download-wpi]. Huomaa, että voit valita myös asentaa kehittäminen muita osia, kuten PHP, SQL Server ja Microsoft SQL Server PHP ja WebMatrix Drivers.

Lisätietoja PowerShellin Azure käyttämisestä on Katso, [miten voit käyttää PowerShellin Azure][powershell-tools].

##<a name="azure-cli"></a>Azure CLI

Azure-CLI on joukko käyttöönottoon ja hallintaan Azure palvelut, kuten Azure sivustot ja Azuren näennäiskoneiden komentoja. Lisätietoja Azure CLI asentamisesta on artikkelissa [Azure-CLI asentaminen](xplat-cli-install.md).

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja on artikkelissa [PHP Developer Center](/develop/php/).


[install-php]: http://www.php.net/manual/en/install.php
[composer-github]: https://github.com/composer/composer
[composer-phar]: http://getcomposer.org/composer.phar
[nodejs-org]: http://nodejs.org/
[install-node-linux]: https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager
[download-wpi]: http://go.microsoft.com/fwlink/?LinkId=253447
[mac-installer]: http://go.microsoft.com/fwlink/?LinkId=252249
[blob-service]: http://go.microsoft.com/fwlink/?LinkId=252714
[table-service]: http://go.microsoft.com/fwlink/?LinkId=252715
[queue-service]: http://go.microsoft.com/fwlink/?LinkId=252716
[azure cli]: http://go.microsoft.com/fwlink/?LinkId=252717
[powershell-tools]: http://go.microsoft.com/fwlink/?LinkId=252718
[php-sdk-github]: http://go.microsoft.com/fwlink/?LinkId=252719
[install-git]: http://git-scm.com/book/en/Getting-Started-Installing-Git

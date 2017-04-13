<properties
    pageTitle="HEHKULAMPUN käyttöönotto Linux virtual koneeseen | Microsoft Azure"
    description="Lue, miten voit asentaa hehkulampun pino Linux AM"
    services="virtual-machines-linux"
    documentationCenter="virtual-machines"
    authors="jluk"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="NA"
    ms.topic="article"
    ms.date="06/07/2016"
    ms.author="juluk"/>

# <a name="deploy-lamp-stack-on-azure"></a>HEHKULAMPUN pinon Azure-käyttöönotto
Tässä artikkelissa opastaa käyttöönottamisesta Apache verkkopalvelin, MySQL ja Azure-PHP (hehkulampun pinon). Sinun on Azure-tili ([maksuton kokeiluversio](https://azure.microsoft.com/pricing/free-trial/)) ja [Azure CLI](../xplat-cli-install.md) , joka on [liitetty Azure-tiliisi](../xplat-cli-connect.md).

Tämän artikkelin hehkulampun asentamiseen kahdella tavalla:

## <a name="quick-command-summary"></a>Pika-komennon yhteenveto

1) Ottaa käyttöön uuden AM hehkulampun

```
# One command to create a resource group holding a VM with LAMP already on it
$ azure group create -n uniqueResourceGroup -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json
```

2) Valitse aiemmin luotu AM hehkulampun käyttöönotto

```
# Two commands: one updates packages, the other installs Apache, MySQL, and PHP
user@ubuntu$ sudo apt-get update
user@ubuntu$ sudo apt-get install apache2 mysql-server php5 php5-mysql
```

## <a name="deploy-lamp-on-new-vm-walkthrough"></a>Ottaa käyttöön uuden AM ongelmatilanteita hehkulampun

Voit aloittaa luomalla uusi [resurssiryhmä](../azure-resource-manager/resource-group-overview.md) , joka sisältää AM:

    $ azure group create uniqueResourceGroup westus
    info:    Executing command group create
    info:    Getting resource group uniqueResourceGroup
    info:    Creating resource group uniqueResourceGroup
    info:    Created resource group uniqueResourceGroup
    data:    Id:                  /subscriptions/########-####-####-####-############/resourceGroups/uniqueResourceGroup
    data:    Name:                uniqueResourceGroup
    data:    Location:            westus
    data:    Provisioning State:  Succeeded
    data:    Tags: null
    data:
    info:    group create command OK

Voit luoda itse AM, voit käyttää jo kirjoitettujen Azure Resurssienhallinta-mallin löydy [tähän GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/lamp-app).

    $ azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json uniqueResourceGroup uniqueLampName

Jotkin Lisää syötteiden kehotteita vastauksen pitäisi näkyä seuraavat:

    info:    Executing command group deployment create
    info:    Supply values for the following parameters
    storageAccountNamePrefix: lampprefix
    location: westus
    adminUsername: someUsername
    adminPassword: somePassword
    mySqlPassword: somePassword
    dnsLabelPrefix: azlamptest
    info:    Initializing template configurations and parameters
    info:    Creating a deployment
    info:    Created template deployment "uniqueLampName"
    info:    Waiting for deployment to complete
    data:    DeploymentName     : uniqueLampName
    data:    ResourceGroupName  : uniqueResourceGroup
    data:    ProvisioningState  : Succeeded
    data:    Timestamp          :
    data:    Mode               : Incremental
    data:    CorrelationId      : d51bbf3c-88f1-4cf3-a8b3-942c6925f381
    data:    TemplateLink       : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json
    data:    ContentVersion     : 1.0.0.0
    data:    DeploymentParameters :
    data:    Name                      Type          Value
    data:    ------------------------  ------------  -----------
    data:    storageAccountNamePrefix  String        lampprefix
    data:    location                  String        westus
    data:    adminUsername             String        someUsername
    data:    adminPassword             SecureString  undefined
    data:    mySqlPassword             SecureString  undefined
    data:    dnsLabelPrefix            String        azlamptest
    data:    ubuntuOSVersion           String        14.04.2-LTS
    info:    group deployment create command OK

Olet nyt luonut Linux AM hehkulampun asennettu valmiiksi. Jos haluat, voit tarkistaa asennusta hyppäämällä alaspäin [Tarkista hehkulampun asennettu].

## <a name="deploy-lamp-on-existing-vm-walkthrough"></a>Valitse aiemmin luotu AM ongelmatilanteita hehkulampun käyttöönotto

Jos tarvitset apua Linux AM voit head [tässä opit luomaan Linux AM] (. / virtual-machines-linux-quick-create-cli.md). Seuraavaksi tarvitset SSH Linux AM kyselyjä. Jos tarvitset apua luomisessa SSH-näppäintä voit head [tässä opit luomaan SSH avain Linux/Mac] (. / virtual-machines-linux-mac-create-ssh-keys.md).
Jos sinulla jo SSH-näppäintä, siirry eteenpäin ja SSH Linux-AM kanssa-kyselyjä `ssh username@uniqueDNS`.

Nyt kun käsittelet Linux AM sisällä, selkeät hehkulampun pino asentamisessa Debian perustuva jaot. Tarkka komentoja voi vaihdella muiden Linux distros.

#### <a name="installing-on-debianubuntu"></a>Debian/Ubuntu asentaminen

Sinun on asennettu seuraavat paketit: `apache2`, `mysql-server`, `php5`, ja `php5-mysql`. Voit asentaa ne suoraan otsikkorivialueen pakkauksissa tai käyttämällä Tasksel. Ohjeet molemmat vaihtoehdot näkyvät jäljempänä.
Ennen kuin asennat sinun on ladattava ja Päivitä paketin luettelot.

    user@ubuntu$ sudo apt-get update
    
##### <a name="individual-packages"></a>Yksittäisten pakettien
Käyttämällä piharakennus get:

    user@ubuntu$ sudo apt-get install apache2 mysql-server php5 php5-mysql

##### <a name="using-tasksel"></a>Tasksel käyttäminen
Vaihtoehtoisesti voit ladata Tasksel, Debian/Ubuntu-työkalua, joka asentaa useita liittyvät paketteja järjestelmään koordinoidun "tehtävän".

    user@ubuntu$ sudo apt-get install tasksel
    user@ubuntu$ sudo tasksel install lamp-server

Kun olet suorittanut joko yllä olevista voit pyydetään Asenna nämä paketit ja muut riippuvuussuhteiden lukumäärä. Painamalla "k" ja sitten Enter Jatka ja noudata kehotteita, muut järjestelmänvalvojan salasanan määrittäminen MySQL. Asennetaan vähintään tarvittavat PHP laajennukset PHP käytettäväksi MySQL tarvitaan. 

![][1]

Suorita seuraava komento, jos haluat nähdä muita PHP tunnisteet, jotka ovat käytettävissä pakettien:

    user@ubuntu$ apt-cache search php5


#### <a name="create-infophp-document"></a>Info.php asiakirjan luominen

Tarkista Apache, MySQL ja PHP käytössäsi version komentorivin kautta kirjoittamalla pitäisi nyt `apache2 -v`, `mysql -v`, tai `php -v`.

Jos haluat testata tarkemmin, voit luoda nopeasti PHP tietojen sivulla voit tarkastella selaimessa. Luo uusi tiedosto Nano tekstieditorissa tällä komennolla:

    user@ubuntu$ sudo nano /var/www/html/info.php

Sisällä GNU Nano tekstieditorissa Lisää seuraavat rivit:

    <?php
    phpinfo();
    ?>

Valitse Tallenna ja sulje tekstieditori.

Käynnistä Apache tällä komennolla, jotta kaikki uudet asennuksesi tulee voimaan.

    user@ubuntu$ sudo service apache2 restart

## <a name="verify-lamp-successfully-installed"></a>Tarkista hehkulampun onnistui

Nyt voit tarkistaa juuri luomasi selaimessa valitsemalla http://youruniqueDNS/info.php PHP tiedot-sivulla, pitäisi näyttää seuraavanlaiselta.

![][2]

Voit tarkistaa Apache asennuksen Apache2 Ubuntu oletusarvoinen sivun tarkasteleminen siirtymällä voit http://youruniqueDNS/. Raportissa pitäisi näkyä seuraavankaltaiselta.

![][3]

Onnittelut, sinulla on vain asennuksen hehkulampun pinon Azure AM!

## <a name="next-steps"></a>Seuraavat vaiheet

Tutustu hehkulampun pinossa Ubuntu ohjeissa:

- [https://Help.Ubuntu.com/Community/ApacheMySQLPHP](https://help.ubuntu.com/community/ApacheMySQLPHP)

[1]: ./media/virtual-machines-linux-deploy-lamp-stack/configmysqlpassword-small.png
[2]: ./media/virtual-machines-linux-deploy-lamp-stack/phpsuccesspage.png
[3]: ./media/virtual-machines-linux-deploy-lamp-stack/apachesuccesspage.png

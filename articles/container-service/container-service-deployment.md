<properties
   pageTitle="Ottaa käyttöön Azure säilö Service-klusterin | Microsoft Azure"
   description="Ota käyttöön Azure säilö Service-klusterin Azure portaalin, Azure CLI tai PowerShellin avulla."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, säilöt-mikro-palveluja, Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/13/2016"
   ms.author="rogardle"/>

# <a name="deploy-an-azure-container-service-cluster"></a>Azure säilö Service-klusterin käyttöönotto

Azure säilö-palvelu sisältää nopeaa käyttöönottoa Suositut Avaa lähde säilöstä klusterointi ja tiedonsiirron ratkaisuja. Voit ottaa Ohjauskoneen/OS ja Azure Resurssienhallinta malleja Docker Swarm varausyksiköiden tai Azure portaalin Azure säilö-palvelun avulla. Näiden klustereiden käyttöönoton käyttämällä Azure virtuaalikoneen asteikko joukot ja varausyksiköiden hyödyntää Azure verkko- ja tallennustilaa tarjouksia. Käyttää Azure säilö palvelu, tarvitset Azure tilauksen. Jos sinulla ei ole, valitse voit rekisteröityä [maksuttoman kokeiluversion käyttäjäksi](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935).

Tämän asiakirjan käydään läpi käyttöönotto Azure säilö Service-klusterin [Azure portal](#creating-a-service-using-the-azure-portal) [Azure käyttöliittymä (CLI)](#creating-a-service-using-the-azure-cli)ja [Azure PowerShell-moduulin](#creating-a-service-using-powershell)avulla.  

## <a name="create-a-service-by-using-the-azure-portal"></a>Palvelun luominen Azure-portaalissa

Kirjaudu Azure-portaaliin, valitsemalla **Uusi**ja Etsi **Azure säilö palvelun**Azure Marketplacesta.

![Luo käyttöönoton 1](media/acs-portal1.png)  <br />

Valitse **Azure säilö palvelu**ja sitten **Luo**.

![Luo käyttöönoton 2](media/acs-portal2.png)  <br />

Anna seuraavat tiedot:

- **Käyttäjänimi**: Tämä on käyttäjänimi, jota käytetään kunkin näennäiskoneiden tilin ja Azure säilö Service-klusterin asettaa virtuaalikoneen asteikko.
- **Tilaus**: Valitse Azure tilaus.
- **Resurssiryhmä**: Valitse aiemmin luotu resurssiryhmä tai luoda uuden.
- **Sijainti**: Valitse Azure alue, Azure säilö palvelun käyttöönottoa varten.
- **SSH julkisella avaimella**: Lisää julkisella avaimella, jota käytetään vastaan Azure säilö palvelun näennäiskoneiden todennusta varten. On tärkeää, että avaimeen sisältää ei rivinvaihdot ja että se sisältää "ssh-rsa" etuliite ja 'username@domain' postfix. Pitäisi näyttää seuraavanlaiselta: **ssh-rsa AAAAB3Nz... <>...... UcyupgH azureuser@linuxvm **. Ohjeita suojattu runko (SSH) näppäimet luomisesta on artikkelissa [Linux]( https://azure.microsoft.com/documentation/articles/virtual-machines-linux-ssh-from-linux/) - ja [Windows]( https://azure.microsoft.com/documentation/articles/virtual-machines-linux-ssh-from-windows/) -artikkeleissa.

Kun haluat jatkaa, valitse **OK** .

![Luo käyttöönoton 3](media/acs-portal3.png)  <br />

Valitse tiedonsiirron tyyppi. Vaihtoehdot ovat:

- **Toimialueen Ohjauskoneen/OS**: Ohjauskoneen/OS klusterin ottaa käyttöön.
- **Swarm**: Docker Swarm klusterin ottaa käyttöön.

Kun haluat jatkaa, valitse **OK** .

![Luo deployment 4](media/acs-portal4.png)  <br />

Anna seuraavat tiedot:

- **Perustyyli laskea**: klusterin perustyylien määrää.
- **Agentti Laske**: varten Docker Swarm, se on käyttäjäagenttien agentti asteikko määrittäminen aloitusnumero. Toimialueen Ohjauskoneen/OS tämä on yksityinen asteikko joukon käyttäjäagenttien aloitusnumero. Lisäksi on luotu julkisen asteikko-joukon, joka sisältää ennalta määrä agenttien vuoksi. Tätä yleisen asteikon määrittäminen tekijöiden määrä määräytyy mukaan montako perustyylit on luotu klusterin--yksi julkisen agentti yksi perustyyli ja kaksi kolme tai viidestä perustyylin julkisen agenttien.
- **Agentti virtuaalikoneen kokoa**: agentti näennäiskoneiden kokoa.
- **DNS-etuliite**: maailman yksilöivä nimi, jota käytetään etuliite täydelliset toimialuenimet-palvelun tärkeisiin osiin.

Kun haluat jatkaa, valitse **OK** .

![Luo käyttöönoton 5](media/acs-portal5.png)  <br />

Kun palvelun vahvistus on valmis, valitse **OK** .

![Luo käyttöönoton 6](media/acs-portal6.png)  <br />

Valitse **Luo** luodaksesi käyttöönottoprosessin.

![Luo käyttöönoton 7](media/acs-portal7.png)  <br />

Jos olet kiinnittäminen käyttöönoton Azure-portaaliin, näet käyttöönoton tilan.

![Luo käyttöönoton 8](media/acs-portal8.png)  <br />

Kun asennus on valmis, Azure säilö Service-klusteriin on valmis käytettäväksi.

## <a name="create-a-service-by-using-the-azure-cli"></a>Luo palvelu Azure-CLI avulla

Voit luoda Azure säilö palvelun esiintymän komentoriviltä, sinun on Azure tilauksen. Jos sinulla ei ole, valitse voit rekisteröityä [maksuttoman kokeiluversion käyttäjäksi](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935). Haluat myös on [asennettu](../xplat-cli-install.md) ja [määritetty](../xplat-cli-connect.md) Azure-CLI.

Käyttöönotto Ohjauskoneen/OS tai Docker Swarm klusterin, valitse jokin seuraavista malleista GitHub. Huomaa, että molemmat mallit ovat samat, lukuun ottamatta orchestrator oletusvalinta.

* [Toimialueen Ohjauskoneen/OS malli](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos)
* [Swarm malli](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm)

Varmista seuraavaksi, että Azure-CLI yhteydessä Azure-tilaukseen. Voit tehdä tämän käyttämällä seuraava komento:

```bash
azure account show
```
Jos ei palautetaan Azure-tili, kirjaudu CLI Azure seuraava komento avulla.

```bash
azure login -u user@domain.com
```

Seuraavaksi määrittäminen käyttämään Azure Resurssienhallinta Azure CLI-työkalut.

```bash
azure config mode arm
```

Azure resurssiryhmä- ja säilö Service-klusterin luominen seuraava komento, jossa:

- **RESOURCE_GROUP** on resurssiryhmä, jota haluat käyttää tämän palvelun nimi.
- **Sijainti** on Azure alue, jossa resurssiryhmä ja Azure säilö palvelun käyttöönoton luodaan.
- **TEMPLATE_URI** on käyttöönotto-tiedoston sijainti. Huomaa, että tämä on oltava raaka tiedosto ei ole GitHub-Käyttöliittymän osoitin. Etsi seuraava URL-osoite ja valitse azuredeploy.json tiedosto GitHub ja **raaka** -painiketta.

> [AZURE.NOTE] Kun suoritat tämän komennon-liittymän kysyy käyttöönoton parametriarvot.

```bash
azure group create -n RESOURCE_GROUP DEPLOYMENT_NAME -l LOCATION --template-uri TEMPLATE_URI
```

### <a name="provide-template-parameters"></a>Anna Malliparametrit

Tämä komento edellyttää parametrien määrittäminen vuorovaikutteisesti. Jos haluat lisätä parametreja, kuten JSON-muotoiset merkkijonon, voit tehdä niin käyttämällä `-p` vaihtaminen. Esimerkki:

 ```bash
azure group deployment create RESOURCE_GROUP DEPLOYMENT_NAME --template-uri TEMPLATE_URI -p '{ "param1": "value1" … }'
```

Vaihtoehtoisesti voit antaa JSON-muotoiset parametrit-tiedoston avulla `-e` vaihtaminen:

```bash
azure group deployment create RESOURCE_GROUP DEPLOYMENT_NAME --template-uri TEMPLATE_URI -e PATH/FILE.JSON
```

Näet parametrit Esimerkki-tiedosto nimeltä `azuredeploy.parameters.json`, etsi se GitHub Azure säilö Service-malleja.

## <a name="create-a-service-by-using-powershell"></a>Luo palvelu PowerShell-toiminnolla

Voit myös ottaa Azure säilö Service-klusterin PowerShellin avulla. Tämän asiakirjan perustuu versio 1.0 [PowerShellin Azure moduuli](https://azure.microsoft.com/blog/azps-1-0/).

Toimialueen Ohjauskoneen/OS tai Docker Swarm klusterin ottamaan Valitse jokin seuraavista malleista. Huomaa, että molemmat mallit ovat samat, lukuun ottamatta orchestrator oletusvalinta.

* [Toimialueen Ohjauskoneen/OS malli](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos)
* [Swarm malli](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm)

Ennen kuin luot klusterin Azure-tilaukseesi, varmista, että PowerShell-istunnon on allekirjoitettu Azure avulla. Voit tehdä tämän kanssa `Get-AzureRMSubscription` komento:

```powershell
Get-AzureRmSubscription
```

Jos haluat kirjautua Azure `Login-AzureRMAccount` komento:

```powershell
Login-AzureRmAccount
```

Jos olet ottaminen käyttöön uusi resurssiryhmä, sinun on luotava resurssiryhmän. Luo uusi resurssiryhmä `New-AzureRmResourceGroup` komento ja määrittää resurssin ryhmän nimi ja kohdetaulukon alueen:

```powershell
New-AzureRmResourceGroup -Name GROUP_NAME -Location REGION
```

Kun olet luonut resurssiryhmä, voit luoda oman klusterin seuraavalla komennolla. URI haluamasi mallin määritetylle `-TemplateUri` parametria. Kun suoritat tämän komennon, PowerShell kysyy käyttöönoton parametriarvot.

```powershell
New-AzureRmResourceGroupDeployment -Name DEPLOYMENT_NAME -ResourceGroupName RESOURCE_GROUP_NAME -TemplateUri TEMPLATE_URI
```

### <a name="provide-template-parameters"></a>Anna Malliparametrit

Jos olet tottunut käyttämään PowerShell, tiedät, että voit käy läpi cmdlet-komento käytettävissä parametrit kirjoittamalla miinusmerkki (-) ja painamalla sitten SARKAIN-näppäintä. Sama toiminto toimii myös mallin määrittävät parametrit. Heti, kun kirjoitat mallin nimi, cmdlet hakee mallin, jäsentää parametrit ja Lisää komento dynaamisesti Malliparametrit. Tämä on hyvin helppo mallia parametrin arvot. Ja jos unohdat tarvittavat parametrin arvon, PowerShell kehottaa sinua arvon.

Alla on koko-komento, sisällyttää parametreilla. Voit antaa oman arvot resurssien nimet.

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName RESOURCE_GROUP_NAME-TemplateURI TEMPLATE_URI -adminuser value1 -adminpassword value2 ....
```

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet luonut toimii klusterin, katso tiedostot-yhteyden ja hallinnan tiedot:

- [Yhteyden muodostaminen Azure säilö Service-klusterin](container-service-connect.md)
- [Azure säilö palvelun ja Ohjauskoneen/Käyttöjärjestelmä](container-service-mesos-marathon-rest.md)
- [Azure säilö palvelu ja Docker Swarm](container-service-docker-swarm.md)

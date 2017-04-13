<properties
    pageTitle="Siirtää IaaS resurssien perinteinen Azure resurssien hallinnan avulla Azure CLI | Microsoft Azure"
    description="Tässä artikkelissa käydään läpi resurssien ympäristö tuettu siirron classic-Azure resurssien hallinnan avulla Azure CLI"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/19/2016"
    ms.author="cynthn"/>

# <a name="migrate-iaas-resources-from-classic-to-azure-resource-manager-by-using-azure-cli"></a>Siirtää IaaS resurssien perinteinen Azure resurssien hallinnan avulla Azure CLI

Seuraavia ohjeita noudattamalla voit Azure käyttöliittymä (CLI) komentojen avulla voit siirtää infrastruktuurin service (IaaS) resursseina perinteinen käyttöönoton mallista Azure resurssien hallinnan käyttöönottomalli. Artikkelin edellyttää [Azure CLI](../xplat-cli-install.md).

>[AZURE.NOTE] Kaikki tässä kuvatut toiminnot ovat idempotent. Jos sinulla on ongelmia kuin ominaisuus jota ei tueta tai määritysvirhe, on suositeltavaa valmisteleminen, yritä uudelleen Keskeytä tai Vahvista toiminto. Ympäristön yritä sitten suorittaa toiminto uudelleen.

## <a name="step-1-prepare-for-migration"></a>Vaihe 1: Siirtoon valmistautuminen

Seuraavassa on joitakin parhaita käytäntöjä, joilla on suositeltavaa, kun arvioit siirretään IaaS resurssit perinteinen resurssien hallinta:

- Lue [luettelo ei ole tuettu määrityksiä tai ominaisuuksia](virtual-machines-windows-migration-classic-resource-manager.md). Jos sinulla ei ole tuettu määrityksiä tai ominaisuuksia käyttäviä näennäiskoneiden, on suositeltavaa odottaa ilmoitetaan ominaisuus/configuration-tuen. Voit vaihtoehtoisesti toiminto poistaa tai siirtää pois, määritys käyttöön siirron, jos sitä tarpeisiisi.
-   Jos on automatisoitu komentosarjoja tähän tarkoitukseen infrastruktuuri ja sovellusten käyttöönotto tänään, yritä luoda käyttämällä komentosarjoja siirron samalla testi-asetukset. Vaihtoehtoisesti voit määrittää otoksen ympäristöissä mukaan Azure-portaalissa.

## <a name="step-2-set-your-subscription-and-register-the-provider"></a>Vaihe 2: Määritä tilauksen ja rekisteröi toimittaja

Siirron skenaarioille haluat molemmat perinteinen ympäristön määrittäminen ja Resurssienhallinta. [Asenna Azure CLI](../xplat-cli-install.md) ja [Valitse tilauksen](../xplat-cli-connect.md).

Kirjaudu sisään-tiliisi.
    
    azure login

Valitse Azure tilaus käyttämällä seuraavaa komentoa.

    azure account set "<azure-subscription-name>"

>[AZURE.NOTE] Rekisteröinti on aikaväli, mutta se on tehtävä vain kerran, ennen kuin yrität siirron. Ilman rekisteröintiä näet seuraavan virhesanoman 

>   *BadRequest: Tilaus ei ole rekisteröity siirtoa varten.* 

Rekisteröi siirron resurssi-palvelussa seuraavassa-komennon avulla. Huomaa, että joissakin tapauksissa tämä komento aikakatkaistaan. Kuitenkin rekisteröinti onnistuu.

    azure provider register Microsoft.ClassicInfrastructureMigrate

Odota viisi minuuttia rekisteröinnin loppuun. Voit tarkistaa hyväksynnän tilan käyttämällä seuraavaa komentoa. Varmista, että RegistrationState on `Registered` ennen kuin jatkat.

    azure provider show Microsoft.ClassicInfrastructureMigrate

CLI voit vaihtaa `asm` tilassa.

    azure config mode asm

## <a name="step-3-make-sure-you-have-enough-azure-resource-manager-virtual-machine-cores-in-the-azure-region-of-your-current-deployment-or-vnet"></a>Vaihe 3: Varmista, että sinulla on tarpeeksi Azure Resurssienhallinta virtuaalikoneen sydämiä Azure alueen nykyisen käyttöönottoa tai VNET

Tässä vaiheessa sinun on vaihdettava `arm` tilassa. Toimi samoin seuraavalla komennolla.

```
azure config mode arm
```

Seuraavat CLI-komennon avulla voit tarkistaa käytössäsi Azure Resurssienhallinta sydämiä nykyisen määrän. Lisätietoja core kiintiön on artikkelissa [rajoitukset ja Azure Resurssienhallinta](../articles/azure-subscription-service-limits.md#limits-and-the-azure-resource-manager)

```
azure vm list-usage -l "<Your VNET or Deployment's Azure region"
```

Kun olet valmis tarkistetaan tässä vaiheessa voit siirtyä takaisin `asm` tilassa.

    azure config mode asm


## <a name="step-4-option-1---migrate-virtual-machines-in-a-cloud-service"></a>Vaihe 4: Vaihtoehto 1 - siirtää näennäiskoneiden pilvipalvelussa 

Hae pilvipalveluihin luettelo käyttämällä seuraava komento ja valitse sitten pilvipalvelussa, jonka haluat siirtää. Huomaa, että, jos pilvipalvelussa VMs ovat virtual verkossa tai jos ne on web/työntekijä roolit, näyttöön tulee virhesanoma.

    azure service list

Suorita seuraava komento hakee yksityiskohtaiset pilvipalvelussa sijaitsevaan käyttöönoton nimi. Useimmissa tapauksissa käyttöönoton nimi on sama kuin cloud palvelunimi.

    azure service show <serviceName> -vv

Valmistele näennäiskoneiden pilvipalvelussa siirtoa varten. Käytettävissä on kaksi vaihtoehtoista valittavana.

Jos haluat siirtää VMs ympäristö luodaan virtual verkkoon, käytä seuraavaa komentoa.

    azure service deployment prepare-migration <serviceName> <deploymentName> new "" "" ""

Jos haluat siirtää aiemmin virtual verkkoon mallin resurssien hallinnan käyttöönotto, käytä seuraavaa komentoa.

    azure service deployment prepare-migration <serviceName> <deploymentName> existing <destinationVNETResourceGroupName> subnetName <vnetName>

Kun valmistelu on tarkistettu, voit selata yksityiskohtainen tuloste hakee siirron tila VMs ja varmistaa, että ne ovat `Prepared` tila.

    azure vm show <vmName> -vv

Tarkista määritykset valmiita resurssien CLI tai Azure portaalin. Jos et ole valmis siirtoa varten ja haluat palata vanha tila, käytä seuraavaa komentoa.

    azure service deployment abort-migration <serviceName> <deploymentName>

Jos valmiita määritys näyttää hyvältä, voit siirtää eteenpäin ja vahvista resurssit seuraavat-komennon avulla.

    azure service deployment commit-migration <serviceName> <deploymentName>


    
## <a name="step-4-option-2----migrate-virtual-machines-in-a-virtual-network"></a>Vaihe 4: Vaihtoehto 2 – siirtää näennäiskoneiden virtual verkossa

Valitse virtual verkkoon, jonka haluat siirtää. Huomaa, että jos virtual verkon sisältää web/työntekijä roolien tai VMs määritykset, joita ei tueta, näyttöön tulee kelpoisuuden tarkistamisen virhesanoma.

Saat virtual verkkojen tilaus käyttämällä seuraavaa komentoa.

    azure network vnet list
    
Tulos näyttää tältä:

![Näyttökuva koko virtual verkko-niminen korostettuna komentoriviltä.](./media/virtual-machines-linux-cli-migration-classic-resource-manager/vnet.png)

Edellä olevassa esimerkissä **virtualNetworkName** on koko nimi **"Ryhmän classicubuntu16 classicubuntu16"**.

Virtuaalinen verkon valittua siirtoon valmistautuminen seuraavat-komennon avulla.

    azure network vnet prepare-migration <virtualNetworkName>

Tarkista määritykset valmiita näennäiskoneiden CLI tai Azure portaalin. Jos et ole valmis siirtoa varten ja haluat palata vanha tila, käytä seuraavaa komentoa.

    azure network vnet abort-migration <virtualNetworkName>

Jos valmiita määritys näyttää hyvältä, voit siirtää eteenpäin ja vahvista resurssit seuraavat-komennon avulla.

    azure network vnet commit-migration <virtualNetworkName>

## <a name="step-5-migrate-a-storage-account"></a>Vaihe 5: Siirrä tallennustilan-tili

Kun olet siirtänyt näennäiskoneiden, on suositeltavaa siirtää tallennustilan tilin.

Tallennustilan tilin valmisteleminen siirron seuraavat-komennon avulla

    azure storage account prepare-migration <storageAccountName>

Tarkista valmiita tallennustilan tilin määritys CLI tai Azure portaalin. Jos et ole valmis siirtoa varten ja haluat palata vanha tila, käytä seuraavaa komentoa.

    azure storage account abort-migration <storageAccountName>

Jos valmiita määritys näyttää hyvältä, voit siirtää eteenpäin ja vahvista resurssit seuraavat-komennon avulla.

    azure storage account commit-migration <storageAccountName>

## <a name="next-steps"></a>Seuraavat vaiheet

- [Tuetaan käyttöympäristö siirto IaaS resurssit klassinen resurssien hallinta](virtual-machines-windows-migration-classic-resource-manager.md)
- [Tekniset perinpohjaisesti käsittelevään artikkeliin-ympäristö tuettu siirron classic-resurssien hallinta](virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)

<properties
    pageTitle="Luo Linux-AM Azure mallin avulla | Microsoft Azure"
    description="Luo Linux AM Azure Azure Resurssienhallinta-mallin avulla."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="azure-service-management,azure-resource-manager" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/24/2016"
    ms.author="v-livech"/>

# <a name="create-a-linux-vm-using-an-azure-template"></a>Luo Linux-AM Azure mallin avulla

Tässä artikkelissa kerrotaan, miten voit nopeasti ottaa käyttöön Linux Virtual tietokoneen Azure Azure-mallin avulla.  Artikkelin edellyttää:

- Azure tili ([maksuton kokeiluversio](https://azure.microsoft.com/pricing/free-trial/)).

- sisään kirjautunut [Azure CLI](../xplat-cli-install.md) `azure login`.

- Azure CLI _on oltava_ Azure Resurssienhallinta tilassa `azure config mode arm`.

Linux AM mallin käyttöönottoa nopeasti myös [Azure portal](virtual-machines-linux-quick-create-portal.md).

## <a name="quick-command-summary"></a>Pika-komennon yhteenveto

```bash
azure group create \
-n myResourceGroup \
-l westus \
--template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

## <a name="detailed-walkthrough"></a>Laskun

Mallien avulla voit luoda VMs Azure asetuksia, jotka haluat mukauttaa käynnistämisen yhteydessä, asetusten, kuten käyttäjänimet ja isäntänimet. Microsoft on tämän artikkelin avaamista Azure-mallin toista Ubuntu-AM sekä verkon käyttöoikeusryhmän (NSG) käyttävien porttiin 22 Avaa SSH varten.

Azure Resurssienhallinta mallit ovat JSON-tiedostot, joita voi käyttää yksinkertaisten kertakäyttöisen tehtävien, kuten avaamista Ubuntu AM tämän artikkelin tehdyksi.  Azure mallien avulla voidaan myös käyttää monimutkaisia Azure käyttömahdollisuudet koko ympäristöjen, kuten testaus, keskihajonta tai tuotannon käyttöönotto-pino.

## <a name="create-the-linux-vm"></a>Luo Linux AM

Koodin seuraavassa esimerkissä esitetään, miten voit soittaa `azure group create` luoda resurssiryhmän ja otetaan käyttöön SSH suojattu Linux-AM samanaikaisesti [Azure Resurssienhallinta tämän mallin](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json)avulla. Muista, että esimerkissä, sinun on käytettävä nimiä, jotka ovat yksilöllisiä-ympäristöön. Tässä esimerkissä käytetään `myResourceGroup` resurssi-ryhmän nimen ja `myVM` AM nimeksi.

```bash
azure group create \
--name myResourceGroup \
--location westus \
--template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

Tulosteen pitäisi näyttää samalta kuin seuraava tulos lohko:

```bash
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Creating resource group myResourceGroup
info:    Created resource group myResourceGroup
info:    Supply values for the following parameters
sshKeyData: ssh-rsa AAAAB3Nza<..ssh public key text..>VQgwjNjQ== myAdminUser@myVM
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "azuredeploy"
data:    Id:                  /subscriptions/<..subid text..>/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

Kyseisen Esimerkki käyttöön AM avulla `--template-uri` parametria.  Voit myös ladata tai paikallisesti mallin luominen ja välittää mallin avulla `--template-file` parametrin argumenttina mallitiedoston polku. Azure-CLI pyytää mallin vaatii parametrit.

## <a name="next-steps"></a>Seuraavat vaiheet

Etsi [mallivalikoimasta](https://azure.microsoft.com/documentation/templates/) saat tietää, mitä sovelluksen kehysten ottamaan Seuraava.

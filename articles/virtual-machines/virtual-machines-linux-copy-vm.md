<properties
    pageTitle="Azure-Linux AM kopion luominen | Microsoft Azure"
    description="Opettele luomaan kopion Azure Linux virtuaalikoneen resurssien hallinnan käyttöönottomalli"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="cynthn"/>

# <a name="create-a-copy-of-a-linux-virtual-machine-running-on-azure"></a>Azure-käyttöjärjestelmässä Linux virtual koneen kopion luominen


Tämän artikkelin avulla voit luoda kopion Azure virtuaalikoneen (AM) käynnissä Linux käyttämällä resurssien hallinnan käyttöönottomalli. Ensin voit kopioida uuteen säilöön käyttöjärjestelmän ja tietojen levyjen sitten verkkoresursseja määrittäminen ja luo uusi virtuaalikoneen.

Voit myös [ladata ja luo mukautettuja levyn kuvasta AM](virtual-machines-linux-upload-vhd.md).


## <a name="before-you-begin"></a>Ennen aloittamista

Varmista, että seuraavat edellytykset täyttyvät, ennen kuin aloitat vaiheet:

- [Azure CLI] on (... / xplat cli-install.md) ladataan ja asennetaan tietokoneeseen. 

- Sinun on myös tietoja aiemmin Azure-Linux AM tietoja:

| AM lähdetiedot | Mistä sellaisen saat |
|------------|-----------------|
| AM nimi | `azure vm list` |
| Resurssiryhmä-nimi | `azure vm list` |
| Sijainti | `azure vm list` |
| Tallennustilan tilin nimi | `azure storage account list -g <resourceGroup>` |
| Säilön nimi | `azure storage container list -a <sourcestorageaccountname>` |
| Lähdetiedoston AM Näennäiskiintolevyn nimi | `azure storage blob list --container <containerName>` |



- Sinun on joitakin asetusta uusi AM tietoja:   <br> -Säilön nimi   <br> AM - nimi   <br> -AM kokoa   <br> -vNet nimi   <br> -Aliverkon nimi   <br> -IP nimi   <br> -NIC nimi
    

## <a name="login-and-set-your-subscription"></a>Kirjaudu sisään ja määritä tilauksen

1. Kirjaudu CLI.
        
        azure login

2. Varmista, että Resurssienhallinta-tilassa.
    
        azure config mode arm

3. Määritä oikea tilaus. Voit valita azure-tili-luettelosta kaikki tilauksistasi näkyviin.

        azure account set <SubscriptionId>



## <a name="stop-the-vm"></a>Lopeta AM 

Lopeta ja poistaa varauksen lähteen AM. Voit käyttää azure AM luettelo-näkymäksi saat näkyviin luettelon kaikista tilauksen ja niiden resurssin VMs ryhmien nimet.
    
        azure vm stop <ResourceGroup> <VmName>
        azure vm deallocate <ResourceGroup> <VmName>




## <a name="copy-the-vhd"></a>Kopioi Näennäiskiintolevy


Voit kopioida Näennäiskiintolevyn lähde-tallennustilan kohde käyttäen `azure storage blob copy start`. Tässä esimerkissä tarkastellaan kopioi Näennäiskiintolevy tallennustilan samaan tiliin, mutta eri säilöön.

Voit kopioida Näennäiskiintolevyn saman tilin tallennustilan säilö, kirjoittamalla:

        azure storage blob copy start https://<sourceStorageAccountName>.blob.core.windows.net:8080/<sourceContainerName>/<SourceVHDFileName.vhd> <newcontainerName>
        

## <a name="set-up-the-virtual-network-for-your-new-vm"></a>Uusi AM virtual verkon määrittäminen

Määritä VPN ja NIC uusi AM. 

    azure network vnet create <ResourceGroupName> <VnetName> -l <Location>

    azure network vnet subnet create -a <address.prefix.in.CIDR/format> <ResourceGroupName> <VnetName> <SubnetName>

    azure network public-ip create <ResourceGroupName> <IpName> -l <yourLocation>

    azure network nic create <ResourceGroupName> <NicName> -k <SubnetName> -m <VnetName> -p <IpName> -l <Location>


## <a name="create-the-new-vm"></a>Luo uusi AM 

Voit nyt luoda oman ladatut virtual levyn [resurssien hallinnan mallin avulla](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd) tai kautta CLI AM määrittämällä kopioitu levylle URI kirjoittamalla:

```bash
azure vm create -n <newVMName> -l "<location>" -g <resourceGroup> -f <newNicName> -z "<vmSize>" -d https://<storageAccountName>.blob.core.windows.net/<containerName/<fileName.vhd> -y Linux
```



## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja Azure CLI avulla voit hallita uuden virtuaalikoneen on artikkelissa [Azure CLI komentoja varten Azure Resurssienhallinta](azure-cli-arm-commands.md).

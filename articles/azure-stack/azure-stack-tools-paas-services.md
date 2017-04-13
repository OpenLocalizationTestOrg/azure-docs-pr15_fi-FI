<properties
    pageTitle="Työkalut ja PaaS palvelut Azure pinon | Microsoft Azure"
    description="Opettele PaaS services Azure Pinotut käytön aloittaminen."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="erikje"/>

# <a name="tools-for-azure-stack"></a>Azure pinon Työkalut

Voit ladata Työkalut seuraavalla tavalla. Jos haluat ilmoittaa uusia palveluja ja työkaluja, noudata #AzureStack Twitter.

## <a name="template-tools"></a>Mallin Työkalut

### <a name="azure-stack-github-templates"></a>Azure pinon Github mallit
Tutustu [Azure pinon GitHub malleja](https://github.com/Azure/AzureStack-QuickStart-Templates)valikoiman. [Azure](https://github.com/Azure/azure-quickstart-templates), kuten "Pika-aloitus" Azure Resurssienhallinta-mallien pyritään yksinkertainen rakenneosat- ja esimerkkejä on valmis ottamaan Microsoft Azure pinon tekninen esikatselu kuitti, joka kertoo ympäristöön hyvin alkuun. Luetaan ensimmäisen osapuolen työmäärää esimerkkejä [ad-ei-ha](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/ad-non-ha)- [sql-2014-ei-ha](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/sql-2014-non-ha), [sharepoint-2013-ei-ha](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/sharepoint-2013-non-ha), kuten myös useita yksinkertaisia 101 malleja, kuten [101-yksinkertainen-windows-AM](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-simple-windows-vm).


### <a name="marketplace-item-packaging-tool-and-sample"></a>Marketplace kohteen pakkaus työkalu ja Esimerkki
[Lataa ja pakkaus-työkalulla](http://www.aka.ms/azurestackmarketplaceitem) voit luoda omia mukautettuja malleja pinon Azure marketplace lisääminen marketplace-kohteita. Ohjeet marketplace kohteen luominen ja sen lisääminen vuokraajiin löytyy [Luominen Marketplace](azure-stack-create-and-publish-marketplace-item.md)-kohteen.

## <a name="developer-tools"></a>Kehitystyökalut


### <a name="use-visual-studio-and-azure-stack-tp2-on-the-mas-con01-virtual-machine"></a>Käyttää Visual Studio ja Azure pinon TP2 MAS CON01 virtuaalikoneen
Jos haluat käyttää Azure pinon malleja konsolissa AM Visual Studion avulla, sinun on asennettava tarvittavat työkalut oikeat versiot. Seuraavien ohjeiden avulla voit asentaa tuetut versiot TP2.

1. Etätyöpöytäyhteys avulla voit kirjautua MAS CON01 virtuaalikoneen azurestack\azurestackadmin tunnuksilla.
2. Asenna ja avaa Web-ympäristö asennusohjelma.
3. Etsi ja asenna **Visual Studio yhteisön 2015 Microsoft Azure SDK - 2.9.5 kanssa**.
4. **Microsoft Azure PowerShell (syyskuuhun)** , joka saa asennetaan SDK käyttämällä **Lisää tai poista sovellus**-asennuksen.
5. Avaa Web-ympäristö asennusohjelma.
6. Etsi ja asenna **Microsoft Azure PowerShell - Azure pinon tekninen esikatselu 2**. 
7. Käynnistä MAS CON01 virtuaalikoneen.
8. Etätyöpöytäyhteys avulla voit kirjautua MAS CON01 virtuaalikoneen azurestack\azurestackadmin tunnuksilla.
9. Avaa Visual Studio ja vahvista, että voit muodostaa Azure pino-ympäristössä, Hae malleja ja niin edelleen. 

### <a name="azure-powershell-sdk"></a>Azure PowerShell SDK-paketissa
Azure PowerShell on moduuli, joka sisältää cmdlet-komentojen Azure- ja Windows PowerShellin Azure pinossa. Voit Cmdlet-komentoja avulla voit luoda, Testaa, ottaminen käyttöön ja hallita ratkaisuihin ja palveluihin pinon Azure-ympäristön kautta.
[Lataa Azure PowerShell SDK](http://aka.ms/azStackPsh) ja [PowerShellin asentaminen](azure-stack-connect-powershell.md).

### <a name="azure-cross-platform-command-line-interfaces"></a>Azure cross platform komento rivin liityntäkohdat
Asenna nopeasti Azure käyttöliittymä (Azure CLI) käyttämään Avaa lähde shell-pohjainen komentojen luomisen ja ylläpidon Microsoft Azure Pinotut resurssit.

[Lataa Windows CLI](http://aka.ms/azstack-windows-cli)

[Lataa Mac CLI](http://aka.ms/azstack-linux-cli)

[Lataa Linux CLI](http://aka.ms/azstack-mac-cli)

>[AZURE.NOTE]
>
> + Jos olet Mac tai Linux-laitteeseen, saat myös CLI-komennon avulla`npm install -g azure-cli@0.9.11`</br>
> + Jos näyttöön tulee varmenteen vahvistuksen havaitsemien virheiden, suorita-komento`set NODE_TLS_REJECT_UNAUTHORIZED=0`

<properties
    pageTitle="Azure Resurssienhallinta mallia Azure Pinotut (vuokraajan kehittäjät) | Microsoft Azure"
    description="Opettele käyttämään Azure Resurssienhallinta malleja Azure Pinotut käyttöönotto ja valmistella kaikkien resurssien sovelluksen yhteen, koordinoidun toiminnossa."
    services="azure-stack"
    documentationCenter=""
    authors="heathl17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="helaw"/>

# <a name="use-azure-resource-manager-templates-in-azure-stack"></a>Azure Resurssienhallinta mallia Azure Pinotut

Azure Resurssienhallinta-mallit käyttöön ja valmistella kaikkien resurssien sovelluksen yhteen, koordinoidun toiminnossa. Voit määrittää resurssit ja miten se otetaan käyttöön.  Voit myös Ota halutaan muuttaa resursseja resurssiryhmän malleja uudelleen.

Mallit voidaan ottaa käyttöön Microsoft Azure pino-portaalissa, PowerShell, komentoriviltä ja Visual Studio.

[AZURE.VIDEO microsoft-azure-stack-tp1--foundational-skills-1-deploying-json-templates]

Seuraavat mallit ovat käytettävissä [GitHub](http://aka.ms/azurestackgithub):

## <a name="deploy-sharepoint-non-high-availability"></a>Ottaa käyttöön SharePoint (suuri saatavuuden)

PowerShellin DSC tunnisteen avulla voit luoda SharePoint 2013-klusterissa, joka sisältää seuraavat:

-   Virtuaalinen verkossa

-   Kolme tallennustilan-tiliä

-   Kaksi ulkoisen kuormituksen tasoitusmääritykset

-   Yksi uusi-metsää yksittäisen toimialueella ohjauskoneen määritetty AM

-   SQL Server 2014 erilliseksi palvelimeksi määritetty yksi AM

-   Yksi AM määritetty tietokoneesta SharePoint 2013-klusterissa

## <a name="deploy-ad-non-high-availability"></a>Ottaa käyttöön AD (suuri saatavuuden)

PowerShell DSC-tunniste avulla voit luoda AD-toimialueen ohjauskoneen palvelin, joka sisältää seuraavat:

-   Virtuaalinen verkossa

-   Tallennustilan tilien

-   Ulkoinen kuormituksen

-   Yksi uusi-metsää yksittäisen toimialueella ohjauskoneen määritetty AM

## <a name="deploy-adsql-non-high-availability"></a>Ottaa käyttöön AD/SQL (suuri saatavuuden)

PowerShell DSC-tunniste avulla voit luoda SQL Server 2014 itsenäisen palvelimen, joka sisältää seuraavat:

-   Virtuaalinen verkossa

-   Kaksi tallennustilan tilit

-   Ulkoinen kuormituksen

-   Yksi uusi-metsää yksittäisen toimialueella ohjauskoneen määritetty AM

-   SQL Server 2014 erilliseksi palvelimeksi määritetty yksi AM

## <a name="vm-dsc-extension-azure-automation-pull-server"></a>VM-DSC-Extension-Azure-Automation-Pull-Server

PowerShellin DSC tunnisteen avulla voit määrittää aiemmin virtuaalikoneen paikallisen määritysten hallinta (pienin.yht.JAETTAVA) ja rekisteröi Azure automaatio tilin DSC erotettu palvelimeen.

## <a name="create-a-virtual-machine-from-a-user-image"></a>Luo virtual machine käyttäjän kuvasta

Luo mukautettuja kuvasta virtual machine. Tämä malli otetaan käyttöön myös (DNS) virtual verkosta, julkiseen IP-osoite ja Verkkokäyttöliittymän.

## <a name="simple-vm"></a>Yksinkertainen AM

Ota käyttöön yksinkertainen Windows-AM, joka sisältää (DNS) virtual verkosta, julkiseen IP-osoite ja Verkkokäyttöliittymän.

## <a name="cancel-a-running-template-deployment"></a>Peruuta käynnissä mallin käyttöönotto

Voit peruuttaa käynnissä malli-käyttöönotto `Stop-AzureRmResourceGroupDeployment` PowerShell cmdlet-komennon.


## <a name="next-steps"></a>Seuraavat vaiheet

[Ottaa käyttöön mallit-portaalissa](azure-stack-deploy-template-portal.md)

[Azure resurssin hallinnassa: yleiskatsaus](../azure-resource-manager/resource-group-overview.md)


<properties
    pageTitle="Komentorivin Azure Pinotut mallien käyttöönotto | Microsoft Azure"
    description="Opettele käyttämään Office kaikissa ympäristöissä komento rivin interface (CLI) ottamaan mallit ClientVM tai kun Azure pinon muodostaa VPN-yhteys."
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
    ms.date="09/26/2016"
    ms.author="helaw"/>

# <a name="deploy-templates-in-azure-stack-using-the-command-line"></a>Mallien Azure Pinotut komentoriviltä käyttöönotto

Käytä komentorivillä Azure Resurssienhallinta-mallit käyttöön Azure pinon Käsitteiden. Azure Resurssienhallinta-mallit käyttöön ja valmistella yhteen, koordinoidun toiminnossa sovelluksen resurssit.

## <a name="download-template"></a>Mallin lataaminen        
Voit esikatsella ympäristö, jonka CLI- [tallennustilan tilin Esimerkki mallin luominen](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-create-storage-account)tiedostojen azuredeploy.json ja azuredeploy.parameters.json lataaminen.

## <a name="deploy-template"></a>Mallin ottaminen käyttöön
Etsi kansio, johon tiedostot on ladattu ja suorittamalla seuraavan komennon ottaa mallin käyttöön:

    azure group create "cliRG" "local" –f azuredeploy.json –d "testDeploy" –e azuredeploy.parameters.json

Tämä komento ottaa mallin käyttöön resurssi ryhmän **cliRG** Azure pinon Käsitteiden oletussijainti.

## <a name="validate-template-deployment"></a>Vahvista mallin käyttöönotto
Saat tämän resurssin ryhmittely ja tallennustilaa tilin, käytä seuraavia komentoja:

    azure group list

    azure storage account list

## <a name="next-steps"></a>Seuraavat vaiheet

[Käyttöoikeuksien hallinta](azure-stack-manage-permissions.md)

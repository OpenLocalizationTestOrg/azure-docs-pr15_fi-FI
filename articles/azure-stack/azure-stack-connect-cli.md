<properties
    pageTitle="Yhteyden muodostaminen Azure pinon CLI kanssa | Microsoft Azure"
    description="Lue, miten voit hallita ja asentaa Azure pinon resurssien Office kaikissa ympäristöissä komentorivivalitsimet interface (CLI) avulla"
    services="azure-stack"
    documentationCenter=""
    authors="HeathL17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="helaw"/>

# <a name="install-and-configure-azure-stack-cli"></a>Asenna ja Määritä Azure pinon CLI

Tässä asiakirjassa on opastaa käyttäminen hallittavan Azure pinon resurssien Linux- ja Mac-asiakasympäristöjen Azure käyttöliittymä (CLI).  

## <a name="install-azure-stack-cli"></a>Asenna Azure pinon CLI

Jos olet Mac tai Linux, voit hankkia CLI käyttämällä seuraava komento:
  
    `npm install -g azure-cli@0.10.4`.


## <a name="connect-to-azure-stack"></a>Yhteyden muodostaminen Azure pino
Seuraavissa vaiheissa määrität Azure CLI muodostaa Azure pinossa. Valitse Kirjaudu sisään ja tilauksen tietojen hakemiseen.

1.  Hae suorittamalla tämän PowerShell active directory-resurssi-tuotetunnus-arvo:
        
          (Invoke-RestMethod -Uri https://api.azurestack.local/metadata/endpoints?api-version=1.0 -Method Get).authentication.audiences[0]

2.  Seuraavan CLI-komennon avulla voit lisätä Azure pinon ympäristön varmistetaan, että voit päivittää *--active directory-resurssi-tuotetunnus-* tiedoilla URL-Osoitteen noutaminen edellisessä vaiheessa:

           azure account env add AzureStack --resource-manager-endpoint-url "https://api.azurestack.local" --management-endpoint-url "https://api.azurestack.local" --active-directory-endpoint-url  "https://login.windows.net" --portal-url "https://portal.azurestack.local" --gallery-endpoint-url "https://portal.azurestack.local" --active-directory-resource-id "https://azurestack.local-api/" --active-directory-graph-resource-id "https://graph.windows.net/"

3.  Kirjaudu sisään käyttämällä seuraavaa komentoa (korvaa *käyttäjänimi* käyttäjänimelläsi):

        azure login -e AzureStack -u “<username>”

    >[AZURE.NOTE]Jos näyttöön tulee varmenteen vahvistuksen havaitsemien virheiden, poistaa käytöstä varmenteen kelpoisuuden suorittamalla komennon `set        NODE_TLS_REJECT_UNAUTHORIZED=0`.

4.  Määritä Azure määritystilassa Azure resurssien hallinta käyttämällä seuraava komento:

        azure config mode arm

5.  Hae luettelo tilaukset.

        azure account list     

## <a name="next-steps"></a>Seuraavat vaiheet

[Mallien Azure CLI käyttöönotto](azure-stack-deploy-template-command-line.md)

[Yhdistä PowerShellin avulla](azure-stack-connect-powershell.md)

[Käyttöoikeuksien hallinta](azure-stack-manage-permissions.md)

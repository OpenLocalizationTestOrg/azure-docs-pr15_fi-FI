<properties
    pageTitle="Ottaa käyttöön AM, Azure pinon avain säilöön tallennettuja salasanalla | Microsoft Azure"
    description="Opi ottamaan AM, Azure pinon avain säilöön tallennettuja salasanalla"
    services="azure-stack"
    documentationCenter=""
    authors="rlfmendes"
    manager="natmack"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/26/2016"
    ms.author="ricardom"/>

# <a name="deploy-a-vm-by-retrieving-the-password-stored-in-key-vault"></a>Ottaa käyttöön AM hakemalla avain säilöön salasanan

Kun haluat siirtää suojatun arvo (kuten salasanaa) parametrina käyttöönoton aikana, voit tallentaa arvon salaisuus avaimen Azure pino-säilöön ja Azure Resurssienhallinta muiden mallien arvo viittaa. Toiminta viittaa sisällyttäminen malliin, joten toiminta on määritetty ei koskaan. Sinun ei tarvitse manuaalisesti Anna arvo toiminta aina, kun otat resurssit. Määritä, ketkä käyttäjät tai palvelun ansaitun käyttää toiminta.

## <a name="reference-a-secret-with-static-id"></a>Salainen, jonka tunnus on staattinen viittaus

Voit viitata toiminta-alueella parametrit-tiedosto, joka välittää arvot mallin. Toiminta viitata kulkeva avaimen säilö resurssitunnus ja toiminta nimi. Tässä esimerkissä avaimen säilö toiminta on jo olemassa. Staattinen arvo käyttäminen sen resurssin tunnus.

    "parameters": {
    "adminPassword": {
    "reference": {
    "keyVault": {
    "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
    },
    "secretName": "sqlAdminPassword"


>[AZURE.NOTE]Parametri, joka hyväksyy toiminta on oltava *securestring*.

## <a name="next-steps"></a>Seuraavat vaiheet
[Esimerkki sovelluksen kanssa avaimen säilö käyttöönotto](azure-stack-kv-sample-app.md)

[AM avaimen säilö sertifikaatilla käyttöönotto](azure-stack-kv-push-secret-into-vm.md)


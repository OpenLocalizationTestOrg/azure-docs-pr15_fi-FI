<properties
    pageTitle="Määrittää avaimen säilö näennäiskoneiden Azure Resurssienhallinta | Microsoft Azure"
    description="Voit määrittää avaimen säilö Azure Resurssienhallinta-virtuaalikoneen käytettäväksi."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/31/2016"
    ms.author="singhkay"/>

# <a name="set-up-key-vault-for-virtual-machines-in-azure-resource-manager"></a>Määritä avaimen säilö näennäiskoneiden Azure Resurssienhallinta

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]perinteinen käyttöönottomalli

Azure Resurssienhallinta Pinotut tietoja/varmenteet mallintaa resursseina, jotka ovat resurssin tarjoajan avaimen säilö. Lisätietoja Azure avaimen säilö on artikkelissa [Azure avaimen säilö ominaisuudet?](../key-vault/key-vault-whatis.md)

Avaimen säilö, jota käytetään Azure Resurssienhallinta näennäiskoneiden tilauksen, avaimen säilö *EnabledForDeployment* -ominaisuuden asetukseksi on määritetty tosi. Tee tämä eri asiakassovelluksissa."

## <a name="use-cli-to-set-up-key-vault"></a>Määrittää avaimen säilö CLI avulla
Voit luoda avaimen säilö komentorivivalitsimet interface (CLI)-kohdassa [hallinta avaimen säilö käyttäminen CLI](../key-vault/key-vault-manage-with-cli.md#create-a-key-vault).

Saat CLI sinun on luotava avaimen säilö, ennen kuin määrität käyttöönotto-käytäntö. Voit tehdä tämän käyttämällä seuraava komento:

    azure keyvault set-policy ContosoKeyVault –enabled-for-deployment true

## <a name="use-templates-to-set-up-key-vault"></a>Mallien avulla voit määrittää avaimen säilö
Kun mallina, sinun on määritettävä `enabledForDeployment` -ominaisuuden arvoksi `true` avaimen säilö resurssin.

    {
      "type": "Microsoft.KeyVault/vaults",
      "name": "ContosoKeyVault",
      "apiVersion": "2015-06-01",
      "location": "<location-of-key-vault>",
      "properties": {
        "enabledForDeployment": "true",
        ....
        ....
      }
    }

Muita vaihtoehtoja, joita voit määrittää, kun luot avaimen säilö mallien avulla Katso [luominen avaimen säilö](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).

<properties services="virtual-machines" title="Setting up Azure CLI for service management" authors="squillace" solutions="" manager="timlt" editor="tysonn" />

<tags
   ms.service="virtual-machine"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="linux"
   ms.workload="infrastructure"
   ms.date="04/13/2015"
   ms.author="rasquill" />

## <a name="using-azure-cli"></a>Azure CLI käyttäminen

Seuraavat vaiheet avulla voit käyttää Azure CLI helposti uusimpaan versioon ja ERISNIMI tilaus. Jos haluat asentaa Azure CLI ja muodostaa yhteyden tiliisi ensimmäisen kerran, katso [Azure käyttöliittymä (Azure CLI)](xplat-cli-install.md).

### <a name="step-1-update-azure-cli-version"></a>Vaihe 1: Päivitä Azure CLI versio

Käytettävä välttämättömien komennot hallinta tilassa Azure CLI pitäisi olla uusimman version Jos mahdollista. Voit tarkistaa version, kirjoita `azure --version`. Näyttöön tulee suunnilleen:

    $ azure --version
    0.8.17 (node: 0.10.25)

Jos haluat päivittää Azure CLI on artikkelissa [Azure CLI](https://github.com/Azure/azure-xplat-cli).

### <a name="step-2-set-the-azure-account-and-subscription"></a>Vaihe 2: Määritä Azure-tili ja tilauksen

Kun olet muodostanut Azure CLI tilillä, jota haluat käyttää, voi olla useita tilauksia. Jos teet, tarkista tilaukset, jotka ovat käytettävissä tilissäsi kirjoittamalla `azure account list`, ja valitse sitten tilauksen, jota haluat käyttää kirjoittamalla `azure account set <subscription id or name> true` missä _tilauksen tunnus tai nimi_ on tilauksen tunnus tai tilauksen nimi, jota haluat käsitellä nykyisessä istunnossa. Näyttöön tulee seuraavanlaiselta:

    $ azure account set "Visual Studio Ultimate with MSDN" true
    info:    Executing command account set
    info:    Setting subscription to "Visual Studio Ultimate with MSDN" with id "0e220bf6-5caa-4e9f-8383-51f16b6c109f".
    info:    Changes saved
    info:    account set command OK

> [AZURE.NOTE] Jos sinulla ei vielä ole Azure-tili, mutta sinulla on MSDN-tilaukseen, saat ilmaisia Azure hyvitykset aktivoimalla [MSDN tilaajan edut tähän](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) --tai vapaan tiliä voi käyttää. Joko toimii Azure käytön.

<properties
   pageTitle="Koon muuttaminen Linux AM | Microsoft Azure"
   description="Voit laajentaa tai skaalata alaspäin Linux virtual-koneen AM koon muuttaminen."
   services="virtual-machines-linux"
   documentationCenter="na"
   authors="mikewasson"
   manager="timlt"
   editor=""
   tags=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/16/2016"
   ms.author="mikewasson"/>


# <a name="how-to-resize-a-linux-vm"></a>Linux AM koon muuttaminen

## <a name="overview"></a>Yleiskatsaus 

Kun valmistelu virtual machine (AM), voit skaalata AM ylös tai alas [AM kokoa]muuttamalla[vm-sizes]. Joissakin tapauksissa sinun täytyy poistaa varauksen AM ensin. Näin voi käydä, jos uusi koko ei ole käytettävissä, joka isännöi AM laitteisto-klusterin.

Tässä artikkelissa kerrotaan koon muuttaminen käyttämällä [Azure CLI]Linux-AM[azure-cli].

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]perinteinen käyttöönoton mallia.


## <a name="resize-a-linux-vm"></a>Linux AM koon muuttaminen 

Koon muuttaminen AM toimimalla seuraavasti.

1. Suorita seuraava komento CLI. Tämä komento on lueteltu AM koot, jotka ovat käytettävissä mihin AM nykyisessä laitteisto-klusterin.

    ```
    azure vm sizes -g <resource-group> --vm-name <vm-name>
    ```

2. Jos luettelossa on halutun kokoinen, suorita seuraava komento koon AM.

    ```
    azure vm set -g <resource-group> --vm-size <new-vm-size> -n <vm-name>  
        --enable-boot-diagnostics --boot-diagnostics-storage-uri
        https://<storage-account-name>.blob.core.windows.net/ 
    ```

    Vaihdon käynnistetään uudelleen AM. Uudelleenkäynnistyksen jälkeen aiemmin OS ja tietojen levyjen voidaan määrittää uudelleen. Mitään tilapäinen levyllä, menetetään.

    Käytä `--enable-boot-diagnostics` valinnalla [käynnistyksen diagnostiikka][boot-diagnostics], kirjaudu käynnistys liittyvien virheiden.

3. Jos haluamasi kokoiseksi ei näy luettelossa, suorita seuraavat komennot poistaa varauksen AM, muuttaa sen kokoa ja Käynnistä AM.

    ```
    azure vm deallocate -g <resource-group> <vm-name>
    azure vm set -g <resource-group> --vm-size <new-vm-size> -n <vm-name>  
        --enable-boot-diagnostics --boot-diagnostics-storage-uri
        https://<storage-account-name>.blob.core.windows.net/ 
    azure vm start -g <resource-group> <vm-name>
    ```

   > [AZURE.WARNING] Dynaaminen IP-osoitteita AM myönnetyt myös vapauttamista AM versiot. OS ja tietoja-levyjen näin ei käy.
   
## <a name="next-steps"></a>Seuraavat vaiheet

Lisää skaalattavuus suorittaa useita AM esiintymät ja skaalata ulos. Lisätietoja on artikkelissa [Skaalaa automaattisesti Linux koneet virtuaalikoneen-asteikko-joukko][scale-set]. 

<!-- links -->
   
[azure-cli]: ../xplat-cli-install.md
[boot-diagnostics]: https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/
[scale-set]: ../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md 
[vm-sizes]: virtual-machines-linux-sizes.md
<properties
    pageTitle="Perinteinen Windows-AM kokoa | Microsoft Azure"
    description="Muuta Windows virtual machine-perinteinen käyttöönoton mallissa Azure PowerShellin avulla luotu."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="Drewm3"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="drewm"/>


# <a name="resize-a-windows-vm-created-in-the-classic-deployment-model"></a>Windows-AM luotu perinteinen käyttöönoton mallin koon muuttaminen

Tässä artikkelissa kerrotaan Windows AM perinteinen käyttöönoton mallin Azure PowerShellin avulla luotu koon muuttaminen.

Kun mahdollisuus kokoa AM, on kahdesta käsitteestä, jotka ohjaavat koot käytettävissä koon virtuaalikoneen solualue. Ensimmäinen on alue, jossa oman AM on otettu käyttöön. AM-koot käytettävissä alueella on kohdassa Azure alueiden verkkosivun palvelut-välilehti. Toinen on fyysinen laitteisto isännöinnin tällä hetkellä oman AM. Fyysinen palvelinten isännöinnin VMs on ryhmitelty klustereiden yleisiä fyysinen laite. Maksutavan AM koon muuttaminen vaihtelee sen mukaan, jos reunaviivaa uusi AM tukee tällä hetkellä isännöivän AM laitteisto-klusterin.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Voit myös [muuttaa AM, joka on luotu resurssien hallinnan käyttöönottomalli](virtual-machines-windows-resize-vm.md).


## <a name="add-your-account"></a>Tilin lisääminen

Sinun on määritettävä PowerShellin Azure perinteinen Azure resurssien käyttöä varten. Noudata seuraavia ohjeita voit määrittää perinteinen resurssien PowerShellin Azure.

1. Kirjoita komentoriville seuraava PowerShell `Add-AzureAccount` ja sitten **ENTER-näppäintä**. 
2. Kirjoita sähköpostiosoite ja Azure-tilaus ja valitse **Jatka**. 
3. Kirjoita tilin salasana. 
4. Valitse **Kirjaudu sisään**. 


## <a name="resize-in-the-same-hardware-cluster"></a>Sama laitteisto-klusterin koon muuttaminen

Seuraavien toimien koon AM käytettävissä isännöivän AM laitteisto-klusterin koon muuttaminen.

1. Suorita seuraava PowerShell-komento käytettävissä isännöinnin pilvipalvelussa, joka sisältää AM laitteisto-klusterin AM koot on lueteltu.

    ```powershell
    Get-AzureService | where {$_.ServiceName -eq "<cloudServiceName>"}
    ```

2. Suorita seuraavat komennot koon AM.

    ```powershell
    Get-AzureVM -ServiceName <cloudServiceName> -Name <vmName> | Set-AzureVMSize -InstanceSize <newVMSize> | Update-AzureVM
    ```

## <a name="resize-on-a-new-hardware-cluster"></a>Laitteisto-klusterissa koon muuttaminen

Koon muuttaminen ei ole käytettävissä isännöinnin AM pilveen laitteisto-klusterin koon AM palvelu ja kaikki pilvipalvelussa VMs on luotava uudelleen. Kunkin pilvipalvelussa isännöidään yksittäisen laitteisto-klusterin, joten kaikki VMs pilvipalvelussa on koko, joka on tuettu laitteisto-klusterin. Seuraavat vaiheet kuvataan koon muuttaminen AM poistamalla ja luotaessa pilvipalvelussa.

1. Suorittaa seuraavan PowerShell komennon käytettävissä AM koot, alueella on lueteltu. 

    ```powershell
    Get-AzureLocation | where {$_.Name -eq "<locationName>"}
    ```

2. Kaikki asetukset kunkin AM pilvipalvelussa, joka sisältää kokoa AM muistiin. 
3. Poista kaikki VMs valitsemalla vaihtoehto, jos haluat säilyttää levyjen kunkin AM pilvipalvelussa.
4. Luo kokoa käyttämällä reunaviivaa AM AM.
5. Luo kaikki muut VMs käyttämällä AM koko on käytettävissä isännöintipalvelu cloud nyt laitteisto-klusterin pilvipalvelussa olleisiin.

Esimerkki komentosarjan poistaminen ja luominen uudelleen käyttämällä uusi AM koko pilvipalveluun löytyy [tähän](https://github.com/Azure/azure-vm-scripts). 


## <a name="next-steps"></a>Seuraavat vaiheet

- [Muuta kokoa AM, joka on luotu resurssien hallinnan käyttöönottomalli](virtual-machines-windows-resize-vm.md).
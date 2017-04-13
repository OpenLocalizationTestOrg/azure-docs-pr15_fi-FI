<properties
    pageTitle="Tietoja Windows näennäiskoneiden kuvat | Microsoft Azure"
    description="Lue lisätietoja siitä, miten kuvia voi käyttää näennäiskoneiden Windows Azure-tietokannassa."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/21/2016"
    ms.author="cynthn"/>

# <a name="about-images-for-windows-virtual-machines"></a>Tietoja Windows näennäiskoneiden kuvat

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

[AZURE.INCLUDE [virtual-machines-common-classic-about-images](../../includes/virtual-machines-common-classic-about-images.md)]



## <a name="working-with-images"></a>Kuvien käsitteleminen

PowerShellin Azure-moduulin avulla voit hallita käytettävissä olevat kuvat Azure-tilaukseen. Voit myös Azure perinteinen portaalin kuva joitakin tehtäviä varten, mutta komentorivin saat Lisää asetuksia.


-   **Hae kaikki kuvat**:`Get-AzureVMImage`palauttaa kaikki nykyisen tilauksesi käytettävissä kuvat luettelon: kuvien sekä Azure tai kumppaneita tulostusvaihtoehtoja. Tämä tarkoittaa sitä, voit saada suuren luettelon. Seuraava esimerkeissä näytetään hankkiminen lyhentää luettelon.
-   **Hae kuva perheille**:`Get-AzureVMImage | select ImageFamily` saa kuva perheille luettelon näyttämällä merkkijonot **ImageFamily** ominaisuus.
-   **Saat kaikkien kuvien tietyn tuoteperheeseen**:`Get-AzureVMImage | Where-Object {$_.ImageFamily -eq $family}`
-   **Etsi AM kuvat**: `Get-AzureVMImage | where {(gm –InputObject $_ -Name DataDiskConfigurations) -ne $null} | Select -Property Label, ImageName` Tämä toimii suodattamalla DataDiskConfiguration-ominaisuutta, joka koskee vain AM kuvia. Tässä esimerkissä suodattaa myös tulosteen vain otsikko ja kuvan nimen.
-   **Generalized kuva**:`Save-AzureVMImage –ServiceName "myServiceName" –Name "MyVMtoCapture" –OSState "Generalized" –ImageName "MyVmImage" –ImageLabel "This is my generalized image"`
-   **Tallenna tietynlaista kuva**:`Save-AzureVMImage –ServiceName "mySvc2" –Name "MyVMToCapture2" –ImageName "myFirstVMImageSP" –OSState "Specialized" -Verbose`
>[Azure.Tip] OSState-parametri on pakollinen, jos haluat luoda AM-kuva, joka sisältää tietoja levyjen sekä käyttöjärjestelmän levy. Jos et käytä parametrin, cmdlet Luo OS kuva. Parametrin arvo osoittaa, onko kuvan generalized tai erilaisia, perusteella, onko käyttöjärjestelmä levy on valmisteltu uudelleenkäyttöä varten.
-   **Kuvan poistaminen**:`Remove-AzureVMImage –ImageName "MyOldVmImage"`


## <a name="next-steps"></a>Seuraavat vaiheet

Voit myös [luoda Windows-tietokoneen perinteinen-portaalissa](virtual-machines-windows-classic-tutorial.md)


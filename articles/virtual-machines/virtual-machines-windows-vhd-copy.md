<properties
    pageTitle="Tietynlaista AM kopion luominen Azure | Microsoft Azure"
    description="Opettele luomaan kopion erityinen Windows-AM käynnissä Azure-tietokannassa, resurssien hallinnan käyttöönottomalli."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="cynthn"/>
    
    
    
# <a name="create-a-copy-of-a-specialized-windows-vm-running-in-azure"></a>Erityistä Windows-AM Azure käynnissä kopion luominen 

Tämän artikkelin avulla voit luoda kopion Näennäiskiintolevyn erityinen Windows-AM, jossa on käytössä Azure AZCopy-työkalun avulla. Voit sitten Luo uusi AM Näennäiskiintolevyn kopio. 

- Jos haluat kopioida generalized AM, luominen [aiemmin generalized Azure-AM AM kuva](virtual-machines-windows-capture-image.md).

- Jos haluat ladata Näennäiskiintolevyn paikallisen AM, kuten jokin Hyper-V, katso [lataaminen Windows Näennäiskiintolevyn paikallisen-AM, Azure-](virtual-machines-windows-upload-image.md)sovelluksella luodut kohteesta.


## <a name="before-you-begin"></a>Ennen aloittamista

Varmista, että voit:

- On lisätietoja **lähde- ja tallennustilaa tilit**. Lähteen AM sinun täytyy tallennustilan tilin ja säilö nimet. Säilön nimi on yleensä **näennäiskiintolevyjen**. Sinun on myös kohde tallennustilan tilin. Jos ei ole vielä, voit luoda sen joko portaalissa (**Lisää Services** > tallennustilan tilit > Lisää) tai [Uusi AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) cmdlet-komennolla. 

- Azure [PowerShell 1.0](../powershell-install-configure.md) on (tai uudempi) asennettuna.

- Olet ladannut ja asentanut [AzCopy-työkalu](../storage/storage-use-azcopy.md). 


## <a name="deallocate-the-vm"></a>Poista varaus AM

Poistaa AM, mikä vapauttaa Näennäiskiintolevyn kopioidaan varauksen. 

- **Portaalissa**: Valitse **näennäiskoneiden** > **myVM** > lopettaminen
- **PowerShellin**: `Stop-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM` vapauttaa **myVM** resurssin ryhmän **myResourceGroup**-niminen AM.

Azure-portaalissa AM **tila** vaihtuu **pysäytetty** **pysäytetty (Varaus poistettu)**.


## <a name="get-the-storage-account-urls"></a>Hae tallennustilan tilin URL-osoitteet

Tarvitset lähde- ja tallennustilaa tilit URL-osoitteet. URL-osoitteet näyttävät: `https://<storageaccount>.blob.core.windows.net/<containerName>/`. Jos jo tiedät tallennustilan tilin ja säilön nimi, voit korvata vain tiedot sulkeissa luominen URL-osoite. 

Voit käyttää Azure portal tai PowerShellin Azure saat URL-osoite:

- **Portaalissa**: Valitse **Lisää palveluja** > **tallennustilan tilit**  >  <storage account> **BLOB-objektit** ja Näennäiskiintolevyn lähdetiedosto on todennäköisesti **näennäiskiintolevyjen** säilössä. Valitse **Ominaisuudet** : säilö ja kopioi **URL-osoite**, jossa teksti. Sinun on sekä lähde- ja säilöjen URL-osoitteet. 

- **PowerShellin**: `Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"` saa tiedot, AM nimeltä **myVM** -resurssin ryhmän **myResourceGroup**. Hakutuloksista ja Etsi **tallennustilan** profiilin **Näennäiskiintolevyn Uri**. Uri ensimmäinen osa on säilö URL-osoite ja viimeinen osa AM OS Näennäiskiintolevyn nimi.

## <a name="get-the-storage-access-keys"></a>Hae tallennustilan pikanäppäimet

Etsi pikanäppäimet lähde- ja tallennustilaa tilit. Katso lisätietoja pikanäppäinten [tietoja Azure-tallennustilan tilit](../storage/storage-create-storage-account.md).

- **Portaalissa**: Valitse **Lisää palveluja** > **tallennustilan tilit**  >  <storage account> **Kaikkia asetuksia** > **pikanäppäimet**. Kopioi merkintä **Avain1**avain.
- **PowerShellin**: `Get-AzureRmStorageAccountKey -Name mystorageaccount -ResourceGroupName myResourceGroup` saa tallennustilan tilin **mystorageaccount** tallennustilan näppäintä resurssin ryhmän **myResourceGroup**. Kopioi merkintä **Avain1**avain.


## <a name="copy-the-vhd"></a>Kopioi Näennäiskiintolevy 

Voit kopioida tiedostoja käyttämällä AzCopy tallennustilan tilien välillä. Saat kohdesäilön, jos määritetyssä säilössä ei ole valittu, se luodaan puolestasi. 

Jos haluat käyttää AzCopy, Avaa komentorivi paikallisessa tietokoneessa ja Selaa kansioon, johon on asennettu AzCopy. Se on samankaltainen kuin *C:\Program Files (x86) \Microsoft SDKs\Azure\AzCopy*. 

Jos haluat kopioida kaikki tiedostot säilöön, voit käyttää **/S** valitsin. Tämä voidaan kopioida OS Näennäiskiintolevyn ja kaikkia tietoja levyjä, jos ne ovat samassa säilö. Tässä esimerkissä esitetään, kuinka voit kopioida kaikki tiedostot-tallennustilan tilin **mysourcestorageaccount** säilö- **mysourcecontainer** säilö- **mydestinationcontainer** **mydestinationstorageaccount** tallennustilan tilin. Korvaa omalla tallennustilan tilit ja säilöjä nimet. Korvaa `<sourceStorageAccountKey1>` ja `<destinationStorageAccountKey1>` oman nuolinäppäimillä.

```
    AzCopy /Source:https://mysourcestorageaccount.blob.core.windows.net/mysourcecontainer `
        /Dest:https://mydestinationatorageaccount.blob.core.windows.net/mydestinationcontainer `
        /SourceKey:<sourceStorageAccountKey1> /DestKey:<destinationStorageAccountKey1> /S
```

Jos haluat vain kopioida tietyn Näennäiskiintolevyn säilön useita tiedostoja, voit määrittää myös ohjatulla /Pattern tiedostonimi. Tässä esimerkissä vain tiedosto nimeltä **myFileName.vhd** kopioidaan.

```
    AzCopy /Source:https://mysourcestorageaccount.blob.core.windows.net/mysourcecontainer `
        /Dest:https://mydestinationatorageaccount.blob.core.windows.net/mydestinationcontainer `
        /SourceKey:<sourceStorageAccountKey1> /DestKey:<destinationStorageAccountKey1> `
        /Pattern:myFileName.vhd
```


Kun se on valmis, näyttöön tulee sanoma, joka näyttää suunnilleen:

```
  Finished 2 of total 2 file(s).
  [2016/10/07 17:37:41] Transfer summary:
  -----------------
  Total files transferred: 2
  Transfer successfully:   2
  Transfer skipped:        0
  Transfer failed:         0
  Elapsed time:            00.00:13:07
```

## <a name="troubleshooting"></a>Vianmääritys

- Kun käytät AZCopy, jos näet virheen "palvelimeen ei onnistunut tarkistamiseen pyynnön. Varmista, että Authorization-otsikko arvo on muotoiltu oikein myös allekirjoitus." ja käytössäsi on avaimen 2 tai toissijainen tallennusväline-näppäintä, yritä ensisijainen tai 1st tallennustilan avaimen avulla.


## <a name="next-steps"></a>Seuraavat vaiheet

- Voit luoda uuden AM liittämällä [kopio, Näennäiskiintolevyn AM kuin OS-levyn avulla](virtual-machines-windows-create-vm-specialized.md).













<properties
    pageTitle="Ota käyttöön virtuaalikoneen asteikko joukoissa-sovellus | Microsoft Azure"
    description="Ota käyttöön virtuaalikoneen asteikko joukoissa-sovellus"
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gbowerman"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="guybo"/>


# <a name="upgrade-a-virtual-machine-scale-set"></a>Päivitä virtuaalikoneen asteikko-määrittäminen

Tässä artikkelissa kuvataan, kuinka voit siirtää OS päivitys, Määritä ilman mitään käyttökatkot Azure virtuaalikoneen-näytöstä. Tässä yhteydessä OS päivitys liittyy muuttamisesta versio tai SKU käyttöjärjestelmän tai muuttamisesta mukautetun kuvan URI. Päivitetään ilman käyttökatkot tarkoittaa päivittäminen näennäiskoneiden yhden kerrallaan tai ryhmien (esimerkiksi yksi kerrallaan vika toimialue) eikä yhdellä kertaa. Näin säilyttää käynnissä minkä tahansa näennäiskoneiden ei ole päivitetty.

Moniselitteisyyden välttämiseksi erottaa japanin kolmenlaisia OS päivitys, haluat ehkä suorittaa:

- Voit muuttaa versio tai SKU ympäristö kuvan. Muuttaminen esimerkiksi Ubuntu 14.04.2-LTS versio-14.04.201506100 14.04.201507060, tai muuttamalla 16.04.0-LTS/latest Ubuntu 15.10/uusimmat versiot. Tässä artikkelissa käsitellään Tämä skenaario.

- Muuttaminen, joka viittaa mukautetun kuvan uuden version URI laadittuihin (**Ominaisuudet** > **virtualMachineProfile** > **storageProfile** > **osDisk** > **Kuva** > **uri**). Tässä artikkelissa käsitellään Tämä skenaario.

- Aikavyöhykkeiden sisällä virtual machine-OS (tämä esimerkkejä suojauksen korjaustiedoston ja asenna Windows Update). Tässä skenaariossa tuettuja, mutta se ei koske tämän artikkelin.

Kaksi ensimmäistä vaihtoehdot ovat tuetut vaatimukset tämän artikkelin piiriin. Sinun on luoda uusi asteikolla määritetty suorittamaan kolmas vaihtoehto.

Virtuaalikoneen asteikko joukkoja, jotka on otettu käyttöön osana [Azure palvelun kangasta](https://azure.microsoft.com/services/service-fabric/) -klusterin eivät koske tähän.

Tavallinen järjestyksen muuttaminen käyttöjärjestelmän versio/SKU ympäristö kuvan tai mukautetun kuvan URI näyttää seuraavasti:

1. Pyydä virtuaalikoneen asteikko set-malli.

2. Muuta mallin versio, tuote tai URI-arvo.

3. Päivitä mallia.

4. Tee *manualUpgrade* puhelun näennäiskoneiden asteikko-määrittäminen. Tämä vaihe on vain, jos *upgradePolicy* on määritetty **Manuaalinen** asteikko määrittäminen. Jos se on määritetty päivittymään **automaattisesti**, kaikki näennäiskoneiden päivitetään samalla kertaa aiheuttaa näin käyttökatkot.


Nämä taustan ohjeet mielessä katsotaan, kuinka voit päivittää asteikolla PowerShellin ja käyttämällä REST API-version. Näissä esimerkeissä kattaa ympäristö kuva kirjainkoon, mutta tässä artikkelissa on tarpeeksi tietoa, voit mukauttaa tätä prosessia mukautetun kuvan.

## <a name="powershell"></a>PowerShellin##

Tässä esimerkissä päivittää uuteen versioon 4.0.20160229 määrittäminen Windows virtuaalikoneen asteikko. Kun olet päivittänyt mallin, se tekee päivitys yhden virtuaalikoneen esiintymä kerrallaan.

```powershell
$rgname = "myrg"
$vmssname = "myvmss"
$newversion = "4.0.20160229"
$instanceid = "1"

# get the VMSS model
$vmss = Get-AzureRmVmss -ResourceGroupName $rgname -VMScaleSetName $vmssname

# set the new version in the model data
$vmss.virtualMachineProfile.storageProfile.imageReference.version = $newversion

# update the virtual machine scale set model
Update-AzureRmVmss -ResourceGroupName $rgname -Name $vmssname -VirtualMachineScaleSet $vmss

# now start updating instances
Update-AzureRmVmssInstance -ResourceGroupName $rgname -VMScaleSetName $vmssname -InstanceId $instanceId
```

Jos päivität mukautetun kuvan sen sijaan, että kuva käyttöympäristö muuttaminen URI-korvaa "Määritä uuden version"-rivi seuraavanlaiselta:

```powershell
# set the new version in the model data
$vmss.virtualMachineProfile.storageProfile.osDisk.image.uri= $newURI
```


## <a name="the-rest-api"></a>REST-Ohjelmointirajapinnalla

Seuraavassa on lueteltu pari Python esimerkkejä, jotka käyttävät Azure REST-Ohjelmointirajapinnalla aloittamaan päivityksen käyttöjärjestelmän versio. Molemmat asteikko määrittäminen mallin, joka päivitetyn mallin HYLLYTETTY perään GET kevyt [azurerm](https://pypi.python.org/pypi/azurerm) kirjaston Azure REST API paketti funktioiden avulla. Ne näyttävät myös virtuaalikoneen esiintymät näkymissä tunnistavan mukaan update domain näennäiskoneiden.

### <a name="vmssupgrade"></a>Vmssupgrade

 [Vmssupgrade](https://github.com/gbowerman/vmsstools) on Python komentosarjan, jota käytetään aloittamaan OS päivityksen suorittaminen virtuaalikoneen asteikon asettaa Päivitä toimialueen samaan aikaan.

![Vmssupgrade komentosarjan näennäiskoneiden tai Päivitä toimialueen valitseminen](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssupgrade-screenshot.png)

Tämä komentosarja voit valita tietyn näennäiskoneiden Päivitä tai Päivitä toimialueen määrittäminen. Se tukee kuvan käyttöympäristö muuttamisesta tai muuttamisesta mukautetun kuvan URI.

### <a name="vmsseditor"></a>Vmsseditor

[Vmsseditor](https://github.com/gbowerman/vmssdashboard) on yleinen editorin virtuaalikoneen asteikko joukkojen, joka näkyy virtuaalikoneen tila heatmap kohtaa, johon yksi rivi on yksi toimialue. Muun muassa asteikko määrittäminen mallin päivittäminen uutta versiota, tuote tai mukautetun kuvan URI ja valitse vika toimialueiden päivittäminen. Kun teet näin, kaikki kyseisen toimialueen update näennäiskoneiden päivitetään uuden mallin. Vaihtoehtoisesti voit tehdä vaiheittaisen päivityksen valittua erän koon perusteella.  

Seuraavassa näyttökuvassa näkyy asteikolla määrittäminen Ubuntu 14.04-2LTS versio 14.04.201507060 malli. Monia vaihtoehtoja on lisätty tämä työkalu, koska tässä näyttökuvassa on tehty.

![Vmsseditor malli asteikolla Ubuntu 14.04-2LTS määrittäminen](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssEditor1.png)

Kun olet napsauttanut **päivittäminen** ja valitse **Hae tiedot**, näennäiskoneiden UD 0-Aloita päivittäminen.

![Vmsseditor näyttäminen päivitys käynnissä](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssEditor2.png)

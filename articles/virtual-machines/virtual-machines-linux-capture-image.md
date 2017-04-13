<properties
    pageTitle="Sieppaa Linux AM, jos haluat käyttää mallina | Microsoft Azure"
    description="Lue, miten voit kerätä ja generalize kuvan Linux-pohjaiset Azure virtual koneen (AM) luotu Azure resurssien hallinnan käyttöönottomalli."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="iainfou"/>


# <a name="capture-a-linux-virtual-machine-running-on-azure"></a>Sieppaa Linux virtual machine-Azure-käyttöjärjestelmässä

Noudattamalla tämän artikkelin generalize ja siepata Azure Linux virtuaalikoneen (AM) Resurssienhallinta käyttöönotto-mallissa. Kun yleistetään AM, poista tilitiedot ja valmisteleminen AM, jota käytetään kuvana. Valitse sieppaa generalized virtual kiintolevyn (Näennäiskiintolevyn)-kuva käyttöjärjestelmän, näennäiskiintolevyjen liitetyssä levyille ja uusi AM käyttöönotoissa [Resurssienhallinta-malli](../azure-resource-manager/resource-group-overview.md) . 

Luodaksesi VMs käyttämällä kuvan verkkoresursseja määritetään kunkin uuden AM ja (JavaScript Object Notation tai JSON-tiedosto)-mallin avulla voit ottaa käyttöön otettu Näennäiskiintolevyn kuvista. Tällä tavalla voit toistaa AM sen nykyisen ohjelmiston kokoonpanoa, samalla tavalla kuin kuvien käyttäminen Azure Marketplacesta.

>[AZURE.TIP]Jos haluat luoda kopion aiemmin Linux-AM ja sen erityinen varmuuskopiointi-ja virheenkorjaus-kohdassa [Create kopion Linux virtual machine-Azure-käyttöjärjestelmässä](virtual-machines-linux-copy-vm.md). Ja jos haluat ladata Linux Näennäiskiintolevyn paikallisen AM from, katso [lataaminen ja luoda mukautettuja levyn näköistiedoston Linux AM](virtual-machines-linux-upload-vhd.md).  

## <a name="before-you-begin"></a>Ennen aloittamista

Varmista, että seuraavat edellytykset täyttyvät:

* **Azure AM luotu resurssien hallinnan käyttöönottomalli** - Jos et ole luonut Linux AM voit käyttää [portal](virtual-machines-linux-quick-create-portal.md), [Azure CLI](virtual-machines-linux-quick-create-cli.md)tai [Resurssienhallinta malleja](virtual-machines-linux-cli-deploy-templates.md). 

    Määritä AM tarpeen mukaan. Esimerkiksi [lisätä tietojen levyjä](virtual-machines-linux-add-disk.md), päivitykset ja asentaa sovelluksia. 
* **Azure CLI** - asennuksen [Azure CLI](../xplat-cli-install.md) paikalliseen tietokoneeseen.

## <a name="step-1-remove-the-azure-linux-agent"></a>Vaihe 1: Poista Azure Linux-agentti

Suorita **waagent** -komento ensin **deprovision** -parametrin Linux AM. Tällä komennolla voit poistaa tiedostot ja tiedot, jotta valmiina, joilla AM. Lisätietoja on artikkelissa [Azure Linux agentti käyttöoppaassa](virtual-machines-linux-agent-user-guide.md).

1. Muodostaa yhteyttä Linux AM SSH-asiakas.

2. Kirjoita SSH-ikkunassa seuraava komento:

    ```
    sudo waagent -deprovision+user
    ```
    >[AZURE.NOTE] Suorittaa komennon vain AM, jotka haluat siepata kuvana. Ei takaa kuva ole valittu kaikki luottamuksellisia tietoja tai sopii uudelleenjakamista varten.

3. Kirjoita **y** Jatka. Voit lisätä **-Pakota** parametri välttää vahvistus tämän vaiheen.

4. Komennon jälkeen, kirjoittamalla **exit**. Tässä vaiheessa sulkee SSH asiakas.

    
## <a name="step-2-capture-the-vm"></a>Vaihe 2: Sieppaaminen AM

Generalize ja tallentaa AM Azure-CLI avulla. Seuraavissa esimerkeissä korvaa Esimerkki parametrinimet oman arvoilla. Esimerkki parametrinimet ovat **myResourceGroup**, **myVnet**ja **myVM**.

5. Avaa paikalliseen tietokoneeseen Azure CLI ja [Azure-tilaukseen kirjautuminen](../xplat-cli-connect.md). 

6. Varmista, että Resurssienhallinta-tilassa.

    ```
    azure config mode arm
    ```

7. Sulje AM, jotka olet käyttömahdollisuus purettu jo käyttämällä seuraava komento:

    ```
    azure vm deallocate -g MyResourceGroup -n myVM
    ```

8. Generalize AM seuraavalla komennolla:

    ```
    azure vm generalize -g MyResourceGroup -n myVM
    ```

9. Nyt suorittaa **azure AM sieppaus** -komentoa, joka kuvaa AM. Seuraavassa esimerkissä kuva näennäiskiintolevyt ovat oteta talteen, joiden nimet alkavat **MyVHDNamePrefix**ja **-t** -asetus määrittää mallia **MyTemplate.json**polku. 

    ```
    azure vm capture MyResourceGroup MyResourceGroup MyVHDNamePrefix -t MyTemplate.json
    ```

    >[AZURE.IMPORTANT]Kuva näennäiskiintolevytiedostoja luodaan oletusarvoisesti samalle tallennustilan-tilille, jota käytetään alkuperäisen AM. *Saman tallennustilan tilin* avulla voit tallentaa mitä tahansa kuvan luot uuden VMs näennäiskiintolevyt. 

6. Etsi siepatun kuvan sijainti, Avaa tekstieditorissa JSON-malli. Etsi **storageProfile** **uri** **Kuva** **järjestelmän** säilö sijaitsee. Esimerkiksi OS levyn näköistiedoston URI on samankaltainen kuin`https://xxxxxxxxxxxxxx.blob.core.windows.net/system/Microsoft.Compute/Images/vhds/MyVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`

## <a name="step-3-create-a-vm-from-the-captured-image"></a>Vaihe 3: Luo AM siepatun kuvan
Nyt käyttää kuvan mallin avulla voit luoda Linux AM. Seuraavia ohjeita noudattamalla voit Azure-CLI ja luo uusi virtual verkko AM tallennetaan JSON tiedostomallin.

### <a name="create-network-resources"></a>Luo verkkoresursseja

Mallin käyttäminen, sinun on määritettävä VPN ja NIC uusi AM. Suosittelemme seuraavia resursseja Resurssiryhmän luominen AM kuva tallennussijainnin. Suorita komennot samalla tavalla kuin seuraavassa, korvaaminen nimet resurssien ja sopiva Azure sijainti ("centralus"-komentoja):

    azure group create MyResourceGroup1 -l "centralus"

    azure network vnet create MyResourceGroup1 myVnet -l "centralus"

    azure network vnet subnet create MyResourceGroup1 myVnet mySubnet

    azure network public-ip create MyResourceGroup1 myIP -l "centralus"

    azure network nic create MyResourceGroup1 myNIC -k mySubnet -m myVnet -p myIP -l "centralus"

### <a name="get-the-id-of-the-nic"></a>Verkkokortti tunnuksen hankkiminen

Asentavat AM kuvasta tallensit sieppauksen aikana JSON, tarvitset tunnuksen NIC-sivustosta. Hanki suorittamalla seuraavan komennon:

    azure network nic show MyResourceGroup1 myNIC

**Tunnus** -tulos muistuttaa`/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic`



### <a name="create-a-vm"></a>Luo AM
Nyt varten suorittamalla seuraavan komennon voit luoda oman AM AM siepatun kuvan. **-F** parametrin avulla voit määrittää tallennetun mallin JSON-tiedoston polku.

    azure group deployment create MyResourceGroup1 MyDeployment -f MyTemplate.json

Komento-tulostus sinua kehotetaan AM nimeämään, järjestelmänvalvojan käyttäjänimeä ja salasanaa ja aiemmin luotu NIC tunnus.

    info:    Executing command group deployment create
    info:    Supply values for the following parameters
    vmName: myNewVM
    adminUserName: myAdminuser
    adminPassword: ********
    networkInterfaceId: /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resource Groups/MyResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic

Seuraavassa esimerkissä näkyy näkemääsi onnistuneen käyttöönottoa varten:

    + Valmistellaan malli-määrityksiä ja parametrit
    + Käyttöönoton tiedot luominen: luodaan mallin käyttöönoton xxxxxxx
    + Käyttöönoton suorittamiseen tietojen odotetaan: DeploymentName: MyDeployment tietojen: ResourceGroupName: MyResourceGroup1 tietojen: ProvisioningState: onnistui tietojen: aikaleima: xxxxxxx tietojen: tila: osittaiset tiedot: nimiarvo tyyppi


    data:    ------------------  ------------  -------------------------------------

    tietoja: vmName merkkijonon myNewVM


    tietoja: vmSize merkkijonon Standard_D1


    tietoja: adminUserName merkkijonon myAdminuser


    tietoja: adminPassword SecureString Määrittämätön


    tietoja: networkInterfaceId merkkijonon /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic tiedot: ryhmän käyttöönoton create OK-komento

### <a name="verify-the-deployment"></a>Tarkista käyttöönotto

Nyt SSH, virtuaalikoneen luomasi vahvistamiseksi käyttöönotto- ja uusi AM käytön aloittaminen. Muodostaa kautta SSH Etsi IP-osoitteen AM luomasi suorittamalla seuraavan komennon:

    azure network public-ip show MyResourceGroup1 myIP

Julkinen IP-osoite näkyy komennon tulosteessa. Oletusarvon mukaan voit muodostaa yhteyden Linux-AM SSH porttiin 22 mukaan.

## <a name="create-additional-vms"></a>Luo muita VMs
Käyttöön otettu kuva ja mallin muita VMs on edellisen osion vaiheiden avulla. Muita vaihtoehtoja VMs luominen kuvan sisältävät pikaopas mallin avulla tai **azure AM luominen** -komennon suorittaminen.

### <a name="use-the-captured-template"></a>Siepatun käyttäminen

Voit käyttää siepatun kuvan ja mallin, toimimalla seuraavasti (yksityiskohtainen on edellisen osion):

* Varmista, että AM kuva saman tallennustilan tilin, joka isännöi oman AM Näennäiskiintolevyn.
* Kopioi JSON mallitiedoston ja uusi AM Näennäiskiintolevyn (tai näennäiskiintolevyjen) OS-levyn yksilöivä nimi. Määritä esimerkiksi **storageProfile**, valitse **näennäiskiintolevyn** **uri**- **osDisk** SITEN, samalla yksilöllinen nimi`https://xxxxxxxxxxxxxx.blob.core.windows.net/vhds/MyNewVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`
* Luo NIC samassa tai eri virtual verkkoon.
* Muokatun mallin JSON-tiedoston avulla, Luo käyttöönoton resurssiryhmä, jossa määrität virtual verkkoon.

### <a name="use-a-quickstart-template"></a>Pikaopas mallin käyttäminen

Jos haluat määrittää automaattisesti kun luot AM kuvan verkko, voit määrittää nämä resurssit mallina. Esimerkiksi näkyviin GitHub [101-AM-kohteesta-käyttäjä-kuva-malli](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) . Tämä malli luo AM mukautetun kuvan ja tarvittavat virtual verkko-julkiseen IP-osoite ja NIC resurssit. Tutustu vaiheista mallia käyttämällä Azure-portaalissa, [Luo virtual koneen mukautetun kuvasta Resurssienhallinta mallin avulla](http://codeisahighway.com/how-to-create-a-virtual-machine-from-a-custom-image-using-an-arm-template/).

### <a name="use-the-azure-vm-create-command"></a>Käytä azure AM create-komento

Yleensä on helpointa Resurssienhallinta-mallin avulla voit luoda AM kuva. Sijaan voit luoda AM _imperatively_ käyttämällä **azure AM luominen** -komento **-Q** (**--Kuva urn**) parametrin. Jos käytät tätä tapaa, voit myös siirtää Määritä uusi AM OS .vhd-tiedoston sijainti **-d** (**--os-levy-näennäiskiintolevyn**)-parametrin. Tämä tiedosto on oltava näennäiskiintolevyjen säilö-tallennustilan tilin kuva Näennäiskiintolevytiedosto tallennuspaikan. Komento kopioi Näennäiskiintolevyn uusi AM automaattisesti **näennäiskiintolevyjen** säilöön.

Ennen kuin suoritat **azure AM luominen** kuvan, tee seuraavat vaiheet:

1.  Luoda resurssiryhmän tai selvitä aiemmin luotu resurssiryhmä käyttöönottoa varten.

2.  Luo uusi AM resurssille julkiseen IP-osoite ja NIC resurssin. Katso tämän artikkelin ohjeita voit luoda VPN, julkiseen IP-osoite ja NIC CLI on. (**azure AM luoda** myös luoda NIC: ssä, mutta haluat siirtää muut parametrit VPN ja aliverkon.)


Suorita sitten komento, joka välittää URI uusi OS Näennäiskiintolevytiedosto ja kuvaan. Tässä esimerkissä koon Standard_A1 AM luodaan Yhdysvaltojen Itä-alueella.

    azure vm create MyResourceGroup1 myNewVM eastus Linux -d "https://xxxxxxxxxxxxxx.blob.core.windows.net/vhds/MyNewVHDNamePrefix.vhd" -Q "https://xxxxxxxxxxxxxx.blob.core.windows.net/system/Microsoft.Compute/Images/vhds/MyVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd" -z Standard_A1 -u myAdminname -p myPassword -f myNIC

Lisää komento asetuksia, suorita `azure help vm create`.

## <a name="next-steps"></a>Seuraavat vaiheet

Oman VMs kanssa CLI hallinnasta on artikkelissa tehtävien [käyttöönotto ja hallita näennäiskoneiden Azure Resurssienhallinta mallit ja Azure-CLI](virtual-machines-linux-cli-deploy-templates.md).

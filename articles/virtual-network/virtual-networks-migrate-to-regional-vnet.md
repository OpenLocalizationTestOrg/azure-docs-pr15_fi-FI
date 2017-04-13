<properties 
   pageTitle="Siirtäminen affiniteetti ryhmistä alueellisen Virtual Network (VNet)"
   description="Opi siirtämään affiniteetti ryhmistä alueellisen vnets"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="how-to-migrate-from-affinity-groups-to-a-regional-virtual-network-vnet"></a>Siirtäminen affiniteetti ryhmistä alueellisen Virtual Network (VNet)

Voit varmistaa, että resurssit luodaan samaan affiniteetti ryhmän suoritettavien fyysisesti palvelimet, jotka ovat lähellä toisiaan ottaminen käyttöön näiden resurssien viestintä nopeammin affiniteetti-ryhmä. Aiemmin affiniteetti ryhmät on tarve luominen virtual verkkojen (VNets). Samaan aikaan, hallita VNets hallinnan verkkopalvelu vain saattaa toimia tietyn fyysinen palvelimia tai asteikko. Arkkitehtuuri parannuksia on lisätty verkon hallinnan alueen.

Arkkitehtuuri parannuksia tuloksena affiniteetti ryhmät ovat enää suositellut tai virtual verkkojen pakollinen. Affiniteetti ryhmien käyttäminen VNets korvataan alueet. VNets, jotka liittyvät alueiden kutsutaan alueellisen VNets.

Lisäksi on suositeltavaa, että et käytä affiniteetti ryhmien yleinen. Lukuun ottamatta VNet, vaatimus affiniteetti ryhmät on myös tärkeää varmistaa resurssit, kuten suorittaminen ja tallennustilaa, on asetettu toistensa lähelle avulla. Kuitenkin Azure verkon nykyisen rakenteen sijainti näitä vaatimuksia ei enää tarvita. Katso [affiniteetti ryhmät ja VMs](#Affinity-groups-and-VMs) muutamia jäljellä olevan tietyn tapauksissa missä kannattaa käyttää affiniteetti-ryhmän.

## <a name="creating-and-migrating-to-regional-vnets"></a>Luominen ja alueelliset VNets siirtyminen

*Alueen*työsähköpostiin, kun luot uuden VNets, käytä. Näyttöön tulee Tämä näkyy jo vaihtoehdoissa hallinta-portaalissa. Huomaa, että verkon määritystiedostossa näyttää *sijainniksi*.

>[AZURE.IMPORTANT] Vaikka on edelleen tarkalleen ottaen voi luoda virtual verkkoon, joka on liitetty affiniteetti-ryhmä, ei ole pakottavaa tekemään niin. Monia uusia ominaisuuksia, kuten verkon käyttöoikeusryhmät, ovat käytettävissä vain käytettäessä alueellisen VNet ja eivät ole käytettävissä virtual verkkojen, jotka liittyvät affiniteetti ryhmät.

### <a name="about-vnets-currently-associated-with-affinity-groups"></a>Tietoja VNets liittyvien affiniteetti ryhmät

VNets, joka on liitetty affiniteetti ryhmät on otettu käyttöön alueellisen VNets siirto. Voit siirtää alueellisen VNet, toimimalla seuraavasti:

1. Vie verkon määritys-tiedosto. Voit käyttää PowerShell tai hallinta-portaalin. Katso ohjeet hallinta-portaalissa [määrittäminen oman VNet verkon määritys-tiedoston avulla](virtual-networks-using-network-configuration-file.md).

1. Muokkaa verkon määritysten tiedoston vanhojen arvojen korvaaminen uusilla arvoilla. 

    > [AZURE.NOTE] **Sijainti** on alue, joka on valittu affiniteetti ryhmä, johon on liitetty oman VNet. Esimerkiksi jos että VNet liittyy affiniteetti-ryhmä, joka sijaitsee Länsi US, kun siirrät, sijaintisi on osoitettava Länsi US. 
    
    Muokkaa seuraavat rivit verkon määritystiedostossa-arvojen korvaaminen omalla: 

    **Vanha arvo:** \<VirtualNetworkSitename = "VNetUSWest" AffinityGroup = "VNetDemoAG"\> 

    **Uusi arvo:** \<VirtualNetworkSitename = "VNetUSWest" sijainnin = "Länsi USA"\>

1. Tallenna muutokset ja [tuoda](virtual-networks-using-network-configuration-file.md) verkon määritysten Azure.

>[AZURE.NOTE] Tämä siirto ei aiheuta mitään palveluihin käyttökatkot.

## <a name="affinity-groups-and-vms"></a>Affiniteetti ryhmät ja VMs

Kuten edellä mainittiin, affiniteetti ryhmät suositellaan enää yleensä VMs. Affiniteetti ryhmän kannattaa käyttää vain silloin, kun joukko VMs on oltava suoria alin verkko-viive, välillä VMs. Sijoittamalla VMs affiniteetti-ryhmässä VMs kaikki sijoitetaan saman Laske klusterin tai skaalauksen yksikön.

On tärkeää muistaa, että käyttämällä affiniteetti-ryhmässä voit on kaksi, mahdollisesti negatiivinen vaikutukset:

- AM koot joukko rajoittuvat AM koot Laske asteikko tarjoamia joukkoon.

- On suurempi todennäköisyyden sille, että ei voi varata uusi AM. Tämä tapahtuu, kun affiniteetti ryhmän tietyn asteikko on kapasiteetti.

### <a name="what-to-do-if-you-have-a-vm-in-an-affinity-group"></a>Jos sinulla AM affiniteetti-ryhmä

VMs affiniteetti-ryhmässä olevat ei tarvitse poistaa henkilöt affiniteetti.

Kun AM on otettu käyttöön, se otetaan käyttöön yhden asteikko. Affiniteetti ryhmät voivat rajoittaa käytettävissä AM koot AM-käyttöönoton joukko, mutta aiemmin AM, jotka on otettu käyttöön on jo rajoitettu AM koot asteikko, jossa AM on otettu käyttöön käytettävissä olevat. Tästä syystä AM poistaminen affiniteetti-ryhmä ei ole vaikutusta.
 

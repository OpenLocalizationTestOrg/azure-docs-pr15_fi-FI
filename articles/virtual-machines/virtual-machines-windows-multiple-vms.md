<properties
    pageTitle="Luo useita näennäiskoneiden | Microsoft Azure"
    description="Useita näennäiskoneiden luominen Windows-asetukset"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="gbowerman"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="guybo"/>

# <a name="create-multiple-azure-virtual-machines"></a>Luo useita Azure-virtuaalikoneissa

On kohtaa, johon haluat luoda on runsaasti samalla näennäiskoneiden (VMs) skenaarioita. Esimerkkejä tehokas tietojenkäsittely (HPC) suurissa Tietoanalyysi, skaalattava ja usein tilattomien keskimmäisen tai Taustajärjestelmä (kuten webservers) ja palvelinten hajautettu tietokannat.

Tässä artikkelissa käsitellään useita VMs luominen Azure käytettävissä olevista vaihtoehdoista. Nämä vaihtoehdot muutakin kuin yksinkertainen tapauksissa, jossa VMs joukon luominen manuaalisesti. Jos haluat luoda monta VMs, prosesseja, joissa käytetään tavallisesti ei skaalata hyvin, jos haluat luoda enintään muutama VMs.

Voit luoda useita samanlaisia VMs on käyttämään, _resurssi silmukat_Azure Resurssienhallinta-rakennetta.

## <a name="resource-loops"></a>Resurssin silmukat

Resurssin silmukoita ovat syntaktisia lyhenne Azure Resurssienhallinta mallit. Resurssin silmukoita luoda samalla tavalla kuin määritetty resurssien joukon silmukassa. Resurssin silmukoita avulla voit luoda useita tallennustilan tilit, verkkoliittymät tai näennäiskoneiden. Lisätietoja resurssin silmukoita viitata [luominen VMs käytettävyyden määrittää resurssin silmukoita avulla](https://azure.microsoft.com/documentation/templates/201-vm-copy-index-loops/).

## <a name="challenges-of-scale"></a>Haasteiden asteikko

Vaikka resurssin silmukoita helpompi pilvi-infrastruktuuria tasolla muodostetaan ja tuottaa lyhyempi mallit, tietyt haasteita säilyvät. Esimerkiksi jos luot 100 näennäiskoneiden resurssin silmukan avulla, haluat yhdistää verkon käyttöliittymän ohjaimia (NIC) vastaavan VMs ja tallennustilaa tilit. Koska VMs määrä on todennäköisesti eroaa tilien tallennustilan määrän, sinun on käsitellä eri resurssin silmukan koot liian. Nämä ovat solvable ongelmia, mutta monimutkaisuus kasvaa merkittävästi asteikko.

Toinen todennus tapahtuu, kun tarvitset infrastruktuuri, jossa Skaalaa elastically. Voit haluta esimerkiksi Automaattinen skaalaus-infrastruktuuria, joka automaattisesti suurentaa tai pienentää VMs määrä vastauksena työmäärää. VMs eivät ole integroitu järjestelmä, voit vaihdella numero (mittakaava, ja käytetään). Jos olet skaalata poistamalla VMs, on vaikea takaa suuren käytettävyyden varmistamalla, että saapuva päivitys-ja vika VMs.

Lopuksi käytettäessä resurssien silmukka, voit luoda resurssien useita puhelut siirtyvät pohjana kangasta. Useita kutsuja luodessasi samalla resurssien Azure on implisiittisesti mahdollisuuden parantaa yhteydessä tämän rakenteen ja optimoi käyttöönoton luotettavuutta ja suorituskykyä. Tämä on missä _virtuaalikoneen skaalaus-vaihtoehdon_ olla.

## <a name="virtual-machine-scale-sets"></a>Virtuaalikoneen asteikko joukot

Virtuaalikoneen asteikko ovat Azure Laske resurssin ottaa käyttöön ja hallita samanlaiset VMs joukkoa. Kaikki VMs kanssa määritetty samaa, AM asteikkoa joukot on helppo skaalata ja Skaalaa ulos. Muutat VMs määrä joukkoa. Voit myös määrittää AM asteikko joukkoja Automaattinen skaalaus työmäärää tarpeiden perusteella.

Sovellusten vaikuttavat skaalata Laske resursseja, ja valitse asteikko-toimintojen implisiittisesti saapuva vika ja Päivitä toimialueilla.

Sen sijaan, että hajautettuna useita resursseja, kuten NIC- ja VMs AM asteikko on verkossa, varasto, virtuaalikoneen ja tunniste-ominaisuudet, joita voit määrittää keskitetysti.

Esittely AM asteikko joukot-kohdassa [virtuaalikoneen asteikko määrittää product-sivu](https://azure.microsoft.com/services/virtual-machine-scale-sets/). Lisätietoja Siirry [näennäiskoneiden asteikko määrittää ohjeissa](https://azure.microsoft.com/documentation/services/virtual-machine-scale-sets/).

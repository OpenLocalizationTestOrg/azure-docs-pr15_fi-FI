<properties 
    pageTitle="Verkon määritysten tiedoston virtual verkon määrittäminen" 
    description="Voit viedä ja tuoda verkon määritystiedoston, jotta voit luoda tai muokata virtual verkkojen Azure hallinta-portaalin ohjeita. " 
    services="virtual-network" 
    documentationCenter="" 
    authors="jimdial" 
    manager="carmonm" 
    editor="tysonn"/>

<tags
    ms.service="virtual-network"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services" 
    ms.date="03/15/2016"
    ms.author="jdial"/>

# <a name="configure-a-virtual-network-using-a-network-configuration-file"></a>Verkon määritysten tiedoston virtual verkon määrittäminen

Voit määrittää virtual verkon (VNet) Azure hallinta-portaalissa tai verkon määritys-tiedoston avulla.

## <a name="creating-and-modifying-a-network-configuration-file"></a>Luovat ja muokkaavat verkon kokoonpanotiedosto 
Verkoston asetusten tuominen aiemmin VPN-määritys-on helpointa tekijän verkon kokoonpanotiedosto sisältävät asetuksia, jotka haluat määrittää virtual lopettaminen tallentaminen.

Muokkaa verkko-kokoonpanotiedosto voi vain avata tiedoston, tee tarvittavat muutokset Valitse Tallenna tiedosto. Voit käyttää mitä tahansa *xml* -editoriin halutaan muuttaa verkko-kokoonpanotiedosto. 

Lisäohjeita [verkon rakenteen määrityksiä](https://msdn.microsoft.com/library/azure/jj157100.aspx)tiiviisti noudattamalla. 

Azure katsoo aliverkon, jossa on jotain käyttöön kuin **käytössä**. Jos aliverkon on käytössä, sitä ei voi muokata. Ennen muokkaamista, siirrä mitään, jonka olet ottanut käyttöön aliverkon ja eri aliverkon, joka ei ole muokataan.   Katso [siirtää AM tai eri aliverkon roolin esiintymä](virtual-networks-move-vm-role-to-subnet.md).

## <a name="export-and-import-virtual-network-settings-using-the-management-portal"></a>VPN-asetusten hallinta-portaalissa vieminen ja tuominen  
Voit tuoda ja viedä sisältämät verkon kokoonpanotiedosto käyttämällä PowerShell tai hallinta-portaalin verkon määritysten asetukset. Alla olevien ohjeiden avulla voit vieminen ja tuominen hallinta-portaalissa. 

### <a name="to-export-your-network-settings"></a>Vie verkkoasetukset
Kun viet, kaikki tilauksen virtual verkot asetukset kirjoitetaan .xml-tiedostossa. 

1. Lokitiedoston **hallinta-portaalin**.
2. Valitse hallinta-portaalin **verkot** -sivun alareunassa **Vie**. 
3. Varmista, että olet valinnut tilaus, jonka haluat viedä verkkoasetukset **Vie verkon määritys** -ikkunassa. Napsauta oikeassa alakulmassa valintamerkki. 
4. Kun ohjelma kysyy, Tallenna *NetworkConfig.xml* tiedosto valitsemaasi sijaintiin.


### <a name="to-import-your-network-settings"></a>Voit tuoda verkkoasetukset

1. Valitse **Hallinta-portaalin**-siirtymisruudun vasemmassa alareunassa valitsemalla **Uusi**.
2. Valitse **Verkkopalvelut** -> **VPN** -> **Tuo määritykset**.
3. Valitse **Tuo verkko-kokoonpanotiedosto** -sivulla Selaa verkon määritys-tiedosto ja valitse sitten **Seuraava** -nuoli.
4. **Verkoston luominen** -sivulla näet kirjautumisnäyttö, jossa näkyy, mitkä osat verkon määritysten muutettu tai luotu tiedot. Jos muutokset näyttävät oikeilta sinulle, valitse Päivitä tai luo virtuaalisia verkon Jatka valintamerkki. 
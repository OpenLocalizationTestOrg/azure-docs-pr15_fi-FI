<properties
   pageTitle="Ota käyttöön tai poistaa sen käytöstä Azure AM seuranta"
   description="Tässä artikkelissa käsitellään käyttöön ottaminen tai käytöstä Azure AM seuranta"
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="kmouss"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="02/08/2016"
   ms.author="kmouss"/>
   
# <a name="enable-or-disable-azure-vm-monitoring"></a>Ota käyttöön tai poistaa käytöstä Azure AM seuranta

Tässä osassa kuvataan ottaminen käyttöön tai poistaminen käytöstä Azuren näennäiskoneiden valvontaan. Seuranta on oletusarvoisesti käytössä Azure Virtual tietokoneissa, jos käyttöön [Azure portal](https://portal.azure.com) ja seurantaa kaaviot ovat käytettävissä 1 minuutin pisteeseen oletusarvon mukaan. Voit ottaa käyttöön tai poistaminen käytöstä seuranta käyttämällä portal tai Azure käyttöliittymä (Mac), Linux ja Windows (Azure-CLI). 

## <a name="enable--disable-monitoring-through-the-azure-portal"></a>Ottaa käyttöön / poistaa käytöstä palvelun Azure-portaalissa seuranta
 
Voit ottaa Azure AM, joka sisältää tietoja omassa 1 minuutin kausien seuranta. (tallennustilan muutokset koskevat). Diagnostiikan yksityiskohtaiset tiedot ovat käytettävissä, AM portaalin viivakaavioissa tai Ohjelmointirajapinnan kautta. Oletusarvon mukaan Azure Portaalilla seuranta, mutta voit poistaa sen käytöstä seuraavalla tavalla. Voit ottaa AM on käynnissä tai pysäytetty-tilan seuranta.

- Avaa Azure portaalin **osoitteessa [https://portal.azure.com](https://portal.azure.com)**

- Valitse vasemmassa siirtymisruudussa näennäiskoneiden.

- Valitse luettelo-näennäiskoneiden on käynnissä vai pysäytetty esiintymä. Virtuaalikoneen blad avautuu.

- Valitse "Kaikkia asetuksia".

- Valitse "Diagnostiikka".

- Muuta tila käyttöön tai poistaa sen käytöstä. Voit myös valita tämä sivu-tiedot, jotka haluat ottaa käyttöön, virtuaalikoneen valvonnan taso.

[Azure.Note] Diagnostiikka-valitsin on oletusarvo, kun luot uuden virtual koneen

![Ottaa käyttöön / poistaa käytöstä palvelun Azure-portaalissa seuranta.][1]


## <a name="enable--disable-monitoring-with-azure-cli"></a>Ottaa käyttöön / poistaa käytöstä Azure CLI seurantaa
 
Voit ottaa käyttöön Azure-AM seurantaa.

- Luo tiedosto nimeltä PrivateConfig.json esimerkiksi seuraavat sisältöä.
        {"storageAccountName": "tallennustilan tilin, voit hakea tietoja", "storageAccountKey": "tilin avain"}
- Suorita seuraava komento Azure CLI.

        azure vm extension set myvm LinuxDiagnostic Microsoft.OSTCExtensions 2.0 --private-config-path PrivateConfig.json

[Azure.Note] Voit muuttaa versiosta 2.0 uudempi versio, kun niitä on saatavilla. 

Saat lisätietoja määrittäminen seurantaa arvot ja malleja, elinkaaresta asiakirja - **[Käyttämällä Linux diagnostiikan alanumeron näytön Linux AM suorituskykyä ja diagnostiikkatiedot](virtual-machines-linux-classic-diagnostic-extension.md).

<!--Image references-->
[1]: ./media/virtual-machines-linux-vm-monitoring/portal-enable-disable.png
 


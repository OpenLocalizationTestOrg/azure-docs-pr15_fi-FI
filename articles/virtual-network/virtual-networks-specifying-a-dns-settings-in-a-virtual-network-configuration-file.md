<properties 
   pageTitle="DNS-asetusten määrittäminen VPN-kokoonpanotiedosto | Microsoft Azure"
   description="Voit muuttaa DNS-palvelimen asetusten virtual verkossa VPN-kokoonpanotiedosto perinteinen käyttöönoton mallin avulla"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" 
   tags="azure-service-management" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/23/2016"
   ms.author="jdial" /> 


# <a name="specifying-dns-settings-in-a-virtual-network-configuration-file"></a>DNS-asetusten määrittäminen VPN-kokoonpanotiedosto

Verkko-kokoonpanotiedosto on kaksi elementtejä, jotka voit määrittää toimialueen nimi järjestelmän (DNS)-asetukset: **DnsServers** ja **DnsServerRef**. Voit lisätä DNS-palvelimet luettelo määrittämällä niiden IP-osoitteet ja viittaus nimet **DnsServers** -elementtiä. Voit sitten määrittää, mikä DNS-palvelinten määritteet DnsServers elementistä käytetään virtual verkossa oleville sivustoille verkon eri **DnsServerRef** elementti.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Tässä artikkelissa käsitellään perinteinen käyttöönottomalli.

Verkko-kokoonpanotiedosto voi sisältää seuraavat osat. Jokaisen osan otsikko on linkitetty sivulle, jossa on lisätietoja elementin arvo-asetukset.

>[AZURE.IMPORTANT] Lisätietoja verkko-kokoonpanotiedosto määrittämisestä on artikkelissa [määrittäminen Virtual verkon käyttämällä verkon määritystiedostoa](virtual-networks-using-network-configuration-file.md). Lisätietoja muut elementit, verkon määritysten tiedostossa on artikkelissa [Azure Virtual verkon määritysten rakenne](https://msdn.microsoft.com/library/azure/jj157100.aspx).

[DNS-osa](http://go.microsoft.com/fwlink/?LinkId=248093)

    <Dns>
      <DnsServers>
        <DnsServer name="ID1" IPAddress="IPAddress1" />
        <DnsServer name="ID2" IPAddress="IPAddress2" />
        <DnsServer name="ID3" IPAddress="IPAddress3" />
      </DnsServers>
    </Dns>

>[AZURE.WARNING] **DnsServer** osaan **name** -määritettä käytetään vain viitteenä **DnsServerRef** elementti. Isäntänimi DNS-palvelin ei vastaa. Kunkin **DnsServer** -määritteen arvon on oltava yksilöllinen koko Microsoft Azure-tilauksessa

[Sivustojen VPN-elementti](http://go.microsoft.com/fwlink/?LinkId=248093)

    <DnsServersRef>
      <DnsServerRef name="ID1" />
      <DnsServerRef name="ID2" />
      <DnsServerRef name="ID3" />
    </DnsServersRef>

>[AZURE.NOTE] Jotta voit määrittää tämän asetuksen Virtual verkkosivustot-elementin, se on aiemmin määritettävä DNS-osaan. Virtuaalinen verkkosivustot osaan DnsServerRef *nimi* on viitattava määritettyä DNS-osan DnsServer *nimi*nimi-arvoa.

## <a name="next-steps"></a>Seuraavat vaiheet

- Tietoja [Azure Virtual verkon määritysten rakenne](http://go.microsoft.com/fwlink/?LinkId=248093).
- Tietoja [Azure määritysten malli](https://msdn.microsoft.com/library/windowsazure/ee758710).
- [Verkon määritysten tiedostojen käyttäminen virtual verkon määrittäminen](virtual-networks-using-network-configuration-file.md).

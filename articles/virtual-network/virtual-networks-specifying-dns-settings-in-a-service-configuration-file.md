<properties 
   pageTitle="DNS-asetusten määrittäminen palvelun määritystiedostossa | Microsoft Azure"
   description="palvelun määritystiedoston käyttäminen VPN mukautettujen DNS-asetusten määrittäminen"
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
   ms.date="02/24/2016"
   ms.author="jdial" />

# <a name="specifying-dns-settings-in-a-service-configuration-file"></a>Palvelun määritystiedostossa DNS-asetusten määrittäminen

## <a name="dns-elements"></a>DNS-osat

Palvelun määritys-tiedostossa voi olla DnsServers-osa ja luettelo (DNS, Domain Name System)-palvelimet palvelua käyttävät IPv4-osoitteet. Palvelun määritystiedostossa ohittavat verkon määritystiedostossa asetukset. Lisätietoja on artikkelissa [Azure määritysten malli (.cscfg tiedosto)](https://msdn.microsoft.com/library/azure/ee758710.aspx).

**NetworkConfiguration-elementti**

      <DnsServers>
        <DnsServer name="ID1" IPAddress="IPAddress1" />
        <DnsServer name="ID2" IPAddress="IPAddress2" />
        <DnsServer name="ID3" IPAddress="IPAddress3" />
      </DnsServers>

>[AZURE.WARNING] **DnsServer** osaan **name** -määritettä käytetään vain viitteen nimi. Isäntänimi DNS-palvelin ei vastaa. Kunkin **DnsServer** -määritteen arvon on oltava yksilöllinen koko Microsoft Azure-tilaukseen.

## <a name="see-also"></a>Katso myös

[Azure määritysten malli (.cscfg)](https://msdn.microsoft.com/library/windowsazure/ee758710)

[Azure Virtual verkon määritysten rakenne](http://go.microsoft.com/fwlink/?LinkId=248093)

[Verkon määritys-tiedostojen käyttämisestä Virtual verkon määrittäminen](http://go.microsoft.com/fwlink/?LinkId=248094)

[Tietoja VPN-asetusten hallinta-portaalissa](http://go.microsoft.com/fwlink/?LinkId=248092)


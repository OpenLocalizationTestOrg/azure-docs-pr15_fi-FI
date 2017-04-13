<properties 
   pageTitle="Tarkasteleminen ja muokkaaminen isäntänimet | Microsoft Azure"
   description="Voit tarkastella ja muuttaa Azuren näennäiskoneiden isäntänimet, sivuston ja työntekijä roolien nimenselvitys"
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
   ms.date="04/27/2016"
   ms.author="jdial" />

# <a name="viewing-and-modifying-hostnames"></a>Tarkasteleminen ja muokkaaminen isäntänimet

Anna roolin kopioita isäntänimi viittaavan, rooleille palvelun määritystiedosto on määritettävä isäntänimen arvoa. Voit tehdä, lisäämällä haluamasi isäntänimi **rooli** -elementin **vmName** -määrite. **VmName** -määritteen arvon käytetään roolin jokaiselle esiintymälle isäntänimi pohjana. Esimerkiksi jos **vmName** on *webrole* ja on roolin kolmessa tilanteessa, esiintymien isäntänimet on *webrole0*, *webrole1*ja *webrole2*. Sinun ei tarvitse määrittää isäntänimen näennäiskoneiden määritystiedostossa, koska virtual machine isäntänimi lisätään virtuaalikoneen nimen perusteella. Saat lisätietoja Microsoft Azure-palvelun määrittäminen [Azure määritysten malli (.cscfg tiedoston)](https://msdn.microsoft.com/library/azure/ee758710.aspx)

## <a name="viewing-hostnames"></a>Isäntänimet tarkasteleminen

Voit tarkastella näennäiskoneiden ja-roolin esiintymien isäntänimet pilvipalvelussa käyttämällä alla olevia työkaluja.

### <a name="azure-portal"></a>Azure Portal

[Azure portal](http://portal.azure.com) avulla voit tarkastella näennäiskoneiden isäntänimien virtual tietokoneelle yhteenveto-sivu. Ota huomioon, että sivu näyttää arvon **nimi** ja **Isäntänimi**. Vaikka ne ovat aluksi samat, isäntänimi muuttaminen ei muuta virtuaalikoneen tai roolin esiintymän nimi.

Rooli esiintymiä voi katsella Azure-portaalissa, mutta kun pilvipalvelussa esiintymät-luettelosta isäntänimi ei ole näkyvissä. Näet jokaisen esiintymän nimi, mutta nimi ei vastaa isäntänimi.

### <a name="service-configuration-file"></a>Palvelun kokoonpanotiedosto

Palvelun kokoonpanotiedosto käyttöön palvelun voi ladata Azure-portaalissa-palvelun **määrittäminen** -sivu. Voit etsiä sitten Nähdäksesi isäntänimi **roolinimi** -elementin **vmName** -määrite. Ota huomioon, että tämä isäntänimi käytetään pohjana isäntänimi roolin kunkin esiintymän. Esimerkiksi jos **vmName** on *webrole* ja on roolin kolmessa tilanteessa, esiintymien isäntänimien on *webrole0*, *webrole1*ja *webrole2*.

### <a name="remote-desktop"></a>Etätyöpöytä

Kun olet ottanut käyttöön Etätyöpöytä (Windows), Windows PowerShell-Etäpalvelujen (Windows) tai (Linux ja Windows) SSH yhteydet näennäiskoneiden tai rooli esiintymät, voit tarkastella isäntänimi aktiivinen Etätyöpöytä-yhteyden eri tavalla:

- Kirjoita isäntänimi komentokehote tai SSH päätteen.

- Kirjoittamalla ipconfig/kaikki komentokehotteeseen (vain Windowsissa).

- Tarkastele tietokoneen järjestelmäasetuksia (vain Windows).

### <a name="azure-service-management-rest-api"></a>Azure hallinnan REST-Ohjelmointirajapinnalla

REST-asiakasohjelmassa noudattamalla seuraavia ohjeita:

1. Varmista, että Asiakasvarmenne muodostaa Azure-portaaliin. Asiakkaan sertifikaatin hankkiminen noudattamalla esitettyjä [miten: Lataa ja tuo Julkaisuasetukset ja Tilaustiedot](https://msdn.microsoft.com/library/dn385850.aspx). 

1. Määritä x-ms-versio ja 2013-11-01-arvo-otsikko-nimi.

1. Lähetä pyyntö seuraavassa muodossa: https://management.core.windows.net/\<subscrition tuotetunnus-\>/services/hostedservices/\<palvelun nimi\>?embed tiedot = tosi

1. Etsi **RoleInstance** jokaisen osan **isäntänimi** -elementti.

>[AZURE.WARNING] Voit tarkastella sisäinen jälkiliitteen cloud-palvelua muiden puhelun vastausta valitsemalla **InternalDnsSuffix** elementti tai suorittamalla ipconfig/kaikki komentoriviltä, Etätyöpöydän istunnon (Windows) tai käynnissä kissa /etc/resolv.conf-SSH-päätteen (Linux).

## <a name="modifying-a-hostname"></a>Isäntänimi muokkaaminen

Voit muokata virtuaalikoneen tai roolin esiintymän isäntänimi lataamalla muokattu palvelun kokoonpanotiedosto tai nimeämällä tietokoneen etätyöpöydän istunnosta.

## <a name="next-steps"></a>Seuraavat vaiheet

[Nimenselvitys (DNS)](virtual-networks-name-resolution-for-vms-and-role-instances.md)

[Azure määritysten malli (.cscfg)](https://msdn.microsoft.com/library/windowsazure/ee758710.aspx)

[Azure Virtual verkon määritysten rakenne](http://go.microsoft.com/fwlink/?LinkId=248093)

[Määritä DNS-asetukset, verkon määritysten tiedostojen käyttäminen](virtual-networks-specifying-a-dns-settings-in-a-virtual-network-configuration-file.md)

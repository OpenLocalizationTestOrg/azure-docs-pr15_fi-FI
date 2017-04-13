<properties
   pageTitle="Varattu IP | Microsoft Azure"
   description="Lisätietoja varattu IP-osoitteet sekä niiden hallintaa varten"
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
   ms.date="02/10/2016"
   ms.author="jdial" />

# <a name="reserved-ip-overview"></a>Varattu IP-yleiskatsaus
IP-osoitteiden Azure jakautuvat kahteen luokkaan: dynaaminen ja varattu. Julkisten IP-osoitteiden Azure hallitsee ovat dynaamisia oletusarvoisesti. Joka tarkoittaa, että IP-osoitetta käytetään tietyn pilvipalvelussa (VIP) tai voit käyttää esimerkiksi AM tai rooli esiintymän suoraan (ILPIP) voit muuttaa ajoittain, kun resursseja on suljettu tai Varaus poistettu.

Jos haluat estää muuttamasta IP-osoitteet, voit varata IP-osoite. Varattu IP-osoitteet voidaan käyttää vain VIP, joilla varmistetaan, että cloud-palvelun IP-osoite on sama huolimatta, kun resursseja on suljettu tai Varaus poistettu. Lisäksi voit muuntaa käytetään varattu IP-osoitteeseen VIP aiemmin dynaaminen IP-osoitteet.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Lue, miten voit varata staattinen julkiseen IP-osoite [resurssien hallinnan käyttöönottomalli](virtual-network-ip-addresses-overview-arm.md).

Varmista, että tiedät, miten [IP-osoitteiden](virtual-network-ip-addresses-overview-classic.md) toimii Azure.

## <a name="when-do-i-need-a-reserved-ip"></a>Milloin tarvitsen varattu IP?
- Jos **haluat varmistaa, että IP on varattu-tilaukseesi**. Jos haluat varata IP-osoite, joka ei ole julkaistaan tilauksesta missään tilanteessa, sinun on käytettävä varattu julkiseen IP.  
- **Haluat IP yhteydenpitoon oman pilvipalvelussa, jopa monista pysäytetty tai Varaus poistettu tila (VMs)**. Jos haluat käyttää IP-osoitetta, joka ei muutu palvelun silloinkin, kun VMs pilvipalvelussa ovat Lopeta tai Varaus poistettu.
- Jos **haluat varmistaa, että lähtevän tietoliikenteen azuren käyttää ennakoitavissa IP-osoite**. On ehkä määritetty sallimaan vain tiettyjen IP-osoitteiden-liikenne paikalliseen palomuurin. Varaaminen IP-tietävät lähde-IP-osoite ja ei tarvitse päivittää palomuurisäännöt IP-muuta vuoksi.

## <a name="faq"></a>USEIN KYSYTYT KYSYMYKSET
1. Varattu IP käyttää kaikissa Azure-palveluissa  
  - Varattu IP-osoitteet voi käyttää vain VMs ja cloud palvelun esiintymän roolit tarjoamia VIP kautta.
1. Kuinka monta varattu IP-osoitteet voi olla?  
  - Tällä hetkellä kaikki Azure tilaukset on oikeutettu käyttämään 20 varattu IP-osoitteet. Voit kuitenkin pyytää muita varattu IP-osoitteet. Lisätietoja lisätietoja [tilaus ja palvelun rajoitukset](../azure-subscription-service-limits.md) -sivulla.
1. Onko varattu IP-osoitteet varausta?
  - Alla on tietoja on [Varattu IP Address hinnoittelu-tiedot](http://go.microsoft.com/fwlink/?LinkID=398482) .
1. Miten IP-osoitteen varata?
  - Voit käyttää PowerShell tai [Azure hallinta REST API](https://msdn.microsoft.com/library/azure/dn722420.aspx) varata tietyn alueen IP-osoite. Tämä varattu IP-osoite on liitetty tilauksen. Et voi varata IP-osoitteen avulla hallinta-portaalin.
1. Voinko käyttää tätä affiniteetti ryhmän VNets perusteella?
  - Varattu IP-osoitteet tuetaan vain alueellisen VNets. Se ei tueta VNets, jotka liittyvät affiniteetti ryhmät. Lisätietoja liittämisestä VNet alueen tai affiniteetti-ryhmässä on [tietoja alueellisen VNets ja affiniteetti ryhmät](virtual-networks-migrate-to-regional-vnet.md).

## <a name="how-to-manage-reserved-vips"></a>Varattu VIPs hallinta

Ennen kuin voit käyttää varattu IP-osoitteet, sinun on lisättävä tilauksen. Voit luoda varattu IP julkisten IP-osoitteiden käytettävissä *Yhdysvaltojen Keski* -sijaintiin varannon suorittamalla seuraavan PowerShell-komennon:

    New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US"

Huomaa kuitenkin, että et voi määrittää IP parhaillaan varattu. Voit tarkastella tietoja IP-osoitteiden on varattu-tilaukseesi, suorita seuraava komento PowerShell ja Huomaa arvot *ReservedIPName* ja *osoite*:

    Get-AzureReservedIP

    ReservedIPName       : MyReservedIP
    Address              : 23.101.114.211
    Id                   : d73be9dd-db12-4b5e-98c8-bc62e7c42041
    Label                :
    Location             : Central US
    State                : Created
    InUse                : False
    ServiceName          :
    DeploymentName       :
    OperationDescription : Get-AzureReservedIP
    OperationId          : 55e4f245-82e4-9c66-9bd8-273e815ce30a
    OperationStatus      : Succeeded

Kun IP on varattu, se säilyy liitetty tilauksen, kunnes poistat sen. Voit poistaa yllä varattu IP suorittamalla seuraavan PowerShell-komennon:

    Remove-AzureReservedIP -ReservedIPName "MyReservedIP"

## <a name="how-to-reserve-the-ip-address-of-an-existing-cloud-service"></a>Voit varata aiemmin pilvipalvelussa IP-osoite

Voit varata aiemmin pilvipalvelussa IP-osoitteen lisäämällä *Palvelun nimi -* parametrin. Varaa pilvipalvelussa *TestService* *Yhdysvaltojen Keski* -sijaintiin IP-osoite, suorittaa seuraavan PowerShell-komennon:

    New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US" -ServiceName TestService


## <a name="how-to-associate-a-reserved-ip-to-a-new-cloud-service"></a>Voit liittää varattu IP uusi pilvipalveluun
Alla olevaa komentosarjaa Luo uusi varattu IP ja sitten yhdistää pilvipalveluun uusi nimeltä *TestService*.

    New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US"
    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name TestVM -InstanceSize Small -ImageName $image.ImageName `
  	| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
  	| New-AzureVM -ServiceName TestService -ReservedIPName MyReservedIP -Location "Central US"

>[AZURE.NOTE] Kun luot varattu IP käytettäväksi pilvipalveluun, sinun on edelleen nähdäksesi AM käyttämällä *VIP:&lt;porttinumero >* saapuvien viestintään. Varaaminen IP ei tarkoita voit muodostaa yhteyden AM suoraan. Varattu IP määritetään pilvipalveluun, joka AM on otettu käyttöön. Jos haluat muodostaa yhteyden suoraan AM IP-osoitteita, sinun on määritettävä esiintymän tason julkiseen IP. Esiintymän tason julkiseen IP on julkinen (nimi ILPIP) IP, joka on määritetty suoraan yhteyttä AM. Se ei voi varata. Lisätietoja on kohdassa [Esiintymän tason julkiseen IP (ILPIP)](virtual-networks-instance-level-public-ip.md) .

## <a name="how-to-remove-a-reserved-ip-from-a-running-deployment"></a>Varattu IP poistamisesta käynnissä käyttöönotto
Voit poistaa lisätään uusi palvelu luotu edellä komentosarja varattu IP suorittamalla seuraavan PowerShell-komennon:

    Remove-AzureReservedIPAssociation -ReservedIPName MyReservedIP -ServiceName TestService

>[AZURE.NOTE] Varattu IP poistaminen käynnissä käyttöönoton Poista varauksen tilauksesta. Se vapauttaa yksinkertaisesti IP käytettävän toiselle resurssille-tilaukseesi.

## <a name="how-to-associate-a-reserved-ip-to-a-running-deployment"></a>Voit liittää varattu IP käynnissä käyttöönottoon
Alla olevaa komentosarjaa Luo uusi pilvipalvelussa nimeltä *TestService2* uusi AM, nimeltä *TestVM2*kanssa ja liittää aiemmin varattu IP nimeltä *MyReservedIP* pilvipalveluun.

    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name TestVM2 -InstanceSize Small -ImageName $image.ImageName `
  	| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
  	| New-AzureVM -ServiceName TestService2 -Location "Central US"
    Set-AzureReservedIPAssociation -ReservedIPName MyReservedIP -ServiceName TestService2

## <a name="how-to-associate-a-reserved-ip-to-a-cloud-service-by-using-a-service-configuration-file"></a>Voit liittää varattu IP pilvipalveluun käyttämällä palvelun kokoonpanotiedosto
Voit myös liittää varattu IP pilvipalveluun palvelun määritys (CSCFG)-tiedoston avulla. Esimerkki xml-alla näkyy määrittäminen käyttämään nimeltä *MyReservedIP*varattu VIP pilvipalveluun:

    <?xml version="1.0" encoding="utf-8"?>
    <ServiceConfiguration serviceName="ReservedIPSample" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="4" osVersion="*" schemaVersion="2014-01.2.3">
      <Role name="WebRole1">
        <Instances count="1" />
        <ConfigurationSettings>
          <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
        </ConfigurationSettings>
      </Role>
      <NetworkConfiguration>
        <AddressAssignments>
          <ReservedIPs>
           <ReservedIP name="MyReservedIP"/>
          </ReservedIPs>
        </AddressAssignments>
      </NetworkConfiguration>
    </ServiceConfiguration>

## <a name="next-steps"></a>Seuraavat vaiheet

- Tietoja [IP-osoitteiden](virtual-network-ip-addresses-overview-classic.md) toiminta perinteinen käyttöönotto-mallissa.

- Lisätietoja [varattu yksityiset IP-osoitteet](virtual-networks-reserved-private-ip.md).

- Lisätietoja [esiintymän tason julkiseen IP (ILPIP)-osoitteista](virtual-networks-instance-level-public-ip.md).

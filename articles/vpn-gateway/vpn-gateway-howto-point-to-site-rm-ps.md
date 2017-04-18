<properties 
   pageTitle="Resurssien hallinnan käyttöönotto mallin virtual verkkoon pisteen sivuston VPN yhdyskäytävä-yhteyden määrittäminen | Microsoft Azure"
   description="Suojattu yhteys Azure Virtual verkon luomalla pisteen sivuston VPN-yhdyskäytävän yhteys."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/17/2016"
   ms.author="cherylmc" />

# <a name="configure-a-point-to-site-connection-to-a-vnet-using-powershell"></a>VNet, PowerShellin pisteen sivuston yhteyden asetusten määrittäminen

> [AZURE.SELECTOR]
- [Resurssien hallinta - portaalissa Azure](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
- [Resurssien hallinta - PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
- [Perinteinen - Azure Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)

Pisteen sivuston (P2S) määrityksen voit luoda suojatun yhteyden yksittäisiä asiakastietokoneesta virtual verkkoon. P2S yhteyden on hyödyllinen, kun haluat muodostaa yhteyttä VNet remote sijainnista, kuten kotona tai konferenssiin, tai jos sinulla on vain muutama asiakkaat, jotka on virtual verkkoon. 

Pisteen sivuston yhteydet, eivät edellytä VPN-laitetta tai julkiselle IP-osoite, jos haluat tutustua. VPN-yhteys on muodostettu yhteys käynnistämällä asiakastietokoneesta. Saat lisätietoja pisteen sivuston yhteydet [VPN yhdyskäytävän usein kysytyt kysymykset](vpn-gateway-vpn-faq.md#point-to-site-connections) ja [suunnittelu- ja rakenne](vpn-gateway-plan-design.md). 

Tässä artikkelissa käydään läpi luomalla VNet pisteen sivuston yhteyden Resurssienhallinta käyttöönoton mallin PowerShellin avulla.

### <a name="deployment-models-and-methods-for-p2s-connections"></a>Käyttöönotto-malleja ja viestintämenetelmien P2S yhteydet

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

Seuraavassa taulukossa on kaksi käyttöönoton mallit ja käytettävissä käyttöönoton viestintämenetelmien P2S määrityksiä. Kun määritysvaiheet artikkeli on käytettävissä, on linkki suoraan tästä taulukosta.

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)] 

## <a name="basic-workflow"></a>Tavallinen työnkulku 

![Piste-,-sivusto-kaavion] (./media/vpn-gateway-howto-point-to-site-rm-ps/p2srm.png "pisteen sivuston")

Tässä skenaariossa luodaan virtual verkko on kohdassa sivuston yhteys. Ohjeiden avulla voit luoda sertifikaatteja, joita tarvitaan määritysten. P2S yhteyden koostuu seuraavat kohteet: A VNet VPN-yhdyskäytävän, pääkansion varmenteen .cer-tiedosto (julkinen avain), Asiakasvarmenne ja asiakkaan VPN-määritys. 

Käytämme tässä kokoonpanossa seuraavat arvot. Olemme Määritä muuttujat tämän artikkelin kohdassa [1](#declare) . Voit joko käyttää vaiheet hallintapaketteihin ja käyttämällä arvot muuttamatta niitä tai muuttaa ne vastaamaan ympäristön. 

### <a name="example"></a>Esimerkkiarvot

- **Nimi: VNet1**
- **Osoite tila: 192.168.0.0/16** ja **10.254.0.0/16**<br>Tässä esimerkissä Käytämme useita osoitetilaa esitä määritysten toimivat useita osoite välilyöntejä. Useiden osoite välilyöntien ei kuitenkaan vaatia määritysten.
- **Aliverkon nimi: FrontEnd**
    - **Aliverkon osoitealueita: 192.168.1.0/24**
- **Aliverkon nimi: taustaan**
    - **Aliverkon osoitealueita: 10.254.1.0/24**
- **Aliverkon nimi: GatewaySubnet**<br>*GatewaySubnet* aliverkon nimi on pakollinen toimimaan VPN-yhdyskäytävä.
    - **Aliverkon osoitealueita: 192.168.200.0/24** 
- **VPN asiakkaan osoitteen resurssivarantoon: 172.16.201.0/24**<br>VPN-asiakkaille, jotka muodostavat yhteyden käyttämällä pisteen sivuston tätä yhteyttä VNet saavat IP-osoitteen varannon VPN-asiakasohjelman osoite.
- **Tilaus:** Jos sinulla on useita tilauksia, varmista, että käytät oikeaa.
- **Resurssiryhmä: TestRG**
- **Sijainti: Itä US**
- **DNS-palvelin: IP-osoite** DNS-palvelimen, jota haluat käyttää nimenselvitys.
- **GW nimi: Vnet1GW**
- **Julkinen IP-nimi: VNet1GWPIP**
- **VpnType: RouteBased**

## <a name="before-beginning"></a>Ennen kuin aloitat

- Varmista, että sinulla on Azure tilaus. Jos sinulla ei vielä ole Azure tilaus, voit ottaa [MSDN tilaajan edut](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) tai sisäänkirjautumisen määrittäminen [ilmainen tili](https://azure.microsoft.com/pricing/free-trial/).
    
- Asenna uusin versio Azure Resurssienhallinta PowerShell cmdlet-komentoja. Katso, [miten voit asentaa ja määrittää PowerShellin Azure](../powershell-install-configure.md) lisätietoja asentaminen PowerShellin cmdlet-komennot. Tässä määrityksessä PowerShellin Varmista, että käytät järjestelmänvalvojana. 

## <a name="declare"></a>Osa 1 - Kirjaudu sisään ja määritä muuttujat

Tässä osassa Kirjaudu sisään ja määritellä määritysten käytettävät arvot. Ilmoitettu arvoja käytetään komentosarjamallit. Muuta vastaamaan oman ympäristön arvoja. Tai voit käyttää ilmoitettu arvot ja käymällä läpi vaiheet hioa nimellä.

1. PowerShell-konsolissa kirjautua Azure-tili. Tämä cmdlet-komento kysyy kirjautumisen tunnistetietoja Azure-tiliin. Kirjautuminen, kun se lataa tilin asetukset niin, että ne ovat käytettävissä PowerShellin Azure.

        Login-AzureRmAccount 

2. Saat luettelon käytettävissä Azure tilaukset.

        Get-AzureRmSubscription

3. Määritä tilaus, jota haluat käyttää. 

        Select-AzureRmSubscription -SubscriptionName "Name of subscription"

4. Määritä muuttujat, jota haluat käyttää. Käytä seuraavassa esimerkissä korvaaminen Omat tarvittaessa arvot.

        $VNetName  = "VNet1"
        $FESubName = "FrontEnd"
        $BESubName = "Backend"
        $GWSubName = "GatewaySubnet"
        $VNetPrefix1 = "192.168.0.0/16"
        $VNetPrefix2 = "10.254.0.0/16"
        $FESubPrefix = "192.168.1.0/24"
        $BESubPrefix = "10.254.1.0/24"
        $GWSubPrefix = "192.168.200.0/26"
        $VPNClientAddressPool = "172.16.201.0/24"
        $RG = "TestRG"
        $Location = "East US"
        $DNS = "8.8.8.8"
        $GWName = "VNet1GW"
        $GWIPName = "VNet1GWPIP"
        $GWIPconfName = "gwipconf"
        
## <a name="ConfigureVNet"></a>Osa 2 – Määritä VNet 

1. Luo resurssiryhmä.

        New-AzureRmResourceGroup -Name $RG -Location $Location

2. Luo virtuaalisia verkkoon, *FrontEnd*, *Taustajärjestelmä*ja *GatewaySubnet*nimeämisestä ne aliverkon määritykset. Seuraavia etuliitteitä on kuuluttava VNet osoitetilaa, joita voit määritetty.

        $fesub = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName -AddressPrefix $FESubPrefix
        $besub = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName -AddressPrefix $BESubPrefix
        $gwsub = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName -AddressPrefix $GWSubPrefix

3. Luo virtuaalisia verkkoon. Määritetty DNS-palvelin on oltava DNS-palvelin, jossa voit ratkaista yrität muodostaa yhteyden resurssien nimet. Tässä esimerkissä on käytetty julkiseen IP-osoite. Muista käyttää omia arvoja.
    
        New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG -Location $Location -AddressPrefix $VNetPrefix1,$VNetPrefix2 -Subnet $fesub, $besub, $gwsub -DnsServer $DNS

4. Määritä luomasi virtual verkon muuttujat.

        $vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
        $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet

5. Pyydä dynaamisesti määritetty julkiseen IP-osoite. Tätä IP-osoitetta tarvitaan yhdyskäytävän toimii oikein. Luot yhteyden yhdyskäytävä yhdyskäytävän IP-määritys.
        
        $pip = New-AzureRmPublicIpAddress -Name $GWIPName -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic
        $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip

## <a name="Certificates"></a>Osa 3 - varmenteet

Azure käyttävät sertifikaatteja tarkistamiseen pisteen sivuston VPN-yhteydet VPN-asiakkaille. Voit viedä Base-64-koodattu X.509 .cer-tiedosto enterprise-todistus-ratkaisun luoma pääkansion varmenteen tai pääkansion itse allekirjoitetun varmenteen julkisten varmenteen tiedot (ei yksityinen avain). Valitse tuotava julkisen varmenteen tietojen varmenteen päämyöntäjien Azure. Lisäksi tarvitset Luo Asiakasvarmenne varmenteen päämyöntäjien asiakkaille. Kukin asiakas haluaa virtual verkkoon P2S-yhteyden avulla, on oltava asiakkaan varmenne, joka on luotu varmenteen päämyöntäjien asennettuna.

### <a name="cer"></a>1. hankkiminen varmenteen päämyöntäjien .cer-tiedosto

Tarvitset varmenteen julkisten tietojen hakeminen pääkansio-varmenne, jota haluat käyttää.

- Jos käytössäsi on enterprise-sertifikaatin järjestelmän, hanki pääkansio-varmenne, jota haluat käyttää .cer-tiedoston. 

- Jos et käytä enterprise-todistus-ratkaisun, tarvitset pääkansion itse allekirjoitetun sertifikaatin luomiseen. Windows 10-ohjeita on voidaan viitata [käyttäminen itse allekirjoitettua päämyöntäjien varmenteet pisteen sivuston määrityksiä](vpn-gateway-certificates-point-to-site.md).


1. Saadakseen .cer-tiedosto varmenteen Avaa **certmgr.msc** ja Etsi pääkansion varmenne. Kaksoisnapsauta pääkansion itse allekirjoitettua varmennetta, **kaikki**tehtävät ja valitse sitten **Vie**. **Varmenteen vieminen-toiminnossa**avautuu.

2. Ohjatun Valitse **Seuraava**, valitse **ei, älä vie yksityinen avain**ja valitse sitten **Seuraava**.

3. Valitse **Viennin tiedostomuoto** -sivulla Valitse **Base-64-koodattu X.509 (. CER).** Valitse sitten **Seuraava**. 

4. Napsauta **Tiedoston vieminen** **Selaa** haluamaasi kohtaan, johon haluat viedä varmenne. **Tiedostonimi**-sertifikaatin tiedoston nimeksi. Valitse sitten **Seuraava**.

5. Valitse varmenteen vieminen **loppuun** .

### <a name="generate"></a>2. Luo asiakkaan varmenne

Luo seuraavaksi asiakkaan varmenteet. Voit luoda yksilölliset varmenteen joko kunkin asiakkaan, joka muodostaa yhteyden, tai voit käyttää samaa varmennetta useasta asiakasohjelmasta käsin. Luodaan yksilöllinen asiakkaan varmenteet etuna on mahdollisuus kumota yhtä varmennetta tarvittaessa. Muussa tapauksessa kaikki on käytössä sama Asiakasvarmenne ja huomaat, että haluat kumota yksi asiakas varmennetta, jos tarvitset Luo ja asentaa uusia sertifikaatteja kaikki ohjelmat, joita käyttää varmenteen todentamiseen. Kunkin asiakaskoneen myöhemmin tämän Harjoitus-asiakas-varmenteet on asennettu.

- Jos käytössäsi on enterprise-todistus-ratkaisun, Luo Asiakasvarmenne yleisiä nimi-arvo muodossa 'name@yourdomain.com', sijaan NetBIOS "Toimialue\käyttäjänimi" Muotoile. 

- Jos käytössäsi on itse allekirjoitettua varmennetta, nähdä luomiseen Asiakasvarmenne [käyttäminen itse allekirjoitettua päämyöntäjien varmenteet pisteen sivuston määrityksiä](vpn-gateway-certificates-point-to-site.md) .

### <a name="exportclientcert"></a>3. asiakas-varmenteen vieminen

Asiakasvarmenne tarvitaan todennusta varten. Sen jälkeen luodaan sertifikaattia, vie se. Voit viedä Asiakasvarmenne asennetaan myöhemmin kunkin asiakaskoneen.

1. Voit viedä Asiakasvarmenne, voit käyttää *certmgr.msc*. Napsauta hiiren kakkospainikkeella Asiakasvarmenne, jonka haluat viedä, valitse **Kaikki tehtävät**ja valitse sitten **Vie**.
2. Vie asiakkaan varmenne ja yksityinen avain. Tämä on *.pfx* -tiedosto. Varmista, että tallentaa tai muista salasana (avain), jonka olet määrittänyt tämän todistuksen.

### <a name="upload"></a>4. Lataa pääkansion varmenteen .cer-tiedosto

Määrittää varmenteen nimi-arvo korvaaminen omalla muuttujaa:

        $P2SRootCertName = "Mycertificatename.cer"

Varmenteen päämyöntäjien julkisen varmenne-tietojen lisääminen Azure. Voit ladata tiedostoja enintään 20 varmenteet. Ei ladata Azure varmenteen päämyöntäjien yksityinen avain. Kun .cer-tiedosto on ladattu palvelimeen, Azure käyttää sitä todennetaan asiakkaat, jotka virtual verkkoon. 

Korvaa tiedoston omalla ja suorita sitten Cmdlet-komentoja.
    
        $filePathForCert = "C:\cert\Mycertificatename.cer"
        $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($filePathForCert)
        $CertBase64 = [system.convert]::ToBase64String($cert.RawData)
        $p2srootcert = New-AzureRmVpnClientRootCertificate -Name $P2SRootCertName -PublicCertData $CertBase64

## <a name="creategateway"></a>Osa 4 - Luo VPN-yhdyskäytävän

Määritä ja luoda oman VNet VPN-yhdyskäytävä. *-GatewayType* on oltava **Vpn** ja *-VpnType* on oltava **RouteBased**. Tämä voi kestää jopa 45 minuuttia.

        New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG `
        -Location $Location -IpConfigurations $ipconf -GatewayType Vpn `
        -VpnType RouteBased -EnableBgp $false -GatewaySku Standard `
        -VpnClientAddressPool $VPNClientAddressPool -VpnClientRootCertificates $p2srootcert

## <a name="clientconfig"></a>Osa 5 – VPN asiakasohjelman määritysten paketin lataaminen

Yhteyden muodostaminen käyttämällä P2S Azure asiakkaiden on oltava Asiakasvarmenne ja asennettu VPN asiakasohjelman määritysten paketin. VPN asiakasohjelman määritysten paketit ovat Windows-asiakkaiden käytettävissä. VPN-asiakasohjelman paketti sisältää tiedot, Windowsin ja on VPN-yhteyttä, johon haluat muodostaa VPN-Asiakasohjelmistoa määrittämiseen. Paketin ei asennu lisäohjelmistoa. Katso lisätietoja [VPN yhdyskäytävän usein kysytyt kysymykset](vpn-gateway-vpn-faq.md#point-to-site-connections) .

1. Kun yhdyskäytävä on luotu, voit ladata asiakasohjelman määritysten paketin. Tässä esimerkissä Lataa paketin 64-bittinen asiakkaille. Jos haluat ladata 32-bittinen asiakas, tilalle 'x86' "Amd64". Voit ladata VPN-asiakasohjelman myös Azure-portaalissa.

        Get-AzureRmVpnClientPackage -ResourceGroupName $RG `
        -VirtualNetworkGatewayName $GWName -ProcessorArchitecture Amd64

2. PowerShell-cmdlet-komento palauttaa URL-linkkiä. Seuraavassa on esimerkki mitä palautetut URL-osoite näyttää:

        "https://mdsbrketwprodsn1prod.blob.core.windows.net/cmakexe/4a431aa7-b5c2-45d9-97a0-859940069d3f/amd64/4a431aa7-b5c2-45d9-97a0-859940069d3f.exe?sv=2014-02-14&sr=b&sig=jSNCNQ9aUKkCiEokdo%2BqvfjAfyhSXGnRG0vYAv4efg0%3D&st=2016-01-08T07%3A10%3A08Z&se=2016-01-08T08%3A10%3A08Z&sp=r&fileExtension=.exe"

3. Kopioi ja Liitä linkki, joka palautetaan selaimessa, voit ladata paketin. Valitse Asenna paketti asiakastietokoneen. Jos saat SmartScreen-valikko, valitse **lisätietojen**, sitten asentaminen paketin **suorittaminen silti** .

4. Asiakastietokoneen Siirry **Verkkoasetukset** ja valitse **VPN-yhteyttä**. Näet luettelossa yhteys. Se näytetään nimi virtual verkko, jossa se muodostaa yhteyden ja näyttää samalla tavalla kuin tässä esimerkissä: 

    ![VPN-asiakas] (./media/vpn-gateway-howto-point-to-site-rm-ps/vpn.png "VPN-asiakas")


## <a name="clientcertificate"></a>Osa 6 - asentaminen asiakkaan varmenne

Kunkin asiakaskoneen täytyy olla Asiakasvarmenne, jotta todennusta. Kun asennat sertifikaattia, sinun on salasanaa, jolla on luotu sertifikaattia vietäessä.

1. Kopioi asiakastietokone .pfx-tiedosto.
2. Kaksoisnapsauta asennusohjeet .pfx-tiedosto. Älä muokkaa asennuksen sijainti.

## <a name="connect"></a>7 - osan yhdistäminen Azure

1. Muodostaa yhteyttä VNet asiakastietokoneen, siirry VPN-yhteydet ja Etsi VPN-yhteyden, jonka loit. Sen nimi on sama nimi kuin virtual verkossa. Valitse **Muodosta yhteys**. Ponnahdusikkunoiden viesti saattaa tulla näkyviin, joka viittaa sertifikaatin avulla. Jos näin käy, valitse **Jatka** käyttämään laajentamiseen. 

2. **Yhteyden** tila-sivulla valitsemalla **Muodosta** yhteys käynnistämiseen. Jos näkyviin **Valitse varmenne** -viesti, varmista, että Asiakasvarmenne, jossa on yksi, johon haluat muodostaa. Jos se ei ole, käyttää oikea sertifikaatti avattavan luettelon nuolta ja valitse sitten **OK**.

    ![Asiakkaan VPN-yhteyden] (./media/vpn-gateway-howto-point-to-site-rm-ps/clientconnect.png "Asiakkaan VPN-yhteyden")

3. Yhteys nyt vahvistettava.

    ![Yhteyttä] (./media/vpn-gateway-howto-point-to-site-rm-ps/connected.png "Yhteyttä")

## <a name="verify"></a>Osa 8 - Tarkista yhteys

1. Varmista, että VPN-yhteys on aktiivinen, Avaa järjestelmänvalvojan oikeuksin suoritettava komentokehote ja suorita *ipconfig/kaikki*.

2. Tarkastele tuloksia. Huomaa, että olet saanut IP-osoite on yksi osoitteet pisteen sivuston VPN asiakkaan osoitteen varannon, jonka olet määrittänyt kokoonpanoa kuluessa. Tuloksena saadaan jotakin tämän tapaista:
    
        PPP adapter VNet1:
            Connection-specific DNS Suffix .:
            Description.....................: VNet1
            Physical Address................:
            DHCP Enabled....................: No
            Autoconfiguration Enabled.......: Yes
            IPv4 Address....................: 172.16.201.3(Preferred)
            Subnet Mask.....................: 255.255.255.255
            Default Gateway.................:
            NetBIOS over Tcpip..............: Enabled

## <a name="addremovecert"></a>Voit lisätä tai poistaa luotetun varmenteiden varmenne

Varmenteita käytetään tarkistamiseen pisteen sivuston VPN-yhteydet VPN-asiakkaille. Seuraavat vaiheet käy läpi lisäämällä ja poistamalla varmenteet. Kun lisäät Base64-koodattu X.509 (.cer) tiedoston Azure-ovat kehottaa Azure luotettava ylimmällä tasolla, joka edustaa tiedoston. 

Voit lisätä tai poistaa varmenteet PowerShell-toiminnolla tai Azure-portaalissa. Jos haluat tehdä tämän Azure-portaalissa, siirry oman **VPN-yhdyskäytävän > Asetukset > pisteen sivuston määritysten > pääkansio varmenteiden**. Seuraavat vaiheet opastusta näiden tehtävien PowerShellin avulla. 

### <a name="add-a-trusted-root-certificate"></a>Lisää luotetun varmenteiden varmenne

Voit lisätä enintään 20 luotetun varmenteiden varmenteen .cer-tiedostoja Azure. Noudata seuraavia ohjeita voit lisätä varmenteen päämyöntäjien.

1. Luominen ja valmisteleminen uuden pääkansion-varmenne, jota Azure lisätään. Vie julkisella avaimella, kuten Base-64-koodattu X.509 (. CER) ja avaa se tekstieditorissa. Kopioi vain osan alla. 
 
    Kopioi arvot, kuten seuraavassa esimerkissä:

    ![varmenne] (./media/vpn-gateway-howto-point-to-site-rm-ps/copycert.png "varmenne")
    
2. Määritä varmenteen nimi ja avaintiedoista muuttujana. Korvaa tiedot oman seuraavassa esitetyllä tavalla Esimerkki:

        $P2SRootCertName2 = "ARMP2SRootCert2.cer"
        $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"


3. Lisää uusi pääkansion varmenne. Voit lisätä vain yksi kerrallaan.

        Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayname "VNet1GW" -ResourceGroupName "TestRG" -PublicCertData $MyP2SCertPubKeyBase64_2


4. Voit varmistaa, että uusi varmenne on lisätty oikein seuraavat cmdlet-komennolla.

        Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
        -VirtualNetworkGatewayName "VNet1GW"

### <a name="to-remove-a-trusted-root-certificate"></a>Jos haluat poistaa luotetun varmenteiden varmenne

Voit poistaa Azure luotetun varmenteiden varmenne. Kun poistat luotettu varmenteen, asiakkaan sertifikaatteja, joita luotiin varmenteen päämyöntäjien ei enää voi muodostaa Azure pisteen sivuston kautta. Jos haluat muodostaa asiakkaat, ne on asennettava uusi Asiakasvarmenne, joka on luotu sertifikaatti, joka on luotettu Azure.

1. Jos haluat poistaa luotetun varmenteiden varmenne, Muokkaa seuraavassa esimerkissä:

    Määritä muuttujat.

        $P2SRootCertName2 = "ARMP2SRootCert2.cer"
        $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"

2. Poista varmenne.

        Remove-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -PublicCertData $MyP2SCertPubKeyBase64_2
 
3. Seuraavan cmdlet-komennon avulla voit varmistaa, että varmenne on poistettu. 

        Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
        -VirtualNetworkGatewayName "VNet1GW"

## <a name="revoke"></a>Voit hallita käytettävissäsi olevia kumottu asiakkaan varmenteet

Voit kumota asiakkaan varmenteet. Kumottujen varmenteiden luettelo voit estää valikoivasti pisteen sivuston connectivity yksittäisiä asiakkaan varmenteet perusteella. Jos poistat pääkansion varmenne-.cer Azure, poistaa kaikki asiakkaan varmenteet luodaan ja kirjautunut kumottu pääkansio-todistus-käyttöä. Jos haluat kumota tietyn Asiakasvarmenne ei pääkansio, tee niin. Sen mukaan, joka luotiin varmenteen päämyöntäjien varmenteet on edelleen voimassa.

Yleinen käytäntö on pääkansio sertifikaatin avulla voit hallita ryhmän tai organisaation tasolla, access tarkasti rajattuja valvoa yksittäisten käyttäjien kumottu asiakkaan varmenteet ilmenevän vian.

### <a name="revoke-a-client-certificate"></a>Peruuttaa Asiakasvarmenne

1. Pyydä kumota Asiakasvarmenne allekirjoitus.

        $RevokedClientCert1 = "ClientCert1"
        $RevokedThumbprint1 = "‎ef2af033d0686820f5a3c74804d167b88b69982f"

2. Lisää allekirjoitus kumottu allekirjoitus luettelo.

        Add-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
        -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -Thumbprint $RevokedThumbprint1

3. Varmista, että allekirjoitus on lisätty kumottujen varmenteiden luettelo. Sinun on lisättävä allekirjoitus yksi kerrallaan.

        Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG

### <a name="reinstate-a-client-certificate"></a>Palauttaa Asiakasvarmenne

Voit palauttaa Asiakasvarmenne poistamalla asiakassertifikaattien-luettelosta allekirjoitus.

1.  Poista allekirjoitus kumottu asiakkaan varmenteen allekirjoitus-luettelosta.

        Remove-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
        -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -Thumbprint $RevokedThumbprint1

2. Tarkista, että allekirjoitus on poistettu kumottu luettelosta.

        Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG

## <a name="next-steps"></a>Seuraavat vaiheet

Voit lisätä virtual machine virtual verkkoon. Lisätietoja on kohdassa [Create Virtual Machine](../virtual-machines/virtual-machines-windows-hero-tutorial.md) ohjeita.
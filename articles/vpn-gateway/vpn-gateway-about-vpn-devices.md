<properties 
   pageTitle="Tietoja VPN-laitteiden sivusto sivusto VPN-yhdyskäytävän yhteyksien Azure Virtual verkkojen | Microsoft Azure"
   description="Tässä artikkelissa käsitellään VPN-laitteet ja S2S VPN-yhdyskäytävän yhteyksien IP-parametrit ja on linkkejä ohjeita ja esimerkkejä."
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor=""
  tags="azure-resource-manager, azure-service-management"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/13/2016"
   ms.author="yushwang;cherylmc" />

# <a name="about-vpn-devices-for-site-to-site-vpn-gateway-connections"></a>Tietoja sivuston sivuston VPN-yhdyskäytävän yhteyksien VPN-laitteet

Sivusto (S2S) VPN-yhteyden määrittäminen edellyttää VPN-laite. Sivuston sivuston yhteydet voidaan luoda hybrid ratkaisun tai aina, kun haluat suojatun yhteyden paikallisen verkon ja virtual verkon välille. Tässä artikkelissa käsitellään yhteensopiva VPN-laitteet ja määrityksiä. 

>[AZURE.NOTE] Kun sivusto yhteyden määrittämisestä julkisen IPv4-IP-osoite vaaditaan VPN-laitteeseesi.                                                                                                                                                                               

Jos laitteesi ei ole [vahvistettu VPN-laitteet](#devicetable) -taulukossa, katso tässä artikkelissa [Non vahvistetaan VPN-laitteet](#additionaldevices) -osio. On mahdollista, että laitteesi voivat edelleen käyttää Azure. VPN-laitetuki pyydä laitteen valmistajalta.

**Huomautus Kun tarkastelet taulukoiden kohteet:**

- Järjestelmässä on staattinen ja dynaaminen reititys termejä muutosta. Kohtaat todennäköisesti molemmat ehdot. Ei ole tekemät muutokset, vain nimet muutetaan.
    - Staattinen reititys = PolicyBased
    - Dynaaminen reititys = RouteBased
- Suuri suorituskyvyn VPN-yhdyskäytävän ja RouteBased VPN-yhdyskäytävän määritykset ovat samat, ellei muuta ole mainittu. Vahvistettu VPN-laitteet ja RouteBased VPN yhdyskäytävät yhteensopivissa ovat esimerkiksi myös Azure hyvin suorituskyvyn VPN-yhdyskäytävä-yhteensopiva. 


## <a name="devicetable"></a>Vahvistettu VPN-laitteet 

Olemme tarkistaneet vakio VPN-laitteiden yhteistyössä laitteen toimittajien kanssa. Seuraava luettelo sisältää laitteen perheille kaikissa laitteissa toimii Azure VPN yhdyskäytävät. Katso [Tietoja VPN-yhdyskäytävän](vpn-gateway-about-vpngateways.md) vahvistamiseksi yhdyskäytävään, joka on luotava haluat määrittää ratkaisun tyyppi. 

Jotta VPN-laitteen määrittäminen viitata linkkejä, jotka vastaavat sopiva laite perhe. VPN-laitetuki pyydä laitteen valmistajalta.



| **Toimittajan**                      | **Laitteen tuoteperhe**                                        | **Pienin käyttöjärjestelmäversio**                             | **PolicyBased**                                                                                                                                                                                                             | **RouteBased**                                                                                                                                                                    |
|---------------------------------|----------------------------------------------------------|----------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Jätetuotteet Telesis                  | AR sarjan VPN-yhteyttä reitittimen                                    | 2.9.2                                              | Tulossa pian                                                                                                                                                                                                                                          | Ei ole yhteensopiva                                                                                                                                                                                               |
| Barracuda verkkojen Inc.        | Barracuda NextGen palomuurin F-sarjan             | PolicyBased: 5.4.3 RouteBased: 6.2.0  | [Määritysohjeet](https://techlib.barracuda.com/NGF/AzurePolicyBasedVPNGW) | [Määritysohjeet](https://techlib.barracuda.com/NGF/AzureRouteBasedVPNGW)                                                                                                                                                                                              |
| Barracuda verkkojen Inc.        |  Barracuda NextGen palomuurin X-sarjan                 | Barracuda palomuuri 6.5 | [Barracuda palomuuri](https://techlib.barracuda.com/BFW/ConfigAzureVPNGateway) | Ei ole yhteensopiva                                                                                                                                                                                               |
| Brokadi                         | Vyatta 5400 vRouter                                      | Virtuaalinen reitittimen 6.6R3 GA                            | [Määritysohjeet](http://www1.brocade.com/downloads/documents/html_product_manuals/vyatta/vyatta_5400_manual/wwhelp/wwhimpl/js/html/wwhelp.htm#href=VPN_Site-to-Site%20IPsec%20VPN/Preface.1.1.html)                                       | Ei ole yhteensopiva                                                                                                                                                                                               |
| Tarkista piste                     | Suojattu yhdyskäytävä                                         | R75.40, R75.40VS                                     | [Määritysohjeet](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk101275)                                         | [Määritysohjeet](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk101275) |
| Cisco                           | ASA                                                      | 8.3                                                | [Cisco-objektit](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASA)                                                                                                                                                                        | Ei ole yhteensopiva                                                                                                                                                                                               |
| Cisco                           | ASR                                                      | IOS 15.1 (PolicyBased) IOS 15.2 (RouteBased)                | [Cisco-objektit](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASR)                                                                                                                                                                        | [Cisco-objektit](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASR)                                                                                                                                 |
| Cisco                           | ISR                                                      | IOS 15.0 (PolicyBased) IOS 15.1 (RouteBased *)               | [Cisco-objektit](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ISR)                                                                                                                                                                        | [Cisco näytteiden *](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ISR)                                                                                                                                |
| Citrix                          | NetScaler MPX-SDX, VPX      |10.1 ja yllä                                           | [Integrointi ohjeita](https://docs.citrix.com/en-us/netscaler/11-1/system/cloudbridge-connector-introduction/cloudbridge-connector-azure.html)                                                                                                                                                                            | Ei ole yhteensopiva                                                                                                                                                                                               |
| Dell SonicWALL                  | TZ sarjan NSA sarjan SuperMassive sarjan, E-luokan NSA sarja | SonicOS 5.8.x, [SonicOS 5.9.x](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide/supported-platforms?ParentProduct=850), [SonicOS 6.x](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide/supported-platforms?ParentProduct=646 )          | [Ohjeet - SonicOS 6.2](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide?ParentProduct=646) [Ohjeet - SonicOS 5,9](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide?ParentProduct=850)                                                                                                                                   | [Ohjeet - SonicOS 6.2](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide?ParentProduct=646) [Ohjeet - SonicOS 5,9](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide?ParentProduct=850)                                                                                                                                                                                      |
| F5                              | ISO-IP-sarja                                 |           12.0                                            | [Määritysohjeet](https://devcentral.f5.com/articles/connecting-to-windows-azure-with-the-big-ip)                                                                                                                                                                          | [Määritysohjeet](https://devcentral.f5.com/articles/big-ip-to-azure-dynamic-ipsec-tunneling)                                                                                                                                                                                         |
| Fortinet                        | Virustorjuntapalomuurien                                                | FortiOS 5.2.7                                      | [Määritysohjeet](http://docs.fortinet.com/d/fortigate-configuring-ipsec-vpn-between-a-fortigate-and-microsoft-azure)                                                                                                                                                                          | [Määritysohjeet](http://docs.fortinet.com/d/fortigate-configuring-ipsec-vpn-between-a-fortigate-and-microsoft-azure)                                                                                                                                  |
| Internet-aloitteestaan japani (IIJ) | SEIL sarja                                              | SEIL / 4.60, SEIL/B1 4.60 SEIL/x86 3.20 X            | [Määritysohjeet](http://www.iij.ad.jp/biz/seil/ConfigAzureSEILVPN.pdf)                                                                                                                                                                   | Ei ole yhteensopiva                                                                                                                                                                                               |
| Juniper                         | SRX                                                      | JunOS 10.2 (PolicyBased) JunOS 11,4 (RouteBased)            | [Juniper mallit](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SRX)                                                                                                                                                                      | [Juniper mallit](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SRX)                                                                                                                              |
| Juniper                         | J-sarjan                                                 | JunOS 10.4r9 (PolicyBased) JunOS 11,4 (RouteBased)          | [Juniper mallit](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/JSeries)                                                                                                                                                                 | [Juniper mallit](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/JSeries)                                                                                                                         |
| Juniper                         | ISG                                                      | ScreenOS 6.3 (PolicyBased ja RouteBased)                  | [Juniper mallit](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/ISG)                                                                                                                                                                      | [Juniper mallit](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/ISG)                                                                                                                              |
| Juniper                         | SSG                                                      | ScreenOS 6.2 (PolicyBased ja RouteBased)                  | [Juniper mallit](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SSG)                                                                                                                                                                      | [Juniper mallit](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SSG)                                                                                                                              |
| Microsoft                       | Reititys ja etäkäyttö Access-palvelu                        | Windows Server 2012                                | Ei ole yhteensopiva                                                                                                                                                                                                                                       | [Microsoft-objektit](http://go.microsoft.com/fwlink/p/?LinkId=717761)                                                                                           |
| Avaa järjestelmien AG | Tehtävä ohjausobjektin suojattu yhdyskäytävä | PUUTTUU | [Oppaaseen](https://www.open.ch/_pdf/Azure/AzureVPNSetup_Installation_Guide.pdf) | [Oppaaseen](https://www.open.ch/_pdf/Azure/AzureVPNSetup_Installation_Guide.pdf) |
| Openswan                        | Openswan                                                 | 2.6.32                                             | (Tulossa)                                                                                                                                                                                                                                        | Ei ole yhteensopiva                                                                                                                                                                                               |
| Palo Alto Networks              | Kaikki laitteet, joissa PANOROINTI OS     | PANOROINTI-OS 6.1.5 tai uudempi (PolicyBased), PANOROINTI-OS 7.0.5 tai uudempi (RouteBased)       | [Määritysohjeet]( https://live.paloaltonetworks.com/t5/Configuration-Articles/How-to-Configure-VPN-Tunnel-Between-a-Palo-Alto-Networks/ta-p/59065)                                                                                                                                                                                          | [Määritysohjeet](https://live.paloaltonetworks.com/t5/Integration-Articles/Configuring-IKEv2-VPN-for-Microsoft-Azure-Environment/ta-p/60340)                                                                                                                                                                                    |
| Watchguard                      | Kaikki                                                      | Fireware XTM v11.x                                 | [Määritysohjeet](http://customers.watchguard.com/articles/Article/Configure-a-VPN-connection-to-a-Windows-Azure-virtual-network/)                                                                                                                                                                          | Ei ole yhteensopiva                                                                                                                                                                                               |

(*) ISR 7200 sarjan reitittimen tukevat vain PolicyBased VPN-yhteydet.

## <a name="additionaldevices"></a>Non vahvistetaan VPN-laitteet

Jos et näe vahvistettu VPN laitteiden taulukossa mainittuja laitteen, se saattaa toimivat edelleen sivusto yhteys. Tarkista, että VPN-laite täyttää [VPN yhdyskäytävät](vpn-gateway-about-vpngateways.md#gateway-requirements) -artikkelin yhdyskäytävän vaatimukset-osassa kuvatut vähimmäisvaatimukset. Vähimmäisvaatimukset täyttävät laitteiden pitäisi toimia myös hyvin VPN yhdyskäytävien kanssa. Pyydä laitteen valmistajalta tuki- ja lisäohjeita.


## <a name="editing-device-configuration-samples"></a>Laitteen määrittäminen näytteiden muokkaaminen

Kun olet ladannut annettu VPN laitteen määritysten otosten, sinun on korvata joitakin arvot vastaamaan-ympäristön asetuksia. 

**Voit muokata otoksen seuraavasti:**

1. Avaa malli, Muistiossa. 
1. Etsi ja korvaa kaikki <*teksti*> merkkijonot arvot, jotka koskevat-ympäristöön. Muista lisätä < ja >. Kun nimi on määritetty, voit valita nimen on oltava yksilöivä otsikko. Jos komento ei toimi, katso laitteen valmistajan käyttöohjeista.

| **Esimerkkiteksti**                | **Jos haluat muuttaa**                                                                                                        |
|----------------------------------|----------------------------------------------------------------------------------------------------------------------|
| &lt;RP_OnPremisesNetwork&gt;           | Objektin nimi, että valittu. Esimerkki: myOnPremisesNetwork                                                       |
| &lt;RP_AzureNetwork&gt;                | Objektin nimi, että valittu. Esimerkki: myAzureNetwork                                                            |
| &lt;RP_AccessList&gt;                  | Objektin nimi, että valittu. Esimerkki: myAzureAccessList                                                         |
| &lt;RP_IPSecTransformSet&gt;           | Objektin nimi, että valittu. Esimerkki: myIPSecTransformSet                                                       |
| &lt;RP_IPSecCryptoMap&gt;              | Objektin nimi, että valittu. Esimerkki: myIPSecCryptoMap                                                          |
| &lt;SP_AzureNetworkIpRange&gt;         | Määritä alue. Esimerkki: 192.168.0.0                                                                                  |
| &lt;SP_AzureNetworkSubnetMask&gt;      | Määritä aliverkkopeite. Esimerkki: 255.255.0.0                                                                            |
| &lt;SP_OnPremisesNetworkIpRange&gt;    | Määritä paikallisen alue. Esimerkki: 10.2.1.0                                                                         |
| &lt;SP_OnPremisesNetworkSubnetMask&gt; | Määrittää paikallisen aliverkkopeite. Esimerkki: 255.255.255.0                                                              |
| &lt;SP_AzureGatewayIpAddress&gt;       | Nämä tiedot tietyn virtual verkkoon, ja se sijaitsee hallinta-portaalin **Yhdyskäytävän IP**-osoite. |
| &lt;SP_PresharedKey&gt;                | Nämä tiedot on virtual verkossa, ja se sijaitsee hallinta-portaalin hallinta avaimeksi.          |



## <a name="ipsec-parameters"></a>IP-parametrit

>[AZURE.NOTE] Seuraavassa taulukossa lueteltuja arvot tukemat Azure VPN-yhdyskäytävän, vaikka tällä hetkellä tällä ei voi määrittää tai valita tiettyjä yhdistelmä Azure VPN-yhdyskäytävän. Sinun on määritettävä mitään rajoituksia paikallisen VPN-laitteesta. Lisäksi on puristin MSS 1350-palvelussa.

### <a name="ike-phase-1-setup"></a>IKE vaihe 1: n asennusohjelma

| **Ominaisuus**                                       | **PolicyBased** | **RouteBased ja standardiksi tai suuri suorituskyvyn VPN-yhdyskäytävän** |
|----------------------------------------------------|--------------------------------|------------------------------------------------------------------|
| IKE versio                                        | IKEv1                          | IKEv2                                                            |
| Diffie Hellman-ryhmä                               | Ryhmä 2 (1 024-bittinen)             | Ryhmä 2 (1 024-bittinen)                                               |
| Todentamismenetelmä                              | Ennalta jaettu avain                 | Ennalta jaettu avain                                                   |
| Salauksen algoritmit                              | AES256 AES128 3DES             | AES256 3DES                                                      |
| Hajautuksessa algoritmin                                  | SHA1(SHA128)                   | SHA1(SHA128), SHA2(SHA256)                                                     |
| Vaihe 1 Security Association (SA) elinaika (aika) | 28,800 sekuntia                 | 10,800 sekuntia                                                   |


### <a name="ike-phase-2-setup"></a>IKE vaiheen 2 asetukset

| **Ominaisuus**                                                             | **PolicyBased**                 | **RouteBased ja standardiksi tai suuri suorituskyvyn VPN-yhdyskäytävän**   |
|--------------------------------------------------------------------------|------------------------------------------------|--------------------------------------------------------------------|
| IKE versio                                                              | IKEv1                                          | IKEv2                                                              |
| Hajautuksessa algoritmin                                                        | SHA1(SHA128)                                   | SHA1(SHA128)                                                       |
| Vaihe 2 Security Association (SA) elinaika (aika)                        | 3 600 sekuntia                                  | 3 600 sekuntia                                                                  |
| Vaihe 2 Security Association (SA) elinaika (nopeus)                  | 102,400,000 KT                                 | -                                                                  |
| IP-SA-salaus ja todennus on (järjestyksessä preference) | 1. ESP-AES256 2. ESP-AES128 3. ESP-3DES 4. PUUTTUU | Katso *RouteBased yhdyskäytävän IP Security Association (SA) on* (alla) |
| Sopivaa eteenpäin salaisuus (PFS)                                            | Ei                                             | Ei (*)|
| Kuolleiden Peer tunnistus                                                      | Ei tueta                                  | Tuettu                                                          |

(*) Azure yhdyskäytävän IKE vastaajan hyväksyä PFS DH ryhmän 1, 2, 5, 14, 24.

### <a name="routebased-gateway-ipsec-security-association-sa-offers"></a>RouteBased yhdyskäytävän IP Security Association (SA) on

Seuraavassa taulukossa on lueteltu IP SA salaus ja todennus on. Tarjoukset näkyvät preference järjestystä tarjous on esitetty tai hyväksytty.

| **IP-SA-salaus ja todennus tarjouksia** | **Azure yhdyskäytävään, mikäli käynnistäjä**                               | **Vastaajan Azure yhdyskäytävän**                                   |
|---------------------------------------------------|--------------------------------------------------------------|--------------------------------------------------------------|
| 1                                                 | ESP AES_256 SHA                                              | ESP AES_128 SHA                                              |
| 2                                                 | ESP AES_128 SHA                                              | ESP 3_DES MD5                                                |
| 3                                                 | ESP 3_DES MD5                                                | ESP 3_DES SHA                                                |
| 4                                                 | ESP 3_DES SHA                                                | AH SHA1 ESP AES_128 kanssa null HMAC kanssa                      |
| 5                                                 | AH SHA1 ESP AES_256 kanssa null HMAC kanssa                      | AH SHA1 kanssa ESP 3_DES null HMAC kanssa                        |
| 6                                                 | AH SHA1 ESP AES_128 kanssa null HMAC kanssa                      | AH MD5 kanssa ESP 3_DES null HMAC, ei ole käyttöajan ehdotettu kanssa |
| 7                                                 | AH SHA1 kanssa ESP 3_DES null HMAC kanssa                        | AH SHA1 ESP 3_DES SHA1 ei käyttöajan kanssa                    |
| 8                                                 | AH MD5 kanssa ESP 3_DES null HMAC, ei ole käyttöajan ehdotettu kanssa | AH MD5 ESP 3_DES MD5 ei käyttöajan kanssa                     |
| 9                                                 | AH SHA1 ESP 3_DES SHA1 ei käyttöajan kanssa                    | ESP DES MD5                                                  |
| 10                                                | AH MD5 ESP 3_DES MD5 ei käyttöajan kanssa                     | ESP DES SHA1 ei käyttöajan                                   |
| 11                                                | ESP DES MD5                                                  | ETTÄ SHA1 ESP DES null HMAC, jossa ei ole käyttöajan ehdotettu        |
| 12                                                | ESP DES SHA1 ei käyttöajan                                   | ETTÄ MD5 ESP DES null HMAC, jossa ei ole käyttöajan ehdotettu        |
| 13                                                | ETTÄ SHA1 ESP DES null HMAC, jossa ei ole käyttöajan ehdotettu        | AH SHA1 ESP DES SHA1 ei käyttöajan kanssa                      |
| 14                                                | ETTÄ MD5 ESP DES null HMAC, jossa ei ole käyttöajan ehdotettu        | AH MD5 ESP DES MD5 ei käyttöajan kanssa                       |
| 15                                                | AH SHA1 ESP DES SHA1 ei käyttöajan kanssa                      | ESP SHA ei käyttöajan                                        |
| 16                                                | AH MD5 ESP DES MD5 ei käyttöajan kanssa                       | ESP MD5, ei ole käyttöajan                                        |
| 17                                                | -                                                            | ETTÄ SHA, ei ole käyttöajan                                         |
| 18                                                | -                                                            | AH MD5, ei ole käyttöajan                                        |


- Voit määrittää RouteBased ja suuri suorituskyvyn VPN yhdyskäytävien IP ESP NULL salausta. Null perustuvat salaus ei tarjoa suojauksen salataanko siirrettävät tiedot ja käytetään vain, kun suurin nopeus ja pienin viive on pakollinen.  Asiakkaat voivat valita tällä VNet VNet viestintä tilanteissa tai kun salausta käytetään muualla ratkaisun.

- Käytä Azure VPN-yhdyskäytävän oletusasetukset paikallisen-yhteys Internetin kautta, salaus ja hajautusfunktiota algoritmit lueteltu varmistaa kriittinen viestintä taulukoissa.






### <a name="is-bgp-supported-on-all-azure-vpn-gateway-skus"></a>Tuetaan erityisen kaikissa Azure VPN-yhdyskäytävä-tuotteissa?

Ei, erityisen tuetaan Azure **Vakio** - ja **korkean** VPN yhdyskäytävät. **Perustoiminnot** TUOTE ei tueta.

### <a name="can-i-use-bgp-with-azure-policy-based-vpn-gateways"></a>Voit käyttää erityisen Azure Policy-Based VPN yhdyskäytävien?

Ei, erityisen tuetaan vain reitti-pohjainen VPN-yhdyskäytävät.

### <a name="can-i-use-private-asns-autonomous-system-numbers"></a>Yksityinen lähetysilmoitukset (yksipuolisia järjestelmän numerot) käyttäminen

Kyllä, voit käyttää omia lähetysilmoitukset julkinen tai yksityinen lähetysilmoitukset paikallisen verkkojen ja Azure virtual verkot.

#### <a name="are-there-asns-reserved-by-azure"></a>Onko varattu Azure lähetysilmoitukset?

Kyllä, seuraavat lähetysilmoitukset itsellään Azure sekä sisäisten ja ulkoisten peerings varten:

- Julkinen lähetysilmoitukset: 8075 8076, 12076
- Yksityinen lähetysilmoitukset: 65515 65517 65518, 65519, 65520

Nämä lähetysilmoitukset ei voi määrittää käytössä paikallisen VPN-laitteet, Azure VPN yhdyskäytävien yhdistettäessä.

### <a name="can-i-use-the-same-asn-for-both-on-premises-vpn-networks-and-azure-vnets"></a>Saman ASN käyttää sekä paikallisen VPN-verkkojen ja Azure VNets

Ei, sinun on liitettävä eri lähetysilmoitukset paikallisen lopettaminen ja Azure VNets Jos olet muodostamassa erityisen kanssa. Azure VPN-yhdyskäytävät on oletusarvoisesti määritetty, 65515 ASN, onko erityisen on käytössä ei paikallisen-yhteys. Voit ohittaa tämän oletuksena määrittämällä eri ASN VPN-yhdyskäytävän luotaessa tai muuttaa ASN yhdyskäytävän luomisen jälkeen. Tarvitset paikallisen lähetysilmoitukset liittäminen vastaavan Azure paikalliseen verkkoon yhdyskäytävät.

### <a name="what-address-prefixes-will-azure-vpn-gateways-advertise-to-me"></a>Mitä osoitteiden etuliitteiden Azure VPN yhdyskäytävien ilmoittaa minulle?

Azure VPN-yhdyskäytävän ilmoittaa seuraavat tiet paikallisen erityisen-laitteet:

- VNet-osoitteiden etuliitteiden
- Osoitteiden etuliitteiden varten kunkin paikallisen verkon yhdyskäytävien yhteydessä Azure VPN-yhdyskäytävän
- Tiet opitut asiat muiden erityisen peering istuntojen yhteydessä Azure-VPN-yhdyskäytävän **oletusarvon reitin tai minkä tahansa VNet etuliite päällekkäinen tiet lukuun ottamatta**.

#### <a name="can-i-advertise-default-route-00000-to-azure-vpn-gateways"></a>Azure VPN yhdyskäytävien reitin oletusarvo (0.0.0.0/0) mainostaa

Ei tällä hetkellä.

#### <a name="can-i-advertise-the-exact-prefixes-as-my-virtual-network-prefixes"></a>Tarkka etuliitteiden mainostaa VPN-etuliitteitä nimellä

Ei, mainonta saman etuliitteiden kuin jokin Virtual verkon osoitteiden etuliitteiden on estetty tai suodattaa Azure-ympäristössä. Voit kuitenkin mainostaa etuliitteen, joka on osa on näennäinen verkoston sisällä. Esimerkiksi Virtual verkon voi käyttää osoite tilaa 10.10.0.0/16 ja voi mainostaa 10.0.0.0/8.

### <a name="can-i-use-bgp-with-my-vnet-to-vnet-connections"></a>Voinko käyttää erityisen VNet VNet-yhteyksiä?

Kyllä, voit käyttää erityisen paikallisen-yhteydet ja VNet VNet yhteydet.

### <a name="can-i-mix-bgp-with-non-bgp-connections-for-my-azure-vpn-gateways"></a>Voit voin sekoittaa erityisen erityisen yhteyksiä varten Azure VPN-yhdyskäytävät?

Kyllä, voit käyttää samaa Azure VPN-yhdyskäytävän erityisen ja erityisen yhteydet.

### <a name="does-azure-vpn-gateway-support-bgp-transit-routing"></a>Onko Azure VPN yhdyskäytävän tukea erityisen salataanko siirrettävät reititys?

Kyllä, erityisen salataanko siirrettävät reititys tuetaan, Azure VPN yhdyskäytävien **ei** mainostaa oletusarvon tiet, muut erityisen kollegat lukuun ottamatta. Jotta salataanko siirrettävät reitittämisen useita Azure VPN-yhdyskäytäviä, sinun on otettava erityisen kaikki keskitason VNet VNet yhteydet.

### <a name="can-i-have-more-than-one-tunnels-between-azure-vpn-gateway-and-my-on-premises-network"></a>Voi olla useita tunneleita Azure VPN-yhdyskäytävän ja paikallisen verkon välillä?

Kyllä, voit luoda useita S2S VPN tunneleita Azure VPN-yhdyskäytävän ja paikallisen verkon välillä. Huomaa, että nämä tunneleita lasketaan vastaan Azure VPN-yhdyskäytävät tunneleita kokonaismäärän. Jos sinulla on kaksi tarpeettomat tunneleita Azure VPN-yhdyskäytävän ja jokin paikallisen verkon välillä, ne käyttää esimerkiksi 2 tunneleita Azure-VPN-yhdyskäytävän, (10 standardin) ja korkean 30 yhteensä kiintiön ulos.

### <a name="can-i-have-multiple-tunnels-between-two-azure-vnets-with-bgp"></a>Voi olla useita tunneleita välillä kaksi Azure VNets erityisen kanssa?

Ei, tarpeettomat tunneleita virtual verkkojen kahdet välillä ei tueta.

### <a name="can-i-use-bgp-for-s2s-vpn-in-an-expressroutes2s-vpn-co-existence-configuration"></a>Voit käyttää erityisen S2S VPN-ExpressRoute/S2S VPN-rinnakkainen määritykset?

Ei tällä hetkellä.

### <a name="what-address-does-azure-vpn-gateway-use-for-bgp-peer-ip"></a>Mitä osoitetta Azure VPN-yhdyskäytävän käyttää erityisen Peer IP-osoite?

Azure VPN-yhdyskäytävän varata yksittäinen IP-osoite määritetty näennäinen verkon GatewaySubnet alueelta. Oletusarvon mukaan on alueen toinen viimeisen osoitetta. Esimerkiksi jos että GatewaySubnet on 10.12.255.0/27, väliltä 10.12.255.0 10.12.255.31, valitse Azure VPN-yhdyskäytävän erityisen Peer IP-osoite on 10.12.255.30. Löydät nämä tiedot, kun teet luettelon Azure VPN-yhdyskäytävän tiedot.

### <a name="what-are-the-requirements-for-the-bgp-peer-ip-addresses-on-my-vpn-device"></a>Mitkä ovat erityisen Peer IP-osoitteiden VPN laitteessa vaatimukset?

Paikallisen erityisen Vertaisjärjestelmän IP-osoite **Ei saa** olla sama kuin VPN-laitteesi julkiseen IP-osoite. Käytä eri IP-osoite VPN-laitteeseen erityisen Peer IP-osoite. Voi olla osoitteelle silmukka-liittymän laitteeseen. Määritä tätä osoitetta vastaavan paikallisen yhdyskäytävien sijaintia kuvaava.

### <a name="what-should-i-specify-as-my-address-prefixes-for-the-local-network-gateway-when-i-use-bgp"></a>Mitä voin on määritettävä osoite-etuliitteitä kuin paikalliseen verkkoon yhdyskäytävän erityisen käyttää?

Azure paikallisen yhdyskäytävien määrittää ensimmäisen osoite etuliitteitä paikallista verkkoa varten. Erityisen, jossa on jaettava host etuliite (/ 32 etuliite) on erityisen Peer IP-osoite osoite-tilaa paikallista verkkoa varten. Jos erityisen Peer IP 10.52.255.254, Määritä "10.52.255.254/32" kuin paikalliseen verkkoon yhdyskäytävän edustava paikallisen verkon localNetworkAddressSpace. Tämä on varmistettava Azure VPN-yhdyskäytävän muodostaa erityisen istunnon S2S VPN tunnelissa.

### <a name="what-should-i-add-to-my-on-premises-vpn-device-for-the-bgp-peering-session"></a>Mitä kannattaa lisätä paikallisen VPN laitteeseeni erityisen peering istunnon aikana?

Laitteen VPN-IP-S2S VPN-tunnelin valitsemalla Lisää isännän reitti Azure erityisen Peer IP-osoite. Jos Azure VPN Peer IP on "10.12.255.30", sinun olisi lisätään "10.12.255.30" nexthop-liittymällä, vastaava IP-tunneliliittymän, host reitti VPN-laitteessasi.

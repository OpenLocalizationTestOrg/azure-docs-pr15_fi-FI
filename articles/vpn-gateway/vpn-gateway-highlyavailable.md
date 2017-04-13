<properties
   pageTitle="Yleiskatsaus käytettävissä määritykset ja Azure VPN yhdyskäytävien | Microsoft Azure"
   description="Tässä artikkelissa on yleiskatsaus käytettävissä käyttämällä Azure VPN yhdyskäytävien asetukset."
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor=""
   tags=""/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/24/2016"
   ms.author="yushwang"/>

# <a name="highly-available-cross-premises-and-vnet-to-vnet-connectivity"></a>Käytettävissä olevien paikallisten ja VNet VNet yhteydet

Tässä artikkelissa on yleiskatsaus käytettävissä kokoonpanoasetusten määrittäminen paikallisen-ja VNet VNet connectivity Azure VPN yhdyskäytävien avulla.

## <a name = "activestandby"></a>Lisätietoja Azure VPN yhdyskäytävän redundancy

Jokaisen Azure VPN-yhdyskäytävän kuuluu kaksi esiintymää aktiivinen valmius-kokoonpanossa. Suunniteltu ylläpito-ja suunnittelematon häiriöt, tapahtuu aktiivinen esiintymään valmius esiintymän menee (automaattisesti) automaattisesti ja jatka S2S VPN- tai VNet VNet yhteydet. Vaihda päälle aiheuttavat lyhyt keskeytymisen. Suunniteltu ylläpito yhteydet palautetaan 10 – 15 sekunnin ajan kuluessa. Odottamattomia ongelmia yhteyden palautus on myöhemmäksi, noin minuutin Huonoin tapaus-1 ja puoli minuuttia. Yhdyskäytävän P2S VPN-asiakasohjelman yhteyksien P2S yhteydet katkaistaan ja käyttäjien on peräisin asiakaskoneiden uudelleen.

![Aktiivinen valmius](./media/vpn-gateway-highlyavailable/active-standby.png)

## <a name="highly-available-cross-premises-connectivity"></a>Käytettävissä olevien paikallisten yhteys

Antamaan parempi käytettävyys cross paikallisen yhteyksien muutama vaihtoehtojen käytettävissä:

- Useita paikallisen VPN-laitteet
- Aktiivinen aktiivinen Azure VPN-yhdyskäytävän
- Yhdistelmä

### <a name = "activeactiveonprem"></a>Useita paikallisen VPN-laitteet

Voit tehdä useita VPN-laitteita paikallisen verkosta Azure VPN-yhdyskäytävää, yhdistäminen seuraavassa kaaviossa esitetyllä tavalla:

![Useita paikallisen VPN](./media/vpn-gateway-highlyavailable/multiple-onprem-vpns.png)

Tämä määritys tarjoaa useita aktiivisten tunnelien Yhdyskäytävästä saman Azure VPN paikallisen laitteet samassa sijainnissa. On joitakin vaatimukset ja julkaisun rajoitukset:

1. Sinun täytyy luoda useita S2S VPN-yhteydet VPN-laitteilla Azure. Kun yhdistät useiden VPN-laitteiden samaan paikalliseen verkkoon Azure, sinun täytyy yhden paikalliseen verkkoon kirjauduttaessa yhdyskäytävän VPN-pienoisohjelman ja yksi yhteys luodaan Azure VPN-yhdyskäytävän paikalliseen verkkoon kirjauduttaessa Gateway.

2. Vastaavat VPN-laitteiden paikalliseen verkkoon kirjauduttaessa yhdyskäytävät on oltava yksilöllinen julkisten IP-osoitteiden "GatewayIpAddress"-ominaisuutta.

3. Erityisen vaaditaan määritysten. Kunkin paikalliseen verkkoon kirjauduttaessa yhdyskäytävän edustava VPN-laite on oltava yksilöllinen erityisen peer IP-osoite "BgpPeerIpAddress"-ominaisuudessa määritetyn.

4. Kunkin paikallisen yhdyskäytävien AddressPrefix ominaisuuden-kentässä on päällekkäin. Kirjoita /32 "BgpPeerIpAddress" AddressPrefix-kenttä, esimerkiksi 10.200.200.254/32 CIDR-muodossa.

5. Käytettävä erityisen ilmoittaa saman etuliitteiden samaa paikallisen verkon etuliitteiden Azure VPN-yhdyskäytävään, ja liikenne lähetetään edelleen nämä tunnelissa samanaikaisesti.

6. Jokaista lasketaan vastaan Azure VPN-yhdyskäytävän 10 Basic- ja Standard tuotteissa tunneleita enimmäismäärä ja korkean SKU 30. 

Tässä määrityksessä Azure VPN-yhdyskäytävän on edelleen aktiivinen valmius-tilassa niin saman automaattisesti toiminta ja lyhyt keskeyttäminen tapahtuu edelleen kuin kuvattu [yllä](#activestandby). Mutta näiden määritysten suojusten virheet tai keskeytyksiä paikalliseen verkkoon ja VPN-laitteisiin.
 
### <a name="active-active-azure-vpn-gateway"></a>Aktiivinen aktiivinen Azure VPN-yhdyskäytävän

Voit nyt luoda Azure VPN-yhdyskäytävän aktiivinen aktiivinen-kokoonpanossa, jossa molemmat esiintymät yhdyskäytävän VMs yhteisten S2S VPN tunneleita paikallisen VPN-laitteeseen, kuten seuraavassa kaaviossa on esitetty:

![Aktiivinen aktiivinen](./media/vpn-gateway-highlyavailable/active-active.png)

Tässä määrityksessä Azure yhdyskäytävän jokaiselle esiintymälle on yksilöllinen julkiseen IP-osoite ja kunkin muodostavat perheelle IP/IKE S2S VPN-tunnelin paikallisen yhdyskäytävien ja yhteyden paikallisen VPN-laitteeseen. Huomaa, että molemmat VPN-tunneleita ovat todella osa samaa yhteyttä. Sinun on edelleen määrittää paikallisen VPN-laitteeseen, jotta voit hyväksyä tai muodosta kaksi S2S VPN-tunneleita nämä kaksi Azure VPN yhdyskäytävän julkiseen IP-osoitteisiin.

Koska Azure yhdyskäytäväesiintymästä koostuva aktiivinen aktiivinen kokoonpanossa, Azure virtual verkosta liikenne paikalliseen verkkoon reititetään sekä tunnelissa samanaikaisesti, vaikka paikallisen VPN-laite saattaa Web-sivuston yhteensopivaksi yhden tunnelin sijaan. Huomautus vaikka saman TCP tai UDP työnkulku aina siirtää samaan tunnelin tai polku, ellei ylläpitotapahtuman tapahtuu jokin esiintymät.

Kun suunniteltu ylläpito tai suunnittelematon tapahtuman tapahtuu yhden yhdyskäytävän esiintymään, suojaustunnelin kyseisen esiintymästä paikallisen VPN-laite katkaistaan. Vastaavat tiet VPN-laitteissa kannattaa poistaa tai peruuttaa automaattisesti niin, että liikenne voidaan vaihtaa muut aktiivisen suojaustunnelin. Azure-puolen päälle Vaihda tapahtuu automaattisesti tarvittavien esiintymästä aktiivinen esiintymä.

### <a name="dual-redundancy-active-active-vpn-gateways-for-both-azure-and-on-premises-networks"></a>Kahden-redundancy: aktiivinen-aktiivinen VPN yhdyskäytävien Azuren ja paikallisten-verkoissa

Mahdollisimman Luotettavat vaihtoehto on aktiivinen aktiivinen yhdyskäytävien verkon ja Azure-yhdistäminen seuraavassa kuvassa esitetyllä tavalla.

![Kahden Redundancy](./media/vpn-gateway-highlyavailable/dual-redundancy.png)

Tähän luominen ja Azure VPN yhdyskäytävän aktiivinen aktiivinen-kokoonpanon määritys ja luo kaksi paikalliseen verkkoon kirjauduttaessa yhdyskäytävien ja kaksi yhteyksien lisääminen kahden paikallisten VPN-laitteiden yllä olevien ohjeiden mukaisesti. Tulos on koko verkko-yhteyksien 4 IP-tunneleita Azure virtual verkon ja paikallisen verkon välille.

Kaikkien yhdyskäytävien ja tunnelit ovat aktiivisia Azure reunasta, jotta liikenne jakamisen kesken kaikki 4 tunneleita samanaikaisesti, vaikka kunkin TCP tai UDP-työnkulku uudelleen seuraa saman tunnelin tai polku Azure reunasta. Vaikka levittämällä liikenne voit nähdä hieman parempi siirtonopeuden kautta IP-tunneleita, määritysten ensisijainen tavoite on suuri käytettävyys. Ja hajauttamisen tilastollinen laatu vuoksi on vaikea säätää, miten eri sovelluksen liikenne ehtojen mitta vaikuttaa kooste siirtonopeuden.

Tämä topologian edellyttää kahta paikalliseen verkkoon kirjauduttaessa yhdyskäytävää ja kaksi yhteyttä tukemaan paikallisen VPN-laitteiden kahdet ja erityisen vaaditaan, jotta kaksi yhteyttä samaan paikalliseen verkkoon. Nämä vaatimukset ovat samat kuin [yläpuolella](#activeactiveonprem). 

## <a name="highly-available-vnet-to-vnet-connectivity-through-azure-vpn-gateways"></a>Erittäin käytettävissä VNet VNet Connectivity Azure VPN yhdyskäytävien kautta

Aktiivinen aktiivinen määrityksiä voi käyttää myös Azure VNet VNet yhteydet. Voit luoda aktiivinen aktiivinen VPN yhdyskäytävien sekä virtual verkkojen ja yhdistä ne yhteen 4 tunneleita kahden VNets välillä saman koko verkko-yhteys lomakkeen seuraavassa kuvassa esitetyllä tavalla:

![VNet VNet](./media/vpn-gateway-highlyavailable/vnet-to-vnet.png)

Tällä varmistetaan, että ovat aina kahdet tunneleita suunnitellun ylläpidon tapahtumat, tarjoavat näkyisivät paremmin käytettävyys kaksi virtual verkkojen välillä. Vaikka saman topologian varten rajat paikallisesta connectivity edellyttää kahta yhteydet-VNet VNet topologian, kuten alla on vain yksi yhteys kunkin Gatewayn. Lisäksi erityisen on valinnainen, ellei salataanko siirrettävät reititys VNet VNet-yhteyden kautta on tarvittavat.


## <a name="next-steps"></a>Seuraavat vaiheet

Katso [Määrittäminen aktiivinen aktiivinen VPN yhdyskäytävien paikallisen - ja VNet VNet yhteydet](vpn-gateway-activeactive-rm-powershell.md) ohjeet Määritä aktiivinen aktiivinen rajat paikallisen ja VNet VNet yhteydet.

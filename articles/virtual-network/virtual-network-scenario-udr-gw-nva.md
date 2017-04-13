<properties 
   pageTitle="Hybrid yhteys, jonka tason 2-sovellus | Microsoft Azure"
   description="Lue, miten voit ottaa käyttöön virtual laitteiden ja UDR monitasoisten sovelluksen ympäristön luominen Azure"
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
   ms.date="05/05/2016"
   ms.author="jdial" />

# <a name="virtual-appliance-scenario"></a>Virtuaalinen laitteen-skenaario

Käytetty vaihtoehto suurempi Azure asiakkaan välillä on tarjottava kaksitasoista sovelluksen tarjoamia yhteyden Internetiin, samalla access takaisin taso-paikallisen-palvelinkeskukseen. Tämän asiakirjan opastaa käytettäessä käyttäjän määrittämät tiet (UDR), VPN-yhdyskäytävän ja verkon virtual laitteiden ottamaan kaksitasoinen-ympäristössä, joka täyttää seuraavat vaatimukset:

- Web-sovelluksen on oltava käytettävissä vain julkisesta Internetistä.
- Isäntäpalvelimen sovelluksen on oltava voivat käyttää Taustajärjestelmä sovelluspalvelin.
- Kaikki liikenne Internet-verkkosovellus on käymällä läpi palomuurin virtual laitteen. Tämä virtual laitteen käytetään Internet-liikenne.
- Kaikki sovelluspalvelin liikenne on käymällä läpi palomuurin virtual laitteen. Tämä virtual laitteen käytetään käyttöön Lopeta palvelimeen ja peräisin paikallisen verkon kautta VPN-yhdyskäytävän access.
- Järjestelmänvalvojat voivat hallita palomuurin virtual laitteiden paikallisen tietokoneisiin, käyttämällä kolmannen palomuuri virtual laitteen käytetään ainoastaan hallintatoimintoihin.

Tämä on vakio DMZ-skenaario DMZ ja suojatun verkon. Näiden skenaario on rakennettava Azure käyttämällä NSGs, palomuurin virtual laitteiden tai molempia. Alla olevassa taulukossa näkyy osa ja haitat NSGs ja palomuurin virtual laitteiden välillä.

||Asiantuntijoille|Kuvakkeet|
|---|---|---|
|NSG|Ei ole kustannukset. <br/>Integroitu Azure RBAC. <br/>ARM-malleihin voi luoda sääntöjä.|Suurempi ympäristöissä voi vaihdella monimutkaisuutta. |
|Palomuurin|Tietoja tasossa täydet oikeudet. <br/>Keskitetyn hallinnan palomuuri-konsolin kautta.|Palomuurin laitteen kustannukset. <br/>Integroitu ei Azure RBAC.|

Alla ratkaisun käyttää palomuurin virtual laitteiden toteuttamisesta tilanne verkon DMZ/suojattuja.

## <a name="considerations"></a>Huomioon otettavia seikkoja

Voit ottaa käyttöön ympäristön selitetty edellä Azure käyttämällä eri ominaisuuksia, tänään, seuraavasti.

- **Virtual verkon (VNet)**. Azure-VNet toimii samalla tavalla paikalliseen verkkoon ja voidaan Segmentoitu siirtäminen yhden tai useamman aliverkosta liikenne eristystaso sekä koskee erottaminen on.
- **Virtual laitteen**. Useita kumppanien on virtual laitteita, joita voi käyttää kolme palomuurit yllä olevien ohjeiden Azure Marketplacesta. 
- **Käyttäjän määrittämät tiet (UDR)**. Reitin taulukoita voi olla käyttämä Azure verkko ohjaamaan VNet-pakettien UDRs. Aliverkosta nämä reititystaulukot voi suojata. Yksi Azure uusimpia toimintoja on mahdollisuus käyttää reititystaulukon GatewaySubnet mahdollistavat edelleen kaikki liikenne tulevat kyselyjä Azure VNet hybrid yhteyden virtual laitteeseen.
- **IP-osoitteen välityksen**. Oletusarvon mukaan Azure verkko ohjelma toimittaa VPN käyttöliittymän kortit (NIC)-paketit vain, jos paketin kohde-IP-osoite vastaa NIC IP-osoite. Sen vuoksi, jos UDR määrittää paketin lähetettävä annetun virtual laitteen, Azure verkko ohjelma pudota paketin. Jotta paketin toimitetaan AM (Kirjoita tässä tapauksessa virtual laitteen), joka ei ole paketin todellinen kohde, tarvitset käyttöön virtual laitteen IP-osoitteen välityksen.
- **Verkon käyttöoikeusryhmät (NSGs)**. Alla olevassa esimerkissä Soita käyttäminen NSGs, mutta NSGs käytetty aliverkosta ja/tai NIC-ratkaisun avulla voi suodattaa ja siitä uloskirjautuminen näiden aliverkosta ja NIC liikenne.


![IPv6-yhteyden](./media/virtual-network-scenario-udr-gw-nva/figure01.png)

Tässä esimerkissä on tilauksessa, joka sisältää seuraavat:

- 2 resurssiryhmät, ei näy kaaviossa. 
    - **ONPREMRG**. Sisältää kaikki resurssit tarvittavat simuloida paikalliseen verkkoon.
    - **AZURERG**. Sisältää kaikki Azure virtual Verkkoympäristössä tarvittavat resurssit. 
- VNet, nimeltä **onpremvnet** käytetään jäljitteleminen paikallisen-palvelinkeskukseen, Segmentoitu alla kuvatulla tavalla.
    - **onpremsn1**. Jos aliverkon sisältävä virtual machine (AM), joka on käynnissä Ubuntu jäljitteleminen on paikallinen-palvelin.
    - **onpremsn2**. Jos aliverkon sisältävä AM, Ubuntu jäljitteleminen paikalliseen tietokoneeseen järjestelmänvalvojan käyttämä käyttöjärjestelmä.
- On yksi palomuurin virtual laitteen, nimeltä **OPFW** **onpremvnet** käyttää ylläpitämään **azurevnet**tunnelin.
- VNet, jonka nimi on **azurevnet** Segmentoitu alla kuvatulla tavalla.
    - **azsn1**. Ulkoista palomuuria aliverkon käytetään ainoastaan ulkoista palomuuria. Kaikki Internet-liikenne annettava tämän aliverkon kautta. Tämän aliverkon sisältää vain NIC, linkitetty ulkoista palomuuria.
    - **azsn2**. Edusta aliverkon isännöinnin AM, käynnissä verkkopalvelimeen, joita käytetään Internetistä.
    - **azsn3**. Taustajärjestelmä aliverkon isännöinnin AM, käynnissä Taustajärjestelmä sovelluksen palvelin, jossa käytetään edusta-WWW-palvelin.
    - **azsn4**. Hallinnan aliverkon käyttää yksinomaan kaikki palomuurin virtual laitteiden hallinta pääsy. Tämän aliverkon sisältää vain NIC jokaiselle palomuurin virtual laitteelle ratkaisun käytetään.
    - **GatewaySubnet**. Azure hybrid yhteyden aliverkon ExpressRoute ja VPN-yhdyskäytävän tarvittavat antamaan Azure VNets ja muiden verkkojen välinen yhteys. 
- On 3 palomuurin virtual laitteiden **azurevnet** verkossa. 
    - **AZF1**. Julkinen Internet näyttämiä resurssille julkisen IP-osoitteen käyttäminen Azure ulkoista palomuuria. Haluat varmistaa, käytössäsi ovat mallia Marketplacesta tai suoraan laitteen toimittajan kyseisen säännösten 3 NIC virtual laitteen.
    - **AZF2**. Sisäinen palomuuri käytetään ohjaamaan liikenteen **azsn2** ja **azsn3**välillä. Tämä on myös 3 NIC virtual laitteen.
    - **AZF3**. Järjestelmänvalvojat voivat käyttää paikallisen palvelinkeskuksen ja liitetty avulla voit hallita kaikkia palomuuri-laitteiden hallinta aliverkon hallinta palomuuri. Voit etsiä 2 NIC virtual laitteen malleja Marketplacesta tai pyytää suoraan laitteen toimittajalta.

## <a name="user-defined-routing-udr"></a>Käyttäjän määrittämät reititys (UDR)

Azure kunkin aliverkon voidaan linkittää UDR taulukon avulla voit määrittää, miten liikenne aloitetaan siten, että aliverkon reititetään. Jos UDRs ei ole määritetty, Azure käytetään oletusarvon tiet, jotta liikenne paikalliseen Toiminnonkulku yhden aliverkon. Käy ymmärtämään UDRs, [mitä käyttäjän määrittämät tiet, ja IP-osoitteen välityksen](./virtual-networks-udr-overview.md#ip-forwarding).

Jotta viestintä on valmis – oikea palomuuri laitteen edellä viimeisen vaatimus perusteella haluat luoda seuraavat reitti-taulukko, joka sisältää UDRs- **azurevnet**.

### <a name="azgwudr"></a>azgwudr

Tässä skenaariossa juoksutus paikallisen from Azure vain liikenteen hallinta palomuurit yhdistämällä **AZF3**käytetään ja liikenne on Siirry sisäinen **AZF2**palomuurin läpi. Vain yksi reititys on tarpeen **GatewaySubnet** alla kuvatulla tavalla.

|Kohde|Seuraavan kohteen|SELITYS|
|---|---|---|
|10.0.4.0/24|10.0.3.11|Sallii paikallisen liikenteen hallinta palomuurin **AZF3** saavuttamiseksi.|

### <a name="azsn2udr"></a>azsn2udr

|Kohde|Seuraavan kohteen|SELITYS|
|---|---|---|
|10.0.3.0/24|10.0.2.11|Mahdollistaa tietoliikenteen Taustajärjestelmä aliverkon isännöinnin sovelluspalvelin **AZF2** kautta|
|0.0.0.0/0|10.0.2.10|Sallii kaikki muu tietoliikenne reititetään **AZF1** kautta|

### <a name="azsn3udr"></a>azsn3udr

|Kohde|Seuraavan kohteen|SELITYS|
|---|---|---|
|10.0.2.0/24|10.0.3.10|Mahdollistaa tietoliikenteen **azsn2** kulkevan sovelluksen palvelimesta webserver **AZF2** kautta|

Sinun on myös aliverkosta reitin taulukoiden luominen **onpremvnet** jäljitteleminen paikallisen palvelinkeskukseen.

### <a name="onpremsn1udr"></a>onpremsn1udr

|Kohde|Seuraavan kohteen|SELITYS|
|---|---|---|
|192.168.2.0/24|192.168.1.4|Mahdollistaa tietoliikenteen **onpremsn2** **OPFW** kautta|

### <a name="onpremsn2udr"></a>onpremsn2udr

|Kohde|Seuraavan kohteen|SELITYS|
|---|---|---|
|10.0.3.0/24|192.168.2.4|Mahdollistaa tietoliikenteen varmuuskopioidut aliverkon Azure-tietokannassa **OPFW** kautta|
|192.168.1.0/24|192.168.2.4|Mahdollistaa tietoliikenteen **onpremsn1** **OPFW** kautta|

## <a name="ip-forwarding"></a>IP-välityksen 

UDR ja IP-osoitteen välityksen ovat ominaisuuksia, joita voit käyttää yhdessä, jotta voidaan hallita liikenne vuo Azure-VNet virtual laitteita.  Virtuaalinen laitteen ei ole enää mitään muuta kuin AM, joka suoritetaan, sovellus käyttää käsittelyyn verkkoliikennettä jollakin tavalla, kuten palomuurin tai NAT-laitteen.

Tämä virtual laitteen AM on oltava vastaanottaa saapuvan liikenteen, joka ei ole itse osoitettu. Anna AM vastaanottamaan tietoliikennettä osoitettu muihin kohteisiin, on otettava käyttöön IP-osoitteen välityksen AM. Tämä on Azure asetusta, ei Vieras-käyttöjärjestelmässä. Virtuaalinen laitteen on edelleen suorittaa joitakin sovelluksen saapuvan liikenteen ja lähettää sen oikein.

Saat lisätietoja IP-osoitteen välityksen käy [mitä käyttäjän määrittämät tiet, ja IP-osoitteen välityksen](./virtual-networks-udr-overview.md#ip-forwarding).

Esimerkkinä Kuvitellaan Azure vnet ovat seuraavat asetukset:

- Aliverkon **onpremsn1** sisältää AM, nimeltä **onpremvm1**.
- Aliverkon **onpremsn2** sisältää AM, nimeltä **onpremvm2**.
- Virtuaalinen laitteen, nimeltä **OPFW** on yhdistetty **onpremsn1** ja **onpremsn2**.
- Käyttäjän määrittämät reitin linkitetty **onpremsn1** määrittää, että kaikki liikenne paikalliseen **onpremsn2** lähetettävä **OPFW**.

Tässä vaiheessa Jos **onpremvm1** yrittää muodostaa yhteyden **onpremvm2**, UDR käytetään ja liikenne lähetetään **OPFW** kuin seuraavan kohteen. Ota huomioon, että todellinen paketin kohteen muutu, edelleen lukee **onpremvm2** on kohde. 

Ilman IP-osoitteen välityksen **OPFW**käytössä Azure virtual verkko logiikan pudota paketit, koska se sallii pakettien lähettämisen AM, jos AM IP-osoite on paketin kohde.

IP-osoitteen välityksen Azure virtual verkon logiikan lähettää edelleen pakettien OPFW, muuttamatta sen alkuperäiseen kohdeosoite. **OPFW** on täyttökahva paketit ja mitä voit tehdä.

Yllä skenaarion toimimaan, sinun on otettava IP-osoitteen välityksen NIC- **OPFW**, **AZF1**, **AZF2**ja **AZF3** , joita käytetään reititys (kaikki NIC linkitetty hallinta aliverkon niistä lukuun ottamatta). 

## <a name="firewall-rules"></a>Palomuurisäännöt

Yllä olevien ohjeiden mukaisesti IP-osoitteen välityksen vain varmistaa pakettien lähetetään virtual laitteita. Oman laitteen on vielä päättää, mitä tehdään näiden pakettien. Yllä tilanteessa tarvitset seuraavien sääntöjen luominen oman laitteiden:

### <a name="opfw"></a>OPFW

OPFW edustaa paikallisen-laite, joka sisältää seuraavat säännöt:

- **Reitin**: kaikki liikenne paikalliseen 10.0.0.0/16 (**azurevnet**) on lähetettävä tunnelin **ONPREMAZURE**kautta.
- **Käytäntö**: Salli kaikkien sellaisten kaksisuuntaisten liikenteen **port2** ja **ONPREMAZURE**välillä.
 
### <a name="azf1"></a>AZF1

AZF1 edustaa Azure virtual laitteen sisältävät seuraavat säännöt:

- **Käytäntö**: Salli kaikkien sellaisten kaksisuuntaisten liikenteen **port1** ja **port2**välillä.

### <a name="azf2"></a>AZF2

AZF2 edustaa Azure virtual laitteen sisältävät seuraavat säännöt:

- **Reitin**: kaikki liikenne paikalliseen 10.0.0.0/16 (**onpremvnet**) lähetettävä Azure yhdyskäytävän IP-osoite (eli 10.0.0.1) **port1**kautta.
- **Käytäntö**: Salli kaikkien sellaisten kaksisuuntaisten liikenteen **port1** ja **port2**välillä.

## <a name="network-security-groups-nsgs"></a>Verkon käyttöoikeusryhmät (NSGs)

Tässä skenaariossa on ei käytetä NSGs. Voit kuitenkin käyttää NSGs kunkin aliverkon rajoittaa saapuvan ja lähtevän tietoliikenteen. Voit esimerkiksi käyttää seuraavia NSG sääntöjä ulkoisen FW aliverkon.

**Saapuvan postin**

- Salli kaikki TCP-liikenne Internetistä aliverkon minkä tahansa AM 80 porttiin.
- Estä kaikki muu tietoliikenne Internetistä.

**Lähtevän postin**
- Estä kaikki liikenne Internetiin.

## <a name="high-level-steps"></a>Korkean tason vaiheet

Tässä skenaariossa ottamaan korkean tason seuraavia ohjeita noudattamalla.

1.  Azure-tilaukseen kirjautuminen.
2.  Jos haluat ottaa käyttöön VNet jäljitteleminen paikalliseen verkkoon, Varaa resurssit, jotka ovat osa **ONPREMRG**.
3.  Varaa resurssit, jotka ovat osa **AZURERG**.
4.  Valmistele- **onpremvnet** tunnelia **azurevnet**.
5.  Kun kaikki resurssit on valmisteltu, **onpremvm2** kirjautua ja ping 10.0.3.101 Testaa **onpremsn2** ja **azsn3**välinen yhteys.

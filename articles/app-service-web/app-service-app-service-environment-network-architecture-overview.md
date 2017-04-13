<properties 
    pageTitle="Verkoston arkkitehtuuri yleiskatsaus sovelluksen palvelun ympäristöissä" 
    description="Verkon topologian ofApp palvelun ympäristöissä arkkitehtuuri yleiskatsaus." 
    services="app-service" 
    documentationCenter="" 
    authors="stefsch" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="stefsch"/>   

# <a name="network-architecture-overview-of-app-service-environments"></a>Verkoston arkkitehtuuri yleiskatsaus sovelluksen palvelun ympäristöissä

## <a name="introduction"></a>Johdanto ##
Sovelluksen palvelun ympäristöissä luodaan aina [VPN] aliverkon[ virtualnetwork] -sovelluksen-palvelun ympäristössä sovellukset voivat viestiä yksityinen päätepisteet saman virtual verkkotopologia sijaitsevat.  Koska asiakkaat voivat lukita niiden virtual verkkoinfrastruktuuria osat, on tärkeää ymmärtää tiedostotyypit, verkon tietoliikenne kulkee, ilmetä App Service-ympäristössä.

## <a name="general-network-flow"></a>Yleisen verkon työnkulku ##
 
Kun sovellus palvelun ympäristössä (Ietokannan) käyttää julkinen virtual IP-osoite (VIP)-sovellusten, kaiken saapuvan liikenteen saapuu, julkisen VIP.  Tämä sisältää liikenteen HTTP- ja HTTPS-sovellusten ja muu tietoliikenne FTP, remote muistin toiminnot ja Azure hallintatoiminnot.  Käytettävissä olevat julkiset VIP tietyt portit (pakolliset ja valinnaiset) täydellinen luettelo on artikkelissa rajoittamisesta [Saapuvan liikenteen] [ controllinginboundtraffic] sovelluksen palvelun ympäristöön. 

Sovelluksen palvelun ympäristöissä tukevat myös käynnissä olevat sovellukset, jotka on sidottu vain VPN sisäisen osoitteen, kutsutaan myös ILB (sisäinen kuormituksen) osoite.  ILB käytössä Ietokannan, HTTP ja HTTPS-liikenne sovellukset sekä remote muistin puhelut-saapuvat ILB-osoite.  Yleisimmät ILB Ietokannan määritykset, FTP/FTPS liikenne saapuu myös ILB-osoite.  Kuitenkin Azure hallintatoiminnot edelleen Toiminnonkulku ILB julkisen VIP portit 454/455 käytössä Ietokannan.

Seuraavassa kuvassa näkyy eri saapuvan ja lähtevän verkon kulkee yleiskuvaus missä sovellukset on sidottu julkinen virtual IP-osoite sovellus Service-ympäristöön:

![Yleisen verkon työnkulut][GeneralNetworkFlows]

Yksityinen asiakkaan päätepisteet erilaisilla viestiä App Service-ympäristössä.  Esimerkiksi sovellukset App Service-ympäristössä muodostaa yhteyden tietokantaan palvelimesta IaaS näennäiskoneiden saman virtual verkkotopologia-käyttöjärjestelmässä.

>[AZURE.IMPORTANT] Katsoo verkkokaavion "Muut Laske resurssien" otetaan käyttöön eri aliverkon App Service-ympäristöstä. Käyttöönotto Ietokannan saman aliverkon resurssien estä yhteyden Ietokannan-resurssit (lukuun ottamatta tietyn sisäisessä-Ietokannan reititys). Ottaa käyttöön eri aliverkon sijaan (sama VNET). Sovelluksen Service-ympäristön sitten voi yhdistää. Tarvitaan lisämäärityksiä.

Sovelluksen palvelun ympäristöissä myös yhteyttä Sql DB ja Azure-tallennustilan hallinta ja sovelluksen palvelun toimintaympäristö tarvittavat resurssit.  On Sql- ja tallennustilaa resursseissa, joka on yhteydessä App Service-ympäristössä sijaitsevat sama alue kuin sovelluksen Service-ympäristössä, kun muiden sijaitsevat remote Azure alueilla.  Tuloksena lähtevän Internet-yhteys on aina pakollinen App palvelun ympäristöön toimii oikein. 

Koska aliverkon ympäristössä App palvelu on otettu käyttöön, verkon käyttöoikeusryhmät avulla voidaan hallita aliverkon saapuvan liikenteen.  Lisätietoja siitä, miten voit hallita saapuvan liikenteen App Service-ympäristöön on seuraavassa [artikkelissa][controllinginboundtraffic].

Lisätietoja lähtevän Internet-yhteys sallimisesta App Service-ympäristön on seuraavassa artikkelissa käyttämisestä [Express reitin][ExpressRoute].  Artikkelissa kuvatulla tavalla samoin käyttää käsiteltäessä sivuston sivuston yhteyden ja käyttämällä pakotettu tunneling.

## <a name="outbound-network-addresses"></a>Lähtevän verkko-osoitteet ##
Kun sovellus palvelun ympäristössä tekee lähtevät puhelut, IP-osoite on aina liitetty lähtevät puhelut.  IP-osoite, jota käytetään määräytyy kutsutaan päätepisteen sijaitseeko virtual verkkotopologia tai virtual verkkotopologia ulkopuolella.

Jos kutsutaan päätepisteen on **ulkopuolella** VPN-topologian, lähtevä osoite (eli lähtevä NAT-osoite), jota käytetään on julkinen VIP App Service-ympäristön.  Tämä osoite löytyy portaalin käyttöliittymässä ominaisuudet sivu App Service-ympäristössä.
 
![Lähtevät IP-osoite][OutboundIPAddress]

Tämä osoite voidaan määrittää myös ASEs, joissa on vain julkinen VIP luomalla sovelluksen App Service-ympäristössä ja suorittaa sitten *nslookup* sovelluksen osoite. Voimassa oleva IP-osoite on sekä julkinen VIP sekä sovelluksen palvelun ympäristön lähtevän NAT-osoite.

Jos kutsutaan päätepisteen ei **sisällä** VPN-topologian, puheluja sovelluksen lähtevä osoite on käynnissä sovelluksen yksittäisiä Laske resurssin sisäinen IP-osoite.  Mutta ei ole pysyvät VPN sisäisiä IP-osoitteita sovelluksiin määritystä.  Sovellusten voi liikkua eri Laske resurssit ja vuoksi skaalauksen toimintojen muuttaa resurssien App Service-ympäristössä käytettävissä Laske resurssivarantoon.

Kuitenkin App Service-ympäristössä on aina aliverkon sijaitsevat, sinun on taata käynnissä sovelluksen Laske resurssin sisäinen IP-osoite aina oltava aliverkon CIDR-alueella.  Tuloksena käytettäessä tarkasti rajattuja käyttöoikeusluettelot tai verkon käyttöoikeusryhmät suojaamiseen pääsy muiden päätepisteet virtual verkoston aliverkon-alueeseen, joka sisältää sovelluksen Service-ympäristö on tietokannan käyttöoikeudet.

Seuraavassa kaaviossa on esitetty käsitteiden tarkemmin:

![Lähtevän verkko-osoitteet][OutboundNetworkAddresses]

Yllä olevassa kaaviossa:

- Sovelluksen Service-ympäristön julkisen VIP on 192.23.1.2, eli käytetään, kun kutsuja "Internet-päätepisteet Lähtevät IP-osoite.
- Sovelluksen Service-ympäristön sisältävän aliverkon CIDR on 10.0.1.0/26.  Muut päätepisteet saman VPN-infrastruktuurin näkevät puhelut sovelluksista kuin peräisin johonkin osoite tällä alueella.

## <a name="calls-between-app-service-environments"></a>Sovelluksen palvelun ympäristöissä väliset puhelut ##
Monimutkaisempia tilanne voi ilmetä, jos käyttöön useita sovelluksen palvelun ympäristöissä virtual samassa verkostossa ja tee lähtevät puhelut yhden sovelluksen Service-ympäristöstä toiseen App Service-ympäristössä.  Seuraavanlaisiin toimintojen App palvelun ympäristön puhelut käsitellään myös "Internet-puhelut.

Seuraavassa kaaviossa on esitetty Esimerkki kerroksittain arkkitehtuuri yhden sovelluksen Service-ympäristössä (esimerkiksi "Eteen oven" verkkosovelluksissa)-sovellusten ja kutsumista sovellukset toisen sovelluksen palvelun ympäristössä (esimerkiksi sisäinen taustatietokantaan API sovellukset ole tarkoitus olla käytettävissä Internetistä). 

![Sovelluksen palvelun ympäristöissä väliset puhelut][CallsBetweenAppServiceEnvironments] 

Yllä olevassa esimerkissä App palvelun ympäristön "Ietokannan yhden" on 192.23.1.2 Lähtevät IP-osoite.  Jos sovelluksen käytössä tämän sovelluksen palvelun ympäristön tekee lähtevän kutsun sovelluksen toisen sovelluksen palvelun ympäristössä, jossa ("Ietokannan kaksi") käytössä sijaitsevat samassa virtual verkossa lähtevä puhelu on käsitellään "Internet-puhelu.  Tuloksena saapuvat toinen verkkoliikennettä App palvelun ympäristön näkyy muodossa peräisin 192.23.1.2 (eli ei aliverkon osoite alueen ensimmäinen App Service-ympäristön).

Vaikka eri App palvelun ympäristöissä väliset puhelut käsitellään "Internet-puhelut, kun sovellus palvelun kummassakin ympäristössä sijaitsevat saman Azure alueen ‑sovelluksen verkkoliikenteen tarpeisiin säilyy alueellisen Azure verkossa ja ei fyysisesti ohjaa julkisen Internetin välityksellä.  Tuloksena voit käyttää verkon käyttöoikeusryhmän toisen sovelluksen Service-ympäristön aliverkon päästää vain ensimmäisen sovelluksen Service-ympäristöstä (jonka Lähtevät IP-osoite on 192.23.1.2), saapuvat puhelut varmistaa tietosuojaa App palvelun ympäristöjen välillä.

## <a name="additional-links-and-information"></a>Muita linkkejä ja tiedot ##
Kaikki artikkelit ja miten-, Muokkaa sovelluksen palvelun ympäristöissä ovat käytettävissä [sovelluksen palvelun ympäristöissä Lueminut-tiedosto](../app-service/app-service-app-service-environments-readme.md).

Lisätietoja saapuva App palvelun ympäristöissä porteista ja saapuvan liikenteen hallinta verkon käyttöoikeusryhmät avulla on käytettävissä [seuraavassa][controllinginboundtraffic].

Käyttämisestä käyttäjän tiedot on määritetty tiet myöntää App palvelun ympäristöissä lähtevän Internet-yhteys on käytettävissä tämän [artikkelin][ExpressRoute]. 


<!-- LINKS -->
[virtualnetwork]: http://azure.microsoft.com/services/virtual-network/
[controllinginboundtraffic]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[ExpressRoute]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/

<!-- IMAGES -->
[GeneralNetworkFlows]: ./media/app-service-app-service-environment-network-architecture-overview/NetworkOverview-1.png
[OutboundIPAddress]: ./media/app-service-app-service-environment-network-architecture-overview/OutboundIPAddress-1.png
[OutboundNetworkAddresses]: ./media/app-service-app-service-environment-network-architecture-overview/OutboundNetworkAddresses-1.png
[CallsBetweenAppServiceEnvironments]: ./media/app-service-app-service-environment-network-architecture-overview/CallsBetweenEnvironments-1.png


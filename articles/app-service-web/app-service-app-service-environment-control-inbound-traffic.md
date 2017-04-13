<properties 
    pageTitle="Sovelluksen Service-ympäristöön saapuvan liikenteen hallinta" 
    description="Lisätietoja verkon suojaussäännöt ohjaamaan saapuvan liikenteen App palvelun ympäristöön määrittämisestä." 
    services="app-service" 
    documentationCenter="" 
    authors="ccompy" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/02/2016" 
    ms.author="stefsch"/>   

# <a name="how-to-control-inbound-traffic-to-an-app-service-environment"></a>Sovelluksen Service-ympäristöön saapuvan liikenteen hallinta

## <a name="overview"></a>Yleiskatsaus ##
Sovelluksen Service-ympäristössä voidaan luoda- **joko** virtual Azure Resurssienhallinta-verkko- **tai** classic käyttöönoton malli [VPN][virtualnetwork].  Uusi virtual verkko- ja uusi aliverkon voidaan määrittää sovelluksen palvelun ympäristö luodaan aikaan.  Voit myös sovelluksen Service-ympäristössä voidaan luoda olemassa virtual verkko-ja olemassa aliverkon.  Viimeaikaisesta muutoksesta kesäkuussa 2016 tehdään, jossa ASEs nyt voidaan ottaa huomioon virtual verkkoja, jotka käyttävät julkinen osoitealueet tai RFC1918 osoite välilyöntejä (eli yksityisten osoitteiden).  Lisätietoja sovelluksen Service-ympäristössä luomisesta on artikkelissa [miten Luo sovelluksen Service-ympäristössä][HowToCreateAnAppServiceEnvironment].

Sovelluksen Service-ympäristössä aina on luotava aliverkon sisällä, koska aliverkon muodostaa verkon rajan, joiden avulla voit lukita saapuvan liikenteen takana seuraavat laitteet ja palvelut siten, että HTTP- ja HTTPS-liikenne hyväksytään vain tietyn ylöspäin IP-osoitteista.

Saapuvan ja lähtevän tietoliikenteen aliverkon hallitaan [verkon käyttöoikeusryhmän]käyttäminen[NetworkSecurityGroups]. Saapuvan liikenteen hallinta edellyttää verkon suojaussäännöt luominen verkon käyttöoikeusryhmän ja sitten verkkosuojauksen määrittäminen ryhmälle aliverkon sisältävän sovelluksen Service-ympäristössä.

Kun käyttöoikeusryhmän verkko on määritetty aliverkon, saapuvan liikenteen sovelluksiin App Service-ympäristössä on sallittujen ja estettyjen Salli perusteella ja estä verkko-käyttöoikeusryhmän määritetty sääntöjä.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="network-ports-used-in-an-app-service-environment"></a>Verkko-portit, joita käytetään sovelluksen Service-ympäristössä ##
Ennen kuin lukitus saapuvan verkkoliikenteen verkon käyttöoikeusryhmän kanssa, on tärkeää tietää App Service-ympäristössä porteista pakolliset ja valinnaiset verkon määrittäminen.  Sulkeminen vahingossa-liikenne paikalliseen joitakin portit käytöstä voi johtaa toiminnallisuuteen App Service-ympäristössä.

Seuraavassa on luettelo porteista App Service-ympäristössä. Kaikki portit ovat **TCP**, ellei muuta selvästi:

- 454: **pakollinen portin** käyttämä Azure infrastruktuurin hallinta ja ylläpito App palvelun ympäristöissä SSL kautta.  Estä liikenteen tähän porttiin.  Tämä portti on aina sidottu Ietokannan julkisen VIP.
- 455: **pakollinen portin** käyttämä Azure infrastruktuurin hallinta ja ylläpito App palvelun ympäristöissä SSL kautta.  Estä liikenteen tähän porttiin.  Tämä portti on aina sidottu Ietokannan julkisen VIP.
- 80: portti sovelluksiin App palvelun suunnitelmien App Service-ympäristössä käytössä HTTP-liikenteen oletus.  ILB käyttävä Ietokannan portin sitoa Ietokannan ILB osoitetta.
- 443: Oletus portti SSL liikenteen sovelluksiin App palvelun suunnitelmien App Service-ympäristössä käytössä.  ILB käyttävä Ietokannan portin sitoa Ietokannan ILB osoitetta.
- 21: FTP ohjausobjektin kanava.  Portin voi olla estetty turvallisesti, jos FTP ei käytetä.  ILB käyttävä Ietokannan, valitse portin voidaan sitoa ILB osoite Ietokannan varten.
- 990: FTPS ohjausobjektin kanava.  Portin voi olla estetty turvallisesti, jos FTPS ei käytetä.  ILB käyttävä Ietokannan, valitse portin voidaan sitoa ILB osoite Ietokannan varten.
- 10001 10020: tietojen kanavat FTP.  Ohjausobjektin, kanavan kanssa porttien voidaan turvallisesti estää Jos FTP ei käytetä.  ILB käyttävä Ietokannan-portin voit sitoa Ietokannan ILB osoitteeseen.
- 4016: remote portin Visual Studio 2012.  Portin voi olla estetty turvallisesti, jos ominaisuus ei ole käytössä.  ILB käyttävä Ietokannan portin sitoa Ietokannan ILB osoitetta.
- 4018: remote portin Visual Studio 2013: n kanssa.  Tämä portti voit estää turvallisesti, jos ominaisuus ei ole käytössä.  ILB käyttävä Ietokannan portin sitoa Ietokannan ILB osoitetta.
- 4020 kuuluu yritykseen: remote portin Visual Studio 2015.  Portin voi olla estetty turvallisesti, jos ominaisuus ei ole käytössä.  ILB käyttävä Ietokannan portin sitoa Ietokannan ILB osoitetta.

## <a name="outbound-connectivity-and-dns-requirements"></a>Lähtevien yhteyksien ja DNS-vaatimukset ##
Sovelluksen palvelun ympäristöön toimii oikein se vaatii eri päätepisteet lähtevän pääsyä. Täydellinen luettelo Ietokannan käyttämän ulkoisen päätepisteet on [Verkon määritysten ExpressRoute varten](app-service-app-service-environment-network-configuration-expressroute.md#required-network-connectivity) on artikkelissa "Pakollinen verkkoyhteyden"-osassa.

Sovelluksen palvelun ympäristöissä edellyttävät kelvollinen DNS-infrastruktuurin määritetty virtual verkkoon.  Jos DNS-määrityksiä muutetaan jostakin syystä ympäristössä palvelun sovelluksen luomisen jälkeen, kehittäjät voit pakottaa sovelluksen palvelun ympäristössä, jos haluat jatkaa uuden DNS-määrityksiä.  Käynnistävä juokseva ympäristön uudelleenkäynnistyksen "Uudelleen"-kuvakkeen sijaitsevat [Azure portal] -sovellus palvelun ympäristön hallinta-sivu yläreunassa[ NewPortal] aiheuttaa ympäristön Nosta uusi DNS-määrityksiä.

On myös suositeltavaa, että kaikki mukautetut DNS-palvelimet vnet määritettävä etuajassa ennen luominen sovelluksen Service-ympäristössä.  Jos virtual verkon DNS-määritys muutetaan kesken App palvelun ympäristö luodaan, joka johtaa sovelluksen palvelun ympäristön luominen prosessin epäonnistumista.  Vastaavat alueilla: Jos mukautettujen DNS-palvelin on olemassa VPN-yhdyskäytävän toinen pää- ja DNS-palvelin ei ole käytettävissä tai poissa käytöstä, sovelluksen palvelun ympäristön luontiprosessi myös epäonnistuu.

## <a name="creating-a-network-security-group"></a>Verkon käyttöoikeusryhmän luominen ##
Tarkat tiedot siitä, miten verkkosuojauksen ryhmittelee työ-kohdassa seuraavat [tiedot][NetworkSecurityGroups].  Tietoja alla kosketus korostaa verkon suojausryhmien kohdistus määrittäminen ja verkon käyttöoikeusryhmän soveltaminen aliverkon, joka sisältää sovelluksen Service-ympäristössä.

**Huomautus:** Verkon suojausryhmien voi määrittää käyttämällä graafisesti [Azure Portal](https://portal.azure.com) tai Azure PowerShellin kautta.

Verkon käyttöoikeusryhmät luodaan ensin tilauksen liittyvät erillinen kokonaisuutena. Koska verkon suojausryhmien luodaan Azure alue, varmista verkko-käyttöoikeusryhmän luodaan samalla alueella kuin sovelluksen Service-ympäristössä.

Seuraavassa esitellään verkon käyttöoikeusryhmän luominen:

    New-AzureNetworkSecurityGroup -Name "testNSGexample" -Location "South Central US" -Label "Example network security group for an app service environment"

Kun verkon käyttöoikeusryhmän on luotu, verkko-suojaussäännöt lisätään siihen.  Koska sääntöryhmän voivat muuttua ajan kuluessa, on suositeltavaa väliin, numerointimallissa käytetään prioriteettien avulla on helppo lisätä lisäsääntöjä ajan kuluessa.

Alla olevassa esimerkissä säännön, jonka myöntää hallinta-porttien tarvitsemia Azure infrastruktuurin hallinta ja ylläpitää App Service-ympäristössä.  Huomaa, että kaikki hallinta liikenne jatkuu SSL: n ja asiakkaan varmenteet suojattu niin vaikka portit avataan ne eivät ole käytettävissä minkä tahansa kohteen kuin Azure hallinta infrastruktuurin mukaan.


    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "ALLOW AzureMngmt" -Type Inbound -Priority 100 -Action Allow -SourceAddressPrefix 'INTERNET'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '454-455' -Protocol TCP
    

Kun lukitus pääsy portti 80 ja 443 "Piilota" sovelluksen Service-ympäristön takana seuraavat laitteet tai palveluja, sinun on tietää seuraavat IP-osoite.  Esimerkiksi jos käytössäsi on web palomuurin (WAF), WAF on oma IP-osoite (tai osoitteet) jota ohjelma käyttää, kun joiden mukaan-liikenne paikalliseen edeltävät App Service-ympäristössä.  Sinun on käytettävä IP-osoitteen verkon suojauksen säännön *SourceAddressPrefix* -parametri.

Seuraavassa esimerkissä saapuvan liikenteen ylöspäin IP-osoite-sallitaan erikseen.  Osoitteen *1.2.3.4* käytetään paikkamerkkinä ylöspäin WAF IP-osoite.  Muuttaa vastaamaan ylöspäin laitteen tai palvelun käytettävä osoite-arvoa.

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT HTTP" -Type Inbound -Priority 200 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT HTTPS" -Type Inbound -Priority 300 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP
    
Tarvittaessa FTP-tuki seuraavien sääntöjen voidaan mallina FTP ohjausobjektin portti- ja tietojen käyttöoikeuden myöntäminen kanavan portit.  Koska FTP tilallisten protokolla, ei ehkä voi reitittää FTP liikenteen perinteinen HTTP/HTTPS-palomuurin tai välityspalvelimen laitteen läpi.  Tässä tapauksessa sinun on määritetty *SourceAddressPrefix* arvon – esimerkiksi IP-osoitealueita kehittäjä tai käyttöönoton koneet, mitkä FTP asiakkaiden on käynnissä. 

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT FTPCtrl" -Type Inbound -Priority 400 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '21' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT FTPDataRange" -Type Inbound -Priority 500 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '10001-10020' -Protocol TCP

(**Huomautus:** kanava-portin tietoalueen voivat muuttua aikana esikatselu.)

Jos remote virheenkorjaus Visual Studiossa käytetään seuraavia sääntöjä näytetään, miten käyttöoikeus.  Ei kunkin tuettuun Visual Studio erillisen säännön, koska jokaisessa versiossa käyttää eri porttia remote virheenkorjaus.  FTP Accessin kanssa remote muistin liikenne saattaa rivity oikein perinteinen WAF tai laitteen välityspalvelimen kautta.  *SourceAddressPrefix* voi määrittää sen sijaan IP-osoitealueita Kehitystyökalut-laitteiden käyttöjärjestelmä Visual Studio.

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2012" -Type Inbound -Priority 600 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4016' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2013" -Type Inbound -Priority 700 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4018' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2015" -Type Inbound -Priority 800 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4020' -Protocol TCP

## <a name="assigning-a-network-security-group-to-a-subnet"></a>Verkon käyttöoikeusryhmän määritteleminen aliverkon ##
Verkon käyttöoikeusryhmän on oletusarvon suojaus-sääntö, joka estää kaiken ulkoisen tietoliikenteen käytön.  Yhdistämällä verkko-suojaussäännöt yllä olevien ohjeiden- ja tietoturva sääntö, joka on estäminen saapuvan liikenteen tulos on vain liikenne liittyvän toiminnon *Salli* alueet voivat liikenne lähettää sovellukset App-palvelun ympäristössä lähde-osoitteesta.

Verkon käyttöoikeusryhmän lisätään suojaussäännöt, kun se on voi määrittää aliverkon sisältävän sovelluksen Service-ympäristössä.  Tehtävä-komento viittaa sekä sovelluksen Service-ympäristön sijainti virtual verkon nimi sekä nimi, jossa sovellus Service-ympäristön luotiin aliverkon.  

Alla olevassa esimerkissä on varattu aliverkon ja VPN verkon käyttöoikeusryhmän:


    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-test'

Kun verkko-suojauksen ryhmämäärityksen onnistuu (varauksen on pitkään käynnissä olevien toimintojen ja voi kestää muutaman minuuttia), vain saapuvan liikenteen säännöt *Salli* onnistuneesti saavuttaa sovellukset App Service-ympäristössä.

Seuraavassa esimerkissä esitetään täydellisiä, poistaminen ja dis-liittää aliverkosta verkko-käyttöoikeusryhmän näin:


    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Remove-AzureNetworkSecurityGroupFromSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-test'

## <a name="special-considerations-for-explicit-ip-ssl"></a>Erityistä Huomioitavaa eksplisiittinen IP-SSL ##
Jos sovellus on määritetty eksplisiittinen IP-SSL osoite (käytettävissä ASEs, joilla on vain julkinen VIP), sijaan App Service-ympäristön oletusarvon IP-osoitteen HTTP- ja HTTPS liikenteen kulkee kyselyjä aliverkon on erilaisia kuin portit 80 ja 443 portit päälle.

IP-SSL jokaisen osoitteen porteista yksittäisten kahdet löytyy App palvelun ympäristön tiedot UX sivu-portaalin käyttöliittymässä.  Valitse "kaikki asetukset"--> "IP-osoitteiden".  "IP-osoitteiden"-sivu näyttää kaikki eksplisiittisesti määritetyt IP-SSL-osoitteiden taulukon sovelluksen-palvelu-ympäristössä sekä erityisiä portin pair, jota käytetään reitittää HTTP- ja HTTPS liikenteen kunkin IP-SSL-osoitteeseen liittyvä.  Se on tämä portti pari on sääntöjen määrittäminen verkon käyttöoikeusryhmän käytettävä DestinationPortRange parametrit.

Kun sovellus Ietokannan on määritetty käyttämään IP-SSL-yhteyttä, ulkoiset asiakkaat eivät näe ja ei tarvitse huolehtia erityistä porttia pari yhdistäminen.  Liikenne sovelluksia juoksuttaa tavallisesti määritetty IP-SSL-osoitteeseen.  Muodosta laitepari erityistä porttia käännöksen automaattisesti sisäisesti aikana tapahtuu lopullinen jalan reitittää liikenteen Ietokannan joka sisältää aliverkon kyselyjä. 

## <a name="getting-started"></a>Käytön aloittaminen

Aloita sovelluksen palvelun ympäristöissä artikkelissa [Johdanto App Service-ympäristöön][IntroToAppServiceEnvironment]

Kaikki artikkelit ja miten-, Muokkaa sovelluksen palvelun ympäristöissä ovat käytettävissä [sovelluksen palvelun ympäristöissä Lueminut-tiedosto](../app-service/app-service-app-service-environments-readme.md).

Lisätietoja suojatusti muodostamisesta Taustajärjestelmä resurssin App palvelun ympäristön sovellusten ympärillä on artikkelissa [yhteyden suojatusti Taustajärjestelmä resurssien App palvelun ympäristön][SecurelyConnecttoBackend]

Saat lisätietoja Azure App Service-ympäristö [Azure App palvelun][AzureAppService].

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[HowToCreateAnAppServiceEnvironment]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[IntroToAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[SecurelyConnecttoBackend]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-securely-connecting-to-backend-resources/
[NewPortal]:  https://portal.azure.com  

<!-- IMAGES -->
 

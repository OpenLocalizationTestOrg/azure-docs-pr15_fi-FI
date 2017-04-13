<properties 
    pageTitle="Kerroksittain suojausarkkitehtuuri App palvelun ympäristöjen kanssa" 
    description="Käyttöönoton kerroksittain suojausarkkitehtuuri App palvelun ympäristöjen kanssa." 
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
    ms.date="08/30/2016" 
    ms.author="stefsch"/>   

# <a name="implementing-a-layered-security-architecture-with-app-service-environments"></a>Käyttöönoton kerroksittain suojausarkkitehtuuri App palvelun ympäristöjen kanssa

## <a name="overview"></a>Yleiskatsaus ##
 
Koska App palvelun ympäristöissä Anna käyttöön virtual verkostoon erillään runtime-ympäristössä, kehittäjät voivat luoda kerroksittain suojausarkkitehtuuri tarjoamalla sovelluksen fyysinen kunkin tason verkkokäyttö tasoja.

Yhteisen tahdon on piilottaa API Edellinen-päät Yleiset Internet-yhteys- ja Salli vain API kutsuttava ylöspäin verkkosovelluksissa.  [Verkon käyttöoikeusryhmät (NSGs)] [ NetworkSecurityGroups] voidaan käyttää aliverkosta, jotka sisältävät sovelluksen palvelun ympäristöissä julkisen rajoittamisesta API-sovellukset.

Seuraavassa kuvassa on esimerkki arkkitehtuuri WebAPI mukaan-sovelluksen käyttöön App Service-ympäristössä.  Kolme erillistä web app-esiintymät, ottaa käyttöön kolme erillistä App palvelun ympäristöissä soittaa taustatietokantaan saman WebAPI-sovellukseen.

![Monta-arkkitehtuuri][ConceptualArchitecture] 

Vihreä plusmerkkejä osoittavat verkko-käyttöoikeusryhmän, jos aliverkon, joka sisältää "apiase" sallii saapuvat puhelut ylöspäin web-sovelluksissa sekä kutsut itse nimellä.  Kuitenkin saman verkko-käyttöoikeusryhmän estää erikseen käytön yleiset saapuvan liikenteen Internetistä. 

Loput tässä artikkelissa käydään läpi vaiheet, verkko-käyttöoikeusryhmän määrittämiseksi aliverkon, joka sisältää "apiase".

## <a name="determining-the-network-behavior"></a>Verkon asetusten määrittäminen ##
Jotta tiedät, mitä verkon suojaussäännöt tarvitaan, sinun on määritettävä mitä verkkoasiakkaiden salliminen saavuttamiseksi App Service-ympäristöä, joka sisältää API-sovellus ja mitkä asiakkaat estetään.

[Verkon käyttöoikeusryhmät (NSGs)] jälkeen[ NetworkSecurityGroups] aliverkosta, joita käytetään ja sovelluksen palvelun ympäristöissä on otettu käyttöön kyselyjä aliverkosta, NSG sääntöjä koskevat **kaikki** sovellukset App Service-ympäristössä käytössä.  Esimerkki-arkkitehtuuri käytön tämän artikkelin, kun verkon käyttöoikeusryhmän käytetään aliverkon, joka sisältää "apiase", "apiase" sovelluksen Service-ympäristössä käytössä kaikki sovellukset suojattava suojaussäännöt samat. 

- **Määrittää seuraavat soittajat Lähtevät IP-osoite:**  Mikä on IP-osoite tai ylöspäin soittajat osoitteet?  Nämä osoitteet on erikseen sallitaan access NSG.  Sovelluksen palvelun ympäristöissä väliset puhelut pidetään "Internet-puhelut, koska tämä tarkoittaa lähtevien IP-osoite määritetty jokaiseen kolme App ylöspäin Service-ympäristöissä on sallittava"apiase"aliverkon NSG access.   Lisätietoja tarkistamalla Lähtevät IP-osoite, sovellus-palvelun ympäristössä sovellukset on artikkelissa [Verkon arkkitehtuuri] [ NetworkArchitecture] yleiskatsaus-artikkelissa.
- **Tuleeko taustatietokantaan API-sovelluksen Soita itse?**  Joskus huomaamatta ja muotoiltu piste on tilanne, jossa taustatietokantaan sovelluksen täytyy kutsua itse.  Jos haluat kutsua itseään API taustatietokantaan-sovellus App Service-ympäristössä, tämä on käsiteltävä myös nimellä "Internet-puhelu.  Esimerkki-arkkitehtuuri Tämä edellyttää, lähtevät IP-osoite "apiase" sovelluksen palvelun ympäristössä sekä käytön salliminen.

## <a name="setting-up-the-network-security-group"></a>Verkko-käyttöoikeusryhmän määrittäminen ##
Kun Lähtevät IP-osoitteiden määrittäminen kutsutaan, seuraava vaihe on muodostaa verkon käyttöoikeusryhmän.  Verkon käyttöoikeusryhmät voi luoda n sekä Resurssienhallinta virtual verkkojen sekä perinteinen virtual verkot.  Seuraavissa esimerkeissä Näytä luominen ja määrittäminen NSG perinteinen virtual verkon PowerShellin avulla.

Esimerkki käyttöympäristön ympäristöjen sijaitsevat Etelä keskitetyn Meille, jotta tyhjä NSG luodaan alueen:

    New-AzureNetworkSecurityGroup -Name "RestrictBackendApi" -Location "South Central US" -Label "Only allow web frontend and loopback traffic"

Ensin eksplisiittinen Salli sääntö on lisätty mainittuja [Saapuvan] liikenteen on artikkelissa Azure hallinta-infrastruktuurin[ InboundTraffic] sovelluksen Service-ympäristössä.

    #Open ports for access by Azure management infrastructure
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW AzureMngmt" -Type Inbound -Priority 100 -Action Allow -SourceAddressPrefix 'INTERNET' -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '454-455' -Protocol TCP
    
Seuraavaksi kaksi sääntöä lisätään sallivat HTTP- ja HTTPS-kutsujen ensimmäisen ylöspäin App Service-ympäristöstä ("fe1ase").

    #Grant access to requests from the first upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe1ase" -Type Inbound -Priority 200 -Action Allow -SourceAddressPrefix '65.52.xx.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe1ase" -Type Inbound -Priority 300 -Action Allow -SourceAddressPrefix '65.52.xx.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Huuhdotaan ja toista toisessa ja kolmannessa ylöspäin App palvelun ympäristöjen ("fe2ase" ja "fe3ase").

    #Grant access to requests from the second upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe2ase" -Type Inbound -Priority 400 -Action Allow -SourceAddressPrefix '191.238.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe2ase" -Type Inbound -Priority 500 -Action Allow -SourceAddressPrefix '191.238.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP
    
    #Grant access to requests from the third upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe3ase" -Type Inbound -Priority 600 -Action Allow -SourceAddressPrefix '23.98.abc.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe3ase" -Type Inbound -Priority 700 -Action Allow -SourceAddressPrefix '23.98.abc.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Lopuksi myöntää oikeuden taustatietokantaan API App palvelun ympäristön Lähtevät IP-osoite, niin, että se voi soittaa takaisin itseensä.

    #Allow apps on the apiase environment to call back into itself
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP apiase" -Type Inbound -Priority 800 -Action Allow -SourceAddressPrefix '70.37.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS apiase" -Type Inbound -Priority 900 -Action Allow -SourceAddressPrefix '70.37.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Ei ole muita verkko suojaussäännöt on määritettävä koska jokaisella NSG on oletusarvoinen säännöt, jotka Internetistä saapuvien käytön estäminen oletusarvoisesti.

Verkko-käyttöoikeusryhmän säännöt täydelliseen näkyvät jäljempänä.  Huomautus siitä, miten viimeisen sääntö, joka näkyy korostettuna, estää saapuvan kaikki soittajat lukuun ottamatta on myönnetty erikseen access-käyttö.

![NSG määritys][NSGConfiguration] 

Viimeinen vaihe on käyttää NSG aliverkon, joka sisältää "apiase" sovelluksen Service-ympäristössä.  

     #Apply the NSG to the backend API subnet
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'yourvnetnamehere' -SubnetName 'API-ASE-Subnet'

NSG on käytetty aliverkon, jossa vain kolme ylöspäin App palvelun ympäristöjen ja sovelluksen Service-ympäristöä, joka sisältää Ohjelmointirajapinnan taustatietokannan, voivat soittaa "apiase"-ympäristössä.


## <a name="additional-links-and-information"></a>Muita linkkejä ja tiedot ##
Kaikki artikkelit ja miten-, Muokkaa sovelluksen palvelun ympäristöissä ovat käytettävissä [sovelluksen palvelun ympäristöissä Lueminut-tiedosto](../app-service/app-service-app-service-environments-readme.md).

[Verkon käyttöoikeusryhmät](../virtual-network/virtual-networks-nsg.md)tietoja. 

Tietoja [Lähtevät IP-osoitteiden] [ NetworkArchitecture] ja sovelluksen Service-ympäristössä.

[Verkon portit] [ InboundTraffic] käyttää sovelluksen Service-ympäristössä.

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[NetworkArchitecture]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-architecture-overview/
[InboundTraffic]:  https://azure.microsoft.com/en-us/documentation/articles/app-service-app-service-environment-control-inbound-traffic/

<!-- IMAGES -->
[ConceptualArchitecture]: ./media/app-service-app-service-environment-layered-security/ConceptualArchitecture-1.png
[NSGConfiguration]:  ./media/app-service-app-service-environment-layered-security/NSGConfiguration-1.png

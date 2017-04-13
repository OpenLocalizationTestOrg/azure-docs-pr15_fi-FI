<properties 
    pageTitle="GEO Distributed asteikko App palvelun ympäristöjen kanssa" 
    description="Opettele skaalata vaakasuunnassa geo-jakauman käyttäminen liikenteen hallinta ja sovelluksen palvelun ympäristöissä sovellukset." 
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
    ms.date="09/07/2016" 
    ms.author="stefsch"/>   

# <a name="geo-distributed-scale-with-app-service-environments"></a>GEO Distributed asteikko App palvelun ympäristöjen kanssa

## <a name="overview"></a>Yleiskatsaus ##
Sovelluksen tilanteita, jotka edellyttävät erittäin suuri asteikko voi ylittää Laske Resurssikapasiteetti-sovelluksen yhteen käyttöönoton käytettävissä.  Äänestyksen sovellusten urheilu tapahtumien ja informaatiojärjestelmä Viihde tapahtumat ovat kaikki esimerkkejä skenaariot, jotka tarvitsevat erittäin suuri asteikko. Suuri asteikko vaatimukset voit täytettävä vaakasuunnassa laajentaminen sovellukset-useita sovellusten käyttöönoton tehdään yhden alueen ja eri alueilla, käsittelemään erittäin kuormituksen vaatimusten kanssa.

Sovelluksen palvelun ympäristöissä on ihanteellinen ympäristössä vaaka-akselille.  Kerran sovelluksen palvelun ympäristön määritys on valittu, jotka tukevat tunnetut pyynnön korvaus-kehittäjät voivat ottaa käyttöön sovelluksen palvelun ympäristöjen "evästeiden leikkurin" tavalla saavuttaa haluamasi piikin kuormituksen kapasiteetti.

Oletetaan esimerkiksi sovelluksen App palvelun ympäristön määritys käytössä testattu 20K pyyntöjen sekunnissa (RPS).  Jos haluamasi piikin kuormituksen kapasiteetti on 100K RPS, sitten viisi (5)-sovelluksen palvelun ympäristöissä voi luoda ja määritetty varmistamiseksi sovellus voi käsitellä suurin arvioidut kuormitus.

Koska asiakkaiden access yleensä sovelluksia mukautettu (tai vanity)-toimialue, kehittäjät on tapa jakaa kaikki esiintymät App palvelun ympäristön sovelluspyyntöjen.  Voit tehdä tämän on ratkaisemiseksi mukautetun toimialueen käyttäminen aiemmin [Azure liikenteen hallinta profiilin][AzureTrafficManagerProfile].  Liikenteen hallinta profiilin voi määrittää osoittavan lainkaan yksittäisiä App Service-ympäristössä.  Liikenteen hallinta automaattisesti käsittelee jakelutavoista asiakkaiden kaikissa sovelluksen palvelun ympäristöjen liikenteen hallinta-profiilin asetukset kuormituksen perusteella.  Tämä menetelmä toimii riippumatta siitä, onko kaikki sovelluksen palvelun ympäristöissä käyttöön vain yhden Azure alueen tai otetaan käyttöön maailmanlaajuisesti Azure alueille.

Lisäksi asiakkaat access-sovelluksia vanity toimialueen kautta, koska asiakkaat eivät tiedä App palvelun ympäristöissä käynnissä sovelluksen määrän.  Tuloksena kehittäjät voivat nopeasti ja helposti lisätä ja poistaa sovelluksen palvelun ympäristöissä havaittujen liikenne kuormituksen perusteella.

Alla olevassa Käsitekaavio esittää sovelluksen skaalata vaakasuunnassa pois kolme App palvelun ympäristöissä yhden alueen yli.

![Monta-arkkitehtuuri][ConceptualArchitecture] 

Loput tässä artikkelissa käydään läpi liittyvistä hajautettu topologian otoksen sovelluksen käyttämällä useita sovelluksen palvelun ympäristöissä määrittäminen vaiheet.

## <a name="planning-the-topology"></a>Topologian suunnittelu ##
Rakennuksen hajautettu app vaatiman-tallennustilan ulos, ennen kuin se auttaa on joitakin laitteita tiedot etuajassa.

- **Mukautetun toimialueen sovelluksen:**  Mikä on asiakkaiden käyttää sovelluksen mukautettua toimialuenimeä?  Esimerkki sovelluksen mukautettu toimialuenimi on *www.scalableasedemo.com*
- **Liikenteen hallinta toimialueen:**  Toimialuenimi on valittava luotaessa käytettävän [Azure liikenteen hallinta profiilin][AzureTrafficManagerProfile].  Tämä nimi yhdistetään *trafficmanager.net* liitteen rekisteröidä toimialueen tapahtuman, jota hallitaan liikenteen hallinta.  Esimerkki-sovelluksen valinnut nimi on *skaalattava ietokannan-esittely*.  Toimialuenimi, jota hallitaan liikenteen hallinta tuloksena on *skaalattava demo.trafficmanager.net kuin ietokannan*.
- **Strategia skaalauksen app vaatiman tallennustilan:**  Jaetaan sovelluksen vaatiman tallennustilan useita sovelluksen palvelun ympäristöissä yhden alueen yli?  Useiden alueiden?  Mix-ja-VASTINE kummassakin?  Päätös olisi perusteella, jossa asiakkaan liikenne säilyttävät sekä miten hyvin muiden sovelluksen tukemalla taustatietokantaan infrastruktuurin skaalata odotuksia.  Esimerkiksi 100 %: n tilattomien-sovellukseen, sovelluksen erittäin skaalata käyttämällä useita sovelluksen palvelun ympäristöissä yhdistelmää Azure alueittain kerrottuna App palvelun ympäristöissä käyttöön useita Azure alueiden välillä.  15 + julkisen Azure alueet käytettävissä valittavana asiakkaiden todella muodostaa maailmanlaajuisesti hyper-akseli sovelluksen vaatiman-tallennustilan.  Käytetään tämän artikkelin Esimerkki sovelluksen kolme App palvelun ympäristöissä on luotu Azure alueen (Etelä keskitetyn US).
- **Nimeämiskäytäntö App Service-ympäristössä:**  Kunkin sovelluksen Service-ympäristön edellyttää yksilöivä nimi.  Yksi tai kaksi App palvelun ympäristöissä lisäksi on hyödyllistä nimeämiskäytäntöä voit tunnistaa kunkin sovelluksen Service-ympäristössä.  Esimerkki sovelluksen käytettiin yksinkertainen nimeämiskäytäntöä.  Kolme App Service-ympäristöissä nimet ovat *fe1ase*, *fe2ase*ja *fe3ase*.
- **Nimeämiskäytäntö sovellusten:**  Koska useita kertoja sovellus otetaan käyttöön, jokaiselle esiintymälle käyttöön sovelluksen tarvita nimi.  Yksi sovellus palvelun ympäristöissä vähän tunnetut mutta korjaustoimintoa ominaisuus on yli useita sovelluksen palvelun ympäristöissä voi käyttää sovelluksen nimi.  Koska kunkin sovelluksen Service-ympäristö on yksilöllinen jälkiliitteen, kehittäjät voit käyttää uudelleen jokaisen ympäristössä tarkka sovelluksen nimeä.  Kehittäjä voi olla esimerkiksi sovellusten nimeltä seuraavasti: *myapp.foo1.p.azurewebsites.net*, *myapp.foo2.p.azurewebsites.net*, *myapp.foo3.p.azurewebsites.net*jne.  Esimerkki sovelluksen vaikka sovelluksen jokaiselle esiintymälle on myös yksilöllinen nimi.  Käyttää sovelluksen esiintymää nimet ovat *webfrontend1*, *webfrontend2*ja *webfrontend3*.


## <a name="setting-up-the-traffic-manager-profile"></a>Liikenteen hallinta profiilin määrittäminen ##
Kun sovellus useita kertoja on otettu käyttöön useita sovelluksen Service-ympäristössä, yksittäisiä sovelluksen esiintymät voidaan rekisteröidä liikenteen hallinta.  Esimerkki sovelluksen liikenteen hallinta profiilin tarvitaan *skaalattava demo.trafficmanager.net kuin ietokannan* , joka reitittää asiakkaille johonkin seuraavista käyttöön sovelluksen esiintymät:

- **webfrontend1.fe1ase.p.azurewebsites.net:**  Esimerkki sovelluksen käyttöön ensimmäisen sovelluksen Service-ympäristön esiintymä.
- **webfrontend2.fe2ase.p.azurewebsites.net:**  Esimerkki sovelluksen käyttöön toisen sovelluksen Service-ympäristön esiintymä.
- **webfrontend3.fe3ase.p.azurewebsites.net:**  Esimerkki sovelluksen käyttöön kolmannen sovelluksen Service-ympäristön esiintymä.

Helpoin tapa rekisteröidä useita Azure App palvelun päätepisteet, kaikki käynnissä **sama** Azure alue on PowerShellin [Azure resurssien hallinnan liikenteen hallinta tukevat][ARMTrafficManager].  

Ensimmäiseksi on luotava Azure liikenteen hallinta-profiiliin.  Seuraava koodi näkyy, kuinka profiilin on luotu otoksen sovelluksen:

    $profile = New-AzureTrafficManagerProfile –Name scalableasedemo -ResourceGroupName yourRGNameHere -TrafficRoutingMethod Weighted -RelativeDnsName scalable-ase-demo -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"

Huomaa, miten *RelativeDnsName* -parametrin arvoksi määritetty *skaalattava ietokannan-esittely*.  Tämä on miten toimialueen nimeä *skaalattava demo.trafficmanager.net kuin ietokannan* luodaan ja liittyvän liikenteen hallinta profiilin.

*TrafficRoutingMethod* -parametri määrittää käytännön liikenteen hallinta käytetään määritettäessä levittämällä asiakkaan kuormituksen kaikissa käytettävissä päätepisteet kuormituksen.  Tässä esimerkissä *painotettu* valittiin menetelmää.  Tämä aiheuttaa asiakkaan pyynnöt parhaillaan levitä kaikissa rekisteröidyn sovelluksen päätepisteet liittyvät päätepisteiden suhteellinen leveydet perusteella. 

Käyttämällä profiilia, joka on luotu sovelluksen jokaiselle esiintymälle on lisätä profiiliin alkuperäisen Azure päätepiste.  Seuraava koodi hakee viittauksen kunkin edusta-web-sovelluksen ja lisää sitten kunkin sovelluksen liikenteen hallinta-päätepisteen *TargetResourceId* -parametrin kautta.


    $webapp1 = Get-AzureRMWebApp -Name webfrontend1
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend1 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp1.Id –EndpointStatus Enabled –Weight 10

    $webapp2 = Get-AzureRMWebApp -Name webfrontend2
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend2 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp2.Id –EndpointStatus Enabled –Weight 10

    $webapp3 = Get-AzureRMWebApp -Name webfrontend3
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend3 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp3.Id –EndpointStatus Enabled –Weight 10
    
    Set-AzureTrafficManagerProfile –TrafficManagerProfile $profile
    
Huomaa, miten on kutsu *Lisää AzureTrafficManagerEndpointConfig* yksittäisiä sovelluksen jokaiselle esiintymälle.  Kunkin Powershell-komennon *TargetResourceId* -parametri viittaa jokin kolmesta käyttöön sovelluksen esiintymät.  Liikenteen hallinta profiilin levitä Lataa kaikki kolme päätepisteet rekisteröity profiilissa yli.

Kaikki kolme päätepisteet käyttää *leveys* -parametri on sama arvo (10).  Tämä aiheuttaa liikenteen hallinta leviämisen asiakkaan pyynnöt kaikki kolme sovelluksen esiintymät koko suhteellisen tasaisesti. 


## <a name="pointing-the-apps-custom-domain-at-the-traffic-manager-domain"></a>Valitsemalla sovelluksen mukautetun toimialueen toimialueen liikenteen hallinta ##
Tarvittavat viimeinen vaihe on osoittavan mukautetun toimialueen sovelluksen osoitteessa liikenteen hallinta-toimialue.  Esimerkki sovelluksen siis *www.scalableasedemo.com* osoittamalla *skaalattava demo.trafficmanager.net kuin ietokannan*.  Tämä vaihe on valmis toimialuerekisteröijän, joka hallitsee mukautettua toimialuetta.  

Käytä käyttämäsi Rekisteröintipalvelun toimialueen hallintatyökalut, CNAME tietueet on luotu, joka osoittaa toimialueen liikenteen hallinta mukautetun toimialueen.  Alla olevassa kuvassa on esimerkki CNAME Tässä määrityksessä näyttää seuraavalta:

![Mukautetun toimialueen CNAME][CNAMEforCustomDomain] 

Vaikka ei koske tämän ohjeaiheen, muista, että yksittäisiä sovelluksen jokaiselle esiintymälle on oltava rekisteröity sitä sekä mukautetun toimialueen.  Muuten jos pyyntö on sovelluksen esiintymään, sovellus ei ole rekisteröity-sovelluksen mukautetun toimialueen kutsu epäonnistuu.  

Tässä esimerkissä mukautettu toimialue on *www.scalableasedemo.com*ja sovelluksen jokaiselle esiintymälle on liitetty mukautettua toimialuetta.

![Mukautettua toimialuetta][CustomDomain] 

Artikkelissa seuraavassa artikkelissa rekisteröinnistä [Mukautetut toimialueet]recap rekisteröinnin mukautetun toimialueen Azure App palvelun sovelluksilla,[RegisterCustomDomain].

## <a name="trying-out-the-distributed-topology"></a>Jaettu-topologian kokeilla ##
Liikenteen hallinta ja DNS-määritys lopputulos on se, että pyyntö *www.scalableasedemo.com* juoksuttaa kautta seuraavassa järjestyksessä:

1. Selaimessa tai laitteen tekee *www.scalableasedemo.com* DNS hakukentän
2. CNAME-merkinnän toimialueen rekisteröintipalvelussa aiheuttaa ohjataan Azure liikenteen hallinta DNS-haku.
3. DNS-haku tehdään *skaalattava demo.trafficmanager.net kuin ietokannan* vastaan Azure liikenteen hallinta DNS-palvelimia.
4. Perusteella (käytetään aiemmin luotaessa liikenteen hallinta profiilin *TrafficRoutingMethod* -parametri) käytännön kuormituksen liikenteen hallinta valitse jotakin määritetyn päätepisteistä ja palauttaa kyseisen päätepisteen FQDN selaimessa tai laitteessa.
5.  Päätepisteen FQDN on erillisen app-sovelluksen Service-ympäristössä käytössä URL-osoite, selaimen tai laitteen Pyydä ratkaisemiseksi FQDN IP-osoite Microsoft Azure DNS-palvelimeen. 
6. Selaimessa tai laitteessa lähettää HTTP/su-pyynnön IP-osoite.  
7. Pyyntö vuoroon jokin jokin sovellus Service-ympäristössä käytössä sovelluksen esiintymät.

Konsolin alla olevassa kuvassa otoksen app ratkaiseminen on käytössä jokin kolmesta otoksen App palvelun ympäristöissä (Tällöin toinen kolme App palvelun ympäristöjen) sovelluksen esiintymään mukautetun toimialueen DNS-haku:

![DNS-haku][DNSLookup] 

## <a name="additional-links-and-information"></a>Muita linkkejä ja tiedot ##
Kaikki artikkelit ja miten-, Muokkaa sovelluksen palvelun ympäristöissä ovat käytettävissä [sovelluksen palvelun ympäristöissä Lueminut-tiedosto](../app-service/app-service-app-service-environments-readme.md).

Ohjeet PowerShellin [Azure resurssien hallinnan liikenteen hallinta tukevat][ARMTrafficManager].  

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[AzureTrafficManagerProfile]:  https://azure.microsoft.com/documentation/articles/traffic-manager-manage-profiles/
[ARMTrafficManager]:  https://azure.microsoft.com/documentation/articles/traffic-manager-powershell-arm/
[RegisterCustomDomain]:  https://azure.microsoft.com/en-us/documentation/articles/web-sites-custom-domain-name/


<!-- IMAGES -->
[ConceptualArchitecture]: ./media/app-service-app-service-environment-geo-distributed-scale/ConceptualArchitecture-1.png
[CNAMEforCustomDomain]:  ./media/app-service-app-service-environment-geo-distributed-scale/CNAMECustomDomain-1.png
[DNSLookup]:  ./media/app-service-app-service-environment-geo-distributed-scale/DNSLookup-1.png
[CustomDomain]:  ./media/app-service-app-service-environment-geo-distributed-scale/CustomDomain-1.png 

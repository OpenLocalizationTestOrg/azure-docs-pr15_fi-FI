<properties
   pageTitle="Sovelluksen yhdyskäytävän virheellinen yhdyskäytävä (502) vianmääritys | Microsoft Azure"
   description="Opettele sovelluksen yhdyskäytävän 502 vianmääritys"
   services="application-gateway"
   documentationCenter="na"
   authors="amitsriva"
   manager="rossort"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/02/2016"
   ms.author="amitsriva" />

# <a name="troubleshooting-bad-gateway-errors-in-application-gateway"></a>Virheellinen yhdyskäytävä vianmääritys sovelluksen yhdyskäytävän

## <a name="overview"></a>Yleiskatsaus

Kun olet määrittäminen Azure-sovelluksen yhdyskäytävän jokin virheitä, jotka käyttäjät voivat kohdata on "palvelinvirhe: 502 – verkkopalvelin vastaanotti virheellisen vastauksen aikana visuaalisessa gateway tai välityspalvelimen palvelimeksi". Tämä voi johtua seuraavista tärkeimmät seuraavista syistä:

- Azure sovelluksen-yhdyskäytävän taustatietokantaan resurssivarantoon ei ole määritetty tai tyhjä.
- Kaikki VMs tai esiintymät AM asteikko joukko ovat kunnossa.
- Taustatietokannan VMs tai esiintymät AM asteikko määrittäminen ei vastaa oletusarvon kunto näytteenottimen.
- Mukautetun kunto keräysputkien virheellinen tai virheellisestä määrittäminen.
- Pyydä aikakatkaisu tai yhteysongelmat pyynnöt.

## <a name="empty-backendaddresspool"></a>Tyhjä BackendAddressPool

### <a name="cause"></a>Syy

Jos sovelluksen yhdyskäytävä ei ole VMs tai AM asteikko määrittäminen käyttävät taustatietokantaa osoite varannon, sitä ei voi reitittää asiakkaan pyyntö ja ilmoittaa – virheellinen yhdyskäytävä-virhe.

### <a name="solution"></a>Ratkaisu

Varmista, että taustatietokantaa osoite resurssivarantoon ei ole tyhjä. Voit tehdä tämän joko PowerShell, CLI tai portal kautta.

    Get-AzureRmApplicationGateway -Name "SampleGateway" -ResourceGroupName "ExampleResourceGroup"

Edellisen cmdlet-komennon tulosteen pitäisi olla tyhjä taustatietokantaan osoite resurssivarantoon. Seuraavassa on esimerkki, jossa kaksi jakavat palautetaan joka on määritetty Taustajärjestelmä VMs Toimialuenimeä tai IP-osoitteet. BackendAddressPool valmistelu tila on on "onnistui".
    
    BackendAddressPoolsText: 
            [{
                "BackendAddresses": [{
                    "ipAddress": "10.0.0.10",
                    "ipAddress": "10.0.0.11"
                }],
                "BackendIpConfigurations": [],
                "ProvisioningState": "Succeeded",
                "Name": "Pool01",
                "Etag": "W/\"00000000-0000-0000-0000-000000000000\"",
                "Id": "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name>/backendAddressPools/pool01"
            }, {
                "BackendAddresses": [{
                    "Fqdn": "xyx.cloudapp.net",
                    "Fqdn": "abc.cloudapp.net"
                }],
                "BackendIpConfigurations": [],
                "ProvisioningState": "Succeeded",
                "Name": "Pool02",
                "Etag": "W/\"00000000-0000-0000-0000-000000000000\"",
                "Id": "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name>/backendAddressPools/pool02"
            }]


## <a name="unhealthy-instances-in-backendaddresspool"></a>Valitse BackendAddressPool perusasemassa esiintymiä

### <a name="cause"></a>Syy

Jos perusasemassa BackendAddressPool kaikki esiintymät, valitse sovelluksen yhdyskäytävä ei ole reitin käyttäjäpyyntö, minkä tahansa taustatietokannan. Palvelupyynnön voi myös taustatietokantaan esiintymät on kunnossa, mutta se ei ole otettu käyttöön tarvittavaa sovellusta.

### <a name="solution"></a>Ratkaisu

Varmista, että esiintymät ovat kunnossa sovellus on määritetty oikein. Tarkista, jos taustatietokantaan esiintymät voivat vastata ping-pyyntö toiseen saman VNet-AM. Jos määritetty julkisen lopuksi, varmista, että web-sovelluksen selaimen pyyntö on kalibroinnista.

## <a name="problems-with-default-health-probe"></a>Oletusarvoinen kunto näytteenottimen liittyviä ongelmia

### <a name="cause"></a>Syy

502 virheet on myös oletus kunto näytteenottimen ei pysty saavuttaa taustatietokantaan VMs usein käytetyt ilmaisimet. Sovelluksen yhdyskäytävän esiintymää on valmisteltu, kun se määrittää oletusarvon kunto-näytteenottimen kunkin BackendAddressPool BackendHttpSetting ominaisuuksien avulla voit automaattisesti. Voit määrittää tämän näytteenottimen tarvitaan käyttäjän syötteitä. Tarkemmin sanottuna kuormituksen sääntö on määritetty, kun tiedonsiirtosuhteen soitetaan BackendHttpSetting ja BackendAddressPool välillä. Oletusarvo-näytteenotin on määritetty kunkin nämä yhteydet ja sovelluksen yhdyskäytävän valmistelee on määritetty BackendHttpSetting osaan portti BackendAddressPool jokaiselle esiintymälle säännöllisiä kuntotietojen tarkistus yhteyden. Seuraavassa taulukossa on lueteltu oletusarvon kunto näytteenottimen liittyvät arvot.


|Tutkia ominaisuus | Arvo | Kuvaus|
|---|---|---|
| Tutkia URL-osoite| http://127.0.0.1/ | URL-polku |
| Väli | 30 | Aikavälin tutkia sekunnin kuluttua |
| Aikakatkaisu  | 30 | Tutkia aikakatkaisu sekunnin kuluttua |
| Perusasemassa kynnysarvo | 3 | Tutkia Laske uudelleen. Taustatietokannan palvelimen on merkitty, kun peräkkäiset näytteenottimen virheen Laske saavuttaa perusasemassa raja-arvon. |

### <a name="solution"></a>Ratkaisu

- Varmista, että oletussivusto on määritetty ja seuraa kohteessa 127.0.0.1.
- Jos BackendHttpSetting määrittää portti kuin 80, oletussivuston on määritettävä kuunnella, että Port (portti)-palvelussa.
- Http://127.0.0.1:port kutsu tulee palauttaa 200 HTTP-tuloksen koodi. Tämä on palautettava 30 sekuntia aikakatkaisuajan kuluessa.
- Varmista, että määritetyt on avoinna ja että ei ole palomuurisäännöt tai Azure verkon käyttöoikeusryhmät, joka estää saapuvan ja lähtevän tietoliikenteen käyttävät porttia.
- Jos Azure VMs tai pilvipalvelussa käytetään FQDN tai julkiseen IP-perinteinen varmistaa, että sitä vastaava [päätepisteen](../virtual-machines/virtual-machines-windows-classic-setup-endpoints.md) avataan.
- Jos AM on määritetty kautta Azure Resurssienhallinta ja ulkopuolella VNet, jossa sovellus yhdyskäytävä on otettu käyttöön, [Verkko-käyttöoikeusryhmän](../virtual-network/virtual-networks-nsg.md) on määritetty sallimaan haluamasi portti käyttöön.


## <a name="problems-with-custom-health-probe"></a>Mukautetun kunto näytteenottimen liittyviä ongelmia

### <a name="cause"></a>Syy

Mukautetun kunto keräysputkien Salli tutkitaan toiminta oletusarvoiseen joustavasti. Mukautetun keräysputkien käytettäessä käyttäjät voivat määrittää näytteenottimen aikaväli, URL-Osoitteen ja testaa polku ja kuinka monta epäonnistui vastaukset hyväksyä, ennen kuin taustatietokantaan resurssivarantoon esiintymän merkitään virheelliseksi merkitsemisen. Seuraavat lisäominaisuudet lisätään.

|Tutkia ominaisuus| Kuvaus|
|---|---|
| Nimi | Näytteenottimen nimi. Tätä nimeä käytetään viittaamaan keräysputken taustatietokantaan HTTP asetuksissa. |
| Protokolla | Lähetä näytteenottimen protokolla. Näytteenottimen käyttävät taustatietokantaa HTTP-asetuksissa määritetyn mukaisesti protokolla |
| Host (isäntä) |  Lähetä näytteenottimen isäntänimi. Käytettävissä vain, kun usean sivuston on määritetty sovelluksen yhdyskäytävässä. Tämä on sama kuin AM isäntänimi.  |
| Polku | Suhteellinen polku näytteenottimen. Virheellinen polun alkaa "/". Näytteenottimen lähetetään \<protokolla\>://\<host\>:\<portin\>\<polku\> |
| Väli | Tutkia välin sekunteina. Tämä on kaksi peräkkäistä keräysputkien aikavälin.|
| Aikakatkaisu | Tutkia aikakatkaisun sekunteina. Näytteenottimen on merkitty ei onnistunut, jos vastausta ei vastaanoteta tämän aikakatkaisuajan kuluessa. |
| Perusasemassa kynnysarvo | Tutkia Laske uudelleen. Taustatietokannan palvelimen on merkitty, kun peräkkäiset näytteenottimen virheen Laske saavuttaa perusasemassa raja-arvon. |


### <a name="solution"></a>Ratkaisu

Vahvista, että mukautettu kunto tutkia on määritetty oikein kuin edellisessä taulukossa. Edellisen vianmääritysohjeita on lisäksi myös toimittava seuraavasti:

- Varmista, että näytteenottimen on määritetty oikein [opas](application-gateway-create-probe-ps.md)mukaan.
- Jos sovelluksen yhdyskäytävä on määritetty yksittäisen sivuston, oletusarvoisesti isännän nimi on määritettävä "127.0.0.1", ellei muussa määritetty mukautettuja näytteenottimen.
- Varmistaa, että http:// puhelun\<host\>:\<portin\>\<polku\> palauttaa 200 HTTP-tuloksen koodi.
- Varmistaa, että aikaväli, aikakatkaisu ja UnhealtyThreshold hyväksyttävät alueiden sisällä.

## <a name="request-time-out"></a>Pyynnön aikakatkaisu

### <a name="cause"></a>Syy

Pyytäminen saapuessa sovelluksen yhdyskäytävä koskee määritetyn säännöt pyynnön ja reitittää taustatietokantaan resurssivarantoon esiintymään. Odottaa määritettäviä aikaväli taustatietokantaan esiintymän vastausta. Oletusarvoisesti tämä väli on **30 sekuntia**. Jos sovelluksen yhdyskäytävän saa vastausta taustatietokantaan sovelluksesta aikaväliä, pyyntö nähdä 502 virhe.

### <a name="solution"></a>Ratkaisu

Sovelluksen yhdyskäytävän avulla käyttäjät voivat määrittää tämän asetuksen BackendHttpSetting, joita voidaan sitten käyttää eri jakavat kautta. Eri taustatietokantaan jakavat voi olla eri BackendHttpSetting ja näin ollen eri pyynnön aikakatkaisu määritetty.

    New-AzureRmApplicationGatewayBackendHttpSettings -Name 'Setting01' -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 60

## <a name="next-steps"></a>Seuraavat vaiheet

Jos edellä kuvatut toimet eivät ratkaise ongelmaa, Avaa [tukevat lippu](https://azure.microsoft.com/support/options/).

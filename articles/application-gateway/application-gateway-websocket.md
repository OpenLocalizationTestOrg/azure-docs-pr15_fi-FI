<properties
   pageTitle="Sovellusten yhdyskäytävän WebSocket tuki | Microsoft Azure"
   description="Tällä sivulla on yleiskuvaus sovelluksen yhdyskäytävän WebSocket tuki."
   documentationCenter="na"
   services="application-gateway"
   authors="amsriva"
   manager="rossort"
   editor="amsriva"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/16/2016"
   ms.author="amsriva"/>

# <a name="application-gateway-websocket-support"></a>Sovellusten yhdyskäytävän WebSocket tuki

Sovelluksen yhdyskäytävä on tuki WebSocket yli yhdyskäytävän koosta riippumatta. Ei ole käyttäjän määrittämät asetusta valikoivasti käyttöön ottaminen tai käytöstä WebSocket tuki. Voit jatkaa käyttämällä vakio HTTPListener portti edelleen vastaanottaa WebSocket liikenne 80 ja 443. WebSocket liikenne ohjataan sitten WebSocket käytössä palvelimeen käyttämällä tarvittavat Taustajärjestelmä resurssivarantoon sovelluksen yhdyskäytävän sääntöjen mukaisesti. Standardoitu [RFC6455](https://tools.ietf.org/html/rfc6455) WebSocket-protokollan avulla kaksisuuntainen viestintä asiakkaan ja palvelimen välillä pitkään käynnissä TCP-yhteyden kautta. Tämän ominaisuuden avulla vuorovaikutusta viestintään verkkopalvelin ja asiakkaan, joka voi olla kaksisuuntainen, ilman kysely nimellä edellytetään HTTP-pohjaista käyttöotot edellyttää välillä.  WebSocket on pieni yleisrasite toisin kuin HTTP ja voit käyttää useita pyyntö/vastauksia tuloksena on tehokkaampaa käyttö resurssien saman TCP-yhteyden. WebSocket protokollat on suunniteltu toimimaan perinteinen HTTP-portit 80 ja 443 päälle.

Yhteyttä palvelimeen on vastattava sovelluksen yhdyskäytävän keräysputkien, joka on kuvattu [kunto näytteenottimen yhteenveto](application-gateway-probe-overview.md) -osassa. Sovelluksen yhdyskäytävän kunto keräysputkien ovat vain HTTP/HTTPS-Tämä tarkoittaa sitä, että jokaisen palvelimeen on vastattava HTTP keräysputkien sovelluksen Gatewayn ja haluat reitittää liikenteen WebSocket palvelimeen.

## <a name="listener-configuration-element"></a>Listener määritys-elementti

Olemassa olevan HTTPListener voidaan tukemaan WebSocket. Seuraavassa on katkelma otoksen mallitiedostosta HttpListeners elementti. Tarvitset tukea WebSocket ja WebSocket tietoliikenteen HTTP- ja HTTPS kuuntelijoita. Samalla tavalla kuin voit käyttää [portal](application-gateway-create-gateway-portal.md) tai [PowerShell](application-gateway-create-gateway-arm.md) luomalla sovelluksen yhdyskäytävän kuuntelijoita portti edelleen tukemaan WebSocket liikenne 80 ja 443.


    "httpListeners": [
                {
                    "name": "appGatewayHttpsListener",
                    "properties": {
                        "FrontendIPConfiguration": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendIPConfigurations/DefaultFrontendPublicIP"
                        },
                        "FrontendPort": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendPorts/appGatewayFrontendPort443'"
                        },
                        "Protocol": "Https",
                        "SslCertificate": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/sslCertificates/appGatewaySslCert1'"
                        },
                    }
                },
                {
                    "name": "appGatewayHttpListener",
                    "properties": {
                        "FrontendIPConfiguration": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendIPConfigurations/appGatewayFrontendIP'"
                        },
                        "FrontendPort": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendPorts/appGatewayFrontendPort80'"
                        },
                        "Protocol": "Http",
                    }
                }
            ],

## <a name="backendaddresspool-backendhttpsetting-and-routing-rule-configuration"></a>Säännön BackendAddressPool, BackendHttpSetting ja reititys-määritys

BackendAddressPool olisi voidaan määrittää Taustajärjestelmä resurssivarantoon WebSocket käytössä-palvelimiin. BackendHttpSetting yritysnumerokentissä Taustajärjestelmä portti 80 ja 443 vain. Evästeiden perustuva affiniteetti ja requestTimeouts ominaisuudet eivät koske WebSocket liikenne. Ei muutoksia ei tarvita reititys säännön. "Perustiedot" Jatka varten tarvittavat listener vastaavan Taustajärjestelmä osoite resurssivarantoon sitominen reititystietoja sääntö. 

    "requestRoutingRules": [{
        "name": "<ruleName1>",
        "properties": {
            "RuleType": "Basic",
            "httpListener": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/httpListeners/appGatewayHttpsListener')]"
            },
            "backendAddressPool": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendAddressPools/ContosoServerPool')]"
            },
            "backendHttpSettings": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
            }
        }

    }, {
        "name": "<ruleName2>",
        "properties": {
            "RuleType": "Basic",
            "httpListener": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/httpListeners/appGatewayHttpListener')]"
            },
            "backendAddressPool": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendAddressPools/ContosoServerPool')]"
            },
            "backendHttpSettings": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
            }

        }
    }]

## <a name="websocket-enabled-backend"></a>WebSocket käytössä taustaan

Oman Taustajärjestelmä on oltava käynnissä määritetty HTTP/HTTPS verkkopalvelimen portti (yleensä 80 ja 443) WebSocket toimimaan varten. Tämä vaatimus koskee, koska WebSocket-protokolla edellyttää alkuperäisen kättely on HTTP with päivityksen WebSocket protokolla otsikko-kenttänä.

    GET /chat HTTP/1.1
    Host: server.example.com
    Upgrade: websocket
    Connection: Upgrade
    Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
    Origin: http://example.com
    Sec-WebSocket-Protocol: chat, superchat
    Sec-WebSocket-Version: 13

Tämän ominaisuuden voivat johtua siitä, että sovelluksen yhdyskäytävän Taustajärjestelmä kunto näytteenottimen tukee vain HTTP/HTTPS-protokollaa. Jos yhteyttä palvelimeen ei vastaa HTTP/HTTPS keräysputkien, se otettava ulos Taustajärjestelmä resurssivaranto ja ei ole pyynnöt myös WebSocket, saavuttaa tämän Taustajärjestelmä.

## <a name="next-steps"></a>Seuraavat vaiheet

Kun WebSocket tukeen liittyviä, siirry [sovelluksen Gatewayn luominen](application-gateway-create-gateway.md) WebSocket käytössä web-sovelluksen käytön aloittaminen.

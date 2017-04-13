<properties
    pageTitle="Mikä on Azure välitys? | Microsoft Azure"
    description="Yleistä Azure välitys"
    services="service-bus"
    documentationCenter=".net"
    authors="banisadr"
    manager="timlt"
    editor="" />

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/28/2016"
    ms.author="babanisa" />

# <a name="what-is-azure-relay"></a>Mikä on Azure välitys?

Azure välityspalvelu helpottaa hybrid-sovellusten avulla voit näyttää suojatusti palvelut, jotka sijaitsevat yrityksen yritysverkkoa julkisen pilveen avaamatta palomuuri-yhteyden tai edellyttävät yrityksen verkkoinfrastruktuuria tunkeutuva muutokset. Välitys tukee useita eri protokollia ja web services standardeja.

Välityspalveluun tukee perinteinen yksisuuntainen-pyynnön ja vastauksen ja vertaisääni-liikenne. Se tukee myös internet-alueen käyttöön Julkaise ja tilaa skenaariot ja kaksisuuntaisen socket viestintä parantavat pisteestä tehokkuuden tapahtuma-jakauman. 

Rakenteessa välittäminen tietojen siirto paikallisen-palvelun muodostaa yhteyden välityspalvelu lähtevän portin kautta ja luo kaksisuuntaisen-socket viestintään, jotka on liitetty tiettyyn rendezvous sähköpostiosoitteeseen. Asiakkaan sitten viestiä paikallisen-palvelun kanssa lähettämällä liikenne välityspalveluun kohdistamisen rendezvous-osoite. Välityspalveluun sitten "välittää" tietoja läpi kunkin asiakkaan omistautunut kaksisuuntaisen-socket paikallisen-palveluun. Asiakas ei tarvita paikallisen palvelun suoran yhteyden, se ei tarvitse tietää, johon palvelu sijaitsee, ja paikallisen palvelun ei tarvitse mitään saapuvien porttien Avaa palomuurin.

Välitys myöntämä avaimen ominaisuuksien osat ovat kaksisuuntaisen, puskuroimatonta viestintä verkon rajojen TCP kaltaisessa rajoitusta, päätepisteen etsiminen, tilan ja päällekkäin päätepisteen suojaus. Välitys käyttäjän ominaisuudet eroavat verkon tason integrointi tekniikoita, kuten VPN, että se voi olla suodatetut yhden sovelluksen-päätepisteen yhteen tietokoneeseen, VPN-tekniikka ollessa paljon enemmän tunkeutuva, kun se on riippuvainen muuttamista Verkkoympäristössä.

Azure välitys sisältää kaksi toimintoja:

1. [Hybrid yhteydet](#hybrid-connections) - käyttää Avaa Vakio Web Sockets ottaminen käyttöön useita skenaariot

2. [WCF-releitä](#wcf-relays) - käyttää Windows Communication Foundation käyttöön Remote käsitteellisiä puhelut

Hybrid yhteydet ja WCF releitä käyttöön assests siellä olevia yrityksen yrityksen verkossa oleville suojattua yhteyttä. Päällekkäiseksi käyttö on riippuvainen alla mainitut tietyn tarpeitasi:

|                                    | WCF-välitys | Hybrid yhteydet |
| ---------------------------------- |:---------:|:------------------:|
| **WCF**                            |     x     |                    |
| **.NET core**                      |           |         x          |
| **.NET framework**                 |     x     |         x          |
| **JavaScript/NodeJS**              |           |         x          |
| **Java***                          |           |         x          |
| **Standardit perustuva Avaa protokolla**  |           |         x          |
| **Useita RPC Programing mallit** |           |         x          |
*<sub>Yleiset tavoitettavuuden mukaan</sub>

## <a name="hybrid-connections"></a>Hybrid yhteydet

Azure välitys Hybrid yhteydet-ominaisuus on suojattu, Avaa-protokollan kehitys aiemmin välitys-ominaisuudet, joita voidaan toteuttaa missä tahansa ympäristössä ja kielellä, joka on basic WebSocket-ominaisuus, joka sisältää erikseen WebSocket Ohjelmointirajapinnan yhteiset selaimet. HTTP- ja WebSockets perusteella Hybrid yhteydet.

## <a name="wcf-relays"></a>WCF-releitä

WCF-välitys toimii, koko .NET Framework (NETFX) ja WCF. Muodostat yhteyden paikallinen-palvelun ja käyttämällä WCF "välitys" sidontojen kuuluu välityspalvelu välillä. Taustalla välitys sidontojen Yhdistä uudet transport sidonta elementit suunniteltu WCF kanava-komponentteja, jotka integroida palvelun Bus pilveen.

## <a name="service-history"></a>Palvelun historia

Hybrid yhteydet koulittujen että ensiksi nimeltä "BizTalk Services"-ominaisuutta, joka on luotu Azure palvelun Bus WCF välitys tasaisesti. Uusi Hybrid yhteydet-ominaisuus täydentää aiemmin WCF välitys ja kaksi palvelun nämä ominaisuudet ovat olemassa rinnakkais lähitulevaisuudessa; välitys-palvelussa hän jakaa yhteistä yhdyskäytävää, mutta ei olisikaan erilaisia.

WCF-välitys on vanha välitys ojentamassa, että monet asiakkaat voivat jo käyttämällä niiden WCF programing mallit.

## <a name="next-steps"></a>Seuraavat vaiheet:

- [Välitys usein kysytyt kysymykset](relay-faq.md)
- [Luo nimitila](relay-create-namespace-portal.md)
- [.NET käytön aloittaminen](relay-hybrid-connections-dotnet-get-started.md)
- [Solmun käytön aloittaminen](relay-hybrid-connections-node-get-started.md)
<properties
    pageTitle="Palvelun Bus välitys yleiskatsaus | Microsoft Azure"
    description="Palvelun Bus välitys yleiskatsaus."
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="get-started-article"
    ms.date="09/01/2016"
    ms.author="sethm"/>


# <a name="overview-of-service-bus-relay"></a>Yleistä palvelun Bus välitys

Palvelun Bus tärkeä osa on keskitetty (mutta erittäin kuormituksen saapuva) *välitys* -palvelu, jonka avulla voit luoda hybrid-sovelluksia, jotka suoritetaan Azure palvelinkeskuksen ja paikallisen oman yritysympäristössä.  Palvelun Bus välitys tukee useita eri protokollia ja web services standardeja. Tämä vaihtoehto sisältää SOAP-WS-*- ja jopa muille käyttäjille. Välityspalveluun helpottaa hybrid-sovellusten avulla voit näyttää turvallisesti Windows Communication Foundation (WCF)-palvelut, jotka sijaitsevat yrityksen yritysverkkoa julkisen pilveen avaamatta palomuuri-yhteyden tai edellyttävät yrityksen verkkoinfrastruktuuria tunkeutuva muutokset. 

![Välitys käsitteitä](./media/service-bus-relay-overview/sb-relay-01.png)

Välityspalveluun tukee perinteinen yksisuuntainen viestintä-pyynnön ja vastauksen viestintä ja peer ja peer messaging. Se tukee myös internet-alueen käyttöön Julkaise ja tilaa skenaariot ja kaksisuuntaisen socket viestintä parantavat pisteestä tehokkuuden tapahtuma-jakauman. 

Relayed tekstiviesti rakenteessa paikallisen-palvelun muodostaa yhteyden välityspalvelu lähtevän portin kautta ja luo kaksisuuntaisen-socket viestintään, jotka on liitetty tiettyyn rendezvous sähköpostiosoitteeseen. Asiakkaan sitten viestiä paikallisen-palvelun kanssa lähettämällä viestejä välityspalveluun kohdistamisen rendezvous-osoite. Välityspalveluun sitten "välittää" viestejä kautta on jo lisätty kaksisuuntaisen-socket paikallisen-palveluun. Asiakas ei tarvita paikallisen palvelun suoran yhteyden, se ei tarvitse tietää, johon palvelu sijaitsee, ja paikallisen palvelun ei tarvitse mitään saapuvien porttien Avaa palomuurin.

Muodostat yhteyden paikallinen-palvelun ja käyttämällä WCF "välitys" sidontojen kuuluu välityspalvelu välillä. Taustalla välitys sidontojen Yhdistä uudet transport sidonta elementit suunniteltu WCF kanava-komponentteja, jotka integroida palvelun Bus pilveen. 

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja palvelun Bus välitys on seuraavissa artikkeleissa.

- [Azure palvelun Bus arkkitehtuuri yleiskatsaus](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)
- [Opi käyttämään palvelua Bus välityspalvelu](service-bus-dotnet-how-to-use-relay.md)

 
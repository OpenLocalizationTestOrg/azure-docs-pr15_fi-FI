<properties
    pageTitle="Palvelun Bus tekstiviesti yleiskatsaus | Microsoft Azure"
    description="Palvelun Bus viestit: joustavia tietojen toimituksen pilveen"
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
    ms.date="09/27/2016"
    ms.author="sethm"/>


# <a name="service-bus-messaging-flexible-data-delivery-in-the-cloud"></a>Palvelun Bus messaging: joustavia tietojen toimituksen pilveen

Microsoft Azure palvelun Bus on luotettavaa tietoa toimituksen palvelu. Tämän palvelun tarkoituksena on helpompaa viestintä. Kahden tai useamman osapuolen haluat vaihtaa tietoja, kun he tarvitsevat viestintä-järjestelmä. Palvelun Bus on se tai kolmannen osapuolen viestintä-järjestelmä. Tämä muistuttaa fyysisesti postal-palveluun. Postin palvelut helpottavat hyvin lähettää erilaisia kirjainten ja paketteja, joissa on erilaisia toimituksen oikeudet, missä tahansa maailmanlaajuisesti.

Samoin kuin postal palvelun toimittaminen kirjaimia, palvelun Bus on joustavia tietoja toimituksesta lähettäjän ja vastaanottajan. Tekstiviesti-palvelun varmistaa tiedot toimitetaan, vaikka kahden osapuolen koskaan sekä verkossa samaan aikaan tai ne ole täsmälleen samalla kertaa. Tällä tavalla viestintä on samanlainen kuin lähettää kirjeen, – se viestintä ollessa asettamisen puhelun (tai puhelun käyttötavoista on - ennen kutsun odottaa ja soittajan tunnus, joka on paljon samoin kuin se messaging).

Viestin lähettäjän edellyttävät lisäksi toimituksen ominaisuudet, mukaan lukien tapahtumia, kaksoiskappaleiden aikapohjaisten vanhentuminen ja jonottaminen erilaisia. Nämä kuvioita on postinumero analogioilla: toista toimituksen, vaadittua allekirjoitusta, osoite muuttaminen tai peruuttaminen.

Palvelun Bus tukee kahta erillistä tekstiviesti kuvioita: *välitys* ja *se messaging*.

## <a name="service-bus-relay"></a>Palvelun Bus välitys

Palvelun Bus [välitys](../service-bus-relay/service-bus-relay-overview.md) -osa on keskitetty (mutta erittäin kuormituksen saapuva) palvelu, joka tukee useita eri protokollia ja Web services standardeja. Tämä vaihtoehto sisältää SOAP-WS-*- ja jopa muille käyttäjille. [Välityspalvelu](../service-bus-relay/service-bus-dotnet-how-to-use-relay.md) useilla eri välitys yhteysasetukset ja auttavat neuvottelemalla vertaisääni-suorat yhteydet, kun se on mahdollista. Palvelun Bus on tarkoitettu .NET kehittäjät, jotka käyttävät Windows Communication Foundation (WCF), sekä suorituskykyä ja käytettävyyttä, ja pääsee sen välityspalvelu välityksellä SOAP-PROTOKOLLAN ja muut liittymät. Näin SOAP-ja muut ohjelmointi ympäristön voi integroida palvelun Bus.

Välityspalveluun tukee perinteinen yksisuuntainen viestintä-pyynnön ja vastauksen viestintä ja peer ja peer messaging. Se tukee myös Internet-alueen käyttöön tapahtuman jakauman Julkaise-tilaa skenaariot ja parantavat pisteestä tehokkuuden kaksisuuntaisen socket viestintä. Relayed tekstiviesti rakenteessa paikallisen-palvelun muodostaa yhteyden välityspalvelu lähtevän portin kautta ja luo kaksisuuntaisen-socket viestintään, jotka on liitetty tiettyyn rendezvous sähköpostiosoitteeseen. Asiakkaan sitten viestiä paikallisen-palvelun kanssa lähettämällä viestejä välityspalveluun kohdistamisen rendezvous-osoite. Välityspalveluun sitten "välittää" viestejä kautta on jo lisätty kaksisuuntaisen-socket paikallisen-palveluun. Asiakas ei tarvita paikallisen palvelun suoran yhteyden eikä pakollinen tietää, johon palvelu sijaitsee, ja paikallisen palvelun ei tarvitse mitään saapuvien porttien Avaa palomuurin.

Käynnistät yhteys paikalliseen-palvelun ja välityspalvelu, välillä käyttämällä WCF "välitys" sidontojen kuuluu. Taustalla välitys sidontojen Yhdistä transport suunniteltu WCF kanava-komponentteja, jotka palvelun Bus pilveen integroida sidonta-osat.

Palvelun Bus välitys on monia etuja, mutta vaatii palvelimen ja asiakkaan sekä olla online-tilassa samanaikaisesti, jotta voit lähettää ja vastaanottaa viestejä. Tämä ei ole optimaalista, HTTP-tyyppiseksi viestintään, jossa pyynnöt eivät välttämättä ole yleensä pitkäikäiset eikä asiakkaiden, joka yhdistää vain toisinaan selaimissa, kuten mobile-sovellukset, ja niin edelleen. Se tukee erilliset viestintä viestintä ja on oma etuja; asiakkaiden ja palvelinten voit muodostaa yhteyden, kun tarvitaan ja suorittaa niiden asynkroninen tavalla.

## <a name="brokered-messaging"></a>Se viestintä

Toisin kuin välitys-värimallin [se viestiliikenne](service-bus-queues-topics-subscriptions.md) voidaan ajatella kuin asynkroninen tai "toimialueeseesi irrotettu." Tuottajat (lähettäjien) ja kuluttajille (vastaanottajia) ei tarvitse olla online-tilassa samanaikaisesti. Viestinnän infrastruktuurin tallentaa luotettavasti viestien "broker" (kuten jonossa), kunnes vievää osapuoli on valmiina vastaanottamaan ne. Näin katkaistaan, joko vapaaehtoisesti; jaetun sovelluksen osat esimerkiksi ylläpito tai osan kaatumisen vuoksi ilman vaikuttavia koko järjestelmään. Lisäksi vastaanottava sovellus voi olla tulossa online tiettyjä aikoina päivän, kuten varasto-hallintajärjestelmään, joita vain tarvitaan Suorita business päivän lopussa.

Palvelun Bus se tekstiviesti infrastruktuuri core osat ovat olevien, aiheet ja tilaukset.  Tärkein ero on ohjeaiheissa tukevat Julkaise ja tilaa ominaisuuksista, joita voi käyttää kehittyneitä sisällön perustuva reitityksen ja toimituksen logiikan, mukaan lukien lähettäminen useille vastaanottajille. Komponentit käyttöön uusia asynkroninen tekstiviesti skenaarioita, kuten ajallinen irtautus, Julkaise ja tilaa ja kuormituksen tasaus. Saat lisätietoja näiden tekstiviesti yksiköiden [palvelun Bus olevien, aiheet, ja tilaukset](service-bus-queues-topics-subscriptions.md).

Välitys-infrastruktuuria, jossa se tekstiviesti ominaisuuksien annetaan WCF- ja .NET Framework ohjelmoijille ja myös muut kautta.

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja palvelun Bus viestintä on seuraavissa artikkeleissa.

- [Palvelun Bus perusteet](service-bus-fundamentals-hybrid-solutions.md)
- [Palvelun Bus olevien, aiheet ja tilaukset](service-bus-queues-topics-subscriptions.md)
- [Opi käyttämään palvelua Bus olevien](service-bus-dotnet-get-started-with-queues.md)
- [Palvelun Bus aiheet ja tilaukset käyttäminen](./service-bus-dotnet-how-to-use-topics-subscriptions.md)
 

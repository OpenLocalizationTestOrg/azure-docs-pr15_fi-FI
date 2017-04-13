<properties
   pageTitle="Yleistä kangasta luotettavaa palvelun ohjelmoinnin mallin | Microsoft Azure"
   description="Lisätietoja palvelun kangasta luotettavaa palvelun ohjelmoinnin mallin ja aloita kirjoittaminen Omat palvelut."
   services="Service-Fabric"
   documentationCenter=".net"
   authors="masnider"
   manager="timlt"
   editor="vturecek; mani-ramaswamy"/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="03/25/2016"
   ms.author="masnider;vturecek"/>

# <a name="reliable-services-overview"></a>Luotettavan Services-palvelujen yleiskatsaus
Azure palvelun kangasta yksinkertaistaa kirjoittaminen ja tilattomien ja tilallisten luotettava palveluiden hallinta. Tämä asiakirja keskustella:

- Luotettavan Services programming malli tilattomien ja tilallisten varten.
- Sinun on tehtävä, kun kirjoitat luotettavaa palvelun valinnat.
- Joissakin tilanteissa ja esimerkkejä siitä, milloin kannattaa käyttää luotettavia palveluja ja miten ne on kirjoitettu.

Luotettavia palveluja on käytettävissä palvelun kangasta ohjelmoinnin malleja. Lisätietoja luotettava toimijoiden ohjelmoinnin malli on artikkelissa [Johdatus palvelun kangasta luotettava toimijat](service-fabric-reliable-actors-introduction.md).

Palvelun kangasta palvelu on koostua määritys-sovelluksen koodi- ja Postitoimipaikka (valinnainen).

Palvelun kangasta hallitsee palvelujen elinkaaren valmistelu ja käyttöönoton päivittäminen ja poistaminen kautta [palvelun kangasta sovellusten hallinta](service-fabric-deploy-remove-applications.md).

## <a name="what-are-reliable-services"></a>Mitkä ovat luotettavia palveluja?
Luotettavan Services tutustutaan yksinkertainen, tehokkaita ja ylimmän tason ohjelmoinnin mallin avulla voit ilmaista asiat on kerrottu sovelluksen. Luotettavan palveluihin ohjelmointi mallia näyttöön tulee:

- Tilallisten palveluiden luotettavia palveluja ohjelmoinnin mallin avulla voit tallentaa sisällä palvelun tilan oikealla johdonmukaisesti ja luotettavasti käyttämällä luotettava sivustokokoelmat. Tämä on erittäin käytettävissä sivustokokoelman luokat, joka on tuttu kaikki, jotka on käyttänyt C# sivustokokoelmat yksinkertainen joukko. Perinteisesti services tarvittavat ulkoiset järjestelmät luotettava tilan hallinta. Luotettava sivustokokoelmat, jossa voit tallentaa osavaltio vieressä oman Laske saman suuren käytettävyyden ja olet tullut toiminta-erittäin käytettävissä ulkoisen stores luotettavuutta ja muita viive-parannukset, etsitään yhtä suorittaminen ja tila on.

- Yksinkertainen malli on käynnissä oman koodin, joka näyttää ohjelmointi malleja, joita käytetään. Koodi on hyvin määritetyn aloituskohdan ja helposti hallitun elinkaari.

- Vaihdettava viestintä-malli. Käytä transport valittua, kuten HTTP ja [Verkko-Ohjelmointirajapinnan](service-fabric-reliable-services-communication-webapi.md), WebSockets, mukautetut TCP-protokollaa. Luotettavia palveluja on joitakin hyvien ulos,-valmiilla asetukset voit käyttää tai voit antaa oman.

## <a name="what-makes-reliable-services-different"></a>Minkälainen luotettavia palveluja eri?
Luotettavan Services-palvelun kangasta poikkeaa palveluja, sinun on kirjoitettu ennen. Palvelun kangasta on luotettavasti, käytettävyyttä, yhdenmukaisuuden ja skaalattavuus.  

- **Luotettavuuden**--palvelun säilyvät myös epäluotettavista ympäristössä, jossa tietokoneesi saattaa epäonnistua tai osumien verkko.

- **Käytettävyys**--palvelussa on tavoitettavissa ja vastaa. (Tämä ei tarkoita, että palvelut, joita ei löydy tai Etusivulta ei voi olla ulkopuolella.)

- **Skaalattavuus**--Services on irrotettu määritetyt laitteisto- ja niiden Suurenna tai Pienennä lisäämällä tai laitteiston tai virtual resurssien – tarpeen mukaan. Palvelut ovat helposti osioitu (erityisesti tilallisten laatikko) varmistaaksesi, että palvelun riippumaton osiin skaalata ja vastata virheet erikseen. Lopuksi palvelun kangasta tehostaa palvelujen on oltava kevyt tuhansia palvelujen valmisteltu prosessin, jolla sallitaan sijaan edellyttävän tai osoitetaan koko OS esiintymät yksittäisen esiintymän tietyn työmäärää.

- **Yhdenmukaisuuden**--tämä palvelu tallennettuja tietoja voidaan taata yhtenäistämistä (tämä koskee vain tilallisten palvelut - Lisää osoitteessa myöhemmin)

## <a name="service-lifecycle"></a>Palvelun elinkaari
Riippumatta siitä, onko palvelun tilallisten tai kansalaisuudettomalla, luotettavia palveluja on yksinkertainen elinkaari, jonka avulla voit nopeasti Liitä koodi ja käytön aloittaminen.  On vain yksi tai kaksi menetelmiä, jotka haluat toteuttaa saat palvelun tekemiseksi.

- **CreateServiceReplicaListeners/CreateServiceInstanceListeners** - kohtaa, johon palvelu määrittää, se haluaa käyttää viestintä-pino on. Viestintä-pino [Verkko-Ohjelmointirajapinnan](service-fabric-reliable-services-communication-webapi.md), kuten on mitä määrittää kuunteleminen päätepisteen tai päätepisteet (miten asiakkaat saavuttaa sen)-palvelun. Määrittää myös siitä, miten Lopeta ylös palvelukoodi loput käyttäminen tulevat viestit.

- **RunAsync** – tämä on, jossa palvelun suoritetaan sen liiketoimintalogiikan. Peruutus-tunnuksen, joka on annettu on signaali, kun tukevat kannattaa lopettaa. Jos sinulla on palvelu, jota tarvitsee jatkuvasti viestien luotettava jonon ulos ja käsitellä niitä, tämä on esimerkiksi, jossa tukevat tapahtuu.

### <a name="service-startup"></a>Palvelun

Luotettavan palvelun elinkaari pää tapahtumat ovat:

1. Service-objekti (kohdetta, joka johdetaan tilattomien tai tilallisten-palvelua) muodostetaan.

2. `CreateServiceReplicaListeners` / `CreateServiceInstanceListeners` Menetelmää kutsutaan, jossa palvelun voit palauttaa yhden tai useamman viestintä kuuntelijoita sen värisinä.
  - Huomaa, että tämä ei ole valinnainen, vaikka useimmat palvelut näyttää joitakin päätepisteen suoraan.

3. Viestintä kuuntelijoita luodaan, kun se avataan.
  - Viestintä kuuntelijat menetelmän `OpenAsync()`, jonka nimi on tässä vaiheessa ja joka palauttaa listening-palvelun osoite. Jos luotettavaa palvelua käytetään jonkin valmiin ICommunicationListeners, tämä käsitellään puolestasi.

4. Kun viestintä kuuntelutoiminto on avoinna, `RunAsync()` menetelmää tärkeimmät palvelun kutsutaan.
  - Huomaa, että `RunAsync()` on valinnainen. Jos palvelun kaikki työnsä suoraan käyttäjän saatuaan puhelinkeskustelut, ei ole tarpeen sen toteuttamisesta `RunAsync()`.

### <a name="service-shutdown"></a>Palvelun sulkeminen

Kun palvelu on suljetaan (tullaan poistamaan, päivitetyn tai siirretty) puhelun tilauksen näkyy peilikuvana: ensin, peruuttaminen-tunnuksen saamisesta `RunAsync()` peruutetaan; Valitse `CloseAsync()` kutsutaan viestintä kuuntelijoita.

Liittyy muutama huomioon Sammuta tilallisten palveluiden koskeva huomautus:

- Palvelun kangasta ei edistää palvelun ensisijainen tilaan, ennen kuin toinen replika `CloseAsync` ja `RunAsync` on palautettu. Jos käytössäsi on valmiita viestintä-kuuntelija `CloseAsync` menetelmä käsitellään puolestasi.

- Kun näitä vaihtoehtoja, joka palauttaa ei ole määräajan, menetät heti voi kirjoittaa luotettava sivustokokoelmat ja näin ollen ei voi suorittaa todellinen työ. On suositeltavaa, että palaat yhteydessä peruutus pyynnön mahdollisimman pian.

## <a name="example-services"></a>Esimerkki palvelut
Tietää ohjelmoinnin tätä mallia, voit nähdäksesi, kuinka nämä kappaletta sopivat yhteen kaksi eri palveluissa lyhyesti.

### <a name="stateless-reliable-services"></a>Tilattomien luotettavia palveluja
Tilattomien palvelu on sellainen, jossa on literaaleina ei sisällä palvelun tilan tai tila, joka ei sisällä tietoja on täysin kertakäyttöisiä ja ei edellytä synkronoinnin, replikoinnin, pysyvyyttä tai suuren käytettävyyden.

Esimerkkinä Laskimen, joka ei ole muistia ja vastaanottaa kaikkien ehtojen ja toimintojen samalla kertaa.

Tässä tapauksessa palvelun RunAsync() voi olla tyhjä, koska ei ole taustan tehtävän käsittelyyn palvelu on suoritettava. Kun Laskin-palvelu on luotu, se palauttaa ICommunicationListener (esimerkiksi [Verkko-Ohjelmointirajapinnan](service-fabric-reliable-services-communication-webapi.md)), joka avaa kuunteleminen päätepisteen joitakin porttiin. Tämä kuunteleminen päätepiste yhdistää eri tavat (esimerkiksi: "Lisää (n1, n2)"), joka määrittää ja Laskimen julkisen API.

Kun puhelu soitetaan on jokin muu sähköpostiohjelma, sopiva menetelmä käynnistetään ja laskuri-palvelun suorittaa laskutoimituksia annetut tiedot ja palauttaa tuloksen. Se ei tallenna mitään tila.

Tässä esimerkissä Laskimen ei tallenna mitään sisäinen tila on kuvattu yksinkertainen. Mutta useimmat palvelut eivät ole todella tilattomien. Sen sijaan ne externalize joitakin Store niiden tilaan. (Esimerkiksi minkä tahansa web-sovelluksen, joka on riippuvainen pitäminen varmuuskopioiminen kaupan tai välimuistin istunnon tila ei ole täysin tilattomien.)

Yleinen esimerkki miten tilattomien services voi käyttää palvelua kangasta on edusta, joka paljastaa julkiselle Ohjelmointirajapinnan web-sovelluksen. Edusta-palvelun puheen sitten tilallisten palveluiden käyttäjän pyyntöä. Tässä tapauksessa asiakkaiden puhelut ohjataan tunnetut porttiin, kuten 80, jossa tilattomien palvelu Kuuntele. Tilattomien palvelua vastaanottaa puhelua ja määrittää, onko puhelu luotettujen osapuolen, kuin sekä mikä palvelu se on tarkoitettu.  Tilattomien palvelun välittää puhelun tilallisten palvelun oikea-osioon ja odottaa vastausta. Tilattomien palvelun saa vastausta, se vastauksia takaisin alkuperäiseen asiakkaalle.

### <a name="stateful-reliable-services"></a>Tilallisten luotettavia palveluja
Tilallisten palvelu on sellainen, joka on oltava osa-funktion, mitä yhdenmukaisia ja esitä-palvelun tila. Harkitse palvelu, joka laskee jatkuvasti jonkin arvon perusteella se saa päivityksiä liukuvan keskiarvon. Voit tehdä tämän sen on oltava käytössä olevista pyynnöt, se on prosessin sekä nykyisen keskiarvon. Palvelun, joka hakee, käsittelee ja tallentaa tiedot (kuten Azure Blob-objektien tai taulukon tallentaa luodun tänään) ulkoiset säilössä on tilallisten. Se vain pitää tilaan ulkoisen tilan säilö.

Useimmat palvelut tallentaa ulkoisesti, niiden tilaan tänään, koska ulkoisen säilö on, mikä tarjoaa luotettavuutta, käytettävyyttä, skaalattavuus ja yhdenmukaisuuden valtion. -Palvelun kangasta tilallisten services ei tarvitse tallentaa niiden tilaan ulkoisesti. Palvelun kangasta kestää varoen nämä vaatimukset palvelun-koodin ja palvelun tilan.

Oletetaan, että haluat kirjoittaa palvelu, joka kestää pyynnöt muunnokset, jotka on suoritettava kuva ja kuva, joka on muunnettava sarjalle.  Tämän palvelun se palauttavat viestintä kuuntelija, joka avaa viestintä-portin määrittäminen ja mahdollistaa lähetysten API-Liittymän kautta, kuten (oletetaan, että verkko-Ohjelmointirajapinnan) `ConvertImage(Image i, IList<Conversion> conversions)`. Tämä API-palvelu voi ottaa tiedot ja tallentaa pyynnön luotettava jonossa ja palauttaa joitakin tunnuksen sitten asiakkaan, jotta se voi seurata pyynnön (koska pyynnöt voi kestää jonkin aikaa).

Tämä palvelu RunAsync voi olla monimutkaisia. Palvelun voi olla silmukan sisällä sen RunAsync, joka hakee pyynnöt IReliableQueue ulos, suorittaa luettelossa muunnokset ja tallentaa tulokset IReliableDictionary, niin, että asiakas on takaisin, kun he pääsevät käyttämään muunnettu kuvia. Voit varmistaa, että, vaikka jotain epäonnistuu kuva ei menetetä, luotettavaa palvelua tuoda jonossa, suorittaminen muunnokset ja tallentaa tuloksen tapahtuman. Tässä tapauksessa viesti poistetaan vain-jonossa ja tulokset on tallennettu tulos sanaston muunnokset ollessa. Jos järjestelmässä epäonnistuu keskellä (esimerkiksi tietokoneen tämä esiintymä koodi on käytössä), pyynnön säilyy jonossa odottavien käsiteltävä uudelleen.

Huomautus Tämä palvelu tietoja on, että se kuulostaa kuten .NET-palvelun Normaali. Ainoa ero on tietorakenteiden käytössä (IReliableQueue ja IReliableDictionary) tarjoamien palvelun kangasta ja näin ollen on tehty erittäin luotettava, käytettävissä ja yhtenäinen.

## <a name="when-to-use-reliable-services-apis"></a>Luotettavan palvelut-ohjelmointirajapinnan käyttäminen
Jos jokin seuraavista vapausasteiden tarpeitasi palvelun sovelluksen, ota huomioon luotettava Services API:

- Sinun on annettava sovelluksen toimintaa yli useita yksiköiden tila (kuten tilausten ja kohteiden järjestäminen rivi).

- Sovelluksen tila voi luonnollisesti mallintaa luotettava sanastot ja olevien.

- Osavaltio on oltava käytettävissä erittäin pienen viive access.

- Sovellus on valvoa samanaikainen ja rakeisuuden tapahtuma-toimintoa yli vähintään yksi luotettava sivustokokoelmat.

- Haluat hallita tietoliikenteen tai ohjausobjektien osiointimalli palvelun.

- Koodi on-threaded runtime-ympäristössä.

- Sovellus on dynaamisesti luomiseen tai poistamiseen luotettava sanastot tai olevien suorituksen aikana.

- Sinun täytyy ohjelmallisesti valvoa palvelua kangasta toimitettuja varmuuskopiointi ja palauttaminen yhteyttä palvelun tilan * ominaisuuksia.

- Sovellus on säilyttää muutoslokia sen tilan * yksikköä.

- Haluat kehittää tai tarjoaman tilaan kolmannen osapuolen-kehitettyjä, mukautetut tarjoajat *.

> [AZURE.NOTE] * Ominaisuudet osoitteessa SDK yleiseen käyttöön.


## <a name="next-steps"></a>Seuraavat vaiheet
+ [Luotettavan palvelut-pikaopas](service-fabric-reliable-services-quick-start.md)
+ [Luotettavan palvelujen erikoishaun käyttö](service-fabric-reliable-services-advanced-usage.md)
+ [Luotettavan toimijoiden ohjelmoinnin malli](service-fabric-reliable-actors-introduction.md)

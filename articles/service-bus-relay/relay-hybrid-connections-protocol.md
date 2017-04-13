<properties 
    pageTitle="Azure välitys Hybrid yhteydet protokolla | Microsoft Azure"
    description="zure välitys Hybrid yhteydet protokolla opas."
    services="service-bus"
    documentationCenter="na"
    authors="clemensv"
    manager="timlt"
    editor="tysonn" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/28/2016"
    ms.author="sethm" />

# <a name="azure-relay-hybrid-connections-protocol"></a>Azure välitys Hybrid yhteydet-protokolla

Azure välitys on yksi tärkeimmistä ominaisuuksien pylväiden Azure palvelun Bus-ympäristön. Välitys uusi "Hybrid yhteydet-ominaisuus on suojattu, Avaa-protokollan kehitystä, HTTP ja WebSockets perusteella.

Se korvaa ensiksi nimeltä "BizTalk Services"-ominaisuutta, joka on luotu oma protokolla Foundation tasaisesti. Hybrid yhteydet integroida Azure sovelluksen palvelut toimivat säilyvät-on.

"Hybrid yhteydet-avulla vahvistamiseksi kaksisuuntaisen, binaarinen stream välisessä kaksi verkossa olevat sovellukset, jolla toisen tai molemmat seuraavista osapuolet voivat sijaita NAT tai palomuurit takana.

Tässä asiakirjassa on kuvattu asiakkaan aktiviteettien Hybrid yhteydet välityksen asiakasohjelmien listener ja lähettäjä-roolien ja miten kuuntelijoita hyväksy uusia yhteyksiä muodostettaessa.

## <a name="interaction-model"></a>Vuorovaikutuksen malli

Hybrid yhteydet välitys muodostaa kahden osapuolen tarjoamalla rendezvous pisteen Azure pilveen, sekä osapuolet voi tutkia ja muodostaa yhteyden oman verkon näkökulmasta. Rendezvous kohdassa kutsutaan "Hybrid yhteyden" tämän ja muiden asiakirjojen API ja Azure-portaalissa. Hybrid yhteydet Palvelupäätepisteen viitataan "palveluna" jäljempänä tässä asiakirjassa.

Valitse useita verkko ohjelmointirajapinnan perustettu terminologiaa leans vuorovaikutus-mallin:

Tällä listener, joka ilmaisee ensin valmiuden käsittelemään saapuvia yhteyksiä ja hyväksyy ne tämän jälkeen saapuville viesteille. Toinen puoli ei asiakaskoneen, joka muodostaa yhteyden listener odotetaan yhteyden hyväksyä vahvistamiseksi kaksisuuntaisen polku. "Yhteyden", "Kuunnella", "Hyväksy" on useimmissa socket ohjelmointirajapinnan löydät samoin ehdoin.

Mikä tahansa välittäminen viestintä sisältävään osapuoli lähteviä yhteyksiä kohti Palvelupäätepisteen, joka tekee "kuuntelua" myös "asiakas" puhekielen käytössä ja voivat aiheuttaa muita termejä Osastollasi; Käytämme vuoksi Hybrid yhteyksien tarkka sanasto on seuraavanlainen:

Yhteyden molemmille puolille ohjelmia kutsutaan "asiakas", koska ne ovat asiakkaat-palveluun. Asiakas, joka odottaa ja hyväksyy yhteydet on "kuuntelua" tai "listener rooli" pidetään. Asiakas, joka käynnistää uusi yhteys kuuntelija palvelun kautta kohti kutsutaan "lähettäjä" tai "lähettäjä"-roolin.

## <a name="listener-interactions"></a>Listener vuorovaikutukset

Kuuntelun neljä vuorovaikutukset-palvelussa kaikki wire tiedot on artikkelissa jäljempänä tässä asiakirjassa viittaus-osassa.

### <a name="listen"></a>Kuuntele

Osoita valmiuden kuuntelija olevan palvelun valmis hyväksymään yhteyksiä, se luo lähtevän web socket yhteyden. Yhteyden kättely suorittaa määritetty välitys nimitilan ja suojaustunnus, joka antaa "Kuunnella" tämän ohjeen, johon funktio Hybrid yhteyden nimi. Kun web-socket hyväksytään palvelu, rekisteröinti on suoritettu ja aiemmin luotuja web-socket säilytetään toiminnassa nimellä "ohjausobjektin kanavan" vuorovaikutuksessa ottaminen käyttöön. Palvelun sallii enintään 25 samanaikaista kuuntelijoita Hybrid yhteyden. Jos luettelossa on 2 tai useampi active kuuntelijoita, saapuvia yhteyksiä täsmätään kattavia asioita satunnaisessa järjestyksessä, tiedenäyttelyn jakauman ei välttämättä.

### <a name="accept"></a>Hyväksy

Aina, kun lähettäjän avautuu uusi yhteys-palvelusta, palvelun valitseminen ja ilmoittaa jonkin aktiivisen kuuntelijoita Hybrid-yhteyttä. Ilmoitus lähetetään kuuntelua avaa ohjausobjektin kanavan JSON viestinä Web socket päätepiste, jonka kuuntelutoiminto on muodostettava yhteys hyväksymistä varten URL-osoite.

URL-Osoitteen voi ja käyttää suoraan kuuntelua ilman ylimääräisiä työ; koodattu tiedot on voimassa vain lyhyen ajanjakson, olennaisesti varten niin kauan, kun kyseessä on valmis Odota, yhteys on muodostettu lopusta loppuun, mutta enintään 30 sekuntia. URL-Osoitteen voi käyttää vain yhtä onnistuneen yhteyden muodostaminen. Kun ja rendezvous URL-osoite Web socket-yhteys on muodostettu, Web-socket edelleen toimintaa välitetään lähde- ja sen lähettäjälle painamalla ilman mitään toimia tai tulkintaan palvelu.

### <a name="renew"></a>Uusiminen 

Suojaustunnus, jota käytetään Rekisteröi kuuntelua ja ylläpitää ohjausobjektin kanavan voivat vanhentua kuuntelua ollessa aktiivinen. Suojaustunnuksen määräajan ei vaikuta jatkuvaa yhteyksiä, mutta se aiheuttaa ohjausobjektin kanavan poistetaan palvelu-palvelussa tai pian voimassaolon kokouksen jälkeen. "Uusi"-eleen on JSON kuuntelutoiminto lähettää sen korvaa liittyvän ohjausobjektin, kanavan tunnuksen niin, että ohjausobjektin kanavan voidaan ylläpitää pitkän viestin.

### <a name="ping"></a>Ping-kutsu 

Jos ohjausobjektin kanavan pysyy käyttämättömänä pitkään välittäjiä matkalla, kuten kuormituksen tasoitusmääritykset tai NAT saattaa pudota TCP-yhteyden. "Ping" liikkeen vältetään, lähettämällä vähän tietoja kanavalla, joka muistuttaa kaikille verkon reitin, joka yhteyden tarkoitus on toiminnassa, ja se toimii myös liveness testi kuuntelua varten. Jos ping-kutsu epäonnistuu, ohjausobjektin kanavan voidaan pitää käyttökelvoton ja kuuntelua tulee uudelleen.

## <a name="sender-interaction"></a>Lähettäjän vuorovaikutus

Lähettäjän on vain yksi vuorovaikutus-palvelussa, muodostaa yhteyden.

### <a name="connect"></a>Yhdistäminen

"Yhdistä" liikkeen Avaa Web-socket palvelun Hybrid yhteyden nimen ja (valinnainen, mutta pakollinen oletusarvon mukaan) ja antaa "Lähetä" käyttöoikeudet kyselymerkkijonon suojaustunnus. Palvelun sitten yllä olevien ohjeiden tavalla listener käsitellä ja on rendezvous-yhteyden, Web-socket liitetään listener. Web-socket hyväksymisen edelleen vuorovaikutus-Web-socket vuoksi on yhdistetty kuuntelija kanssa.

## <a name="interaction-summary"></a>Vuorovaikutuksen yhteenveto

Vuorovaikutuksen tämän mallin tulos on, että lähettäjä-asiakasohjelman tulee ulos kättely "SIIVOA-Web-socket joka on yhteydessä kuuntelija ja, joka on edelleen preambles eikä valmistelu kanssa. Näin lähes mistä tahansa aiemmin Web socket asiakkaan käyttöönoton helposti hyödyntää Hybrid yhteydet-palvelua vain toimittamalla oikein rakennettava URL-Osoitteen niiden Web socket asiakkaan kerroksen kyselyjä.

Rendezvous yhteyden Web Socket joka kuuntelua hakee läpi Hyväksy vuorovaikutuksen on myös SIIVOA ja voi luovuttaa, kaikki olemassa olevat Web socket Serverin käyttöönoton joitakin mahdollisimman vähän ylimääräisiä otetaan, joka erottaa "Hyväksy" toimenpiteet niiden framework paikalliseen verkkoon kirjauduttaessa kuuntelijoita ja Hybrid yhteydet remote "Hyväksy" toimintojen kanssa.

## <a name="protocol-reference"></a>Protokolla

Tässä osassa kuvataan edellä kuvatun protokolla vuorovaikutukset tietoja.

Kaikki Web socket yhteydet tehdään porttiin 443 päivityksen HTTPS 1.1, joka on tavallisesti otetaan joitakin Web socket kehyksen tai API. Kuvaus tähän on käytettävissä käyttöönoton neutraalit, ilman ehdottaminen tietyn framework.

## <a name="listener-protocol"></a>Listener protokolla

Listener protokolla koostuu kaksi yhteyden liikkeitä ja kolme viestin toimintoja.

### <a name="listener-control-channel-connection"></a>Ohjausobjektin kanavan kuunteluyhteys

Ohjausobjektin kanavan avaamisen luomisessa Web socket yhteyden:

*wss: / / {address nimitilan} /* *$servicebus* */* *hybridconnection /**{polku}? sb hc-toiminnon =... & sb hc tuotetunnus-=... & sb hc-tunnuksen =... "*

*Nimitilan-osoite* on täydellinen toimialuenimi, joka isännöi Hybrid-yhteyden yleensä likiarvon Azure välitys nimitilan {*omanimi}. servicebus.windows.net.*

Kyselyn kyselymerkkijonon parametrin asetukset ovat seuraavat

| Parametri        | Pakollinen? | Kuvaus                                                                                                                                                                                        |
|--------------|-----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SB hc-toiminto | Kyllä       | Listener roolin parametrin on oltava **sb hc-toiminnon = kuunnella**                                                                                                                                |
| {polku}       | Kyllä       | Valmiiksi määritetyt Hybrid yhteys Rekisteröi tämä listener URL-koodatun nimitilan polku. Tämä lauseke näkyy kiinteä, * **$servicebus**/**hybridconnection /*** polun osan. |
| SB hc-tunnus  | Kyllä\*     | Kuuntelutoiminto on määritettävä kelvollinen, URL-koodatun palvelun Bus jaettu Access suojaustunnuksen nimitilan tai Hybrid-yhteys, joka antaa **kuunnella** oikealle.                                           |
| SB hc tuotetunnus-     | Ei        | Valinnainen asiakkaan annettu tunnus sallii lopusta loppuun diagnostiikan jäljitys.                                                                                                                             |

Jos Web Socket yhteys epäonnistuu vuoksi ei ole rekisteröity, Hybrid yhteyden polku tai virheellinen tai puuttuu-tunnuksen tai jokin muu virhe, virhe palautetta toimitetaan säännöllisesti HTTP 1.1 tila palautetta mallin. Tilan kuvaus sisältää virhe seuranta-tunnuksen, joka voidaan välittää Azure-tuki:

| Koodi | Virhe          | Kuvaus                                                            |
|------|----------------|------------------------------------------------------------------------|
| 404  | Ei löydy      | Hybrid yhteyden **polku** on virheellinen tai Perusosoitteen on virheellinen |
| 401  | Ei valtuuksia   | Suojauksen salasana on puuttuu tai Vääränlainen tai on virheellinen                  |
| 403  | Käyttö estetty      | Suojauksen salasana ei ole kelvollinen Tämä polku, jotta tämä toiminto          |
| 500  | Sisäinen virhe | Järjestelmässä tapahtui virhe-palvelussa                                    |

Jos palvelun Sammuta Web socket yhteyden tarkoituksella, kun se on alun perin määritetty, tällöin tiedoksi käyttämällä sopiva Web socket protokolla virhekoodin sekä kuvaava virhesanoma, joka sisältää seuranta-tunnus syy. Palvelu ei ole suljetaan ohjausobjektin kanavan ilman encountering virhetilassa. Minkä tahansa SIIVOA sammuttamisen on ohjaa asiakas.

| WS tila | Kuvaus                                                                        |
|-----------|------------------------------------------------------------------------------------|
| 1001      | Hybrid yhteyden polku on poistettu tai poistettu käytöstä.                           |
| 1008      | Suojauksen salasana on päättynyt ja näin ollen rikkoo luvan käytäntöä. |
| 1011      | Järjestelmässä tapahtui virhe palvelun sisällä.                                           |

### <a name="accept-handshake"></a>Hyväksy handshake 

Hyväksy-ilmoitus lähetetään palvelun kuuntelua aiemmin muodostettu ohjausobjektin kanavan JSON viestinä Web socket tekstin kehyksessä. Ei vastausta tähän viestiin.

Viestin sisältää JSON-objektin nimeltä "Hyväksy", joka määrittää tällä hetkellä seuraavia ominaisuuksia:

-   **osoite** – URL-merkkijono, jota käytetään vahvistamiseksi palveluun Web-Socket Hyväksy saapuva yhteys.

-   **tunnus** – tässä yhteydessä yksilöllinen. Jos lähettäjä-asiakas on annettu tunnus, se on annettu lähettäjä-arvo, muuten järjestelmä-arvo on.

-   **connectHeaders** – kaikki HTTP-otsikot, jotka on annettu välitys päätepisteen lähettäjä, joka sisältää myös sek-WebSocket-protokollan ja sek-WebSocket-laajennukset otsikot.

| Hyväksy viestin                                                                     |
|------------------------------------------------------------------------------------|
```json
{                                                                                  
    "accept" : {                                                                                    
        "address" : "wss://168.61.148.205:443/$servicebus/hybridconnection/{path}?...",                                                                          
        "id" : "4cb542c3-047a-4d40-a19f-bdc66441e736\_G0\_G1",                         
        "connectHeaders" : {                                                                
            "Host" : "…",                                                                       
            "Sec-WebSocket-Protocol" : "…",                                                     
            "Sec-WebSocket-Extensions" : "…"                                                    
        }                                                                                     
    }
}
```

JSON-viestin osoite URL-Osoitetta käytetään kuuntelutoiminto muodostaa hyväksymisen tai hylkäämisen lähettäjän socket Web-Socket.

#### <a name="accepting-the-socket"></a>Hyväksy socket

Hyväksy-kuuntelua muodostaa annettu osoite WebSocket yhteyden.

Seuraavat asiat esikatselu-ajan, URI-osoite voi käyttää ajoneuvon IP-osoite sekä toimittamaan palvelimen TLS-sertifikaatti epäonnistuu vahvistuksen osoite. Tämä korjattava aikana esikatselu.

Jos "Hyväksy"-sanoma "Sec-WebSocket-protokollan" otsikko, odotetaan, että kuuntelua vain Hyväksy WebSocket, jos se tukee kyseistä protokolla ja, että se asettaa otsikon WebSocket on muodostettu.

Sama koskee "Sec-WebSocket-laajennukset"-otsikko. Jos kehys tukee tiedostotunnistetta, sitä Määritä otsikon tarvittavat "Sec-WebSocket-laajennukset" kättely tiedostotunnisteen *palvelimen* puoli vastata.

URL-osoite on käytettävä-vahvistamiseksi Hyväksy socket on, mutta sisältää seuraavat parametrit:

| Parametri        | Pakollinen? | Kuvaus                                                                                                                                                                                                                                                                                   |
|--------------|-----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SB hc-toiminto | Kyllä       | Socket hyväksymistä varten parametrin on oltava **sb hc-toiminnon = Hyväksy**                                                                                                                                                                                                                          |
| {polku}       | Kyllä       | Valmiiksi määritetyt Hybrid yhteys Rekisteröi tämä listener URL-koodatun nimitilan polku. Tämä lauseke näkyy kiinteä, * **$servicebus**/**hybridconnection /*** polun osan.                                                                                            
                                                                                                                                                                                                                                                                                                                           
                            The path expression MAY be extended with a suffix and a query string expression that follows the registered name after a separating forward slash.                                                                                                                                             
                                                                                                                                                                                                                                                                                                                           
                            This allows the sender client to pass dispatch arguments to the accepting listener when it is not possible to include HTTP headers.                                                                                                                                                            
                                                                                                                                                                                                                                                                                                                           
                            The expectation is that the listener framework will parse out the fixed path portion and the registered name from the path and make the remainder, possibly without any query string arguments prefixed by “sb-“, available to the application for deciding whether to accept the connection.  
                                                                                                                                                                                                                                                                                                                           
                            For more detail see the “Sender Protocol” section below.                                                                                                                                                                                                                                       |
| SB hc tuotetunnus-| No        | Katso **tunnus** kuvaus.                                                                                                                                                                                                                                                              |

Jos järjestelmässä tapahtuu virhe, palvelun voi vastata seuraavasti:

| Koodi | Virhe          | Kuvaus                         |
|------|----------------|-------------------------------------|
| 403  | Käyttö estetty      | URL-osoite ei ole kelvollinen.               |
| 500  | Sisäinen virhe | Järjestelmässä tapahtui virhe-palvelussa |

Kun yhteys on muodostettu, palvelimen Sammuta Web socket kun lähettäjän Web socket tilaan alas tai seuraavat tilat

| WS tila | Kuvaus                                                                        |
|-----------|------------------------------------------------------------------------------------|
| 1001      | Lähettäjä-asiakasohjelman sulkee yhteys                                        |
| 1001      | Hybrid yhteyden polku on poistettu tai poistettu käytöstä.                           |
| 1008      | Suojauksen salasana on päättynyt ja näin ollen rikkoo luvan käytäntöä. |
| 1011      | Järjestelmässä tapahtui virhe palvelun sisällä.                                           |

#### <a name="rejecting-the-socket"></a>Hylkää socket

Vastaavat handshake hylkäämällä socket jälkeen tarkistetaan "Hyväksy"-viesti edellyttää, niin, että tilakoodin ja tilan kuvaus viestintä hylkäyksen syy voi flow lähettäjälle.

Tähän protokolla-rakenne vaihtoehto on käyttää WebSocket kättelyn (joka on suunniteltu loppuun määritetyn virhetilassa) niin, että listener asiakkaan käyttöotot edelleen riippuvaisia WebSocket asiakkaan ja ei tarvitse käyttää ylimääräinen, täydellisiä valmisteluja HTTP-asiakas.

Jos haluat hylätä socket, asiakkaan käsittelee "Hyväksy"-viestin URI-osoite ja liittää kaksi kyselyparametri:

| Parametri             | Pakollinen? | Kuvaus                             |
|-------------------|-----------|-----------------------------------------|
| statusCode        | Kyllä       | HTTP-tilan numerokoodin                |
| TilanKuvaus | Kyllä       | Ihmisten luettavissa syy hylkääminen |

Tuloksena URI käytetään WebSocket; yhteyden muodostaminen uudelleen muista, että TLS-sertifikaatti ei ehkä vastaa osoite aikana esikatselussa, jotta vahvistus on ehkä ole käytettävissä.

Kun olet suorittanut oikein, tämä handshake epäonnistuu tarkoituksella HTTP-virhekoodin 410, koska ei ole WebSocket ei ole muodostettu. Jos näyttöön tulee virhesanoma, nämä vaihtoehdot ovat:

| Koodi | Virhe          | Kuvaus                         |
|------|----------------|-------------------------------------|
| 403  | Käyttö estetty      | URL-osoite ei ole kelvollinen.               |
| 500  | Sisäinen virhe | Järjestelmässä tapahtui virhe-palvelussa |

### <a name="listener-token-renewal"></a>Listener tunnuksen uusiminen

Kun listener tunnuksen on päättymässä, se korvaa lähettämällä kehyksen tekstiviestillä palvelua aiemmin luotuja ohjausobjektin kanavan kautta. Viesti sisältää JSON objektin nimeltä "renewToken", joka määrittää tällä hetkellä seuraavaa ominaisuutta:

-   **Suojaustunnuksen** – kelvollinen, URL-koodatun palvelun Bus jaettu Access suojaustunnuksen nimitilan- tai Hybrid-yhteys, joka antaa **kuunnella** oikealle.

| renewToken viesti                                                                                                                                                    
|------------------------------------------------------------------------------------------------|
```json
{
    "renewToken" : {      
        "token" : "SharedAccessSignature sr=http%3a%2f%2fcontoso.servicebus.windows.net%2fHybridConnection1%2f&amp;sig=XXXXXXXXXX%3d&amp;se=1471633754&amp;skn=SasKeyName"
    }                                                                                                                                                            
}
```                                                                                                                                                                      

Jos suojaustunnuksen vahvistus epäonnistuu, käyttö on estetty ja pilvipalvelussa suljetaan ja järjestelmä antaa virheilmoituksen ohjausobjektin kanava-websocket, muuten ei ole vastausta.

| WS tila | Kuvaus                                                                        |
|-----------|------------------------------------------------------------------------------------|
| 1008      | Suojauksen salasana on päättynyt ja näin ollen rikkoo luvan käytäntöä. |

<a name="sender-protocol"></a>Lähettäjä-protokolla
---------------

Lähettäjä-protokolla on tehokas samanlainen miten kuuntelija on muodostettu. Tavoitteena on avoimuutta lopusta loppuun-Web-socket varten. Muodostaa yhteyden osoite on sama kuin kuuntelua, mutta "toiminto" eroaa ja tunnuksen on eri käyttöoikeudet:

*wss: / / {address nimitilan} /* *$servicebus* */* *hybridconnection /**{polku}? sb hc-toiminnon =... & sb hc tuotetunnus-=... \[& väli-hc-tunnuksen =... \]*

*Nimitilan-osoite* on täydellinen toimialuenimi, joka isännöi Hybrid-yhteyden yleensä likiarvon Azure välitys nimitilan {*omanimi}. servicebus.windows.net.*

Pyyntö voi olla haluamaansa ylimääräiset HTTP otsikot, mukaan lukien sovelluksen määrittämän niistä. Kaikki annettu otsikot flow kuuntelua ja löytyvät "Hyväksy" valvonta-sanoma "connectHeader"-aluetta.

Kyselyn kyselymerkkijonon parametrin asetukset ovat seuraavat

| Parametri        | Pakollinen? | Kuvaus                                                                                                                                                                                                       |
|--------------|-----------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SB hc-toiminto | Kyllä       | Listener roolin parametrin on oltava **toiminnon = yhteyden**                                                                                                                                                    |
| {polku}       | Kyllä       | Valmiiksi määritetyt Hybrid yhteys Rekisteröi tämä listener URL-koodatun nimitilan polku.                                                                                                               
                                                                                                                                                                                                                                               
                            The path expression MAY be extended with a suffix and a query string expression to communicate further                                                                                                             
                                                                                                                                                                                                                                               
                            If the Hybrid Connection is registered under the path “hyco”, the path expression can be “**hyco/**suffix?param=value&…” followed by the query string parameters defined here. A complete expression may then be:  
                                                                                                                                                                                                                                               
                            *wss://{ns}/**$servicebus**/**hybridconnection/*** **hyco/**suffix?param=value*& sb-hc-action=…&sb-hc-id=…\[&sbc-hc-token=…\]*                                                                                     
                                                                                                                                                                                                                                               
                            The path expression is passed through to the listener in the address URI contained in the “accept” control message.                                                                                                |
| SB hc-tunnuksen | Yes\*     | Kuuntelutoiminto on määritettävä kelvollinen, URL-koodatun palvelun Bus jaettu Access suojaustunnuksen nimitilan-tai Hybrid-yhteys, joka antaa oikeuden **lähettää** .                                                            | | SB hc tuotetunnus-| No        | Valinnainen tunnuksen, joka sallii lopusta loppuun diagnostiikan jäljityksen ja on saatavana kuuntelua Hyväksy kättelyn aikana.                                                                                       |

Jos Web Socket yhteys epäonnistuu vuoksi ei ole rekisteröity, Hybrid yhteyden polku tai virheellinen tai puuttuu-tunnuksen tai jokin muu virhe, virhe palautetta toimitetaan säännöllisesti HTTP 1.1 tila palautetta mallin. Tilan kuvaus sisältää virhe seuranta-tunnuksen, joka voidaan välittää Azure-tuki:

| Koodi | Virhe          | Kuvaus                                                            |
|------|----------------|------------------------------------------------------------------------|
| 404  | Ei löydy      | Hybrid yhteyden **polku** on virheellinen tai Perusosoitteen on virheellinen |
| 401  | Ei valtuuksia   | Suojauksen salasana on puuttuu tai Vääränlainen tai on virheellinen                  |
| 403  | Käyttö estetty      | Suojauksen salasana ei ole kelvollinen Tämä polku, jotta tämä toiminto          |
| 500  | Sisäinen virhe | Järjestelmässä tapahtui virhe-palvelussa                                    |

Jos palvelun Sammuta Web socket yhteyden tarkoituksella, kun se on alun perin määritetty, tällöin tiedoksi käyttämällä sopiva Web socket protokolla virhekoodin sekä kuvaava virhesanoma, joka sisältää seuranta-tunnus syy.

| WS tila | Kuvaus                                                                        |
|-----------|------------------------------------------------------------------------------------|
| 1 000      | Kuuntelutoiminto Sammuta socket.                                                 |
| 1001      | Hybrid yhteyden polku on poistettu tai poistettu käytöstä.                           |
| 1008      | Suojauksen salasana on päättynyt ja näin ollen rikkoo luvan käytäntöä. |
| 1011      | Järjestelmässä tapahtui virhe palvelun sisällä.                                           |

## <a name="next-steps"></a>Seuraavat vaiheet:

- [Välitys usein kysytyt kysymykset](relay-faq.md)
- [Luo nimitila](relay-create-namespace-portal.md)
- [.NET käytön aloittaminen](relay-hybrid-connections-dotnet-get-started.md)
- [Solmun käytön aloittaminen](relay-hybrid-connections-node-get-started.md)
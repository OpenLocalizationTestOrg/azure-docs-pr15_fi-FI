
# <a name="azure-and-internet-of-things"></a>Azure ja asioiden Internet

Tervetuloa käyttämään Microsoftin Azure ja Internetiä (IoT) asioiden. Tässä artikkelissa esitellään IoT ratkaisuarkkitehtuuri, joka kuvaa IoT ratkaisun Azure-palveluiden käyttäminen voi ottaa käyttöön yhteiset ominaisuudet. IoT ratkaisut edellyttävät suojatun kaksisuuntaisen yhteyden laitteiden, mahdollisesti numerointi miljoonia, ja ratkaisun Taustatietokannassa. Esimerkiksi ratkaisu Taustatietokannassa avulla automatisoitu, ehkäisevän analytics paljasta tapahtuma laitteen cloud-stream-asuun.

Azure IoT Hub on keskeisenä edistäjänä, kun otat tämän IoT ratkaisuarkkitehtuuri Azure-palveluiden käyttäminen. IoT Suite tarjoaa tämän arkkitehtuurin IoT tiettyjen tapausten osalta valmis, päästä päähän, toteutukset. Esimerkiksi: 

- *Etätietokoneen seuranta* ratkaisu mahdollistaa laitteiden, kuten luokkaan koneet tilaa. 
- *Ehkäisevän kunnossapidon* ratkaisu auttaa ennakoimaan tarpeita ylläpito laitteita, kuten pumput etäyhteyden pumppaus asemilla ja välttää ennalta suunnittelematon käyttökatkoja.

## <a name="iot-solution-architecture"></a>IoT ratkaisuarkkitehtuuri

Seuraavassa kaaviossa esitetään tyypillinen IoT arkkitehdin ratkaisu. Kaavioon ei sisälly tiettyjen Azure-palveluiden nimet, mutta kuvataan yleinen IoT arkkitehdin ratkaisu keskeisiä tekijöitä. Tällä arkkitehtuuri, jotka he lähettävät cloud gateway tietojen kerääminen IoT laitteet. Cloud gateway mahdollistaa tietojen käsittelyssä-johon tiedot toimitetaan rivin liiketoiminnan sovelluksia tai Dashboard-sivuun tai esityksen muuhun laitteeseen ihmisen toimijoiden välisiä muut palvelut.

![IoT ratkaisuarkkitehtuuri][img-solution-architecture]

> [AZURE.NOTE] Katso [Microsoftin Azure IoT Reference Architecture-arkkitehtuurin]perusteellisen keskustelun IoT arkkitehtuurin,[lnk-refarch].

### <a name="device-connectivity"></a>Laitteen yhteys

IoT ratkaisun tällä arkkitehtuuri laitteiden lähettää kaukomittaus, kuten anturin lukemat pumping työasemasta, varastointi ja käsittely cloud päätepistettä. Ehkäisevän kunnossapidon tilanteessa Taustatietokannassa avulla anturi tiedot virta selvittää, kun tietyn pumppu vaatii huoltoa. Laitteet voi myös vastaanottaa ja vastata cloud laitteen komennot, lukemasta sanomia jostain pilven päätepiste. Esimerkiksi ehkäisevän kunnossapidon tilanteessa ratkaisu Taustatietokannassa voi lähettää komentoja muut pumput pumping aseman aloittamaan lastaamalla virrat, ennen kuin ylläpito on määrä alkaa varmistaaksesi, että voit aloittaa ylläpito-insinööri, kun hän saapuu.

Yksi suurimpia haasteita IoT projektit on yhteyden laitteiden ratkaisu taustatietokantaan luotettavasti ja turvallisesti. IoT laitteilla on erilaiset ominaisuudet verrattuna muiden asiakkaiden, kuten selaimet ja mobiili apps. IoT laitteet:

- Ovat yleensä sulautetut järjestelmät ihmisen puuttuu operaattori.
- Voidaan ottaa käyttöön kaukaisissa paikoissa, jossa fyysinen käyttö on kallista.
- Ehkä vain saavutettavissa ratkaisun Taustatietokannassa. Mitenkään muun laitteen kanssa.
- Saattaa olla vain osittainen, teho ja käsittely resursseja.
- Voi olla ajoittain hidasta tai kallista verkkoyhteys.
- Saatat joutua käyttämään omia, mukautettuja tai toimialakohtaisia protokollia.
- Voit luoda suuri joukko suosittuja ympäristöjen laitteiston ja ohjelmiston avulla.

Edellä mainittujen vaatimusten lisäksi IoT kaikissa ratkaisuissa on myös toimitettava asteikko, tietoturvan ja luotettavuuden. Syntyvää yhteysvaatimuksista on Kova ja aikaa vievää vakuutuksenvälittäjän viestinnän ja web kuten perinteisten tekniikoiden avulla. Azure IoT keskitin ja IoT laitteen SDK: T helpottavat ratkaisuja, jotka täyttävät nämä vaatimukset.

Laite voi ottaa suoraan yhteyttä cloud gateway-päätepiste, tai jos laitetta ei voida käyttää tietoliikenneprotokollia, joka tukee cloud-yhdyskäytävän, se muodostaa keskitason yhdyskäytävän kautta. Esimerkiksi [Azure IoT pöytäkirjan gateway] [ lnk-protocol-gateway] protokolla käännös voi suorittaa, jos laitetta ei voi käyttää protokollia, joka tukee IoT Hub.

### <a name="data-processing-and-analytics"></a>Tietojen käsittely ja analytics

Takaisin ratkaisu lopettaa IoT on pilven, jossa suurimman osan tietojen käsittelyyn tapahtuu, kuten suodatuksen ja kootaan kaukomittaus ja reititys muut palvelut. Lopeta takaisin ratkaisu IoT:

- Vastaanottaa kaukomittaus mittakaavassa laitteita ja määrittää, miten käsitellä ja tallentaa tiedot. 
- Avulla asiakas voi lähettää komentoja pilvestä tietyn laitteen.
- Sisältää laitteen rekisteröinti ominaisuuksista, joiden avulla tarjotaan laitteisiin ja laitteista sallitaan muodostaa infrastruktuurin hallintaan.
- Voit seurata laitteiden tilaa ja valvoa niiden toimintaa.

Ehkäisevän kunnossapidon tilanteessa ratkaisu Taustatietokannassa kaukomittaus historialliset tiedot tallennetaan. Uudelleen Voit näiden tietojen avulla voit tunnistaa kuvioita, jotka ilmaisevat tietyn pumpun huolto erääntyy.

IoT ratkaisut voivat olla Automaattinen palaute silmukoita. Esimerkiksi analytics-moduuli uudelleen tunnistaa kaukomittaus, tietyn laitteen lämpötila on yli normaalin toiminnan tasoa. Ratkaisun jälkeen voit lähettää komennon laitteelle kotitietokoneen korjaustoimia.

### <a name="presentation-and-business-connectivity"></a>Esityksen ja business connectivity

Esityksen ja business connectivity kerros avulla käyttäjät voivat käsitellä IoT ratkaisun ja laitteet. Sen avulla käyttäjät voivat tarkastella ja analysoida niiden laitteiden kerätyt tiedot. Näitä näkymiä voit ottaa raporttinäkymät tai BI-raportit, jotka voidaan näyttää sekä historiatietoja tai lähes reaaliaikaisia tietoja. Operaattori voi esimerkiksi tarkistaa erityisesti pumppaus station tila ja katso kaikki ilmoitukset järjestelmä käynnistyy. Tämän tason mahdollistaa myös integroinnin IoT ratkaisun Taustatietokannassa aiemmin rivin liiketoiminnan sovellusten ja useiden tai yrityksen liiketoimintaprosesseja työnkulkujen kanssa. Esimerkiksi ehkäisevän kunnossapidon ratkaisu voidaan integroida ajoitusjärjestelmä, poistokirjojen suunnittelijan käymään pumping station, kun ratkaisu on tunnistaa pumppu tarvitsee huoltoa.

![IoT ratkaisun dashboard][img-dashboard]

[img-solution-architecture]: ./media/iot-azure-and-iot/iot-reference-architecture.png
[img-dashboard]: ./media/iot-azure-and-iot/iot-suite.png

[lnk-machinelearning]: http://azure.microsoft.com/documentation/services/machine-learning/
[Azure IoT Suite]: http://azure.microsoft.com/solutions/iot
[lnk-protocol-gateway]:  ../articles/iot-hub/iot-hub-protocol-gateway.md
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf

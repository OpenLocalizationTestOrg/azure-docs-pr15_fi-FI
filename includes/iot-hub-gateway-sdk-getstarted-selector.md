> [AZURE.SELECTOR]
- [Linux](../articles/iot-hub/iot-hub-linux-gateway-sdk-get-started.md)
- [Windows](../articles/iot-hub/iot-hub-windows-gateway-sdk-get-started.md)

Tässä artikkelissa on yksityiskohtaiset PE [Hello World-mallikoodin] [ lnk-helloworld-sample] kuvaamaan [Azure IoT Gateway SDK: N] keskeisiä osia[ lnk-gateway-sdk] arkkitehtuuri. Näytteen käyttää IoT keskitin Gateway SDK rakentaa yksinkertainen gateway, joka kirjaa "Hei" viestin tiedostoon viiden sekunnin välein.

Tässä vaihekuvauksessa käsitellään:

- **Käsitteet**: komponentit, jotka muodostavat kaikki yhdyskäytävä luodaan IoT Gateway SDK Käsitteellinen yleiskatsaus.  
- **Hello World Esimerkki arkkitehtuuri**: kuvaa siitä, miten käsitteet koskevat Hello World Esimerkki ja miten osat sopivat yhteen.
- **Kuinka rakentaa näyte**: näyte rakentaa tarvittavat vaiheet.
- **Suorittaminen näyte**: näyte suorittamiseen tarvittavat vaiheet. 
- **Tyypillinen tulos**: tulos mallin suorittamiseen kertoo esimerkin.
- **Koodikatkelmien**: Hello World Esimerkki siitä, miten toteuttaa gateway tärkeimmät osat näyttämään koodikatkelmien kokoelma.

## <a name="azure-iot-gateway-sdk-concepts"></a>Azure IoT Gateway-SDK liittyvät käsitteet

Ennen tutkii mallikoodin tai luoda oman kentän yhdyskäytävä IoT Gateway SDK: N avulla, sinun on hyvä ymmärtää käsitteistä, ihmisoikeudet SDK: n arkkitehtuuri.

### <a name="modules"></a>Moduulit

Yhdyskäytävällä, jolla Azure IoT Gateway SDK rakentaa luomalla ja kokoaminen *Moduulit*. Moduulien *viestien* avulla voit vaihtaa tietoja toistensa kanssa. Moduuli vastaanottaa viestin, suorittaa jonkin toiminnon, voit myös muuntaa se uuteen viestiin ja julkaisee sitten se voi käsitellä muiden moduulien. Joissakin moduuleissa voi tuottaa vain uudet viestit ja saapuvia viestejä ei koskaan käsitellä. Ketjun moduulien Luo tietojenkäsittelyn pipelinen muunnos suoritetaan yhdessä vaiheessa että pipeline-tiedot kunkin moduulin.

![Ketjun moduulien Azure IoT Gateway SDK on rakennettu yhdyskäytävä][1]
 
SDK sisältää seuraavat:

- Valmiista moduuleja, joita yhteiset yhdyskäytävä toimintoihin.
- Liittymät avulla kehittäjä voi kirjoittaa mukautettuja moduulit.
- Tarpeen ottaa käyttöön ja suorittaa joukon moduulit infrastruktuuri.

SDK sisältää HAL-kerroksen, jonka avulla voit rakentaa yhdyskäytävien toimimaan eri käyttöjärjestelmissä ja ympäristöissä.

![Azure IoT Gateway SDK abstraction layer][2]

### <a name="messages"></a>Viestit

Moduulit, jotka kulkevat viestit toisilleen tietoja ajattelua on kätevä tapa miten gateway-Funktiot conceptualize, vaikka se ei täsmällisemmin, mitä tapahtuu. Moduulien avulla välittäjän keskenään, ne julkaista viestejä broker (väylä, pubsub tai viestinnän kuvio) ja anna broker reitittää viestin sen yhteydessä moduuleihin.

Moduulit **Broker_Publish** -funktiota käyttäen viestin julkaiseminen välittäjän. Välittäjän lähettää viestit kutsumalla takaisinkutsufunktion moduuli. Sanoma koostuu avain-arvo-ominaisuudet ja välittää muistilohkon sisällön.

![Azure IoT Gateway SDK: n välittäjän roolia][3]

### <a name="message-routing-and-filtering"></a>Sanomien reititys ja suodatus

Viestien oikean moduuleihin johtamisesta, kahdella eri tavalla. Linkit voidaan välittää välittäjän välittäjän tietää lähteen ja kunkin moduulin allas tai moduulin voi suodattaa viestin ominaisuuksia. Moduuli ohjeidesi olisi vain viestin, jos se on tarkoitettu viesti. Linkit ja viestin suodatus on mitä Luo viestin pipeline tehokkaasti.

## <a name="hello-world-sample-architecture"></a>Hello World Esimerkki arkkitehtuuri

Hello World-malli on kuvattu edellisessä osassa kuvatut käsitteet. Hello World Esimerkki toteuttaa yhdyskäytävä, jossa on kaksi moduulit koostuvat pipeline:

-   *Hei kaikki* -moduuli Luo viestin viiden sekunnin välein ja välittää sen logger-moduuli.
-   *Lokin* moduulin kirjoittaa tiedostoon saamansa viestit.

![Arkkitehtuuri perustuu Azure IoT Gateway SDK Hello World Esimerkki][4]

Kuten kuvattu edellisessä jaksossa, Hello World-moduuli ei välitä viestejä suoraan logger-moduulin viiden sekunnin välein. Sen sijaan se julkaisee viestin välittäjän viiden sekunnin välein.

Lokin moduuli vastaanottaa viestin välittäjän ja toimii, viestin sisältö kirjoitetaan tiedostoon.

Logger-moduulin kuluttaa vain välittäjän viestit, se julkaisee uusia viestejä ei koskaan välittäjän.

![Miten broker reitittää sanomat Azure IoT Gateway SDK: n moduulien välillä][5]

Yllä kuvassa Hello World Esimerkki ja suhteellisia polkuja, jotka toteuttavat eri osia näytteen [tietovaraston]lähdetiedostojen arkkitehtuurin[lnk-gateway-sdk]. Koodi tutkia tai käyttää apuna alla koodikatkelmia.

<!-- Images -->
[1]: media/iot-hub-gateway-sdk-getstarted-selector/modules.png
[2]: media/iot-hub-gateway-sdk-getstarted-selector/modules_2.png
[3]: media/iot-hub-gateway-sdk-getstarted-selector/messages_1.png
[4]: media/iot-hub-gateway-sdk-getstarted-selector/high_level_architecture.png
[5]: media/iot-hub-gateway-sdk-getstarted-selector/detailed_architecture.png

<!-- Links -->
[lnk-helloworld-sample]: https://github.com/Azure/azure-iot-gateway-sdk/tree/master/samples/hello_world
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk
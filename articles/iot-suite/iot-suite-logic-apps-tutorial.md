<properties
  pageTitle="Azure IoT ohjelmistopaketissa ja logiikka sovellusten | Microsoft Azure"
  description="Opetusohjelma siitä, miten voit yhdistää logiikan sovellusten Azure IoT ohjelmistopakettiin Liiketoimintaprosessi varten."
  services=""
  suite="iot-suite"
  documentationCenter=""
  authors="aguilaaj"
  manager="timlt"
  editor=""/>

<tags
  ms.service="iot-suite"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="na"
  ms.date="08/16/2016"
  ms.author="araguila"/>
  
# <a name="tutorial-connect-logic-app-to-your-azure-iot-suite-remote-monitoring-preconfigured-solution"></a>Opetusohjelma: Yhdistäminen logiikan App Azure IoT Suite Remote seuranta valmiiksi ratkaisu

[Microsoft Azure IoT Suite] [ lnk-internetofthings] remote seuranta esimääritettyjä ratkaisu on erinomainen tapa pääset nopeasti alkuun lopusta loppuun-toiminto-joukko, jonka exemplifies IoT-ratkaisun. Tässä opetusohjelmassa esitellään logiikan sovelluksen lisääminen oman Microsoft Azure IoT Suite remote seuranta esimääritettyjä ratkaisu. Nämä vaiheet kuvaavat IoT ratkaisu täsmentää ottamaan yhdistämällä liiketoimintaprosessien.

_Jos etsit vaiheittainen valmistelutoimet remote seuranta valmiiksi ratkaisun käyttöön, katso [Opetusohjelma: Aloita IoT valmiiksi ratkaisuja][lnk-getstarted]._

Ennen kuin aloitat Tässä opetusohjelmassa, sinun on:

- Valmistele remote seurantaa esimääritettyjä ratkaisu Azure-tilaukseesi.

- Luo SendGrid tili, jotta voit lähettää sähköpostia, joka käynnistää liiketoimintaprosessin. Voit rekisteröidä ilmainen kokeiluversio tili osoitteessa [SendGrid](https://sendgrid.com/) valitsemalla **Kokeile ilmaiseksi**. Kun olet rekisteröinyt ilmainen kokeiluversio-tilin, sinun on Luo SendGrid, joka myöntää käyttöoikeuksia sähköpostin [Ohjelmointirajapinnan avain](https://sendgrid.com/docs/User_Guide/Settings/api_keys.html) . Tarvitset Ohjelmointirajapinnan avain myöhemmin opetusohjelman.

Oletetaan, että olet jo valmisteltu remote seuranta valmiiksi ratkaisu, siirry [Azure portal]kyseisen ratkaisun resurssiryhmä[lnk-azureportal]. Resurssiryhmä on sama nimi kuin ratkaisun nimeä valitsit kun remote seurantaa ratkaisu on valmisteltu. Resurssiryhmän näet valmistellun Azure resurssien ratkaisun muutosaluetta lukuun ottamatta Azure Active Directory-sovellus, josta löydät perinteinen Azure-portaalissa. Seuraavassa näyttökuvassa näkyy esimerkki **resurssiryhmä** -sivu remote seurantaa esimääritettyjä ratkaisua:

![](media/iot-suite-logic-apps-tutorial/resourcegroup.png)

Aloita määrittäminen logiikan sovellus käyttää ennalta määritettyjä-ratkaisuun.

## <a name="set-up-the-logic-app"></a>Logiikan-sovelluksen määrittäminen

1. Valitse __Lisää__ resurssien ryhmä-sivu Azure-portaalissa yläreunassa.

2. Etsi __Logiikan sovelluksen__, valitse se ja valitse sitten **Luo**.

3. Täytä __nimi__ ja käytä samaa **tilaus** ja **resurssiryhmä** , jota käytit, kun remote seurantaa ratkaisu on valmistelun yhteydessä. Valitse __Luo__.

    ![](media/iot-suite-logic-apps-tutorial/createlogicapp.png)

4. Kun käyttöönoton on valmis, näet logiikan sovellus näkyy resurssiryhmän resurssiksi.

5. Napsauta logiikan-sovellusta, siirry logiikan sovellus-sivu, valitse Avaa **Logiikan sovellusten Designer** **Tyhjä logiikan sovelluksen** -malli.

    ![](media/iot-suite-logic-apps-tutorial/logicappsdesigner.png)

6. Valitse __pyyntö__. Tämä toiminto määrittää, että saapuvat HTTP-pyyntö, jonka tietyn JSON muotoiltuna paketti säädösten käynnistintä.

7. Liitä pyytää leipätekstin JSON rakenteen seuraavasti:

    ```
    {
      "$schema": "http://json-schema.org/draft-04/schema#",
      "id": "/",
      "properties": {
        "DeviceId": {
          "id": "DeviceId",
          "type": "string"
        },
        "measuredValue": {
          "id": "measuredValue",
          "type": "integer"
        },
        "measurementName": {
          "id": "measurementName",
          "type": "string"
        }
      },
      "required": [
        "DeviceId",
        "measurementName",
        "measuredValue"
      ],
      "type": "object"
    }
    ```
    
    > [AZURE.NOTE] Voit kopioida HTTP post URL-osoite, kun tallennat logiikan sovelluksen, mutta sinun on ensin lisättävä toiminnon.

8. Valitse __+ Uusi vaihe__ kohdasta manuaalinen käynnistäminen. Valitse **Lisää toiminnon**.

    ![](media/iot-suite-logic-apps-tutorial/logicappcode.png)

9. Etsi **SendGrid - sähköpostin lähettäminen** ja napsauttamalla sitä.

    ![](media/iot-suite-logic-apps-tutorial/logicappaction.png)

10. Yhteys-, kuten **SendGridConnection**nimi, kirjoita **SendGrid Ohjelmointirajapinnan avain** , voit luoda, kun SendGrid-tilin määrittäminen ja sitten **Luo**.

    ![](media/iot-suite-logic-apps-tutorial/sendgridconnection.png)

11. Lisää sähköpostiosoitteet omistat sekä **mistä** - ja **mihin** -kenttiin. Lisää **Remote seuranta ilmoitus [DeviceId]** **Aihe** -kenttään. Lisää **Sähköpostiviestin teksti** -kenttään **laitteen [DeviceId] on raportoinut [measurementName] [measuredValue] arvona.** Voit lisätä **[DeviceId]**, **[measurementName]**ja **[measuredValue]** napsauttamalla **Voit lisätä tietoja aiemmissa vaiheissa** -osassa.

    ![](media/iot-suite-logic-apps-tutorial/sendgridaction.png)

12. Valitse yläreunan-valikossa __Tallenna__ .

13. Valitse **Pyydä** käynnistin ja kopioi __Http Post tätä URL-osoite__ -arvo. Tarvitset tätä URL-Osoitetta jäljempänä tässä opetusohjelmassa.

> [AZURE.NOTE] Logiikan-sovellusten avulla voit suorittaa [toiminnon monista erilaisista] [ lnk-logic-apps-actions] mukaan lukien toiminnot Office 365: ssä. 

## <a name="set-up-the-eventprocessor-web-job"></a>EventProcessor Web-projektin määrittäminen

Tässä osassa voit muodostaa esimääritettyjä ratkaisu luomasi logiikan-sovellukseen. Tämän tehtävän suorittamiseen voit lisätä toiminnon, joka käynnistyy, kun laite tunnistimen arvon kynnysarvo logiikan sovelluksen käynnistäminen URL-osoite.

1. Kloonaa uusimman version git asiakkaan avulla [azure-iot-remote-seuranta github säilöön][lnk-rmgithub]. Esimerkki:

    ```
    git clone https://github.com/Azure/azure-iot-remote-monitoring.git
    ```

2. Avaa Visual Studion __RemoteMonitoring.sln__ säilö paikallisen kopion.

3. Avaa tiedoston __ActionRepository.cs__ **infrastruktuurin\\säilöön** kansio.

4. Päivitä **actionIds** sanaston __Http kirjaa tätä URL-Osoitetta__ , merkille logiikan sovelluksesta seuraavasti:

    ```
    private Dictionary<string,string> actionIds = new Dictionary<string, string>()
    {
        { "Send Message", "<Http Post to this UR>" },
        { "Raise Alarm", "<Http Post to this UR> }
    };
    ```

5. Tallenna muutokset ratkaisussa ja Lopeta Visual Studio.

## <a name="deploy-from-the-command-line"></a>Ottaa käyttöön komentoriviltä

Tässä osassa voit ottaa käyttöön tällä hetkellä käytössä Azure versio korvaa remote seurantaa ratkaisu päivitetty versio.

1. Seuraavan [keskihajonta määritetään] [ lnk-devsetup] käyttöönottoa varten-ympäristön määritys ohjeita.

2.  Ottamaan paikallisesti noudattamalla [paikallisen käyttöönoton] [ lnk-localdeploy] ohjeita.

3.  Ota käyttöön pilvessä ja Päivitä aiemmin cloud käyttöönoton, noudattamalla [cloud käyttöönoton] [ lnk-clouddeploy] ohjeita. Käytä alkuperäisen käyttöönoton käyttöönoton nimeksi. Esimerkki Jos alkuperäisen käyttöönoton kutsuttiin **demologicapp**, käytä seuraavaa komentoa:

    ``
    build.cmd cloud release demologicapp
    ``
    
    Muodosta komentosarjaa suoritettaessa muista saman Azure-tili, tilauksen, alue ja käytit, kun ratkaisun valmisteltu Active Directory-esiintymä.

## <a name="see-your-logic-app-in-action"></a>Artikkelissa logiikan sovelluksen-toiminto

Remote seurantaa esimääritettyjä ratkaisu on oletusarvoisesti määrittäminen, kun ratkaista valmistella kaksi sääntöä. Molemmat säännöt ovat **SampleDevice001** laitteeseen:

* Lämpötila > 38.00
* Kosteus > 48.00

Lämpötila säännön käynnistää **Nosta hälytys** toiminnon ja kosteus säännön käynnistää **SendMessage** -toiminnon. Jos olet käyttänyt molemmat toiminnot URL-Osoitetta **ActionRepository** luokan, logiikan sovelluksen käynnistää joko sääntöä. Molempien sääntöjen SendGrid avulla voit lähettää sähköpostia **,** osoite yksityiskohdilla ilmoituksen.

> [AZURE.NOTE] Logiikan sovelluksen käynnistäminen aina, kun kynnysarvo täyttyy säilyy. Tarpeettomien sähköpostit välttämiseksi voit käytöstä sääntöjä ratkaisu-portaalissa tai käytöstä logiikan [Azure portal]-sovellus[lnk-azureportal].

Lisäksi saa sähköpostiviestejä, näet myös kun logiikan-sovellus toimii portaalissa:

![](media/iot-suite-logic-apps-tutorial/logicapprun.png)

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet käyttänyt logiikan sovelluksen esimääritettyjä ratkaisu muodostaa liiketoimintaprosessien, Lisätietoja on esimääritetyt ratkaisut diaesitysten asetuksista:

- [Dynaaminen telemetriatietojen käyttäminen remote seurantaa esimääritettyjä ratkaisu][lnk-dynamic]
- [Laitteen tiedot metatiedot remote seurannassa valmiiksi ratkaisu][lnk-devinfo]

[lnk-dynamic]: iot-suite-dynamic-telemetry.md
[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[lnk-internetofthings]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-getstarted]: iot-suite-getstarted-preconfigured-solutions.md
[lnk-azureportal]: https://portal.azure.com
[lnk-logic-apps-actions]: ../connectors/apis-list.md
[lnk-rmgithub]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-devsetup]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/dev-setup.md
[lnk-localdeploy]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/local-deployment.md
[lnk-clouddeploy]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/cloud-deployment.md

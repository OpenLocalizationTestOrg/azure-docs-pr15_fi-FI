<properties
   pageTitle="Opetusohjelma: Käsittele EDIFACT laskujen Azure BizTalk Services-palvelujen avulla | Microsoft Azure BizTalk palvelut"
   description="Voit luoda ja määritä ruutuun yhdistimen tai API-sovellus ja käytä Azure-sovelluksen palvelun logiikan-sovelluksessa"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="msftman"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="05/31/2016"
   ms.author="deonhe"/>

# <a name="tutorial-process-edifact-invoices-using-azure-biztalk-services"></a>Opetusohjelma: Prosessin EDIFACT laskut Azure BizTalk Services-palvelujen avulla
Voit määrittää ja ottaa X12 ja EDIFACT sopimusten BizTalk palveluiden portaali. Tässä opetusohjelmassa on Tarkista luomisesta EDIFACT sopimuksia siirtämisen laskujen kaupan kumppanien välillä. Tässä opetusohjelmassa kirjoitetaan henkilöihin liittyvät kaksi kaupan kumppaneita, joka vaihtaa EDIFACT viestejä Northwind ja Contoso-kattavan business-ratkaisun ympärille.  

## <a name="sample-based-on-this-tutorial"></a>Esimerkki Tässä opetusohjelmassa perusteella
Tässä opetusohjelmassa kirjoitetaan ympärille otoksen **EDIFACT laskut käyttämällä BizTalk Services lähettäminen**, joka on ladattavissa [MSDN koodin valikoima](http://go.microsoft.com/fwlink/?LinkId=401005). Voit käyttää valmista ja käydä läpi opetusohjelman selvittääksesi, miten malli on luotu. Tai tämän opetusohjelman avulla voi luoda omia ratkaisu maasta ylöspäin. Tässä opetusohjelmassa suunnataan toisen lähestymistavan, jotta ymmärrät, miten tämä ratkaisu on luotu. Myös mahdollisimman hyvin, opetusohjelman otosten ulkoasultaan ja käyttää samoja nimiä palvelutiedot (esimerkiksi rakenteita, muunnoksia) käytetään otoksessa.  

>[AZURE.NOTE] Koska tätä ratkaisua käsittää viestin lähettämistä EAI-siltauksesta Muokkaa silta, käyttää uudelleen [BizTalk Services silta ketjutus malli](http://code.msdn.microsoft.com/BizTalk-Bridge-chaining-2246b104) -malli.  

## <a name="what-does-the-solution-do"></a>Mitä ratkaisun tarkoittaa?

Tämän ratkaisun Northwind saa EDIFACT laskujen Contoso. Nämä laskut eivät ole Muokkaa vakiotiedostomuoto. Näin on ennen kuin lähetät laskun Northwind, se on muunnettava EDIFACT laskun (kutsutaan myös INVOIC) asiakirjaan. Kuitissa Northwind on käsitellä EDIFACT lasku ja palaa Contoso valvonta-sanoma (kutsutaan myös CONTRL).

![][1]  

Tavoitteet business-skenaarion Contoso käyttää Microsoft Azure BizTalk Servicesin ominaisuudet.

*   Contoson käyttää EAI siltoja muuntamiseen EDIFACT INVOIC alkuperäinen lasku.

*   EAI siltaan lähettää viestin Muokkaa Lähetä silta käyttöön määritetty BizTalk palvelut-portaalissa sopimuksen osana.

*   Muokkaa Lähetä käsittelee EDIFACT INVOIC ja reitittää Northwind.

*   Laskun saatuaan Northwind palaa CONTRL viestin Muokkaa saa silta käyttöön sopimuksen osana.  

> [AZURE.NOTE] Voit myös tätä ratkaisua myös esitellään, miten laskut lähetetään erissä sen sijaan, että kunkin laskun lähettäminen erikseen jonottaminen avulla.  

Viimeistele skenaariota, Käytämme palvelun Bus olevien laskun lähettäminen Northwind Contoso tai vastaanottamiseen kuittaus Northwind-tietokannasta. Nämä olevien voidaan luoda avulla asiakassovellus, joka on ladattavissa ja sisältyy esimerkkipakkaus, joka on saatavana osana tässä opetusohjelmassa.  

## <a name="prerequisites"></a>Edellytykset

*   Sinulla on oltava palvelun Bus nimitila. Ohjeita nimitilan luomisesta on artikkelissa [How To: Voit luoda tai muokata palvelun Bus Service-Namespace](https://msdn.microsoft.com/library/azure/hh674478.aspx). Uutiskirjeistä oletetaan, että sinulla on jo valmisteltu, palvelun Bus nimitilan kutsutaan **edifactbts**.

*   Sinulla on oltava BizTalk palvelut-tilaus. Katso ohjeet [Luo BizTalk-palvelun avulla Azure perinteinen portal](http://go.microsoft.com/fwlink/?LinkID=302280). Tässä opetusohjelmassa uutiskirjeistä oletetaan BizTalk Services tilauksesi **contosowabs**.

*   Rekisteröi BizTalk palvelut-tilauksesi BizTalk palvelut-portaalissa. Katso ohjeet [rekisteröiminen BizTalk palvelun käyttöönoton BizTalk palvelut-portaalissa](https://msdn.microsoft.com/library/hh689837.aspx)

*   Sinulla on oltava asennettuna Visual Studio.

*   Sinulla on oltava BizTalk Services SDK-paketissa. Voit ladata SDK [http://go.microsoft.com/fwlink/?LinkId=235057](http://go.microsoft.com/fwlink/?LinkId=235057)  

## <a name="step-1-create-the-service-bus-queues"></a>Vaihe 1: Luo palvelun Bus olevien  
Tämä ratkaisu käyttää palvelun Bus olevien lähettäminen kaupan kumppanien välillä. Contoson ja Northwind lähettää viestejä olevien-kohtaa, johon EAI-ja/tai Muokkaa siltoja tarjoaman ne. Tämä ratkaisu on kolme palvelua Bus olevien:

*   **northwindreceive** – Northwind saa laskun Contoso Tämä jono päälle.

*   **contosoreceive** – Contoso saa kuittaus Northwind Tämä jono päälle.

*   **keskeytetty** – kaikki hyllytetty Sanomat reititetään Tämä jono. Viestit ovat hyllytetty, jos ne eivät toimi käsiteltäessä.

Voit luoda nämä palvelun Bus olevien esimerkkipakkaus sisältyvät asiakassovellus avulla.  

1.  Avaa sijainti, johon olet ladannut otosten- **Opetusohjelma lähettäminen laskut käyttämällä BizTalk Services Muokkaa Bridges.sln**.

2.  Painamalla **F5** voivat laatia ja Käynnistä **Opas** asiakassovellukseen.

3.  Kirjoita näytön palvelun Bus ACS nimitilan, myöntäjän nimi ja myöntäjä-näppäintä.

    ![][2]  
4.  Sanomaruutu, jossa kehotetaan kolme olevien luodaan palvelun Bus nimitilan. Valitse **OK**.

5.  Jätä käyttävä opetusohjelma-asiakas. Avaa, valitse **Palvelu Bus** > **_palvelun Bus nimitilan_** > **olevien**, ja varmista, että kolme olevien luodaan.  

## <a name="step-2-create-and-deploy-trading-partner-agreement"></a>Vaihe 2: Luo ja ota käyttöön kaupan kumppanisopimus
Luo kaupan kumppanisopimus Contoso ja Northwind välillä. Kaupan kumppanin sopimuksen määrittää kaupan sopimuksen välillä kaksi liikekumppanien, kuten mitkä viestin rakenteen käyttäminen, mikä tekstiviesti protokolla kannattaa käyttää, jne. Kaupan kumppanisopimus sisältää kaksi Muokkaa siltoja, yksi viestien lähettämiseen kaupan kumppaneille (jota kutsutaan **Muokkaa Lähetä silta**) ja toinen vastaanottaa viestejä kaupan yhteistyökumppanien (jota kutsutaan **Muokkaa vastaanottaa silta**).

Tämä ratkaisu kontekstissa Muokkaa Lähetä vastaa sopimuksen Lähetä-puolella ja käytetään EDIFACT laskun lähettäminen Northwind Contoso. Vastaavasti Muokkaa vastaanota vastaa vastaanota side sopimuksen ja vastaanota kuittaussanomien käytetään Northwind-tietokannasta.  

### <a name="create-the-trading-partners"></a>Luo kaupan kumppaneille

Luo aluksi kaupan kumppanien Contoson ja Northwind.  

1.  BizTalk-palvelut-portaalissa **kumppaneille** -välilehdessä valitsemalla **Lisää**.

2.  Kumppanin uuden sivun Syötä **Contoso** kumppanin nimi ja valitse sitten **Tallenna**.

3.  Toista vaihe toisen kumppanin **Northwind**luomiseen.  

### <a name="create-the-agreement"></a>Sopimuksen luominen
Kaupan kumppanin toimeenpano luodaan business-profiileista kaupankäyntipäivä kumppaneille. Tämä ratkaisu käyttää oletusarvon kumppanin-profiileista, jotka luodaan automaattisesti, kun kumppanien luomaasi.  

1.  Valitse BizTalk palvelut-portaalissa **sopimusten** > **Lisää**.

2.  Uuden sopimuksen **Yleisasetukset** -sivun Määritä arvot alla olevassa kuvassa esitetyllä tavalla ja valitse sitten **Jatka**.

    ![][3]  

    Kun valitset **Jatka**, **Vastaanottaminen** ja **Lähettää asetusten** välilehtien ole käytettävissä.

3.  Luo Contoso ja Northwind Lähetä sopimus. Tämä sopimus koskee, miten Contoso lähettää EDIFACT laskun Northwind.

    1.  Valitse **Lähetä asetukset**.

    2.  Säilytä oletusarvot **Saapuva URL-osoite**, **Muunna**ja **Batching** -välilehdissä.

    3.  Lataa **EFACT_D93A_INVOIC.xsd** rakenteen **protokolla** -välilehden **Mallit** -osassa. Tämä rakenne on käytettävissä esimerkkipakkaus.

        ![][4]  
    4.  Määritä Service Bus olevien tiedot **Transport** -välilehdessä. Lähetä Include, sopimuksen Käytämme **northwindreceive** jonossa EDIFACT laskun lähettäminen Northwind ja **hyllytetty** jonossa reitittämään kaikki viestit, joissa käsiteltäessä epäonnistua ja keskeytetään. Sinun on luotu- **Vaihe 1: Luo palvelun Bus olevien** (tässä kohdassa).

        ![][5]  

        Valitse **Transport asetukset > Transport tyyppi** ja **keskeyttäminen viestiasetukset > Transport tyyppi**ja valitse Azure palvelun Bus sisältävät arvot kuvassa esitetyllä tavalla.

4.  Luo Contoso ja Northwind vastaanota sopimus. Tämä sopimus koskee miten Contoso vastaanottaa Northwind kuittausta.

    1.  Valitse **Vastaanota asetukset**.

    2.  Säilytä oletusarvot **Transport** ja **muuntaminen** -välilehdissä.

    3.  Lataa **EFACT_4.1_CONTRL.xsd** rakenteen **protokolla** -välilehden **Mallit** -osassa. Tämä rakenne on käytettävissä esimerkkipakkaus.

    4.  Valitse **reitti** -välilehdessä voit varmistaa, että vain kuittaussanomien Northwind-tietokannasta reititetään Contoso suodattimen luominen. Valitse **Reitin asetukset**-kohdassa **Lisää** reititys suodattimen luominen.

        ![][6]  
        1.  Anna arvot **Säännön nimi**, **reitin sääntö**ja **reitin kohde** kuvassa esitetyllä tavalla.

        2.  Valitse **Tallenna**.

    5.  **Reititys** -välilehden Määritä uudelleen, jossa keskeytetty kuittaussanomien (jotka eivät läpäise käsiteltäessä kuittaussanomien) reititetään. Määritä transport tyypiksi Azure palvelun Bus, reitittää kohteen tyyppi **jonossa**, todennustyyppi **Jaettu Access allekirjoitus** (SAS), anna palvelun Bus nimitilan SAS yhteysmerkkijono ja kirjoita sitten **keskeytetty**jonon nimi.

5.  Valitse lopuksi **Ota** käyttöön sopimuksen. Huomaa päätepisteet jossa Lähetä ja vastaanota toimeenpano tule käyttöön.

    *   **Lähetyksen asetukset** -välilehden **Saapuva URL-osoite**Kirjoita muistiin päätepiste. Voit lähettää viestin Contoso käyttämällä Muokkaa Lähetä siltaan Northwind voit lähettää viestin tämä päätepiste.

    *   Valitse **Vastaanottoasetuksia** -välilehden **Transport**Huomautus päätepiste. Sähköpostin lähettäminen Northwind Contoso avulla Muokkaa vastaanottaa sillan, voit lähettää viestin tämä päätepiste.  

## <a name="step-3-create-and-deploy-the-biztalk-services-project"></a>Vaihe 3: Luo ja ota BizTalk Services-projekti

Edellisessä vaiheessa käyttöön Muokkaa lähettää ja vastaanottaa sopimusten EDIFACT laskuja ja kuittaussanomien. Näiden sopimusten voidaan käsitellä vain viestejä, jotka vakio EDIFACT viestin rakenteen mukainen. Kuitenkin tämä ratkaisu skenaariossa kohti Contoso lähettää laskun Northwind sisäiset omistusoikeuksia rakenteessa. Siis ennen viestin lähettämistä Muokkaa Lähetä siltaan, se on muunnettava sisäiset rakenteesta vakio EDIFACT laskun rakenteeseen. BizTalk palvelujen EAI project tekee.

BizTalk Services-projektin **InvoiceProcessingBridge**, joka muuntaa viesti on myös lataamasi malli sisältää. Projektin sisältää seuraavat tiedot:

*   **INHOUSEINVOICE. XSD** – sisäiset lasku, joka lähetetään Northwind-rakennetta.

*   **EFACT_D93A_INVOIC. XSD** – vakio EDIFACT laskun rakenne.

*   **EFACT_4.1_CONTRL. XSD** – EDIFACT kuittaus, joka lähettää Northwind Contoso-rakennetta.

*   **INHOUSEINVOICE_to_D93AINVOIC. TRFM** – muunnoksen, joka liittää sisäiset laskun rakenteen vakio EDIFACT laskun rakenteeseen.  

### <a name="create-the-biztalk-services-project"></a>Luo BizTalk Services-projekti
1.  Laajenna InvoiceProcessingBridge projektin Visual Studio-ratkaisun ja avaa sitten **MessageFlowItinerary.bcs** -tiedosto.

2.  Napsauta mitä tahansa kangasta ja määritä **BizTalk palvelun URL-osoite** -ominaisuuden ruutuun voit määrittää BizTalk Services tilauksen nimi. Esimerkiksi `https://contosowabs.biztalk.windows.net`.

    ![][7]  
3.  Vedä **Xml One-Way silta** työkaluryhmästä alusta. Määritä **Kohteen nimi** ja **Osoite suhteellinen** ominaisuuksiksi mittasillan **ProcessInvoiceBridge**. Kaksoisnapsauta Avaa silta määritys pinta **ProcessInvoiceBridge** .

4.  **Viestin tyypit** -valintaruutu napsauttamalla plus (**+**) Määritä saapuvan viestin rakenne-painiketta. Saapuvan viestin lähettämistä EAI-sillan on aina sisäiset lasku, koska tämä arvoksi **INHOUSEINVOICE**.

    ![][8]  
5.  **Xml-muunnos** -muotoa ja valitse ominaisuusruutu **Maps** -ominaisuutta, pistettä (****...). Valitse **Kartat valinta** -valintaikkunassa valitse **INHOUSEINVOICE_to_D93AINVOIC** muunnos-tiedosto ja valitse sitten **OK**.

    ![][9]  
6.  Siirry takaisin **MessageFlowItinerary.bcs**ja työkaluryhmästä, vedä **Two-Way ulkoisen Palvelupäätepisteen** **ProcessInvoiceBridge**oikealla. Sen **Kohteen nimi** -ominaisuuden arvoksi **EDIBridge**.

7.  Napsauta ratkaisunhallinnassa Laajenna **MessageFlowItinerary.bcs** ja **EDIBridge.config** -tiedostoa. Korvaa **EDIBridge.config** sisällön seuraavasti.

    > [AZURE.NOTE] Miksi .config-tiedostoa muokata tarvitaan? Ulkoisen Palvelupäätepisteen, jotka on lisätty silta suunnittelutyökalun alusta edustaa Muokkaa siltoja, jotka on otettu käyttöön aiemmin. Muokkaa siltoja ovat kaksisuuntainen siltoja, ja Lähetä ja vastaanota side. EAI sillan, joka on lisätty silta suunnittelija on kuitenkin yksisuuntainen siltaa. Käsittelemään kaksi siltoja, toista viestiä exchange-kuvioiden Käytämme mukautetun silta toiminnan lisäämällä sen määritys .config-tiedostossa. Lisäksi mukautettuja toimintoja käsittelee myös todennus Muokkaa Lähetä silta päätepisteelle. Mukautetun toiminta on käytettävissä eri otoksen osoitteessa [BizTalk Services silta ketjutus malli - EAI Muokkaa](http://code.msdn.microsoft.com/BizTalk-Bridge-chaining-2246b104). Tämä ratkaisu käyttää otosten uudelleen.  
    
    ```
<?xml version="1.0" encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <extensions>
          <behaviorExtensions>
            <add name="BridgeAuthentication" 
                  type="Microsoft.BizTalk.Bridge.Behaviour.BridgeBehaviorElement, Microsoft.BizTalk.Bridge.Behaviour, Version=1.0.0.0, Culture=neutral, PublicKeyToken=ae58f69b69495c05" />
          </behaviorExtensions>
        </extensions>
        <behaviors>
          <endpointBehaviors>
            <behavior name="BridgeAuthenticationConfiguration">
              <!-- Enter the ACS namespace, issuer name and issuer secret of the BizTalk Services deployment -->
              <BridgeAuthentication acsnamespace="[YOUR ACS NAMESPACE]" 
                                    issuername="owner" 
                                    issuersecret="[YOUR ACS SECRET]" />
              <webHttp />
            </behavior>
          </endpointBehaviors>
        </behaviors>
        <bindings>
          <webHttpBinding>
            <binding name="BridgeBindingConfiguration">
              <security mode="Transport" />
            </binding>
          </webHttpBinding>
        </bindings>
        <client>
          <clear />
          <!--
            Go BizTalk Portal > Agreement > Send Settings > Inbound URL
            Copy the Endpoint URL and paste it in the below address field
          -->
          <endpoint name="TwoWayExternalServiceEndpointReference1" 
                    address="[YOUR EDI BRIDGE SEND URI]" 
                    behaviorConfiguration="BridgeAuthenticationConfiguration" 
                    binding="webHttpBinding" 
                    bindingConfiguration="BridgeBindingConfiguration" 
                    contract="System.ServiceModel.Routing.IRequestReplyRouter" />
        </client>
      </system.serviceModel>
    </configuration>

    ```
8.  Päivitä EDIBridge.config-tiedosto, joka sisältää tiedot

    *   Valitse _<behaviors>_, ACS nimitilan ja BizTalk palvelut-tilaukseen liittyvää avain.

    *   Valitse _<client>_, anna päätepiste, jossa Muokkaa Lähetä sopimus on otettu käyttöön.

    Tallenna muutokset ja sulje määritystiedostoa.

9.  Työkaluryhmästä Valitse **yhdistin** ja liittyä **ProcessInvoiceBridge** ja **EDIBridge** osat. Valitse yhdistin ja valitse Ominaisuudet-ruudussa tilaksi **Suodatusehtoja** **Vastaa kaikkia**. Näin varmistat, että kaikki viestit käsitellään EAI silta reititetään Muokkaa siltaan.

    ![][10]  
10.  Tallenna muutokset ratkaisu.  

### <a name="deploy-the-project"></a>Ota käyttöön-projekti

1.  Tietokoneessa, jossa BizTalk Services-projekti on luotu ja lataa ja asenna BizTalk palvelut-tilauksen SSL-varmenne. BizTalk palvelut, valitse **raporttinäkymät-ikkunan**, ja valitse sitten **Lataa SSL-varmenne**. Kaksoisnapsauta varmennetta ja noudata kehotteen ja viimeistele asennus. Varmista, että olet asentanut **Luotettujen päämyöntäjien** varmenteen store-kohdan varmennetta.

2.  Napsauta Visual Studio ratkaisunhallinnassa **InvoiceProcessingBridge** projektin hiiren kakkospainikkeella ja valitse sitten **Ota käyttöön**.

3.  Arvot kuvassa esitetyllä tavalla, ja valitse sitten **Ota käyttöön**. Saat ACS tunnistetiedot BizTalk palveluiden valitsemalla **Yhteystiedot** BizTalk Services-koontinäytöstä.

    ![][11]  

    Kopioi tulosteen-ruudussa päätepiste, jossa EAI siltaan on otettu käyttöön, esimerkiksi `https://contosowabs.biztalk.windows.net/default/ProcessInvoiceBridge`. Tarvitset tätä päätepisteen URL-Osoitetta myöhemmässä vaiheessa.  

## <a name="step-4-test-the-solution"></a>Vaihe 4: Testaa ratkaisu


Tässä ohjeaiheessa on Tarkista testaaminen ratkaisun avulla otosten osana **Opetusohjelma** -asiakassovellukseen.  

1.  Visual Studiossa painamalla F5-näppäintä Käynnistä **Opetusohjelma asiakas**.

2.  Näytön on oltava vaiheesta, johon arvot, jossa on luotu palvelun Bus olevien. Valitse **Seuraava**.

3.  Antaa ACS tunnistetietojen BizTalk Services tilaus ja päätepisteet seuraavaan ikkunaan, jossa EAI ja Muokkaa (vastaanotto) siltoja on otettu käyttöön.

    Oli kopioinut EAI silta päätepisteen edellisessä vaiheessa. Saat Muokkaa silta päätepisteen BizTalk palvelut-portaalissa, siirry sopimuksen > vastaanottoasetuksia > Transport > päätepiste.

    ![][12]  
4.  Valitse seuraavassa ikkunassa Contoso, **Lähetä sisäiset lasku** -painike. Tiedoston avata valintaikkunan, Avaa INHOUSEINVOICE.txt-tiedosto. Tiedoston sisällöstä ja valitse **OK** , jos haluat lähettää laskun.

    ![][13]  
5.  Laskun vastaanotetaan muutaman sekunnin kuluttua Northwind. **Näytä viesti** -linkki on vastaanottanut Northwind lasku napsauttamalla. Huomaa, miten Northwind vastaanottanut lasku on vakio määritelty EDIFACT Contoso lähettämä yksi ollessa sisäiset rakenne.

    ![][14]  
6.  Valitse lasku ja valitse sitten **Lähetä vastausta**. Valitse näkyviin tulevassa valintaikkunassa Huomaa, että Interchange Format-tunnus on sama vastaanotetun laskun ja lähetetty kuittaus. Valitse **Lähetä kuittaus** -valintaikkunassa OK.

    ![][15]  
7.  Muutaman sekunnin kuluttua kuittaus onnistuneesti vastaanotetaan Contoso.

    ![][16]  

## <a name="step-5-optional-send-edifact-invoice-in-batches"></a>Vaihe 5 (valinnainen): Lähetä EDIFACT laskun erissä 
BizTalk Services Muokkaa siltoja tukee myös jonottaminen lähtevien viestien. Tämä ominaisuus on hyödyllinen kumppanit, jotka mieluiten viestien (tietyt kriteerit) sen sijaan, että yksittäisten viestien vastaanottamiseen.

Tärkeimmät kuvasuhteen, kun käsittelet erissä on todellinen erä kutsutaan myös release ehdot-versiossa. Miten vastaanottava kumppani haluaa vastaanottaa viestejä voi perustua release ehdot. Jos jonottaminen on käytössä, Muokkaa siltaan Lähetä lähtevän viestin vastaanottava kumppani, kunnes release ehto täyttyy. Esimerkiksi batching ehtojen perusteella viestin koon toimitusten erän vain silloin, kun tiekuljetuksiin tarkoitettuihin n viestit ovat erämuotoinen. Erän ehto voi olla myös aikapohjaisten, siten, että erä lähetetään kiinteä kerrallaan päivittäin. Tämä ratkaisu Yritämme viestin koko perustuvat ehdot.

1.  Valitse aiemmin luomasi sopimuksen BizTalk palvelut-portaalissa. Valitse lähetysasetukset > jonottaminen > Lisää erä.

2.  Siirtoerän nimi Kirjoita **InvoiceBatch**, kuvaus ja valitse sitten **Seuraava**.

3.  Määritä erä ehdot, joka määrittää, mitkä viestit on erämuotoinen. Tämä ratkaisu on erän kaikkiin viesteihin. Näin on Valitse määritelmät lisäasetus käyttöön ja kirjoita **1 = 1**. Tämä on ehto, joka on aina true ja näin ollen kaikki viestit erämuotoinen. Valitse **Seuraava**.

    ![][17]  
4.  Määritä erä release ehdot. Drop-ruudusta Valitse **MessageCountBased**ja **Laske** **3**. Tämä tarkoittaa, että erän kolme viestit lähetetään Northwind. Valitse **Seuraava**.

    ![][18]  
5.  Tarkista yhteenveto ja valitse sitten **Tallenna**. Valitse **Ota käyttöön** , ota sopimuksen uudelleen.

6.  Palaa **Opetusohjelma asiakas**, valitsemalla **Lähetä sisäiset lasku**ja kehotteiden mukaisesti lähetetään lasku. Huomaat, että ei laskun on saapunut Northwind, koska erän koko ei täyty. Toista tämä vaihe kaksi kertaa, niin, että sinulla on kolme laskun Northwind lähetettyihin viesteihin. Tämä täyttää erän release ehdot 3 viestien ja pitäisi tulla näkyviin laskun Northwind-palvelussa.


<!--Image references-->
[1]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-1.PNG  
[2]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-2.PNG  
[3]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-3.PNG
[4]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-4.PNG  
[5]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-5.PNG  
[6]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-6.PNG  
[7]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-7.PNG  
[8]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-8.PNG
[9]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-9.PNG  
[10]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-10.PNG  
[11]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-11.PNG  
[12]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-12.PNG  
[13]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-13.PNG
[14]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-14.PNG  
[15]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-15.PNG  
[16]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-16.PNG  
[17]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-17.PNG  
[18]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-18.PNG


<properties 
    pageTitle="Lisätietoja yrityksen integrointi Pack toistaa X12 viestin Connctor | Microsoft Azure App palvelun | Microsoft Azure" 
    description="Opettele käyttämään kumppanien yrityksen Integration Pack ja logiikka-sovelluksissa" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="padmavc" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="padmavc"/>

# <a name="get-started-with-decode-x12-message"></a>Pura koodaus X12 viestin käytön aloittaminen

Vahvistaa kumppanin kielikohtaiset ominaisuudet ja Muokkaa luo tapahtuman kukin ehtojoukko XML-asiakirja ja luo acknowledgment käsitellyt tapahtuman.

## <a name="create-the-connection"></a>Yhteyden muodostaminen

### <a name="prerequisites"></a>Edellytykset

* Azure tilin; Voit luoda [ilmaisen tilin](https://azure.microsoft.com/free)

* Integrointi-tiliä tarvitaan Pura koodaus X12 viestin Connectorin käyttö. Lisätietoja siitä, miten voit luoda [Integrointi tilin](./app-service-logic-enterprise-integration-create-integration-account.md), [kumppaneiden](./app-service-logic-enterprise-integration-partners.md) ja [X12 sopimuksen](./app-service-logic-enterprise-integration-x12.md)

### <a name="connect-to-decode-x12-message-using-the-following-steps"></a>Yhdistä Pura koodaus X12 viesti seuraavalla tavalla:

1. [Logiikan-sovelluksen luominen](./app-service-logic-create-a-logic-app.md) on esimerkki

2. Tämä yhdistin ei ole Käynnistimet. Käynnistä logiikan sovellus, kuten pyynnön käynnistimen muiden käynnistimien avulla.  Logiikan sovelluksen suunnittelussa Lisää käynnistin ja lisätä toiminnon.  Valitse Näytä Microsoft hallitun ohjelmointirajapinnan-avattavasta luettelosta ja kirjoita sitten "x12" hakuruutuun.  Valitse X12 – purkaa X12 viesti

    ![Etsi x12](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage1.png)  

3. Jos et ole aiemmin luonut mahdolliset yhteydet integrointi tilille, sinua kehotetaan yhteystiedot

    ![integrointi tiliyhteys](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage4.png)    

4. Anna integrointi tilin tiedot.  Tarvittavat ominaisuudet tähdellä

  	| Ominaisuus | Tiedot |
  	| -------- | ------- |
  	| Yhteyden nimi * | Mikä tahansa yhteyden nimi |
  	| Integrointi tilin * | Kirjoita integrointi tilin nimi. Varmista, että integrointi-tili ja logiikka app ovat Azure samassa sijainnissa |

    Kun valmiina, yhteyden tiedot näyttää seuraavankaltaiselta
    
    ![integrointi yhteys luodaan](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage5.png) 

5. Valitse **Luo**
    
6. Huomaa, yhteys on luotu.

    ![integrointi tilin yhteystiedot](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage6.png) 

7. Valitse X12 flat tiedoston viestin koodaus

    ![Anna pakolliset kentät](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage7.png) 

## <a name="x12-decode-does-following"></a>X12 purkaa seuraavan käytettäessä

* Tarkistaa vastaan myynti kumppanin sopimuksen kirjekuori
* Luo XML-asiakirjan jokaisen tapahtuman määrittäminen.
* Vahvistaa kumppanin kielikohtaiset ominaisuudet ja Muokkaa
    * Muokkaa-rakenteellista kelpoisuustarkistus ja laajennettu rakenteen tarkistaminen
    * Interchange kirjekuoren rakenteen vahvistus.
    * Rakenteen tarkistaminen kirjekuoren ohjausobjektin mallin pohjalta.
    * Rakenteen tarkistaminen tapahtuman määrittäminen tietojen elementit viestin mallin pohjalta.
    * Muokkaa vahvistus suorittaa tapahtuman määrittäminen tietojen osat 
* Tarkistaa, että interchange, ryhmä ja tapahtuman määrittäminen ohjausobjektin numerot eivät ole samoja
    * Tarkistaa aiemmin vastaanotetut liittymiä vastaan interchange hallinnan numero.
    * Tarkistaa ryhmän hallinnan numero muut ryhmän ohjausobjektin luvuilla vaihdon vastaan.
    * Tarkistaa, tapahtuman ohjausobjektin numeron määrittäminen vastaan muiden tapahtuman määrittäminen ohjausobjektin numerot kyseisen ryhmän.
* Muuntaa koko interchange XML-muotoon 
    * Jaa Interchange tapahtuman joukkona - keskeyttää tapahtuman joukot-virhe: jäsentää jokaisen tapahtuman määrittäminen liittymän eri XML-tiedostoon. Jos yhden tai useamman tapahtuman määrittää vaihdon eivät läpäise tarkistusta, X12 Pura koodaus keskeyttää vain kyseisiä tapahtuman joukkoja.
    * Jaa Interchange tapahtuman joukkona - keskeyttää Interchange Format-virhe: jäsentää jokaisen tapahtuman määrittäminen liittymän eri XML-tiedostoon.  Jos yhden tai useamman tapahtuman määrittää vaihdon eivät läpäise tarkistusta, X12 Pura koodaus keskeyttää koko interchange.
    * Säilytä Interchange Format - keskeyttää tapahtuman joukot-virhe: Luo XML-tiedoston koko oletusmäärä musiikkitietojen. X12 Pura koodaus keskeyttää vain ne tapahtuma-joukot, jotka eivät läpäise tarkistusta, kun jatkat käsittelemään muun tapahtuman asettaa
    * Säilytä Interchange Format - keskeyttää Interchange Format-virhe: Luo XML-tiedoston koko oletusmäärä musiikkitietojen. Jos yhden tai useamman tapahtuman määrittää vaihdon eivät läpäise tarkistusta, X12 Pura koodaus keskeyttää koko interchange 
* Luo tekniset ja/tai toiminnalliset acknowledgment (Jos määritetty).
    * Tekniset Acknowledgment Luo tuloksena otsikon vahvistus. Tekniset acknowledgment ilmoittaa interchange ylä- ja perävaunun osoite vastaanottaja käsittelyä.
    * Toimi Acknowledgment Luo tuloksena leipätekstin vahvistus. Toimi acknowledgment raportit kunkin käsiteltäessä vastaanotettu asiakirja virhe

## <a name="next-steps"></a>Seuraavat vaiheet

[Lisätietoja yrityksen Integration Pack] (./app-service-logic-enterprise-integration-overview.md "Lisätietoja yrityksen integrointi Pack") 



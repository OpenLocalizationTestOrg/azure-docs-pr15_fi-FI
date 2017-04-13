<properties 
    pageTitle="Lisätietoja yrityksen integrointi Pack toistaa EDIFACT viestin yhdistimen | Microsoft Azure App palvelun | Microsoft Azure" 
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

# <a name="get-started-with-decode-edifact-message"></a>Toistaa EDIFACT viestin käytön aloittaminen

Vahvistaa kumppanin kielikohtaiset ominaisuudet ja Muokkaa luo tapahtuman kukin ehtojoukko XML-asiakirja ja luo acknowledgment käsitellyt tapahtuman.

## <a name="create-the-connection"></a>Yhteyden muodostaminen

### <a name="prerequisites"></a>Edellytykset

* Azure tilin; Voit luoda [ilmaisen tilin](https://azure.microsoft.com/free)

* Toistaa EDIFACT viestin Connectorin käyttö edellyttää integrointi-tiliä. Lisätietoja siitä, miten voit luoda [Integrointi tilin](./app-service-logic-enterprise-integration-create-integration-account.md), [kumppaneiden](./app-service-logic-enterprise-integration-partners.md) ja [EDIFACT sopimuksen](./app-service-logic-enterprise-integration-edifact.md)

### <a name="connect-to-decode-edifact-message-using-the-following-steps"></a>Yhdistä toistaa EDIFACT viestin seuraavalla tavalla:

1. [Logiikan-sovelluksen luominen](./app-service-logic-create-a-logic-app.md) on esimerkki.

2. Tämä yhdistin ei ole Käynnistimet. Käynnistä logiikan sovellus, kuten pyynnön käynnistimen muiden käynnistimien avulla.  Logiikan sovelluksen suunnittelussa Lisää käynnistin ja lisätä toiminnon.  Valitse Näytä Microsoft hallitun ohjelmointirajapinnan-avattavasta luettelosta ja Kirjoita hakukenttään "EDIFACT".  Valitse koodaus EDIFACT viesti

    ![Etsi EDIFACT](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactdecodeimage1.png)
    
3. Jos et ole aiemmin luonut mahdolliset yhteydet integrointi tilille, sinua kehotetaan yhteystiedot

    ![integrointi-tilin luominen](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactdecodeimage2.png)  

4. Anna integrointi tilin tiedot.  Tarvittavat ominaisuudet tähdellä

  	| Ominaisuus | Tiedot |
  	| -------- | ------- |
  	| Yhteyden nimi * | Mikä tahansa yhteyden nimi |
  	| Integrointi tilin * | Kirjoita integrointi tilin nimi. Varmista, että integrointi-tili ja logiikka app ovat Azure samassa sijainnissa |

    Kun valmiina, yhteyden tiedot näyttää seuraavankaltaiselta

    ![integrointi luotu tili](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactdecodeimage3.png)  

5. Valitse **Luo**

6. Huomaa, yhteys on luotu

    ![integrointi tilin yhteystiedot](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactdecodeimage5.png)  

7. Voit valita EDIFACT tietuetiedostoon viestin koodaus

    ![Anna pakolliset kentät](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactdecodeimage5.png)  

## <a name="edifact-decode-does-following"></a>Onko EDIFACT toistaa seuraavan

* Sopimuksen määrän, ratkaise vastaavat lähettäjän valitsin ja tunniste ja vastaanotin valitsin ja tunnus
* Jakaa useita liittymiä yhden viestin erilliseen kyselyjä.
* Tarkistaa vastaan myynti kumppanin sopimuksen kirjekuori
* Purkaa vaihdon.
* Vahvistaa kumppanin kielikohtaiset ominaisuudet ja Muokkaa sisältää
    * Interchange kirjekuoren rakenteen vahvistus.
    * Rakenteen tarkistaminen kirjekuoren ohjausobjektin mallin pohjalta.
    * Rakenteen tarkistaminen tapahtuman määrittäminen tietojen elementit viestin mallin pohjalta.
    * Muokkaa vahvistus suorittaa tapahtuman määrittäminen tietojen osat
* Tarkistaa, että interchange, ryhmä ja tapahtuman määrittäminen ohjausobjektin numerot eivät ole samoja (Jos määritetty) 
    * Tarkistaa aiemmin vastaanotetut liittymiä vastaan interchange hallinnan numero. 
    * Tarkistaa ryhmän hallinnan numero muut ryhmän ohjausobjektin luvuilla vaihdon vastaan. 
    * Tarkistaa, tapahtuman ohjausobjektin numeron määrittäminen vastaan muiden tapahtuman määrittäminen ohjausobjektin numerot kyseisen ryhmän.
* Luo XML-asiakirjan jokaisen tapahtuman määrittäminen.
* Muuntaa koko interchange XML-muotoon 
    * Jaa Interchange tapahtuman joukkona - keskeyttää tapahtuman joukot-virhe: jäsentää jokaisen tapahtuman määrittäminen liittymän eri XML-tiedostoon. Jos yhden tai useamman tapahtuman määrittää vaihdon eivät läpäise tarkistusta, valitse EDIFACT toistaa keskeyttää vain kyseisiä tapahtuman joukkoja. 
    * Jaa Interchange tapahtuman joukkona - keskeyttää Interchange Format-virhe: jäsentää jokaisen tapahtuman määrittäminen liittymän eri XML-tiedostoon.  Jos yhden tai useamman tapahtuman määrittää vaihdon eivät läpäise tarkistusta, valitse EDIFACT toistaa keskeyttää koko interchange.
    * Säilytä Interchange Format - keskeyttää tapahtuman joukot-virhe: Luo XML-tiedoston koko oletusmäärä musiikkitietojen. EDIFACT toistaa keskeyttää vain ne tapahtuma-joukot, jotka eivät läpäise tarkistusta samalla, kun käsitellä kaikkia muita tapahtuman joukot
    * Säilytä Interchange Format - keskeyttää Interchange Format-virhe: Luo XML-tiedoston koko oletusmäärä musiikkitietojen. Jos yhden tai useamman tapahtuman määrittää vaihdon eivät läpäise tarkistusta, valitse EDIFACT toistaa keskeyttää koko interchange 
* Luo tekniset (hallinta)-ja/tai toiminnallisten acknowledgment (Jos määritetty).
    * Tekniset Acknowledgment tai CONTRL ACK raportit valmis vastaanotettu vaihdon syntaktisia tarkastuksen tulokset.
    * Toimi acknowledgment hyväksyy sen, hyväksyä tai hylätä vastaanotettu interchange tai ryhmästä

## <a name="next-steps"></a>Seuraavat vaiheet

[Lisätietoja yrityksen Integration Pack] (./app-service-logic-enterprise-integration-overview.md "Lisätietoja yrityksen integrointi Pack") 
<properties 
    pageTitle="Lisätietoja yrityksen integrointi Pack koodata X12 viestin Connctor | Microsoft Azure App palvelun | Microsoft Azure" 
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

# <a name="get-started-with-encode-x12-message"></a>Käytä X12 viestin käytön aloittaminen

Vahvistaa kumppanin kielikohtaiset ominaisuudet ja Muokkaa muuntaa XML-koodattuja viestejä vaihdon Muokkaa tapahtuman joukot ja pyytää tekniset ja/tai toiminnalliset acknowledgment

## <a name="create-the-connection"></a>Yhteyden muodostaminen

### <a name="prerequisites"></a>Edellytykset

* Azure tilin; Voit luoda [ilmaisen tilin](https://azure.microsoft.com/free)

* Käytä x12 viestin Connectorin käyttö edellyttää integrointi-tiliä. Lisätietoja siitä, miten voit luoda [Integrointi tilin](./app-service-logic-enterprise-integration-create-integration-account.md), [kumppaneiden](./app-service-logic-enterprise-integration-partners.md) ja [X12 sopimuksen](./app-service-logic-enterprise-integration-x12.md)

### <a name="connect-to-encode-x12-message-using-the-following-steps"></a>Yhdistäminen käytä X12 viesti seuraavalla tavalla:

1. [Logiikan-sovelluksen luominen](./app-service-logic-create-a-logic-app.md) on esimerkki

2. Tämä yhdistin ei ole Käynnistimet. Käynnistä logiikan sovellus, kuten pyynnön käynnistimen muiden käynnistimien avulla.  Logiikan sovelluksen suunnittelussa Lisää käynnistin ja lisätä toiminnon.  Valitse Näytä Microsoft hallitun ohjelmointirajapinnan-avattavasta luettelosta ja kirjoita sitten "x12" hakuruutuun.  Valitse joko X12-koodata X12 viestin sopimuksen nimen mukaan tai X12-koodata X 12 viestiin käyttäjätietojen mukaan.  

    ![Etsi x12](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage1.png) 

3. Jos et ole aiemmin luonut mahdolliset yhteydet integrointi tilille, sinua kehotetaan yhteystiedot

    ![integrointi tiliyhteys](./media/app-service-logic-enterprise-integration-x12connector/x12encodeimage1.png) 


4. Anna integrointi tilin tiedot.  Tarvittavat ominaisuudet tähdellä

  	| Ominaisuus | Tiedot |
  	| -------- | ------- |
  	| Yhteyden nimi * | Mikä tahansa yhteyden nimi |
  	| Integrointi tilin * | Kirjoita integrointi tilin nimi. Varmista, että integrointi-tili ja logiikka app ovat Azure samassa sijainnissa |

    Kun valmiina, yhteyden tiedot näyttää seuraavankaltaiselta

    ![integrointi yhteys luodaan](./media/app-service-logic-enterprise-integration-x12connector/x12encodeimage2.png) 


5. Valitse **Luo**

6. Huomaa, yhteys on luotu.

    ![integrointi tilin yhteystiedot](./media/app-service-logic-enterprise-integration-x12connector/x12encodeimage3.png) 

#### <a name="x12---encode-x12-message-by-agreement-name"></a>X12-koodata X12 viestin sopimuksen nimen mukaan

7. Valitse X12 sopimuksen koodata avattavan luettelon ja xml-viestin.

    ![Anna pakolliset kentät](./media/app-service-logic-enterprise-integration-x12connector/x12encodeimage4.png) 

#### <a name="x12---encode-x12-message-by-identities"></a>X12-koodata X12 sanomassa käyttäjätietoja

7.  Anna lähettäjän tunnus, lähettäjän valitsin, vastaanottaja-tunnus ja vastaanotin valitsin X12 määritetty sopimuksen.  Valitse koodata xml-viesti

    ![Anna pakolliset kentät](./media/app-service-logic-enterprise-integration-x12connector/x12encodeimage5.png) 

## <a name="x12-encode-does-following"></a>X12 koodata käytettäessä seuraavat:

* Sopimuksen tarkkuus vertaamalla lähettäjä ja vastaanottaja konteksti-ominaisuudet.
* Serializes Muokkaa interchange, XML-koodattuja viestejä muuntaminen vaihdon Muokkaa tapahtuman joukot.
* Tapahtuman määrittäminen ylä- ja perävaunun osia koskee
* Luo Interchange Format-ohjausobjektin numero, ryhmän ohjausobjektin luvun ja kunkin lähtevän interchange tapahtuman määrittäminen ohjausobjektin numero
* Korvaa erottimet-paketin tiedoista
* Vahvistaa kumppanin kielikohtaiset ominaisuudet ja Muokkaa
    * Tapahtuman määrittäminen tietojen elementit vastaan viestin rakenteen rakenteen tarkistaminen
    * Muokkaa vahvistus suorittaa tapahtuman määrittäminen tietojen osat.
    * Laajennetun vahvistuksen suorittaa tapahtuman määrittäminen tietojen osat
* Pyytää tekniset ja/tai toiminnalliset acknowledgment (Jos määritetty).
    * Tekniset Acknowledgment Luo tuloksena otsikon vahvistus. Tekniset acknowledgment ilmoittaa interchange ylä- ja perävaunun osoite vastaanottaja käsittely
    * Toimi Acknowledgment Luo tuloksena leipätekstin vahvistus. Toimi acknowledgment raportit kunkin käsiteltäessä vastaanotettu asiakirja virhe

## <a name="next-steps"></a>Seuraavat vaiheet

[Lisätietoja yrityksen Integration Pack] (./app-service-logic-enterprise-integration-overview.md "Lisätietoja yrityksen integrointi Pack") 


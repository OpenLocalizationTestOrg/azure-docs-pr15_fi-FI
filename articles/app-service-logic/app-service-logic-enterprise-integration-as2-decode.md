<properties 
    pageTitle="Lisätietoja yrityksen integrointi Pack toistaa AS2 viestin Connctor | Microsoft Azure App palvelun | Microsoft Azure" 
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

# <a name="get-started-with-decode-as2-message"></a>Toistaa AS2 viestin käytön aloittaminen

Muodosta yhteys AS2 viestin koodaus muodostaa suojausta ja luotettavuutta siirrettäessä viestejä. Se on digitaalinen allekirjoitus, salauksen ja kuittaussanomien viestin poistamisen ilmoitukset (MDN) kautta.

## <a name="create-the-connection"></a>Yhteyden muodostaminen

### <a name="prerequisites"></a>Edellytykset

* Azure tilin; Voit luoda [ilmaisen tilin](https://azure.microsoft.com/free)

* Integrointi-tiliä tarvitaan toistaa AS2 viestin Connectorin käyttö. Lisätietoja siitä, miten voit luoda [Integrointi tilin](./app-service-logic-enterprise-integration-create-integration-account.md), [kumppaneiden](./app-service-logic-enterprise-integration-partners.md) ja [AS2 sopimuksen](./app-service-logic-enterprise-integration-as2.md)

### <a name="connect-to-decode-as2-message-using-the-following-steps"></a>Yhdistä toistaa AS2 viestin seuraavalla tavalla:

1. [Logiikan-sovelluksen luominen](./app-service-logic-create-a-logic-app.md) on esimerkki.

2. Tämä yhdistin ei ole Käynnistimet. Käynnistä logiikan sovellus, kuten pyynnön käynnistimen muiden käynnistimien avulla.  Logiikan sovelluksen suunnittelussa Lisää käynnistin ja lisätä toiminnon.  Valitse Näytä Microsoft hallitun ohjelmointirajapinnan-avattavasta luettelosta ja Kirjoita hakukenttään "AS2".  Valitse AS2 – AS2 viestin koodaus

    ![Etsi AS2](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage1.png)

3. Jos et ole aiemmin luonut mahdolliset yhteydet integrointi tilille, sinua kehotetaan yhteystiedot

    ![Integrointi yhteyden luominen](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage2.png)

4. Anna integrointi tilin tiedot.  Tarvittavat ominaisuudet tähdellä

  	| Ominaisuus   | Tiedot |
  	| --------   | ------- |
  	| Yhteyden nimi *    | Mikä tahansa yhteyden nimi |
  	| Integrointi tilin * | Kirjoita integrointi tilin nimi. Varmista, että integrointi-tili ja logiikka app ovat Azure samassa sijainnissa |

    Kun valmiina, yhteyden tiedot näyttää seuraavankaltaiselta

    ![yhteyden integrointi](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage3.png)

5. Valitse **Luo**
    
6. Huomaa, yhteys on luotu.  Nyt jatkaa logiikan sovelluksen muita ohjeita

    ![integrointi yhteys luodaan](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage4.png) 

7. Valitse leipätekstin ja otsikot pyynnön tulostus

    ![Anna pakolliset kentät](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage5.png) 

## <a name="the-as2-decode-does-the-following"></a>Toistaa AS2 tekee seuraavat toimet

* Käsittelee AS2/HTTP-otsikot
* Tarkistaa allekirjoitus (Jos määritetty)
* Purkaa viestit (Jos määritetty)
* Purkaa viestin (Jos määritetty)
* Vastaanotetun MDN alkuperäisen lähtevän viestin täsmää
* Päivitykset ja numeroidulla hyväksyntä tietokannan tietueisiin
* Kirjoittaa AS2 tilan raportointi-tietueet
* Tulosteen paketti sisältö on koodattu base64
* Määrittää, onko MDN tarvitaan, ja onko MDN pitäisi olla synkronoitu vai asynkroninen kokoonpanon perusteella AS2 sopimuksessa
* Luo synkronoitu tai asynkroninen MDN, (perustuu sopimuksen määrityksiä)
* Määrittää korrelaatio tunnusten ja ominaisuudet MDN

##<a name="try-it-for-yourself"></a>Kokeile itse

Miksi ei Kokeile. Valitse [tähän](https://azure.microsoft.com/documentation/templates/201-logic-app-as2-send-receive/) perustoiminnot logiikan sovelluksen oman käytön logiikan sovellusten AS2 ominaisuuksien käyttöönotto 

## <a name="next-steps"></a>Seuraavat vaiheet

[Lisätietoja yrityksen Integration Pack] (./app-service-logic-enterprise-integration-overview.md "Lisätietoja yrityksen integrointi Pack") 
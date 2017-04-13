<properties 
    pageTitle="B2B ratkaisujen luominen ja yrityksen integrointi Pack | Microsoft Azure App palvelun | Microsoft Azure" 
    description="Lisätietoja tietojen yrityksen Integration Pack B2B-ominaisuuksilla vastaanottamiseen" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="msftman" 
    manager="erikre" 
    editor="cgronlun"/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/08/2016" 
    ms.author="deonhe"/>

# <a name="learn-about-receiving-data-using-the-b2b-features-of-the-enterprise-integration-pack"></a>Lisätietoja tietojen yrityksen Integration Pack B2B-ominaisuuksilla vastaanottamiseen#

## <a name="overview"></a>Yleiskatsaus ##

Tämä asiakirja kuuluu logiikan sovellusten yrityksen Integration Pack. Lisätietoja saat lisätietoja [yrityksen Integration Pack ominaisuuksia](./app-service-logic-enterprise-integration-overview.md)yleiskuvauksessa.

## <a name="prerequisites"></a>Edellytykset ##

AS2 ja X12 toiminnot, sinun on tili yrityksen integrointi

[Yrityksen integrointi tilin luominen](./app-service-logic-enterprise-integration-accounts.md)

## <a name="how-to-use-the-logic-apps-b2b-connectors"></a>Logiikan sovellusten B2B yhdistimien käyttämisestä ##

Kun olet luonut tilin integrointi ja lisätty kumppanit ja toimeenpano siihen on valmis logiikan sovelluksen luominen, joka toteuttaa business-business (B2B) työnkulun.

Tämä walkthru näet AS2 ja X12 käyttäminen business business logiikan sovelluksen, joka vastaanottaa tietoja kaupan kumppanin luominen toiminnot.

1. Luo uusi logiikan sovellus ja [linkittää sen integrointi-tiliisi](./app-service-logic-enterprise-integration-accounts.md).  
2. **Pyyntö – kun HTTP-pyyntö on vastaanotettu** käynnistimen lisääminen logiikan-sovellukseen  
![](./media/app-service-logic-enterprise-integration-b2b/flatfile-1.png)  
3. Lisää ensin valitsemalla **Lisää toiminto** **Toistaa AS2** toiminta  
![](./media/app-service-logic-enterprise-integration-b2b/transform-2.png)  
4. Kirjoita hakuruutuun word **as2** voi suodattaa kaikki toiminnot, jota haluat käyttää johonkin  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-5.png)  
6. Valitse **AS2 - koodaus AS2 viesti** -toiminto  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-6.png)  
7. Kuten, Lisää **tekstissä** , joka otetaan syötteeksi. Valitse tässä esimerkissä HTTP-pyynnön, joka on käynnistänyt sovelluksen logiikan tekstiosaan. Voit myös kirjoittaa lausekkeen kirjoittanut otsikon**ylä** -kentässä:

    @triggerOutputs()['headers']

8. Lisää **otsikoita** , joita tarvitaan AS2. Nämä ovat HTTP-pyynnön otsikot. Tässä esimerkissä Valitse HTTP-pyynnön, joka on käynnistänyt sovelluksen logiikan sarakeotsikoita.
9. Lisää nyt Pura koodaus X12 viesti-toiminto valitsemalla **Lisää toiminnon** uudelleen  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-9.png)   
10. Kirjoita hakuruutuun word **x12** voi suodattaa kaikki toiminnot, jota haluat käyttää johonkin  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-10.png)  
11. Valitse **X12-koodaus X12 viestin** lisättävä logiikan-sovelluksen toiminto  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-as2message.png)  
12. Sinun täytyy nyt syöte toimintoon, joka on yllä AS2-toiminnon tulos. Todellinen viestin sisältöä on JSON-objekti ja base64 koodattu. Sinun täytyy määrittää lausekkeen syötteen lisätä seuraavan lausekkeen niin syötteen **X12 FLAT tiedoston viestin vastaanottaja Pura KOODAUS** -kentässä vuoksi  

    @base64ToString(body('Decode_AS2_message')?['AS2Message']?['Content'])  

13. Tämä vaihe purkaa X12 tietojen vastaanottanut kaupan kumppanilta ja siirtää JSON-objektin kohteiden määrä. Jotta voit antaa tietoja vastaanottamisesta tietää kumppanin voit lähettää takaisin vastauksen sisältävän AS2 viestin poistamisen ilmoitus (MDN) HTTP-vastauksen toiminto  
14. Lisää **vastaus** -toiminto valitsemalla **Lisää toiminnon**   
![](./media/app-service-logic-enterprise-integration-b2b/b2b-14.png)  
15. Kirjoita hakuruutuun word- **vastauksen** voi suodattaa kaikki toiminnot, jota haluat käyttää johonkin  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-15.png)  
16. Valitse lisättävä se **vastaus** -toiminto  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-16.png)  
17. Määrittää vastaus **LEIPÄTEKSTI** -kentän käyttäminen seuraava lauseke MDN tulosteesta **Pura koodaus X12 viesti** -toiminto  

    @base64ToString(body('Decode_AS2_message')?['OutgoingMdn']?['Content'])  

![](./media/app-service-logic-enterprise-integration-b2b/b2b-17.png)  
18. Työn tallentaminen  
![](./media/app-service-logic-enterprise-integration-b2b/transform-5.png)  

Tässä vaiheessa olet valmis B2B logiikan sovelluksen määrittämisestä. Käytännön sovelluksessa, haluat ehkä tallentaa purettu X12 LOB-sovelluksen tai tietojen kaupan tiedot. Voit helposti lisätä toiminto tai kirjoittaa mukautetun ohjelmointirajapinnan haluat muodostaa yhteyden oman Toimialasovellusten ja nämä ohjelmointirajapinnan käyttäminen logiikan sovelluksen toiminnot.

## <a name="features-and-use-cases"></a>Ominaisuudet ja käytä tapauksissa ##

- AS2 ja X12 toistaa ja koodata toiminnot voit vastaanottaa tietoja ja Lähetä tiedot kohteeksi kumppanien alan vakio protokollat logiikan sovellusten käyttäminen  
- Voit tehdä AS2 ja X12 kanssa tai ilman niitä toisiinsa vaihtaa tietoja liikekumppanien tarpeen mukaan
- B2B toimien avulla on helppo kumppanit ja toimeenpano luominen integrointi-tilin ja käyttää niitä logiikan-sovelluksessa  
- Logiikan sovelluksen muiden toimintojen pidentämällä voit lähettää ja vastaanottaa tiedot ja muita sovelluksia ja palveluja, kuten SalesForce  

## <a name="learn-more"></a>Opi lisää ##

[Lisätietoja yrityksen Integration Pack](./app-service-logic-enterprise-integration-overview.md)  
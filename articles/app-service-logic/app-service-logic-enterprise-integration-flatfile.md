<properties
    pageTitle="Opi salaaminen ja salauksen tasainen tiedostot yrityksen Integration Pack ja logiikka-sovellusten käyttämisestä | Microsoft Azure App palvelun | Microsoft Azure"
    description="Yrityksen Integration Pack ja logiikka sovellusten ominaisuuksien avulla salaaminen ja salauksen tasainen tiedostot"
    services="app-service\logic"
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

# <a name="enterprise-integration-with-flat-files"></a>Kiinteä tiedostojen Enterprise-integrointi

## <a name="overview"></a>Yleiskatsaus

Haluat ehkä koodata XML-sisällön, ennen kuin lähetät sen business kumppanin tilanne business business (B2B). Logiikan-sovellukseen, tekemät Azure App palvelun logiikan sovellukset-ominaisuutta voit koodaus yhdistimen tietuetiedostoon toiminto. Logiikan-sovellus, jonka luot saa sen XML eri lähteistä, mukaan lukien HTTP käynnistimestä, toisesta sovelluksesta tai jopa yhdestä monissa [yhdistimet](../connectors/apis-list.md)sisällön. Saat lisätietoja logiikan sovelluksista [logiikan sovellusten ohjeista](./app-service-logic-what-are-logic-apps.md "Lisätietoja logiikan sovellukset").  

## <a name="how-to-create-the-flat-file-encoding-connector"></a>Yhdistimen koodaus tietuetiedostoon luominen

Voit lisätä koodaus logiikan sovelluksen yhdistimen tietuetiedostoon seuraavasti.

1. Luo logiikan sovellus ja [linkittää sen tilin integrointi](./app-service-logic-enterprise-integration-accounts.md "opetteleminen asiakkaan linkittäminen työyhteyshenkilöön integrointi logiikan-sovellukseen"). Tili sisältää voit salata XML-tietojen rakennetta.  
2. Lisää **pyyntö – kun HTTP-pyyntö on vastaanotettu** käynnistimen logiikan-sovellukseen.  
![Näyttökuva Valitse käynnistin](./media/app-service-logic-enterprise-integration-flatfile/flatfile-1.png)    
3. Lisää tietuetiedostoon, toiminto-koodaus seuraavasti:

    a. Valitse **plus** -merkkiä.

    b. Valitse **Lisää toiminnon** linkki (tulee näkyviin, kun olet valinnut plusmerkkiä).

    c-näppäinyhdistelmää. Kirjoita hakuruutuun *Tasainen* kaikki suodatustoiminnot johonkin, jota haluat käyttää.

    d. Valitse **Tasainen koodaus** -vaihtoehto luettelosta.   
![Näyttökuva, tasainen tiedoston koodaus-vaihtoehto](./media/app-service-logic-enterprise-integration-flatfile/flatfile-2.png)   
4. Valitse **Tasainen koodaus** -valintaikkunan **sisällön** tekstiruutuun.  
![Näyttökuva sisällön tekstiruutu](./media/app-service-logic-enterprise-integration-flatfile/flatfile-3.png)  
5. Valitse leipäteksti-tunniste, jonka haluat koodata sisältöä. Leipäteksti-tunniste täyttää sisältö-kentässä.     
![Näyttökuva leipäteksti-tunniste](./media/app-service-logic-enterprise-integration-flatfile/flatfile-4.png)  
6. Valitse **Mallin nimi** -luetteloruutu ja valitse rakenteen haluat koodata syötteen sisällön avulla.    
![Näyttökuva rakenteen nimi-luetteloruutu](./media/app-service-logic-enterprise-integration-flatfile/flatfile-5.png)  
7. Tallenna työsi.   
![Näyttökuva, Tallenna-kuvaketta](./media/app-service-logic-enterprise-integration-flatfile/flatfile-6.png)  

Tässä vaiheessa olet valmis tietuetiedostoon-koodausta yhdistimen määrittäminen. Käytännön sovelluksessa haluat ehkä liiketoiminta-sovelluksessa, kuten Salesforce koodattu tietojen tallennusta varten. Tai voit lähettää koodattuja tietoja kohteeksi kumppanin. Voit helposti lisätä toiminnon lähettää koodaus-toiminnon tulos Salesforce- tai kaupan kumppanin jollakin annettu yhdistimet.

Nyt voit testata sitten yhdistimen tekemällä pyyntö HTTP-päätepisteen ja mukaan lukien XML-sisällön tekstissä pyynnön.  

## <a name="how-to-create-the-flat-file-decoding-connector"></a>Yhdistimen koodauksen tietuetiedostoon luominen

>[AZURE.NOTE] Näiden vaiheiden suorittamiseen tarvitset rakenne-tiedosto on jo ladattu integrointi tiliin.

1. Lisää **pyyntö – kun HTTP-pyyntö on vastaanotettu** käynnistimen logiikan-sovellukseen.  
![Näyttökuva Valitse käynnistin](./media/app-service-logic-enterprise-integration-flatfile/flatfile-1.png)    
2. Lisää tietuetiedostoon, koodauksen, toimi seuraavasti:

    a. Valitse **plus** -merkkiä.

    b. Valitse **Lisää toiminnon** linkki (tulee näkyviin, kun olet valinnut plusmerkkiä).

    c-näppäinyhdistelmää. Kirjoita hakuruutuun *Tasainen* kaikki suodatustoiminnot johonkin, jota haluat käyttää.

    d. Valitse **Tasainen koodauksen** -vaihtoehto luettelosta.   
![Näyttökuva, tasainen tiedoston koodauksen vaihtoehto](./media/app-service-logic-enterprise-integration-flatfile/flatfile-2.png)   
- Valitse **sisältöohjausobjekti** . Tämä tuottaa aiemmissa vaiheissa, jota voit käyttää sisältöä haluat toistaa sisältöä. Huomaa, että *leipäteksti* -Saapuva HTTP-pyyntö on käytettävissä, jota käytetään sisältöä haluat purkaa. Voit myös kirjoittaa sisällön toistaa suoraan **sisällön** ohjausobjektiin.     
- Valitse *leipäteksti* -tunniste. Ilmoitus leipäteksti-tunniste on nyt **sisältöohjausobjekti** .
- Valitse nimi, jonka haluat voit toistaa sisältö-rakenteen. Seuraavassa näyttökuvassa näkyy, että *OrderFile* on valitun mallin nimi. Tämän mallin nimi oli ladattu aiemmin integrointi-tilille.

 ![Näyttökuva, tasainen tiedoston koodauksen valintaikkuna](./media/app-service-logic-enterprise-integration-flatfile/flatfile-decode-1.png)    
- Tallenna työsi.  
![Näyttökuva, Tallenna-kuvaketta](./media/app-service-logic-enterprise-integration-flatfile/flatfile-6.png)    

Tässä vaiheessa olet valmis koodauksen yhdistimen tasainen tiedoston määrittäminen. Käytännön sovelluksessa haluat ehkä liiketoiminta-sovelluksessa, kuten Salesforce purettu tietojen tallennusta varten. Voit helposti lisätä toiminnon lähettäminen Salesforce koodauksen toiminnon tulos.

Voit testata sitten yhdistimen nyt tehdä pyyntö HTTP-päätepisteen ja XML-sisällön haluat purkaa pyynnön tekstiosaan.  

## <a name="next-steps"></a>Seuraavat vaiheet
- [Lisätietoja yrityksen Integration Pack] (./app-service-logic-enterprise-integration-overview.md "Lisätietoja yrityksen integrointi Pack").  

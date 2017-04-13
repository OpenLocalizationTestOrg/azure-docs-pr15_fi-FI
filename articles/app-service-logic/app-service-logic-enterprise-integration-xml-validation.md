<properties 
    pageTitle="XML-tarkistus yrityksen integrointi Pack yleiskatsaus | Microsoft Azure App palvelun | Microsoft Azure" 
    description="Lisätietoja vahvistus toiminta: n yrityksen Integration Pack ja logiikka" 
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

# <a name="enterprise-integration-with-xml-validation"></a>Yrityksen XML-vahvistus-integrointi

## <a name="overview"></a>Yleiskatsaus
Usein B2B skenaarioille sopimukseen kumppanien on vahvistaa, jotka he vaihtavat keskenään viestit ovat voimassa, ennen kuin tietojen käsittely voi alkaa. Yrityksen integrointi Pack Tarkista tiedostojen valmiin mallin pohjalta XML-vahvistus-yhdistin avulla.  

## <a name="how-to-validate-a-document-with-the-xml-validation-connector"></a>Voit tarkistaa tiedoston XML-vahvistus-yhdistin
1. Luo logiikan sovelluksen ja [linkittää sen tilin integrointi](./app-service-logic-enterprise-integration-accounts.md "opetteleminen asiakkaan linkittäminen työyhteyshenkilöön integrointi logiikan-sovellukseen") , joka sisältää rakennetta, voit tarkistaa XML-tiedot.
2. **Pyyntö – kun HTTP-pyyntö on vastaanotettu** käynnistimen lisääminen logiikan-sovellukseen  
![](./media/app-service-logic-enterprise-integration-xml/xml-1.png)    
3. Lisää ensin valitsemalla **Lisää toiminnon** mukaan **XML-vahvistus** -toiminto  
4. Kirjoita hakuruutuun *xml* voi suodattaa, jota haluat käyttää johonkin kaikki toiminnot 
5. Valitse **XML-tarkistus**     
![](./media/app-service-logic-enterprise-integration-xml/xml-2.png)   
6. Valitse **sisällön** tekstiruutu  
![](./media/app-service-logic-enterprise-integration-xml/xml-1-5.png)
7. Valitse leipäteksti-tunniste sisältöön, joka vahvistetaan.   
![](./media/app-service-logic-enterprise-integration-xml/xml-3.png)  
8. Valitse **Mallin nimi** -luetteloruutu ja valitse sitten Vahvista syötteen *sisältöä* yllä haluamasi rakenne     
![](./media/app-service-logic-enterprise-integration-xml/xml-4.png) 
9. Työn tallentaminen  
![](./media/app-service-logic-enterprise-integration-xml/xml-5.png) 

Tässä vaiheessa olet valmis määrittäminen kelpoisuustarkistuksen yhdistin. Käytännön sovelluksessa haluat ehkä tallentaa vahvistettu tietoja LOB-sovellus, esimerkiksi SalesForce. Voit helposti lisätä toiminnon Lähetä tarkistuksen tulos Salesforce. 

Voit testata kelpoisuuden toiminnon nyt tekemällä pyynnön HTTP-päätepisteen.  

## <a name="next-steps"></a>Seuraavat vaiheet

[Lisätietoja yrityksen Integration Pack] (./app-service-logic-enterprise-integration-overview.md "Lisätietoja yrityksen integrointi Pack")   
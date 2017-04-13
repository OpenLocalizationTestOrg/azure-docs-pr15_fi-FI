<properties 
    pageTitle="Yleistä integrointi asiakkaat- ja Enterprise-integroinnin Pack | Microsoft Azure App palvelun | Microsoft Azure" 
    description="Lisätietoja kaikki integrointi tietoja, yrityksen Integration Pack ja logiikka sovellukset" 
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

# <a name="overview-of-integration-accounts"></a>Yleistä integrointi tilit

## <a name="what-is-an-integration-account"></a>Mikä on integrointi-tili?
Integrointi-tili on Azure-tili, jonka hallittavan palvelutiedot, mukaan lukien rakenteita, kartat, varmenteet, kumppaneiden ja sopimusten yrityksen integrointi sovellusten avulla. Käytä integrointi-tiliä, jotta voit käyttää rakennetta, kartan tai varmenne, kuten on minkä tahansa integration-sovelluksen, voit luoda.

## <a name="create-an-integration-account"></a>Luo tili integrointi 
1. Valitse **Selaa**   
![](./media/app-service-logic-enterprise-integration-accounts/account-1.png)  
2. **Integrointi** suodatin haku-ruutuun ja valitse tulosten luettelosta **Integrointi tilit**     
 ![](./media/app-service-logic-enterprise-integration-accounts/account-2.png)  
3. Valitse *Lisää* -painikkeen valikossa sivun yläreunassa      
![](./media/app-service-logic-enterprise-integration-accounts/account-3.png)  
4. Kirjoita **nimi**, **tilauksen** , jota haluat käyttää, joko Luo uusi **resurssiryhmä** tai valitse aiemmin luotu resurssiryhmä-kohdassa **sijainti** missä integrointi tilisi isännöityjen, valitse **hinnoittelu taso**ja valitse sitten **Luo** -painike.   

  Tässä vaiheessa valitsemaasi paikkaan valmisteltu integrointi-tili. Tämä on suoritettava minuutin kuluessa.    
![](./media/app-service-logic-enterprise-integration-accounts/account-4.png)  
5. Päivitä sivu. Näkyviin tulee uusi tili integrointi luettelossa. Onnittelen!  
![](./media/app-service-logic-enterprise-integration-accounts/account-5.png) 

## <a name="how-to-link-an-integration-account-to-a-logic-app"></a>Voit linkittää integrointi tilin logiikan-sovellukseen
Logiikan-sovellusten pääsyn kartat, mallit, sopimuksia ja muita tietoja, jotka sijaitsevat integrointi tilisi järjestyksessä linkittää integrointi-tili ensin logiikan-sovellukseen.

### <a name="here-are-the-steps-to-link-an-integration-account-to-a-logic-app"></a>Näin integrointi asiakkaan linkittäminen logiikan-sovellus 

#### <a name="prerequisites"></a>Edellytykset
- Integrointi-tiliä
- Logiikan-sovellus

>[AZURE.NOTE]Varmista, että integrointi tili ja logiikka app ovat **samassa sijainnissa Azure** ennen aloittamista

1. Valitse **asetukset** -linkki sovelluksen logiikan valikosta  
![](./media/app-service-logic-enterprise-integration-accounts/linkaccount-1.png)   
2. Valitse kohde **Integrointi tilin** asetukset-sivu  
![](./media/app-service-logic-enterprise-integration-accounts/linkaccount-2.png)   
3. Valitse integration tili haluat linkittää oman logiikan **integrointi-tili** -sovellus avattava luetteloruutu  
![](./media/app-service-logic-enterprise-integration-accounts/linkaccount-3.png)   
4. Työn tallentaminen  
![](./media/app-service-logic-enterprise-integration-accounts/linkaccount-4.png)   
5. Näkyviin tulee ilmoitus, joka ilmaisee, että kaikki palvelutiedot integrointi tilisi ovat nyt käytettävissä logiikan-sovellukseen integrointi tilisi linkitetty logiikan-sovellukseen.  
![](./media/app-service-logic-enterprise-integration-accounts/linkaccount-5.png)   

Voit nyt, että tilisi integrointi on linkitetty logiikan sovelluksen, voit logiikan-sovellus ja B2B yhdistimiä, kuten XML-tarkistus, tietuetiedostoon koodata tai purkaa tai Muunna avulla voit luoda sovelluksia B2B ominaisuuksilla.  
    
## <a name="how-to-delete-an-integration-account"></a>Voit poistaa tilin integrointi?
1. Valitse **Selaa**  
![](./media/app-service-logic-enterprise-integration-overview/overview-1.png)    
2. **Integrointi** suodatin haku-ruutuun ja valitse tulosten luettelosta **Integrointi tilit**     
 ![](./media/app-service-logic-enterprise-integration-overview/overview-2.png)  
3. Valitse **integration-tili** , josta haluat poistaa  
![](./media/app-service-logic-enterprise-integration-overview/overview-3.png)  
4. Valitse **Poista** linkki, joka sijaitsee-valikosta   
![](./media/app-service-logic-enterprise-integration-accounts/delete.png)  
5. Vahvista valintasi    

## <a name="how-to-move-an-integration-account"></a>Miten voin siirtää integrointi-tili?
Voit siirtää integrointi tiliä helposti uusi tilaus ja uusi resurssiryhmä. Jos haluat siirtää integrointi-tiliisi, toimi seuraavasti:

>[AZURE.IMPORTANT] Sinun on päivitettävä kaikki komentosarjat käyttämään uusi resurssi tunnukset, kun siirrät integrointi-tiliä.

1. Valitse **Selaa**  
![](./media/app-service-logic-enterprise-integration-overview/overview-1.png)    
2. **Integrointi** suodatin haku-ruutuun ja valitse tulosten luettelosta **Integrointi tilit**     
 ![](./media/app-service-logic-enterprise-integration-overview/overview-2.png)  
3. Valitse **integration-tili** , josta haluat poistaa  
![](./media/app-service-logic-enterprise-integration-overview/overview-3.png)  
4. Valitse **Siirrä** linkki, joka sijaitsee-valikosta   
![](./media/app-service-logic-enterprise-integration-accounts/move.png)  
5. Vahvista valintasi    

## <a name="next-steps"></a>Seuraavat vaiheet
- [Lisätietoja yrityksen Integration Pack] (./app-service-logic-enterprise-integration-overview.md "Lisätietoja yrityksen integrointi Pack")  
- [Lisätietoja toimeenpano] (./app-service-logic-enterprise-integration-agreements.md "Lue lisää yrityksen integrointi toimeenpano")  


 
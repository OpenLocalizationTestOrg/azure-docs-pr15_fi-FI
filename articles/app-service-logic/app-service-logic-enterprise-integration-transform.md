<properties 
    pageTitle="Yrityksen integrointi Pack yleiskatsaus | Microsoft Azure App palvelun | Microsoft Azure" 
    description="Yrityksen Integration Pack ominaisuuksien avulla voit ottaa käyttöön business prosessin ja integrointi skenaariot Microsoft Azure-sovelluksen-palvelun avulla" 
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

# <a name="enterprise-integration-with-xml-transforms"></a>XML-muunnoksia Enterprise-integrointi

## <a name="overview"></a>Yleiskatsaus
Yrityksen integrointi Muunna yhdistimen muuntaa tietoja muodosta toiseen muotoon. Saatat joutua esimerkiksi saapuvan viestin, joka sisältää nykyisen päivämäärän YearMonthDay-muodossa. Muunto avulla voit muotoilla päivämäärän muotoon MonthDayYear uudelleen.

## <a name="what-does-a-transform-do"></a>Mitä muunto tarkoittaa?
Muunto, joka on tunnetaan myös nimellä kartan, koostuu lähde XML-rakenteen (IME) ja kohde XML-rakenteen (tulos). Voit helpottaa käsitellä tai ohjausobjektien tiedot, mukaan lukien merkkijonon yhdistämismäärityksiä, ehdollinen tehtäviä, aritmeettisia lausekkeita päivämäärä kellonaika formatters ja toistuminen jopa rakenteita eri sisäiset funktiot.

## <a name="how-to-create-a-transform"></a>Voit luoda muunto?
Voit luoda muunnos tai Yhdistä Visual Studio [Yrityksen integrointi SDK](https://aka.ms/vsmapsandschemas)avulla. Kun olet valmis, luominen ja testaus muunnoksen, lataa muunnoksen integrointi-tilillesi. 

## <a name="how-to-use-a-transform"></a>Opi käyttämään muunto
Kun olet ladannut muunnoksen integrointi tilille, voit sen logiikan sovelluksen luominen. Logiikan sovellus suoritetaan sitten oman muunnoksia aina, kun logiikan sovellus käynnistyy (ja on syötteen sisältöä, jonka käyttöoikeutta muunnettavat).

**Käyttää muunto-muodossa seuraavasti**:

### <a name="prerequisites"></a>Edellytykset 
Esikatselussa sinun on:  

-  [Luo Azure-Funktiot-säilö] (https://ms.portal.azure.com/#create/Microsoft.FunctionApp "Luo Azure-Funktiot-säilö")  
-  [Lisää funktio Azure-Funktiot-säilöön] (https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-logic-app-transform-function%2Fazuredeploy.json "Tämä malli luo webhook mukaan C# azure funktion muunto-ominaisuuksia, jotka logiikan sovellusten integrointi skenaariot käyttäminen")    
-  Luo tili integrointi ja kartan lisääminen  

>[AZURE.TIP] Pane merkille Azure-funktioiden säilö ja Azure-funktion nimi, sinun on ne seuraavassa vaiheessa.  

Nyt, sinun on otettava varoen edellytykset, on aika logiikan-sovelluksen luominen:  

1. Logiikan sovelluksen ja [linkittää sen tilin integrointi](./app-service-logic-enterprise-integration-accounts.md "opetteleminen asiakkaan linkittäminen työyhteyshenkilöön integrointi logiikan-sovellukseen") , joka sisältää kartan luominen
2. **Pyyntö – kun HTTP-pyyntö on vastaanotettu** käynnistimen lisääminen logiikan-sovellukseen  
![](./media/app-service-logic-enterprise-integration-transforms/transform-1.png)    
3. Lisää mukaan ensin valitsemalla **Lisää toiminto** **Muuntaa XML** -toiminto   
![](./media/app-service-logic-enterprise-integration-transforms/transform-2.png)   
4. Kirjoita sana *transform* hakuruutuun voi suodattaa kaikki toiminnot, jota haluat käyttää johonkin  
![](./media/app-service-logic-enterprise-integration-transforms/transform-3.png)  
5. Valitse **Muunna XML** -toiminto   
![](./media/app-service-logic-enterprise-integration-transforms/transform-4.png)  
6. Valitse **FUNKTIO SÄILÖ** , joka sisältää funktion, jota käytät. Tämä on Azure-funktioiden säilö, jonka loit aiemmin seuraavasti nimi.
7. Valitse haluamasi **FUNKTIO** . Tämä on aiemmin luomasi Azure-funktion nimi.
8. Voit lisätä **sisältöä** , voit muuntaa XML. Huomaa, että voit käyttää kaikki XML-tiedot, näyttöön tulee HTTP-pyyntö kuin **sisällön**. Valitse tässä esimerkissä HTTP-pyynnön, joka on käynnistänyt sovelluksen logiikan tekstiosaan.
9. Valitse **KARTAN** , jota haluat käyttää suorittamiseen muunnoksen nimi. Kartan tulee olla jo integrointi tilissäsi. Aiemmassa vaiheessa jo annoit logiikan-sovelluksen käytön kartan sisältävälle integrointi-tiliisi.
10. Työn tallentaminen  
![](./media/app-service-logic-enterprise-integration-transforms/transform-5.png) 

Tässä vaiheessa olet valmis, voit vaihtaa määrittäminen. Käytännön sovelluksessa haluat ehkä tallentaa LOB-sovellus, esimerkiksi SalesForce muunnettua havaintoaineistoa. Voit helposti kuin muunnoksen tulosteen lähettäminen Salesforce toiminto. 

Voit testata oman Muunna nyt tekemällä pyynnön HTTP-päätepisteen.  

## <a name="features-and-use-cases"></a>Ominaisuudet ja käytä tapauksissa

- Luotu kartan muunnos voi olla yksinkertainen, kuten nimi ja osoite kopioiminen tiedostosta toiseen. Vaihtoehtoisesti voit luoda monimutkaisia muunnoksia ulos,-valmiilla kartta-toimintojen avulla.  
- Useita kartta-toimintojen ja Funktiot ovat helposti saatavilla, esimerkiksi merkkijonot ja päivämäärän kellonaikafunktiot.  
- Tee kopio Suorasaantitiedot mallien välillä. Tämä on pelkästään Piirrä viiva, joka yhdistää niiden vastaaviin määritelty kohde tietolähteen rakenne-elementtejä Mapper, sisältyvät SDK.  
- Luodessasi kartan, voit tarkastella kartan, graafinen esitys, joka näyttää kaikki yhteydet ja voit luoda linkkejä.
- Testi-kartan avulla voit lisätä XML-esimerkkiviesti. Yksinkertainen napsauttamalla Testaa luomasi kartan ja nähdä luotu tulosteen.  
- Lataa aiemmin luotu kartat  
- XML-muoto tukee.


## <a name="learn-more"></a>Opi lisää
- [Lisätietoja yrityksen Integration Pack] (./app-service-logic-enterprise-integration-overview.md "Lisätietoja yrityksen integrointi Pack")  
- [Lisätietoja kartat] (./app-service-logic-enterprise-integration-maps.md "Lue lisää yrityksen integrointi määritykset")  
 
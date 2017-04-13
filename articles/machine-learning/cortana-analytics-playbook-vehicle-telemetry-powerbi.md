<properties 
    pageTitle="Ajoneuvon telemetriatietojen analytics ratkaisu mallin PowerBI Raporttinäkymät-ikkunan määrittämisohjeet | Microsoft Azure" 
    description="Cortana yritystieto-ominaisuuksien käyttäminen saada reaaliaikaisia ja ennakoivan oivallusten luomiseen ajoneuvon terveys- ja ajo-käyttöä." 
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="bradsev" />


# <a name="vehicle-telemetry-analytics-solution-template-powerbi-dashboard-setup-instructions"></a>Ajoneuvon telemetriatietojen analytics ratkaisu mallin PowerBI Raporttinäkymät-ikkunan määritysohjeet ovat

Tässä **valikossa** linkkejä tämän playbook luvut. 

[AZURE.INCLUDE [cap-vehicle-telemetry-playbook-selector](../../includes/cap-vehicle-telemetry-playbook-selector.md)]


Ajoneuvon Telemetriatietojen Analytics-ratkaisun esitellään, miten auton dealerships, autojen valmistajat ja vakuutusyhtiöiden voidaan hyödyntää Cortana liiketoimintatietojen ominaisuuksia ja myönnä reaaliaikainen ja ennakoivan oivallusten luomiseen ajoneuvon terveyteen ja ajo käyttöä asema parannukset alueella asiakkaan, ilmenee, R & D ja markkinointi kampanjat. Tämä asiakirja sisältää vaiheittaiset ohjeet, miten voit määrittää PowerBI raporttien ja koontinäytön kun ratkaisu on otettu käyttöön tilauksen. 


## <a name="prerequisites"></a>Edellytykset
1.  Ajoneuvon Telemetriatietojen Analytics-ratkaisun käyttöönotto siirtymällä [https://gallery.cortanaanalytics.com/SolutionTemplate/Vehicle-Telemetry-Analytics-3](https://gallery.cortanaanalytics.com/SolutionTemplate/Vehicle-Telemetry-Analytics-3)  
2.  [Asenna Microsoft Power BI Desktopin](http://www.microsoft.com/download/details.aspx?id=45331)
3.  On [Azure tilauksen](https://azure.microsoft.com/pricing/free-trial/). Jos sinulla ei ole Azure tilauksen, aloittaminen Azure ilmainen tilaus
4.  Microsoft PowerBI-tili
    

## <a name="cortana-intelligence-suite-components"></a>Cortana liiketoimintatietojen Suite osat
Osana ajoneuvon Telemetriatietojen Analytics-ratkaisun mallin seuraavat Cortana Liiketoimintatieto-palvelut on otettu käyttöön tilauksen.

- **Tapahtuman keskittimet** ingesting ajoneuvon telemetriatietojen tapahtumien miljoonia Azure kyselyjä.
- Muutokset, reaaliaikaiset tiedot ajoneuvon terveyteen **Stream analyyttisten**s ja jatkuu tiedot pitkään varastoon tehokkaan erä analysoinnissa.
- **Koneen Learning** poikkeavuuksista tunnistaminen reaaliaikaisesti ja eräkäsittely ja myönnä ennakoivan tiedot.
- **Hdinsightista** on hyödyntää tasolla tietojen muuntamiseen
- **Data Factory** käsittelee tiedonsiirron, aikataulutus, Resurssienhallinta ja eräkäsittely putkijohto seuranta.

**Power BI** antaa tämän ratkaisun monipuolisia Raporttinäkymät-ikkunan reaaliaikaiset tiedot ja ennakoivan analytics visualisointeja. 

Ratkaisu käyttää kahta eri tietolähteistä: **Simuloitu ajoneuvon signaalit ja diagnostiikan tietojoukko** ja **ajoneuvon luettelon**.

Ajoneuvon telemaattiset-simulator on tämän ratkaisun mukana. Se tietokoneesta kuuluu äänimerkki vianmääritystiedot ja signaalit vastaavat ajoneuvon kunto ja ajo kuvion tiettynä ajankohtana. 

Ajoneuvon luettelo on viittaus tietojoukon sisältävä VIN mallin määritys


## <a name="powerbi-dashboard-preparation"></a>PowerBI Raporttinäkymät-ikkunan valmistelu

### <a name="deployment"></a>Käyttöönotto

Kun asennus on valmis, kaikki komponentit, joka on merkitty vihreällä pitäisi näkyä seuraavassa kaaviossa. 

- Siirry vastaavat palvelut, tarkista, onko kaikki nämä ottanut onnistuneesti valitsemalla vihreä solmujen oikeassa yläkulmassa olevaa nuolta.
- Lataa tiedot simulator-paketti, napsauttamalla kirjoittaminen **Ajoneuvon telemaattiset Simulator** solmu yläosassa olevaa nuolta. Tallenna ja Pura tiedostot paikallisesti tietokoneeseen. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/1-deployed-components.png)

Nyt haluat määrittää PowerBI Raporttinäkymät-ikkunan monipuolisia visualisointeja ja myönnä reaaliaikainen ja ennakoivan havainnollistamisen ajoneuvon terveys- ja ajo-käyttöä. Kestää noin 45 minuuttia raporttien luominen ja määrittäminen raporttinäkymät tunneiksi. 


### <a name="setup-power-bi-real-time-dashboard"></a>Power BI reaaliaikainen Raporttinäkymät-ikkunan asetukset

**Luo Simuloitu tiedot**

1. Paikallisessa tietokoneessa Siirry kansioon, johon purit ajoneuvon telemaattiset Simulator-paketti![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/2-vehicle-telematics-simulator-folder.png)
2.  Suorita sovellus ***CarEventGenerator.exe***.
3.  Se tietokoneesta kuuluu äänimerkki vianmääritystiedot ja signaalit vastaavat ajoneuvon kunto ja ajo kuvion tiettynä ajankohtana. Tämä on julkaistu, joka on määritetty osana käyttöönoton Azure tapahtumaa-toiminnossa esiintymä.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/3-vehicle-telematics-diagnostics.png)
     
**Aloita reaaliaikainen dashboard-sovellus**

Ratkaisu on sovellus, joka luo reaaliaikainen Raporttinäkymät-ikkunan PowerBI. Tämän sovelluksen seuraa tapahtumaa-toiminnossa esiintymään, josta Stream Analytics julkaisee tapahtumat jatkuvasti. Jokaisen tapahtuman, joka vastaanottaa tämän sovelluksen käsittelee tiedot käyttämällä tietokoneen Learning – vastaus tulosmalli päätepiste. Seuraava tietojoukon on julkaistu PowerBI push API visualisoinnille. 

Voit ladata sovelluksen seuraavasti:

1.  Verkkokaavio-näkymän PowerBI-solmu ja valitse **Lataa reaaliaikaisen Dashboard-sovellus**"-linkki ominaisuudet-ruudussa.![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard-new1.png)
2.  Pura ja Tallenna sovellus paikallisesti![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/4-real-time-dashboard-application.png)

3.  Suorita sovellus **RealtimeDashboardApp.exe**
4.  Anna kelvollinen Power BI-tunnistetiedot, kirjaudu sisään ja sitten **Hyväksy**
    
    ![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/5-sign-into-powerbi.png)
    
    ![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/6-powerbi-dashboard-permissions.png)


### <a name="configure-powerbi-reports"></a>Määritä PowerBI-raportit
Reaaliaikainen raporttien ja koontinäytön suorittaminen kestää noin 30-45 minuuttia. Siirry [http://powerbi.com](http://powerbi.com) ja kirjaudu sisään.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/6-1-powerbi-signin.png)

Uusi tietojoukko luodaan Power BI. Valitse **ConnectedCarsRealtime** tietojoukko.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/7-select-connected-cars-realtime-dataset.png)

Tallenna tyhjä raportti käyttämällä **Ctrl + s**.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/8-save-blank-report.png)

Anna raportin nimeä *ajoneuvon Telemetriatietojen Analytics reaaliaikaisen - raportit*.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/9-provide-report-name.png)

## <a name="real-time-reports"></a>Reaaliaikainen raportit
Tämä ratkaisu on kolme reaaliaikainen raportteja:

1.  Ajoneuvot-toiminnossa
2.  Ajoneuvot edellyttävän ylläpito
3.  Ajoneuvot kuntotietojen

Voit määrittää kaikkien kolmen reaaliaikainen raporttien tai lopettaa kaikki vaiheen jälkeen ja jatka seuraavaan osaan erä raporttien asetusten määrittäminen. On suositeltavaa luoda kolme raportteja voi esittää ratkaisun reaaliaikainen polun koko tiedot.  

### <a name="1-vehicles-in-operation"></a>1. toiminnon ajoneuvot
  
Kaksoisnapsauta **sivu 1** ja anna sille nimi "Ajoneuvot toiminnon"  
    ![Yhdistetty autot - toiminnossa ajoneuvot](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4a.png)  

Valitse **vin** -kenttä **kentät** ja valitse visualisointityyppi **"**kortti".  

Korttivisualisointi luodaan kuvassa osoitetulla tavalla.  
    ![Yhdistetyn autojen – Valitse vin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4b.png)

Valitse Lisää uusi visualisoinnin tyhjää aluetta.  

Valitse **Kaupunki** - ja **vin** kentät. Muuta visualisointi **"Kartta"**. Vedä **vin** arvot-alueelle. Vedä kentät **Kaupunki** **selitealueen** .   
    ![Yhdistetty autot - kortti-visualisointi](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4c.png)
  
Valitse **muoto** -kohdassa **visualisointeja**, valitse **otsikko** ja muuta **tekstin** **"Ajoneuvot toiminnon kaupungin mukaan"**.  
    ![Yhdistetty autot - toiminnon kaupungin perusteella ajoneuvot](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4d.png)   

Lopullinen visualisointi näyttää kuvassa osoitetulla tavalla.    
    ![Yhdistetyn autot - viimeinen visualisointi](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4e.png)

Valitse Lisää uusi visualisoinnin tyhjää aluetta.  

Valitse **Kaupunki** - ja **vin**, Vaihda visualisointityyppi **Liitetty pylväskaavio**. Varmista **akseli-alueella** ja **vin** **arvo** alueen **Kaupunki** -kentät  

Lajittele **"Vin määrä"** -kaavio  
    ![Yhdistetyn autot - vin määrä](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4f.png)  

Muuttaa kaavion **otsikko** **"Ajoneuvot toiminnon kaupungin mukaan"**  

Valitse **muoto** -osassa ja valitse sitten **Tiedot värejä** **","** **Näytä** kaikki  
    ![Yhdistetyn autot - Näytä kaikki tiedot värit](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4g.png)  

Yksittäisten Kaupunki värin muuttaminen valitsemalla väri-kuvaketta.  
    ![Yhdistetty autot - Muuta värejä](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4h.png)  

Valitse Lisää uusi visualisoinnin tyhjää aluetta.  

Valitse **Liitetty pylväskaavio** visualisoinnin visualisointeja, vedä **Kaupunki** -kentät **selitealueen** **mallin** ja **vin** **arvo** alueella **akseli** -alueella.  
    ![Yhdistetyn autot - liitetty pylväskaavio](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4i.png)  
    ![Yhdistetyn autot - hahmonnettaessa](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4j.png)
  
Järjestä kaikki visualisoinnin tällä sivulla kuvassa osoitetulla tavalla.  
    ![Yhdistetyn autot - visualisoinnit](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4k.png)

Olet määrittänyt "Ajoneuvot toiminnon" reaaliaikainen raportin. Voit siirtyä seuraavaan reaaliaikainen raportin luominen tai lopettaa tähän ja määrittäminen raporttinäkymät. 

### <a name="2-vehicles-requiring-maintenance"></a>2. ajoneuvot edellyttävän ylläpito
  
Valitse ![Lisää](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) voit lisätä uuden raportin nimeä se uudelleen **"ajoneuvot edellyttävän ylläpito"**

![Yhdistetyn autot - ajoneuvot edellyttävän ylläpito](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4l.png)  

Valitse **vin** kenttä ja vaihda visualisointityyppi **korttiin**.  
    ![Yhdistetyn autot - Vin kortti-visualisointi](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4m.png)  

Kentän nimi "MaintenanceLabel" tietojoukossa on. Tämä kenttä voi olla arvo "0" tai "1"." Se on määritetty Azure koneen Learning mallin valmisteltu ratkaisun osana ja integroitu reaaliaikainen polku mukaan. Arvo on "1" ilmaisee ajoneuvon edellyttää ylläpito. 

Voit lisätä ajoneuvot tiedot, jotka vaativat ylläpito näkyvät **Sivun tasolla** suodattimen seuraavasti: 

1. Vedä **Sivun tason suodattimia** **"MaintenanceLabel"** -kenttään.  
![Yhdistetyn autot - sivun tason suodattimet](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n1.png)  

2. Valitse esitä MaintenanceLabel sivun tason suodatin alareunassa **Perussuodatus** valikko.  
![Yhdistetty autot - Perussuodatus](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n2.png)  

3.  Määritä suodatin sen arvoksi **"1"**    
![Yhdistetyn autot - suodattimen arvo](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n3.png)  


Valitse Lisää uusi visualisoinnin tyhjää aluetta.  

Valitse visualisoinnit **Liitetty pylväskaavio**  
![Yhdistetyn autot - Vind Korttivisualisointi](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4o.png)  
![Yhdistetyn autot - liitetty pylväskaavio](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4p.png)

Vedä kenttä **mallin** **akselin** alueelle, **Vin** **arvo** -alueelle. Valitse Lajittele visualisoinnin **vin määrän**mukaan.  Muuttaa kaavion **otsikko** **"Ajoneuvot edellyttävän ylläpito mallin mukaan"**  

Vedä **vin** kentät **Värikylläisyys** esitä **-kenttien** ![kentät](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4field.png) **visualisointi** -välilehden osa  
![Yhdistetyn autot - värikylläisyys](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4q.png)  

Visualisointien **muoto** -osasta **Tietojen värien** muuttaminen  
Pienin värin muuttaminen: **F2C812**  
Suurin värin muuttaminen: **FF6300**  
![Yhdistetty autot - väri muuttuu](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4r.png)  
![Yhdistetty autot - uudet visualisoinnin värit](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4s.png)  

Valitse Lisää uusi visualisoinnin tyhjää aluetta.  

Valitse visualisoinnit **liitetty pylväskaavio** , vedä **vin** -kentän **arvo** -alueelle, vedä **Kaupunki** -kentät **akseli** -alueelle. Lajittele kaavio **"Vin määrä"**. Muuttaa kaavion **otsikko** **"Ajoneuvot edellyttävän ylläpito kaupungin mukaan"**   
![Yhdistetyn autot - ajoneuvot edellyttävän ylläpito kaupungin perusteella](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4t.png)  

Valitse Lisää uusi visualisoinnin tyhjää aluetta.  

Valitse **Usean rivin** korttivisualisointi visualisointeja, vedä **malli** ja **vin** **kentät** -alueelle.  
![Yhdistetyn autot - usean rivin kortti](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4u.png)    

Uudelleenjärjestäminen kaikki visualisoinnin lopullinen raportti näyttää seuraavasti:  
![Yhdistetyn autot - usean rivin kortti](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4v.png)  

Olet määrittänyt "Ajoneuvot edellyttävän ylläpito" reaaliaikainen raportin. Voit siirtyä seuraavaan reaaliaikainen raportin luominen tai lopettaa tähän ja määrittäminen raporttinäkymät. 

### <a name="3-vehicles-health-statistics"></a>3. kuntotietojen ajoneuvot
  
Valitse ![Lisää](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) Lisää uusi raportti, anna sen nimeksi **"ajoneuvot kuntotietojen"**  

Valitse **Mittarikaavion** visualisointi visualisointeja, ja vedä **nopeus** -kentän **arvo, pienin arvo, suurin arvo** alueisiin.  
![Yhdistetyn autot - usean rivin kortti](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4w.png)  

Muuta **arvo** alueella **nopeus** koosteiden oletusasetusten valitseminen **keskiarvo** 

Muuta **nopeus** **Pienin** alueen ja **vähintään** koosteiden oletusasetusten valitseminen

Muuta **nopeus** **enintään** alueen **suurimman** ja koosteiden oletusasetusten valitseminen

![Yhdistetyn autot - usean rivin kortti](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4x.png)  

**Värillinen otsikko** nimeksi **"Keskinopeus"** 
 
![Yhdistetyn autot - mittari](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4y.png)  

Valitse Lisää uusi visualisoinnin tyhjää aluetta.  

Lisää vastaavasti **mittari** **keskimääräinen oil**, **polttoaineen**ja **keskimääräinen Lauhkean ohjelma**.  

Muuta kenttien kunkin mittari kohti kuin koosteiden oletusasetusten valitseminen ohjeet ovat **"Keskinopeus"** mitta.

![Yhdistetyn autot - Gauges](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4z.png)

Valitse Lisää uusi visualisoinnin tyhjää aluetta.

Valitse **rivi- ja liitetty pylväskaavio** visualisointeja, vedä **Kaupunki** -kentät kyselyjä **Jaettu akselin**, vedä **nopeus**- **Sarakkeen arvot** -alueelle **tirepressure ja engineoil kenttien** muuttaa niiden yhdistämistapa, **keskiarvo**. 

Vedä **engineTemperature** **Rivin arvot** -alueelle, **keskiarvon**koonti-tyypin muuttaminen. 

![Yhdistetyn autot - visualisointien kentät](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4aa.png)

Muuttaa kaavion **otsikko** **"keskinopeus, tire paine, ohjelma oil**ja ohjelma lämpötilan".  

![Yhdistetyn autot - visualisointien kentät](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4bb.png)

Valitse Lisää uusi visualisoinnin tyhjää aluetta.

Valitse **Treemap** visualisoinnin visualisointeja, vedä **malli** -kentässä **ryhmä** -alueelle ja vedä kenttä **MaintenanceProbability** **arvot** -alueelle.

Muuta kaavion **otsikkoa** **"Ajoneuvon mallien edellyttävän ylläpito"**.

![Yhdistetty autot - muuta kaavion otsikko](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4cc.png)

Valitse Lisää uusi visualisoinnin tyhjää aluetta.

Valitse visualisointi **100 % pinottu palkkikaavio kaavio** , vedä **Kaupunki** -kenttään **akseli** -alueelle ja vedä **MaintenanceProbability** **RecallProbability** kentät **arvo** -alueelle.

![Yhdistetyn autot - Lisää uusi visualisointi](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4dd.png)

Valitse **Muotoile**, valitse **Tietoja värit**ja **MaintenanceProbability** värin määrittäminen arvoon **"F2C80F"**.

Kaavion **otsikon** muuttaminen **"todennäköisyys, ajoneuvon ylläpito ja peruuttaa mukaan**postitoimipaikka".

![Yhdistetyn autot - Lisää uusi visualisointi](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ee.png)

Valitse Lisää uusi visualisoinnin tyhjää aluetta.

Valitse visualisoinnit-visualisoinnin **Aluekaavio** , vedä **mallin** **akseli** -alueelle ja vedä **engineOil, tirepressure, nopeuden ja MaintenanceProbability** kentät **arvot** -alueelle. Muuttaa niiden yhdistämistapa **"Keskiarvo"**. 

![Yhdistetty autot - muuta yhdistämistapa](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ff.png)

Kaavion otsikon muuttaminen **"keskimääräinen ohjelma oil**, tire paine, nopeuden ja ylläpito todennäköisyys mallin mukaan".

![Yhdistetty autot - muuta kaavion otsikko](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4gg.png)

Valitse Lisää uusi visualisoinnin tyhjää kohtaa:

1. Valitse **Pistekaavio** visualisoinnin visualisointeja.
2. Vedä **malli** -kentän **tiedot** ja **selite** -alueelle.
3. Vedä **polttoaineen** **akselin** alueelle, muuta koosteen **keskiarvo**.
4. Vedä **engineTemparature** **y-akselin**alueelle, muuta koosteen **keskiarvo**
5. Vedä **vin** kentän **koko** -alueelle.


![Yhdistetyn autot - Lisää uusi visualisointi](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4hh.png)

Muuta kaavion **otsikko** **"Polttoaineen keskiarvoja, ohjelma lämpötilan mallin mukaan"**.

![Yhdistetty autot - muuta kaavion otsikko](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ii.png)

Lopullinen raportti näyttää alla kuvatulla tavalla.

![Yhdistetty autojen lopullinen raportti](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4jj.png)

### <a name="pin-visualizations-from-the-reports-to-the-real-time-dashboard"></a>PIN-tunnuksen visualisoinnit raporteista reaaliaikainen koontinäyttö
  
Luo tyhjä Raporttinäkymät-ikkunan valitsemalla raporttinäkymien vieressä plus-kuvaketta. Voit nimetä sen "Ajoneuvon analyysin Telemetriatietojen"

![Yhdistetyn autot-koontinäyttö](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.5.png)

Kiinnitä visualisoinnin yläpuolella raporteista koontinäyttö. 
 
![Yhdistetyn autot-koontinäyttö](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.6.png)

Koontinäytön pitäisi näyttää seuraavasti, kun kolme raportteja luodaan ja vastaavan visualisoinnit ovat kiinnitetyt koontinäyttö. Jos et ole luonut kaikki raportit, raporttinäkymät saattaa näyttää erilaiselta. 

![Yhdistetyn autot-koontinäyttö](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-4.0.png)

Onnittelen! Olet luonut reaaliaikainen Raporttinäkymät-ikkunan. Kun jatkat suorittamaan CarEventGenerator.exe ja RealtimeDashboardApp.exe, live-päivitykset pitäisi näkyä koontinäytössä. Olisi kestää noin 10 – 15 minuuttia seuraavien vaiheiden mukaisesti.

 
##  <a name="setup-power-bi-batch-processing-dashboard"></a>Power BI erän käsittely Raporttinäkymät-ikkunan asetukset

>[AZURE.NOTE] Kuluu pääty eräkäsittely putkijohto suorittaminen loppuun ja käsitellä vuoden nykyarvo luotujen tietojen noin kaksi tunnit (käyttöönoton onnistuminen). Odota niin loppuun, ennen kuin suoritat seuraavat vaiheet käsittelyä varten. 

**Lataa suunnittelutyökalun PowerBI-tiedosto**

-   Valmiiksi määritetyt suunnittelutyökalun PowerBI-tiedosto on käyttöönoton mukana
-   Verkkokaavio-näkymän PowerBI-solmu ja valitse Ominaisuudet-ruudussa **Lataa PowerBI suunnittelutyökalun Tiedosto** -linkki![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/9.5-download-powerbi-designer.png)

-   Tallentaminen paikallisesti

**Määritä PowerBI-raportit**

-   Avaa suunnittelutyökalun Tiedosto "VehicleTelemetryAnalytics - työpöydän Report.pbix PowerBI-työpöytäsovelluksessa. Jos sinulla ei vielä ole, asenna PowerBI työpöydän [PowerBI työpöydän asentaminen](http://www.microsoft.com/download/details.aspx?id=45331). 

-   Valitse **Muokkaa kyselyt**.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/10-edit-powerbi-query.png)

- Kaksoisnapsauta **lähde**.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/11-set-powerbi-source.png)

- Päivitä palvelimen yhteysmerkkijonoa Azure SQL-palvelimeen, joka saatiin valmisteltu käyttöönoton osana. Valitse kaavio Azure SQL-solmu ja Näytä ominaisuudet-ruudussa palvelimen nimi.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/11.5-view-server-name.png)

- Jätä **tietokannan** *connectedcar*.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/12-set-powerbi-database.png)

- Valitse **OK**.
- Näkyviin tulee **Windows-tunnistetiedot** -välilehdessä valitsemat oletus muuttaminen tietokantaan **tunnistetiedot** napsauttamalla **tietokanta** -välilehden oikeassa.
- Anna **käyttäjänimi** ja **salasana** , Azure SQL-tietokannan, joka on määritetty käyttöönoton sen asennuksen aikana.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/13-provide-database-credentials.png)

- Valitse **Muodosta yhteys**
- Toista edellä kuvatut toimet ole oikeanpuoleisen ruudun kolme jäljellä kyselyt ja Päivitä tietolähteen yhteyden lisätiedot.
- Valitse **Sulje ja lataa**. Power BI Desktopin tiedoston tietojoukkoja on yhdistetty SQL Azure-tietokannan taulukoihin.
- **Sulje** Power BI Desktopin tiedosto.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/14-close-powerbi-desktop.png)

- Napsauta **Tallenna** -painiketta, Tallenna muutokset. 
 
Olet määrittänyt nyt kaikki erän käsittely polku ratkaisun vastaavat raportit. 


## <a name="upload-to-powerbicom"></a>*Powerbi.com* lataaminen
 
1.  Siirry PowerBI web-portaalissa osoitteessa http://powerbi.com ja kirjaudu sisään.
2.  Valitse **Nouda tiedot**  
3.  Lataa Power BI Desktopin tiedosto.  
4.  Jos haluat ladata, valitsemalla **Nouda tiedot -> tiedostot Hae paikallinen tiedosto ->**  
5.  Siirry **"VehicleTelemetryAnalytics – työpöydän Report.pbix"**  
6.  Kun tiedosto on ladattu palvelimeen, sinun tulee näkyviin takaisin Power BI-työ tilaa.  

Tietojoukko ja raportin tyhjä Raporttinäkymät-ikkunan luodaan puolestasi.  
 

PIN-kaavioiden avulla koontinäytön **Ajoneuvon analyysin Telemetriatietojen** **Power**BI. Valitse edellä luotu tyhjä Raporttinäkymät-ikkunan ja Selaa sitten **Raportit** -osa napsauttamalla juuri lataamasi raportti.  

![Ajoneuvon Telemetriatietojen PowerBI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard1.png) 


**Huomautus raportti on kuusi sivua:**  
Sivu 1: Ajoneuvon arvon.  
Sivu 2: Reaaliaikainen ajoneuvon kunto  
Sivu 3: Perustuva Aggressively ajoneuvot   
Sivu 4: Mieleen ajoneuvot  
Sivun 5: Polttoaineen perustuva tehokkaasti ajoneuvot  
Sivun 6: Contoso-Logo  

![Yhdistetyn autojen PowerBI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard2.png)
 

**Sivu 3**, PIN-tunnuksen seuraavasti:  

1.  VIN määrä  
    ![Yhdistetyn autojen PowerBI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard3.png) 

2.  Perustuva aggressively ajoneuvot mallin – vesiputouskaavion mukaan  
    ![Ajoneuvon Telemetriatietojen - kaavioiden PIN-tunnuksen 4](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard4.png)

**Sivun 5**, PIN-tunnuksen seuraavasti: 
 
1.  Vin määrä    
    ![Ajoneuvon Telemetriatietojen - kaavioiden PIN-tunnuksen 5](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard5.png)  
2.  Polttoaineen tehokas ajoneuvot mallin mukaan: liitetty pylväskaavio  
    ![Ajoneuvon Telemetriatietojen - kaavioiden PIN-tunnuksen 6](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard6.png)

**Sivu 4**, PIN-tunnuksen seuraavasti:  

1.  Vin määrä  
    ![Ajoneuvon Telemetriatietojen - kaavioiden PIN-tunnuksen 7](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard7.png) 

2.  Peruutetun ajoneuvot kaupungin perusteella, malli: Treemap  
    ![Ajoneuvon Telemetriatietojen - kaavioiden PIN-tunnuksen 8](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard8.png)  

**Sivun 6**, PIN-tunnuksen seuraavasti:  

1.  Contoso-ominaisuudet-logo  
    ![Ajoneuvon Telemetriatietojen - kaavioiden PIN-tunnuksen 9](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard9.png)

**Järjestää koontinäyttö**  

1.  Siirry koontinäyttöön.
2.  Pitämällä hiiren osoitinta kunkin kaavion ja nimeä se perustuvan nimeäminen säädetty valmis Raporttinäkymät-ikkunan alla olevassa kuvassa. Myös siirtyminen kaaviota muistuttavan alla Raporttinäkymät-ikkunan.  
    ![Ajoneuvon Telemetriatietojen - järjestää Raporttinäkymät-ikkunan 2](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-organize-dashboard2.png)  
    ![Ajoneuvon Telemetriatietojen PowerBI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard.png)
3.  Jos olet luonut kaikki mainittu tässä asiakirjassa, kuten seuraavassa kuvassa pitäisi nyt lopullinen valmiista raporttinäkymästä. 

![Ajoneuvon Telemetriatietojen - järjestää Raporttinäkymät-ikkunan 2](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-organize-dashboard3.png)

Onnittelen! Olet luonut raporttien ja koontinäytön artikkelista reaaliaikainen, ennakoivan ja erä havainnollistamisen ajoneuvon terveys- ja ajo-käyttöä.  

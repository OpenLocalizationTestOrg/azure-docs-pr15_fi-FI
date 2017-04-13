<properties 
    pageTitle="Lineaarisen regressiosuoran käyttäminen koneen Learning | Microsoft Azure" 
    description="Lineaarisen regressiosuoran mallien Excelissä ja Azure koneen Learning Studiossa vertailu" 
    metaKeywords="" 
    services="machine-learning" 
    documentationCenter="" 
    authors="garyericson" 
    manager="jhubbard" 
    editor="cgronlun"  />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/09/2016" 
    ms.author="kbaroni;garye" />

# <a name="using-linear-regression-in-azure-machine-learning"></a>Lineaarisen regressiosuoran käyttäminen Azure koneen oppiminen

> *Kate Baroni* *Ben Boatman* ovat enterprise-ratkaisun architects-Microsoftin tietojen havainnollistamisen huippuosaamisen. Tässä artikkelissa ne kuvaavat kokemuksensa käytössä olevan regression analysis-ohjelmistopaketin siirtämisestä käyttämällä Azure koneen Learning pilvipohjainen ratkaisu.  
 
&nbsp; 
  
[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]  
 
## <a name="goal"></a>Tavoite

Projektin aloittaminen mielessä kaksi tavoitteet:  

1. Ennakoivan analyysin avulla voit parantaa Microsoftin organisaation kuukausittain tuotto ennusteiden tarkkuutta  
2. Azure ML avulla voit vahvistaa, optimointi, Suurenna nopeutta, ja asteikon Microsoftin tulokset.  

Monet yritykset, kuten organisaation käy läpi kuukausittain tuotto, ennusteiden prosessi. Liiketoiminnan analyytikot pieni tiimimme on luovat tietokoneen Learning avulla tukevat prosessi ja ennusteen tarkkuuden parantamiseksi.  Ryhmän käytetyt useita kuukausia tietojen keräämisen useista lähteistä ja tietojen määritteet poikki kulkee tilastoanalyyseja tunnistaminen kiinnostavat palvelut myynnin ennusteiden määritteitä.  Seuraavat vaiheet on Aloita prototyyppiä tilastollinen regression malleissa tietoja Excelissä.  Muutaman viikon kuluessa oli Excel-regression malli, joka on outperforming nykyiseen kenttään ja talousmallit ennusteiden prosessit. Tämä on ollut perusaikataulun tekstinsyöttö tulos.  


Olemme sitten siirtäminen Microsoftin ennakoivan analytics päälle Azure ML ja katso, miten Azure ML voi parantaa ennakoivan suorituskykyyn kulunut seuraavaan vaiheeseen.


## <a name="achieving-predictive-performance-parity"></a>Ennakoivan suorituskyvyn välistä eroa saavuttamiseksi

Tutustu ensimmäisen prioriteetti on saavuttamiseksi välistä eroa Excel- ja Azure ML regression mallien välillä.  Annettu täsmälleen samat tiedot ja saman Jaa koulutus ja testaus on etsimäsi saavuttamiseksi ennakoivan suorituskyvyn välistä eroa Excel-ja Azure ML tiedot.   Alun perin on epäonnistui. Excel-mallin outperformed Azure ML-malli.   Virhe vuoksi puuttui tietoa Azure ML perus tool-asetusta. Azure ML tuotetiimin synkronointiin on saatu paremman käsityksen määrittäminen edellyttää Microsoftin tietojoukot base ja saavuttaa välistä eroa kaksi mallien välillä.  

### <a name="create-regression-model-in-excel"></a>Regressiosuoran mallin luominen Excelissä
Tutustu Excel-regressio käyttää Excel-Analyysityökalut-kohdan vakio lineaarisen regressiosuoran malli. 

Olemme lasketaan *suora % virhe tarkoittaa* ja käyttää sitä mittaa suorituskykyä mallin.  Kului 3 kuukauden vuoroon toimimasta mallin Excelissä.  Olemme tuodut paljon learning Azure ML kokeen hyödyllisiin tietoja vaatimukset kädessä tuli kyselyjä.

### <a name="create-comparable-experiment-in-azure-machine-learning"></a>Vastaa kokeen luominen Azure koneen oppiminen  
Olemme ja tutustu kokeen luominen Azure ML seuraavasti:  

1.  Ladattu dataset Azure ml (erittäin pienen tiedoston) csv-tiedostona
2.  Luo uusi koe ja [Valitse sarakkeet-tietojoukko] käytetään[ select-columns] moduulin, valitse samoja tietoja ominaisuuksia käytetään Excelissä   
3.  [Jaa tietoja] käytetään[ split] moduuli ( *Suhteellinen lausekkeen* tilassa) voit jakaa tietoja joukkoihin täsmälleen saman junassa kuin oli suoritettu Excelissä  
4.  [Lineaarisen regressiosuoran] kanssa experimented[ linear-regression] moduuli (oletusasetuksia vain), kuvattu ja verrattuna Microsoftin Excelin regression mallin tulokset

### <a name="review-initial-results"></a>Alkuperäinen tulosten tarkastelu
Ensimmäinen Excel-mallin outperformed selkeästi Azure ML-malliin:  

|   |Excel|Azure ML|
|---|:---:|:---:|
|Suorituskyky|   |  |
|<ul style="list-style-type: none;"><li>Säätää R neliö</li></ul>| 0.96 |PUUTTUU|
|<ul style="list-style-type: none;"><li>Kertoimen. <br />Määrittäminen</li></ul>|PUUTTUU|   0.78<br />(pieni tarkkuus)|
|Suora virhe tarkoittaa |  $9. 5M|  $ 19.4 M|
|Keskimääräinen suora virhe (%)|   6.03 %|  12.2: %

Kun olemme suoritettiin sekä prosessin ja tulokset sovelluskehittäjät ja tietojen tutkijoiden Azure ML ryhmän, ne nopeasti sille hyödyllisiä vihjeitä.  

* Kun käytät [Lineaarisen regressiosuoran] [ linear-regression] moduulissa Azure ML hakuja kahdella tavalla:
    *  Liukuvärin laskeutumisnopeus online: Voi olla suurempi skaalaus ongelmia logolle
    *  Tavallinen neliösumman: Tämä on Useimmat käyttäjät Ota huomioon, kun ne kuulla lineaarisen regressiosuoran. Pieni tietojoukkoja tavallisen neliösumman voi olla useampi paras vaihtoehto.
*  Harkitse tweaking tason 2 Regularization leveys-parametrin suorituskyvyn parantamiseksi. Muutos iteraatiokertojen 0,001 on käytössä oletusarvoisesti ja tutustu small tietojoukolle, on määritetty 0,005 suorituskyvyn parantamiseksi.    

### <a name="mystery-solved"></a>Mystery ratkaista!
Kun on lisätty suositukset, emme saavuttaa suorituskyvyn Azure ml kuin Excel saman perusarvo:   

|| Excel|Azure ML (nimikirjain)|Azure ML ja pienimmän neliösumman|
|---|:---:|:---:|:---:|
|Nimetty arvo  |Toteutuneita arvoja (numero)|sama|sama|
|Paremmin katselemalla  |Excel -> tietojen analysointi -> regressio|Lineaarisen regressiosuoran.|Muuttujan lineaarinen regressio|
|Paremmin katselemalla asetukset|PUUTTUU|Oletusarvo|Tavallinen neliösumman<br />TASON 2 = 0,005|
|Tietojoukon|26 rivit, 3 ominaisuuksia, otsikko 1.   Kaikki numeerinen.|sama|sama|
|Jaa: junassa|Excelin koulutus ensin 18 riveillä testattu viimeksi 8 riveille.|sama|sama|
|Jaa: testi|Excelin keskivirheen kaava viimeksi 8 rivit|sama|sama|
|**Suorituskyky**||||
|Säätää R neliö|0.96|PUUTTUU||
|Korrelaatiokerroin|PUUTTUU|0.78|0.952049|
|Suora virhe tarkoittaa |$9. 5M|$ 19.4 M|$9. 5M|
|Keskimääräinen suora virhe (%)|<span style="background-color: 00FF00;">6.03 %</span>|12.2: %|<span style="background-color: 00FF00;">6.03 %</span>|

Lisäksi Excel-kertoimet verrattuna hyvin Azure koulutetun mallin ominaisuus-leveydet:

||Excelin kertoimet|Azure ominaisuus leveydet|
|---|:---:|:---:|
|Leikkauspiste/hajonnan|19470209.88|19328500|
|Ominaisuus A|0.832653063|0.834156|
|Ominaisuuden B|11071967.08|11007300|
|Ominaisuuden C|25383318.09|25140800|

## <a name="next-steps"></a>Seuraavat vaiheet

Olemme määrittää tarjoaman Azure ML verkkopalvelun Excelissä.  Tutustu liiketoiminnan analyytikot riippuvaisia Excel ja Microsoft tarvitaan voi soittaa Azure ML WWW-palvelun kanssa Excel-tietoja sisältävä rivi ja sen budjetoidut palautusarvo Excelissä.   

On myös määrittää optimoi Microsoftin mallin asetukset ja algoritmit Azure ML käytettävissä.

### <a name="integration-with-excel"></a>Excel-integrointi
Microsoftin ratkaisun oli operationalize Microsoftin Azure ML regression mallin luomalla verkkopalvelun koulutetun mallista.  Muutaman minuutin kuluessa web-palvelu on luotu ja emme voi soittaa suoraan Excelistä, funktio palauttaa arvon budjetoidut tuotto.    

*Web Services-koontinäytöstä* -osassa on ladattava Excel-työkirja.  Työkirja sisältää valmiiksi muotoilluille web service API- ja rakennetiedostot tiedot upotettu.   Kun napsautat *Lataa Excel-työkirjan*, se avautuu ja voit tallentaa sen paikalliseen tietokoneeseen.    

![][1]
 
Avaa työkirja, jossa kopioi ennalta määritetyt parametrit on sininen parametri-osassa alla kuvatulla tavalla.  Kun parametrit on annettu, Excel kutsuu AzureML web-palveluun ja budjetoidut tulos otsikot näkyy vihreä ennustetut arvot-kohdassa.  Työkirjan edelleen luoda ennusteiden parametrien perusteella kaikki parametrit-kenttään rivikohteiden koulutetun malli.   Lisätietoja siitä, miten voit käyttää tätä toimintoa Tarkista [Azure koneen Learning verkkopalvelun Excelistä](machine-learning-consuming-from-excel.md). 

![][2]
 
### <a name="optimization-and-further-experiments"></a>Optimointiin ja edelleen kokeissa
Nyt kun on ollut perusaikataulun meidän Excel-malli, on siirretty eteenpäin optimoi Microsoftin Azure ML lineaarisen regressiosuoran mallin.  On käytetty moduulin [Suodatin-pohjainen ominaisuudet] [ filter-based-feature-selection] parantamisesta Microsoftin valintaa aloitustiedot us saavuttamiseksi 4.6 %: n suorituskyvyn parantaminen elementit ja se auttoi tarkoittaa suora virhe.   Tulevien projektien Käytämme tämä toiminto, jolla voi tallentaa us viikon tietojen määritteet löydät oikean käytettävissä olevia ominaisuuksia käytettävät mallintaminen läpikäyminen.  

Seuraavaksi on aio sisällyttää muita algoritmeista, kuten [Bayesian] [ bayesian-linear-regression] tai [Tehosti päätös puita] [ boosted-decision-tree-regression] Microsoftin kokeessa vertaileminen suorituskykyä.    

Voit halutessasi kokeilla regressio hyvä tietojoukko yrittää on energiajärjestelmien tehokkuuden regressio otoksen tietojoukko, on paljon numeeriset määritteet. Tietojoukon annetaan esimerkki tietojoukkoja ML Studiossa osana.  Voit käyttää monenlaisia Koulujen moduulit ennustaa lämmitys kuormituksen tai jäähdytys kuormituksen.  Alla olevassa kaaviossa on eri regressio suorituskyvyn vertailu oppii vastaan energiajärjestelmien tehokkuuden tietojoukko korrelaatio kohde muuttujan Cooling kuormituksen: 

|Malli|Suora virhe tarkoittaa|Pääkansio keskiarvo neliö virhe|Suhteellinen suora virhe|Suhteellinen neliö virhe|Korrelaatiokerroin
|---|---|---|---|---|---
|Tehosti Päätöspuun|0.930113|1.4239|0.106647|0.021662|0.978338
|Lineaarisen regressiosuoran (liukuvärjäys laskeutumisnopeus)|2.035693|2.98006|0.233414|0.094881|0.905119
|Neural verkon regressio|1.548195|2.114617|0.177517|0.047774|0.952226
|Lineaarisen regressiosuoran (tavallisen pienimmän neliösumman)|1.428273|1.984461|0.163767|0.042074|0.957926  

## <a name="key-takeaways"></a>Tärkeimmät Takeaways 

Olemme opitut asiat usein mukaan käytössä Excel regression- ja Azure koneen Learning kokeilua rinnakkain. Perusaikataulun mallin luomisesta Excelissä ja vertaamalla käyttämällä Azure ML [Lineaarisen regressiosuoran] [ linear-regression] auttoi meille tietoja Azure ML ja olemme havaitsi mahdollisuudet tietojen valinta ja mallin suorituskyvyn parantamiseksi.         

Myös löydetyillä että on suositeltavaa käyttää [Suodatin-pohjainen ominaisuudet] [ filter-based-feature-selection] nopeuttamiseksi tulevien tekstinsyöttö projektit.  Soveltaminen tietojen ominaisuudet, voit luoda parannettu mallin Azure millilitroina kanssa yleinen suorituskyvyn parantamiseksi. 

Voi siirtää ennakoivan analyyttisten ennusteiden from Azure ML Exceliin systeemisesti sallii merkittävästi voi tarjota onnistuneesti laaja business käyttäjän käyttäjäryhmälle tulokset.     


## <a name="resources"></a>Resurssit
Resurssien näkyvät suojaamisessa regression käyttäminen:  

* Regressiosuoran Excelissä.  Jos olet kokeillut koskaan regression Excelissä, tässä opetusohjelmassa on helppo: [http://www.excel-easy.com/examples/regression.html](http://www.excel-easy.com/examples/regression.html)
* Regressiosuoran ja ennustamista varten.  Tyler Chessman kirjoittamasi kuvaaminen ajan sarjan ennusteiden Excelissä, jossa on hyvä aloittelijan kuvaus lineaarisen regressiosuoran blogi-artikkelissa. [http://sqlmag.com/SQL-Server-Analysis-Services/Understanding-Time-Series-Forecasting-Concepts](http://sqlmag.com/sql-server-analysis-services/understanding-time-series-forecasting-concepts)  
*   Tavallinen neliösumman lineaarisen regressiosuoran: Virheet, ongelmista ja käsitellään.  Johdanto ja Regression keskustelun: [http://www.clockbackward.com/2009/06/18/ordinary-least-squares-linear-regression-flaws-problems-and-pitfalls/](http://www.clockbackward.com/2009/06/18/ordinary-least-squares-linear-regression-flaws-problems-and-pitfalls/ )

[1]: ./media/machine-learning-linear-regression-in-azure/machine-learning-linear-regression-in-azure-1.png
[2]: ./media/machine-learning-linear-regression-in-azure/machine-learning-linear-regression-in-azure-2.png


<!-- Module References -->
[bayesian-linear-regression]: https://msdn.microsoft.com/library/azure/ee12de50-2b34-4145-aec0-23e0485da308/
[boosted-decision-tree-regression]: https://msdn.microsoft.com/library/azure/0207d252-6c41-4c77-84c3-73bdf1ac5960/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
 

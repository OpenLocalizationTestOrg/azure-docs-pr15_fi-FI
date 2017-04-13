<properties
    pageTitle="Vaihe 3: Luo uuden tietokoneen Learning kokeen | Microsoft Azure"
    description="Vaihe 3 / kehittäminen vaiheittainen ennakoivan ratkaisu: Luo uusi koulutus kokeen Azure koneen Learning Studiossa."
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016" 
    ms.author="garye"/>


# <a name="walkthrough-step-3-create-a-new-azure-machine-learning-experiment"></a>Vaiheittainen kuvaus vaihe 3: Luo uusi Azure koneen Learning kokeen

Tämä on kolmas vaihe vaiheittaista [kehittäminen ennakoivan analytics-ratkaisun Azure koneen oppiminen](machine-learning-walkthrough-develop-predictive-solution.md)


1.  [Koneen Learning työtilan luominen](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  [Ladata olemassa olevia tietoja](machine-learning-walkthrough-2-upload-data.md)
3.  **Luo uusi koe**
4.  [Kouluttaminen ja arvioi mallit](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.  [WWW-palvelun käyttöön](machine-learning-walkthrough-5-publish-web-service.md)
6.  [Access web-palvelu](machine-learning-walkthrough-6-access-web-service.md)

----------

Tätä vaiheittaista seuraava vaihe on luoda kokeen koneen Learning Studiossa, joka käyttää on ladattu tietojoukko.  

1.  Studiossa Valitse **+ Uusi** ikkunan alareunassa.
2.  Valitse **KOKEILE**ja valitse sitten "Tyhjä kokeen". Kokeen oletusnimi alusta yläosassa ja nimeä se kuvaava

    > [AZURE.TIP] Se on hyvä Täytä **Yhteenveto** - ja **kuvaus** kokeen **Ominaisuudet** -ruudussa. Nämä ominaisuudet antaa sinulle mahdollisuuden asiakirjan kokeen niin, että kuka tahansa tarkastelee sen myöhemmin ymmärtävät tavoitteet ja menetelmät.

3.  Vasemmalla puolella kokeen moduuli-valikoimasta Kuvapohjan, laajenna **Tallennettu tietojoukkoja**.
4.  Etsi **Omat tietojoukkoja** luotuihin tietojoukko ja vedä alusta. Löydät myös dataset kirjoittamalla nimen **hakuruutuun yläpuolella valikoimasta** .  

##<a name="prepare-the-data"></a>Tietojen valmisteleminen
Voit tarkastella tietoja ensimmäiset 100 riviä ja tilastollisia tietoja koko tietojoukko tulosteen portti tietojoukko (pieni ympyrä alareunassa) valitsemalla ja sitten **Visualisoi**.  

Koska datatiedosto ei sisältää sarakeotsikot, Studio on tarjonnut yleinen otsikoita *(Sarake1, Sarake2 jne.)*. Hyvä otsikot eivät ole välttämättömiä luominen mallin, mutta ne helpottavat tietojen kokeilla käyttöä varten. Myös, kun olemme myöhemmin julkaista tämän mallin verkkopalvelun, otsikot avulla tunnistaa palvelun käyttäjän sarakkeet.  

Kaavaan lisätään sarakeotsikot [Muokkaa metatietojen] [ edit-metadata] moduuli.
[Muokkaa metatietojen] käytön[ edit-metadata] moduuli, voit muuttaa tietojoukko liittyviä metatietoja. Tässä tapauksessa antaa Lisää sarakeotsikoiden helpossa muodossa nimiä. 

[Muokkaa metatietojen]käytön[edit-metadata], sinun on ensin määritettävä mitkä sarakkeet (eli tässä tapauksessa kaikki tavat.) Seuraavaksi voit määrittää toiminnon suorittaminen niiden sarakkeiden (eli tässä tapauksessa muuttaminen sarakeotsikot.)

1.  Kirjoita **hakukenttään** "metatietojen" moduuli-valikoimasta. Näkyviin tulee [Muokkaa metatietojen] [ edit-metadata] moduuli-luettelossa.
2.  Napsauta ja vedä [Muokkaa metatietojen] [ edit-metadata] moduulin sivulle alusta ja pudota se alla on lisätty aiemmin tietojoukko.
3.  Tietojoukon yhdistäminen [Muokkaa metatietojen][edit-metadata]: Valitse tulosteen portti DataSet (pieni ympyrää alareunassa olevan tietojoukon.) Vedä seuraavaksi [Muokkaa metatietojen] syötteen portin[ edit-metadata] (pieni ympyrä moduulin yläreunassa), vapauta hiiripainike. Tietojoukko ja moduulin pysyvät yhteydessä, vaikka voit siirtää joko alusta.

    Kokeen pitäisi näyttää tältä:  

    ![Muokkaa metatietojen lisääminen][2]
    
    Punainen huutomerkki ilmaisee, että emme ole määritetty tämän moduulin ominaisuuksien vielä. Olemme Tee, että seuraava.
    
    > [AZURE.TIP] Voit lisätä kommentin moduulin kaksoisnapsauttamalla moduulin ja tekstin kirjoittaminen. Tämä auttaa näet yhdellä silmäyksellä, mikä moduulin jokaisen oman kokeessa. Kaksoisnapsauta tässä tapauksessa [Muokkaa metatietojen] [ edit-metadata] moduuli ja Kirjoita kommentti "Lisää sarakeotsikot". Napsauta mitä tahansa muuta kohtaa Sulje tekstiruutu alusta. Voit näyttää kommentin, valitse moduuli alanuolta.

4.  Valitse [Muokkaa metatietojen][edit-metadata], valitse **Ominaisuudet** -ruudun oikealla puolella alusta **Käynnistä sarakevalitsin**.
5.  **Valitse sarakkeet** -valintaikkunassa, valitse kaikki rivit **Käytettävissä olevat sarakkeet** ja valitse > Siirrä ne **Valitut sarakkeet**.
Valintaikkunan pitäisi näyttää tältä: ![sarakevalitsin kaikki valitut sarakkeet][4]
7.  Napsauta **OK** -valintamerkkiä.
8.  Takaisin- **Ominaisuudet** -ruudussa Etsi **Uusi sarakkeiden nimet** -parametrin. Kirjoita tähän kenttään nimiluettelon 21 sarakkeiden tietojoukossa erottaa ne toisistaan puolipisteillä ja -sarakkeiden järjestys. Voit hankkia sarakkeiden nimet tietojoukko ohjeista UCI-sivustossa tai asiasta voit kopioida ja liittää seuraavassa luettelossa:  

          Status of checking account, Duration in months, Credit history, Purpose, Credit amount, Savings account/bond, Present employment since, Installment rate in percentage of disposable income, Personal status and sex, Other debtors, Present residence since, Property, Age in years, Other installment plans, Housing, Number of existing credits, Job, Number of people providing maintenance for, Telephone, Foreign worker, Credit risk  

    Ominaisuudet-ruutu näyttää tältä:

    ![Muokkaa metatietojen ominaisuuksien määrittäminen][1]

> [AZURE.TIP] Jos haluat tarkistaa sarakeotsikot, suorita kokeen (Valitse **Suorita** kokeen piirtoalustan alapuolella). Milloin se päättyy suorittaminen ( [Muokkaa metatietojen]näkyy vihreä valintamerkki[edit-metadata]), valitse tulosteen portti [Muokkaa metatietojen] [ edit-metadata] moduuli ja valitse **Visualisoi**. Voit tarkastella moduuliin tulosteen edistymistä tiedot – kokeen samalla tavalla.

##<a name="create-training-and-test-datasets"></a>Luo koulutus ja testaa tietojoukkoja

Kokeen seuraava vaihe on erillinen tietojoukkoja, käytetään sekä koulutus- ja testaus Microsoftin mallin luomiseen.

Voit tehdä tämän Käytämme [Jaetut tiedot] [ split] moduuli.  

1.  [Jaa tietojen] etsimistä[ split] moduuli, vedä alusta ja yhdistä se viimeisen [Muokkaa metatietojen] [ edit-metadata] moduuli.
2.  Oletusarvon mukaan Jaa suhde on 0,5 ja **Randomized Jaa** -parametri on määritetty. Tarkoittaa sitä, että tiedot satunnaisia puolet tulosteen läpi yksi portti [Jaetut tiedot] [ split] moduulin ja puolet – toiseen. Voit muuttaa näitä sekä **satunnainen lähde** -parametri, voit muuttaa tietojen testaus – koulutus Jaa. Tässä esimerkissä ne jätetään-on.
    
    > [AZURE.TIP] **Ensimmäisen tulostus-tietojoukko rivien nimittäjä** -ominaisuus määrittää, kuinka paljon tietoja siirretään vasemmalle tulostusportin kautta. Esimerkiksi jos määrität suhteen 0.7, valitse 70 % tiedot on vasen portin kautta tulostus ja 30 % oikean portin kautta.  
    
3. Kaksoisnapsauta [Jaetut tiedot] [ split] moduuli ja Kirjoita kommentti, "koulutus/testaaminen tietojen jakaminen 50 %". 

Käytämme [Jaetut tiedot] tulosteiden[ split] moduulin kuitenkin olemme, kuten, mutta japanin valita käytettävän vasemman tulosteen koulutus tiedot ja oikealla tulosteen kuin testauksen tiedot.  

Kuten edellä UCI sivustossa, misclassifying hyvin luottotietojen riskin mahdollisimman alhainen kustannus on viisi kertaa suurempi kuin misclassifying sellaisena kuin se on hyvin pieni luottotietojen riskin kustannukset. Laskemisesta tämä on luoda uusi tietojoukko, joka kuvastaa kustannukset-toiminto. Uusi tietojoukko riskejä esimerkin on replikoida viisi kertaa, kun kukin vähäinen Esimerkki ei replikoida.   

Emme voi tehdä tämän replikoinnin R-koodin avulla:  

1.  Etsi ja vedä [R-komentosarjan suorittaminen] [ execute-r-script] moduuli sivulle kokeen Kuvapohjan ja kytke vasemman tulosteen [Jaetut tiedot] [ split] [R-komentosarjan suorittaminen] ensimmäinen syötteen portti ("Dataset1")-moduulista[ execute-r-script] moduuli.
2. Kaksoisnapsauta [R-komentosarjan suorittaminen] [ execute-r-script] moduuli ja Kirjoita kommentti "Kustannusten määrittäminen".
2.  Valitse **Ominaisuudet** -ruudussa Poista **R komentosarja** -parametrin oletusteksti ja kirjoita tämä komentosarja:

          dataset1 <- maml.mapInputPort(1)
          data.set<-dataset1[dataset1[,21]==1,]
          pos<-dataset1[dataset1[,21]==2,]
          for (i in 1:5) data.set<-rbind(data.set,pos)
          maml.mapOutputPort("data.set")


Tämän saman replikoinnin käyttö kunkin tulostukseen [Jaetut tiedot] annettava[ split] moduulin niin, että tietojen koulutusta ja testauksen on sama kustannusten muuttaminen.

1.  Napsauta hiiren kakkospainikkeella [R-komentosarjan suorittaminen] [ execute-r-script] moduuli ja valitse **Kopioi**.
2.  Kokeen alusta hiiren kakkospainikkeella ja valitse **Liitä**.
3.  Kytke ensimmäisen syötteen, [R-komentosarjan suorittaminen] [ execute-r-script] moduulin oikealle tulosteen portin [Jaetut tiedot] [ split] moduuli. 
4.  Alusta loppuun Valitse **Suorita**. 

> [AZURE.TIP] Kopio, R-komentosarjan suorittaminen moduulin sisältää saman komentosarjan kuin alkuperäinen moduuli. Kun kopioit ja liität moduuli kangasta, kopioi säilyttää kaikki alkuperäisen ominaisuudet.  

Tutustu kokeen näyttää nyt tältä:

![Jaa-moduulin ja R-komentosarjojen lisääminen][3]

Katso lisätietoja käyttämisestä R-komentosarjoja kohdassa kokeissa, [Laajenna oman kokeen r](machine-learning-extend-your-experiment-with-r.md).

**Seuraavaksi: [junassa ja arvioi mallien](machine-learning-walkthrough-4-train-and-evaluate-models.md)**


[1]: ./media/machine-learning-walkthrough-3-create-new-experiment/create1.png
[2]: ./media/machine-learning-walkthrough-3-create-new-experiment/create2.png
[3]: ./media/machine-learning-walkthrough-3-create-new-experiment/create3.png
[4]: ./media/machine-learning-walkthrough-3-create-new-experiment/columnselector.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/

<properties
    pageTitle="Vaihe 4: Kouluttaminen ja arvioi ennakoivan analyyttisten mallien | Microsoft Azure"
    description="Kehittäminen ennakoivan ratkaisu vaiheittainen vaihe 4: junassa, pisteet ja arvioi Azure koneen Learning Studiossa useita malleja."
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
    ms.date="10/04/2016"
    ms.author="garye"/>


# <a name="walkthrough-step-4-train-and-evaluate-the-predictive-analytic-models"></a>Vaiheittainen kuvaus vaihe 4: Kouluttaminen ja arvioi ennakoivan analyyttisten mallit

Tässä artikkelissa on Neljäs vaihe vaiheittaista [kehittäminen ennakoivan analytics-ratkaisun Azure koneen oppiminen](machine-learning-walkthrough-develop-predictive-solution.md)


1.  [Koneen Learning työtilan luominen](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  [Ladata olemassa olevia tietoja](machine-learning-walkthrough-2-upload-data.md)
3.  [Luo uusi koe](machine-learning-walkthrough-3-create-new-experiment.md)
4.  **Kouluttaminen ja arvioi mallit**
5.  [WWW-palvelun käyttöön](machine-learning-walkthrough-5-publish-web-service.md)
6.  [Access web-palvelu](machine-learning-walkthrough-6-access-web-service.md)

----------

Yksi eduista Azure Konepohjaisten Oppimistekniikoiden Studio koneen learning malleja on mahdollisuus yritä mallin useamman kuin yhden tyypin kerrallaan kokeessa ja vertaa tuloksia. Tällaista vuorovaikutteisuudesta avulla löydät parhaan ratkaisun ongelmaasi.

Olemme kehittämässä tätä vaiheittaista kokeessa emme Luo kahdentyyppisiä malleja ja vertaa sitten niiden tulosmalli tulokset voit päättää, mitä algoritmin haluamme käyttämään Microsoftin lopullinen kokeessa.  

Emme voi valita eri mallien on. Saat käytettävissä olevat mallit-Laajenna moduuli-valikoiman **Koneen Learning** -solmu ja laajenna sitten **Alusta mallista** ja sen alle solmut. Tämän kokeen tarkoitetaan on valita tuki Vector Machine (SVM) ja kaksi luokan tehosti päätös puut moduulit.    

> [AZURE.TIP] Saat lisäohjeita siitä, mitä koneen Learning algoritmin parhaiten sopiva yrität ratkaisemaan tietty ongelma, katso, [miten voit valita Microsoft Azure koneen Learning algoritmeista](machine-learning-algorithm-choice.md).

##<a name="train-the-models"></a>Mallien kouluttaminen
Ensin määritetään tehosti päätös puun mallin:  

1.  Etsi [Kahden luokan tehosti Päätöspuun] [ two-class-boosted-decision-tree] moduulin moduuli-valikoimasta ja vedä se alusta.
2.  Etsi [Junassa mallin] [ train-model] moduuli, vedä alusta ja liitä tehosti päätös puun moduulin tulosteen [Junassa mallin] vasemman syötteen portin ("auttamaan kouluttamattomia malli")[ train-model] moduuli.
    
    [Kahden luokan tehosti Päätöspuun] [ two-class-boosted-decision-tree] moduulin alustaa yleinen malli ja [Kouluttaminen mallin] [ train-model] kouluttaminen mallin koulutus tietojen avulla. 
     
3.  Yhdistä vasemmalle [R-komentosarjan suorittaminen] vasemman tulos ("tuloksen tietojoukko")[ execute-r-script] moduulin oikealle syötteen portin ("tietojoukko") [Junassa mallin] [ train-model] moduuli.

    > [AZURE.TIP] Älä annettava kaksi syötteiden ja [R-komentosarjan suorittaminen] tulosteiden[ execute-r-script] tämän kokeen, jotta jäädä ne irrallisia moduuli. 

4.  Valitse [Junassa mallin] [ train-model] moduuli. **Ominaisuudet** -ruudussa valitsemalla **Käynnistä sarakevalitsin**, valitse **Kaikki** avattavan luettelon alaspäin **Käytettävissä olevat sarakkeet** -kohdassa ja kirjoita "Riskin luottokortti" tekstikenttään. Valitse **Kaikki** avattavassa valikossa **Valitut sarakkeet**-kohdassa. Valitse "Luottokortti riskin" ja valitse korostettu nuolipainiketta, kun haluat siirtää **Valitut sarakkeet**. 
5.  Valitse **Tallenna**.


Tämän osan kokeen näyttää nyt tältä:  

![Mallin koulutus][1]

Seuraavaksi kokeiluversion määrittäminen SVM-malli.  

Ensimmäinen SVM hieman kuvaus. Tehosti päätös puut toimivat hyvin kaikenlaisia ominaisuuksia. Kuitenkin jälkeen SVM moduulin Luo lineaarisia valitsin, malli, jonka se luo on paras testi-virhe, kun kaikki numeeriset ominaisuudet on sama asteikko. Jos haluat muuntaa kaikki numeeriset ominaisuudet samaa asteikkoa, Käytämme "Tanh-muunnos ( [Tietojen normalisointi] [ normalize-data] moduuli.) Tämä muunnoksia tässä numeroiden [0,1]-alueelle. Merkkijonon ominaisuudet muunnetaan SVM moduulin categorical ominaisuudet ja sitten binaarinen 0 ja 1-ominaisuuksia, joten ei tarvitse transform manuaalisesti merkkijonon ominaisuudet. Lisäksi et halua Muunna luottotietojen riskin sarake (sarake 21) - on numeerinen, mutta se on on olet koulutus mallin ennustaa, jotta annettava jätä se yksin arvo.  

Määrittää SVM-mallin, toimi seuraavasti:

1.  Etsi [Kahden luokan tuki Vector koneen] [ two-class-support-vector-machine] moduulin moduuli-valikoimasta ja vedä se alusta.
2.  Napsauta hiiren kakkospainikkeella [Junassa mallin] [ train-model] moduulista **Kopioi**alusta hiiren kakkospainikkeella ja valitse **Liitä**. [Junassa mallin] kopio[ train-model] moduuli on saman sarakkeen valinnan, alkuperäisen.
3.  SVM-moduulin tulosteen yhdistäminen toisen [Junassa mallin] vasemman syötteen portin ("auttamaan kouluttamattomia malli")[ train-model] moduuli.
4.  [Tietojen normalisointi] etsiminen[ normalize-data] moduulin ja vedä se alusta.
5.  Tämä moduuli syötteen yhdistäminen vasemman [R-komentosarjan suorittaminen] vasemman tulosteen[ execute-r-script] moduuli (Huomaa, että tulostusportin moduulin voidaan yhdistää useita moduuli).
6.  Kytke vasemman tulos ("muuntaa tietojoukko") [Tietojen normalisointi] [ normalize-data] moduulin oikealle syötteen portin ("tietojoukko") toisen [Junassa mallin] [ train-model] moduuli.
7.  [Tietojen normalisointi] **Ominaisuudet** -ruudussa[ normalize-data] moduulin, valitse **Tanh** **muunnos menetelmän** parametrin.
8.  Valitsemalla **Käynnistä sarakevalitsin**, valitse "Ei ole sarakkeita" **Ala merkillä**, valitse **Sisällytä** ensimmäisen avattavan luettelon, valitse **Saraketyyppi** toinen avattavassa valikossa ja valitse **numeerinen** kolmannen avattavasta valikosta. Määrittää, että kaikki numeeristen sarakkeiden (ja vain numeerinen) muutetaan.
9.  Napsauttamalla plusmerkkiä (+) oikealla puolella rivi – Tämä luo rivin avattavista luetteloista. Valitse ensimmäinen avattavassa valikossa **Jätä pois** , valitse **sarakkeiden nimet** toisen avattavasta valikosta tekstikenttään ja valitse "Riskin luottokortti" sarakkeet-luettelosta. Määrittää, että luottotietojen riskin sarakkeen ohitetaan (annettava toiminto, koska tämä sarake on numeerinen ja jotta muussa muuntaa).
10. Valitse **OK**.  


[Tietojen normalisointi] [ normalize-data] moduuli on nyt määritetty suorittamaan kaikki paitsi luottotietojen riski-sarakkeen numeeristen sarakkeiden Tanh-muunnos.  

Tämän osan Microsoftin kokeen pitäisi näyttää seuraavanlaiselta:  

![Toinen mallin koulutus][2]  

##<a name="score-and-evaluate-the-models"></a>Pisteet ja arvioi mallit

Käytämme testauksen tiedot, jotka on erotettu [Jaetut tiedot] [ split] moduulin sija Microsoftin koulutetun mallit. Kaksi mallia nähdäksesi, joka on luotu paremmat tulokset tulokset jälkeen voit verrata.  

1.  Etsi [Pistemäärän mallin] [ score-model] moduulin ja vedä se alusta.
2.  Yhdistä [Junassa mallin] [ train-model] moduuli, jolla on yhdistetty [Kahden luokan tehosti Päätöspuun] [ two-class-boosted-decision-tree] moduulin vasemmalle syötteen portin [Pistemäärän mallin] [ score-model] moduuli.
3.  Kytke oikean syötteen [Tulos mallin] [ score-model] oikean [R-komentosarjan suorittaminen] vasemman tulosteen moduulin[ execute-r-script] moduuli.

    [Tulos mallin] [ score-model] moduuli voi nyt luottokortin tiedot suoraan testauksen tiedot, suorita mallin avulla ja vertailla ennusteiden mallin Luo testauksen tietojen todellinen luottotietojen riski-sarake.

4.  Kopioiminen ja liittäminen [Pistemäärän mallin] [ score-model] moduulin kopioiminen luomiseen ja vedä uusi moduuli alusta.
5.  Kytke vasemman syötteen tämän moduulin SVM-malli (eli yhdistäminen [Junassa mallin] tulostusportin[ train-model] moduuli, jolla on yhdistetty [Kahden luokan tuki Vector koneen] [ two-class-support-vector-machine] moduuli).
6.  SVM-mallin on voit tehdä saman muunnos testi-tietoihin on samoin koulutus-tietoihin. Kopioiminen ja liittäminen [Tietojen normalisointi] niin[ normalize-data] moduulin Luo toinen kopio ja liitä se oikean [R-komentosarjan suorittaminen] vasemman tulosteen[ execute-r-script] moduuli.
7.  Kytke oikean syötteen [Tulos mallin] [ score-model] [Tietojen normalisointi] vasemman tulosteen moduulin[ normalize-data] moduuli.  

Kaksi tulosmalli tulokset arvioidaan Käytämme lisääminen [Arvioi mallin] [ evaluate-model] moduuli.  

1.  Etsi [Arvioi mallin] [ evaluate-model] moduulin ja vedä se alusta.
2.  Kytke vasemman syötteen [Tulos mallin] tulostusportin[ score-model] tehosti päätös puun mallin yhdistetty moduuli.
3.  Kytke oikean syötteen toinen [Pistemäärän mallin] [ score-model] moduuli.  

Voit suorittaa kokeen valitsemalla **Suorita** -painike alla alusta. Voi kestää muutaman minuutin kuluttua. Pyörivä ilmaisin, valitse kunkin moduulin osoittaa, että se on käynnissä ja valitse sitten vihreä valintamerkki kun moduuli on valmis. Kun kaikki moduulit on valintamerkki, koe on suoritettu.

Kokeen pitäisi näyttää tältä:  

![Molempien arvioiminen][3]

Voit tarkistaa tulokset valitsemalla [Arvioi mallin] tulostusportin[ evaluate-model] moduuli ja valitse **Visualisoi**.  

[Arvioi mallin] [ evaluate-model] moduulin tuottaa kaaria ja arvot, joiden avulla voit verrata kahta scored mallien tuloksia. Voit tarkastella vastaanotin operaattori säännössä käytettävän ominaisuuden (OHJETIEDOSTO) kaaria, tarkkuus/peruuttaminen kaaria tai hissin kaaria tulokset. Näyttää tiedot sisältää sekaannusta matriisin, kumulatiivisen arvot-alueen käyrän (AUC) ja muita tietoja. Voit muuttaa raja-arvo, siirtämällä liukusäädintä vasemmalle tai oikealle ja katso, miten se vaikuttaa olevat arvot.  

Oikealla puolella kaavion Valitse **Scored tietojoukko** tai **Scored tietojoukko vertaileminen** Korosta liittyvät käyrä ja näyttää alla liittyvät arvot. Selite kaaria, valitse "Tulos tietojoukko" vastaa vasen syötteen portti [Arvioi mallin] [ evaluate-model] moduuli - tässä tapauksessa tämä on tehosti päätös puun malli. "Tulos tietojoukko vertaileminen" vastaa oikean syötteen portin – tässä tapauksessa SVM-malli. Kun napsautat nämä otsikot, mallin käyrän näkyy korostettuna ja näyttää vastaavat arvot seuraavassa kuvassa esitetyllä tavalla.  

![Mallien kaaria OHJETIEDOSTO][4]

Tarkastelemalla nämä arvot voit päättää, mitä mallin lähinnä antamalla haluamaasi tulosta. Voit siirtyä takaisin ja että kokeen muuttamalla arvot eri mallien käytöstä. 

> [AZURE.TIP] Suorita historiatietoihin on käytettävissä aina, kun suoritat kokeen kirjaa siitä, että iteraation. Voit tarkastella näitä Iteraatioita ja palaa halutut arvosarjat, valitsemalla **Näytä Suorita HISTORIA** piirtoalustan alapuolella. Voit myös napsauttaa **Edellisen Suorita** **Ominaisuudet** -ruudussa voit palata välittömästi edeltävän iteraation yhdessä on avoinna.
> 
Voit tehdä mitään yhteyttä kokeen iteraation kopion valitsemalla **Tallenna AS** piirtoalustan alapuolella. Kokeile **Yhteenveto** - ja **kuvaus** -ominaisuuksien avulla voit pitää kirjaa siitä, mitä olen yrittänyt kokeen toistoja.

>  Lisätietoja on artikkelissa [Hallitse kokeilla Iteraatioita Azure koneen Learning Studiossa](machine-learning-manage-experiment-iterations.md).  


----------

**Seuraavaksi: [Ota käyttöön web-palvelu](machine-learning-walkthrough-5-publish-web-service.md)**

[1]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/train1.png
[2]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/train2.png
[3]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/train3.png
[4]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/train4.png


<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[normalize-data]: https://msdn.microsoft.com/library/azure/986df333-6748-4b85-923d-871df70d6aaf/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[two-class-boosted-decision-tree]: https://msdn.microsoft.com/library/azure/e3c522f8-53d9-4829-8ea4-5c6a6b75330c/
[two-class-support-vector-machine]: https://msdn.microsoft.com/library/azure/12d8479b-74b4-4e67-b8de-d32867380e20/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/

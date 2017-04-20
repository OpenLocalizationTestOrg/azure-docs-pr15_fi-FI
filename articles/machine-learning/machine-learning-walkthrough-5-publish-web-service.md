<properties
    pageTitle="Vaihe 5: Machine Learning verkkopalvelun käyttöönotto | Microsoftin Azure"
    description="Vaihe 5 / kehittäminen ehkäisevän ratkaisu RE: ottaa käyttöön ennakoivia kokeen Machine Learning Studio WWW-palvelulle."
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


# <a name="walkthrough-step-5-deploy-the-azure-machine-learning-web-service"></a>Vaihe vaiheelta vaihe 5: Käyttöönotto Azure Machine Learning Web-palvelu

Tämä on viides vaihe vaiheelta [Azure Machine Learning ehkäisevän analytics ratkaisu kehittäminen](machine-learning-walkthrough-develop-predictive-solution.md)


1.  [Luo työtila Machine Learning](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  [Lataa tiedot](machine-learning-walkthrough-2-upload-data.md)
3.  [Luo uusi koe](machine-learning-walkthrough-3-create-new-experiment.md)
4.  [Junan ja arvioi mallit](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.  **Web-palvelun käyttöönotto**
6.  [Web-palvelun käyttö](machine-learning-walkthrough-6-access-web-service.md)

----------

Antaa toisten mahdollisuus käyttää olemme luonut tämän vaihekuvauksen ehkäisevän mallin, olemme käyttäjäpohjaista Azure-Web-palveluun.

Tähän asti olemme kokeillut koulutuksen meidän malli. Mutta käyttöönotettu palvelu ei enää aiotaan tehdä koulutus - se luo laskentamenetelmää Saavuttaaksemme meidän mallin käyttäjän syötteen mukaan. Tutustutaan tekemään joitakin valmistelu muuntaa tämän kokeen ***koulutus*** -kokeeseen niin ***ehkäisevän*** kokeilla. 

Tämä on kaksivaiheinen prosessi:  

1. Muuntaa *ehkäisevän kokeen* *koulutuksen kokeilla* luodut
2. Ottaa käyttöön ennakoivia kokeen WWW-palvelulle

Mutta ensin meidän on leikata alas hieman tästä kokeesta. Meillä tällä hetkellä on kaksi eri mallia kokeeseen, mutta haluamme yhtä mallia vain, kun emme käyttöön tämän WWW-palvelulle.  

Oletetaan, että olet päätimme, että tehosti puu-malli oli parempi malli. Joten Ensimmäinen tehtävä on poistaa [Kaksi-luokan tukea Vector koneen] [ two-class-support-vector-machine] ja moduulin moduulit, joka käytettiin koulutukseen sen. Haluat ehkä tehdä kopion kokeilla ensin valitsemalla **Tallenna nimellä** kokeen piirtoalustan alaosasta.

Meidän on poistaa seuraavat moduulit:  

- [2-luokan tukea Vector koneen][two-class-support-vector-machine]
- [Malli junan] [ train-model] ja [Tulos mallin] [ score-model] moduulit, jotka on yhdistetty siihen
- [Rajat tietojen vakioimiseksi] [ normalize-data] (molemmat niistä)
- [Mallin arvioiminen][evaluate-model]

Valitse moduuli ja paina Delete-näppäintä tai napsauttamalla moduulia hiiren kakkospainikkeella ja valitse **Poista**.

Nyt olemme valmiita ottamaan tämän mallin avulla [Kaksi-luokan Päätöksentekopuu tehosti][two-class-boosted-decision-tree].

## <a name="convert-the-training-experiment-to-a-predictive-experiment"></a>Koulutuksen kokeen muuntaminen ehkäisevän koe

Muuntaminen ehkäisevän koe neljä vaihetta:

1. Tallenna malli olemme koulutettuja olet ja korvaa sitten meidän koulutus-moduuleja
2. Leikkauksen poistaa moduuleja, joita käytettiin vain koulutuksen kokeen
3. Jos Web-palvelu hyväksyy syötteen ja jossa se luo tuloste

Onneksi kaikki kolme vaihetta voidaan toteuttaa valitsemalla **määrittää Web-palvelun** alareunassa kokeen alusta (Valitse **ehkäisevän Web service** -vaihtoehdon).

Kun valitset **Määritä Web-palveluun**, käydä monella tavalla:

- Koulutetut malli tallennetaan yhden **Mallin koulutettua** moduulina moduulin valikoimasta vasemmalta kokeen alustan (löydät sen **Koulutettujen mallit**-kohdassa).
- Poistetaan moduulien, joka käytettiin koulutukseen. Erityisesti:
  - [2-luokan tehosti Päätöksentekopuu][two-class-boosted-decision-tree]
  - [Malli junan][train-model]
  - [Tietojen jakaminen][split]
  - [R-komentosarja suorittaa] toisen[ execute-r-script] moduuli, joka on käytetty jotta koetus
- Koulutetut tallennetun mallin lisätään takaisin koe.
- **Web service-syötteen** ja **tulosteen Web service** -moduulit lisätään.

> [AZURE.NOTE] Koe on tallennettu välilehtiin, jotka on lisätty koe kangas yläreunassa kahdessa osassa: alkuperäinen koulutus-koe on **kokeilla koulutus**-välilehden ja **kokeilla Predictive**on juuri luotu ehkäisevän kokeeseen.

Meidän on toteutettava muita askeleen tämän tietyn kokeen kanssa.
Olemme lisännyt kaksi [R-komentosarja suorittaa] [ execute-r-script] moduulit tarjoavat painotus-funktiota, tiedot koulutuksesta ja testaukseen. Olemme ei tarvitse tehdä lopullisen mallin.

Poistaa yhden [R-komentosarja suorittaa] Machine Learning Studio[ execute-r-script] moduuli [Jaa] poistamisen[ split] moduuli. Nyt voimme nyt poistaa toisen ja muodostaa [Metadata Editor] [ metadata-editor] [Tulos mallin]avulla suoraan[score-model].    

Meidän koe pitäisi nyt näyttää seuraavalta:  

![Pisteytys koulutetun malli][4]  

> [AZURE.NOTE] Ihmettelet ehkä, miksi emme jättää UCI Saksan luottokortin tiedot dataset ehkäisevän kokeessa. Palvelua aiotaan käyttää käyttäjän tietojen alkuperäinen tietojoukon, niin miksi jättää alkuperäisen dataset-mallissa?
>
>On totta, että palvelu ei tarvitse alkuperäistä luottokortin tiedot. Mutta se on malli, joka sisältää tietoja, kuten kuinka monta saraketta on ja mitkä sarakkeet ovat numeerisia tietoja. Tämän rakenteen tiedot on tarpeen tulkita käyttäjän tiedot. Emme jätä näitä osia, jotka on kytketty siten, että tulosmalli moduuli on dataset-rakenteen, kun palvelu on käynnissä. Tietoja ei käytetä, rakenne.  

Suorita koe vielä kerran (Valitse **Suorita**). Jos haluat varmistaa, että malli toimii edelleen, valitse [Tulos mallin] tulosteen[ score-model] moduuli ja valitse **Näytä tulokset**. Näet, että alkuperäiset tiedot näytetään, sekä luotto-riski arvo ("tulos otsikot") ja tulosmalli todennäköisyys ("tulos todennäköisyydet".) 

## <a name="deploy-the-web-service"></a>Web-palvelun käyttöönotto

Voit ottaa koe perinteistä Web-palveluun tai uuden Web-palvelun perusteella Azure Resource Manageriin.

### <a name="deploy-as-a-classic-web-service"></a>Kuin perinteiset Web-palvelun käyttöönotto ###

Ottamaan classic verkkopalvelu meidän kokeilla johdetaan alla piirtoalusta **Käyttöön Web-palveluun** ja valitse **Verkkopalvelun käyttöönotto [Classic]**. Machine Learning Studio ottaa käyttöön WWW-palvelulle kokeeseen ja siirtää sinut kyseisen Web-palvelun dashboard. Tässä kokeessa (**snapshot tarkastella** tai **tarkastella uusinta**) palaa ja yksinkertainen testata Web-palvelun (ks. **testi Web-palveluun** alla). On myös tietoja tähän luoda sovelluksia, jotka voivat käyttää Web-palvelun (Lisätietoja tämän vaihekuvauksen seuraavassa vaiheessa).

![Web service-raporttinäkymät][6]

Voit määrittää palvelun napsauttamalla **CONFIGURATION** -välilehteä. Voit tässä muuttaa palvelunimi (se on antanut kokeen nimi oletusarvon mukaan) ja anna kuvaus. Voit myös antaa enemmän helppokäyttöinen otsikot syöttö- ja -sarakkeissa.  

![Määritä Internet-palvelu][5]  

### <a name="deploy-as-a-new-web-service"></a>Ota käyttöön uusi Web-palvelu

Ottamaan uuden Web-palvelun meidän kokeilla johdetaan napsauttamalla alapuolella piirtoalustaan ja **Ottaa käyttöön Web-palveluun [uusi]** **Ottaa käyttöön Web-palveluun** . Machine Learning Studio siirtää Azure Machine Learning palvelut kokeen käyttöönotto verkkosivun.

Internet-palvelun nimi ja valitse hinnoittelu suunnitelma. Jos sinulla on olemassa hinnoittelu suunnitelma voit valita sen, muuten on luotava palvelun hinta uuden suunnitelman. 

1.  **Hinta suunnitelman** avattava luettelo, valitse aiemman suunnitelman tai **uuden suunnitelman valitsemalla** -vaihtoehdon.
2.  Kirjoita **Suunnitelman nimi**, nimi, joka määrittää suunnitelman laskussa.
3.  Valitse **Kuukausittain suunnitelma tasoa**. Suunnitelman tasoa oletusarvon oletus-alue ja Web-palvelun suunnitelmat otetaan käyttöön kyseisellä alueella.

Valitse **Ota käyttöön** ja **pikaopas** -sivulta, että Web-palvelu avautuu.

Voit määrittää palvelun valitsemalla **Määritä** valikkovaihtoehdon. Voit tässä muuttaa palvelunimi (se on antanut kokeen nimi oletusarvon mukaan) ja anna kuvaus. 

Voit testata Web-palvelun valinta napsauttamalla **Testaa** valikkovaihtoehto (ks. **testi Web-palveluun** alla). Sovelluksia, jotka voivat käyttää Web-palvelun luomisesta saat napsauttamalla **Kulutettava** valikkovaihtoehto (Lisätietoja tämän vaihekuvauksen seuraavassa vaiheessa).

> [AZURE.TIP] Web-palvelu päivittää sen jälkeen, kun olet ottanut sen käyttöön. Esimerkiksi jos haluat muuttaa mallia, muokata koulutuksen kokeeseen, tweak mallin parametrit ja **ottaa käyttöön Web**-palvelu. Valitse **Ota käyttöön Web-palveluun [Classic]** tai **Ottaa käyttöön Web-palveluun [uusi]**. Kun otat kokeen, se korvaa Web-palvelun avulla nyt päivitetty malli.  

## <a name="test-the-web-service"></a>Voit testata Web-palvelu

### <a name="test-a-classic-web-service"></a>Kokeile perinteistä Web-palveluun

Voit testata palvelua Machine Learning Studio tai portaalin Azure Machine Learning Web-palvelut. Azure Machine Learning verkkopalveluiden portaalin testaaminen on se etu, joten voit ottaa käyttöön 

**Testaa Machine Learning Studio**

**DASHBOARD** -sivun kohdasta **Oletusarvoinen päätepiste** **testi** -painiketta. Dialogi pop-up ja kysy palvelun syöttötiedot. Nämä ovat samoja sarakkeita, jotka olivat Saksan luotto alkuperäisen riskin dataset-kohteen.  

Syötä tietoja ja valitse sitten **OK**. 

**Azure Machine Learning verkkopalveluiden portaalin testaaminen**

Valitse **Test** esikatselu-linkki **DASHBOARD** -sivun **Oletusarvoinen päätepiste**. Testisivu Azure Machine Learning verkkopalveluiden portaalin Web-palvelun päätepisteen avautuu ja kysyy palvelun syöttötiedot. Nämä ovat samoja sarakkeita, jotka olivat Saksan luotto alkuperäisen riskin dataset-kohteen.

Valitse **Test - vastaus**. 

Web-palvelun tiedot siirtyy kautta **Web service-syöte** -moduulista [Editorin metatiedot] kautta[ metadata-editor] moduuli, ja [Tulos mallin] [ score-model] moduuli, jossa se on tulos. Tulokset ovat sitten tulosteen **Web service tuotoksen**kautta WWW-palvelusta.

**Uuden Web-palvelun testaaminen**

Azure Machine Learning Web services portal Valitse sivun yläosassa **testi** . **Testaa** sivu avautuu ja voit syöttää tietoja palvelun. Näyttää syöttökentät vastaavat sarakkeet, jotka olivat Saksan luotto alkuperäisen riskin dataset-kohteen. 

Syötä tietoja ja valitse sitten **Testi pyyntö-vastaus**.

Testin tulokset tulevat näkyviin sivu-sarakkeen oikeassa reunassa. 

Testattaessa portaalin Azure Machine Learning Web-palvelut voit ottaa näytteen tiedot, joiden avulla voit testata pyyntö-vastaus-palvelua. Jos olet luonut Web-palvelun Machine Learning Studio, mallitietoja otetaan tiedot oman käytetyn mallin junaan.

> [AZURE.TIP] Tapaa, jolla Meillä on määritetty, ehkäisevän kokeen [Tulos mallin] syntyy kokonaan[ score-model] moduuli palautetaan. Tämä sisältää syöttää tietoja sekä luotto-riski arvo ja tulosmalli todennäköisyys. Jos haluat palauttaa jokin eri - esimerkiksi vain luotto riskin arvo - [Projektin sarakkeita] voi lisätä ja[ project-columns] [Tulos mallin] välillä moduuli[ score-model] ja **Web service-tulosteen** halua Web-palvelu palauttaa sarakkeiden poistamiseksi. 

## <a name="manage-the-web-service"></a>Web-palvelun hallinta

**Perinteinen Web-palvelun hallinta**

Kun olet ottanut perinteistä Web-palveluun, voit hallita [Azure klassinen portal](https://manage.windowsazure.com).

1. Kirjaudu sisään [Azure klassinen portal](https://manage.windowsazure.com).
2. Microsoftin Azure palvelut-paneelissa Valitse **MACHINE LEARNING**.
3. Valitse työtilan.
4. Valitse **Web serviceS** -välilehti.
5. Valitse Web-palvelu on luotu.
6. Valitse "default"-päätepiste.

Täältä voit suorittaa toimintoja, kuten tarkkailla, miten Web-palvelu tekee ja tee suorituskykyä tweaks muuttamalla kuinka monta samanaikaisen kutsuu palvelu pystyy käsittelemään.
Voit myös julkaista Web-palvelun Azure Marketplacen.

Lisätietoja Katso:

- [Päätepisteiden luominen](machine-learning-create-endpoint.md)
- [Skaalaus Web-palveluun](machine-learning-scaling-webservice.md)
- [Julkaise Azure Marketplacen Azure Machine Learning Web-palveluun](machine-learning-publish-web-service-to-azure-marketplace.md)

**Azure Machine Learning verkkopalveluiden portaalin Web-palvelun hallinta**

Kun olet ottanut käyttöön Web-palveluun, perinteistä tai uutta, voit hallita [Azure Machine Learning Web services portal](https://servics.azureml.net).

Voit valvoa Web-palvelun suorituskyky:

1. Kirjaudu sisään [Azure Machine Learning Web services portal](https://servics.azureml.net).
2. Valitse **Web-palvelut**.
3. Valitse Web-palvelu.
4. Valitse **Dashboard**.

----------

**Seuraava: [Web-palvelun käyttö](machine-learning-walkthrough-6-access-web-service.md)**

[1]: ./media/machine-learning-walkthrough-5-publish-web-service/publish1.png
[2]: ./media/machine-learning-walkthrough-5-publish-web-service/publish2.png
[3]: ./media/machine-learning-walkthrough-5-publish-web-service/publish3.png
[4]: ./media/machine-learning-walkthrough-5-publish-web-service/publish4.png
[5]: ./media/machine-learning-walkthrough-5-publish-web-service/publish5.png
[6]: ./media/machine-learning-walkthrough-5-publish-web-service/publish6.png


<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[metadata-editor]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[normalize-data]: https://msdn.microsoft.com/library/azure/986df333-6748-4b85-923d-871df70d6aaf/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[two-class-boosted-decision-tree]: https://msdn.microsoft.com/library/azure/e3c522f8-53d9-4829-8ea4-5c6a6b75330c/
[two-class-support-vector-machine]: https://msdn.microsoft.com/library/azure/12d8479b-74b4-4e67-b8de-d32867380e20/
[project-columns]: https://msdn.microsoft.com/en-us/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
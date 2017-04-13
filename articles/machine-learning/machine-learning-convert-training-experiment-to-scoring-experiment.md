<properties
    pageTitle="Muuntaa tietokoneen Learning koulutus-kokeen ennakoivan kokeen | Microsoft Azure"
    description="Voit muuntaa tietokoneen Learning-koulutusta koe, koulutus ennakoivan analytics-mallin ennakoivan kokeen voidaan ottaa käyttöön web-palvelu, johon haluat käyttää."
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
    ms.date="08/19/2016"
    ms.author="garye"/>

# <a name="convert-a-machine-learning-training-experiment-to-a-predictive-experiment"></a>Koneen Learning koulutus-kokeen muuntaminen ennakoivan kokeen

Azure koneen Learning mahdollistaa muodosta, Testaa ja ota käyttöön ennakoivan analytics-ratkaisuja.

Kun olet luonut ja iteroitava- *koulutus kokeilla* kouluttaminen ennakoivan analytics-malli ja julkaista sen avulla uusia tietoja pisteet, sinun täytyy valmisteleminen ja nopeuttaa oman koe, näkyvissä pistemäärä. Voit sitten ottaa tämän *ennakoivan kokeen* Azure web-palvelu niin, että käyttäjät voivat lähettää tietoja malliin ja saada yhteyttä malleilla.

Muuntamalla ennakoivan kokeen saat koulutetun mallin voidaan ottaa käyttöön WWW-palvelulle. WWW-palvelun käyttäjät lähettävät syöttötiedot mallin ja mallin lähettää takaisin tekstinsyöttö tulokset. Niin kuin voit muuntaa ennakoivan kokeen haluat kannattaa pitää mielessä, miten odotat muiden käytettävän mallin.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Muuntaa koulutus kokeen ennakoivan kokeen prosessissa on kolme vaihetta:

1.  Tallenna koneen learning malli, jotka olet koulutus ja korvaa konepohjaisten oppimistekniikoiden algoritmin ja [Junassa mallin] [ train-model] moduulit koulutetun tallennetun mallin kanssa.
2.  Poistaa vain, kun käytössäsi on näkyvissä pistemäärä moduulit voit kokeilla. Koulutus kokeen sisältää lukuisia moduulit, jotka tarvitaan koulutus, mutta ei tarvita, kun malli on koulutetun ja tä näkyvissä pistemäärä.
3.  Määritä missä oman kokeessa hyväksyt WWW-palvelun käyttäjän tiedot ja tietoja palautetaan.

## <a name="set-up-web-service-button"></a>Määrittää WWW-palvelun-painike

Kun olet suorittanut oman kokeen (kokeen alusta alareunassa**Suorita** painiketta), **Määrittää WWW-palvelun** valitsemalla (Valitse **Ennakoivan verkkopalvelu** -vaihtoehto) suorittaa puolestasi muuntaa koulutus kokeen ennakoivan kokeen kolme vaihetta:

1.  Se tallentaa koulutetun mallin (vasemmalta puolelta kokeen alusta)-moduulissa-valikoiman **Koulutetun mallit** -osassa moduuli korvaa konepohjaisten oppimistekniikoiden algoritmin ja [Junassa mallin] [ train-model] moduulit koulutetun tallennetun mallin kanssa.
2.  Se poistaa moduulit, joita selkeästi ei tarvita. Tämän esimerkin mukaan lukien [Jaetut tiedot][split], 2. [Pistemäärän mallin][score-model], ja [Arvioi mallin] [ evaluate-model] moduulit.
3.  Se luo WWW-palvelun syötteen ja tulosteen moduulit ja lisää ne oletuskansiot oman kokeessa.

Esimerkiksi seuraavat kokeen harjoittaa kahden luokan tehosti päätös puun mallin laskenta mallitietojen avulla:

![Koulutus kokeen][figure1]

Tämä kokeessa moduulit suorittaa lähinnä neljä eri toimintoja:

![Moduuli-Funktiot][figure2]

Kun muunnat koulutus-kokeen ennakoivan koe, jotkin näitä moduuleja ei enää tarvita tai niissä on eri tarkoitukseen:

- **Tiedot** - malli-tietojoukko tietoja ei käytetä aikana näkyvissä pistemäärä - WWW-palvelun käyttäjän antaa tiedot annettava tulos. Tämä tietojoukko tietotyyppejä, kuten metatietoja käytetään kuitenkin koulutetun mallin mukaan. Jotta haluat pitää dataset ennakoivan kokeessa, jotta se voi antaa metatiedot.

- **Prep** - tiedot, jotka lähetetään näkyvissä pistemäärä, mukaan näitä moduuleja voi tai ei ehkä ole tarpeen käsittelemään saapuvat tiedot.

    Esimerkiksi tässä esimerkissä otoksen tietojoukko saattaa olla puuttuvat arvot, ja se sisältää sarakkeet, joita ei tarvita kouluttaminen malli. Niin [SIIVOA puuttuvien tietojen] [ clean-missing-data] moduuli on sisällytetty käsittelevät puuttuvat arvot ja [Valitse sarakkeet-tietojoukko] [ select-columns] moduulin oli kyseiset ylimääräisiä sarakkeita pois tiedonkulun. Jos tiedät, että tiedot, jotka lähetetään näkyvissä pistemäärä – web-palvelu ei ole puuttuvat arvot, ja voit poistaa [SIIVOA puuttuvien tietojen] [ clean-missing-data] moduuli. Kuitenkin jälkeen [Valitse sarakkeet-tietojoukko] [ select-columns] moduuli auttaa määrittää toiminnoista on tulos, moduulin on edelleen.

- **Junassa** – kun mallin koulutus onnistuneesti voit tallentaa ne yhden koulutetun mallin moduuli. Nämä yksittäiset moduulit tilalle sitten koulutetun tallennetun mallin.

- **Tulos** - Jaa-moduulin tässä esimerkissä käytetään tietovirta jakautuvan testitiedot ja koulutus tiedot. Ennakoivan kokeessa tämä ei tarvita, ja ne voidaan poistaa. Vastaavasti 2. [Pistemäärän mallin] [ score-model] moduulin ja [Arvioi mallin] [ evaluate-model] moduulin käytetään vertaileminen tulokset testitiedot, jotta näitä moduuleja myös ei tarvita ennakoivan kokeessa. Jäljellä olevat [Pisteet mallin] [ score-model] moduulissa tarvitaan kuitenkin palauttaa tuloksen tuloksen WWW-palvelun kautta.

Näin tämän esimerkin ulkoasun jälkeen valitsemalla **Määritä Web-palvelu**:

![Muuntaa ennakoivan kokeen][figure3]

Tämä voi riittää oman kokeen voi ottaa käyttöön WWW-palvelun valmistelemisesta. Haluat ehkä kuitenkin tehdä tietyn käyttäjän kokeen lisätoimia.

### <a name="adjust-input-and-output-modules"></a>Säädä syöttö- ja moduulit

Koulutus-kokeessa käyttää koulutus tietojoukko ja Tiesitkö joitakin käsittely saat lomakkeen koneen learning algoritmin tarvittavat tiedot. Jos vastaanottamaan WWW-palvelun kautta haluamiasi tietoja ei tarvitse tämän käsittely, voit siirtää **Web-palvelu syötteen moduuli** eri solmu oman kokeessa.

Oletusarvon mukaan **Määrittää WWW-palvelun** siirtää **Web-palvelu input** -moduulin Tietovuo edellä olevassa kuvassa yläreunassa. Kuitenkin jos syöttötiedot ei tarvitse tämän käsittely, valitse voit manuaalisesti sijoittaa **WWW-palvelun syötteen** aiempia tietojen käsittely-moduulit:

![Web-palvelu syötteen siirtäminen][figure4]

WWW-palvelun kautta syöttötiedot nyt välittää suoraan pistemäärän malli-moduulin ilman mitään esikäsittely.

Vastaavasti oletusarvoisesti **Määrittää WWW-palvelun** siirtää tietoja tiedonkulun alareunassa Web palvelun tulostus-moduuli. Web-palvelu palauttaa käyttäjän tässä esimerkissä [Tulos mallin] tulosteen[ score-model] moduuli, joka sisältää valmis syöttötiedot vektorin plus lasketaan tulokset.

Jos esimerkiksi mieluummin palauttaa erilaiset -, vain tulosmalli tulokset ja ei koko vektorin syöttötiedot - vaihtoehdon, sinun on lisätä [Valitse sarakkeet-tietojoukko] [ select-columns] moduulin haluat jättää pois kaikki sarakkeet paitsi tulosmalli tulokset. [Valitse sarakkeet-tietojoukko] tulosteen Siirrä sitten **WWW-palvelun tulostus** -moduulin[ select-columns] moduuli:

![Web-palvelu tulosteen siirtäminen][figure5]

### <a name="add-or-remove-additional-data-processing-modules"></a>Lisää tai poista muut tietojenkäsittely moduulit

Jos määritettynä on muita moduulit oman kokeessa, joista tiedät ei tarvita aikana näkyvissä pistemäärä, ne voidaan poistaa. Koska on siirtynyt **WWW-palvelun input** -moduulin pisteen jälkeen tietojen käsittely-moduuleja, on esimerkiksi poistaa [SIIVOA puuttuvien tietojen] [ clean-missing-data] ennakoivan kokeen moduulia.

Tutustu ennakoivan kokeen näyttää nyt tältä:

![Lisää moduuliin poistaminen][figure6]

### <a name="add-optional-web-service-parameters"></a>Lisää WWW-palvelun valinnaisten parametrien

Haluat ehkä muuttaa moduulit toiminnan, kun palvelua käytetään WWW-palvelun käyttäjälle. *WWW-palvelun parametrien* avulla voit tehdä näin.

[Tietojen tuominen] on määrittäminen Yleinen esimerkki[ import-data] moduulin niin, että käyttöön WWW-palvelun käyttäjä voi määrittää jotakin muuta tietolähdettä, kun web-palvelu on käytettävissä. [Tietojen vieminen] tai[ export-data] moduulin niin, että toinen kohde voidaan määrittää.

Voit määrittää WWW-palvelun parametrien ja toisiinsa moduulin parametreja ja voit määrittää, ovatko ne pakollinen tai valinnainen. WWW-palvelun käyttäjä voi annettava arvoja näiden parametrien kun palvelua käytetään ja moduulin toiminnot muuttuvat vastaavasti.

Saat lisätietoja WWW-palvelun parametrien [Avulla Azure koneen Learning Web palvelun parametrien ] [ webserviceparameters].

[webserviceparameters]: machine-learning-web-service-parameters.md


## <a name="deploy-the-predictive-experiment-as-a-web-service"></a>Ottaa käyttöön ennakoivan kokeen web-palvelu

Nyt kun ennakoivan kokeen on valmisteltu riittävän, voit ottaa Azure web-palvelu. Web-palvelun avulla käyttäjät voivat lähettää tietoja malliin ja mallin palauttaa sen ennusteiden.

Saat lisätietoja valmis käyttöönottoprosessin [käyttöönotto Azure koneen Learning web-palvelu][deploy]

[deploy]: machine-learning-publish-a-machine-learning-web-service.md


<!-- Images -->
[figure1]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure1.png
[figure2]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure2.png
[figure3]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure3.png
[figure4]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure4.png
[figure5]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure5.png
[figure6]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure6.png


<!-- Module References -->
[clean-missing-data]: https://msdn.microsoft.com/library/azure/d2c5ca2f-7323-41a3-9b7e-da917c99f0c4/
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[export-data]: https://msdn.microsoft.com/library/azure/7a391181-b6a7-4ad4-b82d-e419c0d6522c/

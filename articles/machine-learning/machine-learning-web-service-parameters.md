<properties 
    pageTitle="Käytä Azure Konepohjaisten Oppimistekniikoiden WWW-palvelun parametrien | Microsoft Azure" 
    description="Miten Azure koneen Learning Web palvelun parametrien avulla voit muokata mallin toiminnan, kun web-palvelu on käytettävissä." 
    services="machine-learning" 
    documentationCenter="" 
    authors="raymondlaghaeian" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016" 
    ms.author="raymondl;garye"/>

#<a name="use-azure-machine-learning-web-service-parameters"></a>Azure Konepohjaisten Oppimistekniikoiden WWW-palvelun parametrien avulla

Azure koneen Learning WWW-palvelun luodaan julkaisemalla koe, joka sisältää moduulit määritettäviä parametreilla. Haluat ehkä muuttaa moduulin web-palvelu on käynnissä. *WWW-palvelun parametrien* avulla voit tehdä tämän tehtävän. 

[Tietojen tuominen] on määrittäminen Yleinen esimerkki[ reader] moduulin niin, että julkaistun WWW-palvelun käyttäjä voi määrittää jotakin muuta tietolähdettä, WWW-palvelun tulit. [Tietojen vieminen] tai[ writer] moduulin niin, että toinen kohde voidaan määrittää. Muita esimerkkejä [Ominaisuus hajautuksessa] bittien määrän muuttaminen[ feature-hashing] moduuli tai määrä haluamasi ominaisuudet [Suodatin-pohjainen ominaisuudet] [ filter-based-feature-selection] moduuli. 

Voit määrittää WWW-palvelun parametrien ja toisiinsa moduulin parametreja oman kokeessa ja voit määrittää, ovatko ne pakollinen tai valinnainen. WWW-palvelun käyttäjä voi annettava arvoja näiden parametrien WWW-palvelun puhelun aikana. 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


##<a name="how-to-set-and-use-web-service-parameters"></a>Määrittäminen ja käyttäminen WWW-palveluparametrit

Web-palvelun parametri määrittää napsauttamalla moduulin parametrin vieressä olevaa kuvaketta ja valitsemalla "Määrittäminen web-palvelun parametri". Tämä luo uusi Web-palvelun parametri ja muodostaa moduulin parametrin. Valitse web-palvelu on käytettävissä, kun käyttäjä voi määrittää WWW-palvelun parametrin arvo ja sitä käytetään moduuli-parametrin.

Kun määrität Web-palvelun parametri, voivat käyttää jotain muuta moduulin parametriä kokeessa. Jos määrität moduulista parametri liittyvät Web-palvelun parametri, voit kyseisen saman Web-palvelun parametri muiden moduulin, kunhan parametrin odottaa samantyyppisten arvon. Esimerkiksi jos Web-palveluparametri on numeerinen arvo, valitse se vain voidaan moduulin parametrien, joka olettaa numeerisen arvon. Kohdassa käyttäjä voi määrittää WWW-palvelun parametrin arvon, kun sitä käytetään kaikki liittyvät moduuliparametrit.

Voit päättää, säätää oletusarvon Web-palveluparametri. Jos teet-parametri on valinnainen WWW-palvelun käyttäjälle. Jos et anna oletusarvo-käyttäjän tarvitaan antamaan arvon, kun web-palvelu on käytettävissä.

WWW-palvelun API ohjeissa sisältää määrittämiseen Web-palveluparametri ohjelmallisesti WWW-palvelun käyttäminen WWW-palvelun käyttäjän tietoja.

>[AZURE.NOTE] Perinteinen verkkopalvelun API ohjeissa on tarkoitettu web-palvelu **raporttinäkymät-IKKUNAN** koneen Learning Studiossa **API ohjesivu** -linkin kautta. Web-palvelu on tarkoitettu uusi verkkopalvelun API ohjeissa palvelun **Kulutettava** -ja **Swagger API** [Azure koneen Learning Web Services](https://services.azureml.net/Quickstart) -portaalissa.


##<a name="example"></a>Esimerkki

Esimerkkinä oletetaan koe [Tietojen vieminen] on[ writer] moduuli, joka lähettää tietoja Azure-blob-säiliö. Olemme määrittää nimeltä "Blob polku-Web-palvelun parametri, joka sallii web Palvelukäyttäjä voi muuttaa polku-blob-säiliö, kun palvelua käytetään.

1.  Koneen Learning Studiossa [Vie tiedot] valitsemalla[ writer] moduulin napsauttamalla sitä. Sen ominaisuudet näkyvät ominaisuudet-ruudun oikealla puolella kokeen alusta.

2.  Määritä tallennustilan:

    - Valitse **Määritä tietojen kohde**-kohdassa "Azure-Blob-säiliö".
    - Valitse **Määritä todennustyyppi**, "Tilin".
    - Kirjoita tilin tiedot Azure-blob-säiliö. 
    <p />

3.  Napsauta oikealla puolella **polku blob-säilö parametri, joka alkaa**-kuvaketta. Se näyttää tältä:

    ![Web-palvelu Kyselyparametrin kuvake][icon]

    Valitse "Määrittäminen web-palvelun parametri".

    Tapahtuma lisätään kohtaan **WWW-palveluparametrit** -blob-säilö, joka alkaa polku"-niminen ominaisuudet-ruudun alareunassa. Tämä on WWW-palveluparametri, joka on nyt [Vie tiedot] liittyvät[ writer] moduuli-parametrin.

4.  Nimeämään Web-palveluparametri nimi, kirjoita "Blob polku" ja paina **Enter** -näppäintä. 
 
5.  Antamaan Web-palveluparametri oletusarvo napsauttamalla nimen oikealla puolella olevaa kuvaketta, valitse "Anna oletusarvo", lukuarvon (esimerkiksi "container1/output1.csv") ja paina **Enter** -näppäintä.

    ![Web-palvelun parametri][parameter]

6.  Valitse **Suorita**. 

7.  **Käyttöönotto Web** -palvelu ja valitse **Käyttöönotto verkkopalvelun [perinteinen]** tai **Käyttöönotto verkkopalvelun [uusi]** WWW-palvelun käyttöön.

WWW-palvelun käyttäjä voi määrittää uutta kohdetta nyt [Tietojen vieminen] [ writer] moduulin WWW-palvelun käytettäessä.

##<a name="more-information"></a>Lisätietoja

Lisää esimerkiksi artikkelissa [Koneen Learning blogi](http://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx)- [WWW-palvelun parametrien](http://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx) tapahtuma.

Lisätietoja Konepohjaisten Oppimistekniikoiden verkkopalvelun käyttämisestä on Katso, [miten tarjoaman julkaistu konepohjaisten oppimistekniikoiden web-palveluun](machine-learning-consume-web-services.md).



<!-- Images -->
[icon]: ./media/machine-learning-web-service-parameters/icon.png
[parameter]: ./media/machine-learning-web-service-parameters/parameter.png


<!-- Module References -->
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[reader]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[writer]: https://msdn.microsoft.com/library/azure/7a391181-b6a7-4ad4-b82d-e419c0d6522c/
 

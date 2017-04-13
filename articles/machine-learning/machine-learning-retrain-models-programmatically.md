<properties
    pageTitle="Suuntaamista koneen Learning mallien ohjelmallisesti | Microsoft Azure"
    description="Opettele ohjelmallisesti suuntaamista mallin ja Päivitä juuri koulutetun mallin käyttäminen Azure koneen Learning web-palveluun."
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
    ms.author="raymondl;garye;v-donglo"/>


# <a name="retrain-machine-learning-models-programmatically"></a>Suuntaamista koneen Learning mallien ohjelmallisesti  

Tätä vaiheittaista kerrotaan, miten suuntaamista ohjelmallisesti Azure koneen Learning verkkopalvelun C# ja tietokoneen Learning eräkäsittely-palvelun avulla.

Kun olet retrained mallin, seuraava vaihe vaiheelta Näytä päivittämisestä mallin käyttöön ennakoivan web-palvelu:

- Jos olet asentanut perinteinen verkkopalvelun koneen Learning Web Services-portaalissa, katso [suuntaamista perinteinen verkkopalvelun](machine-learning-retrain-a-classic-web-service.md). 
- Jos uusi web-palvelu on otettu käyttöön, katso [suuntaamista uusi verkkopalvelun avulla tietokoneen Learning hallinnan cmdlet-komennot](machine-learning-retrain-new-web-service-using-powershell.md).

Katso uudelleenkoulutuksen prosessin yleiskuvaus [suuntaamista koneen Learning malli](machine-learning-retrain-machine-learning-model.md).

Jos haluat aloittaa luodun uuden Azure resurssin vastuuhenkilön mukaan-web-palvelun kanssa, katso [suuntaamista nykyinen ennakoivan web-palvelu](machine-learning-retrain-existing-resource-manager-based-web-service.md).

## <a name="create-a-training-experiment"></a>Koulutus kokeen luominen
 
Tässä esimerkissä käytetään "Esimerkki 5: junassa-testi, arvioi binaarinen luokituksen: aikuisen tietojoukko"-mallit Microsoft Azure koneen Learning. 
    
Voit luoda kokeen seuraavasti:

1.  Kirjaudu sisään, Microsoft Azure koneen Koulujen Studio. 
2.  Napsauta oikeassa alakulmassa koontinäyttö, **Uusi**.
3.  Valitse Microsoft-Samples Esimerkki 5.
4.  Nimeä kokeessa, koe, alusta alkuun valitsemalla kokeen nimi "Esimerkki 5: junassa-testi, arvioi binaarinen luokituksen: aikuisen tietojoukko".
5.  Kirjoita laskenta-malli.
6.  Kokeen alareunassa Kuvapohjan, valitse **Suorita**.
7.  **Määritä web-palvelu** ja valitse **Retraining web-palveluun**. 

    ![Alkuperäinen kokeen.][2]

Kaavion 2: Alkuperäinen kokeen.

## <a name="create-a-predictive-experiment-and-publish-as-a-web-service"></a>Ennakoivan kokeen luominen ja julkaiseminen web-palvelu  

Seuraavaksi voit luoda Predicative kokeen.

1.  Kokeen alusta alareunassa **Määrittää WWW-palvelun** ja valitse **Ennakoivan Web-palveluun**. Tämä tallentaa mallin koulutetun mallina ja lisää web-palvelu ja moduulit. 
2.  Valitse **Suorita**. 
3.  Kun koe on suoritettu, valitse **Käyttöönotto verkkopalvelun [perinteinen]** tai **Ottaa käyttöön Web-palveluun [uusi]**.

## <a name="deploy-the-training-experiment-as-a-training-web-service"></a>Ottaa käyttöön koulutus kokeen koulutus web-palvelu

Voit suuntaamista koulutetun mallin koulutus koe, jonka loit Retraining web-palvelu on otettava käyttöön. Web-palvelu on yhdistetty *WWW-palvelun tulosteen* moduuli * [Junassa mallin] [ train-model] * moduuli voi tuottaa uudet koulutetun mallit.

1. Palaa koulutus kokeen vasemmanpuoleisessa ruudussa kokeissa-kuvaketta ja valitse sitten kokeen nimeltä laskenta-malli.  
2. Kirjoita Etsi kokeen kohteet-hakuruutuun web-palveluun. 
3. *Web-palvelu Input* -moduulin vetäminen kokeen alusta ja Yhdistä tulos *SIIVOA puuttuvien tietojen* moduuli.  Tällä varmistetaan, että uudelleenkoulutuksen tietojen käsitellään samalla tavalla kuin alkuperäiset koulutus-tiedot.
4. Vedä kaksi *verkkopalvelun tulosteen* moduulit kokeen alusta. Yhdistä *Junassa malli* -moduulin tulosteen ensimmäisen ja toisen *Arvioi mallin* moduulin tulosteen. Web-palvelu tulosteessa **Junassa** mallin antaa us koulutetun uusi malli. **Arvioi mallin** liitetty tulosteen palauttaa kyseisen moduulin tulostus on suorituskyvyn tulokset.
5. Valitse **Suorita**. 

Seuraavaksi on otettava käyttöön, koulutus kokeen kuin verkkopalveluun, joka tuottaa koulutetun mallin ja mallin arvioinnin tuloksen. Tämän vuoksi seuraava toimintojoukon ovat riippuvaisia onko käsittelet perinteinen verkkopalvelun tai uusi web-palveluun.  
  
**Perinteinen web-palvelu**

Kokeen alusta alareunassa **Määrittää WWW-palvelun** ja valitse **Käyttöönotto verkkopalvelun [perinteinen]**. WWW-palvelun **raporttinäkymät-ikkunan** näkyy Ohjelmointirajapinnan avain ja API ohjesivu eräkäsittely. Vain eräkäsittely menetelmää voi käyttää koulutetun malleja.

**Uusi web-palvelu**

Kokeen alusta alareunassa **Määrittää WWW-palvelun** ja valitse **Ota käyttöön Web-palvelu [uusi]**. Web Service Azure koneen Learning Web Services-portaalin Avaa web-palvelu käyttöönotto-sivulle. WWW-palvelun nimi ja valitse maksusuunnitelma ja valitse sitten **Ota käyttöön**. Vain eräkäsittely menetelmää voi käyttää koulutetun malleja

Kummassakin tapauksessa kokeen päätyttyä on käytössä, tuloksena olevat työnkulun pitäisi näyttää seuraavasti:

![Tuloksena olevat työnkulun suorittamisen jälkeen.][4]

Kaavion 3: Tuloksena työnkulun suorittamisen jälkeen.

## <a name="retrain-the-model-with-new-data-using-bes"></a>Suuntaamista mallin avulla BES uusilla tiedoilla

Tässä esimerkissä käytetään C# uudelleenkoulutuksen-sovelluksen luominen. Voit myös Python tai R sample code tämän tehtävän suorittamiseen.

Soita uudelleenkoulutusta API:

1. C#-konsolisovelluksen luominen Visual Studio (-> uusi projekti -> Windows Desktop -> Console-sovellus).
2.  Kirjaudu tietokoneeseen Learning WWW-palvelun-portaaliin.
3.  Jos käytät perinteistä verkkopalvelun, valitse **Perinteinen verkkopalveluihin**.
    1.  Valitse web-palvelu käsitellään.
    2.  Valitse oletusarvoinen päätepiste.
    3.  Valitse **tarjoaman**.
    4.  Valitse **Kulutettava** -sivulla **Otoksen** koodiosassa **erä**.
    5.  Siirry vaiheeseen 5.
4.  Jos käsittelet web-palvelussa, valitse **Web Services**.
    1.  Valitse web-palvelu käsitellään.
    2.  Valitse **tarjoaman**.
    3.  Valitse **erä**Kulutettava-sivulla **Otoksen** koodiosassa alareunassa.
5.  Kopioi eräkäsittely otoksen C# koodi ja liittää sen Program.cs-tiedostoon, varmistetaan, että nimitilan säilyy muuttumattomana.

Lisää Nuget-paketti Microsoft.AspNet.WebApi.Client kommenttien määritetyllä tavalla. Voit lisätä Microsoft.WindowsAzure.Storage.dll viittaus ensin joutua asentaa Microsoft Azure-tallennustilan Services client-kirjasto. Lisätietoja on artikkelissa [Windows liittyviä palveluja](https://www.nuget.org/packages/WindowsAzure.Storage).

### <a name="update-the-apikey-declaration"></a>Päivitä apikey-ilmoitus

Etsi **apikey** -ilmoitus.

    const string apiKey = "abc123"; // Replace this with the API key for the web service

**Kulutettava** sivun **Basic kulutus tiedot** -osassa Etsi perusavaimen ja kopioi se **apikey** -ilmoitus.

### <a name="update-the-azure-storage-information"></a>Päivitä Azuren tallennustilaan

BES sample code latauksia Azuren tallennustilaan tiedoston paikalliseen levyasemaan (esimerkiksi "C:\temp\CensusIpnput.csv"), käsittelee sen ja kirjoittaa tulokset takaisin Azure-tallennustilan.  

Suoritettava tehtävä on hakea tallennustilan tilin nimi, avain ja säilö tiedot tallennustilan tilin perinteinen Azure portaalin ja Päivitä vastaavien arvojen koodi. 

1. Kirjaudu perinteinen Azure-portaaliin.
1. Valitse vasemmassa siirtymisruudussa-sarakkeessa **tallennustilan**.
1. Valitse jokin tallentamiseen retrained mallin tallennustilan tilit-luettelosta.
1. Valitse **Hallitse pikanäppäimet**sivun alareunaan.
1. Kopioida ja tallentaa **Access perusavaimen** ja sulje ikkuna. 
1. Valitse sivun yläreunassa **säilöjen**.
1. Valitse aiemmin luotu säilö tai luoda uuden ja Tallenna nimi.

Etsi *StorageAccountName*, *StorageAccountKey*ja *StorageContainerName* ilmoitukset ja Päivitä arvot tallensit Azure-portaalista.

    const string StorageAccountName = "mystorageacct"; // Replace this with your Azure Storage Account name
    const string StorageAccountKey = "a_storage_account_key"; // Replace this with your Azure Storage Key
    const string StorageContainerName = "mycontainer"; // Replace this with your Azure Storage Container name
            
Voit myös on Varmista lähdetiedoston koodin määritettyyn sijaintiin. 

### <a name="specify-the-output-location"></a>Määritä tulostuskohde

Kun määrität tulostuskohde pyytää-paketti, *RelativeLocation* määritetyn tiedoston tiedostotunniste on määritettävä ilearner. 

Esimerkki:

    Outputs = new Dictionary<string, AzureBlobDataReference>() {
        {
            "output1",
            new AzureBlobDataReference()
            {
                ConnectionString = storageConnectionString,
                RelativeLocation = string.Format("{0}/output1results.ilearner", StorageContainerName) /*Replace this with the location you would like to use for your output file, and valid file extension (usually .csv for scoring results, or .ilearner for trained models)*/
            }
        },

>[AZURE.NOTE] Tulosteen sijaintien nimet saattaa poiketa olevia näiden vaiheiden järjestyksen, jossa web palvelun tulostus-toimintoon on lisätty perusteella. Koska olet määrittänyt tämän koulutus Kokeile kaksi tulostus, tulokset ovat tallennustilan sijaintitiedot molemmat arvosarjat.  

![Uudelleenkoulutuksen tulostus][6]

Kaavio 4: Uudelleenkoulutusta tulos.

## <a name="evaluate-the-retraining-results"></a>Arvioi uudelleenkoulutuksen tulokset
 
Kun käynnistät sovelluksen, tulos on voi käyttää arvioinnin tulokset URL-osoite ja Suojaussidokset tunnuksen.

Näet retrained mallin suorituskyvyn tulosten yhdistäminen *BaseLocation*, *RelativeLocation*ja *SasBlobToken* *output2* tulos tulokset kohteesta (katso edellä uudelleenkoulutuksen tulosteen kuvassa) ja liittämällä täydellinen URL-osoite selaimen osoiteriville.  

Tarkastele tuloksia määrittääksesi, onko juuri koulutetun mallin suorittaa tarpeeksi sekä korvaa aiemmin luotuun.

Kopioi tulosteen tulokset *BaseLocation*, *RelativeLocation*ja *SasBlobToken* , voit käyttää niitä uudelleenkoulutuksen aikana.

## <a name="next-steps"></a>Seuraavat vaiheet

Jos ennakoivan WWW-palvelun käyttöön valitsemalla **Käyttöön Web-palvelu [perinteinen]**, katso [suuntaamista perinteinen verkkopalvelun](machine-learning-retrain-a-classic-web-service.md).

Jos ennakoivan WWW-palvelun käyttöön valitsemalla **Ota käyttöön Web-palvelu [uusi]**, katso [suuntaamista uusi verkkopalvelun avulla tietokoneen Learning hallinnan cmdlet-komennot](machine-learning-retrain-new-web-service-using-powershell.md).

<!-- Retrain a New web service using the Machine Learning Management REST API -->


[1]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE01.png
[2]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE02.png
[3]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE03.png
[4]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE04.png
[5]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE05.png
[6]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE06.png
[7]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE07.png


<!-- Module References -->
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
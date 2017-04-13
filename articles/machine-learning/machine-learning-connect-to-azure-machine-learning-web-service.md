<properties
    pageTitle="Yhteyden muodostaminen tietokoneen Learning verkkopalvelun | Microsoft Azure"
    description="C# tai Python Muodosta yhteys Azure kone Learning Web-palvelu todennus-näppäintä."
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016" 
    ms.author="garye" />


# <a name="connect-to-an-azure-machine-learning-web-service"></a>Yhteyden muodostaminen Azure koneen Learning Web-palvelu

Azure koneen Learning Kehitystyökalut-toiminto on Web-palvelun API, jotta syötteen tiedoista ennusteiden reaaliajassa tai erä-tilassa. Azure koneen Learning Studio avulla voit luoda ennusteiden ja Azure koneen Learning WWW-palvelun käyttöön.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Lisätietoja luominen ja käyttöönotto koneen Learning verkkopalvelun koneen Learning Studiossa:

- Katso opetusohjelma luomisesta kokeen koneen Learning Studiossa, [Luo ensimmäinen kokeen](machine-learning-create-experiment.md).
- Lisätietoja WWW-palvelun ottamisesta käyttöön on artikkelissa [tietokoneen Learning verkkopalvelun käyttöönotto](machine-learning-publish-a-machine-learning-web-service.md).
- Lisätietoja tietokoneen oppiminen käy yleensä [Tietokoneen Oppimiskeskus ohjeissa](https://azure.microsoft.com/documentation/services/machine-learning/).

## <a name="azure-machine-learning-web-service"></a>Azure koneen Learning Web-palvelu ##

Azure koneen Learning Web-palvelun kanssa ulkoisen sovelluksen yhteydessä tietokoneen Learning mallin työnkulun reaaliajassa tulosmalli. Koneen Learning Web-Palvelukutsu palauttaa tekstinsyöttö tuloksia ulkoiseen sovellukseen. Jotta tietokoneen Learning verkkopalvelun kutsu välittää API-näppäintä, joka on luotu, kun otat käyttöön tekstinsyöttö. Koneen Learning WWW-palvelun perustuu REST-ohjelmointi projektien Web Suositut arkkitehtuuri vaihtoehtoa.

Azure koneen Learning on kahdentyyppisiä palveluita:

- Vastaus palvelun (RRS) – pieni viive, erittäin skaalattava palvelun, joka tarjoaa koneen Learning Studio käyttöliittymään tilattomien malleihin luotu ja otettu käyttöön.
- Erän suorittamisen Service (BES) – asynkroninen palvelu, joka tietueita erä tulosten.

Lisätietoja tietokoneen Learning verkkopalvelut artikkelissa [käyttöönotto koneen Learning verkkopalvelun](machine-learning-publish-a-machine-learning-web-service.md).

## <a name="get-an-azure-machine-learning-authorization-key"></a>Azure koneen Learning luvan Key-tunnuksen hankkiminen ##

Kun otat käyttöön oman koe, API avaimet on luotu WWW-palvelun. Voit hakea näppäimet useista sijainneista.

## <a name="from-the-microsoft-azure-machine-learning-web-services-portal"></a>Microsoft Azure koneen Learning Web Services-portaalista

Kirjaudu [Microsoft Azure koneen Learning Web Services](https://services.azureml.net) -portaaliin.

Voit hakea uuden tietokoneen Learning Web-palvelu Ohjelmointirajapinnan avain:

1. Valitse Azure koneen Learning Web Services-portaalissa **Verkkopalvelut** yläreunan valikkoa.
2. Valitse Web-palvelu, johon haluat noutaa.
3. Valitse yläreunan **Kulutettava**.
4. Kopioi ja tallenna sitten **Perusavain**.


Voit hakea Ohjelmointirajapinnan avain perinteinen koneen Learning Web-palvelu:

1. Valitse Azure koneen Learning Web Services-portaalissa **Perinteinen verkkopalvelut** yläreunan valikkoa.
2. Valitse Web-palvelu, joiden kanssa työskentelet.
3. Valitse päätepiste, jonka haluat noutaa.
3. Valitse yläreunan **Kulutettava**.
4. Kopioi ja tallenna sitten **Perusavain**.

## <a name="classic-web-service"></a>Perinteinen Web-palvelu ##

 Perinteinen WWW-palvelun avaimen hakemiseen voi myös tietokoneen Learning Studiossa tai Azure portaalin.

### <a name="machine-learning-studio"></a>Koneen Learning Studio ###

1. Valitse **VERKKOPALVELUT** koneen Learning Studiossa vasemmalla puolella.
2. Valitse Web-palvelu. **Raporttinäkymät-IKKUNAN** -välilehdessä on **Ohjelmointirajapinnan avain** .

### <a name="azure-portal"></a>Azure portal ###

1. Valitse **Tietokoneen LEARNING** vasemmalla puolella.
2. Valitse työtila, jossa WWW-palvelun sijaitsee.
3. Valitse **WEB-palvelut**.
4. Valitse Web-palvelu.
5. Valitse päätepisteen. "Ohjelmointirajapinnan avain" on alaspäin oikeassa alakulmassa.

## <a id="connect"></a>Yhteyden muodostaminen tietokoneen Learning Web-palvelu

Voit muodostaa yhteyden tietokoneen Learning verkkopalvelun avulla ohjelmoinnin HTTP-pyynnön ja vastauksen tukemille kielille. Voit tarkastella esimerkkejä C#, Python ja R verkkosivulta koneen Learning palvelun ohje.

**Koneen Learning API Ohje** Koneen Learning API Ohje luodaan, kun otat verkkopalvelun. Katso [Azure koneen Learning ongelmatilanteita - WWW-palvelun käyttöön](machine-learning-walkthrough-5-publish-web-service.md).
Koneen Learning API Ohje sisältää tekstinsyöttö verkkopalvelun tiedot.

2. Valitse Web-palvelu, joiden kanssa työskentelet.
3. Valitse päätepiste, jonka haluat tarkastella API Ohjesivun.
3. Valitse yläreunan **Kulutettava**.
3. Valitse kohdasta pyynnön vastauksen tai eräkäsittely päätepisteet **API ohjesivu** .

**Uusi Web-palvelu Ohje koneen Learning API-näkymään**

Valitse Azure Konepohjaisten Oppimistekniikoiden Web Services-portaaliin:

1. Valitse **VERKKOPALVELUT** yläreunan valikosta.
2. Valitse Web-palvelu, johon haluat noutaa.

Valitse Hanki URI-pyynnön Reposonse ja erä suorittamisen palveluiden ja -koodi C#, R ja Python **Kulutettava** .

Valitse **Swagger API** saat kutsua annettu URI API Swagger mukaan ohjeissa.

### <a name="c-sample"></a>C#-Esimerkki ###

Muodostaa yhteyden tietokoneeseen Learning verkkopalvelun Käytä **HttpClient** kulkeva ScoreData. ScoreData sisältää FeatureVector n kolmiulotteisen vector numeerisia ominaisuuksia, joka edustaa ScoreData. Voit todentaa koneen Learning service API-näppäimen kanssa.

Muodostaa yhteyden tietokoneeseen Learning verkkopalvelun **Microsoft.AspNet.WebApi.Client** NuGet paketin on oltava asennettuna.

**Asenna Visual Studiossa Microsoft.AspNet.WebApi.Client NuGet**

1. Lataa-tietojoukko: UCI julkaista: aikuisen 2 luokan tietojoukko Web-palveluun.
2. Valitse **Työkalut** > **NuGet pakettien hallinta** > **Paketin hallinta-konsolin**.
2. Valitse **Asenna-paketin Microsoft.AspNet.WebApi.Client**.

**Jos haluat suorittaa koodin otosten**

1. Julkaise "Esimerkki 1: lataa tietojoukko UCI: aikuisen 2 luokan tietojoukko" koe, tietokoneen Learning otoksen kokoelman osa.
2. Määrittää apiKey-näppäimen kanssa verkkopalvelun. Katso **Azure koneen Learning luvan Key-tunnuksen hankkiminen** yläpuolella.
3. Määrittää serviceUri pyytää URI.


### <a name="python-sample"></a>Python malli ###

Muodostaa yhteyden tietokoneeseen Learning verkkopalvelun käyttää kulkeva ScoreData **urllib2** -kirjastoa. ScoreData sisältää FeatureVector n kolmiulotteisen vector numeerisia ominaisuuksia, joka edustaa ScoreData. Voit todentaa koneen Learning service API-näppäimen kanssa.


**Jos haluat suorittaa koodin otosten**

1. Ottaa käyttöön "Esimerkki 1: lataa tietojoukko UCI: aikuisen 2 luokan tietojoukko" koe, osa tietokoneen Learning otoksen kokoelman.
2. Määrittää apiKey-näppäimen kanssa verkkopalvelun. On tämän artikkelin alun lähellä **Azure koneen Learning luvan Key-tunnuksen hankkiminen** -osassa.
3. Määrittää serviceUri pyytää URI.

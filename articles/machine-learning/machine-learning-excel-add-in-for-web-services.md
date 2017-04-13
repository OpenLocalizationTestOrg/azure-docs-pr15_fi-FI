<properties
    pageTitle="Excel-apuohjelmassa tietokoneen Learning verkkopalvelut | Microsoft Azure"
    description="Opi käyttämään Azure koneen Learning verkkopalvelut kirjoittamatta lainkaan koodia suoraan Excelissä."
    services="machine-learning"
    documentationCenter=""
    authors="tedway"
    manager="jhubbard"
    editor="cgronlun"
    tags=""/>

<tags
    ms.service="machine-learning"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="10/05/2016"
    ms.author="tedway;garye" />

# <a name="excel-add-in-for-azure-machine-learning-web-services"></a>Excel-apuohjelma Azure koneen Learning-verkkopalvelut

Excel on helppo Soita verkkopalvelut suoraan tarvitsematta kirjoittaa koodia.

## <a name="steps-to-use-an-existing-web-service-in-the-workbook"></a>Ohjeet, joiden avulla aiemmin verkkopalvelun työkirjassa

1. Avaa [malli Excel-tiedosto](http://aka.ms/amlexcel-sample-2), joka sisältää Excel-apuohjelma ja Titanic henkilöliikenteen tietoja.
2. Valitse WWW-palvelun napsauttamalla sitä-"Titanic jälkeenjääneellä Predictor (Excel-apuohjelma näyte) [tulos]" tässä esimerkissä.

    ![Valitse Web-palvelu][01]

3. Tällöin näyttöön **Predict** -osaan.  Tämä työkirja sisältää jo mallitiedot, mutta tyhjän työkirjan voit valita solun Excelissä ja valitse **Käytä mallitiedot**.
4. Valitse ylätunnisteiden tiedot ja syöttötiedot alue-kuvaketta.  Varmista, että "tiedoissa on otsikoita"-valintaruutu on valittuna.
5. Valitse **tulostus**Kirjoita solujen lukumäärän, jossa haluat näkyvän, esimerkiksi "H1" tässä tulos.
6. Valitse **ennustaa**.

    ![Ennustaa osa][02]

## <a name="steps-to-add-a-new-web-service"></a>Uusi Web-palvelu lisäämisen vaiheet

WWW-palvelun käyttöön tai aiemmin luodun WWW-palvelun avulla. Katso lisätietoja käyttöönotto verkkopalvelun [ongelmatilanteita vaihe 5: Azure koneen Learning WWW-palvelun käyttöön](machine-learning-walkthrough-5-publish-web-service.md).

Saat WWW-palvelun API-näppäintä. Jos teet tämän, riippuu siitä, onko uuden tietokoneen Learning-WWW-palvelun perinteinen koneen Learning Web-palvelu on julkaistu.

**Perinteinen WWW-palvelun avulla** 

1. Koneen Learning Studiossa **VERKKOPALVELUT** osan vasemmanpuoleisessa ruudussa ja valitse sitten WWW-palvelun.

    ![Valitse Studio Web-palvelu][04]

2. Kopioi WWW-palvelun API-näppäintä.

    ![Studio Ohjelmointirajapinnan avain][05]

3. **Raporttinäkymät-IKKUNAN** WWW-palvelun-välilehdessä **PYYNNÖN ja VASTAUKSEN** -linkkiä.
4. Etsi **Pyytää URI** -osassa.  Kopioida ja tallentaa URL-Osoitteen.

>[AZURE.NOTE] On nyt mahdollista kirjautua [Azure koneen Learning Web Services](https://services.azureml.net) -portaaliin hankkiminen perinteinen koneen Learning Web-palvelu Ohjelmointirajapinnan avain.

**Käytä uusi Web-palvelu**

1. [Azure koneen Learning Web Services](https://services.azureml.net) -portaalissa **Verkkopalveluita**ja valitse WWW-palvelun. 
2. Valitse **tarjoaman**.
3. Etsi **Basic kulutus info** -osa. Kopioida ja tallentaa **Perusavaimen** ja **Vastaus** URL-osoite.


## <a name="steps-to-add-a-new-web-service"></a>Uusi Web-palvelu lisäämisen vaiheet

1. WWW-palvelun käyttöön tai aiemmin luodun WWW-palvelun avulla. Katso lisätietoja käyttöönotto verkkopalvelun [ongelmatilanteita vaihe 5: Azure koneen Learning WWW-palvelun käyttöön](machine-learning-walkthrough-5-publish-web-service.md).
2. Valitse **tarjoaman**.
3. Etsi **Basic kulutus info** -osa. Kopioida ja tallentaa **Perusavaimen** ja **Vastaus** URL-osoite.
2. Excelissä, siirry kohtaan **Verkkopalvelut** (Jos olet **Predict** -osan, valitse Siirry Web palveluluetteloon mustaa nuolta).

    ![Siirry WWW-palvelun valinta][03]
    
3. Valitse **Lisää WWW-palvelun**.
4. Liitä URL-osoite **URL-osoite**, jossa Excel-apuohjelmassa teksti-ruutuun.
5. Liitä API-/ perusavain **Ohjelmointirajapinnan avain**-tekstiruutuun.
6. Valitse **Lisää**.

    ![Perinteinen verkkopalvelun URL-osoite ja Ohjelmointirajapinnan avain.][06]

10. WWW-palvelun käyttöä varten noudattamalla edellä ohjeita, "Ohjeet, joiden avulla aiemmin luotuun-verkkopalvelun."

## <a name="sharing-your-workbook"></a>Työkirjan jakaminen

Jos tallennat työkirjan, olet lisännyt verkkopalvelut API-/ perusavain on myös tallennettu. Tämä tarkoittaa olisi vain jakaa työkirjan luotat henkilöiden kanssa.

Pyydä kysyttävää kommentti-osassa tai tutustu [keskustelupalstalle](http://go.microsoft.com/fwlink/?LinkID=403669&clcid=0x409).

[01]: ./media/machine-learning-excel-add-in-for-web-services/image1.png
[02]: ./media/machine-learning-excel-add-in-for-web-services/image2.png
[03]: ./media/machine-learning-excel-add-in-for-web-services/image3.png
[04]: ./media/machine-learning-excel-add-in-for-web-services/image4.png
[05]: ./media/machine-learning-excel-add-in-for-web-services/image5.png
[06]: ./media/machine-learning-excel-add-in-for-web-services/image6.png

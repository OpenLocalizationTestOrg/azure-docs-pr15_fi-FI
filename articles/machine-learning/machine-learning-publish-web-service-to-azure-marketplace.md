<properties 
    pageTitle="Julkaise koneen learning WWW-palvelun Azure Marketplacesta | Microsoft Azure" 
    description="Julkaiseminen Azure koneen Learning Web-palvelu Azure Marketplacesta" 
    services="machine-learning" 
    documentationCenter="" 
    authors="BharathS" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/08/2016" 
    ms.author="bharaths"/>

# <a name="publish-azure-machine-learning-web-service-to-the-azure-marketplace"></a>Azure koneen Learning WWW-palvelun julkaiseminen Azure Marketplacesta 

Azure Marketplacesta on julkaista Azure koneen Learning verkkopalvelut maksetuksi tai vapaa kulutus ulkoiset asiakkaat palvelut. Tässä artikkelissa on linkkejä ohjeet, joiden avulla pääset alkuun, prosessin. Avulla varmistamisen, voit määrittää WWW-palvelut käytettäväksi muiden kehittäjille tarjoaman sovelluksessaan.


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="overview-of-the-publishing-process"></a>Julkaisuprosessin yleiskatsaus 

Azure koneen Learning WWW-palvelun julkaiseminen Azure Marketplacesta vaiheet ovat seuraavat:

1. Luoda ja julkaista koneen Learning – vastaus palvelu (RRS)
2. Ota käyttöön palvelun tuotannon ja hanki Ohjelmointirajapinnan avain ja OData tiedot.
3. Julkaistun verkkopalvelun URL-Osoitteen avulla voit julkaista [Azure](https://publish.windowsazure.com/workspace/) Marketplace (Data Market) 
4. Kun lähetetty, tarjous käydään läpi ja täytyy hyväksyä ennen asiakkaille, voit aloittaa ostamalla. Julkaisuprosessin saattaa kestää muutaman työpäivän. 

## <a name="walk-through"></a>Käy läpi
###<a name="step-1-create-and-publish-a-machine-learning-request-response-service-rrs"></a>Vaihe 1: Luominen ja julkaiseminen tietokoneen Learning – vastaus palvelu (RRS)###
 Jos et ole vielä tämän, tutustu tämän [käydä läpi](machine-learning-walkthrough-5-publish-web-service.md)katsaus.

###<a name="step-2-deploy-the-service-to-production-and-obtain-the-api-key-and-odata-endpoint-information"></a>Vaihe 2: Palvelun käyttöön tuotannon ja hankkia Ohjelmointirajapinnan avain ja OData tiedot###
1. [Azure perinteinen Portal](http://manage.windowsazure.com) **Koneen LEARNING** -vaihtoehdon valitseminen vasemman reunan siirtymispalkissa ja valitse työtilassa. 

2. **VERKKOPALVELUT** -välilehdessä ja valitse haluat julkaista marketplace web-palveluun.

    ![Azure Marketplacesta][workspace]

3. Valitse haluat on tarjoaman marketplace päätepiste. Jos et ole luonut muita päätepisteet, voit valita **oletusarvoinen** päätepiste.

4. Kun olet napsauttanut päätepisteen, voi tarkastella **Ohjelmointirajapinnan avain**. Tämä tieto on tietoja vaiheesta 3 myöhemmin, joten varmista sen.

    ![Azure Marketplacesta][apikey]

5. Valitse **PYYNNÖN ja VASTAUKSEN** -menetelmä ei ole suositeltavaa julkaisun erä suorittamisen palvelut Marketplacesta vaiheessa. Tämä siirtää sinut pyynnön ja vastauksen menetelmän API Ohje-sivulle.

6. Kopioi **OData päätepisteosoite**, sinun on nämä tiedot myöhemmin vaiheessa 3.

    ![Azure Marketplacesta][odata]




Ota käyttöön palvelun kyselyjä tuotannon.



###<a name="step-3-use-the-url-of-the-published-web-service-to-publish-to-azure-marketplace-datamarket"></a>Vaihe 3: Julkaista Azure Marketplace (DataMarket) julkaistun verkkopalvelun URL-Osoitteen avulla###

1.  Siirry [Azure Marketplace (Data Market)](http://datamarket.azure.com/home) 
2.  Napsauta sivun yläreunassa **Julkaise** -linkkiä. Tämä siirtää sinut [Microsoft Azure Julkaisemisportaali](https://publish.windowsazure.com)
3.  Valitse **lähteet** -osan julkaisijan Rekisteröi.
4.  Luodessasi uusi tarjous, valitse **Tietopalvelut**ja valitse sitten **Luo uusi tietopalvelu**. 
 
    ![Azure Marketplacesta][image1]

    <br />


5.  Valitse **suunnitelmien** Anna tiedot oman tarjoaa, mukaan lukien hinnoittelu suunnitelma. Päätä, jos tarjoat maksuttomia tai maksullisia palvelu. Hae maksettava, Anna maksutiedot, kuten pankki- ja tiedot.

6.  Valitse **markkinoinnin** on tarjous, kuten otsikko ja kuvaus tarjous tietoja.

7.  Valitse **hinnoittelu** hinnan asettaminen tietyn maiden suunnitelmien tai antaa järjestelmän "autoprice" tarjous.

8. Valitse **Palvelun tietojen** -välilehdessä **Verkkopalvelun** **Tietolähteeseen**.

    ![Azure Marketplacesta][image2]

9.  Pyydä verkkopalvelun URL-osoite ja API avaimeen Azure perinteinen-portaaliin, artikkelissa vaihe 2 edellä kuvatulla tavalla.

10. Liitä OData päätepisteosoite Marketplace-tietojen palvelun asetukset-valintaikkunan **Palvelun URL** -tekstiruutuun.

11. Valitse **todennus**- **ylä** **Todennus värimallin**.

    - Kirjoita **ylätunnisteen nimi**"lupa".
    - **Otsikon arvo**Kirjoita "Haltijan" (ilman lainausmerkkejä), valitse **VÄLINÄPPÄINTÄ** ja liitä Ohjelmointirajapinnan avain.
    - Valitse **Tämä palvelu on OData** -valintaruutu.
    - Valitse **Testaa yhteys** Testaa yhteys.

12. Valitse **Luokat**-Varmista **Tietokoneen Learning** on valittuna.

13. Kun olet tehnyt kirjoittaminen tarjous, metatietoa valitsemalla **Julkaise**ja **siirtää vaiheet**. Tässä vaiheessa voit ilmoitetaan muut ongelmat, jotka haluat korjata.

14. Kun olet tarkistanut kaikki avoimet seurantakohteet, valmiiksi, valitse **pyytää hyväksyntää, jos haluat siirtää tuotannon**. Julkaisuprosessin saattaa kestää muutaman työpäivän. 


[image1]:./media/machine-learning-publish-web-service-to-azure-marketplace/image1.png
[image2]:./media/machine-learning-publish-web-service-to-azure-marketplace/image2.png
[workspace]:./media/machine-learning-publish-web-service-to-azure-marketplace/selectworkspace.png
[apikey]:./media/machine-learning-publish-web-service-to-azure-marketplace/apikey.png
[odata]:./media/machine-learning-publish-web-service-to-azure-marketplace/odata.png
 

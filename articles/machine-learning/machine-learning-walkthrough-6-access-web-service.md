<properties
    pageTitle="Vaihe 6: Käyttää tietokoneen Learning WWW-palvelun | Microsoft Azure"
    description="Kehittäminen ennakoivan ratkaisu vaiheittainen vaihe 6: aktiivinen Azure koneen Learning-verkkopalvelun käyttää."
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


# <a name="walkthrough-step-6-access-the-azure-machine-learning-web-service"></a>Vaiheittainen kuvaus vaihe 6: Käyttää Azure koneen Learning web-palvelu

Tämä on viimeinen vaihe vaiheittaista [kehittäminen ennakoivan analytics-ratkaisun Azure koneen oppiminen](machine-learning-walkthrough-develop-predictive-solution.md)


1.  [Koneen Learning työtilan luominen](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  [Ladata olemassa olevia tietoja](machine-learning-walkthrough-2-upload-data.md)
3.  [Luo uusi koe](machine-learning-walkthrough-3-create-new-experiment.md)
4.  [Kouluttaminen ja arvioi mallit](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.  [WWW-palvelun käyttöön](machine-learning-walkthrough-5-publish-web-service.md)
6.  **Access web-palvelu**

----------

Tätä vaiheittaista edellisessä vaiheessa on otettu käyttöön verkkopalveluun, joka käyttää Microsoftin luottotietojen riskin tekstinsyöttö malli. Nyt käyttäjien tarvitsee tietojen lähettäminen ja vastaanottaminen tulokset. 

Web-palvelu on Azure verkkopalveluun, joka voi vastaanottaa ja palauttaa tiedot REST API käyttäminen toisella seuraavista tavoista:  

-   **Pyynnön ja vastauksen** - käyttäjä lähettää rivejä luottokortin tiedot käyttämällä HTTP-protokolla-palveluun ja palvelun vastaa vähintään yksi määrittää tulosten.
-   **Eräkäsittely** - käyttäjän rivejä luottokortin tiedot tallennetaan Azuren Blob-objektien ja lähettää sitten palveluun blob-sijainti. Palvelun pisteyttää usealla tietorivillä syötteen Blob-objektien, tallentaa tulokset toiseen Blob-objektien ja palauttaa kyseisen säilön URL-osoite.  

Nopein ja helpoin tapa käyttää web-palvelu on käytettävissä [Azure Web App Marketplace](https://azure.microsoft.com/marketplace/web-applications/all/)Web App-mallien avulla.
Nämä web app-malleissa voit luoda mukautetun verkkosovelluksen tietokenttiä tietää web-palvelu kenttään annettavat tiedot, ja se voi palauttaa. Kaikki sinun on suoritettava on pääsy web-palvelu ja tiedot ja malli hoitaa loput.

Lisätietoja web app-mallien käyttämisestä on artikkelissa [Kulutettava Azure koneen Learning WWW-palvelun web app-malli](machine-learning-consume-web-service-with-web-app-template.md).

Voit luoda mukautetun sovelluksen käyttämään käyttämällä starter R, C# ja Python ohjelmoinnin kielet tarkoitettu web-palveluun.
Löydät kaikki tiedot [siitä, miten tarjoaman Azure koneen Learning-verkkopalvelun, jotka on julkaistu tietokoneesta, kannattaa tutustua kokeen](machine-learning-consume-web-services.md).


<properties
    pageTitle="Suositusten saaminen erissä: koneen Koulujen suosituksia Ohjelmointirajapinta | Microsoft Azure"
    description="Azure konepohjaisten oppimistekniikoiden suosituksia--käytön suosituksia erissä"
    services="cognitive-services"
    documentationCenter=""
    authors="luiscabrer"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/17/2016"
    ms.author="luisca"/>

# <a name="get-recommendations-in-batches"></a>Nouda suositukset erissä

>[AZURE.NOTE] Suositusten saaminen erissä on monimutkaisempi kuin käytön suosituksia yksi kerrallaan. Tarkista ohjelmointirajapinnan hankkiminen yksittäisen pyynnön suosituksia tietoja:

> [Kohteesta suositukset](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3d4)<br>
> [Käyttäjän kohteen suositukset](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3dd)
>
> Erän näkyvissä pistemäärä toimii vain versiot, jotka on luotu 2016 21 heinäkuussa.


Tilanteita, joissa sinun on hankittava suosituksia useamman kuin yhden kohteen kerrallaan. Esiintymän haluat ehkä luodaan suositukset-välimuisti tai analysoiminen jopa suosituksia, jotka olet uudistamassa tyypit.

Erän tulosmalli toimintoja, kuten olemme Soita ne ovat asynkronisten toimintojen. Haluat lähettää pyynnön ja odota, että toiminto on valmis kerätä tulokset.  

Enemmän tarkka, seuraavassa on kuvattu toimet, joiden:

1.  Luo Azure-tallennustilan säilö, jos sitä ei vielä ole.
2.  Lataa syötteen tiedosto, joka on kuvattu Azure-Blob-säiliö suositus pyyntöjen.
3.  Tehoa aivoriihipalavereihin tulosmalli erän työn.
4.  Odota asynkroninen toiminto on valmis.
5.  Kun toiminto on valmis, voit kerätä tulokset Blob-objektien tallennustilaan.

Oletetaan, että käy läpi kaikki nämä vaiheet.

## <a name="create-a-storage-container-if-you-dont-have-one-already"></a>Luo tallennustilan säilö, jos sitä ei vielä ole

Siirry [Azure portal](https://portal.azure.com) ja luo uusi tallennustilan-tili, jos sitä ei vielä ole. Voit tehdä tämän Siirry **Uusi** > **tietojen** + **tallennustilan** > **Tallennustilan tilin**.

Sen jälkeen, kun sinulla on tallennustilan-tili, sinun täytyy luoda blob säilöjen, johon tallennat syötteen ja tulosteen eräkäsittely.

Lataa syötteen tiedosto, joka kuvaa kunkin suositukset pyytää Blob-säiliö japanin Soita tiedoston input.json tähän.
Kun olet säilön, sinun on ladattava tiedosto, joka on kuvattu pyynnöt, sinun on suoritettava suositukset-palvelusta.

Erän voit suorittaa vain yhden tyyppisiä pyyntö tietyn muodosta. Kerromme määrittäminen näitä tietoja seuraavassa osassa. Nyt oletetaan, että olemme suorittaa kohteen suosituksia tietyn muodosta ulos. Lähdetiedoston sisältää kunkin pyynnöt syötetietoja (eli tässä tapauksessa lähde-kohteet).

Tämä on esimerkki input.json-tiedosto näyttää seuraavalta:

    {
      "requests": [
      { "SeedItems": [ "C9F-00163", "FKF-00689" ] },
      { "SeedItems": [ "F34-03453" ] },
      { "SeedItems": [ "D16-3244" ] },
      { "SeedItems": [ "C9F-00163", "FKF-00689" ] },
      { "SeedItems": [ "F43-01467" ] },
      { "SeedItems": [ "BD5-06013" ] },
      { "SeedItems": [ "P45-00163", "FKF-00689" ] },
      { "SeedItems": [ "C9A-69320" ] }
      ]
    }

Kuten näet, tiedosto on JSON-tiedosto, jossa kukin pyynnöt on tiedot, joita tarvitaan suositukset-Lähetyspyyntö. Luo samanlainen JSON pyynnöt, jotka haluat täyttää varten ja kopioi se Blob-objektien tallennustilaan juuri luomasi säilöön.

## <a name="kick-start-the-batch-job"></a>Tehoa erän työn aivoriihipalavereihin

Seuraava vaihe on lähettää uuden Siirtoerän työn. Jos haluat lisätietoja, tarkista [API-viittaus](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/).

Pyyntö leipätekstiin Ohjelmointirajapinnan on Määritä sijainnit, joissa syöte, tulostus ja virhetiedostojen pitäisi tallennetaan. Se on myös Määritä tunnistetiedot, joita voi käyttää sijaintien. Lisäksi on määritettävä joitakin parametreja, jotka koskevat koko erän (tyyppi, suositukset, voit pyytää-malli ja luo käyttäminen, puhelun kohti tulosten määrä ja niin edelleen.)

Seuraavassa on esimerkki pyynnön tekstiin pitäisi näyttää esimerkiksi tältä:

    {
      "input": {
        "authenticationType": "PublicOrSas",
        "baseLocation": "https://mystorage1.blob.core.windows.net/",
        "relativeLocation": "container1/batchInput.json",
        "sasBlobToken": "?sv=2015-07_restofToken_…4Z&sp=rw"
      },
      "output": {
        "authenticationType": "PublicOrSas",
        "baseLocation": "https://mystorage1.blob.core.windows.net/",
        "relativeLocation": "container1/batchOutput.json ",
        "sasBlobToken": "?sv=2015-07_restofToken_…4Z&sp=rw"
      },
      "error": {
        "authenticationType": "PublicOrSas",
        "baseLocation": "https://mystorage1.blob.core.windows.net/",
        "relativeLocation": "container1/errors.txt",
        "sasBlobToken": "?sv=2015-07_restofToken_…4Z&sp=rw"
      },
      "job": {
        "apiName": "ItemRecommend",
        "modelId": "9ac12a0a-1add-4bdc-bf42-c6517942b3a6",
        "buildId": 1015703,
        "numberOfResults": 10,
        "includeMetadata": true,
        "minimalScore": 0.0
      }
    }

Tässä muutama huomioon Huomaa:

-   Tällä hetkellä **authenticationType** on määritettävä aina **PublicOrSas**.

-   Sinun on hankittava jaettu Access allekirjoitus (SAS)-tunnuksen, jotta lukemiseen ja kirjoittamiseen /, Blob storage tilisi suositukset-Ohjelmointirajapinnan. Lisätietoja siitä, miten luodaan Suojaussidokset tunnusten löytyy [suosituksia API](../storage/storage-dotnet-shared-access-signature-part-1.md)-sivulla.

-   Vain **apiName** , joka on tällä hetkellä tueta on **ItemRecommend**, jota käytetään kohteen kohteen suosituksia. Jonottaminen ei tällä hetkellä tue käyttäjän kohteen suosituksia.

## <a name="wait-for-the-asynchronous-operation-to-finish"></a>Odota asynkronisen toiminnon loppuun

Kun käynnistät erä-toiminnolla, vastaus palauttaa toiminnon sijainti-otsikon, joka antaa tiedot, joita tarvitaan seurata toiminnon.
Voit seurata toimintoa käyttämällä [Noutaa toiminnon tilan API]( https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3da), aivan kuten mitä muodosta toiminnon toiminnan seuraamiseen.

## <a name="get-the-results"></a>Saat tulokset

Kun toiminto on valmis, oletetaan, että virheitä ei, voit kerätä tulokset jaettavaan-Blob-tallennustilan.

Alla olevassa esimerkissä näyttää, miltä tulos voi näyttää. Tässä esimerkissä on Näytä tulokset erän vain kaksi tarjouspyyntöjen (niitä) kanssa.

    {
      "results":
      [   
        {
          "request": { "seedItems": [ "DAF-00500", "P3T-00003" ] },
          "recommendations": [
            {
              "items": [
                {
                  "itemId": "F5U-00011",
                  "name": "L2 Ethernet Adapter-Win8Pro SC EN/XD/XX Hdwr",
                  "metadata": ""
                }
              ],
              "rating": 0.722,
              "reasoning": [ "People who like the selected items also like 'L2 Ethernet Adapter-Win8Pro SC EN/XD/XX Hdwr'" ]
            },
            {
              "items": [
                {
                  "itemId": "G5Y-00001",
                  "name": "Docking Station for Srf Pro/Pro2 SC EN/XD/ES Hdwr",
                  "metadata": ""
                }
              ],
              "rating": 0.718,
              "reasoning": [ "People who like the selected items also like 'Docking Station for Srf Pro/Pro2 SC EN/XD/ES Hdwr'" ]
            }
          ]
        },
        {
          "request": { "seedItems": [ "C9F-00163" ] },
          "recommendations": [
            {
              "items": [
                {
                  "itemId": "C9F-00172",
                  "name": "Nokia 2K Shell for Nokia Lumia 630/635 - Green",
                  "metadata": ""
                }
              ],
              "rating": 0.649,
              "reasoning": [ "People who like 'MOZO Flip Cover for Nokia Lumia 635 - White' also like 'Nokia 2K Shell for Nokia Lumia 630/635 - Green'" ]
            },
            {
              "items": [
                {
                  "itemId": "C9F-00171",
                  "name": "Nokia 2K Shell for Nokia Lumia 630/635 - Orange",
                  "metadata": ""
                }
              ],
              "rating": 0.647,
              "reasoning": [ "People who like 'MOZO Flip Cover for Nokia Lumia 635 - White' also like 'Nokia 2K Shell for Nokia Lumia 630/635 - Orange'" ]
            },
            {
              "items": [
                {
                  "itemId": "C9F-00170",
                  "name": "Nokia 2K Shell for Nokia Lumia 630/635 - Yellow",
                  "metadata": ""
                }
              ],
              "rating": 0.646,
              "reasoning": [ "People who like 'MOZO Flip Cover for Nokia Lumia 635 - White' also like 'Nokia 2K Shell for Nokia Lumia 630/635 - Yellow'" ]
            }       
          ]
        }
    ]}


## <a name="learn-about-the-limitations"></a>Lisätietoja rajoituksista

-   Vain yhtä erää työn voidaan kutsua tilauskohtaisten kerrallaan.
-   Työn syötteen komentojonotiedosto eivät saa olla yli 2 Megatavua.

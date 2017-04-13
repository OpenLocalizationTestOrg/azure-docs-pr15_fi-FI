
<properties
    pageTitle="Siirtää Azure kognitiiviset Services suositukset API DataMarket suositukset API | Microsoft Azure"
    description="Azure konepohjaisten oppimistekniikoiden suosituksia--suosituksia kognitiiviset palvelun siirtyminen"
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
    ms.date="09/01/2016"
    ms.author="luisca"/>


# <a name="migrate-to-azure-cognitive-services-recommendations-api-from-the-datamarket-recommendations-api"></a>Siirtää Azure kognitiiviset Services suositukset API DataMarket suositukset Ohjelmointirajapinta
Tämän artikkelin avulla voit siirtää [Microsoft DataMarket suosituksia API](https://datamarket.azure.com/dataset/amla/recommendations) [Microsoft Azure kognitiiviset Services suosituksia API](https://www.microsoft.com/cognitive-services/en-us/recommendations-api).

DataMarket suositukset-Ohjelmointirajapinnan ne eivät enää hyväksymällä asiakkaille 31 joulukuussa 2016- ja poistettu 2017 28 helmikuun.

## <a name="how-do-i-start-using-the-azure-cognitive-services-recommendations-api"></a>Miten Azure kognitiiviset verkkopalvelun suositukset-Ohjelmointirajapinnan käyttö aloitetaan?

Siirtää kognitiiviset Services suositukset-Ohjelmointirajapinta, toimi seuraavasti:

1.  Jos sinulla ei vielä ole Azure tilauksen, yhden [tilaa](https://portal.azure.com/#create/Microsoft.CognitiveServices/apitype/Recommendations/pricingtier/S1) . 

1.  Vaiheittaiset ohjeet kognitiiviset Services suositukset-Ohjelmointirajapinnan rekisteröityminen ja käyttää sitä ohjelmallisesti [Pikaopas](cognitive-services-recommendations-quick-start.md) hakeminen 

1.  Siirry [kognitiiviset Services suosituksia API siirtymissivu](https://www.microsoft.com/cognitive-services/en-us/recommendations-api) , Lue lisää palvelu ja ohjeissa.

## <a name="i-used-the-recommendations-ui-to-build-my-models-is-there-a-similar-tool-for-the-cognitive-services-recommendations-api"></a>Voin käyttää suositukset-Käyttöliittymän voit luoda Omat mallit. Onko kognitiiviset Services suositukset-Ohjelmointirajapinnan samanlaisen työkalun?

Täysin! [Suosituksia Käyttöliittymän](http://recommendations-portal.azurewebsites.net/) URL-osoite on http://recommendations-portal.azurewebsites.net. 

>[AZURE.NOTE] DataMarket tunnistetiedot eivät toimi tässä. Azure-portaalissa tilauksen rekisteröityminen, sekä siirtyä [Suosituksia Käyttöliittymän](http://recommendations-portal.azurewebsites.net/)käyttämiseen tarvittavat tili-näppäintä.
Jos sinulla ei ole tiliä-näppäintä, katso tehtävän 1 [Pikaopas](cognitive-services-recommendations-quick-start.md).

##<a name="is-the-new-api-format-the-same-as-the-datamarket-recommendations-api"></a>On uusi API-muoto sama kuin DataMarket suositukset-Ohjelmointirajapinnan?

Ohjelmointirajapinnan tukee saman skenaariot ja Prosessi kulkee kuin DataMarket tue tilanteisiin, mutta todellinen API-liittymän on päivitetty [Microsoft REST API ohjeiden](https://github.com/Microsoft/api-guidelines/blob/master/Guidelines.md)mukainen. API ovat yhdenmukaisempi ja työ paremmin kanssa Työkalut tukevat Swagger.

Tutustu ymmärtää kunkin API [API explorer](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3db).
Käytä *Kokeile* Testaa API-kutsu painike. Tiedostot, jotka suositukset-Ohjelmointirajapinnan (luettelo ja käyttö-tiedostot) muoto ei ole muuttunut, jotta voit jatkaa samaa tiedostoja ja/tai infrastruktuurin suunnittelemasi luomiseen kyseiset tiedostot.

##<a name="what-are-some-new-features-in-the-cognitive-services-recommendations-api"></a>Mitä uusia ominaisuuksia-kognitiiviset Services suositukset-Ohjelmointirajapinnan?

Kahden edellisen kuukauden ajalta Microsoft on julkaissut kognitiiviset Services suositukset-Ohjelmointirajapinnan uusia ominaisuuksia:
-   Suosituksia Käyttöliittymän koulutus ja testausta mallien tarvitsematta kirjoittaa koodin Yksi tekstirivi
-   Erän näkyvissä samalla kertaa antaa sinun suosituksia tuhansia pistemäärä
-   Arvot tuki kyselyyn laatua, suositukset mallien rakentaminen
-   Liiketoiminnan sääntöjen tuki
-   Luetteloi ja ladata tiedostoja käyttö- ja luettelo
-   Luokitus muodosta tuki kyselyn laatua, kohteen ominaisuuksien suositukset-mallissa
-   Lisätty mahdollisuus tuotteen luettelon hakeminen

## <a name="when-does-microsoft-stop-supporting-the-datamarket-recommendations-api"></a>Kun Microsoft lopettaa DataMarket suositukset-Ohjelmointirajapinnan tukevat?

[Suosituksia API-DataMarket](https://datamarket.azure.com/dataset/amla/recommendations) lopettaa asiakkaille hyväksymisen jälkeen 31 joulukuussa 2016- ja 2017 28 helmikuun on kokonaan poistettu. 

## <a name="what-if-i-dont-have-the-data-that-i-need-to-recreate-my-models-in-the-cognitive-services-recommendations-api"></a>Entä jos minulla ei ole tietoja, jotka haluat luoda kognitiiviset Services suositukset-Ohjelmointirajapinnan Omat mallit?

Haluat tehdä tämän siirtymisen mahdollisimman helppoa puolestasi. Emme voi auttaa vanha mallien siirtäminen DataMarket-tilisi tilauksen Azure kognitiiviset Services suosituksia API. Ota meihin yhteyttä-palvelussa [mlapi@microsoft.com](mailto://mlapi@microsoft.com). Pyydämme sinua tietosuojasyistä antamaan oman DataMarket Tilaustunnus ja kognitiiviset palvelut-tilauksen tunnus ennen mallien liittäminen on uusi tili.

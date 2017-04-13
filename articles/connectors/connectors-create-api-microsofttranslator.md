<properties
    pageTitle="Microsoft Translator lisääminen logiikan sovellusten | Microsoft Azure"
    description="Yleistä REST API parametreilla Microsoft Translator-yhdistin"
    services=""
    suite=""
    documentationCenter="" 
    authors="MandiOhlinger"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="multiple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="08/18/2016"
   ms.author="mandia"/>

# <a name="get-started-with-the-microsoft-translator-connector"></a>Microsoft Translator-yhdistin käytön aloittaminen
Muodosta yhteys Microsoft Translator tekstin kääntäminen, tunnista kieli ja paljon muuta. Microsoft Translator voit tehdä seuraavia toimia: 

- Muodostaa yhteyttä business työnkulku, sinulla on Microsoft Translator tietojen perusteella. 
- Toimintojen avulla voit kääntää tekstiä, tunnista kieli ja paljon muuta. Nämä toiminnot vastauksen saaminen ja tee tulosteen käytettävissä, jos haluat käyttää muuta vaihtoehtoa. Kun uusi tiedosto luodaan dropbox, voit kääntää tekstiä toisella kielellä käyttämällä Microsoft Translator-tiedoston.

Jos haluat lisätä toiminnon logiikan sovelluksissa, katso [logiikan-sovelluksen luominen](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Käynnistimet ja toiminnot
Microsoft Translator sisältää seuraavat toimet. Ei ole Käynnistimet.

Käynnistimien | Toiminnot
--- | ---
Ei mitään | <ul><li>Kielen tunnistaminen</li><li>Teksti puheeksi</li><li>Tekstin kääntäminen</li><li>Hae kielet</li><li>Hae puhe kielet</li></ul>

Yhdistimien tukevat tietojen JSON ja XML-muodossa.


## <a name="create-a-connection-to-microsoft-translator"></a>Yhteyden muodostaminen Microsoft Translator

>[AZURE.INCLUDE [Steps to create a connection to Microsoft Translator](../../includes/connectors-create-api-microsofttranslator.md)]


## <a name="swagger-rest-api-reference"></a>Swagger REST API-viittaus
Koskee versiota: 1.0.

### <a name="detect-language"></a>Kielen tunnistaminen    
Tunnistaa Lähdekieli, valita tekstiä.  
```GET: /Detect```

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|kyselyn|merkkijono|Kyllä|kyselyn|ei mitään |Teksti, jonka kieli tunnistetaan|

#### <a name="response"></a>Vastaus
|Nimi|Kuvaus|
|---|---|
|200|Okei|
|Oletusarvo|Epäonnistui.|


### <a name="text-to-speech"></a>Teksti puheeksi    
Muuntaa annetun tekstin puheeksi kuin äänivirtaa Aalto-muodossa.  
```GET: /Speak```

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|kyselyn|merkkijono|Kyllä|kyselyn|ei mitään |Muunnettava teksti|
|kieli|merkkijono|Kyllä|kyselyn|ei mitään |Kielikoodi Luo puheeksi (Esimerkki: "fi-yhteyttä)|

#### <a name="response"></a>Vastaus
|Nimi|Kuvaus|
|---|---|
|200|Okei|
|Oletusarvo|Epäonnistui.|


### <a name="translate-text"></a>Tekstin kääntäminen    
Kääntää tekstiä käyttämällä Microsoft Translator määritetty kieli.  
```GET: /Translate```

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|kyselyn|merkkijono|Kyllä|kyselyn|ei mitään |Tekstin kääntäminen|
|languageTo|merkkijono|Kyllä|kyselyn| ei mitään|Kohdekieli koodin (esimerkiksi: "f")|
|languageFrom|merkkijono|Ei|kyselyn|ei mitään |Tietolähteen kieli. Jos ei anneta, Microsoft Translator yrittää automaattinen tunnistus. (Esimerkki: fi)|
|luokka|merkkijono|Ei|kyselyn|Yleiset |Käännös-luokka (oletus: Yleiset")|

#### <a name="response"></a>Vastaus
|Nimi|Kuvaus|
|---|---|
|200|Okei|
|Oletusarvo|Epäonnistui.|


### <a name="get-languages"></a>Hae kielet    
Hakee kaikki kielet, joka tukee Microsoft Translator.  
```GET: /TranslatableLanguages```

Ei ole parametreja puhelun. 

#### <a name="response"></a>Vastaus
|Nimi|Kuvaus|
|---|---|
|200|Okei|
|Oletusarvo|Epäonnistui.|


### <a name="get-speech-languages"></a>Hae puhe kielet    
Hakee puhe synthesis käytettävissä olevat kielet.  
```GET: /SpeakLanguages``` 

Ei ole parametreja puhelun.

#### <a name="response"></a>Vastaus
|Nimi|Kuvaus|
|---|---|
|200|Okei|
|Oletusarvo|Epäonnistui.|

## <a name="object-definitions"></a>Objektimääritykset

#### <a name="language-language-model-for-microsoft-translator-translatable-languages"></a>Kieli: Microsoft Translator käännettävissä kielten kieli-malli

|Ominaisuuden nimi | Tietotyyppi | Pakollinen|
|---|---|---|
|Koodi|merkkijono|Ei|
|Nimi|merkkijono|Ei|


## <a name="next-steps"></a>Seuraavat vaiheet

[Logiikan-sovelluksen luominen](../app-service-logic/app-service-logic-create-a-logic-app.md).

Siirry [API-luettelosta](apis-list.md).


<!--References-->
[5]: https://datamarket.azure.com/developer/applications/
[6]: ./media/connectors-create-api-microsofttranslator/register-your-application.png

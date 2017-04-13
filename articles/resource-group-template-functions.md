<properties
   pageTitle="Resurssien hallinnan malli toiminnot | Microsoft Azure"
   description="Tässä artikkelissa kuvataan Funktiot hakea arvoja, merkkijonot ja ovat numeeriset arvot ja hakea asennustiedot Azure Resurssienhallinta-mallin avulla."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/12/2016"
   ms.author="tomfitz"/>

# <a name="azure-resource-manager-template-functions"></a>Azure Resurssienhallinta malli-Funktiot

Tässä ohjeaiheessa kerrotaan kaikki toiminnot, voit käyttää Azure Resurssienhallinta-malli.

Mallin funktioista ja niiden parametrit on asennustavan. Esimerkiksi Resurssienhallinta ratkaisee **variables('var1')** ja **VARIABLES('VAR1')** sama kuin. Kun lasketaan, ellet nimenomaisesti funktion Muokkaa palvelupyyntö (esimerkiksi toUpper tai toLower)-funktion säilyttää kirjainkoon. Tiettyjen resurssityypit ehkä kirjainkoon vaatimukset riippumatta siitä, miten funktiot lasketaan.

## <a name="numeric-functions"></a>Numeeriset Funktiot

Resurssienhallinta sisältää seuraavat Funktiot käsittelyyn kokonaislukuja:

- [Lisää](#add)
- [copyIndex](#copyindex)
- [jako](#div)
- [kokonaisluku](#int)
- [Mod](#mod)
- [MUL](#mul)
- [Sub](#sub)


<a id="add" />
### <a name="add"></a>Lisää

**Lisää (operand1 operand2)**

Palauttaa kahden annettu kokonaisluvun.

| Parametri                          | Pakollinen | Kuvaus
| :--------------------------------: | :------: | :----------
| operand1                           |   Kyllä    | Jos haluat lisätä ensimmäisen kokonaisluvun.
| operand2                           |   Kyllä    | Jos haluat lisätä toisen kokonaisluku.

Seuraavassa esimerkissä lasketaan yhteen kaksi parametria.

    "parameters": {
      "first": {
        "type": "int",
        "metadata": {
          "description": "First integer to add"
        }
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Second integer to add"
        }
      }
    },
    ...
    "outputs": {
      "addResult": {
        "type": "int",
        "value": "[add(parameters('first'), parameters('second'))]"
      }
    }

<a id="copyindex" />
### <a name="copyindex"></a>copyIndex

**copyIndex(offset)**

Palauttaa nykyisen hakemiston iteraation silmukan. 

| Parametri                          | Pakollinen | Kuvaus
| :--------------------------------: | :------: | :----------
| Siirtymä                           |   Ei    | Voit lisätä nykyisen arvon summa.

Tätä funktiota käytetään aina **Kopioi** objekti. Katso täydellinen kuvaus siitä, miten voit käyttää **copyIndex** [luominen Azure resurssien hallinnan resurssit useita kertoja](resource-group-create-multiple.md).

Seuraavassa esimerkissä esitetään, kopioi ympyrä ja nimestä indeksiarvo. 

    "resources": [ 
      { 
        "name": "[concat('examplecopy-', copyIndex())]", 
        "type": "Microsoft.Web/sites", 
        "copy": { 
          "name": "websitescopy", 
          "count": "[parameters('count')]" 
        }, 
        ...
      }
    ]


<a id="div" />
### <a name="div"></a>jako

**jako (operand1, operand2)**

Palauttaa kokonaisluvun jako kaksi annettu kokonaislukujen.

| Parametri                          | Pakollinen | Kuvaus
| :--------------------------------: | :------: | :----------
| operand1                           |   Kyllä    | Kokonaisluku, joka jaetaan.
| operand2                           |   Kyllä    | Kokonaisluku, joka voidaan jakaa. Ei voi olla 0.

Seuraavassa esimerkissä jakaa yhden parametrin toiseen parametrin.

    "parameters": {
      "first": {
        "type": "int",
        "metadata": {
          "description": "Integer being divided"
        }
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Integer used to divide"
        }
      }
    },
    ...
    "outputs": {
      "divResult": {
        "type": "int",
        "value": "[div(parameters('first'), parameters('second'))]"
      }
    }

<a id="int" />
### <a name="int"></a>kokonaisluku

**Int(valueToConvert)**

Muuntaa määritetyn arvon kokonaisluku.

| Parametri                          | Pakollinen | Kuvaus
| :--------------------------------: | :------: | :----------
| valueToConvert                     |   Kyllä    | Kokonaisluku muunnettava arvo. Arvon tyyppi voi olla vain merkkijono tai kokonaisluku.

Seuraavassa esimerkissä muuntaa käyttäjän antamaa parametriarvo kokonaisluku.

    "parameters": {
        "appId": { "type": "string" }
    },
    "variables": { 
        "intValue": "[int(parameters('appId'))]"
    }


<a id="mod" />
### <a name="mod"></a>Mod

**Mod (operand1, operand2)**

Palauttaa jakolaskun kokonaisluku-sivuliike, käyttämällä kahta annettu kokonaislukuja.

| Parametri                          | Pakollinen | Kuvaus
| :--------------------------------: | :------: | :----------
| operand1                           |   Kyllä    | Kokonaisluku, joka jaetaan.
| operand2                           |   Kyllä    | Kokonaisluku, joka voidaan jakaa, on oltava sama kuin 0.

Seuraava esimerkki palauttaa jakolaskun jakamalla yhden parametrin toisen parametrin.

    "parameters": {
      "first": {
        "type": "int",
        "metadata": {
          "description": "Integer being divided"
        }
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Integer used to divide"
        }
      }
    },
    ...
    "outputs": {
      "modResult": {
        "type": "int",
        "value": "[mod(parameters('first'), parameters('second'))]"
      }
    }

<a id="mul" />
### <a name="mul"></a>MUL

**MUL (operand1, operand2)**

Palauttaa kahden annettu kokonaislukujen kertolaskun.

| Parametri                          | Pakollinen | Kuvaus
| :--------------------------------: | :------: | :----------
| operand1                           |   Kyllä    | Jos haluat kertoa ensimmäisen kokonaisluvun.
| operand2                           |   Kyllä    | Jos haluat kertoa toisen kokonaisluku.

Seuraavassa esimerkissä kertoo toisen parametrin yksi parametri.

    "parameters": {
      "first": {
        "type": "int",
        "metadata": {
          "description": "First integer to multiply"
        }
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Second integer to multiply"
        }
      }
    },
    ...
    "outputs": {
      "mulResult": {
        "type": "int",
        "value": "[mul(parameters('first'), parameters('second'))]"
      }
    }

<a id="sub" />
### <a name="sub"></a>Sub

**Sub (operand1, operand2)**

Palauttaa kahden annettu kokonaisluvun vähennyslasku.

| Parametri                          | Pakollinen | Kuvaus
| :--------------------------------: | :------: | :----------
| operand1                           |   Kyllä    | Kokonaisluku, joka on vähennetään.
| operand2                           |   Kyllä    | Kokonaisluku, joka on vähennetty.

Seuraavassa esimerkissä vähentää yhden toinen parametri-parametri.

    "parameters": {
      "first": {
        "type": "int",
        "metadata": {
          "description": "Integer subtracted from"
        }
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Integer to subtract"
        }
      }
    },
    ...
    "outputs": {
      "subResult": {
        "type": "int",
        "value": "[sub(parameters('first'), parameters('second'))]"
      }
    }

## <a name="string-functions"></a>Merkkijonofunktiot

Resurssienhallinta merkkijonojen käsittelemisestä on seuraavat toiminnot:

- [Base64](#base64)
- [ketjutettu](#concat)
- [pituus](#lengthstring)
- [padLeft](#padleft)
- [Korvaa](#replace)
- [Ohita](#skipstring)
- [Jaa](#split)
- [merkkijono](#string)
- [alimerkkijono](#substring)
- [Ota](#takestring)
- [toLower](#tolower)
- [toUpper](#toupper)
- [rajaaminen](#trim)
- [uniqueString](#uniquestring)
- [URI](#uri)


<a id="base64" />
### <a name="base64"></a>Base64

**Base64 (inputString)**

Palauttaa syötteen merkkijonon base64-esitys.

| Parametri                          | Pakollinen | Kuvaus
| :--------------------------------: | :------: | :----------
| inputString                        |   Kyllä    | Palauttaa base64 esitys merkkijonoarvo.

Seuraavassa esimerkissä esitetään, miten base64-funktiota käytetään.

    "variables": {
      "usernameAndPassword": "[concat('parameters('username'), ':', parameters('password'))]",
      "authorizationHeader": "[concat('Basic ', base64(variables('usernameAndPassword')))]"
    }

<a id="concat" />
### <a name="concat---string"></a>ketjutettu - merkkijono

**ketjutettu (merkkijono1, merkkijono2, merkkijono3,...)**

Yhdistää useita merkkijonoarvoa ja palauttaa yhdistetty merkkijono. 

| Parametri                          | Pakollinen | Kuvaus
| :--------------------------------: | :------: | :----------
| merkkijono1                        |   Kyllä    | Jos haluat yhdistää merkkijonoarvo.
| Lisää merkkijonoja             |   Ei     | Merkkijonon arvot, jos haluat yhdistää.

Tämä funktio voi olla jokin muu luku argumenttien ja hyväksyä merkkijonoja tai matriiseja oleville parametreille. Esimerkki ketjuttaa matriisin on artikkelissa [ketjutettu - matriisin](#concatarray).

Seuraavassa esimerkissä esitetään, miten voit yhdistää useita merkkijonoarvoa palauttaa yhdistetty merkkijono.

    "outputs": {
        "siteUri": {
          "type": "string",
          "value": "[concat('http://', reference(resourceId('Microsoft.Web/sites', parameters('siteName'))).hostNames[0])]"
        }
    }


<a id="lengthstring" />
### <a name="length---string"></a>pituus - merkkijono

**length(String)**

Palauttaa tekstimerkkijonon merkkien määrän.

| Parametri                          | Pakollinen | Kuvaus
| :--------------------------------: | :------: | :----------
| merkkijono                        |   Kyllä    | Merkkijonoarvon, jota käytetään tehostavia merkkien määrän.

Esimerkki käytetään pituus ja taulukko-kohdassa [pituus - matriisin](#length).

Seuraava esimerkki palauttaa merkkijonon merkkien määrän. 

    "parameters": {
        "appName": { "type": "string" }
    },
    "variables": { 
        "nameLength": "[length(parameters('appName'))]"
    }
        

<a id="padleft" />
### <a name="padleft"></a>padLeft

**padLeft (valueToPad totalLength, paddingCharacter)**

Palauttaa merkkijonon, tasattu lisäämällä merkkiä vasemmalle asti kurottavat määritetyn pituinen.
  
| Parametri                          | Pakollinen | Kuvaus
| :--------------------------------: | :------: | :----------
| valueToPad                         |   Kyllä    | Merkkijono tai kokonaisluku tasata.
| totalLength                        |   Kyllä    | Palautetun merkkijonon merkkien määrä.
| paddingCharacter                   |   Ei     | Vasen-täyttö, kunnes pituinen saavutetaan käytettävät merkki. Oletusarvo on välilyönti.

Seuraavassa esimerkissä esitetään, miten kirjoituskentän käyttäjän antamaa parametriarvo lisäämällä nolla, kunnes merkkijonon saavuttaa 10 merkkiä. Jos alkuperäisen parametriarvo on yli 10 merkkiä, ei ole merkkien lisätään.

    "parameters": {
        "appName": { "type": "string" }
    },
    "variables": { 
        "paddedAppName": "[padLeft(parameters('appName'),10,'0')]"
    }

<a id="replace" />
### <a name="replace"></a>Korvaa

**Korvaa (originalString, oldCharacter, newCharacter)**

Palauttaa määritetyn merkkijonon, toinen merkki korvataan uusi merkkijono, joka sisältää kaikki esiintymät merkin.

| Parametri                          | Pakollinen | Kuvaus
| :--------------------------------: | :------: | :----------
| originalString                     |   Kyllä    | Merkkijono, joka on korvattu toisen merkin merkki kaikki esiintymät.
| oldCharacter                       |   Kyllä    | Voit poistaa alkuperäisen merkkijonon merkki.
| newCharacter                       |   Kyllä    | Voit lisätä tekstin näyttäminen poistettu merkin merkki.

Seuraavassa esimerkissä esitetään, miten voit poistaa kaikki katkoviivat käyttäjän antamaa merkkijonon.

    "parameters": {
        "identifier": { "type": "string" }
    },
    "variables": { 
        "newidentifier": "[replace(parameters('identifier'),'-','')]"
    }

<a id="skipstring" />
### <a name="skip---string"></a>Ohita - merkkijono
**Ohita (originalValue, numberToSkip)**

Palauttaa merkkijonon, jossa kaikki merkit määritetyn merkkijonon taaksepäin.

| Parametri                          | Pakollinen | Kuvaus
| :--------------------------------: | :------: | :----------
| originalValue                      |   Kyllä    | Merkkijono, käytettävä ohitetaan.
| numberToSkip                       |   Kyllä    | Voit ohittaa merkkien määrä. Jos tämä arvo on 0 tai pienempi, funktio palauttaa merkkijonon merkkiä. Jos se on suurempi kuin merkkijonon pituus, funktio palauttaa tyhjän merkkijonon. 

Esimerkki Ohita käyttäminen matriisi on artikkelissa [ohittaa - matriisi](#skip).

Seuraavassa esimerkissä ohittaa merkkijonomäärän merkkijonon.

    "parameters": {
      "first": {
        "type": "string",
        "metadata": {
          "description": "Value to use for skipping"
        }
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Number of characters to skip"
        }
      }
    },
    "resources": [
    ],
    "outputs": {
      "return": {
        "type": "string",
        "value": "[skip(parameters('first'),parameters('second'))]"
      }
    }


<a id="split" />
### <a name="split"></a>Jaa

**Jaa (inputString, delimiterString)**

**Jaa (inputString, delimiterArray)**

Palauttaa matriisin merkkijonoja, joka sisältää alimerkkijonoja syötteen merkkijonon, joka on erotettu toisistaan määritetyn erottimia.

| Parametri                          | Pakollinen | Kuvaus
| :--------------------------------: | :------: | :----------
| inputString                        |   Kyllä    | Voit jakaa merkkijono.
| erottimen                          |   Kyllä    | Jos haluat käyttää, erotin voi olla yhden merkkijonon tai matriisin merkkijonoja.

Seuraavassa esimerkissä jakautuu syötteen merkkijonon toisistaan pilkulla.

    "parameters": {
        "inputString": { "type": "string" }
    },
    "variables": { 
        "stringPieces": "[split(parameters('inputString'), ',')]"
    }

Seuraavassa esimerkissä jakautuu syötteen merkkijonon pilkuilla tai puolipisteellä.

    "variables": {
      "stringToSplit": "test1,test2;test3",
      "delimiters": [ ",", ";" ]
    },
    "resources": [ ],
    "outputs": {
      "exampleOutput": {
        "value": "[split(variables('stringToSplit'), variables('delimiters'))]",
        "type": "array"
      }
    }

<a id="string" />
### <a name="string"></a>merkkijono

**String(valueToConvert)**

Muuntaa tekstimerkkijonon määritetyllä arvolla.

| Parametri                          | Pakollinen | Kuvaus
| :--------------------------------: | :------: | :----------
| valueToConvert                     |   Kyllä    | Merkkijono muunnettava arvo. Minkätyyppinen arvo tahansa voidaan muuntaa, mukaan lukien objektit ja matriisien käyttöä.

Seuraavassa esimerkissä muuntaa käyttäjän antamaa parametriarvot merkkijonoja.

    "parameters": {
      "jsonObject": {
        "type": "object",
        "defaultValue": {
          "valueA": 10,
          "valueB": "Example Text"
        }
      },
      "jsonArray": {
        "type": "array",
        "defaultValue": [ "a", "b", "c" ]
      },
      "jsonInt": {
        "type": "int",
        "defaultValue": 5
      }
    },
    "variables": { 
      "objectString": "[string(parameters('jsonObject'))]",
      "arrayString": "[string(parameters('jsonArray'))]",
      "intString": "[string(parameters('jsonInt'))]"
    }

<a id="substring" />
### <a name="substring"></a>alimerkkijono

**alimerkkijono (stringToParse, startIndex, pituus)**

Palauttaa alimerkkijonon, joka alkaa määritetyn merkin ja sisältää määritetyn määrän merkkejä.

| Parametri                          | Pakollinen | Kuvaus
| :--------------------------------: | :------: | :----------
| stringToParse                     |   Kyllä    | Alkuperäinen merkkijono, josta alimerkkijonon puretaan.
| startIndex                         | Ei      | Nollasta ensimmäisen merkin sijainnin alimerkkijonon.
| pituus                             | Ei      | Alimerkkijonon merkkien määrä.

Seuraavassa esimerkissä poimii kolme ensimmäistä merkkiä parametrin.

    "parameters": {
        "inputString": { "type": "string" }
    },
    "variables": { 
        "prefix": "[substring(parameters('inputString'), 0, 3)]"
    }

<a id="takestring" />
### <a name="take---string"></a>Ota - merkkijono
**Ota (originalValue, numberToTake)**

Palauttaa määritetyn määrän merkkejä merkkijonon, joka palauttaa merkkijonon alusta.

| Parametri                          | Pakollinen | Kuvaus
| :--------------------------------: | :------: | :----------
| originalValue                      |   Kyllä    | Merkkijono tulevat olevat merkit.
| numberToTake                       |   Kyllä    | Jos haluat ottaa merkkien määrä. Jos tämä arvo on 0 tai pienempi, funktio palauttaa tyhjän merkkijonon. Jos se on suurempi kuin määritetyn merkkijonon pituus, kaikki merkkijonon merkit palautetaan.

Esimerkki Ota käyttäminen matriisi on artikkelissa [kestää - matriisi](#take).

Seuraavassa esimerkissä kerrotaan, kuinka merkkijonomäärän merkkijonosta.

    "parameters": {
      "first": {
        "type": "string",
        "metadata": {
          "description": "Value to use for taking"
        }
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Number of characters to take"
        }
      }
    },
    "resources": [
    ],
    "outputs": {
      "return": {
        "type": "string",
        "value": "[take(parameters('first'), parameters('second'))]"
      }
    }

<a id="tolower" />
### <a name="tolower"></a>toLower

**toLower(stringToChange)**

Muuntaa määritetyn merkkijonon, pienet kirjaimet.

| Parametri                          | Pakollinen | Kuvaus
| :--------------------------------: | :------: | :----------
| stringToChange                     |   Kyllä    | Merkkijono, voit muuntaa pienet kirjaimet.

Seuraavassa esimerkissä muuntaa käyttäjän antamaa parametriarvo pienet kirjaimet.

    "parameters": {
        "appName": { "type": "string" }
    },
    "variables": { 
        "lowerCaseAppName": "[toLower(parameters('appName'))]"
    }

<a id="toupper" />
### <a name="toupper"></a>toUpper

**toUpper(stringToChange)**

Muuntaa määritetyn merkkijonon isoiksi kirjaimiksi.

| Parametri                          | Pakollinen | Kuvaus
| :--------------------------------: | :------: | :----------
| stringToChange                     |   Kyllä    | Merkkijono, voit muuntaa isoiksi kirjaimiksi.

Seuraavassa esimerkissä muuntaa käyttäjän antamaa parametriarvo isoiksi kirjaimiksi.

    "parameters": {
        "appName": { "type": "string" }
    },
    "variables": { 
        "upperCaseAppName": "[toUpper(parameters('appName'))]"
    }

<a id="trim" />
### <a name="trim"></a>rajaaminen

**TRIM (stringToTrim)**

Poistaa kaikki alussa ja lopussa olevia piilotetut merkit määritetyn merkkijonon.

| Parametri                          | Pakollinen | Kuvaus
| :--------------------------------: | :------: | :----------
| stringToTrim                       |   Kyllä    | Merkkijono rajaaminen.

Seuraavassa esimerkissä rajaa välilyöntimerkit käyttäjän antamaa parametrin arvosta.

    "parameters": {
        "appName": { "type": "string" }
    },
    "variables": { 
        "trimAppName": "[trim(parameters('appName'))]"
    }

<a id="uniquestring" />
### <a name="uniquestring"></a>uniqueString

**uniqueString (baseString,...)**

Luo deterministic hash merkkijonon parametreiksi annettujen arvojen perusteella. 

| Parametri                          | Pakollinen | Kuvaus
| :--------------------------------: | :------: | :----------
| baseString      |   Kyllä    | Merkkijono, jota käytetään hash-funktio luo yksilöllinen merkkijono.
| Lisää parametrit sellaisena kuin se on tarpeen mukaan    | Ei       | Voit lisätä niin monta merkkijonoja tarvittaessa luoda arvo, joka määrittää yksilöllisyyden taso.

Tämä toiminto on hyödyllinen, kun haluat luoda resurssin yksilöllinen nimi. Sinun on määritettävä parametriarvot, joka rajoittaa yksilöllisyyden tuloksen. Voit määrittää, onko nimi on yksilöllinen tilauksen, resurssiryhmä tai käyttöönoton alaspäin. 

Palautettu arvo ei ole satunnainen merkkijono, mutta sen sijaan, että hash-funktion tulos. Palautettu arvo on 13 merkkiä pitkä. Ei ole yksilöivä. Voit yhdistää etuliite oman nimeämiskäytäntöä luominen kuvaava nimi-arvo. Seuraavassa esimerkissä esitetään palautetun arvon muoto. Todellinen arvo vaihtelee, annettujen parametrien.

    tcvhiyu5h2o5o

Seuraavissa esimerkeissä käyttämisestä uniqueString Luo yksilöllinen arvo tavallisimmat tasoissa.

Tilauksen suodatetut yksilölliset arvot

    "[uniqueString(subscription().subscriptionId)]"

Resurssiryhmä suodatetut yksilölliset arvot

    "[uniqueString(resourceGroup().id)]"

Yksilölliset arvot suodatetut resurssiryhmä käyttöönotto

    "[uniqueString(resourceGroup().id, deployment().name)]"
    
Seuraavassa esimerkissä on yksilöllinen nimi resurssiryhmän perusteella tallennustilan tilin luominen (tämän resurssiryhmän nimi ei sisällä yksilöllinen Jos rakennettava samalla tavalla).

    "resources": [{ 
        "name": "[concat('contosostorage', uniqueString(resourceGroup().id))]", 
        "type": "Microsoft.Storage/storageAccounts", 
        ...



<a id="uri" />
### <a name="uri"></a>URI

**URI (baseUri, relativeUri)**

Luo absoluuttinen URI baseUri ja relativeUri merkkijonon yhdistämällä.

| Parametri                          | Pakollinen | Kuvaus
| :--------------------------------: | :------: | :----------
| baseUri                            |   Kyllä    | Perus-uri-merkkijono.
| relativeUri                        |   Kyllä    | Perus-uri-merkkijono lisääminen suhteellinen uri-merkkijono.

**BaseUri** -parametrin voi sisältää tietyn tiedoston, mutta vain pääkansion polun käytetään URI luodessaan. Esimerkiksi kulkeva **http://contoso.com/resources/azuredeploy.json** baseUri kuin parametrin johtaa **http://contoso.com/resources/**perus-URI.

Seuraavassa esimerkissä esitetään muodostamisesta linkin sisäkkäisiä malleja ylemmän tason mallin arvon perusteella.

    "templateLink": "[uri(deployment().properties.templateLink.uri, 'nested/azuredeploy.json')]"

## <a name="array-functions"></a>Matriisi-Funktiot

Resurssienhallinta sisältää useita funktioita käsittelyyn taulukon arvot.

- [ketjutettu](#concatarray)
- [pituus](#length)
- [Ohita](#skip)
- [Ota](#take)

Saat matriisin merkkijonoarvot on erotettu arvon mukaan-kohdassa [Jaa](#split).

<a id="concatarray" />
### <a name="concat---array"></a>ketjutettu - matriisi

**ketjutettu (matriisi1, matriisi2, matriisi3,...)**

Yhdistää useita taulukoita ja palauttaa ketjutetut matriisin. 

| Parametri                          | Pakollinen | Kuvaus
| :--------------------------------: | :------: | :----------
| Matriisi1                        |   Kyllä    | Jos haluat yhdistää matriisi.
| Lisää taulukoita             |   Ei     | Jos haluat yhdistää matriiseja.

Tämä funktio voi olla jokin muu luku argumenttien ja hyväksyä merkkijonoja tai matriiseja oleville parametreille. Esimerkki ketjuttaa merkkijonoarvoa on artikkelissa [ketjutettu - merkkijono](#concat).

Seuraavassa esimerkissä esitetään, miten voit yhdistää kahden matriisin.

    "parameters": {
        "firstarray": {
            type: "array"
        }
        "secondarray": {
            type: "array"
        }
     },
     "variables": {
         "combinedarray": "[concat(parameters('firstarray'), parameters('secondarray'))]
     }
        

<a id="length" />
### <a name="length---array"></a>pituus - matriisi

**length(Array)**

Palauttaa matriisin elementtien määrä.

| Parametri                          | Pakollinen | Kuvaus
| :--------------------------------: | :------: | :----------
| matriisi                        |   Kyllä    | Matriisin käytettävät käytön elementtien määrä.

Matriisin kanssa tämän funktion avulla voit määrittää minuuttimäärän, jonka Iteraatioita luotaessa resursseja. Seuraavassa esimerkissä parametrin **siteNames** viitata matriisin nimet web-sivustojen luomiseen.

    "copy": {
        "name": "websitescopy",
        "count": "[length(parameters('siteNames'))]"
    }

Saat lisätietoja tämän funktion käyttämisestä matriisin [luominen Azure resurssien hallinnan resurssit useita kertoja](resource-group-create-multiple.md). 

Esimerkki pituus käyttäminen merkkijonoarvo on artikkelissa [pituus - merkkijono](#lengthstring).

<a id="skip" />
### <a name="skip---array"></a>Ohita - matriisi
**Ohita (originalValue, numberToSkip)**

Palauttaa kaikkien osien kanssa kun määritetty matriisista.

| Parametri                          | Pakollinen | Kuvaus
| :--------------------------------: | :------: | :----------
| originalValue                      |   Kyllä    | Voit käyttää ohittamiseen taulukko.
| numberToSkip                       |   Kyllä    | Voit ohittaa elementtien määrä. Jos tämä arvo on 0 tai pienempi, kaikki matriisin elementit palautetaan. Jos se on suurempi kuin matriisin pituus, funktio palauttaa tyhjän taulukon. 

Esimerkki merkkijonon käyttäminen Ohita on artikkelissa [Ohita - merkkijono](#skipstring).

Seuraavassa esimerkissä ohittaa matriisissa määritetyn määrän.

    "parameters": {
      "first": {
        "type": "array",
        "metadata": {
          "description": "Values to use for skipping"
        },
        "defaultValue": [ "one", "two", "three" ]
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Number of elements to skip"
        }
      }
    },
    "resources": [
    ],
    "outputs": {
      "return": {
        "type": "array",
        "value": "[skip(parameters('first'), parameters('second'))]"
      }
    }

<a id="take" />
### <a name="take---array"></a>Ota - matriisi
**Ota (originalValue, numberToTake)**

Palauttaa määritetyn määrän elementtejä kanssa matriisin alusta.

| Parametri                          | Pakollinen | Kuvaus
| :--------------------------------: | :------: | :----------
| originalValue                      |   Kyllä    | Osien hakeminen tulevat taulukko.
| numberToTake                       |   Kyllä    | Ottaa elementtien määrä. Jos tämä arvo on 0 tai pienempi, funktio palauttaa tyhjän taulukon. Jos se on suurempi kuin määritetyn taulukon pituuden, kaikki matriisin elementit palautetaan.

Esimerkki Ota käyttäminen merkkijono on artikkelissa [kestää - merkkijono](#takestring).

Seuraavassa esimerkissä kerrotaan, kuinka osat matriisin annettuun määrään.

    "parameters": {
      "first": {
        "type": "array",
        "metadata": {
          "description": "Values to use for taking"
        },
        "defaultValue": [ "one", "two", "three" ]
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Number of elements to take"
        }
      }
    },
    "resources": [
    ],
    "outputs": {
      "return": {
        "type": "array",
        "value": "[take(parameters('first'),parameters('second'))]"
      }
    }

## <a name="deployment-value-functions"></a>Käyttöönoton arvo-Funktiot

Resurssienhallinta käytön osia mallin ja käyttöönottoon liittyvät arvot on seuraavat toiminnot:

- [käyttöönotto](#deployment)
- [Parametrit](#parameters)
- [muuttujat](#variables)

Hakee arvoja resursseja, resurssiryhmät tai tilaukset-kohdassa [resurssi-Funktiot](#resource-functions).

<a id="deployment" />
### <a name="deployment"></a>käyttöönotto

**Deployment()**

Palauttaa tietoja nykyistä käyttöönotto-toimintoa.

Tämä funktio palauttaa objekti, joka on välitetty käyttöönoton aikana. Palautetun objektin ominaisuudet vaihtelevat mukaan, onko käyttöönotto-objektin välitetään linkin tai sitoutuvat objektina. 

Kun käyttöönotto-objekti on kulunut sitoutuvat, kuten käyttäessäsi PowerShellin Azure **- TemplateFile** -parametrin osoittamaan paikallisen tiedoston, palautettu objekti on seuraavanlainen:

    {
        "name": "",
        "properties": {
            "template": {
                "$schema": "",
                "contentVersion": "",
                "parameters": {},
                "variables": {},
                "resources": [
                ],
                "outputs": {}
            },
            "parameters": {},
            "mode": "",
            "provisioningState": ""
        }
    }

Kun objekti välitetään linkkinä, kuten käytettäessä **- TemplateUri** parametria osoittamaan remote objektin objektin palautetaan muodossa. 

    {
        "name": "",
        "properties": {
            "templateLink": {
                "uri": ""
            },
            "template": {
                "$schema": "",
                "contentVersion": "",
                "parameters": {},
                "variables": {},
                "resources": [],
                "outputs": {}
            },
            "parameters": {},
            "mode": "",
            "provisioningState": ""
        }
    }

Seuraavassa esimerkissä esitetään käyttämisestä deployment() Linkitä toiseen malliin URI ylemmän tason mallin perusteella.

    "variables": {  
        "sharedTemplateUrl": "[uri(deployment().properties.templateLink.uri, 'shared-resources.json')]"  
    }  

<a id="parameters" />
### <a name="parameters"></a>Parametrit

**parametrien (parameterName)**

Palauttaa parametrin arvon. Määritetyn parametrin nimi on määritettävä mallin parametrit-osassa.

| Parametri                          | Pakollinen | Kuvaus
| :--------------------------------: | :------: | :----------
| parameterName                      |   Kyllä    | Palauttaa parametrin nimi.

Seuraavassa esimerkissä esitetään yksinkertaistettu käytetään parametrit-funktiota.

    "parameters": { 
      "siteName": {
          "type": "string"
      }
    },
    "resources": [
       {
          "apiVersion": "2014-06-01",
          "name": "[parameters('siteName')]",
          "type": "Microsoft.Web/Sites",
          ...
       }
    ]

<a id="variables" />
### <a name="variables"></a>muuttujat

**muuttujat (variableName)**

Palauttaa muuttujan arvon. Määritetyn muuttujan nimi on määritettävä mallin muuttujat-osassa.

| Parametri                          | Pakollinen | Kuvaus
| :--------------------------------: | :------: | :----------
| muuttujan nimi                      |   Kyllä    | Palauttaa muuttujan nimi.

Seuraavassa esimerkissä muuttuja-arvoa.

    "variables": {
      "storageName": "[concat('storage', uniqueString(resourceGroup().id))]"
    },
    "resources": [
      {
        "type": "Microsoft.Storage/storageAccounts",
        "name": "[variables('storageName')]",
        ...
      }
    ],

## <a name="resource-functions"></a>Resurssin Funktiot

Resurssienhallinta käytön resurssin arvot on seuraavat toiminnot:

- [listKeys ja luettelo {Value}](#listkeys)
- [toimittajien](#providers)
- [viittaus](#reference)
- [resourceGroup](#resourcegroup)
- [resourceId](#resourceid)
- [tilauksen](#subscription)

Hakee arvoja parametrit, muuttujien tai nykyisen käyttöönotto-kohdassa [käyttöönoton arvo Funktiot](#deployment-value-functions).

<a id="listkeys" />
<a id="list" />
### <a name="listkeys-and-listvalue"></a>listKeys ja luettelo {Value}

**listKeys (resourceName tai resourceIdentifier, apiVersion)**

**luettelon {Value} (resourceName tai resourceIdentifier, apiVersion)**

Palauttaa kaikki Resurssityyppi, joka tukee luettelo-toiminto arvot. Yleisimmät käyttö on **listKeys**. 
  
| Parametri                          | Pakollinen | Kuvaus
| :--------------------------------: | :------: | :----------
| resourceName tai resourceIdentifier |   Kyllä    | Resurssin yksilöllinen.
| apiVersion                         |   Kyllä    | Resurssin suorituksenaikainen tila API-versio.

Toiminto, joka alkaa **luettelon** voidaan käyttää funktiota mallin. Käytettävissä olevat toiminnot ovat **listKeys**lisäksi myös toimintoja, kuten **luettelon** **listAdminKeys**ja **listStatus**. Voit selvittää, mitkä resurssityypit on luettelo-toiminto, käytä seuraavaa PowerShell-komentoa.

    Get-AzureRmProviderOperation -OperationSearchString *  | where {$_.Operation -like "*list*"} | FT Operation

Tai hakea Azure CLI luettelomuotoisena. Seuraavassa esimerkissä hakee **apiapps**kaikki toiminnot ja JSON-apuohjelman [jq](http://stedolan.github.io/jq/download/) avulla voit suodattaa vain luettelon toiminnot.

    azure provider operations show --operationSearchString */apiapps/* --json | jq ".[] | select (.operation | contains(\"list\"))"

ResourceId voidaan määrittää [resourceId-toiminnon](#resourceid) avulla tai käyttämällä muotoa **{providerNamespace} / {resourceType} / {resourceName}**.

Seuraavassa esimerkissä esitetään, kuinka palauttaa ensisijainen ja toissijainen näppäimet tallennustilan tilin tulostus-osassa.

    "outputs": { 
      "listKeysOutput": { 
        "value": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName')), '2016-01-01')]", 
        "type" : "object" 
      } 
    } 

Palautetun objektin listKeys on seuraavanlainen:

    {
      "keys": [
        {
          "keyName": "key1",
          "permissions": "Full",
          "value": "{value}"
        },
        {
          "keyName": "key2",
          "permissions": "Full",
          "value": "{value}"
        }
      ]
    }

<a id="providers" />
### <a name="providers"></a>toimittajien

**toimittajien (providerNamespace; [resourceType])**

Palauttaa tietoja resurssin tarjoajaan ja sen tuetuista resurssityypit. Jos et anna resurssilaji-funktio palauttaa resurssi-palvelun tuetut tietotyypit.

| Parametri                          | Pakollinen | Kuvaus
| :--------------------------------: | :------: | :----------
| providerNamespace                  |   Kyllä    | Palvelun Namespace
| resourceType                       |   Ei     | Resurssin määritetyn nimitilan tyyppi.

Tuetut kunkin tyyppiselle palautetaan muodossa. Matriisin järjestys ei välttämättä.

    {
        "resourceType": "",
        "locations": [ ],
        "apiVersions": [ ]
    }

Seuraavassa esimerkissä esitetään, miten toimittaja-funktiota käytetään:

    "outputs": {
        "exampleOutput": {
            "value": "[providers('Microsoft.Storage', 'storageAccounts')]",
            "type" : "object"
        }
    }

<a id="reference" />
### <a name="reference"></a>viittaus

**viittaus (resourceName tai resourceIdentifier, [apiVersion])**

Palauttaa objektiin toiselle resurssille suorituksenaikainen tila.

| Parametri                          | Pakollinen | Kuvaus
| :--------------------------------: | :------: | :----------
| resourceName tai resourceIdentifier |   Kyllä    | Nimi tai resurssin yksilöllinen.
| apiVersion                         |   Ei     | Määritettyä resurssia API-versio. Lisää tämä parametri, kun resurssin ei ole valmisteltu sisällä samaan malliin.

**Viite** -funktio hakee arvonsa runtime-tilasta ja näin ollen ei voi käyttää muuttujat-osassa. Sen avulla voidaan tulostaa osan mallina.

Viite-funktiolla implisiittisesti kielessä, yksi resurssi riippuu toiselle resurssille viitatun resurssi on valmisteltu sisällä samaan malliin. Sinun ei tarvitse myös **dependsOn** -ominaisuudella. Funktio ei lasketa, kunnes viitatun resurssi on tehnyt käyttöönotto.

Seuraavassa esimerkissä viittaa tallennustilan-tili, joka on otettu käyttöön samaan malliin.

    "outputs": {
        "NewStorage": {
            "value": "[reference(parameters('storageAccountName'))]",
            "type" : "object"
        }
    }

Seuraavassa esimerkissä viittaa tallennustilan-tili, jota ei ole otettu käyttöön, tätä mallia, mutta saman resurssin ryhmän otetaan käyttöön resursseina on olemassa.

    "outputs": {
        "ExistingStorage": {
            "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName')), '2016-01-01')]",
            "type" : "object"
        }
    }

Voit hakea tietyn arvon palautetut objektista, kuten blob päätepisteen URI-tunnus, kuten seuraavassa esimerkissä.

    "outputs": {
        "BlobUri": {
            "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName')), '2016-01-01').primaryEndpoints.blob]",
            "type" : "string"
        }
    }

Seuraavassa esimerkissä viittaa tallennustilan-tilin eri resurssiryhmä.

    "outputs": {
        "BlobUri": {
            "value": "[reference(resourceId(parameters('relatedGroup'), 'Microsoft.Storage/storageAccounts/', parameters('storageAccountName')), '2016-01-01').primaryEndpoints.blob]",
            "type" : "string"
        }
    }

**Viite** -funktion palauttama objektin ominaisuudet vaihtelevat resurssin lajin mukaan. Ominaisuuden nimet ja resurssilaji-arvot näkyviin yksinkertainen malli, joka palauttaa objektin **tulostaa** osan luominen Jos sinulla on aiemman resurssin kyseisen tyypin, mallin palauttaa vain objektin ilman käyttöönotto uusi resursseja. Jos sinulla ei ole aiemman resurssin kyseisen tyypin, mallin ottaa käyttöön vain tyypin ja palauttaa objektin. Lisää nämä ominaisuudet muista malleista, jotka on hakea dynaamisesti käyttöönoton aikana. 

<a id="resourcegroup" />
### <a name="resourcegroup"></a>resourceGroup

**resourceGroup()**

Palauttaa objektin, joka edustaa nykyisen resurssiryhmä. 

Palautettujen objekti on seuraavanlainen:

    {
      "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}",
      "name": "{resourceGroupName}",
      "location": "{resourceGroupLocation}",
      "tags": {
      },
      "properties": {
        "provisioningState": "{status}"
      }
    }

Seuraavassa esimerkissä resurssin ryhmän sijainti määrittää sivuston sijainti.

    "resources": [
       {
          "apiVersion": "2014-06-01",
          "type": "Microsoft.Web/sites",
          "name": "[parameters('siteName')]",
          "location": "[resourceGroup().location]",
          ...
       }
    ]

<a id="resourceid" />
### <a name="resourceid"></a>resourceId

**resourceId ([subscriptionId], [resourceGroupName] resourceType, resourceName1, [resourceName2]...)**

Palauttaa resurssin yksilöllinen. 
      
| Parametri         | Pakollinen | Kuvaus
| :---------------: | :------: | :----------
| subscriptionId    |   Ei     | Oletusarvo on voimassa oleva tilaus. Määritä tämä arvo, kun haluat hakea resurssin toiseen tilaukseen.
| resourceGroupName |   Ei     | Oletusarvo on nykyinen resurssiryhmä. Määritä tämä arvo, kun haluat hakea toisen resurssiryhmä resurssin.
| resourceType      |   Kyllä    | Resurssien mukaan lukien resurssin tarjoajan nimitilan tyyppi.
| resourceName1     |   Kyllä    | Resurssin nimi.
| resourceName2     |   Ei     | Seuraava resurssin nimi osa Jos resurssi on IF.

Voit käyttää tätä toimintoa, kun resurssinimi on epäselvä tai valmisteltu ole samassa mallissa. Tunnus palautetaan seuraavanlainen:

    /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/{resourceProviderNamespace}/{resourceType}/{resourceName}

Seuraavassa esimerkissä esitetään noutamisesta web-sivuston ja tietokannan resurssitunnukset. Sivusto on nimeltä **myWebsitesGroup** resurssiryhmä ja tietokanta on nykyisen resurssiryhmän tämän mallin.

    [resourceId('myWebsitesGroup', 'Microsoft.Web/sites', parameters('siteName'))]
    [resourceId('Microsoft.SQL/servers/databases', parameters('serverName'), parameters('databaseName'))]
    
Usein haluat käyttää tätä toimintoa, kun käytät tallennustilan asiakas- tai VPN vaihtoehtoinen resurssi-ryhmässä. Tallennustilan asiakas- tai VPN voidaan käyttää useiden resurssin ryhmien; Tämän vuoksi et halua poistaa ne yhteen resurssiryhmä poistettaessa. Seuraavassa esimerkissä esitetään, miten helposti voidaan ulkoisen resurssiryhmä resurssin:

    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
          "virtualNetworkName": {
              "type": "string"
          },
          "virtualNetworkResourceGroup": {
              "type": "string"
          },
          "subnet1Name": {
              "type": "string"
          },
          "nicName": {
              "type": "string"
          }
      },
      "variables": {
          "vnetID": "[resourceId(parameters('virtualNetworkResourceGroup'), 'Microsoft.Network/virtualNetworks', parameters('virtualNetworkName'))]",
          "subnet1Ref": "[concat(variables('vnetID'),'/subnets/', parameters('subnet1Name'))]"
      },
      "resources": [
      {
          "apiVersion": "2015-05-01-preview",
          "type": "Microsoft.Network/networkInterfaces",
          "name": "[parameters('nicName')]",
          "location": "[parameters('location')]",
          "properties": {
              "ipConfigurations": [{
                  "name": "ipconfig1",
                  "properties": {
                      "privateIPAllocationMethod": "Dynamic",
                      "subnet": {
                          "id": "[variables('subnet1Ref')]"
                      }
                  }
              }]
           }
      }]
    }

<a id="subscription" />
### <a name="subscription"></a>tilauksen

**Subscription()**

Palauttaa tietoja tilauksen muotoa.

    {
        "id": "/subscriptions/#####",
        "subscriptionId": "#####",
        "tenantId": "#####"
    }

Seuraavassa esimerkissä esitetään tilauksen tulostaa osan funktiota. 

    "outputs": { 
      "exampleOutput": { 
          "value": "[subscription()]", 
          "type" : "object" 
      } 
    } 


## <a name="next-steps"></a>Seuraavat vaiheet
- Kuvaus Azure Resurssienhallinta-mallin osissa on kohdassa [yhtä aikaa muiden kanssa Azure Resurssienhallinta-mallit](resource-group-authoring-templates.md)
- Jos haluat yhdistää useita malleja, katso [linkitetyn mallien Azure resurssien hallinnan avulla](resource-group-linked-templates.md)
- Tietyn määrän kertoja käydä luodessasi resurssin lajin, katso [luominen Azure resurssien hallinnan resurssit useita kertoja](resource-group-create-multiple.md)
- Olet luonut mallin ottamisesta käyttöön on kohdassa [käyttöönotto sovelluksen Azure Resurssienhallinta-malli](resource-group-template-deploy.md)


<properties 
    pageTitle="Uuden mallin version 2016-06-01 | Microsoft Azure" 
    description="Opettele kirjoittamaan JSON määrittelyn logiikan sovelluksia uusin versio" 
    authors="jeffhollan" 
    manager="dwrede" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/25/2016"
    ms.author="jehollan"/>
    
# <a name="new-schema-version-2016-06-01"></a>Uuden mallin version 2016-06-01

Uusi malli ja API logiikan sovellusten versio on parannuksia, mikä parantaa luotettavuutta ja helpottaminen käyttäminen logiikan sovelluksia. On 3 keskeisiä eroja:

1. Alueet-toiminnot, jotka sisältävät kokoelma toiminnot, joita lisäys.
1. Ehdot ja silmukat ovat pääosin toiminnot
1. Suorittamisen järjestys tarkempia kautta `runAfter` ominaisuuden (mikä korvaa `dependsOn`)

Lisätietoja logiikan-sovellusten päivittäminen 2015-08-01 – esikatselu-rakenteen 2016-06-01-rakenteen [Tarkista päivityksen aikatiedot.](#upgrading-to-2016-06-01-schema)


## <a name="1-scopes"></a>1. käyttöalueen

Tämän mallin suurimmista muutoksista on käyttöalueen ja mahdollisuus sisäkkäisiä sisäkkäistä toiminnot.  Tästä on hyötyä, kun ryhmittely toimintojoukon yhdessä tai hänen nimeään tarvitse lisätä sisäkkäisiä toiminnot sisäkkäistä (esimerkiksi ehto voi olla toisen ehdon).  Lisätietoja laajuuden syntaksi voi olla löytyneen [tähän](app-service-logic-loops-and-scopes.md)mutta yksinkertainen laajuus Esimerkki löytyvät alla:


```
{
    "actions": {
        "My_Scope": {
            "type": "scope",
            "actions": {                
                "Http": {
                    "inputs": {
                        "method": "GET",
                        "uri": "http://www.bing.com"
                    },
                    "runAfter": {},
                    "type": "Http"
                }
            }
        }
    }
}
```

## <a name="2-conditions-and-loops-changes"></a>2. ehdot ja silmukat muutokset

Rakenteen aiemmissa versioissa ehdot ja silmukat on liitetty yksi toiminto parametrit.  Tämä rajoitus on ollut palauttaa tätä rakennetta ja nyt ehdot ja silmukat näkyvät toiminnon tyyppi.  Lisätietoja [tämän artikkelin](app-service-logic-loops-and-scopes.md)löytyy ja Esimerkki ehto-toiminto on alla:

```
{
    "If_trigger_is_foo": {
        "type": "If",
        "expression": "@equals(triggerBody(), 'foo')",
        "runAfter": { },
        "actions": {
            "Http_2": {
                "inputs": {
                    "method": "GET",
                    "uri": "http://www.bing.com"
                },
                "runAfter": {},
                "type": "Http"
            }
        },
        "else": 
        {
            "if_trigger_is_bar": "..."
        }      
    }
}
```

## <a name="3-runafter-property"></a>3. ominaisuuden RunAfter

Uuden `runAfter` ominaisuuden korvaa `dependsOn` avulla sallia tarkkaan Suorita järjestys.  `dependsOn`oli paremmin "toiminto suoritettiin ja onnistui,", mutta monta kertaa haluat suorittaa toiminnon, jos edellinen toiminto onnistuu, epäonnistui tai ohitetaan.  `runAfter`mahdollistaa kyseisen joustavuutta.  Onko objekti, joka määrittää kaikki sen jälkeen suoritettava toiminto nimet ja määrittää tila, joka on hyväksyttävä käynnistimen matriisin.  Esimerkki, jos haluat suorittaa jälkeen vaiheet onnistui A ja B onnistui tai se ei onnistunut, käyttää seuraavia `runAfter` ominaisuus:

```
{
    "...",
    "runAfter": {
        "A": ["Succeeded"],
        "B": ["Succeeded", "Failed"]
    }
}
```

## <a name="upgrading-to-2016-06-01-schema"></a>2016-06-01 rakenteen päivittäminen

Uuden 2016-06-01 rakenteen päivittäminen kestää vain muutama vaihe.  Rakenteen muutokset tiedot löytyvät [Tässä artikkelissa](app-service-logic-schema-2016-04-01.md).  Päivitysprosessin on käynnissä päivityksen komentosarjan tallentaminen uutena sovelluksena logiikan ja korvaamisen mahdollisesti vanha logiikan app tarvittaessa.

1. Avaa nykyinen logiikan-sovellus.
1. Valitse työkalurivin **Päivitä rakenne** -painike
   
    ![][1]
   
    Päivitetyn määritelmän palautetaan.  Voit kopioida ja liittää tämän resurssin määritys, jos tarvitset, mutta on **erittäin suositeltavaa** , että kaikki yhteys **Tallenna nimellä** -painikkeen avulla viittaukset eivät ole kelvollisia päivitetyn logiikan-sovelluksessa.
1. Napsauta työkalurivin Päivitä sivu **Tallenna nimellä** -painiketta.
1. Täytä nimi ja logiikka-sovelluksen tila ja valitsemalla **Luo** päivityksen logiikan sovelluksen käyttöönotto.
1. Tarkista päivitetyn logiikan-sovellus toimii odotetulla tavalla.

    >[AZURE.NOTE] Jos käytössäsi on Manuaalinen tai pyyntö käynnistin, uusi logiikan sovelluksen ovat muuttuneet takaisinsoitto URL-osoite.  Uusi URL-Osoitteen avulla voit tarkistaa se toimii lopusta loppuun, ja voit kopioida aiemmin logiikan sovelluksen säilyttää edellisen URL-osoitteet päälle.

1. *Valinnainen* Korvaa edellisen logiikan sovelluksen uusi rakenneversio (vierekkäiset **Päivityksen rakenne** -kuvaketta seuraavassa kuvassa) työkalurivillä **Kloonaa** -painikkeen avulla.  Tämä on tarpeen vain, jos haluat säilyttää saman Resurssitunnus tai pyydä logiikan sovelluksen käynnistimen URL-osoite.

### <a name="upgrade-tool-notes"></a>Päivitystyökalun muistiinpanot

#### <a name="condition-mapping"></a>Ehto yhdistäminen

Työkalu saat parhaan työmäärään ryhmittämään true ja false haaran toiminnot yhdessä alueen päivitetyn määrityksessä.  Erityisesti suunnittelutyökalun rakenteessa `@equals(actions('a').status, 'Skipped')` pitäisi näyttää sisältökorttina `else` toiminto.  Jos työkalu tunnistaa kuviot, se ei tunnista se on mahdollisesti luoda erilliset edellytykset TOSI ja EPÄTOSI haaran.  Toiminnot voivat olla uudelleen yhdistettyjen Lähetä päivitys tarvittaessa.

#### <a name="foreach-with-condition"></a>ForEach-ehdolla
  
Foreach silmukan ehdolla kohti kohteen edellistä kaavaa voi replikoida suodatustoiminto uusi rakenne.  Suoritetaan automaattisesti päivitettäessä.  Ehto tulee suodatustoiminnon ennen foreach silmukan (vain kohteet, jotka vastaavat ehto matriisin palauttamiseksi) ja foreach-toiminto on siirretty matriisin.  Voit tarkastella esimerkki tämän [artikkelin sisältö](app-service-logic-loops-and-scopes.md)

#### <a name="resource-tags"></a>Resurssin tunnisteet

Resurssin tunnisteet poistetaan päivitys ja haluat määrittää ne uudelleen päivitetyn työnkulun.

## <a name="other-changes"></a>Muut muutokset

### <a name="manual-trigger-renamed-to-request-trigger"></a>Manuaalinen käynnistäminen pyynnön käynnistäminen uudelleen

Tyypin `manual` on poistettu ja nimetä uudelleen `request` kanssa tyyppi, `http`.  Tämä on entistä yhdenmukaisemmat käynnistintä käytetään muodostaa kuvion tyyppi.

### <a name="new-filter-action"></a>Uusi "suodatustoiminto"

Jos käsittelet suuri matriisi ja haluat suodattaa alaspäin pienempi joukko kohteita, voit käyttää uuden "" suodatintyyppi.  Se hyväksyy matriisin ja ehto ja arvioi kutakin ehto ja palauttaa matriisin ehdon täyttävien kohteiden.

### <a name="foreach-and-until-action-restrictions"></a>ForEach ja ennen toiminnon rajoitukset

Foreach ja silmukan asti on rajoitettu yhteen-toiminto.

### <a name="trackedproperties-on-actions"></a>TrackedProperties-toiminnot

Toiminnot voi olla lisäominaisuuden (saman tason haluat `runAfter` ja `type`) kutsutaan `trackedProperties`.  Objekti, joka määrittää tiettyjen toiminnon syötteiden tai tulostaa Azure diagnostiikan telemetriatietojen, jotka on lähetetty työnkulun osana sisällytettävät on.  Esimerkki:

```
{                
    "Http": {
        "inputs": {
            "method": "GET",
            "uri": "http://www.bing.com"
        },
        "runAfter": {},
        "type": "Http",
        "trackedProperties": {
            "responseCode": "@action().outputs.statusCode",
            "uri": "@action().inputs.uri"
        }
    }
}
```

## <a name="next-steps"></a>Seuraavat vaiheet
- [Käytä logiikan app työnkulun määritys](app-service-logic-author-definitions.md)
- [Logiikan sovelluksen käyttöönotto mallin luominen](app-service-logic-create-deploy-template.md)


<!-- Image references -->
[1]: ./media/app-service-logic-schema-2016-04-01/upgradeButton.png

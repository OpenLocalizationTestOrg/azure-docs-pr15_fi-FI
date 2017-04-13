<properties
    pageTitle="Logiikan App rajoitukset ja määritykset | Microsoft Azure"
    description="Yleisiä tietoja palvelun rajoitukset ja määritykset arvot käytettävissä logiikan-sovelluksissa."
    services="logic-apps"
    documentationCenter=".net,nodejs,java"
    authors="jeffhollan"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jehollan"/>

# <a name="logic-app-limits-and-configuration"></a>Logiikan App rajoitukset ja määritykset

Seuraavassa on tietoja nykyisen rajoittaa ja määritysten tiedot Azure logiikan sovellusten.

## <a name="limits"></a>Rajoitukset

### <a name="http-request-limits"></a>HTTP-pyynnön rajoitukset

Nämä ovat yhden HTTP-pyynnön ja/tai yhdistin puhelun rajoitukset

#### <a name="timeout"></a>Aikakatkaisu

|Nimi|Raja|Huomautuksia|
|----|----|----|
|Pyynnön aikakatkaisu|1 minuutti|[Asynkroninen kuvion](app-service-logic-create-api-app.md) tai [kunnes silmukan](app-service-logic-loops-and-scopes.md) hyvittää tarpeen mukaan|

#### <a name="message-size"></a>Viestin koko

|Nimi|Raja|Huomautuksia|
|----|----|----|
|Viestin koko|50 MT|Jotkin yhdistimien ja ohjelmointirajapinnan eivät tue 50 Megatavua.  Pyyntö käynnistimen tukee enintään 25 Mt|
|Lausekkeen arvioinnista raja|131,072 merkit|`@concat()`, `@base64()`, `string` voi olla enintään tämä|

#### <a name="retry-policy"></a>Yritä käytäntö

|Nimi|Raja|Huomautuksia|
|----|----|----|
|Yritä yritykset|4|Voit määrittää [käytännön parametrin uudelleen](https://msdn.microsoft.com/en-us/library/azure/mt643939.aspx)|
|Max viive:|1 tunti|Voit määrittää [käytännön parametrin uudelleen](https://msdn.microsoft.com/en-us/library/azure/mt643939.aspx)|
|Min viive:|20 min|Voit määrittää [käytännön parametrin uudelleen](https://msdn.microsoft.com/en-us/library/azure/mt643939.aspx)|

### <a name="run-duration-and-retention"></a>Suorita keston ja säilytys

Nämä ovat suorittaa yhteen logiikan sovelluksen rajoitukset.

|Nimi|Raja|Huomautuksia|
|----|----|----|
|Suorita kesto|90 päivää||
|Tallennustilan säilytys|90 päivää|Tämä on Suorita alkamisajasta|
|Min-toistovälin|15 s||
|Maks-toistovälin|500 päivää||


### <a name="looping-and-debatching-limits"></a>Toistoasetuksia ja debatching rajoitukset

Nämä ovat suorittaa yhteen logiikan sovelluksen rajoitukset.

|Nimi|Raja|Huomautuksia|
|----|----|----|
|ForEach kohteet|5 000|[Kysely-toiminnon](../connectors/connectors-native-query.md) avulla voit suodattaa koko matriisin tarpeen mukaan|
|Iteraatioita asti|10 000||
|SplitOn kohteet|5 000||
|ForEach rinnakkaisuus|20|Voit määrittää peräkkäisiä tarkoitettu kaikille niille lisäämällä `"operationOptions": "Sequential"` , `foreach` toiminto|


### <a name="throughput-limits"></a>Siirtonopeuden rajoitukset

Nämä ovat yhden logiikan app esiintymän rajoitukset. 

|Nimi|Raja|Huomautuksia|
|----|----|----|
|Käynnistimien sekunnissa|100|Työnkulut voivat jakaa käytettävistä useiden sovelluksista tarpeen mukaan|

### <a name="definition-limits"></a>Määritys rajoitukset

Nämä ovat yhden logiikan sovelluksen määrityksen rajoitukset.

|Nimi|Raja|Huomautuksia|
|----|----|----|
|ForEach toiminnot|1|Voit lisätä sisäkkäisiä työnkulkuja, joilla voit laajentaa tämä tarpeen mukaan|
|Kohti työnkulun toiminnot|60|Voit lisätä sisäkkäisiä työnkulkuja, joilla voit laajentaa tämä tarpeen mukaan|
|Sallittu toiminnon upottamalla syvyys|5|Voit lisätä sisäkkäisiä työnkulkuja, joilla voit laajentaa tämä tarpeen mukaan|
|Työnkulut tilauskohtaisten alueittain|1 000||
|Käynnistimien työnkulun kohden|10||
|Lausekkeen merkkien enimmäismäärä|8 192||
|Maks `trackedProperties` koko-merkit|16 000|
|`action`/`trigger`nimi on liian|80||
|`description`pituusrajoituksen|256||
|`parameters`raja|50||
|`outputs`raja|10||

## <a name="configuration"></a>Määritys

### <a name="ip-address"></a>IP-osoite

Soitettua puhelua- [yhdistin](../connectors/apis-list.md) haetaan alla määritetty IP-osoite.

Soitettua puhelua logiikan-sovelluksen suoraan-(eli joko [HTTP](../connectors/connectors-native-http.md) tai [HTTP + Swagger](../connectors/connectors-native-http-swagger.md)) saattaa tulla jokin [Azure-palvelinkeskuksen IP-alueita](https://www.microsoft.com/en-us/download/details.aspx?id=41653).

|Logiikan sovelluksen alue|Lähtevät IP|
|-----|----|
|Australia Itä|40.126.251.213|
|Australia varaaja|40.127.80.34|
|Brasilia Etelä|191.232.38.129|
|Keskitetyn Intia|104.211.98.164|
|Keskitetyn USA|40.122.49.51|
|Itä-Aasian|23.99.116.181|
|Yhdysvaltojen Itä|191.237.41.52|
|Yhdysvaltojen Itä 2|104.208.233.100|
|Japani Itä|40.115.186.96|
|Japani Länsi|40.74.130.77|
|Pohjois-keskitetyn USA|65.52.218.230|
|Pohjois-Eurooppa|104.45.93.9|
|Etelä keskitetyn USA|104.214.70.191|
|Kaakkoisaasialaiset Aasian|13.76.231.68|
|Etelä Intia|104.211.227.225|
|Länsi Europe|40.115.50.13|
|Länsi Intia|104.211.161.203|
|Länsi USA|104.40.51.248|


## <a name="next-steps"></a>Seuraavat vaiheet  

- Aloita logiikan sovellukset, katso [logiikan sovelluksen luominen](app-service-logic-create-a-logic-app.md) -opetusohjelma.  
- [Näytä yleisiä esimerkkejä ja skenaariot](app-service-logic-examples-and-scenarios.md)
- [Voit automatisoida liiketoimintaprosesseja logiikan sovelluksilla](http://channel9.msdn.com/Events/Build/2016/T694) 
- [Opettele järjestelmien integroida logiikan sovellukset](http://channel9.msdn.com/Events/Build/2016/P462)
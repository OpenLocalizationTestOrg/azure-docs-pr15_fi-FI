<properties
    pageTitle="Azure Funktiot Sovelluskehittäjän opas | Microsoft Azure"
    description="Tietoja Azure Funktiot käsitteitä ja osat, jotka löytyvät kaikki kielet ja sidontojen."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure Funktiot, funktioiden, käsittely, webhooks, dynaaminen suorittaminen, serverless arkkitehtuuri"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="05/13/2016"
    ms.author="chrande"/>

# <a name="azure-functions-developer-reference"></a>Azure Funktiot Sovelluskehittäjän opas

Azure Funktiot jakaa muutaman core tekniset käsitteitä ja osien kieltä tai voit käyttää sidonta riippumatta. Ennen kuin siirryt kyselyjä Koulujen tiedot valitsemallasi kielellä tai sidonta, muista lukea tämä yleiskatsaus, joka koskee kaikkia niitä.

Tässä artikkelissa oletetaan, että olet jo Lue [yleisiä tietoja Azure-funktioista](functions-overview.md) ja tuntemiasi [WebJobs SDK-käsitteistä, kuten käynnistimien, sidontojen, ja JobHost runtime](../app-service-web/websites-dotnet-webjobs-sdk.md). Azure Funktiot perustuu WebJobs SDK-paketissa. 


## <a name="function-code"></a>Funktio

*Funktio* on ensisijainen joka kertoo Azure-funktioissa. Funktion koodin kirjoittaminen haluamallasi kielellä ja koodi-tiedostot ja määritystiedosto tallennetaan samaan kansioon. Määritys on JSON ja tiedoston nimi on `function.json`. Eri kielten tuetaan ja jokaisessa on hieman erilainen kokemusta, joka toimii parhaiten kyseisen kielen. 

`function.json` Tiedosto sisältää tietyn funktiolle, mukaan lukien sen sidontojen määritys. Suoritettavan lukee tiedosto ja selvittää, mitkä tapahtumat käynnistettävän käytöstä, mitä tietoja haluat sisällyttää soitettaessa funktio ja mihin haluat lähettää tietoja välitetty pitkin-funktion itse. 

```json
{
    "disabled":false,
    "bindings":[
        // ... bindings here
        {
            "type": "bindingType",
            "direction": "in",
            "name": "myParamName",
            // ... more depending on binding
        }
    ]
}
```

Voit estää suoritettavan toiminnon suorittaminen määrittämällä `disabled` -ominaisuuden arvoksi `true`.

`bindings` -Ominaisuus on, jossa määrittää käynnistimien ja sidontojen. Kunkin sidonta jakaa muutamia yleisiä asetuksia ja joitakin asetuksia, jotka ovat tietyn tyyppisiä sidonta. Jokaisen sidonta edellyttää seuraavia asetuksia:

|Ominaisuus|Arvot ja tyypit|Kommentit|
|---|-----|------|
|`type`|merkkijono|Sidontatyyppi. Esimerkiksi `queueTrigger`.
|`direction`|"in" "out"| Ilmaisee, onko sidonta tietojen vastaanottamiseen funktioon tai lähettämällä funktion tiedot.
| `name` | merkkijono | Nimi, jota käytetään sidotun tietojen-funktiossa. C#, tämä on argumentin; JavaScript on avain/arvoluettelon avain.

## <a name="function-app"></a>Funktio-sovellus

Funktion app koostuu yksittäisiä Funktiot, joita hallitaan yhdessä Azure-sovelluksen palvelu. Kaikki toiminnot funktio-sovelluksessa voit jakaa vastaavan hinnoittelu palvelupaketin, jatkuva käyttöönotto ja suorituksenaikainen versio. Kaikki Funktiot, jotka on kirjoitettu kielellä, jossa on useita jakaa saman funktion sovellus. Ajattele funktio-sovellus voi järjestää ja hallinnoida muunnokset oman funktiot. 

## <a name="runtime-script-host-and-web-host"></a>Runtimen (script host ja web isäntä)

Runtime tai script host on pohjana WebJobs SDK isäntä joka seuraa tapahtumille, kerää ja lähettää tietoja ja suorittaa kädessä koodisi. 

Helpottaa HTTP käynnistimet on myös WWW-isännän joka on suunniteltu äärellä script host tuotannon tilanteissa. Näin eristää script host-web-isäntään hallitsee edusta-liikenne.

## <a name="folder-structure"></a>Kansiorakenne

[AZURE.INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

Kun määrittäminen projektin käyttöönoton Funktiot funktio-sovellukseen, sovelluksen Azure-palvelussa, voit Käsittele tätä kansiorakenne sivuston koodi. Voit käyttää olemassa työkaluja, kuten jatkuva integrointi ja käyttöönotto tai mukautetun käyttöönoton komentosarjoja varten harjoittelu koodiin transpilation tai aika-paketin asennuksen.

>[AZURE.NOTE] `wwwroot` Tähän kansioon on kohtaa, johon tiedostot käyttöön. Kuitenkin kansiota ei täytyy sisältää otetaan käyttöön, tiedostoissa, jotka lopettaa kanssa `wwwroot\wwwroot`. Sen sijaan että `host.json` tiedoston ja funktio kansiot on oltava suoraan otat käyttöön ylimmällä tasolla.

## <a id="fileupdate"></a>Päivittämisestä funktion app-tiedostot

Sisäänrakennettu Azure portaalin funktion editorin avulla voit päivittää *function.json* -tiedosto ja funktion koodi-tiedosto. Voit ladata tai päivittää muita tiedostoja, kuten *package.json* tai *project.json* tai riippuvuudet, sinulla käyttöönoton muilla menetelmillä.

Funktion sovellukset on suunniteltu sovellus-palveluun, jotta kaikki [käytettävissä olevat vakio web Apps-sovellusten Käyttöönottoasetukset](../app-service-web/web-sites-deploy.md) ovat käytettävissä myös funktion sovellusten. Seuraavassa on joitakin tapoja, voit ladata tai päivitä funktion app-tiedostot. 

#### <a name="to-use-app-service-editor"></a>Sovelluksen palvelun editorin käyttäminen

1. Valitse **funktio-sovelluksen asetusten**Azure-Funktiot-portaalissa.

2. Valitse **Lisäasetukset** -osiossa **sovellus palvelun asetukset**.

3. Valitse **Sovelluksen Service editoria** sovellusvalikko siirtymisruudussa **KEHITYSTYÖKALUT**-kohdasta.

4.  Valitse **Siirry**.

    Kun sovellus Service editoria latautuu, näet *wwwroot* *host.json* tiedoston ja funktio kansioita. 

5. Avaa tiedostoja, voit muokata niitä, tai vedä ja pudota kehittäminen tietokoneesta, voit ladata tiedostoja.

#### <a name="to-use-the-function-apps-scm-kudu-endpoint"></a>Voit käyttää funktiota app palvelujen ohjauksen hallinta (Kudu) päätepistettä

1. Siirry: `https://<function_app_name>.scm.azurewebsites.net`.

2. Valitse **Virheenkorjaus konsolin > CMD**.

3. Siirry `D:\home\site\wwwroot\` päivittämään *host.json* tai `D:\home\site\wwwroot\<function_name>` päivittää funktion tiedostot.

4. Vedä ja pudota tiedoston haluat ladata tiedoston ruudukon asianmukaiseen kansioon. Jos poistat tiedoston tiedoston ruudukossa on kaksi aluetta. *.Zip* -tiedostoja näkyviin tulee selite "Vetämällä tähän ladataan ja unzip." Muiden tiedostotyyppien Avaa tiedoston ruudukon, mutta ulkopuolella "unzip"-ruutuun.

#### <a name="to-use-ftp"></a>Jos haluat käyttää FTP

1. Noudata [Tässä](../app-service-web/web-sites-deploy.md#ftp) saat FTP määritetty.

2. Kun yhteys on muodostettu funktion app-sivuston, Kopioi päivitetyt *host.json* -tiedoston `/site/wwwroot` tai funktion tiedostojen kopioiminen `/site/wwwroot/<function_name>`.

#### <a name="to-use-continuous-deployment"></a>Jos haluat käyttää jatkuva käyttöönotto

Noudata ohjeita artikkelissa [Jatkuva Azure-toimintojen käyttöönotto](functions-continuous-deployment.md).

## <a name="parallel-execution"></a>Rinnakkaisia suorittaminen

Useita käynnistävän tapahtumien yhteydessä nopeammin kuin yhden threaded funktion runtime voit käsitellä niitä, suorituksen voi käynnistää useita kertoja rinnakkain-toimintoa.  Jos funktio-sovellus on [Dynaaminen palvelun suunnittelu](functions-scale.md#dynamic-service-plan), funktio sovellus voi skaalata ulos automaattisesti.  Jokaisen esiintymän funktio-sovelluksessa voi onko sovellus toimii dynaaminen palvelun suunnitteleminen tai tavallisen [Sovelluksen-palvelun suunnittelu](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md), käsitellä samanaikainen funktion ohjelmarakennekaaviossa käyttämällä useita viestiketjuissa siirtyminen rinnakkain.  Samanaikainen funktion ohjelmarakennekaaviossa jokaisen funktion app esiintymän enimmäismäärä vaihtelee funktion sovelluksen muistin koon. 

## <a name="azure-functions-pulse"></a>Azure Funktiot Pulse  

Pulse on live tapahtuma-muodossa, jossa näkyy funktion tiheyttä, onnistuu tai epäonnistuu. Voit valvoa keskimääräinen suoritusaika. Lisäämme Lisää toimintoja ja mukauttamisen siihen ajan kuluessa. Voit käyttää **Pulse** -sivun **Seuranta** -välilehti.

## <a name="repositories"></a>Säilöjen tietoihin

Azure-Funktiot koodi on Avaa lähde ja tallennettu GitHub säilöjen tietoihin:

* [Azure Funktiot runtime](https://github.com/Azure/azure-webjobs-sdk-script/)
* [Azure Funktiot-portaalissa](https://github.com/projectkudu/AzureFunctionsPortal)
* [Azure Funktiot-mallit](https://github.com/Azure/azure-webjobs-sdk-templates/)
* [Azure WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/)
* [Azure WebJobs SDK-laajennukset](https://github.com/Azure/azure-webjobs-sdk-extensions/)

## <a name="bindings"></a>Sidontojen

Seuraavassa on kaikki tuetut sidontojen sisällysluettelon.

[AZURE.INCLUDE [dynamic compute](../../includes/functions-bindings.md)]

## <a name="reporting-issues"></a>Raportointiongelmat

[AZURE.INCLUDE [Reporting Issues](../../includes/functions-reporting-issues.md)] 

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja on seuraavissa resursseissa:

* [Azure Funktiot C# Sovelluskehittäjän opas](functions-reference-csharp.md)
* [Azure Funktiot F # Sovelluskehittäjän opas](functions-reference-fsharp.md)
* [Azure Funktiot NodeJS Sovelluskehittäjän opas](functions-reference-node.md)
* [Azure Funktiot käynnistimien ja sidontojen](functions-triggers-bindings.md)
* [Azure-funktioita: matkan](https://blogs.msdn.microsoft.com/appserviceteam/2016/04/27/azure-functions-the-journey/) -Azure App palvelun tiimin blogia. Miten Azure funktiot on suunniteltu historiatiedot.






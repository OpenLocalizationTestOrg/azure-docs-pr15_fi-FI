<properties
   pageTitle="Käsittele raportteja JavaScript-Ohjelmointirajapinnan käyttäminen | Microsoft Azure"
   description="Power BI upotetun, käsittele raportteja JavaScript-Ohjelmointirajapinnan käyttäminen"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="interact-with-power-bi-reports-using-the-javascript-api"></a>Käsittele Power BI-raportteja JavaScript-Ohjelmointirajapinnan käyttäminen

Power BI JavaScript-Ohjelmointirajapinnan avulla voit helposti upottaa sovellustesi Power BI-raportit. API-sovellukset voivat käsitellä eri raportin osia, kuten sivujen ja suodattimet. Tämä vuorovaikutteisuus tekee Power BI-raportit haluat integroida sovelluksen osan.

Power BI-taulukkoraportin upottaminen sovelluksen avulla, joka on iframe sovelluksen osana. Iframe toimii rajan sovelluksesi ja raportin välillä, kuten näet seuraavan kuvan mukaisesti. 

![Power BI upotettu IFRAME-kehyksen ilman Javascript-Ohjelmointirajapinnan](media\powerbi-embedded-interact-with-reports\powerbi-embedded-interact-report-1.png)

IFRAME-kehyksen helpottaa upottamisen prosessin usein, mutta ilman JavaScript-Ohjelmointirajapinnan raportti ja sovelluksen et voi vaikuttaa toisiinsa. Haluat tämän puuttuminen vuorovaikutus tarvitsevasi raportissa ei ole todella sovelluksen osa. Raportti ja sovelluksen on todella kommunikoimiseen edestakaisin, kuten seuraavassa kuvassa.

![Power BI upotettuja iframe Javascript-Ohjelmointirajapinnan kanssa](media\powerbi-embedded-interact-with-reports\powerbi-embedded-interact-report-2.png)

Power BI JavaScript-Ohjelmointirajapinnan avulla voit kirjoittaa tunnus, jolla voit turvallisesti kulkevat iframe-reunaa. Ottaa käyttöön sovelluksen suorittavan toiminnon ohjelmallisesti raportin ja kuunnella toimintoja, jotka käyttäjät tekevät raportin tapahtumia varten.

## <a name="what-can-you-do-with-the-power-bi-javascript-api"></a>Mitä voit tehdä Power BI JavaScript-Ohjelmointirajapinnan kanssa?
JavaScript-Ohjelmointirajapinnan kanssa hallita raportteja, siirry raportin sivujen, suodattaa raportin ja käsitellä upottaminen tapahtumat. Seuraavassa kaaviossa on esitetty Ohjelmointirajapinnan rakenne.

![Power BI JavaScript-Ohjelmointirajapinnan kaavio](media\powerbi-embedded-interact-with-reports\powerbi-embedded-interact-report-3.png)


### <a name="manage-reports"></a>Raporttien hallinta
Javascript-Ohjelmointirajapinnan avulla voit hallita raportin ja sivun tasolla ongelman:

- Upottaa tietyn Power BI raportin suojatusti sovelluksesi - yritä [upottaa sovelluksen esittely](http://azure-samples.github.io/powerbi-angular-client/#/scenario1)
  - Määritä käyttöoikeustietue
- Raportin määrittäminen
  - Ottaminen käyttöön ja poistaminen käytöstä suodatinruudun ja sivusiirtymisruutu - yritä [päivittää asetukset esittely sovelluksen](http://azure-samples.github.io/powerbi-angular-client/#/scenario6)
  - Sivujen ja suodattimet oletusarvojen määrittäminen - yritä [Aseta oletukset-esittely](http://azure-samples.github.io/powerbi-angular-client/#/scenario5)
- Kirjoita ja poistu koko näytön tilasta

[Lisätietoja raportin upottaminen](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embedding-Basics)


### <a name="navigate-to-pages-in-a-report"></a>Siirry raportin sivujen
JavaScript-Ohjelmointirajapinnan enbales saat tietää, että kaikki sivut-raportti ja voit määrittää nykyiselle sivulle. Kokeile [siirtymisruudun esittely sovelluksen](http://azure-samples.github.io/powerbi-angular-client/#/scenario3).

[Lisätietoja sivuissa siirtyminen](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Page-Navigation)

### <a name="filter-a-report"></a>Raportin suodattaminen
JavaScript-Ohjelmointirajapinnan on perus- ja suodatusominaisuudet upotetun raporttien ja raportin sivuja. Kokeile [suodattaminen esittely-sovellus](http://azure-samples.github.io/powerbi-angular-client/#/scenario4)ja tarkista tässä johdanto lisäkoodin.  


#### <a name="basic-filters"></a>Tavallinen suodattimet
Perussuodatin on sijoitettu sarake tai hierarkian tasolla, ja se sisältää arvoluettelon voit lisätä tai poistaa.

```
const basicFilter: pbi.models.IBasicFilter = {
  $schema: "http://powerbi.com/product/schema#basic",
  target: {
    table: "Store",
    column: "Count"
  },
  operator: "In",
  values: [1,2,3,4]
}
```


#### <a name="advanced-filters"></a>Lisäsuodattimet
Lisäsuodattimet Käytä loogisten operaattorien ja tai tai ja Hyväksy yksi tai kaksi ehdot, joissa omia operaattori ja arvo. Tuetut ehdot ovat:

- Ei mitään
- LessThan
- LessThanOrEqual
- GreaterThan
- GreaterThanOrEqual
- Sisältää
- DoesNotContain
- StartsWith
- DoesNotStartWith
- On
- IsNot
- ONTYHJÄ
- IsNotBlank

```
const advancedFilter: pbi.models.IAdvancedFilter = {
  $schema: "http://powerbi.com/product/schema#advanced",
  target: {
    table: "Store",
    column: "Name"
  },
  logicalOperator: "Or",
  conditions: [
    {
      operator: "Contains",
      value: "Wash"
    },
    {
      operator: "Contains",
      value: "Park"
    }
  ]
}
```
[Lisätietoja suodattamisesta](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Filters)


### <a name="handling-events"></a>Tapahtumien käsitteleminen
Lisäksi lähettäminen tiedot iframe-sovelluksesi voi vastaanottaa tiedot tulevat iframe seuraavista tilanteista:

- Upottaminen
  - lataaminen
  - Virhe
- Raportit
  - pageChanged
  - dataSelected (tulossa)

[Lisätietoja tapahtumien käsittely](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Handling-Events)


## <a name="next-steps"></a>Seuraavat vaiheet
Lisätietoja Power BI JavaScript-Ohjelmointirajapinnan Tarkista on seuraavissa linkeissä:

- [JavaScript-Ohjelmointirajapinnan Wiki](https://github.com/Microsoft/PowerBI-JavaScript/wiki)
- [Objektimallin viiteopas](https://microsoft.github.io/powerbi-models/modules/_models_.html)
- Esimerkkejä, joiden
  - [Kulma](http://azure-samples.github.io/powerbi-angular-client)
  - [Ember](https://github.com/Microsoft/powerbi-ember)
- [Live esittely](https://microsoft.github.io/PowerBI-JavaScript/demo/)

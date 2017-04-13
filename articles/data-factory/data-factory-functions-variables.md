<properties 
    pageTitle="Tietoja Factory Funktiot ja Järjestelmämuuttujat | Microsoft Azure" 
    description="Sisältää luettelon Azure Data Factory Funktiot ja Järjestelmämuuttujat" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"
    services="data-factory"
/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/11/2016" 
    ms.author="shlo"/>

# <a name="azure-data-factory---functions-and-system-variables"></a>Azure tietojen Factory - Funktiot ja Järjestelmämuuttujat
Tässä artikkelissa on tietoja funktioita ja Azure Data Factory tukemat muuttujia.
  
## <a name="data-factory-system-variables"></a>Tietoja Factory Järjestelmämuuttujat

Muuttujan nimi | Kuvaus | Objektin laajuus | Vaikutusalueen JSON ja käytä tapauksissa
------------- | ----------- | ------------ | ------------------------
WindowStart | Aikavälin nykyisen tehtävän suorittaminen ikkunan alkuun | toiminta | <ol><li>Määritä tietokyselyjä valinta. Lisätietoja [Tietojen siirtämistä toiminnot](data-factory-data-movement-activities.md) on artikkelissa Viitattu yhdistin.</li><li>Välittää parametreja rakenteen komentosarjan (esimerkki yllä).</li>
WindowEnd | Aikavälin nykyisen tehtävän suorittaminen ikkunan loppuun | toiminta | sama kuin edellä
SliceStart | Tietoja sektoria yhdistämällä aikavälin alkuun | toiminta<br/>tietojoukko | <ol><li>Määritä dynaaminen kansion polut ja tiedostonimet käsitellessäsi [Azure-Blob-objektien](data-factory-azure-blob-connector.md) ja [tiedostojärjestelmän tietojoukkoja](data-factory-onprem-file-system-connector.md).</li><li>Määritä syötteen riippuvuudet tehtävän syötteiden sivustokokoelman tietojen factory-funktioiden kanssa.</li></ol>
SliceEnd | Nykyinen tietojen sektori yhdistämällä aikavälin loppuun | toiminta<br/>tietojoukko | sama kuin edellä. 

> [AZURE.NOTE] Tällä hetkellä tietojen factory edellyttää, että määritetyn tehtävän täsmälleen aikatauluun vastaa tulostus-tietojoukko käytettävyys-parametrissa aikatauluun. Tämä tarkoittaa WindowStart, WindowEnd ja SliceStart ja SliceEnd aina yhdistäminen aikavälillä ja yksi tulos sektoria.
 
## <a name="data-factory-functions"></a>Tietoja Factory-Funktiot 

Voit käyttää funktioita tietojen factory mukana yllä mainittu Järjestelmämuuttujat seuraaviin tarkoituksiin:

1.  Valinnan tietokyselyjä määrittäminen (artikkeleissa yhdistin on artikkelissa [Tietojen siirtämistä toiminnot](data-factory-data-movement-activities.md) viittaavat.

    Käynnistää tietojen factory-funktion syntaksi on: ** $$ ** valinnan tietokyselyjä ja muita ominaisuuksia tehtävän ja tietojoukkoja.  
2. Sivustokokoelman määrittäminen syötteen riippuvuudet tietojen factory funktioiden toimintaa syöttää (katso yllä malli).

    $ ei tarvita argumenteille syötteen riippuvuuden lausekkeita.   

Seuraavassa esimerkissä JSON tiedoston **sqlReaderQuery** -ominaisuus on määritetty **Text.Format** -funktion palauttama arvo. Tässä esimerkissä käytetään myös järjestelmämuuttujan **WindowStart**, joka vastaa tehtävän suorittaminen ikkunan alkamisaika.
    
    {
        "Type": "SqlSource",
        "sqlReaderQuery": "$$Text.Format('SELECT * FROM MyTable WHERE StartTime = \\'{0:yyyyMMdd-HH}\\'', WindowStart)"
    }

### <a name="functions"></a>Funktiot

Seuraavissa taulukoissa luetellaan kaikki Azure Data Factory toiminnot:

Luokka | Funktio | Parametrit | Kuvaus
-------- | -------- | ---------- | ----------- 
Aika | AddHours(X,Y) | X päivämäärä ja aika <br/><br/>Akselin siirtymä: kokonaisluku | Lisää tiettynä aikana X Y tuntia. <br/><br/>Esimerkki: 9/5/2013 12:00:00 PM + 2 tuntia = 9/5/2013 2:00:00 PM
Aika | AddMinutes(X,Y) | X päivämäärä ja aika <br/><br/>Akselin siirtymä: kokonaisluku | Lisää X Y minuuttia.<br/><br/>Esimerkki: 9/15/2013 12:00:00 PM + 15 minuutin = 9/15/2013 12:15:00 PM
Aika | StartOfHour(X) | X päivämäärä ja aika | Saa aloitusaika tuntia vastaavan x tunti-osa. <br/><br/>Esimerkki: StartOfHour 9/15/2013 05:10:23 PM on 9/15/2013 05:00:00 PM
Päivämäärä | AddDays(X,Y) | X päivämäärä ja aika<br/><br/>Akselin siirtymä: kokonaisluku | Lisää X Y päivää.<br/><br/>Esimerkki: 9/15/2013 12:00:00 PM + 2 päivää = 9/17/2013 12:00:00 PM
Päivämäärä | AddMonths(X,Y) | X päivämäärä ja aika<br/><br/>Akselin siirtymä: kokonaisluku | Lisää X Y kuukauden.<br/><br/>Esimerkki: 9/15/2013 12:00:00 PM + 1 kuukausi = 10/15/2013 12:00:00 PM 
Päivämäärä | AddQuarters(X,Y) | X päivämäärä ja aika <br/><br/>Akselin siirtymä: kokonaisluku | Lisää Y * x 3 kuukautta.<br/><br/>Esimerkki: 9/15/2013 12:00:00 PM + 1 vuosineljänneksen = 12/15/2013 12:00:00 PM
Päivämäärä | AddWeeks(X,Y) | X päivämäärä ja aika<br/><br/>Akselin siirtymä: kokonaisluku | Lisää Y * x 7 päivää<br/><br/>Esimerkki: 9/15/2013 12:00:00 PM + 1 viikko = 9/22/2013 12:00:00 PM
Päivämäärä | AddYears(X,Y) | X päivämäärä ja aika<br/><br/>Akselin siirtymä: kokonaisluku | Lisää X Y vuotta.<br/><br/>Esimerkki: 9/15/2013 12:00:00 PM + 1 vuosi = 9/15/2014 12:00:00 PM
Päivämäärä | Day(X) | X päivämäärä ja aika | Saa x päivä-osa.<br/><br/>Esimerkki: Päivän 9/15/2013 12:00:00 PM on 9. 
Päivämäärä | DayOfWeek(X) | X päivämäärä ja aika | Saa päivä, viikko-osan X.<br/><br/>Esimerkki:: N DayOfWeek 9/15/2013 12:00:00 PM on sunnuntai.
Päivämäärä | DayOfYear(X) | X päivämäärä ja aika | Saa x vuosi-osan vuodesta viikonpäivään.<br/><br/>Esimerkkejä:<br/>12/1/2015: päivä 335 2015<br/>12/31/2015: 2015 365 päivän<br/>12/31/2016: päivä 366 2016 (karkausvuosi)
Päivämäärä | DaysInMonth(X) | X päivämäärä ja aika | Saa edustaa kuukauden osan parametrin X kuukauden päivää.<br/><br/>Esimerkki: DaysInMonth 9/15/2013 on 30, koska käsittelyalueella on 30 päivää syyskuu kuukauden.
Päivämäärä | EndOfDay(X) | X päivämäärä ja aika | Hakee päivämäärän ja kellonajan, joka edustaa päivä (päivä-komponentti) x loppuun.<br/><br/>Esimerkki: EndOfDay 9/15/2013 05:10:23 PM on 9/15/2013 11:59:59 PM.
Päivämäärä | EndOfMonth(X) | X päivämäärä ja aika | Saa edustaa kuukauden osan parametrin X kuukauden lopussa. <br/><br/>Esimerkki: EndOfMonth 9/15/2013 05:10:23 PM on 30/9/2013 23:59:59: 00 (päivämäärä aika, joka edustaa kuukauden syyskuu)
Päivämäärä | StartOfDay(X) | X päivämäärä ja aika | Saa päivän vastaavan parametrin X päivä-osan alkuun.<br/><br/>Esimerkki: StartOfDay 9/15/2013 05:10:23 PM on 9/15/2013 12:00:00 AM.
Päivämäärä ja aika | FROM(X) | X-merkkijono | Jäsennä merkkijonon X päivämäärä-aika.
Päivämäärä ja aika | Ticks(X) | X päivämäärä ja aika | Saa jakoviivojen parametrin X-ominaisuus. Yksi ajanjakso on sama kuin 100 nanosekunnin. Tämän ominaisuuden arvo on arvo, joka on keskiyön jälkeen kuluneita 12:00:00, tammikuun 1 0001. 
Teksti | Format(X) | X muuttujan | Muotoilee teksti.

#### <a name="textformat-example"></a>Text.Format Esimerkki

    "defines": { 
        "Year" : "$$Text.Format('{0:yyyy}',WindowStart)",
        "Month" : "$$Text.Format('{0:MM}',WindowStart)",
        "Day" : "$$Text.Format('{0:dd}',WindowStart)",
        "Hour" : "$$Text.Format('{0:hh}',WindowStart)"
    }

Katso [Mukautettu päivämäärä ja aika-Muotoilumerkkijonoa](https://msdn.microsoft.com/library/8kb3ddd4.aspx) aiheeseen, jossa käsitellään erilaisia muotoiluvaihtoehtoja voit käyttää (esimerkiksi: VV ja vvvv). 

> [AZURE.NOTE] Funktion sisällyttäminen toista funktiota käytettäessä sinun ei tarvitse käyttää **$$** etuliite sisä-funktion. Esimerkiksi: $$Text.Format ('PartitionKey eq \\' my_pkey_filter_value\\' ja RowKey sivu \\' {0:yyyy-MM-dd hh}\\", Time.AddHours (SliceStart -6)). Tässä esimerkissä Huomaa, että **$$** etuliite ei käytetä **Time.AddHours** -funktiota. 
  


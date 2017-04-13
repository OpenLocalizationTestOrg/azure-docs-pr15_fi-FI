<properties 
    pageTitle="Käyttämisestä näkyvissä pistemäärä profiilit Azure hakutoiminnossa | Microsoft Azure | Isännöityjen pilvipalvelussa haku" 
    description="Paranna haun kautta näkyvissä profiilit pistemäärä Azure hakutoiminnossa isännöityä cloud search-palvelun käyttöön Microsoft Azure luokitus." 
    services="search" 
    documentationCenter="" 
    authors="HeidiSteen" 
    manager="mblythe" 
    editor=""/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="10/17/2016" 
    ms.author="heidist"/>

# <a name="how-to-use-scoring-profiles-in-azure-search"></a>Tulosmalli profiilit käyttämisestä Azure haku

Tulosmalli profiilit ovat Microsoft Azure haun, mukauttaa haun tulosten laskeminen vaikuttavien, miten kohteiden luokitellut hakutulosluettelon ominaisuus. Voit ajatella näkyvissä profiilit pistemäärä asiayhteyden mallin avulla lisäämällä ennalta määritettyjä ehtoja vastaavat kohteet. Oletetaan esimerkiksi, että sovelluksesi on online hotellista varaus sivusto. Lisäämällä `location` -kentässä, haut, jotka sisältävät esimerkiksi Seattle johtaa suurempiin tulosten kohteiden termi, joka on Seattle `location` kentän. Huomaa, että voit määrittää useita tulosmalli profiilin tai ei mitään, jos oletusarvoinen lasketaan riittää sovelluksen.

Voit ladata avulla voit kokeilla tulosmalli profiilit-sovelluksen malli, joka käyttää tulosmalli profiilit hakutulosten arvon.mukaan järjestyksen muuttaminen. Malli on – eivät välttämättä hyvin realistiset tosielämän sovellusten kehittämisen – Luo konsolisovelluksen, mutta hyötyä malli apuväline. 

Sovelluksen malli esittelee kuvitteellista tiedoista, jota kutsutaan tulosmalli toiminnat `musicstoreindex`. Esimerkki sovelluksen helppokäyttöisyyden on helppo muokata tulosmalli profiilit ja kyselyjen ja nähdä välittömästi vaikutuksista arvon.mukaan tilauksen, kun ohjelma on suoritettu.

<a id="sub-1"></a>
## <a name="prerequisites"></a>Edellytykset

Esimerkkisovellus on kirjoitettu C# Visual Studio 2013: n avulla. Kokeile vapaa [Visual Studio 2013 Express edition](http://www.visualstudio.com/products/visual-studio-express-vs.aspx) , jos sinulla ei vielä ole Visual Studio kopio.

Sinun on Azure tilauksen ja Azure Search-palvelun, viimeistele opetusohjelman. Katso ohjeet palvelun [luominen hakupalvelun portaalissa](search-create-service-portal.md) .

[AZURE. SISÄLLYTÄ [sinun on suoritettava tässä opetusohjelmassa Azure tilin:](../../includes/free-trial-note.md)]

<a id="sub-2"></a>
## <a name="download-the-sample-application"></a>Lataa sovelluksen malli

Siirry [Azure haun näkyvissä pistemäärä profiilit esittely](https://azuresearchscoringprofiles.codeplex.com/) codeplexissä Lataa sovelluksen Tässä opetusohjelmassa kuvataan malli.

Valitse lähdekoodi-välilehdessä voit saada ratkaisun zip-tiedoston **lataaminen** . 

 ![][12]

<a id="sub-3"></a>
## <a name="edit-appconfig"></a>Muokkaa app.config

1. Sen jälkeen, kun olet poiminut tiedostot, Avaa ratkaisun Visual Studiossa Muokkaa määritystiedostoa.
1. Napsauta ratkaisunhallinnassa Kaksoisnapsauta **app.config**. Tämä tiedosto määrittää Palvelupäätepisteen ja `api-key` todennetaan pyynnön. Voit hankkia nämä arvot perinteinen-portaalista.
1. Kirjautuminen [Azure Portal](https://portal.azure.com).
1. Siirry Azure haun palvelun Raporttinäkymät-ikkunan.
1. Napsauttamalla **Ominaisuudet** -ruutua palvelun URL-Osoitteen kopioiminen
1. Voit kopioida **näppäimet** -ruutua `api-key`.

Kun olet lisännyt URL-osoite ja `api-key` app.config, sovellusasetukset pitäisi näyttää tältä:

   ![][11]


<a id="sub-4"></a>
## <a name="explore-the-application"></a>Tutustu sovelluksen

Voit ryhtyä lähes voivat laatia ja suorita sovellus, mutta ennen kuin teet, tutustu JSON tiedostojen luominen ja täytä indeksi.

**Schema.JSON** määrittää hakemiston, mukaan lukien tulosmalli profiilit, jotka ovat ensisijaisia Tässä esittelyssä. Huomaa, että rakenne määritellään kaikki kentät, joiden avulla indeksin, mukaan lukien etsittävän kentät, kuten `margin`, tulosmalli profiilin käytettävissä. Näkyvissä pistemäärä profiilin syntaksi on kuvattu [Lisää Azure-hakuindeksin tulosmalli profiilia](http://msdn.microsoft.com/library/azure/dn798928.aspx).

**Tiedot1 3.json** alkuperäiset tiedot, 246 albumit muutama tyylilajit yli. Tiedot on todellinen albumin ja esittäjän tiedot, kuvitteellista kentillä yhdistelmä `price` ja `margin` kuvataan Etsi-toimintoja. Datatiedostot mukaisia indeksi ja Azure-hakupalvelun ladataan. Kun tiedot on ladattu ja indeksoitu-voit myöntää kyselyitä, jotka perustuvat sitä.

**Program.cs** suorittaa seuraavia toimintoja:

- Avaa konsoli-ikkuna.

- Muodostaa yhteyden Azure hakutuloksia hyödyntämällä palvelun URL-osoite ja `api-key`.

- Poistaa `musicstoreindex` jos se on olemassa.

- Luo uuden `musicstoreindex` schema.json-tiedoston avulla.

- Lisää hakemiston avulla datatiedostot.

- Kyselyn neljä kyselyjen käyttäminen indeksi. Huomaa tulosmalli profiilit arvona kyselyparametri. Kaikki kyselyt Etsi saman termin parhaiten". Ensimmäisen kyselyn näytetään oletusarvoisesti näkyvissä pistemäärä. Jäljellä olevat kolme kyselyt Käytä tulosmalli profiilia.

<a id="sub-5"></a>
## <a name="build-and-run-the-application"></a>Muodosta ja suorita sovellus

Sääntö connectivity tai kokoonpanon viittaus ongelmia, luominen ja varmistaa ongelmia ei ole toimimaan ensin ulos sovelluksen käyttämiseen. Raportissa pitäisi näkyä console-sovelluksen, Avaa taustalla. Kaikki neljä kyselyt suorittaa pitämättä järjestyksessä. Monta järjestelmiin koko ohjelma suorittaa kohdassa 15 sekunnin kuluttua. Jos console-sovellus on viesti, jossa ilmoitetaan "valmiina. Paina enter-näppäintä ja jatka", ohjelma onnistui. 

Vertaileminen kysely suoritetaan, voit Merkitse kopioiminen ja liittäminen kyselytulokset-konsolin ja liitä ne Excel-tiedoston. 

Seuraavassa kuvassa ensimmäinen kolme kyselyt-rinnakkais tulokset. Kyselyjen käytetään hakusana, "parhaiten", jossa näkyy useiden albumien nimet.

   ![][10]

Ensimmäinen kysely käyttää oletusarvoisesti näkyvissä pistemäärä. Koska hakusanoja näkyy vain albumien nimet ja muut ehdot on määritetty, ottaa parhaiten' albumin nimen kohteet palautetaan siinä järjestyksessä, jossa hakupalvelua löytää ne. 

Toisen kyselyn käyttää tulosmalli profiilin, mutta huomaat, että profiilin oli ei vaikuta. Tulokset ovat samat kuin ensimmäisen kyselyn. Tämä johtuu siitä tulosmalli profiilin osa kentän ('Tyylilaji"), joka ei ole germane kyselyyn. Hakusanoja 'parhaalla' ei ole mitään minkä tahansa tiedoston 'Tyylilaji'-kentässä. Kun tulosmalli profiilia ei ole vaikutusta, tulokset ovat samat kuin oletusarvoisesti näkyvissä pistemäärä.  

Kolmas kysely on näkyvissä pistemäärä profiilin vaikutus ensimmäisen osoitus. Hakusanoja parhaiten edelleen '' niin Yritämme albumit samat, mutta koska tulosmalli profiilin sisältää lisää ehtoja, jotka osa 'luokitus' ja 'viimeksi päivitetty', jotkin kohteet ovat liikkuvat suurempi luettelossa.

Seuraavassa kuvassa on esitetty neljännen ja lopullisen kyselyn tehosti 'katteen' mukaan. 'Reunuksen'-kenttä ei tehdä hakuja ja ei voi palauttaa hakutuloksissa. 'Reunuksen' arvon on lisätty manuaalisesti laskentataulukon Ohje kuvaavat hiiren osoitin kohtaan, tietueita, joiden suurempi reunukset näkyvät suurempi hakutulosten luettelosta. 

   ![][9]

Nyt kun olet experimented tulosmalli profiileja, yritä käyttää eri kyselysyntaksia ohjelman vaihtaminen näkyvissä pistemäärä profiilit tai tehokkaan tiedot. Seuraavan osion linkeissä on lisätietoja.

<a id="next-steps"></a>
## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja profiileista näkyvissä pistemäärä. Katso [Lisää Azure-hakuindeksin tulosmalli profiilin](http://msdn.microsoft.com/library/azure/dn798928.aspx) tiedot.

Lisätietoja haun syntaksi ja kyselyn parametrit. [Etsi asiakirjoista (Azure-haun REST-Ohjelmointirajapinta)](http://msdn.microsoft.com/library/azure/dn798927.aspx) lisätietoja.

Tarvitsetko lisätietoja indeksien luominen ja edellinen vaihe? [Tässä videossa](http://channel9.msdn.com/Shows/Cloud+Cover/Cloud-Cover-152-Azure-Search-with-Liam-Cavanagh) ymmärtää perusteet.

<!--Anchors-->
[Prerequisites]: #sub-1
[Download the sample application]: #sub-2
[Edit app.config]: #sub-3
[Explore the application]: #sub-4
[Build and run the application]: #sub-5
[Next steps]: #next-steps

<!--Image references-->
[12]: ./media/search-get-started-scoring-profiles/AzureSearch_CodeplexDownload.PNG
[11]: ./media/search-get-started-scoring-profiles/AzureSearch_Scoring_AppConfig.PNG
[10]: ./media/search-get-started-scoring-profiles/AzureSearch_XLSX1.PNG
[9]: ./media/search-get-started-scoring-profiles/AzureSearch_XLSX2.PNG 
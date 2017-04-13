<properties 
    pageTitle="Tietoja Factory Käyttötapaus - tuotteen suositukset" 
    description="Lisätietoja toteutettu käyttämällä Azure Data Factory yhdessä muiden palveluiden käyttötapaus." 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/01/2016" 
    ms.author="shlo"/>

# <a name="use-case---product-recommendations"></a>Käyttötapaus - tuotteen suositukset 

Azure Data Factory on monia ratkaisu pikatoimintojen Cortana Liiketoimintatieto-ohjelmistopaketin käyttää palveluita.  Katso [Cortanan liiketoimintatietojen Suite](http://www.microsoft.com/cortanaanalytics) sivun lisätietoja tämän ohjelmiston. Tässä asiakirjassa on kuvaavat yleisiä käyttötapaus, Azure käyttäjillä on jo ratkaistu ja käyttämällä Azure Data Factory ja muut Cortana Liiketoimintatieto-Komponenttipalvelut.

## <a name="scenario"></a>Skenaario

Online jälleenmyyjät haluat yleisesti houkuttelemaan asiakkaidensa ostaa tuotteita, jos ne ovat todennäköisesti kiinnostunut ja näin ollen todennäköisesti ostaminen tuotteiden kanssa. Tämän vuoksi online jälleenmyyjät on mukauttaa niiden käyttäjän käyttökokemuksesi käyttämällä mukautettuja tuotteen suosituksia tietyn käyttäjän. Mukautetut suositukset on tehtävä perustuvat nykyisen ja historiallisten ostokset toiminta, tuotetiedot, uusien merkkien ja tuote- ja Segmentointi tiedot.  Lisäksi he voivat kirjoittaa yleinen käyttö toiminta kaikkien niiden käyttäjien yhdistää analyysiin perustuvat käyttäjän tuotteen suositukset.

Nämä jälleenmyyjät tavoite on käyttäjän napsauttamalla myyntiä muunnokset optimointi ja ansaita suurempi myyntituotto.  Ne saavuttamiseksi tämä muuntaminen toimittamalla tilannekohtaiset ja toiminta suosituksia perustuu asiakkaan etu- ja toiminnot. Käytä tässä tapauksessa Käytämme online jälleenmyyjät esimerkkinä yritykset, joiden haluat optimoida asiakkaidensa. Kuitenkin näiden periaatteiden koskevat yritysten, jotka haluaa osallistuminen ympärille sen tuotteiden ja palvelujen asiakkaiden ja parantaa asiakkaidensa mukautettuja tuotteen suosituksia ostavaa käyttökokemuksen.

## <a name="challenges"></a>Haasteita

On monia haasteita kyseisen online jälleenmyyjät hymiö yritettäessä Toteuta tällaista käyttötapaus. 

Ensin erikokoisia ja muotojen tietojen on oltava nautittuina useista tietolähteistä, sekä paikallisen ja pilveen. Näitä tietoja ovat tuotetietojen historiallisten toiminta asiakastietoja ja käyttäjätiedot, kun käyttäjä avaa verkkokauppa-sivustossa. 

Toinen, mukautetut tuotteen suosituksia oltava tarkasti ja kohtuudella lasketaan ja ennustettujen. Tuotteen, brändiä ja asiakastietoja toiminta ja selaimen lisäksi online jälleenmyyjät on myös lisättävä asiakaspalautteen edellisten ostot, voit ottaa huomioon parhaat tuotteen suositukset, käyttäjän määrittämiseksi. 

Kolmas suositukset on oltava heti toimituksen saumaton selaamisesta ja ostamalla kokemus ja anna useimmat uusimmat ja käyttökelpoiset suositukset käyttäjälle. 

Lopuksi jälleenmyyjät on mitata niiden lähestymistapa tehokkuutta seuraamalla yleinen ylös-myynti ja rajat-Tilausasiakas napsauttamalla muuntaminen myynnin onnistuu ja muuttaa niiden tulevien suosituksia.

## <a name="solution-overview"></a>Ratkaisun yleiskatsaus

Tässä esimerkissä käyttötapaus on ratkaistu ja toteuttaa todellisia Azure käyttäjien Azure Data Factory ja muut Cortana Liiketoimintatieto-Komponenttipalvelut, mukaan lukien [HDInsight](https://azure.microsoft.com/services/hdinsight/) ja [Power BI](https://powerbi.microsoft.com/).

Online jälleenmyyjältä käyttää Azure-Blob-säilö, paikallisen SQL Serveriä, Azure SQL-DB ja relaatiotietoja mart niiden tietojen tallennusasetuksia työnkulun koko.  Blob-säilö sisältää asiakastietoja, toiminta asiakastietoja ja tuotteen tietojen. Tuotteen tietojen sisältää brändiä tuotetiedot ja tuotteen luettelon tallennettu paikallisesti SQL-tietovarasto. 

Kaikki tiedot on yhdistetty ja tuotteen suositus järjestelmän aikana perustuu asiakkaan etu- ja toiminnot, samalla, kun käyttäjä avaa tuotteista on sivustossa mukautettuja suosituksia syötetään. Asiakkaiden on myös tuotteita, jotka liittyvät hän katsoo tuotteen perusteella yleinen sivuston käyttötavat, jotka eivät ole mitään yhdelle käyttäjälle.

![käyttötapauskaavio](./media/data-factory-product-reco-usecase/diagram-1.png)

Gigatavua raaka web lokitiedostoja luodaan päivittäin online jälleenmyyjältä sivustosta puolirakenteisia-tiedostoina. Raaka web-lokitiedostojen ja asiakas- ja tuote-luettelotietojen nautitaan kyselyjä Azure-Blob-säiliö, käyttämällä Data Factory yleisesti käyttöön tietojen siirto palveluna säännöllisesti. Raaka lokitiedostojen päivän osioitu (vuoden ja kuukauden mukaan) Blob-objektien tallennustilaan pitkään tallennuksessa.  [Azure Hdinsightiin](https://azure.microsoft.com/services/hdinsight/) käytetään osion blob-säilön raaka lokitiedostot ja käsitellä nieltäväksi lokit tasolla rakenne ja Possu komentosarjojen avulla. Osioitu web kirjaa tietoja käsitellään sitten Pura tarvittavat syötteiden Koulujen suositus järjestelmä luo mukautettuja tuotteen suositukset tietokoneelle.

Suositus, jota käytetään tässä esimerkissä Koulujen koneen on suositus platform- [Apache Mahout](http://mahout.apache.org/)Koulujen Avaa lähde-koneen.  Mikä tahansa [Azure koneen Learning](https://azure.microsoft.com/services/machine-learning/) tai mukautetun mallin voi suojata skenaariota.  Mahout mallin käytetään ennustaa kohteiden perusteella yleinen käyttötavat sivustossa samankaltaiset ja luo mukautettuja suositukset tietylle käyttäjälle perusteella.

Lopuksi mukautettuja tuotteen suosituksia tulosjoukon siirretään relaatiotietoja mart kulutus jälleenmyyjältä sivuston.  Tulosjoukko voi myös käyttää suoraan Blob-objektien tallennustilaan toisessa sovelluksessa tai siirtää muiden kuluttajille ja käytä tapauksissa Lisää stores.

## <a name="benefits"></a>Edut

Optimointi niiden tuotteen suositus strategia ja tasaaminen ja tavoitteet, ratkaisun täyttää online jälleenmyyjältä kaupallisen ja markkinoinnin tavoitteet. Lisäksi siirretyt operationalize ja hallita tehokkaasti, luotettava ja edullinen tuotteen suositus työnkulun. Lähestymistapa tekeminen on helppoa niiden päivittää niiden mallin ja tehdä sen tehokkuuden myynnin napsauttamalla muuntaminen onnistuu toimenpiteet perusteella. Käyttämällä Azure Data Factory siirretyt hylkää niiden aikaa ja kallista manuaalinen cloud Resurssienhallinta ja siirry tarvittaessa cloud Resurssienhallinta. Tämän vuoksi siirretyt voit säästää aikaa, rahaa, ja vähentää ajan ratkaisun käyttöönotto. Tietonäkymien hierarkiatasolla ja toiminnallisia palvelun kunto tuli helposti visualisointi ja vianmääritys ja intuitiivinen Data Factory valvonnan ja hallinnan Käyttöliittymän käytettävissä Azure-portaalista. Ratkaisun voi nyt ajoitettu ja hallitaan niin, että valmis tiedot luotettavasti valmistettu ja toimittaminen käyttäjille ja tietojen ja käsittelyn riippuvuudet hallitaan automaattisesti ilman ihmisen.

Antamalla tämän ostoskoriin Käyttömukavuuden luotu lisää kilpailua, mielenkiintoisempia asiakkaan online jälleenmyyjältä kohdata ja parantaa näin ollen myynti- ja Yleinen-asiakkaiden tyytyväisyyttä.



  
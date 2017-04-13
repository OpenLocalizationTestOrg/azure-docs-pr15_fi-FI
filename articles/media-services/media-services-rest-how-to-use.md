<properties 
    pageTitle="Media Services REST API yleiskatsaus | Microsoft Azure" 
    description="Media Services REST API yleiskatsaus" 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="10/12/2016"
    ms.author="juliako"/>


# <a name="media-services-rest-api-overview"></a>Media Services REST API yleiskatsaus 

[AZURE.INCLUDE [media-services-selector-setup](../../includes/media-services-selector-setup.md)]

Microsoft Azure Media Services on palvelu, jota voidaan käyttää OData-pohjainen HTTP-pyynnöt ja vastata siihen takaisin sisään yksityiskohtainen JSON tai atom + kirja. Koska Azure rakenne ohjeiden mukainen Media Services, on joukko tarvittavat HTTP-otsikot, jotka kukin asiakas on käytettävä Media Services yhdistettäessä sekä joukon valinnaisen otsikon, jonka avulla voidaan. Seuraavissa kohdissa kuvataan otsikot ja HTTP verbien avulla voit luominen pyynnöt ja vastaukset vastaanottamisen Media-palveluita.

##<a name="considerations"></a>Huomioon otettavia seikkoja 

Kun käytät muiden ottaa huomioon seuraavat asiat.

- Kun kyselyt kohteita ei palauttaa yhtä aikaa, koska julkisen muiden v2 rajoittaa kyselyn tuloksia 1000 tulokset 1000 kohteiden enimmäismäärä. Sinun on käytettävä **Ohita** ja **toteuttaa** (.NET) / **ylös** (REST) muodossa, joka on kuvattu [.NET tässä esimerkissä](media-services-dotnet-manage-entities.md#enumerating-through-large-collections-of-entities) ja [REST API tässä esimerkissä](media-services-rest-manage-entities.md#enumerating-through-large-collections-of-entities). 

- Kun käyttämällä JSON ja määrittämällä **__metadata** avainsanojen käyttäminen pyynnön (esimerkiksi viittaa linkitetyn objektin) on määritettävä **Accept** -otsikon [JSON yksityiskohtainen](http://www.odata.org/documentation/odata-version-3-0/json-verbose-format/) muotoon (katso seuraava esimerkki). OData ymmärrä pyynnön **__metadata** -ominaisuuden avulla yksityiskohtainen valittu.  

        POST https://media.windows.net/API/Jobs HTTP/1.1
        Content-Type: application/json;odata=verbose
        Accept: application/json;odata=verbose
        DataServiceVersion: 3.0
        MaxDataServiceVersion: 3.0
        x-ms-version: 2.11
        Authorization: Bearer <token> 
        Host: media.windows.net
        
        {
            "Name" : "NewTestJob", 
            "InputMediaAssets" : 
                [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Aba5356eb-30ff-4dc6-9e5a-41e4223540e7')"}}]
        . . . 
        

## <a name="standard-http-request-headers-supported-by-media-services"></a>Vakio HTTP-pyynnön otsikot Media-palveluissa tuetut

Jokaisen kutsun, voit tehdä Media Services on joukko tarvittavat otsikot, jotka on lisättävä puhelinnumeroon ja myös valinnaisen ylä joukko saattaa sisällytettävien. Seuraavassa taulukossa on lueteltu tarvittavat otsikot:


Ylätunniste|Tyyppi|Arvo
---|---|---
Todennus|Haltijan|Haltijan on vain hyväksytyssä luvan järjestelmä. Arvon on oltava myös ACS myöntämä käyttöoikeustietue.
x-ms-versio|Desimaaliluku|2.11
DataServiceVersion|Desimaaliluku|3.0
MaxDataServiceVersion|Desimaaliluku|3.0



>[AZURE.NOTE] Koska Media Services näyttää sen pohjana metatietojen resurssi-säilöön, REST API kautta käyttämällä OData-pyyntö; sisällytetään DataServiceVersion ja MaxDataServiceVersion otsikot Jos ne eivät ole, sitten tällä hetkellä Media Services oletetaan, käytä DataServiceVersion arvo on 3.0.

Seuraavassa on valinnainen otsikot joukko:

Ylätunniste|Tyyppi|Arvo
---|---|---
Päivämäärä|RFC 1123 päivämäärä|Pyynnön aikaleima
Hyväksy|Sisältötyyppi|Pyydetty sisältötyyppi vastauksen esimerkiksi seuraavasti:<p> -sovelluksen/json; odata = yksityiskohtainen<p> -sovelluksen/atom + xml<p> Vastaukset voi olla eri sisältötyypin, kuten blob-Nouda, jossa onnistuneen vastaus sisältää tietoina blob-muodossa.
Hyväksy-koodaus|GZip-Kovera|GZIP ja DEFLATE koodaus, kun se on käytettävissä. Huomautus: Suuri resurssien Media Services voi ohittaa tämän otsikon ja noncompressed tietojen palauttaminen.
Hyväksy kieli|"FI" ja "-es".|Määrittää vastauksen ensisijainen kieli.
Hyväksy merkistö|Merkistö-tyypin, kuten "UTF-8"|Oletusarvo on UTF-8.
X-HTTP-menetelmä|HTTP-menetelmä|Sallii asiakkaiden tai palomuurit, jotka eivät tue HTTP-menetelmiä, kuten käyttöön tai poista nämä eri tavalla, tunneloidun GET puhelun kautta.
Sisältötyyppi|Sisältötyyppi|Sisältötyypin pyynnön tekstiosa HYLLYTETTY tai POST pyytää.
asiakkaan pyynnön tuotetunnus-|Merkkijono|Soittajan määritetty arvo, joka määrittää tietyn pyynnön. Jos määritetty, tämä arvo sisällytetään vastausviestissä avulla pyynnön. <p><p>**Tärkeää**<p>Arvojen enimmillään 2096b (2k).

## <a name="standard-http-response-headers-supported-by-media-services"></a>Vakio HTTP-vastauksen otsikot Media-palveluissa tuetut

Seuraavassa taulukossa on otsikot, jotka voidaan palauttaa sinulle on pyytää resurssin ja haluat suorittaa toiminnon mukaan joukko.


Ylätunniste|Tyyppi|Arvo
---|---|---
pyynnön tunnus|Merkkijono|Nykyinen toiminto palvelun luotu yksilöllinen.
asiakkaan pyynnön tuotetunnus-|Merkkijono|Tunniste määrittämää alkuperäinen-pyynnössä soittajan, jos se on valittavissa.
Päivämäärä|RFC 1123 päivämäärä|Päivämäärä, jolloin pyynnön käsiteltiin.
Sisältötyyppi|Vaihtelee|Vastauksen teksti sisältötyyppi.
Sisällön koodaus|Vaihtelee|GZip tai Kovera tarvittaessa.


## <a name="standard-http-verbs-supported-by-media-services"></a>Media-palveluissa tuetut HTTP perustoiminnot

Seuraavassa on luettelo kaikista HTTP verbien, jonka avulla voidaan tehdä HTTP pyytää:


Toiminto|Kuvaus
---|---
HAE|Palauttaa nykyisen arvon objektin.
KIRJAA|Luo annettujen tietojen perusteella objektin tai lähettää komennon.
VALITSEMINEN|Korvaa objektin, tai Luo nimetty objektin (tarvittaessa).
POISTA|Poistaa objektin.
YHDISTÄMINEN|Päivittää objektin nimetty ominaisuuden muutokset.
PÄÄ|Palauttaa objektin, HAE vastausta metatiedot.

##<a name="limitation"></a>Rajoitus

Kun kyselyt kohteita ei palauttaa yhtä aikaa, koska julkisen muiden v2 rajoittaa kyselyn tuloksia 1000 tulokset 1000 kohteiden enimmäismäärä. Sinun on käytettävä **Ohita** ja **toteuttaa** (.NET) / **ylös** (REST) muodossa, joka on kuvattu [.NET tässä esimerkissä](media-services-dotnet-manage-entities.md#enumerating-through-large-collections-of-entities) ja [REST API tässä esimerkissä](media-services-rest-manage-entities.md#enumerating-through-large-collections-of-entities). 


## <a name="discovering-media-services-model"></a>Media-palveluiden mallin etsiminen

Kirjastonäkymiä Media Services kohteiden enemmän, voidaan $metadata-toimintoa. Sen avulla voit hakea kaikki kelvollinen kohde tiedostotyypit, kohteen ominaisuudet, yhteydet, toiminnot, toiminnot ja niin edelleen. Seuraavassa esimerkissä esitetään, miten voit käyttää URI: https://media.windows.net/API/$ metatiedot.

Olisi liittäminen "? api version=2.x" URI-tunnus, jos haluat tarkastella metatiedot selaimessa tai Älä sisällytä x-ms-version-ylätunnisteen pyyntö loppuun.



##<a name="media-services-learning-paths"></a>Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]





 

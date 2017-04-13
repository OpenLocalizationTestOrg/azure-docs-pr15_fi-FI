<properties
    pageTitle="Istunnon affiniteetti Azure-työkalujen käyttäminen Pimennys ottaminen käyttöön"
    description="Lue, miten istunnon affiniteetti Azure-työkalujen käyttäminen Pimennys käyttöön."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690950.aspx -->

# <a name="enable-session-affinity"></a>Istunnon affiniteetti ottaminen käyttöön #

Sisällä varten Pimennys Azure-Työkalut voit ottaa HTTP-istunnon affiniteetti tai "muistilappuja istunnot", että roolien. Seuraava kuva esittää valintaikkunan istunnon affiniteetti-toiminnon käyttöön ottaminen käytettäviä **Kuormituksen tasaamisen** ominaisuuksia:

![][ic719492]

## <a name="to-enable-session-affinity-for-your-role"></a>Jos haluat ottaa käyttöön istunnon affiniteetin tehtäväsi ##

1. Pimennys 's Project Explorer-roolin hiiren kakkospainikkeella, valitse **Azure**ja valitse sitten **Kuormituksen tasaamisen**.
1. **WorkerRole1 kuormituksen tasaamisen ominaisuudet** -valintaikkunassa:
    1. Tarkista **Enable HTTP istunnon affiniteetti (muistilappuja istuntoa) tämän roolin.**
    1. **Syötteen päätepisteen käyttämään**Valitse syötteen päätepisteen, jos haluat käyttää, esimerkiksi **http (julkinen: 80, yksityinen: 8080)**. Sovellus on käytettävä tämän päätepisteen kuin sen HTTP-päätepisteen. Voit ottaa käyttöön useita päätepisteet roolin, mutta voit valita jonkin niistä tukemaan muistilappuja istunnot.
    1. Muodosta sovelluksesi uudelleen.

Jos käytössä, jos sinulla on useampi kuin yksi rooli esiintymä-pyyntöjen tulevat tietyn asiakkaan säilyvät parhaillaan käsittelemien roolin samassa esiintymässä.

Pimennys-työkalujen avulla tämän asentamalla määräten IIS-moduuli kutsutaan sovellus pyytää reititys (ARR) jokaiseen Roolin esiintymät. ARR reititetään uudelleen pyyntöjen asianmukainen rooli-esiintymässä. Työkalujen telakoit valitun päätepisteen automaattisesti niin, että HTTP saapuvan liikenteen reititetään ensin ARR-ohjelmisto. Työkalujen myös Luo uusi sisäinen päätepiste, jotka Java-palvelin on määritetty kuunnella. Joka on käyttää ARR uudelleenreititykseen HTTP-liikenne paikalliseen asianmukainen rooli esiintymän päätepiste. Näin monen esiintymän käyttöönoton roolin jokaiselle esiintymälle on käänteisen välityspalvelimen kaikkien muiden esiintymien ottaminen käyttöön alas jäävät istuntoja.

## <a name="notes-about-session-affinity"></a>Istunnon affiniteetti huomautuksia ##

* Istunnon affiniteetti ei toimi Laske emulaattori. Asetuksia voi käyttää Laske emulaattori ilman häiritse muodostusprosessin tai laskea emulaattorin suorittamisen, mutta itse ominaisuus ei toimi Laske emulaattori.
* Istunnon affiniteetti ottaminen käyttöön johtaa kasvua rahamäärän levytilan mukaan Azure-tietokannassa, käyttöönoton lisäohjelmistoa ladattu ja asennettu roolin esiintymät käynnistyessään palvelun Azure pilveen.
* Aika alustaa rooleille kestää kauemmin.
* Sisäinen päätepisteen toimii liikenne rerouter edellä mainittua on added.ss

Esimerkki ylläpitoa istunnon tiedot, kun istunnon affiniteetti on otettu käyttöön Katso, [miten voit säilyttää istunnon tietoja istunnon affiniteetti][].

## <a name="see-also"></a>Katso myös ##

[Azure työkalujen Pimennys varten][]

[Pimennys Azure Hei maailma-sovelluksen luominen][]

[Azure-työkalut asennetaan Pimennys][] 

[Istunnon tiedot istunnon affiniteetti ja ylläpitäminen][]

Saat lisätietoja Azure käyttäminen Java [Azure Java Developer Center][].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure työkalujen Pimennys varten]: http://go.microsoft.com/fwlink/?LinkID=699529
[Pimennys Azure Hei maailma-sovelluksen luominen]: http://go.microsoft.com/fwlink/?LinkID=699533
[Istunnon tiedot istunnon affiniteetti ja ylläpitäminen]: http://go.microsoft.com/fwlink/?LinkID=699539
[Azure-työkalut asennetaan Pimennys]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic719492]: ./media/azure-toolkit-for-eclipse-enable-session-affinity/ic719492.png

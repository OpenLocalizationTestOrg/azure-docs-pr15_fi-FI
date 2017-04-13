<properties 
    pageTitle="Virta Analytics julkaisutiedot | Microsoft Azure" 
    description="Virta Analytics julkaisutiedot" 
    services="stream-analytics" 
    documentationCenter="" 
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>

#<a name="stream-analytics-release-notes"></a>Virta Analytics julkaisutiedot

## <a name="notes-for-04152016-release-of-stream-analytics"></a>Muistiinpanojen Stream Analytics 04/15/2016-versiossa ##

Tämä julkaisu sisältää seuraavan päivityksen.

Otsikko | Kuvaus
---|---
Yleiseen käyttöön Power BI tulostaa  | [Power BI tulostaa](stream-analytics-power-bi-dashboard.md) ovat nyt yleisesti saatavilla. 90 päivän luvan vanhenemista Power BI on poistettu. Lisätietoja tilanteissa, joissa todennus on uusia luominen Power BI-koontinäytön kohdassa [Uusi luvan](stream-analytics-power-bi-dashboard.md#Renew-authorization) .

## <a name="notes-for-03032016-release-of-stream-analytics"></a>Muistiinpanojen Stream Analytics 03/03/2016-versiossa ##

Tämä julkaisu sisältää seuraavan päivityksen.

Otsikko | Kuvaus
---|---
Uudet Stream Analytics-kyselykieltä kohteet  | SAQL on nyt [GetType](https://msdn.microsoft.com/library/azure/mt643890.aspx "GetType MSDN-sivun"), [TRY_CAST](https://msdn.microsoft.com/library/azure/mt643735.aspx "TRY_CAST MSDN-sivulle") ja [REGEXMATCH](https://msdn.microsoft.com/library/azure/mt643891.aspx "REGEXMATCH MSDN-sivu").

## <a name="notes-for-12102015-release-of-stream-analytics"></a>Muistiinpanojen 12/10/2015 Stream Analytics-versiossa ##

Tämä julkaisu sisältää seuraavan päivityksen.

Otsikko | Kuvaus
---|---
REST API Versiopäivitys | REST API-versio on päivitetty 2015-10-01. Tiedot löytyvät [Stream Analytics hallinta REST API-viittaus](https://msdn.microsoft.com/library/azure/dn835031.aspx) ja [tietokoneen Learning integrointi Stream Analytics](stream-analytics-how-to-configure-azure-machine-learning-endpoints-in-stream-analytics.md)MSDN-sivuston.
Azure koneen Learning integrointi | Tässä versiossa on tuki Azure Konepohjaisten Oppimistekniikoiden käyttäjän määrittämät funktiot. Katso lisätietoja sekä [Yleiset blogin ilmoituksen](http://blogs.technet.com/b/machinelearning/archive/2015/12/10/apply-azure-ml-as-a-function-in-azure-stream-analytics.aspx) [opetusohjelma](stream-analytics-machine-learning-integration-tutorial.md) .

## <a name="notes-for-11122015-release-of-stream-analytics"></a>Muistiinpanojen 11/12/2015 Stream Analytics-versiossa ##

Tämä julkaisu sisältää seuraavan päivityksen.

Otsikko | Kuvaus
---|---
Valitse uusi toiminta | Valitse Stream Analytics on laajennettu, jotta * ominaisuus-seuraaja sisäkkäisiä tietueen nimellä. Lisätietoja Ota yhteyttä [http://msdn.microsoft.com/library/mt622759.aspx](http://msdn.microsoft.com/library/mt622759.aspx "Monimutkaisten tietotyyppien").

## <a name="notes-for-10222015-release-of-stream-analytics"></a>Muistiinpanojen 10/22/2015 Stream Analytics-versiossa ##

Tämä julkaisu sisältää seuraavat päivitykset.

Otsikko | Kuvaus
---|---
Kyselyn kieliominaisuudet | Virta Analytics on laajennettu kyselykielen sisällyttämällä seuraavat ominaisuudet: [itseisarvo](https://msdn.microsoft.com/library/azure/mt574054.aspx), [PYÖRISTÄ.KERR.ylös](https://msdn.microsoft.com/library/azure/mt605286.aspx), [eksponentti](https://msdn.microsoft.com/library/azure/mt605289.aspx), [FLOOR](https://msdn.microsoft.com/library/azure/mt605240.aspx), [POWER](https://msdn.microsoft.com/library/azure/mt605287.aspx), [merkki](https://msdn.microsoft.com/library/azure/mt605290.aspx), [neliö](https://msdn.microsoft.com/library/azure/mt605288.aspx)ja [neliöjuuri](https://msdn.microsoft.com/library/azure/mt605238.aspx).
Poistaa kooste rajoitukset  | Tässä versiossa poistaa 15 koosteet rajoittamista kyselyn. Ei rajaa määrään koosteet kysely on nyt.
Lisätty ryhmän System.Timestamp-ominaisuus | [GROUP BY](https://msdn.microsoft.com/library/azure/dn835023.aspx) -funktion avulla nyt window_type tai [System.Timestamp](https://msdn.microsoft.com/library/azure/mt598501.aspx).
Lisätty siirtymä Tumbling ja Hopping windows | [Tumbling](https://msdn.microsoft.com/library/azure/dn835055.aspx) ja [Hopping](https://msdn.microsoft.com/library/azure/dn835041.aspx) windows tasataan vastaan nolla aika (1/1/0001 12:00:00 AM UTC-ajan). Uusi (valinnainen) parametrin 'offsetsize' avulla määrittämällä mukautetun Siirtymä (tai tasaus).


## <a name="notes-for-09292015-release-of-stream-analytics"></a>Muistiinpanojen 09/29/2015 Stream Analytics-versiossa ##

Tämä julkaisu sisältää seuraavat päivitykset.

Otsikko | Kuvaus
---|---
Azure IoT Suite julkisen esikatselu | Virta Analytics sisältyy Public Preview Azure IoT-ohjelmiston.
Azure Portal-integrointi | Lisäksi jatkuvan tavoitettavuustietojen Azure hallinta-portaalissa Stream Analytics on nyt integroitu [Azure-portaalissa](https://azure.microsoft.com/overview/preview-portal/). Huomaa, että Stream Analytics esikatselu portal-toiminto on tällä hetkellä alijoukkoa toimintoja tarjota Azure hallinta-portaalissa ei ole tukea selaimessa kyselyn testikäyttöön Power BI tulosteen määritys- ja selaamalla tai luomalla uuden syötteen ja tulosteen resurssien tilaukset, joihin sinulla on pääsy.
DocumentDB tulostus-tuki | Virta Analytics työt voi nyt siirtää [DocumentDB](https://azure.microsoft.com/services/documentdb/).
IoT keskittimeen IME-tuki | Virta Analytics työt voidaan nyt ingest IoT keskittimet tiedot.
AIKALEIMA mukaan erilaisten tapahtumille | Kun yksi tietovirta sisältää useita tapahtuman tyyppejä aikaleimat tallentamisessa eri kenttiä, nyt voit [AIKALEIMA mukaan](http://msdn.microsoft.com/library/mt573293.aspx) lausekkeiden kanssa voit määrittää eri aikaleimakenttien kaikissa tapauksissa.

## <a name="notes-for-09102015-release-of-stream-analytics"></a>Muistiinpanojen 09/10/2015 Stream Analytics-versiossa ##

Tämä julkaisu sisältää seuraavat päivitykset.

Otsikko|Kuvaus
---|---
PowerBI ryhmät tuki|Jotta tietoja voidaan jakaa muiden Power BI-käyttäjien kanssa, Stream Analytics työt nyt kirjoittaa [PowerBI](stream-analytics-define-outputs.md#power-bi) ryhmiin sisällä Power BI-tilillesi.

## <a name="notes-for-08202015-release-of-stream-analytics"></a>Muistiinpanojen 08/20/2015 Stream Analytics-versiossa ##

Tämä julkaisu sisältää seuraavat päivitykset.

Otsikko|Kuvaus
---|---
Lisätty viimeisin toiminto |[Viimeisin](http://msdn.microsoft.com/library/mt421186.aspx) toiminto on nyt saatavilla Stream Analytics töitä, voit hakea viimeisimmän tapahtuman tapahtuma-tietovirta tietyn aikajakson sisällä.
Uusi taulukko-Funktiot|Matriisi-funktioita [GetArrayElement](http://msdn.microsoft.com/library/mt270218.aspx), [GetArrayElements](http://msdn.microsoft.com/library/mt298451.aspx) ja [GetArrayLength](http://msdn.microsoft.com/library/mt270226.aspx) ovat nyt käytettävissä.
Uusi tietue-Funktiot|Tietueen toiminnot [GetRecordProperties](http://msdn.microsoft.com/library/mt270221.aspx) ja [GetRecordPropertyValue](http://msdn.microsoft.com/library/mt270220.aspx) ovat nyt käytettävissä.

## <a name="notes-for-07302015-release-of-stream-analytics"></a>Muistiinpanojen 07/30/2015 Stream Analytics-versiossa ##

Tämä julkaisu sisältää seuraavat päivitykset.

Otsikko|Kuvaus
---|---
Power BI-organisaatiotunnus erillisen Azure tunnuksesta|Tämän ominaisuuden avulla [Power BI-tulostus](stream-analytics-power-bi-dashboard.md) ASA töiden minkä tahansa Azure tilin tyyppi (Live ID-tunnus tai organisaatiotunnus)-kohdassa. Voit lisäksi on yksi organisaatiotunnus Azure-tilin ja käyttää uudelleenkäyttöä varten sallimisesta Power BI-tuloste.
Palvelun Bus olevien tulosteen tuki|[Palvelun Bus olevien](stream-analytics-define-outputs.md#service-bus-queues) tulostaa ovat nyt saatavilla Stream Analytics työt.
Palvelun Bus aiheet tulosteen tuki|[Palvelun Bus aiheet](stream-analytics-define-outputs.md#service-bus-topics) tulostaa ovat nyt saatavilla Stream Analytics työt.

## <a name="notes-for-07092015-release-of-stream-analytics"></a>Muistiinpanojen 07/09/2015 Stream Analytics-versiossa ##

Tämä julkaisu sisältää seuraavat päivitykset.


Otsikko|Kuvaus
---|---
Mukautettu Blob tulosteen jakaminen|BLOB storage tulostaa Salli vaihtoehto, joka määrittää kuinka usein nyt, että tulostus BLOB kirjoitetaan ja rakenteen ja tulosteen polku kansiorakenne muoto. 

## <a name="notes-for-05032015-release-of-stream-analytics"></a>Muistiinpanojen 05/03/2015 Stream Analytics-versiossa ##

Tämä julkaisu sisältää seuraavat päivitykset.


Otsikko|Kuvaus
---|---
Suurin arvo on suurennettu Out of tilaus poikkeama-ikkuna|Out of tilauksen poikkeama ikkunan enimmäiskoko on nyt 59:59 (mm: ss)
JSON Siirtomuoto: Rivin arvot on erotettu luetteloerottimella tai matriisista.|Nyt asetus on siirrettäessä Blob-objektien tallennustilaan tai tapahtumaa-toiminnossa tulosteen joko matriisi JSON objektien tai jakamalla JSON-objektien kanssa uusi rivi. 

## <a name="notes-for-04162015-release-of-stream-analytics"></a>Muistiinpanojen 04/16/2015 Stream Analytics-versiossa ##


Otsikko|Kuvaus
---|---
Viive Azure-tallennustilan tilin määrittäminen|Kun luot Stream Analytics työn alueen ensimmäistä kertaa, sinun pyydetään tallennustilan uuden tilin luominen tai määrittää aiemmin luodun tilin alueella olevat työt Stream Analytics seurantaa varten. Vuoksi viive määrittäminen seurantaa toinen Stream Analytics-työ luominen saman alueen 30 minuutin kuluessa pyytää toisen tallennustilan tilin sijaan näkyy viimeksi määritetty yksi seuranta tallennustilan tilin avattavasta argumenteille. Tallennustilan tarpeettomat-tilin luominen välttämiseksi Odota luotuasi työn alueen ennen muita valmistelutöiden alueen ensimmäisen kerran 30 minuuttia.
Työn päivittäminen|Tällä hetkellä Stream Analytics ei tue määritystä tai käynnissä olevan projektin määrittäminen live muutokset. Jotta voit muuttaa syöte, tulostus, kyselyn, asteikko tai käynnissä olevan projektin määrittäminen, lopeta ensin työn.
Tietotyyppien johtaa lähde|Jos CREATE TABLE-lause ei käytetä, syötteen tyyppi johdetaan syötteen muoto, esimerkiksi CSV kaikki kentät on merkkijono. Kenttiä on oikea tyyppi CAST-funktion käyttäminen siten vältät ristiriita virheitä muunnetaan erikseen.
Puuttuvat kentät ovat outputted kuin null-arvoja|Viittaaminen kentän, joka ei ole lähde-johtaa tulosteen tapahtuman null-arvoja.
KANSSA lauseet on edeltävän SELECT-lauseet|Kyselyn SELECT-lauseet on seurattava alikyselyjen määritetty sisään lauseita.
Muisti-ongelma|Streaming Analytics työt suuri poikkeama, järjestys tapahtumien ja/tai monimutkaisia kyselyjä ylläpito tilan suuria määriä saattaa aiheuttaa sivustokokoelmiesi muistia, tuloksena on projektin työn uudelleen. Projektin toimintojen lokit näytetään aloittamisesta ja lopettamisesta-toimintoja. Voit välttää tämän ongelman skaalata, kyselyyn useita osioiden välillä. Tulevissa versioissa tämä rajoitus pystyttävä täyttämään heikennä suorituskyvyn sellaisiin töissä, eikä niitä uudelleen.
Suuri blob-syötteiden ilman paketti aikaleima voi aiheuttaa Muisti-ongelma|Muissa Blob-objektien tallennustilaan suuria tiedostoja voi aiheuttaa kaatumisen Stream Analytics työt, jos aikaleimakenttää ei ole määritetty kautta AIKALEIMA mukaan. Voit välttää tämän ongelman säilyttää kunkin Blob-objektien alle 10 Megatavun kokoisia.
SQL-tietokantaan tapahtuman aseman rajoitus|Kun Käytä SQL-tietokantaan tulostuksen kohteen, erittäin suuri tulosteen tietomäärien saattaa aiheuttaa aikakatkaisu Stream Analytics-työ. Voit ratkaista ongelman pienentää äänenvoimakkuutta käyttämällä koosteet tai suodattimen operaattorit tai valitse Azure-Blob-säiliö tai tapahtuman keskittimet tulostus-kohteeksi sen sijaan.
PowerBI tietojoukkoja voi olla vain yksi taulukko|PowerBI ei tue useammalla kuin yhdellä taulukolla annetun tietojoukko.

## <a name="get-help"></a>Ohjeiden saaminen
Saat lisäohjeita yritä Microsoftin [Azure Stream Analytics-keskustelupalsta](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Seuraavat vaiheet

- [Azure Stream Analytics esittely](stream-analytics-introduction.md)
- [Azure Stream Analytics käytön aloittaminen](stream-analytics-get-started.md)
- [Skaalaa Azure Stream Analytics töitä](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics kyselyn kieliohje](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics hallinta REST API-viittaus](https://msdn.microsoft.com/library/azure/dn835031.aspx)
 

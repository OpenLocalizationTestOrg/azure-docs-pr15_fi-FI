<properties
   pageTitle="Lokitiedoston Analytics-tietojen vieminen Power BI | Microsoft Azure"
   description="Power BI on pilvipohjainen business analytics-palvelu Microsoftilta, joka tuottaa monipuolisia visualisointeja ja raportteja eri tietojoukkojen analysointia varten.  Lokitiedoston Analytics jatkuvasti tiedot voidaan viedä OMS säilö Power BI niin avulla voidaan hyödyntää sen visualisoinnit ja Analyysityökalut.  Tässä artikkelissa käsitellään määrittäminen kyselyjen Log Analytics, joka vie automaattisesti säännöllisin väliajoin Power BI."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="bwren" />

# <a name="export-log-analytics-data-to-power-bi"></a>Power BI Log Analytics-tietojen vieminen

[Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/) on pilvipohjainen business analytics-palvelu Microsoftilta, joka tuottaa monipuolisia visualisointeja ja raportteja eri tietojoukkojen analysointia varten.  Lokitiedoston Analytics automaattisesti tiedot voidaan viedä OMS säilö Power BI niin avulla voidaan hyödyntää sen visualisoinnit ja Analyysityökalut.

Kun määrität Power BI Log Analytics kanssa, voit luoda lokin kyselyitä, jotka vie niiden tulokset vastaavat tietojoukkoja Power BI.  Kyselyn ja vie säilyy automaattinen suorittaminen aikataulussa, joka määritetään dataset pitäminen ajan tasalla Log Analytics keräämiä uusimmat tiedot.

![Lokitiedoston Analytics Power BI](media/log-analytics-powerbi/overview.png)

## <a name="power-bi-schedules"></a>Power BI-aikatauluja

*Power BI-aikataulun* sisältää log haun, joka vie tietojoukko OMS säilöstä vastaavan tietojoukko Power BI-ja aikataulun, joka määrittää, kuinka usein haun suoritetaan tasalla dataset.

Tietojoukon kentät vastaavat log haun palauttamat tietueet ominaisuudet.  Haku palauttaa tietueita erityyppisiä dataset sisältää kaikki mukana tietuetyyppien ominaisuudet.  

> [AZURE.NOTE] Paras käytäntö on käyttää lokiin hakukyselyn, joka palauttaa tietoja eli suorittamiseen minkä tahansa koonnin käyttämällä komentoja, kuten [Mitta](log-analytics-search-reference.md#measure)on.  Voit suorittaa minkä tahansa kooste ja laskutoimitukset Power BI: n perustietoja.

## <a name="connecting-oms-workspace-to-power-bi"></a>Yhteyden muodostaminen Power BI OMS työtila

Ennen kuin voit viedä lokin Analytics Power BI-yhdistettävä OMS työtilan Power BI-tiliisi seuraavalla tavalla.  

1. Valitse OMS-konsolin **asetukset** -ruutu.
2. Valitse **tilit**.
3. Valitse **Työtilan tiedot** -osassa **Yhdistä Power BI-tilille**.
4. Anna tunnistetiedot Power BI-tilisi.

## <a name="create-a-power-bi-schedule"></a>Power BI aikataulun luominen

Luo Power BI-aikataulua kunkin tietojoukko seuraavalla tavalla.

1. Valitse OMS-konsolin **Log haku** -ruutua.
2. Kirjoita uuden kyselyn tai valitse tallennettu haku palauttaa tiedot, jotka haluat viedä **Power BI**.  
3. Napsauta **Power BI** -valintaikkunan avaaminen sivun yläosassa **Power BI** -painiketta.
4. Seuraavassa taulukossa tiedot ja valitse **Tallenna**.

| Ominaisuus | Kuvaus |
|:--|:--|
| Nimi | Tunnistaa aikatauluun, kun tarkastelet Power BI-aikatauluja luettelon nimi. |
| Tallennettu haku | Lokitiedoston haun.  Voit valita nykyisen kyselyn tai valitse aiemmin luotu tallennetun haun avattava. |
| Aikataulu | Kuinka usein haluat suorittaa tallennetun haun ja vie Power BI-tietojoukko.  Arvon on oltava 15 minuuttia ja 24 tuntia. |
| Tietojoukon nimi | Power BI: n tietojoukon nimi.  Se voi luoda, jos se ei ole ja jos sitä ole päivitetty. |

## <a name="viewing-and-removing-power-bi-schedules"></a>Power BI-aikataulujen tarkasteleminen ja poistaminen

Näytä luettelo olemassa olevat Power BI-raporttimallit noudattamalla seuraavia ohjeita.

1. Valitse OMS-konsolin **asetukset** -ruutu.
2. Valitse **Power BI**.

Lisäksi aikataulun yksityiskohdista näytetään, kuinka monta kertaa, aikatauluun on suoritettu edellisen viikon ja viimeisen synkronoinnin tila.  Synkronoi havaitsi virheen, jos olet napsauttanut linkkiä tietueiden lokitiedoston etsiminen toimimaan virheen tiedot.

Voit poistaa aikataulun napsauttamalla **X** -, **Poista sarake**.  Voit poistaa **käytöstä**valitsemalla aikataulun.  Voit muokata aikataulun poistaa sen ja luoda uusia asetuksia.

![Power BI-aikatauluja](media/log-analytics-powerbi/schedules.png)

## <a name="sample-walkthrough"></a>Esimerkki vaiheittainen kuvaus
Seuraavassa osassa käydään läpi Esimerkki Power BI-aikataulun luominen ja sen tietojoukon avulla voit luoda yksinkertaisen raportin.  Tässä esimerkissä joukon tietokoneita kaikki suorituskykytietoja viedään Power BI ja viivakaavion luodaan näyttämään suorittimen käyttö.

### <a name="create-log-search"></a>Log-haun luominen
Aluksi luodaan tiedot, jotka haluat lähettää dataset lokitiedoston etsiminen.  Tässä esimerkissä käytetään kysely, joka palauttaa kaikki suorituskykytietoja tietokoneissa nimellä, joka alkaa *srv*.  

![Power BI-aikatauluja](media/log-analytics-powerbi/walkthrough-query.png)

### <a name="create-power-bi-search"></a>Power BI-haun luominen
Napsautetaan **Power BI** -painiketta, Avaa Power BI-valintaikkuna ja anna vaaditut tiedot.  Haluamme haun voi suorittaa kerran tunnissa ja luo kutsutaan *Contoso Perf*tietojoukko.  Koska jo on avata haun, joka luo tiedot haluamme, *Käytä nykyistä hakukyselyn* oletusarvo säilyy **Tallentaa**haun.

![Power BI-haku](media/log-analytics-powerbi/walkthrough-schedule.png)

### <a name="verify-power-bi-search"></a>Tarkista Power BI-haku
Voit varmistaa, että on luonut aikataulun oikein, nähdä hänen Power BI hakujen luettelo **asetukset** -ruudun kohdassa OMS raporttinäkymät-ikkunassa.  Syy Odota muutama minuutti ja päivittää tätä näkymää, ennen kuin se ilmoittaa, että synkronointi on suoritettu.

![Power BI-haku](media/log-analytics-powerbi/walkthrough-schedules.png)

### <a name="verify-the-dataset-in-power-bi"></a>Tarkista Power BI-tietojoukko
Olemme Kirjaudu sisään Microsoftin tilille osoitteessa [powerbi.microsoft.com](http://powerbi.microsoft.com/) ja vieritä **tietojoukkoja** vasemman ruudun alareunassa.  Näemme, että *Contoso Perf* tietojoukko näy, joka ilmaisee, että Microsoftin vienti on suoritettu onnistuneesti.

![Power BI-tietojoukko](media/log-analytics-powerbi/walkthrough-datasets.png)

### <a name="create-report-based-on-dataset"></a>Raportin luominen tietojoukko perusteella
Olemme valitsemalla **Contoso Perf** tietojoukko ja valitse sitten **tuloksista** oikeuden tarkastella kentät, jotka ovat tämän tietojoukon **kentät** -ruudussa.  Olemme suorittimen käyttö kussakin tietokoneessa, jossa näkyy viivakaavion luominen suorittaa seuraavat toimenpiteet.

1. Valitse viivakaavion visualisointi.
2. Vedä **nimi** **tason** raporttisuodattimeen ja tarkista **suoritin**.
3. Vedä **CounterName** **tason** raporttisuodattimeen ja tarkista **Suoritinaika**.
4. Vedä **CounterValue** **arvot**.
5. Vedä **tietokoneen** **selite**.
6. Vedä **TimeGenerated** **akseli**.

Näkyvissä tuloksena viivakaaviota näkyy Microsoftin tietojoukko tiedoilla.

![Power BI-viivakaavio](media/log-analytics-powerbi/walkthrough-linegraph.png)

### <a name="save-the-report"></a>Tallenna raportti
Olemme Tallenna raportti valitsemalla näytön yläreunassa Tallenna-painiketta ja vahvista, että se näkyy nyt vasemmanpuoleisessa ruudussa raportit-osioon.

![Power BI-raportit](media/log-analytics-powerbi/walkthrough-report.png)

## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja [lokin haut](log-analytics-log-searches.md) luonnissa kyselyitä, jotka voi viedä Power BI.
- Lue lisää [Power BI](http://powerbi.microsoft.com) luonnissa visualisoinnit Log Analytics viennin mukaan.

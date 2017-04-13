<properties
   pageTitle="Tietojen siirtäminen SQL-tietovarasto | Microsoft Azure"
   description="Tietojen siirtäminen SQL Azure-tietovarasto, ratkaisujen kehittämiseen liittyviä vinkkejä."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/25/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="migrate-your-data"></a>Tietojen siirtäminen
Tiedot voidaan siirtää eri lähteistä SQL-tietovarasto eri työkaluja.  SYÖTTÖ kopio ja SSIS bcp kaikki voidaan tämän tavoitteen saavuttamiseksi. Kuitenkin tietojen kasvaa summana kannattaa ottaa huomioon tietoja siirtymisprosessista vaiheiden jakamiseen. Tämä niille mahdollisuuden jokaisen vaiheen sekä varmistaa sujuvan tietojen siirtäminen toimintakykyyn suorituskyvyn parantamiseksi.

Tässä artikkelissa kerrotaan ensin SYÖTTÖ kopio ja SSIS bcp yksinkertainen siirron käyttötavoista. Se sitten hieman tarkempaa kyselyjä miten siirron voi optimoida ulkoasun.

## <a name="azure-data-factory-adf-copy"></a>Azure Factory (SYÖTTÖ) kopioi
[Kopioi SYÖTTÖ][] on osa [Azure Data Factory][]. Voit käyttää SYÖTTÖ kopioi tietojen vieminen tasainen tiedostojen paikallinen tallennus remote Azure Blob-objektien tallennustilaan tai suoraan SQL-tietovarasto tasainen tiedostojen käyttöön.

Jos tietojen käynnistyy tasainen tiedostot ja valitse sinun on ensin se siirretään tallennustilan Azure-blob ennen aloittamista kuormituksen sen SQL-tietovarasto. Kun tiedot siirretään Azure-blob-säiliö voit käyttää [SYÖTTÖ kopioi][] uudelleen push-tietojen tuominen SQL-tietovarasto.

PolyBase sisältää myös tehokas-vaihtoehto tiedot lataamista varten. Kuitenkin tarkoittaa kaksi työkaluilla yhden sijaan. Jos tarvitse parhaan suorituskyvyn ja käytä PolyBase. Jos haluat yksi työkalu kokemus (ja tiedot eivät ole valtaviin) SYÖTTÖ on vastausta.

> [AZURE.NOTE] PolyBase edellyttää UTF-8 olevan datatiedostot. Tämä on SYÖTTÖ kopioi oletusarvoisen koodauksen siten ei ole enää mitään, jos haluat muuttaa. Näin voit muuttaa ei SYÖTTÖ kopioi oletusasetuksista muistutus.

HEAD päälle seuraavassa artikkelissa joitakin hyvien [SYÖTTÖ objektit][].

## <a name="integration-services"></a>Integration Services ##
Integration Services (SSIS) on tehokas ja joustava Pura Muunna ja lataa (ETL), joka tukee monitasoisia työnkulkuja, muuntamiseen ja tietojen lataamisen monella. SSIS avulla voit siirtää tietoja Azure tai laajempi siirron osana.

> [AZURE.NOTE] UTF-8 viedä SSIS ilman tavu tilauksen Merkitse tiedostossa. Voit määrittää tämän ensin käyttämällä johdetun sarakkeen-osan muuntaa käyttämään 65001 UTF-8 koodisivu tiedonkulun merkki-tiedot. Kun sarakkeet muuntanut kirjoittaa tiedot tietuetiedostoon kohde-sovittimen, varmistetaan, että 65001 myös valittu kuin koodisivua tiedostolle.

SSIS muodostaa yhteyden SQL-tietovarasto samalla tavalla kuin se muodostaa yhteyden SQL Server-käyttöönotto. Yhteyksien on kuitenkin käyttää ADO.NET-Yhteyksienhallinnan. Voit myös olisi huolehtia määrittäminen "Käytä joukkona Lisää kun niitä on saatavilla" Suurenna nopeus-asetukseksi. Lue lisätietoja [ADO.NET kohde sovittimen][] -artikkelista Saat lisätietoja tämän ominaisuuden

> [AZURE.NOTE] Yhteyden muodostaminen SQL Azure-tietovarasto avulla OLEDB ei tueta.

Lisäksi on aina mahdollista, että paketin epäonnistumiseen määräpäivä rajoittaminen tai verkko-ongelmia. Rakenne-paketteja, jotta he voivat jatkaa sekä virheen, ilman tekeminen uudelleen työmäärän, jonka valmistunut ennen virheen.

Lisätietoja Lue [SSIS-ohjeista][].

## <a name="bcp"></a>BCP
BCP on komentorivin apuohjelma, joka on tarkoitettu tietuetiedostoon tuonti ja vienti. Jotkin muunnos voidaan suorittaa tietojen viennin aikana. Voit suorittaa yksinkertaisen muunnoksia avulla voit valita ja muuntaa tiedot kyselyn. Kun viety, tasainen tiedostot sitten voidaan ladata suoraan kohteeseen tietovarasto SQL-tietokantaan.

> [AZURE.NOTE] Kannattaa usein käyttää tietojen vieminen lähde-järjestelmässä näkymässä käytettyä muunnoksia hyvä. Näin varmistat, että logiikan säilyttää ja prosessi on toistettavien.

Bcp etuja ovat:

- Yksinkertaisuuden. BCP komennot ovat helppo luominen ja suorittaminen
- Voi käyttää uudelleen käynnistämiseen kuormituksen prosessi. Kerran viedyn kuormituksen voidaan suorittaa, kuinka monta kertaa tahansa

Bcp rajoitukset ovat seuraavat:

- BCP toimii vain taulukoituja tasainen tiedostot. Se ei toimi kuten xml- tai JSON-tiedostoja
- BCP ei tue vieminen UTF-8. Tämä voi estää PolyBase käyttäminen bcp viedä tiedot
- Muunnoksen ominaisuuksia ovat Vie vaiheen vain tarkasteltavana ja yksinkertainen laatu
- BCP ei ole mukauttaa on tehokkaat tietojen lataaminen Internetin välityksellä. Verkon skannattavien saattaa aiheuttaa virheen kuormituksen.
- onko kohdetietokannassa ennen kuormituksen rakenteen riippuvainen BCP

Lisätietoja on artikkelissa [Käytä bcp lataamaan tietojen tuominen SQL-tietovarasto][].

## <a name="optimizing-data-migration"></a>Tietojen siirtäminen optimointi
SQLDW tietojen siirron prosessi voi voidaan tehokkaasti jakaa erillinen kolme vaihetta:

1. Tietojen vieminen
2. Tietoja siirretään Azure
3. Lataa SQLDW kohdetietokannan

Jokaisen vaiheen voi optimoida erikseen luoda tehokkaat, voi käyttää uudelleen käynnistämiseen ja joustavat siirtoprosessia, suurentaa suorituskyvyn kussakin vaiheessa.

## <a name="optimizing-data-load"></a>Optimoiminen tietojen lataus
Näitä käänteisessä järjestyksessä hetken; katsoo tietojen lataaminen nopein tapa on PolyBase. PolyBase kuormituksen prosessin optimoiminen sijoittaa edellytykset edellisten vaiheiden niin kannattaa ymmärtää tämä jaettu. Ne:

1. Datatiedostojen koodaus
2. Muotoile datatiedostojen
3. Tietoja tiedostojen sijainti

### <a name="encoding"></a>Koodaus
PolyBase edellyttää datatiedostot on koodattu UTF-8. Tämä tarkoittaa, että kun tietojen vieminen sen on oltava tämä vaatimus. Jos tiedot sisältävät vain basic ASCII-merkkien (ei ole laajennettu ASCII) sitten nämä kartan suoraan UTF-8-standardin ja sinun ei tarvitse olla huolissaan liikaa tietoja koodaus. Jos tiedot sisältävät erikoismerkkejä, kuten treemat, korostukset tai symboleja tai tietojen tukee latinalainen kieliä sitten sinun on varmistaa, että viedä tiedostot ovat oikein koodattu UTF-8.

> [AZURE.NOTE] BCP ei tue tietojen vieminen UTF-8. Tämän vuoksi paras vaihtoehto on käyttää Integration Services tai SYÖTTÖ-kopioi tietojen viennin. Se on osoittavat että tiedosto ei tarvita UTF-8-tavuinen tilauksen Merkitse (BOM).

Koodattu UTF-16 tiedostoja on uudelleen kirjoitetaan ***edellisen*** tiedonsiirron.

### <a name="format-of-data-files"></a>Muotoile datatiedostojen
PolyBase valtuuttaa kiinteän rivin pääte \n tai rivi. Tietoja tiedostojen on oltava standardin mukainen. Ei ole merkkijono tai sarakkeen päättymiskohdat koskevat rajoitukset.

Joudut ehkä määrittää jokaisessa sarakkeessa tiedoston PolyBase ulkoisen taulukon osana. Varmista, että kaikki viedyn sarakkeet tarvitaan ja tyypit vaaditut standardit mukaiseksi.

Lue lisätietoja takaisin [siirtää rakenteen] artikkelista tuetut tietotyypit tarkat tiedot.

### <a name="location-of-data-files"></a>Tietoja tiedostojen sijainti
SQL-tietovarasto käyttää PolyBase lataa tiedot Azure-Blob-säiliö yksityisesti. Näin ollen tiedot on ensin siirrettävä blob varastoon.

## <a name="optimizing-data-transfer"></a>Optimoiminen tiedonsiirto
Tietojen siirtäminen mahdollisimman pieneksi osat on Azure tietojen siirto. Paitsi kaistanleveys on ongelma, mutta myös verkon luotettavuutta vakavasti voivat haitata edistyminen. Tietojen siirtämisestä Azure on oletusarvoisesti Internetin välityksellä niin siirto virheiden hyvinkin kohtuudella todennäköisesti. Nämä virheet voi kuitenkin säätää tiedot lähetetään uudelleen kokonaan tai osittain.

Lähettäjä sinulla on useita vaihtoehtoja parantamiseksi nopeuden ja toimintakykyyn toimintaohjeet:

### <a name="expressroute"></a>[ExpressRoute][]
Sinun kannattaa harkita [ExpressRoute][] avulla voit nopeuttaa siirto. [ExpressRoute][] avulla voit yksityisen yhteyden Azure, jotta yhteyden ei siirry julkisen Internetin välityksellä. Tämä ei ole mukaan tavoin pakollinen vaihe. Kuitenkin sen voit parantaa kun valitseminen tietojen Azure-paikallisen tai rinnakkain.

[ExpressRoute][] käytön etuja ovat:

1. Parannettu luotettavuus:
2. Verkon nopeammin nopeus
3. Pienet verkon latenssin
4. verkkosuojauksen

[ExpressRoute][] on hyötyä määrälle skenaariot;. Et pelkästään siirto.

Kiinnostunut? Lisätietoja ja hinnoittelu Tarkista seuraavasta [ExpressRoute ohjeissa][].

### <a name="azure-import-and-export-service"></a>Azure Tuo ja vie-palvelu
Azure tuominen ja vieminen-palvelu on tarkoitettu suuri (gt ++) valtaviin (TT ++) siirtoja tietojen Azure tietojen siirron prosessi. Se tehdään tietojen kirjoittaminen levyille ja toimittamista Azure tietokeskuksen. Levy sisältö sitten ladataan kyselyjä Azure-tallennustilan BLOB puolestasi.

Vie tuontiprosessin ylätason näkymän on seuraavanlainen:

1. Määritä Azure-Blob-säiliö-säilö tietojen vastaanottamiseksi
2. Tietojen vieminen paikallinen tallennus
2. Kopioi tiedot 3,5 tuuman SATA III tai II kiintolevyaseman levyasemien [Azure Tuo/Vie-työkalulla]
3. Luo käyttämällä Azure tuominen ja vieminen palvelua, joka tarjoaa [Azure Tuo/Vie työkalun] tuottamat Päivyri-tiedostojen tuonti-työ
4. Toimita levyjen nimettyjen Azure tietokeskuksen
5. Tietoja siirretään Azure-Blob-säiliö-säilö
6. Lataa tiedot käyttämällä PolyBase SQLDW

### <a name="azcopy-utility"></a>[AZCopy][] -apuohjelma
[AZCopy][] -apuohjelma on erinomainen työkalu Azure-tallennustilan BLOB siirretään tietojen käytön. Pienten (Mt ++) on suunniteltu hyvin suuri (gt ++) tietojen siirtojen. [AZCopy] myös on suunniteltu antamaan hyvä joustavat siirtonopeuden siirrettäessä tietoja Azure ja se on tietojen siirto vaiheessa. Kerran siirretyt voit ladata tiedot käyttämällä PolyBase SQL-tietovarasto kyselyjä. Voit myös sisällyttää AZCopy että SSIS-pakettien "Suorittaa prosessin"-tehtävän avulla.

Voit käyttää AZCopy ensin tarvitset Lataa ja asenna se. Ei [tuotannon versio][] ja [preview-versiosta][] .

Lataa tiedosto tiedostojärjestelmästä on alla kuvattua komennon:

```
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:abc.txt
```

Ylätason prosessin yhteenveto voi olla:

1. Määritä Azure tallennustilan blob-säilö tietojen vastaanottamiseksi
2. Tietojen vieminen paikallinen tallennus
3. AZCopy tietojen säilössä Azure-Blob-säiliö
4. Lataa tiedot SQL-tietovarasto PolyBase käyttäminen

Täysi asiakirjat saatavilla: [AZCopy][].

## <a name="optimizing-data-export"></a>Optimoiminen tietojen vieminen
Lisäksi varmistaa, että PolyBase mukaan asettelun vaatimusten mukainen viennin voit myös kysyä optimoi paranna edelleen tietojen viennin.

> [AZURE.NOTE] PolyBase vaatii tietojen olevan UTF-8-muodossa, on epätodennäköistä suorittaa tietojen viennin bcp avulla. BCP ei tue määritetty datatiedostot UTF-8. SSIS- tai SYÖTTÖ kopioi on paljon paremmin hyödyntää suorittamiseen tällaisen tietojen vienti.

### <a name="data-compression"></a>Tietojen pakkaus
PolyBase voi lukea pakattu gzip tietoja. Jos pystyt tietojen gzip tiedostojen pakkaaminen pienentää niiden ollessa verkossa tietojen määrää.

### <a name="multiple-files"></a>Useita tiedostoja
Purkaa suurten taulukoiden tuominen useita tiedostoja ei vain avulla voidaan parantaa Vie nopeutta, se helpottaa myös siirron re-startability-tietojen kerran Azure-blob-säiliö Yleinen hallinta. Yksi PolyBase monia nice ominaisuuksia on, että se lukee kaikki kansion sisällä olevat tiedostot ja käsittele yhdeksi taulukoksi. Näin ollen hyvä eristää tiedostot kullekin taulukolle omassa kansioon.

PolyBase tukee myös nimeltään "rekursiivinen kansion ohitus-ominaisuutta. Voit käyttää tätä toimintoa parantavia organisaation viedyn tietojen parantaa tietojen hallinta.

Lisätietoja ladataan PolyBase tietojasi, katso [Käytä PolyBase lataamaan tietojen tuominen SQL-tietovarasto][].


## <a name="next-steps"></a>Seuraavat vaiheet
Lisätietoja siirto on [Siirtyminen, SQL-tietovarasto ratkaisu][].
Artikkelissa kehitys Lisävihjeitä [kehittäminen yleiskatsaus][].

<!--Image references-->

<!--Article references-->
[AZCopy]: ../storage/storage-use-azcopy.md
[Kopioi SYÖTTÖ]: ../data-factory/data-factory-data-movement-activities.md 
[SYÖTTÖ-objektit]: ../data-factory/data-factory-samples.md
[ADF Copy examples]: ../data-factory/data-factory-copy-activity-tutorial-using-visual-studio.md
[kehittäminen yleiskatsaus]: sql-data-warehouse-overview-develop.md
[Siirtää SQL-tietovarasto ratkaisu]: sql-data-warehouse-overview-migrate.md
[SQL Data Warehouse development overview]: sql-data-warehouse-overview-develop.md
[Lataa tiedot SQL-tietovarasto bcp avulla]: sql-data-warehouse-load-with-bcp.md
[Lataa tiedot SQL-tietovarasto PolyBase avulla]: sql-data-warehouse-get-started-load-with-polybase.md


<!--MSDN references-->

<!--Other Web references-->
[Azure Data Factory]: http://azure.microsoft.com/services/data-factory/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[ExpressRoute dokumentaatio]: http://azure.microsoft.com/documentation/services/expressroute/

[tuotannon versio]: http://aka.ms/downloadazcopy/
[Preview-versiosta]: http://aka.ms/downloadazcopypr/
[ADO.NET kohde sovittimen]: https://msdn.microsoft.com/library/bb934041.aspx
[SSIS-asiakirjat]: https://msdn.microsoft.com/library/ms141026.aspx

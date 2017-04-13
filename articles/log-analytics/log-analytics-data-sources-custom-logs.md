<properties 
   pageTitle="Mukautettu Kirjaa lokin Analytics | Microsoft Azure"
   description="Lokitiedoston Analytics voi kerätä tapahtumien tekstitiedostoista Windows- ja Linux tietokoneissa.  Tässä artikkelissa kerrotaan, miten voit määrittää uuden mukautetun lokin ja he luovat OMS säilössä tietueiden tietoja."
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

# <a name="custom-logs-in-log-analytics"></a>Lokitiedoston Analytics mukautetun lokit

Lokitiedoston Analytics mukautettu lokit tietolähteen avulla voit kerätä tapahtumia tekstitiedostoista Windows- ja Linux tietokoneissa. Useita sovelluksia lokitiedot tekstitiedostoista kirjaaminen Vakiopalvelu, kuten Windowsin tapahtumalokiin kirjaaminen tai Syslog sijaan.  Kun kerännyt, kunkin tietueen lokiin voidaan jäsentää yksittäisiä kenttiä Log Analytics [Mukautetut kentät](log-analytics-custom-fields.md) -ominaisuuden avulla.

![Mukautetun lokitiedoston sivustokokoelman](media/log-analytics-data-sources-custom-logs/overview.png)

Lokitiedostojen kerätään on vastattava seuraavat ehdot.

- Kirjaudu on yksittäinen kullakin rivillä on tai käyttää aikaleima vastaavat jompikumpi seuraavista muodoista jokaisen merkinnän alkuun.

    YYYY-MM-DD HH <br>
  P.K.VVVV HH: MM: SS AM/PM <br>
  Ma DD-YYYY ss
    
- Lokitiedosto on ei salli pyöreä päivitykset, johon tiedosto korvautuu uusien tapahtumien. 

## <a name="defining-a-custom-log"></a>Mukautetun lokitiedoston määrittäminen

Seuraavien ohjeiden avulla voit määrittää mukautetun lokitiedostoon.  Siirry tämän artikkelin Esimerkki lisääminen mukautetun lokitiedoston vaiheittainen loppuun.

### <a name="step-1-open-the-custom-log-wizard"></a>Vaihe 1. Avaa ohjattu mukautetun lokitiedoston

Ohjattu mukautetun Log suorittaa OMS-portaalissa, ja voit määrittää uuden mukautetun lokin kerää.

1.  Siirry OMS-portaalin **asetuksia**.
2.  Valitse **tiedot** ja valitse sitten **Mukautettu lokitiedot**.
3.  Oletusarvon mukaan kaikki määritysmuutoksia automaattisesti siirretty, kaikki agenttien vuoksi.  Linux agenttien vuoksi määritystiedoston lähetetään Fluentd tietojen kerääminen.  Jos haluat muokata tämän tiedoston manuaalisesti jokaisen Linux-agentti, poista sitten valinta *Käytä alla määritysten Linux-tietokoneissa*.
4.  Valitse **Lisää +** Avaa ohjatun mukautetun loki.

### <a name="step-2-upload-and-parse-a-sample-log"></a>Vaihe 2. Lataa ja jäsentää otoksen lokia

Voit aloittaa lataamalla mukautetun lokitiedoston näyte.  Ohjattu toiminto jäsentää ja näyttää tässä tiedostossa, voit tarkistaa.  Log Analytics käyttävät, voit määrittää kunkin tietueen tunnistamiseen erottimen.

**Uusi rivi** on oletusarvoinen erotin ja käytetään lokitiedostot, joissa on yksittäinen riviä kohden.  Jos rivi alkaa päivämäärän ja kellonajan jossakin käytettävissä olevat muodot, voit määrittää **aikaleiman** erottimen, joka tukee tapahtumia, jotka ulottuvat enemmän kuin yksi rivi. 

Jos käytössä on aikaleiman erottimen, kunkin tietueen OMS tallennetaan TimeGenerated-ominaisuuden täytetään määritetty lokitiedosto kyseisen tapahtuman päivämäärä ja kellonaika.  Jos käytössä on uusi rivi erottimen, TimeGenerated täytetään päivämäärän ja kellonajan Log Analytics kerääminen tapahtuma. 

>[AZURE.NOTE]Lokitiedoston Analytics käsittelee tällä hetkellä kerääminen lokista, käytä aikaleiman erottimen UTC-ajan päivämäärä ja kellonaika.  Tämä muutetaan pian aikavyöhykkeen käyttämisestä agentti. 
 
1.  Valitse **Selaa** ja Etsi mallitiedosto.  Huomaa, että tämä saattaa painike voi olla joko **Tiedosto** kaikissa selaimissa.
2.  Valitse **Seuraava**. 
3.  Mukautettu Log ohjattu Lataa tiedosto ja tietueet, jotka se luettelosta.
4.  Voit muuttaa erotin, jota käytetään uuden tietueen etsiminen ja erotin, joka määrittää parhaiten log tiedostossa olevat tietueet.
5.  Valitse **Seuraava**.

### <a name="step-3-add-log-collection-paths"></a>Vaihe 3. Lisää log sivustokokoelman polut

Sinun on määritettävä vähintään yksi polut-kohtaa, johon se löydä mukautetun lokitiedoston agentti.  Voit joko antaa polkua ja lokitiedoston nimi tai voit määrittää polun yleismerkkien nimi.  Tämä tukee sovelluksia, jotka luot uuden tiedoston päivittäin tai kun yhden tiedoston saavuttaa tietyn koon.  Voit myös antaa eri poluista yhteen lokitiedostoon.

Esimerkiksi sovellus voi luoda lokitiedostoon aina päivän päivämäärän, kuten log20100316.txt nimestä. Näiden lokin kuvion voi olla *lokin\*.txt* joka koskee kaikki seuraavan sovelluksen lokitiedosto on nimeäminen värimallin.

Seuraavassa taulukossa on esimerkkejä kelvollinen kuvioiden avulla ja määritä eri lokitiedostot. 

| Kuvaus | Polku |
|:--|:--|
| Kaikki tiedostot *C:\Logs* .txt-tunnisteella Windows-agentti | C:\Logs\\\*.txt |
| Kaikki tiedostot kohteessa *C:\Logs* lokin ja valitse Windows-agentti .txt-tunnisteella nimellä | C:\Logs\log\*.txt |
| Kaikki tiedostot Linux agentti */var/log/audit* .txt-tunnisteella | /var/log/audit/*.txt |
| Kaikki tiedostot kohteessa */var/log/audit* lokin ja valitse Linux agentti .txt-tunnisteella nimellä | /var/log/audit/log\*.txt |
  

1.  Voit määrittää mitä polku-muotoa Valitse Windows tai Linux lisätään.
2.  Kirjoita polku ja valitse **+** painiketta.
3.  Toista tämä minkä tahansa uusia polkuja.

### <a name="step-4-provide-a-name-and-description-for-the-log"></a>Vaihe 4. Nimi ja kuvaus lokin

Nimi, jonka määrität käytetään lokitiedoston tyyppi yllä olevien ohjeiden mukaisesti.  Se päättyy aina _CL palkkina mukautetun loki.

1.  Kirjoita lokitiedoston nimi.  ** \_p** jälkiliite annetaan automaattisesti.
2.  Lisää halutessasi **kuvaus**.
3.  Valitse **Seuraava** tallentaa mukautetun log-määritys.

### <a name="step-5-validate-that-the-custom-logs-are-being-collected"></a>Vaihe 5. Vahvista, että mukautettu lokit kerätään
Voi kestää tunneiksi alkuperäinen tietojen uuden mukautetun lokista Log Analytics näkyvän.  Keräätkö tapahtumat Käynnistä polussa lokien määrittämäsi määritetty mukautettuja lokin pisteestä.  Se ei säily tapahtumat, jotka olet ladannut mukautetun lokitiedoston luonnin aikana, mutta se kerää lokitiedostojen, voit etsiä jo olemassa olevan merkinnät.

Kun lokiin Analytics käynnistyy kerääminen mukautetun lokista, sen tietueet ovat käytettävissä Log haulla.  Käytä antamasi mukautetun lokitiedoston **tyyppi** kyselyn nimeä.

>[AZURE.NOTE] Jos RawData-ominaisuus ei ole haku, joudut ehkä suljettava ja avaamalla selain uudelleen.

### <a name="step-6-parse-the-custom-log-entries"></a>Vaihe 6. Mukautetun lokimerkintöjä jäsentää

Koko lokitapahtuman tallennetaan yhden ominaisuuden **RawData**.  Haluat todennäköisesti erottaa tiedot kunkin saapumista yksittäisiä ominaisuuksia, jotka on tallennettu eri osiin.  Voit tehdä tämän käyttämällä lokiin Analytics [Mukautetut kentät](log-analytics-custom-fields.md) -toiminnon.

Yksityiskohtaisia ohjeita jäsennyksen mukautetun lokitapahtuman ei anneta tähän.  Tutustu tietoihin [Mukautetut kentät](log-analytics-custom-fields.md) ohjeissa.

## <a name="disabling-a-custom-log"></a>Mukautetun lokitiedoston poistaminen käytöstä

Et voi poistaa mukautetun lokitiedoston määritys, kun se on luotu, mutta voit poistaa sen käytöstä poistamalla kaikki sen sivustokokoelman polkuja.

1.  Siirry OMS-portaalin **asetuksia**.
2.  Valitse **tiedot** ja valitse sitten **Mukautettu lokitiedot**.
3.  Valitse **tiedot** käytöstä mukautetun lokitiedoston määritelmän vieressä.
4.  Poista kaikki sivustokokoelman polut mukautetun log-määritys.


## <a name="data-collection"></a>Tietojen kerääminen

Lokitiedoston Analytics kerää uusia merkintöjä jokaisen mukautetun lokista noin 5 minuuttia.  Agentti tallentaa sijaintia kunkin lokitiedoston, joka se kerää.  Jos agentti menee offline-tilaan tietyn ajan, sitten Log Analytics kerää tapahtumat-kohtaa, johon se viimeksi jäi, vaikka ne on luotu agentti ollessa offline-tilassa.

Lokitapahtuman koko sisällön kirjoitetaan yhden ominaisuuden **RawData**.  Tämä voi jäsentää useita ominaisuuksia, joita voit analysoida ja määrittämällä [Mukautettuja kenttiä](log-analytics-custom-fields.md) , kun olet luonut mukautetun lokitiedoston etsittävä erikseen.


## <a name="custom-log-record-properties"></a>Mukautetun lokitiedoston tietueen ominaisuudet-valintaikkunassa

Mukautetun lokitiedoston tietueella on seuraavassa taulukossa tyyppi, joka saadaan Kirjaa nimen ja ominaisuudet.

| Ominaisuus | Kuvaus |
|:--|:--|
| TimeGenerated | Päivämäärä ja aika, tietue on kerännyt Log Analytics.  Jos lokiin käyttää mukaan aikaerotin tämä on kerätty merkinnästä aika. |
| SourceSystem  | Edustajan tietueen tyyppi on kerätty. <br> Yhdistä OpsManager – Windows-agentti, joko suoraan tai SCOM <br> Linux – kaikki Linux agenttien vuoksi  |
| RawData             | Kerättyjen tapahtuma koko tekstin. |
| ManagementGroupName | SCOM tekijöiden hallinta-ryhmän nimi.  Muiden tekijöiden tämä on AOI -\<työtilan tunnus\> |


## <a name="log-searches-with-custom-log-records"></a>Lokitiedoston haut mukautetun lokitiedoston tietueisiin

Mukautetun lokien tietueet on tallennettu OMS säilöön, aivan kuten tietueiden jostakin muusta tietolähteestä.  Heillä on vastaavat nimi, jonka annat, kun haluat määrittää lokiin, jotta voit käyttää tyyppi-ominaisuuden hakuun Hae tietueet kerääminen tietyn lokista tyyppi.

Seuraavassa taulukossa on eri log hakuja, jotka hakevat tietueiden mukautetun lokien esimerkkejä.

| Kyselyn | Kuvaus |
|:--|:--|
| Tyyppi = MyApp_CL | Kaikki tapahtumat mukautetun Kirjaudu nimetyn MyApp_CL. |
| Tyyppi = MyApp_CL Severity_CF = virhe | Kaikki tapahtumat mukautetun lokista nimeltä MyApp_CL arvo on *Virhe* mukautetun kentän nimi *Severity_CF*. |




## <a name="sample-walkthrough-of-adding-a-custom-log"></a>Esimerkki vaiheista mukautetun lokitiedoston lisääminen

Seuraavassa osassa käydään läpi Esimerkki luominen mukautetun loki.  Otoksen lokin kerätty on yksittäinen päivämäärä ja aika- ja sitten pilkku alkaen kullekin riville erotinmerkeillä erotellut kentät koodi, tila ja viestin.  Useita otoksen merkinnät näkyvät jäljempänä.

    2016-03-10 01:34:36 207,Success,Client 05a26a97-272a-4bc9-8f64-269d154b0e39 connected
    2016-03-10 01:33:33 208,Warning,Client ec53d95c-1c88-41ae-8174-92104212de5d disconnected
    2016-03-10 01:35:44 209,Success,Transaction 10d65890-b003-48f8-9cfc-9c74b51189c8 succeeded
    2016-03-10 01:38:22 302,Error,Application could not connect to database
    2016-03-10 01:31:34 303,Error,Application lost connection to database

### <a name="upload-and-parse-a-sample-log"></a>Lataa ja jäsentää otoksen lokia

Syy on yksi lokitiedostojen ja näkee, se kerääminen tapahtumat.  Tässä tapauksessa uusi rivi on riittävä erottimen.  Jos lokiin merkintä saattaa kattavat useita viivoja, sitten aikaleiman erottimen on käytettävä.

![Lataa ja jäsentää otoksen lokia](media/log-analytics-data-sources-custom-logs/delimiter.png)

### <a name="add-log-collection-paths"></a>Lisää log sivustokokoelman polut

Lokitiedostojen sijaitsevat *C:\MyApp\Logs*.  Uusi tiedosto luodaan päivittäin, joka sisältää päivämäärän kuvion *appYYYYMMDD.log*nimellä.  Olisi riittävästi kuvion lokin *C:\MyApp\Logs\\\*.log*.

![Lokitiedoston keräämisen polun](media/log-analytics-data-sources-custom-logs/collection-path.png)

### <a name="provide-a-name-and-description-for-the-log"></a>Nimi ja kuvaus lokin

Käytämme *MyApp_CL* ja kirjoita nimi, **kuvaus**.

![Lokitiedoston nimi](media/log-analytics-data-sources-custom-logs/log-name.png)


### <a name="validate-that-the-custom-logs-are-being-collected"></a>Vahvista, että mukautettu lokit kerätään

Käytämme kyselyn *tyyppi = MyApp_CL* palauttaa kaikki tietueet kerättyjen lokista.

![Log-kysely, jossa ei ole mukautetut kentät](media/log-analytics-data-sources-custom-logs/query-01.png)

### <a name="parse-the-custom-log-entries"></a>Mukautetun lokimerkintöjä jäsentää

Käytämme mukautetut kentät Määritä *EventTime*, *koodi*, *tila*ja *viestin* kentät ja Microsoft voi tarkastella ero, kysely palauttaa tietueet.

![Log-kysely, jossa on mukautetut kentät](media/log-analytics-data-sources-custom-logs/query-02.png)

## <a name="next-steps"></a>Seuraavat vaiheet

- [Mukautettujen kenttien](log-analytics-custom-fields.md) avulla voit mukautetun lokitiedoston merkinnät jäsentää yksittäisiä kenttiä.
- Lisätietoja [lokin haut](log-analytics-log-searches.md) tietolähteet ja ratkaisujen kerättyjen tietojen analysointia varten. 
## <a name="specifying-formats"></a>Muodot määrittäminen

Azure Data Factory tukee seuraavia muoto: 

- [Teksti-muodossa](#specifying-textformat)
- [JSON-muoto](#specifying-jsonformat)
- [Avro muoto](#specifying-avroformat)
- [ORC muoto](#specifying-orcformat)
- [Tilat-muoto](#specifying-parquetformat)

### <a name="specifying-textformat"></a>TekstinMuoto määrittäminen

Jos muodossa on määritetty **TekstinMuoto**, voit määrittää seuraavia **valinnaisia** ominaisuuksia **muoto** -kohdassa.

| Ominaisuus | Kuvaus | Sallittu arvo | Pakollinen |
| -------- | ----------- | -------- | -------- | 
| columnDelimiter | Merkki, jota käytetään eri sarakkeisiin tiedostossa. | Vain yksi merkki on sallittu. Oletusarvo on pilkku (,). <br/><br/>Jos haluat käyttää Unicode-merkin, viitata vastaava koodi hakeminen [Unicode-merkkejä](https://en.wikipedia.org/wiki/List_of_Unicode_characters) . Esimerkiksi kannattaa käyttää harvinaisissa tulostuskelvottoman merkki, jonka tiedoissa ei todennäköisesti ole: Määritä "\u0001" joka edustaa Aloita sekä otsikko (ne). | Ei |
| rowDelimiter | Merkki, jota käytetään ja rivit-tiedostossa. | Vain yksi merkki on sallittu. Oletusarvo on jokin seuraavista arvoista luku: "\r\n", "\r", "\n" ja "\r\n" kirjoitusoikeudet. | Ei |
| escapeChar | Erikoismerkin lisääminen käytettävä pääse sarakkeen erottimen tiedoston sisällöstä. <br/><br/>Et voi määrittää escapeChar ja quoteChar taulukon. | Vain yksi merkki on sallittu. Ole oletusarvoa. <br/><br/>Esimerkki: Jos käytössäsi on pilkku (","), kun sarake-erotin, mutta Haluatko, että teksti pilkku (Esimerkki: "Hei, maailman"), voit määrittää '$' ohjausmerkkiä ja käyttää merkkijonon "Hei$, maailman" lähteessä. | Ei | 
| quoteChar | Merkki, jolla tarjous merkkijonoarvo. Sarakkeiden ja rivien erottimia sisällä tarjouksen merkit pidetään osana merkkijonoarvo. Tämä ominaisuus on käytettävissä sekä syöttö- ja tietojoukkoja.<br/><br/>Et voi määrittää escapeChar ja quoteChar taulukon. | Vain yksi merkki on sallittu. Ole oletusarvoa. <br/><br/>Esimerkiksi, jos sinulla on pilkku (",") sarake-erotin, mutta haluat on pilkku teksti (Esimerkki: < Hei, maailman >), voit määrittää "(lainausmerkki) lainausmerkkejä ja käytä merkkijonon"Hei, maailman"lähteessä. | Ei |
| nullValue | Yhtä tai useampaa merkkiä käytetään null-arvoa. | Yhtä tai useampaa merkkiä. Oletusarvot on luku-ja "\N" "\N" ja "NULL" kirjoitus. | Ei |
| encodingName | Määritä koodauksen nimi. | Kelvollinen koodauksen nimi. Katso [Encoding.EncodingName ominaisuus](https://msdn.microsoft.com/library/system.text.encoding.aspx). Esimerkki: windows-1 250 tai shift_jis. Oletusarvo on UTF-8. | Ei | 
| Ensimmäinenriviotsikkona | Määrittää, harkitse ensimmäisen rivin otsikkona. Syötteen tietojoukko Data Factory lukee ensimmäisen rivin otsikkona. Tulostus tietojoukko Data Factory kirjoittaa ensimmäisen rivin otsikkona. <br/><br/>Katso [ **Ensimmäinenriviotsikkona** ja **skipLineCount** käytön skenaariot](#scenarios-for-using-firstrowasheader-and-skiplinecount) otoksen skenaarioita. | TOSI<br/>EPÄTOSI (oletus) | Ei |
| skipLineCount | Ilmaisee, voit ohittaa luettaessa tietojen syötteen tiedostoista rivien määrä. Jos skipLineCount ja Ensimmäinenriviotsikkona on määritetty, rivit ohitetaan ensin ja sitten syötteen tiedoston lukeminen otsikkotiedot. <br/><br/>Katso [Ensimmäinenriviotsikkona ja skipLineCount käytön skenaariot](#scenarios-for-using-firstrowasheader-and-skiplinecount) otosten skenaarioita. | Kokonaisluku | Ei | 
| treatEmptyAsNull | Määrittää, käsittele null tai tyhjä merkkijono kuin null-arvon luettaessa tietojen syöttö-tiedostosta. | TRUE (oletus)<br/>EPÄTOSI | Ei |  

#### <a name="textformat-example"></a>TekstinMuoto-Esimerkki
Seuraavassa esimerkissä näkyy joitakin ominaisuuksia Muotoile TekstinMuoto.

    "typeProperties":
    {
        "folderPath": "mycontainer/myfolder",
        "fileName": "myblobname"
        "format":
        {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "rowDelimiter": ";",
            "quoteChar": "\"",
            "NullValue": "NaN"
            "firstRowAsHeader": true,
            "skipLineCount": 0,
            "treatEmptyAsNull": true
        }
    },

Voit käyttää escapeChar quoteChar sijaan, korvaa rivi quoteChar seuraavat escapeChar kanssa:

    "escapeChar": "$",



### <a name="scenarios-for-using-firstrowasheader-and-skiplinecount"></a>Ensimmäinenriviotsikkona ja skipLineCount käytön skenaariot

- Kopioidaan tekstitiedoston tiedoston lähteestä ja haluat lisätä rakenteen metatiedot sisältävä otsikkorivi (esimerkiksi: SQL-rakenteen). Määritä **Ensimmäinenriviotsikkona** tässä tapauksessa tulostus-tietojoukko tosi. 
- Otsikkorivi-tiedoston käsittelytoiminto, sisältävä tekstitiedosto kopioidaan ja haluat, että arvoviivat. Määritä **Ensimmäinenriviotsikkona** syötteen tietojoukossa tosi.
- Kopioidaan tekstitiedostosta ja haluat ohittaa alussa muutaman rivit, jotka eivät sisällä tietoja tai otsikon tietoja. Määritä **skipLineCount** osoittamaan ohitetaan rivien määrä. Jos tiedoston loppuosa on otsikkorivi, voit määrittää myös **Ensimmäinenriviotsikkona**. Jos **skipLineCount** ja **Ensimmäinenriviotsikkona** on määritetty, rivit ohitetaan ensin ja sitten otsikkotiedot luetaan syötteen tiedostosta

### <a name="specifying-avroformat"></a>AvroFormat määrittäminen
Jos muodossa on määritetty AvroFormat, sinun ei tarvitse määrittää typeProperties-osan Muotoile-osan ominaisuuksia. Esimerkki:

    "format":
    {
        "type": "AvroFormat",
    }

Käyttämään Avro Muotoile rakennetaulukko voidaan viitata [Apache rakenne opetusohjelma](https://cwiki.apache.org/confluence/display/Hive/AvroSerDe).

Huomaa seuraavat seikat:  

- [Monimutkaisten tietotyyppien](http://avro.apache.org/docs/current/spec.html#schema_complex) ei tueta (tietueiden arvoluettelot, taulukoita, kartat, liittoja ja kiinteä)

### <a name="specifying-jsonformat"></a>JsonFormat määrittäminen

Jos muodossa on määritetty **JsonFormat**, voit määrittää seuraavia **valinnaisia** ominaisuuksia **muoto** -kohdassa.

| Ominaisuus | Kuvaus | Pakollinen |
| -------- | ----------- | -------- |
| filePattern | Määritä tiedot tallennetaan JSON-tiedostoille mallia. Sallitut arvot ovat: **setOfObjects** ja **arrayOfObjects**. **Oletusarvo** on: **setOfObjects**. Katso jäljempänä osien lisätietoja näistä kuviot.| Ei |
| encodingName | Määritä koodausta nimi. Kelvollinen koodauksen nimiä luetteloon, tutustu: [Encoding.EncodingName](https://msdn.microsoft.com/library/system.text.encoding.aspx) ominaisuus. Esimerkki: windows-1 250 tai shift_jis. **Oletusarvo** on: **UTF-8**. | Ei | 
| nestingSeparator | Merkki, jota käytetään erottamaan sisäkkäisiä tasoja. Oletusarvo on ".' (piste). | Ei | 


#### <a name="setofobjects-file-pattern"></a>setOfObjects tiedoston kuvio

Kunkin tiedosto sisältää yksittäisen objektin tai rivi-erotettu/yhdistetään useita objekteja. Kun tämä asetus on valittu tulostus-tietojoukko, kopioi tehtävän tuottaa yhden JSON-tiedosto, jonka kunkin objektin (rivin erotetun) riviä kohden.

**yhtenä objektina** 

    {
        "time": "2015-04-29T07:12:20.9100000Z",
        "callingimsi": "466920403025604",
        "callingnum1": "678948008",
        "callingnum2": "567834760",
        "switch1": "China",
        "switch2": "Germany"
    }

**rivin erotetun JSON** 

    {"time":"2015-04-29T07:12:20.9100000Z","callingimsi":"466920403025604","callingnum1":"678948008","callingnum2":"567834760","switch1":"China","switch2":"Germany"}
    {"time":"2015-04-29T07:13:21.0220000Z","callingimsi":"466922202613463","callingnum1":"123436380","callingnum2":"789037573","switch1":"US","switch2":"UK"}
    {"time":"2015-04-29T07:13:21.4370000Z","callingimsi":"466923101048691","callingnum1":"678901578","callingnum2":"345626404","switch1":"Germany","switch2":"UK"}
    {"time":"2015-04-29T07:13:22.0960000Z","callingimsi":"466922202613463","callingnum1":"789037573","callingnum2":"789044691","switch1":"UK","switch2":"Australia"}
    {"time":"2015-04-29T07:13:22.0960000Z","callingimsi":"466922202613463","callingnum1":"123436380","callingnum2":"789044691","switch1":"US","switch2":"Australia"}

**ketjutetut JSON**

    {
        "time": "2015-04-29T07:12:20.9100000Z",
        "callingimsi": "466920403025604",
        "callingnum1": "678948008",
        "callingnum2": "567834760",
        "switch1": "China",
        "switch2": "Germany"
    }
    {
        "time": "2015-04-29T07:13:21.0220000Z",
        "callingimsi": "466922202613463",
        "callingnum1": "123436380",
        "callingnum2": "789037573",
        "switch1": "US",
        "switch2": "UK"
    }
    {
        "time": "2015-04-29T07:13:21.4370000Z",
        "callingimsi": "466923101048691",
        "callingnum1": "678901578",
        "callingnum2": "345626404",
        "switch1": "Germany",
        "switch2": "UK"
    }


#### <a name="arrayofobjects-file-pattern"></a>arrayOfObjects tiedosto kuvio. 

Kunkin tiedosto sisältää matriisin objekteja. 

    [
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        },
        {
            "time": "2015-04-29T07:13:21.0220000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "123436380",
            "callingnum2": "789037573",
            "switch1": "US",
            "switch2": "UK"
        },
        {
            "time": "2015-04-29T07:13:21.4370000Z",
            "callingimsi": "466923101048691",
            "callingnum1": "678901578",
            "callingnum2": "345626404",
            "switch1": "Germany",
            "switch2": "UK"
        },
        {
            "time": "2015-04-29T07:13:22.0960000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "789037573",
            "callingnum2": "789044691",
            "switch1": "UK",
            "switch2": "Australia"
        },
        {
            "time": "2015-04-29T07:13:22.0960000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "123436380",
            "callingnum2": "789044691",
            "switch1": "US",
            "switch2": "Australia"
        },
        {
            "time": "2015-04-29T07:13:24.2120000Z",
            "callingimsi": "466922201102759",
            "callingnum1": "345698602",
            "callingnum2": "789097900",
            "switch1": "UK",
            "switch2": "Australia"
        },
        {
            "time": "2015-04-29T07:13:25.6320000Z",
            "callingimsi": "466923300236137",
            "callingnum1": "567850552",
            "callingnum2": "789086133",
            "switch1": "China",
            "switch2": "Germany"
        }
    ]

### <a name="jsonformat-example"></a>JsonFormat Esimerkki

Jos sinulla on JSON-tiedosto, jonka Seuraava sisältö:  

    {
        "Id": 1,
        "Name": {
            "First": "John",
            "Last": "Doe"
        },
        "Tags": ["Data Factory”, "Azure"]
    }

ja haluat kopioida Azure SQL-taulukkoon seuraavanlainen: 

Tunnus  | Name.First | Name.Middle | Name.Last | Tunnisteet
--- | ---------- | ----------- | --------- | ----
1 | Teemu | Null | Doe | ["Tietojen Factory", "Azure"]

Syötteen tietojoukko JsonFormat sisältötyyppi määritellään seuraavasti: (osittainen määritelmä asianmukainen osia kanssa)

    "properties": {
        "structure": [
            {"name": "Id", "type": "Int"},
            {"name": "Name.First", "type": "String"},
            {"name": "Name.Middle", "type": "String"},
            {"name": "Name.Last", "type": "String"},
            {"name": "Tags", "type": "string"}
        ],
        "typeProperties":
        {
            "folderPath": "mycontainer/myfolder",
            "format":
            {
                "type": "JsonFormat",
                "filePattern": "setOfObjects",
                "encodingName": "UTF-8",
                "nestingSeparator": "."
            }
        }
    }

Jos rakenne ei ole määritetty, kopioi tehtävän objektiksi rakenteen oletusarvoisesti ja kopioi jokaisen asian. 

#### <a name="supported-json-structure"></a>Tuetut JSON-rakenne
Huomaa seuraavat seikat: 

- Kunkin objektin nimi-arvoa paria kokoelma on nyt yhdistetty yhden tietorivin taulukkomuodossa. Objektien voivat olla ylemmän tason ja voit määrittää, miten Litistä tietojoukko rakenne oletusarvoisesti upottamalla erottimella (.). Katso [JsonFormat Esimerkki](#jsonformat-example) edeltävän osan esimerkki.  
- Jos rakenne ei ole määritetty Data Factory-tietojoukko, kopioi tehtävän havaitsee ensimmäisen objektista rakenteen ja Litistä koko objekti. 
- Jos JSON-syöte on taulukko, kopioi tehtävän muuntaa koko matriisin arvo merkkijonon. Voit ohittaa [sarakkeen yhdistäminen tai suodattaminen](#column-mapping-with-translator-rules)avulla.
- Jos määritettynä on samannimiset samalla tasolla, kopioi tehtävän valitsee viimeistä.
- Ominaisuuksien nimien kirjainkoko on merkitsevä. Kaksi ominaisuudet, joilla on sama nimi, mutta erilaisia suolet käsitellään kahta erillistä ominaisuudet. 

### <a name="specifying-orcformat"></a>OrcFormat määrittäminen
Jos muodossa on määritetty OrcFormat, sinun ei tarvitse määrittää typeProperties-osan Muotoile-osan ominaisuuksia. Esimerkki:

    "format":
    {
        "type": "OrcFormat"
    }

> [AZURE.IMPORTANT] Jos kopioit ei ORC tiedostoja **nimellä-on** paikallisen ja pilven säilöjen tietoihin, sinun täytyy asentaa JRE 8 (Ajonaikaista Java-ympäristöä) yhdyskäytävän käyttämääsi laitteeseen. 64-bittinen yhdyskäytävän edellyttää JRE 64-bittistä ja 32-bittinen yhdyskäytävän edellyttää 32-bittinen JRE. Löydät [täältä](http://go.microsoft.com/fwlink/?LinkId=808605)molemmat versiot. Valitse haluamasi vaihtoehto.

Huomaa seuraavat seikat:

-   Monimutkaisten tietojen lajeja ei tueta (STRUCT, KARTAN, luettelot, unionin)
-   ORC tiedostossa on kolme [pakkaamisen liittyvät asetukset](http://hortonworks.com/blog/orcfile-in-hdp-2-better-compression-better-performance/): ei mitään, ZLIB-SNAPPY. Tietoja Factory tukee tietojen lukeminen ORC tiedosto jossakin näistä pakattu muodoista. Tiedostossa käytetään Tiivistys pakkauksenhallinta on metatiedoissa lukea tiedot. Kuitenkin ORC johtava kirjoitettaessa Data Factory valitsee ZLIB, joka on ORC oletusarvo. Tällä hetkellä ei ole vaihtoehto, jos haluat ohittaa tämän. 

### <a name="specifying-parquetformat"></a>ParquetFormat määrittäminen
Jos muodossa on määritetty ParquetFormat, sinun ei tarvitse Määritä typeProperties-osan Muotoile-osan ominaisuudet. Esimerkki:

    "format":
    {
        "type": "ParquetFormat"
    }

> [AZURE.IMPORTANT] Jos kopioit ei tilat tiedostoja **nimellä-on** paikallisen ja cloud tietoja tallennetuista tiedoista, sinun on asennettava JRE 8 (Ajonaikaista Java-ympäristöä) yhdyskäytävän käyttämääsi laitteeseen. 64-bittinen yhdyskäytävän edellyttää JRE 64-bittistä ja 32-bittinen yhdyskäytävän edellyttää 32-bittinen JRE. Löydät [täältä](http://go.microsoft.com/fwlink/?LinkId=808605)molemmat versiot. Valitse haluamasi vaihtoehto.

Huomaa seuraavat seikat:

-   Monimutkaisten tietojen lajeja ei tueta (kartta-LUETTELOSTA)
-   Tilat tiedostolla on seuraavat pakkaamisen liittyvät vaihtoehdot: ei mitään, SNAPPY, GZIP ja LZO. Tietoja Factory tukee tietojen lukeminen nämä pakattu muodoissa ORC tiedostosta. Se käyttää metatiedot pakkauksenhallintaa lukea tiedot. Kuitenkin kirjoitettaessa tilat tiedoston Data Factory valitsee SNAPPY, mikä on oletusarvo tilat-muodossa. Tällä hetkellä ei ole vaihtoehto, jos haluat ohittaa tämän ongelman. 

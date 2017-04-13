<properties
   pageTitle="Jakaa tietoja yleisesti DocumentDB | Microsoft Azure"
   description="Lisätietoja maailman asteikko geo replikoinnin ja automaattisesti palautus yleinen tietokantojen Azure DocumentDB täysin hallitun NoSQL tietokanta-palvelun avulla."
   services="documentdb"
   documentationCenter=""
   authors="kiratp"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="documentdb"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="kipandya"/>
   
   
# <a name="distribute-data-globally-with-documentdb"></a>Jakaa tietoja yleisesti DocumentDB

> [AZURE.NOTE] Yleinen jakautumisen DocumentDB tietokantoja on yleisesti saatavilla ja automaattisesti käyttöön juuri luomasi DocumentDB kaikki tilit. Yritämme yleinen käyttöönottaminen kaikkien asiakkaiden, mutta sillä välin voit halutessasi Yleinen osoitteisto käytössä tililläsi, [Ota yhteyttä tukeen](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) ja Microsoft ota se puolestasi nyt.

Azure DocumentDB on suunniteltu tarpeiden IoT sovellusten koostuva miljoonia yleisesti eri aikajaksoille laitteet ja internet-asteikon sovellukset, jotka toimittavat erittäin vastaa kokemukset käyttäjille kaikkialla maailmassa. Tietokannan järjestelmät yleisölle tarkoitetun haasteellista saavuttaa pieni viive access sovelluksen tiedot-useita maantieteellisistä alueista, joiden hyvin määritettyjen tietojen yhdenmukaisuutta ja käytettävyyden oikeudet. Yleisesti jaettu tietokanta-järjestelmää DocumentDB helpottaa tietojen yleinen jakautumisen tarjoamalla täysin hallitun, monille tietokannan tilit, jotka tarjoavat Tyhjennä kompromissien yhdenmukaisuuden, käytettävyys ja suorituskyvyn, kaikki vastaavat oikeudet kanssa välillä. DocumentDB tietokannan tilit tarjotaan suuren käytettävyyden, yksi numero ms viiveitä, useita [hyvin määritetyn yhdenmukaisuuden tasot] [consistency], läpinäkyvä alueellisen automaattisesti usean homing ohjelmointirajapinnan kanssa ja voi skaalata elastically yli maapallon siirtonopeuden ja tallennustilaa. 

  
## <a name="configuring-multi-region-accounts"></a>Monille tilien määrittäminen

Yli maapallon skaalata DocumentDB tilin määrittämisessä voidaan toteuttaa alle minuutissa [Azure portal](documentdb-portal-global-replication.md)kautta. Sinun on suoritettava on valita oikean yhdenmukaisuuden tason useita tuetut hyvin määritetyn yhdenmukaisuuden tasoa ja jokin muu luku Azure alueiden liittäminen tietokannan-tiliin. DocumentDB yhdenmukaisuuden tasot tarjoavat Tyhjennä kompromissien tietyn yhdenmukaisuuden on voimassa ja suorituskyvyn välillä. 

![DocumentDB on useita, hyvin määritelty (kevennetty) yhdenmukaisuuden mallien valittavana][1]

(Kevennetty) yhdenmukaisuuden mallien sopivat hyvin DocumentDB on useita, määritelty.

Oikea yhdenmukaisuuden-tason valitseminen määräytyy tietojen yhdenmukaisuuden takaa sovelluksen tarpeitasi. DocumentDB kopioi tiedot eri kaikki määritetyn alueilla ja takaa yhtenäisyyden, jonka olet valinnut tietokanta-tilillesi automaattisesti. 


## <a name="using-multi-region-failover"></a>Käyttämällä monille automaattisesti 

Azure DocumentDB on läpinäkyvä automaattisesti tietokannan asiakkaat voivat Azure alueille – uusia [usean homing API] [ developingwithmultipleregions] sovelluksen edelleen käyttää Looginen päätepiste ja on keskeytyksettä vikasietotilaa mukaan. Voit ohjataan automaattisesti, antamalla joustavasti Uudelleensijoita tietokannan tapahtuman jokin alueen mahdollista virheen ehdoista tilin ilmene, mukaan lukien sovelluksen, infrastruktuuri, palvelun tai alueellisen virheet (real tai Simuloitu). DocumentDB alueellisen virheen, jos palvelun epäonnistuu läpinäkyvä tietokanta-tilisi kautta ja sovelluksen jatkaa käyttämään tietoja menettämättä käytettävyyttä. Kun DocumentDB tarjoaa [99,99 % käytettävyys palvelutasosopimuksia][sla], voit testata sovelluksen end käytettävyys ominaisuudet simuloimalla alueellisen epäonnistui molemmille [ohjelmallisesti] [ arm] sekä Azure-portaalin kautta.


## <a name="scaling-across-the-planet"></a>Skaalaus maailman eri
DocumentDB voit valmistella siirtonopeuden ja tarjoaman DocumentDB valikoimien milloin tahansa skaalaus-tallennustila yleisesti tietokannan tilisi alueiden välillä erikseen. DocumentDB sivustokokoelman distributed yleisesti ja hallita kaikki alueet tietokannan tilisi automaattisesti. Jokin Azure alueet, joissa voit jaetaan tietokannan tilisi kokoelmien [DocumentDB-palvelu on käytettävissä][serviceregions]. 

Siirtonopeuden ostettu ja tilaa DocumentDB valikoimien, on automaattisesti valmisteltu kaikkien alueiden kaikissa tasaisesti. Näin sovelluksen saumattomasti skaalata yli maapallo [maksaa siirtonopeuden ja tallennustilaa, käytössäsi on jokaisen tunnin kuluessa][pricing]. Esimerkiksi jos on valmisteltu 2 miljoonaa RUs DocumentDB keräämistä varten, valitse kunkin tietokannan tilisi alueiden saa 2 miljoonaa RUs, että. Tämä on esitelty alla.

![Skaalauksen siirtonopeuden DocumentDB kokoelman yli neljä aluetta][2]

DocumentDB takaa < 10 ms lukemiseen ja < 15 ms kirjoittamiseen viiveet suurempia osoitteessa P99. Lue pyynnöt välisenä ajankohtana palvelinkeskuksen saraketunnuksen takaa [yhdenmukaisuuden vaatimukset valitsemasi][consistency]. Merkinnät ovat aina quorum vahvistetun paikallisesti, ennen kuin he ovat kuitata asiakkaille. Kirjoita alueen prioriteetti on määritetty tietokannan tileille. Kirjoita nykyisen alueen tilin toimii suurimman prioriteetin nimetty alue. Kaikki SDK: T läpinäkyvä reitittää tietokannan tilin kirjoituksia nykyisen kirjoitus-alueelle. 

Lopuksi, koska DocumentDB on kokonaan [rakenteen ympäristöstä riippumattomalla tavalla] [ vldb] -sinun tarvitse huolehtia hallinta tai päivittämisen rakenteita tai toissijaisia indeksejä useita palvelinkeskusten yli. [SQL-kyselyjä] [ sqlqueries] jatkaa työskentelyä sovellusten ja -Tietomallit jatkaa muodostaen. 


## <a name="enabling-global-distribution"></a>Yleinen osoitteisto ottaminen käyttöön 

Voit helpottaa tietojen paikallisesti tai yleisesti jaetaan joko liittämällä vähintään yksi Azure alueet, joiden DocumentDB tietokannan tili. Voit lisätä tai osien poistaminen tietokannan tiliisi milloin tahansa. Yleinen jakamisen ottaminen portaalin avulla, katso, [miten voit suorittaa DocumentDB yleinen tietokannan replikoiminen Azure-portaalissa](documentdb-portal-global-replication.md). Yleinen osoitteisto ohjelmallisesti käyttöön on artikkelissa [kehitysmaiden monille DocumentDB tilien kanssa](documentdb-developing-with-multiple-regions.md).

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja jakelutavoista tiedot yleisesti DocumentDB seuraavissa artikkeleissa:

* [Liikenteen ja tallennustilaa kokoelma valmistelu] [throughputandstorage]
* [Usean homing kautta REST API. .NET, Java tai Python solmu SDK: T] [developingwithmultipleregions]
* [DocumentDB yhdenmukaisuuden tasot] [consistency]
* [Käytettävyys palvelutasosopimuksia] [sla]
* [Tietokannan tilin hallinta] [manageaccount]

[1]: ./media/documentdb-distribute-data-globally/consistency-tradeoffs.png
[2]: ./media/documentdb-distribute-data-globally/collection-regions.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[pcolls]: documentdb-partition-data.md
[consistency]: documentdb-consistency-levels.md
[consistencytradeooffs]: ./documentdb-consistency-levels/#consistency-levels-and-tradeoffs
[developingwithmultipleregions]: documentdb-developing-with-multiple-regions.md
[createaccount]: documentdb-create-account.md
[manageaccount]: documentdb-manage-account.md
[manageaccount-consistency]: documentdb-manage-account.md#consistency
[throughputandstorage]: documentdb-manage.md
[arm]: documentdb-automation-resource-manager-cli.md
[regions]: https://azure.microsoft.com/regions/
[serviceregions]: https://azure.microsoft.com/en-us/regions/#services 
[pricing]: https://azure.microsoft.com/pricing/details/documentdb/
[sla]: https://azure.microsoft.com/support/legal/sla/documentdb/ 
[vldb]: http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf
[sqlqueries]: documentdb-sql-query.md


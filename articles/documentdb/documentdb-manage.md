<properties
    pageTitle="DocumentDB tallennustilan ja suorituskyvyn | Microsoft Azure" 
    description="Lisätietoja tietojen tallentaminen ja asiakirjojen säilyttäminen DocumentDB ja kuinka voit skaalata DocumentDB kapasiteetin tarpeiden sovelluksesi." 
    keywords="Asiakirjojen säilyttäminen"
    services="documentdb" 
    authors="syamkmsft" 
    manager="jhubbard" 
    editor="cgronlun" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/18/2016" 
    ms.author="syamk"/>

# <a name="learn-about-storage-and-predictable-performance-provisioning-in-documentdb"></a>Lisätietoja tallentamisesta ja ennakoitavissa suorituskyvyn valmistelu DocumentDB
Azure DocumentDB on täysin hallitun, skaalattava tiedostopohjaisia NoSQL tietokannan palvelu JSON asiakirjoissa. DocumentDB, jossa ei ole näennäiskoneiden vuokrata, ohjelmiston asentaminen tai näytön tietokannat. DocumentDB ylläpitämä ja valvoo jatkuvasti Microsoft insinöörien maailman luokan käytettävyyden, suorituskykyä ja tietojen suojaaminen aikana.  

Voivat aloittaa [tietokannan tilin](documentdb-create-account.md) luomalla DocumentDB ja [Azure portal](https://portal.azure.com/) [DocumentDB tietokannan](documentdb-create-database.md) . DocumentDB tietokantojen tarjotaan mittayksiköt SSD asemaa Suoritettaessa palautettua tallennustilan ja siirtonopeuden. Nämä yksiköt on valmisteltu [tietokannan kokoelmien luominen](documentdb-create-collection.md) tietokanta-tilin kanssa, joka voi skaalata ylös tai alas milloin tahansa sovelluksen vaatimukset täyttävän varattu siirtonopeuden valikoimien avulla. 

Jos sovelluksen ylittää oman varattu siirtonopeuden yhtä tai useaa loppu-pyynnöt on rajoitettu kohti sivustokokoelman välein. Tämä tarkoittaa, että jotkin sovelluksen pyynnöt välttämättä onnistu samalla, kun muut saattaa olla rajoitettu.

Tässä artikkelissa on yleiskatsaus resursseja ja arvot, jotka ovat käytettävissä hallinta kapasiteetin ja suunnitella tietojen tallentaminen. 

## <a name="database-account"></a>Tiliin
Kuin Azure tilaaja voit valmistella vähintään yksi DocumentDB tietokannan tilit ja hallita tietokannan resursseja. Jokaisen tilauksen on liitetty yhden Azure-tilauksen. 

DocumentDB tilejä voi luoda [Azure portal](documentdb-create-account.md)kautta tai käyttämällä [ARM-mallin tai Azure CLI](documentdb-automation-resource-manager-cli.md).

## <a name="databases"></a>Tietokannat
Yksittäisen DocumentDB tietokannassa voi olla käytännössä rajattoman määrän asiakirjan tallennustilan ryhmitelty. Kokoelmien antaa suorituskyvyn eristystaso – valikoimien on valmisteltu siirtonopeuden, joka ei ole jaettu ja muut sivustokokoelmat saman tietokannan tai tilin kanssa. DocumentDB tietokanta on joustavasti kokoinen, väliltä gigatavun TBs, Suoritettaessa palautettua asiakirjojen säilyttäminen ja valmistellun siirtonopeuden. Toisin kuin perinteisen RDBMS tietokannan DocumentDB tietokannan on määritetty ei yhteen tietokoneeseen ja voi olla useita koneet tai klustereiden.  

DocumentDB kuin on tarpeen skaalata sovellustesi, voit luoda lisää sivustokokoelmia tai tietokantoihin tai molemmat. Tietokantojen voi luoda [Azure portal](documentdb-create-database.md) tai jokin [DocumentDB SDK: T](documentdb-dotnet-samples.md)kautta.   

## <a name="database-collections"></a>Tietokannan sivustokokoelmat
Kunkin DocumentDB tietokannassa voi olla yksi tai useampi sivustokokoelmat. Kokoelmien toimii hyvin käytettävissä olevat tiedot osioiden asiakirjojen säilyttäminen ja käsittelyä varten. Valikoimien voit tallentaa asiakirjat erilaisten rakenteen käyttäminen. DocumentDB's automaattisen indeksoinnin ja kyselyn ominaisuuksien avulla voit helposti Suodata ja Hae tiedostot. Kokoelma on vaikutusalueen asiakirjan tallennustilan ja kyselyn suorittamista varten. Kokoelma on myös kaikki sen sisältämien asiakirjojen tapahtuman toimialue. Kokoelmien kohdistetaan siirtonopeuden Azure-portaalissa tai SDK: T arvon perusteella. 

Kokoelmien ovat automaattisesti osioinut vähintään yksi fyysiset palvelimet yhdeksi DocumentDB. Kun luot kokoelma, voit määrittää valmistellun siirtonopeuden myyntialueella pyynnön toisen ja osion avaimen ominaisuuden kohden. Tämän ominaisuuden arvoa käytetään DocumentDB osiot ja reitin pyynnöt kyselyjen kuten asiakirjojen jakamiseen. Osion avainarvon toimii myös tallennettujen toimintosarjojen ja käynnistimien tapahtuman reunaa. Valikoimien on varattu määrää nopeus tietyn kyseisen kokoelma, jolla ei ole jaettu muiden saman tilin sivustokokoelmat. Voit skaalata vuoksi sovelluksen sekä tallennustilan ja siirtonopeuden ulos. 

Kokoelmien voi luoda [Azure portal](documentdb-create-collection.md) tai jokin [DocumentDB SDK: T](documentdb-sdk-dotnet.md)kautta.   
 
## <a name="request-units-and-database-operations"></a>Pyydä yksiköt ja tietokannan toiminnot

Kun luot kokoelma, varata siirtonopeuden kannalta sekunnissa [pyynnön yksiköt (RU)](documentdb-request-units.md) . Sen sijaan pohtivat ja laitteistoresurssien hallinta, voit ajatella **pyynnön yksikön** yksittäisen mittaa resurssien tarvitse suorittaa eri tietokannan ja palvelun sovelluksen pyynnön. Luku 1 kt asiakirjan käyttää samaa 1 RU riippumatta tallennettu kokoelma tai samanaikaiset pyynnöt käynnissä samaan määrän kohteiden määrän. DocumentDB, mukaan lukien monimutkaisia toimintoja, kuten SQL-kyselyjä on varmasti ennakoitavissa RU arvo, joka voidaan määrittää kehittäminen milloin kaikki pyynnöt. Jos tiedät, että tiedostojen koosta ja kunkin toiminnon (lukuja, kirjoituksia ja kyselyt) tukemaan sovelluksen taajuus, voit valmistella siirtonopeuden sovelluksen tarpeiden tarkka summa ja skaalata tietokannan ylös ja alas, kun suorituskyvyn tarpeet muuttuvat. 

Valikoimien voidaan varata lohkot 100 pyynnön yksiköiden sekunnissa-pyyntö yksiköt sekunnissa miljoonia ylöspäin lähimpään sataan siirtonopeuden kanssa. Valmistellun siirtonopeuden voidaan säätää sivustokokoelman käsittelyn muuttaminen tarpeita sopeuduttava ja käyttää sovelluksen kuviot elinkaaren. Lisätietoja on artikkelissa [DocumentDB suorituskyvyn tasot](documentdb-performance-levels.md). 

>[AZURE.IMPORTANT] Kokoelmien ovat laskutettavan kohteita. Kustannus määräytyy valmistellun siirtonopeuden mitattuna pyynnön mittayksikkö sekunnissa gigatavuina kulutettu kokonaistallennustila sekä-kokoelman. 

Kuinka monta pyynnön yksikköä tietyn toiminnon, kuten Lisää, poista, kyselyn tai tallennetun toimintosarjan suorittamisen käyttää? Pyyntö yksikkö mittaa normitettu pyynnön käsittelyn kustannukset. Luku 1 kt asiakirjan on 1 RU, mutta pyyntö lisätään, korvaa tai poista samassa asiakirjassa tarjoaman Lisää käsittely-palvelusta ja siten enemmän pyynnön yksiköitä. Jokaisen palvelun vastaus sisältää mukautetun otsikon (`x-ms-request-charge`), joka ilmoittaa pyynnön yksiköt kulutettu pyynnön. Tämä ylätunniste on myös [SDK: T](documentdb-sdk-dotnet.md)kautta. .NET-SDK [RequestCharge](https://msdn.microsoft.com/library/azure/dn933057.aspx#P:Microsoft.Azure.Documents.Client.ResourceResponse`1.RequestCharge) on [ResourceResponse](https://msdn.microsoft.com/library/azure/dn799209.aspx) objektin ominaisuus. Jos haluat arvioida ennen kuin soitat yksittäisen siirtonopeuden tarpeitasi, voit [kapasiteetin suunnittelu](documentdb-request-units.md#estimating-throughput-needs) auttaa tämä arvio kanssa. 

>[AZURE.NOTE] Perusaikataulun 1 pyynnön yksikön 1 kt tiedoston vastaa yksinkertainen GET asiakirjan [Istunnon yhdenmukaisuuden](documentdb-consistency-levels.md)kanssa. 

Seuraavat seikat, jotka vaikuttavat pyynnön yksiköt kulutettu toiminnon vastaan DocumentDB tietokanta-tili on. Näitä tekijöitä ovat:

- Asiakirjan koko. Kun asiakirjan koot suurentaa kulutettu lukeminen tai tiedostoon tiedot myös kasvattaa yksiköt.
- Ominaisuuden määrä. Kaikki ominaisuudet, kirjoittaa asiakirjan kulutettu yksiköt indeksoinnin olettaen oletusarvon kasvattaa kuin ominaisuuden määrä kasvaa.
- Tietojen yhdenmukaisuuden. Kun käytät tietojen kelpoisuus tasot vahvan tai joka Staleness, yksikköä Lisää kulutettu voit lukea tiedostoja.
- Indeksoituja ominaisuuksia. Indeksi-käytäntö kunkin sivustokokoelman määrittää, mitkä ominaisuudet ovat indeksoituja oletusarvoisesti. Voit pienentää pyynnön yksikkö-kulutus rajoittamalla näytettävien indeksoitujen ominaisuuksien. 
- Asiakirjan indeksoinnin. Kunkin asiakirjan indeksoidaan automaattisesti oletusarvon mukaan voit käyttää vähemmän pyynnön yksiköt Jos et halua indeksoida jotkin tiedostojen.

Lisätietoja on artikkelissa [DocumentDB pyynnön yksiköt](documentdb-request-units.md). 

Kuten alla on taulukko, jossa näkyy kolme eri tiedoston kokoa (1 kt, 4 kt ja 64 Kilotavua) ja kaksi suorituskyvyn eri tasoilla valmistelu pyynnön yksikköä (500 lukee/toisen + 100 kirjoituksia/toisen ja 500 lukee/toisen + 500 kirjoituksia/toisen). Tietojen yhdenmukaisuuden on määritetty istunnossa ja indeksoinnin käytäntö on määritetty ei mitään.

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>Tiedoston koko</strong></p></td>
            <td valign="top"><p><strong>Lukee/toisen</strong></p></td>
            <td valign="top"><p><strong>Kirjoituksia/toisen</strong></p></td>
            <td valign="top"><p><strong>Pyydä yksiköt</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>1 KT</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 *1) + (100* 5) = 1 000 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>1 KT</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 *5) + (100* 5) = 3 000 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>4 KT</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 *1.3) + (100* 7) = 1,350 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>4 KT</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 *1.3) + (500* 7) = 4,150 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>64 KILOTAVUA</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 *10) + (100* 48) = 9,800 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>64 KILOTAVUA</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 *10) + (500* 48) = 29,000 RU/s</p></td>
        </tr>
    </tbody>
</table>

Kyselyitä, tallennettujen toimintosarjojen ja käynnistimien tarjoaman pyynnön yksiköt parhaillaan suoritettavia toimintoja monimutkaisuuden perusteella. Kun kehität sovelluksen, Tarkasta pyynnön maksu otsikko ja ymmärtää helpommin, miten kutakin toimintoa muissa pyynnön yksikön kapasiteetti.  


## <a name="choice-of-consistency-level-and-throughput"></a>Yhdenmukaisuuden taso ja siirtonopeuden valinta
Yhdenmukaisuuden oletustaso valinta vaikuttaa liikenteen ja viive. Voit määrittää yhdenmukaisuuden oletustaso sekä ohjelmallisesti ja palvelun Azure-portaalissa. Voit ohittaa yhdenmukaisuuden tasoa kohden pyynnön välein. Oletusarvon mukaan yhdenmukaisuuden taso määrittää **istunnon**, joka sisältää monotoninen luku/kirjoituksia ja lukea kirjoitus-oikeudet. Istunnon yhdenmukaisuuden soveltuvat erinomaisesti käyttäjän keskitettyä sovellukset ja tarjoaa ihanteellinen saldo, yhdenmukaisuutta ja suorituskyvyn valinnat.    

Ohjeita yhdenmukaisuuden-tason Azure-portaalissa muuttamisesta on artikkelissa [DocumentDB tilin hallinta](documentdb-manage-account.md#consistency). Tai Katso lisätietoja yhdenmukaisuuden tasojen [käyttäminen yhdenmukaisuuden tasot](documentdb-consistency-levels.md).

## <a name="provisioned-document-storage-and-index-overhead"></a>Valmistellun asiakirjan tallennustilan ja hakemisto katseltavan
DocumentDB tukee sekä yhden osion ja osioitua kokoelmien luomista. DocumentDB kunkin osion tukee jopa 10 gt: n Suoritettaessa palautettua tallennustilan. Asiakirjan tallennustilan 10 Gigatavun sisältää tiedostoja sekä tallennustilan indeksin. Oletusarvon mukaan DocumentDB sivustokokoelman on määritetty indeksoida kaikki asiakirjat automaattisesti tarvitsematta erikseen toissijainen indeksit- tai rakenne. Sovellusten DocumentDB mukaan tyypillinen indeksi katseltavan on 2-20 % välillä. Indeksoinnin DocumentDB käyttämä tekniikka varmistaa, että riippumatta ominaisuuksien arvoja, indeksi-katseltavan enintään yli 80 % oletusasetuksilla tiedostojen koosta. 

Oletusarvon mukaan kaikki tiedostot ovat indeksoituja DocumentDB mukaan automaattisesti. Kuitenkin voit halutessasi hienosäätää indeksi katseltavan voit poistaa tiettyjen asiakirjojen lisääminen tai korvaaminen asiakirjan, milloin indeksoinnin kuvatulla tavalla [DocumentDB indeksoinnin käytännöt](documentdb-indexing-policies.md). Voit määrittää DocumentDB kokoelman indeksointi kaikki asiakirjat kokoelmassa ulkopuolelle. Voit myös määrittää DocumentDB kokoelman indeksoida valikoivasti vain tiettyjä ominaisuuksia tai yleismerkkejä JSON-asiakirjojen verkkopolut kuvatulla tavalla [kokoelma indeksoinnin käytännön määrittäminen](documentdb-indexing-policies.md#configuring-the-indexing-policy-of-a-collection). Kirjoita siirtonopeuden – toisin sanoen voit käyttää vähemmän pyynnön yksiköt myös pois ominaisuudet tai asiakirjojen parantaa.   

## <a name="next-steps"></a>Seuraavat vaiheet

Lue lisää DocumentDB toiminta-kohdassa [osioimisen ja Azure DocumentDB skaalaus](documentdb-partition-data.md).

Katso ohjeet valvonnasta suorituskyvyn tasot Azure-portaalissa [näytön DocumentDB-tili](documentdb-monitor-accounts.md). Saat lisätietoja suorituskyvyn tasojen loppu valitseminen [suorituskyvyn tasojen DocumentDB](documentdb-performance-levels.md).
 

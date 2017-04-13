<properties 
    pageTitle="DocumentDB hierarkkisia resurssin mallin ja -käsitteistä | Microsoft Azure" 
    description="Lisätietoja DocumentDB's tietokantoja, sivustokokoelmat, käyttäjän määrittämiä funktioita (UDF), tiedostoja, resursseja ja paljon muuta käyttöoikeuksia hierarkkisten malli."
    keywords="Hierarkkinen mallin documentdb azure-Microsoft azure"   
    services="documentdb" 
    documentationCenter="" 
    authors="AndrewHoh" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/28/2016" 
    ms.author="anhoh"/>

# <a name="documentdb-hierarchical-resource-model-and-concepts"></a>DocumentDB hierarkkisia resurssin mallin ja käsitteitä

Tietokannan kohteiden, joka hallitsee DocumentDB kutsutaan **resurssit**. Kullekin resurssille tunnistetaan yksilöllisesti looginen URI. Voit käsitellä perustoiminnot HTTP- ja pyynnön ja vastauksen ylä- ja tilakoodin resurssit. 

Tässä artikkelissa lukemalla pystyt seuraaviin kysymyksiin:

- DocumentDB's resurssin mallin kuvaus
- Mitä ovat määritetty resurssien käyttäjän määrittämät resurssit verrattuna?
- Miten resurssin osoite?
- Miten kokoelmien toimii?
- Miten tallennettujen toimintosarjojen, käynnistimien ja käyttäjän määrittämät funktiot (UDF) toimii?

## <a name="hierarchical-resource-model"></a>Hierarkkinen resurssin malli
Kuin seuraavassa kaaviossa on kuvattu, DocumentDB hierarkkisia **resurssin mallin** koostuu joukot resurssien tietokantaan tili-kohdasta jokaisen käytettävissä loogisen tai vakaata URI kautta. Resurssien joukon viitataan kuin **syötteen** sisältö. 

>[AZURE.NOTE] DocumentDB on erittäin tehokkaasti TCP-protokolla, joka on myös RESTful sen viestintä mallissa [.NET asiakkaan SDK](https://msdn.microsoft.com/library/azure/dn781482.aspx)kautta.

![DocumentDB hierarkkisia resurssin malli][1]  
**Hierarkkinen resurssin malli**   

Voit aloittaa tiedostojesi käytön tiedoilla, sinun täytyy [DocumentDB tietokannan tilin luominen](documentdb-create-account.md) Azure-tilauksen käyttäminen. Tietokannan tilin voi olla **tietokannat**sisältävät useita **kokoelmien**, jokainen puolestaan sisältää joukko **tallennettujen toimintosarjojen, käynnistää UDF-tiedostojen** ja siihen liittyvät **liitteet** (esikatselu-toimintoa). Tietokannan on liitetty myös **käyttäjille**, molemmissa joukon **käyttöoikeudet** sivustokokoelmat, tallennettujen toimintosarjojen, käynnistimien, UDF, asiakirjat ja liitteet. Vaikka tietokantoja, käyttäjät, käyttöoikeudet ja sivustokokoelmat ovat tunnetun rakenteita järjestelmän resursseja, asiakirjat ja liitteet sisältävät haluamaansa, käyttäjän määrittämä JSON sisältöä.  

|Resurssi   |Kuvaus
|-----------|-----------
|Tiliin   |Tietokannan-tili on liitetty tietokantojen ja rahassa Blob-objektien tallennustilaan liitteiden (esikatselu-toimintoa). Voit luoda yhden tai useamman tietokanta-tilien Azure-tilauksen käyttäminen. Katso lisätietoja seuraavasta Microsoftin [hinnat sivun](https://azure.microsoft.com/pricing/details/documentdb/).
|Tietokannan   |Tietokanta on looginen säilön asiakirjan tallennustilaa osioitu kokoelmien yli. Kannattaa myös käyttäjien säilöä.
|Käyttäjän   |Loogisen nimitila vaikutusalueen käyttöoikeudet. 
|Käyttöoikeudet |Todennus-tunnuksen, liittyvät käyttäjältä tietyn resurssin käyttöoikeutta.
|Sivustokokoelman |Kokoelma on JSON asiakirjojen ja liittyvän JavaScript-sovelluksen logiikkaa säilö. Kokoelma on laskutettavan kokonaisuus, jossa [kustannukset](documentdb-performance-levels.md) määritetään kokoelman liittyvät suorituskyvyn tason mukaan. Kokoelmien voi olla yksi tai useampi osioiden/palvelin ja käsitellään käytännössä rajoittamaton tallennus- tai siirtonopeuden tietomääristä skaalata.
|Tallennettu toimintosarja   |Sovelluksen logiikkaa kirjoitettu JavaScript, joka on rekisteröity kokoelma ja muulla tavoin suoritetaan tietokantamoduuli.
|Käynnistin    |Sovelluksen logiikkaa kirjoitettu JavaScript suoritetaan ennen tai jälkeen joko Lisää, korvaa tai poistotoiminto.
|UDF    |Sovelluksen logiikkaa kirjoitettu JavaScript. UDF mahdollistavat malli mukautettua kyselyä-operaattori ja laajentaa siten core DocumentDB kyselykielen.
|Asiakirjan   |Käyttäjän määrittämät (haluamaansa) JSON sisältöä. Oletusarvon mukaan ei rakenne on määritettävä eikä toissijainen indeksien tarvitse kaikki asiakirjat kokoelmaan toimitettavat.
|(Ennakkoversio) Liite   |Liitteen on erityisen asiakirja, joka sisältää viittauksia ja ulkoiset blob/median metatietoja. Kehittäjä voi halutessaan on DocumentDB hallitsee Blob-objektien tai tallentaa sen ulkoisen blob-palveluntarjoajan, kuten OneDrive, Dropbox. 


## <a name="system-vs-user-defined-resources"></a>Järjestelmän ja käyttäjän määrittämät resurssit
Resurssit, kuten tietokannan tilit, kokoelmien, käyttäjät, käyttöoikeudet, tallennettujen toimintosarjojen, käynnistimien, ja UDF - kaikki on kiinteä rakenne ja kutsutaan järjestelmäresursseja. Sen sijaan resurssit, kuten asiakirjat ja liitteet on ei rajoituksia rakenne ja käyttäjän määrittämät resurssit esimerkkejä. Valitse DocumentDB, järjestelmä- ja käyttäjän määrittämät resurssien edustaa ja tuleeko vakio-yhteensopiva JSON. Kaikki resurssit-järjestelmän tai käyttäjän määrittämä, on seuraavat yhteiset ominaisuudet.

> [AZURE.NOTE] Huomaa, että kaikki järjestelmän luotu resurssin ominaisuudet ovat etuliite alaviivaa (_) niiden JSON kirjoitusasun mukaisena.

<table>
    <tbody>
        <tr>
            <td valign="top"><p><strong>Ominaisuus</strong></p></td>
            <td valign="top"><p><strong>Käyttäjän asetettavan tai järjestelmä?</strong></p></td>
            <td valign="top"><p><strong>Tarkoitus</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>_rid</p></td>
            <td valign="top"><p>Järjestelmä</p></td>
            <td valign="top"><p>Järjestelmän luoma yksilöllinen ja hierarkkisten resurssin tunnus</p></td>
        </tr>
        <tr>
            <td valign="top"><p>_etag</p></td>
            <td valign="top"><p>Järjestelmä</p></td>
            <td valign="top"><p>pakollinen optimistisen ohjausobjektin resurssin ETag</p></td>
        </tr>
        <tr>
            <td valign="top"><p>_ts</p></td>
            <td valign="top"><p>Järjestelmä</p></td>
            <td valign="top"><p>Viimeksi päivitetty aikaleima resurssin</p></td>
        </tr>
        <tr>
            <td valign="top"><p>_self</p></td>
            <td valign="top"><p>Järjestelmä</p></td>
            <td valign="top"><p>Yksilöivä käytettävissä URI resurssin</p></td>
        </tr>
        <tr>
            <td valign="top"><p>tunnus</p></td>
            <td valign="top"><p>Järjestelmä</p></td>
            <td valign="top"><p>Käyttäjän määrittämä yksilöllinen nimi resurssille (, sama osion avaimen arvo). Jos käyttäjä ei ole määritetty tunnusta, tunnus on järjestelmä</p></td>
        </tr>
    </tbody>
</table>

### <a name="wire-representation-of-resources"></a>Resurssien Wire esittäminen
DocumentDB määrätä minkä tahansa omistusoikeuksia tunnisteet JSON vakio- tai koodausten; se toimii vakio yhteensopiva JSON-tiedostoja.  
 
### <a name="addressing-a-resource"></a>Osoitteiden resurssi
Kaikki resurssit ovat käytettävissä URI. Resurssin **_self** -ominaisuuden arvo vastaa resurssin suhteellinen URI. Muodon muuttaminen URI koostuu /\<syötteen\>/ {_rid} polku osia:  

|Arvo _self |Kuvaus
|-------------------|-----------
|/DBS   |Syötteen tietokantojen tietokanta-tilissä
|/DBS/ {dbName}  |Tietokannan, jonka arvoa {dbName} vastaavat tunnus
|{dbName} /DBS/ /colls/   |Valitse tietokannan kokoelmien syöte
|/colls/ /DBS/ {dbName} {collName} |Sivustokokoelman, jonka arvoa {collName} vastaavat tunnus
|/colls/ /DBS/ {dbName} {collName} / asiakirjoja    |Valitse kokoelma asiakirjojen syöte
|/DBS/ {dbName} {collName} /colls/ /docs/ {Asiakirjatunnusta}    |Asiakirja, jossa arvoa {doc} vastaavat tunnus
|/DBS/ {dbName} / käyttäjät /   |Tietokannan käyttäjiä syöte
|/DBS/ {dbName} / käyttäjät / {käyttäjätunnus}   |Käyttäjä, jolla arvoa {user} vastaavat tunnus
|/DBS/ {dbName} / käyttäjät / {käyttäjätunnus} / käyttöoikeudet   |Käyttöoikeudet-kohdassa käyttäjän syöte
|/DBS/ {dbName} / käyttäjät / {käyttäjätunnus} /permissions/ {permissionId}    |Käyttöoikeus, jonka arvoa {käyttöoikeudet} vastaavat tunnus
  
Kullekin resurssille on yksilöivä käyttäjän määrittämä tarjoamia tunnus-ominaisuuden nimi. Huomautus: asiakirjojen, jos käyttäjä ei ole määritetty tunnusta, tuettu Microsoftin SDK: T luo automaattisesti yksilöllisen tunnuksen asiakirjan. Tunnus on käyttäjän määrittämä merkkijono, enintään 256 merkkiä, joka on yksilöivä tietyn ylätason resurssin kontekstissa. 

Kullekin resurssille on myös on järjestelmä hierarkkisia resurssitunnus (tunnetaan myös nimellä RID), joka on _rid-ominaisuuden kautta. RID käyttää annetun resurssin koko hierarkia ja se on kätevä sisäinen esitys käytettävä viite-eheyden hajautettu tavalla. Sisäisesti käyttää sitä DocumentDB tehokas reitittämiseen tarvitsematta cross osion haut RID on yksilöivä tietokanta-tili. Arvot _self ja _rid-ominaisuudet ovat sekä vaihtoehtoinen canonical resurssin esityksiä. 

DocumentDB REST API tukevat resurssien ja reititystä pyynnöt tunnus ja _rid ominaisuudet.

## <a name="database-accounts"></a>Tietokannan tilit
Voit valmistella vähintään yksi DocumentDB tietokannan tilit Azure-tilauksen käyttäminen.

Voit [luoda ja hallita DocumentDB tietokannan](documentdb-create-account.md) Azure-portaalissa osoitteessa [http://portal.azure.com/](https://portal.azure.com/)kautta. Luomisen ja ylläpidon tietokannan tilin edellyttää järjestelmänvalvojan oikeudet, voidaan suorittaa vain Azure tilauksen mukaisesti. 

### <a name="database-account-properties"></a>Tietokannan tilin ominaisuudet
Osana valmistelu ja tietokannan tilin hallinnan asetusten määrittäminen ja lue seuraavat ominaisuudet:  

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>Ominaisuuden nimi</strong></p></td>
            <td valign="top"><p><strong>Kuvaus</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>Yhdenmukaisuuden käytäntö</p></td>
            <td valign="top"><p>Tämän ominaisuuden määrittäminen tietokannan tili kokoelmien yhdenmukaisuuden oletustaso. Voit ohittaa yhdenmukaisuuden tasoa kohden pyynnön välein käyttämällä [x-ms-yhdenmukaisuuden-taso] pyynnön otsikko. <p><p>Huomaa, että tämä ominaisuus koskee vain <i>käyttäjän määrittämät resurssit</i>. Kaikki järjestelmän määritetyt resurssit on määritetty tukemaan lukuja ja kyselyt vahva yhdenmukaisuuden kanssa.</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Luvan näppäimet</p></td>
            <td valign="top"><p>Nämä ovat ensisijainen ja toissijainen pää- ja vain luku-tilassa näppäimet, jotka sisältävät kaikki resurssit tietokannan tilissä järjestelmänvalvojan oikeudet.</p></td>
        </tr>
    </tbody>
</table>

Huomaa, että lisäksi valmistelu, määrittäminen ja tietokannan tilin hallinta Azure-portaalista, voit luoda ja hallita DocumentDB tietokanta käyttämällä [Azure DocumentDB REST API](https://msdn.microsoft.com/library/azure/dn781481.aspx) sekä [asiakkaan SDK: T](https://msdn.microsoft.com/library/azure/dn781482.aspx)myös ohjelmallisesti.  

## <a name="databases"></a>Tietokannat
DocumentDB tietokanta on looginen säilön yksi tai useampi sivustokokoelmat ja käyttäjät, kuten seuraavassa kaaviossa on esitetty. Voit luoda haluamasi määrän tietokantoja DocumentDB tietokanta-tili, tarjouksen rajoitukset.  

![Tietokannan hierarkkisia tilin ja sivustokokoelmat-malli][2]  
**Tietokanta on looginen säilön käyttäjien ja kokoelmien**

Tietokannassa voi olla lähes rajoittamattoman asiakirjan tallennustilan osioinut sivustokokoelmat, jotka muodostavat niiden sisältämät asiakirjojen tapahtuman toimialueet. 

### <a name="elastic-scale-of-a-documentdb-database"></a>DocumentDB tietokannan joustavasti asteikon
DocumentDB tietokanta on oletusarvoisesti – väliltä muutaman gt petabytes Suoritettaessa palautettua asiakirjojen säilyttäminen ja valmistellun siirtonopeuden joustavasti. 

Toisin kuin perinteisen RDBMS tietokannan DocumentDB-tietokantaan ei ole määritetty yhteen tietokoneeseen. DocumentDB, jossa sovelluksen asteikko tarpeiden kasvaa, voit luoda lisää sivustokokoelmat tai tietokantoihin. Myös Microsoft eri ensimmäisen osapuolen sovelluksissa olet käyttänyt DocumentDB consumer mittakaava uudelleen luomalla erittäin suuri DocumentDB tietokantojen kunkin sisältävän tuhansia kokoelmien asiakirjan tallennustilan teratavua. Voit Suurenna tai Pienennä tietokannan lisäämällä tai poistamalla sivustokokoelmat sovelluksen asteikko ehtojen mukaan. 

Voit luoda minkä tahansa tietokannan veloittaa tarjous kokoelmien määrän. Valikoimien on Suoritettaessa palautettua säilytys- ja siirtonopeuden valmisteltu puolestasi valitun suorituskyvyn tason mukaan.

DocumentDB tietokanta on myös käyttäjien säilön. Käyttäjä--vuorostaan on looginen nimitila käyttöoikeudet on joukko, joka sisältää tarkasti rajattuja todennus ja kokoelmien, asiakirjat ja liitteet.  
 
DocumentDB resurssin mallin muita resursseja, joiden tietokannat voidaan luoda, korvataan, poistetut, lue tai numeroitu helposti käyttämällä [Azure DocumentDB REST API](https://msdn.microsoft.com/library/azure/dn781481.aspx) tai mitä tahansa [asiakkaan SDK: T](https://msdn.microsoft.com/library/azure/dn781482.aspx). DocumentDB takaa vahva yhtenäisyyden luettavaksi tai kyselyt tietokannan resurssin metatiedot. Tietokannan poistaminen automaattisesti varmistaa, että ei voi käyttää kaikkia sivustokokoelmia tai sisältämät käyttäjät.   

## <a name="collections"></a>Sivustokokoelmat
DocumentDB sivustokokoelman on JSON tiedostojen säilö. Kokoelma on myös mittayksikön tapahtumia ja kyselyjen asteikko. 

### <a name="elastic-ssd-backed-document-storage"></a>Joustavasti Suoritettaessa palautettua asiakirjojen säilyttäminen
Kokoelma on sisäisesti joustavasti – se automaattisesti kasvaa ja pienenee, kun lisäät tai poistat tiedostoja. Kokoelmien looginen resursseja ja voi olla fyysinen osioiden tai palvelimiin. Osioiden toiseen määrä määräytyy DocumentDB tallennustilan koko ja kokoelmaa valmistellun siirtonopeuden perusteella. DocumentDB jokaista osiota on kiinteä liittyy Suoritettaessa palautettua tallennustilaa ja suuren replikoida. Osion hallinta hallitsee täysin Azure DocumentDB ja sinun ei tarvitse monimutkaisia koodin kirjoittaminen ja osioiden hallinta. DocumentDB sivustokokoelmat ovat **käytännössä rajoittamaton** tallennustilan ja siirtonopeuden. 

### <a name="automatic-indexing-of-collections"></a>Automaattisen indeksoinnin kokoelmien
DocumentDB on tosi rakenteen vapaa-tietokantaan. Ei oletetaan, että tai minkä tahansa rakenteen edellyttäminen JSON-tiedostoja. Kun lisäät asiakirjojen kokoelmaa, DocumentDB indeksoi automaattisesti ne ja ne ovat käytettävissä kyselyä. Tiedostojen automaattisen indeksoinnin tarvitsematta rakenteen tai toissijaisia indeksejä on avaimen ominaisuuksien DocumentDB, ja kirjoita optimoitu, Lukitse vapaat ja log rakenne hakemiston ylläpito tekniikoita käyttöön. DocumentDB tukee kestävä äänenvoimakkuuden erittäin kirjoituksia aikana edelleen yhdenmukaisia kyselyt. Asiakirjan ja hakemisto tallennustilan käytetään kunkin sivustokokoelman käyttämä tallennustila laskemiseen. Voit hallita-tallennustilan ja suorituskyvyn valinnat liittyvät indeksoinnin määrittämällä kokoelma indeksoinnin käytäntöä. 

### <a name="configuring-the-indexing-policy-of-a-collection"></a>Kokoelma indeksoinnin käytännön määrittäminen
Valikoimien indeksoinnin käytännön avulla voit tehdä suorituskyky ja tallennustilaa valinnat liittyvät indeksoinnin. Indeksoinnin määritysten osana käytettävissä ovat seuraavat vaihtoehdot:  

-   Valitse, onko kokoelman indeksoi automaattisesti kaikkiin asiakirjoihin vai ei. Oletusarvon mukaan kaikki asiakirjat indeksoidaan automaattisesti. Voit valita automaattisen indeksoinnin poistaminen käytöstä ja lisää valikoivasti indeksiin vain tietyt asiakirjat. Vastaavasti valikoivasti voit jättää vain tietyt asiakirjat. Voit tehostaa automaattinen ominaisuuden on TOSI tai EPÄTOSI kokoelma indexingPolicy ja käyttämällä [x-ms-indexingdirective] pyynnön otsikon lisääminen, korvaaminen tai poistaminen asiakirjan.  
-   Valitse, haluatko lisätä tai poistaa tiettyjen polkujen tai kuvioita asiakirjoissa pois indeksistä. Voit tehostaa tätä asetusta includedPaths ja excludedPaths-kokoelma indexingPolicy tarpeen mukaan. Voit myös määrittää-tallennustilan ja suorituskyvyn valinnat alue- ja hash kyselyjen polkua kuviot varten. 
-   Valitse painikkeen välillä (yhdenmukaisia) ja asynkroninen (kuitata) indeksi-päivitykset. Oletusarvon mukaan indeksi päivitetään synkronoidusti kunkin Lisää, korvaa tai poista asiakirjan-kokoelmaan. Näin vastaamaan samalla yhdenmukaisuuden tasolla, joka lukee asiakirjan kyselyt. Kun DocumentDB on optimoitu kirjoittaminen ja tukee asiakirjan kirjoituksia sekä synkronisen indeksi ylläpito ja tarjota yhdenmukaista kyselyjen kestävä tietomääriä, voit määrittää tietyt sivustokokoelmat niiden hakemiston päivittäminen purjeveneestä. Viive-indeksointi osa edelleen kirjoittamisen suorituskykyä ja sopii joukkona nieltynä tilanteita, joissa ensisijaisesti luku näkyvä sivustokokoelmat.

Indeksoinnin käytännön voi muuttaa suorittamalla HYLLYTETTY-kokoelmaan. Tämä onnistuu [asiakkaan SDK](https://msdn.microsoft.com/library/azure/dn781482.aspx)- [Azure Portal](https://portal.azure.com) tai [Azure DocumentDB REST API](https://msdn.microsoft.com/library/azure/dn781481.aspx)kautta.

### <a name="querying-a-collection"></a>Kokoelma kyselyt
Kokoelma olevissa tiedostoissa voi olla haluamaansa rakenteita ja voit tehdä kyselyn kokoelma olevissa tiedostoissa, mutta estää kaikki rakenteen tai toissijaisen indeksien toimiston ulkopuolella. Voit tehdä kyselyn sivustokokoelman [DocumentDB SQL-syntaksi](https://msdn.microsoft.com/library/azure/dn782250.aspx), joka sisältää rich hierarkkisia, Relaatio- ja paikkatietojen operaattorit ja laajennettavuus JavaScript-pohjainen UDF kautta. JSON kieliopin sallii mallinnus JSON asiakirjoja puut tarroja nimellä solmut. Tämä hyödynnetään sekä DocumentDB's automaattisen indeksoinnin avulla sekä DocumentDB on SQL-kielioppia. DocumentDB-kyselykieltä koostuu kolmesta tärkeimmät ominaisuuksia:   

1.  Kyselyn toiminnot, jotka vastaavat luonnollisesti puun rakenteen mukaan lukien hierarkkisia kyselyt ja ennusteiden on pieni joukko. 
2.  Alijoukko, mukaan lukien yhdistelmä, suodattaminen, ennusteiden, koosteet ja itse liitokset relaatio-toimintoa. 
3.  Täysin JavaScript-pohjaisen UDF, jotka toimivat (1) ja (2).  

DocumentDB kyselyn mallin yrittää tasapaino toimintoja, tehokkuuden ja yksinkertaisuuden välillä. DocumentDB-tietokantamoduuli grafiikkatiedostomuotoja käännetään, ja suorittaa kyselyn SQL-lauseet. Voit tehdä kyselyn kokoelma [Azure DocumentDB REST API](https://msdn.microsoft.com/library/azure/dn781481.aspx) tai jokin [asiakkaan SDK: T](https://msdn.microsoft.com/library/azure/dn781482.aspx). .NET SDK tulee LINQ-palvelussa.

> [AZURE.TIP] Voit kokeilla DocumentDB ja suorittaa SQL-kyselyjä [Kyselyn leikkikenttä](https://www.documentdb.com/sql/demo)Microsoftin tietojoukko.

### <a name="multi-document-transactions"></a>Usean tiedoston tapahtumat
Tietokannan tapahtumia on turvallinen ja ennakoitavissa ohjelmoinnin mallin samanaikainen muutoksiin tietoihin. RDBMS kirjoittaa liiketoimintalogiikan perinteinen asennustapa on kirjoittaa **tallennettujen toimintosarjojen** ja/tai **käynnistimien** ja toimitetaan tietokantapalvelimen tapahtumien suorittamista varten. RDBMS sovelluksen ohjelmointi tarvitaan käsitellä erillisiä ohjelmoinnin kielillä: 

- (Ei tapahtumiin liittyviä) sovelluksen ohjelmointikielellä (kuten JavaScript, Python, C#, Java jne.)
- T-SQL-tapahtumien ohjelmointikieli, joka suoritetaan grafiikkatiedostomuotoja tietokanta

Laaja sitoutunut JavaScript- ja JSON suoraan tietokantamoduuli, jotka DocumentDB tarjoaa intuitiivisen ohjelmoinnin mallin suoritetaan JavaScript-pohjaisen sovelluksen logiikkaa suoraan tallennettujen toimintosarjojen ja käynnistimien sivustokokoelmat. Näin molemmat seuraavista toimista:

- Samanaikainen tehokas soveltaminen hallita, palautus, automaattinen indeksoinnin suoraan-tietokantamoduuli JSON objekti-kaaviot
- Ilmoittaminen luonnollisesti Hallintavuo, muuttujan rajauksen, varauksen ja integrointi poikkeuksen käsittely perusalkioiden tietokannan tapahtumia suoraan ohjelmointikielellä JavaScript kannalta

JavaScript-logiikkaa rekisteröity sivustokokoelman tasolla voi myöntää sitten tietokannan laskutoimituksia tietyn sivustokokoelman asiakirjat. DocumentDB rivittyy implisiittisesti mukaan JavaScript tallennetut toimintosarjat ja käynnistimet sisällä ympäröivä HAPPAMAN tapahtumia, joiden tilannevedoksen eristystaso koko kokoelman olevissa tiedostoissa. Sen suorittamisen aikana, jos JavaScript-koodia ilmoittaa poikkeuksen, valitse koko tapahtuma on keskeytetty. Tuloksena ohjelmoinnin malli on kuvattu yksinkertainen vielä tehokkaita. JavaScript-kehittäjille Hae "kestävät-ohjelmoinnin mallin käytettäessä niiden tuttuja kielen rakenteita ja kirjaston perusalkioiden edelleen.   

Voi suorittaa JavaScript suoraan saman osoitetilan kuin puskurin resurssivarantoon tietokantamoduuli mahdollistaa performant ja tapahtumien suorittamisen tietokannan toimintojen vastaan asiakirjojen kokoelmaa. Lisäksi DocumentDB tietokantamoduuli tekee JSON laaja sitoumusta ja JavaScript jättää pois kaikki impedanssin toisiaan sovelluksen tyyppi-järjestelmiä ja tietokanta.   

Kun olet luonut kokoelma, voit rekisteröidä tallennettujen toimintosarjojen ja käynnistimien UDF [Azure DocumentDB REST API](https://msdn.microsoft.com/library/azure/dn781481.aspx) tai jokin [asiakkaan SDK: T](https://msdn.microsoft.com/library/azure/dn781482.aspx)kokoelman kanssa. Rekisteröinnin jälkeen voit viitata ja suorita ne. Ota huomioon seuraavat tallennetun toimintosarjan kirjoitettu kokonaan JavaScript-seuraava koodi on kaksi argumenttia (tekijän nimi ja kirjan nimen) ja luo uuden asiakirjan, asiakirjan kyselyt ja päivittää sen – kaikki implisiittinen HAPPAMAN tapahtumasta. Milloin tahansa suorituksen aikana, jos JavaScript-poikkeus-virhe ilmenee, koko tapahtuman keskeyttää.

    function businessLogic(name, author) {
        var context = getContext();
        var collectionManager = context.getCollection();        
        var collectionLink = collectionManager.getSelfLink()
            
        // create a new document.
        collectionManager.createDocument(collectionLink,
            {id: name, author: author},
            function(err, documentCreated) {
                if(err) throw new Error(err.message);
                
                // filter documents by author
                var filterQuery = "SELECT * from root r WHERE r.author = 'George R.'";
                collectionManager.queryDocuments(collectionLink,
                    filterQuery,
                    function(err, matchingDocuments) {
                        if(err) throw new Error(err.message);
                        
                        context.getResponse().setBody(matchingDocuments.length);
                       
                        // Replace the author name for all documents that satisfied the query.
                        for (var i = 0; i < matchingDocuments.length; i++) {
                            matchingDocuments[i].author = "George R. R. Martin";
                            // we don’t need to execute a callback because they are in parallel
                            collectionManager.replaceDocument(matchingDocuments[i]._self,
                                matchingDocuments[i]);   
                        }
                    })
            })
    };

Asiakas voi "toimittaa" yllä JavaScript logiikan, tapahtumien suorittamisen kautta HTTP POST-tietokantaan. Saat lisätietoja HTTP menetelmillä [RESTful aktiviteettien DocumentDB resursseja](https://msdn.microsoft.com/library/azure/mt622086.aspx). 

    client.createStoredProcedureAsync(collection._self, {id: "CRUDProc", body: businessLogic})
       .then(function(createdStoredProcedure) {
            return client.executeStoredProcedureAsync(createdStoredProcedure.resource._self,
                "NoSQL Distilled",
                "Martin Fowler");
        })
        .then(function(result) {
            console.log(result);
        },
        function(error) {
            console.log(error);
        });


Huomaa, että koska tietokanta ymmärtää grafiikkatiedostomuotoja JSON ja JavaScript-ei ole järjestelmän tyyppiristiriidan, tai ei ole "OR määritys-koodin luominen tärkeä pakollinen.   

Tallennettujen toimintosarjojen ja käynnistimien vuorovaikutuksessa kokoelma ja asiakirjojen kokoelman kautta hyvin määritetyn objektimallia, joka paljastaa sivustokokoelman nykyisessä kontekstissa.  

DocumentDB sivustokokoelmat voidaan luoda, poistaa, lue tai numeroitu helposti käyttämällä joko [Azure DocumentDB REST API](https://msdn.microsoft.com/library/azure/dn781481.aspx) tai mitä tahansa [asiakkaan SDK: T](https://msdn.microsoft.com/library/azure/dn781482.aspx). DocumentDB on aina vahva yhdenmukaisuuden luettavaksi tai kyselyt kokoelma metatiedot. Kokoelma poistetaan automaattisesti varmistaa, että käytät asiakirjat, liitteet, tallennettujen toimintosarjojen, käynnistimien ja UDF sisältämät.   

## <a name="stored-procedures-triggers-and-user-defined-functions-udf"></a>Tallennettujen toimintosarjojen, käynnistimien ja käyttäjän määrittämät funktiot (UDF)
Edellisessä osassa kuvattua voit kirjoittaa sovelluksen logiikkaa suorittamiseen sisältyy tietokantamoduuli tapahtumasta suoraan. Sovelluksen logiikkaa voidaan kirjoittaa kokonaan JavaScript- ja voidaan mallintaa tallennetun toimintosarjan, käynnistimen tai UDF. Tallennetun toimintosarjan tai käynnistimen JavaScript-koodia Lisää, korvaa, poista, luku- tai kyselyn kokoelma asiakirjat. Sisällä UDF JavaScript-koodia ei voi toisaalta, lisääminen, korvaa tai tiedostojen poistaminen. UDF Luetteloi kyselyn tulosjoukon asiakirjat ja tuottaa toiseen tulosjoukon. Usean vuokraajan DocumentDB ottaa käyttöön tarkka mukaan varauksen resurssi-hallinnon. Kunkin tallennetun toimintosarjan, käynnistimen tai UDF saa kiinteä quantum käyttöjärjestelmän resursseja työnsä. Lisäksi tallennettujen toimintosarjojen, käynnistimien tai UDF ei voi linkittää vastaan ulkoisen JavaScript-kirjastojen ja ovat mustalla listalla, jos ne ylittävät niihin varattu resurssi budjetteja. Voit rekisteröidä, tallennettujen toimintosarjojen, käynnistimien tai UDF unregister kokoelma kanssa käyttämällä REST API.  Rekisteröinnin tallennetun toimintosarjan, käynnistimen tai UDF on valmiiksi käännetty ja tallennettu tavu koodi, joka saa suorittaa myöhemmin. Seuraavassa osassa osoittavat, miten voit DocumentDB JavaScript-SDK Rekisteröi ja suorita unregister tallennetun toimintosarjan, käynnistimen ja UDF. JavaScript-SDK on yksinkertainen paketti [DocumentDB REST API](https://msdn.microsoft.com/library/azure/dn781481.aspx)kautta. 

### <a name="registering-a-stored-procedure"></a>Tallennetun toimintosarjan rekisteröiminen
Tallennetun toimintosarjan rekisteröinti Luo uuden tallennetun toimintosarjan resurssin sivustokokoelman HTTP POST kautta.  

    var storedProc = {
        id: "validateAndCreate",
        body: function (documentToCreate) {
            documentToCreate.id = documentToCreate.id.toUpperCase();
            
            var collectionManager = getContext().getCollection();
            collectionManager.createDocument(collectionManager.getSelfLink(),
                documentToCreate,
                function(err, documentCreated) {
                    if(err) throw new Error('Error while creating document: ' + err.message;
                    getContext().getResponse().setBody('success - created ' + 
                            documentCreated.name);
                });
        }
    };
    
    client.createStoredProcedureAsync(collection._self, storedProc)
        .then(function (createdStoredProcedure) {
            console.log("Successfully created stored procedure");
        }, function(error) {
            console.log("Error");
        });

### <a name="executing-a-stored-procedure"></a>Suoritetaan tallennettu toimintosarja
Tallennetun toimintosarjan tehdään lähettämällä HTTP-POST vastaan aiemman tallennetun toimintosarjan resurssin siirtämällä parametrit pyynnön tekstissä kuvatulla tavalla.

    var inputDocument = {id : "document1", author: "G. G. Marquez"};
    client.executeStoredProcedureAsync(createdStoredProcedure.resource._self, inputDocument)
        .then(function(executionResult) {
            assert.equal(executionResult, "success - created DOCUMENT1");
        }, function(error) {
            console.log("Error");
        });

### <a name="unregistering-a-stored-procedure"></a>Rekisteröintiä tallennettu toimintosarja
Rekisteröintiä tallennetun toimintosarjan tehdään ainoastaan lähettämällä HTTP poistaa aiemmin tallennettu toimintosarja-resurssin vastaan.   

    client.deleteStoredProcedureAsync(createdStoredProcedure.resource._self)
        .then(function (response) {
            return;
        }, function(error) {
            console.log("Error");
        });


### <a name="registering-a-pre-trigger"></a>Vanhat käynnistimen rekisteröiminen
Käynnistimen rekisteröinti tapahtuu luomalla uuden käynnistimen resurssin sivustokokoelman HTTP POST kautta. Voit määrittää, jos käynnistin on muotoon tai toiminnon tyypin ja kirjaa käynnistimen voi liittää (kuten luominen, korvaa, poista tai kaikki).   

    var preTrigger = {
        id: "upperCaseId",
        body: function() {
                var item = getContext().getRequest().getBody();
                item.id = item.id.toUpperCase();
                getContext().getRequest().setBody(item);
        },
        triggerType: TriggerType.Pre,
        triggerOperation: TriggerOperation.All
    }
    
    client.createTriggerAsync(collection._self, preTrigger)
        .then(function (createdPreTrigger) {
            console.log("Successfully created trigger");
        }, function(error) {
            console.log("Error");
        });

### <a name="executing-a-pre-trigger"></a>Suoritetaan ennen käynnistin
Käynnistimen suorittamisen tehdään määrittämällä asiakirjan resurssin kautta pyynnön otsikko kirjaa, käyttöön ja poistaminen-pyynnön milloin olemassa olevan käynnistimen nimi.  
 
    client.createDocumentAsync(collection._self, { id: "doc1", key: "Love in the Time of Cholera" }, { preTriggerInclude: "upperCaseId" })
        .then(function(createdDocument) {
            assert.equal(createdDocument.resource.id, "DOC1");
        }, function(error) {
            console.log("Error");
        });

### <a name="unregistering-a-pre-trigger"></a>Rekisteröintiä vanhat käynnistin
Rekisteröintiä käynnistimen tehdään ainoastaan kautta varmenteiden HTTP poistaa aiemman käynnistimen resurssin vastaan.  

    client.deleteTriggerAsync(createdPreTrigger._self);
        .then(function(response) {
            return;
        }, function(error) {
            console.log("Error");
        });

### <a name="registering-a-udf"></a>UDF rekisteröiminen
UDF rekisteröinti tapahtuu luomalla uuden UDF resurssin sivustokokoelman HTTP POST kautta.  

    var udf = { 
        id: "mathSqrt",
        body: function(number) {
                return Math.sqrt(number);
        },
    };
    client.createUserDefinedFunctionAsync(collection._self, udf)
        .then(function (createdUdf) {
            console.log("Successfully created stored procedure");
        }, function(error) {
            console.log("Error");
        });

### <a name="executing-a-udf-as-part-of-the-query"></a>UDF osana kyselyn suorittaminen
UDF on määritetty osana SQL-kysely ja käytetään laajentaa core [SQL kyselykielen DocumentDB](https://msdn.microsoft.com/library/azure/dn782250.aspx)avulla.

    var filterQuery = "SELECT udf.mathSqrt(r.Age) AS sqrtAge FROM root r WHERE r.FirstName='John'";
    client.queryDocuments(collection._self, filterQuery).toArrayAsync();
        .then(function(queryResponse) {
            var queryResponseDocuments = queryResponse.feed;
        }, function(error) {
            console.log("Error");
        });

### <a name="unregistering-a-udf"></a>Rekisteröintiä UDF 
Rekisteröintiä UDF tehdään ainoastaan lähettämällä HTTP poistaa aiemman UDF resurssin vastaan.  

    client.deleteUserDefinedFunctionAsync(createdUdf._self)
        .then(function(response) {
            return;
        }, function(error) {
            console.log("Error");
        });

Vaikka edellä katkelmat osoittanut rekisteröinnin (JULKAISE), rekisteröinnin (pakollinen), luku-ja luettelo (HAE) ja suorittaminen (Kirjaa) [DocumentDB JavaScript SDK](https://github.com/Azure/azure-documentdb-js)kautta, voit käyttää myös [REST API](https://msdn.microsoft.com/library/azure/dn781481.aspx) - tai muita [asiakkaan SDK: T](https://msdn.microsoft.com/library/azure/dn781482.aspx). 

## <a name="documents"></a>Asiakirjojen
Voit lisätä, korvaa, poista, lukea, Luetteloi ja kyselyn kokoelman haluamaansa JSON-tiedostoista. DocumentDB määrätä, minkä tahansa rakenne ja ei edellytä toissijaisia indeksejä tukemiseksi kyselyt asiakirjojen kokoelman päälle.   

On todella avoinna olevan tietokannan palvelun DocumentDB ei varasto mitään erityistä tietotyypeistä (esimerkiksi päivämäärä-aika) tai tietyn koodausten JSON asiakirjoissa. Huomaa, että DocumentDB ei edellytä mitään erityistä JSON nimeämiskäytännön kodifioida asiakirjoissa; yhteydet DocumentDB SQL-syntaksi on hyvin tehokas hierarkkisia ja relaatio kyselyn kyselyn ja projektin tiedostoja ilman erityistä huomautukset tai kodifioida tiedostojen välisiä suhteita ei tarvitse operaattorit erottaa ominaisuudet.  
 
Kuin muita resursseja, joiden asiakirjoja voi luoda, korvattu, poistetut, luku, numeroidut ja kyselyn pohjana helposti käyttämällä REST API tai mitä tahansa [asiakkaan SDK: T](https://msdn.microsoft.com/library/azure/dn781482.aspx). Tiedoston poistaminen välittömästi vapauttaa kiintiön, vastaa kaikille sisäkkäisiä liitteitä. Asiakirjojen luku yhdenmukaisuuden taso seuraa yhdenmukaisuuden käytännön tietokanta-tilissä. Pyyntö kohti välein riippuen tietojen kelpoisuus mukaan sovelluksesi voi ohittaa tämän käytännön. Kun kysely suoritetaan asiakirjoja, lue yhdenmukaisuuden seuraa määrittää kokoelman indeksoinnin tilaa. Saat "yhtenäinen" Tämä seuraa asiakkaan yhdenmukaisuuden käytännön. 

## <a name="attachments-and-media"></a>Liitteet- ja media
>[AZURE.NOTE] Liite- ja media-resurssit ovat esikatselu-ominaisuuksia.
 
DocumentDB avulla voit tallentaa binaarinen BLOB/media DocumentDB tai oman remote media-kaupasta. Voit myös edustavat media kannalta erityisen tiedoston liitteen metatiedot. Liitettä DocumentDB on erityisen (JSON)-asiakirja, joka viittaa tallennettu muualle media/blob. Liitteen on yksinkertaisesti määräten asiakirjan, joka kuvaa media, median tallennustilaan tallennettuja metatiedot (esimerkiksi sijainti, tekijä jne.). 

Harkitse sosiaalisen luku-sovellusta, jolla käyttää DocumentDB tallentamiseen käsin kirjoitettuja huomautuksia ja metatietoja, mukaan lukien kommentit, korostaa kirjanmerkkejä, luokitusten, tykkäykset/mieltymyksistä liittyvää e-kirjan tietyn käyttäjän jne.   

-   Kirjan itse sisältö on tallennettu media muistiin joko DocumentDB tietokannan tililläsi tai remote media-kaupasta. 
-   Sovelluksen voi tallentaa kunkin käyttäjän metatietoja distinct-asiakirjana – esimerkiksi Esko 's metatiedot Kirja1 on tallennettu /colls/joe/docs/book1 käyttämät asiakirjassa. 
-   Valitsemalla käyttäjän annetun kirjan sisältösivuja liitteet tallennetaan vastaavan asiakirjan esimerkiksi /colls/joe/docs/book1/chapter1, valitse /colls/joe/docs/book1/chapter2 jne. 

Huomaa, että edellä olevissa esimerkeissä välittää resurssin hierarkian helpossa muodossa tunnukset avulla. Resurssit ovat käyttää kautta REST API kautta resurssien yksilölliset tunnukset. 

Liitteen _media-ominaisuus viittaa, joka hallitsee DocumentDB median media sen URI. DocumentDB varmistavat roskaa voit kerätä median, kun kaikki avoimet viittaus jätetään pois. DocumentDB Luo liitteen, kun lataat uuden media ja lisää osoittamaan lisätyn media _media automaattisesti. Jos haluat tallentaa mediatiedostojen remote blob-säilö hallinnassa on käytössä, voit (esimerkiksi OneDrive-tallennustilan Azure DropBox jne), voit silti käyttää liitteet viittaamaan mediatiedostojen. Tässä tapauksessa luodaan liitteen ja täytä _media-ominaisuudeksi.   

Muita resursseja, joiden liitteiden voidaan luoda, korvataan, poistetut, lue ja luetteloida helposti käyttämällä REST API tai mitä tahansa asiakkaan SDK: T. Tiedostot, joiden liitteet luku yhdenmukaisuuden taso seuraa yhdenmukaisuuden käytännön tietokanta-tilissä. Pyyntö kohti välein riippuen tietojen kelpoisuus mukaan sovelluksesi voi ohittaa tämän käytännön. Kun kyselyistä liitteitä, lue yhdenmukaisuuden seuraa määrittää kokoelman indeksoinnin tilaa. Saat "yhtenäinen" Tämä seuraa asiakkaan yhdenmukaisuuden käytännön. 
 
## <a name="users"></a>Käyttäjät
DocumentDB käyttäjä on looginen nimitilan ryhmittely käyttöoikeudet. DocumentDB käyttäjä voi vastata käyttäjän käyttäjätietojen hallintajärjestelmän tai valmiin sovelluksen roolin. Käyttäjän edustaa yksinkertaisesti DocumentDB, otetaan ryhmittämään kohdassa tietokannan käyttöoikeudet.   

Soveltamisesta usean vuokraajan-sovelluksessa voit luoda käyttäjien DocumentDB, joka vastaa todellisten käyttäjien tai sovelluksen alihallinnat. Voit luoda käyttäjän käyttöoikeudet, jotka vastaavat access päättää eri sivustokokoelmat, asiakirjat, liitteet, jne.   

Kuin sovellustesi skaalata käyttäjän kasvu kanssa, voit toteuttaa shard eri tapoja tietojen. Voit muovata jokaiselle käyttäjälle seuraavasti:   

-   Kunkin käyttäjän yhdistää tietokannan.
-   Kunkin käyttäjän yhdistää kokoelma. 
-   Useiden käyttäjien vastaava siirtymällä oma sivustokokoelman. 
-   Useiden käyttäjien vastaava Siirry kokoelmien joukko.   

Riippumatta siitä, voit valita tietyn sharding strategia malli todellisten käyttäjien käyttäjinä DocumentDB tietokannan ja liittää tietoa hakuindeksointi voidaan suojata kunkin käyttäjän käyttöoikeudet.  

![Käyttäjän sivustokokoelmat][3]  
**Sharding strategioita ja mallinnus käyttäjät**

Muita resursseja, kuten käyttäjien DocumentDB voidaan luoda, korvataan, poistetut, lue tai numeroitu helposti käyttämällä REST API tai mitä tahansa asiakkaan SDK: T. DocumentDB on aina vahva yhdenmukaisuuden luettavaksi tai kyselyt käyttäjän resurssin metatiedot. Se on osoittavat että käyttäjän poistaminen automaattisesti varmistaa, että et voi käyttää mitä tahansa sen sisältämät käyttöoikeudet. Vaikka DocumentDB vapauttaa käyttöön käyttöoikeudet kiintiön osana poistetulla käyttäjällä taustalla, poistetut käyttöoikeudet on käytettävissä välittömästi uudelleen voi käyttää.  

## <a name="permissions"></a>Käyttöoikeudet
Access ohjausobjektin kannalta resurssit tietokannan tilien, kuten tietokantoja, käyttäjien ja käyttöoikeuksien pidetään *järjestelmänvalvojan* resurssit jälkeen tarvitaan järjestelmänvalvojan oikeudet. Toisaalta resurssit, kuten asiakirjoja, liitteet, tallennettujen toimintosarjojen, käynnistimien ja UDF sivustokokoelmat suodatetut kohdassa tietyn tietokannan ja katsoa *sovelluksen resurssit*. Vastaa resurssien ja roolit, joilla on käyttää niitä (eli järjestelmänvalvoja ja käyttäjän) kahdentyyppisiä, luvan mallin määrittää kahdentyyppisiä *pikanäppäimiä*: *perustyylin* ja *resurssin avaimen*. Perustyyli-näppäin on osa tietokanta-tili ja on annettu developer (tai järjestelmänvalvoja) kuka on valmistelu tietokanta-tili. Perustyyli avain on järjestelmänvalvojan semantiikkaan liittyvien siten, että se voidaan sallivat sekä sovellus että järjestelmänvalvojan resurssien käytön. Sen sijaan Resurssiavain on hajautetun pikanäppäin, joka sallii *tietyn* sovelluksen resurssin käyttöoikeutta. Näin ollen sieppaa tietokannan käyttäjien ja käyttöoikeuksien käyttäjällä on tietyn resurssin (esimerkiksi sivustokokoelman, asiakirjan, liitteenä, tallennetun toimintosarjan, käynnistimen tai UDF) välinen suhde.   

Ainoa tapa hankkia resurssin näppäintä on kohdassa tietyn käyttäjän käyttöoikeuksia resurssin luominen. Huomaa, jotta he voivat luoda tai hakea käyttöoikeus, perustyyli näppäintä on esitettävä authorization-otsikko. Käyttöoikeuksien resurssin sitoo käyttöoikeuden ja käyttäjän resurssille. Kun olet luonut oikeudet resurssin, käyttäjän on vain esittämään jotta voit käyttää resurssi liitetty resurssi-näppäintä. Näin ollen resurssi-näppäintä, voi tarkastella oikeudet resurssin loogiset ja Järjestä esittäminen.  

Muita resursseja, joiden DocumentDB käyttöoikeudet voidaan luoda, korvataan, poistetut, lue tai numeroitu helposti käyttämällä REST API tai mitä tahansa asiakkaan SDK: T. DocumentDB on aina vahva yhdenmukaisuuden luettavaksi tai kyselyt käyttöoikeus metatiedot. 

## <a name="next-steps"></a>Seuraavat vaiheet
Lue lisää resurssien käsitteleminen käyttämällä HTTP-komentoja [RESTful aktiviteettien DocumentDB resursseja](https://msdn.microsoft.com/library/azure/mt622086.aspx).


[1]: media/documentdb-resources/resources1.png
[2]: media/documentdb-resources/resources2.png
[3]: media/documentdb-resources/resources3.png


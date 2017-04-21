#<a name="data-management-and-business-analytics"></a>Tietojen hallinta- ja Business Analytics

Hallintaa ja analysointia tiedot pilvipalveluun vain yhtä tärkeää muualla ennalleen. Azure tarjoaa ‑näkymässä voit tehdä tämän alueen tekniikoita relaatio- ja relaatio tietojen käsitteleminen. Tässä artikkelissa esitellään asetukset. 

##<a name="table-of-contents"></a>Sisällysluettelo      

- [Blob-objektien tallennustilaan](#blob)
- [Tietokannan Hallintajärjestelmään käytössä Virtual Machine](#dbinvm)
- [SQL-tietokantaan](#sqldb)
    - [SQL-tietojen synkronointi](#datasync)
    - [Tietoja SQL Reporting näennäiskoneiden käyttäminen](#datarpt)
- [Taulukkotallennus](#tblstor)
- [Hadoop](#hadoop)

## <a name="blob"></a>Blob-objektien tallennustilaan

Sana "blob" on lyhyt "BLOB-objekti- ja kuvataan täsmälleen mitä blob on: kokoelma binaarinen tietoja. Vielä vaikka ne ovat yksinkertainen-BLOB-objektit on aivan hyötyä. [Kuva 1](#Fig1) havainnollistaa Azure-Blob-säiliö perusteet.

<a name="Fig1"></a>![Kaavio BLOB-objektit][blobs]
 
**Kuva 1: Azure-Blob-säiliö tallentaa binaaritietoja - BLOB - säilöt.**

Jos haluat käyttää BLOB-objektit, luot Azure- *tallennustilan tilin*. Osana Määritä Azure palvelinkeskukseen, joka tallentaa luomasi tällä tilillä objektit. Kaikkialla, missä se sijaitsee kunkin blob luot kuuluu joitakin tilisi tallennustilan säilö. Voit käyttää blob-sovellus on lomakkeen URL-osoite:

http://&lt;*StorageAccount*&gt;.blob.core.windows.net/&lt;*säilö*&gt;/&lt;*BlobName*&gt;

&lt;*StorageAccount* &gt; on yksilöllinen, kun uusi tallennustilan-tili on luotu, kun &lt; *säilö* &gt; ja &lt; *BlobName* &gt; ovat tietyn säilön ja Blob-objektien kyseisen säilön nimet. 

Azure on kahdenlaisia BLOB-objektit. Vaihtoehdot:

- *Estä* BLOB, joihin voi olla enintään 200 gigatavua tietoa. Kuin nimessä ehdottaa estä Blob-objektien jaettu joidenkin lohkot määrä. Virheen ilmetessä aikana estä Blob-objektien siirtäminen Uudelleenlähetyksen voit jatkaa viimeisimmän estä sen sijaan, että koko Blob-objektien lähettäminen uudelleen. Estä BLOB-objektit on aivan yleisen lähestymistavan säilöön, ja ne ovat yleisimmin käytettyjä blob-tyypin tänään.

- *Sivun* BLOB, joka voi olla mahdollisimman suuri, yhden teratavun. Sivun BLOB käsitellään RAM ja siten yksitellen on jaettu joidenkin sivujen määrä. Sovellus on ilmainen lukemiseen ja kirjoittamiseen yksittäisiä sivuja satunnaisesti blob. -Azuren näennäiskoneiden, esimerkiksi luot VMs käyttäminen sivun BLOB pysyvän säilön OS levyille ja tietojen levyjä.

Käytätkö estä BLOB tai sivun BLOB-sovellukset voi käyttää Blob-objektien tietoja usealla eri tavalla. Vaihtoehdot ovat seuraavat:

- Suoraan RESTful (eli HTTP-pohjainen) käyttää protokollaa. Azure sovellukset ja ulkoisiin sovelluksiin, mukaan lukien sovellusten käytössä paikallinen, voit käyttää tätä vaihtoehtoa.
- Käytä Azure-tallennustilan asiakas-kirjastoon, joka tarjoaa Lisää developer soveltuvia käyttöliittymän, päälle raaka RESTful blob-protokollaa. Jälleen Azure sovellukset ja ulkoisiin sovelluksiin voi käyttää BLOB tämän kirjaston käyttöä.
- Voit käyttää Azure asemat-asetusta, jolla Azure sovelluksen Käsittele sivun Blob-objektien kanssa NTFS-tiedostojärjestelmää paikalliseen asemaan. Sovellusten sivun Blob-objektien näyttää tavallisen Windowsin tiedostojärjestelmässä, käyttää vakio tiedoston i/o. Itse asiassa lukee ja kirjoittaa lähetetään pohjana olevan sivun Blob-objektien, joka sisältää Azure-asema. 

Laitteiston virheiden ehkäisemiseksi ja käytettävyyden parantaminen, joka blob replikoida Azure palvelinkeskuksen kolme tietokoneiden välillä. Kirjoittaminen blob päivittää kaikki kolme kopiot, joten myöhemmin lukee ei ole näkyvissä epäyhtenäisiä tuloksia. Voit myös määrittää, että Blob-objektien tiedot kopioidaan toiseen Azure palvelinkeskukseen saman geo mutta vähintään 500 Mailia poissa. Tämä kopioiminen, kutsutaan *geo replikointi*tapahtuu päivitys muutaman minuutin kuluessa blob ja se on hyödyllinen palauttaminen.

Tietojen BLOB myös voidaan luoda Azure *Sisällön toimituksen (CDN)*kautta. Tallentamalla välimuistiin Blob-objektien tietoja eri puolilla maailmaa palvelinten kymmeniä kopiot CDN voit nopeuttaa access tietoihin, joita voi käyttää toistuvasti. 

Yksinkertainen sellaisina kuin ne ovat-BLOB-objektit ovat kannattaa valita seuraavissa tilanteissa. Tallentaminen ja streaming ääni- ja ovat selkeitä esimerkkejä ovat varmuuskopiot ja muita tietoja arkistointi. Kehittäjät voivat käyttää myös BLOB pitoon erimuotoisia tietoja ne, kuten mitä tahansa. Voit tallentaa ja käyttää binaaritietoja selkeä tapa ottaa voivat olla hyödyllisiä Pikamääritys.


## <a name="dbinvm"></a>Tietokannan Hallintajärjestelmään käytössä Virtual Machine

Useiden sovellusten riippuvaisia tänään jokin lisätoimi tietokannan hallintajärjestelmän (DBMS). Suhteellinen järjestelmien, kuten SQL Server on useimmin käytetyt valinta, mutta muista kuin relaatiotietokannoista tavoista, yleisesti tunnettu *NoSQL* tekniikoita, saat päivittäin suositumpi. Azuren näennäiskoneiden avulla kertoaksesi cloud sovellusten näiden tietojen hallinta-asetusten avulla voit suorittaa tietokannan Hallintajärjestelmään (relaatio tai NoSQL)-AM. [Kuva 2](#Fig2) näkyy, miltä tämä näyttää SQL Serverin kanssa.

<a name="Fig2"></a>![SQL Server Virtual Machine kaavio][SQLSvr-vm]
 
**Kuva 2: Azuren näennäiskoneiden avulla tietokannan Hallintajärjestelmään käytössä AM-BLOB myöntämä pysyvyyttä kanssa.**

Kehittäjät ja tietokantojen järjestelmänvalvojille tässä skenaariossa muistuttaa paljon käynnissä saman ohjelmiston omia palvelinkeskukseen. Seuraavassa esimerkissä esimerkiksi lähes kaikki SQL Server-ominaisuuksia voi käyttää, ja sinulla on täydet järjestelmänvalvojan käyttöoikeudet järjestelmään. Sinulla on myös vastuussa hallinta tietokantapalvelin, aivan kuin se on käytössä paikallisesti.

Kuten [kuvassa 2](#Fig2) on tietokantoja näkyvät tallennetaan palvelimen käytetään AM paikallisen levyllä. Valitse kattaa kuitenkin kaikkien näiden levyjen kirjoitetaan Azure-blob. (Se on samanlainen käyttämään SAN-Blob-objektien, kuten LUN visuaalisessa paljon omia joten.) Kun kaikki Azuren Blob-objektien kanssa sen sisältämät tiedot on replikoida kolme kertaa sisällä palvelinkeskuksen ja jos pyydät se geo replikoida toiseen palvelinkeskukseen samalla alueella. On myös mahdollista käyttää asetuksia, kuten SQL Server tietokantapeilaus käyttövarmuuden varten. 

Toinen tapa käyttää SQL Server AM on hybrid-sovelluksen luominen kohtaa, johon tiedot sijaitsee Azure sovelluksen logiikkaa suorittamisen paikallisen aikana. Tämä esimerkiksi tehdä mielessä, kun sovellukset useisiin sijainteihin tai eri mobiililaitteet jakavat samat tiedot. Jotta välisen cloud tietokanta ja paikallisen logiikan yksinkertaisempi organisaatiossa, voit käyttää Azure Virtual Network luomiseen näennäisen yksityisverkon (VPN) Azure palvelinkeskuksen ja paikallisen omassa palvelinkeskuksen välinen yhteys.


## <a name="sqldb"></a>SQL-tietokantaan

Usean käyttäjän tietokannan Hallintajärjestelmään käytössä AM on ensimmäinen vaihtoehto, jotka tulevat mieleesi pilveen jäsenneltyjen tietojen hallintaa varten. Se ei ole vain valinta, vaikka, ei ole myöskään aina paras valinta. Joissakin tapauksissa käyttäen ympäristö Service (PaaS) lähestymistapa tietojen hallinta mahdollistaa kannattaa. Azure on PaaS-tekniikkaa SQL-tietokanta, jonka avulla voit tehdä tämän relaatiotietoja. [Kuva 3](#Fig3) on kuvattu tämän asetuksen. 

<a name="Fig3"></a>![Kaavio SQL-tietokantaan][SQL-db]
 
**Kuva 3: SQL-tietokanta on jaettu PaaS relaatio tallennuspalvelu.**

SQL-tietokantaan ei anna kunkin asiakkaan omassa SQL Serverin fyysinen esiintymään. Sen sijaan se sisältää usean vuokraajan palvelun, kunkin asiakkaan looginen SQL-tietokanta palvelimen kanssa. Kaikkien asiakkaiden jakaa suorittaminen ja tallennustilaa kapasiteettia, joka sisältää palvelun. Ja -Blob-säiliö, jossa SQL-tietokantaan kaikki tiedot on tallennettu Azure palvelinkeskukseen, jossa tietokantoja valmiin suuren käytettävyyden (HA) sisällä kolme eri tietokoneissa.

Sovellukseen SQL-tietokantaan muistuttaa paljon SQL Server. Sovellusten voit antaa vastaan relaatio taulukoita SQL-kyselyjä, T-SQL-toimintosarja toimintaohjeita ja suorita tapahtumat useiden taulukoiden välillä. Ja sovellusten käyttää SQL-tietokanta käyttämällä taulukkomuotoinen tietojen Stream (TKJ)-protokollaa, SQL Server käyttää samaa protokollaa käyttäjät voivat käsitellä tietoja käyttämällä kohteen Framework, ADO.NET, JDBC ja muut tutut tiedot access-rajapintojen. 

Mutta koska SQL-tietokanta on käynnissä Azure tietojen keskikohdan mukaan pilvipalveluun, sinun ei tarvitse tehdä mitään järjestelmän fyysinen ominaisuuksia, kuten levyn käytön hallinta. Voit myös ei tarvitse huolehtia ohjelmiston päivittäminen tai alemman tason hallintatehtäviä käsittely. Kunkin asiakkaan organisaation ohjaa edelleen, mukaan lukien niiden rakenteita ja käyttäjä-kirjautumiset-omassa tietokantoja, mutta monet hankalista järjestelmänvalvojan tehtävät ovat valmiiksi. 

Kun SQL-tietokantaan näyttää paljon kuten SQL Server-sovellukset, se ei toimia täsmälleen sama kuin fyysisiä tai virtuaalikoneen tietokannan Hallintajärjestelmään. Koska se suoritetaan jaetun laite, suorituskykyyn vaihtelevat kuormituksen sijoitettu kyseisen laitteen kaikkien asiakkaiden kanssa. Tämä tarkoittaa, että suorittaessaan Yammer-SQL-tietokantaan tallennetun toimintosarjan saattavat vaihdella yhden päivän toiseen. 

Nykyään SQL-tietokannan avulla voit luoda tietokannan pitämällä enintään 150 gigatavua. Jos haluat käsitellä suurempi tietokantoja, palvelun tarjoaa *Federation*toiminto. Voit tehdä tämän tietokannan järjestelmänvalvoja luo vähintään kaksi *federation jäsenet*, joista jokaisella on oma rakenteen käyttäminen erillisessä tietokanta. Tiedot on jaettu nämä jäsenien jotakin, mitä viitataan usein nimellä *sharding*kunkin jäsenelle määritetään yksilöivä *federation-näppäintä*. -Sovelluksen ongelmat SQL-kyselyjä vastaan määrittämällä federation avainta, joka yksilöi federation jäsenen kyselyn tiedot olisi kohde. Näin perinteinen relaatio menetelmän käyttämisestä suurista tietomääristä. Tavalliseen tapaan on valinnat; kyselyjen eikä tapahtumia voi olla federation jäsenet-esiintymän. Mutta kun nämä valinnat on hyväksyttävä relaatio PaaS palvelu on paras valinta, käyttäen SQL Federation voi olla hyvä ratkaisu.

SQL-tietokanta voidaan käyttää sovellusten Azure tai muualla, kuten paikallisen joten. Tämä on hyötyä cloud-sovelluksissa, jotka on relaatiotietoja sekä paikallisen sovelluksia, jotka voit hyötyä tietojen tallentaminen pilveen. Mobile-sovelluksen voi luottaa SQL-tietokantaan, voit hallita jaettua relaatiotietoja, esimerkiksi voi irtaimisto-sovellus, joka suoritetaan useita jälleenmyyjät eri puolilla maailmaa.

SQL-tietokantaan Pohdi nostaa ilmeisimmät (ja tärkeitä) ongelma: olet käyttötilanne SQL Server AM ja kun on SQL-tietokanta tallentaa? Tavalliseen tapaan on valinnat ja siten hallintatavan valitseminen paremmalta määräytyy tarpeitasi. 

Yksi helppo tapa Ajattele on saada SQL-tietokanta on uusi sovellusten samalla, kun SQL Server AM on kätevä vaihtoehto, kun siirrät aiemmin luotua paikallisen sovelluksen pilveen. Myös voi olla hyödyllistä, jotta voit tarkastella päätökseen Lisää tarkasti rajattuja tavalla, mutta. Esimerkiksi SQL-tietokanta on helpompi käyttää, koska rajoitustarve on pieni asetukset ja hallinta. Mutta käynnissä SQL Server AM voi olla aiempaa helpommin ennakoitavia suorituskyky - ei ole jaettu - palvelu ja se tukee myös suurten ei ole liitetty tietokantojen kuin SQL-tietokantaan. Silti SQL-tietokanta sisältää valmiita replikoinnin tietoja ja käsittely-tehokkaasti suojausvalintaikkuna suuren käytettävyyden tietokannan Hallintajärjestelmään hyvin vähän työlle. Kun SQL Serverin avulla voit hallita ja hieman laajempi tietyt asetukset, SQL-tietokanta on helpompaa määritetty ja merkittävästi vähemmän työtä hallintaan.

Lopuksi on tärkeää osoittaa, että SQL-tietokanta ei ole vain PaaS tietojen palvelun Azure käytettävissä. Microsoft käytettävissä on myös muita vaihtoehtoja. Esimerkiksi ClearDB on MySQL-PaaS ojentamassa, kun Cloudant myy NoSQL-vaihtoehto. PaaS tietopalvelut ovat oikean ratkaisun seuraavissa tilanteissa ja siten tätä tapaa tietojen hallinta on tärkeä osa Azure.


### <a name="datasync"></a>SQL-tietojen synkronointi

Kun SQL-tietokanta säilyttää kolme kopiota tietokantojen sisällä yhden Azure palvelinkeskukseen, se ei replikoida tiedot automaattisesti Azure palvelinkeskusten välillä. Sen sijaan se sisältää SQL-tietojen synkronointi-palvelu, jonka avulla voit tehdä tämän. [Kuva 4](#Fig4) näkyy, miltä tämä näyttää.

<a name="Fig4"></a>![SQL-tietojen synkronointi kaavio][SQL-datasync]
 
**Kuva 4: SQL-tietojen synkronointi tietojen muiden Azure SQL-tietokannan tietojen synkronointi ja paikallisen palvelinkeskusten.**

Kun kaavio näyttää SQL-tietojen synkronointi voit synkronoida tiedot eri sijainneissa. Oletetaan, että käytössäsi on useita Azure palvelinkeskusten sovelluksen esimerkiksi SQL-tietokantaan tallennettuja tietoja. SQL-tietojen synkronointi avulla, että tietojen synkronoiminen. SQL-tietojen synkronointi synkronoida myös Azure palvelinkeskuksen ja SQL Serveriä paikallisen joten esiintymä tietojen. Tämä voi olla hyötyä ylläpidon sekä paikallisen sovellusten käyttämiä tietoja paikallisen kopion ja käyttää Azure sovellusten cloud kopio. Ja vaikka se ei näy kuvassa, SQL-tietojen synkronointi voi käyttää myös SQL-tietokantaan ja SQL Serveriä AM Azure tai muualla-tietojen synkronointi.

Synkronointi voi olla kaksisuuntaisen ja voit määrittää täsmälleen tietoja synkronoidaan ja kuinka usein se on valmis. (Synkronoinnin tietokantoja ei ole atominen, kuitenkin - on aina vähintään joitakin viive.) Ja kuitenkin sitä käytetään SQL-tietojen synkronointi-synkronoinnin määrittäminen on täysin määritysten perustuva; ei ole koodia kirjoittamiseen.


### <a name="datarpt"></a>Tietoja SQL Reporting näennäiskoneiden käyttäminen

Tietokannan sisältää tietoja, kun joku todennäköisesti kannattaa luoda raportteja tiedot. Azure suorittamalla SQL Server Reporting Services (SSRS)-Azuren näennäiskoneiden, joka vastaa toiminnaltaan käynnissä SQL Server Reporting Services-ympäristöön. Voit sitten suorittaa raportteja Azure SQL-tietokantaan tallennettuja tietoja SSRS.  [Kuva 5](#Fig5) näyttää prosessin toiminta.

<a name="Fig5"></a>![Kaavio SQL raportointi][SQL-report]
 
**Kuva 5: SQL Server Reporting Services käynnissä-Azuren näennäiskoneiden on raportointipalveluiden tietojen SQL-tietokantaan. .**

Ennen kuin käyttäjä voi tarkastella raportin, joku määrittää, että raporttiin pitäisi millaiseksi (vaihe 1). SSRS-AM, jossa voit tehdä tämän joko kaksi työkalut: SQL Server Data Tools, SQL Server 2012 osa tai sen edeltäjä Business Intelligence (BI) Development Studiossa. Nämä raporttimääritykset ilmaistaan SSRS, jossa raportin Definition Language (RDL). Kun raportin RDL-tiedostot luonut ne ladataan AM pilvipalvelussa (vaihe 2). Raporttimääritys on nyt valmis käytettäväksi.

Seuraavaksi sovelluksen käyttäjä käyttää raportin (vaihe 3). Sovelluksen välittää pyynnön SSRS AM (vaihe 4), johon yhteyshenkilöt SQL-tietokanta tai muista lähteistä, saat tarvitsemansa tiedot (vaihe 5). SSRS käyttää näitä tietoja ja hahmontamiseen raportin (vaihe 6), valitse haluamasi RDL-tiedostojen palauttaa raportin sovellukseen (vaihe 7: ssä), jossa näkyy käyttäjän (vaihe 8).

Raportin upottaminen sovelluksen, kuvassa-skenaario ei ainoa vaihtoehto. On myös voinut tarkastella raportteja-Report Managerin AM AM SharePoint- tai muulla tavoin. Raporttien myös voidaan yhdistää, yksi raportin, joka sisältää linkin toiseen.

SSRS-Azure-AM tutustutaan täysi toiminnallisuus pilveen raportoinnin ratkaisuksi. Raporttien, voit käyttää mitä tahansa tietolähdettä SSRS tukemat. Sovelluksia ja raportteja voi olla upotettu koodi tai laitekokonaisuuksien tukemaan mukautetun toiminnan. Raportin suorittaminen ja värien ovat nopeasti, koska palvelimen sisällön raportoiminen ja ohjelma suorittaa yhdessä samalla virtuaalipalvelimen.



## <a name="tblstor"></a>Taulukkotallennus

Relaatiotietoja on hyötyä seuraavissa tilanteissa, mutta se ei ole aina sopiva vaihtoehto. Jos sovellus on erittäin paljon löyhästi jäsenneltyjen tietojen nopea ja yksinkertainen, esimerkiksi relaatiotietokannasta eivät ehkä toimi hyvin. NoSQL tekniikka todennäköisesti parempi vaihtoehto.

Azure-taulukkotallennus on esimerkki tällaisen NoSQL lähestymistapa. Sen nimen huolimatta taulukkotallennus ei tue vakio relaatiotietoja taulukot. Sen sijaan se sisältää kutsutaan *tallentaa avain/arvo*-tietojoukko liittäminen tiettyyn avaimeen ja auttaa muita sovellusten käyttö tietojen antamalla sitten-näppäintä. [Kuva 6](#Fig6) on kuvattu perusteet.

<a name="Fig6"></a>![Kaavio taulukkotallennus][SQL-tblstor]
 
**Kuva 6: Azure-taulukkotallennus on avain/arvo-säilössä, joka tarjoaa nopean, yksinkertainen pääsyn suuria tietomääriä.**

BLOB-objektit, kuten jokaisessa taulukossa on liitetty Azure-tallennustilan tilin. Taulukoiden nimetään myös paljon URL-osoite on lomakkeen BLOB-objektit, kuten

http://&lt;*StorageAccount*&gt;.table.core.windows.net/&lt;*taulukon nimi*&gt;

Kuten kuvassa näkyy, kullekin taulukolle on jaettu joidenkin määrä osioita, joihin voidaan tallentaa eri tietokoneeseen. (Tämä on lomakkeeseen sharding, SQL-Federation kanssa.) Azure sovellukset ja sovellusten muualla käyttää taulukon käyttämällä RESTful OData-protokolla tai Azure-tallennustilan asiakas-kirjasto.

Taulukon kunkin osion pitää joitakin *kohteita*sisältävän jopa 255 *Ominaisuudet*määrä. Jokaisella ominaisuudella on nimi ja tyyppi (esimerkiksi binaarinen, Bool, päivämäärä ja aika, kokonaisluku tai merkkijono) arvon. Toisin kuin relaatio tallennustilan seuraavassa taulukossa on kiinteä ei rakenne ja hieman eri kohteiden samassa taulukossa voi olla käyttämällä eri ominaisuuksia. Yhden kohteen on ehkä vain sisältävä nimi, esimerkiksi kun toisen yksikön samassa taulukossa on kaksi Int-ominaisuutta, joka sisältää asiakkaan tunnusnumero ja luottotietojen luokitus merkkijono-ominaisuus.

Tunnistavan tietyn kohteen taulukossa olevan sovelluksen on kyseisen yksikön avain. Avain on kaksi osaa: *osion avainta* , joka määrittää tiettyyn osioon ja *rivin avainta* , joka määrittää kyseisen osion kohteen. [Kuva 6](#Fig6)esimerkiksi asiakas pyytää osion A ja rivin avaimen 3 kohde ja taulukkotallennus palauttaa kyseisen kohteen, myös kaikki sen ominaisuuksien.

Tähän rakenteeseen avulla olla suuri taulukot - yhden taulukon voi olla enintään 100 teratavua tietojen - ja se sallii ne sisältävät tietojen nopea käytön. Se myös näyttää rajoituksia, mutta. Esimerkiksi ei tueta tapahtumien päivitykset, jotka ulottuvat taulukoita tai jopa osioiden yhdestä taulukosta. Joukon päivityksiä taulukkoon vain voidaan ryhmitellä atomisia tapahtumaksi, jos kaikki liittyvät kohteet ovat samassa osiossa. Käytettävissä on myös tapa taulukkoa koskeva kysely arvon perusteella sen ominaisuuksia, eikä onko liitokset tuki useiden taulukoiden välillä. Ja relaatiotietokannoista, toisin kuin taulukoissa on ei tue tallennettuja toimintosarjoja.

Azure-taulukkotallennus on hyvä sovelluksissa, jotka tarvitsevat paljon löyhästi jäsenneltyjen tietojen nopea ja halpaa. Internet-sovellus, joka sisältää useita käyttäjiä profiilitiedot voi käyttää esimerkiksi taulukot. Nopeasti on tärkeää tässä tilanteessa ja sovelluksen todennäköisesti ei tarvitse SQL koko potenssiin. Anna tämän toiminnon määrittäminen ja myönnä nopeuden ja koon voi joskus tulkita ja niin taulukkotallennus sopivuuden vain joidenkin ongelmien varalta.


## <a name="hadoop"></a>Hadoop

Organisaatioiden on on luominen tietojen varastoja vuosikymmeniä. Nämä relaatio taulukoihin useimmin tallennettuja tietoja hallintatyökalua salliminen käyttäminen ja opi tiedoista monella eri tavalla. SQL Server-esiintymän, on yleisiä työkaluilla, kuten SQL Server Analysis Services avulla voit tehdä tämän.

Mutta oletetaan, että haluat tehdä muita kuin relaatiotietoja analyysi. Tietoja voi kestää useita lomakkeita: tietoja anturit tai RFID tunnisteita, palvelimen klustereihin-web-sovellusten tuottamat käyttötiedot lokitiedostojen kuvat ja lääketieteellinen diagnostiikan laitteet. Nämä tiedot myös saattaa olla erittäin suuri, liian suuri voidaan käyttää tehokkaasti perinteinen tietovarasto. Big datasta ongelmista, kuten, harvinaisten vain muutama vuosi sitten, nyt on tullut aivan Yleiset.

Voit analysoida tällaisen big datasta meidän alan on suurelta päätynyt yksittäisen ratkaisua: Avaa lähde-tekniikkaa Hadoop. Hadoop suoritetaan klusterin fyysisiä tai näennäiskoneiden, ne toimivat näiden tietokoneissa tietojen levittäminen ja sitä käsitellään rinnakkain. Lisää koneet Hadoop on käytettävä, sitä nopeammin voit suorittaa riippumatta siitä, mitä se toimi jokaisen.

Tämän ongelman tyyppi on julkinen pilveen luonnollinen sopiva. Sen sijaan ylläpito army paikallisen palvelimissa, jotka saattavat istut käyttämättömänä paljon aikaa käynnissä Hadoop pilveen avulla voit luoda (ja maksu) VMs vain silloin, kun tarvitset niitä. Sitäkin parempi Lisää ja lisää big datasta, jonka haluat analysoida ja Hadoop luodaan pilvipalvelussa, tallentaminen, toiseen siirtyminen ongelmia. Microsoft toimittaa Hadoop-palvelun avulla voit hyödyntää seuraavia synergioita Azure. [Kuva 7](#Fig7) näyttää tämän palvelun tärkeimmät osat.

<a name="Fig7"></a>![Hadoop kaavio][hadoop]

**Kuva 7: Valitse Azure Hadoop suorittaa MapReduce työt, jotka käsittelevät tietoja käyttämällä useita näennäiskoneiden rinnakkain.**

Käyttämään Hadoop Azure ensin pyydät Hadoop-klusterin luominen tässä cloud-ympäristössä, joka määrittää VMs tarvitset määrän. Määrittäminen Hadoop-klusterin itse on-trivial tehtävään ja siten auttaa muita valita puolestasi Azure on järkevää. Kun olet valmis käyttämällä klusterin, voit sulkea sen. Ei tarvitse maksaa Laske resurssit, jotka eivät ole käytössä.

Hadoop-sovellus, tunnetaan yleisesti nimellä *työn*käyttää *MapReduce*nimeltään ohjelmoinnin malli. Kuten kuvassa näkyy, MapReduce työn logiikan suorittaa samanaikaisesti useita VMs yli. Käsittelemällä tietojen rinnakkain Hadoop voit analysoida tietoja paljon nopeammin kuin yksittäisen tietokoneen ratkaisuja.

Valitse Azure-tiedot MapReduce, työn työstää on yleensä käytettävissä Blob-objektien tallennustilaan. Hadoop-MapReduce työt odottavat, tiedot tallennetaan *Hadoop Distributed (HDFS)-järjestelmän*. HDFS on samankaltainen kuin joitakin tapoja, joilla;-Blob-objektien tallennustilaan se kopioi tiedot useita fyysinen palvelinten, kuten. Sijaan kopioida tätä toimintoa, Hadoop-Azure paljastaa sen sijaan kuvassa näkyy Blob-objektien tallennustilaan HDFS API-Liittymän kautta. Kun MapReduce työn logiikan saattavat mukaan se on tavallisen HDFS-tiedostojen avaaminen, tietojen virtauttaa BLOB-itse asiassa käsitteleminen työn. Ja tukemaan missä useita työt suoritetaan tietoja kirjainkoon Hadoop-Azure myös tiedot kopioidaan koko HDFS BLOB VMs käynnissä. 

MapReduce työt yleisesti kirjoitetaan Java nykyään toimintatavan, joka tukee Hadoop-Azure. Microsoft on lisännyt MapReduce töiden luominen muilla kielillä, kuten C#, F # ja JavaScript-tuki. Tee tämä big datasta tekniikka helpommin käytettävissä suuremman ryhmän kehittäjien on.

Sekä HDFS ja MapReduce Hadoop myös muita tekniikoita, analysoida tietoja kirjoittamatta MapReduce työn itse salliminen. Esimerkiksi Possu on suunniteltu analysoiminen big datasta, kun rakenne on nimeltään HiveQL kaltaisessa SQL-kielen korkean tason kieli. Possu ja rakenteen todella Luo MapReduce työt, jotka käsittelevät HDFS tietoja, mutta ne piilottaa tämän monimutkaisuus niiden käyttäjiltä. Molemmat ovat mukana Hadoop-Azure.

Microsoft toimittaa HiveQL ohjaimen myös Excel. Excel apuohjelman avulla, liiketoiminnan analyytikot voit luoda HiveQL kyselyt (ja täten MapReduce työt) suoraan Excelissä, ja valitse käsitellä ja esittää tulokset, käyttämällä PowerPivot- ja muilla Excelin työkaluilla. Hadoop-Azure sisältää myös muita tekniikoita konepohjaisten oppimistekniikoiden kirjastojen Mahout ja graph kaivos järjestelmän Pegasus kuten.

Suuri Tietoanalyysi on tärkeää ja siten Hadoop on myös tärkeää. Microsoft pyritään antamalla Hadoop Azure, valitse hallittu palveluna sekä linkkejä tuttuja työkaluja, kuten Excel, että tämä tekniikka helppokäyttöisen laajempi joukolle käyttäjiä.

Laajemmin kaikenlaisia tietoja on tärkeää. Tämän vuoksi Azure sisältää solualueen tietojen hallinta- ja business analytics-asetukset. Jostakin sovelluksen yrität luoda, on todennäköistä, että löydät jotain tässä cloud-ympäristössä, joka toimii puolestasi.

[blobs]: ./media/cloud-storage/Data_01_Blobs.png
[SQLSvr-vm]: ./media/cloud-storage/Data_02_SQLSvrVM.png
[SQL-db]: ./media/cloud-storage/Data_03_SQLdb.png
[SQL-datasync]: ./media/cloud-storage/Data_04_SQLDataSync.png
[SQL-report]: ./media/cloud-storage/Data_05_SQLReporting.png
[SQL-tblstor]: ./media/cloud-storage/Data_06_TblStorage.png
[hadoop]: ./media/cloud-storage/Data_07_Hadoop.png

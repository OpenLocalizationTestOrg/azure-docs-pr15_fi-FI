<properties 
    pageTitle="Tietojen siirtäminen skaalata ulos cloud tietokantojen välillä | Microsoft Azure" 
    description="Kerrotaan, miten voit käsitellä shards ja siirtää tiedot itse isännöityä palvelu joustavasti tietokannan ohjelmointirajapinnan kautta." 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="ddove"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="ddove" />

# <a name="moving-data-between-scaled-out-cloud-databases"></a>Tietojen siirtäminen skaalata ulos cloud tietokantojen välillä

Jos olet ohjelmiston palvelun kehittäjän ja yhtäkkiä sovelluksen elinkaarensa sisältää demand, tarvitset sopimaan kasvu. Näin voit lisätä muita tietokantoja (shards). Miten voit levittää uusien tietokantojen tiedot ilman toiminnan tietojen eheys? Tietojen siirtäminen rajoitettu tietokantojen uusien tietokantojen **Jaa ja yhdistäminen-työkalun** avulla.  

Jaa ja yhdistäminen-työkalu suoritetaan Azure web-palvelu. Järjestelmänvalvojan tai kehittäjän käyttää työkalua shardlets (tietojen shard) siirtäminen eri tietokantojen (shards) välillä. Työkalu käyttää shard kartan hallinta säilyttää palvelun metatietojen tietokanta ja varmista, että yhtenäinen yhdistämismääritykset.

![Yleiskatsaus][1]

## <a name="download"></a>Lataa
[Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge/)


## <a name="documentation"></a>Ohjeet
1. [Joustavasti tietokannan Jaa Yhdistä työkalun opetusohjelma](sql-database-elastic-scale-configure-deploy-split-and-merge.md)
* [Jaa ja yhdistäminen suojausasetukset](sql-database-elastic-scale-split-merge-security-configuration.md)
* [Jaa ja yhdistäminen suojausasiat](sql-database-elastic-scale-split-merge-security-configuration.md)
* [Shard kartan hallinta](sql-database-elastic-scale-shard-map-management.md)
* [Siirtää aiemmin luotujen tietokantojen asteikko itsestään](sql-database-elastic-convert-to-use-elastic-tools.md)
* [Joustavasti Tietokantatyökalut](sql-database-elastic-scale-introduction.md)
* [Joustavasti tietokannan Työkalut-sanasto](sql-database-elastic-scale-glossary.md)

## <a name="why-use-the-split-merge-tool"></a>Jaa Yhdistä työkalun käyttötarkoitus

**Joustavuus**

Sovellukset on venyttää joustavasti yhteen Azure SQL-DB-tietokantaan rajoissa. Tietojen siirtäminen tarvittaessa uusien tietokantojen säilyttäen eheys-työkalun avulla.

**Voit kasvattaa jakaminen** 

Sinun täytyy lisätä Yleinen räjähtäviä määrän kasvun mukaan. Voit tehdä Luo muita kapasiteetin sharding tiedot ja jakamalla sen kaikissa soluissa lisää tietokantoja, kunnes kapasiteetti tarpeet täyttyvät. Tämä on ensisijainen Esimerkki Jaa"-ominaisuutta. 

**Voit pienentää yhdistäminen**

Kapasiteetti on Pienennä liiketoiminnan kausiluonteisista laatu vuoksi. Työkalun voit skaalata vähemmän asteikon yksiköitä, kun business hidastaa. '' Yhdistämistoiminto joustavasti asteikko Jaa yhdistämisen palvelussa kattaa tämä vaatimus. 

**Hallitse kohdepisteet siirtämällä shardlets**

Useita alihallinnoista tietokantaa kohden, jossa shardlets shards kohdistus voi aiheuttaa kapasiteetin pullonkaulojen joitakin shards käyttöön. Tämä edellyttää Varaa uudelleen shardlets tai varattu shardlets siirtäminen uuteen tai vähemmän käytetään shards. 

## <a name="concepts--key-features"></a>Käsitteitä ja ominaisuuksia

**Asiakkaan isännöidä palveluja**

Jaa ja yhdistäminen toimitetaan asiakkaan isännöimä palveluna. Täytyy ottaa käyttöön ja isännöidä palvelun Microsoft Azure-tilaukseesi. Voit ladata NuGet paketti sisältää määritys mallin suorittamiseen tiedot tietyn käyttöönottoa varten. Katso lisätietoja [Jaa yhdistämisen opetusohjelma](sql-database-elastic-scale-configure-deploy-split-and-merge.md) . Koska palvelu suoritetaan Azure tilauksen, voit määrittää sekä määrittää useimmat suojaus-palvelun. Oletusmallin on asetuksia voit määrittää SSL, varmenne-pohjaiseen todennuksen ja salauksen tallennettuja tunnistetietoja, tee näin suojaamalla ja IP-rajoituksia. Löydät lisätietoja suojaus-ominaisuuksia seuraavat tiedoston [Jaa yhdistämisen suojausasetukset](sql-database-elastic-scale-split-merge-security-configuration.md).

Oletusarvo on otettu käyttöön palvelun suoritetaan yksi työntekijä ja yksi web rooli. Jokainen käyttää A1 AM koon Azure pilvipalveluihin. Vaikka et voi muokata näitä asetuksia, kun käyttöönotto paketti, voit muuttaa niitä käynnissä pilvipalvelussa (palvelun Azure-portaalissa) onnistuneen käyttöönoton jälkeen. Huomaa, että työntekijän rooli on ei ole määritetty useamman kuin yhden esiintymän teknisistä syistä. 

**Shard kartta-integrointi**

Jaa ja yhdistäminen-palvelun toimii shard kartta-sovelluksen kanssa. Kun käytät Jaa ja yhdistäminen-palvelun Jaa tai Yhdistä alueiden tai shards shardlets, palvelu säilyttää shard kartan ajan tasalla. Voit tehdä tämän palvelun muodostaa yhteyden sovelluksen shard kartan manager-tietokannan ja ylläpitää alueiden ja yhdistämismääritykset Yhdistä/Jaa/move-pyyntöjen edistymisestä. Näin varmistat, että shard kartan esittää ajan tasalla Näytä aina kun Jaa yhdistämistä ovat ajan tasalla. Jaa, Yhdistä ja shardlet siirto-toimintojen otetaan siirtämällä shardlets erän lähde-shard kohde shard. Aikana shardlet siirrossa veloittaa nykyisen erän shardlets on merkitty shard kartan offline-tilassa ja eivät ole käytettävissä tietojen riippuva reititys yhteyksien **OpenConnectionForKey** Ohjelmointirajapinnan käyttäminen. 

**Yhtenäinen shardlet yhteydet**

Tietojen siirto käynnistyessä shardlets uuden erän mahdolliset shard kartan annettujen tietojen riippuva reititys yhteydet, tallentamiseen shardlet shard ovat lopetettujen ja myöhempiä yhteyksiä shard muuntomalli API näihin shardlets on estetty, kun tietojen siirto on käynnissä, jotta vältät ristiriidat. Muut shardlets saman shard-yhteydet myös Hae lopetettu, mutta se onnistuu uudelleen heti, yritä uudelleen. Kun erä on siirretty, shardlets merkitään online uudelleen, kohde shard ja lähdetietojen poistetaan lähde-shard. Palvelun käy läpi nämä vaiheet jokaisen erän, kunnes kaikki shardlets on siirretty. Johtaa yhteyden kill varallisuuden valmis Jaa/yhdistämisen ja Siirrä-toiminnon aikana.  

**Shardlet käytettävyyden hallinta**

Rajoittaminen yhteyden lopettamisen shardlets, kuten edellä yläpuolella nykyisen erän rajoittaa siitä laajuus shardlets yhtä erää kerrallaan. Tämä on ensisijainen toimintatavan missä valmis shard pysyy offline-tilassa sen shardlets aikana, jakaminen ja yhdistäminen-toiminto. Siirtoerän, eri shardlets kerrallaan, siirrä määrä lasketaan koko on configuration-parametria. Se voidaan määrittää sovelluksen käytettävyys ja suorituskyvyn tarpeiden mukaan kutakin Jaa ja yhdistä työkirjat-toimintoa. Huomaa, että alue, joka sisältää lukittu shard määrityksen voi olla suurempi kuin määritetty erän koko. Tämä johtuu siitä palvelun valitsee alueen kokoa siten, että tosiasiallinen määrä on sharding avainarvot tietojen vastaa noin erän koko. Tämä on tärkeää muistaa erityisesti asutussa sharding näppäimet varten. 

**Metatiedon**

Jaa ja yhdistäminen-palvelun käyttää tietokantaa, jos haluat säilyttää sen tilan ja säilyttää lokit pyynnön käsiteltäessä. Käyttäjän Luo tietokanta-tilauksen ja antaa yhteysmerkkijonon sen määritystiedostossa palvelun käyttöönottoa varten. Käyttäjän organisaatiostasi järjestelmänvalvojat voivat myös yhteydet tähän tietokantaan pyynnön käynnissä ja tutki mahdolliset virheet koskevat yksityiskohtaiset tiedot.

**Sharding tilatieto**

Jaa ja yhdistäminen-palvelun erottaa (1) sharded taulukot, (2) viittaus taulukoita ja (3) Normaali taulukot. Jaa ja Yhdistä ja Siirrä-toiminnon semantiikkaan liittyvien riippuvat käyttää taulukon ja määritellään seuraavasti: 

* **Sharded taulukoiden**: Jaa, yhdistäminen ja Siirrä toiminnot siirretään shardlets lähde kohde shard. Onnistunut käyttöönotto yleinen pyynnön, kun kyseiset shardlets eivät ole enää olemassa lähteen. Huomaa, että kohde taulukot on olemassa kohde shard ja ei saa olla ennen toiminnon käsittely kohde-alueen tietojen. 

* **Viittaus taulukot**: viittaus taulukoita, Jaa, yhdistää ja Siirrä toimintojen kopioida tiedot kohteen shard lähteestä. Huomaa kuitenkin, että muutoksia ilmetä tietyn taulukon kohde-shard, jos minkä tahansa rivin on jo olemassa olevat samannimiset tässä taulukossa. Taulukossa on tyhjä minkä tahansa viittaus taulukon kopiointi Hae käsitteleminen.

* **Muut taulukot**: muiden taulukoiden välille voi olla esitä lähde-tai Jaetut ja Yhdistä-toiminnon kohteen. Jaa ja yhdistäminen-palvelun hylkää näiden taulukoiden tietojen siirto-ja kopio-toimintoja. Huomaa kuitenkin, että ne voi häiritä näitä toimintoja, jos rajoitukset.

Viittaus sharded taulukot ja tiedot tarjoaa **SchemaInfo** API shard kartassa. Seuraavassa esimerkissä on kuvattu nämä annetun shard kartan hallinta objektin smm-ohjelmointirajapinnan käyttäminen: 

    // Create the schema annotations 
    SchemaInfo schemaInfo = new SchemaInfo(); 

    // Reference tables 
    schemaInfo.Add(new ReferenceTableInfo("dbo", "region")); 
    schemaInfo.Add(new ReferenceTableInfo("dbo", "nation")); 

    // Sharded tables 
    schemaInfo.Add(new ShardedTableInfo("dbo", "customer", "C_CUSTKEY")); 
    schemaInfo.Add(new ShardedTableInfo("dbo", "orders", "O_CUSTKEY")); 

    // Publish 
    smm.GetSchemaInfoCollection().Add(Configuration.ShardMapName, schemaInfo); 

Taulukoiden "alue" ja 'nation' on määritetty viittaus taulukoina ja kopioidaan Jaa/yhdistämisen ja Siirrä toimintojen kanssa. "asiakas" ja "Tilaukset-puolestaan on määritetty sharded taulukoina. C_CUSTKEY ja O_CUSTKEY yhteyshenkilönä sharding-näppäintä. 

**Viite-eheyden säilyttäminen**

Jaa ja yhdistäminen-palvelun analysoi taulukoiden väliset riippuvuudet ja viiteavaimen ja perusavaimen yhteyksiä avulla vaiheen siirtymisen viittaus taulukot ja shardlets toimintoja. Yleensä viittaus taulukot kopioidaan ensin riippuvuuden järjestyksessä sitten shardlets kopioidaan niiden riippuvuuksia kuluessa kunkin erän järjestyksessä. Tämä on tarpeen, niin, että kohde shard Viiteavain Perusavain rajoitukset noudatetaan, kun saapuu uusia tietoja. 

**Shard kartan yhdenmukaisuutta ja potentiaalisen täyttäminen**

Virheet, kun Jaa ja yhdistäminen-palvelu jatkaa toimintojen jälkeen kaikki käyttökatkosta ja tarkoituksena on suoritettava jokin käynnissä pyyntöjä. Kuitenkin ehkä palauttaa tilanteissa, esimerkiksi kun kohde shard kadonneita tai korjata käsiin. Näiden tilanteissa, jotka on määritetty siirrettävä joitakin shardlets jatkaa lähde-shard sijaitsevat. Palvelun varmistaa shardlet yhdistämismääritysten päivitetään vain sen jälkeen, kun tarvittavat tiedot on kopioitu onnistuneesti kohteeseen. Shardlets vain poistettu lähde, kun niiden tiedot on kopioitu kohde ja vastaavan yhdistämismääritykset on päivitetty. Poistotoiminto tapahtuu taustalla, kun alue on jo online kohde shard. Jaa ja yhdistäminen-palvelun varmistaa aina tallennettu shard määrityksen yhdistämismääritykset oikeellisuuden.


## <a name="the-split-merge-user-interface"></a>Jaa ja yhdistäminen-käyttöliittymä

Jaa ja yhdistäminen palvelupakettiin sisältää työntekijän rooli ja web-rooli. Web-roolin käytetään lähettää jaetun yhdistämisen pyyntöjä vuorovaikutteinen tavalla. Käyttöliittymän pääkomponentit ovat seuraavat:

-    Toiminnon tyyppi: Toiminnon tyyppi on valintanappi, joka määrittää, millaisia pyyntö palvelu suorittama toiminto. Voit valita Jaa, yhdistää ja siirtää skenaarioita. Voit myös peruuttaa aiemmin lähetetty toiminto. Voit käyttää Jaa, yhdistäminen ja siirtää alueen shard kartat pyynnöt. Luettelon shard yhdistää vain tuki Siirrä-toimintoja.

-    Shard kartan: Pyynnön parametrit seuraavan osion kansilehden shard kartta- ja isännöinnin shard kartta tietokannan tietoja. Haluat erityisesti nimetä Azure SQL-tietokanta-palvelin ja tietokanta isännöinnin shardmap, shard kartta-tietokanta ja lopuksi shard kartan nimi ‑yhteyden. Tällä hetkellä toiminto hyväksyy vain yhden joukko tunnistetietoja. Nämä tunnistetiedot on on oikeus tehdä muutoksia shard karttaan sekä siten, että käyttäjätiedot shards.

-    Lähdealueella (Jaa ja yhdistää): Jaa ja Yhdistä-toiminto käsittelee alueen alin ja ylin-näppäintä. Voit määrittää toiminnon, jolla rajoittamaton hyvin avainarvon, "suuri avain on enintään"-valintaruutu ja jätä hyvin avaimen kentän tyhjäksi. Vastaavat täsmälleen yhdistämismäärityksen ja sen shard karttaasi rajoja ei tarvitse alueen avaimen arvoja, jotka määrität. Jos et määritä kaikki alueen rajat lainkaan palvelu johtaa lähinnä alueen puolestasi automaattisesti. Voit hakea tietyn shard kartan nykyisen yhdistämismääritykset GetMappings.ps1 PowerShell-komentosarjaa.

-    Jaetun tietolähteen toiminta (jaettu): määrittää Jaa toimintoja, osoita Jaa lähdealueella. Voit tehdä tämän antamalla kohtaa, johon haluat lisätä jaon suoritetaan sharding-näppäintä. Käytä valintanappi määrittää, haluatko (lukuun ottamatta Jaa-näppäintä) alueen alaosassa siirtää vai haluatko yläosassa Siirrä (mukaan lukien Jaa-näppäintä).

-    Tietolähteen Shardlet (Siirry): Siirrä toiminnot ovat eri jakaminen ja yhdistäminen toimintojen ne eivät edellytä alueen kuvaamaan lähde. Siirrä lähde tunnistetaan yksinkertaisesti sharding avainarvon, jota aiot siirtää.

-    Kohteen Shard (jaettu): kun tiedot on annettu Jaa-Toiminto lähdettä, sinun on määritettävä haluamaasi tiedot kopioidaan antamalla Azure SQL-Db-palvelin ja tietokanta nimen kohteen.

-    Kohdealue (Yhdistä): yhdistämistä siirtää aiemmin shard shardlets. Voit tunnistaa aiemmin shard antamalla alueen rajat olemassa olevan alueen, jonka haluat yhdistää.

-    Erän koko: Erän koko ohjausobjektit, jotka siirtyvät offline-tilassa kerrallaan aikana tietojen siirto shardlets määrä. Tämä on kokonaisluku, missä voit käyttää pienemmät arvot, kun olet luottamuksellisia pitkä shardlets käyttökatkot kausien. Suuremmat arvot kasvattaa aika, jonka annetun shardlet offline-tilassa, mutta voi parantaa suorituskykyä.

-    Toiminnon tunnus (Peruuta): Jos sinulla on meneillään oleviin toimintoa, joka ei enää tarvita, voit peruuttaa toiminnon antamalla sen toiminnon tunnus tähän kenttään. Voit noutaa toiminnon tunnus pyynnön tila-taulukossa (Katso osan 8.1) tai web-selaimessa, jossa lähettämäsi pyynnön tulosteesta.


## <a name="requirements-and-limitations"></a>Vaatimukset ja rajoitukset 

Jaa ja yhdistäminen-palvelun käyttöönottoon peritään seuraavat vaatimukset ja rajoitukset: 

* Shards on olemassa ja rekisteröitävä shard määrityksen, ennen kuin Jaa-yhdistämistoiminnon nämä shards voidaan suorittaa. 

* Palvelu ei luoda, taulukoiden ja muiden tietokantaobjektien automaattisesti toimintansa osana. Tämä tarkoittaa, että kaikki sharded ja viittaus taulukoiden rakenne on olemassa ennen minkään Jaa/yhdistämisen ja Siirrä-toiminnon kohde-shard. Sharded taulukoiden tarvitaan erityisesti on tyhjä alue, missä uusi shardlets ovat lisätään Jaa/yhdistämisen ja Siirrä-toiminnolla. Muussa tapauksessa toiminto epäonnistuu kohde shard alkuperäisen yhdenmukaisuuden-valintaruutu. Huomaa myös, jotka viittaavat tiedot kopioidaan vain, jos viittauksen taulukko on tyhjä ja että mikään ei ole yhdenmukaisuuden suhteessa muihin samanaikaisen kirjoittaa viittaus taulukot-toimintoja. Suosittelemme tämän: suoritettaessa Jaa/yhdistämistä ei ole kirjoitustoimintojen tehdä muutoksia viittaus-taulukoihin.

* Palvelu on riippuvainen rivin tunnistetietojen yksilöllinen indeksi tai avainta, joka sisältää parantaa suorituskykyä ja suuri shardlets luotettavuutta sharding-näppäintä. Näin tietojen siirtämiseen jopa tarkempaan rakeisuuden, kuin vain sharding avainarvon palvelu. Näin voit pienentää enimmäismäärän vapaata ja lukitusten, joita tarvitaan toiminnon aikana. Harkitse yksilöllinen indeksi tai perusavain, mukaan lukien tietyn taulukon sharding-näppäintä, jos haluat käyttää taulukon Yhdistä/Jaa/move-pyyntöjen luomista. Nopeuttamiseksi sharding avain on oltava avain tai hakemiston alussa sarakkeen.

* Kokouspyynnön käsittely aikana shardlet tietoja voi olla käytössä sekä lähde kohde shard. Tämä on tarpeen suojautua virheet shardlet siirron aikana. Jaa ja yhdistäminen shard rakenneruudun integrointi varmistaa, että yhteydet riippuvaiset reititys API shard kartassa **OpenConnectionForKey** -menetelmällä tiedot – ei ole näkyvissä jokin epäyhtenäiset keskitason hyötyä. Kun muodostat lähde- tai kohdeluettelon shards ilman **OpenConnectionForKey** menetelmällä, epäyhtenäinen keskitason hyötyä voi kuitenkin näkyvissä kun Yhdistä/Jaa/move-pyyntöjen ovat ajan tasalla. Asiakasyhteydet saattaa näkyä osittaisen tai samat tulokset ajoituksen tai pohjana yhteyden shard mukaan. Tämä rajoitus on tällä hetkellä yhteyksiä Avainhankkeiden monivaiheisen Shard joustavasti asteikko-kyselyjen.

* Jaa ja yhdistäminen-palvelun metatietojen-tietokantaa ei voi jakaa eri rooleja välillä. Jaa ja yhdistäminen-palvelun käytössä väliaikaisen rooli on esimerkiksi eri metatietojen tietokannasta kuin tuotannon rooli.
 

## <a name="billing"></a>Laskutus 

Jaa ja yhdistäminen-palvelu toimii pilvipalvelussa Microsoft Azure-tilaukseesi. Tämän vuoksi cloud Services ovat voimassa palvelu-esiintymään. Paitsi, jos teet usein Jaa/yhdistämisen ja Siirrä toimintoja, on suositeltavaa poistaa jaetun yhdistämisen pilvipalvelussa. Tallentaa suorittaminen kustannuksiin tai käyttöön cloud palvelun esiintymät. Voit uudelleen käyttöön ja aloittaa helposti suorituskelpoiset kokoonpanosi aina, kun haluat suorittaa jakaminen ja yhdistäminen. 
 
## <a name="monitoring"></a>Seuranta 
### <a name="status-tables"></a>Tila-taulukot 

Jaa ja yhdistäminen-palvelu sisältää metatietojen **RequestStatus** juuri tallentaa tietokannan valmiit ja jatkuvaa pyyntöjen seurantaa varten. Taulukossa on lueteltu rivin Jaa yhdistämisen sivupyynnön, jotka on lähetetty tässä esiintymässä Jaa ja yhdistäminen-palvelun. Antaa sivupyynnön seuraavat tiedot:

* **Aikaleima**: kellonajan ja päivämäärän, jolloin pyynnön aloitettiin.

* **Toimintotunnukseksi**: GUID-tunnus, joka yksilöi pyynnön. Pyyntö avulla voidaan myös peruuttaa toiminnon ollessa vielä kesken.

* **Tila**: nykyisen tilan pyynnön. Jatkuva pyyntöjen se näyttää myös nykyinen vaihe, jossa pyydetään.

* **CancelRequest**: lippu, joka ilmaisee, onko pyyntö on peruutettu.

* **Edistymisen**: prosentti-arvion suorituksesta toiminnon. 50 arvo tarkoittaa, että toiminto on noin 50 % valmiina.

* **Tiedot**: XML-arvo, joka sisältää tarkempia edistymisestä. Edistymisen raportille päivitetään säännöllisesti rivien määrä kopioidaan lähteestä kohteeseen. Virheet tai poikkeukset-sarake sisältää lisätietoja virheestä.


### <a name="azure-diagnostics"></a>Azure diagnostiikka

Jaa ja yhdistäminen-palvelu käyttää Azure diagnostiikka Azure SDK 2,5 seuranta-ja diagnostiikka perusteella. Diagnostiikan asetusten hallintaan seuraavassa kuvatulla: [Ottaminen käyttöön diagnostiikka Azure Cloud Services-palveluissa ja näennäiskoneiden](../cloud-services/cloud-services-dotnet-diagnostics.md). Lataa paketti sisältää kaksi diagnostiikka käyttömahdollisuudet – yhden web rooli ja toisen työntekijän oikeuksia. Määritysten diagnostiikka-palvelun noudattamalla ohjeita [Cloud palvelun Fundamentals Microsoft Azure-tietokannassa](https://code.msdn.microsoft.com/windowsazure/Cloud-Service-Fundamentals-4ca72649). Se sisältää kirjautua suorituskyvyn laskureita, IIS-lokit, Windowsin tapahtumalokien ja jaa yhdistämisen sovelluksen tapahtumalokien määrityksiä. 

## <a name="deploy-diagnostics"></a>Ota käyttöön vianmäärityksen 

Jotta seuranta- ja diagnostiikka diagnostiikan määritysten käyttäminen Internetin kautta tai työntekijä roolit myöntämä NuGet-paketti, suorita seuraavat komennot Azure PowerShellin avulla: 

    $storage_name = "<YourAzureStorageAccount>" 
    
    $key = "<YourAzureStorageAccountKey" 
    
    $storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key  
    
    
    $config_path = "<YourFilePath>\SplitMergeWebContent.diagnostics.xml" 
    
    $service_name = "<YourCloudServiceName>" 
    
    Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Production -Role "SplitMergeWeb" 
    
    
    $config_path = "<YourFilePath>\SplitMergeWorkerContent.diagnostics.xml" 
    
    $service_name = "<YourCloudServiceName>" 
    
    Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Production -Role "SplitMergeWorker" 

Voit etsiä lisätietoja siitä, miten voit määrittää ja ottaa käyttöön tässä diagnostiikan asetusten: [Ottaminen käyttöön diagnostiikka Azure Cloud Services-palveluissa ja näennäiskoneiden](../cloud-services/cloud-services-dotnet-diagnostics.md). 

## <a name="retrieve-diagnostics"></a>Noutaa diagnostiikka 

Voit käyttää omaa diagnostiikka helposti Visual Studio palvelimen Resurssienhallinnasta palvelimen Explorer puun Azure-osassa. Avaa Visual Studio esiintymän ja valikkorivillä valitsemalla Näytä ja palvelimen Explorer. Napsauta muodostaa Azure tilauksen Azure-kuvaketta. Siirry Azure -> tallennustilan -> <your storage account> -> taulukot -> WADLogsTable. Lisätietoja on artikkelissa [Selaaminen tallennustilan resurssien palvelimen Resurssienhallinnassa](http://msdn.microsoft.com/library/azure/ff683677.aspx). 

![WADLogsTable][2]

Edellä olevassa kuvassa näkyy korostettuna WADLogsTable sisältää yksityiskohtaiset tapahtumat Jaa ja yhdistäminen-palvelun sovelluksen lokista. Huomautus ladatun paketin oletusarvo-määritys on suunnattu tuotannon käyttöönotto. Tämän vuoksi, jolla lokit ja laskureita vedetään palveluesiintymiä väli on suuri (5 minuuttia). Testaa ja kehittämisen pienentämällä välin säätämällä diagnostiikan asetusten verkossa tai Työntekijä-roolin tarpeitasi. Napsauta hiiren kakkospainikkeella rooli Visual Studio palvelimen Explorer (katso yllä) ja sitten säätää siirtää diagnostiikka asetukset-valintaikkunassa: 

![Määritys][3]


## <a name="performance"></a>Suorituskyky

Yleensä parempi suorituskyky on muilta mitä suurempi, Lisää performant palvelutasot Azure SQL-tietokantaan. Korkeampi suurempi palvelutasot IO, suorittimen ja muistin kohdistukset hyötyä joukkona kopio ja poista toiminnoista, joita jaettu ja yhdistäminen-palvelu. Tästä syystä suurentaa palvelun tason vain ne tietokantoja varten määritetyn, rajoitetun ajanjakson ajan.

Palvelun suorittaa myös vahvistus kyselyjen Normaali toimintansa osana. Nämä vahvistus-kyselyt odottamattomia esiintyminen kohde-alueen tietojen etsiminen ja varmistaa, mikä tahansa Jaa/yhdistämisen ja Siirrä-toiminto käynnistyy yhdenmukaisia tilasta. Kaikki nämä kyselyt Työskentele sharding avaimen alueiden määritetty toiminto laajuuden ja erän koko pyyntö määritelmän osana. Nämä kyselyt suorittaa parhaiten, kun indeksi on esitä, joka on alussa sarakkeen sharding-avain. 

Lisäksi yksilöllisyyden ominaisuus alussa sarakkeen sharding-näppäimen kanssa sallii optimoitu toimintatavan, joka rajoittaa log tilaa ja muistin kulutus resurssin palvelu. Yksilöllisyyden tämä ominaisuus edellyttää siirtää suuria tietomääriä (yleensä yläpuolella 1 gt). 

## <a name="how-to-upgrade"></a>Päivittäminen

1. [Ota käyttöön Jaa ja yhdistäminen-palvelun](sql-database-elastic-scale-configure-deploy-split-and-merge.md)noudattamalla.
2. Voit muuttaa cloud palvelun määritysten tiedoston Jaa ja yhdistäminen-käyttöönoton vastaamaan uudet määritykset parametrit. Uusi Vaadittu parametri on käytetty salauksen sertifikaatin tietojen. Voit tehdä tämän helposti on verrata uuden mallin kokoonpanotiedosto Lataa aiemmin luotu kokoonpanosi vastaan. Varmista, että lisäät verkossa ja Työntekijä-roolin "DataEncryptionPrimaryCertificateThumbprint" ja "DataEncryptionPrimary" asetukset.
3. Ennen kuin otat päivityksen Azure, varmista, että kaikki käynnissä Jaa ja yhdistäminen toiminnot on valmis. Voit helposti tehdä tämän tekemällä kyselyn jatkuvaa pyynnöt Jaa yhdistämisen metatietojen tietokannan RequestStatus ja PendingWorkflows taulukot.
4. Päivitä aiemmin cloud palvelun käyttöönoton Jaa-yhdistämisen Azure-tilaukseesi uusi paketti ja päivitetyt palvelun kokoonpanotiedosto.

Sinun ei tarvitse valmistella metatietojen uuden tietokannan päivittäminen Jaa-yhdistämistä varten. Uuden version päivittää automaattisesti metatietojen aiemmin luotu tietokanta uuteen versioon. 

## <a name="best-practices--troubleshooting"></a>Parhaat käytännöt ja vianmääritys
 
-    Määritä testin palvelutili ja että tärkeimmät Jaa/yhdistämisen ja Siirrä-toimintoja, joiden testi-vuokraajan on syytä olla useita shards yli. Varmista, että kaikki metatiedot on määritetty oikein shard kartta ja että toimintoja ei riko rajoitukset tai viiteavaimien.
-    Säilytä testi-vuokraajan arvopisteiden koko edellä suurimman alihallintaan varmistamiseksi kohtaat ei arvopisteiden koko suurin tiedot koon liittyvät ongelmat. Näin voit arvioida ylärajan-siirtyä yhtä vuokraajan kuluvaa aikaa. 
-    Varmista, että rakenteen sallii poistot. Jaa ja yhdistäminen-palvelu edellyttää mahdollisuus tietojen poistaminen lähde-shard, kun tiedot on kopioitu onnistuneesti kohteeseen. Esimerkiksi **käynnistimien** voit estää palvelun poistaminen lähteen tiedot ja saattaa epäonnistua toimintoja.
-    Sharding-näppäintä on oltava perusavain tai yksilöllinen indeksi määritelmä ensimmäistä saraketta. Joka varmistaa parhaan suorituskyvyn, jakaminen ja yhdistäminen vahvistus-kyselyiden ja todellisten tietojen siirtämistä ja poistaminen toimintoja, jotka toimivat aina sharding tärkeimmät alueet.
-    Collocate alue ja tietojen keskelle, joissa tietokantoja sijaitsevat Jaa ja yhdistäminen-palvelussa. 

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]



<!--Anchors-->
<!--Image references-->
[1]:./media/sql-database-elastic-scale-overview-split-and-merge/split-merge-overview.png
[2]:./media/sql-database-elastic-scale-overview-split-and-merge/diagnostics.png
[3]:./media/sql-database-elastic-scale-overview-split-and-merge/diagnostics-config.png
 

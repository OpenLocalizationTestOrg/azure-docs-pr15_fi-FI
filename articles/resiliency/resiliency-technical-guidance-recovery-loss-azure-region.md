<properties
   pageTitle="Tekniset Azure alue-ohjeet menetyksiä palauttamisesta vikasietoisuudelle | Microsoft Azure"
   description="Artikkeli tietoja ja joustavat, erittäin käytettävissä vika varmatoimisempia sovellusten suunnitteleminen sekä palauttaminen suunnitteleminen"
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

#<a name="azure-resiliency-technical-guidance-recovery-from-a-region-wide-service-disruption"></a>Azure vikasietoisuudesta tekniset ohjeet: alueen laajuisen keskeytetty palauttaminen

Azure on jaettu fyysisesti ja loogisesti yksiköt kutsutaan alueet. Alueen sisältää vähintään yhden lähellä toisiaan palvelinkeskusten. Milloin tämä kirjoittaminen Azure on 24 alueiden eri puolilla maailmaa.

Valitse harvinaisissa on mahdollista, että koko alueen tilat muuttuvat ei voi käyttää esimerkiksi verkon virheiden vuoksi. Tai tilat katoavat kokonaan, esimerkiksi luonnollinen huono vuoksi. Tässä osassa kerrotaan Azure ominaisuuksia sovellukset, jotka on hajautettu alueiden luomiseen. Näiden jakauman avulla voit pienentää yhden alueen virhe voi vaikuttaa muiden alueiden mahdollisuus.

##<a name="cloud-services"></a>Pilvipalveluihin

###<a name="resource-management"></a>Resurssienhallinta

Voit jakaa Laske esiintymiä eri alueilla luomalla erillisen pilvipalvelussa kohde kunkin alueen ja julkaisemalla asennuspaketin kunkin pilvipalvelussa. Huomaa kuitenkin, että jakaminen liikenne pilvipalveluihin eri alueiden välillä on pantava täytäntöön sovelluksen kehittäjä tai liikenteen hallinta-palvelun kanssa.

Vara roolin esiintymät käyttöönottaminen etukäteen palauttaminen lukumäärän on tärkeää näkökohdat kapasiteetin suunnittelua. Ottaa täysikokoista toissijainen käyttöönoton varmistaa, että kapasiteetti on jo käytettävissä tarvittaessa; Tämä kuitenkin tehokkaasti kaksinkertaistuu kustannukset. Tavallisen kuvion on pieni, toissijainen käyttöönoton, riittävän suureksi kriittinen palvelujen suorittamista. Pieni toissijainen käyttöönottoon on hyvä, sekä varata kapasiteettia, ja testikäyttöön toissijainen ympäristön määritys.

>[AZURE.NOTE]Tilauksen kiintiön ei kapasiteetin takaa. Kiintiö on yksinkertaisesti luottotietojen rajoitukset. Kapasiteetin takaamiseksi roolit tarvittava määrä on määritettävä palvelun mallin ja roolien on otettava käyttöön.

###<a name="load-balancing"></a>Kuormituksen

Lataa saldo liikenne eri alueilla edellyttää liikenne management-ratkaisun. Azure on [Azure liikenteen hallinta](https://azure.microsoft.com/services/traffic-manager/). Voit myös hyödyntää kolmannen osapuolen palvelujen, joissa on samanlainen liikenne hallintatoiminnot.

###<a name="strategies"></a>Strategioita

Useita vaihtoehtoinen strategioita ovat käytettävissä eri aikajaksoille Laske käyttöönoton eri alueilla. Nämä on mukautettava tietyn liiketoiminnan vaatimukset ja sovelluksen tilanteissa. Korkean tason tavoista voidaan jakaa seuraavat luokat:

  * __Ota käyttöön tietojen uudelleen__: Tämän menetelmän-sovellus on myymälät alusta alkaen tietojen aikaan. Tämä on vastaava vähäinen sovelluksia, jotka eivät tarvitse taattua Palautumisaika.

  * __Lämpimän vara (aktiivinen/passiivinen)__: toissijainen isännöityä palvelu luodaan vaihtoehtoinen alueen ja roolien on otettu käyttöön taata mahdollisimman vähän kapasiteetin; kuitenkin roolit et saa tuotannon liikenne. Tämä vaihtoehto on hyötyä sovelluksissa, jotka eivät on suunniteltu liikenne jakamiseen eri alueilla.

  * __Kuuma vara (aktiivinen/aktiivinen)__: sovelluksen tarkoituksena on saada tuotannon Lataa useita alueilla. Kunkin alueen pilvipalveluihin on ehkä määritetty suurempi kuin tarvittavat kapasiteetin tietojen palauttaminen tarkoituksiin. Voit myös pilvipalveluun voi skaalata milloin tietojen ja automaattisesti tarvittaessa ulos. Tämän menetelmän edellyttää sovelluksen rakenteessa merkittäviin sijoitus, mutta se on merkittäviä etuja. Näitä ovat pieniä ja taattua palautus ajan, jatkuva testaaminen kaikkien palautus sijainnit ja kapasiteettia tehokas käyttö.

Hajautettu rakenne on valmis ulkopuolelle jääviä tämän asiakirjan. Lisätietoja on artikkeleissa [palauttaminen ja suuren käytettävyyden Azure-sovellusten](https://aka.ms/drtechguide).

##<a name="virtual-machines"></a>Näennäiskoneiden

Infrastruktuurin kuin service (IaaS) näennäiskoneiden (VMs) palauttaminen muistuttaa ympäristö kuin palvelu (PaaS) Laske monin palauttaminen. On tärkeää eroja, kuitenkin siitä, että IaaS AM koostuu AM ja AM levy.

  * __Voit luoda cross alueen varmuuskopiot, jotka ovat yhdenmukaisia sovelluksen käyttöä Azure varmuuskopion__.
  [Azure varmuuskopion](https://azure.microsoft.com/services/backup/) avulla voit luoda sovelluksen yhdenmukaisia varmuuskopioiden useille AM levyille ja varmuuskopioiden replikoinnin tuki eri alueilla. Voit tehdä tämän valitsemalla geo toisinto varmuuskopion säilö luomisen yhteydessä. Huomaa, että replikointi varmuuskopion säilö on määritettävä luomisen yhteydessä. Se ei voi määrittää myöhemmin. Alueen menetetään, jos Microsoft tekee varmuuskopioista käytettävissä asiakkaille. Asiakkaat voivat palauttaa mihin tahansa niiden määritetyn palauttaminen asioista.

  * __Erota tietojen levyn käyttöjärjestelmän levyltä__. Tärkeää tarkasteltaviksi IaaS VMs on, että et voi vaihtaa käyttöjärjestelmän levyn luominen uudelleen AM. Tämä ei ole ongelma, jos palauttamisen strategia käyttöön tietojen jälkeen. Se kuitenkin olla ongelma, jos käytössäsi on lämpimän vara lähestymistapa varata kapasiteettia. Toteuttamisesta tämä oikein, sinulla on oltava oikea käyttöjärjestelmä levyn käyttöön ensisijainen ja toissijainen-sijainnit ja hakemuksen tiedot on tallennettu eri asemaan. Jos mahdollista Käytä vakio käyttöjärjestelmän määrityksistä, jotka voidaan suorittaa molemmista sijainneista. Jälkeen automaattisesti sinun on liitettävä tiedot-asema sitten aiemmin IaaS-VMs-toissijainen Ohjauskoneen. Kopioi tiedot näiden levyjen asemasta tilannevedosten etäpalvelimeen AzCopy avulla.

  * __Huomioon yhdenmukaisuuden ongelmia geo-automaattisesti useita AM levyjen jälkeen__. AM levyjen on toteutettu Azuren tallennustilaan BLOB-objektit ja on sama geo replikoinnin-ominaisuus. Ellei [Azure varmuuskopiointi](https://azure.microsoft.com/services/backup/) on käytössä, mikään ei ole yhtenäisyyden levyille, koska geo replikoinnin on asynkroninen ja kopioi erikseen. Yksittäisten AM levyjen on varmasti on yhtenäinen kaatumisen tilan jälkeen geo-automaattisesti, mutta ei ole sama kuin levyille. Tämä saattaa aiheuttaa ongelmia joissakin tapauksissa (esimerkiksi kohdalla levyn raitasarjoittamista).

##<a name="storage"></a>Tallennustilan

###<a name="recovery-by-using-geo-redundant-storage-of-blob-table-queue-and-vm-disk-storage"></a>Käyttämällä Geo tarpeettomat tallennustilaan blob, taulukkoon, jonossa ja AM levytilasta palauttaminen

Azure-BLOB-objektit, taulukoiden, olevien ja AM levyjä ovat kaikki geo replikoida oletusarvoisesti. Tätä kutsutaan nimellä Geo tarpeettomat Storage (GRS). GRS kopioi tallennuspaikkaan tietoja pisteparin palvelinkeskuksen satoja Mailia toisistaan tietyn maantieteellisen alueen sisällä. GRS on suunniteltu antamaan muita kestävyyttä siinä tapauksessa on pää palvelinkeskuksen huono. Microsoft-ohjausobjekteja, kun vikasietotila ilmenee ja automaattisesti on rajoitettu suurkatastrofeja, jossa ensisijaisen paikalleen katsotaan vakava-paljon aikaa. Joissakin tilanteissa voi olla useita päiviä. Tietoja yleensä replikoida muutaman minuutin kuluessa, vaikka synkronoinnin aikaväli ei ole vielä kata tason palvelusopimus.

Geo-automaattisesti, jos ei ole ei muutu siitä, miten tilin voi käyttää (URL-osoite ja tili-näppäin ei muutu). Tallennustilan-tili on kuitenkin olla toisella alueella jälkeen automaattisesti. Tämä voi olla vaikutusta sovellukset edellyttävät alueellisen affiniteetti niiden tallennustilan tilin kanssa. Myös palvelut ja sovellukset, jotka eivät vaadi tallennustilan-tiliä saman joten varten rajat-palvelinkeskuksen viive ja kaistanleveyden ovat ehkä voi näyttävien syitä liikenne siirtyy automaattisesti-kohtaan tilapäisesti. Tämä voi ottaa huomioon kokonaisvaltainen tietojen palauttaminen strategia kyselyjä.

Lisäksi automaattinen automaattisesti GRS myöntämä Azure on otettu käyttöön palvelu, joka antaa lukuoikeudet toissijainen tallennuspaikka tietojen kopio. Tätä kutsutaan lukuoikeudet Geo tarpeettomat Storage (RA GRS).

Saat lisätietoja GRS ja RA GRS tallennustilan [Azuren tallennustilaan replikoinnin](../storage/storage-redundancy.md).

###<a name="geo-replication-region-mappings"></a>GEO replikoinnin alueen määritykset:

On tärkeää tietää, missä tietojen geo-replikoida, jotta tiedä, mistä tiedot, jotka edellyttävät tallennustilan alueellisen affiniteetti muita esiintymiä käyttöönotto. Seuraavassa taulukossa on ensisijainen ja toissijainen sijainti parit:

[AZURE.INCLUDE [paired-region-list](../../includes/paired-region-list.md)]

###<a name="geo-replication-pricing"></a>GEO replikoinnin hinnat:

GEO replikoinnin sisältyy nykyistä hinnoittelua Azure-tallennustilan. Tätä kutsutaan Geo tarpeettomat Storage (GRS). Jos et halua tietojen geo replikoida geo replikoinnin voit poistaa käytöstä tilissäsi. Tätä kutsutaan paikallisesti tarpeettomat tallennustilan ja verrattuna GRS diskontatun hinta laskutetaan.

###<a name="determining-if-a-geo-failover-has-occurred"></a>Sen selvittäminen, jos geo-automaattisesti on tapahtunut

Jos geo-vikasietotila ilmenee, tämä kirjataan [Azure palvelun kunnon koontinäyttö](https://azure.microsoft.com/status/). Sovellusten ottaa käyttöön automaattisen keino tunnistaminen, mutta seurannan tallennustilan tilin geo-alueen avulla. Tämä voidaan käynnistää palautus muita toimintoja, kuten Laske resurssien geo-alueessa, jonne haluat siirtää niiden aktivoinnin. Voit suorittaa kyselyn tämän palvelun hallinnasta API, käyttämällä [Hae tallennustilan tilin ominaisuudet](https://msdn.microsoft.com/library/ee460802.aspx). Merkittävät ominaisuudet ovat seuraavat:

    <GeoPrimaryRegion>primary-region</GeoPrimaryRegion>
    <StatusOfPrimary>[Available|Unavailable]</StatusOfPrimary>
    <LastGeoFailoverTime>DateTime</LastGeoFailoverTime>
    <GeoSecondaryRegion>secondary-region</GeoSecondaryRegion>
    <StatusOfSecondary>[Available|Unavailable]</StatusOfSecondary>

###<a name="vm-disks-and-geo-failover"></a>AM levyjen ja geo automaattisesti

Kuten edellä osan AM levyille, mikään ei ole tietoja yhdenmukaisuuden AM levyille jälkeen automaattisesti. Varmuuskopioiden oikeellisuuden, jotta varmuuskopion tuotteen esimerkiksi tietojen suojauksen hallinta käytetään varmuuskopioiminen ja palauttaminen sovelluksen tietoja.

##<a name="database"></a>Tietokannan

###<a name="sql-database"></a>SQL-tietokantaan

Azure SQL-tietokanta on kahdentyyppisiä palautus: Geo palauttaminen ja aktiivinen Geo-replikoinnin.

####<a name="geo-restore"></a>GEO palauttaminen

[GEO palautus](../sql-database/sql-database-recovery-using-backups.md#geo-restore) on myös käytettävissä Basic, Vakio ja Premium-tietokannat. Se sisältää oletusarvoinen palautus-asetus, kun tietokanta ei ole käytettävissä vuoksi tapahtuma alueen, jossa nykyisessä tietokannassa. Kaltaisilta ajankohta: palauttaa Geo palauttaminen riippuvainen tietokannan varmuuskopioiden geo tarpeettomat Azure-tallennustilan. Palauttaa geo replikoida varmuuskopion, ja siksi joustavat tallennustilan katkokset ensisijainen alueen. Lisätietoja on artikkelissa [palauttaa Azure SQL-tietokanta tai siirtyy toissijaisen automaattisesti](../sql-database/sql-database-disaster-recovery.md).

####<a name="active-geo-replication"></a>Aktiivinen Geo-replikointi

[Aktiivinen Geo-replikointia](../sql-database/sql-database-geo-replication-overview.md) ei kaikki tietokannan tasoa. Se on suunniteltu sovelluksissa, joissa on enemmän kohdetoimialueen palautusvaatimukset kuin Geo palauttaminen tarjota. Käytä aktiivinen Geo-replikoinnin, voit luoda enimmillään neljä luettavissa secondaries palvelimissa eri alueilla. Voit aloittaa mihin tahansa secondaries automaattisesti. Lisäksi aktiivinen Geo-replikoinnin avulla voidaan tukea sovelluksen päivitys tai siirtäminen käyttötavoista sekä kuormituksen tasaus, vain luku-toiminnoista. Lisätietoja on artikkelissa [määrittää Geo replikoinnin](../sql-database/sql-database-geo-replication-portal.md) ja epäonnistuu [päälle toissijaisen tietokanta](../sql-database/sql-database-geo-replication-failover-portal.md). Viittaavat [rakenne cloud palauttaminen-sovelluksen aktiivinen Geo-replikoinnin SQL-tietokantaan](../sql-database/sql-database-designing-cloud-solutions-for-disaster-recovery.md) ja [hallinta juokseva päivitykset cloud-sovellusten avulla SQL, tietokanta, aktiivinen Geo-replikoinnin](../sql-database/sql-database-manage-application-rolling-upgrade.md) suunnittelu ja käyttöönotto sovellukset ja sovellusten päivitys ilman käyttökatkot käyttöönottamisesta.

###<a name="sql-server-on-virtual-machines"></a>Näennäiskoneiden SQL Server

Erilaisia asetukset ovat käytettävissä palautus- ja SQL Server 2012 (ja uudempi versio) suorittamisen Azuren näennäiskoneiden suuren käytettävyyden. Lisätietoja on artikkelissa [suuren käytettävyyden ja SQL Server Azuren näennäiskoneiden palauttaminen](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).

##<a name="other-azure-platform-services"></a>Muut Azure platform-palvelut

Kun yrität suorittaa pilvipalvelussa sijaitsevaan useita Azure alueilla, ota huomioon kunkin riippuvuudet vaikutukset. Seuraavissa osissa palvelukohtaisia ohjeet oletetaan, että sinun on käytettävä samaa Azure palvelua vaihtoehtoinen Azure palvelinkeskukseen. Tämä koskee määritys ja tietojen replikoinnin tehtävät.

>[AZURE.NOTE]Joissakin tapauksissa nämä vaiheet voi auttaa pienentämään palvelukohtaisia käyttökatkosta tapahtuman koko palvelinkeskuksen sijaan. Sovelluksen näkökulmasta palvelukohtaisia käyttökatkosta voi olla juuri kuin rajoittaminen ja edellyttäisi palvelun siirtämisestä tilapäisesti vaihtoehtoinen Azure alue.

###<a name="service-bus"></a>Palvelun Bus

Azure palvelun Bus käyttää yksilöllisen nimitilan, jotka ulottuvat ei Azure alueet. Niin ensimmäisen tarpeen on tarpeen palvelun bus nimitilan vaihtoehtoinen alueen asetukset. Välillä on kuitenkin myös varmistamiseksi huomioitavista jonossa olevat viestit. On useita strategioita replikoiminen viestien Azure alueiden välillä. Nämä replikoinnin strategioita ja muut varalle Lisätietoja on artikkelissa [turvallisuustarkoituksiin sovellusten vastaan palvelun Bus katkokset ja katastrofit parhaat käytännöt](../service-bus-messaging/service-bus-outages-disasters.md). Katso käytettävyys muuta huomioonotettavaa [Bus Service (saatavuuden)](./resiliency-technical-guidance-recovery-local-failures.md#other-azure-platform-services).

###<a name="app-service"></a>Sovelluksen-palvelu

Azure-sovelluksen Service-ohjelma, esimerkiksi verkkosovelluksissa tai Mobile-sovellusten siirtäminen toissijainen Azure alue, tarvitset varmuuskopion käytettävissä julkaisun sivuston. Jos käyttökatkosta ei liity koko Azure palvelinkeskukseen, saatat pystyä ladataan sivuston sisällön viimeisimmät varmuuskopiot FTP avulla. Valitse uuden sovelluksen luominen vaihtoehtoinen alueen, paitsi jos olet aiemmin tehnyt tämän varata kapasiteettia. Sivuston julkaiseminen uusi alue ja tee tarvittavat määritykset muutokset. Nämä muutokset voivat olla tietokannan yhteysmerkkijonoja tai muita aluekohtaisia asetuksia. Tarvittaessa lisätä sivuston SSL-varmenne ja muuttaa DNS CNAME-tietue, jotta mukautettua toimialuenimeä osoittaa redeployed Azure Web App-URL-osoite.

###<a name="hdinsight"></a>Hdinsightiin

HDInsight liittyvät tiedot tallennetaan oletusarvoisesti Azure-Blob-objektien tallennustilaan. HDInsight edellyttää, että Hadoop-klusterin käsittelyn MapReduce työt on sijaittava yhtä samalla alueella, joka sisältää tiedot, joita analysoida tallennustilan-tilinä. Jos käytät käytettävissä Azuren tallennustilaan geo replikoinnin-ominaisuutta, voit käyttää tietojen toissijainen alueen, jossa tiedot on replikoida, jos jostain syystä ensisijainen alue ei enää ole käytettävissä. Voit luoda uuden Hadoop-klusterin alueen, jossa tiedot on replikoida ja jatkaa sitä käsitellään. Katso käytettävyys muuta huomioonotettavaa [HDInsight (saatavuuden)](./resiliency-technical-guidance-recovery-local-failures.md#other-azure-platform-services).

###<a name="sql-reporting"></a>SQL-raportointi

Tällä hetkellä Azure alueen menettämistä jatkossa palauttamiseen tarvitaan useita SQL Reporting esiintymiä eri Azure alueilla. Nämä SQL Reporting esiintymät kannattaa käyttää samat tiedot ja tiedot on oltava oma palautus, jos huono suunnitteleminen. Voit ylläpitää ulkoisen varmuuskopioita kunkin raportin RDL-tiedoston.

###<a name="media-services"></a>Media-palveluita

Azure Media-palvelut on eri palautus-menetelmän koodaus ja streaming. Yleensä streaming on enemmän kriittinen alueellisen käyttökatkosta aikana. Valmistautuminen tämä on oltava Media Services-tilin kaksi eri Azure alueilla. Koodattu sisältö on sijoitettava molemmat alueet. Aikana virheen voit ohjata streaming liikenne vaihtoehtoinen alue. Koodaus voi käyttää mitä tahansa Azure alue. Jos aika-luottamukselliset koodaus on esimerkiksi live käsittely aikana, sinun on oltava valmiita, voit lähettää työt vaihtoehtoinen palvelinkeskukseen aikana virheet.

###<a name="virtual-network"></a>VPN

Tiedostojen on nopein tapa vaihtoehtoinen Azure alueen virtual verkon määrittäminen. Kun olet virtual verkon määrittäminen ensisijainen Azure alueen, verkon määritysten tiedoston nykyisen verkon [Vie VPN-asetukset](../virtual-network/virtual-networks-create-vnet-classic-portal.md) . Jos ensisijainen alueen käyttökatkosta, [palauttaa virtual verkon](../virtual-network/virtual-networks-create-vnet-classic-portal.md) tallennetun tiedoston. Määritä muut pilvipalveluihin näennäiskoneiden ja paikallisen-asetusten uusi virtual verkko-käyttöä varten.

##<a name="checklists-for-disaster-recovery"></a>Tarkistusluettelot palauttaminen

##<a name="cloud-services-checklist"></a>Cloud Services-tarkistusluettelo

  1. Tutustu tämän artikkelin Cloud Services-osio.
  2. Luo rajat-alueen tietojen palauttaminen strategia.
  3. Tietoja valinnat-varaaminen kapasiteetin vaihtoehtoinen alueilla.
  4. Käyttää liikenne reititys työkaluja, kuten Azure liikenteen hallinta.

##<a name="virtual-machines-checklist"></a>Näennäiskoneiden tarkistusluettelo

  1. Tutustu tämän artikkelin näennäiskoneiden-osio.
  2. [Azure varmuuskopion](https://azure.microsoft.com/services/backup/) avulla voit luoda eri alueilla sovelluksen yhdenmukaisia varmuuskopiot.

##<a name="storage-checklist"></a>Tallennustilan tarkistusluettelo

  1. Tutustu tämän artikkelin tallennustilan-osio.
  2. Tallennustilan resurssien geo-replikointia ei poista käytöstä.
  3. Tietoja vaihtoehtoinen alueen geo replikoinnin Jos automaattisesti.
  4. Luo mukautettu varmuuskopion strategioita käyttäjän ohjaa automaattisesti strategioita.

##<a name="sql-database-checklist"></a>SQL-tietokantaan tarkistusluettelo

  1. Tutustu tämän artikkelin SQL-tietokanta-osio.
  2. Käytä [Geo palauttaminen](../sql-database/sql-database-recovery-using-backups.md#geo-restore) tai [Geo replikoinnin](../sql-database/sql-database-geo-replication-overview.md) tarpeen mukaan.

##<a name="sql-server-on-virtual-machines-checklist"></a>SQL Server-näennäiskoneiden tarkistusluettelo

  1. Tutustu tämän artikkelin osio näennäiskoneiden SQL-palvelimeen.
  2. Käytä rajat-alueen AlwaysOn käytettävyys ryhmät tai tietokantapeilausta.
  3. Vuorotellen varmuuskopiointi ja palauttaminen blob storage.

##<a name="service-bus-checklist"></a>Palvelun Bus tarkistusluettelo

  1. Tutustu tämän artikkelin palvelun Bus-osio.
  2. Määritä Service Bus nimitilan vaihtoehtoinen alueen.
  3. Harkitse mukautetun replikoinnin strategioita viestit eri alueilla.

##<a name="app-service-checklist"></a>Sovelluksen tarkistusluettelo

  1. Tutustu tämän artikkelin App Service-osio.
  2. Säilyttää sivuston varmuuskopioiden ensisijainen alueen ulkopuolella.
  3. Jos käyttökatkosta on osittain, yritä hakea nykyisen sivuston kanssa FTP.
  4. Ottaa käyttöön sivuston uuteen tai aiemmin luotuun sivustoon vaihtoehtoinen alueen suunnitelma.
  5. Suunnittele määritysmuutoksia sovellus-ja DNS CNAME-tietueet.

##<a name="hdinsight-checklist"></a>HDInsight tarkistusluettelo

  1. Tutustu tämän artikkelin HDInsight-osio.
  2. Luo uuden Hadoop-klusterin alueen replikoitua tiedoilla.

##<a name="sql-reporting-checklist"></a>SQL Reporting tarkistusluettelo

  1. Tutustu tämän artikkelin SQL raportointi-osio.
  2. Ylläpidä vaihtoehtoinen SQL Reporting esiintymä toisella alueella.
  3. Ylläpidä erillisessä aiot replikoida kohde alueen.

##<a name="media-services-checklist"></a>Media-palveluiden tarkistusluettelo

  1. Tutustu tämän artikkelin Media Services-osio.
  2. Vaihtoehtoinen alueen Media Services-tilin luominen
  3. Koodata samaa sisältöä sekä alueilla tukemaan streaming automaattisesti.
  4. Lähettää koodauksen työt vaihtoehtoinen alueen Jos keskeytetty.

##<a name="virtual-network-checklist"></a>Virtuaalinen verkon tarkistusluettelo

  1. Tutustu tämän artikkelin VPN-osio.
  2. Käytä viedä uudelleenluomiseen toisen alueen VPN-asetukset.

##<a name="next-steps"></a>Seuraavat vaiheet

Tässä artikkelissa on kohdistettu [Azure vikasietoisuudesta tekniset ohjeet](./resiliency-technical-guidance.md)sarjaan kuuluvan. Seuraava artikkeli sarjassa keskitytään [palauttamisesta Azure paikallisen palvelinkeskukseen](./resiliency-technical-guidance-recovery-on-premises-azure.md).

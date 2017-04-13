<properties
   pageTitle="Tekniset ohjeet: paikallisen virheiden Azure palauttamisesta | Microsoft Azure"
   description="Artikkeli-tietoja ja suunnittelet joustavat, erittäin käytettävissä, vikasietoinen sovellusten sekä paikalliset virheet sisällä Azure kohdistettu palauttaminen suunnitteleminen."
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

#<a name="azure-resiliency-technical-guidance-recovery-from-local-failures-in-azure"></a>Azure vikasietoisuudelle tekniset ohjeet: palauttamisesta paikalliset virheet Azure-tietokannassa

On kaksi ensisijainen uhkien sovelluksen käyttöön:

* Laitteet, kuten asemat ja palvelinten virhe
* Kriittinen resurssit, kuten Laske piikin kuormituksen olosuhteissa ennenaikaisesti

Azure sisältää yhdistelmän Resurssienhallinta, joustavuus, kuormituksen tasaamisen ja jakaminen käyttöön suuri käytettävyys näissä tilanteissa. Joitain näistä toiminnoista suoritetaan automaattisesti kaikissa Azure-palveluissa. Joissakin tapauksissa sovelluksen kehittäjä on kuitenkin tehdä niistä hyötyvät lisätoimia.

##<a name="cloud-services"></a>Pilvipalveluihin

Azure pilvipalveluihin koostuu vähintään yksi verkossa tai työntekijä roolit sivustokokoelmat. Yhden tai useamman esiintymät rooli voidaan suorittaa samanaikaisesti. Kokoonpanon määrittää esiintymien määrän. Rooli esiintymät seurataan ja hallitaan osan, jonka nimi ohjauskoneen kangasta. Kangasta ohjauskoneen tunnistaa ja vastaa ohjelmiston ja laitteiston virheet automaattisesti.

Rooli jokaisen esiintymän suoritetaan omassa virtuaalikoneen (AM) ja välittää sen kangasta ohjaimella Vieras-agentti kautta. Vieras-agentti kerää resurssi ja solmun arvot, mukaan lukien AM käyttö, tila, lokit, resurssien käyttö, poikkeukset ja virheen ehdot. Kangasta ohjauskoneen kyselyt Vieras-agentti määritettäviä väliajoin ja se käynnistyy AM, jos Vieras-agentti lakkaa vastaamasta. Laitteistovirhe liittyvät kangasta ohjauskoneen siirtää kaikki tarvittavien roolin esiintymät uusi laitteisto-solmu ja telakoit verkon reitin liikenteen olemassa.

Hyötyvät näiden ominaisuuksien kehittäjät on varmistettava, että palvelun rooleista välttää tilan tallentamisesta roolin esiintymät. Sen sijaan kaikki pysyvien tietojen pitäisi voi käyttää kestävät tallennuspaikkaa, kuten Azuren tallennustilaan tai Azure SQL-tietokantaan. Näin pyyntöjen mitään roolia. Se tarkoittaa sitä, että rooli esiintymät voit siirtyä milloin tahansa ilman, että luot epäyhtenäisyydet palvelun lyhytkestoisia tai jatkuva-tilassa.

Tallentaa valtion ulkoisesti roolit vaatimus on useita vaikutuksia. Se merkitsee esimerkiksi, että kaikki liittyvät muutokset Azuren tallennustilaan taulukoksi olisi voi muuttaa yksittäisen kohteen ryhmän tapahtuman, jos mahdollista. Ei aina voi muuttaa yksittäisen tapahtuman kaikki. Sinun on otettava huolellisesti varmistaa, että roolin esiintymän virheet ei aiheuttaa ongelmia, kun ne keskeyttää pitkään käynnissä olevia toimintoja, jotka ulottuvat kahden tai useamman päivitykset pysyvä palvelun tila. Jos jonkin muun roolin yrittää tällaisen toimen uudelleen, se on tarkoitus ja käsitellä tapaus, jossa työn osittain suoritettu.

Esimerkkinä palvelu, joka osioiden tiedot yli useita stores. Jos työntekijä-roolin siirtyy samalla, kun se on kunnolliselle shard, siirtämiseen shard ei ehkä loppuun. Tai siirtämiseen saattavat näkyä sen alusta eri Työntekijä-roolin, mikä saattaa aiheuttaa yhteydetön tietoja tai tietovirheitä mukaan. Ongelmien välttämiseksi pitkään käynnissä olevia toimintoja on oltava jompikumpi tai molemmat seuraavista toimista:

 * *Idempotent*: toistettavien ilman vaikutukset. Jos idempotent, pitkään suoritettavien-toiminto olisi on saman tuloksen riippumatta siitä, kuinka monta kertaa, se on suoritettu, vaikka se keskeyttää suorituksen aikana.
 * *Vaiheittainen uudelleenkäynnistettävää*: voivat jatkaa virheen viimeisimmän lähtien. Voit vaiheittainen uudelleenkäynnistettävää, pitkään suoritettavien toiminto olisi koostuvat pienempi atomisia toimintojen sarjan. Se tallentaa etenemisen myös kestävät tallennustilaan niin, että kunkin myöhemmin kutsu vastataan Jos edeltäjä on lopetettu.

Lopuksi kaikki pitkään käynnissä olevia toimintoja olisi voi käynnistää toistuvasti, kunnes ne onnistuu. Valmistelu toiminto voi esimerkiksi voidaan sijoitettu Azure jonossa ja sitten poistetaan Työntekijä-roolin mukaan vain silloin, kun se onnistuu. Muistista täytyy mahdollisesti Puhdista tiedoissa on keskeytetty toimintojen luominen.

###<a name="elasticity"></a>Joustavuus

Aloitusnumero käynnissä kunkin roolin esiintymien määritetään kunkin roolin määrittäminen. Järjestelmänvalvojat aluksi kannattaa määrittää rooleille toimimaan perusteella arvioidut kuormituksen kahden tai useamman esiintymät. Mutta voit helposti skaalata roolin esiintymät tai alas käyttö kuviot muuttaa. Voit tehdä tämän manuaalisesti Azure-portaalissa, tai voit automatisoida käyttämällä Windows PowerShellin, Service Management API tai kolmannen osapuolen työkaluja. Lisätietoja on artikkelissa [miten Automaattinen skaalaus sovelluksen](../cloud-services/cloud-services-how-to-scale.md).

###<a name="partitioning"></a>Jakaminen

Azure kangasta ohjauskoneen käyttää kahdenlaisia osiot:

* *Päivitä toimialueen* käytetään päivittää palvelun roolin esiintymien ryhmissä. Azure ottaa käyttöön useita päivityksen toimialueille palvelun esiintymät. Paikallaan tapahtuvan päivityksen kangasta ohjauskoneen kokoaa kaikki esiintymät alaspäin yhden toimialue, päivittää ne ja käynnistää niitä uudelleen ennen siirtymistä seuraavan päivityksen toimialueen. Tämän menetelmän estää koko palvelu ei ole käytettävissä päivityksen aikana.
* *Virhe toimialueen* määrittää mahdolliset kohtiin tai verkko-virheen. Rooli, jossa on enemmän kuin yksi esiintymä-kangasta ohjauskoneen varmistaa, että esiintymät jakautuu useita vika toimialueiden toiminnan service eristettyjen laitteisto estäminen. Vian toimialueiden ohjaavat kaikki näyttäminen palvelimeen ja klusterin virheet.

[Azure palvelun tason sopimuksen (SLA)](https://azure.microsoft.com/support/legal/sla/) takaa, että kun vähintään kaksi web roolin esiintymät otetaan käyttöön eri vika ja päivittää toimialueet, ne on ulkoinen yhteys vähintään 99.95 prosenttia. Toisin kuin päivityksen toimialueita ei ei voi hallita vian toimialuemäärän. Azure varaa vika toimialueet ja jakaa niitä roolin esiintymät automaattisesti. AT jokaisen roolin vähintään kaksi ensimmäistä esiintymiä eri vika sijoitetaan, ja Päivitä toimialueet, kun haluat varmistaa, että kaikki rooli ja vähintään kaksi esiintymää täyttävät SLA. Seuraavassa kaaviossa edustaa.

![Virhe toimialueen eristyksen yksinkertaistettu näkymä](./media/resiliency-technical-guidance-recovery-local-failures/partitioning-1.png)

###<a name="load-balancing"></a>Kuormituksen

Web-roolin kaikki saapuva liikenne kulkee tilattomien kuormituksen, joka jakaa asiakkaan pyyntöihin roolin esiintymien välillä. Yksittäisten roolin esiintymiä ei ole julkisten IP-osoitteiden ja ne eivät ole käytettävissä suoraan Internetistä. Web roolit ovat tilattomien niin, että asiakkaan pyyntö voidaan reitittää minkä tahansa rooli-esiintymässä. [StatusCheck](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.statuscheck.aspx) -tapahtuma käynnistetään 15 sekunnin välein. Voit käyttää tätä onko rooli on valmis vastaanottamaan tietoliikennettä vai onko se on varattu ja olisi otettava ulos kuormituksen kiertoa.

##<a name="virtual-machines"></a>Näennäiskoneiden

Azure-Virtuaalikoneissa eroaa ympäristö kuin palvelu (PaaS) Laske monenlaista suhteessa suuren käytettävyyden roolit. Joissakin tapauksissa sinun täytyy tehdä lisätyötä, jotta suuren käytettävyyden.

###<a name="disk-durability"></a>Levyn kestävyyttä

Toisin kuin PaaS rooli esiintymät virtuaalikoneen asemat tallennetut tiedot on jatkuva myös silloin, kun virtuaalikoneen siirretä. Azure-virtuaalikoneissa käyttää AM levyjen siellä olevia BLOB Azuren tallennustilaan. Azuren tallennustilaan käytettävyys ominaispiirteet vuoksi virtual machine asemat tallennetut tiedot on myös erittäin käytettävissä.

Huomaa, että D-asemassa (kohdassa Windows VMs) poikkeus tähän sääntöön. D-asemassa on todella fyysinen tallennustilan Teline-palvelimeen, joka isännöi AM ja sen tiedot menetetään, jos AM on kuluessa. D-asemassa on tarkoitettu vain väliaikaisten. Linux-kohdassa Azure "yleensä" (mutta ei aina) paljastaa paikallisen tilapäinen levyn /dev/sdb estä laitteen. Se on usein käyttöön Azure Linux-agentti kuin /mnt/resource tai /mnt-liityntäkohdat sen (määritettäviä /etc/waagent.conf kautta).

###<a name="partitioning"></a>Jakaminen

Azure ymmärtää grafiikkatiedostomuotoja tasoa PaaS-sovelluksessa (web rooli ja työntekijän rooli) ja näin ollen oikein jakaa ne vika ja Päivitä toimialueilla. Sen sijaan tasoa infrastruktuuri kuin palvelusovelluksen (IaaS)-manuaalisesti on määritettävä käytettävyys joukot kautta. Käytettävyys joukot tarvitaan kohdassa IaaS SLA.

![Käytettävyys määrittää Azure-virtuaalikoneissa](./media/resiliency-technical-guidance-recovery-local-failures/partitioning-2.png)

Edellä olevassa kaaviossa Internet Information Services (IIS) tason (joka toimii web app-taso) ja SQL-tason (joka toimii tietojen taso) määritetään eri käytettävyys joukot. Näin varmistat, että kaikki esiintymät kunkin tason laitteiston redundancy jakamalla näennäiskoneiden vika toimialueilla ja, koko tasoa ei oteta päivityksen aikana.

###<a name="load-balancing"></a>Kuormituksen

Jos VMs olisi pitänyt liikenne ne jakautuminen-sovellusta ja lataa saldo VMs on ryhmittelet TCP tai UDP päätepisteen yli. Lisätietoja on artikkelissa [kuormituksen tasaamisen näennäiskoneiden](../virtual-machines/virtual-machines-linux-load-balance.md). Jos VMs saa syötteen toisesta lähteestä (esimerkiksi queuing järjestelmä), kuormituksen ei tarvita. Kuormituksen käyttää basic kuntotarkistuksen määrittääksesi, onko liikenne lähettää solmun. On myös mahdollista luoda omia keräysputkien toteuttamisesta sovelluksen kielikohtaiset kunto-arvot, jotka määrittävät, onko AM tietoliikenteen vastaanottamiseen.

##<a name="storage"></a>Tallennustilan

Azure-tallennustila on Azure perusaikataulun kestävät tietopalvelu. Se on blob, taulukko tai jonon AM levytilasta. Replikoinnin ja Resurssienhallinta yhdistelmän käyttää yhden palvelinkeskuksen hyvin käytettävyyttä. Azuren tallennustilaan käytettävyys SLA takaa, että vähintään 99,9 prosenttia:

* Oikein muotoiltu pyynnöt Lisää, Päivitä-, luku- ja poistaa tietoja oikein ja onnistuneesti käsitellään.
* Tallennustilan tilit on yhteys Internet-yhdyskäytävä.

###<a name="replication"></a>Replikoinnin

Azure-tallennustilan helpottaa tietojen kestävyyttä yllä kaikkien tietojen eri asemat useita kopioita täysin riippumaton fyysinen tallennustilan alijärjestelmien alueen yli. Tietojen replikoinnin synkronoidusti ja kopiot on vahvistettu, ennen kuin kirjoitus kuitata. Azure tallennus on erittäin yhdenmukaisia, mikä tarkoittaa, että lukee ovat taata vastaamaan viimeisimmän kirjoituksia. Kopioi tiedot tarkistetaan lisäksi jatkuvasti voit etsiä ja korjata bittinen rengasmädän usein huomaamatta uhkien, tallennettujen tietojen eheyden.

Palvelujen hyötyvät replikoinnin käyttämällä Azure-tallennustilan. Palvelun developer ei tarvitse tehdä muita työt palauttaminen paikallisen epäonnistui.

###<a name="resource-management"></a>Resurssienhallinta

Tallennustilan tilit luotu toukokuu 2014 jälkeen voi kasvaa enintään 500 Teratavun (edellisen enimmäismäärä on 200 TT). Jos tarvitset lisää tilaa, sovellukset on suunniteltu tallennustilan useita tilejä.

###<a name="virtual-machine-disks"></a>Virtuaalikoneen levyjen

Virtual machine levy on tallennettu sivun Blob-objektien Azuren tallennustilaan, anna sen samat kestävyys ja skaalattavuus ominaisuudet kuin Blob-objektien tallennustilaan. Tämän rakenteen ansiosta tiedot virtual machine levyllä pysyvä, vaikka palvelimessa, jossa AM epäonnistuu, ja AM on käynnistettävä uudelleen toisessa palvelimessa.

##<a name="database"></a>Tietokannan

###<a name="sql-database"></a>SQL-tietokantaan

Azure SQL-tietokanta on tietokantaan serviceksi. Sen avulla voit valmistella nopeasti, lisää tiedot ja kysely-relaatiotietokannat sovellukset. Se sisältää monia tuttuja SQL Server-ominaisuuksia ja toimintoja, aikana abstracting liittyvistä laitteiston, määritys, päivitetään ja vikasietoisuudelle.

>[AZURE.NOTE] Azure SQL-tietokantaan ei tarjoa yksi-yhteen-ominaisuus välistä eroa SQL Serverin kanssa. Se on tarkoitettu täyttämään on erilaisia vaatimuksia--sellainen, joka on yksilöllisesti soveltuvat cloud-sovellukset (joustavasti mittakaava, tietokannan palveluna pienentää ylläpito kustannuksia ja niin edelleen). Lisätietoja on artikkelissa [pilvestä SQL Server-vaihtoehdon valitseminen: SQL Azure (PaaS)-tietokanta tai SQL Server Azure VMs (IaaS)](../sql-database/sql-database-paas-vs-sql-server-iaas.md).

####<a name="replication"></a>Replikoinnin

Azure SQL-tietokanta sisältää valmiita vikasietoisuudelle solmu tason virhe. Kaikki kirjoituksia tietokantaan, automaattisesti replikoida kahden tai useamman taustan solmujen quorum Vahvista-tekniikka kautta. (Ensisijainen ja toissijainen vähintään yksi täytyy vahvistaa, että tehtävä on kirjoitettu lokiin ennen tapahtuman katsotaan onnistuneen ja palauttaa.) Solmun virheen, kun tietokanta siirtyy automaattisesti johonkin toissijainen replikoita. Tämä aiheuttaa lyhytkestoisia yhteyden keskeytyskohdasta, asiakassovellukset. Tästä syystä kaikkien Azure SQL-tietokanta-asiakkaiden on pantava täytäntöön joitakin lomakkeen lyhytkestoisia yhteyden käsittelyyn. Lisätietoja on artikkelissa [uudelleen palvelun tiettyjä ohjeita](../best-practices-retry-service-specific.md).

####<a name="resource-management"></a>Resurssienhallinta

Tietokantojen luotaessa on määritetty ylemmässä kokorajoituksen. Tällä hetkellä käytettävissä enimmäiskoko on 1 TT (koon rajoitukset vaihtelevat palvelutaso, katso [palvelutasot ja Azure SQL-tietokantoja tasot suorituskyvyn](../sql-database/sql-database-resource-limits.md#service-tiers-and-performance-levels)perusteella. Kun tietokannan käynnit ylemmässä kokorajoituksen, Hylkää Lisää tai päivitä komennoissa. (Kyselyt ja tietojen poistaminen on vielä mahdollista.)

Azure SQL-tietokanta käyttämällä tietokannassa, kangasta resurssien. Sen sijaan, että valvonta on kangasta se käyttää kuitenkin soi topologian esiintyvien virheiden. Klusterin jokaisen replikan on kaksi muiden tekijöiden ja vastaava tunnistaminen mennessään. Kun replikan menee, sen muiden tekijöiden käynnistäminen kokoonpanon uudelleenmääritys agentti, luo se uudelleen toisessa tietokoneessa. Ohjelma rajoittimen annetaan varmistaa, että loogisia palvelin ei käytä resursseja on liikaa tietokoneeseen tai suurempi kuin tietokoneen fyysinen rajoitukset.

###<a name="elasticity"></a>Joustavuus

Jos sovellus edellyttää enintään 1 TT tietokannan rajoitus, se on pantava asteikko-kohtaa lähestymistapa. Voit skaalata ulos Azure SQL-tietokannan jakaminen manuaalisesti, eli sharding, tietojen useita SQL-tietokannat. Mittakaava-kohtaa tämän menetelmän tarjoaa mahdollisuuden saavuttamiseksi lähes lineaarinen kustannukset kasvu ja asteikko. Joustavasti kasvu tai kapasiteetin pyydettäessä voi kasvaa vaiheittainen kustannuksia tarvita, sillä tietokantoja on laskuttaa keskimääräinen todellinen koko päivässä, jota käytetään perusteella ei ole mahdollista enimmäiskoko perusteella.

##<a name="sql-server-on-virtual-machines"></a>Näennäiskoneiden SQL Server

Asentamalla-Azuren näennäiskoneiden SQL Server (2014 tai uudempi versio), voit hyödyntää SQL Serverin perinteinen käytettävyys-ominaisuuksia. Nämä ominaisuudet ovat AlwaysOn käytettävyys ryhmiä ja tietokantapeilausta. Huomaa, että Azure VMs, varasto ja verkko on erilaisia kuin paikallisen,-virtualisoidussa IT-infrastruktuurin toiminnallisia ominaisuuksia. Hyvin käytettävyys ja palauttaminen (HA/DR) SQL Server-ratkaisun Azure onnistuneen soveltaminen edellyttää ymmärtää nämä erot ja suunnitella, jotta ratkaisu.

###<a name="high-availability-nodes-in-an-availability-set"></a>Suuren käytettävyyden solmujen käytettävyyden määrittäminen

Kun otat suuren käytettävyyden ratkaista Azure, voit määrittää Azure käytettävyys suuren käytettävyyden solmut erillisessä vika toimialueille ja päivittää toimialueet. Jos Poista, käytettävyys on Azure käsite. Parhaita käytäntöjä, joita noudattamalla voit varmistaa, että tietokantoja on todella hyvin käytettävissä, onko käytössäsi AlwaysOn käytettävyys ryhmiin, tietokantapeilausta tai jotakin muuta on. Paras käytäntö ei toimi, jos haluat ehkä EPÄTOSI olettaen, että järjestelmään on erittäin käytettävissä. Mutta todellisuudessa oman solmujen voi kaikki epäonnistua samanaikaisesti, koska ne tapahtuvat sijoittaa Azure alueen vika toimialue.

Tämä suositus ei ole tarvittaessa log toimitus kanssa. Tietojen palauttaminen-ominaisuudeksi on varmistettava, että palvelimet suoritetaan erillisessä Azure alueilla. Määritelmän näiden alueiden on erillinen vika toimialueita.

Azure Cloud Services VMs käyttöön palvelun perinteinen-portaalissa olevan samat käytettävyys, saat käyttöön ne samaan pilvipalvelussa. VMs käyttöön kautta Azure Resource Manager (nykyisen portaalin) ei ole tämä rajoitus. Varten perinteinen portal käyttöön VMs Azure pilvipalvelussa, vain saman pilvipalvelussa solmut voivat osallistua käytettävyys samat. Varmista, että ne ylläpitää niiden IP-osoitteet vaikka palvelun kenties virtual samassa verkostossa saadaan Cloud Services-VMs. DNS-päivityksen keskeytyksiä ei tarvitse.

###<a name="azure-only-high-availability-solutions"></a>Vain Azure: suuren käytettävyyden ratkaisuja

Käytössä voi olla suuren käytettävyyden ratkaista SQL Server Azure-tietokannan käyttämällä AlwaysOn käytettävyys ryhmille tai tietokantapeilausta.

Seuraavassa kaaviossa näytetään AlwaysOn käytettävyys ryhmät käynnissä Azuren näennäiskoneiden-arkkitehtuuri. Tässä kaaviossa on otettu yksityiskohtaiseen artikkeliin tämän aiheen [suuren käytettävyyden ja SQL Server-Azuren näennäiskoneiden palauttaminen](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).

![Microsoft Azure AlwaysOn käytettävyys ryhmät](./media/resiliency-technical-guidance-recovery-local-failures/high_availability_solutions-1.png)

Voit myös automaattisesti valmistelu AlwaysOn käytettävyys ryhmät käyttöönotto-kattavan Azure VMs-Azure portaalin AlwaysOn-mallin avulla. Lisätietoja on artikkelissa [SQL Server AlwaysOn tarjoaa Microsoft Azure Portal-valikoimassa](https://blogs.technet.microsoft.com/dataplatforminsider/2014/08/25/sql-server-alwayson-offering-in-microsoft-azure-portal-gallery/).

Seuraavassa kaaviossa näytetään tietokantapeilausta Azuren näennäiskoneiden-käyttö. Se on myös otettu perusteellisempaa aiheesta [suuren käytettävyyden ja SQL Server-Azuren näennäiskoneiden palauttaminen](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).

![Microsoft Azure tietokantapeilauksen](./media/resiliency-technical-guidance-recovery-local-failures/high_availability_solutions-2.png)

>[AZURE.NOTE] Molemmat arkkitehtuureihin edellyttää toimialueen ohjauskoneen. Tietokantapeilausta, jossa on kuitenkin mahdollista palvelimen varmenteiden käytöstä poistamista toimialueen ohjauskoneen tarvetta varten.

##<a name="other-azure-platform-services"></a>Muut Azure platform-palvelut

Sovellukset, jotka on suunniteltu Azure hyötyvät platform ominaisuuksia, Palauta paikalliset virheet. Joissakin tapauksissa voi tehdä niin, että tietty skenaario käytettävyyden toimenpiteitä.

###<a name="service-bus"></a>Palvelun Bus

Pienentämään vastaan väliaikaisen käyttökatkosta Azure palvelun Bus, kannattaa ehkä luoda kestävät asiakkaan jonossa. Tämä tilapäisesti viestit, joita ei voi lisätä palvelun Bus jonossa käytetään vaihtoehtoista, paikallinen tallennustilan-järjestelmä. Sovelluksen päättää käsittelemisestä tilapäisesti tallennetut viestit, kun palvelu on palautettu. Lisätietoja on artikkelissa [käyttäen palvelua Bus suorituskykyparannukset parhaat käytännöt se viestintä](../service-bus-messaging/service-bus-performance-improvements.md) ja [Palvelun Bus (palauttaminen)](./resiliency-technical-guidance-recovery-loss-azure-region.md#other-azure-platform-services).

###<a name="hdinsight"></a>Hdinsightiin

Tiedot, jotka on liitetty Azure Hdinsightiin tallennetaan oletusarvoisesti Azure-Blob-objektien tallennustilaan. Azure-tallennustilan määrittää Blob-objektien tallennustilaan suuren käytettävyyden ja kestävyyttä ominaisuudet. Usean solmun käsittelyn, johon on liitetty Hadoop MapReduce työt ilmenee, lyhytkestoisia Hadoop Distributed tiedoston järjestelmän (HDFS), kun Hdinsightista on se. Tulokset MapReduce-työstä myös tallennetaan oletusarvoisesti Azure-Blob-säiliö, niin, että käsitellyt tiedot kestävät ja pysyy käytettävissä, kun Hadoop-klusterin käyttömahdollisuus puretaan. Lisätietoja on artikkelissa [HDInsight (palauttaminen)](./resiliency-technical-guidance-recovery-loss-azure-region.md#other-azure-platform-services).

##<a name="checklists-for-local-failures"></a>Paikalliset virheet Tarkistusluettelot

###<a name="cloud-services"></a>Pilvipalveluihin

  1. Tutustu tämän artikkelin Cloud Services-osio.
  2. Määritä kunkin roolin vähintään kaksi esiintymät.
  3. Pysyvä tilaan kestävät tallennustilaa, ole-roolin esiintymät.
  4. Käsittele oikein StatusCheck tapahtuma.
  5. Rivittää liittyvät muutokset tapahtumat, kun se on mahdollista.
  6. Varmista, että työntekijä tehtävät ovat idempotent ja uudelleenkäynnistettävää.
  7. Kutsu toimintoja, kunnes ne onnistuu edelleen.
  8. Harkitse automaattisen skaalauksen poistaminen strategioita.

###<a name="virtual-machines"></a>Näennäiskoneiden

  1. Tutustu tämän artikkelin näennäiskoneiden-osio.
  2. Älä käytä D-asemassa pysyvän säilön.
  3. Voit ryhmitellä palvelutaso koneet käytettävyys määrittäminen.
  4. Kuormituksen tasaamisen ja valinnainen keräysputkien määrittäminen.

###<a name="storage"></a>Tallennustilan

  1. Tutustu tämän artikkelin tallennustilan-osio.
  2. Käytä useita tallennustilan tilejä, kun tietojen tai kaistanleveyden ylittää kiintiön.

###<a name="sql-database"></a>SQL-tietokantaan

  1. Tutustu tämän artikkelin SQL-tietokanta-osio.
  2. Ota käyttöön uudelleen käytännön lyhytkestoisia virheiden käsittelemiseen.
  3. Määritä jakaminen/sharding strategia asteikko-kohtaa.

###<a name="sql-server-on-virtual-machines"></a>Näennäiskoneiden SQL Server

  1. Tutustu tämän artikkelin osio näennäiskoneiden SQL-palvelimeen.
  2. Noudata edellisen suosituksia näennäiskoneiden.
  3. Käytä SQL Serverin suuren käytettävyyden ominaisuuksia, kuten AlwaysOn.

###<a name="service-bus"></a>Palvelun Bus

  1. Tutustu tämän artikkelin palvelun Bus-osio.
  2. Harkitse varmuuskopion kestävät asiakkaan jonon luomista.

###<a name="hdinsight"></a>Hdinsightiin

  1. Tutustu tämän artikkelin HDInsight-osio.
  2. Ei ole muita käytettävyys toimia ei tarvita, paikalliset virheet.

##<a name="next-steps"></a>Seuraavat vaiheet

Tässä artikkelissa on kohdistettu [Azure vikasietoisuudesta tekniset ohjeet](./resiliency-technical-guidance.md)sarjaan kuuluvan. Seuraava artikkeli sarjassa on [alueen laajuisen keskeytetty palauttamisesta](./resiliency-technical-guidance-recovery-loss-azure-region.md).

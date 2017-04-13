<properties
   pageTitle="Suuren käytettävyyden tarkistusluettelon | Microsoft Azure"
   description="Asetukset ja toiminnot, jotka voit kuitenkin varmistaa, että voit nopeasti tarkistusluettelon ovat parantaminen sovellusten käytettävyyden Azure kanssa."
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

#<a name="high-availability-checklist"></a>Suuren käytettävyyden tarkistusluettelo
Yksi Azure hyvien eduista on mahdollisuus pilven avulla sovellusten käytettävyyden (ja skaalattavuus). Varmista, että teet näistä asetuksista useimmat-alla tarkistusluettelon on tarkoitus avulla voit varmistaa, että sovelluksesi ovat joustavat osittain avaimen infrastruktuuri perusteet. 

>[AZURE.NOTE] Useimmat alla ehdotuksista asioita, jotka voidaan ottaa käyttöön sovelluksen milloin tahansa ja ovat näin ollen erinomaisesti "quick korjaukset". Parhaiten pitkään ratkaisu liittyy usein sovelluksen-rakenne, joka on luotu pilveen.  Tarkistusluettelo näihin (Lisää suunnittelun aloittaminen alueisiin, Lue Microsoftin [käytettävyyttä tarkistusluettelon](../best-practices-availability-checklist.md).

###<a name="are-you-using-traffic-manager-in-front-of-your-resources"></a>Käytät liikenteen hallinta eteen resurssien?
Liikenteen hallinta auttaa reitittää internet-liikenteen Azure alueiden tai Azuren ja paikallisten sijainnit yli. Voit tehdä tämän syistä, mukaan lukien viive ja käytettävyyden määrälle. Jos haluat lisätietoja käyttämisestä liikenteen hallinta oman vikasietoisuudesta ja kerro tietoliikenteestä alueille, lue [Käynnissä VMs useita palvelinkeskusten suuren käytettävyyden Azure-kohdassa](../guidance/guidance-compute-multiple-datacenters.md).

__Mitä tapahtuu, jos et käytä liikenteen hallinta?__ Jos käytät liikenteen hallinta sovelluksen edessä, sinun on rajoitettu vain yhden alueen resurssien. Tämä rajoittaa oman mittakaava, kasvaa viive käyttäjille, jotka eivät ole lähellä valitulla alueella ja laskee alueen laajuinen keskeytetty kyseessä suojausta.

###<a name="have-you-avoided-using-a-single-virtual-machine-for-any-role"></a>Olet, välttää käyttämällä minkä tahansa roolin virtual yhteen tietokoneeseen?
Hyvän suunnittelun vältetään mitä tahansa yksittäistä pistettä virheestä. Tämä on tärkeää kaikki palvelun suunnittelu (paikallisen tai pilvipalvelussa), mutta on hyötyä pilveen voi parantaa skaalattavuus ja vikasietoisuudelle, vaikka laajentaminen (lisääminen näennäiskoneiden) sijaan skaalaus ylöspäin (joko tehokkaampia virtual machine). Jos haluat lisätietoja skaalattava sovelluksen suunnitella, lue [suuren rakennettu Microsoft Azure-sovellusten käytettävyyden](resiliency-high-availability-azure-applications.md).

__Mitä tapahtuu, jos sinulla on roolin virtual yhteen tietokoneeseen?__ Yhteen tietokoneeseen on virhe yksittäisen kohdan ja ei ole käytettävissä, [Azure virtuaalikoneen taso-palvelusopimus](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_0/). Paras tapauksissa suoritetaan vain Hieno, mutta tämä ei ole joustavat rakenne, ei koske Azure virtuaalikoneen SLA ja mikä tahansa yksittäisen kohdan virheen kasvaa epäonnistuu, jos jokin käyttökatkot mahdollisuutta.

###<a name="are-you-using-a-load-balancer-in-front-of-your-applications-internet-facing-vms"></a>Käytät kuormituksen eteen sovelluksen Internetiin yhteydessä oleva VMs?
Kuormituksen tasoitusmääritykset avulla voit levittää saapuvan liikenteen sovelluksen yli koneet haluamaansa määrä. Voit voit Lisää/Poista koneet kuormituksen tasauspalvelun milloin tahansa, joka toimii hyvin näennäiskoneiden on (ja myös Automaattinen skaalaus virtuaalikoneen asteikko joukot kanssa), jotta voit käsitellä helposti lisäykset liikenne tai AM epäonnistuu. Jos haluat lisätietoja kuormituksen tasoitusmääritykset, lue [Azure ladata tasaustoiminto yleiskatsaus](../load-balancer/load-balancer-overview.md) ja [useita VMs skaalattavuus ja käytettävyys Azure-käyttöjärjestelmä](../guidance/guidance-compute-multi-vm.md).

__Mitä tapahtuu, jos et käytä kuormituksen tasauspalvelun eteen Internetiin yhteydessä oleva VMs?__ Kuormituksen tasauspalvelun ilman ei osaat skaalata ulos (Lisää Lisää näennäiskoneiden) ja ainoana vaihtoehtona päivitetään skaalata (web osoittava virtuaalikoneen koon kasvattaminen). Voit myös yleisölle tarkoitetun yksittäisen kohdan kyseisen virtuaalikoneen virhe. Sinun on myös kirjoittaa DNS-koodia, huomaat, jos on katkennut Internetiin yhteydessä oleva machine ja Yhdistä uudelleen DNS-tiedot uuteen tietokoneeseen, voit aloittaa sen asetetaan.

###<a name="are-you-using-availability-sets-for-your-stateless-application-and-web-servers"></a>Ovat käyttäen käytettävyys määrittää tilattomien sovellus ja WWW-palvelimien?
Oman VMs tietokoneesi sijoittaminen sovelluksen samalla tasolla käytettävyys määrittäminen on oikeutettu Azure AM SLA. Määritä myös saatavuus osaksi varmistaa, että tietokoneesi liikkeelle eri päivitys domains (eli eri host koneet, joka on asennettu eri aikoina) ja vika domains (eli host koneet, joilla on yhteiset power lähde- ja verkon vaihtaa). Olematta käytettävyys määrittäminen oman VMs löytynyt host samaan tietokoneeseen ja näin saattaa olla yksittäisen kohdan virhe, joka ei ole näkyvissä sinulle. Jos haluat lisätietoja lisääntyvien oman VMs käytettävyys joukkojen avulla käytettävyyden selvittäminen, lue [hallinta näennäiskoneiden käytettävyyttä](../virtual-machines/virtual-machines-windows-manage-availability.md).

__Mitä tapahtuu, jos et käytä määrittäminen tilattomien sovellukset ja WWW-palvelimien saatavuus?__ Käytä saatavuus määrittää tarkoittaa, että et voi hyödyntää Azure AM SLA. Se tarkoittaa sitä, että sovelluksesi kerroksen koneet voitu kaikki siirtyä offline-tilassa, onko päivitys host machine (käytät VMs isännöivän tietokoneen) tai yleisten epäonnistui.

###<a name="are-you-using-virtual-machine-scale-sets-vmss-for-your-stateless-application-or-web-servers"></a>Käytät virtuaalikoneen asteikko joukot (VMSS) tilattomien sovellusta tai web-palvelimia?
Hyvä skaalattava ja joustavat rakenne käyttää VMSS, varmista, että olet voit suurentaminen tai kutistaminen sovelluksen taso koneet määrä (esimerkiksi web-taso). VMSS avulla voit määrittää, miten sovelluksen taso Skaalaa (lisäämällä tai poistamalla määritettyjen ehtojen perusteella palvelimet). Jos haluat lisätietoja käyttämisestä Azure virtuaalikoneen asteikko joukot niin, että liikenne piikkarit, että vikasietoisuudelle, lue [Virtuaalikoneen asteikko joukot Yleiset](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md).

__Mitä tapahtuu, jos et meille virtuaalikoneen asteikko määrittäminen tilattomien sovellukseni web-palvelimen kanssa?__ Ilman VMSS voit rajoittaa skaalata ilman rajoituksia ja optimointi resurssien käyttöä. Rakenteen, jossa ei ole VMSS on skaalauksen yläraja, joka on käsiteltävä koodia (tai manuaalisesti). Tämä VMSS puuttuminen tarkoittaa, että sovelluksesi voit lisätä ja poistaa laitteiden (riippumatta asteikko) auttaa käsitellään liikenteen suuri piikkarit ei helposti (esimerkiksi ylennyksen aikana tai jos sivuston/app/tuotteen virusperäisen).

###<a name="are-you-using-premium-storage-and-separate-storage-accounts-for-each-of-your-virtual-machines"></a>Sinulla on käytössä premium tallennustilan ja erillinen tallennustilan tilit kunkin käyttäjän näennäiskoneiden?
Paras käytäntö on käyttää premium tallennustilan tuotannon näennäiskoneiden on. Lisäksi Varmista että erillisessä tallennustilan tilin käyttäminen kunkin virtuaalikoneen (tämä on tosi pieniä käyttöönotoissa. Suurempi käyttöönotoissa, sitä voi käyttää uudelleen tallennustilan tilit, mutta useita tietokoneissa on tasaamisen, joka on tehtävä varmistamiseksi ovat saapuva päivityksen toimialueilla ja tasojen sovelluksen kautta). Jos haluat lisätietoja Azure-tallennustilan suorituskyky ja skaalattavuus, lue [Microsoft Azure tallennustilan suorituskyky ja skaalattavuus tarkistusluettelon](../storage/storage-performance-checklist.md).

__Mitä tapahtuu, jos et käytä erillisessä tallennustilan tilit kunkin virtuaalikoneen varten?__ Tallennustilan-tilin, kuten paljon resursseja on yksi virheestä. Vaikka monet suojaukset ja Azure-tallennustilan vikasietoisuudelle ominaisuuksia, virheen yksittäisen kohdan ei ole koskaan hyvä rakenne. Jos käyttöoikeudet vioittunut että tilin tallennustila on osumien tai [IOPS rajoittaa](../azure-subscription-service-limits.md#virtual-machine-disk-limits) saavutetaan, joihin esimerkiksi kaikki näennäiskoneiden tallennustilan tilin avulla. Lisäksi, jos on keskeytetty, jotka vaikuttaa tallennustilan aikaleiman, joka sisältää tietyn tallennustilan tilin voi olla useita näennäiskoneiden vaikuttaa.

###<a name="are-you-using-a-load-balancer-or-a-queue-between-each-tier-of-your-application"></a>Käytät kuormituksen tasauspalvelun tai jonon kunkin tason sovelluksesi välillä?
Kuormituksen tasoitusmääritykset tai olevien välillä kunkin tason sovelluksen avulla voit helposti skaalata kunkin tason sovelluksesi itsenäisesti ja helposti. Sinun on valittava välinen viive-monimutkaisuus, perusteella tekniikalla ja jakauma (eli kuinka paljon jaat sovelluksen) on. Yleensä olevien yleensä suurempi viive ja lisää monimutkaisuus mutta hyötyä sinulla on enemmän joustavat ja jonka avulla voi jakaa sovelluksen suurempaa aluetta (kuten eri alueilla). Jos haluat lisätietoja siitä, miten voit käyttää sisäistä kuormituksen tasoitusmääritykset tai olevien, lue [sisäinen ladata tasaustoiminto yleiskatsaus](../load-balancer/load-balancer-internal-overview.md) ja [Azure ja palvelun Bus olevien - verrattuna ja asiaan](../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md).

__Mitä tapahtuu, jos et käytä kuormituksen tai jono kunkin tason sovelluksesi välillä?__ Kuormituksen tai jonossa, ilman kunkin tason sovelluksesi välillä on vaikeaa skaalata sovelluksen ylös tai alas ja jakaa sen Lataa useita tietokoneissa. Ei tällöin voi aiheuttaa päälle tai valmistelu resurssien ja riskin käyttökatkot ja heikko käyttökokemus-kohdassa Jos sinulla on odottamattomia muutokset liikenne ja järjestelmän virheet.
 
###<a name="are-your-sql-databases-using-active-geo-replication"></a>Käytät SQL-tietokantoja aktiivinen geo-replikoinnin? 
Aktiivinen Geo-replikoinnin avulla voit määrittää 4 luettavissa toissijainen tietokantojen samassa tai eri alueilla. Toissijainen tietokantoja on keskeytetty tai ensisijainen tietokantayhteyden muodostamisessa ei käytettävissä. Jos haluat lisätietoja SQL-tietokannan aktiivinen geo replikoinnin, lue [yleiskatsaus: SQL, tietokanta, aktiivinen Geo-replikoinnin](../sql-database/sql-database-geo-replication-overview.md).
 
 __Mitä tapahtuu, jos et käytä active geo-replikoinnin SQL-tietokantoja?__ Ilman aktiivinen geo-toistoa, jos ensisijainen tietokannan koskaan menee offline-tilassa (suunniteltu ylläpito, palvelu, laitteistovirheen jne.) sovelluksen tietokanta on offline-tilassa ennen kuin voit tuoda online-tilaan ensisijaisen tietokannan kunnossa-tilaan. 
 
###<a name="are-you-using-a-cache-azure-redis-cache-in-front-of-your-databases"></a>Käytät välimuistin (Azure Redis välimuisti) edessä tietokantoja?
Jos sovellus on hyvin tietokannan kuormituksen kohtaa, johon useimmat tietokannan kutsujen ovat lukuja, voit nopeuttaa sovelluksesi ja pienentää tietokannassa kuormituksen käyttöönoton välimuistiin kerroksen eteen tietokannan purku luku toiminnoista. Voit nopeuttaa sovelluksen ja pienentää sijoittamalla välimuistiin kerrosta eteen tietokannan että tietokannan latautuvat (kasvava näin voi käsitellä asteikko). Jos haluat lisätietoja Azure Redis välimuistin, lue [ohjeet välimuisti](../best-practices-caching.md).
 
 __Mitä tapahtuu, jos et käytä välimuistin eteen tietokannan?__ Jos tietokannan tietokone on tehokas käsitellään liikenteen kuormituksen laitetaan se sitten sovelluksen vastaa normaalisti, vaikka tämä saattaa tarkoittaa, että alemman ladattaessa voit maksat tietokannan tietokoneelle, joka on kalliimpi kuin on tarpeen. Jos tietokoneen tietokanta ei ole tehokas tarpeeksi käsittelemään että latautuvat sitten käynnistät koet heikko käyttäjän kohdata sovelluksen (viive aikakatkaisu ja mahdollisesti palvelun käyttökatkot).
 
###<a name="have-you-contacted-microsoft-azure-support-if-you-are-expecting-a-high-scale-event"></a>Olet yhteyttä Microsoft Azure-tuen jos odotat hyvin asteikko-tapahtuma?
Azure tuki avulla voit parantaa service-rajoitukset käsitellä suunniteltu hyvin liikenne tapahtumat (kuten uuden tuotteen käynnistyy tai erityinen juhlapäivät). Azure tukea myös ehkä avulla voit yhdistää asiantuntijoiden kuka ohjeen Tarkista suunnittelemasi tilin työryhmän kanssa ja auttavat löytävät hyvin asteikko tapahtuman tarpeiden paras mahdollinen ratkaisu. Jos haluat lisätietoja siitä, miten voit Azure tuelta, lue [Azure tukevat usein kysyttyjä kysymyksiä](https://azure.microsoft.com/support/faq/).

__Mitä tapahtuu, jos ei ota yhteyttä tukeen Azure suuri-akseli-tapahtuman?__ Jos et viestiä tai suunnitteleminen, suuri tietoliikenne tapahtuma-ja voit tuota pallolla tiettyjä [Azure services rajoittaa](../azure-subscription-service-limits.md) ja luoda näin heikko käyttäjäkokemuksen (tai worse, käyttökatkot) tapahtuman aikana. Arkkitehtuuri tarkistukset ja niistä ennen virtatason voi auttaa pienentämään näitä riskejä.

###<a name="are-you-using-a-content-delivery-network-azure-cdn-in-front-of-your-web-facing-storage-blobs-and-static-assets"></a>Käytät sisällön toimituksen verkon (Azure CDN) edessä web osoittava tallennustilan BLOB-objektit ja kiinteät resurssit?
CDN avulla pääset käytöstä palvelinten kuormituksen tallentamalla välimuistiin CDN POP/reunan sijainnit, jotka sijaitsevat eri puolilla maailmaa sisältöä. Voit tehdä tämän pienentää viive, Suurenna skaalattavuus, pienennä palvelimen kuormituksen ja strategia suojautumista vastaan service(DOS) kalastelu eston osana. Jos haluat lisätietoja siitä, miten voit lisätä oman vikasietoisuudelle tai vähentää asiakkaan viive Azure CDN avulla, lue [yleiskatsaus, Azure sisällön toimituksen verkon (CDN)](../cdn/cdn-overview.md).

__Mitä tapahtuu, jos et käytä CDN?__ Jos et käytä CDN kaikki asiakkaan liikenne tulee siirtyä resurssit. Tämä tarkoittaa, että näet suurempi Lataa joka vähentää niiden skaalattavuus palvelimiin. Lisäksi asiakkaille saattaa ilmetä suurempi viiveitä Sisältöverkot tarjoaa eri puolilla maailmaa sijainnit, jotka todennäköisesti lähemmäksi asiakkaillesi.

##<a name="next-steps"></a>Seuraavat vaiheet:
Jos haluat lukea lisää suunnitella hyvin käytettävyyden sovellusten Lue [suuren rakennettu Microsoft Azure-sovellusten käytettävyyden](resiliency-high-availability-azure-applications.md).

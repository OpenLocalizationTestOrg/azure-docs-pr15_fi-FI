<properties
   pageTitle="Vikasietoisuudelle tarkistusluettelon | Microsoft Azure"
   description="Tarkistusluettelon, jossa on ohjeita vikasietoisuudelle koskee suunnittelun aikana."
   services=""
   documentationCenter="na"
   authors="petertaylor9999"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="best-practice"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="petertay"/>

# <a name="azure-resiliency-guidance-resiliency-checklist"></a>Azure vikasietoisuudelle ohjeet: Vikasietoisuudelle tarkistusluettelo

Sovelluksen vikasietoisuudelle tarvitaan suunnittelusta ja pienentävät virheen tilat, joita saattaa ilmetä eri. Tarkista valmistelutyöt vastaan sovelluksen-rakenne, jotta se Lisää joustavat kohteet.

## <a name="requirements"></a>Vaatimukset

- **Määritä asiakkaan käytettävyys vaatimuksia.** Asiakkaan on käytettävyys osia koskevat vaatimukset sovelluksessa ja tämä vaikuttaa sovelluksen rakenne. Sopimuksen hankkiminen käytettävyys kohteet kunkin tekstiosion sovelluksesi asiakkaalle, muuten suunnittelemasi saattaa vastannut asiakkaan odotuksia. Lisätietoja [Azure joustavat-sovellusten suunnitteleminen](guidance-resiliency-overview.md) asiakirjan [vikasietoisuudelle tarpeen määrittäminen](guidance-resiliency-overview.md#defining-your-resiliency-requirements) -osassa.

## <a name="failure-mode-analysis"></a>Virhe tilassa analyysi

- **Suorittaa sovelluksen virheen tila-analyysin (FMA).** FMA jakaantuu etsimisen vikasietoisuudelle aikaisin suunnitteluvaiheessa-sovellukseen. FMA tavoitteet ovat seuraavat:  

    - Määritä, minkä tyyppisiä virheet sovelluksen kärsiä. 
    - Sieppaa mahdolliset tehosteet ja vaikutus erityyppisiin sovelluksen virhe.
    - Määritä palautus strategioita. 

    Lisätietoja on artikkelissa [Azure joustavat-sovellusten suunnitteleminen: Virhe tilassa analysis][fma].  

## <a name="application"></a>Sovelluksen

- **Ottaa käyttöön useita kertoja palvelut.** Palvelujen väistämättä epäonnistuu, ja jos sovellus riippuu yksittäisen esiintymän palvelun väistämättä epäonnistuu myös. [Azure sovelluksen](../app-service/app-service-value-prop-what-is.md)-palvelun käytön valmistelu useita kertoja, valitse [Sovelluksen-palvelun suunnittelu](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) , joka tarjoaa useita kertoja. Määrittää Azure pilvipalveluihin kunkin käyttäjän roolien käyttää [useita kertoja](../cloud-services/cloud-services-choose-me.md#scaling-and-management). [Azuren näennäiskoneiden (VMs)](../virtual-machines/virtual-machines-windows-about.md), varmista AM-arkkitehtuuri sisältää useamman kuin yhden AM ja kunkin AM sisältyy [saatavuus][availability-sets].   

- **Kuormituksen tasauspalvelun avulla voit jakaa pyynnöt.** Kuormituksen tasauspalvelun jakaa sovelluksen pyynnöt kunnossa palveluesiintymiä poistamalla perusasemassa esiintymät kierto. Jos käyttää Azure App palvelun tai Azure pilvipalveluihin, se on jo kuormitus tasataan puolestasi. Jos sovellus käyttää Azure VMs, sinun on valmistelu kuormituksen. Katso lisätietoja [Azure kuormituksen](../load-balancer/load-balancer-overview.md) yhteenvedossa. 

- **Määritä Azure sovelluksen yhdyskäytävien käyttämään useita kertoja.** Sovelluksen vaatimukset mukaan [Azure sovelluksen yhdyskäytävän](../application-gateway/application-gateway-introduction.md) voi sopia paremmin jakaminen pyynnöt sovelluksen palveluihin. Kuitenkin yhden sovelluksen Gateway-palveluun esiintymät taata SLA mukaan se on mahdollista, että sovelluksesi voi epäonnistuu, jos sovellus yhdyskäytävän esiintymän epäonnistuu. Valmistele useita Normaali tai suurempi sovelluksen yhdyskäytävän esiintymän taata käytettävyys palvelun [SLA](https://azure.microsoft.com/support/legal/sla/application-gateway/v1_0/)ehtojen mukaisesti.

- **Käytä käytettävyys määrittää sovelluksen kunkin tason**. Siten kopioita [saatavuus] [ availability-sets] takaa vähintään yksi AM esiintymä yhteys [SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_2/)mukaisesti. Jos yhteyttä VMs eivät käytettävyys määrittää siihen ei ole takaa suojaaminen ne ja on mahdollista, että ne voi kaikki epäonnistua tai päivittää samanaikaisesti. 

- **Harkitse sovelluksen käyttöönotto useiden alueiden välillä.** Jos sovellus otetaan käyttöön vain yhden alueen, palauttaa koko aluetta ei ole käytettävissä, sovelluksen myös ei ole käytettävissä. Tämä voi johtua voi hyväksyä sovelluksen SLA ehtojen mukaisesti. Jos näin on, harkitse sovelluksen ja palveluja käyttöönotto useiden alueiden välillä. Monille käyttöönoton käyttää (jakaminen pyynnöt useita aktiivisia esiintymiä eri) aktiivinen aktiivinen-kuvion tai (pitäminen varaa, "kirja" esiintymän, siltä varalta, että ensisijainen esiintymän epäonnistuu) Aktiivinen-passiivinen-kuvion. On suositeltavaa käyttöön useita kertoja sovelluksen services yli aluekohtaiset parit. Lisätietoja on artikkelissa [liiketoiminnan jatkuvuuden ja tietojen palauttaminen (BCDR): Azure Parittainen alueiden](../best-practices-availability-paired-regions.md).

- **Toteuta vikasietoisuudelle kuviot remote toimille tarvittaessa.** Jos sovellus riippuu välisen Etätyöpöytä, viestintä-polku väistämättä epäonnistuu. Jos luettelossa on useita virheitä, sovelluksen palveluiden jäljellä olevan kunnossa esiintymiä voi olla täyttyminen pyynnöt. Useita kuvioita on hyötyä käsittely Yleiset virheet, mukaan lukien aikakatkaisu kohdalla, [Yritä kuvion]kanssa[retry-pattern], [katkaisin] [ circuit-breaker] kuviota ja muiden. Lisätietoja on artikkelissa [Azure joustavat-sovellusten suunnitteleminen](guidance-resiliency-overview.md#implementing-resiliency-strategies). 

- **Automaattisen skaalauksen poistaminen avulla voit vastata kuormituksen kasvaa.** Jos sovellusta ei ole määritetty skaalata kuormituksen kasvaa automaattisesti, on mahdollista, että sovelluksesi services epäonnistuu, jos kyllästyä pyynnöt. Lisätietoja on seuraavissa artikkeleissa:

    - Yleinen: [skaalattavuus tarkistusluettelo](../best-practices-scalability-checklist.md) 
    - Azure sovelluksen-palvelu: [skaalata esiintymän Laske manuaalisesti tai automaattisesti][app-service-autoscale]
    - Pilvipalveluihin: [kuinka Automaattinen skaalaus pilvipalveluun][cloud-service-autoscale]
    - Näennäiskoneiden: [Automaattinen skaalaus ja virtuaalikoneen skaalaus-vaihtoehdon avulla][vmss-autoscale]


- **Ota käyttöön asynkroninen toimintojen aina, kun se on mahdollista.** Synkronoitujen toimintojen voi viedä resurssit ja estää muita toimintoja, kun kutsuja odottaa Viimeistele prosessi. Suunnittele jokaisen osan sovelluksesi sallimaan asynkronisten toimintojen aina, kun se on mahdollista. Katso lisätietoja ottamisesta käyttöön asynkroninen ohjelmoinnin C# [asynkroninen asynkroninen ohjelmointi ja odotettava][asynchronous-c-sharp].

- **Azure liikenteen hallinnan avulla voit reitittää liikenteen sovelluksen eri alueilla.**  [Azure liikenteen hallinta] [ traffic-manager] suorittaa DNS tasolla kuormituksen ja voit reitittää liikenteen perusteella [liikenne reititys] eri alueiden[ traffic-manager-routing] määrittämäsi menetelmän mukaan ja sovelluksen päätepisteet kunto. 

- **Määritä ja testaa kunto keräysputkien kuormituksen tasoitusmääritykset ja liikenteen valvojat.** Varmista, että kunto-logiikkaa kriittiset osat järjestelmä tarkistaa eivätkä reagoi kunto keräysputkien asianmukaisesti.

    - Kuntotietojen probes [Azure liikenteen] hallinnan[ traffic-manager] ja [Azure kuormituksen] [ load-balancer] yhteyshenkilönä tiettyä toimintoa. For liikenteen hallinta kunto näytteenottimen määrittää, ei onnistu toisen alueen yli. Kuormituksen, saat sen määrittää, voit poistaa AM kierto.      

    - Liikenteen hallinta-näytteenottimen kunto päätepisteen Tarkista tärkeät riippuvuuksia, jotka on otettu käyttöön sama alueella ja jonka virheen aiheuttaa toisen alueen siirtyy automaattisesti.  

    - Kuormituksen terveys-päätepisteen Ilmoita AM kunto. Älä sisällytä muiden tasoa tai ulkoisia palveluja. Muussa tapauksessa virhe, joka esiintyy ulkopuolella AM aiheuttavat kuormituksen AM poistaminen kierto.

    - Ohjeita yksityiskohtaisista kunnon valvonta-sovelluksessa on artikkelissa [Kunto päätepisteen seuranta kuvio](https://msdn.microsoft.com/library/dn589789.aspx).

- **Seurata kolmannen osapuolen palveluita.** Jos sovellus on kolmannen osapuolen palvelujen riippuvuudet, tunnistamaan where kuinka kolmannen osapuolen palveluun voi epäonnistua ja mitä Tehosteasetukset kyseiset virheet on sovelluksessa. Kolmannen osapuolen palvelun ei välttämättä sisällä seuranta- ja Diagnostiikka, joten on tärkeää Kirjaudu oman ohjelmarakennekaaviossa ne ja yhdistää ne sovelluksen terveyteen liittyvät ja diagnostiikan kirjaamisen käyttäen yksilöllinen. Katso lisätietoja parhaat käytännöt seuranta- ja Diagnostiikka- [näyttö ja diagnostiikka ohjeet] [ monitoring-and-diagnostics-guidance] asiakirjan.

- **Varmista, että kaikki kolmannen osapuolen palveluun, voit käyttää sisältää SLA.** Jos sovellus riippuu kolmannen osapuolen palveluun, mutta kolmannen osapuolen sisältää ei SLA lomakkeen käytettävyys takaa, myös sovelluksen käytettävyys ei voida taata. Oman SLA on vain yhtä hyvä sovelluksen vähintään saatavana osana.


## <a name="data-management"></a>Tietojen hallinta

- **Tietoja replikoinnin menetelmät sovelluksen tietolähteet.** Hakemuksen tiedot tallennetaan eri tietolähteistä ja on eri käytettävyys vaatimuksia. Laskeminen mistäkin tietojen tallennuksen Azure replikoinnin menetelmiä, mukaan lukien [Azure-tallennustilan replikoinnin](../storage/storage-redundancy.md) ja [SQL, tietokanta, aktiivinen Geo-replikoinnin](../sql-database/sql-database-geo-replication-overview.md) varmistaa, että sovelluksesi tietojen vaatimukset täyttyvät.

- **Varmista, että ei yksittäisen käyttäjätilin tuotannon ja varmuuskopiointi tietojen käytön.** Varmuuskopioiden tiedot ovat käsiin, jos yhden yksittäisen käyttäjätilin on oikeus kirjoittaa tuotannon ja varmuuskopion lähteistä. Joku tarkoituksellisesti sivun reunaan voinut poistaa kaikki tiedot, kun tavallinen käyttäjä vahingossa voinut poistaa sen. Suunnittele sovelluksesi käyttörajoituksia jokaiselle käyttäjätilille niin, että vain käyttäjät, jotka edellyttävät kirjoitusoikeudet kirjoitusoikeus ja se on vain joko tuotannon tai varmuuskopiointi, mutta ei molempia.

- **Tietolähteen päällä epäonnistua ja hylätty takaisin asiakirjan käsitellä, ja testaa se.** Jos tietolähde epäonnistuu catastrophically tapauksessa ihmisten operaattori on joukko epäonnistuu päälle uusi tietolähde kuvata ohjeita noudattamalla. Jos kuvata vaiheet on virheitä, operaattori ei voi onnistuneesti seurata niitä ja hylätty resurssin päälle. Testaa ohjepaketti vaihe vaiheelta, miten varmistaa, että seuraavien ne operaattori on onnistuneesti epäonnistua päälle ja epäonnistua takaisin tietolähteen säännöllisesti.

- **Vahvista tietojen varmuuskopiot.** Tarkista säännöllisesti varmuuskopioidut tiedot ovat vastaa odotuksiasi suorittamalla komentosarja Vahvista tietojen eheys, rakenne ja kyselyt. Ei ottaa varmuuskopion, jos se ei ole hyödyllisiä palauttaa tietolähteet. Kirjaudu sisään ja raportin ristiriidoista, jotta varmuuskopion palvelun voidaan korjata.

- **Harkitse tallennustilan tilin tyyppi, joka on geo tarpeettomat.** Tilin Azuren tallennustilaan tallennettuja tietoja aina replikoida paikallisesti. On kuitenkin useita replikoinnin strategioita valittavana tallennustilan tilin on valmisteltu. Valitse [Azure lukuoikeudet Geo tarpeettomat Storage (RA GRS)](../storage/storage-redundancy.md#read-access-geo-redundant-storage) suojaa sovelluksen tiedot harvinaisissa kirjainkokoa vastaan, kun koko aluetta ei ole käytettävissä.

    > [AZURE.NOTE] Saat VMs ei riippuvaisia RA GRS replikoinnin palauttaa AM levyjen (näennäiskiintolevytiedostoja). Käytä sen sijaan [Azure varmuuskopiointi][azure-backup].   

## <a name="operations"></a>Toiminnot

- **Ota käyttöön seuranta ja ilmoitat parhaat käytännöt-sovelluksessa.** Ilman ERISNIMI valvonta, diagnostiikka- ja ilmoitat tällä ei voi tunnistaa sovelluksen virheet ja ilmoita operaattori virheikkuna. Lisätietoja parhaista käytännöistä on lisätietoja [Seuranta- ja diagnostiikka ohjeet] [ monitoring-and-diagnostics-guidance] asiakirjan.

- **Mittaa remote puhelutiedot ja jakaa tietoja sovelluksen ryhmän.**  Jos ei seurata tai raportin remote puhelutiedot reaaliaikaisesti ja antaa helposti tarkastella näitä tietoja, toiminnot-ryhmä ei ole välittömästi näkymän, sovelluksen kunto kyselyjä. Ja jos voit vain mittayksikön remote puhelun keskimääräinen aika, sinun ei ole tarpeeksi tietoa, joka paljastaa ongelmat-palveluissa. Yhteenveto remote puhelun arvot, kuten viive, liikenteen ja virheiden ratkaiseminen 99 ja 95-arvona. Suorita tilastoanalyyseja paljastaa kunkin prosenttipiste sisällä tapahtuvien virheiden arvojen mukaisesti.

- **Seurata lyhytkestoisia poikkeukset ja uudelleenyritykset sopiva ajankohta.** Jos ei seuraa ja valvo lyhytkestoisia poikkeukset ja yritä uudelleen yritykset ajan kuluessa, on mahdollista, että seurantakohteen tai virhe voi olla piilotettu jollakin sovelluksesi uudelleen logiikan. Toisin sanoen jos että seuranta ja kirjaamisen vain näkyy onnistumisesta tai epäonnistumisesta toiminnon, toiminto oli yritettävä useita kertoja vuoksi poikkeukset kertoma on piilotettu. Trendin suurentamisesta poikkeukset ajan kuluessa ilmaisee, että palvelu on ottaa ongelma saattaa epäonnistua. Lisätietoja on artikkelissa [uudelleen palvelun tiettyjä ohjeita][retry-service-guidance].

- **Ota käyttöön nopean varoitus järjestelmän, joka varoittaa operaattori.** Määritä suorituskyvyn ilmaisimet sovelluksen terveyden, kuten lyhytkestoisia poikkeukset ja remote Soita viive ja määritä haluamasi raja-arvot kullekin niistä. Lähetä ilmoitus toimintoja, kun raja-arvo on saavutettu. Määritä nämä raja-arvot tasoilla, joilla voidaan tunnistaa ongelmat, ennen kuin ne tulevat kriittinen ja Vaadi palautus vastaus.

- **Sovelluksen dokumentointi versiossa.** Yksityiskohtainen release Prosessidokumentaatio ilman operaattori virheelliset päivityksen tai väärin sovelluksen asetusten määrittäminen. Selvästi määrittäminen ja asiakirjan release prosessin ja varmistaa, että se on käytettävissä koko operations-ryhmä. Joustavat käyttöönoton sovelluksesi parhaat käytännöt on kuvattu [joustavat käyttöönoton] [ guidance-resilient-deployment] Vikasietoisuudelle ohjeet asiakirjan osaan.

- **Varmista, että ryhmän usealle henkilölle koulutus sovelluksen ja kaikki manuaaliset palautus toimien.** Jos sinulla on vain yksi operaattori jäsenet, kuka voi seurata ja projektin palauttaminen vaiheet, hänestä tulee virhe yhden kohdan. Useiden henkilöiden tunnistus ja palautus kouluttaminen ja varmista, että tehtäviä on aina oltava vähintään yksi aktiivinen milloin tahansa.

- **Automatisoida sovelluksen käyttöönotto.** Jos toimintojen henkilökunnan on tehtävä manuaalisesti käyttöön sovelluksesi, ihmisten virhe voi aiheuttaa käyttöönoton epäonnistuu. Saat lisätietoja parhaat käytännöt automatisointi sovelluksen käyttöönottoa [joustavat käyttöönoton] [ guidance-resilient-deployment] Vikasietoisuudelle ohjeet asiakirjan osaan.

- **Suunnittele release prosessin Suurenna sovelluksen käytettävyyttä.** Jos release prosessin edellyttää services siirryt offline-tilaan käyttöönoton aikana, sovelluksesi ei ole käytettävissä, kunnes ne ovat peräisin online-tilaan. [Sininen ja vihreä](http://martinfowler.com/bliki/BlueGreenDeployment.html) tai [Kanarian Vapauta](http://martinfowler.com/bliki/CanaryRelease.html) käyttöönotto-tekniikka käyttöön tuotannon sovellusta. Toinen näistä menetelmistä liittyä käyttöönotto release-koodin rinnalla tuotannon koodi, niin käyttäjät release koodin voit ohjata tuotannon koodin, jos. Lisätietoja on artikkelissa [joustavat käyttöönoton] [ guidance-resilient-deployment] Vikasietoisuudelle ohjeet asiakirjan osaan.

- **Kirjaudu sisään ja että sovellusten valvonta.** Jos käytät vaiheistettu käyttöönoton tekniikoita, kuten sininen ja vihreä tai Kanarian versioiden ole käynnissä tuotannon sovelluksen-version. Jos ongelma pitäisi tehdä, on ehdottoman tärkeää selvittää sovelluksen version aiheuttavan ongelman. Ota käyttöön tehokkaat kirjaaminen strategia kannattaa tallentaa mahdollisimman paljon versio kielikohtaiset-tietoja. 

- **Varmista, että sovelluksesi ei voi käyttää työpöytäversioihin [Azure tilauksen rajoituksen](../azure-subscription-service-limits.md).** Azure tilaukset oltava rajoitukset resurssin tietyntyyppisten, kuten resurssiryhmien määrä, sydämiä määrä ja tallennustilaa tilien määrä.  Jos sovelluksen tarpeen ylittävät Azure tilauksen rajoitukset, luo toisen Azure tilauksen ja säännöstä riittävät resurssit siellä.

- **Varmista, että sovelluksesi ei voi käyttää työpöytäversioihin [palvelun - rajoituksen](../azure-subscription-service-limits.md).** Yksittäisten Azure services on kulutus rajoitukset &mdash; esimerkiksi rajoittaa tallennustilan, siirtonopeuden, useita yhteyksiä, pyynnöt sekunnissa ja muita tietoja. Sovelluksen epäonnistuu, jos se yrittää käyttää resurssit nämä raja-arvot. Tämä aiheuttaa palvelun rajoittava ja mahdolliset käyttökatkot tarvittavien käyttäjien. 

    Tietyn palvelun ja sovelluksen tarpeen mukaan voit usein välttää nämä raja-arvot mukaan Skaalaus (esimerkiksi valitsemalla toisen hinnoittelu taso) tai laajentaminen (lisääminen uusia esiintymiä).  

- **Suunnittele sovelluksen tallennuksen vaatimukset kirjoitettavat Azure tallennustilan skaalattavuus ja suorituskyvyn kohteet.** Azuren tallennustila on suunniteltu funktio sisällä ennalta määritettyjä skaalattavuus ja suorituskyvyn kohteisiin, suunnitella sovelluksen käyttämiseksi näiden kohteiden tallentamiseen. Jos ylität nämä kohteet sovelluksen kohdata tallennustilan rajoitusta. Voit korjata ongelman valmistella tallennustilan tilejä. Jos suoritat työpöytäversioihin tallennustilan tilin rajoitus, valmistella Azure lisäpalveluita ja valmistella tallennustilan tilejä siellä. Lisätietoja on artikkelissa [Azure-tallennustilan skaalattavuus ja suorituskyvyn kohteet](../storage/storage-scalability-targets.md).

- **Valitse sovelluksen oikeassa AM koko.** Mittaa todellinen suorittimen, muistin, levyn ja i/o, että VMs tuotannon ja varmista, että olet valinnut AM koko on riittävä. Jos ei sovelluksen saattaa ilmetä kapasiteetin ongelmia, kuten VMs esitellä rajoja. AM koot on kuvattu [koot näennäiskoneiden Azure](../virtual-machines/virtual-machines-windows-sizes.md) -asiakirja.

- **Onko sovelluksen työmäärää vakaata tai vaihtelevia ajan kuluessa.** Jos havainnollistamiseen vaihtelee jaettuina, käytä Azure AM asteikko määrittää automaattisesti näytöstä AM esiintymien määrän. Muussa tapauksessa sinun on manuaalisesti suurentaa tai pienentää VMs määrä. Lisätietoja on artikkelissa [Virtuaalikoneen asteikko joukot yleiskatsaus](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md).

- **Valitse oikea palvelun tason Azure SQL-tietokantaan.** Jos sovellus käyttää Azure SQL-tietokanta, varmista, että olet valinnut asianmukaiset palvelutaso. Jos valitset taso, jota ei voi käsitellä sovelluksen tietokannan tapahtuman yksikkö (DTU) vaatimukset, tietojen käyttöä on rajoitettu. Katso lisätietoja valitsemalla oikea palvelusopimus [SQL-tietokanta-asetukset ja suorituskyky: ymmärtää, mikä on käytettävissä palvelun kunkin tason](../sql-database/sql-database-service-tiers.md) asiakirjan. 

- **On peruutus palvelupaketti käyttöönottoa varten.** On mahdollista, että sovelluksen käyttöönotto voi epäonnistua ja aiheuttaa sovelluksesi eivät ole käytettävissä. Suunnittele palauttaminen prosessin minimoi käyttökatkot ja palata viimeisen tunnetut hyvä versioon. Katso [joustavat käyttöönoton] [ guidance-resilient-deployment] lisätietoja vikasietoisuudesta ohjeet asiakirjan osaan. 

- **Luoda käyttäminen Azure tuki.** Jos prosessi, jossa yhteydessä [Azure tuki](https://azure.microsoft.com/support/plans/) ei ole määritetty, ennen kuin ottaa yhteyden tukeen tarpeen vaatiessa käyttökatkot voidaan pidentää, kun tuki-prosessi on siirtynyt ensimmäisen kerran. Sisällytä yhteyttä tukipalveluun ja escalating ongelmat sovelluksen vikasietoisuudelle-sen osana.

- **Varmista, että sovelluksesi ei käytä yli tilauskohtaisten tilien tallennustilan enimmäismäärä.** Azure sallii enintään 200 tilauskohtaisten tallennustilan tunnuksilla. Jos sovellus edellyttää tallennustilan tilejä kuin on tällä hetkellä käytettävissä tilauksen, sinun on uusi tilaus ja tallennustilaa tilit luominen. Lisätietoja on artikkelissa [Azure tilaus ja palvelun rajoitukset, kiintiöiden ja rajoitukset](../azure-subscription-service-limits.md#storage-limits).

- **Varmista, että sovelluksesi ei ole suurempi kuin levyjen virtuaalikoneen skaalattavuus kohteet.** Azure-IaaS AM tukee liittäminen määrä tietojen levyjen useista tekijöistä, kuten AM kokoa ja tallennustilaa tilin tyypin mukaan. Jos sovelluksen ylittää levyjen virtuaalikoneen skaalattavuus kohteet, valmistella lisätallennustilaa tilit ja luo virtuaalikoneen levyjen siellä. Lisätietoja on artikkelissa [Azure-tallennustilan skaalattavuus ja suorituskyvyn kohteet] (... / storage/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks)

## <a name="test"></a>Testi

- **Suorita automaattisesti ja sovelluksen testaus tuntisesta.** Jos et ole täysin testattu automaattisesti ja tuntisesta, et voi olla varma, että sovelluksesi riippuvaisia palveluita tulee takaisin synkronoidun pikaviestikeskustelun aikana palauttaminen. Varmista, että sovelluksesi riippuvainen palveluiden automaattisesti ja virheiden takaisin oikeassa järjestyksessä.

- **Suorita vika lisäämisen testaaminen sovelluksen.** Sovelluksen voi epäonnistua monesta eri syystä vanhentumisen, kuten ennenaikaisesti järjestelmäresursseja AM tai tallennustilan virheet. Testaa sovelluksesi mahdollisimman sekä lähellä simuloimalla tai käynnistävä reaali virheet-ympäristössä. Esimerkiksi poistaa sertifikaatit keinotekoisesti tarjoaman järjestelmäresursseja, poistaminen ja tallennustilaa lähde. Tarkista sovelluksen mahdollisuus kaikenlaisia virheet, yksin ja yhdessä palauttaminen. Tarkista, että virheet eivät välittäminen tai CSS-järjestelmän kautta.

- **Tuotannon sekä synteettistä ja todelliset käyttäjätiedot testien suorittaminen** Numero- ja samanlaiset harvoin, joten on tärkeää sininen ja vihreä tai Kanarian käyttöönotto ja esikatsella sovelluksen tuotannon. Näin voit esikatsella sovelluksen tuotannon reaali kuormitus ja varmistaa, että se toimii odotetulla tavalla, kun täysin käyttöön.

## <a name="security"></a>Tietoturva

- **Ota käyttöön sovellustason suojautumista distributed eston riskiä (WWW.microsoft.com-sivustoa kohtaan).** Azure palvelut suojataan WWW.microsoft.com-sivustoa kohtaan kalastelu verkon tasolla. Kuitenkin Azure ei voi suojata sovelluskerroksen kalastelu vastaan, koska se on vaikea erottaa tosi käyttäjäpyynnöt haittaohjelmien pyynnöt. Lisätietoja siitä, miten voit suojautua sovelluskerroksen WWW.microsoft.com-sivustoa kohtaan kalastelu on [Microsoft Azure verkon Security](http://download.microsoft.com/download/C/A/3/CA3FC5C0-ECE0-4F87-BF4B-D74064A00846/AzureNetworkSecurity_v3_Feb2015.pdf) (Lataa PDF) kohdassa "Suojaaminen vastaan WWW.microsoft.com-sivustoa kohtaan".

- **Ota käyttöön käyttöoikeuksien myöntäminen tarpeen mukaan käyttää sovelluksen resursseja.** Access sovelluksen resurssit oletusarvo olisi mahdollisimman. Hyväksynnän välein suurempi tason käyttöoikeuksien myöntäminen Liian makrosuojausasetusten sovelluksen resurssien käyttöoikeuksien myöntäminen oletusarvoisesti voi aiheuttaa joku tarkoituksellisesti sivun reunaan tai vahingossa poistaminen resurssit. Azure avulla voit hallita käyttöoikeuksia [Roolipohjainen käyttöoikeuksien valvonta](../active-directory/role-based-access-built-in-roles.md) , mutta se on tärkeää varmistaa vähintään oikeus käyttöoikeuksien muita resursseja, joiden oman käyttöoikeudet järjestelmien, kuten SQL Server. 

## <a name="telemetry"></a>Telemetriatietojen

- **Kirjaudu telemetriatietoja, kun sovellus on käynnissä tuotantoympäristössä.** Tehokkaat telemetriatietojen tietojen kaappaaminen, kun sovellus on käynnissä tuotantoympäristössä tai sinulla ei ole riittävästi tietoja syy ongelmien vianmääritys samalla, kun se on aktiivisesti tarjota käyttäjille. Lisätietoja on käytettävissä lokiin kirjaamisen parhaista käytännöistä on käytettävissä [Seuranta- ja diagnostiikka ohjeet] [ monitoring-and-diagnostics-guidance] asiakirjan.

- **Ota käyttöön asynkroninen kaavan kirjaaminen.** Jos kirjaaminen toiminnot on synkronoitu, ne voi estää sovelluksen-koodi. Varmistaakseen, että kirjaaminen toimintojen noudatetaan asynkroninen toimintoina. 

- **Yhdistää tietoja palvelun rajojen.** Tyypillinen n-taso-sovelluksessa pyytäminen voi siirtää useita palvelun rajat. Esimerkiksi pyytäminen yleensä on peräisin web-tason on välitetty business taso ja lopuksi samanlainen tietojen taso. Monimutkaisempia tilanteissa pyytäminen voidaan jakaa useita eri Servicesin ja stores. Varmista, että kirjaaminen järjestelmän numeroidulla puhelut palvelun rajojen, jotta voit seurata pyynnön koko sovelluksen.

##  <a name="azure-resources"></a>Azure-resurssit 

- **Azure Resurssienhallinta mallia, Varaa resurssit.** Resurssienhallinta mallit helpottavat automatisoida ominaisuuksissa PowerShell tai Azure-CLI, joka johtaa luotettavaa käyttöönottoprosessin kautta. Lisätietoja on artikkelissa [Azure Resurssienhallinta yleiskatsaus][resource-manager].

- **Anna resurssien kuvaavat nimet.** Anna resurssien kuvaavat nimet helpottaa sen roolin osat ja etsiä tietyn resurssin. Lisätietoja on artikkelissa [nimeämiskäytännöt Azure resurssien suositus](guidance-naming-conventions.md) 

- **Käytä Roolipohjainen käyttöoikeuksien valvonta (RBAC)**. RBAC avulla voit hallita Azure resurssien avulla voit ottaa käyttöön. RBAC avulla voit määrittää luvan roolit DevOps työryhmän jäsenille ja estää vahingossa tai muutokset käyttöön resursseja. Lisätietoja on artikkelissa [Azure-portaalissa käyttöoikeuksien hallinnan käytön aloittaminen](../active-directory/role-based-access-control-what-is.md) 

- **Käytä resurssien lukitukset kriittinen resurssit, kuten VMs.** Resurssien lukitukset poistetaan vahingossa resurssin estää operaattori. Lisätietoja [lukituksen resurssien Azure resurssien hallinta](../resource-group-lock-resources.md) 

- **Alueellisten parit.** Kun otat käyttöön kahta aluetta, valitse alueiden saman aluekohtaiset pari. Jos laajan käyttökatkosta palautus yhden alueen ensin ulos jokainen. Joidenkin palvelujen esimerkiksi Geo tarpeettomat lisätallennustilaa on automaattinen replikoinnin pisteparin alueen. Lisätietoja on artikkelissa [liiketoiminnan jatkuvuuden ja tietojen palauttaminen (BCDR): Azure Parittainen alueiden](../best-practices-availability-paired-regions.md) 

- **Järjestä resurssiryhmät elinkaari käyttötarkoituksen mukaan**.  Yleensä resurssiryhmä pitäisi näkyä resursseja, joilla on sama elinkaari. Tämä on helppo hallita käyttöönotoissa, poista testi ominaisuuksissa ja määrittää käyttöoikeuksia, vähentää mahdollisuutta, että tuotannon käyttöönoton poistetaan vahingossa tai muokata. Luo erillisen resurssiryhmiä tuotannon, kehitystä, ja testaa ympäristössä. Sijoittaa monille-käyttöönoton resurssien kunkin alueen erillisessä resurssin ryhmiin. Tämä on helppo käyttöön jonkin alueen vaikuttamatta muihin voimassa. 

## <a name="azure-services"></a>Azure-palvelut 

Tarkistusluettelon seuraavat koskevat tiettyjen palveluiden Azure-tietokannassa.

###  <a name="app-service"></a>Sovelluksen-palvelu 

- **Käytä vakio- tai Premium taso.** Nämä tasoa tuki väliaikaisen paikkojen ja automaattisen varmuuskopioinnin. Lisätietoja on artikkelissa [Azure App palvelun suunnitelmien perusteellisempaa yleiskatsaus](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) 

- **Vältä skaalaus ylös tai alas.** Sen sijaan Valitse taso ja esiintymän koon, jotka täyttävät suorituskyvyn tarpeen tyypillinen kuormituksen ja [Mittakaava,](../app-service-web/web-sites-scale.md) esiintymät käsitellään liikenteen äänenvoimakkuuden muutoksia. Skaalaus ylös ja alas voi käynnistää sovellus uudelleen.  

- **Tallentaa sovelluksen asetusten määritykset.** Sovelluksen asetukset määritysasetukset sovelluksen asetusten avulla. Määritä asetukset Resurssienhallinta-mallien tai PowerShellin, jotta voit käyttää niitä osana automaattisen käyttöönoton / Päivitä prosessi, joka on luotettava. Lisätietoja on artikkelissa [Configure web apps Azure sovelluksen-palvelussa](../app-service-web/web-sites-configure.md). 

- **Luo erillisen sovelluksen palvelusopimusten vaihtoehdot tuotannon ja testi.** Älä käytä paikkojen tuotannon ympäristöön testikäyttöön.  Kaikki sovellukset sisällä saman sovelluksen palvelusopimus jakaa saman AM esiintymät. Jos lisäät tuotannon ja testaa ominaisuuksissa vastaavan palvelupaketin, se heikentää tuotannon käyttöönotto. Lataa testien voi esimerkiksi heikentää live tuotantolaitoksen. Sijoittamalla testi ominaisuuksissa tuominen eri suunnitelma voit erottaa ne tuotannon-versiosta.  

- **Verkko-ohjelmointirajapinnan erillisessä verkkosovelluksissa**. Jos ratkaisu on verkkopohjainen edusta- ja verkko-Ohjelmointirajapinnan, harkitse decomposing ne yhdeksi erillisen palvelun sovelluksen sovellukset. Tämä on helppo Hajota ratkaisun kuormituksen mukaan. Voit suorittaa web app- ja Ohjelmointirajapinnan erillisen sovelluksen palvelusopimusten vaihtoehdot, jotta niitä voi skaalata itsenäisesti. Jos et tarvitse kyseisiä skaalattavuus ensimmäisen kerran, voit asentaa sovellukset vastaavan palvelupaketin, ja siirrä ne erillisessä suunnitelmien myöhemmin tarvittaessa.

- **Vältä Azure SQL-tietokantoja varmuuskopiointi App palvelun backup-toiminnon avulla.** Käytä sen sijaan [SQL-tietokantaan automaattisen varmuuskopioinnin][sql-backup]. Sovelluksen palvelun varmuuskopiointi Vie tietokantaan SQL .bacpac tiedostoon, joka DTUs kustannukset.  

- **Ota käyttöön väliaikaisen paikka.** Luo väliaikaisen käyttöönoton paikka. Sovellusten päivitykset käyttöön väliaikaisen paikka ja varmista ennen vaihtaminen kyselyjä tuotannon käyttöönotto. Tämä vähentää tuotannon virheelliset päivityksen. Se myös varmistaa, että kaikki esiintymät on lämmitettävä ennen parhaillaan vaihtaa paikkaa kyselyjä tuotannon. Useita sovelluksia on merkittäviä warmup ja kiinnostunut aloitusaika. Lisätietoja on artikkelissa [väliaikaisen ympäristöissä Azure App palvelun web-sovellusten määrittäminen](../app-service-web/web-sites-staged-publishing.md). 

-  **Luo käyttöönoton paikka pitoon viimeksi toiminut (LKG) käyttöönotto.** Kun otat käyttöön päivitys tuotannon, siirtäminen LKG paikka edellisen tuotannon käyttöönotto. Tämä on helppo aikaisempi virheelliset käyttöönotto. Jos huomaat ongelmia myöhemmin, voit nopeasti palauttaa LKG-versioon. Lisätietoja on artikkelissa [Azure viittaus arkkitehtuuri: Basic web-sovelluksen](guidance-web-apps-basic.md). 

- **Diagnostiikan kirjaus käyttöön**, mukaan lukien sovelluksen kirjaaminen ja WWW-palvelimen kirjaaminen. Lokiin kirjaaminen on tärkeää seurantaa ja vianmääritys. Katso [Ota lokiin kirjaaminen web Apps-sovellukset Azure App palvelun diagnostiikka](../app-service-web/web-sites-enable-diagnostic-log.md)

- **Kirjaudu blob storage**. Tämä on helppo kerätä ja analysoida tietoja. 

- **Luoda erilliset tallennustilan tilin lokitiedot.** Älä käytä samaa tallennustilan tiliä lokit ja sovelluksen tiedot. Tämä auttaa estämään kirjaaminen vähentäminen sovelluksen suorituskykyyn. 

- **Valvoa suorituskykyä.** Käytä suorituskyvyn palvelun esimerkiksi [Uuden Relic](http://newrelic.com/) tai [Hakemuksen tiedot](../application-insights/app-insights-overview.md) sovelluksen suorituskyvyn seuranta ja valitse kuormituksen toimintaa.  Suorituskyvyn seurantaa antaa reaaliaikaisia tietoja sovellukseen. Voit vianmääritys ja syiden analysoinnissa virheitä. 


###  <a name="application-gateway"></a>Sovelluksen yhdyskäytävän 

- **Valmistele vähintään kaksi esiintymät.** Ottaa käyttöön sovelluksen yhdyskäytävän vähintään kaksi esiintymät. Yksittäisen esiintymän on yksittäinen virheestä. Käytä vähintään kaksi esiintymät redundanssin ja skaalattavuus. Saadakseen [SLA](https://azure.microsoft.com/support/legal/sla/application-gateway/v1_0/)on valmisteltava kahden tai useamman Normaali tai suurempi esiintymät. 

### <a name="azure-search"></a>Azure haku

- **Valmistele useita replikan.** Käytä vähintään kaksi replikoiden luku suuren käytettävyyden tai luku-ja kirjoitusoikeudet suuren käytettävyyden kolmen.

- **Määritä Indeksoijilla monille käyttöönotoissa.** Jos sinulla on monille käyttöönottoa, ota huomioon jatkuvuus indeksoinnin asetusten.

    - Jos tietolähde on geo replikoida, osoita sen paikallisten tietojen lähteen replikan kunkin indeksointitoiminto kunkin alueellisen Azure Search-palvelun.  

    - Jos tietolähteen ei ole geo replikoida, osoita useita Indeksoijilla saman tietolähteen niin, että Azure Etsi palveluita useita alueilla jatkuvasti ja itsenäisesti indeksoida tietolähteestä. Lisätietoja on artikkelissa [Azure haun suorituskyky ja optimointi huomioitavista][search-optimization].

###  <a name="azure-storage"></a>Azure-tallennustilan 

- **Käytä sovelluksen tietojen lukuoikeus geo tarpeettomat storage (RA GRS).** RA GRS tallennustilan kopioi toissijainen alueen tiedot ja vain luku-pääsee toissijainen alueelta. Jos ensisijainen alueella on tallennustilan käyttökatkosta, sovellus voi lukea tiedot toissijainen alueelta. Lisätietoja on artikkelissa [Azure-tallennustilan replikoinnin](../storage/storage-redundancy.md).

- **Saat AM levyjä, käytä Premium tallennustilan** Lisätietoja on artikkelissa [Premium tallennustilan: tehokas tallennustila Azure virtuaalikoneen työmääriä](../storage/storage-premium-storage.md).

- **Luo varmuuskopio jonon jonon tallennustilan toisen alueen.** Jonon tallennuksessa vain luku-replika on rajoitettu käyttö, koska ei jono tai jonosta kohteiden poistamisen. Luo varmuuskopio jonon sijaan tallennustilan tilillä toisen alueen. Jos näkyvissä on tallennustilan käyttökatkosta, sovellus käyttää varmuuskopion jonossa kunnes ensisijainen alue on käytettävissä uudelleen. Sen mukaan, sovellus voi silti käsitellä uusia pyyntöjä.  

###  <a name="documentdb"></a>DocumentDB 

- **Tietokannan replikoiminen eri alueilla.** Monille tilillä DocumentDB tietokannassa on yksi kirjoittaminen alue- ja lue alueille. Jos kirjoitus-alueella on ilmennyt virhe, voit lukea toisen replikasta. Asiakkaan SDK käsittelee tämän automaattisesti. Voit myös epäonnistua toisen alueen kirjoittaminen alueen yli. Lisätietoja on kohdassa [Jaa tiedot yleisesti DocumentDB kanssa](../documentdb/documentdb-distribute-data-globally.md).


###  <a name="sql-database"></a>SQL-tietokantaan 

- **Käytä vakio- tai Premium taso.** Nämä tasoa on enää ajankohta palauttaminen piste (35 päivää). Lisätietoja on artikkelissa [SQL-tietokanta-asetukset ja suorituskykyä](../sql-database/sql-database-service-tiers.md).

- **SQL-tietokantaan valvoa.** Valvonta voidaan vianmääritys haittaohjelmien kalastelu tai ihmisten virhe. Lisätietoja on artikkelissa [valvonta SQL-tietokannan käytön aloittaminen](../sql-database/sql-database-auditing-get-started.md). 

- **Käytä aktiivinen Geo sallittuja** Aktiivinen Geo-replikoinnin avulla voit luoda luettavissa toissijainen toisella alueella.  Jos ensisijainen tietokannan epäonnistuu, tai vain on otettava offline-tilassa, suorittaa siirtyy toissijaisen tietokannan manuaalinen automaattisesti.  Epäonnistuu päällä, kunnes toissijaisen tietokanta pysyy vain luku-tilassa.  Lisätietoja on artikkelissa [SQL, tietokanta, aktiivinen Geo-replikoinnin](../sql-database/sql-database-geo-replication-overview.md). 

- **Käytä sharding**. Harkita sharding osion tietokannan vaakasuunnassa. Sharding tarjota vika yksinään. Lisätietoja on artikkelissa [Azure SQL-tietokantaan, skaalaus](../sql-database/sql-database-elastic-scale-introduction.md). 

- **Palauttaa ihmisten virhesanomasta ajankohta palauttaminen avulla.**  Ajankohta palauttaminen palauttaa tietokannan aikaisempaan aika. Lisätietoja on artikkelissa [palauttaa Azure SQL-tietokantaan, käyttämällä automaattista tietokannan varmuuskopioita][sql-restore].

- **Palvelun käyttökatkosta palauttaminen geo palauttaminen avulla.** GEO palauttaminen palauttaa tietokannan geo tarpeettomat varmuuskopiosta.  Lisätietoja on artikkelissa [palauttaa Azure SQL-tietokantaan, käyttämällä automaattista tietokannan varmuuskopioita][sql-restore].


###  <a name="sql-server-running-in-a-vm"></a>SQL Server (käynnissä AM)

- **Tietokannan replikoiminen.** SQL Server aina käyttöön käytettävyys-ryhmien avulla voit toistaa tietokannan. On suuri käytettävyys, jos yksi SQL Server-esiintymän epäonnistuu. Jos haluat lisätietoja, lue [Lisätietoja...](guidance-compute-n-tier-vm.md) 

- **Varmuuskopioi tietokanta**. Jos käytät jo [Azure varmuuskopio](https://azure.microsoft.com/documentation/services/backup/) varmuuskopioida oman VMs, kannattaa käyttää [Azure varmuuskopiointi käyttämällä DPM SQL Server-toiminnoista](../backup/backup-azure-backup-sql.md). Tämä vaihtoehto, jossa on yksi varajärjestelmänvalvoja rooli organisaatiossa ja yhdistetty palautus ohjeet VMs ja SQL Server. Muussa tapauksessa käytä [SQL Server hallitun varmuuskopio Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx). 


###  <a name="traffic-manager"></a>Liikenteen hallinta 

- **Suorita manuaalinen tuntisesta.** Suorita jälkeen liikenteen hallinta-automaattisesti manuaalinen tuntisesta sijaan kaatuvat automaattisesti takaisin. Ennen kaatuvat takaisin, varmista, että kaikki sovelluksen alijärjestelmien kunnossa.  Muussa tapauksessa voit luoda tilanteeseen, jossa sovellus kääntää edestakaisin välillä tietojen keskikohdan mukaan. Lisätietoja on artikkelissa [Käynnissä VMs useita alueilla](guidance-compute-multiple-datacenters.md).

- **Luo kunto-näytteenottimen päätepiste**. Luo mukautettu päätepiste, sovelluksen yleinen kunto raportoi. Näin voi epäonnistua päälle, jos minkä tahansa kriittisen polun epäonnistuu, ei pelkästään edusta liikenteen hallinta. Päätepisteen tulisi palauttaa HTTP-virhekoodi, jos kaikki tärkeät riippuvuus on perusasemassa tai saavuttamattomissa. Älä raportoi virheet vähäinen palveluiden kuitenkin. Muussa tapauksessa kunto näytteenottimen saattaa aiheuttaa automaattisesti, kun sitä ei tarvita, tunnistettujen luominen. Lisätietoja on artikkelissa [liikenteen hallinta päätepisteen seuranta- ja automaattisesti](../traffic-manager/traffic-manager-monitoring.md).


###  <a name="virtual-machines"></a>Näennäiskoneiden 

- **Vältä tuotannon työmäärää käytössä yksittäisen AM.** Yksittäisen AM käyttöönoton ei ole joustavat suunniteltu tai suunnittelematon ylläpito. Sijoita sen sijaan useita VMs käytettävyyden määrittäminen tai AM asteikko joukon, jossa kuormituksen etualalle. Saadakseen SLA vähintään kaksi näennäiskoneiden on otettava käyttöön käytettävyys samat kuluessa. Lisätietoja on artikkelissa [Virtuaalikoneen asteikko joukot yleiskatsaus](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md). 

- **Määritä käytettävyys, kun valmistelu AM.** Tällä hetkellä tällä ei voi lisätä Resurssienhallinta AM jälkeen AM on valmisteltu saatavuus. Kun lisäät uuden AM aiemmin käyttöön määrittäminen, varmista, että voit luoda NIC AM ja lisää Verkkokortti kuormituksen taustatietokantaan osoiteryhmän. Muussa tapauksessa kuormituksen ei reitittää verkkoliikenteen, AM. 

- **Sijoittaa sovelluksen kunkin tason erillisessä käytettävyys määrittäminen.** N-taso-sovelluksessa ei aseta viedyt VMs eri tasojen käytettävyys samat. Käytettävyys määrittäminen VMs sijoitetaan vika toimialueilla (FDs) ja Päivitä domains (UD). Kuitenkin saat redundancy hyötyä FDs ja UDs jokaisen AM käytettävyys määrittäminen on voitava muodostaa saman asiakkaan pyyntöjen. 

- **Valitse oikea AM koko suorituskyvyn tarpeiden perusteella.** Kun aiemmista työtehtävistäsi toiselle työntekijälle Siirry Azure ja aloita parhaiten vastaava paikallisen palvelimiin AM haluamasi kokoinen. Valitse todellinen havainnollistamiseen, jotka koskevat suorittimen ja muistin levyn IOPS suorituskykyä ja koon säätäminen tarvittaessa. Tämä auttaa varmistamaan, sovellus toimii odotetulla tavalla cloud-ympäristössä. Myös, jos tarvitset useita NIC, on otettava huomioon jokaisen koon NIC rajoitus. 

- **Käytä näennäiskiintolevyjen premium-tallennustilan.** Azure Premium tallennustilan tehokas, pieni viive levyn tukee. Lisätietoja on artikkelissa [Premium tallennustilan: tehokas tallennustila Azure virtuaalikoneen työmääriä](../storage/storage-premium-storage.md) Valitse AM koko, joka tukee premium tallentamista. 

- **Luoda erilliset tallennustilan tilin kunkin AM.** Lisää näennäiskiintolevyt yhden AM erillisessä tallennustilan-tilille. Näin voit välttää pallolla tallennustilan tilit IOPS rajoitukset. Lisätietoja on artikkelissa [Azure-tallennustilan skaalattavuus ja suorituskyvyn kohteet](../storage/storage-scalability-targets.md). Jos otat VMs suuri määrä, kuitenkin tietoinen tallennustilan tilit tilausta kohti rajoitus. Katso [tallennustilarajojen](../azure-subscription-service-limits.md#storage-limits).

- **Luo erillisen tallennustilan tili vianmäärityslokit varten**. Älä kirjoita vianmäärityslokit saman tallennustilan tilille, kuin näennäiskiintolevyjen Vältä Diagnostiikan kirjaus vaikuttaa AM-levyjen IOPS. Normaali paikallisesti tarpeettomat storage (LRS)-tili on riittävä vianmäärityslokit. 

- **Sovellusten asentaminen tietojen DVD-levyllä, ei OS levy.** Muussa tapauksessa saattaa saavuttaa levyn kokorajoituksen. 

- **Azure varmuuskopioinnin avulla voit varmuuskopioida VMs.** Varmuuskopioiden suojaavat vahingossa tietojen menettämisen. Lisätietoja on kohdassa [Suojaa Azure VMs palautus-palveluiden säilö kanssa](../backup/backup-azure-vms-first-look-arm.md).

- **Ota käyttöön vianmäärityslokit**, mukaan lukien basic kunto mittarit, infrastruktuuri lokit ja [käynnistyksen diagnostiikka][boot-diagnostics]. Käynnistyksen diagnostiikka auttavat vianmäärityksen käynnistys epäonnistui, jos oman AM saa käynnistys tilaan. Lisätietoja on artikkelissa [Yleistä, Azure vianmäärityslokit][diagnostics-logs].

- **Käytä AzureLogCollector-tunniste**. (Windows VMs ainoastaan.) Tämän tunnisteen koostaa Azure ympäristö lokit ja lataa ne Azure tallennustilan ilman etäyhteyden kirjautumalla järjestelmään AM operaattori. Lisätietoja on artikkelissa [AzureLogCollector tunniste](../virtual-machines/virtual-machines-windows-log-collector-extension.md).


###  <a name="virtual-network"></a>VPN 

- **Julkisten IP-osoitteiden lisääminen NSG whitelist tai estä aliverkon.** Haittaohjelmien käyttäjiltä käytön estäminen tai salliminen access vain käyttäjät, joilla on oikeus käyttää sovellusta.  

- **Luo mukautettu kunto näytteenottimen.** Kuormituksen tasauspalvelun kunto Probes testata HTTP- tai TCP. AM käytetään HTTP-palvelin, HTTP Keräysputken on paremmin ilmaisin kuin TCP-näytteenottimen terveyden tila. HTTP-näytteenotin Käytä mukautettuja päätepiste, joka ilmoittaa sovelluksen, mukaan lukien kaikki tärkeät riippuvuudet yleinen kunto. Lisätietoja on artikkelissa [Azure kuormituksen yleiskatsaus](../load-balancer/load-balancer-overview.md).

- **Kuntotietojen näytteenottimen ei estä.** Kuormituksen tasauspalvelun kunto näytteenottimen lähetetään tunnetut IP-osoite, 168.63.129.16. Ei estä liikenne tai pois IP palomuurin käytännöt tai verkon suojaussäännöt ryhmän (NSG). Estäminen kunto näytteenottimen aiheuttaa kuormituksen AM poistaminen kierto. 

- **Ota käyttöön kuormituksen kirjaaminen.** Lokit näyttäminen montako VMs sitten taustatietokantatiedosto ei saa verkkoliikennettä vuoksi epäonnistui näytteenottimen vastaukset. Lisätietoja on artikkelissa [Azure kuormituksen analysointitietoja loki](../load-balancer/load-balancer-monitor-log.md).


<!-- links -->
[app-service-autoscale]: ../monitoring-and-diagnostics/insights-how-to-scale.md
[asynchronous-c-sharp]:https://msdn.microsoft.com/library/mt674882.aspx
[availability-sets]:../virtual-machines/virtual-machines-windows-manage-availability.md
[azure-backup]: https://azure.microsoft.com/documentation/services/backup/
[boot-diagnostics]: https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/
[circuit-breaker]: https://msdn.microsoft.com/library/dn589784.aspx
[cloud-service-autoscale]: ../cloud-services/cloud-services-how-to-scale.md
[diagnostics-logs]: ../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md
[fma]: guidance-resiliency-failure-mode-analysis.md
[guidance-resilient-deployment]: guidance-resiliency-overview.md#resilient-deployment
[load-balancer]: load-balancer/load-balancer-overview.md
[monitoring-and-diagnostics-guidance]: ../best-practices-monitoring.md
[resource-manager]: ../azure-resource-manager/resource-group-overview.md
[retry-pattern]: https://msdn.microsoft.com/library/dn589788.aspx
[retry-service-guidance]: ../best-practices-retry-service-specific.md
[search-optimization]: ../search/search-performance-optimization.md
[sql-backup]: ../sql-database/sql-database-automated-backups.md
[sql-restore]: ../sql-database/sql-database-recovery-using-backups.md
[traffic-manager]: ../traffic-manager/traffic-manager-overview.md
[traffic-manager-routing]: ../traffic-manager/traffic-manager-routing-methods.md
[vmss-autoscale]: ../virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-overview.md

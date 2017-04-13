<properties
   pageTitle="Cloud tietojen palauttaminen solutions - SQL, tietokanta, aktiivinen Geo-replikoinnin | Microsoft Azure"
   description="Opettele suunnitella cloud tietojen palauttaminen ratkaisuja liiketoiminnan jatkuvuuden suunnittelu geo replikoinnin käyttäminen sovelluksen tietojen varmuuskopiointi Azure SQL-tietokantaan."
   keywords="cloud palauttaminen, tietojen palauttaminen ratkaisuja, sovelluksen tietojen varmuuskopiointi geo-replikoinnin liiketoiminnan jatkuvuuden suunnittelu"
   services="sql-database"
   documentationCenter=""
   authors="anosov1960"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management"
   ms.date="07/20/2016"
   ms.author="sashan"/>

# <a name="design-an-application-for-cloud-disaster-recovery-using-active-geo-replication-in-sql-database"></a>Suunnittele sovelluksen cloud palauttaminen käyttämällä Active Geo-replikoinnin SQL-tietokantaan


> [AZURE.NOTE] Kaikki tasoa kaikki tietokannat on nyt [Aktiivinen Geo-replikoinnin](sql-database-geo-replication-overview.md) .



Opettele käyttämään [Active Geo-replikoinnin](sql-database-geo-replication-overview.md) SQL-tietokannan rakenteen tietokantasovelluksia joustavat aluekohtaiset virheet ja kriittinen katkokset. Liiketoiminnan jatkuvuus suunnittelu, harkitse sovelluksen käyttöönotto topologian palvelun ohjeasiakirjoissa, kohdennat, liikenne viive ja kustannukset. Tässä artikkelissa on yleisiä sovelluksen kuviot katsomalla ja keskustella edut ja kunkin asetuksen valinnat. Tietoja Active Geo-replikointi joustavasti jakavat lisätietoja [joustavasti resurssivarantoon varalle](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).

## <a name="design-pattern-1-active-passive-deployment-for-cloud-disaster-recovery-with-a-co-located-database"></a>Rakenne-kaava 1: Aktiivinen-passiivinen käyttöönoton cloud palauttaminen yhtä sijaitsevat tietokannan kanssa

Tämä asetus on parhaiten sovellusten seuraavat ominaisuudet:

+ Yhden alueen Azure Active esiintymä
+ Tietojen käyttäminen luku-kirjoitus (RW) vahva riippuvuus
+ Sovelluksen logiikkaa ja tietokannan välinen yhteys rajat-alueen ei hyväksytä vuoksi viive ja liikenteen kustannukset    

Tässä tapauksessa sovelluksen käyttöönotto topologian on tarkoitettu käsittely alueellisen lieventämiseksi, kun kaikki sovelluksen osat ovat vaikuttaneet ja sinun pitää yksikkönä automaattisesti. Sovelluksen logiikkaa ja tietokannan, replikoida toisen alueen maantieteelliset redundancy, mutta niitä ei käytetä sovelluksen kuormituksen Normaali olosuhteissa. Toissijainen alueen sovellus on määritettävä käyttämään SQL yhteysmerkkijonon toissijainen-tietokantaan. Liikenteen hallinta määritetään [automaattisesti reititys](../traffic-manager/traffic-manager-configure-failover-routing-method.md)-menetelmää.  

> [AZURE.NOTE] [Azure liikenteen hallinta](../traffic-manager/traffic-manager-overview.md) käytetään ainoastaan kuvassa varten tämän artikkelin koko. Voit käyttää minkä tahansa kuormituksen tasaamisen ratkaisu, joka tukee automaattisesti reititys-menetelmää.    

Lisäksi tärkeimmät sovelluksen esiintymät Ota huomioon, valvoo ensisijaisen tietokannan lähettämällä säännöllisiä T-SQL vain luku-tilassa (RO) komennot pieni [työntekijän rooli sovelluksen](cloud-services-choose-me.md#tellmecs) käyttöönotto. Voit käyttää sitä automaattisesti käynnistettävän automaattisesti, voit luoda ilmoituksen sovelluksen järjestelmänvalvojan konsolissa tai molempia. Voit varmistaa, että seuranta ei vaikuttaa alueen laajuinen katkokset, olisi seurantaa Palvelusovellusten esiintymät käyttöön kunkin alueen ja jokaiselle esiintymälle yhteydessä ensisijaisen tietokannan ensisijainen alueen ja paikallisen alueen toissijaisen tietokanta on. 

> [AZURE.NOTE] Sekä sovellusten valvonta on aktiivinen ja tutkia ensisijainen ja toissijainen tietokannat. Jälkimmäinen voidaan tunnistaa epäonnistui toissijainen alue-ja ilmoitus, kun sovellusta ei ole suojattu.     

Seuraavassa kaaviossa näkyvät tässä määrityksessä käyttökatkosta ennen.

![SQL-tietokannan Geo-replikointi määritys. Cloud palauttaminen.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern1-1.png)

Ensisijainen alueen käyttökatkosta jälkeen seurantaa sovellus tunnistaa ensisijainen tietokanta ei ole käytettävissä ja rekisteröi ilmoituksen. Sovelluksen-SLA, riippuen voit päättää, kuinka monta peräkkäistä seurantaa keräysputkien epäonnistua, ennen kuin se ilmoittaa tietokannan käyttökatkosta. Tavoitteet koordinoidun automaattisesti sovelluksen läpikuultavuutta ja tietokanta on oltava seurantaa sovelluksen toimimalla seuraavasti:

1. Jos haluat käynnistää läpikuultavuutta automaattisesti [ensisijainen läpikuultavuutta tilan päivittäminen](https://msdn.microsoft.com/library/hh758250.aspx) .
2. Soita toissijaisen tietokanta aloittaa [tietokannan automaattisesti](sql-database-geo-replication-portal.md).

Automaattisesti, kun sovellus käsittelee käyttäjien pyyntöjen toissijainen alueen, mutta ne ovat yhteyteen tietokannan, koska ensisijaisen tietokannan nyt on toissijainen alueen. Tässä skenaariossa on kuvattu seuraavan kaavion avulla. Kaikissa kaavioissa yhtenäiset viivat tarkoittavat yhteyksiä, pisteviivoja osoittaa keskeytetty yhteydet ja Lopeta merkit tarkoittavat toiminnon Käynnistimet.


![GEO-replikointi: Siirtyy toissijaisen tietokanta automaattisesti. Sovelluksen tietojen varmuuskopiointi.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern1-2.png)

Jos käyttökatkosta tapahtuu toissijainen alueen, replikoinnin linkityksen ensisijaisen ja toissijaisen tietokanta on hyllytetty ja seurantaa sovelluksen Rekisteröi ilmoituksen, että ensisijainen tietokanta on määritetty. Sovelluksen suorituskyvyn ei vaikuttaa, mutta sovellus toimii näkyviä ja uudempi vastuullasi tapauksessa molemmat alueet epäonnistuu peräkkäin.

> [AZURE.NOTE] On suositeltavaa vain käyttöönoton käyttömahdollisuudet yksittäisen DR-alue. Tämä johtuu siitä Azure paikkojen useimmilla on kummallakin alueella. Määritysten eivät suojaa sovelluksesi sekä alueiden kriittinen virhe. Vian epätodennäköistä tapahtuman voit palauttaa [geo palauttaminen](sql-database-disaster-recovery.md#recovery-using-geo-restore)-toiminnon kolmas alueen tietokantoja.

Kun käyttökatkosta, joiden on lievity, toissijaisen tietokanta synkronoidaan automaattisesti ensisijainen. Synkronoinnin aikana ensisijaisen suorituskyky saattaa hieman heikentyä tiedot, jotka on synkronoitava määrän mukaan. Seuraavassa kaaviossa on kuvattu käyttökatkosta toissijainen alueen.

![Toissijaisen tietokanta synkronoida ensisijainen. Cloud palauttaminen.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern1-3.png)


Rakenne-kuvion avaimen **etuja** ovat:

+ SQL-yhteysmerkkijonon on määritetty sovellusten käyttöönoton aikana kunkin alueen ja ei muuta jälkeen automaattisesti.
+ Sovelluksen suorituskyvyn vaikuttaa ei tue sovelluksen ja tietokannan ovat aina yhtä sijaitsee.

Tärkeimmät **tarjoa** on, että tarpeettomat sovelluksen esiintymää toissijainen alueen käytetään vain palauttaminen.

## <a name="design-pattern-2-active-active-deployment-for-application-load-balancing"></a>Rakenne-kaava 2: aktiivinen-aktiivinen käyttöönoton sovelluksen kuormituksen tasaamisen
Cloud tietojen palauttaminen asetus sopii parhaiten sovellusten seuraavat ominaisuudet:

+ Tietokannan lukujen kirjoituksia, suuri suhde
+ Tietokannan kirjoita viive ei vaikuta käyttäjäkokemuksen  
+ Vain luku-logiikkaa voidaan erottaa-luku-ja kirjoitusoikeudet logiikan eri yhteysmerkkijonon
+ Vain luku-logiikkaa ei ole riippuvainen tietojen täysin synkronoitu ja viimeisimmät päivitykset  

Jos sovellukset on nämä ominaisuudet, kuormituksen loppukäyttäjien yhteydet useita sovellusten esiintymiä eri alueiden välillä voi parantaa suorituskykyä ja käyttäjäkokemuksen. Kunkin alueen on toteuttamisesta kuormituksen tasaamisen, luku-ja kirjoitusoikeudet (RW) logiikan ensisijainen alueen ensisijainen-tietokantaan yhdistetty sovelluksen aktiivinen esiintymä. Vain luku-tilassa (RO)-funktioiden pitäisi olla yhteydessä toissijaisen tietokanta samalla, kun Sovellusesiintymän alueella. Liikenteen hallinta määritetään käyttämään [pyöreän pöydän reititys](../traffic-manager/traffic-manager-configure-round-robin-routing-method.md) tai [suorituskyvyn reititys](../traffic-manager/traffic-manager-configure-performance-routing-method.md) [läpikuultavuutta seuranta](../traffic-manager/traffic-manager-monitoring.md) sovelluksen jokaiselle esiintymälle käytössä.

Kuvion #1, kuten huomioon samalla seurantaa sovelluksen käyttöönotto. Mutta toisin kuin kuvion #1 seurantaa sovelluksen eivät ole vastuussa käynnistävä läpikuultavuutta vikasietotilaa.

> [AZURE.NOTE] Tämä kaava käyttää useamman kuin yhden toissijaisen tietokanta, vain yksi secondaries käytettävä automaattisesti edellä syistä. Koska tätä mallia edellyttää toissijaisen vain luku-, se vaatii aktiivisen Geo-replikoinnin.

Suorituskyvyn reititys ohjaamaan käyttäjän yhteydet lähinnä käyttäjän maantieteellisen sijainnin sovelluksen esiintymää on määritettävä liikenteen hallinta. Seuraavassa kaaviossa on kuvattu käyttökatkosta ennen määritysten.
![Ei ole käyttökatkosta: lähimpään sovelluksen reitittämisestä suorituskykyä. GEO-replikointi.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern2-1.png)

Jos järjestelmä tunnistaa tietokannan käyttökatkosta ensisijainen alueen, käynnistät automaattisesti ensisijaisen tietokannan johonkin toissijainen alueiden muuttaminen ensisijainen tietokannan sijainti. Liikenteen hallinta automaattisesti ulkopuolelle offline-tilassa läpikuultavuutta reititys-taulukosta, mutta jatkaa loppukäyttäjien liikenne reitittämisestä online jäljellä olevat esiintymät. Ensisijainen tietokanta on nyt toisella alueella, koska kaikki online esiintymät on muutettava muodostaa uusi ensisijainen niiden luku-ja kirjoitusoikeudet SQL-yhteysmerkkijonon. On tärkeää, että voit tehdä muutoksen ennen aloitetaan tietokannan vikasietotilaa. Vain luku-SQL-yhteyden merkkijonot olisi pysyy muuttumattomana, kun ne aina osoittamalla saman alueen tietokannan. Automaattisesti vaiheet ovat:  

1. Voit muuttaa luku-ja kirjoitusoikeudet SQL yhteyden merkkijonot siten, että uusi ensisijainen.
2. Soita nimettyjen toissijainen tietokannan järjestyksessä [Valmistele tietokannan automaattisesti](sql-database-geo-replication-portal.md).

Seuraavassa kaaviossa on kuvattu uusien määritysten jälkeen vikasietotilaa.
![Määritysten jälkeen automaattisesti. Cloud palauttaminen.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern2-2.png)

Jos jossakin toissijainen alueet käyttökatkosta-liikenteen hallinta automaattisesti poistaa offline-tilassa läpikuultavuutta alueen reititys-taulukosta. Replikoinnin kanavan alueella olevat toissijaisen tietokanta on hyllytetty. Jäljellä olevat alueet saada muokkaalinkki liikenne tässä tilanteessa, koska sovelluksen suorituskyvyn vaikuttaa käyttökatkosta aikana. Kun käyttökatkosta, joiden on lievity, sellaisiin alueen toissijaisen tietokanta synkronoidaan välittömästi ensisijainen. Synkronoinnin aikana ensisijaisen suorituskyky saattaa hieman heikentyä tiedot, jotka on synkronoitava määrän mukaan. Seuraavassa kaaviossa on kuvattu käyttökatkosta jossakin toissijainen alueet.

![Toissijainen alueen käyttökatkosta. Cloud palauttaminen - Geo replikoinnin.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern2-3.png)

Rakenne-kuvion avaimen **etu** on, että voit skaalata sovelluksen työmäärää yli useita secondaries saavuttaminen optimaalisen peruskäyttäjän. Tämä vaihtoehto **kompromissien** ovat seuraavat:

+ Palvelusovellusten esiintymät ja tietokannan välisiä yhteyksiä luku-ja kirjoitusoikeudet on erilaiset viive ja kustannukset
+ Sovelluksen suorituskykyyn vaikuttaa aikana käyttökatkosta
+ Palvelusovellusten esiintymät tarvitaan muutatkin dynaamisesti SQL-yhteysmerkkijonon tietokannan automaattisesti.  

> [AZURE.NOTE] Samaa menetelmää avulla voidaan purku erityinen toiminnoista, kuten raportoinnin työt, yritystietotyökalujen tai varmuuskopiot. Yleensä näistä toiminnoista kuluttavat merkittäviä tietokannan resurssit on suositeltavaa, jonka olet määrittänyt toissijainen tietokantojen ne vastaavat odotettua työmäärää suorituskyvyn tason kanssa.

## <a name="design-pattern-3-active-passive-deployment-for-data-preservation"></a>Rakenne-kaava 3: Aktiivinen-passiivinen käyttöönoton tietojen säilyttäminen  
Tämä asetus on parhaiten sovellusten seuraavat ominaisuudet:

+ Tietoja ei menetetä on hyvin liiketoiminnan riskejä. Tietokannan vikasietotilaa voidaan auta vain, jos käyttökatkosta on pysyvä.
+ Sovellus toimii "vain luku-tilassa" tietyn ajan.

Tätä mallia sovelluksen siirtyy toissijaisen-tietokantaan yhdistetty vain luku-tilassa. Sovelluksen logiikkaa ensisijainen alueen sijaitsee yhtä ensisijaisen tietokannan kanssa, ja se toimii luku ja kirjoitus-tilassa (RW). Sovelluksen logiikkaa toissijainen alueen sijaitsee yhtä toissijainen tietokannan kanssa, ja se on valmis toimimaan vain luku-tilassa (RO).  Liikenteen hallinta määritetään käyttämään [läpikuultavuutta seuranta](../traffic-manager/traffic-manager-monitoring.md) otettu käyttöön molemmissa Palvelusovellusten esiintymät [automaattisesti reititystä](../traffic-manager/traffic-manager-configure-failover-routing-method.md) .

Seuraavassa kaaviossa on kuvattu käyttökatkosta ennen määritysten.
![Aktiivinen-passiivinen-ympäristö, ennen kuin automaattisesti. Cloud palauttaminen.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern3-1.png)

Liikenteen hallinta havaitsee ensisijainen alueen yhteys epäonnistui, kun se siirtyy automaattisesti käyttäjän liikenne toissijainen alueen application-esiintymä. Tämä kaava on tärkeää, **Älä** aloittaa tietokannan automaattisesti, kun käyttökatkosta havaitaan. Toissijainen alueen sovellus aktivoituu ja toimii vain luku-tilassa käyttämällä toissijaisen tietokanta. Tämä on kuvattu seuraavassa kaaviossa.

![Käyttökatkosta: Sovelluksen vain luku-tilassa. Cloud palauttaminen.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern3-2.png)

Kun ensisijainen alueen käyttökatkosta, joiden on lievity, liikenteen hallinta havaitsee yhteyden palauttaminen ensisijainen alueen ja vaihtaa käyttäjän liikenne sovelluksen esiintymää ensisijainen alueen. Sovelluksen kyseisen esiintymän säilyy ja toimii ensisijainen tietokannan käyttämistä, luku-kirjoitus-tilassa.

> [AZURE.NOTE] Koska tätä mallia edellyttää lukuoikeudet toissijaisen edellyttää aktiivinen Geo-replikoinnin.

Toissijainen alueen käyttökatkosta liikenteen hallinta merkitsee sovelluksen ensisijainen alueen päätepiste heikentynyt ja replikoinnin kanavan on hyllytetty. Tämä käyttökatkosta ei vaikuta sovelluksen suorituskykyyn käyttökatkosta aikana. Kun käyttökatkosta, joiden on lievity, toissijaisen tietokanta synkronoidaan välittömästi ensisijainen. Synkronoinnin aikana ensisijaisen suorituskyky saattaa hieman heikentyä tiedot, jotka on synkronoitava määrän mukaan.

![Käyttökatkosta: Toissijaisen tietokanta. Cloud palauttaminen.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern3-3.png)

Rakenne-kaava on monia **etuja**:

+ Se estää tietojen menettämisen tilapäinen katkokset aikana.
+ Se ei edellytä voit ottaa käyttöön seurannan sovelluksen, kun palautus on saatu esiin liikenteen hallinta.
+ Käyttökatkot vain riippuu siitä, miten nopeasti liikenteen hallinta havaitsee connectivity-virhe, joka on määritettävä.

**Kompromissien** ovat seuraavat:

+ Sovelluksen on oltava toimimaan vain luku-tilassa.
+ Se tarvitaan dynaamisesti Vaihda siihen, kun yhteys on vain luku-tietokantaan.
+ Luku-ja kirjoitusoikeudet toimintojen antamisen jatkaminen edellyttää palautus ensisijainen alueen, mikä tarkoittaa, että koko tietojen käyttöä voidaan poistaa käytöstä tuntia tai jopa päivää.

> [AZURE.NOTE] Pysyvä palvelun käyttökatkosta alueen, jos kyseessä on manuaalisesti Aktivoi tietokannan automaattisesti ja hyväksy tietojen menettämisen. Sovellus on toissijainen alue, jossa luku-ja kirjoitusoikeudet tietokannan toiminnassa.

## <a name="business-continuity-planning-choose-an-application-design-for-cloud-disaster-recovery"></a>Liiketoiminnan jatkuvuus suunnittelu: Valitse sovellus-rakenne, cloud palauttaminen

Yrityksen tietyn cloud tietojen palauttaminen määrittäminen voit yhdistää tai laajentaa näitä rakenne kuviot tarpeita parhaiten sovelluksen.  Kuten edellä mainittiin, voit valita määrittäminen perustuu haluat tarjota asiakkaillesi ja sovelluksen käyttöönottoa topologian SLA. Voit parantaa päätöksesi, seuraavassa taulukossa on Vertailtu perusteella arvioidut tietojen menettämisen vaihtoehdot tai palautus tavoitteen (RPO) ja arvioitu Palautumisaika (Lisää).

| Kuvion | RPO | NNA
| :--- |:--- | :---
| Aktiivinen-passiivinen käyttöönoton yhtä sijaitsevat tietokannan käytön palauttaminen | Luku-ja kirjoitusoikeudet < 5 s | Virhe tunnistus aika + automaattisesti Ohjelmointirajapinnan kutsu + sovelluksen vahvistus testaaminen
| Aktiivinen aktiivinen käyttöönoton sovelluksen kuormituksen tasaamisen | Luku-ja kirjoitusoikeudet < 5 s | Virhe tunnistus aika + automaattisesti API puhelun + muuttaminen SQL-yhteysmerkkijonon + sovelluksen vahvistus testaaminen
| Aktiivinen-passiivinen käyttöönoton tietojen säilyttäminen | Vain luku-tilassa < 5 sec luku-ja kirjoitusoikeudet on nolla | Vain luku-tilassa = yhteys epäonnistui tunnistus aika + sovelluksen vahvistus testi <br>Luku-ja kirjoitusoikeudet = aika pienentämään käyttökatkosta

## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja Azure SQL-tietokanta automaattisen varmuuskopioinnin on artikkelissa [SQL-tietokantaan automaattisen varmuuskopioinnin suunnittelu](sql-database-automated-backups.md)
- Liiketoiminnan jatkuvuus-yleiskatsaus ja tilanteita, joissa on artikkelissa [liiketoiminnan jatkuvuuden yhteenveto](sql-database-business-continuity.md)
- Lisätietoja palauttaminen automaattisen varmuuskopioinnin käyttämisestä on artikkelissa [tietokannan palvelun käynnistämä-varmuuskopioista palauttaminen](sql-database-recovery-using-backups.md)
- Lisätietoja nopeampaa palautusasetukset on artikkelissa [Aktiivinen-Geo-replikoinnin](sql-database-geo-replication-overview.md)  
- Lisätietoja käyttämisestä automaattisen varmuuskopioinnin arkistointia varten, tutustu artikkeliin [tietokannan kopioiminen](sql-database-copy.md)
- Tietoja Active Geo-replikointi joustavasti jakavat lisätietoja [joustavasti resurssivarantoon varalle](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).

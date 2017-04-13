<properties
   pageTitle="Tietojen palauttaminen ja suuren käytettävyyden Azure sovellusten | Microsoft Azure"
   description="Tekniset yhteenvetoja ja syvyys tiedot suuri käytettävyys ja tietojen palauttaminen sovellusten rakennettu Microsoft Azure-sovellusten suunnitteleminen."
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

#<a name="disaster-recovery-and-high-availability-for-applications-built-on-microsoft-azure"></a>Tietojen palauttaminen ja sovellusten Microsoft Azure rakennettu suuri käytettävyys

##<a name="introduction"></a>Johdanto

Tässä artikkelissa keskitytään hyvin Azure-sovellusten käytettävyyden. Suuren kokonaisvaltainen strategia myös palauttaminen näkökohdat. Virheet ja katastrofit pilveen suunnitteleminen edellyttää, että tunnista virheet nopeasti. Valitse Toteuta strategia, joka vastaa omaa poikkeama sovelluksen käyttökatkot varten. Sinun on lisäksi, harkitse tietojen menettämisen sovellusta hyväksyt ilman aiheuttaa haitalliset business vaikutukset, kun se on palautettu laajuus.

Useimpien yritysten oletetaan, että ne on valmistettu määräaikainen ja suurissa virheet. Ennen kuin voit vastata kysymykseen itsellesi, yrityksesi harjoittelu nämä virheet? Voit testata tietokantojen ja sinulla on oikeat prosessit paikassa palautus? Ei ehkä ole. Tämä johtuu onnistuneen palauttaminen alkaa paljon suunnitteluun ja toteuttamiseen nämä prosessit suunnittelee. Monet muut toiminnassa vaatimukset, kuten suojauksen, kuten-palauttaminen saa harvoin huolellisesti analyysi- ja aikakohdistus edellyttää. Myös useimmat yritykset ei ole maantieteellisesti alueet, joiden tarpeettomat kapasiteetin budjetti. Näin ollen jopa toiminta tärkeät sovellukset on usein raporttikyselyä ERISNIMI tietojen palauttamisen suunnittelu.

Cloud ympäristöjen, kuten Azure, anna maantieteellisesti hajautettuja alueiden eri puolilla maailmaa. Myös näissä ympäristöissä sisältää ominaisuuksia, jotka tukevat käytettävyys ja tietojen palauttaminen skenaarioita. Nyt voidaan antaa jokaisen toiminta kriittinen cloud sovelluksen tietojen oikeinkirjoitusta järjestelmän asianmukaisesti huomioon. Azure on vikasietoisuudesta ja palauttaminen monia sen palveluiden hallinta. Study ympäristö näiden ominaisuuksien huolellisesti ja täydentää sovelluksen strategioita kanssa.

Tässä artikkelissa ääriviivat tarvittavat arkkitehtuuri vaiheet, sinun on otettava tietojen kuitti Azure käyttöönotto. Sitten voit toteuttaa suurempi jatkuvuuden Liiketoimintaprosessi. Liiketoiminnan jatkuvuus suunnitelma on ohjeet, jatkuvat toiminnot haitalliset olosuhteissa. Tämä voi johtua tekniikalla, kuten downed palvelu tai luonnollinen tietojen, kuten myrsky- tai power käyttökatkosta epäonnistui. Sovelluksen vikasietoisuudelle Luonnonvahinkoja koskevat on suurempi tietojen palauttaminen prosessin alijoukko ohjeiden NIST tässä asiakirjassa: [Tiedot tekniikka järjestelmien suunnitteluopas vara](https://www.fismacenter.com/sp800-34.pdf).

Seuraavissa osissa määrittää eri virheet, avulla voit käsitellä niitä ja arkkitehtuureihin, jotka tukevat seuraavia tekniikoita. Nämä tiedot on syötteen tietojen palauttaminen prosessit ja toimintojen, jotta yrityksen tietojen palauttaminen määrittäminen toimii oikein ja tehokkaasti.

##<a name="characteristics-of-resilient-cloud-applications"></a>Joustavat cloud sovellusten ominaisuudet

Hyvin architected sovelluksen voi kestää vain virheet taktisen tasolla ja hyväksyt myös strategisten järjestelmää virheet alueen tasolla. Seuraavissa osissa määrittää koko asiakirjan eri ominaisuuksia joustavat pilvipalveluihin kuvaamaan Viitattu termejä.

###<a name="high-availability"></a>Suuri käytettävyys

Erittäin käytettävissä cloud-sovelluksen toteuttaa strategioita Vaimenna riippuvuudet, kuten hallittuja cloud-ympäristössä tarjoamien palvelujen käyttökatkosta. Tämän menetelmän sallii huolimatta mahdolliset virheet cloud-ympäristössä ominaisuuksia, sovellus jatkaa toimimaan odotettu toimintojen ja toimintojen systeeminen ominaisuudet. Tämä koskee perusteellisempaa kanavan 9 videosarja [Failsafe: joustavat Cloud arkkitehtuureihin ohjeet](https://channel9.msdn.com/Series/FailSafe).

Kun sovellus otetaan käyttöön, ota huomioon ominaisuuksien käyttökatkosta todennäköisyyden. Harkitse lisäksi vaikutus käyttökatkosta on sovelluksessa liiketoiminnan kannalta ennen Sukeltava kattavaa käyttöönoton strategiat kyselyjä. Ilman määräpäivää huomiota business vaikutus ja todennäköisyys pallolla riskin ehto-käyttöönoton voi olla kallista ja mahdollisesti tarpeettomat.

Harkitse auton vastaavasti, suuren. Myös laatu-osat ja ylätason tekniikka ei estä ajoittaiset virheet. Esimerkiksi kun auton syötetään tasainen tire, auton toimii edelleen, mutta se toimii eivät toimi oikein toimintojen kanssa. Jos suunnitellut tämän mahdolliset esityksen, voit johonkin näistä ohut rimmed vara renkaat, kunnes pääset korjaa ostaminen. Vaikka käyttämätöntä tire ei salli nopeus, voit silti toimi ajoneuvon ennen kuin voit korvata tire. Vastaavasti pilvipalvelussa suunnittelee ominaisuuksia ei menetetä, voit estää suhteellisen pieni ongelma joka tuo koko sovelluksen alaspäin. Tämä pätee, vaikka pilvipalvelussa on suoritettava toiminnot eivät toimi oikein.

On erittäin käytettävissä pilvipalveluihin muutaman tärkeimmät ominaisuudet: käytettävyyden, skaalattavuus ja vikasietoa. Nämä ominaisuudet liittyvät toisiinsa, mutta se on tärkeää ymmärtää kunkin ja miten ne vaikuttavat ratkaisun yleistä käytettävyyttä.

###<a name="availability"></a>Käytettävyys

Käytettävissä sovelluksen katsoo sen pohjana oleva infrastruktuuri ja riippuvaisia palveluita käytettävyyttä. Käytettävissä olevat sovellukset poistaa yksittäisen kohtiin virheen redundancy ja joustavat rakenteen kautta. Kun voit laajentaa huomioitavia Azure käytettävyys, on tärkeää ymmärtää platform tehokkaita käytettävyyttä käsite. Tehokas käytettävyys katsoo palvelussa tason toimeenpano (SLA) kunkin riippuvaiset palvelun ja kumulatiivinen vaikuttavat yhteensä järjestelmän käytettävyyttä.

Järjestelmän käytettävyyttä on aika-ikkunassa järjestelmä voi toimia prosenttiosuuden mitta. Esimerkiksi käytettävyys SLA vähintään kaksi esiintymien verkossa tai Työntekijä-roolin Azure-tietokannassa on 99.95 prosenttia (ulos 100 prosenttia). Se ei mittaa suorituskykyä tai rooleja palvelut toimintoja. Kuitenkin pilvipalvelussa sijaitsevaan tehokkaita käytettävyyttä vaikuttaa myös riippuvainen muiden eri palvelutasosopimuksia. Lisää liikkuvat osat järjestelmässä Lisää huolellisesti, sinun on otettava varmistamiseksi, että voit resiliently täyttävät käytettävyys sen käyttäjille.

Ota huomioon seuraavat palvelutasosopimuksia Azure-palvelua, joka käyttää Azure services: Laske Azure SQL-tietokanta ja Azure-tallennustilan.

|Azure-palvelu|SLA   |Mahdollinen minuuttia käyttökatkot ja kuukauden (30 päivää)|
|:------------|:-----|:----------------------------------------:|
|Laske      |99.95 %|21,6 minuuttia                              |
|SQL-tietokantaan |99,99 %|4.3 minuuttia                              |
|Tallennustilan      |99.90 %|43.2 minuuttia                              |

Täytyy suunnitella kaikki palvelujen eri aikoina mahdollisesti taaksepäin. Vaivaton tässä esimerkissä minuuttia kuukaudessa, sovellus saattaa olla alaspäin kokonaismäärän on 108 minuuttia. 30 päivän kuukausi on yhteensä 43,200 minuuttia. 108 minuuttia on.25 prosenttiosuus minuutin 30 päivää kuukaudessa (43,200 minuuttia) kokonaismäärä. Kyseinen rivimäärä on tehokas saatavuuden 99.75 prosenttia cloud-palveluun.

Kuitenkin tässä asiakirjassa kuvattuja käytettävyys menetelmiä käyttäen parantaa näin. Esimerkiksi jos suunnittelet sovellusta edelleen käytössä, kun SQL-tietokanta ei ole käytettävissä, voit poistaa, kaava. Tämä saattaa tarkoittaa, että sovellus toimii rajoitetun ominaisuuksia, joten liittyy myös business vaatimukset ottaa huomioon. Katso Azure palvelutasosopimuksia kattavaan luetteloon, [Palvelun tason sopimuksia](https://azure.microsoft.com/support/legal/sla/).

###<a name="scalability"></a>Skaalattavuus

Skaalattavuus vaikuttaa suoraan käytettävyyttä. Sovelluksen, jotka parantavat kuormitus epäonnistuu ei ole enää käytettävissä. Skaalattavissa sovellukset voivat täyttävät parantavat demand ja hyväksyttävä aika windows yhtenäinen tulos. Kun järjestelmä on skaalattava, se sovittaa vaaka- tai pystysuunnassa hallittavan kasvaa kuormituksen hyvään suorituskykyyn säilyttäen. Peruskäsitteet skaalauksen vaakasuunnassa Lisää samankokoinen (suoritin, muistin ja kaistanleveyden), Lisää koneet aikana pystysuora skaalauksen kasvaa aiemmin koneet kokoa. Azure-sinulla pystysuora skaalausasetukset Laske koneen erikokoisista valitsemiseen. Koneen koon muuttaminen vaatii kuitenkin uudelleen käyttöönotto. Tämän vuoksi eniten joustavia ratkaisuja käsitellään skaalauksen vaakasuunnassa. Tämä koskee erityisesti suorittaminen, koska voit helposti lisätä verkossa tai Työntekijä-roolin kopioiden määrä. Nämä uusia esiintymiä käsitellään liikenteen Azure-Web-portaaliin, PowerShell-komentosarjojen tai koodi. Peruskalenteri tietyn valvottu arvot kasvu tämä päätös. Tässä skenaariossa käyttäjän suorituskykyä tai arvot ei haittaohjelman huomattavia avattavan kuormitus. Yleensä Internetin kautta tai työntekijä roolit tallentaa ulkoisesti tila. Tämä mahdollistaa joustavan kuormituksen tasaamisen ja kaksivaiheista käsittely tehdyistä muutoksista esiintymän arvo. Skaalauksen vaakasuunnassa toimii hyvin myös palvelut, kuten Azure-tallennustilan, joka ei ole Porrastettu asetusten skaalauksen pystysuunnassa.

Cloud ominaisuuksissa pitäisi näkyä asteikon yksiköitä kokoelma nimellä. Näin sovelluksen joustavasti-ylläpidon loppukäyttäjät siirtonopeuden tarpeisiin. Skaalaa yksiköt kuuluvat lämpökartaksi Internetin kautta tai sovelluksen palvelimen-tasolla. Tämä johtuu siitä Azure on jo tilattomien Laske solmujen Internetin kautta tai työntekijä rooleilla. Lisääminen Lisää Laske aikayksikön-asennuksen aiheuttaa ei puoli sovelluksen tilan hallinta-tehosteet, koska Laske aikayksikön tilattomien. Tallennustilan asteikko-yksikkö vastaa osion tietojen hallinnasta (structured tai rakenteeton). Tallennustilan aikayksikön esimerkiksi Azure-taulukosta osion Azure-Blob-säilö ja Azure SQL-tietokantaan. Jopa usean Azure-tallennustilan tilin käyttö on suora vaikutus sovelluksen skaalattavuus. Täytyy suunnitella erittäin skaalattava pilvipalvelussa, voit lisätä useita tallennustilan asteikon yksiköitä. Esimerkiksi jos sovellus käyttää relaatiotietoja-osion tiedot yli useita SQL-tietokannat. Tällöin avulla seurata joustavasti Laske asteikko mallin tallennustilan. Vastaavasti Azuren tallennustilaan mahdollistaa tietojen jakaminen mallit, jotka edellyttävät siirtonopeuden tarpeiden Laske kerroksen tarkoituksellisen suunnittelua varten. Parhaita käytäntöjä suunnitteluun skaalattava pilvipalveluihin luettelo on artikkelissa [Rakenne, suuren mittakaavan palvelut Azure pilvipalveluihin parhaat käytännöt](https://azure.microsoft.com/blog/best-practices-for-designing-large-scale-services-on-windows-azure/).

###<a name="fault-tolerance"></a>Vikasietoa

Sovellusten olettaa jokaisen riippuvaiset cloud-ominaisuutta voi ja siirtyvät joskus samanaikaisesti. Vian varmatoimisempia sovellus tunnistaa ja siirtoja epäonnistui osia, voit jatkaa ja palauta oikeita tuloksia tietyn aikajakson sisällä ympärille. Lyhytkestoisia virhetilanteita vika varmatoimisempia järjestelmän käyttää uudelleen käytännön. Lisää vakavia virheitä sovelluksen voit havaita ja epäonnistua päälle vaihtoehtoinen laitteistoa tai ehdollisen suunnitelmat, kunnes virhe korjataan. Luotettavan sovelluksen voit oikein hallinta yhden tai useita osia virheen ja jatkaa oikein. Vian varmatoimisempia sovellukset voivat käyttää vähintään yhden rakenne strategioita, kuten redundancy, replikoinnin tai toiminnot eivät toimi oikein.

##<a name="disaster-recovery"></a>Tietojen palauttaminen

Cloud käyttöönoton voi lakata toimimasta systeeminen käyttökatkosta riippuvaisia palveluita tai pohjana oleva infrastruktuuri vuoksi. Ehdoin jatkuvuuden liiketoimintasuunnitelma käynnistää tietojen palauttaminen-prosessin. Tässä prosessissa on yleensä toimintojen henkilöstön ja automaattisia ohjeita, jos haluat aktivoida sovelluksen käytettävissä alueella. Tämä edellyttää sovelluksen käyttäjille, tietojen ja palvelujen siirretään uusi alue. Siihen liittyy myös varmuuskopiosta tai jatkuvan replikoinnin.

Harkitse, suuri mahdollisuus palauttaminen tasainen tire käyttämällä varaosien saatavuus verrattuna edellisen vastaavasti. Sen sijaan palauttaminen on otettava kohtaa, johon auton ei ole enää toiminnassa Auto, kaatumisen jälkeen vaihetta. Tässä tapauksessa paras mahdollinen ratkaisu on tehokas tapa muuttaa autot, matka-palvelua tai ystävän soittamalla löytää. Tässä skenaariossa on todennäköisesti käyttäjästä tulee pidempi viive käytön takaisin matkoilla. On myös monimutkaisia korjaaminen ja palaa alkuperäiseen ajoneuvon. Samalla tavalla palauttaminen toisen alueen on monimutkainen tehtävä, joka liittyy yleensä joitakin käyttökatkot ja mahdollisesta tietojen menettämisen. Paremmin ymmärrä ja arvioiminen varalle, on tärkeää määrittää kahden ehdot: palautus aika tavoitteen (RTO) ja palautus valitsemalla tavoitteen (RPO).

###<a name="recovery-time-objective"></a>Palautus aika-tavoite

RTO on kohdistettu palauttaminen toiminnoista. Tämä on business tarpeiden perusteella ja se liittyy sovelluksen tärkeys. Kriittinen business-sovellukset edellyttävät pienen RTO.

###<a name="recovery-point-objective"></a>Palautus piste-tavoite

RPO on hyväksyttävä aikaikkunan menettää tietojen palauttaminen vuoksi. Esimerkiksi RPO onko tunnin on kokonaan Varmuuskopioi tai kopioida tiedot vähintään kerran tunnissa. Voit noutaa sovelluksen vaihtoehtoinen alueen, kun varmuuskopiotiedot voi puuttua tunnin tietojen ylöspäin. Tärkeiden sovellusten kohde RTO, kuten paljon pienempi RPO.

##<a name="checklist"></a>Tarkistusluettelo

Yhteenveto oletetaan, että tärkeimmät kohdat, jotka on tasaamisessa sisältö (ja sen [suuri käytettävyys](./resiliency-high-availability-azure-applications.md) ja Azure sovellusten [palauttaminen](./resiliency-disaster-recovery-azure-applications.md) Aiheeseen liittyviä artikkeleita). Yhteenveto toimii oman käytettävyys ja tietojen palauttaminen suunnittelussa huomioon kohteiden tarkistusluettelon. Nämä ovat parhaita käytäntöjä, jotka on hyötyä asiakkaille, saat tietoja onnistuneen ratkaisun toteuttaminen vakavia hakemista.

1. Järjestä kunkin sovelluksen risk Assessment-sovelluksesta, koska kunkin on erilaisia vaatimuksia. Jotkin sovellukset ovat kriittisiä enemmän kuin muiden ja Tasaa ylimääräisiä maksaa arkkitehti ne tietojen palauttamista varten.
1. Tämän toiminnon avulla voit määrittää RTO ja RPO kunkin sovelluksen.
1. Rakenne-virhe, sovellus-arkkitehtuuri alkaen.
1. Ota käyttöön suuri käytettävyys parhaat käytännöt aikana tasaamisen kustannukset, monimutkaisuus ja risk.
1. Ota käyttöön tietojen ja prosessien.
  * Harkitse virheet, jotka ulottuvat aivan valmis cloud käyttökatkosta moduulitaso.
  * Määrittää viite- ja tapahtumatietojen varmuuskopion strategioita.
  * Valitse usean sivuston tietojen palauttaminen-arkkitehtuuri.
1. Määritä tietojen palauttaminen prosesseja, automaatio ja testauksen tiettyyn omistajaan. Omistajan tulisi hallinta ja oma koko prosessi.
1. Asiakirjan prosesseja, jotta ne ovat helposti toistettavien. Vaikka yksi omistaja, usealle pitäisi ymmärtää ja seuraa emergency prosessit.
1. Kouluttaminen toteuttamisesta prosessi.
1. Käyttää Normaali tietojen simulaatioita koulutus- ja kelpoisuuden yhteydessä.

##<a name="summary"></a>Yhteenveto

Kun laitteiston tai sovellukset eivät sisällä Azure, tekniikoita ja strategioita hallinta ne ovat eri kuin milloin paikallista järjestelmiin tapahtuu virhe. Tärkeimmät syy tähän on, että cloud ratkaisuja on yleensä useita riippuvuuksia julkaisuinfrastruktuuri on niille päiville Azure alue ja tuleeko erillisiä palveluita. On käsitellä suuren käytettävyyden menetelmillä osittaisia virheitä. Voit hallita Lisää vakavia virheitä, mahdollisesti tietojen tapahtumasta vuoksi käyttämällä varalle.

Azure tunnistaa ja käsitellä useita virheitä, mutta on useita erityyppisiä virheet, jotka edellyttävät sovelluksen strategiat. On aktiivisesti valmisteleminen ja hallitse sovelluksia ja palveluja virheet.

Luodessasi sovelluksen saatavuus ja tietojen palauttaminen suunnitella, harkitse sovelluksen virhe business-vaikutukset. Määrittäminen prosesseja, käytännöt ja menetelmiä Palauta kriittiset järjestelmät kriittinen tapahtuma kestää jonkin aikaa, suunnittelussa ja sitoumus. Ja kun muodostat suunnitelmat, voit et voi lopettaa siellä. Säännöllisesti analysoida, Testaa ja parantaa jatkuvasti suunnitelmien sovelluksen portfolion, yrityksen tarpeet ja käytettävissäsi tekniikoita perusteella. Azure tarjoaa uusia ominaisuuksia ja aiheuttaa uusia haasteita luominen tehokkaat sovelluksia, jotka kestävät virheet.

##<a name="additional-resources"></a>Lisäresursseja

[Microsoft Azure rakennettu sovellusten suuri käytettävyys](resiliency-high-availability-azure-applications.md)

[Microsoft Azure-sovellusten palauttaminen](resiliency-disaster-recovery-azure-applications.md)

[Azure vikasietoisuudelle tekniset ohjeet](resiliency-technical-guidance.md)

[Yleistä: Cloud liiketoiminnan jatkuvuuden ja tietokannan tietojen palauttaminen SQL-tietokantaan](../sql-database/sql-database-business-continuity.md)

[SQL Server Azuren näennäiskoneiden suuri käytettävyys ja tietojen palauttaminen](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md)

[FailSafe: Joustavat cloud arkkitehtuureihin ohjeet](https://channel9.msdn.com/Series/FailSafe)

[Suurissa Azure Cloud Services-palvelut rakenteen parhaat käytännöt](https://azure.microsoft.com/blog/best-practices-for-designing-large-scale-services-on-windows-azure/)

##<a name="next-steps"></a>Seuraavat vaiheet

Tässä artikkelissa on kohdistettu palauttaminen ja suuren käytettävyyden Azure sovellusten artikkelisarja osa. Seuraava artikkeli sarjassa on [suuri käytettävyys rakennettu Microsoft Azure-sovellusten](resiliency-high-availability-azure-applications.md).

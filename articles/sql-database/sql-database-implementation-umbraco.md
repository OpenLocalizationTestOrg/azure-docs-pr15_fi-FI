<properties
   pageTitle="Azure SQL-tietokannan Azure Esimerkkitapaus - Umbraco | Microsoft Azure"
   description="Tietoja siitä, miten Umbraco käyttää nopeasti valmistelu SQL-tietokantaan ja skaalaus-palveluiden tuhansia vuokraajiin pilveen"
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/22/2016"
   ms.author="carlrab"/>

# <a name="umbraco-uses-azure-sql-database-to-quickly-provision-and-scale-services-for-thousands-of-tenants-in-the-cloud"></a>Umbraco käyttää nopeasti säännöstä ja skaalausta Servicesiin pilveen vuokraajiin tuhansia Azure SQL-tietokanta

![Umbraco-Logo](./media/sql-database-implementation-umbraco/umbracologo.png)

Umbraco on Suositut Avaa lähde sisällön hallinta-järjestelmä (CMS), voit suorittaa mitään pieni markkinointikampanjan tai esite sivustoissa monimutkaisia sovellusten Fortune 500-yrityksiä ja Yleinen median sivustot. 

> "On varsin suuren yhteisön sovelluskehittäjät, jotka järjestelmän käyttäminen yli 100 000 kehittäjät Microsoftin keskustelupalstat ja yli 350,000 sivustot ja jotka ovat elävä käynnissä Umbraco."

> – Morten Christensen tekniset liidin Umbraco

> [AZURE.VIDEO azure-sql-database-case-study-umbraco]

Asiakkaan ominaisuuksissa helpottaa Umbraco lisätty Umbraco nimellä--palvelun (UaaS): ohjelmiston nimellä--palvelun (SaaS) tarjoaa, joka ei tarvitse paikallisten versioiden sisältää valmiita skaalaus ja poistaa hallinta yleisrasite ottamalla kehittäjät keskittyminen tuotteen innovaatiot sijaan ratkaisun hallinta. Umbraco voi säätää näitä etuja Microsoft Azure tarjoamia joustava ympäristö nimellä--palvelun (PaaS)-mallin avulla.

UaaS avulla SaaS asiakkaat voivat käyttää Umbraco CMS ominaisuuksia, jotka olivat aiemmin niiden reach ulos. Näiden asiakkaiden on valmisteltu kanssa työskentely CMS-ympäristön, joka sisältää tuotannon tietokannan. Asiakkaat, voit lisätä enintään kaksi muut tietokannat kehittämisen ja väliaikaisen ympäristöissä yhtiön mukaan. Uuteen ympäristöön pyydettäessä automatisoidun prosessin määrittää asiakkaan tietokannan automaattisesti. Uuden tietokannan on valmis sekunteina, koska tietokanta on jo valmiiksi valmisteltu Umbraco Azure joustavasti olevilla käytettävissä olevien tietokantojen (katso kuva 1) mukaan.


![Umbraco valmistelu elinkaari](./media/sql-database-implementation-umbraco/figure1.png)

Kuva 1. Valmistelu elinkaari Umbraco serviceksi (UaaS)
 
##<a name="azure-elastic-pools-and-automation-simplify-deployments"></a>Azure joustavasti jakavat- ja automaatiofunktiot yksinkertaistaa ominaisuuksissa

Azure SQL-tietokanta ja Azure muiden Umbraco asiakkaat voivat itse valmistella ympäristöissä ja Umbraco helposti seurata sekä tietokantojen hallinta intuitiivisen työnkulun osana:

1.  Valmistelu

    Umbraco ylläpitää 200 käytettävissä olevat ennalta valmistellun tietokantojen joustavasti jakavat kapasiteetista. Kun uusi asiakas kirjautuu UaaS, Umbraco on lähelle reaaliajassa CMS-ympäristössä, jossa on uusi asiakkaalle määrittämällä tietokannan käytettävyys käytetään varannon.

    Kun käytettävyys resurssivarantoon saavuttaa sen raja-arvon, uusi joustavasti varanto on luotu ja uusien tietokantojen ovat valmiiksi valmistellun varataan asiakkaille tarpeen mukaan.

    Toteutuksen jälkeisen on täysin automaattinen C# hallinta kirjastojen ja Azure-palvelu Bus olevien avulla.

2.  Käyttämiseen

    Asiakkaiden käyttäminen 3 ympäristöissä (tuotannon, väliaikaisen ja/tai kehitys), jokaisen omassa tietokannan. Asiakas-tietokantoja on joustavasti tietokannan jakavat, joka mahdollistaa Umbraco antamaan tehokas skaalaus eikä sinun tarvitse perusteettomasti valmistelu.

    ![Umbraco projektin yleiskatsaus](./media/sql-database-implementation-umbraco/figure2.png)

    ![Umbraco projektin tiedot](./media/sql-database-implementation-umbraco/figure3.png)

    Kuva 2. Umbraco nimellä--palvelun (UaaS) asiakkaan sivustoon, jossa projektin yleiskatsaus- ja tiedot

    Azure SQL-tietokanta käyttämällä tietokannan tapahtuman yksiköt (DTUs) vastaa tosielämän tietokannan tapahtumia vaaditaan suhteellisia power. Asiakkaiden UaaS tietokannat toimivat yleensä noin 10 DTUs, mutta jokaisella joustavuus skaalata pyydettäessä. Tämä tarkoittaa UaaS voit varmistaa, että asiakkaiden on aina tarvittavat resurssit, vaikka piikin esiintymistä. Esimerkiksi viimeisimmät sunnuntai yöllä urheilu tapahtuman aikana yksi UaaS asiakkaan kokeneilta tietokanta päät enintään 100 DTUs Ottelu ajaksi. Azure joustavasti jakavat suoritetut Umbraco tukemaan, suuri tarvittaessa ilman kansiohierarkiassa.

3.  Valvonta

    Umbraco näyttöjen tietokannan toiminto, jossa käytetään raporttinäkymien Azure portaalin sekä mukautetut sähköposti-ilmoituksia.

4.  Tietojen palauttaminen

    Azure kaksi vaihtoehtoa palauttaminen (DR): aktiivinen Geo-replikointi ja Geo palauttaminen. Yrityksen kannattaa valita DR haluamasi vaihtoehto määräytyy sen [Liiketoiminnan jatkuvuus tavoitteet](sql-database-business-continuity.md).

    Aktiivinen Geo-replikoinnin on nopein taso vastauksen käyttökatkot yhteydessä. Käytä aktiivinen Geo-replikoinnin, voit luoda enimmillään neljä luettavissa secondaries palvelimissa eri alueilla ja voit aloittaa automaattisesti mihin tahansa secondaries sitten virheen sattuessa.

    Umbraco ei edellytä Geo replikoinnin, mutta se hyödyntää, Azure Geo-palauttaminen pienin käyttökatkot varmistaa Jos käyttökatkosta. GEO palautus on riippuvainen tietokannan varmuuskopioiden geo tarpeettomat Azure-tallennustilan. Käyttäjät, jotka voivat palauttaminen varmuuskopiosta, kun kyseessä on käyttökatkosta ensisijainen alueen.

5.  Jään valmistelu

    Kun projektiympäristössä, jossa on poistettu, liittyvät tietokannat (kehitys, väliaikaisen tai reaaliaikaista) poistetaan Azure palvelun Bus jonon tyhjennys aikana. Tämä automatisoidun prosessin palauttaa käyttämättömät tietokantojen Umbraco's joustavasti tietokannan käytettävyyden resurssivarantoon, että ne käytettävissä tulevien valmisteluun suurinta käyttöastetta säilyttäen.

##<a name="elastic-pools-allow-uaas-to-scale-with-ease"></a>Joustavasti jakavat Salli UaaS skaalata vaivattomasti

Azure joustavasti tietokannan jakavat hyödyntämistä Umbraco voi optimoida asiakkaiden suorituskyvyn tarvitsematta yli - tai alapuolella säännöstä. Umbraco on tällä hetkellä lähes 3 000 tietokantojen 19 joustavasti tietokannan jakavat skaalata helposti tarvittaessa sopimaan 325,000 asiakkaat-tai uusi asiakkaat, jotka haluat ottaa CMS pilveen mahdollisuuden kautta.

Itse asiassa Morten Christensen, teknisiä johtaa Umbraco, milloin mukaan "UaaS on nyt sijainti kasvua noin 30 päivää kohden asiakkaille. Asiakkaidemme ovat delighted käyttää pysty Valmistele uusien projektien sekunteina, julkaista välittömästi käyttämällä "yhdellä napsautuksella käyttöönotto" kehitysympäristö päivitykset live sivustoihinsa ja tehdä muutoksia, kuten nopeasti, jos ne etsiä virheitä."

Jos asiakas ei edellytä toisen tai kolmannen ympäristön enää, se voi poistaa vain ne ympäristössä. Joka vapauttaa, jonka avulla voidaan muiden asiakkaiden osana Umbraco joustavasti tietokannan käytettävyyden varannon resursseja.

![Umbraco käyttöönotto-arkkitehtuuri](./media/sql-database-implementation-umbraco/figure4.png)

Kuva 3. UaaS käyttöönoton arkkitehtuuri Microsoft Azure-

##<a name="the-path-from-datacenter-to-cloud"></a>Cloud palvelinkeskuksen polun

Umbraco kehittäjät alun perin tehty päätös Siirry SaaS mallin, ne tietää, että ne on edullinen ja skaalattava vielä palvelun muodostetaan.

> "Joustavasti tietokannan jakavat sopivat sopivaa ratkaistavaan Microsoftin kun SaaS ojentamassa koska emme voi soittaa kapasiteetin ylös ja alas tarpeen mukaan. Valmistelu on helppoa, ja tutustu määritykset on pitää käyttö enintään."

> – Morten Christensen tekniset liidin Umbraco

"On haluat käyttää Microsoftin aikaa, että asiakkaiden ongelmien ratkaisemiseen hallinta ei infrastruktuuria. Olemme määrittää helposti asiakkaidensa useimmat arvoa"kertoo Niels Hartvig Umbraco myönnetään. "On alun perin pidettäviä isännöinnin molemmista palvelimissa, mutta kapasiteetin suunnittelua olisi ollut nightmare." Coincidentally Umbraco ei käyttää minkä tahansa tietokannan Järjestelmänvalvojat, joka alaviivoja avainarvon-ehdotus käytettäessä UaaS.

Yksi tärkeä tavoite Umbraco kehittäjille oli lisäämistapaa UaaS asiakkaiden valmistelu ympäristöissä nopeasti ja kapasiteettirajoitukset. Mutta -Umbraco palvelinkeskusten erillinen isännöityä palvelujen tarjoaminen on pakollinen ylimääräisen kapasiteetin käsittelemään käsittely bursts paljon. Jotka on tarkoitettu lisääminen huomattavia Laske infrastruktuurin, on ollut säännöllisesti underutilized.

Lisäksi Umbraco ryhmälle määrittää, he voivat käyttää paljon niiden aiemmin koodin mahdollisimman ratkaista. Umbraco kehittäjän Mikkel Madsen kertoo, "emme tyytyväinen Microsoft Kehitystyökalut, emme jo aiemmin, kuten Microsoft SQL Server, Microsoft Azure SQL-tietokanta tai ASP.net Internet Information Services (IIS). Ennen kuin IaaS tai PaaS investointien cloud-ratkaisun on etsimäsi varmistaaksesi, että se tue sekä Microsoft-työkalut ja ympäristöjen, joten ei tarvitse tehdä koodia valtaviin muutoksia perus. "

Kaikki sen ehdot täyttävän Umbraco etsitään cloud kumppanin kanssa seuraavanlainen pätevyys:

-   Riittävän kapasiteetin ja luotettavuutta
-   Tuki Microsoft Kehitystyökalut niin, että Umbraco insinöörien ei pakotettu reinvent kokonaan niiden kehitysympäristö
-   Tavoitettavuustietojen kaikista missä UaaS joiden kilpailee (yritysten tarvetta varmistaa, että he pääsevät niiden tiedot nopeasti ja niiden tiedot tallennetaan paikkaan, joka täyttää niiden alueellisen lainmukaisia) maantieteelliset markkina-alueet

##<a name="why-umbraco-chose-azure-for-uaas"></a>Miksi Umbraco valitsemasi Azure UaaS

Käyttäjäryhmäsääntöihin Morten Christensen "jälkeen lähitulevaisuudessa Microsoftin asetukset on valittuna Azure koska se täyttää kaikki Microsoftin ehdot, hallinta ja skaalattavuus kannattaa ja kustannustehokkuus. Kokeiluversion määrittäminen Azure VMs-ympäristöissä ja kussakin ympäristössä on oma Azure SQL-tietokanta, kaikki joustavasti tietokannan jakavat esiintymät. Tietokantojen kehitystä, väliaikaisen ja live ympäristöjen välillä erottamalla Microsoft tarjoa asiakkaidemme tehokkaat suorituskyvyn eristystaso vastaavat asteikko – erittäin suuri win. "

Morten jatkuu "ennen on ollut valmistelu Verkkotietokannat palvelimet manuaalisesti. Nyt emme ole Ajattele. Kaikki on automaattinen – valmistelu uudelleenjärjestäminen-. "

Morten on myös hyvää Azure myöntämä skaalauksen ominaisuuksia. "Joustavasti tietokannan jakavat sopivat sopivaa ratkaistavaan Microsoftin kun SaaS ojentamassa koska emme voi soittaa kapasiteetin ylös ja alas tarpeen mukaan. Valmistelu on helppoa, ja tutustu määritykset on pitää käyttö enintään." Morten kerrotaan "joustavasti jakavat sekä service-taso-pohjainen DTUs assurance helppokäyttöisyyden antaa us potenssiin valmistelu uusi resurssi jakavat pyydettäessä. Hiljattain jokin suurempi asiakkaidensa Harjakatto, 100 DTUs sen live-ympäristössä. Käytä Azure, Microsoftin joustavasti jakavat annettu asiakkaan tietokantojen ja resurssit, joita he reaaliajassa eikä sinun tarvitse ennustaa DTU vaatimukset. Yksinkertaisesti sijoittaa, että asiakkaiden Hanki Ota ympärille aika, joka odotuksia ja suorituskyvyn palvelun tason sopimusten tavata."

Mikkel Madsen laskee: "Microsoft olet embraced tehokkaita Azure algoritmin, joka yhdistää käytetty SaaS vaihtoehto (onboarding uusi asiakkaat reaaliajassa tasolla) Tutustu sovelluksen kuviota (valmistellaan valmiiksi tietokantoja, sekä kehittämisen ja live) päälle (Azure palvelun Bus olevien käyttäminen yhdessä Azure SQL-tietokanta) tekniikkaan."

##<a name="with-azure-uaas-is-exceeding-customer-expectations"></a>Azure, jossa UaaS on suurempi kuin asiakkaan odotuksia

Jälkeen valitsemalla sen pilvikumppani Azure Umbraco on voi antaa UaaS asiakkaiden optimoitu sisällön hallinta-suorituskyky ilman pakollinen itse isännöityä ratkaisusta IT-resurssi sijoitus. Morten lukee, "on perinteisen Wordin kanssa developer helppokäyttöisyys ja skaalattavuus, jotka Azure antavat us ja sen asiakkaiden on ihanaa ominaisuuksiin ja luotettavuutta, että te. Yleistä on hyvä win varten us!"
 
## <a name="more-information"></a>Lisätietoja

- Lisätietoja Azure joustavasti tietokannan jakavat on artikkelissa [joustavasti tietokannan jakavat](sql-database-elastic-pool.md).

- Lisätietoja Azure palvelun Bus on artikkelissa [Azure palvelun Bus](https://azure.microsoft.com/services/service-bus/).

- Lisätietoja verkko-roolit ja työntekijä roolit on artikkelissa [Työntekijä roolit](../fundamentals-introduction-to-azure.md#compute). 

- Lisätietoja virtual verkko-kohdassa [virtual verkko](https://azure.microsoft.com/documentation/services/virtual-network/).    

- Lisätietoja varmuuskopiointi ja palauttaminen on artikkelissa [Liiketoiminnan jatkuvuus](sql-database-business-continuity.md).  

- Lisätietoja ppols seuranta-kohdassa [jakavat seuranta](sql-database-elastic-pool-manage-portal.md). 

- Lisätietoja Umbraco palveluna on artikkelissa [Umbraco](https://umbraco.com/cloud).


<properties
   pageTitle="Cloud Cruiser ja Microsoft Azure Laskutus API integrointi | Microsoft Azure"
   description="On yksilöllinen perspektiivi kumppanilta Microsoft Azure Laskutus Cloud Cruiser-niiden kokemukset Azure Laskutus-ohjelmointirajapinnan integroiminen tuotteesta.  Tämä on erityisen hyödyllinen Azure ja Cloud Cruiser asiakkaat, jotka ovat kiinnostuneita käyttämällä/kokeilemisen Cloud Cruiser Microsoft Azure Pack."
   services=""
   documentationCenter=""
   authors="BryanLa"
   manager="mbaldwin"
   editor=""
   tags="billing"
   />

<tags
   ms.service="billing"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="billing"
   ms.date="09/08/2016"
   ms.author="mobandyo;sirishap;bryanla"/>

# <a name="cloud-cruiser-and-microsoft-azure-billing-api-integration"></a>Cloud Cruiser ja Microsoft Azure Laskutus API-integrointi

Tässä artikkelissa kuvataan, miten uusi Microsoft Azure Laskutus ohjelmointirajapinnan saatuja tietoja voidaan Cloud Cruiser-työnkulun kustannukset simuloinnissa ja analysointia varten.

## <a name="azure-ratecard-api"></a>Azure RateCard Ohjelmointirajapinta
RateCard Ohjelmointirajapinnan tietoja korko azuren. Kun todennustapa kanssa myönnät käyttöoikeudet, voit tehdä kyselyn Ohjelmointirajapinnan kerätä Azure-palvelujen metatietoa sekä korvaukset liittyvät ID: lläsi tarjota.

Seuraavassa on esimerkki vastausta näkyy hinnat A0 API (Windows) esiintymä:

    {
        "MeterId": "0e59ad56-03e5-4c3d-90d4-6670874d7e29",
        "MeterName": "Compute Hours",
        "MeterCategory": "Virtual Machines",
        "MeterSubCategory": "A0 VM (Windows)",
        "Unit": "Hours",
        "MeterRates":
        {
            "0": 0.029
        },
        "EffectiveDate": "2014-08-01T00:00:00Z",
        "IncludedQuantity": 0.0,
        "MeterStatus": "Active"
    },

### <a name="cloud-cruisers-interface-to-azure-ratecard-api"></a>Cloud Cruiser's liittymän Azure RateCard Ohjelmointirajapinta
Cloud Cruiser avulla voidaan hyödyntää RateCard API tietoja eri tavoilla. Tässä artikkelissa kerromme kuinka sen avulla voidaan tehdä IaaS kuormituksen kustannusten simuloinnissa ja analysointia varten.

Tämä Käyttötapaus osoittamaan Kuvitellaan työmäärää eri esiintymien käynnissä Microsoft Azure Pack (WAP). Tavoitteena on simuloida tämän saman työmäärää Azure- ja siitä, kuten siirron kustannusarvioita. Jotta voit luoda tämän simuloinnissa, on kaksi suoritettavat tärkeimmät tehtävät:

1. **Tuonti- ja palvelutiedot kerätty RateCard Ohjelmointirajapinnan.** Tämä tehtävä suoritetaan myös työkirjoja, joissa RateCard Ohjelmointirajapinnan purkaa muunnetaan ja julkaistu uusi korko-palvelupaketti. Tämä uusi korko-suunnitelma käytetään simuloitaessa arvioida Azure hinnat.

2. **Normalisointi WAP services Azure koskevat ja IaaS.** Oletusarvon mukaan WAP services perustuvat perustuu yksittäisten resurssien (suorittimen, muistikoko, levyn koko jne.), kun Azure services esiintymän koon (A0, A1, A2 jne.). Ensimmäinen tehtävä voi maksettavan korvauksen Cloud Cruiser ETL ohjelma, kutsutaan työkirjaa, jossa nämä resurssit voi koota esiintymän kokoja, paperimuotoon Azure esiintymän palvelut.

### <a name="import-data-from-the-ratecard-api"></a>Tietojen tuominen RateCard-Ohjelmointirajapinta

Cloud Cruiser työkirjojen on automaattinen toiminto kerätä ja käsitellä RateCard Ohjelmointirajapinnan tiedot.  ETL (Pura-muunnos-kuormitus) työkirjojen avulla voit määrittää sivustokokoelman, muunnos ja tietojen julkaiseminen Cloud Cruiser-tietokantaan.

Kussakin työkirjassa voi olla yksi tai useita sivustokokoelmat, jonka avulla voi yhdistää tietoja eri lähteistä täydentää tai verkkotunnistetietojen käyttötietojen. Seuraavat kaksi näyttökuvat Näytä uuden *sivustokokoelman* luominen luodun työkirjan ja eikä RateCard API- *sivustokokoelman* tuovan tiedot:

![Kuva 1 – uuden sivustokokoelman luominen][1]

![Kuva 2 - tietojen tuominen uuteen kokoelmaan][2]

Kun tiedot on tuotu työkirjaan, on mahdollista luoda useita vaiheet ja muunnos prosessit, muokata ja mallin tiedot. Tässä esimerkissä koska emme vain kiinnostunut infrastruktuurin nimellä--palvelun (IaaS) Käytämme muunnos ohjeita voit poistaa tarpeettomat rivit tai tietueet, liittyviä palveluja kuin IaaS.

Seuraavassa näyttökuvassa näkyy käyttänyt RateCard API-kerättyjä tietoja käytetään muunnos vaiheet:

![Kuva 3 - muunnos vaiheet RateCard API kerättyjen tietojen käsittelemiseen][3]

### <a name="defining-new-services-and-rate-plans"></a>Uusia palveluja ja korko-Palvelupaketit

Palveluiden määrittäminen Cloud Cruiser eri tavoilla. Jokin vaihtoehdoista on palvelujen tuominen käyttötietojen. Tätä menetelmää käytetään tavallisesti julkisen paveikslėlis, jossa palveluja on jo määritetty tarjoaja käsiteltäessä.

Korko-palvelupaketti on joukko korvaukset tai hinnat, joita voidaan käyttää eri palvelut, joka perustuu voimaantulopäivät tai ryhmän niiden asiakkaiden ja paljon muuta. Korko-suunnitelmien voidaan myös Cloud Cruiser, simuloinnissa tai "entä-jos-skenaarioita services muutokset voivat vaikutus työmäärä kokonaiskustannukset luomiseen.

Tässä esimerkissä Käytämme RateCard Ohjelmointirajapinnan palvelutiedot uusien palveluiden määrittäminen Cloud Cruiser. Samalla tavalla voit luoda uuden korko suunnitteleminen Cloud Cruiser voit Käytämme korvaukset liittyviä palveluja.

Muunnoksen prosessin lopussa on mahdollista uutta vaihetta luominen ja julkaiseminen RateCard Ohjelmointirajapinnan tiedot uusia palveluja ja -korvauksia.

![Kuva 4 - uusien palveluiden ja -korvauksia RateCard Ohjelmointirajapinnan tiedot julkaisemisesta][4]

### <a name="verify-azure-services-and-rates"></a>Tarkista Azure palveluiden ja -korvauksia

Julkaisemisen palveluiden ja -korvauksia, jälkeen voit tarkistaa tuodut palveluluetteloon Cloud Cruiser *palvelut* -välilehdessä:

![Kuva 5 – tarkistetaan uusia palveluja][5]

*Korko-Palvelupaketit* -välilehdessä voit tarkistaa nimeltä "AzureSimulation" kursseista kanssa RateCard Ohjelmointirajapinnan tuoduista uuden korko palvelupaketin.

![Kuva 6 - tarkistetaan uusi korko suunnitteleminen ja liittyvät korvaukset][6]

### <a name="normalize-wap-and-azure-services"></a>Normalisointi WAP ja Azure palvelut

Oletusarvon mukaan WAP on käyttötietojen suorittaminen, muistin ja verkkoresursseja perusteella. Voit määrittää Cloud Cruiser-pohjaisten palvelujen suoraan-kohdistus tai laskutettavan näiden resurssien käyttö. Voit esimerkiksi määrittää basic korvaus tunnin välein suorittimen käyttö tai veloittaa varattu erillisen Gigatavua.

Esimerkiksi jotta voit verrata kustannuksia WAP ja Azure-annettava koostaa resurssien käyttö-WAP kyselyjä nippujen, jotka voidaan linkittää sitten Azure-palveluihin. Muunnos voidaan toteuttaa helposti työkirjoissa:

![Kuva 7 – muuntaa WAP tietoja normitettava palvelut][7]

Työkirjan, viimeinen vaihe on julkaista tiedot Cloud Cruiser-tietokantaan. Tässä vaiheessa käyttötietojen on nyt koottu services (eli yhdistää Azure-palvelut) ja yhdistetty oletusarvon korvaukset maksut luomiseen.

Työkirjan jälkeiset voit automatisoida tietojen käsittely lisäämällä tehtävän Scheduler-ja taajuus-ja suorita työkirjan aika.

![Kuva 8 – työkirjan ajoitus][8]

### <a name="create-reports-for-workload-cost-simulation-analysis"></a>Raporttien luominen työmäärää kustannusten simuloinnissa analysointia varten

Kun käyttö kerätään ja kulujen ladataan Cloud Cruiser tietokantaan, emme voidaan hyödyntää Cloud Cruiser havainnollistamisen moduulin luominen kustannukset, jotka haluavat olemme simuloinnissa työmäärää.

Kuvataan Tämä skenaario, jotta voit luoda seuraavan raportin:

![Kustannusten vertailu][9]

Ylimmät kaavio näyttää kustannukset vertailu-palveluissa, vertailu hinnan, joissa kunkin tietyn palvelun työmäärää WAP (tummansinisinä) ja Azure (Vaaleansininen) välillä.

Ala-kaavio näyttää samat tiedot mutta osaston mukaan. Näyttää niiden työmäärää toimimaan WAP ja Azure, sekä niiden säästöjen palkin (vihreä) välisen eron osastokohtaisia kustannuksiin.

## <a name="azure-usage-api"></a>Azure käyttö Ohjelmointirajapinta


### <a name="introduction"></a>Johdanto

Microsoft käyttöön viimeksi Azure käyttö API salliminen tilaajille hakemaan ohjelmallisesti käyttötiedot lisäämään tietoja niiden kulutus kyselyjä. Tämä on hyvä uutiset Cloud Cruiser asiakkaat, joilla voit hyödyntää tämän API kautta tehokkaan tietojoukko.

Cloud Cruiser voidaan hyödyntää usealla eri tavalla Ohjelmointirajapinnan käyttö integrointi. Rakeisuuden (kerran tunnissa käyttötietojen) ja resurssin metatietoja Ohjelmointirajapinnan kautta on tarvittavat tietojoukko tukemaan joustavia Showback tai palautus malleja. 

Tässä opetusohjelmassa on esittää esimerkki siitä, miten Cloud Cruiser voi hyötyä käyttö API-tiedot. Tarkemmin sanottuna on luoda resurssiryhmä Azure, liittää tunnisteet tilin rakenne ja valitse kuvaavat tietojen ja käsittelyn Cloud Cruiser tunnistetiedot.
 
Lopullisen tavoitteen on voivat luoda raportteja, kuten yksi sekä voi analysoida kustannus- ja kulutus mukaan tilirakenteen tunnisteet täyttänyt.

![Kuva 10 - raportti, jonka erittelyä ohjauskoodien käyttäminen][10]

### <a name="microsoft-azure-tags"></a>Microsoft Azure-tunnisteet

Azure käyttö Ohjelmointirajapinnan kautta tiedot sisältävät kulutus tietojen lisäksi myös resurssin metatietoja tunnisteiden liittyy. Tunnisteiden avulla on helppo järjestää resurssit, mutta tehokkaita, sinun on varmistettava, että:

- Tunnisteiden oikein käytetään resursseja säännöstä milloin
- Tunnisteita käytetään oikein Showback/palautus-prosessi ja useiden käyttö organisaation tili rakenteeseen.

Nämä molemmat vaatimukset voi olla hankalaa, erityisesti silloin, kun kyseessä on manuaalisen prosessin toimittamista tai charging side. Väärin, virheellinen tai jopa puuttuvat tunnisteet ovat yleisiä asiakkaiden valituksia, kun tunnisteiden ja virheet voi helpottaa lataamiseen reunassa hankalaa.

Uusi Azure käyttö API-Cloud Cruiser voi tuoda resurssin tunnisteiden tiedot ja kautta kehittyneitä ETL työkalua kutsutaan työkirjoja, korjata yleisiä tunnisteiden virheet. Kautta muunnoksen säännöllisiä lausekkeita ja tietojen korrelaatio-Cloud Cruiser voi tunnistaa virheellisesti merkittynä resurssit ja käytä oikea tunnisteet varmistaa, että oikea liitoksen kuluttaja resursseista.

Lataamiseen-puolella Cloud Cruiser Showback/palautus automatisoi ja voidaan hyödyntää tunnistetiedot ja useiden käyttö tarvittavat kuluttajille (osasto, osasto, projektin jne.). Tämä automaatio on erittäin suuri parantaminen ja varmistaa, että yhdenmukaisia ja valvottavat lataamiseen prosessi.
 

### <a name="creating-a-resource-group-with-tags-on-microsoft-azure"></a>Resurssiryhmä luominen Microsoft Azure tunnisteiden avulla
Tässä opetusohjelmassa ensimmäiseksi on luotava resurssiryhmä Azure-portaalissa luomalla uusia tunnisteita liitettäväksi resurssit. Tässä esimerkissä on luovatko seuraavat tunnisteet: osasto-ympäristössä, omistaja, projektin.

Seuraavassa näyttökuvassa näkyy otoksen resurssiryhmä liittyvien tunnisteiden avulla.

![Kuva 11 - resurssiryhmä, jossa liittyvistä tunnisteista Azure-portaalissa][11]

Seuraavaksi hakemaan tiedot käyttö Ohjelmointirajapinnan Cloud Cruiser. Käyttö-Ohjelmointirajapinnan on tällä hetkellä vastaukset JSON-muodossa. Tässä on esimerkki noutaa tiedot:


    {
      "id": "/subscriptions/bb678b04-0e48-4b44-XXXX-XXXXXXX/providers/Microsoft.Commerce/UsageAggregates/Daily_BRSDT_20150623_0000",
      "name": "Daily_BRSDT_20150623_0000",
      "type": "Microsoft.Commerce/UsageAggregate",
      "properties":
      {
        "subscriptionId": "bb678b04-0e48-4b44-XXXX-XXXXXXXXX",
        "usageStartTime": "2015-06-22T00:00:00+00:00",
        "usageEndTime": "2015-06-23T00:00:00+00:00",
        "meterName": "Compute Hours",
        "meterRegion": "",
        "meterCategory": "Virtual Machines",
        "meterSubCategory": "Standard_D1 VM (Non-Windows)",
        "unit": "Hours",
        "instanceData": "{\"Microsoft.Resources\":{\"resourceUri\":\"/subscriptions/bb678b04-0e48-4b44-XXXX-XXXXXXXX/resourceGroups/DEMOUSAGEAPI/providers/Microsoft.Compute/virtualMachines/MyDockerVM\",\"location\":\"eastus\",\"tags\":{\"Department\":\"Sales\",\"Project\":\"Demo Usage API\",\"Environment\":\"Test\",\"Owner\":\"RSE\"},\"additionalInfo\":{\"ImageType\":\"Canonical\",\"ServiceType\":\"Standard_D1\"}}}",
        "meterId": "e60caf26-9ba0-413d-a422-6141f58081d6",
        "infoFields": {},
        "quantity": 8

      },
    },


### <a name="import-data-from-the-usage-api-into-cloud-cruiser"></a>Tietojen tuominen Cloud Cruiser Ohjelmointirajapinnan käyttö

Cloud Cruiser työkirjojen on automaattinen toiminto kerätä ja käsitellä tietoja Ohjelmointirajapinnan käyttö. Työkirjaksi ETL (Pura-muunnos-kuormitus) voit määrittää sivustokokoelman, muunnos ja tietojen julkaiseminen Cloud Cruiser-tietokantaan.

Kussakin työkirjassa voi olla yksi tai useita sivustokokoelmat. Voit yhdistää tietoja eri lähteistä täydentää tai verkkotunnistetietojen käyttötietojen. Tässä esimerkissä luodaan uusi taulukko työkirjassa Azuren mallin (_UsageAPI)_ ja määritä uuden _sivustokokoelman_ tietojen tuomisesta Ohjelmointirajapinnan käyttö.

![Kuva 3 - Ohjelmointirajapinnan käyttötiedot on tuotu UsageAPI-taulukko][12]

Huomaa, että tämä työkirja on jo muihin taulukoihin palvelujen tuominen Azure (_ImportServices_) ja käsitellä kulutus tiedot Laskutus-Ohjelmointirajapinnan (_PublishData_).

Olemme seuraava käyttää Ohjelmointirajapinnan käyttö _UsageAPI_ -taulukon tietojen täyttämiseen, ja yhdistää tietoja Laskutus-Ohjelmointirajapinnan kulutus tiedoilla _PublishData_ taulukon.

### <a name="processing-the-tag-information-from-the-usage-api"></a>Käyttö-Ohjelmointirajapinnan tunnistetiedot käsittely

Kun tiedot on tuotu työkirjaan, luodaan muunnos vaiheet _UsageAPI_ taulukon käsittelemiseen Ohjelmointirajapinnan tiedot. Ensimmäiseksi on "JSON Jaa" suoritin avulla voit poimia tunnisteet yksittäiseen kenttään ja valitse sitten Luo kentät jokaiselle ne (osasto, Project, omistaja ja ympäristön).

![Kuva 4 – Luo uusia kenttiä tunnistetiedot][13]

Ilmoitus "verkko-palvelun puuttuu tunnistetiedot (keltaisen ruudun), mutta on Tarkista, että se on osa samaan resurssiryhmä katsomalla _ResourceGroupName_ -kenttä. Koska on tämän resurssin-ryhmästä resurssien tunnisteita, on tämän toiminnon avulla puuttuvat tunnisteen käyttäminen resurssi myöhemmässä vaiheessa.

Seuraavaksi Luo liittämisestä tunnisteet tiedoista _ResourceGroupName_hakutaulukko. Tämä hakutaulukko käytetään seuraavaksi voit täydentää tunnistetiedot kulutus tiedot.

### <a name="adding-the-tag-information-to-the-consumption-data"></a>Tunniste-tietojen lisääminen kulutustiedot

Nyt voimme siirtyä _PublishData_ -taulukko, joka käsittelee Laskutus-Ohjelmointirajapinnan kulutus tiedot, ja tunnisteet poimittujen kenttien lisääminen. Tämä toimenpide suoritetaan katsomalla käyttämällä _ResourceGroupName_ hakujen avaimeksi edellisessä vaiheessa luotu hakutaulukkoon.

![Kuva 5 – täyttää hakujen tiedoilla tilin-rakenne][14]

Huomaa, että "verkko-palvelun tilin rakenne-kentissä on tehty, ongelman korjaaminen puuttuvat tunnisteet kanssa. On myös kaikille resurssien tilin rakenne-kentät kuin Microsoftin kohde resurssiryhmä ja "Muut", jotta voidaan erottaa toisistaan raporteissa.

Nyt vain annettava Lisää käyttötietojen julkaisemaan. Tässä vaiheessa kunkin palvelun määritetty Rate meidän suunnitteleminen asianmukaiset korvaukset käytetään käyttötietojen, ja tuloksena kulun ladattu tietokantaan.

Paras osa on, että vain tarvitse käydä läpi prosessia kerran. Työkirja on valmis, kun haluat lisätä Scheduler ja se suoritetaan hourly tai päivittäin määritettynä ajankohtana. Valitse on vain ohjeisiin ja uusien raporttien luominen tai muokkaamalla aiemmin luotuja, jotta kuvaava havainnollistamisen käyttämistä cloud käyttö tietojen analysointiin.

### <a name="next-steps"></a>Seuraavat vaiheet

+ Lisätietoja Cloud Cruiser työkirjat ja raporttien luomisesta on on lisätietoja Cloud Cruiser online [asiakirjat](http://docs.cloudcruiser.com/) (kelvollinen Kirjautuminen vaaditaan).  Lisätietoja Cloud Cruiser, ota yhteyttä [info@cloudcruiser.com](mailto:info@cloudcruiser.com).
+ Katso yleiskuvaus Azure resurssien käyttö- ja RateCard API [Hanki tuominen Microsoft Azure-resurssin kulutus tietoja](billing-usage-rate-card-overview.md) .
+ Katso lisätietoja sekä API, jotka ovat osa ohjelmointirajapinnan Azure resurssin Manager-ohjelman tuottamien joukko [Azure Laskutus REST API viittaus](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) .
+ Jos haluat hallitsee malli-koodiksi, katso Tutustu Microsoft Azure Laskutus API MALLIKOODEJA [Azure MALLIKOODEJA](https://azure.microsoft.com/documentation/samples/?term=billing)käyttöön.

### <a name="learn-more"></a>Opi lisää
+ [Azure resurssin hallinnassa: yleiskatsaus](azure-resource-manager/resource-group-overview.md) on artikkelissa lisätietoja tietoja Azure Resurssienhallinta.

<!--Image references-->
 
[1]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Create-New-Workbook-Collection.png "Kuva 1 – uuden sivustokokoelman luominen"
[2]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Import-Data-From-RateCard.png "Kuva 2 - tietojen tuominen uuteen kokoelmaan"
[3]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Transformation-Steps-Process-RateCard-Data.png "Kuva 3 - muunnos vaiheet RateCard API kerättyjen tietojen käsittelemiseen"
[4]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Publish-RateCard-Data-New-Services-Rates.png "Kuva 4 - uusien palveluiden ja -korvauksia RateCard Ohjelmointirajapinnan tiedot julkaisemisesta"
[5]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Verify-Azure-Services-And-Pricing1.png "Kuva 5 – tarkistetaan uusia palveluja"
[6]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Verify-Azure-Services-And-Pricing2.png "Kuva 6 - tarkistetaan uusi korko suunnitteleminen ja liittyvät korvaukset"
[7]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Transforming-WAP-Normalize-Services.png "Kuva 7 – muuntaa WAP tietoja normitettava palvelut"
[8]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Workbook-Scheduling.png "Kuva 8 – työkirjan ajoitus"
[9]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Workload-Cost-Simulation-Report.png "Kuva 9 – työmäärää kustannukset vertailu-skenaario esimerkkiraportti"
[10]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/1_ReportWithTags.png "Kuva 10 - raportti, jonka erittelyä ohjauskoodien käyttäminen"
[11]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/2_ResourceGroupsWithTags.png "Kuva 11 - resurssiryhmä, jossa liittyvistä tunnisteista Azure-portaalissa"
[12]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/3_ImportIntoUsageAPISheet.png "Kuva 12 - Ohjelmointirajapinnan käyttötiedot on tuotu UsageAPI-taulukko"
[13]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/4_NewTagField.png "Kuva 13 – Luo uusia kenttiä tunnistetiedot"
[14]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/5_PopulateAccountStructure.png "Kuva 14 - täyttää hakujen tiedoilla tilin-rakenne"

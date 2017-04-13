<properties
   pageTitle="Hanki tietoja tuominen Microsoft Azure-resurssin kulutus | Microsoft Azure"
   description="Tässä artikkelissa Azure Laskutus käyttö ja RateCard ohjelmointirajapinnan, joita käytetään antamaan havainnollistamisen Azure resurssin kulutus ja trendien Käsitteellinen yleiskatsaus."
   services=""
   documentationCenter=""
   authors="BryanLa"
   manager="mbaldwin"
   editor=""
   tags="billing"/>

<tags
   ms.service="billing"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="billing"
   ms.date="08/16/2016"
   ms.author="mobandyo;bryanla"/>

# <a name="gain-insights-into-your-microsoft-azure-resource-consumption"></a>Hanki tietoja tuominen Microsoft Azure-resurssin kulutus

Asiakkaiden ja kumppaneiden vaatii tarkasti ennustaa ja niiden Azure kustannusten hallinta.  Kun ne siirtyvät Capex Opex mallia, he tarvitsevat voi tehdä showback ja palautus analyysi sekä tarjota tilassa tarkkuuden arvio ja laskutus-erityisesti suuri cloud käyttöönottoa varten.

Azure resurssien käyttö- ja korko kortin ohjelmointirajapinnan käsitellään tässä artikkelissa osoitteessa nämä tarpeet ottamalla uusia johtopäätöksiä oman kulutus Azure resurssien kyselyjä.  

## <a name="introducing-the-azure-resource-usage-and-ratecard-apis"></a>Azure resurssien käyttö- ja RateCard API esittely

Azure resurssien käyttö- ja RateCard ohjelmointirajapinnan on toteutettu resurssin toimittaja-ohjelmointirajapinnan tarjoamia mukaan Azure Resurssienhallinta perheen osana.  

### <a name="azure-resource-usage-api-preview"></a>Azure resurssien käyttö API (ennakkoversio)
Asiakkaiden ja kumppaneiden käyttää Azure resurssien käyttö-Ohjelmointirajapinnan arvioitu Azure kulutus tietonsa avulla. Ominaisuuksia:

- **Azure Roolipohjainen käyttöoikeuksien valvonta** - asiakkaat ja kumppanit voit määrittää niiden käytäntöjen [Azure portal](https://portal.azure.com) -tai – Määritä, ketkä käyttäjät tai sovellukset hankkia tilauksen käyttötietojen käytön [Azure PowerShellin cmdlet-komennot](powershell-install-configure.md) . Soittajat käytettävä vakio Azure Active Directory-tunnusten todennusta varten. Soittajan on myös lisättävä joko lukijan, omistaja tai osallistujan rooli, jotta voit avata käyttötietojen Azure tilauksessa.

- **Hourly tai päivittäinen koosteet** - soittajat voit määrittää ne haluat niiden Azure käyttötietoja tunnin välein uudelleenkäytettävät tai päivittäinen uudelleenkäytettävät. Oletusarvo on päivittäin.

- **Tuottamien esiintymän metatietojen (sisältää resurssin tunnisteet)** – esiintymän tason tiedot, kuten täydellinen resurssin uri (/subscriptions/ {Tilaustunnus} / …), sekä resurssin ryhmän tiedot ja resurssin tunnisteet säädetty vastaus. Tämä auttaa asiakkaita deterministically ja ohjelmallisesti varata käyttö tunnisteilla, käytä tapauksissa, kuten rajat lataudu.

- **Resurssin metatietoja annettu** - resurssitiedot, kuten mittarin nimi, mittarin luokka, mittarin sub-luokka, yksikkö ja alueen myös välitetään vastauksen, anna soittajat tietoja, mikä on käytetty paremmin. Myös Yritämme käyttämällä voi kohdistaa resurssin metatietojen termeistä Azure-portaalissa Azure käyttö CSV-EA Laskutus CSV ja julkisen muissa ohjelmissa, jotta asiakkaat voivat yhdistää tietoja eri kokemukset.

- **Tarjouksen tyypeissä käyttö** – käyttötiedot voi käyttää, jos kaikki tarjota mukaan lukien ryhmävakuutussopimukset, MSDN, raha sitoumus, raha luottotietojen ja EA muun muassa.

### <a name="azure-resource-ratecard-api-preview"></a>Azure resurssin RateCard API (ennakkoversio)
Asiakkaiden ja kumppaneiden käyttää Azure resurssin RateCard Ohjelmointirajapinnan avulla käytettävissä Azure resurssien luetteloa mukana arvioitu hintatiedot kunkin. Ominaisuuksia:

- **Azure Roolipohjainen käyttöoikeuksien valvonta** - asiakkaat ja kumppanit voit määrittää niiden käytäntöjen [Azure portal](https://portal.azure.com) -tai – Määritä, ketkä käyttäjät tai sovellukset hankkia RateCard tietoihin pääsy [Azure PowerShellin cmdlet-komennot](powershell-install-configure.md) . Soittajat käytettävä vakio Azure Active Directory-tunnusten todennusta varten. Soittajan on myös lisättävä joko lukijan, omistaja tai osallistujan rooli, jotta voit avata käyttötietojen Azure tilauksessa.

- **Tuki ryhmävakuutussopimukset, MSDN, raha sitoumus ja raha luottotietojen tarjoaa (EA ei tueta)** – tämä Ohjelmointirajapinnan on Azure tarjouksen tason korko tietoa, ja tilauksen tason.  Tämä API kutsujan on välitettävä tarjouksen tiedot resurssitiedot ja korvaukset.  Kuin EA tarjouksia mukautettuja rekisteröinti hinnat, emme pysty esittämään EA korvaukset tällä hetkellä.

## <a name="scenarios"></a>Skenaariot

Seuraavassa on kuvattu joitakin tilanteita, jotka tehdään mahdollisuuksiin käyttö ja RateCard-ohjelmointirajapinnan yhdistelmä:

- **Kuukauden aikana käyttävät azure** - asiakkaat voivat käyttää käyttö ja RateCard API yhdessä saat paremman havainnollistamisen kyselyjä niiden cloud käyttävät kuukauden aikana analysoiminen käyttö- ja maksu arvioiden tunnin välein ja päivittäisten uudelleenkäytettävät mukaan.

- **Määritä ilmoitukset** – asiakkaat ja kumppanit määrittää niiden cloud kulutus resurssi- tai raha-pohjaisia ilmoitukset hakemalla arvioitu kulutus ja kulujen arvion käyttö ja RateCard Ohjelmointirajapinnan käyttäminen.

- **Predict laskun** – asiakkaat ja kumppanit voivat siirtyä hänen arvioitu kulutus ja cloud käyttävät ja käyttää tietokoneen learning algoritmit ennustaa mitä niiden laskun olisi valitun laskutusjakson lopussa.

- **Vanhat kulutus kustannukset-analyysi** – asiakkaat voivat myös Käytä RateCard Ohjelmointirajapinnan ennustaa paljonko niiden laskun olisi olisivat voit siirtyä niiden työmääriä Azure-by antamalla toivottuja käyttö numeroita. Jos asiakkaita on aiemmin työmääriä muissa paveikslėlis tai yksityinen paveikslėlis, he myös yhdistää niiden käyttöä Azure korvaukset saat paremman arvio niiden arvioitu Azure käyttävät. Tämä on parannettu näkymä, mikä voi saada [Azure hinnoittelu Laskimen](https://azure.microsoft.com/pricing/calculator/)kautta (esimerkiksi) Laskutus kumppaneita on pivot-tarjous ja vertaa ja kontrasti eri tarjouksen eri lisäksi ryhmävakuutussopimukset, mukaan lukien raha sitoumus ja raha luottotietojen välillä. API tarjoavat myös mahdollisuus kustannusten arviointi muutoksista alueittain, ottaminen käyttöön tarvitaan käyttöönoton päätösten, kun eri puolilla maailmaa ohjauskoneita käyttöönottaminen resursseja voi olla suora vaikutus kokonaiskustannukset entä jos-analyysin tyyppi.

- **Entä jos-analyysi** -

    - Asiakkaiden ja kumppaneiden selville, onko olisi edullisempaa toisen alueen tai toisen määritysten Azure resurssin niiden työmääriä suorittamiseen. Azure resurssien kustannukset saattavat vaihdella perusteella Azure alue, jossa ne ovat käytössä ja näin saat kustannukset optimointi asiakkaat ja kumppanit.

    - Asiakkaiden ja kumppaneiden voit myös määrittää Jos Azure tarjouksen jonkin muun antaa parempia korvaus Azure resurssi.

## <a name="partner-solutions"></a>Kumppaniratkaisuihin

[Microsoft Azure käyttö- ja RateCard ohjelmointirajapinnan käyttöön Cloudyn voit antaa ITFM asiakkaiden](billing-usage-rate-card-partner-solution-cloudyn.md) kuvataan Azure Laskutus API kumppanin [Cloudyn](https://www.cloudyn.com/microsoft-azure/)tarjoamia integrointi kokemus.  Tässä artikkelissa on yksityiskohtaiset soveltamisala niiden-ohjelmissa, mukaan lukien lyhyt video, jossa näytetään, miten Azure asiakas voi käyttää Cloudyn ja Azure Laskutus-ohjelmointirajapinnan voitot havainnollistamisen Azure kulutus niiden tiedoista.

[Cloud Cruiser ja Microsoft Azure Laskutus API integroinnin](billing-usage-rate-card-partner-solution-cloudcruiser.md) kuvataan, miten [Cloud Cruiser Express Azure Pack](http://www.cloudcruiser.com/partners/microsoft/) toimii suoraan WAP-portaaliin, ottaminen käyttöön asiakkaat voivat hallita saumattomasti yhteen käyttöliittymä niiden Microsoft Azure yksityinen tai isännöity julkinen cloud toiminta- ja ominaisuuksia.   

## <a name="next-steps"></a>Seuraavat vaiheet
+ Katso lisätietoja sekä API, jotka ovat osa ohjelmointirajapinnan Azure resurssin Manager-ohjelman tuottamien joukko [Azure Laskutus REST API viittaus](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) .
+ Jos haluat hallitsee malli-koodiksi, katso Tutustu Microsoft Azure Laskutus API MALLIKOODEJA [Azure MALLIKOODEJA](https://azure.microsoft.com/documentation/samples/?term=billing)käyttöön.

## <a name="learn-more"></a>Opi lisää
+ [Azure resurssin hallinnassa: yleiskatsaus](azure-resource-manager/resource-group-overview.md) on artikkelissa lisätietoja tietoja Azure Resurssienhallinta.
+ Saat apua kasvun hallitsemista cloud tarvittavat työkalut Suite käyttävät, lue lisätietoja artikkelista Gartner [Markkina-opas oppilaitosten IT taloudellisen tilanteen Management (ITFM)-työkalut](http://www.gartner.com/technology/reprints.do?id=1-212F7AL&ct=140909&st=sb).

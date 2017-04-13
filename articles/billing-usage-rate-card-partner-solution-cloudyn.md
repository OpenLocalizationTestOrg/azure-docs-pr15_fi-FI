<properties
   pageTitle="Microsoft Azure käyttö- ja RateCard ohjelmointirajapinnan käyttöön Cloudyn ITFM tarjoaminen asiakkaille | Microsoft Azure"
   description="On yksilöllinen perspektiivi kumppanilta Microsoft Azure Laskutus Cloudyn, valitse niiden kokemukset Azure Laskutus-ohjelmointirajapinnan integroiminen niiden tuotteen.  Tämä on erityisen hyödyllisiä Azure ja Cloudyn asiakkaat, jotka ovat kiinnostuneita käyttämällä/kokeilemisen Cloudyn Azure-palvelut."
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

# <a name="microsoft-azure-usage-and-ratecard-apis-enable-cloudyn-to-provide-itfm-for-customers"></a>Microsoft Azure käyttö ja RateCard ohjelmointirajapinnan käyttöön Cloudyn ITFM tarjoaminen asiakkaille

Uusi Microsoft Azure resurssien käyttö- ja RateCard API yksityinen esikatsella valittiin Cloudyn ja kumppanin kehittäminen Microsoft cloud hallintatoiminnot johtavan.  Käyttö-Ohjelmointirajapinnan on arvioitu Azure kulutus tietojen käytön tilauksen. RateCard-Ohjelmointirajapinnan on valmis hintatiedot kaikki Azure palvelujen ei - Enterprise Agreement EA asiakkaille. Integroitu yhdessä, nämä API perusta on suoritettu IT taloudellisen tilanteen Management (ITFM)-työkaluja, kuten Cloudyn tulostusvaihtoehtoja-syötteestä.

## <a name="introduction"></a>Johdanto

Tiedoilla RateCard Ohjelmointirajapinnan käyttö Ohjelmointirajapinnan tiedot niin kutsutut "kertolaskun" (hinta käyttö [yksiköt] [$unit] = yksityiskohtaiset käyttö- ja kustannukset) luo eniten hajautetun, tarkkoja ja luotettavia laskutustiedot käytettävissä Azure tänään.

![ITFM yleiskatsaus][1]

Nämä ohjelmointirajapinnan käyttö tarjoaa avaintiedoista asiakkaiden käyttö- ja kustannukset, sallimisen Cloudyn, jos haluat analysoida asiakas yksinkertainen, ohjelmallisesti tavalla ja sen asiakkaiden eri ITFM tehtävien suorittamiseen.

## <a name="integrating-cloudyn-with-the-ratecard-and-usage-apis"></a>Cloudyn integrointi RateCard ja ohjelmointirajapinnan käyttö
RateCard Ohjelmointirajapinnan edellyttää useita syöteparametrit – kuten alueen tiedot, valuutan ja aluekohtaiset asetukset – mutta tärkeimmät on OfferDurableID, joka määrittää Azure tarjoaminen asiakkaalle tyyppi on käytössä (ryhmävakuutussopimukset, 6 ja 12 kuukauden vanhoja sitoumus suunnitelmat, MSDN tarjouksia, MPN tarjouksia, erikoistarjoukset ja muuta). OfferDurableID löytyy [Azure käyttö- ja portaalissa Laskutus](https://account.windowsazure.com/Subscriptions)-kohdassa "Tarjota tunnuksen" annetun tilaus.

Asiakkaiden lisätä rekisteröinnin [Cloudyn Azure](https://www.cloudyn.com/microsoft-azure/) Services, niiden OfferDurableID-koodin, joka sallii Cloudyn hakemaan niiden asiaa hintatiedot RateCard API-Liittymän kautta.  Tietoja erityyppisistä tarjouksia voi löysi vähintään yhden [Microsoft Azure tarjouksen tiedot](https://azure.microsoft.com/support/legal/offer-details/) -sivu.

![Cloudyn ITFM ohjelma yleiskatsaus][2]

Cloudyn käyttää sekä käyttö ja RateCard ohjelmointirajapinnan lisäksi Azure suorituskyvyn API, Luo muita kerrokset visualisoinnin, analytics, ilmoitat, raportointi, kustannusten hallinnan ja yhtenäisen suosituksia tarjoaa Azure asiakkaiden luotettava yrityksen cloud ITFM-työkalu.

## <a name="cloudyn-itfm-use-cases-enabled-by-usage-and-ratecard-api-integration"></a>Cloudyn ITFM käyttötapauksiin käyttöön käyttö- ja RateCard API-integrointi
Yleisiä Cloudyn ITFM käyttötapauksiin käyttöön käyttö ja RateCard API sisältää:

+ **Kustannus-analyysi** - sallii cloud kustannukset katketa alaspäin kaikki alkuperäisen yksilöivä dimensio (provider, palvelu, tili, alue jne.). Azure käyttö ja RateCard ohjelmointirajapinnan tehdä helppoa, antamalla käyttö eniten hajautetun hajautuksen tai kustannustietojen tilille, joka on sitten ryhmitetty Cloudyn mukaan suodatettu ja esittää käyttäjälle, kuvan tai taulukko lomakkeen kohden.

![Kustannusten analyysin Ympyräkaavio][3]

+ **Kustannusten kohdistus 360** - mahdollistaa talous ja IT-esimiehille paljastaa todelliset kustannukset breakdown ja ohjaimet trendejä cloud niiden käyttöönottoa. Se sallii edelleen projektipäälliköt voivat yhdistää helposti käyttöönoton kulujen liiketoimintayksiköitä, osastot, alueet ja lisää tarjoaa ennennäkemättömät havainnollistamisen kyselyjä cloud kustannuksia ja yrityksen palautus ja showbacks. Azure käyttö ja RateCard ohjelmointirajapinnan yhteyshenkilönä syötteeksi Cloudyn's kustannukset kohdistus hakukone, joka täydentää API määrittämällä menetelmistä ja liiketoimintalogiikan untagged tai untaggable resurssien varaamisen.

![Kustannusten kohdistamisen 360 kaavio][4]

+ **Cost-Effective koko** - ja neuvoo oikealla olevaa underutilized näennäiskoneiden, mikä pienentää liian suuri tai perusteettomasti valmistellun asiakkaan kuluja. Se tekee tarkastamalla virtuaalikoneen suorittimen ja RAM-Muistia mittarit (joko suorituskyky-API) tuntia suorituksenaikainen (joko käyttö API) ja kustannukset (joko RateCard API). Cloudyn sitten oikealle koon ja neuvoo underutilized suorittimen tai RAM-Muistia resurssit (suorituskyky) perusteella, ja laskee arvioitu säästöjen kertomalla hinnan delta (RateCard) välillä VMs todellinen aika-käyttö (käyttö) underutilized koneen perusteella.

![Edullinen koon muuttaminen][5]

+ **Cloud sovittaminen suositukset** - tarjoaa taloudellisen tilanteen neuvoja cloud sovittaminen. Käyttäjän nykyinen kustannukset cloud varat, jotka on otettu käyttöön pää cloud toimittajien tutkii ja vertaa sitä vastaava käyttöönoton Azure-kustannukset. Se sitten tarjoaa hajautetun, kohti kuin resursseja, kirjanpidossa perustuva sovittaminen Azure suositukset. Cloudyn käyttää arvioidaan käytettävä Azure (perustuu suorituskyvyn mittarit ja käyttäjäasetuksia) vastaavan käyttöönoton jälkeen RateCard API vastaavat deployment Azure-kustannusten arvioiminen.

+ **Raportteja** - käyttöön Azure's suorituskyvyn API, näissä raporteissa on matriisin suorittimen ja RAM-Muistia käyttö optimointi suositukset-toimintoja. Alla on esiintymän käyttö raportin Esimerkki esittäminen esiintymän breakdown keskimääräinen suorittimen käytön mukaan.

![Raportteja][6]

+ **Luokan hallinta** - ominaisuus, joka tehokkaita Cloudyn, joka yhdistää tilauksen unorganized cloud resurssit. Sen avulla käyttäjät voivat luoda omia yksilöllisiä luokat (tunnisteiden) tehokkaita mittaaminen ja raportointi luvun, joka on tasossa yrityksen käytäntöjä. Lisäksi käyttäjät voivat helposti sääteleviä ja luokittelu epäyhtenäinen tunnisteita (eli kirjoitusvirheitä ja muut) ja automaattinen haku tarkkoja kustannukset attribution untagged resursseja.

![Luokan hallinta][7]

## <a name="video"></a>Video

Seuraavassa on lyhyt video, jossa näytetään, miten Azure asiakas käyttää Cloudyn Azure ja ohjelmointirajapinnan Azure laskutus ja myönnä havainnollistamisen Azure kulutus niiden tiedoista.

> [AZURE.VIDEO cloudyn-provides-cloud-itfm-tools-via-microsoft-azure-apis]


## <a name="next-steps"></a>Seuraavat vaiheet

+ Aloita [Cloudyn Azure](https://www.cloudyn.com/microsoft-azure/) maksuttoman kokeiluversion nähdäksesi, kuinka voit hankkia kustannukset läpinäkyvyyttä mukautetun raportoinnin ja analysoinnin Microsoft Azure-pilvi käyttöönottoa varten.
+ Katso yleiskuvaus Azure resurssien käyttö- ja RateCard API [Hanki tuominen Microsoft Azure-resurssin kulutus tietoja](billing-usage-rate-card-overview.md) .
+ Katso lisätietoja sekä API, jotka ovat osa ohjelmointirajapinnan Azure resurssin Manager-ohjelman tuottamien joukko [Azure Laskutus REST API viittaus](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) .
+ Jos haluat hallitsee malli-koodiksi, katso Tutustu Microsoft Azure Laskutus API MALLIKOODEJA [Azure MALLIKOODEJA](https://azure.microsoft.com/documentation/samples/?term=billing)käyttöön.

## <a name="learn-more"></a>Opi lisää
+ Lisätietoja Microsoft Azure Enterprise Agreement (EA) tarjouksia käy [yrityksen käyttöoikeudet Azure] (https://azure.microsoft.com/pricing/enterprise-agreement/)
+ [Azure resurssin hallinnassa: yleiskatsaus](azure-resource-manager/resource-group-overview.md) on artikkelissa lisätietoja tietoja Azure Resurssienhallinta.
+ Saat apua kasvun hallitsemista cloud tarvittavat työkalut Suite käyttävät, lue lisätietoja artikkelista Gartner [Markkina-opas oppilaitosten IT taloudellisen tilanteen Management (ITFM)-työkalut](http://www.gartner.com/technology/reprints.do?id=1-212F7AL&ct=140909&st=sb).

<!--Image references-->
[1]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-ITFM-Overview.png
[2]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-ITFM-Engine-Overview.png
[3]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Cost-Analysis-Pie-Chart.png
[4]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Cost-Allocation-360-Chart.png
[5]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Cost-Effective-Sizing.png
[6]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Performance-Reports.png
[7]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Category-Manager.png

Tietoja factory on usean vuokraajan-palvelu, joka on oletusarvoinen seuraavia rajoituksia paikassa, varmista, että asiakkaiden tilaukset suojauksessa toistensa toiminnoista. Monia rajoituksia voit helposti nostaa ylöspäin enimmäisrajan tilauksen ottamalla yhteyttä tukipalveluun. 

**Resurssi** | **Oletusarvoinen enimmäismäärä** | **Enimmäismäärä**
-------- | ------------- | -------------
tietoja tehtaan Azure tilaus: | 50 | [Ota yhteyttä tukeen](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
putkistot tietojen factory sisällä | 2500 | [Ota yhteyttä tukeen](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
tietojoukkoja tietojen factory sisällä | 5000 | [Ota yhteyttä tukeen](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
samanaikainen sektoreiden tietojoukko kohden | 10 | 10
objektille myyntijakso objektit- <sup>1</sup> tavua | 200 KT | 2 000 KT
objektille tietojoukko ja linkitetyistä objektit- <sup>1</sup> tavua | 100 KT | 2 000 KT
HDInsight tarvittaessa klusterin sydämiä tilauksen <sup>2</sup> | 48 | [Ota yhteyttä tukeen](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
Cloud tietojen siirtämistä yksikkö <sup>3</sup> | 8 | [Ota yhteyttä tukeen](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
Myyntijakso tehtävän toimii Laske uudelleen | 1 000 | MaxInt (32-bittinen)

<sup>1</sup> putkijohto ja tietojoukko linkitetyistä objektit edustavat looginen ryhmittely havainnollistamiseen. Nämä objektit rajoitukset eivät koske määriä tietoa, voit siirtää ja käsitellä Azure Data Factory-palvelussa. Tietoja factory on suunniteltu skaalata petabytes tietojen käsittelemiseen.

<sup>2</sup> tarvittaessa HDInsight sydämiä varataan tilauksen, joka sisältää tietoja-factory ulos. Tuloksena yllä rajoitus on Data Factory voimassa tarvittaessa HDInsight sydämiä core rajoitus, ja eroaa Azure-tilaukseen liittyvää core rajoitus.

<sup>3</sup> cloud tietojen siirtämistä yksikön (dmu tässä) on käytössä cloud cloud kopiointi. Se on mitta, joka edustaa Data Factory yhtenä yksikkönä potenssiin (suorittimen ja muistin verkon resurssivaraukset yhdistelmä). Voit tehostaa suurempi kopioi siirtonopeuden mukaan hyödyntäminen Lisää DMUs joissakin tilanteissa. Lisätietoja on ohjeaiheessa [Cloud tietojen siirtämistä yksiköt](../../articles/data-factory/data-factory-copy-activity-performance.md#cloud-data-movement-units) -osaan, valitse tiedot.

**Resurssi** | **Oletusarvoinen alaraja** | **Alaraja**
-------- | ------------------- | -------------
Ajoituksen väli | 15 minuutin | 15 minuutin
Uudelleenyritykset väli | sekunnin | sekunnin
Yritä aikakatkaisuarvo | sekunnin | sekunnin


### <a name="web-service-call-limits"></a>Web-palvelun puhelun rajoitukset

Azure Resurssienhallinta on API-kutsujen rajoitukset. API soittaa [Azure Resurssienhallinta API rajoitukset](../azure-subscription-service-limits.md#resource-group-limits)korvaus-palvelussa. 



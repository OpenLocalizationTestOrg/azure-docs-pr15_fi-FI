<properties
    pageTitle="Azure-sovelluksen palveluun ClearDB MySql-tietokantojen usein kysytyt | Microsoft Azure"
    description="Vastauksia yleisiin kysymyksiin ClearDB MySQL-tietokantojen käyttäminen Azure App palvelussa."
    documentationCenter="php"
    services=""
    authors="sunbuild"
    manager="yochayk"
    editor=""
    tags="mysql"/>

<tags
    ms.service="multiple"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/27/2016"
    ms.author="sumuth"/>

# <a name="faq-for-cleardb-mysql-databases-with-azure-app-service"></a>Usein kysyttyjä Kysymyksiä ClearDB MySql-tietokannat Azure-sovelluksen ominaisuudet

Nämä usein kysytyt kysymykset vastataan yleisiin kysymyksiin käyttämisestä ja ostamalla ClearDB MySQL tietokantojen Azure-verkkosovelluksissa.

## <a name="what-options-do-i-have-for-mysql-on-azure"></a>Asetuksista tarvitsen MySQL-Azure?

Sinulla on useita vaihtoehtoja:

* [ClearDB jaettu MySQL-tietokantaan](/marketplace/partners/cleardb/databases/)

* [ClearDB MySQL Premium klustereiden](/marketplace/partners/cleardb-clusters/cluster/)

* [Azure-AM MySQL klusteriin](https://github.com/azure/azure-quickstart-templates/tree/master/mysql-replication)

* [Yksi Azure-AM käytössä MySQL-esiintymä](virtual-machines/virtual-machines-windows-classic-mysql-2008r2.md)

ClearDB on MySQL, isännöintipalvelu ja hallitsee MySQL-infrastruktuurin puolestasi. Kun suoritat oman MySQL-klusterin tai tietokannan Azure Virtual Machine, sinun on määrittäminen MySQL-palvelin ja pitää sen päivitetty korjaukset.

## <a name="do-i-need-a-credit-card-for-the-web-app--mysql-template-in-the-azure-marketplace"></a>Tarvitaan luottokorttia Web Appin + Azure Marketplacesta MySQL-malli?

Tämä riippuu siitä, että käytät tilauksen tyypin. Seuraavassa on joitakin usein käytettyjä tilaustyypit:

* [Käytön mukaan](/offers/ms-azr-0003p/): edellyttää luottokortin tiedot, ja kun ostat luottokorttisi veloitetaan maksettu MySQL-tietokantaan.

* [Maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/): sisältää hyvitykset käyttäminen Microsoft Azure-palvelut, mutta ei salli kolmannen osapuolen resurssien hankkiminen. Kolmannen osapuolen palvelujen tai maksettu MySQL-tietokantaan, sinun on käytettävä luottokorttia käyttöön tilauksen. Web Apps-sovellukset voit luoda vapaa ClearDB MySQL-tietokantaan.

* [MSDN-tilaus](/pricing/member-offers/msdn-benefits/) ja **Valitse keskihajonta-testi maksaa MSDN tavalliseen tapaan**: kaltaisilta maksuttoman kokeiluversion MSDN-tilauksen edellyttää, että voit hankkia maksettu MySQL-ratkaisun ClearDB luottokorttia.

* [Enterprise Agreement (EA)](/pricing/enterprise-agreement/): EA asiakkaat ovat laskuttaa vastaan niiden EA neljännesvuosittain kaikkien niiden Azure Marketplace (kolmannen osapuolen) ostot erilliseen, kootut laskussa. Ovat laskuttaa raha sitoumus minkä tahansa marketplace-ostot ulkopuolella. Huomaa, että tällä hetkellä Azure kaupan ei ole rekisteröity Azerbaidžan, Kroatia, Norja ja Puerto Rico asiakkaiden käytettävissä. 

*  [DreamSpark](https://www.dreamspark.com/Product/Product.aspx?productid=99): Voit luoda vain vapaa ClearDB tietokantoja verkkosovelluksissa. Ei ole rajoitettu vapaa ClearDB MySQL-tietokantoja, voit luoda määrän. Huomaa, että vapaa tietokantoja on ei, jota käytetään tuotannon web Apps-sovellukset, kun tämä palvelu on tarkoitettu vain kokeiluversio.

## <a name="why-was-i-charged-350-for-a-web-app--mysql-from-the-azure-marketplace"></a>Miksi 3.50 verkkosovellukseen + MySQL Azure Marketplacesta on veloitetaan?

Tietokannan oletusarvo on Titan, joka on $3.50. Olemme piilottaminen kustannukset tietokannan luonnin aikana ja voit ostaa vahingossa ei ollut tarkoitus tietokannan. Olemme yrität löytää voi parantaa käyttökokemusta mutta vasta sitten kaikki oman valitun hinnoittelu tasoa web app-ja tietokanta on kuitattava ennen valitsemalla **Luo** ja resurssien käyttöönoton käynnistäminen.

## <a name="i-am-running-mysql-on-my-own-azure-virtual-machine-can-i-connect-my-azure-web-app-to-my-database"></a>Käytössäni MySQL omaa Azure virtuaalikoneen. Voin muodostaa yhteyden tietokantaan Azure web-sovelluksen?

Kyllä. Voit muodostaa web app-tietokantaan, kunhan Azure AM on antanut etäkäyttöpalvelimen web App-sovellukseen. Lisätietoja on artikkelissa [Asenna MySQL virtual koneeseen](virtual-machines/virtual-machines-windows-classic-mysql-2008r2.md).

## <a name="in-which-countries-are-cleardb-premium-mysql-clusters-supported"></a>Missä maissa ovat ClearDB Premium MySQL klustereiden tukee?

[ClearDB Premium MySQL klustereiden](/marketplace/partners/cleardb-clusters/cluster/) ovat käytettävissä kaikilla alueilla Azure maailmanlaajuisesti lukuun ottamatta Intia, Australian Brasilia Etelä tai kiina.

## <a name="can-i-create-a-new-cluster-prior-to-creating-a-database-with-cleardb-premium-cluster-solution"></a>Voit luoda uuden klusterin ennen tietokannan luomiseen ClearDB premium klusterin-ratkaisuun?

Ei, tyhjä ClearDB klustereiden ei voi luoda. Azure portaalin avulla voit luoda tietokantoja klusterin, joita voi luoda uuden klusterin samanaikaisesti.

## <a name="will-i-be-warned-if-i-try-to-delete-a-cleardb-mysql-database-that-is-in-use-by-one-of-my-applications"></a>Voin varoitetaan siitä Jos yritän poistaa ClearDB MySQL-tietokantaan, joka on käytössä jokin Omat sovellukset?

Ei, Azure ei varoittaa, jos poistat marketplace osto, joka määräytyy sovelluksen.

## <a name="which-regions-can-i-create-cleardb-databases-in"></a>Mitä alueita voit luoda ClearDB tietokantojen?

Azure Marketplacesta ei ole rekisteröity Azerbaidžan, Kroatia, Norja tai Puerto Rico asiakkaiden käytettävissä. Näiden alueiden ClearDB ei ole käytettävissä.

## <a name="what-pricing-tier-should-i-choose-for-a-production-web-app-and-database"></a>Mitä hinnoittelu taso tuotannon web app-ja tietokanta kannattaa valita?

Käytä Basic tai suurempi hinta taso verkkosovelluksissa. ClearDB Suosittelemme Saturnuksella tai Jupiter suunnitelma. Tarkista ominaisuudet ja rajoitukset kunkin hinnoittelu tason sekä [Web Apps -sovellusten](/pricing/details/app-service/) ja [ClearDB MySQL tietokannoissa](/marketplace/partners/cleardb/databases/) , valitse haluamasi vaihtoehto tarpeitasi.

## <a name="how-do-i-upgrade-my-cleardb-database-from-one-plan-to-another"></a>Kuinka päivitän ClearDB tietokantani palvelupaketista toiseen?

[Azure portal](https://portal.azure.com)voi skaalata ClearDB jaetun isännöintipalvelu tietokannan. Lue lisätietoja tässä [artikkelissa](https://blogs.msdn.microsoft.com/appserviceteam/2016/10/06/upgrade-your-cleardb-mysql-database-in-azure-portal/) . Tällä hetkellä päivitys ei tueta, ClearDB Premium klustereiden Azure-portaalissa.

## <a name="i-cant-see-my-cleardb-database-in-azure-portal"></a>Azure-portaalissa ClearDB tietokantani ei näy?

Jos ClearDB tietokannan Azure Resurssienhallinta tai [Uuden Azure-portaalin](https://portal.azure.com)avulla luodaan, se ei näy [Vanha Azure-portaalissa](https://manage.windowsazure.com). Työ-ongelman on linkki tietokannan manuaalisesti web Appiin. Lisää vastaavasti Jos ClearDB tietokannan luominen [Vanha portal](https://manage.windowsazure.com) et voi olla näe tietokannan [Uuden Azure-portaalin](https://portal.azure.com). Ei ole työ-ympärille jälkimmäisessä skenaarion.

## <a name="who-do-i-contact-for-support-when-my-database-is-down"></a>Kuka voin ottaa yhteyttä tukeen, kun tietokanta ei ole käytettävissä?

Yhteyshenkilön [ClearDB tukevat](https://www.cleardb.com/developers/help/support) minkä tahansa tietokannan liittyvät ongelmat. Valmistaudu antamaan Azure tilauksen tiedot.

## <a name="can-i-create-additional-users-for-my-cleardb-mysql-database-cluster-solution"></a>Voinko luoda muita käyttäjiä oma ClearDB MySQL-tietokantaan klusterin ratkaisun? 

Ei. Et voi luoda uusia käyttäjiä, mutta voit luoda ClearDB tietokanta-klusterin muut tietokannat.  

## <a name="can-basicpro-series-databases-be-upgraded-in-place-similar-to-planetary-plans-today-on-cleardb-portal"></a>Basic/Pro sarjan tietokantojen voidaan päivittää käytönaikainen samalla tänään Planetary suunnitelmien ClearDB portaalissa?

Kyllä, Basic sarjan tietokantojen voidaan päivittää käytönaikainen (Basic 60 – Basic 500). Pro sarjan voidaan päivittää käytönaikainen (Pro 125 Pro 1 000) Pro 60 lukuun ottamatta. Microsoft ei tue päivitetään Pro 60 tietokantaa tällä hetkellä. 

## <a name="when-i-migrate-my-resources-from-one-subscription-to-another-does-my-cleardb-mysql-database-get-migrated-as-well"></a>Kun siirrän resurssien yhden tilauksesi toiseen, ClearDB MySQL-tietokantaan ei siirretä sekä? 

Kun suoritat resurssin siirto-tilauksissa, käyttää joitakin [rajoituksia](./app-service-web/app-service-move-resources.md) . ClearDB MySQL-tietokantaan on kolmannen osapuolen palvelun ja näin ollen ei siirretä Azure tilauksen siirron aikana. Jos hallitset ei MySQL-tietokantaan siirretään Azure resurssien ennen siirtämistä, ClearDB MySQL-tietokannat voidaan poistaa käytöstä. Siirtää manuaalisesti tietokantoja ensin ja tee sitten Azure tilauksen siirron web App-sovelluksen. 

## <a name="can-i-transfer-a-cleardb-database-from-a-credit-card-subscription-to-an-ea-subscription"></a>Voit siirtää ClearDB tietokannan luottokortin tilauksesta EA tilaukseen?

Aiemmin luotujen ClearDB tietokantojen Käytä luottokorttia, liittyvät aiemmin tilaukset. Voit käyttää EA-tilaus, sinun on siirrettävä tiedot uuteen tietokantaan seuraavasti:

* Osta uusi ClearDB tietokanta EA tilaus.
* Tietojen siirtäminen uuteen tietokantaan.
* Päivitä sovellus uuteen tietokantaan.
* Poista vanha ClearDB tietokannan.

Kun luot MySQL-tietokantaan (ClearDB) tai luoda uuden verkkosovellukseen MySQL (ClearDB)-tilaus, voit valita määrittää, miten maksat palvelun. EA-tilauksen Microsoft ei estä kolmannen osapuolen palveluihin ClearDB Azure-portaalissa hankintoja. EA tilaukset ulkopuolella raha sitoumus laskuttaa ja ovat laskuttaa vuosineljännesten ja jälkikäteen. EA asiakas on määritetty maksutapaa, kuten luottokorttia maksaa kolmannen osapuolen marketplace palvelut.

## <a name="where-can-i-see-the-charges-for-cleardb-resources-in-an-ea-subscription"></a>Jossa EA tilauksen ClearDB resurssit ovat näkyvissä?

Suora EA asiakkaiden Azure Marketplacesta kulujen näkyvät yritysportaaliin. Huomaa, että kaikki marketplace hankintoja ja kulutus ulkopuolella raha sitoumus laskuttaa ja ovat laskuttaa neljännesvuosittain ja jälkikäteen. EA asiakkaiden on maksaa suoraan kolmannen osapuolen palveluntarjoajien ja voit tehdä ottamalla maksutapaa, kuten luottokorttia niiden EA-tilillä.

Epäsuora EA asiakkaat löytävät niiden Azure Marketplace-tilauksia yritysportaalin **Tilausten hallinta** -sivulla, mutta hinnat piilotetaan. Asiakkaiden tulisi ottaa yhteyttä niiden LSP lisätietoja marketplace kulut.

Kolmannen osapuolen palvelujen Azure Marketplace-käyttöä voidaan hallita EA Azure-rekisteröinti järjestelmänvalvojat. Ne poistetaan käytöstä tai ottaminen käyttöön access 3 osapuolen ostot tilien hallinta-ja tilaukset-kaupasta yritysportaalin tilit-kohdassa.

## <a name="who-do-i-contact-for-questions-about-my-bill-for-cleardb-services-in-my-ea-subscription"></a>Kuka voin ottaa yhteyttä kysymyksille ClearDB palveluiden laskun tietoja-EA tilauksen?

Ota yhteyttä [Yrityksen asiakastukeen](http://aka.ms/AzureEntSupport) tapauksen Laskutus-kohdassa EA niiden rekisteröinti. EA Portal tukiryhmän sisällytetään kysymyksiisi tai ohjeet ongelman ratkaisemiseen.

 



## <a name="more-information"></a>Lisätietoja

[Azure Marketplacesta usein kysytyt kysymykset](/marketplace/faq/)

<properties
    pageTitle="Hallitse päätepisteet Azure liikenteen hallinta | Microsoft Azure"
    description="Tämän artikkelin avulla voit lisätä, poistaa, ottaminen käyttöön ja poistaminen käytöstä päätepisteet Azure liikenteen hallinta."
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="add-disable-enable-or-delete-endpoints"></a>Lisääminen, poistaminen käytöstä, ottaminen käyttöön tai poistaa päätepisteet

Azure-sovelluksen palvelun Web Apps-ominaisuus on jo automaattisesti ja pyöreän pöydän liikenne reititys toimintoja sivustot sisällä palvelinkeskukseen, riippumatta siitä, sivuston-tilassa. Voit määrittää automaattisesti ja pyöreän pöydän liikenne reititys sivustot ja cloud Services-palvelujen eri palvelinkeskusten Azure liikenteen hallinta. Tarpeen vastaavia toimintoja ensimmäiseksi on lisättävä cloud palvelun tai sivuston päätepisteen liikenteen hallinta.

>[AZURE.NOTE]  Tässä artikkelissa kerrotaan, miten voit käyttää perinteinen portaalin. Azure perinteinen portal tukee vain luomista ja tehtävään päätepisteet pilvipalveluihin ja verkkosovelluksissa. Uuden [Azure portaalissa](https://portal.azure.com) on ensisijainen käyttöliittymä.

Voit myös poistaa yksittäisiä päätepisteet liikenteen hallinta profiilin osana olevat. Päätepisteen poistaminen käytöstä jättää sen osana profiilin, mutta profiilin toimii aivan kuin päätepiste ei sisälly ei. Tämä toiminto on hyödyllinen poistaminen väliaikaisesti päätepisteen, joka sijaitsee ylläpidon tila tai on otettava käyttöön uudelleen. Kun päätepisteen on miltei, se voi olla käytössä.

>[AZURE.NOTE] Päätepisteen poistaminen käytöstä ei mitään tehdään sen käyttöönoton tilan Azure-tietokannassa. Luo siis kelvollinen päätepisteen pysyy ylös- ja vastaanottaa liikenne myös silloin, kun käytöstä liikenteen hallinta. Lisäksi päätepisteen toisen profiilin poistaminen käytöstä ei vaikuta toiseen profiiliin sen tila.

## <a name="to-add-a-cloud-service-or-website-endpoint"></a>Jos haluat lisätä cloud palvelun tai sivuston päätepiste

1. Etsi Azure perinteinen portaalin liikenteen hallinta-ruudussa liikenteen hallinta profiilia, joka sisältää päätepisteen asetuksia, jota haluat muokata. Avaa asetukset-sivun, valitse profiili-nimen oikealla puolella olevaa nuolta.
2. Valitse sivun yläreunassa **päätepisteet** tarkastelemiseen, jotka ovat jo kokoonpanosi päätepisteet.
3. Sivun alareunassa valitsemalla **Lisää** käyttää **Lisää Palvelupäätepisteet** -sivulla. Sivun luettelo oletusarvon mukaan **Päätepisteiden**kohdassa pilvipalveluihin.
4. Valitse pilvipalveluun pilvipalveluihin, voit lisätä ne päätepisteet profiilia-luettelossa. Valinnan cloud palvelunimi poistaa sen päätepisteet luettelo.
5. Sivustot- **Palvelutyyppi** avattavasta luettelosta ja valitse sitten **Web Appissa**.
6. Valitse sivustot-luettelosta Lisää ne päätepisteet profiilia. Valinnan poistaminen sivuston nimi poistaa sen päätepisteet luettelo. Voit valita vain yhden sivuston kohti Azure palvelinkeskuksen (tunnetaan myös nimellä alue). Kun valitset ensimmäisen sivuston, sama joten muiden sivustojen käytettävistä valinnan. Huomaa, että vain Standard sivustot näkyvät.
7. Kun olet valinnut päätepisteet profiilin, valitsemalla valintamerkki oikeassa alakulmassa Tallenna muutokset.

>[AZURE.NOTE] Kun lisäät tai poistat päätepisteen *automaattisesti* liikenne reititys menetelmällä profiilista, automaattisesti prioriteetti-luettelossa ei voidaan tutkia ne niin kuin haluat. Voit muuttaa järjestystä määritys-sivulla automaattisesti prioriteetti-luettelossa. Lisätietoja on kohdassa [Määritä automaattisesti-liikenne reititys](traffic-manager-configure-failover-routing-method.md).

## <a name="to-disable-an-endpoint"></a>Päätepisteen poistaminen käytöstä

1. Etsi Azure perinteinen portaalin liikenteen hallinta-ruudussa liikenteen hallinta profiilia, joka sisältää päätepisteen asetuksia, jota haluat muokata. Avaa asetukset-sivun, valitse profiili-nimen oikealla puolella olevaa nuolta.
2. Valitse sivun yläreunassa **päätepisteet** tarkastelemiseen, jotka sisältyvät kokoonpanosi päätepisteet.
3. Valitse päätepiste, jonka haluat poistaa käytöstä ja valitse sitten sivun alareunassa **Poista käytöstä** .
4. Asiakkaat edelleen liikenne lähettää päätepisteen toistaminen-elinaika (TTL). Voit muuttaa TTL liikenteen hallinta profiilin määritys-sivulla.

## <a name="to-enable-an-endpoint"></a>Jos haluat ottaa käyttöön päätepisteen

1. Etsi Azure perinteinen portaalin liikenteen hallinta-ruudussa liikenteen hallinta profiilia, joka sisältää päätepisteen asetuksia, jota haluat muokata. Avaa asetukset-sivun, valitse profiili-nimen oikealla puolella olevaa nuolta.
2. Valitse sivun yläreunassa **päätepisteet** tarkastelemiseen, jotka sisältyvät kokoonpanosi päätepisteet.
3. Valitse päätepiste, jonka haluat ottaa käyttöön ja valitse sitten sivun alaosassa **Ota käyttöön** .
4. Asiakkaiden ohjataan käytössä päätepisteen profiilin mukaisesti.

## <a name="to-delete-a-cloud-service-or-website-endpoint"></a>Jos haluat poistaa cloud palvelun tai sivuston päätepiste

1. Etsi Azure perinteinen portaalin liikenteen hallinta-ruudussa liikenteen hallinta profiilia, joka sisältää päätepisteen asetuksia, jota haluat muokata. Avaa asetukset-sivun, valitse profiili-nimen oikealla puolella olevaa nuolta.
2. Valitse sivun yläreunassa **päätepisteet** tarkastelemiseen, jotka ovat jo kokoonpanosi päätepisteet.
3. Valitse päätepisteet-sivulla, jonka haluat poistaa profiilista päätepisteen nimi.
4. Valitse sivun alareunassa **Poista**.

## <a name="next-steps"></a>Seuraavat vaiheet

* [Liikenteen hallinta-profiileista hallinta](traffic-manager-manage-profiles.md)
* [Määritä reititys menetelmät](traffic-manager-configure-routing-method.md)
* [Vianmäärityksen liikenteen hallinta heikentynyt tila](traffic-manager-troubleshooting-degraded.md)
* [Liikenteen hallinta suorituskykyyn liittyviä tietoja](traffic-manager-performance-considerations.md)
* [Toiminnot-liikenteen hallinta (REST API hakemisto)](http://go.microsoft.com/fwlink/p/?LinkID=313584)

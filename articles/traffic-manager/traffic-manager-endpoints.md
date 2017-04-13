<properties
   pageTitle="Hallitse päätepisteet Azure liikenteen hallinta | Microsoft Azure"
   description="Tämän artikkelin avulla voit lisätä, poistaa, ottaminen käyttöön ja poistaminen käytöstä päätepisteet Azure liikenteen hallinta."
   services="traffic-manager"
   documentationCenter=""
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="traffic-manager"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/17/2016"
   ms.author="sewhee" />

# <a name="add-disable-enable-or-delete-endpoints"></a>Lisääminen, poistaminen käytöstä, ottaminen käyttöön ja poistaminen päätepisteet

Azure-sovelluksen palvelun Web Apps-ominaisuus on jo automaattisesti ja pyöreän pöydän liikenne reititys toimintoja sivustot sisällä palvelinkeskukseen, riippumatta siitä, sivuston-tilassa. Voit määrittää automaattisesti ja pyöreän pöydän liikenne reititys sivustot ja cloud Services-palvelujen eri palvelinkeskusten Azure liikenteen hallinta. Tarpeen vastaavia toimintoja ensimmäiseksi on lisättävä cloud palvelun tai sivuston päätepisteen liikenteen hallinta.

>[AZURE.NOTE] Et voi lisätä ulkoisiin sijainteihin tai liikenteen hallinta-profiileista kuin päätepisteet Azure perinteinen-portaalissa. Sinun on käytettävä REST API [Luominen määritys](http://go.microsoft.com/fwlink/p/?LinkId=400772) - tai Windows PowerShellin [Lisää AzureTrafficManagerEndpoint](http://go.microsoft.com/fwlink/p/?LinkId=400774).

Voit myös poistaa yksittäisiä päätepisteet liikenteen hallinta profiilin osana olevat. Päätepisteet ovat pilvipalveluiden ja sivustot. Päätepisteen poistaminen käytöstä jättää sen osana profiilin, mutta profiilin toimii aivan kuin päätepiste ei sisälly ei. Tämä toiminto on erittäin hyödyllinen poistaminen väliaikaisesti päätepisteen, joka sijaitsee ylläpidon tila tai on otettava käyttöön uudelleen. Kun päätepisteen on miltei, se voi olla käytössä.

>[AZURE.NOTE] Päätepisteen poistaminen käytöstä ei mitään tehdään sen käyttöönoton tilan Azure-tietokannassa. Luo siis kelvollinen päätepisteen säilyy ylös- ja vastaanottaa liikenne myös silloin, kun käytöstä liikenteen hallinta. Lisäksi päätepisteen toisen profiilin poistaminen käytöstä ei vaikuta toiseen profiiliin sen tila.

## <a name="to-add-a-cloud-service-or-website-endpoint"></a>Jos haluat lisätä cloud palvelun tai sivuston päätepiste


1. Azure perinteinen portaalissa liikenteen hallinta-ruudun Etsi liikenteen hallinta profiilia, joka sisältää, jota haluat muokata päätepistettä asetukset ja valitse sitten profiilin nimen oikealla puolella olevaa nuolta. Profiilin asetukset-sivu avautuu.
2. Valitse sivun yläreunassa **päätepisteet** tarkastelemiseen, jotka ovat jo kokoonpanosi päätepisteet.
3. Sivun alareunassa valitsemalla **Lisää** käyttää **Lisää Palvelupäätepisteet** -sivulla. Sivun luettelo oletusarvon mukaan **Päätepisteiden**kohdassa pilvipalveluihin.
4. Valitse pilvipalveluun pilvipalveluihin, jotta ne päätepisteet profiilia-luettelossa. Valinnan cloud palvelunimi poistaa sen päätepisteet luettelo.
5. Sivustot- **Palvelutyyppi** avattavasta luettelosta ja valitse sitten **Web Appissa**.
6. Valitse sivustot-luettelosta Lisää ne päätepisteet profiilia. Valinnan poistaminen sivuston nimi poistaa sen päätepisteet luettelo. Huomaa, että voit valita vain yhden sivuston kohti Azure palvelinkeskuksen (tunnetaan myös nimellä alue). Jos valitset sivusto, joka isännöi useita sivustoja, kun valitset ensimmäisen sivuston palvelinkeskukseen, muiden saman joten poistuvat käytöstä valinnan. Huomaa, että vain Standard sivustot näkyvät.
7. Kun olet valinnut päätepisteet profiilin, valitsemalla valintamerkki oikeassa alakulmassa Tallenna muutokset.

>[AZURE.NOTE] Jos käytössäsi on *automaattisesti* liikenne reititys menetelmä, kun lisäät tai poistat päätepisteen, muista säätäminen automaattisesti prioriteetti-luettelossa määritys-sivulla vastaamaan kokoonpanon automaattisesti järjestyksessä. Lisätietoja on kohdassa [Määritä automaattisesti-liikenne reititys](traffic-manager-configure-failover-routing-method.md).

## <a name="to-disable-an-endpoint"></a>Päätepisteen poistaminen käytöstä

1. Azure perinteinen portaalissa liikenteen hallinta-ruudun Etsi liikenteen hallinta profiilia, joka sisältää, jota haluat muokata päätepistettä asetukset ja valitse sitten profiilin nimen oikealla puolella olevaa nuolta. Profiilin asetukset-sivu avautuu.
2. Valitse sivun yläreunassa **päätepisteet** tarkastelemiseen, jotka sisältyvät kokoonpanosi päätepisteet.
3. Valitse päätepiste, jonka haluat poistaa käytöstä ja valitse sitten sivun alareunassa **Poista käytöstä** .
4. Liikenne lopetetaan juoksutus päätepisteen perusteella DNS--elinaika (TTL) määritetty liikenteen hallinta toimialuenimi. Voit muuttaa TTL-liikenteen hallinta profiilin määritys-sivulla.

## <a name="to-enable-an-endpoint"></a>Jos haluat ottaa käyttöön päätepisteen

1. Azure perinteinen portaalissa liikenteen hallinta-ruudun Etsi liikenteen hallinta profiilia, joka sisältää, jota haluat muokata päätepistettä asetukset ja valitse sitten profiilin nimen oikealla puolella olevaa nuolta. Profiilin asetukset-sivu avautuu.
2. Valitse sivun yläreunassa **päätepisteet** tarkastelemiseen, jotka sisältyvät kokoonpanosi päätepisteet.
3. Valitse päätepiste, jonka haluat ottaa käyttöön ja valitse sitten sivun alaosassa **Ota käyttöön** .
4. Liikenne alkaa juoksutus uudelleen nimellä sanellun mukaan profiili-palveluun.

## <a name="to-delete-a-cloud-service-or-website-endpoint"></a>Jos haluat poistaa cloud palvelun tai sivuston päätepiste


1. Azure perinteinen portaalissa liikenteen hallinta-ruudun Etsi liikenteen hallinta profiilia, joka sisältää, jota haluat muokata päätepistettä asetukset ja valitse sitten profiilin nimen oikealla puolella olevaa nuolta. Profiilin asetukset-sivu avautuu.
2. Valitse sivun yläreunassa **päätepisteet** tarkastelemiseen, jotka ovat jo kokoonpanosi päätepisteet.
3. Valitse päätepisteet-sivulla, jonka haluat poistaa profiilista päätepisteen nimi.
4. Valitse sivun alareunassa **Poista**.

>[AZURE.NOTE] Et voi poistaa ulkoisiin sijainteihin tai liikenteen hallinta-profiileista kuin päätepisteet Azure perinteinen-portaalissa. Käytä Windows PowerShell. Lisätietoja on artikkelissa [Poista AzureTrafficManagerEndpoint](https://msdn.microsoft.com/library/dn690251.aspx).

## <a name="next-steps"></a>Seuraavat vaiheet

- [Määritä automaattisesti reititys menetelmä](traffic-manager-configure-failover-routing-method.md)
- [PYÖRISTÄ-funktiota Mikko reititys menetelmä määrittäminen](traffic-manager-configure-round-robin-routing-method.md)
- [Määritä suorituskyvyn reititys menetelmä](traffic-manager-configure-performance-routing-method.md)
- [Vianmäärityksen liikenteen hallinta heikentynyt tila](traffic-manager-troubleshooting-degraded.md)
- [Toiminnot-liikenteen hallinta (REST API hakemisto)](http://go.microsoft.com/fwlink/p/?LinkID=313584)

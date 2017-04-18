<properties
   pageTitle="Azure pilvipalveluihin ja näennäiskoneiden diagnostiikka määrittämisestä | Microsoft Azure"
   description="Tässä artikkelissa käsitellään määrittäminen diagnostiikkatietoja virheenkorjaus Azure cloude palvelut ja Visual Studio näennäiskoneiden (VMs)."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="configuring-diagnostics-for-azure-cloud-services-and-virtual-machines"></a>Azure Pilvipalveluiden ja näennäiskoneiden diagnostiikka määrittäminen

Jos sinun tarvitsee Azure pilvipalvelussa tai Azure virtuaalikoneen liittyviä ongelmia, voit määrittää Azure diagnostiikka helpommin Visual Studion avulla. Azure diagnostiikka sieppaa Järjestelmätiedot ja lokitiedot näennäiskoneiden ja virtuaalikoneen esiintymät, jotka suoritetaan pilvipalvelussa sijaitsevaan ja siirtää tiedot valittua tallennustilan-tilille. Saat lisätietoja kirjautumisesta Azure Diagnostiikka [Diagnostiikka web Apps-sovellukset Azure App palvelun lokiin kirjaaminen käyttöön](./app-service-web/web-sites-enable-diagnostic-log.md) .

Tässä ohjeaiheessa esitellään ottaminen käyttöön ja määrittäminen Azure diagnostiikka Visual Studiossa, ennen kuin ja käyttöönoton jälkeen ja Azure-virtuaalikoneissa. Se myös näyttää valitseminen diagnostiikka tietotyypeistä voi kerätä ja kuinka voit tarkastella tietoja, kun se on kerätty.

Voit määrittää Azure diagnostiikka seuraavilla tavoilla:

- Voit muuttaa diagnostiikka määritysasetukset Visual Studiossa **Diagnostiikka määritys** -valintaikkunan kautta. Asetukset on tallennettu tiedosto nimeltä diagnostics.wadcfgx (diagnostics.wadcfg Azure SDK 2.4 tai aiempi versio). Voit myös muokata suoraan määritystiedosto. Jos päivität tiedoston manuaalisesti, määritysten muutokset tulevat voimaan seuraavan käyttöönottoa pilveen palvelun Azure tai suorittaa palvelun emulaattori.

- Käytä **Cloud Explorer** tai **Palvelimen Explorer** Visual Studiossa käynnissä pilvipalvelussa tai virtuaalikoneen diagnostiikka-asetusten muuttaminen.

## <a name="azure-26-diagnostics-changes"></a>Azure 2.6 diagnostiikka muutokset

Azure SDK 2.6 projektien Visual Studiossa seuraavat muutokset on tehty. (Nämä muutokset koskevat myös Azure SDK uudemmissa versioissa.)

- Paikallinen emulaattori tukee nyt Diagnostiikka. Tämä tarkoittaa sitä, voit diagnostiikka tietojen kerääminen ja varmistaa sovelluksen luodaan oikean jäljittää samalla, kun olet kehittämiseen ja testaamiseen Visual Studiossa. Yhteysmerkkijonon `UseDevelopmentStorage=true` mahdollistaa diagnostiikka tietojen kerääminen samalla, kun käytät cloud palvelun projektin Visual Studiossa Azure tallennustilan emulaattori avulla. Kaikki diagnostiikka tiedot kerätään (kehittäminen tallennus)-tallennustilan tilin.

- Diagnostiikan tallennustilan tilin-yhteysmerkkijonon (Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString) tallennetaan uudelleen service-kokoonpanotiedosto (.cscfg). Azure SDK 2,5 diagnostiikka-tallennustilan tilin määritettiin diagnostics.wadcfgx-tiedostossa.

On joitakin keskeisiä eroja miten yhteysmerkkijonon käyttänyt Azure SDK 2.4 ja aiempi versio ja miten se toimii Azure SDK 2.6 ja uudemmissa versioissa välillä.

- Azure SDK 2.4 ja aiemmissa yhteysmerkkijono on käytetty ohjelmaa diagnostiikka-lisäosan tallennustilan tilin tiedot siirretään diagnostiikka lokitiedot.

- Azure SDK 2.6 ja uudemmissa versioissa diagnostiikka-yhteysmerkkijonon käytetään Visual Studio diagnostiikka-tunniste määrittää haluamasi tallennustilan tilitiedot julkaisemisen aikana. Yhteysmerkkijonon avulla voit määrittää eri määritykset, Visual Studio käyttää, kun julkaiset eri tallennustilan tilit. Koska diagnostiikka-laajennus ei ole enää käytettävissä (jälkeen Azure SDK 2,5), erillisenä .cscfg-tiedostoa ei voi ottaa diagnostiikka-tunniste. Sinun on otettava tunniste erikseen työkaluilla, kuten Visual Studiossa tai PowerShellin kautta.

- Visual Studio paketin tulosteen sisältää voidaan yksinkertaistaa määrittämisestä PowerShellin diagnostiikka-tunniste, diagnostiikka-tunniste kunkin roolin julkisen määrityskohde XML. Visual Studio käyttää diagnostiikan yhteysmerkkijonon täytä esitä julkisen määrityksessä tallennustilan tilitiedot. Julkinen config tiedostot Extensions-kansio luodaan, ja noudata kuvion PaaSDiagnostics. &lt;RoleName >. PubConfig.xml. Minkä tahansa mukaan PowerShell-käyttöönotoissa käyttämällä tätä mallia kunkin määritysten yhdistäminen rooli.

- Yhteysmerkkijonon .cscfg tiedostossa käytetään myös [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) diagnostiikka-tietojen käytön, jotta se voi näkyä **Seuranta** -välilehti. Yhteysmerkkijonon tarvitaan yksityiskohtainen seurantatiedot näyttäminen portaalin-palvelun määrittäminen.

## <a name="migrating-projects-to-azure-sdk-26-and-later"></a>Luotujen projektien Azure SDK 2.6 ja uudempi versio

Kun siirtyminen Azure SDK 2,5 Azure SDK 2.6 tai uudempi versio, jos haluat käyttää määritettyä .wadcfgx tiedoston diagnostiikka tallennustilan-tiliä, valitse se säilyy siellä. Joustavuutta eri tallennustilan tilin käyttäminen eri tallennustilan käyttömahdollisuudet hyödyntää, sinun on yhteysmerkkijonon manuaalisesti lisääminen projektiin. Jos olet siirtänyt projektin Azure SDK 2.4 tai sitä vanhempi versio Azure SDK 2.6, diagnostiikka yhteyden merkkijonot säilytetään. Huomaa kuitenkin muutokset miten yhteysmerkkijonon käsitellään Azure SDK 2.6 määritetyllä tavalla edellisessä osassa.

### <a name="how-visual-studio-determines-the-diagnostics-storage-account"></a>Miten Visual Studio määrittää diagnostiikka-tallennustilan tilin

- Jos Diagnostiikka yhteysmerkkijonon määritetään .cscfg-tiedostossa, Visual Studio käyttää sitä Määritä diagnostiikka-tunniste, kun julkaiset ja luotaessa julkisen määritysten xml-tiedostot Pakkaaminen aikana.

- Jos Diagnostiikka ei yhteysmerkkijonon määritetään .cscfg-tiedosto, valitse Visual Studio kuuluu takaisin määrittäminen diagnostiikka-tunniste, kun julkaiseminen ja luodaan julkisen määritysten xml-tiedostoja, kun pakkaamista .wadcfgx-tiedostossa määritetyn tallennustilan tilin avulla.

- Diagnostiikan yhteysmerkkijonon .cscfg tiedoston ohittaa tallennustilan tilin .wadcfgx-tiedostossa. Jos Diagnostiikka yhteysmerkkijonon määritetään .cscfg-tiedostossa, Visual Studio käyttää, ja ohittaa .wadcfgx tallennustilan tilin.

### <a name="what-does-the-update-development-storage-connection-strings-checkbox-do"></a>Mitä "päivitys kehittäminen tallennustilan yhteyden merkkijonot..." valintaruutu toimi?

**Päivityksen kehittäminen tallennustilan yhteyden merkkijonot diagnostiikka-ja Microsoft Azure tallennustilan tilin tunnistetiedoilla julkaiset Microsoft Azure välimuisti** valintaruudun valinta avulla on helppo päivittää kaikki kehittäminen tallennustilan tilin yhteyden merkkijonoja julkaisemisen aikana määritetyn Azure-tallennustilan tilin.

Oletetaan esimerkiksi, valitse tämä valintaruutu ja diagnostiikka-yhteysmerkkijono määrittää `UseDevelopmentStorage=true`. Kun julkaiset projektin Azure-Visual Studio päivittyvät automaattisesti diagnostiikka-yhteysmerkkijonon Julkaise ohjatussa tallennustilan tilin kanssa. Jos reaali tallennustilan-tili on määritetty diagnostiikka-yhteysmerkkijonon, sitten tilin käytetään kuitenkin sen sijaan.

## <a name="diagnostics-functionality-differences-between-azure-sdk-24-and-earlier-and-azure-sdk-25-and-later"></a>Toimintojen diagnostiikka Azure SDK 2.4 ja 2.5 ja uudempi aiemmin ja Azure SDK erot

Jos päivität projektin Azure SDK 2.4 Azure SDK 2.5 tai uudempi versio, voit pitäisi olla muista diagnostiikka toiminnot-erot.

- **Määritysten ohjelmointirajapinnan on poistettu** – ohjelmallisesti määritys-kansio on käytettävissä Azure SDK 2.4 tai sitä aiemmissa versioissa, mutta se on poistettu Azure SDK 2,5 ja uudemmissa versioissa. Jos Diagnostiikka-määritys on tällä hetkellä määritetty koodi, tarvitset määrittämään voit jatkaa työskentelyä siirretyt projektin diagnostiikka järjestyksessä tyhjästä nämä asetukset. Azure SDK 2.4 diagnostiikka määritystiedosto on diagnostics.wadcfg ja diagnostics.wadcfgx Azure SDK 2,5 tai uudempaa versiota.

- **Cloud Palvelusovellusten diagnostiikkaa voidaan määrittää vain roolin tasolla, ei esiintymän tasolla.**

- **Aina, kun asennat sovelluksen, diagnostiikka-määritys on päivitetty** – Tämä voi aiheuttaa ongelmia välistä eroa, jos muutat diagnostiikka-asetusten Server Explorerista ja ota sitten sovellus uudelleen.

- **Azure SDK 2,5 ja uudempi versio, kaatumisen Vedostaa on määritetty diagnostiikka-kokoonpanotiedosto ei koodissa** – Jos sinulla on määritetty koodi kaatumisvedokset, sinun tulee siirtämään määritykset koodin manuaalisesti määritystiedostoa, koska kaatumisvedokset ei siirretä Azure SDK 2.6 siirron aikana.

## <a name="enable-diagnostics-in-cloud-service-projects-before-deploying-them"></a>Ota diagnostiikan cloud palvelun projektien ennen kuin otat ne

Visual Studiossa voit kerätä diagnostiikka tietoja, jotka suoritetaan Azure, kun suoritat palvelun emulaattori ennen kuin otat sen roolien. Diagnostiikan asetusten Visual Studiossa, kaikki muutokset tallentuvat diagnostics.wadcfgx määritystiedostossa. Nämä asetukset Määritä diagnostiikka tietojen tallennuspaikan, kun otat käyttöön oman pilvipalvelussa tallennustilan tili.

### <a name="to-enable-diagnostics-in-visual-studio-before-deployment"></a>Jos haluat ottaa käyttöön Visual Studiossa diagnostiikka ennen käyttöönottoa

1. Roolin kiinnostavaa ryhmää hiiren kakkospainikkeella ja valitse Valitse **Ominaisuudet**ja valitse rooli **Ominaisuudet** -ikkunassa **määritys** -välilehti.

1. Varmista **Diagnostiikka** -osassa **Ota diagnostiikka** -valintaruutu on valittuna.

    ![Ota käyttöön diagnostiikka-vaihtoehdon käyttäminen](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796660.png)

1. Valitse Määritä haluamaasi diagnostiikka-tiedot tallennetaan tallennustilan pistettä (…)-painike. Voit valita tallennustilan tilin on diagnostiikka tietojen tallennussijainnin.

    ![Määritä käytettävä tallennuskiintiön tili](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796661.png)

1. **Luo tallennustilan yhteysmerkkijono** -valintaikkunassa määrittää haluat muodostaa yhteyden Azure-tallennustilan emulaattorin Azure tilauksen vai manuaalisesti tunnistetiedot.

    ![Tallennustilan tili-valintaikkuna](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796662.png)

  - Jos valitset Microsoft Azure tallennustilan emulaattorin vaihtoehto, yhteysmerkkijono on määritetty UseDevelopmentStorage = true.

  - Jos valitset tilaus-vaihtoehto, voit valita Azure tilauksen, jota haluat käyttää ja tilin nimi. Voit valita Azure tilausten hallinta tilien hallinta-painiketta.

  - Jos valitset manuaalisesti syötetty tunnistetiedot-asetus, sinua kehotetaan nimen ja Azure-tili jota haluat käyttää avainta.

1. **Diagnostiikan määritys** -valintaikkuna valitsemalla **Määritä** . Välilehtien (lukuun ottamatta **Yleinen** ja **Log kansioiden**) edustaa diagnostiikan tietolähdettä, jossa voit kerätä. Oletusvälilehti, **Yleiset**-mahdollistaa seuraavat diagnostiikka tietojen sivustokokoelman asetukset: **vain virheet**, **kaikki tiedot**ja **Mukautettu suunnitelma**. Oletusasetus **vain virheet**, kestää vähintään tallennustilan määrä, koska ei siirrä varoituksia tai jäljitys viestit. Kaikki tietoja-asetuksen siirtää suurin osa tiedoista ja on siksi eniten kallista vaihtoehto tallennustilan kannalta.

    ![Ota käyttöön Azure diagnostiikka- ja määrittäminen](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758144.png)

1. Tässä esimerkissä Valitse **Mukautettu-suunnitelma** -vaihtoehdon, jotta voit mukauttaa kerättyjä tietoja.

1. **Kiintiö megatavuina** -ruutuun määrittää, paljonko tilaa haluat varata tallennustilan tilisi diagnostiikka tiedoille. Voit halutessasi muuttaa oletusarvoa.

1. Valitse kunkin diagnostiikka kerää tiedot-välilehdessä sen **käyttöön siirtää <log type> ** valintaruudun valinta. Jos haluat kerätä sovelluksen lokit, valitse **sovelluksen lokit siirto Ota käyttöön** -valintaruutu, jos **Sovelluslokit** -välilehdessä. Määritä myös muita tietoja vaatii diagnostiikka kunkin tietotyypin. Kohdassa **Määritä diagnostiikka tietolähteiden** kokoonpanotietoja varten tämän artikkelin kussakin välilehdessä.

1. Kun olet kaikki haluamasi diagnostiikka tietojen keräämisen, valitse **OK** -painiketta.

1. Suorita Azure cloud palvelun projektin Visual Studiossa tavalliseen tapaan. Kun käytät sovelluksen, lokitiedot, jotka olet ottanut käyttöön tallennetaan määrittämääsi Azure-tallennustilan tilin.

## <a name="enable-diagnostics-in-azure-virtual-machines"></a>Ota käyttöön vianmäärityksen Azuren näennäiskoneiden

Visual Studiossa voit valita Azuren näennäiskoneiden diagnostiikka hakukenttä.

### <a name="to-enable-diagnostics-in-azure-virtual-machines"></a>Jos haluat ottaa käyttöön Azuren näennäiskoneiden diagnostiikka

1. Valitse **Palvelimen Explorer**Azure solmu ja muodostaa Azure-tilauksesi, jos et ole vielä yhteydessä.

1. Laajenna **näennäiskoneiden** solmu. Voit luoda uuden virtual koneen tai valitse jokin, joka on jo olemassa.

1. Valitse for virtuaalikoneen kiinnostavaa ryhmää hiiren kakkospainikkeella ja **Määritä**. Tämä näyttää virtuaalikoneen määritys-valintaikkuna.

    ![Azure virtuaalikoneen määrittäminen](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796663.png)

1. Jos sitä ei ole jo asennettu, Lisää Microsoft Agent-diagnostiikka seuranta-tunniste. Tämän tunnisteen avulla voit kerätä tietoja diagnostiikka Azure virtuaalikoneen. Asennettu laajennukset-luettelosta valitsemalla Valitse käytettävissä tunniste-avattava valikko ja valitse sitten Microsoft Agent-diagnostiikka seuranta.

    ![Azure virtuaalikoneen-laajennuksen asentaminen](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766024.png)

    >[AZURE.NOTE] Muita diagnostiikka-laajennukset ovat käytettävissä kohdassa näennäiskoneiden. Lisätietoja on artikkelissa Azure AM tunnisteet ja toiminnot.

1. Valitse Lisää-tunniste ja tarkastella sen **Diagnostiikka configuration** -valintaikkunan **Lisää** -painiketta.

1. Valitse **valitsemalla Määritä tallennustilan tili ja valitse sitten **OK** -painiketta** .

    Välilehtien (lukuun ottamatta **Yleinen** ja **Log kansioiden**) edustaa diagnostiikan tietolähdettä, jossa voit kerätä.

    ![Ota käyttöön Azure diagnostiikka- ja määrittäminen](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758144.png)

    Oletusvälilehti, **Yleiset**-mahdollistaa seuraavat diagnostiikka tietojen sivustokokoelman asetukset: **vain virheet**, **kaikki tiedot**ja **Mukautettu suunnitelma**. Oletusasetus **vain virheet**, kestää vähintään tallennustilan määrä, koska ei siirrä varoituksia tai jäljitys viestit. **Kaikki tietoja** -asetuksen siirtää suurin osa tiedoista ja on siksi eniten kallista vaihtoehto tallennustilan kannalta.

1. Tässä esimerkissä Valitse **Mukautettu-suunnitelma** -vaihtoehdon, jotta voit mukauttaa kerättyjä tietoja.

1. **Kiintiö megatavuina** -ruutuun määrittää, paljonko tilaa haluat varata tallennustilan tilisi diagnostiikka tiedoille. Voit halutessasi muuttaa oletusarvoa.

1. Valitse kunkin diagnostiikka kerää tiedot-välilehdessä sen **käyttöön siirtää <log type> ** valintaruudun valinta.

    Jos haluat kerätä sovelluksen lokit, valitse **sovelluksen lokit siirto Ota käyttöön** -valintaruutu, jos **Sovelluslokit** -välilehdessä. Määritä myös muita tietoja vaatii diagnostiikka kunkin tietotyypin. Kohdassa **Määritä diagnostiikka tietolähteiden** kokoonpanotietoja varten tämän artikkelin kussakin välilehdessä.

1. Kun olet kaikki haluamasi diagnostiikka tietojen keräämisen, valitse **OK** -painiketta.

1. Tallenna päivitetyn projektin.

    Näyttöön tulee sanoma **Microsoft Azure tehtävän loki** -ikkunassa virtuaalikoneen on päivitetty.

## <a name="configure-diagnostics-data-sources"></a>Määritä diagnostiikka-tietolähteet

Kun olet ottanut käyttöön diagnostiikka tietojen kerääminen, voit valita täsmälleen tietolähteiden haluat kerätä ja kerätyistä tiedoista. Seuraavassa on luettelo **Diagnostiikka määritys** -valintaikkuna ja mitä kukin määritysvaihtoehto tarkoittaa välilehtiä.

### <a name="application-logs"></a>Sovelluksen lokit

**Sovelluksen lokit** sisältää diagnostiikka verkkosovelluksen tuottamat tiedot. Jos haluat ottaa sovelluksen lokit, valitse **sovelluksen lokit siirto käyttöön** -valintaruutu. Voit suurentaa tai pienentää minuuttimäärä, kun sovelluslokit siirretään tallennustilan tiliisi muuttamalla **Siirtää ajan (min)** -arvo. Voit myös muuttaa lokiin määrittämällä Log arvo kerätyistä tiedoista määrää. Voit esimerkiksi valita **yksityiskohtainen** lisätietoja tai valitse **Kriittinen** kannattaa tallentaa vain vakavia virheitä. Jos sinulla on tiettyjä diagnostiikka-palvelun, joka tietokoneesta kuuluu äänimerkki sovelluksen lokit, voit siepata ne lisäämällä tarjoajan GUID-tunnus **Tarjoajan GUID-tunnus** -ruutuun.

  ![Sovelluksen lokit](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758145.png)

  Saat lisätietoja sovelluksen lokit [Diagnostiikka web Apps-sovellukset Azure App palvelun lokiin kirjaaminen käyttöön](./app-service-web/web-sites-enable-diagnostic-log.md) .

### <a name="windows-event-logs"></a>Windowsin tapahtumalokien

Jos haluat ottaa Windowsin tapahtumalokien, valitse **Ota käyttöön Siirrä ja Windowsin tapahtumalokien** -valintaruutu. Voit suurentaa tai pienentää minuuttimäärä, jonka kun tapahtumalokit siirretään tallennustilan tiliisi muuttamalla **Siirtää ajan (min)** -arvo. Valitse tapahtumat, jota haluat seurata muunlaisten valintaruudut.

  ![Tapahtumalokien](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796664.png)

Jos käytät Azure SDK 2.6 tai uudempi versio ja haluat määrittää mukautetun tietolähteen, kirjoita sen **<Data source name>** tekstiä ruutuun ja valitse sitten sen vieressä olevaa **Lisää** -painiketta. Tietolähteen lisätään diagnostics.cfcfg-tiedosto.

Jos käytät Azure SDK 2,5 ja haluat määrittää mukautetun tietolähteen, voit lisätä sen `WindowsEventLog` diagnostics.wadcfgx-osassa tiedoston, kuten seuraavassa esimerkissä.

```
<WindowsEventLog scheduledTransferPeriod="PT1M">
   <DataSource name="Application!*" />
   <DataSource name="CustomDataSource!*" />
</WindowsEventLog>
```
### <a name="performance-counters"></a>Suorituskyvyn laskureita

Laskuri-Suorituskykytiedot auttavat järjestelmän kootussa ja tehdä järjestelmän ja sovellusten suorituskykyä. Lisätietoja on kohdassa [Create and Käytä suorituskyvyn laskureita Azure-sovelluksessa](https://msdn.microsoft.com/library/azure/hh411542.aspx) . Jos haluat ottaa näyttöleikkeen suorituskyvyn laskureita, valitse **siirron suorituskyvyn laskureita käyttöön** -valintaruutu. Voit suurentaa tai pienentää minuuttimäärä, jonka kun tapahtumalokit siirretään tallennustilan tiliisi muuttamalla **Siirtää ajan (min)** -arvo. Valitse, jota haluat seurata suorituskykylaskureita valintaruudut.

  ![Suorituskyvyn laskureita](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758147.png)

Voit seurata suorituskykyä laskuri, joka ei ole luettelossa, syöttää käyttämällä ehdotettu syntaksia ja valitse sitten **Lisää** -painiketta. Virtuaalikoneen käyttöjärjestelmä määrittää, mitä voit seurata suorituskykylaskureita. Saat lisätietoja syntaksista [Laskuri polun määrittäminen](https://msdn.microsoft.com/library/windows/desktop/aa373193.aspx).

### <a name="infrastructure-logs"></a>Julkaisuinfrastruktuuri-lokit

Jos haluat ottaa näyttöleikkeen infrastruktuurin lokit, jotka sisältävät tietoja Azure diagnostiikan infrastruktuuri, RemoteAccess-moduulin ja RemoteForwarder moduulin, valitse **infrastruktuurin lokit siirto käyttöön** -valintaruutu. Voit suurentaa tai pienentää minuuttimäärä, kun lokit siirretään tallennustilan tiliisi muuttamalla siirtää ajan (min)-arvo.

  ![Diagnostiikan infrastruktuurin lokit](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758148.png)

  Lisätietoja on kohdassa [Kirjaaminen tietojen kerääminen Azure Diagnostiikan avulla](https://msdn.microsoft.com/library/azure/gg433048.aspx) .

### <a name="log-directories"></a>Log-kansiot

Jos haluat ottaa lokiin kansioita, jotka sisältävät log kansioista Internet Information Services (IIS) pyyntöjen kerättyjä tietoja, epäonnistui pyynnöt tai kansioita, joka on valittava, valitse **Ota loki kansioiden siirto** -valintaruutu. Voit suurentaa tai pienentää minuuttimäärä, kun lokit siirretään tallennustilan tiliisi muuttamalla **Siirtää ajan (min)** -arvo.

Voit valita haluat kerätä, kuten **IIS-lokit** ja **Epäonnistui pyytää** lokit lokit-ruutuihin. Tallennustilan säilö oletusnimet ovat käytettävissä, mutta voit halutessasi muuttaa nimet.

Lisäksi voit siepata lokit mistä tahansa kansiosta. Määritä vain polku **suora hakemistosta Log** -osassa ja valitse sitten **Lisää hakemisto** -painiketta. Lokit voi tallentaa määritetyn säiliöiden.

  ![Log-kansiot](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796665.png)

### <a name="etw-logs"></a>Tapahtumien seuranta-lokit

Jos [Tapahtuman jäljitys for Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803(v=vs.85).aspx) (tapahtumien seuranta) ja haluat siepata tapahtumien seuranta lokit, valitse **Ota käyttöön tapahtumien seuranta lokit siirto** -valintaruutu. Voit suurentaa tai pienentää minuuttimäärä, kun lokit siirretään tallennustilan tiliisi muuttamalla **Siirtää ajan (min)** -arvo.

Tapahtuman lähteistä ja tapahtuman luettelot, jotka määrität välitetyt tapahtumat. Voit määrittää tapahtumalähdettä, kirjoita **Tapahtumalähteet** -osiossa nimi ja valitse sitten **Lisää Tapahtumalähde** -painiketta. Voit vastaavasti määrittää tapahtuma-luettelo **Tapahtuma-luettelot** -kohdassa ja valitsemalla **Lisää tapahtuman luettelon** .

  ![Tapahtumien seuranta-lokit](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766025.png)

  Tapahtumien seuranta-framework tuetaan ASP.NET kautta luokkien [System.Diagnostics.aspx] (https://msdn.microsoft.com/library/system.diagnostics (v=vs.110) nimitila. Microsoft.WindowsAzure.Diagnostics nimitilan, jotka periytyvät ja laajentaa standard [System.Diagnostics.aspx] (https://msdn.microsoft.com/library/system.diagnostics (v=vs.110) luokkien, mahdollistaa System.Diagnostics.aspx (https://msdn.microsoft.com/library/system.diagnostics (v=vs.110) kuin kirjaaminen kehys Azure-ympäristössä. Lisätietoja on artikkelissa [kestää ohjausobjektin, ja Microsoft Azure jäljitys](https://msdn.microsoft.com/magazine/ff714589.aspx) ja [Ottaminen käyttöön diagnostiikka Azure Cloud Services-palveluissa ja näennäiskoneiden](./cloud-services/cloud-services-dotnet-diagnostics.md).

### <a name="crash-dumps"></a>Kaatumisvedokset

Jos haluat kerätä tietoja siitä, milloin roolin esiintymän kaatuu, valitse **siirron kaatua kirjoittaa käyttöön** -valintaruutu. (ASP.NET käsittelee useimmat poikkeusten, koska tämä on yleensä hyödyllistä vain työntekijä roolit.) Voit suurentaa tai pienentää tallennustilaa kaatumisvedokset käytetty prosenttiosuus muuttamalla **Hakemisto-kiintiön (%)** -arvoa. Voit muuttaa tallennustilan säilö, jossa kaatumisvedokset tallennetaan, ja voit valita, haluatko siepata **koko** tai **tarkasteleminen** dump.

Tällä hetkellä seurataan prosessit näkyvät. Valitse prosessit, jotka haluat säilyttää valintaruudut. Lisää luetteloon toinen prosessi, kirjoita prosessin nimi ja valitse sitten **Lisää Process** -painike.

  ![Kaatumisvedokset](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766026.png)

  Katso [kestää ohjausobjektin, ja Microsoft Azure jäljitys](https://msdn.microsoft.com/magazine/ff714589.aspx) ja [Microsoft Azure diagnostiikka osa 4: Mukautettu lokiin kirjaaminen-osat ja Azure diagnostiikka 1.3 muutokset](http://justazure.com/microsoft-azure-diagnostics-part-4-custom-logging-components-azure-diagnostics-1-3-changes/) lisätietoja.

## <a name="view-the-diagnostics-data"></a>Diagnostiikka-tietojen tarkasteleminen

Kun olet kerännyt diagnostiikka tiedot pilvipalveluun tai virtual machine, voit tarkastella sitä.

### <a name="to-view-cloud-service-diagnostics-data"></a>Voit tarkastella cloud diagnostiikka tiedot

1. Ota yhteyttä pilvipalvelussa kuin tavanomainen ja suorittamalla asennustiedosto.

1. Voit tarkastella tietoja diagnostiikka raportin, joka luo Visual Studio tai taulukoissa tallennustilan tilissäsi. Jos haluat tarkastella raportin tietoja, Avaa **Cloud Explorer** tai **Palvelimen Resurssienhallinnassa**, Avaa pikavalikko solmun roolin kiinnostavaa ryhmää ja valitse sitten **Näytä diagnostiikkatiedot**.

    ![Diagnostiikan tietojen tarkasteleminen](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC748912.png)

    Raportti, jossa näkyvät käytettävissä olevat tiedot tulee näkyviin.

    ![Microsoft Azure diagnostiikka-raportin Visual Studiossa](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796666.png)

    Jos uusimpia tietoja ei näy, voit joutua odottamaan siirron ajan kuluttua.

    Valitse **Päivitä** linkki tietojen päivittäminen heti, tai valitse aikavälin tiedot päivittyvät automaattisesti **Automaattinen päivitys** avattavassa luettelossa. Virhetietojen vieminen Valitse **Vie CSV** -painiketta voit avata laskentataulukon CSV-tiedoston luominen.

    **Cloud Explorer** tai **Palvelimen Resurssienhallinnassa**Avaa tallennustilan tili, joka liittyy käyttöönotto.

1. Avaa taulukko-katseluohjelman diagnostiikka-taulukot ja miten tiedot, jotka voit kerätä. IIS-lokit ja mukautetun lokit voit avata blob-säilö. Seuraavassa taulukossa tarkastelemalla löydät taulukon tai blob-säilö, joka sisältää tiedot, jotka olet kiinnostunut. Lisäksi, että lokitiedoston tiedot-taulukon merkinnät sisältävät EventTickCount, DeploymentId, rooli ja RoleInstance avulla voit selvittää, mitä virtuaalikoneen ja roolin luotu tiedot ja milloin. 

  	|Diagnostiikkatiedot|Kuvaus|Sijainti|
  	|---|---|---|
  	|Sovelluksen lokit|Lokit, joka koodisi Luo soittamalla maksutapojen System.Diagnostics.Trace-luokka.|WADLogsTable|
  	|Tapahtumalokien|Tämä tieto on Windowsin tapahtumalokien virtual-tietokoneissa. Windows tallentaa tiedot lokitiedostot, mutta sovellusten ja palveluiden myös niiden avulla virheistä tai lokitiedot.|WADWindowsEventLogsTable|
  	|Suorituskyvyn laskureita|Valitse mikä tahansa suorituskyvyn laskuri, joka on käytettävissä virtuaalikoneen voi kerätä tietoja. Käyttöjärjestelmä on suorituskyvyn laskureita, jotka sisältävät useita tilastotiedot, kuten muistin käyttö- ja suoritin aika.|WADPerformanceCountersTable|
  	|Julkaisuinfrastruktuuri-lokit|Lokitiedostot luodaan itse diagnostiikka-infrastruktuuria.|WADDiagnosticInfrastructureLogsTable|
  	|IIS-lokit|Näiden lokien tallentaminen web-pyynnöt. Jos pilvipalvelussa sijaitsevaan saa liikenteen merkittäviä määrä, lokitiedostot voivat olla aivan pitkiä, jotta olisi kerätä ja tallentaa nämä tiedot vain, kun tarvitse sitä.|Voit etsiä epäonnistui pyynnön kirjaa kohdassa wad iis-failedreqlogs polussa käyttöönoton, rooli ja esiintymän blob-säilö. Voit etsiä valmis lokit iis-publish logfiles wad-kohdassa. Merkinnät jokaiselle tiedostolle tehdään WADDirectories taulukossa.|
  	|Kaatumisvedokset|Nämä tiedot sisältää binaarikuvia pilvipalvelussa sijaitsevaan prosessin (yleensä työntekijän rooli).|wad koska-Vedostaa blob-säilö|
  	|Mukautetun lokitiedostot|Tiedot, jotka olet ennalta lokitiedot.|Voit määrittää koodin mukautetun lokitiedostojen sijainti tallennustilan tilissäsi. Voit esimerkiksi määrittää mukautetun blob-säilö.|

1. Jos katkaistaan kaikenlaisia tietoja, voit kokeilla lisääntyvien puskuri tietojen tyypin ja peruuttamisessa sekä siirrot virtuaalikoneen-tallennustilan tilin väli.

1. (valinnainen) Tyhjennä tiedot ajoittain, kun haluat vähentää tallennustilan kokonaiskustannuksiin tallennustilan-tililtä.

1. Kun teet koko käyttöönottoa, diagnostics.cscfg-tiedosto (.wadcfgx Azure SDK 2,5 varten) päivitetään Azure ja pilvipalvelussa sijaitsevaan vastataan diagnostiikka-asetusten muutokset. Jos-, Päivitä olemassa, .cscfg-tiedosto ei ole päivitetty Azure. Diagnostiikan asetusten, voit muuttaa kuitenkin seuraavan osion ohjeiden mukaan. Saat lisätietoja suorittamiseen koko käyttöönottoa ja päivitys olemassa [Azure sovelluksen ohjatun julkaisutoiminnon](vs-azure-tools-publish-azure-application-wizard.md).

### <a name="to-view-virtual-machine-diagnostics-data"></a>Voit tarkastella virtuaalikoneen diagnostiikka tietoja

1. Valitse **Diagnostiikka tietojen**virtuaalikoneen pikavalikosta.

    ![Näytä diagnostiikka tietojen Azure virtuaalikoneen](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766027.png)

    Tämä avaa **Diagnostiikka yhteenveto** -ikkunan.

    ![Azure virtuaalikoneen diagnostiikka yhteenveto](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796667.png)  

    Jos uusimpia tietoja ei näy, voit joutua odottamaan siirron ajan kuluttua.

    Valitse **Päivitä** linkki tietojen päivittäminen heti, tai valitse aikavälin tiedot päivittyvät automaattisesti **Automaattinen päivitys** avattavassa luettelossa. Virhetietojen vieminen Valitse **Vie CSV** -painiketta voit avata laskentataulukon CSV-tiedoston luominen.

## <a name="configure-cloud-service-diagnostics-after-deployment"></a>Määritä cloud palvelun diagnostiikka käyttöönoton jälkeen

Jos olet tutkiminen pilvestä ongelma service kyseisen jo käynnissä, voit kerätä tietoja, joita et ole määrittänyt ennen rooli on alun perin käyttöön. Tässä tapauksessa voit aloittaa kerätä tietoja palvelimen Explorerin asetusten avulla. Voit määrittää yhden esiintymän tai kaikki esiintymät diagnostiikka-roolin, sen mukaan, onko diagnostiikka määritys-valintaikkunan avaaminen esiintymän tai rooli pikavalikon. Jos määrität rooli-solmu, tekemäsi muutokset koskevat kaikki esiintymät. Jos määrität solmun esiintymän, tekemäsi muutokset koskevat vain kyseisen esiintymän.

### <a name="to-configure-diagnostics-for-a-running-cloud-service"></a>Voit määrittää diagnostiikkaa käynnissä pilvipalvelussa

1. Palvelimen Resurssienhallinnassa Laajenna **Cloud Services** -solmu ja laajenna sitten solmujen Etsi roolin tai esiintymän, jonka haluat tutkia tai molempia.

    ![Diagnostiikan asetusten määrittäminen](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC748913.png)

1. Pikavalikon esiintymä-solmu tai rooli solmu **päivityksen diagnostiikka**asetukset ja valitse diagnostiikan asetuksia, jotka haluat kerätä.

    Lisätietoja määritykset tämän ohjeaiheen kohdassa **Määritä diagnostiikka tietolähteet** . Lisätietoja diagnostiikka-tietojen tarkasteleminen tämän ohjeaiheen-kohdassa **Näytä diagnostiikka-tiedot** .

    Jos muutat tietojen kerääminen **Palvelimen**Resurssienhallinnassa, nämä muutokset ovat voimassa, kunnes pilvipalvelussa sijaitsevaan Ota kokonaan uudelleen. Jos käytössäsi on oletusarvoinen Julkaisuasetukset, muutokset eivät korvaa, oletusarvo julkaista asetus on Päivitä aiemmin käyttöönoton sijaan tehdä koko lukea. Varmista, että asetukset tyhjentää käyttöönoton milloin, Julkaise ohjattu **Lisäasetukset** -välilehti ja poista **käyttöönoton Päivitä** -valintaruutu. Ota uudelleen kyseinen valintaruutu ole valittuna, kun asetukset palautetaan kaltaisia .wadcfgx (tai .wadcfg)-tiedoston joukkona roolin ominaisuudet-editorin kautta. Jos päivität käyttöönoton, Azure pitää vanhat asetukset.

## <a name="troubleshoot-azure-cloud-service-issues"></a>Azure cloud palvelun ongelmien vianmääritys

Jos kohtaat ongelmia cloud palvelun projektien, rooli määrää jää "varattu-tila, kuten toistuvasti kierrättää tai ilmoittaa sisäinen palvelinvirhe, on työkaluja ja menetelmiä, voit tehdä vianmäärityksen ja korjauksen ongelmat. Katso esimerkkejä yleisten ongelmien ja ratkaisujen sekä, käsitteitä ja vianmääritys ja korjaus virheet käytettävät työkalut, [Azure PaaS Laske diagnostiikka-tietoihin](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).

## <a name="q--a"></a>Q & A

**Mikä on puskurikoon ja kuinka suuri on?**

Virtuaalikoneen jokaiselle esiintymälle kiintiön rajoittaa diagnostiikan tietojen voi olla tallennettuna paikallisessa tiedostojärjestelmässä. Lisäksi voit määrittää puskurikoko mistäkin diagnostiikan tiedot, jotka ovat käytettävissä. Puskurin kooksi toimii samalla tavalla kuin yksittäisiä kiintiön kyseisen tietojen tyypin. Valintaikkunan alareunassa valitsemalla selviää yleinen kiintiön ja muistin, joka on vielä. Jos määrität suurempi puskuri tai useita tietotyyppejä, yleinen kiintiö lähteä ratkomaan. Voit muuttaa yleinen kiintiön muokkaamalla diagnostics.wadcfg/.wadcfgx määritystiedostoa. Diagnostiikan tiedot on tallennettu samaan tiedostojärjestelmän sovelluksen tietoina joten jos sovellus käyttää paljon levytilaa, ei kannata Lisää yleinen diagnostiikka-kiintiön.

**Mikä on siirto-ajan, ja kuinka kauan on?**

Siirto on aika, joka kuluu tietojen välille kuvaa. Siirrä kukin ajan kuluttua tiedot siirretään paikallisesta tiedostojärjestelmästä virtual tietokoneeseen tallennustilan tilin taulukoihin. Jos tiedot, jotka on kerätty määrä ylittää kiintiön ennen siirtoa määräajan, vanhat tiedot hylätään. Voit pienentää siirto-ajan, jos olet menettämättä tietoja, koska tietojen ylittää puskurikoon tai yleinen kiintiö.

**Aikavyöhyke ovat aikaleimat sisään?**

Aikaleimoja ovat tietokeskuksen, joka isännöi pilvipalvelussa sijaitsevaan paikallinen aikavyöhyke. Log-taulukoiden seuraavat kolme aikaleima-sarakkeiden avulla.

  - **PreciseTimeStamp** on tapahtuman tapahtumien seuranta aikaleimaa. Aika, tapahtuma kirjataan asiakasohjelmassa.

  - **AIKALEIMA** on PreciseTimeStamp pyöristetään alaspäin Lataa korkojakso reunaa. Jotta jos Lataa korkojakso on 5 minuuttia ja tapahtuman kellonaika 00:17:12, AIKALEIMA on 00:15:00.

  - **Aikaleima** on aikaleiman, johon kohde on luotu Azure taulukkoon.

**Miten kustannusten hallinta vianmääritystiedot kerätessään?**

Pienennä kustannus on suunniteltu oletusasetuksia (**tason** **Virhe** asettaminen ja **siirtää ajan** arvoksi **1 minuutti**). Jos diagnostiikan lisätietojen kerääminen tai pienentää siirto kauden kasvattaa Laske kustannukset. Kerätä enemmän tietoja kuin tarvitset ja Älä unohda käytöstä tietojen kerääminen, kun et enää tarvitse sitä. Voit aina ottaa sen käyttöön uudelleen, myös suorituksen, kuten edellisessä osassa.

**Miten epäonnistui pyynnön lokien kerääminen IIS?**

Oletusarvon mukaan IIS ei kerää epäonnistui pyynnön lokitiedot. Voit määrittää IIS voi kerätä niitä, vaikka muokkaat seuraavan koodin korostetut web-roolin.

**Saan ei jäljitystiedot RoleEntryPoint vaihtoehtoja, kuten OnStart. Mikä nyt mättää?**

RoleEntryPoint menetelmiä kutsutaan WAIISHost.exe, ei IIS kontekstissa. Tämän vuoksi tiedot Web.config, joka tavallisesti mahdollistaa jäljitys ei käytä. Voit ratkaista ongelman .config tiedoston lisääminen web-roolin projektiin ja nimeä tiedosto vastaamaan tulosteen kokoonpano, joka sisältää RoleEntryPoint-koodin. Oletusarvoinen web rooli-projekti .config-tiedoston nimi olisi WAIISHost.exe.config. Lisää tämän tiedoston seuraavista riveistä:

```
<system.diagnostics>
  <trace>
      <listeners>
          <add name “AzureDiagnostics” type=”Microsoft.WindowsAzure.Diagnostics.DiagnosticMonitorTraceListener”>
              <filter type=”” />
          </add>
      </listeners>
  </trace>
</system.diagnostics>
```

Nyt **Ominaisuudet** -ikkunassa Määritä **Kopioi aina** **Kopioi tulosteen hakemisto** -ominaisuus.

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja diagnostiikka Azure kirjautumisesta on artikkelissa [Azure Cloud Services-palveluissa ja näennäiskoneiden ottaminen käyttöön diagnostiikka](./cloud-services/cloud-services-dotnet-diagnostics.md) - ja [Diagnostiikka web Apps-sovellukset Azure App palvelun lokiin kirjaaminen käyttöön](./app-service-web/web-sites-enable-diagnostic-log.md).

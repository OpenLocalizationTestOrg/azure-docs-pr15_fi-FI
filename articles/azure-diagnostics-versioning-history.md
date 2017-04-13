<properties
    pageTitle="Azure diagnostiikka versiohistoria"
    description="SELITYS muuttuu eri versioiden Azure diagnostiikka kuin lähetetty eri Microsoft Azure SDK: T-versioita."
    services="multiple"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="02/12/2016"
    ms.author="robb"/>


# <a name="microsoft-azure-diagnostics-version-history"></a>Microsoft Azure diagnostiikka versiohistoria

Uusi Azure diagnostiikka? Artikkelissa [Azure diagnostiikka yleiskatsaus](azure-diagnostics.md).

Jokaisen Azure SDK-version mukana yleensä uuden version Azure-kansio. Seuraavassa taulukossa on kuvattu SDK-versiossa liittyvät Azure SDK ja Azure diagnostiikka versiot.



Azure SDK-versio | Azure diagnostiikka-versio | Malli
--- | --- | ---
1.x      | 1.0 | laajennus
2.0-2.4| 1.0 | "
2.5      | 1.2 | tiedostotunniste
2.6      | 1.3 | "
2.7      | 1.4 | "
2,8      | 1,5 | "


Uusin versio on 1,5, johon sisältyy Azure SDK 2,8. Paikallinen emulaattori tilanteita, joissa käytetään vain Azure diagnostiikka-tunniste, joka sisältää SDK-version. Minkä tahansa sovelluksen käyttää uusimman version automaattisesti, kun käynnissä Azure, joka riippumatta SDK sovelluksen version muodostetaan. Paitsi jos olet asentanut uusimman Azure SDK-paketissa, sinulla ei ole kaikki työkaluista, joiden avulla voit hyödyntää uusia ominaisuuksia.

Eri toiminnot ja muutokset seuraavalla tavalla.

## <a name="azure-sdk-28"></a>Azure SDK 2,8
Azure SDK 2,8 lisätty lähettämisen diagnostiikka tietojen [Havainnollistamisen sovelluksen](./application-insights/app-insights-cloudservices.md) ongelmien vianmääritys sovelluksen sekä järjestelmän ja infrastruktuurin tason yli on helpompaa.

## <a name="azure-26-diagnostics-changes"></a>Azure 2.6 diagnostiikka muutokset

Azure SDK 2.6 pilvipalvelussa projektien Visual Studiossa seuraavat muutokset on tehty. (Nämä muutokset koskevat myös Azure SDK uudemmissa versioissa.)

- Paikallinen emulaattori tukee nyt Diagnostiikka. Tämä tarkoittaa sitä, voit diagnostiikka tietojen kerääminen ja varmistaa sovelluksen luodaan oikean jäljittää samalla, kun olet kehittämiseen ja testaamiseen Visual Studiossa. Yhteysmerkkijonon `UseDevelopmentStorage=true` mahdollistaa diagnostiikka tietojen kerääminen samalla, kun käytät cloud palvelun projektin Visual Studiossa Azure tallennustilan emulaattori avulla. Kaikki diagnostiikka tiedot kerätään (kehittäminen tallennus)-tallennustilan tilin.

- Diagnostiikan tallennustilan tilin-yhteysmerkkijonon (Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString) tallennetaan uudelleen service-kokoonpanotiedosto (.cscfg). Azure SDK 2,5 diagnostiikka-tallennustilan tilin määritettiin diagnostics.wadcfgx-tiedostossa.

On joitakin keskeisiä eroja miten yhteysmerkkijonon käyttänyt Azure SDK 2.4 ja aiempi versio ja miten se toimii Azure SDK 2.6 ja uudemmissa versioissa välillä.

- Azure SDK 2.4 ja aiemmissa yhteysmerkkijono on käytetty ohjelmaa diagnostiikka-lisäosan tallennustilan tilin tiedot siirretään diagnostiikka lokitiedot.

- Azure SDK 2.6 ja uudemmissa versioissa diagnostiikka-yhteysmerkkijonon käytetään Visual Studio diagnostiikka-tunniste määrittää haluamasi tallennustilan tilitiedot julkaisemisen aikana. Yhteysmerkkijonon avulla voit määrittää eri määritykset, Visual Studio käyttää, kun julkaiset eri tallennustilan tilit. Koska diagnostiikka-laajennus ei ole enää käytettävissä (jälkeen Azure SDK 2,5), erillisenä .cscfg-tiedostoa ei voi ottaa diagnostiikka-tunniste. Sinun on otettava tunniste erikseen työkaluilla, kuten Visual Studiossa tai PowerShellin kautta.

- Visual Studio paketin tulosteen sisältää voidaan yksinkertaistaa määrittämisestä PowerShellin diagnostiikka-tunniste, diagnostiikka-tunniste kunkin roolin julkisen määrityskohde XML. Visual Studio käyttää diagnostiikan yhteysmerkkijonon täytä esitä julkisen määrityksessä tallennustilan tilitiedot. Julkisen config tiedostot Extensions-kansio luodaan ja noudata mallia PaaSDiagnostics. <RoleName>. PubConfig.xml. Minkä tahansa mukaan PowerShell-käyttöönotoissa käyttämällä tätä mallia kunkin määritysten yhdistäminen rooli.

- Yhteysmerkkijonon .cscfg tiedostossa käytetään myös Azure portaali diagnostiikka-tietojen käytön, jotta se voi näkyä **Seuranta** -välilehti. Yhteysmerkkijonon tarvitaan yksityiskohtainen seurantatiedot näyttäminen portaalin-palvelun määrittäminen.

### <a name="migrating-projects-to-azure-sdk-26-and-later"></a>Luotujen projektien Azure SDK 2.6 ja uudempi versio

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

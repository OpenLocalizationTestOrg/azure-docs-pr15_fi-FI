<properties
   pageTitle="Määrittäminen käyttämällä useita palvelun määrityksiä Azure projektin | Microsoft Azure"
   description="Opettele määrittämään Azure cloud-palvelun projektin muuttamalla ServiceDefinition.csdef ja ServiceConfiguration.cscfg tiedostot."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="configuring-your-azure-project-using-multiple-service-configurations"></a>Käyttämällä useita palvelun määrityksiä Azure projektin määrittäminen

Azure cloud-palvelun project sisältää kaksi tiedostojen: ServiceDefinition.csdef ja ServiceConfiguration.cscfg. Nämä tiedostot ovat pakattu ja Azure cloud palvelusovellus ja Azure käyttöön.

- **ServiceDefinition.csdef** -tiedosto sisältää metatietoja, joita tarvitaan cloud palvelusovelluksen, mukaan lukien mitä se sisältää rooleja vaatimukset Azure käyttöympäristön mukaan. Tämä tiedosto sisältää myös asetukset, jotka koskevat kaikki esiintymät. Nämä asetukset voi lukea suorituksen aikana Azure palvelun isännöinnin Runtime Ohjelmointirajapinnan käyttäminen. Tätä tiedostoa ei voi päivittää, että palvelu on käynnissä Azure-tietokannassa.

- **ServiceConfiguration.cscfg** tiedosto määrittää arvot määritetty palvelun määritystiedostossa asetukset, ja määrittää, kuinka monta kunkin roolin suorittamisen. Tätä tiedostoa voi päivittää pilvipalvelussa sijaitsevaan on käynnissä Azure-tietokannassa.

Microsoft Visual Studio Azure-työkalut tarjoavat ominaisuuden sivuja, joiden avulla voit määrittää asetukset tallennetut tiedostot. Käyttämään ominaisuus-sivut roolin viittaus alapuolella Azure cloud palvelun projektin ratkaisunhallinnassa, kaksoisnapsauta tai rooli viittaus hiiren kakkospainikkeella ja valitse **Ominaisuudet**-seuraavassa kuvassa esitetyllä tavalla.

![VS_Solution_Explorer_Roles_Properties](./media/vs-azure-tools-multiple-services-project-configurations/IC784076.png)

Lisätietoja palvelun määritystä ja palvelun tiedostojen pohjana olevat mallit Hae [Rakenteen viittaus](https://msdn.microsoft.com/library/azure/dd179398.aspx). Lisätietoja palvelun määritykset Katso, [miten voit määrittää pilvipalveluihin](./cloud-services/cloud-services-how-to-configure.md).

## <a name="configuring-role-properties"></a>Rooli ominaisuuksien määrittäminen

Web-rooli ja Työntekijä-roolin ominaisuus-sivut ovat samankaltaisia, vaikka on joitakin eroja, seuraavien osien maininnut.

**Välimuisti** -sivulla voit määrittää palvelujen välimuistiin Azure.

### <a name="configuration-page"></a>Määrityssivulla

**Määritys** -sivulla voidaan määrittää seuraavat ominaisuudet:

**Esiintymiä**

Ominaisuuden **esiintymä** Laske palvelun suoritetaan tämän roolin esiintymien määrän.

**Ylimääräisen pieni**, **Pieni**, **Normaali**, **suuri**tai **Ylimääräiset suuri** **AM koko** -ominaisuuden arvoksi.  Lisätietoja on artikkelissa [koot pilvipalveluihin](./cloud-services/cloud-services-sizes-specs.md).

**Käynnistystoiminto** (Vain web-rooli)

Määritä tämän ominaisuuden avulla voit määrittää, että Visual Studio Käynnistä selain HTTP-päätepisteet tai HTTPS-päätepisteet tai molemmat virheenkorjaus käynnistyessä.

HTTPS päätepisteen-asetus on käytettävissä vain, jos olet jo määrittänyt HTTPS-päätepisteen roolin. Voit määrittää HTTPS-päätepisteen **päätepisteet** ominaisuuden sivulla.

Jos olet jo lisännyt päätepisteen HTTPS-HTTPS päätepisteen-asetus on käytössä oletusarvoisesti ja Visual Studio Käynnistä selain tämän päätepisteen käynnistyessä virheenkorjaus HTTP-päätepisteen selaimen lisäksi. Oletetaan, että molemmat käynnistysasetukset ovat käytössä.

**Vianmääritys**

Oletusarvon mukaan diagnostiikka käytössä Web rooli. Azure cloud projektin ja tallennustilaa palvelutili on määritetty käyttämään paikallisen säilön emulaattori. Kun olet valmis Azure käyttöön, voit valita Päivitä tallennustilan-tili, jota käytetään Azure tallennustilan pilveen muodostin-painike (****...). Voit siirtää diagnostiikka tiedot tallennustilan tilin pyydettäessä tai automaattisesti tietyin väliajoin. Saat lisätietoja Azure Diagnostiikka [Ottaminen käyttöön diagnostiikka Azure Cloud Services-palveluissa ja näennäiskoneiden](./cloud-services/cloud-services-dotnet-diagnostics.md).

## <a name="settings-page"></a>Asetukset-sivulla

Valitse **asetukset** -sivulla voit lisätä määritysasetukset palvelun. Asetukset ovat nimi-arvo-pareina. Rooli koodin voi lukea määritysasetukset [Azure hallitun kirjaston](http://go.microsoft.com/fwlink?LinkID=171026)myöntämä luokkien avulla suorituksen arvot. Tarkemmin sanottuna [GetConfigurationSettingValue](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.getconfigurationsettingvalue.aspx) menetelmä palauttaa arvon nimettyä määritys suorituksen aikana.

### <a name="configuring-a-connection-string-to-a-storage-account"></a>Yhteysmerkkijonon tallennustilan tilin määrittäminen

Yhteysmerkkijonon on, joka tarjoaa yhteyden ja todennustiedot tallennustilan emulaattori tai Azure-tallennustilan tilin määritys-asetusta. Aina, kun koodi on käyttää Azure tallennustilan services tietoja – eli blob, jonossa tai taulukkotietojen – koodin suorittamisen roolissa, sinun on yhteysmerkkijonon tallennustilan tilin määrittäminen.

Yhteysmerkkijonon, joka osoittaa Azure-tallennustilan tilin käytettävä määritetyssä muodossa. Lisätietoja yhteyden merkkijonot luomisesta on artikkelissa [Määrittäminen Azure tallennustilan yhteyden merkkijonoja](./storage/storage-configure-connection-string.md).

Kun olet valmis kokeilemaan palvelun vastaan Azure liittyviä palveluja tai kun olet valmis ottamaan yhteyttä pilvipalvelussa Azure, voit muuttaa minkä tahansa yhteyden merkkijonot osoittamaan Azure-tallennustilan tilin arvo. Valitse (****...) ja valitse **Enter tallennustilan tilin tunnistetiedot**. Kirjoita tilin tiedot, joka sisältää tilin nimi ja tili-näppäintä. **Tallennustilan tilin yhteysmerkkijono** -valintaikkunassa voit myös määrittää, haluatko käyttää oletusarvon HTTPS päätepisteet (oletusasetus), oletusarvon HTTP päätepisteet tai mukautetun päätepisteet. Voit haluta käyttää mukautettuja päätepisteet, jos olet rekisteröinyt mukautettua toimialuenimeä palvelun kuvatulla tavalla [määrittäminen blob-tietojen Azure-tallennustilan tilin mukautettua toimialuenimeä](./storage/storage-custom-domain-name.md).

>[AZURE.IMPORTANT] Muokkaa yhteyttä-merkkijonot osoittamaan Azure-tallennustilan tilin, ennen kuin otat yhteyttä palvelun. Toiminto ei ole saattaa aiheuttaa tehtäväsi ei, voit aloittaa ja käy läpi valmisteltaessa, varattu ja pysäyttäminen tiloja.

## <a name="endpoints-page"></a>Päätepisteet-sivulla

Työntekijän rooli voi olla jokin muu luku HTTP, HTTPS tai TCP päätepisteistä. Päätepisteet voi olla syötteen päätepisteitä, jotka ovat käytettävissä ulkoiset asiakkaat- tai sisäinen päätepisteitä, jotka ovat käytettävissä rooleista, joissa on käytössä-palvelussa.

- Tee HTTP-päätepisteen käytettävissä ulkoiset asiakkaat ja selaimet kirjaimilla päätepisteen-tyypin muuttaminen ja määritä nimi- ja julkinen porttinumero.

- Tee HTTPS-päätepisteen käytettävissä ulkoiset asiakkaat ja selaimet päätepisteen tyypin muuttaminen **syötteen**ja määritä nimi, julkinen porttinumero ja hallinta-varmenteen nimi.

    Huomaa, että ennen kuin voit määrittää hallinta-varmenteen, on määritettävä varmenteen **varmenteiden** ominaisuutta-sivulla.

- Tee päätepisteen käytettävissä sisäiseen käyttöön muiden roolia pilvipalvelussa sisäinen päätepisteen tyypin muuttaminen ja määritä nimi ja mahdolliset yksityinen portit tämän päätepisteen.

## <a name="local-storage-page"></a>Paikallinen tallennustila

**Paikallinen tallennus** ominaisuutta-sivulla voit varata vähintään yksi rooli paikallinen tallennus-resursseja. Paikallinen tallennus resurssi on varattu kansio, johon rooli esiintymää on käynnissä Azure virtuaalikoneen tiedostojärjestelmässä.

## <a name="certificates-page"></a>Varmenteet-sivu

Valitse **Varmenteet** -sivulla voit yhdistää varmenteet tehtäväsi. Sertifikaatteja, joita voit lisätä avulla voidaan määrittää HTTPS-päätepisteet **päätepisteet** ominaisuuden-sivulla.

**Varmenteiden** ominaisuutta-sivulla Lisää varmenteiden tietoja service-määritys. Huomaa, että sertifikaattien tarkasteleminen ei ole pakattu palveluun; täytyy ladata sertifikaattien tarkasteleminen erikseen Azure [Azure perinteinen portal](http://go.microsoft.com/fwlink/?LinkID=213885)kautta.

Varmenteen liitettävä tehtäväsi, Anna nimi varmenteen. Tämä nimi avulla voit viitata varmennetta, kun määrität HTTPS-päätepisteen **päätepisteet** ominaisuuden-sivu. Määritä seuraavaksi, onko sertifikaatin **Paikallisesta** tai **Nykyisen käyttäjän** ja säilön nimi. Kirjoita lopuksi varmenteen allekirjoitus. Jos varmenne on nykyinen User\Personal (My) säilön, voit kirjoittaa varmenteen allekirjoitus valitsemalla varmenteen täyttää luetteloista. Jos se sijaitsee toiseen sijaintiin, kirjoita allekirjoitusta käsin.

Kun lisäät varmenteen säilöstä, keskitason sertifikaatteja lisätään automaattisesti asetukset puolestasi. Keskitason nämä varmenteet on myös ladattava Azure määrittämiseksi oikein palvelun SSL.

Minkä tahansa hallinta sertifikaatteja, joita voit yhdistää palvelun koskevat palvelua vain, kun se suoritetaan pilveen. Kun palvelun toimii paikallisen kehitysympäristö, se käyttää vakiomuotoinen varmenne, jota hallitaan Laske emulaattori.

## <a name="configuring-the-azure-cloud-service-project"></a>Azure cloud palvelun projektin määrittäminen

Voit määrittää asetukset, jotka koskevat koko Azure cloud-palvelun projektin avaat projektin solmun pikavalikon ja valitse Avaa ominaisuussivujen ominaisuudet. Seuraavassa taulukossa on ominaisuuden sivut.

|Ominaisuus-sivu|Kuvaus|
|---|---|
|Sovelluksen|Tällä sivulla voit näyttää tietoja Azure-työkaluja, joilla cloud palvelun projektin käyttää, ja voit päivittää sen uusimpaan versioon Työkalut versiosta.|
|Tapahtumien luominen|Tällä sivulla voit määrittää vanhat muodosta ja jälkeinen muodosta tapahtumat.|
|Kehittäminen|Tällä sivulla voit määrittää muodosta määritysohjeet ja ehtojen vallitessa jälkeinen muodosta tapahtumat suoritetaan.|
|WWW|Tällä sivulla voit määrittää WWW-palvelimeen liittyviä asetuksia.|

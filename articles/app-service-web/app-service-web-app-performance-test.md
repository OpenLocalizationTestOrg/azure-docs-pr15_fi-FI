<properties
   pageTitle="Testaa Azure web-sovelluksen suorituskyvyn | Microsoft Azure"
   description="Suorittaa Azure web app suorituskyvyn testejä voit tarkistaa, kuinka sovelluksen käsittelee käyttäjän kuormituksen. Mittaa vastausajan ja etsiä virheitä, jotka saattavat tarkoittaa ongelmia."
   services="app-service\web"
   documentationCenter=""
   authors="ecfan"
   manager="douge"
   editor="jimbe"/>

<tags
   ms.service="app-service-web"
   ms.workload="web"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="05/25/2016"
   ms.author="estfan; manasma; ahomer"/>

# <a name="performance-test-your-azure-web-app-under-load"></a>Suorituskyvyn testata Azure koodiin kuormitus

Tarkista web-sovelluksen suorituskyvyn, ennen kuin voit käynnistää sen tai Ota päivitykset käyttöön tuotannon. Näin voit helpommin arvioida onko sovellus on valmis julkaistavaksi. Ilmeen varmempi sovelluksen voit käsitellä liikenne piikin käytön aikana tai oman seuraava markkinoinnin push.

Julkinen esikatselun aikana voit testi sovelluksen ilmaiseksi Azure-portaalissa.
Näiden testien simuloida sovelluksen käyttäjän kuormitus tietyn ajan kuluessa ja mitata sinua sovelluksen vastaus. Esimerkiksi testitulokset näkyvät kuinka nopeasti sovelluksen reagoi käyttäjien tietty määrä. Ne näyttävät myös, kuinka monta pyynnöt epäonnistui, joka saattaa ilmaista sovelluksen ongelmia.      

![Etsi suorituskykyongelmia-sovellukseen](./media/app-service-web-app-performance-test/azure-np-perf-test-overview.png)

## <a name="before-you-start"></a>Ennen aloittamista

* Tarvitset on [Azure tilauksen](https://account.windowsazure.com/subscriptions), jos sitä ei vielä ole. Katso, kuinka voit [avata Azure tilin maksutta](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).

* Sinun on [Visual Studio Team Services](https://www.visualstudio.com/products/what-is-visual-studio-online-vs) -tilin pitämään suorituskyky-testi historia. Sopivan tilin luodaan automaattisesti, kun määrität suorituskykytesti. Tai voit luoda uuden tilin tai Käytä aiemmin luotua tiliä, jos ole tilin omistajan. 

* Ottaa käyttöön sovelluksen testikäyttöön tuotantoympäristössä. On käyttää sovelluksen Service-palvelupaketti kuin käyttää tuotannon suunnitelman sovelluksen. Näin ei vaikuta nykyisten asiakkaiden tai hidastaa tuotannon sovelluksen. 

## <a name="set-up-and-run-your-performance-test"></a>Määritä ja suorita suorituskykytesti

0.  Kirjautuminen [Azure Portal](https://portal.azure.com). Visual Studio Team Services-tilin, jotka omistat käyttämään Kirjaudu tilin omistajan.

0.  Siirry web Appissa.

    ![Siirry Selaa kaikki Web Apps-koodiin](./media/app-service-web-app-performance-test/azure-np-web-apps.png)

0.  Siirry **testi**.

    ![Valitse Työkalut-testi](./media/app-service-web-app-performance-test/azure-np-web-app-details-tools-expanded.png)
 
0. Nyt linkki [Visual Studio Team Services](https://www.visualstudio.com/products/what-is-visual-studio-online-vs) -tilin pitämään suorituskyky-testi historia.

    Jos sinulla on Team Services-tili, jota käytetään, valitse tilin. Jos et, Luo uusi tili.

    ![Valitse olemassa olevan ryhmän Services-tilin tai luoda uuden tilin](./media/app-service-web-app-performance-test/azure-np-no-vso-account.png)

0.  Luo suorituskykytesti. Määritä tiedot, ja Suorita testi. 

Voit katsoa reaaliajassa tulokset testin suorittamisen aikana.

Oletetaan esimerkiksi, että sovellus, joka on antanut, kuponkeja, Edellinen vuosi lomakalenterin myynti on. Tapahtuman viimeksi 15 minuuttia, Huippu-kuormituksen 100 samanaikainen asiakkaiden kanssa. Haluat kaksoisnapsauttamalla asiakkaiden määrän tänä vuonna. Myös haluat parantaa asiakkaiden tyytyväisyyttä vähentämällä sivun latausajasta 5 sekuntia 2 sekunnin ajan. Olemme siis testataan Microsoftin päivitetyn sovelluksen suorituskyvyn 250 käyttäjää 15 minuutin.

Olemme simuloida kuormituksen monipuolisimmat, muodostamalla virtual käyttäjien (asiakkaat) kuka seuraavasta Microsoftin verkkosivustosta samanaikaisesti. Tämä näyttää us montako pyynnöt kaatuvat tai vastaa hitaasti.

  ![Luoda ja määrittää Suorituskykytestin suorittaminen](./media/app-service-web-app-performance-test/azure-np-new-performance-test.png)

   *  Web-sovelluksen oletusarvoinen URL-osoite lisätään automaattisesti. 
   Voit muuttaa muiden sivujen (vain HTTP GET pyyntöjen) Testaa URL-osoite.

   *  Voit simuloida paikallisen ehdot ja vähentää viive, valitse sijainti, joka on lähimpänä käyttäjille luonnissa kuormituksen.

  Seuraavassa on käynnissä testi. Ensimmäisen minuutin aikana usealle sivulle Lataa haluamme hitaammin.

  ![Testi käynnissä reaaliaikaisia tietoja](./media/app-service-web-app-performance-test/azure-np-running-perf-test.png)

  Kun testi on valmis, on tietoja, että sivu latautuu paljon nopeammin ensimmäisen minuutin kuluttua. Tämä auttaa tunnistamaan, jossa on kannattaa aloittaa ongelman vianmäärityksen.

  ![Valmiit testi näyttää tulokset, mukaan lukien epäonnistui pyynnöt](./media/app-service-web-app-performance-test/azure-np-perf-test-done.png)

## <a name="test-multiple-urls"></a>Testaa useita URL-osoitteet

Voit suorittaa suorituskyvyn testien saamiesi useita URL-osoitteet, jotka edustavat lopusta loppuun-Käyttäjäskenaario lataamalla Visual Studio Web Test-tiedosto. Joitakin tapoja, joilla voit luoda Visual Studio Web testi ovat:

* [Voit kerätä tietoliikenteen Fiddler ja vieminen Visual Studio Web Test-tiedostona](http://docs.telerik.com/fiddler/Save-And-Load-Traffic/Tasks/VSWebTest)
* [Lataa testitiedosto luominen Visual Studio](https://www.visualstudio.com/docs/test/performance-testing/run-performance-tests-app-before-release)

Lataa ja Suorita testi Visual Studio-Web-tiedosto:
 
0. Noudata edellä esiteltyjä avaamaan **uuden suorituskyvyn testata** -sivu.
   Valitse tämä sivu CONFIGFURE TESTATA käyttämällä-vaihtoehto, jos haluat avata **Määritä testin avulla** -sivu.  

    ![Määritä kuormituksen testaaminen sivu avaaminen](./media/app-service-web-app-performance-test/multiple-01-authoring-blade.png)

0. Tarkista, että tyyppi on määritetty **Visual Studio Web Test** ja valitse HTTP-arkistotiedostoja.
    "Kansio"-kuvakkeen avulla voit avata tiedoston valitseminen-valintaikkunassa.

    ![Useita URL-Osoitteen Visual Studio Web Test-tiedoston lataaminen](./media/app-service-web-app-performance-test/multiple-01-authoring-blade2.png)

    Kun tiedosto on ladattu, näet URL-tiedot-kohdassa testataan URL-osoitteiden luettelon.
 
0. Määritä käyttäjän kuormitus ja testaa keston ja valitse sitten **Suorita testi**.

    ![Valitsemalla käyttäjän kuormituksen ja kesto](./media/app-service-web-app-performance-test/multiple-01-authoring-blade3.png)

    Kun testi on valmis, näet tulokset näkyvät kaksi ruutua. Vasemmassa ruudussa näkyy performnace tiedot joukkona kaavioita.

    ![Suorituskyvyn tulokset-ruutu](./media/app-service-web-app-performance-test/multiple-01a-results.png)

    Oikeanpuoleisessa ruudussa näkyy epäonnistuneiden pyyntöjen virheen tyypin ja kuinka monta kertaa, se on tapahtunut luettelo.

    ![Pyyntö virheet-ruutu](./media/app-service-web-app-performance-test/multiple-01b-results.png)

0. Suorita testi valitsemalla oikeanpuoleisen ruudun yläreunassa **Suorita** -kuvaketta.

    ![Uudestaan testi](./media/app-service-web-app-performance-test/multiple-rerun-test.png)

##  <a name="q--a"></a>Q & A

#### <a name="q-is-there-a-limit-on-how-long-i-can-run-a-test"></a>K: onko rajoitusta, kuinka kauan voin suorittaa testin? 

**A**: Kyllä, voit suorittaa ylöspäin tunnin testauksessa Azure-portaalissa.

#### <a name="q-how-much-time-do-i-get-to-run-performance-tests"></a>K: miten paljon aikaa pääsen suorituskyvyn testit? 

**A**: public preview-version, kun saat 20 000 virtual käyttäjän minuuttia (VUMs) vapaata kuukausittain Visual Studio Team Services-tilin kanssa. VUM on virtual testauksessa minuuttien määrä kerrottuna käyttäjien lukumäärä. Jos tarpeitasi ylittää vapaa, voit hankkia enemmän aikaa ja maksaa vain käyttötarkoitus.

#### <a name="q-where-can-i-check-how-many-vums-ive-used-so-far"></a>K: missä voin tarkistaa, kuinka monta VUMs olen käyttänyt mennessä?

**A**: Voit tarkistaa tämän summan Azure-portaalissa.

![Siirry Team Services-tiliin](./media/app-service-web-app-performance-test/azure-np-vso-accounts.png)

![Tarkista VUMs käytetään](./media/app-service-web-app-performance-test/azure-np-vso-accounts-vum-summary.png)

#### <a name="q-what-is-the-default-option-and-are-my-existing-tests-impacted"></a>K: mikä on oletusasetus ja oma aiemmin testien vaikuttaa?

**A**: suorituskyvyn kuormituksen testeissä oletusasetus on manuaalinen testi - sama kuin ennen useita URL-Osoitteen Testaa asetus on lisätty portaaliin.
Olemassa olevan testien edelleen käyttää määritetyn URL-osoite, ja ne toimivat kuin aiemmin.

#### <a name="q-what-features-not-supported-in-the-visual-studio-web-test-file"></a>K: mitä ominaisuuksia ei tueta Visual Studio Web Test-tiedosto?

**A**: tällä hetkellä tätä ominaisuutta ei tue Web-testi laajennukset, tietolähteet ja poiminnan säännöt. Muokkaa WWW-testi-tiedostosi, jotta Poista nämä. Toivottavasti Lisää tuki näissä toiminnoissa tulevaisuudessa päivitykset.

#### <a name="q-does-it-support-any-other-web-test-file-formats"></a>K: se tukee muiden verkko-testi tiedostomuotojen?
  
**A**: on käytettävissä vain Visual Studio Web Test muotoisten tiedostojen tuetaan.
Olemme olisi ILO kuulla Jos tarvitset muiden tiedostomuotojen tukea. Sähköpostin osoitteeseen [vsoloadtest@microsoft.com](mailto:vsoloadtest@microsoft.com).

#### <a name="q-what-else-can-i-do-with-a-visual-studio-team-services-account"></a>K: mitä-muita Visual Studio Team Services-tilin kanssa voi tehdä?

**A**: Ota uusi tili valitsemalla ```https://{accountname}.visualstudio.com```. Jakaa koodi, luoda, Testaa, seuraavat töitä ja toimitus-ohjelmisto – kaikki pilvipalvelussa työkalua tai kieli. Lue lisää siitä, miten [Visual Studio Team Services](https://www.visualstudio.com/products/what-is-visual-studio-online-vs) -ominaisuuksia ja palveluita avulla työryhmän yhteistyötä ja ota käyttöön jatkuvasti.

## <a name="see-also"></a>Katso myös

* [Yksinkertainen cloud suorituskyvyn testit](https://www.visualstudio.com/docs/test/performance-testing/getting-started/get-started-simple-cloud-load-test)
* [Apache Jmeter suorituskyvyn testien suorittaminen](https://www.visualstudio.com/docs/test/performance-testing/getting-started/get-started-jmeter-test)
* [Tallentaminen ja uudelleen toistumaan pilvipohjainen kuormituksen testit](https://www.visualstudio.com/docs/test/performance-testing/getting-started/record-and-replay-cloud-load-tests)
* [Suorituskyvyn testata sovelluksen pilveen](https://www.visualstudio.com/docs/test/performance-testing/getting-started/getting-started-with-performance-testing)

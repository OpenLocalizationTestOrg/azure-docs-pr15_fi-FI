<properties
    pageTitle="Hei maailma pilvipalveluun varten Azure luominen Pimennys"
    description="Lue, miten voit luoda yksinkertaisen Hei maailma sovelluksen Pimennys Azure-työkalujen avulla."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690944.aspx -->

# <a name="create-a-hello-world-cloud-service-for-azure-in-eclipse"></a>Hei maailma pilvipalveluun varten Azure luominen Pimennys #

Seuraavia ohjeita noudattamalla voit luoda ja ottaa käyttöön basic JSP-sovelluksen Azure Pimennys Azure-työkalujen avulla. JSP esimerkissä näkyy yksinkertaisuuden, mutta erittäin vastaavat vaiheet on vastaava Java-servlet Azure käyttöönoton osalta.

Sovelluksen näyttää seuraavankaltaiselta:

![][ic600360]

## <a name="prerequisites"></a>Edellytykset ##

* Java Developer Kit (JDK), v 1.7 tai uudempi.
* Pimennys IDE Java ss kehittäjille Indigo tai uudempi versio. Tämä voi ladata <http://www.eclipse.org/downloads/>.
* Java-pohjainen web-palvelimeen tai sovelluspalvelin, kuten Apache Tomcat, GlassFish, JBoss sovelluspalvelin, jota tai IBM® WebSphere® sovelluksen Server Liberty Core jakauman.
* Azure tilauksen, joka on saatu <http://azure.microsoft.com/pricing/purchase-options/>.
* Pimennys Azure työkalut. Lisätietoja on artikkelissa [asentamista varten Pimennys Azure-Työkalut][].

## <a name="to-create-a-hello-world-application"></a>Hei maailma-sovelluksen luominen ##

Ensin asetetaan ensin Java projektin luomisesta.

*  Käynnistä Pimennys, ja valitse **Tiedosto**valikosta, **Uusi**ja valitse sitten **Dynaamiset Web-projekti**. (Jos et näe **Dynaaminen Web-projekti** peruutuksesta käytettävissä projektin jälkeen valitsemalla **Tiedosto**, **Uusi**, toimi seuraavasti: Valitse **Tiedosto**, **Uusi**, valitse **projektia**, laajenna **Web**, **dynaaminen Web**-projekti ja valitse **Seuraava**.)
*  Tässä opetusohjelmassa tarkoituksiin nimeä projektin **MyHelloWorld**. (Varmistaa nimeä, seuraavat vaiheet Tässä opetusohjelmassa toiminta SODAN tiedoston nimenä MyHelloWorld). Näyttöön tulee näkyviin seuraavankaltaiselta: ![][ic589576]
* Valitse **Valmis**.
* Laajenna **MyHelloWorld**Pimennys 's Project Explorer-näkymässä. **WebContent**hiiren kakkospainikkeella, valitse **Uusi**ja valitse sitten **JSP tiedosto**.
* Nimeä tiedosto **index.jsp** **JSP uusi tiedosto** -valintaikkunassa. Ota pääkansion **MyHelloWorld/WebContent**kuin seuraavassa esitetyllä tavalla:  ![][ic659262]
* **Valitse JSP malli** -valintaikkunassa tarkoitetaan tässä opetusohjelmassa valitsemalla **Uusi JSP-tiedosto (html)** ja valitse **Valmis**.
* Kun index.jsp-tiedosto avautuu Pimennys, Lisää teksti dynaamisesti **Hei maailma!** sisällä nykyisen `<body>` elementti. Oman päivitetyt `<body>` sisältö tulee olla seuraavat:
```
    <body>
    <b><% out.println("Hello World!"); %></b>
    </body>
```
* Tallenna index.jsp.

## <a name="to-deploy-your-application-to-azure-the-quick-and-simple-way"></a>Ottaa käyttöön sovelluksen Azure, voit helposti ja nopeasti ##

Heti, kun sinulla on valmis kokeilemaan Java web-sovelluksen, voit kokeilla suoraan Azure pilveen seuraavat pikakuvaketta.

1. Valitse Pimennys 's Project Explorer **MyHelloWorld**.
1. Pimennys-työkalurivin **Julkaise** avattava painike ja valitse sitten **Julkaise kuin Azure pilvipalvelussa**
    ![][publishDropdownButton]
1. Jos et ole luonut Azure käyttöönotto-projekti ennen tämän sovelluksen julkaiset tämän sovelluksen Azure ensimmäistä kertaa, Azure-käyttöönotto-projektin luodaan automaattisesti puolestasi. Pitäisi näkyä seuraava kehote, jossa näkyvät myös JDK paketin ja sovelluspalvelin, joka otetaan automaattisesti käyttöön oman sovelluksen käyttämiseen.
    ![][ic789598]

    Pikavalikon tämän menetelmän avulla nopea ja helppo tapa, voit esikatsella sovelluksen Azure eikä sinun tarvitse tiettyä server tai muu kuin oletusarvoiset JDK määrittäminen. Jos ole tyytyväinen oletusasetuksia, voit valita **OK** , jos haluat jatkaa seuraavien ohjeiden mukaisesti.
    Jos haluat muuttaa JDK tai sovelluspalvelin-sovelluksen käyttäminen, jotka voit tehdä myöhemmin muokkaamalla Azure-käyttöönotto-projektin, joka on luotu automaattisesti, tai voit napsauttaa **Peruuta** nyt ja lue **Lisätietoja Azure käyttöönoton projektiosasta** Tässä opetusohjelmassa.
1. **Julkaise Azure** -valintaikkunassa:
    1. Jos ei ole tilaaminen **tilauksen** luettelosta, mutta voit tuoda tilauksen tiedot seuraavasti:
        1. Valitse **Tuo tiedostosta JULKAISE-asetukset**.
        1. Valitse **Tuo tilauksen tiedot** -valintaikkunasta **Lataa JULKAISE-asetustiedosto**. Jos et ole vielä kirjautunut Azure-tili, sinua pyydetään kirjautumaan. Valitse sinua pyydetään Tallenna Azure julkaista asetustiedosto. Tallentaa paikallisessa tietokoneessa.
        1. Edelleen: **Tuo Tilaustiedot** -valintaikkunasta **Selaa** -painiketta, valitse edellisessä vaiheessa paikallisesti tallennetun Julkaise asetukset-tiedosto ja valitse sitten **Avaa**. Näytössä pitäisi näyttää seuraavankaltaiselta: ![][ic644267]
        1. Valitse **OK**.
    1. **Tilauksen**Valitse tilaus, jota haluat käyttää käyttöönottoa varten.
    1. **Tallennustilan tilin**Valitse tallennustilan tili, jota haluat käyttää tai valitse **Uusi** , jos haluat luoda uuden tallennustilan tilin.
    1. **Palvelunimi**Valitse pilvipalvelussa, jota haluat käyttää tai valitse **Uusi** ja luo uusi pilvipalvelussa.
    1. Valitse **Kohde-Käyttöjärjestelmä**, jota haluat käyttää käyttöönoton käyttöjärjestelmän versio.
    1. Valitse **väliaikaisen** **kohde-ympäristössä**, tarkoitetaan tässä opetusohjelmassa. (Kun olet valmis ottamaan tuotannon sivustoon, muutat tämä **tuotannon**.)
    1. Valinnainen: Varmista, että **Korvaa edellisen käyttöönoton** on valittuna, jos haluat korvata automaattisesti edellisen käyttöönoton uudet käyttöönoton. Kun otat tämän asetuksen, se ei ole kokemusta "409 ristiriidan" ongelmat julkaistessasi samaan sijaintiin.
        Huomaa, että **Julkaise Azure** -valintaikkunan sisältää osan **Etäkäyttöä**varten. Oletusarvon mukaan etäkäyttöpalvelimen ei ole otettu käyttöön ja olemme eivät salli sitä tässä esimerkissä. Jotta etäkäyttöpalvelimen, sinun on kirjoitettava käyttäjänimi ja salasana, jota käytetään kirjauduttaessa etäyhteyden välityksellä. Saat lisätietoja etäkäyttöpalvelimen [Ottaminen käyttöön etäkäyttöpalvelimen Azure-versioiden Pimennys][].
        **Julkaise Azure** -valintaikkuna tulee näkyviin seuraavankaltaiselta: ![][ic719488]
1. Valitse **Julkaise** julkaiseminen väliaikaisen-ympäristössä.
    Kun sinua suorittamaan koko muodosta, valitse **Kyllä**. Tämä voi kestää useita minuutteja ensimmäisen muodosta.
    **Azure tehtävän loki** näkyy Pimennys välilehtien näkymät-osio.
    ![][ic719489]
   Voit käyttää lokia sekä **Console** -näkymässä näet käyttöönoton etenemisen. Löytämiseen on [Azure hallinta-portaalin][]kirjautuminen ja **Cloud Services** -osan avulla voit seurata tilaa.
1. Jos käyttöönoton on otettu, **Azure tehtävän loki** näkyy tila on **julkaistu**. Valitse **julkaistu**seuraavassa kuvassa esitetyllä tavalla ja selain avautuu käyttöönoton esiintymä.
    ![][ic719490]

Koska tämä on väliaikainen ympäristöön käyttöönoton, DNS-nimi on lomakkeen http://&lt;*guid*&gt;. cloudapp.net ja sisältää DNS-nimi sekä loppuliite sovelluksen URL-osoite avautuu. Esimerkiksi http://447564652c20426f6220526f636b7321.cloudapp.net/MyHelloWorld. ( **MyHelloWorld** osa on merkitsevä.) Näet myös DNS-nimi, jos napsautat käyttöönoton nimeä Azure ympäristö hallinta-portaalissa (sisällä hallinta-portaalin pilvipalveluihin osa).

Vaikka tämä hallintapaketteihin oli väliaikaisen ympäristön käyttöönottoa, käyttöönoton tuotannon seuraa samat vaiheet, paitsi sisällä **Julkaise Azure** -valintaikkuna, valitse sen sijaan, että **väliaikaisen** **tuotannon** **kohde-ympäristössä**. Käyttöönoton tuotannon hakutulosten URL-Osoitteen mediaan sijaan GUID-tunnus, kuten väliaikaisesta käytettäviä DNS-nimen perusteella.

>[AZURE.WARNING] Tässä vaiheessa olet ottanut Azure sovelluksen pilveen. Kuitenkin ennen jatkamista, huomaat, että-sovellus otettu käyttöön, vaikka se ei ole käynnissä, säilyvät kertymä tilauksen laskutettavan ajan. Tämän vuoksi on erittäin tärkeää poistaa tarpeettoman ominaisuuksissa Azure tilauksesta.

## <a name="about-azure-deployment-projects"></a>Azure käyttöönoton projekteista ##

Yhden tai useamman Azure Java-sovellusten käyttöön, jotta Azure käyttöönotto-projektin tarvita. Tehoste toistetaan "paketti" vaikuttavat sovellustesi jotta voi julkaista Azure kyselyjä rivitetty rooli.

Lisäksi sovellusten tietoja Azure-käyttöönotto-projektin sisältää tietoja myös muita pääosat järjestelmän käyttöönottoa tärkeintä: sovelluksen palvelimen säilön ja suorita-koodiin ja suorittaa sen Java-runtime. Azure tukee Java CRT Runtime ja Java-sovelluksen palvelinten voit valita suuria valintaa.

Vaikka käyttää tässä esimerkissä on yksinkertaistettu huomattavasti opetustarkoituksiin, Azure-käyttöönotto-projektin voi sisältää myös muita tärkeitä kokoonpanotietoja, jonka avulla voit luoda lähes satunnaisesti monimutkaisia, skaalattava, helposti saatavilla, monitasoisten pilvipalveluihin sovellusten. Voit ottaa **istunnon affiniteetti ("muistilappuja istuntoa")**, **Nopea tallentamisesta välimuistiin**, **remote virheenkorjaus**, **SSL purkaminen**, **palomuuri/portti reititys**, **etäkäyttöpalvelimen**ja useita muita tehokkaita ominaisuuksia.

Jos olet suorittanut edellisen osan Tässä opetusohjelmassa ("ottamaan Azure-sovellusta helposti ja nopeasti"), näkyvät nyt Azure uuden käyttöönottoprojektin Project Explorer luomaa automaattisesti ja nimeltä "**MyHelloWorld_onAzure**".

Sinun on myös käynnistäminen Tässä opetusohjelmassa luomalla ensin tyhjä Azure käyttöönottoprojektin itse ja lisäämällä siihen oman sovellukset. Se on pidemmän prosessin, mutta jossa voit määrittää tarkemmin alkuperäiset määritykset alusta.

Azure uuden käyttöönottoprojektin luominen alusta alkaen, napsauttamalla painiketta **Uuden Azure käyttöönottoprojektin** ![][ic710876].

Aiemmin luodusta Azure käyttöönoton hoidetuissa, vai luominen alusta alkaen, vaikka olet voi muuttaa sen asetuksia ja osia, kuten JDK tai sovelluspalvelin-tasaisesti helposti milloin tahansa.

Voit muuttaa JDK tai sovelluspalvelin tai Azure käyttöönotto-Projectissa sovellusluettelo:

1. Laajenna Project Explorer project-solmu (kuten **MyHelloWorld_onAzure**)
2. Napsauta hiiren kakkospainikkeella **WorkerRole1**
3. Laajenna pikavalikon **Azure** -alivalikko
4. Valitse **Server-määritys**

Riippumatta siitä, loitko server configuration seuraavasti muokkaamalla Azure aiemmin luodun käyttöönottoprojektin kuin yllä tai luomalla uuden alusta, tulee näkyviin keskustelut, jonka avulla voi määrittää JDK, palvelin-ja sovelluksen samantyyppisten. Saat lisätietoja näiden valintaikkunat, esimerkiksi muuttaa JDK sovelluspalvelin ja Lisää tai poista sovellusten käyttöönoton-asetusten muuttamisesta on artikkelissa [Server configuration ominaisuudet][] .

## <a name="windows-only-to-deploy-your-application-to-the-compute-emulator"></a>Vain Windows: Laske emulaattori sovellusta ottamaan ##

>[AZURE.NOTE] Azure emulaattorin on käytettävissä vain Windows. Ohittaa tämän osan, jos käytät kuin Windows käyttöjärjestelmä.

Jos olet luonut uuden Azure käyttöönottoprojektin ohjeiden aiemmin, eli implisiittisesti julkaisemalla sovelluksesi Azure, JDK ja sovelluksen palvelimet on määritetty pilveen, mutta ei paikallisen emuloinnin. Paikallinen emulaattori testaaminen projektin valmisteleminen, toimi seuraavasti:

1. Valitse Pimennys 's Project Explorer **MyHelloWorld_onAzure**.
1. Napsauta hiiren kakkospainikkeella **WorkerRole1**.
1. Laajenna **Azure** -alivalikko, valitse pikavalikosta.
1. Valitse **Server-määritys**.
1. Tarkista **JDK** -välilehdessä, jos työkalujen on ennalta määritetty oletusarvoinen paikallisen JDK puolestasi. Jos näin ei ole tai jos haluat muuttaa oletettu oletusasetuksia, varmista, että **Käytä tämän tiedoston polun testikäyttöön paikallisesti JDK** -valintaruutu on valittuna eivätkä JDK asennuksen sijainti, jota haluat käyttää on määritetty. Jos haluat muuttaa, valitsemalla **Selaa** ja Selaa-ohjausobjektin avulla, valitse käyttämään JDK kansion sijainti.
1. Valitse **palvelin** -välilehti.
1. Kirjoita **polku paikallisen palvelimen** tekstiruutuun valintaikkunan alareunassa, paikallisesti asennettu palvelimeen, tyyppi ja palvelimen yläosassa valintaikkunan **käyttöönotto tämäntyyppisen palvelin** -valintaruutu valittuna pääversionumero ja polun. Jos haluat käyttää eri tai pääversion sovelluspalvelin, muuttaa kohdassa kyseisen valintaruudun valinta.
1. Valitse **OK**.
1. Napsauta Pimennys-työkalurivin **Azure emulaattorin Suorita** -painiketta, ![][ic710879]. Jos **Azure emulaattorin Suorita** -painike ei ole käytössä, varmistaa, että **MyHelloWorld_onAzure** on valittuna Pimennys 's Project Explorer ja varmista, että käyttäjän Pimennys Project Explorer on kohdistus nykyisen ikkunan. Tämä ensin koko muodosta projektin aloittaminen ja käynnistä sitten Laske emulaattori-Java web-sovelluksen. (Huomaa, että tietokoneen suorituskyky-ominaisuudet, mukaan ensimmäisen muodosta saattaa kestää muutaman minuutin kuluttua muutaman sekunnin, mutta myöhemmin muodostaa saavat nopeammin.) Kun muodosta ensimmäiseksi on valmis, voit pyydetään mukaan Windows käyttäjän ohjausobjektin Käyttäjätilien sallimaan tällä komennolla voit tehdä muutoksia tietokoneeseen. Valitse **Kyllä**.

>[AZURE.IMPORTANT] Jos käyttäjätilien valvonta-kehote ei ole näkyvissä, valitse Käyttäjätilien valvonta-kuvaketta Windowsin tehtäväpalkissa ja valitse se ensin. Joskus Käyttäjätilien kehote ei näy ylin ikkunassa, mutta se on näkyvissä vain tehtäväpalkin kuvakkeena.

1. Tarkastele tulosteen Laske emulaattorin Käyttöliittymän määrittääksesi, onko projektin ongelmia. Sisällön käyttöönoton voi kestää muutaman minuutin sovelluksen alkamaan täysin Laske emulaattori kuluessa.
1. Käynnistä selain ja käyttää URL-Osoitetta `http://localhost:8080/MyHelloWorld` osoitteeksi ( `MyHelloWorld` URL-osoitteen kirjainkoko on merkitsevä). Näkyviin tulee MyHelloWorld sovelluksen (index.jsp tulosteen), samalla tavalla kuin seuraavassa kuvassa: ![][ic589579]

Kun olet valmis, Lopeta sovelluksesi suorittamasta Laske-emulaattorin Pimennys työkalurivillä napsauttamalla **Palauta Azure emulaattorin** -painiketta, ![][ic710880].

## <a name="to-delete-your-deployment"></a>Jos haluat poistaa koko käyttöönotto ##

Poista käyttöönottoa varten Pimennys Azure-työkalujen sisällä, varmista, että **MyHelloWorld_onAzure** on valittu käyttäjän Pimennys Project Explorer, että Pimennys Project Explorer on nykyisen ikkunan tietotasoja ja valitse sitten **julkaisu** -painiketta, ![][ic710883], Pimennys työkalurivillä. (Sama toiminto voi tehdä **MyHelloWorld_onAzure** Pimennys 's Project Explorer-hiiren kakkospainikkeella, valitsemalla **Azure** ja valitsemalla sitten **Undeploy Azure pilvestä**.) Tässä näkyvät **Julkaisun Azure projekti** -valintaikkuna.

![][ic719491]

Valitse tilaus ja pilvi palvelu, joka sisältää käyttöönoton, valitse käyttöönoton, jonka haluat poistaa ja valitse sitten **Poista julkaisu**.

(Vaihtoehtoinen työkalujen avulla voit poistaa käyttöönoton on käyttää Azure hallinta-portaalin **Cloud Services** -osa: Siirry käyttöönoton, valitse se ja valitse sitten **Poista** -painiketta. Tämä lopettaa, ja poista-käyttöönotto. Jos haluat vain lopettaa käyttöönotto ja poista se, valitse **Poista** -painiketta sijaan **Pysäytä** -painike, mutta kuten edellä mainittiin, jos et poista käyttöönotto-laskutettavan ovat edelleen kertymä käyttöönottoa varten, vaikka se pysäytetään).

## <a name="see-also"></a>Katso myös ##

[Azure työkalujen Pimennys varten][]

[Azure-työkalut asennetaan Pimennys][] 

[Mitä uusia ominaisuuksia, Pimennys Azure Työkalut][]

Saat lisätietoja Azure käyttäminen Java [Azure Java Developer Center][].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure hallinta-portaalissa]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure Role Properties]: http://go.microsoft.com/fwlink/?LinkID=699525
[Azure työkalujen Pimennys varten]: http://go.microsoft.com/fwlink/?LinkID=699529
[Azure-Pimennys versioiden ottaminen käyttöön etäkäyttö]: http://go.microsoft.com/fwlink/?LinkID=699538
[Azure-työkalut asennetaan Pimennys]: http://go.microsoft.com/fwlink/?LinkId=699546
[Ominaisuudet: palvelimen asetukset]: http://go.microsoft.com/fwlink/?LinkID=699525#server_configuration_properties
[Mitä uusia ominaisuuksia, Pimennys Azure Työkalut]: http://go.microsoft.com/fwlink/?LinkID=699552

<!-- IMG List -->

[ic589576]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic589576.png
[ic589579]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic589579.png
[ic600360]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic600360.png
[ic644267]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic644267.png
[ic659262]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic659262.png
[ic710876]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710876.png
[ic710879]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710879.png
[ic710880]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710880.png
[ic710882]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710882.png
[ic710883]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710883.png
[ic719488]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719488.png
[ic719489]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719489.png
[ic719490]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719490.png
[ic719491]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719491.png
[ic789598]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic789598.png
[publishDropdownButton]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/publishDropdownButton.png

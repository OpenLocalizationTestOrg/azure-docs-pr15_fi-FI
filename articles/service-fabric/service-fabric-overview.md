<properties
   pageTitle="Yleistä palvelun kangasta | Microsoft Azure"
   description="Yleiskatsaus palvelun kangasta, jossa sovellusten koostuvat asteikko sekä toimintakykyyn on monta microservices. Palvelun kangasta on jaettu systems-ympäristö luodaan skaalattava, luotettavia ja hallita helposti pilveen sovellukset"
   services="service-fabric"
   documentationCenter=".net"
   authors="msfussell"
   manager="timlt"
   editor="masnider"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/22/2016"
   ms.author="mfussell"/>

# <a name="overview-of-service-fabric"></a>Palvelun kangasta yleiskatsaus
Palvelun kangasta on jaettu systems-ympäristö, joka on helppo paketointi, ottaminen käyttöön ja hallita skaalattava ja luotettava microservices. Palvelun kangasta korjaa myös merkittäviä haasteita kehittäminen ja cloud sovellusten hallinta. Kehittäjät ja järjestelmänvalvojat voivat välttää ratkaiseminen monimutkaisia infrastruktuurin ongelmia ja kohdistus sijaan tietää, että ne ovat skaalattava, luotettava ja helpommin hallittaviin kriittisten, vaativia työmääriä yksityiskohtaisista. Palvelun kangasta edustaa seuraavan sukupolven middleware-ympäristön luomiseen ja näiden yritysluokan, taso 1 cloud-akseli sovellusten hallinta.

Tässä [lyhyessä videossa](https://aka.ms/servicefabricvideo) esitellään palvelun kangasta ja microservices.


## <a name="applications-composed-of-microservices"></a>Sovellusten koostuva microservices
Palvelun kangasta avulla voit luoda ja jaetun resurssivarantoon tietokoneissa (jota kutsutaan klusterin) on erittäin suuri tiheyden käytössä microservices koostuva skaalattava ja luotettavaa sovellusten hallinta. Se sisältää kehittyneitä runtime etsimisen hajautettu, skaalattava tilattomien ja tilallisten microservices. Se tarjoaa täydellinen sovelluksen hallintatoiminnot myös valmistelu, käyttöönotosta, seuranta, päivittäminen ja päivitetään ja poistetaan käyttöön sovellukset.

Miksi microservices lähestymistapa on tärkeä? Kaksi tärkeintä syytä ovat seuraavat:

1. Niillä voi skaalata eri osia sovelluksen tarpeiden mukaan.

2. Kehitystyöryhmiä voivat-juoksevan tehtyjä muutoksia tietoresurssien ja tarjota näin ominaisuuksia asiakkaille entistä nopeammin ja useammin.

Palvelun kangasta tehostaa useita Microsoft services tänään, mukaan lukien Azure SQL-tietokanta, Azure DocumentDB, Cortana, Power BI, Microsoft Intune, Azure tapahtuman keskittimet, Azure IoT, Skype for Business ja monia core Azure palvelut.

Palvelun kangasta räätälöity luominen cloud alkuperäisen palvelut, joita voit aloittaa pienenä tarpeen mukaan ja valtaviin asteikkoa, jossa satoja tai tuhansia koneet kasvaa.

Tämän päivän Internet-akseli-palvelut on suunniteltu, microservices. Microservices on esimerkkejä protokolla yhdyskäytävät, käyttäjäprofiilien ostokset carts, varaston käsittelyn olevien, ja tallentaa välimuistiin. Palvelun kangasta on microservices-ympäristö, jonka avulla kaikki microservice yksilöivä nimi, joka voi olla tilattomien tai tilallisten.

Palvelun kangasta on täydellinen runtime ja elinkaari hallintatoiminnot koostuu näistä microservices sovelluksia. Nimeksi isännät microservices sisällä säilöjen käyttöön ja aktivoida palvelua kangasta klusterin yli. Siirtyvät VMs säilöjen tekee mahdollista sitä tilauksen lisäys-arvon. Vastaavasti toisen tilauksen määrän suuruus avulla on mahdollista siirtämällä säilöjen microservices. Yksittäisen Azure SQL-tietokanta-klusterin koostuu esimerkiksi satoja, joissa käytetään kymmeniä tuhansia säilöjen isännöinnin tietokantojen tuhansia satoja yhteensä. Kunkin tietokanta on palvelun kangasta tilallisten microservice. Sama on tosi kerrottiin, muiden palvelujen, joka on miksi termi "hyperscale" käytetään kuvaamaan palvelun kangasta ominaisuuksia. Jos säilöjen antaa HD, valitse microservices antaa hyperscale.

Lue lisätietoja microservices tavasta [Miksi microservices-lähestymistavan sovellusten rakentamiseen?](service-fabric-overview-microservices.md)

## <a name="container-deployment-and-orchestration"></a>Säilön käyttöönotto- ja tiedonsiirron
Palvelun kangasta on [orchestrator](service-fabric-cluster-resource-manager-introduction.md) , microservices koneet klusterin. Microservices voidaan kehittää käyttämästä [palvelun kangasta ohjelmoinnin mallien](service-fabric-choose-framework.md) käyttöönotto [Vieras suoritettavat](service-fabric-deploy-existing-app.md)monella tavalla. Palvelun kangasta voit ottaa käyttöön palvelut säilö kuvat ja seurannan aikana voit sekoitetaan prosessit Services-palvelujen ja säilöjen yhdessä samalla sovelluksessa palvelut. Jos haluat vain [käyttöön ja hallita säilö kuvia](service-fabric-containers-overview.md) eri tietokoneissa klusterin, palvelun kangasta on tämän sopivaa vaihtoehtoa.


## <a name="create-service-fabric-clusters-anywhere"></a>Luo palvelun kangasta klustereiden missä tahansa
Voit luoda palvelun kangasta klustereiden monta ympäristöissä, mukaan lukien Azure tai paikallinen, Windows Server- tai Linux. Lisäksi kehitysympäristö SDK: ssa on sama kuin tuotantoympäristössä ei emulointeja osallistuvien kanssa. Toisin sanoen se käytetään paikallista kehittämistä klusterissa se ottaa käyttöön saman klusterin muiden ympäristöjen.

Lisätietoja luomisesta klustereiden paikallinen, lue [Windows Server-tai Linux klusterin luominen](service-fabric-deploy-anywhere.md) tai Azure luomisesta klusterin [Azure portaalin kautta](service-fabric-cluster-creation-via-portal.md).

![Palvelun kangasta ympäristö][Image1]

## <a name="stateless-and-stateful-service-fabric-microservices"></a>Tilattomien ja tilallisten palvelun kangasta microservices

Palvelun kangasta avulla voit luoda sovelluksia microservices koostuvat. Tilattomien microservices (protokolla yhdyskäytävät, web-välityspalvelimet jne.) ei enää mutable tila tietyllä pyyntö ja sen vastaus-palvelusta. Azure pilvipalveluihin työntekijä roolit ovat esimerkiksi tilattomien palvelun. Tilallisten microservices (käyttäjätilit, tietokantoja, laitteita, ostokset carts, olevien jne.) ylläpitää tilaan mutable, tärkeimpien pyynnön ja vastauksen sen lisäksi. Tämän päivän Internet-akseli sovellusten koostuvat tilattomien ja tilallisten microservices yhdistelmä.

Miksi on tilallisten microservices sekä tilattomien niistä? Kaksi tärkeintä syytä ovat seuraavat:

1. Mahdollisuus luoda suuren siirtonopeuden, pieni viive, vikasietoisen online tapahtuman käsittely (OLTP) services säilyttämällä koodi ja sulje tiedot samaan tietokoneeseen. Esimerkkejä ovat vuorovaikutteisia storefronts, haku, asioita Internet (IoT) systems, kaupan järjestelmien, luottokortin käsittelyn ja petoksen järjestelmiä ja henkilökohtainen tietueiden hallinta.

2. Sovelluksen rakenne yksinkertaistaminen. Tilallisten microservices muita olevien tarvitse ja välimuistiin, tarvitse perinteisesti laskettuna tilattomien sovelluksen saatavuudesta ja viive vaatimusten. Tilallisten services ovat luonnollisesti suuren käytettävyyden ja pieni-viive-liikkuvat osat hallittavan sovelluksen kokonaisuutena määrän.

Saat lisätietoja sovelluksen kuviot palvelun kangasta, lue [sovelluksen skenaariot](service-fabric-application-scenarios.md) ja [valitsemalla ohjelmoinnin mallin framework](service-fabric-choose-framework.md) palvelun kanssa

## <a name="application-lifecycle-management"></a>Sovelluksen elinkaaren hallinta
Palvelun kangasta tukee pääosin koko sovelluksen elinkaari hallinnan (elinkaaren tuominen ESILLE) cloud sovellusten--kehitystä, käyttöönottoa, päivittäiset hallinnan ja ylläpito-potentiaalisen vanha voidaan poistaa käytöstä.

Palvelun kangasta elinkaaren tuominen ESILLE-ominaisuuksien avulla järjestelmänvalvojat / IT operaattoreiden yksinkertainen, pieni-touch työnkulkuja valmistelu, korjaustiedoston, asentamisesta ja sovellusten valvonta. Valmiiden työnkulkujen vähentää huomattavasti IT operaattoreiden pitämään sovellusten jatkuvasti käytettävissä olevien.

Useimpien sovellusten koostuvat tilattomien ja tilallisten microservices ja muut suoritettavat/CRT Runtime käyttöön yhdessä yhdistelmän. Pyytämällä vahva tyypit sovelluksia ja pakattu microservices palvelun kangasta ottaa käyttöön useita Palvelusovellusten esiintymät käyttöönotto. Jokaiselle esiintymälle on hallita ja päivittää itsenäisesti. Palvelun kangasta on, ota käyttöön *kaikki* suoritettavat tai Runtimen ja tarkastella niitä luotettavasta. Ottaa käyttöön esimerkiksi palvelun kangasta, ASP.NET Core 1, Node.js, Java VMs, komentosarjojen tai muu, joka muodostaa sovelluksen.

Saat lisätietoja sovelluksen elinkaaren hallinta [sovelluksen elinkaari](service-fabric-application-lifecycle.md) luku- ja minkä tahansa koodin lisäämistä nähdä [käyttöönotto Vieras, joka on suoritettava](service-fabric-deploy-existing-app.md)

## <a name="key-capabilities"></a>Tärkeimmät ominaisuudet
Palvelun kangasta avulla voit:

- Kehittää erittäin skaalattava sovelluksia, jotka ovat itsekorjautumisominaisuudet.

- Palvelun kangasta ohjelmoinnin mallin microservices koostuva sovellusten kehittämiseen. Tai isännöidä yksinkertaisesti Vieras suoritettavat ja muiden sovellusten kehysten valittua, kuten ASP.NET Core 1 tai Node.js.

- Erittäin luotettavaa tilattomien ja tilallisten microservices kehittäminen

- Ottaa käyttöön ja orchestrate säilöt sisältävät Windows säilöjen ja Docker säilöjen klusterin. Näiden säilöjen voi säilö Vieras suoritettavat tai luotettavaa tilattomien ja tilallisten microservices.  Kummassakin tapauksessa saat säilö portin host Porttimääritystä, säilön havaitsemista ja automaattinen automaattisesti.

- Yksinkertaistaa sovelluksesi rakenteen tilallisten microservices tekstin näyttäminen tallentaa ja olevien avulla.

- Ota käyttöön Azure tai Windows Server- tai Linux käytön nolla koodin muuttuu paikallisen paveikslėlis. Kirjoita kerran ja ottaa sitten palvelun kangasta klusterin.

- "Palvelinkeskuksen käyttämääsi laitteeseen" lähestymistavan kehittäminen Paikallinen kehitysympäristö on sama koodi, joka suoritetaan Azure palvelinkeskusten.

- Ota käyttöön sovellusten sekunnin kuluttua.

- Ota käyttöön sovellusten suurempi kuin näennäiskoneiden käyttöönotto satoja tai tuhansia sovellusten kohti koneen kaavan avulla.

- Ottaa käyttöön eri versioita samassa sovelluksessa rinnakkain, jokainen itsenäisesti laajennettavat.

- Hallitse elinkaari tilallisten sovellustesi ilman mitään käyttökatkot, mukaan lukien purkaa ja sitovan päivitykset.

- Käyttämällä .NET-ohjelmointirajapinnan sovellusten hallinta-Java (Linux), PowerShell, Azure CLI (Linux) tai muut liittymät.

- Päivitysten ja korjausten microservices sovelluksissa erikseen.

- Seurata ja vianmäärityksen sovellustesi kunto sekä määrittää jäsenyydelle suorittamiseen automaattinen korjaus.

- Mahdollisuus asteikko-kohtaa Skaalaus-kohdassa klusterin sekä mittakaava ylöspäin näkyvien solmujen määrän tai asteikko-luettelosta kunkin solmun tietää, että sovelluksesi automaattisesti skaalata ja jaetaan mukaan käytettävissä olevat resurssit kokoa.

- Katso itsekorjautumisominaisuudet resurssin tasaustoiminto orchestrate sovellusten uudelleenjakamista klusterin yli. Palvelu palauttaa virheet ja optimoi jakautumisen kuormituksen resursseilla perusteella.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Seuraavat vaiheet

* Lisätietoja:
    * [Miksi microservices lähestymistapa sovellusten?](service-fabric-overview-microservices.md)
    * [Termejä yleiskatsaus](service-fabric-technical-overview.md)
* Palvelun kangasta [kehitysympäristö](service-fabric-get-started.md) luomiseen  
* Palvelun [ohjelmoinnin mallin framework valitseminen](service-fabric-choose-framework.md)

[Image1]: media/service-fabric-overview/Service-Fabric-Overview.png

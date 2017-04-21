<properties
    title="required"
    pageTitle="Microsoftin artikkeleja käytetään mukautettuja korotuksia tunnisteet"
    description="Näyttää mukautetun korotuksia laajennukset, jotka mahdollistavat upotettuja videoita, muistiinpanot ja vinkkejä, Uudelleenkäytettävä sisältö ja azure.microsoft.com artikkeleja kohteet."
    services=""
    solutions=""
    documentationCenter=""
    authors="tysonn"
    manager="carolz"
    editor=""/>

<tags
    ms.service="contributor-guide"
    ms.devlang=""
    ms.topic="article"
    ms.tgt_pltfrm=""
    ms.workload=""
    ms.date="01/22/2015"
    ms.author="tysonn"/>

## <a name="markdown-for-azuremicrosoftcom"></a>Azure.microsoft.com korotuksia

Yleiset korotuksia vinkkejä on kohdassa [Korotuksia perusominaisuuksiin](https://help.github.com/articles/markdown-basics/) ja tutustu [korotuksia cheatsheet](./media/documents/markdown-cheatsheet.pdf?raw=true). Jos haluat luoda artikkelissa crosslinks korotuksia, katso [linkittäminen ohjeet] (. / create-links-markdown.md#markdown-syntax-for-acom-relative-links.md/).

Azure.microsoft.com tukee [läheisyydessä koodin lohkot](https://help.github.com/articles/github-flavored-markdown/#fenced-code-blocks) ja [syntaksi korostuksen](https://help.github.com/articles/github-flavored-markdown/#syntax-highlighting). Kuitenkin ACOM tukee vain yhden syntaksi korostuksen värimallin koodin tekstialueen kieli riippumatta.

## <a name="custom-markdown-extensions-used-in-our-technical-articles"></a>Microsoftin artikkeleja käytetään mukautettuja korotuksia tunnisteet

Tutustu artikkeleihin käyttää GitHub flavored korotuksia useimmat artikkelissa muotoilu - kohdan, linkkejä, luettelot, otsikot jne. Mutta Käytämme mukautetun korotuksia tunnisteet missä tarvitsemme azure.microsoft.com hahmontamisen sivujen tehokkaan muotoilua. Näin laajennukset on tällä hetkellä käytössä:

+ [Muistiinpanojen ja vinkkejä]
+ [Sisältää]
+ [Upotettuja videoita]
+ [Tekniikka ja platform valitsimien]

## <a name="notes-and-tips"></a>Muistiinpanojen ja vinkkejä

Voit tehdä muistiinpanoja ja vihjeet 4 erityyppisiä:

- AZURE. HUOMAUTUS
- AZURE. VAROITUS
- AZURE. TIPss
- AZURE. TÄRKEÄÄ

###<a name="usage"></a>Käyttö
Käytä, muistiinpanot ja vinkkejä toimintoa harkiten artikkeleita koko. Kun käytät niitä, valitse haluamasi haluamasi muistiinpanon tai Vihje:

- Käytä AZURE. Huomautus Voit korostaa neutraalit tai positiivinen tietoja, joka korostaa tai täydentää päätekstin tärkeimmistä asioista. Muistiinpanon antaa tietoja, jotka koskevat vain tietyissä tapauksissa.

  ![](./media/custom-markdown-extensions/Notes-note.PNG)

- Käytä AZURE. Varoitus ilmoittamaan käyttäjän tilanteeseen, jossa saattaa aiheuttaa ongelmia myöhemmin. Esimerkiksi valitsemalla jokin vaihtoehto tai teet tiettyjä valinnan saattaa pysyvästi lukita voit takaisin tietyn skenaarion.

  ![](./media/custom-markdown-extensions/Notes-warning.PNG)

- Käytä AZURE. Vihje Voit auttaa käyttäjiä tekniikoita ja niiden erityistarpeita tekstin kuvattuja toimia. Vihje saattaa myös ehdottaa vaihtoehtoisia menetelmiä, jotka eivät välttämättä ole selvää. Vihjeitä, mutta jotka eivät ole välttämättömiä basic tietoja tekstin.

  ![](./media/custom-markdown-extensions/Notes-tip.PNG)

- Käytä AZURE. TÄRKEÄÄ tietoa, joka on tehtävän suorittamista.

  ![](./media/custom-markdown-extensions/Notes-important.PNG)

Kun nämä muistiinpanot ja vinkkejä tukevat koodin lohkot, kuvia, luetteloita ja linkkejä, yritä säilyttää muistiinpanot ja vinkkejä yksinkertainen ja selkeä. Jos tiedä luominen monimutkaisten muistiinpanojen erilaisilla muotoilua, joka voi olla toinen osa on artikkelissa päätekstin Tarvitsetko merkki. Ja artikkelista liikaa muistiinpanoja voi olla häiritsevät ja vaikea Skannaa tai lukea.

###<a name="sample-markdown"></a>Esimerkki korotuksia

Kaikki mallit näyttäminen AZURE. HUOMAUTUS. Vihje, varoituksia tai tärkeää käyttämään korvaa "Huomautus" korotuksia:

    > [AZURE.TIP]

    > [AZURE.WARNING]

    > [AZURE.IMPORTANT]

Yksittäisen kappaleen:

    > [AZURE.NOTE] Jotta voit suorittaa tässä opetusohjelmassa, on oltava aktiivinen Microsoft Azure-tili. Jos sinulla ei ole tiliä, voit luoda ilmainen kokeiluversio tili vain muutaman minuutin.

Multiparagraph:

    > [AZURE.NOTE] Jotta voit suorittaa tässä opetusohjelmassa, on oltava aktiivinen Microsoft Azure-tili.
    >
    > Jos sinulla ei ole tiliä, voit [luoda ilmainen kokeiluversio tili](http://www.windowsazure.com/pricing/free-trial/) -vain muutaman minuutin.

## <a name="includes"></a>Sisältää

Uudelleenkäytettävä teksti säilöön, tutustu GitHub sijaitsee tiedostoja, että olemme Soita "on". Kun on tekstiä, joka on käytettävä useita artikkeleissa, voit lisätä tiedoston Uudelleenkäytettävän tietojen viittaus. Sisällytä itse on yksinkertainen korotuksia (.md)-tiedosto. Se voi olla mikä tahansa kelvollinen korotuksia, esimerkiksi tekstin ja kuvien linkit. Kaikki sisältävät korotuksia tiedostojen on oltava [/ sisältää hakemiston](https://github.com/Azure/azure-content/tree/master/includes) säilö pääsivustoon. Kun artikkeli on julkaistu, Sisällytä teksti on integroitu saumattomasti julkaistun aihetta.

- Tietyn syntaksin avulla Sisällytä viittaavat.

- Olet sijoittanut Sisällytä mediatiedostojen on luotava media-kansio tietyn Sisällytä. Media-kansioista on kuuluttava azure-sisällön/sisältää ja media- [kansiossa](https://github.com/Azure/azure-content/tree/master/includes/media). Media-kansio ei saa sisältää sen pääsivustoon kuvia. Jos Sisällytä ei ole kuvia, vastaava media-kansio ei ole pakollinen.

###<a name="usage"></a>Käyttö

- Käytä sisältää useita artikkeleissa saman tekstin haluamaasi.

- Sisältää on tarkoitettu merkittävästi summien sisällön - kappaleen tai kaksi, jaettu toimintosarjan tai jaetun osan. Älä käytä niitä kaikkeen pienempi kuin lauseen; **he ovat ei tuotenimet**.

- Varmista kaikki Sisällytä teksti on kirjoitettu valmis lauseiden tai lauseita, jotka eivät ole riippuvaisia edeltävä teksti tai seuraava teksti on artikkelissa, joka viittaa Sisällytä. Ohittaa nämä ohjeet Luo untranslatable merkkijono, joka katkaisee lokalisoitu kokemus on artikkelissa. 

- Älä upota on muita sisältää. Hän ei tue julkaiseminen järjestelmän DPS.

- Älä Jaa media tiedostojen välillä. Käyttää erillisenä tiedostona kunkin Sisällytä ja artikkelissa yksilöllinen nimi. Tallenna mediatiedosto Sisällytä liittyvät media-kansioon.

- Älä käytä Sisällytä on artikkelin vain sisältö.  Sisältää on täydentävät artikkelin loput sisältöön.

- Koska kaikki sisältää on oltava / hakemistosta, joka sisältää artikkelista Sisällytä polku on aina

    .. / sisältää

- Älä toista linkin tai kuvan tiedostonimi viittaus on artikkelissa ja Sisällytä. Lisää "-Sisällytä" linkki viittauksesta tai media tiedostonimeen Vältä toistuvan viittaus:

 **Linkin viittaus**

 Muuta: odata.org,: odata.org Sisällytä

 **Kuva viittaus**

 Muuta: table.png,: taulukon include.png

###<a name="sample-markdown"></a>Esimerkki korotuksia
Lisätä Sisällytä asiakirjat-artikkelissa syntaksi on seuraava:

    [AZURE.INCLUDE [include-short-name](../includes/include-file-name.md)]

Esimerkki

    [AZURE.INCLUDE [howto-blob-storage](../includes/howto-blob-storage.md)]

Sisällytä ensimmäinen osa on Sisällytä nimi polku ja ilman .md-tunniste. Toinen osa on Sisällytä suhteellinen polku / sisältää hakemiston .md-tunniste.

###<a name="rendering"></a>Värien

Sisällytä seliteominaisuudet hahmontamisen GitHub-sivulla toimi seuraavasti:

 [AZURE. SISÄLLYTÄ ohjeet-blob-säiliö]

Hahmontamisen HTML-azure.microsoft.com, HTML-sisältää HTML tiedoston muiden yhdistetään. Kuitenkin HTML sisältää HTML kommentti alkuperäisen ja Sisällytä korotuksia tiedostonimi ja GitHub Vahvista hash. Kommentti sisältyy niin, että sisältölähde voit helposti tunnistaa ja GitHub löytyvät vianmääritystä varten:

  ![](./media/custom-markdown-extensions/include.png)


## <a name="embedded-videos"></a>Upotettuja videoita

Tutustu artikkeleja tukee artikkeleja embeddeded videoita, kunhan videot ovat Microsoftin [kanavan 9](http://channel9.msdn.com/) sivustossa. Kanavan 9 videoita on integroitu [azure.microsoft.com Video keskelle](http://azure.microsoft.com/documentation/videos/home/). Sovellus tukee tällä hetkellä ei ole liitetty YouTube-videoiden; Jos olet yhteisön osallistujan, olet Tervetuloa linkitettävän YouTubeen, jos haluat korostaa videon kirjataan siellä. Microsoft osallistujien käytettävä kanavan 9 ja videon keskelle.

### <a name="usage"></a>Käyttö

- Varmista, että video on Video Centerin.

- Kopioi videon tunnus kanavan 9 videon helpossa muodossa olevan URL-osoite tai Azure Video Centeristä. Video [http://azure.microsoft.com/documentation/videos/azure-scheduler-unusual-schedules/](http://azure.microsoft.com/documentation/videos/azure-scheduler-unusual-schedules/) video tunnus on esimerkiksi **azure-ajoitus-epätavallisia-aikatauluja**.

### <a name="syntax"></a>Syntaksi

    > [AZURE.VIDEO video-id-string]

### <a name="rendering"></a>Värien

Käytössä GitHub: [https://github.com/Azure/azure-content-pr/blob/master/articles/web-sites-backup.md](https://github.com/Azure/azure-content-pr/blob/master/articles/web-sites-backup.md)

Julkaistun artikkelin: [http://azure.microsoft.com/documentation/articles/web-sites-backup/](http://azure.microsoft.com/documentation/articles/web-sites-backup/)


## <a name="technology-and-platform-selectors"></a>Tekniikka ja ympäristö valitsimet

Käytä tekniikoista ja ympäristöistä switchers artikkeleja, kun luot useita flavors osoite erot käyttöönoton saman artikkelin tekniikoita tai ympäristöjen välillä. Tämä on yleensä parhaiten koskevat Microsoftin mobiilisovellusten ympäristön sisällön kehittäjille. Tällä hetkellä kahta erilaista valitsimia, [Yksinkertainen valitsimien](#simple-selectors) ja [Kaksisuuntainen valitsimia](#two-way-selectors).

Koska sama valitsin korotuksia siirtyy aihe valitulla alueella, on suositeltavaa siten keskustelun aiheen rivinvalitsinta Sisällytä sitten viittaava, Sisällytä kaikista aiheista, jotka käyttävät samaa valitseminen.

###<a id="simple-selectors"></a>Yksinkertainen valitsimet

Yksinkertainen (yksisuuntainen) valitsimien hahmonnetaan joukkona valintanappien oikealle otsikon alapuolella. Käytä painikkeet, kun asiakkaat on valittavana ympäristö tai tekniikka yhtenäisiä, kuten .NET, Node.js ja Java aiheet.  Käyttää mukautettuja korotuksia tunniste minkä tahansa valitsimien - Älä käytä HTML valitsimia.  

Katso, näet, miten tekijän luotu saman artikkelin, mutta käytetyt valitsimien käyttöön siirtyminen ne kaikki yli 8 versioiden [ilmoituksen keskittimet käytön aloittaminen](http://azure.microsoft.com/documentation/articles/notification-hubs-windows-phone-get-started/) .

![Yksinkertainen valitsin-Esimerkki](./media/custom-markdown-extensions/selectors.PNG)

####<a name="syntax"></a>Syntaksi

    > [AZURE.SELECTOR]
    - [Linkittää #1 otsikon](link #1 url)
    - [linkki #2 otsikko](link #2 url)

Esimerkki:

    > [AZURE.SELECTOR]
    - [Yleinen Windows](../articles/notification-hubs-windows-store-dotnet-get-started/)
    - [Windows Phone](../articles/notification-hubs-windows-phone-get-started/)
    - [iOS](../articles/notification-hubs-ios-get-started/)
    - [Android](../articles/notification-hubs-android-get-started/)
    - [Kindle](../articles/notification-hubs-kindle-get-started/)
    - [Baidu](../articles/notification-hubs-baidu-get-started/)
    - [Xamarin.iOS](../articles/partner-xamarin-notification-hubs-ios-get-started/)
    - [Xamarin.Android](../articles/partner-xamarin-notification-hubs-android-get-started/)

#### <a name="rendering"></a>Värien

Yllä oleva kuva näkyy värien azure.microsoft.com. Hahmontamisen GitHub sivuilla valitsimien hahmonnetaan luettelomerkeillä varustetun luettelon linkkejä.

###<a id="two-way-selectors"></a>Kaksisuuntainen valitsimet

Kaksisuuntainen valitsimien avulla käyttäjät voivat valita kaksisuuntaisen matriisissa ohjeaiheissa. Tämä on olennainen, kun Azure tekniikka Mobile-palveluihin, kuten tukee useita Taustajärjestelmä ympäristöjen sekä useasta asiakasohjelmasta käsin. Ota huomioon seuraavat asiat:

- Kun se on tarkoitettu `(Platform | Backend)`, dropwdown tekstin nyt muokattavissa.
- Sinun ei tarvitse luettelokohteen kussakin matriisissa, mutta pystyt kohteen, jossa on olemassa aiheen URL-osoite ja ei ole kaksoiskappale.
- Linkin voi olla minkä tahansa URL-osoite, mutta se on yleensä toisen GitHub aiheen.

Katso, miten tekijän luotu saman artikkelin (9 mobiilisovelluksen alustojen ja 2 Taustajärjestelmä ympäristöt), mutta käytetyt valitsimien käyttöön siirtyminen ne kaikki yli 15 versioiden ohjeaiheessa [Mobile palveluiden käytön aloittaminen](http://azure.microsoft.com/en-us/documentation/articles/mobile-services-ios-get-started/) . Huomaa, että 3 artikkeleita ei ole Taustajärjestelmä molemmat versiot.

![Kaksisuuntainen valitsimien Esimerkki](./media/custom-markdown-extensions/selector-list.png)

####<a name="syntax"></a>Syntaksi

    > [AZURE. VALITSIN-luettelo (Dropdown1 | Dropdown2)]     -  [(Dropdown1Text1 | Dropdown2Text1)](../articles/dropdown1-text1-dropdown2-text1.md)
    - [(Dropdown1Text1 | Dropdown2Text2)](../articles/dropdown1-text1-dropdown2-text1.md)
    - [(Dropdown1Text2 | Dropdown2Text3)](../articles/dropdown1-text1-dropdown2-text1.md)
    - [(Dropdown1Text3 | Dropdown2Text4)](../articles/dropdown1-text1-dropdown2-text1.md)

Esimerkki:

    > [AZURE. VALITSIN-luettelo (Platform | Taustajärjestelmä)]     -  [(iOS | .NET)](./mobile-services-dotnet-backend-ios-get-started-push.md)
    - [(iOS | JavaScript)](./mobile-services-javascript-backend-ios-get-started-push.md)
    - [(Windows yleinen C# | .NET)](./mobile-services-dotnet-backend-windows-universal-dotnet-get-started-push.md)
    - [(Windows yleinen C# | JavaScript)](./mobile-services-javascript-backend-windows-universal-dotnet-get-started-push.md)
    - [(Windows Phone | .NET)](./mobile-services-dotnet-backend-windows-phone-get-started-push.md)
    - [(Windows Phone | JavaScript)](./mobile-services-javascript-backend-windows-phone-get-started-push.md)
    - [(Android | .NET)](./mobile-services-dotnet-backend-android-get-started-push.md)
    - [(Android | JavaScript)](./mobile-services-javascript-backend-android-get-started-push.md)
    - [(Xamarin iOS | JavaScript)](./partner-xamarin-mobile-services-ios-get-started-push.md)
    - [(Xamarin Android | JavaScript)](./partner-xamarin-mobile-services-android-get-started-push.md)

#### <a name="rendering"></a>Värien

Yllä oleva kuva näkyy värien azure.microsoft.com. Hahmontamisen GitHub sivuilla valitsimien hahmonnetaan luettelomerkeillä varustetun luettelon linkkejä.

<!--Anchors-->
[Muistiinpanojen ja vinkkejä]: #notes-and-tips
[Sisältää]: #includes
[Upotettuja videoita]: #embedded-videos
[Tekniikka ja ympäristö valitsimet]: #technology-and-platform-selectors

###<a name="contributors-guide-links"></a>Osallistujat-opas linkit

- [Yleistä artikkelissa](./../README.md)
- [Indeksi ohjeet artikkeleista](./contributor-guide-index.md)

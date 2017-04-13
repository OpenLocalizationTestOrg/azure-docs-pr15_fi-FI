<properties
    pageTitle="Lataa testata sovelluksen Visual Studio ryhmän Services-palveluiden avulla | Microsoft Azure"
    description="Opettele kuormituksen testi Azure palvelun kangasta sovellusten Visual Studio Team Servicesin avulla."
    services="service-fabric"
    documentationCenter="na"
    authors="cawams"
    manager="timlt"
    editor="" />

<tags
    ms.service="multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="multiple"
    ms.date="07/29/2016"
    ms.author="cawa" />

# <a name="load-test-your-application-by-using-visual-studio-team-services"></a>Lataa testata sovelluksen Visual Studio ryhmän Services-palvelujen avulla

Tämän artikkelin avulla opit käyttämään Microsoft Visual Studio kuormituksen ominaisuuksien testaamiseen kuormituksen testi sovelluksen. Se käyttää Azure palvelun kangasta tilallisten palvelun takaisin loppu ja tilattomien palvelun web-edusta. Käyttää tässä esimerkissä-sovellus on lentokone-sijaintiin simulator. Voit säätää lentokone tunnus, lähtevien aika ja kohde. Sovelluksen takaisin Lopeta käsittelee pyynnöt ja edustatietokanta näyttää kartalla lentokone, joka vastaa ehtoa.

Seuraavassa kaaviossa on kuvattu palvelun kangasta sovellus, jossa testaamiseen.

![Esimerkki lentokone sijainti-sovelluksen kaavio][0]

## <a name="prerequisites"></a>Edellytykset
Ennen aloittamista sinun on suoritettava seuraavat:

- Visual Studio Team Services-tilin hankkiminen. Voit hankkia sen ilmaiseksi [Visual Studio Team Services](https://www.visualstudio.com)-palvelussa.
- Hanki ja asenna Visual Studio 2013: n tai Visual Studio 2015. Tässä artikkelissa käyttää Visual Studio 2015 Enterprise edition, mutta Visual Studio 2013 ja muut versiot toimii samalla tavalla.
- Ottaa käyttöön sovelluksen väliaikaisen-ympäristöön. Katso, [miten Visual Studiossa remote klusteriin sovellusten](service-fabric-publish-app-remote-cluster.md) Lisätietoja aiheesta.
- Tietoja sovelluksen käyttö kuvio. Näitä tietoja käytetään simuloida kuormituksen mallia.
- Tietoja tavoitteen kuormituksen testaaminen. Näin voit analysoida kuormituksen testitulokset ja tulkita.

## <a name="create-and-run-the-web-performance-and-load-test-project"></a>Luominen ja suorittaminen suorituskykyä ja lataa testi-projekti

### <a name="create-a-web-performance-and-load-test-project"></a>Projektin luominen suorituskykyä ja kuormituksen testaaminen

1. Avaa Visual Studio 2015. Valitse **Tiedosto** > **Uusi** > **projektin** valikkorivillä avaa **Uusi projekti** -valintaikkuna.

2. Laajenna **Visual C#** -solmu ja valitse **Testaa** > **suorituskykyä ja lataa testi projektin**. Kirjoita projektin nimi ja valitse sitten **OK** -painiketta.

    ![Näyttökuva uusi projekti-valintaikkuna][1]

    Raportissa pitäisi näkyä uusi suorituskykyä ja lataa testi projektin ratkaisunhallinnassa.

    ![Näyttökuva ratkaisunhallinnassa, jossa näkyy uusi projekti][2]

### <a name="record-a-web-performance-test"></a>Tietueen web suorituskyky-testi

1. Avaa .webtest projekti.

2. Valitse **Lisää tallennuksen** kuvake tallennus-istunnon aloittaminen selaimessa.

    ![Selaimessa Lisää tallenne-kuvake][3]

    ![Selaimessa tietue-painike][4]

3. Siirry palvelun kangasta-sovellus. Tallennus-ruudussa pitäisi näkyä web-pyynnöt.

    ![Näyttökuva web pyynnöt tallennus-ruudussa][5]

4. Voit suorittaa toimintoja, jotka käyttäjät voivat suorittaa odotat järjestyksessä. Näitä toimintoja käytetään mallina kuormituksen luomiseen.

5. Kun olet valmis, valitse Pysäytä tallennus **Pysäytä** -painike.

    ![Pysäytä-painike][6]

    Visual Studio .webtest projektin on siepata pyynnöt sarjaa. Dynaaminen parametrit korvautuvat automaattisesti. Tässä vaiheessa voit poistaa ylimääräiset, toistuvat riippuvuuden pyynnöt, jotka eivät ole osana Testiskenaario.

6. Tallenna projekti ja valitse sitten **Suorita testi** -komennon web suorituskyky-testin suorittaminen paikallisesti ja varmista, että kaikki toimii oikein.

    ![Näyttökuva Suorita testi-komento][7]

### <a name="parameterize-the-web-performance-test"></a>Parameterize web suorituskyky-testi

Voit parameterize web suorituskyvyn testi muuntaminen koodattu web suorituskyvyn testi ja muokkaamalla koodi. Vaihtoehtoisesti voit sitoa web suorituskyvyn testi tietoluettelon niin, että testi iteroivaa läpi tiedot. [Luo ja Suorita testi koodattu web suorituskyvyn](https://msdn.microsoft.com/library/ms182552.aspx) Saat lisätietoja muuntaminen web suorituskyvyn testi koodattu testi. [Lisää WWW-suorituskyvyn tietolähteen testaaminen](https://msdn.microsoft.com/library/ms243142.aspx) saat tietoja siitä, miten tiedot sitoa web suorituskyky-testi.

Tässä esimerkissä on muuntaminen web suorituskyvyn testi koodattu testi, jotta voit korvata lentokone tunnus luotu GUID-tunnus ja lisätä Lisää pyyntöjä lentojen lähettäminen eri sijainneissa.

### <a name="create-a-load-test-project"></a>Lataa testiprojektin luominen

Lataa testiprojektin koostuu vähintään yksi skenaariot kuvaaman web suorituskyvyn numero- ja Yksikkötesti, sekä muita määritettyä kuormituksen Testaa asetukset. Seuraavissa vaiheissa kuvataan kuormituksen testiprojektin luominen:

1. Valitse suorituskykyä ja lataa testi projektin pikavalikosta **Lisää** > **Kuormituksen testi**. Valitse **Lataa testata** ohjatun toiminnon testi-asetusten määrittämiseen **Seuraava** -painiketta.

2. Valitse **Lataa malli** -osassa, haluatko vakion käyttäjän kuormituksen tai vaihe lataamiseen, joka alkaa muutamalla käyttäjällä ja käyttäjien kasvaa ajan myötä.

    Jos on hyvä arvio käyttäjän kuormituksen määrä ja haluat nähdä, miten nykyinen järjestelmä suorittaa, valitse **Vakio ladata**. Jos lähinnä lisätietoja, järjestelmä suorittaa yhtenäisyyden parantaminen eri Lataa-kohdassa, valitse **Vaiheessa kuormituksen**.

3. **Testaa Mix** -osassa valitsemalla **Lisää** -painiketta ja valitse sitten testi, jonka haluat sisällyttää kuormituksen testi. **Jakauman** -sarakkeen avulla voit Määritä yhteensä testien suorittaminen jokaisen testin prosentteina.

4. Valitse **Suorita asetukset** -kohdassa Määritä kuormituksen testin kesto.

    >[AZURE.NOTE] **Testaa Iteraatioita** -asetus on käytettävissä vain, kun suoritat Visual Studiossa paikallisesti kuormituksen testi.

5. Määritä sijainti, johon kuormituksen testi pyynnöt luodaan, **Suorita asetukset** **sijainti** -osassa. Ohjattu toiminto saattaa kehottaa sinua kirjautumaan ryhmän Services-tilin. Kirjaudu sisään ja valitse sitten maantieteellisen sijainnin. Kun olet valmis, valitse **Valmis** -painiketta.

6. Kun kuormituksen testi on luotu, Avaa .loadtest projektin ja valitse Suorita-asetus ja **Suorita asetukset**kuten nykyisen > **suorittaa Settings1 [Active]**. **Ominaisuudet** -ikkuna avautuu käytön asetukset.

7. **Suorita asetukset** ominaisuudet-ikkuna **tulokset** -osassa **Ajoitus tiedot tallennustilan** -asetus on **ei mitään** oletusarvoisesti. Vaihda arvoksi **Kaikki yksittäisiä tietoja** , josta haluat lisätietoja, valitse Lataa tulokset. Saat lisätietoja siitä, miten voit muodostaa yhteyden Visual Studio Team Services ja lataa Testaa [Ladata testaamiseen](https://www.visualstudio.com/load-testing.aspx) .

### <a name="run-the-load-test-by-using-visual-studio-team-services"></a>Suorita kuormituksen testi Visual Studio ryhmän Services-palvelujen avulla

Valitse **Suorita kuormituksen testata** -komento aloittaa Suorita testi.

![Näyttökuva kuormituksen Test Suorita-komentoa][8]

## <a name="view-and-analyze-the-load-test-results"></a>Tarkastella ja analysoida kuormituksen tulokset

Lataa kuin testaaminen etenee, Suorituskykytiedot on kaaviomuotoisia. Raportissa pitäisi näkyä seuraavat Graphin kaltaisen.

![Näyttökuva kuormituksen testitulokset kaavion suorituskyky][9]

1. Valitse **Lataa raportti** -linkki sivun yläreunassa. Kun raportti on ladattu, valitse **Näytä raportti** -painike.

    **Graph** -välilehdessä näet eri suorituskyvyn laskureita kaaviot. **Yhteenveto** -välilehden yleinen tulokset näkyvät. **Taulukot** -välilehti näkyy välitetty ja epäonnistui kuormituksen testiä kokonaismäärän.

2. Luvun linkkejä **testi** > **epäonnistui** ja **virheet** > **määrä** sarakkeita, niin näet virheen yksityiskohtaiset tiedot.

    **Tiedot** -välilehden Näyttää epäonnistuneiden pyyntöjen virtual käyttäjä- ja Skenaariotiedot. Nämä tiedot voivat olla hyödyllisiä, jos kuormituksen testi sisältää useita skenaariot.

Hakutuloksia [analysoiminen ladata testata ladata testata Analyzer kaaviot-näkymässä](https://www.visualstudio.com/load-testing.aspx) lisätietoja tarkasteleminen kuormituksen testitulokset.

## <a name="automate-your-load-test"></a>Automaattinen lataaminen-testi

Visual Studio ryhmän Services ladata Test on ohjelmointirajapinnan avulla voit hallita kuormituksen testien ja analysoiminen Team Services-tilin. Lisätietoja on kohdassa [Cloud kuormituksen testaaminen Rest API](http://blogs.msdn.com/b/visualstudioalm/archive/2014/11/03/cloud-load-testing-rest-apis-are-here.aspx) .

## <a name="next-steps"></a>Seuraavat vaiheet
- [Seuranta- ja ohjelmistossa services paikallisesta kehittäminen asetuksissa](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[0]: ./media/service-fabric-vso-load-test/OverviewDiagram.png
[1]: ./media/service-fabric-vso-load-test/NewProjectDialog.png
[2]: ./media/service-fabric-vso-load-test/Project.png
[3]: ./media/service-fabric-vso-load-test/AddRecording.png
[4]: ./media/service-fabric-vso-load-test/AddRecording2.png
[5]: ./media/service-fabric-vso-load-test/ActionSequence.png
[6]: ./media/service-fabric-vso-load-test/StopRecording.png
[7]: ./media/service-fabric-vso-load-test/RunTest.png
[8]: ./media/service-fabric-vso-load-test/RunTest2.png
[9]: ./media/service-fabric-vso-load-test/Graph.png

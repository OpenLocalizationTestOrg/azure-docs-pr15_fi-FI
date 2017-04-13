<properties
   pageTitle="Virheenkorjaus sovelluksesi Visual Studiossa | Microsoft Azure"
   description="Paranna luotettavuutta ja suorituskykyä palvelujen kehittämällä ja virheenkorjaus ne Visual Studiossa paikallista kehittämistä klusterissa."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/21/2016"
   ms.author="vturecek;mikhegn"/>

# <a name="debug-your-service-fabric-application-by-using-visual-studio"></a>Palvelun kangasta sovelluksen virheenkorjaus Visual Studion avulla

## <a name="debug-a-local-service-fabric-application"></a>Paikallisen palvelun kangasta-sovelluksen korjaaminen

Voit säästää aikaa ja rahaa käyttöönotto ja virheenkorjaus Azure palvelun kangasta sovelluksen paikallisen tietokoneen kehittäminen klusterin. Visual Studio voit ottaa käyttöön sovelluksen paikallisen klusterin ja yhdistää viivalla automaattisesti virheenkorjaus sovelluksen esiintymät.

1. Aloita paikallista kehittämistä klusterin [palvelun kangasta kehitysympäristö](service-fabric-get-started.md)määrittäminen ohjeiden mukaisesti.

2. Painamalla **F5-näppäintä** tai valitse **Virheenkorjaus** > **Käynnistä virheenkorjaus**.

    ![Käynnistä virheenkorjaus sovelluksen][startdebugging]

3. Määritä keskeytyskohdat koodin ja käydä läpi sovellus valitsemalla **Virheenkorjaus** -valikossa komentoja.

    > [AZURE.NOTE] Visual Studio liittää sovelluksen kaikki esiintymät. Samalla, kun olet poistumassa – koodin, keskeytyskohdat Hae osumien useat prosessit, tuloksena on samanaikaista istuntoa. Poista käytöstä keskeytyskohtien, kun ne on osumien, että kunkin keskeytyskohdasta ehdollinen säikeen tunnus tai käyttämällä diagnostiikan tapahtumat.

4. **Diagnostiikan tapahtumat** -ikkuna tulee näkyviin automaattisesti, jotta voit tarkastella diagnostiikan tapahtumat reaaliajassa.

    ![Reaaliaikainen diagnostiikan tapahtumien tarkasteleminen][diagnosticevents]

5. Voit myös avata Cloud Explorer **Diagnostiikan tapahtumat** -ikkuna.  **Palvelun kangasta**ja valitse hiiren kakkospainikkeella mitä tahansa solmu ja valitse **Näytä Streaming jäljittää**.

    ![Avaa diagnostiikan tapahtumat-ikkuna][viewdiagnosticevents]

    Jos haluat suodattaa oman jäljittää tiettyyn palveluun tai sovelluksen käyttöön streaming jäljittää tiettyyn tai sovelluksen.

6. Diagnostiikan tapahtumat voidaan näyttää automaattisesti luotu **ServiceEventSource.cs** -tiedoston, ja niitä kutsutaan sovelluksen koodista.

    ```csharp
    ServiceEventSource.Current.ServiceMessage(this, "My ServiceMessage with a parameter {0}", result.Value.ToString());
    ```

7. **Diagnostiikan tapahtumat** -ikkunassa tukee suodattaminen ja keskeyttäminen tarkistetaan tapahtumia reaaliajassa.  Suodatin on yksinkertainen merkkijono haun tapahtuman viestin, mukaan lukien sen sisältöä.

    ![Suodattaa, keskeyttäminen ja jatkaminen tai Tarkasta reaaliajassa tapahtumat][diagnosticeventsactions]

8. Virheenkorjaus services on eräänlainen virheenkorjaus toisessa sovelluksessa. Helppoa virheenkorjaus tavallisesti keskeytyskohdat kautta Visual Studio määrittäminen. Luotettavan sivustokokoelmat toistaa useita solmujen välillä, mutta ne edelleen Toteuta IEnumerable. Tämä tarkoittaa, että voit tarkastella tulokset Visual Studiossa korjattaessa näet, mitä olet tallentanut, sisällä. Määrittää keskeytyskohdasta koodisi yksinkertaisesti missä tahansa.

    ![Käynnistä virheenkorjaus sovelluksen][breakpoint]

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="debug-a-remote-service-fabric-application"></a>Remote palvelun kangasta-sovelluksen korjaaminen

Jos palvelun kangasta-sovellukset ovat käynnissä Azure-palvelu kangasta klusterissa, pystyt korjata ne suoraan Visual Studio etäyhteyden välityksellä.

> [AZURE.NOTE] Ominaisuus edellyttää [palvelun kangasta SDK 2.0](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) ja [Azure SDK.NET 2.9](https://azure.microsoft.com/downloads/).    

<!-- -->
> [AZURE.WARNING] Remote virheenkorjaus on tarkoitettu keskihajonta/testi skenaarioissa niin, etteivät ne voidaan käyttää tuotantoympäristössä, käynnissä olevat sovellukset vaikutus vuoksi.

1. Yhteyttä klusterin **Cloud**Explorerissa Siirry, napsauta hiiren kakkospainikkeella ja valitse **Ota käyttöön virheenkorjaus**

    ![Ota käyttöön remote virheenkorjaus][enableremotedebugging]

    Tämä poisto-käytöstä prosessin käyttöönottoon remote muistin tunniste yhteyttä klusterin sekä tarvittavat määritykset.

2. Klusterisolmu **Cloud**Explorerissa hiiren kakkospainikkeella ja valitse **Liitä virheenkorjaus**

    ![Liitä virheenkorjaus][attachdebugger]

3. Valitse **Liitä käsittely** -valintaikkunasta haluamasi virheenkorjaus prosessi ja **Liitä**

    ![Valitse prosessi][chooseprocess]

    Prosessin liitettävä, nimi on sama kuin nimesi kokoonpanon palvelun projektin nimeä.

    Virheenkorjaus liittää kaikkien solmujen suorittaminen uudelleen.
    - Missä ovat virheenkorjaus tilattomien palvelun tapauksessa kaikki esiintymät palvelun kaikissa solmuissa kuuluvat virheenkorjausistunnon.
    - Jos korjattava tilallisten palvelu, vain ensisijainen se minkä tahansa osion on aktiivinen ja näin ollen pyydetty virheenkorjaus mukaan. Jos ensisijainen se siirtää virheenkorjausistunnon aikana, replika käsittely on osa virheenkorjaus-istunto.
    - Todellisen vain asianmukaiset osioiden tai tietyn palvelun esiintymät, jotta voit ehdollinen keskeytyskohdat poistettava vain tiettyyn osioon tai esiintymä.

    ![Ehdollinen keskeytyskohdasta][conditionalbreakpoint]

    > [AZURE.NOTE] On tällä hetkellä tue virheenkorjaus palvelun kangasta klusterin useita esiintymiä service suoritettavan tiedoston nimi.

4. Kun olet valmis virheenkorjaus sovelluksen, voit poistaa käytöstä remote muistin tunniste napsauttamalla klusterin **Cloud** Resurssienhallinnassa ja valitse **Poista virheenkorjaus**

    ![Poista remote virheenkorjaus käytöstä][disableremotedebugging]

## <a name="streaming-traces-from-a-remote-cluster-node"></a>Streaming jäljittää remote klusterin solmun

Olet myös pysty stream jäljittää suoraan Visual Studio remote klusterisolmu. Tämän ominaisuuden avulla stream tapahtumien seuranta Jäljitä tapahtumat-palvelun kangasta klusterisolmu, suoraan Visual Studiossa on valmistettu.

> [AZURE.NOTE] Ominaisuus edellyttää [palvelun kangasta SDK 2.0](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) ja [Azure SDK.NET 2.9](https://azure.microsoft.com/downloads/).

<!-- -->
> [AZURE.WARNING]Streaming jäljittää on tarkoitettu keskihajonta/testi skenaarioissa niin, etteivät ne voidaan käyttää tuotantoympäristössä, käynnissä olevat sovellukset vaikutus vuoksi.
> Tuotannon tilanteessa olisi luottavat välittäminen tapahtumien Azure Diagnostiikan avulla.

1. Yhteyttä klusterin **Cloud**Explorerissa Siirry, napsauttamalla hiiren kakkospainikkeella ja valitse **Ota käyttöön Streaming jäljittää**

    ![Ota käyttöön remote streaming jäljittää][enablestreamingtraces]

    Tämä poisto-käytöstä prosessin käyttöönottoon streaming jäljittää tunniste, valitse yhteyttä klusterin sekä tarvittavat määritykset.

2. Laajenna **Cloud**Explorerissa **solmut** -elementin, napsauta hiiren kakkospainikkeella virtauttaa jäljittää-ja valitse **Näytä Streaming jäljittää** solmu

    ![Näytä remote streaming jäljittää][viewremotestreamingtraces]

    Toista vaihetta 2 niin monta solmujen, kun haluat nähdä jäljittää kohteesta. Kunkin solmut-muodossa näkyy oma-ikkunassa.

    Voit nyt näe lähettämän palvelun kangasta ja palvelujen jälkitiedot. Jos haluat suodattaa näyttämään vain tietylle sovellukselle tapahtumat, kirjoittaa sovelluksen suodattimen nimestä.

    ![Streaming jäljittää tarkasteleminen][viewingstreamingtraces]

4. Kun siirryt streaming jäljittää yhteyttä klusterin, voit poistaa käytöstä remote streaming jäljittää napsauttamalla klusterin **Cloud** Resurssienhallinnassa ja valitse **Käytöstä Streaming jäljittää**

    ![Remote streaming jäljittää poistaminen käytöstä][disablestreamingtraces]

## <a name="next-steps"></a>Seuraavat vaiheet

- [Testaa palvelun kangasta palvelu](service-fabric-testability-overview.md).
- [Visual Studiossa palvelun kangasta sovellusten hallinta](service-fabric-manage-application-in-visual-studio.md).

<!--Image references-->
[startdebugging]: ./media/service-fabric-debugging-your-application/startdebugging.png
[diagnosticevents]: ./media/service-fabric-debugging-your-application/diagnosticevents.png
[viewdiagnosticevents]: ./media/service-fabric-debugging-your-application/viewdiagnosticevents.png
[diagnosticeventsactions]: ./media/service-fabric-debugging-your-application/diagnosticeventsactions.png
[breakpoint]: ./media/service-fabric-debugging-your-application/breakpoint.png
[enableremotedebugging]: ./media/service-fabric-debugging-your-application/enableremotedebugging.png
[attachdebugger]: ./media/service-fabric-debugging-your-application/attachdebugger.png
[chooseprocess]: ./media/service-fabric-debugging-your-application/chooseprocess.png
[conditionalbreakpoint]: ./media/service-fabric-debugging-your-application/conditionalbreakpoint.png
[disableremotedebugging]: ./media/service-fabric-debugging-your-application/disableremotedebugging.png
[enablestreamingtraces]: ./media/service-fabric-debugging-your-application/enablestreamingtraces.png
[viewingstreamingtraces]: ./media/service-fabric-debugging-your-application/viewingstreamingtraces.png
[viewremotestreamingtraces]: ./media/service-fabric-debugging-your-application/viewremotestreamingtraces.png
[disablestreamingtraces]: ./media/service-fabric-debugging-your-application/disablestreamingtraces.png

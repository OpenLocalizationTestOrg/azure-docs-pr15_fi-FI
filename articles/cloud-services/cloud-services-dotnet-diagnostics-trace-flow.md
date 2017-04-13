<properties
    pageTitle="Jäljitä Cloud Services-sovelluksen Azure vianmäärityksen virtaus | Microsoft Azure"
    description="Lisää jäljitys viestien Azure avulla virheenkorjaus mittaaminen suorituskyvyn, seuranta, liikenne analyysi ja lisää sovellus."
    services="cloud-services"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="02/20/2016"
    ms.author="robb"/>



# <a name="trace-the-flow-of-a-cloud-services-application-with-azure-diagnostics"></a>Cloud Services-sovelluksen Azure vianmäärityksen etenemisen seuranta

Jäljitys on tapa valvoa sovellusta suorittamisen on käynnissä. Voit käyttää [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace.aspx), [System.Diagnostics.Debug](https://msdn.microsoft.com/library/system.diagnostics.debug.aspx)ja [System.Diagnostics.TraceSource](https://msdn.microsoft.com/library/system.diagnostics.tracesource.aspx) luokkien tietoja virheistä ja sovellusten suorittamiseen tallentavan lokit, tekstitiedostoista tai muissa laitteissa myöhempää käyttöä varten. Saat lisätietoja jäljitys [jäljityksen ja Instrumenting sovellukset](https://msdn.microsoft.com/library/zs6s4h68.aspx).


## <a name="use-trace-statements-and-trace-switches"></a>Käytä Jäljitä lauseet ja Jäljitä valitsimet

Ota käyttöön jäljityksen lisäämällä [DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx) sovelluksen määritys ja puheluiden soittamiseen System.Diagnostics.Trace tai System.Diagnostics.Debug sovelluksen koodissa Cloud Services-sovelluksessa. Käytä määritysten tiedoston *app.config* työntekijä roolit ja web-roolien *web.config* . Kun luot uuden isännöityä palvelun Visual Studio mallin avulla, Azure diagnostiikka lisätään automaattisesti projektin ja DiagnosticMonitorTraceListener lisätään rooleihin, jotka olet lisännyt haluamasi määritystiedosto.

Lisätietoja sijoittaminen jäljitys lauseet on artikkelissa [Toimintaohje: Lisää seuranta-lauseet sovelluksen koodin](https://msdn.microsoft.com/library/zd83saa2.aspx).

Sijoittamalla [Jäljitys valitsimet](https://msdn.microsoft.com/library/3at424ac.aspx) -koodin ja voit määrittää, onko jäljityksen tapahtuu ja laajuuden se on. Voit valvoa tuotantoympäristössä sovelluksen tilaa. Tämä on erityisen tärkeää liiketoimintasovelluksessa, joka käyttää useissa tietokoneissa on useita osia. Lisätietoja on artikkelissa [Toimintaohje: Määritä jäljitys valitsimet](https://msdn.microsoft.com/library/t06xyy08.aspx).

## <a name="configure-the-trace-listener-in-an-azure-application"></a>Määritä Jäljityksen kuuntelutoiminnon Azure-sovelluksessa

Jäljitys-virheenkorjaus ja TraceSource, edellyttää määrittäminen "kuuntelijoita" Voit kerätä ja tallentaa lähetetyt viestit. Kuuntelijoita kerää, tallentaa ja sanomien jäljitys. Ne suoraan jäljitys tulosteet haluamasi kohteen, kuten lokitiedoston, ikkunan tai tekstitiedosto. Azure diagnostiikka käyttää [DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx) -luokka.

Ennen kuin suoritat seuraavat toimet, Azure diagnostiikan näyttö on alusta. Toiminto-kohdassa [Ottaminen käyttöön diagnostiikka Microsoft Azure-tietokannassa](cloud-services-dotnet-diagnostics.md).

Huomaa, että jos käytät malleilla, jotka ovat myöntämä Visual Studiossa, kuuntelua määritysten lisätään automaattisesti puolestasi.


### <a name="add-a-trace-listener"></a>Lisää Jäljityksen kuuntelutoiminnon

1. Avaa tehtäväsi web.config tai app.config-tiedosto.
2. Lisää seuraava koodi-tiedostoon. Muuta versiota käyttämään viittaat kokoonpanon versionumero. Kokoonpanoversio ei välttämättä ole muuta kunkin Azure SDK-version kanssa paitsi päivityksiä siihen.

    ```
    <system.diagnostics>
        <trace>
            <listeners>
                <add type="Microsoft.WindowsAzure.Diagnostics.DiagnosticMonitorTraceListener,
                  Microsoft.WindowsAzure.Diagnostics,
                  Version=2.8.0.0,
                  Culture=neutral,
                  PublicKeyToken=31bf3856ad364e35"
                  name="AzureDiagnostics">
                  <filter type="" />
                </add>
            </listeners>
        </trace>
    </system.diagnostics>
    ```
    >[AZURE.IMPORTANT] Varmista, että sinulla on projektiviittaus Microsoft.WindowsAzure.Diagnostics kokoonpanon. Päivitä versionumero yllä xml-vastaamaan viitatun Microsoft.WindowsAzure.Diagnostics kokoonpanon versio.

3. Tallenna tiedosto config.

Saat lisätietoja kuuntelijoita [Jäljityksen kuuntelutoiminnot](https://msdn.microsoft.com/library/4y5y10s7.aspx).

Lisää kuuntelua jälkeen voit lisätä Jäljitä lauseet koodisi.


### <a name="to-add-trace-statement-to-your-code"></a>Jäljitys-lauseen lisääminen koodi

1. Avaa lähdetiedosto sovelluksen. Esimerkiksi <RoleName>.cs tiedoston Työntekijä roolin tai web-rooli.
2. Lisää seuraava lauseella, jos se ei ole vielä lisätty:
    ```
        using System.Diagnostics;
    ```
3. Voit lisätä Jäljitä lauseet haluamaasi sovelluksen tilan tietoja. Voit muotoilla jäljitys-lauseen tulos usealla eri tavalla. Lisätietoja on artikkelissa [Toimintaohje: Lisää seuranta-lauseet sovelluksen koodin](https://msdn.microsoft.com/library/zd83saa2.aspx).
4. Tallenna lähdetiedosto.

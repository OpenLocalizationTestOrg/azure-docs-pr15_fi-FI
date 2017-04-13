<properties
    pageTitle="Opi käyttämään Azure diagnostiikka (.NET) pilvipalveluihin | Microsoft Azure"
    description="Azure Diagnostiikan avulla voit kerätä tietoja Azure pilvipalveluihin virheenkorjaus, mittaaminen suorituskyvyn, seuranta, liikenne analyysi ja lisää."
    services="cloud-services"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="01/25/2016"
    ms.author="robb"/>



# <a name="enabling-azure-diagnostics-in-azure-cloud-services"></a>Azure diagnostiikka Azure Cloud Services-palveluissa ottaminen käyttöön

Artikkelissa [Azure diagnostiikka yleiskatsaus](../azure-diagnostics.md) taustatietoa Azure Diagnostiikka.


## <a name="how-to-enable-diagnostics-in-a-worker-role"></a>Diagnostiikan Työntekijä-roolin ottaminen käyttöön

Tämä tutustua kuvataan ottamisesta käyttöön Azure Työntekijä-rooli määrää tietokoneesta kuuluu äänimerkki telemetriatietojen tietoja käyttämällä .NET EventSource-luokka. Azure diagnostiikka käytetään telemetriatietojen tietojen kerääminen ja tallentaa sen Azure-tallennustilan tilin. Kun työntekijä-roolin luominen Visual Studio ottaa automaattisesti käyttöön diagnostiikka 1.0 Azure SDK: T .NET 2.4 tai sitä vanhempi versio-ratkaisun osana. Seuraavissa ohjeissa kerrotaan Työntekijä-roolin käytöstä diagnostiikka 1.0 ratkaisusta ja käyttöönottaminen diagnostiikka 1.2 tai 1.3 Työntekijä-roolin luominen.

### <a name="pre-requisites"></a>Vaatimukset
Tässä artikkelissa oletetaan ole Azure-tilausta ja käytät Visual Studio 2013 Azure SDK-paketissa. Jos sinulla ei ole Azure tilaus, voit rekisteröityä [Maksuttoman kokeiluversion käyttäjäksi][]. Varmista, että [asentaminen ja määrittäminen PowerShellin Azure versio 0.8.7 tai uudempi][].

### <a name="step-1-create-a-worker-role"></a>Vaihe 1: Työntekijä-roolin luominen
1.  Käynnistä **Visual Studio 2013**.
2.  Luo uusi **Azure pilvipalvelussa** projekti **Cloud** -malli, joka on suunnattu .NET Framework 4.5.  Projektin "WadExample" nimeä ja valitse Ok.
3.  **Työntekijän rooli** ja valitse Ok. Projektin luodaan.
4.  Kaksoisnapsauta **Ratkaisunhallinnassa** **WorkerRole1** ominaisuudet-tiedoston avulla.
5.  **Määritys** -välilehden poistaa **Käyttöön diagnostiikka** käytöstä diagnostiikka 1.0 (Azure SDK 2.4 ja eariler).
6.  Luoda ratkaisu varmistaaksesi, että sinulla on virheitä.

### <a name="step-2-instrument-your-code"></a>Vaihe 2: Soittimen koodi
Korvaa WorkerRole.cs sisällön seuraava koodi. Luokan SampleEventSourceWriter, [EventSource luokan][]perittyjä toteuttaa neljä kirjaaminen tapaa: **SendEnums**, **MessageMethod**, **SetOther** ja **HighFreq**. Ensimmäinen parametri **WriteEvent** tapaan määrittää vastaaviin tapahtuman tunnus. Suorita menetelmä toteuttaa äärettömän silmukka, joka kutsuu kunkin kirjaaminen menetelmistä toteutettu **SampleEventSourceWriter** luokan 10 sekunnin välein.

    using Microsoft.WindowsAzure.ServiceRuntime;
    using System;
    using System.Diagnostics;
    using System.Diagnostics.Tracing;
    using System.Net;
    using System.Threading;

    namespace WorkerRole1
    {
    sealed class SampleEventSourceWriter : EventSource
    {
        public static SampleEventSourceWriter Log = new SampleEventSourceWriter();
        public void SendEnums(MyColor color, MyFlags flags) { if (IsEnabled())  WriteEvent(1, (int)color, (int)flags); }// Cast enums to int for efficient logging.
        public void MessageMethod(string Message) { if (IsEnabled())  WriteEvent(2, Message); }
        public void SetOther(bool flag, int myInt) { if (IsEnabled())  WriteEvent(3, flag, myInt); }
        public void HighFreq(int value) { if (IsEnabled()) WriteEvent(4, value); }

    }

    enum MyColor
    {
        Red,
        Blue,
        Green
    }

    [Flags]
    enum MyFlags
    {
        Flag1 = 1,
        Flag2 = 2,
        Flag3 = 4
    }

    public class WorkerRole : RoleEntryPoint
    {
        public override void Run()
        {
            // This is a sample worker implementation. Replace with your logic.
            Trace.TraceInformation("WorkerRole1 entry point called");

            int value = 0;

            while (true)
            {
                Thread.Sleep(10000);
                Trace.TraceInformation("Working");

                // Emit several events every time we go through the loop
                for (int i = 0; i < 6; i++)
                {
                    SampleEventSourceWriter.Log.SendEnums(MyColor.Blue, MyFlags.Flag2 | MyFlags.Flag3);
                }

                for (int i = 0; i < 3; i++)
                {
                    SampleEventSourceWriter.Log.MessageMethod("This is a message.");
                    SampleEventSourceWriter.Log.SetOther(true, 123456789);
                }

                if (value == int.MaxValue) value = 0;
                SampleEventSourceWriter.Log.HighFreq(value++);
            }
        }

        public override bool OnStart()
        {
            // Set the maximum number of concurrent connections
            ServicePointManager.DefaultConnectionLimit = 12;

            // For information on handling configuration changes
            // see the MSDN topic at http://go.microsoft.com/fwlink/?LinkId=166357.

            return base.OnStart();
        }
    }
    }


### <a name="step-3-deploy-your-worker-role"></a>Vaihe 3: Käyttöönotto työntekijän rooli
1.  Käyttöön Visual Studion Azure Työntekijä-roolin **WadExample** projektin valitsemalla Napsauta ratkaisunhallinnassa ja valitse sitten **Julkaise** **luominen** -valikosta.
2.  Valitse tilauksen.
3.  Valitse **Microsoft Azure Julkaisuasetukset** -valintaikkunan **Luo uusi...**.
4.  Kirjoita **luominen pilvipalvelussa ja tallennustilaa tili** -valintaikkunassa **nimi** (esimerkiksi "WadExample") ja valitse alueen tai affiniteetti ryhmä.
5.  Määritä **ympäristön** **vaiheet**.
6.  Muuta muita **asetuksia** tarpeen mukaan ja valitse sitten **Julkaise**.
7.  Käyttöönoton jälkeen Tarkista Azure perinteinen portaalin että cloud-palvelu on **käytössä** -tilaan.

### <a name="step-4-create-your-diagnostics-configuration-file-and-install-the-extension"></a>Vaihe 4: Luo diagnostiikka-kokoonpanotiedosto ja asenna laajennus
1.  Lataa julkisen määritysten tiedoston rakenteen määritys suorittamalla seuraavan PowerShell-komennon:
2.
        (Hae AzureServiceAvailableExtension - ExtensionName 'PaaSDiagnostics' - ProviderNamespace "Microsoft.Azure.Diagnostics"). PublicConfigurationSchema | Tiedosto vanhenee-koodaus utf8 - tiedostopolku "WadConfig.xsd"

2.  XML-tiedoston lisääminen projektiin **WorkerRole1** **WorkerRole1** projektin napsauttaa hiiren kakkospainikkeella ja valitse **Lisää** -> **Uusi kohde...**  ->  **Visual C# kohteiden** -> **tietojen** -> **XML-tiedosto**. Nimeä tiedosto "WadExample.xml".

    ![CloudServices_diag_add_xml](./media/cloud-services-dotnet-diagnostics/AddXmlFile.png)

3.  Liitä WadConfig.xsd määritystiedostoa. Varmista, että WadExample.xml editorin ikkuna on aktiivinen ikkuna. Paina **F4** Avaa **Ominaisuudet** -ikkuna. Valitse **Mallit** -ominaisuuden **Ominaisuudet** -ikkunassa. Valitse **** ... **Mallit** -ominaisuudessa. Valitse **Lisää …** -painiketta ja siirry sijaintiin, johon tallensit XSD-tiedosto ja valitse tiedosto WadConfig.xsd. Valitse **OK**.
4.  Korvaa WadExample.xml määritys-tiedoston sisällön, seuraavat XML ja Tallenna tiedosto. Tämä kokoonpanotiedosto määrittää muutama kerääminen suorituskyvyn laskureita: yksi suorittimen käyttö ja yksi muistin käyttö. Valitse määritykset määrittää vastaava SampleEventSourceWriter luokan menetelmiä neljä tapahtumat.

```
        <?xml version="1.0" encoding="utf-8"?>
        <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
            <WadCfg>
                <DiagnosticMonitorConfiguration overallQuotaInMB="25000">
                <PerformanceCounters scheduledTransferPeriod="PT1M">
                    <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT1M" unit="percent" />
                    <PerformanceCounterConfiguration counterSpecifier="\Memory\Committed Bytes" sampleRate="PT1M" unit="bytes"/>
                    </PerformanceCounters>
                    <EtwProviders>
                        <EtwEventSourceProviderConfiguration provider="SampleEventSourceWriter" scheduledTransferPeriod="PT5M">
                            <Event id="1" eventDestination="EnumsTable"/>
                            <Event id="2" eventDestination="MessageTable"/>
                            <Event id="3" eventDestination="SetOtherTable"/>
                            <Event id="4" eventDestination="HighFreqTable"/>
                            <DefaultEvents eventDestination="DefaultTable" />
                        </EtwEventSourceProviderConfiguration>
                    </EtwProviders>
                </DiagnosticMonitorConfiguration>
            </WadCfg>
        </PublicConfig>
```

### <a name="step-5-install-diagnostics-on-your-worker-role"></a>Vaihe 5: Asentaminen diagnostiikka työntekijän rooli
Hallintaan diagnostiikka verkossa tai Työntekijä rooliin PowerShellin cmdlet-komennot ovat: Set-AzureServiceDiagnosticsExtension, Hae AzureServiceDiagnosticsExtension ja poista AzureServiceDiagnosticsExtension.

1.  Avaa Azure PowerShell.
2.  Suorita komentosarja diagnostiikka asennetaan työntekijä tehtäväsi (korvaa *StorageAccountKey* wadexample tallennustilan tilin tallennustilan tilin-näppäimen kanssa):

```
    $storage_name = "wadexample"
    $key = "<StorageAccountKey>"
    $config_path="c:\users\<user>\documents\visual studio 2013\Projects\WadExample\WorkerRole1\WadExample.xml"
    $service_name="wadexample"
    $storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key
    Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Staging -Role WorkerRole1
```

### <a name="step-6-look-at-your-telemetry-data"></a>Vaihe 6: Tarkista telemetriatietojen tiedot
Siirry Visual Studio **Palvelimen Explorer** wadexample tallennustilan tilin. Pilveen jälkeen palvelu on käyttänyt pitäisi näkyä taulukot **WADEnumsTable**, **WADHighFreqTable** **WADMessageTable**, **WADPerformanceCountersTable** ja **WADSetOtherTable**noin 5 minuuttia. Kaksoisnapsauta johonkin tarkastelemiseen, joka on kerätty telemetriatietojen taulukot.
    ![CloudServices_diag_tables](./media/cloud-services-dotnet-diagnostics/WadExampleTables.png)


## <a name="configuration-file-schema"></a>Määritysten rakennetta

Diagnostiikka-kokoonpanotiedosto määrittää arvot, joita käytetään alustaa diagnostiikan asetuksia diagnostiikka-agentti käynnistyessä. Hae [uusimmat rakenteen viittaus](https://msdn.microsoft.com/library/azure/mt634524.aspx) kelvollisia arvoja ja esimerkkejä.

## <a name="troubleshooting"></a>Vianmääritys

Jos sinulla on ongelmia, katso ohjeet yleisimpien ongelmien [Vianmääritys Azure diagnostiikka](../azure-diagnostics-troubleshooting.md) .

## <a name="next-steps"></a>Seuraavat vaiheet
Muuta tietoja kerätään, [Katso virtuaalikoneen luettelo liittyvät artikkelit Azure diagnostiikka](azure-diagnostics.md#cloud-services) liittyviä ongelmia tai saada lisätietoja diagnostiikka yleinen.


[EventSource luokan]: http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource(v=vs.110).aspx

[Debugging an Azure Application]: http://msdn.microsoft.com/library/windowsazure/ee405479.aspx   
[Collect Logging Data by Using Azure Diagnostics]: http://msdn.microsoft.com/library/windowsazure/gg433048.aspx
[Maksuton kokeiluversio]: http://azure.microsoft.com/pricing/free-trial/
[Asenna ja määritä PowerShellin Azure versio 0.8.7 tai uudempi versio]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/

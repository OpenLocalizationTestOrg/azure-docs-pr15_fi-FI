<properties
    pageTitle="Azure diagnostiikka käyttämisestä näennäiskoneiden | Microsoft Azure"
    description="Azure Diagnostiikan avulla voit kerätä tietoja kohteesta Azuren näennäiskoneiden virheenkorjaus, mittaaminen suorituskyvyn, seuranta, liikenne analyysi ja lisää."
    services="virtual-machines"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="virtual-machines"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="02/20/2016"
    ms.author="robb"/>



# <a name="enabling-diagnostics-in-azure-virtual-machines"></a>Azuren näennäiskoneiden diagnostiikka ottaminen käyttöön

Artikkelissa [Azure diagnostiikka yleiskatsaus](azure-diagnostics.md) taustatietoa Azure Diagnostiikka.

## <a name="how-to-enable-diagnostics-in-a-virtual-machine"></a>Virtual Machine diagnostiikka ottaminen käyttöön

Tämä tutustua käsitellään etäasennuksen diagnostiikka Azure virtuaalikoneen kehittäminen tietokoneesta. Opit myös sovellus, joka suoritetaan, että Azure virtuaalikoneen ja telemetriatietojen tietoja käyttämällä .NET [EventSource luokan][]tietokoneesta kuuluu äänimerkki ottamisesta käyttöön. Azure diagnostiikka käytetään telemetriatietojen kerätä ja tallentaa sen Azure-tallennustilan tilin.

### <a name="pre-requisites"></a>Vaatimukset
Tämän tutustua olettaa ole Azure-tilausta ja käytät Visual Studio 2013 Azure SDK-paketissa. Jos sinulla ei ole Azure tilaus, voit rekisteröityä [Maksuttoman kokeiluversion käyttäjäksi][]. Varmista, että [asentaminen ja määrittäminen PowerShellin Azure versio 0.8.7 tai uudempi][].

### <a name="step-1-create-a-virtual-machine"></a>Vaihe 1: Luo Virtual Machine
1.  Käynnistä Visual Studio 2013 kehittäminen tietokoneessasi.
2.  Visual Studio **Palvelimen Explorer** Laajenna **Azure**, **näennäiskoneiden** kakkospainikkeella ja valitse **Luo virtuaalikoneen**.
3.  **Valitse tilaus** -valintaikkunassa Azure tilaus ja valitse **Seuraava**.
4.  Valitse **Windows Server 2012 R2 Palvelinkeskukseen, marraskuussa 2014** **virtuaalikoneen kuvan valitseminen** -valintaikkunassa ja valitse **Seuraava**.
5.  Määritä **Virtuaalikoneen perusasetukset**virtuaalikoneen nimi "wadexample". Määritä järjestelmänvalvojan käyttäjänimi ja salasana ja valitse **Seuraava**.
6.  **Cloud Palveluasetukset** -valintaikkunassa Luo uusi pilvipalvelussa nimeltä "wadexampleVM". Luo uusi tallennustilan tili nimeltä "wadexample" ja valitse **Seuraava**.
7.  Valitse **Luo**.

### <a name="step-2-create-your-application"></a>Vaihe 2: Luo sovelluksen
1.  Käynnistä Visual Studio 2013 kehittäminen tietokoneessasi.
2.  Luo uusi Visual C# Console-sovellus, joka sopii .NET Framework 4.5. Nimeä projektin "WadExampleVM".
    ![CloudServices_diag_new_project](./media/virtual-machines-dotnet-diagnostics/NewProject.png)
3.  Korvaa Program.cs sisällön seuraava koodi. Luokan **SampleEventSourceWriter** toteuttaa neljä kirjaaminen tapaa: **SendEnums**, **MessageMethod**, **SetOther** ja **HighFreq**. Ensimmäinen parametri WriteEvent tapaan määrittää vastaaviin tapahtuman tunnus. Suorita menetelmä toteuttaa äärettömän silmukka, joka kutsuu kunkin kirjaaminen menetelmistä toteutettu **SampleEventSourceWriter** luokan 10 sekunnin välein.

        using System;
        using System.Diagnostics;
        using System.Diagnostics.Tracing;
        using System.Threading;

        namespace WadExampleVM
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

        class Program
        {
        static void Main(string[] args)
        {
            Trace.TraceInformation("My application entry point called");

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
        }
        }


4.  Tallenna tiedosto ja valitse **Muodosta ratkaisu** luonnissa koodisi **luominen** -valikosta.


### <a name="step-3-deploy-your-application"></a>Vaihe 3: Sovelluksen käyttöönotto
1.  Napsauta **Ratkaisunhallinnassa** **WadExampleVM** projektin hiiren kakkospainikkeella ja valitse **Avaa Resurssienhallinnassa kansio**.
2.  Siirry kansioon, *bin\Debug* ja kopioi kaikki tiedostot (WadExampleVM.*)
3.  **Palvelimen** Explorerissa virtuaalikoneen Napsauta hiiren kakkospainikkeella ja valitse **Yhdistä käyttäen Etätyöpöytä**.
4.  Kun yhteys on muodostettu AM Luo kansio nimeltä WadExampleVM ja liitä sovelluksen tiedostot-kansioon.
5.  Käynnistä sovellus WadExampleVM.exe. Raportissa pitäisi näkyä tyhjä konsoli-ikkuna.

### <a name="step-4-create-your-diagnostics-configuration-and-install-the-extension"></a>Vaihe 4: Luo diagnostiikka-asetusten ja asenna laajennus
1.  Lataa julkisen määritysten tiedoston rakenteen määritys kehittäminen tietokoneeseen suorittamalla seuraavan PowerShell-komennon:

        (Get-AzureServiceAvailableExtension -ExtensionName 'PaaSDiagnostics' -ProviderNamespace 'Microsoft.Azure.Diagnostics').PublicConfigurationSchema | Out-File -Encoding utf8 -FilePath 'WadConfig.xsd'

2.  Avaa Visual Studiossa, joko ei ole avointen projektien projektin avoinna tai Visual Studiossa esiintymän uutta XML-tiedostoa. Visual Studiossa, valitse **Lisää** -> **Uusi kohde...**  ->  **Visual C# kohteiden** -> **tietojen** -> **XML-tiedosto**. Nimeä tiedosto "WadExample.xml"
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

### <a name="step-5-remotely-install-diagnostics-on-your-azure-virtual-machine"></a>Vaihe 5: Etäasennuksen diagnostiikka-Azure virtuaalikoneen
Hallintaan diagnostiikka-AM PowerShellin cmdlet-komennot ovat: Set-AzureVMDiagnosticsExtension, Hae AzureVMDiagnosticsExtension ja poista AzureVMDiagnosticsExtension.

1.  Kehitystyökalut-tietokoneessa Avaa PowerShell-Azure.
2.  Suorita komentosarja etäasennuksen diagnostiikka oman AM (korvaa *StorageAccountKey* wadexamplevm tallennustilan tilin tallennustilan tilin-näppäimen kanssa):

        $storage_name = "wadexamplevm"
        $key = "<StorageAccountKey>"
        $config_path="c:\users\<user>\documents\visual studio 2013\Projects\WadExampleVM\WadExampleVM\WadExample.xml"
        $service_name="wadexamplevm"
        $vm_name="WadExample"
        $storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key
        $VM1 = Get-AzureVM -ServiceName $service_name -Name $vm_name
        $VM2 = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $config_path -Version "1.*" -VM $VM1 -StorageContext $storageContext
        $VM3 = Update-AzureVM -ServiceName $service_name -Name $vm_name -VM $VM2.VM


### <a name="step-6-look-at-your-telemetry-data"></a>Vaihe 6: Tarkista telemetriatietojen tiedot
Siirry Visual Studio **Palvelimen Explorer** wadexample tallennustilan tilin. Kun AM on ollut käynnissä noin 5 minuuttia pitäisi näkyä **WADEnumsTable**, **WADHighFreqTable**, **WADMessageTable**, **WADPerformanceCountersTable** ja **WADSetOtherTable**taulukot. Kaksoisnapsauta johonkin tarkastelemiseen, joka on kerätty telemetriatietojen taulukot.

![CloudServices_diag_wadexamplevm_tables](./media/virtual-machines-dotnet-diagnostics/WadExampleVMTables.png)

## <a name="configuration-file-schema"></a>Määritysten rakennetta

Diagnostiikka-kokoonpanotiedosto määrittää arvot, joita käytetään alustaa diagnostiikan asetuksia diagnostiikka-agentti käynnistyessä. Hae [uusimmat rakenteen viittaus](https://msdn.microsoft.com/library/azure/mt634524.aspx) kelvollisia arvoja ja esimerkkejä.

## <a name="troubleshooting"></a>Vianmääritys

Lisätietoja on kohdassa [Vianmääritys Azure diagnostiikka](azure-diagnostics-troubleshooting.md) .


## <a name="next-steps"></a>Seuraavat vaiheet
Muuta tietoja kerätään, [Katso virtuaalikoneen luettelo liittyvät artikkelit Azure diagnostiikka](azure-diagnostics.md#virtual-machines-using-azure-diagnostics) liittyviä ongelmia tai saada lisätietoja diagnostiikka yleinen.


[EventSource luokan]: http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource(v=vs.110).aspx

[Debugging an Azure Application]: http://msdn.microsoft.com/library/windowsazure/ee405479.aspx   
[Collect Logging Data by Using Azure Diagnostics]: http://msdn.microsoft.com/library/windowsazure/gg433048.aspx
[Maksuton kokeiluversio]: http://azure.microsoft.com/pricing/free-trial/
[Asenna ja määritä PowerShellin Azure versio 0.8.7 tai uudempi versio]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/

<properties
    pageTitle="Ota jatkuva toimituksen remote virheenkorjaus | Microsoft Azure"
    description="Opettele remote virheenkorjaus käytettäessä jatkuva toimituksen Azure käyttöön ottaminen käyttöön"
    services="cloud-services"
    documentationCenter=".net"
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="tarcher"/>

# <a name="enable-remote-debugging-when-using-continuous-delivery-to-publish-to-azure"></a>Ota käyttöön remote virheenkorjaus käytettäessä jatkuva toimituksen Azure julkaiseminen

Voit ottaa remote virheenkorjaus Azure-tietokannassa, pilvipalveluihin tai näennäiskoneiden, kun käytät [Jatkuva toimittamista](cloud-services-dotnet-continuous-delivery.md) julkaiseminen Azure toimimalla seuraavasti.

## <a name="enabling-remote-debugging-for-cloud-services"></a>Pilvipalveluihin remote virheenkorjaus ottaminen käyttöön

1. Valitse Muodosta agent Määritä alkuperäinen ympäristö Azure kuvatulla tavalla [Komentorivivalitsimet luominen Azure](http://msdn.microsoft.com/library/hh535755.aspx).
2. Koska paketille tarvitaan remote virheenkorjaus Runtimen (msvsmon.exe), asenna [Remote Tools for Visual Studio 2015](http://www.microsoft.com/en-us/download/details.aspx?id=48155) (tai [Microsoft Visual Studio 2013: n päivitys 5 Remote työkaluja](https://www.microsoft.com/en-us/download/details.aspx?id=48156) , jos käytät Visual Studio 2013). Vaihtoehtoisesti voit kopioida remote virheenkorjaus binaaritiedostoja järjestelmästä, jossa on asennettu Visual Studio.
3. Varmenteen luominen kuvatulla tavalla [Azure pilvipalveluihin yleistä varmenteet](cloud-services-certs-create.md). Säilytä .pfx ja RDP varmenteen allekirjoitus ja Lataa sertifikaatti kohde pilvipalveluun.
4. MSBuild komentoriviltä seuraavista vaihtoehdoista avulla voit luoda ja pakata niin, että remote virheenkorjaus käytössä. (Järjestelmä- ja project tiedostojen kulma hakasulkeissa kohteiden todellinen polut korvaa).

        msbuild /TARGET:PUBLISH /PROPERTY:Configuration=Debug;EnableRemoteDebugger=true;VSX64RemoteDebuggerPath="<remote tools path>";RemoteDebuggerConnectorCertificateThumbprint="<thumbprint of the certificate added to the cloud service>";RemoteDebuggerConnectorVersion="2.7" "<path to your VS solution file>"

    `VSX64RemoteDebuggerPath`on kansion polku, joka sisältää msvsmon.exe Remote-työkalujen Visual Studio.
    `RemoteDebuggerConnectorVersion`oman pilvipalvelussa Azure SDK-versio on. Pitäisi myös vastata Visual Studiossa asennettu versio.

5. Julkaise kohde pilvipalveluun edellisessä vaiheessa luotu paketin ja .cscfg-tiedoston avulla.
6. Sertifikaattien (.pfx-tiedosto), jossa on Azure SDK ja Visual Studio .NET asennettuna tietokoneeseen. Varmista, että voit tuoda `CurrentUser\My` sertifikaattisäilöön, muuten liittäminen Visual Studiossa virheenkorjaus epäonnistuu.

## <a name="enabling-remote-debugging-for-virtual-machines"></a>Remote virheenkorjaus näennäiskoneiden ottaminen käyttöön

1. Luo Azure virtuaalikoneen. Katso [luominen Virtual-Machine, joissa on käytössä Windows Server](../virtual-machines/virtual-machines-windows-hero-tutorial.md) tai [luoda ja hallita Azuren näennäiskoneiden Visual Studiossa](../virtual-machines/virtual-machines-windows-classic-manage-visual-studio.md).
2. Voit tarkastella [Azure perinteinen portaalisivu](http://go.microsoft.com/fwlink/p/?LinkID=269851)virtual machine Raporttinäkymät-ikkunan Nähdäksesi virtual machine **RDP VARMENTEEN ALLEKIRJOITUSTA**. Tätä arvoa käytetään `ServerThumbprint` tunniste-määritys-arvo.
3. Luoda Asiakasvarmenne [Varmenteet yleistä Azure pilvipalveluihin](cloud-services-certs-create.md) (pitää .pfx ja RDP varmenteen allekirjoitus).
4. Asenna PowerShellin Azure (versio 0.7.4 tai uudempi) kuvatulla tavalla [asentaminen ja määrittäminen PowerShellin Azure](../powershell-install-configure.md).
5. Suorita seuraava komentosarja käyttöön RemoteDebug-tunniste. Korvaa polkujen ja henkilökohtaisia tietoja omalla, kuten tilauksen nimi ja palvelunimi ja allekirjoitus.

    >[AZURE.NOTE] Tämä komentosarja on määritetty Visual Studio 2015. Jos käytät Visual Studio 2013, muokata `$referenceName` ja `$extensionName` varaukset alla käyttämään `RemoteDebugVS2013` (sen sijaan, että `RemoteDebugVS2015`).

    <pre>
   Lisää AzureAccount

    Valitse-AzureSubscription "Microsoft tilauksen"

    $vm = get-AzureVM - palvelun nimi "mytestvm1"-nimen "mytestvm1"

    $endpoints = @( ,@{Name="RDConnVS2013"; PublicPort = 30400; PrivatePort = 30398} ,@{Name="RDFwdrVS2013"; PublicPort = 31400; PrivatePort = 31398})  

    foreach ($endpoint $endpoints) {Lisää AzureEndpoint - AM $vm-$endpoint nimi. Name - protokollan tcp - PublicPort $endpoint. PublicPort - Paikallisportti $endpoint. PrivatePort}

    $referenceName = "Microsoft.VisualStudio.WindowsAzure.RemoteDebug.RemoteDebugVS2015" $publisher = "Microsoft.VisualStudio.WindowsAzure.RemoteDebug" $extensionName = "RemoteDebugVS2015" $version = "1.*" $publicConfiguration = "<PublicConfig>< Connector.Enabled > tosi < /Connector.Enabled ><ClientThumbprint>56D7D1B25B472268E332F7FC0C87286458BFB6B2</ClientThumbprint><ServerThumbprint>E7DCB00CB916C468CC3228261D6E4EE45C8ED3C6</ServerThumbprint><ConnectorPort>30398</ConnectorPort><ForwarderPort>31398</ForwarderPort></PublicConfig>"

    $vm | Määritä AzureVMExtension `
    -ReferenceName $referenceName ` 
    -Publisher $publisher `
    -ExtensionName $extensionName ` 
    -Version $version ' - PublicConfiguration $publicConfiguration

    foreach ($extension $vm. AM. ResourceExtensionReferences) {Jos (($extension. Viittausnimi - eq $referenceName) `
    -and ($extension.Publisher -eq $publisher) ` 
    - ja ($extension. Nimi - eq $extensionName) "- ja ($extension. Version - eq $version)) {$extension. ResourceExtensionParameterValues [0]. Avain = 'config.txt' break}}

    $vm | Päivitä AzureVM </pre>

6. Sertifikaattien (.pfx), jossa on Azure SDK ja Visual Studio .NET asennettuna tietokoneeseen.

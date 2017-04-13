<properties
    pageTitle="Symantec Endpoint Protection asentaminen AM | Microsoft Azure"
    description="Opettele asentaminen ja määrittäminen Symantec Endpoint Protection suojaus-tunniste uuteen tai aiemmin luotuun Azure AM perinteinen käyttöönotto-mallin avulla luotu."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="iainfou"/>

# <a name="how-to-install-and-configure-symantec-endpoint-protection-on-a-windows-vm"></a>Asentaminen ja määrittäminen Windows-AM Symantec Endpoint Protection

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

Tässä artikkelissa kerrotaan, asentaa ja määrittää Symantec Endpoint Protection asiakkaan koneessa aiemmin luotuun virtual (AM) käytössä Windows Server. Tämä on täydellinen asiakas, joka sisältää palvelujen, kuten virusten ja vakoiluohjelmien suojaus, palomuurin ja tunkeutumisen estäminen. Asiakas on asennettu suojaus-tunniste on käyttämällä AM-agentti.

Jos sinulla on olemassa olevaan tilaukseen Symantecilta paikallisen ratkaisun, voit suojata Azuren näennäiskoneiden. Jos et ole asiakkaan vielä, voit rekisteröityä kokeiluversioon. Saat lisätietoja tämän ratkaisun [Symantec Endpoint Protection Microsoft Azure-ympäristössä][Symantec]. Tällä sivulla on myös linkit käyttöoikeustiedot ja asiakkaan asennusohjeet, jos olet jo Symantec asiakkaalle.

## <a name="install-symantec-endpoint-protection-on-an-existing-vm"></a>Symantec Endpoint Protection asentaminen aiemmin AM

Ennen kuin aloitat, tarvitset seuraavat:

- PowerShellin Azure-moduulissa, versio 0.8.2 tai uudempi työpaikan tietokoneeseen. Voit tarkistaa, joihin on asennettu ja PowerShellin Azure-version **Get-moduulin azure | Muotoile taulukkoa-version** komento. Ohjeita ja linkin uusimpaan versioon on artikkelissa [asentaminen ja määrittäminen PowerShellin Azure][PS]. Kirjaudu sisään ja Azure tilauksen käyttämällä `Add-AzureAccount`.

- Käynnissä Azure virtuaalikoneen AM agentti.

Varmista ensin, että AM-agentti on jo asennettu virtuaalikoneen. Täytä cloud palvelunimi ja virtuaalikoneen nimi ja suorita sitten komento järjestelmänvalvojan PowerShellin Azure-komentokehotteeseen seuraavat komennot. Korvaa kaiken tarjoukset, mukaan lukien < ja > merkit.

> [AZURE.TIP] Jos et tiedä pilvipalvelussa ja virtuaalikoneen nimet, suorita **Get-AzureVM** luettelon nykyisen tilauksesi kaikki näennäiskoneiden nimet.

    $CSName = "<cloud service name>"
    $VMName = "<virtual machine name>"
    $vm = Get-AzureVM -ServiceName $CSName -Name $VMName
    write-host $vm.VM.ProvisionGuestAgent

Jos **Kirjoitus-host** -komento näkyy **Tosi**, AM-agentti asennettu. Jos näyttää **Epätosi**, katso ohjeet ja lataa Azure blogin linkki lähettää [AM agentti ja laajennukset - osa 2][Agent].

Jos AM-agentti on asennettu, voit asentaa Symantec Endpoint Protection-agentti nämä komennot.

    $Agent = Get-AzureVMAvailableExtension -Publisher Symantec -ExtensionName SymantecEndpointProtection

    Set-AzureVMExtension -Publisher Symantec –Version $Agent.Version -ExtensionName SymantecEndpointProtection -VM $vm | Update-AzureVM

Voit varmistaa, että Symantec suojaus-laajennus on asennettu ja on ajan tasalla seuraavasti:

1.  Kirjaudu virtuaalikoneen. Ohjeita on artikkelissa [miten virtuaalikoneen käytössä Windows Server Kirjaudu][Logon].
2.  Windows Server 2008 R2, valitse **Käynnistä > Symantec Endpoint Protection**. Windows Server 2012: ssa tai Windows Server 2012 R2-aloitusnäytön Kirjoita **Symantec**ja valitse sitten **Symantec Endpoint Protection**.
3.  Päivitysten **Tila Symantec Endpoint Protection** -ikkunan **tila** -välilehdessä tai tarvittaessa uudelleen.

## <a name="additional-resources"></a>Lisäresursseja

[Miten Kirjaudu Virtual-Machine, joissa on käytössä Windows Server][Logon]

[Azure AM tunnisteet ja toiminnot][Ext]


<!--Link references-->
[Symantec]: http://www.symantec.com/connect/blogs/symantec-endpoint-protection-now-microsoft-azure

[Portal]: http://manage.windowsazure.com

[Create]: virtual-machines-windows-classic-tutorial.md

[PS]: ../powershell-install-configure.md

[Agent]: http://go.microsoft.com/fwlink/p/?LinkId=403947

[Logon]: virtual-machines-windows-classic-connect-logon.md

[Ext]: http://go.microsoft.com/fwlink/p/?linkid=390493
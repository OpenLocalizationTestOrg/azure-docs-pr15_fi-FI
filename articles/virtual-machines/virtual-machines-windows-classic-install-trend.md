<properties
    pageTitle="Asenna trendi Micro laaja suojaus AM | Microsoft Azure"
    description="Tässä artikkelissa kuvataan asentaa ja määrittää trendi mikro suojaus AM, joka on luotu Azure perinteinen käyttöönottomalli."
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


# <a name="how-to-install-and-configure-trend-micro-deep-security-as-a-service-on-a-windows-vm"></a>Asentaa ja määrittää trendi mikro laaja suojauksen palveluna Windows AM

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

Tässä artikkelissa kerrotaan, asentaa ja määrittää trendi mikro laaja suojauksen palveluna uuteen tai aiemmin luotuun virtual tietokoneeseen (AM) käytössä Windows Server. Laaja Security palveluna sisältää haittaohjelmien torjuntasuojaus, palomuuri, tunkeutumisen estäminen järjestelmän sekä eheys seuranta.

Asiakas on asennettu suojaus-tunniste AM-agentti kautta. Uusi virtual tietokoneeseen sinun on asennettava laaja suojaus-agentti sekä AM-agentti. Aiemmin luodun virtual tietokoneeseen, AM-agentti ei ole sinun on ladattava ja asennettava ensin. Tässä artikkelissa käsitellään sekä tilanteissa.

Jos sinulla on olemassa olevaan tilaukseen trendi mikro paikallisen ratkaisun, voit suojautua Azuren näennäiskoneiden. Jos et ole asiakkaan vielä, voit rekisteröityä kokeiluversioon. Lisätietoja tämä ratkaisu on trendin mikro blogimerkinnässä [Microsoft Azure AM agentti tunniste, laaja suojaus](http://go.microsoft.com/fwlink/p/?LinkId=403945).

## <a name="install-the-deep-security-agent-on-a-new-vm"></a>Asenna uusi AM laaja suojaus-agentti

[Azure perinteinen portaalissa](http://manage.windowsazure.com) voit asentaa AM-agentti ja trendi mikro suojaus-tunniste, kun **-Valikoima** -vaihtoehdon avulla voit luoda virtuaalikoneen. Jos olet luomassa virtual yhteen tietokoneeseen, portaalissa on helppo tapa lisätä suojauksen trendi mikro.

Asetus **-Valikoima** avautuu ohjattu toiminto, jonka avulla voit määrittää virtuaalikoneen. Ohjatun toiminnon viimeisellä sivulla voit asentaa AM agentti ja trendi mikro suojaus-tunniste. Yleisiä ohjeita on artikkelissa [Create virtual machine käynnissä Windows Azure perinteinen-portaalissa](virtual-machines-windows-classic-tutorial.md). Kun saat viimeiseen ohjatun toiminnon sivulla, toimi seuraavasti:

1.  Tarkista **AM agentti**, **Asenna AM agentti**.

2.  Tarkista **Suojaus tunnisteet** **Trendi mikro laaja suojauksen agentti**.

    ![Asenna AM-agentti ja laaja Suojausagentti](./media/virtual-machines-windows-classic-install-trend/InstallVMAgentandTrend.png)

3.  Napsauta Luo virtuaalikoneen valintamerkkiä.

## <a name="install-the-deep-security-agent-on-an-existing-vm"></a>Laaja suojaus-agentti asentaminen aiemmin AM

Asentamiseksi agentti aiemmin AM edellyttää seuraavia:

- PowerShellin Azure-moduulissa, versio 0.8.2 tai uudempi, paikalliseen tietokoneeseen asennettu. Voit tarkistaa, joihin on asennettu käyttämällä PowerShellin Azure-version **Get-moduulin azure | Muotoile taulukkoa-version** komento. Ohjeet ja linkin uusimpaan versioon Katso, [miten voit asentaa ja määrittää PowerShellin Azure](../powershell-install-configure.md). Kirjaudu sisään ja Azure tilauksen käyttämällä `Add-AzureAccount`.

- Kohteen virtuaalikoneen asennettuihin AM agentti.

Tarkista ensin, että AM-agentti on jo asennettu. Täytä cloud palvelunimi ja virtuaalikoneen nimi ja suorita sitten komento järjestelmänvalvojan PowerShellin Azure-komentokehotteeseen seuraavat komennot. Korvaa kaiken tarjoukset, mukaan lukien < ja > merkit.

    $CSName = "<cloud service name>"
    $VMName = "<virtual machine name>"
    $vm = Get-AzureVM -ServiceName $CSName -Name $VMName
    write-host $vm.VM.ProvisionGuestAgent

Jos et tiedä sen pilvipalvelussa ja virtuaalikoneen nimi, suorittamalla **Get-AzureVM** näytettävä kaikki näennäiskoneiden tiedot nykyisen tilauksesi.

Jos **Kirjoitus-host** -komento palauttaa arvon **Tosi**, AM-agentti asennettu. Jos se palauttaa arvon **Epätosi**, lue ohjeet ja lataa Azure blogin linkki lähettää [AM agentti ja laajennukset - osa 2](http://go.microsoft.com/fwlink/p/?LinkId=403947).

Jos AM-agentti on asennettu, suorita seuraavat komennot.

    $Agent = Get-AzureVMAvailableExtension TrendMicro.DeepSecurity -ExtensionName TrendMicroDSA

    Set-AzureVMExtension -Publisher TrendMicro.DeepSecurity –Version $Agent.Version -ExtensionName TrendMicroDSA -VM $vm | Update-AzureVM

## <a name="next-steps"></a>Seuraavat vaiheet

Kestää muutaman minuutin agentti Aloita käynnissä, kun se on asennettu. Sen jälkeen, sinun täytyy aktivoida virtuaalikoneen laaja suojaus, jotta se voi hallita laaja suojauksen hallinnan avulla. Lisätietoja on kohdassa:

- Tämä ratkaisu [Instant-On Cloud Security for Microsoft Azure](http://go.microsoft.com/fwlink/?LinkId=404101) trendi on artikkelissa
- [Esimerkki Windows PowerShell-komentosarjaa](http://go.microsoft.com/fwlink/?LinkId=404100) virtuaalikoneen määrittäminen
- Otosten [ohjeita](http://go.microsoft.com/fwlink/?LinkId=404099)

## <a name="additional-resources"></a>Lisäresursseja

[Käytössä Windows Server virtual tietokoneeseen kirjautuminen]

[Azure AM tunnisteet ja toiminnot]


<!--Link references-->
[Käytössä Windows Server virtual tietokoneeseen kirjautuminen]: virtual-machines-windows-classic-connect-logon.md
[Azure AM tunnisteet ja toiminnot]: http://go.microsoft.com/fwlink/p/?linkid=390493&clcid=0x409

<properties
    pageTitle="Palauta salasana tai Windows-AM etätyöpöydän kokoonpano | Microsoft Azure"
    description="Opi palauttamaan tilin salasanan tai Etätyöpöytä palveluita Azure portal tai PowerShellin Azure Windows-AM."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="iainfou"/>

# <a name="how-to-reset-the-remote-desktop-service-or-its-login-password-in-a-windows-vm"></a>Etätyöpöytä-palvelua tai sen kirjautumissalasana-Windows-AM palauttaminen

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Jos et saa yhteyttä Windows virtual machine (AM), voit paikallisen järjestelmänvalvojan salasanan tai palauta etätyöpöydän palvelun määritykset. Voit käyttää joko Azure portaalin tai AM Access-tunniste PowerShellin Azure salasanan. Jos käytössäsi on PowerShell, varmista, että on asennettu työn uusimman PowerShell-moduulin ja olet kirjautuneena Azure-tilaukseen. Lue lisätietoja kohdasta [asentaminen ja määrittäminen PowerShellin Azure](../powershell-install-configure.md).

> [AZURE.TIP] Voit tarkistaa version PowerShellin avulla on asennettuna`Import-Module Azure, AzureRM; Get-Module Azure, AzureRM | Format-Table Name, Version`

## <a name="windows-vms-in-resource-manager-deployment-model"></a>Windowsin Resurssienhallinta käyttöönoton mallin VMs

### <a name="azure-portal"></a>Azure Portal
Valitse oman AM napsauttamalla **Selaa** > **näennäiskoneiden** > *Windows virtuaalikoneen* > **kaikkia asetuksia** > **salasanan**. Vaihda salasana-sivu tulee näkyviin:

![Salasanan vaihtamiseen sivulla](./media/virtual-machines-windows-reset-rdp/Portal-RM-PW-Reset-Windows.png)

Kirjoita käyttäjänimi ja uusi salasana ja valitse sitten **Tallenna**. Yritä muodostaa yhteyttä AM uudelleen.

### <a name="vmaccess-extension-and-powershell"></a>VMAccess tunnisteen ja PowerShell

Varmista, että olet Azure PowerShell 1.0 tai uudempi versio ja olet kirjautunut tiliin seuraavasti `Login-AzureRmAccount` cmdlet-komento.

#### <a name="reset-the-local-administrator-account-password"></a>**Paikallisen järjestelmänvalvojan tilin salasanan vaihtaminen**

Voit palauttaa järjestelmänvalvojan salasanan tai käyttäjän nimi [Määrittäminen AzureRmVMAccessExtension](https://msdn.microsoft.com/library/mt619447.aspx) PowerShell-komennolla.

Luoda paikallisen järjestelmänvalvojan tilin tunnistetiedot käyttämällä seuraava komento:

    $cred=Get-Credential

Jos kirjoitat nykyisen tilin kuin eri nimellä, VMAccess tunniste-komennon nimeää paikallisen järjestelmänvalvojatili, määrittää kyseisen tilin salasana ja ongelmien etätyöpöydän uloskirjautuminen tapahtumasta. Jos paikallisen järjestelmänvalvojatili on poistettu käytöstä, VMAccess tunniste on sallinut sen käytön.

AM access-tunniste avulla voit määrittää uuden tunnistetietoja seuraavasti:

    Set-AzureRmVMAccessExtension -ResourceGroupName "myRG" -VMName "myVM" -Name "myVMAccess" `
        -Location WestUS -UserName $cred.GetNetworkCredential().Username `
        -Password $cred.GetNetworkCredential().Password -typeHandlerVersion "2.0"


Korvaa `myRG`, `myVM`, `myVMAccess`, ja sijainnin asetukset tärkeimpiä arvoilla.


#### <a name="reset-the-remote-desktop-service-configuration"></a>**Palauttaa etätyöpöydän palvelun määritykset**

Voit palauttaa etäkäyttöpalvelimen oman AM [Määrittäminen AzureRmVMExtension](https://msdn.microsoft.com/library/mt603745.aspx) tai [Aseta AzureRmVMAccessExtension](https://msdn.microsoft.com/library/mt619447.aspx)avulla seuraavasti. (Korvaa `myRG`, `myVM`, `myVMAccess` ja sijainnin oman arvot.)

    Set-AzureRmVMExtension -ResourceGroupName "myRG" -VMName "myVM" `
        -Name "myVMAccess" -ExtensionType "VMAccessAgent" -Location WestUS `
        -Publisher "Microsoft.Compute" -typeHandlerVersion "2.0"

Tai:<br>

    Set-AzureRmVMAccessExtension -ResourceGroupName "myRG" -VMName "myVM" `
        -Name "myVMAccess" -Location WestUS -typeHandlerVersion "2.0


> [AZURE.TIP] Molemmat komennot lisääminen uusi nimetty AM access agentti virtuaalikoneen. Milloin tahansa AM voi olla vain yksittäinen AM access agentti. AM access agentti ominaisuuksien määrittämiseen onnistuneesti, poista Määritä aiemmin käyttämällä joko access-agentti `Remove-AzureRmVMAccessExtension` tai `Remove-AzureRmVMExtension`. Lähtien PowerShellin Azure 1.2.2 versio, voit välttää tämän vaiheen käytettäessä `Set-AzureRmVMExtension` kanssa `-ForceRerun` vaihtoehto. Kun käytät `-ForceRerun`, varmista, että käyttämään AM access-agentti sama nimi kuin määrittäminen edellisen komennon.

Jos olet silti pysty muodostamaan etäyhteyden virtuaalikoneen, katso Lisää vaiheet [vianmääritys Etätyöpöytä yhteydet tietokoneeseen Azure virtual Windows-pohjaisesta](virtual-machines-windows-troubleshoot-rdp-connection.md), kokeile.


## <a name="windows-vms-in-the-classic-deployment-model"></a>Windows perinteinen käyttöönoton mallin VMs

### <a name="azure-portal"></a>Azure portal

Näennäiskoneiden luotu perinteinen käyttöönotto-mallin Palauta etätyöpöydän palvelun [Azure portal](https://portal.azure.com) avulla. Valitse: **Selaa** > **näennäiskoneiden (perinteinen)** > *Windows virtuaalikoneen* > **Palauta Remote...**. Seuraava sivu tulee näkyviin.

![Palauta RDP määritys-sivulla](./media/virtual-machines-windows-reset-rdp/Portal-RDP-Reset-Windows.png)

Voit kokeilla palauttamisesta nimi ja paikallisen järjestelmänvalvojan tilin salasana. Valitse: **Selaa** > **näennäiskoneiden (perinteinen)** > *Windows virtuaalikoneen* > **kaikkia asetuksia** > **salasanan**. Seuraava sivu tulee näkyviin.

![Salasanan vaihtamiseen sivulla](./media/virtual-machines-windows-reset-rdp/Portal-PW-Reset-Windows.png)

Kun kirjoitat uuden käyttäjänimi ja salasana, valitse **Tallenna**.

### <a name="vmaccess-extension-and-powershell"></a>VMAccess tunnisteen ja PowerShell

Varmista, että AM-agentti asennettu virtuaalikoneen. VMAccess-tunniste on ei voi asentaa ennen kuin voit käyttää sitä, kunhan AM-agentti ei ole käytettävissä. Tarkista AM-agentti on jo asennettu seuraavat-komennon avulla. (Korvaa "myCloudService" ja "myVM" pilvipalvelussa sijaitsevaan ja että AM nimet tarpeen mukaan. Opit seuraavat nimet suorittamalla `Get-AzureVM` ilman parametreja.)

    $vm = Get-AzureVM -ServiceName "myCloudService" -Name "myVM"
    write-host $vm.VM.ProvisionGuestAgent

Jos **Kirjoitus-host** -komento näkyy **Tosi**, AM-agentti asennettu. Jos näyttää **Epätosi**, katso ohjeet ja lataa [AM agentti ja laajennukset - osa 2](http://go.microsoft.com/fwlink/p/?linkid=403947&clcid=0x409) Azure blogikirjoituksessa linkki.

Jos olet luonut virtuaalikoneen-portaalissa, tarkista, onko `$vm.GetInstance().ProvisionGuestAgent` palauttaa arvon **Tosi**. Jos et voi määrittää tällä komennolla:

    $vm.GetInstance().ProvisionGuestAgent = $true

Tämä komento estää seuraavan virheen, kun käytät **Määrittäminen AzureVMExtension** -komento seuraavat vaiheet: "Säännöstä Vieras agentti on otettava käyttöön AM objektin ennen kuin määrität IaaS AM Access-tunniste."

#### <a name="reset-the-local-administrator-account-password"></a>**Paikallisen järjestelmänvalvojan tilin salasanan vaihtaminen**

Luo kirjautumisen tunnistetiedot nykyisen paikallisen järjestelmänvalvojan tilin nimi ja uusi salasana uudelleen ja suorita `Set-AzureVMAccessExtension` seuraavasti.

    $cred=Get-Credential
    Set-AzureVMAccessExtension –vm $vm -UserName $cred.GetNetworkCredential().Username `
        -Password $cred.GetNetworkCredential().Password  | Update-AzureVM

Jos kirjoitat nykyisen tilin kuin eri nimellä, VMAccess-tunniste nimeää paikallisen järjestelmänvalvojatili, määrittää kyseisen tilin salasana ja kirjaudu ulos etätyöpöydän ongelmien vianmääritys. Jos paikallisen järjestelmänvalvojatili on poistettu käytöstä, VMAccess tunniste on sallinut sen käytön.

Nämä komennot Palauta myös etätyöpöydän palvelun määritykset.

#### <a name="reset-the-remote-desktop-service-configuration"></a>**Palauttaa etätyöpöydän palvelun määritykset**

Etätyöpöytä palvelun määritykset vaihtamaan suorittamalla seuraavan komennon:

    Set-AzureVMAccessExtension –vm $vm | Update-AzureVM

VMAccess-tunniste suoritetaan kaksi komennot virtuaalikoneen:

- `netsh advfirewall firewall set rule group="Remote Desktop" new enable=Yes`

Tämä komento ottaa käyttöön valmiin Windowsin palomuuri-ryhmän, jonka avulla etätyöpöydän liikenteen, joka käyttää porttinumeroa 3389.

- `Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server' -name "fDenyTSConnections" -Value 0`

Tämä komento asettaa fDenyTSConnections rekisteriarvon 0, ottaminen käyttöön etätyöpöytä yhteydet.


## <a name="next-steps"></a>Seuraavat vaiheet

Jos et pysty vaihtamaan salasanan Azure AM access-tunniste ei vastaa, voit [palauttaa paikallisen Windowsin salasanan offline-tilassa](virtual-machines-windows-reset-local-password-without-agent.md). Tämä menetelmä on edistynyt prosessi ja edellyttää, että voit muodostaa yhteyden toiseen AM ongelmallinen AM virtual kiintolevylle. Noudattamalla tässä artikkelissa kerrotaan ensin ja yritä vain auta offline-tilassa salasanan palauttaminen-menetelmää.

[Azure AM tunnisteet ja toiminnot](virtual-machines-windows-extensions-features.md)

[Yhteyden muodostaminen Azure virtual-koneen RDP tai SSH](http://msdn.microsoft.com/library/azure/dn535788.aspx)

[Etätyöpöytä yhteydet tietokoneeseen Azure virtual Windows-pohjaisesta vianmääritys](virtual-machines-windows-troubleshoot-rdp-connection.md)

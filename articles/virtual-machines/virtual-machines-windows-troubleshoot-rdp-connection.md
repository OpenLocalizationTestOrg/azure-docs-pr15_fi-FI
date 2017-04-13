<properties
    pageTitle="Ei RDP Azure AM | Microsoft Azure"
    description="Ongelmien vianmääritys, kun et voi muodostaa Windows virtuaalikoneen etätyöpöydän Azure-tietokannassa"
    keywords="Remote desktop-virhe, virhe Etätyöpöytäyhteys ei voi muodostaa yhteyttä AM, Etätyöpöydän vianmääritys"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="top-support-issue,azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="support-article"
    ms.date="10/26/2016"
    ms.author="iainfou"/>

# <a name="troubleshoot-remote-desktop-connections-to-an-azure-virtual-machine"></a>Azure virtuaalikoneen etätyöpöydän yhteyden vianmääritys

Windows-pohjaisesta Azure virtuaalikoneen (AM) Remote Desktop Protocol (RDP) yhteys voi epäonnistua eri syistä, jolloin ei voi käyttää omaa AM. Ongelma voi olla Etätyöpöytä-palvelussa AM, verkkoyhteys tai etätyöpöydän asiakkaan tietokoneeseen. Tässä artikkelissa ohjaa läpi joitakin yleisimmät menetelmistä RDP yhteysongelmien ratkaisemiseen. 

Jos tarvitset apua milloin tahansa tämän artikkelin, ota Azure asiantuntijoille [MSDN Azure](https://azure.microsoft.com/support/forums/)ja Pinon ylivuoto keskustelupalstoilla. Vaihtoehtoisesti voit jättää Azure tuki tapahtuma. Siirry [Azure tukevat sivuston](https://azure.microsoft.com/support/options/) ja **Pyydä tuki**.

<a id="quickfixrdp"></a>

## <a name="quick-troubleshooting-steps"></a>Nopea vianmääritysohjeita
Kokeile vianmäärityksen jokaisen vaiheen jälkeen muodostetaan uudelleen AM:

1. Palauta etätyöpöydän määritys.
2. Tarkista verkon käyttöoikeusryhmän säännöt / Cloud Services päätepisteet.
3. Tarkista AM konsolin lokitiedot.
4. AM resurssin kuntotietojen tarkistus.
5. Vaihda AM salasana.
6. Käynnistä oman AM.
7. Ota yhteyttä AM uudelleen.

Jatka lukemista, jos tarvitset tarkempia ohjeita ja selitykset.

> [AZURE.TIP] Jos et ole yhteydessä, Azure [Express reititys](../expressroute/expressroute-introduction.md) tai [sivuston sivuston VPN](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) -yhteyden kautta portaalissa oman AM **Yhdistä** -painike näkyy harmaana, joudut Luo ja määritä oman AM julkiseen IP-osoite, ennen kuin voit käyttää RDP. Voit lukea lisää tietoja [julkisten IP-osoitteiden Azure-tietokannassa](../virtual-network/virtual-network-ip-addresses-overview-arm.md).


## <a name="ways-to-troubleshoot-rdp-issues"></a>Tapoja RDP-ongelmien vianmääritys
Voit tehdä VMs luotu resurssien hallinnan käyttöönottomalli jollakin seuraavista tavoista:

- [Azure portal](#using-the-azure-portal) - hyvien, jos haluat palauttaa nopeasti RDP määritysten tai käyttäjän tunnistetiedot ja ei ole asennettu Azure-työkalut.
- [PowerShellin azure](#using-azure-powershell) - Jos olet perehtynyt siihen PowerShell-kehotteessa palauttaa nopeasti käyttämällä Azure PowerShellin cmdlet-komennot RDP määritysten tai käyttäjän tunnistetiedot.

Voit etsiä myös vaiheet vianmäärityksen VMs luotu [perinteinen käyttöönotto-malli](#troubleshoot-vms-created-using-the-classic-deployment-model).


<a id="fix-common-remote-desktop-errors"></a>
## <a name="troubleshoot-using-the-azure-portal"></a>Azure-portaalissa vianmääritys
Vianmäärityksen jokaisen vaiheen jälkeen yritä muodostaa yhteyttä AM uudelleen. Jos yhteyden muodostaminen ei vieläkään onnistu, kokeile seuraavaan vaiheeseen.

1. **Palauta RDP-yhteys**. Tämä vianmäärityksen vaihe palauttaa RDP-määritys, kun etäyhteyksien on poistettu käytöstä tai Windowsin palomuurin säännöt estävät RDP, kuten.

    Valitse oman AM Azure-portaalissa. Selaa luettelon loppuun lähellä **tuki + vianmääritys** kohtaan asetukset-ruudussa. Napsauta **Vaihda salasana** -painiketta. Määritä **tilassa** voit **palauttaa vain määritys** ja valitse sitten **Päivitä** -painiketta:

    ![Palauttaa Azure-portaalissa RDP-määritykset](./media/virtual-machines-windows-troubleshoot-rdp-connection/reset-rdp.png)

2. **Varmista verkon käyttöoikeusryhmän säännöt**. Vianmäärityksen tämän vaiheen tarkistaa, että sääntöä verkon suojaus-ryhmässä sallimaan RDP tietoliikenne. RDP oletusportti on porttinumeroa 3389. Säännön sallimaan RDP tietoliikenne ei voidaan luoda automaattisesti, kun luot oman AM.

    Valitse oman AM Azure-portaalissa. Valitse Asetukset-ruudun **verkkoliittymät** .

    ![Näytä verkkoliittymät AM Azure-portaalissa](./media/virtual-machines-windows-troubleshoot-rdp-connection/select-network-interfaces.png)

    Valitse verkkoliittymän (ei yleensä vain yksi)-luettelosta:

    ![Valitse verkkoliittymän Azure-portaalissa](./media/virtual-machines-windows-troubleshoot-rdp-connection/select-interface.png)

    Valitse **Verkko-käyttöoikeusryhmän** tarkastelemiseen liittyvät verkon-liittymän verkko-käyttöoikeusryhmän:

    ![Valitse Security verkkoryhmän Azure-portaalissa](./media/virtual-machines-windows-troubleshoot-rdp-connection/select-nsg.png)

    Varmista, että saapuvan liikenteen sääntö on, joka sallii RDP liikenne porttinumeroa 3389. Seuraavassa esimerkissä on kelvollinen suojaussäännön, joka sallii RDP-liikenne. Voit tarkastella `Service` ja `Action` on määritetty oikein:

    ![Tarkista RDP NSG säännön Azure-portaalissa](./media/virtual-machines-windows-troubleshoot-rdp-connection/verify-nsg-rules.png)

    Jos sinulla ei ole säännön, joka sallii RDP-liikenne, [Verkko-käyttöoikeusryhmän säännön luominen](virtual-machines-windows-nsg-quickstart-portal.md). Salli porttinumeroa 3389.

3. **Tarkista AM käynnistyksen diagnostiikka**. Tämä vianmäärityksen vaihe tarkistaa määrittääksesi, jos AM Raportoi ongelma AM console-lokit. Kaikki VMs on käynnistyksen diagnostiikka käytössä, jotta tämä vianmäärityksen vaihe voi olla valinnainen.
    
    Tietyn vianmääritysohjeita on artikkelissa laajemmin, mutta saattaa johtua leveämpi ongelman, joka on vaikuttavia RDP-yhteys. Saat lisätietoja console-lokit ja AM näyttökuva tarkistaminen [Käynnistyksen diagnostiikkaa VMs](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/).

4. **AM resurssin kuntotietojen tarkistus**. Vianmäärityksen tämän vaiheen tarkistaa on ei Azure alustaksi, joka voi olla vaikutusta AM yhteys tunnetut ongelmat.

    Valitse oman AM Azure-portaalissa. Selaa luettelon loppuun lähellä **tuki + vianmääritys** kohtaan asetukset-ruudussa. Napsauta **resurssin kunto** -painiketta. Kunnossa AM raportoi siten, että **käytettävissä**:

    ![Tarkista AM resurssin kunto Azure-portaalissa](./media/virtual-machines-windows-troubleshoot-rdp-connection/check-resource-health.png)

5. **Palauta käyttäjän tunnistetietoja**. Vianmäärityksen tämän vaiheen palauttaa paikallisen järjestelmänvalvojatilin salasana, kun ole varma, tai olet unohtanut tunnistetiedot.

    Valitse oman AM Azure-portaalissa. Selaa luettelon loppuun lähellä **tuki + vianmääritys** kohtaan asetukset-ruudussa. Napsauta **Vaihda salasana** -painiketta. Varmista, että **tila** on määritetty **salasanan** ja kirjoita käyttäjänimesi ja uusi salasana. Valitse lopuksi **Päivitä** -painiketta:

    ![Palauta käyttäjän tunnistetietoja Azure-portaalissa](./media/virtual-machines-windows-troubleshoot-rdp-connection/reset-password.png)

6. **Oman AM uudelleen**. Vianmäärityksen tässä vaiheessa voit korjata pohjana AM itse aiheuttaa ongelmia.

    Valitse oman AM Azure-portaalissa ja valitse **Yleistä** -välilehti. Napsauta **Käynnistä** -painiketta:

    ![Käynnistä AM Azure-portaalissa](./media/virtual-machines-windows-troubleshoot-rdp-connection/restart-vm.png)

7. **Ota yhteyttä AM uudelleen**. Tämä vianmäärityksen vaihe redeploys oman toiseen isäntään sisällä Azure tahansa pohjana ympäristössä tai ongelmien korjaaminen AM.

    Valitse oman AM Azure-portaalissa. Selaa luettelon loppuun lähellä **tuki + vianmääritys** kohtaan asetukset-ruudussa. **Ota uudelleen** -painiketta ja valitse sitten **Ota uudelleen**:

    ![Ota uudelleen AM Azure-portaalissa](./media/virtual-machines-windows-troubleshoot-rdp-connection/redeploy-vm.png)

    Kun tämä toiminto on valmis, tilapäiset levyn tiedot menetetään ja dynaamisia IP-osoitteita, jotka liittyvät AM päivitetään.

Jos kohtaat edelleen RDP ongelmat, voit [avata tukipyynnön](https://azure.microsoft.com/support/options/) tai Lue [Lisää RDP vianmäärityksen käsitteitä ja vaiheet](virtual-machines-windows-detailed-troubleshoot-rdp.md).


## <a name="troubleshoot-using-azure-powershell"></a>PowerShellin Azure käyttämiseen liittyviä ongelmia
Jos et ole jo, [Asenna ja määritä uusimman PowerShellin Azure](../powershell-install-configure.md).

Seuraavissa esimerkeissä käytetään muuttujat `myResourceGroup`, `myVM`, ja `myVMAccessExtension`. Korvaa oman arvot näiden muuttujien nimissä ja sijainnit.

> [AZURE.NOTE] Voit palauttaa käyttäjän tunnistetietoja ja RDP-asetusten [Määrittäminen AzureRmVMAccessExtension](https://msdn.microsoft.com/library/mt619447.aspx) PowerShell-cmdlet-komennolla. Seuraavissa esimerkeissä `myVMAccessExtension` on nimi, jonka määrittää yhteydessä. Jos ole aiemmin käyttänyt VMAccessAgent, voit avata aiemmin tunnisteen nimi käyttämällä `Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"` tarkistamaan, AM ominaisuudet. Voit tarkastella nimeä, tarkista tulos "Laajennukset"-osassa.

Vianmäärityksen jokaisen vaiheen jälkeen yritä muodostaa yhteyttä AM uudelleen. Jos yhteyden muodostaminen ei vieläkään onnistu, kokeile seuraavaan vaiheeseen.

1. **Palauta RDP-yhteys**. Tämä vianmäärityksen vaihe palauttaa RDP-määritys, kun etäyhteyksien on poistettu käytöstä tai Windowsin palomuurin säännöt estävät RDP, kuten.

    Seuraa esimerkki palauttaa AM, jonka nimi on RDP-yhteyden `myVM` - `WestUS` sijainti ja nimeltä resurssiryhmä `myResourceGroup`:

    ```powershell
    Set-AzureRmVMAccessExtension -ResourceGroupName "myResourceGroup" `
        -VMName "myVM" -Location Westus -Name "myVMAccessExtension"
    ```

2. **Varmista verkon käyttöoikeusryhmän säännöt**. Vianmäärityksen tämän vaiheen varmistaa, että sääntöä verkon suojaus-ryhmässä sallimaan RDP tietoliikenne. RDP oletusportti on porttinumeroa 3389. Säännön sallimaan RDP tietoliikenne ei voidaan luoda automaattisesti, kun luot oman AM.

    Määritä ensin kaikki määritystietoja verkon suojaus-ryhmä `$rules` muuttuja. Seuraavassa esimerkissä hakee tietoja verkko-käyttöoikeusryhmän nimeltä `myNetworkSecurityGroup` resurssiryhmän nimeltä `myResourceGroup`:

    ```powershell
    $rules = Get-AzureRmNetworkSecurityGroup -ResourceGroupName "myResourceGroup" `
        -Name "myNetworkSecurityGroup"
    ```

    Nyt tarkastella säännöt, jotka on määritetty verkon käyttöoikeusryhmän. Varmista, että sääntö on sallimaan saapuvia yhteyksiä porttinumeroa 3389 seuraavasti:

    ```powershell
    $rules.SecurityRules
    ```

    Seuraavassa esimerkissä on kelvollinen suojaussäännön, joka sallii RDP-liikenne. Voit tarkastella `Protocol`, `DestinationPortRange`, `Access`, ja `Direction` on määritetty oikein:

    ```powershell
    Name                     : default-allow-rdp
    Id                       : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/securityRules/default-allow-rdp
    Etag                     : 
    ProvisioningState        : Succeeded
    Description              : 
    Protocol                 : TCP
    SourcePortRange          : *
    DestinationPortRange     : 3389
    SourceAddressPrefix      : *
    DestinationAddressPrefix : *
    Access                   : Allow
    Priority                 : 1000
    Direction                : Inbound
    ```

    Jos sinulla ei ole säännön, joka sallii RDP-liikenne, [Verkko-käyttöoikeusryhmän säännön luominen](virtual-machines-windows-nsg-quickstart-powershell.md). Salli porttinumeroa 3389.

3. **Palauta käyttäjän tunnistetietoja**. Tämä vianmäärityksen vaihe palauttaa, voit määrittää, kun ole varma, tai olet unohtanut tunnistetiedot paikallisen järjestelmänvalvojan tilin salasana.

    Määritä ensin määrittämällä tunnukset käyttäjänimi ja uusi salasana `$cred` muuttujan seuraavasti:

    ```powershell
    $cred=Get-Credential
    ```

    Nyt päivittää oman AM tunnistetietoja. Seuraavassa esimerkissä päivittää AM, jonka nimi on tunnistetietoja `myVM` - `WestUS` sijainti ja nimeltä resurssiryhmä `myResourceGroup`:

    ```powershell
    Set-AzureRmVMAccessExtension -ResourceGroupName "myResourceGroup" `
        -VMName "myVM" -Location WestUS -Name "myVMAccessExtension" `
        -UserName $cred.GetNetworkCredential().Username `
        -Password $cred.GetNetworkCredential().Password
    ```

4. **Oman AM uudelleen**. Vianmäärityksen tässä vaiheessa voit korjata pohjana AM itse aiheuttaa ongelmia.

    Seuraavassa esimerkissä käynnistyy nimeltä AM `myVM` resurssiryhmän nimeltä `myResourceGroup`:

    ```powershell
    Restart-AzureRmVM -ResourceGroup "myResourceGroup" -Name "myVM"
    ```

5. **Ota yhteyttä AM uudelleen**. Tämä vianmäärityksen vaihe redeploys oman toiseen isäntään sisällä Azure tahansa pohjana ympäristössä tai ongelmien korjaaminen AM.

    Seuraavassa esimerkissä redeploys nimeltä AM `myVM` - `WestUS` sijainti ja nimi resurssiryhmä `myResourceGroup`:

    ```powershell
    Set-AzureRmVM -Redeploy -ResourceGroupName "myResourceGroup" -Name "myVM"
    ```

Jos kohtaat edelleen RDP ongelmat, voit [avata tukipyynnön](https://azure.microsoft.com/support/options/) tai Lue [Lisää RDP vianmäärityksen käsitteitä ja vaiheet](virtual-machines-windows-detailed-troubleshoot-rdp.md).


## <a name="troubleshoot-vms-created-using-the-classic-deployment-model"></a>Perinteinen käyttöönotto-mallin avulla luotu VMs vianmääritys

Vianmäärityksen jokaisen vaiheen jälkeen Yritä yhdistää uudelleen AM.

1. **Palauta RDP-yhteys**. Tämä vianmäärityksen vaihe palauttaa RDP-määritys, kun etäyhteyksien on poistettu käytöstä tai Windowsin palomuurin säännöt estävät RDP, kuten.

    Valitse oman AM Azure-portaalissa. Valitse **... Lisää** -painiketta ja valitse sitten **Palauta etäkäyttö**:

    ![Palauttaa Azure-portaalissa RDP-määritykset](./media/virtual-machines-windows-troubleshoot-rdp-connection/classic-reset-rdp.png)

2.  **Tarkista pilvipalveluihin päätepisteet**. Vianmäärityksen tämän vaiheen tarkistaa, että sinulla on päätepisteet Cloud Services-palveluissa sallimaan RDP tietoliikenne. RDP oletusportti on porttinumeroa 3389. Säännön sallimaan RDP tietoliikenne ei voidaan luoda automaattisesti, kun luot oman AM.

    Valitse oman AM Azure-portaalissa. Näytä oman AM määritetyt päätepisteet **päätepisteet** -painiketta. Varmista, että päätepisteet olemassa, joka sallii RDP liikenteen porttinumeroa 3389.
    
    Seuraavassa esimerkissä esitetään kelvollinen päätepisteet, jotka sallivat RDP liikenne:

    ![Tarkista pilvipalveluihin päätepisteet Azure-portaalissa](./media/virtual-machines-windows-troubleshoot-rdp-connection/classic-verify-cloud-services-endpoints.png)

    Jos sinulla ei ole päätepisteen, joka sallii RDP-liikenne, [Luo pilvipalveluihin päätepiste](virtual-machines-windows-classic-setup-endpoints.md). Salli yksityisten portti 3389 TCP.

3. **Tarkista AM käynnistyksen diagnostiikka**. Tämä vianmäärityksen vaihe tarkistaa määrittääksesi, jos AM Raportoi ongelma AM console-lokit. Kaikki VMs on käynnistyksen diagnostiikka käytössä, jotta tämä vianmäärityksen vaihe voi olla valinnainen.
    
    Tietyn vianmääritysohjeita on artikkelissa laajemmin, mutta saattaa johtua leveämpi ongelman, joka on vaikuttavia RDP-yhteys. Saat lisätietoja console-lokit ja AM näyttökuva tarkistaminen [Käynnistyksen diagnostiikkaa VMs](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/).

4. **AM resurssin kuntotietojen tarkistus**. Vianmäärityksen tämän vaiheen tarkistaa on ei Azure alustaksi, joka voi olla vaikutusta AM yhteys tunnetut ongelmat.

    Valitse oman AM Azure-portaalissa. Selaa luettelon loppuun lähellä **tuki + vianmääritys** kohtaan asetukset-ruudussa. Napsauta **Resurssin kunto** -painiketta. Kunnossa AM raportoi siten, että **käytettävissä**:

    ![Tarkista AM resurssin kunto Azure-portaalissa](./media/virtual-machines-windows-troubleshoot-rdp-connection/classic-check-resource-health.png)

5. **Palauta käyttäjän tunnistetietoja**. Tämä vianmäärityksen vaihe palauttaa, voit määrittää, kun ole varma, tai olet unohtanut tunnistetiedot paikallisen järjestelmänvalvojan tilin salasana.

    Valitse oman AM Azure-portaalissa. Selaa luettelon loppuun lähellä **tuki + vianmääritys** kohtaan asetukset-ruudussa. Napsauta **Vaihda salasana** -painiketta. Kirjoita käyttäjänimesi ja uusi salasana. Valitse lopuksi **Tallenna** -painiketta:

    ![Palauta käyttäjän tunnistetietoja Azure-portaalissa](./media/virtual-machines-windows-troubleshoot-rdp-connection/classic-reset-password.png)

6. **Oman AM uudelleen**. Vianmäärityksen tässä vaiheessa voit korjata pohjana AM itse aiheuttaa ongelmia.

    Valitse oman AM Azure-portaalissa ja valitse **Yleistä** -välilehti. Napsauta **Käynnistä** -painiketta:

    ![Käynnistä AM Azure-portaalissa](./media/virtual-machines-windows-troubleshoot-rdp-connection/classic-restart-vm.png)
    
Jos kohtaat edelleen RDP ongelmat, voit [avata tukipyynnön](https://azure.microsoft.com/support/options/) tai Lue [Lisää RDP vianmäärityksen käsitteitä ja vaiheet](virtual-machines-windows-detailed-troubleshoot-rdp.md).


## <a name="troubleshoot-specific-rdp-errors"></a>Tietyn RDP vianmääritys
Voit kohdata tietyn virhesanoman, kun yrität muodostaa yhteyttä AM RDP kautta. Seuraavassa luetellaan yleisimmät virhesanomat:

- [Etäistunto katkesi, koska ei ole Remote työpöydän palvelimet voi antaa käyttöoikeuden](virtual-machines-windows-troubleshoot-specific-rdp-errors.md#rdplicense).
- [Etätyöpöytä ei löydä tietokoneen "nimi"](virtual-machines-windows-troubleshoot-specific-rdp-errors.md#rdpname).
- [Todennus on virhe. Paikallinen suojaus viranomainen ei voi muodostaa yhteyttä](virtual-machines-windows-troubleshoot-specific-rdp-errors.md#rdpauth).
- [Windowsin suojaus-virhe: tunnistetietoja ei ollut](virtual-machines-windows-troubleshoot-specific-rdp-errors.md#wincred).
- [Tämä tietokone ei voi muodostaa etätietokone](virtual-machines-windows-troubleshoot-specific-rdp-errors.md#rdpconnect).


## <a name="additional-resources"></a>Lisäresursseja
Jos mikään näistä virheitä on tapahtunut ja edelleen ei voi muodostaa AM etätyöpöydän kautta, lue yksityiskohtaiset [vianmääritysoppaan etätyöpöydän](virtual-machines-windows-detailed-troubleshoot-rdp.md).

- [Azure IaaS (Windows) diagnostiikka-paketti](https://home.diagnostics.support.microsoft.com/SelfHelp?knowledgebaseArticleFilter=2976864)
- Käytettäessä AM sovellusten vianmääritysohjeita on artikkelissa [vianmääritys Azure-AM käytössä-sovellukseen](virtual-machines-linux-troubleshoot-app-connection.md).
- Jos suojattu runko (SSH) avulla voit muodostaa yhteyden Linux-AM Azure-tietokannassa on vaikeuksia, lue [vianmäärityksen SSH yhteydet Linux-AM Azure-tietokannassa](virtual-machines-linux-troubleshoot-ssh-connection.md).

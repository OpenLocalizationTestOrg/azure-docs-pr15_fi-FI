<properties
   pageTitle="Paikallisen Windows-salasanan palauttaminen, kun Azure Vieras-agentti ei ole asennettu | Microsoft Azure"
   description="Miten salasanan paikallisen Windows-käyttäjätili kun Azure Vieras-agentti ei ole asennettu tai toimii AM"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/05/2016"
   ms.author="iainfou"/>

# <a name="how-to-reset-local-windows-password-for-azure-vm"></a>Miten salasanan paikallisen Windows Azure AM
Voit palauttaa paikallisen AM Azure käyttämisen [Azure portal tai PowerShellin Azure](virtual-machines-windows-reset-rdp.md) annettu Azure Vieras-agentti on asennettu Windows salasanan. Tämä tapa on ensisijainen tapa salasanan Azure-AM. Jos kohtaat ongelmia ja Azure Vieras-agentti ei vastaa tai kaatuvat asentamiseksi ladattuasi mukautetun kuvan, voit manuaalisesti palauttaa Windows-salasana. Tässä artikkelissa kuvataan, miten paikallisen tilin salasanan liittämällä tietolähteen OS virtual levyn toiseen AM. 

> [AZURE.WARNING] Käytä vain tätä prosessia auta. Yritä aina salasanan avulla [Azure portal tai PowerShellin Azure](virtual-machines-windows-reset-rdp.md) ensin.


## <a name="overview-of-the-process"></a>Prosessin
Core vaiheet suoritetaan paikallisen salasanan palauttaminen ollessa Azure Vieras-agentti ei voi käyttää Windows-AM Azure-tietokannassa on seuraavanlainen:

- Poista lähde AM. Virtuaalinen levyjen säilyvät.
- Lähde-AM OS levyn liittäminen toisesta AM Azure-tilauksen piiriin kuuluvien. Tämä AM kutsutaan vianmäärityksen AM.
- Luo lähde-AM OS levyllä config tiedostoja avulla vianmäärityksen AM.
- Irrota AM OS levyltä vianmäärityksen AM.
- Resurssienhallinta-mallin avulla voit luoda AM, alkuperäinen virtual levyn avulla.
- Kun uusi AM käynnistyy, config tiedostot Päivitä tarvittavat käyttäjän salasanan.


## <a name="detailed-steps"></a>Yksityiskohtaisia ohjeita
Yritä aina salasanan avulla [Azure portal tai PowerShellin Azure](virtual-machines-windows-reset-rdp.md) ennen kuin yrität seuraavien ohjeiden mukaisesti. Varmista, että sinulla on oman AM varmuuskopion, ennen kuin aloitat. 

1. Poista tarvittavien AM Azure-portaalissa. Poistaminen AM poistaa vain metatietojen sisällä Azure AM viittaus. Virtuaalinen levyjen säilyvät, kun AM poistetaan:

    - Valitse AM Azure-portaalissa, napsauta *poistaminen*:

    ![Poista aiemmin AM](./media/virtual-machines-windows-reset-local-password-without-guest-agent/delete_vm.png)

2. Liittää lähde-AM OS levyn vianmäärityksen AM. Vianmäärityksen AM on oltava sama kuin lähteen AM OS levyn alue (kuten `West US`):

    - Valitse vianmäärityksen AM Azure-portaalissa. Valitse *levyjen* | *Liitä aiemmin luotu*:

    ![Liitä aiemmin levy](./media/virtual-machines-windows-reset-local-password-without-guest-agent/disks_attach_existing.png)

    Valitse *Näennäiskiintolevytiedosto* ja valitse sitten tallennustilan-tili, joka sisältää lähde AM:

    ![Tallennustilan tilin valitseminen](./media/virtual-machines-windows-reset-local-password-without-guest-agent/disks_select_storageaccount.PNG)

    Valitse lähde-säilö. Lähde-säilö on yleensä *näennäiskiintolevyjen*:

    ![Valitse tallennustilan säilö](./media/virtual-machines-windows-reset-local-password-without-guest-agent/disks_select_container.png)

    Valitse liitettävä OS näennäiskiintolevyn. Valitse Viimeistele prosessi *valitsemalla* :

    ![Valitse tietolähteen virtual levy](./media/virtual-machines-windows-reset-local-password-without-guest-agent/disks_select_source_vhd.png)

3. Muodostaa yhteyden vianmäärityksen AM etätyöpöydän ja varmistaa lähde-AM OS levy on näkyvissä:

    - Valitse vianmäärityksen AM Azure-portaalissa, ja valitse *Yhdistä*.
    - Avaa RDP-tiedosto, joka lataa. Kirjoita käyttäjänimi ja salasana, vianmäärityksen AM.
    - Etsi tietoja levy, liitetty Resurssienhallinnassa. Jos lähde on AM Näennäiskiintolevyn on liitetty vianmäärityksen AM vain tiedot-levyn, sen on oltava f-asema:

    ![Näytä liitetyt tiedot levy](./media/virtual-machines-windows-reset-local-password-without-guest-agent/troubleshooting_vm_fileexplorer.png)

4. Luo `gpt.ini` - `\Windows\System32\GroupPolicy` lähde-AM asemassa (Jos on olemassa gpt.ini, nimeä uudelleen gpt.ini.bak):

    > [AZURE.WARNING] Varmista, että et vahingossa luo seuraavat tiedostot kansiossa C:\Windows, vianmäärityksen AM OS-asema. Luo lähde AM, joka on liitteenä tietojen DVD-levyllä OS asemasta seuraavat tiedostot.

    - Lisää seuraavat rivit kyselyjä `gpt.ini` luomasi tiedosto:

    ```
    [General]
    gPCFunctionalityVersion=2
    gPCMachineExtensionNames=[{42B5FAAE-6536-11D2-AE5A-0000F87571E3}{40B6664F-4972-11D1-A7CA-0000F87571E3}]
    Version=1
    ```

    ![Luo gpt.ini](./media/virtual-machines-windows-reset-local-password-without-guest-agent/create_gpt_ini.png)
 
5. Luo `scripts.ini` - `\Windows\System32\GroupPolicy\Machine\Scripts`. Varmista, että piilotetut kansiot ovat näkyvissä. Luo tarvittaessa `Machine` tai `Scripts` kansiot.

    - Lisää seuraavat rivit `scripts.ini` luomasi tiedosto:

    ```
    [Startup]
    0CmdLine=C:\Windows\System32\FixAzureVM.cmd
    0Parameters=
    ```

    ![Luo scripts.ini](./media/virtual-machines-windows-reset-local-password-without-guest-agent/create_scripts_ini.png)
 
6. Luo `FixAzureVM.cmd` - `\Windows\System32` seuraavat sisällöllä korvaaminen `<username>` ja `<newpassword>` oman arvot:

    ```
    NET USER <username> <newpassword>
    ```

    ![Luo FixAzureVM.cmd](./media/virtual-machines-windows-reset-local-password-without-guest-agent/create_fixazure_cmd.png)

    Oman AM määritetty salasana monimutkaisuus vaatimukset on täytettävä määritettäessä uutta salasanaa.

7. Azure-portaalissa irrottaa vianmäärityksen AM levyltä:

    - Valitse vianmäärityksen AM Azure-portaalissa- *levyjä*.
    - Valitse vaiheessa 2 liitetty tietojen levy, valitse *Katkaise yhteys*:

    ![Irrota levy](./media/virtual-machines-windows-reset-local-password-without-guest-agent/detach_disk.png)

8. Ennen kuin luot AM, hanki lähde OS levylle URI:

    - Valitse tallennustilan tilin Azure-portaalissa- *BLOB-objektit*.
    - Valitse säilö. Lähde-säilö on yleensä *näennäiskiintolevyjen*:

    ![Valitse tallennustilan tilin blob](./media/virtual-machines-windows-reset-local-password-without-guest-agent/select_storage_details.png)

    Valitse tietolähteen AM OS Näennäiskiintolevyn ja valitse *Kopioi* -painike *URL* -nimi-kohdan vieressä:

    ![Kopioi URI](./media/virtual-machines-windows-reset-local-password-without-guest-agent/copy_source_vhd_uri.png)

9. Luo lähde-AM OS levyltä AM:

    - [Azure Resurssienhallinta tämän mallin](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd) avulla voit luoda AM erityinen Näennäiskiintolevyn. Valitse `Deploy to Azure` -painikkeen napsauttaminen avaa Azure portaalin malliin perustuvan yksityiskohdilla täydennetty puolestasi.
    - Jos haluat säilyttää kaikki AM edellisiin asetuksiin, valitse aiemmin luotu VNet, aliverkon, verkkosovittimen tai julkiseen IP *Muokkaa mallia* .
    - Valitse `OSDISKVHDURI` parametrin tekstiruutu, Liitä lähde Näennäiskiintolevyn URI hankkia edellisessä vaiheessa:

    ![AM luominen mallista](./media/virtual-machines-windows-reset-local-password-without-guest-agent/create_new_vm_from_template.png)

10. Sen jälkeen, kun uusi AM on käynnissä, muodosta yhteys etätyöpöydän määrittämäsi uudella salasanalla AM `FixAzureVM.cmd` komentosarjan.

11. Poista uusi AM remote istunnon-ympäristön tyhjentääksesi seuraavat tiedostot:

    - Valitse %windir%\System32
        - Poista FixAzureVM.cmd
    - Valitse %windir%\System32\GroupPolicy\Machine\
        - Poista scripts.ini
    - Valitse %windir%\System32\GroupPolicy
        - Poista gpt.ini (Jos gpt.ini oli ennen ja sen nimetty gpt.ini.bak .bak-tiedosto takaisin gpt.ini nimeä)

## <a name="next-steps"></a>Seuraavat vaiheet
Jos et edelleenkään saa yhteyttä Etätyöpöydän avulla, on [RDP vianmäärityksen opas](virtual-machines-windows-troubleshoot-rdp-connection.md). [Yksityiskohtainen RDP vianmäärityksen opas](virtual-machines-windows-detailed-troubleshoot-rdp.md) tarkastelee menetelmiä toimintaohjeet sijaan vianmäärityksen. Voit myös [avata Azure tukipyyntö](https://azure.microsoft.com/support/options/) käytännön järjestelmänvalvojalta.
<properties
    pageTitle="Windows Azure lataaminen Näennäiskiintolevyn valmisteleminen | Microsoft Azure"
    description="Windows-Näennäiskiintolevyn valmisteleminen ennen lataamista Azure suositellut käytännöt"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="genlin"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/18/2016"
    ms.author="glimoli;genli"/>

# <a name="prepare-a-windows-vhd-to-upload-to-azure"></a>Windows Azure lataaminen Näennäiskiintolevyn valmisteleminen
Voit ladata Windows AM paikallisen from Azure valmisteleminen oikein virtual kovalevy (Näennäiskiintolevyn). On useita suositeltua vaihetta, voit tehdä, ennen kuin olet ladannut Näennäiskiintolevyn Azure. Tässä artikkelissa kerrotaan, kuinka lataaminen Microsoft Azure Windows Näennäiskiintolevyn ja se myös kerrotaan, [miten voit käyttää](#step23).

## <a name="prepare-the-virtual-disk"></a>Valmistele virtual levy

>[AZURE.NOTE] 
> Azure tukee vain [luonti 1 näennäiskoneiden](http://blogs.technet.com/b/ausoemteam/archive/2015/04/21/deciding-when-to-use-generation-1-or-generation-2-virtual-machines-with-hyper-v.aspx) , jotka ovat Näennäiskiintolevyn-tiedostomuodossa. Azure VHDX uudempaan tiedostomuotoon ei tueta. 
>
> Näennäiskiintolevyn on oltava kiinteän kokoinen, ei ole dynaaminen. Tarvittaessa alla olevia ohjeita Tietotyylit kuvatun VHDX tai dynaaminen levylle. Näennäiskiintolevyn enimmäiskoko on 1,023 Gigatavua.


Varmista, että Windows-Näennäiskiintolevyn toimii oikein, paikallisessa palvelimessa. Korjaa virheitä AM itse ennen kuin yrität muuntaa tai Lataa Azure kuluessa.

Jos haluat muuntaa virtual levytilaa tarvittavat muotoon Azure, käytä seuraavissa osissa on kerrottu tavoilla. Varmuuskopioi AM ennen virtual levyn muuntaminen prosessi-tai Sysprep.

### <a name="convert-using-hyper-v-manager"></a>Muuntaa Hyper-V hallinnan avulla
- Avaa Hyper-V hallinta ja valitse vasemman reunan paikalliseen tietokoneeseen. Ylemmäs valikko, valitse **toiminto** > **Muokkaa levy**.
    - **Etsi Virtual kiintolevyn** näytössä Etsi ja valitse virtual levytilaa.
    - Valitse seuraavassa näytössä **muuntaminen**
        - Jos haluat muuntaa VHDX **Näennäiskiintolevyn** ja valitse **Seuraava**
        - Jos haluat muuntaa dynaamisen levyn **kiinteän kokoinen** ja valitse **Seuraava**

    - Selaa ja valitse **Uusi Näennäiskiintolevytiedosto polku**.
    - Valitse **Valmis** , sulje.

### <a name="convert-using-powershell"></a>Muuntaa PowerShellin avulla
Voit muuntaa virtual DVD-levyllä [Muunna Näennäiskiintolevyn PowerShell cmdlet-komennon](http://technet.microsoft.com/library/hh848454.aspx)avulla. Seuraavassa esimerkissä on ovat VHDX muuntaminen Näennäiskiintolevyn ja dynaamisen muuntaminen kiinteä tyyppi:

```powershell
Convert-VHD –Path c:\test\MY-VM.vhdx –DestinationPath c:\test\MY-NEW-VM.vhd -VHDType Fixed
```

### <a name="convert-from-vmware-vmdk-disk-format"></a>Muuntaa VMware VMDK levyn muodossa
Jos sinulla on Windows AM kuva [VMDK tiedostomuodossa](https://en.wikipedia.org/wiki/VMDK), muuntaa sen Näennäiskiintolevyn [Microsoft Virtual Machine muuntimen](https://www.microsoft.com/download/details.aspx?id=42497)avulla. Lue lisätietoja blogia [VMware VMDK, Hyper-V Näennäiskiintolevyn voit muuntaa](http://blogs.msdn.com/b/timomta/archive/2015/06/11/how-to-convert-a-vmware-vmdk-to-hyper-v-vhd.aspx) .

## <a name="prepare-windows-configuration-for-upload"></a>Valmistele lataaminen Windowsiin

> [AZURE.NOTE] Suorita seuraavat komennot [järjestelmänvalvojan](https://technet.microsoft.com/library/cc947813.aspx)oikeuksilla.

1. Poista staattinen pysyvän reitin reititys-taulukossa seuraavasti:

    - Voit tarkastella reitti-taulukossa, suorittamalla `route print`.
    - Tarkista **Pysyvyyttä tiet** osat. Jos näkyvissä on pysyvän reitin, avulla voit poistaa sen [reitin poistaminen](https://technet.microsoft.com/library/cc739598.apx) .

2. Poista WinHTTP-välityspalvelimen:

    ```
    netsh winhttp reset proxy
    ```

3. Määrittää levyn SAN käytännön [Onlineall](https://technet.microsoft.com/library/gg252636.aspx):

    ```
    diskpart san policy=onlineall
    ```

4. Käytä Windows koordinoidun yleisajan (UTC-ajan) aika ja määritä käynnistystapa Windows aika (w32time)-palvelun **automaattisesti**:

    ```
    REG ADD HKLM\SYSTEM\CurrentControlSet\Control\TimeZoneInformation /v RealTimeIsUniversal /t REG_DWORD /d 1
    sc config w32time start= auto
    ```


## <a name="configure-windows-services"></a>Windows-palvelujen määrittäminen
5. Varmista, että seuraavat Windows-palvelut on määritetty **Windows oletusarvot**. Ne on määritetty seuraavassa luettelossa on kerrottu käynnistysasetukset. Voit suorittaa nämä komennot käynnistysasetukset uudelleen:

    ```
    sc config bfe start= auto

    sc config dcomlaunch start= auto

    sc config dhcp start= auto

    sc config dnscache start= auto

    sc config IKEEXT start= auto

    sc config iphlpsvc start= auto

    sc config PolicyAgent start= demand

    sc config LSM start= auto

    sc config netlogon start= demand

    sc config netman start= demand

    sc config NcaSvc start= demand

    sc config netprofm start= demand

    sc config NlaSvc start= auto

    sc config nsi start= auto

    sc config RpcSs start= auto

    sc config RpcEptMapper start= auto

    sc config termService start= demand

    sc config MpsSvc start= auto

    sc config WinHttpAutoProxySvc start= demand

    sc config LanmanWorkstation start= auto

    sc config RemoteRegistry start= auto
    ```


## <a name="configure-remote-desktop-configuration"></a>Etätyöpöytä kokoonpanon määrittäminen
6. Jos määritettynä on yhdistetty Remote Desktop Protocol (RDP) kuuntelua itse allekirjoitettua sertifikaatteja, poista ne:

    ```
    REG DELETE "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp\SSLCertificateSHA1Hash”
    ```

    Katso lisätietoja määrittämisestä RDP listener varmenteet [Listener varmenteen käyttömahdollisuudet Windows Server](https://blogs.technet.microsoft.com/askperf/2014/05/28/listener-certificate-configurations-in-windows-server-2012-2012-r2/)

7. Määritä RDP-palvelun [Alive](https://technet.microsoft.com/library/cc957549.aspx) arvot:

    ```
    REG ADD "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v KeepAliveEnable /t REG_DWORD  /d 1 /f

    REG ADD "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v KeepAliveInterval /t REG_DWORD  /d 1 /f

    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp" /v KeepAliveTimeout /t REG_DWORD /d 1 /f
    ```

8. Määritä todennustila RDP-palveluun:

    ```
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v UserAuthentication /t REG_DWORD  /d 1 /f

    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v SecurityLayer /t REG_DWORD  /d 1 /f

    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v fAllowSecProtocolNegotiation /t REG_DWORD  /d 1 /f
    ```

9. Ota käyttöön RDP-palvelun lisäämällä seuraavista aliavaimista rekisterin:

    ```
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD  /d 0 /f
    ```


## <a name="configure-windows-firewall-rules"></a>Määritä Windowsin palomuurin säännöt
10. Salli kolme palomuuri-profiileista (toimialue, yksityinen ja julkinen) – WinRM ja PowerShell Remote-palvelun käyttöön:

    ```
    Enable-PSRemoting -force
    ```

11. Varmista, että seuraavat vieras käyttöjärjestelmän palomuurisäännöt ovat paikallaan:

    - Saapuva

    ```
    netsh advfirewall firewall set rule dir=in name="File and Printer Sharing (Echo Request - ICMPv4-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (LLMNR-UDP-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (NB-Datagram-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (NB-Name-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (Pub-WSD-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (SSDP-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (UPnP-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (WSD EventsSecure-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Windows Remote Management (HTTP-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Windows Remote Management (HTTP-In)" new enable=yes
    ```

    - Saapuvan ja lähtevän

    ```
    netsh advfirewall firewall set rule group="Remote Desktop" new enable=yes

    netsh advfirewall firewall set rule group="Core Networking" new enable=yes
    ```

    - Lähtevän

    ```
    netsh advfirewall firewall set rule dir=out name="Network Discovery (LLMNR-UDP-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (NB-Datagram-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (NB-Name-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (Pub-WSD-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (SSDP-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (UPnPHost-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (UPnP-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (WSD Events-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (WSD EventsSecure-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (WSD-Out)" new enable=yes
    ```


## <a name="additional-windows-configuration-steps"></a>Lisätietoja Windows-määritysvaiheet
12. Suorita `winmgmt /verifyrepository` vahvistamaan, että Windows Management Instrumentation (WMI)-tietovarasto vastaa. Jos säilö on vioittunut, katso lisätietoja [rajaamisesta on blogikirjoituksessa](https://blogs.technet.microsoft.com/askperf/2014/08/08/wmi-repository-corruption-or-not).

13. Varmista, että käynnistyksessä käytettävien tietojen (BCD)-asetuksia seuraavasti:

    ```
    bcdedit /set {bootmgr} integrityservices enable

    bcdedit /set {default} device partition=C:

    bcdedit /set {default} integrityservices enable

    bcdedit /set {default} recoveryenabled Off

    bcdedit /set {default} osdevice partition=C:

    bcdedit /set {default} bootstatuspolicy IgnoreAllFailures
    ```

14. Poista ylimääräiset Transport Driver Interface suodattimet, kuten ohjelmisto, joka analysoi TCP-paketit.
15. Jos haluat varmistaa, että levy on kunnossa ja yhdenmukaisia, suorita `CHKDSK /f` komento.
16. Poista kolmannen osapuolen ja ohjaimen liittyvät fyysinen osia tai virtualisointi-tekniikkaa.
17. Varmista, että kolmannen osapuolen sovelluksen ei käytetä portti 3389. Tämä portti käytetään Azure RDP-palvelun. Voit käyttää `netstat -anob` Tarkista portit, jotka käyttävät sovellusten hallinta-komennolla.
18. Jos Windows Näennäiskiintolevyn, jotka haluat ladata on toimialueen ohjauskoneen noudattamalla [näitä lisävaiheita](https://support.microsoft.com/kb/2904015) valmisteleminen levy.
19. Käynnistä AM, varmista, että Windows on edelleen kunnossa tavoittaa RDP-yhteyden avulla.
20. Nykyisen paikallisen järjestelmänvalvojan salasanan ja varmista, että voit käyttää tähän tiliin kirjautuminen Windows RDP-yhteyden kautta.  Tämä käyttöoikeus ohjataan "Salli kirjautuminen kautta Etätyöpöytäpalvelut"-ryhmäkäytäntöobjekti. Tämä objekti on asiakaslomakkeet-kohdassa "Tietokoneen Tietokoneasetukset\Windows Settings\Security \Suojausasetukset\Paikalliset."


## <a name="install-windows-updates"></a>Windows-päivitysten asentaminen
22. Asenna uusimmat päivitykset Windows. Jos tämä ei ole mahdollista, varmista, että seuraavat päivitykset on asennettu:

    - [KB3137061](https://support.microsoft.com/kb/3137061) Microsoft Azure VMs ei palauta verkon käyttökatkosta ja vioittumisen ongelmia

    - [KB3115224](https://support.microsoft.com/kb/3115224) Luotettavuuden VMs, joissa on käytössä Windows Server 2012 R2 tai Windows Server 2012 isännän parannukset

    - [KB3140410](https://support.microsoft.com/kb/3140410) MS16-031: Suojauspäivitys Microsoft Windowsin osoite laajentamisen: 8 maaliskuu-2016

    - [KB3063075](https://support.microsoft.com/kb/3063075) Monta tunnus 129 tapahtumat kirjataan, kun suoritat Windows Server 2012 R2-virtual machine Microsoft Azure

    - [KB3137061](https://support.microsoft.com/kb/3137061) Microsoft Azure VMs ei palauta verkon käyttökatkosta ja vioittumisen ongelmia

    - [KB3114025](https://support.microsoft.com/kb/3114025) Kun käytät Azure tiedostojen tallennustilan Windows 8.1 tai Server 2012 R2: n hidas suorituskyky

    - [KB3033930](https://support.microsoft.com/kb/3033930) Korjaustiedoston kasvaa RIO puskuri prosessia Windows Azure-palvelun kohti 64K rajoitus

    - [KB3004545](https://support.microsoft.com/kb/3004545) Et voi käyttää näennäiskoneiden, joita isännöidään Azure isännöintipalveluina Windowsin VPN-yhteyden kautta

    - [KB3082343](https://support.microsoft.com/kb/3082343) Paikallisen-VPN-yhteys menetetään, kun Azure sivusto sivusto VPN tunneleita Windows Server 2012 R2 RRAS

    - [KB3140410](https://support.microsoft.com/kb/3140410) MS16-031: Suojauspäivitys Microsoft Windowsin osoite laajentamisen: 8 maaliskuu-2016

    - [KB3146723](https://support.microsoft.com/kb/3146723) MS16-048: Kuvaus CSRSS suojauspäivitys: 12 huhtikuu 2016
    - [KB2904100](https://support.microsoft.com/kb/2904100) Järjestelmän jumittuu levyn i/o Windowsin aikana<a id="step23"></a>
23. Jos haluat luoda kuvan ottaa käyttöön useita koneet siitä, sinun on generalize kuvan suorittamalla `sysprep` ennen Näennäiskiintolevyn lataaminen Azure. Sinun ei tarvitse suorittaa `sysprep` tietynlaista Näennäiskiintolevyn käyttämiseen. Lisätietoja generalized kuva luomisesta on seuraavissa artikkeleissa:

    - [Luoda AM kuva aiemmin Azure-AM käyttämällä resurssien hallinnan käyttöönottomalli](virtual-machines-windows-create-vm-generalized.md)
    - [Luoda aiemmin Azure-AM perinteinen käyttöönoton modeemin AM kuva](virtual-machines-windows-classic-capture-image.md)
    - [Palvelinrooleista Sysprep-tuki](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)



## <a name="suggested-extra-configurations"></a>Ehdotettu ylimääräisiä käyttömahdollisuudet

Seuraavat asetukset eivät vaikuta Näennäiskiintolevyn ladataan. On kuitenkin erittäin suositeltavaa, että sinulla on määritetty ne.

- Asenna [Azuren näennäiskoneiden agentti](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Kun olet asentanut agentti, voit ottaa AM tunnisteet. AM-laajennukset Toteuta useimpien tärkeitä toimintoja, jota haluat käyttää omaa VMs salasanojen, kuten määrittäminen RDP ja monia muita.

- Dump lokin voi olla hyötyä Windows kaatumisen vianmääritys. Ota käyttöön Dump log sivustokokoelman:

    ```
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 2 /f`

    REG ADD "HKLM\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps" /v DumpFolder /t REG_EXPAND_SZ /d "c:\CrashDumps" /f

    REG ADD "HKLM\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps" /v DumpCount /t REG_DWORD /d 10 /f

    REG ADD "HKLM\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps" /v DumpType /t REG_DWORD /d 2 /f

    sc config wer start= auto
    ```

- Kun AM luodaan Azure-määrittää järjestelmän määrittämiä koon sivutustiedoston asemassa D:

    ```
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management" /t REG_MULTI_SZ /v PagingFiles /d "D:\pagefile.sys 0 0" /f
    ```


## <a name="next-steps"></a>Seuraavat vaiheet

- [Lataa Windows AM kuva Azure Resurssienhallinta-versioiden](virtual-machines-windows-upload-image.md)

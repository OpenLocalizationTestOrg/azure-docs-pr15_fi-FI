<properties
   pageTitle="Azure AM varmuuskopiointi epäonnistuu: yhteyttä tilannevedoksen tila AM-agentti ei voitu muodostaa - tilannevedoksen AM sub tehtävän aikakatkaisu | Microsoft Azure"
   description="Ongelmia syitä ja Azure AM varmuuskopioinnin virheiden liittyvät ratkaisut ei voitu muodostaa tilannevedoksen tila AM-agentti kanssa. Virhe aikakatkaisu tilannevedoksen AM sub tehtävä"
   services="backup"
   documentationCenter=""
   authors="genlin"
   manager="cfreeman"
   editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jimpark; markgal;genli"/>

# <a name="azure-vm-backup-fails-could-not-communicate-with-the-vm-agent-for-snapshot-status---snapshot-vm-sub-task-timed-out"></a>Azure AM varmuuskopiointi epäonnistuu: yhteyttä tilannevedoksen tila AM-agentti ei voitu muodostaa - tilannevedoksen AM sub tehtävän aikakatkaisu

## <a name="summary"></a>Yhteenveto

Jälkeen rekisteröiminen ja ajoitus virtuaalikoneen (AM) Azure varmuuskopion, Azure varmuuskopion palvelun käynnistää varmuuskopiointityön määritettynä ajankohtana toimittamalla tulevat ajankohta tilannevedoksen AM varmuuskopio-tunnisteella. On ehdot, jotka voivat estää tilannevedoksen saatu, joka johtaa varmuuskopion epäonnistui. Tässä artikkelissa on vianmääritysohjeita varten Azure AM varmuuskopioinnin virheiden liittyvät tilannevedoksen aikakatkaisun virhe seurantakohteita.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="symptom"></a>Oire

Microsoft Azure varmuuskopiointi infrastruktuurin serviceksi (IaaS) AM epäonnistuu, ja palauttaa [Azure portal](https://portal.azure.com/)työn virhetiedot seuraavan virhesanoman:

**Yhteyttä tilannevedoksen tila AM-agentti ei voitu muodostaa - tilannevedoksen AM sub tehtävän aikakatkaisu.**

## <a name="cause"></a>Syy
Neljä yleistä syytä tämän virheen:

- AM ei ole Internet-yhteyttä.
- AM asennettu Microsoft Azure AM-agentti ei ole ajan tasalla (for Linux VMs).
- Varmuuskopion tunniste ei päivitä tai ladata.
- Tilannevedoksia tila ei voi hakea tai tilannevedosten ei voi ottaa.

## <a name="cause-1-the-vm-does-not-have-internet-access"></a>Syy 1: AM ei ole Internet-yhteys
Käyttöönotto-vaatimus kohti AM ei ole Internet-käyttöoikeutta tai rajoituksista paikassa, joka estää Azure infrastruktuuri on.

Varmuuskopion tunniste vaativat Azure julkiseen IP-osoitteet oikein. Tämä johtuu siitä, että se lähettää komentoja Azuren tallennustilaan päätepisteen (HTTP URL-osoite) AM tilannevedosten hallintaan. Jos laajennus ei ole julkinen Internet-yhteys, varmuuskopioinnin epäonnistuu myöhemmin.

### <a name="solution"></a>Ratkaisu
Tässä skenaariossa avulla voit ratkaista ongelman jollakin seuraavista tavoista:

- Whitelist Azure palvelinkeskuksen IP-alueita
- Luo HTTP-liikenne paikalliseen flow liikerata

### <a name="to-whitelist-the-azure-datacenter-ip-ranges"></a>Voit whitelist Azure-palvelinkeskuksen IP-alueita

1. Hanki [luettelon Azure-palvelinkeskuksen IP-osoitteet](https://www.microsoft.com/download/details.aspx?id=41653) whitelisted.
2. Salli IP-osoitteet uusi NetRoute cmdlet-komennolla. Tee tämä cmdlet-komento Azure AM laajennettuja PowerShell-ikkunassa (Suorita järjestelmänvalvojana).
3. Lisää sääntöjä verkon suojauksen ryhmän (NSG), jos käytössäsi on jokin annettavien IP-osoitteet.

### <a name="to-create-a-path-for-http-traffic-to-flow"></a>Luoda HTTP-liikenne paikalliseen flow polku

1. Jos sinulla on verkon rajoitukset sijainti (esimerkiksi NSG), ota käyttöön HTTP-välityspalvelin ja haluat reitittää liikenteen.
2. Jos sinulla on verkon käyttöoikeusryhmän (NSG), lisätä sääntöjä Salli käyttää Internet-HTTP-välityspalvelin.

Lue, miten voit [määrittää HTTP-välityspalvelin AM varmuuskopioiden hakeminen](backup-azure-vms-prepare.md#using-an-http-proxy-for-vm-backups).

## <a name="cause-2-the-microsoft-azure-vm-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>Syy 2: AM asennettuna Microsoft Azure AM-agentti ei ole ajan tasalla (for Linux VMs)

### <a name="solution"></a>Ratkaisu
Ongelmat, jotka vaikuttavat vanha AM agentti virheet eniten agent- tai laajennus liittyvä virheet Linux VMs varten. Yleiset ohjeena vianmääritys ongelman ensimmäiset vaiheet ovat seuraavat:

1. [Asenna uusimmat Azure AM-agentti](https://github.com/Azure/WALinuxAgent).
2. Varmista, että Azure agent on käynnissä AM. Voit tehdä tämän suorittamalla seuraavan komennon:```ps -e```

    Jos tämä toimenpide ei ole käynnissä, käynnistä se uudelleen seuraavista komennoista avulla.

    Saat Ubuntu:```service walinuxagent start```

    Muut jaot varten: "" palvelun waagent alku
```

3. [Configure the auto restart agent](https://github.com/Azure/WALinuxAgent/wiki/Known-Issues#mitigate_agent_crash).

4. Run a new test backup. If the failures persist, please collect logs from the following folders for further debugging.

    We require the following logs from the customer’s VM:

    - /var/lib/waagent/*.xml
    - /var/log/waagent.log
    - /var/log/azure/*

If we require verbose logging for waagent, follow these steps to enable this:

1. In the /etc/waagent.conf file, locate the following line:

    Enable verbose logging (y|n)

2. Change the **Logs.Verbose** value from n to y.
3. Save the change, and then restart waagent by following the previous steps in this section.

## Cause 3: The backup extension fails to update or load
If extensions cannot be loaded, then Backup fails because a snapshot cannot be taken.

### Solution
For Windows guests:

1. Verify that the iaasvmprovider service is enabled and has a startup type of automatic.
2. If this is not the configuration, enable the service to determine whether the next backup succeeds.

For Linux guests:

The latest version of VMSnapshot Linux (extension used by backup) is 1.0.91.0

If the backup extension still fails to update or load, you can force the VMSnapshot extension to be reloaded by uninstalling the extension. The next backup attempt will reload the extension.

### To uninstall the extension

1. Go to the [Azure portal](https://portal.azure.com/).
2. Locate the particular VM that has backup problems.
3. Click **Settings**.
4. Click **Extensions**.
5. Click **Vmsnapshot Extension**.
6. Click **uninstall**.

This will cause the extension to be reinstalled during the next backup.

## Cause 4: The snapshots status cannot be retrieved or the snapshots cannot be taken
VM backup relies on issuing snapshot command to underlying storage. The backup can fail because it has no access to storage or because of a delay in snapshot task execution.

### Solution
The following conditions can cause snapshot task failure:

| Cause | Solution |
| ----- | ----- |
| VMs that have Microsoft SQL Server Backup configured. By default, VM Backup runs a VSS Full backup on Windows VMs. On VMs that are running SQL Server-based servers and on which SQL Server Backup is configured, snapshot execution delays may occur. | Set following registry key if you are experiencing backup failures because of snapshot issues.<br><br>[HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT] "USEVSSCOPYBACKUP"="TRUE" |
| VM status is reported incorrectly because the VM is shut down in RDP. If you shut down the virtual machine in RDP, check the portal to determine whether that VM status is reflected correctly. | If it’s not, shut down the VM in the portal by using the ”Shutdown” option in the VM dashboard. |
| Many VMs from the same cloud service are configured to back up at the same time. | It’s a best practice to spread out the VMs from the same cloud service to have different backup schedules. |
| The VM is running at high CPU or memory usage. | If the VM is running at high CPU usage (more than 90 percent) or high memory usage, the snapshot task is queued and delayed and eventually times out. In this situation, try on-demand backup. |
|The VM cannot get host/fabric address from DHCP.|DHCP must be enabled inside the guest for IaaS VM Backup to work.  If the VM cannot get host/fabric address from DHCP response 245, then it cannot download ir run any extensions. If you need a static private IP, you should configure it through the platform. The DHCP option inside the VM should be left enabled. View more information about [Setting a Static Internal Private IP](../virtual-network/virtual-networks-reserved-private-ip.md).|

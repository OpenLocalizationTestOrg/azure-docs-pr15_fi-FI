<properties
    pageTitle="Vianmääritys SSH yhteys, AM | Microsoft Azure"
    description="Miten käynnissä Linux Azure-AM, kuten "SSH epäonnistui" tai 'SSH yhteyden hylätty' vianmääritys."
    keywords="ssh yhteyden hylätty, ssh virhe, azure ssh, SSH yhteys epäonnistui"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="top-support-issue,azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="iainfou"/>

# <a name="troubleshoot-ssh-connections-to-an-azure-linux-vm-that-fails-errors-out-or-is-refused"></a>Azure Linux AM, joka ei onnistu, virheitä, tai hylätään SSH yhteyden vianmääritys
On useita syitä, että käytössä ilmenee virheitä suojattu runko (SSH) SSH epäonnistuneita tai SSH hylätään, kun yrität muodostaa yhteyden Linux virtual machine (AM). Tämän artikkelin avulla voit etsiä ja korjata ongelmaa. Voit käyttää vianmääritystä ja ratkaisemista yhteysongelmien Azure portal Azure CLI tai Linux AM Access-tunniste.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Jos tarvitset apua milloin tahansa tämän artikkelin, ota Azure asiantuntijoille [MSDN Azure](http://azure.microsoft.com/support/forums/)ja Pinon ylivuoto keskustelupalstoilla. Vaihtoehtoisesti voit jättää Azure tuki tapahtuma. Siirry [Azure tukevat sivusto](http://azure.microsoft.com/support/options/) ja valitse **tuesta**. Lue tietoja käyttämällä Azure tukevat [Microsoft Azure tukevat usein kysytyt kysymykset](http://azure.microsoft.com/support/faq/).


## <a name="quick-troubleshooting-steps"></a>Vianmäärityksen pikatoiminnot
Vianmäärityksen jokaisen vaiheen jälkeen Yritä yhdistää uudelleen AM.

1. Palauttaa SSH määritykset.
2. Palauta käyttäjän tunnistetiedot.
3. Varmista [Verkon käyttöoikeusryhmän](../virtual-network/virtual-networks-nsg.md) sääntöjä SSH tietoliikenteen salliminen.
    - Varmista, että verkko-käyttöoikeusryhmän säännön olemassa sallimaan SSH tietoliikenne (oletusarvoisesti porttinumeroa 22).
    - Et voi käyttää uudelleenohjaus / käyttämättä Azure kuormituksen yhdistämismääritys.
4. [AM resurssin kuntotietojen](../resource-health/resource-health-overview.md)tarkistus. 
    - Varmista, että AM ilmoittaa siten, että kunnossa.
    - Jos sinulla on käynnistyksen diagnostiikka käytössä, tarkista AM ei raportoi lokit käynnistyksen virheet.
5. Käynnistä AM.
6. Ota uudelleen käyttöön AM.

Jatka lukemista lisää vianmääritysohjeita ja selitykset.


## <a name="available-methods-to-troubleshoot-ssh-connection-issues"></a>Käytettävissä olevat menetelmät SSH yhteysongelmien vianmääritys

Voit palauttaa tunnistetiedot tai SSH määritysten jollakin seuraavista tavoista:

- [Azure portal](#using-the-azure-portal) - hyvien, jos haluat palauttaa nopeasti SSH määritykset tai SSH avain ja olet ei ole asennettu Azure-työkalut.
- [Azure CLI komennot](#using-the-azure-cli) – Jos olet jo komentorivillä, nopeasti palauttaa SSH määritys ja tunnistetiedot.
- [Azure VMAccessForLinux tunniste](#using-the-vmaccess-extension) - luominen ja käyttäminen uudelleen json määritystiedostot vaihtamaan SSH määritysten tai käyttäjän tunnistetiedot.

Vianmäärityksen jokaisen vaiheen jälkeen yritä muodostaa yhteyttä AM uudelleen. Jos yhteyden muodostaminen ei vieläkään onnistu, kokeile seuraavaan vaiheeseen.


## <a name="using-the-azure-portal"></a>Azure-portaalissa
Azure-portaalissa on nopea tapa Palauta SSH määritysten tai käyttäjän tunnistetiedot asentamatta mitä tahansa työkaluja paikalliseen tietokoneeseen.

Valitse oman AM Azure-portaalissa. Siirry **tuki + vianmääritys** -osioon ja valitse **salasanan** seuraavan esimerkin mukaisesti:

![Palauttaa SSH määritysten tai tunnistetiedot Azure-portaalissa](./media/virtual-machines-linux-troubleshoot-ssh-connection/reset-credentials-using-portal.png)

### <a name="reset-the-ssh-configuration"></a>Palauttaa SSH määritykset
Valitse ensimmäisessä vaiheessa `Reset SSH configuration only` **tilassa** avattavasta valikosta kuin edellisessä näyttökuvan, valitse **Palauta** . Kun tämä toiminto on valmis, yritä käyttää omaa AM uudelleen.

### <a name="reset-ssh-credentials-for-a-user"></a>Palauta käyttäjän tunnistetiedot SSH
Palauttaa nykyisen käyttäjän tunnistetietojen, valitse joko `Reset SSH public key` tai `Reset password` **tilan** avattavasta valikosta kuin edellisessä näyttökuvan. Määritä käyttäjänimi ja SSH-näppäintä tai uusi salasana ja valitse sitten **Palauta** .

Voit myös luoda käyttäjän tästä valikosta AM sudo oikeuksilla. Kirjoita uuden käyttäjänimeä ja liittyvän salasanan tai SSH-näppäintä ja valitse sitten **Palauta** .


## <a name="using-the-azure-cli"></a>Azure CLI käyttäminen
Jos et ole jo, [Asenna Azure-CLI ja liitä Azure-tilaukseen](../xplat-cli-install.md). Varmista, että käytät resurssien hallinnan tila seuraavasti:

```
azure config mode arm
```

Jos olet luonut ja ladata mukautetun Linux levyn näköistiedoston, varmista, että [Microsoft Azure Linux Agent](virtual-machines-linux-agent-user-guide.md) versio 2.0.5 tai uudempi on asennettu. Saat VMs luotu valikoiman kuvia access-tunniste on jo asentanut ja määrittänyt puolestasi.

### <a name="reset-ssh-configuration"></a>Palauta SSH määritys
Itse SSHD kokoonpano on määritetty virheellisesti tai palvelun havaitsi virheen. Voit palauttaa SSHD, varmista, että itse SSH-määritykset. SSHD palauttaminen olisi voit tehdä vianmäärityksen ensimmäinen vaihe.

Seuraava esimerkki palauttaa SSHD-AM, jonka nimi on `myVM` resurssiryhmän nimeltä `myResourceGroup`. Käytä omia AM ja resurssien ryhmänimiä seuraavasti:

```bash
azure vm reset-access --resource-group myResourceGroup --name myVM \
    --reset-ssh
```

### <a name="reset-ssh-credentials-for-a-user"></a>Palauta käyttäjän tunnistetiedot SSH
Jos SSHD näkyy oikein, voit palauttaa giver käyttäjän salasana. Seuraava esimerkki palauttaa tunnistetietojen `myUsername` määritetyn arvon `myPassword`, valitse nimeltä AM `myVM` - `myResourceGroup`. Käytä omia arvot seuraavasti:

```bash
azure vm reset-access --resource-group myResourceGroup --name myVM \
     --username myUsername --password myPassword
```

Jos SSH avaimen todennusta, voit palauttaa käyttäjän SSH-näppäintä. Seuraavassa esimerkissä päivitykset tallennetaan SSH avain `~/.ssh/azure_id_rsa.pub` käyttäjän nimi `myUsername`, valitse nimeltä AM `myVM` - `myResourceGroup`. Käytä omia arvot seuraavasti:

```bash
azure vm reset-access --resource-group myResourceGroup --name myVM \
    --username myUsername --ssh-key-file ~/.ssh/azure_id_rsa.pub
```


## <a name="using-the-vmaccess-extension"></a>Käyttämällä VMAccess-tunniste
Linux AM Access-laajennus lukee json-tiedosto, joka määrittää toiminnot suoritetaan. Nämä toiminnot ovat palauttamista SSHD, palauttamisesta SSH-näppäintä tai Lisää ensin käyttäjä. Voit edelleen käyttää Azure-CLI Soita VMAccess-tunniste, mutta voit käyttää json-tiedostojen välillä useita VMs tarvittaessa. Tämän menetelmän avulla voit luoda säilön json-tiedostoista, jotka voit sitten kutsua annettu skenaarioita.

### <a name="reset-sshd"></a>Palauta SSHD
Luo tiedosto nimeltä `PrivateConf.json` seuraavat sisältöä:

```bash
{  
    "reset_ssh":"True"
}
```

Käytä Azure-CLI, valitse soitat `VMAccessForLinux` tunniste SSHD yhteyden palauttaminen määrittämällä json-tiedosto. Seuraava esimerkki palauttaa SSHD-niminen AM `myVM` - `myResourceGroup`. Käytä omia arvot seuraavasti:

```bash
azure vm extension set myResourceGroup myVM \
    VMAccessForLinux Microsoft.OSTCExtensions "1.2" \
    --private-config-path PrivateConf.json
```

### <a name="reset-ssh-credentials-for-a-user"></a>Palauta käyttäjän tunnistetiedot SSH
Jos SSHD näkyy oikein, voit palauttaa giver käyttäjän tunnistetiedot. Jos haluat palauttaa käyttäjän salasanan, Luo tiedosto nimeltä `PrivateConf.json`. Seuraava esimerkki palauttaa tunnistetietojen `myUsername` määritetyn arvon `myPassword`. Kirjoita seuraavat rivit yhdeksi oman `PrivateConf.json` tiedoston, käyttämällä omaa arvoja:

```bash
{
    "username":"myUsername", "password":"myPassword"
}
```

Tai Palauta käyttäjän SSH-näppäintä, luo ensin tiedosto nimeltä `PrivateConf.json`. Seuraava esimerkki palauttaa tunnistetietojen `myUsername` määritetyn arvon `myPassword`, valitse nimeltä AM `myVM` - `myResourceGroup`. Kirjoita seuraavat rivit yhdeksi oman `PrivateConf.json` tiedoston, käyttämällä omaa arvoja:

```bash
{
    "username":"myUsername", "ssh_key":"mySSHKey"
}
```

Kun olet luonut json-tiedostossa, käytä Azure-CLI Soita `VMAccessForLinux` tunniste SSH käyttäjän tunnistetietojen nollaaminen määrittämällä json-tiedosto. Seuraava esimerkki palauttaa nimeltä AM tunnistetietoja `myVM` - `myResourceGroup`. Käytä omia arvot seuraavasti:

```
azure vm extension set myResourceGroup myVM \
    VMAccessForLinux Microsoft.OSTCExtensions "1.2" \
    --private-config-path PrivateConf.json
```


## <a name="restart-a-vm"></a>Käynnistä AM
Jos on Palauta SSH määritys ja käyttäjän tunnistetiedot tai havaitsi virheen, kun teet näin, voit kokeilla uudelleenkäynnistyksen AM osoitteeseen pohjana Laske ongelmat.

### <a name="azure-portal"></a>Azure portal
Voit käynnistää AM, Azure-portaalissa, valitse että AM ja valitsemalla ***Käynnistä** -painiketta, kuten seuraavassa esimerkissä:

![Käynnistä AM Azure-portaalissa](./media/virtual-machines-linux-troubleshoot-ssh-connection/restart-vm-using-portal.png)

### <a name="azure-cli"></a>Azure CLI
Seuraavassa esimerkissä käynnistyy nimeltä AM `myVM` resurssiryhmän nimeltä `myResourceGroup`. Käytä omia arvot seuraavasti:

```bash
azure vm restart --resource-group myResourceGroup --name myVM
```


## <a name="redeploy-a-vm"></a>Ota uudelleen AM
Voit käyttöön toisen solmun sisällä Azure, joka saattaa korjata kaikki pohjana ongelmien AM. Lisätietoja AM mallirakenteeseen artikkelissa [Ota uusi Azure solmu virtual tietokone uudelleen](virtual-machines-windows-redeploy-to-new-node.md).

> [AZURE.NOTE] Kun tämä toiminto on valmis, tilapäiset levyn tiedot menetetään ja dynaamisia IP-osoitteita, jotka liittyvät virtuaalikoneen päivitetään.

### <a name="azure-portal"></a>Azure portal
Jos haluat käyttöön AM, Azure-portaalissa, valitse AM ja Vieritä alaspäin **tuki + vianmääritys** -osaan. Valitse **Ota uudelleen** -painiketta, kuten seuraavassa esimerkissä:

![Ota uudelleen AM Azure-portaalissa](./media/virtual-machines-linux-troubleshoot-ssh-connection/redeploy-vm-using-portal.png)

### <a name="azure-cli"></a>Azure CLI
Seuraavassa esimerkissä redeploys nimeltä AM `myVM` resurssiryhmän nimeltä `myResourceGroup`. Käytä omia arvot seuraavasti:

```bash
azure vm redeploy --resource-group myResourceGroup --name myVM
```

## <a name="vms-created-by-using-the-classic-deployment-model"></a>VMs perinteinen käyttöönotto-mallin avulla luotu analyysinäkymä

Kokeile näitä ohjeita voit korjata yleisimmät SSH epäonnistuneita VMs, jotka on luotu käyttämällä perinteinen käyttöönotto-mallin varten. Jokaisen vaiheen jälkeen Yritä yhdistää uudelleen AM.

- Palauttaa [Azure portal](https://portal.azure.com)etäkäyttö. Azure-portaalissa oman AM Valitse ja valitse **Palauta Remote...** -painiketta.

- Käynnistä AM. Valitse [Azure portal](https://portal.azure.com)oman AM ja **Käynnistä** -painiketta.

    -TAI –

    Valitse [Azure perinteinen portal](https://manage.windowsazure.com) **näennäiskoneiden** > **esiintymät** > **uudelleen**.

- Ota uusi Azure solmun AM uudelleen. Lisätietoja ottamisesta käyttöön AM on artikkelissa [Ota uusi Azure solmu virtual tietokone uudelleen](virtual-machines-windows-redeploy-to-new-node.md).

    Kun tämä toiminto on valmis, tilapäiset levyn tiedot menetetään ja dynaamisia IP-osoitteita, jotka liittyvät virtuaalikoneen päivitetään.

- Noudata [nollaaminen salasanan tai Linux-pohjaiset näennäiskoneiden SSH](virtual-machines-linux-classic-reset-access.md) :
    - Vaihda salasana tai SSH-näppäintä.
    - _Sudo_ käyttäjätilin luominen.
    - Palauttaa SSH määritykset.

- Tarkista AM resurssin kunto minkä tahansa ympäristö ongelmien varalta.<br>
  Valitse AM ja vieritä **asetukset** > **Tarkista kunto**.


## <a name="additional-resources"></a>Lisäresursseja

- Jos et edelleenkään pysty SSH, että AM jälkeen kuvattujen toimenpiteiden jälkeen, katso [Lisätietoja vianmääritysohjeet](virtual-machines-linux-detailed-troubleshoot-ssh-connection.md) tarkistettava lisätoimia ratkaise ongelmaa.

- Saat lisätietoja sovellusten käyttö [vianmääritys access-sovellukseen Azure virtuaalikoneen käytössä](virtual-machines-linux-troubleshoot-app-connection.md)

- Lisätietoja vianmäärityksestä näennäiskoneiden, jotka on luotu käyttämällä perinteinen käyttöönotto-mallin Katso, [miten vaihtamaan salasanan tai Linux-pohjaiset näennäiskoneiden SSH](virtual-machines-linux-classic-reset-access.md).

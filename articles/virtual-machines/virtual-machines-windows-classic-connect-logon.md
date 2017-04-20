<properties
    pageTitle="Perinteinen Azure-VM Kirjaudu | Microsoftin Azure"
    description="Kirjaudu näennäiskoneessa Windows perinteinen käyttöönottomalli luotu Azure classic-portaalin avulla."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="cynthn"/>


# <a name="log-on-to-a-windows-virtual-machine-using-the-azure-classic-portal"></a>Kirjaudu sisään näennäiskoneessa Windows Azure classic-portaalin avulla.

Azure klassinen portal käyttää **Yhdistä** -painiketta aloittaa Remote Desktop-istunnon ja kirjautua Windows VM.

Haluatko muodostaa yhteyden Linux VM? Katso [käynnissä Linux näennäiskoneeseen kirjautuminen](virtual-machines-linux-mac-create-ssh-keys.md).

Katso, miten [seuraavasti, Azure uuden portaalin avulla](virtual-machines-windows-connect-logon.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] 

## <a name="video-walkthrough"></a>Video vaihe vaiheelta

Tässä on video PE ohjeita Tässä opetusohjelmassa. Se kattaa myös päätepisteet ja julkisten ja yksityisten portit, joita käytetään yhteyden muodostamiseksi Windows-VM-Azure.

[AZURE.VIDEO logging-on-to-vm-running-windows-server-on-azure]


## <a name="connect-to-the-virtual-machine"></a>Muodosta yhteys virtual machine

1. Kirjaudu sisään Azure classic-portaaliin.

2. Valitse **virtuaalikoneet**ja valitse sitten virtual machine.

3. Komentopalkissa sivun alaosassa Valitse **Yhdistä**.

    ![Kirjaudu sisään virtual machine](./media/virtual-machines-windows-classic-connect-logon/connectwindows.png)
    
> [AZURE.TIP] Jos **Yhdistä** -painike ei ole käytettävissä, katso tämän artikkelin lopussa vianmääritysvihjeitä.

## <a name="log-on-to-the-virtual-machine"></a>Kirjaudu sisään virtual machine

[AZURE.INCLUDE [virtual-machines-log-on-win-server](../../includes/virtual-machines-log-on-win-server.md)]

## <a name="next-steps"></a>Seuraavat vaiheet

-   Jos **Yhdistä** -painike ei ole käytössä tai sinulla on muita ongelmia etätyöpöytäyhteyden kanssa, koeta palauttaa kokoonpanon. Näennäiskoneen Dashboardista **Nopeasti Glance**, valitsemalla **Palauta etäyhteyden kokoonpano**.
-   Ongelmia salasanan kanssa Kokeile sen palauttamista. Näennäiskoneen Dashboardista **Glance nopeasti**, valitse **Vaihda salasana**.

Jos nämä vihjeet eivät toimi tai ei ole mitä tarvitset, katso [vianmääritys etätyöpöytäyhteydet ja Windows-pohjaisten Azure näennäiskoneessa](virtual-machines-windows-troubleshoot-rdp-connection.md). Tässä artikkelissa käydään läpi määrittää ja ratkaista yhteisiä ongelmia.



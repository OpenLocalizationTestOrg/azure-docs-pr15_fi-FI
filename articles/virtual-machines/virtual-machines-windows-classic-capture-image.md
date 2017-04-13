<properties
    pageTitle="Kuvan tallentaminen Azure-Windowsin AM | Microsoft Azure"
    description="Kuvan tallentaminen Azure Windows-virtuaalikoneen, luotu perinteinen käyttöönottomalli."
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
    ms.date="09/27/2016"
    ms.author="cynthn"/>

#<a name="capture-an-image-of-an-azure-windows-virtual-machine-created-with-the-classic-deployment-model"></a>Kuvan tallentaminen Azure Windows-virtuaalikoneen, luotu perinteinen käyttöönottomalli.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Resurssienhallinta malli on artikkelissa [Tee kopio Windows AM käynnissä Azure-tietokannassa](virtual-machines-windows-vhd-copy.md).


Tässä artikkelissa kerrotaan sieppaaminen Azure virtual-tietokoneessa, jossa Windows, jotta voit käyttää sitä kuvana muiden näennäiskoneiden luomiseen. Tämä kuva on käyttöjärjestelmän levy- ja minkä tahansa tietojen levyjä, jotka on liitetty virtuaalikoneen. Se ei ole verkko määritykset, joten sinun on määrittäminen käyttäjille, jotka käyttävät kuvan näennäiskoneiden luodessasi.

Azure tallentaa kuvan **Omat kuvat**-kohdassa. Tämä on samassa paikassa minkä tahansa lisättyjen kuvien tallennuspaikka. Lisätietoja kuvien on artikkelissa [tietoja näennäiskoneiden kuvat](virtual-machines-linux-classic-about-images.md).

##<a name="before-you-begin"></a>Ennen aloittamista##

Näissä vaiheissa oletetaan, että olet jo luonut Azure virtuaalikoneen ja määrittänyt käyttöjärjestelmä, mukaan lukien kaikki tiedot levyjen liittäminen. Jos et vielä ole vielä, Katso seuraavat ohjeet:

- [Voit luoda virtual machine kuva](virtual-machines-windows-classic-createportal.md)
- [Tietoja DVD-levyllä liittämisestä virtual machine](virtual-machines-windows-classic-attach-disk.md)
- Varmista, että palvelinrooleista tuetaan Sysprep. Lisätietoja on artikkelissa [Palvelinrooleista Sysprep-tuki](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).

> [AZURE.WARNING] Tämä toimenpide poistaa alkuperäisen virtuaalikoneen sen jälkeen, kun se kirjataan. 

Ennen caputuring Azure virtuaalikoneen kuva on suositeltavaa varmuuskopioida kohde virtuaalikoneen. Azure-virtuaalikoneissa voit varmuuskopioida Azure varmuuskopion käytöstä. Lisätietoja on artikkelissa [Azure-virtuaalikoneissa varmuuskopioida](../backup/backup-azure-vms.md). Muita ratkaisuja on saatavissa sertifioituja kumppaneita. Selvitä, mikä on tällä hetkellä käytettävissä, Etsi Azure Marketplacesta.


##<a name="capture-the-virtual-machine"></a>Sieppaa virtuaalikoneen

1. Valitse [Azure perinteinen portal](http://manage.windowsazure.com)-virtuaalikoneen **Yhdistä** . Ohjeita on artikkelissa [miten kirjaudutaan sisään käytössä Windows Server virtual koneeseen] [].

2.  Avaa komentokehoteikkuna järjestelmänvalvojana.

3.  Vaihda kansio `%windir%\system32\sysprep`, ja suorita sitten sysprep.exe.

4.  **Järjestelmän** -valintaikkuna. Toimi seuraavasti:

    - **Järjestelmän puhdistustoiminto** **Kirjoita ruutuun esittelyyn (Tervetuloa-ohjelma)** ja varmista, että **Yleistä** on valittuna. Lisätietoja Sysprepin käyttämisestä on artikkelissa [Voit käyttää Sysprep: Johdanto][].

    - Valitse **Sammuta** **Sammuta**.

    - Valitse **OK**.

    ![Suorita Sysprep](./media/virtual-machines-windows-classic-capture-image/SysprepGeneral.png)

7.  Sysprep sulkee virtuaalikoneen, joka **pysäyttää**virtuaalikoneen Azure perinteinen portaalissa tila muuttuu.

8.  Azure perinteinen-portaalissa Valitse **näennäiskoneiden** virtuaalikoneen haluat ottaa näyttöleikkeen.

9.  Valitse komentopalkista **siepata**.

    ![Sieppaa virtuaalikoneen](./media/virtual-machines-windows-classic-capture-image/CaptureVM.png)

    **Sieppaa virtuaalikoneen** -valintaikkuna tulee näkyviin.

10. Kirjoita **Kuvan nimi**, uuden kuvan nimi.

11. Ennen Windows Server-kuva on lisätty mukautettuja kuvien joukon, se on generalized suorittamalla Sysprep, kuten edellä kuvatut vaiheet. Valitse ilmaisemaan, jonka teit, joka **on suoritetaan Sysprep virtuaalikoneen käyttöön** .

12. Napsauta kannattaa tallentaa kuvan valintamerkkiä. Uusi kuva on nyt **kuvia**.

    ![Kuva sieppaus onnistui](./media/virtual-machines-windows-classic-capture-image/VMCapturedImageAvailable.png)

##<a name="next-steps"></a>Seuraavat vaiheet

Kuva on valmis käytettäväksi näennäiskoneiden luomiseen. Voit tehdä tämän Luo virtual machine käyttäminen **Valikoima-** valikkovaihtoehto ja valitsemalla juuri luomasi kuva. Lisätietoja on kohdassa [Create virtual koneen kuvan](virtual-machines-windows-classic-createportal.md).



[Miten kirjaudutaan sisään käytössä Windows Server virtual tietokoneeseen]: virtual-machines-windows-classic-connect-logon.md
[Voit käyttää: esittely]: http://technet.microsoft.com/library/bb457073.aspx
[Run Sysprep.exe]: ./media/virtual-machines-capture-image-windows-server/SysprepCommand.png
[Enter Sysprep.exe options]: ./media/virtual-machines-windows-classic-capture-image/SysprepGeneral.png
[The virtual machine is stopped]: ./media/virtual-machines-capture-image-windows-server/SysprepStopped.png
[Capture an image of the virtual machine]: ./media/virtual-machines-windows-classic-capture-image/CaptureVM.png
[Enter the image name]: ./media/virtual-machines-capture-image-windows-server/Capture.png
[Image capture successful]: ./media/virtual-machines-capture-image-windows-server/CaptureSuccess.png
[Use the captured image]: ./media/virtual-machines-capture-image-windows-server/MyImagesWindows.png

<properties
    pageTitle="Liitä DVD-levyllä AM | Microsoft Azure"
    description="Tietoja DVD-levyllä liittäminen Windows virtual machine-perinteinen käyttöönotto-mallin avulla luotu ja sen valmistelu."
    services="virtual-machines-windows, storage"
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
    ms.date="06/27/2016"
    ms.author="cynthn"/>

# <a name="attach-a-data-disk-to-a-windows-virtual-machine-created-with-the-classic-deployment-model"></a>Liitä tiedot DVD-levyllä Windows virtual machine-perinteinen käyttöönotto-mallin avulla luotu

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Jos haluat käyttää uuden portaalin, katso, [miten voit liittää tiedot levyn Windows-AM Azure-portaalissa](virtual-machines-windows-attach-disk-portal.md).

Jos tarvitset lisätiedot-levyn, voit liittää tyhjä levy tai olemassa olevaa levyä tiedoilla AM. Kummassakin tapauksessa levyjen ovat .vhd-tiedostoja, jotka sijaitsevat Azure-tallennustilan tilin. Kyseessä uusi DVD-levyllä, kun liität levyn sinun myös on alustaa, jotta se on valmis käytettäväksi Windows-AM.

Saat lisätietoja levyjen [tietoja levyjen ja näennäiskiintolevyjen näennäiskoneiden](virtual-machines-windows-about-disks-vhds.md).


[AZURE.INCLUDE [howto-attach-disk-windows-linux](../../includes/howto-attach-disk-windows-linux.md)]

## <a name="initialize-the-disk"></a>Alusta levy

1. Muodosta yhteys virtuaalikoneen. Katso ohjeet [käytössä Windows Server virtual tietokoneeseen kirjautuminen][logon].

2. Kun kirjaudut virtuaalikoneen, Avaa **Serverin hallinta**. Valitse vasemmanpuoleisessa ruudussa **tiedoston ja tallennustilaa palvelut**.

    ![Avaa palvelimen hallinta](./media/virtual-machines-windows-classic-attach-disk/fileandstorageservices.png)

3. Laajenna-valikko ja valitse **levyjä**.

4. **Levyjen** -osassa on luettelo levyjen. Useimmissa tapauksissa se on levy 0, levy 1 ja 2 levyn. Levy 0 on käyttöjärjestelmän levy, levy 1 on väliaikainen levy ja levy 2 on liitetty AM vain tiedot-levy. Uusien tietojen levyn sisältyviä osio on **Tuntematon**. Levyn hiiren kakkospainikkeella ja valitse **alusta**.

5.  Saat ilmoituksen, että kaikki tiedot poistetaan, kun levyn alustaa. Valitse **Kyllä** , kuittaa varoitus ja alusta levy. Kun valmis Partion lueteltujen **GPT**. Napsauta levyn uudelleen hiiren kakkospainikkeella ja valitse **Uusi asema**.

6.  Suorita ohjattu käyttämällä oletusarvoja. Kun ohjattu toiminto on valmis, **asemat** -osassa on luettelo uusi asema. Levy on nyt online-tilassa ja käyttövalmis tietojen tallentamiseen.

    ![Avauksen ja vaihdon onnistuneesti](./media/virtual-machines-windows-classic-attach-disk/newvolumecreated.png)

> [AZURE.NOTE] Koon AM määrittää, kuinka monta levyjä, voit liittää siihen. Lisätietoja on artikkelissa [näennäiskoneiden koot](virtual-machines-linux-sizes.md).

## <a name="additional-resources"></a>Lisäresursseja

[Miten irrottaa tietokoneesta Windowsin virtual DVD-levyllä](virtual-machines-windows-classic-detach-disk.md)

[Tietoja levyille ja näennäiskiintolevyjen näennäiskoneiden](virtual-machines-linux-about-disks-vhds.md)

[logon]: virtual-machines-windows-classic-connect-logon.md

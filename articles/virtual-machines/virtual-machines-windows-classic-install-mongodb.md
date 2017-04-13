<properties
    pageTitle="Asenna Windows AM MongoDB | Microsoft Azure"
    description="Opi asentamaan MongoDB, joka on luotu käyttämällä perinteinen käyttöönoton mallin käytössä Windows Server Azure-AM."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="iainfou"/>

#<a name="install-mongodb-on-a-windows-vm"></a>Asenna Windows AM MongoDB

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Asenna ja määritä MongoDB Resurssienhallinta käyttöönoton mallin, lue [artikkeli](virtual-machines-windows-classic-install-mongodb.md).

[MongoDB] [ MongoDB] on Suositut Avaa lähde, tehokas NoSQL tietokanta. Tämän artikkelin avulla voit luoda [Azure perinteinen portal]Windows Server virtual kone (AM)[AzurePortal]. Luo ja liitä tiedot DVD-levyllä ennen asentaminen ja määrittäminen MongoDB AM. Jos sinulla on aiemmin AM Azure-tietokannassa, jossa haluat käyttää, voit siirtyä suoraan [asennuksesta ja määrityksestä MongoDB](#install-and-run-mongodb-on-the-virtual-machine).


## <a name="create-a-virtual-machine-running-windows-server"></a>Luo virtual koneen käytössä Windows Server

Noudata seuraavia ohjeita voit luoda virtual machine.

[AZURE.INCLUDE [virtual-machines-create-WindowsVM](../../includes/virtual-machines-create-windowsvm.md)]

> [AZURE.NOTE] Voit lisätä päätepisteen MongoDB luotaessa virtuaalikoneen ja määrittää sen seuraavasti: Anna sille nimi kuin **Mongo**, käytä **TCP** -protokolla ja määritä julkisista ja yksityisistä-portit **27017**.

## <a name="attach-a-data-disk"></a>Liitä tiedot DVD-levyllä
Antamaan virtuaalikoneen tallennustilan Liitä tiedot DVD-levyllä ja alusta sen niin, että Windows käyttää sitä. Jos sinulla on jo tietoja DVD-levyllä, voit liittää aiemmin levyn tai voit liittää tyhjä levy.

[AZURE.INCLUDE [howto-attach-disk-windows-linux](../../includes/howto-attach-disk-windows-linux.md)]

Ohjeita valmistellaan levy on artikkelissa "miten: alusta uudet tiedot-levy Windows Server"- [liittäminen tietojen DVD-levyllä Windows virtual koneeseen](virtual-machines-windows-classic-attach-disk.md).

## <a name="install-and-run-mongodb-on-the-virtual-machine"></a>Asentaminen ja suorittaminen MongoDB virtuaalikoneen

[AZURE.INCLUDE [install-and-run-mongo-on-win2k8-vm](../../includes/install-and-run-mongo-on-win2k8-vm.md)]

## <a name="summary"></a>Yhteenveto
Tässä opetusohjelmassa opit, voit luoda virtual koneen käytössä Windows Server, muodostaa siihen etäyhteyden ja liitä tiedot DVD-levyllä.  Opit myös asentaa ja määrittää MongoDB Windows-pohjaisesta virtuaalikoneen. Nyt voit käyttää MongoDB-Windows-pohjaisesta-virtuaalikoneen seuraamalla [MongoDB dokumentaatio]Lisäasetukset ohjeaiheet[MongoDocs].

[MongoDocs]: http://docs.mongodb.org/manual/
[MongoDB]: http://www.mongodb.org/
[AzurePortal]: http://manage.windowsazure.com

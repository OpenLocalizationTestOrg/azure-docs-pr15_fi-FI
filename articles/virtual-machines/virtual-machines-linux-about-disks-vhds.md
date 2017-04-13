<properties
    pageTitle="Tietoja levyille ja Linux VMs näennäiskiintolevyjen | Microsoft Azure"
    description="Lisätietoja levyjen ja näennäiskiintolevyjen perusteet Linux näennäiskoneiden Azure."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/16/2016"
    ms.author="cynthn"/>

# <a name="about-disks-and-vhds-for-azure-virtual-machines"></a>Tietoja levyille ja Azure-virtuaalikoneissa näennäiskiintolevyjen

Toiseen tietokoneeseen, kuten näennäiskoneiden Azure-tietokannassa avulla levyjen sijainniksi tallentaa käyttöjärjestelmän ja sovellusten. Kaikki Azuren näennäiskoneiden on vähintään kaksi levyä – Linux käyttöjärjestelmän DVD-levyllä (kyseessä Linux AM) ja tilapäinen DVD-levyllä. Käyttöjärjestelmä levyn luodaan kuvan, ja käyttöjärjestelmän levyn ja kuvan ovat todella virtual kiintolevyillä (näennäiskiintolevyjen) tallennetaan Azure-tallennustilan tilin. Näennäiskoneiden voi olla myös vähintään yksi tietojen levyjä, joka tallennetaan myös näennäiskiintolevyjen. Tässä artikkelissa on myös [Windows-näennäiskoneiden](virtual-machines-windows-about-disks-vhds.md)käytettävissä.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

> [AZURE.NOTE] Jos sinulla on hetkeksi, Auta meitä kehittämään Azure Linux AM ohjeista tehokkaasta tämän oman kokemukset [nopeasti kyselyn](https://aka.ms/linuxdocsurvey) . Jokaisen vastauksen auttaa meitä ohjeen saat tekemäsi työt.

## <a name="operating-system-disk"></a>Käyttöjärjestelmän levy

Jokaisen virtuaalikoneen on liitetty käyttöjärjestelmän toiselle levylle. Se on rekisteröity SATA-asema ja merkintä oletusarvoisesti c-asema. Tämä on suurin kapasiteetista 1023 gigatavua (gt). 

##<a name="temporary-disk"></a>Tilapäinen levy

Tilapäinen levyn luodaan automaattisesti puolestasi. Valitse Linux näennäiskoneiden levy on yleensä /dev/sdb ja on muotoiltu ja lisätallennustilaa /mnt/resource Azure Linux-agentti mukaan.

Tilapäinen levyn koko vaihtelee virtuaalikoneen koon perusteella. Lisätietoja on artikkelissa [Linux näennäiskoneiden koot](virtual-machines-linux-sizes.md).

>[AZURE.WARNING] Älä tallenna tietojen tilapäinen levyn. Se tarjoaa väliaikaisten sovellukset ja prosessit ja on tarkoitus tallentaa vain tiedot, kuten sivu tai Vaihda tiedostot. 

Lisätietoja siitä, miten Azure käyttää tilapäinen levy on kohdassa [tietoa tilapäinen asema, Microsoft Azuren näennäiskoneiden](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)

## <a name="data-disk"></a>Tietoja levyn

Tietoja DVD-levyllä on Näennäiskiintolevyn, joka on liitetty virtual tietokoneeseen, voit tallentaa sovelluksen tietoja tai muita tietoja, jotka haluat säilyttää. Tietoja levyjen on rekisteröity SCSI asemat ja merkitty kirjaimella, joka on valittava.  Kunkin tiedot on suurin kapasiteetin 1023 gigatavua. Virtuaalikoneen koon määrää isännöimiseen levyjen käyttää kuinka monta tietojen levyjä, voit liittää sen ja tallennustilaa tyyppi.

>[AZURE.NOTE] Saat lisätietoja näennäiskoneiden valmiuksia [Linux näennäiskoneiden koot](virtual-machines-linux-sizes.md).

Azure Luo käyttöjärjestelmä-levyn, kun luot virtual machine kuva. Jos käytät kuva, joka sisältää tietoja-levyjä, Azure luo myös tietojen-levyjä, kun se luo virtuaalikoneen. Muussa tapauksessa voit lisätä tietojen levyjä, virtuaalikoneen luomisen jälkeen.

Voit lisätä tietoja levyjen virtual tietokoneeseen milloin tahansa **liittäminen** virtuaalikoneen levylle. Voit käyttää Näennäiskiintolevyn on ladattu tai kopioidut tallennustilan tilisi tai aiemmin Azure luo puolestasi. Liittäminen tietojen DVD-levyllä liittää tallennustilan tililtä Näennäiskiintolevytiedosto virtuaalikoneen, sijoittamalla varauksen-' ylläpito, jotta sitä voi poistaa säilöstä samalla, kun se on edelleen liitetty.

## <a name="about-vhds"></a>Tietoja näennäiskiintolevyjen

Käyttää Azure näennäiskiintolevyjen ovat .vhd-tiedostoja, jotka on tallennettu vakio- tai maksullisten tallennustilan tilillä Azure-BLOB sivun. Lisätietoja sivun BLOB-objektit on artikkelissa [tietoja estä BLOB-objektit ja sivun BLOB-objektit](https://msdn.microsoft.com/library/ee691964.aspx). Lisätietoja tietoja premium tallentamisesta on artikkelissa [Premium tallennustilan: tehokas tallennustila Azure virtuaalikoneen työmääriä](../storage/storage-premium-storage.md).

Azure tukee kiintolevy Näennäiskiintolevyn muotoa. Kiinteä muoto säädetään loogisen levyn lineaarisesti tiedostossa, niin, että levyn siirtymä X tallennetaan blob siirtymä X. Pieni alatunnisteen blob lopussa kuvataan Näennäiskiintolevyn ominaisuudet. Usein kiinteä muoto vievät tilaa turhaan koska useimmille levyille on suuri käyttämättömät alueiden niitä. Kuitenkin Azure tallentaa .vhd tiedostot lyhyet muodossa, jotta saat kiinteä ja dynaaminen-levyjen edut samanaikaisesti. Lisätietoja on artikkelissa [virtual kiintolevyillä käytön aloittaminen](https://technet.microsoft.com/library/dd979539.aspx).

Kaikki Azure, jota haluat käyttää lähteenä levyjen tai kuvien luomiseen .vhd tiedostot ovat vain luku-tilassa. Kun luot levyltä tai kuva, Azure tekee .vhd tiedostojen kopiot. Näitä kopioita voi olla vain luku- tai luku-ja-ja kirjoitusoikeudet, riippuen siitä, miten voit käyttää Näennäiskiintolevyn.

Kun luot virtual machine kuvan, Azure Luo virtuaalikoneen, joka on kopio .vhd lähdetiedosto DVD-levyllä. Voit suojautua vahingossa Azure sijoittaa varaus lähde .vhd-tiedosto, jota käytetään kuvan, käyttöjärjestelmä-levyn tai tietojen DVD-levyllä luomiseen.

Ennen kuin voit poistaa .vhd lähdetiedosto, tarvitset Poista varauksen poistamalla levyn tai kuva. .Vhd-tiedoston, joka on käytössä virtual machine kuin käyttöjärjestelmän-levyn poistaminen, voit poistaa virtuaalikoneen-käyttöjärjestelmä, levyn ja .vhd lähdetiedosto yhdellä kertaa poistaminen virtuaalikoneen ja poistamalla kaikki siihen levyjä. Kuitenkin .vhd-tiedosto, joka on lähteen tiedot levyn poistaminen edellyttää, että useita vaiheita määrittäminen järjestyksessä--irrottaa virtuaalikoneen levyltä, Poista levy, ja poista .vhd-tiedosto.

>[AZURE.WARNING] Jos .vhd lähdetiedosto poistaminen tallennustilan tai poistaa tallennustilaa tilisi, Microsoft ei voi palauttaa tietoja puolestasi.


## <a name="troubleshooting"></a>Vianmääritys
[AZURE.INCLUDE [virtual-machines-linux-lunzero](../../includes/virtual-machines-linux-lunzero.md)]

## <a name="next-steps"></a>Seuraavat vaiheet

-  [Liitä DVD-levyllä](virtual-machines-linux-add-disk.md) Lisää tallennustilaa oman AM.
-  [Määritä ohjelmiston RAID](virtual-machines-linux-configure-raid.md) redundanssin.
-  [Siepata Linux virtual koneen](virtual-machines-linux-classic-capture-image.md) , jotta voit ottaa nopeasti käyttöön muita VMs.



<properties
    pageTitle="Tietoja levyille ja Windows näennäiskiintolevyjen VMs | Microsoft Azure"
    description="Lisätietoja levyille ja näennäiskoneiden näennäiskiintolevyjen Windows Azure-tietokannassa."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="cynthn"/>

# <a name="about-disks-and-vhds-for-azure-virtual-machines"></a>Tietoja levyille ja Azure-virtuaalikoneissa näennäiskiintolevyjen

Toiseen tietokoneeseen, kuten näennäiskoneiden Azure-tietokannassa avulla levyjen sijainniksi tallentaa käyttöjärjestelmän ja sovellusten. Kaikki Azuren näennäiskoneiden on vähintään kaksi levyä – Windows-käyttöjärjestelmän levyn ja tilapäinen DVD-levyllä. Käyttöjärjestelmän levy on luotu kuvan, ja käyttöjärjestelmän levyn ja kuvan ovat virtual kiintolevyillä (näennäiskiintolevyjen) tallennetaan Azure-tallennustilan tilin. Näennäiskoneiden voi olla myös vähintään yksi tietojen levyjä, joka tallennetaan myös näennäiskiintolevyjen. Tässä artikkelissa on myös [Linux näennäiskoneiden](virtual-machines-linux-about-disks-vhds.md)käytettävissä.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]



## <a name="operating-system-disk"></a>Käyttöjärjestelmän levy

Jokaisen virtuaalikoneen on liitetty käyttöjärjestelmän toiselle levylle. Se on rekisteröity SATA-asema ja merkintä oletusarvoisesti c-asema. Tämä on suurin kapasiteetista 1023 gigatavua (gt). 

##<a name="temporary-disk"></a>Tilapäinen levy

Tilapäinen levyn luodaan automaattisesti puolestasi. D:-asemassa merkintä tilapäinen levyn oletusarvon mukaan ja se tallennetaan pagefile.sys. 

Tilapäinen levyn koko vaihtelee virtuaalikoneen koon perusteella. Lisätietoja on artikkelissa [Windows koot näennäiskoneiden](virtual-machines-windows-sizes.md).

>[AZURE.WARNING] Älä tallenna tietojen tilapäinen levyn. Se tarjoaa väliaikaisten sovellukset ja prosessit ja on tarkoitus tallentaa vain tiedot, kuten sivu tai Vaihda tiedostot. Yhdistettävä uudelleen levyä toinen kirjain, katso [muuttaminen Windowsin tilapäinen levy kirjain](virtual-machines-windows-classic-change-drive-letter.md).

Lisätietoja siitä, miten Azure käyttää tilapäinen levy on kohdassa [tietoa tilapäinen asema, Microsoft Azuren näennäiskoneiden](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)

## <a name="data-disk"></a>Tietoja levyn

Tietoja DVD-levyllä on Näennäiskiintolevyn, joka on liitetty virtual tietokoneeseen, voit tallentaa sovelluksen tietoja tai muita tietoja, jotka haluat säilyttää. Tietoja levyjen on rekisteröity SCSI asemat ja merkitty kirjaimella, joka on valittava.  Kunkin tiedot on suurin kapasiteetin 1023 gigatavua. Virtuaalikoneen koon määrää isännöimiseen levyjen käyttää kuinka monta tietojen levyjä, voit liittää sen ja tallennustilaa tyyppi.

>[AZURE.NOTE] Saat lisätietoja näennäiskoneiden valmiuksia [koot Windows näennäiskoneiden](virtual-machines-windows-sizes.md).

Azure Luo käyttöjärjestelmä-levyn, kun luot virtual machine kuva. Jos käytät kuva, joka sisältää tietoja-levyjä, Azure luo myös tietojen-levyjä, kun se luo virtuaalikoneen. Muussa tapauksessa voit lisätä tietojen levyjä, virtuaalikoneen luomisen jälkeen.

Voit lisätä tietoja levyjen virtual tietokoneeseen milloin tahansa **liittäminen** virtuaalikoneen levylle. Voit käyttää Näennäiskiintolevyn on ladattu tai kopioidut tallennustilan tilisi tai aiemmin Azure luo puolestasi. Liittäminen tietojen DVD-levyllä liittää Näennäiskiintolevytiedosto AM sijoittamalla varauksen-' ylläpito, jotta sitä voi poistaa säilöstä samalla, kun se on edelleen liitetty.

## <a name="about-vhds"></a>Tietoja näennäiskiintolevyjen

Käyttää Azure näennäiskiintolevyjen ovat .vhd-tiedostoja, jotka on tallennettu vakio- tai maksullisten tallennustilan tilillä Azure-BLOB sivun. Saat lisätietoja sivun BLOB [tietoja estä BLOB-objektit ja sivun BLOB-objektit](https://msdn.microsoft.com/library/ee691964.aspx). Saat lisätietoja premium tallennustilan [Premium tallennustilan: tehokas tallennustila Azure virtuaalikoneen työmääriä](../storage/storage-premium-storage.md).

Azure tukee kiintolevy Näennäiskiintolevyn muotoa. Kiinteä-muotoisten säädetään loogisen levyn lineaarisesti tiedostossa, niin, että levyn siirtymä X tallennetaan blob siirtymä X. Pieni alatunnisteen blob lopussa kuvataan Näennäiskiintolevyn ominaisuudet. Kiinteä muoto jätteet tilaa usein, koska useimmille levyille on suuri käyttämättömät alueiden niitä. Kuitenkin Azure tallentaa .vhd tiedostot lyhyet muodossa, jotta saat kiinteä ja dynaaminen-levyjen edut samanaikaisesti. Lisätietoja on artikkelissa [virtual kiintolevyillä käytön aloittaminen](https://technet.microsoft.com/library/dd979539.aspx).

Kaikki Azure, jota haluat käyttää lähteenä levyjen tai kuvien luomiseen .vhd tiedostot ovat vain luku-tilassa. Kun luot levyltä tai kuva, Azure tekee .vhd tiedostojen kopiot. Näitä kopioita voi olla vain luku- tai luku-ja-ja kirjoitusoikeudet, riippuen siitä, miten voit käyttää Näennäiskiintolevyn.

Kun luot virtual machine kuvan, Azure Luo virtuaalikoneen, joka on kopio .vhd lähdetiedosto DVD-levyllä. Voit suojautua vahingossa Azure sijoittaa varaus lähde .vhd-tiedosto, jota käytetään kuvan, käyttöjärjestelmä-levyn tai tietojen DVD-levyllä luomiseen.

Ennen kuin voit poistaa .vhd lähdetiedosto, tarvitset Poista varauksen poistamalla levyn tai kuva. Virtuaalikoneen käyttöjärjestelmän levyä, voit poistaa ja .vhd lähdetiedosto yhdellä kertaa poistaminen virtuaalikoneen ja poistamalla kaikki siihen levyjä. Kuitenkin .vhd-tiedosto, joka on lähteen tiedot levyn poistaminen edellyttää useita vaiheita määrittäminen järjestyksessä. Ensin voit irrottaa virtuaalikoneen-levyltä ja valitse Poista levy ja poista .vhd-tiedosto.

>[AZURE.WARNING] Jos .vhd lähdetiedosto poistaminen tallennustilan tai poistaa tallennustilaa tilisi, Microsoft ei voi palauttaa tietoja puolestasi.



## <a name="next-steps"></a>Seuraavat vaiheet
-  [Liitä DVD-levyllä](virtual-machines-windows-attach-disk-portal.md) Lisää tallennustilaa oman AM.
-  [Lataa kuva AM Windows Azure,](virtual-machines-windows-upload-image.md) uusi AM luomiseen.
-  Jotta sovelluksesi käyttää d-asema tietojen [muuttaminen Windowsin tilapäinen levy kirjain](virtual-machines-windows-classic-change-drive-letter.md) .

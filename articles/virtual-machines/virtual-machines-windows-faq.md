<properties
    pageTitle="Usein kysyttyjä Kysymyksiä Windows VMs | Microsoft Azure"
    description="On vastauksia joihinkin Windows näennäiskoneiden Resurssienhallinta-mallin avulla luotu usein kysyttyihin kysymyksiin."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="cynthn"/>

# <a name="frequently-asked-question-about-windows-virtual-machines"></a>Usein kysyttyjä kysymyksiä Windows näennäiskoneiden tietoja 


Tässä artikkelissa käsitellään Windows näennäiskoneiden luotu Azure Resurssienhallinta käyttöönoton mallin usein kysyttyihin kysymyksiin. Tässä ohjeaiheessa Linux-versio on [usein kysyttyjä kysymyksiä Linux näennäiskoneiden tietoja](virtual-machines-linux-faq.md)

## <a name="what-can-i-run-on-an-azure-vm"></a>Mitä Azure-AM-voi käyttää?

Kaikki tilaajille voidaan suorittaa Palvelinohjelmisto Azure virtuaalikoneen. Lisätietoja tukikäytännöistä suorittamisen Palvelinohjelmisto Microsoft Azure-tietokannassa on artikkelissa [Microsoft Palvelinohjelmisto tukevat Azuren näennäiskoneiden](https://support.microsoft.com/kb/2721672)

Tiettyjen Windows 7 ja Windows 8.1-versioiden ovat käytettävissä MSDN Azure etu tilaajille ja MSDN keskihajonta ja testaa ryhmävakuutussopimukset tilaajille kehitys- ja tehtävien. Lisätietoja on ohjeet ja rajoitukset, kuten kohdassa [Windows-asiakasohjelman kuvia MSDN-tilaajille](http://azure.microsoft.com/blog/2014/05/29/windows-client-images-on-azure/). 


## <a name="how-much-storage-can-i-use-with-a-virtual-machine"></a>Kuinka paljon tallennustilaa virtual machine kanssa voidaan käyttää?

Tietoja kunkin levyn voi olla enintään 1 Teratavun. Voit käyttää tietojen levyjen määrä määräytyy virtuaalikoneen kokoa. Lisätietoja on artikkelissa [näennäiskoneiden koot](virtual-machines-windows-sizes.md).

Azure-tallennustilan tilin tallennustila käyttöjärjestelmän levyn ja kaikki tiedot levyjä. Kunkin levy on tallennettu sivun Blob-objektien .vhd-tiedosto. Hinnat tiedot-kohdassa [Tallennustilan hinnat tiedot](https://azure.microsoft.com/pricing/details/storage/).


## <a name="how-can-i-access-my-virtual-machine"></a>Miten voin käyttää Omat virtuaalikoneen?

Muodostaa yhteyden RDP (Remote Desktop) käyttäminen Windows AM etäyhteyden. Katso ohjeet [liittämisestä ja Azure virtual-tietokoneessa, jossa Windows-kirjautumisen](virtual-machines-windows-connect-logon.md). Kaksi samanaikaista yhteyttä enintään tuetaan, paitsi jos palvelin on määritetty Etätyöpöytäpalvelut istunnon isäntään.  


Jos sinulla on ongelmia etätyöpöydän kautta, katso [vianmääritys etätyöpöydän yhteydet, Windows-pohjaisesta Azure Virtual koneen](virtual-machines-windows-troubleshoot-rdp-connection.md). 

Jos olet tottunut käyttämään Hyper-V, etsit ehkä samalla VMConnect työkalun. Azure ei tarjoa samanlaisia työkalua, koska konsoli käyttää virtual tietokonetta ei ole tuettu.

## <a name="can-i-use-the-temporary-disk-the-d-drive-by-default-to-store-data"></a>Tallentaa tiedot tilapäinen levyn (oletusarvoisesti d-asema) avulla?

Älä käytä tilapäinen levyn tietojen tallentamiseen. Se on vain väliaikaisten, niin menetä tietoja, joita ei voi palauttaa. Tietojen menetyksen voi ilmetä, kun virtuaalikoneen siirtää toisen isännän. Koon virtual machine-isäntä tai laitteiston epäonnistui isännän ovat virtual machine voi siirtää syistä.

Jos sinulla on sovellus, joka on käytettävä D: kirjain, voit määrittää asemakirjaimet niin, että tilapäinen levyn käyttää jollakin muulla kuin D:. Katso ohjeet [muuttaminen Windowsin tilapäinen levy kirjain](virtual-machines-windows-classic-change-drive-letter.md).

## <a name="how-can-i-change-the-drive-letter-of-the-temporary-disk"></a>Miten tilapäinen levyn kirjain voi muuttaa?

Voit muuttaa sivun tiedoston siirtäminen ja määrittämällä asemakirjaimet kirjain, mutta haluat Varmista, että tietyssä järjestyksessä toimet. Katso ohjeet [muuttaminen Windowsin tilapäinen levy kirjain](virtual-machines-windows-classic-change-drive-letter.md).

## <a name="can-i-add-an-existing-vm-to-an-availability-set"></a>Lisään aiemmin AM käytettävyys Määritä?

Ei. Oman AM käytettävyys-joukon osana halutessasi haluat luoda joukon sisällä AM. Tällä hetkellä ei ole mahdollisuutta lisätä AM Määritä, kun se on luotu saatavuus.

## <a name="can-i-upload-a-virtual-machine-to-azure"></a>Voin ladata virtual machine Azure?

Kyllä. Lisätietoja on kohdassa [Lataa AM Windows Azure, kuva](virtual-machines-windows-upload-image.md)

## <a name="can-i-resize-the-os-disk"></a>OS-levyn kokoa

Kyllä. Ohjeet Katso, [miten voit laajentaa Virtual koneen Azure-resurssiryhmä-OS-asema](virtual-machines-windows-expand-os-disk.md).

## <a name="can-i-copy-or-clone-an-existing-azure-vm"></a>Voit kopioida tai Kloonaa aiemmin Azure-AM?

Kyllä. Katso ohjeet [luominen Windows-virtual machine Resurssienhallinta käyttöönoton mallin kopio](virtual-machines-windows-vhd-copy.md).

## <a name="why-am-i-not-seeing-canada-central-and-canada-east-regions-through-azure-resource-manager"></a>Miksi en näe Kanada Keski- ja Kanada Itä alueiden Azure Resurssienhallinta kautta?

Kanada Keski- ja Kanada Itä kaksi kaksi uutta ei ole rekisteröity automaattisesti aiemmin Azure tilaukset virtuaalikoneen luomista varten. Tämä rekisteröinti tapahtuu automaattisesti, kun virtual machine otetaan käyttöön palvelun muiden Azure resurssien hallinnan alueen Azure-portaalissa. Kun virtual machine otetaan käyttöön Azure alue, kaksi uutta pitäisi olla käytettävissä seuraavien näennäiskoneiden.

## <a name="does-azure-support-linux-vms"></a>Tukeeko Azure Linux VMs?

Kyllä. Voit luoda nopeasti Linux AM, voit kokeilla, katso [luominen Linux AM, valitse Azure-portaalissa](virtual-machines-linux-quick-create-portal.md).

## <a name="can-i-add-a-nic-to-my-vm-after-its-created"></a>Voit lisätä NIC Omat AM sen luomisen jälkeen?

Ei. NIC lisääminen voi tehdä vain luonnin aikana.

## <a name="are-there-any-computer-name-requirements"></a>Mitä tahansa tietokoneen nimi vaatimuksia?

Kyllä. Tietokonenimi voi olla enintään 15 merkkiä. Saat lisätietoja ympärille nimeäminen resurssien [infrastruktuurin nimeämiseen liittyviä ohjeita](virtual-machines-windows-infrastructure-naming-guidelines.md) .

## <a name="what-are-the-username-requirements-when-creating-a-vm"></a>Mitä käyttäjänimi-vaatimuksista AM luotaessa?

Käyttäjänimet voi olla enintään 20 merkkiä ja ei voi loppua pisteeseen ("."). 

Seuraavat käyttäjänimet eivät ole sallittuja:

<table>
    <tr>
        <td style="text-align:center">Järjestelmänvalvoja </td><td style="text-align:center"> Järjestelmänvalvoja </td><td style="text-align:center"> käyttäjän </td><td style="text-align:center"> Käyttäjä1</td>
    </tr>
    <tr>
        <td style="text-align:center">Testaa </td><td style="text-align:center"> käyttäjä2 </td><td style="text-align:center"> test1 </td><td style="text-align:center"> user3</td>
    </tr>
    <tr>
        <td style="text-align:center">admin1 </td><td style="text-align:center"> 1 </td><td style="text-align:center"> 123 </td><td style="text-align:center"> lisääminen</td>
    </tr>
    <tr>
        <td style="text-align:center">actuser  </td><td style="text-align:center"> adm </td><td style="text-align:center"> admin2 </td><td style="text-align:center"> aspnet</td>
    </tr>
    <tr>
        <td style="text-align:center">Varmuuskopiointi </td><td style="text-align:center"> konsolin </td><td style="text-align:center"> David </td><td style="text-align:center"> Vieras</td>
    </tr>
    <tr>
        <td style="text-align:center">Teemu </td><td style="text-align:center"> omistaja </td><td style="text-align:center"> pääkansio </td><td style="text-align:center"> palvelin</td>
    </tr>
    <tr>
        <td style="text-align:center">SQL </td><td style="text-align:center"> tuki </td><td style="text-align:center"> support_388945a0 </td><td style="text-align:center"> sys</td>
    </tr>
    <tr>
        <td style="text-align:center">test2 </td><td style="text-align:center"> test3 </td><td style="text-align:center"> Käyttäjä4 </td><td style="text-align:center"> user5</td>
    </tr>
</table>

## <a name="what-are-the-password-requirements-when-creating-a-vm"></a>Mitä salasanavaatimukset AM luotaessa?

Salasanojen on oltava 8 123 merkkiä ja täytettävä seuraavat vaatimukset 4 monimutkaisuus ulos 3:

- On pienempi merkit
- Olla ylemmän merkkiä
- On numero
- On erikoismerkki (Regex vastattava [\W_])

Seuraavat salasanat eivät ole sallittuja:

<table>
    <tr>
        <td style="text-align:center">abc@123</td><td style="text-align:center">P@$$w0rd</td><td style="text-align:center">P@ssw0rd</td><td style="text-align:center">P@ssword123</td><td style="text-align:center">Pa$ word</td>
    </tr>
    <tr>
        <td style="text-align:center">pass@word1</td><td style="text-align:center">Salasanan!</td><td style="text-align:center">Salasana1</td><td style="text-align:center">Password22</td><td style="text-align:center">ILOVEYOU!</td>
    </tr>
</table>

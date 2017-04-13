<properties
    pageTitle="Usein kysytyt kysymykset Linux VMs | Microsoft Azure"
    description="On vastauksia joihinkin Linux näennäiskoneiden Resurssienhallinta-mallin avulla luotu usein kysyttyihin kysymyksiin."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="cynthn"/>

# <a name="frequently-asked-question-about-linux-virtual-machines"></a>Usein kysyttyjä kysymyksiä Linux näennäiskoneiden tietoja

Tässä artikkelissa käsitellään Linux näennäiskoneiden luotu Azure Resurssienhallinta käyttöönoton mallin usein kysyttyihin kysymyksiin. Katso tämän artikkelin Windows-version [usein kysyttyjä kysymyksiä tietoja Windows näennäiskoneiden](virtual-machines-windows-faq.md)

## <a name="what-can-i-run-on-an-azure-vm"></a>Mitä Azure-AM-voi käyttää?

Kaikki tilaajille voidaan suorittaa Palvelinohjelmisto Azure virtuaalikoneen. Lisätietoja on artikkelissa [Linux-Azure-Endorsed jakelu](virtual-machines-linux-endorsed-distros.md)


## <a name="how-much-storage-can-i-use-with-a-virtual-machine"></a>Kuinka paljon tallennustilaa virtual machine kanssa voidaan käyttää?

Tietoja kunkin levyn voi olla enintään 1 Teratavun. Voit käyttää tietojen levyjen määrä määräytyy virtuaalikoneen kokoa. Lisätietoja on artikkelissa [näennäiskoneiden koot](virtual-machines-linux-sizes.md).

Azure-tallennustilan tilin tallennustila käyttöjärjestelmän levyn ja kaikki tiedot levyjä. Kunkin levy on tallennettu sivun Blob-objektien .vhd-tiedosto. Hinnat tiedot-kohdassa [Tallennustilan hinnat tiedot](https://azure.microsoft.com/pricing/details/storage/).


## <a name="how-can-i-access-my-virtual-machine"></a>Miten voin käyttää Omat virtuaalikoneen?

Muodostaa etäyhteyden kirjautua virtuaalikoneen suojattu runko (SSH) avulla. Voit muodostaa yhteyden [Windows](virtual-machines-linux-ssh-from-windows.md) - tai [Linux-ja Mac](virtual-machines-linux-mac-create-ssh-keys.md)ohjeiden mukaisesti. Oletusarvon mukaan SSH sallii enintään 10 samanaikaisia yhteyksiä. Voit suurentaa tämän numeron muokkaamalla määritystiedostoa.


Jos sinulla on ongelmia, tutustu artikkeliin [vianmääritys suojattu runko (SSH) yhteydet](virtual-machines-linux-troubleshoot-ssh-connection.md).


## <a name="can-i-use-the-temporary-disk-devsdb1-to-store-data"></a>Tallentaa tiedot tilapäinen levyn (/ keskihajonta/sdb1) avulla?

Älä käytä tilapäinen levyn (/ keskihajonta/sdb1), tietojen tallentamiseen. Se on käytettävissä vain väliaikaisten. Voit menetä tietoja, joita ei voi palauttaa.


## <a name="can-i-copy-or-clone-an-existing-azure-vm"></a>Voit kopioida tai Kloonaa aiemmin Azure-AM?

Kyllä. Katso ohjeet [luomisesta Linux virtual kone Resurssienhallinta käyttöönoton mallin kopio](virtual-machines-linux-copy-vm.md).


## <a name="why-am-i-not-seeing-canada-central-and-canada-east-regions-through-azure-resource-manager"></a>Miksi en näe Kanada Keski- ja Kanada Itä alueiden Azure Resurssienhallinta kautta?

Kanada Keski- ja Kanada Itä kaksi kaksi uutta ei ole rekisteröity automaattisesti aiemmin Azure tilaukset virtuaalikoneen luomista varten. Tämä rekisteröinti tapahtuu automaattisesti, kun virtual machine otetaan käyttöön palvelun muiden Azure resurssien hallinnan alueen Azure-portaalissa. Kun virtual machine otetaan käyttöön Azure alue, kaksi uutta pitäisi olla käytettävissä seuraavien näennäiskoneiden.


## <a name="can-i-add-a-nic-to-my-vm-after-its-created"></a>Voit lisätä NIC Omat AM sen luomisen jälkeen?

Ei. NIC lisääminen voi tehdä vain luonnin aikana.


## <a name="are-there-any-computer-name-requirements"></a>Mitä tahansa tietokoneen nimi vaatimuksia?

Kyllä. Tietokonenimi voi olla enintään 64 merkkiä. Saat lisätietoja ympärille nimeäminen resurssien [infrastruktuurin nimeämiseen liittyviä ohjeita](virtual-machines-linux-infrastructure-naming-guidelines.md) .


## <a name="what-are-the-username-requirements-when-creating-a-vm"></a>Mitä käyttäjänimi-vaatimuksista AM luotaessa?

Käyttäjänimet on oltava 1-64 merkin pituinen.

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

Salasanojen on 6 72 merkkiä ja täyttävät 3 ulos 4 monimutkaisuus seuraavat vaatimukset:

- On pienempi merkit
- Olla ylemmän merkkiä
- On numero
- On erikoismerkki (Regex vastattava [\W_])

Seuraavat salasanat eivät ole sallittuja:

<table>
    <tr>
        <td style="text-align:center">abc@123</td>
        <td style="text-align:center">P@$$w0rd</td>
        <td style="text-align:center">P@ssw0rd</td>
        <td style="text-align:center">P@ssword123</td>
        <td style="text-align:center">Pa$ word</td>
    </tr>
    <tr>
        <td style="text-align:center">pass@word1</td>
        <td style="text-align:center">Salasanan!</td>
        <td style="text-align:center">Salasana1</td>
        <td style="text-align:center">Password22</td>
        <td style="text-align:center">ILOVEYOU!</td>
    </tr>
</table>

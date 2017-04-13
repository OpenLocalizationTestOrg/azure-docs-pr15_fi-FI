<properties
    pageTitle="Tallennustilan ratkaisuja ohjeet | Microsoft Azure"
    description="Lisätietoja keskeisiä suunnittelu ja käyttöönotto ohjeita käyttöönoton Azure infrastruktuuripalvelut tallennustilan ratkaisuja."
    documentationCenter=""
    services="virtual-machines-windows"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="storage-infrastructure-guidelines"></a>Tallennustilan infrastruktuurin ohjeet

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)] 

Tässä artikkelissa keskitytään tietoja tallennustilatarpeet ja tyyliseikat virtuaalikoneen (AM) parhaan mahdollisen suorituskyvyn saavuttamiseksi.


## <a name="implementation-guidelines-for-storage"></a>Tallennustilan käyttöönotto-ohjeet

Päätökset:

- Sinun on käytettävä vakio- tai Premium tallennustilan havainnollistamiseen
- Luo levyjen 1023 megatavun raitasarjoittamista levy on?
- Levyn raitasarjoittamista saavuttamiseksi havainnollistamiseen i/o optimaalisen on?
- Mitä määrittäminen tallennustilan tilien sinun tarvitsee isännöidä IT työmäärää tai infrastruktuurin?

Tehtävät:

- Tarkista i/o vaatimukset sovellusten käyttöönoton ja Suunnittele sopiva numero ja tallennustilaa tilien tyyppi.
- Luo tallennustilan tilille nimeämiskäytännön mukaisesti. Voit käyttää PowerShellin Azure tai portaalin.


## <a name="storage"></a>Tallennustilan

Azuren tallennustila on avaimen osa käyttöönotto ja näennäiskoneiden (VMs) ja sovellusten hallinta. Azure-tallennustilan tarjoaa projektiin tiedostotietoja, erimuotoisia tietoja ja viestit ja se on myös tukevat VMs infrastruktuurin osa.

Käytettävissä olevat tukemiseen VMs on kahdenlaisia tallennustilan tilejä:

- Vakio tallennustilan tilit antaa käyttää Blob-objektien tallennustilaan (käytetään projektiin Azure AM levyjen)-taulukkotallennus, jonossa varasto ja tallentamista.
- [Premium tallennustilan](../storage/storage-premium-storage.md) tilit pitää tehokkaita, pieni viive levyn i/o tehostettu toiminnoista, kuten AlwaysOn-klusterin SQL-palvelimet tuki. Premium tallennustilan tukee tällä hetkellä vain Azure AM levyille.

Azure Luo VMs käyttöjärjestelmä-levyn, väliaikaiset DVD-levyllä ja nolla tai useammalle valinnaiset tiedot levylle. Käyttöjärjestelmä levyn ja tietojen levyjen ovat sivun Azure-BLOB-tilapäinen levy on tallennettu paikallisesti solmu, jossa koneen sijaitsee. Huolehtia, kun suunnittelet sovellusten käyttää vain tilapäinen levyä-pysyvien tietojen kuin AM voidaan siirtää isäntien ylläpitotapahtuman aikana välillä. Tilapäinen levyn tallennetut tiedot menetetään.

Kestävyys ja suuren käytettävyyden tarjoaa pohjana Azuren tallennustilaan ympäristön varmistaa, että tiedot on suojattu vastaan suunnittelematon ylläpito tai laitteiston virheet. Kun suunnittelet Azuren tallennustilaan-ympäristössä, voit toistaa AM tallennustilan:

- paikallisesti annetun Azure palvelinkeskuksen sisällä
- yli Azure palvelinkeskusten tietyllä alueella
- Azure palvelinkeskusten eri alueiden välillä

Lue [Lisätietoja suuren Replikointiasetukset](../storage/storage-introduction.md#replication-for-durability-and-high-availability).

Käyttöjärjestelmä levyille ja tietojen levyjen on enimmäiskokoa 1023 gigatavua (gt). Blob enimmäiskoko on 1 024 Gigatavua ja, joka on oltava (gt on 1 024<sup>3</sup> tavua) Näennäiskiintolevytiedosto metatiedot (alatunniste). Tallennustilan välilyöntejä Windows Server 2012: ssa voit ylittäisivät rajoitusta mukaan yhteysvarannon yhdessä tietojen levyä, jos haluat esittää loogisen tietomääristä 1023 megatavun oman AM.

On skaalattavuus koskevat tietyt rajoitukset suunniteltaessa Azuren tallennustilaan käyttöönoton – Katso lisätietoja [Microsoft Azure tilauksen ja palvelun rajoitukset, kiintiöiden ja rajoitukset](azure-subscription-service-limits.md#storage-limits) . Katso myös [Azure tallennustilan skaalattavuus ja suorituskyvyn kohteet](../storage/storage-scalability-targets.md).

Sovelluksen muistiin voit tallentaa erimuotoisia objektitietoja, kuten asiakirjojen, kuvien, varmuuskopioiden, kokoonpanotietoja, lokit jne. Voit käyttää Blob-objektien tallennustilaan. Sen sijaan, että sovelluksesi AM liitetty virtual levylle kirjoittamiseen sovellus voi kirjoittaa suoraan Azure-blob-säiliö. Blob-objektien tallennustilaan mahdollistaa myös [kuuman ja hienoja tallennustilan tasoa](../storage/storage-blob-storage-tiers.md) eri käytettävyys tarpeet ja kustannukset rajoitukset.


## <a name="striped-disks"></a>Mustat levyjen
Lisäksi voidaan luoda levyjen 1023 megatavun monta tilanteissa raitasarjoittamista käyttäminen tietojen levyjen parantaa suorituskykyä sallimalla useita BLOB yksittäisen aseman tallennustila taakse. Raitasarjoittamista, jossa i/o tarvitse kirjoittaa ja lukea yhden loogisen levyn etenee rinnakkain.

Azure joutuu tietojen levyjen määrä ja kaistanleveyttä käytettävissä, jos AM kokoa koskevat rajoitukset. Lisätietoja on artikkelissa [näennäiskoneiden koot](virtual-machines-windows-sizes.md).

Jos käytät levyn raitasarjoittamista Azure tietojen levyjä, voit seuraavia ohjeita:

- Tietoja levyjen on aina oltava enimmäiskoko (1023 gt).
- Liitä tietojen enimmäismäärä levyjen sallittu AM kokoa.
- Käytä tallennustilan välilyöntejä.
- Vältä Azure tietojen levyn välimuistiasetukset (välimuistiin käytännön = ei mitään).

Lisätietoja [tallennustilan välilyöntejä - suorituskyvyn suunnitteleminen](http://social.technet.microsoft.com/wiki/contents/articles/15200.storage-spaces-designing-for-performance.aspx).


## <a name="multiple-storage-accounts"></a>Usean tallennustilan tilin

Azuren tallennustilaan ympäristön suunnitellessasi voit käyttää useita tallennustilan tilejä VMs lukumääränä käyttöönottoa kasvaa. Näin voit jakaa pohjana Azuren tallennustilaan infrastruktuuri, jos haluat säilyttää parhaan mahdollisen suorituskyvyn VMs ja sovellusten ulos i/o. Kun suunnittelet sovellukset, jotka ovat käyttöönotosta, voit kunkin AM on i/o-vaatimukset ja saldo, kyseiset VMs Azuren tallennustilaan tilien välillä. Yritä välttää ryhmittely suuri I/O uudempaan VMs-vain yhden tai kahden tallennustilan tilit.

Lisätietoja eri Azure tallennusasetukset i/o-ominaisuuksista ja jotkin suosittelee enimmäisarvot on artikkelissa [Azure tallennustilan skaalattavuus ja suorituskyvyn kohteet](../storage/storage-scalability-targets.md).


## <a name="next-steps"></a>Seuraavat vaiheet

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)] 
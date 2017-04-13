<properties
    pageTitle="Virtuaalikoneen asteikko joukon luominen Azure-portaalissa | Microsoft Azure"
    description="Ota käyttöön asteikko joukot Azure-portaalissa."
    keywords="virtuaalikoneen asteikko joukot" 
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gatneil"
    manager="madhana"
    editor="tysonn"
    tags="azure-resource-manager" />

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/15/2016"
    ms.author="gatneil"/>

# <a name="create-a-virtual-machine-scale-set-using-the-azure-portal"></a>Virtuaalikoneen asteikko joukon luominen Azure-portaalissa

Tässä opetusohjelmassa kerrotaan, miten helppoa on virtuaalikoneen asteikko joukon luominen muutaman minuutin kuluttua mukaan Azure-portaalissa. Jos sinulla ei ole Azure tilauksen, luo [ilmaisen tilin](https://azure.microsoft.com/free/) , ennen kuin aloitat.

## <a name="choose-the-vm-image-from-the-marketplace"></a>Valitse AM kuva Marketplacesta

-Portaalista voit helposti ottaa asteikolla määrityksessä CentOS, CoreOS, Debian, Avaa Suse, punainen on Enterprise Linux, SUSE Linux Enterprise Serveriin, Ubuntu Server tai Windows Server-kuvia.

Ensimmäisen kerran Siirry [Azure portal](https://portal.azure.com) selaimessa. Valitse `New`, Etsi `scale set`, ja valitse sitten `Virtual machine scale set` tapahtuma:

![ScaleSetPortalOverview](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalOverview.PNG)

## <a name="create-the-linux-virtual-machine"></a>Luo Linux virtuaalikoneen

Voit nyt käyttää oletusasetuksia ja luoda nopeasti virtuaalikoneen.

* Valitse `Basics` sivu, asteikko-joukon nimi. Tämä nimi tulee kuormituksen eteen asteikko-määrittäminen FQDN kantaluvun, joten varmista, että nimi on yksilöllinen kaikissa Azure.

* Valitse haluamasi Käyttöjärjestelmä, kirjoita haluamasi käyttäjänimesi ja valitse mitä käyttöoikeuden lajin mieluummin. Jos valitset salasanan, sen on oltava vähintään 12 merkkiä pitkä ja täyttävät kolmen neljä monimutkaisuus seuraavat vaatimukset: yksi pieni kirjain, yksi iso kirjain, yksi numero ja yksi erikoismerkki. Katso lisätietoja [käyttäjänimi ja salasana](../virtual-machines/virtual-machines-windows-faq.md#what-are-the-username-requirements-when-creating-a-vm). Jos valitset `SSH public key`, varmista, että vain Liitä olevan julkinen avain ei yksityinen avain:

![ScaleSetPortalBasics](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalBasics.PNG)

* Kirjoita haluamasi resurssiryhmän nimi ja sijainti ja valitse sitten `OK`.

* Valitse `Virtual machine scale set service settings` sivu: haluamasi toimialueen nimen otsikko (perus kuormituksen eteen asteikko-määrittäminen FQDN). Tämän tarran on oltava yksilöllinen kaikissa Azure.

* Valitse haluamasi käyttöjärjestelmän levyn näköistiedoston, esiintymän määrä sekä tietokoneen koko.

* Ottaminen käyttöön tai poistaminen käytöstä Automaattinen skaalaus ja määrittäminen Jos otettu käyttöön:

![ScaleSetPortalService](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalService.PNG)

* Valitse `Summary` sivu, kun vahvistus on valmis, valitse `OK`.

* Valitse lopuksi `Purchase` sivu, valitse `Purchase` Aloita asteikon määrittäminen käyttöönoton.

## <a name="connect-to-a-vm-in-the-scale-set"></a>Yhteyden muodostaminen AM asteikko-määrittäminen

Kun asteikko määrittäminen on otettu käyttöön, siirry `Inbound NAT Rules` kuormituksen oleville mittakaava-välilehti:

![ScaleSetPortalNatRules](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalNatRules.PNG)

Voit muodostaa yhteyden kunkin AM mittakaava, Määritä NAT-sääntöjen avulla. Esimerkiksi Windows asteikko voit määrittää, onko NAT säännön saapuvan porttiin 50000-voi liität RDP kautta, että koneen `<load-balancer-ip-address>:50000`. Linux asteikko-joukko, muodostaa yhteyden komento `ssh -p 50000 <username>@<load-balancer-ip-address>`.

## <a name="next-steps"></a>Seuraavat vaiheet

Saat ohjeet asteikko joukot CLI ottamisesta [näitä ohjeita](./virtual-machine-scale-sets-cli-quick-create.md).

Saat ohjeet asteikko joukot PowerShell ottamisesta [näitä ohjeita](./virtual-machine-scale-sets-windows-create.md).

Saat ohjeet asteikko määrittää Visual Studio ottamisesta [näitä ohjeita](./virtual-machine-scale-sets-vs-create.md).

Yleisiä ohjeita Tutustu [dokumentaatio yhteenvetosivu skaalaus-vaihtoehdon avulla](./virtual-machine-scale-sets-overview.md).

Yleisiä tietoja Tutustu [asteikko joukkojen tärkeimmät aloitussivu](https://azure.microsoft.com/services/virtual-machine-scale-sets/).


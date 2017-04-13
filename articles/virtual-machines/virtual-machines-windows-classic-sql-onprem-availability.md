<properties
    pageTitle="Laajenna paikalliseen aina käytettävyys ryhmittelee Azure | Microsoft Azure"
    description="Tässä opetusohjelmassa käytetään resursseja perinteinen käyttöönotto-mallin avulla luotu ja kerrotaan, miten käyttämällä ohjattua Lisää replikan SQL Server Management Studiossa (SSMS) lisää aina käyttöön käytettävyys ryhmän replikan Azure-tietokannassa."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="07/12/2016"
    ms.author="MikeRayMSFT" />

# <a name="extend-on-premises-always-on-availability-groups-to-azure"></a>Laajenna paikalliseen aina käytettävyys ryhmittelee Azure

Aina käyttöön käytettävyys ryhmät tarjoavat suuren käytettävyyden ryhmien tietokannan lisäämällä toissijainen replikoita. Nämä replikoiden Salli kaatuvat päälle tietokantoja, jos kyseessä on virhe. Lisäksi niiden avulla voidaan purku luku työmääriä tai varmuuskopion tehtävät.

Voit laajentaa paikallisen käytettävyys ryhmät, Microsoft Azure valmistelu vähintään yksi Azure VMs SQL Serverin kanssa ja lisätä ne sitten replikoiden paikallisen käytettävyys-ryhmiin.

Tässä opetusohjelmassa oletetaan, että seuraavasti:

- Aktiivinen Azure-tilaus. Voit [rekisteröityä maksuttoman kokeiluversion käyttäjäksi](https://azure.microsoft.com/pricing/free-trial/).

- Aiemmin luodun aina käyttöön käytettävyys ryhmän paikalliset. Katso lisätietoja käytettävyys ryhmiä, [Aina käyttöön käytettävyys ryhmät](https://msdn.microsoft.com/library/hh510230.aspx).

- Paikallisen verkko- ja Azure virtual verkon väliset yhteydet. Lisätietoja virtual verkoston luomisesta on artikkelissa [määrittäminen sivuston sivuston VPN Azure perinteinen-portaalissa](../vpn-gateway/vpn-gateway-site-to-site-create.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

## <a name="add-azure-replica-wizard"></a>Lisää ohjattu Azure replikointi

Tässä osassa näytetään, miten **Ohjattu: Lisää Azure replikointi** avulla voit laajentaa sisältämään Azure replikoiden aina käyttöön käytettävyys ryhmän ratkaisu.

1. Valitse sisällä SQL Server Management Studiossa Laajenna **Aina käyttöön suuri käytettävyys** > **Käytettävyys ryhmät** > **käytettävyys-ryhmän nimi**.

1. **Käytettävyys replikoiden**hiiren kakkospainikkeella ja valitse sitten **Lisää replikan**.

1. **Käytettävyys ohjatun Lisää replikan** näkyy oletusarvoisesti. Valitse **Seuraava**.  Jos olet valinnut **Älä näytä tämä sivu** vaihtoehto sivun alareunassa edellisen käynnistämisen yhteydessä tämän ohjatun toiminnon, tässä näytössä ei näy.

    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742861.png)

1. Voit tarvitaan muodostaa kaikki olemassa olevat toissijainen replikat. Voit valita **Yhdistä...** vieressä replikoissa tai voit valita **Yhteyden kaikki...** näytön alareunassa. Jälkeen todennusta Valitse **Seuraava** kohtaan seuraavassa näytössä.

1. **Määritä replikoiden** sivulla useita välilehdet näkyvät yläreunassa: **replikoiden**, **päätepisteet**, **Varmuuskopiointi asetukset**ja **Listener**. Valitse **replikoiden** -välilehdessä **Lisää Azure replikan...** Käynnistä ohjattu Azure-replikan luominen.

    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742863.png)

1. Valitse Azure hallinta varmenne paikallisen sertifikaatin Windows-kaupasta, jos olet asentanut eteen. Valitse tai kirjoita Azure tilauksen tunnus, jos olet käyttänyt eteen. Voit valita lataamisen lataaminen ja asentaminen Azure-hallinta-varmenteen ja lataa tilausluettelosta Azure-tilin avulla.

    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742864.png)

1. Voit täyttää kunkin kentän arvoja, joita käytetään luomaan Azure virtuaalikoneen (AM), joka isännöi se sivulle.

  	|Asetus|Kuvaus|
|---|---|
|**Kuva**|Valitse halutut OS ja SQL Server|
|**AM kokoa**|Valitse AM tarpeitasi parhaiten sopiva koko|
|**AM nimi**|Määritä uusi AM yksilöllinen nimi. Nimi on 3 ja 15 merkkiä sisältävät, voi sisältää vain kirjaimia, numeroita ja väliviivoja, on alettava kirjaimella ja perässä kirjaimen tai numeron.|
|**AM käyttäjänimi**|Määritä käyttäjänimi, jotka saattavat muuttua AM järjestelmänvalvoja-tili|
|**AM järjestelmänvalvojan salasana**|Uuden tilin salasanan määrittäminen|
|**Vahvista salasana**|Uuden tilin salasana|
|**VPN**|Määritä Azure virtual verkko, jossa uusi AM tulisi käyttää. Lisätietoja virtual verkot on artikkelissa [Virtual verkon yleiskatsaus](../virtual-network/virtual-networks-overview.md).|
|**VPN aliverkon**|Määritä VPN-aliverkon, uusi AM kannattaa käyttää|
|**Toimialueen**|Vahvista toimialueen täyttää arvo on oikea|
|**Toimialueen käyttäjänimi**|Määritä tili, joka on paikallisen klusterisolmut paikalliseen järjestelmänvalvojaryhmään|
|**Salasana**|Määritä toimialueen käyttäjänimi salasana|

1. Valitse **OK** , jos haluat vahvistaa Käyttöönottoasetukset.

1. Juridiset ehdot näkyvät Seuraava. Lue ja valitse **OK** , jos hyväksyt nämä ehdot.

1. **Määritä replikoiden** -sivu tulee näkyviin uudelleen. Tarkista **replikoiden** **päätepisteet**ja **Varmuuskopiointi-asetukset** -välilehdissä se uuden Azure-asetukset. Muokkaa asetuksia oman yrityksesi tarpeita vastaavan.  Lisätietoja sisälsi seuraavien välilehtien parametrit on artikkelissa [Replikoiden sivun Määritä (käytettävyys ryhmän ohjattu/Lisää replikan ohjattu)](https://msdn.microsoft.com/library/hh213088.aspx). Huomaa, että kuuntelijoita ei voi luoda käytettävyys-ryhmiä, jotka sisältävät Azure replikoiden Listener-välilehdellä. Lisäksi jos kuuntelija on jo luotu ennen ohjatun käynnistämiseen, näyttöön tulee sanoma, joka ilmaisee, että se ei tue Azure. Olemme katsomalla kuuntelijoita luominen **Luo käytettävyys-ryhmän Listener** -osassa.

    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742865.png)

1. Valitse **Seuraava**.

1. Valitse tietojen synkronointi menetelmä, **Valitse ensimmäinen tietojen synkronointi** -sivulla ja valitse **Seuraava**. Useimmat tilanteessa Valitse **Koko tietojen synkronointi**. Lisätietoja tietojen synkronointi menetelmistä on artikkelissa [Valitse tietojen synkronointi aloitussivu (aina käyttöön käytettävyys ryhmän ohjatut toiminnot)](https://msdn.microsoft.com/library/hh231021.aspx).

1. Tarkista tulokset **vahvistus** -sivulla. Korjaa Avoimet kysymykset ja käyttää vahvistus tarvittaessa. Valitse **Seuraava**.

    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742866.png)

1. Tarkista **Yhteenveto** -sivulla ja valitse sitten **Valmis**.

1. Valmistelu käynnistyy. Kun ohjattu toiminto on valmis, valitse **Sulje** Sulje ohjattu toiminto ulos.

>[AZURE.NOTE] Ohjattu Azure-replikan luominen luo lokitiedoston Users\User Name\AppData\Local\SQL Server\AddReplicaWizard. Lokitiedosto voidaan epäonnistui Azure replikan ominaisuuksissa vianmääritys. Jos ohjattu toiminto epäonnistuu, suorittaa toiminnon, kaikki edellisen toiminnot peruutetaan, mukaan lukien poistaminen valmistellun AM.

## <a name="create-an-availability-group-listener"></a>Käytettävyys-ryhmän listener luominen

Käytettävyys-ryhmän luomisen jälkeen sinun on luotava kuuntelija asiakkaiden muodostaa replikoita. Kuuntelijoita suora ensisijaisen tai toissijaisen vain luku-replika saapuvia yhteyksiä. Lisätietoja kuuntelijoita on artikkelissa [määrittäminen ILB-listener aina käyttöön käytettävyys ryhmien Azure-tietokannassa](virtual-machines-windows-classic-ps-sql-int-listener.md).

## <a name="next-steps"></a>Seuraavat vaiheet

Lisäksi **Ohjattu: Lisää Azure replikointi** avulla voit laajentaa aina käyttöön käytettävyys ryhmäsi Azure-voit ehkä myös siirtää joitakin SQL Server-työmääriä kokonaan Azure. Aloita-kohdasta [valmistelu SQL Server Virtual Machine-Azure](virtual-machines-windows-portal-sql-server-provision.md).

Katso muita aiheeseen liittyviä suorittamalla SQL Server Azure VMs:, [SQL Server-Azuren näennäiskoneiden](virtual-machines-windows-sql-server-iaas-overview.md).

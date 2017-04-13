<properties
    pageTitle="SQL Server Azure-Virtuaalikoneissa usein kysytyt kysymykset | Microsoft Azure"
    description="Tässä artikkelissa on vastauksia usein kysyttyihin kysymyksiin käynnissä SQL Server Azure VMs."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="v-shysun"
    manager="felixwu"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/13/2016"
    ms.author="v-shysun"/>

# <a name="sql-server-on-azure-virtual-machines-faq"></a>SQL Server Azure-Virtuaalikoneissa usein kysytyt kysymykset

Tässä ohjeaiheessa on vastauksia joihinkin yleisimmät [SQL](https://azure.microsoft.com/services/virtual-machines/sql-server/)Serveriä-Azuren näennäiskoneiden kysymyksiin.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="frequently-asked-questions"></a>Usein kysytyt kysymykset

1. **Miten voin luoda Azure virtuaalikoneen SQL Serverin kanssa?**

    Voit tehdä tämän kahdella eri tavalla. Helpoin ratkaisu on luotava Virtual Machine, joka sisältää SQL Server. Katso Azure rekisteröityminen ja SQL-AM luominen-portaalista opetusohjelmassa [säännöstä SQL Server-virtual tietokoneeseen Azure-portaalissa](virtual-machines-windows-portal-sql-server-provision.md). Käytössä on myös manuaalisesti SQL Serverin asentamisesta AM ja uudelleen [Käyttöoikeuden Mobility Azure Software Assurance](https://azure.microsoft.com/pricing/license-mobility/)-paikallisen-käyttöoikeus.

1. **Mitä eroa SQL VMs ja SQL-tietokanta-palvelu?**

    Käsitteellisesti käytössä SQL Server Azure virtuaalikoneen ei, joka ei ole käynnissä SQL Server remote palvelinkeskukseen. [SQL-tietokanta](../sql-database/sql-database-technical-overview.md) on sen sijaan tietokanta nimellä--palvelun. SQL-tietokanta, jossa ei ole koneet, jotka isännöivät tietokantoja käyttöoikeus. Katso täysi vertailu [pilvestä SQL Server-vaihtoehdon valitseminen: SQL Azure (PaaS)-tietokanta tai SQL Server Azure VMs (IaaS)](../sql-database/sql-database-paas-vs-sql-server-iaas.md).

1. **Miten oma paikallisen SQL Server-tietokantaan voit siirtää pilveen?**

    Luo ensin Azure virtuaalikoneen SQL Server-esiintymän kanssa. Tämän jälkeen siirtää paikallisen tietokantoja kyseisen esiintymän. Katso tietoja siirron strategioita [siirtäminen SQL Server Azure-AM, SQL Server-tietokantaan](virtual-machines-windows-migrate-sql.md).

2. **Voit muuttaa asennetut ominaisuudet tai toinen SQL Server-esiintymän asentaminen samaan AM?**

    Kyllä. SQL Serverin asennustietovälineen sijaitsee **C** -asema sijaintiin. Suorita **Setup.exe** kyseisestä sijainnista lisäämään uusia SQL Server-esiintymiä tai muuttaa tietokoneeseen SQL Serverin muut asennetut ominaisuudet.

3. **Miten uusi versio/versioksi SQL Server Azure-AM päivitetään?**

    Ei tällä hetkellä ei ole paikallaan tapahtuva päivitys SQL Server Azure-AM käynnissä. Luo uuden Azure virtual koneen haluamasi SQL Serverin versio/Edition ja siirtämällä sitten tietokantoja uuteen palvelimeen Vakio [tietojen siirron avulla](virtual-machines-windows-migrate-sql.md).

4. **Miten voin asentaa SQL Server-käyttöoikeus versioni-Azure-AM?**

    Kopioi SQL Serverin asennustietovälineen Windows Server AM ja asenna SQL Server AM. Käyttöoikeuksien syitä, jos sinulla on [Käyttöoikeus Mobility Azure Software Assurance](https://azure.microsoft.com/pricing/license-mobility/).

5. **Onko sinulla maksaa AM SQL-kustannukset, jos sitä käytetään vain valmius/automaattisesti?**

    Jos luot SQL-AM käyttää valikoimaa, sinun on oltava erillistä lisenssiä valmiustilassa SQL-AM ja hinnoittelua on sama. Jos asennat SQL Serverin manuaalisesti virtual tietokoneessa, jossa [Käyttöoikeus Mobility](https://azure.microsoft.com/pricing/license-mobility/), voit halutessasi on yksi vapaa passiivinen SQL esiintymä automaattisesti. Tarkista rajoituksista ja vaatimuksista.

6. **Miten päivitykset ja service Pack-paketit käytetään SQL Server-AM?**

    Voit hallita host koneen, mukaan lukien milloin ja miten päivitysten asentamisen näennäiskoneiden antaa. Käyttöjärjestelmä voit käyttää windows-päivitykset manuaalisesti tai voit ottaa ajoituspalvelu, jota kutsutaan [Automaattinen korjaaminen](virtual-machines-windows-classic-sql-automated-patching.md). Automaattinen korjaus asentaa päivityksiä, jotka on merkitty tärkeää, kuten SQL Server-päivitykset kyseiseen luokkaan. Muiden valinnaisten päivitysten SQL Server on oltava asennettuna manuaalisesti.

7. **Onko se mahdollista määrittää määrityksiä ei näy virtuaalikoneen valikoimassa (esimerkiksi Windows 2008 R2 + SQL Server 2012)?**

    Ei. Virtuaalikoneen valikoiman kuvia, jotka sisältävät SQL Server-valittava jokin annettu kuvista.

9. **Miten voin asentaa SQL Datatyökalut-Azure-AM?**

    Lataa ja asenna SQL Datatyökalut [Microsoft SQL Server Data Tools – Business Intelligence for Visual Studio 2013 ](https://www.microsoft.com/en-us/download/details.aspx?id=42313).

## <a name="resources"></a>Resurssit

Yleiskatsaus SQL Server-Azuren näennäiskoneiden Katso video [Azure AM on paras ympäristö SQL Server 2016](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/Azure-VM-is-the-best-platform-for-SQL-Server-2016). Saat myös hyvä johdanto ohjeaiheessa [Yhteenvedossa Azuren näennäiskoneiden SQL Server](virtual-machines-windows-sql-server-iaas-overview.md).

Muita resursseja:

- [Valmistele SQL Server-virtual tietokoneeseen Azure-portaalissa](virtual-machines-windows-portal-sql-server-provision.md)
- [Tietokannan siirtäminen SQL Server Azure AM](virtual-machines-windows-migrate-sql.md)
- [Suuri käytettävyys ja SQL Server Azure-Virtuaalikoneissa palauttaminen](virtual-machines-windows-sql-high-availability-dr.md)
- [Suorituskyvyn SQL Server Azuren näennäiskoneiden parhaat käytännöt](virtual-machines-windows-sql-performance.md)
- [Sovelluksen kuvioiden ja Development strategioita Azuren näennäiskoneiden SQL Server](virtual-machines-windows-sql-server-app-patterns-dev-strategies.md)

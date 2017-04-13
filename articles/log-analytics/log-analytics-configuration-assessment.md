<properties
    pageTitle="Määritysten arviointi ratkaisu Log Analytics | Microsoft Azure"
    description="Lokitiedoston Analytics kokoonpanon arviointi-ratkaisun on Operations Manager tekijöiden tai Operations Manager-hallinta-ryhmässä System Center Operations Manager server-julkaisuinfrastruktuuri nykyisen tilan yksityiskohtaisia tietoja."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="configuration-assessment-solution-in-log-analytics"></a>Lokitiedoston Analytics määritys arviointi ratkaisu

Lokitiedoston Analytics kokoonpanon arviointi-ratkaisun auttaa löytämään mahdolliset määritysongelmia, ilmoituksia ja knowledge suosituksia kautta.

Tämä ratkaisu edellyttää System Center Operations Manager. Kokoonpanon arviointi ei ole käytettävissä, jos käytössäsi on vain suoraan yhdistettyjä agenttien vuoksi.

Silverlight-laajennuksen joidenkin tietojen tarkasteleminen kokoonpanon arviointi ratkaisu edellyttää selaimen.

>[AZURE.NOTE] Alku 5 heinäkuussa 2016, kokoonpanon arviointi-ratkaisun voidaan lisätä enää Log Analytics-työtilojen ja tämä ratkaisu ei enää olemassa olevan käyttäjien saatavilla jälkeen 2016 1 elokuun. Käyttämällä tätä ratkaisua SQL Serverin tai Active Directory-asiakkaiden on suositeltavaa, voit käyttää sen sijaan, [SQL Server arviointi](log-analytics-sql-assessment.md), [Active Directory-arviointi](log-analytics-ad-assessment.md) ja [Active Directory-replikoinnin tila](log-analytics-ad-replication-status.md) ratkaisuja. Käytössä on Windows-kokoonpanon arviointi Hyper-V ja System Center virtuaalikoneen hallinta, suosittelemme tapahtuman kokoelman ja muutosten seuranta ja myönnä ongelmat sisällä oman ympäristöissä kokonaisvaltaiseksi näkymän ominaisuudet.

![Määritysten arviointi-ruutu](./media/log-analytics-configuration-assessment/oms-config-assess-tile.png)

Määritystietoja valvottu palvelinten lokitietoihin ja sitten lähetetään OMS palveluun pilveen käsittelyä varten. Logiikan käytetään vastaanotetut tiedot ja pilvipalvelussa tietueiden tietoja. Käsitellyt tiedot palvelimia näkyy seuraavilla alueilla:

- **Ilmoitukset:** Näyttää määritysten liittyvät, ennakoiva ilmoituksia, jotka on tehty korotettuna valvottu palvelimia. Nämä on valmistettu tekijä Microsoft Customer ja parhaita käytäntöjä kentästä organisaation tuki (CSS) sääntöjä.
- **Knowledge suosituksia:** Näyttää, on suositeltavaa toiminnoista, jotka löytyvät infrastruktuurin; Microsoft Knowledge Base-artikkeleista Nämä ohjelma ehdottaa automaattisesti tietokoneen learning käyttämisessä käytön kokoonpanosi.
- **Palvelimia ja analysoida toiminnoista:** Näyttää palvelimet ja toiminnoista, jotka seurataan OMS mukaan.

![Määritysten arviointi Raporttinäkymät-ikkunan](./media/log-analytics-configuration-assessment/oms-config-assess-dash01.png)

### <a name="technologies-you-can-analyze-with-configuration-assessment"></a>Voit analysoida määritysten Assessment tekniikat

OMS kokoonpanon arviointi analysoi seuraavista toiminnoista:

- Windows Server 2012 ja Microsoft Hyper-V Server 2012
- Windows Server 2008: n ja Windows Server 2008 R2, mukaan lukien:
    - Active Directory
    - Hyper-V Host (isäntä)
    - Yleiset käyttöjärjestelmä
- SQL Server 2008 ja uudempi versio
    - SQL Server-tietokantamoduuli
- Microsoft SharePoint 2010: ssä
- Microsoft Exchange Server 2010: n ja Microsoft Exchange Server 2013: ssa
- Microsoft Lync Server 2013: n ja Lync Server 2010
- System Center 2012 SP1 – virtuaalikoneen hallinta

SQL Server analysis tuetaan seuraavat 32-bittinen ja 64-bittiset versiot:

- SQL Server 2016 - kaikki versiot
- SQL Server 2014 - kaikki versiot
- SQL Server 2008: n ja 2008 R2 - kaikki versiot

SQL Server-tietokantamoduuli analysoidaan kaikki tuetut versioihin. 32-bittinen versio SQL Serverin lisäksi tuetaan, kun WOW64 käyttöönoton käynnissä.

## <a name="installing-and-configuring-the-solution"></a>Asentaminen ja määrittäminen ratkaisu
Seuraavat tiedot avulla voit asentaa ja määrittää ratkaisu.

- Operations Manager vaaditaan kokoonpanon arviointi-ratkaisun.
- Tarvitset Operations Manager agentti käyttöön kussakin tietokoneessa, johon haluat arvioida sen asetukset.
- Lisää kokoonpanon arviointi-ratkaisun OMS työtilan käyttämisestä [ratkaisuvalikoimasta Lisää Log Analytics-ratkaisuja](log-analytics-add-solutions.md)kuvatut toimet.  Ei ole tarvitaan lisämäärityksiä.

## <a name="configuration-assessment-data-collection-details"></a>Määritysten arvioinnin sivustokokoelman tietojen

Kokoonpanon arviointi kerää kokoonpanotietoja, metatietojen ja tila-tiedot, jotka on otettu käyttöön tekijöiden avulla.

Seuraavassa taulukossa on tietoja sivustokokoelman menetelmistä ja muita tietoja siitä, miten tiedot kerätään kokoonpanon arviointi.

| käyttöympäristö | Suora agentti | SCOM agentti | Azure-tallennustilan | SCOM tarvitaan? | SCOM agentti tietojen lähetetyissä kutsuissa hallinta-ryhmä | sivustokokoelman korkojakso |
|---|---|---|---|---|---|---|
|Windows|![Ei](./media/log-analytics-configuration-assessment/oms-bullet-red.png)|![Kyllä](./media/log-analytics-configuration-assessment/oms-bullet-green.png)|![Ei](./media/log-analytics-configuration-assessment/oms-bullet-red.png)|            ![Kyllä](./media/log-analytics-configuration-assessment/oms-bullet-green.png)|![Kyllä](./media/log-analytics-configuration-assessment/oms-bullet-green.png)| kahdesti päivässä|

Seuraavassa taulukossa on esimerkkejä tietojen keräämistä kokoonpanon arviointi:

|**Tietotyyppi**|**Kentät**|
|---|---|
|Määritys|Asiakastunnus, AgentID, tunnus, ManagedTypeID, ManagedTypePropertyID, CurrentValue, ChangeDate|
|Metatietojen|BaseManagedEntityId, ObjectStatus, OrganizationalUnit, ActiveDirectoryObjectSid, PhysicalProcessors, NetworkName, IP-osoite, ForestDNSName, NetbiosComputerName, VirtualMachineName, LastInventoryDate, HostServerNameIsVirtualMachine, IP-osoite, NetbiosDomainName, LogicalProcessors, DNSName, näyttönimi, DomainDnsName, ActiveDirectorySite, PrincipalName, OffsetInMinuteFromGreenwichTime|
|Tila|StateChangeEventId, StateId, NewHealthState, OldHealthState, kontekstissa, TimeGenerated, TimeAdded, StateId2, BaseManagedEntityId, MonitorId, HealthState, LastModified, LastGreenAlertGenerated, DatabaseTimeModified|

## <a name="configuration-assessment-alerts"></a>Määritysten arviointi ilmoitukset
Voit tarkastella ja hallita ilmoituksia kokoonpanon arviointi ja ilmoitukset-sivu. Ilmoitukset ilmoita, joka on havaittu ongelma ja miten voit ratkaista ongelman syy. Ne on myös tietoa määritysasetukset ympäristössä, joka voi aiheuttaa suorituskykyongelmia.

![Näytä ilmoitukset](./media/log-analytics-configuration-assessment/oms-config-assess-alerts01.png)

>[AZURE.NOTE] Kokoonpanon arviointi-ilmoitukset ovat erilaisia kuin lokin Analytics muut ilmoitukset. Silverlight-laajennuksen tarkasteleminen ilmoitusten tarvitaan selaimen.

Kun olet valinnut kohteen tai luokan kohteiden ilmoitukset-sivulla, näet luettelon palvelimia tai työmääriä ilmoituksia, jotka koskevat kunkin kohteen kanssa.

![ilmoitusten luettelo](./media/log-analytics-configuration-assessment/oms-config-assess-alerts-view-config.png)

Kunkin ilmoitus sisältää linkin artikkeliin Microsoft Knowledge Base-tietokannassa. Seuraavissa artikkeleissa on lisätietoja ilmoituksen.

>[AZURE.TIP] Oletusarvon mukaan näytettäviä ilmoituksia enimmäismäärä on 2 000. Voit tarkastella ilmoitusten valitsemalla ilmoituspalkissa ilmoitukset-luettelon yläpuolella.

Voit valita minkä tahansa kohteen Knowledge Base-artikkelin, jotka voivat auttaa ratkaisemaan ongelman, joka on luotu ilmoituksen syy-luettelosta.

![Knowledge Base-artikkeli](./media/log-analytics-configuration-assessment/oms-config-assess-alerts-details-kb.png)

Voit hallita ilmoitusten säännöt ohittamaan tiettyjä sääntöjä tai luokkien säännöt.

![ilmoitusten sääntöjen hallinta](./media/log-analytics-configuration-assessment/oms-config-assess-alert-rules.png)

## <a name="knowledge-recommendations"></a>Knowledge suositukset
Kun tarkastelet knowledge suositukset, on jaettu log hakutulosten luettelo Microsoft KB artikkeleita suositus toiminnoista ja tietokoneissa, joissa on lisätietoja ilmoituksen.

![hakutulosten knowledge suositukset](./media/log-analytics-configuration-assessment/oms-config-assess-knowledge-recommendations.png)

## <a name="servers-and-workloads-analyzed"></a>Palvelinten ja analysoida toiminnoista
Kun tarkastelet knowledge suositukset, on jaettu log hakutulosten palvelimia ja toiminnoista, jotka tunnetusti OMS Operations Manager.

![Palvelinten ja toiminnoista](./media/log-analytics-configuration-assessment/oms-config-assess-servers-workloads.png)

## <a name="next-steps"></a>Seuraavat vaiheet

- [Log rajaamalla Log Analytics](log-analytics-log-searches.md) avulla voit tarkastella määrityksistä arvioinnin tietoja.

<properties
    pageTitle="SQL Server-liiketoimintatietojen | Microsoft Azure"
    description="Tässä aiheessa käytetään resursseja perinteinen käyttöönotto-mallin avulla luotu ja kuvataan käytettävissä Business Intelligence (BI)-ominaisuudet käyttöön Azuren näennäiskoneiden (VMs) SQL Serveriä."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="guyinacube"
    manager="erikre"
    editor="monicar"
    tags="azure-service-management"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/04/2016"
    ms.author="asaxton" />

# <a name="sql-server-business-intelligence-in-azure-virtual-machines"></a>Azuren näennäiskoneiden SQL Server liiketoimintatiedot

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

Microsoft Azure Virtual Machine-valikoima sisältää kuvia, jotka sisältävät SQL Serverissä. SQL Server-versioiden tukemat valikoiman kuvia tiedostot ovat samat asennuksen paikallisen tietokoneisiin ja näennäiskoneiden asennusta. Tässä ohjeaiheessa on yhteenveto SQL Serverin Business Intelligence (BI) ominaisuudet asennettuihin kuvat ja määritysvaiheet vaaditaan, kun virtual machine on valmisteltu. Tässä ohjeaiheessa kerrotaan myös tuettu käyttöönotto topologioissa Liiketoimintatieto-ominaisuuksia ja parhaita käytäntöjä.

## <a name="license-considerations"></a>Käyttöoikeuden huomioon otettavia seikkoja

SQL Server-Microsoft Azuren näennäiskoneiden-käyttöoikeutta kahdella tavalla:

1. Käyttöoikeuden mobility edut, jotka ovat osa SA-ylläpito. Lisätietoja on artikkelissa [Käyttöoikeuden Mobility Azure Software Assurance](https://azure.microsoft.com/pricing/license-mobility/).

1. Maksaa kohti tuntihinta, Azuren näennäiskoneiden SQL Server on asennettu. On "SQL Server-kohdassa [Näennäiskoneiden hinnat](https://azure.microsoft.com/pricing/details/virtual-machines/#Sql).

Lisätietoja käyttöoikeuksien myöntämistä ja on artikkelissa [Näennäiskoneiden käyttöoikeudet usein kysytyt kysymykset](https://azure.microsoft.com/pricing/licensing-faq/%20/).

## <a name="sql-server-images-available-in-azure-virtual-machine-gallery"></a>SQL Server käytettävissä olevat kuvat Azure Virtual Machine-valikoima

Microsoft Azure Virtual Machine-valikoima sisältää useita kuvia, jotka sisältävät Microsoft SQL Server. Asennettuna virtuaalikoneen kuvat ohjelmistot vaihtelee käyttöjärjestelmän versio ja SQL Server-version. Azure virtuaalikoneen valikoimassa käytettävissä olevat kuvat luettelo muuttuu usein.

![SQL azure AM-valikoiman kuva](./media/virtual-machines-windows-classic-ps-sql-bi/IC741367.png)

![PowerShellin](./media/virtual-machines-windows-classic-ps-sql-bi/IC660119.gif) Seuraavaa PowerShell-komentosarjaa palauttaa Azure kuvia, jotka sisältävät "SQL-palvelin" ImageName-luettelossa:

    # assumes you have already uploaded a management certificate to your Microsoft Azure Subscription. View the thumbprint value from the "settings" menu in Azure classic portal.

    $subscriptionID = ""    # REQUIRED: Provide your subscription ID.
    $subscriptionName = "" # REQUIRED: Provide your subscription name.
    $thumbPrint = "" # REQUIRED: Provide your certificate thumbprint.
    $certificate = Get-Item cert:\currentuser\my\$thumbPrint # REQUIRED: If your certificate is in a different store, provide it here.-Ser  store is the one specified with the -ss parameter on MakeCert

    Set-AzureSubscription -SubscriptionName $subscriptionName -Certificate $certificate -SubscriptionID $subscriptionID

    Write-Host -foregroundcolor green "List of available gallery images where imagename contains 2016"
    Write-Host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
    get-azurevmimage | where {$_.ImageName -Like "*SQL-Server-2016*"} | select imagename,category, location, label, description

    Write-Host -foregroundcolor green "List of available gallery images where imagename contains 2014"
    Write-Host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
    get-azurevmimage | where {$_.ImageName -Like "*SQL-Server-2014*"} | select imagename,category, location, label, description

Lisätietoja versiot ja SQL Server tukemista toiminnoista on seuraavissa artikkeleissa:

- [SQL Server-versioissa](https://www.microsoft.com/server-cloud/products/sql-server-editions/#fbid=Zae0-E6r5oh)

- [SQL Server 2016-versioiden tukemat ominaisuudet](https://msdn.microsoft.com/library/cc645993.aspx)

### <a name="bi-features-installed-on-the-sql-server-virtual-machine-gallery-images"></a>SQL Server Virtual Machine-valikoiman kuvia asennettuihin liiketoimintatiedot

Seuraavaan taulukkoon on koottu yleisiä Microsoft Azure Virtual Machine-valikoiman kuvia asennetaan SQL Serverin yritystieto-ominaisuuksien"

- SQL Server 2016 RC3

- SQL Server 2014 SP1 Enterprise

- SQL Server 2014 SP1 Standard

- SQL Server 2012 SP2: n Enterprise

- SQL Server 2012 SP2 Standard

|SQL Server BI-toiminto|Asennettuihin valikoiman kuva|Huomautuksia|
|---|---|---|
|**Reporting Services-alkuperäistilassa**|Kyllä|Asennettu, mutta vaatii konfigurointi, mukaan lukien raportin hallinnan URL-osoite. Kohdassa [Määritä Reporting Services](#configure-reporting-services).|
|**Reporting Services SharePoint-tilassa**|Ei|Microsoft Azure Virtual Machine-valikoiman kuva ei ole SharePoint- tai SharePoint asennustiedostot. <sup>1</sup>|
|**Analysis Servicesin moniulotteisista ja tietojen louhinta (OLAP)**|Kyllä|Asennettu ja määritetty Analysis Services-oletusesiintymää|
|**Analysis Servicesin taulukkomuotoisen**|Ei|Tuetut SQL Server 2012-2014 2016 kuvia, mutta se ei ole asennettu oletusarvoisesti. Asenna toinen Analysis Services-esiintymän. Katso kohta Asenna tämän artikkelin muita SQL Server Services ja ominaisuuksia.|
|**Analysis Services-PowerPivot for SharePoint**|Ei|Microsoft Azure Virtual Machine-valikoiman kuva ei ole SharePoint- tai SharePoint asennustiedostot. <sup>1</sup>|

<sup>1</sup> lisätietoja SharePointissa ja Azuren näennäiskoneiden on artikkelissa [Microsoft Azure arkkitehtuureihin SharePoint 2013: n](https://technet.microsoft.com/library/dn635309.aspx) ja [SharePoint-käyttö käyttöön Microsoft Azuren näennäiskoneiden](https://www.microsoft.com/download/details.aspx?id=34598).

![PowerShellin](./media/virtual-machines-windows-classic-ps-sql-bi/IC660119.gif) Suorittaa seuraavan PowerShell-komennon Asennetut palvelut, jotka sisältävät "SQL-palvelunimi luettelo.

    get-service | Where-Object{ $_.DisplayName -like '*SQL*' } | Select DisplayName, status, servicetype, dependentservices | format-Table -AutoSize

## <a name="general-recommendations-and-best-practices"></a>Yleiset suosituksia ja parhaita käytäntöjä

- Virtual machine Suositeltu vähimmäiskoko on **A3** , kun SQL Server Enterprise-versioon. SQL Server BI-versioiden Analysis Services ja Reporting Servicesin suositellaan **A4** virtuaalikoneen kokoa.

    Nykyiset AM koot tietoja on artikkelissa [Azure virtuaalikoneen koot](virtual-machines-linux-sizes.md).

- Levynhallinnan parhaat käytännöt kannattaa tallentaa tiedot, loki ja asemat kuin **C**varmuuskopiot: ja **D**:. Esimerkiksi luoda tietojen levyjen **E**: ja **F**:.

    - Välimuistin oletusarvon **C**-asema käytännön asema: ei ole paras mahdollinen tietojen käsitteleminen.

    - **D**: asema on väliaikainen asema, jota käytetään ensisijaisesti sivutustiedosto. **D**: asema ei säilytetä ja Blob-objektien tallennustilaan ei tallenneta. Hallinnan tehtäviä, kuten virtuaalikoneen koon muuttaminen Palauta **D**: asema. On suositeltavaa **ei** Käytä **D**: asema tietokantatiedostojen, mukaan lukien tempdb.

    Katso lisätietoja luomisesta ja liittämisestä levyjä, [liittäminen tietojen DVD-levyllä Virtual koneeseen](virtual-machines-windows-classic-attach-disk.md).

- Pysäytä tai poista et aio käyttää palveluja. Esimerkki Jos virtuaalikoneen käytetään vain Reporting Services lopettaminen tai poista Analysis Services ja SQL Server-Integrointipalveluilla. Seuraavassa kuvassa on esimerkki palvelut, jotka on aloitettu oletusarvoisesti.

    ![SQL Server-palvelut](./media/virtual-machines-windows-classic-ps-sql-bi/IC650107.gif)

    >[AZURE.NOTE] SQL Server-tietokantamoduuli tarvitaan tuetut BI-tilanteet. Yhden palvelimen AM topologian tietokantamoduuli tarvitaan on käytössä sama AM.

    Lisätietoja on kohdassa: [Poista Reporting Services](https://msdn.microsoft.com/library/hh479745.aspx) - ja [Poista Analysis Services esiintymä](https://msdn.microsoft.com/library/ms143687.aspx).

- Tarkista **Windows Update** uusi "tärkeitä päivityksiä". Microsoft Azure Virtual Machine kuvat usein päivitetään; tärkeitä päivityksiä voi kuitenkin ovat käytettävissä **Windows** Update, kun AM-kuva on viimeksi päivitetty.

## <a name="example-deployment-topologies"></a>Esimerkki käyttöönoton topologioissa

Seuraavassa on esimerkki asennuksia, jotka käyttävät Microsoft Azuren näennäiskoneiden. Nämä kaavioissa sijoitteluja ovat vain tietyt mahdollista topologioissa, voit käyttää SQL Server BI-ominaisuuksia ja Microsoft Azuren näennäiskoneiden.

### <a name="single-virtual-machine"></a>Yksittäisen Virtual Machine

Analysis Services, Reporting Services, SQL Server-tietokantamoduuli, ja tietolähteiden virtual yhteen tietokoneeseen.

![1 virtuaalikoneen BI IAS skenaarion](./media/virtual-machines-windows-classic-ps-sql-bi/IC650108.gif)

### <a name="two-virtual-machines"></a>Kaksi näennäiskoneiden

- Analysis Services, Reporting Services ja SQL Server-tietokantamoduuli virtual yhteen tietokoneeseen. Käyttöönottoon sisältää raportin server-tietokannat.

- Toinen AM tietolähteiden. Toinen AM sisältää SQL Server-tietokantamoduuli tietolähteenä.

![2 näennäiskoneiden skenaarion BI iaas](./media/virtual-machines-windows-classic-ps-sql-bi/IC650109.gif)

### <a name="mixed-azure--data-on-azure-sql-database"></a>Erilaiset Azure – tietojen Azure SQL-tietokantaan

- Analysis Services, Reporting Services ja SQL Server-tietokantamoduuli virtual yhteen tietokoneeseen. Käyttöönottoon sisältää raportin server-tietokannat.

- Tietolähde on Azure SQL-tietokantaan.

![BI iaas skenaariot AM ja AzureSQL tietolähteenä](./media/virtual-machines-windows-classic-ps-sql-bi/IC650110.gif)

### <a name="hybrid-data-on-premises"></a>Hybrid – tietojen paikalliseen

- Esimerkki käyttöönoton Analysis Services-Reporting Services ja SQL Server-tietokantamoduuli suorittaa yhteen virtual tietokoneeseen. Virtuaalikoneen isännöi raportin server-tietokannat. Virtuaalikoneen liitetään paikallisen-toimialueen Azure Virtual verkon kautta tai joitakin muita VPN-tunnelointia ratkaisu.

- Tietolähde on paikallisen.

![BI iaas skenaariot AM ja paikalliseen tietolähteet](./media/virtual-machines-windows-classic-ps-sql-bi/IC654384.gif)

## <a name="reporting-services-native-mode-configuration"></a>Reporting Services-alkuperäistilassa kokoonpanon määritys

SQL Server virtuaalikoneen-valikoiman kuva sisältää Reporting Services alkuperäistilassa asennettu, mutta raportin-palvelinta ei ole määritetty. Tämän osan vaiheet määrittäminen Reporting Services-raporttipalvelin. Katso tarkempia tietoja määrittäminen Reporting Services alkuperäiseen tilaan, [Asenna Reporting Services alkuperäisen tilassa raportin Server (SSRS)](https://msdn.microsoft.com/library/ms143711.aspx).

>[AZURE.NOTE] Samanlaista sisältöä, joka käyttää Windows PowerShell-komentosarjojen määrittäminen raporttipalvelimessa on kohdassa [PowerShellin luominen Azure AM kanssa alkuperäisen tilassa raporttipalvelin](virtual-machines-windows-classic-ps-sql-report.md).

### <a name="connect-to-the-virtual-machine-and-start-the-reporting-services-configuration-manager"></a>Yhteyden muodostaminen virtuaalikoneen ja aloita Reporting Services-määritysten hallinta

On kaksi yleisiä työnkulut Azure virtuaalikoneen yhteyden:

- Jos haluat muodostaa, valitse virtuaalikoneen nimi ja valitse sitten **Yhdistä**. Etätyöpöytäyhteys avautuu ja tietokonenimi täytetään automaattisesti.

    ![yhteyden muodostaminen azure virtuaalikoneen](./media/virtual-machines-windows-classic-ps-sql-bi/IC650112.gif)

- Muodosta yhteys virtuaalikoneen Windows Etätyöpöytäyhteys kanssa. Etätyöpöytä käyttöliittymässä:

    1. Kirjoita **cloud palvelunimi** kuin tietokonenimi.

    1. Kirjoita kaksoispiste (:) ja julkisen porttinumero, joka on määritetty TCP remote työpöydän päätepisteelle.

        Myservice.cloudapp.NET:63133

        Lisätietoja on artikkelissa [pilvipalveluun ominaisuudet?](https://azure.microsoft.com/manage/services/cloud-services/what-is-a-cloud-service/).

**Aloita Reporting Services-määritysten hallinta.**

1. **Windows Server 2012**:

1. Kirjoita **aloitusnäytön-** sovellusten luettelo **Reporting Services** .

1. Napsauta **Reporting Services-määritysten hallinta** ja valitse **Suorita järjestelmänvalvojana**.

1. **Windows Server 2008 R2**:

1. Napsauta **Käynnistä-painiketta**ja valitse sitten **Kaikki ohjelmat**.

1. Valitse **Microsoft SQL Server 2016**.

1. Valitse **määritystyökalut**.

1. Napsauta **Reporting Services-määritysten hallinta** ja valitse **Suorita järjestelmänvalvojana**.

Tai

1. Napsauta **Käynnistä-painiketta**.

1. Kirjoita **Hae ohjelmista ja tiedostoista** -valintaikkunassa **reporting Servicesin**. Jos AM on käynnissä Windows Server 2012, kirjoita **reporting Servicesin** Windows Server 2012: n aloitusnäyttöön.

1. Napsauta **Reporting Services-määritysten hallinta** ja valitse **Suorita järjestelmänvalvojana**.

    ![Etsi ssrs määritysten hallinta](./media/virtual-machines-windows-classic-ps-sql-bi/IC650113.gif)

### <a name="configure-reporting-services"></a>Reporting Services-palvelujen määrittäminen

**Palvelutili ja WWW-palvelun URL-osoite:**

1. Tarkista **Palvelimen nimi** on paikallinen palvelimen nimi ja valitse **Yhdistä**.

1. Tyhjä **Raportti Server-tietokannan nimi**muistiin. Tietokanta on luotu, kun määritykset on valmis.

1. Tarkista **Raportin palvelimen tila** on **Aloitettu**. Jos haluat tarkistaa palvelu Windows Server Manager-palvelu on **SQL Server Reporting Services** -Windows-palvelun.

1. Valitse **Palvelutilin** ja muuta tarpeen mukaan tili. Jos muu kuin toimialueen liitettyjen ympäristössä käytetään virtuaalikoneen valmiin **raporttipalvelimen** tili on riittävä. Saat lisätietoja palvelutili [Palvelutilin](https://msdn.microsoft.com/library/ms189964.aspx).

1. Valitse vasemmanpuoleisessa ruudussa **WWW-palvelun URL** .

1. Valitse **Käytä** määrittäminen oletusarvot.

1. Huomautus **raportin Server Web palvelun URL-osoitteita**. Huomaa, että TCP oletusportti on 80 ja kuuluu URL-osoite. Myöhemmin Luo Microsoft Azure virtuaalikoneen päätepiste portin.

1. Tarkista **tulokset** -ruudussa onnistui toiminnot.

**Tietokanta:**

1. Valitse vasemmanpuoleisessa ruudussa **tietokanta** .

1. Valitse **Muuta tietokanta**.

1. **Luo uusi raportti-server-tietokanta** on valittuna ja valitse sitten Seuraava.

1. Tarkista **Palvelimen nimi** ja valitse **Testaa yhteyttä**.

1. Jos tulos on **Testiyhteys onnistui**, valitse **OK** ja valitse sitten **Seuraava**.

1. Huomautus tietokannan nimi on **raporttipalvelimen** ja **raporttipalvelimen tila** on **alkuperäisen** ja valitse sitten **Seuraava**.

1. Valitse **tunnistetiedot** -sivulla **Seuraava** .

1. Valitse **Yhteenveto** -sivulla **Seuraava** .

1. Valitse **edistymistä ja valmis** -sivulla **Seuraava** .

**Web-portaalin URL-osoite tai Report Managerin URL 2012 ja 2014:**

1. Valitse **Web-portaalin URL-osoite**tai **Report Managerin URL** 2014 ja 2012 vasemmanpuoleisessa ruudussa.

1. Valitse **Käytä**.

1. Tarkista **tulokset** -ruudussa onnistui toiminnot.

1. Valitse **Lopeta**.

Lisätietoja raportin palvelimen käyttöoikeuksien on artikkelissa [Alkuperäisen tilassa raporttipalvelimen myöntää käyttöoikeuksia](https://msdn.microsoft.com/library/ms156014.aspx).

### <a name="browse-to-the-local-report-manager"></a>Selaa, paikallinen Report Managerin

Vahvista määritykset, siirry raportin hallinnassa AM.

1. Käynnistä Internet Explorer AM järjestelmänvalvojan oikeuksilla.

1. Selaa-AM http://localhost/reports.

### <a name="to-connect-to-remote-web-portal-or-report-manager-for-2014-and-2012"></a>Yhdistä etäsivuston portal tai Report Managerin 2014 ja 2012: n avulla

Jos haluat muodostaa yhteyden web-portaalin tai Report Managerin 2014 ja 2012 virtuaalikoneen etätietokoneista, luo uuden virtual tietokoneen TCP-päätepistettä. Oletusarvon mukaan raporttipalvelimen seuraa pyyntöjen porttiin **80**varten. Jos määrität raportin palvelimen URL-osoitteet eri porttia, porttinumero on määritettävä seuraavat ohjeet.

1. Luo päätepisteen porttinumeroa 80 virtuaalikoneen. Lisätietoja on artikkelissa, tässä asiakirjassa [virtuaalikoneen päätepisteet ja palomuurin porttien](#virtual-machine-endpoints-and-firewall-ports) -osassa.

1. Avaa portti 80 virtual machine palomuuri.

1. Etsi web-portaalin tai report Managerin käyttäen Azure virtuaalikoneen **DNS-nimen** URL-osoite palvelimen nimi. Esimerkki:

    **Raporttipalvelimen**: http://uebi.cloudapp.net/reportserver  **Web-portaaliin**: http://uebi.cloudapp.net/reports

    [Raportin Serverin käytön palomuurin määrittäminen](https://msdn.microsoft.com/library/bb934283.aspx)

### <a name="to-create-and-publish-reports-to-the-azure-virtual-machine"></a>Voit luoda ja julkaista raportteja Azure virtuaalikoneen

Seuraavassa taulukossa on yhteenveto aiemmin raportit paikallisen tietokoneen isännöidään käyttöön Microsoft Azure virtuaalikoneen raporttipalvelimen saatavilla olevista vaihtoehdoista:

- **Raportin muodostin**: virtuaalikoneen sisältää valitsemalla-kerran version Microsoft SQL Server Report Builder 2012 ja SQL-2014. Aloittaa virtuaalikoneen ja SQL-2016 raportin muodostin ensimmäisen kerran:

    1. Käynnistä selain järjestelmänvalvojan oikeuksilla.

    1. Etsi web-portaalin virtuaalikoneen, ja valitsemalla **Lataa** -kuvakkeen oikeassa yläkulmassa.
    
    1. Valitse **raportin muodostin**.

    Lisätietoja on artikkelissa [Käynnistä raportin muodostin](https://msdn.microsoft.com/library/ms159221.aspx).

- **SQL Server Data Tools**: AM: SQL Server Data Tools on asennettu virtuaalikoneen ja voidaan virtuaalikoneen **Raportin palvelimen projektien** ja raporttien luomiseen. SQL Server Data Tools julkaista raportit virtuaalikoneen raportin palvelimeen.

- **SQL Server Data Tools: Remote**: paikallisessa tietokoneessa Luo Reporting Services-projekti SQL Server Data Tools, joka sisältää Reporting Services-raportteja. Määritä muodostaa WWW-palvelun URL-projekti.

    ![SSRS projektin SSDT projektin ominaisuudet](./media/virtual-machines-windows-classic-ps-sql-bi/IC650114.gif)

- Luo. Näennäiskiintolevyn kiintolevy, joka sisältää raportteja ja valitse Lähetä ja liitä asemaa.

    1. Luo. Näennäiskiintolevyn paikalliseen tietokoneeseen, joka sisältää raporttien kiintolevylle.

    1. Luominen ja hallinta-varmenteen asentaminen.

    1. Lataa Näennäiskiintolevytiedosto Azure Lisää AzureVHD-cmdlet-komennolla [luominen ja lataa Windows Server Näennäiskiintolevyn Azure avulla](virtual-machines-windows-classic-createupload-vhd.md).

    1. Liittää levyn virtuaalikoneen.

## <a name="install-other-sql-server-services-and-features"></a>Asenna muiden SQL Server Services ja toiminnot

Voit asentaa muita SQL Server Analysis Servicesin taulukkomuotoisen tilassa palveluihin suorittamalla SQL Serverin ohjatun määritystoiminnon. Asennustiedostot ovat virtual tietokoneen paikalliseen levyasemaan.

1. Napsauta **Käynnistä-painiketta** ja valitse sitten **Kaikki ohjelmat**.

1. Valitse **Microsoft SQL Server 2016**, **Microsoft SQL Server 2014** tai **Microsoft SQL Server 2012** ja valitse **Määritystyökalut**.

1. Valitse **SQL Serverin asennus keskelle**.

C:\SQLServer_13.0_full\setup.exe, C:\SQLServer_12.0_full\setup.exe tai C:\SQLServer_11.0_full\setup.exe tai

>[AZURE.NOTE] SQL Server-asennusohjelman ensimmäistä kertaa asennuksen tiedostoja voi ladata ja edellyttävät virtuaalikoneen uudelleen ja SQL Serverin asennus uudelleenkäynnistämistä.
>
>Jos haluat mukauttaa toistuvasti-Microsoft Azure virtuaalikoneen valittu kuva, kannattaa ehkä luoda SQL Server-kuva. Analysis Services SysPrep-toiminto on käytössä SQL Server 2012 SP1 CU2. Lisätietoja on artikkelissa [asennetaan SQL Serverin avulla SysPrep Huomioitavaa](https://msdn.microsoft.com/library/ee210754.aspx) ja [Palvelinrooleista Sysprep-tuki](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).

### <a name="to-install-analysis-services-tabular-mode"></a>Asenna taulukkomuotoinen Analysis Services-tilassa

Ohjeita tämän osan **yhteenvedon** Analysis Services-taulukkomallissa tilassa asennuksen. Lisätietoja on seuraavissa artikkeleissa:

- [Asentaa Analysis Services-Taulukkomallissa tilassa](https://msdn.microsoft.com/library/hh231722.aspx)

- [Taulukkomuotoinen mallinnus (Adventure Works-opas)](https://msdn.microsoft.com/library/140d0b43-9455-4907-9827-16564a904268)

**Voit asentaa Analysis Services taulukkomuotoinen tilan seuraavasti:**

1. SQL Serverin ohjatun asennuksen **asennuksen** vasemmanpuoleisessa ruudussa ja valitse sitten **Uusi SQL Serverin erillinen asennuksen tai toimintojen lisääminen aiemmin asennetun**.

    - Jos näet **Etsi kansio**, c:\SQLServer_13.0_full, c:\SQLServer_12.0_full tai c:\SQLServer_11.0_full selaamalla ja valitse sitten **Ok**.

1. Valitse tuotteen päivitykset-sivulla **Seuraava** .

1. Valitse **Asennustyyppi** -sivulla Valitse **Suorita uusi asennus SQL Serverin** ja valitse **Seuraava**.

1. Valitse **Asetukset-roolin** -sivulla **SQL Serverin ominaisuuksia asennuksen**.

1. Valitse **Ominaisuudet** -sivulla **Analysis Services**.

1. Kirjoita kuvaava nimi, kuten **taulukko** **Esiintymän nimi** ja **Esiintymätunnus** tekstiruutuihin **Esiintymän määritys** -sivulla.

1. **Analysis Services-määritys** -sivulla **Taulukkomuotoinen tilan**valitseminen. Lisää nykyisen käyttäjän järjestelmänvalvojan oikeudet-luetteloon.

1. Viimeistele ja sulje ohjattu SQL Serverin asennus.

## <a name="analysis-services-configuration"></a>Analysis Services-kokoonpanon määritys

### <a name="remote-access-to-analysis-services-server"></a>Etäkäyttö Analysis Services-palvelimeen

Analysis Services-palvelin tukee vain windows-todennusta. Jotta etäkäyttö Analysis Services client-sovelluksista, kuten SQL Server Management Studiossa tai SQL Server Data Tools-virtuaalikoneen on liitettävä paikallisen toimialueen Virtual Azure-toiminnon avulla. Lisätietoja on artikkelissa [Azure Virtual verkossa](../virtual-network/virtual-networks-overview.md).

Analysis Services **oletusesiintymää** seuraa TCP portin **2383**. Avaa portti näennäiskoneiden palomuurin. Analysis Services liitetty nimettyyn esiintymään seuraa myös portin **2383**.

Varten **nimetty** Analysis Services-palvelu SQL Server-selain tarvitaan portin käyttöoikeuksien hallinta. SQL Server-selain oletusarvo on portti **2382**.

Näennäiskoneiden palomuuri-avaa portti **2382** ja luo staattinen analyysipalveluiden nimeltä esiintymän portin.

1. Voit tarkistaa portit, jotka ovat jo käytössä AM ja portit käyttävä prosessi, suorita seuraava komento järjestelmänvalvojan oikeuksin:

        netstat /ao

1. Käytä SQL Server Management Studiossa staattinen Analysis Services luo arvo esiintymän portin päivittämällä portin' taulukkomuotoinen AS-esiintymän yleiset ominaisuudet. Katso lisätietoja, "käyttävät kiinteä porttia oletusarvoisesti tai nimetty" määrittäminen [Windowsin palomuurin Salli Analysis Services-käyttö](https://msdn.microsoft.com/library/ms174937.aspx#bkmk_fixed).

1. Käynnistä uudelleen Analysis Services-palvelun taulukkoesiintymien.

Lisätietoja on artikkelissa, tässä asiakirjassa **virtuaalikoneen päätepisteet ja palomuurin porttien** -osassa.

## <a name="virtual-machine-endpoints-and-firewall-ports"></a>Virtuaalikoneen päätepisteet ja palomuurin porttien

Tässä osassa on yhteenveto Microsoft Azure virtuaalikoneen päätepisteet luominen ja avaaminen virtuaalikoneen palomuurit portit. Seuraavassa taulukossa on yhteenveto, jos haluat luoda päätepisteet **TCP** -portit ja avaa näennäiskoneiden palomuurin porttien.

- Jos käytössäsi on yhden AM ja kaksi seuraavat ehdot täyttyvät, sinun ei tarvitse luoda AM päätepisteet ja sinun ei tarvitse avata portit AM palomuuri.

    - SQL Server-ominaisuuksien käyttöönotto AM ei etäyhteyden muodostaa. Paikallisesti AM, SQL Server-ominaisuuksien ja vahvistamisesta Etätyöpöytäyhteys AM, ei pidetä etäyhteyden muodostaminen SQL Server-ominaisuuksia.

    - Älä yhdistä AM paikallisen toimialueeseen Azure Virtual verkko-tai VPN tunneloinnin ratkaisu.

- Jos virtuaalikoneen ei ole liitetty toimialue, mutta haluat etäyhteyden muodostaa yhteyden SQL Server-ominaisuuksien käyttöönotto AM:

    - Avaa portit AM palomuuri.

    - Luo virtuaalikoneen päätepisteet merkille portit (*).

- Jos virtuaalikoneen on liitetty toimialueeseen käyttämällä VPN-tunnelin, kuten Azure Virtual verkko-päätepisteet eivät ole pakollisia. Avaa kuitenkin portit AM palomuuri.

  	|Port (portti)|Tyyppi|Kuvaus|
|---|---|---|
|**80**|TCP|Raporttipalvelimen etäkäyttöpalvelimen (*).|
|**1433**|TCP|SQL Server Management Studiossa (*).|
|**1434**|UDP|SQL Server-selain. Tämä tarvita, kun-AM liitetty toimialueeseen.|
|**2382**|TCP|SQL Server-selain.|
|**2383**|TCP|SQL Server Analysis Services-oletusesiintymää ja liitetty nimetty esiintymät.|
|**Käyttäjän määrittämät**|TCP|Luo staattinen analyysipalveluiden nimetyt esiintymän portin porttinumero, voit valita ja salliminen palomuurin porttinumero.|

Lisätietoja päätepisteet luomisesta on seuraavissa artikkeleissa:

- Luo päätepisteet:[Voit määrittää Virtual Machine päätepisteet](virtual-machines-windows-classic-setup-endpoints.md).

- SQL Server: Katso [valmistelu SQL Server Virtual Machine Azure-](virtual-machines-windows-portal-sql-server-provision.md)"Kaikki kokoonpano vaiheet virtuaalikoneen käyttäminen SQL Server Management Studiossa yhteyden" osio.

Seuraavassa kaaviossa on kuvattu avaaminen Salli etäkäyttöpalvelimen ominaisuuksien ja-komponentteja AM AM palomuurin porttien.

![Avaa Azure VMs bi-sovelluksen portit](./media/virtual-machines-windows-classic-ps-sql-bi/IC654385.gif)

## <a name="resources"></a>Resurssit

- Tarkista tukikäytännöistä Microsoft server-ohjelmiston käytetään Azure virtuaalikoneen-ympäristössä. Seuraavassa aiheessa on yhteenveto ominaisuuksien, kuten BitLocker, vikasietoklustereihin ja verkon kuormituksen tasaamisen tuki. [Microsoft server-ohjelmiston tukevat Azuren näennäiskoneiden](http://support.microsoft.com/kb/2721672).

- [SQL Server Azure-Virtuaalikoneissa yleiskatsaus](virtual-machines-windows-sql-server-iaas-overview.md)

- [Näennäiskoneiden](https://azure.microsoft.com/documentation/services/virtual-machines/)

- [SQL Server Virtual kone Azure-valmistelu](virtual-machines-windows-portal-sql-server-provision.md)

- [Tietoja DVD-levyllä liittämisestä Virtual Machine](virtual-machines-windows-classic-attach-disk.md)

- [Tietokannan siirtäminen SQL Server Azure AM](virtual-machines-windows-migrate-sql.md)

- [Tilaa Analysis Services-esiintymän määrittäminen](https://msdn.microsoft.com/library/gg471594.aspx)

- [Moniulotteisia mallinnus (Adventure Works-opas)](https://technet.microsoft.com/library/ms170208.aspx)

- [Azure dokumentaatio Center](https://azure.microsoft.com/documentation/)

- [Kun Power BI: n käyttäminen Yhdistelmäympäristössä](https://msdn.microsoft.com/library/dn798994.aspx)

>[AZURE.NOTE] [Lähetä palautetta- ja yhteystiedot Microsoft SQL Server-yhteyden kautta](https://connect.microsoft.com/SQLServer/Feedback)

### <a name="community-content"></a>Yhteisösisältö

- [Azure SQL-tietokannan hallinta PowerShellin avulla](http://blogs.msdn.com/b/windowsazure/archive/2013/02/07/windows-azure-sql-database-management-with-powershell.aspx)

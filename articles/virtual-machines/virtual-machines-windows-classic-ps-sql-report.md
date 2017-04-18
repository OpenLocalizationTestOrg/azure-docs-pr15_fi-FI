<properties 
    pageTitle="PowerShellin avulla voit luoda AM alkuperäistilassa raporttipalvelimen | Microsoft Azure"
    description="Tässä artikkelissa kuvataan, sekä esitellään käyttöönotto- ja SQL Server Reporting Services alkuperäistilassa raporttipalvelimen Azure Virtual Machine määrittäminen. "
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

# <a name="use-powershell-to-create-an-azure-vm-with-a-native-mode-report-server"></a>Luo Azure AM alkuperäistilassa raporttipalvelimen PowerShellin avulla

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]
 

Tässä artikkelissa kuvataan, sekä esitellään käyttöönotto- ja SQL Server Reporting Services alkuperäistilassa raporttipalvelimen Azure Virtual Machine määrittäminen. Ohjeita tämän asiakirjan manuaalisesti yhdistelmän avulla voit luoda virtuaalikoneen- ja Windows PowerShell-komentosarjaa määrittämiseksi Reporting Services AM. Kokoonpanon komentosarjaa sisältää HTTP tai HTTPs-portin avaamiseen.

>[AZURE.NOTE] Jos **HTTPS** ei tarvitse raporttipalvelimen, **Siirry vaiheeseen 2**.
>
>Kun olet luonut AM vaiheessa 1, siirry kohtaan Käytä komentosarjaa raporttipalvelimen ja HTTP määrittäminen. Kun olet suorittanut komentosarja, raporttipalvelimen on valmis käytettäväksi.

## <a name="prerequisites-and-assumptions"></a>Käytön edellytykset ja oletukset

- **Azure-tilauksen**: Tarkista sydämiä käytettävissä Azure-tilauksen määrä. Jos luot suositeltu AM koko, **a3**, sinun on **4** käytettävissä sydämiä. Jos käytät **A2**AM koosta, sinun on **2** käytettävissä sydämiä.
    
    - Tarkista tilauksen Azure perinteinen portaalissa core sallitut valitsemalla asetukset vasemmanpuoleisessa ruudussa ja valitse sitten Valitse käyttö yläreunan-valikossa.
    
    - Voit suurentaa core kiintiötä [Azure](https://azure.microsoft.com/support/options/)tuelta. AM kokoa on artikkelissa [Azure virtuaalikoneen koot](virtual-machines-linux-sizes.md).

- **Windows PowerShell-komentosarjojen**: aiheen oletetaan, että Windows PowerShellin basic kokemusta. Lisätietoja Windows PowerShellin avulla on seuraavissa artikkeleissa:

    - [Käynnistäminen Windows PowerShellin Windows Server](https://technet.microsoft.com/library/hh847814.aspx)
    
    - [Windows PowerShellin käytön aloittaminen](https://technet.microsoft.com/library/hh857337.aspx)

## <a name="step-1-provision-an-azure-virtual-machine"></a>Vaihe 1: Valmistelu Azure virtuaalikoneen

1. Siirry Azure perinteinen portaalin.

1. Valitse vasemmanpuoleisessa ruudussa **näennäiskoneiden** .

    ![Microsoft azuren näennäiskoneiden](./media/virtual-machines-windows-classic-ps-sql-report/IC660124.gif)

1. Valitse **Uusi**.

    ![Uusi-painike](./media/virtual-machines-windows-classic-ps-sql-report/IC692019.gif)

1. Valitse **valikoimasta**.

    ![Uusi AM valikoimasta](./media/virtual-machines-windows-classic-ps-sql-report/IC692020.gif)

1. Valitse **SQL Server 2014 RTM Standard – Windows Server 2012 R2** ja Jatka napsauttamalla nuolta.

    ![Seuraava](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)

    Jos tarvitset perustuva tilaukset-ominaisuus Reporting Services-tietoja, valitse **SQL Server 2014 RTM Enterprise – Windows Server 2012 R2**. Katso lisätietoja SQL Server-versiot ja tukemista [-Versiot, SQL Server 2012 tue ominaisuuksia](https://msdn.microsoft.com/library/cc645993.aspx#Reporting).

1. **Virtuaalikoneen määritys** -sivulla Muokkaa seuraavat kentät:
                                    
    - Jos näkyvissä on enemmän kuin yksi **versio JULKAISUPÄIVÄ**, valitse uusimman version.
    
    - **Virtuaalikoneen nimi**: tietokonenimeä käytetään myös seuraava määritys-sivulla Cloud palvelun DNS oletusnimeä. DNS-nimen on oltava yksilöllinen Azure palvelu. Harkitse määrittäminen AM tietokoneen nimi, joka kuvaa AM käyttötarkoituksen. Esimerkiksi ssrsnativecloud.
    
    - **Taso**: Vakio
    
    - **Koko: A3** on SQL Server työmääriä suositellut AM koon. Jos AM käytetään vain raporttipalvelimen, A2 AM koosta riittää, ellei raporttipalvelimen ilmenee suuren kuormituksen. Katso AM hintatiedot, [Näennäiskoneiden hinnat](https://azure.microsoft.com/pricing/details/virtual-machines/).
    
    - **Uuden käyttäjänimi**: antaisit nimi on luotu AM järjestelmänvalvojana.
    
    - **Uusi salasana** ja **Vahvista**. Tätä salasanaa, uusi järjestelmänvalvojan tilille ja kannattaa käyttää vahva salasana.
    
    - Valitse **Seuraava**. ![Seuraava](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)

1. Valitse seuraavalla sivulla Muokkaa seuraavat kentät:

    - **Pilvipalvelussa**: Luo **Uusi pilvipalvelussa**.
    
    - **Cloud palvelun DNS-nimi**: Tämä on pilvipalvelussa, joka on liitetty AM julkinen DNS-nimi. Oletusnimi on AM nimen kirjoittamasi nimi. Jos valitse myöhemmin vaiheet aiheen luotettujen SSL-varmenteen luominen ja sitten DNS-nimeä käytetään "**myönnetty**" varmenteen arvo.
    
    - **Alue/affiniteetti ryhmän/Virtual verkon**: Valitse lähimpänä olevan käyttäjiesi.
    
    - **Tallennustilan tilin**: automaattisesti luotu tallennustilan tilillä.
    
    - **Saatavuus**: ei mitään.
    
    - **PÄÄTEPISTEET** Pidä **Etätyöpöytä** ja **PowerShell** -päätepisteet ja lisää sitten HTTP- tai HTTPS päätepisteen käyttöympäristön mukaan.

        - **HTTP**: julkisista ja yksityisistä-portit **80**ovat. Huomaa, että jos käytössäsi on yksityinen portti kuin 80, muokata **$HTTPport = 80** http-komentosarja.

        - **HTTPS**: julkisista ja yksityisistä portit ovat **443**. Paras käytäntö on yksityinen portin ja palomuurin ja yksityiset porttia raporttipalvelimen määrittäminen. Lisätietoja päätepisteet Katso, [miten voit määrittää määrittäminen tietoliikenteen Virtual Machine](virtual-machines-windows-classic-setup-endpoints.md). Huomaa, että jos käytät porttiin 443, kuin muuttaa parametrin **$HTTPsport = 443** HTTPS-komentosarja.
    
    - Valitse Seuraava. ![Seuraava](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)

1. Ohjatun toiminnon viimeisellä sivulla pitää **asentaa AM-agentti** valittuna oletusarvoisesti. Tämän artikkelin vaiheet eivät käytä AM-agentti, mutta jos aiot pidä tämän AM AM agentti ja laajennukset, voit parantaa hän CM.  Saat lisätietoja AM-agentti [AM agentti ja laajennukset – osa 1](https://azure.microsoft.com/blog/2014/04/11/vm-agent-and-extensions-part-1/). Oletus-laajennuksia ad käynnissä on "BGINFO"-tunniste, joka näyttää AM työpöydälle, kuten sisäinen IP- ja vapauttaa levytilaa Järjestelmätiedot.

1. Valitse valmis. ![Okei](./media/virtual-machines-windows-classic-ps-sql-report/IC660122.gif)

1. **Tila** AM näyttää **Aloitus (Provisioning)** säännöstä aikana ja näyttää sitten kuin **suorittaminen** , kun AM on valmisteltu ja käyttövalmis.

## <a name="step-2-create-a-server-certificate"></a>Vaihe 2: Palvelimen varmenteen luominen

>[AZURE.NOTE] Jos sinun ei tarvitse HTTPS raporttipalvelimen, voit **ohittaa vaiheen 2** ja siirry kohtaan **Käytä komentosarjaa raporttipalvelimen ja HTTP määrittäminen**. HTTP-komentosarjan avulla voit nopeasti määrittää raporttipalvelimessa ja raporttipalvelimen päivitetään käyttövalmis.

Jotta voit käyttää HTTPS AM, sinun on luotettu SSL-varmenteen. Oman toimintamallin jollakin seuraavista tavoista:

- Kelvollinen SSL-varmenne varmenteiden myöntäjältä (CA) antamaa myöntänyt ja luottaa Microsoft. Pääkansio varmenteiden tarvitaan jaetaan Microsoft pääkansion varmenne-ohjelman kautta. Lisätietoja tämän ohjelman artikkelissa [Windows- ja Windows Phone 8 SSL pääkansion varmenteen ohjelma (jäsenen CA)](http://social.technet.microsoft.com/wiki/contents/articles/14215.windows-and-windows-phone-8-ssl-root-certificate-program-member-cas.aspx) ja [Microsoft pääkansion varmenne-ohjelman esittely](http://social.technet.microsoft.com/wiki/contents/articles/3281.introduction-to-the-microsoft-root-certificate-program.aspx).

- Itse allekirjoitetun varmenteen. Itse allekirjoitetun varmenteet ei suositella tuotannon ympäristössä.

### <a name="to-use-a-certificate-created-by-a-trusted-certificate-authority-ca"></a>Voit käyttää varmennetta, joka on luotu mukaan luotettu varmenteen myöntäjä (CA)

1. **Palvelimen varmenteen myöntäjältä verkkosivuston pyytäminen**. 

    Voit käyttää ohjatun-varmenteen pyynnön-tiedoston (Certreq.txt) sertifikaatti, jonka lähettää varmenteiden myöntäjä luomiseen, tai Luo online myöntäjä pyyntö. Esimerkiksi Microsoft varmenteen Services Windows Server 2012. Mukaan tunnistus assurance palvelimen varmenteen tarjoamia taso on useita päiviä hyväksyä pyynnön ja lähettää sinulle varmenteen tiedoston varmenteiden myöntäjä useaan kuukauteen. 

    Lisätietoja server-varmenteiden on seuraavissa artikkeleissa: 
    
    - Käytä [Certreq](https://technet.microsoft.com/library/cc725793.aspx), [Certreq](https://technet.microsoft.com/library/cc725793.aspx).
    
    - Suojauksen työkaluja hallintaan Windows Server 2012.

    [Suojauksen Työkalut hallintaan Windows Server 2012](https://technet.microsoft.com/library/jj730960.aspx)

    >[AZURE.NOTE] Luotettu SSL-varmenteen **myöntänyt** -kentässä on oltava sama kuin uuden AM käytettävä **Cloud palvelun DNS-nimi** .

1. **Asenna palvelinvarmennetta WWW-palvelimessa**. WWW-palvelin on tässä tapauksessa AM, joka isännöi raporttipalvelimessa ja sivusto luodaan myöhemmin vaiheet, kun määrität Reporting Services. Lisätietoja palvelimen sertifikaatin asentamisessa verkkopalvelin varmenteen MMC: N avulla kohdassa [palvelimen varmenteen asentaminen](https://technet.microsoft.com/library/cc740068).
    
    Jos haluat käyttää tämän ohjeaiheen mukana komentosarja, raporttipalvelimen määrittäminen varmenteet arvon **allekirjoitus** tarvitaan parametrina komentosarjan. Lisätietoja on kohdassa Lisätietoja siitä, miten voit hankkia varmenteen allekirjoitus.

1. Määritä palvelimen sertifikaatin raporttipalvelin. Tehtävä on valmis seuraavan osion, kun määrität raporttipalvelimessa.

### <a name="to-use-the-virtual-machines-self-signed-certificate"></a>Käyttämään näennäiskoneiden itse allekirjoitetun varmenteen

Itse allekirjoitetun varmenteen on luotu AM, kun AM on valmistelun yhteydessä. Varmenne on sama nimi kuin AM DNS-nimi. Vältä varmenteen virheitä on pakollinen, että varmenne on luotettava AM itse ja myös kaikki sivuston käyttäjät.

1. Luotetaanko päämyöntäjä paikallisen AM-varmenteen lisääminen **Luotettujen päämyöntäjien**varmenne. Seuraavassa on kuvattu toimet, joiden yhteenveto. Yksityiskohtaisia ohjeita siitä, miten voit luottaa varmenteiden Myöntäjä on kohdassa [palvelimen varmenteen asentaminen](https://technet.microsoft.com/library/cc740068).

    1. Azure perinteinen työkaluportaaliin ja valitse AM ja valitse Yhdistä. Selaimen määritysten mukaan sinua voidaan pyytää tallentamaan Etätyöpöytätiedostossa muodostamisesta AM.
    
        ![yhteyden muodostaminen azure virtuaalikoneen](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) Käytä AM käyttäjänimi, käyttäjänimi ja salasana, kun loit AM määritetty. 
    
        Esimerkiksi seuraavan kuvan mukaisesti AM on **ssrsnativecloud** ja käyttäjänimi on **testuser**.
        
        ![inlcudes AM käyttäjätunnus](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)
    
    1. Suorita mmc.exe. Lisätietoja on artikkelissa [Toimintaohje: MMC-laajennuksen sertifikaattien tarkasteleminen](https://msdn.microsoft.com/library/ms788967.aspx).
    
    1. Konsolin sovelluksen **Tiedosto** -valikossa **Varmenteet** -laajennuksen lisääminen, valitse **Tietokone** pyydettäessä ja valitse sitten **Seuraava**.
    
    1. Valitse **Paikallistietokoneen** hallinta ja valitse sitten **Valmis**.
    
    1. Valitse **Ok** ja laajenna sitten **Varmenteet - Omat** solmut ja valitse sitten **Varmenteet**. Varmenteen nimetään AM DNS-nimen ja päättyy **cloudapp.net**. Varmenteen nimeä hiiren kakkospainikkeella ja valitse **Kopioi**.
    
    1. Laajenna **Luotettujen päämyöntäjien** -solmu ja napsauta hiiren kakkospainikkeella **Varmenteet** ja valitse sitten **Liitä**.
    
    1. Vahvista-kohdassa **Luotettujen päämyöntäjien** varmenteen nimi-ruudussa ja varmista, että ei ole virheitä ja näet varmennetta. Jos haluat käyttää tämän ohjeaiheen mukana HTTPS-komentosarjan, raporttipalvelimen määrittäminen varmenteet arvon **allekirjoitus** tarvitaan parametrina komentosarjan. **Hankkiminen allekirjoitusta**, suorita seuraava. On myös PowerShell otoksen hakemiseen allekirjoitus [Käytä komentosarjaa Määritä raporttipalvelimen ja HTTPS](#use-script-to-configure-the-report-server-and-HTTPS)-osassa.
        
        1. Kaksoisnapsauta varmennetta, kuten ssrsnativecloud.cloudapp.net nimeä.
        
        1. Valitse **tiedot** -välilehti.
        
        1. Valitse **allekirjoitus**. Allekirjoitus arvo näkyy tiedot-kenttään esimerkiksi a6 08 3c df\ssreg f9 0b f7 e3 7c 25 ed a4 ed 7e ac 91 9c 2c fb 2f.
        
        1. Kopioi allekirjoitus ja Tallenna arvo myöhempää käyttöä varten tai Muokkaa komentosarja nyt.
        
        1. (*) Ennen kuin suoritat komentosarjan Poista välilyönnit välissä arvot pareina. Esimerkki ennen merkille allekirjoitus olisi a6083cdff90bf7e37c25eda4ed7eac919c2cfb2f nyt.
        
        1. Määritä palvelimen sertifikaatin raporttipalvelin. Tehtävä on valmis seuraavan osion, kun määrität raporttipalvelimessa.

Jos käytössäsi on itse allekirjoitetun varmenteen SSL-varmenteen nimi vastaa jo AM isäntänimi. Tämän vuoksi koneen DNS on jo rekisteröity yleisesti ja niitä voi käyttää minkä tahansa.

## <a name="step-3-configure-the-report-server"></a>Vaihe 3: Raportin ‑palvelin

Tässä osassa esitellään määrittäminen AM palvelimeksi raportointipalveluiden alkuperäistilassa raportti. Määrittäminen raporttipalvelimen jollakin seuraavista tavoista:

- Komentosarjan avulla voit määrittää raporttipalvelimen

- Määritä raporttipalvelimen määritysten hallinnan avulla.

Tarkempia ohjeita on kohdassa [yhteyden muodostaminen virtuaalikoneen ja aloittaa raportoinnin määritysten hallinta](virtual-machines-windows-classic-ps-sql-bi.md#connect-to-the-virtual-machine-and-start-the-reporting-services-configuration-manager).

**Todennus Huomautus:** Windows-todennus on suositellut todentamismenetelmän ja se on oletusarvon Reporting Services-todennusta. Vain käyttäjät, joille on määritetty AM käyttää Reporting Services ja Reporting Services-rooleille määritettyjen.

### <a name="use-script-to-configure-the-report-server-and-http"></a>Määritä raporttipalvelimen ja HTTP-komentosarjan avulla

Windows PowerShell-komentosarjaa avulla voit määrittää raporttipalvelimessa, toimimalla seuraavasti. Kokoonpano sisältää HTTP-ei HTTPS:

1. Azure perinteinen työkaluportaaliin ja valitse AM ja valitse Yhdistä. Selaimen määritysten mukaan sinua voidaan pyytää tallentamaan Etätyöpöytätiedostossa muodostamisesta AM.

    ![yhteyden muodostaminen azure virtuaalikoneen](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) Käytä AM käyttäjänimi, käyttäjänimi ja salasana, kun loit AM määritetty. 

    Esimerkiksi seuraavan kuvan mukaisesti AM on **ssrsnativecloud** ja käyttäjänimi on **testuser**.
    
    ![inlcudes AM käyttäjätunnus](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)

1. Avaa **Windows PowerShell ise:** AM, järjestelmänvalvojan oikeuksilla. PowerShell ise: asennetaan oletusarvoisesti Windows Server 2012. On suositeltavaa ise: Käytä sen sijaan, että vakio Windows PowerShell-ikkunassa niin, että voit komentosarjan liittäminen ise:, Muokkaa komentosarjaa ja suorittamalla komentosarja.

1. Valitse Windows PowerShell ise: valitsemalla **Näytä** ja valitse sitten **Näytä komentosarja-ruudussa**.

1. Kopioi seuraavaa komentosarjaa ja komentosarjan liittäminen komentosarjan Windows PowerShell ise:-ruudussa.

        ## This script configures a Native mode report server without HTTPS
        $ErrorActionPreference = "Stop"
        
        $server = $env:COMPUTERNAME
        $HTTPport = 80 # change the value if you used a different port for the private HTTP endpoint when the VM was created.
        
        ## Set PowerShell execution policy to be able to run scripts
        Set-ExecutionPolicy RemoteSigned -Force
        
        ## Utility method for verifying an operation's result
        function CheckResult
        {
            param($wmi_result, $actionname)
            if ($wmi_result.HRESULT -ne 0) {
                write-error "$actionname failed. Error from WMI: $($wmi_result.Error)"
            }
        }
        
        $starttime=Get-Date
        write-host -foregroundcolor DarkGray $starttime StartTime
        
        ## ReportServer Database name - this can be changed if needed
        $dbName='ReportServer'
        
        ## Register for MSReportServer_ConfigurationSetting
        ## Change the version portion of the path to "v11" to use the script for SQL Server 2012
        $RSObject = Get-WmiObject -class "MSReportServer_ConfigurationSetting" -namespace "root\Microsoft\SqlServer\ReportServer\RS_MSSQLSERVER\v12\Admin"
        
        ## Report Server Configuration Steps
        
        ## Setting the web service URL ##
        write-host -foregroundcolor green "Setting the web service URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## SetVirtualDirectory for ReportServer site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportServerWebService','ReportServer',1033)
            CheckResult $r "SetVirtualDirectory for ReportServer"
        
        ## ReserveURL for ReportServerWebService - port $HTTPport (for local usage)
            write-host "Calling ReserveURL port $HTTPport"
            $r = $RSObject.ReserveURL('ReportServerWebService',"http://+:$HTTPport",1033)
            CheckResult $r "ReserveURL for ReportServer port $HTTPport" 
           
        ## Setting the Database ##
        write-host -foregroundcolor green "Setting the Database"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## GenerateDatabaseScript - for creating the database
            write-host "Calling GenerateDatabaseCreationScript for database $dbName"
            $r = $RSObject.GenerateDatabaseCreationScript($dbName,1033,$false)
            CheckResult $r "GenerateDatabaseCreationScript"
            $script = $r.Script
        
        ## Execute sql script to create the database
            write-host 'Executing Database Creation Script'
            $savedcvd = Get-Location
            Import-Module SQLPS              ## this automatically changes to sqlserver provider
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
          
        ## GenerateGrantRightsScript 
            $DBUser = "NT Service\ReportServer"
            write-host "Calling GenerateDatabaseRightsScript with user $DBUser"
            $r = $RSObject.GenerateDatabaseRightsScript($DBUser,$dbName,$false,$true)
            CheckResult $r "GenerateDatabaseRightsScript"
            $script = $r.Script
        
        ## Execute grant rights script
            write-host 'Executing Database Rights Script'
            $savedcvd = Get-Location
            cd sqlserver:\
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
        
        ## SetDBConnection - uses Windows Service (type 2), username is ignored
            write-host "Calling SetDatabaseConnection server $server, DB $dbName"
            $r = $RSObject.SetDatabaseConnection($server,$dbName,2,'','')
            CheckResult $r "SetDatabaseConnection"  
        
        ## Setting the Report Manager URL ##
        
        write-host -foregroundcolor green "Setting the Report Manager URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## SetVirtualDirectory for Reports (Report Manager) site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportManager','Reports',1033)
            CheckResult $r "SetVirtualDirectory"
        
        ## ReserveURL for ReportManager  - port $HTTPport
            write-host "Calling ReserveURL for ReportManager, port $HTTPport"
            $r = $RSObject.ReserveURL('ReportManager',"http://+:$HTTPport",1033)
            CheckResult $r "ReserveURL for ReportManager port $HTTPport"
        
        write-host -foregroundcolor green "Open Firewall port for $HTTPport"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## Open Firewall port for $HTTPport
            New-NetFirewallRule -DisplayName “Report Server (TCP on port $HTTPport)” -Direction Inbound –Protocol TCP –LocalPort $HTTPport
            write-host "Added rule Report Server (TCP on port $HTTPport) in Windows Firewall"
        
        write-host 'Operations completed, Report Server is ready'
        write-host -foregroundcolor DarkGray $starttime StartTime
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time

1. Jos olet luonut AM HTTP-porttiin kuin 80, muokata parametrin $HTTPport = 80.

1. Reporting Services on määritetty komentosarja. Jos haluat suorittaa komentosarjan raportointipalvelujen, muokata polku "v11" Get-WmiObject-ilmoituksen nimitilan versio-osa.

1. Komentosarjan suorittaminen

**Kelpoisuustarkistus**: Varmista, että perusraportin server-toiminto toimii, on tämän artikkelin [Tarkista määritykset](#verify-the-configuration) -osassa.

### <a name="use-script-to-configure-the-report-server-and-https"></a>Määritä raporttipalvelimen ja HTTPS-komentosarjan avulla

Windows PowerShellin avulla voit määrittää raporttipalvelimessa, toimimalla seuraavasti. Kokoonpano sisältää HTTPS-ei HTTP.

1. Azure perinteinen työkaluportaaliin ja valitse AM ja valitse Yhdistä. Selaimen määritysten mukaan sinua voidaan pyytää tallentamaan Etätyöpöytätiedostossa muodostamisesta AM.

    ![yhteyden muodostaminen azure virtuaalikoneen](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) Käytä AM käyttäjänimi, käyttäjänimi ja salasana, kun loit AM määritetty. 

    Esimerkiksi seuraavan kuvan mukaisesti AM on **ssrsnativecloud** ja käyttäjänimi on **testuser**.

    ![inlcudes AM käyttäjätunnus](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)

1. Avaa **Windows PowerShell ise:** AM, järjestelmänvalvojan oikeuksilla. PowerShell ise: asennetaan oletusarvoisesti Windows Server 2012. On suositeltavaa ise: Käytä sen sijaan, että vakio Windows PowerShell-ikkunassa niin, että voit komentosarjan liittäminen ise:, Muokkaa komentosarjaa ja suorittamalla komentosarja.

1. Komentosarjojen suorittamisen käyttöön Suorita Windows PowerShell-komentoa:

        Set-ExecutionPolicy RemoteSigned

    Voit suorittaa vahvistamiseksi käytännön seuraavasti:

        Get-ExecutionPolicy

1. Valitse **Windows PowerShell ise:**valitsemalla **Näytä** ja valitse sitten **Näytä komentosarja-ruudussa**.

1. Kopioi seuraava komentosarja ja liittämällä sen Windows PowerShell ise: komentosarja-ruudussa.

        ## This script configures the report server, including HTTPS
        $ErrorActionPreference = "Stop"
        $httpsport=443 # modify if you used a different port number when the HTTPS endpoint was created.
        
        # You can run the following command to get (.cloudapp.net certificates) so you can copy the thumbprint / certificate hash
        #dir cert:\LocalMachine -rec | Select-Object * | where {$_.issuer -like "*cloudapp*" -and $_.pspath -like "*root*"} | select dnsnamelist, thumbprint, issuer
        #
        # The certifacte hash is a REQUIRED parameter
        $certificatehash="" 
        # the certificate hash should not contain spaces
        
        if ($certificatehash.Length -lt 1) 
        {
            write-error "certificatehash is a required parameter"
        } 
        # Certificates should be all lower case
        $certificatehash=$certificatehash.ToLower()
        $server = $env:COMPUTERNAME
        # If the certificate is not a wildcard certificate, comment out the following line, and enable the full $DNSNAme reference.
        $DNSName="+"
        #$DNSName="$server.cloudapp.net"
        $DNSNameAndPort = $DNSName + ":$httpsport"
        
        ## Utility method for verifying an operation's result
        function CheckResult
        {
            param($wmi_result, $actionname)
            if ($wmi_result.HRESULT -ne 0) {
                write-error "$actionname failed. Error from WMI: $($wmi_result.Error)"
            }
        }
        
        $starttime=Get-Date
        write-host -foregroundcolor DarkGray $starttime StartTime
        
        ## ReportServer Database name - this can be changed if needed
        $dbName='ReportServer'
        
        write-host "The script will use $DNSNameAndPort as the DNS name and port" 
        
        ## Register for MSReportServer_ConfigurationSetting
        ## Change the version portion of the path to "v11" to use the script for SQL Server 2012
        $RSObject = Get-WmiObject -class "MSReportServer_ConfigurationSetting" -namespace "root\Microsoft\SqlServer\ReportServer\RS_MSSQLSERVER\v12\Admin"
        
        ## Reporting Services Report Server Configuration Steps
        
        ## 1. Setting the web service URL ##
        write-host -foregroundcolor green "Setting the web service URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## SetVirtualDirectory for ReportServer site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportServerWebService','ReportServer',1033)
            CheckResult $r "SetVirtualDirectory for ReportServer"
        
        ## ReserveURL for ReportServerWebService - port 80 (for local usage)
            write-host 'Calling ReserveURL port 80'
            $r = $RSObject.ReserveURL('ReportServerWebService','http://+:80',1033)
            CheckResult $r "ReserveURL for ReportServer port 80" 
        
        ## ReserveURL for ReportServerWebService - port $httpsport
            write-host "Calling ReserveURL port $httpsport, for URL: https://$DNSNameAndPort"
            $r = $RSObject.ReserveURL('ReportServerWebService',"https://$DNSNameAndPort",1033)
            CheckResult $r "ReserveURL for ReportServer port $httpsport" 
        
        ## CreateSSLCertificateBinding for ReportServerWebService port $httpsport
            write-host "Calling CreateSSLCertificateBinding port $httpsport, with certificate hash: $certificatehash"
            $r = $RSObject.CreateSSLCertificateBinding('ReportServerWebService',$certificatehash,'0.0.0.0',$httpsport,1033)
            CheckResult $r "CreateSSLCertificateBinding for ReportServer port $httpsport" 
            
        ## 2. Setting the Database ##
        write-host -foregroundcolor green "Setting the Database"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## GenerateDatabaseScript - for creating the database
            write-host "Calling GenerateDatabaseCreationScript for database $dbName"
            $r = $RSObject.GenerateDatabaseCreationScript($dbName,1033,$false)
            CheckResult $r "GenerateDatabaseCreationScript"
            $script = $r.Script
        
        ## Execute sql script to create the database
            write-host 'Executing Database Creation Script'
            $savedcvd = Get-Location
            Import-Module SQLPS                    ## this automatically changes to sqlserver provider
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
          
        ## GenerateGrantRightsScript 
            $DBUser = "NT Service\ReportServer"
            write-host "Calling GenerateDatabaseRightsScript with user $DBUser"
            $r = $RSObject.GenerateDatabaseRightsScript($DBUser,$dbName,$false,$true)
            CheckResult $r "GenerateDatabaseRightsScript"
            $script = $r.Script
        
        ## Execute grant rights script
            write-host 'Executing Database Rights Script'
            $savedcvd = Get-Location
            cd sqlserver:\
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
        
        ## SetDBConnection - uses Windows Service (type 2), username is ignored
            write-host "Calling SetDatabaseConnection server $server, DB $dbName"
            $r = $RSObject.SetDatabaseConnection($server,$dbName,2,'','')
            CheckResult $r "SetDatabaseConnection"  
        
        ## 3. Setting the Report Manager URL ##
        
        write-host -foregroundcolor green "Setting the Report Manager URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## SetVirtualDirectory for Reports (Report Manager) site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportManager','Reports',1033)
            CheckResult $r "SetVirtualDirectory"
        
        ## ReserveURL for ReportManager  - port 80
            write-host 'Calling ReserveURL for ReportManager, port 80'
            $r = $RSObject.ReserveURL('ReportManager','http://+:80',1033)
            CheckResult $r "ReserveURL for ReportManager port 80"
        
        ## ReserveURL for ReportManager - port $httpsport
            write-host "Calling ReserveURL port $httpsport, for URL: https://$DNSNameAndPort"
            $r = $RSObject.ReserveURL('ReportManager',"https://$DNSNameAndPort",1033)
            CheckResult $r "ReserveURL for ReportManager port $httpsport" 
        
        ## CreateSSLCertificateBinding for ReportManager port $httpsport
            write-host "Calling CreateSSLCertificateBinding port $httpsport with certificate hash: $certificatehash"
            $r = $RSObject.CreateSSLCertificateBinding('ReportManager',$certificatehash,'0.0.0.0',$httpsport,1033)
            CheckResult $r "CreateSSLCertificateBinding for ReportManager port $httpsport" 
        
        write-host -foregroundcolor green "Open Firewall port for $httpsport"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## Open Firewall port for $httpsport
            New-NetFirewallRule -DisplayName “Report Server (TCP on port $httpsport)” -Direction Inbound –Protocol TCP –LocalPort $httpsport
            write-host "Added rule Report Server (TCP on port $httpsport) in Windows Firewall"
        
        write-host 'Operations completed, Report Server is ready'
        write-host -foregroundcolor DarkGray $starttime StartTime
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time

1. Komentosarjan **$certificatehash** -parametrin muokkaaminen:

    - Tämä on **pakollinen** parametri. Jos et tallentanut edellisen vaiheen varmenne-arvo, käytä jollakin seuraavista tavoista varmenteen hash-arvon kopioiminen varmenteet-allekirjoitus.:

        Avaa Windows PowerShell ise: AM, ja suorita seuraava komento:

            dir cert:\LocalMachine -rec | Select-Object * | where {$_.issuer -like "*cloudapp*" -and $_.pspath -like "*root*"} | select dnsnamelist, thumbprint, issuer

        Tulos näyttää seuraavankaltaiselta. Jos komentosarja palauttaa tyhjän rivin, AM ei ole määritetty esimerkiksi varmenne, on kohdassa [käyttämään näennäiskoneiden Self-signed varmennetta](#to-use-the-virtual-machines-self-signed-certificate).
    
    TAI
    
    - Suorita AM mmc.exe ja lisää sitten **Varmenteet** -laajennuksen.
    
    - Kaksoisnapsauta **Luotettu varmenteen päämyöntäjien** solmu varmenteen nimi. Jos käytössäsi AM itse allekirjoitettua varmennetta, varmenteen nimetään AM DNS-nimen ja päättyy **cloudapp.net**.
    
    - Valitse **tiedot** -välilehti.
    
    - Valitse **allekirjoitus**. Allekirjoitus arvo näkyy tiedot-kenttään esimerkiksi af 11 60 b6 4 28 8 d 89 0a 82 12 ff 6 b a9 c3 66 4f 31 90 48
    
    - **Ennen kuin suoritat komentosarjan**, poista välilyönnit välissä arvot pareina. Esimerkki af1160b64b288d890a8212ff6ba9c3664f319048

1. **$Httpsport** -parametrin muokkaaminen: 

    - Jos olet käyttänyt porttiin 443 HTTPS-päätepisteen, sinun ei tarvitse päivittää parametriä komentosarjan. Muussa tapauksessa arvoksi portin valitsit, kun olet määrittänyt HTTPS yksityinen päätepisteen AM.

1. **$DNSName** -parametrin muokkaaminen: 

    - Komentosarja on määritetty yleismerkki varmenne $DNSName = "+". Jos et ole haluat yleismerkkien varmenteen sidonnan määrittäminen, kommentoi pois $DNSName ="+"ja ota käyttöön seuraava rivi koko $DNSNAme viittaus, ## $DNSName="$server.cloudapp.net".

        Muuttaa $DNSName arvon, jos et halua käyttää Reporting Services virtual machine DNS-nimi. Jos käytät parametri, varmenne on myös käyttää tätä nimeä ja rekisteröi yleisesti DNS-palvelimen nimeä.

1. Reporting Services on määritetty komentosarja. Jos haluat suorittaa komentosarjan raportointipalvelujen, muokata polku "v11" Get-WmiObject-ilmoituksen nimitilan versio-osa.

1. Komentosarjan suorittaminen

**Kelpoisuustarkistus**: Varmista, että perusraportin server-toiminto toimii, on tämän artikkelin [Tarkista määritykset](#verify-the-connection) -osassa. Tarkista varmenne sidonta Avaa komentorivi järjestelmänvalvojan oikeuksin ja suorittamalla seuraavan komennon:

    netsh http show sslcert

Tulos ovat seuraavat:

    IP:port                      : 0.0.0.0:443

    Certificate Hash             : f98adf786994c1e4a153f53fe20f94210267d0e7

### <a name="use-configuration-manager-to-configure-the-report-server"></a>Voit määrittää raporttipalvelimen käyttämällä määritysten hallinta

Jos et halua suorittaa määrittäminen raporttipalvelimen PowerShell-komentosarjaa, voit raportointipalveluiden alkuperäistilassa määritysten hallinnan avulla voit määrittää raporttipalvelimen ohjeiden mukaisesti.

1. Azure perinteinen työkaluportaaliin ja valitse AM ja valitse Yhdistä. Käytä käyttäjänimi ja salasana, kun loit AM määritetty.

    ![yhteyden muodostaminen azure virtuaalikoneen](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif)

1. Suorita Windows update ja asenna päivitykset AM. Jos uudelleenkäynnistys AM vaaditaan, Käynnistä AM ja muodostaa AM perinteinen Azure-portaalista.

1. Kirjoita **Reporting Services** AM Käynnistä-valikosta ja Avaa **Reporting Services-määritysten hallinta**.

1. Jätä oletusarvot **Palvelimen nimi** ja **Raporttiesiintymä**. Valitse **Muodosta yhteys**.

1. Valitse vasemmanpuoleisessa ruudussa **WWW-palvelun URL**.

1. Oletusarvon mukaan RS on määritetty HTTP-portin 80 "Kaikkien varattujen" IP kanssa. Lisää HTTPS:

    1. **SSL**-varmenne: Valitse varmenne, jota haluat käyttää, esimerkiksi [AM nimi]. cloudapp.net. Jos luettelossa ei ole sertifikaatteja, katso kohta **Vaihe 2: palvelimen varmenteen luominen** lisätietoja siitä, miten voit asentaa ja joihin luotat AM varmenne.
    
    1. Valitse **SSL-portti**: Valitse 443. Jos olet määrittänyt HTTPS yksityinen päätepisteen eri yksityinen porttiin AM, käytä arvo tähän.
    
    1. Valitse **Käytä** ja odota, kunnes toiminto on valmis.

1. Valitse vasemmanpuoleisessa ruudussa **tietokanta**.

    1. Valitse **Muuta Databas**e.
    
    1. **Luo uusi raportti-server-tietokanta** ja valitse sitten **Seuraava**.
    
    1. Käytä oletusnimeä **Palvelimen nimi**: nimellä AM nimi ja hyväksy oletusnimi **Todennustyyppi** **Nykyisen käyttäjän** – **Integroitu suojaus**. Valitse **Seuraava**.
    
    1. Käytä oletusnimeä **raporttipalvelimen** **Tietokannan nimi** ja valitse **Seuraava**.
    
    1. Käytä oletusnimeä **Todennustyyppi** kuin **Palvelun käyttäjätiedot** ja valitse **Seuraava**.
    
    1. Valitse **Yhteenveto** -sivulla **Seuraava** .
    
    1. Kun määritykset on valmis, valitse **Valmis**.

1. Valitse vasemmanpuoleisessa ruudussa **Report Managerin URL-osoite**. Käytä oletusnimeä **Näennäiskansio** **raportteina** ja sitten **Käytä**.

1. Valitse **Lopeta** Sulje Reporting Services-määritysten hallinta.

## <a name="step-4-open-windows-firewall-port"></a>Vaihe 4: Avaa Windowsin palomuuri portti

>[AZURE.NOTE] Jos olet käyttänyt jokin komentosarjat määrittäminen raporttipalvelimessa, voit ohittaa tämän osion. Komentosarja sisällyttää Avaa palomuuri portti vaiheen. Oletusarvo on portti 80 HTTP ja HTTPS porttia 443.

Etäkäyttö Report Managerin tai virtuaalikoneen raporttipalvelimeen, TCP-päätepistettä tarvitaan AM. Se tarvitaan saman portin avaaminen AM palomuuri. Päätepisteen luotiin, kun AM on valmistelun yhteydessä.

Tässä osassa on perustietoja siitä, miten voit avata palomuurin portin. Lisätietoja on artikkelissa [raportin Serverin käytön palomuurin määrittäminen](https://technet.microsoft.com/library/bb934283.aspx)

>[AZURE.NOTE] Jos komentosarjan avulla voit määrittää raporttipalvelimessa, voit ohittaa tämän osion. Komentosarja sisällyttää Avaa palomuuri portti vaiheen.

Jos olet määrittänyt varten HTTPS yksityinen porttiin 443 kuin, Muokkaa seuraavaa komentosarjaa asianmukaisesti. Avaa Windowsin palomuurin porttiin **443** , suorita seuraavat vaiheet:

1. Avaa Windows PowerShell-ikkuna järjestelmänvalvojan oikeuksilla.

1. Jos olet käyttänyt porttiin 443 kuin, kun olet määrittänyt HTTPS-päätepisteen AM, Päivitä portin seuraava komento ja suorita-komento:

        New-NetFirewallRule -DisplayName “Report Server (TCP on port 443)” -Direction Inbound –Protocol TCP –LocalPort 443

1. Kun komento on valmis, **Ok** näkyy komentokehote.

Voit varmistaa, että portti on auki, Avaa Windows PowerShell-ikkuna ja suorittamalla seuraavan komennon:

    get-netfirewallrule | where {$_.displayname -like "*report*"} | select displayname,enabled,action

## <a name="verify-the-configuration"></a>Tarkista määritykset

Voit varmistaa, että perusraportin server-toiminto toimii nyt, Avaa selaimen järjestelmänvalvojan oikeuksin ja selaamalla seuraavat raportin palvelimen ad report Managerin URL-Osoitteet:

- Siirry AM, raporttipalvelimen URL:

        http://localhost/reportserver

- Siirry AM, raportin Manager URL-osoite:

        http://localhost/Reports

- Siirry paikallisessa tietokoneessa AM **remote** raportin hallinta. Päivitä DNS-nimi seuraavan esimerkin mukaisesti. Kun ohjelma kysyy salasanaa, käyttämällä järjestelmänvalvojan tunnistetietoja, voit luoda, kun AM on valmistelun yhteydessä. Käyttäjänimi on [toimialue]\[käyttäjänimi]-muodossa, missä toimialueen AM tietokonenimi, esimerkiksi ssrsnativecloud\testuser. Jos et käytä HTTP**S**, poista **s** URL-osoite. Lisätietoja on kohdassa Lisätietoja luomisesta AM muita käyttäjiä.

        https://ssrsnativecloud.cloudapp.net/Reports

- Paikallisesta tietokoneesta Etsi remote raporttipalvelimen URL-osoite. Päivitä DNS-nimi seuraavan esimerkin mukaisesti. Jos et käytä HTTPS-poistaa s URL-osoite.

        https://ssrsnativecloud.cloudapp.net/ReportServer

## <a name="create-users-and-assign-roles"></a>Käyttäjien ja roolien määrittäminen

Kun määrittäminen ja tarkistaminen raporttipalvelimessa, Yleiset järjestelmänvalvojan tehtävä on luoda yhden tai usean käyttäjän ja määritettävä käyttäjille Reporting Services-rooleille. Lisätietoja on seuraavissa artikkeleissa:

- [Paikallisen käyttäjätilin luominen](https://technet.microsoft.com/library/cc770642.aspx)

- [Myönnä käyttöoikeudet raporttipalvelimen (Report Managerin)](https://msdn.microsoft.com/library/ms156034.aspx))

- [Voit luoda ja hallita roolimäärityksiä](https://msdn.microsoft.com/library/ms155843.aspx)

## <a name="to-create-and-publish-reports-to-the-azure-virtual-machine"></a>Voit luoda ja julkaista raportteja Azure virtuaalikoneen

Seuraavassa taulukossa on yhteenveto aiemmin raportit paikallisen tietokoneen isännöidään käyttöön Microsoft Azure virtuaalikoneen raporttipalvelimen saatavilla olevista vaihtoehdoista:

- **RS.exe komentosarja**: Käytä RS.exe komentosarjan, joka kopioi raportti-kohteet ja raporttipalvelimen, Microsoft Azure virtuaalikoneen. Lisätietoja on kohdassa "Alkuperäistilassa alkuperäiseen tilaan – Microsoft Azure virtuaalikoneen" [otoksen Reporting Services-rs.exe komentosarjan, joka siirtää sisällön raportin palvelinten välillä](https://msdn.microsoft.com/library/dn531017.aspx).

- **Raportin muodostin**: virtuaalikoneen sisältää valitsemalla-kerran Microsoft SQL Server Report Builder versio. Aloittaa virtuaalikoneen raportin muodostin ensimmäisen kerran:

    1. Käynnistä selain järjestelmänvalvojan oikeuksilla.
    
    1. Siirry report Managerin virtuaalikoneen, ja valitse **Report Builder** valintanauhassa.

    Lisätietoja on artikkelissa [asentaminen, Uninstalling, ja sitä tukevat Report Builder](https://technet.microsoft.com/library/dd207038.aspx).

- **SQL Server Data Tools: AM**: Jos olet luonut ja SQL Server 2012 AM ja valitse SQL Server Data Tools on asennettu virtuaalikoneen ja avulla voidaan luoda **Raportin palvelimen projektien** ja virtuaalikoneen raportteja. SQL Server Data Tools julkaista raportit virtuaalikoneen raportin palvelimeen.
    
    Jos olet luonut AM SQL Serverin 2014 kanssa, voit asentaa SQL Serverin tietoihin Työkalut - BI visual Studio. Lisätietoja on seuraavissa artikkeleissa:

    - [Microsoft SQL Server Data Tools – Business Intelligence for Visual Studio 2013](https://www.microsoft.com/download/details.aspx?id=42313)

    - [Microsoft SQL Server Data Tools – Business Intelligence for Visual Studio 2012](https://www.microsoft.com/download/details.aspx?id=36843)

    - [SQL Server Data Tools ja SQL Server-liiketoimintatietojen (SSDT BI)](http://curah.microsoft.com/30004/sql-server-data-tools-ssdt-and-sql-server-business-intelligence)

- **SQL Server Data Tools: Remote**: paikallisessa tietokoneessa Luo Reporting Services-projekti SQL Server Data Tools, joka sisältää Reporting Services-raportteja. Määritä muodostaa WWW-palvelun URL-projekti.

    ![SSRS projektin SSDT projektin ominaisuudet](./media/virtual-machines-windows-classic-ps-sql-report/IC650114.gif)

- **Käytä komentosarjaa**: kopioi raportti palvelimen sisällön komentosarjan avulla. Lisätietoja on artikkelissa [otoksen Reporting Services-rs.exe komentosarjan, joka siirtää sisällön raportin palvelinten välillä](https://msdn.microsoft.com/library/dn531017.aspx).

## <a name="minimize-cost-if-you-are-not-using-the-vm"></a>Pienennä kustannukset, jos käytössäsi ei ole AM

>[AZURE.NOTE] Pienentämiseksi kulujen, että Azuren näennäiskoneiden muussa kuin Käytä Sammuta AM perinteinen Azure-portaalista. Jos käytät Windows power asetukset sisällä AM sulkeutumaan AM, perittävän sama määrä edelleen AM. Voit pienentää kulujen haluat Sammuta AM Azure perinteinen-portaalissa. Jos et enää tarvitse AM, muista poistaa AM ja liittyvän .vhd-tiedostoja tallennustilan lisämaksujen välttämiseksi. Lisätietoja [näennäiskoneiden hinnat](https://azure.microsoft.com/pricing/details/virtual-machines/)tarkastelu usein kysytyt kysymykset-osiossa.

## <a name="more-information"></a>Lisätietoja

### <a name="resources"></a>Resurssit

- Katso yhden palvelimen SQL Server-liiketoimintatietojen ja SharePoint 2013: n käyttöönoton liittyvää sisältöä on samanlainen, [Luo SQL Server BI: kanssa Azure AM ja SharePoint 2013: n Windows PowerShellin](https://msdn.microsoft.com/library/azure/dn385843.aspx).

- Katso [Käyttöön SQL Server liiketoimintatietojen Azuren näennäiskoneiden](https://msdn.microsoft.com/library/dn321998.aspx)SQL Server-liiketoimintatietojen ja SharePoint 2013: n usean palvelimen toteutukseen liittyvät samanlaista sisältöä.

- Katso [SQL Server-liiketoimintatietojen Azuren näennäiskoneiden](virtual-machines-windows-classic-ps-sql-bi.md)SQL Server-liiketoimintatietojen Azuren näennäiskoneiden-versioiden liittyviä yleisiä tietoja.

- Saat lisätietoja Azure kustannukset Laske kulujen on artikkelissa [Azure hinnoittelu](https://azure.microsoft.com/pricing/calculator/?scenario=virtual-machines)Laskimen näennäiskoneiden-välilehti.

### <a name="community-content"></a>Yhteisösisältö

- Katso vaiheittaiset ohjeet luomisesta Reporting Services alkuperäisen tilassa raporttipalvelimen käyttämättä komentosarja- [Isännöinnin SQL Raportointipalvelu Azure Virtual Machine](http://adititechnologiesblog.blogspot.in/2012/07/hosting-sql-reporting-service-on-azure.html).

### <a name="links-to-other-resources-for-sql-server-in-azure-vms"></a>Linkkejä muihin SQL Server Azure VMs resurssit

[SQL Server Azure-Virtuaalikoneissa yleiskatsaus](virtual-machines-windows-sql-server-iaas-overview.md)

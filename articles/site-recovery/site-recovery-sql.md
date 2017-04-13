<properties 
    pageTitle="SQL Server suojaaminen SQL Server-palauttaminen ja Azure palauttaminen | Microsoft Azure" 
    description="Tässä artikkelissa käsitellään replikoida SQL Server-tietokantaan SQL Server Azure sivuston palauttamisen tietojen ominaisuudet." 
    services="site-recovery" 
    documentationCenter="" 
    authors="rayne-wiselman" 
    manager="jwhit" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.workload="backup-recovery" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/04/2016" 
    ms.author="raynew"/>


# <a name="protect-sql-server-with-sql-server-disaster-recovery-and-azure-site-recovery"></a>SQL Server suojaaminen SQL Server-palauttaminen ja Azure sivuston palauttaminen 


Azure palauttaminen-palvelun osaltaan mukaan orchestrating replikoinnin, automaattisesti ja näennäiskoneiden ja fyysiset palvelimet yrityksen liiketoiminnan jatkuvuuden ja tietojen palauttaminen (BCDR) määrittäminen. Koneet voi replikoida Azure tai toissijaisen paikallisen tietokeskuksen. Lue pikaisesti [Azure palauttaminen ominaisuudet?](site-recovery-overview.md).

 Tässä artikkelissa käsitellään suojaa SQL Serverin sovelluksen yhdistämällä SQL Server BCDR Technologies-tuote ja Azure palauttaminen takaisin loppuun. Sinulla pitäisi ymmärtää SQL Serverin tietojen palauttaminen ominaisuuksia (vikasietoklusteri, AlwaysOn käytettävyys ryhmät-tietokannan peilaukseen, kirjaudu toimitus) ja Azure palauttaminen, ennen kuin otat tilanteita, joissa on tässä artikkelissa kuvatulla tavalla.



## <a name="overview"></a>Yleiskatsaus

Monta työmääriä Käytä SQL Serverin perustana. Sovellusten, kuten SharePoint, Dynamics ja SAP Toteuta tietopalvelujen SQL Serverin avulla.  Sovellusten SQL Serverin käyttöönotto useilla eri tavoilla:

- **Erillisestä SQL Server**: SQL Server- ja kaikki tietokannat isännöidään yhteen tietokoneeseen (fyysisiä tai näennäiskansio). Kun virtualisoidussa, host klusterointi käytetään paikallisen suuren käytettävyyden. Ei ole Vieras tason suuren käytettävyyden ei käytössä.
- **SQL Server automaattisesti klusterointi esiintymät (aina käyttöön FCI)**: SQL server-esiintymien kanssa Jaettujen levyjen kahden tai useamman solmut määritetään automaattisesti Windows-klusterin. Jos jokin klusterin esiintymät alas-klusterin voi epäonnistua toisessa esiintymässä SQL Serverin kautta. Tätä asetusta käytetään yleensä HA ensisijainen-sivustossa. Se ei suojaavat virhe- tai käyttökatkosta jaettua tallennustilan tasolla. Jaettu levy voidaan toteuttaa ISCSI Fiber kanavan tai jaettu VHDx.
- **SQL aina käyttöön käytettävyys ryhmät**: näiden määritysten kaksi solmut ovat määritys on jaettu, mitään klusterin SQL Server-tietokantoja määritetty käytettävyys-ryhmässä synkronoitu replikoinnin ja automaattinen automaattisesti.

Enterprise-versioissa SQL Server sisältää myös alkuperäisen tietojen palauttaminen technologies palauttaminen tietokantojen etäpalvelimeen. Tässä artikkelissa on hyödyntää ja integrointi näitä alkuperäisen SQL tietojen palauttaminen tekniikoita: 

- SQL aina käyttöön käytettävyys ryhmät palauttaminen SQL Server 2012-tai Enterprise-versiot 2014.
- SQL tietokantapeilausta SQL Server Standard edition (mikä tahansa versio)- tai SQL Server 2008 R2 suuri suojaus-tilassa.


Sivuston palauttaminen suojata SQL Server-taulukossa on esitetty.

 |**Paikalliseen paikalliseen** | **Paikalliseen Azure avulla** 
---|---|---
**Hyper-V** | Kyllä | Kyllä
**VMware** | Kyllä | Kyllä 
**Fyysinen palvelin** | Kyllä | Kyllä


## <a name="support-and-integration"></a>Tuki- ja integrointi

Tämän artikkelin käyttötavoista tukee seuraavia SQL Server-versioita:


- SQL Server 2014 Enterprise- ja vakio
- SQL Server 2012: n Enterprise- ja vakio
- SQL Server 2008 R2: n Enterprise- ja vakio


Sivuston palauttaminen voi integroida koottu alla olevassa taulukossa antamaan tietojen palauttaminen ratkaista alkuperäisen SQL Server BCDR-tekniikoiden kanssa.

**Toiminto** |**Tiedot** | **SQL Serverin versio** 
---|---|---
**AlwaysOn käytettävyys-ryhmä** | Erillinen useita kertoja SQL Server suoritetaan automaattisesti klusterin, jossa on useita solmujen.<br/><br/>Tietokantojen voidaan ryhmitellä automaattisesti ryhmiin, jotka voidaan kopioida (peilattu) SQL Server-esiintymien, siten, että ei ole jaettu tallennustilan ei tarvita.<br/><br/>Sisältää palauttaminen ensisijainen sivuston ja vähintään yhden toissijaisen sivustojen välillä. Kaksi solmujen voidaan määrittää jaetussa mitään klusterin SQL Server-tietokantoja määritetty käytettävyys-ryhmässä synkronoitu replikoinnin ja automaattinen automaattisesti. | SQL Server 2014 & 2012 Enterprise edition
**Vikasietoklusteri (AlwaysOn FCI)** | SQL Server hyödyntää Windows vikasietoklusteri paikallisen SQL Server työmääriä hyvän käytettävyyden.<br/><br/>Käynnissä olevat esiintymät SQL Serverin kanssa Jaettujen levyjen solmujen määritetään automaattisesti klusterin. Jos erillisen alas klusterin ei kautta toiseen.<br/><br/>Klusterin ei suojaavat virheen tai jaettua tallennustilan katkokset. Jaetun levyn voidaan toteuttaa iSCSI fiber kanavan tai jakaa VHDXs. | SQL Server Enterprise-versiot<br/><br/>SQL Server Standard edition (rajoitettu kaksi solmujen)
**Tietokantapeilaus (suuri suojaus-tila)** | Suojaa toissijainen yksittäistä kopiota yhden tietokannan. Käytettävissä olevat (synkronoitu) sekä suuri suojaus erinomainen suorituskyky (asynkroninen) replikoinnin käyttöön ja päinvastoin. Automaattisesti klusteria ei tarvita. | SQL Server 2008 R2<br/><br/>SQL Server Enterprise kaikki versiot
**Erillisestä SQL Server** | SQL-palvelin ja tietokanta isännöidään yhdellä Palvelimella (fyysinen tai virtual). Host klusterointi käytetään suuren käytettävyyden, jos palvelin kuuluu virtual. Ei ole Vieras tason suuren käytettävyyden. | Enterprise tai Standard edition

## <a name="deployment-recommendations"></a>Käyttöönoton suositukset


Tässä taulukossa on yhteenveto Microsoftin suosituksia SQL Server BCDR technologies-integraation palauttaminen.

**Versio** |**Edition** | **Käyttöönotto** | **Paikallinen, paikallinen** | **Paikallinen Azure avulla** 
---|---|---|---|---
SQL Server 2014 tai 2012 | Yrityksen | Automaattisesti esiintymä | AlwaysOn käytettävyys ryhmät | AlwaysOn käytettävyys ryhmät
 | Yrityksen | Suuren käytettävyyden AlwaysOn käytettävyys-ryhmät | AlwaysOn käytettävyys ryhmät | AlwaysOn käytettävyys ryhmät
 | Vakio | Automaattisesti esiintymä (FCI) | Sivuston palauttaminen replikointia paikallisen peilikuva | Sivuston palauttaminen replikointia paikallisen peilikuva
 | Enterprise- tai vakio | Erillisestä | Sivuston palauttaminen sallittuja | Sivuston palauttaminen sallittuja
SQL Server 2008 R2 | Enterprise- tai vakio | Automaattisesti esiintymä (FCI) | Sivuston palauttaminen replikointia paikallisen peilikuva | Sivuston palauttaminen replikointia paikallisen peilikuva
 | Enterprise- tai vakio | Erillisestä | Sivuston palauttaminen sallittuja | Sivuston palauttaminen sallittuja
SQL Server (kaikki versiot) | Enterprise- tai vakio | Automaattisesti esiintymä - DTC-sovellus | Sivuston palauttaminen sallittuja | Ei tueta


## <a name="deployment-prerequisites"></a>Käyttöönoton edellytykset

Mitä sinun on ennen kuin voit aloittaa:

- Paikallisen SQL Serverin käyttöönoton tuetut SQL Server-versiota. Yleensä myös tarvitset Active Directory SQL Serveriä varten.
- Skenaarion edellytykset, jonka haluat ottaa käyttöön. Edellytykset löytyy kunkin käyttöönotto-artikkelissa. [Sivuston palauttaminen yleiskatsaus](site-recovery-overview.md)toimitetaan näitä linkkejä.
- Jos haluat määrittää palautustoiminnot Azure-tietokannassa, tarvitset [Azure virtuaalikoneen valmiuden](http://www.microsoft.com/download/details.aspx?id=40898) -työkalun suorittaminen SQL Server virtual-tietokoneissa, varmista, että ne ovat yhteensopivia Azure ja palauttaminen.


## <a name="set-up-active-directory"></a>Active Directory määrittäminen

Sinun on Active Directory SQL Serverin oikein toissijaisen palautus-sivustossa. muutama vaihtoehdoista:

- **Pieni yritys**, jos sinulla on small useita sovelluksia ja yhden toimialueen ohjauskoneen paikallisen sivuston ja haluat epäonnistua koko sivuston kautta, suosittelemme käyttämään palauttaminen repication replikoida toimialueen ohjauskoneen toissijainen palvelinkeskukseen tai Azure.

- **Normaali, suuri yritykseen**, jos sinulla on runsaasti sovelluksen, käytössäsi on Active Directory-metsää ja haluat epäonnistua sovelluksen tai työmäärää, on suositeltavaa, voit määrittää muita ohjauskoneen, toissijaisen palvelinkeskuksen tai Azure. Huomaa, että jos käytät AlwaysOn käytettävyys ryhmiä palauttamaan etäpalvelimeen Suosittelemme käyttämään palautetun SQL Server-esiintymän määrittäminen toisen muita toimialueen ohjauskoneen toissijaisen tai Azure.

Ohjeita tämän asiakirjan katsottava, että toissijainen sijainti toimialueen ohjauskoneen on käytettävissä. [Lue lisää](site-recovery-active-directory.md) suojaaminen Active Directory ja palauttaminen.

## <a name="integrate-protection-with-sql-server-always-on-on-premises-to-azure"></a>Integroi suojaus SQL Server Always-On (paikallisen Azure avulla)


Sivuston palauttaminen tukemien SQL AlwaysOn. Jos olet luonut ryhmälle käytettävyys SQL Azure virtuaalikoneen, sitten sivuston palauttaminen avulla voit hallita vikasietotilaa käytettävyys-ryhmien määrittäminen 'Toissijaisena' kanssa. 

>[AZURE.NOTE] Tämä ominaisuus on tällä hetkellä esikatselu ja käytettävissä, kun Hyper-V Host (isäntä)-palvelimet ensisijainen joten hallitaan VMM paveikslėlis tai [Määrityspalvelimen](site-recovery-vmware-to-azure.md#configuration-server-prerequisites)hallitsee VMware asetukset. Oikealle, kun tätä ominaisuutta ei ole käytettävissä uudessa Azure-portaalissa.

#### <a name="prerequisites"></a>Edellytykset

Näin tarvitset SQL AlwaysOn integroida palauttaminen:

- Paikalliseen SQL Server (erillinen server tai automaattisesti klusterin).
- Yhden tai useamman Azuren näennäiskoneiden SQL Server on asennettu
- SQL käytettävyys ryhmän määritetty välillä paikallisen SQL Serverin ja SQL Serveriä Azure-tietokannassa
- PowerShellin Etäpalvelujen tulisi olla käytössä paikallisen SQL Server-tietokoneessa. VMM palvelimeen tai määrityspalvelimen pitäisi remote PowerShell soittaminen SQL Server.
- Käyttäjätilin lisätään paikallisen SQL-palvelimeen nämä SQL käyttäjän ryhmissä, joissa on ainakin seuraavat oikeudet:
    - Muuta KÄYTETTÄVYYS ryhmälle: [Tässä](https://msdn.microsoft.com/library/hh231018.aspx)ja [tähän](https://msdn.microsoft.com/library/ff878601.aspx#Anchor_3)
    - ALTER tietokanta - käyttöoikeudet[tähän](https://msdn.microsoft.com/library/ff877956.aspx#Security)
- RunAs-tilin luodaanko VMM palvelimessa tai tilin luodaan käyttämällä CSPSConfigtool.exe edellisessä vaiheessa mainitusta käyttäjän määrityspalvelimen 
- SQL-PS-moduulin asennetaan SQL-palvelimessa on paikallisen ja Azure-virtuaalikoneissa
- AM-agentti on oltava asennettuna Azure-näennäiskoneiden
- NTAUTHORITY\System pitäisi olla seuraavat oikeudet SQL Server Azure-tietokannassa näennäiskoneiden käytössä:
    - Muuta KÄYTETTÄVYYS ryhmä - käyttöoikeudet [tähän](https://msdn.microsoft.com/library/hh231018.aspx)ja [tähän](https://msdn.microsoft.com/library/ff878601.aspx#Anchor_3)
    - ALTER tietokanta - käyttöoikeudet [tähän](https://msdn.microsoft.com/library/ff877956.aspx#Security)

####  <a name="step-1-add-a-sql-server"></a>Vaihe 1: Lisää SQL Server


1. Valitse Lisää uusi SQL Server **Lisää SQL** . 

    ![Lisää SQL](./media/site-recovery-sql/add-sql.png)

2. Valitse **SQL-asetusten määrittäminen** > **nimi** Anna viittaamaan SQL Server-palvelimen nimi.
3. **SQL Server (FQDN)** Määritä lähdettä, johon haluat lisätä SQL Serverin täydellinen toimialuenimi. Siltä varalta, että automaattisesti-klusterin SQL Server on asennettu, anna FQDN klusterin ja ole minkään klusterisolmut.  
4. Valitse oletusesiintymää **SQL Server-esiintymän** tai mukautetun esiintymän nimi.
5. Valitse **Management Server** VMM palvelimeen tai määrityspalvelimen rekisteröity palauttaminen säilö. Sivuston palauttaminen käyttää tätä Management server vaihtamaan SQL Serverin kanssa.
6. Suorita **tiliksi** Anna RunAs-tili, joka on luotu VMM-palvelimeen tai tili, joka on luotu Configuraaaon palvelimen nimi. Tämä tili voidaan käyttää SQL Serverin ja pitäisi olla luku-ja automaattisesti käytettävyys ryhmiä SQL Server-tietokoneessa.

    ![Lisää SQL-valintaikkuna](./media/site-recovery-sql/add-sql-dialog.png)

Kun olet lisännyt SQL Serverin se näkyy **SQL-palvelimet** -välilehti. 

![SQL Server-luettelo](./media/site-recovery-sql/sql-server-list.png)


#### <a name="step-2-add-a-sql-availability-group"></a>Vaihe 2: Lisää SQL käytettävyys-ryhmä

1. SQL Server kun tietokoneeseen on lisätty seuraavaksi käytettävyys-ryhmien lisääminen sivuston palauttaminen. Saadakseen ne lisätään edellisessä vaiheessa SQL Serverin sisällä alirakenteeseen ja valitse sitten Lisää SQL käytettävyys-ryhmä. 

    ![Lisää SQL AG](./media/site-recovery-sql/add-sqlag.png)

2. SQL käytettävyys ryhmän voi olla replikoiminen vähintään yksi Azuren näennäiskoneiden. Kun lisäämällä sql käytettävyys-ryhmän nimi ja Azure virtuaalikoneen haluamaasi voit olla epäonnistui päälle palauttaminen käytettävyys ryhmän tilauksen tarvitaan.

    ![Lisää SQL AG-valintaikkuna](./media/site-recovery-sql/add-sqlag-dialog.png)

3. Edellä olevassa esimerkissä käytettävyys ryhmän MJP1-AG tulee ensisijainen virtual laitteeseen SQLAGVM2 sisällä tilauksen DevTesting2 automaattisesti käytössä. 

>[AZURE.NOTE] Vain, jotka ovat perus lisätä edellä vaiheessa SQL Server-palvelimen käytettävyys-ryhmät ovat käytettävissä lisääminen sivuston palauttaminen. Jos olet tehnyt käytettävyys ryhmä-ensisijainen SQL Server tai jos olet lisännyt useita käytettävyys ryhmät SQL Serverin sen jälkeen, kun se on lisätty, voit päivittää työkirjan käytettävissä päivitys-vaihtoehtoa käytettäessä SQL Server.

#### <a name="step-3-create-a-recovery-plan"></a>Vaihe 3: Luo palautus suunnitelma

Seuraavaksi palautus Suunnittele näennäiskoneiden ja käytettävyyden ryhmät. Valitse lähde-ja Microsoft Azure kohteeksi samassa VMM palvelimessa tai määrityspalvelimen, jota käytit vaiheessa 1.

![Luo suunnitelma palauttaminen](./media/site-recovery-sql/create-rp1.png)

![Luo suunnitelma palauttaminen](./media/site-recovery-sql/create-rp2.png)

Esimerkissä Sharepoint-sovelluksen koostuu 3 näennäiskoneiden, joka käyttää SQL käytettävyys ryhmän sen Taustajärjestelmä. Tämä palautus-suunnitelmassa on voinut valita molemmat käytettävyys ryhmän sekä virtuaalikoneen, jotka muodostavat sovellus. 

Voit mukauttaa palautus-suunnitelma siirtämällä näennäiskoneiden eri automaattisesti ryhmiin automaattisesti järjestyksen valmistautuminen. Käytettävyys ryhmän aina epäonnistui ensimmäisen päälle, koska se voidaan käyttää minkä tahansa sovelluksen taustassa. 

![Mukauta palautus suunnitelma](./media/site-recovery-sql/customize-rp.png)

### <a name="step-4--fail-over"></a>Vaihe 4: Epäonnistua päälle

Eri automaattisesti asetukset ovat käytettävissä, kun käytettävyys-ryhmä on lisätty palautus suunnitteleminen.

Automaattisesti | Tiedot
--- | ---
**Suunniteltu automaattisesti** | Suunniteltu automaattisesti tarkoittaa sitä ei ole tietojen menetyksen automaattisesti. Voit tehostaa SQL käytettävyys ryhmän käytettävyys tila on ensin määritettävä synkroninen, ja sitten automaattisesti käynnistyy, jotta voin annettu lisäämällä käytettävyys-ryhmän palauttaminen virtuaalikoneen käytettävyys-ryhmän ensisijainen. Kun vikasietotilaa on valmis, käytettävyys tila on määritetty saman arvon sellaisena kuin se oli, ennen suunniteltuja vikasietotilaa käynnistetty.
**Suunnittelematon automaattisesti** | Suunnittelematon automaattisesti voi aiheuttaa tietojen menettämisen kyselyjä. Kun käynnistävä suunnittelematon automaattisesti käytettävyys ryhmän käytettävyys-tila ei ole muutettu ja se tehdään ensisijainen voin annettu lisäämällä käytettävyys-ryhmän palauttaminen virtuaalikoneen. Kun suunnittelematon automaattisesti on suoritettu ja SQL Serveriä paikallisen palvelin on jälleen käytettävissä, käänteinen replikoinnin on saatu käytettävyys-ryhmä. Huomaa, että tämä toiminto ei ole käytettävissä palautus osalta ja se tulee SQL käytettävyys ryhmän SQL Server-välilehdessä
**Testaa automaattisesti** | Testaa automaattisesti SQL käytettävyys ryhmän ei tueta. Jos testi automaattisesti, palautus-suunnitelma sisältävä SQL käytettävyys ryhmän käynnistäminen, automaattisesti ohittaa käytettävyys ryhmän.


Harkitse automaattisesti vaihtoehdoista.

Vaihtoehto | Tiedot
--- | ---
**Vaihtoehto 1** | 1. Suorita testi-automaattisesti sovelluksen ja edusta tasoa.<br/><br/>2. Päivitä sovelluksen tason käyttämään replikan kopion vain luku-tilassa ja testata sovelluksen vain luku-tilassa.
**Vaihtoehto 2** | 1. luoda kopion replikan SQL Server-virtuaalikoneen esiintymän (joko VMM Kloonaa sivuston sivuston tai Azure varmuuskopiointi) ja noutaa sen testi verkossa<br/><br/> 2. Suorita testi vikasietotilaa jotakin palautus-palvelupakettia.

Vaihe 5: Epäonnistua takaisin

Jos haluat tehdä käytettävyys-ryhmän uudelleen ensisijainen paikallisen SQL Serverin jälkeen voit tehdä käynnistävä suunniteltu automaattisesti käyttöön palautus suunnitteleminen ja valitsemalla suunta Microsoft Azure paikallisen VMM palvelimeen.

>[AZURE.NOTE] Kun käänteinen suunnittelematon automaattisesti-replikointi on aina, kun haluat jatkaa replikointi käytettävyys-ryhmä. Kunnes tehdään replikointi pysyy keskeytetty.



### <a name="protect-machines-without-a-vmm-server-or-a-configuration-server"></a>Suojaa koneet ilman VMM palvelimeen tai Configuration-palvelin

Ympäristössä, joita ei ole hallitaan VMM-palvelimeen tai määritysten palvelimeen Azure automaatio Runbooks voidaan määrittää komentosarjaan määritetty automaattisesti SQL käytettävyys ryhmien. Alla on ohjeet, joka määrittää:

1.  Luo paikallisen tiedoston komentosarjan käytettävyys-ryhmästä epäonnistuu. Esimerkki-komentosarja määrittää käytettävyys-ryhmän polku Azure se ja epäonnistuu päälle replikan esiintymää. Tämä komentosarja suoritetaan SQL Serverin replikan virtuaalikoneen välittämällä on mukautettu komentosarja-tunniste.

        Param(
        [string]$SQLAvailabilityGroupPath
        )
        import-module sqlps
        Switch-SqlAvailabilityGroup -Path $SQLAvailabilityGroupPath -AllowDataLoss -force

2.  Lataa komentosarja Blob-objektien Azure-tallennustilan tilin. Käytä tässä esimerkissä:

        $context = New-AzureStorageContext -StorageAccountName "Account" -StorageAccountKey "Key"
        Set-AzureStorageBlobContent -Blob "AGFailover.ps1" -Container "script-container" -File "ScriptLocalFilePath" -context $context

3.  Luo Azure automaatio-runbookin käynnistää SQL Server-replikan virtuaalikoneen Azure-komentosarjoja. Esimerkki-komentosarja avulla voit tehdä tämän. [Lisätietoja](site-recovery-runbook-automation.md) käyttämisestä automaatio runbooks palautus-palvelupakettien. 

        workflow SQLAvailabilityGroupFailover
        {
            param (
                [Object]$RecoveryPlanContext
            )

            $Cred = Get-AutomationPSCredential -name 'AzureCredential'
    
            #Connect to Azure
            $AzureAccount = Add-AzureAccount -Credential $Cred
            $AzureSubscriptionName = Get-AutomationVariable –Name ‘AzureSubscriptionName’
            Select-AzureSubscription -SubscriptionName $AzureSubscriptionName
    
            InLineScript
            {
            #Update the script with name of your storage account, key and blob name
            $context = New-AzureStorageContext -StorageAccountName "Account" -StorageAccountKey "Key";
            $sasuri = New-AzureStorageBlobSASToken -Container "script-container"- Blob "AGFailover.ps1" -Permission r -FullUri -Context $context;
     
            Write-output "failovertype " + $Using:RecoveryPlanContext.FailoverType;
               
            if ($Using:RecoveryPlanContext.FailoverType -eq "Test")
                {
                #Skipping TFO in this version.
                #We will update the script in a follow-up post with TFO support
                Write-output "tfo: Skipping SQL Failover";
                }
            else
                {
                Write-output "pfo/ufo";
                #Get the SQL Azure Replica VM.
                #Update the script to use the name of your VM and Cloud Service
                $VM = Get-AzureVM -Name "SQLAzureVM" -ServiceName "SQLAzureReplica";     
       
                Write-Output "Installing custom script extension"
                #Install the Custom Script Extension on teh SQL Replica VM
                Set-AzureVMExtension -ExtensionName CustomScriptExtension -VM $VM -Publisher Microsoft.Compute -Version 1.3| Update-AzureVM; 
                    
                Write-output "Starting AG Failover";
                #Execute the SQL Failover script
                #Pass the SQL AG path as the argument.
       
                $AGArgs="-SQLAvailabilityGroupPath sqlserver:\sql\sqlazureVM\default\availabilitygroups\testag";
       
                Set-AzureVMCustomScriptExtension -VM $VM -FileUri $sasuri -Run "AGFailover.ps1" -Argument $AGArgs | Update-AzureVM;
       
                Write-output "Completed AG Failover";

                }
        
            }
        }

4.  Kun luot "1 käynnistyksen ryhmitellä valmiiksi" komentosarjaan määritetty vaiheen, joka käynnistää epäonnistuu päälle käytettävyys ryhmät automaatio-runbookin lisääminen sovelluksen palautus suunnitteleminen.

## <a name="integrate-protection-with-sql-alwayson-on-premises-to-on-premises"></a>Integroi suojaus SQL AlwaysOn (paikallisen paikalliseen)

Jos SQL Server on käytössä suuren käytettävyyden ryhmät tai vikasietoklusterin esiintymä, on suositeltavaa käytettävyys ryhmien käyttäminen palautus-sivustossa. Huomaa, että nämä ohjeet sovelluksissa, jotka eivät käytä Jaetut tapahtumat.

1. [Määritä tietokantojen](https://msdn.microsoft.com/library/hh213078.aspx) käytettävyys-ryhmiin.
2. Luo uuden virtual verkon toissijaisen.
3. Määritä uusi virtual verkko-ja ensisijainen sivuston sivusto VPN-yhteyttä.
4. Luo virtual machine palautus-sivustosta ja asentaa SQL Server.
5. Laajentaa olemassa olevia AlwaysOn käytettävyys-ryhmiä ja uusi SQL Server-virtuaalikoneen. SQL Server-esiintymän määrittäminen asynkroninen replikan-kopiona.
6. Käytettävyys-ryhmän listener luominen tai päivittäminen sisältämään asynkroninen replikan virtuaalikoneen aiemmin kuuntelua.
7. Varmista, että sovelluksen klusterissa on asennuksen käyttämällä kuuntelua. Jos määrittäminen käyttämällä tietokantapalvelimen nimi, Päivitä sen käyttämistä kuuntelua, jotta sinun ei tarvitse tehdä uudelleen jälkeen vikasietotilaa.

Jaetut tapahtumat käyttävät sovellukset on suositus [Palauttaminen SAN toistot](site-recovery-vmm-san.md) tai [VMWare/fyysisiä palvelimen sivusto sivusto replikoinnin](site-recovery-vmware-to-vmware.md).

### <a name="recovery-plan-considerations"></a>Palautus suunnitelma huomioon otettavia seikkoja

1. Lisää tämä mallikomentosarja VMM kirjastoon ensisijainen ja toissijainen sivustoissa.

        Param(
        [string]$SQLAvailabilityGroupPath
        )
        import-module sqlps
        Switch-SqlAvailabilityGroup -Path $SQLAvailabilityGroupPath -AllowDataLoss -force

2. Kun luot "1 käynnistyksen ryhmitellä valmiiksi" komentosarjaan määritetty vaiheen, joka käynnistää komentosarja epäonnistuu päälle käytettävyys ryhmien lisääminen sovelluksen palautus suunnitteleminen.



## <a name="protect-a-standalone-sql-server"></a>Suojata yksittäisen SQL Server

Tässä määrityksessä Suosittelemme palauttaminen replikoinnin avulla voit suojata SQL Server-tietokoneeseen. Tarkat ohjeet sen mukaan, onko SQL Server on määritetty virtuaalikoneen tai fyysinen palvelimeen, ja haluat replikoida Azure vai toissijainen paikallisen sivuston. Ohjeet [Sivuston palauttaminen yleiskatsaus](site-recovery-overview.md)käyttöönoton kaikissa tilanteissa.


## <a name="protect-a-sql-server-cluster-standard-or-2008-r2"></a>Suojaa SQL Server-klusterin (vakio- tai 2008 R2)

Klusterin SQL Server Standard edition-tai SQL Server 2008 R2 Suosittelemme palauttaminen replikoinnin avulla voit suojata SQL Server.

### <a name="on-premises-to-on-premises"></a>Paikalliseen paikalliseen

- Jos sovellus käyttää jaettua tapahtumat Suosittelemme käyttöönottoa [Palauttaminen SAN toistot](site-recovery-vmm-san.md) Hyper-V-ympäristön ja [VMware/fyysisiä palvelimen VMware](site-recovery-vmware-to-vmware.md) VMware-ympäristössä.

- DTC sovelluksille hyödyntää yllä lähestymistavan palauttaa erilliseksi palvelimeksi klusterin hyödyntäminen paikallisen suuri suojaus DB peilikuva.

### <a name="on-premises-to-azure"></a>Paikalliseen Azure avulla

Sivuston palauttaminen ei tue Vieras Klusterituki, kun replikoiminen Azure. SQL Server myös ei tarjoa edullinen palauttamisratkaisun Standard edition. On suositeltavaa suojaaminen erilliseksi SQL Server-ympäristöön SQL Server-klusterin ja palauttaa Azure-tietokannassa.


1. Voit määrittää muita erillinen SQL Server-esiintymän paikallisen sivuston.
2. Määritä tämä esiintymä yhteyshenkilönä tietokannoille, jotka on suojauksen peilikuva. Määritä peilaukseen suuri suojaus-tilassa.
3.  Sivuston palauttaminen määrittämistä paikallisen sivuston ympäristöön ([Hyper-V](site-recovery-hyper-v-site-to-azure.md) tai [VMware/fyysisiä server](site-recovery-vmware-to-azure-classic.md).
4.  Sivuston palauttaminen replikoinnin avulla voit toistaa uusi SQL Server-esiintymän Azure. Se on suuri suojaus peilikuva kopio ja niin se voidaan synkronoida ensisijainen klusterin, mutta se replikoida Azure käyttämällä palauttaminen replikoinnin.

Seuraavassa kuvassa on kuvattu nämä asetukset.

![Vakio-klusterin](./media/site-recovery-sql/BCDRStandaloneClusterLocal.png)


### <a name="failback-considerations"></a>Tuntisesta huomioon otettavia seikkoja

Saat SQL vakio klustereiden tuntisesta jälkeen suunnittelematon automaattisesti edellyttävät SQL-varmuuskopiointi ja peilikuva-esiintymän palauttaminen alkuperäiseen klusterin ja uudelleenmäärityksen peilikuva.

## <a name="next-steps"></a>Seuraavat vaiheet
[Lue lisää](site-recovery-best-practices.md) siitä, että käytön valmis ottamaan palauttaminen.










 
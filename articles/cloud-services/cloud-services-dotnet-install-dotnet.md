<properties
   pageTitle=".NET asentaminen Cloud-palvelun rooli | Microsoft Azure"
   description="Tässä artikkelissa käsitellään manuaalinen asennus .NET framework Cloud Service-Web-ja työntekijä roolit"
   services="cloud-services"
   documentationCenter=".net"
   authors="thraka"
   manager="timlt"
   editor=""/>

<tags
   ms.service="cloud-services"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/10/2016"
   ms.author="adegeo"/>

# <a name="install-net-on-a-cloud-service-role"></a>.NET asentaminen Cloud-palvelun rooli 

Tässä artikkelissa käsitellään .NET framework asentaminen Cloud Service-Web- ja työntekijä roolit. Näiden ohjeiden avulla voit asentaa .NET 4.6.1 Azure Vieras OS perheen 4. Katso uusimmat tiedot Vieras OS versioiden [Azure Vieras OS julkaisut ja SDK yhteensopivuuden matriisissa](cloud-services-guestos-update-matrix.md).

Asentamisessa .NET Internetin kautta tai työntekijä roolien sisältyy .NET asennuspaketti osana Cloud projektin ja käynnistäminen asennuksen osana roolin käynnistys tehtävät mukaan lukien.  

## <a name="add-the-net-installer-to-your-project"></a>.NET-installer lisääminen projektiin
- Lataa WWW-asennusohjelma haluat asentaa .NET Framework
    - [.NET 4.6.1 web Installer](http://go.microsoft.com/fwlink/?LinkId=671729)
- Web-roolin
  1. Valitse **Ratkaisunhallinnassa**cloud palvelun projektin **roolit** oikealle kohdasta rooli ja valitse **Lisää > uusi kansio**. Luo kansio nimeltä *bin*
  2. Napsauta hiiren kakkospainikkeella **bin** -kansio ja valitse **Lisää > olemassa olevan kohteen**. Valitse .NET installer ja lisää se bin-kansioon.
- Työntekijän roolin
  1. Napsauta hiiren kakkospainikkeella rooli ja valitse **Lisää > olemassa olevan kohteen**. Valitse .NET-asennusohjelma ja lisätä rooliin. 

Näin roolin sisällön kansioon automaattisesti lisätty cloud-palvelupakettiin ja ottaa käyttöön virtuaalikoneen yhdenmukaisia kohtaa lisätä tiedostoja. Toista tämä prosessi Internetin kautta tai työntekijä rooleista pilvipalvelussa sijaitsevaan siten, että kaikki roolit on asennusohjelman kopio.

> [AZURE.NOTE] Asentamista .NET 4.6.1 tehtäväsi pilvipalvelussa, vaikka sovelluksen kohteena .NET 4.6. Azure Vieras OS sisältää päivitykset [3098779](https://support.microsoft.com/kb/3098779) ja [3097997](https://support.microsoft.com/kb/3097997). .NET 4.6 asennuksen päälle nämä päivitykset, jotka saattavat aiheuttaa ongelmia, kun käynnissä .NET-sovellukset, joten sinun on asennettava suoraan .NET 4.6.1 .NET 4.6 sijaan. Lisätietoja on artikkelissa [kt 3118750](https://support.microsoft.com/kb/3118750).

![Roolin sisältö installer-tiedostoja][1]

## <a name="define-startup-tasks-for-your-roles"></a>Määritä oman roolien tehtävät käynnistys
Käynnistyksen tehtävien avulla voit suorittaa toimintoja, ennen kuin roolin käynnistyy. .NET Frameworkin asentamisesta käynnistys-tehtävän osana varmistat, että puitteissa on asennettu jokin sovellus-koodi on suorittaa. Tehtävien Saat lisätietoja käynnistettäessä: [Azure käynnistys tehtävien suorittaminen](cloud-services-startup-tasks.md). 

1. Lisää seuraavat *ServiceDefinition.csdef* tiedostoon **WebRole** tai **WorkerRole** solmun rooleista:
    
    ```xml
    <LocalResources>
      <LocalStorage name="NETFXInstall" sizeInMB="1024" cleanOnRoleRecycle="false" />
    </LocalResources>    
    <Startup>
      <Task commandLine="install.cmd" executionContext="elevated" taskType="simple">
        <Environment>
          <Variable name="PathToNETFXInstall">
            <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='NETFXInstall']/@path" />
          </Variable>
          <Variable name="ComputeEmulatorRunning">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
          </Variable>
        </Environment>
      </Task>
    </Startup>
    ```

    Yllä määritys toimii console-komennon *install.cmd* ja järjestelmänvalvojan oikeudet, joten se Asenna .NET framework. Kokoonpanon Luo LocalStorage myös nimellä *NETFXInstall*. Käynnistyskomentosarjan määrittää temp-kansion käyttäminen paikallinen tallennus tämän resurssin niin, että .NET framework installer ladattu ja asennettu resurssi. On tärkeää tämän resurssin koon määrittäminen vähintään 1 024 Mt varmistamiseksi puitteissa asennetaan oikein. Lisätietoja käynnistys tehtävät ohjeaiheessa: [yleisiä pilvipalvelussa käynnistys tehtävät](cloud-services-startup-tasks-common.md) 

2. Luo tiedosto **install.cmd** ja lisää se oikea napsautus-roolin rooleista ja valitsemalla **Lisää > aiemmin luotu kohde...**. Jotta kaikki roolit pitäisi olla nyt .NET-asennustiedostoa sekä install.cmd-tiedosto.
    
    ![Roolin sisältö kaikkien tiedostojen kanssa][2]

    > [AZURE.NOTE] Käytä tavallista tekstieditoria, kuten notepadissä tiedoston luomiseen. Jos käytät Visual Studio Luo tekstitiedosto ja määritä sen nimeksi 'cmd.' tiedostossa voi kuitenkin olla UTF-8-tavuinen tilauksen merkki ja ensimmäisen rivin komentosarjan suorittaminen aiheuttaa virheen. Jos Visual Studio avulla voit luoda tiedoston Poistu Lisää REM (Huomautus) tiedoston ensimmäisen rivin niin, että se ohitetaan suoritettaessa. 

3. Lisää seuraavaa komentosarjaa **install.cmd** -tiedostoon:

    ```
    REM Set the value of netfx to install appropriate .NET Framework. 
    REM ***** To install .NET 4.5.2 set the variable netfx to "NDP452" *****
    REM ***** To install .NET 4.6 set the variable netfx to "NDP46" *****
    REM ***** To install .NET 4.6.1 set the variable netfx to "NDP461" *****
    REM ***** To install .NET 4.6.2 set the variable netfx to "NDP462" *****
    set netfx="NDP461"
    
    REM ***** Set script start timestamp *****
    set timehour=%time:~0,2%
    set timestamp=%date:~-4,4%%date:~-10,2%%date:~-7,2%-%timehour: =0%%time:~3,2%
    set "log=install.cmd started %timestamp%."
    
    REM ***** Exit script if running in Emulator *****
    if %ComputeEmulatorRunning%=="true" goto exit
    
    REM ***** Needed to correctly install .NET 4.6.1, otherwise you may see an out of disk space error *****
    set TMP=%PathToNETFXInstall%
    set TEMP=%PathToNETFXInstall%
    
    REM ***** Setup .NET filenames and registry keys *****
    if %netfx%=="NDP462" goto NDP462
    if %netfx%=="NDP461" goto NDP461
    if %netfx%=="NDP46" goto NDP46
        set "netfxinstallfile=NDP452-KB2901954-Web.exe"
        set netfxregkey="0x5cbf5"
        goto logtimestamp
    
    :NDP46
    set "netfxinstallfile=NDP46-KB3045560-Web.exe"
    set netfxregkey="0x6004f"
    goto logtimestamp
    
    :NDP461
    set "netfxinstallfile=NDP461-KB3102438-Web.exe"
    set netfxregkey="0x6040e"
    goto logtimestamp
    
    :NDP462
    set "netfxinstallfile=NDP462-KB3151802-Web.exe"
    set netfxregkey="0x60632"
    
    :logtimestamp
    REM ***** Setup LogFile with timestamp *****
    md "%PathToNETFXInstall%\log"
    set startuptasklog="%PathToNETFXInstall%log\startuptasklog-%timestamp%.txt"
    set netfxinstallerlog="%PathToNETFXInstall%log\NetFXInstallerLog-%timestamp%"
    echo %log% >> %startuptasklog%
    echo Logfile generated at: %startuptasklog% >> %startuptasklog%
    echo TMP set to: %TMP% >> %startuptasklog%
    echo TEMP set to: %TEMP% >> %startuptasklog%
    
    REM ***** Check if .NET is installed *****
    echo Checking if .NET (%netfx%) is installed >> %startuptasklog%
    set /A netfxregkeydecimal=%netfxregkey%
    set foundkey=0
    FOR /F "usebackq skip=2 tokens=1,2*" %%A in (`reg query "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full" /v Release 2^>nul`) do @set /A foundkey=%%C
    echo Minimum required key: %netfxregkeydecimal% -- found key: %foundkey% >> %startuptasklog%
    if %foundkey% GEQ %netfxregkeydecimal% goto installed
    
    REM ***** Installing .NET *****
    echo Installing .NET with commandline: start /wait %~dp0%netfxinstallfile% /q /serialdownload /log %netfxinstallerlog%  /chainingpackage "CloudService Startup Task" >> %startuptasklog%
    start /wait %~dp0%netfxinstallfile% /q /serialdownload /log %netfxinstallerlog% /chainingpackage "CloudService Startup Task" >> %startuptasklog% 2>>&1
    if %ERRORLEVEL%== 0 goto installed
        echo .NET installer exited with code %ERRORLEVEL% >> %startuptasklog%   
        if %ERRORLEVEL%== 3010 goto restart
        if %ERRORLEVEL%== 1641 goto restart
        echo .NET (%netfx%) install failed with Error Code %ERRORLEVEL%. Further logs can be found in %netfxinstallerlog% >> %startuptasklog%
    
    :restart
    echo Restarting to complete .NET (%netfx%) installation >> %startuptasklog%
    EXIT /B %ERRORLEVEL%
    
    :installed
    echo .NET (%netfx%) is installed >> %startuptasklog%
    
    :end
    echo install.cmd completed: %date:~-4,4%%date:~-10,2%%date:~-7,2%-%timehour: =0%%time:~3,2% >> %startuptasklog%
    
    :exit
    EXIT /B 0
    ```
        
    Asenna komentosarja tarkistaa, onko määritetty .NET framework-versio on jo asennettu tietokoneeseen tekemällä kyselyn rekisterin. Jos .NET-versio ei ole asennettu sitten .net Web Installer on käynnistetty. Komentosarjan kirjaa ongelmat ja ongelmien *InstallLogs* paikallisen tallennustilaan tallennettuja *startuptasklog-(currentdatetime) .txt* -tiedostoon kaikki tehtävän.

    > [AZURE.NOTE] Komentosarjan edelleen avulla voit asentaa .NET 4.5.2 tai .NET 4.6 jatkuvuuden. Ei tarvitse asentaa .NET 4.5.2 manuaalisesti, sillä se on jo käytössä Azure Vieras OS. Sen sijaan, että asentaminen .NET 4.6 Asenna suoraan .NET 4.6.1 [kt 3118750](https://support.microsoft.com/kb/3118750)vuoksi.
      

## <a name="configure-diagnostics-to-transfer-the-startup-task-logs-to-blob-storage"></a>Vianmääritys ja siirtää käynnistys tehtävän virhelokit blob storage määrittäminen 
Yksinkertaistaa minkä tahansa asennuksen vianmääritys voit määrittää Azure diagnostiikka voit siirtää minkä tahansa lokitiedostojen luoma käynnistyskomentosarjan tai Blob-objektien tallennustilaan .NET asennusohjelma. Tämän menetelmän voit tarkastella lokit vain lataamalla lokitiedostojen Blob-objektien tallennustilaan sijaan tarvitse etätyöpöydän rooliin.

Avaa Diagnostiikka *diagnostics.wadcfgx* ja lisää **kansioita** solmun seuraavasti: 

```xml 
<DataSources>
 <DirectoryConfiguration containerName="netfx-install">
  <LocalResource name="NETFXInstall" relativePath="log"/>
 </DirectoryConfiguration>
</DataSources>
```

Tämä määritetään azure Diagnostiikka *NETFXInstall* resurssin *log* hakemiston olevien tiedostojen siirtämiseen diagnostiikka tallennustilan tilin *netfx Asenna* blob-säilö.

## <a name="deploying-your-service"></a>Palvelun käyttöönotto 
Kun otat käyttöön palvelun käynnistys-tehtävien Suorita ja asenna .NET framework, jos se ei ole vielä asennettu. Oman roolit on varattu-tilan aikana puitteissa asentaa ja jopa uudelleen Jos framework asentaminen edellyttää sitä. 

## <a name="additional-resources"></a>Lisäresursseja

- [.NET Frameworkin asentamisesta][]
- [Toimintaohje: määrittää, mitkä .NET Framework-versiot on asennettu][]
- [.NET Framework-asennusten vianmääritys][]

[Toimintaohje: määrittää, mitkä .NET Framework-versiot on asennettu]: https://msdn.microsoft.com/library/hh925568.aspx
[.NET Frameworkin asentamisesta]: https://msdn.microsoft.com/library/5a4x27ek.aspx
[.NET Framework-asennusten vianmääritys]: https://msdn.microsoft.com/library/hh925569.aspx

<!--Image references-->
[1]: ./media/cloud-services-dotnet-install-dotnet/rolecontentwithinstallerfiles.png
[2]: ./media/cloud-services-dotnet-install-dotnet/rolecontentwithallfiles.png

 

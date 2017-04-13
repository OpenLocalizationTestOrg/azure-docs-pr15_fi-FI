<properties
    pageTitle="PHP Internetin kautta tai työntekijä roolit | Microsoft Azure"
    description="PHP Internetin kautta tai työntekijä roolien luominen Azure pilvipalvelussa ja määrittämisestä PHP runtime opas."
    services=""
    documentationCenter="php"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

#<a name="how-to-create-php-web-and-worker-roles"></a>Miten voit luoda PHP Internetin kautta tai työntekijä roolit

## <a name="overview"></a>Yleiskatsaus

Tässä oppaassa kerrotaan, kuinka PHP verkossa tai työntekijä roolien luominen Windowsin kehitysympäristö, valitse PHP tietyn version käytettävissä "sisäiset-versioista, muuttaa PHP määritystä, laajennusten ottaminen käyttöön ja Azure-käyttöön. Tässä artikkelissa myös verkossa tai työntekijän rooli, jotta voit käyttää PHP Runtimen (Mukautettu määritys ja laajennukset), jonka annat määrittämisestä.

## <a name="what-are-php-web-and-worker-roles"></a>Mitkä ovat PHP Internetin kautta tai työntekijä roolit?

Azure tarjoaa kolme Laske sovellusten mallit: Azure App palvelun Azuren näennäiskoneiden ja Azure pilvipalveluihin. Kaikkien kolmen mallit tukevat PHP. Cloud Services, joka sisältää Internetin kautta tai työntekijä roolit on *ympäristö palveluna (PaaS)*. Sisällä pilvipalveluun web-rooli on erillinen Internet Information Services (IIS) verkkopalvelimen host edusta-WWW-sovelluksiin. Työntekijän rooli suorittamisen asynkroninen, pitkään käynnissä olevien tai pysyvät tehtävät riippumatta käyttäjän toimia tai syöte.

Lisätietoja näistä asetuksista on artikkelissa [Laske isännöintipalvelu Azure vaihtoehtoja](./cloud-services/cloud-services-choose-me.md).

## <a name="download-the-azure-sdk-for-php"></a>Lataa Azure SDK PHP

[Azure SDK PHP] koostuu useita osia. Tässä artikkelissa käytetään kaksi: PowerShellin Azure ja Azure emulaattorit. Nämä kaksi osaa voidaan asentaa Microsoft Web ympäristö Installer kautta. Katso lisätietoja, [miten voit asentaa ja määrittää PowerShellin Azure](powershell-install-configure.md).

## <a name="create-a-cloud-services-project"></a>Cloud Services-projektin luominen

PHP verkossa tai Työntekijä roolin luominen ensimmäiseksi on luotava Azure Serviceen projektin. Azure-palvelu-projekti on looginen säilön verkko- ja työntekijä roolien ja se sisältää projektin [palvelun määritelmä (.csdef)] ja [palvelun määritykset (.cscfg)] -tiedostoja.

Jos haluat luoda uuden Azure Serviceen projektin, suorita PowerShellin Azure järjestelmänvalvojana ja suorita seuraava komento:

    PS C:\>New-AzureServiceProject myProject

Tämä komento luo uusi kansio (`myProject`), johon voit lisätä verkko- ja työntekijä roolit.

## <a name="add-php-web-or-worker-roles"></a>Lisää PHP verkossa tai työntekijä roolit

Voit lisätä projektin PHP web-rooli, suorita seuraava komento-projektin pääkansion sisällä:

    PS C:\myProject> Add-AzurePHPWebRole roleName

Työntekijä-roolin tällä komennolla:

    PS C:\myProject> Add-AzurePHPWorkerRole roleName

> [AZURE.NOTE] `roleName` Parametri on valinnainen. Jos jätetään pois, sen nimeä luodaan automaattisesti. Luotu ensimmäisen web-rooli on `WebRole1`, toinen on `WebRole2`ja niin edelleen. Luotu ensimmäisen työntekijän rooli on `WorkerRole1`, toinen on `WorkerRole2`ja niin edelleen.

## <a name="specify-the-built-in-php-version"></a>Määritä sisäinen PHP-versio

Kun lisäät PHP verkossa tai työntekijän rooli projektiin, määritys projektitiedostoihin muokataan niin, että PHP asennetaan kunkin sovelluksen verkossa tai työntekijä esiintymässä, kun se on otettu käyttöön. Saat PHP, jotka asennetaan oletusarvoisesti version, suorita seuraava komento:

    PS C:\myProject> Get-AzureServiceProjectRoleRuntime

Yllä olevien komentojen tulosteen muistuttaa mitä näkyy alla. Tässä esimerkissä `IsDefault` lippu on määritetty `true` PHP 5.3.17, joka ilmaisee, että se on asennettu oletusversio PHP varten.

    Runtime Version     PackageUri                      IsDefault
    ------- -------     ----------                      ---------
    Node 0.6.17         http://nodertncu.blob.core...   False
    Node 0.6.20         http://nodertncu.blob.core...   True
    Node 0.8.4          http://nodertncu.blob.core...   False
    IISNode 0.1.21      http://nodertncu.blob.core...   True
    Cache 1.8.0         http://nodertncu.blob.core...   True
    PHP 5.3.17          http://nodertncu.blob.core...   True
    PHP 5.4.0           http://nodertncu.blob.core...   False

Voit määrittää PHP suorituksenaikainen versio PHP versioita, jotka on lueteltu. Jos esimerkiksi haluat asettaminen PHP (roolin nimellä `roleName`), 5.4.0, käytä seuraavaa komentoa:

    PS C:\myProject> Set-AzureServiceProjectRole roleName php 5.4.0

> [AZURE.NOTE] Käytettävissä PHP versioita voi muuttua myöhemmin.

## <a name="customize-the-built-in-php-runtime"></a>Valmiin PHP runtime mukauttaminen

Sinulla on täydet oikeudet, johon on asennettu, kun noudattamalla edellä myös muuttamisesta PHP runtime määritysten `php.ini` asetukset ja tiedostotunnisteiden ottaminen käyttöön.

Voit mukauttaa valmiin PHP runtime, toimimalla seuraavasti:

1. Lisää uusi kansio, jonka nimi on `php`, `bin` kansion web-roolin. Työntekijä-roolin lisää se on rooli pääkansioon.
2. : `php` Kansion luominen toiseen kansioon, jota kutsutaan `ext`. Sijoita `.dll` tunniste-tiedostot (esimerkiksi `php_mongo.dll`), jonka haluat ottaa käyttöön tähän kansioon.
3. Lisää `php.ini` tiedosto `php` kansio. Ota käyttöön kaikki mukautetut tunnisteet ja määritä minkä tahansa PHP direktiivejä tässä tiedostossa. Esimerkiksi, jos haluat ottaa `display_errors` - ja ota käyttöön `php_mongo.dll` tunniste, sisällön lisääminen `php.ini` tiedoston olisi seuraavasti:

        display_errors=On
        extension=php_mongo.dll

> [AZURE.NOTE] Asetuksia, jotka eivät erikseen asettaminen `php.ini` tiedostoa, sinun on annettava automaattisesti voi määrittää oletusarvojen mukaisiksi. Kuitenkin ottaa huomioon, että voit lisätä täydellinen `php.ini` tiedosto.

## <a name="use-your-own-php-runtime"></a>Käytä omia PHP runtime
Joissakin tapauksissa sijaan valitsemalla valmiin PHP runtime ja määrität sen edellä kuvatulla haluat ehkä säätää oman PHP runtime. Esimerkiksi voit verkossa tai Työntekijä-roolin, voit käyttää omaa kehitysympäristö saman PHP runtime. Tämä on helppo varmistaa, että sovellus ei muutu toiminta tuotantoympäristössä.

### <a name="configure-a-web-role-to-use-your-own-php-runtime"></a>Määrittää WWW-roolin käyttämään omaa PHP runtime

Voit määrittää WWW-roolin käyttämään PHP runtime, joka saadaan, toimi seuraavasti:

1. Azure Serviceen projektin luominen ja lisää PHP web rooli tämän artikkelin ohjeiden aiemmin.
2. Luo `php` kansioon `bin` kansio, joka on web-roolin pääkansio ja lisää sitten oman PHP Runtimen (kaikki binaaritiedostot tiedostojen alikansiot, jne.) `php` kansio.
3. (VALINNAINEN) Jos PHP runtime on käytössä [Microsoft SQL Server PHP ohjaimet][sqlsrv drivers], voit määrittää WWW-roolin asentaa [SQL Server Native Client 2012] tarvitset[ sql native client] kun se on valmisteltu. Lisää [sqlncli.msi x64 installer] `bin` web-roolin pääkansion kansioon. Seuraavan vaiheen ohjeiden mukaisesti käynnistyskomentosarja suoritetaan asennusohjelma äänettömästi, kun rooli on valmisteltu. Jos PHP runtime ei käytä PHP Drivers Microsoft SQL Server, voit poistaa seuraava rivi näkyvät seuraavassa vaiheessa komentosarja:

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

4. Määritä käynnistys-tehtävä, joka määrittää [Internet Information Services (IIS)] [ iis.net] Käsittele pyyntöjä PHP Runtimen avulla `.php` sivuja. Voit tehdä tämän Avaa `setup_web.cmd` tiedosto (- `bin` web-roolin pääkansion-tiedosto) tekstieditorissa ja korvata sen sisällön seuraavaa komentosarjaa:

        @ECHO ON
        cd "%~dp0"

        if "%EMULATED%"=="true" exit /b 0

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

        SET PHP_FULL_PATH=%~dp0php\php-cgi.exe
        SET NEW_PATH=%PATH%;%RoleRoot%\base\x86

        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+"[fullPath='%PHP_FULL_PATH%',maxInstances='12',idleTimeout='60000',activityTimeout='3600',requestTimeout='60000',instanceMaxRequests='10000',protocol='NamedPipe',flushNamedPipe='False']" /commit:apphost
        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+"[fullPath='%PHP_FULL_PATH%'].environmentVariables.[name='PATH',value='%NEW_PATH%']" /commit:apphost
        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+"[fullPath='%PHP_FULL_PATH%'].environmentVariables.[name='PHP_FCGI_MAX_REQUESTS',value='10000']" /commit:apphost
        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/handlers /+"[name='PHP',path='*.php',verb='GET,HEAD,POST',modules='FastCgiModule',scriptProcessor='%PHP_FULL_PATH%',resourceType='Either',requireAccess='Script']" /commit:apphost
        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /"[fullPath='%PHP_FULL_PATH%'].queueLength:50000"

5. Sovelluksen tiedostojen lisääminen web-roolin pääkansioon. Tämä on web-palvelimen pääkansioon.

6. Julkaise sovelluksesi [Julkaise sovelluksesi](#how-to-publish-your-application) -kohdassa alla kuvatulla tavalla.

> [AZURE.NOTE] `download.ps1` Komentosarjan (- `bin` web-roolin pääkansion kansion) voi poistaa, kun olet suorittanut ohjatun oman PHP runtime edellä kuvatut vaiheet.

### <a name="configure-a-worker-role-to-use-your-own-php-runtime"></a>Käyttää omia PHP runtime Työntekijä roolin määrittäminen

Voit määrittää työntekijän rooli käyttämään PHP runtime, joka saadaan, toimi seuraavasti:

1. Azure Serviceen projektin luominen ja lisää PHP työntekijän rooli tämän artikkelin ohjeiden aiemmin.
2. Luo `php` Työntekijä-roolin pääkansion kansioon ja lisää sitten oman PHP Runtimen (kaikki binaaritiedostot tiedostojen alikansiot, jne.) `php` kansio.
3. (VALINNAINEN) Jos PHP runtime käyttää [Microsoft SQL Server PHP ohjaimet][sqlsrv drivers], sinun on määrittäminen työntekijä tehtäväsi asentaa [SQL Server Native Client 2012] [ sql native client] kun se on valmisteltu. Voit tehdä tämän Lisää [sqlncli.msi x64 installer] Työntekijä-roolin pääkansioon. Seuraavan vaiheen ohjeiden mukaisesti käynnistyskomentosarja suoritetaan asennusohjelma äänettömästi, kun rooli on valmisteltu. Jos PHP runtime ei käytä PHP Drivers Microsoft SQL Server, voit poistaa seuraava rivi näkyvät seuraavassa vaiheessa komentosarja:

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

4. Määritä käynnistys-tehtävä, joka lisää oman `php.exe` suoritettavan Työntekijä-roolin PATH-ympäristömuuttuja kun rooli on valmistelun yhteydessä. Voit tehdä tämän avaamalla `setup_worker.cmd` -tiedoston (työntekijän rooli pääkansion) tekstieditorissa ja korvata sen sisällön seuraavaa komentosarjaa:

        @echo on

        cd "%~dp0"

        echo Granting permissions for Network Service to the web root directory...
        icacls ..\ /grant "Network Service":(OI)(CI)W
        if %ERRORLEVEL% neq 0 goto error
        echo OK

        if "%EMULATED%"=="true" exit /b 0

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

        setx Path "%PATH%;%~dp0php" /M

        if %ERRORLEVEL% neq 0 goto error

        echo SUCCESS
        exit /b 0

        :error

        echo FAILED
        exit /b -1

5. Lisää sovellus tiedostojen Työntekijä-roolin pääkansioon.

6. Julkaise sovelluksesi [Julkaise sovelluksesi](#how-to-publish-your-application) -kohdassa alla kuvatulla tavalla.

## <a name="run-your-application-in-the-compute-and-storage-emulators"></a>Suorita sovellus suorittaminen ja tallennustilaa emulointeja

Azure emulointeja on paikallisessa ympäristössä, jossa voit testata Azure sovelluksen ennen niiden käyttöönottoa pilveen. Eroja emulointeja ja Azure ympäristön välillä. Tämä paremmin on artikkelissa [Azure tallennustilan emulointikone kehittämiseen käyttäminen](./storage/storage-use-emulator.md).

Huomaa, että sinulla on oltava PHP paikallisesti, jotta voit käyttää Laske emulaattori. Laske emulaattori suorittamaan sovelluksesi paikallisen PHP-asennuksen avulla.

Suorittamaan projektin emulointeja Suorita projektin pääkansiosta seuraava komento:

    PS C:\MyProject> Start-AzureEmulator

Tällainen tulos tulee näkyviin:

    Creating local package...
    Starting Emulator...
    Role is running at http://127.0.0.1:81
    Started

Näet sovelluksen käynnissä emulaattori avaamalla selaimen ja tuloksissa paikallinen osoite selaamalla (`http://127.0.0.1:81` edellä olevassa esimerkissä tuloksissa).

Lopeta emulointeja, suorita tämä komento:

    PS C:\MyProject> Stop-AzureEmulator

## <a name="publish-your-application"></a>Sovelluksen julkaiseminen

Voit julkaista sovelluksesi, sinun täytyy ensin tuoda oman Julkaisuasetukset [Tuo AzurePublishSettingsFile](https://msdn.microsoft.com/library/azure/dn790370.aspx) cmdlet-komennolla. Voit julkaista sovelluksesi [Julkaise AzureServiceProject](https://msdn.microsoft.com/library/azure/dn495166.aspx) cmdlet-komennolla. Lisätietoja kirjautumisesta on artikkelissa [miten voit asentaa ja määrittää PowerShellin Azure](powershell-install-configure.md).

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja on artikkelissa [PHP Developer Center](/develop/php/).

[Azure SDK PHP]: /develop/php/common-tasks/download-php-sdk/
[install ps and emulators]: http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409
[palvelun määritys (.csdef)]: http://msdn.microsoft.com/library/windowsazure/ee758711.aspx
[palvelun määritykset (.cscfg)]: http://msdn.microsoft.com/library/windowsazure/ee758710.aspx
[iis.net]: http://www.iis.net/
[sql native client]: http://msdn.microsoft.com/sqlserver/aa937733.aspx
[sqlsrv drivers]: http://php.net/sqlsrv
[sqlncli.msi x64 installer]: http://go.microsoft.com/fwlink/?LinkID=239648

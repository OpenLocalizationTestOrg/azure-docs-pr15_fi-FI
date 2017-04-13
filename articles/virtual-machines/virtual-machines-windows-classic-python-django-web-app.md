<properties
    pageTitle="Python web app, jossa Django | Microsoft Azure"
    description="Tässä opetusohjelmassa avulla opit isännöidä Django perustuva verkkosivustoa Azure Windows Server 2012 R2 palvelinkeskuksen virtual machine-perinteinen käyttöönoton mallin avulla."
    services="virtual-machines-windows"
    documentationCenter="python"
    authors="huguesv"
    manager="wpickett"
    editor=""
    tags="azure-service-management"/>


<tags 
    ms.service="virtual-machines-windows" 
    ms.workload="web" 
    ms.tgt_pltfrm="vm-windows" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="08/04/2015" 
    ms.author="huvalo"/>


# <a name="django-hello-world-web-application-on-a-windows-server-vm"></a>Windows Server-AM Django Hei maailma WWW-sovellusta

> [AZURE.SELECTOR]
- [Windows](virtual-machines-windows-classic-python-django-web-app.md)
- [Mac tai Linux](virtual-machines-linux-python-django-web-app.md)

<br>

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]
 

Tässä opetusohjelmassa kerrotaan, miten Microsoft Azure käyttämällä Windows Server-virtual machine Django perustuva sivustossa isännöimiseen. Tässä opetusohjelmassa oletetaan, että ei ole edellisen kokemus Azure avulla. Kun olet suorittanut Tässä opetusohjelmassa, sinun on Django-sovellukseen ylös- ja suorittamalla pilveen.

Opit, miten voit:

* Määritä Azure virtual-koneen isäntään Django. Kun tässä opetusohjelmassa kerrotaan, miten voit tehdä tämän Windows Server-kohdassa, sama voi myös tehdä Linux ylläpidettävä Azure AM.
* Luo uusi Django-sovellus Windows.

Tässä opetusohjelmassa seuraamalla voit luoda yksinkertaisen Hei maailma web-sovelluksen. Sovelluksen sovelluksessa Azure virtuaalikoneen.

Näyttökuva valmiin sovelluksen näyttöön tulee seuraava.

![Hei maailma sivun näyttäminen Azure selainikkunassa.][1]

[AZURE.INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## <a name="creating-and-configuring-an-azure-virtual-machine-to-host-django"></a>Luominen ja määrittäminen Azure virtual-koneen isäntään Django

1. Noudata annettu [tähän](virtual-machines-windows-classic-tutorial.md) luominen Azure virtuaalikoneen, Windows Server 2012 R2 palvelinkeskuksen-jakauman.

1. Opasta Azure ohjaamaan portti 80 liikenne Internetistä virtuaalikoneen 80 porttiin:
 - Siirry Azure perinteinen portaalissa juuri luomasi virtuaalikoneen ja **PÄÄTEPISTEET** -välilehti.
 - Valitse näytön alalaidassa olevaa **Lisää** -painiketta.
    ![Lisää päätepiste](./media/virtual-machines-windows-classic-python-django-web-app/django-helloworld-addendpoint.png)

 - **TCP** -protokollaa avaamista on **julkinen PORTTIIN 80** kuin **YKSITYISTÄ PORTTIIN 80**.
![][port80]
1. Valitse **Yhdistä** **Etätyöpöydän** käyttäminen kirjautumiseen etäyhteyden juuri luomasi Azure virtuaalikoneen **KOONTINÄYTTÖ** -välilehti.  

**Tärkeä huomautus:** Alla olevia ohjeita oletetaan, että olet kirjautunut virtuaalikoneen oikein ja on myöntänyt komennot on paikallinen kone sijaan.

## <a id="setup"> </a>Python, Django, WFastCGI asentaminen

**Huomautus:** Jotta voit ladata Internet Explorerissa IE ESC asetuksia on ehkä (Käynnistä/järjestelmänvalvojan Työkalut/palvelin hallinta ja paikallinen palvelin, valitse **Internet Explorerin suojaus-parannetun**, poissa käytöstä).

1. Asenna uusimmat Python 2.7 tai 3.4 [python.org][].
1. Asenna käyttämällä pip wfastcgi ja django paketit.

    Käytä seuraavaa komentoa Python 2.7.

        c:\python27\scripts\pip install wfastcgi
        c:\python27\scripts\pip install django

    Käytä seuraavaa komentoa Python 3.4.

        c:\python34\scripts\pip install wfastcgi
        c:\python34\scripts\pip install django

## <a name="installing-iis-with-fastcgi"></a>IIS-asennuksen FastCGI

1. Asenna IIS FastCGI tukeen.  Tämä saattaa kestää useita minuutteja suorittamiseen.

        start /wait %windir%\System32\PkgMgr.exe /iu:IIS-WebServerRole;IIS-WebServer;IIS-CommonHttpFeatures;IIS-StaticContent;IIS-DefaultDocument;IIS-DirectoryBrowsing;IIS-HttpErrors;IIS-HealthAndDiagnostics;IIS-HttpLogging;IIS-LoggingLibraries;IIS-RequestMonitor;IIS-Security;IIS-RequestFiltering;IIS-HttpCompressionStatic;IIS-WebServerManagementTools;IIS-ManagementConsole;WAS-WindowsActivationService;WAS-ProcessModel;WAS-NetFxEnvironment;WAS-ConfigurationAPI;IIS-CGI

## <a name="creating-a-new-django-application"></a>Uuden Django-sovelluksen luominen

1.  - *C:\inetpub\wwwroot*Kirjoita seuraava komento Django uuden projektin luominen:

    Käytä seuraavaa komentoa Python 2.7.

        C:\Python27\Scripts\django-admin.exe startproject helloworld

    Käytä seuraavaa komentoa Python 3.4.

        C:\Python34\Scripts\django-admin.exe startproject helloworld

    ![Uusi AzureService-komennon tuloksen](./media/virtual-machines-windows-classic-python-django-web-app/django-helloworld-cmd-new-azure-service.png)

1.  **Django järjestelmänvalvoja** -komento luo perusrakenteen Django perustuva sivustot:

  -   **helloworld\manage.PY** avulla voit isännöinnin aloittaa ja lopettaa Django perustuva sivustosi
  -   **helloworld\helloworld\settings.PY** sisältää sovelluksen Django asetuksia.
  -   **helloworld\helloworld\urls.PY** on yhdistäminen kunkin URL-osoite ja sen näkymän välillä.

1.  Luo uusi tiedosto nimeltä **views.py** *C:\inetpub\wwwroot\helloworld\helloworld* hakemistossa. Tämä sisältää näkymän, joka hahmontaa "Hei"-sivu. Käynnistä editorin ja syötä seuraavat:

        from django.http import HttpResponse
        def home(request):
            html = "<html><body>Hello World!</body></html>"
            return HttpResponse(html)

1.  Korvaa urls.py-tiedoston sisällön seuraavasti.

        from django.conf.urls import patterns, url
        urlpatterns = patterns('',
            url(r'^$', 'helloworld.views.home', name='home'),
        )

## <a name="configuring-iis"></a>IIS: N määrittäminen

1. Poista Yleiset applicationhost.config käsittelytoimintoja-osan lukitus.  Tämä ottaa käyttöön oman Web.config python käsittelijä käyttö.

        %windir%\system32\inetsrv\appcmd unlock config -section:system.webServer/handlers

1. Ota käyttöön WFastCGI.  Tämä lisää sovelluksen yleinen applicationhost.config, joka viittaa oman Python kääntäjän suoritettavan ja wfastcgi.py komentosarja.

    Python 2.7:

        c:\python27\scripts\wfastcgi-enable

    Python 3.4:

        c:\python34\scripts\wfastcgi-enable

1. Luo *C:\inetpub\wwwroot\helloworld*web.config-tiedosto.  Arvo `scriptProcessor` määritettä on vastattava edellisessä vaiheessa tulos.  Artikkelissa on wfastcgi asetusten pypi [wfastcgi][] sivu.

    Python 2.7:

        <configuration>
          <appSettings>
            <add key="WSGI_HANDLER" value="django.core.handlers.wsgi.WSGIHandler()" />
            <add key="PYTHONPATH" value="C:\inetpub\wwwroot\helloworld" />
            <add key="DJANGO_SETTINGS_MODULE" value="helloworld.settings" />
          </appSettings>
          <system.webServer>
            <handlers>
                <add name="Python FastCGI" path="*" verb="*" modules="FastCgiModule" scriptProcessor="C:\Python27\python.exe|C:\Python27\Lib\site-packages\wfastcgi.pyc" resourceType="Unspecified" />
            </handlers>
          </system.webServer>
        </configuration>

    Python 3.4:

        <configuration>
          <appSettings>
            <add key="WSGI_HANDLER" value="django.core.handlers.wsgi.WSGIHandler()" />
            <add key="PYTHONPATH" value="C:\inetpub\wwwroot\helloworld" />
            <add key="DJANGO_SETTINGS_MODULE" value="helloworld.settings" />
          </appSettings>
          <system.webServer>
            <handlers>
                <add name="Python FastCGI" path="*" verb="*" modules="FastCgiModule" scriptProcessor="C:\Python34\python.exe|C:\Python34\Lib\site-packages\wfastcgi.py" resourceType="Unspecified" />
            </handlers>
          </system.webServer>
        </configuration>

1. Päivitä sijainti, IIS oletus Web-sivuston django project-kansio.

        %windir%\system32\inetsrv\appcmd set vdir "Default Web Site/" -physicalPath:"C:\inetpub\wwwroot\helloworld"

1. Lataa-web-sivua selaimessa.

![Hei maailma sivun näyttäminen Azure selainikkunassa.][1]


## <a name="shutting-down-your-azure-virtual-machine"></a>Azure virtuaalikoneen suljetaan

Kun enää tarvitse Tässä opetusohjelmassa, sulkea ja/tai poistaa juuri luomasi Azure virtuaalikoneen vapauttaa resursseja muiden hakeminen ja välttää Azure käyttö lisäkustannuksia.

[1]: ./media/virtual-machines-windows-classic-python-django-web-app/django-helloworld-browser-azure.png

[port80]: ./media/virtual-machines-windows-classic-python-django-web-app/django-helloworld-port80.png

[Web Platform Installer]: http://www.microsoft.com/web/downloads/platform.aspx
[Python.org]: https://www.python.org/downloads/
[wfastcgi]: https://pypi.python.org/pypi/wfastcgi

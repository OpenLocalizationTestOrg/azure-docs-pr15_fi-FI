<properties 
    pageTitle="Määrittäminen Python Azure App palvelun Web Apps-sovellusten kanssa" 
    description="Tässä opetusohjelmassa kuvataan yhtä aikaa muiden kanssa ja basic verkkopalvelimen yhdyskäytävän Interface (WSGI)-yhteensopivan Python-sovelluksen määrittäminen Azure palvelun Web sovellukset." 
    services="app-service" 
    documentationCenter="python" 
    tags="python"
    authors="huguesv" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="02/26/2016" 
    ms.author="huvalo"/>




# <a name="configuring-python-with-azure-app-service-web-apps"></a>Määrittäminen Python Azure App palvelun Web Apps-sovellusten kanssa

Tässä opetusohjelmassa kuvataan yhtä aikaa muiden kanssa ja basic Web Server Gateway Interface (WSGI) yhteensopivan Python-sovelluksen määrittäminen [Azure palvelun Web sovellukset](http://go.microsoft.com/fwlink/?LinkId=529714).

Se kuvaa Git käyttöönoton, kuten virtual ympäristön ja paketin asennuksen requirements.txt ominaisuuksia.


## <a name="bottle-django-or-flask"></a>Pullot, Django tai: N?

Azure Marketplace sisältää pullot, Django ja: N puitteissa malleja. Jos ensimmäinen koodiin Azure-sovelluksen palvelun kehittävät tai et ole aiemmin käyttänyt Git, on suositeltavaa noudattamalla jokin näissä Opetusohjelmissa, jotka sisältävät vaiheittaiset ohjeet etsimisen toimimasta-sovelluksen käyttäminen Git käyttöönoton Windows- tai Mac-valikoimasta:

- [Pullot web Apps-sovellusten luominen](web-sites-python-create-deploy-bottle-app.md)
- [Django web Apps-sovellusten luominen](web-sites-python-create-deploy-django-app.md)
- [Web Apps-sovellusten luominen: N avulla](web-sites-python-create-deploy-flask-app.md)


## <a name="web-app-creation-on-azure-portal"></a>Web-sovelluksen luominen Azure-portaalissa

Tässä opetusohjelmassa olettaa Azure olemassa oleva tilaus ja Azure-portaalissa käyttöoikeudet.

Jos sinulla ei ole olemassa web app-sovelluksessa, voit luoda yhden [Azure-portaalissa](https://portal.azure.com).  Vasemmassa yläkulmassa uusi-painiketta ja valitse sitten **Web + Mobile** > **Web Appissa**.

## <a name="git-publishing"></a>Git julkaiseminen

Määritä Git julkaiseminen juuri luomasi web-sovelluksen [Paikallisen Git käyttöönoton Azure sovelluksen-palveluun](app-service-deploy-local-git.md)osoitteessa ohjeita noudattamalla. Tässä opetusohjelmassa käytetään Git luominen, hallita ja julkaista Python Microsoftin online Azure-sovelluksen palveluun.

Kun Git julkaisu on määritetty, Git säilö luodaan ja web-sovellukseen kytketty. Säilön URL-osoite näkyy ja voidaan vastedes viemään tietoja paikallisen kehitysympäristö pilveen. Voit julkaista sovellusten Git kautta, varmista, että Git-asiakasohjelma ja käyttämällä push web app-sisällön Azure-sovelluksen palveluun annettuja ohjeita.


## <a name="application-overview"></a>Sovellusten yleiskuvaus

Seuraavien osien luodaan seuraavat tiedostot. Ne sijoitetaan Git säilö pääsivustoon.

    app.py
    requirements.txt
    runtime.txt
    web.config
    ptvs_virtualenv_proxy.py


## <a name="wsgi-handler"></a>WSGI käsittely

WSGI on Python kuvaaman [PEP 3333](http://www.python.org/dev/peps/pep-3333/) määrittäminen liittymän, web-palvelin ja Python välillä. Se on standardoitu käyttöliittymän eri web-sovellusten ja kehysten käyttäminen Python kirjoittamisesta. Suositut Python web kehysten käyttäminen tänään WSGI. Azure palvelun Web sovellukset antaa tällaisia kehysten; tuki Lisäksi kokeneille käyttäjille voi myös Luo omia, kunhan mukautetun käsittelyn seuraa WSGI määrityksen ohjeita.

Tässä on esimerkki `app.py` , joka määrittää mukautetun käsittelyn:

    def wsgi_app(environ, start_response):
        status = '200 OK'
        response_headers = [('Content-type', 'text/plain')]
        start_response(status, response_headers)
        response_body = 'Hello World'
        yield response_body.encode()

    if __name__ == '__main__':
        from wsgiref.simple_server import make_server

        httpd = make_server('localhost', 5555, wsgi_app)
        httpd.serve_forever()

Voit suorittaa tämän sovelluksen kanssa paikallisesti `python app.py`, siirry `http://localhost:5555` selaimessa.


## <a name="virtual-environment"></a>Virtuaalinen ympäristössä

Edellä olevassa esimerkissä sovellus ei edellytä mitään ulkoisen pakettien, mutta se on todennäköisesti, että sovelluksesi edellyttää joitakin.

Ulkoisen paketin riippuvuudet hallinta helpottuu, Azure Git käyttöönoton tukee virtual ympäristöissä luomista.

Kun Azure havaitsee requirements.txt säilö ylimmällä, se luo automaattisesti nimeltä virtuaalisen ympäristön `env`. Tämä tapahtuu vain ensimmäisen käyttöönoton tai minkä tahansa käyttöönoton jälkeen valitun Python aikana runtime on muuttunut.

Todennäköisesti kannattaa luoda virtuaalisen ympäristön paikallisesti kehitystä varten, mutta älä sisällytä se Git säilöön.


## <a name="package-management"></a>Paketin hallinta

Paketteja, jotka on lueteltu requirements.txt asennetaan automaattisesti käyttämällä pip virtual ympäristössä. Näin tapahtuu jokaisen ympäristöön, mutta pip ohittaa asennus, jos paketti on jo asennettu.

Esimerkki `requirements.txt`:

    azure==0.8.4


## <a name="python-version"></a>Python versio

[AZURE.INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-runtime.md)]

Esimerkki `runtime.txt`:

    python-2.7


## <a name="webconfig"></a>Web.config

Tarvitset web.config-tiedoston, voit määrittää, kuinka palvelimeen Käsittele pyynnöt luomiseen.

Huomaa, että jos web.x.y.config tiedosto säilöön-josta taas x.y vastaa valitun Python runtime ja Azure kopioi automaattisesti web.config haluamasi tiedosto.

Seuraavissa esimerkeissä web.config luottavat näennäisen ympäristön välityspalvelimen komentosarjan, joka on kuvattu seuraavassa osassa.  Esimerkissä käytetään WSGI käsittelijä kanssa toimivat `app.py` yläpuolella.

Esimerkki `web.config` Python 2.7 varten:

    <?xml version="1.0"?>
    <configuration>
      <appSettings>
        <add key="WSGI_ALT_VIRTUALENV_HANDLER" value="app.wsgi_app" />
        <add key="WSGI_ALT_VIRTUALENV_ACTIVATE_THIS"
             value="D:\home\site\wwwroot\env\Scripts\activate_this.py" />
        <add key="WSGI_HANDLER"
             value="ptvs_virtualenv_proxy.get_virtualenv_handler()" />
        <add key="PYTHONPATH" value="D:\home\site\wwwroot" />
      </appSettings>
      <system.web>
        <compilation debug="true" targetFramework="4.0" />
      </system.web>
      <system.webServer>
        <modules runAllManagedModulesForAllRequests="true" />
        <handlers>
          <remove name="Python27_via_FastCGI" />
          <remove name="Python34_via_FastCGI" />
          <add name="Python FastCGI"
               path="handler.fcgi"
               verb="*"
               modules="FastCgiModule"
               scriptProcessor="D:\Python27\python.exe|D:\Python27\Scripts\wfastcgi.py"
               resourceType="Unspecified"
               requireAccess="Script" />
        </handlers>
        <rewrite>
          <rules>
            <rule name="Static Files" stopProcessing="true">
              <conditions>
                <add input="true" pattern="false" />
              </conditions>
            </rule>
            <rule name="Configure Python" stopProcessing="true">
              <match url="(.*)" ignoreCase="false" />
              <conditions>
                <add input="{REQUEST_URI}" pattern="^/static/.*" ignoreCase="true" negate="true" />
              </conditions>
              <action type="Rewrite"
                      url="handler.fcgi/{R:1}"
                      appendQueryString="true" />
            </rule>
          </rules>
        </rewrite>
      </system.webServer>
    </configuration>


Esimerkki `web.config` Python 3.4 varten:

    <?xml version="1.0"?>
    <configuration>
      <appSettings>
        <add key="WSGI_ALT_VIRTUALENV_HANDLER" value="app.wsgi_app" />
        <add key="WSGI_ALT_VIRTUALENV_ACTIVATE_THIS"
             value="D:\home\site\wwwroot\env\Scripts\python.exe" />
        <add key="WSGI_HANDLER"
             value="ptvs_virtualenv_proxy.get_venv_handler()" />
        <add key="PYTHONPATH" value="D:\home\site\wwwroot" />
      </appSettings>
      <system.web>
        <compilation debug="true" targetFramework="4.0" />
      </system.web>
      <system.webServer>
        <modules runAllManagedModulesForAllRequests="true" />
        <handlers>
          <remove name="Python27_via_FastCGI" />
          <remove name="Python34_via_FastCGI" />
          <add name="Python FastCGI"
               path="handler.fcgi"
               verb="*"
               modules="FastCgiModule"
               scriptProcessor="D:\Python34\python.exe|D:\Python34\Scripts\wfastcgi.py"
               resourceType="Unspecified"
               requireAccess="Script" />
        </handlers>
        <rewrite>
          <rules>
            <rule name="Static Files" stopProcessing="true">
              <conditions>
                <add input="true" pattern="false" />
              </conditions>
            </rule>
            <rule name="Configure Python" stopProcessing="true">
              <match url="(.*)" ignoreCase="false" />
              <conditions>
                <add input="{REQUEST_URI}" pattern="^/static/.*" ignoreCase="true" negate="true" />
              </conditions>
              <action type="Rewrite" url="handler.fcgi/{R:1}" appendQueryString="true" />
            </rule>
          </rules>
        </rewrite>
      </system.webServer>
    </configuration>


Staattiset tiedostot käsitellään WWW-palvelimen suoraan avaamatta Python koodi, suorituskyvyn parantamiseksi.

Yllä olevassa esimerkissä staattiset tiedostot levyn sijainnin pitäisi täsmätä sijainnin URL-osoitteen. Tämä tarkoittaa, että pyyntö `http://pythonapp.azurewebsites.net/static/site.css` palvelee tiedosto `\static\site.css`.

`WSGI_ALT_VIRTUALENV_HANDLER`ei, jossa voit määrittää WSGI käsittelijä. Yllä olevassa esimerkissä se on `app.wsgi_app` koska käsittelijä on nimeltä funktio `wsgi_app` - `app.py` pääkansioon.

`PYTHONPATH`Voit mukauttaa, mutta jos asennat kaikki riippuvuudet virtual ympäristössä määrittämällä ne requirements.txt, sinun ei kannata on muutettava sitä.


## <a name="virtual-environment-proxy"></a>Näennäisen ympäristön välityspalvelimen

Seuraavaa komentosarjaa käytetään noutaa WSGI käsittelijä, aktivoida virtual ympäristön ja kirjaudu virheet. Se on suunniteltu Yleinen ja käytetään ilman muutoksia.

Sisällön `ptvs_virtualenv_proxy.py`:

     # ############################################################################
     #
     # Copyright (c) Microsoft Corporation. 
     #
     # This source code is subject to terms and conditions of the Apache License, Version 2.0. A 
     # copy of the license can be found in the License.html file at the root of this distribution. If 
     # you cannot locate the Apache License, Version 2.0, please send an email to 
     # vspython@microsoft.com. By using this source code in any fashion, you are agreeing to be bound 
     # by the terms of the Apache License, Version 2.0.
     #
     # You must not remove this notice, or any other, from this software.
     #
     # ###########################################################################

    import datetime
    import os
    import sys
    import traceback

    if sys.version_info[0] == 3:
        def to_str(value):
            return value.decode(sys.getfilesystemencoding())

        def execfile(path, global_dict):
            """Execute a file"""
            with open(path, 'r') as f:
                code = f.read()
            code = code.replace('\r\n', '\n') + '\n'
            exec(code, global_dict)
    else:
        def to_str(value):
            return value.encode(sys.getfilesystemencoding())

    def log(txt):
        """Logs fatal errors to a log file if WSGI_LOG env var is defined"""
        log_file = os.environ.get('WSGI_LOG')
        if log_file:
            f = open(log_file, 'a+')
            try:
                f.write('%s: %s' % (datetime.datetime.now(), txt))
            finally:
                f.close()

    ptvsd_secret = os.getenv('WSGI_PTVSD_SECRET')
    if ptvsd_secret:
        log('Enabling ptvsd ...\n')
        try:
            import ptvsd
            try:
                ptvsd.enable_attach(ptvsd_secret)
                log('ptvsd enabled.\n')
            except: 
                log('ptvsd.enable_attach failed\n')
        except ImportError:
            log('error importing ptvsd.\n');

    def get_wsgi_handler(handler_name):
        if not handler_name:
            raise Exception('WSGI_ALT_VIRTUALENV_HANDLER env var must be set')
    
        if not isinstance(handler_name, str):
            handler_name = to_str(handler_name)
    
        module_name, _, callable_name = handler_name.rpartition('.')
        should_call = callable_name.endswith('()')
        callable_name = callable_name[:-2] if should_call else callable_name
        name_list = [(callable_name, should_call)]
        handler = None
        last_tb = ''

        while module_name:
            try:
                handler = __import__(module_name, fromlist=[name_list[0][0]])
                last_tb = ''
                for name, should_call in name_list:
                    handler = getattr(handler, name)
                    if should_call:
                        handler = handler()
                break
            except ImportError:
                module_name, _, callable_name = module_name.rpartition('.')
                should_call = callable_name.endswith('()')
                callable_name = callable_name[:-2] if should_call else callable_name
                name_list.insert(0, (callable_name, should_call))
                handler = None
                last_tb = ': ' + traceback.format_exc()
    
        if handler is None:
            raise ValueError('"%s" could not be imported%s' % (handler_name, last_tb))
    
        return handler

    activate_this = os.getenv('WSGI_ALT_VIRTUALENV_ACTIVATE_THIS')
    if not activate_this:
        raise Exception('WSGI_ALT_VIRTUALENV_ACTIVATE_THIS is not set')

    def get_virtualenv_handler():
        log('Activating virtualenv with %s\n' % activate_this)
        execfile(activate_this, dict(__file__=activate_this))

        log('Getting handler %s\n' % os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        handler = get_wsgi_handler(os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        log('Got handler: %r\n' % handler)
        return handler

    def get_venv_handler():
        log('Activating venv with executable at %s\n' % activate_this)
        import site
        sys.executable = activate_this
        old_sys_path, sys.path = sys.path, []
    
        site.main()
    
        sys.path.insert(0, '')
        for item in old_sys_path:
            if item not in sys.path:
                sys.path.append(item)

        log('Getting handler %s\n' % os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        handler = get_wsgi_handler(os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        log('Got handler: %r\n' % handler)
        return handler


## <a name="customize-git-deployment"></a>Mukauta Git käyttöönotto

[AZURE.INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-deployment.md)]


## <a name="troubleshooting---package-installation"></a>-Paketin asennuksen vianmääritys

[AZURE.INCLUDE [web-sites-python-troubleshooting-package-installation](../../includes/web-sites-python-troubleshooting-package-installation.md)]


## <a name="troubleshooting---virtual-environment"></a>Vianmääritys – Virtual ympäristössä

[AZURE.INCLUDE [web-sites-python-troubleshooting-virtual-environment](../../includes/web-sites-python-troubleshooting-virtual-environment.md)]

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja on artikkelissa [Python Developer Center](/develop/python/).

>[AZURE.NOTE] Jos haluat aloittaa Azure App palvelun ennen rekisteröimässä Azure-tili, siirry [Yritä App palvelu](http://go.microsoft.com/fwlink/?LinkId=523751), jossa lyhytkestoinen starter verkkosovellukseen heti voit luoda sovelluksen-palvelussa. Ei ole pakollinen; luottokortit ei ole sitoumukset.

## <a name="whats-changed"></a>Mikä on muuttunut
* Katso muutoksen opas verkkosivuilta App palveluun: [Azure App palvelu ja sen vaikutus aiemmin Azure-palvelut](http://go.microsoft.com/fwlink/?LinkId=529714)





 

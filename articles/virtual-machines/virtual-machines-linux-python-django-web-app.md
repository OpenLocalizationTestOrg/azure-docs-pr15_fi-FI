<properties 
    pageTitle="Python web app, jossa Django Linux | Microsoft Azure" 
    description="Opettele isännöidä käyttämällä Linux virtual koneen Azure-Django-pohjainen web-sovelluksen." 
    services="virtual-machines-linux" 
    documentationCenter="python" 
    authors="huguesv" 
    manager="wpickett" 
    editor=""
    tags="azure-resource-manager"/>

<tags 
    ms.service="virtual-machines-linux" 
    ms.workload="web" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="11/17/2015" 
    ms.author="huvalo"/>
    
# <a name="django-hello-world-web-application-on-a-linux-vm"></a>Django Hei maailma web-sovelluksen Linux AM

> [AZURE.SELECTOR]
- [Windows](virtual-machines-windows-classic-python-django-web-app.md)
- [Mac tai Linux](virtual-machines-linux-python-django-web-app.md)

<br>

Tässä opetusohjelmassa kerrotaan, miten Microsoft Azure käyttämällä Linux virtual koneen Django perustuva sivustossa isännöimiseen. Tässä opetusohjelmassa oletetaan, että ei ole edellisen kokemus Azure avulla. Kun olet suorittanut tämän oppaan, on Django-sovellukseen ylös- ja suorittamalla pilveen.

Opit, miten voit:

* Määritä Azure virtual-koneen isäntään Django. Kun tässä opetusohjelmassa kerrotaan, miten voit tehdä tämän **Linux**-kohdassa, sama voi myös tehdä ylläpidettävä Azure Windows Server-AM. 
* Voit luoda uuden Django sovelluksen Linux.

Tässä opetusohjelmassa seuraamalla voit luoda yksinkertaisen Hei maailma web-sovelluksen. Sovelluksen sovelluksessa Azure virtuaalikoneen.

Näyttökuva valmiin sovelluksen on alla:

![Hei maailma sivun näyttäminen Azure selainikkunassa.](./media/virtual-machines-linux-python-django-web-app/mac-linux-django-helloworld-browser.png)

[AZURE.INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## <a name="creating-and-configuring-an-azure-virtual-machine-to-host-django"></a>Luominen ja määrittäminen Azure virtual-koneen isäntään Django

1. Noudata annettu [tähän](virtual-machines-linux-quick-create-portal.md) luominen Azure virtuaalikoneen, *Ubuntu Server 14.04 l.* jakauman.  Voit halutessasi valita salasanan todennusta SSH julkisella avaimella sijaan.

1. Muokkaa verkko-käyttöoikeusryhmän sallimaan http liikenteen porttia 80 annettujen ohjeiden mukaisesti [tähän](../virtual-network/virtual-networks-create-nsg-arm-pportal.md).

1. Oletusarvon mukaan uuden virtuaalikoneen ei ole täydellinen toimialuenimi (FQDN).  Voit luoda sellaisen noudattamalla [Tässä](virtual-machines-linux-portal-create-fqdn.md).  Tämä vaihe on valinnainen suorittamiseen tässä opetusohjelmassa.

## <a id="setup"> </a>Kehitysympäristö määrittäminen

**Huomautus:** Jos käytössäsi on asennettava Python tai haluat käyttää asiakkaan kirjastot, lue [Python oppaaseen](../python-how-to-install.md).

Ubuntu Linux AM sisältää jo valmiiksi asennettu Python 2.7, mutta se ei ole Apache tai django asennettuna.  Muodostaa yhteyttä AM ja asentamaan Apache ja django seuraavasti.

1.  Käynnistä **terminaalissa** uudessa ikkunassa.
    
1.  Kirjoita seuraava komento muodostaa Azure AM.  Jos et luo FQDN, voit muodostaa yhteyden yhteenveto Azure perinteinen portaalissa virtuaalikoneen näkyvät julkiseen IP-osoite.

        $ ssh yourusername@yourVmUrl

1.  Kirjoita seuraavat komennot django asentaminen:

        $ sudo apt-get install python-setuptools python-pip
        $ sudo pip install django

1.  Kirjoita seuraava komento ja mod wsgi Apache asentaminen:

        $ sudo apt-get install apache2 libapache2-mod-wsgi


## <a name="creating-a-new-django-application"></a>Uuden Django-sovelluksen luominen

1.  Avaa käytit edellisen osion ssh kyselyjä oman AM **Pääte** -ikkuna.
    
1.  Kirjoita uuden Django projektin seuraavista komennoista:

        $ cd /var/www
        $ sudo django-admin.py startproject helloworld

    **Django admin.py** -komentosarja muodostaa perusrakenteen Django perustuva sivustot:
    -   **helloworld/Manage.PY** avulla voit isännöinnin aloittaa ja lopettaa Django perustuva sivustosi
    -   **helloworld/helloworld/Settings.PY** sisältää sovelluksen Django asetuksia.
    -   **helloworld/helloworld/URLs.PY** on yhdistäminen kunkin URL-osoite ja sen näkymän välillä.

1.  Luo uusi tiedosto nimeltä **views.py** **/var/www/helloworld/helloworld** hakemistossa. Tämä sisältää näkymän, joka hahmontaa "Hei"-sivu. Käynnistä editorin ja syötä seuraavat:
        
        from django.http import HttpResponse
        def home(request):
            html = "<html><body>Hello World!</body></html>"
            return HttpResponse(html)

1.  Nyt korvaa **urls.py** -tiedoston sisällön seuraavasti:

        from django.conf.urls import patterns, url
        urlpatterns = patterns('',
            url(r'^$', 'helloworld.views.home', name='home'),
        )


## <a name="setting-up-apache"></a>Apache määrittäminen

1.  Luo Apache virtual isännän kokoonpano tiedoston **/etc/apache2/sites-available/helloworld.conf**. Määritä sisällön seuraavat ja korvaa *yourVmName* käytät (esimerkiksi *pyubuntu*) tietokoneen nimi.

        <VirtualHost *:80>
        ServerName yourVmName
        </VirtualHost>
        WSGIScriptAlias / /var/www/helloworld/helloworld/wsgi.py
        WSGIPythonPath /var/www/helloworld

1.  Ota käyttöön sivustossa seuraavalla komennolla:

        $ sudo a2ensite helloworld

1.  Käynnistä Apache seuraavalla komennolla:

        $ sudo service apache2 reload

1.  Lataa-web-sivua selaimessa:

    ![Hei maailma sivun näyttäminen Azure selainikkunassa.](./media/virtual-machines-linux-python-django-web-app/mac-linux-django-helloworld-browser.png)


## <a name="shutting-down-your-azure-virtual-machine"></a>Azure virtuaalikoneen suljetaan

Kun enää tarvitse tämä opetusohjelma, sulkeminen ja/tai poista juuri luomasi Azure virtuaalikoneen vapauttaa resursseja muiden hakeminen ja välttää Azure käyttö lisäkustannuksia.

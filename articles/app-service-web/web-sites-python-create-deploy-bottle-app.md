<properties 
    pageTitle="Python web-sovellusten kanssa pullot Azure-tietokannassa" 
    description="Opetusohjelma, joissa esitellään käynnissä Python verkkosovellukseen Azure App palvelun verkkosovelluksissa." 
    services="app-service\web" 
    documentationCenter="python" 
    tags="python"
    authors="huguesv" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="02/19/2016"
    ms.author="huvalo"/>


# <a name="creating-web-apps-with-bottle-in-azure"></a>Azure pullot web Apps-sovellusten luominen

Tässä opetusohjelmassa kerrotaan, miten Aloita käynnissä Python Azure App palvelun verkkosovelluksissa. Web Apps on rajoitettu ilmainen isännöinti ja nopeaa käyttöönottoa ja voit käyttää Python! Kun sovelluksen kasvaa, voit siirtyä maksettu isännöinnin ja voit myös integroida kaikkien Azure muiden kanssa.

Luo pullot web framework-sovellusta (Katso tämä opetusohjelma vaihtoehtoiset versiot [Django](web-sites-python-create-deploy-django-app.md) ja [: N](web-sites-python-create-deploy-flask-app.md)). Web-sovelluksen luominen Azure Marketplace-Git käyttöönoton asetusten määrittäminen ja Kloonaa säilö paikallisesti. Sitten Suorita web app paikallisesti, tee muutokset, Vahvista ja push- [Azure palvelun Web sovellukset](http://go.microsoft.com/fwlink/?LinkId=529714). Opetusohjelman esitetään, kuinka voit tehdä tämän Windows-tai Mac tai Linux.

[AZURE.INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

>[AZURE.NOTE] Jos haluat aloittaa Azure App palvelun ennen rekisteröimässä Azure-tili, siirry [Yritä App palvelu](http://go.microsoft.com/fwlink/?LinkId=523751), jossa lyhytkestoinen starter verkkosovellukseen heti voit luoda sovelluksen-palvelussa. Ei ole pakollinen; luottokortit ei ole sitoumukset.

## <a name="prerequisites"></a>Edellytykset

- Windows, Mac tai Linux
- Python 2.7 tai 3.4
- setuptools, pip-virtualenv (vain Python 2.7)
- Git
- [Python Tools for Visual Studio 2.2][] (PTVS) - Huomautus: Tämä on valinnainen

**Huomautus**: TFS julkaiseminen on tällä hetkellä voi käyttää Python projekteissa.

### <a name="windows"></a>Windows

Jos sinulla ei vielä ole Python 2.7 tai 3,4 asennettu (32-bittinen), on suositeltavaa [Azure SDK Python 2.7] tai [Azure SDK Python 3.4] käyttäminen WWW-ympäristö asennusohjelma asennuksen. Tämä asentaa 32-bittisen version Python, setuptools, pip, virtualenv, (32-bittinen Python on asennettavat Azure host-tietokoneissa) jne. Voit myös saat Python [python.org].

Git Suosittelemme [Git Windowsin] tai [Windowsin GitHub]. Jos käytät Visual Studiossa, voit käyttää integroitu Git tuki.

On myös suositeltavaa asentaminen [Python Työkalut 2.2 Visual Studio]. Tämä on valinnainen, mutta jos käytössäsi on [Visual Studiossa], mukaan lukien vapaa Visual Studio yhteisön 2013 tai Visual Studio Express 2013 Web-sitten Näin aloitat hyvien Python IDE.

### <a name="maclinux"></a>Mac tai Linux

Pitäisi olla Python ja Git jo asennettu, mutta varmista, että sinulla on Python 2.7 tai 3.4.


## <a name="web-app-creation-on-the-azure-portal"></a>Web-sovelluksen luominen Azure-portaalissa

Sovelluksen luomisessa ensimmäiseksi on [Azure-portaalin](https://portal.azure.com)kautta web-sovelluksen luominen.  

1. Kirjaudu Azure-portaaliin ja valitse **Uusi** -painike vasemmassa alakulmassa. 
3. Kirjoita hakukenttään "python".
4. Valitse hakutuloksista **pullot**ja valitse sitten **Luo**.
5. Määritä uusi pullot sovellus, kuten luoda uuden sovelluksen palvelun suunnitelman ja uusi resurssiryhmä sen. Valitse **Luo**.
6. Määritä Git julkaiseminen juuri luomasi web-sovelluksen [Paikallisen Git käyttöönoton Azure sovelluksen-palveluun](app-service-deploy-local-git.md)osoitteessa ohjeita noudattamalla.
 
## <a name="application-overview"></a>Sovellusten yleiskuvaus

### <a name="git-repository-contents"></a>Git säilöön sisältö

Seuraavassa on perustietoja alkuperäisen Git säilössä, jossa on Kloonaa seuraavan osion löydät tiedostot.

    \routes.py
    \static\content\
    \static\fonts\
    \static\scripts\
    \views\about.tpl
    \views\contact.tpl
    \views\index.tpl
    \views\layout.tpl

Sovelluksen tärkeimmät lähteet. Koostuu perustyylin asettelun 3 sivut (indeksi yhteyshenkilön tietoja).  Staattiseksi sisällöksi ja komentosarjoja sisältävät automaattinen, jquery, modernizr ja vastata siihen.

    \app.py

Paikallinen kehittämisen palvelimen tuen. Tämän toiminnon avulla voit suorittaa sovelluksen paikallisesti.

    \BottleWebProject.pyproj
    \BottleWebProject.sln

Projektin [Python Tools for Visual Studio]tiedostot.

    \ptvs_virtualenv_proxy.py

IIS-välityspalvelimen virtual ympäristöissä ja PTVS remote virheenkorjaus tuki.

    \requirements.txt

Tämän sovelluksen tarvitsemat ulkoiset paketit. Käyttöönoton komentosarja pip Asenna tässä tiedostossa mainittu paketit.
 
    \web.2.7.config
    \web.3.4.config

IIS: N määritystiedostot. Käyttöönoton komentosarja käyttää tarvittavat web.x.y.config ja kopioida sen web.config.

### <a name="optional-files---customizing-deployment"></a>Valinnaiset tiedostot - mukauttaminen käyttöönotto

[AZURE.INCLUDE [web-sites-python-customizing-deployment](../../includes/web-sites-python-customizing-deployment.md)]

### <a name="optional-files---python-runtime"></a>Valinnaiset tiedostot - Python runtime

[AZURE.INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-runtime.md)]

### <a name="additional-files-on-server"></a>Muita tiedostoja palvelimeen

Joidenkin tiedostojen luotu palvelimeen, mutta ei lisätä git säilöön. Nämä ovat luomia käyttöönoton komentosarja.

    \web.config

IIS-kokoonpanotiedosto. Luotu web.x.y.config jokaisen ympäristöön.

    \env\

Python virtual ympäristössä. Luodaan käyttöönoton aikana, jos yhteensopiva virtual ympäristössä, jossa ei vielä ole web-sovellukseen.  Requirements.txt luetellut paketit ovat pip asennettu, mutta pip ohittaa asennus, jos paketit on jo asennettu.

Seuraavat 3 osissa, valitse 3 eri ympäristöissä web app-kehittämiseen toimintatapaa:

- Windowsin Python Tools for Visual Studio
- Komentorivin Windowsiin
- Mac tai Linux-komento viivalla


## <a name="web-app-development---windows---python-tools-for-visual-studio"></a>Web App - Windows - Python Kehitystyökalut Visual Studio

### <a name="clone-the-repository"></a>Kloonaa säilö

Ensin Kloonaa säilöön käyttämällä annettu Azure-portaalin URL-osoite. Lisätietoja on artikkelissa [Paikallisen Git käyttöönoton Azure-sovelluksen palveluun](app-service-deploy-local-git.md).

Avaa ratkaisutiedosto (.sln), joka sisältyy säilö ylimmällä.

![](./media/web-sites-python-create-deploy-bottle-app/ptvs-solution-bottle.png)

### <a name="create-virtual-environment"></a>Luo virtuaalisia ympäristössä

Nyt luoda virtuaalisen ympäristön paikallisen kehittämiseen. Napsauta hiiren kakkospainikkeella **Python ympäristöissä** valitsemalla **Lisää näennäisen ympäristön...**.

- Varmista, että ympäristö on `env`.

- Valitse Perus kääntäjän. Varmista, että käyttää samaa Python, joka on valittuna web-sovelluksen (-runtime.txt tai Azure-portaalissa koodiin **Sovelluksen asetukset** -sivu).

- Varmista, että ladattava ja asennettava pakettien vaihtoehto on valittuna.

![](./media/web-sites-python-create-deploy-bottle-app/ptvs-add-virtual-env-27.png)

Valitse **Luo**. Tämä luo virtual ympäristössä, ja asenna requirements.txt luetellut riippuvuudet.

### <a name="run-using-development-server"></a>Suorita development Serveriä käyttämällä

Käynnistä virheenkorjaus painamalla F5-näppäintä ja että selain aukeaa automaattisesti käytössä paikallisesti sivulle.

![](./media/web-sites-python-create-deploy-bottle-app/windows-browser-bottle.png)

Voit määrittää keskeytyskohdat käyttää lähteistä, katso Windowsin jne. Katso lisätietoja [Python Tools for Visual Studio dokumentaatio] eri ominaisuuksia.

### <a name="make-changes"></a>Muutosten tekeminen

Nyt voit kokeilla tekemällä muutoksia sovelluksen lähteiden ja/tai malleja.

Kun olet testannut tekemäsi muutokset, Vahvista Git säilö:

![](./media/web-sites-python-create-deploy-bottle-app/ptvs-commit-bottle.png)

### <a name="install-more-packages"></a>Asenna lisää paketit

Sovellus voi olla riippuvuudet Python ja pullot lisäksi.

Voit asentaa muita paketteja käyttämällä pip. Jos haluat asentaa paketin, näennäisen ympäristön hiiren kakkospainikkeella ja valitse **Asenna Python paketti**.

Kirjoita asentaminen Azure SDK Python, jonka kautta pääset käsiksi Azure-tallennustilan, palvelun bus ja Azure muista palveluista, esimerkiksi `azure`:

![](./media/web-sites-python-create-deploy-bottle-app/ptvs-install-package-dialog.png)

Virtual ympäristöön hiiren kakkospainikkeella ja valitse **Luo requirements.txt** päivittämään requirements.txt.

Valitse sitten Vahvista muutokset requirements.txt Git säilöön.

### <a name="deploy-to-azure"></a>Ottaa käyttöön Azure

Käynnistettävän käyttöönoton valitsemalla **Synkronoi** tai **Push**. Synkronointi ei push ja erotettu.

![](./media/web-sites-python-create-deploy-bottle-app/ptvs-git-push.png)

Ensimmäisen käyttöönoton kestää jonkin aikaa, koska se luo virtuaalisen ympäristön, asenna pakettien jne.

Visual Studio ei näy käyttöönoton etenemisen. Jos haluat tarkastella tulosteen, katso [vianmääritys - käyttöönoton](#troubleshooting-deployment).

Siirry Azure URL-Osoitteen tekemäsi muutokset näkyviin.


## <a name="web-app-development---windows---command-line"></a>Web app-kehitys - Windows - komento rivi

### <a name="clone-the-repository"></a>Kloonaa säilö

Ensin Kloonaa säilöön käyttämällä annettu Azure-portaalin URL-osoite ja lisää Azure säilöön etäkohteeseen. Lisätietoja on artikkelissa [Paikallisen Git käyttöönoton Azure-sovelluksen palveluun](app-service-deploy-local-git.md).

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url> 

### <a name="create-virtual-environment"></a>Luo virtuaalisia ympäristössä

Olemme Luo uusi virtuaalisen ympäristön tarkoitettu (Älä lisää se säilö). Virtuaalinen ympäristöissä-Python eivät ole relocatable, joten jokainen parissa sovelluksen kehittäjä Luo omia paikallisesti.

Varmista, että käyttää samaa Python, joka on valittuna web-sovelluksen (-runtime.txt tai web-sovelluksen Azure-portaalissa sovelluksen asetukset-sivu)

Saat Python 2.7:

    c:\python27\python.exe -m virtualenv env

Saat Python 3.4:

    c:\python34\python.exe -m venv env

Asenna kaikki sovelluksesi vaatii ulkoisen paketit. Säilön ylimmällä tasolla requirements.txt-tiedoston avulla voit asentaa paketit virtual-ympäristössä:

    env\scripts\pip install -r requirements.txt

### <a name="run-using-development-server"></a>Suorita development Serveriä käyttämällä

Voit käynnistää sovellusten kehittämisen-palvelimen seuraavalla komennolla:

    env\scripts\python app.py

Konsolin näkyy URL-osoite ja palvelimen portti seuraa avulla:

![](./media/web-sites-python-create-deploy-bottle-app/windows-run-local-bottle.png)

Avaa selain, että URL-osoite.

![](./media/web-sites-python-create-deploy-bottle-app/windows-browser-bottle.png)

### <a name="make-changes"></a>Muutosten tekeminen

Nyt voit kokeilla tekemällä muutoksia sovelluksen lähteiden ja/tai malleja.

Kun olet testannut tekemäsi muutokset, Vahvista Git säilö:

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a>Asenna lisää paketit

Sovellus voi olla riippuvuudet Python ja pullot lisäksi.

Voit asentaa muita paketteja käyttämällä pip. Jos esimerkiksi asentaminen Azure SDK Python, jonka kautta pääset käsiksi Azure-tallennustilan, palvelun bus ja Azure muista palveluista, kirjoita:

    env\scripts\pip install azure

Varmista, että voit päivittää requirements.txt:

    env\scripts\pip freeze > requirements.txt

Vahvista muutokset:

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-to-azure"></a>Ottaa käyttöön Azure

Käynnistettävän käyttöönoton tuo Azure muutokset:

    git push azure master

Käyttöönoton komentosarja, mukaan lukien näennäisen ympäristön luonti-pakettien asennuksen web.config luominen tulos tulee näkyviin.

Siirry Azure URL-Osoitteen tekemäsi muutokset näkyviin.


## <a name="web-app-development---maclinux---command-line"></a>Web-sovellusten kehittämiseen - Mac tai Linux - komento

### <a name="clone-the-repository"></a>Kloonaa säilö

Ensin Kloonaa säilöön käyttämällä annettu Azure-portaalin URL-osoite ja lisää Azure säilöön etäkohteeseen. Lisätietoja on artikkelissa [Paikallisen Git käyttöönoton Azure-sovelluksen palveluun](app-service-deploy-local-git.md).

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url> 

### <a name="create-virtual-environment"></a>Luo virtuaalisia ympäristössä

Olemme Luo uusi virtuaalisen ympäristön tarkoitettu (Älä lisää se säilö). Virtuaalinen ympäristöissä-Python eivät ole relocatable, joten jokainen parissa sovelluksen kehittäjä Luo omia paikallisesti.

Varmista, että käyttää samaa Python, joka on valittuna web-sovelluksen (-runtime.txt tai Azure-portaalissa koodiin sovelluksen asetukset-sivu).

Saat Python 2.7:

    python -m virtualenv env

Saat Python 3.4:

    python -m venv env
tai pyvenv kirjekuori

Asenna kaikki sovelluksesi vaatii ulkoisen paketit. Säilön ylimmällä tasolla requirements.txt-tiedoston avulla voit asentaa paketit virtual-ympäristössä:

    env/bin/pip install -r requirements.txt

### <a name="run-using-development-server"></a>Suorita development Serveriä käyttämällä

Voit käynnistää sovellusten kehittämisen-palvelimen seuraavalla komennolla:

    env/bin/python app.py

Konsolin näkyy URL-osoite ja palvelimen portti seuraa avulla:

![](./media/web-sites-python-create-deploy-bottle-app/mac-run-local-bottle.png)

Avaa selain, että URL-osoite.

![](./media/web-sites-python-create-deploy-bottle-app/mac-browser-bottle.png)

### <a name="make-changes"></a>Muutosten tekeminen

Nyt voit kokeilla tekemällä muutoksia sovelluksen lähteiden ja/tai malleja.

Kun olet testannut tekemäsi muutokset, Vahvista Git säilö:

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a>Asenna lisää paketit

Sovellus voi olla riippuvuudet Python ja pullot lisäksi.

Voit asentaa muita paketteja käyttämällä pip. Jos esimerkiksi asentaminen Azure SDK Python, jonka kautta pääset käsiksi Azure-tallennustilan, palvelun bus ja Azure muista palveluista, kirjoita:

    env/bin/pip install azure

Varmista, että voit päivittää requirements.txt:

    env/bin/pip freeze > requirements.txt

Vahvista muutokset:

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-to-azure"></a>Ottaa käyttöön Azure

Käynnistettävän käyttöönoton tuo Azure muutokset:

    git push azure master

Käyttöönoton komentosarja, mukaan lukien näennäisen ympäristön luonti-pakettien asennuksen web.config luominen tulos tulee näkyviin.

Siirry Azure URL-Osoitteen tekemäsi muutokset näkyviin.


## <a name="troubleshooting---package-installation"></a>-Paketin asennuksen vianmääritys

[AZURE.INCLUDE [web-sites-python-troubleshooting-package-installation](../../includes/web-sites-python-troubleshooting-package-installation.md)]


## <a name="troubleshooting---virtual-environment"></a>Vianmääritys – Virtual ympäristössä

[AZURE.INCLUDE [web-sites-python-troubleshooting-virtual-environment](../../includes/web-sites-python-troubleshooting-virtual-environment.md)]


## <a name="next-steps"></a>Seuraavat vaiheet

Saat lisätietoja pullot ja Python Tools for Visual Studio seuraavista linkeistä: 
 
- [Pullot dokumentaatio]
- [Python Tools for Visual Studio dokumentaatio]

Lisätietoja Azure-taulukkotallennus ja MongoDB avulla:

- [Pullot ja MongoDB-Azure Python Tools for Visual Studio kanssa]
- [Pullot ja Visual Studio työkaluilla Python Azure-Azure-taulukkotallennus]

## <a name="whats-changed"></a>Mikä on muuttunut
* Katso muutoksen opas verkkosivuilta App palveluun: [Azure App palvelu ja sen vaikutus aiemmin Azure-palvelut](http://go.microsoft.com/fwlink/?LinkId=529714)


<!--Link references-->
[Pullot ja MongoDB-Azure Python Tools for Visual Studio kanssa]: web-sites-python-ptvs-bottle-table-storage.md
[Pullot ja Visual Studio työkaluilla Python Azure-Azure-taulukkotallennus]: web-sites-python-ptvs-bottle-table-storage.md

<!--External Link references-->
[Azure SDK Python 2.7]: http://go.microsoft.com/fwlink/?linkid=254281
[Azure SDK Python 3.4]: http://go.microsoft.com/fwlink/?linkid=516990
[Python.org]: http://www.python.org/
[Git for Windowsissa]: http://msysgit.github.io/
[GitHub for Windowsissa]: https://windows.github.com/
[Python Tools for Visual Studio]: http://aka.ms/ptvs
[Python Tools for Visual Studio 2.2]: http://go.microsoft.com/fwlink/?LinkID=624025
[Visual Studio]: http://www.visualstudio.com/
[Python Tools for Visual Studio dokumentaatio]: http://aka.ms/ptvsdocs 
[Pullot dokumentaatio]: http://bottlepy.org/docs/dev/index.html
 

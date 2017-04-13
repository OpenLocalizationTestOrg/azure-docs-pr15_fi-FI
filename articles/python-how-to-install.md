<properties
    pageTitle="Asenna Python ja SDK - Azure"
    description="Opi asentamaan Python ja SDK Azure käytettäväksi."
    services=""
    documentationCenter="python"
    authors="lmazuel"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="lmazuel"/>

# <a name="installing-python-and-the-sdk"></a>Python ja SDK: N asentaminen

Python on helppo määritys Windows ja esiasennukseen Mac, Linux ja [Bash Windows](https://msdn.microsoft.com/commandline/wsl/about). Tässä oppaassa esitellään asennuksen etkä saa tietokoneen valmis käytettäväksi Azure.

## <a name="whats-in-the-python-azure-sdk"></a>Mikä Python Azure SDK?

Azure SDK Python sisältää osat, joiden avulla voit kehittää, käyttöönottoa ja Azure Python-sovellusten hallinta. Tarkemmin sanottuna Azure SDK Python sisältää seuraavat:

* **Kirjastoissa**. Luokan nämä kirjastot on käyttöliittymään Azure resurssit, kuten tallennustilan tilit-näennäiskoneiden hallinta.

* **Runtimen kirjastoihin**. Luokan nämä kirjastot Azure ominaisuuksien, kuten tallennuskiintiön ja palvelun bus liittymän avulla.

## <a name="which-python-and-which-version-to-use"></a>Mitä Python ja mitä versiota

Python tulkkien useita flavors ovat käytettävissä - Esimerkkejä:

* CPython - vakio- ja yleisimmin käytettyjä Python kääntäjän
* PyPy - nopea ja yhteensopiva vaihtoehtoisen toteutuksen CPython
* IronPython - Python kääntäjän, joka suoritetaan .net/CLR
* Jython - Python kääntäjän, joka suoritetaan Java virtuaalikoneen

**CPython** v2.7 tai v3.3 + ja PyPy 5.4.0 testattu ja tuetut Python Azure SDK-paketissa.

## <a name="where-to-get-python"></a>Mistä saa Python?

Saat CPython seuraavilla tavoilla:

* Suoraan [www.python.org][]
* Hyvämaineinen distro, kuten [www.continuum.io][], [www.enthought.com][] tai [www.activestate.com][] kohteesta
* Tietolähteen luominen!

Jos sinulla ei ole tietyn tarpeen, on suositeltavaa kaksi ensimmäistä vaihtoehtoa.

## <a name="sdk-installation-on-windows-linux-and-macos-client-libraries-only"></a>SDK-asennuksen Windows Linux ja Mac OS (vain asiakas-kirjastojen)

Jos sinulla on jo asennettu Python, voit asentaa paljon asiakkaan kirjastojen aiemmin Python 2.7 tai Python 3.3 + ympäristössä pip. Paketit ladataan [Python paketin indeksi][] (PyPI).

Voit joutua järjestelmänvalvojaoikeudet:

- Linux-ja Mac OS- `sudo` komentoa: `sudo pip install azure-mgmt-compute`.
- Windows: Avaa PowerShell-/ komentokehote järjestelmänvalvojana

Voit asentaa yksittäin kunkin kirjaston jokaisen Azure palvelun:

```console
   $ pip install azure-batch          # Install the latest Batch runtime library
   $ pip install azure-mgmt-scheduler # Install the latest Storage management library
```

Esikatselu-paketit voidaan asentaa käyttämällä `--pre` merkintä:

```console
   $ pip install --pre azure-mgmt-compute # will install only the latest Compute Management library
```

Voit myös asentaa Azure kirjastojen joukko yksirivinen käyttämisen `azure` metatiedot-paketti. Koska kaikki pakettien metatiedot tämän paketin on julkaistu vakaa ja `azure` metatiedot paketti on yhä esikatselu. Kuitenkin core-palvelupaketteja koodin laatu/täydellisiä perspektiivit niin sanottu "vakaata" tällä hetkellä
- se virallisesti lukee sellaisenaan synkronoinnissa muiden kielten kanssa mahdollisimman pian. Emme ole budjetointi edelleen tärkeimmät muutokset vasta sitten.

Koska preview-versio, sinun on käytettävä `--pre` merkintä:

```console
   $ pip install --pre azure
```
   
tai suoraan

```console
   $ pip install azure==2.0.0rc6
```

## <a name="getting-more-packages"></a>Lisää pakettien käytön

[Python paketin indeksi][] (PyPI) on Python kirjastojen monipuolisia valintaa.  Jos haluat asentaa Distro, on jo useimmat kiinnostavat bittien tekniset tietojenkäsittely-web-kehitys eri skenaarioissa.


## <a name="python-tools-for-visual-studio"></a>Python Tools for Visual Studio

[Python Tools for Visual Studio][] (PTVS) on Microsoftin, joka muuttuu ja täydellisiksi Python IDE/OSS laajennus:

![How-to-asennus-python-ptvs](./media/python-how-to-install/how-to-install-python-ptvs.png)

Käyttämällä PTVS on valinnainen, mutta kun antaa Python ja Web: Project-ratkaisun, virheenkorjaus profilointi, vuorovaikutteinen ikkunan, mallin muokkaaminen tai Intellisense-versiota.

PTVS myös on helppo käyttöön Microsoft Azure-tukeen [Pilvipalveluihin][] ja [sivustojen][]käyttöönottoa varten.

PTVS toimii aiemmin Visual Studio-2013 ja 2015 asennuksen.  Asiakirjat, ladattavat tiedostot ja keskustelut on artikkelissa [Python Tools for Visual Studio].  

## <a name="python-azure-scenarios-for-linux-and-macos"></a>Python Azure tilanteita, joissa Linux- ja Mac OS

Linux-tai Mac OS-tärkeimmät Azure tilanteita, joissa tuetaan:

1. Muissa Azure-palveluita käyttämällä Python asiakas-kirjastot

2. Käynnissä sovelluksen Linux AM

3. Kehittäminen ja julkaiseminen Azure-sivustojen käyttäminen Git

Ensimmäinen skenaario mahdollistaa tuottaa monipuolisia web Apps-sovellusten, joka hyödyntää Azure REST API Azure PaaS-ominaisuudet, kuten [Blob-objektien tallennustilaan][], [jonossa tallennustilan][] [taulukkotallennus][] kautta Pythonic kääreisiin jne. Nämä toimii samannimisen Windows, Mac tai Linux.  Voit käyttää myös asiakkaan nämä kirjastot paikallista kehittämistä käyttämääsi laitteeseen tai Linux-AM Azure-käyttöjärjestelmässä.

AM-skenaariossa voit Aloita Linux-AM valittua (Ubuntu CentOS, Suse) ja suorita ja hallita mitä haluat.  Esimerkkinä voit suorittaa [IPython][] STAA/muistikirjan Windows/Mac tai Linux-tietokoneeseen ja osoita selaimen Linux tai IPython-ohjelma-käyttöjärjestelmässä Azure Windows usean prosessi AM. Katso lisätietoja [IPython muistikirjan Azure][] -opetusohjelman.

Saat lisätietoja Linux AM asetukset-kohdassa [Luo virtuaalikoneen käynnissä Linux][] -opetusohjelma.

Käytä Git käyttöönoton, voit kehittää Python web-sovelluksen ja julkaista sen Azure sivustoon kaikissa käyttöjärjestelmissä.  Kun painat lisääminen säilöön Azure, luo automaattisesti virtuaalisen ympäristön ja pip Asenna tarvittavat paketit.

Saat lisätietoja kehittäminen ja julkaiseminen Azure sivustojen Opetusohjelmissa [Luominen sivustojen kanssa Django][], [Luominen sivustojen kanssa pullot][]ja [Luoda sivustoja: N kanssa][]. Yleisiä tietoja käyttämällä minkä tahansa WSGI-yhteensopiva framework on artikkelissa [Määrittäminen Python Azure-sivuston käyttö][].


## <a name="additional-software-and-resources"></a>Lisäohjelmistoa ja resurssit:

* [Azure SDK Python ReadTheDocs](http://azure-sdk-for-python.readthedocs.io/en/latest/)
* [Azure SDK Python Github](https://github.com/Azure/azure-sdk-for-python)
* [Virallinen Azure esimerkkejä, joiden Python](https://azure.microsoft.com/documentation/samples/?platform=python)
* [Continuum Analytics Python jakelu][]
* [Enthought Python jakauman.][]
* [ActiveState Python jakauman.][]
* [SciPy - tuotepakettia tieteellisen Python kirjastotyypit][]
* [NumPy - Python ovat numeeriset arvot kirjasto][]
* [Django Project - kehittynyt web framework/CMS][]
* [IPython - aiemmin Lisäasetukset STAA/muistikirjan Python varten][]
* [Azure IPython muistikirjan][]
* [Python Tools for Visual Studio-GitHub][]
* [Python Developer Center](/develop/python/)

[Continuum Analytics Python jakelu]: http://continuum.io
[Enthought Python jakauman.]: http://www.enthought.com
[ActiveState Python jakauman.]: http://www.activestate.com
[WWW.Python.org]: http://www.python.org
[WWW.continuum.IO]: http://continuum.io
[WWW.enthought.com]: http://www.enthought.com
[WWW.activestate.com]: http://www.activestate.com
[SciPy - tuotepakettia tieteellinen Python kirjastotyypit]: http://www.scipy.org
[NumPy - Python ovat numeeriset arvot kirjasto]: http://www.numpy.org
[Django Project - kehittynyt web framework/CMS]: http://www.djangoproject.com
[IPython - aiemmin Lisäasetukset STAA/muistikirjan Python varten]: http://ipython.org
[IPython]: http://ipython.org
[Azure IPython muistikirjan]: virtual-machines-linux-jupyter-notebook.md
[Pilvipalveluihin]: cloud-services-python-ptvs.md
[Sivuston käyttö]: web-sites-python-ptvs-django-mysql.md
[Python Tools for Visual Studio]: http://aka.ms/ptvs
[Python Tools for Visual Studio-GitHub]: https://github.com/microsoft/ptvs
[Python paketin indeksi]: http://pypi.python.org/pypi
[Microsoft Azure SDK for Python 2.7]: http://go.microsoft.com/fwlink/?LinkId=254281
[Microsoft Azure SDK for Python 3.4]: http://go.microsoft.com/fwlink/?LinkID=516990
[Setting up a Linux VM via the Azure portal]: create-and-configure-opensuse-vm-in-portal.md
[How to use the Azure Command-Line Interface]: crossplat-cmd-tools.md
[Luo Virtual koneeseen, jossa käytetään Linux]: virtual-machines-linux-quick-create-cli.md
[Sivustojen luominen Django]: web-sites-python-create-deploy-django-app.md
[Pullot sisältävän sivuston luominen]: web-sites-python-create-deploy-bottle-app.md
[Sivustojen luominen: N avulla]: web-sites-python-create-deploy-flask-app.md
[Määrittäminen Python Azure-sivuston käyttö]: web-sites-python-configure.md
[taulukkotallennus]: storage-python-how-to-use-table-storage.md
[jonon tallennustila]: storage-python-how-to-use-queue-storage.md
[Blob-objektien tallennustilaan]: storage-python-how-to-use-blob-storage.md

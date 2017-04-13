<properties
    pageTitle="Virtual koneen IPython muistikirjan palvelimeksi määrittäminen | Microsoft Azure"
    description="Voit määrittää kehittyneen analyysin ylös Azure Virtual Machine käytettäväksi tiede ympäristössä IPython palvelimen kanssa."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="xibingao;bradsev" />

# <a name="set-up-an-azure-virtual-machine-as-an-ipython-notebook-server-for-advanced-analytics"></a>Määritä Azure virtuaalikoneen IPython muistikirjan palvelimeksi kehittyneen analyysin varten

Tässä ohjeaiheessa esitellään valmistelu ja Azure virtual-koneen kehittyneen analyysin, joita käytetään osana tietojen tiede-ympäristöä varten. Windows-virtuaalikoneen on määritetty tukemaan työkaluja, kuten kuten IPython muistikirjan, Azure-tallennustilan Explorer, AzCopy sekä muita apuohjelmia, jotka ovat hyödyllisiä kehittyneen analyysin projektien. Azure tallennustilan Explorer ja AzCopy, esimerkiksi on kätevä tapoja tietojen lataaminen Azure-blob-säiliö paikallisesta tietokoneesta tai lataa se tietokoneeseesi Blob-objektien tallennustilaan.

## <a name="create-vm"></a>Vaihe 1: Yleisiä tarkoitus Azure virtuaalikoneen luominen

Jos on jo Azure virtuaalikoneen ja haluat määrittää sen IPython muistikirjan-palvelimelle, voit ohittaa tämän vaiheen ja siirry [Vaihe 2: Lisää päätepisteen IPython muistikirjoja aiemmin virtuaalikoneen](#add-endpoint).

Ennen kuin aloitat luominen virtual machine-Azure, sinun on tietokone, jota tarvitaan käsittelemään niiden projektin tiedot koon määrittäminen. Pienempi tietokoneissa on vähemmän muistia vähemmän kuin suurempi koneet suorittimen sydämiä, mutta ne ovat myös vähemmän kallista. Luettelo tietokoneeseen tyypit ja hinnat ohjeessa <a href="http://azure.microsoft.com/pricing/details/virtual-machines/" target="_blank">Näennäiskoneiden hinnoittelu</a> -sivu

1. Kirjaudu sisään <a href="https://manage.windowsazure.com" target="_blank">perinteinen Azure</a>-portaaliin ja valitse **Uusi** vasemmassa alakulmassa. Näyttöön ponnahdusikkuna. Valitse **Laske** -> **VIRTUAALIKONEEN** -> **FROM VALIKOIMA**.

    ![Työtilan luominen][24]

2. Valitse jokin seuraavista kuvat:

    * Windows Server 2012 R2 Palvelinkeskukseen
    * Windows Server Essentials käyttäjäkokemuksen (Windows Server 2012 R2)

    Valitse oikealla oikeassa alakulmassa Siirry seuraavaan määritys-sivulla nuoli.

    ![Työtilan luominen][25]

3. Virtuaalikoneen haluat luoda, valitse tietokoneen koko nimi (oletus: A3) koko prosessin suorittaminen tietokoneen tiedot ja miten tehokkaita perusteella haluat machine (muistin koon ja Laske sydämiä määrä), kirjoita tietokoneen käyttäjänimi ja salasana. Valitse nuoli oikealle, jos haluat siirtyä seuraavaan määritys-sivulla.

    ![Työtilan luominen][26]

4. Valitse **Alue/AFFINITEETTI ryhmän/VIRTUAL verkkoon** , joka sisältää **TALLENNUSTILAN tili** , jota aiot käyttää virtual tietokoneen ja valitse sitten tallennustilan tilin. Lisää päätepisteen alareunaan **PÄÄTEPISTEET** -kenttään kirjoittamalla nimen päätepisteen ("IPython" tätä). Voit käyttää mitä tahansa merkkijonoa päätepiste ja mikä tahansa kokonaisluku väliltä 0 ja 65 536, joka on **käytettävissä** **Julkisen PORTIN** **nimi** . **Yksityinen PORTIN** on oltava **9999**. Käyttäjien kannattaa **välttää** käytön julkisen portit, jotka on jo myönnetty internet-palvelut. <a href="http://www.chebucto.ns.ca/~rakerman/port-table.html" target="_blank">Portit, Internet-palveluiden</a> on luettelo porteista, joka on määritetty ja voidaan välttää.

    ![Työtilan luominen][27]

5. Valitse Aloita valmistelu virtuaalikoneen valintamerkkiä.

    ![Työtilan luominen][28]


15 25 minuuttia virtuaalikoneen valmistelu voi kestää. Kun virtuaalikoneen on luotu, tämän tietokoneen tila pitäisi näyttää **käynnissä**.

![Työtilan luominen][29]

## <a name="add-endpoint"></a>Vaihe 2: Lisää päätepisteen IPython muistikirjoja aiemmin virtuaalikoneen

Jos olet luonut virtuaalikoneen noudattamalla vaiheessa 1, päätepisteen IPython muistikirja on jo lisätty ja voit ohittaa tämän vaiheen.

Jos virtuaalikoneen on jo olemassa, ja sinun on lisättävä päätepisteen IPython muistikirjan, jota käytetään asentamisessa vaiheessa 3 alla ensimmäinen loki perinteinen Azure-portaaliin, valitse virtuaalikoneen ja lisää IPython muistikirjan palvelimen päätepisteen. Seuraavassa kuvassa on portaalin näyttökuva jälkeen IPython muistikirjan päätepisteen on lisätty Windows virtual koneeseen.

![Työtilan luominen][17]

## <a name="run-commands"></a>Vaihe 3: Asenna IPython muistikirjan ja muut hyödyt Työkalut

Kun virtuaalikoneen on luotu, kirjaudu sisään Windows virtuaalikoneen Remote Desktop Protocol (RDP) avulla. Ohjeita on artikkelissa [miten Kirjaudu virtuaalikoneen käytössä Windows Server](../virtual-machines/virtual-machines-windows-classic-connect-logon.md). Avaa **komentokehote** (**ei Powershell-komentoikkunassa**) **järjestelmänvalvoja** ja suorittamalla seuraavan komennon.

    set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/MachineSetup/Azure_VM_Setup_Windows.ps1'

    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"

Kun asennus on valmis, IPython muistikirjan palvelimen käynnistyy automaattisesti *C:\\käyttäjien\\\<käyttäjänimi\>\\asiakirjojen\\IPython muistikirjat* hakemisto.

Kirjoita pyydettäessä salasana IPython muistikirjan ja tietokoneen järjestelmänvalvojan salasanan. Näin IPython muistikirjan suorittamaan palveluna tietokoneeseen.

## <a name="access"></a>Vaihe 4: IPython muistikirjojen käyttäminen WWW-selaimella
Käyttää IPython muistikirja-palvelinta, avaa sivuston selaimessa ja syötteen *https://&#60;virtual DNS nimi >: & #60; julkisen porttinumero >* URL-tekstiruutuun. Tässä *& #60; julkisen porttinumero >* pitäisi olla porttinumero, jolloin IPython muistikirjan päätepisteen on lisätty määritetty.

*& #60; virtuaalikoneen DNS nimi >* tukikäytännöistä Azure perinteinen Portal. Kun lokiin kirjaaminen-perinteinen-portaaliin, valitse **NÄENNÄISKONEIDEN**, valitse luomasi machine ja valitse sitten **raporttinäkymät-IKKUNAN**, DNS-nimi näkyy seuraavasti:

![Työtilan luominen][19]

Kohtaat varoituksen, jossa sanotaan, että _sivuston suojausvarmenteessa ongelma_ (Internet Explorer) tai (Chrome), _yhteys on ei yksityinen_ esitetty seuraavassa esitetyllä tavalla. Valitse **Jatka tämän sivuston (ei suositella)** (Internet Explorer) tai **Lisäasetukset** ja valitse sitten * *, jatka & #60;* DNS-nimen*> (epäluotettavien) ** (Chrome), jos haluat jatkaa. Kirjoita sitten Siirry IPython muistikirjoihin aiemmin määritetty salasana.

**Internet Explorer:**
![työtilan luominen][20]

**Chrome:**
![työtilan luominen][21]

Kun olet kirjautunut sisään IPython muistikirjaan, hakemisto *DataScienceSamples* näkyy selaimesta. Tämä kansio sisältää otoksen IPython muistikirjat, jotka on jaettu Microsoft avulla käyttäjät voivat suorittaa tietojen tiede tehtävät. Nämä otoksen IPython muistikirjat on kuitattu ulos [**Github**](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks) säilöstä näennäiskoneiden prosessin määrittäminen IPython muistikirjan palvelimen aikana. Microsoft ylläpitää ja päivittää tätä säilöä usein. Käyttäjät voivat käy Github säilö saat viimeksi päivitetty otoksen IPython muistikirjat.
![Työtilan luominen][18]

## <a name="upload"></a>Vaihe 5: Lataa aiemmin luodun IPython muistikirjan paikallisesta tietokoneesta IPython muistikirjan palvelimeen

IPython muistikirjat on helposti käyttäjät voivat ladata aiemmin luodun IPython muistikirjan tietokoneissa näennäiskoneiden IPython muistikirjan palvelimeen. Kun käyttäjät kirjautuvat IPython muistikirjan selaimessa, valitse **kansio** , joka IPython muistikirjan ladataan. Valitse IPython muistikirjan .ipynb tiedoston lataaminen paikallisesta tietokoneesta **Resurssienhallinnassa**ja vedä ja pudota se selaimella IPython muistikirjan-kansioon. Napsauttamalla **Lataa** -painiketta, .ipynb-tiedoston lataaminen IPython muistikirjan palvelimeen. Muiden käyttäjien sitten aloittaa sen käyttämisen web-selaimissa.

![Työtilan luominen][22]

![Työtilan luominen][23]


##<a name="shutdown"></a>Sulkeminen ja Kohdista varaustiedoista virtual machine, kun ne eivät ole käytössä

Azure-Virtuaalikoneissa laitteiden hinnoittelu kuin **maksaa vain käyttötarkoitus**. Jos haluat varmistaa, että sinulla on ei laskutettava käytettäessä ei virtuaalikoneen, sen on oltava **pysäytetty (Deallocated)** -tilassa, kun ne eivät ole käytössä.

> [AZURE.NOTE] Jos suljet virtuaalikoneen sisällä AM (käyttämällä Windows power asetukset)-AM lopetetaan mutta säilyy kohdistettu. Jotta eivät enää laskuteta pysäyttämällä näennäiskoneiden [Azure perinteinen Portal](http://manage.windowsazure.com/). Voit myös lopettaa soittamalla **ShutdownRoleOperation** "PostShutdownAction" ja "StoppedDeallocated" yhtä AM PowerShellin kautta.

Sammuta ja poista virtuaalikoneen varaus:

1. Kirjaudu sisään käyttämällä tiliä [Azure perinteinen Portal](http://manage.windowsazure.com/) .  

2. Valitse vasemman reunan siirtymispalkissa **NÄENNÄISKONEIDEN** .

3. Valitse näennäiskoneiden luettelo virtuaalikoneen nimeä Valitse Siirry **RAPORTTINÄKYMÄ** -sivulle.

4. Valitse sivun alareunassa **Sammuta**.

![AM sulkeminen][15]

Virtuaalikoneen Varaus poistettu, mutta sitä ei poisteta. Virtuaalikoneen saattaa uudelleen milloin tahansa perinteinen Azure-portaalista.

## <a name="your-azure-vm-is-ready-to-use-whats-next"></a>Azure-AM käyttövalmiuden: Mitä seuraavaksi?

Virtuaalikoneen on nyt valmis käytettäväksi tietojen tiede-harjoituksissa. Virtuaalikoneen on myös valmis käytettäväksi IPython muistikirjan palvelimeksi tarkasteluun ja tiedot ja yhdessä muiden tehtävien käsittely Azure koneen oppiminen ja ryhmän tietojen tiede-prosessin.

Seuraavat vaiheet ryhmän tietojen tiede-prosessin on yhdistetty [Oppimispolku](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) ja voi sisältää ohjeet, joita tietojen siirtäminen HDInsight, käsitellä ja kuuntele sinne valmistelussa learning tiedoista Azure Konepohjaisten Oppimistekniikoiden.


[15]: ./media/machine-learning-data-science-setup-virtual-machine/vmshutdown.png
[17]: ./media/machine-learning-data-science-setup-virtual-machine/add-endpoints-after-creation.png
[18]: ./media/machine-learning-data-science-setup-virtual-machine/sample-ipnbs.png
[19]: ./media/machine-learning-data-science-setup-virtual-machine/dns-name-and-host-name.png
[20]: ./media/machine-learning-data-science-setup-virtual-machine/browser-warning-ie.png
[21]: ./media/machine-learning-data-science-setup-virtual-machine/browser-warning.png
[22]: ./media/machine-learning-data-science-setup-virtual-machine/upload-ipnb-1.png
[23]: ./media/machine-learning-data-science-setup-virtual-machine/upload-ipnb-2.png
[24]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-1.png
[25]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-2.png
[26]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-3.png
[27]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-4.png
[28]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-5.png
[29]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-6.png

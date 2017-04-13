<properties
    pageTitle="Jupyter/IPython-muistikirjan luominen | Microsoft Azure"
    description="Opi ottamaan Jupyter/IPython muistikirjan Linux virtual-laitteeseen, joka on luotu käyttämällä Azure resurssien hallinnan käyttöönotto-malli."
    services="virtual-machines-linux"
    documentationCenter="python"
    authors="crwilcox"
    manager="wpickett"
    editor=""
    tags="azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="python"
    ms.topic="article"
    ms.date="11/10/2015"
    ms.author="crwilcox"/>

# <a name="jupyter-notebook-on-azure"></a>Azure Jupyter muistikirjan

[Jupyter projektin](http://jupyter.org), aiemmin [IPython project](http://ipython.org)sisältää työkaluja tieteellisen tietojenkäsittely tehokkaita vuorovaikutteinen liittymät, joka yhdistää koodin suorittamisen live laskennallinen asiakirjan luomisen avulla. Muistikirjan nämä tiedostot voivat sisältää satunnaisia tekstin, matemaattisia kaavoja, syötteen koodi, tulokset, kuvat, videot ja media, että Moderni selain pystyy näyttäminen muita kuvia. Onko ole täysin ennen Python ja haluat lisätietoja hauska ja vuorovaikutteisia ympäristössä tai tehdä joitakin vakavia rinnakkain/tekniset tietojenkäsittely, Jupyter muistikirja on.

![Näyttökuva](./media/virtual-machines-linux-jupyter-notebook/ipy-notebook-spectral.png) käyttämällä SciPy ja Matplotlib pakettien äänen tallennus rakenteen analysointia varten.


## <a name="jupyter-two-ways-azure-notebooks-or-custom-deployment"></a>Jupyter kaksi tapaa: Azure muistikirjoja tai mukautettu käyttöönotto
Azure on palvelu, jonka avulla voit [nopeasti käyttää Jupyter ](http://blogs.technet.com/b/machinelearning/archive/2015/07/24/introducing-jupyter-notebooks-in-azure-ml-studio.aspx).  Azure-muistikirja-palvelun avulla voit helposti käyttää Jupyter's web helppokäyttöisen liittymän skaalattava laskennallinen resursseja Python tehon ja sen monta kirjastoihin.  Koska asennuksen käsitellään-palvelun, käyttäjät voivat käyttää näiden resurssien hallinta ja käyttäjän määritys edellyttää ilman.

Jos muistikirja-palvelu ei toimi käyttämässäsi skenaariossa edelleen artikkeli, jossa näkyvät ottamisesta käyttöön Microsoft Azure Jupyter-muistikirjan Linux näennäiskoneiden (VMs) avulla.

[AZURE.INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## <a name="create-and-configure-a-vm-on-azure"></a>Luominen ja AM määrittämistä Azure

Ensimmäiseksi on luotava virtual machine (AM), joka on Azure-käyttöjärjestelmässä.
Tämä AM on valmis käyttöjärjestelmän pilveen ja avulla suoritetaan Jupyter muistikirjan. Azure on voi suorittaa Linux- ja Windows näennäiskoneiden ja käsiteltävät aiheet Jupyter kummankintyyppisten näennäiskoneiden-asetukset.

### <a name="create-a-linux-vm-and-open-a-port-for-jupyter"></a>Luo Linux AM ja avaa portin Jupyter

Ohjeiden annettu [tähän] [ portal-vm-linux] virtual koneen *Ubuntu* jakauman luomiseen. Tässä opetusohjelmassa käytetään Ubuntu palvelimen 14.04 l.. Käyttäjän nimi *azureuser*oletetaan.

Kun virtuaalikoneen ottaa käyttöön annettava Avaa suojauksen säännön verkon suojaus-ryhmä.  **Verkon käyttöoikeusryhmät** Siirry Azure-portaaliin, ja avaa-välilehden suojaus-ryhmän vastaa omaa AM. Sinun on lisättävä saapuva suojaus-säännön seuraavilla asetuksilla: **TCP** -protokollan **\*** lähdeportin (julkinen) ja **9999** Kohdeportti (yksityinen).

![Näyttökuva](./media/virtual-machines-linux-jupyter-notebook/azure-add-endpoint.png)

Valitse verkko-käyttöoikeusryhmän **Verkkoliittymät** ja Huomaa **Julkiseen IP-osoite** , se tarvittaessa muodostaa yhteyttä AM seuraavassa vaiheessa.

## <a name="install-required-software-on-the-vm"></a>Asenna tarvittavat ohjelmistot AM

Suoritettaviksi sekä AM Jupyter muistikirja on on ensin asennettava Jupyter ja sen riippuvuudet. Muodostaa yhteyttä linux AM käyttämällä ssh ja käyttäjänimen ja salasanan pariliitos olet valinnut, kun loit AM. Tässä opetusohjelmassa on käyttää painovärit, muste ja yhteyden Windows.

### <a name="installing-jupyter-on-ubuntu"></a>Jupyter asennetaan Ubuntu
Asenna Anaconda, Suositut tietojen tiede python jakauman, käytä jotakin seuraavista [Continuum Analytics](https://www.continuum.io/downloads)-linkkejä.  Tämän asiakirjan kirjoittaminen vuodesta linkit alla on eniten päivämäärän versiot ylöspäin.

#### <a name="anaconda-installs-for-linux"></a>Anaconda asentaa Linux varten
<table>
  <th>Python 3.4</th>
  <th>Python 2.7</th>
  <tr>
    <td>
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86_64.sh'>64-bittinen</href>
    </td>
    <td>
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda-2.3.0-Linux-x86_64.sh'>64-bittinen</href>
    </td>
  </tr>
  <tr>
    <td>
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86.sh'>32-bittinen</href>
    </td>
    <td>
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda-2.3.0-Linux-x86.sh'>32-bittinen</href>
    </td>  
  </tr>
</table>


#### <a name="installing-anaconda3-230-64-bit-on-ubuntu"></a>Asentaminen Anaconda3 2.3.0 64-bittinen myös Ubuntu
Tämä on esimerkkinä, miten voit asentaa Anaconda Ubuntu

    # install anaconda
    cd ~
    mkdir -p anaconda
    cd anaconda/
    curl -O https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86_64.sh
    sudo bash Anaconda3-2.3.0-Linux-x86_64.sh -b -f -p /anaconda3

    # clean up home directory
    cd ..
    rm -rf anaconda/

    # Update Jupyter to the latest install and generate its config file
    sudo /anaconda3/bin/conda install jupyter -y
    /anaconda3/bin/jupyter-notebook --generate-config


![Näyttökuva](./media/virtual-machines-linux-jupyter-notebook/anaconda-install.png)


### <a name="configuring-jupyter-and-using-ssl"></a>Jupyter määrittäminen ja SSL-yhteys
Kun olet asentanut annettava hetki asetukset Jupyter määritystiedostoja. Jos kohtaat troubles määrittäminen Jupyter voi olla hyötyä Katso [Jupyter ohjeissa suoritetaan muistikirjan-palvelimen](http://jupyter-notebook.readthedocs.org/en/latest/public_server.html)kanssa.

Seuraavassa on `cd` Microsoftin SSL-varmenteen luominen ja muokkaaminen profiilit-kokoonpanotiedosto profiili-kansioon.

Linux seuraavalla komennolla.

    cd ~/.jupyter

Seuraavalla komennolla voit luoda SSL-varmenne (Linux ja Windows).

    openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem

Huomaa, että, koska luodaan itse allekirjoitettua SSL-varmenne, kun yhteyden muistikirjan selaimen avulla voit suojausvaroitus.  Pitkään tuotannon käyttöä haluat käyttää organisaation liittyvät oikein allekirjoitettua varmennetta.  Varmenteen hallinta on tämä esittely laajemmin, emme helppokäyttöisiä itse allekirjoitetun varmenteen voi nyt.

Lisäksi sertifikaatilla, sinun on määritettävä myös siihen salasanan muistikirjan luvattomalta käytöltä.  Tietoturvasyistä Jupyter käyttää salatut salasanat sen määritystiedostossa, joten sinun on salaamaan salasanasi.  IPython sisältää apuohjelman kiellä; komentokehotteeseen varten suorittamalla seuraavan komennon.

    /anaconda3/bin/python -c "import IPython;print(IPython.lib.passwd())"

Tämä pyytää salasanaa ja vahvistuksen ja sitten tulostaa salasana. Huomautus Tämä seuraavassa vaiheessa.

    Enter password:
    Verify password:
    sha1:b86e933199ad:a02e9592e59723da722.. (elided the rest for security)

Seuraavaksi on Muokkaa profiilin kokoonpanotiedosto, joka on `jupyter_notebook_config.py` kansion olet käyttämässä.  Huomaa, että tämä tiedosto ei ehkä ole – luo se vain, jos näin on.  Tämä tiedosto on useita kenttiä ja oletusarvoisesti kaikki on Kommentoitu ulos.  Voit halutessasi avata tätä tiedostoa tekstieditorissa mielesi ja varmista, että siinä on vähintään seuraavan sisällön. **Varmista, että korvaa c.NotebookApp.password kanssa kuin edellisessä vaiheessa sha1 config**.

    c = get_config()

    # You must give the path to the certificate file.
    c.NotebookApp.certfile = u'/home/azureuser/.jupyter/mycert.pem'

    # Create your own password as indicated above
    c.NotebookApp.password = u'sha1:b86e933199ad:a02e9592e5 etc... '

    # Network and browser details. We use a fixed port (9999) so it matches
    # our Azure setup, where we've allowed traffic on that port
    c.NotebookApp.ip = '*'
    c.NotebookApp.port = 9999
    c.NotebookApp.open_browser = False

### <a name="run-the-jupyter-notebook"></a>Suorita Jupyter-muistikirja

Tässä vaiheessa emme jatkaa Jupyter muistikirja. Toiminto, siirry kansioon, tallentaa muistikirjojen ja Käynnistä Jupyter muistikirjan palvelin seuraavalla komennolla.

    /anaconda3/bin/jupyter-notebook

Jupyter muistikirjaa osoitteessa pitäisi nyt `https://[PUBLIC-IP-ADDRESS]:9999`.

Kun muistikirjan käyttöoikeudet, kirjaudu sisään-sivulla pyytää salasanasi. Ja kun kirjaudut, näet "Jupyter muistikirjan koontinäyttö", joka on kaikkien muistikirjan toimille ohjausmerkit keskelle.  Tällä sivulla voit luoda uudet muistikirjat ja avata aiemmin luotuja.

![Näyttökuva](./media/virtual-machines-linux-jupyter-notebook/jupyter-tree-view.png)

### <a name="using-the-jupyter-notebook"></a>Käyttämällä Jupyter-muistikirja

Jos valitset **Uusi** -painiketta, näet seuraavat aloitussivu.

![Näyttökuva](./media/virtual-machines-linux-jupyter-notebook/jupyter-untitled-notebook.png)

Alueen merkitty `In []:` on kehote kirjoitusaluetta, voit kirjoittaa kelvollinen Python-koodi tähän ja se suoritetaan, kun valitset `Shift-Enter` tai napsauta "toista-kuvaketta (oikealle osoittavaksi kolmion työkalurivillä).

## <a name="a-powerful-paradigm-live-computational-documents-with-rich-media"></a>Tehokas koaksiaalikaapeliin: live laskennallinen rich media-asiakirjojen käyttäminen

Muistikirjan itse olisi tuntuu hyvin luonnollinen kaikki, jotka on käyttänyt Python ja word-suoritin, koska se on joitakin tapoja, joilla niiden sekoitusta: Voit suorittaa lohkot Python koodin, mutta voit myös pitää muistiinpanot ja muuta tekstiä muuttamalla soluun "Koodista" "Korotuksia" tyylin käyttäminen työkalurivin avattavasta valikosta.

Jupyter on paljon suurempi kuin word-suoritin, kun se sallii sekoittamalla laskenta ja rich media (tekstiä, kuvia, videoita ja lähes mitä tahansa Moderni selain näyttää). Voit käyttää, teksti, koodi, videoita ja lisää!

![Näyttökuva](./media/virtual-machines-linux-jupyter-notebook/jupyter-editing-experience.png)

Ja Python on monta erinomainen kirjastojen tieteellisen ja teknisen power kanssa, seuraavassa näyttökuvassa yksinkertaisen laskutoimituksen voidaan suorittaa saman vaivattomasti kuin monitasoista analyysi-kaikki yhden ympäristössä.

Tämä koaksiaalikaapeliin on sekoitus Moderni verkossa power live laskenta tarjoaa useita vaihtoehtoja ja sopii Ihannetapauksessa pilveen; Muistikirjan voidaan käyttää:

* Tallenna kokeilevaa laskennallinen piirustuslehtiön kuin käyttää ongelma.

* Jos haluat jakaa tuloksia live' laskennallinen lomakkeen tai tulosteina muodossa (HTML, PDF) työtovereiden kanssa.

* Jakaminen ja esittää reaaliaikaisia opettaminen materiaalien, joihin sisältyy laskenta, jotta opiskelijat heti kokeilla reaali koodin, muokkaa sitä ja Suorita uudelleen vuorovaikutteisesti.

* Anna "suoritettavan papereita", joka esittää tutkimustulosten niin, että voit heti uudestaan, vahvistetaan ja muut laajennettu.

* Ympäristön yhteiskäytön lasketaan muodossa: useat käyttäjät voivat kirjautua samassa muistikirjan palvelimessa, voit jakaa reaaliajassa laskennallinen istunnon.


Siirtyessäsi IPython lähde koodin [säilöön][], löydät koko hakemistosta, muistikirjan esimerkkejä, jotka voit ladata ja sitten kokeilla käyttöön oman Azure Jupyter AM kanssa.  Lataa vain `.ipynb` sivuston tiedostoja ja lataa ne muistikirjan Azure AM Raporttinäkymät-ikkunan päälle (tai ladata ne suoraan AM).

## <a name="conclusion"></a>Tekemistä

Jupyter muistikirja on tehokas käyttöliittymä käyttämiseen vuorovaikutteisesti Azure Python-ekosysteemiin potenssiin.  Se peittää käyttö tilanteissa, mukaan lukien yksinkertainen tarkasteluun ja koulujen Python, tietojen analysointi ja visualisointi, simuloinnissa ja rinnakkaisia tietojenkäsittely monien. Tuloksena oleva muistikirja-tiedostoissa tietueeksi, funktiolauseita, joka suoritetaan, ja voit jakaa Jupyter muiden käyttäjien kanssa.  Jupyter muistikirjan voidaan käyttää paikallisen sovelluksen, mutta se sopii Ihannetapauksessa cloud-versioiden Azure

Jupyter core-ominaisuudet ovat käytettävissä myös sisällä Visual Studio [Python Tools for Visual Studio][] (PTVS) kautta. PTVS on maksuton ja Avaa-lähde-laajennuksen Microsoftilta, joka muuttuu Visual Studio Lisäasetukset Python kehitysympäristö, joka sisältää IntelliSense-virheenkorjaus, jossa laajennettu editori profilointi ja rinnakkaisia integrointi.

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja on artikkelissa [Python Developer Center](/develop/python/).

[portal-vm-linux]: https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-tutorial-portal-rm/
[säilöön]: https://github.com/ipython/ipython
[Python Tools for Visual Studio]: http://aka.ms/ptvs

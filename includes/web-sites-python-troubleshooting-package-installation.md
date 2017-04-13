Jotkin paketit saa asentaa pip suoritettaessa Azure avulla.  Vain lähettää paketin ei ole käytettävissä Python Package indeksi.  Voi olla esimerkiksi kääntäjä tarvitaan (esimerkiksi kääntäjä ei ole käytettävissä tietokoneessa, jossa web-sovelluksen Azure App Service).

Tässä osiossa tarkastellaan ongelman käsitellä tavalla.

### <a name="request-wheels"></a>Pyydä vierityspainikkeet

Jos paketin asennuksen edellyttää esimerkiksi kääntäjä, kokeile ottaa yhteyttä paketin omistajaan pyytää, että vierityspainikkeet käytettäväksi paketille.

[Python 2.7 Microsoft Visual C++-kääntäjän][]viimeisimmät käytettävissä on nyt helpompi luoda paketteja, jotka on alkuperäisen koodin Python 2.7.

### <a name="build-wheels-requires-windows"></a>Muodosta vierityspainikkeet (edellyttää Windows)

Huomautus: Tämä vaihtoehto muista kääntää paketin käyttämällä Python-ympäristössä, joka vastaa ympäristö/arkkitehtuuri/versio, jota käytetään sovelluksen Azure-palvelu web-sovelluksen (Windows/32-bit/2.7 tai 3.4).

Jos paketin asentaminen ei onnistu, koska se edellyttäisi esimerkiksi kääntäjä, voit asentaa kääntäjä paikallisessa tietokoneessa ja luoda kiekkopainiketta paketin, jossa sisällytettävien sitten lisääminen säilöön.

Mac tai Linux-käyttäjät: Jos ei ole käyttö Windows-tietokoneeseen, katso, miten voit luoda AM Azure [Luo virtuaalikoneen käynnissä Windows][] .  Voit käyttää sitä vierityspainikkeet luominen ja lisääminen säilöön hylkää AM Jos haluat. 

Python 2.7 voit asentaa [Microsoft Visual C++ kääntäjän Python 2.7 varten][].

Python 3.4 olet asentanut [Microsoft Visual C++ 2010 Express][].

Luoda vierityspainikkeet, sinun on kiekkopainiketta package:

    env\scripts\pip install wheel

Käytät `pip wheel` riippuvuus kääntäminen:

    env\scripts\pip wheel azure==0.8.4

Tämä luo .whl tiedoston \wheelhouse-kansiossa.  Lisää \wheelhouse kansio ja kiekkopainiketta tiedostot oman-säilöön.

Voit lisätä oman requirements.txt Muokkaa `--find-links` vaihtoehto yläreunassa. Tämä kertoo pip Tarkista tarkkaa vastinetta paikallisessa kansiossa, ennen kuin siirryt python package indeksi.

    --find-links wheelhouse
    azure==0.8.4

Jos haluat sisällyttää kaikki riippuvuudet \wheelhouse-kansio ja käytä python paketin hakemiston lainkaan, voit pakottaa pip ohittamaan lisäämällä paketin hakemiston `--no-index` oman requirements.txt yläreunaan.

    --no-index

### <a name="customize-installation"></a>Mukauta asennusta

Voit mukauttaa käyttöönoton komentosarja asentaa paketin virtual ympäristössä, käyttää vaihtoehtoinen installer, kuten helposti\_Asenna.  Katso esimerkki, joka on Kommentoitu ulos deploy.cmd.  Varmista, että näiden pakettien yhteystiedoissasi requirements.txt, voit estää pip niiden asentamista.

Lisää tämä käyttöönoton komentosarja:

    env\scripts\easy_install somepackage

Käytä helposti ehkä myös\_asennuksen ja asennuksen exe installer (jotkin ovat zip-yhteensopiva, helppoa\_Asenna tukee niitä).  Lisää asennusohjelma oman-säilöön ja käynnistää helposti\_asentaa siirtämällä suoritettavan tiedoston polku.

Lisää tämä käyttöönoton komentosarja:

    env\scripts\easy_install "%DEPLOYMENT_SOURCE%\installers\somepackage.exe"

### <a name="include-the-virtual-environment-in-the-repository-requires-windows"></a>Sisällytä näennäisen ympäristön tietovarasto (edellyttää Windows)

Huomautus: Tämä vaihtoehto muista käyttää virtual ympäristössä, joka vastaa ympäristö/arkkitehtuuri/versio, jota käytetään sovelluksen Azure-palvelu web-sovelluksen (Windows/32-bit/2.7 tai 3.4).

Jos lisäät näennäisen ympäristön säilössä, voit estää käyttöönoton komentosarja tekemästä näennäisen ympäristön hallinnan Azure luomalla tyhjän tiedoston:

    .skipPythonDeployment

On suositeltavaa, että poistat aiemmin virtual ympäristön sovellukseen, jotta mittausvirheistä tiedostoja kun virtual ympäristöön on hallitaan automaattisesti.


[Luo käyttöjärjestelmässä Virtual-Machine]: http://azure.microsoft.com/documentation/articles/virtual-machines-windows-hero-tutorial/
[Microsoft Visual C++ kääntäjä Python 2.7]: http://aka.ms/vcpython27
[Microsoft Visual C++ 2010 Express]: http://go.microsoft.com/?linkid=9709949

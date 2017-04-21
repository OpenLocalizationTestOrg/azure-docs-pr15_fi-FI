#<a name="how-to-use-the-azure-command-line-tools-for-mac-and-linux"></a>Azure komentorivivalitsimet työkalujen käyttämisestä Mac-ja Linux

Tässä oppaassa kerrotaan, miten for Macin ja Linux Azure komentorivivalitsimet-työkalujen avulla voit luoda ja hallita Azure palvelut. Tilanteita, joissa kattaa ovat **asentaminen työkaluja**, **julkaisun asetusten tuominen**, **luomisen ja ylläpidon Azure sivustot**ja **luomisen ja ylläpidon Azuren näennäiskoneiden**. Katso täydellinen oppaat [Azure työkalun for Macin ja Linux dokumentaatio][reference-docs]. 

##<a name="table-of-contents"></a>Sisällysluettelo
* [Mitkä ovat Azure komentorivivalitsimet Tools for Macin ja Linux](#Overview)
* [Asentamisesta Mac- ja Linux Azure komentorivivalitsimet Työkalut](#Download)
* [Voit luoda Azure-tili](#CreateAccount)
* [Voit ladata ja tuo Julkaisuasetukset](#Account)
* [Voit luoda ja Azure-sivuston hallinta](#WebSites)
* [Voit luoda ja hallita Azure-virtuaalikoneen](#VMs)


<h2><a id="Overview"></a>Mitkä ovat Azure komentorivivalitsimet Tools for Macin ja Linux</h2>

Azure komentorivivalitsimet Tools for Macin ja Linux ovat komentorivin työkaluja käyttöönotto ja Azure palveluiden hallinta.
 
Tuetut tehtävät ovat seuraavat:

* Tuo julkaisun asetukset.
* Luo ja Azure sivustojen hallinta.
* Luoda ja hallita Azuren näennäiskoneiden.

Täydellinen luettelo tuetuista komennoista, kirjoita `azure -help` komentorivillä asennettuasi Työkalut-valikon tai [oppaat][reference-docs].

<h2><a id="Download">Asentamisesta Mac- ja Linux Azure komentorivivalitsimet Työkalut</a></h2>

Seuraava luettelo sisältää tietoja komentorivin työkalujen käyttöjärjestelmästä riippuen asentaminen:

* **Mac**: Lataa [Azure SDK Installer][mac-installer]. Avaa ladatut .pkg-tiedosto ja asennuksen vaiheet niin kehottaessa.

* **Linux**: Asenna uusin versio [Node.js] [ nodejs-org] (katso [Asentaa Node.js kautta pakettien hallinta][install-node-linux]), suorita seuraava komento:

        npm install azure-cli -g

    **Huomautus**: Tämä komento oikeuksin suoritettava joudut ehkä:

        sudo npm install azure-cli -g

* **Windows**: Suorita Winows installer (MSI-tiedosto), joka on saatavilla täällä: [Azure komentorivityökalujen][windows-installer].


Voit testata asennuksen, kirjoita `azure` komentokehotteen. Jos asennus onnistui, näkyviin tulee luettelo kaikkien käytettävissä olevien `azure` komennot.

<h2><a id="CreateAccount"></a>Voit luoda Azure-tili</h2>

Työkaluilla Azure komentorivivalitsimet for Macin ja Linux, tarvitset Azure-tili.

Avaa selain ja siirry [http://www.windowsazure.com] [ windowsazuredotcom] ja valitse **maksuttoman kokeiluversion** oikeassa yläkulmassa.

![Azure Web-sivusto][Azure Web Site]

Tilin luominen ohjeiden mukaisesti.

<h2><a id="Account"></a>Lataamisesta ja tuo Julkaisuasetukset</h2>

Aluksi sinun on ensin ladattava ja tuoda oman Julkaisuasetukset. Näin voit luoda ja hallita Azure Services-työkalujen avulla. Voit ladata oman Julkaisuasetukset, käytä `account download` komento:

    azure account download

Tämä Avaa oletusselaimessa ja pyytää kirjautumaan hallinta-portaalin. Kun olet kirjautunut sisään, että `.publishsettings` tiedosto ladataan. Tämän tiedoston tallennuspaikan muistiin.

Tuo seuraavaksi `.publishsettings` tiedoston suorittamalla seuraavan komennon korvaaminen `{path to .publishsettings file}` polku oman `.publishsettings` tiedosto:

    azure account import {path to .publishsettings file}

Voit poistaa kaikki tallentamat tiedot <code>import</code> -komennon avulla <code>account clear</code> komento:

    azure account clear

Saat näkyviin luettelon vaihtoehdoista `account` komentoja, käytä `-help` vaihtoehto:

    azure account -help

Kun olet tuonut oman Julkaisuasetukset, sinun on poistettava `.publishsettings` tiedoston tietoturvasyistä.

> [AZURE.NOTE] Kun tuot Julkaisuasetukset, tunnistetietojen käyttäminen Azure tilauksen tallennetaan sisällä oman `user` kansio. Oman `user` kansio on suojattu käyttöjärjestelmästä. On suositeltavaa ottaa lisätoimia salaamaan oman `user` kansio. Voit tehdä seuraavasti:    
> 
> - Windows-kansion ominaisuuksien muokkaaminen tai BitLockerin.
> - Mac-käyttöön FileVault kansion.
> - Valitse Ubuntu salattu aloitus hakemisto-toiminnon avulla. Muut Linux jaot tarjoavat vastaavia toimintoja.

Olet nyt valmiina parhaillaan luomisen ja ylläpidon Azure sivustot ja Azuren näennäiskoneiden.  

<h2><a id="WebSites"></a>Voit luoda ja hallita Azure-sivusto</h2>

###<a name="create-a-website"></a>Luo sivusto

Voit luoda Azure verkkosivuston, Luo kutsutaan tyhjää kansiota `MySite` ja Etsi kyseiseen kansioon.

Suorita seuraava komento:

    azure site create MySite --git

Tämä komento tulosteen sisältää juuri luomasi sivuston oletusarvoinen URL-osoite. `--git` Asetuksen avulla voit julkaista verkkosivuston luomalla git säilöjen tietoihin sekä paikallisen sovelluksen hakemistossa ja muuttaa sivuston tietokeskuksen git avulla. Huomaa, että jos paikallisesta kansiosta on jo git säilö, komento lisää uusi etäyhteyksien aiemmin säilöön valitsemalla muuttaa sivuston data Centerissä säilöön.

Huomaa, että voit suorittaa `azure site create` komento on jokin seuraavista vaihtoehdoista:

* `--location [location name]`. Tämän asetuksen avulla voit määrittää sijainnin tietokeskuksen, jossa sivuston luomisen (esimerkiksi "Länsi US"). Jos tämä vaihtoehto, voi promted sijainti.
* `--hostname [custom host name]`. Tämän asetuksen avulla voit määrittää mukautetun hostname-sivustoon.

Voit lisätä sisältöä Valitse sivusto-kansioon. Käyttää Normaali git kulun (`git add`, `git commit`) vahvistusta sisältöä. Seuraavat git-komennon avulla voit siirtää sivuston sisällön Azure: 

    git push azure master

###<a name="set-up-publishing-from-github"></a>Määritä GitHub julkaiseminen

Voit määrittää jatkuva julkaiseminen GitHub säilöstä `--GitHub` asetus sivuston luomisen yhteydessä:

    auzre site create MySite --github --githubusername username --githubpassword password --githubrepository githubuser/reponame

Jos käytössäsi on paikallinen Kloonaa GitHub tietovaraston tai jos tietokoneeseesi on säilön kanssa GitHub säilöön viittauksen yhteen remote-komennon automaattisesti julkaista koodin GitHub säilössä sivustoon. Tämän jälkeen haluamasi muutokset, miten GitHub säilöön automaattisesti julkaistava sivustoon.

Kun määrität julkaisun GitHub, käytetään oletusarvon haara on perusmuodon haara. Jos haluat määrittää eri haara, suorita seuraava komento että paikallinen säilöstä:

    azure site repository <branch name>

###<a name="configure-app-settings"></a>Sovelluksen asetusten määrittäminen

Sovelluksen asetukset ovat avain-arvo paria, jotka ovat käytettävissä sovelluksen suorituksen aikana. Jos määrität Azure-sivustoa, sovelluksen arvon ohittaa asetukset samalla avaimella, jotka on määritetty sivuston korostetut. Node.js ja PHP sovellusten app-asetukset ovat käytettävissä ympäristömuuttujat. Seuraavassa esimerkissä kerrotaan, miten voit määrittää avain-arvo-pari:

    azure site config add <key>=<value> 

Jos haluat nähdä luettelon kaikista avain/arvo paria, käyttämällä seuraavaa:

    azure site config list 

Tai jos tiedät avain ja haluat nähdä arvon, voit:

    azure site config get <key> 

Jos haluat muuttaa olemassa olevan avaimen Poista ensin olemassa oleva avain ja lisää se sitten uudelleen. Poista-komento on:

    azure site config clear <key> 

###<a name="list-and-show-sites"></a>Luettelo- ja Näytä sivustot

Luettelo oman sivuston, käytä seuraavaa komentoa:

    azure site list

Saat lisätietoja sivuston, käytä `site show` komento. Seuraavassa esimerkissä esitetään tiedot `MySite`:

    azure site show MySite

###<a name="stop-start-or-restart-a-site"></a>Lopeta ja Käynnistä sivuston uudelleen

Voit lopettaa, Käynnistä tai sivusto on käynnistettävä uudelleen `site stop`, `site start`, tai `site restart` komentoja:

    azure site stop MySite
    azure site start MySite
    azure site restart MySite

###<a name="delete-a-site"></a>Sivuston poistaminen

Lopuksi voit poistaa sivuston, jossa `site delete` komento:

    azure site delete MySite

Jos käytössäsi on jokin seuraavista komennoista yläpuolella kansioon, johon suoritit `site create`, sinun ei tarvitse määrittää sivuston nimi `MySite` viimeisen parametrina.

Saat näkyviin täydellisen luettelon `site` komentoja, käytä `-help` vaihtoehto:

    azure site -help 

<h2><a id="VMs"></a>Voit luoda ja hallita Azure-virtuaalikoneen</h2>

Azure-virtuaalikoneen luodaan virtuaalikoneen kuvasta (.vhd-tiedosto) antamasi tai, joka on kuva-valikoimassa. Jos haluat nähdä kuvat, jotka ovat käytettävissä, käytä `vm image list` komento:

    azure vm image list

Voit valmistella ja käynnistettäessä olevista kanssa käytettävissä olevat kuvat `vm create` komento. Seuraavassa esimerkissä luomisesta Linux-virtual machine (jota kutsutaan `myVM`) kuvan kuva-valikoimassa (CentOS 6.2). Pääkansio käyttäjänimi ja salasana virtuaalikoneen `myusername` ja `Mypassw0rd` tarpeen mukaan. (Huomaa, että `--location` parametri ilmaisee tietokeskuksen, jossa virtuaalikoneen on luotu. Jos jätät `--location` parametri, kehotteessa pyydetään valitsemaan.)

    azure vm create myVM OpenLogic__OpenLogic-CentOS-62-20120509-en-us-30GB.vhd myusername --location "West US"

Kannattaa harkita lähi `--ssh` merkinnän (Linux) tai `--rdp` merkintä (Windows) `vm create` etäyhteyksien tietokoneella näennäismuistin luomasi käyttöön.

Jos mieluummin valmistella virtual koneen mukautetun kuvasta, voit luoda kuvan .vhd-tiedosto, jonka `vm image create` komento ja valitse Käytä `vm create` valmistelu virtuaalikoneen komento. Seuraavassa esimerkissä luomisesta Linux-kuva (jota kutsutaan `myImage`) paikallisen .vhd-tiedostosta. ( `--location` Parametri ilmaisee, jossa kuva on tallennettu tiedot.)

    azure vm image create myImage /path/to/myImage.vhd --os linux --location "West US"

Sen sijaan, että luominen paikalliseen .vhd kuvan, voit luoda kuvan .vhd, Azuren Blob-objektien tallennustilaan tallennettuja. Voit tehdä tämän kanssa `blob-url` parametri:

    azure vm image create myImage --blob-url <url to .vhd in Blob Storage> --os linux

Kun olet luonut kuvan, voit valmistella virtual kone kuvasta käyttämällä `vm create`. Alla oleva komento luo kutsutaan virtual koneen `myVM` edellä luotu kuvasta (`myImage`).

    azure vm create myVM myImage myusername --location "West US"

Kun on valmisteltu virtual machine, haluat ehkä luoda päätepisteet sallimaan virtuaalikoneen etäkäyttö (esimerkiksi). Seuraavassa esimerkissä `vm create endpoint` komennon ulkoisten porteista 22 ja paikallisen portin 22 avaamiseen `myVM`:

    azure vm endpoint create myVM 22 22

Saa yksityiskohtaisia tietoja (kuten IP-osoite, DNS-nimi ja tiedot) virtual kone `vm show` komento:

    azure vm show myVM

Sammuta Käynnistä-painiketta, tai virtuaalikoneen uudelleen, käytä jotakin seuraavista komennoista:

    azure vm shutdown myVM
    azure vm start myVM
    azure vm restart myVM

Ja lopuksi Poista AM `vm delete` komento:

    azure vm delete myVM

Komentojen luomisen ja ylläpidon näennäiskoneiden kattavaan luetteloon, käytä `-h` vaihtoehto:

    azure vm -h

<!-- IMAGES -->
[Azure Web Site]: ./media/crossplat-cmd-tools/freetrial.png

<!-- LINKS -->
[nodejs-org]: http://nodejs.org/
[install-node-linux]: https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager
[mac-installer]: http://go.microsoft.com/fwlink/?LinkId=252249
[windows-installer]: http://go.microsoft.com/fwlink/?LinkID=275464
[reference-docs]: http://go.microsoft.com/fwlink/?LinkId=252246
[windowsazuredotcom]: http://www.windowsazure.com


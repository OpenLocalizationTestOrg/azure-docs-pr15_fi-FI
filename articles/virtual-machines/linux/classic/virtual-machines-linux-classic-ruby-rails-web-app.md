<properties
    pageTitle="Ruby kulkevia sivustossa Linux AM isännöidä | Microsoft Azure"
    description="Määritä ja isännöi Ruby Azure käyttämällä Linux virtual koneen kiskot-sivustossa."
    services="virtual-machines-linux"
    documentationCenter="ruby"
    authors="rmcmurray"
    manager="wpickett"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="web"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="ruby"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="ruby-on-rails-web-application-on-an-azure-vm"></a>Ruby Azure-AM kulkevia verkkosovelluksen

Tässä opetusohjelmassa esitetään, kuinka voit isännöidä Ruby kulkevia sivustossa Azure Linux virtual-tietokoneen avulla.  

Tässä opetusohjelmassa on vahvistettu käyttämällä Ubuntu palvelimen 14.04 l.. Jos käytät eri Linux-jakauman, voit joutua muuttamaan ohjeiden avulla voit asentaa kulkevia.

> [AZURE.IMPORTANT] Azure on kaksi eri käyttöönoton mallien luominen ja käyttäminen resurssit: [Resurssienhallinta ja perinteinen](../../../resource-manager-deployment-model.md).  Tässä artikkelissa käsitellään perinteinen käyttöönotto-mallin. Microsoft suosittelee, että useimmat uudet käyttöönoton käyttävät Resurssienhallinta-malli.

## <a name="create-an-azure-vm"></a>Luo Azure AM

Aloita luomalla Azure-AM Linux-kuva.

Luodaksesi AM, voit käyttää Azure perinteinen portaalin tai Azure käyttöliittymä (CLI).

### <a name="azure-management-portal"></a>Azure hallinta-portaalissa

1. Kirjaudu sisään [Azure perinteinen portal](http://manage.windowsazure.com)
2. Valitse **Uusi** > **Laske** > **virtuaalikoneen** > **nopea luominen**. Valitse Linux-kuva.
3. Kirjoita salasana.

Kun valmistelun yhteydessä AM, napsauta AM nimeä ja valitse **raporttinäkymät-ikkunan**. Etsi SSH päätepisteen **SSH tiedot**-kohdasta.

### <a name="azure-cli"></a>Azure CLI

[Luo virtuaalikoneen käynnissä Linux]noudattamalla[vm-instructions].

Kun valmistelun yhteydessä AM, voit hankkia SSH päätepisteen suorittamalla seuraavan komennon:

    azure vm endpoint list <vm-name>  

## <a name="install-ruby-on-rails"></a>Asenna Ruby kiskoilla

1. SSH avulla voit muodostaa yhteyden AM.

2. SSH istunnosta käyttää Ruby asennetaan AM seuraavia komentoja:

        sudo apt-get update -y
        sudo apt-get upgrade -y
        sudo apt-get install ruby ruby-dev build-essential libsqlite3-dev zlib1g-dev nodejs -y

    Asennus kestää muutaman minuutin kuluttua. Kun se on valmis, käytä seuraavaa komentoa voit varmistaa, että Ruby on asennettu:

        ruby -v

    Palauttaa arvon, joka asennettiin Ruby-version.

3. Seuraavalla komennolla kulkevia asentaminen:

        sudo gem install rails --no-rdoc --no-ri -V

    Käytä--ei-sanan "RDOC" ja--ei o liput Ohita asentaminen asiakirjat, mikä on nopeampaa.
    Tämä komento todennäköisesti kestää kauan suoritetaan, siten lisääminen -V Näyttää tietoja asennuksen edistymisestä.

## <a name="create-and-run-an-app"></a>Luominen ja suorittaminen sovelluksen

Kirjautuneena edelleen SSH kautta, suorita seuraavat komennot:

    rails new myapp
    cd myapp
    rails server -b 0.0.0.0 -p 3000

[Uusi](http://guides.rubyonrails.org/command_line.html#rails-new) komento luo uuden kulkevia sovelluksen. [Palvelimesta](http://guides.rubyonrails.org/command_line.html#rails-server) -komento aloittaa WEBrick verkkopalvelin kulkevia mukana tulevaa. (Tuotannon käytettäväksi, todennäköisesti haluat käyttää eri palvelimeen, kuten Unicorn tai matkustaja.)

Raportissa pitäisi näkyä tulosteen seuraavankaltaiselta.

    => Booting WEBrick
    => Rails 4.2.1 application starting in development on http://0.0.0.0:3000
    => Run `rails server -h` for more startup options
    => Ctrl-C to shutdown server
    [2015-06-09 23:34:23] INFO  WEBrick 1.3.1
    [2015-06-09 23:34:23] INFO  ruby 1.9.3 (2013-11-22) [x86_64-linux]
    [2015-06-09 23:34:23] INFO  WEBrick::HTTPServer#start: pid=27766 port=3000

## <a name="add-an-endpoint"></a>Lisää päätepisteen

1. Siirry [Azure perinteinen portal] [ management-portal] ja valitse kohdassa AM.

    ![virtuaalikoneen luettelo][vmlist]

2. Valitse sivun yläosassa **PÄÄTEPISTEET** ja valitse sitten **+ Lisää PÄÄTEPISTEEN** sivun alareunassa.

    ![päätepisteet-sivulla][endpoints]

3. **Lisää PÄÄTEPISTEEN** -valintaikkunassa valitsemalla "Lisää erillinen päätepisteen" ja valitse **Seuraava** -nuoli.

    ![Uusi päätepisteen-valintaikkuna][new-endpoint1]

3. Kirjoita valintaikkunan seuraavalla sivulla seuraavat tiedot:

    * **Nimi**: HTTP

    * **PROTOKOLLA**: TCP

    * **Julkinen portti**: 80

    * **Yksityinen portti**: 3000

    Tämä luo Julkinen portti 80, joka reitittää liikenteen yksityinen portti 3000, jossa kulkevia palvelimen seuraa.

    ![Uusi päätepisteen-valintaikkuna][new-endpoint]

4. Napsauta Tallenna päätepiste valintamerkkiä.

5. Viestin pitäisi näkyä, joka ilmoittaa **Päivitys käynnissä**. Kun tämä viesti ei enää näy, päätepisteen on aktiivinen. Voit nyt testata sovelluksen siirtymällä virtuaalikoneen DNS-nimi. Sivuston pitäisi näyttää seuraavankaltaiselta:

    ![kiskot oletussivun][default-rails-cloud]

## <a name="next-steps"></a>Seuraavat vaiheet

Tässä opetusohjelmassa teit useimmat vaiheita manuaalisesti. Tuotantoympäristössä jonka kirjoittaa sovelluksen kehittämisen tietokoneeseen ja Azure AM käyttöön. Useimmissa tuotannon ympäristöissä ylläpitää myös yhdessä toisen palvelimen prosessin Apache tai NginX, mitä kahvoja pyytää reitittämisestä useita kulkevia sovelluksen esiintymät ja kohdistetussa kiinteät resurssit, kuten kiskot-sovellus. Lisätietoja on artikkelissa http://rubyonrails.org/deploy/.

Saat lisätietoja Ruby kiskoilla käy [Ruby kulkevia apuviivat][rails-guides].

Azure services Ruby-sovelluksesta on artikkelissa:

* [Tallentaa erimuotoisia tietoja käyttämällä BLOB-objektit][blobs]

* [Säilön avain/arvo-pareina taulukoiden avulla][tables]

* [Tukee suuren kaistanleveyden sisältöä sisällön toimituksen verkoston kanssa][cdn-howto]

<!-- WA.com links -->
[blobs]: ../../../storage/storage-ruby-how-to-use-blob-storage.md
[cdn-howto]: https://azure.microsoft.com/develop/ruby/app-services/
[management-portal]: https://manage.windowsazure.com/
[tables]: ../../../storage/storage-ruby-how-to-use-table-storage.md
[vm-instructions]: ../../virtual-machines-linux-classic-createportal.md

<!-- External Links -->
[rails-guides]: http://guides.rubyonrails.org/
[sqlite3]: http://www.sqlite.org/

<!-- Images -->

[default-rails-cloud]: ./media/virtual-machines-linux-classic-ruby-rails-web-app/basicrailscloud.png
[vmlist]: ./media/virtual-machines-linux-classic-ruby-rails-web-app/vmlist.png
[endpoints]: ./media/virtual-machines-linux-classic-ruby-rails-web-app/endpoints.png
[new-endpoint]: ./media/virtual-machines-linux-classic-ruby-rails-web-app/newendpoint.png
[new-endpoint1]: ./media/virtual-machines-linux-classic-ruby-rails-web-app/newendpoint1.png

<properties 
    pageTitle="Windowsin SSH näppäimet käyttäminen Linux VMs | Microsoft Azure" 
    description="Opettele luominen ja muodostaa Linux virtual tietokoneen Azure Windows-tietokoneessa SSH näppäimet avulla." 
    services="virtual-machines-linux" 
    documentationCenter="" 
    authors="squillace" 
    manager="timlt" 
    editor=""
    tags="azure-service-management,azure-resource-manager" />

<tags 
    ms.service="virtual-machines-linux" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="rasquill"/>

# <a name="how-to-use-ssh-keys-with-windows-on-azure"></a>Voit käyttää SSH näppäimiä Windows Azure-

> [AZURE.SELECTOR]
- [Windows](virtual-machines-linux-ssh-from-windows.md)
- [Linux/Mac](virtual-machines-linux-mac-create-ssh-keys.md)

Kun muodostat yhteyden Azure Linux-näennäiskoneiden (VMs)-suojausta lisäämistapaa kirjautua Linux AM [julkisen avaimen](https://wikipedia.org/wiki/Public-key_cryptography) avulla. Tässä prosessissa on julkisista ja yksityisistä avaimen exchange, suojattu runko (SSH)-komennon avulla tarkistamiseen itse sijaan käyttäjänimeä ja salasanaa. Salasanat ovat kannalta-pakottaa Enterprisessa, erityisesti Internetiin yhteydessä oleva VMs, kuten verkko-palvelimiin. Tässä artikkelissa on yleiskatsaus SSH näppäimet ja miten voit luoda haluamasi näppäimet Windows-tietokoneessa.


## <a name="overview-of-ssh-and-keys"></a>Yleisiä tietoja SSH ja avaimet

Voit turvallisesti kirjautua Linux AM käyttämällä julkiset ja yksityiset avaimet:

- **Julkinen avain** on sijoitettu Linux AM tai muu palvelu, jota haluat käyttää julkisen avaimen.
- **Yksityinen avain** on, mitä voit esittää Linux AM silloin, kun kirjaudut sisään, jäsenyytesi. Suojaa yksityinen avain. Älä jaa sitä.

Nämä julkiset ja yksityiset avaimet voidaan useita VMs ja palveluista. Näppäimet kahdet ei tarvitse kunkin AM tai palvelu, jota haluat käyttää. Katso tarkempia yleiskatsaus [julkisen avaimen](https://wikipedia.org/wiki/Public-key_cryptography).

SSH on salattua yhteyttä-protokolla, joka mahdollistaa suojatun kirjautumiset suojaamaton yhteyksillä. Linux VMs ylläpidettävä Azure yhteyden protokolla on. Vaikka SSH itse tarjoaa salattua yhteyttä, salasanojen SSH yhteyksiä edelleen jättää AM koske väsytysmenetelmiä kalastelu tai arvata salasanoja. Entistä turvallisempi, ja ensisijaisen, yhteyden muodostaminen AM, käyttämällä SSH keinot on käyttää näitä julkiset ja yksityiset avaimet, eli SSH avaimet.

Jos et halua käyttää SSH näppäimet, voit yhä kirjautua että Linux VMs salasanalla. Jos yhteyttä AM eikä julkaista Internet-salasanojen voi riittää. Kuitenkin yhä haluat säilyttää kunnossa salasanakäytännöt ja käytännöistä, kuten salasanan vähimmäispituus ja ne päivitetään säännöllisesti ja hallita salasanojen kunkin Linux AM. SSH avaimen vähentää yksittäisiä tunnistetietojen hallinta useita VMs koko monimutkaisuudesta.


## <a name="windows-packages-and-ssh-clients"></a>Windows-paketit ja SSH asiakkaat

Voit muodostaa yhteyden ja hallita Linux VMs Azure-tietokannassa **ssh** -asiakas. Windows-tietokoneissa ei ole yleensä **ssh** asiakas on asennettu. Yleisten Windowsin SSH asiakkaiden voit asentaa sisältyvät seuraavat paketit:

- [Git For Windowsissa](https://git-for-windows.github.io/)
- [painovärit, muste](http://www.chiark.greenend.org.uk/~sgtatham/putty/)
- [MobaXterm](http://mobaxterm.mobatek.net/)
- [Cygwin](https://cygwin.com/)

> [AZURE.NOTE] Uusimmat Windows 10 vuosipäivä päivitys sisältääkin Bash Windows. Tämän ominaisuuden avulla voit suorittaa Linux ja Accessin apuohjelmia, kuten SSH asiakkaan Windows alijärjestelmää. Bash Windows on kehitteillä ja pidetään beeta-versiossa. Saat lisätietoja Bash Windows [Bash Ubuntu Windows](https://msdn.microsoft.com/commandline/wsl/about).


## <a name="which-key-files-do-you-need-to-create"></a>Mitä avaimen tiedostoja haluat luoda?

Azure edellyttää vähintään 2048-bittinen, **ssh-rsa** Muotoile julkiset ja yksityiset avaimet. Jos hallinnoit Azure resurssien perinteinen käyttöönotto-mallin avulla, sinun on myös luomiseen PEM (`.pem` tiedosto).

Seuraavassa on käyttöönotto-skenaariot ja tiedostotyypit, voit käyttää kaikkien:

1. minkä tahansa käyttöönoton [Azure portal](https://portal.azure.com)ja Resurssienhallinta ominaisuuksissa käyttämällä [Azure CLI](../xplat-cli-install.md)varten tarvittavat **ssh-rsa** -avaimet.
    - Painettavat näppäimet on yleensä kaikki Useimmat käyttäjät tarvitsevat.
2. `.pem`tiedosto on luotava VMs [Classic-portaalissa](https://manage.windowsazure.com). Painettavat näppäimet tuetaan myös Classic-käyttöönotoissa, joissa käytetään [Azure CLI](../xplat-cli-install.md).
    - Tarvitset vain luominen Jos hallinnoit perinteinen käyttöönotto-mallin avulla luodut resurssit nämä muita avaimet ja varmenteet.


## <a name="install-git-for-windows"></a>Git Windows-version asentaminen

Edellisen osion luettelossa useita paketteja, jotka sisältävät `openssl` Windows-työkalu. Tämä työkalu tarvita luominen julkiset ja yksityiset avaimet. Seuraavissa esimerkeissä yksityiskohtaiset tiedot asentaminen ja **Git for Windows**, mutta voit valita sen mukaan, kumpi paketin laite. **Windowsin Git** kautta pääset käsiksi joitakin Avaa lähde-lisäohjelmistoa ([OSS](https://en.wikipedia.org/wiki/Open-source_software)) työkalut ja apuohjelmia, joita voi olla hyötyä, kun käsittelet Linux VMs.

1. Lataa ja asenna **Git Windows** seuraavaan sijaintiin: [https://git-for-windows.github.io/](https://git-for-windows.github.io/).

2. Hyväksy oletusasetusten asennuksen aikana, paitsi jos haluat muuttaa ne erikseen.

3. **Käynnistä-valikko** **Git Bash** pitäminen > **Git** > **Git Bash**. Seuraavassa esimerkissä konsolin seuraavankaltaiselta:

    ![Git for Windowsin Bash käyttöliittymä](./media/virtual-machines-linux-ssh-from-windows/git-bash-window.png)


## <a name="create-a-private-key"></a>Luo yksityinen avain

1. Valitse **Git Bash** -ikkunassa Käytä `openssl.exe` Luo yksityinen avain. Seuraava esimerkki luo avain nimeltä `myPrivateKey` ja -nimisen sertifikaatin `myCert.pem`:

    ```bash
    openssl.exe req -x509 -nodes -days 365 -newkey rsa:2048 \
        -keyout myPrivateKey.key -out myCert.pem
    ```

    Tulos näyttää seuraavankaltaiselta seuraavassa esimerkissä:

    ```bash
    Generating a 2048 bit RSA private key
    .......................................+++
    .......................+++
    writing new private key to 'myPrivateKey.key'
    -----
    You are about to be asked to enter information that will be incorporated
    into your certificate request.
    What you are about to enter is what is called a Distinguished Name or a DN.
    There are quite a few fields but you can leave some blank
    For some fields there will be a default value,
    If you enter '.', the field will be left blank.
    -----
    Country Name (2 letter code) [AU]:
    ```

2. Vastaa näyttöön maan nimi, sijainti, organisaationimi.

3. Nykyinen toimimasta hakemistossa luodaan uusi yksityinen avain ja varmenne. Suojauksen parhaista käytännöistä Määritä käyttöoikeudet-yksityinen avain niin, että vain sinä voit käyttää:

    ```bash
    chmod 0600 myPrivateKey
    ```

4. Jos haluat myös klassinen resurssien, muunna `myCert.pem` , `myCert.cer` (DER encoded X509 varmenteen). Suorita tämä vaihe on valinnainen, vain, jos haluat hallita erityisesti vanhempia perinteinen resursseja. 

    Muunna sertifikaatti seuraava komento:

    ```bash
    openssl.exe  x509 -outform der -in myCert.pem -out myCert.cer
    ```

## <a name="create-a-private-key-for-putty"></a>Luo yksityinen avain painovärit, muste

Painovärit, muste on yhteiset SSH asiakas for Windowsissa. Olet maksuttomia, jonka haluat SSH mikä tahansa asiakas. Käyttämään painovärit, muste haluat luoda uusia tyyppisen avain - painovärit, muste yksityinen avain (PPK). Jos et halua käyttää painovärit, muste, Ohita tämä osa.

Seuraavassa esimerkissä luodaan tämän muut yksityinen avain painovärit, muste käyttämään varten:

1. Yksityinen avain muuntaminen RSA yksityinen avain, PuTTYgen ymmärtämään **Git Bash** avulla. Seuraava esimerkki luo avain nimeltä `myPrivateKey_rsa` nimeltä aiemmin Key `myPrivateKey`:

    ```bash
    openssl rsa -in ./myPrivateKey.key -out myPrivateKey_rsa
    ```

    Suojauksen parhaista käytännöistä Määritä käyttöoikeudet-yksityinen avain niin, että vain sinä voit käyttää:

    ```bash
    chmod 0600 myPrivateKey_rsa
    ```

2. Lataa ja suorita PuTTYgen seuraavaan sijaintiin: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)

3. Valitse-valikosta: **tiedoston** > **kuormituksen yksityinen avain**

4. Etsi yksityinen avain (`myPrivateKey_rsa` edellisessä esimerkissä). Kun käynnistät **Git Bash** oletuskansio on `C:\Users\%username%`. Muuta tiedoston suodatin näyttämään **kaikki tiedostot (\*.\*)**:

    ![Lataa aiemmin luotu yksityinen avain PuTTYgen](./media/virtual-machines-linux-ssh-from-windows/load-private-key.png)

5. Valitse **Avaa**. Kehote ilmaisee, että avain on tuotu onnistuneesti:

    ![PuTTYgen tuominen onnistui-näppäintä](./media/virtual-machines-linux-ssh-from-windows/successfully-imported-key.png)

6. Valitse **OK** ja sulje kehotteen.

7. Julkinen avain näkyy **PuTTYgen** -ikkunan yläreunassa. Kopioi ja Liitä tämä julkisella avaimella Azure portaalin tai Azure Resurssienhallinta malliin, kun luot Linux AM. Voit myös napsauttaa kopion tallentaminen tietokoneeseen **julkisella avaimella tallentaminen** :

    ![Tallenna painovärit, muste julkinen avain-tiedosto](./media/virtual-machines-linux-ssh-from-windows/save-public-key.png)

    Seuraavassa esimerkissä esitetään, miten kopioida ja liittää tämän julkisella avaimella Azure portaalin Linux AM luodessasi. Julkinen avain on yleensä sitten tallennettu `~/.ssh/authorized_keys` käyttöön uuden AM.

    ![Käytä julkisella avaimella, kun haluat luoda AM Azure-portaalissa](./media/virtual-machines-linux-ssh-from-windows/use-public-key-azure-portal.png)

7. Takaisin **PuTTYgen**, valitse **Tallenna yksityinen avain**:

    ![Tallenna tiedosto painovärit, muste yksityinen avain](./media/virtual-machines-linux-ssh-from-windows/save-ppk-file.png)

    > [AZURE.WARNING] Sinulta kysytään, jos haluat jatkaa kirjoittamatta avaintasi salasana. Salasana on liitetty yksityinen avain salasanan tavoin. Vaikka joku on yksityinen avain hankkiminen, ne edelleen voi todentaa vain avaimen avulla. Hänen on myös salasana. Ilman salasana, jos joku hakee yksityinen avain, ne voit kirjata AM tai palvelu, joka käyttää näppäimelle. Suosittelemme, että luot salasana. Jos unohdat salasana, käytettävissä on kuitenkin ei voi palauttaa sitä enää myöhemmin.

    Jos haluat kirjoittaa salasana, valitse **ei**, salasana Kirjoita PuTTYgen pääikkunassa ja valitse sitten **Tallenna yksityinen avain** uudelleen. Muussa tapauksessa jatka valitsemalla **Kyllä** mutta estää valinnainen salasana.

8. Kirjoita nimi ja sijainti PPK-tiedosto tallennetaan.


## <a name="use-putty-to-ssh-to-a-linux-machine"></a>Käytä Putty SSH Linux-tietokoneeseen

Uudelleen painovärit, muste on yhteiset SSH asiakas for Windowsissa. Olet maksuttomia, jonka haluat SSH mikä tahansa asiakas. Seuraavat vaiheet Tietotyylit käyttämisestä oman Azure AM käyttämällä SSH todentamismenetelmä yksityinen avain. Vaiheet ovat samanlaisia muiden SSH avaimen asiakkaiden kannalta hänen nimeään tarvitse lisätä ladata yksityinen avain tarkistamiseen SSH yhteys.

1. Lataa ja suorita painovärit, muste seuraavasta sijainnista: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)

2. Kirjoita isäntänimi tai IP-osoite, että AM Azure-portaalista:

    ![Avaa uusi painovärit, muste-yhteys](./media/virtual-machines-linux-ssh-from-windows/putty-new-connection.png)

3. Ennen kuin valitset **avoinna**, valitse **yhteys** > **SSH** > **Auth** -välilehti. Etsi ja valitse Yksityinen avain:

    ![Valitse painovärit, muste-yksityinen avain todennusta varten](./media/virtual-machines-linux-ssh-from-windows/putty-auth-dialog.png)

4. Valitse **Avaa** muodostaa virtuaalikoneen
 

## <a name="next-steps"></a>Seuraavat vaiheet
Voit myös luoda julkiset ja yksityiset avaimet [OS X- ja Linux avulla](virtual-machines-linux-mac-create-ssh-keys.md).

Saat lisätietoja Bash Windows ja eduista OSS Työkalut helposti saatavilla Windows-tietokoneessa [Ubuntu Windows-Bash](https://msdn.microsoft.com/commandline/wsl/about).

Jos sinulla on vaikeuksia muodostaa yhteyttä Linux VMs SSH avulla, katso [vianmääritys SSH yhteydet Azure-Linux AM](virtual-machines-linux-troubleshoot-ssh-connection.md).
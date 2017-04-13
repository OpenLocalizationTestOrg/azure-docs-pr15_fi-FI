<properties
   pageTitle="Luo ja lataa FreeBSD AM kuva | Microsoft Azure"
   description="Opettele luomaan ja lataa virtual kiintolevyn (Näennäiskiintolevyn), joka sisältää FreeBSD käyttöjärjestelmän Azure virtuaalikoneen luominen"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="KylieLiang"
   manager="timlt"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="08/29/2016"
   ms.author="kyliel"/>

# <a name="create-and-upload-a-freebsd-vhd-to-azure"></a>Luo ja lataa FreeBSD Näennäiskiintolevyn Azure

Tässä artikkelissa kerrotaan, miten voit luoda ja ladata virtual kiintolevyn (Näennäiskiintolevyn), joka sisältää FreeBSD-käyttöjärjestelmä. Kun olet ladannut, voit käyttää oma kuva virtual machine (AM) luominen Azure.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


## <a name="prerequisites"></a>Edellytykset
Tässä artikkelissa oletetaan, että seuraavat kohteet:

- **Azure tilauksen**– Jos ei ole tiliä voit luoda sellaisen vain muutaman minuutin. Jos sinulla on MSDN-tilaus, katso lisätietoja artikkelista [kuukausittain Azure luottotietojen Visual Studio-tilaajille.](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). Muussa tapauksessa Lue luomisesta [ilmainen kokeiluversio tili](https://azure.microsoft.com/pricing/free-trial/).  

- **PowerShellin azure-Työkalut**– PowerShellin Azure moduuli on asennettu ja käyttämään Azure tilauksen. Lataa moduuli, on artikkelissa [Azure Lataa](https://azure.microsoft.com/downloads/). Opetusohjelmassa kuvataan, miten asentaminen ja määrittäminen moduuli on saatavilla täällä. Lataa Näennäiskiintolevyn [Azure-lataukset](https://azure.microsoft.com/downloads/) -cmdlet-komennolla.

- **FreeBSD käyttöjärjestelmiä .vhd tiedostossa**--on tuettu FreeBSD käyttöjärjestelmä on oltava asennettuna virtual kiintolevylle. Useita työkaluja .vhd tiedostojen luomiseen. Voit esimerkiksi .vhd-tiedoston luominen ja asentaa käyttöjärjestelmän virtualisointiratkaisun, kuten Hyper-V avulla. Saat ohjeet ja käyttöön Hyper-V- [asentaa Hyper-V ja luo virtual machine](http://technet.microsoft.com/library/hh846766.aspx).

> [AZURE.NOTE] Azure VHDX uudempaan tiedostomuotoon ei tueta. Voit muuntaa levyn Näennäiskiintolevyn muotoon Hyper-V Manager tai cmdlet-komento, [Muunna näennäiskiintolevyn](https://technet.microsoft.com/library/hh848454.aspx)avulla. Lisäksi on [MSDN-sivuston käyttämisestä FreeBSD Hyper - v opetusohjelma](http://blogs.msdn.com/b/kylie/archive/2014/12/25/running-freebsd-on-hyper-v.aspx).

Tämän tehtävän kuuluu seuraavat viisi vaiheet.

## <a name="step-1-prepare-the-image-for-upload"></a>Vaihe 1: Valmisteleminen kuvan lataaminen

Valitse virtuaalikoneen, johon asensit FreeBSD-käyttöjärjestelmä tekemällä seuraavat toimet:

1. Ota käyttöön DHCP.

        # echo 'ifconfig_hn0="SYNCDHCP"' >> /etc/rc.conf
        # service netif restart

2. Ota käyttöön SSH.

    SSH on käytössä oletusarvoisesti levyltä asennuksen jälkeen. Jos se ei ole otettu käyttöön jostain syystä tai jos käytät FreeBSD Näennäiskiintolevyn suoraan, kirjoita seuraava komento:

        # echo 'sshd_enable="YES"' >> /etc/rc.conf
        # ssh-keygen -t dsa -f /etc/ssh/ssh_host_dsa_key
        # ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key
        # service sshd restart

3. Määritä serial konsolissa.

        # echo 'console="comconsole vidconsole"' >> /boot/loader.conf
        # echo 'comconsole_speed="115200"' >> /boot/loader.conf

4. Asenna sudo.

    Pääkansio-tili on poistettu käytöstä Azure-tietokannassa. Tämä tarkoittaa haluat hyödyntää sudo lataamasta käyttäjän, suorita komennot oikeuksin.

        # pkg install sudo
;
5. Azure agentti edellytyksistä.

        # pkg install python27  
        # pkg install Py27-setuptools27   
        # ln -s /usr/local/bin/python2.7 /usr/bin/python   
        # pkg install git

6. Asenna Azure agentti.

    Azure-agentti uusimman version löytyy aina [github](https://github.com/Azure/WALinuxAgent/releases). Versio 2.0.10 + virallisesti tukee FreeBSD 10 ja 10.1 ja versio 2.1.4 virallisesti tukee FreeBSD 10.2 ja myöhemmissä versioissa.

        # git clone https://github.com/Azure/WALinuxAgent.git  
        # cd WALinuxAgent  
        # git tag  
        …
        WALinuxAgent-2.0.16
        …
        v2.1.4
        v2.1.4.rc0
        v2.1.4.rc1

    2.0-esimerkkinä käytetään 2.0.16:

        # git checkout WALinuxAgent-2.0.16
        # python setup.py install  
        # ln -sf /usr/local/sbin/waagent /usr/sbin/waagent  

    2.1 esimerkkinä käytetään 2.1.4:

        # git checkout v2.1.4
        # python setup.py install  
        # ln -sf /usr/local/sbin/waagent /usr/sbin/waagent  
        # ln -sf /usr/local/sbin/waagent2.0 /usr/sbin/waagent2.0

    >[AZURE.IMPORTANT] Kun olet asentanut Azure-agentti, se on hyvä varmistamaan, että se on käynnissä:

        # waagent -version
        WALinuxAgent-2.1.4 running on freebsd 10.3
        Python: 2.7.11
        # service –e | grep waagent
        /etc/rc.d/waagent
        # cat /var/log/waagent.log

7. Deprovision järjestelmä.

    Deprovision järjestelmän Puhdista ja tekemällä siitä sopii toiminnon uudelleen. Seuraava komento poistaa myös viimeisen valmistellun käyttäjätilin ja liittyvät tiedot:

        # echo "y" |  /usr/local/sbin/waagent -deprovision+user  
        # echo  'waagent_enable="YES"' >> /etc/rc.conf

    Voit nyt Sammuta oman AM.

## <a name="step-2-create-a-storage-account-in-azure"></a>Vaihe 2: Luo Azure-tallennustilan tilin ##

Tarvitset lataamaan .vhd-tiedoston, jotta se voidaan luoda virtual machine Azure-tallennustilan tilin. Voit käyttää Azure perinteinen portaalin tallennustilan tilin luominen.

1. Kirjautuminen [Azure perinteinen portal](https://manage.windowsazure.com).

2. Valitse **Uusi**komentopalkista.

3. Valitse **tietopalvelut** > **tallennustilan** > **nopea luominen**.

    ![Nopea tallennustilan tilin luominen](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/Storage-quick-create.png)

4. Täytä kentät seuraavasti:

    - Kirjoita **URL** -kenttään alitoimialueen nimi-tallennustilan tilin URL-Osoitetta. Tapahtuma voi olla 3 – 24 numeroita ja pieniä kirjaimia. Tämä nimi muuttuu isäntänimi sisällä URL-osoite, jota käytetään Azure-Blob-säiliö, Azure jonon tallennustilan tai Azure-taulukosta tallennustilan resurssit-tilausta.

    - Valitse **Sijainti ja affiniteetti ryhmä** -pudotusvalikosta **sijainnin tai affiniteetti ryhmän** tallennustilan tilin. Affiniteetti-ryhmässä voit sijoittaa pilvipalveluiden ja tallennustilaa saman tietokeskuksen.

    - **Replikoinnin** -kentän valitseminen käytön **Geo tarpeettomat** replikoinnin tallennustilan tilin. GEO replikointi on käytössä oletusarvoisesti. Tämä asetus, kopioi tiedot toissijainen sijainti ilmaiseksi, niin, että tallennustilan epäonnistuu päälle kyseiseen sijaintiin pää virheen ilmetessä ensisijainen sijainnissa. Toissijainen sijainti määritetään automaattisesti, eikä niitä voi muuttaa. Jos tarvitset oman pilvitallennuspalveluita vuoksi lakisääteiset vaatimukset tai organisaation käytäntöä sijainti määrittää tarkemmin, voit poistaa käytöstä geo replikoinnin. Muista kuitenkin, että jos otat myöhemmin geo replikoinnin, voit veloitetaan erikseen tietojen siirto maksu replikointia tiedot toissijainen haluamaasi kohtaan. Alennuksen etsiminen tarjotaan tallennustilan palveluja ilman geo toistoa. Lisätietoja geo-replikoinnin tallennustilan tilien hallinta löytyy tähän: [luominen, hallinta, tai poista tallennustilan-tili](../storage-create-storage-account/#replication-options).

    ![Kirjoita tallennustilan tilin tiedot](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/Storage-create-account.png)


5. Valitse **Luo tallennustilan tili**. Tili näkyy nyt **tallennus**.

    ![Tallennustilan tili on luotu](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/Storagenewaccount.png)

6. Seuraavaksi luodaan säilö .vhd ladatut tiedostot. Valitse tallennustilan tilin nimi ja valitse sitten **säilöt**.

    ![Tallennustilan tilin tiedot](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/storageaccount_detail.png)

7. Valitse **Luo säilön**.

    ![Tallennustilan tilin tiedot](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/storageaccount_container.png)

8. Kirjoita oman säilön nimi **nimi** -kenttään. Valitse sitten **Access** -pudotusvalikosta haluamasi tyypin käyttöoikeuskäytäntö.

    ![Säilön nimi](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/storageaccount_containervalues.png)

    > [AZURE.NOTE] Oletusarvon mukaan säilö on yksityinen, ja niitä voi käyttää vain tilin omistajan. Anna julkisen lukuoikeudet BLOB-säilö, mutta et säilön ominaisuudet ja metatietoja, käyttämällä **Julkisen Blob** -vaihtoehtoa. Anna koko julkisen lukuoikeudet säilö ja BLOB-objektit, asetuksella **Julkinen säilö** .

## <a name="step-3-prepare-the-connection-to-azure"></a>Vaihe 3: Valmisteleminen Azure yhteys

Ennen kuin voit ladata .vhd-tiedoston, sinun täytyy suojatun yhteyden tietokoneen ja Azure tilauksen välillä. Voit käyttää Azure Active Directory (Azure AD) tapaa tai varmenne toiminto.

### <a name="use-the-azure-ad-method-to-upload-a-vhd-file"></a>Azure AD-menetelmän avulla .vhd tiedoston lataaminen

1. Avaa Azure PowerShell console.

2. Kirjoita seuraava komento:  
    `Add-AzureAccount`

    Tämä komento Avaa-kirjautuminen ikkunan kohtaa, johon voit kirjautua käyttämällä työpaikan tai oppilaitoksen tiliä.

    ![PowerShell-ikkuna](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/add_azureaccount.png)

3. Azure todentaa ja tallentaa tunnistetietoja. Valitse sulkee ikkunan.

### <a name="use-the-certificate-method-to-upload-a-vhd-file"></a>Varmenne-menetelmän avulla .vhd tiedoston lataaminen

1. Avaa Azure PowerShell console.

2. Tyyppi:  `Get-AzurePublishSettingsFile`.

3. Selain avautuu ja kysyy, Haluatko ladata .publishsettings tiedoston. Tämä tiedosto on tietoja ja Azure tilauksen varmennetta.

    ![Selain-lataussivulta](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/Browser_download_GetPublishSettingsFile.png)

3. Tallenna tiedosto .publishsettings.

4. Tyyppi:  `Import-AzurePublishSettingsFile <PathToFile>`, jossa `<PathToFile>` on .publishsettings tiedoston koko polku.

   Lisätietoja on artikkelissa [Azure cmdlet-komentojen käytön aloittaminen](http://msdn.microsoft.com/library/windowsazure/jj554332.aspx).

   Saat lisätietoja asennuksesta ja määrityksestä PowerShellin [asentaminen ja määrittäminen PowerShellin Azure](../powershell-install-configure.md).

## <a name="step-4-upload-the-vhd-file"></a>Vaihe 4: Lataa .vhd-tiedosto

Kun lataat .vhd-tiedoston, voit siirtää sen mihin tahansa sisällä Blob-objektien tallennustilaan. Seuraavassa on joitakin ehtoja käytät, kun haluat ladata tiedoston:
-  **BlobStorageURL** on URL-osoite, jonka loit vaiheessa 2 tallennustilan tilin.
-  **YourImagesFolder** on Blob-objektien tallennustilaan säilössä, johon haluat tallentaa kuvat.
- **VHDName** on otsikko, joka tunnistaa virtual kovalevy Azure perinteinen-portaalissa.
- **PathToVHDFile** on koko polku ja .vhd-tiedoston nimi.


Käytetään edellisessä vaiheessa PowerShellin Azure-ikkunasta Kirjoita seuraava komento:

        Add-AzureVhd -Destination "<BlobStorageURL>/<YourImagesFolder>/<VHDName>.vhd" -LocalFilePath <PathToVHDFile>

## <a name="step-5-create-a-vm-with-the-uploaded-vhd-file"></a>Vaihe 5: Luo AM ladatut .vhd-tiedosto
Kun olet ladannut .vhd-tiedoston, voit lisätä sen kuvana mukautetun kuvia, jotka liittyvät tilauksen ja luo mukautettuja kuvalla virtual machine-luetteloon.

1. Käytetään edellisessä vaiheessa PowerShellin Azure-ikkunasta Kirjoita seuraava komento:

        Add-AzureVMImage -ImageName <Your Image's Name> -MediaLocation <location of the VHD> -OS <Type of the OS on the VHD>

    > [AZURE.NOTE]Määritä Linux OS tyyppi. PowerShellin Azure nykyversio hyväksyy vain "Linux" tai "Windows" parametrina.

2. Kun suoritat edellä kuvatut vaiheet, uusi kuva näkyy, kun valitset Azure perinteinen portaalissa **kuvat** -välilehti.  

    ![Valitse kuva](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/addfreebsdimage.png)

3. Luo virtual machine valikoimasta. Tämä uusi kuva on nyt **Omat kuvat**.
4. Valitse uusi kuva. Seuraavaksi kerrotaan pyytää määrittämään isäntänimi, salasana, SSH-näppäintä ja niin edelleen.

    ![Mukautettu kuva](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/createfreebsdimageinazure.png)

4. Kun olet suorittanut valmistelun, näet oman FreeBSD AM Azure käynnissä.

    ![Azure FreeBSD kuva](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/freebsdimageinazure.png)

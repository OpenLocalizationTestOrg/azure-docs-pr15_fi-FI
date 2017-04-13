<properties
    pageTitle="Valitse Linux AM Apache Tomcat määrittäminen | Microsoft Azure"
    description="Opi määrittämään Apache Tomcat7 käyttämällä Azure virtual machine (AM) Linux käynnissä."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="NingKuang"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/15/2015"
    ms.author="ningk"/>

#<a name="how-to-set-up-tomcat7-on-a-linux-virtual-machine-with-microsoft-azure"></a>Voit määrittää Tomcat7 Linux Virtual tietokoneessa, jossa Microsoft Azure

Apache Tomcat (tai yksinkertaisesti Tomcat, aiemmin myös Hanoi Tomcat) on Avaa lähde verkkopalvelin ja servlet säilö kehittänyt Apache ohjelmiston Foundation (ASF) mukaan. Tomcat toteuttaa Java-Servlet ja -su Microsystems JavaServer sivujen (JSP)-määritykset ja jossa Java suorittamisen täysin Java HTTP-web server-ympäristössä. Yksinkertaisin määrityksessä Tomcat suoritetaan yksi käyttöjärjestelmä prosessi. Tämä prosessi suoritetaan Java virtual machine (JVM). Jokaisen HTTP pyyntö selaimesta Tomcat käsitellään eri viestiketjun Tomcat prosessin nimellä.  

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Tässä oppaassa asennetaan tomcat7 Linux-kuva ja ottaa käyttöön Microsoft Azure.  

Opit seuraavat asiat:  

-   Miten voit luoda virtual machine Azure-tietokannassa.
-   Miten virtuaalikoneen valmisteleminen tomcat7.
-   Voit asentaa tomcat7.

Oletetaan, että käyttäjälle on jo Azure tilauksen.  Jos et voi rekisteröityä maksuttoman kokeiluversion osoitteessa [http://azure.microsoft.com](https://azure.microsoft.com/). Jos sinulla on MSDN-tilaus, katso lisätietoja artikkelista [Microsoft Azure määräten hinnat: MSDN MPN ja Bizspark edut](https://azure.microsoft.com/pricing/member-offers/msdn-benefits/?c=14-39). Lisätietoja Azure-kohdassa [Azure ominaisuudet?](https://azure.microsoft.com/overview/what-is-azure/).

Tässä artikkelissa oletetaan, että basic kokemusta tomcat ja Linux.  

##<a name="phase-1-create-an-image"></a>Vaihe 1: Luo kuva
Tässä vaiheessa luodaan käyttämällä Linux kuva Azure virtual kone.  

###<a name="step-1-generate-an-ssh-authentication-key"></a>Vaihe 1: Luo SSH-todennuksen avain
SSH on tärkeä apuväline järjestelmänvalvojille. Määrittäminen perustuu ihmisten määritetään salasanan access-suojauksen ei kuitenkaan parhaiden käytäntöjen mukaisesti. Käyttäjät voivat katkaista järjestelmään käyttäjänimen ja salasanan heikoilla perusteella.

Hyviä uutisia on se, että voi jättää etäkäyttöpalvelimen auki ja ei tarvitse huolehtia salasanat. Menetelmä muodostuu julkiseen salaus todentaminen. Käyttäjän yksityinen avain on se, joka antaa todennusta. Voit lukita käyttäjätiliä estääksesi salasanan todennusta kokonaan.

Tätä menetelmää toisen etuna on se, että et tarvitse eri salasanat kirjautua sisään eri palvelimiin. Voi todentaa käyttämällä Omat yksityinen avain kaikissa palvelimissa, joka estää tarvitse muistaa useita salasanoja.

On myös mahdollista kirjautua vaatii salasanan tällä menetelmällä.

Luo SSH todennus-avain seuraavasti.

1.  Lataa ja asenna puttygen seuraavaan sijaintiin: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)
2.  Suorita PUTTYGEN. KOMENTORIVIVALITSIMET.
3.  Valitse **Luo** näppäimet luomiseen. Prosessin voit suurentaa poistaa satunnaisuuden siirtämällä hiiren osoitin ikkunan tyhjää aluetta.  
![][1]
4.  Luo prosessin jälkeen Puttygen.exe näkyy luotu avain. Esimerkki:  
![][2]
5.  Valitse ja kopioi julkinen avain- **näppäintä** ja tallenna se publicKey.pem-tiedostossa. Älä napsauta **Tallenna julkisella avaimella**, koska tallennetun julkisella avaimella tiedostomuoto on erilainen kuin haluamme julkinen avain.
6.  Valitse **Tallenna yksityinen avain** ja tallenna se privateKey.ppk-tiedostossa.

###<a name="step-2-create-the-image-in-the-azure-portal"></a>Vaihe 2: Luo kuvan Azure-portaalissa.
[Azure-portaali](https://portal.azure.com/)valitsemalla **Uusi** luominen kuvan valitseminen käyttötarkoitukseen Linux kuva tehtäväpalkissa. Seuraavassa esimerkissä Ubuntu 14.04 kuva.
![][3]

Jos **Isäntänimi** Määritä URL-osoite, jota olet ja Internet-asiakkaat käyttää käyttää virtual tämän tietokoneen nimi. Määritä DNS-nimi, esimerkiksi tomcatdemo loppuosa ja Azure Luo kuin tomcatdemo.cloudapp.net URL-osoite.  

**SSH todennus-näppäintä**kopioi avainarvon **publicKey.pem** -tiedosto, joka sisältää puttygen luoma julkinen avain.  
![][4]

Määrittää muita asetuksia haluamallasi tavalla ja valitse sitten Luo.  

##<a name="phase-2-prepare-your-virtual-machine-for-tomcat7"></a>Vaihe 2: Valmisteleminen virtuaalikoneen Tomcat7
Tässä vaiheessa päätepisteen tomcat tietoliikenteen määrittäminen ja liitä uuden virtuaalikoneen.
###<a name="step-1-open-the-http-port-to-allow-web-access"></a>Vaihe 1: Avaa web käyttää HTTP-portti
Azure päätepisteet koostuvat protocol (TCP tai UDP) sekä julkisista ja yksityisistä portin. Yksityinen portti on, palvelu on Kuuntele virtuaalikoneen portti. Julkinen portti on käytettävä portti Azure pilvipalvelussa Kuuntele ulkoisesti tulevaa, Internet-liikenteen.  

TCP-portin 8080 on oletusarvoinen porttinumero, mitkä tomcat seuraa. Avata portin Azure päätepisteen Salli sinun ja muiden asiakkaiden Internetiä tomcat sivuja.  

1.  Azure-portaalissa, valitse **Selaa** -> **virtuaalikoneen**, ja valitse sitten virtuaalikoneen, jonka loit.  
![][5]
2.  Jos haluat lisätä päätepisteen virtuaalikoneen, valitse **päätepisteet** -ruutuun.
![][6]
3.  Valitse **Lisää**.  
    1.  **Päätepisteen**päätepisteen nimi Kirjoita päätepisteen ja kirjoita **Julkisen portti**80.  

        Jos määrität sen 80, ei tarvitse lisätä porttinumero URL-osoite, jonka avulla voit käyttää tomcat. Esimerkiksi http://tomcatdemo.cloudapp.net.    

        Jos määrität muu arvo, kuten 81, sinun on lisättävä porttinumero URL-osoite tomcat käyttämiseen. Esimerkiksi http://tomcatdemo.cloudapp.net:81 /.
    2.  Kirjoita 8080 yksityinen-porttiin. Oletusarvon mukaan tomcat seuraa porttinumeroa 8080. Jos olet muuttanut oletusarvo kuunnella tomcat portti, sinun on päivitettävä yksityinen Port (portti) on sama kuin tomcat odottava portti.  
    ![][7]

4.  Valitse lisättävä päätepisteelle virtuaalikoneen **OK** .



###<a name="step-2-connect-to-the-image-you-created"></a>Vaihe 2: Yhteyden muodostaminen kuva, jonka loit.
Voit muodostaa yhteyden virtuaalikoneen jokin SSH-työkalu. Tässä esimerkissä Käytämme painovärit, muste.  

Hae DNS-nimeä virtuaalikoneen ensin Azure-portaalista. **Valitse Selaa** -> **näennäiskoneiden** -> virtuaalikoneen nimi **ominaisuuksien**ja Etsi **Ominaisuudet** -ruutu, **Toimialuenimi** -kenttään.  

Pyydä porttinumero SSH yhteyksien **SSH** -kentästä. Tässä on esimerkki.  
![][8]

Lataa painovärit, muste [täältä](http://www.putty.org/) .  

Kun olet ladataan, valitse suoritettava tiedosto painovärit, muste. KOMENTORIVIVALITSIMET. Perustiedot asetusten kanssa isäntänimi ja porttinumero saatu virtuaalikoneen ominaisuudet. Tässä on esimerkki:  
![][9]

Valitse vasemmanpuoleisessa ruudussa **yhteyden** -> **SSH** -> **todennus** ja valitse sitten **Selaa** **privateKey.ppk** -tiedosto, jossa on yksityinen avain paikan määrittäminen luoma puttygen vaihe 1: Luo kuva. Tässä on esimerkki:  
![][10]

Valitse **Avaa**. Olet ehkä ilmoitettavan viesti-ruutuun. Jos olet määrittänyt DNS-nimen ja porttinumero oikein, valitse **Kyllä**.
![][11]  


Katso seuraavasti:  
![][12]

Käyttäjänimi, kun loit virtuaalikoneen vaihe 1: Luo kuva. Näet seuraavanlaiselta:  
![][13]





##<a name="phase-3-install-software"></a>Vaihe 3: Ohjelmiston asentaminen
Tässä vaiheessa sinun on asennettava suorituksenaikainen Java-ympäristö, tomcat ja tomcat muita osia.  

###<a name="java-runtime-environment"></a>Suorituksenaikainen Java-ympäristö
Java kirjoitetaan tomcat. On kahdenlaisia Java Development Kit (JDKs) (OpenJDK ja Oracle JDK) ja valita sitten haluamasi vaihtoehto.  

>AZURE. Huomautus: Sekä JDKs on lähes samalla koodi Luokat Java-Ohjelmointirajapinta, mutta virtuaalikoneen koodi on erilaisella. Jos kirjastoihin, OpenJDK yleensä käyttäminen Avaa kirjastojen Oracle yleensä käyttämään suljettu niistä. Oracle JDK on useita luokkia ja jotkin kiinteitä virheet ja Oracle JDK säilyy enemmän kuin OpenJDK.

Seuraavat komennot Lataa eri JDKs.  

Avaa jdk   

    sudo apt-get update  
    sudo apt-get install openjdk-7-jre  

Oracle-jdk  

-   Voit ladata JDK Oracle-sivustosta seuraavasti:  

        wget --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u5-b13/jdk-8u5-linux-x64.tar.gz  

-   Luo kansio, joka sisältää JDK tiedostot seuraavasti:  

        sudo mkdir /usr/lib/jvm  

-   Voit purkaa JDK tiedostot-kansioon/usr/lib/jvm /:  

        sudo tar -zxf jdk-8u5-linux-x64.tar.gz  -C /usr/lib/jvm/  

-   Voit määrittää Oracle JDK oletukseksi JVM seuraavasti:  

        sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk1.8.0_05/bin/java 100  
        sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk1.8.0_05/bin/javac 100  

####<a name="test"></a>Testi:
Voit käyttää seuraavaa komentoa Testaa Jos suorituksenaikainen Java-ympäristö on asennettu oikein:  

    java -version  

Jos olet asentanut Avaa jdk, pitäisi tulla seuraava sanoma:![][14]

Jos olet asentanut oracle-jdk, pitäisi tulla seuraava sanoma:![][15]

###<a name="tomcat7"></a>Tomcat7
Seuraava komento avulla voit asentaa tomcat7:  

    sudo apt-get install tomcat7  

Jos et käytä tomcat7, käyttää tätä komentoa tarvittavat muunnelmaa.  

####<a name="test"></a>Testi:

Voit tarkistaa, jos tomcat7 on asennettu, siirry DNS-tomcat-palvelimen nimen (http://tomcatexample.cloudapp.net/ on tämän artikkelin Esimerkki URL-osoite). Jos näet seuraavalta sivulta, sinulla on asennettu tomcat7 oikein.
![][16]


###<a name="install-other-tomcat-components"></a>Asenna Tomcat muut osat
On muita valinnaisia tomcat osia, voit myös asentaa.  

Jos haluat nähdä kaikki käytettävissä olevat osat **sudo piharakennus välimuistin haun tomcat7** komennolla. Seuraavat komennot ovat esimerkkejä asentaminen hyödyllisiä joitakin osia.  

    sudo apt-get install tomcat7-admin      #admin web applications
    sudo apt-get install tomcat7-user         #tools to create user instances  

##<a name="phase-4-configure-tomcat"></a>Vaihe 4: Määritä Tomcat
Tässä vaiheessa voit hallita tomcat.
###<a name="start-and-stop-tomcat7"></a>Aloittaa ja lopettaa tomcat7
Tomcat7 palvelimen käynnistyy automaattisesti, kun asennat sen. Voit myös aloittaa sen itse seuraavalla komennolla:   

    sudo /etc/init.d/tomcat7 start

Jos haluat lopettaa tomcat7:  

    sudo /etc/init.d/tomcat7 stop

Voit tarkastella tomcat7 tilan seuraavasti:  

    sudo /etc/init.d/tomcat7 status

Jos haluat aloittaa tomcat palvelut:  

    sudo /etc/init.d/tomcat7 restart

###<a name="tomcat-administration"></a>Tomcat hallinta
Voit muokata Tomcat käyttäjän kokoonpanotiedosto määrittäminen järjestelmänvalvojan tunnistetietoja seuraavalla komennolla:  

    sudo vi  /etc/tomcat7/tomcat-users.xml   

Tässä on esimerkki:  
![][17]  

>AZURE. Huomautus: Luo järjestelmänvalvojan käyttäjänimeä vahva salasana.  

Tämän tiedoston muokattuasi Käynnistä tomcat7 services seuraavalla komennolla voit varmistaa, että muutokset tulevat voimaan uudelleen:  

    sudo /etc/init.d/tomcat7 restart  

Avaa selain ja kirjoita URL-osoite **http://<your tomcat server DNS name>/manager/html**. Tässä artikkelissa esimerkiksi URL-osoite on http://tomcatexample.cloudapp.net/manager/html.  

Kun yhteys on muodostettu, pitäisi näkyä jotain seuraavankaltaiselta:  
![][18]

##<a name="common-issues"></a>Yleisiä ongelmia

###<a name="cant-access-the-virtual-machine-with-tomcat-and-moodle-from-the-internet"></a>Eikö virtuaalikoneen Tomcat ja Moodle Internetistä

-   **Oire**  
Tomcat on käynnissä, mutta et näe Tomcat sivun selaimen kanssa.
-   **Mahdollisista kirjainkoko**   
    1.  Tomcat odottavan portin ei ole sama kuin virtuaalikoneen 's päätepisteen tomcat tietoliikenteen yksityinen-porttiin.  

        Tarkista portti julkisen ja yksityisen portti päätepisteasetukset ja varmista, että yksityinen portti on sama kuin tomcat odottava portti. Katso vaihe 1: Luo kuva ohjeet virtuaalikoneen päätepisteet määrittämisestä.  

        Voit määrittää tomcat odottavan portin, Avaa /etc/httpd/conf/httpd.conf (punainen on julkaistu versio) tai /etc/tomcat7/server.xml (Debian julkaistu versio). Tomcat odottavan portin on oletusarvoisesti 8080. Tässä on esimerkki:  

            <Connector port="8080" protocol="HTTP/1.1"  connectionTimeout="20000"  URIEncoding="UTF-8"            redirectPort="8443" />  

        Jos käytössäsi on virtual machine, kuten Debian tai Ubuntu ja haluat muuttaa oletusarvoinen portti, Tomcat kuunnella (esimerkiksi 8081), kannattaa myös avata portin käyttöjärjestelmän varten. Avaa profiili:  

            sudo vi /etc/default/tomcat7  

        Valitse viimeisen rivin kommentointi ja muuta "ei", "Kyllä".  

            AUTHBIND=yes

    2.  Palomuurin on poistettu käytöstä tomcat odottavan-portin.

        Jos näet vain Tomcat oletussivun paikallisen isännältä, ongelma on todennäköisesti portti, joka on kuunteli mukaan tomcat on estänyt palomuurin. Voit selata verkkosivun w3m-työkalun avulla. Seuraavat komennot Asenna w3m ja siirry Tomcat oletusarvo-sivulle:  

            sudo yum  install w3m w3m-img
            w3m http://localhost:8080  

-   **Ratkaisu**
    1. Jos tomcat kuunnella portti ei ole sama kuin tietoliikenteen virtuaalikoneen päätepisteen yksityinen-portin, on sama kuin tomcat odottava portti yksityinen-portin on vaihtaminen.   

    2.  Jos ongelman syynä on palomuuri/iptables, Lisää seuraavat rivit /etc/sysconfig/iptables:  

            -A INPUT -p tcp -m tcp --dport 80 -j ACCEPT
            -A INPUT -p tcp -m tcp --dport 443 -j ACCEPT  

        Huomaa, että toiselta riviltä tarvitaan vain https-liikenne.  

        Varmista, että tämä on edellä kaikki rivit, jotka yleisesti rajoittaa käyttöä, esimerkiksi seuraavat:  

            -A INPUT -j REJECT --reject-with icmp-host-prohibited  

        Lataa iptables, suorita seuraava komento:  

            service iptables restart  

        Tämä on testattu CentOS 6.3.

###<a name="permission-denied-when-upload-you-project-files-to-varlibtomcat7webapps"></a>Käyttöoikeuksien estetty milloin projektin /var/lib/tomcat7/webapps tiedostojen lataaminen /  

-   **Oire**  
Jos käytät (kuten FileZilla) SFTP mikä tahansa asiakas muodostaa virtuaalikoneen ja siirry /var/lib/tomcat7/webapps/sivuston, näyttöön tulee virhesanoma seuraavankaltaiselta:  

        status: Listing directory /var/lib/tomcat7/webapps
        Command:    put "C:\Users\liang\Desktop\info.jsp" "info.jsp"
        Error:  /var/lib/tomcat7/webapps/info.jsp: open for write: permission denied
        Error:  File transfer failed

-   **Mahdollisista kirjainkoko** Sinulla ei ole oikeutta käyttää /var/lib/tomcat7/webapps-kansio.  
-   **Ratkaisu**  
Käyttöoikeuden saaminen on pääkansio-tililtä. Voit muuttaa kansion omistajan pääkansion käytit, kun valmistelu koneen käyttäjänimi. Tässä on esimerkki azureuser tilin nimen kanssa:  

        sudo chown azureuser -R /var/lib/tomcat7/webapps

    -R-vaihtoehdon avulla voit käyttää kaikkien tiedostojen sisältyy kansion käyttöoikeuksia liian.  

    Huomaa, että tämä komento toimii myös kansioita. -R-vaihtoehto muuttaa kaikki tiedostot ja kansiot sisältyy kansion käyttöoikeuksia. Tässä on esimerkki:  

        sudo chown -R username:group directory  

    Tämä komento muuttuu kaikki tiedostot ja kansiot sisältyy hakemistosta ja kansion itsensä omistajuus (käyttäjän ja ryhmän).  

    Seuraava komento on vain muuttuu kansion kansion käyttöoikeuksia, mutta jättää tiedostoja ja kansioita hakemiston yksinään.  

        sudo chown username:group directory



[1]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-01.png
[2]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-02.png
[3]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-03.png
[4]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-04.png
[5]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-05.png
[6]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-06.png
[7]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-07.png
[8]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-08.png
[9]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-09.png
[10]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-10.png
[11]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-11.png
[12]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-12.png
[13]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-13.png
[14]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-14.png
[15]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-15.png
[16]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-16.png
[17]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-17.png
[18]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-18.png

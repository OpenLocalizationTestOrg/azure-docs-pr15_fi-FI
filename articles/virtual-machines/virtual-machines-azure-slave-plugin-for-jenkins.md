<properties
    pageTitle="Laajennuksen Azure toissijaisen käyttäminen Jenkins jatkuva integrointi | Microsoft Azure"
    description="Tässä artikkelissa käsitellään laajennuksen Azure toissijaisen käyttäminen Jenkins jatkuva integrointi."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="rmcmurray"
    manager="wpickett"
    editor="" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="java"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="robmcm"/>

# <a name="how-to-use-the-azure-slave-plug-in-with-jenkins-continuous-integration"></a>Opi käyttämään laajennuksen Azure toissijaisen Jenkins jatkuva integrointi

Voit tehdä laajennuksen Azure toissijaisen varten Jenkins Azure säännöstä toissijaisen solmujen kun käynnissä hajautettu muodostaa.

## <a name="install-the-azure-slave-plug-in"></a>Asenna laajennus Azure toissijaisen

1. Valitse **Hallitse Jenkins**Jenkins Raporttinäkymät-ikkunan.

1. Valitse **Hallitse laajennukset** **Jenkins hallinta** -sivulla.

1. Valitse **käytettävissä olevat** -välilehti.

1. Kirjoita Suodatin-kenttään käytettävissä laajennukset luettelon yläpuolella **Azure** rajoittaminen asiaa laajennukset luettelon.

    Jos olet käytettävissä laajennukset luettelossa, löydät Azure toissijaisen laajennuksen **hallinta ja luoda jaettu** -kohdassa.

1. Valitse **Azure toissijaisen laajennuksen** -valintaruutu.

1. Valitse **asennusta ilman uudelleen** tai **Lataa ja asenna uudelleenkäynnistyksen jälkeen**.

Nyt kun laajennus on asennettu, seuraavat vaiheet ovat määrittää laajennuksen Azure tilauksen profiilin ja toissijaisen solmun virtuaalikoneen luominen mallin, jota käytetään luominen.


## <a name="configure-the-azure-slave-plug-in-with-your-subscription-profile"></a>Tilauksen profiilisi ja laajennuksen Azure toissijaisen määrittäminen

Tilauksen profiili, jota kutsutaan myös kuin Julkaisuasetukset, on XML-tiedosto, joka sisältää suojatun tunnistetiedot tiettyjä kehittäminen ympäristön Azure-käyttöä varten tarvitset lisätietoja. Voit määrittää laajennuksen Azure toissijaisen, sinun on:

* Tilauksen tunnus
* Tilauksen varmenteen hallinta

Nämä löytyy [tilauksen profiilin]. Alla on esimerkki tilauksen profiili.

    <?xml version="1.0" encoding="utf-8"?>

        <PublishData>

        <PublishProfile SchemaVersion="2.0" PublishMethod="AzureServiceManagementAPI">

        <Subscription

            ServiceManagementUrl="https://management.core.windows.net"

            Id="<Subscription ID value>"

            Name="Pay-As-You-Go"
            ManagementCertificate="<Management certificate value>" />

        </PublishProfile>

    </PublishData>

Kun olet tilauksen profiilin tai määrittää laajennuksen Azure toissijaisen seuraavasti:

1. Valitse **Hallitse Jenkins**Jenkins Raporttinäkymät-ikkunan.

1. Valitse **määrittää järjestelmän**.

1. Vieritä sivua, jotta löydät **Cloud** -osassa.

1. Valitse **Lisää uusi cloud > Microsoft Azure**.

    ![cloud-osassa][cloud section]

    Tämä näyttää kentät kohtaa, johon haluat lisätä tilauksen tiedot.

    ![tilauksen määritys][subscription configuration]

1. Kopioi tilauksen tunnus ja hallinnan varmenteen arvot tilauksen profiilin ja liitä ne vastaavat kentät.

    Tilauksen tunnus ja hallinta-varmenteen kopioitaessa Älä sisällytä tarjoukset, jotka kehystä arvot.

1. Valitse **Tarkista määritykset**.

1. Kun määritykset on vahvistettu on oikein, valitse **Tallenna**.

## <a name="set-up-a-virtual-machine-template-for-the-azure-slave-plug-in"></a>Määritetään virtuaalikoneen mallin Azure toissijaisen laajennus

Virtuaalikoneen mallin määrittää parametrit, laajennus avulla voit luoda toissijaisen solmu Azure. Seuraavissa vaiheissa Luo Ubuntu-virtuaalikoneen mallin.

1. Valitse **Hallitse Jenkins**Jenkins Raporttinäkymät-ikkunan.

1. Valitse **määrittää järjestelmän**.

1. Vieritä sivua, jotta löydät **Cloud** -osassa.

1. **Cloud** -kohdassa Etsi **Lisää Azure virtuaalikoneen malli**ja valitse sitten **Lisää**.

    ![Lisää AM malli][add vm template]

    Tämä näyttää kentät, johon kirjoitat luotavan mallin tietoja.

    ![tyhjä Yleiset määritys][blank general configuration]

1. Kirjoita **nimi** -ruutuun Azure cloud-palvelunimi. Jos kirjoittamasi nimi viittaa aiemmin pilvipalvelussa, kyseisen palvelun valmisteltu virtuaalikoneen. Muussa tapauksessa Azure Luo uusi tunnus.

1. Kirjoita **kuvaus** -ruutuun teksti, joka kuvaa luotavan mallin. Tämä koskee vain tietueet ja valmistelu virtual machine ei käytetä.

1. **Otsikot** -ruudussa mallia, jota olet luomassa tunnistetaan ja käytetään myöhemmin viittaavat mallia, kun Jenkins projektin luominen. Anna meidän tarkoitus **linux** tähän ruutuun.

1. Valitse **alue** -luettelosta alue, johon virtuaalikoneen luodaan.

1. Valitse **Virtuaalikoneen koko** -luettelosta haluamasi koko.

1. Määritä tallennustilan-tili, johon virtuaalikoneen luodaan **Tallennustilan tilin nimi** -ruutuun. Varmista, että se on sama alue kuin käytetään pilvipalvelussa. Jos haluat luoda uuden tallennustilan, voit jättää tämän valintaruudun tyhjäksi.

1. Säilytys aika määrittää minuuttia, ennen kuin Jenkins poistaa käyttämättömänä toissijaisen määrän. Jätä tämä 60 oletusarvo. Voit valita myös sulkeutumaan toissijaisen sijaan poistamista, kun sitä ei käytetä. Saadakseen ne, valitse **Vain Sammuta (Älä poista) jälkeen säilytys aika** -valintaruutu.

1. Valitse **käyttö** -luettelosta haluamasi ehto, kun toissijaisen solmu käytetään. Valitse **Utilize mahdollisimman paljon solmu**nyt.

    Tässä vaiheessa lomakkeen pitäisi näyttää seuraavanlaiselta hieman:

    ![tarkistuspiste yleisen mallin config][checkpoint general template config]

    Seuraava vaihe on tarkkoja tietoja, jotka haluat luoda toissijaisesta käyttöjärjestelmän kuva.

1. **Kuva perhe tai tunnus** -ruutuun on määritettävä, mitä järjestelmän kuva asennetaan virtuaalikoneen. Voit kuva perheille luettelosta tai määrittää mukautetun kuvan.

    Jos haluat valita kuvan perheille luettelosta, kirjoita kuvan Sukunimi ensimmäisen merkin (kirjainkoko on merkitsevä). Esimerkiksi kirjoittamalla **U** Avaa Ubuntu palvelimen perheille luettelo. Kun olet valinnut luettelosta, Jenkins käyttää kyseisen perheen järjestelmän, että kuva uusimman version, kun valmistelu virtuaalikoneen.

    ![OS kuva luettelon malli][OS Image list sample]

    Jos sinulla on mukautettu kuva, jota haluat käyttää sen sijaan, kirjoita nimi mukautetun kuvan. Mukautetun kuvan nimet näkyvät luettelona, ei, joten varmista, että nimi on kirjoitettu oikein.

    Tässä opetusohjelmassa, kirjoita **U** ja tuo näkyviin Ubuntu kuvien luettelo ja valitse sitten **Ubuntu palvelimen 14.04 l.**.

1. Valitse **Käynnistä menetelmä** -luettelosta **SSH**.

1. Alla olevaa komentosarjaa kopioida ja liittää sen **Alusta komentosarja** -ruutuun.

        # Install Java

        sudo apt-get -y update

        sudo apt-get install -y openjdk-7-jdk

        sudo apt-get -y update --fix-missing

        sudo apt-get install -y openjdk-7-jdk

        # Install git

        sudo apt-get install -y git

        #Install ant

        sudo apt-get install -y ant

        sudo apt-get -y update --fix-missing

        sudo apt-get install -y ant

    Alusta komentosarja suoritetaan virtuaalikoneen luomisen jälkeen. Tässä esimerkissä komentosarja asentaa Java Git ja ant.

1. Kirjoita **käyttäjänimi** ja **salasana** -ruutuihin haluamasi arvot, jotka luodaan virtuaalikoneen järjestelmänvalvojatilin.

1. Valitse **Tarkista mallin** ja tarkistaa, jos määrittämäsi parametrit on voimassa.

1. Valitse **Tallenna**.


## <a name="create-a-jenkins-job-that-runs-on-a-slave-node-on-azure"></a>Luo Jenkins työ, joka suoritetaan Azure toissijaisen solmun

Tässä osiossa luot Jenkins tehtävän, joka suoritetaan Azure toissijaisen solmun. Sinun on oltava oman projektisi, jos haluat seurata GitHub.

1. Jenkins raporttinäkymät-ikkunassa Valitse **Uusi kohde**.

1. Kirjoita luotavan tehtävän nimi.

1. Projektin tyyppi-kohdasta **Freestyle projekti**.

1. Valitse **Ok**.

1. Valitse **Rajoita, jossa projektin suorittaa**tehtävän määritys-sivulla.

1. Kirjoita **Otsikko-lauseketta** -ruudussa **linux**. Edellisessä osassa on luonut toissijaisen, että olemme **linux**, eli mitä määritämme alkamispäivämäärän tähän.

1. **Muodosta** -osassa valitsemalla **Lisää Muodosta vaihe** ja valitse **Suorita shell**.

1. Muokkaa seuraavaa komentosarjaa, **(GitHub tilin nimen)**, **(projektinimi)**ja **(projektin hakemiston)** tilalle haluamasi arvot, ja liitä muokattu komentosarja, joka näkyy tekstialueelle.

        # Clone from git repo

        currentDir="$PWD"

        if [ -e (your project directory) ]; then

            cd (your project directory)

            git pull origin master

        else

            git clone https://github.com/(your GitHub account name)/(your project name).git

        fi

        # change directory to project

        cd $currentDir/(your project directory)

        #Execute build task

        ant

1. Valitse **Tallenna**.

1. Jenkins raporttinäkymät-ikkunassa juuri luomasi tehtävän päälle ja napsauttamalla avattavan luettelon nuolta tehtävän näyttöasetukset.

1. Valitse **Muodosta nyt**.

Jenkins sitten luominen toissijaisen solmu luotu edellisessä osassa mallin ja suorita muodosta tähän vaiheessa määrittämäsi komentosarja.

## <a name="next-steps"></a>Seuraavat vaiheet

Saat lisätietoja Azure käyttäminen Java [Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[tilauksen profiili]: http://go.microsoft.com/fwlink/?LinkID=396395

<!-- IMG List -->

[cloud section]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-cloud-section.png
[subscription configuration]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-account-configuration-fields.png
[add vm template]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-add-vm-template.png
[blank general configuration]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-slave-template-general-configuration-blank.png
[checkpoint general template config]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-slave-template-general-configuration.png
[OS Image list sample]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-os-family-list-sample.png
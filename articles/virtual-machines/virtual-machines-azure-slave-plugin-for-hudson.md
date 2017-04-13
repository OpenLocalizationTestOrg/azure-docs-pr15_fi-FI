<properties
    pageTitle="Laajennuksen Azure toissijaisen käyttäminen Hudson jatkuva integrointi | Microsoft Azure"
    description="Tässä artikkelissa käsitellään laajennuksen Azure toissijaisen käyttäminen Hudson jatkuva integrointi."
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

# <a name="how-to-use-the-azure-slave-plug-in-with-hudson-continuous-integration"></a>Opi käyttämään laajennuksen Azure toissijaisen Hudson jatkuva integrointi

Laajennus Hudson Azure toissijaisen avulla voit valmistella toissijaisen solmujen Azure, kun käynnissä hajautettu muodostaa.

## <a name="install-the-azure-slave-plug-in"></a>Asenna laajennus Azure toissijaisen

1. Valitse **Hallitse Hudson**Hudson Raporttinäkymät-ikkunan.

1. Valitse **Hallitse laajennukset** **Hudson hallinta** -sivulla.

1. Valitse **käytettävissä olevat** -välilehti.

1. Valitse **Hae** ja kirjoita **Azure** rajoittaminen asiaa laajennukset luettelon.

    Jos olet käytettävissä laajennukset luettelossa, löydät Azure toissijaisen laajennuksen **hallinta ja luoda jaettu** -kohdassa **Muut** -välilehdessä.

1. Valitse valintaruutu, jos **Azure toissijaisen**laajennuksen.

1. Valitse **Asenna**.

1. Käynnistä Hudson.

Nyt kun laajennus on asennettu, seuraavat vaiheet olisi, voit määrittää laajennuksen Azure tilauksen profiilin ja luo mallin, jota käytetään luominen toissijaisen solmun AM.

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

            Id="<Subscription ID>"

            Name="Pay-As-You-Go"
            ManagementCertificate="<Management certificate value>" />

        </PublishProfile>

    </PublishData>

Kun tilaus profiilin, määritä laajennuksen Azure toissijaisen seuraavasti.

1. Valitse **Hallitse Hudson**Hudson Raporttinäkymät-ikkunan.

1. Valitse **määrittää järjestelmän**.

1. Vieritä sivua, jotta löydät **Cloud** -osassa.

1. Valitse **Lisää uusi cloud > Microsoft Azure**.

    ![Lisää uusi pilveen][add new cloud]

    Tämä näyttää kentät kohtaa, johon haluat lisätä tilauksen tiedot.

    ![Määritä profiili][configure profile]

1. Kopioi tilauksen tunnus ja hallinta-varmenteen tilauksen profiilin ja liitä ne vastaaviin kenttiin.

    Tilauksen tunnus ja hallinta-varmenteen kopioitaessa **Älä** Sisällytä tarjoukset, jotka kehystä arvot.

1. Valitse **Tarkista**määritysten.

1. Kun määritykset on vahvistettu onnistuneesti, valitse **Tallenna**.

## <a name="set-up-a-virtual-machine-template-for-the-azure-slave-plug-in"></a>Määritetään virtuaalikoneen mallin Azure toissijaisen laajennus

Virtuaalikoneen mallin määrittää laajennuksen käytetään luomaan toissijaisen solmu Azure parametrit. Seuraavissa vaiheissa on luot mallin Ubuntu AM varten.

1. Valitse **Hallitse Hudson**Hudson Raporttinäkymät-ikkunan.

1. Valitse **määrittää järjestelmän**.

1. Vieritä sivua, jotta löydät **Cloud** -osassa.

1. **Cloud** jaksoon Etsi **Lisää Azure virtuaalikoneen malli** ja valitse **Lisää** -painiketta.

    ![Lisää AM malli][add vm template]

1. Määritä cloud palvelunimi **nimi** -kenttään. Jos kirjoittamasi nimi viittaa aiemmin pilvipalvelussa, kyseisen palvelun valmisteltu AM. Muussa tapauksessa Azure Luo uusi tunnus.

1. Kirjoita **kuvaus** -kenttään luotavan mallin kuvailevan tekstin. Tiedot on vain asiakirjojen tarkoituksiin eikä ei käytetä valmistelu AM.

1. Lisää **linux** **otsikot** -kenttään. Tämän tarran mallia, jota olet luomassa tunnistetaan ja käytetään myöhemmin viittaavat mallia, kun Hudson projektin luominen.

1. Valitse alue, jossa AM luodaan.

1. Valitse haluamasi AM koko.

1. Määritä tallennustilan-tili, johon AM luodaan. Varmista, että se on sama alue kuin käytetään pilvipalvelussa. Jos haluat luoda uuden tallennustilan, voit jättää tämän kentän tyhjäksi.

1. Säilytys aika määrittää minuuttia, ennen kuin Hudson poistaa käyttämättömänä toissijaisen määrän. Jätä tämä 60 oletusarvo.

1. Valitse **käyttö**-haluamasi ehto, kun toissijaisen solmu käytetään. Nyt Valitse **mahdollisimman paljon solmu Utilize**.

    Tässä vaiheessa lomakkeen näyttää hieman tämän tapaista:

    ![mallin config][template config]

1. **Kuva perhe** tai tunnus on määritettävä, mitä järjestelmän kuva asennetaan oman AM. Voit kuva perheille luettelosta tai määrittää mukautetun kuvan.

    Jos haluat valita kuvan perheille luettelosta, kirjoita kuvan Sukunimi ensimmäisen merkin (kirjainkoko on merkitsevä). Esimerkiksi kirjoittamalla **U** Avaa Ubuntu palvelimen perheille luettelo. Kun olet valinnut luettelosta, Jenkins käyttää kyseisen perheen järjestelmän, että kuva uusimman version, kun valmistelu oman AM.

    ![OS perhe-luettelo][OS family list]

    Jos sinulla on mukautettu kuva, jota haluat käyttää sen sijaan, kirjoita nimi mukautetun kuvan. Mukautetun kuvan nimet ei näy luettelon, joten varmista, että nimi on kirjoitettu oikein.    

    Kirjoita **U** noutaa Ubuntu kuvia luettelo ja valitse **Ubuntu Server 14.04 l.**Tässä opetusohjelmassa.

1. **Menetelmä Käynnistä**Valitse **SSH**.

1. Alla olevaa komentosarjaa kopioiminen ja liittäminen **alusta komentosarja** -kenttään.

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

    **Alusta komentosarja** suoritetaan AM luomisen jälkeen. Tässä esimerkissä komentosarja asentaa Java git ja ant.

1. Kirjoita **käyttäjänimi** ja **salasana** -kentissä, jotka luodaan oman AM järjestelmänvalvojatilin haluamasi arvot.

1. Valitse **Tarkista mallia** ja tarkistaa, jos määrittämäsi parametrit on voimassa.

1. Valitse **Tallenna**.

## <a name="create-a-hudson-job-that-runs-on-a-slave-node-on-azure"></a>Luo Hudson työ, joka suoritetaan Azure toissijaisen solmun

Tässä osiossa luot Hudson tehtävän, joka suoritetaan Azure toissijaisen solmun.

1. Valitse Hudson Raporttinäkymät-ikkunan **Uusi projekti**.

1. Kirjoita nimi, jonka luot työ.

1. Projektin tyyppi-asetukseksi **luominen - tyyppiseksi ohjelmiston työn**.

1. Valitse **OK**.

1. Valitse määritys-sivulla **kohtaa, johon voidaan suorittaa tämän projektin rajoita**.

1. Valitse **solmu ja otsikko-valikko** ja valitse **linux** (on määritetty tämän tarran luotaessa virtuaalikoneen mallin edellisessä osassa).

1. **Muodosta** -osassa valitsemalla **Lisää Muodosta vaihe** ja valitse **Suorita shell**.

1. Muokkaa seuraavaa komentosarjaa, **{github tilin nimen}**, **{projektinimi}**ja **{projektin hakemistossa}** tilalle haluamasi arvot, ja liitä muokattu komentosarja, joka näkyy tekstialueelle.

        # Clone from git repo

        currentDir="$PWD"

        if [ -e {your project directory} ]; then

            cd {your project directory}

            git pull origin master

        else

            git clone https://github.com/{your github account name}/{your project name}.git

        fi

        # change directory to project

        cd $currentDir/{your project directory}

        #Execute build task

        ant

1. Valitse **Tallenna**.

1. Etsi Hudson Raporttinäkymät-ikkunan työn juuri luonut ja sitten **aikataulu muodosta** -kuvaketta.

Hudson sitten Luo toissijaisen solmu edellisessä osassa luodaan mallin avulla ja suorita muodosta tähän vaiheessa määrittämäsi komentosarja.

## <a name="next-steps"></a>Seuraavat vaiheet

Saat lisätietoja Azure käyttäminen Java [Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[tilauksen profiili]: http://go.microsoft.com/fwlink/?LinkID=396395

<!-- IMG List -->

[add new cloud]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-addcloud.png
[configure profile]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-configureprofile.png
[add vm template]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-addnewvmtemplate.png
[template config]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-templateconfig1-withdata.png
[OS family list]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-oslist.png


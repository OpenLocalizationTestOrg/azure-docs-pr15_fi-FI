<properties 
    pageTitle="Lataa Java Azure SDK" 
    description="Opi lataamaan Azure SDK for Javan otoksen koodilla maven-testi projektien ja perustoimet säädettyjen Azure Tookit Pimennys varten." 
    services="" 
    documentationCenter="java" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="multiple" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="download-the-azure-sdk-for-java"></a>Lataa Java Azure SDK

Tässä artikkelissa on ohjeet lataaminen ja asentaminen Java Azure-kirjastoissa.

**Huomautus:** Java Azure-kirjastot jaetaan [Apache käyttöoikeus, versiosta 2.0]-kohdassa[license].

## <a name="azure-libraries-for-java---manual-download"></a>Azure kirjastojen Java - Manuaalinen lataaminen

Manuaalinen asennus Java Azure-kirjastoja, valitsemalla Lataa ZIP-tiedosto, jossa on kaikki kirjastoja ja kaikki riippuvuudet <http://go.microsoft.com/fwlink/?LinkId=690320> .

Kun olet ladannut zip-tiedoston tietokoneeseen, Pura sisältö ja PURKKI tiedostojen lisääminen projektiin jokin seuraavista vaihtoehdoista avulla:

* Tuo Pimennys Java projektin PURKKI tiedostot.

* Määrittää **Luominen polku** Java projektin Pimennys sisältämään PURKKI-tiedostojen polku.

Lisätietoja muodosta liikeratoja Pimennys määrittämisestä on artikkelissa [Java luominen polku] Pimennys-sivustosta.

**Huomautus:** Katso license.txt ja ThirdPartyNotices.txt tiedosto sisällä ZIP-käyttöoikeus ja muita tietoja.

## <a name="azure-libraries-for-java---building-with-maven"></a>Java - Azure kirjastojen luomista ja maven-testi

### <a name="step-1---set-up-your-project-to-use-maven-for-build"></a>Vaihe 1 – projektin määrittäminen käyttämään muodosta maven-testi

Maven-testi projektien luominen Pimennys, jotka käyttävät Azure kirjastojen Java-noudattamalla [Aloittaminen Azure hallinta kirjastojen Java] [ maven-getting-started] artikkelissa. 

### <a name="step-2---configure-your-maven-settings-with-the-requisite-dependencies"></a>Vaihe 2: edeltävät riippuvuudet ja maven-testi-asetusten määrittäminen

Kun projekti on määritetty käyttämään maven-testi muodosta, voit lisätä syntaksin pom.xml tiedoston edeltävät riippuvuudet, kuten seuraavassa esimerkissä. Huomaa, että sinun ei tarvitse lisätä jokaisen riippuvuus, joka näkyy seuraavassa esimerkissä; haluat vain lisätä tietyn riippuvuuksia, mitä projektiin edellyttää.

> [AZURE.NOTE] Kunkin `<version>` seuraava näyte osan korvaaminen tässä esimerkissä "n.n.n" paikkamerkit kelvollinen versio lukuja, jotka saadaan [Azure kirjastojen säilöön-maven-testi].

    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-compute</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-network</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-sql</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-storage</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-websites</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-media</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-servicebus</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-serviceruntime</artifactId>
        <version>n.n.n</version>
    </dependency>

## <a name="installing-the-azure-toolkit-for-eclipse"></a>Azure-työkalut asennetaan Pimennys

Tässä osassa on basic asennusohjeet varten Pimennys; Azure-Työkalut Lisätietoja on artikkelissa [asentamista varten Pimennys Azure-Työkalut].

### <a name="prerequisites"></a>Edellytykset

1. Windows-operting järjestelmien [uusiin varten Pimennys Azure-Työkalut] -artikkelissa.
1. Macintosh- tai Linux operting järjestelmät [uusiin varten Pimennys Azure-Työkalut] -artikkelissa.
1. Pimennys IDE Java ss kehittäjille Indigo tai uudempi versio. Tämä voi ladata <http://www.eclipse.org/downloads/>.

### <a name="basic-installation-steps"></a>Asennuksen perusvaiheet

1. Valitse **Asenna uusi ohjelmisto**Pimennys, **Ohje** -valikosta.
1. Kirjoita sivuston sijainnin <http://dl.microsoft.com/eclipse> ja paina **Enter**-näppäintä.
1. Valitse palautettavat kohteet asenneta, ja valitse **Valmis**.

Azure-Työkalut, Pimennys käyttää Azure SDK uusimman version. Tämä voi ladata käyttämällä <http://go.microsoft.com/fwlink/?LinkID=252838>WWW-ympäristö asennusohjelma (WebPI). Jos se ei ole, kun luot ensimmäisen Azure käyttöönottoprojektin varten Pimennys Azure-työkalujen asentaa automaattisesti Azure SDK oikean version.

## <a name="see-also"></a>Katso myös

[Azure työkalujen Pimennys varten]

[Azure-työkalut asennetaan Pimennys] 

[Pimennys Azure Hei maailma-sovelluksen luominen]

Saat lisätietoja Azure käyttäminen Java [Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure kirjastojen säilöön-maven-testi]: http://go.microsoft.com/fwlink/?LinkID=286274
[Azure työkalujen Pimennys varten]: http://go.microsoft.com/fwlink/?LinkID=699529
[Pimennys Azure Hei maailma-sovelluksen luominen]: http://go.microsoft.com/fwlink/?LinkID=699533
[Azure-työkalut asennetaan Pimennys]: http://go.microsoft.com/fwlink/?LinkId=699546
[Java muodosta polku]: http://help.eclipse.org/luna/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2Freference%2Fref-properties-build-path.htm
[license]: http://www.apache.org/licenses/LICENSE-2.0.html
[maven-getting-started]: http://go.microsoft.com/fwlink/?LinkID=622998
[zip-download]: http://go.microsoft.com/fwlink/?LinkId=690320
[Mitä uusia ominaisuuksia, Pimennys Azure Työkalut]: http://go.microsoft.com/fwlink/?LinkId=690333

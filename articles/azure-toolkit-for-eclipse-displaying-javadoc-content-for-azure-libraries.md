<properties
    pageTitle="Javadoc sisällön näyttäminen Pimennys Java Azure kirjastot-paketti"
    description="Miten Javadoc sisällön näyttäminen Pimennys Azure-kirjastojen."
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

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh698319.aspx -->

# <a name="displaying-javadoc-content-in-eclipse-for-the-azure-libraries-package-for-java"></a>Javadoc sisällön näyttäminen Pimennys Java Azure kirjastot-paketti #

Java Azure-kirjastojen Javadoc sisällön voi tarkastella Pimennys-ympäristöön liittämällä Java Azure-kirjastojen Javadoc näytettävä sisältö. Seuraamalla näitä ohjeita noudattamalla voit käyttää tätä toimintoa Pimennys kuluessa.

Tässä toimintosarjassa oletetaan, että olet jo lisännyt Java Azure-kirjaston muodosta polku.

## <a name="to-display-javadoc-content-in-eclipse-for-the-azure-libraries-for-java"></a>Javadoc olevan sisällön näyttäminen Pimennys Java Azure-kirjastojen ##

* Pimennys 's Project Explorer sisällä projektin **Viitattu kirjastoissa** osassa Avaa pikavalikon Azure-kirjaston Java JAR. Esimerkiksi **microsoft-windowsazure-api-0.1.0.jar** (versionumero voi olla eri, minkä version olet asentanut riippuvainen).
* Valitse **Ominaisuudet**.
* Olet **Ominaisuudet** -valintaikkunan vasemmanpuoleisessa ruudussa Napsauta **Javadoc sijainti**. **Javadoc sijainti** -valintaikkuna tulee näkyviin.
* Voit määrittää **Javadoc URL-osoite**tai **Javadoc arkistoon**.
    * Jos haluat määrittää **Javadoc URL-Osoitteen**, käytä URL-osoitteet, kuten **http://dl.windowsazure.com/javadoc** tai **http://dl.windowsazure.com/storage/javadoc**.
    * Jos haluat käyttää **Javadoc arkistoon**, voit määrittää ulkoisen tiedoston tai työtilatiedoston.
    Tee valintasi ja Selaa ja vahvista tarpeen mukaan. Seuraavassa esimerkissä liittää Java Azure-kirjastoja vastaava Javadoc JAR, joka on ladattu paikallisesti **c:\MyAzureJARs**-nimiseen kansioon.
    ![][ic553487]
* *Valinnainen*: Valitse **Vahvista**. Tässä saattaa näkyä Javadoc JAR mahdolliset ongelmat.
* Valitse **OK**.

Kun kirjastoon liittyvän Javadoc sisällön pitäisi näyttää Pimennys IDE kuluessa. Esimerkiksi jos `blob` on määritetty tyypin `CloudBlockBlob` oman koodiin seuraavassa on esimerkki Javadoc sisältöön, joka tulee näkyviin, kun kirjoitat `blob.acquireLease` koodissa:

![][ic553488]

## <a name="see-also"></a>Katso myös ##

[Azure työkalujen Pimennys varten][]

[Pimennys Azure Hei maailma-sovelluksen luominen][]

[Azure-työkalut asennetaan Pimennys][] 

Saat lisätietoja Azure käyttäminen Java [Azure Java Developer Center][].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure työkalujen Pimennys varten]: http://go.microsoft.com/fwlink/?LinkID=699529
[Pimennys Azure Hei maailma-sovelluksen luominen]: http://go.microsoft.com/fwlink/?LinkID=699533
[Azure-työkalut asennetaan Pimennys]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic553487]: ./media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553487.png
[ic553488]: ./media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553488.png
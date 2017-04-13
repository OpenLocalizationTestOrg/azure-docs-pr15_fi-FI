<properties
    pageTitle="Azure Palvelupäätepisteet"
    description="Kuvataan varten Pimennys Azure-työkalujen Azure palvelupäätepiste-asetukset."
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

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn268600.aspx -->

# <a name="azure-service-endpoints"></a>Azure Palvelupäätepisteet #

Azure päätepisteiden selvittää, onko sovelluksen on otettu käyttöön ja hallinnassa on käytössä yleinen Azure-ympäristö, Azure 21vianetin ylläpitämä Kiinan tai yksityisen Azure ympäristössä. **Palvelupäätepisteet** -valintaikkunan avulla voit määrittää, mitä palvelun päätepisteitä, jota haluat käyttää. Avaa **Palvelupäätepisteet** -valintaikkuna sisällä Pimennys, **ikkunan** **asetukset**, laajenna **Azure**ja valitse **Päätepisteiden**. Määrittäminen **Aseta aktiivinen** kenttä määrittää, mitkä Azure päätepisteiden käytetään nykyisen työtilassa Azure projektien.

Seuraavassa kuvassa näkyy **Palvelupäätepisteet** -valintaikkuna.

![][ic719493]

## <a name="to-set-the-service-endpoints"></a>Voit määrittää palvelujen päätepisteitä ##

**Palvelupäätepisteet** -valintaikkunassa tee jompikumpi seuraavista toimista:

* Jos haluat käyttää Yleinen Azure alustan, **Määritä aktiivinen** -avattavasta luettelosta valitsemalla **windowsazure.com** ja valitse **OK**.
* Jos haluat käyttää Azure **Active määrittäminen** -avattavasta luettelosta Kiinassa, 21vianetin ylläpitämä Valitse **windowsazure.cn (kiina)** ja valitse **OK**.
* Jos haluat käyttää yksityinen Azure ympäristö:
    1. Valitse **Muokkaa**.
    2. Näkyviin tulee valintaikkuna, jossa ilmoitetaan, että **Palvelupäätepisteet** -valintaikkuna on suljettu, ja määrittää asetustiedoston avataan. Valitse **OK**.
    3. Preferencesets.xml-tiedostossa, Luo uusi `preferenceset` elementti. Tämän uuden osan luominen `name`, `blob`, `management`, `portalURL` ja `publishsettings` määritteet ja lisää arvot niille, jotka vastaavat yksityinen Azure ympäristö. Voit käyttää nykyisen annettujen arvojen `preferenceset` malleiksi osat. **Huomautus**: käytettävä arvo `blob` määritteen on oltava teksti "blob" URL-osoitteen.
    4. Tallenna ja sulje preferencesets.xml.
    5. Avaa **Palvelupäätepisteet** -valintaikkuna.
    6. **Määritä aktiivinen** -avattavasta luettelosta, jonka loit aktiivinen Valitse ja valitse **OK**.
    7. Kun olet luonut yksityinen Azure ympäristö `preferenceset` elementti, voit muuttaa arvoja määritetty valitsemalla **Services päätepisteen** -valintaikkunan **Muokkaa** -painiketta. Voit myös luoda useita yksityinen Azure ympäristö `preferenceset` elementit, kysymykset.

## <a name="see-also"></a>Katso myös ##

[Azure työkalujen Pimennys varten][]

[Azure-työkalut asennetaan Pimennys][] 

[Pimennys Azure Hei maailma-sovelluksen luominen][]

Saat lisätietoja Azure käyttäminen Java [Azure Java Developer Center][].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure työkalujen Pimennys varten]: http://go.microsoft.com/fwlink/?LinkID=699529
[Pimennys Azure Hei maailma-sovelluksen luominen]: http://go.microsoft.com/fwlink/?LinkID=699533
[Azure-työkalut asennetaan Pimennys]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic719493]: ./media/azure-toolkit-for-eclipse-azure-service-endpoints/ic719493.png

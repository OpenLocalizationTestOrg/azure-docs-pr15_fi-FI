<properties
    pageTitle="Suurissa yrityksissä käyttöönotto"
    description="Opi ottamaan suurissa yrityksissä Pimennys Azure-työkalujen avulla."
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

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn268601.aspx -->

# <a name="deploying-large-deployments"></a>Suurissa yrityksissä käyttöönotto #

Jos käyttöönoton on liian suuri sisältyvän approot oletuskansioon, voit käyttää paikallisen säilön resurssin käyttöönoton pääkansio, että JDK ja sovelluspalvelin.

## <a name="to-use-a-local-storage-resource-as-the-deployment-root-folder-for-large-deployments"></a>Paikallinen tallennus resurssin käytettävän suurissa yrityksissä käyttöönotto-pääkansio ##

1. Luo uusi paikallinen tallennus resurssi. Resurssin nimi ei ole merkitystä. Tallennustilan resursseja on määritetty rooli tasolla. Nopein tapa käyttää paikallisen säilön asetusten valintaikkuna, joista voit luoda paikallisen säilön uusi resurssi on seuraavalla tavalla: rooli **Project Explorer** -näkymässä hiiren kakkospainikkeella (Laajenna projektisolmu Azure, jos et näe rooli), **Azure**, ja valitse sitten **Paikallinen tallennus**. Sisällä **Paikallinen tallennus** -valintaikkuna valitsemalla **Lisää** Luo uusi paikallinen tallennus resurssi.
1. Määritä haluamasi koko vähintään 2048 megatavua (mitään vähemmän voi aiheuttaa saman tiedoston kokoa ongelmia, kun ilmenee approot).
1. Varmista, että **Tyhjennä sisältö, kun rooli esiintymä on kuluessa** on valittuna, Tämä auttaa estämään käyttöönoton käynnistys logiikan suorittamisen ristiriidat huomioon resurssin olemassa tiedostojen, kun rooli esiintymä on kuluessa.
1. Varmistaa, että **ympäristömuuttuja tallentaminen resurssin kansiopolku käyttöönoton jälkeen** -arvoksi on määritetty merkkijonon **DEPLOYROOT**. Paikallinen tallennus resurssien-valintaikkuna näyttää seuraavankaltaiselta.
    ![][ic667943]

Vaihtoehtoisesti, jos käytät **DEPLOYROOT** paikallisen resurssin *nimi* ja vaihtamaan automaattisesti luotu ympäristön muuttujan nimi (joka määritetään **DEPLOYROOT_PATH** tässä tapauksessa), jotka toimivat sovelluksen sekä.

Lisätietoja paikallisesta säilöstä resurssin luominen tukikäytännöistä [Paikallinen tallennus ominaisuudet][].

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
[Paikallinen tallennus-ominaisuudet]: http://go.microsoft.com/fwlink/?LinkID=699525#local_storage_properties

<!-- IMG List -->

[ic667943]: ./media/azure-toolkit-for-eclipse-deploying-large-deployments/ic667943.png

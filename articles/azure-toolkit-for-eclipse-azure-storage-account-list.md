<properties
    pageTitle="Azure-tallennustilan tilin luettelo"
    description="Azure-työkalujen käyttäminen Pimennys tallennustilan-käyttäjätilin asetusten hallinta"
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

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn205108.aspx -->

# <a name="azure-storage-account-list"></a>Azure-tallennustilan tilin luettelo #

Azure-tallennustilan tilit käyttöön Lataa sijainnit, jota käytetään JDK, sovelluspalvelin ja haluamaansa osat sekä tilan tallentaminen välimuistiin käytettäessä. Pimennys ylläpitää tunnetut tallennustilan-tileistä, jotka ovat käytettävissä projektien Pimennys työtilassa. Avaa **Tallennustilan tilit** -valintaikkuna, jossa käytetään hallitsemaan luetteloa sisällä Pimennys, **ikkunan** **asetukset**, laajenna **Azure**ja **Tallennustilaa**tilit.

Seuraavassa kuvassa näkyy **Tallennustilan tilit** -valintaikkuna.

![][ic719496]

Tämän valintaikkunan voi avata myös **tilit** -linkistä valintaikkunoissa, jotka käyttävät tilit, kuten jokin seuraavista:

* **Palvelinkokoonpanon** -valintaikkunan **JDK** -välilehti.
* **Palvelinkokoonpanon** -valintaikkunasta **palvelin** -välilehti.
* **Lisää osa** -valintaikkuna.
* **Välimuisti** ominaisuudet-valintaikkuna.

## <a name="to-import-your-storage-accounts-using-a-publish-settings-file"></a>Voit tuoda tallennustilan asiakkaiden Julkaise asetukset-tiedoston avulla ##

1. Sisällä **Tallennustilan tilit** -valintaikkuna Valitse **JULKAISE - asetustiedosto tuo**.
2. (Ohita tämä vaihe, jos olet jo tallentanut Julkaise asetustiedosto paikallisessa tietokoneessa.) Valitse **Tuo tilauksen tiedot** -valintaikkunasta **Lataa JULKAISE-asetustiedosto**. Jos et ole vielä kirjautunut Azure-tili, sinua pyydetään kirjautumaan. Valitse sinua pyydetään Tallenna Azure julkaista asetustiedosto. (Tuloksena kirjautuminen - sivuilla näkyviä ohjeita voit ohittaa ne Azure portaalin tarjoamien ja on tarkoitettu Visual Studio käyttäjille.) Tallentaa paikallisessa tietokoneessa.
3. Edelleen: **Tuo Tilaustiedot** -valintaikkuna napsauttamalla **Selaa** -painiketta, valitse aiemmin tallentamasi paikallisesti Julkaise asetukset-tiedosto ja valitse sitten **Avaa**.
4. Valitse **OK** ja sulje **Tuo tilauksen tiedot** -ikkunaan.

## <a name="to-create-a-new-storage-account"></a>Luo uusi tallennustilan-tili ##

1. Sisällä **Tallennustilan tilit** -valintaikkuna valitsemalla **Lisää**.
2. **Lisää tallennustilaa tili** -valintaikkunassa sisällä valitsemalla **Uusi**.
3. Sisällä **Uusi tallennustilan tili** -valintaikkunassa Määritä seuraavat arvot:
    * Tallennustilan tilin nimi.
    * Tallennustilan tilin sijainti.
    * Tallennustilan tilin kuvaus.
    * Tilaus, johon tallennustilan tilin kuuluu.
4. Valitse Sulje **Uusi tallennustilan tili** -valintaikkunassa **OK** .

Voi kestää useita minuutteja tallennustilan tilin luominen. Kun se on luotu, valitse **OK** ja sulje **Lisää tallennustilaa tili** -valintaikkunassa ja tallennustilaa tililuettelon lisätään uusi tallennustilan tilisi.

## <a name="to-add-an-existing-storage-account-to-the-list"></a>Voit lisätä käytössä olevan tallennustilan tilin luetteloon ##

1. Jos sinulla ei ole Azure-tallennustilan tilin, luo sellainen seuraamalla **Voit luoda uuden tallennustilan tilin osan** olevien ohjeiden mukaisesti. (Vaihtoehtoisesti voit luoda uuden tallennustilan tilin osoitteessa [Azure hallinta-portaalin][].)
2. Sisällä **Tallennustilan tilit** -valintaikkuna valitsemalla **Lisää**.
3. Kirjoita arvot sisällä **Lisää tallennustilaa tili** -valintaikkunassa **nimi** ja **Pikanäppäin**. Tilin nimi ja access-näppäin on oltava Azure tallennustilan-tilille. [Azure hallinta-portaalin][] **tallennustilan** -osan avulla voit tarkastella tallennustilan tilin nimi ja avaimet. **Lisää tallennustilaa tili** -valintaikkuna näyttää seuraavankaltaiselta.

    ![][ic719497]

4. Valitse Sulje **Lisää tallennustilaa tili** -valintaikkunassa **OK** .

## <a name="to-modify-a-storage-account-to-use-a-new-access-key"></a>Jos haluat muokata tallennustilan-tili, jota käytetään uusien pikanäppäin ##

1. Sisällä **Tallennustilan tilit** -valintaikkunan Valitse tallennustilan tili, jota haluat muokata, ja valitse sitten **Muokkaa**.
2. **Tallennustilan tilin pikanäppäin muokkaaminen** -valintaikkunan sisällä muokata **Pikanäppäin** -arvoa.
3. Valitse **OK** ja sulje **Tallennustilan tilin pikanäppäin muokkaaminen** -valintaikkuna.

## <a name="to-remove-a-storage-account-from-the-list-maintained-in-eclipse"></a>Jos haluat poistaa tallennustilan tilin ylläpidetään Pimennys luettelosta ##

1. Sisällä **Tallennustilan tilit** -valintaikkunan Valitse tallennustilan tili, jota haluat muokata, ja valitse sitten **Poista**.
2. Valitse **OK** tallennustilan tilin poistaminen pyydettäessä.

>[AZURE.NOTE] **Tallennustilan tilit** -valintaikkunan kautta tallennustilan-tilin poistaminen vain poistaa sen sisällä Pimennys näytettävä tallennustilan tilien luettelo. Se ei poista tallennustilan tilin Azure tilauksesta. Lisäksi tallennustilan tili saattaa näkyä uudelleen luettelon jälkeen Pimennys Lataa tilauksen tiedot.

## <a name="see-also"></a>Katso myös ##

[Azure työkalujen Pimennys varten][]

[Azure-työkalut asennetaan Pimennys][] 

[Pimennys Azure Hei maailma-sovelluksen luominen][]

Saat lisätietoja Azure käyttäminen Java [Azure Java Developer Center][].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure työkalujen Pimennys varten]: http://go.microsoft.com/fwlink/?LinkID=699529
[Azure hallinta-portaalissa]: http://go.microsoft.com/fwlink/?LinkID=512959
[Pimennys Azure Hei maailma-sovelluksen luominen]: http://go.microsoft.com/fwlink/?LinkID=699533
[Azure-työkalut asennetaan Pimennys]: http://go.microsoft.com/fwlink/?LinkId=699546
[What's New in the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699552

<!-- IMG List -->

[ic719496]: ./media/azure-toolkit-for-eclipse-azure-storage-account-list/ic719496.png
[ic719497]: ./media/azure-toolkit-for-eclipse-azure-storage-account-list/ic719497.png

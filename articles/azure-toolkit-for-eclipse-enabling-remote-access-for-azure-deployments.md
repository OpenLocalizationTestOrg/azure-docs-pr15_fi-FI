<properties
    pageTitle="Azure-Pimennys versioiden ottaminen käyttöön etäkäyttö"
    description="Opettele käyttöön etäkäyttöpalvelimen Azure-versioiden Pimennys Azure-työkalujen avulla."
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

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690951.aspx -->

# <a name="enabling-remote-access-for-azure-deployments-in-eclipse"></a>Azure-Pimennys versioiden ottaminen käyttöön etäkäyttö

Käyttöönoton vianmääritystä saattaa ottaminen käyttöön ja muodostaa isännöinnin käyttöönoton virtuaalikoneen etäkäyttöpalvelimen avulla. Etäkäyttö luottaa Valitse työpöydän RDP Remote Protocol (). Voit määrittää etäkäyttöpalvelimen käyttöönoton jälkeen se on julkaistu Azure, tai jos käytössäsi on Pimennys Windows-käyttöjärjestelmä, voit määrittää etäkäyttöpalvelimen ennen kuin julkaiset Azure. Huomaa, että sinun on remote työpöytäsovellus, joka on yhteensopiva käyttämäsi käyttöjärjestelmän muodostamiseksi koko käyttöönotto virtual machine Azure-tietokannassa.

## <a name="how-to-enable-remote-access-before-you-deploy-to-azure"></a>Etäkäyttö ottaminen käyttöön, ennen kuin otat Azure

> [AZURE.NOTE] Jotta etäkäyttöpalvelimen ennen niiden käyttöönottoa Azure sovellusta, sinun on Pimennys on käynnissä Windows.

Seuraava kuva esittää valintaikkunan käytetään etäkäyttöä **Etäkäyttöä** ominaisuudet.

![][ic719494]

Näytä **Etäkäyttöpalvelimen** ominaisuudet-valintaikkunan kahdella tavalla:

* Valitse **Lisäasetukset** -linkki **Etäkäyttöpalvelimen** kohdassa **Julkaise Azure** -valintaikkuna.
* Avaa Azure projektin **Ominaisuudet** -valintaikkuna.

Kun luot uuden Azure käyttöönottoprojektin, projektin ei ole käytössä oletusarvoisesti etäkäyttö. Voit kuitenkin helposti ottaa etäkäyttöpalvelimen määrittämällä käyttäjänimi ja salasana, **Julkaise Azure** -valintaikkunassa. Etäkäyttö salasana salataan käyttämällä X.509-varmenteita. Jos et käytä antaa oman varmenteen salaus riippuvainen Pimennys Azure ‑laajennuksen sisältyy itse allekirjoitettua varmennetta. Tämä itse allekirjoitetun varmenteen on sekä julkinen sertifikaattitiedosto (SampleRemoteAccessPublic.cer) ja henkilökohtaiset tiedot Exchange (PFX) sertifikaattitiedosto (SampleRemoteAccessPrivate.pfx) tallennettuja Azure projektin **varmenne** -kansiossa. Tämä sisältää sertifikaatin yksityinen avain, ja siinä on oletusarvoinen salasanaa, **Salasana1**. Kuitenkin salasana on julkinen knowledge, varmennetta käytetään vain käyttöön liittyviä tarkoituksiin ei tuotannon-käyttöönottoa varten. Niin kuin käyttöön liittyviä tarkoituksiin, kun haluat ottaa käyttöön oman käyttöönotoissa istuntojen olisi napsautat **Julkaise Azure** -valintaikkunassa voit määrittää oman varmenteen **Lisäasetukset** -linkkiä. Huomaa, että sinun on Lataa sertifikaatti PFX-version isännöityä palvelun Azure hallinta-portaalin niin, että Azure purkaa käyttäjän salasana.

Opetusohjelman loppuosa näytetään, miten käyttöön Azure-käyttöönotto-projektin, joka on alun perin luotu etäkäyttöpalvelimen käytöstä etäkäyttö. Tarkoitetaan tässä opetusohjelmassa luodaan uusi itse allekirjoitetun varmenteen ja sen .pfx-tiedosto on valittua salasanan. Voit myös halutessasi sertifikaatilla varmenteen myöntäjältä.

## <a name="how-to-enable-remote-access-after-you-have-deployed-to-azure"></a>Etäkäyttö ottaminen käyttöön, kun olet ottanut käyttöön Azure

Voit ottaa etäkäyttöpalvelimen sen jälkeen, kun olet ottanut käyttöön Azure käyttöön seuraavasti:

1. Kirjaudu sisään käyttämällä Azure tiliä Azure hallinta-portaalin
1. **Pilvipalveluihin**luettelosta Valitse käyttöön pilvipalvelussa
1. Napsauta cloud palvelun web-sivun **Määritä** linkkiä
1. Valitse määritys-sivun alareunaan **Remote** linkki
1. Kun näkyviin tulee ponnahdusikkunoiden valintaikkuna:
    * Määritä rooli, jonka haluat otat etäkäyttöä
    * Valitse **Ota käyttöön etätyöpöytä** -valintaruutu
    * Määritä käyttäjänimi ja salasana, jota haluat käyttää etäkäyttöä varten
    * Valitse varmenne, jonka avulla
1. Valitse **OK** 

Näkyviin tulee viesti, jossa ilmoitetaan, että kokoonpanon muutos on käynnissä, joka saattaa kestää muutaman minuuttia. Kun määritysten muutos on valmis, **Kirjaudu etänä** osan tämän artikkelin ohjeiden mukaisesti.
    
## <a name="how-to-enable-remote-access-in-your-package"></a>Miten voit ottaa käyttöön etäkäyttöpalvelimen pakettiin

1. Pimennys 's Project Explorer-ruudussa Azure projektin hiiren kakkospainikkeella ja valitse **Ominaisuudet**.

1. **Ominaisuudet** -valintaikkunassa **Azure** Laajenna vasemmanpuoleisessa ruudussa ja valitse **Etäkäyttö**.

1. Varmista **Etäkäyttöpalvelimen** -valintaikkunasta **Ota käyttöön kaikki roolit työpöydän etäyhteyksien kanssa nämä kirjautumistietosi** on valittuna.

1. Määritä Etätyöpöytäyhteys käyttäjänimi.

1. Määritä ja vahvista salasana käyttäjälle. Määrittää tässä valintaikkunassa käyttäjän käyttäjänimi ja salasana, arvoja käytetään, kun teet Etätyöpöytäyhteys. (Huomaa, että se PFX salasana erillistä salasanaa).

1. Määritä käyttäjätilin päättymispäivän.

1. Valitse **Uusi** voit luoda uuden itse allekirjoitettua varmennetta. (Vaihtoehtoisesti voit valita varmenteen työtila tai tiedosto-järjestelmästä **työtilan** tai **tiedostojärjestelmän** -painikkeiden avulla vastaavasti mutta tarkoitetaan tässä opetusohjelmassa on luo uutta varmennetta.)

    * Määritä **Uusi varmenne** -valintaikkuna ja vahvista salasana käytät PFX tiedoston.

    * Hyväksy annettu **Nimi (CN)**tai käytä mukautettua nimeä.

    * Määritä uusi varmenne .cer-lomakkeen tallennuspaikka polku ja tiedostonimi. Tämä vaihe ja siirry seuraavaan vaiheeseen voit käyttää Azure projektin **varmenne** -kansioon, mutta maksuttomia jokin muu sijainti. Tarkoitetaan tässä opetusohjelmassa käytetään **c:\mycert\mycert.cer**. (Luo ennen jatkamista **c:\mycert** -kansio tai Käytä aiemmin luodun kansion halutessasi.)

    * Määritä polku ja tiedostonimi, johon uuden varmenteen ja sen yksityinen avain .pfx-lomakkeen tallennetaan. Tarkoitetaan tässä opetusohjelmassa käytetään **c:\mycert\mycert.pfx**. **Uusi varmenne** -valintaikkuna pitäisi muistuttaa (Päivitä kansion polku, jos et ole käyttänyt **c:\mycert**) seuraavasti:

        ![][ic712275]

    * Valitse **OK** ja sulje **Uusi varmenne** -valintaikkuna.

1. **Etäkäyttö** -valintaikkunan pitäisi näyttää seuraavankaltaiselta:</p>

    ![][ic719495]

1. Valitse **OK** ja sulje **Etäkäyttöpalvelimen** -valintaikkuna.
    
Muodosta uudelleen sovelluksen käyttöönotto cloud määrittäminen Luo kanssa.

## <a name="to-log-in-remotely"></a>Kirjaudu sisään etäyhteydellä

Kun rooli esiintymää on valmis, voit etäyhteyden sisäänkirjautumista virtuaalikoneen, joka isännöi sovelluksen.

* Jos käytät Pimennys ja Windows valittuna **Käynnistä Etätyöpöytä, valitse Ota käyttöön** -vaihtoehdon, Azure käyttöönoton aikana, näyttöön tulee kanssa Etätyöpöytäyhteys kirjautumisnäyttö käyttöönoton käynnistyessä. Kun ohjelma pyytää käyttäjänimeä ja salasanaa, kirjoita etäyhteyden käyttäjälle määritetyt arvot ja voivat kirjautua sisään.

* Toinen tapa kirjautua etäyhteyden on <a href="http://go.microsoft.com/fwlink/?LinkID=512959">Azure hallinta-portaalin</a>kautta:

    * Azure hallinta-portaalin **Cloud Services** -näkymässä pilvipalvelussa sijaitsevaan, valitse **esiintymät**, valitse tiettyä esiintymää ja napsauta sitten **Muodosta** -painiketta. **Yhdistä** -painike tulee näkyviin komentopalkin seuraavalla tavalla:

        ![][ic659273]

    * Kun olet napsauttanut **Muodosta** -painiketta, voit pyydetään RDP-tiedoston avaaminen. Avaa tiedosto ja noudata ohjeita. (Voit myös tallentaa tiedoston paikalliseen tietokoneeseen, ja suorita sitten tiedosto kaksoisnapsauttamalla sitä remote Kirjaudu sisään virtuaalikoneen tarvitsematta, siirry ensin hallinta-portaalin.)

    * Kun ohjelma pyytää käyttäjänimeä ja salasanaa, kirjoita remote käyttäjälle määritetyt arvot ja voivat kirjautua sisään.

> [AZURE.NOTE] Jos olet-Windows käyttöjärjestelmässä, sinun on Etätyöpöydän asiakas, joka on yhteensopiva käyttämäsi käyttöjärjestelmän ja ohjeiden avulla voit määrittää, että asiakkaan RDP-tiedostossa, jota olet ladannut asetuksineen.

## <a name="see-also"></a>Katso myös

[Azure työkalujen Pimennys varten][]

[Pimennys Azure Hei maailma-sovelluksen luominen][]

[Azure-työkalut asennetaan Pimennys][] 

Saat lisätietoja Azure käyttäminen Java [Azure Java Developer Center][].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure työkalujen Pimennys varten]: http://go.microsoft.com/fwlink/?LinkID=699529
[Pimennys Azure Hei maailma-sovelluksen luominen]: http://go.microsoft.com/fwlink/?LinkID=699533
[Azure-työkalut asennetaan Pimennys]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic712275]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic712275.png
[ic719495]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic719495.png
[ic719494]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic719494.png
[ic659273]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic659273.png

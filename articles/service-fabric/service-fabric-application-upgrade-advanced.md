<properties
   pageTitle="Sovelluksen päivitys: laajennettu aiheet | Microsoft Azure"
   description="Tässä artikkelissa käsitellään joitakin palvelun kangasta-sovelluksen päivittäminen liittyviä lisäohjeita."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/14/2016"
   ms.author="subramar"/>

# <a name="service-fabric-application-upgrade-advanced-topics"></a>Palvelun kangasta sovelluksen päivitys: laajennettu aiheita

## <a name="adding-or-removing-services-during-an-application-upgrade"></a>Lisäämällä tai poistamalla palveluiden sovelluksen päivityksen aikana

Jos jokin sovellus, joka on jo otettu käyttöön, ja julkaistaan päivityksen on lisätty uusi palvelu, uusi palvelu lisätään sovelluksen.  On päivitettävä ei vaikuta palveluja, jotka on jo sovelluksen osa. Kuitenkin palvelun, joka on lisätty esiintymää on oltava käynnissä, uusi palvelu on aktiivinen (käyttämällä `New-ServiceFabricService` cmdlet-komento).

Sovelluksen päivitys osana myös poistaa palvelut. Kuitenkin to-be-poistetut-palvelun nykyistä esiintymää on lopetettava ennen jatkamista päivitys (käyttämällä `Remove-ServiceFabricService` cmdlet-komento). 

## <a name="manual-upgrade-mode"></a>Manuaalisen päivityksen tila

> [AZURE.NOTE]  Lähetetä manuaalisessa tilassa pitää vain epäonnistui tai keskeytetty päivitystä varten. Valvotun tila on palvelun kangasta sovellusten suositellut päivitystilan.

Azure palvelun kangasta tarjoaa useita päivityksen tila ja klustereiden tukemaan. Valittu Käyttöönottoasetukset saattavat olla erilaiset eri ympäristöissä.

Valvotun juokseva sovelluksen päivitys on yleisimmät päivitys tuotannon käyttäminen. Kun päivityksen käytäntö määritetään, palvelun kangasta varmistaa, että sovellus on kunnossa ennen päivitystä etenee.

 Sovelluksen järjestelmänvalvojan avulla manuaalinen juokseva sovelluksen päivityksen tila on yhteensä hallintaoikeutta päivityksen edistyminen eri päivityksen toimialueiden kautta. Tämä tila on hyödyllinen, kun mukautettu tai monimutkaisia kunto arvioinnin käytännön vaaditaan tai poikkeavien päivitys tapahtuu (esimerkiksi sovellus on jo tietojen menettämisen).

Lopuksi automaattinen juokseva sovelluksen päivitys on hyötyä kehittäminen tai testausta ympäristöissä nopea iteraation jakson aikana palvelun tarjoamista varten.

## <a name="change-to-manual-upgrade-mode"></a>Muuta manuaalisen päivityksen tila
**Manuaalinen**– Pysäytä sovelluksen päivitys osoitteessa nykyisen UD ja muuta päivityksen tila lähetetä manuaalisesti. Kun järjestelmänvalvoja on Soita manuaalisesti **MoveNextApplicationUpgradeDomainAsync** Jatka päivitystä tai käynnistäminen peruutus mukaan aloitetaan uusi päivitys. Kun päivitys Lisää manuaalisen tilaan, se pysyy manuaalisen-tilassa, kunnes uusi päivitys on aloitettu. **GetApplicationUpgradeProgressAsync** -komento palauttaa KANGASTA\_SOVELLUKSEN\_päivittää\_tilan\_liikkuva\_ETEENPÄIN\_odottaa.

## <a name="upgrade-with-a-diff-package"></a>Päivitä eroavuus-paketissa

Palvelun kangasta-sovellus voidaan päivittää avulla valmistelu koko, itsenäinen sovelluspaketin. Sovelluksen voi päivittää myös käyttämällä eroavuus-paketti, joka sisältää vain päivitetty sovellus-tiedostoja, päivitetyt sovellusluettelo ja tiedostojen service-tiedostot.

Koko sovelluspaketin sisältää kaikki tarvittavat pystytkin yhä käyttämään palvelua kangasta-sovelluksen tiedostot. Eroavuus paketti sisältää vain ne tiedostot, jotka ovat muuttuneet viimeisen säännöstä ja nykyisen päivityksen välillä sekä koko sovellusluettelo ja palvelun luettelon tiedostot. Viittausten sovellusluettelo tai palvelu-luettelo, joka ei löydy muodosta asettelun etsitään kuva-kaupasta.

Koko sovelluksen paketit ovat sovelluksen klusterin ensimmäinen asennus edellyttää. Seuraavat päivitykset voi olla koko sovelluspaketin tai eroavuus paketin.

Kun käyttämällä eroavuus paketti on hyvä vaihtoehto tästä huolimatta:

* Eroavuus paketin suositellaan, kun käytössä on suuren sovelluspaketin, joka viittaa useiden tiedostojen service-tiedostot ja/tai useita koodi-paketteja, config pakettien tai tietojen pakettien.

* Eroavuus paketti on ensisijainen käyttöönotto-järjestelmään, joka luo muodosta asettelun suoraan sovelluksen muodostusprosessin ollessasi. Tässä tapauksessa vaikka koodi ei ole muuttunut, äskettäin luodun kokoonpanot Hae eri tarkistussumma. Käyttämällä koko sovelluspaketin edellyttäisi päivittää kaikki koodi-paketit versio. Käytä eroavuus-paketti, vain antaisit tiedostot, jotka ovat muuttuneet ja tiedostojen tiedostoja, jos versio on muuttunut.

Kun sovellus päivitetään Visual Studiossa, eroavuus paketin julkaistaan automaattisesti. Eroavuus pakkauksen luominen manuaalisesti, on päivitettävä sovelluksen luettelo ja palvelun luettelot, mutta muutettu pakettien sisällytetään lopullinen sovelluspaketin. 

Esimerkiksi aloitetaan seuraavan sovelluksen (versionumerot annettu tietoja helpottamiseksi):

```text
app1        1.0.0
  service1  1.0.0
    code    1.0.0
    config  1.0.0
  service2  1.0.0
    code    1.0.0
    config  1.0.0
```

Nyt oletetaan haluat päivittää vain koodin paketin service1 eroavuus paketin PowerShellin avulla. Päivitetty sovellus on nyt seuraavat kansiorakenne:

```text
app1        2.0.0      <-- new version
  service1  2.0.0      <-- new version
    code    2.0.0      <-- new version
    config  1.0.0
  service2  1.0.0
    code    1.0.0
    config  1.0.0
```

Tässä tapauksessa voit päivittää sovelluksen luettelo 2.0.0 ja service1 vastaamaan koodin paketin päivitys palvelun luettelo. Oman sovelluspaketin kansio on seuraava rakenne:

```text
app1/
  service1/
    code/
```

## <a name="next-steps"></a>Seuraavat vaiheet

[Päivittäminen oman sovellus käyttää Visual Studio](service-fabric-application-upgrade-tutorial.md) esitellään sovelluksen päivityksen Visual Studiossa.

[Päivittäminen sovelluksen käyttämällä Powershell](service-fabric-application-upgrade-tutorial-powershell.md) esitellään sovelluksen päivityksen PowerShellin avulla.

Määrittää, miten sovelluksesi päivittää [Päivittäminen](service-fabric-application-upgrade-parameters.md)parametreilla.

Varmista sovelluksen päivitykset yhteensopivat opettelemalla käyttämään [Tietoja Sarjatoiminto](service-fabric-application-upgrade-data-serialization.md).

Sovelluksen päivitykset yleisten ongelmien korjaaminen viittaamalla [Vianmääritys sovelluksen päivitykset](service-fabric-application-upgrade-troubleshooting.md)ohjeita.
 

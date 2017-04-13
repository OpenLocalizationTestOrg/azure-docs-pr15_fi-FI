<properties
   pageTitle="Voit säilyttää vakion virtual IP-osoite, pilvipalveluun | Microsoft Azure"
   description="Lue, miten voit varmistaa, että virtual IP-osoite (VIP) Azure pilvipalvelussa ei muuta."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="how-to-retain-a-constant-virtual-ip-address-for-a-cloud-service"></a>Voit säilyttää vakion virtual IP-osoite, pilvipalveluun

Kun päivität, jota isännöidään Azure pilvipalveluun, voit joutua varmistaa, että virtual IP-osoite (VIP)-palvelun ei muutu. Monta toimialueen hallintapalvelut käyttää toimialuenimien rekisteröiminen toimialueen nimi järjestelmän (DNS). DNS toimii vain, jos VIP säilyy ennallaan. Voit **Ohjatun julkaisutoiminnon** Azure Työkalut varmistaa, että pilvipalvelussa sijaitsevaan VIP ei muutu, kun päivität sen. Saat lisätietoja toimialueen DNS-hallinnan käyttämisestä pilvipalveluihin [määrittäminen Azure pilvipalvelussa mukautettua toimialuenimeä](./cloud-services/cloud-services-custom-domain-name.md).

## <a name="publishing-a-cloud-service-without-changing-its-vip"></a>Julkaiseminen pilvipalveluun muuttamatta sen VIP

Pilvipalveluun VIP varataan, kun ensin käyttöönottoa Azure tietyssä ympäristössä, kuten tuotantoympäristössä. VIP ei muutu, paitsi jos poistat käyttöönoton erikseen tai se poistetaan implisiittisesti käyttöönoton Päivitysprosessista. Jos haluat säilyttää VIP, on ei poista käyttöönoton ja varmista myös, että Visual Studio ei poista käyttöönoton automaattisesti. Voit hallita toiminnan käyttöönoton asetusten määrittäminen **Ohjatun julkaisutoiminnon**, joka tukee useita asennusvaihtoehdot. Voit määrittää ajan tasalla käyttöönoton tai päivitä-käyttöönotosta, joka voi olla vaiheittainen tai samanaikaisesti, ja molemmat päivityksen ominaisuuksissa tiedostotyyppejä säilyttää VIP. Katso määritelmät käyttöönoton eri seuraavanlaisiin [Azure sovelluksen ohjatun julkaisutoiminnon](vs-azure-tools-publish-azure-application-wizard.md).  Lisäksi voit määrittää, onko pilvipalveluun edellisen käyttöönoton poistetaan, ilmenee virhe. VIP odottamatta saattavat muuttua, jos et määritä asetusta oikein.

### <a name="to-update-a-cloud-service-without-changing-its-vip"></a>Jos haluat päivittää pilvipalveluun muuttamatta sen VIP

1. Kun otat käyttöön oman pilvipalvelussa vähintään kerran, Avaa pikavalikon Azure projektin solmu ja valitse sitten **Julkaise**. Ohjattu **Julkaiseminen Azure-sovellus** tulee näkyviin.

1. Valitse tilaukset-luettelosta valita, johon haluat ottaa käyttöön ja valitse sitten **Seuraava** . Ohjatun toiminnon **asetukset** -sivu tulee näkyviin.

1. **Yleiset asetukset** -välilehden Tarkista, että nimi pilvipalvelussa, johon olet käyttöönotto- **ympäristössä**, **Muodosta määritys**ja **Palvelun määritykset** ovat kaikki oikein.

1. **Lisäasetukset** -välilehdessä Varmista, että tallennustilan tilin ja käyttöönoton otsikko ovat oikein, **Poista virheen käyttöönotto** -valintaruutu ole valittu ja että **käyttöönoton** päivitys-valintaruutu on valittuna. Valitsemalla **käyttöönoton** päivitys-valintaruudun varmistat, että käyttöönoton ei voi poistaa, ja että VIP ei menetetä, kun julkaiset sovelluksen. Valinnan **poistaminen käyttöönoton virhe-valintaruudun valinta**, voit varmistaa, että VIP ei menetetä virheen ilmetessä käyttöönoton aikana.

1. Voit määrittää tarkemmin siitä, miten haluat roolit voi päivittää, valitse **käyttöönoton update** -ruudun vieressä olevaa **asetukset** -linkki ja valitse sitten lisäävän päivityksen tai samanaikaisen päivityksen vaihtoehto **käyttöönoton update** asetukset-valintaikkunan. Jos valitset lisäävän päivityksen, jokaiselle esiintymälle on päivitetty yksitellen niin, että sovellus on aina käytettävissä. Jos valitset samanaikaisen päivityksen, kaikki esiintymät päivitetään samanaikaisesti. Samanaikainen päivittämisestä on nopeampaa, mutta palvelun eivät välttämättä ole käytettävissä päivityksen aikana.

1. Kun olet tehnyt asetuksia, valitse **Seuraava** -painiketta.

1. Valitse ohjatun toiminnon **Yhteenveto** -sivulla Tarkista asetukset ja valitse sitten **Julkaise** -painikkeen.

  >[AZURE.WARNING] Jos asennus epäonnistuu, olisi osoite miksi se epäonnistui ja ota uudelleen nopeasti, voit estää pilvipalvelussa sijaitsevaan vioittunut.

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja julkaisemisesta Azure Visual Studio on artikkelissa [ohjattuun julkaista Azure-sovelluksen](vs-azure-tools-publish-azure-application-wizard.md).

<properties 
    pageTitle="Azure RemoteApp - miten kaistanleveys ja laatu ilmene työn samassa paikassa? | Microsoft Azure"
    description="Katso, miten kaistanleveys Azure RemoteApp vaikuttaa käyttäjän käyttäjäkokemuksen laatuun."
    services="remoteapp"
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />

# <a name="azure-remoteapp---how-do-network-bandwidth-and-quality-of-experience-work-together"></a>Azure RemoteApp - miten kaistanleveys ja laatu ilmene työn samassa paikassa?

> [AZURE.IMPORTANT]
> Azure RemoteApp on on lopetettu. Lue lisätietoja [ilmoitus](https://go.microsoft.com/fwlink/?linkid=821148) .

Kun kyseessä on [Yleinen kaistanleveys](remoteapp-bandwidth.md) Azure RemoteApp vaaditaan, ota huomioon seuraavat seikat - näihin kuuluvat kaikki dynaaminen järjestelmä, joka vaikuttaa koko käyttäjäkokemus. 

- **Käytettävissä olevat kaistanleveys ja nykyisen Verkkoehdot** - parametrien (menetyksiltä, viive-synkronointivirheiden) tiettynä hetkenä samassa verkossa saattaa olla vaikutusta sovelluksen streaming kokemus, eli laskettu yleinen käyttökokemusta. Käytettävissä verkon kaistanleveyden on kuormituksen, satunnainen tappio, viive funktio, koska kaikki parametrit vaikuttavat kuormituksen ohjausobjektin, järjestelmä joka puolestaan ohjausobjektit nopeutta rajuille välttämiseksi.  Esimerkiksi epäluotettavia verkossa tai verkkoon, joiden odotusaika tekee käyttäjän toimia virheelliset myös verkossa, jossa 1 000 Mt kaistanleveyden. Tappio ja viive määräytyvät käyttäjämäärä, jotka ovat samassa verkossa ja mitä käyttäjät tekevät (esimerkiksi katsella videoita, lataamista suuria tiedostoja, tulostaminen).
- **Käyttö-skenaario** - kokemus määräytyy sen mukaan, mitä käyttäjät tekevät esimerkiksi ja ryhmänä samassa verkossa. Esimerkiksi luetaan yksi dia vaatii vain yhden kehys päivitetään; Jos käyttäjä skims ja vierittää tekstiasiakirjan sisällön päälle, ne on päivitettävä sekunnissa kehysten määrä. Viestintä ja tuotua tässä skenaariossa palvelimeen myöhemmin käyttää Lisää kaistanleveys. Kannattaa myös erittäin Esimerkki: useiden käyttäjien (esimerkiksi 4K selvitys) high definition videoiden katseleminen, pitämällä HD neuvottelupuheluita, toistaminen 3D video visualisointi: n tai CAD-järjestelmiä parissa. Kaikki nämä voi tehdä myös erittäin suuri kaistanleveyden verkon käytännössä käyttökelvoton.
- **Näyttötarkkuus ja sivujen määrä** - Lisää kaistanleveys on täysi päivitys isommaksi näyttöihin kuin tililuettelon. Pohjana oleva tekniikka onko koodaus ja lähettävän vain alueet, jotka on päivitetty näyttöjen melko hyvin, mutta silloin, koko näytön on päivitettävä. Kun käyttäjällä on suurempi ratkaisu-näytössä (esimerkiksi 4K selvitys), kyseinen päivitys edellyttää enemmän kuin näyttö, jossa pienempi tarkkuus (kuten 1024x768px) kaistanleveys. Tämä saman logiikka koskee, jos käytössäsi on useampi kuin yksi näyttö uudelleenohjausta varten. Kaistanleveys on suurenee sivujen määrä.
- **Leikepöydän ja laitteen uudelleenohjaus** – tämä on hyvin ilmeisimmät ongelma, mutta usein käyttäjä tallentaa suuri tietojoukko Leikepöydälle kestää vähän aikaa tietojen siirtämiseen etätyöpöydän asiakkaan palvelimeen. Edeltävät kokemus voi heiketä lähetyksen Leikepöydän sisällön edeltävät kokemuksen. Sama koskee laitteen uudelleenohjaus – Jos skannerista tai verkkokamera tuottaa paljon tietoja on lähetetään seuraavat palvelimeen tai tulostin on saatava suurien tiedostojen tai paikallinen tallennus on oltava käytettävissä sovelluksen käynnissä kopioi suurta tiedostoa pilveen, käyttäjä voi huomata kehysten hylkäämisen tai tilapäisesti "kiinnitetty" video koska laitteen uudelleenohjaus tarvittavien tietojen yhä kaistanleveys on. 

Kun tarkastelet verkon kaistanleveyden tarpeitasi, varmista, että kaikki toimi kuin järjestelmän seikoista.

Palaa [tärkeimmät verkon kaistanleveyden artikkelissa](remoteapp-bandwidth.md), tai siirry testaaminen että [kaistanleveys](remoteapp-bandwidthtests.md).

## <a name="learn-more"></a>Opi lisää
- [Azure RemoteApp verkon kaistanleveyden käytön arviointi](remoteapp-bandwidth.md)

- [Azure RemoteApp - testaaminen verkon kaistanleveyden käytön joitakin yleisiä tilanteita, joissa kanssa](remoteapp-bandwidthtests.md)

- [Azure RemoteApp-kaistanleveys - yleisiä ohjeita (Jos et voi testata oman)](remoteapp-bandwidthguidelines.md)
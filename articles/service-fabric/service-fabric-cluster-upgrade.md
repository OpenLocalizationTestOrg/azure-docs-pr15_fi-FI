<properties
   pageTitle="Päivitä Azure palvelun kangasta-klusterin | Microsoft Azure"
   description="Päivittää palvelun kangasta koodi ja/tai määritys, joka suoritetaan palvelun kangasta klusterin, mukaan lukien klusterin päivityksen tila päivittäminen varmenteet-sovelluksen porttien, tekevät OS korjaukset lisääminen ja niin edelleen. Mitä voit odottaa päivitykset suoritetaan?"
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/10/2016"
   ms.author="chackdan"/>


# <a name="upgrade-an-azure-service-fabric-cluster"></a>Azure-palvelu kangasta-klusterin päivittäminen

> [AZURE.SELECTOR]
- [Azure klusterin](service-fabric-cluster-upgrade.md)
- [Erillisestä klusterin](service-fabric-cluster-upgrade-windows-server.md)

Nykyaikainen järjestelmän upgradability suunnittelemisesta on saavuttamiseksi pitkään success tuotteen-näppäintä. Azure-palvelu kangasta-klusterin on omista, mutta Microsoft hallitsee osittain resurssi. Tässä artikkelissa kuvataan, mitä hallitaan automaattisesti ja mitä voit määrittää itse.

## <a name="controlling-the-fabric-version-that-runs-on-your-cluster"></a>Kangasta-versio, joka suoritetaan yhteyttä klusterin hallinta

Voit määrittää yhteyttä klusterin saavat kangasta Automaattiset päivitykset, kun Microsoft julkaisee uuden version tai valita haluamasi yhteyttä klusterin on tuettu kangasta-version.

Voit tehdä tämän "upgradeMode" klusterikokoonpanon määrittäminen portaalissa tai käyttämällä Resurssienhallinta luominen tai uudempi live klusterin aikaa 

>[AZURE.NOTE] Varmista, että voit pitää yhteyttä klusterin tuetut kangasta-versiota käyttävien aina. Kun ja kerromme palvelun kangasta uuden version julkaisemisen, aiemmasta versiosta on merkitty tuki loppuun aikaisintaan 60 päivän päivämäärän. uudet ovat ilmoitettu [-palvelun kangasta tiimin blogia](https://blogs.msdn.microsoft.com/azureservicefabric/ ). Uudessa versiossa on käytettävissä ja valitse sitten. 

14 päivää ennen yhteyttä klusterin on käynnissä, Vapauta voimassaolon kunto tapahtuma on luotu, joka siirtää yhteyttä klusterin varoitus kuntotietojen. Klusterin pysyy varoitus-tilaan, kunnes päivität tuetut kangasta-versioon.


### <a name="setting-the-upgrade-mode-via-portal"></a>Päivitystilan kautta portaalin määrittäminen 

Voit määrittää klusterin automaattinen tai Manuaalinen klusterin luotaessa.

![Create_Manualmode][Create_Manualmode]

Voit määrittää klusterin automaattinen tai Manuaalinen live klusterin, käytät hallinta-versiota. 

#### <a name="upgrading-to-a-new-version-on-a-cluster-that-is-set-to-manual-mode-via-portal"></a>Päivittäminen uuteen versioon klusterissa, joka on määritetty Manuaalinen tila portaalissa kautta.
 
Päivität uuteen versioon, sinun tarvitsee on käytettävissä versio valitsemalla avattavasta valikosta ja Tallenna. Kangasta päivitys saa käynnistynyt automaattisesti. Klusterin kuntokäytännöt (yhdistelmä solmu kuntotietojen ja kuntotietojen kaikki klusterin sovellusten) on noudattanut päivityksen aikana.

Jos klusterin kuntokäytännöt eivät täyty, päivitys on peruutettu. Vierittää alaspäin tämän asiakirjan Lue lisää siitä, miten voit määrittää mukautetun kyseiset kuntokäytännöt. 

Kun olet korjannut ongelmat, jotka ovat palauttamista, sinun täytyy Aloita päivitys uudelleen noudattamalla samoja vaiheita kuin ennen.

![Manage_Automaticmode][Manage_Automaticmode]

### <a name="setting-the-upgrade-mode-via-a-resource-manager-template"></a>Päivitystilan kautta Resurssienhallinta mallin määrittäminen 

"UpgradeMode"-määritysten lisääminen Microsoft.ServiceFabric/clusters resurssin määrityksen ja Aseta "clusterCodeVersion" jokin tuettu kangasta versioiden alla kuvatulla tavalla ja ottaa sitten mallin käyttöön. "UpgradeMode" kelvollinen arvot ovat "Manuaalinen" tai "Automaattinen"
 
![ARMUpgradeMode][ARMUpgradeMode]

#### <a name="upgrading-to-a-new-version-on-a-cluster-that-is-set-to-manual-mode-via-a-resource-manager-template"></a>Päivittäminen uuteen versioon klusterissa, joka on määritetty Manuaalinen tilaan kautta Resurssienhallinta mallia.
 
Klusterin ollessa manuaalinen tilassa päivittäminen uutta versiota, muuta "clusterCodeVersion" tuettu versio ja ota se käyttöön. Mallin käyttöönoton potkut ovat kiellettyjä kangasta päivityksen saa käynnistynyt automaattisesti. Klusterin kuntokäytännöt (yhdistelmä solmu kuntotietojen ja kuntotietojen kaikki klusterin sovellusten) on noudattanut päivityksen aikana.

Jos klusterin kuntokäytännöt eivät täyty, päivitys on peruutettu. Vierittää alaspäin tämän asiakirjan Lue lisää siitä, miten voit määrittää mukautetun kyseiset kuntokäytännöt. 

Kun olet korjannut ongelmat, jotka ovat palauttamista, sinun täytyy Aloita päivitys uudelleen noudattamalla samoja vaiheita kuin ennen.

### <a name="get-list-of-all-available-version-for-all-environments-for-a-given-subscription"></a>Saat kaikki käytettävissä olevat version luettelon kaikki tietyn tilauksen ympäristössä

Suorita seuraava komento ja pitäisi saada tulos tällainen.

"supportExpiryUtc" kertoo, että kun annetun release vanheneminen tai on vanhentunut. Uusin versio ei ole kelvollinen päivämäärä - arvo on "9999 – 12-31T23:59:59.9999999", mikä tarkoittaa juuri, erääntymispäivä ei ole vielä määritetty.

```REST
GET https://<endpoint>/subscriptions/{{subscriptionId}}/providers/Microsoft.ServiceFabric/clusterVersions?api-version= 2016-09-01

Output:
{
                  "value": [
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/5.0.1427.9490",
                      "name": "5.0.1427.9490",
                      "type": "Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "5.0.1427.9490",
                        "supportExpiryUtc": "2016-11-26T23:59:59.9999999",
                        "environment": "Windows"
                      }
                    },
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/4.0.1427.9490",
                      "name": "5.1.1427.9490",
                      "type": " Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "5.1.1427.9490",
                        "supportExpiryUtc": "9999-12-31T23:59:59.9999999",
                        "environment": "Windows"
                      }
                    },
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/4.4.1427.9490",
                      "name": "4.4.1427.9490",
                      "type": " Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "4.4.1427.9490",
                        "supportExpiryUtc": "9999-12-31T23:59:59.9999999",
                        "environment": "Linux"
                      }
                    }
                  ]
                }


```

## <a name="fabric-upgrade-behavior-when-the-cluster-upgrade-mode-is-automatic"></a>Päivityksen kangasta vaikutus klusterin päivityksen tila on automaattinen

Microsoft ylläpitää kangasta koodi ja määrityksistä, jotka suoritetaan Azure-klusterin. Olemme suorittaa Automaattiset valvottu päivitykset ohjelmiston tarpeen mukaan. Nämä päivitykset voi olla koodi ja määritykset. Varmista, että sovelluksesi kärsii ei ole vaikutusta tai mahdollisimman pieni vaikutus vuoksi nämä päivitykset, että seuraavat vaiheet suorittamalla päivitykset:

### <a name="phase-1-an-upgrade-is-performed-by-using-all-cluster-health-policies"></a>Vaihe 1: Päivityksen suoritetaan käyttämällä kaikki klusterin kuntokäytännöt

Tässä vaiheessa päivitysten jatkaa päivityksen toimialueen kerrallaan ja sovellukset, jotka olivat käynnissä klusterin edelleen toimimaan ilman mitään käyttökatkot. Klusterin kuntokäytännöt (yhdistelmä solmu kuntotietojen ja kuntotietojen kaikki klusterin sovellusten) on noudattanut päivityksen aikana.

Jos klusterin kuntokäytännöt eivät täyty, päivitys on peruutettu. Valitse Tilauksen omistaja lähetetään sähköpostitse. Sähköpostiviesti sisältää seuraavat tiedot:

- Ilmoitus, että on ollut palauttamaan klusterin päivitys.
- Ehdotetut korjaavat toimenpiteet, jos sellainen on.
- (N), kunnes on Suorita vaihe 2 päivien määrän.

Yritämme suorittaa saman päivityksen muutama kertaa siltä varalta, että kaikki päivitykset epäonnistui infrastruktuurin syistä. N päivä sähköpostiviesti on lähetetty päivämäärän jälkeen jatkamista vaiheessa 2.

Jos klusterin kuntokäytännöt täyty, päivitys pidettäviä onnistuneen ja suoritetuksi. Tämä voi johtua alkuperäinen päivitys tai jokin päivityksen uusinnoiksi aikana tämän vaiheen. Ei onnistunut Suorita sähköpostin ei vahvisteta. Näin voit välttää lähettämistä voit liian monta sähköpostia--sähköpostin vastaanottaminen pitäisi näkyä poikkeuksen normaaliksi. Odotamme useimmat onnistuu ilman vaikuttavat sovelluksen käytettävyyden klusterin-päivitykset.

### <a name="phase-2-an-upgrade-is-performed-by-using-default-health-policies-only"></a>Vaihe 2: Päivityksen suoritetaan käyttämällä kunto oletuskäytäntöjä vain

Tässä vaiheessa kunto-käytäntöjä on määritetty siten, että sovellukset, jotka olivat kunnossa päivitys alussa määrä säilyy ennallaan päivitysprosessin ajaksi. Vaihe 1, kuten vaiheessa 2-päivitysten jatkaa päivityksen toimialueen kerrallaan ja sovellukset, jotka olivat käynnissä klusterin edelleen toimimaan ilman mitään käyttökatkot. Klusterin kuntokäytännöt (yhdistelmä solmu kuntotietojen ja kuntotietojen kaikki klusterin sovellusten) on noudattanut päivityksen ajaksi.

Jos klusterin kuntokäytännöt voimassa ei täyty, päivitys on peruutettu. Valitse Tilauksen omistaja lähetetään sähköpostitse. Sähköpostiviesti sisältää seuraavat tiedot:

- Ilmoitus, että on ollut palauttamaan klusterin päivitys.
- Ehdotetut korjaavat toimenpiteet, jos sellainen on.
- (N), kunnes on suorittaa vaiheessa 3 päivien määrän.

Yritämme suorittaa saman päivityksen muutama kertaa siltä varalta, että kaikki päivitykset epäonnistui infrastruktuurin syistä. Muistutus-sähköposti lähetetään päivää ennen n päivä on muutama. N päivä sähköpostiviesti on lähetetty päivämäärän jälkeen jatkamista vaiheessa 3. Lähetämme sinulle vaihe 2: n sähköpostit on otettava vakavasti ja korjaavat toimenpiteet on otettava.

Jos klusterin kuntokäytännöt täyty, päivitys pidettäviä onnistuneen ja suoritetuksi. Tämä voi johtua alkuperäinen päivitys tai jokin päivityksen uusinnoiksi aikana tämän vaiheen. Ei onnistunut Suorita sähköpostin ei vahvisteta.

### <a name="phase-3-an-upgrade-is-performed-by-using-aggressive-health-policies"></a>Vaihe 3: Päivityksen suoritetaan käyttämällä kohdetoimialueen kuntokäytännöt

Tässä vaiheessa kuntokäytännöt suunnattu päivitystä sovellukset kunto sijaan. Vain muutamia klusteri päivityksistä Lopeta tämän vaiheen. Jos yhteyttä klusterin saa tämän vaiheen, on hyvinkin sovelluksen tulee perusasemassa tai menettää käytettävyyttä.

Samoin kuin muut kaksi vaihetta, vaiheessa 3 päivitykset nyt yksi päivityksen toimialue kerrallaan.

Jos klusterin kuntokäytännöt eivät täyty, päivitys on peruutettu. Yritämme suorittaa saman päivityksen muutama kertaa siltä varalta, että kaikki päivitykset epäonnistui infrastruktuurin syistä. Tämän jälkeen klusterin on kiinnitetty, niin, että eivät ole enää saatavilla tukea ja päivityksiä.

Sähköpostiviesti, jossa tiedot lähetetään tilauksen omistaja, sekä korjaavat toimenpiteet. Odotamme ei mitään klustereiden saat vaiheeseen, jossa vaiheessa 3 epäonnistui.

Jos klusterin kuntokäytännöt täyty, päivitys pidettäviä onnistuneen ja suoritetuksi. Tämä voi johtua alkuperäinen päivitys tai jokin päivityksen uusinnoiksi aikana tämän vaiheen. Ei onnistunut Suorita sähköpostin ei vahvisteta.

## <a name="cluster-configurations-that-you-control"></a>Klusterimääritykset, voit hallita

Lisäksi mahdollisuus määrittää klusterin päivityksen tila, seuraavat määritykset, joita voit muuttaa live klusterissa.

### <a name="certificates"></a>Varmenteet

Voit lisätä uusia tai poistaa klusterin ja asiakasohjelman kautta portaalin varmenteet helposti. Lisätietoja [Saat yksityiskohtaiset ohjeet tähän](service-fabric-cluster-security-update-certs-azure.md) asiakirjaan

![Näyttökuva, jossa näkyy varmenteen thumbprints Azure-portaalissa.][CertificateUpgrade]


### <a name="application-ports"></a>Sovelluksen portit

Voit muuttaa sovelluksen portit muuttamalla kuormituksen resurssin ominaisuuksia, jotka liittyvät solmun tyyppiä. Voit käyttää portaalin tai voit käyttää Resurssienhallinta PowerShell suoraan.

Voit avata uuden porttiin kaikki VMs solmutyyppi, toimi seuraavasti:

1. Lisää uusi näytteenottimen tarvittavat kuormituksen.

    Jos yhteyttä klusterin käyttämät-portaalissa, kuormituksen tasoitusmääritykset nimetään "Kg name resurssien ryhmä-NodeTypename", yksi solmu mistäkin. Koska kuormituksen tasauspalvelun nimet ovat vain resurssiryhmä yksilöivä, on parasta Jos etsit niitä tiettyjen resurssiryhmä-kohdassa.

    ![Näyttökuva, jossa näytteenottimen lisääminen kuormituksen portaalissa.][AddingProbes]

2. Lisää uusi sääntö kuormituksen.

    Lisätä uuden säännön samaan kuormituksen käyttämällä keräysputken, jonka loit edellisessä vaiheessa.

    ![Uuden säännön lisääminen kuormituksen portaalissa.][AddingLBRules]


### <a name="placement-properties"></a>Sijainti-ominaisuudet

Kunkin solmutyypit voit lisätä mukautetun asettelun ominaisuudet, joita haluat käyttää sovelluksia. NodeType on oletusarvo-ominaisuus, jota voit käyttää lisäämättä sitä erikseen.

>[AZURE.NOTE] Lisätietoja asettelun rajoitukset, solmun ominaisuudet ja miten ne määritetään viittaa "Sijainnin rajoitukset ja solmujen ominaisuudet" [Kuvaava Your klusterin](service-fabric-cluster-resource-manager-cluster-description.md)palvelun kangasta klusterin Resurssienhallinta-asiakirjassa.

### <a name="capacity-metrics"></a>Kapasiteetin arvot

Kunkin solmutyypit voit lisätä mukautetun kapasiteetin arvot, jotka haluat käyttää raportin Lataa sovellukset. Lisätietoja raportin mittarit kapasiteetin käyttö ladata, voit tarkistaa [Kuvaava Your klusterin](service-fabric-cluster-resource-manager-cluster-description.md) ja [arvot ja lataa](service-fabric-cluster-resource-manager-metrics.md)palvelun kangasta klusterin Resurssienhallinta-tiedostoja.

### <a name="fabric-upgrade-settings---health-polices"></a>Kangasta päivitysasetukset - kunto käytännöt

Voit määrittää mukautetun kunto käytännöt kangasta päivitystä varten. Jos olet määrittänyt yhteyttä klusterin kangasta Automaattiset päivitykset, kyseiset käytännöt Hae käyttöön vaiheen 1 kangasta automaattisten päivitysten.
Jos olet määrittänyt päivityksiä varten manuaalinen kangasta yhteyttä klusterin, kyseiset käytännöt Hae käyttöön aina, kun valitset järjestelmän käyttö kangasta päivitys yhteyttä klusterin käynnistävä uusi versio. Jos Ohita käytäntöjä, oletusarvoja käytetään.

Voit määrittää mukautetun kuntokäytännöt tai tarkastella nykyisiä asetuksia, valitse "kangasta päivitys"-sivu valitsemalla Lisäasetukset päivitysasetukset. Lue ohjeet seuraavassa kuvassa. 

![Mukautetun kunto käytäntöjen hallinta][HealthPolices]

### <a name="customize-fabric-settings-for-your-cluster"></a>Mukauta yhteyttä klusterin kangasta asetukset

Lisätietoja on ohjeaiheessa [kangasta klusterin kangasta Palveluasetukset](service-fabric-cluster-fabric-settings.md) mitä ja miten niitä voi mukauttaa.

### <a name="os-patches-on-the-vms-that-make-up-the-cluster"></a>OS korjaukset, jotka muodostavat klusterin VMs

Tämä ominaisuus on suunniteltu tulevaisuudessa automaattinen toiminto. Mutta tällä hetkellä vastaat korjaustiedoston oman VMs. Toimi tässä yhden AM kerrallaan, niin, että et tee alaspäin enemmän kuin yksi kerrallaan.

### <a name="os-upgrades-on-the-vms-that-make-up-the-cluster"></a>OS päivitykset, jotka muodostavat klusterin VMs

Jos sinun on päivitettävä klusterin näennäiskoneiden OS-kuva, sinun täytyy tehdä se AM yksi kerrallaan. Olet vastuussa päivitykseen – tällä hetkellä ei automation tämän.

## <a name="next-steps"></a>Seuraavat vaiheet
- Opi mukauttamaan osa [kangasta klusterin kangasta Palveluasetukset](service-fabric-cluster-fabric-settings.md)
- Lue, miten [yhteyttä klusterin sisään ja ulos](service-fabric-cluster-scale-up-down.md)
- Lue lisää [sovellus-päivitykset](service-fabric-application-upgrade.md)

<!--Image references-->
[CertificateUpgrade]: ./media/service-fabric-cluster-upgrade/CertificateUpgrade2.png
[AddingProbes]: ./media/service-fabric-cluster-upgrade/addingProbes2.PNG
[AddingLBRules]: ./media/service-fabric-cluster-upgrade/addingLBRules.png
[HealthPolices]: ./media/service-fabric-cluster-upgrade/Manage_AutomodeWadvSettings.PNG
[ARMUpgradeMode]: ./media/service-fabric-cluster-upgrade/ARMUpgradeMode.PNG
[Create_Manualmode]: ./media/service-fabric-cluster-upgrade/Create_Manualmode.PNG
[Manage_Automaticmode]: ./media/service-fabric-cluster-upgrade/Manage_Automaticmode.PNG
<properties
    pageTitle="Luo ja siirtää:: HSM suojattu näppäimet Azure avaimen säilö | Microsoft Azure"
    description="Tämän artikkelin avulla voit suunnitella, Luo ja Siirrä oman:: HSM suojattu käytettävät näppäimet Azure avaimen säilö."
    services="key-vault"
    documentationCenter=""
    authors="cabailey"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="cabailey"/>
#<a name="how-to-generate-and-transfer-hsm-protected-keys-for-azure-key-vault"></a>Luo ja siirtää:: HSM suojattu näppäimet Azure avaimen säilö

##<a name="introduction"></a>Johdanto

Lisätty assurance käytettäessä Azure avaimen säilö, voit tuoda tai luo näppäimet moduuleissa laitteiston security (HSMs), jätä koskaan:: HSM reunaa. Tässä skenaariossa kutsutaan usein *Tuo oma avain*tai BYOK. HSMs ovat FIPS 140-2 tason 2 vahvistettu. Azure avaimen säilö käyttää HSMs Thales nShield tuoteperheen suojaaminen avaimien.

Tämän artikkelin tietojen avulla voit auttaa suunnitteleminen, luominen ja Siirrä oman:: HSM suojattu käytettävät näppäimet Azure avaimen säilö.

Tämä toiminto ei ole käytettävissä Azure Kiinan. 

>[AZURE.NOTE] Saat lisätietoja Azure avaimen säilö [Azure avaimen säilö ominaisuudet?](key-vault-whatis.md)  
>
>Hakeminen aloittaminen opetusohjelmaan, joka sisältää luominen avaimen säilö:: HSM suojattu näppäimet varten, katso [Azure avaimen säilö käytön aloittaminen](key-vault-get-started.md).

Lisätietoja luodaan ja siirretään:: HSM suojatun avaimen Internetin kautta:

- Voit luoda avaimen offline-tilassa työasema, mikä vähentää uhanalaisten.

- Avain salataan kanssa Key Exchange Key (KEK), joka pysyy salattuja, kunnes se siirretään Azure avaimen säilö HSMs. Vain salattuja versio key-tuotetunnuksen jättää alkuperäisen työaseman.

- Työkaluryhmä määrittää ominaisuudet vuokraajan avainta, joka sitoo key-tuotetunnuksen Azure avaimen säilö suojauksen maailmaa. Niin kun Azure avaimen säilö HSMs vastaanottaminen ja salauksen key-tuotetunnuksen, nämä HSMs käyttää sitä. Key-tuotetunnuksen ei voi viedä. Tämän sidonnan valvoo Thales HSMs.

- -Avain Exchange Key (KEK), jota käytetään salaamaan key-tuotetunnuksen luodaan sisällä Azure avaimen säilö HSMs ja ei voi viedä. HSMs viite-että ulkopuolella HSMs KEK versiota ei ole Tyhjennä voi olla. Lisäksi työkaluryhmä sisältää todistus-Thales KEK ei voi viedä ja luotiin aito:: HSM, joka on valmistanut Thales sisällä.

- Työkaluryhmä sisältää todistus-että Azure avaimen säilö suojauksen maailman on myös luotu aito:: HSM valmistanut Thales Thales. Tämä todistus todistaa haluat, että Microsoft käyttää aito laitteiston.

- Microsoft käyttää erillisen KEKs ja erota suojauksen maailman kunkin maantieteellisen alueen. Tämä erottaminen varmistaa, että key-tuotetunnuksen voi käyttää vain tiedot keskikohdan mukaan-alue, jossa on salattu se. Esimerkiksi Euroopan asiakkaan näppäintä ei voi käyttää tietoja keskikohdan mukaan-Pohjois-Amerikan ja Aasian.

##<a name="more-information-about-thales-hsms-and-microsoft-services"></a>Lisätietoja Thales HSMs ja Microsoft-palveluista

Thales e-suojaus on salauksen ja cyber security-ratkaisujen palvelujen, korkean teknologian, valmistus, government ja tietotekniikan yleinen johtavan. 40 vuotta Jäljitä tietueen suojaaminen yrityksen ja government tiedot Thales ratkaisuja käytetään neljä viiden suurimman energiajärjestelmien ja maanpuolustusteollisuudessa yhtiöiden. Ehdotetaan niihin ratkaisuja käytetään myös 22 NATON maat, ja sen suojaamista yli 80 prosenttia maailmanlaajuisesti maksu tapahtumien.

Microsoft on yhdessä Thales tehostavat kuva tilan HSMs kanssa. Nämä parannukset mahdollistavat tyypillinen edut isännöityä palveluja ilman avaimien voida hallintaoikeutta. Tarkemmin sanottuna nämä parannukset avulla Microsoft hallinta HSMs niin, että sinun ei tarvitse. Kuin pilvipalveluun Azure avaimen säilö Skaalaa on lyhyt ilmoitus organisaation käyttö piikkarit mukaiseksi. Samanaikaisesti, key-tuotetunnuksen suojataan sisällä Microsoftin HSMs: avaimen elinkaari hallinnan säilyttäminen, koska Luo avain ja siirrä se Microsoftin HSMs.

##<a name="implementing-bring-your-own-key-byok-for-azure-key-vault"></a>Käyttöönoton Tuo oma avain (BYOK) varten Azure avaimen säilö

Käytä seuraavia tietoja ja menetelmiä, jos Luo oma:: HSM suojattu avain ja se siirretään Azure avaimen säilö, Näytä key (BYOK)-vaihtoehto.


##<a name="prerequisites-for-byok"></a>BYOK edellytyksistä

Seuraavassa taulukossa on luettelo edellytyksistä tuoda Omat key (BYOK) Azure avaimen säilö, katso.

|Vaatimus|Lisätietoja|
|---|---|
|Azure-tilausta|Luo Azure-avain säilöön, sinun on Azure tilaus: [varten maksuttoman kokeiluversion käyttäjäksi rekisteröityminen](https://azure.microsoft.com/pricing/free-trial/)|
|Azure avaimen säilö Premium palvelutaso tukemaan:: HSM suojattu näppäimet|Palvelutasot ja ominaisuuksista Azure avaimen säilö varten, katso lisätietoja [Azure avaimen säilö hinnoittelu](https://azure.microsoft.com/pricing/details/key-vault/) -sivustossa.|
|Thales:: HSM, smartcards ja tukiohjelmisto|Tarvitset Thales laitteiston suojaus-moduulin ja basic toiminnallisia tuntemus Thales HSMs käyttöoikeudet. Saat luettelon yhteensopiva mallien tai ostaa:: HSM, jos sinulla ei ole yhtä [Thales laitteiston suojauksen moduuli](https://www.thales-esecurity.com/msrms/buy) .|
|Seuraavat laitteet ja ohjelmat:<ol><li>Offline-tilassa x64 työasema on vähintään Windows 7 ja Thales nShield-ohjelmiston, joka on vähintään Windows toiminnon järjestelmän versio 11.50.<br/><br/>Tämän työaseman käytetään Windows 7: ssä, sinun täytyy [asentaa Microsoft .NET Framework 4.5](http://download.microsoft.com/download/b/a/4/ba4a7e71-2906-4b2d-a0e1-80cf16844f5f/dotnetfx45_full_x86_x64.exe).</li><li>Työasema, joka on yhteydessä Internetiin ja on vähintään Windows toiminnon järjestelmän Windows 7: n.</li><li>USB-asemassa tai muita kannettavat tallennustilan laitteissa, joissa on vähintään 16 Mt vapaata tilaa.</li></ol>|Tietoturvasyistä Suosittelemme ensimmäisen työasema ei ole yhteydessä verkkoon. Kuitenkin tämä suositus ei ohjelmallisesti säilytetä.<br/><br/>Huomaa, että ohjeisiin, jotka noudattavat, tämän työaseman kutsutaan yhteys katkaistu työaseman.</p></blockquote><br/>Lisäksi jos vuokraajan key-tuotetunnuksen tuotannon verkossa, on suositeltavaa, että käytät toisen, erillisen työasemassa työkaluryhmä Lataa ja lataa vuokraajan-näppäintä. Mutta testausta varten, voit käyttää samaa työaseman ensimmäisen vaiheen.<br/><br/>Huomaa, että ohjeisiin, jotka noudattavat, tämän toisen työaseman kutsutaan työaseman Internet-yhteys.</p></blockquote><br/>|

##<a name="generate-and-transfer-your-key-to-azure-key-vault-hsm"></a>Luo ja Siirrä key-tuotetunnuksen Azure avaimen säilö:: HSM

Käytät Luo ja Siirrä key-tuotetunnuksen Azure-avain säilö:: HSM viisi seuraavasti:

- [Vaihe 1: Valmisteleminen työasemaan Internet-yhteys](#step-1-prepare-your-internet-connected-workstation)
- [Vaihe 2: Valmistele yhteys katkaistu työasemassa](#step-2-prepare-your-disconnected-workstation)
- [Vaihe 3: Luo key-tuotetunnuksen](#step-3-generate-your-key)
- [Vaihe 4: Valmisteleminen key-tuotetunnuksen siirtoa varten](#step-4-prepare-your-key-for-transfer)
- [Vaihe 5: Siirrä key-tuotetunnuksen Azure avaimen säilö](#step-5-transfer-your-key-to-azure-key-vault)

## <a name="step-1-prepare-your-internet-connected-workstation"></a>Vaihe 1: Valmisteleminen työasemaan Internet-yhteys
Toimi seuraavasti, joka on yhteydessä Internetiin työasemalle tätä ensimmäistä vaihetta.


###<a name="step-11-install-azure-powershell"></a>Vaihe 1.1: Azure PowerShellin asentaminen

Internet-yhteys, työasema lataa ja asenna PowerShellin Azure-moduuli, joka sisältää cmdlet-komentojen Azure avaimen säilö. Tämä edellyttää vähintään versiota 0.8.13.

Asennusohjeet Katso, [miten voit asentaa ja määrittää PowerShellin Azure](../powershell-install-configure.md).

###<a name="step-12-get-your-azure-subscription-id"></a>Vaihe 1.2: Azure tilaus-tunnuksen hankkiminen

PowerShellin Azure-istunnon aloittaminen ja kirjaudu Azure-tili käyttämällä seuraava komento:

        Add-AzureAccount
Kirjoita ponnahdusikkunoiden selainikkunassa Azure tilin käyttäjänimi ja salasana. Käyttämällä [Hae AzureSubscription](https://msdn.microsoft.com/library/azure/dn790366.aspx) -komentoa:

        Get-AzureSubscription
Etsi tulosteesta, voit käyttää Azure avaimen säilö tilauksen tunnus. Tarvitset tätä Tilaustunnus myöhemmässä vaiheessa.

Sulje PowerShellin Azure-ikkunaa.

###<a name="step-13-download-the-byok-toolset-for-azure-key-vault"></a>Vaihe 1.3: Lataa BYOK työkaluryhmä Azure avaimen säilö

Siirry Microsoft Download Centeristä ja [Lataa Azure avaimen säilö BYOK työkaluryhmä](http://www.microsoft.com/download/details.aspx?id=45345) maantieteellisen alueen tai Azure esiintymä. Käytä tunnistamisessa paketin lataaminen ja sen vastaavan SHA-256 paketti-hash nimi seuraavat tiedot:

---

**Pohjois-Amerikka:**

KeyVault-BYOK-Työkalut – Yhdistynyt States.zip

305F44A78FEB750D1D478F6A0C345B097CD5551003302FA465C73D9497AB4A03

---

**Eurooppa:**

KeyVault-BYOK-Työkalut-Europe.zip

C73BB0628B91471CA7F9ADFCE247561C6016A5103EF1A315D49C3EA23AFC0B9C

---

**Aasian:**

KeyVault-BYOK-Työkalut-AsiaPacific.zip

BE9A84B6C76661929F9FDAD627005D892B3B8F9F19F351220BB4F9C356694192

---

**Latinalainen Amerikka:**

KeyVault-BYOK-Työkalut-LatinAmerica.zip
    
9E8EE11972DECE8F05CD898AF64C070C375B387CED716FDCB788544AE27D3D23

---

**Japani:**

KeyVault-BYOK-Työkalut-Japan.zip

E6B88C111D972A02ABA3325F8969C4E36FD7565C467E9D7107635E3DDA11A8B2

---

**Australia:**

KeyVault-BYOK-Työkalut-Australia.zip

7660D7A675506737857B14F527232BE51DC269746590A4E5AB7D50EDD220675D

---

[**Azure Government:**](https://azure.microsoft.com/features/gov/)

KeyVault-BYOK-Työkalut-USGovCloud.zip

53801A3043B0F8B4A50E8DC01A935C2BFE61F94EE027445B65C52C1ACC2B5E80

---

**Kanada:**

KeyVault-BYOK-Työkalut-Canada.zip

A42D9407B490E97693F8A5FA6B60DC1B06B1D1516EDAE7C9A71AA13E12CF1345

---

**Saksa:**

KeyVault-BYOK-Työkalut-Germany.zip

4795DA855E027B2CA8A2FF1E7AE6F03F772836C7255AFC68E576410BDD28B48E

---
**Intia:**

KeyVault-BYOK-Työkalut-India.zip

26853511EB767A33CF6CD880E78588E9BBE04E619B17FBC77A6B00A5111E800C

---

Vahvista oman ladatut BYOK toolset, PowerShellin Azure-istunnossa eheyden [Get-FileHash](https://technet.microsoft.com/library/dn520872.aspx) cmdlet-komennon avulla

    Get-FileHash KeyVault-BYOK-Tools-*.zip

Työkaluryhmä sisältää seuraavat:

- Avaimen Exchange Key (KEK)-paketti, joka on nimi alkaa sanalla **BYOK-KEK - pkg-.**
- Suojauksen maailman-paketti, joka on nimi alkaa sanalla **BYOK-SecurityWorld - pkg-.**
- Nimetty v python komentosarjan**erifykeypackage.py.**
- Komentorivin suoritettavan tiedoston nimetyn **KeyTransferRemote.exe** ja liittyvät dll.
- Visual C++ Redistributable paketin, jonka nimi on **vcredist_x64.exe.**

Kopioi paketin USB-asemassa tai muita kannettavat tallennustilan.

##<a name="step-2-prepare-your-disconnected-workstation"></a>Vaihe 2: Valmistele yhteys katkaistu työasemassa

Tee tämä toinen vaihe työasemaan, joka ei ole yhteydessä verkkoon (Internetiin tai sisäisen verkon) seuraavasti.


###<a name="step-21-prepare-the-disconnected-workstation-with-thales-hsm"></a>Vaihe 2.1: Valmisteleminen yhteys katkaistu työasema on Thales:: HSM

NCipher (Thales) tuki-ohjelmiston asentaminen Windows-tietokoneessa ja liitä sitten Thales::-HSM tietokoneeseen.

Varmistaa, että Thales Työkalut polusta (**%nfast_home%\bin** ja **%nfast_home%\python\bin**). Kirjoita esimerkiksi seuraavasti:

        set PATH=%PATH%;”%nfast_home%\bin”;”%nfast_home%\python\bin”

Lisätietoja on artikkelissa Thales:: HSM mukana käyttöoppaassa.

###<a name="step-22-install-the-byok-toolset-on-the-disconnected-workstation"></a>Vaihe 2.2: Asentaminen BYOK työkaluryhmä yhteys katkaistu työaseman

Kopioi BYOK toolset paketin USB-asemassa tai muita kannettavat tallennustilan ja toimi sitten seuraavasti:

1. Pura tiedostot ladattu paketista muihin kansioihin.
2. Suorita vcredist_x64.exe kyseiseen kansioon.
3. Noudata asennuksen Visual C++ Runtimen osien Visual Studio 2013.

##<a name="step-3-generate-your-key"></a>Vaihe 3: Luo key-tuotetunnuksen

Tee tämä kolmannessa vaiheessa yhteys katkaistu työasemaan seuraavasti.

###<a name="step-31-create-a-security-world"></a>Vaihe 3.1: Luo suojauksen Maailman

Avaa komentokehote ja suorita ohjelma Thales uuden maailmaa.

    new-world.exe --initialize --cipher-suite=DLf1024s160mRijndael --module=1 --acs-quorum=2/3

Ohjelma luo **Suojauksen maailman** tiedoston % NFAST_KMDATA%\local\world, joka vastaa C:\ProgramData\nCipher\Key hallinta a-kansioon. Voit käyttää eri arvoja vuokaavio, mutta tässä esimerkissä voit pyytää sinua kirjoittamaan kolmen tyhjän korttien ja PIN jokaiselle. Sitten minkä tahansa korttien antaa suojauksen maailman täydet oikeudet. Korteissa muuttuvat **Järjestelmänvalvoja kortin määrittää** uuden suojauksen maailmalle.

Toimi sitten seuraavasti:

- Varmuuskopioi maailman-tiedosto. Suojatun ja maailman tiedoston, järjestelmänvalvojan kortit sekä niiden PIN ja varmista, että ei ole yksi henkilö on enemmän kuin yhden kortin käyttöoikeus.

###<a name="step-32-validate-the-downloaded-package"></a>Vaihe 3,2: Vahvista ladatut pakkaaminen

Tämä vaihe on valinnainen, mutta suositeltava niin, että voit tarkistaa seuraavasti:

- Avaimen Exchange-näppäintä, joka sisältyy työkaluryhmä on luotu-aito Thales::-HSM.
- Suojaus, joka sisältyy työkaluryhmä maailman hash on luotu genuine Thales::-HSM.
- Avaimen Exchange-avain on ei voi viedä.

>[AZURE.NOTE]Vahvistamiseen ladatut paketti:: HSM on yhdistettävä, virta ja on oltava sen suojauksen maailman (kuten juuri luomasi).

Vahvista ladatun paketin:

1.  Suorita verifykeypackage.py komentosarja sitominen jokin maantieteellisen alueen tai Azure esiintymä mukaan seuraavasti:
    - Pohjois-Amerikassa:

            python verifykeypackage.py -k BYOK-KEK-pkg-NA-1 -w BYOK-SecurityWorld-pkg-NA-1
    - Eurooppa:

            python verifykeypackage.py -k BYOK-KEK-pkg-EU-1 -w BYOK-SecurityWorld-pkg-EU-1
    - Saat Aasian:

            python verifykeypackage.py -k BYOK-KEK-pkg-AP-1 -w BYOK-SecurityWorld-pkg-AP-1
    - Latinalainen Amerikka: varten

            python verifykeypackage.py -k BYOK-KEK-pkg-LATAM-1 -w BYOK-SecurityWorld-pkg-LATAM-1
    - Japani:

            python verifykeypackage.py -k BYOK-KEK-pkg-JPN-1 -w BYOK-SecurityWorld-pkg-JPN-1
    - Australia:

            python verifykeypackage.py -k BYOK-KEK-pkg-AUS-1 -w BYOK-SecurityWorld-pkg-AUS-1
    - [Azure Government](https://azure.microsoft.com/features/gov/), joka käyttää Azure US government-esiintymä:

            python verifykeypackage.py -k BYOK-KEK-pkg-USGOV-1 -w BYOK-SecurityWorld-pkg-USGOV-1
    - Kanadan:

            python verifykeypackage.py -k BYOK-KEK-pkg-CANADA-1 -w BYOK-SecurityWorld-pkg-CANADA-1
    - Saksa:

            python verifykeypackage.py -k BYOK-KEK-pkg-GERMANY-1 -w BYOK-SecurityWorld-pkg-GERMANY-1
    - Intia: varten

            python verifykeypackage.py -k BYOK-KEK-pkg-INDIA-1 -w BYOK-SecurityWorld-pkg-INDIA-1
    >[AZURE.TIP]Thales ohjelmisto sisältää python %NFAST_HOME%\python\bin etsiminen

2.  Varmista, että näet seuraavat tiedot, joka ilmaisee onnistuneen vahvistuksen: **tulos: SUCCESS**

Tämä komentosarja vahvistaa allekirjoittaja ketju ylöspäin Thales pääkansio-näppäintä. Pääkansio avaimeen hash on upotettu komentosarja ja sen arvon pitäisi olla **59178a47 de508c3f 291277ee 184f46c4 f1d9c639**. Voit myös varmistaa arvoksi erikseen ohjesisältöä [Thales sivuston](http://www.thalesesec.com/).

Voit nyt luoda uuden tunnuksen.

###<a name="step-33-create-a-new-key"></a>Vaihe 3.3: Luo uusi avain

Luo avain Thales **generatekey** -ohjelman avulla.

Suorita seuraava komento avaimen luomiseen:

    generatekey --generate simple type=RSA size=2048 protect=module ident=contosokey plainname=contosokey nvram=no pubexp=

Kun suoritat tämän komennon, käytä seuraavia ohjeita:

- Parametrin *suojaaminen* on määritettävä arvo- **moduulin**esitetyllä. Tämä luo moduuli suojatun-näppäintä. BYOK työkaluryhmä ei tue OCS suojattu avaimet.

- Korvaa *contosokey* **ident** ja **plainname** arvo tahansa merkkijonoarvo. Voit pienentää ylimääräiset hallinnolliset ja vähentää virheitä, on suositeltavaa, että käytät samoja arvoja molemmille. **Ident** arvon on oltava vain lukuja, viivojen ja pienet kirjaimet.

- Pubexp jäljellä on tyhjä (oletusarvo) tässä esimerkissä, mutta voit määrittää määritetyistä arvoista. Lisätietoja on Thales ohjeissa.

Tämä komento luo Tokenized avain tiedoston %NFAST_KMDATA%\local kansiossa **key_simple_**, **ident** , joka on määritetty komennon perään alkaen nimellä. Esimerkki: **key_simple_contosokey**. Tiedoston nimi sisältää salatun avaimen.

Varmuuskopioi Tokenized avain tiedoston turvalliseen paikkaan.

>[AZURE.IMPORTANT] Kun siirrät key-tuotetunnuksen myöhemmin Azure avaimen säilö, Microsoft ei voi viedä takaisin avaimeen-kohtaa niin on erittäin tärkeää varmuuskopioida avain ja suojaus maailman turvallisesti. Pyydä Thales ohjeet ja parhaat käytännöt key-tuotetunnuksen varmuuskopioiminen.

Olet nyt valmis siirtämään key-tuotetunnuksen Azure avaimen säilö.

##<a name="step-4-prepare-your-key-for-transfer"></a>Vaihe 4: Valmisteleminen key-tuotetunnuksen siirtoa varten

Tee tämä Neljäs vaihe, yhteys katkaistu työasemaan seuraavasti.

###<a name="step-41-create-a-copy-of-your-key-with-reduced-permissions"></a>Vaihe 4.1: Key-tuotetunnuksen kopion luominen rajoitetun käyttöoikeuksien avulla

Komentoriviltä Key-tuotetunnus, oikeudet pienentämiseksi suorittamalla jompikumpi maantieteellisen alueen tai Azure esiintymä mukaan seuraavasti:

- Pohjois-Amerikassa:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-NA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-NA-1
- Eurooppa:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-EU-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-EU-1
- Saat Aasian:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AP-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AP-1
- Latinalainen Amerikka: varten

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-LATAM-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-LATAM-1
- Japani:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-JPN-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-JPN-1
- Australia:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AUS-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AUS-1
- [Azure Government](https://azure.microsoft.com/features/gov/), joka käyttää Azure US government-esiintymä:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USGOV-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USGOV-1
- Kanadan:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-CANADA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-CANADA-1
- Saksa:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-GERMANY-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-GERMANY-1
- Intia: varten

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-INDIA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-INDIA-1


Kun suoritat tämän komennon, korvaa *contosokey* määrittämäsi saman arvon **Vaihe 3.3: Luo uusi avain** vaiheesta, [Luo key-tuotetunnuksen](#step-3-generate-your-key) .

Sinua pyydetään suojauksen maailman järjestelmänvalvojan korttien laajennus.

Kun komento on valmis, näet **tulos: SUCCESS** ja nimeltä key_xferacId_ tiedostossa on kopio, key-tuotetunnuksen rajoitetun oikeuksilla<contosokey>.

###<a name="step-42-inspect-the-new-copy-of-the-key"></a>Vaihe 4.2: Tutkia sovelluksen uusi kopio avain

Voit myös Suorita Thales apuohjelmia, Vahvista uusi avain mahdollisimman vähän käyttöoikeuksia:

- aclprint.PY:

        "%nfast_home%\bin\preload.exe" -m 1 -A xferacld -K contosokey "%nfast_home%\python\bin\python" "%nfast_home%\python\examples\aclprint.py"
- kmfile-dump.exe:

        "%nfast_home%\bin\kmfile-dump.exe" "%NFAST_KMDATA%\local\key_xferacld_contosokey"
Kun suoritat näitä komentoja, korvaa contosokey määrittämäsi saman arvon **Vaihe 3.3: Luo uusi avain** vaiheesta, [Luo key-tuotetunnuksen](#step-3-generate-your-key) .

###<a name="step-43-encrypt-your-key-by-using-microsofts-key-exchange-key"></a>Vaihe 4.3: Salaaminen key-tuotetunnuksen Microsoftin avain Exchange avaimen avulla

Suorita jompikumpi seuraavista komennoista maantieteellisen alueen tai Azure esiintymä mukaan:

- Pohjois-Amerikassa:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-NA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-NA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Eurooppa:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-EU-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-EU-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Saat Aasian:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AP-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AP-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Latinalainen Amerikka: varten

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-LATAM-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-LATAM-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Japani:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-JPN-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-JPN-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Australia:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AUS-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AUS-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- [Azure Government](https://azure.microsoft.com/features/gov/), joka käyttää Azure US government-esiintymä:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USGOV-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USGOV-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Kanadan:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-CANADA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-CANADA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Saksa:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-GERMANY-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-GERMANY-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Intia: varten

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-INDIA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-INDIA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey


Kun suoritat tämän komennon, käytä seuraavia ohjeita:

- Korvaa *contosokey* tunniste, jota käytit Luo avain **Vaihe 3.3: Luo uusi avain** vaiheesta, [Luo key-tuotetunnuksen](#step-3-generate-your-key) .

- Korvaa *SubscriptionID* Azure tilaukseen, joka sisältää avaimen säilö tunnus. Tämän arvon noudettu aiemmin, valitse **Vaihe 1.2: Hae Azure Tilaustunnus** [valmisteleminen Internet-yhteys työaseman](#step-1-prepare-your-internet-connected-workstation) vaiheesta.

- Korvaa *ContosoFirstHSMKey* selite, jota käytetään tulostus-tiedostonimi.

Kun tämä onnistuu, näyttöön tulee **tulos: SUCCESS** ja uuden tiedoston nykyiseen kansioon, jossa on seuraava nimi: TransferPackage -*ContosoFirstHSMkey*.byok

###<a name="step-44-copy-your-key-transfer-package-to-the-internet-connected-workstation"></a>Vaihe 4.4: Kopioiminen avaimen siirto-paketin työaseman Internet-yhteys

Kopioi kohdetiedosto edellisessä vaiheessa (KeyTransferPackage ContosoFirstHSMkey.byok) Internet-yhteys työasemaan USB-asemassa tai muita kannettavat tallennustilan avulla.

##<a name="step-5-transfer-your-key-to-azure-key-vault"></a>Vaihe 5: Siirrä key-tuotetunnuksen Azure avaimen säilö

Internet-yhteys, työasemassa Tässä viimeisessä vaiheessa, käytä avaimen siirto-paketti, jonka kopioit yhteys katkaistu työaseman Azure avaimen säilö:: HSM voit ladata [Lisää AzureKeyVaultKey](https://msdn.microsoft.com/library/azure/dn868048\(v=azure.300\).aspx) cmdlet-komennon:

    Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMkey' -KeyFilePath 'c:\TransferPackage-ContosoFirstHSMkey.byok' -Destination 'HSM'

Jos lataaminen onnistuu, näet juuri lisäämäsi avain ominaisuudet näkyviin.


##<a name="next-steps"></a>Seuraavat vaiheet

Voit nyt:: HSM suojattu avaimeen avaimen säilöön. Lisätietoja on kohdassa **, haluatko käyttää laitteiston:: (HSM)-moduulia** [Azure avaimen säilö käytön aloittaminen](key-vault-get-started.md) -opetusohjelmassa.

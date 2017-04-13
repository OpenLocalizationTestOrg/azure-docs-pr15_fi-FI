<properties
    pageTitle="Aloita Azure avaimen säilö | Microsoft Azure"
    description="Tässä opetusohjelmassa avulla pääset alkuun Azure avaimen säilö vahvistettu säilön luominen Azure voivat tallentaa ja hallita salausavaimet ja Azure tietoja."
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
    ms.topic="hero-article"
    ms.date="10/24/2016"
    ms.author="cabailey"/>

# <a name="get-started-with-azure-key-vault"></a>Azure avaimen säilö käytön aloittaminen #
Azure avaimen säilö on käytettävissä useimmilla alueilla. Lisätietoja on artikkelissa [avaimen säilö hinnat sivun](https://azure.microsoft.com/pricing/details/key-vault/).

## <a name="introduction"></a>Johdanto  
Tässä opetusohjelmassa avulla pääset alkuun Azure avaimen säilö vahvistettu säilön (säilö) luominen Azure voivat tallentaa ja hallita salausavaimet ja Azure tietoja. Se käydään läpi käyttäminen PowerShellin Azure luominen säilö, joka sisältää avaimen tai salasana, joita voit käyttää Azure-sovellukseen. Se sitten kerrotaan, miten sovellus voi käyttää kyseisen avain tai salasana.

**Arvioitu kesto:** 20 minuutin ajan

>[AZURE.NOTE]  Tässä opetusohjelmassa ei ole ohjeiden Kirjoita Azure-sovellus, että jokin vaiheista sisältyy, eli miten hakemuksen avaimen tai salaisuus käyttäminen avaimen säilö.
>
>Tässä opetusohjelmassa käyttää PowerShellin Azure. Katso ohjeet Office kaikissa ympäristöissä käyttöliittymä [vastaavat tässä opetusohjelmassa](key-vault-manage-with-cli.md).

Yleistietoja Azure avaimen säilö, katso [Azure avaimen säilö ominaisuudet?](key-vault-whatis.md)

## <a name="prerequisites"></a>Edellytykset

Jotta voit suorittaa tässä opetusohjelmassa, sinulla on oltava seuraavasti:

- Microsoft Azure-tilausta. Jos ei ole, voit rekisteröityä [ilmaisen tilin](https://azure.microsoft.com/pricing/free-trial/).
- Azure PowerShell- **version minimivaatimus, 1.1.0**. Voit asentaa PowerShellin Azure ja liittää sen Azure-tilaus, katso, [miten voit asentaa ja määrittää PowerShellin Azure](../powershell-install-configure.md). Jos olet jo asentanut PowerShellin Azure ja et tiedä versioksi PowerShellin Azure-konsolin, kirjoita `(Get-Module azure -ListAvailable).Version`. Kun PowerShellin Azure 0.9.1 kautta 0.9.8 asennettu versio, voit käyttää tässä opetusohjelmassa edelleen kanssa makroon pieniä muutoksia. Esimerkiksi sinun on käytettävä `Switch-AzureMode AzureResourceManager` komento ja Azure avaimen säilö komentoja ovat muuttuneet. Versiot 0.9.1 0.9.8 kautta avaimen säilö cmdlet-komentojen luettelo on artikkelissa [Azure avaimen säilö cmdlet-komennot](https://msdn.microsoft.com/library/azure/dn868052\(v=azure.98\).aspx). 
- Sovellus, joka on määritetty käyttämään-näppäintä tai salasana, jonka luot tässä opetusohjelmassa. Esimerkkisovellus on käytettävissä [Microsoft Download Centeristä](http://www.microsoft.com/en-us/download/details.aspx?id=45343). Katso ohjeet mukana Lueminut-tiedosto.


Tässä opetusohjelmassa on suunniteltu PowerShellin Azure aloittelijoille, mutta se olettaa ymmärtää peruskäsitteet, kuten moduulit, cmdlet-komennot ja istuntoja. Lisätietoja on artikkelissa [Windows PowerShellin käytön aloittaminen](https://technet.microsoft.com/library/hh857337.aspx).

Saat yksityiskohtaisia ohjeita cmdlet, joissa kerrotaan tässä opetusohjelmassa, **Hanki ohjeita** cmdlet-komennon avulla

    Get-Help <cmdlet-name> -Detailed

Saat ohjeita **Kirjautuminen AzureRmAccount** cmdlet-komento, esimerkiksi:

    Get-Help Login-AzureRmAccount -Detailed

Voit myös lukea tutustutaan Azure resurssien hallinta PowerShellin Azure-opetusohjelmassa:

- [Asentaminen ja määrittäminen PowerShellin Azure](../powershell-install-configure.md)
- [Resurssien hallinnan Azure PowerShellin avulla](../powershell-azure-resource-manager.md)


## <a id="connect"></a>Tilausten yhdistäminen ##

PowerShellin Azure-istunnon aloittaminen ja kirjaudu sisään Azure tiliisi seuraavalla komennolla:  

    Login-AzureRmAccount 

Huomaa, että, jos käytössäsi on työnkulun esiintymään Azure, esimerkiksi Azure Government Käytä parametri - ympäristön tällä komennolla. Esimerkki:`Login-AzureRmAccount –Environment (Get-AzureRmEnvironment –Name AzureUSGovernment)`

Kirjoita ponnahdusikkunoiden selainikkunassa Azure tilin käyttäjänimi ja salasana. Azure PowerShell saa kaikki haluamasi tilaukset, jotka liittyvät tällä tilillä ja oletusarvoisesti, käyttää ensimmäistä.

Jos on useita tilauksia ja haluat määrittää tietyn käytettävä Azure avaimen säilö, kirjoita näet tilisi tilaukset:

    Get-AzureRmSubscription

Jos haluat määrittää tilauksen käyttämään, kirjoita:

    Set-AzureRmContext -SubscriptionId <subscription ID>

Lisätietoja PowerShellin Azure Katso, [miten asennetaan ja määritetään PowerShellin Azure](../powershell-install-configure.md).


## <a id="resource"></a>Luo uusi resurssiryhmä ##

Kun käytät Azure Resurssienhallinta, kaikki liittyvät resurssit luodaan resurssiryhmä sisällä. Luodaan uusi resurssiryhmä nimetyt **ContosoResourceGroup** Tässä opetusohjelmassa:

    New-AzureRmResourceGroup –Name 'ContosoResourceGroup' –Location 'East Asia'


## <a id="vault"></a>Luo avaimen säilö ##

[Uusi AzureRmKeyVault](https://msdn.microsoft.com/library/azure/mt603736\(v=azure.300\).aspx) cmdlet-komennon avulla voit luoda avaimen säilö. Tämä cmdlet-komento on kolme pakollinen parametreja: **resurssiryhmän nimi**, **avaimen säilö nimi**ja **maantieteellisen sijainnin**.

Jos käytät **ContosoKeyVault**säilö nimi, **ContosoResourceGroup**resurssin ryhmänimi ja **Itä-Aasian**sijainnin, esimerkiksi:

    New-AzureRmKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia'

Cmdlet tulos näyttää avaimen säilö juuri luomasi ominaisuudet. Kaksi tärkeimmät ominaisuudet ovat:

- **Säilö nimi**: Tämä on esimerkki **ContosoKeyVault**. Voit käyttää tätä nimeä muiden avaimen säilö cmdlet-komennot.
- **Säilö URI**: esimerkissä tämä on https://contosokeyvault.vault.azure.net/. Oman säilö sen REST API-Liittymän kautta käyttävät sovellukset on käytettävä tämän URI.

Suorita kaikki tämän avaimen säilö nyt oikeus Azure-tili. Kukaan muu ei vielä.

>[AZURE.NOTE]  Jos näet **tilaus ei ole rekisteröity käyttämään nimitilan 'Microsoft.KeyVault'** virheen, kun yrität luoda uuden käyttäjän avaimen säilö, suorittaa `Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.KeyVault"` ja suorita uusi AzureRmKeyVault-komentoa. Lisätietoja on artikkelissa [Rekisteröi AzureRmResourceProvider](https://msdn.microsoft.com/library/azure/mt759831\(v=azure.300\).aspx).
>

## <a id="add"></a>Lisää avaimen tai salaisuus avaimen säilö ##

Jos haluat Azure avaimen säilö luovan ohjelmisto suojattu näppäintä, [Lisää AzureKeyVaultKey](https://msdn.microsoft.com/library/azure/dn868048\(v=azure.300\).aspx) cmdlet-komennon ja kirjoita seuraava komento:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey' -Destination 'Software'

Kuitenkin jos on olemassa oleva ohjelmisto suojattu avain. PFX tallennettu C:\ kiintolevylle softkey.pfx, jotka haluat ladata Azure avain säilöön-tiedostossa Kirjoita muuttujan **securepfxpwd** salasanalla **123** Määritä. PFX-tiedosto:

    $securepfxpwd = ConvertTo-SecureString –String '123' –AsPlainText –Force

Kirjoita sitten Tuo-näppäintä. PFX-tiedosto, joka suojaa avain ohjelmiston avain säilöön-palvelussa:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey' -KeyFilePath 'c:\softkey.pfx' -KeyFilePassword $securepfxpwd


Voit nyt viitata avaimeen luomasi tai ladata sen URI käyttämällä Azure avaimen säilö. Käytä **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** saat aina nykyisen version ja Hae tietyn versiossa **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87** .  

Jos haluat avaimeen URI, kirjoita:

    $Key.key.kid

Salaisuus lisääminen säilöön, jotka on nimetty SQLPassword salasana ja arvo on Pa$ $w0rd Azure avaimen säilö, ensin muuntaa arvon, Pa$ $w0rd suojatun merkkijonoksi kirjoittamalla seuraava:

    $secretvalue = ConvertTo-SecureString 'Pa$$w0rd' -AsPlainText -Force

Valitse Kirjoita seuraava komento:

    $secret = Set-AzureKeyVaultSecret -VaultName 'ContosoKeyVault' -Name 'SQLPassword' -SecretValue $secretvalue

Voit nyt viitata salasana, jonka lisäsit Azure avaimen säilö käyttämällä sen URI. Käytä **https://ContosoVault.vault.azure.net/secrets/SQLPassword** saat aina nykyisen version ja Hae tietyn versiossa **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d** .

Jos haluat tämän salaisuus URI, kirjoita:

    $secret.Id

Oletetaan, että Näytä avain tai salaisuus juuri luomasi:

- Jos haluat tarkastella key-tuotetunnuksen, kirjoita:`Get-AzureKeyVaultKey –VaultName 'ContosoKeyVault'`
- Jos haluat tarkastella salainen, tyyppi:`Get-AzureKeyVaultSecret –VaultName 'ContosoKeyVault'`

Nyt avaimen säilö ja avain tai salaisuus on valmis käyttämään sovelluksia. Sinun on sallittava sovellukset voivat käyttää niitä.  

## <a id="register"></a>Sovelluksen rekisteröiminen Azure Active Directory-hakemistosta ##

Kehittäjä-erilliseen tietokoneeseen yleensä tekemän tämän vaiheen. Ei koske Azure avaimen säilö, mutta sisältyy tähän täydellisiä varten.


>[AZURE.IMPORTANT] Opetusohjelman, tilisi tai säilö sovellus, jossa rekisteröidään tämän vaiheen suorittamiseen kaikkien on oltava samassa kansiossa Azure.

Avaimen säilö käyttävät sovellukset on todentavat tunnuksen Azure Active Directorysta. Voit tehdä tämän sovelluksen omistaja on ensin rekisteröitävä sovelluksen niiden Azure Active Directoryn. Sovelluksen omistaja saa rekisteröinnin lopussa seuraavat arvot:


- **Tunnus** (tunnetaan myös nimellä Asiakastunnus) ja **todennuksen avain** (tunnetaan myös nimellä jaettuun salaisuus). Sovellus on pitää nämä arvot Azure Active Directory-tunnuksen hankkiminen. Miten sovellus on määritetty toiminto riippuu sovelluksesta. Avaimen säilö malli-sovelluksen sovelluksen omistaja määrittää nämä arvot app.config-tiedostoa.

Voit rekisteröidä sovelluksen Azure Active Directoryn seuraavasti:

1. Kirjaudu Azure perinteinen-portaaliin.
2. Valitse vasemmalla olevassa **Active Directory**ja valitse sitten kansio, jossa sovellus rekisteröidään. <br> <br> **Huomautus:** Sinun on valittava samaan kansioon, joka sisältää Azure tilaus, johon olet luonut avaimen säilö. Jos et tiedä mikä directory tämä on- **asetukset**, tilauksen, jonka loit avaimen säilö tunnistaminen ja viimeisen sarakkeen näkyvät kansion nimi muistiin.

3. Valitse **SOVELLUKSET**. Jos ei ole sovelluksia on lisätty kansioon, tällä sivulla näkyvät vain **Lisää sovellus** -linkki. Napsauta linkkiä tai vaihtoehtoisesti valitsemalla **Lisää** komentopalkista.
4.  Ohjatun **Lisää sovellus** - **Mitä haluat tehdä?** -sivulla **Lisää sovellus kehittää oman organisaation ulkopuolelta**.
5.  **Kerro sovelluksesi tietoja** -sivulla sovelluksesi nimi ja valitse sitten **WEB APPLICATION ja/tai verkko-Ohjelmointirajapinnan** (oletusarvo). Napsauta **Seuraava** -kuvake.
6.  **Sovelluksen ominaisuudet** -sivun Määritä **KIRJAUDU edelleen URL-osoite** ja **Sovelluksen tunnus URI** web-sovelluksen. Jos sovellusta ei ole näitä arvoja, voit tehdä niistä sinua tämän vaiheen käyttäjäksi (esimerkiksi määrittämällä molemmista ruuduista http://test1.contoso.com). Se ei ole merkitystä, jos näiden sivustojen olemassa. Asiat on kerrottu siitä, että kunkin sovelluksen tunnus URI-sovellus on erilaisia jokaisen sovelluksen hakemistossa. Hakemiston käyttää tätä merkkijonoa tunnistamiseen sovelluksen.
7.  Valitse Tallenna muutokset ohjatussa **Valmis** -kuvake.
8.  Napsauta **Pika-aloitus** -sivulla **Määritä**.
9.  Siirry **näppäimet** -osaan, valitse keston ja valitse sitten **Tallenna**. Sivun päivittää ja näyttää nyt avainarvon. Sinun on määritettävä sovelluksen tämä arvo sekä **Asiakastunnus** -arvon. (Tämä määritys ohjeet ovat sovelluksen kielikohtaiset.)
10. Kopioi tämän sivun, jota voit käyttää seuraavassa vaiheessa voit määrittää oman säilö asiakkaan tunnus-arvon.

## <a id="authorize"></a>Määritä sovellus käyttää avainta tai salaisuus ##

Määritä sovellus käyttää avainta tai säilö salaisuus sivustoa  [Määrittäminen AzureRmKeyVaultAccessPolicy](https://msdn.microsoft.com/library/azure/mt603625\(v=azure.300\).aspx) cmdlet-komento.

Esimerkiksi riippumatta siitä, onko nimesi säilö **ContosoKeyVault** ja haluat sallia sovelluksen on Asiakastunnus 8f8c4bbd 485b-45fd-98f7-ec6300b7b4ed, ja haluat sallia sovelluksen salaus ja kirjaudu näppäimet lisääminen säilöön, toimi seuraavasti:


    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToKeys decrypt,sign

Jos haluat sallia saman sovelluksesta lukea tietoja oman säilöön, suorita seuraava:


    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToSecrets Get

## <a id="HSM"></a>Jos haluat käyttää laitteiston:: (HSM)-moduulia ##

Lisätty assurance voit tuoda tai luo näppäimet moduuleissa laitteiston security (HSMs), jätä koskaan:: HSM reunaa. HSMs ovat FIPS 140-2 tason 2 vahvistettu. Jos tämä vaatimus ei koske voit, ohittaa tämän osion ja siirry [liittyvistä näppäimet ja tietoja ja poistaa avaimen säilö](#delete).

Voit luoda:: HSM suojattu painettavat näppäimet, sinun on käytettävä [Azure avaimen säilö Premium palvelutaso tukemaan:: HSM suojattu näppäimet](https://azure.microsoft.com/pricing/free-trial/). Huomaa, että tämä toiminto ei ole käytettävissä Azure Kiinan lisäksi.


Kun luot avaimen säilö, Lisää **Tuote -** parametri:


    New-AzureRmKeyVault -VaultName 'ContosoKeyVaultHSM' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia' -SKU 'Premium'



Voit lisätä ohjelmisto suojattu näppäimet (kuten aiemmin) ja:: HSM suojattu näppäimet tämän avaimen säilö. Luominen:: HSM suojattu avain **-kohteen** :: 'HSM' parametri:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -Destination 'HSM'

Seuraavalla komennolla voit tuoda-näppäintä. PFX-tiedosto tietokoneesta. Tämä komento tuodaan HSMs avain avain säilöön-palvelussa:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -KeyFilePath 'c:\softkey.pfx' -KeyFilePassword $securepfxpwd -Destination 'HSM'


Seuraava komento tuo "Tuo oma avain" (BYOK)-paketti. Tässä skenaariossa voit luoda paikallisen::-HSM key-tuotetunnuksen, ja siirrä se HSMs avain säilöön-palvelussa ilman jätä:: HSM rajan avainta:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -KeyFilePath 'c:\ITByok.byok' -Destination 'HSM'

Katso tarkempia ohjeet Luo BYOK-paketti, [Voit luoda ja Azure avaimen säilö siirto:: HSM suojattu näppäimet](key-vault-hsm-protected-keys.md).

## <a id="delete"></a>Poista avaimen säilö ja liittyvän avaimet ja tietoja ##

Jos et enää tarvitse avaimen säilö ja avain tai sen sisältämät salaisuus, voit poistaa avaimen säilö [Poista AzureRmKeyVault](https://msdn.microsoft.com/library/azure/mt619485\(v=azure.300\).aspx) cmdlet-komennolla:

    Remove-AzureRmKeyVault -VaultName 'ContosoKeyVault'

Vaihtoehtoisesti voit poistaa koko Azure resurssi-ryhmä, joka sisältää avaimen säilö ja muut resurssit, joiden ryhmään sisältyviä:

    Remove-AzureRmResourceGroup -ResourceGroupName 'ContosoResourceGroup'


## <a id="other"></a>Muut Azure PowerShellin cmdlet-komennot ##

Muut komennot, joista voi olla hyötyä Azure avaimen säilö hallinta:

- `$Keys = Get-AzureKeyVaultKey -VaultName 'ContosoKeyVault'`: Tämä komento saa taulukkomuotoinen Näytä kaikki näppäimet ja valitut ominaisuudet.
- `$Keys[0]`: Tämä komento näyttää luettelon kaikista määritetyn avaimen ominaisuudet
- `Get-AzureKeyVaultSecret`: Tämä komento on lueteltu taulukkomuotoinen Näytä kaikki salainen nimien ja valitut ominaisuudet.
- `Remove-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey'`: Esimerkki voit poistaa tietyn avaimen.
- `Remove-AzureKeyVaultSecret -VaultName 'ContosoKeyVault' -Name 'SQLPassword'`: Esimerkki voit poistaa tietyn salaisuus.


## <a id="next"></a>Seuraavat vaiheet ##

Katso seuranta-opetusohjelma, joka käyttää Azure avaimen säilö web-sovelluksen, [Käytä Azure avaimen säilö WWW-sovelluksesta](key-vault-use-from-web-application.md).

Jos haluat nähdä, miten tärkeimmät säilö käytetään, artikkelissa [Azure avaimen säilö kirjaaminen](key-vault-logging.md).

Luettelo Azure avaimen säilö uusimman Azure PowerShellin cmdlet-komennot on artikkelissa [Azure avaimen säilö cmdlet](https://msdn.microsoft.com/library/azure/dn868052\(v=azure.300\).aspx). 
 

Ohjelmointi viittauksia, artikkelissa [Azure avaimen säilö kehittäjän oppaassa](key-vault-developers-guide.md).

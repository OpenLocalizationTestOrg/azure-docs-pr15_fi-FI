<properties
    pageTitle="Hallitse avaimen säilö käyttämällä CLI | Microsoft Azure"
    description="Tässä opetusohjelmassa avulla voit automatisoida yleisiä tehtäviä avain säilöön CLI avulla"
    services="key-vault"
    documentationCenter=""
    authors="BrucePerlerMS"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/26/2016"
    ms.author="bruceper"/>

# <a name="manage-key-vault-using-cli"></a>Avaimen säilö käyttämällä CLI hallinta #
Azure avaimen säilö on käytettävissä useimmilla alueilla. Lisätietoja on artikkelissa [avaimen säilö hinnat sivun](https://azure.microsoft.com/pricing/details/key-vault/).

## <a name="introduction"></a>Johdanto  
Tässä opetusohjelmassa avulla pääset alkuun Azure avaimen säilö vahvistettu säilön (säilö) luominen Azure voivat tallentaa ja hallita salausavaimet ja Azure tietoja. Se käydään läpi käyttäminen Azure Office kaikissa ympäristöissä käyttöliittymä luominen säilö, joka sisältää avaimen tai salasana, joita voit käyttää Azure-sovellukseen. Se sitten kerrotaan, miten sovelluksen voit käyttää kyseistä avain tai salasana.

**Arvioitu kesto:** 20 minuutin ajan

>[AZURE.NOTE]  Tässä opetusohjelmassa ei ole ohjeita Kirjoita Azure-sovellus, että jokin vaiheista sisältyy, jossa esitetään, kuinka voit sallia sovelluksen käyttämään avaimen tai salaisuus avaimen säilö.
>
>Tällä hetkellä et voi määrittää Azure avaimen säilö Azure-portaalissa. Käytä Office kaikissa ympäristöissä käyttöliittymä näitä ohjeita. Tai katso ohjeet PowerShellin Azure [vastaavat tässä opetusohjelmassa](key-vault-get-started.md).

Yleistietoja Azure avaimen säilö, katso [Azure avaimen säilö ominaisuudet?](key-vault-whatis.md)

## <a name="prerequisites"></a>Edellytykset
Jotta voit suorittaa tässä opetusohjelmassa, sinulla on oltava seuraavasti:

- Microsoft Azure-tilausta. Jos ei ole, voit rekisteröityä [maksuttoman kokeiluversion käyttäjäksi](https://azure.microsoft.com/pricing/free-trial).
- Käyttöliittymä versio 0.9.1 tai uudempi versio. Asenna uusin versio ja Azure tilauksen yhdistäminen on ohjeaiheessa [asennetaan ja määritetään Azure Office kaikissa ympäristöissä käyttöliittymä](../xplat-cli-install.md).
- Sovellus, joka on määritetty käyttämään-näppäintä tai salasana, jonka luot tässä opetusohjelmassa. Esimerkkisovellus on käytettävissä [Microsoft Download Centeristä](http://www.microsoft.com/download/details.aspx?id=45343). Katso ohjeet mukana Lueminut-tiedosto.

## <a name="getting-help-with-azure-cross-platform-command-line-interface"></a>Ohjeiden saaminen Azure Office kaikissa ympäristöissä komentorivivalitsimet käyttöliittymä

Tässä opetusohjelmassa oletetaan, että olet tutustunut (Bash, pääte-komentokehote) käyttöliittymä

Käyttää--Ohje tai -h parametrin avulla voidaan tarkastella tiettyjä komentoja ohjetta. Voit vaihtoehtoisesti azure Ohje [komento] [Valitsimet] muoto voidaan myös palauttaa samat tiedot. Esimerkiksi seuraavat komennot kaikki palauttavat samat tiedot:

    azure account set --help

    azure account set -h

    azure help account set

Epävarmoissa komennon tarvitsemia parametreista, viittaavat ohjeen avulla--Ohje, -h tai azure Ohje [komento].

Voit myös lukea opetusohjelmassa tutustutaan Azure resurssien hallinnan Azure Office kaikissa ympäristöissä komentorivivalitsimet käyttöliittymän:

- [Asentaminen ja määrittäminen Azure Office kaikissa ympäristöissä komentoliittymä rivi](../xplat-cli-install.md)
- [Azure Office kaikissa ympäristöissä käyttöliittymä ja Azure resurssien hallinnan avulla](../xplat-cli-azure-resource-manager.md)


## <a name="connect-to-your-subscriptions"></a>Tilausten yhdistäminen

Voit kirjautua sisään organisaation tilille, käytä seuraavaa komentoa:

    azure login -u username -p password

tai jos haluat kirjautua sisään kirjoittamalla vuorovaikutteisesti

    azure login

>[AZURE.NOTE]  Kirjaudu sisään-menetelmä toimii vain organisaation tilillä. Käytössä on organisaatiotili on käyttäjä, joka hallitsee organisaatiosi, ja määrittää organisaation Azure Active Directory-vuokraajan.


Jos ei ole tällä hetkellä käytössä on organisaatiotili ja käyttävät Azure tilauksen Kirjaudu Microsoft-tili, voit helposti luoda sen seuraavien ohjeiden mukaisesti.

1.  Kirjaudu [Azure hallinta-portaalin](https://manage.windowsazure.com/)ja valitse Active Directory-kirjautuminen.
2.  Jos hakemistoa löytyy, valitse Luo hakemistossa ja anna tarvittavat tiedot.
3.  Valitse hakemisto ja Lisää uusi käyttäjä. Käyttöoikeuden uudelle käyttäjälle on organisaatiotili. Käyttäjän luonnin aikana voit toimitetaan kummankin osoitteen käyttäjän ja tilapäinen salasana. Tallentaa tiedot, kun sitä käytetään toisen vaiheen.
4.  Työkaluportaaliin ja valitse asetukset ja valitse sitten järjestelmänvalvojat. Valitse Lisää ja Lisää uusi käyttäjä työtovereiden järjestelmänvalvojana. Näin Hallitse tilaustasi Azure organisaatiotili.
5.  Lopuksi Kirjaudu ulos Azure portaalin ja kirjaudu sitten takaisin sisään käyttämällä uutta organisaation tiliä. Jos ensimmäisen kerran sisäänkirjautumisessa tämän tilillä, voit pyytää salasanan vaihtaminen.

Saat lisätietoja käytössä on organisaatiotili käyttäminen Microsoft Azure [Microsoft Azure organisaationa rekisteröityminen](../active-directory/sign-up-organization.md).

Jos on useita tilauksia ja haluat määrittää tietyn käytettävä Azure avaimen säilö, kirjoita näet tilisi tilaukset:

    azure account list

Jos haluat määrittää tilauksen käyttämään, kirjoita:

    azure account set <subscription name>

Lisätietoja Azure Office kaikissa ympäristöissä käyttöliittymä määrittämisestä on artikkelissa [asentaminen ja määrittäminen Azure Office kaikissa ympäristöissä käyttöliittymä](../xplat-cli-install.md).


## <a name="switch-to-using-azure-resource-manager"></a>Siirry Azure resurssien hallinnan avulla

Avaimen säilö edellyttää Azure Resurssienhallinta, joten kirjoita Siirry Azure resurssien hallinnan tila seuraavasti:

    azure config mode arm

## <a name="create-a-new-resource-group"></a>Luo uusi resurssiryhmä

Kun käytät Azure Resurssienhallinta, kaikki liittyvät resurssit luodaan resurssiryhmä sisällä. Luodaan uusi resurssiryhmä 'ContosoResourceGroup' opetusohjelmassa.

    azure group create 'ContosoResourceGroup' 'East Asia'

Ensimmäinen parametri on resurssiryhmän nimi ja sijainti on toisen parametrin. Sijainti-komennolla `azure location list` tunnistavan määrittäminen yhteystietoon, tässä esimerkissä vaihtoehtoisen sijainnin. Jos tarvitset lisätietoja, kirjoita:`azure help location`

## <a name="register-the-key-vault-resource-provider"></a>Rekisteröi avaimen säilö resurssin tarjoajaan
Varmista, että avaimen säilö resurssin palvelun rekisteröidään tilauksen:

`azure provider register Microsoft.KeyVault`

Tämä on tehtävä vain kerran tilauskohtaisten vain.


## <a name="create-a-key-vault"></a>Luo avaimen säilö

Käytä `azure keyvault create` komento luo avaimen säilö. Tämä komentosarja on kolme pakollinen parametreja: resurssien ryhmänimi ja maantieteellisen sijainnin avaimen säilö nimi.

Jos käytät ContosoKeyVault säilö nimi, ContosoResourceGroup resurssin ryhmänimi ja Itä-Aasian sijainnin, esimerkiksi:

    azure keyvault create --vault-name 'ContosoKeyVault' --resource-group 'ContosoResourceGroup' --location 'East Asia'

Tämä komento tulos näyttää avaimen säilö juuri luomasi ominaisuudet. Kaksi tärkeimmät ominaisuudet ovat:

- **Nimi**: Tämä on esimerkki ContosoKeyVault. Voit käyttää tätä nimeä muiden avaimen säilö cmdlet-komennot.
- **vaultUri**: Tämä on esimerkki https://contosokeyvault.vault.azure.net. Oman säilö sen REST API-Liittymän kautta käyttävät sovellukset on käytettävä tämän URI.

Suorita kaikki tämän avaimen säilö nyt oikeus Azure-tili. Kukaan muu ei vielä.


## <a name="add-a-key-or-secret-to-the-key-vault"></a>Lisää avaimen tai salaisuus avaimen säilö

Jos haluat Azure avaimen säilö luovan ohjelmisto suojattu näppäintä, käytä `azure key create` komento ja kirjoita seuraava:

    azure keyvault key create --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey' --destination software

Kuitenkin jos on olemassa oleva avain .pem-tiedosto tallennetaan paikallinen tiedosto, jonka haluat ladata Azure avaimen säilö softkey.pem-tiedostossa, kirjoita seuraava tuo-näppäintä. PEM tiedosto, joka suojaa avain ohjelmiston avain säilöön-palvelussa:

    azure keyvault key import --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey' --pem-file './softkey.pem' --password 'PaSSWORD' --destination software

Voit nyt viitata avain, joka luodaan tai ladataan Azure avaimen säilö käyttämällä sen URI. Käytä **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** saat aina nykyisen version ja Hae tietyn versiossa **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87** .

Salaisuus lisääminen säilöön, joka sisältää arvon, Pa$ $w0rd Azure avaimen säilö, joka on nimetty SQLPassword salasanan ja kirjoita seuraava:

    azure keyvault secret set --vault-name 'ContosoKeyVault' --secret-name 'SQLPassword' --value 'Pa$$w0rd'

Voit nyt viitata salasana, jonka lisäsit Azure avaimen säilö käyttämällä sen URI. Käytä **https://ContosoVault.vault.azure.net/secrets/SQLPassword** saat aina nykyisen version ja Hae tietyn versiossa **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d** .

Oletetaan, että Näytä avain tai salaisuus juuri luomasi:

- Jos haluat tarkastella key-tuotetunnuksen, kirjoita:`azure keyvault key list --vault-name 'ContosoKeyVault'`
- Jos haluat tarkastella salainen, tyyppi:`azure keyvault secret list --vault-name 'ContosoKeyVault'`


## <a name="register-an-application-with-azure-active-directory"></a>Sovelluksen rekisteröiminen Azure Active Directory-hakemistosta

Kehittäjä-erilliseen tietokoneeseen yleensä tekemän tämän vaiheen. Se ei koske Azure avaimen säilö, mutta ei mukana, täydellisiä.


>[AZURE.IMPORTANT] Opetusohjelman, tilisi tai säilö sovellus, jossa rekisteröidään tämän vaiheen suorittamiseen kaikkien on oltava samassa kansiossa Azure.

Avaimen säilö käyttävät sovellukset on todentavat tunnuksen Azure Active Directorysta. Voit tehdä tämän sovelluksen omistaja on ensin rekisteröitävä sovelluksen niiden Azure Active Directoryn. Sovelluksen omistaja saa rekisteröinnin lopussa seuraavat arvot:


- **Tunnus** (tunnetaan myös nimellä Asiakastunnus) ja **todennuksen avain** (tunnetaan myös nimellä jaettuun salaisuus). Sovellus on pitää molempia näistä arvoista Azure Active Directory-tunnuksen hankkiminen. Miten sovellus on määritetty toiminto riippuu sovelluksesta. Avaimen säilö malli-sovelluksen sovelluksen omistaja määrittää nämä arvot app.config-tiedostoa.



Voit rekisteröidä sovelluksen Azure Active Directoryn seuraavasti:

1. Kirjaudu Azure-portaaliin.
2. Valitse vasemmalla olevassa **Active Directory**ja valitse sitten kansio, jossa sovellus rekisteröidään. <br> <br> Huomautus: Sinun on valittava samaan kansioon, joka sisältää Azure tilaus, johon olet luonut avaimen säilö. Jos et tiedä mikä directory tämä on- **asetukset**, tilauksen, jonka loit avaimen säilö tunnistaminen ja viimeisen sarakkeen näkyvät kansion nimi muistiin.

3. Valitse **SOVELLUKSET**. Jos ei ole sovelluksia on lisätty kansioon, tällä sivulla näkyy vain **Lisää sovellus** -linkki. Napsauta linkkiä tai Vaihtoehtoisesti voit napsauttaa **Lisää** komentopalkista.
4.  Ohjatun **Lisää sovellus** - **Mitä haluat tehdä?** -sivulla **Lisää sovellus kehittää oman organisaation ulkopuolelta**.
5.  **Kerro sovelluksesi tietoja** -sivulla sovelluksesi nimi ja valitse **WEB APPLICATION ja/tai verkko-Ohjelmointirajapinnan** (oletusarvo). Napsauta seuraava-kuvake.
6.  **Sovelluksen ominaisuudet** -sivun Määritä **KIRJAUDU edelleen URL-osoite** ja **Sovelluksen tunnus URI** web-sovelluksen. Jos sovellusta ei ole näitä arvoja, voit tehdä niistä sinua tämän vaiheen käyttäjäksi (esimerkiksi määrittämällä molemmista ruuduista http://test1.contoso.com). Ei ole merkitystä, jos näiden sivustojen; asiat on kerrottu siitä, että kunkin sovelluksen tunnus URI-sovellus on erilaisia jokaisen sovelluksen hakemistossa. Hakemiston käyttää tätä merkkijonoa tunnistamiseen sovelluksen.
7.  Napsauta Tallenna muutokset ohjatussa valmis-kuvaketta.
8.  Napsauta Pika-aloitus-sivulla **Määritä**.
9.  Siirry **näppäimet** -osaan, valitse keston ja valitse sitten **Tallenna**. Sivun päivittää ja näyttää nyt avainarvon. Sinun on määritettävä sovelluksen tämä arvo sekä **Asiakastunnus** -arvon. (Tämä määritys ohjeet on sovelluksen kielikohtaiset.)
10. Kopioi tämän sivun, jota voit käyttää seuraavassa vaiheessa voit määrittää oman säilö asiakkaan tunnus-arvon.




## <a name="authorize-the-application-to-use-the-key-or-secret"></a>Määritä sovellus käyttää avainta tai salaisuus

Voit hyväksyä avain tai säilö salaisuus sovellus, `azure keyvault set-policy` komento.

Esimerkiksi jos säilöön nimi on ContosoKeyVault ja haluat sallia sovelluksen on Asiakastunnus 8f8c4bbd 485b-45fd-98f7-ec6300b7b4ed, ja haluat sallia sovelluksen salaus ja kirjaudu näppäimet lisääminen säilöön, suorita seuraavat:

    azure keyvault set-policy --vault-name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --perms-to-keys '["decrypt","sign"]'

>[AZURE.NOTE] Jos käytössäsi on Windows-komentokehote, olisi puolilainausmerkkeihin korvaaminen lainausmerkkejä ja pääse myös sisäinen lainausmerkeissä. Esimerkiksi: "[\"salauksen\",\"Kirjaudu\"]".

Jos haluat sallia saman sovelluksesta lukea tietoja oman säilöön, suorita seuraava:

    azure keyvault set-policy --vault-name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --perms-to-secrets '["get"]'

## <a name="if-you-want-to-use-a-hardware-security-module-hsm"></a>Jos haluat käyttää laitteiston:: (HSM)-moduulia ##

Lisätty assurance voit tuoda tai luo näppäimet moduuleissa laitteiston security (HSMs), jätä koskaan:: HSM reunaa. HSMs ovat FIPS 140-2 tason 2 vahvistettu. Jos tämä vaatimus ei koske voit, ohittaa tämän osion ja siirry [liittyvistä näppäimet ja tietoja ja poistaa avaimen säilö](#delete-the-key-vault-and-associated-keys-and-secrets).

Luo:: HSM suojattu painettavat näppäimet on oltava säilö tilauksessa, joka tukee:: HSM suojattu näppäimet.

Kun luot keyvault, Lisää "tuote"-parametri:

    azure azure keyvault create --vault-name 'ContosoKeyVaultHSM' --resource-group 'ContosoResourceGroup' --location 'East Asia' --sku 'Premium'

Voit lisätä ohjelmisto suojattu näppäimet (kuten aiemmin) ja:: HSM suojattu näppäimet tämän säilö. Luo:: HSM suojatun avaimen määrittämällä:: 'HSM' kohdeparametri:

    azure keyvault key create --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --destination 'HSM'

Seuraava komento avulla voit tuoda näppäintä .pem tiedostosta tietokoneeseen. Tämä komento tuodaan HSMs avain avain säilöön-palvelussa:

    azure keyvault key import --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --pem-file '/.softkey.pem' --destination 'HSM' --password 'PaSSWORD'

Seuraava komento tuo "Tuo oma avain" (BYOK)-paketti. Näillä oikeuksilla voit luoda paikallisen::-HSM key-tuotetunnuksen, ja siirrä se HSMs avain säilöön-palvelussa ilman jätä:: HSM rajan avainta:

    azure keyvault key import --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --byok-file './ITByok.byok' --destination 'HSM'

Saat lisätietoja yksityiskohtaiset ohjeet Luo BYOK-paketti, katso, [miten voit käyttää HSM-Protected-näppäimiä Azure avaimen säilö](key-vault-hsm-protected-keys.md).


## <a name="delete-the-key-vault-and-associated-keys-and-secrets"></a>Poista avaimen säilö ja liittyvän avaimet ja tietoja

Jos et enää tarvitse avaimen säilö ja avain tai sen sisältämät salaisuus, voit poistaa avaimen säilö azure keyvault Poista-komennon avulla:

    azure keyvault delete --vault-name 'ContosoKeyVault'

Vaihtoehtoisesti voit poistaa koko Azure resurssi-ryhmä, joka sisältää avaimen säilö ja muut resurssit, joiden ryhmään sisältyviä:

    azure group delete --name 'ContosoResourceGroup'


## <a name="other-azure-cross-platform-command-line-interface-commands"></a>Azure Office kaikissa ympäristöissä käyttöliittymä muut komennot

Muut komennot, voit ehkä apua hallinnassa Azure avaimen säilö.

Tämä komento on lueteltu taulukkomuotoinen Näytä kaikki näppäimet ja valitut ominaisuudet:

    azure keyvault key list --vault-name 'ContosoKeyVault'

Tämä komento näyttää luettelon kaikista määritetyn avaimen ominaisuudet:

    azure keyvault key show --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey'

Tämä komento on lueteltu taulukkomuotoinen Näytä kaikki salainen nimien ja valitut ominaisuudet:

    azure keyvault secret list --vault-name 'ContosoKeyVault'

Tässä on esimerkki siitä, miten voit poistaa tietyn avaimen:

    azure keyvault key delete --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey'

Tässä on esimerkki siitä, miten voit poistaa tietyn salaisuus:

    azure keyvault secret delete --vault-name 'ContosoKeyVault' --secret-name 'SQLPassword'


## <a name="next-steps"></a>Seuraavat vaiheet

Ohjelmointi viittauksia, artikkelissa [Azure avaimen säilö kehittäjän oppaassa](key-vault-developers-guide.md).

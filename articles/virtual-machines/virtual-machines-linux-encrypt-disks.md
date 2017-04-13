<properties
   pageTitle="Salaa Linux AM levyille | Microsoft Azure"
   description="Salaaminen levyille Linux-AM Azure-CLI ja resurssien hallinnan käyttöönottomalli"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="10/11/2016"
   ms.author="iainfou"/>

# <a name="encrypt-disks-on-a-linux-vm-using-the-azure-cli"></a>Salaa levyille Linux-AM Azure-CLI käyttäminen
Parannettu virtuaalikoneen (AM) suojaus ja vaatimustenmukaisuus virtual Azure-levyjen voidaan salata rest-palvelussa. Levyjen salataan käyttämällä salausavaimet, joka on suojattu Azure-avain säilöön. Määrittää nämä salausavaimet sekä voit valvoa niiden käyttöä. Tässä artikkelissa kuvataan, miten salaa virtual levyille Linux-AM Azure-CLI ja resurssien hallinnan käyttöönottomalli.


## <a name="quick-commands"></a>Pikakomennot
Jos haluat tehdä nopeasti tehtävän osan seuraavat asiat kantaluku komennot salaa virtual levyille oman AM. Tarkempia tietoja ja kontekstia kunkin vaiheen löytyy loput [käynnistäminen tässä](#overview-of-disk-encryption)asiakirjassa.

Sinun on [Uusin Azure CLI](../xplat-cli-install.md) asennettu ja kirjautunut sisään käyttämällä Resurssienhallinta-tilan seuraavasti:

```
azure config mode arm
```

Seuraavissa esimerkeissä korvaa Esimerkki parametrinimet oman arvoilla. Esimerkki parametrinimet ovat `myResourceGroup`, `myKeyVault`, ja `myVM`.

Ensin Azure-tilauksen piiriin kuuluvien Azure avain säilöön-toimittajan ottaminen käyttöön ja luoda resurssiryhmän. Seuraavassa esimerkissä luodaan resurssin ryhmänimi `myResourceGroup` - `WestUS` sijainti:

```bash
azure provider register Microsoft.KeyVault
azure group create myResourceGroup --location WestUS
```

Luo Azure avaimen säilö. Seuraava esimerkki luo avain-säilö nimeltä `myKeyVault`:

```bash
azure keyvault create --vault-name myKeyVault --resource-group myResourceGroup \
  --location WestUS
```

Luo muotoonsa avaimen avain säilöön ja salauksen käyttöön. Seuraava esimerkki luo avain nimeltä `myKey`:

```bash
azure keyvault key create --vault-name myKeyVault --key-name myKey \
  --destination software
azure keyvault set-policy --vault-name myKeyVault --resource-group myResourceGroup \
  --enabled-for-disk-encryption true
```

Luo päätepisteen Azure Active Directoryn avulla käsittelyyn todennus ja vaihtamalla salausavaimet avain säilöstä. `--home-page` Ja `--identifier-uris` ei tarvitse olla todellisia reititettävä osoite. Parhaan mahdollisen suojauksen, asiakkaan tietoja on tarkoitus käyttää sen sijaan, että salasanoja. Azure-CLI ei voi tällä hetkellä luoda asiakkaan tietoja. Asiakkaan tietoja voidaan luoda vain Azure-portaalissa. Seuraavassa esimerkissä luodaan päätepisteen Azure Active Directory-niminen `myAADApp` ja käyttää salasanan, jonka `myPassword`. Määritä toimimalla seuraavasti:

```bash
azure ad app create --name myAADApp \
  --home-page http://testencrypt.contoso.com \
  --identifier-uris http://testencrypt.contoso.com \
  --password myPassword
```

Huomautus `applicationId` edeltävä komento tulosteen mukaisesti. Tämä tunnus käytetään seuraavasti:

```bash
azure ad sp create --applicationId myApplicationID
azure keyvault set-policy --vault-name myKeyVault --spn myApplicationID \
  --perms-to-keys [\"all\"] --perms-to-secrets [\"all\"]
```

Lisää tietoja DVD-levyllä aiemmin AM. Seuraava esimerkki lisää tietoja DVD-levyllä AM, jonka nimi on `myVM`:

```bash
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

Tarkista tiedot avaimen säilö ja luomasi avain. Tarvitset avaimen säilö tunnus, URI ja avain viimeisessä vaiheessa URL-osoite. Seuraavassa esimerkissä tarkistaa tiedot, Key-säilö nimeltä `myKeyVault` ja avain nimeltä `myKey`:

```bash
azure keyvault show myKeyVault
azure keyvault key show myKeyVault myKey
```

Salaa levyjen seuraavasti, kirjoittamalla oman parametrinimet koko:

```bash
azure vm enable-disk-encryption --resource-group myResourceGroup --vm-name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
```

Azure-CLI ei voi luoda yksityiskohtainen virheitä salauksen aikana. Saat lisätietoja vianmäärityksestä `/var/log/azure/Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux/0.x.x.x/extension.log`. Edellisen komennon on monta muuttujaa ja saat ehkä ole paljon ilmoitus siitä, miksi prosessin epäonnistuu, valmiiksi-komento Esimerkki olisi seuraavasti:

```bash
azure vm enable-disk-encryption -g myResourceGroup -n MyVM \
  --aad-client-id 147bc426-595d-4bad-b267-58a7cbd8e0b6 \
  --aad-client-secret P@ssw0rd! \
  --disk-encryption-key-vault-url https://myKeyVault.vault.azure.net/ \ 
  --disk-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --key-encryption-key-url https://myKeyVault.vault.azure.net/keys/myKey/6f5fe9383f4e42d0a41553ebc6a82dd1 \
  --key-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResoureGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --volume-type Data
```

Lopuksi Tarkista Vahvista, että virtual levyjen on nyt salattu salaus-tila. Seuraavassa esimerkissä tarkistetaan AM, jonka nimi on tilaa `myVM` - `myResourceGroup` resurssiryhmä:

```bash
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```

## <a name="overview-of-disk-encryption"></a>Salauksen yleiskatsaus
Virtuaalinen levyille Linux VMs salataan käyttämällä [dm crypt](https://wikipedia.org/wiki/Dm-crypt)rest-palvelussa. On maksutonta virtual Azure-levyjen salaamiseen. Salausavaimet tallennetaan Azure avain säilöön ohjelmiston suojaamalla tai voit tuoda tai luo avaimien laitteiston suojauksen moduuleissa (HSMs) oikeus FIPS 140-2 tason 2 mukaisia. Nämä salausavaimet hallinnan säilyttäminen ja voit valvoa niiden käyttöä. Nämä salausavaimet käytetään salaaminen ja salauksen virtual oman AM yhdistetty levyjä. Azure Active Directory-päätepisteen on suojattu järjestelmä myöntämistä nämä salausavaimet, kuten VMs on kytketty käyttöön ja poistaminen käytöstä.

Prosessi, jossa AM salaaminen on seuraavanlainen:

1. Luo muotoonsa avaimen Azure-avain säilö.
2. Määritä salausavaimen voidaan käyttää levyjen salaamiseen.
3. Lukemaan salausavaimen Azure avain säilöstä Luo päätepisteen Azure Active Directory käyttäminen tarvittavat käyttöoikeudet.
4. Lähetä salaamaan virtual levyjä, päätepisteen Azure Active Directory-ja sopiva salausavaimen käytettävä komento.
5. Azure Active Directory-päätepisteen pyytää tarvittavat salausavaimen Azure avain säilöstä.
6. Virtuaalinen levyjen salataan käyttämällä annettua salausavaimen.


## <a name="supporting-services-and-encryption-process"></a>Palvelut ja salauksen prosessi
Salauksen riippuvainen Lisää seuraavat osat:

- **Azure avain säilöön** - käytettävä suojaa salausavaimet ja käyttää levyn salaus/salauksen prosessin tietoja. 
  - Jos sellainen on olemassa, voit käyttää aiemmin Azure-avain säilö. Ei ole tälle avaimen säilö salaaminen levyjä.
  - Erota järjestelmänvalvojan rajat avaimen näkyvyyttä, voit luoda erillinen avaimen säilö.
- **Azure Active Directory** - käsittelee suojatun vaihtamalla tarvittavat salausavaimet ja todennusta varten tarvittavat toiminnot. 
  - Aiemmin luodun Azure Active Directory-esiintymän käytetään tavallisesti asunto sovelluksen. 
  - Sovellus on enemmän päätepisteen pyytää ja Hae myönnetty tarvittavat salausavaimet avaimen säilö ja virtuaalikoneen-palveluja varten. Todellinen sovellus, joka integroituu Azure Active Directory ei kehittävät.


## <a name="requirements-and-limitations"></a>Vaatimukset ja rajoitukset
Tuetut tilanteet ja salauksen vaatimukset:

- Seuraavat Linux palvelin tuotteissa - Ubuntu, CentOS, SUSE ja SUSE Linux Enterprise Server (SLES) ja punainen on Enterprise Linux.
- Kaikki resurssit (esimerkiksi avaimen säilö, tallennustilan tilin ja AM) on oltava sama Azure alue- ja tilausasioiden.
- Vakio A, D, DS, G ja GS sarjan VMs.

Salauksen ei tällä hetkellä tueta seuraavissa tilanteissa:

- Tavallinen taso VMs.
- VMs luotu perinteinen käyttöönotto-mallin avulla.
- Linux VMs OS levyn salauksen käytöstä.
- Päivitys jo salattu Linux-AM salausavaimet.


## <a name="create-the-azure-key-vault-and-keys"></a>Luo Azure avaimen säilö ja avaimet
Viimeistele loput oppaan, sinun on [Uusin Azure CLI](../xplat-cli-install.md) asennettu ja kirjautunut sisään käyttämällä Resurssienhallinta-tilan seuraavasti:

```
azure config mode arm
```

Koko komento olevassa esimerkissä korvaa kaikki esimerkki parametrien nimet, sijainti ja avainarvot. Seuraavissa esimerkeissä käytetään konferenssi, `myResourceGroup`, `myKeyVault`, `myAADApp`jne.

Ensimmäiseksi on luotava Azure avaimen säilö, voit tallentaa salausavaimet. Azure avaimen säilö voi tallentaa näppäimet, tietoja tai salasanoja, jotta voit toteuttaa niitä turvallisesti sovelluksiin ja palveluihin. Virtuaalinen levyn salauksen käyttää avainta säilö salausavaimen, jota käytetään salaaminen ja salauksen virtual levyjen tallentamiseen. 

Azure-tilaukseesi Azure avain säilöön-toimittajan ottaminen käyttöön ja valitse sitten Luo resurssiryhmä. Seuraavassa esimerkissä luodaan nimeltä resurssiryhmä `myResourceGroup` - `WestUS` sijainti:

```bash
azure provider register Microsoft.KeyVault
azure group create myResourceGroup --location WestUS
```

Sama alue on sijaittava salausavaimet ja Laske liittyvät resurssit, kuten säilytys- ja AM itse sisältävät Azure avaimen säilö. Seuraavassa esimerkissä luodaan Azure avaimen säilö nimeltä `myKeyVault`:

```bash
azure keyvault create --vault-name myKeyVault --resource-group myResourceGroup \
  --location WestUS
```

Voit tallentaa salausavaimet käyttämällä ohjelmistoa tai:: laitteiston suojauksen mallin (HSM) suojaus. :: HSM edellyttää premium avaimen säilö. Tällä lisäkustannuksia luominen premium avaimen säilö vakio avaimen säilö, joka tallentaa ohjelmisto suojattu näppäimet sijaan. Luo premium avaimen säilö edellisessä vaiheessa lisäämällä `--sku Premium` -komentoa. Seuraavassa esimerkissä ohjelmisto suojattu näppäimet koska luomaasi vakio avaimen säilö. 

Molemmat suojaus-mallien Azure-ympäristö on myönnettävä access pyytää salausavaimet, kun purkaa virtual levyjen käynnistyy AM. Luo salausavain sisällä avaimen säilö ja valitse käyttöön virtual salauksen käytettäväksi. Seuraava esimerkki luo avain nimeltä `myKey` ja käyttöön salauksen:

```bash
azure keyvault key create --vault-name myKeyVault --key-name myKey \
  --destination software
azure keyvault set-policy --vault-name myKeyVault --resource-group myResourceGroup \
  --enabled-for-disk-encryption true
```


## <a name="create-the-azure-active-directory-application"></a>Azure Active Directory-sovelluksen luominen
Kun virtual levyjen salaus tai salauksen poistaminen, käytä päätepisteen käsittelemään todennus ja vaihtamalla salausavaimet avain säilöstä. Tämä päätepiste Azure Active Directory-sovelluksen avulla Azure ympäristö pyytää puolesta AM tarvittavat salausavaimet. Azure Active Directory oletusesiintymää sisältyy tilauksen, mutta monet organisaatiot on varattu Azure Active Directory-kansioita.

Kun luot ei koko Azure Active Directory-sovelluksen `--home-page` ja `--identifier-uris` parametrit seuraavassa esimerkissä ei tarvitse olla todellisia reititettävä osoite. Seuraava esimerkki määrittää myös salasana-pohjainen salaisuus luotaessa näppäimet Azure portaalin sijaan. Tällä hetkellä luodaan näppäimet ei voi luoda Azure-CLI. 

Azure Active Directory-sovelluksen luominen Seuraavassa esimerkissä luodaan sovelluksen nimi `myAADApp` ja käyttää salasanan, jonka `myPassword`. Määritä toimimalla seuraavasti:

```bash
azure ad app create --name myAADApp \
  --home-page http://testencrypt.contoso.com \
  --identifier-uris http://testencrypt.contoso.com \
  --password myPassword
```

Pane merkille `applicationId` , joka palautetaan tuloksissa edellisen komennon. Tämä tunnus käytetään joissakin annettuja ohjeita. Seuraavaksi luodaan palvelun pääasiallista nimi (SPN), niin, että sovellus on käytettävissä käyttöympäristössä. Onnistuneesti salata tai purkaa virtual levyjä, avain säilöön tallennettuja salausavaimen käyttöoikeuksista on määritettävä sallimaan lukemaan näppäimet Azure Active Directory-sovelluksen. 

Luo SPN ja määritä tarvittavat käyttöoikeudet seuraavalla tavalla:

```bash
azure ad sp create --applicationId myApplicationID
azure keyvault set-policy --vault-name myKeyVault --spn myApplicationID \
  --perms-to-keys [\"all\"] --perms-to-secrets [\"all\"]
```


## <a name="add-a-virtual-disk-and-review-encryption-status"></a>Lisää virtual DVD-levyllä ja tarkastele salauksen tila
Voit salata todella joidenkin virtual levyjen avulla lisätä levyn aiemmin AM. 5 gt tietoja DVD-levyllä lisääminen aiemmin luotuun AM seuraavasti:

```bash
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

Virtuaalinen levyjen eivät ole salattuja. Tarkista oman AM salauksen nykyisen tilan seuraavasti:

```bash
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```


## <a name="encrypt-virtual-disks"></a>Salaa virtual levyjen
Salaa nyt virtual levyjä, voit noutaa yhteen kaikki edellisen osat:

1. Määritä Azure Active Directory-sovellus ja salasana.
2. Määritä avaimen säilö levyille, salatun metatietojen tallentamiseen.
3. Määritä todellinen salaus ja salauksen salausavaimet.
4. Määrittää, haluatko salata OS levy, tiedot-levyjä tai kaikki.

Antaa Tarkista Azure-avain säilö ja sitten-näppäintä voit luoda, koska tarvitaan avaimen säilö tunnusta URI, ja paina näppäintä URL-Osoitteen viimeisessä vaiheessa tiedot:

```bash
azure keyvault show myKeyVault
azure keyvault key show myKeyVault myKey
```

Salaa käyttämällä tuloste virtual levyjen `azure keyvault show` ja `azure keyvault key show` komennot seuraavasti:

```bash
azure vm enable-disk-encryption --resource-group myResourceGroup --vm-name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
```

Edellisen komennon on monta muuttujaa, kuten seuraavassa esimerkissä on viittauksen valmiiksi-komento:

```bash
azure vm enable-disk-encryption -g myResourceGroup -n MyVM \
  --aad-client-id 147bc426-595d-4bad-b267-58a7cbd8e0b6 \
  --aad-client-secret P@ssw0rd! \
  --disk-encryption-key-vault-url https://myKeyVault.vault.azure.net/ \ 
  --disk-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --key-encryption-key-url https://myKeyVault.vault.azure.net/keys/myKey/6f5fe9383f4e42d0a41553ebc6a82dd1 \
  --key-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResoureGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --volume-type Data
```

Azure-CLI ei voi luoda yksityiskohtainen virheitä salauksen aikana. Saat lisätietoja vianmäärityksestä `/var/log/azure/Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux/0.x.x.x/extension.log` -salataan AM.

Lopuksi avulla, tarkista Vahvista, että virtual levyjen on nyt salattu salaus-tila:

```bash
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```


## <a name="add-additional-data-disks"></a>Lisää tiedot levyjä
Kun salattuja tietoja levyjä, voit lisätä myöhemmin, että AM muita virtual levyille ja myös salata. Kun suoritat `azure vm enable-disk-encryption` komennon, suurentaa järjestys-version avulla `--sequence-version` parametria. Parametrin järjestys version avulla voit suorittaa saman AM toistuvia toimintoja.

Antaa esimerkiksi lisätä toisen virtual levyn oman AM seuraavasti:

```bash
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

Suorita komento salaamaan virtual levyjen aika-lisääminen `--sequence-version` parametrin ja Microsoftin ensimmäisellä käyttökerralla arvot kasvavat seuraavasti:

```bash
azure vm enable-disk-encryption --resource-group myResourceGroup --vm-name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
  --sequence-version 2
```


## <a name="next-steps"></a>Seuraavat vaiheet

- Katso lisätietoja hallitsemisesta Azure avaimen säilö, mukaan lukien poistaminen salausavaimet ja vaults, [hallita avaimen säilö käyttämällä CLI](../key-vault/key-vault-manage-with-cli.md).
- Saat lisätietoja salauksen, kuten valmistellaan salattuja mukautetun AM Azure-lataaminen [Azure salauksen](../security/azure-security-disk-encryption.md).
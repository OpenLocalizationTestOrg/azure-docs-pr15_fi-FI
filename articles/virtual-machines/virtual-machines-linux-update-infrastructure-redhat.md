<properties
   pageTitle="Punainen on päivitys infrastruktuuri (RHUI) | Microsoft Azure"
   description="Lue lisätietoja punainen on päivitys infrastruktuuri (RHUI) tarvittaessa punainen on Enterprise Linux esiintymien Microsoft Azure-"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="BorisB2015"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="10/14/2016"
   ms.author="borisb"/>

# <a name="red-hat-update-infrastructure-rhui-for-on-demand-red-hat-enterprise-linux-vms-in-azure"></a>Tarvittaessa punainen on Enterprise Linux VMs Azure-tietokannassa on punainen päivityksen infrastruktuuri (RHUI)

Luotu tarvittaessa punainen on Enterprise Linux (RHEL) kuvista käytettävissä Azure Marketplacesta näennäiskoneiden on rekisteröity käyttämään punainen on päivitys infrastruktuuri (RHUI) käyttöön Azure-tietokannassa.  Tarvittaessa RHEL esiintymät pääsevät alueellisen yum säilöön ja vastaanottaa lisäävät päivitykset.

Luettelo yum säilöön, joka hallitsee RHUI, on määritetty RHEL esiintymän valmistelun aikana. Sinun ei tarvitse tehdä mitään lisämäärityksiä - Suorita `yum update` jälkeen RHEL esiintymää on valmis uusimmat päivitykset.

> [AZURE.NOTE] Azure RHUI infrastruktuuri on päivitetty viimeksi (syyskuu 2016) ja vaatii muutoksia aiemmin RHEL kopioita määrityksessä Azure RHUI keskeytyksettä käyttämään. Lisätietoja on RHUI Azure infrastruktuurin päivitys-osassa.


## <a name="rhui-azure-infrastructure-update"></a>RHUI Azure infrastruktuurin päivitys
Syyskuussa 2016 vuodesta Azure on uudella valikoimalla punainen on päivitys infrastruktuuri (RHUI)-palvelimiin. Seuraavissa palvelimissa otettu käyttöön [Azure liikenteen hallinta]( https://azure.microsoft.com/services/traffic-manager/) , jotta yhden loppupisteen (rhui 1.micrsoft.com) voidaan käyttää minkä tahansa AM alueen riippumatta. He myös käyttää SSL-varmenne, joka on ketjutettu tunnettujen varmenteen myöntäjä (Baltimore pääkansio). Puhelujen automaattinen päivitys olisi vaarallinen jotkin asiakkaat, joilla on käyttöoikeusluettelot tai mukautettuja reititys taulukoita RHUI päivitys-palvelimia, joten tämä päivitys "sisältyy." Nämä palvelimiin onboarding manuaalisesti ovat käytettävissä tämän sivun ja valmis komentosarjan onboarding automaattinen tavalla (yhteydessä yksittäisiä vaiheita vahvistusta). Uusi RHEL PAYG kuvia, Azure Marketplacesta (versiot päivätty syyskuussa 2016 tai uudempi) osoittaa automaattisesti uusia Azure RHUI-palvelimia ja muita toimia ei tarvita.

### <a name="the-new-azure-rhui-infrastructure-onboarding-timeline"></a>Uusi Azure RHUI infrastruktuurin onboarding aikajana

| Päivämäärä | Huomautus |
| --- | --- |
|2016 22 syyskuu | RHUI palvelimet ja asenna ohjeita käytettäväksi. Ottaa käyttöön käyttämällä uutta (syyskuu 2016 päivätty) VMs RHEL PAYG marketplace kuvia käyttää uusia RHUI-palvelimia automaattisesti, mutta aiemmin VMs ovat "sisältyy"
|1 marraskuun 2016 | Aiempien versioiden RHEL PAYG AM-kuvat (joka käyttää vanha Azure RHUI-palvelimia) poistetaan Azure Marketplace-valikoimasta
|16 tammikuussa-2017 | Vanha Azure RHUI palvelimet poistettu käytöstä. Päivitä kaikkien tarvittavien PAYG-RHEL VMs tällä hetkellä pitämään Azure RHUI pääsy

### <a name="the-ips-for-the-new-rhui-content-delivery-servers-are"></a>Uudet RHUI sisällön toimitus-palvelimet IP-osoitteet on

```
# Azure Global
13.91.47.76
40.85.190.91
52.187.75.218
52.174.163.213

# Azure US Government
13.72.186.193
```

### <a name="manual-update-procedure-to-use-the-new-azure-rhui-servers"></a>Manuaalinen päivitysprosessiin uusien Azure RHUI palvelinten käyttäminen

Lataa (joko kääntö) julkinen avain allekirjoitus

```
curl -o RPM-GPG-KEY-microsoft-azure-release https://download.microsoft.com/download/9/D/9/9d945f05-541d-494f-9977-289b3ce8e774/microsoft-sign-public.asc 
```

Varmista ladatut tunnus

```
gpg --list-packets --verbose < RPM-GPG-KEY-microsoft-azure-release
```

Tarkista tulosteen, tarkista `keyid` ja `user ID packet`:

```
Version: GnuPG v1.4.7 (GNU/Linux)
:public key packet:
        version 4, algo 1, created 1446074508, expires 0
        pkey[0]: [2048 bits]
        pkey[1]: [17 bits]
        keyid: EB3E94ADBE1229CF
:user ID packet: "Microsoft (Release signing) <gpgsecurity@microsoft.com>"
:signature packet: algo 1, keyid EB3E94ADBE1229CF
        version 4, created 1446074508, md5len 0, sigclass 0x13
        digest algo 2, begin of digest 1a 9b
        hashed subpkt 2 len 4 (sig created 2015-10-28)
        hashed subpkt 27 len 1 (key flags: 03)
        hashed subpkt 11 len 5 (pref-sym-algos: 9 8 7 3 2)
        hashed subpkt 21 len 3 (pref-hash-algos: 2 8 3)
        hashed subpkt 22 len 2 (pref-zip-algos: 2 1)
        hashed subpkt 30 len 1 (features: 01)
        hashed subpkt 23 len 1 (key server preferences: 80)
        subpkt 16 len 8 (issuer key ID EB3E94ADBE1229CF)
        data: [2047 bits]
```

Asenna julkinen avain

```
sudo install -o root -g root -m 644 RPM-GPG-KEY-microsoft-azure-release /etc/pki/rpm-gpg
sudo rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-microsoft-azure-release
```

Lataa ja tarkista JÄLLEENMYYNTIHINNAN-asiakasohjelman asentaminen

Lataa: N RHEL 6

```
curl -o azureclient.rpm https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel6/rhui-azure-rhel6-2.0-2.noarch.rpm 
```

Saat RHEL 7

```
curl -o azureclient.rpm https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel7/rhui-azure-rhel7-2.0-2.noarch.rpm  
```

Tarkista:

```
rpm -Kv azureclient.rpm
```

Tarkista tulos paketin allekirjoitus on OK

```
azureclient.rpm:
    Header V3 RSA/SHA256 Signature, key ID be1229cf: OK
    Header SHA1 digest: OK (927a3b548146c95a3f6c1a5d5ae52258a8859ab3)
    V3 RSA/SHA256 Signature, key ID be1229cf: OK
    MD5 digest: OK (c04ff605f82f4be8c96020bf5c23b86c)
```

Asenna JÄLLEENMYYNTIHINNAN

```
sudo rpm -U azureclient.rpm
```

Valmistumisen Varmista, että voit käyttää Azure RHUI lomaketta AM

### <a name="all-in-one-script-for-automating-the-above-task"></a>Kaikki one komentosarjan yllä tehtävien automatisointi
Käytä seuraavaa komentosarjaa tarpeen mukaan voit automatisoida päivittämistä tarvittavien VMs uusi Azure RHUI-palvelimiin.

```
# Download key
curl -o RPM-GPG-KEY-microsoft-azure-release https://download.microsoft.com/download/9/D/9/9d945f05-541d-494f-9977-289b3ce8e774/microsoft-sign-public.asc 

# Validate key
if ! gpg --list-packets --verbose < RPM-GPG-KEY-microsoft-azure-release | grep -q "keyid: EB3E94ADBE1229CF"; then
    echo "Keyfile azure.asc NOT valid. Exiting."
    exit 1
fi

# Install Key
sudo install -o root -g root -m 644 RPM-GPG-KEY-microsoft-azure-release /etc/pki/rpm-gpg
sudo rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-microsoft-azure-release

# Download RPM package
if grep -q "release 7" /etc/redhat-release; then
    ver=7
elif  grep -q "release 6" /etc/redhat-release; then
    ver=6
else
    echo "Version not supported, exiting"
    exit 1
fi

url=https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel$ver/rhui-azure-rhel$ver-2.0-2.noarch.rpm
curl -o azureclient.rpm "$url"

# Verify package
if ! rpm -Kv azureclient.rpm | grep -q "key ID be1229cf: OK"; then
    echo "RPM failed validation ($url)"
    exit 1
fi

# Install package
sudo rpm -U azureclient.rpm
```

## <a name="rhui-overview"></a>RHUI yleiskatsaus
[Punainen on päivitys infrastruktuuri](https://access.redhat.com/products/red-hat-update-infrastructure) on erittäin skaalattava ratkaisun yum säilöön sisällön punainen on Enterprise Linux cloud esiintymät, joita isännöidään punainen on sertifioitu cloud tarjoajien hallinta. Seuraavat massa-projektin pohjalta RHUI avulla cloud tarjoajien palvelua paikallisesti peilikuvat sisältö on punainen isännöimä säilöön, luoda mukautetun säilöjen tietoihin omia sisältöä ja saataville suurelle ryhmälle seuraavista loppukäyttäjät järjestelmän sisällön toimittamista kuormituksen kautta näiden säilöjen tietoihin.

## <a name="regions-where-rhui-is-available"></a>Alueet, joissa RHUI on käytettävissä
RHUI on käytettävissä kaikilla alueilla, jossa RHEL tarvittaessa kuvat ovat käytettävissä. Siinä kaikki julkisen alueiden Azure US Government alueiden ja [Azure tila dashboard](https://azure.microsoft.com/status/) -sivun luettelossa. RHUI käytön valmistelu RHEL tarvittaessa kuvista VMs sisältyy niiden hinta. Lisää alue/kansallisia cloud käytettävyys päivitetään olemme Laajenna RHEL tarvittaessa käytettävyys tulevaisuudessa.

> [AZURE.NOTE] Azure isännöimä RHUI käyttöä on rajoitettu VMs kuluessa [Microsoft Azure palvelinkeskuksen IP-alueita](https://www.microsoft.com/download/details.aspx?id=41653).

## <a name="get-updates-from-another-update-repository"></a>Päivitysten noutaminen toiseen päivityksen säilöön

Jos haluat päivittää eri päivitys säilöstä (sijaan Azure isännöimä RHUI) tarvitset unregister kopioita RHUI- ja rekisteröi ne uudelleen haluamasi päivitys-infrastruktuuriin (esimerkiksi punainen on satelliitti tai punainen on asiakkaan Portal CDN). Sinun on sopivan punaista on tilaukset näistä palveluista ja rekisteröinti [Azure-tietokannassa on punainen Pilvikäyttöä](https://access.redhat.com/ecosystem/partners/ccsp/microsoft-azure)varten.

Unregister RHUI ja rekisteröi päivitys-infrastruktuurin seuraa vaiheet alla.

1.  Muokkaa /etc/yum.repos.d/rh-cloud.repo ja muuta kaikki `enabled=1` , `enabled=0`. Esimerkki:

        # sed -i 's/enabled=1/enabled=0/g' /etc/yum.repos.d/rh-cloud.repo

2.  Muokkaa /etc/yum/pluginconf.d/rhnplugin.conf ja muuta `enabled=0` , `enabled=1`.
3.  Rekisteröi sitten haluttu infrastruktuuriin, kuten punainen on asiakkaan Portal. Noudata punainen on ratkaisu opas [Rekisteröi](https://access.redhat.com/solutions/253273)ja tilaa järjestelmän punainen on asiakas-portaaliin.

> [AZURE.NOTE] Azure isännöimä RHUI pääsy sisältyy RHEL ryhmävakuutussopimukset (PAYG) kuva hinnan. Rekisteröintiä PAYG-RHEL AM from Azure isännöimä RHUI Muunna virtuaalikoneen tuo-Your-omistaja-käyttöoikeus (BYOL) tyyppi AM ja näin ollen voit voi olla lisäkustannuksia kaksinkertainen Jos saman AM rekisteröityä päivitykset toisesta lähteestä. 
> 
> Jos haluat käyttää päivitys-infrastruktuurin muu kuin johdonmukaisesti Azure isännöimä RHUI harkitse luominen ja käyttöönotto (BYOL tyyppi) omia kuvia, [Luo ja lataa punainen on perustuva virtual tietokoneen Azure](virtual-machines-linux-redhat-create-upload-vhd.md) -artikkelissa kuvatulla tavalla.

## <a name="next-steps"></a>Seuraavat vaiheet
Punainen on Enterprise Linux AM luominen Azure Marketplace ryhmävakuutussopimukset kuva ja suorituskykykertoimen Azure isännöimä RHUI Siirry [Azure Marketplacesta](https://azure.microsoft.com/marketplace/partners/redhat/). Osaat käyttää `yum update` RHEL-esiintymän ilman mitään Lisäasetukset.
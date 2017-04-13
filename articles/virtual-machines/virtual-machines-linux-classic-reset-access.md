<properties
        pageTitle="Linux AM salasanan palauttaminen ja SSH key CLI | Microsoft Azure"
        description="Opi käyttämään VMAccess tunniste-Azure käyttöliittymä (CLI) SSH-näppäintä tai Linux AM salasanan palauttaminen ja korjata SSH määritysten levyn yhtenäisyyden tarkistaminen"
        services="virtual-machines-linux"
        documentationCenter=""
        authors="cynthn"
        manager="timlt"
        editor=""
        tags="azure-service-management"/>

<tags
        ms.service="virtual-machines-linux"
        ms.workload="infrastructure-services"
        ms.tgt_pltfrm="vm-linux"
        ms.devlang="na"
        ms.topic="article"
        ms.date="06/14/2016"
        ms.author="cynthn"/>

# <a name="how-to-reset-a-linux-vm-password-or-ssh-key-fix-the-ssh-configuration-and-check-disk-consistency-using-the-vmaccess-extension"></a>SSH-näppäintä tai Linux AM salasanan palauttaminen, korjata SSH määritykset ja tarkista levyn yhdenmukaisuuden käyttämällä VMAccess-tunniste


Jos et pysty muodostamaan yhteyttä Linux virtual tietokoneen Azure unohtuneen salasanan, virheellinen suojattu runko (SSH)-näppäintä, koska tai SSH määritys-ongelma käyttää VMAccessForLinux-tunniste Azure-CLI vaihtamaan salasanan tai SSH-näppäintä, korjaa SSH määritykset ja tarkista levyn yhdenmukaisuuden. 

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Lue, miten [Resurssienhallinta-mallin seuraavasti](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess).

Azure-CLI ja käytät **azure AM tunniste set** -komennon komentorivivalitsimet-liittymästä (Bash, pääte-komentokehote) komentoja. Suorita **azure ohjeen AM tunniste määrittää** yksityiskohtaisia laajennuksen käytön.

Azure-CLI voit tehdä seuraavat toimet:

+ [Salasanan palauttaminen](#pwresetcli)
+ [Palauta SSH avain](#sshkeyresetcli)
+ [Palauta salasana ja SSH-avain](#resetbothcli)
+ [Uusi sudo käyttäjätilin luominen](#createnewsudocli)
+ [Palauttaa SSH määritykset](#sshconfigresetcli)
+ [Käyttäjän poistaminen](#deletecli)
+ [Näytä tila VMAccess-tunniste](#statuscli)
+ [Lisätty levyjen yhtenäisyyden tarkistaminen](#checkdisk)
+ [Lisätty levyille Linux AM korjaaminen](#repairdisk)


## <a name="prerequisites"></a>Edellytykset

Tarvitset seuraavasti:

- Tarvitset [Azure-CLI asentaminen](../xplat-cli-install.md) ja [yhteyden muodostaminen tilauksen](../xplat-cli-connect.md) käyttämään tilisi Azure resursseja.
- Määritä perinteinen käyttöönoton mallin suunnittelutilassa kirjoittamalla komentoriville seuraava komento:
        
        azure config mode asm
        
- On uusi salasana tai joukko SSH näppäimet, jos haluat palauttaa jompikumpi. Sinun ei tarvitse näitä, jos haluat palauttaa SSH määritykset.


## <a name="pwresetcli"></a>Salasanan palauttaminen

1. Luo tiedosto nimeltä PrivateConf.json viivat. Korvaa sulkeiden ja & #60; paikkamerkin & #62; arvot omilla tiedoillasi.

        {
        "username":"<currentusername>",
        "password":"<newpassword>",
        "expiration":"<2016-01-01>"
        }

2. Suorita tämä komento korvaaminen virtuaalikoneen varten & #60; AM nimi & #62; nimi.

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* –-private-config-path PrivateConf.json

## <a name="sshkeyresetcli"></a>Palauta SSH avain

1. Luo tiedosto nimeltä PrivateConf.json nämä sisällöllä. Korvaa sulkeiden ja & #60; paikkamerkin & #62; arvot omilla tiedoillasi.

        {
        "username":"<currentusername>",
        "ssh_key":"<contentofsshkey>"
        }

2. Suorita tämä komento korvaaminen virtuaalikoneen varten & #60; AM nimi & #62; nimi.

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

## <a name="resetbothcli"></a>Palauta salasana ja SSH avain

1. Luo tiedosto nimeltä PrivateConf.json nämä sisällöllä. Korvaa sulkeiden ja & #60; paikkamerkin & #62; arvot omilla tiedoillasi.

        {
        "username":"<currentusername>",
        "ssh_key":"<contentofsshkey>",
        "password":"<newpassword>"
        }

2. Suorita tämä komento korvaaminen virtuaalikoneen varten & #60; AM nimi & #62; nimi.

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

## <a name="createnewsudocli"></a>Uusi sudo käyttäjätilin luominen

Jos olet unohtanut käyttäjänimi, voit luoda uuden sudo myöntäjän VMAccess. Tässä tapauksessa aiemmin käyttäjänimeä ja salasanaa ei muuteta.

Jos haluat luoda uuden sudo käyttäjän salasanalla, [salasanan](#pwresetcli) komentosarjan käyttäminen ja määritä uusi nimi.

Luo uusi sudo käyttäjä SSH avaimen käytön, [Palauta SSH avain](#sshkeyresetcli) komentosarjan käyttäminen ja määritä uusi nimi.

[Palauta salasana ja SSH-näppäintä](#resetbothcli) avulla voit myös luoda uuden käyttäjän salasanan ja SSH avaimen access.

## <a name="sshconfigresetcli"></a>Palauttaa SSH määritykset

Jos SSH määritys on toivotun tilan, myös AM access saattavat menettää. Voit palauttaa määritykset oletustilaan VMAccess-tunniste. Voit tehdä vain haluat määrittää "reset_ssh" käyttäjäavainten "True". Tunnisteen SSH palvelimeen uudelleen, Avaa oman AM SSH porttiin ja SSH määritykset Palauta oletusarvot. Käyttäjätilin (nimi, salasana tai SSH näppäimet) ei voi muuttaa.

> [AZURE.NOTE] SSH määritystiedosto, joka saa Palauta sijaitsee /etc/ssh/sshd_config.

1. Luo tiedosto nimeltä PrivateConf.json sisältötyypin kanssa.

        {
        "reset_ssh":"True"
        }

2. Suorita tämä komento korvaaminen virtuaalikoneen varten & #60; AM nimi & #62; nimi. 

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

## <a name="deletecli"></a>Käyttäjän poistaminen

Jos haluat poistaa käyttäjätilin kirjautumatta sisään, AM suoraan, voit käyttää tätä komentosarjaa.

1. Luo tiedosto nimeltä PrivateConf.json sisältötyypin poistaminen käyttäjänimi & #60; usernametoremove & #62; korvaaminen kanssa. 

        {
        "remove_user":"<usernametoremove>"
        }

2. Suorita tämä komento korvaaminen virtuaalikoneen varten & #60; AM nimi & #62; nimi. 

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

## <a name="statuscli"></a>Näytä tila VMAccess-tunniste

Näyttötila VMAccess tunniste-komennon suorittaminen

        azure vm extension get

## <a name="a-namecheckdiskacheck-consistency-of-added-disks"></a>< name = "checkdisk" <</a>lisätty levyjen yhtenäisyyden tarkistaminen

Kaikki levyjen Linux virtuaalikoneen fsck suorittamiseen tarvitset seuraavasti:

1. Luo tiedosto nimeltä PublicConf.json sisältötyypin kanssa. Tarkista levy on totuusarvo, sinulta kysytään, haluatko tarkistaa levyjen liitetty virtuaalikoneen vai ei. 

        {   
        "check_disk": "true"
        }

2. Suorita tällä komennolla voit suorittaa, korvaaminen virtuaalikoneen varten & #60; AM nimi & #62; nimi.

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --public-config-path PublicConf.json 

## <a name='repairdisk'></a>Korjaa levyjen 

Korjaa levyjä, jotka eivät käyttöönottaminen tai on asennustapa määritysvirheet Palauta Linux virtuaalikoneen asennustapa-määritysten VMAccess tunnisteen avulla. Korvaaminen levyn & #60; yourdisk & #62; nimi.

1. Luo tiedosto nimeltä PublicConf.json sisältötyypin kanssa. 

        {
        "repair_disk":"true",
        "disk_name":"<yourdisk>"
        }

2. Suorita tällä komennolla voit suorittaa, korvaaminen virtuaalikoneen varten & #60; AM nimi & #62; nimi.

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --public-config-path PublicConf.json



## <a name="next-steps"></a>Seuraavat vaiheet

* Jos haluat käyttää Azure PowerShellin cmdlet-komennot tai Azure Resurssienhallinta malleja vaihtamaan salasanan tai SSH avaimen, korjata SSH-määritys ja levyn yhtenäisyyden tarkistaminen, [VMAccess tunniste ohjeet GitHub](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess). 

* Voit myös käyttää [Azure portal](https://portal.azure.com) salasanan tai Linux AM SSH avaimen käyttöön perinteinen käyttöönotto-mallissa. Portaalin Älä tällä hetkellä voi käyttää tätä varten Linux AM käyttöön resurssien hallinnan käyttöönottomalli.

* Lisätietoja [virtuaalikoneen tunnisteet ja ominaisuuksien](virtual-machines-linux-extensions-features.md) käyttämisestä Azuren näennäiskoneiden AM tunnisteet.

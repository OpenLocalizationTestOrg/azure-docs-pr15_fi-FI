<properties
   pageTitle="Johdanto FreeBSD Azure | Microsoft Azure"
   description="Opi käyttämään FreeBSD näennäiskoneiden Azure-"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="KylieLiang"
   manager="timlt"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="08/27/2016"
   ms.author="kyliel"/>

# <a name="introduction-to-freebsd-on-azure"></a>Valitse Azure FreeBSD esittely
Tässä artikkelissa on yleisiä tietoja käynnissä FreeBSD virtual koneen Azure-tietokannassa.

## <a name="overview"></a>Yleiskatsaus
Microsoft Azure FreeBSD on Lisäasetukset käyttöjärjestelmä käytettävä power Moderni palvelimet-työasemien, ja upotettu ympäristöissä. Kuva FreeBSD 10.3 tarjoaa Microsoft Corporation, ja se on käytettävissä Azure-tietokannassa. Se perustuu FreeBSD 10.3 versio ja Azure AM Vieras agentti [2.1.4](https://github.com/Azure/WALinuxAgent/releases/tag/v2.1.4) on asennettu. Agentti on vastuussa välisen FreeBSD AM ja Azure kangasta toimintoja, kuten valmistelussa AM ensimmäisellä käyttökerralla (käyttäjänimi, salasana, isäntänimi jne.) ja toiminnot Valikoiva AM-laajennukset on otettu käyttöön.
FreeBSD tulevissa versioissa, kuten strategia kannattaa pysyt ajan tasalla ja käytettäväksi uusimmista julkaisuista pian, kun ne on julkaistu FreeBSD release suunnitteluryhmät työryhmän. Tulevan version on [FreeBSD 11](https://www.freebsd.org/releases/11.0R/schedule.html).

## <a name="deploying-a-freebsd-virtual-machine"></a>FreeBSD virtual koneen käyttöönotto
Käyttöönotto FreeBSD virtual koneen on onnistuu helposti ja nopeasti käyttämällä kuvan [Azure Marketplacesta](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/).

## <a name="vm-extensions-for-freebsd"></a>FreeBSD AM tunnisteet
Seuraavassa on lueteltu tuetut FreeBSD AM tunnisteet.

### <a name="vmaccess"></a>VMAccess

[VMAccess](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) -tunniste tehdä seuraavia toimia:

- Palauta alkuperäinen sudo-käyttäjän salasanan.
- Luo uusi sudo käyttäjä määritetty salasanalla.
- Määritä annettu-näppäimen kanssa julkisen host-näppäintä.
- Palauta julkisen host avain aikana AM valmistelu Jos Host (isäntä)-näppäin ei ole toimitettu.
- Avaa SSH portti (22) ja palauttaa sshd_config, jos reset_ssh on määritetty tosi.
- Poista olemassa olevaa käyttäjää.
- Tarkista levyjä.
- Korjaa levy, joka on lisätty.

### <a name="customscript"></a>CustomScript

[CustomScript](https://github.com/Azure/azure-linux-extensions/tree/master/CustomScript) -tunniste tehdä seuraavia toimia:

- Jos, lataa mukautettuja komentosarjoja Azuren tallennustilaan tai ulkoisen julkisen storage (esimerkiksi GitHub).
- Suorita tapahtuma-kohdan komentosarja.
- Tue sisäisiä komentoja.
- Muuntaa Windows-tyylin rivi-käyttöliittymän ja Python komentosarjoja automaattisesti.
- Poista Tuoterakenteen käyttöliittymän ja Python komentosarjat automaattisesti.
- Suojaa luottamukselliset tiedot CommandToExecute.

## <a name="authentication-user-names-passwords-and-ssh-keys"></a>Todentaminen: käyttäjänimet, salasanat ja SSH näppäimet
Kun olet luomassa FreeBSD virtual koneen, Azure-portaalissa, sinun on annettava käyttäjänimi, salasana tai SSH julkinen avain.
Käyttöönoton FreeBSD virtual tietokoneen Azure käyttäjänimet on vastaa järjestelmätilillä nimet (UID < 100) jo virtuaalikoneen ("pääkansio," esimerkiksi).
Tällä hetkellä tuetaan vain RSA SSH avain. Monirivinen SSH näppäintä on "---Aloita SSH2 JULKISELLA avaimella---" alussa ja lopussa "---SSH2 julkinen avain---".

## <a name="obtaining-superuser-privileges"></a>Pääkäyttäjän oikeudet hankkiminen
Sellaisten tili on käyttäjätili, jolla on määritetty Azure virtuaalikoneen esiintymän käyttöönoton aikana. Sudo paketti asennettiin julkaistun FreeBSD kuvassa.
Kun olet kirjautunut sisään tämän käyttäjätilin kautta, voit suorittaa komennot kuin ensisijaisen käyttämällä komennon syntaksissa.

    # sudo <COMMAND>

Voit hankkia pääkansion shell myös käyttämällä sudo -s.

## <a name="next-steps"></a>Seuraavat vaiheet
- Siirry [Azure Marketplacesta](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/) FreeBSD AM luomiseen.
- Jos haluat tuoda oman FreeBSD Azure-viitata [luominen ja lataa FreeBSD Näennäiskiintolevyn Azure avulla](../virtual-machines-linux-classic-freebsd-create-upload-vhd.md).

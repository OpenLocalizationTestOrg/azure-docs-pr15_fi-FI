<properties 
    pageTitle="Pääkansio oikeudet käyttää Linux näennäiskoneiden | Microsoft Azure" 
    description="Opettele käyttämään pääkansion oikeudet Linux virtual tietokoneeseen Azure-tietokannassa." 
    services="virtual-machines-linux" 
    documentationCenter="" 
    authors="szarkos" 
    manager="timlt" 
    editor=""
    tags="azure-service-management,azure-resource-manager" />

<tags 
    ms.service="virtual-machines-linux" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/17/2016" 
    ms.author="szark"/>


# <a name="using-root-privileges-on-linux-virtual-machines-in-azure"></a>Pääkansio oikeudet käyttäminen Linux näennäiskoneiden Azure-tietokannassa

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Oletusarvon mukaan `root` Linux näennäiskoneiden Azure-käyttäjä ei ole käytössä. Käyttäjät voivat suorittaa komennot oikeuksin käyttämällä `sudo` komento. Kuitenkin kokemus saattavat vaihdella sen mukaan, kuinka järjestelmä on valmistelun yhteydessä.

1. **SSH avain ja salasana tai vain** - virtuaalikoneen on valmisteltu joko sertifikaatilla (`.CER` tiedoston) tai SSH avain sekä salasanaa, tai vain käyttäjänimi ja salasana. Tässä tapauksessa `sudo` pyytää käyttäjän salasana ennen komennon suorittamista.

2. **Vain SSH avain** - virtuaalikoneen on valmisteltu sertifikaatilla (`.cer`, `.pem`, tai `.pub` tiedoston) tai SSH avaimen, mutta ei ole salasana.  Tässä tapauksessa `sudo` **eivät ole** käyttäjän salasana ennen komennon suorittamista kysy.


## <a name="ssh-key-and-password-or-password-only"></a>SSH avain ja salasanan tai vain salasana

Kirjaudu sisään SSH-näppäintä tai salasanan todennusta Linux virtuaalikoneen ja suorita komentojen `sudo`, esimerkiksi:

    # sudo <command>
    [sudo] password for azureuser:

Tällöin käyttäjää pyydetään salasanaa. Kun olet lisännyt salasana `sudo` kanssa-komennon suorittaminen `root` oikeudet.

Voit myös ottaa passwordless sudo muokkaamalla `/etc/sudoers.d/waagent` tiedoston, kuten:

    #/etc/sudoers.d/waagent
    azureuser ALL = (ALL) NOPASSWD: ALL

Tämä muutos Salli passwordless sudo käyttäjän "azureuser".

## <a name="ssh-key-only"></a>SSH avaimen vain

Kirjaudu sisään käyttämällä SSH avaimen todennus Linux virtuaalikoneen ja suorita komentojen `sudo`, esimerkiksi:

    # sudo <command>

Tässä tapauksessa käyttäjän on **ei** sinua pyydetään antamaan salasana. Kun painamalla `<enter>`, `sudo` kanssa-komennon suorittaminen `root` oikeudet.

 

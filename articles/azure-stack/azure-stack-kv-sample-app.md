<properties
    pageTitle="Salli revtrieve Azure pinon avaimen säilö tietoja sovelluksen | Microsoft Azure"
    description="Esimerkki-sovelluksen käyttäminen Azure pinon avaimen säilö"
    services="azure-stack"
    documentationCenter=""
    authors="rlfmendes"
    manager="natmack"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/26/2016"
    ms.author="ricardom"/>

# <a name="run-the-sample-application-for-key-vault"></a>Suorita avaimen säilö otoksen hakeminen 

Tässä oppaassa hakea tietoja ja salasanat avain säilöstä malli-sovellusta käytetään.

## <a name="download-the-samples-and-prepare"></a>Lataa mallit ja valmisteleminen

Lataa [Azure avaimen säilö asiakkaan näytteiden sivun](https://www.microsoft.com/en-us/download/details.aspx?id=45343)Azure avaimen säilö asiakkaan mallit.

Pura paikalliseen tietokoneeseen .zip-tiedoston sisällön.

Lue **README.md** -tiedosto (tämä on tekstitiedosto) ja noudata sitten ohjeita.

## <a name="run-sample-1--hellokeyvault"></a>Esimerkki #1 – HelloKeyVault suorittaminen
HelloKeyVault on console-sovellus, jossa käydään läpi on avaimen säilö tukemat skenaariota:

  1. Luo/tuo-näppäintä (:: HSM tai ohjelmisto-näppäin)
  2. Salaa salaisuus, sisällön avaimen avulla
  3. Rivitä sisällön avain avaimen säilö avaimen avulla
  4. Muun sisällön avain
  5. Salauksen toiminta
  6. Määritä salaisuus

Konsolisovelluksen suoritetaan ilman muutoksia, sillä erotuksella, että tarvittavat määritykset ovat App.Config päivitetään mukaan seuraavasti:

1. Päivitä sovellus määrityksiä HelloKeyVault\App.config säilö URL-osoite ja lainan Sovellustunnus ja salaisuus. Tietoja voi luoda myös **scripts\GetAppConfigSettings.ps1**avulla.
2. Päivitä GetAppConfigSettings.ps1 pakollinen muuttujien arvoihin.
3. Käynnistä Windows PowerShell-ikkunassa.
4. Suorita GetAppConfigSettings.ps1 komentosarja PowerShell-ikkunassa.
5. Kopioi komentosarja tulokset HelloKeyVault\App.config-tiedostoon.


## <a name="next-steps"></a>Seuraavat vaiheet

[Ottaa käyttöön AM avaimen säilö salasanalla](azure-stack-kv-deploy-vm-with-secret.md)

[AM avaimen säilö varmenteella käyttöönotto](azure-stack-kv-push-secret-into-vm.md)
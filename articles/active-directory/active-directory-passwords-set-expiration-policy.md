<properties
    pageTitle="Määritä salasanan vanhenemiskäytännöistä Azure Active Directoryn | Microsoft Azure"
    description="Lue, miten voit tarkistaa vanhenemiskäytännöistä ja vaihtaa käyttäjän salasanan vanhentumisesta yksinään tai joukkona Azure Active directory-salasanojen"
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="curtand"/>


# <a name="set-password-expiration-policies-in-azure-active-directory"></a>Määrittää salasanan vanhenemiskäytännöistä Azure Active Directoryssa

> [AZURE.IMPORTANT] **Oletko tähän koska sinulla on ongelmia kirjautumisessa?** Jos näin on, [seuraavalla siitä, miten voit muuttaa ja oman salasanan](active-directory-passwords-update-your-own-password.md).

Microsoftin pilvipalvelussa, Yleinen järjestelmänvalvoja ole olevaksi salasanojen määrittäminen Microsoft Active Directory moduulin Windows PowerShellin Azure avulla. Windows PowerShellin cmdlet-komennot avulla voit myös poistaa ei-vanhene määritysten tai Nähdäksesi käyttäjän salasana on määritetty ei vanhene. Tämä artikkeli tarjoaa ohjeen pilvipalveluihin, kuten Microsoft Intunella ja Office 365: ssä, jotka perustuvat Microsoft Azure Active Directory käyttäjätiedot ja hakemisto-palveluja varten.

  > [AZURE.NOTE] Vain käyttäjätilit, joille ei ole synkronoitu hakemistosynkronoinnin salasanat voi määrittää ei olevaksi. Saat lisätietoja hakemistosynkronoinnin [hakemistosynkronoinnin ohjeet](https://msdn.microsoft.com/library/azure/hh967642.aspx)aiheiden luettelo.

Jos haluat käyttää Windows PowerShellin cmdlet-komennot, ensin on asennettava ne.

## <a name="what-do-you-want-to-do"></a>Mitä haluat tehdä?

- [Salasanan vanhenemiskäytäntöä tarkistaminen](#how-to-check-expiration-policy-for-a-password)

- [Aseta salasana vanhentumaan](#set-a-password-to-expire)

- [Salasanan määrittäminen niin, että se ei ole päättyy](#set-a-password-to-never-expire)

## <a name="how-to-check-expiration-policy-for-a-password"></a>Salasanan vanhenemiskäytäntöä tarkistaminen

1.  Yhdistä Windows PowerShellin yrityksen järjestelmänvalvojan tunnistetiedoilla.

2.  Tee jompikumpi seuraavista:

    - Saat näkyviin onko yhden käyttäjän salasanan määrittäminen aina voimassa olevaksi ajamalla seuraavan cmdlet-komennon avulla käyttäjätunnus (UPN) (esimerkiksi aprilr@contoso.onmicrosoft.com) tai Käyttäjätunnuksella, jos haluat tarkistaa käyttäjän:`Get-MSOLUser -UserPrincipalName <user ID> | Select PasswordNeverExpires`

    - Saat näkyviin "Salasana aina voimassa"-asetuksen kaikille käyttäjille ajamalla seuraavan cmdlet-komennon:`Get-MSOLUser | Select UserPrincipalName, PasswordNeverExpires`

## <a name="set-a-password-to-expire"></a>Aseta salasana vanhentumaan

1.  Yhdistä Windows PowerShellin yrityksen järjestelmänvalvojan tunnistetiedoilla.

2.  Tee jompikumpi seuraavista:

    - Voit määrittää yhden käyttäjän salasanan niin, että salasana vanhenee ajamalla seuraavan cmdlet-komennon avulla käyttäjätunnus (UPN) tai käyttäjän käyttäjätunnusta:`Set-MsolUser -UserPrincipalName <user ID> -PasswordNeverExpires $false`

    - Voit määrittää organisaation kaikkien käyttäjien salasanat, niin, että ne päättyy, käytä seuraavan cmdlet-komennon:`Get-MSOLUser | Set-MsolUser -PasswordNeverExpires $false`

## <a name="set-a-password-to-never-expire"></a>Aseta salasana aina voimassa olevaksi

1. Yhdistä Windows PowerShellin yrityksen järjestelmänvalvojan tunnistetiedoilla.

2.  Tee jompikumpi seuraavista:

    - Voit määrittää yhden käyttäjän aina voimassa olevaksi ajamalla seuraavan cmdlet-komennon avulla käyttäjätunnus (UPN) tai käyttäjän käyttäjätunnusta:`Set-MsolUser -UserPrincipalName <user ID> -PasswordNeverExpires $true`

    - Voit määrittää kaikkien käyttäjien salasanat organisaation aina voimassa olevaksi ajamalla seuraavan cmdlet-komennon:`Get-MSOLUser | Set-MsolUser -PasswordNeverExpires $true`

## <a name="next-steps"></a>Seuraavat vaiheet

* **Oletko tähän koska sinulla on ongelmia kirjautumisessa?** Jos näin on, [seuraavalla siitä, miten voit muuttaa ja oman salasanan](active-directory-passwords-update-your-own-password.md).

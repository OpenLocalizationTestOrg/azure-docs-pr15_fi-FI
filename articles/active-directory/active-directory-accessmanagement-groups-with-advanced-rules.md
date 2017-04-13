
<properties
    pageTitle="Määritteet avulla voit luoda Monitasoisempia sääntöjä | Microsoft Azure"
    description="Miten-, Muokkaa Luo lisäsääntöjä, ryhmän, mukaan lukien tuetut lausekkeiden säännön operaattorit ja parametrit."
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
    ms.date="08/15/2016"
    ms.author="curtand"/>


# <a name="using-attributes-to-create-advanced-rules"></a>Luoda Monitasoisempia sääntöjä määritteet avulla

Azure perinteinen portaalin avulla voit luoda Monitasoisempia sääntöjä, jotta monimutkaisia määrite perustuvan dynaamisen jäsenyydet Azure Active Directory (Azure AD)-ryhmä.  

Kun kaikki käyttäjän muutoksen määritteitä, järjestelmä ilmoittaa kaikki kansiossa, voit tarkastella, jos käyttäjä määritettä muutoksen käynnistää jonkin ryhmän dynaaminen ryhmän säännöt Lisää tai poistaa. Jos käyttäjä täyttää säännön ryhmälle, ne lisätään jäseneksi kyseiseen ryhmään. Jos hän ei enää täytä ryhmän jäsen niitä säännön, ne poistetaan jäseniksi kyseiseen ryhmään.

## <a name="to-create-the-advanced-rule"></a>Jos haluat luoda lisäsäännön

1. [Azure perinteinen portal](https://manage.windowsazure.com)Valitse **Active Directory**ja avaa sitten organisaatiosi hakemistoon.

2. Valitse **ryhmät** -välilehti ja avaa sitten ryhmän, jota haluat muokata.

3. Valitse **määritys** -välilehti, **lisäsäännön** -vaihtoehto ja kirjoita tekstiruutuun lisäsäännön.

## <a name="constructing-the-body-of-an-advanced-rule"></a>Kehittynyt sääntö leipätekstiin luominen

Lisäsäännön, voit luoda dynaamisia ryhmien jäsenyyksien on käytettävä binaarinen lauseke, joka koostuu kolmesta osasta ja tuloksena on TOSI tai EPÄTOSI-Tulosta. Kolme osaa on:

- Vasen parametri
- Binaarinen operaattori
- Oikea vakio

Valmis lisäsäännön näyttää seuraavankaltaiselta: (leftParameter binaryOperator "RightConstant"), missä avaavat ja sulkeva Sulje tarvittavat koko binaarinen lauseketta, lainausmerkkejä tarvittavat oikean vakion ja vasen parametrin syntaksi on user.property. Kehittynyt sääntö voi olla useampi kuin yksi binaarinen lausekkeita, jotka on erotettu toisistaan- ja- ja ja - ei loogiset operaattorit.
Seuraavassa on esimerkkejä oikein rakennettava lisäsäännön:

- (user.department - eq "Myynti")- tai (user.department - eq "Markkinointi")
- (user.department - eq "Myynti")- ja - ei (user.jobTitle-sisältää "SDE")

Katso luettelo kaikista tuettujen parametrien ja lausekkeen säännön operaattoreita, seuraavissa osissa.

Oman lisäsäännön leipätekstiin koko pituus voi olla enintään 2 048 merkkiä.

> [AZURE.NOTE]
>Merkkijono ja regex toiminnot ovat kirjainkokoa. Voit suorittaa Null tarkistukset, käyttämällä $null vakiona, kuten, user.department - eq $null.
Merkkijonojen tarjoukset "pitäisi olla ohitettuja käyttämällä" merkki, esimerkiksi user.department - eq \`"Myynti".

## <a name="supported-expression-rule-operators"></a>Tuetut lausekkeiden säännön operaattorit
Seuraavassa taulukossa on lueteltu tuetut lauseke säännön operaattorit kaikki ja niiden syntaksi, jota käytetään lisäsäännön tekstiosassa:

| Operaattori        | Syntaksi         |
|-----------------|----------------|
| Eri suuri kuin      | -ne            |
| Yhtä suuri kuin          | -eq            |
| Ei alkaa | -notStartsWith |
| Alkaa seuraavasti:     | -startsWith    |
| Ei sisällä    | -notContains   |
| Sisältää        | -on      |
| Vastaa       | -notMatch      |
| Vastine           | -vastaa         |


## <a name="query-error-remediation"></a>Kysely-virheen korjaaminen
Seuraavassa taulukossa on lueteltu mahdolliset virheet ja miten voit korjata ne, jos ne sijaitsevat

| Kyselyn jäsennä-virhe     | Virhe käyttö       | Korjattu käyttö             |
|-----------------------|-------------------|-----------------------------|
| Virhe: Määrite ei tueta.                                      | (user.invalidProperty - eq "Value")       | (user.department - eq "value")<br/>Ominaisuuden pitäisi täsmätä vaihtoehto [tuetut ominaisuudet-luettelosta](#supported-properties).                          |
| Virhe: Operaattori ei tue määrite.                       | (user.accountEnabled-on tosi)                                                                               | (user.accountEnabled - eq tosi)<br/>Ominaisuus on boolean-tyypin. Tuetut operaattoreita (-eq tai - ne) totuusarvo tyypin yllä olevasta luettelosta.                                                                                                                                   |
| Virhe: Kyselyn kääntämisen virhe.                                      | (user.department - eq "Myynti")- ja (user.department - eq "Markkinointi")(user.userPrincipalName-match"*@domain.ext") | (user.department - eq "Myynti")- ja (user.department - eq "Markkinointi")<br/>Loogisten operaattorien vastattava yllä tuetut ominaisuudet-luettelosta. (user.userPrincipalName-vastaa ".*@domain.ext")or(user.userPrincipalName -vastaa "@domain.ext$")Error Normaali-lausekkeessa. |
| Virhe: Binaarinen lauseke ei ole oikeassa muodossa.                     | (user.department – eq "Myynti") (user.department - eq "Myynti") (user.department-eq "Myynti")                             | (user.accountEnabled - eq tosi)- ja (user.userPrincipalName-on"alias@domain")<br/>Kyselyssä on useita virheitä. Sulje kuin oikeille.                                                                                                                            |
| Virhe: Tuntematon virhe dynaaminen jäsenyydet määrittäminen. | (user.accountEnabled - eq "True" AND user.userPrincipalName-on"alias@domain")                               | (user.accountEnabled - eq tosi)- ja (user.userPrincipalName-on"alias@domain")<br/>Kyselyssä on useita virheitä. Sulje kuin oikeille.                                                                                                                            |

## <a name="supported-properties"></a>Tuetut ominaisuudet
Kaikki käyttäjän ominaisuuksia, voit käyttää omaa lisäsäännön ovat seuraavat:

### <a name="properties-of-type-boolean"></a>Boolean-tyypin ominaisuudet

Sallitun operaattorit

* -eq


* -ne


| Ominaisuudet:     | Sallittu arvo  | Käyttö                          |
|----------------|-----------------|--------------------------------|
| accountEnabled | TOSI, EPÄTOSI      | user.accountEnabled - eq tosi)  |
| dirSyncEnabled | TOSI, EPÄTOSI null | (user.dirSyncEnabled - eq tosi) |

### <a name="properties-of-type-string"></a>Merkkijonomuotoisen ominaisuudet

Sallitun operaattorit

* -eq


* -ne


* -notStartsWith


* -StartsWith


* -on


* -notContains


* -vastaa


* -notMatch

| Ominaisuudet:                 | Sallittu arvo                                                                                        | Käyttö                                                     |
|----------------------------|-------------------------------------------------------------------------------------------------------|-----------------------------------------------------------|
| Kaupunki                       | Mikä tahansa merkkijonoarvo tai $null                                                                           | (user.city - eq "value")                                   |
| maa                    | Mikä tahansa merkkijonoarvo tai $null                                                                            | (user.country - eq "value")                                |
| osasto                 | Mikä tahansa merkkijonoarvo tai $null                                                                          | (user.department - eq "value")                             |
| Näyttönimi                | Mikä tahansa merkkijonoarvo                                                                                 | (user.displayName - eq "value")                            |
| facsimileTelephoneNumber   | Mikä tahansa merkkijonoarvo tai $null                                                                           | (user.facsimileTelephoneNumber - eq "value")               |
| givenName                  | Mikä tahansa merkkijonoarvo tai $null                                                                           | (user.givenName - eq "value")                              |
| Asema                   | Mikä tahansa merkkijonoarvo tai $null                                                                           | (user.jobTitle - eq "value")                               |
| sähköposti                       | Mikä tahansa merkkijonoarvo tai $null (käyttäjän SMTP-osoite)                                                  | (user.mail - eq "value")                                   |
| mailNickName               | Mikä tahansa merkkijonoarvo (sähköpostin alias käyttäjän)                                                            | (user.mailNickName - eq "value")                           |
| Nuolet                     | Mikä tahansa merkkijonoarvo tai $null                                                                           | (user.mobile - eq "value")                                 |
| Objektitunnus                   | GUID-tunnus käyttäjäobjekti                                                                            | (user.objectId - eq "1111111-1111-1111-1111-111111111111") |
| passwordPolicies           | Ei mitään DisableStrongPassword DisablePasswordExpiration DisablePasswordExpiration, DisableStrongPassword |   (user.passwordPolicies - eq "DisableStrongPassword")                                                      |
| physicalDeliveryOfficeName | Mikä tahansa merkkijonoarvo tai $null                                                                            | (user.physicalDeliveryOfficeName - eq "value")             |
| Postinumero                 | Mikä tahansa merkkijonoarvo tai $null                                                                            | (user.postalCode - eq "value")                             |
| preferredLanguage          | ISO 639-1-koodi                                                                                        | (user.preferredLanguage - eq "en-US")                      |
| sipProxyAddress            | Mikä tahansa merkkijonoarvo tai $null                                                                            | (user.sipProxyAddress - eq "value")                        |
| tila                      | Mikä tahansa merkkijonoarvo tai $null                                                                            | (user.state - eq "value")                                  |
| streetAddress              | Mikä tahansa merkkijonoarvo tai $null                                                                            | (user.streetAddress - eq "value")                          |
| Sukunimi                    | Mikä tahansa merkkijonoarvo tai $null                                                                            | (user.surname - eq "value")                                |
| telephoneNumber            | Mikä tahansa merkkijonoarvo tai $null                                                                            | (user.telephoneNumber - eq "value")                        |
| usageLocation              | Kaksi kirjaimella maakoodi                                                                           | (user.usageLocation - eq "USA")                             |
| userPrincipalName          | Mikä tahansa merkkijonoarvo                                                                                     | (user.userPrincipalName - eq"alias@domain")               |
| userType                   | jäsenen Vieras $null                                                                                    | (user.userType - eq "Jäsenen")                              |

### <a name="properties-of-type-string-collection"></a>Kirjoita merkkijono sivustokokoelman ominaisuudet

Sallitun operaattorit

* -on


* -notContains

| Poperties      | Sallittu arvo                        | Käyttö                                                |
|----------------|---------------------------------------|------------------------------------------------------|
| otherMails     | Mikä tahansa merkkijonoarvo                      | (user.otherMails-on"alias@domain")           |
| proxyAddresses | SMTP: alias@domain smtp:alias@domain | (user.proxyAddresses-sisältää "SMTP:alias@domain") |

## <a name="extension-attributes-and-custom-attributes"></a>Tunnisteen määritteet ja mukautetut määritteet
Tunnisteen määritteet ja mukautetut määritteet tuetaan dynaaminen jäsenyyden säännöt.

Tunnisteen määritteet on synkronoitu paikalliseen ikkunan palvelimen AD ja ota muodossa "ExtensionAttributeX", jossa X on 1-15.
Esimerkki säännön, joka käyttää tunniste-määrite

(user.extensionAttribute15 - eq "Markkinointi")

Mukautetut määritteet synkronoidaan paikalliseen Windows Server AD tai yhdistetyn SaaS-sovelluksesta ja muodossa "user.extension_[GUID]\__ [määrite]", jossa [GUID] on yksilöllinen tunnus-AAD AAD määrite luoneen sovelluksen ja linkitettyä [määrite] on määritteen nimi.
Esimerkki säännön, joka käyttää mukautetun määritteen on

User.extension_c272a57b722d4eb29bfe327874ae79cb__OfficeNumber  

Mukautetun määritteen nimi löytyy hakemiston tekemällä kyselyn käyttäjä on määrite Graph Resurssienhallinnassa ja etsimällä määritteen nimi.

## <a name="direct-reports-rule"></a>Alaisten sääntö
Voit nyt lisätä perustuvat käyttäjän esimies-määritteen ryhmän jäsenet.

**Voit määrittää ryhmän "Manager" ryhmänä**

1. Azure perinteinen-portaalissa **Active Directory**ja valitse sitten ja organisaation hakemiston nimi.

2. Valitse **ryhmät** -välilehti ja avaa sitten ryhmän, jota haluat muokata.

3. Valitse **määritys** -välilehti ja valitse sitten **Lisäasetukset-sääntö**.

4. Kirjoita säännön seuraavaa syntaksia:

    Suora raportteja varten *{obectID_of_manager} alaiset*. Esimerkki alaiset kelvollinen säännön on

                    Direct Reports for "62e19b97-8b3d-4d4a-a106-4ce66896a863”

    missä "62e19b97-8b3d-4d4a-a106-4ce66896a863" esimies objektitunnus. Objektitunnus löytyvät Azure AD käyttäjälle, joka on esimies käyttäjän sivun **profiili-välilehti** .

3. Kun tallennat tämän säännön, kaikki käyttäjät, jotka täyttävät säännön liitetään ryhmän jäsenet. Täytä alun perin ryhmän muutama minuutti voi kestää.


## <a name="using-attributes-to-create-rules-for-device-objects"></a>Laitteen objektien sääntöjen luominen käyttämällä määritteet

Voit myös luoda säännön, joka valitsee ryhmän jäsenyyden laitteen objektit. Laitteen määritteet voidaan käyttää:

| Ominaisuudet:              | Sallittu arvo                  | Käyttö                                                       |
|-------------------------|---------------------------------|-------------------------------------------------------------|
| Näyttönimi             | mikä tahansa merkkijonoarvo                | (device.displayName - eq "Juhani Iphone")                       |
| deviceOSType            | mikä tahansa merkkijonoarvo                | (device.deviceOSType - eq "IOS")                             |
| deviceOSVersion         | mikä tahansa merkkijonoarvo                | (laite. OSVersion - eq "9.1")                                |
| isDirSynced             | TOSI, EPÄTOSI null                 | (device.isDirSynced - eq "true")                             |
| isManaged               | TOSI, EPÄTOSI null                 | (device.isManaged - eq "false")                              |
| isCompliant             | TOSI, EPÄTOSI null                 | (device.isCompliant - eq "true")                             |
| deviceCategory          | mikä tahansa merkkijonoarvo                | (device.deviceCategory - eq "")                              |
| deviceManufacturer      | mikä tahansa merkkijonoarvo                | (device.deviceManufacturer - eq "Microsoft")                 |
| deviceModel             | mikä tahansa merkkijonoarvo                | (device.deviceModel - eq "IPhone 7 +")                        |
| deviceOwnership         | mikä tahansa merkkijonoarvo                | (device.deviceOwnership - eq "")                             |
| Toimialuenimi              | mikä tahansa merkkijonoarvo                | (device.domainName - eq "contoso.com")                       |
| enrollmentProfileName   | mikä tahansa merkkijonoarvo                | (device.enrollmentProfileName - eq "")                       |
| isRooted                | TOSI, EPÄTOSI null                 | (device.deviceOSType - eq "true")                            |
| managementType          | mikä tahansa merkkijonoarvo                | (device.managementType - eq "")                              |
| organizationalUnit      | mikä tahansa merkkijonoarvo                | (device.organizationalUnit - eq "")                          |
| deviceId                | kelvollinen deviceId                | (device.deviceId - eq "d4fe7726-5966-431c-b3b8-cddc8fdb717d" |

> [AZURE.NOTE]
> Sääntöjen laite ei voi luoda käyttämällä "yksinkertaisen säännön" avattava Azure perinteinen-portaalissa.


## <a name="additional-information"></a>Lisätietoja
Seuraavissa artikkeleissa on lisätietoja Azure Active Directory.

* [Dynaaminen ryhmien jäsenyyksien vianmääritys](active-directory-accessmanagement-troubleshooting.md)

* [Resurssien käytön hallinta ja Azure Active Directory-ryhmä](active-directory-manage-groups.md)

* [Ryhmän asetusten määrittämiseen Azure Active Directory cmdlet-komennot](active-directory-accessmanagement-groups-settings-cmdlets.md)

* [Artikkelissa indeksin Azure Active Directory-sovellusten hallinta](active-directory-apps-index.md)

* [Azure Active Directory-integraation paikallisen käyttäjätietoja](active-directory-aadconnect.md)

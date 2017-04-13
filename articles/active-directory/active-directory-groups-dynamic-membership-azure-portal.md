
<properties
    pageTitle="Ryhmäjäsenyyden lisäsääntöjä luominen Azure Active Directory-esikatselu määritteet avulla | Microsoft Azure"
    description="Voit luoda Monitasoisempia sääntöjä dynaaminen ryhmän jäsenyyden mukaan lukien tuetut lausekkeiden säännön operaattorit ja parametrit."
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
    ms.date="09/12/2016"
    ms.author="curtand"/>


# <a name="using-attributes-to-create-advanced-rules-for-group-membership-in-azure-active-directory-preview"></a>Ryhmäjäsenyyden lisäsääntöjä luominen Azure Active Directory-esikatselu määritteet avulla

Azure portaalin avulla voit luoda Monitasoisempia sääntöjä, jotta monimutkaisia määrite perustuvan dynaamisen jäsenyydet Azure Active Directory (Azure AD) esikatselu-ryhmä. [Mikä on esikatselussa?](active-directory-preview-explainer.md) Tämän artikkelin tiedot säännön määritteet ja syntaksi näiden sääntöjen luomiseen.

## <a name="to-create-the-advanced-rule"></a>Jos haluat luoda lisäsäännön

1.  Kirjautuminen [Azure portaaliin](https://portal.azure.com) tilille, jolla on yleisen järjestelmänvalvojan kansion.

2.  Valitse **Lisää palveluja**, kirjoita tekstiruutuun **käyttäjät ja ryhmät** ja paina sitten **ENTER-näppäintä**.

  ![Avaava käyttäjien hallinta](./media/active-directory-groups-dynamic-membership-azure-portal/search-user-management.png)

3.  Valitse **käyttäjät ja ryhmät** -sivu **Kaikki ryhmät**.

  ![Avaaminen ryhmät-sivu](./media/active-directory-groups-dynamic-membership-azure-portal/view-groups-blade.png)

4. Valitse **käyttäjät ja ryhmät - kaikki ryhmät** -sivu valitsemalla **Lisää** -komento.

  ![Lisää uusi ryhmä](./media/active-directory-groups-dynamic-membership-azure-portal/add-group-type.png)

5. Kirjoita nimi ja kuvaus uudelle ryhmälle **ryhmä** -sivu. Valitse sen mukaan, haluatko luoda säännön käyttäjille tai laitteille **jäsenyyden Kirjoita** **Dynaaminen-käyttäjän** tai **Dynaaminen laitteen**, ja valitse sitten **Lisää dynaamisen kyselyn**. Katso laitteen sääntöjä käytetään määritteitä [käyttäminen määritteet laitteen objektien sääntöjen luomiseen](#using-attributes-to-create-rules-for-device-objects).

  ![Dynaaminen jäsenyyden säännön lisääminen](./media/active-directory-groups-dynamic-membership-azure-portal/add-dynamic-group-rule.png)

6. Kirjoita **Lisää dynamic jäsenyyden lisäsäännön** -ruutuun säännön **Dynamic jäsenyyden säännöt** -sivu, painamalla Enter-näppäintä ja valitse **Luo** sivu alareunassa.

7. Valitse **Luo** **ryhmä** -sivu, voit luoda ryhmän.

## <a name="constructing-the-body-of-an-advanced-rule"></a>Kehittynyt sääntö leipätekstiin luominen

Lisäsäännön, voit luoda dynaamisia ryhmien jäsenyyksien on käytettävä binaarinen lauseke, joka koostuu kolmesta osasta ja tuloksena on TOSI tai EPÄTOSI-Tulosta. Kolme osaa on:

- Vasen parametri
- Binaarinen operaattori
- Oikea vakio

Valmis lisäsäännön näyttää seuraavankaltaiselta: (leftParameter binaryOperator "RightConstant"), missä avaavat ja sulkeva Sulje tarvittavat koko binaarinen lauseketta, lainausmerkkejä tarvittavat oikean vakion ja vasen parametrin syntaksi on user.property. Kehittynyt sääntö voi olla useampi kuin yksi binaarinen lausekkeita, jotka on erotettu toisistaan- ja- ja ja - ei loogiset operaattorit.

Seuraavassa on esimerkkejä oikein rakennettava lisäsäännön:

- (user.department - eq "Myynti")- tai (user.department - eq "Markkinointi")
- (user.department - eq "Myynti")- ja - ei (user.jobTitle-sisältää "SDE")

Katso luettelo kaikista tuettujen parametrien ja lausekkeen säännön operaattoreita, seuraavissa osissa. Katso laitteen sääntöjä käytetään määritteitä [käyttäminen määritteet laitteen objektien sääntöjen luomiseen](#using-attributes-to-create-rules-for-device-objects).

Oman lisäsäännön leipätekstiin koko pituus voi olla enintään 2 048 merkkiä.

> [AZURE.NOTE]
>Merkkijono ja regex toiminnot ovat kirjainkokoa. Voit suorittaa Null tarkistukset, käyttämällä $null vakiona, kuten, user.department - eq $null.
Merkkijonojen tarjoukset "pitäisi olla ohitettuja käyttämällä" merkki, esimerkiksi user.department - eq \`"Myynti".

## <a name="supported-expression-rule-operators"></a>Tuetut lausekkeiden säännön operaattorit
Seuraavassa taulukossa on lueteltu tuetut lausekkeen säännön operaattorit kaikki ja niiden syntaksi, jota käytetään lisäsäännön tekstiosassa:

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
| Virhe: Tuntematon virhe dynamic jäsenyydet määrittäminen. | (user.accountEnabled - eq "True" AND user.userPrincipalName-on"alias@domain")                               | (user.accountEnabled - eq tosi)- ja (user.userPrincipalName-on"alias@domain")<br/>Kyselyssä on useita virheitä. Sulje kuin oikeille.                                                                                                                            |

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

1. Vaiheiden 1 – 5: [Voit luoda lisäsäännön](#to-create-the-advanced-rule)ja valitse **Dynaaminen käyttäjän** **jäsenyyden tyyppi** .

2. Anna **dynaaminen jäsenyyden säännöt** -sivu sääntöä, jonka seuraavaa syntaksia:

    Suora raportteja varten *{obectID_of_manager} alaiset*. Esimerkki alaiset kelvollinen säännön on

                    Direct Reports for "62e19b97-8b3d-4d4a-a106-4ce66896a863”

    missä "62e19b97-8b3d-4d4a-a106-4ce66896a863" esimies objektitunnus. Objektitunnus löytyvät Azure AD käyttäjälle, joka on esimies käyttäjän sivun **profiili-välilehti** .

3. Kun tallennat tämän säännön, kaikki käyttäjät, jotka täyttävät säännön liitetään ryhmän jäsenet. Täytä alun perin ryhmän muutama minuutti voi kestää.


## <a name="using-attributes-to-create-rules-for-device-objects"></a>Laitteen objektien sääntöjen luominen käyttämällä määritteet

Voit myös luoda säännön, joka valitsee ryhmän jäsenyyden laitteen objektit. Laitteen määritteet voidaan käyttää:

| Ominaisuudet:           | Sallittu arvo                  | Käyttö                                                |
|----------------------|---------------------------------|------------------------------------------------------|
| Näyttönimi          | mikä tahansa merkkijonoarvo                | (device.displayName - eq "Juhani Iphone")                 |
| deviceOSType         | mikä tahansa merkkijonoarvo                | (device.deviceOSType - eq "IOS")                      |
| deviceOSVersion      | mikä tahansa merkkijonoarvo                | (laite. OSVersion - eq "9.1")                         |
| isDirSynced          | TOSI, EPÄTOSI null                 | (device.isDirSynced - eq "true")                      |
| isManaged            | TOSI, EPÄTOSI null                 | (device.isManaged - eq "false")                       |
| isCompliant          | TOSI, EPÄTOSI null                 | (device.isCompliant - eq "true")                      |


## <a name="additional-information"></a>Lisätietoja
Seuraavissa artikkeleissa on lisätietoja Azure Active Directory-ryhmiä.

* [On olemassa olevia ryhmiä](active-directory-groups-view-azure-portal.md)
* [Uuden ryhmän luominen ja jäsenten lisääminen](active-directory-groups-create-azure-portal.md)
* [Ryhmän asetusten hallinta](active-directory-groups-settings-azure-portal.md)
* [Ryhmän jäsenyyksien hallinta](active-directory-groups-membership-azure-portal.md)
* [Dynaaminen sääntöjen ryhmän käyttäjien hallinta](active-directory-groups-dynamic-membership-azure-portal.md)

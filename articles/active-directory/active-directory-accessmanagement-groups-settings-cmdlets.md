<properties
    pageTitle="Azure Active Directory cmdlet-komennot ryhmän asetusten määrittämiseen | Microsoft Azure"
    description="Miten hallita ryhmiä avulla Azure Active Directory-cmdlet-komennot asetukset."
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
    ms.date="09/22/2016"
    ms.author="curtand"/>


# <a name="azure-active-directory-cmdlets-for-configuring-group-settings"></a>Ryhmän asetusten määrittämiseen Azure Active Directory cmdlet-komennot

Hakemistossa voi määrittää yhdistetyn ryhmien seuraavat asetukset:

1.  Luokitusten: CSV-luettelo, jotka käyttäjät voivat määrittää ryhmälle luokitukset. Esimerkkejä olisi "Luokiteltu", "Salaisuus" ja "Yläreunan salainen."

2.  Käyttö ohjeiden URL-osoite: URL-osoite, joka osoittaa käyttäjät käyttöehdot varten yhdistetyn ryhmien käyttäminen oman organisaation määritysten mukaisesti. Tätä URL-osoite näkyy käyttöliittymä, jossa käyttäjät saavat käyttää ryhmiä.

3.  Ryhmän luomisen käytössä: onko ei mitään, jotkin tai kaikki käyttäjät voivat luoda yhdistetyn ryhmiä. Kun arvoksi, kaikki käyttäjät voivat luoda ryhmiä. Kun asetus on poissa käytöstä, ei ole käyttäjät voivat luoda ryhmiä. Kun käytössä, voit myös määrittää käyttöoikeusryhmän, jonka käyttäjät, jotka ovat edelleen oikeus luoda ryhmät.

Nämä asetukset on määritetty asetuksia ja SettingsTemplate objekteja. Alun perin et näe asetukset objekteja hakemistossa. Tämä tarkoittaa hakemistossa on määritetty oletusasetukset. Muuta oletusasetuksia, luot uuden asetukset objektin asetukset mallin avulla. Mallien asetukset on määritetty Microsoft.

Voit ladata [Microsoft Connect-sivustossa](http://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185)näitä toimintoja käytetään cmdlet-komennot sisältävä moduuli.

## <a name="create-settings-at-the-directory-level"></a>Luo asetukset sivustotasolla

Seuraavasti Luo asetukset directory tasolla, jotka koskevat kaikkia Office-ryhmien hakemistossa.

1. Jos et tiedä, mitä SettingTemplate käyttämään, tämä cmdlet-komento palauttaa asetukset mallien luettelo:

    `Get-MsolAllSettingTemplate`

    ![Luettelon asetukset-malleista](./media/active-directory-accessmanagement-groups-settings-cmdlets/list-of-templates.png)

2. Jos haluat lisätä käyttö yleisohjeet URL-osoite, sinun on ensin Hae SettingsTemplate-objekti, joka määrittää käyttö yleisohjeet URL-osoite-arvo. Toisin sanoen Group.Unified mallin:

    `$template = Get-MsolSettingTemplate –TemplateId 62375ab9-6b52-47ed-826b-58e47e0e304b`

3. Seuraavaksi luominen mallin pohjalta uuden asetukset-objekti:

    `$setting = $template.CreateSettingsObject()`

4. Päivitä käyttö yleisohjeet arvo:

    `$setting["UsageGuidelinesUrl"] = "<https://guideline.com>"`

5. Lopuksi käyttää asetuksia:

    `New-MsolSettings –SettingsObject $setting`

    ![Lisää käyttö yleisohjeet URL-osoite](./media/active-directory-accessmanagement-groups-settings-cmdlets/add-usage-guideline-url.png)

Seuraavassa on määritetty Group.Unified SettingsTemplate asetukset.

 **Asetus**                          | **Kuvaus**                                                                                             
--------------------------------------|-----------------------------------------------
 <ul><li>ClassificationList<li>Tyyppi: merkkijono<li>Oletusarvo: ""                  | Pilkuilla erotettu luettelo kelvollinen luokitus-arvoja, jotka voidaan lisätä yhdistetyn ryhmät.                
 <ul><li>EnableGroupCreation<li>Tyyppi: totuusarvo<li>Oletus: TOSI              | Merkinnän, joka ilmaisee, voiko yhdistetyssä ryhmän luomisen hakemistossa.                               
 <ul><li>GroupCreationAllowedGroupId<li>Tyyppi: merkkijono<li>Oletusarvo: ""         | GUID-tunnus, joka on oikeus luoda yhdistetyn ryhmiä käyttöoikeusryhmän myös silloin, kun EnableGroupCreation == false.
 <ul><li>UsageGuidelinesUrl<li>Tyyppi: merkkijono<li>Oletusarvo: ""                  | Ryhmän käyttö ohjeiden linkki.                                                                       

## <a name="read-settings-at-the-directory-level"></a>Lukuasetukset sivustotasolla

Nämä vaiheet lukea directory tasolla asetuksia, jotka koskevat kaikkia Office-ryhmiin hakemistossa.

1. Lue kaikki hakemistoasetusten:

    `Get-MsolAllSettings`

2. Lue tietty ryhmä, kaikkia asetuksia:

    `Get-MsolAllSettings -TargetType Groups -TargetObjectId <groupObjectId>`

3. Lue tietyn kansion asetukset-SettingId-ominaisuus GUID-tunnus:

    `Get-MsolSettings –SettingId dbbcb0ea-a6ff-4b44-a1f3-9d7cef74984c`

    ![Asetusten tunnus GUID-tunnus](./media/active-directory-accessmanagement-groups-settings-cmdlets/settings-id-guid.png)

## <a name="update-settings-at-the-directory-level"></a>Sivustotasolla-asetusten päivittäminen

Nämä vaiheet Päivitä hakemiston tasolla asetuksia, jotka koskevat kaikkia Office-ryhmiin hakemistossa.

1. Saat asetukset objektiin:

    `$setting = Get-MsolSettings –SettingId dbbcb0ea-a6ff-4b44-a1f3-9d7cef74984c`

2. Hae päivitettävän arvo:

    `$value = $Setting.GetSettingsValue()`

3. Päivitä arvo:

    `$value["AllowToAddGuests"] = "false"`

4. Päivittää asetus:

    `Set-MsolSettings –SettingId dbbcb0ea-a6ff-4b44-a1f3-9d7cef74984c –SettingsValue $value`

## <a name="remove-settings-at-the-directory-level"></a>Poista asetukset sivustotasolla

Tässä vaiheessa poistaa kansion tasolla asetuksia, jotka koskevat kaikkia Office-ryhmiin hakemistossa.

    `Remove-MsolSettings –SettingId dbbcb0ea-a6ff-4b44-a1f3-9d7cef74984c`

## <a name="cmdlet-syntax-reference"></a>Cmdlet-kyselykielen syntaksi

Löydät lisää PowerShellin Azure Active Directory-ohjeista [Azure Active Directory cmdlet-komennot](http://go.microsoft.com/fwlink/p/?LinkId=808260).

## <a name="settingstemplate-object-reference-groupunified-settingstemplate-object"></a>SettingsTemplate objektiviittaus (Group.Unified SettingsTemplate objekti)

- "nimi": "EnableGroupCreation", "tyyppi": "System.Boolean", "oletusarvo": "true", "kuvaus": "Totuusarvo merkinnän, joka ilmaisee yhdistetyssä ryhmän luominen-toiminto ei."

- "nimi": "GroupCreationAllowedGroupId", "tyyppi": "System.Guid", "oletusarvo": "", "kuvaus": "GUID-tunnus, joka on yhdistetty ryhmitellä whitelisted käyttöoikeusryhmän."

- "nimi": "ClassificationList", "tyyppi": "System.String", "oletusarvo": "", "kuvaus": "Pilkuilla erotettu luettelo kelvollinen luokitus-arvoja, jotka voi suojata yhdistetyssä ryhmiin."

- "nimi": "UsageGuidelinesUrl", "tyyppi": "System.String", "oletusarvo": "", "kuvaus": "Ryhmän käyttö ohjeiden linkki."

Nimi | tyyppi | Oletusarvo | kuvaus
----------  | ----------  | ---------  | ----------
"EnableGroupCreation"  | "System.Boolean"  | "true"  | "Totuusarvo merkinnän, joka ilmaisee yhdistetyssä ryhmän luominen-toiminto ei."
"GroupCreationAllowedGroupId"  | "System.Guid"  | ""  | "GUID-tunnus, joka on yhdistetty ryhmitellä whitelisted käyttöoikeusryhmän."
"ClassificationList"  | "System.String"  | ""  | "Pilkuilla erotettu luettelo kelvollinen luokitus-arvoja, jotka voidaan lisätä yhdistetyn ryhmien."
"UsageGuidelinesUrl"  | "System.String"  | ""  | "Ryhmän käyttö ohjeiden linkki."

## <a name="next-steps"></a>Seuraavat vaiheet

Löydät lisää PowerShellin Azure Active Directory-ohjeista [Azure Active Directory cmdlet-komennot](http://go.microsoft.com/fwlink/p/?LinkId=808260).

Muita Ohje Microsoft ohjelma vastuuhenkilöltä Juhani de Jong on saatavilla osoitteessa [Juhani 's ryhmät blogiin](http://robsgroupsblog.com/blog/configuring-settings-for-office-365-groups-in-azure-ad).

* [Resurssien käytön hallinta ja Azure Active Directory-ryhmä](active-directory-manage-groups.md)

* [Azure Active Directory-integraation paikallisen käyttäjätietoja](active-directory-aadconnect.md)

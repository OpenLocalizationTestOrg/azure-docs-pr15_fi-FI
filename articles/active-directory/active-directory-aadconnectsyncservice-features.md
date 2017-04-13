<properties
    pageTitle="Azure AD Connect synkronointiominaisuudet ja määritykset | Microsoft Azure"
    description="Tässä artikkelissa kuvataan puoli ominaisuudet Azure AD Connect synkronointi-palveluun."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="andkjell;markvi"/>

# <a name="azure-ad-connect-sync-service-features"></a>Azure AD Connect synkronoinnin ominaisuudet

Azure AD Connect synkronointi-toiminto on kaksi osaa:

- Paikallisen osa nimeltä **Azure AD Connect synkronoinnin**, kutsutaan myös **sync engine**.
- Azure AD tunnetaan myös nimellä **Azure AD Connect synkronointipalvelun** asuvat palvelu

Tässä ohjeaiheessa kerrotaan, miten **Azure AD Connect synkronointipalvelun** seuraavat ominaisuudet toimivat ja miten voit määrittää ne Windows PowerShellin avulla.

Nämä asetukset on määritetty [Active Directory moduulin Windows PowerShellin Azure](http://aka.ms/aadposh)mukaan. Lataa ja asenna se erikseen Azure AD Connect. Tässä ohjeaiheessa kuvattu cmdlet-komennot Macin [2016 maaliskuussa Vapauta (koontiversio 9031.1)](http://social.technet.microsoft.com/wiki/contents/articles/28552.microsoft-azure-active-directory-powershell-module-version-release-history.aspx#Version_9031_1). Jos sinulla ei ole kuvattu tämän artikkelin cmdlet-komennot tai ne eivät tuota saman tuloksen, varmista, että suoritat uusimpaan versioon.

Voit tarkastella Azure Active Directoryn määritykset suorittamalla `Get-MsolDirSyncFeatures`.  
![Hae MsolDirSyncFeatures tulos](./media/active-directory-aadconnectsyncservice-features/getmsoldirsyncfeatures.png)

Monet näistä asetuksista saavat muuttaa vain Azure AD Connect.

Seuraavia asetuksia voidaan määrittää `Set-MsolDirSyncFeature`:

DirSyncFeature | Kommentti
--- | ---
[DuplicateProxyAddressResiliency<br/>DuplicateUPNResiliency](#duplicate-attribute-resiliency) | Sallii määritteen karanteeniin, kun se on kaksoiskappale toisen objektin sijaan kaatuvat koko objektin viennin aikana.
[EnableSoftMatchOnUpn](#userprincipalname-soft-match) | Sallii objektien liittämisessä userPrincipalName lisäksi ensisijainen SMTP-osoite.
[SynchronizeUpnForManagedUsers](#synchronize-userprincipalname-updates) | Synkronointi-ohjelma päivittää hallitun/käyttöoikeus (ei ole-liitetty) käyttäjille userPrincipalName-määritteen avulla.

Kun olet ottanut ominaisuuden, sitä ei voi poistaa käytöstä uudelleen.

>[AZURE.NOTE] 24 elokuussa, 2016 ominaisuus *kaksoiskappaleiden määrite vikasietoisuudelle* on käytössä oletusarvoisesti, uusi Azure AD-kansioita. Tätä ominaisuutta myös esiin ja kansiot, jotka on luotu ennen tätä päivää käytössä. Saat siitä sähköposti-ilmoituksen, kun hakemistossa Hae tämä toiminto on käytössä.

Seuraavat asetukset on määritetty Azure AD Connect ja eivät voi muokata `Set-MsolDirSyncFeature`:

DirSyncFeature | Kommentti
--- | ---
DeviceWriteback | [Azure AD Connect: Ottaminen käyttöön laitteen takaisinkirjoituksen](active-directory-aadconnect-feature-device-writeback.md)
DirectoryExtensions | [Azure AD Connect synkronointi: kansion tunnisteet](active-directory-aadconnectsync-feature-directory-extensions.md)
PasswordSync | [Salasanojen synkronoinnin Azure AD Connect synkronoinnin toteuttaminen](active-directory-aadconnectsync-implement-password-synchronization.md)
UnifiedGroupWriteback | [Esikatselu: Ryhmän takaisinkirjoituksen](active-directory-aadconnect-feature-preview.md#group-writeback)
UserWriteback | Ei tueta.

## <a name="duplicate-attribute-resiliency"></a>Kaksoiskappaleiden määrite vikasietoisuudelle
Sen sijaan, että epäonnistumista valmistelu objektien kaksoiskappaleiden UPNs / proxyAddresses-kaksoiskappaleet määrite on "karanteeniin" ja väliaikaisena arvona on määritetty. Kun ristiriita ratkennut, väliaikaiset UPN muutetaan oikean arvon automaattisesti. Tämä ongelma voi ottaa käyttöön täydellisen Käyttäjätunnuksen ja proxyAddress erikseen. Lisätietoja on artikkelissa [tunnistetietojen synkronointi ja kaksoiskappaleiden määrite vikasietoisuudelle](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md).

## <a name="userprincipalname-soft-match"></a>UserPrincipalName Pehmeä vastaavuus
Jos tämä ominaisuus on käytössä, Pehmeä vastaavuus on käytössä UPN lisäksi [ensisijainen SMTP-osoite](https://support.microsoft.com/kb/2641663), joka on aina käytössä. Pehmeä vastine käytetään vastaamaan cloud aiemmin luodut käyttäjät Azure AD-ympäristöön.

Jos haluat vastine paikallisen AD-tili ja pilveen luotu aiemmin luotujen tilien ja et käytä Exchange Onlinen ja valitse tämä ominaisuus on hyödyllinen. Tässä skenaariossa yleensä ei ole syytä määrittää SMTP-määritteen pilveen.

Tämä ominaisuus on oletusarvoisesti juuri luotu Azure AD-kansioiden. Jos tämä ominaisuus on käytössä, suorittamalla näkyviin:  
```
Get-MsolDirSyncFeatures -Feature EnableSoftMatchOnUpn
```

Jos tämä ominaisuus ei ole otettu käyttöön Azure AD-kansion, valitse voit ottaa sen käyttöön suorittamalla:  
```
Set-MsolDirSyncFeature -Feature EnableSoftMatchOnUpn -Enable $true
```

## <a name="synchronize-userprincipalname-updates"></a>Synkronoi userPrincipalName-päivitykset
Perinteisesti päivitykset UserPrincipalName-määritteen paikallisen synkronointipalvelun käyttö on estetty, jos molemmat ehdot täyttyvät:

- Käyttäjän hallinnasta (ei ole-liitetty).
- Käyttäjä ei ole määritetty käyttöoikeutta.

Lisätietoja on artikkelissa [Office 365: ssä, Azure-tai Intune käyttäjänimet eivät vastaa paikallisen täydellisen Käyttäjätunnuksen tai vaihtoehtoinen Kirjautumistunnus](https://support.microsoft.com/kb/2523192).

Tämän ominaisuuden avulla synkronointi-ohjelma userPrincipalName päivitetään, kun se on muutettu paikallisen ja salasanan synkronointi. Jos käytät federaatio, tätä ominaisuutta ei tueta.

Tämä ominaisuus on oletusarvoisesti juuri luotu Azure AD-kansioiden. Jos tämä ominaisuus on käytössä, suorittamalla näkyviin:  
```
Get-MsolDirSyncFeatures -Feature SynchronizeUpnForManagedUsers
```

Jos tämä ominaisuus ei ole otettu käyttöön Azure AD-kansion, valitse voit ottaa sen käyttöön suorittamalla:  
```
Set-MsolDirSyncFeature -Feature SynchronizeUpnForManagedUsers -Enable $true
```

Kun tämä ominaisuus on otettu käyttöön, aiemmin userPrincipalName-arvot pysyvät-on. Seuraava muutos userPrincipalName määrite paikallisia Normaali delta Synkronoi käyttäjät päivittyy UPN.  

## <a name="see-also"></a>Katso myös

- [Azure AD Connect synkronointi](active-directory-aadconnectsync-whatis.md)
- Paikallisen ympäristön [integroinnissa paikallisen-käyttäjätietojen Azure Active Directory-hakemistosta](active-directory-aadconnect.md).

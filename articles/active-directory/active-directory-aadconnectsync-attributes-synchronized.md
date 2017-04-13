<properties
    pageTitle="Azure AD Connect synkronointi: attribuutit synkronoidaan Azure Active Directory | Microsoft Azure"
    description="Luettelo attribuuteista, jotka synkronoidaan Azure Active Directory."
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
    ms.date="09/13/2016"
    ms.author="markvi;andkjell"/>


# <a name="azure-ad-connect-sync-attributes-synchronized-to-azure-active-directory"></a>Azure AD Connect synkronointi: attribuutit synkronoidaan Azure Active Directory
Tässä artikkelissa on lueteltu attribuuteista, jotka synkronoidaan Azure AD Connect synkronointi.  
Määritteet on ryhmitelty liittyvät Azure AD-sovellus.

## <a name="attributes-to-synchronize"></a>Jos haluat synkronoida määritteet
Yleisiä kysymyksiä on *vähimmäisvaatimukset attribuuteista synkronoimaan luettelon ominaisuudet*. Oletus- tai suositellaan on pitää oletusarvoiset määritteet, jotta koko GAL (Yleinen osoiteluettelo) on rakennettava pilveen ja Hae kaikkia ominaisuuksia Office 365-toiminnoista. Joissakin tapauksissa on joitakin määritteitä, organisaatiosi ei ole synkronoitu pilveen koska seuraavanlaisia määritteitä sisältävät arkaluonteisia tai PII (henkilökohtaisia tietoja) tietoja, kuten tässä esimerkissä:  
![virheelliset attribuutit](./media/active-directory-aadconnectsync-attributes-synchronized/badextensionattribute.png)

Tässä tapauksessa luettelo määritteistä tämän ohjeaiheen alussa ja määrittää nämä määritteet, joka sisältää luottamukselliset tai henkilökohtaisia tietoja tietojen ja ei voi synkronoida. Poista nämä määritteet [Azure AD-sovellus](active-directory-aadconnect-get-started-custom.md#azure-ad-app-and-attribute-filtering)ja määritteen suodattaminen asennuksen aikana.

>[AZURE.WARNING] Kun niiden valinnan poistaminen määritteitä, pitäisi olla varovainen ja Poista vain nämä määritteet ei voi synkronoida. Poistamalla valinnan määritteet voi olla negatiivinen vaikutus ominaisuuksia.

## <a name="office-365-proplus"></a>Office 365 ProPlus

| Määritteen nimi| Käyttäjän| Kommentti |
| --- | :-: | --- |
| accountEnabled| X| Määrittää, jos tili on käytössä.|
| Kirjautuminen| X|  |
| Näyttönimi| X|  |
| objectSID| X| mekaanisen ominaisuus. AD käyttäjätunnus käyttää ylläpitämään synkronointiin välillä Azure AD- ja AD.|
| pwdLastSet| X| mekaanisen ominaisuus. Milloin kannattaa poistaa jo myönnetty tunnusten käytetään. Salasanan synkronointi ja federation käyttämä.|
| sourceAnchor| X| mekaanisen ominaisuus. Pysyvä tunnus, jos haluat säilyttää Lisää ja Azure AD välinen suhde.|
| usageLocation| X| mekaanisen ominaisuus. Käyttäjän maa. Käyttää käyttöoikeusmääritykset.|
| userPrincipalName| X| Täydellisen Käyttäjätunnuksen on käyttäjän Kirjautumistunnus. Yleensä sama kuin [sähköpostin] arvo.|

## <a name="exchange-online"></a>Exchange Online

| Määritteen nimi| Käyttäjän| Yhteyshenkilö| Ryhmän| Kommentti |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Määrittää, jos tili on käytössä.|
| avustaja| X| X|  |  |
| authOrig| X| X| X|  |
| c| X| X|  |  |
| Kirjautuminen| X|  | X|  |
| Mää| X| X|  |  |
| yrityksen| X| X|  |  |
| countryCode| X| X|  |  |
| osasto| X| X|  |  |
| kuvaus| X| X| X|  |
| Näyttönimi| X| X| X|  |
| dLMemRejectPerms| X| X| X|  |
| dLMemSubmitPerms| X| X| X|  |
| extensionAttribute1| X| X| X|  |
| extensionAttribute10| X| X| X|  |
| extensionAttribute11| X| X| X|  |
| extensionAttribute12| X| X| X|  |
| extensionAttribute13| X| X| X|  |
| extensionAttribute14| X| X| X|  |
| extensionAttribute15| X| X| X|  |
| extensionAttribute2| X| X| X|  |
| extensionAttribute3| X| X| X|  |
| extensionAttribute4| X| X| X|  |
| extensionAttribute5| X| X| X|  |
| extensionAttribute6| X| X| X|  |
| extensionAttribute7| X| X| X|  |
| extensionAttribute8| X| X| X|  |
| extensionAttribute9| X| X| X|  |
| facsimiletelephonenumber| X| X|  |  |
| givenName| X| X|  |  |
| Kotipuhelin| X| X|  |  |
| tiedot| X| X| X| Tämän määritteen tällä hetkellä ei ole käytetty-ryhmissä.|
| Nimikirjaimet| X| X|  |  |
| l| X| X|  |  |
| legacyExchangeDN| X| X| X|  |
| mailNickname| X| X| X|  |
| mangedBy|  |  | X|  |
| hallinta| X| X|  |  |
| jäsen|  |  | X|  |
| Nuolet| X| X|  |  |
| Koska HABSeniorityIndex| X| X| X|  |
| Koska PhoneticDisplayName| X| X| X|  |
| msExchArchiveGUID| X|  |  |  |
| msExchArchiveName| X|  |  |  |
| msExchAssistantName| X| X|  |  |
| msExchAuditAdmin| X|  |  |  |
| msExchAuditDelegate| X|  |  |  |
| msExchAuditDelegateAdmin| X|  |  |  |
| msExchAuditOwner| X|  |  |  |
| msExchBlockedSendersHash| X| X|  |  |
| msExchBypassAudit| X|  |  |  |
| msExchCoManagedByLink|  |  | X|  |
| msExchDelegateListLink| X|  |  |  |
| msExchELCExpirySuspensionEnd| X|  |  |  |
| msExchELCExpirySuspensionStart| X|  |  |  |
| msExchELCMailboxFlags| X|  |  |  |
| msExchEnableModeration| X|  | X|  |
| msExchExtensionCustomAttribute1| X| X| X| Tämän määritteen on tällä hetkellä ole kulutettu Exchange online-palvelun. |
| msExchExtensionCustomAttribute2| X| X| X| Tämän määritteen on tällä hetkellä ole kulutettu Exchange online-palvelun. |
| msExchExtensionCustomAttribute3| X| X| X| Tämän määritteen on tällä hetkellä ole kulutettu Exchange online-palvelun. |
| msExchExtensionCustomAttribute4| X| X| X| Tämän määritteen on tällä hetkellä ole kulutettu Exchange online-palvelun. |
| msExchExtensionCustomAttribute5| X| X| X| Tämän määritteen on tällä hetkellä ole kulutettu Exchange online-palvelun. |
| msExchHideFromAddressLists| X| X| X|  |
| msExchImmutableID| X|  |  |  |
| msExchLitigationHoldDate| X| X| X|  |
| msExchLitigationHoldOwner| X| X| X|  |
| msExchMailboxAuditEnable| X|  |  |  |
| msExchMailboxAuditLogAgeLimit| X|  |  |  |
| msExchMailboxGuid| X|  |  |  |
| msExchModeratedByLink| X| X| X|  |
| msExchModerationFlags| X| X| X|  |
| msExchRecipientDisplayType| X| X| X|  |
| msExchRecipientTypeDetails| X| X| X|  |
| msExchRemoteRecipientType| X|  |  |  |
| msExchRequireAuthToSendTo| X| X| X|  |
| msExchResourceCapacity| X|  |  |  |
| msExchResourceDisplay| X|  |  |  |
| msExchResourceMetaData| X|  |  |  |
| msExchResourceSearchProperties| X|  |  |  |
| msExchRetentionComment| X| X| X|  |
| msExchRetentionURL| X| X| X|  |
| msExchSafeRecipientsHash| X| X|  |  |
| msExchSafeSendersHash| X| X|  |  |
| msExchSenderHintTranslations| X| X| X|  |
| msExchTeamMailboxExpiration| X|  |  |  |
| msExchTeamMailboxOwners| X|  |  |  |
| msExchTeamMailboxSharePointUrl| X|  |  |  |
| msExchUserHoldPolicies| X|  |  |  |
| msOrg IsOrganizational|  |  | X|  |
| objectSID| X|  | X| mekaanisen ominaisuus. AD käyttäjätunnus käyttää ylläpitämään synkronointiin välillä Azure AD- ja AD.|
| oOFReplyToOriginator|  |  | X|  |
| otherFacsimileTelephone| X| X|  |  |
| otherHomePhone| X| X|  |  |
| otherTelephone| X| X|  |  |
| hakulaite| X| X|  |  |
| physicalDeliveryOfficeName| X| X|  |  |
| Postinumero| X| X|  |  |
| proxyAddresses| X| X| X|  |
| publicDelegates| X| X| X|  |
| pwdLastSet| X|  |  | mekaanisen ominaisuus. Milloin kannattaa poistaa jo myönnetty tunnusten käytetään. Salasanan synkronointi ja federation käyttämä.|
| reportToOriginator|  |  | X|  |
| reportToOwner|  |  | X|  |
| securityEnabled|  |  | X| Johdettu groupType|
| SN| X| X|  |  |
| sourceAnchor| X| X| X| mekaanisen ominaisuus. Pysyvä tunnus, jos haluat säilyttää Lisää ja Azure AD välinen suhde.|
| St| X| X|  |  |
| streetAddress| X| X|  |  |
| targetAddress| X| X|  |  |
| telephoneAssistant| X| X|  |  |
| telephoneNumber| X| X|  |  |
| thumbnailphoto| X| X|  |  |
| otsikko| X| X|  |  |
| unauthOrig| X| X| X|  |
| usageLocation| X|  |  | mekaanisen ominaisuus. Käyttäjän maa. Käyttää käyttöoikeusmääritykset.|
| userCertificate| X| X|  |  |
| userPrincipalName| X|  |  | Täydellisen Käyttäjätunnuksen on käyttäjän Kirjautumistunnus. Yleensä sama kuin [sähköpostin] arvo.|
| userSMIMECertificates| X| X|  |  |
| wWWHomePage| X| X|  |  |

## <a name="sharepoint-online"></a>SharePoint Online

| Määritteen nimi| Käyttäjän| Yhteyshenkilö| Ryhmän| Kommentti |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Määrittää, jos tili on käytössä.|
| authOrig| X| X| X|  |
| c| X| X|  |  |
| Kirjautuminen| X|  | X|  |
| Mää| X| X|  |  |
| yrityksen| X| X|  |  |
| countryCode| X| X|  |  |
| osasto| X| X|  |  |
| kuvaus| X| X| X|  |
| Näyttönimi| X| X| X|  |
| dLMemRejectPerms| X| X| X|  |
| dLMemSubmitPerms| X| X| X|  |
| extensionAttribute1| X| X| X|  |
| extensionAttribute10| X| X| X|  |
| extensionAttribute11| X| X| X|  |
| extensionAttribute12| X| X| X|  |
| extensionAttribute13| X| X| X|  |
| extensionAttribute14| X| X| X|  |
| extensionAttribute15| X| X| X|  |
| extensionAttribute2| X| X| X|  |
| extensionAttribute3| X| X| X|  |
| extensionAttribute4| X| X| X|  |
| extensionAttribute5| X| X| X|  |
| extensionAttribute6| X| X| X|  |
| extensionAttribute7| X| X| X|  |
| extensionAttribute8| X| X| X|  |
| extensionAttribute9| X| X| X|  |
| facsimiletelephonenumber| X| X|  |  |
| givenName| X| X|  |  |
| hideDLMembership|  |  | X|  |
| Kotipuhelin| X| X|  |  |
| tiedot| X| X| X|  |
| nimikirjaimet| X| X|  |  |
| ipPhone| X| X|  |  |
| l| X| X|  |  |
| sähköposti| X| X| X|  |
| mailNickname| X| X| X|  |
| managedBy|  |  | X|  |
| hallinta| X| X|  |  |
| jäsen|  |  | X|  |
| middleName| X| X|  |  |
| Nuolet| X| X|  |  |
| msExchTeamMailboxExpiration| X|  |  |  |
| msExchTeamMailboxOwners| X|  |  |  |
| msExchTeamMailboxSharePointLinkedBy| X|  |  |  |
| msExchTeamMailboxSharePointUrl| X|  |  |  |
| objectSID| X|  | X| mekaanisen ominaisuus. AD käyttäjätunnus käyttää ylläpitämään synkronointiin välillä Azure AD- ja AD.|
| oOFReplyToOriginator|  |  | X|  |
| otherFacsimileTelephone| X| X|  |  |
| otherHomePhone| X| X|  |  |
| otherIpPhone| X| X|  |  |
| otherMobile| X| X|  |  |
| otherPager| X| X|  |  |
| otherTelephone| X| X|  |  |
| hakulaite| X| X|  |  |
| physicalDeliveryOfficeName| X| X|  |  |
| Postinumero| X| X|  |  |
| postOfficeBox| X| X|  |  |
| preferredLanguage| X|  |  |  |
| proxyAddresses| X| X| X|  |
| pwdLastSet| X|  |  | mekaanisen ominaisuus. Milloin kannattaa poistaa jo myönnetty tunnusten käytetään. Salasanan synkronointi ja federation käyttämä.|
| reportToOriginator|  |  | X|  |
| reportToOwner|  |  | X|  |
| securityEnabled|  |  | X| Johdettu groupType|
| SN| X| X|  |  |
| sourceAnchor| X| X| X| mekaanisen ominaisuus. Pysyvä tunnus, jos haluat säilyttää Lisää ja Azure AD välinen suhde.|
| St| X| X|  |  |
| streetAddress| X| X|  |  |
| targetAddress| X| X|  |  |
| telephoneAssistant| X| X|  |  |
| telephoneNumber| X| X|  |  |
| thumbnailphoto| X| X|  |  |
| otsikko| X| X|  |  |
| unauthOrig| X| X| X|  |
| URL-osoite| X| X|  |  |
| usageLocation| X|  |  | mekaanisen ominaisuus. Käyttäjän maa. Käyttää käyttöoikeusmääritykset.|
| userPrincipalName| X|  |  | Täydellisen Käyttäjätunnuksen on käyttäjän Kirjautumistunnus. Yleensä sama kuin [sähköpostin] arvo.|
| wWWHomePage| X| X|  |  |

## <a name="lync-online"></a>Lync online-tilassa

| Määritteen nimi| Käyttäjän| Yhteyshenkilö| Ryhmän| Kommentti |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Määrittää, jos tili on käytössä.|
| c| X| X|  |  |
| Kirjautuminen| X|  | X|  |
| Mää| X| X|  |  |
| yrityksen| X| X|  |  |
| osasto| X| X|  |  |
| kuvaus| X| X| X|  |
| Näyttönimi| X| X| X|  |
| facsimiletelephonenumber| X| X| X|  |
| givenName| X| X|  |  |
| Kotipuhelin| X| X|  |  |
| ipPhone| X| X|  |  |
| l| X| X|  |  |
| sähköposti| X| X| X|  |
| mailNickname| X| X| X|  |
| managedBy|  |  | X|  |
| hallinta| X| X|  |  |
| jäsen|  |  | X|  |
| Nuolet| X| X|  |  |
| msExchHideFromAddressLists| X| X| X|  |
| msRTCSIP ApplicationOptions| X|  |  |  |
| msRTCSIP DeploymentLocator| X| X|  |  |
| msRTCSIP-rivi| X| X|  |  |
| msRTCSIP OptionFlags| X| X|  |  |
| msRTCSIP OwnerUrn| X|  |  |  |
| msRTCSIP PrimaryUserAddress| X| X|  |  |
| msRTCSIP-UserEnabled| X| X|  |  |
| objectSID| X|  | X| mekaanisen ominaisuus. AD käyttäjätunnus käyttää ylläpitämään synkronointiin välillä Azure AD- ja AD.|
| otherTelephone| X| X|  |  |
| physicalDeliveryOfficeName| X| X|  |  |
| Postinumero| X| X|  |  |
| preferredLanguage| X|  |  |  |
| proxyAddresses| X| X| X|  |
| pwdLastSet| X|  |  | mekaanisen ominaisuus. Milloin kannattaa poistaa jo myönnetty tunnusten käytetään. Salasanan synkronointi ja federation käyttämä.|
| securityEnabled|  |  | X| Johdettu groupType|
| SN| X| X|  |  |
| sourceAnchor| X| X| X| mekaanisen ominaisuus. Pysyvä tunnus, jos haluat säilyttää Lisää ja Azure AD välinen suhde.|
| St| X| X|  |  |
| streetAddress| X| X|  |  |
| telephoneNumber| X| X|  |  |
| thumbnailphoto| X| X|  |  |
| otsikko| X| X|  |  |
| usageLocation| X|  |  | mekaanisen ominaisuus. Käyttäjän maa. Käyttää käyttöoikeusmääritykset.|
| userPrincipalName| X|  |  | Täydellisen Käyttäjätunnuksen on käyttäjän Kirjautumistunnus. Yleensä sama kuin [sähköpostin] arvo.|
| wWWHomePage| X| X|  |  |

## <a name="azure-rms"></a>Azure RMS

| Määritteen nimi| Käyttäjän| Yhteyshenkilö| Ryhmän| Kommentti |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Määrittää, jos tili on käytössä.|
| Kirjautuminen| X|  | X| Nimi tai tunnus. Useimmin etuliite [sähköpostin]-arvo.|
| Näyttönimi| X| X| X| Merkkijono, joka edustaa usein esitetyt kutsumanimi (Etunimi Sukunimi) nimeä.|
| sähköposti| X| X| X| koko sähköpostiosoitteesi.|
| jäsen|  |  | X|  |
| objectSID| X|  | X| mekaanisen ominaisuus. AD käyttäjätunnus käyttää ylläpitämään synkronointiin välillä Azure AD- ja AD.|
| proxyAddresses| X| X| X| mekaanisen ominaisuus. Käyttää Azure AD. Sisältää kaikki käyttäjän toissijainen sähköpostiosoitteet.|
| pwdLastSet| X|  |  | mekaanisen ominaisuus. Milloin kannattaa poistaa jo myönnetty tunnusten käytetään.|
| securityEnabled|  |  | X| Johdettu groupType.|
| sourceAnchor| X| X| X| mekaanisen ominaisuus. Pysyvä tunnus, jos haluat säilyttää Lisää ja Azure AD välinen suhde.|
| usageLocation| X|  |  | mekaanisen ominaisuus. Käyttäjän maa. Käyttää käyttöoikeusmääritykset.|
| userPrincipalName| X|  |  | Tämä UPN on käyttäjän Kirjautumistunnus. Yleensä sama kuin [sähköpostin] arvo.|

## <a name="intune"></a>Intune

| Määritteen nimi| Käyttäjän| Yhteyshenkilö| Ryhmän| Kommentti |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Määrittää, jos tili on käytössä.|
| c| X| X|  |  |
| Kirjautuminen| X|  | X|  |
| kuvaus| X| X| X|  |
| Näyttönimi| X| X| X|  |
| sähköposti| X| X| X|  |
| mailNickname| X| X| X|  |
| jäsen|  |  | X|  |
| objectSID| X|  | X| mekaanisen ominaisuus. AD käyttäjätunnus käyttää ylläpitämään synkronointiin välillä Azure AD- ja AD.|
| proxyAddresses| X| X| X|  |
| pwdLastSet| X|  |  | mekaanisen ominaisuus. Milloin kannattaa poistaa jo myönnetty tunnusten käytetään. Salasanan synkronointi ja federation käyttämä.|
| securityEnabled|  |  | X| Johdettu groupType|
| sourceAnchor| X| X| X| mekaanisen ominaisuus. Pysyvä tunnus, jos haluat säilyttää Lisää ja Azure AD välinen suhde.|
| usageLocation| X|  |  | mekaanisen ominaisuus. Käyttäjän maa. Käyttää käyttöoikeusmääritykset.|
| userPrincipalName| X|  |  | Täydellisen Käyttäjätunnuksen on käyttäjän Kirjautumistunnus. Yleensä sama kuin [sähköpostin] arvo.|

## <a name="dynamics-crm"></a>Dynamics CRM:

| Määritteen nimi| Käyttäjän| Yhteyshenkilö| Ryhmän| Kommentti |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Määrittää, jos tili on käytössä.|
| c| X| X|  |  |
| Kirjautuminen| X|  | X|  |
| Mää| X| X|  |  |
| yrityksen| X| X|  |  |
| countryCode| X| X|  |  |
| kuvaus| X| X| X|  |
| Näyttönimi| X| X| X|  |
| facsimiletelephonenumber| X| X|  |  |
| givenName| X| X|  |  |
| l| X| X|  |  |
| managedBy|  |  | X|  |
| hallinta| X| X|  |  |
| jäsen|  |  | X|  |
| Nuolet| X| X|  |  |
| objectSID| X|  | X| mekaanisen ominaisuus. AD käyttäjätunnus käyttää ylläpitämään synkronointiin välillä Azure AD- ja AD.|
| physicalDeliveryOfficeName| X| X|  |  |
| Postinumero| X| X|  |  |
| preferredLanguage| X|  |  |  |
| pwdLastSet| X|  |  | mekaanisen ominaisuus. Milloin kannattaa poistaa jo myönnetty tunnusten käytetään. Salasanan synkronointi ja federation käyttämä.|
| securityEnabled|  |  | X| Johdettu groupType|
| SN| X| X|  |  |
| sourceAnchor| X| X| X| mekaanisen ominaisuus. Pysyvä tunnus, jos haluat säilyttää Lisää ja Azure AD välinen suhde.|
| St| X| X|  |  |
| streetAddress| X| X|  |  |
| telephoneNumber| X| X|  |  |
| otsikko| X| X|  |  |
| usageLocation| X|  |  | mekaanisen ominaisuus. Käyttäjän maa. Käyttää käyttöoikeusmääritykset.|
| userPrincipalName| X|  |  | Täydellisen Käyttäjätunnuksen on käyttäjän Kirjautumistunnus. Yleensä sama kuin [sähköpostin] arvo.|

## <a name="3rd-party-applications"></a>3 osapuolien sovellukset
Tämän ryhmän on käytetty yleinen työmäärää tai sovelluksen mahdollisimman vähän määritteet attribuuteista. Sen avulla voidaan kuormituksen, eivät näy toista osaa tai -Microsoft sovelluksen. Sitä käytetään eksplisiittisesti seuraavat asiat:

- Yammer (vain käyttäjän kulutettu)
- [Hybrid Business Business (B2B) rajat organisaatiokaavion yhteistyö tilanteita, joissa esimerkiksi SharePoint tarjoamia](http://go.microsoft.com/fwlink/?LinkId=747036)

Tämän ryhmän on määritteitä, jotka voidaan käyttää, jos Office 365: ssä, Dynamics tai Intune ei käytetä Azure AD-kansio. Siinä on pieni joukko core määritteet.

| Määritteen nimi| Käyttäjän| Yhteyshenkilö| Ryhmän| Kommentti |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Määrittää, jos tili on käytössä.|
| Kirjautuminen| X|  | X|  |
| Näyttönimi| X| X| X|  |
| givenName| X| X|  |  |
| sähköposti| X|  | X|  |
| managedBy|  |  | X|  |
| mailNickName| X| X| X|  |
| jäsen|  |  | X|  |
| objectSID| X|  |  | mekaanisen ominaisuus. AD käyttäjätunnus käyttää ylläpitämään synkronointiin välillä Azure AD- ja AD.|
| proxyAddresses| X| X| X|  |
| pwdLastSet| X|  |  | mekaanisen ominaisuus. Milloin kannattaa poistaa jo myönnetty tunnusten käytetään. Salasanan synkronointi ja federation käyttämä.|
| SN| X| X|  |  |
| sourceAnchor| X| X| X| mekaanisen ominaisuus. Pysyvä tunnus, jos haluat säilyttää Lisää ja Azure AD välinen suhde.|
| usageLocation| X|  |  | mekaanisen ominaisuus. Käyttäjän maa. Käyttää käyttöoikeusmääritykset.|
| userPrincipalName| X|  |  | Täydellisen Käyttäjätunnuksen on käyttäjän Kirjautumistunnus. Yleensä sama kuin [sähköpostin] arvo.|

## <a name="windows-10"></a>Windows 10: ssä
Windows 10: ssä toimialueen liittyneet computer(device) Synkronoi joitakin Azure AD määritteet. Lisätietoja käyttötavoista on artikkelissa [Windows 10: lle Azure AD Connect toimialueeseen liittyneet laitteiden kohtaa](active-directory-azureadjoin-devices-group-policy.md). Nämä määritteet aina Synkronoi ja Windows 10: ssä ei näy sovelluksen, voit poistaa valinnan. Windows 10: ssä toimialueen liitetyissä tietokoneissa tunnistetaan on täydennetty määrite userCertificate.

| Määritteen nimi| Laite| Kommentti |
| --- | :-: | --- |
| accountEnabled| X| |
| deviceTrustType| X| Toimialueen liittyneet tietokoneet englanninkielisissä arvo. |
| Näyttönimi | X| |
| MS-DS-CreatorSID | X| Kutsutaan myös registeredOwnerReference.|
| objectGUID | X| Kutsutaan myös deviceID.|
| objectSID | X| Kutsutaan myös onPremisesSecurityIdentifier.|
| sovelluksena | X| Kutsutaan myös deviceOSType.|
| operatingSystemVersion | X| Kutsutaan myös deviceOSVersion.|
| userCertificate | X| |

**Käyttäjän** seuraavanlaisia määritteitä on lisäksi muista sovelluksista on valittuna.  

| Määritteen nimi| Käyttäjän| Kommentti |
| --- | :-: | --- |
| domainFQDN| X| Kutsutaan myös dnsDomainName. Esimerkiksi contoso.com.|
| domainNetBios| X| Kutsutaan myös netBiosName. Esimerkiksi CONTOSO.|

## <a name="exchange-hybrid-writeback"></a>Exchange hybrid takaisinkirjoituksen
Nämä määritteet kirjoitetaan takaisin Azure AD-paikallisen Active Directory käyttöön **Exchange hybrid**valittaessa. Exchange-versiosi riippuen ehkä synkronoitava vähemmän määritteet.

| Määritteen nimi| Käyttäjän| Yhteyshenkilö| Ryhmän| Kommentti |
| --- | :-: | :-: | :-: | --- |
| Koska ExternalDirectoryObjectID| X|  |  | Johdettu cloudAnchor Azure AD. Tämän määritteen on uusi Exchange 2016.|
| msExchArchiveStatus| X|  |  | Online-arkiston: Avulla asiakkaat voivat arkistoida sähköposti.|
| msExchBlockedSendersHash| X|  |  | Suodatus: Kirjoittaa takaisin paikalliseen suodattaminen ja online Turvalliset sekä estettävät lähettäjän tietojen asiakkaiden.|
| msExchSafeRecipientsHash| X|  |  | Suodatus: Kirjoittaa takaisin paikalliseen suodattaminen ja online Turvalliset sekä estettävät lähettäjän tietojen asiakkaiden.|
| msExchSafeSendersHash| X|  |  | Suodatus: Kirjoittaa takaisin paikalliseen suodattaminen ja online Turvalliset sekä estettävät lähettäjän tietojen asiakkaiden.|
| msExchUCVoiceMailSettings| X|  |  | Ota käyttöön yhdistetyn viestinnän (UM) - Online vastaajaan: käyttämän Microsoft Lync Server integrointi Lync Serveriin osoittamaan paikallisen, että käyttäjällä on vastaajan online Services-palveluissa.|
| msExchUserHoldPolicies| X|  |  | Oikeustoimiin liittyvän pidon: Pilvipalveluihin avulla voit määrittää käyttäjät, jotka sijaitsevat oikeustoimiin liittyvän pitkään.|
| proxyAddresses| X| X| X| Vain Exchange Online-sivuston osoite on lisätty x500.|

## <a name="device-writeback"></a>Laitteen takaisinkirjoituksen
Laitteen objektit luodaan Active Directoryssa. Nämä objektit voi olla yhdistetty Azure AD laitteet tai toimialueeseen liittyneet Windows 10-tietokoneissa.

| Määritteen nimi| Laite| Kommentti |
| --- | :-: | --- |
| altSecurityIdentities | X| |
| Näyttönimi | X| |
| DN | X| |
| Koska CloudAnchor | X| |
| Koska DeviceID | X| |
| Koska DeviceObjectVersion | X| |
| Koska DeviceOSType | X| |
| Koska DeviceOSVersion | X| |
| Koska DevicePhysicalIDs | X| |
| Koska KeyCredentialLink | X| Vain Windows Server 2016 AD rakenteen kanssa |
| Koska IsCompliant | X| |
| Koska IsEnabled | X| |
| Koska IsManaged | X| |
| Koska RegisteredOwner | X| |


## <a name="notes"></a>Huomautuksia

- Kun käytät vaihtoehtoinen tunnus, paikallisen määrite userPrincipalName synkronoidaan Azure AD-määrite onPremisesUserPrincipalName. Esimerkki sähköpostin vaihtoehtoinen tunnus-määrite synkronoidaan Azure AD-määrite userPrincipalName.
- Olevista luetteloista objektityyppi **käyttäjän** koskee myös objektin tyyppi **inetOrgHenkilö**.

## <a name="next-steps"></a>Seuraavat vaiheet
Lisätietoja [Azure AD Connect synkronointi](active-directory-aadconnectsync-whatis.md) -määritys.

Lisätietoja [ympäristön integroinnissa paikallisen-käyttäjätietojen Azure Active Directory-hakemistosta](active-directory-aadconnect.md).

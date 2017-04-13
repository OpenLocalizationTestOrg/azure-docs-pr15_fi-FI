<properties
   pageTitle="Azure Active Directory valvonta raportin tapahtumat | Microsoft Azure"
   description="Valvottavien tapahtumien, jotka ovat käytettävissä tarkasteleminen ja lataaminen Azure Active Directorysta"
   services="active-directory"
   documentationCenter=""
   authors="dhanyahk"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/19/2016"
   ms.author="dhanyahk"/>

# <a name="azure-active-directory-audit-report-events"></a>Azure Active Directory valvonta raportin tapahtumat

*Näissä ohjeissa on osa [Azure Active Directory raportoinnin oppaan] (aktiivinen-hakemisto-raportointi-guide.md).*

Azure Active Directory valvontalokiraportti auttaa asiakkaita tunnistaa sellaisten toimintojen niiden Azure Active Directory on tapahtunut. Sellaisten toiminnot ovat käyttöoikeuksien muutokset (esimerkiksi roolin luominen tai salasanan vaihtamisesta), muuttaminen asetukset (esimerkiksi Salasanakäytännöt) tai muutokset directory kokoonpanon (esimerkiksi toimialueen federation asetusten muutokset). Valvonta-tietueen raporteissa tapahtuman nimi, joka suorittaa toiminnon, muuta- ja päivämäärä ja kellonaika (UTC-ajan) vaikuttaa kohde resurssin toimija varten. Asiakkaat voivat hakea valvontalokin tapahtumien luetteloon niiden Azure Active Directory [Azure-portaalin](https://portal.azure.com/)kautta kuvatulla tavalla [näkymän valvonta-lokit](active-directory-reporting-azure-portal.md).


## <a name="list-of-audit-report-events"></a>Valvontalokien raportin tapahtumaluettelon
<!--- audit event descriptions should be in the past tense --->

Tapahtumat                               | Tapahtuman kuvaus
------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------
**Käyttäjän tapahtumat**                      |
Käyttäjän lisääminen                             | Lisätä käyttäjän kansio.
Poista käyttäjä                          | Poistaa käyttäjän kansio.
Käyttöoikeus-ominaisuuksien määrittäminen               | Käyttöoikeus-ominaisuuksien määrittäminen käyttäjän hakemistossa.
Käyttäjän salasanan vaihtaminen                  | Hakemisto-käyttäjän salasanan palauttaminen.
Käyttäjän salasanan muuttaminen                 | Muuttaa hakemisto-käyttäjän salasanan.
Muuta käyttöoikeus                  | Muuttaa hakemistossa käyttäjälle käyttöoikeus. Jos haluat nähdä käyttöoikeudet on päivitetty, tarkista [päivityksen käyttäjän](#update-user-attributes) ominaisuudet alla
Päivitä käyttäjä                          | Päivittää käyttäjän kansio. [Noudata seuraavia ohjeita](#update-user-attributes) määritteitä, jotka voidaan päivittää.
Määritä voimassa-käyttäjän salasanan muuttaminen       | Ominaisuuden, joka pakottaa käyttäjä voi vaihtaa oman salasanansa, kun kirjaudut sisään.
Päivitä käyttäjän tunnistetietoja        | Käyttäjä on muuttanut salasanan  
**Ryhmän tapahtumien**                     |
Lisää-ryhmä                            | Luo ryhmä hakemistossa.
Päivitä ryhmä                         | Päivitetty ryhmän hakemistossa. Jos haluat nähdä, mitkä ominaisuudet on päivitetty, viitata alla olevasta osiosta- [Ryhmän ominaisuudet tapahtumista](#update-group-attributes)
Ryhmän poistaminen                         | Poistaa ryhmän hakemiston.
Jäsenen lisääminen ryhmään            | Lisätä jäsenen hakemistossa.
Jäsenen poistaminen ryhmästä         | Poistaa hakemiston ryhmän jäsen.
CreateGroupSettings              | Luotu ryhmäasetukset
UpdateGroupSettings          | Päivitetty ryhmäasetuksia. Jos haluat nähdä, mitä asetuksia on päivitetty, katso alla olevasta osiosta- [Ryhmän ominaisuudet tapahtumista](#update-group-attributes)
DeleteGroupSettings            | Poistetun ryhmäasetukset
SetGroupLicense                  | Määritä ryhmän käyttöoikeus.
SetGroupManagedBy              | Tuleeko käyttäjän ryhmän.
AddGroupMember               | Ryhmän jäsen on lisätty
RemoveGroupMember            | Jäsenen poistaminen ryhmästä
AddGroupOwner                | Lisätty ryhmän omistajaa
RemoveGroupOwner                 | Poistetun ryhmän omistaja
**Sovelluksen tapahtumat**               |
Lisää palvelu lyhennyksen.                | Palvelun lyhennys lisätä hakemiston.
Poista palvelun lyhennyksen.             | Poistaa palvelun lyhennys hakemiston.
Palvelun pääasiallista tunnistetietojen lisääminen    | Palvelun lyhennys lisätty tunnukset.
Poista palvelun pääasiallista käyttäjätiedot | Poistetun tunnistetiedot principal-palvelusta.
Lisää delegointi merkintä                 | Luotu [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permissiongrant-entity) hakemistossa.
Delegointi tapahtuma                 | Päivittää [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permissiongrant-entity) hakemistossa.
Poistaa delegointi              | Poistaa [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permissiongrant-entity) hakemistossa.
**Rooli tapahtumat**                      |
Lisää rooliin roolin jäsen              | Lisätty käyttäjän kansio roolin.
Roolin jäsen poistaminen roolista         | Poistaa käyttäjän kansio rooli.
Määritä yrityksen yhteystiedot      | Määritä yritystason yhteyshenkilön asetuksissa. Tämä sisältää Microsoft Online Services markkinoinnin sekä teknisiä ilmoituksia sähköpostiosoitteet.
Lisää delegointi merkintä                 | Luotu [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#OAuth2PermissionGrantEntity) hakemistossa.
Delegointi tapahtuma                 | Päivittää [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#OAuth2PermissionGrantEntity) hakemistossa.
Poistaa delegointi              | Poistaa [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#OAuth2PermissionGrantEntity) hakemistossa.
AddSevicePrincipalOwner          | Palvelun lyhennys lisätty omistajaa.
RemoveSevicePrincipalOwner   | Poistaa palvelun lyhennys omistaja.
AddApplication  | Lisää sovellus.
UpdateApplication | Päivitä sovellus. Jos haluat nähdä, mitä app-asetuksista on päivitetty, katso alla olevasta osiosta- [Sovelluksen ominaisuudet tapahtumista](#update-application-attributes)
DeleteApplication | Poista sovellus.
RestoreApplication|Palauta sovellus.
AddApplicationOwner   |     Omistajan lisääminen sovellukseen.
RemoveApplicationOwner    |     Poista omistaja-sovelluksesta.
**Rooli tapahtumat**                         |
Lisää rooliin roolin jäsen                 | Lisätty käyttäjän kansio roolin.
Roolin jäsen poistaminen roolista            | Poistaa käyttäjän kansio rooli.
AddRoleDefinition               | Lisätty roolimääritys.
UpdateRoleDefinition                | Päivitetty roolimääritys. Jos haluat nähdä, mitä roolin asetukset on päivitetty, viitata, kohdassa [Roolin määritys ominaisuudet tapahtumista](#update-role-definition-attributes)
DeleteRoleDefinition                | Poistaa roolimääritys.
AddRoleAssignmentToRoleDefinition | Lisätty roolimääritys roolimääritys avulla.
RemoveRoleAssignmentFromRoleDefinition | Poistetun roolimääritys roolimääritys kohteesta.
AddRoleFromTemplate     | Lisätty rooli mallista.
UpdateRole  | Päivitetty rooli.
AddRoleScopeMemberToRole    | Kohdistetuissa jäsen lisätty rooliin.
RemoveRoleScopedMemberFromRole  | Poistaa alueen jäsentä rooli.
**Laitteen tapahtumat (kaikille uusille tapahtumille)**                    |
AddDevice-toiminto               | Lisätty laite.
UpdateDevice            | Päivitetty laite. Jos haluat nähdä, mitä laitteiden ominaisuuksia on päivitetty, viitata alla olevasta osiosta [laitteiden ominaisuuksia Audited](#update-device-attributes)
DeleteDevice            | Poistetun laite.
AddDeviceConfiguration      | Lisätty laitemääritys.
UpdateDeviceConfiguration   | Päivitetty laitteen määritys. Jos haluat nähdä ominaisuudet laitteen määritys on päivitetty, viitata alla olevasta osiosta [laitteen määritysten ominaisuudet Audited](#update-device-configuration-attributes)
DeleteDeviceConfiguration   | Poistetun laitteen määritys.
AddRegisteredOwner     | Lisätty rekisteröity omistaja laitteeseen.
AddRegisteredUsers     | Rekisteröityneet käyttäjät lisätään laite.
RemoveRegisteredOwner    | Poista laitteen rekisteröity omistaja.
RemoveRegisteredUsers    | Poista laitteen rekisteröityneet käyttäjät.
RemoveDeviceCredentials  | Poista laitteen tunnistetiedot.
**B2B tapahtumat**                       |
Erän kutsut ladattu.              | Järjestelmänvalvoja on ladattu tiedosto, joka sisältää kutsujen lähettämisen kumppanin käyttäjät.
Erän kutsut käsitteleminen.             | Kumppanin käyttäjien kutsujen sisältävän tiedoston käsitelty.
Kutsu ulkoinen käyttäjä.                | Ulkoinen käyttäjä on kutsuttu hakemiston.
Lunasta ulkoisen käyttäjän kutsu.         | Ulkoinen käyttäjä on tuoteavaimella niiden kutsun hakemiston.
Ulkoisten käyttäjien lisääminen ryhmään.          | Ulkoinen käyttäjä on määritetty jäsenyyden hakemiston ryhmään.
Määritä ulkoisen käyttäjän sovelluksen. | Ulkoinen käyttäjä on määritetty suoraan access-sovellukseen.
Virusperäisen Palveltavien kohteiden luominen.               | Uuden vuokraajan on luotu Azure AD-kutsu lunastushinta.
Virusperäisen käyttäjän luominen.                 | Käyttäjä on luotu aiemmin alihallinnan Azure AD-kutsu lunastushinta.
**Järjestelmänvalvojan resurssit (kaikille uusille tapahtumille)**             |
AddAdministrativeUnit   |   Lisää järjestelmänvalvojan yksikkö.
UpdateAdministrativeUnit|   Päivitä hallinnollinen yksikkö. Jos haluat nähdä, mitä järjestelmänvalvojan yksikkö-ominaisuuksia on päivitetty, viitata kohdan [yksikön hallintaominaisuudet tapahtumista](#update-administrative-unit-attributes)
DeleteAdministrativeUnit|   Poista järjestelmänvalvojan yksikkö.
AddMemberToAdministrativeUnit|  Jäsenen lisääminen hallinnollinen yksikkö.
RemoveMemberFromAdministrativeUnit|     Poista jäsen hallinnollinen yksikkö.
**Kansion tapahtumat**                 |
Kumppanin lisääminen yritykseen               | Kumppanin lisätä hakemiston.
Kumppanin poistaminen yrityksen          | Poistaa kumppanin hakemiston.
DemotePartner   |   Siirrä alemmalle tasolle kumppani.
Yrityksen toimialueen lisääminen                | Lisännyt toimialueen hakemiston.
Yrityksen toimialueen poistaminen           | Poistaa toimialueen hakemiston.
Päivitä toimialueen                        | Päivittää kansio toimialueella. Jos haluat nähdä, mitä toimialueen ominaisuudet on päivitetty, viitata-jäljempänä olevaan kohtaan [toimialueen ominaisuudet tapahtumista](#update-domain-attributes)
Määritä toimialue-todennus            | Voit muuttaa yrityksen toimialueen oletusasetusta.
Määritä yrityksen yhteystiedot      | Määritä yritystason yhteyshenkilön asetuksissa. Tämä sisältää Microsoft Online Services markkinoinnin sekä teknisiä ilmoituksia sähköpostiosoitteet.
Toimialueen federation asetusten määrittäminen    | Päivitetty toimialueen federation-asetukset.
Vahvista toimialue                        | Vahvistettu toimialue-kansioon.
Tarkista sähköpostin vahvistama.         | Vahvistettu toimialue käyttämällä sähköpostin vahvistus-kansioon.
Yrityksen DirSyncEnabled merkinnän määrittäminen   | Ominaisuuden, jonka avulla saat Azure AD-synkronointi hakemisto.
Määritä Salasanakäytäntö                  | Määrittää käyttäjän salasanan pituus ja merkin rajoitukset.
Määritä yrityksen tiedot              | Päivitetty yritystason tiedot. Katso lisätietoja [Get-MsolCompanyInformation](https://msdn.microsoft.com/library/azure/dn194126.aspx) PowerShell cmdlet-komennon.
SetCompanyAllowedDataLocation  |    Määritä yrityksen sallittu tietojen sijainti.
SetCompanyDirSyncEnabled    |   DirSyncEnabled-merkinnän määrittäminen.
SetCompanyDirSyncFeature |  Määritä DirSync-ominaisuus.
SetCompanyInformation |   Määritä yrityksen tiedot.
SetCompanyMultiNationalEnabled    |     Määritä yrityksen kansainvälisissä ominaisuus on käytössä.
SetDirectoryFeatureOnTenant   |     Määritä hakemisto-ominaisuuden vuokraajan.
SetTenantLicenseProperties  |   Vuokraajan käyttöoikeuden ominaisuuksien määrittäminen.
CreateCompanySettings   |   Luo yrityksen asetukset
UpdateCompanySettings   |   Päivitä yrityksen asetukset. Jos haluat nähdä ominaisuudet yrityksen on päivitetty, viitata, [valvoa yrityksen ominaisuudet](#update-company-attributes) kohdassa
DeleteCompanySettings   |   Poista yrityksen asetukset
SetAccidentalDeletionThreshold   |  Määritä vahingossa raja-arvon.
SetRightsManagementProperties   |   Sisältöoikeuksien hallinnan ominaisuuksien määrittäminen.
PurgeRightsManagementProperties     |   Tyhjennä sisältöoikeuksien hallinnan ominaisuudet.
UpdateExternalSecrets   |   Päivitä ulkoisia tietoja
**Käytännön tapahtumat (kaikille uusille tapahtumille)**           |
AddPolicy   |   Lisää käytännön.
UpdatePolicy    |   Päivitä käytännön.
DeletePolicy    |   Poista käytäntö.
AddDefaultPolicyApplication     |   Käytännön lisääminen sovellukseen.
AddDefaultPolicyServicePrincipal    |   Lisää käytännön palvelun lyhennyksen.
RemoveDefaultPolicyApplication  |   Poista käytännön sovelluksen.
RemoveDefaultPolicyServicePrincipal     |   Poista palvelun lyhennys käytännön.
RemovePolicyCredentials     |   Poistaa käytännön tunnistetiedot.

## <a name="audit-report-retention"></a>Raportin säilytys valvonta
Azure AD-valvontaraportin tapahtumat säilyvät 180 päivän ajan. Jos haluat lisätietoja raporttien säilytys artikkelissa [Azure Active Directory raportti-palvelimen säilytyskäytäntöjä](active-directory-reporting-retention.md).

Asiakkaiden tallentaa niiden valvontalokin tapahtumien säilytys pitkällä raportointi-Ohjelmointirajapinnan voidaan hakemaan valvontalokin tapahtumien säännöllisesti erillinen tietojen säilöön. Katso lisätietoja [Raportointi-Ohjelmointirajapinnan käytön aloittaminen](active-directory-reporting-api-getting-started.md) .

## <a name="properties-included-with-each-audit-event"></a>Valvontalokien kuhunkin tapahtumaan kuuluva ominaisuudet

Ominaisuus      | Kuvaus
------------- | --------------------------------------------------------------
Päivämäärä ja aika | Päivämäärä ja aika, valvonta tapahtuman
Toimija         | Käyttäjän tai palvelu, joka suorittaa toiminnon lyhennyksen.
Toiminto        | Toiminto, joka on suoritettu
Kohde        | Käyttäjän tai palvelu, joka on suoritettu toiminto lyhennyksen.


## <a name="update-user-attributes"></a>"Päivitä käyttäjä" määritteet
"Päivitys käyttäjä-valvonta-tapahtuma on lisätietoja siitä, mitä käyttäjä määritteitä on päivitetty. Kunkin määritteen edellisen arvon ja uusi arvo on mukana.

Määrite                           | Kuvaus
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AccountEnabled                      | Käyttäjä voi kirjautua sisään.
AssignedLicense                     | Kaikki käyttöoikeudet, jotka on määritetty käyttäjälle.
AssignedPlan                        | Palvelun suunnitelmat tuloksena käyttäjä saa käyttöoikeudet.
LicenseAssignmentDetail             | Käyttöoikeuksien käyttäjä saa tiedot. Esimerkiksi jos ryhmän perustuva käyttöoikeudet mukana, tämä koskee ryhmä, johon myönnetty käyttöoikeus.
Matkapuhelin                              | Käyttäjän Matkapuhelin.
OtherMail                           | Käyttäjän vaihtoehtoinen sähköpostiosoite.
OtherMobile                         | Käyttäjän vaihtoehtoinen Matkapuhelin.
StrongAuthenticationMethod          | Luettelo vahvistus tavoista määritetty käyttäjän Monimenetelmäisen todentamisen, kuten äänipuhelu, SMS tai vahvistusta koodilla mobiilisovelluksessa.
StrongAuthenticationRequirement     | Jos Monimenetelmäisen todentamisen on käytössä, käytössä vai poissa käytöstä kyseisen käyttäjän.
StrongAuthenticationUserDetails     | Käyttäjän puhelinnumero, vaihtoehtoinen puhelinnumero ja sähköpostiosoite Monimenetelmäisen todentamisen ja salasanan palauttaminen toimialueesta.
StrongAuthenticationPhoneAppDetail  | Tiedot forof puhelimen sovellukset rekisteröity suorittamiseen 2FA kirjautuminen
TelephoneNumber                     | Käyttäjän puhelinnumero.
AlternativeSecurityId               | Objektin vaihtoehtoinen Suojaustunnus.
CreationType                        |Käyttäjä (joko pyytämällä tai virusperäisen) luonti-menetelmää.
InviteTicket                        |Kutsu liput käyttäjän luettelo.
InviteReplyUrl                      |Kutsun hyväksymisen yhteydessä vastaa URL-osoitteet.
InviteResources                     |Luettelon, johon käyttäjä on kutsuttu resursseista.
LastDirSyncTime                     |Objekti on päivitetty vuoksi synkronoidaan kohteesta tärkeimpien viimeksi directory (asiakas, paikallinen).
MSExchRemoteRecipientType           |Karttoja, MSO vastaanottajan käyttämiseen. Viitata [MSO vastaanottajan tyypit] https://msdn.microsoft.com/library/microsoft.office.interop.outlook.recipient.type.aspx vastaanottajan tyypeissä
PreferredDataLocation               |Ensisijainen sijainti käyttäjän, ryhmän, yhteyshenkilön, yleiseen kansioon tai laitteen tiedot.
ProxyAddresses                      |Osoite, jolla vastaanottajan Exchange Server-objektin tunnistetaan ulkomaan sähköpostijärjestelmästä.
StsRefreshTokensValidFrom           |Suorittaa Päivitä suojaustunnuksen kumoamistiedot – minkä tahansa STS päivityksen tunnusten myönnetty ennen tällä hetkellä pidetään vanhentunut.
UserPrincipalName                   |Täydellisen Käyttäjätunnuksen, joka on Internet-tyyli-kirjautumisnimi käyttäjän.
UserState                           |Käyttäjän odottaa hyväksyntää/PendingAcceptance/hyväksytty/PendingVerification tila.
UserStateChangedOn                  |Viimeisin muutos UserState aikaleima. Elinkaari-työnkulkujen käynnistys käytetään.
UserType                            |Käyttäjätyyppi. Jäsen (0), Vieras (1), Viral(2).

## <a name="update-group-attributes"></a>"Päivitä ryhmän" määritteet
Määrite                           | Kuvaus
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
Luokitus            | Luokitus yhdistetyssä ryhmän (HBI, MBI jne.).
Kuvaus               | Tietoja objektin luettavassa kuvaava lauseita.  
Näyttönimi               |Objektin näyttönimi
DirSyncEnabled            |Ilmaisee ilmeneekö synkronointi tärkeä-kansio (asiakas, paikallinen).
GroupLicenseAssignment    |Ryhmän käyttöoikeusmääritykset
GroupType                 |Tyypin, yhdistetyn (0)
IsMembershipRuleLocked    |Ilmaisee, että MembershipRule-ominaisuus on määritetty Omatoiminen ryhmän management-palvelun ja käyttäjät eivät voi muuttaa. Koskevat vain ryhmiä, jossa GroupType sisältää GroupType.DynamicMembership
IsPublic                  |Merkintä, joka osoittaa, jos ryhmä on julkinen/yksityinen.
LastDirSyncTime           |Objekti on päivitetty synkronoidaan kohteesta tärkeimpien viimeksi (paikalliseen asiakas)-hakemisto.
Sähköposti                      |Ensisijainen sähköpostiosoite
MailEnabled               |Ilmaisee, onko tämän ryhmän sähköposti-ominaisuutta.
MailNickname              |Address book objektin monikeria, yleensä sähköpostin nimessä edellisen osan "@" symboli.   
MembershipRule            |Merkkijono ilmoittaminen käyttämät Omatoiminen ryhmän hallintapalvelun määrittää tämän ryhmän jäsenet, joita on kuuluttava ehdot. Katso myös IsMembershipRuleLocked. Koskee vain ryhmiä, jossa GroupType sisältää GroupType.DynamicMembership.
MembershipRuleProcessingState| Arvoluettelon määrittämiä Omatoiminen ryhmän hallintapalvelun, käsittelyn ryhmän jäsenyyden tilan määrittämisestä. Koskee vain ryhmiä, jossa GroupType sisältää GroupType.DynamicMembership.
ProxyAddresses            |Osoite, jolla vastaanottajan Exchange Server-objektin tunnistetaan ulkomaan sähköpostijärjestelmästä.
RenewedDateTime           |Kun ryhmä viimeksi uusia aikaleima-tietue.   
SecurityEnabled           |Ilmaisee, onko ryhmän jäsenyyden voivat vaikuttaa luvan päätöksiä.
WellKnownObject           |Hakemisto-objektia, nimeävät yhdeksi valmiiksi määritettyjä nimet.

## <a name="update-device-attributes"></a>"Päivitä laitteen" määritteet
Määrite                           | Kuvaus
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AccountEnabled | Ilmaisee, onko suojauksen lyhennys voi todentaa.
CloudAccountEnabled | Ilmaisee, onko suojauksen lyhennys voi todentaa. Kirjoittama InTune, kun laite on selvittänyt paikalliseen.
CloudDeviceOSType | Laitteen lajin perusteella käyttöjärjestelmän esimerkiksi Windows RT-iOS. Kun määrittää pilvipalvelussa (kuten Intuneen), tämän määritteen tulee tärkeimpien hakemistossa DeviceOSType.
CloudDeviceOSVersion | Käyttöjärjestelmän versio. Kun määrittää pilvipalvelussa (kuten Intuneen), tämän määritteen tulee tärkeimpien hakemistossa DeviceOSVersion.
CloudDisplayName | Luvun näyttönimi LDAP-määritteen arvo. Kun määrittää pilvipalvelussa (kuten Intuneen), tämän määritteen tulee tärkeimpien näyttönimi hakemistossa.
CloudCreated |Ilmaisee, onko objekti on luotu pilvipalveluihin.
CompliantUntil | Mitä saakka laitteen katsotaan yhteensopiva.
DeviceMetadata |Mukautetun laitteen metatiedot
DeviceObjectVersion | Tämän määritteen käytetään laitteen rakenne-version selvittäminen.
DeviceOSType |Laitteen lajin perusteella käyttöjärjestelmän esimerkiksi Windows RT-iOS. Rekisteröintipalvelua kirjoittama ja tarkoituksena on päivitettävä MDM hallintapalvelun tai STS Vaalea hallintapalvelun.
DeviceOSVersion |Käyttöjärjestelmän versio. Rekisteröintipalvelua kirjoittama ja tarkoituksena on päivitettävä MDM hallintapalvelun tai STS Vaalea hallintapalvelun.
DevicePhysicalIds |Moniarvoisen määrite on tarkoitus tallentaa fyysinen laite tunnukset. Tämä voi sisältyä tunnukset, TPM thumbprints, laitteiston BIOS tietyn tunnukset jne.
DirSyncEnabled | Ilmaisee, onko synkronointi tapahtuu tärkeimpien (asiakkaalta, paikalliseen) hakemisto.
Näyttönimi |Objektin näyttönimi
IsCompliant | Tämän määritteen käytetään laitteen mobiililaitteiden hallinta-tilan hallinta.
IsManaged |Tämän määritteen käytetään osoittamaan laitteen hallitsee pilvestä mobiililaitteiden
LastDirSyncTime |Objekti on päivitetty vuoksi synkronoidaan kohteesta tärkeimpien viimeksi (paikalliseen asiakas)-hakemisto.

## <a name="update-device-configuration-attributes"></a>"Päivitä laitteen kokoonpanotietoja" määritteet
Määrite                           | Kuvaus
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
MaximumRegistrationInactivityPeriod |Laite ei ehkä ole aktiivinen ennen sen päivien enimmäismäärän pidetään poistamista varten.
RegistrationQuota   |Käytännön avulla laitteen merkintöjen yksittäisen käyttäjän sallittu enimmäismäärä.

## <a name="update-service-principal-configuration-attributes"></a>"Päivitä palvelun pääasiallista määritykset" määritteet
Määrite                           | Kuvaus
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AccountEnabled |Ilmaisee, onko suojauksen lyhennys voi todentaa.
AppPrincipalId | Suojauksen lyhennys ulkoinen, sovelluksen määrittämä tunnistetiedot.
Näyttönimi |Objektin näyttönimi
ServicePrincipalName    | Palvelun pääasiallista nimi, sisältävät "nimi/myöntäjä", jossa nimi määrittää sovelluksen luokan arvon ja myöntäjä sisältää vähintään isäntänimi [: portti] tai "nimi", joka määrittää palvelun lyhennys tunniste.

## <a name="update-app-attributes"></a>"Päivittää" määritteet
Määrite                           | Kuvaus
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AppAddress |(Uudelleenohjaus URL-osoitteet) osoitteet, jotka on määritetty palvelun lyhennys joukko.
Sovelluksen tunnus                               |Sovelluksen tunnus   
AppIdentifierUri | Sovelluksen URI, joka yksilöi sovelluksen.  Se on yleensä sovelluksen Accessin URL-osoite.
AppLogoUrl |CDN tallennetaan sovelluksen logon kuvan URL-osoite.
AvailableToOtherTenants | TOSI sovellus on usean vuokraajan-sovellus (eli voidaan käyttää muiden omistajien).
Näyttönimi | Sovelluksen nimi näyttönimi
Oikeuden |Sovelluksen oikeuksista luettelo.
ExternalUserAccountDelegationsAllowed |Merkitse, joka ilmaisee, onko resurssi-sovelluksen luotettu yhden ja luoda delegointimerkintöjen ulkoisten käyttäjätilien.
GroupMembershipClaims |Ryhmäjäsenyyden väittää käytännön.
PublicClient | TOSI, jos asiakas ei voi pitää salainen (eli luottamukselliset tiedot asiakas-OAuth2.0)
RecordConsentConditions | Ehdot-määrityksen mukaisesti sopimuksen ehdot suostumusta tyypit: mitään (0), SilentConsentForPartnerManagedApp(1). Tämä arvo tarjoamia Graph API-rakenne ja voi olla vain määrittäminen tai muuttaa sitä vuokraajan järjestelmänvalvoja.
RequiredResourceAccess |XML-sisällön RequiredResourceAccess-ominaisuuden arvon.
Web App-sovelluksen |Jos arvo on TOSI, ilmaisee, että sovellus on verkkosovellukseen.
WwwHomepage |Ensisijainen Web-sivu.

## <a name="update-role-attributes"></a>"Päivitä rooli" määritteet
Määrite                           | Kuvaus
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AppAddress |(Uudelleenohjaus URL-osoitteet) osoitteet, jotka on määritetty palvelun lyhennys joukko.
BelongsToFirstLoginObjectSet |Jos arvo on TOSI, ilmaisee, että objekti kuuluu tarvitse ottaa käyttöön uuden vuokraajan ensimmäisen järjestelmänvalvojan kirjautuminen objektijoukon.
Sisäiset |Ilmaisee, onko objektin elinkaaren omistaa järjestelmä.
Kuvaus | Tietoja objektin luettavassa kuvaava lauseita.  
Näyttönimi |Objektin näyttönimi
MailNickname | Address book objektin monikeria, yleensä sähköpostin nimessä edellisen osan "@" symboli.
RoleDisabled |Ilmaisee, onko rooli huomiotta tarkoitetaan access tarkistukset.
RoleTemplateId | Roolimallin tunnistetiedot.
ServiceInfo |Palvelukohtaisia valmistelu tiedot, jotka on käytetty MOAC ja/tai muiden palveluesiintymiä (tyyppisiä saman tai toisen palvelun) mukaan.
TaskSetScopeReference |Tunnistaa TaskSet ja alueet, jotka liittyvät rooli tai RoleTemplate joukko.
ValidationError |Tietoja useista lähteistä palvelu ei ole lyhytaikainen, palvelukohtaisia virhe ominaisuudet tai objektin järjestelmänvalvojan toiminnon ratkaisemiseksi linkin kuvaava julkaisema.
WellKnownObject |Hakemisto-objektia, nimeävät yhdeksi valmiiksi määritettyjä nimet.

## <a name="update-role-definition-attributes"></a>"Päivitä roolimääritys" määritteet
Määrite                           | Kuvaus
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AssignableScopes    |Käyttöoikeuksien käyttöalueet, jota voidaan käyttää kun tämä RoleDefinition määritteleminen suojauksen lyhennys kokoelma.
Näyttönimi     |Objektin näyttönimi
GrantedPermissions  |Tämä RoleDefinition myöntämien oikeuksien.


## <a name="update-administrative-unit-attributes"></a>"Päivitä järjestelmänvalvojan yksikköä" määritteet
Määrite                           | Kuvaus
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
Kuvaus |   Tämä ominaisuus päivitetään, kun muutat hallinnollinen yksikkö kuvaus.
Näyttönimi |   Tämä ominaisuus päivitetään, kun muutat järjestelmänvalvojan yksikön nimeä.

## <a name="update-company-attributes"></a>"Päivitä yritys" määritteet
Määrite                           | Kuvaus
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AllowedDataLocation     | Sijainti, johon yrityksen käyttäjät voivat olla valmistelun yhteydessä.
AuthorizedServiceInstance   | Palvelun esiintymät, johon suunnitelma otetaan käyttöön nimet.
DirSyncEnabled |Ilmaisee, onko synkronointi tapahtuu tärkeimpien (asiakkaalta, paikalliseen) hakemisto.
DirSyncStatus |Ilmaisee, onko synkronointi osoite kirjan objektien vuokraajan tässä yhteydessä ilmenee tärkeimpien (asiakkaalta, paikalliseen) kansioon. laajentamista yhtiön objektit DirSyncEnabled-ominaisuuden.
DirSyncFeatures |Lippu seurantaan vuokraajan käytössä ja poissa käytöstä Dirsync-työkalun ominaisuuksien määrittäminen.
DirectoryFeatures |Käytössä/poissa käytöstä kansion ominaisuudet.
DirSyncConfiguration |Sisältää kaikki tietyn nykyisen vuokraajan DirSync-määritys.
Näyttönimi |Objektin näyttönimi
IsMnc |Totuusarvo Merkitse arvoksi "true" kansainvälisissä yrityksen-ominaisuus on käytössä yrityksen iff.
ObjectSettings | Kokoelma objektin laajuus koskevat asetukset.
PartnerCommerceUrl |Commerce kumppanisivustossa URL-osoite.
PartnerHelpUrl |Ohjeen kumppanisivustossa URL-osoite.
PartnerSupportEmail | URL-osoite näkyy kumppanin tuki sähköpostiin.
PartnerSupportTelephone |URL-osoite näkyy kumppanin tuen puhelinnumero.
PartnerSupportUrl |Tuen kumppanisivustossa URL-osoite.
StrongAuthenticationDetails |StrongAuthentication liittyvät tiedot.
StrongAuthenticationPolicy |Vahva todennus käytäntö yritykselle.
TechnicalNotificationMail |Tekniset yrityksen esiintyviä ilmoittamaan sähköpostiosoite.
TelephoneNumber |Puhelinnumerot, joihin ITU suositus E.123 mukaisia.
TenantType |Palvelutili tyyppi. Jos tämä arvo ei ole määritetty, vuokraajan on yritys. Muussa tapauksessa mahdolliset arvot ovat: MicrosoftSupport (0), SyndicatePartner (1), (2) BreadthPartner BreadthPartnerDelegatedAdmin (3) ResellerPartnerDelegatedAdmin (4) ValueAddedResellerPartnerDelegatedAdmin (5).
VerifiedDomain |Määritä DNS-toimialuenimien sidottu yrityksen.

## <a name="update-domain-attributes"></a>"Päivitä toimialueen" määritteet
Määrite                           | Kuvaus
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
Ominaisuudet    |Mahdollisesti-bittinen toimialueen ominaisuuksia kuvaavan merkinnät.
Oletusarvo |Ilmaisee, onko toimialue oletusarvoa. esimerkiksi oletusarvon UserPrincipalName liitteen kun järjestelmänvalvoja luo uuden käyttäjän MOAC.
Alkuperäinen |Ilmaisee, onko toimialueen yritykselle, alustavaksi toimialueeksi kuin OCP kohdistetaan. Alustavaksi toimialueeksi on yksilöllinen aliraportti toimialue Microsoft Online-toimialueen; e.g.contoso3.microsoftonline.com.
LiveType    |Vastaava Windows Live nimitilan, jos sellainen on tyyppi.
Nimi    | Päätepisteen tunnus.
PasswordNotificationWindowDays  |Sinulta kysytään salasana vanhenee käyttäjän päivien määrän.
PasswordValidityPeriodDays  | Salasana on hyvin, ennen kuin se on vaihdettava päivien määrän.

Valvontatiedot on pakollinen ohjausobjekti useita yhteensopivuuden asetukset. Asiakkaille, jotka täyttävät yhteensopivuuden niiden asetusten Azure Active Directory valvonta raportin avulla on suositeltavaa asiakkaan lähettää kopion ohjeaiheen asiakkaan viedyn valvontaraportin kerrotaan raportin tiedot kopiota. Jos tarkastaja haluat ymmärtää, joka Azure tällä hetkellä täyttää yhteensopivuuden asetukset, ohjata tarkastaja Microsoft Azure Trust Centeriin [Yhteensopivuus-sivu](https://azure.microsoft.com/support/trust-center/compliance/) .

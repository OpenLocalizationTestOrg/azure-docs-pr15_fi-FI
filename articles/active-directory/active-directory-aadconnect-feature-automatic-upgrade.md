<properties
   pageTitle="Azure AD Connect: Automaattisen päivityksen | Microsoft Azure"
   description="Tässä ohjeaiheessa kerrotaan sisäistä automaattisen päivityksen ominaisuutta Azure AD Connect synkronointiin."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/24/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-automatic-upgrade"></a>Azure AD Connect: Automaattinen päivitys
Tämä ominaisuus otettiin muodosta 1.1.105.0 (julkaistu helmikuussa 2016) kanssa.

## <a name="overview"></a>Yleiskatsaus
Varmistetaan, että Azure AD Connect-asennus on aina ajan tasalla on todella helppoa **Automaattinen päivitys** -ominaisuudella. Tämä ominaisuus on käytössä oletusarvoisesti pika-asennukset ja DirSync päivityksistä. Kun uusi versio julkaistaan, asennuksen päivitetään automaattisesti.

Automaattinen päivitys on käytössä oletusarvoisesti seuraavat asiat:

- Voit ilmaista asetukset asennus- ja DirSync-päivitykset.
- Käytä SQL Express LocalDB, joka on Expressin asetukset aina käyttää. SQL Expressin kanssa DirSync myös käyttämällä LocalDB.
- AD-tili on Expressin asetukset ja DirSync luomia MSOL_ oletustilin.
- On pienempi kuin 100 000 objektit metaverse.

Automaattisen päivityksen nykyisen tilan voidaan tarkastella PowerShell cmdlet-komennon `Get-ADSyncAutoUpgrade`. Se on seuraavista tiloista:

Tila | Kommentti
---- | ----
Käytössä | Automaattinen päivitys on käytössä.
Keskeytetty | Määrittää järjestelmän vain. Järjestelmä ei ole enää oikeutettu saamaan Automaattiset päivitykset.
Ei käytössä | Automaattinen päivitys on poistettu käytöstä.

Voit vaihtaa **käytössä** ja **poissa käytöstä** ja `Set-ADSyncAutoUpgrade`. Vain järjestelmä kannattaa asettaa tila **Suspended**.

Automaattinen päivitys käyttää Azure AD yhteyden kunto päivityksen infrastruktuurin. Automaattisen päivityksen toimimaan Varmista, että olet avannut URL-osoitteet välityspalvelimen määrittäminen **Azure AD yhteyden** suorittimessa [Office 365: n URL-osoitteet](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)ja IP-osoitealueet.

Jos **Synkronoinnin palvelun hallinta** Käyttöliittymän suoritetaan palvelimessa, päivitys keskeytetään, kunnes Käyttöliittymän on suljettu.

## <a name="troubleshooting"></a>Vianmääritys
Jos Yhdistä-asennuksesi ei päivityksen itse oikein, toimi näiden ohjeiden avulla selvää, mikä saattaa olla väärä.

Ensin automaattisen päivityksen voidaan yrittää uusi versio julkaistaan ensimmäisenä ei olisi olettaa. Ei tahallisten poistaa satunnaisuuden ennen päivitystä yritetään niin ei hälytyslaitteet, jos asennuksesi ei ole päivitetty heti.

Jos järjestelmässä on väärä, valitse ensin Suorita `Get-ADSyncAutoUpgrade` varmistamiseksi automaattinen päivitys on käytössä.

Valitse Varmista, että olet avannut palomuurin tai välityspalvelimen vaadittavat URL-osoitteet. Automaattinen päivitys käyttää Azure AD yhteyden kunto kuvatulla tavalla [Yleiskatsaus](#overview). Jos käytät välityspalvelinta, varmista, että kunto on määritetty käyttämään [välityspalvelinta](active-directory-aadconnect-health-agent-install.md#configure-azure-ad-connect-health-agents-to-use-http-proxy). Testaa myös Azure AD [kunto-yhteys](active-directory-aadconnect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service) .

Vahvistettu Azure AD yhteyden on aika eventlogs tarkastelevat. Käynnistä Tapahtumienvalvonta ja Etsi **sovellus** tapahtumaloki. Lisää tapahtumaloki-suodattimen **Azure AD yhteyden päivittäminen** lähteen ja tapahtuman tunnus alueen **300 399**.  
![Tapahtumaloki suodattimen automaattinen päivitys](./media/active-directory-aadconnect-feature-automatic-upgrade/eventlogfilter.png)  

Nyt näet eventlogs liittyvän automaattisen päivityksen tila.  
![Tapahtumaloki suodattimen automaattinen päivitys](./media/active-directory-aadconnect-feature-automatic-upgrade/eventlogresult.png)  

Tuloskoodi on yleiskuvaus valtion etuliite.

Tulos koodin etuliite | Kuvaus
--- | ---
Onnistui | Asennus on tehty.
UpgradeAborted | Tilapäinen pysäyttää päivittämisen. Sitä yritetään uudelleen, ja se, että se onnistuu myöhemmin.
UpgradeNotSupported | Järjestelmässä on määritys, joka estää järjestelmän päivitetään automaattisesti. Se yritetään tila muuttuu, mutta se, että järjestelmä on päivitettävä manuaalisesti.

Ohessa on luettelo yleisimmistä löydät viestit. Se ei Näytä kaikki, mutta tuloksena viestin pitäisi olla selkeä, mikä ongelma on.

Tulosta viesti | Kuvaus
--- | ---
**UpgradeAborted** |
UpgradeAbortedCouldNotSetUpgradeMarker | Ei voi kirjoittaa rekisterin.
UpgradeAbortedInsufficientDatabasePermissions | Sisäiset Järjestelmänvalvojat-ryhmän ei ole tietokannan käyttöoikeudet. Päivitä manuaalisesti Azure AD Connect voit ratkaista ongelman uusimman version.
UpgradeAbortedInsufficientDiskSpace | Ei ole riittävästi levytilaa tukemaan päivityksen.
UpgradeAbortedSecurityGroupsNotPresent | Voi etsiä ei ja ratkaista synkronoinnin ohjelma käyttää kaikkien käyttöoikeusryhmien.
UpgradeAbortedServiceCanNotBeStarted | NT-palvelua **Microsoft Azure AD-synkronointi** ei voitu käynnistää.
UpgradeAbortedServiceCanNotBeStopped | NT-palvelua **Microsoft Azure AD-synkronoinnin** lopettaminen epäonnistui.
UpgradeAbortedServiceIsNotRunning | NT-palvelua **Microsoft Azure AD-synkronointi** ei ole käynnissä.
UpgradeAbortedSyncCycleDisabled | [Ajoituksen](active-directory-aadconnectsync-feature-scheduler.md) SyncCycle-asetus on poistettu käytöstä.
UpgradeAbortedSyncExeInUse | [Synkronoinnin palvelun hallinta Käyttöliittymän](active-directory-aadconnectsync-service-manager-ui.md) on avoinna palvelimessa.
UpgradeAbortedSyncOrConfigurationInProgress | Ohjattu asennus on käynnissä tai synkronoinnista on ajoitettu päivitysyrityksen ulkopuolella.
**UpgradeNotSupported** |
UpgradeNotSupportedCustomizedSyncRules | Olet lisännyt omia mukautettuja sääntöjä työnkulkuun.
UpgradeNotSupportedDeviceWritebackEnabled | [Laitteen takaisinkirjoituksen](active-directory-aadconnect-feature-device-writeback.md) -ominaisuus on otettu käyttöön.
UpgradeNotSupportedGroupWritebackEnabled | [Ryhmän takaisinkirjoituksen](active-directory-aadconnect-feature-preview.md#group-writeback) -ominaisuus on otettu käyttöön.
UpgradeNotSupportedInvalidPersistedState | Asennus ei ole DirSync-päivityksen tai Express-asetukset.
UpgradeNotSupportedMetaverseSizeExceeeded | Sinulla metaverse yli 100 000 objekteja.
UpgradeNotSupportedMultiForestSetup | Yrität muodostaa yhteyden useita metsää. Pika-asennus yhdistää vain yhden metsää.
UpgradeNotSupportedNonLocalDbInstall | Et käytä SQL Server Express LocalDB tietokannan.
UpgradeNotSupportedNonMsolAccount | [AD Connector-tilin](active-directory-aadconnect-accounts-permissions.md#active-directory-account) ei ole enää MSOL_ oletustilin.
UpgradeNotSupportedStagingModeEnabled | Palvelin on määritetty väliaikaisen [tilassa](active-directory-aadconnectsync-operations.md#staging-mode).
UpgradeNotSupportedUserWritebackEnabled | [Käyttäjän takaisinkirjoituksen](active-directory-aadconnect-feature-preview.md#user-writeback) -ominaisuus on otettu käyttöön.

## <a name="next-steps"></a>Seuraavat vaiheet
Lisätietoja [ympäristön integroinnissa paikallisen-käyttäjätietojen Azure Active Directory-hakemistosta](active-directory-aadconnect.md).

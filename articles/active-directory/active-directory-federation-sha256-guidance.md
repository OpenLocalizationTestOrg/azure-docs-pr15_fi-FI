<properties
    pageTitle="Muuta hash Allekirjoitusalgoritmi for Office 365: n vastaaminen osapuolen luota | Microsoft Azure"
    description="Tällä sivulla on ohjeita SHA algoritmin federation luota Office 365: n muuttaminen"
    keywords="SHA1, SHA256, O365-federaatio, aadconnect, adfs, ad fs, muuta sha federation luota, käyttäisit osapuolen luota"
    services="active-directory"
    documentationCenter=""
    authors="anandyadavmsft"
    manager="samueld"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/01/2016"
    ms.author="anandy"/>

# <a name="change-signature-hash-algorithm-for-office-365-replying-party-trust"></a>Office 365 vastattaessa osapuolen luota hash Allekirjoitusalgoritmi muuttaminen

## <a name="overview"></a>Yleiskatsaus

Azure Active Directory Federation Services (AD FS) kirjautuu Microsoft Azure Active Directory varmistaakseen, että hän ei voi olla peukaloitu sen tunnukset. Allekirjoitus voi perustua SHA1 tai SHA256. Azure Active Directory tukee nyt tunnusten SHA256 algoritmin on allekirjoitettu ja suosittelemme SHA256 tunnuksen allekirjoitus-algoritmin määrittäminen suurimpien suojaustasoa. Tässä artikkelissa määritetään turvallisempi SHA256 tunnuksen allekirjoitus-algoritmin tason tarvittavat vaiheet.

## <a name="change-the-token-signing-algorithm"></a>Tunnuksen allekirjoitus-algoritmin muuttaminen

Kun olet määrittänyt Allekirjoitusalgoritmi mainitun alla kaksi prosesseja, AD FS kirjautuu paikkamerkkejä käyttäisit osapuolen luottamussuhde SHA256 Office 365: n. Sinun ei tarvitse tehdä ylimääräisiä määritysten muutokset ja muutos ei ole vaikutusta, voitko käyttää Office 365: ssä tai Azure AD-sovelluksia.

### <a name="ad-fs-management-console"></a>AD FS hallintakonsoli

1. Avaa AD FS-hallintakonsoli ensisijainen ADFS-palvelimeen.
2. Laajenna AD FS-solmu ja valitse **Käyttäisit osapuolen luottaa**.
3. Office 365: n ja Azure varmenteen käyttäjän osapuolen luottamuksen hiiren kakkospainikkeella ja valitse **Ominaisuudet**.
4. Valitse **Lisäasetukset** -välilehti ja valitse suojatun hash-algoritmin SHA256.
5. Valitse **OK**.

![SHA256 Allekirjoitusalgoritmi--MMC](./media/active-directory-aadconnectfed-sha256guidance/mmc.png)

### <a name="ad-fs-powershell-cmdlets"></a>AD FS PowerShellin cmdlet-komennot

1. Minkä tahansa ADFS-palvelimeen Avaa PowerShell-kohdassa järjestelmänvalvojan oikeudet.
2. Määrittää suojatun hash-algoritmin **Määrittäminen AdfsRelyingPartyTrust** cmdlet-komennolla.

 <code>Set-AdfsRelyingPartyTrust -TargetName 'Microsoft Office 365 Identity Platform' -SignatureAlgorithm 'http://www.w3.org/2001/04/xmldsig-more#rsa-sha256'</code>

## <a name="also-read"></a>Lue myös

* [Korjaa Office 365: n luottamussuhde Azure AD Connect](./active-directory-aadconnect-federation-management.md#repairing-the-trust)

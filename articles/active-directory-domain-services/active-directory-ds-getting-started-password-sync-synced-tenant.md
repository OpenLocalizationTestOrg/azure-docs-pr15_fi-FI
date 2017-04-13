<properties
    pageTitle="Azure AD-toimialueen palveluista: Ota käyttöön salasanan synkronointi | Microsoft Azure"
    description="Azure Active Directory-toimialueen palveluiden käytön aloittaminen"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/20/2016"
    ms.author="maheshu"/>

# <a name="enable-password-synchronization-to-azure-ad-domain-services"></a>Azure AD-toimialueen palveluihin salasanojen synkronoinnin ottaminen käyttöön
Edeltävät tehtävät otit Azure AD-toimialueen palveluista Azure AD-vuokraajan. Seuraava tehtävä on Azure AD-toimialueen palveluista salasanojen synkronoinnin käyttöönotto. Kun tunnistetiedon synkronointi on määritetty, käyttäjät voivat kirjautua sisään käyttämällä yrityksen käyttöoikeutensa hallitun toimialueeseen.

Vaiheet ovat eri perusteella, onko organisaatiossasi on tarkoitus käyttää vain pilvipalveluita Azure AD vuokraaja tai on määritetty synkronoimaan käyttämällä Azure AD Connect paikallista hakemistoa.

<br>

> [AZURE.SELECTOR]
- [Vain pilvipalveluita Azure AD vuokraaja](active-directory-ds-getting-started-password-sync.md)
- [Synkronoitu Azure AD-vuokraajan](active-directory-ds-getting-started-password-sync-synced-tenant.md)

<br>


## <a name="task-5-enable-password-synchronization-to-aad-domain-services-for-a-synced-azure-ad-tenant"></a>Vaihe 5: Ota käyttöön salasanojen synkronoinnin AAD-toimialueen palveluihin synkronoidussa Azure AD-vuokraajan
A synkronoitu Azure AD-vuokraajan on määritetty synkronoimaan käyttämällä Azure AD Connect organisaation paikallisesta hakemistosta. Azure AD Connect ei synkronoidu NTLM ja Kerberos tunnistetietojen hajautusarvot Azure AD oletusarvoisesti. Jos haluat käyttää Azure AD-toimialueen palveluista, haluat määrittäminen Azure AD Connect synkronoimaan tunnistetiedon hajautusarvot NTLM ja Kerberos-todennus vaaditaan. Seuraavat vaiheet synkronointia tarvittavat tunnistetiedot hajautusarvot Azure AD-asiakasympäristöön.


### <a name="install-or-update-azure-ad-connect"></a>Asenna tai päivitä Azure AD Connect
Azure AD Connect uusimmat suositellut julkaisemisen asentaminen toimialueen liitettyjen tietokoneeseen. Jos sinulla on aiemmin luodun Azure AD Connect asennuksen esiintymän, haluat päivittää sen Azure AD Connect uusimman version. Tunnetut ongelmat/virheet, jotka voivat olla jo korjattu välttämiseksi varmistaakseen, että aina Azure AD Connect uusimman version.

**[Lataa Azure AD-muodosta](http://www.microsoft.com/download/details.aspx?id=47594)**

Suositeltava versio: **1.1.281.0** - julkaistu 7 syyskuussa 2016.

  > [AZURE.WARNING] Sinun on asennettava Azure AD Connect (NTLM ja Kerberos-todennus vaaditaan) vanha salasana-tunnistetiedot käyttöön Synkronoi Azure AD-vuokraajan uusimmat suositellut julkaisemisen. Tämä toiminto ei ole käytettävissä aiempien versioiden Azure AD Connect tai vanhoja DirSync-työkalun avulla.

Azure AD Connect asennusohjeet ovat käytettävissä seuraavassa artikkelissa - [Azure AD Connect käytön aloittaminen](../active-directory/active-directory-aadconnect.md)


### <a name="enable-synchronization-of-ntlm-and-kerberos-credential-hashes-to-azure-ad"></a>NTLM ja Kerberos tunnistetiedon hajautusarvot, Azure AD synkronoinnin käyttöönotto
Suorittaa seuraavaa PowerShell-komentosarjaa kunkin AD metsää Pakota koko salasanojen synkronoinnin, ja ota käyttöön kaikki paikallisen käyttäjien tunnistetiedon hajautusarvot Azure AD-vuokraajan synkronoiminen. Tämä komentosarja mahdollistaa Azure AD-vuokraajan synkronoitavaksi NTLM/Kerberos-todennus vaaditaan tunnistetiedon hajautusarvot.

```
$adConnector = "<CASE SENSITIVE AD CONNECTOR NAME>"  
$azureadConnector = "<CASE SENSITIVE AZURE AD CONNECTOR NAME>"  
Import-Module adsync  
$c = Get-ADSyncConnector -Name $adConnector  
$p = New-Object Microsoft.IdentityManagement.PowerShell.ObjectModel.ConfigurationParameter "Microsoft.Synchronize.ForceFullPasswordSync", String, ConnectorGlobal, $null, $null, $null
$p.Value = 1  
$c.GlobalParameters.Remove($p.Name)  
$c.GlobalParameters.Add($p)  
$c = Add-ADSyncConnector -Connector $c  
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $azureadConnector -Enable $false   
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $azureadConnector -Enable $true  
```

Hakemiston koon mukaan (käyttäjien lukumäärä ryhmittelee jne), tunnistetiedon hajautusarvot Azure AD, synkronointi kestää jonkin aikaa. Salasanat on käytettävissä Azure AD-toimialueen palveluista hallitun toimialueen, pian, kun tunnistetiedon hajautusarvot on synkronoitu Azure AD.


<br>

## <a name="related-content"></a>Aiheeseen liittyvää sisältöä

- [Käyttöön salasanojen synkronoinnin AAD-toimialueen palveluihin vain pilvipalveluita Azure Active directory](active-directory-ds-getting-started-password-sync.md)

- [Hallitut Azure AD-toimialueen palvelut-toimialueen hallintaan](active-directory-ds-admin-guide-administer-domain.md)

- [Liity Windows-virtual machine Azure AD-toimialueen palveluista hallitun toimialueeseen](active-directory-ds-admin-guide-join-windows-vm.md)

- [Liittyminen punainen on Enterprise Linux virtual koneen Azure AD-toimialueen palveluista hallitun toimialueeseen](active-directory-ds-admin-guide-join-rhel-linux-vm.md)

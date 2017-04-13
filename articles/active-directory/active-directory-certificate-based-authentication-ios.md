<properties 
    pageTitle="Aloita pohjainen todennus iOS | Microsoft Azure" 
    description="Opettele määrittämään pohjainen todennus ratkaisuissa iOS-laitteille" 
    services="active-directory" 
    authors="MarkusVi"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="10/21/2016" 
    ms.author="markvi" />



# <a name="get-started-with-certificate-based-authentication-on-ios---public-preview"></a>Pohjainen todennus iOS - Public Preview-version käytön aloittaminen

> [AZURE.SELECTOR]
- [iOS](active-directory-certificate-based-authentication-ios.md)
- [Android-laitteeseen](active-directory-certificate-based-authentication-android.md)


Tässä ohjeaiheessa esitellään määrittämisestä ja hyödyntää Office 365 Enterprise-, Business-ja Education-alihallinnat mukaan Varmenteen todentaminen (CBA) käyttäjille iOS-laitteessa. 

CBA mahdollistaa todennus Azure Active Directoryssa Asiakasvarmenne Android- ja iOS-laitteen kanssa yhdistettäessä Exchange online-tilin lisääminen: 

- Office mobile sovelluksista, kuten Microsoft Outlook- tai Microsoft Word   
- Exchange ActiveSync (EAS)-asiakkaat 

Tämän ominaisuuden määrittämisestä ei tarvitse kirjoittaa käyttäjänimen ja salasanan yhdistelmä tiettyjä sähköposti- ja Microsoft Office-sovellusten mobiililaitteessa. 
 

## <a name="supported-scenarios-and-requirements"></a>Tuetut tilanteet ja vaatimukset  



### <a name="general-requirements"></a>Yleiset vaatimukset 


Tämän artikkelin kaikki skenaarioille tarvitaan seuraavat toimet:  

- Varmenteen authority(s) asiakkaan sertifikaattien myöntämiseen pääsyä.  

- Varmenteiden authority(s) on oltava määritettynä Azure Active Directoryn. Löydät yksityiskohtaiset ohjeet Viimeistele määritys [Käytön aloittaminen](#getting-started) -osassa.  

- Varmenteiden päämyöntäjä- ja minkä tahansa Keskitason varmenteiden myöntäjistä on oltava määritettynä Azure Active Directoryn.  

- Jokainen varmenteen myöntäjä on oltava kumottujen varmenteiden luettelo (luettelon), jota voidaan käyttää vastakkaisten URL-osoite Internet kautta.  

- Asiakkaan varmenne on annettava asiakkaan todennusta varten.  


- Vain Exchange ActiveSync-asiakkaat asiakkaan varmenne on oltava käyttäjän reititettävä sähköpostiosoite Exchange online-lyhennys nimi tai aihe vaihtoehtoinen nimi-kentän RFC822 nimi-arvoa. Azure Active Directory yhdistää välityspalvelimen osoite hakemistossa määritteen RFC822-arvo.  



### <a name="office-mobile-applications-support"></a>Office mobile-sovellukset tukevat 

| Sovellukset                      | Tuki      |
| ---                       | ---          |
| Word tai Excel tai PowerPoint | ![Tarkista][1]  |
| OneNote                   | ![Tarkista][1]  |
| OneDrive                  | ![Tarkista][1]  |
| Outlookin                   | ![Tarkista][1]  |
| Yammer-                    | ![Tarkista][1]  |
| Skype for Business        | Tulossa pian  |


### <a name="requirements"></a>Vaatimukset  

Laitteen käyttöjärjestelmän version on oltava iOS 9 ja yllä 

Liittoutumispalvelimen on oltava määritettynä.  

Azure todentaja tarvitaan iOS Office-sovellukset.  

Azure Active Directory kumota Asiakasvarmenne ADFS-tunnuksen on oltava seuraavat vaateet:  

  - `http://schemas.microsoft.com/ws/2008/06/identity/claims/<serialnumber>`  
(Sertifikaattia järjestysluvun) 

  - `http://schemas.microsoft.com/2012/12/certificatecontext/field/<issuer>`  
(Asiakas-varmenteen myöntäjä merkkijono) 

Azure Active Directory Lisää nämä saatavat, Päivitä-tunnuksen, jos ne ovat käytettävissä ADFS-tunnuksen (tai SAML-tunnuksen). Kun Päivitä-tunnuksen on tarkistettava, voit tarkistaa peruuttaminen käyttävät näitä tietoja. 

Paras käytäntö sinun on päivitettävä ADFS-virhesivujen kanssa seuraavasti:

- Azure-käyttöoikeudet asentamisessa iOS vaatimus

- Käyttäjän varmenteen hankkiminen ohjeita. 

Lisätietoja on artikkelissa [AD FS-kirjautuminen sivujen mukauttaminen](https://technet.microsoft.com/library/dn280950.aspx).  



### <a name="exchange-activesync-clients-support"></a>Exchange ActiveSync-ohjelmien tuki 


IOS 9 tai uudempi versio on tuettu alkuperäisen iOS-sähköpostiohjelmassa. Voit selvittää, jos tätä ominaisuutta tuetaan, ota kaikki muut Exchange ActiveSync sovellukset sovelluksen kehittäjän.  



## <a name="getting-started"></a>Käytön aloittaminen 


Aluksi haluat määrittää varmenteiden myöntäjistä Azure Active Directoryn. Kunkin varmenteen myöntäjä, lataa seuraavasti: 

- Julkisen osan varmenteen *.cer* -muodossa 

- Vastakkaisten URL-osoitteet, joissa varmenteen kumottujen varmenteiden luettelot (kumottujen varmenteiden luettelot löytyvät) sijaitsevat Internetissä
 

Alla on varmenteen myöntäjä rakenteen: 

    class TrustedCAsForPasswordlessAuth 
    { 
       CertificateAuthorityInformation[] certificateAuthorities;    
    } 

    class CertificateAuthorityInformation 

    { 
        CertAuthorityType authorityType; 
        X509Certificate trustedCertificate; 
        string crlDistributionPoint; 
        string deltaCrlDistributionPoint; 
        string trustedIssuer; 
        string trustedIssuerSKI; 
    }                

    enum CertAuthorityType 
    { 
        RootAuthority = 0, 
        IntermediateAuthority = 1 
    } 


Voit ladata tiedot, voit käyttää kautta Windows PowerShellin Azure AD-moduulin.  
Seuraavassa on esimerkkejä lisäämällä, poistamalla tai muokkaamalla varmenteen myöntäjä. 



### <a name="configuring-your-azure-ad-tenant-for-certificate-based-authentication"></a>Azure AD-vuokraajan sertifikaatin määrittäminen perustuva todentaminen 

1. Käynnistä Windows PowerShell järjestelmänvalvojan oikeuksilla. 

2. Azure AD-moduulin asentaminen. Sinun on asennettava versio [1.1.143.0](http://www.powershellgallery.com/packages/AzureADPreview/1.1.143.0) tai uudempi versio.  

        Install-Module -Name AzureADPreview –RequiredVersion 1.1.143.0 

3. Muodostaa yhteyttä kohdevuokraajassa: 

        Connect-AzureAD 

### <a name="adding-a-new-certificate-authority"></a>Uuden varmenteen myöntäjä lisääminen

1. Varmenteen myöntäjä ominaisuuksia ja lisää se Azure Active Directory: 

        $cert=Get-Content -Encoding byte "[LOCATION OF THE CER FILE]" 
        $new_ca=New-Object -TypeName Microsoft.Open.AzureAD.Model.CertificateAuthorityInformation 
        $new_ca.AuthorityType=0 
        $new_ca.TrustedCertificate=$cert 
        New-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $new_ca 

5. Hae varmenteiden myöntäjistä: 

        Get-AzureADTrustedCertificateAuthority 


### <a name="retrieving-the-list-certificate-authorities"></a>Luettelon varmenteiden myöntäjistä hakeminen

Hae varmenteiden myöntäjistä, tallennettujen Azure Active Directory-vuokraajan: 

        Get-AzureADTrustedCertificateAuthority 


### <a name="removing-a-certificate-authority"></a>Varmenteen myöntäjä poistaminen

1.  Hae varmenteiden myöntäjistä: 

        $c=Get-AzureADTrustedCertificateAuthority 


2. Poista varmenteen myöntäjä varmennetta: 

        Remove-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[2] 


### <a name="modfiying-a-certificate-authority"></a>Modfiying varmenteen myöntäjä 

1.  Hae varmenteiden myöntäjistä: 

        $c=Get-AzureADTrustedCertificateAuthority 


2. Muokkaa ominaisuuksia-varmenteen myöntäjä: 

        $c[0].AuthorityType=1 

3. Määritä **varmenteen myöntäjä**: 

        Set-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[0] 




## <a name="testing-office-mobile-applications"></a>Office mobile-sovellusten testaaminen  

Voit testata todennus mobiililaitteiden Office-sovelluksessa seuraavasti: 

1.  Asenna Office mobile-sovelluksen (esimerkiksi Onedriveen) testin laitteesi App Storesta.

2.  Varmista, että käyttäjävarmenne on valmisteltu testi laitteeseen. 

3.  Käynnistä sovellus. 

4.  Kirjoita käyttäjänimi ja valitse käyttäjävarmenne, jota haluat käyttää. 

Sinun pitäisi onnistuneesti kirjauduttava. 





## <a name="testing-exchange-activesync-client-applications"></a>Exchange ActiveSync-asiakassovellusten testaaminen

Exchange ActiveSync-pohjainen todennus kautta käyttöön EAS-profiili, jossa asiakkaan varmenne on oltava käytettävissä-sovellukseen. EAS-profiilin on oltava seuraavat tiedot:

- Käyttäjävarmenne, jota käytetään todennus 

- EAS-päätepisteen on oltava outlook.office365.com (katso tätä ominaisuutta tuetaan tällä hetkellä vain Exchange Onlinen usean vuokraajan-ympäristössä)

EAS-profiili on määritetty ja laitteeseen kuten Intuneen tai lisäämällä varmenteen EAS-profiili laitteeseen manuaalisesti MDM käyttö kautta.  

### <a name="testing-eas-client-applications-on-ios"></a>EAS-asiakassovellusten iOS testaaminen 

Voit testata todennus alkuperäisen mail-sovellukseen iOS 9 tai yläpuolella seuraavasti: 

1.  Määritä EAS-profiili, jotka täyttävät edellä. 

2.  Profiilin asentaminen iOS-laitteessa (joko käyttämällä MDM, kuten Intuneen tai Apple määritystoiminnon-sovellus)

3.  Kun profiili on asennettu, Avaa alkuperäinen Mail-sovellus ja tarkista sähköpostin synkronoidaan



## <a name="revocation"></a>Peruuttaminen

Kumota Asiakasvarmenne Azure Active Directory hakee kumottujen varmenteiden luettelo (luettelon) varmenteen myöntäjä tietojen osana ladattu URL-osoitteista ja tallentaa sen välimuistiin. Viimeisin julkaista aikaleima (**Voimaantulopäivä** -ominaisuus)-luettelon avulla varmistetaan luettelon on edelleen voimassa. Luettelon viitataan säännöllisesti Kumotaanko käyttöoikeus sertifikaatteja, joita ovat osa luettelon.

Jos Lisää Pikahaun kumoaminen vaaditaan (esimerkiksi käyttäjä menettää laite), voit mitätöidään käyttäjän todennus-tunnuksen. Voit poistaa todennus-tunnuksen, Määritä Windows PowerShellin tietyn käyttäjän **StsRefreshTokenValidFrom** -kenttä. Sinun on päivitettävä kullekin käyttäjälle haluat käytön estäminen **StsRefreshTokenValidFrom** -kenttään.
 
Voit varmistaa, että peruuttaminen jatkuu, sinun on määritettävä luettelon **Voimaantulopäivä** päivämääräksi, kun arvo määrittää **StsRefreshTokenValidFrom** ja varmistaa, sertifikaatti poistettavaan on luettelon.
 
Seuraavissa vaiheissa kuvataan prosessi, jossa päivittäminen ja poistavat luvan tunnuksen määrittämällä **StsRefreshTokenValidFrom** -kenttään. 

1. Yhdistä järjestelmänvalvojan tunnistetietojen MSOL-palveluun: 

        $msolcred = get-credential 
        connect-msolservice -credential $msolcred 

1.  Hae käyttäjän StsRefreshTokensValidFrom nykyisen arvon: 

        $user = Get-MsolUser -UserPrincipalName test@yourdomain.com` 
        $user.StsRefreshTokensValidFrom 


1.  Määrittää käyttäjän yhtä suuri kuin nykyinen aikamerkinnän uuden StsRefreshTokensValidFrom-arvon seuraavasti: 

        Set-MsolUser -UserPrincipalName test@yourdomain.com -StsRefreshTokensValidFrom ("03/05/2016")


Voit määrittää päivämäärän on oltava tulevaisuudessa. Jos päivämäärä ei ole myöhemmin, **StsRefreshTokensValidFrom** -ominaisuutta ei ole määritetty. Jos päivämäärä on myöhemmin, **StsRefreshTokensValidFrom** on määritetty nykyisen kellonajan (ei ole merkitty Set-MsolUser-komennon päivämäärä). 



<!--Image references-->
[1]: ./media/active-directory-certificate-based-authentication-ios/ic195031.png
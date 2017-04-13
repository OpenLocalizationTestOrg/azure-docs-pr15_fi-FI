<properties 
    pageTitle="Aloittaminen pohjainen todennus Android-laitteeseen | Microsoft Azure" 
    description="Opettele määrittämään pohjainen todennus-ratkaisujen kanssa Android-laitteille" 
    services="active-directory" 
    authors="markusvi"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="10/10/2016" 
    ms.author="markvi" />



# <a name="get-started-with-certificate-based-authentication-on-android---public-preview"></a>Pohjainen todennus Android - Public Preview-version käytön aloittaminen  

> [AZURE.SELECTOR]
- [iOS](active-directory-certificate-based-authentication-ios.md)
- [Android-laitteeseen](active-directory-certificate-based-authentication-android.md)


Tämän artikkelin avulla voit määrittää ja hyödyntää Office 365 Enterprise-, Business-ja Education-pohjainen todennus (CBA) käyttäjille Android-laitteessa, alihallinnat, jotka. 

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

- Kunkin varmenteen myöntäjä on oltava kumottujen varmenteiden luettelo (luettelon), jota voidaan käyttää vastakkaisten URL-osoite Internet kautta.  

- Asiakkaan varmenne on annettava asiakkaan todennusta varten.  


- Vain Exchange ActiveSync-asiakkaat asiakkaan varmenne on oltava käyttäjän reititettävä sähköpostiosoite Exchange online-lyhennys nimi tai aihe vaihtoehtoinen nimi-kentän RFC822 nimi-arvoa. Azure Active Directory yhdistää välityspalvelimen osoite hakemistossa määritteen RFC822-arvo.  



### <a name="office-mobile-applications-support"></a>Office mobile-sovellukset tukevat 

| Sovellukset                      | Tuki      |
| ---                       | ---          |
| Word tai Excel tai PowerPoint | ![Tarkista][1]  |
| OneNote                   | Tulossa pian  |
| OneDrive                  | ![Tarkista][1]  |
| Outlookin                   | ![Tarkista][1]  |
| Yammer-                    | ![Tarkista][1]  |
| Skype for Business        | ![Tarkista][1]  |


### <a name="requirements"></a>Vaatimukset  

Laitteen käyttöjärjestelmän version on oltava Android 5.0 (tikkukaramelli) ja suurempia. 

Liittoutumispalvelimen on oltava määritettynä.  


Azure Active Directory kumota Asiakasvarmenne ADFS-tunnuksen on oltava seuraavat saatavat:  

  - `http://schemas.microsoft.com/ws/2008/06/identity/claims/<serialnumber>`  
(Sertifikaattia järjestysluvun) 

  - `http://schemas.microsoft.com/2012/12/certificatecontext/field/<issuer>`  
(Asiakas-varmenteen myöntäjä merkkijono) 

Azure Active Directory Lisää nämä saatavat, Päivitä-tunnuksen, jos ne ovat käytettävissä ADFS-tunnuksen (tai SAML-tunnuksen). Kun Päivitä-tunnuksen on tarkistettava, voit tarkistaa peruuttaminen käyttävät näitä tietoja. 

Paras käytäntö on päivitettävä ADFS-virhesivujen, kuinka saat käyttäjävarmenne. 

Lisätietoja on artikkelissa [AD FS-kirjautuminen sivujen mukauttaminen](https://technet.microsoft.com/library/dn280950.aspx).  



### <a name="exchange-activesync-clients-support"></a>Exchange ActiveSync-ohjelmien tuki 


Tietyt Exchange ActiveSync-sovellukset Android 5.0 (tikkukaramelli) tai uudempi tuetaan. Voit selvittää, jos sähköpostisovelluksen tukevat tätä ominaisuutta, ota yhteyttä sovelluksen kehittäjän. 



## <a name="getting-started"></a>Käytön aloittaminen 


Aluksi haluat määrittää varmenteiden myöntäjistä Azure Active Directoryn. Jokainen varmenteen myöntäjä, lataa seuraavasti: 

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



### <a name="configuring-your-azure-ad-tenant-for-certificate-based-authentication"></a>Azure AD-vuokraajan varmenteen määrittäminen perustuva todentaminen 

1. Käynnistä Windows PowerShell järjestelmänvalvojan oikeuksilla. 

2. Azure AD-moduulin asentaminen. Sinun on asennettava versio [1.1.143.0](http://www.powershellgallery.com/packages/AzureADPreview/1.1.143.0) tai uudempi versio.  

        Install-Module -Name AzureADPreview –RequiredVersion 1.1.143.0 

3. Muodostaa yhteyttä kohdevuokraajassa: 

        Connect-AzureAD 

### <a name="adding-a-new-certificate-authority"></a>Uuden varmenteen myöntäjä lisääminen

1. Varmenteen myöntäjä ominaisuuksia ja Azure Active Directory lisäämistä: 

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

1.  Asenna Office mobile-sovelluksen (esimerkiksi Onedriveen) testin laitteesi Google Play-kaupasta.

2.  Varmista, että käyttäjävarmenne on valmisteltu testi laitteeseen. 

3.  Käynnistä sovellus. 

4.  Kirjoita käyttäjänimi ja valitse käyttäjävarmenne, jota haluat käyttää. 

Sinun pitäisi onnistuneesti kirjauduttava. 





## <a name="testing-exchange-activesync-client-applications"></a>Exchange ActiveSync-asiakassovellusten testaaminen

Exchange ActiveSync-pohjainen todennus kautta käyttöön sisältävä sertifikaattia EAS-profiilin on oltava käytettävissä-sovellukseen. EAS-profiilin on oltava seuraavat tiedot:

- Käyttäjävarmenne, jota käytetään todennus 

- EAS-päätepisteen on oltava outlook.office365.com (katso tätä ominaisuutta tuetaan tällä hetkellä vain Exchange Onlinen usean vuokraajan-ympäristössä)

EAS-profiili on määritetty ja laitteeseen kuten Intuneen tai sijoittamalla varmenteen EAS-profiili laitteeseen manuaalisesti MDM käyttö kautta.  


### <a name="testing-eas-client-applications-on-android"></a>EAS-asiakassovellusten Android testaaminen 

Voit esikatsella sovelluksessa Android 5.0 (tikkukaramelli) tai uudempi todennus-suorittaa seuraavia ohjeita:  

1. Määrittää sovellus, joka täyttää vaatimukset edellä EAS-profiiliin.  


2. Kun profiili on määritetty oikein, Avaa sovellus ja tarkista, että sähköpostin synkronointi. 




## <a name="revocation"></a>Peruuttaminen

Kumota Asiakasvarmenne Azure Active Directory hakee kumottujen varmenteiden luettelo (luettelon) varmenteen myöntäjä tietojen osana ladattu URL-osoitteista ja tallentaa sen välimuistiin. Viimeisin julkaista aikaleima (**Voimaantulopäivä** -ominaisuus)-luettelon avulla varmistetaan luettelon on edelleen voimassa. Luettelon viitataan säännöllisesti Kumotaanko käyttöoikeus sertifikaatteja, joita ovat osa luettelon.

Jos Lisää Pikahaun kumoaminen vaaditaan (esimerkiksi käyttäjä menettää laite), voit mitätöidään käyttäjän todennus-tunnuksen. Voit poistaa todennus-tunnuksen, Määritä Windows PowerShellin tietyn käyttäjän **StsRefreshTokenValidFrom** -kenttä. Sinun on päivitettävä kullekin käyttäjälle haluat käytön estäminen **StsRefreshTokenValidFrom** -kenttään.
 
Voit varmistaa, että peruuttaminen toistuu, sinun on määritettävä luettelon **Voimaantulopäivä** päivämäärän jälkeen arvo asettamassa **StsRefreshTokenValidFrom** ja varmistaa, sertifikaatti poistettavaan on luettelon.
 
Seuraavissa vaiheissa kuvataan prosessi, jossa päivittäminen ja poistavat luvan tunnuksen määrittämällä **StsRefreshTokenValidFrom** -kenttään. 

1. Yhdistä järjestelmänvalvojan tunnistetietojen MSOL-palveluun: 

        $msolcred = get-credential 
        connect-msolservice -credential $msolcred 

1.  Hae käyttäjän StsRefreshTokensValidFrom nykyisen arvon: 

        $user = Get-MsolUser -UserPrincipalName test@yourdomain.com` 
        $user.StsRefreshTokensValidFrom 


1.  Määrittää käyttäjän yhtä suuri kuin nykyinen aikamerkinnän uuden StsRefreshTokensValidFrom-arvon seuraavasti: 

        Set-MsolUser -UserPrincipalName test@yourdomain.com -StsRefreshTokensValidFrom ("03/05/2016")


Voit määrittää päivämäärän on oltava tulevaisuudessa. Jos päivämäärä ei ole tulevaisuudessa, **StsRefreshTokensValidFrom** -ominaisuutta ei ole määritetty. Jos päivämäärä on myöhemmin, **StsRefreshTokensValidFrom** on määritetty nykyisen kellonajan (ei ole merkitty Set-MsolUser-komennon päivämäärä). 


<!--Image references-->
[1]: ./media/active-directory-certificate-based-authentication-android/ic195031.png
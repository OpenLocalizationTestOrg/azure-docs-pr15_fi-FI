<properties
    pageTitle="Azure AD Connect useita toimialueita"
    description="Tässä asiakirjassa kerrotaan ja määrittämisestä useita ylimmän tason toimialueita O365 ja Azure AD."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="multiple-domain-support-for-federating-with-azure-ad"></a>Usean toimialueen tuen sisällytetyistä Azure AD kanssa
Seuraavat asiakirjat on ohjeita siitä, miten voit käyttää useita ylätason verkkotunnuksia ja alitoimialueisiin, kun Office 365: ssä tai Azure AD-toimialueiden sisällytetyistä.

## <a name="multiple-top-level-domain-support"></a>Useiden ylätason toimialue-tuki
Sisällytetyistä useita, Azure AD ylätason toimialueiden edellyttää joitakin lisämäärityksiä, joita ei tarvita, kun sisällytetyistä yhden ylimmän tason toimialue.

Kun toimialue on äänipuheluita Azure AD-toimialueella Azure-tietokannassa on määritetty useita ominaisuuksia.  Tärkeää yhdeksi on IssuerUri.  Tämä on URI, jota käytetään Azure AD tunnistamiseen, tunnuksen on liitetty toimialueeseen.  URI ei tarvitse ratkaista jokin muu sen on oltava kelvollinen URI.  Oletusarvon mukaan Azure AD asettaa tämän arvon federation palvelun tunnus tiloissa oman AD FS määritys.

>[AZURE.NOTE]Liitetty viestintä service-tunniste on URI, joka yksilöi federation palvelu.  Liitetty viestintä-palvelu on esiintymän AD FS, joka toimii suojauksen suojaustunnuksen palveluna. 

PowerShell-komennon avulla voit siirtyä IssuerUri `Get-MsolDomainFederationSettings - DomainName <your domain>`.

![Hae MsolDomainFederationSettings](./media/active-directory-multiple-domains/MsolDomainFederationSettings.png)

Ongelma syntyy silloin, kun haluat lisätä useita ylimmän tason toimialue.  Esimerkiksi oletetaan, että sinulla on asennuksen federation välillä Azure AD ja paikallisen ympäristön.  Tämän asiakirjan käytän bmcontoso.com.  Nyt olen lisännyt toisen, ylimmän tason toimialueen bmfabrikam.com.

![Toimialueet](./media/active-directory-multiple-domains/domains.png)

Olemme yrittää muuntaa Microsoftin bmfabrikam.com toimialue on liitetty, emme tulee virhesanoma.  Tämä on Azure AD on rajoitus, joka ei salli IssuerUri-ominaisuuden arvoksi on useampi kuin yksi toimialue on sama arvo.  
  

![Liitetty viestintä-virhe](./media/active-directory-multiple-domains/error.png)

### <a name="supportmultipledomain-parameter"></a>SupportMultipleDomain parametri

Voit kiertää tämän annettava Lisää eri IssuerUri, joiden avulla voidaan toteuttaa `-SupportMultipleDomain` parametria.  Tämä parametri käytetään suorittamalla seuraavat cmdlet-komennot:
    
- `New-MsolFederatedDomain`
- `Convert-MsolDomaintoFederated`
- `Update-MsolFederatedDomain`

Tämä parametri on Azure AD IssuerUri määrittäminen niin, että se perustuu muokattavan toimialueen nimi.  Tämä on yksilöllinen kansioiden välillä Azure AD.  Parametrin avulla voit suorittaa loppuun PowerShell-komentoa.

![Liitetty viestintä-virhe](./media/active-directory-multiple-domains/convert.png)

Katsoo meidän bmfabrikam.com verkkotunnuksen asetukset näkyviin seuraavasti:

![Liitetty viestintä-virhe](./media/active-directory-multiple-domains/settings.png)

Huomaa, että `-SupportMultipleDomain` eivät muutu päätepisteet joka määritetään edelleen osoittamaan federation adfs.bmcontoso.com-palveluun.

Toinen vaihtoehto, joka `-SupportMultipleDomain` onko on, että se varmistaa, että AD FS-järjestelmän sisältää tunnusten annettu Azure AD oikea myöntäjä-arvo. Se tekee ottamalla käyttäjät täydellisen Käyttäjätunnuksen toimialueosa ja tämän määrittämistä IssuerUri eli https://{upn jälkiliite toimialueen} / adfs/palvelut ja Salli. 

Näin Azure AD todennuksen aikana tai Office 365-käyttäjän tunnuksen IssuerUri elementti on käytössä Etsi toimialueen Azure AD.  Jos vastinetta ei löydy todennus epäonnistuu. 

Esimerkiksi jos käyttäjän UPN on bsimon@bmcontoso.com, IssuerUri osan tunnuksen AD FS-ongelmat asetetaan http://bmcontoso.com/adfs/services/trust. Tämä vastaavat Azure AD-määritys ja todennus onnistuu.

Seuraavassa on mukautetun ryhmän sääntö, joka sisältää tässä logiikan:

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type =   "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)", "http://${domain}/adfs/services/trust/"));


>[AZURE.IMPORTANT]Voi käyttää - SupportMultipleDomain valitsin, kun yrität lisätä uusia tai muuntaa jo lisätyt toimialueet, sinun täytyy on liitetty luottamuksen tukea ensimmäisen kerran.  


## <a name="how-to-update-the-trust-between-ad-fs-and-azure-ad"></a>AD FS ja Azure AD välinen luottamussuhde päivittämisestä
Jos ei asennuksen liitetyt luota AD FS ja että Azure AD-esiintymässä välillä, joudut ehkä luotava tämä luota.  Tämä johtuu siitä, kun se on alun perin määritetty ilman `-SupportMultipleDomain` parametrin IssuerUri asetuksena on oletusarvo.  Voit alla olevassa näyttökuvassa näkyy IssuerUri on määritetty https://adfs.bmcontoso.com/adfs/services/trust.

Näin nyt, jos olet lisännyt uuden toimialueen Azure AD-portaalissa vaan yrittää muuntaa sen `Convert-MsolDomaintoFederated -DomainName <your domain>`, että saat seuraavan virheilmoituksen.

![Liitetty viestintä-virhe](./media/active-directory-multiple-domains/trust1.png)

Jos yrität lisätä `-SupportMultipleDomain` valitsin on tulee seuraava virhe:

![Liitetty viestintä-virhe](./media/active-directory-multiple-domains/trust2.png)

Vain yritetään suorittaa `Update-MsolFederatedDomain -DomainName <your domain> -SupportMultipleDomain` alkuperäisen toimialueella myös aiheuttaa virheen.

![Liitetty viestintä-virhe](./media/active-directory-multiple-domains/trust3.png)

Noudattamalla seuraavia ohjeita Lisää mukautettu ylimmän tason toimialue.  Jos olet jo lisännyt toimialueen ja ole käyttänyt `-SupportMultipleDomain` parametrin alkavat poistaminen ja alkuperäisen toimialueen päivitetään.  Jos et ole lisännyt ylätason toimialue vielä voit aloittaa lisätä käyttämällä PowerShell, Azure AD Connect toimialueen vaiheisiin.

Seuraavien vaiheiden avulla voit poistaa Microsoft Online luota ja Päivitä alkuperäinen toimialueesi.

2.  Avaa AD FS federation-palvelimeen **AD FS Management.** 
2.  Laajenna vasemmassa reunassa **Luota suhteet** ja **Käyttäisit osapuolen luottaa**
3.  Oikeanpuoleiseen poistaminen **Microsoft Office 365: n Identity-ympäristössä** .
![Poista Microsoft Online-tilassa](./media/active-directory-multiple-domains/trust4.png)
1.  Suorita seuraava tietokoneessa, jossa on asennettu [Active Directory moduulin Windows PowerShellin Azure](https://msdn.microsoft.com/library/azure/jj151815.aspx) : `$cred=Get-Credential`.  
2.  Kirjoita käyttäjänimi ja salasana, yleisellä järjestelmänvalvojalla on sisällytetyistä kanssa Azure AD-toimialueen
2.  Kirjoita PowerShell`Connect-MsolService -Credential $cred`
4.  Kirjoita PowerShell `Update-MSOLFederatedDomain -DomainName <Federated Domain Name> -SupportMultipleDomain`.  Tämä on alkuperäinen toimialueen.  Käyttämällä yllä toimialueet olisi:`Update-MsolFederatedDomain -DomainName bmcontoso.com -SupportMultipleDomain`


Seuraavien vaiheiden avulla voit lisätä uuden ylimmän tason toimialueen PowerShellin avulla

1.  Suorita seuraava tietokoneessa, jossa on asennettu [Active Directory moduulin Windows PowerShellin Azure](https://msdn.microsoft.com/library/azure/jj151815.aspx) : `$cred=Get-Credential`.  
2.  Kirjoita käyttäjänimi ja salasana, yleisellä järjestelmänvalvojalla on sisällytetyistä kanssa Azure AD-toimialueen
2.  Kirjoita PowerShell`Connect-MsolService -Credential $cred`
3.  Kirjoita PowerShell`New-MsolFederatedDomain –SupportMultipleDomain –DomainName`

Seuraavien vaiheiden avulla voit lisätä käyttämällä Azure AD Connect uusi ylimmän tason toimialue.

1.  Käynnistä Azure AD Connect työpöydältä tai Käynnistä-valikko
2.  Valitse "Lisää lisätoimialuetta Azure AD" ![Lisää ylimääräinen Azure AD-toimialueesta](./media/active-directory-multiple-domains/add1.png)
3.  Kirjoita Azure AD ja Active Directory-tunnistetiedot
4.  Valitse toinen toimialue haluat määrittää yhdistämiseen.
![Lisää ylimääräinen Azure AD-toimialueesta](./media/active-directory-multiple-domains/add2.png)
5.  Valitse Asenna


### <a name="verify-the-new-top-level-domain"></a>Vahvista uusi ylätason toimialue
PowerShell-komennolla `Get-MsolDomainFederationSettings - DomainName <your domain>`voit tarkastella päivitetyt IssuerUri.  Seuraavassa näyttökuvassa näkyy federation asetukset on päivitetty, tutustu alkuperäisen toimialueen http://bmcontoso.com/adfs/services/trust

![Hae MsolDomainFederationSettings](./media/active-directory-multiple-domains/MsolDomainFederationSettings.png)

Ja tutustu uuden toimialueen IssuerUri on määritetty https://bmfabrikam.com/adfs/services/trust

![Hae MsolDomainFederationSettings](./media/active-directory-multiple-domains/settings2.png)


##<a name="support-for-sub-domains"></a>Alitoimialueisiin tuki
Kun lisäät aliraportti toimialueen vuoksi käsitellä tavalla Azure AD-toimialueet, se Peri ylemmän tason asetukset.  Tämä tarkoittaa, että IssuerUri on vastattava vanhemmat.

Avulla niin Oletetaan esimerkiksi, että voin on bmcontoso.com ja lisää sitten corp.bmcontoso.com.  Tämä tarkoittaa, että IssuerUri corp.bmcontoso.com-käyttäjän on oltava **http://bmcontoso.com/adfs/services/trust.**  Kuitenkin toteutettu edellä Azure AD-standardin säännön luo tunnuksen, jonka myöntäjä kuin **http://corp.bmcontoso.com/adfs/services/trust.** jotka eivät vastaa toimialueen tarvittava arvo ja todennus epäonnistuu.

### <a name="how-to-enable-support-for-sub-domains"></a>Alitoimialueisiin tuen ottaminen käyttöön
Jotta voit ratkaista ongelman AD FS varmenteen käyttäjän osapuolen luottamus Microsoft Online on päivitettävä.  Tällöin sinun on määritettävä mukautetun ryhmän säännön niin, että se Sahalaita käytöstä kaikki alitoimialueisiin-käyttäjän täydellisen Käyttäjätunnuksen jälkiliitteen luodessaan mukautetun myöntäjä-arvon. 

Seuraavat varaa toimi seuraavasti:

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^((.*)([.|@]))?(?<domain>[^.]*[.].*)$", "http://${domain}/adfs/services/trust/"));

Seuraavien vaiheiden avulla voit lisätä mukautetun ryhmän tukemaan alitoimialueisiin.

1.  Avaa AD FS hallinta
2.  Microsoft Online RP luota hiiren kakkospainikkeella ja valitse Muokkaa ryhmän säännöt
3.  Valitse Varaa kolmas sääntö ja korvaa ![Muokkaa varaaminen](./media/active-directory-multiple-domains/sub1.png)
4.  Korvaa nykyinen vaatimus:
    
        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)","http://${domain}/adfs/services/trust/"));
        
    kanssa
    
        `c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^((.*)([.|@]))?(?<domain>[^.]*[.].*)$", "http://${domain}/adfs/services/trust/"));`
    
![Korvaa varaaminen](./media/active-directory-multiple-domains/sub2.png)
5.  Valitse Ok.  Valitse Käytä.  Valitse Ok.  Sulje AD FS hallinta.


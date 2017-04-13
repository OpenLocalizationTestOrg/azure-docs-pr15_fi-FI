<properties
    pageTitle="Varmenteen uusiminen ohjeita Office 365: ssä ja Azure Active Directory-käyttäjille | Microsoft Azure"
    description="Tässä artikkelissa kerrotaan Office 365-käyttäjille, kuinka voit ratkaista ongelmat, joka ilmoittaa ne uudistamalla varmenteen sähköpostit."
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


# <a name="renew-federation-certificates-for-office-365-and-azure-active-directory"></a>Uusia Office 365: ssä ja Azure Active Directory federation varmenteet

##<a name="overview"></a>Yleiskatsaus

Onnistuneiden federation Azure Active Directory (Azure AD) ja Active Directory Federation Services (AD FS) välillä käyttämä AD FS suojauksen tunnusten Azure AD voit kirjautua varmenteet vastattava mikä on määritetty Azure AD. Mikä tahansa ristiriita voi aiheuttaa katkennut luota. Azure AD varmistat, että nämä tiedot säilytetään synkronoinnissa asentaessasi AD FS ja Web-sovelluksen välityspalvelimen (ekstranet käytön).

Tässä artikkelissa on lisätietoja hallita oman tunnuksen kirjautuminen varmenteet ja pitämään ne kanssa Azure AD-synkronoinnin seuraavissa tapauksissa:

* Web-sovelluksen välityspalvelin ei käyttöönoton ja näin ollen federation metatiedot ei ole käytettävissä ekstranet.
* Käytät AD FS oletusarvon määrittäminen ei tunnuksen kirjautuminen varmenteet.
* Käytössäsi on kolmannen osapuolen palveluntarjoaja.

## <a name="default-configuration-of-ad-fs-for-token-signing-certificates"></a>Kirjautuminen varmenteet tunnuksen ADFS oletusarvon määrittäminen

Suojaustunnuksen allekirjoituksen ja salauksen varmenteet tunnuksen ovat yleensä itse allekirjoitettua varmenteet ja ovat käteviä, vuosi. Oletusarvon mukaan AD FS on nimeltään **AutoCertificateRollover**automaattisen uusinnan-prosessin. Jos käytät AD FS 2.0 tai uudempi versio, Office 365: ssä ja Azure AD automaattisesti Päivitä varmennetta, ennen kuin se päättyy.

### <a name="renewal-notification-from-the-office-365-portal-or-an-email"></a>Office 365-portaalin tai sähköpostiviestin ilmoitus uusiminen

>[AZURE.NOTE] Jos olet saanut sähköpostitse tai portaalin ilmoitus kysytään, uusi varmennetta Office-kohdassa Tarkista sinun tarvitse tehdä mitään [hallinta muutoksista tunnuksen kirjautuminen varmenteet](#managecerts) . Microsoft on tietoinen mahdollista ongelman, joka voi aiheuttaa ilmoitukset varmenteiden uusiminen on lähetetty, vaikka mitään toimia ei tarvita.

Azure AD yrittää Valvo federation metatiedot ja Päivitä kirjautuminen varmenteet metatiedot merkitty tunnuksen. Kirjautuminen varmenteet-tunnuksen 30 päivää Azure AD tarkistaa, jos uusi varmenteet ovat käytettävissä kyselyt federation metatiedot.

* Jos se federation metatiedot äänestyksen järjestäminen ja Hae uusi varmenteet onnistuneesti, ei ole sähköposti-ilmoituksen tai varoitus Office 365-portaalissa on myöntänyt käyttäjälle.
* Jos sitä ei voi hakea kirjautuminen varmenteet uuden tunnuksen, joko koska federation metatiedot ei ole tavoitettavissa tai Automaattinen sertifikaatin palauttaminen ei ole käytössä, Azure AD ongelmat sähköposti-ilmoituksen ja varoitus Office 365-portaalissa.

![Office 365-portaalin ilmoitus](./media/active-directory-aadconnect-o365-certs/notification.png)

>[AZURE.IMPORTANT] Jos käytät AD FS liiketoiminnan jatkuvuuden varmistamiseksi, varmista, että palvelinten on seuraavat päivitykset, niin, että todennus epäonnistuu lisätietoja tunnetuista ongelmista ei tehdä. Tämä lieventää AD FS välityspalvelimen palvelimen tunnettuja uusimisen ja tulevien uusimisen kausien:
>
>Server 2012 R2 - [Windows Server toukokuu 2014 rollup](http://support.microsoft.com/kb/2955164)
>
>Server 2008 R2 ja 2012 - [välityspalvelimen kautta-todennus epäonnistuu, Windows Server 2012: ssa tai Windows 2008 R2 SP1](http://support.microsoft.com/kb/3094446)

## Tarkista, jos varmenteet on päivitettävä<a name="managecerts"></a>

### <a name="step-1-check-the-autocertificaterollover-state"></a>Vaihe 1: Tarkista AutoCertificateRollover osavaltio

ADFS-palvelimeen Avaa PowerShell. Tarkista, että AutoCertificateRollover-arvo on TOSI.

    Get-Adfsproperties

![AutoCertificateRollover](./media/active-directory-aadconnect-o365-certs/autocertrollover.png)

[AZURE.NOTE] Jos käytät AD FS 2.0, suorita ensin Lisää Pssnapin Microsoft.Adfs.Powershell.

### <a name="step-2-confirm-that-ad-fs-and-azure-ad-are-in-sync"></a>Vaihe 2: Varmista, että AD FS ja Azure AD on synkronoitu

ADFS-palvelimeen Avaa PowerShellin Azure AD-kehote ja Azure AD yhdistäminen.

>[AZURE.NOTE] Voit ladata PowerShellin Azure AD [Tässä](https://technet.microsoft.com/library/jj151815.aspx).

    Connect-MsolService

Tarkista AD FS ja luota määritetyn toimialueen ominaisuudet Azure AD määritetty varmenteet.

    Get-MsolFederationProperty -DomainName <domain.name> | FL Source, TokenSigningCertificate

![Hae MsolFederationProperty](./media/active-directory-aadconnect-o365-certs/certsync.png)

Jos sekä tulostus-thumbprints vastaa, varmenteet on synkronoitu Azure AD kanssa.

### <a name="step-3-check-if-your-certificate-is-about-to-expire"></a>Vaihe 3: Tarkista, onko varmennetta vanhenemassa

Hae MsolFederationProperty tai Hae AdfsCertificate tulosteeseen Tarkista päivämäärä kohdassa "Sen jälkeen." Jos päivämäärä on pienempi kuin 30 päivää tieltä-toiminto tekee saapuville.

| AutoCertificateRollover | Varmenteiden Azure AD kanssa | Liitetty viestintä on kaikkien käytettävissä | Kelpoisuus | Toiminto |
|:-----------------------:|:-----------------------:|:-----------------------:|:-----------------------:|:-----------------------:|
| Kyllä | Kyllä | Kyllä | - | Mitään toimia ei tarvita. Katso [Uusi tunnuksen allekirjoitusvarmenne automaattisesti](#autorenew). |
| Kyllä | Ei  | - | Pienempi kuin 15 päivää | Uusia heti. Katso [Uusi tunnuksen allekirjoitusvarmenne manuaalisesti](#manualrenew). |
| Ei | - | - | Alle 30 päivää | Uusia heti. Katso [Uusi tunnuksen allekirjoitusvarmenne manuaalisesti](#manualrenew). |

\[-] Ei ole merkitystä.

## Tunnuksen allekirjoitusvarmenne automaattinen uusiminen (suositus)<a name="autorenew"></a>

Sinun ei tarvitse suorittaa manuaalisia toimia, jos seuraavat ehdot täyttyvät:
- Web Application välityspalvelin, joka ottaa access federation metatiedoista-ekstranet on otettu käyttöön.
- Käytät AD FS-oletusarvon määrittäminen (AutoCertificateRollover on otettu käyttöön).

Tarkista seuraavat vahvistamaan, että varmenteen päivity automaattisesti.

**1. AD FS-ominaisuuden AutoCertificateRollover on määritettävä Tosi.** Tämä ilmaisee, että AD FS luo automaattisesti uuden tunnuksen allekirjoituksen ja suojaustunnuksen salauksen varmenteet, vanhan ennen nykyisten päättyy.

**2. AD FS-liittoutumispalvelinten metatietojen on kaikkien käytettävissä.** Tarkista, että federation metatietojen on kaikkien käytettävissä siirtymällä seuraava URL-osoite (luettelosta yritysverkon) julkinen internet-tietokoneesta:


https:// (your_FS_name) /federationmetadata/2007-06/federationmetadata.xml

Jos `(your_FS_name) `on korvattu federation palvelun isäntänimi organisaatiossa käytetään, kuten fs.contoso.com.  Jos et pysty vahvistamaan molempia näistä asetuksista onnistuu, sinulla ei ole Successfully.  

Esimerkki: https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml

## Uusia tunnuksen allekirjoitusvarmenne manuaalisesti<a name="manualrenew"></a>

Voit halutessasi uusi allekirjoitus manuaalisesti varmenteet tunnuksen. Esimerkiksi seuraavissa tilanteissa saattaa toimia paremmin manuaalinen uudistaminen:
* Suojaustunnuksen kirjautuminen varmenteet ovat itse allekirjoitettua varmenteet. Yleisin syy on, että organisaatio hallitsee AD FS varmenteet rekisteröity organisaation varmenteen myöntäjältä.
* Verkon suojausta ei salli federation metatiedot on yleisesti saatavilla.

Näissä tilanteissa aina, kun päivität tunnuksen kirjautuminen todistuksista, sinun on myös päivitettävä Office 365-toimialueen käyttämällä PowerShell-komentoa, Päivitä MsolFederatedDomain.

### <a name="step-1-ensure-that-ad-fs-has-new-token-signing-certificates"></a>Vaihe 1: Varmista, että AD FS on uuden tunnuksen kirjautuminen varmenteet

**Muun kuin oletusarvoisen määritys**

Jos käytössäsi on muun kuin oletusarvoisen määrittäminen AD FS-Liittoutumispalvelut (jossa **AutoCertificateRollover** on määritetty **Epätosi**), käytössäsi on todennäköisesti mukautetun varmenteet (ei itse allekirjoitetun). Lisätietoja uusiminen kirjautuminen varmenteet AD FS-tunnuksen Katso [ohjeet ei käytössä AD FS itse allekirjoitetun varmenteet](https://msdn.microsoft.com/library/azure/JJ933264.aspx#BKMK_NotADFSCert).

**Liitetty viestintä metatietojen ei ole yleiseen käyttöön**

Toisaalta, jos **AutoCertificateRollover** arvo on **Tosi**, mutta federation metatietojen ei ole julkinen, varmista ensin, että uuden tunnuksen allekirjoitetun varmenteet on luotu AD FS. Vahvista, sinulla on uuden tunnuksen kirjautuminen varmenteet noudattamalla seuraavia ohjeita:

1. Varmista, että olet kirjautuneena ensisijainen ADFS-palvelimeen.
2. Tarkista AD FS nykyisen allekirjoitetun varmenteet avaaminen PowerShell-komentoikkunassa ja suorittamalla seuraavan komennon:

    PS C:\>Get-ADFSCertificate – CertificateType tunnuksen-kirjautuminen

    >[AZURE.NOTE] Jos käytät AD FS 2.0, suorita ensin Lisää Pssnapin Microsoft.Adfs.Powershell.

3. Tarkista luettelossa sertifikaatteja komennon tulokset. Jos AD FS on luonut uutta varmennetta, pitäisi näkyä tulosteesta todistuksista: yksi, jossa **IsPrimary** -arvo on **Tosi** ja **NotAfter** päivämäärä on 5 päivän kuluessa ja yksi, jossa **IsPrimary** on **Epätosi** ja **NotAfter** on noin vuosi tulevaisuudessa.

4. Jos näkyvissä on vain yksi sertifikaatti, **NotAfter** päivämäärä on viisi päivää haluat luoda uutta varmennetta.

5. Luo uusi varmenne, suorita seuraava komento PowerShell komentokehotteeseen: `PS C:\>Update-ADFSCertificate –CertificateType token-signing`.

6. Tarkista päivityksen uudelleen suorittamalla seuraavan komennon: PS C:\>Get-ADFSCertificate – CertificateType tunnuksen-kirjautuminen

Kaksi varmenteet luettelossa pitäisi näkyä nyt, joista toinen on noin vuoden tulevaisuudessa **NotAfter** päivämäärän ja, jossa **IsPrimary** -arvo on **Epätosi**.

### <a name="step-2-update-the-new-token-signing-certificates-for-the-office-365-trust"></a>Vaihe 2: Päivitä uuden tunnuksen kirjautua Office 365: n luota varmenteet

Päivitä Office 365: ssä uuden tunnuksen kirjautuminen varmenteet, jota käytetään luota, seuraavasti.

1.  Avaa Windows PowerShellin Microsoft Azure Active Directory-moduulin.
2.  Suorita $cred = Get-Credential. Kun tämä cmdlet-komento pyytää tunnistetietoja, kirjoittamalla cloud palvelun järjestelmänvalvojan tilin tunnistetiedot.
3.  Suorita yhdistäminen MsolService – tunnistetietojen $cred. Cmdlet muodostaa pilvipalveluun. Luominen kontekstissa, jota voit muodostaa yhteyden pilvipalvelussa tarvitaan ennen kuin suoritat jokin työkalu asentaa muita cmdlet-komennot.
4.  Jos käytät näitä komentoja, tietokoneeseen, jossa ei ole AD FS ensisijainen liittoutumispalvelimen, suorita määrittäminen MSOLAdfscontext-tietokoneen <AD FS primary server>, jossa <AD FS primary server> on ensisijainen AD FS palvelin sisäisen toimialuenimen. Tämä cmdlet-komento luo kontekstissa, jota voit muodostaa yhteyden AD FS.
5.  Suorita päivitys MSOLFederatedDomain – toimialuenimi <domain>. Tämä cmdlet-komento päivittää AD FS asetuksia cloud-palveluun ja määrittää välinen luottamussuhde.


>[AZURE.NOTE] Jos tarvitset tukemaan useita ylimmän tason toimialueet, kuten contoso.com ja fabrikam.com, sinun on käytettävä **SupportMultipleDomain** Vaihda kaikki cmdlet-komennot. Lisätietoja on artikkelissa [useita ylhäältä tason toimialueita tuki](active-directory-aadconnect-multiple-domains.md).

## Azure AD-luota korjaaminen Azure AD Connect avulla<a name="connectrenew"></a>

Jos olet määrittänyt AD FS-klusterin ja Azure AD-valvonta Azure AD Connect avulla, voit tunnistaa, jos haluat ryhtyä, että tunnuksen kirjautuminen varmenteet Azure AD Connect. Jos haluat uusia varmenteet, voit tehdä Azure AD Connect.

Lisätietoja on artikkelissa [korjaaminen luota](./active-directory-aadconnect-federation-management.md#repairing-the-trust).

<properties
    pageTitle="Active Directory Federation Services hallinta ja mukauttaminen ja Azure AD Connect | Microsoft Azure"
    description="AD FS hallinta Azure AD Connect ja AD FS-kirjautuminen käyttäjäkokemuksen Azure AD Connect ja PowerShell mukauttaminen."
    keywords="AD FS, ADFS-AD FS hallinta-AAD yhteyden, Yhdistä-kirjautuminen, AD FS mukauttaminen, luota, O365-federation varmenteen käyttäjän osapuolen korjaaminen"
    services="active-directory"
    documentationCenter=""
    authors="anandyadavmsft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/01/2016"
    ms.author="anandy"/>

# <a name="active-directory-federation-services-management-and-customization-with-azure-ad-connect"></a>Active Directory-liittoutumispalvelut hallinta ja mukauttaminen Azure AD Connect kanssa

Tämän artikkelin tiedot tehtäviin liittyvät Active Directory Federation Services (AD FS), jotka voivat tehdä käyttämällä Microsoft Azure Active Directory-yhteyden sekä muita yleisiä AD FS tehtäviä, joita voidaan vaatia valmis määrityksen AD FS-klusterin.

| Aihe | Se peittää |
|:------|:-------------|
|**AD FS hallinta**|
|[Luota korjaaminen](#repairthetrust)| Liitetty viestintä luota Office 365: n korjaaminen |
|[Lisää AD FS-palvelin](#addadfsserver) | Lisätietoja AD FS-palvelussa AD FS-klusterin laajentaminen|
|[AD FS web application välityspalvelimen palvelimen lisääminen](#addwapserver) | Lisää WAP-palvelussa AD FS-klusterin laajentaminen|
|[Lisää liitetyssä toimialueessa](#addfeddomain)| Liitetyssä toimialueessa lisääminen|
| **AD FS mukauttaminen**|
|[Lisää mukautetun yrityksen logo tai kuva](#customlogo)| Mukauttaminen AD FS kirjautumissivu yrityksen logon ja kuva |
|[Lisää kuvaus-kirjautuminen](#addsignindescription) | Lisää kirjautumissivu kuvauksen |
|[AD FS varaa sääntöjen muokkaaminen](#modclaims) | AD FS väitteitä eri federation tilanteissa muokkaaminen |

## <a name="ad-fs-management"></a>AD FS hallinta

Azure AD Connect on eri AD FS, jotka voivat tehdä Azure AD Connect ohjatun käyttäminen käyttäjien toimia liittyviä tehtäviä. Kun olet tehnyt asennuksen Azure AD Connect suorittamalla ohjattu, voit suorittaa ohjattu uudelleen lisätoiminnot.

### Luota korjaaminen<a name=repairthetrust></a>

Azure AD Connect voit etsiä AD FS ja Azure Active Directory-luota nykyinen kunto ja suorittamaan korjaamaan luota tarvittavat toiminnot. Voit korjata Azure AD seuraavasti ja AD FS valvonta.

1. Valitse **Korjaa AAD ja ADFS luota** tehtävät-luettelosta.
![Korjaa AAD ja ADFS luota](media\active-directory-aadconnect-federation-management\RepairADTrust1.PNG)

2. **Yhteyden muodostaminen Azure AD** -sivulla yleisen järjestelmänvalvojan tunnistetietoja Azure AD, ja valitse **Seuraava**.
![Azure AD yhdistäminen](media\active-directory-aadconnect-federation-management\RepairADTrust2.PNG)

3. Kirjoita **etäkäyttöpalvelimen tunnistetiedot** -sivulla toimialueen järjestelmänvalvojan tunnistetiedot.
![Etäkäyttö tunnistetiedot](media\active-directory-aadconnect-federation-management\RepairADTrust3.PNG)

    Kun valitset **Seuraava**, Azure AD Connect Tarkista varmenteen määrittäminen ja Näytä mahdollisten ongelmien.

    ![Tilan varmenteet](media\active-directory-aadconnect-federation-management\RepairADTrust4.PNG)

    **Valmis määrittäminen** -sivulla näkyy luettelo toiminnoista, jotka suoritetaan korjaamaan luota.

    ![Voit määrittää asetukset](media\active-directory-aadconnect-federation-management\RepairADTrust5.PNG)

4. Valitse korjaa luota **asentaminen** .

>[AZURE.NOTE] Azure AD Connect voi vain korjaaminen tai toimiminen sertifikaatit, jotka on allekirjoitettu. Azure AD Connect ei voi korjata kolmannen osapuolen varmenteet.

### Lisää AD FS-palvelin<a name=addadfsserver></a>

> [AZURE.NOTE] Azure AD Connect edellyttää PFX sertifikaattitiedosto Lisää ADFS-palvelimeen. Voit suorittaa tämän toiminnon vuoksi vain, jos olet määrittänyt AD FS-klusterin Azure AD Connect avulla.

1. Valitse **Ota käyttöön muita Liittoutumispalvelimen** ja valitse **Seuraava**.
![Lisää liittoutumispalvelimen](media\active-directory-aadconnect-federation-management\AddNewADFSServer1.PNG)

2. **Yhteyden muodostaminen Azure AD** -sivulla Kirjoita yleisen järjestelmänvalvojan tunnistetiedot Azure AD ja valitse **Seuraava**.
![Azure AD yhdistäminen](media\active-directory-aadconnect-federation-management\AddNewADFSServer2.PNG)

3. Anna toimialueen järjestelmänvalvojan tunnistetiedot.
![Toimialueen järjestelmänvalvojan tunnistetiedot](media\active-directory-aadconnect-federation-management\AddNewADFSServer3.PNG)

4. Azure AD Connect pyytää PFX-tiedosto, jonka ilmoitit määritettäessä uuden AD FS-klusterin Azure AD Connect ja salasanaa. Valitse **Anna salasana** salasana antamaan PFX-tiedosto.
![Sertifikaatin salasana](media\active-directory-aadconnect-federation-management\AddNewADFSServer4.PNG)

    ![Määritä SSL-varmenne](media\active-directory-aadconnect-federation-management\AddNewADFSServer5.PNG)

5. Anna palvelimen nimi tai IP-osoitteen lisääminen AD FS-klusterin **AD FS-palvelimet** -sivulla.
![AD FS-palvelimet](media\active-directory-aadconnect-federation-management\AddNewADFSServer6.PNG)

6. Valitse **Seuraava** ja siirry lopulliseen **määrittäminen** -sivulla. Kun Azure AD Connect on lisännyt palvelimet AD FS-klusteriin, sinulle annetaan asetus, jos haluat tarkistaa yhteydet.
![Voit määrittää asetukset](media\active-directory-aadconnect-federation-management\AddNewADFSServer7.PNG)

    ![Asennus valmis](media\active-directory-aadconnect-federation-management\AddNewADFSServer8.PNG)

### AD FS web application välityspalvelimen palvelimen lisääminen<a name=addwapserver></a>

> [AZURE.NOTE] Azure AD Connect edellyttää PFX varmenne-tiedoston lisääminen web-sovelluksen välityspalvelin. Tämän vuoksi osaat tämä toiminto vain, jos olet määrittänyt AD FS-klusterin Azure AD Connect avulla.

1. Valitse **Ota käyttöön Web-sovelluksen välityspalvelimen** käytettävissä tehtäväluettelosta.
![Web-sovelluksen välityspalvelimen käyttöönotto](media\active-directory-aadconnect-federation-management\WapServer1.PNG)

2. Azure yleisen järjestelmänvalvojan tunnistetiedoilla.
![Azure AD yhdistäminen](media\active-directory-aadconnect-federation-management\wapserver2.PNG)

3. Valitse **Määritä SSL-varmenne** -sivulla avulla PFX-tiedosto, jonka ilmoitit määritettäessä AD FS-klusterin Azure AD Connect salasana.
![Sertifikaatin salasana](media\active-directory-aadconnect-federation-management\WapServer3.PNG)

    ![Määritä SSL-varmenne](media\active-directory-aadconnect-federation-management\WapServer4.PNG)

4. Lisää palvelimen voidaan lisätä web-sovelluksen välityspalvelinta. Koska web-sovelluksen välityspalvelinta ei voi liittää toimialueeseen, ohjattu toiminto pyytää lisätään palvelimen järjestelmänvalvojan oikeudet.
![Järjestelmänvalvojan tunnistetiedot](media\active-directory-aadconnect-federation-management\WapServer5.PNG)

5. **Välityspalvelimen luota tunnistetiedot** -sivulla Anna välityspalvelimen luota ja AD FS-klusterin ensisijainen palvelimen järjestelmänvalvojan oikeudet.
![Salli käyttäjätietojen](media\active-directory-aadconnect-federation-management\WapServer6.PNG)

6. **Voit määrittää asetukset** -sivulla ohjattu toiminto näyttää luettelo toiminnoista, jotka suoritetaan.
![Voit määrittää asetukset](media\active-directory-aadconnect-federation-management\WapServer7.PNG)

7. Valitse Lopeta määritystä **asentaminen** . Kun määritykset on tehty, ohjatussa toiminnossa asetus, jos haluat tarkistaa yhteydet palvelimiin. Valitse yhteys **vahvistaminen** .
![Asennus valmis](media\active-directory-aadconnect-federation-management\WapServer8.PNG)

### Lisää liitetyssä toimialueessa<a name=addfeddomain></a>

On helppo lisätä käyttämällä Azure AD Connect kanssa Azure AD liitetty toimialue. Azure AD Connect Lisää federation toimialueen ja Muokkaa ryhmän säännöt vastaamaan myöntäjä oikein, kun käytössä on useita toimialueita, Azure AD äänipuheluita.

1. Jos haluat lisätä liitetyssä toimialueessa, valitse tehtävä **Lisää ylimääräinen Azure AD-toimialueen**.
![Lisätietoja Azure AD-toimialueesta](media\active-directory-aadconnect-federation-management\AdditionalDomain1.PNG)

2. Ohjatun toiminnon seuraavalla sivulla Azure AD yleisen järjestelmänvalvojan tunnistetietojen avulla.
![Azure AD yhdistäminen](media\active-directory-aadconnect-federation-management\AdditionalDomain2.PNG)

3. **Etäkäyttö tunnistetiedot** -sivulla on toimialueen järjestelmänvalvojan tunnistetietoja.
![Etäkäyttö tunnistetiedot](media\active-directory-aadconnect-federation-management\additionaldomain3.PNG)

4. Ohjatun toiminnon antaa seuraavalla sivulla, joiden järjestäjäorganisaatiota paikallisesta hakemistosta Azure AD-toimialueiden luetteloa. Valitse toimialue luettelosta.
![Azure AD-toimialueesta](media\active-directory-aadconnect-federation-management\AdditionalDomain4.PNG)

    Kun olet valinnut toimialueen, ohjattu toiminto antaa sinulle muita toimintoja, jotka ohjattu toiminto tekee ja määritykset vaikuttavat tarvittavat tiedot. Joissakin tapauksissa Jos valitset toimialueen, joka on ei ole vielä vahvistanut Azure AD-ohjattu toiminto antaa sinulle tietoja, joiden avulla voit tarkistaa toimialueen. Saat lisätietoja [Lisää Azure Active Directory mukautettua toimialuenimeä](active-directory-add-domain.md) .

5. Valitse **Seuraava**ja **Valmis määrittäminen** -sivulla näkyy luettelo toiminnoista, jotka suorittavat Azure AD Connect. Valitse Lopeta määritystä **asentaminen** .
![Voit määrittää asetukset](media\active-directory-aadconnect-federation-management\AdditionalDomain5.PNG)

## <a name="ad-fs-customization"></a>AD FS mukauttaminen

Seuraavissa osissa on lisätietoja joitakin yleisiä tehtäviä, joudut ehkä suorittamaan mukautettaessa AD FS-kirjautumissivulla.

### Lisää mukautetun yrityksen logo tai kuva<a name=customlogo></a>

Muuta, **Kirjaudu sisään** -sivulla näkyvän yrityksen logon noudattamalla seuraavia Windows PowerShell-cmdlet- ja syntaksivirheitä.

> [AZURE.NOTE] Logon suositellut mitat ovat 260 x 35 @ 96 dpi, joiden koko on enintään 10 Kilotavua.

    Set-AdfsWebTheme -TargetName default -Logo @{path="c:\Contoso\logo.PNG"}

> [AZURE.NOTE] *TargetName* -parametri on pakollinen. Oletusteeman, jotka on julkaistu AD FS nimi on oletusarvo.


### Lisää kuvaus-kirjautuminen<a name=addsignindescription></a>

Voit lisätä kirjautumissivu kuvaus **kirjautumissivu**käyttämällä seuraavia Windows PowerShell-cmdlet- ja syntaksivirheitä.

    Set-AdfsGlobalWebContent -SignInPageDescriptionText "<p>Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information.</p>"

### AD FS varaa sääntöjen muokkaaminen<a name=modclaims></a>

AD FS tukee monipuolisia varaa kieli, jonka avulla voit luoda mukautetun ryhmän säännöt. Lisätietoja on artikkelissa [Varaa säännön kieli rooli](https://technet.microsoft.com/library/dd807118.aspx).

Seuraavissa kohdissa kuvataan, miten voit kirjoittaa joissakin tilanteissa Azure AD liittyviä mukautettuja säännöt ja AD FS-liittoutumispalvelinten.

#### <a name="immutable-id-conditional-on-a-value-being-present-in-the-attribute"></a>Pysyvä edellytyksenä on, että esitä-määritteen arvon tunnus

Azure AD Connect avulla voit määrittää käytettävän lähteen ankkurin, kun objektit on synkronoitu Azure AD määrite. Jos mukautetun määritteen arvo ei ole tyhjä, haluat ehkä annettava pysyvä tunnus-ryhmän. Voit esimerkiksi valita **ms-ds-consistencyguid** lähde-ankkuri-määritteen ja haluat myöntää **ImmutableID** **ms-ds-consistencyguid** , siltä varalta, että määrite on sitä vastaan arvo. Jos ei ole arvoa määrite vastaan, antaa **objectGuid** pysyvä ID-tunnuksellasi.  Voit luoda mukautetun ryhmän sääntöjen seuraavassa kohdassa kuvatulla tavalla.

**Sääntö 1: Kyselyn määritteet**

    c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]
    => add(store = "Active Directory", types = ("http://contoso.com/ws/2016/02/identity/claims/objectguid", "http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid"), query = "; objectGuid,ms-ds-consistencyguid;{0}", param = c.Value);

Tämä sääntö on kyselyt **ms-ds-consistencyguid** ja **objectGuid** Active Directory-käyttäjän arvot. Muuta liikkeen nimi sopiva nimi AD FS-ympäristön. Myös saatavat-tyypin muuttaminen liitetyn viestinnän **objectGuid** ja **ms-ds-consistencyguid**määritysten mukaisesti erisnimen saatavat tyyppi.

Myös käyttämällä **Lisää** ja **ongelman**vältät lisääminen lähtevän ongelma kohteen ja käyttää arvojen arvoja. Lähetät myöhemmin säännön vaatimus, kun olet muodostanut minkä arvon kuin pysyvä tunnus.

**Sääntö 2: Tarkista, onko ms-ds-consistencyguid olemassa käyttäjälle**

    NOT EXISTS([Type == "http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid"])
    => add(Type = "urn:anandmsft:tmp/idflag", Value = "useguid");

Tämä sääntö määrittää väliaikaisen lipun nimeltä **idflag** , joka on määritetty **useguid** Jos ei ole **ms-ds-concistencyguid** täydennetty käyttäjälle. Logiikan takana tämä on tärkeä, että AD FS ei salli tyhjä väitteitä. Niin kun lisäät saatavat http://contoso.com/ws/2016/02/identity/claims/objectguid ja http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid säännön 1, voit lopu ylös **msdsconsistencyguid** varaa vain jos arvo täytetään käyttäjälle. Jos se ei ole täytetty, AD FS näkee, että se on tyhjä arvo ja pudottaa heti. Kaikissa objekteissa on **objectGuid**, jotta kyseisen ryhmän ovat aina valmiina säännön 1 suorittamisen jälkeen.

**Sääntö 3: Antaa ms-ds-consistencyguid pysyvä tunnisteeksi, jos se ei sisällä tietoja**

    c:[Type == "http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid"]
    => issue(Type = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier", Value = c.Value);

Tämä on implisiittisesti **olemassa** -valintaruutu. Jos vaatimus arvo löytyy, valitse ongelman, jota kuin pysyvä tunnus. Edellisessä esimerkissä, **nameidentifier** -ryhmän. Sinun on muutettava tämä asianmukainen vaatimus tyyppi pysyvä ID-ympäristössä.

**Sääntö 4: Antaa objectGuid pysyvä tunnisteeksi Jos ms-ds-consistencyGuid ei ole käytössä**

    c1:[Type == "urn:anandmsft:tmp/idflag", Value =~ "useguid"]
    && c2:[Type == "http://contoso.com/ws/2016/02/identity/claims/objectguid"]
    => issue(Type = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier", Value = c2.Value);

Tämän säännön tarkistetaan yksinkertaisesti tilapäinen merkinnän **idflag**. Voit päättää, antaa varaa sen arvon perusteella.

> [AZURE.NOTE] Sääntöjen järjestyksen on tärkeää.

#### <a name="sso-with-a-subdomain-upn"></a>SSO alitoimialueen täydellisen Käyttäjätunnuksen kanssa

Voit lisätä useita toimialueita, liitetty käyttämällä Azure AD Connect-kohdassa [Lisää uusi liitetyssä toimialueessa](active-directory-aadconnect-federation-management.md#add-a-new-federated-domain). On muokattava UPN vaatimus niin, että myöntäjä tunnus vastaa päätoimialue ja ei alitoimialueen, koska liitetyt päätoimialue kattaa myös lapsen.

Oletusarvon mukaan myöntäjän tunnus varaa sääntö on määritetty:

    c:[Type
    == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(c.Value, “.+@(?<domain>.+)“, “http://${domain}/adfs/services/trust/“));

![Oletusarvoinen myöntäjän tunnus varaaminen](media\active-directory-aadconnect-federation-management\issuer_id_default.png)

Oletus-säännön kestää täydellisen Käyttäjätunnuksen jälkiliitteen yksinkertaisesti ja käyttää sitä myöntäjän tunnus-ryhmän. Esimerkiksi Teemu on sub.contoso.com käyttäjän ja contoso.com on äänipuheluita Azure AD. Teemu Lisää john@sub.contoso.com allekirjoittamisen Azure AD-käyttäjänimi ja oletusarvon myöntäjän tunnus väittää AD FS sääntö käsittelee seuraavalla tavalla.

    c:[Type
    == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(john@sub.contoso.com, “.+@(?<domain>.+)“, “http://${domain}/adfs/services/trust/“));

**Väittää arvo:** http://sub.contoso.com/adfs/services/trust/

Haluat, että vain myöntäjän varaa arvo päätoimialue vastaamaan seuraavista varaa säännön muuttaminen.

    c:[Type == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(c.Value, “^((.*)([.|@]))?(?<domain>[^.]*[.].*)$”, “http://${domain}/adfs/services/trust/“));

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja [Käyttäjäasetusten Kirjaudu sisään](active-directory-aadconnect-user-signin.md).

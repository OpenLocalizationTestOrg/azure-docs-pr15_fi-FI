<properties
    pageTitle="Käyttäjätietojen synkronointi ja kaksoiskappaleiden määrite vikasietoisuudelle | Microsoft Azure"
    description="Uusi toiminta käsittelemisestä objektit, joissa on UPN tai ProxyAddress ristiriitoja käyttämällä Azure AD Connect directory-synkronoinnin aikana."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/29/2016"
    ms.author="markusvi"/>



# <a name="identity-synchronization-and-duplicate-attribute-resiliency"></a>Käyttäjätietojen synkronointi ja monista määritteen vikasietoisuudelle
Kaksoiskappaleiden määritettä Vikasietoisuudelle toimintoa Azure Active Directoryn, joka aiheuttaa **UserPrincipalName** ja **ProxyAddress** suoritettaessa jokin Microsoftin synkronoinnin työkaluja ristiriidat hankaukselle poistetaan.

Nämä kaksi määritteet ovat yleensä tarvitse olla yksilöiviä usean **käyttäjän**, **ryhmän**tai **yhteyshenkilön** objektien annetun Azure Active Directory-vuokraajan.

> [AZURE.NOTE] Vain käyttäjät, voi olla UPNs.

Uusia toimintoja, jotka tämän ominaisuuden avulla on synkronointi-myyntijakso cloud yläosassa, siksi asiakkaan agnostic- ja minkä tahansa Microsoft-synkronoinnin tuotteen mukaan lukien Azure AD Connect, Dirsync-työkalun ja MIM + yhdistimen kannalta. Yleinen termi "synkronointiohjelma" käytetään tässä asiakirjassa edustavat jotakin näistä tuotteista.

## <a name="current-behavior"></a>Nykyinen toiminta
Jos näkyvissä on yritettiin valmistella uuden objektin UPN tai ProxyAddress arvo, joka vastainen yksilöllisyyden rajoitusta, Azure Active Directory estää objektin luomisen. Jos objekti on päivitetty ei-yksilöivän UPN tai ProxyAddress, päivitys vastaavasti epäonnistuu. Valmistelu yritys tai päivitys yritetään yhteydessä Vie kunkin jakson synkronointi-asiakas ja jatkaa epäonnistuu, kunnes ristiriita on selvitetty. Virhe raportin sähköpostin luodaan jokaisen yrityksen yhteydessä ja virheen tallentamat synkronointiohjelma.

## <a name="behavior-with-duplicate-attribute-resiliency"></a>Kaksoiskappaleiden määritettä Vikasietoisuudelle ongelma
Sen sijaan, että puuttuessa kokonaan valmistelu tai päivitä objektin kaksoiskappaleiden määrite Azure Active Directory "eristää" kaksoiskappaleiden määrite joka rikkoisi yksilöllisyyden rajoitus. Jos määrite on pakollinen valmisteluun, kuten UserPrincipalName-palvelun määrittää paikkamerkin-arvoa. Muoto on väliaikainen arvot  
"***<OriginalPrefix>+<4DigitNumber>@<InitialTenantDomain>. onmicrosoft.com***".  
Jos määrite ei tarvita, kuten **ProxyAddress**, Azure Active Directory eristää ristiriidan määritettä yksinkertaisesti ja käyttäjäobjektin luominen tai päivittäminen etenee.

Ehkäisytoimenpiteet määrite tietoja ristiriidasta lähetetään vanhaa toimintaa käyttää samaa virhe raportin uusien sähköpostien. Tämä tieto näkyy vain virheraportin yhden kerran Karanteeni tapahtuu, kun se ei kuitenkin kirjautunut tulevan sähköpostin liitteenä. Myös, koska Vie objektin onnistui-synkronointisovelluksen ei kirjaa virheen ja ei yritä uudelleen Luo / päivitystoiminto myöhemmin synkronointi jaksot yhteydessä.

Tämä ongelma tukemaan uusi määrite on lisätty käyttäjän, ryhmän ja yhteyshenkilön objektiluokat:  
**DirSyncProvisioningErrors**

Tämä on moniarvoinen määritettä, jota käytetään ristiriitaisia määritteitä, jotka rikkoisi yksilöllisyyden rajoite ne lisätään yleensä tallentamiseen. Taustan ajastin tehtävän on otettu Azure Active Directoryn, joka suoritetaan, tarkista päällekkäinen määritteen ristiriidoista, jotka on ratkaistu ja poistettavaan määritteet poistetaan automaattisesti karanteeniin tunnissa.

### <a name="enabling-duplicate-attribute-resiliency"></a>Kaksoiskappaleiden määrite Vikasietoisuudelle ottaminen käyttöön
Kaksoiskappaleiden määrite Vikasietoisuudelle on uuden oletustoiminnan kaikki Azure Active Directory-alihallinnat yli. Se on oletusarvoisesti kaikkien alihallinnat, jotka ensimmäistä kertaa 2016 22 elokuussa tai uudempia synkronointi käytössä. Alihallinnat, jotka ennen tätä päivää synkronointi käytössä on toiminto, joka on käytössä erissä. Tämä rahoittaja alkaa syyskuussa 2016 ja sähköposti-ilmoituksen lähetetään kunkin vuokraajan tekninen ilmoitus yhteyshenkilön kanssa tiettynä päivänä, kun ominaisuus käytössä.

Kun määrite Vikasietoisuudelle kaksoiskappaleet on otettu käyttöön sitä ei voi poistaa käytöstä.

Jos haluat tarkistaa, jos ominaisuus on käytössä vuokraajan, voit tehdä PowerShellin Azure Active Directory-moduulin uusimman version lataamalla ja suorittamalla:

`Get-MsolDirSyncFeatures -Feature DuplicateUPNResiliency`

`Get-MsolDirSyncFeatures -Feature DuplicateProxyAddressResiliency`

Jos haluat ottaa käyttöön itse, ennen kuin se on otettu käyttöön vuokraajan, voit tehdä PowerShellin Azure Active Directory-moduulin uusimman version lataamalla ja suorittamalla:

`Set-MsolDirSyncFeature -Feature DuplicateUPNResiliency -Enable $true`

`Set-MsolDirSyncFeature -Feature DuplicateProxyAddressResiliency -Enable $true`

## <a name="identifying-objects-with-dirsyncprovisioningerrors"></a>Objektit, joissa on DirSyncProvisioningErrors tunnistaminen
Kahdella tällä hetkellä tunnistaa objektit, joissa on virheiden vuoksi kaksoiskappaleiden ominaisuuden ristiriidassa, PowerShellin Azure Active Directory- ja Office 365-hallintaportaalissa. Saatavilla on muita portaalin perustuvat raportointi myöhemmin laajentaa palvelupaketteja.

### <a name="azure-active-directory-powershell"></a>PowerShellin Azure Active Directory
Tässä ohjeaiheessa PowerShell-cmdlet-toteutuu:

- Kaikki seuraavat cmdlet-komennot ovat kirjainkoko on merkitsevä.
- **– ErrorCategory PropertyConflict** on aina oltava. Tällä hetkellä ei muuntyyppisten **ErrorCategory**, mutta tämä voidaan laajentaa myöhemmin.

Aloita ensin suorittamalla **Yhdistä MsolService** ja syöttäminen vuokraajan järjestelmänvalvojan tunnistetietoja.

Käytä jälkeen seuraavat cmdlet-komennot ja operaattorit virheiden tarkasteleminen eri tavalla:

1. [Näet kaikki](#see-all)

2. [Ominaisuuden tyypin mukaan](#by-property-type)

3. [Ristiriitaiset arvon mukaan](#by-conflicting-value)

4. [Merkkijono-haun avulla](#using-a-string-search)

5. [Lajiteltu](#sorted)

6. [Rajoitettu määrä tai kaikki](#in-a-limited-quantity-or-all)


#### <a name="see-all"></a>Näet kaikki
Kun yhteys on muodostettu, voit tarkastella Yleinen luettelo määritteiden valmistelu virheitä vuokraajan suorittamalla:

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict`

Tämä tuloksena on esimerkiksi seuraavat:  
 ![Hae MsolDirSyncProvisioningError](./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/1.png "Get-MsolDirSyncProvisioningError")  


#### <a name="by-property-type"></a>Ominaisuuden tyypin mukaan
Virheet ominaisuuden tyypin mukaan näkyviin **UserPrincipalName** tai **ProxyAddresses** -argumentin **- PropertyName** -merkinnän lisääminen:

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -PropertyName UserPrincipalName`

Tai

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -PropertyName ProxyAddresses`

#### <a name="by-conflicting-value"></a>Ristiriitaiset arvon mukaan
Voit tarkastella virheitä, jotka liittyvät tietyn ominaisuuden (**- PropertyName** on käytettävä sekä tämän merkinnän lisättäessä) **- PropertyValue** -merkinnän lisääminen:

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -PropertyValue User@domain.com -PropertyName UserPrincipalName`


#### <a name="using-a-string-search"></a>Merkkijono-haun avulla
Laaja merkkijonon haun Käytä **- SearchString** merkinnän. Tämä voidaan käyttää itsenäisesti kaikki edellä mainitut liput lukuun ottamatta **-ErrorCategory PropertyConflict**, joka on aina pakollinen:

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -SearchString User`

#### <a name="in-a-limited-quantity-or-all"></a>Rajoitettu määrä tai kaikki
1. **MaxResults <Int> ** avulla voidaan rajoittaa kyselyn arvot tietty määrä.

2. **Kaikki** voidaan varmistaa kaikki tulokset noudetaan siinä tapauksessa, joka on runsaasti virheitä on olemassa.

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -MaxResults 5`

## <a name="office-365-admin-portal"></a>Office 365-hallintaportaalissa

Voit tarkastella kansion synkronointivirheiden Office 365-hallintakeskuksessa. Office 365-portaalissa raportti näyttää vain **käyttäjän** objektit, joissa on nämä virheet. Voit lukea lisää ristiriitojen **ryhmiä** ja **yhteystietoja**ei näy.


![Aktiiviset käyttäjät] (./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/1234.png "Aktiiviset käyttäjät")

Ohjeita siitä, miten voit tarkastella kansion synkronoinnin virheiden ratkaiseminen Office 365-hallintakeskuksessa artikkelissa [tunnista directory synkronointivirheiden Office 365: ssä](https://support.office.com/en-us/article/Identify-directory-synchronization-errors-in-Office-365-b4fc07a5-97ea-4ca6-9692-108acab74067).


### <a name="identity-synchronization-error-report"></a>Käyttäjätietojen hakemistosynkronoinnin virheraportin
Kun kaksoiskappaleiden määrite ristiriitoja objektien käsitellään vuokraajan Ota näin ilmoituksen sisältyy vakio tunnistetietojen Hakemistosynkronoinnin virheraportin sähköpostiviesti, joka lähetetään tekninen ilmoitus. On kuitenkin tärkeä muutos tämän ongelman. Aiemmin kaksoiskappaleiden määritettä ristiriitoja tietoja sisältyy jokaisen myöhemmin virheraportin kunnes ristiriita on ratkennut. Tätä toimintaa, jossa tietyn ristiriidan virhe-ilmoitus näkyviin vain kerran - ristiriitaiset määritettä karanteeniin aikaan.

Tässä on esimerkki siitä, kuten näyttää sähköposti-ilmoituksen ProxyAddress ristiriidan:  
    ![Aktiiviset käyttäjät](./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/6.png "Active Users")  

## <a name="resolving-conflicts"></a>Ristiriitojen ratkaiseminen
Vianmääritys strategia ja tarkkuus taktiikoita näitä virheitä saa erota eri kaksoiskappaleiden määrite virheet on käsitellään menneisyydessä. Ainoa ero on, että ajastin-tehtävän objektipinoja service-puolella vuokraajan lisätä poistettavaan määrite ERISNIMI-objektiin automaattisesti, kun ristiriita on ratkaistu kautta.

Seuraavassa artikkelissa selostetaan eri vianmäärityksen ja korjauksen strategioita: [kaksoiskappaleiden tai virheelliset attribuutit estävät hakemistosynkronoinnin Office 365: ssä](https://support.microsoft.com/kb/2647098).

## <a name="known-issues"></a>Tunnetut ongelmat
Ei mitään tunnetut ongelmat aiheuttaa tietojen menetyksen tai palvelun heikkeneminen. Useita, ne ovat ulkoasua, muiden aiheuttavat vakio "*vanhat vikasietoisuudelle*" kaksoiskappaleiden määrite virheitä ilmenee sen sijaan, että ristiriidan määritettä ehkäisytoimenpiteet ja toiseen aiheuttaa tiettyjä virheitä edellyttämään ylimääräisiä Manuaalinen korjaaminen.

**Core toiminta:**

1. Objektit, joissa on määrite käyttömahdollisuudet edelleen vastaanottaa Vie virheitä kaksoiskappaleiden määritteet, joita karanteeniin sijaan.  
Esimerkki:

    a. Uusi käyttäjä luodaan AD UPN jossa **Joe@contoso.com** ja ProxyAddress**smtp:Joe@contoso.com**

    b. Objektin ominaisuudet ovat ristiriidassa aiemmin luotuun ryhmään, jossa on ProxyAddress **SMTP:Joe@contoso.com**.

    c-näppäinyhdistelmää. Vie-yhteydessä **ProxyAddress ristiriita** -virhe ilmenee sen sijaan, että karanteeniin ristiriidan määritteet. Toiminnon yritetään myöhemmin synkronointi kunkin jakson yhteydessä, kun se pitänyt ennen vikasietoisuudelle-ominaisuus on käytössä.

2. Jos kaksi ryhmää luodaan paikallisen sama SMTP-osoite, yksi epäonnistuu ensimmäisellä kerralla vakio kaksoiskappaleiden **ProxyAddress** virheen valmistelu. Päällekkäinen arvo on kuitenkin oikein karanteenissa jakson seuraavan synkronoinnin yhteydessä.

**Office-portaalin raportin**:

1. Kaksi objektia UPN ristiriidan joukon virhesanoman on sama. Tämä ilmaisee, että olet molemmat ovat samat niiden UPN muuttaa / karanteeniin, jos mahdollista vain yhden niistä ollut muuttaa tietoja.

2. Täydellisen Käyttäjätunnuksen ristiriitoja yksityiskohtainen virhesanoma näkyy käyttäjälle, joka on ollut niiden UPN muuttaa karanteeniin asetettujen väärä näyttönimi. Esimerkki:

    a. **Käyttäjä A** Synkronoi ensimmäinen määrittäminen **UPN = User@contoso.com **.

    b. **Käyttäjä B** yritti voi synkronoida vuorossa kanssa **UPN = User@contoso.com **.

    c-näppäinyhdistelmää. **Käyttäjän B** Täydellisen Käyttäjätunnuksen muutetaan **User1234@contoso.onmicrosoft.com** ja **User@contoso.com** **DirSyncProvisioningErrors**lisätään.

    d. **Käyttäjä B** virhesanoma kertoo, että **käyttäjä A** on jo **User@contoso.com** kuten Yksilöllisen, mutta se näkyy **Käyttäjän B** oma näyttönimi.



**Käyttäjätietojen hakemistosynkronoinnin virheraportin**:

Ohjeita *siitä, miten voit ratkaista ongelman* linkki on virheellinen:  
    ![Aktiiviset käyttäjät](./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/6.png "Active Users")  

Se osoitettava [https://aka.ms/duplicateattributeresiliency](https://aka.ms/duplicateattributeresiliency).


## <a name="see-also"></a>Katso myös

- [Azure AD Connect synkronointi](active-directory-aadconnectsync-whatis.md)

- [Azure Active Directory-integraation paikallisen käyttäjätietoja](active-directory-aadconnect.md)

- [Määritä kansion synkronointivirheiden Office 365: ssä](https://support.office.com/en-us/article/Identify-directory-synchronization-errors-in-Office-365-b4fc07a5-97ea-4ca6-9692-108acab74067)

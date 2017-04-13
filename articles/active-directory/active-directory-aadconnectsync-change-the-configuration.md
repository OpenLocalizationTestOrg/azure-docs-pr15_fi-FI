<properties
    pageTitle="Azure AD Connect synkronointi: Voit tehdä muutoksia työnkulkuun oletusarvon | Microsoft Azure"
    description="Esitellään voit tehdä muutoksia työnkulkuun Azure AD Connect synkronointiin."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/31/2016"
    ms.author="billmath"/>


# <a name="azure-ad-connect-sync-how-to-make-a-change-to-the-default-configuration"></a>Azure AD Connect synkronointi: Voit muuttaa oletusarvoiset määritykset
Tässä ohjeaiheessa tarkoituksena on opastusta voit tehdä muutoksia työnkulkuun oletusarvon Azure AD Connect synkronointiin. Se sisältää joitakin yleisiä tilanteita, joissa vaiheet. Sinun pitäisi tehdä yksinkertaisia muutoksia oman määrittäminen liiketoimintasääntöjen perusteella tätä kokemusta.

## <a name="synchronization-rules-editor"></a>Synkronoinnin säännöt-editori
Voit tarkastella ja muuttaa oletusarvon määrittäminen käytetään synkronoinnin säännöt-editorin. Löydät sen **Azure AD Connect** -ryhmän Käynnistä-valikossa.  
![Käynnistä-valikon Synkronoi säännön editori](./media/active-directory-aadconnectsync-change-the-configuration/startmenu2.png)

Kun avaat sen, näet oletusarvon ruutuun sääntöjä.

![Synkronoi sääntö-editori](./media/active-directory-aadconnectsync-change-the-configuration/sre2.png)

### <a name="navigating-in-the-editor"></a>Siirtyminen-editorissa
Editorin yläreunassa avattavat avulla voit etsiä nopeasti tietyn säännön. Jos haluat nähdä sääntöjä, johon sisältyy määritettä proxyAddresses, voit muuttaa avattavat-seuraavankaltaiselta:  
![SRE suodattaminen](./media/active-directory-aadconnectsync-change-the-configuration/filtering.png)  
Palauta suodattaminen ja ajan tasalla määrityksen lataaminen painamalla näppäimistön **F5-näppäintä** .

Oikeasta yläkulmasta edellyttää painikkeen **Lisää uusi sääntö**. Tämän painikkeen avulla voit luoda mukautetun säännön.

Alalaidassa noudattavat painikkeet ovat Synkronoi valitun säännön. **Muokkaa** ja **Poista** tehdä mitä näköiset. **Vie** tuottaa PowerShell-komentosarjaa varten luotaessa synkronointi sääntö. Tämän toiminnon avulla voit siirtää synkronointi säännön palvelimesta toiseen.

## <a name="create-your-first-custom-rule"></a>Ensimmäisen mukautetun säännön luominen
Yleisimmät muutos on määrite kulkee muutokset. Lähdekansion tiedot eivät välttämättä ole kuin Azure AD. Esimerkin sisältö, jonka haluat Varmista, että käyttäjän etunimi on aina **ERISNIMI-funktiolla**.

### <a name="disable-the-scheduler"></a>Poistaa ajoitustoiminnon käytöstä
[Ajoitus](active-directory-aadconnectsync-feature-scheduler.md) suoritetaan 30 minuutin välein oletusarvon mukaan. Haluat Varmista, että se ei käynnisty samalla, kun teet muutoksia ja vianmääritys uudet säännöt. Voit tilapäisesti poistaa ajoitustoiminnon käytöstä, Käynnistä PowerShell ja suorittaminen`Set-ADSyncScheduler -SyncCycleEnabled $false`

![Poistaa ajoitustoiminnon käytöstä](./media/active-directory-aadconnectsync-change-the-configuration/schedulerdisable.png)  

### <a name="create-the-rule"></a>Luo sääntö

1. Valitse **Lisää uusi sääntö**.
2. Kirjoita **kuvaus** -sivulla seuraavasti:  
![Saapuva säännön suodattaminen](./media/active-directory-aadconnectsync-change-the-configuration/description2.png)  
    - Nimi: Anna säännön kuvaava nimi.
    - Kuvaus: Jotkin selventää niin toisen ymmärtää sääntö on varten.
    - Yhdistetty järjestelmän: objektin löytyvät järjestelmä. Tässä tapauksessa emme Valitse Active Directory Connectorin.
    - Yhdistetyn järjestelmän/Metaverse objektityyppi: Valitse **käyttäjä** - ja **henkilön** .
    - Linkkityyppi: Vaihda arvoksi **liittymään**.
    - Järjestys: On arvo, joka on yksilöllinen järjestelmässä. Pienet numeerinen arvo osoittaa laskettavat.
    - Tunnisteen: Jättää tyhjäksi. Vain ruutuun sääntöjä Microsoftin on tämä ruutu, jossa arvo.
3. Anna **givenName ISNOTNULL** **Scoping suodatin** -sivulla.  
![Saapuvan liikenteen sääntö rajauksen suodatin](./media/active-directory-aadconnectsync-change-the-configuration/scopingfilter.png)  
Tässä osassa voidaan määrittää, mitkä-objektit sääntö on voimassa. Jos tyhjä, sääntö koskee kaikki käyttäjän objektit. Mutta, joka sisältää neuvotteluhuoneiden palvelutilejä ja henkilöt muita käyttäjäobjekteja.
4. **Liittyminen säännöt**jätä se tyhjä.
5. Muuta **muunnoksia** -sivun FlowType-kohteen **lauseke**. Valitse kohde-määritteen **givenName**ja kirjoita lähde `PCase([givenName])`.
![Saapuvan liikenteen sääntö muunnoksia](./media/active-directory-aadconnectsync-change-the-configuration/transformations.png)  
Synkronoi-ohjelma kirjainkoko on merkitsevä sekä Funktionimi määritteen nimi. Jos kirjoitat väärän jotain, käytettävyysvaroitus tulee näkyviin, kun lisäät sääntö. Editorin avulla voit Tallenna ja jatka, joten sinun on sääntö Avaa ja korjaa sääntö.
6. Valitse **Lisää** Tallenna sääntö.

Uuden mukautetun säännön pitäisi näkyä synkronointi-sääntöjen järjestelmässä.

### <a name="verify-the-change"></a>Vahvista muutos
Haluat uuden muutos, varmista, että se toimii odotetulla tavalla ja on yliheittävä ei virheitä. Sen mukaan, sinulla on objektien määrä on kahdella eri tavalla, voit tehdä tämän vaiheen.

1. Suorita koko Synkronoi kaikki objektit
2. Suorita esikatselu ja koko synkronointi yhtenä objektina

Käynnistä **Synkronointipalvelu** Käynnistä-valikosta. Tämän osan vaiheet ovat tämä työkalu.

1. **Koko Synkronoi kaikki objektit**  
Valitse **yhdistimet** yläreunassa. Määritä yhdysviivan tekemäsi muutoksen edellisessä osassa, tässä tapauksessa Active Directory ‑toimialueen palveluihin, ja valitse se. Valitse **Suorita** toiminnot ja valitse **Täyden käyttäjätietojen** ja **OK**.
![Koko synkronointi](./media/active-directory-aadconnectsync-change-the-configuration/fullsync.png)  
Objektit on päivitetty metaverse. Haluat nyt tarkastella metaverse objektia.

2. **Esikatselu ja koko Synkronoi yhtenä objektina**  
Valitse **yhdistimet** yläreunassa. Määritä yhdysviivan tekemäsi muutoksen edellisessä osassa, tässä tapauksessa Active Directory ‑toimialueen palveluihin, ja valitse se. Valitse **Etsi yhdistimen tilaa**. Laajuus avulla voit etsiä objektin haluat testata muutoksen avulla. Valitse objekti ja valitse **Esikatselu**. Valitse uusi-näytössä **Vahvista esikatselu**.
![Vahvista esikatselu](./media/active-directory-aadconnectsync-change-the-configuration/commitpreview.png)  
Muutos on nyt sitoutunut metaverse.

**Tarkista metaverse objekti**  
Haluat nyt muutama Esimerkki objektien varmistaaksesi, että arvo on odotettavissa ja sääntöä käytetään. Valitse **Metaverse etsintä** alusta. Lisää kaikki suodattimet, Etsi haluamasi objektit. Avaa hakutulokset-objekti. Tarkista määritteiden arvot ja tarkistaa **Synkronoinnin säännöt** -sarakkeessa, joka sääntöä käytetään kuin oikein.  
![Metaverse haku](./media/active-directory-aadconnectsync-change-the-configuration/mvsearch.png)  
### <a name="enable-the-scheduler"></a>Ota käyttöön ajoitus
Jos kaikki on oikein, voit ottaa ajoituksen uudelleen. Suorita PowerShell- `Set-ADSyncScheduler -SyncCycleEnabled $true`.

## <a name="other-common-attribute-flow-changes"></a>Muita yleisiä määrite työnkulku-muutoksia
Voit tehdä muutoksia määrite työnkulku on kuvattu edellisessä osassa. Tässä osassa toimitetaan muita esimerkkejä. Synkronoi-säännön luominen vaiheet lyhennetty, mutta voit etsiä koko vaiheet edellisessä osassa.

### <a name="use-another-attribute-than-the-default"></a>Käytä toisessa määritettä kuin oletusarvoista
Fabrikam-palvelussa on metsää, jossa paikallisen aakkosten käytetään Etunimi, Sukunimi ja näyttönimi. Nämä määritteet latinalainen esittäminen löytyvät tunnisteen määritteet. Luotaessa yleisen osoiteluettelon Azure AD- ja Office 365-organisaation haluaa nämä määritteet, joita voidaan käyttää sen sijaan.

Oletus-kokoonpanossa objektin paikallisen metsää näyttää seuraavanlaiselta:  
![Työnkulun määritteiden 1](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp1.png)

Luo sääntö ja muut määrite työnkulut, toimi seuraavasti:

- Aloita **Synkronointi säännön editorissa** Käynnistä-valikosta.
- **Saapuvien** vasemmalle ovat valittuina, ja napsauta **Lisää uusi sääntö**-painiketta.
- Anna säännölle nimi ja kuvaus. Valitse paikallisen Active Directory ja asiaa objektityypit.  Valitse **Linkkityyppi**-kohdassa **Liity**. Saat järjestys valitse numero, joka ei ole toisen säännön perusteella. Valinta sääntöjä Aloita 100, jolloin arvo 50 voidaan käyttää tässä esimerkissä.
![Työnkulun määritteiden 2](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp2.png)
- Jätä laajuus tyhjä (eli olisi koskevat kaikkia käyttäjän metsää).
- Liity säännöt jättää tyhjäksi (eli Anna käsitellä liitokset ruutuun sääntö).
- Luo seuraavat kulkee muunnokset:  
![Työnkulun määritteiden 3](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp3.png)
- Valitse **Lisää** Tallenna sääntö.
- Siirry **Synkronoinnin palvelun hallinta**. **Yhdistimien**valitse yhdistin, johon on lisätty sääntö. Valitse **Suorita**ja **täyden käyttäjätietojen**. Täyden käyttäjätietojen laskee kaikkien objektien nykyisen sääntöjen avulla.

Tämä on sama objekti, jossa on mukautettuja säännöllä tulos:  
![Työnkulun määritteiden 4](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp4.png)

### <a name="length-of-attributes"></a>Pituuden, jonka määritteet
Yhteysmerkkijonon määritteet ovat oletusarvoisesti termijoukko Indeksoitavalla ja enimmäispituus on 448 merkkiä. Jos käsittelet merkkijonon määritteitä, jotka saattavat sisältää enemmän, varmista, että sisällytettävien määritteen kulun seuraavasti:  
`attributeName` <- `Left([attributeName],448)`

### <a name="changing-the-userprincipalsuffix"></a>UserPrincipalSuffix muuttaminen
Active Directoryn userPrincipalName-määritettä ei tiedetä aina käyttäjät ja eivät ehkä ole sopiva kuin kirjautumisen tunnus. Azure AD Connect, synkronointi ohjatun asennuksen avulla valitsemalla eri määrite, kuten sähköpostin. Mutta joissakin tapauksissa määritettä on laskettu. Esimerkiksi yrityksen Contoso on kaksi Azure AD-kansioita, yksi tuotannon ja yksi testikäyttöön. Käyttäjien niiden testi-kohdevuokraajassa, jotta voit käyttää toisen jälkiliite kirjautumisen tunnus.  
`userPrincipalName` <- `Word([userPrincipalName],1,"@") & "@contosotest.com"`

Tämä lauseke toteuttaa kaikki ensimmäisen vasemman @-sign (Word) ja KETJUTA kiinteä merkkijonolla.

### <a name="convert-a-multi-value-to-a-single-value"></a>Usean arvon muuntaminen single-arvo
Jotkin määritteet Active Directoryssa on moniarvoinen rakenteessa, vaikka ne näyttävät yksittäisen arvon Active Directoryn käyttäjät ja tietokoneet. Esimerkki on kuvaus-määrite.  
`description` <- `IIF(IsNullOrEmpty([description]),NULL,Left(Trim(Item([description],1)),448))`

Lausekkeen siltä varalta, että määrite on arvo, on otettava ensimmäisen kohteen (kohde)-määritteen alussa ja lopussa olevia välilyöntejä (rajaaminen) poistaminen ja säilyttää sitten merkkijonon ensimmäisen 448 merkkiä (vasen).

### <a name="do-not-flow-an-attribute"></a>Työnkulku ei määrite
Taustatietoa skenaarion tämän osion kohdassa [määrite työnkulku-prosessin](active-directory-aadconnectsync-understanding-declarative-provisioning.md#control-the-attribute-flow-process).

Jatkuvan ei määritteen kahdella eri tavalla. Ensimmäinen on käytettävissä ohjatun asennuksen ja avulla voit [poistaa valitut määritteet](active-directory-aadconnect-get-started-custom.md#azure-ad-app-and-attribute-filtering). Tämä asetus toimii, jos et ole koskaan synkronoinut määrite ennen. Jos olet aloittanut Synkronoi tämän määritteen ja poistaa sen myöhemmin tätä ominaisuutta, valitse synkronointi-ohjelma lopettaa määrite ja olemassa olevien arvojen hallinta jätetään Azure AD.

Jos haluat poistaa määritteen arvon ja varmista, että se ei rivity myöhemmin, sinun täytyy luoda mukautetun säännön.

Fabrikam-palvelussa on on toteutuneet, että joitakin määritteitä, emme synkronointi pilveen tulee siellä. Varmista, että nämä määritteet poistetaan Azure AD haluamme.  
![Virheellinen tunniste määritteet](./media/active-directory-aadconnectsync-change-the-configuration/badextensionattribute.png)

- Luo uusi saapuvien synkronoinnin sääntö ja täytä kuvaus ![kuvaukset](./media/active-directory-aadconnectsync-change-the-configuration/syncruledescription.png)
- Luo määritettä kulkee **lausekkeen** tyypin ja tietolähteeseen **AuthoritativeNull**. Literal **AuthoritativeNull** ilmaisee, että arvo on tyhjä ma vaikka alemman järjestys-synkronoinnin säännöllä pyritään täyttää arvo.
![Tunnisteen määritteet muunnos](./media/active-directory-aadconnectsync-change-the-configuration/syncruletransformations.png)
- Tallenna sääntö Synkronoi. Käynnistä **Synkronointipalvelu**, yhdistimen löydät, valitse **Suorita**ja **Täyden käyttäjätietojen**. Tässä vaiheessa laskee kaikkien määritteen työnkulkujen.
- Varmista, että tarkoitettu muutokset tietoja, jotka viedään hakemalla connector-tilaa.
![Vaiheistettu poistaminen](./media/active-directory-aadconnectsync-change-the-configuration/deletetobeexported.png)

## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja on artikkelissa [Tietoja määritettäviä valmistelu](active-directory-aadconnectsync-understanding-declarative-provisioning.md)määritys mallin.
- Lisätietoja on artikkelissa [tietoja määritettäviä valmistelu](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md)lausekkeissa lausekielellä.

**Yleistä aiheita**

- [Azure AD Connect synkronointi: tietoja ja mukauttaa synkronointi](active-directory-aadconnectsync-whatis.md)
- [Azure Active Directory-integraation paikallisen käyttäjätietoja](active-directory-aadconnect.md)

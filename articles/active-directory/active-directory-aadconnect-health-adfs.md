
<properties
    pageTitle="Käyttämällä Azure AD yhteyden kunto AD FS | Microsoft Azure"
    description="Tämä on Azure AD yhteyden kunto-sivulla voit valvoa yhteyttä paikalliseen AD FS-infrastruktuuria."
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>

# <a name="using-azure-ad-connect-health-with-ad-fs"></a>Käyttämällä Azure AD yhteyden kunto AD FS
Seuraavat asiakirjat on AD FS-infrastruktuurin kanssa Azure AD yhteyden kunnon valvonta. Lisätietoja Azure AD Connect (synkronointi) kanssa Azure AD yhteyden kunnon valvonta on artikkelissa [Käyttämällä Azure AD yhteyden kunto-synkronointia varten](active-directory-aadconnect-health-sync.md). Lisäksi tietoja Active Directory-toimialueen palveluiden kanssa Azure AD yhteyden kunnon valvonta on artikkelissa [Käyttämällä Azure AD yhteyden kunto AD DS: N kanssa](active-directory-aadconnect-health-adds.md).

## <a name="alerts-for-ad-fs"></a>AD FS ilmoitukset
Azure AD yhteyden kunto ilmoitukset-osassa on aktiivinen ilmoitusten luettelo. Kunkin ilmoitus sisältää tiedot, toimintaohjeiden ja linkkejä aiheeseen liittyvät ohjeet.

Voit kaksoisnapsauttaa aktiivinen tai ratkaistu ilmoituksen, voit avata uuden sivu lisätietoja vaiheet voit ratkaista ilmoituksen ja linkit asiakirjat. Voit myös tarkastella historiatietoja ilmoituksia, jotka on poistunut menneisyydessä.

![Azure AD Connect kunto-portaalissa](./media/active-directory-aadconnect-health/alert2.png)



## <a name="usage-analytics-for-ad-fs"></a>AD FS käyttöanalyysin
Azure AD yhteyden kunto käyttöanalyysin analysoi federation-palvelimien todennus-liikenne. Voit kaksoisnapsauttaa Avaa käyttö analytics-sivu, jossa näkyy useita arvot ja ryhmittelyjen käyttö-analytics-ruutuun.

>[AZURE.NOTE] Käyttöanalyysin käytettäväksi AD FS, sinun on varmistettava, että AD FS valvonta on käytössä. Lisätietoja on artikkelissa [AD FS seurannan ottaminen käyttöön](active-directory-aadconnect-health-agent-install.md#enable-auditing-for-ad-fs).

![Azure AD Connect kunto-portaalissa](./media/active-directory-aadconnect-health/report1.png)

Valitse muut arvot, Määritä aikaväli, voit muuttaa ryhmittelyjä, analytics Käyttökaavio hiiren kakkospainikkeella ja valitse Muokkaa kaavion. Voit sitten Määritä aikaväli, valitse eri metrijärjestelmä ja muuttaa ryhmittely. Voit tarkastella eri "käytännön" todennus liikenteen ja ryhmitellä asiaa "ryhmän mukaan" parametreilla seuraavassa taulukossa on kuvattu kunkin arvo:

| Arvo | Ryhmittelyperuste | Mitä ryhmittelyn keskiarvot ja miksi kannattaa käyttää? |
| ------ | -------- | -------------------------------------------- |
| Yhteensä pyytää: Kokonaismäärän pyynnöt käsitteleminen federation-palvelu | Kaikki | Näyttää ryhmittely ilman pyyntöjen kokonaismäärä määrää. |
|  | Sovelluksen | Ryhmien pyyntöjen perusteella kohdennettujen varmenteen käyttäjän osapuolen kokonaismäärä. Tämä ryhmittely on hyötyä ymmärtää, mihin sovellukseen lähetetään paljonko yhteensä liikenne prosentteina. |
|  | Palvelin | Ryhmien pyyntöjen perusteella palvelin, jossa käsitellään pyynnön kokonaismäärä. Selvittääksesi, yhteensä liikenne kuormituksen jakautumisen kannattaa tämän ryhmittelyn. |
|  | Työpaikan liitos | Ryhmien (tunnetaan) perusteella, onko ne ovat peräisin laitteet, jotka ovat liittyneet työpaikkasi yhteensä pyynnöt. Tämä ryhmittely on hyötyä selvittääksesi, jos resursseja voi käyttää käytät laitetta, joka on tuntematon identity-infrastruktuuria. |
|  | Todentamismenetelmä | Ryhmien pyyntöjen perusteella todennuksessa käytettävä todennusmenetelmä kokonaismäärä. Tämä ryhmittely kannattaa ymmärtää yhteinen todennusmenetelmä, joka saa käyttää todennusta varten. Seuraavassa on mahdollista todennustavat <ol> <li>Windows-todennusta (Windows)</li> <li>Lomakepohjainen todennus (lomakkeet)</li> <li>Kertakirjautuminen (kertakirjautuminen)</li> <li>X509 Varmenteen todentaminen (varmenne)</li> <br>Jos liittoutumispalvelimia saa pyynnön SSO-eväste, pyynnön lasketaan muodossa SSO (kertakirjautumisen). Jos eväste ei kelpaa, tässä tapauksessa käyttäjän pyydetään ei tunnistetietoja ja kaavan saumaton access-sovellukseen. Tämä on yleisiä, jos sinulla on useita liittoutumispalvelimia suojattu varmenteen käyttäjän osapuolille. |
|  | Verkkosijainti | Ryhmien pyyntöjen perusteella käyttäjän verkkosijainti kokonaismäärä. Voi olla joko intranet- tai ekstranet. Tämä ryhmittely on hyvä tietää, kuinka monta prosenttia liikenne on peräisin intranet ja ekstranet. |
| Epäonnistui-pyyntöjen kokonaismäärä: Kokonaismäärä epäonnistui federation palvelun käsittelemien pyynnöt. <br> (Tämä arvo on käytettävissä vain Windows Server 2012 R2 AD FS)| Virhetyyppi | Näyttää perustuvat ennalta määritettyjä virhe virheiden määrä. Tämä ryhmittely on hyötyä ymmärtää tavallisia virheitä. <ul><li>Virheellinen käyttäjänimi tai salasana: virheiden vuoksi Virheellinen käyttäjänimi tai salasana.</li> <li>"Ekstranet lukituksen": virheiden vuoksi käyttäjältä, joka on lukittu ekstranet-pyyntöjen </li><li> "Vanhentunut salasana": virheiden vuoksi kirjautuvat sisään vanhentunut salasana.</li><li>"Käytöstä tilin": virheiden vuoksi kirjautuvat ei käytössä-tilillä.</li><li>"Laitteen todennus": virheiden vuoksi käyttäjien todentamiseen laitteen todennusta puuttuessa.</li><li>"Käyttäjän todennus": virheiden vuoksi käyttäjät eivät läpäise vuoksi virheellinen varmenteen todentamiseen.</li><li>"MFA": virheiden vuoksi käyttäjä todennetaan käyttämällä Monimenetelmäisen todentamisen puuttuessa.</li><li>"Muut tunnistetiedon": "Liittoutumispalvelujen käyttöoikeuksien": virheiden vuoksi todennus epäonnistuu.</li><li>"Liittoutumispalvelujen delegointi": virheet liittoutumispalvelujen delegointi virheiden vuoksi.</li><li>"Tunnussanoma hyväksymisen": virheiden vuoksi ADFS hylkäämällä kolmannen osapuolen-tunnistetietojen toimittaja-tunnuksen.</li><li>"Protokolla": Virhe protokolla virheiden vuoksi.</li><li>"Tuntematon": sieppaa kaikki. Muut virheet, jotka eivät sovellu määritetty luokkiin.</li> |
|  | Palvelin | Ryhmien palvelimeen perustuvan virheet. Tämä ryhmittely on hyötyä ymmärtää palvelinten virhe-jakauman. Erimittainen jakauman voi olla viallinen tilaan palvelimen ilmaisin. |
|  | Verkkosijainti | Ryhmät virheet verkkosijainti (intranet ja ekstranet)-pyyntöjen perusteella. Tämä ryhmittely on hyötyä ymmärtää, epäonnistuvat pyynnöt tyyppi. |
|  | Sovelluksen | Ryhmien virheet kohdennettujen sovelluksen (varmenteen käyttäjän osapuolen) perusteella. Se on hyödyllinen selvittääksesi, mikä kohdistettu sovellus näkyy useimmissa virheiden määrä. |
| Käyttäjän: Laske keskiarvo aktiivinen järjestelmässä käyttäjien lukumäärä | Kaikki | Tämä arvo on liitetty viestintä-palvelun käyttäminen valitun aikajakso käyttäjien määrän keskiarvo laskeminen. Käyttäjien ei ole ryhmitelty. <br>Keskimääräinen määräytyy valittuna aikajakso. |
|  | Sovelluksen | Ryhmien perusteella kohdennettujen (varmenteen käyttäjän osapuolen)-sovelluksen käyttäjien määrän keskiarvo. Se on hyödyllinen ymmärtää, kuinka monta käyttäjää on käytössä mihin sovellukseen. |


## <a name="performance-monitoring-for-ad-fs"></a>Suorituskyvyn AD FS: lle
Seurantatiedot Azure AD yhteyden kunto suorituskyvyn seurantaa sisältää arvojen mukaisesti. Seuranta-ruutuun valitsemalla Avaa uusi sivu, jossa yksityiskohtaiset tiedot arvojen mukaisesti.


![Azure AD Connect kunto-portaalissa](./media/active-directory-aadconnect-health/perf1.png)


Valitsemalla Suodatusvaihtoehto yläreunaan sivu, voit suodattaa palvelin, näet yksittäisen palvelimen arvot. Arvot, seuranta kaavion seuranta-sivu-kohdassa hiiren kakkospainikkeella ja valitse sitten Muokkaa kaavio. Valitse uusi sivu, joka avaa, voit valita muita arvot avattavasta luettelosta ja määrittää aikavälin suorituskyvyn tietojen tarkastelemista varten.

## <a name="reports-for-ad-fs"></a>AD FS-raporteissa
Azure AD yhteyden kunto on raportteja tehtävän ja AD FS suorituskykyä. Näiden raporttien avulla järjestelmänvalvojat Hanki tietoja toimintoja niiden AD FS-palvelimiin.

### <a name="top-50-users-with-failed-usernamepassword-logins"></a>Ensimmäiset 50-käyttäjät, joilla on epäonnistunut käyttäjänimellä ja salasanalla kirjautumiset

Tilanteet, joissa epäonnistui käyttöoikeuksien pyytäminen ADFS-palvelimeen on pyyntö ei kelpaa käyttöoikeuksilla, eli Väärä käyttäjänimi tai salasana. Tapahtuu yleensä käyttäjille monimutkaisia salasanoja, unohtunut salasana tai kirjoitusvirheitä vuoksi.

On myös muita syitä, mikä voi johtaa pyynnöt parhaillaan käsittelee AD FS-palvelimet, kuten odottamattoman määrän, mutta: sovellus, joka tallentaa käyttäjätietoja ja tunnistetietoja vanhentumisvälin tai joku yrittää kirjautua tilille, jolla tunnetun salasanat sarjaa. Nämä kaksi esimerkkiä ovat kelvollisia syitä, joka saattaa aiheuttaa ylijännitesuojan pyyntöjä.

Azure AD yhteyden kunto ADFS varten on ylimmän 50 käyttäjät, joilla on epäonnistunut kirjautuminen yritykset vuoksi Virheellinen käyttäjänimi tai salasana raportin. Tämä raportti saavuttaa käsittely klustereihin kaikkien AD FS palvelimien luoma valvonta-tapahtuma

![Azure AD Connect kunto-portaalissa](./media/active-directory-aadconnect-health-adfs/report1a.png)

Tämän raportin sisällä sinulla helposti tietoja seuraavat osat:

- Total # epäonnistuneiden pyyntöjen väärä käyttäjänimellä ja salasanalla viimeisten 30 päivän aikana
- Keskimääräinen # käyttäjien, käyttäjänimi tai salasana ei kelpaa kirjautumisen päivässä epäonnistui.


Valitsemalla tämän osan Avaa Pääraportti-sivu, joka sisältää lisätiedot. Tämä sivu sisältää kaaviona tykkäysten tiedot auttaa luomaan perusaikataulun tietoja pyynnöt Väärä käyttäjänimi tai salasana. Lisäksi sen avulla ensimmäiset 50 käyttäjäluettelo epäonnistuneiden yritysten määrä.

Kaavio sisältää seuraavat tiedot:

- Epäonnistuneiden kirjautumiset vuoksi virheellinen käyttäjänimen ja salasanan kohti päivän välein määrä.
- Eri käyttäjää, joka epäonnistui kirjautumiset kohti päivän välein määrä.
- Asiakkaan IP-osoitteen viimeinen pyyntö

![Azure AD Connect kunto-portaalissa](./media/active-directory-aadconnect-health-adfs/report3a.png)

Raportissa on seuraavat tiedot:

| Raportit-kohta | Kuvaus
| ------ | -------- |
|Käyttäjätunnus| Näyttää, jota käytettiin Käyttäjätunnus. Tämä arvo on mikä käyttäjän kirjoittama joka joissakin tapauksissa on väärä käyttäjän tunnus käytössä.|
|Epäonnistuneiden yritykset| Näyttää epäonnistuneiden yritysten total #, että tiettyjä käyttäjätunnusta. Taulukko on lajiteltu kanssa eniten laskevassa järjestyksessä epäonnistuneiden yritysten määrä.|
|Viimeinen virhe| Näyttää aikaleiman, kun viimeinen virhe.
|Viimeksi virheen IP | Näyttää uusimman virheelliset pyynnöstä asiakkaan IP-osoite.|



>[AZURE.NOTE] Tämä raportti päivitetään automaattisesti tunnin välein kaksi uudet tiedot kerännyt määräajassa. Tuloksena kirjautuminen yritykset viimeisten kahden tunnin aikana ei voi sisällyttää raportin.



## <a name="related-links"></a>Aiheeseen liittyvät linkit

* [Azure AD Connect kunto](active-directory-aadconnect-health.md)
* [Azure AD Connect kunto agentti asennus](active-directory-aadconnect-health-agent-install.md)
* [Azure AD Connect kunto-toiminnot](active-directory-aadconnect-health-operations.md)
* [Azure AD yhteyden kunto käytön synkronointi](active-directory-aadconnect-health-sync.md)
* [Kuntotietojen käyttämällä Azure AD yhteydessä AD DS: ÄÄN](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect kunto usein kysytyt kysymykset](active-directory-aadconnect-health-faq.md)
* [Azure AD Connect kunto versiohistoria](active-directory-aadconnect-health-version-history.md)

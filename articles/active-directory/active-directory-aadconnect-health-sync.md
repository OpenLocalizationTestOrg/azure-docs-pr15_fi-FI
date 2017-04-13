
<properties
    pageTitle="Käyttämällä Azure AD yhteyden kunto synkronoinnin | Microsoft Azure"
    description="Tämä on Azure AD yhteyden kunto-sivulla, jotka käsittelevät seurannassa Azure AD Connect Synkronoi."
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

# <a name="using-azure-ad-connect-health-for-sync"></a>Azure AD yhteyden kunto käytön synkronointi
Seuraavat asiakirjat on Azure AD Connect (synkronointi) kanssa Azure AD yhteyden kunnon valvonta.  Lisätietoja AD FS kanssa Azure AD yhteyden kunnon valvonta artikkelissa [Käyttämällä Azure AD yhteyden kunto AD FS](active-directory-aadconnect-health-adfs.md). Lisäksi Active Directory-toimialueen palveluiden kanssa Azure AD yhteyden kunnon valvonta lisätietoja [Käyttämällä Azure AD yhteyden kunto AD DS: N kanssa](active-directory-aadconnect-health-adds.md).

![Azure AD Connect kunto-synkronointia varten](./media/active-directory-aadconnect-health-sync/sync-blade.png)

## <a name="alerts-for-azure-ad-connect-health-for-sync"></a>Ilmoitusten määrittäminen Azure AD yhteyden synkronointia varten
Azure AD yhteyden kunto ilmoituksia synkronointi-osio on aktiivinen ilmoitusten luettelo. Kunkin ilmoitus sisältää tiedot, toimintaohjeiden ja linkkejä aiheeseen liittyvät ohjeet. Aktiivinen tai ratkaistu ilmoituksen valitsemalla näet lisätietoja sekä vaiheet voit ratkaista ilmoituksen ja linkit lisäohjeita uusi sivu. Voit myös tarkastella historiatietoja ilmoituksia, jotka on poistunut menneisyydessä.

Ilmoituksen annetaan lisätietoja sekä vaiheet valitsemalla voi tehdä ratkaisemiseksi ilmoituksen ja linkit lisäohjeita.

![Azure AD Connect synkronointivirhe](./media/active-directory-aadconnect-health-sync/alert.png)

### <a name="limited-evaluation-of-alerts"></a>Ilmoitusten rajoitettu arviointi
Jos Azure AD Connect ei ole käytössä oletusarvon määrittäminen (esimerkiksi jos määrite suodattaminen muutetaan oletusarvo-määrityksestä Mukautettu määritys), valitse Azure AD yhteyden kunto-agentti ei ole lataavat virhetapahtumat, jotka liittyvät Azure AD Connect.

Tämä rajoittaa ilmoitusten arviointi palvelu. Tulee näkyviin ilmoituspalkki, joka ilmaisee ongelmaa palvelun kohdassa Azure-portaalissa.

![Azure AD Connect kunto-synkronointia varten](./media/active-directory-aadconnect-health-sync/banner.png)

Voit muuttaa valitsemalla "Asetukset" ja salliminen Azure AD yhteyden kunto-edustaja, joka lähettää kaikki virheraportit.

![Azure AD Connect kunto-synkronointia varten](./media/active-directory-aadconnect-health-sync/banner2.png)

## <a name="sync-insight"></a>Synkronoi tietoja
Järjestelmänvalvojien usein haluat lisätietoja Azure AD muutosten synkronointi ja etäisyys muuttuu meneillään kuluvaa aikaa. Tämän ominaisuuden avulla on helppo visualisointi tämä avulla kaavioiden alla:   

- Viive synkronointi-toimintoa
- Objektin muuttaminen trendi

### <a name="sync-latency"></a>Synkronoi viive
Tämä toiminto tarjoaa graafisen trendiä, viive yhdistimet synkronointi-toimintoa (tuominen, vieminen jne.).  Tämä on nopea ja helppo tapa selvittääksesi, paitsi että toiminnot (suurempi, jos sinulla on suuri joukko muutokset) viive mutta voi tunnistaa poikkeamia, joka saattaa edellyttää edelleen tutkimuksen viive.

![Synkronoi viive](./media/active-directory-aadconnect-health-sync/synclatency02.png)

Oletusarvon mukaan näkyy vain '' vientitoiminnon Azure AD-yhdistimen viive.  Haluat nähdä lisää toimintoja yhdistimen tai voit tarkastella muiden yhdistimet toimintojen kakkospainikkeella kaavion, valitse Muokkaa kaavion tai napsauttamalla "Muokkaa viive kaavion"-painiketta ja valitse tietyn toiminnon ja yhdysviivat.

### <a name="sync-object-changes"></a>Synkronoi objekti muuttuu
Tämä toiminto tarjoaa graafinen trendi muutokset, jotka ovat parhaillaan luvun arvioidaan ja Azure AD viedään.  Nykyään yritetään tämän tietojen kerääminen synkronointi lokit on vaikeaa.  Kaavion kautta, ei vain yksinkertaisempi kaikki seurannan muutosten pitkäksi aikaa ympäristössäsi määrää, mutta visuaalisen näkymän virheiden pitkäksi aikaa.

![Synkronoi viive](./media/active-directory-aadconnect-health-sync/syncobjectchanges02.png)

## <a name="object-level-synchronization-error-report-preview"></a>Objektin tason Hakemistosynkronoinnin virheraportin (ennakkoversio)
Tämä toiminto tarjoaa synkronointivirheitä, joita voi ilmetä, kun käyttäjätiedot synkronoidaan Windows Server AD ja käyttämällä Azure AD Connect Azure AD raportin.

- Raportissa käsitellään synkronointisovelluksen tallentama virheitä (Azure AD Connect versio 1.1.281.0 tai uudempi versio)
- Se sisältää sync engine viimeisin synkronointi-toiminnon tapahtuneista virheistä. ("Vie" Azure AD-yhdistimen.)
- Azure AD yhteyden kunto agent-synkronointia varten on oltava lähtevä yhteys tarvittavat päätepisteestä raportin uusimmat tiedot. Katso lisätietoja [koskevat vaatimukset-kohdassa](active-directory-aadconnect-health-agent-install.md#Requirements) .
- Raportti on **päivitetty 30 minuutin** Azure AD yhteyden kunto-agentti synkronointi ladannut tietojen avulla.
Se tarjoaa seuraavia keskeisiä ominaisuuksia

    - Virheiden luokittelu
    - Objektit, joissa on virhe kohti luokan luettelo
    - Kaikki tiedot yhdessä paikassa virheisiin
    - Puolen verrattuna objektien virheen vuoksi ristiriitoja rinnakkain
    - Lataa virheraportin muodossa CVS (tulossa)

### <a name="categorization-of-errors"></a>Virheiden luokittelu
Raportin luokittelee aiemmin synkronointivirheiden seuraavat luokat:

| Luokka | Kuvaus |
| -------------- | ----------- |
| Kaksinkertainen määrite | Virheet Azure AD Connect yrittää luominen tai päivittäminen objektien kanssa vähintään yksi määritteiden arvojen kaksoiskappaleet Azure AD, joka on oltava yksilöllinen vuokraajan, kuten proxyAddresses-UserPrincipalName. |
| Tietojen ristiriidan | Virheitä, kun Pehmeä-vastine ei vastaa objekteja, jotka aiheuttavat synkronointivirheitä. |
| Tietoja tarkistusvirhe | Virheiden vuoksi virheellisiä tietoja, kuten virheellisiä merkkejä kriittinen määritteiden UserPrincipalName, kuten muotoilla virheitä, jotka eivät läpäise tarkistusta ennen Azure AD-kirjoitetaan.|
| Suuri määrite | Virheitä, kun vähintään yksi määritteet ovat suurempia kuin sallitun koon, pituus ja määrä.|
| Muut | Kaikki muut virheet, joka ei mahdu luokista. Palautteen perusteella tähän luokkaan jaetaan sub-luokkaan.

![Synkronoinnin virheiden raportin yhteenveto](./media/active-directory-aadconnect-health-sync/errorreport01.png)
![virhe raporttiluokkien synkronointi](./media/active-directory-aadconnect-health-sync/errorreport02.png)

### <a name="list-of-objects-with-error-per-category"></a>Objektit, joissa on virhe kohti luokan luettelo
Siirtyminen kunkin luokkaan on virhe on kyseisten luokkien objektien luettelon.
![Synkronoi virhe raportin luetteloon](./media/active-directory-aadconnect-health-sync/errorreport03.png)

### <a name="error-details"></a>Virheen yksityiskohtaiset tiedot
Seuraavat tiedot ovat käytettävissä kutakin virhettä yksityiskohtaiset näkymässä

- *AD-objektin* yhdistävää tunnisteet
- Tunnisteet *Azure AD-objektin* yhdistävää (tilanteen mukaan)
- Virheen kuvaus ja niiden ratkaiseminen
- Aiheeseen liittyviä artikkeleita

![Synkronoi virheraportin tiedot](./media/active-directory-aadconnect-health-sync/errorreport04.png)

### <a name="download-the-error-report-as-csv"></a>Lataa virheraportin CSV-muodossa
Tämä ominaisuus on tulossa. Uusien päivitysten tulossa.



## <a name="related-links"></a>Aiheeseen liittyvät linkit
* [Käyttäjätietojen vianmääritys](active-directory-aadconnect-troubleshoot-sync-errors.md)
* [Kaksoiskappaleiden määrite Vikasietoisuudelle](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md)
* [Azure AD Connect kunto](active-directory-aadconnect-health.md)
* [Azure AD Connect kunto agentti asennus](active-directory-aadconnect-health-agent-install.md)
* [Azure AD Connect kunto-toiminnot](active-directory-aadconnect-health-operations.md)
* [Käyttämällä Azure AD yhteyden kunto AD FS](active-directory-aadconnect-health-adfs.md)
* [Kuntotietojen käyttämällä Azure AD yhteydessä AD DS: ÄÄN](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect kunto usein kysytyt kysymykset](active-directory-aadconnect-health-faq.md)
* [Azure AD Connect kunto versiohistoria](active-directory-aadconnect-health-version-history.md)

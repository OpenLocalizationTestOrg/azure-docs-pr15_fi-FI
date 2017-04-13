<properties
   pageTitle="Azure AD Connect synkronointi: toiminnallisia tehtäviä ja huomioon otettavia seikkoja | Microsoft Azure"
   description="Tässä ohjeaiheessa kerrotaan Azure AD Connect synkronointi ja valmistelemisesta toimivien tämän osan toiminnallisia tehtäviä."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/01/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-sync-operational-tasks-and-consideration"></a>Azure AD Connect synkronointi: toiminnallisia tehtäviä ja huomiota
Tässä ohjeaiheessa tavoitteena on kuvaus toiminnallisia tehtäviä Azure AD Connect-synkronointia varten.

## <a name="staging-mode"></a>Väliaikaisen tila
Väliaikaisen tilassa voi käyttää useita skenaarioita, mukaan lukien:

-   Suuren käytettävyyden.
-   Testaa ja ottaa käyttöön uuden määritysmuutoksia.
-   Esittele uuteen palvelimeen ja vanhan poistetaan käytöstä.

Palvelimen väliaikaisen-tilassa voit tehdä muutoksia työnkulkuun ja esikatsella muutoksia, ennen kuin teet palvelimen active. Voit myös suorittaa Täysi tuonti ja vahvista, että kaikki muutokset ovat oikein, ennen kuin olet tehnyt nämä muutokset tuotannon ympäristölle täyden käyttäjätietojen.

Asennuksen aikana voit valita palvelimen väliaikaisen **tilassa**. Tämä toiminto tekee palvelimen tuominen ja synkronointi käytössä, mutta se ei voi käyttää mitä tahansa viennin. Väliaikaisen tilassa palvelin ei ole käynnissä salasanan synkronointi tai salasana takaisinkirjoituksen, vaikka olet valinnut näiden ominaisuuksien asennuksen aikana. Väliaikaisen tilan poistaminen käytöstä, kun palvelin alkaa vieminen, mahdollistaa salasanan synkronointi ja ottaa käyttöön salasanan takaisinkirjoituksen.

Voit pakottaa vienti edelleen synkronoinnin palvelun hallinta-valintaikkunassa.

Palvelimen väliaikaisen tilassa edelleen vastaanottaa muutokset Active Directory- ja Azure AD. On aina uusimmat muutokset ja voivat nopeasti take kopion toiseen palvelimeen vastuut päälle. Jos teet määritysmuutoksia ensisijainen palvelimeen, se on muutosten samassa tilassa väliaikaisen palvelimeen.

Käyttäjille, että sinä kokemusta vanhempia synkronointi-tekniikoiden väliaikaisen tila on eri, sillä palvelin on oma SQL-tietokantaan. Tämä arkkitehtuuri avulla väliaikaisen tilassa palvelimen eri palvelinkeskuksen sijaitsee.

### <a name="verify-the-configuration-of-a-server"></a>Palvelimen määritysten tarkistaminen
Jos haluat käyttää tätä tapaa, toimi seuraavasti:

1. [Valmisteleminen](#prepare)
2. [Tuominen ja synkronoiminen](#import-and-synchronize)
3. [Tarkista](#verify)
4. [Vaihda active server](#switch-active-server)

#### <a name="prepare"></a>Valmisteleminen

1. Asenna Azure AD Connect Valitse **väliaikaisen tila**ja poistaa sen valinnan viimeiselle sivulle ohjatun asennuksen **aloittaa synkronoinnin** . Tässä tilassa voit suorittaa synkronoinnin ohjelma manuaalisesti.
![ReadyToConfigure](./media/active-directory-aadconnectsync-operations/readytoconfigure.png)
2. Kirjaudu ulos ja kirjaudu sisään ja Käynnistä-valikon Valitse **Synkronointipalvelu**.

#### <a name="import-and-synchronize"></a>Tuominen ja synkronoiminen

1. Valitse **yhdistimien**ja valitse ensimmäisen yhdistimen tyypin **Active Directory-toimialueen palveluiden**kanssa. Valitse **Suorita**, valitse **koko tuominen**ja **OK**. Toimi näiden ohjeiden kaikki tämän tyypin yhdistimet.
2. Valitse yhdistin sisältötyyppi **Azure Active Directory (Microsoft)**. Valitse **Suorita**, valitse **koko tuominen**ja **OK**.
3. Varmista, että yhdistimet-välilehti on valittuna. Kunkin yhdistimen tyyppi **Active Directory-toimialueen palveluista**Valitse **Suorita**, valitse **Delta synkronointi**ja **OK**.
4. Valitse yhdistin sisältötyyppi **Azure Active Directory (Microsoft)**. Valitse **Suorita**, valitse **Delta synkronointi**ja **OK**.

Olet nyt vaiheistettu Vie Azure AD muutoksista ja paikallisen AD (Jos käytät Exchange-yhdistelmäratkaisu). Seuraavien ohjeiden avulla voit Tarkasta, mikä on muutettava ennen kuin aloitat todella hakemistojen vieminen.

#### <a name="verify"></a>Tarkista

1. Käynnistä cmd kehote ja siirry`%ProgramFiles%\Microsoft Azure AD Sync\bin`
2. Suorita:`csexport "Name of Connector" %temp%\export.xml /f:x`  
Yhdistimen nimi löytyy synkronointipalvelu. Siinä on samankaltainen kuin "contoso.com – AAD" nimi Azure AD varten.
3. Suorita:`CSExportAnalyzer %temp%\export.xml > %temp%\export.csv`
4. % Temp % nimeltä export.csv, joka voidaan tutkia Microsoft Excel-tiedosto on. Tiedoston nimi sisältää kaikki muutokset, joita voi viedä.
5. Tee tarvittavat muutokset tietojen tai määritysten ja Suorita nämä vaiheet uudelleen (tuonti ja synkronoi ja tarkista), kunnes haluamasi muutokset, joita voi viedä ovat oikein.

**Tietoja export.csv-tiedosto**

Useimmat tiedoston on selkeitä. Jotkin lyhenteet ymmärtää sisältö:

- OMODT – objektin muokkaaminen tyyppi. Ilmaisee, onko objekti tasolla toiminnon lisääminen, päivittäminen ja poistaminen.
- AMODT – määrite muutoksen tyypin. Ilmaisee, onko toiminto määrite tasolla Lisää, Päivitä tai poista.

Jos määritearvo on moniarvoinen, kaikki muutos näkyy. Vain arvot lisätään ja poistetaan määrä on näkyvissä.

#### <a name="switch-active-server"></a>Vaihda active server

1. Aktiivisena palvelimessa Poista käytöstä server (DirSync/FIM/Azure AD-synkronointi), joten se ei vie, Azure AD tai määrittää sen väliaikaisen tila (Azure AD Connect).
2. Suorita ohjattu asennus väliaikaisen **tilassa** palvelimessa ja **väliaikaisen tilan**poistaminen käytöstä.
![ReadyToConfigure](./media/active-directory-aadconnectsync-operations/additionaltasks.png)

## <a name="disaster-recovery"></a>Tietojen palauttaminen
Käyttöönoton suunnittelun osa on liittyvää Jos ei huono, jossa menetät synkronointi-palvelimeen. On eri mallit ja mitkä niistä haluat käyttää määräytyy myös seuraavat seikat:

-   Mikä on kohdassa poikkeama, ei voi tehdä muutoksia objektien Azure AD käyttökatkot aikana?
-   Jos käytät salasanojen synkronoinnin, käyttäjille Hyväksy, että ne on käyttää Azure AD vanha salasana, jos ne muuttaa paikallisen?
-   Sinulla on riippuvuus reaaliaikainen toimintoja, kuten salasana takaisinkirjoituksen?

Vastaamalla näihin kysymyksiin ja organisaation käytäntöjen, riippuen johonkin seuraavista strategioita voidaan toteuttaa:

-   Muodosta uudelleen, kun et tarvitse.
-   On nimeltään **väliaikaisen tilassa**vara valmiustilassa palvelimen.
-   Käytä näennäiskoneiden.

Jos et käytä valmiita SQL Express-tietokanta, valitse Tarkista myös [SQL suuri käytettävyys](#sql-high-availability) -osassa.

### <a name="rebuild-when-needed"></a>Muodosta uudelleen, kun et tarvitse
Elinkykyisten strategia on palvelimen muodosta tarvittaessa suunnitteleminen. Yleensä asentaminen sync engine ja tee sitten tuontiprosessin aikana ja synkronointi voidaan suorittaa muutaman tunnin kuluessa. Jos käytettävissä ei vara palvelimeen, on mahdollista sync engine host tilapäisesti toimialueen ohjauskoneen avulla.

Sync engine palvelin ei tallentaa jonkin valtion objekteista, jotta tietokannasta muodostetaan uudelleen Active Directorysta ja Azure AD tiedoista. **SourceAnchor** -määritettä käytetään liittymään paikallisen- ja objekteja. Olemassa olevat objektit paikallisen ja pilveen palvelimeen uudelleen, valitse sync engine vastaako objektit yhdessä uudelleen-uudelleenasennuksen. Asiakirjan ja Tallenna Word on määritysten muutokset palvelimeen, kuten suodattaminen ja synkronoinnin säännöt. Mukautetun määritysten on uudelleen käyttöön, ennen kuin voit aloittaa synkronoinnin.

### <a name="have-a-spare-standby-server---staging-mode"></a>Vara valmiustilassa palvelin - väliaikaisen tila
Jos sinulla on monimutkaisempi ympäristössä, on yksi tai useampi valmiustilassa palvelin on suositeltavaa. Asennuksen aikana voit ottaa palvelin väliaikaisen **tilassa**.

Lisätietoja on artikkelissa [väliaikaisen tilassa](#staging-mode).

### <a name="use-virtual-machines"></a>Käytä näennäiskoneiden
Yleisiä ja tuettujen tapa on synkronointi-ohjelma suorittaa virtual machine. Siltä varalta, että isäntä on ongelmia, synkronointi-ohjelma-palvelimen kanssa kuva voidaan siirtää toiseen palvelimeen.

### <a name="sql-high-availability"></a>SQL-suuri käytettävyys
Jos käytössäsi ei ole SQL Server Express Azure AD Connect mukana tulevaa, valitse suuren käytettävyyden SQL Serverin myös voidaan pitää. Tuettu vain suuren käytettävyyden ratkaisu on SQL-klusterointi. Ei tueta ratkaisut lisätä peilaukseen ja aina.

## <a name="next-steps"></a>Seuraavat vaiheet

**Yleistä aiheita**  

- [Azure AD Connect synkronointi: tietoja ja mukauttaa synkronointi](active-directory-aadconnectsync-whatis.md)  
- [Azure Active Directory-integraation paikallisen käyttäjätietoja](active-directory-aadconnect.md)  

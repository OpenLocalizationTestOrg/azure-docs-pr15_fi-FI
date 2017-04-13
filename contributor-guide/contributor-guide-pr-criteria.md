# <a name="quality-criteria-for-pull-request-review"></a>Salaus puretaan pyynnön laatukriteerit tarkistaminen

Näitä ehtoja on tarkoitettu käyttäjät, jotka luovat ja ylläpitävät artikkeleja ja salaus puretaan pyynnön tarkistajien kuka tarkistaa sisällön erotettu pyynnöt. Jos salaus puretaan pyyntö ei oikeuta [Automaattinen yhdistäminen](contributor-guide-pull-request-etiquette.md#in-a-hurry-submit-prs-that-can-be-accepted-automatically)-sitä tarkastellaan ihmisten erotettu yhden tarkistajan. Salaus puretaan pyynnön tarkistajat Tarkista yleensä vain uusia tai muutettuja ominaisuudet. Salaus puretaan pyynnön tarkistajat arvioi salaus puretaan pyynnön mukaisesti eston ja estäminen laadun tarkistaminen kohteet tässä artikkelissa luetellut muutokset.

## <a name="blocking-content-quality-items"></a>Sisällön laatu kohteiden estäminen

Salaus puretaan pyynnön päivitykset on täytettävä seuraavat ehdot yhdistettävät. Salaus puretaan pyynnön tarkistajat antaa palautetta salaus puretaan pyynnön kommenteissa nämä kohteet ja tyypin `#hold-off` erotettu pyynnössä palauttaa sinulle (tekijä) palautetta.

| Luokka | Äänenlaadun tarkistaminen kohde |
|----------|---------------------|
|Edellytykset| "Valmis-,-yhdistämisen" ja "kelpoisuuden onnistui" otsikot määritetään PR.|
|Edellytykset| Yhdistä ristiriitoja ei voi estää salaus puretaan pyynnön.|
|Repo eheys|    Salaus puretaan pyyntö sisältää ei ilmeisimmät sisällön muutoksista.|
|Repo eheys|    Salaus puretaan pyyntö ei ole liitetty repo tai epätavallinen, ylimääräiset tiedostoja.|
|Repo eheys |Salaus puretaan pyyntö sisältää alle 100 muuttuneet tiedostot, ellei hinta tarkoituksella päivitetään release haaran perustyylin. (Todellisuudessa hinta pitäisi näkyä äärimmäisenä vähemmän kuin, mutta 100 muutetut tiedostot, kun GitHub ei näy diffs).|
|Nimeäminen |Uudet tiedostot tiedostonimessä noudattamalla [tiedoston nimeäminen ohjeita](file-names-and-locations.md).|
|Nimeäminen |Uusien kansioiden tuotu repo seuraa [kansion nimeämiseen liittyviä ohjeita](file-names-and-locations.md#folder-names-in-the-repo).|
|Sisältö    |Artikkeli on teknistä asiakirjaa ja näin ollen oikean sisällön kanavan. Katso [mitä otsikkotietueen ohjeet](content-channel-guidance.md).|
|Sisältö    |Teknisen asiakirjan aihe sopii tekninen artikkeli. Katso [mitä otsikkotietueen ohjeet](content-channel-guidance.md).|
|Sisältö    |Artikkelissa on johdantokappaleen ja sisällön kirjaamisesta tai käsitteellinen tekstiosaan. Artikkelissa on oltava riittävät, koko sisällön erottuvat muusta artikkelin kuin oma. Ei saa olla pienen osan tiedot. (Poikkeus: "Rajoitukset" aihe, jos se on suuri artikkeliin, jossa on luettelo kaikista palvelun rajoituksia kontekstissa.)|
|Sisältö| Elementtejä, jotka on oltava numeroitujen luetteloiden numeroidaan, elementtejä, jotka kannattaa Järjestämätön luetteloiden luettelomerkeillä. Tämä on basic käytettävyyttä.|
|Sisältö| Epätavallinen tai uusia kuvia, tietoarkkitehtuuri tai rakenteiden tai selvästi normaalista malleja on vetted liidin hinta tarkistajan kanssa. Ryhmistä, joilla on kokeileminen uusia seikkoja on oltava ongelma/ratkaisun alusta tai suunnitelma arvioinnissa kokeissa paikassa.|
|Sivuston rakenteen toiminnot| Switchers käytetään vain eri versioissa saman artikkelin siirtymistä.|
|Sivuston rakenteen toiminnot| Switchered artikkeleita otsikot sisältävät tietoja, jotka kunkin artikkelin eristää muita artikkeleita switchered määrittäminen.|
|Sivuston rakenteen toiminnot| Manuaalisesti kirjoitti TOC eivät ole sallittuja. Artikkelissa on luottavat H2s sen sivun sisällysluettelon.|
|Sivuston rakenteen toiminnot| Jos H2 otsikot ovat näkyvissä, on artikkelissa sisältää vähintään kaksi H2 otsikot. Yksi H2-otsikko-toiminnolla luo yhden kohteen artikkelissa Sisällysluettelon. H2 otsikot on käytettävä H3 otsikot varmistamiseksi Sisällysluettelon luodaan.|
|Korotuksia| HTML-koodi: Sisältölähde ei sisällä estä tasolla – vähäisiä tekstiin HTML on sallittu – kuten Yläindeksi, alaindeksi tai erikoismerkkejä, joka ei voi tehdä korotuksia vähäisiä muutakin HTML. HTML-taulukoiden sallitaan vain, jos taulukko sisältää luettelomerkityille tai numeroiduille luetteloille, mutta se ei yleensä maininta sisältö on yksinkertaistettava, joten lähde on koodattu korotuksia.|
|Korotuksia   |Mukautetun korotuksia elementtejä käytetään tarvittaessa. Ex: Muistiinpanojen on koodattu AZURE avulla. HUOMAA tiedostotunniste, ei ole tekstimuodossa.|
|HAKUKONEOPTIMOINTI    |"& #124; Microsoft Azure-sivuston tunnus tarvitaan|
|HAKUKONEOPTIMOINTI    |H1 otsikko sisältää riittävästi tietoja kuvaavat on artikkelissa erottaa sen muista Azure artikkeleita ja jotta todennäköisesti asiakkaan avainsanat sisältöä. Esimerkiksi "Yhteenveto" H1 otsikkona on virhe.|
|Sanasto| Viittaukset perinteinen ja resurssien hallinnan käyttöönotto mallit ARM-lyhenne, V1 tai V2 käyttöä on eston termejä kohde.|
|Kuvat |Animoitu GIF-tiedostoja ei hyväksytä repo kyselyjä.|
|Kuvat | Kuvien Tyhjennä tarkkuus, ovat maksuttomia väärin kirjoitetut sanat ja eivät sisällä yksityisiä tietoja | 
|Väliaikaisen|Artikkelissa esikatselu on oltava SIIVOA vaiheet. Se ei saa sisältää ilmeisimmät muotoilun ongelmat: <br><br>-Numeroidun tai luettelomerkeillä varustetun luettelon, joka näkyy kuin kappaleesta <br> -Koodi koodin tekstialueen näkyvä osittain koodilohko ja osittain ulkopuolella <br>-Numeroitu väärin vuoksi viallinen sisennys numeroidun vaiheet|

## <a name="non-blocking-content-quality-items"></a>Muut estäminen sisällön laatu kohteet

Nämä kohteet salaus puretaan pyynnön tarkistajat antaa palautetta sekä ohjeet, tekijä voi seurata myöhemmin salaus puretaan pyyntö korjaukset. Palaute estä päätös yhdistäminen. Tekijät pitäisi seurata 3 korjaukset kanssa business päivän aikana.

| Luokka | Äänenlaadun tarkistaminen kohde |
|----------|---------------------|
|Sisältö|Artikkelit pitäisi olla "vaiheisiin" 1 – 3 asiaa ja näyttävien vaiheisiin ja lopussa. Lyhyt teksti sisällytetään, joka auttaa ymmärtämään, miksi seuraavat vaiheet liittyvät asiakkaalle. (Uusia artikkeleita vain). Esimerkki: <https://azure.microsoft.com/en-us/documentation/articles/xplat-cli-install/><br>![](media/contributor-guide-pr-criteria/nextstepsexample.PNG)|
|Sisältö|Oikeinkirjoituksen ja kieliopin kirjoittaminen muut ongelmat - salaus puretaan pyynnön tarkistajat voivat antaa palautetta joitakin pieniä ongelmia vain ei-estäminen palautetta. Jos määritettynä on enemmän kuin muutaman toimituksellinen ongelmista, tarkistajat Kirjaudu jälkeinen julkaisun muokkausta varten on artikkelissa edit-pyynnön.|
|Kuvat|Kuvien käytetään oikea kuvatekstin tyyli ja väri, ja näyttökuvat oikean reunan ja paikkamerkin tyyli. [Lisätietoja kuvan ohjeissa](https://github.com/Azure/azure-content/blob/master/contributor-guide/create-images-markdown.md).|
|Kuvat|Kuvat ovat vaihtoehtoinen teksti. [Lisätietoja kuvan ohjeissa](https://github.com/Azure/azure-content/blob/master/contributor-guide/create-images-markdown.md).|
|Sivuston rakenteen toiminnot|H2 otsikot, kun sivun sisällysluetteloon rivitetään Ihannetapauksessa enintään 2 riveille. Pidempi otsikoilla saat haluamasi artikkelin Sisällysluettelon näyttävyyttä tarkistamaan.|
|Tyylin nimeämiskäytännön|Kaikki otsikot ja otsikot ovat virkkeen kirjainkoko, Azure tyylin kohden.|
|Prosessi|Jos salaus puretaan pyynnön voitu on helposti muutettu hyötyvät PRmerger automaatio-salaus puretaan pyynnön tarkistajat antaa palautetta tekijän käyttämisestä haaroja, jotta muutokset voi yhdistää automaattisesti. [On](https://github.com/Azure/azure-content/blob/master/contributor-guide/contributor-guide-pull-request-etiquette.md#in-a-hurry-submit-prs-that-can-be-accepted-automatically)artikkelissa hinta peruuttamisesta.|
|Prosessi|Kun poistaminen tai nimeäminen uudelleen artikkeliin, varmista, että voit toimia. Tuoda pyynnön tarkistajat tulee Lisää seuraava kommentti ja linkki kommentin:<br><br>*Varmista, että noudatit prosessi osallistujat oppaasta poistamiseen artikkelit: <https://github.com/Azure/azure-content/blob/master/contributor-guide/retire-or-rename-an-article.md> .*|

## <a name="related"></a>Liittyvät

- [Salaus puretaan pyynnön peruuttamisesta ja parhaita käytäntöjä Microsoft osallistujat](contributor-guide-pull-request-etiquette.md)

- [Tuoda pyynnön kommentti automaatio](contributor-guide-pull-request-comments.md)

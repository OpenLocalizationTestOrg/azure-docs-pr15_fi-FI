# <a name="azure-technical-documentation-contributor-guide"></a>Azure teknisten avustaja-opas

Olet löytänyt GitHub säilö, joka sisältää teknisiä ohjeita, jotka on julkaistu Azure asiakirjat-Centeriin numerossa [http://azure.microsoft.com/documentation](http://azure.microsoft.com/documentation)lähde.

Tämä säilö sisältää myös ohjeita voit osallistua Microsoftin teknisten.  Katso luettelo on artikkeleissa osallistujat oppaasta on [hakemiston](https://github.com/Azure/azure-content/blob/master/contributor-guide/contributor-guide-index.md).

## <a name="contribute-to-azure-documentation"></a>Jakamia Azure dokumentaatio

Kiitos kiinnostusta Azure ohjeissa.

* [Tapoja lisätä](#ways-to-contribute)
* [Koodi järjestäminen](#code-of-conduct)
* [Tietoja toimintaan Azure sisältöön](#about-your-contributions-to-azure-content)
* [Tietovaraston organisaation](#repository-organization)
* [GitHub, Git ja tämän säilöön](#use-github-git-and-this-repository)
* [Opi käyttämään korotuksia muokattava aihe](#how-to-use-markdown-to-format-your-topic)
* [Palautteen kommentit ja tuki](./contributor-guide/feedback-and-comments.md)
* [Lisää resursseja](#more-resources)
* [Indeksi kaikki osallistujat oppaan artikkeleista](./contributor-guide/contributor-guide-index.md) (Avaa uusi sivu)

## <a name="ways-to-contribute"></a>Tapoja lisätä 

Voit osallistua [Azure](http://azure.microsoft.com/documentation/) ohjeissa muutamalla eri tavalla:

* Osaltaan [keskustelupalstalla keskustelu](http://social.msdn.microsoft.com/Forums/windowsazure/home).
* Lisää artikkeleita alareunassa Disqus kommentteja.
* Voit helposti osaltaan artikkeleja GitHub käyttöliittymässä. Etsi artikkelin tätä säilöä tai Lisätietoja on artikkelissa [http://azure.microsoft.com/documentation](http://azure.microsoft.com/documentation) ja napsauttamalla linkkiä, joka siirtyy artikkelin GitHub lähde-artikkelissa.
* Jos teet merkittäviä muutoksia aiemmin artikkeleihin, lisääminen tai muuttaminen kuvia tai osallistuvat uuden artikkelin haluat haarautuvat tämän säilöön, asenna Git Bash korotuksia Valintapaneeli ja oppia git jotkut komennot.

##<a name="code-of-conduct"></a>Koodi järjestäminen

Tämä projekti on antanut [Microsoft Avaa lähde niihin](https://opensource.microsoft.com/codeofconduct/). Katso lisätietoja [Järjestä FAQ koodi](https://opensource.microsoft.com/codeofconduct/faq/) tai yhteyshenkilön [opencode@microsoft.com](mailto:opencode@microsoft.com) mitään muita kysymyksiä tai kommentteja.

##<a name="about-your-contributions-to-azure-content"></a>Tietoja toimintaan Azure sisältöön

###<a name="minor-corrections"></a>Vähäisiä korjaukset

Vähäisiä korjauksien tai Lähetä tämä repo-asiakirjat ja koodin esimerkkejä selvennykset jäävät [Azure sivuston käyttöehdot (käyttöehdot)](http://azure.microsoft.com/support/legal/website-terms-of-use/).


###<a name="larger-submissions"></a>Suurempi lähetysten

Jos olet lähettänyt uuden tai merkittäviä muutoksia asiakirjoista ja koodiesimerkkejä salaus puretaan pyyntö, lähetämme kommentti-GitHub sinulta kysytään, haluatko lähettää online osuus käyttöoikeuden sopimuksen (CLA), jos sinulla on jokin näistä ryhmistä:

* Avaa Microsoft-tekniikoiden-ryhmän jäsenet.
* Osallistujat, jotka eivät toimi Microsoft.

Tarvitsemme suorittaminen online-lomake, ennen kuin emme voi hyväksyä salaus puretaan puhelinnumeroon.

Tarkat tiedot ovat käytettävissä osoitteessa [http://azure.github.io/guidelines/#cla](http://azure.github.io/guidelines/#cla).

## <a name="repository-organization"></a>Tietovaraston organisaation

Azure-sisältö-säilössä sisällön seuraa dokumentaatio organisaation [Azure.Microsoft.com](http://azure.microsoft.com). Tämä säilö sisältää kaksi pääkansion kansiot:

### <a name="articles"></a>\articles

*\Articles* -kansio on muotoiltu korotuksia tiedostot, joiden tunniste on *.md* ohjeissa on artikkeleissa.

Azure.Microsoft.com julkaistaan artikkeleita pääkansion polun *http://azure.microsoft.com/documentation/articles/ {artikkelin-nimi-ilman-md} /*.

* **Tiedostonimet artikkeli:** Lisätietoja [Microsoftin tiedostossa nimeäminen ohjeita](./contributor-guide/file-names-and-locations.md).

Oman service-kansiosta käsin artikkeleita julkaistaan Azure.Microsoft.com polussa *http://azure.microsoft.com/documentation/articles/service-folder/ {artikkelissa-nimi-ilman-md} /*

* **Media alikansiot:** *\Articles* kansio sisältää *\media* kansion pääkansion directory artikkelissa mediatiedostojen sisällä on alikansioita, joissa kunkin artikkelin kuvat.  Palvelun kansiot sisältävät eri media-kansio artikkeleita kunkin palvelun-kansiosta käsin. Artikkelin kuva kansiot nimetään samannimisen artikkelissa-tiedostoon, miinus *.md* tunniste.

### <a name="includes"></a>\Includes

Voit luoda Uudelleenkäytettävän sisällön osia sisällytetään vähintään yksi artikkelit. Katso [Mukautetut tunnisteet käytetään Microsoftin tekninen sisältöä](./contributor-guide/custom-markdown-extensions.md).

### <a name="markdown-templates"></a>\markdown mallit

Tämä kansio sisältää Microsoftin vakio korotuksia mallin basic korotuksia muotoilua, sinun on artikkeliin.

### <a name="contributor-guide"></a>\contributor-Guide

Tämä kansio sisältää artikkeleita, jotka ovat osa Microsoftin osallistujien opas.  

## <a name="use-github-git-and-this-repository"></a>GitHub, Git ja tämän säilöön

Lisätietoja voit osallistua GitHub-Käyttöliittymän avulla voit osallistua pieniä muutoksia ja haarautuvat ja Kloonaa Lisää merkittävästi säilö on [asentaminen ja määrittäminen työkaluja yhteiskäyttö GitHub](./contributor-guide/tools-and-setup.md).

Jos asennat GitBash ja valitse voit työskennellä, [Git komennot uuden artikkelin luominen tai päivittäminen aiemmin artikkelissa](./contributor-guide/git-commands-for-master.md) on lueteltu luodaan uusi paikallinen toimimasta haaran, muutosten ja lähettämistä muutokset takaisin tärkeimmät haara

### <a name="branches"></a>Haaroja

On suositeltavaa luoda paikallisen toimimasta haaroja, joka kohdistaa muutoksen tiettyyn alueeseen. Haaroja olisi rajoitettava yksittäinen käsite/artikkelin sekä virtaviivaistaa työnkulku yhdistämisen ristiriitojen mahdollisuutta.  Uusi haara tarvittavat vaikutusalueen on seuraavia toimia:

* Uuden artikkelin (ja liittyvät kuvat)
* Oikeinkirjoituksen ja kieliopin tarkistaminen muokkaukset artikkeliin.
* Käyttää yhden muutoksen lukuisten artikkeleita (esimerkiksi uuden tekijänoikeuksien alatunniste) kautta.

## <a name="how-to-use-markdown-to-format-your-topic"></a>Opi käyttämään korotuksia muokattava aihe

Kaikki tämän säilöön artikkelit käyttää GitHub flavored korotuksia.  Seuraavassa on luettelo resursseista.

- [Korotuksia perusteet](https://help.github.com/articles/markdown-basics/)

- [Tulostettavan korotuksia cheatsheet](./contributor-guide/media/documents/markdown-cheatsheet.pdf?raw=true)

- Luettelossamme korotuksia editors on [Työkalut ja asetukset-aihetta](./contributor-guide/tools-and-setup.md#install-a-markdown-editor).

## <a name="article-metadata"></a>Artikkelissa metatiedot

Artikkelissa metatietojen avulla tiettyjä azure.microsoft.com-sivuston toimintoja, kuten tekijä attribution, avustaja attribution, navigointipolku, artikkelissa kuvaukset ja Hakukoneoptimoinnin optimointi sekä reporting Microsoft käyttää arvioida sisällön. Tällöin metatiedot on tärkeää! [Seuraavassa on ohjeita, jolla varmistetaan, että metatietojen tehdään oikealle](./contributor-guide/article-metadata.md).

### <a name="labels"></a>Otsikot

Automaattinen otsikot on määritetty hakemaan pyynnöt Auta meitä salaus puretaan pyyntö-työnkulun hallinta ja ilmoittamaan, mistä on kyse salaus puretaan pyynnön kanssa:

* Liittyvät osuus käyttöoikeussopimus
    * CLA ei: muutos on suhteellisen pieniä ja ei edellytä, että kirjaudut CLA.
    * CLA pakollinen: Muuta laajuus on suhteellisen suuri ja edellyttää, että kirjaudut CLA.
    * CLA kirjautunut: osallistuja kirjautunut CLA, jotta salaus puretaan pyynnön nyt siirtää eteenpäin tarkistettavaksi.
* Pilari otsikot: otsikot, kuten PnP, Nykyaikainen sovellukset ja TDC auttaa luokittelevat salaus puretaan pyynnöt sisäinen organisaatio on tarkistettava salaus puretaan pyynnön.
* Muuta lähetetään tekijä: tekijä on ilmoitettu odotetaan erotettu pyynnön.

## <a name="more-resources"></a>Lisää resursseja

Katso [indeksi Microsoftin avustaja-oppaan](./contributor-guide/contributor-guide-index.md) sekä ohjeet-ohjeita.

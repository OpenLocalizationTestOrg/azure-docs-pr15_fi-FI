# <a name="pull-request-etiquette-and-best-practices-for-microsoft-contributors-to-azure-documentation"></a>Tuoda pyynnön peruuttamisesta ja parhaita käytäntöjä Microsoft Azure dokumentaatio osallistujien varten

Muutosten julkaiseminen asiakirjat, Lähetä salaus puretaan pyynnöt-että rinnakkainen. Jokainen salaus puretaan pyyntö on tarkistetaan ennen yhdistetään. Tässä artikkelissa selvittääksesi, miten pitäisi toimia salaus puretaan pyynnön tarkastajien kanssa ja kuinka voit luoda salaus puretaan pyynnöt, joka on helppoa ja nopeaa tarkistettava, jotta salaus puretaan pyynnön jonossa kokousaika sopii kaikille paremmin.

## <a name="working-with-pull-request-reviewers"></a>Salaus puretaan pyynnön tarkistajat käsitteleminen

Seuraavassa on sinun tarvitsee tietää käyttämisestä salaus puretaan pyynnön tarkistajat perusteet. 

- <b>Tietoja salaus puretaan pyynnön tarkistajan rooli. Tarkistaja:</b>
  - Varmistaa sisällön basic laatu
  - Estää säilössä muutoksista
  - Antaa palautetta ennen yhdistämistä

  Salaus puretaan pyynnön tarkistajat ovat sisällön hallinnon rooli. Ensisijainen palveluita on ei, jos haluat yhdistää vain riippumatta siitä, mitä lähetetään mahdollisimman pian. Toiminta, jotka edellyttävät tehdä päivityksiä, erityisesti uusien ja raskaasti muokattu artikkeleissa palautteen.

- <b>Suunnittele salaus puretaan-pyyntö tarkistajan kanssa:</b>
  - Tärkeäksi erotettu pyyntöjen
  - Salaus puretaan tarjouspyyntöjen aikakatkaistu/päivätty versioista
  - Salaus puretaan pyynnöt, muuta tai Lisää kuitata tiedostoja

- <b>Salaus puretaan SLA pyytää tarkastelu</b>

  Yksityinen säilössä aina, kun salaus puretaan pyyntö Lisää salaus puretaan pyynnön jonossa valmis Yhdistä-otsikko ryhmän yrittää tarkastella salaus puretaan pyynnön sisällä 12 aukioloajat (M-F, 8 AM 5 PM) ja antaa palautetta tai Yhdistä ei ole palautetta tarvitaanko. Tämä SLA koskee n tarkistaminen arvo ei voi yhdistää siihen. Laatuviiniä yhdistetään täyttävät [määrittämäsi ehdot yhdistämistä varten](contributor-guide-pr-criteria.md). 

## <a name="make-the-pull-request-queue-work-better-for-everyone"></a>Varmista, että salaus puretaan pyynnön jonossa työstämistä kaikille

Hinta-jonossa on kaksi basic vaatimuksia:

- Salaus puretaan pyynnöt, jotka ovat pieniä laajuus ja, jotka sisältävät hyvin samankaltaisia kuin muutokset kestää vähemmän aikaa, kun haluat tarkastella. 
- Salaus puretaan pyynnöt, jotka ovat suuren alueen tai, joka on yhdistelmä erilaisia muutoksia, jotka sisältävät viedä enemmän aikaa, voit tarkastella.

Voit parantaa salaus puretaan pyynnön jonossa työstämistä noudattamalla näitä parhaita käytäntöjä:

- Erillinen vähäisiä päivitykset aiemmin artikkeleihin uusia artikkeleita tai pää kirjoittaa uudelleen. Käyttää erillisen toimimasta haaroja nämä muutokset. 

- Kun poistat artikkeleita tai kuvia, älä käytä poistot uudet sisällön lisäykset tai päivitykset. Käsittele erillisessä toimimasta haaran muutokset ja uusi sisältö.

- Versioiden tai sisällön optimointi Suunnittele hinta tarkistajan kanssa. Voit joutua yhteystietoluetteloonsa ohjeen avulla voit luoda release sivuliikkeen tai järjestämiseen julkaisun kertaa yhdistämisen kellonajan, jotta sisältö on julkaistu oikeaan aikaan.

- Jos yrität järjestämiseen päivitykset tehdyt muutokset, jotka teet azure-sisältö-arvo-säilössä ACOM repo (ie-vasemmassa siirtymisruudussa purkamisen sivuja, Uudelleenohjaa tai learning kartat muutokset), toimivat etuajassa on sointuvat hinta tarkistaja. Muussa tapauksessa voit tuota on paljon katkenneista linkeistä.

## <a name="criteria-for-expedited-pull-requests"></a>Toimitettu erotettu pyynnöt perusteet

- Ota azdocprs suorittamiseen laatuviiniä vain silloin, kun se on välttämätöntä. Voit pyytää toimitettu käsittelyn punainen vyöhykkeen, tietosuoja, legal ja tietoturva-asioita; hinta Saat todella katkennut asiakkaan kokemukset; ja johdon siirtäminen asiantuntijalle. 
- Sisällön ominaisuus julkaisuissa ei oikeuta nopeutettu käsittely - ominaisuus release sisältö vaatii edellisen suunnittelu tai se on käsiteltävä vakio prioriteetti-jonossa kautta.


## <a name="in-a-hurry-submit-prs-that-can-be-accepted-automatically"></a>Osaa? Lähetä laatuviiniä, joka voi hyväksyä automaattisesti

PRMerger automaatio sääntöjen avulla voit enemmän oman päivittäinen laatuviiniä yhdistetään automaattisesti.

PRMerger voivat hyväksyä oman hinta automaattisesti, jos:
* Se vaikuttaa 10 tiedostoja tai vähemmän.
* Siinä on 15 tapahtumasarjan vahvistukset tai vähemmän.
* Pienempi kuin 20 prosenttia tekstin muutoksia.
* Valitsimien/switchers eivät päivity.
* Tiedostoja ei ole poistettu tai lisätä.
* Ei ole kuvat ovat uusia, muuttaa tai poistaa.

Jos salaus puretaan pyyntö ei täytä näitä ehtoja, "PRmerger ei voi yhdistää"-otsikko määritetään automaattisesti niin, että se vaatii Tarkista ihmisten hinta tarkistajan.

### <a name="need-to-make-a-lot-of-little-changes"></a>Ei tarvitse tehdä paljon pieniä muutoksia?

Ota yhteyttä pinon PRMerger automaatio sääntöjen yllä ja toimi seuraavasti:
* Lähettää artikkelit Vaalea muutokset yhteen hinta 10 tai vähemmän tiedostoja.
* Luo erillisen hinta artikkeleita kuvia tai valitsimien muutos. Tämä edellyttää ihmisten Tarkista.
* Luo uusi tai poistetut artikkeleita erillisessä hinta. Tämä edellyttää ihmisten Tarkista.

## <a name="related"></a>Liittyvät

- [Salaus puretaan pyynnön laatukriteerit tarkistaminen](contributor-guide-pr-criteria.md)

- [Tuoda pyynnön kommentti automaatio](contributor-guide-pull-request-comments.md)

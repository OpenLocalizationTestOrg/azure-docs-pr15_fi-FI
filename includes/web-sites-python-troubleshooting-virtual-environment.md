Käyttöönoton komentosarja Ohita näennäisen ympäristön luominen Azure-, jos se havaitsee, että yhteensopiva virtuaalisen ympäristön on jo olemassa.  Näin voit nopeuttaa käyttöönoton huomattavasti.  Pip ohittaa paketteja, jotka on jo asennettu.

Joissakin tilanteissa haluat ehkä pakottaa poistaminen näennäisen-ympäristön.  Haluat toiminto, jos haluat jättää virtuaalisen ympäristön lisääminen säilöön.  Voit halutessasi myös toiminto, jos sinun on tiettyjä pakettien vahingossa tai testata requirements.txt muutokset.

Voit hallita aiemmin näennäisen ympäristön Azure-muutamia vaihtoehtoja:

### <a name="option-1-use-ftp"></a>Vaihtoehto 1: Käytä FTP

Muodostaa yhteyttä palvelimeen FTP-asiakas, ja voit tarvittaessa Poista kirjekuori-kansio.  Huomaa, että jotkin FTP-asiakkaat (kuten selaimet) voi olla vain luku- ja ei salli poistamisen kansioita, joten varmista, että FTP-asiakas käyttää kyseistä ominaisuutta haluat.  FTP-isäntänimi ja käyttäjän näkyvät [Azure Portal](https://portal.azure.com)-web-sovelluksen sivu.

### <a name="option-2-toggle-runtime"></a>Vaihtoehto 2: Näytä tai piilota runtime

Seuraavassa on vaihtoehto, joka hyödyntää siitä, että käyttöönoton komentosarja Poista kirjekuori-kansio, jos se ei vastaa Python haluamasi versio.  Tämä tehokkaasti Poista käytössä-ympäristö, ja luoda uuden.

1. Siirry eri Python-versio (joko runtime.txt tai Azure-portaalissa **Sovelluksen asetukset** -sivu)
1. git push joitakin muutoksia (Ohita pip Asenna virheitä, jos minkä tahansa)
1. Siirry takaisin Python alkuperäinen versio
1. git push muutoksia uudelleen

### <a name="option-3-customize-deployment-script"></a>Vaihtoehto 3: Mukauttaminen käyttöönoton komentosarja

Jos olet mukauttanut käyttöönoton komentosarja, voit muuttaa koodin deploy.cmd pakottaa sen kirjekuori-kansion poistaminen.

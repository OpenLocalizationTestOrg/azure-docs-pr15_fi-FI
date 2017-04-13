Azure määrittää, että sovelluksesi käyttää Python **Jos molemmat ehdot täyttyvät**:

- Requirements.txt tiedoston pääkansio
- pääkansio .py minkä tahansa tiedoston tai runtime.txt, joka määrittää python

Jos näin on, se käyttää Python tietyn käyttöönoton komentosarja, joka suorittaa vakio synkronoinnin tiedostoja sekä Python lisätoiminnot, kuten:

- Näennäisen ympäristön automaattinen hallinta
- Käyttämällä pip requirements.txt luetellut pakettien asennus
- Valitun Python version perusteella tarvittavat web.config luominen.
- Kerää Django sovelluksen staattinen tiedostot

Voit valita tiettyjä ominaisuuksia oletusarvon käyttöönoton ohjeita eikä sinun tarvitse mukauttaa komentosarja.

Jos haluat ohittaa kaikki Python käyttöönotto vaiheet, voit luoda tyhjän tiedoston:

    \.skipPythonDeployment

Jos haluat hallita käyttöönoton, voit ohittaa oletusarvon käyttöönoton komentosarja luomalla seuraavat tiedostot:

    \.deployment
    \deploy.cmd

Voit luoda tiedostot [Azure käyttöliittymä][] .  Tällä komennolla projektin kansiosta:

    azure site deploymentscript --python

Kun nämä tiedostot eivät ole, Azure luo tilapäinen käyttöönoton komentosarja ja suorittaa sen.  On sama kuin se, voit luoda yllä-komennolla.

[Azure käyttöliittymä]: http://azure.microsoft.com/downloads/

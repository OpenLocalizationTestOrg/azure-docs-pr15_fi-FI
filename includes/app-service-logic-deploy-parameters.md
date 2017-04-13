Azure resurssien hallinnan avulla voit määrittää parametrien arvot, jotka haluat määrittää, kun malli otetaan käyttöön. Malli sisältää osan nimeltä parametrit, joka sisältää kaikki parametriarvot.
Olisi määrittää kyseiset arvot, vaihtelevat otettaessa projektin pohjalta tai otat käyttöön ympäristöön parametri. Määritä parametrien arvot, jotka aina säilyy ennallaan. Kunkin parametriarvo käytetään mallin määrittäminen resurssit, jotka ovat käyttöönotto. 

Kun määrität parametreja, mitkä arvot käyttöönoton aikana voit antaa käyttäjän määrittää **allowedValues** -kentän avulla. **Oletusarvo** -kentän avulla voit määrittää arvon parametri, jos arvoa ei ole annettu käyttöönoton aikana.

Olemme kuvataan kunkin parametrin mallia.

### <a name="logicappname"></a>logicAppName

Voit luoda logiikan-sovelluksen nimeä.

    "logicAppName": {
        "type": "string"
    }
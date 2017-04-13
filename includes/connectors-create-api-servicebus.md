### <a name="prerequisites"></a>Edellytykset

Sinulla on oltava [Palvelun Bus](https://azure.microsoft.com/services/service-bus/) -tili.  

Voit käyttää Azure palvelun Bus tilisi logiikan-sovelluksessa, on sallittava muodostaa bus palvelutilin logiikan-sovellus. Lähettäjä voit tehdä sen helposti käyttämällä sisällä logiikan sovelluksen Azure-portaalissa.  

Näin voit muodostaa yhteyden tiliisi palvelun Bus logiikan sovelluksen sallivat:  

1. Yhteyden muodostaminen palvelun Bus logiikan sovelluksen suunnittelussa, valitse avattavasta luettelosta **Näytä Microsoft hallitun ohjelmointirajapinnan** . Kirjoita **palvelun bus** hakuruutuun. Valitse käynnistin tai toiminnon, jota haluat käyttää.  
    ![Palvelun Bus yhteyden kuva 1](./media/connectors-create-api-servicebus/servicebus-1.png)  

2. Jos et ole luonut palvelun Bus ennen kuin kaikki yhteydet, sinua pyydetään antamaan palvelun Bus tunnistetiedot. Nämä käytetään sallivat logiikan sovelluksen yhdistäminen ja käsitellä palvelun Bus-tilisi tiedot. Palvelun Bus yhdistimen tarvitsee yhteysmerkkijonon palvelun Bus nimitilan. Käyttöoikeuksien **hallinta** edellyttää myös. Jos yhteysmerkkijono on nimitilan tai tietyn kohteen on, jos se on hyvä tapa `EntityPath` parametria. Jos näin on, se ei ole oikealle yhteysmerkkijonon logiikan-sovelluksen.  
    ![Palvelun Bus yhteysmerkkijonon](./media/connectors-create-api-servicebus/connectionstring.png)

1. Kun olet saanut yhteysmerkkijonon nimitilan, voit käyttää API yhteyden logiikan-sovelluksissa.  
    ![Palvelun Bus yhteyden kuva 2](./media/connectors-create-api-servicebus/servicebus-2.png)  

3. Huomaa, yhteys on luotu ja voit vapaasti nyt jatkaa logiikan sovelluksen muita ohjeita.  
    ![Palvelun Bus yhteyden kuva 3](./media/connectors-create-api-servicebus/servicebus-3.png)   

Lisääminen käynnistintä.

1. *Sftp* kirjoittaa hakukenttään logiikan sovellusten suunnittelu ja valitse sitten **SFTP - tiedoston lisännyt tai muokannut** käynnistin   
![SFTP käynnistin kuva 1](./media/connectors-create-api-sftp/trigger-1.png)  
- **Kun tiedosto lisätään tai muokata** ohjausobjektin avautuu  
![SFTP käynnistin kuva 2](./media/connectors-create-api-sftp/trigger-2.png)  
- Valitse **...** sijaitsevat ohjausobjektin oikeassa reunassa. Tämä avaa kansion valitsin-ohjausobjekti  
![SFTP käynnistin kuva 3](./media/connectors-create-api-sftp/action-1.png)  
- Valitse Valitse pääkansion uusi tai muokattu tiedostojen seurannassa kansiona **SFTP** . Ilmoitus pääkansion näkyy nyt **kansion** -ohjausobjekti.  
![SFTP käynnistin kuva 4](./media/connectors-create-api-sftp/action-2.png)   

Tässä vaiheessa logiikan-sovellus on määritetty käynnistimen, joka alkaa Suorita käynnistimien ja työnkulku, kun tiedosto on muokattu tai luotu SFTP haluamaasi kansioon. 

>[AZURE.NOTE]Logiikan sovelluksen toimivan sen on oltava vähintään yksi käynnistin ja yksi toiminto. Noudattamalla voit lisätä toiminnon seuraavan osion ohjeita.  
#### <a name="prerequisites"></a>Edellytykset
- Azure tilin; Voit luoda [ilmaisen tilin](https://azure.microsoft.com/free)
- [Dynamics CRM Online-](https://www.microsoft.com/en-us/dynamics/crm-free-trial-overview.aspx) tili 

Ennen kuin käytät tiliäsi Dynamics logiikan-sovelluksessa, Määritä logiikan sovelluksen CRM Online tiliin yhdistämistä varten. Voit tehdä tämän helposti sisällä logiikan sovelluksen Azure-portaalissa. 

Määritä logiikan sovelluksen muodostaa CRM Online-tilisi seuraavasti:

1. Logiikan sovelluksen luominen. Logiikan sovellusten suunnittelussa Valitse **Näytä Microsoft hallitun ohjelmointirajapinnan** avattavasta luettelosta ja Kirjoita hakukenttään "dynamics". Valitse jokin käynnistimet tai toiminnot:  
  ![](./media/connectors-create-api-crmonline/dynamics-triggers.png)
2. Jos et ole aiemmin luonut mahdolliset yhteydet Dynamics, sinua kehotetaan kirjautumaan Dynamics tunnistetiedoilla:  
  ![](./media/connectors-create-api-crmonline/dynamics-signin.png)
3. Valitse **Kirjaudu sisään**ja Anna käyttäjänimi ja salasana. Valitse **Kirjaudu sisään**. 

    Nämä käytetään sallivat logiikan sovelluksen muodostamaan yhteyden ja käyttämään Dynamics-tilisi tiedot. 
4. Huomaa, yhteys on luotu. Nyt jatkaa logiikan sovelluksen muita ohjeita:  
  ![](./media/connectors-create-api-crmonline/dynamics-properties.png)

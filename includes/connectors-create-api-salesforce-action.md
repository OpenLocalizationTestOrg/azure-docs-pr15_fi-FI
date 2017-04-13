Nyt kun olet lisännyt ehdon, sen aika tekemään tiettyjä toimenpiteitä kiinnostavat tietoja, jotka luodaan käynnistin. **Salesforce - Get-objekti** -toiminnon lisääminen seuraavasti. Tämä toiminto saavat tiedot aina, kun uusi liidi luodaan. Toisen toiminnon, jota käyttävät Salesforce - Hae objektin toiminta ja Lähetä sähköpostiviesti käyttämällä Office 365-yhdistin tiedot myös Lisää.  

Voit määrittää tämän toiminnon, sinun on annettava seuraavat tiedot. Huomaat, että se on helppokäyttöinen luomat käynnistimen syötteenä-joillekin uuden tiedoston ominaisuuksien tiedot:

|Tiedoston ominaisuuden luominen|Kuvaus|
|---|---|
|Objektityyppi|Tämä on kiinnostavan Salesforce-objektin laji. Esimerkkejä liidin, tilin jne.|
|Objektitunnus|Tämä vastaa objektin tunnus.|


1. Valitse **Lisää toiminnon** linkki. Avaa tämä hakuruutu, jossa voit hakea toiminnon voit haluat tehdä. Tässä esimerkissä Salesforce-toiminnot ovat kiinnostuksen kohde.      
![Salesforce-toiminnon kuva 1](./media/connectors-create-api-salesforce/action-1.png)  
- Kirjoita *salesforce* Etsi salesforce liittyviä toimintoja.
- Valitse toiminto, joka suoritetaan **Salesforce - Get-objekti** .   **Huomautus**: sinua pyydetään vahvistamaan logiikan sovelluksen Salesforce-tilisi käyttää, jos et ole vielä tehnyt sitä aiemmin.    
![Salesforce-toiminnon kuva 2](./media/connectors-create-api-salesforce/action-2.png)    
- **Hae objekti** ohjausobjektissa avautuu.  
- Valitse Objektityyppi *johtaa* .
- Valitse ohjausobjekti, **Objektitunnus** .
- Valitse **...** tunnuksia, jotka voidaan syötteeksi toimintojen luettelo.       
![Salesforce-toiminnon kuva 3](./media/connectors-create-api-salesforce/action-3.png)    
- Valitse **Liidin tunnus** ohjausobjektin avautuu.   
![Salesforce-toiminnon kuva 4](./media/connectors-create-api-salesforce/action-4.png)     
- Huomaa, että johtaa tunnus on nyt Objektitunnus ohjausobjektin, joka ilmoittaa, että Get-objektin toiminta etsii liidistä, jonka tunnus on yhtä kuin liidin, joka on käynnistänyt sovelluksen logiikan liidin tunnus.  
![Salesforce-toiminnon kuva 5](./media/connectors-create-api-salesforce/action-5.png)  
- Tallenna työsi. Siinä, olet lisännyt Get-objektin toiminta logiikan-sovellukseen. Get-ohjausobjekti pitäisi näyttää tältä:    
![Salesforce-toiminnon kuva 6](./media/connectors-create-api-salesforce/action-6.png)  

Nyt kun olet lisännyt toiminnon saat liidistä, haluat ehkä tekemään tiettyjä toimenpiteitä, kiinnostavia juuri luomasi liidin kanssa. Yritys haluat ehkä lähettää sähköpostia jakeluluettelon ilmoittaa, että uusi liidi on luotu. Oletetaan, että lähettää sähköpostia osittain tärkeät tiedot uuteen liidin objektista Salesforce-Office 365-Connectorin avulla.  

1. Valitse **Lisää toiminnon** Kirjoita *sähköpostin* haku-ohjausobjektissa. Tämä suodattaa niitä, jotka liittyvät sähköpostin lähettäminen ja vastaanottaminen toiminnoista.  
- Valitse **Office 365: n Outlook - lähettää sähköpostia** luettelokohde. Jos et ole jo luonut *yhteyden* Office 365-tiliä, voit pyydetään Office 365-tunnistetietosi, luo se nyt. Kun olet valmis, **Lähetä sähköpostiviesti** -ohjausobjekti avautuu.        
![Salesforce-toiminnon kuva 7](./media/connectors-create-api-salesforce/action-7.png)  
- Kirjoita sähköpostiosoite, jotka haluat lähettänyt sähköpostia **,** ohjausobjektin.
-  Anna *Uusi liidi luotu* - **Aihe** -kohtaa ja valitse *yritys* -tunnuksen. Tässä näkyvät Salesforce luodaan uusi liidistä *yritys* -kenttä.  
-  Voit valita minkä tahansa paikkamerkkejä objektista liidin **leipäteksti** -ohjausobjekti, ja voit myös kirjoittaa jostakin tekstin haluat näkyvän sähköpostiviestin teksti. Tässä on esimerkki:  
![Salesforce-toiminnon kuva 8](./media/connectors-create-api-salesforce/action-8.png)   
- Tallenna työnkulku.  

Se on. Logiikan-sovellus on nyt valmis.  

Nyt voit testata logiikan-sovellus: Salesforce-Luo uusi liidi, joka vastaa ehtoa, jonka loit.  Jos olet toiminut tämän hallintapaketteihin täysin, Luo liidi vain sähköpostiosoite, joka sisältää sen *amazon.com* kanssa. Muutaman sekunnin kuluttua logiikan sovelluksen saatu ja tulokset voivat näyttää seuraavanlaiselta:  
![Salesforce-toiminnon kuva 9](./media/connectors-create-api-salesforce/action-9.png)  


Nyt kun olet lisännyt käynnistintä, sen aika tekemään tiettyjä toimenpiteitä kiinnostavia tietoja, jotka luodaan käynnistintä. Noudattamalla seuraavia ohjeita voit lisätä **SFTP - Pura kansio** -toiminto. Tämä toiminto purkaa tiedoston sisällön, jos määritetyt ehdot täyttyvät. 

Voit määrittää tämän toiminnon, sinun on annettava seuraavat tiedot. Huomaat, että se on helppokäyttöinen luomat käynnistimen syötteenä joistakin uuden tiedoston ominaisuuksien tiedot:

|SFTP - Pura kansio-ominaisuus|Kuvaus|
|---|---|
|Lähde-arkisto tiedostopolku|Tämä on parhaillaan purkaa tiedoston polku. Voit valita yhden merkinnän aiemmissa toiminto tai SFTP palvelimen etsiminen tiedostopolku Selaa.|
|Kohdekansion polku|Tämä on poimitun tiedostot sisältävään polku. Voit valita yhden merkinnän aiemmissa toiminnon kohde polku tai Selaa SFTP palvelin ja valitse polku.|
|Korvaa?|Ilmaisee, jos tiedosto on sama nimi kuin purettu tiedosto löydy kohdekansion polku, jos nykyinen tiedosto voi korvata, vai ei.|

Aloitetaan lisääminen toiminto, joka purkaa tiedostot, jos aiemmin määritetty ehto on *Tosi*. 

1. Valitse **Lisää toiminnon**.        
![SFTP toiminnon ehto kuva 6](./media/connectors-create-api-sftp/condition-6.png)   
- Valitse **SFTP - Pura kansio** -toiminto      
![SFTP toiminnon ehto kuva 7](./media/connectors-create-api-sftp/condition-7.png)   
- Valitse **tietolähteen arkisto tiedostopolku**              
![SFTP toiminnon ehtoon kuva 9](./media/connectors-create-api-sftp/condition-9.png)   
- Valitse **polku** -tunnuksen. Tämä ilmaisee, että käytät käynnistimen löydy olevan tiedoston tiedostopolku kuin lähteen arkisto tiedostopolku.           
![SFTP toiminnon ehtoon kuva 10](./media/connectors-create-api-sftp/condition-10.png)   
- Valitse **kohdekansion polku**           
![SFTP toiminnon ehtoon kuva 11](./media/connectors-create-api-sftp/condition-11.png)   
- Valitse **polku** -tunnuksen. Tämä ilmaisee, että käytät käynnistin löydy olevan tiedoston tiedostopolku kohde polku, poimitun tiedostojen.   
- Kirjoita *\ExtractedFile* **kohdekansion polku** -ohjausobjektiin. Tee tämä vain kohde kansion polku ohjausobjektin tiedoston polku-tunnuksen jälkeen.         
![SFTP toiminnon ehto kuva 12](./media/connectors-create-api-sftp/condition-12.png)   
- Kirjoita *Tosi* -**Korvaa?* ohjausobjektin osoittamaan aiemmin luotujen tiedostojen pitäisi voi korvata, jos ne on sama nimi kuin poimitun tiedostot.      
![SFTP toiminnon ehto kuva 13](./media/connectors-create-api-sftp/condition-13.png)   
- Tallenna muutokset työnkulkuun  

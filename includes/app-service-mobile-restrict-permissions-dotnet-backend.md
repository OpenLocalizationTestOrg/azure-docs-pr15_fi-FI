
Oletusarvon mukaan Mobile-sovelluksen taustassa API voidaan kutsua anonyymisti. Seuraavaksi sinun täytyy rajoittaa vain todennetut asiakkaille.  

+ **Node.js Taustajärjestelmä (joko portaalin)** :  
    
    Valitse Mobile-sovelluksen **asetukset**- **Helposti taulukot** ja valitse taulukko. Valitse **Muuta käyttöoikeuksia**, valitse **todennettu käyttöön vain** kaikki käyttöoikeudet ja valitse sitten **Tallenna**. 

+ **.NET Taustajärjestelmä (C#)**:  

    Siirry server-projektin **ohjaimet** > **TodoItemController.cs**. Lisää `[Authorize]` määrite **TodoItemController** -luokka seuraavasti. Käytön rajoittaminen vain tiettyjä menetelmiä, voit myös käyttää tämän määritteen jolla näistä menetelmistä luokan sijaan. Jos haluat julkaista server project.


        [Authorize]
        public class TodoItemController : TableController<TodoItem>

+ **Node.js Taustajärjestelmä (joko Node.js koodi)** :  
    
    Edellyttää todennusta taulukon käyttöä, Lisää seuraava rivi Node.js palvelimen komentosarja:


        table.access = 'authenticated';

    Lisätietoja on viitata [miten: Vaadi todennus käyttämään taulukoiden](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#howto-tables-auth). Lisätietoja Lataa pikaopas koodiprojektin sivustoon on artikkelissa [Toimintaohje: lataa Node.js Taustajärjestelmä pikaopas koodiprojektin käyttämällä Git](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart).


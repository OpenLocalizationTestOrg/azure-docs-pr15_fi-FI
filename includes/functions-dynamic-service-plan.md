## <a name="dynamic-service-plan"></a>Dynaaminen palvelusopimus

-Dynaaminen palvelun suunnittelu-funktion sovelluksia määritetään funktion sovelluksen esiintymään. Jos tarvitaan esiintymiä lisätään dynaamisesti.
Tilanteita, voi olla useita tietojenkäsittely resurssien tekeminen tehokkaasti käytettävissä Azure infrastruktuurin välillä. Lisäksi että toiminnot suoritetaan pienentäminen meni pyynnöt kokonaisaika rinnakkain. Suoritusaika jokaisen funktion yhteenlaskettu, sekunteina ja koostetun sisältävä funktio-sovelluksessa. Kanssa kustannusten perustuva verran esiintymät, niiden muistin koon ja yhteensä suoritusaika gigatavun sekunteina mitattuna. Tämä on erinomainen tapa, jos Laske tarpeitasi on katkonainen tai projektin kertaa voivat olla erittäin lyhyt, sillä voit maksaa vain Laske resurssien, kun ne ovat todella.   

### <a name="memory-tier"></a>Muistin taso

Sen mukaan, funktion tarvitsee voit valita muistin suoritettava ne funktio-sovelluksessa (säilö funktioita).
Muistin koon vaihtoehdot määräytyvät **128**Mt 1536 megatavua. Valitun muistikoko vastaa toimi Set-tarvitsemia kaikki toiminnot, jotka ovat funktion sovelluksen osa. Jos koodi edellyttää enemmän muistia valittua kokoa, toiminnon sovelluksen esiintymää suljetaan käytettävissä oleva muisti ei vuoksi.

### <a name="scaling"></a>Skaalaus

Azure-Funktiot-ympäristö arvioi liikenne tarpeiden, määritetty käynnistimien perusteella voit päättää, milloin skaalata ylös tai alas. Rakeisuuden, skaalaus on funktion-sovellus. Skaalauksen tarkoitetaan tässä tapauksessa lisääminen Lisää funktio-sovellusten esiintymiä. Inversely liikenne siirtyy-toimintoa sovelluksen esiintymät ovat ei käytössä-tekstin skaalaus nollaan ei mitään suoritettaessa.  

### <a name="resource-consumption-and-billing"></a>Resurssin kulutus ja laskutus

Dynaaminen-tilassa resurssivaraukset tehdään eri tavalla kuin Vakio App palvelusopimus, joten kulutus malli on myös eri salliminen "maksu-käyttökertakustannuksia"-malli. Kulutus raportoidaan app funktiota kohden vain ajaksi, kun koodi suoritetaan.  
Kertomalla muistin kokoa (gt) on laskettu mukaan kaikki Funktiot, funktio-sovelluksen sisällä aika (sekunteina) kokonaismäärä. Muuttaminen kulutus on **gt-su (gigatavun sekuntia)**.
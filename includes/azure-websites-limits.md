Resurssi|Vapaa|Jaettu (ennakkoversio)|Perustoiminnot|Vakio|Premium (ennakkoversio)</th>
---|---|---|---|---|---
[Web-Matkapuhelin tai API sovellusten](https://azure.microsoft.com/services/app-service/) per [App palvelusopimus](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)<sup>1</sup>|10|100|Rajoittamaton<sup>2</sup>|Rajoittamaton<sup>2</sup>|Rajoittamaton<sup>2</sup>
[Logiikan sovellukset](https://azure.microsoft.com/services/app-service/logic/) [App palvelusopimus](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)kohti</a><sup>1</sup>|10|10|10|20 core kohden|20 core kohden
[Sovelluksen palvelusopimus](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)|1 alueittain.|10 resurssiryhmä kohden|100 euroa resurssiryhmä|100 euroa resurssiryhmä|100 euroa resurssiryhmä
Laske esiintymätyyppi|Jaettu|Jaettu|Erillinen<sup>3</sup>|Erillinen<sup>3</sup>|Erillinen<sup>3</sup></p>
[Skaalaa-kohtaa](../articles/app-service-web/web-sites-scale.md) (enintään esiintymät)|jaettu 1|jaettu 1|3 erillinen<sup>3</sup>|10 erillinen<sup>3</sup>|20 omistautunut (50 Ietokannan)<sup>3,4</sup>
Tallennustilan<sup>5</sup>|Vähintään 1 gt<sup>5</sup>|Vähintään 1 gt<sup>5</sup>|10 Gigatavua<sup>5</sup>|50 Gigatavun<sup>5</sup>|500 gt<sup>4,5</sup></p>
Suorittimen aika (lyhyt)<sup>6</sup>|3 minuuttia|3 minuuttia|Rajoittamaton, Vakio [korvaukset](https://azure.microsoft.com/pricing/details/app-service/) maksaminen</a>|Rajoittamaton maksamisen on vakio.|Rajoittamaton maksamisen on vakio.
Suorittimen aika (päivä)<sup>6</sup>|60 minuuttia|240 minuuttia|Rajoittamaton, Vakio [korvaukset](https://azure.microsoft.com/pricing/details/app-service/) maksaminen</a>|Rajoittamaton maksamisen on vakio.|Rajoittamaton maksamisen on vakio.
Muisti (1 tunti)|Sovelluksen palvelusopimus 1 024 Mt|Sovelluksen 1 024 Mt|PUUTTUU|PUUTTUU|PUUTTUU
Kaistanleveyden|165 MT|Rajoittamaton, [tiedonsiirron korvaukset](https://azure.microsoft.com/pricing/details/data-transfers/) käyttäminen|Rajoittamaton, tiedonsiirto korvaukset käyttäminen|Rajoittamaton, tiedonsiirto korvaukset käyttäminen|Rajoittamaton, tiedonsiirto korvaukset käyttäminen
Sovelluksen arkkitehtuuri|32-bittinen|32-bittinen|32-bittinen ja 64-bittinen|32-bittinen ja 64-bittinen|32-bittinen ja 64-bittinen
Web-Sockets esiintymän<sup>7</sup> kohden|5|35|350|Rajoittamaton tallennus|Rajoittamaton tallennus
Samanaikainen [Virheenkorjaus yhteydet](../articles/app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md) sovellusta kohden|1|1|1|5|5
[azurewebsites.NET alitoimialueen FTP/S ja SSL](../articles/app-service-web/web-sites-configure-ssl-certificate.md)|X|X|X|X|X
[Mukautetun toimialueen](../articles/app-service-web/web-sites-custom-domain-name.md) tuki||X|X|X|X
Mukautetun toimialueen [SSL-tuki](../articles/app-service-web/web-sites-configure-ssl-certificate.md)|||Rajoittamaton tallennus|Rajoittamaton, 5 SNI SSL ja sisällyttää 1 IP SSL-yhteyksiä|Rajoittamaton, 5 SNI SSL ja sisällyttää 1 IP SSL-yhteyksiä
Integroitu kuormituksen||X|X|X|X
[Aina päällä](../articles/app-service-web/web-sites-configure.md)|||X|X|X
[Ajoitettu varmuuskopiointi](../articles/app-service-web/web-sites-backup.md)||||Kerran päivässä|Kun kaikki 5 minuuttia<sup>8</sup>
[Automaattinen skaalaus](../articles/app-service-web/web-sites-scale.md)|||X|X|X
[WebJobs](../articles/app-service-web/web-sites-create-web-jobs.md) <sup>9</sup>|X|X|X|X|X
[Azure ajoitus](https://azure.microsoft.com/services/scheduler/) -tuki||X|X|X|X
[Päätepisteen seuranta](../articles/app-service-web/web-sites-monitor.md)|||X|X|X
[Väliaikaisen paikkojen (ennakkoversio)](../articles/app-service-web/web-sites-staged-publishing.md)||||5|20
Mukautettujen toimialueiden app kohden</a>||500|500|500|500
SLA||<p>|99,9 %|99.95 %<sup>10</sup>|99.95 %<sup>10</sup>

<sup>1</sup> Sovellusten ja tallennuskiintiöiden ovat sovelluksen palvelusopimus kohden, ellei ole mainittu muuten.  
<sup>2</sup> Sovellukset, jotka voit ylläpitää nämä tietokoneissa tosiasiallinen määrä määräytyy sovellusten toimintaa, koko tietokoneen esiintymät ja vastaavan resurssien käyttö.  
<sup>3</sup> Erillinen esiintymiä voi olla erikokoisia. Saat lisätietoja [Sovelluksen palvelun hinnat](https://azure.microsoft.com/pricing/details/data-transfers/pricing/details/app-service/) .  
<sup>4</sup> Premium taso sallii enintään 50 laskee esiintymät (veloittaa saatavuuden) ja 500 Gigatavua levytilaa käytettäessä App palvelun ympäristöissä ja 20 Laske esiintymät ja 250 gt: n tallennustilan muuten.  
<sup>5</sup> Tallennustila on sisällön yhteiskokoa yli kaikki sovellukset saman sovelluksen palvelusopimus. Tallennustilan Lisäasetukset ovat käytettävissä [Sovelluksen Service-ympäristössä](../articles/app-service-web/app-service-web-configure-an-app-service-environment.md#storage)  
<sup>6</sup> Nämä resurssit ovat rajoitettu erillinen esiintymät (esiintymän koon ja esiintymien määrän) fyysinen resurssien mukaan.  
<sup>7</sup> Jos sovelluksen skaalata kaksi esiintymissä Basic taso-, sinun on 350 samanaikaisia yhteyksiä kunkin kaksi esiintymää.  
<sup>8</sup> Premium taso sallii varmuuskopion väliajoin alaspäin enintään 5 minuutin välein käytettäessä App palvelun ympäristöissä ja 50 kertaa päivässä muulla tavalla.  
<sup>9</sup> Suorita mukautettu suoritettavat ja/tai komentosarjoja pyydettäessä, aikataulua tai jatkuvasti taustatehtävän kuluessa App palvelun-esiintymä. Aina on pakollinen jatkuva WebJobs suorittamisen. Azure ajoituksen vapaa tai Standard vaaditaan ajoitetun WebJobs. Ei ole valmiiksi määritetyistä rajoitettu WebJobs, voit suorittaa sovelluksen palvelun esiintymän määrän, mutta on käytännön rajoitukset, jotka riippuvat mitä sovelluksen koodi yrittää tehdä.   
<sup>10</sup> 99.95 % annettu käyttöönotoissa, joissa on käytössä useita kertoja Azure liikenteen hallinta SLA määritetty automaattisesti.  

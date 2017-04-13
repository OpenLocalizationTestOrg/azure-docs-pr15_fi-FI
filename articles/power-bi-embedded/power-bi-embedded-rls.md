<properties
   pageTitle="Rivin tason arvopaperille, jonka Power BI upotetun"
   description="Rivin tason arvopaperille, jonka Power BI upotetut tiedot"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="row-level-security-with-power-bi-embedded"></a>Rivin tason arvopaperille, jonka Power BI upotetun

Käytön rajoittaminen tietyn raportin tai tietojoukko, useita eri käyttäjien on käytettävä samaa raporttia kaikki pelkkiä eri tietojen salliminen tietojen avulla voidaan rivin suojauksen (RLS). Power BI upotetun tukee nyt määritetty RLS tietojoukkoja.

![](media\power-bi-embedded-rls\pbi-embedded-rls-flow-1.png)

Voit hyödyntää RLS, on tärkeää ymmärtää kolme tärkeimmät käsitteitä; Käyttäjät, rooleja ja säännöt. Tarkempi katsaus kunkin voit:

**Käyttäjien** – nämä ovat todellinen loppukäyttäjien raporttien tarkasteleminen. Valitse Power BI upotetun käyttäjät tunnistetaan App tunnuksen username-ominaisuus.

**Roolien** – käyttäjät kuuluvat roolit. Rooli on säännöt säilö ja voidaan nimetä jotain, kuten "Myyntipäällikkö" tai "Myyntiedustaja". Power BI upotetun-käyttäjät tunnistetaan App tunnuksen roolit-ominaisuudessa.

**Sääntöjen** – roolit ovat säännöt ja säännöt ovat todellinen suodattimia, joita olet luomassa käytettävät tiedot. Tämä voi johtua helposti "maan = USA" tai jotain paljon dynaaminen.

### <a name="example"></a>Esimerkki

Tässä artikkelissa jäljempänä annamme Esimerkki authoring RLS ja muissa, upotetut-sovelluksesta. Tässä esimerkissä käytetään [jälleenmyynti Analysis](http://go.microsoft.com/fwlink/?LinkID=780547) PBIX mallitiedostossa.

![](media\power-bi-embedded-rls\pbi-embedded-rls-scenario-2.png)

Microsoftin jälleenmyynti Analysis-malli näkyy myynti kaikki myymälät tietyn jälleenmyynti ketjussa. RLS, riippumatta siitä, mitä alueen hallinta kirjautuu ja näkymien raportin ilman niitä näkyy samat tiedot. Ylin johto on määrittänyt kunkin alueen hallinta pitäisi näkyä vain ne hallinta stores myyntiä ja voit tehdä tämän Microsoft käyttää RLS.

RLS on luotu Power BI Desktopin. Kun tietojoukko ja raportti avataan, emme voi siirtyä kaavio-näkymän rakenteen:

![](media\power-bi-embedded-rls\pbi-embedded-rls-diagram-view-3.png)

Seuraavassa on muutamia asioita, huomaat tämän rakenteen käyttäminen:

-   **Myynti** -faktataulukon on tallennettu kaikki toimenpiteet, kuten **Kokonaismyynti**.
-   Muita aiheeseen liittyviä dimension neljä taulukoita on: **kohteen**, **ajan**, **tallentaminen**ja **alue**.
-   Yhteysviivojen nuolet osoittavat, miten suodattimia voi flow yhdestä taulukosta toiseen. Esimerkiksi jos suodatin on sijoitettu **aika [päivämäärä]**, nykyisessä rakenteessa se suodattaa vain alaspäin **Sales** -taulukon arvot. Muiden taulukoiden vaikuttaa suodatin jälkeen kaikki nuolet yhteysviivojen Valitse sales-taulukon ja ole poissa.
-   **Alueen** taulukon ilmaisee, kenelle esimies on kunkin alueen:

    ![](media\power-bi-embedded-rls\pbi-embedded-rls-district-table-4.png)

Perusteella tätä rakennetta, jos emme Käytä suodatinta alueen **Alueen hallinta** -sarakkeen, ja jos, joka suodattaa hakutuloksia käyttäjän katselet raporttia, suodattimen myös suodattaa alaspäin **tallentaminen** ja **Myynnin** taulukot Näytä vain kyseisen tietyn alueen tietojen hallinta.

Näin miten:

1.  Mallinnus-välilehdessä **Roolien hallinta**.  
![](media\power-bi-embedded-rls\pbi-embedded-rls-modeling-tab-5.png)

2.  Luo uusi rooli nimeltään **Manager**.  
![](media\power-bi-embedded-rls\pbi-embedded-rls-manager-role-6.png)

3.  Kirjoita **alueen** taulukon DAX-lauseke: **[hallinnan alue:] = USERNAME()**  
![](media\power-bi-embedded-rls\pbi-embedded-rls-manager-role-7.png)

4.  Varmista, että työskentelet sääntöjä **Modeling** -välilehdessä **näyttömuoto roolit**ja syötä seuraavat:  
![](media\power-bi-embedded-rls\pbi-embedded-rls-view-as-roles-8.png)

    Raporttien näkyy nyt tiedot kuin, jos olet kirjautuneena sisään **Andrew'lle Ma**.

Suodattamisen tapaa, jolla on ollut tässä suodattaa kaikki tietueet **alueen**, **tallentaminen**ja **Myynnin** taulukot alaspäin. Kuitenkin vuoksi suhteita ** **Myynti** - ja**suodattimen luettaviksi **Myynti** ja **kohteen**, ja **kohde** ja **aika** -taulukoita ei suodatetaan alaspäin.

![](media\power-bi-embedded-rls\pbi-embedded-rls-diagram-view-9.png)

Joka voi olla tämä vaatimus ok, kuitenkin olemme halua tarkastella, jonka hän ei ole myyntiä valvojat, emme voi kaksisuuntainen ristisuodatus yhteyden ottaminen käyttöön ja siirtyvät molempiin suuntiin suojaus-suodatin. Voit tehdä tämän muokkaamalla **Myynti** ja **kohteen**suhde tältä:

![](media\power-bi-embedded-rls\pbi-embedded-rls-edit-relationship-10.png)

Nyt suodattimia voi myös flow Sales-taulukon **kohde** -taulukkoon:

![](media\power-bi-embedded-rls\pbi-embedded-rls-diagram-view-11.png)

**Huomautus** Jos käytät tietojen DirectQuery-tilassa, tarvitset käyttöön kaksisuuntainen rajat suodatuksen valitsemalla näistä vaihtoehdoista:

1.  **Tiedoston** -> **asetusten** -> **Esikatselutoiminnot** -> **cross suodattamisen molempiin suuntiin, DirectQuery Ota käyttöön**.
2.  **Tiedoston** -> **asetusten** -> **DirectQuery** -> **sallia Rajoittamattomien mittayksikön DirectQuery-tilassa**.


Lisätietoja sellaisten kaksisuuntaisten ristisuodatus Lataa [Kaksisuuntainen ristisuodatus SQL Server Analysis Services-2016 ja Power BI Desktopin](http://download.microsoft.com/download/2/7/8/2782DF95-3E0D-40CD-BFC8-749A2882E109/Bidirectional cross-filtering in Analysis Services 2016 and Power BI.docx) julkaisu.

Tämä rivittää ylös kaikki työmäärän, joka on tehtävä Power BI Desktopin, mutta on yksi Lisää töiden, joka on tehtävä, varmista, RLS säännöt määritimme työ-Power BI upotetun. Käyttäjien todennus ja myöntänyt sovelluksen ja sovelluksen tunnusten käytetään tietyn Power BI upotetun raportin kyseisen käyttäjän käyttöoikeuden myöntäminen. Power BI upotetun ei ole mitään tiettyjä tietoja, jotka käyttäjän on. RLS toimimaan, sinun on joitakin lisäkontekstia välittää app tunnuksen osana:
-   **käyttäjänimi** (valinnainen) – tämä RLS käyttämisestä on merkkijono, joka voidaan tunnistaa käyttäjän kohdistettaessa RLS säännöt. Katso upotettu tietuetason Power BI-rivin avulla
-   **roolien** – merkkijonon, joka sisältää roolit Valitse rivin tasolle suojaussäännöt käytettäessä. Jos useampi kuin yksi rooli, on siirrettävä merkkijonon matriisina.

Jos username-ominaisuus on käytössä, sinun on myös välitettävä vähintään yksi arvo roolit.

Koko sovelluksen tunnus näyttää seuraavankaltaiselta:

![](media\power-bi-embedded-rls\pbi-embedded-rls-app-token-string-12.png)

Nyt ja kaikki yhteen, kun joku kirjautuu Microsoftin sovellukseen Tarkastele raporttia, hän vain voi, he saavat on Microsoftin rivin suojauksen määrittämät tiedot.

![](media\power-bi-embedded-rls\pbi-embedded-rls-dashboard-13.png)

## <a name="see-also"></a>Katso myös
[Power suojauksen rivin tason (RLS)](https://powerbi.microsoft.com/en-us/documentation/powerbi-admin-rls/)

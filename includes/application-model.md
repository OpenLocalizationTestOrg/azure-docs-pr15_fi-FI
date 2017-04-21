# <a name="compute"></a>Laske

Azure avulla voit ottaa käyttöön ja seurata sovelluksen koodin suorittaminen sisään Microsoft-palvelinkeskuksessa. Kun sovelluksen luominen ja suorittaa sen Azure-koodi- ja määritystietoja yhdessä kutsutaan Azure isännöidään palvelu. Isännöityjen palveluiden on helppo hallinta, skaalata ylös ja alas, uudelleen ja Päivitä uuden sovelluksen koodi-versioiden kanssa. Tässä artikkelissa keskitytään isännöidään Azure palvelun sovelluksen malli.<a id="compare" name="compare"></a>

## Sisällysluettelo<a id="_GoBack" name="_GoBack"></a>

-   [Azure-sovelluksen malli edut][]
-   [Isännöityjen palvelun Core käsitteitä][]
-   [Isännöityjen palvelun Tyyliseikat][]
-   [Sovelluksen akselille][]
-   [Isännöityjen Service Definition ja määritykset][]
-   [Palvelun määritystiedosto][]
-   [Palvelun kokoonpanotiedosto][]
-   [Luominen ja käyttöönotto isännöityä palvelu][]
-   [Viittaukset][]

## <a id="benefits"> </a>Azure sovelluksen mallin edut

Kun asennat sovelluksen isännöityä palveluna, Azure Luo vähintään yksi näennäiskoneiden (VMs), jotka sisältävät sovelluksen koodi ja käynnistyy VMs Fyysinen tietokoneissa asuvat jokin Azure tietojen keskikohdan mukaan. Kun asiakkaan pyyntöihin isännöityä sovelluksen syöttämiseksi tietokeskuksen kuormituksen tasauspalvelun jakaa nämä pyynnöt tasaisesti VMs. Kun sovellus nykyisessä Azure-tietokannassa, käyttäjät kolme tärkeimmät edut:

-   **Suuren käytettävyyden.** Suuren käytettävyyden tarkoittaa Azure varmistaa, että sovellus on käynnissä mahdollisimman paljon ja voi vastata asiakkaan. Jos sovelluksen lopettaa (käsittelemättömän poikkeuksen vuoksi, esimerkiksi), ja valitse Azure tunnistaa tämän ja se näkyy automaattisesti Käynnistä sovelluksesi uudelleen. Jos tietokoneen sovellus on käynnissä kokemukset jokin lisätoimi laitteistovirhe, sitten Azure myös tunnistaa tämä ja automaattisesti Luo uusi AM toimimasta fyysinen käyttäjäprofiilista ja Suorita koodi sieltä. Huomautus: Jotta saat Microsoftin tason palvelusopimus 99.95 prosenttia on käytettävissä sovelluksen on oltava vähintään kaksi virtuaalilaitteiksi sovelluksen-koodin. Näin yksi AM käsitellä asiakkaan pyyntöjä, kun Azure siirtää koodisi epäonnistui AM uusi, hyvä AM.

-   **Skaalattavuus.** Azure avulla voit helposti ja dynaamisesti VMs sovelluksen koodin suorittaminen käsittelemään sovelluksesi markkinoille todellisen kuormituksen määrän muuttaminen. Voit säätää työmäärää, asiakkaiden sijoittaminen sitä aikana maksaa vain, kun tarvitset niitä VMs sovellusta. Kun haluat muuttaa VMs määrää, Azure vastaa tekeminen voidaan muuttaa dynaamisesti virtuaalilaitteiksi niin usein kuin haluttu määrä minuutin kuluessa.

-   **Hallittavuutta.** Koska Azure on ympäristö ojentamassa palvelu (PaaS), hallitsee näiden käyttävät koneet huomioitavat tarvittavan infrastruktuurin (laitteiston itse, sähkö ja verkko). Azure hallitsee myös ympäristö varmistaa, että ajan tasalla käyttöjärjestelmä, jossa kaikki oikea korjaukset ja päivitykset sekä komponenttien päivitykset, kuten .NET Framework ja IIS. Koska kaikki VMs on käytössä Windows Server 2008-Azure on lisäominaisuuksia, kuten diagnostiikan valvonta, työpöydän etätuki, palomuurien ja sertifikaatin säilön määritys. Nämä ominaisuudet ovat käytettävissä numerosta ylimääräisiä kustannus. Itse asiassa suorittaessasi sovelluksen Azure Windows Server 2008-käyttöjärjestelmän (OS) käyttöoikeuden sisältyy. Koska kaikki VMs on käytössä Windows Server 2008-koodi, joka suoritetaan Windows Server 2008: n toimii vain Azure suoritettaessa.

## <a id="concepts"> </a>Isännöidään palvelun Core käsitteitä

Kun sovellus otetaan käyttöön isännöityä palveluna Azure-tietokannassa, se toimii vain yhden tai usean *roolit.* *Rooli* viittaa yksinkertaisesti sovelluksen tiedostoista ja määritykset. Voit määrittää yhden tai useamman roolin sovelluksen, jokaisella on oma joukko sovelluksen tiedostoista ja määritykset. Kunkin roolin sovelluksen voit määrittää VMs tai *rooli esiintymien*määrän suorittamiseen. Alla olevassa kuvassa Näytä kaksi yksinkertaisia esimerkkejä sovelluksen mallintaa isännöityä palvelun roolit ja roolin esiintymät.

##### <a name="figure-1-a-single-role-with-three-instances-vms-running-in-an-azure-data-center"></a>Kuva 1: Rooliin kolme esiintymät (VMs) Azure tietokeskuksen käynnissä

![Kuva][0]

##### <a name="figure-2-two-roles-each-with-two-instances-vms-running-in-an-azure-data-center"></a>Kuva 2: Kaksi roolia, kunkin ja kaksi esiintymää (VMs) Azure tietokeskuksen käynnissä

![Kuva][1]

Rooli esiintymät käsitellä yleensä Internet-asiakasohjelman pyynnöt kirjoittaminen tietokeskuksen kautta mitä kutsutaan *syötteen päätepiste*. Rooliin voi olla 0 tai Lisää syötteen päätepisteet. Kukin päätepiste ilmaisee protocol (HTTP, HTTPS tai TCP) ja portin. Yhteistä on syötteen päätepisteet roolin määrittäminen: HTTP kuuntele porttia 80 ja kuuntele porttiin 443 HTTPS. Alla olevassa kuvassa on esimerkki kahdesta eri rooleja ja eri syötteen päätepisteet ohjaavat asiakkaan pyyntöihin niihin.

![Kuva][2]

Kun luot Azure isännöityä palvelu, se on määritetty julkisesti käytettävissä IP-osoitteeseen, asiakkaat avulla voit käyttää sitä. Yhteydessä luominen isännöityä palvelun URL-etuliitteen, joka on yhdistetty, IP-osoite on valittava. Tämä etuliite on oltava yksilöiviä, kuten ovat olennaisesti varaaminen *etuliite*. cloudapp.net URL-Osoitteen niin, että kukaan muu ei voi olla. Asiakkaiden yhteyttä roolin esiintymät käyttämällä URL-osoite. Yleensä sinulla ei jakaa tai julkaista Azure *etuliite*. cloudapp.net URL-osoite. Sen sijaan, osta DNS-nimi DNS rekisteröintipalvelusta värisinä ja määrittää DNS-nimen asiakkaan pyyntöihin uudelleenohjaamiseen Azure URL-osoite. Lisätietoja on ohjeaiheessa [Mukautetun toimialuenimen Azure-tietokannassa][].

## <a id="considerations"> </a>Isännöidään palvelun Tyyliseikat

Kun suunnittelet sovelluksen cloud-ympäristössä, on useita tapoja, joilla voit ottaa huomioon kuten viive, suuren käytettävyyden ja skaalattavuus.

Tärkeää huomiota sovelluksen koodin paikkaa ei, jos käytät isännöityä palvelun Azure-tietokannassa. On yleisiä tietoja centersin, joka on lähimpänä vähentää viive ja saat parhaan suorituskyvyn mahdollista asiakkaat sovelluksen käyttöönotto.
Voit kuitenkin valita tietokeskuksen lähempänä yrityksesi tai lähemmäksi tietojen, jos jotkin oikeudenkäyttöä tai oikeudelliset tiedot epävarma ja missä se sijaitsee. Voi isännöinnin sovelluksen-koodi on kuusi ympärille maapallon tietojen keskikohdan mukaan. Alla olevassa taulukossa näkyy käytettävissä olevat sijainnit:

<table border="2" cellspacing="0" cellpadding="5" style="border: 2px solid #000000;">
<tbody>
<tr>
<td style="width: 100px;">
**Maa/alue**

</td>
<td style="width: 200px;">
**Osa-alueiden**

</td>
</tr>
<tr>
<td>
Yhdysvallat

</td>
<td>
Etelä Keski ja Etelä keskitetyn

</td>
</tr>
<tr>
<td>
Europe

</td>
<td>
Pohjois- ja Länsi

</td>
</tr>
<tr>
<td>
Aasian

</td>
<td>
Kaakkoisaasialaiset & Itä

</td>
</tr>
</tbody>
</table>
Luodessasi isännöityä palvelu, voit valita alueen osa, sijainti, jossa haluat suorittaa koodin.

Suuren käytettävyyden ja skaalattavuus saavuttamiseksi on vakavasti tärkeää, että sovelluksesi tiedot säilytettävä keskitettyyn säilöön, joka on käytettävissä eri roolin esiintymissä. Jotta tämän Azure on useita tallennusasetukset, kuten BLOB-objektit, taulukoita ja SQL-tietokantaan. Katso lisätietoja tallennustilan tekniikalla [Tietojen tallennustilan tarjouksia Azure-tietokannassa][] on artikkelissa. Alla olevassa kuvassa näkyy, miten kuormituksen sisällä Azure tietokeskuksen jakaa eri roolin esiintymät, mikä on sama tietosäilö asiakkaan pyynnöt.

![Kuva][3]

Yleensä haluat etsiä sovelluksen koodia ja tietojen saman tietokeskuksen kuin näin pieni viive (parempi suorituskyky) kun sovelluksen koodin noutaa tiedot. Lisäksi ei perittävän kaistanleveyden kun tietoja siirretään saman tietokeskuksen kuluessa.

## <a id="scale"> </a>Sovelluksen akselille

Joskus sinun kannattaa ottaa yhden sovelluksen (kuten yksinkertaisen web-sivuston) ja sen ylläpidettävä Azure. Mutta usein, sovellus voi olla useita rooleja, että kaikki toimivat yhdessä. Esimerkiksi alla olevassa kuvassa on kaksi esiintymää sivuston rooli, Tilauksen käsitteleminen-roolin kolme esiintymät ja esiintymän raportin muodostin-roolin. Rooli kaikki toimivat yhdessä ja kaikki tavat koodi koottu yhteen- ja otettu käyttöön yhtenä yksikkönä Azure ylöspäin.

![Kuva][4]

Voit jakaa sovelluksen tuominen eri rooleja kunkin käytössä oma rooli esiintymien (eli VMs) joukon tärkeimmät syynä on skaalata roolit erikseen. Esimerkiksi aikana, monet asiakkaat voivat voidaan ostaminen tuotteiden yrityksestäsi, niin kannattaa roolin esiintymät käynnissä sivuston tehtäväsi sekä niiden käynnissä Tilauksen käsitteleminen tehtäväsi roolin esiintymien lukumäärän, Lisää. Jälkeen vapaapäivä Vuodenaika-kentät voivat nähdä useita tuotteita palauttaa, joten joudut ehkä silti sivuston esiintymät, mutta vähemmän Tilauksen käsitteleminen esiintymät paljon. Loput vuoden aikana voit joutua vain muutaman sivustoon ja Tilauksen käsitteleminen esiintymät. Koko kaikki nämä sinun on ehkä vain yksi raportin muodostin esiintymä. Roolipohjainen ominaisuuksissa Azure-tietokannassa joustavuutta avulla voit helposti mukauttaa sovelluksesi yrityksesi tarpeisiin.

On yhteinen on rooli esiintymät isännöityä palvelun keskenään. Esimerkiksi sivuston rooli hyväksyy asiakkaan tilaus, mutta se offloads Tilauksen käsittelystä Tilauksen käsitteleminen roolin esiintymät. Paras tapa välittää työn lomakkeen esiintymiä eri esiintymien roolin yksi joukko käyttää queuing tekniikka myöntämä Azure-jonossa palvelun tai palvelun Bus olevien. Jonon käytetään Tarinan tähän tärkeä osa. Jonossa avulla isännöityä palvelun skaalata sen roolit salliminen erikseen voit valita työmäärää nykyisiin kustannuksiin. Jos jonossa olevien viestien määrä kasvaa ajan myötä voit skaalata, kuinka monta Tilauksen käsitteleminen roolin esiintymät. Jos jonossa olevien viestien määrä vähenee ajan kuluessa, voit skaalata alaspäin Tilauksen käsitteleminen roolin esiintymien määrän. Näin voit vain maksaa esiintymien pakollinen käsittelemään todellinen työmäärä.

Jonossa on myös luotettavuutta. Kun Skaalausasetusten alaspäin Tilauksen käsitteleminen roolin esiintymien määrän Azure päättää lopettaa esiintymät. Voivat päättää lopettaa esiintymään, joka on keskellä käsittelyn jonon viestin. Kuitenkin viestin käsittely ei onnistu, koska sanoma tulee näkyviin uudelleen, jotta toisen Tilauksen käsitteleminen roolin ‑esiintymä, jossa se vastataan ja käsittelee sen. Vuoksi jonon viestin näkyvyys-viestit ovat taata myöhemmin Hae käsitteleminen. Jonossa toimii myös jakamalla sen kaikista roolin esiintymät, joka pyytää viestien viestien tehokkaasti kuormituksen.

Sivuston rooli-esiintymien voit valvoa ne huomioon liikenteen ja päättää skaalata määrä ylös tai alas paikan päällä. Jonossa voit skaalata sivuston roolin esiintymät ulkopuolisista Tilauksen käsitteleminen roolin esiintymien määrän. Tämä on hyvin tehokas ja antaa joustavasti. Jos sovellus koostuu Lisää rooleista, voit lisätä muita olevien kuin siirtokanava vaihtamaan välissä hyödyntää saman skaalaus ja kustannukset etuja.

## <a id="defandcfg"> </a>Isännöidään Service Definition ja määritykset

Käyttöönotossa Azure isännöityä palvelu edellyttää, että myös palvelun määritystiedosto ja palvelun määritystiedostoa. Nämä tiedostot ovat XML-tiedostot ja niiden avulla voit määrittää isännöidyn palvelun asennusvaihtoehdot määrittämisen avulla. Palvelun määritystiedoston kuvataan kaikki rooleihin, jotka muodostavat isännöityä palvelun ja kuinka ne viestiä. Palvelun kokoonpanotiedosto kuvataan kerrat kullekin rooli ja Määritä rooli jokaiselle esiintymälle-asetukset.

## <a id="def"> </a>Määritelmä-Palvelutiedosto

Voin edellä mainitun palvelun määritys (CSDEF) tiedosto on XML-tiedostoon, joka kuvaa eri rooleja, jotka muodostavat valmis sovelluksen. Täältä löytyvät valmis rakenne XML-tiedosto: [[http://msdn.microsoft.com/library/windowsazure/ee758711.aspx]].
CSDEF-tiedostossa, jonka haluat sovelluksesi rooleille WebRole tai WorkerRole-elementti. Web-roolin (joko WebRole-elementtiä) selkeä käyttöönotto tarkoittaa koodi suoritetaan rooli-esiintymässä, joka sisältää Windows Server 2008: n ja Internet Information Server (IIS).
Käyttöönotto rooli työntekijän rooli (joko WorkerRole-elementtiä) tarkoittaa, että rooli esiintymää on Windows Server 2008: n sitä (IIS ei asenneta).

Voit luoda ja ottaa Työntekijä-roolin, joka käyttää muun toiminnon avulla voit kuunnella web pyynnöt varmasti (esimerkiksi koodi luoda ja käyttää .NET HttpListener). Koska kaikki roolin esiintymät on käytössä Windows Server 2008-koodisi voi suorittaa yleensä sovelluksen Windows-palvelimessa käytettävissä olevat toiminnot
2008.

Kunkin roolin ilmaiset AM reunaviivaa, jota roolin esiintymät tulisi käyttää. Alla olevassa taulukossa näkyy nyt käytettävissä eri AM kokojen ja kaikkien määritteet:

<table border="2" cellspacing="0" cellpadding="5" style="border: 2px solid #000000;">
<tbody>
<tr align="left" valign="top">
<td>
**AM kokoa**

</td>
<td>
**SUORITIN**

</td>
<td>
**RAM-MUISTIA**

</td>
<td>
**Levy**

</td>
<td>
**Huippu-verkon i/o**

</td>
</tr>
<tr align="left" valign="top">
<td>
**Erittäin pienen**

</td>
<td>
1 x 1.0 GHz

</td>
<td>
768 MT

</td>
<td>
20 GT:

</td>
<td>
\~5 Mbps

</td>
</tr>
<tr align="left" valign="top">
<td>
**Pieni**

</td>
<td>
1 x 1,6 GHz

</td>
<td>
1,75 GIGATAVUA

</td>
<td>
225 GT

</td>
<td>
\~100 Mbps

</td>
</tr>
<tr align="left" valign="top">
<td>
**Normaali**

</td>
<td>
2 x 1,6 GHz

</td>
<td>
3,5 GIGATAVUA

</td>
<td>
490 GT

</td>
<td>
\~200 Mbps

</td>
</tr>
<tr align="left" valign="top">
<td>
**Suuri**

</td>
<td>
4 x 1,6 GHz

</td>
<td>
7 GIGATAVUA

</td>
<td>
1 TT:

</td>
<td>
\~400 Mbps

</td>
</tr>
<tr align="left" valign="top">
<td>
**Erittäin suuri**

</td>
<td>
8 x 1,6 GHz

</td>
<td>
14 GT

</td>
<td>
2 TERATAVUA

</td>
<td>
\~800 Mbps

</td>
</tr>
</tbody>
</table>
Perittävän kerran tunnissa kunkin AM käytät roolin esiintymän ja myös perittävän tietoja roolin esiintymät lähettää tietokeskuksen ulkopuolella. Tietojen syöttäminen tietokeskuksen ei perittävän. Lisätietoja on artikkelissa [Azure hinnat] []. Yleinen on suositeltavaa käyttää monta pieni roolin esiintymää eikä muutamia suuri esiintymät siten, että sovelluksesi on enemmän joustavat virhe. Kun kaikki on vähemmän roolin esiintymät, Lisää suuria on yksi ne on yleinen-sovellukseen. Lisäksi mainittu ennen on otettava käyttöön vähintään kaksi esiintymää kunkin roolin jotta saat 99.95 %-palvelun ohjeasiakirjoissa, Microsoft toimittaa.

Palvelun määritystiedoston (CSDEF) on myös kohtaa, johon määrittää monia määritteitä tietoja rooleille sovelluksen. Seuraavassa on kuvattu joitakin hyödyllisempi kohteiden käytettävissä:

-   **Varmenteet**. Voit käyttää varmenteiden salaamiseen tietoja tai jos web-palvelu tukee SSL. Sertifikaatteja on ladattava Azure. Lisätietoja on artikkelissa [Azure hallinta varmenteet][]. XML-asetusta asentaa aiemmin ladattu varmenteet roolin esiintymän varmenteen säilöön, niin, että ne ovat käytettävissä sovelluksen-koodin.

-   **Määritys-asetusta nimet**. Arvot, jotka haluat lukea esitettävän rooli-esiintymässä oman sovellukset. Todellinen arvo asetukset on määritetty määritystiedostossa service (CSCFG), jotka voidaan päivittää milloin tahansa tarvitsematta käyttöön omassa koodissasi. Voit itse asiassa koodin sovellustesi esiintyvien muutetut määritysten arvot niin, ettei mitään käyttökatkot tavalla.

-   **Kirjoita päätepisteet**. Tässä voit määrittää minkä tahansa HTTP, HTTPS tai TCP päätepisteet (portit), jonka haluat näyttää muille oman *etuliite*kautta. cloadapp.net URL-osoite. Kun Azure käyttää tehtäväsi, se määritetään palomuurin rooli-esiintymässä automaattisesti.

-   **Sisäinen päätepisteet**. Tässä voit määrittää minkä tahansa HTTP- tai TCP päätepisteet, jonka haluat muiden roolin esiintymät, jotka on otettu sovelluksen osana käsiksi. Sisäinen päätepisteet Salli kaikki roolin keskustelemaan toisiinsa sovelluksen esiintymät, mutta eivät ole käytettävissä minkä tahansa rooli esiintymät, jotka ovat sovelluksen ulkopuolella.

-   **Tuo moduulit**. Nämä voit myös asentaa hyödyllisiä osien rooli esiintymät. Osien olemassa diagnostiikan seuranta, Etätyöpöytä ja Azure Yhdistä (joka sallii rooli-esiintymä käyttämään paikallista resurssien suojatun kanavan kautta).

-   **Paikallinen tallennus**. Tämä varaa alikansio sovelluksen käyttämään rooli-esiintymässä. Se on kuvattu yksityiskohtaisempia [Tietoja tallennustilan tarjouksia Azure-tietokannassa][] on artikkelissa.

-   **Käynnistyksen tehtävät**. Käynnistyksen tehtävät antaa voi asentaa tarvittavat osat roolin esiintymän, kun se käynnistyy ylöspäin. Tehtävät voidaan suorittaa laajennettuja järjestelmänvalvojana tarvittaessa.

## <a id="cfg"> </a>Service-kokoonpanotiedosto

Palvelun määritystiedosto (CSCFG) on XML-tiedostoon, joka kuvaa, joka voi muuttua ilman mallirakenteeseen sovelluksen asetukset. Täältä löytyvät valmis rakenne XML-tiedosto: [http://msdn.microsoft.com/library/windowsazure/ee758710.aspx][].
CSCFG-tiedosto sisältää rooli elementin kunkin roolin-sovelluksessa. Seuraavassa on joitakin kohteita, voit määrittää CSCFG-tiedosto:

-   **Käyttöjärjestelmäversio**. Tämän määritteen avulla voit valita (OS) käyttöjärjestelmän haluat käyttää kaikkien sovelluksen koodin suorittaminen roolin esiintymien. Tämä OS tunnetaan *Vieras OS*ja uusi versio sisältää uusimmat korjaukset ja päivityksiä saatavilla Vieras OS on julkaistu aikaan. Jos määrität osVersion määritteen "\*", ja sitten Azure päivitykset Vieras OS kunkin roolin esiintymät automaattisesti, kun uusia Vieras OS versioita ovat käytettävissä. Voit valita ulos Automaattiset päivitykset valitsemalla tietyn Vieras käyttöjärjestelmäversio. Esimerkiksi asetus osVersion-määritteen arvo on "WA-Vieras-OS-2,8\_201109 01" aiheuttaa kaikki roolin saat tämän verkkosivun kuvatusta kopioita: [http://msdn.microsoft.com/library/hh560567.aspx][]. Saat lisätietoja Vieras OS versioista [Azure vieraiden OS hallinta päivitykset].

-   **Esiintymät**. Tämä elementti arvo tarkoittaa roolin ilmentymät, jotka haluat valmisteltu tietyn roolin koodin suorittaminen määrä. Voit ladata uuden CSCFG tiedoston Azure (ilman mallirakenteeseen sovelluksen), se on trivially yksinkertainen elementin arvo ja lataa uusi CSCFG tiedosto dynaamisesti Suurenna tai Pienennä roolin esiintymät sovelluksen koodin suorittaminen määrä. Voit helposti skaalata sovelluksen ylös- tai alaspäin samalla hallinta myös siitä, miten paljon perittävän roolin kopioiden vaatimukset tavata todellinen työmäärä.

-   **Määritysten asetusten arvoja**. Tämä elementti ilmaisee asetusten arvot (jotka on määritelty CSDEF-tiedosto). Tehtäväsi lukea nämä arvot on käynnissä. Näiden määritysten arvojen käytetään yleensä yhteyden merkkijonot SQL-tietokantaan tai Azure-tallennustilan, mutta niitä voidaan käyttää Microsoftiin tarkoitukseen.

## <a id="hostedservices"> </a>Luominen ja käyttöönotto isännöityä palvelu

Ensin Siirry [Azure hallinta-portaalin] ja valmistella isännöityä palvelun määrittämällä DNS-etuliitteen ja tietokeskuksen kädessä haluat suorittaa koodin isännöityä palvelun luomiseen tarvitaan. Kirjoita kehitysympäristö palvelun määritystiedoston (CSDEF)-sovelluksen koodin ja kaikki nämä tiedostot pakkauksen (postinumero) luominen (CSPKG) paketti tiedostoon. Laatia myös määritys (CSCFG)-palvelutiedosto. Ottaa käyttöön tehtäväsi Azure Service Management API CSPKG ja CSCFG-tiedostojen lataaminen. Kun käyttöön, Azure-säännöstä rooli esiintymät tietokeskuksen (perustuvan määritystietoja)-sovelluksen koodin purkaa paketista kopioiminen rooli esiintymät ja käynnistys esiintymät. Nyt koodi on hyvin alkuun.

Alla olevassa kuvassa CSPKG ja CSCFG tiedostoja, Luo kehittäminen-tietokoneeseen. CSPKG-tiedosto sisältää CSDEF-tiedosto ja kaksi roolia koodi. Ladattuasi CSPKG ja CSCFG-tiedostoja, joissa Azure Service Management API Azure Luo tietokeskuksen roolin esiintymät. Tässä esimerkissä CSCFG tiedoston merkitty Azure luoko kolmessa tilanteessa roolin \#rooli 1 ja 2 esiintymät \#2.

![Kuva][5]

Lisätietoja käyttöönotosta päivittäminen ja uudelleen oman roolit on artikkelissa [käyttöönotto ja Azure-sovellusten päivittäminen][] .<a id="Ref" name="Ref"></a>

## <a id="references"> </a>Viittaukset

-   [Azure isännöityä palvelun luominen][]

-   [Azure-tietokannassa isännöityä palveluiden hallinta][]

-   [Azure-sovellusten siirtäminen][]

-   [Azure-sovelluksen määrittäminen][]

<div style="width: 700px; border-top: solid; margin-top: 5px; padding-top: 5px; border-top-width: 1px;">

<p>Kirjoitettu Jeffrey Richter (Wintellect)</p>

</div>

  [Azure-sovelluksen malli edut]: #benefits
  [Isännöityjen palvelun Core käsitteitä]: #concepts
  [Isännöityjen palvelun Tyyliseikat]: #considerations
  [Sovelluksen akselille]: #scale
  [Isännöityjen Service Definition ja määritykset]: #defandcfg
  [Palvelun määritystiedosto]: #def
  [Palvelun kokoonpanotiedosto]: #cfg
  [Luominen ja käyttöönotto isännöityä palvelu]: #hostedservices
  [Viittaukset]: #references
  [0]: ./media/application-model/application-model-3.jpg
  [1]: ./media/application-model/application-model-4.jpg
  [2]: ./media/application-model/application-model-5.jpg
  [Mukautetun toimialuenimen määrittämisestä Azure-tietokannassa]: http://www.windowsazure.com/develop/net/common-tasks/custom-dns/
  [Tietoja tallennustilan tarjouksia Azure-tietokannassa]: http://www.windowsazure.com/develop/net/fundamentals/cloud-storage/
  [3]: ./media/application-model/application-model-6.jpg
  [4]: ./media/application-model/application-model-7.jpg
  
  [Azure Pricing]: http://www.windowsazure.com/pricing/calculator/
  [Azure-tietokannassa varmenteiden hallinta]: http://msdn.microsoft.com/library/windowsazure/gg981929.aspx
  [http://msdn.microsoft.com/library/windowsazure/ee758710.aspx]: http://msdn.microsoft.com/library/windowsazure/ee758710.aspx
  [http://msdn.microsoft.com/library/hh560567.aspx]: http://msdn.microsoft.com/library/hh560567.aspx
  [Azure vieraiden OS päivitysten hallinta]: http://msdn.microsoft.com/library/ee924680.aspx
  [Azure hallinta-portaalissa]: http://manage.windowsazure.com/
  [5]: ./media/application-model/application-model-8.jpg
  [Käyttöönotto ja Azure-sovellusten päivittäminen]: http://www.windowsazure.com/develop/net/fundamentals/deploying-applications/
  [Azure isännöityä palvelun luominen]: http://msdn.microsoft.com/library/gg432967.aspx
  [Azure-tietokannassa isännöityä palveluiden hallinta]: http://msdn.microsoft.com/library/gg433038.aspx
  [Azure-sovellusten siirtäminen]: http://msdn.microsoft.com/library/gg186051.aspx
  [Azure-sovelluksen määrittäminen]: http://msdn.microsoft.com/library/windowsazure/ee405486.aspx

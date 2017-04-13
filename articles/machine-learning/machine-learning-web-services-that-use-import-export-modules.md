<properties
    pageTitle="Käyttöönotto Azure ML verkkopalvelut, jotka käyttävät tuonti ja vienti moduulit | Microsoft Azure"
    description="Opettele käyttämään tuontitiedot ja vie tiedot moduulit lähettämiseen ja vastaanottamiseen verkkopalvelun tiedot."
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="raymondlaghaeian"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/12/2016"
    ms.author="v-donglo"/>



# <a name="deploying-azure-ml-web-services-that-use-data-import-and-data-export-modules"></a>Azure ML verkkopalvelut, jotka käyttävät tuonti ja vienti moduulit käyttöönotto 

Kun luot ennakoivan koe, voit tavallisesti lisätä WWW-palvelun syötteen ja tulosteen. Kun otat käyttöön koe, kuluttajille voit lähettää ja vastaanottaa tiedot verkkopalvelusta kautta syötteiden ja tulostaa. Joidenkin sovellusten kuluttaja tiedot voidaan käytettävissä tietosyöte tai ulkoisesta tietolähteestä, kuten Azure-Blob-säiliö jo sijaitsevat. Tällaisissa tapauksissa niitä ei tarvitse lukeminen ja kirjoittaminen käyttäminen WWW-palvelun syötteiden ja tulostaa tiedot. Ne sijaan erä suorittamisen Service (BES) avulla voit lukea tietoja tietolähteestä, tuontitiedot-moduulin ja kirjoittaa tulosmalli tulokset Vie tiedot-moduulin eri sijaintiin.

Tietojen tuominen ja vieminen tietojen moduulit, voivat lukea ja kirjoittaa sijainnit, kuten WWW-URL-osoite HTTP, kyselyn rakenne, Azure SQL-tietokantaan, Azure-taulukkotallennus, Azure-Blob-säiliö Tietosyötteestä kautta antaa tiedot tai paikallinen SQL-tietokantaan.

Tässä ohjeaiheessa käyttää "Esimerkki 5: junassa-testi, arvioi binaarinen luokituksen: aikuisen tietojoukko" Esimerkki ja olettaa dataset on jo ladattu nimeltä censusdata Azure SQL-taulukkoon.

## <a name="create-the-training-experiment"></a>Koulutus kokeen luominen 
 
Kun avaat "Esimerkki 5: junassa-testi, arvioi binaarinen luokituksen: aikuisen tietojoukko" otoksen se käyttää otoksen aikuisen laskenta tulot binaarinen luokitus-tietojoukko. Ja alusta kokeen näyttää samalla tavalla kuin seuraavassa kuvassa.

![Kokeen ensimmäisen määrityksen.](./media/machine-learning-web-services-that-use-import-export-modules/initial-look-of-experiment.png)
  

Voit lukea tietoja Azure SQL-taulukosta seuraavasti:

1.  Poista tietojoukko-moduuli.
2.  Kirjoita tuo osat-hakuruutuun.
3.  Kohteen tulosten luettelosta kokeen alusta *Tietojen tuominen* -moduulin lisääminen.
4.  Yhdistä *Tietojen tuominen* -moduulin *SIIVOA puuttuvien tietojen* moduulin syötteen tulos.
5.  Valitse ominaisuudet-ruudussa Valitse **Tietolähde** avattavasta **Azure SQL-tietokantaan** .
6.  **Tietokantapalvelimen nimi** **tietokannan nimi**, **käyttäjänimi**ja **salasana** -kentät, kirjoita tarvittavat tiedot tietokannan.
7.  Kirjoita tietokannan kysely-kenttään seuraava kysely.

        select [age],
           [workclass],
           [fnlwgt],
           [education],
           [education-num],
           [marital-status],
           [occupation],
           [relationship],
           [race],
           [sex],
           [capital-gain],
           [capital-loss],
           [hours-per-week],
           [native-country],
           [income]
        from dbo.censusdata;

8.  Kokeen alareunassa Kuvapohjan, valitse **Suorita**.

## <a name="create-the-predictive-experiment"></a>Ennakoivan kokeen luominen

Seuraavaksi voit määrittää ennakoivan koe, josta otetaan käyttöön web-palveluun.

1.  Kokeen alusta alareunassa **Määrittää WWW-palvelun** ja valitse **Ennakoivan verkkopalvelun [Suositeltu]**.
2.  Poista *WWW-palvelun syötteen* ja *WWW-palvelun tulosteen moduulit* ennakoivan kokeen. 
3.  Kirjoita Vie osat-hakuruutuun.
4.  Kohteen tulosten luettelosta kokeen alusta *Vie tiedot* -moduulin lisääminen.
5.  Yhdistä *Pistemäärän mallin* moduulin *Vie tiedot* -moduulin syötteen tulos. 
6.  Valitse ominaisuudet-ruudussa Valitse **Azure SQL-tietokannan** tietojen kohde avattavasta valikosta.
7.  **Tietokantapalvelimen nimi** **tietokannan nimen**, **palvelimen käyttäjänimi**ja **palvelimen käyttäjätilin salasana** -kenttien kanssa, kirjoita tarvittavat tiedot tietokannan.
8.  Kirjoita **CSV-tiedosto tallennetaan sarakkeiden luettelo** -kentän tulos otsikot.
9.  Kirjoita **tiedot taulukon nimi-kenttään**dbo. ScoredLabels. Jos taulukossa ei ole, se luodaan, kun koe suoritetaan tai web-palvelu kutsutaan.
10. Kirjoita ScoredLabels **Luetteloerottimella erotetut arvotaulukko sarakkeiden luettelo** -kenttään.

Kun kirjoitat sovellus, joka kutsuu lopullinen web-palvelu, haluat ehkä määrittää eri syötteen kyselyn tai kohdetaulukko suorituksen aikana. Voit määrittää nämä syötteiden ja tulostaa, voit WWW-palveluparametrit-ominaisuus voit määrittää *Tietojen* module *tietolähde* -ominaisuus ja *Vie tiedot* tilassa tietojen destination-ominaisuus.  Saat lisätietoja WWW-palvelun parametrien Cortana liiketoimintatietojen ja tietokoneen Learning blogin [AzureML WWW-palvelun parametrien tapahtuma](https://blogs.technet.microsoft.com/machinelearning/2014/11/25/azureml-web-service-parameters/) .

Voit määrittää Web palveluparametrit tuo kyselyn ja kohdetaulukon seuraavasti:

1.  Valitse *Tuontitiedot* -moduulin ominaisuudet-ruudun yläreunassa olevaa kuvaketta oikeassa yläkulmassa **tietokantakyselyn** kenttä ja valitse **Aseta web-palvelun parametri**.
2.  Valitse *Vie tiedot* -moduulin ominaisuudet-ruudun yläreunassa kuvaketta oikeassa yläkulmassa **tiedot taulukon nimi** -kenttään ja valitse **Aseta web-palvelun parametri**.
3.  *Tietojen vieminen* moduuli ominaisuudet-ruudun **WWW-palveluparametrit** -osassa alareunassa kysely ja nimetä sen uudelleen kysely.
4.  Valitse **tietojen taulukon nimi** ja nimeä **taulukko**.

Kun olet valmis, että kokeessa pitäisi nyt muistuttaa seuraavassa kuvassa.
 
![Kokeen lopullinen ulkoasua.](./media/machine-learning-web-services-that-use-import-export-modules/experiment-with-import-data-added.png)

Nyt voit ottaa kokeen WWW-palvelulle.

## <a name="deploy-the-web-service"></a>WWW-palvelun käyttöön 
Voit ottaa käyttöön perinteinen tai uusi web-palvelu.

### <a name="deploy-a-classic-web-service"></a>Ota käyttöön perinteinen Web-palvelu

Ottaa käyttöön perinteinen Web-palvelu ja sovelluksen tarjoaman se luominen:

1.  Kokeen alustan, valitse Suorita.
2.  Kun Suorita on valmis, **WWW-palvelun käyttöön** ja valitse **Käyttöönotto verkkopalvelun [perinteinen]**.
3.  Etsi web Raporttinäkymät-ikkunan service API-näppäintä. Kopioida ja tallentaa sen myöhempää käyttöä varten.
4.  Valitse **Oletus päätepisteen** -taulukon Avaa API Ohjesivun **Eräkäsittely** -linkki.
5.  Visual Studiossa C#-konsolisovelluksen luominen
6.  Etsi API Ohje-sivulla koodiosassa **Esimerkki** sivun alareunassa.
7.  Kopioi ja liitä Program.cs tiedoston C#-Esimerkki koodi ja poista kaikki viittaukset Blob-objektien tallennustilaan.
8.  Päivitä *apiKey* muuttujan arvon tallennettiin Ohjelmointirajapinnan avain.
9.  Etsi pyynnön ilmoituksen ja Päivitä Web-palvelun parametrien, joka *Tuontitiedot* ja *Vie tiedot* moduulit välitetään arvot. Tässä tapauksessa Käytä alkuperäistä kyselyä, mutta Määritä uusi taulukkonimi.

        var request = new BatchExecutionRequest() 
        {   
            GlobalParameters = new Dictionary<string, string>() {
            { "Query", @"select [age], [workclass], [fnlwgt], [education], [education-num], [marital-status], [occupation], [relationship], [race], [sex], [capital-gain], [capital-loss], [hours-per-week], [native-country], [income] from dbo.censusdata" },
            { "Table", "dbo.ScoredTable2" },
            }
        };

10. Suorita sovellus. 

Suorita valmiiksi uusi taulukko lisätään tulosmalli tulokset sisältävä tietokanta.

### <a name="deploy-a-new-web-service"></a>Uusi Web-palvelu käyttöönotto

Uusi Web-palvelu ja sovelluksen tarjoaman se luominen:

1.  Kokeen alareunassa Kuvapohjan, valitse **Suorita**.
2.  Kun Suorita on valmis, **WWW-palvelun käyttöön** ja valitse **Ota käyttöön Web-palvelu [uusi]**.
3.  Ottaa käyttöön koe-sivulla WWW-palvelun nimi ja valitse hinnoittelu suunnitelma ja valitse sitten **Ota käyttöön**.
4.  Valitse **Kulutettava** **pikaopas** -sivulla.
5.  **Esimerkki** koodiosassa Valitse **erä**.
6.  Visual Studiossa C#-konsolisovelluksen luominen
7.  Kopioi ja liitä C#-sample code Program.cs-tiedostoon.
8.  Päivitä *apiKey* muuttujan arvon **Perusavaimen** **Basic kulutus info** -osa sijaitsee.
9.  Etsi *scoreRequest* ilmoituksen ja Päivitä Web-palvelun parametrien, joka *Tuontitiedot* ja *Vie tiedot* moduulit välitetään arvot. Tässä tapauksessa Käytä alkuperäistä kyselyä, mutta Määritä uusi taulukkonimi.

        var scoreRequest = new
        {
            Inputs = new Dictionary<string, StringTable>()
            {
            },
            GlobalParameters = new Dictionary<string, string>() {
                 { "Query", @"select [age], [workclass], [fnlwgt], [education], [education-num], [marital-status], [occupation], [relationship], [race], [sex], [capital-gain], [capital-loss], [hours-per-week], [native-country], [income] from dbo.censusdata" },
                { "Table", "dbo.ScoredTable3" },
            }
        };

10. Suorita sovellus. 
 


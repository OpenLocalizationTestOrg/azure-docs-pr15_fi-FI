Azure resurssien hallinnan avulla voit määrittää parametrien arvot, jotka haluat määrittää, kun malli otetaan käyttöön. Malli sisältää osan nimeltä parametrit, joka sisältää kaikki parametriarvot.
Olisi määrittää kyseiset arvot, vaihtelevat otettaessa projektin pohjalta tai otat käyttöön ympäristöön parametri. Määritä parametrien arvot, jotka aina säilyy ennallaan. Kunkin parametrin arvon käytetään mallia voit määrittää resurssit, jotka on otettu käyttöön. 

Kun määrität parametreja, mitkä arvot käyttöönoton aikana voit antaa käyttäjän määrittää **allowedValues** -kentän avulla. **Oletusarvo** -kentän avulla voit määrittää arvon parametri, jos arvoa ei ole annettu käyttöönoton aikana.

Olemme kuvataan kunkin parametrin mallia.

### <a name="sitename"></a>sivuston nimi

Verkkosovelluksen tietokenttiä haluat luoda nimi.

    "siteName":{
      "type":"string"
    }

### <a name="hostingplanname"></a>hostingPlanName

Sovelluksen palvelusopimus käytettävät isännöinnin web-sovelluksen nimi.
    
    "hostingPlanName":{
      "type":"string"
    }

### <a name="sku"></a>tuote

Isännöintipalvelu suunnitelman hinnoittelu taso.

    "sku": {
      "type": "string",
      "allowedValues": [
        "F1",
        "D1",
        "B1",
        "B2",
        "B3",
        "S1",
        "S2",
        "S3",
        "P1",
        "P2",
        "P3",
        "P4"
      ],
      "defaultValue": "S1",
      "metadata": {
        "description": "The pricing tier for the hosting plan."
      }
    }

Mallin määrittää arvot, jotka on sallittua parametrin ja määrittää oletusarvon (S1), jos arvoa ei ole määritetty.

### <a name="workersize"></a>workerSize

Esiintymän koon isännöintipalvelu suunnitelman (pieni, Normaali tai suuri).

    "workerSize":{
      "type":"string",
      "allowedValues":[
        "0",
        "1",
        "2"
      ],
      "defaultValue":"0"
    }
    
Mallin määrittää arvot, jotka on sallittua parametrin (0, 1 tai 2) ja määrittää oletusarvon (0), jos arvoa ei ole määritetty. Keskisuurten ja suurten pienille, vastaavat arvot.

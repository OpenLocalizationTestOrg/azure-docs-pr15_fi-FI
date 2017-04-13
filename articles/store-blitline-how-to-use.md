<properties 
    pageTitle="Miten Blitline käytettävä kuva käsittely - Azure ominaisuus opas" 
    description="Opettele käyttämään Blitline palvelun kuvien käsittelyyn Azure-sovelluksesta." 
    services="" 
    documentationCenter=".net" 
    authors="blitline-dev" 
    manager="jason@blitline.com" 
    editor="jason@blitline.com"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="12/09/2014" 
    ms.author="support@blitline.com"/>
# <a name="how-to-use-blitline-with-azure-and-azure-storage"></a>Voit käyttää Blitline Azure ja Azuren tallennustilaan

Tässä oppaassa kerrotaan, miten voit käyttää Blitline palveluja ja voit lähettää Blitline työt.

## <a name="what-is-blitline"></a>Mikä on Blitline?

Blitline on pilvipohjainen kuva käsittelyn palvelun, joka sisältää yrityksen tason kuva käsittelyn murtoluvun hinnan, jota maksaa luominen itse.

Se on, että kuvankäsittely on suoritettu toistuvasti, yleensä uudelleen jokaisen sivuston perusteista alkava. Ymmärrämme tämä koska on aiemmin luonut ne miljoonaa kertaa liian. Yksi päivä on päättänyt ehkä on aika on vain vaikealta kaikille. Microsoft tietää, miten voit tehdä sen vaikealta nopeasti ja tehokkaasti ja Tallenna kaikki työmäärä sen jälkeen.

Lisätietoja on artikkelissa [http://www.blitline.com](http://www.blitline.com).

## <a name="what-blitline-is-not"></a>Mitä Blitline ei ole...

Selventää, mikä Blitline on hyötyä, on usein helpompi tunnistaa Blitline ei mitä ennen työn edetessä.

- Blitline ei ole HTML-pienoisohjelmat, joilla voit ladata kuvia. Sinulla on oltava käytettävissä olevat kuvat julkisesti tai Blitline saavuttamiseksi käytettävissä rajoitetut käyttöoikeudet.

- Blitline eivät käsitteleminen, kuten Aviary.com elävä kuva

- Blitline ei hyväksy kuva latauksia, ei voi siirtää kuvia Blitline suoraan. On siirtää Azuren tallennustilaan tai eri kohdissa Blitline tukee ja kerro Blitline mistä ne saa.

- Blitline on erittäin rinnakkain ja Tee synkronoitu käsittely. Mikä tarkoittaa, että on antaminen postback_url ja emme voi kysyä, kun on tehnyt käsittely.

## <a name="create-a-blitline-account"></a>Blitline-tilin luominen

[AZURE.INCLUDE [blitline-signup](../includes/blitline-signup.md)]

## <a name="how-to-create-a-blitline-job"></a>Blitline projektin luominen

Blitline käyttää JSON haluat poistaa kuvan-toimintoja. Tämä JSON koostuu joitakin yksinkertaisia kenttiä.

Yksinkertainen esimerkki on seuraavanlainen:

        json : '{
       "application_id": "MY_APP_ID",
       "src" : "http://cdn.blitline.com/filters/boys.jpeg",
       "functions" : [ {
           "name": "resize_to_fit",
           "params" : { "width": 240, "height": 140 },
           "save" : { "image_identifier" : "external_sample_1" }
       } ]
    }'

Seuraavassa on JSON, jotka vievät "src" kuva "... boys.jpeg" ja muuta kokoa 240 x 140 kyseiseen kuvaan.

Sovellustunnus ei löytyvät Azure yhteyttä **Yhteyden tiedot** tai **hallinta** -välilehti. Salainen tunnisteen, jonka avulla voit suorittaa töitä Blitline on.

"Tallenna"-parametri ilmaisee tietoja siitä, johon haluat lisätä kuvan, kun olemme ovat käsitelleet. Trivial tässä tapauksessa emme eivät ole määritetty jokin. Jos sijaintia ei ole määritetty Blitline tallentaa sen paikallisesti (ja väliaikaisesti) on yksilöllinen pilvipalvelussa. Osaat käyttämistä palauttama Blitline, kun teet Blitline JSON kyseiseen sijaintiin. "Kuva"-tunnusta tarvitaan ja palautetaan sinulle, kun tämän erityisesti tunnistavan tallennettu kuva.

Voit etsiä lisätietoja *toimintoja* tuetaan tässä: <http://www.blitline.com/docs/functions>

Löytyvät myös työn vaihtoehtoja ohjeista: <http://www.blitline.com/docs/api>

Kun sinulla että JSON sinun tarvitsee on **Kirjaa** sen`http://api.blitline.com/job`

Näkyviin tulee takaisin JSON, joka näyttää seuraavankaltaiselta:

    {
     "results":
         {"images":
            [{
              "image_identifier":"external_sample_1",
              "s3_url":"https://s3.amazonaws.com/dev.blitline/2011110722/YOUR_APP_ID/CK3f0xBF_2bV6wf7gEZE8w.jpg"
            }],
          "job_id":"4eb8c9f72a50ee2a9900002f"
         }
    }


Tämä kertoo, Blitline pyyntö on vastaanotettu, se on sijoittaa ne käsittely jonossa ja kun se on valmis kuva on osoitteessa: **https://s3.amazonaws.com/dev.blitline/2011110722/YOUR\_Sovelluksen\_ID/CK3f0xBF_2bV6wf7gEZE8w.jpg**

## <a name="how-to-save-an-image-to-your-azure-storage-account"></a>Voit tallentaa kuvan Azure-tallennustilan tilin

Jos olet määrittänyt Azure-tallennustilan tilin, voit määrittää helposti Blitline push käsitellyt kuvat Azure säilö kyselyjä. Lisäämällä "azure_destination" Määritä sijainti ja Blitline, jos haluat siirtää käyttöoikeudet.

Tässä on esimerkki:

    job : '{
      "application_id": "YOUR_APP_ID",
      "src" : "http://www.google.com/logos/2011/houdini11-hp.jpg",
         "functions" : [{
         "name": "blur",
         "save" : {
             "image_identifier" : "YOUR_IMAGE_IDENTIFIER",
             "azure_destination" : {
                 "account_name" : "YOUR_AZURE_CONTAINER_NAME",
                 "shared_access_signature" : "SAS_THAT_GIVES_BLITLINE_PERMISSION_TO_WRITE_THIS_OBJECT_TO_CONTAINER",
               }
           }
         }]
       }'


Täyttämällä CAPITALIZED arvot omalla tämän JSON http://api.blitline.com/job, voit lähettää ja "src" Kuvan pehmennys suodattimella käsitteleminen ja sitten miten voit Azure kohteeseen.

###<a name="please-note"></a>Huomaa:

Suojaussidokset on oltava koko SAS URL-osoitteen, mukaan lukien kohdetiedosto tiedostonimi.

Esimerkki:

    http://blitline.blob.core.windows.net/sample/image.jpg?sr=b&sv=2012-02-12&st=2013-04-12T03%3A18%3A30Z&se=2013-04-12T04%3A18%3A30Z&sp=w&sig=Bte2hkkbwTT2sqlkkKLop2asByrE0sIfeesOwj7jNA5o%3D


Voit myös lukea Blitline's Azuren tallennustilaan docs uusin versio [tästä](http://www.blitline.com/docs/azure_storage).


## <a name="next-steps"></a>Seuraavat vaiheet

Käy blitline.com lisätietoja kaikki sekä muita ominaisuuksia:

* Blitline API päätepisteen tiedostojen <http://www.blitline.com/docs/api>
* Blitline API funktiot <http://www.blitline.com/docs/functions>
* Blitline API esimerkkejä <http://www.blitline.com/docs/examples>
* Kolmas osa Nuget kirjaston <http://nuget.org/packages/Blitline.Net>

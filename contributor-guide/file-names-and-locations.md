<properties title="" pageTitle="Tiedostojen nimet ja Azure artikkeleja sijainnit" description="Tässä artikkelissa kerrotaan tiedoston rakennetta ja nimeämiskäytäntöä, kun luot uuden artikkelin noudattamalla." metaKeywords="" services="" solutions="" documentationCenter="" authors="tysonn" videoId="" scriptId="" manager="required" />

<tags ms.service="contributor-guide" ms.devlang="" ms.topic="article" ms.tgt_pltfrm="" ms.workload="" ms.date="03/14/2016" ms.author="tysonn" />

#<a name="file-names-and-locations-for-azure-technical-articles"></a>Tiedostojen nimet ja Azure artikkeleja sijainnit

Microsoftin tekninen sisällön säilössä Käytämme kaikki artikkeleita yksittäiseen kansioon ( **artikkelit** -kansio). Ei ole kansiohierarkia - kaikki artikkelit live tietuetiedostoon rakenteessa. Jos luot artikkeleita ne kansiot, artikkeleita ei voi julkaista.

Sen sijaan, että Käytä tiedostorakenteen järjestäminen tarpeen, käytä tarkka tiedoston nimeämiskäytäntöä, joka ilmaisee aiheet ja joka osaltaan löydettävyys verkossa.

Näin, mitä on tiedettävä:

+ [Säännöt]
+ [Kuvion]
+ [Vakio-esimerkkejä]
+ [Marketplace-sisältö]
+ [Tiedoston nimen hyväksyntä]

##<a name="rules"></a>Säännöt

- Tiedostojen nimet voivat sisältää vain pieniä kirjaimia, numeroita ja väliviivoja. 
- Ilman välilyöntejä tai välimerkkejä. Argumentiksi yhdysmerkit sanat ja lukujen tiedostonimi.
- Yli 80 merkkejä: Tämä on järjestelmän julkaisun rajoitukset
- Käytä-toiminnon käyttö, jotka koskevat, kuten kehittää, osta, muodosta, vianmääritys. Ei - tekeminen sanoja.
- Pieni - sanoja eivät sisällä ja tekstimuodossa ja, jne.
- Kaikkien tiedostojen on oltava korotuksia ja .md tiedostonimen tunniste.
- Sanat. Vältä hyväksymistä odottavat tai tarpeettomat lyhenteet tiedostonimissä

Lyhenteet ja initialisms tiedostonimissä - ohjeita:

- Lyhennä Azure palveluiden nimet - tiedostonimen ensimmäiset sanat pitäisi olla standardia kirjoitetut Azure-palvelu tai tekniikka nimi. 
-   Älä salli kuin mitä tahansa tiedostonimi-lyhenteet rm tai kädessä
- Yleisesti muiden lyhenteet kelpaavat tiedostonimissä tarpeen mukaan. 

##<a name="pattern"></a>Kuvion

Seuraavassa on yleisiä kuvio:

 **Service-Platform-Language-Content-Product-version.MD**

Käytä kuvion osat, jotka koskevat ja tarkista artikkelien säilöön, saat olemassa olevat nimet verrata luettelo. Nimet, jotka eivät ala keskihajonta-ympäristö tai palvelunimi on todennäköisesti epäilty ja myöhästyneitä kautta.

##<a name="standard-examples"></a>Vakio-esimerkkejä

Seuraavassa on muutamia esimerkkejä kelvolliset nimet, jotka noudattavat mallia. :

- cloud-palvelut-dotnet-jatkuva-delivery.md
- Mobiili-palvelut-ios-get-started.md
- documentdb-Hallitse account.md
- Mobile-Services-DotNet-backend-Get-Started-Settings-Sync.MD
- Active-Directory-Java-AUTHENTICATE-Users-Access-Control-ECLIPSE.MD
- Virtual-Machines-Install-Windows-Server-2008r2.MD
- Välimuistin aspnet-istunnon-tila-palvelu
- Azure-SDK-DotNet-Release-Notes-2-8
- storsimple-Disaster-Recovery-Using-Azure-Site-Recovery

##<a name="marketplace-content"></a>Marketplace-sisältö

Erottaa sisältöä, joka ohjeessa on kumppani osuudet Azure Marketplacesta, Aloita tiedostonimet "marketplace" kanssa. Tämä sisältö tulee liian yleisiä kuin useimmat kumppanin sisällön luodaan kumppanien omien sivustojen.

- Marketplace-mongodb-Virtual-Machines-Install-Windows-Server-2008r2.MD

##<a name="file-name-approval"></a>Tiedoston nimen hyväksyntä

Microsoftin ryhmän salaus puretaan pyynnön tarkistajien tarkistettava tiedostonimet, kun uusi tiedosto lähetetään säilöön ensimmäistä kertaa työ on. Salaus puretaan pyynnön tarkistajat Tarkista tiedostonimi ja antaa palautetta kautta salaus puretaan pyynnön kommentti stream tarvittaessa muutoksia. Tiedostonimi on ratkaistava ennen kuin salaus puretaan pyyntö hyväksytään. Osallistujat voivat helposti push päivitys odotetaan erotettu pyynnön.

##<a name="folder-names-in-the-repo"></a>Kansion nimet repo

Luoda kansioita vain palveluille ja tiedostonimi tulee vastata palvelun dynaamisen tietokentän. Käytä vain kirjaimia ja tavuviivojen, ja pienillä kirjaimilla. Hanki hyväksynnän säilöön järjestelmänvalvojan ennen kuin voit luoda uuden kansion, joka ei ole julkaistu palvelun.

##<a name="changing-case-in-file-names"></a>Muuttaa kirjainkokoa tiedostonimissä

Windows-käyttöjärjestelmien ovat kirjainkokoa, joten jos haluat muuttaa korjaa kannen tiedostonimi, se kannattaa olennaisia muutosten tekeminen ellet ole voi tehdä muutoksen Mac tai Linux Esimerkki:

  biztalk-administration-and-Development-Task-List-in-BizTalk-Services--> biztalk-services-administration-and-development-task-list

Seuraavalla komennolla voit nimetä tiedoston:
```
  git mv <articles/service-folder/current-file-name.md> <articles/service-folder/new-file-name>
```

###<a name="contributors-guide-links"></a>Osallistujat-opas linkit

- [Yleistä artikkelissa](./../README.md)
- [Indeksi ohjeet artikkeleista](./contributor-guide-index.md)


<!--Anchors-->
[Säännöt]: #rules
[Kuvion]: #pattern
[Vakio-esimerkkejä]: #standard-examples
[Marketplace-sisältö]: #marketplace-content
[Tiedoston nimen hyväksyntä]: #file-name-approval
